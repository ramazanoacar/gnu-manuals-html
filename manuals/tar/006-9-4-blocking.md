### 9.4 Blocking

*Block* and *record* terminology is rather confused, and it
is also confusing to the expert reader. On the other hand, readers
who are new to the field have a fresh mind, and they may safely skip
the next two paragraphs, as the remainder of this manual uses those
two terms in a quite consistent way.

John Gilmore, the writer of the public domain `tar` from which
GNU `tar` was originally derived, wrote (June 1995):

> The nomenclature of tape drives comes from IBM, where I believe
> they were invented for the IBM 650 or so. On IBM mainframes, what
> is recorded on tape are tape blocks. The logical organization of
> data is into records. There are various ways of putting records into
> blocks, including `F` (fixed sized records), `V` (variable
> sized records), `FB` (fixed blocked: fixed size records, n
> to a block), `VB` (variable size records, n to a block),
> `VSB` (variable spanned blocked: variable sized records that can
> occupy more than one block), etc. The `JCL` ‘`DD RECFORM=`’
> parameter specified this to the operating system.
>
> The Unix man page on `tar` was totally confused about this.
> When I wrote `PD TAR`, I used the historically correct terminology
> (`tar` writes data records, which are grouped into blocks).
> It appears that the bogus terminology made it into POSIX (no surprise
> here), and now François has migrated that terminology back
> into the source code too.

The term *physical block* means the basic transfer chunk from or
to a device, after which reading or writing may stop without anything
being lost. In this manual, the term *block* usually refers to
a disk physical block, *assuming* that each disk block is 512
bytes in length. It is true that some disk devices have different
physical blocks, but `tar` ignore these differences in its own
format, which is meant to be portable, so a `tar` block is always
512 bytes in length, and *block* always mean a `tar` block.
The term *logical block* often represents the basic chunk of
allocation of many disk blocks as a single entity, which the operating
system treats somewhat atomically; this concept is only barely used
in GNU `tar`.

The term *physical record* is another way to speak of a physical
block, those two terms are somewhat interchangeable. In this manual,
the term *record* usually refers to a tape physical block,
*assuming* that the `tar` archive is kept on magnetic tape.
It is true that archives may be put on disk or used with pipes,
but nevertheless, `tar` tries to read and write the archive one
*record* at a time, whatever the medium in use. One record is made
up of an integral number of blocks, and this operation of putting many
disk blocks into a single tape block is called *reblocking*, or
more simply, *blocking*. The term *logical record* refers to
the logical organization of many characters into something meaningful
to the application. The term *unit record* describes a small set
of characters which are transmitted whole to or by the application,
and often refers to a line of text. Those two last terms are unrelated
to what we call a *record* in GNU `tar`.

