# sh restore.sh A.tar file.list
```

---

Next: [Fixing Snapshot Files](#Fixing-Snapshot-Files), Previous: [Recipes](#Recipes), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## Appendix C Configuring Help Summary

Running `tar --help` displays the short `tar` option
summary (see [GNU `tar` documentation](#help)). This summary is organized by *groups* of
semantically close options. The options within each group are printed
in the following order: a short option, eventually followed by a list
of corresponding long option names, followed by a short description of
the option. For example, here is an excerpt from the actual `tar
--help` output:

```
 Main operation mode:

  -A, --catenate, --concatenate   append tar files to an archive
  -c, --create               create a new archive
  -d, --diff, --compare      find differences between archive and
                             file system
      --delete               delete from the archive
```

The exact visual representation of the help output is configurable via
`ARGP_HELP_FMT` environment variable. The value of this variable
is a comma-separated list of *format variable* assignments. There
are two kinds of format variables. An *offset variable* keeps the
offset of some part of help output text from the leftmost column on
the screen. A *boolean* variable is a flag that toggles some
output feature on or off. Depending on the type of the corresponding
variable, there are two kinds of assignments:

Offset assignment
:   The assignment to an offset variable has the following syntax:

    ```
    variable=value
    ```

    where variable is the variable name, and value is a
    numeric value to be assigned to the variable.

Boolean assignment
:   To assign `true` value to a variable, simply put this variable name. To
    assign `false` value, prefix the variable name with ‘`no-`’. For
    example:

    ```
    # Assign true value:
    dup-args
    # Assign false value:
    no-dup-args
    ```

Following variables are declared:

Help Output: *boolean* **dup-args** [¶](#index-dup_002dargs)
:   If true, arguments for an option are shown with both short and long
    options, even when a given option has both forms, for example:

    ```
      -f ARCHIVE, --file=ARCHIVE use archive file or device ARCHIVE
    ```

    If false, then if an option has both short and long forms, the
    argument is only shown with the long one, for example:

    ```
      -f, --file=ARCHIVE         use archive file or device ARCHIVE
    ```

    and a message indicating that the argument is applicable to both
    forms is printed below the options. This message can be disabled
    using `dup-args-note` (see below).

    The default is false.

Help Output: *boolean* **dup-args-note** [¶](#index-dup_002dargs_002dnote)
:   If this variable is true, which is the default, the following notice
    is displayed at the end of the help output:

    > Mandatory or optional arguments to long options are also mandatory or
    > optional for any corresponding short options.

    Setting `no-dup-args-note` inhibits this message. Normally, only one of
    variables `dup-args` or `dup-args-note` should be set.

Help Output: *offset* **short-opt-col** [¶](#index-short_002dopt_002dcol)
:   Column in which short options start. Default is 2.

    ```
    $ tar --help|grep ARCHIVE
      -f, --file=ARCHIVE   use archive file or device ARCHIVE
    $ ARGP_HELP_FMT=short-opt-col=6 tar --help|grep ARCHIVE
          -f, --file=ARCHIVE   use archive file or device ARCHIVE
    ```

Help Output: *offset* **long-opt-col** [¶](#index-long_002dopt_002dcol)
:   Column in which long options start. Default is 6. For example:

    ```
    $ tar --help|grep ARCHIVE
      -f, --file=ARCHIVE   use archive file or device ARCHIVE
    $ ARGP_HELP_FMT=long-opt-col=16 tar --help|grep ARCHIVE
      -f,           --file=ARCHIVE   use archive file or device ARCHIVE
    ```

Help Output: *offset* **doc-opt-col** [¶](#index-doc_002dopt_002dcol)
:   Column in which *doc options* start. A doc option isn’t actually
    an option, but rather an arbitrary piece of documentation that is
    displayed in much the same manner as the options. For example, in
    the description of `--format` option:

    ```
      -H, --format=FORMAT        create archive of the given format.

     FORMAT is one of the following:

        gnu                      GNU tar 1.13.x format
        oldgnu                   GNU format as per tar <= 1.12
        pax                      POSIX 1003.1-2001 (pax) format
        posix                    same as pax
        ustar                    POSIX 1003.1-1988 (ustar) format
        v7                       old V7 tar format
    ```

    the format names are doc options. Thus, if you set
    `ARGP_HELP_FMT=doc-opt-col=6` the above part of the help output
    will look as follows:

    ```
      -H, --format=FORMAT        create archive of the given format.

     FORMAT is one of the following:

            gnu                      GNU tar 1.13.x format
            oldgnu                   GNU format as per tar <= 1.12
            pax                      POSIX 1003.1-2001 (pax) format
            posix                    same as pax
            ustar                    POSIX 1003.1-1988 (ustar) format
            v7                       old V7 tar format
    ```

Help Output: *offset* **opt-doc-col** [¶](#index-opt_002ddoc_002dcol)
:   Column in which option description starts. Default is 29.

    ```
    $ tar --help|grep ARCHIVE
      -f, --file=ARCHIVE         use archive file or device ARCHIVE
    $ ARGP_HELP_FMT=opt-doc-col=19 tar --help|grep ARCHIVE
      -f, --file=ARCHIVE   use archive file or device ARCHIVE
    $ ARGP_HELP_FMT=opt-doc-col=9 tar --help|grep ARCHIVE
      -f, --file=ARCHIVE
               use archive file or device ARCHIVE
    ```

    Notice, that the description starts on a separate line if
    `opt-doc-col` value is too small.

Help Output: *offset* **header-col** [¶](#index-header_002dcol)
:   Column in which *group headers* are printed. A group header is a
    descriptive text preceding an option group. For example, in the
    following text:

    ```
     Main operation mode:

      -A, --catenate, --concatenate   append tar files to
                                 an archive
      -c, --create               create a new archive
    ```

    ‘`Main operation mode:`’ is the group header.

    The default value is 1.

Help Output: *offset* **usage-indent** [¶](#index-usage_002dindent)
:   Indentation of wrapped usage lines. Affects `--usage`
    output. Default is 12.

Help Output: *offset* **rmargin** [¶](#index-rmargin)
:   Right margin of the text output. Used for wrapping.

---

Next: [Tar Internals](#Tar-Internals), Previous: [Configuring Help Summary](#Configuring-Help-Summary), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## Appendix D Fixing Snapshot Files

Various situations can cause device numbers to change: upgrading your
kernel version, reconfiguring your hardware, loading kernel modules in a
different order, using virtual volumes that are assembled dynamically
(such as with LVM or RAID), hot-plugging drives
(e.g. external USB or Firewire drives), etc. In the majority of
cases this change is unnoticed by the users. However, it influences
`tar` incremental backups: the device number is stored in tar
snapshot files (see [Format of the Incremental Snapshot Files](#Snapshot-Files)) and is used to determine whether
the file has changed since the last backup. If the device numbers
change for some reason, by default the next backup you run will be a
full backup.

To minimize the impact in these cases, GNU `tar` comes with
the `tar-snapshot-edit` utility for inspecting and updating
device numbers in snapshot files. (The utility, written by
Dustin J. Mitchell, is also available from the
[GNU `tar` home page](http://www.gnu.org/software/tar/utils/tar-snapshot-edit.html).)

To obtain a summary of the device numbers found in the snapshot file, run

```
$ tar-snapshot-edit snapfile
```

where snapfile is the name of the snapshot file (you can supply as many
files as you wish in a single command line). You can then compare the
numbers across snapshot files, or against those currently in use on the
live filesystem (using `ls -l` or `stat`).

Assuming the device numbers have indeed changed, it’s often possible
to simply tell GNU `tar` to ignore the device number when processing the
incremental snapshot files for these backups, using the
`--no-check-device` option (see [device numbers](#device-numbers)).

Alternatively, you can use the `tar-edit-snapshot` script’s
`-r` option to update all occurrences of the given device
number in the snapshot file(s). It takes a single argument
of the form
‘`olddev-newdev`’, where olddev is the device number
used in the snapshot file, and newdev is the corresponding new device
number. Both numbers may be specified in hex (e.g., ‘`0xfe01`’),
decimal (e.g., ‘`65025`’), or as a major:minor number pair (e.g.,
‘`254:1`’). To change several device numbers at once, specify them
in a single comma-separated list, as in
`-r 0x3060-0x4500,0x307-0x4600`.

Before updating the snapshot file, it is a good idea to create a backup
copy of it. This is accomplished by ‘`-b`’ option. The name of the
backup file is obtained by appending ‘`~`’ to the original file name.

An example session:

```
$ tar-snapshot-edit root_snap.0 boot_snap.0
File: root_snap.0
  Detected snapshot file version: 2

  Device 0x0000 occurs 1 times.
  Device 0x0003 occurs 1 times.
  Device 0x0005 occurs 1 times.
  Device 0x0013 occurs 1 times.
  Device 0x6801 occurs 1 times.
  Device 0x6803 occurs 6626 times.
  Device 0xfb00 occurs 1 times.

