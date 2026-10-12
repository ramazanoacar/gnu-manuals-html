### 26.7 Changing File Names and Attributes [¶](#Changing-File-Names-and-Attributes)

The functions in this section rename, copy, delete, link, and set
the modes (permissions) of files. Typically, they signal a
`file-error` error if they fail to perform their function,
reporting the system-dependent error message that describes the reason
for the failure. If they fail because a file is missing, they signal
a `file-missing` error instead.

For performance, the operating system may cache or alias changes
made by these functions instead of writing them immediately to
secondary storage. See [Files and Secondary Storage](#Files-and-Storage).

In the functions that have an argument newname, if this
argument is a directory name it is treated as if the nondirectory part
of the source name were appended. Typically, a directory name is one
that ends in ‘`/`’ (see [Directory Names](#Directory-Names)). For example, if the
old name is `a/b/c`, the newname `d/e/f/` is treated
as if it were `d/e/f/c`. This special treatment does not apply
if newname is not a directory name but names a file that is a
directory; for example, the newname `d/e/f` is left as-is
even if `d/e/f` happens to be a directory.

In the functions that have an argument newname, if a file by the
name of newname already exists, the actions taken depend on the
value of the argument ok-if-already-exists:

* Signal a `file-already-exists` error if
  ok-if-already-exists is `nil`.
* Request confirmation if ok-if-already-exists is a number.
* Replace the old file without confirmation if ok-if-already-exists
  is any other value.

Command: **add-name-to-file** oldname newname &optional ok-if-already-exists [¶](#index-add_002dname_002dto_002dfile)
:   This function gives the file named oldname the additional name
    newname. This means that newname becomes a new hard
    link to oldname.

    If newname is a symbolic link, its directory entry is replaced,
    not the directory entry it points to. If oldname is a symbolic
    link, this function might or might not follow the link; it does not
    follow the link on GNU platforms. If oldname is a directory,
    this function typically fails, although for the superuser on a few
    old-fashioned non-GNU platforms it can succeed and create a filesystem
    that is not tree-structured.

    In the first part of the following example, we list two files,
    `foo` and `foo3`.

    ```
    $ ls -li fo*
    81908 -rw-rw-rw- 1 rms rms 29 Aug 18 20:32 foo
    84302 -rw-rw-rw- 1 rms rms 24 Aug 18 20:31 foo3
    ```

    Now we create a hard link, by calling `add-name-to-file`, then list
    the files again. This shows two names for one file, `foo` and
    `foo2`.

    ```
    (add-name-to-file "foo" "foo2")
         ⇒ nil
    ```

    ```

    ```

    ```
    $ ls -li fo*
    81908 -rw-rw-rw- 2 rms rms 29 Aug 18 20:32 foo
    81908 -rw-rw-rw- 2 rms rms 29 Aug 18 20:32 foo2
    84302 -rw-rw-rw- 1 rms rms 24 Aug 18 20:31 foo3
    ```

    Finally, we evaluate the following:

    ```
    (add-name-to-file "foo" "foo3" t)
    ```

    and list the files again. Now there are three names
    for one file: `foo`, `foo2`, and `foo3`. The old
    contents of `foo3` are lost.

    ```
    (add-name-to-file "foo1" "foo3")
         ⇒ nil
    ```

    ```

    ```

    ```
    $ ls -li fo*
    81908 -rw-rw-rw- 3 rms rms 29 Aug 18 20:32 foo
    81908 -rw-rw-rw- 3 rms rms 29 Aug 18 20:32 foo2
    81908 -rw-rw-rw- 3 rms rms 29 Aug 18 20:32 foo3
    ```

    This function is meaningless on operating systems where multiple names
    for one file are not allowed. Some systems implement multiple names
    by copying the file instead.

    See also `file-nlinks` in [File Attributes](#File-Attributes).

Command: **rename-file** filename newname &optional ok-if-already-exists [¶](#index-rename_002dfile)
:   This command renames the file filename as newname.

    If filename has additional names aside from filename, it
    continues to have those names. In fact, adding the name newname
    with `add-name-to-file` and then deleting filename has the
    same effect as renaming, aside from momentary intermediate states and
    treatment of errors, directories and symbolic links.

    This command does not follow symbolic links. If filename is a
    symbolic link, this command renames the symbolic link, not the file it
    points to. If newname is a symbolic link, its directory entry
    is replaced, not the directory entry it points to.

    This command does nothing if filename and newname are the
    same directory entry, i.e., if they refer to the same parent directory
    and give the same name within that directory. Otherwise, if
    filename and newname name the same file, this command does
    nothing on POSIX-conforming systems, and removes filename on
    some non-POSIX systems.

    If newname exists, then it must be an empty directory if
    oldname is a directory and a non-directory otherwise.

Command: **copy-file** oldname newname &optional ok-if-already-exists time preserve-uid-gid preserve-permissions [¶](#index-copy_002dfile)
:   This command copies the file oldname to newname. An
    error is signaled if oldname is not a regular file. If newname
    names a directory, it copies oldname into that directory,
    preserving its final name component.

    This function follows symbolic links, except that it does not follow a
    dangling symbolic link to create newname.

    If time is non-`nil`, then this function gives the new file
    the same last-modified time that the old one has. (This works on only
    some operating systems.) If setting the time gets an error,
    `copy-file` signals a `file-date-error` error. In an
    interactive call, a prefix argument specifies a non-`nil` value
    for time.

    If argument preserve-uid-gid is `nil`, we let the operating
    system decide the user and group ownership of the new file (this is
    usually set to the user running Emacs). If preserve-uid-gid is
    non-`nil`, we attempt to copy the user and group ownership of the
    file. This works only on some operating systems, and only if you have
    the correct permissions to do so.

    If the optional argument preserve-permissions is non-`nil`,
    this function copies the file modes (or “permissions”) of
    oldname to newname, as well as the Access Control List and
    SELinux context (if any). See [Information about Files](#Information-about-Files).

    Otherwise, the file modes of newname are left unchanged if it is
    an existing file, and set to those of oldname, masked by the
    default file permissions (see `set-default-file-modes` below), if
    newname is to be newly created. The Access Control List or
    SELinux context are not copied over in either case.

Command: **make-symbolic-link** target linkname &optional ok-if-already-exists [¶](#index-make_002dsymbolic_002dlink)
:   This command makes a symbolic link to target, named
    linkname. This is like the shell command ‘`ln -s
    target linkname`’. The target argument
    is treated only as a string; it need not name an existing file.
    If ok-if-already-exists is an integer, indicating interactive
    use, then leading ‘`~`’ is expanded and leading ‘`/:`’ is
    stripped in the target string.

    If target is a relative file name, the resulting symbolic link
    is interpreted relative to the directory containing the symbolic link.
    See [Absolute and Relative File Names](#Relative-File-Names).

    If both target and linkname have remote file name syntax,
    and if both remote identifications are equal, the symbolic link points
    to the local file name part of target.

    This function is not available on systems that don’t support symbolic
    links.

Command: **delete-file** filename &optional trash [¶](#index-delete_002dfile)
:   This command deletes the file filename. If the file has
    multiple names, it continues to exist under the other names. If
    filename is a symbolic link, `delete-file` deletes only the
    symbolic link and not its target.

    The command signals a suitable kind of `file-error` error if
    filename cannot be deleted. (On GNU and other POSIX-like
    systems, a file can be deleted if its directory is writable.) If the
    file does not exist, this command will not signal any error.

    If the optional argument trash is non-`nil` and the
    variable `delete-by-moving-to-trash` is non-`nil`, this
    command moves the file into the system Trash instead of deleting it.
    See [Miscellaneous File Operations](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Misc-File-Ops) in The GNU
    Emacs Manual. When called interactively, trash is `t` if
    no prefix argument is given, and `nil` otherwise.

    See also `delete-directory` in [Creating, Copying and Deleting Directories](#Create_002fDelete-Dirs).

User Option: **remote-file-name-inhibit-delete-by-moving-to-trash** [¶](#index-remote_002dfile_002dname_002dinhibit_002ddelete_002dby_002dmoving_002dto_002dtrash)
:   If this variable is non-`nil`, remote files are never moved to
    the Trash. They are deleted instead.

Command: **set-file-modes** filename mode &optional flag [¶](#index-set_002dfile_002dmodes)
:   This function sets the *file mode* (or *permissions*) of
    filename to mode.

    By default this function follows symbolic links. However, if the
    optional argument flag is the symbol `nofollow`, this
    function does not follow filename if it is a symbolic link;
    this can help prevent inadvertently changing the mode bits of a file
    somewhere else. On platforms that do not support changing mode bits
    on a symbolic link, this function signals an error when filename
    is a symbolic link and flag is `nofollow`.

    If called non-interactively, mode must be an integer. Only the
    lowest 12 bits of the integer are used; on most systems, only the
    lowest 9 bits are meaningful. You can use the Lisp construct for
    octal numbers to enter mode. For example,

    ```
    (set-file-modes "myfile" #o644 'nofollow)
    ```

    specifies that the file should be readable and writable for its owner,
    readable for group members, and readable for all other users.
    See [File permissions](https://www.gnu.org/software/coreutils/manual/coreutils.html#File-permissions) in The GNU `Coreutils`
    Manual, for a description of mode bit specifications.

    Interactively, mode is read from the minibuffer using
    `read-file-modes` (see below), which lets the user type in either
    an integer or a string representing the permissions symbolically.

    See [Testing Accessibility](#Testing-Accessibility), for the function `file-modes`, which
    returns the permissions of a file.

Function: **set-default-file-modes** mode [¶](#index-set_002ddefault_002dfile_002dmodes)
:   This function sets the default permissions for new files created by
    Emacs and its subprocesses. Every file created with Emacs initially
    has these permissions, or a subset of them (`write-region` will
    not grant execute permissions even if the default file permissions
    allow execution). On GNU and other POSIX-like systems, the default
    permissions are given by the bitwise complement of the ‘`umask`’
    value, i.e. each bit that is set in the argument mode will be
    *reset* in the default permissions with which Emacs creates
    files.

    The argument mode should be an integer which specifies the
    permissions, similar to `set-file-modes` above. Only the lowest
    9 bits are meaningful.

    The default file permissions have no effect when you save a modified
    version of an existing file; saving a file preserves its existing
    permissions.

Macro: **with-file-modes** mode body… [¶](#index-with_002dfile_002dmodes)
:   This macro evaluates the body forms with the default
    permissions for new files temporarily set to modes (whose value
    is as for `set-file-modes` above). When finished, it restores
    the original default file permissions, and returns the value of the
    last form in body.

    This is useful for creating private files, for example.

Function: **default-file-modes** [¶](#index-default_002dfile_002dmodes)
:   This function returns the default file permissions, as an integer.

Function: **read-file-modes** &optional prompt base-file [¶](#index-read_002dfile_002dmodes)
:   This function reads a set of file mode bits from the minibuffer. The
    first optional argument prompt specifies a non-default prompt.
    Second second optional argument base-file is the name of a file
    on whose permissions to base the mode bits that this function returns,
    if what the user types specifies mode bits relative to permissions of
    an existing file.

    If user input represents an octal number, this function returns that
    number. If it is a complete symbolic specification of mode bits, as
    in `"u=rwx"`, the function converts it to the equivalent numeric
    value using `file-modes-symbolic-to-number` and returns the
    result. If the specification is relative, as in `"o+g"`, then
    the permissions on which the specification is based are taken from the
    mode bits of base-file. If base-file is omitted or
    `nil`, the function uses `0` as the base mode bits. The
    complete and relative specifications can be combined, as in
    `"u+r,g+rx,o+r,g-w"`. See [File permissions](https://www.gnu.org/software/coreutils/manual/coreutils.html#File-permissions) in The
    GNU `Coreutils` Manual, for a description of file mode
    specifications.

Function: **file-modes-symbolic-to-number** modes &optional base-modes [¶](#index-file_002dmodes_002dsymbolic_002dto_002dnumber)
:   This function converts a symbolic file mode specification in
    modes into the equivalent integer. If the symbolic
    specification is based on an existing file, that file’s mode bits are
    taken from the optional argument base-modes; if that argument is
    omitted or `nil`, it defaults to 0, i.e., no access rights at
    all.

Function: **file-modes-number-to-symbolic** modes [¶](#index-file_002dmodes_002dnumber_002dto_002dsymbolic)
:   This function converts a numeric file mode specification in
    modes into the equivalent string form. The string which this
    function returns is in the same format produced by the shell command
    `ls -l` and by `file-attributes`, *not* the symbolic
    form accepted by `file-modes-symbolic-to-number` and the
    `chmod` shell command.

Function: **set-file-times** filename &optional time flag [¶](#index-set_002dfile_002dtimes)
:   This function sets the access and modification times of filename
    to time. The return value is `t` if the times are successfully
    set, otherwise it is `nil`. time defaults to the current
    time and must be a time value (see [Time of Day](#Time-of-Day)).

    By default this function follows symbolic links. However, if the
    optional argument flag is the symbol `nofollow`, this
    function does not follow filename if it is a symbolic link;
    this can help prevent inadvertently changing the times of a file
    somewhere else. On platforms that do not support changing times
    on a symbolic link, this function signals an error when filename
    is a symbolic link and flag is `nofollow`.

Function: **set-file-extended-attributes** filename attribute-alist [¶](#index-set_002dfile_002dextended_002dattributes)
:   This function sets the Emacs-recognized extended file attributes for
    filename. The second argument attribute-alist should be
    an alist of the same form returned by `file-extended-attributes`.
    The return value is `t` if the attributes are successfully set,
    otherwise it is `nil`.
    See [Extended File Attributes](#Extended-Attributes).

Function: **set-file-selinux-context** filename context [¶](#index-set_002dfile_002dselinux_002dcontext)
:   This function sets the SELinux security context for filename to
    context. The context argument should be a list
    `(user role type range)`, where each
    element is a string. See [Extended File Attributes](#Extended-Attributes).

    The function returns `t` if it succeeds in setting the SELinux
    context of filename. It returns `nil` if the context was
    not set (e.g., if SELinux is disabled, or if Emacs was compiled
    without SELinux support).

Function: **set-file-acl** filename acl [¶](#index-set_002dfile_002dacl)
:   This function sets the Access Control List for filename to
    acl. The acl argument should have the same form returned
    by the function `file-acl`. See [Extended File Attributes](#Extended-Attributes).

    The function returns `t` if it successfully sets the ACL of
    filename, `nil` otherwise.

---

Next: [File Names](#File-Names), Previous: [Changing File Names and Attributes](#Changing-Files), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.8 Files and Secondary Storage [¶](#Files-and-Secondary-Storage)

After Emacs changes a file, there are two reasons the changes might
not survive later failures of power or media, both having to do with
efficiency. First, the operating system might alias written data with
data already stored elsewhere on secondary storage until one file or
the other is later modified; this will lose both files if the only
copy on secondary storage is lost due to media failure. Second, the
operating system might not write data to secondary storage
immediately, which will lose the data if power is lost
or if there is a media failure.

Although both sorts of failures can largely be avoided by a suitably
configured system, such systems are typically more expensive or
less efficient. In lower-end systems, to survive media failure you
can copy the file to a different device, and to survive a power
failure (or be immediately notified of a media failure) you can use
the `write-region` function with the
`write-region-inhibit-fsync` variable set to `nil`.
Although this variable is ordinarily `t` because that can
significantly improve performance, it may make sense to temporarily
bind it to `nil` if using Emacs to implement database-like
transactions that survive power failure on lower-end systems.
See [Writing to Files](#Writing-to-Files).

On some platforms when Emacs changes a file other processes might not
be notified of the change immediately. Setting
`write-region-inhibit-fsync` to `nil` may improve
notification speed in this case, though there are no guarantees.

---

Next: [Contents of Directories](#Contents-of-Directories), Previous: [Files and Secondary Storage](#Files-and-Storage), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.9 File Names [¶](#File-Names-1)

Files are generally referred to by their names, in Emacs as elsewhere.
File names in Emacs are represented as strings. The functions that
operate on a file all expect a file name argument.

In addition to operating on files themselves, Emacs Lisp programs
often need to operate on file names; i.e., to take them apart and to use
part of a name to construct related file names. This section describes
how to manipulate file names.

The functions in this section do not actually access files, so they
can operate on file names that do not refer to an existing file or
directory.

On MS-DOS and MS-Windows, these functions (like the function that
actually operate on files) accept MS-DOS or MS-Windows file-name syntax,
where backslashes separate the components, as well as POSIX syntax; but
they always return POSIX syntax. This enables Lisp programs to specify
file names in POSIX syntax and work properly on all systems without
change.[22](#FOOT22)

* [File Name Components](#File-Name-Components)
* [Absolute and Relative File Names](#Relative-File-Names)
* [Directory Names](#Directory-Names)
* [Functions that Expand Filenames](#File-Name-Expansion)
* [Generating Unique File Names](#Unique-File-Names)
* [File Name Completion](#File-Name-Completion)
* [Standard File Names](#Standard-File-Names)

---

Next: [Absolute and Relative File Names](#Relative-File-Names), Up: [File Names](#File-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.9.1 File Name Components [¶](#File-Name-Components-1)

The operating system groups files into directories. To specify a
file, you must specify the directory and the file’s name within that
directory. Therefore, Emacs considers a file name as having two main
parts: the *directory name* part, and the *nondirectory* part
(or *file name within the directory*). Either part may be empty.
Concatenating these two parts reproduces the original file name.
[23](#FOOT23)

On most systems, the directory part is everything up to and including
the last slash (backslash is also allowed in input on MS-DOS or
MS-Windows); the nondirectory part is the rest.

For some purposes, the nondirectory part is further subdivided into
the name proper and the *version number*. On most systems, only
backup files have version numbers in their names.

Function: **file-name-directory** filename [¶](#index-file_002dname_002ddirectory)
:   This function returns the directory part of filename, as a
    directory name (see [Directory Names](#Directory-Names)), or `nil` if
    filename does not include a directory part.

    On GNU and other POSIX-like systems, a string returned by this function always
    ends in a slash. On MS-DOS it can also end in a colon.

    ```
    (file-name-directory "lewis/foo")  ; GNU example
         ⇒ "lewis/"
    ```

    ```
    (file-name-directory "foo")        ; GNU example
         ⇒ nil
    ```

Function: **file-name-nondirectory** filename [¶](#index-file_002dname_002dnondirectory)
:   This function returns the nondirectory part of filename.

    ```
    (file-name-nondirectory "lewis/foo")
         ⇒ "foo"
    ```

    ```
    (file-name-nondirectory "foo")
         ⇒ "foo"
    ```

    ```
    (file-name-nondirectory "lewis/")
         ⇒ ""
    ```

Function: **file-name-sans-versions** filename &optional keep-backup-version [¶](#index-file_002dname_002dsans_002dversions)
:   This function returns filename with any file version numbers,
    backup version numbers, or trailing tildes discarded.

    If keep-backup-version is non-`nil`, then true file version
    numbers understood as such by the file system are discarded from the
    return value, but backup version numbers are kept.

    ```
    (file-name-sans-versions "~rms/foo.~1~")
         ⇒ "~rms/foo"
    ```

    ```
    (file-name-sans-versions "~rms/foo~")
         ⇒ "~rms/foo"
    ```

    ```
    (file-name-sans-versions "~rms/foo")
         ⇒ "~rms/foo"
    ```

Function: **file-name-extension** filename &optional period [¶](#index-file_002dname_002dextension)
:   This function returns filename’s final extension, if any,
    after applying `file-name-sans-versions` to remove any
    version/backup part. The extension, in a file name, is the part that
    follows the last ‘`.`’ in the last name component (minus any
    version/backup part).

    This function returns `nil` for extensionless file names such as
    `foo`. It returns `""` for null extensions, as in
    `foo.`. If the last component of a file name begins with a
    ‘`.`’, that ‘`.`’ doesn’t count as the beginning of an
    extension. Thus, `.emacs`’s extension is `nil`, not
    ‘`.emacs`’.

    If period is non-`nil`, then the returned value includes
    the period that delimits the extension, and if filename has no
    extension, the value is `""`.

Function: **file-name-with-extension** filename extension [¶](#index-file_002dname_002dwith_002dextension)
:   This function returns filename with its extension set to
    extension. A single leading dot in the extension will be
    stripped if there is one. For example:

    ```
    (file-name-with-extension "file" "el")
         ⇒ "file.el"
    (file-name-with-extension "file" ".el")
         ⇒ "file.el"
    (file-name-with-extension "file.c" "el")
         ⇒ "file.el"
    ```

    Note that this function will error if filename or
    extension are empty, or if the filename is shaped like a
    directory (i.e., if `directory-name-p` returns non-`nil`).

Function: **file-name-sans-extension** filename [¶](#index-file_002dname_002dsans_002dextension)
:   This function returns filename minus its extension, if any. The
    version/backup part, if present, is only removed if the file has an
    extension. For example,

    ```
    (file-name-sans-extension "foo.lose.c")
         ⇒ "foo.lose"
    (file-name-sans-extension "big.hack/foo")
         ⇒ "big.hack/foo"
    (file-name-sans-extension "/my/home/.emacs")
         ⇒ "/my/home/.emacs"
    (file-name-sans-extension "/my/home/.emacs.el")
         ⇒ "/my/home/.emacs"
    (file-name-sans-extension "~/foo.el.~3~")
         ⇒ "~/foo"
    (file-name-sans-extension "~/foo.~3~")
         ⇒ "~/foo.~3~"
    ```

    Note that the ‘`.~3~`’ in the two last examples is the backup part,
    not an extension.

Function: **file-name-base** filename [¶](#index-file_002dname_002dbase)
:   This function is the composition of `file-name-sans-extension`
    and `file-name-nondirectory`. For example,

    ```
    (file-name-base "/my/home/foo.c")
        ⇒ "foo"
    ```

Function: **file-name-split** filename [¶](#index-file_002dname_002dsplit)
:   This function splits a file name into its components, and can be
    thought of as the inverse of `string-join` with the appropriate
    directory separator. For example,

    ```
    (file-name-split "/tmp/foo.txt")
        ⇒ ("" "tmp" "foo.txt")
    (string-join (file-name-split "/tmp/foo.txt") "/")
        ⇒ "/tmp/foo.txt"
    ```

---

Next: [Directory Names](#Directory-Names), Previous: [File Name Components](#File-Name-Components), Up: [File Names](#File-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.9.2 Absolute and Relative File Names [¶](#Absolute-and-Relative-File-Names)

All the directories in the file system form a tree starting at the
root directory. A file name can specify all the directory names
starting from the root of the tree; then it is called an
*absolute* file name. Or it can specify the position of the file
in the tree relative to a default directory; then it is called a
*relative* file name. On GNU and other POSIX-like systems,
after any leading ‘`~`’ has been expanded, an absolute file name
starts with a ‘`/`’
(see [abbreviate-file-name](#abbreviate_002dfile_002dname)), and a relative one does not. On
MS-DOS and MS-Windows, an absolute file name starts with a slash or a
backslash, or with a drive specification ‘`x:/`’, where
x is the *drive letter*.

Function: **file-name-absolute-p** filename [¶](#index-file_002dname_002dabsolute_002dp)
:   This function returns `t` if file filename is an absolute
    file name, `nil` otherwise. A file name is considered to be
    absolute if its first component is ‘`~`’, or is ‘`~user`’
    where user is a valid login name. In the following examples,
    assume that there is a user named ‘`rms`’ but no user named
    ‘`nosuchuser`’.

    ```
    (file-name-absolute-p "~rms/foo")
         ⇒ t
    ```

    ```
    (file-name-absolute-p "~nosuchuser/foo")
         ⇒ nil
    ```

    ```
    (file-name-absolute-p "rms/foo")
         ⇒ nil
    ```

    ```
    (file-name-absolute-p "/user/rms/foo")
         ⇒ t
    ```

Given a possibly relative file name, you can expand any
leading ‘`~`’ and convert the result to an
absolute name using `expand-file-name` (see [Functions that Expand Filenames](#File-Name-Expansion)). This function converts absolute file names to relative
names:

Function: **file-relative-name** filename &optional directory [¶](#index-file_002drelative_002dname)
:   This function tries to return a relative name that is equivalent to
    filename, assuming the result will be interpreted relative to
    directory (an absolute directory name or directory file name).
    If directory is omitted or `nil`, it defaults to the
    current buffer’s default directory.

    On some operating systems, an absolute file name begins with a device
    name. On such systems, filename has no relative equivalent based
    on directory if they start with two different device names. In
    this case, `file-relative-name` returns filename in absolute
    form.

    ```
    (file-relative-name "/foo/bar" "/foo/")
         ⇒ "bar"
    (file-relative-name "/foo/bar" "/hack/")
         ⇒ "../foo/bar"
    ```

A file name that is an empty string stands for the current buffer’s
default directory.

---

Next: [Functions that Expand Filenames](#File-Name-Expansion), Previous: [Absolute and Relative File Names](#Relative-File-Names), Up: [File Names](#File-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.9.3 Directory Names [¶](#Directory-Names-1)

A *directory name* is a string that must name a directory if it
names any file at all. A directory is actually a kind of file, and it
has a file name (called the *directory file name*), which is
related to the directory name but is typically not identical. (This
is not quite the same as the usual POSIX terminology.) These two
names for the same entity are related by a syntactic transformation.
On GNU and other POSIX-like systems, this is simple: to obtain a
directory name, append a ‘`/`’ to a directory file name that does
not already end in ‘`/`’. On MS-DOS the relationship is more
complicated.

The difference between a directory name and a directory file name is
subtle but crucial. When an Emacs variable or function argument is
described as being a directory name, a directory file name is not
acceptable. When `file-name-directory` returns a string, that is
always a directory name.

The following two functions convert between directory names and
directory file names. They do nothing special with environment
variable substitutions such as ‘`$HOME`’, and the constructs
‘`~`’, ‘`.`’ and ‘`..`’.

Function: **file-name-as-directory** filename [¶](#index-file_002dname_002das_002ddirectory)
:   This function returns a string representing filename in a form
    that the operating system will interpret as the name of a directory (a
    directory name). On most systems, this means appending a slash to the
    string (if it does not already end in one).

    ```
    (file-name-as-directory "~rms/lewis")
         ⇒ "~rms/lewis/"
    ```

Function: **directory-name-p** filename [¶](#index-directory_002dname_002dp)
:   This function returns non-`nil` if filename ends with a
    directory separator character. This is the forward slash ‘`/`’ on
    GNU and other POSIX-like systems; MS-Windows and MS-DOS recognize both
    the forward slash and the backslash ‘`\`’ as directory separators.

Function: **directory-file-name** dirname [¶](#index-directory_002dfile_002dname)
:   This function returns a string representing dirname in a form
    that the operating system will interpret as the name of a file (a
    directory file name). On most systems, this means removing the final
    directory separators from the string, unless the string consists
    entirely of directory separators.

    ```
    (directory-file-name "~lewis/")
         ⇒ "~lewis"
    ```

Function: **file-name-concat** directory &rest components [¶](#index-file_002dname_002dconcat)
:   Concatenate components to directory, inserting a slash
    before the components if directory or the preceding component
    didn’t end with a slash.

    ```
    (file-name-concat "/tmp" "foo")
         ⇒ "/tmp/foo"
    ```

    A directory or components that are `nil` or the empty
    string are ignored—they are filtered out first and do not affect the
    results in any way.

    This is almost the same as using `concat`, but dirname (and
    the non-final components) may or may not end with slash characters,
    and this function will not double those characters.

    In most cases, one or more calls to `expand-file-name` (see [Functions that Expand Filenames](#File-Name-Expansion) are better suited for the job of generating file names
    with leading directories than this function. Use this function only if
    some of the special features of `expand-file-name` get in the way
    of what your program needs to do. For example, the special handling by
    `expand-file-name` of `~`, `~user`, and `nil`,
    or the removal of `.` and `..` might not be what you want.

To convert a directory name to its abbreviation, use this
function:

Function: **abbreviate-file-name** filename [¶](#index-abbreviate_002dfile_002dname)
:   This function returns an abbreviated form of filename. It
    applies the abbreviations specified in `directory-abbrev-alist`
    (see [File Aliases](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Aliases) in The GNU Emacs Manual),
    then substitutes ‘`~`’ for the user’s home directory if the
    argument names a file in the home directory or one of its
    subdirectories. If the home directory is a root directory, it is not
    replaced with ‘`~`’, because this does not make the result shorter
    on many systems.

    You can use this function for directory names and for file names,
    because it recognizes abbreviations even as part of the name.

Function: **file-name-parent-directory** filename [¶](#index-file_002dname_002dparent_002ddirectory)
:   This function returns the directory name of the parent directory of
    filename. If filename is at the root directory of the
    filesystem, it returns `nil`. A relative filename is
    assumed to be relative to `default-directory`, and the return
    value will also be relative in that case. If the return value is
    non-`nil`, it ends in a slash.

---

Next: [Generating Unique File Names](#Unique-File-Names), Previous: [Directory Names](#Directory-Names), Up: [File Names](#File-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.9.4 Functions that Expand Filenames [¶](#Functions-that-Expand-Filenames)

*Expanding* a file name means converting a relative file name to
an absolute one. Since this is done relative to a default directory,
you must specify the default directory as well as the file name
to be expanded. It also involves expanding abbreviations like
`~/`
(see [abbreviate-file-name](#abbreviate_002dfile_002dname)),
and eliminating redundancies like `./` and `name/../`.

Function: **expand-file-name** filename &optional directory [¶](#index-expand_002dfile_002dname)
:   This function converts filename to an absolute file name. If
    directory is supplied, it is the default directory to start with
    if filename is relative and does not start with ‘`~`’.
    (The value of directory should
    itself be an absolute directory name or directory file name; it may
    start with ‘`~`’.) Otherwise, the current buffer’s value of
    `default-directory` is used. For example:

    ```
    (expand-file-name "foo")
         ⇒ "/xcssun/users/rms/lewis/foo"
    ```

    ```
    (expand-file-name "../foo")
         ⇒ "/xcssun/users/rms/foo"
    ```

    ```
    (expand-file-name "foo" "/usr/spool/")
         ⇒ "/usr/spool/foo"
    ```

    If the part of filename before the first slash is
    ‘`~`’, it expands to your home directory, which is typically
    specified by the value of the `HOME` environment variable
    (see [General Variables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#General-Variables) in The GNU Emacs Manual).
    If the part before the first
    slash is ‘`~user`’ and if user is a valid login name,
    it expands to user’s home directory.
    If you do not want this expansion for a relative filename that
    might begin with a literal ‘`~`’, you can use `(concat
    (file-name-as-directory directory) filename)` instead of
    `(expand-file-name filename directory)`.

    File names containing ‘`.`’ or ‘`..`’ are simplified to their
    canonical form:

    ```
    (expand-file-name "bar/../foo")
         ⇒ "/xcssun/users/rms/lewis/foo"
    ```

    In some cases, a leading ‘`..`’ component can remain in the output:

    ```
    (expand-file-name "../home" "/")
         ⇒ "/../home"
    ```

    This is for the sake of filesystems that have the concept of a
    superroot above the root directory `/`. On other filesystems,
    `/../` is interpreted exactly the same as `/`.

    Expanding `.` or the empty string returns the default directory:

    ```
    (expand-file-name "." "/usr/spool/")
         ⇒ "/usr/spool"
    (expand-file-name "" "/usr/spool/")
         ⇒ "/usr/spool"
    ```

    Note that `expand-file-name` does *not* expand environment
    variables; only `substitute-in-file-name` does that:

    ```
    (expand-file-name "$HOME/foo")
         ⇒ "/xcssun/users/rms/lewis/$HOME/foo"
    ```

    Note also that `expand-file-name` does not follow symbolic links
    at any level. This results in a difference between the way
    `file-truename` and `expand-file-name` treat ‘`..`’.
    Assuming that ‘`/tmp/bar`’ is a symbolic link to the directory
    ‘`/tmp/foo/bar`’ we get:

    ```
    (file-truename "/tmp/bar/../myfile")
         ⇒ "/tmp/foo/myfile"
    ```

    ```
    (expand-file-name "/tmp/bar/../myfile")
         ⇒ "/tmp/myfile"
    ```

    If you may need to follow symbolic links preceding ‘`..`’, you
    should make sure to call `file-truename` without prior direct or
    indirect calls to `expand-file-name`. See [Truenames](#Truenames).

Variable: **default-directory** [¶](#index-default_002ddirectory)
:   The value of this buffer-local variable is the default directory for the
    current buffer. It should be an absolute directory name; it may start
    with ‘`~`’. This variable is buffer-local in every buffer.

    `expand-file-name` uses the default directory when its second
    argument is `nil`.

    The value is always a string ending with a slash.

    ```
    default-directory
         ⇒ "/user/lewis/manual/"
    ```

Function: **substitute-in-file-name** filename [¶](#index-substitute_002din_002dfile_002dname)
:   This function replaces environment variable references in
    filename with the environment variable values. Following
    standard Unix shell syntax, ‘`$`’ is the prefix to substitute an
    environment variable value. If the input contains ‘`$$`’, that is
    converted to ‘`$`’; this gives the user a way to quote a
    ‘`$`’.

    The environment variable name is the series of alphanumeric characters
    (including underscores) that follow the ‘`$`’. If the character following
    the ‘`$`’ is a ‘`{`’, then the variable name is everything up to the
    matching ‘`}`’.

    Calling `substitute-in-file-name` on output produced by
    `substitute-in-file-name` tends to give incorrect results. For
    instance, use of ‘`$$`’ to quote a single ‘`$`’ won’t work
    properly, and ‘`$`’ in an environment variable’s value could lead
    to repeated substitution. Therefore, programs that call this function
    and put the output where it will be passed to this function need to
    double all ‘`$`’ characters to prevent subsequent incorrect
    results.

    Here we assume that the environment variable `HOME`, which holds
    the user’s home directory, has value ‘`/xcssun/users/rms`’.

    ```
    (substitute-in-file-name "$HOME/foo")
         ⇒ "/xcssun/users/rms/foo"
    ```

    After substitution, if a ‘`~`’ or a ‘`/`’ appears immediately
    after another ‘`/`’, the function discards everything before it (up
    through the immediately preceding ‘`/`’).

    ```
    (substitute-in-file-name "bar/~/foo")
         ⇒ "~/foo"
    ```

    ```
    (substitute-in-file-name "/usr/local/$HOME/foo")
         ⇒ "/xcssun/users/rms/foo"
         ;; /usr/local/ has been discarded.
    ```

Sometimes, it is not desired to expand file names. In such cases,
the file name can be quoted to suppress the expansion, and to handle
the file name literally. Quoting happens by prefixing the file name
with ‘`/:`’.

Macro: **file-name-quote** name [¶](#index-file_002dname_002dquote)
:   This macro adds the quotation prefix ‘`/:`’ to the file name.
    For a local file name, it prefixes name with ‘`/:`’.
    If name is a remote file name, the local part of name
    (see [Making Certain File Names “Magic”](#Magic-File-Names)) is quoted. If name is already a
    quoted file name, name is returned unchanged.

    ```
    (substitute-in-file-name (file-name-quote "bar/~/foo"))
         ⇒ "/:bar/~/foo"
    ```

    ```

    ```

    ```
    (substitute-in-file-name (file-name-quote "/ssh:host:bar/~/foo"))
         ⇒ "/ssh:host:/:bar/~/foo"
    ```

    The macro cannot be used to suppress file name handlers from magic
    file names (see [Making Certain File Names “Magic”](#Magic-File-Names)).

Macro: **file-name-unquote** name [¶](#index-file_002dname_002dunquote)
:   This macro removes the quotation prefix ‘`/:`’ from the file
    name, if any. If name is a remote file name, the local
    part of name is unquoted.

Macro: **file-name-quoted-p** name [¶](#index-file_002dname_002dquoted_002dp)
:   This macro returns non-`nil`, when name is quoted with the
    prefix ‘`/:`’. If name is a remote file name, the local part
    of name is checked.

---

Next: [File Name Completion](#File-Name-Completion), Previous: [Functions that Expand Filenames](#File-Name-Expansion), Up: [File Names](#File-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.9.5 Generating Unique File Names [¶](#Generating-Unique-File-Names)

Some programs need to write temporary files. Here is the usual way to
construct a name for such a file:

```
(make-temp-file name-of-application)
```

The job of `make-temp-file` is to prevent two different users or
two different jobs from trying to use the exact same file name.

Function: **make-temp-file** prefix &optional dir-flag suffix text [¶](#index-make_002dtemp_002dfile)
:   This function creates a temporary file and returns its name. Emacs
    creates the temporary file’s name by adding to prefix some
    random characters that are different in each Emacs job. The result is
    guaranteed to be a newly created file, containing text if that’s
    given as a string and empty otherwise. On MS-DOS, this function can
    truncate prefix to fit into the 8+3 file-name limits. If
    prefix is a relative file name, it is expanded against
    `temporary-file-directory`.

    ```
    (make-temp-file "foo")
         ⇒ "/tmp/foo232J6v"
    ```

    When `make-temp-file` returns, the file has been created and is
    empty. At that point, you should write the intended contents into the
    file.

    If dir-flag is non-`nil`, `make-temp-file` creates an
    empty directory instead of an empty file. It returns the file name,
    not the directory name, of that directory. See [Directory Names](#Directory-Names).

    If suffix is non-`nil`, `make-temp-file` adds it at
    the end of the file name.

    If text is a string, `make-temp-file` inserts it in the file.

    On Posix systems, Emacs creates the file with permissions that limit its
    access to the current user.

    To prevent conflicts among different libraries running in the same
    Emacs, each Lisp program that uses `make-temp-file` should have its
    own prefix. The number added to the end of prefix
    distinguishes between the same application running in different Emacs
    jobs. Additional added characters permit a large number of distinct
    names even in one Emacs job.

The default directory for temporary files is controlled by the
variable `temporary-file-directory`. This variable gives the user
a uniform way to specify the directory for all temporary files. Some
programs use `small-temporary-file-directory` instead, if that is
non-`nil`. To use it, you should expand the prefix against
the proper directory before calling `make-temp-file`.

User Option: **temporary-file-directory** [¶](#index-temporary_002dfile_002ddirectory-1)
:   This variable specifies the directory name for creating temporary files.
    Its value should be a directory name (see [Directory Names](#Directory-Names)), but it
    is good for Lisp programs to cope if the value is a directory’s file
    name instead. Using the value as the second argument to
    `expand-file-name` is a good way to achieve that.

    The default value is determined in a reasonable way for your operating
    system; it is based on the `TMPDIR`, `TMP` and `TEMP`
    environment variables, with a fall-back to a system-dependent name if
    none of these variables is defined.

    Even if you do not use `make-temp-file` to create the temporary
    file, you should still use this variable to decide which directory to
    put the file in. However, if you expect the file to be small, you
    should use `small-temporary-file-directory` first if that is
    non-`nil`.

User Option: **small-temporary-file-directory** [¶](#index-small_002dtemporary_002dfile_002ddirectory)
:   This variable specifies the directory name for
    creating certain temporary files, which are likely to be small.

    If you want to write a temporary file which is likely to be small, you
    should compute the directory like this:

    ```
    (make-temp-file
      (expand-file-name prefix
                        (or small-temporary-file-directory
                            temporary-file-directory)))
    ```

Function: **make-temp-name** base-name [¶](#index-make_002dtemp_002dname)
:   This function generates a string that might be a unique file
    name. The name starts with base-name, and has several random
    characters appended to it, which are different in each Emacs job. It
    is like `make-temp-file` except that (i) it just constructs a
    name and does not create a file, (ii) base-name should be an
    absolute file name that is not magic, and (iii) if the returned file
    name is magic, it might name an existing file. See [Making Certain File Names “Magic”](#Magic-File-Names).

    **Warning:** In most cases, you should not use this function; use
    `make-temp-file` instead! This function is susceptible to a race
    condition, between the `make-temp-name` call and the creation of
    the file, which in some cases may cause a security hole.

Sometimes, it is necessary to create a temporary file on a remote host
or a mounted directory. The following two functions support this.

Function: **make-nearby-temp-file** prefix &optional dir-flag suffix [¶](#index-make_002dnearby_002dtemp_002dfile)
:   This function is similar to `make-temp-file`, but it creates a
    temporary file as close as possible to `default-directory`. If
    prefix is a relative file name, and `default-directory` is
    a remote file name or located on a mounted file systems, the temporary
    file is created in the directory returned by the function
    `temporary-file-directory`. Otherwise, the function
    `make-temp-file` is used. prefix, dir-flag and
    suffix have the same meaning as in `make-temp-file`.

    ```
    (let ((default-directory "/ssh:remotehost:"))
      (make-nearby-temp-file "foo"))
         ⇒ "/ssh:remotehost:/tmp/foo232J6v"
    ```

Function: **temporary-file-directory** [¶](#index-temporary_002dfile_002ddirectory)
:   The directory for writing temporary files via
    `make-nearby-temp-file`. In case of a remote
    `default-directory`, this is a directory for temporary files on
    that remote host. If such a directory does not exist, or
    `default-directory` ought to be located on a mounted file system
    (see `mounted-file-systems`), the function returns
    `default-directory`. For a non-remote and non-mounted
    `default-directory`, the value of the variable
    `temporary-file-directory` is returned.

In order to extract the local part of the file’s name of a temporary
file, use `file-local-name` (see [Making Certain File Names “Magic”](#Magic-File-Names)).

---

Next: [Standard File Names](#Standard-File-Names), Previous: [Generating Unique File Names](#Unique-File-Names), Up: [File Names](#File-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.9.6 File Name Completion [¶](#File-Name-Completion-1)

This section describes low-level subroutines for completing a file
name. For higher level functions, see [Reading File Names](#Reading-File-Names).

Function: **file-name-all-completions** partial-filename directory [¶](#index-file_002dname_002dall_002dcompletions)
:   This function returns a list of all possible completions for a file
    whose name starts with partial-filename in directory
    directory. The order of the completions is the order of the files
    in the directory, which is unpredictable and conveys no useful
    information.

    The argument partial-filename must be a file name containing no
    directory part and no slash (or backslash on some systems). The current
    buffer’s default directory is prepended to directory, if
    directory is not absolute.

    In the following example, suppose that `~rms/lewis` is the current
    default directory, and has five files whose names begin with ‘`f`’:
    `foo`, `file~`, `file.c`, `file.c.~1~`, and
    `file.c.~2~`.

    ```
    (file-name-all-completions "f" "")
         ⇒ ("foo" "file~" "file.c.~2~"
                    "file.c.~1~" "file.c")
    ```

    ```

    ```

    ```
    (file-name-all-completions "fo" "")
         ⇒ ("foo")
    ```

Function: **file-name-completion** filename directory &optional predicate [¶](#index-file_002dname_002dcompletion)
:   This function completes the file name filename in directory
    directory. It returns the longest prefix common to all file names
    in directory directory that start with filename. If
    predicate is non-`nil` then it ignores possible completions
    that don’t satisfy predicate, after calling that function
    with one argument, the expanded absolute file name.

    If only one match exists and filename matches it exactly, the
    function returns `t`. The function returns `nil` if directory
    directory contains no name starting with filename.

    In the following example, suppose that the current default directory
    has five files whose names begin with ‘`f`’: `foo`,
    `file~`, `file.c`, `file.c.~1~`, and
    `file.c.~2~`.

    ```
    (file-name-completion "fi" "")
         ⇒ "file"
    ```

    ```

    ```

    ```
    (file-name-completion "file.c.~1" "")
         ⇒ "file.c.~1~"
    ```

    ```

    ```

    ```
    (file-name-completion "file.c.~1~" "")
         ⇒ t
    ```

    ```

    ```

    ```
    (file-name-completion "file.c.~3" "")
         ⇒ nil
    ```

User Option: **completion-ignored-extensions** [¶](#index-completion_002dignored_002dextensions)
:   `file-name-completion` usually ignores file names that end in any
    string in this list. It does not ignore them when all the possible
    completions end in one of these suffixes. This variable has no effect
    on `file-name-all-completions`.

    A typical value might look like this:

    ```
    completion-ignored-extensions
         ⇒ (".o" ".elc" "~" ".dvi")
    ```

    If an element of `completion-ignored-extensions` ends in a slash
    ‘`/`’, it signals a directory. The elements which do *not* end
    in a slash will never match a directory; thus, the above value will not
    filter out a directory named `foo.elc`.

---

Previous: [File Name Completion](#File-Name-Completion), Up: [File Names](#File-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.9.7 Standard File Names [¶](#Standard-File-Names-1)

Sometimes, an Emacs Lisp program needs to specify a standard file
name for a particular use—typically, to hold configuration data
specified by the current user. Usually, such files should be located
in the directory specified by `user-emacs-directory`, which is
typically `~/.config/emacs/` or `~/.emacs.d/` by default (see [How Emacs Finds Your Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Find-Init) in The GNU Emacs Manual).
For example, abbrev definitions are stored by default in
`~/.config/emacs/abbrev_defs` or `~/.emacs.d/abbrev_defs`.
The easiest way to specify such a file name is to use the function
`locate-user-emacs-file`.

Function: **locate-user-emacs-file** base-name &optional old-name [¶](#index-locate_002duser_002demacs_002dfile)
:   This function returns an absolute file name for an Emacs-specific
    configuration or data file. The argument `base-name` should be a
    relative file name. The return value is the absolute name of a file
    in the directory specified by `user-emacs-directory`; if that
    directory does not exist, this function creates it.

    If the optional argument old-name is non-`nil`, it
    specifies a file in the user’s home directory,
    `~/old-name`. If such a file exists, the return value is
    the absolute name of that file, instead of the file specified by
    base-name. This argument is intended to be used by Emacs
    packages to provide backward compatibility. For instance, prior to
    the introduction of `user-emacs-directory`, the abbrev file was
    located in `~/.abbrev_defs`. Here is the definition of
    `abbrev-file-name`:

    ```
    (defcustom abbrev-file-name
      (locate-user-emacs-file "abbrev_defs" ".abbrev_defs")
      "Default name of file from which to read abbrevs."
      ...
      :type 'file)
    ```

A lower-level function for standardizing file names, which
`locate-user-emacs-file` uses as a subroutine, is
`convert-standard-filename`.

Function: **convert-standard-filename** filename [¶](#index-convert_002dstandard_002dfilename)
:   This function returns a file name based on filename, which fits
    the conventions of the current operating system.

    On GNU and other POSIX-like systems, this simply returns filename.
    On other operating systems, it may enforce system-specific file name
    conventions; for example, on MS-DOS this function performs a variety
    of changes to enforce MS-DOS file name limitations, including
    converting any leading ‘`.`’ to ‘`_`’ and truncating to three
    characters after the ‘`.`’.

    The recommended way to use this function is to specify a name which
    fits the conventions of GNU and Unix systems, and pass it to
    `convert-standard-filename`.

---

Next: [Creating, Copying and Deleting Directories](#Create_002fDelete-Dirs), Previous: [File Names](#File-Names), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.10 Contents of Directories [¶](#Contents-of-Directories-1)

A directory is a kind of file that contains other files entered under
various names. Directories are a feature of the file system.

Emacs can list the names of the files in a directory as a Lisp list,
or display the names in a buffer using the `ls` shell command. In
the latter case, it can optionally display information about each file,
depending on the options passed to the `ls` command.

Function: **directory-files** directory &optional full-name match-regexp nosort count [¶](#index-directory_002dfiles)
:   This function returns a list of the names of the files in the directory
    directory. By default, the list is in alphabetical order.

    If full-name is non-`nil`, the function returns the files’
    absolute file names. Otherwise, it returns the names relative to
    the specified directory.

    If match-regexp is non-`nil`, this function returns only
    those file names whose non-directory part contain a match for that
    regular expression—the other file names are excluded from the list.
    On case-insensitive filesystems, the regular expression matching is
    case-insensitive.

    If nosort is non-`nil`, `directory-files` does not sort
    the list, so you get the file names in no particular order. Use this if
    you want the utmost possible speed and don’t care what order the files
    are processed in. If the order of processing is visible to the user,
    then the user will probably be happier if you do sort the names.

    If count is non-`nil`, the function will return names of
    first count number of files, or names of all files, whichever
    occurs first. count has to be an integer greater than zero.

    ```
    (directory-files "~lewis")
         ⇒ ("#foo#" "#foo.el#" "." ".."
             "dired-mods.el" "files.texi"
             "files.texi.~1~")
    ```

    An error is signaled if directory is not the name of a directory
    that can be read.

Function: **directory-empty-p** directory [¶](#index-directory_002dempty_002dp)
:   This utility function returns `t` if given directory is an
    accessible directory and it does not contain any files, i.e., is an
    empty directory. It will ignore ‘`.`’ and ‘`..`’ on systems
    that return them as files in a directory.

    Symbolic links to directories count as directories.
    See file-symlink-p to distinguish symlinks.

Function: **directory-files-recursively** directory regexp &optional include-directories predicate follow-symlinks [¶](#index-directory_002dfiles_002drecursively)
:   Return all files under directory whose names match regexp.
    This function searches the specified directory and its
    sub-directories, recursively, for files whose basenames (i.e., without
    the leading directories) match the specified regexp, and returns
    a list of the absolute file names of the matching files
    (see [absolute file names](#Relative-File-Names)). The file names
    are returned in depth-first order, meaning that files in some
    sub-directory are returned before the files in its parent directory.
    In addition, matching files found in each subdirectory are sorted
    alphabetically by their basenames. By default, directories whose
    names match regexp are omitted from the list, but if the
    optional argument include-directories is non-`nil`, they
    are included.

    By default, all subdirectories are descended into. If predicate
    is `t`, errors when trying to descend into a subdirectory (for
    instance, if it’s not readable by this user) are ignored. If it’s
    neither `nil` nor `t`, it should be a function that takes
    one parameter (the subdirectory name) and should return non-`nil`
    if the directory is to be descended into.

    Symbolic links to subdirectories are not followed by default, but if
    follow-symlinks is non-`nil`, they are followed.

Function: **locate-dominating-file** file name [¶](#index-locate_002ddominating_002dfile)
:   Starting at file, go up the directory tree hierarchy looking for
    the first directory where name, a string, exists, and return that
    directory. If file is a file, its directory will serve as the
    starting point for the search; otherwise file should be a
    directory from which to start. The function looks in the starting
    directory, then in its parent, then in its parent’s parent, etc.,
    until it either finds a directory with name or reaches the root
    directory of the filesystem without finding name – in the
    latter case the function returns `nil`.

    The argument `name` can also be a predicate function. The
    predicate is called for every directory examined by the function,
    starting from file (even if file is not a directory). It
    is called with one argument (the file or directory) and should return
    non-`nil` if that directory is the one it is looking for.

Function: **file-in-directory-p** file dir [¶](#index-file_002din_002ddirectory_002dp)
:   This function returns `t` if file is a file in directory
    dir, or in a subdirectory of dir. It also returns
    `t` if file and dir are the same directory. It
    compares the truenames of the two directories. If dir does not
    name an existing directory, the return value is `nil`.

Function: **directory-files-and-attributes** directory &optional full-name match-regexp nosort id-format count [¶](#index-directory_002dfiles_002dand_002dattributes)
:   This is similar to `directory-files` in deciding which files
    to report on and how to report their names. However, instead
    of returning a list of file names, it returns for each file a
    list `(filename . attributes)`, where attributes
    is what `file-attributes` returns for that file.
    The optional argument id-format has the same meaning as the
    corresponding argument to `file-attributes` (see [Definition of file-attributes](#Definition-of-file_002dattributes)).

Constant: **directory-files-no-dot-files-regexp** [¶](#index-directory_002dfiles_002dno_002ddot_002dfiles_002dregexp)
:   This regular expression matches any file name except ‘`.`’ and
    ‘`..`’. More precisely, it matches parts of any nonempty string
    except those two. It is useful as the match-regexp argument to
    `directory-files` and `directory-files-and-attributes`:

    ```
    (directory-files "/foo" nil directory-files-no-dot-files-regexp)
    ```

    returns `nil`, if directory ‘`/foo`’ is empty.

Function: **file-expand-wildcards** pattern &optional full regexp [¶](#index-file_002dexpand_002dwildcards)
:   This function expands the wildcard pattern pattern, returning
    a list of file names that match it.

    pattern is, by default, a “glob”/wildcard string, e.g.,
    ‘`"/tmp/*.png"`’ or ‘`"/*/*/foo.png"`’, but can also be a
    regular expression if the optional regexp parameter is non-`nil`.
    In any case, the matches are applied per sub-directory, so a match
    can’t span a parent/sub directory.

    If pattern is written as an absolute file name,
    the values are absolute also.

    If pattern is written as a relative file name, it is interpreted
    relative to the current default directory. The file names returned are
    normally also relative to the current default directory. However, if
    full is non-`nil`, they are absolute.

Function: **insert-directory** file switches &optional wildcard full-directory-p [¶](#index-insert_002ddirectory)
:   This function inserts (in the current buffer) a directory listing for
    directory file, formatted with `ls` according to
    switches. It leaves point after the inserted text.
    switches may be a string of options, or a list of strings
    representing individual options.

    The argument file may be either a directory or a file
    specification including wildcard characters. If wildcard is
    non-`nil`, that means treat file as a file specification with
    wildcards.

    If full-directory-p is non-`nil`, that means the directory
    listing is expected to show the full contents of a directory. You
    should specify `t` when file is a directory and switches do
    not contain ‘`-d`’. (The ‘`-d`’ option to `ls` says to
    describe a directory itself as a file, rather than showing its
    contents.)

    On most systems, this function works by running a directory listing
    program whose name is in the variable `insert-directory-program`.
    If wildcard is non-`nil`, it also runs the shell specified by
    `shell-file-name`, to expand the wildcards.

    MS-DOS and MS-Windows systems usually lack the standard Unix program
    `ls`, so this function emulates the standard Unix program `ls`
    with Lisp code.

    As a technical detail, when switches contains the long
    ‘`--dired`’ option, `insert-directory` treats it specially,
    for the sake of dired. However, the normally equivalent short
    ‘`-D`’ option is just passed on to `insert-directory-program`,
    as any other option.

User Option: **insert-directory-program** [¶](#index-insert_002ddirectory_002dprogram)
:   This user option specifies the program to run to generate a directory
    listing for the function `insert-directory`. It is ignored on
    systems that generate the listing with Lisp code.

---

Next: [Making Certain File Names “Magic”](#Magic-File-Names), Previous: [Contents of Directories](#Contents-of-Directories), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.11 Creating, Copying and Deleting Directories [¶](#Creating_002c-Copying-and-Deleting-Directories)

Most Emacs Lisp file-manipulation functions get errors when used on
files that are directories. For example, you cannot delete a directory
with `delete-file`. These special functions exist to create and
delete directories.

Command: **make-directory** dirname &optional parents [¶](#index-make_002ddirectory)
:   This command creates a directory named dirname. If
    parents is non-`nil`, as is always the case in an
    interactive call, that means to create the parent directories first,
    if they don’t already exist.
    As a function, `make-directory` returns non-`nil` if dirname
    already exists as a directory and parents is non-`nil`,
    and returns `nil` if it successfully created dirname.
    `mkdir` is an alias for this.

Command: **make-empty-file** filename &optional parents [¶](#index-make_002dempty_002dfile)
:   This command creates an empty file named filename.
    As `make-directory`, this command creates parent directories
    if parents is non-`nil`.
    If filename already exists, this command signals an error.

Command: **copy-directory** dirname newname &optional keep-time parents copy-contents [¶](#index-copy_002ddirectory)
:   This command copies the directory named dirname to
    newname. If newname is a directory name,
    dirname will be copied to a subdirectory there.
    See [Directory Names](#Directory-Names).

    It always sets the file modes of the copied files to match the
    corresponding original file.

    The third argument keep-time non-`nil` means to preserve the
    modification time of the copied files. A prefix arg makes
    keep-time non-`nil`.

    The fourth argument parents says whether to
    create parent directories if they don’t exist. Interactively,
    this happens by default.

    The fifth argument copy-contents, if non-`nil`, means to
    copy the contents of dirname directly into newname if the
    latter is a directory name, instead of copying dirname into
    it as a subdirectory.

Command: **delete-directory** dirname &optional recursive trash [¶](#index-delete_002ddirectory)
:   This command deletes the directory named dirname. The function
    `delete-file` does not work for files that are directories; you
    must use `delete-directory` for them. If recursive is
    `nil`, and the directory contains any files,
    `delete-directory` signals an error.
    If recursive is non-`nil`, there is no error merely because the
    directory or its files are deleted by some other process before
    `delete-directory` gets to them.

    `delete-directory` only follows symbolic links at the level of
    parent directories.

    If the optional argument trash is non-`nil` and the
    variable `delete-by-moving-to-trash` is non-`nil`, this
    command moves the file into the system Trash instead of deleting it.
    See [Miscellaneous File Operations](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Misc-File-Ops) in The GNU
    Emacs Manual. When called interactively, trash is `t` if
    no prefix argument is given, and `nil` otherwise.

---

Next: [File Format Conversion](#Format-Conversion), Previous: [Creating, Copying and Deleting Directories](#Create_002fDelete-Dirs), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
