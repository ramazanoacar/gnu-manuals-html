### B.2 Compiling and Installing `gawk` on Unix-Like Systems [¶](#Compiling-and-Installing-gawk-on-Unix_002dLike-Systems)

Usually, you can compile and install `gawk` by typing only two
commands. However, if you use an unusual system, you may need
to configure `gawk` for your system yourself.

* [Compiling `gawk` for Unix-Like Systems](#Quick-Installation)
* [Shell Startup Files](#Shell-Startup-Files)
* [Additional Configuration Options](#Additional-Configuration-Options)
* [The Configuration Process](#Configuration-Philosophy)
* [Compiling from Git](#Compiling-from-Git)
* [Building the Documentation](#Building-the-Documentation)

---

Next: [Shell Startup Files](#Shell-Startup-Files), Up: [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.2.1 Compiling `gawk` for Unix-Like Systems [¶](#Compiling-gawk-for-Unix_002dLike-Systems)

The normal installation steps should work on all modern commercial
Unix-derived systems, GNU/Linux, BSD-based systems, and the Cygwin
environment for MS-Windows.

After you have extracted the `gawk` distribution, `cd`
to `gawk-5.4.0`. As with most GNU
software, you configure `gawk` for your system by running the
`configure` program. This program is a Bourne shell script that
is generated automatically using GNU Autoconf.
(The Autoconf software is
described fully in
Autoconf—Generating Automatic Configuration Scripts,
which can be found online at
[the Free Software Foundation’s website](https://www.gnu.org/software/autoconf/manual/index.html).)

To configure `gawk`, simply run `configure`:

```
sh ./configure
```

This produces a `Makefile` and `config.h` tailored to your system.
The `config.h` file describes various facts about your system.
You might want to edit the `Makefile` to
change the `CFLAGS` variable, which controls
the command-line options that are passed to the C compiler (such as
optimization levels or compiling for debugging).

Alternatively, you can add your own values for most `make`
variables on the command line, such as `CC` and `CFLAGS`, when
running `configure`:

```
CC=cc CFLAGS=-g sh ./configure
```

See the file `INSTALL` in the `gawk` distribution for
all the details.

After you have run `configure` and possibly edited the `Makefile`,
type:

```
make
```

Shortly thereafter, you should have an executable version of `gawk`.
That’s all there is to it!
To verify that `gawk` is working properly,
run ‘`make check`’. All of the tests should succeed.
If these steps do not work, or if any of the tests fail,
check the files in the `README_d` directory to see if you’ve
found a known problem. If the failure is not described there,
send in a bug report (see [Reporting Problems and Bugs](#Bugs)).

Of course, once you’ve built `gawk`, it is likely that you will
wish to install it. To do so, you need to run the command ‘`make
install`’, as a user with the appropriate permissions. How to do this
varies by system, but on many systems you can use the `sudo`
command to do so. The command then becomes ‘`sudo make install`’. It
is likely that you will be asked for your password, and you will have
to have been set up previously as a user who is allowed to run the
`sudo` command.

* [Building With MPFR](#Compiling-with-MPFR)

---

Up: [Compiling `gawk` for Unix-Like Systems](#Quick-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.2.1.1 Building With MPFR [¶](#Building-With-MPFR)

Use of the MPFR library with `gawk`
is an optional feature: if you have the MPFR and GMP libraries already installed
when you configure and build `gawk`,
`gawk` automatically will be able to use them.

You can install these libraries from source code by fetching them
from the GNU distribution site at `ftp.gnu.org`.

Most modern systems provide package managers which save you the trouble
of building from source. They fetch and install the library header files
and binaries for you. You will need to research how to do this for
your particular system.

---

Next: [Additional Configuration Options](#Additional-Configuration-Options), Previous: [Compiling `gawk` for Unix-Like Systems](#Quick-Installation), Up: [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.2.2 Shell Startup Files [¶](#Shell-Startup-Files-1)

The distribution contains shell startup files `gawk.sh` and
`gawk.csh`, containing functions to aid in manipulating
the `AWKPATH` and `AWKLIBPATH` environment variables.
On a Fedora GNU/Linux system, these files should be installed in `/etc/profile.d`;
on other platforms, the appropriate location may be different.

`gawkpath_default`
:   Reset the `AWKPATH` environment variable to its default value.

`gawkpath_prepend`
:   Add the argument to the front of the `AWKPATH` environment variable.

`gawkpath_append`
:   Add the argument to the end of the `AWKPATH` environment variable.

`gawklibpath_default`
:   Reset the `AWKLIBPATH` environment variable to its default value.

`gawklibpath_prepend`
:   Add the argument to the front of the `AWKLIBPATH` environment variable.

`gawklibpath_append`
:   Add the argument to the end of the `AWKLIBPATH` environment variable.

---

Next: [The Configuration Process](#Configuration-Philosophy), Previous: [Shell Startup Files](#Shell-Startup-Files), Up: [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.2.3 Additional Configuration Options [¶](#Additional-Configuration-Options-1)

There are several additional options you may use on the `configure`
command line when compiling `gawk` from scratch, including:

`--disable-extensions`
:   Disable the extension mechanism within `gawk`. With this
    option, it is not possible to use dynamic extensions. This also
    disables configuring and building the sample extensions in the
    `extension` directory.

    This option may be useful for cross-compiling.
    The default action is to dynamically check if the extensions
    can be configured and compiled.

`--disable-lint`
:   Disable all lint checking within `gawk`. The
    `--lint` and `--lint-old` options
    (see [Command-Line Options](#Options))
    are accepted, but silently do nothing.
    Similarly, setting the `LINT` variable
    (see [Built-in Variables That Control `awk`](#User_002dmodified))
    has no effect on the running `awk` program.

    When used with the GNU Compiler Collection’s (GCC’s)
    automatic dead-code-elimination, this option
    cuts almost 23K bytes off the size of the `gawk`
    executable on GNU/Linux x86\_64 systems. Results on other systems and
    with other compilers are likely to vary.
    Using this option may bring you some slight performance improvement.

    > **CAUTION:** Using this option will cause some of the tests in the test suite
    > to fail. This option may be removed at a later date.

`--disable-mpfr`
:   Skip checking for the MPFR and GMP libraries. This is useful
    mainly for the developers, to make sure nothing breaks if
    MPFR support is not available.

`--disable-nls`
:   Disable all message-translation facilities.
    This is usually not desirable, but it may bring you some slight performance
    improvement.

`--enable-builtin-intdiv0`
:   Add a new built-in function, `intdiv0()`, which generates
    integer division and remainder results at the same time. This
    is discussed more in [Arbitrary-Precision Integer Arithmetic with `gawk`](#Arbitrary-Precision-Integers).

`--enable-O3`
:   Use the GCC option `-O3` when compiling `gawk`
    instead of the standard `-O2`. The additional optimizations
    may improve `gawk`’s performance. However, in other projects,
    `-O3` has been known to cause bugs, thus its use is not
    the default.

`--enable-versioned-extension-dir`
:   Use a versioned directory for extensions. The directory name will
    include the major and minor API versions in it. This makes it possible
    to keep extensions for different API versions on the same system
    without their conflicting with one another.

Use the command ‘`./configure --help`’ to see the full list of
options supplied by `configure`.

---

Next: [Compiling from Git](#Compiling-from-Git), Previous: [Additional Configuration Options](#Additional-Configuration-Options), Up: [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.2.4 The Configuration Process [¶](#The-Configuration-Process)

This section is of interest only if you know something about using the
C language and Unix-like operating systems.

The source code for `gawk` generally attempts to adhere to formal
standards wherever possible. This means that `gawk` uses library
routines that are specified by the ISO C standard and by the POSIX
operating system interface standard.
The `gawk` source code requires using an ISO C compiler (the 1999
standard).

Many Unix systems do not support all of either the ISO or the
POSIX standards. The `missing_d` subdirectory in the `gawk`
distribution contains replacement versions of those functions that are
most likely to be missing.

The `config.h` file that `configure` creates contains
definitions that describe features of the particular operating system
where you are attempting to compile `gawk`. The three things
described by this file are: what header files are available, so that
they can be correctly included, what (supposedly) standard functions
are actually available in your C libraries, and various miscellaneous
facts about your operating system. For example, there may not be an
`st_blksize` element in the `stat` structure. In this case,
‘`HAVE_STRUCT_STAT_ST_BLKSIZE`’ is undefined.

It is possible for your C compiler to lie to `configure`. It may
do so by not exiting with an error when a library function is not
available. To get around this, edit the `custom.h` file.
Use an ‘`#ifdef`’ that is appropriate for your system, and either
`#define` any constants that `configure` should have defined but
didn’t, or `#undef` any constants that `configure` defined and
should not have. The `custom.h` file is automatically included by
the `config.h` file.

It is also possible that the `configure` program generated by
Autoconf will not work on your system in some other fashion.
If you do have a problem, the `configure.ac` file is the input for
Autoconf. You may be able to change this file and generate a
new version of `configure` that works on your system
(see [Reporting Problems and Bugs](#Bugs)
for information on how to report problems in configuring `gawk`).
The same mechanism may be used to send in updates to `configure.ac`
and/or `custom.h`.

---

Next: [Building the Documentation](#Building-the-Documentation), Previous: [The Configuration Process](#Configuration-Philosophy), Up: [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.2.5 Compiling from Git [¶](#Compiling-from-Git-1)

Building `gawk` directly from the development source control
repository is possible, but not recommended for everyday users, as the
code may not be as stable as released versions are. If you really do
want to do that, here are the steps:

```
git clone https://git.savannah.gnu.org/git/gawk.git
cd gawk
./bootstrap.sh && ./configure && make && make check
```

---

Previous: [Compiling from Git](#Compiling-from-Git), Up: [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.2.6 Building the Documentation [¶](#Building-the-Documentation-1)

The generated Info documentation is included in the distribution
`tar` files; you should
not need to rebuild it. However, if it needs to be done, simply running
`make` will do it, assuming that you have a recent enough version
of `makeinfo` installed.

If you wish to build the PDF version of the manuals, you will need
to have TeX installed, and possibly additional packages that
provide the necessary fonts and tools, such as `dvi2pdf`
and `ps2pdf`. You will also need GNU Troff (`groff`)
installed in order to format the reference card and the manual page
(see [Contents of the `gawk` Distribution](#Distribution-contents)). Managing this process is beyond the
scope of this Web page.

Assuming you have all you need, then the following commands produce the
PDF versions of the documentation:

```
cd doc
make pdf
```

This creates PDF versions of all three Texinfo documents included
in the distribution, as well as of the manual page and the reference card.

Similarly, if you have a recent enough version of `makeinfo`,
you can make the HTML version of the manuals with:

```
cd doc
make html
```

This creates HTML versions of all three Texinfo documents included
in the distribution.

---

Next: [Reporting Problems and Bugs](#Bugs), Previous: [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation), Up: [Installing `gawk`](#Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### B.3 Installation on Other Operating Systems [¶](#Installation-on-Other-Operating-Systems)

This section describes how to install `gawk` on
various non-Unix systems.

* [Installation on MS-Windows](#PC-Installation)
* [Compiling and Installing `gawk` on OpenVMS](#OpenVMS-Installation)

---

Next: [Compiling and Installing `gawk` on OpenVMS](#OpenVMS-Installation), Up: [Installation on Other Operating Systems](#Non_002dUnix-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.1 Installation on MS-Windows [¶](#Installation-on-MS_002dWindows)

This section covers installation and usage of `gawk`
on Intel architecture machines running any version of MS-Windows.
In this section, the term “Windows32”
refers to any of Microsoft Windows 95/98/ME/NT/2000/XP/Vista/7/8/10/11.

See also the `README_d/README.pc` file in the distribution.

* [Installing a Prepared Distribution for MS-Windows Systems](#PC-Binary-Installation)
* [Compiling `gawk` for PC Operating Systems](#PC-Compiling)
* [Using `gawk` on PC Operating Systems](#PC-Using)
* [Using `gawk` In The Cygwin Environment](#Cygwin)
* [Using `gawk` In The MSYS Environment](#MSYS)

---

Next: [Compiling `gawk` for PC Operating Systems](#PC-Compiling), Up: [Installation on MS-Windows](#PC-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.1.1 Installing a Prepared Distribution for MS-Windows Systems [¶](#Installing-a-Prepared-Distribution-for-MS_002dWindows-Systems)

The only supported binary distribution for MS-Windows systems
is that provided by Eli Zaretskii’s [“ezwinports”](https://sourceforge.net/projects/ezwinports/) project. Install the compiled `gawk` from there.
Note that to run that port, you need to have the
`libgcc_s_dw2-1.dll` file installed on your system. This file is
part of the GCC distribution, and should reside either in the same
directory where you install `gawk.exe` or somewhere on your
system’s `Path`. You can download this file from
[the MinGW site](https://osdn.net/projects/mingw/releases); look under the
“MinGW.org Compiler Collection (GCC)” for the `LibGCC-1.DLL`
download.

---

Next: [Using `gawk` on PC Operating Systems](#PC-Using), Previous: [Installing a Prepared Distribution for MS-Windows Systems](#PC-Binary-Installation), Up: [Installation on MS-Windows](#PC-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.1.2 Compiling `gawk` for PC Operating Systems [¶](#Compiling-gawk-for-PC-Operating-Systems)

`gawk` can be compiled for Windows32 using MinGW (Windows32).
The file `README_d/README.pc` in the `gawk` distribution
contains additional notes, and `pc/Makefile` contains important
information on compilation options.

To build `gawk` for Windows32, copy the files in
the `pc` directory (*except* for `ChangeLog`) to the
directory with the rest of the `gawk` sources, then invoke
`make` with the appropriate target name as an argument to
build `gawk`. The `Makefile` copied from the `pc`
directory contains a configuration section with comments and may need
to be edited in order to work with your `make` utility.

The `Makefile` supports a number of targets for building various
Windows32 versions. A list of targets is printed if the
`make` command is given without a target. As an example,
to build a native MS-Windows binary of `gawk` using the MinGW tools,
type ‘`make mingw32`’.

If you are building with MinGW using anything other than
`pc/Makefile`, be sure to pass the
`-D__USE_MINGW_ANSI_STDIO` command-line option to GCC. This is
necessary for correct operation of Gawk on Windows, in particular for
displaying translated messages.

---

Next: [Using `gawk` In The Cygwin Environment](#Cygwin), Previous: [Compiling `gawk` for PC Operating Systems](#PC-Compiling), Up: [Installation on MS-Windows](#PC-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.1.3 Using `gawk` on PC Operating Systems [¶](#Using-gawk-on-PC-Operating-Systems)

Information in this section applies to the MinGW port of
`gawk` for MS-Windows. See [Using `gawk` In The Cygwin Environment](#Cygwin) for information about
the Cygwin port.

The MS-Windows version of `gawk` searches for
program files as described in [The `AWKPATH` Environment Variable](#AWKPATH-Variable). However,
semicolons (rather than colons) separate elements in the `AWKPATH`
variable. If `AWKPATH` is not set or is empty, then the default
search path is ‘`.;d:/usr/lib/awk;c:/lib/awk;c:/gnu/lib/awk`’.

Similarly, the `AWKLIBPATH` environment variable, which tells
`gawk` where to look for dynamic extensions (see [Writing Extensions for `gawk`](#Dynamic-Extensions)), also uses semicolons to separate directories. If not
set in the environment, the default value hard-coded into
`gawk` is ‘`d:/usr/lib/gawk/ext-api-version`’,
where api-version is the version of gawk API version for which
`gawk` was compiled (see [API Version Constants and Variables](#Extension-Versioning)).

MS-Windows traditionally supported internationalization and
localization via *codepages*. Each Windows locale specifies a
system-wide codepage, which defines both the non-ASCII characters
supported by Windows and their encoding into small integer values.
Most Windows locales use a single-byte encoding; the notable exceptions
are CJK (Chinese, Japanese, and Korean) codepages which use
one or two bytes for each non-ASCII character. Recent versions of
MS-Windows also support Unicode and the UTF-8 encoding, but introduction of this
support is slow: as of this writing, setting UTF-8 as the system-wide
codepage on Windows is still considered an experimental feature, which by
default is turned off. The Windows designation of the UTF-8 encoding is
known as *codepage 65001*.

The MinGW build of `gawk` for MS-Windows supports multibyte
characters according to the codepage defined by the current system
locale, and it also includes (starting with version 5.4) reasonable
support for text encoded in UTF-8. When the system’s codepage is
anything other than 65001, `gawk` uses the functions from the
MS-Windows C runtime library to convert between multibyte and
wide-character representation of text, and for character
classification and collation; this is in most cases limited to
characters inside the Unicode Basic Multilingual Plane
(BMP), due to the limitations of the Windows runtime. By
contrast, if the system’s codepage is set to 65001, `gawk`
attempts to support the full range of Unicode codepoints, including
the BMP and all the 16 Supplementary Planes defined by
Unicode, by using its own conversion and classification routines
instead of those in the Windows C runtime. (This might still fail to
work as well as it does on modern POSIX systems, because these
routines rely on the character database data included with your
Windows installation.)

When codepage 65001 is used, `gawk` uses UTF-8 to encode
multibyte text, and converts between UTF-8 and 32-bit Unicode
codepoints internally. Thus on Windows, to make sure `awk` programs
written for POSIX systems which handle multibyte non-ASCII text work
correctly on MS-Windows, you should use codepage 65001.

As a special feature, `gawk` version 5.4 and later will use
UTF-8 internally even when the system-wide codepage is something other
than 65001, if `gawk` detects that the Windows terminal window
in which it runs uses codepage 65001 for console output. (Unlike on
POSIX systems, Windows users can change the console encoding
independently of the system-wide locale’s encoding.) To switch the
Windows terminal to using UTF-8 encoding, type ‘`chcp 65001`’ at the
Windows shell’s prompt. This causes `gawk` on Windows to
support the full range of Unicode characters even without changing the
system-wide locale’s codepage. We recommend that you use codepage
65001 on Windows 11 and later systems, where the Windows terminal
supports UTF-8 quite well.

Note that `gawk` on Windows will still treat all input data as
single-byte characters when invoked with the `-b` command-line
option (see [Command-Line Options](#Options)), or when running under
the `"C"` locale (i.e., when the `LC_ALL` environment
variable is set to the value ‘`C`’ by typing ‘`set LC_ALL=C`’ at
the shell prompt before invoking `gawk`). This overrides the
codepage-defined encoding of text.

Under MS-Windows,
`gawk` (and many other text programs) silently
translates end-of-line ‘`\r\n`’ to ‘`\n`’ on input and ‘`\n`’
to ‘`\r\n`’ on output. A special `BINMODE` variable (c.e.)
allows control over these translations and is interpreted as follows:

* If `BINMODE` is `"r"` or one,
  then
  binary mode is set on read (i.e., no translations on reads).
* If `BINMODE` is `"w"` or two,
  then
  binary mode is set on write (i.e., no translations on writes).
* If `BINMODE` is `"rw"` or `"wr"` or three,
  binary mode is set for both read and write.
* `BINMODE=non-null-string` is
  the same as ‘`BINMODE=3`’ (i.e., no translations on
  reads or writes). However, `gawk` issues a warning
  message if the string is not one of `"rw"` or `"wr"`.

The modes for standard input and standard output are set one time
only (after the
command line is read, but before processing any of the `awk` program).
Setting `BINMODE` for standard input or
standard output is accomplished by using an
appropriate ‘`-v BINMODE=N`’ option on the command line.
`BINMODE` is set at the time a file or pipe is opened and cannot be
changed midstream.

On POSIX-compatible systems, this variable’s value has no effect.
Thus, if you think your program will run on multiple different systems
and that you may need to use `BINMODE`, you should simply set it
(in the program or on the command line) unconditionally, and not worry
about the operating system on which your program is running.

The name `BINMODE` was chosen to match `mawk`
(see [Other Freely Available `awk` Implementations](#Other-Versions)).
`mawk` and `gawk` handle `BINMODE` similarly; however,
`mawk` adds a ‘`-W BINMODE=N`’ option and an environment
variable that can set `BINMODE`, `RS`, and `ORS`. The
files `binmode[1-3].awk` (under `gnu/lib/awk` in some of the
prepared binary distributions) have been chosen to match `mawk`’s ‘`-W
BINMODE=N`’ option. These can be changed or discarded; in particular,
the setting of `RS` giving the fewest “surprises” is open to debate.
`mawk` uses ‘`RS = "\r\n"`’ if binary mode is set on read, which is
appropriate for files with the MS-DOS-style end-of-line.

To illustrate, the following examples set binary mode on writes for standard
output and other files, and set `ORS` as the “usual” MS-DOS-style
end-of-line:

```
gawk -v BINMODE=2 -v ORS="\r\n" ...
```

or:

```
gawk -v BINMODE=w -f binmode2.awk ...
```

These give the same result as the ‘`-W BINMODE=2`’ option in
`mawk`.
The following changes the record separator to `"\r\n"` and sets binary
mode on reads, but does not affect the mode on standard input:

```
gawk -v RS="\r\n" -e "BEGIN { BINMODE = 1 }" ...
```

or:

```
gawk -f binmode1.awk ...
```

With proper quoting, in the first example the setting of `RS` can be
moved into the `BEGIN` rule.

Under MS-Windows, the MinGW port of `gawk` supports
both the ‘`|&`’ operator and TCP/IP networking
(see [Using `gawk` for Network Programming](#TCP_002fIP-Networking)).

---

Next: [Using `gawk` In The MSYS Environment](#MSYS), Previous: [Using `gawk` on PC Operating Systems](#PC-Using), Up: [Installation on MS-Windows](#PC-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.1.4 Using `gawk` In The Cygwin Environment [¶](#Using-gawk-In-The-Cygwin-Environment)

`gawk` can be built and used “out of the box” under MS-Windows
if you are using the [Cygwin environment](https://www.cygwin.com).
This environment provides an excellent simulation of GNU/Linux, using
Bash, GCC, GNU Make,
and other GNU programs. Compilation and installation for Cygwin is the
same as for a Unix system:

```
tar -xvpzf gawk-5.4.0.tar.gz
cd gawk-5.4.0
./configure
make && make check
```

When compared to GNU/Linux on the same system, the ‘`configure`’
step on Cygwin takes considerably longer. However, it does finish,
and then the ‘`make`’ proceeds as usual.

You may also install `gawk` using the regular Cygwin installer.
In general Cygwin supplies the latest released version.

Recent versions of Cygwin open all files in binary mode. This means
that you should use ‘`RS = "\r?\n"`’ in order to be able to
handle standard MS-Windows text files with carriage-return plus
line-feed line endings.

The Cygwin environment supports
both the ‘`|&`’ operator and TCP/IP networking
(see [Using `gawk` for Network Programming](#TCP_002fIP-Networking)).

---

Previous: [Using `gawk` In The Cygwin Environment](#Cygwin), Up: [Installation on MS-Windows](#PC-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.1.5 Using `gawk` In The MSYS Environment [¶](#Using-gawk-In-The-MSYS-Environment)

In the MSYS environment under MS-Windows, `gawk` automatically
uses binary mode for reading and writing files. Thus, there is no
need to use the `BINMODE` variable.

This can cause problems with other Unix-like components that have
been ported to MS-Windows that expect `gawk` to do automatic
translation of `"\r\n"`, because it won’t.

Under MSYS2, compilation using the standard ‘`./configure && make`’
recipe works “out of the box.”

---

Previous: [Installation on MS-Windows](#PC-Installation), Up: [Installation on Other Operating Systems](#Non_002dUnix-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.2 Compiling and Installing `gawk` on OpenVMS [¶](#Compiling-and-Installing-gawk-on-OpenVMS)

This subsection describes how to compile and install `gawk` under OpenVMS.

* [Compiling `gawk` on OpenVMS](#OpenVMS-Compilation)
* [Compiling `gawk` Dynamic Extensions on OpenVMS](#OpenVMS-Dynamic-Extensions)
* [Installing `gawk` on OpenVMS](#OpenVMS-Installation-Details)
* [Running `gawk` on OpenVMS](#OpenVMS-Running)
* [The OpenVMS GNV Project](#OpenVMS-GNV)

---

Next: [Compiling `gawk` Dynamic Extensions on OpenVMS](#OpenVMS-Dynamic-Extensions), Up: [Compiling and Installing `gawk` on OpenVMS](#OpenVMS-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.2.1 Compiling `gawk` on OpenVMS [¶](#Compiling-gawk-on-OpenVMS)

To compile `gawk` under OpenVMS, there is a `descrip.mms` for
use with `MMK` or `MMS` as the equivalent of a `makefile`.
`MMS` requires a DCL symbol for the hardware type such
‘`"__x86_64__ = "TRUE"`’ to properly build.

```
$ __x86_64__ == "TRUE"
$ MMS/DESCRIPTION=[.vms]descrip.mms gawk
```

or:

```
$ MMK/DESCRIPTION=[.vms]descrip.mms gawk
```

`MMK` is an open source, free, near-clone of `MMS` and
can better handle ODS-5 volumes with upper- and lowercase file names.
`MMK` is available from <https://github.com/endlesssoftware/mmk>.

With ODS-5 volumes and extended parsing enabled, the case of the target
parameter may need to be exact.

`gawk` has been tested using these VMS Software, Inc.
Community editions:

* VSI C x86-64 V7.6-001 (GEM 50YAN) on OpenVMS x86\_64 V9.2-3
* HP C V7.3-010 on OpenVMS Alpha V8.4-2L1.
* HP C V7.3-020 on OpenVMS IA64 V8.4-2L3.[121](#FOOT121)

Due to HPE cancelling the Hobbyist licensing program, no more testing is
being done on older releases of OpenVMS.

See [The OpenVMS GNV Project](#OpenVMS-GNV) for information on building
`gawk` as a PCSI kit that is compatible with the GNV product.

---

Next: [Installing `gawk` on OpenVMS](#OpenVMS-Installation-Details), Previous: [Compiling `gawk` on OpenVMS](#OpenVMS-Compilation), Up: [Compiling and Installing `gawk` on OpenVMS](#OpenVMS-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.2.2 Compiling `gawk` Dynamic Extensions on OpenVMS [¶](#Compiling-gawk-Dynamic-Extensions-on-OpenVMS)

The extensions that have been ported to OpenVMS can be built using one of
the following commands:

```
$ __x86_64__ == "TRUE"
$ MMS/DESCRIPTION=[.vms]descrip.mms extensions
```

or:

```
$ MMK/DESCRIPTION=[.vms]descrip.mms extensions
```

`gawk` uses `AWKLIBPATH` as either an environment variable
or a logical name to find the dynamic extensions.

Dynamic extensions need to be compiled with the same compiler options for
floating-point, pointer size, and symbol name handling as were used
to compile `gawk` itself.
X86\_64, Alpha, and Itanium should use IEEE floating point. The pointer size
is 32 bits, and the symbol name handling should be exact case with CRC
shortening for symbols longer than 32 bits.

```
/name=(as_is,short)
/float=ieee/ieee_mode=denorm_results
```

Compile-time macros need to be defined before the first OpenVMS-supplied
header file is included, as follows:

```
#if (__CRTL_VER >= 70200000)
#define _LARGEFILE 1
#endif

#ifdef __CRTL_VER
#if __CRTL_VER >= 80200000
#define _USE_STD_STAT 1
#endif
#endif
```

If you are writing your own extensions to run on OpenVMS, you must supply these
definitions yourself. The `config.h` file created when building `gawk`
on OpenVMS does this for you; if instead you use that file or a similar one, then you
must remember to include it before any OpenVMS-supplied header files.

---

Next: [Running `gawk` on OpenVMS](#OpenVMS-Running), Previous: [Compiling `gawk` Dynamic Extensions on OpenVMS](#OpenVMS-Dynamic-Extensions), Up: [Compiling and Installing `gawk` on OpenVMS](#OpenVMS-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.2.3 Installing `gawk` on OpenVMS [¶](#Installing-gawk-on-OpenVMS)

To use `gawk`, all you need is a “foreign” command, which is a
`DCL` symbol whose value begins with a dollar sign. For example:

```
$ GAWK :== $disk1:[gnubin]gawk
```

Substitute the actual location of `gawk.exe` for
‘`$disk1:[gnubin]`’. The symbol should be placed in the
`login.com` of any user who wants to run `gawk`,
so that it is defined every time the user logs on.
Alternatively, the symbol may be placed in the system-wide
`sylogin.com` procedure, which allows all users
to run `gawk`.

If your `gawk` was installed by a PCSI kit into the
`GNV$GNU:` directory tree, the program will be known as
`GNV$GNU:[bin]gnv$gawk.exe` and the help file will be
`GNV$GNU:[vms_help]gawk.hlp`.

The PCSI kit also installs a `GNV$GNU:[vms_bin]gawk_verb.cld` file
that can be used to add `gawk` and `awk` as DCL commands.

For just the current process you can use:

```
$ set command gnv$gnu:[vms_bin]gawk_verb.cld
```

Or the system manager can use `GNV$GNU:[vms_bin]gawk_verb.cld` to
add the `gawk` and `awk` commands to the system-wide ‘`DCLTABLES`’.

The DCL syntax is documented in the `gawk.hlp` file.

Optionally, the `gawk.hlp` entry can be loaded into an OpenVMS help library:

```
$ LIBRARY/HELP sys$help:helplib [.vms]gawk.hlp
```

(You may want to substitute a site-specific help library rather than
the standard OpenVMS library ‘`HELPLIB`’.) After loading the help text,
the command:

```
$ HELP GAWK
```

provides information about both the `gawk` implementation and the
`awk` programming language.

The logical name ‘`AWK_LIBRARY`’ can designate a default location
for `awk` program files. For the `-f` option, if the specified
file name has no device or directory path information in it, `gawk`
looks in the current directory first, then in the directory specified
by the translation of ‘`AWK_LIBRARY`’ if the file is not found.
If, after searching in both directories, the file still is not found,
`gawk` appends the suffix ‘`.awk`’ to the file name and retries
the file search. If ‘`AWK_LIBRARY`’ has no definition, a default value
of ‘`SYS$LIBRARY:`’ is used for it.

---

Next: [The OpenVMS GNV Project](#OpenVMS-GNV), Previous: [Installing `gawk` on OpenVMS](#OpenVMS-Installation-Details), Up: [Compiling and Installing `gawk` on OpenVMS](#OpenVMS-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.2.4 Running `gawk` on OpenVMS [¶](#Running-gawk-on-OpenVMS)

Command-line parsing and quoting conventions are significantly different
on OpenVMS, so examples in this Web page or from other sources often need minor
changes. They *are* minor though, and all `awk` programs
should run correctly.

Here are a couple of trivial tests:

```
$ gawk -- "BEGIN {print ""Hello, World!""}"
$ gawk -"W" version
! could also be -"W version" or "-W version"
```

Note that uppercase and mixed-case text must be quoted.

The OpenVMS port of `gawk` includes a `DCL`-style interface in addition
to the original shell-style interface (see the help entry for details).
One side effect of dual command-line parsing is that if there is only a
single parameter (as in the quoted program string), the command
becomes ambiguous. To work around this, the normally optional `--`
flag is required to force Unix-style parsing rather than `DCL` parsing.
If any other dash-type options (or multiple parameters such as data files to
process) are present, there is no ambiguity and `--` can be omitted.

The `exit` value is a Unix-style value and is encoded into an OpenVMS exit
status value when the program exits.

The OpenVMS severity bits will be set based on the `exit` value.
A failure is indicated by 1, and OpenVMS sets the `ERROR` status.
A fatal error is indicated by 2, and OpenVMS sets the `FATAL` status.
All other values will have the `SUCCESS` status. The exit value is
encoded to comply with OpenVMS coding standards and will have the
`C_FACILITY_NO` of `0x350000` with the constant `0xA000`
added to the number shifted over by 3 bits to make room for the severity codes.

To extract the actual `gawk` exit code from the OpenVMS status, use:

```
unix_status = (vms_status .and. %x7f8) / 8
```

A C program that uses `exec()` to call `gawk` will get the original
Unix-style exit value.

OpenVMS reports time values in GMT unless one of the `SYS$TIMEZONE_RULE`
or `TZ` logical names is set.

The default search path, when looking for `awk` program files specified
by the `-f` option, is `"SYS$DISK:[],AWK_LIBRARY:"`. The logical
name `AWKPATH` can be used to override this default. The format
of `AWKPATH` is a comma-separated list of directory specifications.
When defining it, the value should be quoted so that it retains a single
translation and not a multitranslation `RMS` searchlist.

This restriction also applies to running `gawk` under GNV,
as redirection is always to a DCL command.

If you are redirecting data to an OpenVMS command or utility, the current
implementation requires setting up an OpenVMS foreign command that runs
a command file before invoking `gawk`.
(This restriction may be removed in a future release of `gawk` on OpenVMS.)

Without this command file, the input data will also appear prepended
to the output data.

This also allows simulating POSIX commands that are not found on OpenVMS or the
use of GNV utilities.

The example below is for `gawk` redirecting data to the OpenVMS
`sort` command.

```
$ sort = "@device:[dir]vms_gawk_sort.com"
```

The command file needs to be of the format in the example below.

The first line inhibits the passed input data from also showing up in the
output. It must be in the format in the example.

The next line creates a foreign command that overrides the outer foreign
command which prevents an infinite recursion of command files.

The next to the last command redirects `sys$input` to be
`sys$command`, in order to pick up the data that is being redirected
to the command.

The last line runs the actual command. It must be the last command as the data
redirected from `gawk` will be read when the command file ends.

```
$!'f$verify(0,0)'
$ sort := sort
$ define/user sys$input sys$command:
$ sort sys$input: sys$output:
```

---

Previous: [Running `gawk` on OpenVMS](#OpenVMS-Running), Up: [Compiling and Installing `gawk` on OpenVMS](#OpenVMS-Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.3.2.5 The OpenVMS GNV Project [¶](#The-OpenVMS-GNV-Project)

The OpenVMS GNV package provides a build environment similar to POSIX with ports
of a collection of open source tools. The `gawk` found in the GNV
base kit is an older port. Currently, the GNV project is being reorganized
to supply individual PCSI packages for each component.
See <https://sourceforge.net/p/gnv/wiki/InstallingGNVPackages/>.

The normal build procedure for `gawk` produces a program that
is suitable for use with GNV.

To be compatible and share common code with GNV projects, all
OpenVMS-specific defines and prototypes for `gawk` are in the process of
being moved to `vms/config_vms.h`. The `vms` directory will have
missing or updated system header files and any replacement routines that will
hide most of the differences between OpenVMS and Linux. This is intended to
make `gawk` easier to maintain.

The file `vms/gawk_build_steps.txt` in the distribution documents
the procedure for building an OpenVMS PCSI kit that is compatible with GNV.

---

Next: [Other Freely Available `awk` Implementations](#Other-Versions), Previous: [Installation on Other Operating Systems](#Non_002dUnix-Installation), Up: [Installing `gawk`](#Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### B.4 Reporting Problems and Bugs [¶](#Reporting-Problems-and-Bugs)

> *There is nothing more dangerous than a bored archaeologist.*

— *Douglas Adams, The Hitchhiker’s Guide to the Galaxy*

If you have problems with `gawk` or think that you have found a bug,
report it to the developers; we cannot promise to do anything,
but we might well want to fix it.

* [Defining What Is and What Is Not A Bug](#Bug-definition)
* [Submitting Bug Reports](#Bug-address)
* [Please Don’t Post Bug Reports to USENET](#Usenet)
* [What To Do If You Think There Is A Performance Issue](#Performance-bugs)
* [Where To Send Non-bug Questions](#Asking-for-help)
* [Reporting Problems with Non-Unix Ports](#Maintainers)

---

Next: [Submitting Bug Reports](#Bug-address), Up: [Reporting Problems and Bugs](#Bugs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.4.1 Defining What Is and What Is Not A Bug [¶](#Defining-What-Is-and-What-Is-Not-A-Bug)

Before talking about reporting bugs, let’s define what is a bug,
and what is not.

A bug is:

* When `gawk` behaves differently from what’s described
  in the POSIX standard, and that difference is not mentioned
  in this Web page as being done on purpose.
* When `gawk` behaves differently from what’s described
  in this Web page.
* When `gawk` behaves differently from other `awk`
  implementations in particular circumstances, and that behavior cannot
  be attributed to an additional feature in `gawk`.
* Something that is obviously wrong, such as a core dump.
* When this Web page is unclear or ambiguous about a particular
  feature’s behavior.

The following things are *not* bugs, and should not be reported
to the bug mailing list. You can ask about them on the “help” mailing
list (see [Where To Send Non-bug Questions](#Asking-for-help)), but don’t be surprised if you get an
answer of the form “that’s how `gawk` behaves and it isn’t
going to change.” Here’s the list:

* Missing features, for any definition of *feature*. For example,
  additional built-in arithmetic functions, or additional ways to split
  fields or records, or anything else.

  The number of features that `gawk` does *not* have is
  by definition infinite. It cannot be all things to all people.
  In short, just because `gawk` doesn’t do what *you*
  think it should, it’s not necessarily a bug.
* Behaviors that are defined by the POSIX standard and/or for historical
  compatibility with Unix `awk`. Even if you happen to dislike
  those behaviors, they’re not going to change: changing them would
  break millions of existing `awk` programs.
* Behaviors that differ from how it’s done in other languages. `awk`
  and `gawk` stand on their own and do not have to follow the crowd.
  This is particularly true when the requested behavior change would break
  backwards compatibility.

  This applies also to differences in behavior between `gawk`
  and other language compilers and interpreters, such as wishes for more
  detailed descriptions of what the problem is when a syntax error is
  encountered.
* Documentation issues of the form “the manual doesn’t tell me how to
  do XYZ.” The manual is not a cookbook to solve every little problem
  you may have. Its purpose is to teach you how to solve your problems
  on your own.
* General questions and discussion about `awk` programming or
  why `gawk` behaves the way it does. For that use the “help”
  mailing list: see [Where To Send Non-bug Questions](#Asking-for-help).

For more information, see [Fork My Code, Please!—An Open Letter To Those of You Who Are Unhappy](https://www.skeeve.com/fork-my-code.html),
by Arnold Robbins and Chet Ramey.

| A Note About Fuzzers |
| --- |
| In recent years, people have been running “fuzzers” to generate invalid `awk` programs in order to find and report (so-called) bugs in `gawk`.  In general, such reports are not of much practical use. The programs they create are not realistic and the bugs found are generally from some kind of memory corruption that is fatal anyway.  So, if you want to run a fuzzer against `gawk` and report the results, you may do so, but be aware that such reports don’t carry the same weight as reports of real bugs do. |

---

Next: [Please Don’t Post Bug Reports to USENET](#Usenet), Previous: [Defining What Is and What Is Not A Bug](#Bug-definition), Up: [Reporting Problems and Bugs](#Bugs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.4.2 Submitting Bug Reports [¶](#Submitting-Bug-Reports)

Before reporting a bug, make sure you have really found a genuine bug.

Here are the steps for submitting a bug report. Following them will
make both your life and the lives of the maintainers much easier.

1. Make sure that what you want to report is appropriate.
   See [Defining What Is and What Is Not A Bug](#Bug-definition). If it’s not, you are wasting your
   time and ours.
2. Verify that you have the latest version of `gawk`.
   Many bugs (usually subtle ones) are fixed at each release, and if yours
   is out-of-date, the problem may already have been solved.
3. Please see if setting the environment variable `LC_ALL`
   to `LC_ALL=C` causes things to behave as you expect. If so, it’s
   a locale issue, and may or may not really be a bug.
4. Carefully reread the documentation and see if it says you can do
   what you’re trying to do. If it’s not clear whether you should be able
   to do something or not, report that too; it’s a bug in the documentation!
5. Before reporting a bug or trying to fix it yourself, try to isolate it
   to the smallest possible `awk` program and input data file that
   reproduce the problem.
6. Use the `gawkbug` program to submit the bug report. This
   program sets up a bug report template and opens it in your editor.
   You then need to edit it appropriately to include:
   * The program and data file.
   * The exact results
     `gawk` gave you. Also say what you expected to occur; this helps
     us decide whether the problem is really in the documentation.
   * A fix if you have one.
7. Do *not* send screenshots. Instead, use copy/paste to send text, or
   send files.
8. Please be sure to send all mail in *plain text*,
   not (or not exclusively) in HTML.
9. *All email must be in English. This is the only language
   understood in common by all the maintainers.*

The `gawkbug` program sends email to
“bug dash gawk at gnu dot org”.

The `gawk` maintainers subscribe to this address, and
thus they will receive your bug report.
Do *not* send mail to the maintainers directly;
the bug reporting address is preferred because the
email list is archived at the GNU Project.

If you are using OpenVMS or the MinGW build of `gawk`,
the `gawkbug` script won’t be available. Please send
the previously listed information directly in an email to
the bug list. Please send any test program or data files
as attachments, instead of inline in the email, to avoid
their being mangled by various mail systems.

> **NOTE:** Many distributions of GNU/Linux and the various BSD-based operating systems
> have their own bug reporting systems. If you report a bug using your distribution’s
> bug reporting system, you should also send a copy to
> “bug dash gawk at gnu dot org”.
>
> This is for two reasons. First, although some distributions forward
> bug reports “upstream” to the GNU mailing list, many don’t, so there is a good
> chance that the `gawk` maintainers won’t even see the bug report! Second,
> mail to the GNU list is archived, and having everything at the GNU Project
> keeps things self-contained and not dependent on other organizations.

Please note: We ask that you follow the
[GNU Kind Communication Guidelines](https://gnu.org/philosophy/kind-communication.html) in your correspondence on the
list (as well as off of it).

Additionally, just to be clear, on the list:

* Personal attacks (so called *ad hominem attacks*) are always inappropriate.
  If you make such attacks, you will be banned from the list.
* Discussion of proprietary software is strongly discouraged on GNU mailing lists.
  If you continue to discuss such software after being asked not to, you
  will also be banned.

---

Next: [What To Do If You Think There Is A Performance Issue](#Performance-bugs), Previous: [Submitting Bug Reports](#Bug-address), Up: [Reporting Problems and Bugs](#Bugs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.4.3 Please Don’t Post Bug Reports to USENET [¶](#Please-Don_0027t-Post-Bug-Reports-to-USENET)

> I gave up on Usenet a couple of years ago and haven’t really looked back.
> It’s like sports talk radio—you feel smarter for not having read it.

— *Chet Ramey*

Please do *not* try to report bugs in `gawk` by posting to the
Usenet/Internet newsgroup `comp.lang.awk`. Although some of the
`gawk` developers occasionally read this news group, the primary
`gawk` maintainer no longer does. Thus it’s virtually guaranteed
that he will *not* see your posting.

If you really don’t care about the previous paragraph and continue to
post bug reports in `comp.lang.awk`, then understand that you’re
not reporting bugs, you’re just whining.

Similarly, posting bug reports or questions in web forums (such
as [Stack Overflow](https://stackoverflow.com/)) may get you
an answer, but it won’t be from the `gawk` maintainers,
who do not spend their time in web forums. The steps described here are
the only officially recognized way for reporting bugs. Really.

---

Next: [Where To Send Non-bug Questions](#Asking-for-help), Previous: [Please Don’t Post Bug Reports to USENET](#Usenet), Up: [Reporting Problems and Bugs](#Bugs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.4.4 What To Do If You Think There Is A Performance Issue [¶](#What-To-Do-If-You-Think-There-Is-A-Performance-Issue)

If you think that `gawk` is too slow at doing a particular task,
you should investigate before sending in a bug report. Here are the steps
to follow:

1. Run `gawk` with the `--profile` option (see [Command-Line Options](#Options))
   to see what your
   program is doing. It may be that you have written it in an inefficient manner.
   For example, you may be doing something for every record that could be done
   just once, for every file.
   (Use a `BEGINFILE` rule; see [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE).)
   Or you may be doing something for every file that only needs to be done
   once per run of the program.
   (Use a `BEGIN` rule; see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND).)
2. If profiling at the `awk` level doesn’t help, then you will
   need to compile `gawk` itself for profiling at the C language level.

   To do that, start with the latest released version of
   `gawk`. Unpack the source code in a new directory, and configure
   it:

   ```
   $ tar -xpzvf gawk-X.Y.Z.tar.gz
   ...                                Output omitted
   $ cd gawk-X.Y.Z
   $ ./configure
   ...                                Output omitted
   ```
3. Edit the files `Makefile` and `support/Makefile`.
   Change every instance of `-O2` or `-O` to `-pg`.
   This causes `gawk` to be compiled for profiling.
4. Compile the program by running the `make` command:

   ```
   $ make
   ...                                Output omitted
   ```
5. Run the freshly compiled `gawk` on a *real* program,
   using *real* data. Using an artificial program to try to time one
   particular feature of `gawk` is useless; real `awk` programs
   generally spend most of their time doing I/O, not computing. If you want to prove
   that something is slow, it *must* be done using a real program and real data.

   Use a data file that is large enough for the statistical profiling to measure
   where `gawk` spends its time. It should be at least 100 megabytes in size.

   ```
   $ ./gawk -f realprogram.awk realdata > /dev/null
   ```
6. When done, you should have a file in the current directory named `gmon.out`.
   Run the command ‘`gprof gawk gmon.out > gprof.out`’.
7. Submit a bug report explaining what you think is slow. Include the `gprof.out`
   file with it.

   Preferably, you should also submit the program and the data, or else indicate where to
   get the data if the file is large.
8. If you have not submitted your program and data, be prepared to apply patches and
   rerun the profiling in order to see if the patches were effective.

If you are incapable or unwilling to do the steps listed above, then you will
just have to live with `gawk` as it is.

---

Next: [Reporting Problems with Non-Unix Ports](#Maintainers), Previous: [What To Do If You Think There Is A Performance Issue](#Performance-bugs), Up: [Reporting Problems and Bugs](#Bugs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.4.5 Where To Send Non-bug Questions [¶](#Where-To-Send-Non_002dbug-Questions)

If you have questions related to `awk` programming, or why `gawk`
behaves a certain way, or any other `awk`- or `gawk`-related issue,
please *do not* send it to the bug reporting address.

As of July, 2021, there is a separate mailing list for this purpose:
“help dash gawk at gnu dot org”.
Anything that is not a bug report should be sent to that list.

> **NOTE:** If you disregard these directions and send non-bug mails to the bug list,
> you will be told to use the help list.
> After two such requests you will be silently *blacklisted* from the bug list.

Please note: As with the bug list, we ask that you follow the
[GNU Kind Communication Guidelines](https://gnu.org/philosophy/kind-communication.html) in your correspondence on the help
list (as well as off of it).

If you wish to the subscribe to the list, in order to help out
others, or to learn from others, here are instructions, courtesy
of Bob Proulx:

*Subscribe by email*
:   Send an email message to
    “help dash gawk dash request at gnu dot org”
    with “subscribe” in
    the body of the message. The subject does not matter and is not used.

*Subscribe by web form*
:   To use the web interface visit
    [the list information page](https://lists.gnu.org/mailman/listinfo/help-gawk).
    Use the
    subscribe form to fill out your email address and submit using the
    `Subscribe` button.

*Reply to the confirmation message*
:   In both cases then reply to the confirmation message that is sent to
    your address in reply.

Bob mentions that you may also use email for subscribing and
unsubscribing. For example:

```
$ echo help | mailx -s request help-gawk-request@gnu.org
$ echo subscribe | mailx -s request help-gawk-request@gnu.org
$ echo unsubscribe | mailx -s request help-gawk-request@gnu.org
```

---

Previous: [Where To Send Non-bug Questions](#Asking-for-help), Up: [Reporting Problems and Bugs](#Bugs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.4.6 Reporting Problems with Non-Unix Ports [¶](#Reporting-Problems-with-Non_002dUnix-Ports)

If you find bugs in one of the non-Unix ports of `gawk`,
send an email to the bug list, with a copy to the
person who maintains that port. The maintainers are named in the following list,
as well as in the `README` file in the `gawk` distribution.
Information in the `README` file should be considered authoritative
if it conflicts with this Web page.

The people maintaining the various `gawk` ports are:

|  |  |
| --- | --- |
| Unix and POSIX systems | Arnold Robbins, “arnold at skeeve dot com” |
| MS-Windows with MinGW | Eli Zaretskii, “eliz at gnu dot org” |
| OpenVMS | John Malmberg, “wb8tyw at qsl dot net” |
| z/OS (OS/390) | Daniel Richard G. “skunk at iSKUNK dot ORG” |

If your bug is also reproducible under Unix, send a copy of your
report to the “bug dash gawk at gnu dot org” email list as well.

---

Next: [Summary](#Installation-summary), Previous: [Reporting Problems and Bugs](#Bugs), Up: [Installing `gawk`](#Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### B.5 Other Freely Available `awk` Implementations [¶](#Other-Freely-Available-awk-Implementations)

> *Implementation is the sincerest form of flattery.*

— *L. Peter Deutsch*

> *It’s kind of fun to put comments like this in your awk code:*  
>       `// Do C++ comments work? answer: yes! of course`

— *Michael Brennan*

There are a number of other freely available `awk` implementations.
This section briefly describes where to get them.

The following programs are all written in C:

Unix `awk`
:   Brian Kernighan, one of the original designers of Unix `awk`,
    has made his implementation of
    `awk` freely available.
    You can retrieve it from GitHub:

    ```
    git clone https://github.com/onetrueawk/awk bwkawk
    ```

    This command creates a copy of the [Git](https://git-scm.com)
    repository in a directory named `bwkawk`. If you omit the last argument
    from the `git` command line, the repository copy is created in a
    directory named `awk`.

    To build it, review the settings in the `makefile`, and then just run
    `make`. Note that the result of compilation is named
    `a.out`; you will have to rename it to something reasonable.

    See [Common Extensions Summary](#Common-Extensions)
    for a list of extensions in this `awk` that are not in POSIX `awk`.

    In 2023, Brian Kernighan, along with Al Aho and Peter Weinberger, published
    a second edition of their book on `awk`. Professor Kernighan
    also maintains a [companion web site](https://awk.dev) for the book.
    A copy of all the book’s programs are available there for download.

    As a side note, Dan Bornstein has created a Git repository tracking
    all the versions of BWK `awk` that he could find. It’s
    available at <https://github.com/danfuzz/one-true-awk>.

`mawk`
:   Michael Brennan wrote an independent implementation of `awk`,
    called `mawk`. It is available under the
    GPL (see [GNU General Public License](#Copying)),
    just as `gawk` is.

    The original distribution site for the `mawk` source code
    no longer has it. A copy is available at
    <https://www.skeeve.com/gawk/mawk1.3.3.tar.gz>.

    In 2009, Thomas Dickey took on `mawk` maintenance.
    Basic information is available on
    [the project’s web page](https://www.invisible-island.net/mawk).
    The download URL is
    <https://invisible-island.net/datafiles/release/mawk.tar.gz>.

    Once you have it,
    `gunzip` may be used to decompress this file. Installation
    is similar to `gawk`’s
    (see [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)).

    See [Common Extensions Summary](#Common-Extensions)
    for a list of extensions in `mawk` that are not in POSIX `awk`.

`mawk` 2.0
:   In 2016, Michael Brennan resumed `mawk` development.
    His development snapshots are available via Git from the project’s
    [GitHub page](https://github.com/mikebrennan000/mawk-2).

`awka`
:   Written by Andrew Sumner,
    `awka` translates `awk` programs into C, compiles them,
    and links them with a library of functions that provide the core
    `awk` functionality.
    It also has a number of extensions.

    Both the `awk` translator and the library are released under the GPL.

    To get `awka`, go to <https://sourceforge.net/projects/awka>.

    The project is frozen; no new code changes have been made
    since approximately 2001.

Revive Awka
:   This project, available at <https://github.com/noyesno/awka>,
    intends to fix bugs in `awka` and add more features.

    This project also seems to be frozen; no new code changes have been made
    since approximately 2021.

`pawk`
:   Nelson H.F. Beebe at the University of Utah has modified
    BWK `awk` to provide timing and profiling information.
    It is different from `gawk` with the `--profile` option
    (see [Profiling Your `awk` Programs](#Profiling))
    in that it uses CPU-based profiling, not line-count
    profiling. You may find it at either
    <ftp://ftp.math.utah.edu/pub/pawk/pawk-20030606.tar.gz>
    or
    <https://www.math.utah.edu/pub/pawk/pawk-20030606.tar.gz>.

BusyBox `awk` [¶](#index-BusyBox-Awk)
:   BusyBox is a GPL-licensed program providing small versions of many
    applications within a single executable. It is aimed at embedded systems.
    It includes a full implementation of POSIX `awk`. When building
    it, be careful not to do ‘`make install`’ as it will overwrite
    copies of other applications in your `/usr/local/bin`. For more
    information, see the [project’s home page](https://busybox.net).

The OpenSolaris POSIX `awk`
:   The versions of `awk` in `/usr/xpg4/bin` and
    `/usr/xpg6/bin` on Solaris are more or less POSIX-compliant.
    They are based on the `awk` from Mortice Kern Systems for PCs.
    We were able to make this code compile and work under GNU/Linux
    with 1–2 hours of work. Making it more generally portable (using
    GNU Autoconf and/or Automake) would take more work, and this
    has not been done, at least to our knowledge.

    The source code used to be available from the OpenSolaris website.
    However, that project was ended and the website shut down. Fortunately, the
    [Illumos project](https://wiki.illumos.org/display/illumos/illumos+Home)
    makes this implementation available. You can view the files one at a time from
    <https://github.com/joyent/illumos-joyent/blob/master/usr/src/cmd/awk_xpg4>.

Libmawk [¶](#index-libmawk)
:   This is an embeddable `awk` interpreter derived from
    `mawk`. For more information, see
    <http://repo.hu/projects/libmawk/>.

Mircea Neacsu’s Embeddable `awk` [¶](#index-Neacsu_002c-Mircea)
:   Mircea Neacsu has created an embeddable `awk`
    interpreter, based on BWK awk. It’s available
    at <https://github.com/neacsum/awk>.
    A short overview of how to use it may be found at
    <https://www.codeproject.com/Articles/5264205/Embedding-AWK-in-a-C-program>.

`awkcc` [¶](#index-source-code-awkcc)
:   This is an early adaptation of Unix `awk` that
    translates `awk` into C code. It was done by
    J. Christopher Ramming at Bell Labs, circa 1988.
    It’s available at <https://github.com/nokia/awkcc>.
    Bringing this up to date would be an interesting
    software engineering exercise.

QSE `awk` [¶](#index-QSE-awk)
:   This is an embeddable `awk` interpreter. For more information,
    see <https://code.google.com/p/qse/>. The source can be found at
    <https://code.google.com/archive/p/qse/source/default/source>.

`wak` [¶](#index-Gardner_002c-Ray)
:   `wak` (a permutation of the letters in `awk`) is a small,
    mostly POSIX compatible version of `awk` written in C by
    Ray Gardner.
    The source code is available from [its Github repository](https://github.com/raygard/wak). Some nice articles about its development
    may be found in the author’s [weblog](https://www.raygard.net/awkdoc/).

The following implementations are written in Go:

`goawk`
:   This is an `awk` interpreter written in the
    [Go programming language](https://golang.org/).
    It implements POSIX `awk`, with a few minor extensions.
    Source code is available from <https://github.com/benhoyt/goawk>.
    The author wrote a nice
    [article](https://benhoyt.com/writings/goawk/)
    describing the implementation.

`AWKgo`
:   This is an `awk` to Go translator.
    It was written by the author of `goawk`.
    (See the previous entry in this list.) Source code is
    available from
    <https://github.com/benhoyt/goawk/tree/master/awkgo>.
    The author’s article about it is at
    <https://benhoyt.com/writings/awkgo/>.

The following implementations are written in Java:

`jawk`
:   This is an interpreter for `awk` written in Java. It claims
    to be a full interpreter, although because it uses Java facilities
    for I/O and for regexp matching, the language it supports is different
    from POSIX `awk`. More information is available on the
    [project’s home page](https://jawk.sourceforge.net).

Hoijui’s `jawk`
:   This project, available at <https://github.com/hoijui/Jawk>,
    is another `awk` interpreter written in Java. It uses
    modern Java build tools.

This version is written in Python:

`pawk` [¶](#index-source-code-pawk-_0028Python-version_0029)
:   This is a Python module that claims to bring `awk`-like
    features to Python. See <https://github.com/alecthomas/pawk>
    for more information. (This is not related to Nelson Beebe’s
    modified version of BWK `awk`, described earlier.)

There are several versions written in Rust:

`rust-awk`
:   This program implements a small subset of `awk` in Rust.
    Many features are missing.
    The code is available at
    <https://github.com/wenley/rust-awk>.

`frawk`
:   This is a language for writing short programs. “To a first
    approximation, it is an implementation of the AWK language;
    many common `awk` programs produce equivalent output
    when passed to `frawk`.” However, it has a number of
    important additional features. The code is available at
    <https://github.com/ezrosent/frawk>.

`zawk`
:   This program is based off of `frawk`. The code is available at
    <https://crates.io/crates/zawk>.

`awk-rs`
:   This program simply claims to be “`awk` written in rust.”
    It seems to be frozen, with no updates since 2020.
    The code is available at
    <https://github.com/conradludgate/awk-rs>.

`awk-rs`
:   This program seems to be the most complete and actively
    maintained of the versions written in Rust. It used to be
    named `rawk` but is now called `awk-rs`.
    It seems to be different from the other version with the same
    name. The code is available at
    <https://github.com/pegasusheavy/rawk>.

This version is written in Zig:

`dawk` [¶](#index-dawk)
:   An implementation of `awk` in the Zig programming
    language. See <https://codeberg.org/triallax/dawk>.

And finally, this tool is written in shell:

`cppawk` [¶](#index-cppawk)
:   Quoting from the web page, “`cppawk` is a tiny shell script that is
    used like `awk`. It invokes the C preprocessor (GNU `cpp`) on the Awk
    code and calls Awk on the result.” This program may be of use if
    the way `gawk`’s `@include` facility works doesn’t suit
    your needs. For more information, see <https://www.kylheku.com/cgit/cppawk/>.

See also the “Versions and implementations” section of the
[Wikipedia article](https://en.wikipedia.org/wiki/Awk_language#Versions_and_implementations) on `awk` for information on additional versions.

An interesting collection of library functions is available
at <https://github.com/e36freak/awk-libs>.

An interesting collection of `gawk` extensions is
available <https://github.com/su8/gawk-extensions>.

---

Previous: [Other Freely Available `awk` Implementations](#Other-Versions), Up: [Installing `gawk`](#Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### B.6 Summary [¶](#Summary-18)

* The `gawk` distribution is available from the GNU Project’s main
  distribution site, `ftp.gnu.org`. The canonical build recipe is:

  ```
  wget https://ftp.gnu.org/gnu/gawk/gawk-5.4.0.tar.gz
  tar -xvpzf gawk-5.4.0.tar.gz
  cd gawk-5.4.0
  ./configure && make && make check
  ```

  > **NOTE:** Because of the ‘`https://`’ URL, you may have to supply the
  > `--no-check-certificate` option to `wget` to download
  > the file.
* `gawk` may be built on non-POSIX systems as well. The currently
  supported systems are MS-Windows using
  MSYS, MSYS2, MinGW, and Cygwin,
  and OpenVMS.
  Instructions for each system are included in this appendix.
* Bug reports should be sent via email to “bug dash gawk at gnu dot org”.
  Bug reports should be in English and should include the version of `gawk`,
  how it was compiled, and a short program and data file that demonstrate
  the problem.
* Non-bug emails should be sent to “help dash gawk at gnu dot org”.
  Repeatedly sending non-bug emails to the bug list will get you blacklisted from it.
* There are a number of other freely available `awk`
  implementations. Many are POSIX-compliant; others are less so.

---

Next: [Basic Programming Concepts](#Basic-Concepts), Previous: [Installing `gawk`](#Installation), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix C Implementation Notes [¶](#Implementation-Notes)

This appendix contains information mainly of interest to implementers and
maintainers of `gawk`. Everything in it applies specifically to
`gawk` and not to other implementations.

* [Downward Compatibility and Debugging](#Compatibility-Mode)
* [Making Additions to `gawk`](#Additions)
* [Probable Future Extensions](#Future-Extensions)
* [Some Limitations of the Implementation](#Implementation-Limitations)
* [Extension API Design](#Extension-Design)
* [Summary](#Notes-summary)

---

Next: [Making Additions to `gawk`](#Additions), Up: [Implementation Notes](#Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### C.1 Downward Compatibility and Debugging [¶](#Downward-Compatibility-and-Debugging)

See [Extensions in `gawk` Not in POSIX `awk`](#POSIX_002fGNU),
for a summary of the GNU extensions to the `awk` language and program.
All of these features can be turned off by invoking `gawk` with the
`--traditional` option or with the `--posix` option.

If `gawk` is compiled for debugging with ‘`-DDEBUG`’, then there
is one more option available on the command line:

`-Y`

`--parsedebug`
:   Print out the parse stack information as the program is being parsed.

This option is intended only for serious `gawk` developers
and not for the casual user. It probably has not even been compiled into
your version of `gawk`, since it slows down execution.

---

Next: [Probable Future Extensions](#Future-Extensions), Previous: [Downward Compatibility and Debugging](#Compatibility-Mode), Up: [Implementation Notes](#Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