File: boot_snap.0
  Detected snapshot file version: 2

  Device 0x6801 occurs 3 times.
$ tar-snapshot-edit -b -r 0x6801-0x6901,0x6803-0x6903 root_snap.0 boot_snap.0
File: root_snap.0
  Detected snapshot file version: 2

  Updated 6627 records.

File: boot_snap.0
  Detected snapshot file version: 2

  Updated 3 records.
```

---

Next: [Genfile](#Genfile), Previous: [Fixing Snapshot Files](#Fixing-Snapshot-Files), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## Appendix E Tar Internals

* [Basic Tar Format](#Standard)
* [GNU Extensions to the Archive Format](#Extensions)
* [Storing Sparse Files](#Sparse-Formats)
* [Format of the Incremental Snapshot Files](#Snapshot-Files)
* [Dumpdir](#Dumpdir)

---

Next: [GNU Extensions to the Archive Format](#Extensions), Up: [Tar Internals](#Tar-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### Basic Tar Format

> *(This message will disappear, once this node revised.)*

While an archive may contain many files, the archive itself is a
single ordinary file. Like any other file, an archive file can be
written to a storage device such as a tape or disk, sent through a
pipe or over a network, saved on the active file system, or even
stored in another archive. An archive file is not easy to read or
manipulate without using the `tar` utility or Tar mode in
GNU Emacs.

Physically, an archive consists of a series of file entries terminated
by an end-of-archive entry, which consists of two 512 blocks of zero
bytes. A file
entry usually describes one of the files in the archive (an
*archive member*), and consists of a file header and the contents
of the file. File headers contain file names and statistics, checksum
information which `tar` uses to detect file corruption, and
information about file types.

Archives are permitted to have more than one member with the same
member name. One way this situation can occur is if more than one
version of a file has been stored in the archive. For information
about adding new versions of a file to an archive, see [Updating an Archive](#update).

In addition to entries describing archive members, an archive may
contain entries which `tar` itself uses to store information.
See [Including a Label in the Archive](#label), for an example of such an archive entry.

A `tar` archive file contains a series of blocks. Each block
contains `BLOCKSIZE` bytes. Although this format may be thought
of as being on magnetic tape, other media are often used.

Each file archived is represented by a header block which describes
the file, followed by zero or more blocks which give the contents
of the file. At the end of the archive file there are two 512-byte blocks
filled with binary zeros as an end-of-file marker. A reasonable system
should write such end-of-file marker at the end of an archive, but
must not assume that such a block exists when reading an archive. In
particular, GNU `tar` does not treat missing end-of-file marker as an
error and silently ignores the fact. You can instruct it to issue
a warning, however, by using the `--warning=missing-zero-blocks`
option (see [missing-zero-blocks](#General-Warnings)).

The blocks may be *blocked* for physical I/O operations.
Each record of n blocks (where n is set by the
`--blocking-factor=512-size` (`-b 512-size`) option to `tar`) is written with a single
‘`write ()`’ operation. On magnetic tapes, the result of
such a write is a single record. When writing an archive,
the last record of blocks should be written at the full size, with
blocks after the zero block containing all zeros. When reading
an archive, a reasonable system should properly handle an archive
whose last record is shorter than the rest, or which contains garbage
records after a zero block.

The header block is defined in C as follows. In the GNU `tar`
distribution, this is part of file `src/tar.h`:

```
/* tar Header Block, from POSIX 1003.1-2024
   <https://pubs.opengroup.org/onlinepubs/9799919799/utilities/pax.html#tagtcjh_21>  */

/* POSIX header.  */

struct posix_header
{                              /* byte offset */
  char name[100];               /*   0 */
  char mode[8];                 /* 100 */
  char uid[8];                  /* 108 */
  char gid[8];                  /* 116 */
  char size[12];                /* 124 */
  char mtime[12];               /* 136 */
  char chksum[8];               /* 148 */
  char typeflag;                /* 156 */
  char linkname[100];           /* 157 */
  char magic[6];                /* 257 */
  char version[2];              /* 263 */
  char uname[32];               /* 265 */
  char gname[32];               /* 297 */
  char devmajor[8];             /* 329 */
  char devminor[8];             /* 337 */
  char prefix[155];             /* 345 */
                                /* 500 */
};

#define TMAGIC   "ustar"        /* ustar and a null */
#define TMAGLEN  6
#define TVERSION "00"           /* 00 and no null */
#define TVERSLEN 2

/* Values used in typeflag field.  */
enum
  {

    REGTYPE     = '0',          /* regular file */
    AREGTYPE    = '\0',         /* regular file */
    LNKTYPE     = '1',          /* link */
    SYMTYPE     = '2',          /* symbolic link */
    CHRTYPE     = '3',          /* character special */
    BLKTYPE     = '4',          /* block special */
    DIRTYPE     = '5',          /* directory */
    FIFOTYPE    = '6',          /* FIFO special */
    CONTTYPE    = '7',          /* contiguous; treated as a regular file */

    XHDTYPE     = 'x',          /* Extended header referring to the
                                   next file in the archive */
    XGLTYPE     = 'g'           /* Global extended header */
  };

