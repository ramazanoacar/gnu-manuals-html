# Extract the compressed archive
$ tar xf archive.tar.gz
```

The format recognition algorithm is based on *signatures*, a
special byte sequences in the beginning of file, that are specific for
certain compression formats. If this approach fails, `tar`
falls back to using archive name suffix to determine its format
(see [auto-compress](#auto_002dcompress), for a list of recognized suffixes).

Some compression programs are able to handle different compression
formats. GNU `tar` uses this, if the principal decompressor for the
given format is not available. For example, if `compress` is
not installed, `tar` will try to use `gzip`. As of
version 1.35.90 the following alternatives are
tried[23](#FOOT23):

| Format | Main decompressor | Alternatives |
| --- | --- | --- |
| compress | compress | gzip |
| lzma | lzma | xz |
| bzip2 | bzip2 | lbzip2 |

The only case when you have to specify a decompression option while
reading the archive is when reading from a pipe or from a tape drive
that does not support random access. However, in this case GNU `tar`
will indicate which option you should use. For example:

```
$ cat archive.tar.gz | tar tf -
tar: Archive is compressed.  Use -z option
tar: Error is not recoverable: exiting now
```

If you see such diagnostics, just add the suggested option to the
invocation of GNU `tar`:

```
$ cat archive.tar.gz | tar tzf -
```

Notice also, that there are several restrictions on operations on
compressed archives. First of all, compressed archives cannot be
modified, i.e., you cannot update (`--update`, alias `-u`)
them or delete (`--delete`) members from them or
add (`--append`, alias `-r`) members to them. Likewise, you
cannot append another `tar` archive to a compressed archive using
`--concatenate` (`-A`). Secondly, multi-volume
archives cannot be compressed.

The following options allow to select a particular compressor program:

`-z`

`--gzip`

`--ungzip`
:   Filter the archive through `gzip`.

`-J`

`--xz`
:   Filter the archive through `xz`.

`-j`

`--bzip2`
:   Filter the archive through `bzip2`.

`--lzip`
:   Filter the archive through `lzip`.

`--lzma`
:   Filter the archive through `lzma`.

`--lzop`
:   Filter the archive through `lzop`.

`--zstd`
:   Filter the archive through `zstd`.

`-Z`

`--compress`

`--uncompress`
:   Filter the archive through `compress`.

When any of these options is given, GNU `tar` searches the compressor
binary in the current path and invokes it. The name of the compressor
program is specified at compilation time using a corresponding
`--with-compname` option to `configure`, e.g.
`--with-bzip2` to select a specific `bzip2` binary.
See [Using lbzip2 with GNU `tar`.](#lbzip2), for a detailed discussion.

The output produced by `tar --help` shows the actual
compressor names along with each of these options.

You can use any of these options on physical devices (tape drives,
etc.) and remote files as well as on normal files; data to or from
such devices or remote files is reblocked by another copy of the
`tar` program to enforce the specified (or default) record
size. The default compression parameters are used.
You can override them by using the `-I` option (see
below), e.g.:

```
$ tar -cf archive.tar.gz -I 'gzip -9 -n' subdir
```

A more traditional way to do this is to use a pipe:

```
$ tar cf - subdir | gzip -9 -n > archive.tar.gz
```

Compressed archives are easily corrupted, because compressed files
have little redundancy. The adaptive nature of the
compression scheme means that the compression tables are implicitly
spread all over the archive. If you lose a few blocks, the dynamic
construction of the compression tables becomes unsynchronized, and there
is little chance that you could recover later in the archive.

Other compression options provide better control over creating
compressed archives. These are:

`--auto-compress`

`-a`
:   Select a compression program to use by the archive file name
    suffix. The following suffixes are recognized:

    | Suffix | Compression program |
    | --- | --- |
    | ‘`.gz`’ | `gzip` |
    | ‘`.tgz`’ | `gzip` |
    | ‘`.taz`’ | `gzip` |
    | ‘`.Z`’ | `compress` |
    | ‘`.taZ`’ | `compress` |
    | ‘`.bz2`’ | `bzip2` |
    | ‘`.tz2`’ | `bzip2` |
    | ‘`.tbz2`’ | `bzip2` |
    | ‘`.tbz`’ | `bzip2` |
    | ‘`.lz`’ | `lzip` |
    | ‘`.lzma`’ | `lzma` |
    | ‘`.tlz`’ | `lzma` |
    | ‘`.lzo`’ | `lzop` |
    | ‘`.xz`’ | `xz` |
    | ‘`.zst`’ | `zstd` |
    | ‘`.tzst`’ | `zstd` |

`--use-compress-program=command`

`-I=command`
:   Use external compression program command. Use this option if you
    want to specify options for the compression program, or if you
    are not happy with the compression program associated with the suffix
    at compile time, or if you have a compression program that GNU `tar`
    does not support. The command argument is a valid command
    invocation, as you would type it at the command line prompt, with any
    additional options as needed. Enclose it in quotes if it contains
    white space (see [Running External Commands](#external)).

    The command should follow two conventions:

    First, when invoked without additional options, it should read data
    from standard input, compress it and output it on standard output.

    Secondly, if invoked with the additional `-d` option, it should
    do exactly the opposite, i.e., read the compressed data from the
    standard input and produce uncompressed data on the standard output.

    The latter requirement means that you must not use the `-d`
    option as a part of the command itself.

The `--use-compress-program` option, in particular, lets you
implement your own filters, not necessarily dealing with
compression/decompression. For example, suppose you wish to implement
PGP encryption on top of compression, using `gpg` (see [gpg —- encryption and signing tool](gpg.html#Top) in GNU Privacy Guard
Manual). The following script does that:

```
#! /bin/sh
case $1 in
-d) gpg --decrypt - | gzip -d -c;;
'') gzip -c | gpg -s;;
*)  echo "Unknown option $1">&2; exit 1;;
esac
```

Suppose you name it `gpgz` and save it somewhere in your
`PATH`. Then the following command will create a compressed
archive signed with your private key:

```
$ tar -cf foo.tar.gpgz -Igpgz .
```

Likewise, the command below will list its contents:

```
$ tar -tf foo.tar.gpgz -Igpgz .
```

* [Using lbzip2 with GNU `tar`.](#lbzip2)

---

Up: [Creating and Reading Compressed Archives](#gzip)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.1.1.1 Using lbzip2 with GNU `tar`.

`Lbzip2` is a multithreaded utility for handling
‘`bzip2`’ compression, written by Laszlo Ersek. It makes use of
multiple processors to speed up its operation and in general works
considerably faster than `bzip2`. For a detailed description
of `lbzip2` see <http://freshmeat.net/projects/lbzip2> and
[lbzip2: parallel bzip2 utility](http://www.linuxinsight.com/lbzip2-parallel-bzip2-utility.html).

Recent versions of `lbzip2` are mostly command line compatible
with `bzip2`, which makes it possible to automatically invoke
it via the `--bzip2` GNU `tar` command line option. To do so,
GNU `tar` must be configured with the `--with-bzip2` command
line option, like this:

```
$ ./configure --with-bzip2=lbzip2 [other-options]
```

Once configured and compiled this way, `tar --help` will show the
following:

```
$ tar --help | grep -- --bzip2
  -j, --bzip2                filter the archive through lbzip2
