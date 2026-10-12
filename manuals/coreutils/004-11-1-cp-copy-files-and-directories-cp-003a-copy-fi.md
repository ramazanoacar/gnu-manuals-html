### 11.1 `cp`: Copy files and directories [¶](#cp_003a-Copy-files-and-directories)

`cp` copies files (or, optionally, directories). The copy is
completely independent of the original. You can either copy one file to
another, or copy arbitrarily many files to a destination directory.
Synopses:

```
cp [option]... [-T] source dest
cp [option]... source... directory
cp [option]... -t directory source...
```

* If two file names are given, `cp` copies the first file to the
  second.
* If the `--target-directory` (`-t`) option is given, or
  failing that if the last file is a directory and the
  `--no-target-directory` (`-T`) option is not given,
  `cp` copies each source file to the specified directory,
  using the sources’ names.

Generally, files are written just as they are read. For exceptions,
see the `--sparse` option below.

By default, `cp` does not copy directories. However, the
`-R`, `-a`, and `-r` options cause `cp` to
copy recursively by descending into source directories and copying files
to corresponding destination directories.

When copying from a symbolic link, `cp` normally follows the
link only when not copying recursively or when `--link`
(`-l`) is used. This default can be overridden with the
`--archive` (`-a`), `-d`, `--dereference`
(`-L`), `--no-dereference` (`-P`), and
`-H` options. If more than one of these options is specified,
the last one silently overrides the others.

When copying to a symbolic link, `cp` follows the
link only when it refers to an existing regular file.
However, when copying to a dangling symbolic link, `cp`
refuses by default, and fails with a diagnostic, since the operation
is inherently dangerous. This behavior is contrary to historical
practice and to POSIX.
Set `POSIXLY_CORRECT` to make `cp` attempt to create
the target of a dangling destination symlink, in spite of the possible risk.
Also, when an option like
`--backup` or `--link` acts to rename or remove the
destination before copying, `cp` renames or removes the
symbolic link rather than the file it points to.

By default, `cp` copies the contents of special files only
when not copying recursively. This default can be overridden with the
`--copy-contents` option.

