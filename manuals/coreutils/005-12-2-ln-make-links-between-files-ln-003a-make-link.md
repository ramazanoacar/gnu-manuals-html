### 12.2 `ln`: Make links between files [¶](#ln_003a-Make-links-between-files)

`ln` makes links between files. By default, it makes hard links;
with the `-s` option, it makes symbolic (or *soft*) links.
Synopses:

```
ln [option]... [-T] target linkname
ln [option]... target
ln [option]... target... directory
ln [option]... -t directory target...
```

* If two file names are given, `ln` creates a link to the first
  file from the second.
* If one target is given, `ln` creates a link to that file
  in the current directory.
* If the `--target-directory` (`-t`) option is given, or
  failing that if the last file is a directory and the
  `--no-target-directory` (`-T`) option is not given,
  `ln` creates a link to each target file in the specified
  directory, using the targets’ names.

Normally `ln` does not replace existing files. Use the
`--force` (`-f`) option to replace them unconditionally,
the `--interactive` (`-i`) option to replace them
conditionally, and the `--backup` (`-b`) option to
rename them. Unless the `--backup` (`-b`) option is
used there is no brief moment when the destination does not exist;
this is an extension to POSIX.

A *hard link* is another name for an existing file; the link and the
original are indistinguishable. Technically speaking, they share the
same inode, and the inode contains all the information about a
file – indeed, it is not incorrect to say that the inode *is* the
file. Most systems prohibit making a hard link to
a directory; on those where it is allowed, only the super-user can do
so (and with caution, since creating a cycle will cause problems to many
other utilities). Hard links cannot cross file system boundaries. (These
restrictions are not mandated by POSIX, however.)