```

which means that running `tar --bzip2` will invoke `lbzip2`.

---

Previous: [Creating and Reading Compressed Archives](#gzip), Up: [Using Less Space through Compression](#Compression)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.1.2 Archiving Sparse Files

Files in the file system occasionally have *holes*. A *hole*
in a file is a section of the file’s contents which was never written.
The contents of a hole reads as all zeros. On many operating systems,
actual disk storage is not allocated for holes, but they are counted
in the length of the file. If you archive such a file, `tar`
could create an archive longer than the original. To have `tar`
attempt to recognize the holes in a file, use `--sparse`
(`-S`). When you use this option, then, for any file using
less disk space than would be expected from its length, `tar`
searches the file for holes. It then records in the archive for the file where
the holes (consecutive stretches of zeros) are, and only archives the
“real contents” of the file. On extraction (using `--sparse` is not
needed on extraction) any such files have also holes created wherever the holes
were found. Thus, if you use `--sparse`, `tar` archives won’t
take more space than the original.

GNU `tar` uses two methods for detecting holes in sparse files. These
methods are described later in this subsection.

`-S`

`--sparse`
:   This option instructs `tar` to test each file for sparseness
    before attempting to archive it. If the file is found to be sparse it
    is treated specially, thus allowing to decrease the amount of space
    used by its image in the archive.

    This option is meaningful only when creating or updating archives. It
    has no effect on extraction.

Consider using `--sparse` when performing file system backups,
to avoid archiving the expanded forms of files stored sparsely in the
system.

Even if your system has no sparse files currently, some may be
created in the future. If you use `--sparse` while making file
system backups as a matter of course, you can be assured the archive
will never take more space on the media than the files take on disk
(otherwise, archiving a disk filled with sparse files might take
hundreds of tapes). See [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps).

However, be aware that `--sparse` option may present a serious
drawback. Namely, in order to determine the positions of holes in a file
`tar` may have to read it before trying to archive it, so in total
the file may be read **twice**. This may happen when your OS or your FS
does not support *SEEK\_HOLE/SEEK\_DATA* feature in *lseek* (See
`--hole-detection`, below).

When using ‘`POSIX`’ archive format, GNU `tar` is able to store
sparse files using in three distinct ways, called *sparse
formats*. A sparse format is identified by its *number*,
consisting, as usual of two decimal numbers, delimited by a dot. By
default, format ‘`1.0`’ is used. If, for some reason, you wish to
use an earlier format, you can select it using
`--sparse-version` option.

`--sparse-version=version`
:   Select the format to store sparse files in. Valid version values
    are: ‘`0.0`’, ‘`0.1`’ and ‘`1.0`’. See [Storing Sparse Files](#Sparse-Formats),
    for a detailed description of each format.

Using `--sparse-format` option implies `--sparse`.

`--hole-detection=method`
:   Enforce concrete hole detection method. Before the real contents of sparse
    file are stored, `tar` needs to gather knowledge about file
    sparseness. This is because it needs to have the file’s map of holes
    stored into tar header before it starts archiving the file contents.
    Currently, two methods of hole detection are implemented:

    * `--hole-detection=seek`
      Seeking the file for data and holes. It uses enhancement of the `lseek`
      system call (`SEEK_HOLE` and `SEEK_DATA`) which is able to
      reuse file system knowledge about sparse file contents - so the
      detection is usually very fast. To use this feature, your file system
      and operating system must support it. At the time of this writing
      (2015) this feature, in spite of not being accepted by POSIX, is
      fairly widely supported by different operating systems.
    * `--hole-detection=raw`
      Reading byte-by-byte the whole sparse file before the archiving. This
      method detects holes like consecutive stretches of zeroes. Comparing to
      the previous method, it is usually much slower, although more
      portable.

When no `--hole-detection` option is given, `tar` uses
the ‘`seek`’, if supported by the operating system.

Using `--hole-detection` option implies `--sparse`.

---

Next: [Making `tar` Archives More Portable](#Portability), Previous: [Using Less Space through Compression](#Compression), Up: [Controlling the Archive Format](#Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 8.2 Handling File Attributes

When `tar` reads files, it updates their access times. To
avoid this, use the `--atime-preserve[=METHOD]` option, which can either
reset the access time retroactively or avoid changing it in the first
place.

`--atime-preserve`

`--atime-preserve=replace`

`--atime-preserve=system`
:   Preserve the access times of files that are read. This works only for
    files that you own, unless you have superuser privileges.

    `--atime-preserve=replace` works on most systems, but it also
    restores the data modification time and updates the status change
    time. Hence it doesn’t interact with incremental dumps nicely
    (see [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps)), and it can set access or data modification times
    incorrectly if other programs access the file while `tar` is
    running.

    `--atime-preserve=system` avoids changing the access time in
    the first place, if the operating system supports this.
    Unfortunately, this may or may not work on any given operating system
    or file system. If `tar` knows for sure it won’t work, it
    complains right away.

    Currently `--atime-preserve` with no operand defaults to
    `--atime-preserve=replace`, but this is intended to change to
    `--atime-preserve=system` when the latter is better-supported.

`-m`

`--touch`
:   Do not extract data modification time.

    When this option is used, `tar` leaves the data modification times
    of the files it extracts as the times when the files were extracted,
    instead of setting it to the times recorded in the archive.

    This option is meaningless with `--list` (`-t`).

`--same-owner`
:   Create extracted files with the same ownership they have in the
    archive.

    This is the default behavior for the superuser,
    so this option is meaningful only for non-root users, when `tar`
    is executed on those systems able to give files away. This is
    considered as a security flaw by many people, at least because it
    makes quite difficult to correctly account users for the disk space
    they occupy. Also, the `suid` or `sgid` attributes of
    files are easily and silently lost when files are given away.

    When writing an archive, `tar` writes the user ID and user name
    separately. If it can’t find a user name (because the user ID is not
    in `/etc/passwd`), then it does not write one. When restoring,
    it tries to look the name (if one was written) up in
    `/etc/passwd`. If it fails, then it uses the user ID stored in
    the archive instead.

`--no-same-owner`

`-o`
:   Do not attempt to restore ownership when extracting. This is the
    default behavior for ordinary users, so this option has an effect
    only for the superuser.

`--numeric-owner`
:   The `--numeric-owner` option allows (ANSI) archives to be written
    without user/group name information or such information to be ignored
    when extracting. It effectively disables the generation and/or use
    of user/group name information. This option forces extraction using
    the numeric ids from the archive, ignoring the names.

    This is useful in certain circumstances, when restoring a backup from
    an emergency floppy with different passwd/group files for example.
    It is otherwise impossible to extract files with the right ownerships
    if the password file in use during the extraction does not match the
    one belonging to the file system(s) being extracted. This occurs,
    for example, if you are restoring your files after a major crash and
    had booted from an emergency floppy with no password file or put your
    disk into another machine to do the restore.

    The numeric ids are *always* saved into `tar` archives.
    The identifying names are added at create time when provided by the
    system, unless `--format=oldgnu` is used. Numeric ids could be
    used when moving archives between a collection of machines using
    a centralized management for attribution of numeric ids to users
    and groups. This is often made through using the NIS capabilities.

    When making a `tar` file for distribution to other sites, it
    is sometimes cleaner to use a single owner for all files in the
    distribution, and nicer to specify the write permission bits of the
    files as stored in the archive independently of their actual value on
    the file system. The way to prepare a clean distribution is usually
    to have some Makefile rule creating a directory, copying all needed
    files in that directory, then setting ownership and permissions as
    wanted (there are a lot of possible schemes), and only then making a
    `tar` archive out of this directory, before cleaning
    everything out. Of course, we could add a lot of options to
    GNU `tar` for fine tuning permissions and ownership.
    This is not the good way, I think. GNU `tar` is
    already crowded with options and moreover, the approach just explained
    gives you a great deal of control already.

`-p`

`--same-permissions`

`--preserve-permissions`
:   Extract all protection information.

    This option causes `tar` to set the modes (access permissions) of
    extracted files exactly as recorded in the archive. If this option
    is not used, the current `umask` setting limits the permissions
    on extracted files. This option is by default enabled when
    `tar` is executed by a superuser.

    This option is meaningless with `--list` (`-t`).

---

Next: [Making `tar` Archives More Reproducible](#Reproducibility), Previous: [Handling File Attributes](#Attributes), Up: [Controlling the Archive Format](#Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 8.3 Making `tar` Archives More Portable

Creating a `tar` archive on a particular system that is meant to be
useful later on many other machines and with other versions of `tar`
is more challenging than you might think. `tar` archive formats
have been evolving since the first versions of Unix. Many such formats
are around, and are not always compatible with each other. This section
discusses a few problems, and gives some advice about making `tar`
archives more portable.

One golden rule is simplicity. For example, limit your `tar`
archives to contain only regular files and directories, avoiding
other kind of special files. Do not attempt to save sparse files or
contiguous files as such. Let’s discuss a few more problems, in turn.

* [Portable Names](#Portable-Names)
* [Symbolic Links](#dereference)
* [Hard Links](#hard-links)
* [Old V7 Archives](#old)
* [Ustar Archive Format](#ustar)
* [GNU and old GNU `tar` format](#gnu)
* [GNU `tar` and POSIX `tar`](#posix)
* [Checksumming Problems](#Checksumming)
* [Large or Negative Values](#Large-or-Negative-Values)
* [How to Extract GNU-Specific Data Using Other `tar` Implementations](#Other-Tars)

---

Next: [Symbolic Links](#dereference), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.1 Portable Names

Use portable file and member names. A name is portable if it contains
only ASCII letters and digits, ‘`/`’, ‘`.`’, ‘`_`’, and
‘`-`’; it cannot be empty, start with ‘`-`’ or ‘`//`’, or
contain ‘`/-`’. Avoid deep directory nesting. For portability to
old Unix hosts, limit your file name components to 14 characters or
less.