`cp` generally refuses to copy a file onto itself, with the
following exception: if `--force --backup` is specified with
source and dest identical, and referring to a regular file,
`cp` will make a backup file, either regular or numbered, as
specified in the usual ways (see [Backup options](#Backup-options)). This is useful when
you simply want to make a backup of an existing file before changing it.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-a`’ [¶](#index-_002da-7)

‘`--archive`’
:   Preserve as much as possible of the structure and attributes of the
    original files in the copy (but do not attempt to preserve internal
    directory structure; i.e., ‘`ls -U`’ may list the entries in a copied
    directory in a different order).
    Try to preserve SELinux security context and extended attributes (xattr),
    but ignore any failure to do that and print no corresponding diagnostic.
    Equivalent to `-dR --preserve=all` with the reduced diagnostics.

‘`--attributes-only`’ [¶](#index-_002d_002dattributes_002donly)
:   Copy only the specified attributes of the source file to the destination.
    If the destination already exists, do not alter its contents.
    See the `--preserve` option for controlling which attributes to copy.

‘`-b`’ [¶](#index-_002db-14)

‘`--backup[=method]`’
:   See [Backup options](#Backup-options).
    Make a backup of each file that would otherwise be overwritten or removed.
    As a special case, `cp` makes a backup of source when the force
    and backup options are given and source and dest are the same
    name for an existing, regular file. One useful application of this
    combination of options is this tiny Bourne shell script:

    ```
    #!/bin/sh
    # Usage: backup FILE...
    # Create a GNU-style backup of each listed FILE.
    fail=0
    for i; do
      cp --backup --force --preserve=all -- "$i" "$i" || fail=1
    done
    exit $fail
    ```

‘`--copy-contents`’ [¶](#index-directories_002c-copying-recursively)
:   If copying recursively, copy the contents of any special files (e.g.,
    FIFOs and device files) as if they were regular files. This means
    trying to read the data in each source file and writing it to the
    destination. It is usually a mistake to use this option, as it
    normally has undesirable effects on special files like FIFOs and the
    ones typically found in the `/dev` directory. In most cases,
    `cp -R --copy-contents` will hang indefinitely trying to read
    from FIFOs and special files like `/dev/console`, and it will
    fill up your destination file system if you use it to copy `/dev/zero`.
    This option has no effect unless copying recursively, and it does not
    affect the copying of symbolic links.

‘`-d`’ [¶](#index-_002dd-11)
:   Copy symbolic links as symbolic links rather than copying the files that
    they point to, and preserve hard links between source files in the copies.
    Equivalent to `--no-dereference --preserve=links`.

‘`--debug`’ [¶](#index-_002d_002ddebug-1)
:   Print extra information to stdout, explaining how files are copied.
    This option implies the `--verbose` option.

‘`-f`’ [¶](#index-_002df-10)

‘`--force`’
:   When copying without this option and an existing destination file cannot
    be opened for writing, the copy fails. However, with `--force`,
    when a destination file cannot be opened, `cp` then
    tries to recreate the file by first removing it. The `--force` option
    alone will not remove dangling symlinks.
    When this option is combined with
    `--link` (`-l`) or `--symbolic-link`
    (`-s`), the destination link is replaced, and unless
    `--backup` (`-b`) is also given there is no brief
    moment when the destination does not exist. Also see the
    description of `--remove-destination`.

    This option is independent of the `--interactive` or
    `-i` option: neither cancels the effect of the other.

    This option is ignored when the `--no-clobber` or `-n` option
    is also used.

‘`-H`’ [¶](#index-_002dH-2)
:   If a command line argument specifies a symbolic link, then copy the
    file it points to rather than the symbolic link itself. However,
    copy (preserving its nature) any symbolic link that is encountered
    via recursive traversal.

‘`-i`’ [¶](#index-_002di-11)

‘`--interactive`’
:   When copying a file other than a directory, prompt whether to
    overwrite an existing destination file, and fail if the response
    is not affirmative. The `-i` option overrides
    a previous `-n` option.

‘`-l`’ [¶](#index-_002dl-8)

‘`--link`’
:   Make hard links instead of copies of non-directories.

‘`-L`’ [¶](#index-_002dL-3)

‘`--dereference`’
:   Follow symbolic links when copying from them.
    With this option, `cp` cannot create a symbolic link.
    For example, a symlink (to regular file) in the source tree will be copied to
    a regular file in the destination tree.

‘`-n`’ [¶](#index-_002dn-11)

‘`--no-clobber`’
:   Do not overwrite an existing file; silently skip instead.
    This option overrides a previous `-i` option.
    This option is mutually exclusive with `-b` or `--backup` option.
    This option is deprecated due to having a different exit status from
    other platforms. See also the `--update` option which will
    give more control over how to deal with existing files in the destination,
    and over the exit status in particular.

‘`-P`’ [¶](#index-_002dP-1)

‘`--no-dereference`’
:   Copy symbolic links as symbolic links rather than copying the files that
    they point to. This option affects only symbolic links in the source;
    symbolic links in the destination are always followed if possible.

‘`-p`’ [¶](#index-_002dp-3)

‘`--preserve[=attribute_list]`’
:   Preserve the specified attributes of the original files.
    If specified, the attribute\_list must be a comma-separated list
    of one or more of the following strings:

    ‘`mode`’ [¶](#index-access-control-lists-_0028ACLs_0029)
    :   Preserve attributes relevant to access permissions,
        including file mode bits and (if possible) access control lists (ACLs).
        ACL preservation is system-dependent, and ACLs are not necessarily
        translated when the source and destination are on file systems with
        different ACL formats (e.g., NFSv4 versus POSIX formats).

    ‘`ownership`’
    :   Preserve the owner and group. On most modern systems,
        only users with appropriate privileges may change the owner of a file,
        and ordinary users
        may preserve the group ownership of a file only if they happen to be
        a member of the desired group.

    ‘`timestamps`’
    :   Preserve the times of last access and last modification, when possible.
        On older systems, it is not possible to preserve these attributes
        when the affected file is a symbolic link.
        However, many systems now provide the `utimensat` function,
        which makes it possible even for symbolic links.

    ‘`links`’
    :   Preserve in the destination files
        any links between corresponding source files.
        With `-L` or `-H`, this option can convert
        symbolic links to hard links. For example,

        ```
        $ mkdir c; : > a; ln -s a b; cp -aH a b c; ls -i1 c
        74161745 a
        74161745 b
        ```

        Although `b` is a symlink to regular file `a`,
        the files in the destination directory `c/` are hard-linked.
        Since `-a` implies `--no-dereference` it would copy the symlink,
        but the later `-H` tells `cp` to dereference the command line
        arguments where it then sees two files with the same inode number.
        Then the `--preserve=links` option also implied by `-a`
        will preserve the perceived hard link.

        Here is a similar example that exercises `cp`’s `-L` option:

        ```
        $ mkdir b c; (cd b; : > a; ln -s a b); cp -aL b c; ls -i1 c/b
        74163295 a
        74163295 b
        ```

    ‘`context`’
    :   Preserve SELinux security context of the file, or fail with full diagnostics.

    ‘`xattr`’ [¶](#index-access-control-lists-_0028ACLs_0029-1)
    :   Preserve extended attributes of the file, or fail with full diagnostics.
        If `cp` is built without xattr support, ignore this option.
        If SELinux context, ACLs or Capabilities are implemented using xattrs,
        they are preserved implicitly by this option as well, i.e., even without
        specifying `--preserve=mode` or `--preserve=context`.

    ‘`all`’
    :   Preserve all file attributes.
        Equivalent to specifying all of the above, but with the difference
        that failure to preserve SELinux security context or extended attributes
        does not change `cp`’s exit status. In contrast to `-a`,
        all but ‘`Operation not supported`’ warnings are output.

    Using `--preserve` with no attribute\_list is equivalent
    to `--preserve=mode,ownership,timestamps`.

    In the absence of this option, the permissions of existing destination
    files are unchanged. Each new file is created with the mode of the
    corresponding source file minus the set-user-ID, set-group-ID, and
    sticky bits as the create mode; the operating system then applies either
    the umask or a default ACL, possibly resulting in a more restrictive
    file mode.
    See [File permissions](#File-permissions).

‘`--no-preserve=attribute_list`’ [¶](#index-file-information_002c-preserving)
:   Do not preserve the specified attributes. The attribute\_list
    has the same form as for `--preserve`.

‘`--parents`’ [¶](#index-_002d_002dparents)
:   Form the name of each destination file by appending to the target
    directory a slash and the specified name of the source file. The last
    argument given to `cp` must be the name of an existing directory.
    For example, the command:

    ```
    cp --parents a/b/c existing_dir
    ```

    copies the file `a/b/c` to `existing_dir/a/b/c`, creating
    any missing intermediate directories.

‘`-R`’ [¶](#index-_002dR-3)

‘`-r`’

‘`--recursive`’
:   Copy directories recursively. By default, do not follow symbolic
    links in the source unless used together with the `--link`
    (`-l`) option; see the `--archive` (`-a`), `-d`,
    `--dereference` (`-L`), `--no-dereference`
    (`-P`), and `-H` options. Special files are copied by
    creating a destination file of the same type as the source; see the
    `--copy-contents` option. It is not portable to use
    `-r` to copy symbolic links or special files. On some
    non-GNU systems, `-r` implies the equivalent of
    `-L` and `--copy-contents` for historical reasons.
    Also, it is not portable to use `-R` to copy symbolic links
    unless you also specify `-P`, as POSIX allows
    implementations that dereference symbolic links by default.

‘`--reflink[=when]`’ [¶](#index-_002d_002dreflink_005b_003dwhen_005d)
:   Perform a lightweight, copy-on-write (COW) copy, if supported by the
    file system. Once it has succeeded, beware that the source and destination
    files share the same data blocks as long as they remain unmodified.
    Thus, if an I/O error affects data blocks of one of the files,
    the other suffers the same fate.

    The when value can be one of the following:

    ‘`always`’
    :   If the copy-on-write operation is not supported
        then report the failure for each file and exit with a failure status.
        Plain `--reflink` is equivalent to `--reflink=always`.

    ‘`auto`’
    :   If the copy-on-write operation is not supported then fall back
        to the standard copy behavior.
        This is the default if no `--reflink` option is given.

    ‘`never`’
    :   Disable copy-on-write operation and use the standard copy behavior.

    This option is overridden by the `--link`, `--symbolic-link`
    and `--attributes-only` options, thus allowing it to be used
    to configure the default data copying behavior for `cp`.

‘`--remove-destination`’ [¶](#index-_002d_002dremove_002ddestination)
:   Remove each existing destination file before attempting to open it
    (contrast with `-f` above).

‘`--sparse=when`’ [¶](#index-_002d_002dsparse_003dwhen)
:   A *sparse file* contains *holes* – a sequence of zero bytes that
    does not occupy any file system blocks; the ‘`read`’ system call
    reads these as zeros. This can both save considerable space and
    increase speed, since many binary files contain lots of consecutive zero
    bytes. By default, `cp` detects holes in input source files via a crude
    heuristic and makes the corresponding output file sparse as well.
    Only regular files may be sparse.

    The when value can be one of the following:

    ‘`auto`’
    :   The default behavior: if the input file is sparse, attempt to make
        the output file sparse, too. However, if an output file exists but
        refers to a non-regular file, then do not attempt to make it sparse.

    ‘`always`’
    :   For each sufficiently long sequence of zero bytes in the input file,
        attempt to create a corresponding hole in the output file, even if the
        input file does not appear to be sparse.
        This is useful when the input file resides on a file system
        that does not support sparse files
        (for example, ‘`efs`’ file systems in SGI IRIX 5.3 and earlier),
        but the output file is on a type of file system that does support them.
        Holes may be created only in regular files, so if the destination file
        is of some other type, `cp` does not even try to make it sparse.

    ‘`never`’
    :   Never make the output file sparse.
        This is useful in creating a file for use with the `mkswap` command,
        since such a file must not have any holes.

    For example, with the following alias, `cp` will use the
    minimum amount of space supported by the file system.
    (Older versions of `cp` can also benefit from
    `--reflink=auto` here.)

    ```
    alias cp='cp --sparse=always'
    ```

‘`--strip-trailing-slashes`’ [¶](#index-_002d_002dstrip_002dtrailing_002dslashes)
:   Remove any trailing slashes from each source argument.
    See [Trailing slashes](#Trailing-slashes).

‘`-s`’ [¶](#index-_002ds-16)

‘`--symbolic-link`’
:   Make symbolic links instead of copies of non-directories. All source
    file names must be absolute (starting with ‘`/`’) unless the
    destination files are in the current directory. This option merely
    results in an error message on systems that do not support symbolic links.

‘`-S suffix`’ [¶](#index-_002dS-6)

‘`--suffix=suffix`’
:   Append suffix to each backup file made with `-b`.
    See [Backup options](#Backup-options).

‘`-t directory`’ [¶](#index-_002dt-11)

‘`--target-directory=directory`’
:   Specify the destination directory.
    See [Target directory](#Target-directory).

‘`-T`’ [¶](#index-_002dT-5)

‘`--no-target-directory`’
:   Do not treat the last operand specially when it is a directory or a
    symbolic link to a directory. See [Target directory](#Target-directory).

‘`-u`’ [¶](#index-_002du-6)

‘`--update[=which]`’
:   Do not copy a non-directory that has an existing destination with the
    same or newer modification timestamp; instead, silently skip the file
    without failing. If timestamps are being preserved,
    the comparison is to the source timestamp truncated to the
    resolutions of the destination file system and of the system calls
    used to update timestamps; this avoids duplicate work if several
    ‘`cp -pu`’ commands are executed with the same source and destination.
    This option is ignored if the `-n` or `--no-clobber`
    option is also specified.
    Also, if `--preserve=links` is also specified (like with ‘`cp -au`’
    for example), that will take precedence; consequently, depending on the
    order that files are processed from the source, newer files in the destination
    may be replaced, to mirror hard links in the source.

    which gives more control over which existing files in the
    destination are replaced, and its value can be one of the following:

    ‘`all`’
    :   This is the default operation when an `--update` option is not specified,
        and results in all existing files in the destination being replaced.

    ‘`none`’
    :   This is like the deprecated `--no-clobber` option, where no files in the
        destination are replaced, and also skipping a file does not induce a failure.

    ‘`none-fail`’
    :   This is similar to ‘`none`’, in that no files in the destination
        are replaced, but any skipped files are diagnosed and induce a failure.

    ‘`older`’
    :   This is the default operation when `--update` is specified, and results
        in files being replaced if they’re older than the corresponding source file.

‘`-v`’ [¶](#index-_002dv-7)

‘`--verbose`’
:   Print the name of each file before copying it.

‘`-x`’ [¶](#index-_002dx-3)

‘`--one-file-system`’
:   Skip subdirectories that are on different file systems from the one that
    the copy started on.
    However, mount point directories *are* copied.

‘`-Z`’ [¶](#index-_002dZ-1)

‘`--context[=context]`’
:   Without a specified context, adjust the SELinux security context according
    to the system default type for destination files, similarly to the
    `restorecon` command.
    The long form of this option with a specific context specified,
    will set the context for newly created files only.
    With a specified context, if both SELinux and SMACK are disabled, a warning is
    issued.
    This option is mutually exclusive with the `--preserve=context`
    option, and overrides the `--preserve=all` and `-a` options.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`install`: Copy files and set attributes](#install-invocation), Previous: [`cp`: Copy files and directories](#cp-invocation), Up: [Basic operations](#Basic-operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 11.2 `dd`: Convert and copy a file [¶](#dd_003a-Convert-and-copy-a-file)

`dd` copies input to output with a changeable I/O block size,
while optionally performing conversions on the data. Synopses:

```
dd [operand]...
dd option
```

The only options are `--help` and `--version`.
See [Common options](#Common-options).

By default, `dd` copies standard input to standard output.
To copy, `dd` repeatedly does the following steps in order:

1. Read an input block.
2. If converting via ‘`sync`’, pad as needed to meet the input block size.
   Pad with spaces if converting via ‘`block`’ or ‘`unblock`’, NUL
   bytes otherwise.
3. If ‘`bs=`’ is given and no conversion mentioned in steps (4) or (5)
   is given, output the data as a single block and skip all remaining steps.
4. If the ‘`swab`’ conversion is given, swap each pair of input bytes.
   If the input data length is odd, preserve the last input byte
   (since there is nothing to swap it with).
5. If any of the conversions ‘`swab`’, ‘`block`’, ‘`unblock`’,
   ‘`lcase`’, ‘`ucase`’, ‘`ascii`’, ‘`ebcdic`’ and ‘`ibm`’
   are given, do these conversions. These conversions operate
   independently of input blocking, and might deal with records that span
   block boundaries.
6. Aggregate the resulting data into output blocks of the specified size,
   and output each output block in turn. Do not pad the last output block;
   it can be shorter than usual.

`dd` accepts the following operands,
whose syntax was inspired by the DD (data definition) statement of
OS/360 JCL.

‘`if=file`’ [¶](#index-if)
:   Read from file instead of standard input.

‘`of=file`’ [¶](#index-of)
:   Write to file instead of standard output. Unless
    ‘`conv=notrunc`’ is given, truncate file before writing it.

‘`ibs=bytes`’ [¶](#index-ibs)
:   Set the input block size to bytes.
    This makes `dd` read bytes per block.
    The default is 512 bytes.

‘`obs=bytes`’ [¶](#index-obs)
:   Set the output block size to bytes.
    This makes `dd` write bytes per block.
    The default is 512 bytes.

‘`bs=bytes`’ [¶](#index-bs)
:   Set both input and output block sizes to bytes.
    This makes `dd` read and write bytes per block,
    overriding any ‘`ibs`’ and ‘`obs`’ settings.
    In addition, if no data-transforming `conv` operand is specified,
    input is copied to the output as soon as it’s read,
    even if it is smaller than the block size.

‘`cbs=bytes`’ [¶](#index-cbs)
:   Set the conversion block size to bytes.
    When converting variable-length records to fixed-length ones
    (`conv=block`) or the reverse (`conv=unblock`),
    use bytes as the fixed record length.

‘`skip=n`’ [¶](#index-skip)

‘`iseek=n`’
:   Skip n ‘`ibs`’-byte blocks in the input file before copying.
    If n ends in the letter ‘`B`’, interpret n
    as a byte count rather than a block count.
    (‘`B`’ and the ‘`iseek=`’ spelling are GNU extensions to POSIX.)

‘`seek=n`’ [¶](#index-seek)

‘`oseek=n`’
:   Skip n ‘`obs`’-byte blocks in the output file before
    truncating or copying.
    If n ends in the letter ‘`B`’, interpret n
    as a byte count rather than a block count.
    (‘`B`’ and the ‘`oseek=`’ spelling are GNU extensions to POSIX.)

‘`count=n`’ [¶](#index-count)
:   Copy n ‘`ibs`’-byte blocks from the input file, instead
    of everything until the end of the file.
    If n ends in the letter ‘`B`’,
    interpret n as a byte count rather than a block count;
    this is a GNU extension to POSIX.
    If short reads occur, as could be the case
    when reading from a pipe for example, ‘`iflag=fullblock`’
    ensures that ‘`count=`’ counts complete input blocks
    rather than input read operations.
    As an extension to POSIX, ‘`count=0`’ copies zero blocks
    instead of copying all blocks.

‘`status=level`’ [¶](#index-status)
:   Specify the amount of information printed.
    If this operand is given multiple times, the last one takes precedence.
    The level value can be one of the following:

    ‘`none`’ [¶](#index-none-dd-status_003d)
    :   Do not print any informational or warning messages to standard error.
        Error messages are output as normal.

    ‘`noxfer`’ [¶](#index-noxfer-dd-status_003d)
    :   Do not print the final transfer rate and volume statistics
        that normally make up the last status line.

    ‘`progress`’ [¶](#index-progress-dd-status_003d)
    :   Print the transfer rate and volume statistics on standard error,
        when processing each input block. Statistics are output
        on a single line at most once every second, but updates
        can be delayed when waiting on I/O.

    Transfer information is normally output to standard error upon
    receipt of the ‘`INFO`’ signal or when `dd` exits,
    and defaults to the following form in the C locale:

    ```
    7287+1 records in
    116608+0 records out
    59703296 bytes (60 MB, 57 MiB) copied, 0.0427974 s, 1.4 GB/s
    ```

    The notation ‘`w+p`’ stands for w whole blocks
    and p partial blocks. A partial block occurs when a read or
    write operation succeeds but transfers less data than the block size.
    An additional line like ‘`1 truncated record`’ or ‘`10
    truncated records`’ is output after the ‘`records out`’ line if
    ‘`conv=block`’ processing truncated one or more input records.

    The ‘`status=`’ operand is a GNU extension to POSIX.

‘`conv=conversion[,conversion]…`’ [¶](#index-conv)
:   Convert the file as specified by the conversion argument(s).
    (No spaces around any comma(s).)

    Conversions:

    ‘`ascii`’ [¶](#index-ascii_002c-converting-to)
    :   Convert EBCDIC to ASCII,
        using the conversion table specified by POSIX.
        This provides a 1:1 translation for all 256 bytes.
        This implies ‘`conv=unblock`’; input is converted to
        ASCII before trailing spaces are deleted.

    ‘`ebcdic`’ [¶](#index-ebcdic_002c-converting-to)
    :   Convert ASCII to EBCDIC.
        This is the inverse of the ‘`ascii`’ conversion.
        This implies ‘`conv=block`’; trailing spaces are added
        before being converted to EBCDIC.

    ‘`ibm`’ [¶](#index-alternate-ebcdic_002c-converting-to)
    :   This acts like ‘`conv=ebcdic`’, except it
        uses the alternate conversion table specified by POSIX.
        This is not a 1:1 translation, but reflects common historical practice
        for ‘`~`’, ‘`[`’, and ‘`]`’.

        The ‘`ascii`’, ‘`ebcdic`’, and ‘`ibm`’ conversions are
        mutually exclusive. If you use any of these conversions, you should also
        use the ‘`cbs=`’ operand.

    ‘`block`’ [¶](#index-block-_0028space_002dpadding_0029)
    :   For each line in the input, output ‘`cbs`’ bytes, replacing the
        input newline with a space and truncating or padding input lines with
        spaces as necessary.

    ‘`unblock`’ [¶](#index-unblock)
    :   Remove any trailing spaces in each ‘`cbs`’-sized input block,
        and append a newline.

        The ‘`block`’ and ‘`unblock`’ conversions are mutually exclusive.
        If you use either of these conversions, you should also use the
        ‘`cbs=`’ operand.

    ‘`lcase`’ [¶](#index-lcase_002c-converting-to)
    :   Change uppercase letters to lowercase.

    ‘`ucase`’ [¶](#index-ucase_002c-converting-to)
    :   Change lowercase letters to uppercase.

        The ‘`lcase`’ and ‘`ucase`’ conversions are mutually exclusive.

    ‘`sparse`’ [¶](#index-sparse)
    :   Try to seek rather than write NUL output blocks.
        On a file system that supports sparse files, this will create
        sparse output when extending the output file.
        Be careful when using this conversion in conjunction with
        ‘`conv=notrunc`’ or ‘`oflag=append`’.
        With ‘`conv=notrunc`’, existing data in the output file
        corresponding to NUL blocks from the input, will be untouched.
        With ‘`oflag=append`’ the seeks performed will be ineffective.
        Similarly, when the output is a device rather than a file,
        NUL input blocks are not copied, and therefore this conversion
        is most useful with virtual or pre zeroed devices.

        The ‘`sparse`’ conversion is a GNU extension to POSIX.

    ‘`swab`’ [¶](#index-swab-_0028byte_002dswapping_0029)
    :   Swap every pair of input bytes.

    ‘`sync`’ [¶](#index-sync-_0028padding-with-ASCII-NULs_0029)
    :   Pad every input block to size of ‘`ibs`’ with trailing zero bytes.
        When used with ‘`block`’ or ‘`unblock`’, pad with spaces instead of
        zero bytes.

    The following “conversions” are really file flags
    and don’t affect internal processing:

    ‘`excl`’ [¶](#index-excl)
    :   Fail if the output file already exists; `dd` must create the
        output file itself.

    ‘`nocreat`’ [¶](#index-nocreat)
    :   Do not create the output file; the output file must already exist.

        The ‘`excl`’ and ‘`nocreat`’ conversions are mutually exclusive,
        and are GNU extensions to POSIX.

    ‘`notrunc`’ [¶](#index-notrunc)
    :   Do not truncate the output file.

    ‘`noerror`’ [¶](#index-noerror)
    :   Continue after read errors.

    ‘`fdatasync`’ [¶](#index-fdatasync)
    :   Synchronize output data just before finishing,
        even if there were write errors.
        This forces a physical write of output data,
        so that even if power is lost the output data will be preserved.
        If neither this nor ‘`fsync`’ are specified, output is treated as
        usual with file systems, i.e., output data and metadata may be cached
        in primary memory for some time before the operating system physically
        writes it, and thus output data and metadata may be lost if power is lost.
        See [`sync`: Synchronize cached writes to persistent storage](#sync-invocation).
        This conversion is a GNU extension to POSIX.

    ‘`fsync`’ [¶](#index-fsync)
    :   Synchronize output data and metadata just before finishing,
        even if there were write errors.
        This acts like ‘`fdatasync`’ except it also preserves output metadata,
        such as the last-modified time of the output file; for this reason it
        may be a bit slower than ‘`fdatasync`’ although the performance
        difference is typically insignificant for `dd`.
        This conversion is a GNU extension to POSIX.

‘`iflag=flag[,flag]…`’ [¶](#index-iflag)
:   Access the input file using the flags specified by the flag
    argument(s). (No spaces around any comma(s).)

‘`oflag=flag[,flag]…`’ [¶](#index-oflag)
:   Access the output file using the flags specified by the flag
    argument(s). (No spaces around any comma(s).)

    Here are the flags.

    ‘`append`’ [¶](#index-append)
    :   Write in append mode, so that even if some other process is writing to
        this file, every `dd` write will append to the current
        contents of the file. This flag makes sense only for output.
        If you combine this flag with the ‘`of=file`’ operand,
        you should also specify ‘`conv=notrunc`’ unless you want the
        output file to be truncated before being appended to.

    ‘`cio`’ [¶](#index-cio)
    :   Use concurrent I/O mode for data. This mode performs direct I/O
        and drops the POSIX requirement to serialize all I/O to the same file.
        A file cannot be opened in CIO mode and with a standard open at the
        same time.

    ‘`direct`’ [¶](#index-direct)
    :   Use direct I/O for data, avoiding the buffer cache.
        The kernel may impose restrictions on read or write buffer sizes.
        For example, with an ext4 destination file system and a Linux-based kernel,
        using ‘`oflag=direct`’ will cause writes to fail with `EINVAL` if the
        output buffer size is not a multiple of 512.

    ‘`directory`’ [¶](#index-directory)
    :   Fail unless the file is a directory. Most operating systems do not
        allow I/O to a directory, so this flag has limited utility.

    ‘`dsync`’ [¶](#index-dsync)
    :   Use synchronized I/O for data. For the output file, this forces a
        physical write of output data on each write. For the input file,
        this flag can matter when reading from a remote file that has been
        written to synchronously by some other process. Metadata (e.g.,
        last-access and last-modified time) is not necessarily synchronized.

    ‘`sync`’ [¶](#index-sync)
    :   Use synchronized I/O for both data and metadata.

    ‘`nocache`’ [¶](#index-nocache)
    :   Request to discard the system data cache for a file.
        When count=0 all cached data for the file is specified,
        otherwise the cache is dropped for the processed
        portion of the file. Also when count=0,
        failure to discard the cache is diagnosed
        and reflected in the exit status.

        Because data not already persisted to storage is not discarded from the cache,
        the ‘`sync`’ conversions in the following examples maximize the
        effectiveness of the ‘`nocache`’ flag.

        Here are some usage examples:

        ```
        # Advise to drop cache for whole file
        dd if=ifile iflag=nocache count=0

        # Ensure drop cache for the whole file
        dd of=ofile oflag=nocache conv=notrunc,fdatasync count=0

        # Advise to drop cache for part of file
        # The kernel will consider only complete and
        # already persisted pages.
        dd if=ifile iflag=nocache skip=10 count=10 of=/dev/null

        # Stream data using just the read-ahead cache.
        # See also the ‘direct’ flag.
        dd if=ifile of=ofile iflag=nocache oflag=nocache,sync
        ```

    ‘`nonblock`’ [¶](#index-nonblock)
    :   Use non-blocking I/O.

    ‘`noatime`’ [¶](#index-noatime)
    :   Do not update the file’s access timestamp.
        See [File timestamps](#File-timestamps).
        Some older file systems silently ignore this flag, so it is a good
        idea to test it on your files before relying on it.

    ‘`noctty`’ [¶](#index-noctty)
    :   Do not assign the file to be a controlling terminal for `dd`.
        This has no effect when the file is not a terminal.
        On many hosts (e.g., GNU/Linux hosts), this flag has no effect
        at all.

    ‘`nofollow`’ [¶](#index-nofollow)
    :   Do not follow symbolic links.

    ‘`nolinks`’ [¶](#index-nolinks)
    :   Fail if the file has multiple hard links.

    ‘`binary`’ [¶](#index-binary)
    :   Use binary I/O. This flag has an effect only on nonstandard
        platforms that distinguish binary from text I/O.

    ‘`text`’ [¶](#index-text)
    :   Use text I/O. Like ‘`binary`’, this flag has no effect on
        standard platforms.

    ‘`fullblock`’ [¶](#index-fullblock)
    :   Accumulate full blocks from input. The `read` system call
        may return early if a full block is not available.
        When that happens, continue calling `read` to fill the remainder
        of the block.
        This flag can be used only with `iflag`.
        This flag is useful with pipes for example
        as they may return short reads. In that case,
        this flag is needed to ensure that a ‘`count=`’ argument is
        interpreted as a block count rather than a count of read operations.

    These flags are all GNU extensions to POSIX.
    They are not supported on all systems, and ‘`dd`’ rejects
    attempts to use them when they are not supported. When reading from
    standard input or writing to standard output, the ‘`nofollow`’ and
    ‘`noctty`’ flags should not be specified, and the other flags
    (e.g., ‘`nonblock`’) can affect how other processes behave with the
    affected file descriptors, even after `dd` exits.

The behavior of `dd` is unspecified if operands other than
‘`conv=`’, ‘`iflag=`’, ‘`oflag=`’, and ‘`status=`’ are
specified more than once.

The numeric-valued strings above (n and bytes)
are unsigned decimal integers that
can be followed by a multiplier: ‘`b`’=512, ‘`c`’=1,
‘`w`’=2, ‘`xm`’=m, or any of the
standard block size suffixes like ‘`k`’=1024 (see [Block size](#Block-size)).
These multipliers are GNU extensions to POSIX, except that
POSIX allows bytes to be followed by ‘`k`’, ‘`b`’, and
‘`xm`’. An ‘`xm`’ can be used more than once in a number.
Block sizes (i.e., specified by bytes strings) must be nonzero.

Any block size you specify via ‘`bs=`’, ‘`ibs=`’, ‘`obs=`’, ‘`cbs=`’
should not be too large – values larger than a few megabytes
are generally wasteful or (as in the gigabyte..exabyte case) downright
counterproductive or error-inducing.

To process data with offset or size that is not a multiple of the I/O
block size, you can use a numeric string n that ends in the
letter ‘`B`’.
For example, the following shell commands copy data
in 1 MiB blocks between a flash drive and a tape, but do not save
or restore a 512-byte area at the start of the flash drive:

```
flash=/dev/sda
tape=/dev/st0

# Copy all but the initial 512 bytes from flash to tape.
dd if=$flash iseek=512B bs=1MiB of=$tape

# Copy from tape back to flash, leaving initial 512 bytes alone.
dd if=$tape bs=1MiB of=$flash oseek=512B
```

For failing storage devices, other tools come with a great variety of extra
functionality to ease the saving of as much data as possible before the
device finally dies, e.g.
[GNU `ddrescue`](https://www.gnu.org/software/ddrescue/).
However, in some cases such a tool is not available or the administrator
feels more comfortable with the handling of `dd`.
As a simple rescue method, call `dd` as shown in the following
example: the operand ‘`conv=noerror,sync`’ is used to continue
after read errors and to pad out bad reads with NULs, while
‘`iflag=fullblock`’ caters for short reads (which traditionally never
occur on flash or similar devices):

```
# Rescue data from an (unmounted!) partition of a failing device.
dd conv=noerror,sync iflag=fullblock </dev/sda1 > /mnt/rescue.img
```

Sending an ‘`INFO`’ signal (or ‘`USR1`’ signal where that is unavailable)
to a running `dd` process makes it print I/O statistics to
standard error and then resume copying. In the example below,
`dd` is run in the background to copy 5GB of data.
The `kill` command makes it output intermediate I/O statistics,
and when `dd` completes normally or is killed by the
`SIGINT` signal, it outputs the final statistics.

```
# Ignore the signal so we never inadvertently terminate the dd child.
# (This is not needed when SIGINFO is available.)
trap '' USR1

# Run dd with the fullblock iflag to avoid short reads
# which can be triggered by reception of signals.
dd iflag=fullblock if=/dev/zero of=/dev/null count=5000000 bs=1000 & pid=$!

# Output stats every second.
while kill -s USR1 $pid 2>/dev/null; do sleep 1; done
```

The above script will output in the following format:

```
3441325+0 records in
3441325+0 records out
3441325000 bytes (3.4 GB, 3.2 GiB) copied, 1.00036 s, 3.4 GB/s
5000000+0 records in
5000000+0 records out
5000000000 bytes (5.0 GB, 4.7 GiB) copied, 1.44433 s, 3.5 GB/s
```

The ‘`status=progress`’ operand periodically updates the last line
of the transfer statistics above.

On systems lacking the ‘`INFO`’ signal `dd` responds to the
‘`USR1`’ signal instead, unless the `POSIXLY_CORRECT`
environment variable is set.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`mv`: Move (rename) files](#mv-invocation), Previous: [`dd`: Convert and copy a file](#dd-invocation), Up: [Basic operations](#Basic-operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 11.3 `install`: Copy files and set attributes [¶](#install_003a-Copy-files-and-set-attributes)

`install` copies files while setting their file mode bits and, if
possible, their owner and group. Synopses:

```
install [option]... [-T] source dest
install [option]... source... directory
install [option]... -t directory source...
install [option]... -d directory...
```

* If two file names are given, `install` copies the first file to the
  second.
* If the `--target-directory` (`-t`) option is given, or
  failing that if the last file is a directory and the
  `--no-target-directory` (`-T`) option is not given,
  `install` copies each source file to the specified
  directory, using the sources’ names.
* If the `--directory` (`-d`) option is given,
  `install` creates each directory and any missing parent
  directories. Parent directories are created with mode
  ‘`u=rwx,go=rx`’ (755), regardless of the `-m` option or the
  current umask. See [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid), for how the
  set-user-ID and set-group-ID bits of parent directories are inherited.

`install` is similar to `cp`, but allows you to control the
attributes of destination files. It is typically used in Makefiles to
copy programs into their destination directories. It refuses to copy
files onto themselves.

`install` never preserves extended attributes (xattr).

The program accepts the following options. Also see [Common options](#Common-options).

‘`-b`’ [¶](#index-_002db-15)

‘`--backup[=method]`’
:   See [Backup options](#Backup-options).
    Make a backup of each file that would otherwise be overwritten or removed.

‘`-C`’ [¶](#index-_002dC-3)

‘`--compare`’
:   Compare content of source and destination files, and if there would be no
    change to the destination content, owner, group, permissions, and possibly
    SELinux context, then do not modify the destination at all.
    This option is best used in conjunction with `--user`,
    `--group` and `--mode` options, lest `install`
    incorrectly determines the default attributes that installed files would have
    (as it doesn’t consider setgid directories and POSIX default ACLs for example).
    This could result in redundant copies or attributes that are not reset to the
    correct defaults.

‘`-c`’ [¶](#index-_002dc-14)
:   Ignored; for compatibility with old Unix versions of `install`.

‘`-D`’ [¶](#index-_002dD-2)
:   Create any missing parent directories of dest,
    then copy source to dest.
    Explicitly specifying the `--target-directory=dir` will similarly
    ensure the presence of that hierarchy before copying source arguments.

‘`-d`’ [¶](#index-_002dd-12)

‘`--directory`’
:   Create any missing parent directories, giving them the default
    attributes. Then create each given directory, setting their owner,
    group and mode as given on the command line or to the defaults.

‘`--debug`’ [¶](#index-_002d_002ddebug-2)
:   Print extra information to stdout, explaining how files are copied.
    This option implies the `--verbose` option.

‘`-g group`’ [¶](#index-_002dg-4)

‘`--group=group`’
:   Set the group ownership of installed files or directories to
    group. The default is the process’s current group. group
    may be either a group name or a numeric group ID.

‘`-m mode`’ [¶](#index-_002dm-4)

‘`--mode=mode`’
:   Set the file mode bits for the installed file or directory to mode,
    which can be either an octal number, or a symbolic mode as in
    `chmod`, with ‘`a=`’ (no access allowed to anyone) as the
    point of departure (see [File permissions](#File-permissions)).
    The default mode is ‘`u=rwx,go=rx,a-s`’ – read, write, and
    execute for the owner, read and execute for group and other, and with
    set-user-ID and set-group-ID disabled.
    This default is not quite the same as ‘`755`’, since it disables
    instead of preserving set-user-ID and set-group-ID on directories.
    See [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid).

‘`-o owner`’ [¶](#index-_002do-6)

‘`--owner=owner`’
:   If `install` has appropriate privileges (is run as root), set the
    ownership of installed files or directories to owner. The default
    is `root`. owner may be either a user name or a numeric user
    ID.

‘`--preserve-context`’ [¶](#index-_002d_002dpreserve_002dcontext)
:   Preserve the SELinux security context of files and directories.
    Failure to preserve the context in all of the files or directories
    will result in an exit status of 1. If SELinux is disabled then
    print a warning and ignore the option.

‘`-p`’ [¶](#index-_002dp-4)

‘`--preserve-timestamps`’
:   Set the time of last access and the time of last modification of each
    installed file to match those of each corresponding original file.
    When a file is installed without this option, its last access and
    last modification timestamps are both set to the time of installation.
    This option is useful if you want to use the last modification timestamps
    of installed files to keep track of when they were last built as opposed
    to when they were last installed.

‘`-s`’ [¶](#index-_002ds-17)

‘`--strip`’
:   Strip the symbol tables from installed binary executables.

‘`--strip-program=program`’ [¶](#index-_002d_002dstrip_002dprogram)
:   Program used to strip binaries.

‘`-S suffix`’ [¶](#index-_002dS-7)

‘`--suffix=suffix`’
:   Append suffix to each backup file made with `-b`.
    See [Backup options](#Backup-options).

‘`-t directory`’ [¶](#index-_002dt-12)

‘`--target-directory=directory`’
:   Specify the destination directory.
    See [Target directory](#Target-directory).
    Also specifying the `-D` option will ensure the directory is present.

‘`-T`’ [¶](#index-_002dT-6)

‘`--no-target-directory`’
:   Do not treat the last operand specially when it is a directory or a
    symbolic link to a directory. See [Target directory](#Target-directory).

‘`-v`’ [¶](#index-_002dv-8)

‘`--verbose`’
:   Print the name of each file before copying it.

‘`-Z`’ [¶](#index-_002dZ-2)

‘`--context[=context]`’
:   Without a specified context, adjust the SELinux security context according
    to the system default type for destination files, similarly to the
    `restorecon` command.
    The long form of this option with a specific context specified,
    will set the context for newly created files only.
    With a specified context, if both SELinux and SMACK are disabled, a warning is
    issued.
    This option is mutually exclusive with the `--preserve-context` option.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`rm`: Remove files or directories](#rm-invocation), Previous: [`install`: Copy files and set attributes](#install-invocation), Up: [Basic operations](#Basic-operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 11.4 `mv`: Move (rename) files [¶](#mv_003a-Move-_0028rename_0029-files)

`mv` moves or renames files (or directories). Synopses:

```
mv [option]... [-T] source dest
mv [option]... source... directory
mv [option]... -t directory source...
```

* If two file names are given, `mv` moves the first file to the
  second.
* If the `--target-directory` (`-t`) option is given, or
  failing that if the last file is a directory and the
  `--no-target-directory` (`-T`) option is not given,
  `mv` moves each source file to the specified
  directory, using the sources’ names.

To move a file, `mv` ordinarily simply renames it.
However, if renaming does not work because the destination’s file
system differs, `mv` falls back on copying as if by `cp -a`,
then (assuming the copy succeeded) it removes the original.
If the copy fails, then `mv` removes any partially created
copy in the destination. If you were to copy three directories from
one file system to another and the copy of the first
directory succeeded, but the second didn’t, the first would be left on
the destination file system and the second and third would be left on the
original file system.

`mv` always tries to copy extended attributes (xattr), which may
include SELinux context, ACLs or Capabilities.
Upon failure all but ‘`Operation not supported`’ warnings are output.

If a destination file exists but is normally unwritable, standard input
is a terminal, and the `-f` or `--force` option is not given,
`mv` prompts the user for whether to replace the file. (You might
own the file, or have write permission on its directory.) If the
response is not affirmative, the file is skipped.

Avoid specifying a source name with a trailing slash,
when it might be a symlink to a directory.
Otherwise, `mv` may do something very surprising, since
its behavior depends on the underlying rename system call.
On a system with a modern Linux-based kernel, it fails with
`errno=ENOTDIR`.
However, on other systems (at least FreeBSD 6.1 and Solaris 10) it silently
renames not the symlink but rather the directory referenced by the symlink.
See [Trailing slashes](#Trailing-slashes).

The `mv` command replaces destination directories only if they
are empty. Conflicting populated directories are skipped with a diagnostic.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-b`’ [¶](#index-_002db-16)

‘`--backup[=method]`’
:   See [Backup options](#Backup-options).
    Make a backup of each file that would otherwise be overwritten or removed.

‘`--debug`’ [¶](#index-_002d_002ddebug-3)
:   Print extra information to stdout, explaining how files are copied.
    This option implies the `--verbose` option.

‘`-f`’ [¶](#index-_002df-11)

‘`--force`’
:   Do not prompt the user before replacing a destination file.
    If you specify more than one of the `-i`, `-f`, `-n`
    options, only the final one takes effect.

‘`-i`’ [¶](#index-_002di-12)

‘`--interactive`’
:   Prompt whether to overwrite each existing destination file, regardless
    of its permissions, and fail if the response is not affirmative.
    If you specify more than one of the `-i`, `-f`, `-n`
    options, only the final one takes effect.

‘`-n`’ [¶](#index-_002dn-12)

‘`--no-clobber`’
:   Do not overwrite an existing file; silently fail instead.
    If you specify more than one of the `-i`, `-f`, `-n`
    options, only the final one takes effect.
    This option is mutually exclusive with `-b` or `--backup` option.
    See also the `--update=none` option which will
    skip existing files but not fail.

‘`--no-copy`’ [¶](#index-_002d_002dno_002dcopy)
:   If a file cannot be renamed because the destination file system differs,
    fail with a diagnostic instead of copying and then removing the file.

‘`--exchange`’ [¶](#index-_002d_002dexchange)
:   Exchange source and destination instead of renaming source to destination.
    Both files must exist; they need not be the same type.
    This exchanges all data and metadata.

    This option can be used to replace one directory with another.
    When used this way, it should be combined with
    `--no-target-directory` (`-T`)
    to avoid confusion about the destination location.
    For example, you might use ‘`mv -T --exchange d1 d2`’
    to exchange two directories d1 and d2.

    Exchanges are atomic if the source and destination are both in a
    single file system that supports atomic exchange.
    Non-atomic exchanges are not yet supported.

    If the source and destination might not be on the same file system,
    using `--no-copy` will prevent future versions of `mv`
    from implementing the exchange by copying.

‘`-u`’ [¶](#index-_002du-7)

‘`--update`’
:   Do not move a non-directory that has an existing destination with the
    same or newer modification timestamp;
    instead, silently skip the file without failing.
    If the move is across file system boundaries, the comparison is to the
    source timestamp truncated to the resolutions of the destination file
    system and of the system calls used to update timestamps; this avoids
    duplicate work if several ‘`mv -u`’ commands are executed with the
    same source and destination.
    This option is ignored if the `-n` or `--no-clobber`
    option is also specified.

    which gives more control over which existing files in the
    destination are replaced, and its value can be one of the following:

    ‘`all`’
    :   This is the default operation when an `--update` option is not specified,
        and results in all existing files in the destination being replaced.

    ‘`none`’
    :   This is like the deprecated `--no-clobber` option, where no files in the
        destination are replaced, and also skipping a file does not induce a failure.

    ‘`none-fail`’
    :   This is similar to ‘`none`’, in that no files in the destination
        are replaced, but any skipped files are diagnosed and induce a failure.

    ‘`older`’
    :   This is the default operation when `--update` is specified, and results
        in files being replaced if they’re older than the corresponding source file.

‘`-v`’ [¶](#index-_002dv-9)

‘`--verbose`’
:   Print the name of each file before moving it.

‘`--keep-directory-symlink`’ [¶](#index-_002d_002dkeep_002ddirectory_002dsymlink)
:   Follow existing symlinks to directories when copying.
    Use this option only when the destination directory’s contents are trusted,
    as an attacker can place symlinks in the destination
    to cause `cp` write to arbitrary target directories.

‘`--strip-trailing-slashes`’ [¶](#index-_002d_002dstrip_002dtrailing_002dslashes-1)
:   Remove any trailing slashes from each source argument.
    See [Trailing slashes](#Trailing-slashes).

‘`-S suffix`’ [¶](#index-_002dS-8)

‘`--suffix=suffix`’
:   Append suffix to each backup file made with `-b`.
    See [Backup options](#Backup-options).

‘`-t directory`’ [¶](#index-_002dt-13)

‘`--target-directory=directory`’
:   Specify the destination directory.
    See [Target directory](#Target-directory).

‘`-T`’ [¶](#index-_002dT-7)

‘`--no-target-directory`’
:   Do not treat the last operand specially when it is a directory or a
    symbolic link to a directory. See [Target directory](#Target-directory).

‘`-Z`’ [¶](#index-_002dZ-3)

‘`--context`’
:   This option functions similarly to the `restorecon` command,
    by adjusting the SELinux security context according
    to the system default type for destination files and each created directory.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`shred`: Remove files more securely](#shred-invocation), Previous: [`mv`: Move (rename) files](#mv-invocation), Up: [Basic operations](#Basic-operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 11.5 `rm`: Remove files or directories [¶](#rm_003a-Remove-files-or-directories)

`rm` removes each given file. By default, it does not remove
directories. Synopsis:

```
rm [option]... [file]...
```

If the `-I` or `--interactive=once` option is given,
and there are more than three files or the `-r`, `-R`,
or `--recursive` are given, then `rm` prompts the user
for whether to proceed with the entire operation. If the response is
not affirmative, the entire command is aborted.

Otherwise, if a file is unwritable, standard input is a terminal, and
the `-f` or `--force` option is not given, or the
`-i` or `--interactive=always` option *is* given,
`rm` prompts the user for whether to remove the file.
If the response is not affirmative, the file is skipped.

Any attempt to remove a file whose last file name component is
`.` or `..` is rejected without any prompting, as mandated
by POSIX.

*Warning*: If you use `rm` to remove a file, it is usually
possible to recover the contents of that file. If you want more assurance
that the contents are unrecoverable, consider using `shred`.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-d`’ [¶](#index-_002dd-13)

‘`--dir`’
:   Remove the listed directories if they are empty.

‘`-f`’ [¶](#index-_002df-12)

‘`--force`’
:   Ignore nonexistent files and missing operands, and never prompt the user.
    Ignore any previous `--interactive` (`-i`) option.

‘`-i`’ [¶](#index-_002di-13)
:   Prompt whether to remove each file.
    If the response is not affirmative, silently skip the file without failing.
    Ignore any previous `--force` (`-f`) option.
    Equivalent to `--interactive=always`.

‘`-I`’ [¶](#index-_002dI-1)
:   Prompt once whether to proceed with the command, if more than three
    files are named or if a recursive removal is requested. Ignore any
    previous `--force` (`-f`) option. Equivalent to
    `--interactive=once`.

‘`--interactive [=when]`’ [¶](#index-_002d_002dinteractive-2)
:   Specify when to issue an interactive prompt. when may be
    omitted, or one of:

    * never
      - Do not prompt at all.
    * once
      - Prompt once if more than three files are named or if a recursive
      removal is requested. Equivalent to `-I`.
    * always
      - Prompt for every file being removed. Equivalent to `-i`.

    `--interactive` with no when is equivalent to
    `--interactive=always`.

‘`--one-file-system`’ [¶](#index-_002d_002done_002dfile_002dsystem-1)
:   When removing a hierarchy recursively, do not remove any directory that is on a
    file system different from that of the corresponding command line argument.
    This option is useful when removing a build “chroot” hierarchy,
    which normally contains no valuable data. However, it is not uncommon
    to bind-mount `/home` into such a hierarchy, to make it easier to
    use one’s start-up file. The catch is that it’s easy to forget to
    unmount `/home`. Then, when you use `rm -rf` to remove
    your normally throw-away chroot, that command will remove everything
    under `/home`, too.
    Use the `--one-file-system` option, and it will
    diagnose and skip directories on other file systems.
    Of course, this will not save your `/home` if it and your
    chroot happen to be on the same file system.
    See also `--preserve-root=all` to protect command line arguments
    themselves.

‘`--preserve-root [=all]`’ [¶](#index-_002d_002dpreserve_002droot)
:   Fail upon any attempt to remove the root directory, `/`,
    when used with the `--recursive` option.
    This is the default behavior.
    See [Treating `/` specially](#Treating-_002f-specially).
    When ‘`all`’ is specified, reject any command line argument
    that is not on the same file system as its parent.

‘`--no-preserve-root`’ [¶](#index-_002d_002dno_002dpreserve_002droot)
:   Do not treat `/` specially when removing recursively.
    This option is not recommended unless you really want to
    remove all the files on your computer.
    See [Treating `/` specially](#Treating-_002f-specially).

‘`-r`’ [¶](#index-_002dr-8)

‘`-R`’

‘`--recursive`’
:   Remove the listed directories and their contents recursively.

‘`-v`’ [¶](#index-_002dv-10)

‘`--verbose`’
:   Print the name of each file before removing it.

One common question is how to remove files whose names begin with a
‘`-`’. GNU `rm`, like every program that uses the `getopt`
function to parse its arguments, lets you use the ‘`--`’ option to
indicate that all following arguments are non-options. To remove a file
called `-f` in the current directory, you could type either:

```
rm -- -f
```

or:

```
rm ./-f
```

The Unix `rm` program’s use of a single ‘`-`’ for this purpose
predates the development of the `getopt` standard syntax.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`rm`: Remove files or directories](#rm-invocation), Up: [Basic operations](#Basic-operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 11.6 `shred`: Remove files more securely [¶](#shred_003a-Remove-files-more-securely)

`shred` overwrites devices or files, to help prevent even
extensive forensics from recovering the data.

Ordinarily when you remove a file (see [`rm`: Remove files or directories](#rm-invocation)), its data
and metadata are not actually destroyed. Only the file’s directory
entry is removed, and the file’s storage is reclaimed only when no
process has the file open and no other directory entry links to the
file. And even if file’s data and metadata’s storage space is freed
for further reuse, there are undelete utilities that will attempt to
reconstruct the file from the data in freed storage, and that can
bring the file back if the storage was not rewritten.

On a busy system with a nearly-full device, space can get reused in a few
seconds. But there is no way to know for sure. And although the
undelete utilities and already-existing processes require insider or
superuser access, you may be wary of the superuser,
of processes running on your behalf, or of attackers
that can physically access the storage device. So if you have sensitive
data, you may want to be sure that recovery is not possible
by plausible attacks like these.

The best way to remove something irretrievably is to destroy the media
it’s on with acid, melt it down, or the like. For cheap removable media
this is often the preferred method. However, some storage devices
are expensive or are harder to destroy, so the `shred` utility tries
to achieve a similar effect non-destructively, by overwriting the file
with non-sensitive data.

The `shred` command relies on a **crucial assumption**:
that the file system and hardware overwrite data in place.
Although this is common and is the traditional
way to do things, many modern file system designs do not satisfy this
assumption. Exceptions include:

* Log-structured or journaled file systems, such as ext3/ext4 (in
  `data=journal` mode), Btrfs, NTFS, ReiserFS, XFS, ZFS, file
  systems supplied with AIX and Solaris, etc., when they are configured to
  journal data.
* File systems that write redundant data and carry on even if some writes
  fail, such as RAID-based file systems.
* File systems that make snapshots, such as Network Appliance’s NFS server.
* File systems that cache in temporary locations, such as NFS version 3
  clients.
* Compressed file systems.

For ext3 and ext4 file systems, `shred` is less effective
when the file system is in `data=journal`
mode, which journals file data in addition to just metadata. In both
the `data=ordered` (default) and `data=writeback` modes,
`shred` works as usual. The ext3/ext4 journaling modes can be changed
by adding the `data=something` option to the mount options for a
particular file system in the `/etc/fstab` file, as documented in
the `mount` man page (‘`man mount`’). Alternatively, if
you know how large the journal is, you can shred the journal by
shredding enough file data so that the journal cycles around and fills
up with shredded data.

If you are not sure how your file system operates, then you should assume
that it does not overwrite data in place, which means `shred` cannot
reliably operate on regular files in your file system.

Generally speaking, it is more reliable to shred a device than a file,
since this bypasses file system design issues mentioned above.
However, devices are also problematic for shredding, for reasons
such as the following:

* Solid-state storage devices (SSDs) typically do wear leveling to
  prolong service life, and this means writes are distributed to other
  blocks by the hardware, so “overwritten” data blocks are still
  present in the underlying device.
* Most storage devices map out bad blocks invisibly to
  the application; if the bad blocks contain sensitive data,
  `shred` won’t be able to destroy it.
* With some obsolete storage technologies,
  it may be possible to take (say) a floppy disk back
  to a laboratory and use a lot of sensitive (and expensive) equipment
  to look for the faint “echoes” of the original data underneath the
  overwritten data. With these older technologies, if the file has been
  overwritten only once, it’s reputedly not even that hard. Luckily,
  this kind of data recovery has become difficult, and there is no
  public evidence that today’s higher-density storage devices can be
  analyzed in this way.

  The `shred` command can use many overwrite passes,
  with data patterns chosen to
  maximize the damage they do to the old data.
  By default the patterns are designed for best effect on hard drives using
  now-obsolete technology; for newer devices, a single pass should suffice.
  For more details, see the source code and Peter Gutmann’s paper
  [Secure Deletion of Data from Magnetic and Solid-State Memory](https://www.cs.auckland.ac.nz/~pgut001/pubs/secure_del.html),
  from the proceedings of the Sixth USENIX Security Symposium (San Jose,
  California, July 22–25, 1996).

`shred` makes no attempt to detect or report these problems, just as
it makes no attempt to do anything about backups. However, since it is
more reliable to shred devices than files, `shred` by default does
not deallocate or remove the output file. This default is more suitable
for devices, which typically cannot be deallocated and should not be
removed.

Finally, consider the risk of backups and mirrors.
File system backups and remote mirrors may contain copies of the
file that cannot be removed, and that will allow a shredded file
to be recovered later. So if you keep any data you may later want
to destroy using `shred`, be sure that it is not backed up or mirrored.

```
shred [option]... file[...]
```

The program accepts the following options. Also see [Common options](#Common-options).

‘`-f`’ [¶](#index-_002df-13)

‘`--force`’
:   Override file permissions if necessary to allow overwriting.

‘`-n number`’ [¶](#index-_002dn-number)

‘`--iterations=number`’
:   By default, `shred` uses 3 passes of
    overwrite. You can reduce this to save time, or increase it if you think it’s
    appropriate. After 25 passes all of the internal overwrite patterns will have
    been used at least once.

‘`--random-source=file`’ [¶](#index-_002d_002drandom_002dsource-2)
:   Use file as a source of random data used to overwrite and to
    choose pass ordering. See [Sources of random data](#Random-sources).

‘`-s bytes`’ [¶](#index-_002ds-bytes)

‘`--size=bytes`’
:   Shred the first bytes bytes of the file. The default is to shred
    the whole file. bytes can be followed by a size specification like
    ‘`K`’, ‘`M`’, or ‘`G`’ to specify a multiple. See [Block size](#Block-size).

‘`-u`’ [¶](#index-_002du-8)

‘`--remove[=how]`’
:   After shredding a file, deallocate it (if possible) and then remove it.
    If a file has multiple links, only the named links will be removed.
    Often the file name is less sensitive than the file data, in which case
    the optional how parameter, supported with the long form option,
    gives control of how to more efficiently remove each directory entry.
    The ‘`unlink`’ parameter will just use a standard unlink call,
    ‘`wipe`’ will also first obfuscate bytes in the name, and
    ‘`wipesync`’ will also sync each obfuscated byte in the name to
    the file system.
    Although ‘`wipesync`’ is the default method, it can be expensive,
    requiring a sync for every character in every file. This can become
    significant with many files, or is redundant if your file system provides
    synchronous metadata updates.

‘`-v`’ [¶](#index-_002dv-11)

‘`--verbose`’
:   Display to standard error all status updates as sterilization proceeds.

‘`-x`’ [¶](#index-_002dx-4)

‘`--exact`’
:   By default, `shred` rounds the size of a regular file up to the next
    multiple of the file system block size to fully erase the slack space in
    the last block of the file. This space may contain portions of the current
    system memory on some systems for example.
    Use `--exact` to suppress that behavior.
    Thus, by default if you shred a 10-byte regular file on a system with 512-byte
    blocks, the resulting file will be 512 bytes long. With this option,
    shred does not increase the apparent size of the file.

‘`-z`’ [¶](#index-_002dz-11)

‘`--zero`’
:   Normally, the last pass that `shred` writes is made up of
    random data. If this would be conspicuous on your storage device (for
    example, because it looks like encrypted data), or you just think
    it’s tidier, the `--zero` option adds an additional overwrite pass with
    all zero bits. This is in addition to the number of passes specified
    by the `--iterations` option.

You might use the following command to erase the file system you
created on a USB flash drive. This command typically takes several
minutes, depending on the drive’s size and write speed. On modern
storage devices a single pass should be adequate, and will take one
third the time of the default three-pass approach.

```
shred -v -n 1 /dev/sdd1
```

Similarly, to erase all data on a selected partition of
your device, you could give a command like the following.

```
# 1 pass, write pseudo-random data; 3x faster than the default
shred -v -n1 /dev/sda5
```

To be on the safe side, use at least one pass that overwrites using
pseudo-random data. I.e., don’t be tempted to use ‘`-n0 --zero`’,
in case some device controller optimizes the process of writing blocks
of all zeros, and thereby does not clear all bytes in a block.
Some SSDs may do just that.

A file of ‘`-`’ denotes standard output.
The intended use of this is to shred a removed temporary file.
For example:

```
i=$(mktemp)
exec 3<>"$i"
rm -- "$i"
echo "Hello, world" >&3
shred - >&3
exec 3>&-
```

However, the command ‘`shred - >file`’ does not shred the contents
of file, since the shell truncates file before invoking
`shred`. Use the command ‘`shred file`’ or (if using a
Bourne-compatible shell) the command ‘`shred - 1<>file`’ instead.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [Changing file attributes](#Changing-file-attributes), Previous: [Basic operations](#Basic-operations), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 12 Special file types [¶](#Special-file-types-1)

This chapter describes commands which create special types of files (and
`rmdir`, which removes directories, one special file type).

Although Unix-like operating systems have markedly fewer special file
types than others, not *everything* can be treated only as the
undifferentiated byte stream of *normal files*. For example, when a
file is created or removed, the system must record this information,
which it does in a *directory* – a special type of file. Although
you can read directories as normal files, if you’re curious, in order
for the system to do its job it must impose a structure, a certain
order, on the bytes of the file. Thus it is a “special” type of file.

Besides directories, other special file types include named pipes
(FIFOs), symbolic links, sockets, and so-called *special files*.

* [`link`: Make a hard link via the link syscall](#link-invocation)
* [`ln`: Make links between files](#ln-invocation)
* [`mkdir`: Make directories](#mkdir-invocation)
* [`mkfifo`: Make FIFOs (named pipes)](#mkfifo-invocation)
* [`mknod`: Make block or character special files](#mknod-invocation)
* [`readlink`: Print value of a symlink or canonical file name](#readlink-invocation)
* [`rmdir`: Remove empty directories](#rmdir-invocation)
* [`unlink`: Remove files via the unlink syscall](#unlink-invocation)

---

Next: [`ln`: Make links between files](#ln-invocation), Up: [Special file types](#Special-file-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 12.1 `link`: Make a hard link via the link syscall [¶](#link_003a-Make-a-hard-link-via-the-link-syscall)

`link` creates a single hard link at a time.
It is a minimalist interface to the system-provided
`link` function. See [Hard Links](https://www.gnu.org/software/libc/manual/html_mono/libc.html#Hard-Links) in The GNU C Library Reference Manual.
It avoids the bells and whistles of the more commonly-used
`ln` command (see [`ln`: Make links between files](#ln-invocation)).
Synopsis:

```
link filename linkname
```

filename must specify an existing file, and linkname
must specify a nonexistent entry in an existing directory.
`link` simply calls `link (filename, linkname)`
to create the link.

On a GNU system, this command acts like ‘`ln --directory
--no-target-directory filename linkname`’. However, the
`--directory` and `--no-target-directory` options are
not specified by POSIX, and the `link` command is
more portable in practice.

If filename is a symbolic link, it is unspecified whether
linkname will be a hard link to the symbolic link or to the
target of the symbolic link. Use `ln -P` or `ln -L`
to specify which behavior is desired.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`mkdir`: Make directories](#mkdir-invocation), Previous: [`link`: Make a hard link via the link syscall](#link-invocation), Up: [Special file types](#Special-file-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]