*Symbolic links* (*symlinks* for short), on the other hand, are
a special file type (which not all kernels support: System V release 3
(and older) systems lack symlinks) in which the link file actually
refers to a different file, by name. When most operations (opening,
reading, writing, and so on) are passed the symbolic link file, the
kernel automatically *dereferences* the link and operates on the
target of the link. But some operations (e.g., removing) work on the
link file itself, rather than on its target. The owner and group of a
symlink are not significant to file access performed through
the link, but do have implications on deleting a symbolic link from a
directory with the restricted deletion bit set. On the GNU system,
the mode of a symlink has no significance and cannot be changed, but
on some BSD systems, the mode can be changed and will affect whether
the symlink will be traversed in file name resolution. See [Symbolic Links](https://www.gnu.org/software/libc/manual/html_mono/libc.html#Symbolic-Links) in The GNU C Library Reference Manual.

Symbolic links can contain arbitrary strings; a *dangling symlink*
occurs when the string in the symlink does not resolve to a file.
There are no restrictions against creating dangling symbolic links.
There are trade-offs to using absolute or relative symlinks. An
absolute symlink always points to the same file, even if the directory
containing the link is moved. However, if the symlink is visible from
more than one machine (such as on a networked file system), the file
pointed to might not always be the same. A relative symbolic link is
resolved in relation to the directory that contains the link, and is
often useful in referring to files on the same device without regards
to what name that device is mounted on when accessed via networked
machines.

When creating a relative symlink in a different location than the
current directory, the resolution of the symlink will be different
than the resolution of the same string from the current directory.
Therefore, many users prefer to first change directories to the
location where the relative symlink will be created, so that
tab-completion or other file resolution will find the same target as
what will be placed in the symlink.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-b`’ [¶](#index-_002db-17)

‘`--backup[=method]`’
:   See [Backup options](#Backup-options).
    Make a backup of each file that would otherwise be overwritten or removed.

‘`-d`’ [¶](#index-_002dd-14)

‘`-F`’

‘`--directory`’
:   Allow users with appropriate privileges to attempt to make hard links
    to directories.
    However, this will probably fail due to
    system restrictions, even for the super-user.

‘`-f`’ [¶](#index-_002df-14)

‘`--force`’
:   Remove existing destination files.

‘`-i`’ [¶](#index-_002di-14)

‘`--interactive`’
:   Prompt whether to remove existing destination files,
    and fail if the response is not affirmative.

‘`-L`’ [¶](#index-_002dL-4)

‘`--logical`’
:   If `-s` is not in effect, and the source file is a symbolic
    link, create the hard link to the file referred to by the symbolic
    link, rather than the symbolic link itself.

‘`-n`’ [¶](#index-_002dn-13)

‘`--no-dereference`’
:   Do not treat the last operand specially when it is a symbolic link to
    a directory. Instead, treat it as if it were a normal file.

    When the destination is an actual directory (not a symlink to one),
    there is no ambiguity. The link is created in that directory.
    But when the specified destination is a symlink to a directory,
    there are two ways to treat the user’s request. `ln` can
    treat the destination just as it would a normal directory and create
    the link in it. On the other hand, the destination can be viewed as a
    non-directory – as the symlink itself. In that case, `ln`
    must delete or backup that symlink before creating the new link.
    The default is to treat a destination that is a symlink to a directory
    just like a directory.

    This option is weaker than the `--no-target-directory`
    (`-T`) option, so it has no effect if both options are given.

‘`-P`’ [¶](#index-_002dP-2)

‘`--physical`’
:   If `-s` is not in effect, and the source file is a symbolic
    link, create the hard link to the symbolic link itself. On platforms
    where this is not supported by the kernel, this option creates a
    symbolic link with identical contents; since symbolic link contents
    cannot be edited, any file name resolution performed through either
    link will be the same as if a hard link had been created.

‘`-r`’ [¶](#index-_002dr-9)

‘`--relative`’
:   Make symbolic links relative to the link location.
    This option is only valid with the `--symbolic` option.

    Example:

    ```
    ln -srv /a/file /tmp
    '/tmp/file' -> '../a/file'
    ```

    Relative symbolic links are generated based on their canonicalized
    containing directory, and canonicalized targets. I.e., all symbolic
    links in these file names will be resolved.
    See [`realpath`: Print the resolved file name.](#realpath-invocation), which gives greater control
    over relative file name generation, as demonstrated in the following example:

    ```
    ln--relative() {
      test "$1" = --no-symlinks && { nosym=$1; shift; }
      target="$1";
      test -d "$2" && link="$2/." || link="$2"
      rtarget="$(realpath $nosym -m "$target" \
                  --relative-to "$(dirname "$link")")"
      ln -s -v "$rtarget" "$link"
    }
    ```

‘`-s`’ [¶](#index-_002ds-18)

‘`--symbolic`’
:   Make symbolic links instead of hard links. This option merely produces
    an error message on systems that do not support symbolic links.

‘`-S suffix`’ [¶](#index-_002dS-9)

‘`--suffix=suffix`’
:   Append suffix to each backup file made with `-b`.
    See [Backup options](#Backup-options).

‘`-t directory`’ [¶](#index-_002dt-14)

‘`--target-directory=directory`’
:   Specify the destination directory.
    See [Target directory](#Target-directory).

‘`-T`’ [¶](#index-_002dT-8)

‘`--no-target-directory`’
:   Do not treat the last operand specially when it is a directory or a
    symbolic link to a directory. See [Target directory](#Target-directory).

‘`-v`’ [¶](#index-_002dv-12)

‘`--verbose`’
:   Print the name of each file after linking it successfully.

If `-L` and `-P` are both given, the last one takes
precedence. If `-s` is also given, `-L` and `-P`
are silently ignored. If neither option is given, then this
implementation defaults to `-P` if the system `link` supports
hard links to symbolic links (such as the GNU system), and `-L`
if `link` follows symbolic links (such as on BSD).

An exit status of zero indicates success,
and a nonzero value indicates failure.

Examples:

```
Bad Example:

# Create link ../a pointing to a in that directory.
# Not really useful because it points to itself.
ln -s a ..

Better Example:

# Change to the target before creating symlinks to avoid being confused.
cd ..
ln -s adir/a .

Bad Example:

# Hard coded file names don't move well.
ln -s $(pwd)/a /some/dir/

Better Example:

# Relative file names survive directory moves and also
# work across networked file systems.
ln -s afile anotherfile
ln -s ../adir/afile yetanotherfile
```

---

Next: [`mkfifo`: Make FIFOs (named pipes)](#mkfifo-invocation), Previous: [`ln`: Make links between files](#ln-invocation), Up: [Special file types](#Special-file-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 12.3 `mkdir`: Make directories [¶](#mkdir_003a-Make-directories)

`mkdir` creates directories with the specified names. Synopsis:

```
mkdir [option]... name...
```

`mkdir` creates each directory name in the order given.
It reports an error if name already exists, unless the
`-p` option is given and name is a directory.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-m mode`’ [¶](#index-_002dm-5)

‘`--mode=mode`’
:   Set the file permission bits of created directories to mode,
    which uses the same syntax as
    in `chmod` and uses ‘`a=rwx`’ (read, write and execute allowed for
    everyone) for the point of the departure. See [File permissions](#File-permissions).
    This option affects only directories given on the command line;
    it does not affect any parents that may be created via the `-p` option.

    Normally the directory has the desired file mode bits at the moment it
    is created. As a GNU extension, mode may also mention
    special mode bits, but in this case there may be a temporary window
    during which the directory exists but its special mode bits are
    incorrect. See [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid), for how the
    set-user-ID and set-group-ID bits of directories are inherited unless
    overridden in this way.

‘`-p`’ [¶](#index-_002dp-5)

‘`--parents`’
:   Make any missing parent directories for each argument, setting their
    file permission bits to ‘`=rwx,u+wx`’,
    that is, with the umask modified by ‘`u+wx`’. Ignore
    existing parent directories, and do not change their file permission
    bits.

    If the `-m` option is also given, it does not affect
    file permission bits of any newly-created parent directories.
    To control these bits, set the
    umask before invoking `mkdir`. For example, if the shell
    command ‘`(umask u=rwx,go=rx; mkdir -p P/Q)`’ creates the parent
    `P` it sets the parent’s file permission bits to ‘`u=rwx,go=rx`’.
    (The umask must include ‘`u=wx`’ for this method to work.)
    To set a parent’s special mode bits as well, you can invoke
    `chmod` after `mkdir`. See [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid), for how the set-user-ID and set-group-ID bits of
    newly-created parent directories are inherited.

‘`-v`’ [¶](#index-_002dv-13)

‘`--verbose`’
:   Print a message for each created directory. This is most useful with
    `--parents`.

‘`-Z`’ [¶](#index-_002dZ-4)

‘`--context[=context]`’
:   Without a specified context, adjust the SELinux security context according
    to the system default type for destination files, similarly to the
    `restorecon` command.
    The long form of this option with a specific context specified,
    will set the context for newly created files only.
    With a specified context, if both SELinux and SMACK are disabled, a warning is
    issued.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`mknod`: Make block or character special files](#mknod-invocation), Previous: [`mkdir`: Make directories](#mkdir-invocation), Up: [Special file types](#Special-file-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 12.4 `mkfifo`: Make FIFOs (named pipes) [¶](#mkfifo_003a-Make-FIFOs-_0028named-pipes_0029)

`mkfifo` creates FIFOs (also called *named pipes*) with the
specified names. Synopsis:

```
mkfifo [option] name...
```

A *FIFO* is a special file type that permits independent processes
to communicate. One process opens the FIFO file for writing, and
another for reading, after which data can flow as with the usual
anonymous pipe in shells or elsewhere.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-m mode`’ [¶](#index-_002dm-6)

‘`--mode=mode`’
:   Set the mode of created FIFOs to mode, which is symbolic as in
    `chmod` and uses ‘`a=rw`’ (read and write allowed for everyone)
    for the point of departure. mode should specify only file
    permission bits. See [File permissions](#File-permissions).

‘`-Z`’ [¶](#index-_002dZ-5)

‘`--context[=context]`’
:   Without a specified context, adjust the SELinux security context according
    to the system default type for destination files, similarly to the
    `restorecon` command.
    The long form of this option with a specific context specified,
    will set the context for newly created files only.
    With a specified context, if both SELinux and SMACK are disabled, a warning is
    issued.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`readlink`: Print value of a symlink or canonical file name](#readlink-invocation), Previous: [`mkfifo`: Make FIFOs (named pipes)](#mkfifo-invocation), Up: [Special file types](#Special-file-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 12.5 `mknod`: Make block or character special files [¶](#mknod_003a-Make-block-or-character-special-files)

`mknod` creates a FIFO, character special file, or block special
file with the specified name. Synopsis:

```
mknod [option]... name type [major minor]
```

Unlike the phrase “special file type” above, the term *special
file* has a technical meaning on Unix: something that can generate or
receive data. Usually this corresponds to a physical piece of hardware,
e.g., a printer or a flash drive. (These files are typically created at
system-configuration time.) The `mknod` command is what creates
files of this type. Such devices can be read either a character at a
time or a “block” (many characters) at a time, hence we say there are
*block special* files and *character special* files.

Due to shell aliases and built-in `mknod` functions, using an
unadorned `mknod` interactively or in a script may get you
different functionality than that described here. Invoke it via
`env` (i.e., `env mknod …`) to avoid interference
from the shell.

The arguments after name specify the type of file to make:

‘`p`’ [¶](#index-p-for-FIFO-file)
:   for a FIFO

‘`b`’ [¶](#index-b-for-block-special-file)
:   for a block special file

‘`c`’
:   for a character special file

When making a block or character special file, the major and minor
device numbers must be given after the file type.
If a major or minor device number begins with ‘`0x`’ or ‘`0X`’,
it is interpreted as hexadecimal; otherwise, if it begins with ‘`0`’,
as octal; otherwise, as decimal.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-m mode`’ [¶](#index-_002dm-7)

‘`--mode=mode`’
:   Set the mode of created files to mode, which is symbolic as in
    `chmod` and uses ‘`a=rw`’ as the point of departure.
    mode should specify only file permission bits.
    See [File permissions](#File-permissions).

‘`-Z`’ [¶](#index-_002dZ-6)

‘`--context[=context]`’
:   Without a specified context, adjust the SELinux security context according
    to the system default type for destination files, similarly to the
    `restorecon` command.
    The long form of this option with a specific context specified,
    will set the context for newly created files only.
    With a specified context, if both SELinux and SMACK are disabled, a warning is
    issued.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`rmdir`: Remove empty directories](#rmdir-invocation), Previous: [`mknod`: Make block or character special files](#mknod-invocation), Up: [Special file types](#Special-file-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 12.6 `readlink`: Print value of a symlink or canonical file name [¶](#readlink_003a-Print-value-of-a-symlink-or-canonical-file-name)

`readlink` may work in one of two supported modes:

‘`Readlink mode`’
:   `readlink` outputs the value of the given symbolic links.
    If `readlink` is invoked with an argument other than the name
    of a symbolic link, it produces no output and exits with a nonzero exit code.

‘`Canonicalize mode`’
:   `readlink` outputs the absolute name of the given files which contain
    no `.`, `..` components nor any repeated separators
    (`/`) or symbolic links. The `realpath` command is the
    preferred command to use for canonicalization. See [`realpath`: Print the resolved file name.](#realpath-invocation).

```
readlink [option]... file...
```

By default, `readlink` operates in readlink mode.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-f`’ [¶](#index-_002df-15)

‘`--canonicalize`’
:   Activate canonicalize mode.
    If any component of the file name except the last one is missing or unavailable,
    `readlink` produces no output and exits with a nonzero exit
    code. A trailing slash is ignored.

‘`-e`’ [¶](#index-_002de-4)

‘`--canonicalize-existing`’
:   Activate canonicalize mode.
    If any component is missing or unavailable, `readlink` produces
    no output and exits with a nonzero exit code. A trailing slash
    requires that the name resolve to a directory.

‘`-m`’ [¶](#index-_002dm-8)

‘`--canonicalize-missing`’
:   Activate canonicalize mode.
    If any component is missing or unavailable, `readlink` treats it
    as a directory.

‘`-n`’ [¶](#index-_002dn-14)

‘`--no-newline`’
:   Do not print the output delimiter, when a single file is specified.
    Print a warning if specified along with multiple files.

‘`-s`’ [¶](#index-_002ds-19)

‘`-q`’

‘`--silent`’

‘`--quiet`’
:   Suppress most error messages. On by default.

‘`-v`’ [¶](#index-_002dv-14)

‘`--verbose`’
:   Report error messages.

‘`-z`’ [¶](#index-_002dz-12)

‘`--zero`’
:   Output a zero byte (ASCII NUL) at the end of each line,
    rather than a newline. This option enables other programs to parse the
    output even when that output would contain data with embedded newlines.

The `readlink` utility first appeared in OpenBSD 2.1.

The `realpath` command without options, operates like
`readlink` in canonicalize mode.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`unlink`: Remove files via the unlink syscall](#unlink-invocation), Previous: [`readlink`: Print value of a symlink or canonical file name](#readlink-invocation), Up: [Special file types](#Special-file-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 12.7 `rmdir`: Remove empty directories [¶](#rmdir_003a-Remove-empty-directories)

`rmdir` removes empty directories. Synopsis:

```
rmdir [option]... directory...
```

If any directory argument does not refer to an existing empty
directory, it is an error.

The program accepts the following options. Also see [Common options](#Common-options).

‘`--ignore-fail-on-non-empty`’ [¶](#index-_002d_002dignore_002dfail_002don_002dnon_002dempty)
:   Ignore each failure to remove a directory that is non-empty.

‘`-p`’ [¶](#index-_002dp-6)

‘`--parents`’
:   Remove directory, then try to remove each component of directory.
    So, for example, ‘`rmdir -p a/b/c`’ is similar to ‘`rmdir a/b/c a/b a`’.
    As such, it fails if any of those directories turns out not to be empty.
    Use the `--ignore-fail-on-non-empty` option to make it so such
    a failure does not evoke a diagnostic and does not cause `rmdir` to
    exit unsuccessfully.

‘`-v`’ [¶](#index-_002dv-15)

‘`--verbose`’
:   Give a diagnostic for each successful removal.
    directory is removed.

See [`rm`: Remove files or directories](#rm-invocation), for how to remove non-empty directories recursively.

To remove all empty directories under dirname, including
directories that become empty because other directories are removed,
you can use either of the following commands:

```
# This uses GNU extensions.
find dirname -type d -empty -delete

# This runs on any POSIX platform.
find dirname -depth -type d -exec rmdir {} +
```

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`rmdir`: Remove empty directories](#rmdir-invocation), Up: [Special file types](#Special-file-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 12.8 `unlink`: Remove files via the unlink syscall [¶](#unlink_003a-Remove-files-via-the-unlink-syscall)

`unlink` deletes a single specified file name.
It is a minimalist interface to the system-provided
`unlink` function. See [Deleting Files](https://www.gnu.org/software/libc/manual/html_mono/libc.html#Deleting-Files) in The GNU C Library Reference Manual. Synopsis:
It avoids the bells and whistles of the more commonly-used
`rm` command (see [`rm`: Remove files or directories](#rm-invocation)).

```
unlink filename
```

On some systems `unlink` can be used to delete the name of a
directory. On others, it can be used that way only by a privileged user.
In the GNU system `unlink` can never delete the name of a directory.

The `unlink` command honors the `--help` and
`--version` options. To remove a file whose name begins with
‘`-`’, prefix the name with ‘`./`’, e.g., ‘`unlink ./--help`’.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [File space usage](#File-space-usage), Previous: [Special file types](#Special-file-types), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 13 Changing file attributes [¶](#Changing-file-attributes-1)

A file is not merely its contents, a name, and a file type
(see [Special file types](#Special-file-types)). A file also has an owner (a user ID), a
group (a group ID), permissions (what the owner can do with the file,
what people in the group can do, and what everyone else can do), various
timestamps, and other information. Collectively, we call these a file’s
*attributes*.

These commands change file attributes.

* [`chown`: Change file owner and group](#chown-invocation)
* [`chgrp`: Change group ownership](#chgrp-invocation)
* [`chmod`: Change access permissions](#chmod-invocation)
* [`touch`: Change file timestamps](#touch-invocation)

---

Next: [`chgrp`: Change group ownership](#chgrp-invocation), Up: [Changing file attributes](#Changing-file-attributes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 13.1 `chown`: Change file owner and group [¶](#chown_003a-Change-file-owner-and-group)

`chown` changes the user and/or group ownership of each given file
to new-owner or to the user and group of an existing reference file.
Synopsis:

```
chown [option]... {new-owner | --reference=ref_file} file...
```

If used, new-owner specifies the new owner and/or group as follows
(with no embedded white space):

```
[owner] [ : [group] ]
```

Specifically:

owner
:   If only an owner (a user name or numeric user ID) is given, that
    user is made the owner of each given file, and the files’ group is not
    changed.

owner‘`:`’group
:   If the owner is followed by a colon and a group (a
    group name or numeric group ID), with no spaces between them, the group
    ownership of the files is changed as well (to group).

owner‘`:`’
:   If a colon but no group name follows owner, that user is
    made the owner of the files and the group of the files is changed to
    owner’s login group.

‘`:`’group
:   If the colon and following group are given, but the owner
    is omitted, only the group of the files is changed; in this case,
    `chown` performs the same function as `chgrp`.

‘`:`’
:   If only a colon is given, or if new-owner is empty, neither the
    owner nor the group is changed.

If owner or group is intended to represent a numeric user
or group ID, then you may specify it with a leading ‘`+`’.
See [chown, chgrp, chroot, id: Disambiguating user names and IDs](#Disambiguating-names-and-IDs).

Some older scripts may still use ‘`.`’ in place of the ‘`:`’ separator.
POSIX 1003.1-2001 (see [Standards conformance](#Standards-conformance)) does not
require support for that, but for backward compatibility GNU
`chown` supports ‘`.`’ so long as no ambiguity results,
although it issues a warning and support may be removed in future versions.
New scripts should avoid the use of ‘`.`’ because it is not
portable, and because it has undesirable results if the entire
owner‘`.`’group happens to identify a user whose name
contains ‘`.`’.

It is system dependent whether a user can change the group to an arbitrary one,
or the more portable behavior of being restricted to setting a group of
which the user is a member.

The `chown` command sometimes clears the set-user-ID or
set-group-ID permission bits. This behavior depends on the policy and
functionality of the underlying `chown` system call, which may
make system-dependent file mode modifications outside the control of
the `chown` command. For example, the `chown` command
might not affect those bits when invoked by a user with appropriate
privileges, or when the
bits signify some function other than executable permission (e.g.,
mandatory locking).
When in doubt, check the underlying system behavior.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-c`’ [¶](#index-_002dc-15)

‘`--changes`’
:   Verbosely describe the action for each file whose ownership
    actually changes.

‘`-f`’ [¶](#index-_002df-16)

‘`--silent`’

‘`--quiet`’
:   Do not print error messages about files whose ownership cannot be
    changed.

‘`--from=old-owner`’ [¶](#index-_002d_002dfrom)
:   Change a file’s ownership only if it has current attributes specified
    by old-owner. old-owner has the same form as new-owner
    described above.
    This option is useful primarily from a security standpoint in that
    it narrows considerably the window of potential abuse.
    For example, to reflect a user ID numbering change for one user’s files
    without an option like this, `root` might run

    ```
    find / -owner OLDUSER -print0 | xargs -0 chown -h NEWUSER
    ```

    But that is dangerous because the interval between when the `find`
    tests the existing file’s owner and when the `chown` is actually run
    may be quite large.
    One way to narrow the gap would be to invoke chown for each file
    as it is found:

    ```
    find / -owner OLDUSER -exec chown -h NEWUSER {} \;
    ```

    But that is very slow if there are many affected files.
    With this option, it is safer (the gap is narrower still)
    though still not perfect:

    ```
    chown -h -R --from=OLDUSER NEWUSER /
    ```

‘`--dereference`’ [¶](#index-_002d_002ddereference-2)
:   Do not act on symbolic links themselves but rather on what they point to.
    This is the default when not operating recursively.

    Combining this dereferencing option with the `--recursive` option
    may create a security risk:
    During the traversal of the directory tree, an attacker may be able to
    introduce a symlink to an arbitrary target; when the tool reaches that,
    the operation will be performed on the target of that symlink,
    possibly allowing the attacker to escalate privileges.

‘`-h`’ [¶](#index-_002dh-5)

‘`--no-dereference`’
:   Act on symbolic links themselves instead of what they point to.
    This mode relies on the `lchown` system call.
    On systems that do not provide the `lchown` system call,
    no diagnostic is issued, but see `--verbose`.

‘`--preserve-root`’ [¶](#index-_002d_002dpreserve_002droot-1)
:   Fail upon any attempt to recursively change the root directory, `/`.
    Without `--recursive`, this option has no effect.
    See [Treating `/` specially](#Treating-_002f-specially).

‘`--no-preserve-root`’ [¶](#index-_002d_002dno_002dpreserve_002droot-1)
:   Cancel the effect of any preceding `--preserve-root` option.
    See [Treating `/` specially](#Treating-_002f-specially).

‘`--reference=ref_file`’ [¶](#index-_002d_002dreference)
:   Change the user and group of each file to be the same as those of
    ref\_file. If ref\_file is a symbolic link, do not use the
    user and group of the symbolic link, but rather those of the file it
    refers to.

‘`-v`’ [¶](#index-_002dv-16)

‘`--verbose`’
:   Output a diagnostic for every file processed.
    If a symbolic link is encountered during a recursive traversal
    on a system without the `lchown` system call, and `--no-dereference`
    is in effect, then issue a diagnostic saying neither the symbolic link nor
    its referent is being changed.

‘`-R`’ [¶](#index-_002dR-5)

‘`--recursive`’
:   Recursively change ownership of directories and their contents.

‘`-H`’ [¶](#index-_002dH-3)
:   If `--recursive` (`-R`) is specified and
    a command line argument is a symbolic link to a directory, traverse it.
    See [Traversing symlinks](#Traversing-symlinks).

‘`-L`’ [¶](#index-_002dL-5)
:   In a recursive traversal, traverse every symbolic link to a directory
    that is encountered.

    Combining this dereferencing option with the `--recursive` option
    may create a security risk:
    During the traversal of the directory tree, an attacker may be able to
    introduce a symlink to an arbitrary target; when the tool reaches that,
    the operation will be performed on the target of that symlink,
    possibly allowing the attacker to escalate privileges.

    See [Traversing symlinks](#Traversing-symlinks).

‘`-P`’ [¶](#index-_002dP-3)
:   Do not traverse any symbolic links.
    This is the default if none of `-H`, `-L`,
    or `-P` is specified.
    See [Traversing symlinks](#Traversing-symlinks).

An exit status of zero indicates success,
and a nonzero value indicates failure.

Examples:

```
# Change the owner of /u to "root".
chown root /u

# Likewise, but also change its group to "staff".
chown root:staff /u

# Change the owner of /u and subfiles to "root".
chown -hR root /u
```

---

Next: [`chmod`: Change access permissions](#chmod-invocation), Previous: [`chown`: Change file owner and group](#chown-invocation), Up: [Changing file attributes](#Changing-file-attributes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 13.2 `chgrp`: Change group ownership [¶](#chgrp_003a-Change-group-ownership)

`chgrp` changes the group ownership of each given file
to group (which can be either a group name or a numeric group ID)
or to the group of an existing reference file. See [`chown`: Change file owner and group](#chown-invocation).
Synopsis:

```
chgrp [option]... {group | --reference=ref_file} file...
```

If group is intended to represent a
numeric group ID, then you may specify it with a leading ‘`+`’.
See [chown, chgrp, chroot, id: Disambiguating user names and IDs](#Disambiguating-names-and-IDs).

It is system dependent whether a user can change the group to an arbitrary one,
or the more portable behavior of being restricted to setting a group of
which the user is a member.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-c`’ [¶](#index-_002dc-16)

‘`--changes`’
:   Verbosely describe the action for each file whose group actually
    changes.

‘`-f`’ [¶](#index-_002df-17)

‘`--silent`’

‘`--quiet`’
:   Do not print error messages about files whose group cannot be
    changed.

‘`--from=old-owner`’ [¶](#index-_002d_002dfrom-1)
:   Change a file’s ownership only if it has current attributes specified
    by old-owner. old-owner has the same form as new-owner
    described above.
    This option is useful primarily from a security standpoint in that
    it narrows considerably the window of potential abuse.
    For example, to reflect a user ID numbering change for one user’s files
    without an option like this, `root` might run

    ```
    find / -owner OLDUSER -print0 | xargs -0 chgrp -h NEWUSER
    ```

    But that is dangerous because the interval between when the `find`
    tests the existing file’s owner and when the `chgrp` is actually run
    may be quite large.
    One way to narrow the gap would be to invoke chgrp for each file
    as it is found:

    ```
    find / -owner OLDUSER -exec chgrp -h NEWUSER {} \;
    ```

    But that is very slow if there are many affected files.
    With this option, it is safer (the gap is narrower still)
    though still not perfect:

    ```
    chgrp -h -R --from=OLDUSER NEWUSER /
    ```

‘`--dereference`’ [¶](#index-_002d_002ddereference-3)
:   Do not act on symbolic links themselves but rather on what they point to.
    This is the default when not operating recursively.

    Combining this dereferencing option with the `--recursive` option
    may create a security risk:
    During the traversal of the directory tree, an attacker may be able to
    introduce a symlink to an arbitrary target; when the tool reaches that,
    the operation will be performed on the target of that symlink,
    possibly allowing the attacker to escalate privileges.

‘`-h`’ [¶](#index-_002dh-6)

‘`--no-dereference`’
:   Act on symbolic links themselves instead of what they point to.
    This mode relies on the `lchown` system call.
    On systems that do not provide the `lchown` system call,
    no diagnostic is issued, but see `--verbose`.

‘`--preserve-root`’ [¶](#index-_002d_002dpreserve_002droot-2)
:   Fail upon any attempt to recursively change the root directory, `/`.
    Without `--recursive`, this option has no effect.
    See [Treating `/` specially](#Treating-_002f-specially).

‘`--no-preserve-root`’ [¶](#index-_002d_002dno_002dpreserve_002droot-2)
:   Cancel the effect of any preceding `--preserve-root` option.
    See [Treating `/` specially](#Treating-_002f-specially).

‘`--reference=ref_file`’ [¶](#index-_002d_002dreference-1)
:   Change the group of each file to be the same as that of
    ref\_file. If ref\_file is a symbolic link, do not use the
    group of the symbolic link, but rather that of the file it refers to.

‘`-v`’ [¶](#index-_002dv-17)

‘`--verbose`’
:   Output a diagnostic for every file processed.
    If a symbolic link is encountered during a recursive traversal
    on a system without the `lchown` system call, and `--no-dereference`
    is in effect, then issue a diagnostic saying neither the symbolic link nor
    its referent is being changed.

‘`-R`’ [¶](#index-_002dR-6)

‘`--recursive`’
:   Recursively change the group ownership of directories and their contents.

‘`-H`’ [¶](#index-_002dH-4)
:   If `--recursive` (`-R`) is specified and
    a command line argument is a symbolic link to a directory, traverse it.
    See [Traversing symlinks](#Traversing-symlinks).

‘`-L`’ [¶](#index-_002dL-6)
:   In a recursive traversal, traverse every symbolic link to a directory
    that is encountered.

    Combining this dereferencing option with the `--recursive` option
    may create a security risk:
    During the traversal of the directory tree, an attacker may be able to
    introduce a symlink to an arbitrary target; when the tool reaches that,
    the operation will be performed on the target of that symlink,
    possibly allowing the attacker to escalate privileges.

    See [Traversing symlinks](#Traversing-symlinks).

‘`-P`’ [¶](#index-_002dP-4)
:   Do not traverse any symbolic links.
    This is the default if none of `-H`, `-L`,
    or `-P` is specified.
    See [Traversing symlinks](#Traversing-symlinks).

An exit status of zero indicates success,
and a nonzero value indicates failure.

Examples:

```
# Change the group of /u to "staff".
chgrp staff /u

# Change the group of /u and subfiles to "staff".
chgrp -hR staff /u
```

---

Next: [`touch`: Change file timestamps](#touch-invocation), Previous: [`chgrp`: Change group ownership](#chgrp-invocation), Up: [Changing file attributes](#Changing-file-attributes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 13.3 `chmod`: Change access permissions [¶](#chmod_003a-Change-access-permissions)

`chmod` changes the access permissions of the named files. Synopsis:

```
chmod [option]... {mode | --reference=ref_file} file...
```

`chmod` doesn’t change the permissions of symbolic links, since
the `chmod` system call cannot change their permissions on most systems,
and most systems ignore permissions of symbolic links.
However, for each symbolic link listed on the command
line, `chmod` changes the permissions of the pointed-to file.
In contrast, `chmod` ignores symbolic links encountered during
recursive directory traversals. Options that modify this behavior
are described below.

Only a process whose effective user ID matches the user ID of the file,
or a process with appropriate privileges, is permitted to change the
file mode bits of a file.

A successful use of `chmod` clears the set-group-ID bit of a
regular file if the file’s group ID does not match the user’s
effective group ID or one of the user’s supplementary group IDs,
unless the user has appropriate privileges. Additional restrictions
may cause the set-user-ID and set-group-ID bits of mode or
ref\_file to be ignored. This behavior depends on the policy and
functionality of the underlying `chmod` system call. When in
doubt, check the underlying system behavior.

If used, mode specifies the new file mode bits.
For details, see the section on [File permissions](#File-permissions).
If you really want mode to have a leading ‘`-`’, you should
use `--` first, e.g., ‘`chmod -- -w file`’. Typically,
though, ‘`chmod a-w file`’ is preferable, and `chmod -w
file` (without the `--`) complains if it behaves differently
from what ‘`chmod a-w file`’ would do.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-c`’ [¶](#index-_002dc-17)

‘`--changes`’
:   Verbosely describe the action for each file whose permissions
    actually change.

‘`--dereference`’ [¶](#index-_002d_002ddereference-4)
:   Do not act on symbolic links themselves but rather on what they point to.
    This is the default for command line arguments, but not for
    symbolic links encountered when recursing.

    Combining this dereferencing option with the `--recursive` option
    may create a security risk:
    During the traversal of the directory tree, an attacker may be able to
    introduce a symlink to an arbitrary target; when the tool reaches that,
    the operation will be performed on the target of that symlink,
    possibly allowing the attacker to escalate privileges.

‘`-h`’ [¶](#index-_002dh-7)

‘`--no-dereference`’
:   Act on symbolic links themselves instead of what they point to.
    On systems that do not support this, no diagnostic is issued,
    but see `--verbose`.

‘`-f`’ [¶](#index-_002df-18)

‘`--silent`’

‘`--quiet`’
:   Do not print error messages about files whose permissions cannot be
    changed.

‘`--preserve-root`’ [¶](#index-_002d_002dpreserve_002droot-3)
:   Fail upon any attempt to recursively change the root directory, `/`.
    Without `--recursive`, this option has no effect.
    See [Treating `/` specially](#Treating-_002f-specially).

‘`--no-preserve-root`’ [¶](#index-_002d_002dno_002dpreserve_002droot-3)
:   Cancel the effect of any preceding `--preserve-root` option.
    See [Treating `/` specially](#Treating-_002f-specially).

‘`-v`’ [¶](#index-_002dv-18)

‘`--verbose`’
:   Verbosely describe the action or non-action taken for every file.

‘`--reference=ref_file`’ [¶](#index-_002d_002dreference-2)
:   Change the mode of each file to be the same as that of ref\_file.
    See [File permissions](#File-permissions).
    If ref\_file is a symbolic link, do not use the mode
    of the symbolic link, but rather that of the file it refers to.

‘`-R`’ [¶](#index-_002dR-7)

‘`--recursive`’
:   Recursively change permissions of directories and their contents.

‘`-H`’ [¶](#index-_002dH-5)
:   If `--recursive` (`-R`) is specified and
    a command line argument is a symbolic link to a directory, traverse it.
    This is the default if none of `-H`, `-L`,
    or `-P` is specified.
    See [Traversing symlinks](#Traversing-symlinks).

‘`-L`’ [¶](#index-_002dL-7)
:   In a recursive traversal, traverse every symbolic link to a directory
    that is encountered.

    Combining this dereferencing option with the `--recursive` option
    may create a security risk:
    During the traversal of the directory tree, an attacker may be able to
    introduce a symlink to an arbitrary target; when the tool reaches that,
    the operation will be performed on the target of that symlink,
    possibly allowing the attacker to escalate privileges.

    See [Traversing symlinks](#Traversing-symlinks).

‘`-P`’ [¶](#index-_002dP-5)
:   Do not traverse any symbolic links.
    See [Traversing symlinks](#Traversing-symlinks).

An exit status of zero indicates success,
and a nonzero value indicates failure.

Examples:

```
# Change file permissions of FOO to be world readable
# and user writable, with no other permissions.
chmod 644 foo
chmod a=r,u+w foo

# Add user and group execute permissions to FOO.
chmod +110 file
chmod ug+x file

# Set file permissions of DIR and subsidiary files to
# be the umask default, assuming execute permissions for
# directories and for files already executable.
chmod -R a=,+rwX dir
```

---

Previous: [`chmod`: Change access permissions](#chmod-invocation), Up: [Changing file attributes](#Changing-file-attributes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 13.4 `touch`: Change file timestamps [¶](#touch_003a-Change-file-timestamps)

`touch` changes the access and/or modification timestamps of the
specified files. Synopsis:

```
touch [option]... file...
```

Any file argument that does not exist is created empty, unless
option `--no-create` (`-c`) or `--no-dereference`
(`-h`) was in effect.

A file argument string of ‘`-`’ is handled specially and
causes `touch` to change the times of the file associated with
standard output.

By default, `touch` sets file timestamps to the current time.
Because `touch` acts on its operands left to right, the
resulting timestamps of earlier and later operands may disagree.

When setting file timestamps to the current time, `touch` can
change the timestamps for files that the user does not own but has
write permission for. Otherwise, the user must own the files. Some
older systems have a further restriction: the user must own the files
unless both the access and modification timestamps are being set to the
current time.

The `touch` command cannot set a file’s status change timestamp to
a user-specified value, and cannot change the file’s birth time (if
supported) at all. Also, `touch` has issues similar to those
affecting all programs that update file timestamps. For example,
`touch` may set a file’s timestamp to a value that differs
slightly from the requested time. See [File timestamps](#File-timestamps).

Timestamps assume the time zone rules specified by the `TZ`
environment variable, or by the system default rules if `TZ` is
not set. See [Specifying the Time Zone with `TZ`](https://www.gnu.org/software/libc/manual/html_mono/libc.html#TZ-Variable) in The GNU C Library Reference Manual.
You can avoid ambiguities during
daylight saving transitions by using UTC timestamps.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-a`’ [¶](#index-_002da-8)

‘`--time=atime`’

‘`--time=access`’

‘`--time=use`’
:   Change the access timestamp only. See [File timestamps](#File-timestamps).

‘`-c`’ [¶](#index-_002dc-18)

‘`--no-create`’
:   Do not warn about or create files that do not exist.

‘`-d time`’ [¶](#index-_002dd-15)

‘`--date=time`’
:   Use time instead of the current time. It can contain month names,
    time zones, ‘`am`’ and ‘`pm`’, ‘`yesterday`’, etc. For
    example, `--date="2020-07-21 14:19:13.489392193 +0530"`
    specifies the instant of time that is 489,392,193 nanoseconds after
    July 21, 2020 at 2:19:13 PM in a time zone that is 5 hours and 30
    minutes east of UTC. See [Date input formats](#Date-input-formats).
    File systems that do not support high-resolution timestamps
    silently ignore any excess precision here.

‘`-f`’ [¶](#index-_002df-19)
:   Ignored; for compatibility with BSD versions of `touch`.

‘`-h`’ [¶](#index-_002dh-8)

‘`--no-dereference`’
:   Attempt to change the timestamps of a symbolic link, rather than what
    the link refers to. When using this option, empty files are not
    created, but option `-c` must also be used to avoid warning
    about files that do not exist. Not all systems support changing the
    timestamps of symlinks, since underlying system support for this
    action was not required until POSIX 2008. Also, on some
    systems, the mere act of examining a symbolic link changes the access
    timestamp, such that only changes to the modification timestamp will persist
    long enough to be observable. When coupled with option `-r`, a
    reference timestamp is taken from a symbolic link rather than the file
    it refers to.

‘`-m`’ [¶](#index-_002dm-9)

‘`--time=mtime`’

‘`--time=modify`’
:   Change the modification timestamp only.

‘`-r file`’ [¶](#index-_002dr-10)

‘`--reference=file`’
:   Use the times of the reference file instead of the current time.
    If this option is combined with the `--date=time`
    (`-d time`) option, the reference file’s time is
    the origin for any relative times given, but is otherwise ignored.
    For example, ‘`-r foo -d '-5 seconds'`’ specifies a timestamp
    equal to five seconds before the corresponding timestamp for `foo`.
    If file is a symbolic link, the reference timestamp is taken
    from the target of the symlink, unless `-h` was also in effect.

‘`-t [[cc]yy]mmddhhmm[.ss]`’ [¶](#index-leap-seconds)
:   Use the argument (optional four-digit or two-digit years, months,
    days, hours, minutes, optional seconds) instead of the current time.
    If the year is specified with only two digits, then cc
    is 20 for years in the range 0 … 68, and 19 for years in
    69 … 99. If no digits of the year are specified,
    the argument is interpreted as a date in the current year.
    On the atypical systems that support leap seconds, ss may be
    ‘`60`’.

On systems predating POSIX 1003.1-2001,
`touch` supports an obsolete syntax, as follows.
If no timestamp is given with any of the `-d`, `-r`, or
`-t` options, and if there are two or more files and the
first file is of the form ‘`mmddhhmm[yy]`’ and this
would be a valid argument to the `-t` option (if the yy, if
any, were moved to the front), and if the represented year
is in the range 1969–1999, that argument is interpreted as the time
for the other files instead of as a file name.
Although this obsolete behavior can be controlled with the
`_POSIX2_VERSION` environment variable (see [Standards conformance](#Standards-conformance)), portable scripts should avoid commands whose
behavior depends on this variable.
For example, use ‘`touch ./12312359 main.c`’ or ‘`touch -t
12312359 main.c`’ rather than the ambiguous ‘`touch 12312359 main.c`’.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [Printing text](#Printing-text), Previous: [Changing file attributes](#Changing-file-attributes), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 14 File space usage [¶](#File-space-usage-1)

No file system can hold an infinite amount of data. These commands report
how much storage is in use or available, report other file and
file status information, and write buffers to file systems.

* [`df`: Report file system space usage](#df-invocation)
* [`du`: Estimate file space usage](#du-invocation)
* [`stat`: Report file or file system status](#stat-invocation)
* [`sync`: Synchronize cached writes to persistent storage](#sync-invocation)
* [`truncate`: Shrink or extend the size of a file](#truncate-invocation)

---

Next: [`du`: Estimate file space usage](#du-invocation), Up: [File space usage](#File-space-usage)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 14.1 `df`: Report file system space usage [¶](#df_003a-Report-file-system-space-usage)

`df` reports the amount of space used and available on
file systems. Synopsis:

```
df [option]... [file]...
```

With no arguments, `df` reports the space used and available on all
currently mounted file systems (of all types). Otherwise, `df`
reports on the file system containing each argument file.

Normally the space is printed in units of
1024 bytes, but this can be overridden (see [Block size](#Block-size)).
Non-integer quantities are rounded up to the next higher unit.

For bind mounts and without arguments, `df` only outputs the statistics
for that device with the shortest mount point name in the list of file systems
(mtab), i.e., it hides duplicate entries, unless the `-a` option is
specified.

With the same logic, `df` elides a mount entry of a dummy pseudo device
if there is another mount entry of a real block device for that mount point with
the same device number, e.g. the early-boot pseudo file system ‘`rootfs`’ is
not shown per default when already the real root device has been mounted.

If an argument file resolves to a special file containing
a mounted file system, `df` shows the space available on that
file system rather than on the file system containing the device node.
GNU `df` does not attempt to determine the usage
on unmounted file systems, because on most kinds of systems doing so
requires extremely non-portable intimate knowledge of file system structures.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-a`’ [¶](#index-_002da-9)

‘`--all`’
:   Include in the listing dummy, duplicate, or inaccessible file systems, which
    are omitted by default. Dummy file systems are typically special purpose
    pseudo file systems such as ‘`/proc`’, with no associated storage.
    Duplicate file systems are local or remote file systems that are mounted
    at separate locations in the local file hierarchy, or bind mounted locations.
    Inaccessible file systems are those which are mounted but subsequently
    over-mounted by another file system at that point, or otherwise inaccessible
    due to permissions of the mount point etc.

‘`-B size`’ [¶](#index-_002dB-1)

‘`--block-size=size`’
:   Scale sizes by size before printing them (see [Block size](#Block-size)).
    For example, `-BG` prints sizes in units of 1,073,741,824 bytes.

‘`-h`’ [¶](#index-_002dh-9)

‘`--human-readable`’
:   Append a size letter to each size, such as ‘`M`’ for mebibytes.
    Powers of 1024 are used, not 1000; ‘`M`’ stands for 1,048,576 bytes.
    This option is equivalent to `--block-size=human-readable`.
    Use the `--si` option if you prefer powers of 1000.

‘`-H`’ [¶](#index-_002dH-6)
:   Equivalent to `--si`.

‘`-i`’ [¶](#index-_002di-15)

‘`--inodes`’
:   List inode usage information instead of block usage. An inode (short
    for index node) contains information about a file such as its owner,
    permissions, timestamps, and location on the file system.

‘`-k`’ [¶](#index-_002dk-4)
:   Print sizes in 1024-byte blocks, overriding the default block size
    (see [Block size](#Block-size)).
    This option is equivalent to `--block-size=1K`.

‘`-l`’ [¶](#index-_002dl-9)

‘`--local`’
:   Limit the listing to local file systems. By default, remote file systems
    are also listed.

‘`--no-sync`’ [¶](#index-_002d_002dno_002dsync)
:   Do not invoke the `sync` system call before getting any usage data.
    This may make `df` run significantly faster on systems with many
    file systems, but on some systems (notably Solaris) the results may be slightly
    out of date. This is the default.

‘`--output`’ [¶](#index-_002d_002doutput-2)

‘`--output[=field_list]`’
:   Use the output format defined by field\_list, or print all fields if
    field\_list is omitted. In the latter case, the order of the columns
    conforms to the order of the field descriptions below.

    The use of the `--output` together with each of the options `-i`,
    `-P`, and `-T` is mutually exclusive.

    FIELD\_LIST is a comma-separated list of columns to be included in `df`’s
    output and therefore effectively controls the order of output columns.
    Each field can thus be used at the place of choice, but yet must only be
    used once.

    Valid field names in the field\_list are:

    ‘`source`’
    :   The source of the mount point, usually a device.

    ‘`fstype`’
    :   File system type.

    ‘`itotal`’
    :   Total number of inodes.

    ‘`iused`’
    :   Number of used inodes.

    ‘`iavail`’
    :   Number of available inodes.

    ‘`ipcent`’
    :   Percentage of iused divided by itotal.

    ‘`size`’
    :   Total number of blocks.

    ‘`used`’
    :   Number of used blocks.

    ‘`avail`’
    :   Number of available blocks.

    ‘`pcent`’
    :   Percentage of used divided by size.

    ‘`file`’
    :   The file name if specified on the command line.

    ‘`target`’
    :   The mount point.

    The fields for block and inodes statistics are affected by the scaling
    options like `-h` as usual.

    The definition of the field\_list can even be split among several
    `--output` uses.

    ```
    #!/bin/sh
    # Print the TARGET (i.e., the mount point) along with their percentage
    # statistic regarding the blocks and the inodes.
    df --out=target --output=pcent,ipcent

    # Print all available fields.
    df --o
    ```

‘`-P`’ [¶](#index-_002dP-6)

‘`--portability`’
:   Use the POSIX output format. This is like the default format except
    for the following:

    1. The information about each file system is always printed on exactly
       one line; a mount device is never put on a line by itself. This means
       that if the mount device name is more than 20 characters long (e.g., for
       some network mounts), the columns are misaligned.
    2. The labels in the header output line are changed to conform to POSIX.
    3. The default block size and output format are unaffected by the
       `DF_BLOCK_SIZE`, `BLOCK_SIZE` and `BLOCKSIZE` environment
       variables. However, the default block size is still affected by
       `POSIXLY_CORRECT`: it is 512 if `POSIXLY_CORRECT` is set, 1024
       otherwise. See [Block size](#Block-size).

‘`--si`’ [¶](#index-_002d_002dsi-2)
:   Append an SI-style abbreviation to each size, such as ‘`M`’ for
    megabytes. Powers of 1000 are used, not 1024; ‘`M`’ stands for
    1,000,000 bytes. This option is equivalent to
    `--block-size=si`. Use the `-h` or
    `--human-readable` option if
    you prefer powers of 1024.

‘`--sync`’ [¶](#index-_002d_002dsync)
:   Invoke the `sync` system call before getting any usage data. On
    some systems (notably Solaris), doing this yields more up to date results,
    but in general this option makes `df` much slower, especially when
    there are many or very busy file systems.

‘`--total`’ [¶](#index-_002d_002dtotal)
:   Print a grand total of all arguments after all arguments have
    been processed. This can be used to find out the total size, usage
    and available space of all listed devices. If no arguments are specified
    df will try harder to elide file systems insignificant to the total
    available space, by suppressing duplicate remote file systems.

    For the grand total line, `df` prints ‘`"total"`’ into the
    source column, and ‘`"-"`’ into the target column.
    If there is no source column (see `--output`), then
    `df` prints ‘`"total"`’ into the target column,
    if present.

‘`-t fstype`’ [¶](#index-_002dt-15)

‘`--type=fstype`’
:   Limit the listing to file systems of type fstype. Multiple
    file system types can be specified by giving multiple `-t` options.
    By default, nothing is omitted.

‘`-T`’ [¶](#index-_002dT-9)

‘`--print-type`’
:   Print each file system’s type. The types printed here are the same ones
    you can include or exclude with `-t` and `-x`. The particular
    types printed are whatever is supported by the system. Here are some of
    the common names (this list is certainly not exhaustive):

    ‘`nfs`’ [¶](#index-NFS-file-system-type)
    :   An NFS file system, i.e., one mounted over a network from another
        machine. This is the one type name which seems to be used uniformly by
        all systems.

    ‘`ext2, ext3, ext4, xfs, btrfs…`’ [¶](#index-Linux-file-system-types)
    :   A file system on a locally-mounted device. (The system might even
        support more than one type here; GNU/Linux does.)

    ‘`iso9660, cdfs`’ [¶](#index-CD_002dROM-file-system-type)
    :   A file system on a CD or DVD drive. HP-UX uses ‘`cdfs`’, most other
        systems use ‘`iso9660`’.

    ‘`ntfs,fat`’ [¶](#index-NTFS-file-system)
    :   File systems used by MS-Windows / MS-DOS.

‘`-x fstype`’ [¶](#index-_002dx-5)

‘`--exclude-type=fstype`’
:   Limit the listing to file systems not of type fstype.
    Multiple file system types can be eliminated by giving multiple
    `-x` options. By default, no file system types are omitted.

‘`-v`’
:   Ignored; for compatibility with System V versions of `df`.

`df` is installed only on systems that have usable mount tables,
so portable scripts should not rely on its existence.

An exit status of zero indicates success,
and a nonzero value indicates failure.
Failure includes the case where no output is generated, so you can
inspect the exit status of a command like ‘`df -t ext3 -t reiserfs
dir`’ to test whether dir is on a file system of type
‘`ext3`’ or ‘`reiserfs`’.

Since the list of file systems (mtab) is needed to determine the
file system type, failure includes the cases when that list cannot
be read and one or more of the options `-a`, `-l`, `-t`
or `-x` is used together with a file name argument.

---

Next: [`stat`: Report file or file system status](#stat-invocation), Previous: [`df`: Report file system space usage](#df-invocation), Up: [File space usage](#File-space-usage)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 14.2 `du`: Estimate file space usage [¶](#du_003a-Estimate-file-space-usage)

`du` reports the space needed to represent a set of files.
Synopsis:

```
du [option]... [file]...
```

With no arguments, `du` reports the space needed to represent
the files at or under the current directory.
Normally the space is printed in units of
1024 bytes, but this can be overridden (see [Block size](#Block-size)).
Non-integer quantities are rounded up to the next higher unit.

If two or more hard links point to the same file, only one of the hard
links is counted. The file argument order affects which links
are counted, and changing the argument order may change the numbers
and entries that `du` outputs.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-0`’ [¶](#index-_002d0)

‘`--null`’
:   Output a zero byte (ASCII NUL) at the end of each line,
    rather than a newline. This option enables other programs to parse the
    output even when that output would contain data with embedded newlines.

‘`-a`’ [¶](#index-_002da-10)

‘`--all`’
:   Show counts for all files, not just directories.

‘`--apparent-size`’ [¶](#index-_002d_002dapparent_002dsize)
:   Print apparent sizes, rather than file system usage. The apparent size of a
    file is the number of bytes reported by `wc -c` on regular files,
    or more generally, `ls -l --block-size=1` or `stat --format=%s`.
    For example, a file containing the word ‘`zoo`’ with no newline would,
    of course, have an apparent size of 3. Such a small file may require
    anywhere from 0 to 16 KiB or more of file system space, depending on
    the type and configuration of the file system on which the file resides.
    However, a sparse file created with this command:

    ```
    dd bs=1 seek=2GiB if=/dev/null of=big
    ```

    has an apparent size of 2 GiB, yet on most modern
    file systems, it actually uses almost no space.

    Apparent sizes are meaningful only for regular files and symbolic links.
    Other file types do not contribute to apparent size.

‘`-B size`’ [¶](#index-_002dB-2)

‘`--block-size=size`’
:   Scale sizes by size before printing them (see [Block size](#Block-size)).
    For example, `-BG` prints sizes in units of 1,073,741,824 bytes.

‘`-b`’ [¶](#index-_002db-18)

‘`--bytes`’
:   Equivalent to `--apparent-size --block-size=1`.

‘`-c`’ [¶](#index-_002dc-19)

‘`--total`’
:   Print a grand total of all arguments after all arguments have
    been processed. This can be used to find out the total file system usage of
    a given set of files or directories.

‘`-D`’ [¶](#index-_002dD-3)

‘`--dereference-args`’
:   Dereference symbolic links that are command line arguments.
    Does not affect other symbolic links. This is helpful for finding
    out the file system usage of directories, such as `/usr/tmp`, which
    are often symbolic links.

‘`-d depth`’ [¶](#index-_002dd-depth)

‘`--max-depth=depth`’
:   Show the total for each directory (and file if `--all`) that is at
    most MAX\_DEPTH levels down from the root of the hierarchy. The root
    is at level 0, so `du --max-depth=0` is equivalent to `du -s`.

‘`--files0-from=file`’ [¶](#index-_002d_002dfiles0_002dfrom_003dfile-2)
:   Disallow processing files named on the command line, and instead process
    those named in file file; each name being terminated by a zero byte
    (ASCII NUL).
    This is useful
    when the list of file names is so long that it may exceed a command line
    length limitation.
    In such cases, running `du` via `xargs` is undesirable
    because it splits the list into pieces and makes `du` print
    with the `--total` (`-c`) option for each sublist rather than for the entire list.
    One way to produce a list of ASCII NUL terminated file
    names is with GNU
    `find`, using its `-print0` predicate.
    If file is ‘`-`’ then the ASCII NUL terminated
    file names are read from standard input.

‘`-H`’ [¶](#index-_002dH-7)
:   Equivalent to `--dereference-args` (`-D`).

‘`-h`’ [¶](#index-_002dh-10)

‘`--human-readable`’
:   Append a size letter to each size, such as ‘`M`’ for mebibytes.
    Powers of 1024 are used, not 1000; ‘`M`’ stands for 1,048,576 bytes.
    This option is equivalent to `--block-size=human-readable`.
    Use the `--si` option if you prefer powers of 1000.

‘`--inodes`’ [¶](#index-_002d_002dinodes-1)
:   List inode usage information instead of block usage.
    This option is useful for finding directories which contain many files, and
    therefore eat up most of the inodes space of a file system (see `df`,
    option `--inodes`).
    It can well be combined with the options `-a`, `-c`,
    `-h`, `-l`, `-s`, `-S`, `-t` and
    `-x`; however, passing other options regarding the block size, for
    example `-b`, `-m` and `--apparent-size`, is ignored.

‘`-k`’ [¶](#index-_002dk-5)
:   Print sizes in 1024-byte blocks, overriding the default block size
    (see [Block size](#Block-size)).
    This option is equivalent to `--block-size=1K`.

‘`-L`’ [¶](#index-_002dL-8)

‘`--dereference`’
:   Dereference symbolic links (show the file system space used by the file
    or directory that the link points to instead of the space used by
    the link).

‘`-l`’ [¶](#index-_002dl-10)

‘`--count-links`’
:   Count the size of all files, even if they have appeared already (as a
    hard link).

‘`-m`’ [¶](#index-_002dm-10)
:   Print sizes in 1,048,576-byte blocks, overriding the default block size
    (see [Block size](#Block-size)).
    This option is equivalent to `--block-size=1M`.

‘`-P`’ [¶](#index-_002dP-7)

‘`--no-dereference`’
:   For each symbolic link encountered by `du`,
    consider the file system space used by the symbolic link itself.

‘`-S`’ [¶](#index-_002dS-10)

‘`--separate-dirs`’
:   Normally, in the output of `du` (when not using `--summarize`),
    the size listed next to a directory name, d, represents the sum
    of sizes of all entries beneath d as well as the size of d itself.
    With `--separate-dirs`, the size reported for a directory name,
    d, will exclude the size of any subdirectories.

‘`--si`’ [¶](#index-_002d_002dsi-3)
:   Append an SI-style abbreviation to each size, such as ‘`M`’ for
    megabytes. Powers of 1000 are used, not 1024; ‘`M`’ stands for
    1,000,000 bytes. This option is equivalent to
    `--block-size=si`. Use the `-h` or
    `--human-readable` option if
    you prefer powers of 1024.

‘`-s`’ [¶](#index-_002ds-20)

‘`--summarize`’
:   Display only a total for each argument.

‘`-t size`’ [¶](#index-_002dt-16)

‘`--threshold=size`’
:   Exclude entries based on a given size. The size refers to used
    blocks in normal mode (see [Block size](#Block-size)), or inodes count in conjunction
    with the `--inodes` option.

    If size is positive, then `du` will only print entries with a size
    greater than or equal to that.

    If size is negative, then `du` will only print entries with a size
    smaller than or equal to that.

    Although GNU `find` can be used to find files of a certain size,
    `du`’s `--threshold` option can be used to also filter
    directories based on a given size.

    When combined with the `--apparent-size` option, the
    `--threshold` option elides entries based on apparent size.
    When combined with the `--inodes` option, it elides entries
    based on inode counts.

    Here’s how you would use `--threshold` to find directories with a size
    greater than or equal to 200 megabytes:

    ```
    du --threshold=200MB
    ```

    Here’s how you would use `--threshold` to find directories and
    files – the `-a` – with an apparent size smaller than or
    equal to 500 bytes:

    ```
    du -a -t -500 --apparent-size
    ```

    Here’s how you would use `--threshold` to find directories on the root
    file system with more than 20000 inodes used in the directory tree below:

    ```
    du --inodes -x --threshold=20000 /
    ```

‘`--time`’ [¶](#index-_002d_002dtime-6)
:   Show the most recent modification timestamp (mtime) of any file in the
    directory, or any of its subdirectories. See [File timestamps](#File-timestamps).

‘`--time=ctime`’ [¶](#index-_002d_002dtime-7)

‘`--time=status`’

‘`--time=use`’
:   Show the most recent status change timestamp (ctime) of any file in
    the directory, or any of its subdirectories. See [File timestamps](#File-timestamps).

‘`--time=atime`’ [¶](#index-_002d_002dtime-8)

‘`--time=access`’
:   Show the most recent access timestamp (atime) of any file in the
    directory, or any of its subdirectories. See [File timestamps](#File-timestamps).

‘`--time-style=style`’ [¶](#index-_002d_002dtime_002dstyle-1)
:   List timestamps in style style. This option has an effect only if
    the `--time` option is also specified. The style should
    be one of the following:

    ‘`+format`’ [¶](#index-LC_005fTIME-5)
    :   List timestamps using format, where format is interpreted
        like the format argument of `date` (see [`date`: Print or set system date and time](#date-invocation)).
        For example, `--time-style="+%Y-%m-%d %H:%M:%S"` causes
        `du` to list timestamps like ‘`2020-07-21 23:45:56`’. As
        with `date`, format’s interpretation is affected by the
        `LC_TIME` locale category.

    ‘`full-iso`’
    :   List timestamps in full using ISO 8601-like date, time, and time zone
        components with nanosecond precision, e.g., ‘`2020-07-21
        23:45:56.477817180 -0400`’. This style is equivalent to
        ‘`+%Y-%m-%d %H:%M:%S.%N %z`’.

    ‘`long-iso`’
    :   List ISO 8601 date and time components with minute precision, e.g.,
        ‘`2020-07-21 23:45`’. These timestamps are shorter than
        ‘`full-iso`’ timestamps, and are usually good enough for everyday
        work. This style is equivalent to ‘`+%Y-%m-%d %H:%M`’.

    ‘`iso`’
    :   List ISO 8601 dates for timestamps, e.g., ‘`2020-07-21`’.
        This style is equivalent to ‘`+%Y-%m-%d`’.

    You can specify the default value of the `--time-style` option
    with the environment variable `TIME_STYLE`; if `TIME_STYLE` is not set
    the default style is ‘`long-iso`’. For compatibility with `ls`,
    if `TIME_STYLE` begins with ‘`+`’ and contains a newline,
    the newline and any later characters are ignored; if `TIME_STYLE`
    begins with ‘`posix-`’ the ‘`posix-`’ is ignored; and if
    `TIME_STYLE` is ‘`locale`’ it is ignored.

‘`-X file`’ [¶](#index-_002dX-file)

‘`--exclude-from=file`’
:   Like `--exclude`, except take the patterns to exclude from file,
    one per line. If file is ‘`-`’, take the patterns from standard
    input.

‘`--exclude=pattern`’ [¶](#index-_002d_002dexclude_003dpattern)
:   When recursing, skip subdirectories or files matching pattern.
    For example, `du --exclude='*.o'` excludes files whose names
    end in ‘`.o`’.

‘`-x`’ [¶](#index-_002dx-6)

‘`--one-file-system`’
:   Skip directories that are on different file systems from the one that
    the argument being processed is on.

Since `du` relies on information reported by the operating
system, its output might not reflect the space consumed in the
underlying devices. For example;

* Operating systems normally do not report device space consumed by
  duplicate or backup blocks, error correction bits, and so forth.
  This causes `du` to underestimate the device space actually used.
* In file systems that use copy-on-write, if two distinct files share
  space the output of `du` typically counts the space that would
  be consumed if all files’ non-holes were rewritten, not the space
  currently consumed.
* In file systems that use compression, the operating system might
  report the uncompressed space. (If it does report the compressed space,
  that report might change after one merely overwrites existing file data.)
* Networked file systems historically have had difficulty communicating
  accurate file system information from server to client.

For these reasons `du` might better be thought of as an
estimate of the size of a `tar` or other conventional backup
for a set of files, rather than as a measure of space consumed in the
underlying devices.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`sync`: Synchronize cached writes to persistent storage](#sync-invocation), Previous: [`du`: Estimate file space usage](#du-invocation), Up: [File space usage](#File-space-usage)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]