If you intend to have your `tar` archives to be read on
case-insensitive file systems like FAT32,
you should not rely on case distinction for file names.

---

Next: [Hard Links](#hard-links), Previous: [Portable Names](#Portable-Names), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.2 Symbolic Links

Normally, when `tar` archives a symbolic link, it writes a
block to the archive naming the target of the link. In that way, the
`tar` archive is a faithful record of the file system contents.
When `--dereference` (`-h`) is used with
`--create` (`-c`), `tar` archives the files
symbolic links point to, instead of
the links themselves.

When creating a portable archive from a directory that adversaries
cannot modify, consider using `--dereference`
(`-h`): some systems do not support
symbolic links, and moreover, your distribution might be unusable if
it contains unresolved symbolic links.
See [Symbolic Links](#dereference).

When reading from an archive, the `--dereference` (`-h`)
option causes `tar` to follow an already-existing symbolic
link when `tar` writes or reads a file named in the archive.
Ordinarily, `tar` does not follow such a link, though it may
remove the link before writing a new file. See [Options Controlling the Overwriting of Existing Files](#Dealing-with-Old-Files).

The `--dereference` option is unsafe if an untrusted user can
modify directories while `tar` is running, or if extracting
from an untrusted archive into a nonempty directory. See [Security](#Security).

---

Next: [Old V7 Archives](#old), Previous: [Symbolic Links](#dereference), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.3 Hard Links

Normally, when `tar` archives a hard link, it writes a
block to the archive naming the target of the link (a ‘`1`’ type
block). In that way, the actual file contents is stored in file only
once. For example, consider the following two files:

```
$ ls -l
-rw-r--r-- 2 gray staff 4 2007-10-30 15:11 one
-rw-r--r-- 2 gray staff 4 2007-10-30 15:11 jeden
```

Here, `jeden` is a link to `one`. When archiving this
directory with a verbose level 2, you will get an output similar to
the following:

```
$ tar cvvf ../archive.tar .
drwxr-xr-x gray/staff        0 2007-10-30 15:13 ./
-rw-r--r-- gray/staff        4 2007-10-30 15:11 ./jeden
hrw-r--r-- gray/staff        0 2007-10-30 15:11 ./one link to ./jeden
```

The last line shows that, instead of storing two copies of the file,
`tar` stored it only once, under the name `jeden`, and
stored file `one` as a hard link to this file.

It may be important to know that all hard links to the given file are
stored in the archive. For example, this may be necessary for exact
reproduction of the file system. The following option does that:

`--check-links`

`-l`
:   Check the number of links dumped for each processed file. If this
    number does not match the total number of hard links for the file, print
    a warning message.

For example, trying to archive only file `jeden` with this option
produces the following diagnostics:

```
$ tar -c -f ../archive.tar -l jeden
tar: Missing links to 'jeden'.
```

Although creating special records for hard links helps keep a faithful
record of the file system contents and makes archives more compact, it
may present some difficulties when extracting individual members from
the archive. For example, trying to extract file `one` from the
archive created in previous examples produces, in the absence of file
`jeden`:

```
$ tar xf archive.tar ./one
tar: ./one: Cannot hard link to './jeden': No such file or directory
tar: Error exit delayed from previous errors
```

The reason for this behavior is that `tar` cannot seek back in
the archive to the previous member (in this case, `one`), to
extract it[24](#FOOT24).
If you wish to avoid such problems at the cost of a bigger archive,
use the following option:

`--hard-dereference`
:   Dereference hard links and store the files they refer to.

For example, trying this option on our two sample files, we get two
copies in the archive, each of which can then be extracted
independently of the other:

```
$ tar -c -vv -f ../archive.tar --hard-dereference .
drwxr-xr-x gray/staff        0 2007-10-30 15:13 ./
-rw-r--r-- gray/staff        4 2007-10-30 15:11 ./jeden
-rw-r--r-- gray/staff        4 2007-10-30 15:11 ./one
```

---

Next: [Ustar Archive Format](#ustar), Previous: [Hard Links](#hard-links), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.4 Old V7 Archives

Certain old versions of `tar` cannot handle additional
information recorded by newer `tar` programs. To create an
archive in V7 format (not ANSI), which can be read by these old
versions, specify the `--format=v7` option in
conjunction with the `--create` (`-c`) (`tar` also
accepts `--portability` or `--old-archive` for this
option). When you specify it,
`tar` leaves out information about directories, pipes, fifos,
contiguous files, and device files, and specifies file ownership by
group and user IDs instead of group and user names.

When updating an archive, do not use `--format=v7`
unless the archive was created using this option.

In most cases, a *new* format archive can be read by an *old*
`tar` program without serious trouble, so this option should
seldom be needed. On the other hand, most modern `tar`s are
able to read old format archives, so it might be safer for you to
always use `--format=v7` for your distributions. Notice,
however, that ‘`ustar`’ format is a better alternative, as it is
free from many of ‘`v7`’’s drawbacks.

---

Next: [GNU and old GNU `tar` format](#gnu), Previous: [Old V7 Archives](#old), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.5 Ustar Archive Format

The archive format defined by the POSIX.1-1988 specification is
called `ustar`. Although it is more flexible than the V7 format, it
still has many restrictions (see [ustar](#Formats), for the detailed
description of `ustar` format). Along with V7 format,
`ustar` format is a good choice for archives intended to be read
with other implementations of `tar`.

To create an archive in `ustar` format, use the `--format=ustar`
option in conjunction with `--create` (`-c`).

---

Next: [GNU `tar` and POSIX `tar`](#posix), Previous: [Ustar Archive Format](#ustar), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.6 GNU and old GNU `tar` format

GNU `tar` was based on an early draft of the
POSIX 1003.1 `ustar` standard. GNU extensions to
`tar`, such as the support for file names longer than 100
characters, use portions of the `tar` header record which were
specified in that POSIX draft as unused. Subsequent changes in
POSIX have allocated the same parts of the header record for
other purposes. As a result, GNU `tar` format is
incompatible with the current POSIX specification, and with
`tar` programs that follow it.

In the majority of cases, `tar` will be configured to create
this format by default. This will change in future releases, since
we plan to make ‘`POSIX`’ format the default.

To force creation a GNU `tar` archive, use option
`--format=gnu`.

---

Next: [Checksumming Problems](#Checksumming), Previous: [GNU and old GNU `tar` format](#gnu), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.7 GNU `tar` and POSIX `tar`

Starting from version 1.14 GNU `tar` features full support for
POSIX.1-2001 archives.

A POSIX conformant archive will be created if `tar`
was given `--format=posix` (`--format=pax`) option. No
special option is required to read and extract from a POSIX
archive.

* [Controlling Extended Header Keywords](#PAX-keywords)

---

Up: [GNU `tar` and POSIX `tar`](#posix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.7.1 Controlling Extended Header Keywords

`--pax-option=keyword-list`
:   Handle keywords in PAX extended headers. This option is
    equivalent to `-o` option of the `pax` utility.

Keyword-list is a comma-separated
list of keyword options, each keyword option taking one of
the following forms:

`delete=pattern`
:   When used with one of archive-creation commands,
    this option instructs `tar` to omit from extended header records
    that it produces any keywords matching the string pattern.
    If the pattern contains shell metacharacters like ‘`*`’, it should
    be quoted to prevent the shell from expanding the pattern before
    `tar` sees it.

    When used in extract or list mode, this option instructs tar
    to ignore any keywords matching the given pattern in the extended
    header records. In both cases, matching is performed using the pattern
    matching notation described in POSIX 1003.2, 3.13
    (see [Wildcards Patterns and Matching](#wildcards)). For example:

    ```
    --pax-option 'delete=security.*'
    ```

    would suppress security-related information.

`exthdr.name=string`
:   This keyword allows user control over the name that is written into the
    ustar header blocks for the extended headers. The name is obtained
    from string after making the following substitutions:

    | Meta-character | Replaced By |
    | --- | --- |
    | %d | The directory name of the file, equivalent to the result of the `dirname` utility on the translated file name. |
    | %f | The name of the file with the directory information stripped, equivalent to the result of the `basename` utility on the translated file name. |
    | %p | The process ID of the `tar` process. |
    | %% | A ‘`%`’ character. |

    Any other ‘`%`’ characters in string produce undefined
    results.

    If no option ‘`exthdr.name=string`’ is specified, `tar`
    will use the following default value:

    ```
    %d/PaxHeaders/%f
    ```

    This default helps make the archive more reproducible.
    See [Making `tar` Archives More Reproducible](#Reproducibility). POSIX recommends using
    ‘`%d/PaxHeaders.%p/%f`’ instead, which means the two archives
    created with the same set of options and containing the same set
    of files will be byte-to-byte different. This default will be used
    if the environment variable `POSIXLY_CORRECT` is set.

`exthdr.mtime=value`
:   This keyword defines the value of the ‘`mtime`’ field that
    is written into the ustar header blocks for the extended headers.
    By default, the ‘`mtime`’ field is set to the modification time
    of the archive member described by that extended header (or to the
    value of the `--mtime` option, if supplied).

`globexthdr.name=string`
:   This keyword allows user control over the name that is written into
    the ustar header blocks for global extended header records. The name
    is obtained from the contents of string, after making
    the following substitutions:

    | Meta-character | Replaced By |
    | --- | --- |
    | %n | An integer that represents the sequence number of the global extended header record in the archive, starting at 1. |
    | %p | The process ID of the `tar` process. |
    | %% | A ‘`%`’ character. |

    Any other ‘`%`’ characters in string produce undefined results.

    If no option ‘`globexthdr.name=string`’ is specified, `tar`
    will use the following default value:

    ```
    $TMPDIR/GlobalHead.%n
    ```

    If the environment variable `POSIXLY_CORRECT` is set, the
    following value is used instead:

    ```
    $TMPDIR/GlobalHead.%p.%n
    ```

    In both cases, ‘`$TMPDIR`’ stands for the value of the TMPDIR
    environment variable. If TMPDIR is not set, `tar`
    uses ‘`/tmp`’.

`globexthdr.mtime=value`
:   This keyword defines the value of the ‘`mtime`’ field that
    is written into the ustar header blocks for the global extended headers.
    By default, the ‘`mtime`’ field is set to the time when
    `tar` was invoked.

`keyword=value`
:   When used with one of archive-creation commands, these keyword/value pairs
    will be included at the beginning of the archive in a global extended
    header record. When used with one of archive-reading commands,
    `tar` will behave as if it has encountered these keyword/value
    pairs at the beginning of the archive in a global extended header
    record.

`keyword:=value`
:   When used with one of archive-creation commands, these keyword/value pairs
    will be included as records at the beginning of an extended header for
    each file. This is effectively equivalent to keyword=value
    form except that it creates no global extended header records.

    When used with one of archive-reading commands, `tar` will
    behave as if these keyword/value pairs were included as records at the
    end of each extended header; thus, they will override any global or
    file-specific extended header record keywords of the same names.
    For example, in the command:

    ```
    tar --format=posix --create \
        --file archive --pax-option gname:=user .
    ```

    the group name will be forced to a new value for all files
    stored in the archive.

In any of the forms described above, the value may be
a string enclosed in curly braces. In that case, the string
between the braces is understood either as a textual time
representation, as described in [Date input formats](#Date-input-formats), or a name of
the existing file, starting with ‘`/`’ or ‘`.`’. In the latter
case, the modification time of that file is used.

For example, to set all modification times to the current date, you
use the following option:

```
--pax-option 'mtime:={now}'
```

As another example, the following option helps make the archive
more reproducible. See [Making `tar` Archives More Reproducible](#Reproducibility).

```
--pax-option delete=atime
```

If you extract files from such an archive and recreate the archive
from them, you will also need to eliminate changes due to ctime:

```
--pax-option 'delete=atime,delete=ctime'
```

Normally `tar` saves an mtime value with subsecond resolution
in an extended header for any file with a timestamp that is not on a
one-second boundary. This is in addition to the traditional mtime
timestamp in the header block. Although you can suppress subsecond
timestamp resolution with `--pax-option delete=mtime`,
this hack will not work for timestamps before 1970 or after 2242-03-16
12:56:31 UTC.

If the environment variable `POSIXLY_CORRECT` is set, two POSIX
archives created using the same options on the same set of files might
not be byte-to-byte equivalent even with the above options. This is
because the POSIX default for extended header names includes
the `tar` process ID, which typically differs at each
run. To produce byte-to-byte equivalent archives in this case, either
unset `POSIXLY_CORRECT`, or use the following option, which can be
combined with the above options:

```
--pax-option exthdr.name=%d/PaxHeaders/%f
```

---

Next: [Large or Negative Values](#Large-or-Negative-Values), Previous: [GNU `tar` and POSIX `tar`](#posix), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.8 Checksumming Problems

SunOS and HP-UX `tar` fail to accept archives created using
GNU `tar` and containing non-ASCII file names, that
is, file names having characters with the eighth bit set, because they
use signed checksums, while GNU `tar` uses unsigned
checksums while creating archives, as per POSIX standards. On
reading, GNU `tar` computes both checksums and accepts either of them.
It is somewhat worrying that a lot of people may go
around doing backup of their files using faulty (or at least
non-standard) software, not learning about it until it’s time to
restore their missing files with an incompatible file extractor, or
vice versa.

GNU `tar` computes checksums both ways, and accepts either of them
on read, so GNU tar can read Sun tapes even with their
wrong checksums. GNU `tar` produces the standard
checksum, however, raising incompatibilities with Sun. That is to
say, GNU `tar` has not been modified to
*produce* incorrect archives to be read by buggy `tar`’s.
I’ve been told that more recent Sun `tar` now read standard
archives, so maybe Sun did a similar patch, after all?

The story seems to be that when Sun first imported `tar`
sources on their system, they recompiled it without realizing that
the checksums were computed differently, because of a change in
the default signing of `char`’s in their compiler. So they
started computing checksums wrongly. When they later realized their
mistake, they merely decided to stay compatible with it, and with
themselves afterwards. Presumably, but I do not really know, HP-UX
has chosen their `tar` archives to be compatible with Sun’s.
The current standards do not favor Sun `tar` format. In any
case, it now falls on the shoulders of SunOS and HP-UX users to get
a `tar` able to read the good archives they receive.

---

Next: [How to Extract GNU-Specific Data Using Other `tar` Implementations](#Other-Tars), Previous: [Checksumming Problems](#Checksumming), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.9 Large or Negative Values

> *(This message will disappear, once this node revised.)*

The above sections suggest to use ‘`oldest possible`’ archive
format if in doubt. However, sometimes it is not possible. If you
attempt to archive a file whose metadata cannot be represented using
required format, GNU `tar` will print error message and ignore such a
file. You will than have to switch to a format that is able to
handle such values. The format summary table (see [Controlling the Archive Format](#Formats)) will
help you to do so.

In particular, when trying to archive files 8 GiB or larger, or with
timestamps not in the range 1970-01-01 00:00:00 through 2242-03-16
12:56:31 UTC, you will have to chose between GNU and
POSIX archive formats. When considering which format to
choose, bear in mind that the GNU format uses
two’s-complement base-256 notation to store values that do not fit
into standard ustar range. Such archives can generally be
read only by a GNU `tar` implementation. Moreover, they sometimes
cannot be correctly restored on another hosts even by GNU `tar`. For
example, using two’s complement representation for negative time
stamps that assumes a signed 32-bit `time_t` generates archives
that are not portable to hosts with differing `time_t`
representations.

On the other hand, POSIX archives, generally speaking, can
be extracted by any tar implementation that understands older
ustar format. The exceptions are files 8 GiB or larger,
or files dated before 1970-01-01 00:00:00 or after 2242-03-16
12:56:31 UTC

---

Previous: [Large or Negative Values](#Large-or-Negative-Values), Up: [Making `tar` Archives More Portable](#Portability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.10 How to Extract GNU-Specific Data Using Other `tar` Implementations

In previous sections you became acquainted with various quirks
necessary to make your archives portable. Sometimes you may need to
extract archives containing GNU-specific members using some
third-party `tar` implementation or an older version of
GNU `tar`. Of course your best bet is to have GNU `tar` installed,
but if it is for some reason impossible, this section will explain
how to cope without it.

When we speak about *GNU-specific* members we mean two classes of
them: members split between the volumes of a multi-volume archive and
sparse members. You will be able to always recover such members if
the archive is in PAX format. In addition split members can be
recovered from archives in old GNU format. The following subsections
describe the required procedures in detail.

* [Extracting Members Split Between Volumes](#Split-Recovery)
* [Extracting Sparse Members](#Sparse-Recovery)

---

Next: [Extracting Sparse Members](#Sparse-Recovery), Up: [How to Extract GNU-Specific Data Using Other `tar` Implementations](#Other-Tars)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.10.1 Extracting Members Split Between Volumes

If a member is split between several volumes of an old GNU format archive
most third party `tar` implementation will fail to extract
it. To extract it, use `tarcat` program (see [Concatenate Volumes into a Single Archive](#Tarcat)).
This program is available from
[GNU `tar`
home page](http://www.gnu.org/software/tar/utils/tarcat.html). It concatenates several archive volumes into a single
valid archive. For example, if you have three volumes named from
`vol-1.tar` to `vol-3.tar`, you can do the following to
extract them using a third-party `tar`:

```
$ tarcat vol-1.tar vol-2.tar vol-3.tar | tar xf -
```

You could use this approach for most (although not all) PAX
format archives as well. However, extracting split members from a PAX
archive is a much easier task, because PAX volumes are constructed in
such a way that each part of a split member is extracted to a
different file by `tar` implementations that are not aware of
GNU extensions. More specifically, the very first part retains its
original name, and all subsequent parts are named using the pattern:

```
%d/GNUFileParts/%f.%n
```

where symbols preceded by ‘`%`’ are *macro characters* that
have the following meaning:

| Meta-character | Replaced By |
| --- | --- |
| %d | The directory name of the file, equivalent to the result of the `dirname` utility on its full name. |
| %f | The file name of the file, equivalent to the result of the `basename` utility on its full name. |
| %p | The process ID of the `tar` process that created the archive. |
| %n | Ordinal number of this particular part. |

For example, if the file `var/longfile` was split during archive
creation between three volumes, then the member names will be:

```
var/longfile
var/GNUFileParts/longfile.1
var/GNUFileParts/longfile.2
```

When you extract your archive using a third-party `tar`, these
files will be created on your disk, and the only thing you will need
to do to restore your file in its original form is concatenate them in
the proper order, for example:

```
$ cd var
$ cat GNUFileParts/longfile.1 \
  GNUFileParts/longfile.2 >> longfile
$ rm -f GNUFileParts
```

Notice, that if the `tar` implementation you use supports PAX
format archives, it will probably emit warnings about unknown keywords
during extraction. They will look like this:

```
Tar file too small
Unknown extended header keyword 'GNU.volume.filename' ignored.
Unknown extended header keyword 'GNU.volume.size' ignored.
Unknown extended header keyword 'GNU.volume.offset' ignored.
```

You can safely ignore these warnings.

If your `tar` implementation is not PAX-aware, you will get
more warnings and more files generated on your disk, e.g.:

```
$ tar xf vol-1.tar
var/PaxHeaders/longfile: Unknown file type 'x', extracted as
normal file
Unexpected EOF in archive
$ tar xf vol-2.tar
tmp/GlobalHead.1: Unknown file type 'g', extracted as normal file
GNUFileParts/PaxHeaders/sparsefile.1: Unknown file type
'x', extracted as normal file
```

Ignore these warnings. The `PaxHeaders.*` directories created
will contain files with *extended header keywords* describing the
extracted files. You can delete them, unless they describe sparse
members. Read further to learn more about them.

---

Previous: [Extracting Members Split Between Volumes](#Split-Recovery), Up: [How to Extract GNU-Specific Data Using Other `tar` Implementations](#Other-Tars)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.3.10.2 Extracting Sparse Members

Any `tar` implementation will be able to extract sparse members from a
PAX archive. However, the extracted files will be *condensed*,
i.e., any zero blocks will be removed from them. When we restore such
a condensed file to its original form, by adding zero blocks (or
*holes*) back to their original locations, we call this process
*expanding* a compressed sparse file.

To expand a file, you will need a simple auxiliary program called
`xsparse`. It is available in source form from
[GNU `tar`
home page](http://www.gnu.org/software/tar/utils/xsparse.html).

Let’s begin with archive members in *sparse format
version 1.0*[25](#FOOT25), which are the easiest to expand.
The condensed file will contain both file map and file data, so no
additional data will be needed to restore it. If the original file
name was `dir/name`, then the condensed file will be
named `dir/GNUSparseFile.n/name`, where
n is a decimal number[26](#FOOT26).

To expand a version 1.0 file, run `xsparse` as follows:

```
$ xsparse cond-file
```

where `cond-file` is the name of the condensed file. The utility
will deduce the name for the resulting expanded file using the
following algorithm:

1. If `cond-file` has the form `dir/name`,
   the output file name will be `dir/../name`.
2. Otherwise, the output file name will be `../cond-file`.

In the unlikely case when this algorithm does not suit your needs,
you can explicitly specify output file name as a second argument to
the command:

```
$ xsparse cond-file out-file
```

It is often a good idea to run `xsparse` in *dry run* mode
first. In this mode, the command does not actually expand the file,
but verbosely lists all actions it would be taking to do so. The dry
run mode is enabled by `-n` command line argument:

```
$ xsparse -n /home/gray/GNUSparseFile.6058/sparsefile
Reading v.1.0 sparse map
Expanding file '/home/gray/GNUSparseFile.6058/sparsefile' to
'/home/gray/sparsefile'
Finished dry run
```

To actually expand the file, you would run:

```
$ xsparse /home/gray/GNUSparseFile.6058/sparsefile
```

The program behaves the same way all UNIX utilities do: it will keep
quiet unless it has something important to tell you (e.g. an error
condition or something). If you wish it to produce verbose output,
similar to that from the dry run mode, use `-v` option:

```
$ xsparse -v /home/gray/GNUSparseFile.6058/sparsefile
Reading v.1.0 sparse map
Expanding file '/home/gray/GNUSparseFile.6058/sparsefile' to
'/home/gray/sparsefile'
Done
```

Additionally, if your `tar` implementation has extracted the
*extended headers* for this file, you can instruct `xstar`
to use them in order to verify the integrity of the expanded file.
The option `-x` sets the name of the extended header file to
use. Continuing our example:

```
$ xsparse -v -x /home/gray/PaxHeaders/sparsefile \
  /home/gray/GNUSparseFile/sparsefile
Reading extended header file
Found variable GNU.sparse.major = 1
Found variable GNU.sparse.minor = 0
Found variable GNU.sparse.name = sparsefile
Found variable GNU.sparse.realsize = 217481216
Reading v.1.0 sparse map
Expanding file '/home/gray/GNUSparseFile.6058/sparsefile' to
'/home/gray/sparsefile'
Done
```

An *extended header* is a special `tar` archive header
that precedes an archive member and contains a set of
*variables*, describing the member properties that cannot be
stored in the standard `ustar` header. While optional for
expanding sparse version 1.0 members, the use of extended headers is
mandatory when expanding sparse members in older sparse formats: v.0.0
and v.0.1 (The sparse formats are described in detail in [Storing Sparse Files](#Sparse-Formats).) So, for these formats, the question is: how to obtain
extended headers from the archive?

If you use a `tar` implementation that does not support PAX
format, extended headers for each member will be extracted as a
separate file. If we represent the member name as
`dir/name`, then the extended header file will be
named `dir/PaxHeaders/name`.

Things become more difficult if your `tar` implementation
does support PAX headers, because in this case you will have to
manually extract the headers. We recommend the following algorithm:

1. Consult the documentation of your `tar` implementation for an
   option that prints *block numbers* along with the archive
   listing (analogous to GNU `tar`’s `-R` option). For example,
   `star` has `-block-number`.
2. Obtain verbose listing using the ‘`block number`’ option, and
   find block numbers of the sparse member in question and the member
   immediately following it. For example, running `star` on our
   archive we obtain:

   ```
   $ star -t -v -block-number -f arc.tar
   …
   star: Unknown extended header keyword 'GNU.sparse.size' ignored.
   star: Unknown extended header keyword 'GNU.sparse.numblocks' ignored.
   star: Unknown extended header keyword 'GNU.sparse.name' ignored.
   star: Unknown extended header keyword 'GNU.sparse.map' ignored.
   block        56:  425984 -rw-r--r--  gray/users Jun 25 14:46 2006 GNUSparseFile.28124/sparsefile
   block       897:   65391 -rw-r--r--  gray/users Jun 24 20:06 2006 README
   …
   ```

   (as usual, ignore the warnings about unknown keywords.)
3. Let size be the size of the sparse member, Bs be its block number
   and Bn be the block number of the next member.
   Compute:

   ```
   N = Bs - Bn - size/512 - 2
   ```

   This number gives the size of the extended header part in tar *blocks*.
   In our example, this formula gives: `897 - 56 - 425984 / 512 - 2
   = 7`.
4. Use `dd` to extract the headers:

   ```
   dd if=archive of=hname bs=512 skip=Bs count=N
   ```

   where archive is the archive name, hname is a name of the
   file to store the extended header in, Bs and N are
   computed in previous steps.

   In our example, this command will be

   ```
   $ dd if=arc.tar of=xhdr bs=512 skip=56 count=7
   ```

Finally, you can expand the condensed file, using the obtained header:

```
$ xsparse -v -x xhdr GNUSparseFile.6058/sparsefile
Reading extended header file
Found variable GNU.sparse.size = 217481216
Found variable GNU.sparse.numblocks = 208
Found variable GNU.sparse.name = sparsefile
Found variable GNU.sparse.map = 0,2048,1050624,2048,…
Expanding file 'GNUSparseFile.28124/sparsefile' to 'sparsefile'
Done
```

---

Next: [Comparison of `tar` and `cpio`](#cpio), Previous: [Making `tar` Archives More Portable](#Portability), Up: [Controlling the Archive Format](#Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 8.4 Making `tar` Archives More Reproducible

Sometimes it is important for an archive to be *reproducible*,
so that one can easily verify it to have been derived solely from
its input. We call an archive reproducible, if an archive
created from the same set of input files with the same command line
options is byte-to-byte equivalent to the original one.

However, two archives created by GNU `tar` from two sets of input
files normally might differ even if the input files have the same
contents and GNU `tar` was invoked the same way on both sets of input.
This can happen if the inputs have different modification dates or
other metadata, or if the input directories’ entries are in different orders.

To avoid this problem when creating an archive, and thus make the
archive reproducible, you can run GNU `tar` in the C locale with
some or all of the following options:

`--sort=name`
:   Omit irrelevant information about directory entry order.

`--format=posix`
:   Avoid problems with large files or files with unusual timestamps.
    This also enables `--pax-option` options mentioned below.

`--pax-option='exthdr.name=%d/PaxHeaders/%f'`
:   Omit the process ID of `tar`.
    This option is needed only if `POSIXLY_CORRECT` is set in the environment.

`--pax-option='delete=atime,delete=ctime'`
:   Omit irrelevant information about file access or status change time.

`--clamp-mtime --mtime="$SOURCE_EPOCH"`
:   Omit irrelevant information about file timestamps after
    ‘`$SOURCE_EPOCH`’, which should be a time no less than any
    timestamp of any source file.

`--numeric-owner`
:   Omit irrelevant information about user and group names.

`--owner=0`

`--group=0`
:   Omit irrelevant information about file ownership and group.

`--mode='go+u,go-w'`
:   Omit irrelevant information about file permissions.

When creating a reproducible archive from version-controlled source files,
it can be useful to set each file’s modification time
to be that of its last commit, so that the timestamps
are reproducible from the version-control repository.
If these timestamps are all on integer second boundaries, and if you use
`--format=posix --pax-option='delete=atime,delete=ctime'
--clamp-mtime --mtime="$SOURCE_EPOCH"`
where `$SOURCE_EPOCH` is the the time of the most recent commit,
and if all non-source files have timestamps greater than `$SOURCE_EPOCH`,
then GNU `tar` should generate an archive in ustar format,
since no POSIX features will be needed and the archive will be in the
ustar subset of posix format.

Also, if compressing, use a reproducible compression format; e.g.,
with `gzip` you should use the `--no-name` (`-n`) option.

Here is an example set of shell commands to produce a reproducible
tarball with `git` and `gzip`, which you can tailor to
your project’s needs.

```
function get_commit_time() {
  TZ=UTC0 git log -1 \
    --format=tformat:%cd \
    --date=format:%Y-%m-%dT%H:%M:%SZ \
    "$@"
}
#
# Set each source file timestamp to that of its latest commit.
git ls-files | while read -r file; do
  commit_time=$(get_commit_time "$file") &&
  touch -md $commit_time "$file"
done
#
# Set timestamp of each directory under $FILES
# to the latest timestamp of any descendant.
find $FILES -depth -type d -exec sh -c \
  'touch -r "$0/$(ls -At "$0" | head -n 1)" "$0"' \
  {} ';'
#
# Create $ARCHIVE.tgz from $FILES, pretending that
# the modification time for each newer file
# is that of the most recent commit of any source file.
SOURCE_EPOCH=$(get_commit_time)
TARFLAGS="
  --sort=name --format=posix
  --pax-option=exthdr.name=%d/PaxHeaders/%f
  --pax-option=delete=atime,delete=ctime
  --clamp-mtime --mtime=$SOURCE_EPOCH
  --numeric-owner --owner=0 --group=0
  --mode=go+u,go-w
"
GZIPFLAGS="--no-name --best"
LC_ALL=C tar $TARFLAGS -cf - $FILES |
  gzip $GZIPFLAGS > $ARCHIVE.tgz
```

---

Previous: [Making `tar` Archives More Reproducible](#Reproducibility), Up: [Controlling the Archive Format](#Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 8.5 Comparison of `tar` and `cpio`

> *(This message will disappear, once this node revised.)*

The `cpio` archive formats, like `tar`, do have maximum
file name lengths. The binary and old ASCII formats have a maximum file
length of 256, and the new ASCII and CRC ASCII formats have a max
file length of 1024. GNU `cpio` can read and write archives
with arbitrary file name lengths, but other `cpio` implementations
may crash unexplainedly trying to read them.

`tar` handles symbolic links in the form in which it comes in BSD;
`cpio` doesn’t handle symbolic links in the form in which it comes
in System V prior to SVR4, and some vendors may have added symlinks
to their system without enhancing `cpio` to know about them.
Others may have enhanced it in a way other than the way I did it
at Sun, and which was adopted by AT&T (and which is, I think, also
present in the `cpio` that Berkeley picked up from AT&T and put
into a later BSD release—I think I gave them my changes).

(SVR4 does some funny stuff with `tar`; basically, its `cpio`
can handle `tar` format input, and write it on output, and it
probably handles symbolic links. They may not have bothered doing
anything to enhance `tar` as a result.)

`cpio` handles special files; traditional `tar` doesn’t.

`tar` comes with V7, System III, System V, and BSD source;
`cpio` comes only with System III, System V, and later BSD
(4.3-tahoe and later).

`tar`’s way of handling multiple hard links to a file can handle
file systems that support 32-bit i-numbers (e.g., the BSD file system);
`cpio`s way requires you to play some games (in its “binary”
format, i-numbers are only 16 bits, and in its “portable ASCII” format,
they’re 18 bits—it would have to play games with the "file system ID"
field of the header to make sure that the file system ID/i-number pairs
of different files were always different), and I don’t know which
`cpio`s, if any, play those games. Those that don’t might get
confused and think two files are the same file when they’re not, and
make hard links between them.

`tar`s way of handling multiple hard links to a file places only
one copy of the link on the tape, but the name attached to that copy
is the *only* one you can use to retrieve the file; `cpio`s
way puts one copy for every link, but you can retrieve it using any
of the names.

> What type of check sum (if any) is used, and how is this calculated.

See the attached manual pages for `tar` and `cpio` format.
`tar` uses a checksum which is the sum of all the bytes in the
`tar` header for a file; `cpio` uses no checksum.

> If anyone knows why `cpio` was made when `tar` was present
> at the unix scene,

It wasn’t. `cpio` first showed up in PWB/UNIX 1.0; no
generally-available version of UNIX had `tar` at the time. I don’t
know whether any version that was generally available *within AT&T*
had `tar`, or, if so, whether the people within AT&T who did
`cpio` knew about it.

On restore, if there is a corruption on a tape `tar` will stop at
that point, while `cpio` will skip over it and try to restore the
rest of the files.

The main difference is just in the command syntax and header format.

`tar` is a little more tape-oriented in that everything is blocked
to start on a record boundary.

> Is there any differences between the ability to recover crashed
> archives between the two of them. (Is there any chance of recovering
> crashed archives at all.)

Theoretically it should be easier under `tar` since the blocking
lets you find a header with some variation of ‘`dd skip=nn`’.
However, modern `cpio`’s and variations have an option to just
search for the next file header after an error with a reasonable chance
of resyncing. However, lots of tape driver software won’t allow you to
continue past a media error which should be the only reason for getting
out of sync unless a file changed sizes while you were writing the
archive.

> If anyone knows why `cpio` was made when `tar` was present
> at the unix scene, please tell me about this too.

Probably because it is more media efficient (by not blocking everything
and using only the space needed for the headers where `tar`
always uses 512 bytes per file header) and it knows how to archive
special files.

You might want to look at the freely available alternatives. The
major ones are `afio`, GNU `tar`, and
`pax`, each of which have their own extensions with some
backwards compatibility.

Sparse files were `tar`red as sparse files (which you can
easily test, because the resulting archive gets smaller, and
GNU `cpio` can no longer read it).

---

Next: [Reliability and Security](#Reliability-and-security), Previous: [Controlling the Archive Format](#Formats), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## 9 Tapes and Other Archive Media

> *(This message will disappear, once this node revised.)*

A few special cases about tape handling warrant more detailed
description. These special cases are discussed below.

Many complexities surround the use of `tar` on tape drives. Since
the creation and manipulation of archives located on magnetic tape was
the original purpose of `tar`, it contains many features making
such manipulation easier.

Archives are usually written on dismountable media—tape cartridges,
mag tapes, or floppy disks.

The amount of data a tape or disk holds depends not only on its size,
but also on how it is formatted. A 2400 foot long reel of mag tape
holds 40 megabytes of data when formatted at 1600 bits per inch. The
physically smaller EXABYTE tape cartridge holds 2.3 gigabytes.

Magnetic media are re-usable—once the archive on a tape is no longer
needed, the archive can be erased and the tape or disk used over.
Media quality does deteriorate with use, however. Most tapes or disks
should be discarded when they begin to produce data errors. EXABYTE
tape cartridges should be discarded when they generate an *error
count* (number of non-usable bits) of more than 10k.

Magnetic media are written and erased using magnetic fields, and
should be protected from such fields to avoid damage to stored data.
Sticking a floppy disk to a filing cabinet using a magnet is probably
not a good idea.

* [Device Selection and Switching](#Device)
* [Remote Tape Server](#Remote-Tape-Server)
* [Some Common Problems and their Solutions](#Common-Problems-and-Solutions)
* [Blocking](#Blocking)
* [Many Archives on One Tape](#Many)
* [Using Multiple Tapes](#Using-Multiple-Tapes)
* [Including a Label in the Archive](#label)
* [Verifying Data as It is Stored](#verify)
* [Write Protection](#Write-Protection)

---

Next: [Remote Tape Server](#Remote-Tape-Server), Up: [Tapes and Other Archive Media](#Media)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 9.1 Device Selection and Switching

> *(This message will disappear, once this node revised.)*

`-f [hostname:]file`

`--file=[hostname:]file`
:   Use archive file or device file on hostname.

This option is used to specify the file name of the archive `tar`
works on.

If the file name is ‘`-`’, `tar` reads the archive from standard
input (when listing or extracting), or writes it to standard output
(when creating). If the ‘`-`’ file name is given when updating an
archive, `tar` will read the original archive from its standard
input, and will write the entire new archive to its standard output.

If the file name contains a ‘`:`’, it is interpreted as
‘`hostname:file name`’. If the hostname contains an *at*
sign (‘`@`’), it is treated as ‘`user@hostname:file name`’. In
either case, `tar` will invoke the command `rsh` (or
`remsh`) to start up an `/usr/libexec/rmt` on the remote
machine. If you give an alternate login name, it will be given to the
`rsh`.
Naturally, the remote machine must have an executable
`/usr/libexec/rmt`. This program is free software from the
University of California, and a copy of the source code can be found
with the sources for `tar`; it’s compiled and installed by default.
The exact path to this utility is determined when configuring the package.
It is `prefix/libexec/rmt`, where prefix stands for
your installation prefix. This location may also be overridden at
runtime by using the `--rmt-command=command` option (See [—rmt-command](#Option-Summary), for detailed description of this option. See [Remote Tape Server](#Remote-Tape-Server), for the description of `rmt` command).

If this option is not given, but the environment variable `TAPE`
is set, its value is used; otherwise, old versions of `tar`
used a default archive name (which was picked when `tar` was
compiled). The default is normally set up to be the *first* tape
drive or other transportable I/O medium on the system.

Starting with version 1.11.5, GNU `tar` uses
standard input and standard output as the default device, and I will
not try anymore supporting automatic device detection at installation
time. This was failing really in too many cases, it was hopeless.
This is now completely left to the installer to override standard
input and standard output for default device, if this seems
preferable. Further, I think *most* actual usages of
`tar` are done with pipes or disks, not really tapes,
cartridges or diskettes.

Some users think that using standard input and output is running
after trouble. This could lead to a nasty surprise on your screen if
you forget to specify an output file name—especially if you are going
through a network or terminal server capable of buffering large amounts
of output. We had so many bug reports in that area of configuring
default tapes automatically, and so many contradicting requests, that
we finally consider the problem to be portably intractable. We could
of course use something like ‘`/dev/tape`’ as a default, but this
is *also* running after various kind of trouble, going from hung
processes to accidental destruction of real tapes. After having seen
all this mess, using standard input and output as a default really
sounds like the only clean choice left, and a very useful one too.

GNU `tar` reads and writes archive in records, I
suspect this is the main reason why block devices are preferred over
character devices. Most probably, block devices are more efficient
too. The installer could also check for ‘`DEFTAPE`’ in
`<sys/mtio.h>`.

`--force-local`
:   Archive file is local even if it contains a colon.

`--rsh-command=command`
:   Use remote command instead of `rsh`. This option exists
    so that people who use something other than the standard `rsh`
    (e.g., a Kerberized `rsh`) can access a remote device.

    When this command is not used, the shell command found when
    the `tar` program was installed is used instead. This is
    the first found of `/usr/ucb/rsh`, `/usr/bin/remsh`,
    `/usr/bin/rsh`, `/usr/bsd/rsh` or `/usr/bin/nsh`.
    The installer may have overridden this by defining the environment
    variable `RSH` *at installation time*.

`-[0-7][lmh]`
:   Specify drive and density.

`-M`

`--multi-volume`
:   Create/list/extract multi-volume archive.

    This option causes `tar` to write a *multi-volume* archive—one
    that may be larger than will fit on the medium used to hold it.
    See [Archives Longer than One Tape or Disk](#Multi_002dVolume-Archives).

`-L num`

`--tape-length=size[suf]`
:   Change tape after writing size units of data. Unless suf is
    given, size is treated as kilobytes, i.e. ‘`size x
    1024`’ bytes. The following suffixes alter this behavior:

    | Suffix | Units | Byte Equivalent |
    | --- | --- | --- |
    | b | Blocks | size x 512 |
    | B | Kilobytes | size x 1024 |
    | c | Bytes | size |
    | G | Gigabytes | size x 1024^3 |
    | K | Kilobytes | size x 1024 |
    | k | Kilobytes | size x 1024 |
    | M | Megabytes | size x 1024^2 |
    | P | Petabytes | size x 1024^5 |
    | T | Terabytes | size x 1024^4 |
    | w | Words | size x 2 |

    **Table 9.1:** Size Suffixes

    This option might be useful when your tape drivers do not properly
    detect end of physical tapes. By being slightly conservative on the
    maximum tape length, you might avoid the problem entirely.

`-F command`

`--info-script=command`

`--new-volume-script=command`
:   Execute command at end of each tape. This implies
    `--multi-volume` (`-M`). See [info-script](#info_002dscript), for a detailed
    description of this option.

---

Next: [Some Common Problems and their Solutions](#Common-Problems-and-Solutions), Previous: [Device Selection and Switching](#Device), Up: [Tapes and Other Archive Media](#Media)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 9.2 Remote Tape Server

In order to access the tape drive on a remote machine, `tar`
uses the remote tape server written at the University of California at
Berkeley. The remote tape server must be installed as
`prefix/libexec/rmt` on any machine whose tape drive you
want to use. `tar` calls `rmt` by running an
`rsh` or `remsh` to the remote machine, optionally
using a different login name if one is supplied.

A copy of the source for the remote tape server is provided. Its
source code can be freely distributed. It is compiled and
installed by default.

Unless you use the `--absolute-names` (`-P`) option,
GNU `tar` will not allow you to create an archive that contains
absolute file names (a file name beginning with ‘`/`’). If you try,
`tar` will automatically remove the leading ‘`/`’ from the
file names it stores in the archive. It will also type a warning
message telling you what it is doing.

When reading an archive that was created with a different
`tar` program, GNU `tar` automatically
extracts entries in the archive which have absolute file names as if
the file names were not absolute. This is an important feature. A
visitor here once gave a `tar` tape to an operator to restore;
the operator used Sun `tar` instead of GNU `tar`,
and the result was that it replaced large portions of
our `/bin` and friends with versions from the tape; needless to
say, we were unhappy about having to recover the file system from
backup tapes.

For example, if the archive contained a file `/usr/bin/computoy`,
GNU `tar` would extract the file to `usr/bin/computoy`,
relative to the working directory. If you want to extract the files in
an archive to the same absolute names that they had when the archive
was created, you should do a ‘`cd /`’ before extracting the files
from the archive, or you should either use the `--absolute-names`
option, or use the command ‘`tar -C / …`’.

Some versions of Unix (Ultrix 3.1 is known to have this problem),
can claim that a short write near the end of a tape succeeded,
when it actually failed. This will result in the -M option not
working correctly. The best workaround at the moment is to use a
significantly larger blocking factor than the default 20.

In order to update an archive, `tar` must be able to backspace the
archive in order to reread or rewrite a record that was just read (or
written). This is currently possible only on two kinds of files: normal
disk files (or any other file that can be backspaced with ‘`lseek`’),
and industry-standard 9-track magnetic tape (or any other kind of tape
that can be backspaced with the `MTIOCTOP` `ioctl`).

This means that the `--append`, `--concatenate`, and
`--delete` commands will not work on any other kind of file.
Some media simply cannot be backspaced, which means these commands and
options will never be able to work on them. These non-backspacing
media include pipes and cartridge tape drives.

Some other media can be backspaced, and `tar` will work on them
once `tar` is modified to do so.

Archives created with the `--multi-volume`, `--label`, and
`--incremental` (`-G`) options may not be readable by other version
of `tar`. In particular, restoring a file that was split over
a volume boundary will require some careful work with `dd`, if
it can be done at all. Other versions of `tar` may also create
an empty file whose name is that of the volume header. Some versions
of `tar` may create normal files instead of directories archived
with the `--incremental` (`-G`) option.

---

Next: [Blocking](#Blocking), Previous: [Remote Tape Server](#Remote-Tape-Server), Up: [Tapes and Other Archive Media](#Media)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 9.3 Some Common Problems and their Solutions

```
errors from system:
permission denied
no such file or directory
not owner

errors from tar:
directory checksum error
header format error

errors from media/system:
i/o error
device busy
```

---

Next: [Many Archives on One Tape](#Many), Previous: [Some Common Problems and their Solutions](#Common-Problems-and-Solutions), Up: [Tapes and Other Archive Media](#Media)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]