/* Bits used in the mode field, values in octal.  */
enum
  {
    TSUID       = 04000,        /* set UID on execution */
    TSGID       = 02000,        /* set GID on execution */
    TSVTX       = 01000,        /* reserved */
                                /* file permissions */
    TUREAD      = 00400,        /* read by owner */
    TUWRITE     = 00200,        /* write by owner */
    TUEXEC      = 00100,        /* execute/search by owner */
    TGREAD      = 00040,        /* read by group */
    TGWRITE     = 00020,        /* write by group */
    TGEXEC      = 00010,        /* execute/search by group */
    TOREAD      = 00004,        /* read by other */
    TOWRITE     = 00002,        /* write by other */
    TOEXEC      = 00001         /* execute/search by other */
  };

/* tar Header Block, GNU extensions.  */

/* *BEWARE* *BEWARE* *BEWARE* that the following information is still
   boiling, and may change.  Even if the OLDGNU format description should be
   accurate, the so-called GNU format is not yet fully decided.  It is
   surely meant to use only extensions allowed by POSIX, but the sketch
   below repeats some ugliness from the OLDGNU format, which should rather
   go away.  Sparse files should be saved in such a way that they do *not*
   require two passes at archive creation time.  Huge files get some POSIX
   fields to overflow, alternate solutions have to be sought for this.  */

/* Descriptor for a single file hole.  */

struct sparse
{                              /* byte offset */
  char offset[12];              /*   0 */
  char numbytes[12];            /*  12 */
                                /*  24 */
};

/* Sparse files are not supported in POSIX ustar format.  For sparse files
   with a POSIX header, a GNU extra header is provided which holds overall
   sparse information and a few sparse descriptors.  When an old GNU header
   replaces both the POSIX header and the GNU extra header, it holds some
   sparse descriptors too.  Whether POSIX or not, if more sparse descriptors
   are still needed, they are put into as many successive sparse headers as
   necessary.  The following constants tell how many sparse descriptors fit
   in each kind of header able to hold them.  */

enum
  {
    SPARSES_IN_EXTRA_HEADER     = 16,
    SPARSES_IN_OLDGNU_HEADER    =  4,
    SPARSES_IN_SPARSE_HEADER    = 21
  };

/* Extension header for sparse files, used immediately after the GNU extra
   header, and used only if all sparse information cannot fit into that
   extra header.  There might even be many such extension headers, one after
   the other, until all sparse information has been recorded.  */

struct sparse_header
{                              /* byte offset */
  struct sparse sp[SPARSES_IN_SPARSE_HEADER];
                                /*   0 */
  char isextended;              /* 504 */
                                /* 505 */
};

/* The old GNU format header conflicts with POSIX format in such a way that
   POSIX archives may fool old GNU tar’s, and POSIX tar’s might well be
   fooled by old GNU tar archives.  An old GNU format header uses the space
   used by the prefix field in a POSIX header, and cumulates information
   normally found in a GNU extra header.  With an old GNU tar header, we
   never see any POSIX header nor GNU extra header.  Supplementary sparse
   headers are allowed, however.  */

struct oldgnu_header
{                              /* byte offset */
  char unused_pad1[345];        /*   0 */
  char atime[12];               /* 345 Incr. archive: atime of the file */
  char ctime[12];               /* 357 Incr. archive: ctime of the file */
  char offset[12];              /* 369 Multivolume archive: the offset of
                                   the start of this volume */
  char longnames[4];            /* 381 Not used */
  char unused_pad2;             /* 385 */
  struct sparse sp[SPARSES_IN_OLDGNU_HEADER];
                                /* 386 */
  char isextended;              /* 482 Sparse file: Extension sparse header
                                   follows */
  char realsize[12];            /* 483 Sparse file: Real size*/
                                /* 495 */
};

/* OLDGNU_MAGIC uses both magic and version fields, which are contiguous.
   Found in an archive, it indicates an old GNU header format, which will be
   hopefully become obsolescent.  With OLDGNU_MAGIC, uname and gname are
   valid, though the header is not truly POSIX conforming.  */
#define OLDGNU_MAGIC "ustar  "  /* 7 chars and a null */

/* The standards committee allows only capital A through capital Z for
   user-defined expansion.  Letters in use in other implementations include:

   ’A’ Solaris Access Control List
   ’E’ Solaris Extended Attribute File
   ’I’ Inode only, as in ’star’
   ’N’ Obsolete GNU tar, for file names too long for main header.  */

enum
  {
    /* This is a dir entry that contains the names of files that were in the
       dir at the time the dump was made.  */
    GNUTYPE_DUMPDIR = 'D',

    /* Identifies the *next* file on the tape as having a long linkname.  */
    GNUTYPE_LONGLINK = 'K',

    /* Identifies the *next* file on the tape as having a long name.  */
    GNUTYPE_LONGNAME = 'L',

    /* This is the continuation of a file that began on another volume.  */
    GNUTYPE_MULTIVOL = 'M',

    /* This is for sparse files.  */
    GNUTYPE_SPARSE = 'S',

    /* This file is a tape/volume header.  Ignore it on extraction.  */
    GNUTYPE_VOLHDR = 'V',

    /* Solaris extended header.  */
    SOLARIS_XHDTYPE = 'X'
  };

/* Jörg Schilling star header.  */

struct star_header
{                              /* byte offset */
  char name[100];               /*   0 */
  char mode[8];                 /* 100 */
  char uid[8];                  /* 108 */
  char gid[8];                  /* 116 */
  char size[12];                /* 124 */
  char mtime[12];               /* 136 */
  char chksum[8];               /* 148 */
  char typeflag;                /* 156 */
  char linkname[100];           /* 157 */
  char magic[6];                /* 257 */
  char version[2];              /* 263 */
  char uname[32];               /* 265 */
  char gname[32];               /* 297 */
  char devmajor[8];             /* 329 */
  char devminor[8];             /* 337 */
  char prefix[131];             /* 345 */
  char atime[12];               /* 476 */
  char ctime[12];               /* 488 */
                                /* 500 */
};

enum
  {
    SPARSES_IN_STAR_HEADER = 4,
    SPARSES_IN_STAR_EXT_HEADER = 21
  };

struct star_in_header
{
  char fill[345];       /*   0  Everything that is before t_prefix */
  char prefix[1];       /* 345  t_name prefix */
  char fill2;           /* 346  */
  char fill3[8];        /* 347  */
  char isextended;      /* 355  */
  struct sparse sp[SPARSES_IN_STAR_HEADER]; /* 356  */
  char realsize[12];    /* 452  Actual size of the file */
  char offset[12];      /* 464  Offset of multivolume contents */
  char atime[12];       /* 476  */
  char ctime[12];       /* 488  */
  char mfill[8];        /* 500  */
  char xmagic[4];       /* 508  "tar" */
};

