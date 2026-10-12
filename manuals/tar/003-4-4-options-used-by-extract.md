### 4.4 Options Used by `--extract`

The previous chapter showed how to use `--extract` to extract
an archive into the file system. Various options cause `tar` to
extract more information than just file contents, such as the owner,
the permissions, the modification date, and so forth. This section
presents options to be used with `--extract` when certain special
considerations arise. You may review the information presented in
[How to Extract Members from an Archive](#extract) for more basic information about the
`--extract` operation.

* [Options to Help Read Archives](#Reading)
* [Changing How `tar` Writes Files](#Writing)
* [Coping with Scarce Resources](#Scarce)

---

Next: [Changing How `tar` Writes Files](#Writing), Up: [Options Used by `--extract`](#extract-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.4.1 Options to Help Read Archives

Normally, `tar` will request data in full record increments from
an archive storage device. If the device cannot return a full record,
`tar` will report an error. However, some devices do not always
return full records, or do not require the last record of an archive to
be padded out to the next record boundary. To keep reading until you
obtain a full record, or to accept an incomplete record if it contains
an end-of-archive marker, specify the `--read-full-records` (`-B`) option
in conjunction with the `--extract` or `--list` operations.
See [Blocking](#Blocking).

The `--read-full-records` (`-B`) option is turned on by default when
`tar` reads an archive from standard input, or from a remote
machine. This is because on BSD Unix systems, attempting to read a
pipe returns however much happens to be in the pipe, even if it is
less than was requested. If this option were not enabled, `tar`
would fail as soon as it read an incomplete record from the pipe.

If you’re not sure of the blocking factor of an archive, you can
read the archive by specifying `--read-full-records` (`-B`) and
`--blocking-factor=512-size` (`-b
512-size`), using a blocking factor larger than what the archive
uses. This lets you avoid having to determine the blocking factor
of an archive. See [The Blocking Factor of an Archive](#Blocking-Factor).

* [Reading Full Records](#read-full-records)
* [Ignoring Blocks of Zeros](#Ignore-Zeros)

---

Next: [Ignoring Blocks of Zeros](#Ignore-Zeros), Up: [Options to Help Read Archives](#Reading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Reading Full Records

`--read-full-records`

`-B`
:   Use in conjunction with `--extract` (`--get`,
    `-x`) to read an archive which contains incomplete records, or
    one which has a blocking factor less than the one specified.

---

Previous: [Reading Full Records](#read-full-records), Up: [Options to Help Read Archives](#Reading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Ignoring Blocks of Zeros

Normally, `tar` stops reading when it encounters a block of zeros
between file entries (which usually indicates the end of the archive).
`--ignore-zeros` (`-i`) allows `tar` to
completely read an archive which contains a block of zeros before the
end (i.e., a damaged archive, or one that was created by concatenating
several archives together). This option also suppresses warnings
about missing or incomplete zero blocks at the end of the archive.
This can be turned on, if the need be, using the
`--warning=alone-zero-block --warning=missing-zero-blocks`
options (see [Controlling Warning Messages](#warnings)).

The `--ignore-zeros` (`-i`) option is turned off by default because many
versions of `tar` write garbage after the end-of-archive entry,
since that part of the media is never supposed to be read. GNU `tar`
does not write after the end of an archive, but seeks to
maintain compatibility among archiving utilities.

`--ignore-zeros`

`-i`
:   To ignore blocks of zeros (i.e., end-of-archive entries) which may be
    encountered while reading an archive. Use in conjunction with
    `--extract` or `--list`.

---

Next: [Coping with Scarce Resources](#Scarce), Previous: [Options to Help Read Archives](#Reading), Up: [Options Used by `--extract`](#extract-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.4.2 Changing How `tar` Writes Files

> *(This message will disappear, once this node revised.)*

* [Options Controlling the Overwriting of Existing Files](#Dealing-with-Old-Files)
* [Overwrite Old Files](#Overwrite-Old-Files)
* [Keep Old Files](#Keep-Old-Files)
* [Keep Newer Files](#Keep-Newer-Files)
* [Unlink First](#Unlink-First)
* [Recursive Unlink](#Recursive-Unlink)
* [Setting Data Modification Times](#Data-Modification-Times)
* [Setting Access Permissions](#Setting-Access-Permissions)
* [Directory Modification Times and Permissions](#Directory-Modification-Times-and-Permissions)
* [Writing to Standard Output](#Writing-to-Standard-Output)
* [Writing to an External Program](#Writing-to-an-External-Program)
* [Removing Files](#remove-files)

---

Next: [Overwrite Old Files](#Overwrite-Old-Files), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Options Controlling the Overwriting of Existing Files

When extracting files, if `tar` discovers that the extracted
file already exists, it normally replaces the file by removing it before
extracting it, to prevent confusion in the presence of hard or symbolic
links. (If the existing file is a symbolic link, it is removed, not
followed.) However, if a directory cannot be removed because it is
nonempty, `tar` normally overwrites its metadata (ownership,
permission, etc.). The `--overwrite-dir` option enables this
default behavior. To be more cautious and preserve the metadata of
such a directory, use the `--no-overwrite-dir` option.

To be even more cautious and prevent existing files from being replaced, use
the `--keep-old-files` (`-k`) option. It causes
`tar` to refuse to replace or update a file that already
exists, i.e., a file with the same name as an archive member prevents
extraction of that archive member. Instead, it reports an error. For
example:

```
$ ls
blues
$ tar -x -k -f archive.tar
tar: blues: Cannot open: File exists
tar: Exiting with failure status due to previous errors
```

If you wish to preserve old files untouched, but don’t want
`tar` to treat them as errors, use the
`--skip-old-files` option. This option causes `tar` to
silently skip extracting over existing files.

To be more aggressive about altering existing files, use the
`--overwrite` option. It causes `tar` to overwrite
existing files and to follow existing symbolic links when extracting.

Some people argue that GNU `tar` should not hesitate
to overwrite files with other files when extracting. When extracting
a `tar` archive, they expect to see a faithful copy of the
state of the file system when the archive was created. It is debatable
that this would always be a proper behavior. For example, suppose one
has an archive in which `usr/local` is a link to
`usr/local2`. Since then, maybe the site removed the link and
renamed the whole hierarchy from `/usr/local2` to
`/usr/local`. Such things happen all the time. I guess it would
not be welcome at all that GNU `tar` removes the
whole hierarchy just to make room for the link to be reinstated
(unless it *also* simultaneously restores the full
`/usr/local2`, of course!) GNU `tar` is indeed
able to remove a whole hierarchy to reestablish a symbolic link, for
example, but *only if* `--recursive-unlink` is specified
to allow this behavior. In any case, single files are silently
removed.

Finally, the `--unlink-first` (`-U`) option can improve performance in
some cases by causing `tar` to remove files unconditionally
before extracting them.

---

Next: [Keep Old Files](#Keep-Old-Files), Previous: [Options Controlling the Overwriting of Existing Files](#Dealing-with-Old-Files), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Overwrite Old Files

`--overwrite`
:   Overwrite existing files and directory metadata when extracting files
    from an archive.

    This causes `tar` to write extracted files into the file system without
    regard to the files already on the system; i.e., files with the same
    names as archive members are overwritten when the archive is extracted.
    It also causes `tar` to extract the ownership, permissions,
    and time stamps onto any preexisting files or directories.
    If the name of a corresponding file name is a symbolic link, the file
    pointed to by the symbolic link will be overwritten instead of the
    symbolic link itself (if this is possible). Moreover, special devices,
    empty directories and even symbolic links are automatically removed if
    they are in the way of extraction.

    Be careful when using the `--overwrite` option, particularly when
    combined with the `--absolute-names` (`-P`) option, as this combination
    can change the contents, ownership or permissions of any file on your
    system. Also, many systems do not take kindly to overwriting files that
    are currently being executed.

`--overwrite-dir`
:   Overwrite the metadata of directories when extracting files from an
    archive, but remove other files before extracting.

---

Next: [Keep Newer Files](#Keep-Newer-Files), Previous: [Overwrite Old Files](#Overwrite-Old-Files), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Keep Old Files

GNU `tar` provides two options to control its actions in a situation
when it is about to extract a file which already exists on disk.

`--keep-old-files`

`-k`
:   Do not replace existing files from archive. When such a file is
    encountered, `tar` issues an error message. Upon end of
    extraction, `tar` exits with code 2 (see [exit status](#exit-status)).

`--skip-old-files`
:   Do not replace existing files from archive, but do not treat that
    as error. Such files are silently skipped and do not affect
    `tar` exit status.

    Additional verbosity can be obtained using `--warning=existing-file`
    together with that option (see [Controlling Warning Messages](#warnings)).

---

Next: [Unlink First](#Unlink-First), Previous: [Keep Old Files](#Keep-Old-Files), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Keep Newer Files

`--keep-newer-files`
:   Do not replace existing files that are newer than their archive
    copies. This option is meaningless with `--list` (`-t`).

---

Next: [Recursive Unlink](#Recursive-Unlink), Previous: [Keep Newer Files](#Keep-Newer-Files), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Unlink First

`--unlink-first`

`-U`
:   Remove files before extracting over them.
    This can make `tar` run a bit faster if you know in advance
    that the extracted files all need to be removed. Normally this option
    slows `tar` down slightly, so it is disabled by default.

---

Next: [Setting Data Modification Times](#Data-Modification-Times), Previous: [Unlink First](#Unlink-First), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Recursive Unlink

`--recursive-unlink`
:   When this option is specified, try removing files and directory hierarchies
    before extracting over them. *This is a dangerous option!*

If you specify the `--recursive-unlink` option,
`tar` removes *anything* that keeps you from extracting a file
as far as current permissions will allow it. This could include removal
of the contents of a full directory hierarchy.

---

Next: [Setting Access Permissions](#Setting-Access-Permissions), Previous: [Recursive Unlink](#Recursive-Unlink), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Setting Data Modification Times

Normally, `tar` sets the data modification times of extracted
files to the corresponding times recorded for the files in the archive, but
limits the permissions of extracted files by the current `umask`
setting.

To set the data modification times of extracted files to the time when
the files were extracted, use the `--touch` (`-m`) option in
conjunction with `--extract` (`--get`, `-x`).

`--touch`

`-m`
:   Sets the data modification time of extracted archive members to the time
    they were extracted, not the time recorded for them in the archive.
    Use in conjunction with `--extract` (`--get`, `-x`).

---

Next: [Directory Modification Times and Permissions](#Directory-Modification-Times-and-Permissions), Previous: [Setting Data Modification Times](#Data-Modification-Times), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Setting Access Permissions

To set the modes (access permissions) of extracted files to those
recorded for those files in the archive, use `--same-permissions`
in conjunction with the `--extract` (`--get`,
`-x`) operation.

`--preserve-permissions`

`--same-permissions`

`-p`
:   Set modes of extracted archive members to those recorded in the
    archive, instead of current umask settings. Use in conjunction with
    `--extract` (`--get`, `-x`).

---

Next: [Writing to Standard Output](#Writing-to-Standard-Output), Previous: [Setting Access Permissions](#Setting-Access-Permissions), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Directory Modification Times and Permissions

After successfully extracting a file member, GNU `tar` normally
restores its permissions and modification times, as described in the
previous sections. This cannot be done for directories, because
after extracting a directory `tar` will almost certainly
extract files into that directory and this will cause the directory
modification time to be updated. Moreover, restoring that directory
permissions may not permit file creation within it. Thus, restoring
directory permissions and modification times must be delayed at least
until all files have been extracted into that directory. GNU `tar`
restores directories using the following approach.

The extracted directories are created with the mode specified in the
archive, as modified by the umask of the user, which gives sufficient
permissions to allow file creation. The meta-information about the
directory is recorded in the temporary list of directories. When
preparing to extract next archive member, GNU `tar` checks if the
directory prefix of this file contains the remembered directory. If
it does not, the program assumes that all files have been extracted
into that directory, restores its modification time and permissions
and removes its entry from the internal list. This approach allows
to correctly restore directory meta-information in the majority of
cases, while keeping memory requirements sufficiently small. It is
based on the fact, that most `tar` archives use the predefined
order of members: first the directory, then all the files and
subdirectories in that directory.

However, this is not always true. The most important exception are
incremental archives (see [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps)). The member order in
an incremental archive is reversed: first all directory members are
stored, followed by other (non-directory) members. So, when extracting
from incremental archives, GNU `tar` alters the above procedure. It
remembers all restored directories, and restores their meta-data
only after the entire archive has been processed. Notice, that you do
not need to specify any special options for that, as GNU `tar`
automatically detects archives in incremental format.

There may be cases, when such processing is required for normal archives
too. Consider the following example:

```
$ tar --no-recursion -cvf archive \
    foo foo/file1 bar bar/file foo/file2
foo/
foo/file1
bar/
bar/file
foo/file2
```

During the normal operation, after encountering `bar`
GNU `tar` will assume that all files from the directory `foo`
were already extracted and will therefore restore its timestamp and
permission bits. However, after extracting `foo/file2` the
directory timestamp will be offset again.

To correctly restore directory meta-information in such cases, use
the `--delay-directory-restore` command line option:

`--delay-directory-restore`
:   Delays restoring of the modification times and permissions of extracted
    directories until the end of extraction. This way, correct
    meta-information is restored even if the archive has unusual member
    ordering.

`--no-delay-directory-restore`
:   Cancel the effect of the previous `--delay-directory-restore`.
    Use this option if you have used `--delay-directory-restore` in
    `TAR_OPTIONS` variable (see [TAR\_OPTIONS](#TAR_005fOPTIONS)) and wish to
    temporarily disable it.

---

Next: [Writing to an External Program](#Writing-to-an-External-Program), Previous: [Directory Modification Times and Permissions](#Directory-Modification-Times-and-Permissions), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Writing to Standard Output

To write the extracted files to the standard output, instead of
creating the files on the file system, use `--to-stdout` (`-O`) in
conjunction with `--extract` (`--get`, `-x`). This option is useful if you are
extracting files to send them through a pipe, and do not need to
preserve them in the file system. If you extract multiple members,
they appear on standard output concatenated, in the order they are
found in the archive.

`--to-stdout`

`-O`
:   Writes files to the standard output. Use only in conjunction with
    `--extract` (`--get`, `-x`). When this option is
    used, instead of creating the files specified, `tar` writes
    the contents of the files extracted to its standard output. This may
    be useful if you are only extracting the files in order to send them
    through a pipe. This option is meaningless with `--list`
    (`-t`).

This can be useful, for example, if you have a tar archive containing
a big file and don’t want to store the file on disk before processing
it. You can use a command like this:

```
tar -xOzf foo.tgz bigfile | process
```

or even like this if you want to process the concatenation of the files:

```
tar -xOzf foo.tgz bigfile1 bigfile2 | process
```

However, `--to-command` may be more convenient for use with
multiple files. See the next section.

---

Next: [Removing Files](#remove-files), Previous: [Writing to Standard Output](#Writing-to-Standard-Output), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Writing to an External Program

You can instruct `tar` to send the contents of each extracted
file to the standard input of an external program:

`--to-command=command`
:   Extract files and pipe their contents to the standard input of
    command. When this option is used, instead of creating the
    files specified, `tar` invokes command and pipes the
    contents of the files to its standard output. The command may
    contain command line arguments (see [Running External Commands](#external),
    for more detail).

    Notice, that command is executed once for each regular file
    extracted. Non-regular files (directories, etc.) are ignored when this
    option is used.

The command can obtain the information about the file it processes
from the following environment variables:

`TAR_FILETYPE`
:   Type of the file. It is a single letter with the following meaning:

    |  |  |
    | --- | --- |
    | f | Regular file |
    | d | Directory |
    | l | Symbolic link |
    | h | Hard link |
    | b | Block device |
    | c | Character device |

    Currently only regular files are supported.

`TAR_MODE`
:   File mode, an octal number.

`TAR_FILENAME`
:   The name of the file.

`TAR_REALNAME`
:   Name of the file as stored in the archive.

`TAR_UNAME`
:   Name of the file owner.

`TAR_GNAME`
:   Name of the file owner group.

`TAR_ATIME`
:   Time of last access. It is a decimal number, representing seconds
    since the Epoch. If the archive provides times with nanosecond
    precision, the nanoseconds are appended to the timestamp after a
    decimal point.

`TAR_MTIME`
:   Time of last modification.

`TAR_CTIME`
:   Time of last status change.

`TAR_SIZE`
:   Size of the file.

`TAR_UID`
:   UID of the file owner.

`TAR_GID`
:   GID of the file owner.

Additionally, the following variables contain information about
tar mode and the archive being processed:

`TAR_VERSION`
:   GNU `tar` version number.

`TAR_ARCHIVE`
:   The name of the archive `tar` is processing.

`TAR_BLOCKING_FACTOR`
:   Current blocking factor (see [Blocking](#Blocking)).

`TAR_VOLUME`
:   Ordinal number of the volume `tar` is processing.

`TAR_FORMAT`
:   Format of the archive being processed. See [Controlling the Archive Format](#Formats), for a complete
    list of archive format names.

These variables are defined prior to executing the command, so you can
pass them as arguments, if you prefer. For example, if the command
proc takes the member name and size as its arguments, then you
could do:

```
$ tar -x -f archive.tar \
       --to-command='proc $TAR_FILENAME $TAR_SIZE'
```

Notice single quotes to prevent variable names from being expanded by
the shell when invoking `tar`.

If command exits with a non-0 status, `tar` will print
an error message similar to the following:

```
tar: 2345: Child returned status 1
```

Here, ‘`2345`’ is the PID of the finished process.

If this behavior is not wanted, use `--ignore-command-error`:

`--ignore-command-error`
:   Ignore exit codes of subprocesses. Notice that if the program
    exits on signal or otherwise terminates abnormally, the error message
    will be printed even if this option is used.

`--no-ignore-command-error`
:   Cancel the effect of any previous `--ignore-command-error`
    option. This option is useful if you have set
    `--ignore-command-error` in `TAR_OPTIONS`
    (see [TAR\_OPTIONS](#TAR_005fOPTIONS)) and wish to temporarily cancel it.

---

Previous: [Writing to an External Program](#Writing-to-an-External-Program), Up: [Changing How `tar` Writes Files](#Writing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Removing Files

`--remove-files`
:   Remove files after adding them to the archive.

---

Previous: [Changing How `tar` Writes Files](#Writing), Up: [Options Used by `--extract`](#extract-options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 4.4.3 Coping with Scarce Resources

> *(This message will disappear, once this node revised.)*

* [Starting File](#Starting-File)
* [Same Order](#Same-Order)

---

Next: [Same Order](#Same-Order), Up: [Coping with Scarce Resources](#Scarce)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Starting File

`--starting-file=name`

`-K name`
:   Starts an operation in the middle of an archive. Use in conjunction
    with `--extract` (`--get`, `-x`) or `--list` (`-t`).

If a previous attempt to extract files failed due to lack of disk
space, you can use `--starting-file=name` (`-K
name`) to start extracting only after member name of the
archive. This assumes, of course, that there is now free space, or
that you are now extracting into a different file system. (You could
also choose to suspend `tar`, remove unnecessary files from
the file system, and then resume the same `tar` operation.
In this case, `--starting-file` is not necessary.) See also
[Asking for Confirmation During Operations](#interactive), and [Excluding Some Files](#exclude).

---

Previous: [Starting File](#Starting-File), Up: [Coping with Scarce Resources](#Scarce)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Same Order

`--same-order`

`--preserve-order`

`-s`
:   To process large lists of file names on machines with small amounts of
    memory. Use in conjunction with `--compare` (`--diff`,
    `-d`), `--list` (`-t`) or `--extract`
    (`--get`, `-x`).

The `--same-order` (`--preserve-order`, `-s`) option tells `tar` that the list of file
names to be listed or extracted is sorted in the same order as the
files in the archive. This allows a large list of names to be used,
even on a small machine that would not otherwise be able to hold all
the names in memory at the same time. Such a sorted list can easily be
created by running ‘`tar -t`’ on the archive and editing its output.

This option is probably never needed on modern computer systems.

---

Next: [Looking Ahead: The Rest of this Manual](#looking-ahead), Previous: [Options Used by `--extract`](#extract-options), Up: [GNU `tar` Operations](#operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 4.5 Backup options

GNU `tar` offers options for making backups of files
before writing new versions. These options control the details of
these backups. They may apply to the archive itself before it is
created or rewritten, as well as individual extracted members. Other
GNU programs (`cp`, `install`, `ln`,
and `mv`, for example) offer similar options.

Backup options may prove unexpectedly useful when extracting archives
containing many members having identical name, or when extracting archives
on systems having file name limitations, making different members appear
as having similar names through the side-effect of name truncation.

When any existing file is backed up before being overwritten by extraction,
then clashing files are automatically be renamed to be unique, and the
true name is kept for only the last file of a series of clashing files.
By using verbose mode, users may track exactly what happens.

At the detail level, some decisions are still experimental, and may
change in the future, we are waiting comments from our users. So, please
do not learn to depend blindly on the details of the backup features.
For example, currently, directories themselves are never renamed through
using these options, so, extracting a file over a directory still has
good chances to fail. Also, backup options apply to created archives,
not only to extracted members. For created archives, backups will not
be attempted when the archive is a block or character device, or when it
refers to a remote file.

For the sake of simplicity and efficiency, backups are made by renaming old
files prior to creation or extraction, and not by copying. The original
name is restored if the file creation fails. If a failure occurs after a
partial extraction of a file, both the backup and the partially extracted
file are kept.

‘`--backup[=method]`’ [¶](#index-backup)
:   Back up files that are about to be overwritten or removed.
    Without this option, the original versions are destroyed.

    Use method to determine the type of backups made.
    If method is not specified, use the value of the `VERSION_CONTROL`
    environment variable. And if `VERSION_CONTROL` is not set,
    use the ‘`existing`’ method.

    This option corresponds to the Emacs variable ‘`version-control`’;
    the same values for method are accepted as in Emacs. This option
    also allows more descriptive names. The valid methods are:

    ‘`t`’ [¶](#index-numbered-backup-method)

    ‘`numbered`’
    :   Always make numbered backups.

    ‘`nil`’ [¶](#index-existing-backup-method)

    ‘`existing`’
    :   Make numbered backups of files that already have them, simple backups
        of the others.

    ‘`never`’ [¶](#index-simple-backup-method)

    ‘`simple`’
    :   Always make simple backups.

‘`--suffix=suffix`’ [¶](#index-suffix)
:   Append suffix to each backup file made with `--backup`. If this
    option is not specified, the value of the `SIMPLE_BACKUP_SUFFIX`
    environment variable is used. And if `SIMPLE_BACKUP_SUFFIX` is not
    set, the default is ‘`~`’, just as in Emacs.

---

Previous: [Backup options](#backup), Up: [GNU `tar` Operations](#operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 4.6 Looking Ahead: The Rest of this Manual

You have now seen how to use all eight of the operations available to
`tar`, and a number of the possible options. The next chapter
explains how to choose and change file and archive names, how to use
files to store names of other files which you can then call as
arguments to `tar` (this can help you save time if you expect to
archive the same list of files a number of times), and so forth.

If there are too many files to conveniently list on the command line,
you can list the names in a file, and `tar` will read that file.
See [Reading Names from a File](#files).

There are various ways of causing `tar` to skip over some files,
and not archive them. See [Choosing Files and Names for `tar`](#Choosing).

---

Next: [Choosing Files and Names for `tar`](#Choosing), Previous: [GNU `tar` Operations](#operations), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## 5 Performing Backups and Restoring Files

GNU `tar` is distributed along with the scripts for performing backups
and restores. Even if there is a good chance those scripts may be
satisfying to you, they are not the only scripts or methods available for doing
backups and restore. You may well create your own, or use more
sophisticated packages dedicated to that purpose.

Some users are enthusiastic about `Amanda` (The Advanced Maryland
Automatic Network Disk Archiver), a backup system developed by James
da Silva `jds@cs.umd.edu` and available on many Unix systems.
This is free software, and it is available from <http://www.amanda.org>.

This chapter documents both the provided shell scripts and `tar`
options which are more specific to usage as a backup tool.

To *back up* a file system means to create archives that contain
all the files in that file system. Those archives can then be used to
restore any or all of those files (for instance if a disk crashes or a
file is accidentally deleted). File system *backups* are also
called *dumps*.

* [Using `tar` to Perform Full Dumps](#Full-Dumps)
* [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps)
* [Levels of Backups](#Backup-Levels)
* [Setting Parameters for Backups and Restoration](#Backup-Parameters)
* [Using the Backup Scripts](#Scripted-Backups)
* [Using the Restore Script](#Scripted-Restoration)

---

Next: [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps), Up: [Performing Backups and Restoring Files](#Backups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 5.1 Using `tar` to Perform Full Dumps

> *(This message will disappear, once this node revised.)*

Full dumps should only be made when no other people or programs
are modifying files in the file system. If files are modified while
`tar` is making the backup, they may not be stored properly in
the archive, in which case you won’t be able to restore them if you
have to. (Files not being modified are written with no trouble, and do
not corrupt the entire archive.)

You will want to use the `--label=archive-label`
(`-V archive-label`) option to give the archive a
volume label, so you can tell what this archive is even if the label
falls off the tape, or anything like that.

Unless the file system you are dumping is guaranteed to fit on
one volume, you will need to use the `--multi-volume` (`-M`) option.
Make sure you have enough tapes on hand to complete the backup.

If you want to dump each file system separately you will need to use
the `--one-file-system` option to prevent
`tar` from crossing file system boundaries when storing
(sub)directories.

The `--incremental` (`-G`) (see [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps))
option is not needed, since this is a complete copy of everything in
the file system, and a full restore from this backup would only be
done onto a completely
empty disk.

Unless you are in a hurry, and trust the `tar` program (and your
tapes), it is a good idea to use the `--verify` (`-W`)
option, to make sure your files really made it onto the dump properly.
This will also detect cases where the file was modified while (or just
after) it was being archived. Not all media (notably cartridge tapes)
are capable of being verified, unfortunately.

---

Next: [Levels of Backups](#Backup-Levels), Previous: [Using `tar` to Perform Full Dumps](#Full-Dumps), Up: [Performing Backups and Restoring Files](#Backups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 5.2 Using `tar` to Perform Incremental Dumps

*Incremental backup* is a special form of GNU `tar` archive that
stores additional metadata so that exact state of the file system
can be restored when extracting the archive.

GNU `tar` currently offers two options for handling incremental
backups: `--listed-incremental=snapshot-file` (`-g
snapshot-file`) and `--incremental` (`-G`).

The option `--listed-incremental` instructs tar to operate on
an incremental archive with additional metadata stored in a standalone
file, called a *snapshot file*. The purpose of this file is to help
determine which files have been changed, added or deleted since the
last backup, so that the next incremental backup will contain only
modified files. The name of the snapshot file is given as an argument
to the option:

`--listed-incremental=file`

`-g file`
:   Handle incremental backups with snapshot data in file.

To create an incremental backup, you would use
`--listed-incremental` together with `--create`
(see [How to Create Archives](#create)). For example:

```
$ tar --create \
           --file=archive.1.tar \
           --listed-incremental=/var/log/usr.snar \
           /usr
```

This will create in `archive.1.tar` an incremental backup of
the `/usr` file system, storing additional metadata in the file
`/var/log/usr.snar`. If this file does not exist, it will be
created. The created archive will then be a *level 0 backup*;
please see the next section for more on backup levels.

Otherwise, if the file `/var/log/usr.snar` exists, it
determines which files are modified. In this case only these files will be
stored in the archive. Suppose, for example, that after running the
above command, you delete file `/usr/doc/old` and create
directory `/usr/local/db` with the following contents:

```
$ ls /usr/local/db
/usr/local/db/data
/usr/local/db/index
```

Some time later you create another incremental backup. You will
then see:

```
$ tar --create \
           --file=archive.2.tar \
           --listed-incremental=/var/log/usr.snar \
           /usr
tar: usr/local/db: Directory is new
usr/local/db/
usr/local/db/data
usr/local/db/index
```

The created archive `archive.2.tar` will contain only these
three members. This archive is called a *level 1 backup*. Notice
that `/var/log/usr.snar` will be updated with the new data, so if
you plan to create more ‘`level 1`’ backups, it is necessary to
create a working copy of the snapshot file before running
`tar`. The above example will then be modified as follows:

```
$ cp /var/log/usr.snar /var/log/usr.snar-1
$ tar --create \
           --file=archive.2.tar \
           --listed-incremental=/var/log/usr.snar-1 \
           /usr
```

You can force ‘`level 0`’ backups either by removing the snapshot
file before running `tar`, or by supplying the
`--level=0` option, e.g.:

```
$ tar --create \
           --file=archive.2.tar \
           --listed-incremental=/var/log/usr.snar-0 \
           --level=0 \
           /usr
```

Incremental dumps depend crucially on time stamps, so the results are
unreliable if you modify a file’s time stamps during dumping (e.g.,
with the `--atime-preserve=replace` option), or if you set the clock
backwards.

Metadata stored in snapshot files include device numbers, which,
obviously are supposed to be non-volatile values. However, it turns
out that NFS devices have undependable values when an automounter
gets in the picture. This can lead to a great deal of spurious
redumping in incremental dumps, so it is somewhat useless to compare
two NFS devices numbers over time. The solution implemented
currently is to consider all NFS devices as being equal
when it comes to comparing directories; this is fairly gross, but
there does not seem to be a better way to go.

Apart from using NFS, there are a number of cases where
relying on device numbers can cause spurious redumping of unmodified
files. For example, this occurs when archiving LVM snapshot
volumes. To avoid this, use `--no-check-device` option:

`--no-check-device`
:   Do not rely on device numbers when preparing a list of changed files
    for an incremental dump.

`--check-device`
:   Use device numbers when preparing a list of changed files
    for an incremental dump. This is the default behavior. The purpose
    of this option is to undo the effect of the `--no-check-device`
    if it was given in `TAR_OPTIONS` environment variable
    (see [TAR\_OPTIONS](#TAR_005fOPTIONS)).

There is also another way to cope with changing device numbers. It is
described in detail in [Fixing Snapshot Files](#Fixing-Snapshot-Files).

Note that incremental archives use `tar` extensions and may
not be readable by non-GNU versions of the `tar` program.

To extract from the incremental dumps, use
`--listed-incremental` together with `--extract`
option (see [Extracting Specific Files](#extracting-files)). In this case, `tar` does
not need to access snapshot file, since all the data necessary for
extraction are stored in the archive itself. So, when extracting, you
can give whatever argument to `--listed-incremental`, the usual
practice is to use `--listed-incremental=/dev/null`.
Alternatively, you can use `--incremental`, which needs no
arguments. In general, `--incremental` (`-G`) can be
used as a shortcut for `--listed-incremental` when listing or
extracting incremental backups (for more information regarding this
option, see [incremental-op](#incremental_002dop)).

When extracting from the incremental backup GNU `tar` attempts to
restore the exact state the file system had when the archive was
created. In particular, it will *delete* those files in the file
system that did not exist in their directories when the archive was
created. If you have created several levels of incremental files,
then in order to restore the exact contents the file system had when
the last level was created, you will need to restore from all backups
in turn. Continuing our example, to restore the state of `/usr`
file system, one would do[12](#FOOT12):

```
$ tar --extract \
           --listed-incremental=/dev/null \
           --file archive.1.tar
$ tar --extract \
           --listed-incremental=/dev/null \
           --file archive.2.tar
```

To list the contents of an incremental archive, use `--list`
(see [How to List Archives](#list)), as usual. To obtain more information about the
archive, use `--listed-incremental` or `--incremental`
combined with two `--verbose` options[13](#FOOT13):

```
tar --list --incremental --verbose --verbose --file archive.tar
```

This command will print, for each directory in the archive, the list
of files in that directory at the time the archive was created. This
information is put out in a format which is both human-readable and
unambiguous for a program: each file name is printed as

```
x file
```

where x is a letter describing the status of the file: ‘`Y`’
if the file is present in the archive, ‘`N`’ if the file is not
included in the archive, or a ‘`D`’ if the file is a directory (and
is included in the archive). See [Dumpdir](#Dumpdir), for the detailed
description of dumpdirs and status codes. Each such
line is terminated by a newline character. The last line is followed
by an additional newline to indicate the end of the data.

The option `--incremental` (`-G`)
gives the same behavior as `--listed-incremental` when used
with `--list` and `--extract` options. When used with
`--create` option, it creates an incremental archive without
creating snapshot file. Thus, it is impossible to create several
levels of incremental backups with `--incremental` option.

---

Next: [Setting Parameters for Backups and Restoration](#Backup-Parameters), Previous: [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps), Up: [Performing Backups and Restoring Files](#Backups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 5.3 Levels of Backups

An archive containing all the files in the file system is called a
*full backup* or *full dump*. You could insure your data by
creating a full dump every day. This strategy, however, would waste a
substantial amount of archive media and user time, as unchanged files
are daily re-archived.

It is more efficient to do a full dump only occasionally. To back up
files between full dumps, you can use *incremental dumps*. A *level
one* dump archives all the files that have changed since the last full
dump.

A typical dump strategy would be to perform a full dump once a week,
and a level one dump once a day. This means some versions of files
will in fact be archived more than once, but this dump strategy makes
it possible to restore a file system to within one day of accuracy by
only extracting two archives—the last weekly (full) dump and the
last daily (level one) dump. The only information lost would be in
files changed or created since the last daily backup. (Doing dumps
more than once a day is usually not worth the trouble.)

GNU `tar` comes with scripts you can use to do full
and level-one (actually, even level-two and so on) dumps. Using
scripts (shell programs) to perform backups and restoration is a
convenient and reliable alternative to typing out file name lists
and `tar` commands by hand.

Before you use these scripts, you need to edit the file
`backup-specs`, which specifies parameters used by the backup
scripts and by the restore script. This file is usually located
in `/etc/backup` directory. See [Setting Parameters for Backups and Restoration](#Backup-Parameters), for its
detailed description. Once the backup parameters are set, you can
perform backups or restoration by running the appropriate script.

The name of the backup script is `backup`. The name of the
restore script is `restore`. The following sections describe
their use in detail.

*Please Note:* The backup and restoration scripts are
designed to be used together. While it is possible to restore files by
hand from an archive which was created using a backup script, and to create
an archive by hand which could then be extracted using the restore script,
it is easier to use the scripts. See [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps), before
making such an attempt.

---

Next: [Using the Backup Scripts](#Scripted-Backups), Previous: [Levels of Backups](#Backup-Levels), Up: [Performing Backups and Restoring Files](#Backups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 5.4 Setting Parameters for Backups and Restoration

The file `backup-specs` specifies backup parameters for the
backup and restoration scripts provided with `tar`. You must
edit `backup-specs` to fit your system configuration and schedule
before using these scripts.

Syntactically, `backup-specs` is a shell script, containing
mainly variable assignments. However, any valid shell construct
is allowed in this file. Particularly, you may wish to define
functions within that script (e.g., see `RESTORE_BEGIN` below).
For more information about shell script syntax, please refer to
[the definition of the Shell Command Language](https://pubs.opengroup.org/onlinepubs/009695399/utilities/xcu_chap02.html). See also
[Bash Features](bashref.html#Top) in Bash Reference Manual.

The shell variables controlling behavior of `backup` and
`restore` are described in the following subsections.

* [General-Purpose Variables](#General_002dPurpose-Variables)
* [Magnetic Tape Control](#Magnetic-Tape-Control)
* [User Hooks](#User-Hooks)
* [An Example Text of `Backup-specs`](#backup_002dspecs-example)

---

Next: [Magnetic Tape Control](#Magnetic-Tape-Control), Up: [Setting Parameters for Backups and Restoration](#Backup-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 5.4.1 General-Purpose Variables

Backup variable: **ADMINISTRATOR** [¶](#index-ADMINISTRATOR)
:   The user name of the backup administrator. `Backup` scripts
    sends a backup report to this address.

Backup variable: **BACKUP\_HOUR** [¶](#index-BACKUP_005fHOUR)
:   The hour at which the backups are done. This can be a number from 0
    to 23, or the time specification in form hours:minutes,
    or the string ‘`now`’.

    This variable is used by `backup`. Its value may be overridden
    using `--time` option (see [Using the Backup Scripts](#Scripted-Backups)).

Backup variable: **TAPE\_FILE** [¶](#index-TAPE_005fFILE)
:   The device `tar` writes the archive to. If TAPE\_FILE
    is a remote archive (see [remote-dev](#remote_002ddev)), backup script will suppose
    that your `mt` is able to access remote devices. If RSH
    (see [RSH](#RSH)) is set, `--rsh-command` option will be added to
    invocations of `mt`.

Backup variable: **BLOCKING** [¶](#index-BLOCKING)
:   The blocking factor `tar` will use when writing the dump archive.
    See [The Blocking Factor of an Archive](#Blocking-Factor).

Backup variable: **BACKUP\_DIRS** [¶](#index-BACKUP_005fDIRS)
:   A list of file systems to be dumped (for `backup`), or restored
    (for `restore`). You can include any directory
    name in the list — subdirectories on that file system will be
    included, regardless of how they may look to other networked machines.
    Subdirectories on other file systems will be ignored.

    The host name specifies which host to run `tar` on, and should
    normally be the host that actually contains the file system. However,
    the host machine must have GNU `tar` installed, and
    must be able to access the directory containing the backup scripts and
    their support files using the same file name that is used on the
    machine where the scripts are run (i.e., what `pwd` will print
    when in that directory on that machine). If the host that contains
    the file system does not have this capability, you can specify another
    host as long as it can access the file system through NFS.

    If the list of file systems is very long you may wish to put it
    in a separate file. This file is usually named
    `/etc/backup/dirs`, but this name may be overridden in
    `backup-specs` using `DIRLIST` variable.

Backup variable: **DIRLIST** [¶](#index-DIRLIST)
:   The name of the file that contains a list of file systems to backup
    or restore. By default it is `/etc/backup/dirs`.

Backup variable: **BACKUP\_FILES** [¶](#index-BACKUP_005fFILES)
:   A list of individual files to be dumped (for `backup`), or restored
    (for `restore`). These should be accessible from the machine on
    which the backup script is run.

    If the list of individual files is very long you may wish to store it
    in a separate file. This file is usually named
    `/etc/backup/files`, but this name may be overridden in
    `backup-specs` using `FILELIST` variable.

Backup variable: **FILELIST** [¶](#index-FILELIST)
:   The name of the file that contains a list of individual files to backup
    or restore. By default it is `/etc/backup/files`.

Backup variable: **MT** [¶](#index-MT)
:   Full file name of `mt` binary.

Backup variable: **RSH** [¶](#index-RSH)
:   Full file name of `rsh` binary or its equivalent. You may wish to
    set it to `ssh`, to improve security. In this case you will have
    to use public key authentication.

Backup variable: **RSH\_COMMAND** [¶](#index-RSH_005fCOMMAND)
:   Full file name of `rsh` binary on remote machines. This will
    be passed via `--rsh-command` option to the remote invocation
    of GNU `tar`.

Backup variable: **VOLNO\_FILE** [¶](#index-VOLNO_005fFILE)
:   Name of temporary file to hold volume numbers. This needs to be accessible
    by all the machines which have file systems to be dumped.

Backup variable: **XLIST** [¶](#index-XLIST)
:   Name of *exclude file list*. An *exclude file list* is a file
    located on the remote machine and containing the list of files to
    be excluded from the backup. Exclude file lists are searched in
    /etc/tar-backup directory. A common use for exclude file lists
    is to exclude files containing security-sensitive information
    (e.g., `/etc/shadow` from backups).

    This variable affects only `backup`.

Backup variable: **SLEEP\_TIME** [¶](#index-SLEEP_005fTIME)
:   Time to sleep between dumps of any two successive file systems

    This variable affects only `backup`.

Backup variable: **DUMP\_REMIND\_SCRIPT** [¶](#index-DUMP_005fREMIND_005fSCRIPT)
:   Script to be run when it’s time to insert a new tape in for the next
    volume. Administrators may want to tailor this script for their site.
    If this variable isn’t set, GNU `tar` will display its built-in
    prompt, and will expect confirmation from the console. For the
    description of the default prompt, see [change volume prompt](#change-volume-prompt).

Backup variable: **SLEEP\_MESSAGE** [¶](#index-SLEEP_005fMESSAGE)
:   Message to display on the terminal while waiting for dump time. Usually
    this will just be some literal text.

Backup variable: **TAR** [¶](#index-TAR)
:   Full file name of the GNU `tar` executable. If this is not set, backup
    scripts will search `tar` in the current shell path.

---

Next: [User Hooks](#User-Hooks), Previous: [General-Purpose Variables](#General_002dPurpose-Variables), Up: [Setting Parameters for Backups and Restoration](#Backup-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 5.4.2 Magnetic Tape Control

Backup scripts access tape device using special *hook functions*.
These functions take a single argument — the name of the tape
device. Their names are kept in the following variables:

Backup variable: **MT\_BEGIN** [¶](#index-MT_005fBEGIN)
:   The name of *begin* function. This function is called before
    accessing the drive. By default it retensions the tape:

    ```
    MT_BEGIN=mt_begin

    mt_begin() {
        mt -f "$1" retension
    }
    ```

Backup variable: **MT\_REWIND** [¶](#index-MT_005fREWIND)
:   The name of *rewind* function. The default definition is as
    follows:

    ```
    MT_REWIND=mt_rewind

    mt_rewind() {
        mt -f "$1" rewind
    }
    ```

Backup variable: **MT\_OFFLINE** [¶](#index-MT_005fOFFLINE)
:   The name of the function switching the tape off line. By default
    it is defined as follows:

    ```
    MT_OFFLINE=mt_offline

    mt_offline() {
        mt -f "$1" offl
    }
    ```

Backup variable: **MT\_STATUS** [¶](#index-MT_005fSTATUS)
:   The name of the function used to obtain the status of the archive device,
    including error count. Default definition:

    ```
    MT_STATUS=mt_status

    mt_status() {
        mt -f "$1" status
    }
    ```

---

Next: [An Example Text of `Backup-specs`](#backup_002dspecs-example), Previous: [Magnetic Tape Control](#Magnetic-Tape-Control), Up: [Setting Parameters for Backups and Restoration](#Backup-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 5.4.3 User Hooks

*User hooks* are shell functions executed before and after
each `tar` invocation. Thus, there are *backup
hooks*, which are executed before and after dumping each file
system, and *restore hooks*, executed before and
after restoring a file system. Each user hook is a shell function
taking four arguments:

User Hook Function: **hook** *level host fs fsname* [¶](#index-hook)
:   Its arguments are:

    level
    :   Current backup or restore level.

    host
    :   Name or IP address of the host machine being dumped or restored.

    fs
    :   Full file name of the file system being dumped or restored.

    fsname
    :   File system name with directory separators replaced with colons. This
        is useful, e.g., for creating unique files.

Following variables keep the names of user hook functions:

Backup variable: **DUMP\_BEGIN** [¶](#index-DUMP_005fBEGIN)
:   Dump begin function. It is executed before dumping the file system.

Backup variable: **DUMP\_END** [¶](#index-DUMP_005fEND)
:   Executed after dumping the file system.

Backup variable: **RESTORE\_BEGIN** [¶](#index-RESTORE_005fBEGIN)
:   Executed before restoring the file system.

Backup variable: **RESTORE\_END** [¶](#index-RESTORE_005fEND)
:   Executed after restoring the file system.

---

Previous: [User Hooks](#User-Hooks), Up: [Setting Parameters for Backups and Restoration](#Backup-Parameters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 5.4.4 An Example Text of `Backup-specs`

The following is an example of `backup-specs`:

```
# site-specific parameters for file system backup.

ADMINISTRATOR=friedman
BACKUP_HOUR=1
TAPE_FILE=/dev/nrsmt0

# Use ssh instead of the less secure rsh
RSH=/usr/bin/ssh
RSH_COMMAND=/usr/bin/ssh

# Override MT_STATUS function:
my_status() {
      mts -t $TAPE_FILE
}
MT_STATUS=my_status

# Disable MT_OFFLINE function
MT_OFFLINE=:

BLOCKING=124
BACKUP_DIRS="
        albert:/fs/fsf
        apple-gunkies:/gd
        albert:/fs/gd2
        albert:/fs/gp
        geech:/usr/jla
        churchy:/usr/roland
        albert:/
        albert:/usr
        apple-gunkies:/
        apple-gunkies:/usr
        gnu:/hack
        gnu:/u
        apple-gunkies:/com/mailer/gnu
        apple-gunkies:/com/archive/gnu"

BACKUP_FILES="/com/mailer/aliases /com/mailer/league*[a-z]"
```

---

Next: [Using the Restore Script](#Scripted-Restoration), Previous: [Setting Parameters for Backups and Restoration](#Backup-Parameters), Up: [Performing Backups and Restoring Files](#Backups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 5.5 Using the Backup Scripts

The syntax for running a backup script is:

```
backup --level=level --time=time
```

The `--level` option requests the dump level. Thus, to produce
a full dump, specify `--level=0` (this is the default, so
`--level` may be omitted if its value is
`0`)[14](#FOOT14).

The `--time` option determines when should the backup be
run. Time may take three forms:

hh:mm
:   The dump must be run at hh hours mm minutes.

hh
:   The dump must be run at hh hours.

now
:   The dump must be run immediately.

You should start a script with a tape or disk mounted. Once you
start a script, it prompts you for new tapes or disks as it
needs them. Media volumes don’t have to correspond to archive
files — a multi-volume archive can be started in the middle of a
tape that already contains the end of another multi-volume archive.
The `restore` script prompts for media by its archive volume,
so to avoid an error message you should keep track of which tape
(or disk) contains which volume of the archive (see [Using the Restore Script](#Scripted-Restoration)).

The backup scripts write two files on the file system. The first is a
record file in `/etc/tar-backup/`, which is used by the scripts
to store and retrieve information about which files were dumped. This
file is not meant to be read by humans, and should not be deleted by
them. See [Format of the Incremental Snapshot Files](#Snapshot-Files), for a more detailed explanation of this
file.

The second file is a log file containing the names of the file systems
and files dumped, what time the backup was made, and any error
messages that were generated, as well as how much space was left in
the media volume after the last volume of the archive was written.
You should check this log file after every backup. The file name is
`log-mm-dd-yyyy-level-n`, where mm-dd-yyyy
represents current date, and n represents current dump level number.

The script also prints the name of each system being dumped to the
standard output.

Following is the full list of options accepted by `backup`
script:

`-l level`

`--level=level`
:   Do backup level level (default 0).

`-f`

`--force`
:   Force backup even if today’s log file already exists.

`-v[level]`

`--verbose[=level]`
:   Set verbosity level. The higher the level is, the more debugging
    information will be output during execution. Default level
    is 100, which means the highest debugging level.

`-t start-time`

`--time=start-time`
:   Wait till time, then do backup.

`-h`

`--help`
:   Display short help message and exit.

`-V`

`--version`
:   Display information about the program’s name, version, origin and legal
    status, all on standard output, and then exit successfully.

---

Previous: [Using the Backup Scripts](#Scripted-Backups), Up: [Performing Backups and Restoring Files](#Backups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 5.6 Using the Restore Script

To restore files that were archived using a scripted backup, use the
`restore` script. Its usage is quite straightforward. In the
simplest form, invoke `restore --all`, it will
then restore all the file systems and files specified in
`backup-specs` (see [BACKUP\_DIRS](#General_002dPurpose-Variables)).

You may select the file systems (and/or files) to restore by
giving `restore` a list of *patterns* in its command
line. For example, running

```
restore 'albert:*'
```

will restore all file systems on the machine ‘`albert`’. A more
complicated example:

```
restore 'albert:*' '*:/var'
```

This command will restore all file systems on the machine ‘`albert`’
as well as `/var` file system on all machines.

By default `restore` will start restoring files from the lowest
available dump level (usually zero) and will continue through
all available dump levels. There may be situations where such a
thorough restore is not necessary. For example, you may wish to
restore only files from the recent level one backup. To do so,
use `--level` option, as shown in the example below:

```
restore --level=1
```

The full list of options accepted by `restore` follows:

`-a`

`--all`
:   Restore all file systems and files specified in `backup-specs`.

`-l level`

`--level=level`
:   Start restoring from the given backup level, instead of the default 0.

`-v[level]`

`--verbose[=level]`
:   Set verbosity level. The higher the level is, the more debugging
    information will be output during execution. Default level
    is 100, which means the highest debugging level.

`-h`

`--help`
:   Display short help message and exit.

`-V`

`--version`
:   Display information about the program’s name, version, origin and legal
    status, all on standard output, and then exit successfully.

You should start the restore script with the media containing the
first volume of the archive mounted. The script will prompt for other
volumes as they are needed. If the archive is on tape, you don’t need
to rewind the tape to to its beginning—if the tape head is
positioned past the beginning of the archive, the script will rewind
the tape as needed. See [Tape Positions and Tape Marks](#Tape-Positioning), for a discussion of tape
positioning.

> **Warning:** The script will delete files from the active file
> system if they were not in the file system when the archive was made.

See [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps), for an explanation of how the script makes
that determination.

---

Next: [Date input formats](#Date-input-formats), Previous: [Performing Backups and Restoring Files](#Backups), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## 6 Choosing Files and Names for `tar`

Certain options to `tar` enable you to specify a name for your
archive. Other options let you decide which files to include or exclude
from the archive, based on when or whether files were modified, whether
the file names do or don’t match specified patterns, or whether files
are in specified directories.

This chapter discusses these options in detail.

* [Choosing and Naming Archive Files](#file)
* [Selecting Archive Members](#Selecting-Archive-Members)
* [Reading Names from a File](#files)
* [Excluding Some Files](#exclude)
* [Wildcards Patterns and Matching](#wildcards)
* [Quoting Member Names](#quoting-styles)
* [Modifying File and Member Names](#transform)
* [Operating Only on New Files](#after)
* [Descending into Directories](#recurse)
* [Crossing File System Boundaries](#one)

---

Next: [Selecting Archive Members](#Selecting-Archive-Members), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 6.1 Choosing and Naming Archive Files

By default, `tar` uses an archive file name that was compiled when
it was built on the system; usually this name refers to some physical
tape drive on the machine. However, the person who installed `tar`
on the system may not have set the default to a meaningful value as far as
most users are concerned. As a result, you will usually want to tell
`tar` where to find (or create) the archive. The
`--file=archive-name` (`-f archive-name`)
option allows you to either specify or name a file to use as the archive
instead of the default archive file location.

`--file=archive-name`

`-f archive-name`
:   Name the archive to create or operate on. Use in conjunction with
    any operation.

For example, in this `tar` command,

```
$ tar -cvf collection.tar blues folk jazz
```

`collection.tar` is the name of the archive. It must directly
follow the `-f` option, since whatever directly follows `-f`
*will* end up naming the archive. If you neglect to specify an
archive name, you may end up overwriting a file in the working directory
with the archive you create since `tar` will use this file’s name
for the archive name.

An archive can be saved as a file in the file system, sent through a
pipe or over a network, or written to an I/O device such as a tape,
floppy disk, or CD write drive.

If you do not name the archive, `tar` uses the value of the
environment variable `TAPE` as the file name for the archive. If
that is not available, `tar` uses a default, compiled-in archive
name, usually that for tape unit zero (i.e., `/dev/tu00`).

If you use `-` as an archive-name, `tar` reads the
archive from standard input (when listing or extracting files), or
writes it to standard output (when creating an archive). If you use
`-` as an archive-name when modifying an archive,
`tar` reads the original archive from its standard input and
writes the entire new archive to its standard output.

The following example is a convenient way of copying directory
hierarchy from `sourcedir` to `targetdir`.

```
$ (cd sourcedir; tar -cf - .) | (cd targetdir; tar -xpf -)
```

The `-C` option allows to avoid using subshells:

```
$ tar -C sourcedir -cf - . | tar -C targetdir -xpf -
```

In both examples above, the leftmost `tar` invocation archives
the contents of `sourcedir` to the standard output, while the
rightmost one reads this archive from its standard input and
extracts it. The `-p` option tells it to restore permissions
of the extracted files.

To specify an archive file on a device attached to a remote machine,
use the following:

```
--file=hostname:/dev/file-name
```

`tar` will set up the remote connection, if possible, and
prompt you for a username and password. If you use
`--file=@hostname:/dev/file-name`, `tar`
will attempt to set up the remote connection using your username
as the username on the remote machine.

If the archive file name includes a colon (‘`:`’), then it is assumed
to be a file on another machine. If the archive file is
‘`user@host:file`’, then file is used on the
host host. The remote host is accessed using the `rsh`
program, with a username of user. If the username is omitted
(along with the ‘`@`’ sign), then your user name will be used.
(This is the normal `rsh` behavior.) It is necessary for the
remote machine, in addition to permitting your `rsh` access, to
have the `rmt` program installed (this command is included in
the GNU `tar` distribution and by default is installed under
`prefix/libexec/rmt`, where prefix means your
installation prefix). If you need to use a file whose name includes a
colon, then the remote tape drive behavior
can be inhibited by using the `--force-local` option.

When the archive is being created to `/dev/null`, GNU `tar`
tries to minimize input and output operations. The Amanda backup
system, when used with GNU `tar`, has an initial sizing pass which
uses this feature.

---

Next: [Reading Names from a File](#files), Previous: [Choosing and Naming Archive Files](#file), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 6.2 Selecting Archive Members

*File Name arguments* specify which files in the file system
`tar` operates on, when creating or adding to an archive, or which
archive members `tar` operates on, when reading or deleting from
an archive. See [The Five Advanced `tar` Operations](#Operations).

To specify file names, you can include them as the last arguments on
the command line, as follows:

```
tar operation [option1 option2 …] [file name-1 file name-2 …]
```

If a file name begins with dash (‘`-`’), precede it with
`--add-file` option to prevent it from being treated as an
option.

By default GNU `tar` attempts to *unquote* each file or member
name, replacing *escape sequences* according to the following
table:

| Escape | Replaced with |
| --- | --- |
| \a | Audible bell (ASCII 7) |
| \b | Backspace (ASCII 8) |
| \f | Form feed (ASCII 12) |
| \n | New line (ASCII 10) |
| \r | Carriage return (ASCII 13) |
| \t | Horizontal tabulation (ASCII 9) |
| \v | Vertical tabulation (ASCII 11) |
| \? | ASCII 127 |
| \n | ASCII n (n should be an octal number of up to 3 digits) |

A backslash followed by any other symbol is retained.

This default behavior is controlled by the following command line
option:

`--unquote`
:   Enable unquoting input file or member names (default).

`--no-unquote`
:   Disable unquoting input file or member names.

If you specify a directory name as a file name argument, all the files
in that directory are operated on by `tar`.

If you do not specify files, `tar` behavior differs depending
on the operation mode as described below:

When `tar` is invoked with `--create` (`-c`),
`tar` will stop immediately, reporting the following:

```
$ tar cf a.tar
tar: Cowardly refusing to create an empty archive
Try 'tar --help' or 'tar --usage' for more information.
```

If you specify either `--list` (`-t`) or
`--extract` (`--get`, `-x`), `tar`
operates on all the archive members in the archive.

If run with `--diff` option, tar will compare the archive with
the contents of the working directory.

If you specify any other operation, `tar` does nothing.

By default, `tar` takes file names from the command line. However,
there are other ways to specify file or member names, or to modify the
manner in which `tar` selects the files or members upon which to
operate. In general, these methods work both for specifying the names
of files and archive members.

---

Next: [Excluding Some Files](#exclude), Previous: [Selecting Archive Members](#Selecting-Archive-Members), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 6.3 Reading Names from a File

Instead of giving the names of files or archive members on the command
line, you can put the names into a file, and then use the
`--files-from=file-of-names` (`-T
file-of-names`) option to `tar`. Give the name of the
file which contains the list of files to include as the argument to
`--files-from`. In the list, the file names should be separated by
newlines. You will frequently use this option when you have generated
the list of files to archive with the `find` utility.

`--files-from=file-name`

`-T file-name`
:   Get names to extract or create from file file-name.

If you give a single dash as a file name for `--files-from`, (i.e.,
you specify either `--files-from=-` or `-T -`), then the file
names are read from standard input.

Unless you are running `tar` with `--create`, you cannot use
both `--files-from=-` and `--file=-` (`-f -`) in the same
command.

Any number of `-T` options can be given in the command line.

The following example shows how to use `find` to generate a list of
files smaller than 400 blocks in length[15](#FOOT15) and put that list into a file
called `small-files`. You can then use the `-T` option to
`tar` to specify the files from that file, `small-files`, to
create the archive `little.tgz`. (The `-z` option to
`tar` compresses the archive with `gzip`; see [Creating and Reading Compressed Archives](#gzip) for
more information.)

```
$ find . -size -400 -print > small-files
$ tar -c -v -z -T small-files -f little.tgz
```

By default, each line read from the file list is first stripped off
any leading and trailing whitespace. If the resulting string begins
with ‘`-`’ character, it is considered a `tar` option and is
processed accordingly[16](#FOOT16). Only a
subset of GNU `tar` options is allowed for use in file lists: the
options described in [Position-Sensitive Options](#Position_002dSensitive-Options), and the
`--add-file` option, discussed below.

For example, the common use of this feature is to change to another
directory by specifying `-C` option:

```
$ cat list
-C/etc
passwd
hosts
-C/lib
libc.a
$ tar -c -f foo.tar --files-from list
```

In this example, `tar` will first switch to `/etc`
directory and add files `passwd` and `hosts` to the
archive. Then it will change to `/lib` directory and will archive
the file `libc.a`. Thus, the resulting archive `foo.tar` will
contain:

```
$ tar tf foo.tar
passwd
hosts
libc.a
```

Note, that any options used in the file list remain in effect for the
rest of the command line. For example, using the same `list`
file as above, the following command

```
$ tar -c -f foo.tar --files-from list libcurses.a
```

will look for file `libcurses.a` in the directory `/lib`,
because it was used with the last `-C` option
(see [Position-Sensitive Options](#Position_002dSensitive-Options)).

If such option handling is undesirable, use the
`--verbatim-files-from` option. When this option is in effect,
each line read from the file list is treated as a file name. Notice,
that this means, in particular, that no whitespace trimming is
performed.

The `--verbatim-files-from` affects all `-T` options
that follow it in the command line. The default behavior can be
restored using `--no-verbatim-files-from` option.

The option `--add-file` is provided to add file names that look
like command-line options, e.g.: `--add-file=--my-file`.

Notice, that options that control file list processing, such as
`--verbatim-files-from` or `--null` won’t affect the
file they appear in. They will affect next `--files-from`
option, if there is any.

* [`NUL`-Terminated File Names](#nul)

---

Up: [Reading Names from a File](#files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 6.3.1 `NUL`-Terminated File Names

The `--null` option causes
`--files-from=file-of-names` (`-T file-of-names`)
to read file names terminated by a `NUL` instead of a newline, so
files whose names contain newlines can be archived using
`--files-from`.

`--null`
:   Only consider `NUL`-terminated file names, instead of files that
    terminate in a newline.

`--no-null`
:   Undo the effect of any previous `--null` option.

The `--null` option is just like the one in GNU
`xargs` and `cpio`, and is useful with the
`-print0` predicate of GNU `find`. In
`tar`, `--null` also disables special handling for
file names that begin with dash (similar to
`--verbatim-files-from` option).

This example shows how to use `find` to generate a list of files
larger than 800 blocks in length and put that list into a file called
`long-files`. The `-print0` option to `find` is just
like `-print`, except that it separates files with a `NUL`
rather than with a newline. You can then run `tar` with both the
`--null` and `-T` options to specify that `tar` gets the
files from that file, `long-files`, to create the archive
`big.tgz`. The `--null` option to `tar` will cause
`tar` to recognize the `NUL` separator between files.

```
$ find . -size +800 -print0 > long-files
$ tar -c -v --null --files-from=long-files --file=big.tar
```

The `--no-null` option can be used if you need to read both
`NUL`-terminated and newline-terminated files on the same command line.
For example, if `flist` is a newline-terminated file, then the
following command can be used to combine it with the above command:

```
$ find . -size +800 -print0 |
  tar -c -f big.tar --null -T - --no-null -T flist
```

This example uses short options for typographic reasons, to avoid
very long lines.

GNU `tar` is tries to automatically detect `NUL`-terminated file
lists, so in many cases it is safe to use them even without the
`--null` option. In this case `tar` will print a
warning and continue reading such a file as if `--null` were
actually given:

```
$ find . -size +800 -print0 | tar -c -f big.tar -T -
tar: -: file name read contains nul character
```

The null terminator, however, remains in effect only for this
particular file, any following `-T` options will assume
newline termination. Of course, the null autodetection applies
to these eventual surplus `-T` options as well.

---

Next: [Wildcards Patterns and Matching](#wildcards), Previous: [Reading Names from a File](#files), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]