When writing to tapes, `tar` writes the contents of the archive
in chunks known as *records*. To change the default blocking
factor, use the `--blocking-factor=512-size` (`-b
512-size`) option. Each record will then be composed of
512-size blocks. (Each `tar` block is 512 bytes.
See [Basic Tar Format](#Standard).) On some devices, a larger record size can be read
and written much more efficiently.

Further complicating the problem is that some tape drives ignore the
blocking entirely. For these, a larger record size can still improve
performance (because the software layers above the tape drive still
honor the blocking), but not as dramatically as on tape drives that
honor blocking.

When reading an archive, `tar` can usually figure out the
record size on itself. When this is the case, and a non-standard
record size was used when the archive was created, `tar` will
print a message about a non-standard blocking factor, and then operate
normally[27](#FOOT27). On some tape
devices, however, `tar` cannot figure out the record size
itself. On most of those, you can specify a blocking factor (with
`--blocking-factor`) larger than the actual blocking factor,
and then use the `--read-full-records` (`-B`) option.
(If you specify a blocking factor with `--blocking-factor` and
don’t use the `--read-full-records` option, then `tar`
will not attempt to figure out the recording size itself.) On some
devices, you must always specify the record size exactly with
`--blocking-factor` when reading, because `tar` cannot
figure it out. In any case, use `--list` (`-t`) before
doing any extractions to see whether `tar` is reading the archive
correctly.

`tar` blocks are all fixed size (512 bytes), and its scheme for
putting them into records is to put a whole number of them (one or
more) into each record. `tar` records are all the same size;
at the end of the file there’s a block containing all zeros, which
is how you tell that the remainder of the last record(s) are garbage.

In a standard `tar` file (no options), the block size is 512
and the record size is 10240, for a blocking factor of 20. What the
`--blocking-factor` option does is sets the blocking factor,
changing the record size while leaving the block size at 512 bytes.
20 was fine for ancient 800 or 1600 bpi reel-to-reel tape drives;
most tape drives these days prefer much bigger records in order to
stream and not waste tape. When writing tapes for myself, some tend
to use a factor of the order of 2048, say, giving a record size of
around one megabyte.

If you use a blocking factor larger than 20, older `tar`
programs might not be able to read the archive, so we recommend this
as a limit to use in practice. GNU `tar`, however,
will support arbitrarily large record sizes, limited only by the
amount of virtual memory or the physical characteristics of the tape
device.

* [Format Variations](#Format-Variations)
* [The Blocking Factor of an Archive](#Blocking-Factor)

---

Next: [The Blocking Factor of an Archive](#Blocking-Factor), Up: [Blocking](#Blocking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 9.4.1 Format Variations

> *(This message will disappear, once this node revised.)*

Format parameters specify how an archive is written on the archive
media. The best choice of format parameters will vary depending on
the type and number of files being archived, and on the media used to
store the archive.

To specify format parameters when accessing or creating an archive,
you can use the options described in the following sections.
If you do not specify any format parameters, `tar` uses
default parameters. You cannot modify a compressed archive.
If you create an archive with the `--blocking-factor` option
specified (see [The Blocking Factor of an Archive](#Blocking-Factor)), you must specify that
blocking-factor when operating on the archive. See [Controlling the Archive Format](#Formats), for other
examples of format parameter considerations.

---

Previous: [Format Variations](#Format-Variations), Up: [Blocking](#Blocking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 9.4.2 The Blocking Factor of an Archive

> *(This message will disappear, once this node revised.)*

The data in an archive is grouped into blocks, which are 512 bytes.
Blocks are read and written in whole number multiples called
*records*. The number of blocks in a record (i.e., the size of a
record in units of 512 bytes) is called the *blocking factor*.
The `--blocking-factor=512-size` (`-b
512-size`) option specifies the blocking factor of an archive.
The default blocking factor is typically 20 (i.e., 10240 bytes), but
can be specified at installation. To find out the blocking factor of
an existing archive, use ‘`tar --list --file=archive-name`’.
This may not work on some devices.

Records are separated by gaps, which waste space on the archive media.
If you are archiving on magnetic tape, using a larger blocking factor
(and therefore larger records) provides faster throughput and allows you
to fit more data on a tape (because there are fewer gaps). If you are
archiving on cartridge, a very large blocking factor (say 126 or more)
greatly increases performance. A smaller blocking factor, on the other
hand, may be useful when archiving small files, to avoid archiving lots
of nulls as `tar` fills out the archive to the end of the record.
In general, the ideal record size depends on the size of the
inter-record gaps on the tape you are using, and the average size of the
files you are archiving. See [How to Create Archives](#create), for information on
writing archives.

Archives with blocking factors larger than 20 cannot be read
by very old versions of `tar`, or by some newer versions
of `tar` running on old machines with small address spaces.
With GNU `tar`, the blocking factor of an archive is limited
only by the maximum record size of the device containing the archive,
or by the amount of available virtual memory.

Also, on some systems, not using adequate blocking factors, as sometimes
imposed by the device drivers, may yield unexpected diagnostics. For
example, this has been reported:

```
Cannot write to /dev/dlt: Invalid argument
```

In such cases, it sometimes happen that the `tar` bundled by
the system is aware of block size idiosyncrasies, while GNU `tar`
requires an explicit specification for the block size,
which it cannot guess. This yields some people to consider
GNU `tar` is misbehaving, because by comparison,
the bundle `tar` works OK. Adding `-b 256`,
for example, might resolve the problem.

If you use a non-default blocking factor when you create an archive, you
must specify the same blocking factor when you modify that archive. Some
archive devices will also require you to specify the blocking factor when
reading that archive, however this is not typically the case. Usually, you
can use `--list` (`-t`) without specifying a blocking factor—`tar`
reports a non-default record size and then lists the archive members as
it would normally. To extract files from an archive with a non-standard
blocking factor (particularly if you’re not sure what the blocking factor
is), you can usually use the `--read-full-records` (`-B`) option while
specifying a blocking factor larger then the blocking factor of the archive
(i.e., ‘`tar --extract --read-full-records --blocking-factor=300`’).
See [How to List Archives](#list), for more information on the `--list` (`-t`)
operation. See [Options to Help Read Archives](#Reading), for a more detailed explanation of that option.

`--blocking-factor=number`

`-b number`
:   Specifies the blocking factor of an archive. Can be used with any
    operation, but is usually not necessary with `--list` (`-t`).

Device blocking

`-b blocks`

`--blocking-factor=blocks`
:   Set record size to *blocks\*512* bytes.

    This option is used to specify a *blocking factor* for the archive.
    When reading or writing the archive, `tar`, will do reads and writes
    of the archive in records of *block\*512* bytes. This is true
    even when the archive is compressed. Some devices requires that all
    write operations be a multiple of a certain size, and so, `tar`
    pads the archive out to the next record boundary.

    The default blocking factor is set when `tar` is compiled, and is
    typically 20. Blocking factors larger than 20 cannot be read by very
    old versions of `tar`, or by some newer versions of `tar`
    running on old machines with small address spaces.

    With a magnetic tape, larger records give faster throughput and fit
    more data on a tape (because there are fewer inter-record gaps).
    If the archive is in a disk file or a pipe, you may want to specify
    a smaller blocking factor, since a large one will result in a large
    number of null bytes at the end of the archive.

    When writing cartridge or other streaming tapes, a much larger
    blocking factor (say 126 or more) will greatly increase performance.
    However, you must specify the same blocking factor when reading or
    updating the archive.

    Apparently, Exabyte drives have a physical block size of 8K bytes.
    If we choose our blocksize as a multiple of 8k bytes, then the problem
    seems to disappear. Id est, we are using block size of 112 right
    now, and we haven’t had the problem since we switched…

    With GNU `tar` the blocking factor is limited only
    by the maximum record size of the device containing the archive, or by
    the amount of available virtual memory.

    However, deblocking or reblocking is virtually avoided in a special
    case which often occurs in practice, but which requires all the
    following conditions to be simultaneously true:

    * the archive is subject to a compression option,
    * the archive is not handled through standard input or output, nor
      redirected nor piped,
    * the archive is directly handled to a local disk, instead of any special
      device,
    * `--blocking-factor` is not explicitly specified on the `tar`
      invocation.

    If the output goes directly to a local disk, and not through
    stdout, then the last write is not extended to a full record size.
    Otherwise, reblocking occurs. Here are a few other remarks on this
    topic:

    * `gzip` will complain about trailing garbage if asked to
      uncompress a compressed archive on tape, there is an option to turn
      the message off, but it breaks the regularity of simply having to use
      ‘`prog -d`’ for decompression. It would be nice if gzip was
      silently ignoring any number of trailing zeros. I’ll ask Jean-loup
      Gailly, by sending a copy of this message to him.
    * `compress` does not show this problem, but as Jean-loup pointed
      out to Michael, ‘`compress -d`’ silently adds garbage after
      the result of decompression, which tar ignores because it already
      recognized its end-of-file indicator. So this bug may be safely
      ignored.
    * ‘`gzip -d -q`’ will be silent about the trailing zeros indeed,
      but will still return an exit status of 2 which tar reports in turn.
      `tar` might ignore the exit status returned, but I hate doing
      that, as it weakens the protection `tar` offers users against
      other possible problems at decompression time. If `gzip` was
      silently skipping trailing zeros *and* also avoiding setting the
      exit status in this innocuous case, that would solve this situation.
    * `tar` should become more solid at not stopping to read a pipe at
      the first null block encountered. This inelegantly breaks the pipe.
      `tar` should rather drain the pipe out before exiting itself.

`-i`

`--ignore-zeros`
:   Ignore blocks of zeros in archive (means EOF).

    The `--ignore-zeros` (`-i`) option causes `tar` to ignore blocks
    of zeros in the archive. Normally a block of zeros indicates the
    end of the archive, but when reading a damaged archive, or one which
    was created by concatenating several archives together, this option
    allows `tar` to read the entire archive. This option is not on
    by default because many versions of `tar` write garbage after
    the zeroed blocks.

    Note that this option causes `tar` to read to the end of the
    archive file, which may sometimes avoid problems when multiple files
    are stored on a single physical tape.

`-B`

`--read-full-records`
:   Reblock as we read (for reading 4.2BSD pipes).

    If `--read-full-records` is used, `tar`
    will not panic if an attempt to read a record from the archive does
    not return a full record. Instead, `tar` will keep reading
    until it has obtained a full
    record.

    This option is turned on by default when `tar` is reading
    an archive from standard input, or from a remote machine. This is
    because on BSD Unix systems, a read of a pipe will return however
    much happens to be in the pipe, even if it is less than `tar`
    requested. If this option was not used, `tar` would fail as
    soon as it read an incomplete record from the pipe.

    This option is also useful with the commands for updating an archive.

Tape blocking

When handling various tapes or cartridges, you have to take care of
selecting a proper blocking, that is, the number of disk blocks you
put together as a single tape block on the tape, without intervening
tape gaps. A *tape gap* is a small landing area on the tape
with no information on it, used for decelerating the tape to a
full stop, and for later regaining the reading or writing speed.
When the tape driver starts reading a record, the record has to
be read whole without stopping, as a tape gap is needed to stop the
tape motion without losing information.

Using higher blocking (putting more disk blocks per tape block) will use
the tape more efficiently as there will be less tape gaps. But reading
such tapes may be more difficult for the system, as more memory will be
required to receive at once the whole record. Further, if there is a
reading error on a huge record, this is less likely that the system will
succeed in recovering the information. So, blocking should not be too
low, nor it should be too high. `tar` uses by default a blocking of
20 for historical reasons, and it does not really matter when reading or
writing to disk. Current tape technology would easily accommodate higher
blockings. Sun recommends a blocking of 126 for Exabytes and 96 for DATs.
We were told that for some DLT drives, the blocking should be a multiple
of 4Kb, preferably 64Kb (`-b 128`) or 256 for decent performance.
Other manufacturers may use different recommendations for the same tapes.
This might also depends of the buffering techniques used inside modern
tape controllers. Some imposes a minimum blocking, or a maximum blocking.
Others request blocking to be some exponent of two.

So, there is no fixed rule for blocking. But blocking at read time
should ideally be the same as blocking used at write time. At one place
I know, with a wide variety of equipment, they found it best to use a
blocking of 32 to guarantee that their tapes are fully interchangeable.

I was also told that, for recycled tapes, prior erasure (by the same
drive unit that will be used to create the archives) sometimes lowers
the error rates observed at rewriting time.

I might also use `--number-blocks` instead of
`--block-number`, so `--block` will then expand to
`--blocking-factor` unambiguously.

---

Next: [Using Multiple Tapes](#Using-Multiple-Tapes), Previous: [Blocking](#Blocking), Up: [Tapes and Other Archive Media](#Media)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 9.5 Many Archives on One Tape

Most tape devices have two entries in the `/dev` directory, or
entries that come in pairs, which differ only in the minor number for
this device. Let’s take for example `/dev/tape`, which often
points to the only or usual tape device of a given system. There might
be a corresponding `/dev/nrtape` or `/dev/ntape`. The simpler
name is the *rewinding* version of the device, while the name
having ‘`nr`’ in it is the *no rewinding* version of the same
device.

A rewinding tape device will bring back the tape to its beginning point
automatically when this device is opened or closed. Since `tar`
opens the archive file before using it and closes it afterwards, this
means that a simple:

```
$ tar cf /dev/tape directory
```

will reposition the tape to its beginning both prior and after saving
directory contents to it, thus erasing prior tape contents and
making it so that any subsequent write operation will destroy what has
just been saved.

So, a rewinding device is normally meant to hold one and only one file.
If you want to put more than one `tar` archive on a given tape, you
will need to avoid using the rewinding version of the tape device. You
will also have to pay special attention to tape positioning. Errors in
positioning may overwrite the valuable data already on your tape. Many
people, burnt by past experiences, will only use rewinding devices and
limit themselves to one file per tape, precisely to avoid the risk of
such errors. Be fully aware that writing at the wrong position on a
tape loses all information past this point and most probably until the
end of the tape, and this destroyed information *cannot* be
recovered.

To save directory-1 as a first archive at the beginning of a
tape, and leave that tape ready for a second archive, you should use:

```
$ mt -f /dev/nrtape rewind
$ tar cf /dev/nrtape directory-1
```

*Tape marks* are special magnetic patterns written on the tape
media, which are later recognizable by the reading hardware. These
marks are used after each file, when there are many on a single tape.
An empty file (that is to say, two tape marks in a row) signal the
logical end of the tape, after which no file exist. Usually,
non-rewinding tape device drivers will react to the close request issued
by `tar` by first writing two tape marks after your archive, and by
backspacing over one of these. So, if you remove the tape at that time
from the tape drive, it is properly terminated. But if you write
another file at the current position, the second tape mark will be
erased by the new information, leaving only one tape mark between files.

So, you may now save directory-2 as a second archive after the
first on the same tape by issuing the command:

```
$ tar cf /dev/nrtape directory-2
```

and so on for all the archives you want to put on the same tape.

Another usual case is that you do not write all the archives the same
day, and you need to remove and store the tape between two archive
sessions. In general, you must remember how many files are already
saved on your tape. Suppose your tape already has 16 files on it, and
that you are ready to write the 17th. You have to take care of skipping
the first 16 tape marks before saving directory-17, say, by using
these commands:

```
$ mt -f /dev/nrtape rewind
$ mt -f /dev/nrtape fsf 16
$ tar cf /dev/nrtape directory-17
```

In all the previous examples, we put aside blocking considerations, but
you should do the proper things for that as well. See [Blocking](#Blocking).

* [Tape Positions and Tape Marks](#Tape-Positioning)
* [The `mt` Utility](#mt)

---

Next: [The `mt` Utility](#mt), Up: [Many Archives on One Tape](#Many)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 9.5.1 Tape Positions and Tape Marks

> *(This message will disappear, once this node revised.)*

Just as archives can store more than one file from the file system,
tapes can store more than one archive file. To keep track of where
archive files (or any other type of file stored on tape) begin and
end, tape archive devices write magnetic *tape marks* on the
archive media. Tape drives write one tape mark between files,
two at the end of all the file entries.

If you think of data as a series of records "rrrr"’s, and tape marks as
"\*"’s, a tape might look like the following:

```
rrrr*rrrrrr*rrrrr*rr*rrrrr**-------------------------
```

Tape devices read and write tapes using a read/write *tape
head*—a physical part of the device which can only access one
point on the tape at a time. When you use `tar` to read or
write archive data from a tape device, the device will begin reading
or writing from wherever on the tape the tape head happens to be,
regardless of which archive or what part of the archive the tape
head is on. Before writing an archive, you should make sure that no
data on the tape will be overwritten (unless it is no longer needed).
Before reading an archive, you should make sure the tape head is at
the beginning of the archive you want to read. You can do it manually
via `mt` utility (see [The `mt` Utility](#mt)). The `restore` script does
that automatically (see [Using the Restore Script](#Scripted-Restoration)).

If you want to add new archive file entries to a tape, you should
advance the tape to the end of the existing file entries, backspace
over the last tape mark, and write the new archive file. If you were
to add two archives to the example above, the tape might look like the
following:

```
rrrr*rrrrrr*rrrrr*rr*rrrrr*rrr*rrrr**----------------
```

---

Previous: [Tape Positions and Tape Marks](#Tape-Positioning), Up: [Many Archives on One Tape](#Many)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 9.5.2 The `mt` Utility

> *(This message will disappear, once this node revised.)*

See [The Blocking Factor of an Archive](#Blocking-Factor).

You can use the `mt` utility to advance or rewind a tape past a
specified number of archive files on the tape. This will allow you
to move to the beginning of an archive before extracting or reading
it, or to the end of all the archives before writing a new one.

The syntax of the `mt` command is:

```
mt [-f tapename] operation [number]
```

where tapename is the name of the tape device, number is
the number of times an operation is performed (with a default of one),
and operation is one of the following:

`eof`

`weof`
:   Writes number tape marks at the current position on the tape.

`fsf`
:   Moves tape position forward number files.

`bsf`
:   Moves tape position back number files.

`rewind`
:   Rewinds the tape. (Ignores number.)

`offline`

`rewoff1`
:   Rewinds the tape and takes the tape device off-line. (Ignores number.)

`status`
:   Prints status information about the tape unit.

If you don’t specify a tapename, `mt` uses the environment
variable `TAPE`; if `TAPE` is not set, `mt` will use
the default device specified in your `sys/mtio.h` file
(`DEFTAPE` variable). If this is not defined, the program will
display a descriptive error message and exit with code 1.

`mt` returns a 0 exit status when the operation(s) were
successful, 1 if the command was unrecognized, and 2 if an operation
failed.

---

Next: [Including a Label in the Archive](#label), Previous: [Many Archives on One Tape](#Many), Up: [Tapes and Other Archive Media](#Media)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 9.6 Using Multiple Tapes

Often you might want to write a large archive, one larger than will fit
on the actual tape you are using. In such a case, you can run multiple
`tar` commands, but this can be inconvenient, particularly if you
are using options like `--exclude=pattern` or dumping entire file systems.
Therefore, `tar` provides a special mode for creating
multi-volume archives.

*Multi-volume* archive is a single `tar` archive, stored
on several media volumes of fixed size. Although in this section we will
often call ‘`volume`’ a *tape*, there is absolutely no
requirement for multi-volume archives to be stored on tapes. Instead,
they can use whatever media type the user finds convenient, they can
even be located on files.

When creating a multi-volume archive, GNU `tar` continues to fill
current volume until it runs out of space, then it switches to
next volume (usually the operator is queried to replace the tape on
this point), and continues working on the new volume. This operation
continues until all requested files are dumped. If GNU `tar` detects
end of media while dumping a file, such a file is archived in split
form. Some very big files can even be split across several volumes.

Each volume is itself a valid GNU `tar` archive, so it can be read
without any special options. Consequently any file member residing
entirely on one volume can be extracted or otherwise operated upon
without needing the other volume. Sure enough, to extract a split
member you would need all volumes its parts reside on.

Multi-volume archives suffer from several limitations. In particular,
they cannot be compressed.

GNU `tar` is able to create multi-volume archives of two formats
(see [Controlling the Archive Format](#Formats)): ‘`GNU`’ and ‘`POSIX`’.

* [Archives Longer than One Tape or Disk](#Multi_002dVolume-Archives)
* [Tape Files](#Tape-Files)
* [Concatenate Volumes into a Single Archive](#Tarcat)

---

Next: [Tape Files](#Tape-Files), Up: [Using Multiple Tapes](#Using-Multiple-Tapes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 9.6.1 Archives Longer than One Tape or Disk

To create an archive that is larger than will fit on a single unit of
the media, use the `--multi-volume` (`-M`) option in conjunction with
the `--create` option (see [How to Create Archives](#create)). A *multi-volume*
archive can be manipulated like any other archive (provided the
`--multi-volume` option is specified), but is stored on more
than one tape or file.

When you specify `--multi-volume`, `tar` does not report an
error when it comes to the end of an archive volume (when reading), or
the end of the media (when writing). Instead, it prompts you to load
a new storage volume. If the archive is on a magnetic tape, you
should change tapes when you see the prompt; if the archive is on a
floppy disk, you should change disks; etc.

`--multi-volume`

`-M`
:   Creates a multi-volume archive, when used in conjunction with
    `--create` (`-c`). To perform any other operation on a multi-volume
    archive, specify `--multi-volume` in conjunction with that
    operation.
    For example:

    ```
    $ tar --create --multi-volume --file=/dev/tape files
    ```

The method `tar` uses to detect end of tape is not perfect, and
fails on some operating systems or on some devices. If `tar`
cannot detect the end of the tape itself, you can use
`--tape-length` option to inform it about the capacity of the
tape:

`--tape-length=size[suf]`

`-L size[suf]`
:   Set maximum length of a volume. The suf, if given, specifies
    units in which size is expressed, e.g. ‘`2M`’ mean 2
    megabytes (see [Table 9.1](#size_002dsuffixes), for a list of allowed size
    suffixes). Without suf, units of 1024 bytes (kilobyte) are
    assumed.

    This option selects `--multi-volume` automatically. For example:

    ```
    $ tar --create --tape-length=41943040 --file=/dev/tape files
    ```

    or, which is equivalent:

    ```
    $ tar --create --tape-length=4G --file=/dev/tape files
    ```

When GNU `tar` comes to the end of a storage media, it asks you to
change the volume. The built-in prompt for POSIX locale
is[28](#FOOT28):

```
Prepare volume #n for 'archive' and hit return:
```

where n is the ordinal number of the volume to be created and
archive is archive file or device name.

When prompting for a new tape, `tar` accepts any of the following
responses:

`?`
:   Request `tar` to explain possible responses.

`q`
:   Request `tar` to exit immediately.

`n file-name`
:   Request `tar` to write the next volume on the file file-name.

`!`
:   Request `tar` to run a subshell. This option can be disabled
    by giving `--restrict` command line option to
    `tar`[29](#FOOT29).

`y`
:   Request `tar` to begin writing the next volume.

(You should only type ‘`y`’ after you have changed the tape;
otherwise `tar` will write over the volume it just finished.)

The volume number used by `tar` in its tape-changing prompt
can be changed; if you give the
`--volno-file=file-of-number` option, then
file-of-number should be an non-existing file to be created, or
else, a file already containing a decimal number. That number will be
used as the volume number of the first volume written. When
`tar` is finished, it will rewrite the file with the
now-current volume number. (This does not change the volume number
written on a tape label, as per [Including a Label in the Archive](#label), it *only* affects
the number used in the prompt.)

If you want more elaborate behavior than this, you can write a special
*new volume script*, that will be responsible for changing the
volume, and instruct `tar` to use it instead of its normal
prompting procedure:

`--info-script=command`

`--new-volume-script=command`

`-F command`
:   Specify the command to invoke when switching volumes. The command
    can be used to eject cassettes, or to broadcast messages such as
    ‘`Someone please come change my tape`’ when performing unattended
    backups.

The command can contain additional options, if such are needed.
See [Running External Commands](#external), for a detailed discussion
of the way GNU `tar` runs external commands. It inherits
`tar`’s shell environment. Additional data is passed to it
via the following environment variables:

`TAR_VERSION`
:   GNU `tar` version number.

`TAR_ARCHIVE`
:   The name of the archive `tar` is processing.

`TAR_BLOCKING_FACTOR`
:   Current blocking factor (see [Blocking](#Blocking)).

`TAR_VOLUME`
:   Ordinal number of the volume `tar` is about to start.

`TAR_SUBCOMMAND`
:   A short option describing the operation `tar` is executing.
    See [The Five Advanced `tar` Operations](#Operations), for a complete list of subcommand options.

`TAR_FORMAT`
:   Format of the archive being processed. See [Controlling the Archive Format](#Formats), for a complete
    list of archive format names.

`TAR_FD`
:   File descriptor which can be used to communicate the new volume
    name to `tar`.

These variables can be used in the command itself, provided that
they are properly quoted to prevent them from being expanded by the
shell that invokes `tar`.

The volume script can instruct `tar` to use new archive name,
by writing in to file descriptor `$TAR_FD` (see below for an example).

If the info script fails, `tar` exits; otherwise, it begins
writing the next volume.

If you want `tar` to cycle through a series of files or tape
drives, there are three approaches to choose from. First of all, you
can give `tar` multiple `--file` options. In this case
the specified files will be used, in sequence, as the successive
volumes of the archive. Only when the first one in the sequence needs
to be used again will `tar` prompt for a tape change (or run
the info script). For example, suppose someone has two tape drives on
a system named `/dev/tape0` and `/dev/tape1`. For having
GNU `tar` to switch to the second drive when it needs to write the
second tape, and then back to the first tape, etc., just do either of:

```
$ tar --create --multi-volume --file=/dev/tape0 --file=/dev/tape1 files
$ tar -cM -f /dev/tape0 -f /dev/tape1 files
```

The second method is to use the ‘`n`’ response to the tape-change
prompt.

Finally, the most flexible approach is to use a volume script, that
writes new archive name to the file descriptor `$TAR_FD`. For example, the
following volume script will create a series of archive files, named
`archive-vol`, where archive is the name of the
archive being created (as given by `--file` option) and
vol is the ordinal number of the archive being created:

```
#! /bin/bash
# For this script it's advisable to use a shell, such as Bash,
# that supports a TAR_FD value greater than 9.

echo Preparing volume $TAR_VOLUME of $TAR_ARCHIVE.

name=`expr $TAR_ARCHIVE : '\(.*\)-.*'`
case $TAR_SUBCOMMAND in
-c)       ;;
-d|-x|-t) test -r ${name:-$TAR_ARCHIVE}-$TAR_VOLUME || exit 1
          ;;
*)        exit 1
esac

echo ${name:-$TAR_ARCHIVE}-$TAR_VOLUME >&$TAR_FD
```

The same script can be used while listing, comparing or extracting
from the created archive. For example:

```
# Create a multi-volume archive:
$ tar -c -L1024 -f archive.tar -F new-volume .
# Extract from the created archive:
$ tar -x -f archive.tar -F new-volume .
```

Notice, that the first command had to use `-L` option, since
otherwise GNU `tar` will end up writing everything to file
`archive.tar`.

You can read each individual volume of a multi-volume archive as if it
were an archive by itself. For example, to list the contents of one
volume, use `--list`, without `--multi-volume` specified.
To extract an archive member from one volume (assuming it is described
that volume), use `--extract`, again without
`--multi-volume`.

If an archive member is split across volumes (i.e., its entry begins on
one volume of the media and ends on another), you need to specify
`--multi-volume` to extract it successfully. In this case, you
should load the volume where the archive member starts, and use
‘`tar --extract --multi-volume`’—`tar` will prompt for later
volumes as it needs them. See [Extracting an Entire Archive](#extracting-archives), for more
information about extracting archives.

Multi-volume archives can be modified like any other archive. To add
files to a multi-volume archive, you need to only mount the last
volume of the archive media (and new volumes, if needed). For all
other operations, you need to use the entire archive.

If a multi-volume archive was labeled using
`--label=archive-label` (see [Including a Label in the Archive](#label)) when it was
created, `tar` will not automatically label volumes which are
added later. To label subsequent volumes, specify
`--label=archive-label` again in conjunction with the
`--append`, `--update` or `--concatenate` operation.

Notice that multi-volume support is a GNU extension and the archives
created in this mode should be read only using GNU `tar`. If you
absolutely have to process such archives using a third-party `tar`
implementation, read [Extracting Members Split Between Volumes](#Split-Recovery).

---

Next: [Concatenate Volumes into a Single Archive](#Tarcat), Previous: [Archives Longer than One Tape or Disk](#Multi_002dVolume-Archives), Up: [Using Multiple Tapes](#Using-Multiple-Tapes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 9.6.2 Tape Files

> *(This message will disappear, once this node revised.)*

To give the archive a name which will be recorded in it, use the
`--label=volume-label` (`-V volume-label`)
option. This will write a special block identifying
volume-label as the name of the archive to the front of the
archive which will be displayed when the archive is listed with
`--list`. If you are creating a multi-volume archive with
`--multi-volume` (see [Using Multiple Tapes](#Using-Multiple-Tapes)), then the
volume label will have ‘`Volume nnn`’ appended to the name
you give, where nnn is the number of the volume of the archive.
If you use the `--label=volume-label` option when
reading an archive, it checks to make sure the label on the tape
matches the one you gave. See [Including a Label in the Archive](#label).

When `tar` writes an archive to tape, it creates a single
tape file. If multiple archives are written to the same tape, one
after the other, they each get written as separate tape files. When
extracting, it is necessary to position the tape at the right place
before running `tar`. To do this, use the `mt` command.
For more information on the `mt` command and on the organization
of tapes into a sequence of tape files, see [The `mt` Utility](#mt).

People seem to often do:

```
--label="some-prefix `date +some-format`"
```

or such, for pushing a common date in all volumes or an archive set.

---

Previous: [Tape Files](#Tape-Files), Up: [Using Multiple Tapes](#Using-Multiple-Tapes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 9.6.3 Concatenate Volumes into a Single Archive

Sometimes it is necessary to convert existing GNU `tar` multi-volume
archive to a single `tar` archive. Simply concatenating all
volumes into one will not work, since each volume carries an additional
information at the beginning. GNU `tar` is shipped with the shell
script `tarcat` designed for this purpose.

The script takes a list of files comprising a multi-volume archive
and creates the resulting archive at the standard output. For example:

```
tarcat vol.1 vol.2 vol.3 | tar tf -
```

The script implements a simple heuristics to determine the format of
the first volume file and to decide how to process the rest of the
files. However, it makes no attempt to verify whether the files are
given in order or even if they are valid `tar` archives.
It uses `dd` and does not filter its standard error, so you
will usually see lots of spurious messages.

---

Next: [Verifying Data as It is Stored](#verify), Previous: [Using Multiple Tapes](#Using-Multiple-Tapes), Up: [Tapes and Other Archive Media](#Media)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 9.7 Including a Label in the Archive

To avoid problems caused by misplaced paper labels on the archive
media, you can include a *label* entry — an archive member which
contains the name of the archive — in the archive itself. Use the
`--label=archive-label` (`-V archive-label`)
option[30](#FOOT30) in
conjunction with the `--create` operation to include a label
entry in the archive as it is being created.

`--label=archive-label`

`-V archive-label`
:   Includes an *archive-label* at the beginning of the archive when
    the archive is being created, when used in conjunction with the
    `--create` operation. Checks to make sure the archive label
    matches the one specified (when used in conjunction with any other
    operation).

If you create an archive using both
`--label=archive-label` (`-V archive-label`)
and `--multi-volume` (`-M`), each volume of the archive
will have an archive label of the form ‘`archive-label
Volume n`’, where n is 1 for the first volume, 2 for the
next, and so on. See [Using Multiple Tapes](#Using-Multiple-Tapes), for information on
creating multiple volume archives.

The volume label will be displayed by `--list` along with
the file contents. If verbose display is requested, it will also be
explicitly marked as in the example below:

```
$ tar --verbose --list --file=iamanarchive
V--------- 0/0               0 1992-03-07 12:01 iamalabel--Volume Header--
-rw-r--r-- ringo/user       40 1990-05-21 13:30 iamafilename
```

However, `--list` option will cause listing entire
contents of the archive, which may be undesirable (for example, if the
archive is stored on a tape). You can request checking only the volume
label by specifying `--test-label` option. This option reads only the
first block of an archive, so it can be used with slow storage
devices. For example:

```
$ tar --test-label --file=iamanarchive
iamalabel
```

If `--test-label` is used with one or more command line
arguments, `tar` compares the volume label with each
argument. It exits with code 0 if a match is found, and with code 1
otherwise[31](#FOOT31). No output is displayed, unless you also used the
`--verbose` option. For example:

```
$ tar --test-label --file=iamanarchive 'iamalabel'
⇒ 0
$ tar --test-label --file=iamanarchive 'alabel'
⇒ 1
```

When used with the `--verbose` option, `tar`
prints the actual volume label (if any), and a verbose diagnostics in
case of a mismatch:

```
$ tar --test-label --verbose --file=iamanarchive 'iamalabel'
iamalabel
⇒ 0
$ tar --test-label --verbose --file=iamanarchive 'alabel'
iamalabel
tar: Archive label mismatch
⇒ 1
```

If you request any operation, other than `--create`, along
with using `--label` option, `tar` will first check if
the archive label matches the one specified and will refuse to proceed
if it does not. Use this as a safety precaution to avoid accidentally
overwriting existing archives. For example, if you wish to add files
to `archive`, presumably labeled with string ‘`My volume`’,
you will get:

```
$ tar -rf archive --label 'My volume' .
tar: Archive not labeled to match 'My volume'
```

in case its label does not match. This will work even if
`archive` is not labeled at all.

Similarly, `tar` will refuse to list or extract the
archive if its label doesn’t match the archive-label
specified. In those cases, archive-label argument is interpreted
as a globbing-style pattern which must match the actual magnetic
volume label. See [Excluding Some Files](#exclude), for a precise description of how match
is attempted[32](#FOOT32). If the switch `--multi-volume` (`-M`) is being used,
the volume label matcher will also suffix archive-label by
‘`Volume [1-9]*`’ if the initial match fails, before giving
up. Since the volume numbering is automatically added in labels at
creation time, it sounded logical to equally help the user taking care
of it when the archive is being read.

You can also use `--label` to get a common information on
all tapes of a series. For having this information different in each
series created through a single script used on a regular basis, just
manage to get some date string as part of the label. For example:

```
$ tar -cM -f /dev/tape -V "Daily backup for `date +%Y-%m-%d`"
$ tar --create --file=/dev/tape --multi-volume \
     --label="Daily backup for `date +%Y-%m-%d`"
```

Some more notes about volume labels:

* Each label has its own date and time, which corresponds
  to the time when GNU `tar` initially attempted to write it,
  often soon after the operator launches `tar` or types the
  carriage return telling that the next tape is ready.
* Comparing date labels to get an idea of tape throughput is
  unreliable. It gives correct results only if the delays for rewinding
  tapes and the operator switching them were negligible, which is
  usually not the case.

---

Next: [Write Protection](#Write-Protection), Previous: [Including a Label in the Archive](#label), Up: [Tapes and Other Archive Media](#Media)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 9.8 Verifying Data as It is Stored

`-W` [¶](#index-verify_002c-short-description)

`--verify`
:   Attempt to verify the archive after writing.

This option causes `tar` to verify the archive after writing it.
Each volume is checked after it is written, and any discrepancies
are recorded on the standard error output.

Verification requires that the archive be on a back-space-able medium.
This means pipes, some cartridge tape drives, and some other devices
cannot be verified.

You can insure the accuracy of an archive by comparing files in the
system with archive members. `tar` can compare an archive to the
file system as the archive is being written, to verify a write
operation, or can compare a previously written archive, to insure that
it is up to date.

To check for discrepancies in an archive immediately after it is
written, use the `--verify` (`-W`) option in conjunction with
the `--create` operation. When this option is
specified, `tar` checks archive members against their counterparts
in the file system, and reports discrepancies on the standard error.

To verify an archive, you must be able to read it from before the end
of the last written entry. This option is useful for detecting data
errors on some tapes. Archives written to pipes, some cartridge tape
drives, and some other devices cannot be verified.

One can explicitly compare an already made archive with the file
system by using the `--compare` (`--diff`, `-d`)
option, instead of using the more automatic `--verify` option.
See [Comparing Archive Members with the File System](#compare).

Note that these two options have a slightly different intent. The
`--compare` option checks how identical are the logical contents of some
archive with what is on your disks, while the `--verify` option is
really for checking if the physical contents agree and if the recording
media itself is of dependable quality. So, for the `--verify`
operation, `tar` tries to defeat all in-memory cache pertaining to
the archive, while it lets the speed optimization undisturbed for the
`--compare` option. If you nevertheless use `--compare` for
media verification, you may have to defeat the in-memory cache yourself,
maybe by opening and reclosing the door latch of your recording unit,
forcing some doubt in your operating system about the fact this is really
the same volume as the one just written or read.

The `--verify` option would not be necessary if drivers were indeed
able to detect dependably all write failures. This sometimes require many
magnetic heads, some able to read after the writes occurred. One would
not say that drivers unable to detect all cases are necessarily flawed,
as long as programming is concerned.

The `--verify` (`-W`) option will not work in
conjunction with the `--multi-volume` (`-M`) option or
the `--append` (`-r`), `--update` (`-u`)
and `--delete` operations. See [The Five Advanced `tar` Operations](#Operations), for more
information on these operations.

Also, since `tar` normally strips leading ‘`/`’ from file
names (see [Absolute File Names](#absolute)), a command like ‘`tar --verify -cf
/tmp/foo.tar /etc`’ will work as desired only if the working directory is
`/`, as `tar` uses the archive’s relative member names
(e.g., `etc/motd`) when verifying the archive.

---

Previous: [Verifying Data as It is Stored](#verify), Up: [Tapes and Other Archive Media](#Media)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 9.9 Write Protection

Almost all tapes and diskettes, and in a few rare cases, even disks can
be *write protected*, to protect data on them from being changed.
Once an archive is written, you should write protect the media to prevent
the archive from being accidentally overwritten or deleted. (This will
protect the archive from being changed with a tape or floppy drive—it
will not protect it from magnet fields or other physical hazards.)

The write protection device itself is usually an integral part of the
physical media, and can be a two position (write enabled/write
disabled) switch, a notch which can be popped out or covered, a ring
which can be removed from the center of a tape reel, or some other
changeable feature.

---

Next: [Changes](#Changes), Previous: [Tapes and Other Archive Media](#Media), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## 10 Reliability and Security

The `tar` command reads and writes files as any other
application does, and is subject to the usual caveats about
reliability and security. This section contains some commonsense
advice on the topic.

* [Reliability](#Reliability)
* [Security](#Security)

---

Next: [Security](#Security), Up: [Reliability and Security](#Reliability-and-security)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 10.1 Reliability

Ideally, when `tar` is creating an archive, it reads from a
file system that is not being modified, and encounters no errors or
inconsistencies while reading and writing. If this is the case, the
archive should faithfully reflect what was read. Similarly, when
extracting from an archive, ideally `tar` ideally encounters
no errors and the extracted files faithfully reflect what was in the
archive.

However, when reading or writing real-world file systems, several
things can go wrong; these include permissions problems, corruption of
data, and race conditions.

* [Permissions Problems](#Permissions-problems)
* [Data Corruption and Repair](#Data-corruption-and-repair)
* [Race conditions](#Race-conditions)

---

Next: [Data Corruption and Repair](#Data-corruption-and-repair), Up: [Reliability](#Reliability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 10.1.1 Permissions Problems

If `tar` encounters errors while reading or writing files, it
normally reports an error and exits with nonzero status. The work it
does may therefore be incomplete. For example, when creating an
archive, if `tar` cannot read a file then it cannot copy the
file into the archive.

---

Next: [Race conditions](#Race-conditions), Previous: [Permissions Problems](#Permissions-problems), Up: [Reliability](#Reliability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 10.1.2 Data Corruption and Repair

If an archive becomes corrupted by an I/O error, this may corrupt the
data in an extracted file. Worse, it may corrupt the file’s metadata,
which may cause later parts of the archive to become misinterpreted.
An tar-format archive contains a checksum that most likely will detect
errors in the metadata, but it will not detect errors in the data.

If data corruption is a concern, you can compute and check your own
checksums of an archive by using other programs, such as
`cksum`.

When attempting to recover from a read error or data corruption in an
archive, you may need to skip past the questionable data and read the
rest of the archive. This requires some expertise in the archive
format and in other software tools.

---

Previous: [Data Corruption and Repair](#Data-corruption-and-repair), Up: [Reliability](#Reliability)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 10.1.3 Race conditions

If some other process is modifying the file system while `tar`
is reading or writing files, the result may well be inconsistent due
to race conditions. For example, if another process creates some
files in a directory while `tar` is creating an archive
containing the directory’s files, `tar` may see some of the
files but not others, or it may see a file that is in the process of
being created. The resulting archive may not be a snapshot of the
file system at any point in time. If an application such as a
database system depends on an accurate snapshot, restoring from the
`tar` archive of a live file system may therefore break that
consistency and may break the application. The simplest way to avoid
the consistency issues is to avoid making other changes to the file
system while tar is reading it or writing it.

When creating an archive, several options are available to avoid race
conditions. Some hosts have a way of snapshotting a file system, or
of temporarily suspending all changes to a file system, by (say)
suspending the only virtual machine that can modify a file system; if
you use these facilities and have `tar -c` read from a
snapshot when creating an archive, you can avoid inconsistency
problems. More drastically, before starting `tar` you could
suspend or shut down all processes other than `tar` that have
access to the file system, or you could unmount the file system and
then mount it read-only.

When extracting from an archive, one approach to avoid race conditions
is to create a directory that no other process can write to, and
extract into that.

---

Previous: [Reliability](#Reliability), Up: [Reliability and Security](#Reliability-and-security)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 10.2 Security

In some cases `tar` may be used in an adversarial situation,
where an untrusted user is attempting to gain information about or
modify otherwise-inaccessible files. Dealing with untrusted data
(that is, data generated by an untrusted user) typically requires
extra care, because even the smallest mistake in the use of
`tar` is more likely to be exploited by an adversary than by a
race condition.

* [Privacy](#Privacy)
* [Integrity](#Integrity)
* [Dealing with Live Untrusted Data](#Live-untrusted-data)
* [Security Rules of Thumb](#Security-rules-of-thumb)

---

Next: [Integrity](#Integrity), Up: [Security](#Security)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 10.2.1 Privacy

Standard privacy concerns apply when using `tar`. For
example, suppose you are archiving your home directory into a file
`/archive/myhome.tar`. Any secret information in your home
directory, such as your SSH secret keys, are copied faithfully into
the archive. Therefore, if your home directory contains any file that
should not be read by some other user, the archive itself should be
not be readable by that user. And even if the archive’s data are
inaccessible to untrusted users, its metadata (such as size or
last-modified date) may reveal some information about your home
directory; if the metadata are intended to be private, the archive’s
parent directory should also be inaccessible to untrusted users.

One precaution is to create `/archive` so that it is not
accessible to any user, unless that user also has permission to access
all the files in your home directory.

Similarly, when extracting from an archive, take care that the
permissions of the extracted files are not more generous than what you
want. Even if the archive itself is readable only to you, files
extracted from it have their own permissions that may differ.

---

Next: [Dealing with Live Untrusted Data](#Live-untrusted-data), Previous: [Privacy](#Privacy), Up: [Security](#Security)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 10.2.2 Integrity

When creating archives, take care that they are not writable by a
untrusted user; otherwise, that user could modify the archive, and
when you later extract from the archive you will get incorrect data.

When `tar` extracts from an archive, by default it writes into
files relative to the working directory. If the archive was generated
by an untrusted user, that user therefore can write into any file
under the working directory. When extracting from an
untrusted archive, it is therefore good practice to create an empty
directory and run `tar` in that directory. You can use the
`--directory` (`-C`) option to specify the working
directory (see [Changing the Working Directory](#directory)).

When extracting from an archive, `tar` by default rejects attempts to
modify files outside the working directory.
For example, if a symbolic link points outside the working directory,
`tar` refuses to follow the link, regardless of whether the
symbolic link existed before `tar` was run.
Therefore, when extracting from two or more untrusted archives,
each one can be extracted in turn, into the same initially-empty directory.
Even if an earlier archive creates a symbolic link that
points outside the working directory,
`tar` will reject any later attempts to follow that symbolic link.
However, this safety mechanism applies only to `tar` itself:
it does not apply to other programs you may run later, which will
ordinarily follow symbolic links even if they escape the working directory.

If you use the `--absolute-names` (`-P`) option when
extracting, `tar` respects any file names in the archive, even
file names that begin with ‘`/`’, contain ‘`..`’, or that follow
a symbolic link to escape the extraction directory.
If you use the `--dereference` (`-h`) option when extracting,
`tar` follows any existing symbolic link that is the last component of
a file name, even if that link escapes the extraction directory.
These two options should be used only for trusted archives, as they
can let an archive overwrite any file in your system that you can owrite.

Conversely, with the `--keep-old-files` (`-k`) and
`--skip-old-files` options, `tar` refuses to replace
existing files when extracting. The difference between the two
options is that the former treats existing files as errors whereas the
latter just silently ignores them.

Finally, with the `--no-overwrite-dir` option, `tar`
refuses to replace the permissions or ownership of already-existing
directories. These options may help when extracting from untrusted
archives.

---

Next: [Security Rules of Thumb](#Security-rules-of-thumb), Previous: [Integrity](#Integrity), Up: [Security](#Security)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 10.2.3 Dealing with Live Untrusted Data

Extra care is required when creating from or extracting into a file
system that is accessible to untrusted users. For example, superusers
who invoke `tar` must be wary about its actions being hijacked
by an adversary who is reading or writing the file system at the same
time that `tar` is operating.

When creating an archive from a live file system, `tar` is
vulnerable to denial-of-service attacks. For example, an adversarial
user could create the illusion of an indefinitely-deep directory
hierarchy `d/e/f/g/...` by creating directories one step ahead of
`tar`, or the illusion of an indefinitely-long file by
creating a sparse file but arranging for blocks to be allocated just
before `tar` reads them. There is no easy way for
`tar` to distinguish these scenarios from legitimate uses, so
you may need to monitor `tar`, just as you’d need to monitor
any other system service, to detect such attacks.

While a superuser is extracting from an archive into a live file
system, an untrusted user might replace a directory with a symbolic
link, in hopes that `tar` will follow the symbolic link and
extract data into files that the untrusted user does not have access
to. Even if the archive was generated by the superuser, it may
contain a file such as `d/etc/passwd` that the untrusted user
earlier created in order to break in; if the untrusted user replaces
the directory `d/etc` with a symbolic link to `/etc` while
`tar` is running, `tar` will overwrite
`/etc/passwd`. This attack can be prevented by extracting into a
directory that is inaccessible to untrusted users.

Similar attacks via symbolic links are also possible when creating an
archive, if the untrusted user can modify an ancestor of a top-level
argument of `tar`. For example, an untrusted user that can
modify `/home/eve` can hijack a running instance of ‘`tar -cf
- /home/eve/Documents/yesterday`’ by replacing
`/home/eve/Documents` with a symbolic link to some other
location. Attacks like these can be prevented by making sure that
untrusted users cannot modify any files that are top-level arguments
to `tar`, or any ancestor directories of these files.

---

Previous: [Dealing with Live Untrusted Data](#Live-untrusted-data), Up: [Security](#Security)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 10.2.4 Security Rules of Thumb

This section briefly summarizes rules of thumb for avoiding security
pitfalls.

* Protect archives at least as much as you protect any of the files
  being archived.
* Extract from untrusted archives only into an otherwise-empty
  directory. This directory and its parent should be accessible only to
  trusted users. For example:

  ```
  $ chmod go-rwx .
  $ mkdir -m go-rwx dir
  $ cd dir
  $ tar -xvf /archives/got-it-off-the-net.tar.gz
  ```
* Do not let untrusted users access files extracted from untrusted
  archives without checking first for problems such as setuid programs.
* Do not let untrusted users modify directories that are ancestors of
  top-level arguments of `tar`. For example, while you are
  executing ‘`tar -cf /archive/u-home.tar /u/home`’, do not let an
  untrusted user modify `/`, `/archive`, or `/u`.
* Pay attention to the diagnostics and exit status of `tar`.
* When archiving live file systems, monitor running instances of
  `tar` to detect denial-of-service attacks.
* Avoid unusual options such as `--absolute-names` (`-P`),
  `--dereference` (`-h`), `--overwrite`,
  `--recursive-unlink`, and `--remove-files` unless you
  understand their security implications.

---

Next: [Recipes](#Recipes), Previous: [Reliability and Security](#Reliability-and-security), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## Appendix A Changes

This appendix lists some important user-visible changes between
various versions of GNU `tar`. An up-to-date version of this document
is available at
[the
GNU `tar` documentation page](http://www.gnu.org/software/tar/manual/changes.html).

Use of globbing patterns when listing and extracting.
:   Previous versions of GNU tar assumed shell-style globbing when
    extracting from or listing an archive. For example:

    ```
    $ tar xf foo.tar '*.c'
    ```

    would extract all files whose names end in ‘`.c`’. This behavior
    was not documented and was incompatible with traditional tar
    implementations. Therefore, starting from version 1.15.91, GNU tar
    no longer uses globbing by default. For example, the above invocation
    is now interpreted as a request to extract from the archive the file
    named `*.c`.

    To facilitate transition to the new behavior for those users who got
    used to the previous incorrect one, `tar` will print a warning
    if it finds out that a requested member was not found in the archive
    and its name looks like a globbing pattern. For example:

    ```
    $ tar xf foo.tar  '*.c'
    tar: Pattern matching characters used in file names. Please,
    tar: use --wildcards to enable pattern matching, or --no-wildcards to
    tar: suppress this warning.
    tar: *.c: Not found in archive
    tar: Error exit delayed from previous errors
    ```

    To treat member names as globbing patterns, use the `--wildcards` option.
    If you want to tar to mimic the behavior of versions prior to 1.15.91,
    add this option to your `TAR_OPTIONS` variable.

    See [Wildcards Patterns and Matching](#wildcards), for the detailed discussion of the use of globbing
    patterns by GNU `tar`.

Use of short option `-o`.
:   Earlier versions of GNU `tar` understood `-o` command line
    option as a synonym for `--old-archive`.

    GNU `tar` starting from version 1.13.90 understands this option as
    a synonym for `--no-same-owner`. This is compatible with
    UNIX98 `tar` implementations.

    However, to facilitate transition, `-o` option retains its
    old semantics when it is used with one of archive-creation commands.
    Users are encouraged to use `--format=oldgnu` instead.

    It is especially important, since versions of GNU Automake
    up to and including 1.8.4 invoke tar with this option to produce
    distribution tarballs. See [v7](#Formats), for the detailed discussion
    of this issue and its implications.

    See [Changing Automake’s Behavior](https://www.gnu.org/software/automake/manual/automake.html#Options) in GNU Automake, for a description on how to use various
    archive formats with `automake`.

    Future versions of GNU `tar` will understand `-o` only as a
    synonym for `--no-same-owner`.

Use of short option `-l`
:   Earlier versions of GNU `tar` understood `-l` option as a
    synonym for `--one-file-system`. Since such usage contradicted
    to UNIX98 specification and harmed compatibility with other
    implementations, it was declared deprecated in version 1.14. However,
    to facilitate transition to its new semantics, it was supported by
    versions 1.15 and 1.15.90. The present use of `-l` as a short
    variant of `--check-links` was introduced in version 1.15.91.

Use of options `--portability` and `--old-archive`
:   These options are deprecated. Please use `--format=v7` instead.

Use of option `--posix`
:   This option is deprecated. Please use `--format=posix` instead.

---

Next: [Configuring Help Summary](#Configuring-Help-Summary), Previous: [Changes](#Changes), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## Appendix B Recipes

This appendix provides several recipes for performing common tasks
using GNU `tar`.

* [Copying directory hierarchies](#copy-directory-hierarchy)
* [Restoring Intermediate Directories](#intermediate-directories)

---

Next: [Restoring Intermediate Directories](#intermediate-directories), Up: [Recipes](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### B.1 Copying directory hierarchies

This is a traditional way to copy a directory hierarchy preserving
the dates, modes, owners and link-structure of all the files therein.
It was used back when the `cp` command lacked the `-a`
option:

```
$ (cd sourcedir; tar -cf - .) | (cd targetdir; tar -xf -)
```

You can avoid subshells by using `-C` option:

```
$ tar -C sourcedir -cf - . | tar -C targetdir -xf -
```

The same command using long option forms:

```
$ (cd sourcedir; tar --create --file=- . ) \
       | (cd targetdir; tar --extract --file=-)
```

or

```
$ tar --directory sourcedir --create --file=- . \
       | tar --directory targetdir --extract --file=-
```

---

Previous: [Copying directory hierarchies](#copy-directory-hierarchy), Up: [Recipes](#Recipes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### B.2 Restoring Intermediate Directories

A common concern is how to extract permissions and ownerships of
intermediate directories when extracting only selected members from
the archive. To illustrate this, consider the following archive:

```
# tar tvf A.tar
drwxr-xr-x root/root         0 2017-11-16 14:39 foo/
dr-xr-x--- gray/user         0 2017-11-16 14:39 foo/bar/
-rw-r--r-- gray/user        10 2017-11-16 14:40 foo/bar/file
```

Suppose you extract only the file `foo/bar/file`, while being
‘`root`’:

```
# tar xvf A.tar foo/bar/file
foo/bar/file
```

Now, let’s inspect the content of the created directories:

```
# find foo -ls
427257    0 drwxr-xr-x   3 root     root    16 Nov 17 16:10 foo
427258    0 drwxr-xr-x   2 root     root    17 Nov 17 16:10 foo/bar
427259    0 -rw-r--r--   1 gray     user    10 Nov  6 14:40 foo/bar/file
```

The requested file is restored, including its ownership and
permissions. The intermediate directories, however, are created with
the default permissions, current timestamp and owned by the current
user. This is because by the time `tar` has reached the requested file,
it had already skipped the entries for its parent directories, so it
has no iformation about their ownership and modes.

To restore meta information about the intermediate directories,
you’ll need to specify them explicitly in the command line and use the
`--no-recursive` option (see [Descending into Directories](#recurse)) to avoid extracting
their content.

To automate this process, Neal P. Murphy proposed the following
shell script[33](#FOOT33):

```
#! /bin/sh
(while read path
 do
   path=`dirname $path`
   while [ -n "$path" -a "$path" != "." ]
   do
     echo $path
     path=`dirname $path`
   done
 done < $2 | sort | uniq) |
 tar -x --no-recursion -v -f $1 -T - -T $2
```

The script takes two arguments: the name of the archive file, and the
name of the file list file.

To complete our example, the file list will contain single line:

```
foo/bar/file
```

Supposing its name is `file.list` and the script is named
`restore.sh`, you can invoke it as follows:

```