struct star_ext_header
{
  struct sparse sp[SPARSES_IN_STAR_EXT_HEADER];
  char isextended;
};
```

All characters in header blocks are represented by using 8-bit
characters in the local variant of ASCII. Each field within the
structure is contiguous; that is, there is no padding used within
the structure. Each character on the archive medium is stored
contiguously.

Bytes representing the contents of files (after the header block
of each file) are not translated in any way and are not constrained
to represent characters in any character set. The `tar` format
does not distinguish text files from binary files, and no translation
of file contents is performed.

The `name`, `linkname`, `magic`, `uname`, and
`gname` are null-terminated character strings. All other fields
are zero-filled octal numbers in ASCII. Each numeric field of width
w contains w minus 1 digits, and a null.
(In the extended GNU format, the numeric fields can take
other forms.)

The `name` field is the file name of the file, with directory names
(if any) preceding the file name, separated by slashes.

The `mode` field provides nine bits specifying file permissions
and three bits to specify the Set UID, Set GID, and Save Text
(*sticky*) modes. Values for these bits are defined above.
When special permissions are required to create a file with a given
mode, and the user restoring files from the archive does not hold such
permissions, the mode bit(s) specifying those special permissions
are ignored. Modes which are not supported by the operating system
restoring files from the archive will be ignored. Unsupported modes
should be faked up when creating or updating an archive; e.g., the
group permission could be copied from the *other* permission.

The `uid` and `gid` fields are the numeric user and group
ID of the file owners, respectively. If the operating system does
not support numeric user or group IDs, these fields should
be ignored.

The `size` field is the size of the file in bytes; for archive
members that are symbolic or hard links to another file, this field
is specified as zero.

The `mtime` field represents the data modification time of the file at
the time it was archived. It represents the integer number of
seconds since January 1, 1970, 00:00 Coordinated Universal Time.

The `chksum` field represents
the simple sum of all bytes in the header block. Each 8-bit
byte in the header is added to an unsigned integer, initialized to
zero, the precision of which shall be no less than seventeen bits.
When calculating the checksum, the `chksum` field is treated as
if it were filled with spaces (ASCII 32).

The `typeflag` field specifies the type of file archived. If a
particular implementation does not recognize or permit the specified
type, the file will be extracted as if it were a regular file. As this
action occurs, `tar` issues a warning to the standard error.

The `atime` and `ctime` fields are used in making incremental
backups; they store, respectively, the particular file’s access and
status change times.

The `offset` is used by the `--multi-volume` (`-M`) option, when
making a multi-volume archive. The offset is number of bytes into
the file that we need to restart at to continue the file on the next
tape, i.e., where we store the location that a continued file is
continued at.

The following fields were added to deal with sparse files. A file
is *sparse* if it takes in unallocated blocks which end up being
represented as zeros, i.e., no useful data. A test to see if a file
is sparse is to look at the number blocks allocated for it versus the
number of characters in the file; if there are fewer blocks allocated
for the file than would normally be allocated for a file of that
size, then the file is sparse. This is the method `tar` uses to
detect a sparse file, and once such a file is detected, it is treated
differently from non-sparse files.

Sparse files are often `dbm` files, or other database-type files
which have data at some points and emptiness in the greater part of
the file. Such files can appear to be very large when an ‘`ls
-l`’ is done on them, when in truth, there may be a very small amount
of important data contained in the file. It is thus undesirable
to have `tar` think that it must back up this entire file, as
great quantities of room are wasted on empty blocks, which can lead
to running out of room on a tape far earlier than is necessary.
Thus, sparse files are dealt with so that these empty blocks are
not written to the tape. Instead, what is written to the tape is a
description, of sorts, of the sparse file: where the holes are, how
big the holes are, and how much data is found at the end of the hole.
This way, the file takes up potentially far less room on the tape,
and when the file is extracted later on, it will look exactly the way
it looked beforehand. The following is a description of the fields
used to handle a sparse file:

The `sp` is an array of `struct sparse`. Each `struct
sparse` contains two 12-character strings which represent an offset
into the file and a number of bytes to be written at that offset.
The offset is absolute, and not relative to the offset in preceding
array element.

The header can hold four of these `struct sparse` at the moment;
if more are needed, they are not stored in the header.

The `isextended` flag is set when an `extended_header`
is needed to deal with a file. Note that this means that this flag
can only be set when dealing with a sparse file, and it is only set
in the event that the description of the file will not fit in the
allotted room for sparse structures in the header. In other words,
an extended\_header is needed.

The `extended_header` structure is used for sparse files which
need more sparse structures than can fit in the header. The header can
fit 4 such structures; if more are needed, the flag `isextended`
gets set and the next block is an `extended_header`.

Each `extended_header` structure contains an array of 21
sparse structures, along with a similar `isextended` flag
that the header had. There can be an indeterminate number of such
`extended_header`s to describe a sparse file.

`REGTYPE`

`AREGTYPE`
:   These flags represent a regular file. In order to be compatible
    with older versions of `tar`, a `typeflag` value of
    `AREGTYPE` should be silently recognized as a regular file.
    New archives should be created using `REGTYPE`. Also, for
    backward compatibility, `tar` treats a regular file whose name
    ends with a slash as a directory.

`LNKTYPE`
:   This flag represents a file linked to another file, of any type,
    previously archived. Such files are identified in Unix by each
    file having the same device and inode number. The linked-to name is
    specified in the `linkname` field with a trailing null.

`SYMTYPE`
:   This represents a symbolic link to another file. The linked-to name
    is specified in the `linkname` field with a trailing null.

`CHRTYPE`

`BLKTYPE`
:   These represent character special files and block special files
    respectively. In this case the `devmajor` and `devminor`
    fields will contain the major and minor device numbers respectively.
    Operating systems may map the device specifications to their own
    local specification, or may ignore the entry.

`DIRTYPE`
:   This flag specifies a directory or sub-directory. The directory
    name in the `name` field should end with a slash. On systems where
    disk allocation is performed on a directory basis, the `size` field
    will contain the maximum number of bytes (which may be rounded to
    the nearest disk block allocation unit) which the directory may
    hold. A `size` field of zero indicates no such limiting. Systems
    which do not support limiting in this manner should ignore the
    `size` field.

`FIFOTYPE`
:   This specifies a FIFO special file. Note that the archiving of a
    FIFO file archives the existence of this file and not its contents.

`CONTTYPE`
:   This specifies a contiguous file, which is the same as a normal
    file except that, in operating systems which support it, all its
    space is allocated contiguously on the disk. Operating systems
    which do not allow contiguous allocation should silently treat this
    type as a normal file.

`A` … `Z`
:   These are reserved for custom implementations. Some of these are
    used in the GNU modified format, as described below.

Other values are reserved for specification in future revisions of
the P1003 standard, and should not be used by any `tar` program.

The `magic` field indicates that this archive was output in
the P1003 archive format. If this field contains `TMAGIC`,
the `uname` and `gname` fields will contain the ASCII
representation of the owner and group of the file respectively.
If found, the user and group IDs are used rather than the values in
the `uid` and `gid` fields.

For references, see ISO/IEC 9945-1:1990 or IEEE Std 1003.1-1990, pages
169-173 (section 10.1) for Archive/Interchange File Format; and
IEEE Std 1003.2-1992, pages 380-388 (section 4.48) and pages 936-940
(section E.4.48) for pax - Portable archive interchange.

---

Next: [Storing Sparse Files](#Sparse-Formats), Previous: [Basic Tar Format](#Standard), Up: [Tar Internals](#Tar-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### GNU Extensions to the Archive Format

> *(This message will disappear, once this node revised.)*

The GNU format uses additional file types to describe new types of
files in an archive. These are listed below.

`GNUTYPE_DUMPDIR`

`'D'`
:   This represents a directory and a list of files created by the
    `--incremental` (`-G`) option. The `size` field gives the total
    size of the associated list of files. Each file name is preceded by
    either a ‘`Y`’ (the file should be in this archive) or an ‘`N`’.
    (The file is a directory, or is not stored in the archive.) Each file
    name is terminated by a null. There is an additional null after the
    last file name.

`GNUTYPE_MULTIVOL`

`'M'`
:   This represents a file continued from another volume of a multi-volume
    archive created with the `--multi-volume` (`-M`) option. The original
    type of the file is not given here. The `size` field gives the
    maximum size of this piece of the file (assuming the volume does
    not end before the file is written out). The `offset` field
    gives the offset from the beginning of the file where this part of
    the file begins. Thus `size` plus `offset` should equal
    the original size of the file.

`GNUTYPE_SPARSE`

`'S'`
:   This flag indicates that we are dealing with a sparse file. Note
    that archiving a sparse file requires special operations to find
    holes in the file, which mark the positions of these holes, along
    with the number of bytes of data to be found after the hole.

`GNUTYPE_VOLHDR`

`'V'`
:   This file type is used to mark the volume header that was given with
    the `--label=archive-label` (`-V archive-label`) option when the archive was created. The `name`
    field contains the `name` given after the `--label=archive-label` (`-V archive-label`) option.
    The `size` field is zero. Only the first file in each volume
    of an archive should have this type.

For fields containing numbers or timestamps that are out of range for
the basic format, the GNU format uses a base-256
representation instead of an ASCII octal number. If the leading byte
is 0xff (255), all the bytes of the field (including the leading byte)
are concatenated in big-endian order, with the result being a negative
number expressed in two’s complement form. If the leading byte is
0x80 (128), the non-leading bytes of the field are concatenated in
big-endian order, with the result being a positive number expressed in
binary form. Leading bytes other than 0xff, 0x80 and ASCII octal
digits are reserved for future use, as are base-256 representations of
values that would be in range for the basic format.

You may have trouble reading a GNU format archive on a
non-GNU system if the options `--incremental` (`-G`),
`--multi-volume` (`-M`), `--sparse` (`-S`), or `--label=archive-label` (`-V archive-label`) were
used when writing the archive. In general, if `tar` does not
use the GNU-added fields of the header, other versions of
`tar` should be able to read the archive. Otherwise, the
`tar` program will give an error, the most likely one being a
checksum error.

---

Next: [Format of the Incremental Snapshot Files](#Snapshot-Files), Previous: [GNU Extensions to the Archive Format](#Extensions), Up: [Tar Internals](#Tar-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### Storing Sparse Files

The notion of sparse file, and the ways of handling it from the point
of view of GNU `tar` user have been described in detail in
[Archiving Sparse Files](#sparse). This chapter describes the internal format GNU `tar`
uses to store such files.

The support for sparse files in GNU `tar` has a long history. The
earliest version featuring this support that I was able to find was 1.09,
released in November, 1990. The format introduced back then is called
*old GNU* sparse format and in spite of the fact that its design
contained many flaws, it was the only format GNU `tar` supported
until version 1.14 (May, 2004), which introduced initial support for
sparse archives in PAX archives (see [GNU `tar` and POSIX `tar`](#posix)). This
format was not free from design flaws, either and it was subsequently
improved in versions 1.15.2 (November, 2005) and 1.15.92 (June,
2006).

In addition to GNU sparse format, GNU `tar` is able to read and
extract sparse files archived by `star`.

The following subsections describe each format in detail.

* [Old GNU Format](#Old-GNU-Format)
* [PAX Format, Versions 0.0 and 0.1](#PAX-0)
* [PAX Format, Version 1.0](#PAX-1)

---

Next: [PAX Format, Versions 0.0 and 0.1](#PAX-0), Up: [Storing Sparse Files](#Sparse-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Old GNU Format

The format introduced in November 1990 (v. 1.09) was
designed on top of standard `ustar` headers in such an
unfortunate way that some of its fields overwrote fields required by
POSIX.

An old GNU sparse header is designated by type ‘`S`’
(`GNUTYPE_SPARSE`) and has the following layout:

| Offset | Size | Name | Data type | Contents |
| --- | --- | --- | --- | --- |
| 0 | 345 |  | N/A | Not used. |
| 345 | 12 | atime | Number | `atime` of the file. |
| 357 | 12 | ctime | Number | `ctime` of the file . |
| 369 | 12 | offset | Number | For multivolume archives: the offset of the start of this volume. |
| 381 | 4 |  | N/A | Not used. |
| 385 | 1 |  | N/A | Not used. |
| 386 | 96 | sp | `sparse_header` | (4 entries) File map. |
| 482 | 1 | isextended | Bool | `1` if an extension sparse header follows, `0` otherwise. |
| 483 | 12 | realsize | Number | Real size of the file. |

Each of `sparse_header` object at offset 386 describes a single
data chunk. It has the following structure:

| Offset | Size | Data type | Contents |
| --- | --- | --- | --- |
| 0 | 12 | Number | Offset of the beginning of the chunk. |
| 12 | 12 | Number | Size of the chunk. |

If the member contains more than four chunks, the `isextended`
field of the header has the value `1` and the main header is
followed by one or more *extension headers*. Each such header has
the following structure:

| Offset | Size | Name | Data type | Contents |
| --- | --- | --- | --- | --- |
| 0 | 21 | sp | `sparse_header` | (21 entries) File map. |
| 504 | 1 | isextended | Bool | `1` if an extension sparse header follows, or `0` otherwise. |

A header with `isextended=0` ends the map.

---

Next: [PAX Format, Version 1.0](#PAX-1), Previous: [Old GNU Format](#Old-GNU-Format), Up: [Storing Sparse Files](#Sparse-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### PAX Format, Versions 0.0 and 0.1

There are two formats available in this branch. The version `0.0`
is the initial version of sparse format used by `tar`
versions 1.14–1.15.1. The sparse file map is kept in extended
(`x`) PAX header variables:

`GNU.sparse.size`
:   Real size of the stored file;

`GNU.sparse.numblocks` [¶](#index-GNU_002esparse_002enumblocks_002c-extended-header-variable)
:   Number of blocks in the sparse map;

`GNU.sparse.offset` [¶](#index-GNU_002esparse_002eoffset_002c-extended-header-variable)
:   Offset of the data block;

`GNU.sparse.numbytes` [¶](#index-GNU_002esparse_002enumbytes_002c-extended-header-variable)
:   Size of the data block.

The latter two variables repeat for each data block, so the overall
structure is like this:

```
GNU.sparse.size=size
GNU.sparse.numblocks=numblocks
repeat numblocks times
  GNU.sparse.offset=offset
  GNU.sparse.numbytes=numbytes
end repeat
```

This format presented the following two problems:

1. Whereas the POSIX specification allows a variable to appear multiple
   times in a header, it requires that only the last occurrence be
   meaningful. Thus, multiple occurrences of `GNU.sparse.offset` and
   `GNU.sparse.numbytes` are conflicting with the POSIX specs.
2. Attempting to extract such archives using a third-party’s `tar`
   results in extraction of sparse files in *condensed form*. If
   the `tar` implementation in question does not support POSIX
   format, it will also extract a file containing extension header
   attributes. This file can be used to expand the file to its original
   state. However, posix-aware `tar`s will usually ignore the
   unknown variables, which makes restoring the file more
   difficult. See [Extraction of sparse
   members in v.0.0 format](#extracting-sparse-v0x), for the detailed description of how to
   restore such members using non-GNU `tar`s.

GNU `tar` 1.15.2 introduced sparse format version `0.1`, which
attempted to solve these problems. As its predecessor, this format
stores sparse map in the extended POSIX header. It retains
`GNU.sparse.size` and `GNU.sparse.numblocks` variables, but
instead of `GNU.sparse.offset`/`GNU.sparse.numbytes` pairs
it uses a single variable:

`GNU.sparse.map` [¶](#index-GNU_002esparse_002emap_002c-extended-header-variable)
:   Map of non-null data chunks. It is a string consisting of
    comma-separated values "offset,size[,offset-1,size-1...]"

To address the 2nd problem, the `name` field in `ustar`
is replaced with a special name, constructed using the following pattern:

```
%d/GNUSparseFile.%p/%f
```

The real name of the sparse file is stored in the variable
`GNU.sparse.name`. Thus, those `tar` implementations
that are not aware of GNU extensions will at least extract the files
into separate directories, giving the user a possibility to expand it
afterwards. See [Extraction of sparse
members in v.0.1 format](#extracting-sparse-v0x), for the detailed description of how to
restore such members using non-GNU `tar`s.

The resulting `GNU.sparse.map` string can be *very* long.
Although POSIX does not impose any limit on the length of a `x`
header variable, this possibly can confuse some `tar`s.

---

Previous: [PAX Format, Versions 0.0 and 0.1](#PAX-0), Up: [Storing Sparse Files](#Sparse-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### PAX Format, Version 1.0

The version `1.0` of sparse format was introduced with GNU `tar`
1.15.92. Its main objective was to make the resulting file
extractable with little effort even by non-posix aware `tar`
implementations. Starting from this version, the extended header
preceding a sparse member always contains the following variables that
identify the format being used:

`GNU.sparse.major` [¶](#index-GNU_002esparse_002emajor_002c-extended-header-variable)
:   Major version

`GNU.sparse.minor` [¶](#index-GNU_002esparse_002eminor_002c-extended-header-variable)
:   Minor version

The `name` field in `ustar` header contains a special name,
constructed using the following pattern:

```
%d/GNUSparseFile.%p/%f
```

The real name of the sparse file is stored in the variable
`GNU.sparse.name`. The real size of the file is stored in the
variable `GNU.sparse.realsize`.

The sparse map itself is stored in the file data block, preceding the actual
file data. It consists of a series of decimal numbers delimited
by newlines. The map is padded with nulls to the nearest block boundary.

The first number gives the number of entries in the map. Following are
map entries, each one consisting of two numbers giving the offset and
size of the data block it describes.

The format is designed in such a way that non-posix aware `tar`s and `tar`s not
supporting `GNU.sparse.*` keywords will extract each sparse file
in its condensed form with the file map prepended and will place it
into a separate directory. Then, using a simple program it would be
possible to expand the file to its original form even without GNU `tar`.
See [Extracting Sparse Members](#Sparse-Recovery), for the detailed information on how to extract
sparse members without GNU `tar`.

---

Next: [Dumpdir](#Dumpdir), Previous: [Storing Sparse Files](#Sparse-Formats), Up: [Tar Internals](#Tar-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### Format of the Incremental Snapshot Files

A *snapshot file* (or *directory file*) is created during
incremental backups (see [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps)). It
contains the status of the file system at the time of the dump and is
used to determine which files were modified since the last backup.

GNU `tar` version 1.35.90 supports three snapshot file
formats. The first format, called *format 0*, is the one used by
GNU `tar` versions up to and including 1.15.1. The second format, called
*format 1* is an extended version of this format, that contains more
metadata and allows for further extensions. It was used by alpha release
version 1.15.90. For alpha version 1.15.91 and stable releases
version 1.16 up through 1.35.90, the *format 2* is used.

GNU `tar` is able to read all three formats, but will create
snapshots only in format 2.

This appendix describes all three formats in detail.

0. ‘`Format 0`’ snapshot file begins with a line containing a
   decimal number that represents a UNIX timestamp of the
   beginning of the last archivation. This line is followed by directory
   metadata descriptions, one per line. Each description has the
   following format:

   ```
   [nfs]dev inode name
   ```

   where:

   nfs
   :   A single plus character (‘`+`’), if this directory is located on
       an NFS-mounted partition, otherwise empty.

       (That is, for non-NFS directories, the first character on the
       description line contains the start of the dev field.)

   dev
   :   Device number of the directory;

   inode
   :   I-node number of the directory;

   name
   :   Name of the directory. Any special characters (white-space,
       backslashes, etc.) are quoted.
1. ‘`Format 1`’ snapshot file begins with a line specifying the
   format of the file. This line has the following structure:

   ```
   ‘GNU tar-’tar-version‘-’incr-format-version
   ```

   where tar-version is the version number of GNU `tar`
   implementation that created this snapshot, and
   incr-format-version is the version number of the snapshot format
   (in this case ‘`1`’).

   Next line contains two decimal numbers, representing the
   time of the last backup. First number is the number of seconds, the
   second one is the number of nanoseconds, since the beginning of the
   epoch.

   Lines that follow contain directory metadata, one line per
   directory. Each line is formatted as follows:

   ```
   [nfs]mtime-sec mtime-nsec dev inode name
   ```

   where mtime-sec and mtime-nsec represent last
   modification time of this directory with nanosecond precision;
   nfs, dev, inode and name have the same meaning
   as with ‘`format 0`’.
2. ‘`Format 2`’ snapshot file begins with a format identifier, as described for
   version 1, e.g.:

   ```
   GNU tar-1.35.90-2
   ```

   This line is followed by newline. Rest of file consists of
   records, separated by null (ASCII 0)
   characters. Thus, in contrast to the previous formats, format 2
   snapshot is a binary file.

   First two records are decimal integers, representing the
   time of the last backup. First number is the number of seconds, the
   second one is the number of nanoseconds, since the beginning of the
   epoch. These are followed by arbitrary number of directory records.

   Each *directory record* contains a set of metadata describing a
   particular directory. Parts of a directory record are delimited with
   ASCII 0 characters. The following table describes each
   part. The *Number* type in this table stands for a decimal integer
   in ASCII notation. (Negative values are preceded with a "-"
   character, while positive values have no leading punctuation.)

   | Field | Type | Description |
   | --- | --- | --- |
   | nfs | Character | ‘`1`’ if the directory is located on an NFS-mounted partition, or ‘`0`’ otherwise; |
   | timestamp\_sec | Number | Modification time, seconds; |
   | timestamp\_nsec | Number | Modification time, nanoseconds; |
   | dev | Number | Device number; |
   | ino | Number | I-node number; |
   | name | String | Directory name; in contrast to the previous versions it is not quoted; |
   | contents | Dumpdir | Contents of the directory; See [Dumpdir](#Dumpdir), for a description of its format. |
   |  |

   Dumpdirs stored in snapshot files contain only records of types
   ‘`Y`’, ‘`N`’ and ‘`D`’.

   The specific range of values allowed in each of the *Number* fields
   depends on the underlying C datatypes as determined when `tar`
   is compiled. To see the specific ranges allowed for a particular
   `tar` binary, you can use the
   `--show-snapshot-field-ranges` option:

   ```
   $ tar --show-snapshot-field-ranges
   This tar's snapshot file field ranges are
      (field name      => [ min, max ]):

       nfs             => [ 0, 1 ],
       timestamp_sec   => [ -9223372036854775808, 9223372036854775807 ],
       timestamp_nsec  => [ 0, 999999999 ],
       dev             => [ 0, 18446744073709551615 ],
       ino             => [ 0, 18446744073709551615 ],
   ```

   (This example is from a GNU/Linux x86\_64 system.)

---

Previous: [Format of the Incremental Snapshot Files](#Snapshot-Files), Up: [Tar Internals](#Tar-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### Dumpdir

Incremental archives keep information about contents of each
dumped directory in special data blocks called *dumpdirs*.

Dumpdir is a sequence of entries of the following form:

```
C filename \0
```

where C is one of the *control codes* described below,
filename is the name of the file C operates upon, and
‘`\0`’ represents a nul character (ASCII 0). The white space
characters were added for readability, real dumpdirs do not contain
them.

Each dumpdir ends with a single nul character.

The following table describes control codes and their meanings:

‘`Y`’
:   filename is contained in the archive.

‘`N`’
:   filename was present in the directory at the time the archive
    was made, yet it was not dumped to the archive, because it had not
    changed since the last backup.

‘`D`’
:   filename is a directory.

‘`R`’
:   This code requests renaming of the filename to the name
    specified with the ‘`T`’ command, that immediately follows it.

‘`T`’
:   Specify target file name for ‘`R`’ command (see below).

‘`X`’
:   Specify *temporary directory* name for a rename operation (see below).

Codes ‘`Y`’, ‘`N`’ and ‘`D`’ require filename argument
to be a relative file name to the directory this dumpdir describes,
whereas codes ‘`R`’, ‘`T`’ and ‘`X`’ require their argument
to be an absolute file name.

The three codes ‘`R`’, ‘`T`’ and ‘`X`’ specify a
*renaming operation*. In the simplest case it is:

```
Rsource\0Tdest\0
```

which means “rename file `source` to file `dest`”.

However, there are cases that require using a *temporary
directory*. For example, consider the following scenario:

1. Previous run dumped a directory `foo` which contained the
   following three directories:

   ```
   a
   b
   c
   ```
2. They were renamed *cyclically*, so that:

   ```
   a became b
   b became c
   c became a
   ```
3. New incremental dump was made.

This case cannot be handled by three successive renames, since
renaming `a` to `b` will destroy the existing directory.
To correctly process it, GNU `tar` needs a temporary directory, so
it creates the following dumpdir (newlines have been added for
readability):

```
Xfoo\0
Rfoo/a\0T\0
Rfoo/b\0Tfoo/c\0
Rfoo/c\0Tfoo/a\0
R\0Tfoo/a\0
```

The first command, ‘`Xfoo\0`’, instructs the extractor to create a
temporary directory in the directory `foo`. Second command,
‘`Rfoo/aT\0`’, says “rename file `foo/a` to the temporary
directory that has just been created” (empty file name after a
command means use temporary directory). Third and fourth commands
work as usual, and, finally, the last command, ‘`R\0Tfoo/a\0`’
tells tar to rename the temporary directory to `foo/a`.

The exact placement of a dumpdir in the archive depends on the
archive format (see [Controlling the Archive Format](#Formats)):

* PAX archives

  In PAX archives, dumpdir is stored in the extended header of the
  corresponding directory, in variable `GNU.dumpdir`.
* GNU and old GNU archives

  These formats implement special header type ‘`D`’, which is similar
  to ustar header ‘`5`’ (directory), except that it precedes a data
  block containing the dumpdir.

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [Tar Internals](#Tar-Internals), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## Appendix F Genfile

This appendix describes `genfile`, an auxiliary program
used in the GNU tar testsuite. If you are not interested in developing
GNU tar, skip this appendix.

Initially, `genfile` was used to generate data files for
the testsuite, hence its name. However, new operation modes were being
implemented as the testsuite grew more sophisticated, and now
`genfile` is a multi-purpose instrument.

There are four basic operation modes:

File Generation
:   This is the default mode. In this mode, `genfile`
    generates data files.

File Status
:   In this mode `genfile` displays status of specified files.

Set File Time
:   Set last access and modification times of files given in the
    command line.

Synchronous Execution.
:   In this mode `genfile` executes the given program with
    `--checkpoint` option and executes a set of actions when
    specified checkpoints are reached.

* [Generate Mode](#Generate-Mode)
* [Status Mode](#Status-Mode)
* [Set File Time](#Set-File-Time)
* [Exec Mode](#Exec-Mode)

---

Next: [Status Mode](#Status-Mode), Up: [Genfile](#Genfile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### F.1 Generate Mode

In this mode `genfile` creates a data file for the test
suite. The size of the file is given with the `--length`
(`-l`) option. By default the file contents is written to the
standard output, this can be changed using `--file`
(`-f`) command line option. Thus, the following two commands
are equivalent:

```
genfile --length 100 > outfile
genfile --length 100 --file outfile
```

If `--length` is not given, `genfile` will
generate an empty (zero-length) file.

The command line option `--seek=N` istructs `genfile`
to skip the given number of bytes (N) in the output file before
writing to it. It is similar to the `seek=N` of the
`dd` utility.

You can instruct `genfile` to create several files at one
go, by giving it `--files-from` (`-T`) option followed
by a name of file containing a list of file names. Using dash
(‘`-`’) instead of the file name causes `genfile` to read
file list from the standard input. For example:

```
# Read file names from file file.list
genfile --files-from file.list
# Read file names from standard input
genfile --files-from -
```

The list file is supposed to contain one file name per line. To
use file lists separated by ASCII NUL character, use `--null`
(`-0`) command line option:

```
genfile --null --files-from file.list
```

The default data pattern for filling the generated file consists
of first 256 letters of ASCII code, repeated enough times to fill the
entire file. This behavior can be changed with `--pattern`
option. This option takes a mandatory argument, specifying pattern
name to use. Currently two patterns are implemented:

`--pattern=default`
:   The default pattern as described above.

`--pattern=zero`
:   Fills the file with zeroes.

If no file name was given, the program exits with the code
`0`. Otherwise, it exits with `0` only if it was able to
create a file of the specified length.

Special option `--sparse` (`-s`) instructs
`genfile` to create a sparse file. Sparse files consist of
*data fragments*, separated by *holes* or blocks of zeros. On
many operating systems, actual disk storage is not allocated for
holes, but they are counted in the length of the file. To create a
sparse file, `genfile` should know where to put data fragments,
and what data to use to fill them. So, when `--sparse` is given
the rest of the command line specifies a so-called *file map*.

The file map consists of any number of *fragment
descriptors*. Each descriptor is composed of two values: a number,
specifying fragment offset from the end of the previous fragment or,
for the very first fragment, from the beginning of the file, and
*contents string*, that specifies the pattern to fill the fragment
with. File offset can be suffixed with the following quantifiers:

‘`k`’

‘`K`’
:   The number is expressed in kilobytes.

‘`m`’

‘`M`’
:   The number is expressed in megabytes.

‘`g`’

‘`G`’
:   The number is expressed in gigabytes.

Contents string can be either a fragment size or a pattern.
Fragment size is a decimal number, prefixed with an equals sign. It
can be suffixed with a quantifier, as discussed above. If fragment
size is given, the fragment of that size will be filled with the
currently selected pattern (see [–pattern](#Generate-Mode)) and
written to the file.

A pattern is a string of arbitrary ASCII characters. For each
of them, `genfile` will generate a *block* of data,
filled with that character and will write it to the fragment. The size
of block is given by `--block-size` option. It defaults to 512.
Thus, if pattern consists of n characters, the resulting file
fragment will contain `n*block-size` bytes of data.

The last fragment descriptor can have only file offset part. In this
case `genfile` will create a hole at the end of the file up to
the given offset.

A dash appearing as a fragment descriptor instructs
`genfile` to read file map from the standard input. Each line
of input should consist of fragment offset and contents string,
separated by any amount of whitespace.

For example, consider the following invocation:

```
genfile --sparse --file sparsefile 0 ABCD 1M EFGHI 2000K
```

It will create 3101184-bytes long file of the following structure:

|  |  |  |
| --- | --- | --- |
| Offset | Length | Contents |
| 0 | 4\*512=2048 | Four 512-byte blocks, filled with letters ‘`A`’, ‘`B`’, ‘`C`’ and ‘`D`’. |
| 2048 | 1046528 | Zero bytes |
| 1050624 | 5\*512=2560 | Five blocks, filled with letters ‘`E`’, ‘`F`’, ‘`G`’, ‘`H`’, ‘`I`’. |
| 1053184 | 2048000 | Zero bytes |

The exit code of `genfile --sparse` command is `0`
only if created file is actually sparse. If it is not, the
appropriate error message is displayed and the command exists with
code `1`. The `--quite` (`-q`) option suppresses
this behavior. If `--quite` is given, `genfile
--sparse` exits with code `0` if it was able to create the file,
whether the resulting file is sparse or not.

---

Next: [Set File Time](#Set-File-Time), Previous: [Generate Mode](#Generate-Mode), Up: [Genfile](#Genfile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### F.2 Status Mode

In status mode, `genfile` prints file system status for
each file specified in the command line. This mode is toggled by
`--stat` (`-S`) command line option. An optional argument to this
option specifies output *format*: a comma-separated list of
`struct stat` fields to be displayed. This list can contain
following identifiers:

name
:   The file name.

dev

st\_dev
:   Device number in decimal.

ino

st\_ino
:   Inode number.

mode[.number]

st\_mode[.number]
:   File mode in octal. Optional number specifies octal mask to
    be applied to the mode before outputting. For example, `--stat
    mode.777` will preserve lower nine bits of it. Notice, that you can
    use any punctuation character in place of ‘`.`’.

nlink

st\_nlink
:   Number of hard links.

uid

st\_uid
:   User ID of owner.

gid

st\_gid
:   Group ID of owner.

size

st\_size
:   File size in decimal.

blksize

st\_blksize
:   The size in bytes of each file block.

blocks

st\_blocks
:   Number of blocks allocated.

atime

st\_atime
:   Time of last access.

mtime

st\_mtime
:   Time of last modification

ctime

st\_ctime
:   Time of last status change

sparse
:   A boolean value indicating whether the file is ‘`sparse`’.

Modification times are displayed in UTC as
UNIX timestamps, unless suffixed with ‘`H`’ (for
“human-readable”), as in ‘`ctimeH`’, in which case usual
`tar tv` output format is used.

The default output format is: ‘`name,dev,ino,mode,
nlink,uid,gid,size,blksize,blocks,atime,mtime,ctime`’.

For example, the following command will display file names and
corresponding times of last access for each file in the current working
directory:

```
genfile --stat=name,atime *
```

By default, `genfile` follows symbolic links and returns
information about files pointed to by them. To get information about
the symlink files themselves, use the `--no-dereference`
(`-h`) option.

---

Next: [Exec Mode](#Exec-Mode), Previous: [Status Mode](#Status-Mode), Up: [Genfile](#Genfile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### F.3 Set File Time

This mode is requested by the `--set-time` (`-t`)
command line option. In this mode `genfile` operates
similarly to the `touch` command: for each file listed in the
command line, it sets its access and modification times to the current
timestamp or to the value given with the `--date` option. The
`--date` option takes a date specification in
an almost arbitrary format as its argument (see [Date input formats](#Date-input-formats)), e.g.:

```
genfile --set-time --date='2 days ago' a b c
```

By default, `genfile` follows symbolic links and sets
times of the file they point to. This can be changed by supplying the
`--no-dereference` (`-h`) option: if it is given,
`genfile` will change access and modification times of the
symbolic link itself. Notice, that not all operating systems allow
this.

---

Previous: [Set File Time](#Set-File-Time), Up: [Genfile](#Genfile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]
