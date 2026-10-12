### 14.3 `stat`: Report file or file system status [¶](#stat_003a-Report-file-or-file-system-status)

`stat` displays information about the specified file(s). Synopsis:

```
stat [option]... [file]...
```

With no option, `stat` reports all information about the given files.
But it also can be used to report the information of the file systems the
given files are located on. If the files are links, `stat` can
also give information about the files the links point to.

Due to shell aliases and built-in `stat` functions, using an
unadorned `stat` interactively or in a script may get you
different functionality than that described here. Invoke it via
`env` (i.e., `env stat …`) to avoid interference
from the shell.

‘`-L`’ [¶](#index-_002dL-9)

‘`--dereference`’
:   Change how `stat` treats symbolic links.
    With this option, `stat` acts on the file referenced
    by each symbolic link argument.
    Without it, `stat` acts on any symbolic link argument directly.

‘`-f`’ [¶](#index-_002df-20)

‘`--file-system`’
:   Report information about the file systems where the given files are located
    instead of information about the files themselves.
    This option implies the `-L` option.

‘`--cached=mode`’ [¶](#index-_002d_002dcached_003dmode)
:   Control how attributes are read from the file system;
    if supported by the system. This allows one to
    control the trade-off between freshness and efficiency
    of attribute access, especially useful with remote file systems.
    mode can be:

    ‘`always`’
    :   Always read the already cached attributes if available.

    ‘`never`’
    :   Always synchronize with the latest file system attributes.
        This also mounts automounted files.

    ‘`default`’
    :   Leave the caching behavior to the underlying file system.

‘`-c`’ [¶](#index-_002dc-20)

‘`--format=format`’
:   Use format rather than the default format.
    format is automatically newline-terminated, so
    running a command like the following with two or more file
    operands produces a line of output for each operand:

    ```
    $ stat --format=%d:%i / /usr
    2050:2
    2057:2
    ```

‘`--printf=format`’ [¶](#index-_002d_002dprintf_003dformat)
:   Use format rather than the default format.
    Like `--format`, but interpret backslash escapes,
    and do not output a mandatory trailing newline.
    If you want a newline, include ‘`\n`’ in the format.
    Here’s how you would use `--printf` to print the device
    and inode numbers of `/` and `/usr`:

    ```
    $ stat --printf='%d:%i\n' / /usr
    2050:2
    2057:2
    ```

‘`-t`’ [¶](#index-_002dt-17)

‘`--terse`’
:   Print the information in terse form, suitable for parsing by other programs.

    The output of the following commands are identical and the `--format`
    also identifies the items printed (in fuller form) in the default format.
    The format string would include another ‘`%C`’ at the end with an
    active SELinux security context.

    ```
    $ stat --format="%n %s %b %f %u %g %D %i %h %t %T %X %Y %Z %W %o" ...
    $ stat --terse ...
    ```

    The same illustrating terse output in `--file-system` mode:

    ```
    $ stat -f --format="%n %i %l %t %s %S %b %f %a %c %d" ...
    $ stat -f --terse ...
    ```

The valid format directives for files with `--format` and
`--printf` are:

* %a – Permission bits in octal (see ‘`#`’ and ‘`0`’ printf flags)
* %A – Permission bits in symbolic form (similar to `ls -ld`)
* %b – Number of blocks allocated (see ‘`%B`’)
* %B – The size in bytes of each block reported by ‘`%b`’
* %C – The SELinux security context of a file, if available
* %d – Device number in decimal (st\_dev)
* %D – Device number in hex (st\_dev)
* %Hd – Major device number in decimal
* %Ld – Minor device number in decimal
* %f – Raw mode in hex
* %F – File type
* %g – Group ID of owner
* %G – Group name of owner
* %h – Number of hard links
* %i – Inode number
* %m – Mount point (see selow)
* %n – File name
* %N – Quoted file name with dereference if symbolic link (see below)
* %o – Optimal I/O transfer size hint
* %s – Total size, in bytes
* %r – Device type in decimal (st\_rdev)
* %R – Device type in hex (st\_rdev)
* %Hr – Major device type in decimal (see below)
* %Lr – Minor device type in decimal (see below)
* %t – Major device type in hex (see below)
* %T – Minor device type in hex (see below)
* %u – User ID of owner
* %U – User name of owner
* %w – Time of file birth, or ‘`-`’ if unknown
* %W – Time of file birth as seconds since Epoch, or ‘`0`’
* %x – Time of last access
* %X – Time of last access as seconds since Epoch
* %y – Time of last data modification
* %Y – Time of last data modification as seconds since Epoch
* %z – Time of last status change
* %Z – Time of last status change as seconds since Epoch

The ‘`%a`’ format prints the octal mode, and so it is useful
to control the zero padding of the output with the ‘`#`’ and ‘`0`’
printf flags. For example to pad to at least 3 wide while making larger
numbers unambiguously octal, you can use ‘`%#03a`’.

The ‘`%N`’ format can be set with the environment variable
`QUOTING_STYLE`. If that environment variable is not set,
the default value is ‘`shell-escape-always`’. Valid quoting styles are:

‘`literal`’
:   Output strings as-is; this is the same as the `--literal` (`-N`)
    option.

‘`shell`’
:   Quote strings for the shell if they contain shell metacharacters or would
    cause ambiguous output.
    The quoting is suitable for POSIX-compatible shells like
    `bash`, but it does not always work for incompatible shells
    like `csh`.

‘`shell-always`’
:   Quote strings for the shell, even if they would normally not require quoting.

‘`shell-escape`’
:   Like ‘`shell`’, but also quoting non-printable characters using the POSIX
    ‘`$''`’ syntax suitable for most shells.

‘`shell-escape-always`’
:   Like ‘`shell-escape`’, but quote strings even if they would
    normally not require quoting.

‘`c`’
:   Quote strings as for C character string literals, including the
    surrounding double-quote characters; this is the same as the
    `--quote-name` (`-Q`) option.

‘`c-maybe`’
:   Quote strings as for C character string literals, except omit the
    surrounding double-quote if no escaping is required.

‘`escape`’
:   Quote strings as for C character string literals, except omit the
    surrounding double-quote
    characters; this is the same as the `--escape` (`-b`) option.

‘`clocale`’
:   Quote strings as for C character string literals, except use
    surrounding quotation marks appropriate for the
    locale.

‘`locale`’
:   Quote strings as for C character string literals, except use
    surrounding quotation marks appropriate for the locale, and quote
    `'like this'` instead of `"like
    this"` in the default C locale. This looks nicer on many displays.

The ‘`r`’, ‘`R`’, ‘`%t`’, and ‘`%T`’ formats operate on the st\_rdev
member of the stat(2) structure, i.e., the represented device rather than
the containing device, and so are only defined for character and block
special files. On some systems or file types, st\_rdev may be used to
represent other quantities.

The ‘`%W`’, ‘`%X`’, ‘`%Y`’, and ‘`%Z`’ formats accept a
precision preceded by a period to specify the number of digits to
print after the decimal point. For example, ‘`%.3X`’ outputs the
access timestamp to millisecond precision. If a period is given but no
precision, `stat` uses 9 digits, so ‘`%.X`’ is equivalent to
‘`%.9X`’. When discarding excess precision, timestamps are truncated
toward minus infinity.

```
zero pad:
  $ stat -c '[%015Y]' /usr
  [000001288929712]
space align:
  $ stat -c '[%15Y]' /usr
  [     1288929712]
  $ stat -c '[%-15Y]' /usr
  [1288929712     ]
precision:
  $ stat -c '[%.3Y]' /usr
  [1288929712.114]
  $ stat -c '[%.Y]' /usr
  [1288929712.114951834]
```

The mount point printed by ‘`%m`’ is similar to that output
by `df`, except that:

* stat does not dereference symlinks by default
  (unless `-L` is specified)
* stat does not search for specified device nodes in the
  file system list, instead operating on them directly
* stat outputs the alias for a bind mounted file, rather than
  the initial mount point of its backing device.
  One can recursively call stat until there is no change in output,
  to get the current base mount point

When listing file system information (`--file-system` (`-f`)),
you must use a different set of format directives:

* %a – Free blocks available to non-super-user
* %b – Total data blocks in file system
* %c – Total file nodes in file system
* %d – Free file nodes in file system
* %f – Free blocks in file system
* %i – File System ID in hex
* %l – Maximum length of file names
* %n – File name
* %s – Block size (for faster transfers)
* %S – Fundamental block size (for block counts)
* %t – Type in hex
* %T – Type in human readable form

Timestamps are listed according to the time zone rules specified by
the `TZ` environment variable, or by the system default rules if
`TZ` is not set. See [Specifying the Time Zone
with `TZ`](https://www.gnu.org/software/libc/manual/html_mono/libc.html#TZ-Variable) in The GNU C Library Reference Manual.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`truncate`: Shrink or extend the size of a file](#truncate-invocation), Previous: [`stat`: Report file or file system status](#stat-invocation), Up: [File space usage](#File-space-usage)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 14.4 `sync`: Synchronize cached writes to persistent storage [¶](#sync_003a-Synchronize-cached-writes-to-persistent-storage)

`sync` synchronizes in memory files or file systems to persistent
storage. Synopsis:

```
sync [option] [file]...
```

`sync` writes any data buffered in memory out to the storage device.
This can
include (but is not limited to) modified superblocks, modified inodes,
and delayed reads and writes. This must be implemented by the kernel;
The `sync` program does nothing but exercise the `sync`,
`syncfs`, `fsync`, and `fdatasync` system calls.

The kernel keeps data in memory to avoid doing (relatively slow) device
reads and writes. This improves performance, but if the computer
crashes, data may be lost or the file system corrupted as a
result. The `sync` command instructs the kernel to write
data in memory to persistent storage.

If any argument is specified then only those files will be
synchronized using the fsync(2) syscall by default.

If at least one file is specified, it is possible to change the
synchronization method with the following options. Also see
[Common options](#Common-options).

‘`-d`’ [¶](#index-_002d_002ddata)

‘`--data`’
:   Use fdatasync(2) to sync only the data for the file,
    and any metadata required to maintain file system consistency.

‘`-f`’ [¶](#index-_002d_002dfile_002dsystem-1)

‘`--file-system`’
:   Synchronize all the I/O waiting for the file systems that contain the file,
    using the syscall syncfs(2). You would usually *not* specify
    this option if passing a device node like ‘`/dev/sda`’ for example,
    as that would sync the containing file system rather than the referenced one.
    Depending on the system, passing individual device nodes or files
    may have different sync characteristics than using no arguments.
    I.e., arguments passed to fsync(2) may provide greater guarantees through
    write barriers, than a global sync(2) used when no arguments are provided.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`sync`: Synchronize cached writes to persistent storage](#sync-invocation), Up: [File space usage](#File-space-usage)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 14.5 `truncate`: Shrink or extend the size of a file [¶](#truncate_003a-Shrink-or-extend-the-size-of-a-file)

`truncate` shrinks or extends the size of each file to the
specified size. Synopsis:

```
truncate option... file...
```

Any file that does not exist is created.

If a file is larger than the specified size, the extra data is lost.
If a file is shorter, it is extended and the sparse extended part
(or hole) reads as zero bytes.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-c`’ [¶](#index-_002dc-21)

‘`--no-create`’
:   Do not create files that do not exist.

‘`-o`’ [¶](#index-_002do-7)

‘`--io-blocks`’
:   Treat size as number of I/O blocks of the file rather than bytes.

‘`-r rfile`’ [¶](#index-_002dr-11)

‘`--reference=rfile`’
:   Base the size of each file on the size of rfile.

‘`-s size`’ [¶](#index-_002ds-21)

‘`--size=size`’
:   Set or adjust the size of each file according to size.
    size is in bytes unless `--io-blocks` is specified.
    size may be, or may be an integer optionally followed by,
    one of the following multiplicative suffixes:

    ```
    ‘KB’ =>           1000 (KiloBytes)
    ‘K’  =>           1024 (KibiBytes)
    ‘MB’ =>      1000*1000 (MegaBytes)
    ‘M’  =>      1024*1024 (MebiBytes)
    ‘GB’ => 1000*1000*1000 (GigaBytes)
    ‘G’  => 1024*1024*1024 (GibiBytes)
    ```

    and so on for ‘`T`’, ‘`P`’, ‘`E`’, ‘`Z`’, ‘`Y`’,
    ‘`R`’, and ‘`Q`’.
    Binary prefixes can be used, too: ‘`KiB`’=‘`K`’, ‘`MiB`’=‘`M`’,
    and so on.

    size may also be prefixed by one of the following to adjust
    the size of each file based on its current size:

    ```
    ‘+’  => extend by
    ‘-’  => reduce by
    ‘<’  => at most
    ‘>’  => at least
    ‘/’  => round down to multiple of
    ‘%’  => round up to multiple of
    ```

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [Conditions](#Conditions), Previous: [File space usage](#File-space-usage), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 15 Printing text [¶](#Printing-text-1)

This section describes commands that display text strings.

* [`echo`: Print a line of text](#echo-invocation)
* [`printf`: Format and print data](#printf-invocation)
* [`yes`: Print a string until interrupted](#yes-invocation)

---

Next: [`printf`: Format and print data](#printf-invocation), Up: [Printing text](#Printing-text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 15.1 `echo`: Print a line of text [¶](#echo_003a-Print-a-line-of-text)

`echo` writes each given string to standard output, with a
space between each and a newline after the last one. Synopsis:

```
echo [option]... [string]...
```

Due to shell aliases and built-in `echo` functions, using an
unadorned `echo` interactively or in a script may get you
different functionality than that described here. Invoke it via
`env` (i.e., `env echo …`) to avoid interference
from the shell.

Due to historical and backwards compatibility reasons, certain bare option-like
strings cannot be passed to `echo` as non-option arguments.
It is therefore not advisable to use `echo` for printing unknown or
variable arguments. The `printf` command is recommended as a more
portable and flexible replacement for tasks historically performed by
`echo`. See [`printf`: Format and print data](#printf-invocation).

The program accepts the following options. Also see [Common options](#Common-options).
Options must precede operands, and the normally-special argument
‘`--`’ has no special meaning and is treated like any other
string.

‘`-n`’ [¶](#index-_002dn-15)
:   Do not output the trailing newline.

‘`-e`’ [¶](#index-_002de-5)
:   Enable interpretation of the following backslash-escaped characters in
    each string:

    ‘`\a`’
    :   alert (bell)

    ‘`\b`’
    :   backspace

    ‘`\c`’
    :   produce no further output

    ‘`\e`’
    :   escape

    ‘`\f`’
    :   form feed

    ‘`\n`’
    :   newline

    ‘`\r`’
    :   carriage return

    ‘`\t`’
    :   horizontal tab

    ‘`\v`’
    :   vertical tab

    ‘`\\`’
    :   backslash

    ‘`\0nnn`’
    :   the eight-bit value that is the octal number nnn
        (zero to three octal digits), if nnn is
        a nine-bit value, the ninth bit is ignored

    ‘`\nnn`’
    :   the eight-bit value that is the octal number nnn
        (one to three octal digits), if nnn is
        a nine-bit value, the ninth bit is ignored

    ‘`\xhh`’
    :   the eight-bit value that is the hexadecimal number hh
        (one or two hexadecimal digits)

‘`-E`’ [¶](#index-_002dE-1)
:   Disable interpretation of backslash escapes in each string.
    This is the default. If `-e` and `-E` are both
    specified, the last one given takes effect.

If the `POSIXLY_CORRECT` environment variable is set, then when
`echo`’s first argument is not `-n` it outputs
option-like arguments instead of treating them as options. For
example, `echo -ne hello` outputs ‘`-ne hello`’ instead of
plain ‘`hello`’. Also backslash escapes are always enabled.
To echo the string ‘`-n`’, one of the characters
can be escaped in either octal or hexadecimal representation.
For example, `echo -e '\x2dn'`.

POSIX does not require support for any options, and says
that the behavior of `echo` is implementation-defined if any
string contains a backslash or if the first argument is `-n`.
Portable programs should use the `printf` command instead.
See [`printf`: Format and print data](#printf-invocation).

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`yes`: Print a string until interrupted](#yes-invocation), Previous: [`echo`: Print a line of text](#echo-invocation), Up: [Printing text](#Printing-text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 15.2 `printf`: Format and print data [¶](#printf_003a-Format-and-print-data)

`printf` does formatted printing of text. Synopsis:

```
printf format [argument]...
```

`printf` prints the format string, interpreting ‘`%`’
directives and ‘`\`’ escapes to format numeric and string arguments
in a way that is mostly similar to the C ‘`printf`’ function,
and C language escape sequence processing.
See [`printf` format directives](https://www.gnu.org/software/libc/manual/html_mono/libc.html#Output-Conversion-Syntax) in The GNU C Library Reference Manual, for details.
See also [C99 string escapes:](https://en.cppreference.com/w/c/language/escape). The differences are listed below.

Due to shell aliases and built-in `printf` functions, using an
unadorned `printf` interactively or in a script may get you
different functionality than that described here. Invoke it via
`env` (i.e., `env printf …`) to avoid interference
from the shell.

* The format argument is reused as necessary to convert all the
  given arguments. For example, the command ‘`printf %s a b`’
  outputs ‘`ab`’.
* Missing arguments are treated as null strings or as zeros,
  depending on whether the context expects a string or a number. For
  example, the command ‘`printf %sx%d`’ prints ‘`x0`’.
* Indexed arguments referenced with ‘`%...$`’ formats, can be
  mixed with standard sequential argument references,
  in which case both index types are independent.
  For example, the command ‘`printf '%1$s%s' A`’ prints ‘`AA`’.
  Also the highest referenced argument, either indexed or sequential,
  is considered to be the last one referenced, when determining
  unused arguments for reprocessing with the format argument.
* An additional escape, ‘`\c`’, causes `printf` to produce no
  further output. For example, the command ‘`printf 'A%sC\cD%sF' B
  E`’ prints ‘`ABC`’.
* The hexadecimal escape sequence ‘`\xhh`’ has at most two
  digits, as opposed to C where it can have an unlimited number of
  digits. For example, the command ‘`printf '\x07e'`’ prints two
  bytes, whereas the C statement ‘`printf ("\x07e")`’ prints just
  one.
* An additional directive ‘`%b`’, prints its
  argument string with ‘`\`’ escapes interpreted in the same way as in
  the format string, except that octal escapes should have a leading
  ‘`0`’ like ‘`\0nnn`’.
  If ‘`nnn`’ is a nine-bit value, ignore the ninth bit.
  If a precision is also given, it limits the number of bytes printed
  from the converted string.
* An additional directive ‘`%q`’, prints its argument string
  in a format that can be reused as input by most shells.
  Non-printable characters are escaped with the POSIX ‘`$''`’ syntax,
  and shell metacharacters are quoted appropriately.
  This is an equivalent format to `ls --quoting=shell-escape` output.
* Numeric arguments must be single C constants, possibly with leading
  ‘`+`’ or ‘`-`’. For example, ‘`printf %.4d -3`’ outputs
  ‘`-0003`’.
* If the leading character of a numeric argument is ‘`"`’ or ‘`'`’
  then its value is the numeric value of the immediately following
  character. Any remaining characters are silently ignored if the
  `POSIXLY_CORRECT` environment variable is set; otherwise, a
  warning is printed. For example, ‘`printf "%d" "'a"`’ outputs
  ‘`97`’ on hosts that use the ASCII character set, since
  ‘`a`’ has the numeric value 97 in ASCII.

A floating point argument is interpreted according to
the `LC_NUMERIC` category of either the current or the C locale,
and is printed according to the current locale.
For example, in a locale whose decimal point character is a comma,
the command ‘`printf '%g %g' 2,5 2.5`’ outputs ‘`2,5 2,5`’.
See [Floating point numbers](#Floating-point).

`printf` interprets ‘`\ooo`’ in format as an octal number
(if ooo is 1 to 3 octal digits) specifying a byte to print,
and ‘`\xhh`’ as a hexadecimal number (if hh is 1 to 2 hex
digits) specifying a character to print.
However, when ‘`\ooo`’ specifies a number larger than 255,
`printf` ignores the ninth bit.
For example, ‘`printf '\400'`’ is equivalent to ‘`printf '\0'`’.

`printf` interprets two syntax forms for specifying Unicode
(ISO/IEC 10646) characters.
‘`\u`’ for 16-bit Unicode characters, specified as
four hexadecimal digits hhhh, and ‘`\U`’ for 32-bit Unicode
characters, specified as eight hexadecimal digits hhhhhhhh.
`printf` outputs the Unicode characters
according to the `LC_CTYPE` locale. Unicode characters in the range
U+D800…U+DFFF cannot be specified by this syntax.
This syntax fully supports the universal character subset
introduced in ISO C 99.

The processing of ‘`\u`’ and ‘`\U`’ requires a full-featured
`iconv` facility. It is activated on systems with glibc 2.2 (or newer),
or when `libiconv` is installed prior to this package. Otherwise
‘`\u`’ and ‘`\U`’ will print as-is.

Unicode character syntax is useful for writing strings in a locale
independent way. For example, a string containing the Euro currency symbol

```
$ env printf '\u20AC 14.95'
```

will be output correctly in all locales supporting the Euro symbol
(ISO-8859-15, UTF-8, and others). Similarly, a Chinese string

```
$ env printf '\u4e2d\u6587'
```

will be output correctly in all Chinese locales (GB2312, BIG5, UTF-8, etc).

In these examples, the `printf` command was
invoked via `env` to ensure that we run the program found via
your shell’s search path, and not a shell alias or a built-in function.

For larger strings, you don’t need to look up the hexadecimal code
values of each character one by one. ASCII characters mixed with \u
escape sequences is also known as the JAVA source file encoding. You can
use GNU recode 3.5c (or newer) to convert strings to this encoding. Here
is how to convert a piece of text into a shell script which will output
this text in a locale-independent way:

```
$ LC_CTYPE=zh_TW.big5 env printf \
    '\u4e2d\u6587\n' > sample.txt
$ recode BIG5..JAVA < sample.txt \
    | sed -e "s|^|env printf '|" -e "s|%|%%|g" -e "s|$|\\\\n'|" \
    > sample.sh
```

The only options are a lone `--help` or
`--version`. See [Common options](#Common-options).
Options must precede operands.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`printf`: Format and print data](#printf-invocation), Up: [Printing text](#Printing-text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 15.3 `yes`: Print a string until interrupted [¶](#yes_003a-Print-a-string-until-interrupted)

`yes` prints the command line arguments, separated by spaces and
followed by a newline, forever until it is killed. If no arguments are
given, it prints ‘`y`’ followed by a newline forever until killed.

Upon a write error, `yes` exits with status ‘`1`’.

The only options are a lone `--help` or `--version`.
To output an argument that begins with
‘`-`’, precede it with `--`, e.g., ‘`yes -- --help`’.
See [Common options](#Common-options).

---

Next: [Redirection](#Redirection), Previous: [Printing text](#Printing-text), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 16 Conditions [¶](#Conditions-1)

This section describes commands that are primarily useful for their exit
status, rather than their output. Thus, they are often used as the
condition of shell `if` statements, or as the last command in a
pipeline.

* [`false`: Do nothing, unsuccessfully](#false-invocation)
* [`true`: Do nothing, successfully](#true-invocation)
* [`test`: Check file types and compare values](#test-invocation)
* [`expr`: Evaluate expressions](#expr-invocation)

---

Next: [`true`: Do nothing, successfully](#true-invocation), Up: [Conditions](#Conditions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 16.1 `false`: Do nothing, unsuccessfully [¶](#false_003a-Do-nothing_002c-unsuccessfully)

`false` does nothing except return an exit status of 1, meaning
*failure*. It can be used as a place holder in shell scripts
where an unsuccessful command is needed.
In most modern shells, `false` is a built-in command, so when
you use ‘`false`’ in a script, you’re probably using the built-in
command, not the one documented here.

`false` honors the `--help` and `--version` options.

This version of `false` is implemented as a C program, and is thus
more secure and faster than a shell script implementation, and may safely
be used as a dummy shell for the purpose of disabling accounts.

Unlike all other programs mentioned in this manual, `false`
always exits unsuccessfully, even when invoked with
`--help` or `--version`.

Portable programs should not assume that the exit status of
`false` is 1, as it is greater than 1 on some
non-GNU hosts.

---

Next: [`test`: Check file types and compare values](#test-invocation), Previous: [`false`: Do nothing, unsuccessfully](#false-invocation), Up: [Conditions](#Conditions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 16.2 `true`: Do nothing, successfully [¶](#true_003a-Do-nothing_002c-successfully)

`true` does nothing except return an exit status of 0, meaning
*success*. It can be used as a place holder in shell scripts
where a successful command is needed, although the shell built-in
command `:` (colon) may do the same thing faster.
In most modern shells, `true` is a built-in command, so when
you use ‘`true`’ in a script, you’re probably using the built-in
command, not the one documented here.

`true` honors the `--help` and `--version` options.

However, it is possible to cause `true`
to exit with nonzero status: with the `--help` or `--version`
option, and with standard
output already closed or redirected to a file that evokes an I/O error.
For example, using a Bourne-compatible shell:

```
$ ./true --version >&-
./true: write error: Bad file number
$ ./true --version > /dev/full
./true: write error: No space left on device
```

This version of `true` is implemented as a C program, and is thus
more secure and faster than a shell script implementation, and may safely
be used as a dummy shell for the purpose of disabling accounts.

---

Next: [`expr`: Evaluate expressions](#expr-invocation), Previous: [`true`: Do nothing, successfully](#true-invocation), Up: [Conditions](#Conditions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 16.3 `test`: Check file types and compare values [¶](#test_003a-Check-file-types-and-compare-values)

`test` returns a status of 0 (true) or 1 (false) depending on the
evaluation of the conditional expression expr. Each part of the
expression must be a separate argument.

`test` has file status checks, string operators, and numeric
comparison operators.

`test` has an alternate form that uses opening and closing
square brackets instead a leading ‘`test`’. For example, instead
of ‘`test -d /`’, you can write ‘`[ -d / ]`’. The square
brackets must be separate arguments; for example, ‘`[-d /]`’ does
not have the desired effect. Since ‘`test expr`’ and ‘`[
expr ]`’ have the same meaning, only the former form is discussed
below.

Synopses:

```
test expression
test
[ expression ]
[ ]
[ option
```

Due to shell aliases and built-in `test` functions, using an
unadorned `test` interactively or in a script may get you
different functionality than that described here. Invoke it via
`env` (i.e., `env test …`) to avoid interference
from the shell.

If expression is omitted, `test` returns false.
If expression is a single argument,
`test` returns false if the argument is null and true
otherwise. The argument
can be any string, including strings like ‘`-d`’, ‘`-1`’,
‘`--`’, ‘`--help`’, and ‘`--version`’ that most other
programs would treat as options. To get help and version information,
invoke the commands ‘`[ --help`’ and ‘`[ --version`’, without
the usual closing brackets. See [Common options](#Common-options).

Exit status:

```
0 if the expression is true,
1 if the expression is false,
2 if an error occurred.
```

* [File type tests](#File-type-tests)
* [Access permission tests](#Access-permission-tests)
* [File characteristic tests](#File-characteristic-tests)
* [String tests](#String-tests)
* [Numeric tests](#Numeric-tests)
* [Connectives for `test`](#Connectives-for-test)

---

Next: [Access permission tests](#Access-permission-tests), Up: [`test`: Check file types and compare values](#test-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.3.1 File type tests [¶](#File-type-tests-1)

These options test for particular types of files. (Everything’s a file,
but not all files are the same!)

‘`-b file`’ [¶](#index-_002db-19)
:   True if file exists and is a block special device.

‘`-c file`’ [¶](#index-_002dc-22)
:   True if file exists and is a character special device.

‘`-d file`’ [¶](#index-_002dd-16)
:   True if file exists and is a directory.

‘`-f file`’ [¶](#index-_002df-21)
:   True if file exists and is a regular file.

‘`-h file`’ [¶](#index-_002dL-10)

‘`-L file`’
:   True if file exists and is a symbolic link.
    Unlike all other file-related tests, this test does not dereference
    file if it is a symbolic link.

‘`-p file`’ [¶](#index-_002dp-7)
:   True if file exists and is a named pipe.

‘`-S file`’ [¶](#index-_002dS-11)
:   True if file exists and is a socket.

‘`-t fd`’ [¶](#index-_002dt-18)
:   True if fd is a file descriptor that is associated with a
    terminal.

---

Next: [File characteristic tests](#File-characteristic-tests), Previous: [File type tests](#File-type-tests), Up: [`test`: Check file types and compare values](#test-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.3.2 Access permission tests [¶](#Access-permission-tests-1)

These options test for particular access permissions.

‘`-g file`’ [¶](#index-_002dg-5)
:   True if file exists and has its set-group-ID bit set.

‘`-k file`’ [¶](#index-_002dk-6)
:   True if file exists and has its *sticky* bit set.

‘`-r file`’ [¶](#index-_002dr-12)
:   True if file exists and the user has read access.

‘`-u file`’ [¶](#index-_002du-9)
:   True if file exists and has its set-user-ID bit set.

‘`-w file`’ [¶](#index-_002dw-11)
:   True if file exists and the user has write access.

‘`-x file`’ [¶](#index-_002dx-7)
:   True if file exists and the user has execute access
    (or search permission, if it is a directory).

‘`-O file`’ [¶](#index-_002dO-1)
:   True if file exists and is owned by the current effective user ID.

‘`-G file`’ [¶](#index-_002dG-1)
:   True if file exists and is owned by the current effective group ID.

---

Next: [String tests](#String-tests), Previous: [Access permission tests](#Access-permission-tests), Up: [`test`: Check file types and compare values](#test-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.3.3 File characteristic tests [¶](#File-characteristic-tests-1)

These options test other file characteristics.

‘`-e file`’ [¶](#index-_002de-6)
:   True if file exists.

‘`-s file`’ [¶](#index-_002ds-22)
:   True if file exists and has a size greater than zero.

‘`file1 -nt file2`’ [¶](#index-_002dnt)
:   True if file1 is newer (according to modification date) than
    file2, or if file1 exists and file2 does not.

‘`file1 -ot file2`’ [¶](#index-_002dot)
:   True if file1 is older (according to modification date) than
    file2, or if file2 exists and file1 does not.

‘`file1 -ef file2`’ [¶](#index-_002def)
:   True if file1 and file2 have the same device and inode
    numbers, i.e., if they are hard links to each other.

‘`-N file`’ [¶](#index-_002dN-3)
:   True if file exists and has been modified (mtime) since it was
    last read (atime).

---

Next: [Numeric tests](#Numeric-tests), Previous: [File characteristic tests](#File-characteristic-tests), Up: [`test`: Check file types and compare values](#test-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.3.4 String tests [¶](#String-tests-1)

These options test string characteristics. You may need to quote
string arguments for the shell. For example:

```
test -n "$V"
```

The quotes here prevent the wrong arguments from being passed to
`test` if ‘`$V`’ is empty or contains special characters.

‘`-z string`’ [¶](#index-_002dz-13)
:   True if the length of string is zero.

‘`-n string`’ [¶](#index-_002dn-16)

‘`string`’
:   True if the length of string is nonzero.

‘`string1 = string2`’ [¶](#index-_003d-1)
:   True if the strings are equal.

‘`string1 == string2`’ [¶](#index-_003d_003d-1)
:   True if the strings are equal (synonym for ‘`=`’).
    This form is not as portable to other
    shells and systems.

‘`string1 != string2`’ [¶](#index-_0021_003d)
:   True if the strings are not equal.

‘`string1 > string2`’ [¶](#index-_003e-1)
:   True if string1 is greater than string2 in the current locale.

‘`string1 < string2`’ [¶](#index-_003c-1)
:   True if string1 is less than string2 in the current locale.

---

Next: [Connectives for `test`](#Connectives-for-test), Previous: [String tests](#String-tests), Up: [`test`: Check file types and compare values](#test-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.3.5 Numeric tests [¶](#Numeric-tests-1)

Numeric relational operators. The arguments must be entirely numeric
(possibly negative), or the special expression `-l string`,
which evaluates to the length of string.

‘`arg1 -eq arg2`’ [¶](#index-_002deq)

‘`arg1 -ne arg2`’

‘`arg1 -lt arg2`’

‘`arg1 -le arg2`’

‘`arg1 -gt arg2`’

‘`arg1 -ge arg2`’
:   These arithmetic binary operators return true if arg1 is equal,
    not-equal, less-than, less-than-or-equal, greater-than, or
    greater-than-or-equal than arg2, respectively.

For example:

```
test -1 -gt -2 && echo yes
⇒ yes
test -l abc -gt 1 && echo yes
⇒ yes
test 0x100 -eq 1
error→ test: integer expression expected before -eq
```

---

Previous: [Numeric tests](#Numeric-tests), Up: [`test`: Check file types and compare values](#test-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.3.6 Connectives for `test` [¶](#Connectives-for-test-1)

It is better to use shell logical primitives
rather than these logical connectives internal to `test`,
because an expression may become ambiguous
depending on the expansion of its parameters.

For example, this becomes ambiguous when ‘`$1`’
is set to ‘`'!'`’ and ‘`$2`’ to the empty string ‘`''`’:

```
test "$1" -a "$2"
```

and should be written as:

```
test "$1" && test "$2"
```

The shell logical primitives also benefit from
short circuit operation, which can be significant
for file attribute tests.

‘`! expr`’ [¶](#index-_0021)
:   True if expr is false.
    ‘`!`’ has lower precedence than all parts of expr.
    The ‘`!`’ should be specified to the left
    of a binary expression, I.e., ‘`! 1 -gt 2`’
    rather than ‘`1 ! -gt 2`’.

‘`expr1 -a expr2`’ [¶](#index-_002da-11)
:   True if both expr1 and expr2 are true.
    ‘`-a`’ is left associative,
    and has a higher precedence than ‘`-o`’.

‘`expr1 -o expr2`’ [¶](#index-_002do-8)
:   True if either expr1 or expr2 is true.
    ‘`-o`’ is left associative.

---

Previous: [`test`: Check file types and compare values](#test-invocation), Up: [Conditions](#Conditions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 16.4 `expr`: Evaluate expressions [¶](#expr_003a-Evaluate-expressions)

`expr` evaluates an expression and writes the result on standard
output. Each token of the expression must be a separate argument.

Operands are either integers or strings. Integers consist of one or
more decimal digits, with an optional leading ‘`-`’.
`expr` converts
anything appearing in an operand position to an integer or a string
depending on the operation being applied to it.

Strings are not quoted for `expr` itself, though you may need to
quote them to protect characters with special meaning to the shell,
e.g., spaces. However, regardless of whether it is quoted, a string
operand should not be a parenthesis or any of `expr`’s
operators like `+`, so you cannot safely pass an arbitrary string
`$str` to expr merely by quoting it to the shell. One way to
work around this is to use the GNU extension `+`,
(e.g., `+ "$str" = foo`); a more portable way is to use
`" $str"` and to adjust the rest of the expression to take
the leading space into account (e.g., `" $str" = " foo"`).

You should not pass a negative integer or a string with leading
‘`-`’ as `expr`’s first argument, as it might be
misinterpreted as an option; this can be avoided by parenthesization.
Also, portable scripts should not use a string operand that happens to
take the form of an integer; this can be worked around by inserting
leading spaces as mentioned above.

Operators may be given as infix symbols or prefix keywords. Parentheses
may be used for grouping in the usual manner. You must quote
parentheses and many operators to avoid the shell evaluating them,
however.

Because `expr` uses multiple-precision arithmetic, it works
with integers wider than those of machine registers.

The only options are `--help` and `--version`. See [Common options](#Common-options). Options must precede operands.

Exit status:

```
0 if the expression is neither null nor 0,
1 if the expression is null or 0,
2 if the expression is invalid,
3 if an internal error occurred (e.g., arithmetic overflow).
```

* [String expressions](#String-expressions)
* [Numeric expressions](#Numeric-expressions)
* [Relations for `expr`](#Relations-for-expr)
* [Examples of using `expr`](#Examples-of-expr)

---

Next: [Numeric expressions](#Numeric-expressions), Up: [`expr`: Evaluate expressions](#expr-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.4.1 String expressions [¶](#String-expressions-1)

`expr` supports pattern matching and other string operators. These
have higher precedence than both the numeric and relational operators (in
the next sections).

‘`string : regex`’ [¶](#index-pattern-matching)
:   Perform pattern matching. The arguments are converted to strings and the
    second is considered to be a (basic, a la GNU `grep`) regular
    expression, with a `^` implicitly prepended. The first argument is
    then matched against this regular expression.

    If regex does not use ‘`\(`’ and ‘`\)`’, the `:`
    expression returns the number of characters matched, or 0 if the match
    fails.

    If regex uses ‘`\(`’ and ‘`\)`’, the `:` expression
    returns the part of string that matched the subexpression, or
    the null string if the match failed or the subexpression did not
    contribute to the match.

    Only the first ‘`\( … \)`’ pair is relevant to the return
    value; additional pairs are meaningful only for grouping the regular
    expression operators.

    In the regular expression, `\+`, `\?`, and `\|` are
    operators which respectively match one or more, zero or one, or separate
    alternatives. These operators are GNU extensions. See [Regular Expressions](https://www.gnu.org/software/grep/manual/grep.html#Regular-Expressions) in The GNU Grep Manual, for details of
    regular expression syntax. Some examples are in [Examples of using `expr`](#Examples-of-expr).

‘`match string regex`’ [¶](#index-match)
:   An alternative way to do pattern matching. This is the same as
    ‘`string : regex`’.

‘`substr string position length`’ [¶](#index-substr)
:   Returns the substring of string beginning at position
    with length at most length. If either position or
    length is negative, zero, or non-numeric, returns the null string.

‘`index string charset`’ [¶](#index-index)
:   Returns the first position in string where the first character in
    charset was found. If no character in charset is found in
    string, return 0.

‘`length string`’ [¶](#index-length)
:   Returns the length of string.

‘`+ token`’ [¶](#index-_002b)
:   Interpret token as a string, even if it is a keyword like match
    or an operator like `/`.
    This makes it possible to test `expr length + "$x"` or
    `expr + "$x" : '.*/\(.\)'` and have it do the right thing even if
    the value of $x happens to be (for example) `/` or `index`.
    This operator is a GNU extension. Portable shell scripts should use
    `" $token" : ' \(.*\)'` instead of `+ "$token"`.

To make `expr` interpret keywords as strings, you must use the
`quote` operator.

---

Next: [Relations for `expr`](#Relations-for-expr), Previous: [String expressions](#String-expressions), Up: [`expr`: Evaluate expressions](#expr-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.4.2 Numeric expressions [¶](#Numeric-expressions-1)

`expr` supports the usual numeric operators, in order of increasing
precedence. These numeric operators have lower precedence than the
string operators described in the previous section, and higher precedence
than the connectives (next section).

‘`+ -`’ [¶](#index-_002b-1)
:   Addition and subtraction. Both arguments are converted to integers;
    an error occurs if this cannot be done.

‘`* / %`’ [¶](#index-_002a)
:   Multiplication, division, remainder. Both arguments are converted to
    integers; an error occurs if this cannot be done.

---

Next: [Examples of using `expr`](#Examples-of-expr), Previous: [Numeric expressions](#Numeric-expressions), Up: [`expr`: Evaluate expressions](#expr-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.4.3 Relations for `expr` [¶](#Relations-for-expr-1)

`expr` supports the usual logical connectives and relations. These
have lower precedence than the string and numeric operators
(previous sections). Here is the list, lowest-precedence operator first.

‘`|`’ [¶](#index-_007c)
:   Returns its first argument if that is neither null nor zero, otherwise
    its second argument if it is neither null nor zero, otherwise 0. It
    does not evaluate its second argument if its first argument is neither
    null nor zero.

‘`&`’ [¶](#index-_0026)
:   Return its first argument if neither argument is null or zero, otherwise
    0. It does not evaluate its second argument if its first argument is
    null or zero.

‘`< <= = == != >= >`’ [¶](#index-_003c)
:   Compare the arguments and return 1 if the relation is true, 0 otherwise.
    `==` is a synonym for `=`. `expr` first tries to convert
    both arguments to integers and do a numeric comparison; if either
    conversion fails, it does a lexicographic comparison using the character
    collating sequence specified by the `LC_COLLATE` locale.

---

Previous: [Relations for `expr`](#Relations-for-expr), Up: [`expr`: Evaluate expressions](#expr-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 16.4.4 Examples of using `expr` [¶](#Examples-of-using-expr)

Here are a few examples, including quoting for shell metacharacters.

To add 1 to the shell variable `foo`, in Bourne-compatible shells:

```
foo=$(expr $foo + 1)
```

To print the non-directory part of the file name stored in
`$fname`, which need not contain a `/`:

```
expr $fname : '.*/\(.*\)' '|' $fname
```

An example showing that `\+` is an operator:

```
expr aaa : 'a\+'
⇒ 3
```

```
expr abc : 'a\(.\)c'
⇒ b
expr index abcdef cz
⇒ 3
expr index index a
error→ expr: syntax error
expr index + index a
⇒ 0
```

---

Next: [File name manipulation](#File-name-manipulation), Previous: [Conditions](#Conditions), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 17 Redirection [¶](#Redirection-1)

Unix shells commonly provide several forms of *redirection* – ways
to change the input source or output destination of a command. But one
useful redirection is performed by a separate command, not by the shell;
it’s described here.

* [`tee`: Redirect output to multiple files or processes](#tee-invocation)

---

Up: [Redirection](#Redirection)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 17.1 `tee`: Redirect output to multiple files or processes [¶](#tee_003a-Redirect-output-to-multiple-files-or-processes)

The `tee` command copies standard input to standard output and also
to any files given as arguments. This is useful when you want not only
to send some data down a pipe, but also to save a copy. Synopsis:

```
tee [option]... [file]...
```

If a file being written to does not already exist, it is created. If a
file being written to already exists, the data it previously contained
is overwritten unless the `-a` option is used.

In previous versions of GNU Coreutils (5.3.0–8.23),
a file of ‘`-`’
caused `tee` to send another copy of input to standard output.
However, as the interleaved output was not very useful, `tee` now
conforms to POSIX and treats ‘`-`’ as a file name.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-a`’ [¶](#index-_002da-12)

‘`--append`’
:   Append standard input to the given files rather than overwriting
    them.

‘`-i`’ [¶](#index-_002di-16)

‘`--ignore-interrupts`’
:   Ignore interrupt signals.

‘`-p`’ [¶](#index-_002dp-8)

‘`--output-error[=mode]`’
:   Adjust the behavior with errors on the outputs.
    In summary `-p` allows `tee` to operate in a more
    appropriate manner with pipes, and to continue to process data
    to any remaining outputs, if any pipe outputs exit early.
    The default operation when `--output-error` is *not*
    specified is to exit immediately on error writing to a pipe,
    and diagnose errors writing to a non-pipe.
    The long form `--output-error` option supports selection
    between the following modes:

    ‘`warn`’
    :   Warn on error opening or writing any output, including pipes.
        Writing is continued to still open files/pipes.
        Exit status indicates failure if any output has an error.

    ‘`warn-nopipe`’
    :   This is the default mode when not specified,
        or when the short form `-p` is used.
        Warn on error opening or writing any output, except pipes.
        Writing is continued to still open files/pipes.
        Exit immediately if all remaining outputs become broken pipes.
        Exit status indicates failure if any non pipe output had an error.

    ‘`exit`’
    :   Exit on error opening or writing any output, including pipes.

    ‘`exit-nopipe`’
    :   Exit on error opening or writing any output, except pipes.
        Exit immediately if all remaining outputs become broken pipes.

The `tee` command is useful when you happen to be transferring a large
amount of data and also want to summarize that data without reading
it a second time. For example, when you are downloading a DVD image,
you often want to verify its signature or checksum right away.
The inefficient way to do it is simply:

```
wget https://example.com/some.iso && sha1sum some.iso
```

One problem with the above is that it makes you wait for the
download to complete before starting the time-consuming SHA1 computation.
Perhaps even more importantly, the above requires reading
the DVD image a second time (the first was from the network).

The efficient way to do it is to interleave the download
and SHA1 computation. Then, you’ll get the checksum for
free, because the entire process parallelizes so well:

```
# slightly contrived, to demonstrate process substitution
wget -O - https://example.com/dvd.iso \
  | tee >(sha1sum > dvd.sha1) > dvd.iso
```

That makes `tee` write not just to the expected output file,
but also to a pipe running `sha1sum` and saving the final
checksum in a file named `dvd.sha1`.

However, this example relies on a feature of modern shells
called *process substitution*
(the ‘`>(command)`’ syntax, above;
See [Process Substitution](https://www.gnu.org/software/bash/manual/bash.html#Process-Substitution) in The Bash Reference Manual.),
so it works with `zsh`, `bash`, and `ksh`,
but not with `/bin/sh`. So if you write code like this
in a shell script, start the script with ‘`#!/bin/bash`’.

If any of the process substitutions (or piped standard output)
might exit early without consuming all the data, the `-p` option
is needed to allow `tee` to continue to process the input
to any remaining outputs.

Since the above example writes to one file and one process,
a more conventional and portable use of `tee` is even better:

```
wget -O - https://example.com/dvd.iso \
  | tee dvd.iso | sha1sum > dvd.sha1
```

You can extend this example to make `tee` write to two processes,
computing MD5 and SHA1 checksums in parallel. In this case,
process substitution is required:

```
wget -O - https://example.com/dvd.iso \
  | tee >(sha1sum > dvd.sha1) \
        >(md5sum > dvd.md5) \
  > dvd.iso
```

This technique is also useful when you want to make a *compressed*
copy of the contents of a pipe.
Consider a tool to graphically summarize file system usage data from
‘`du -ak`’.
For a large hierarchy, ‘`du -ak`’ can run for a long time,
and can easily produce terabytes of data, so you won’t want to
rerun the command unnecessarily. Nor will you want to save
the uncompressed output.

Doing it the inefficient way, you can’t even start the GUI
until after you’ve compressed all of the `du` output:

```
du -ak | gzip -9 > /tmp/du.gz
gzip -d /tmp/du.gz | checkspace -a
```

With `tee` and process substitution, you start the GUI
right away and eliminate the decompression completely:

```
du -ak | tee >(gzip -9 > /tmp/du.gz) | checkspace -a
```

Finally, if you regularly create more than one type of
compressed tarball at once, for example when `make dist` creates
both `gzip`-compressed and `bzip2`-compressed tarballs,
there may be a better way.
Typical `automake`-generated `Makefile` rules create
the two compressed tar archives with commands in sequence, like this
(slightly simplified):

```
tardir=your-pkg-M.N
tar chof - "$tardir" | gzip  -9 -c > your-pkg-M.N.tar.gz
tar chof - "$tardir" | bzip2 -9 -c > your-pkg-M.N.tar.bz2
```

However, if the hierarchy you are archiving and compressing is larger
than a couple megabytes, and especially if you are using a multi-processor
system with plenty of memory, then you can do much better by reading the
directory contents only once and running the compression programs in parallel:

```
tardir=your-pkg-M.N
tar chof - "$tardir" \
  | tee >(gzip -9 -c > your-pkg-M.N.tar.gz) \
  | bzip2 -9 -c > your-pkg-M.N.tar.bz2
```

If you want to further process the output from process substitutions,
and those processes write atomically (i.e., write less than the system’s
PIPE\_BUF size at a time), that’s possible with a construct like:

```
tardir=your-pkg-M.N
tar chof - "$tardir" \
  | tee >(md5sum --tag) > >(sha256sum --tag) \
  | sort | gpg --clearsign > your-pkg-M.N.tar.sig
```

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [Working context](#Working-context), Previous: [Redirection](#Redirection), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 18 File name manipulation [¶](#File-name-manipulation-1)

This section describes commands that manipulate file names.

* [`basename`: Strip directory and suffix from a file name](#basename-invocation)
* [`dirname`: Strip last file name component](#dirname-invocation)
* [`pathchk`: Check file name validity and portability](#pathchk-invocation)
* [`mktemp`: Create temporary file or directory](#mktemp-invocation)
* [`realpath`: Print the resolved file name.](#realpath-invocation)

---

Next: [`dirname`: Strip last file name component](#dirname-invocation), Up: [File name manipulation](#File-name-manipulation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 18.1 `basename`: Strip directory and suffix from a file name [¶](#basename_003a-Strip-directory-and-suffix-from-a-file-name)

`basename` removes any leading directory components from
name. Synopsis:

```
basename name [suffix]
basename option... name...
```

If suffix is specified and is identical to the end of name,
it is removed from name as well. Since trailing slashes
are removed prior to suffix matching, suffix will do nothing if it
contains slashes. `basename` prints the result on standard
output.

Together, `basename` and `dirname` are designed such
that if ‘`ls "$name"`’ succeeds, then the command sequence ‘`cd
"$(dirname "$name")"; ls "$(basename "$name")"`’ will, too. This works
for everything except file names containing a trailing newline.

POSIX allows the implementation to define the results if
name is empty or ‘`//`’. In the former case, GNU
`basename` returns the empty string. In the latter case, the
result is ‘`//`’ on platforms where // is distinct from
/, and ‘`/`’ on platforms where there is no difference.

The program accepts the following options. Also see [Common options](#Common-options).
Options must precede operands.

‘`-a`’ [¶](#index-_002da-13)

‘`--multiple`’
:   Support more than one argument. Treat every argument as a name.
    With this, an optional suffix must be specified using the
    `-s` option.

‘`-s suffix`’ [¶](#index-_002ds-23)

‘`--suffix=suffix`’
:   Remove a trailing suffix.
    This option implies the `-a` option.

‘`-z`’ [¶](#index-_002dz-14)

‘`--zero`’
:   Output a zero byte (ASCII NUL) at the end of each line,
    rather than a newline. This option enables other programs to parse the
    output even when that output would contain data with embedded newlines.

An exit status of zero indicates success,
and a nonzero value indicates failure.

Examples:

```
# Output "sort".
basename /usr/bin/sort

# Output "stdio".
basename include/stdio.h .h

# Output "stdio".
basename -s .h include/stdio.h

# Output "stdio" followed by "stdlib"
basename -a -s .h include/stdio.h include/stdlib.h
```

---

Next: [`pathchk`: Check file name validity and portability](#pathchk-invocation), Previous: [`basename`: Strip directory and suffix from a file name](#basename-invocation), Up: [File name manipulation](#File-name-manipulation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 18.2 `dirname`: Strip last file name component [¶](#dirname_003a-Strip-last-file-name-component)

`dirname` prints all but the final slash-delimited component
of each name. Slashes on either side of the final component are
also removed. If the string contains no slash, `dirname`
prints ‘`.`’ (meaning the current directory). Synopsis:

```
dirname [option] name...
```

name need not be a file name, but if it is, this operation
effectively lists the directory that contains the final component,
including the case when the final component is itself a directory.

Together, `basename` and `dirname` are designed such
that if ‘`ls "$name"`’ succeeds, then the command sequence ‘`cd
"$(dirname "$name")"; ls "$(basename "$name")"`’ will, too. This works
for everything except file names containing a trailing newline.

POSIX allows the implementation to define the results if
name is ‘`//`’. With GNU `dirname`, the
result is ‘`//`’ on platforms where // is distinct from
/, and ‘`/`’ on platforms where there is no difference.

The program accepts the following option. Also see [Common options](#Common-options).

‘`-z`’ [¶](#index-_002dz-15)

‘`--zero`’
:   Output a zero byte (ASCII NUL) at the end of each line,
    rather than a newline. This option enables other programs to parse the
    output even when that output would contain data with embedded newlines.

An exit status of zero indicates success,
and a nonzero value indicates failure.

Examples:

```
# Output "/usr/bin".
dirname /usr/bin/sort
dirname /usr/bin//.//

# Output "dir1" followed by "dir2"
dirname dir1/str dir2/str

# Output ".".
dirname stdio.h
```

---

Next: [`mktemp`: Create temporary file or directory](#mktemp-invocation), Previous: [`dirname`: Strip last file name component](#dirname-invocation), Up: [File name manipulation](#File-name-manipulation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 18.3 `pathchk`: Check file name validity and portability [¶](#pathchk_003a-Check-file-name-validity-and-portability)

`pathchk` checks validity and portability of file names. Synopsis:

```
pathchk [option]... name...
```

For each name, `pathchk` prints an error message if any of
these conditions is true:

1. One of the existing directories in name does not have search
   (execute) permission,
2. The length of name is larger than the maximum supported by the
   operating system.
3. The length of one component of name is longer than
   its file system’s maximum.

A nonexistent name is not an error, so long as a file with that
name could be created under the above conditions.

The program accepts the following options. Also see [Common options](#Common-options).
Options must precede operands.

‘`-p`’ [¶](#index-_002dp-9)
:   Instead of performing checks based on the underlying file system,
    print an error message if any of these conditions is true:

    1. A file name is empty.
    2. A file name contains a character outside the POSIX portable file
       name character set, namely, the ASCII letters and digits, ‘`.`’,
       ‘`_`’, ‘`-`’, and ‘`/`’.
    3. The length of a file name or one of its components exceeds the
       POSIX minimum limits for portability.

‘`-P`’ [¶](#index-_002dP-8)
:   Print an error message if a file name is empty, or if it contains a component
    that begins with ‘`-`’.

‘`--portability`’ [¶](#index-_002d_002dportability-1)
:   Print an error message if a file name is not portable to all POSIX
    hosts. This option is equivalent to ‘`-p -P`’.

Exit status:

```
0 if all specified file names passed all checks,
1 otherwise.
```

---

Next: [`realpath`: Print the resolved file name.](#realpath-invocation), Previous: [`pathchk`: Check file name validity and portability](#pathchk-invocation), Up: [File name manipulation](#File-name-manipulation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 18.4 `mktemp`: Create temporary file or directory [¶](#mktemp_003a-Create-temporary-file-or-directory)

`mktemp` manages the creation of temporary files and
directories. Synopsis:

```
mktemp [option]... [template]
```

Safely create a temporary file or directory based on template,
and print its name. If given, template must include at least
three consecutive ‘`X`’s in the last component. If omitted, the template
‘`tmp.XXXXXXXXXX`’ is used, and option `--tmpdir` is
implied. The final run of ‘`X`’s in the template will be replaced
by alphanumeric characters; thus, on a case-sensitive file system,
and with a template including a run of n instances of ‘`X`’,
there are ‘`62**n`’ potential file names.

Older scripts used to create temporary files by simply joining the
name of the program with the process id (‘`$$`’) as a suffix.
However, that naming scheme is easily predictable, and suffers from a
race condition where the attacker can create an appropriately named
symbolic link, such that when the script then opens a handle to what
it thought was an unused file, it is instead modifying an existing
file. Using the same scheme to create a directory is slightly safer,
since the `mkdir` will fail if the target already exists, but
it is still inferior because it allows for denial of service attacks.
Therefore, modern scripts should use the `mktemp` command to
guarantee that the generated name will be unpredictable, and that
knowledge of the temporary file name implies that the file was created
by the current script and cannot be modified by other users.

When creating a file, the resulting file has read and write
permissions for the current user, but no permissions for the group or
others; these permissions are reduced if the current umask is more
restrictive.

Here are some examples (although if you try them, you
will most likely get different file names):

* Create a temporary file in the current directory.

  ```
  $ mktemp file.XXXX
  file.H47c
  ```
* Create a temporary file with a known suffix.

  ```
  $ mktemp --suffix=.txt file-XXXX
  file-H08W.txt
  $ mktemp file-XXXX-XXXX.txt
  file-XXXX-eI9L.txt
  ```
* Create a secure fifo relative to the user’s choice of `TMPDIR`,
  but falling back to the current directory rather than `/tmp`.
  Although `mktemp` does not create fifos, it can create a
  secure directory in which fifos can live. Exit the shell if the
  directory or fifo could not be created.

  ```
  $ dir=$(mktemp -p "${TMPDIR:-.}" -d dir-XXXX) || exit 1
  $ fifo=$dir/fifo
  $ mkfifo "$fifo" || { rmdir "$dir"; exit 1; }
  ```
* Create and use a temporary file if possible, but ignore failure. The
  file will reside in the directory named by `TMPDIR`, if specified,
  or else in `/tmp`.

  ```
  $ file=$(mktemp -q) && {
  >   # Safe to use $file only within this block.  Use quotes,
  >   # since $TMPDIR, and thus $file, may contain whitespace.
  >   echo ... > "$file"
  >   rm "$file"
  > }
  ```
* Act as a semi-random character generator (it is not fully random,
  since it is impacted by the contents of the current directory). To
  avoid security holes, do not use the resulting names to create a file.

  ```
  $ mktemp -u XXX
  Gb9
  $ mktemp -u XXX
  nzC
  ```

The program accepts the following options. Also see [Common options](#Common-options).

‘`-d`’ [¶](#index-_002dd-17)

‘`--directory`’
:   Create a directory rather than a file. The directory will have read,
    write, and search permissions for the current user, but no permissions
    for the group or others; these permissions are reduced if the current
    umask is more restrictive.

‘`-q`’ [¶](#index-_002dq-5)

‘`--quiet`’
:   Suppress diagnostics about failure to create a file or directory. The
    exit status will still reflect whether a file was created.

‘`-u`’ [¶](#index-_002du-10)

‘`--dry-run`’
:   Generate a temporary name that does not name an existing file, without
    changing the file system contents. Using the output of this command
    to create a new file is inherently unsafe, as there is a window of
    time between generating the name and using it where another process
    can create an object by the same name.

‘`-p dir`’ [¶](#index-_002dp-10)

‘`--tmpdir[=dir]`’
:   Treat template relative to the directory dir. If
    dir is not specified (only possible with the long option
    `--tmpdir`) or is the empty string, use the value of
    `TMPDIR` if available, otherwise use ‘`/tmp`’. If this is
    specified, template must not be absolute. However,
    template can still contain slashes, although intermediate
    directories must already exist.

‘`--suffix=suffix`’ [¶](#index-_002d_002dsuffix-6)
:   Append suffix to the template. suffix must not
    contain slash. If `--suffix` is specified, template must
    end in ‘`X`’; if it is not specified, then an appropriate
    `--suffix` is inferred by finding the last ‘`X`’ in
    template. This option exists for use with the default
    template and for the creation of a suffix that starts with
    ‘`X`’.

‘`-t`’ [¶](#index-_002dt-19)
:   Treat template as a single file relative to the value of
    `TMPDIR` if available, or to the directory specified by
    `-p`, otherwise to ‘`/tmp`’. template must not
    contain slashes. This option is deprecated; the use of `-p`
    without `-t` offers better defaults (by favoring the command
    line over `TMPDIR`) and more flexibility (by allowing intermediate
    directories).

Exit status:

```
0 if the file was created,
1 otherwise.
```

---

Previous: [`mktemp`: Create temporary file or directory](#mktemp-invocation), Up: [File name manipulation](#File-name-manipulation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 18.5 `realpath`: Print the resolved file name. [¶](#realpath_003a-Print-the-resolved-file-name_002e)

`realpath` expands all symbolic links and resolves references to
‘`/./`’, ‘`/../`’ and extra ‘`/`’ characters. By default,
all but the last component of the specified files must exist. Synopsis:

```
realpath [option]... file...
```

The file name canonicalization functionality overlaps with that of the
`readlink` command. This is the preferred command for
canonicalization as it’s a more suitable and standard name. In addition
this command supports relative file name processing functionality.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-e`’ [¶](#index-_002de-7)

‘`--canonicalize-existing`’
:   Ensure that all components of the specified file names exist.
    If any component is missing or unavailable, `realpath` will output
    a diagnostic unless the `-q` option is specified, and exit with a
    nonzero exit code. A trailing slash requires that the name resolve to a
    directory.

‘`-m`’ [¶](#index-_002dm-11)

‘`--canonicalize-missing`’
:   If any component of a specified file name is missing or unavailable,
    treat it as a directory.

‘`-L`’ [¶](#index-_002dL-11)

‘`--logical`’
:   Symbolic links are resolved in the specified file names,
    but they are resolved after any subsequent ‘`..`’ components are processed.

‘`-P`’ [¶](#index-_002dP-9)

‘`--physical`’
:   Symbolic links are resolved in the specified file names,
    and they are resolved before any subsequent ‘`..`’ components are processed.
    This is the default mode of operation.

‘`-q`’ [¶](#index-_002dq-6)

‘`--quiet`’
:   Suppress diagnostic messages for specified file names.

‘`--relative-to=dir`’ [¶](#index-_002d_002drelative_002dto)
:   Print the resolved file names relative to the specified directory.
    This option honors the `-m` and `-e` options
    pertaining to file existence.

‘`--relative-base=dir`’ [¶](#index-_002d_002drelative_002dbase)
:   Print the resolved file names as relative *if* the files
    are descendants of dir.
    Otherwise, print the resolved file names as absolute.
    This option honors the `-m` and `-e` options
    pertaining to file existence.
    For details about combining `--relative-to` and `--relative-base`,
    see [Realpath usage examples](#Realpath-usage-examples).

‘`-s`’ [¶](#index-_002ds-24)

‘`--strip`’

‘`--no-symlinks`’
:   Do not resolve symbolic links. Only resolve references to
    ‘`/./`’, ‘`/../`’ and remove extra ‘`/`’ characters.
    When combined with the `-m` option, realpath operates
    only on the file name, and does not touch any actual file.

‘`-z`’ [¶](#index-_002dz-16)

‘`--zero`’
:   Output a zero byte (ASCII NUL) at the end of each line,
    rather than a newline. This option enables other programs to parse the
    output even when that output would contain data with embedded newlines.

Exit status:

```
0 if all file names were printed without issue.
1 otherwise.
```

* [Realpath usage examples](#Realpath-usage-examples)

---

Up: [`realpath`: Print the resolved file name.](#realpath-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 18.5.1 Realpath usage examples [¶](#Realpath-usage-examples-1)

By default, `realpath` prints the absolute file name of given files
(symlinks are resolved, `words` is resolved to `american-english`):

```
cd /home/user
realpath /usr/bin/sort /tmp/foo /usr/share/dict/words 1.txt
⇒ /usr/bin/sort
⇒ /tmp/foo
⇒ /usr/share/dict/american-english
⇒ /home/user/1.txt
```

With `--relative-to`, file names are printed relative to
the given directory:

```
realpath --relative-to=/usr/bin \
         /usr/bin/sort /tmp/foo /usr/share/dict/words 1.txt
⇒ sort
⇒ ../../tmp/foo
⇒ ../share/dict/american-english
⇒ ../../home/user/1.txt
```

With `--relative-base`, relative file names are printed *if*
the resolved file name is below the given base directory. For files outside the
base directory absolute file names are printed:

```
realpath --relative-base=/usr \
         /usr/bin/sort /tmp/foo /usr/share/dict/words 1.txt
⇒ bin/sort
⇒ /tmp/foo
⇒ share/dict/american-english
⇒ /home/user/1.txt
```

When both `--relative-to=DIR1` and `--relative-base=DIR2`
are used, file names are printed relative to dir1 *if* they are
located below dir2. If the files are not below dir2, they are
printed as absolute file names:

```
realpath --relative-to=/usr/bin --relative-base=/usr \
         /usr/bin/sort /tmp/foo /usr/share/dict/words 1.txt
⇒ sort
⇒ /tmp/foo
⇒ ../share/dict/american-english
⇒ /home/user/1.txt
```

When both `--relative-to=DIR1` and `--relative-base=DIR2`
are used, dir1 *must* be a subdirectory of dir2. Otherwise,
`realpath` prints absolutes file names.

---

Next: [User information](#User-information), Previous: [File name manipulation](#File-name-manipulation), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 19 Working context [¶](#Working-context-1)

This section describes commands that display or alter the context in
which you are working: the current directory, the terminal settings, and
so forth. See also the user-related commands in the next section.

* [`pwd`: Print working directory](#pwd-invocation)
* [`stty`: Print or change terminal characteristics](#stty-invocation)
* [`printenv`: Print all or some environment variables](#printenv-invocation)
* [`tty`: Print file name of terminal on standard input](#tty-invocation)

---

Next: [`stty`: Print or change terminal characteristics](#stty-invocation), Up: [Working context](#Working-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 19.1 `pwd`: Print working directory [¶](#pwd_003a-Print-working-directory)

`pwd` prints the name of the current directory. Synopsis:

```
pwd [option]...
```

The program accepts the following options. Also see [Common options](#Common-options).

‘`-L`’ [¶](#index-_002dL-12)

‘`--logical`’
:   If the contents of the environment variable `PWD` provide an
    absolute name of the current directory with no ‘`.`’ or ‘`..`’
    components, but possibly with symbolic links, then output those
    contents. Otherwise, fall back to default `-P` handling.

‘`-P`’ [¶](#index-_002dP-10)

‘`--physical`’
:   Print a fully resolved name for the current directory. That is, all
    components of the printed name will be actual directory names – none
    will be symbolic links.

If `-L` and `-P` are both given, the last one takes
precedence. If neither option is given, then this implementation uses
`-P` as the default unless the `POSIXLY_CORRECT`
environment variable is set.

Due to shell aliases and built-in `pwd` functions, using an
unadorned `pwd` interactively or in a script may get you
different functionality than that described here. Invoke it via
`env` (i.e., `env pwd …`) to avoid interference
from the shell.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`printenv`: Print all or some environment variables](#printenv-invocation), Previous: [`pwd`: Print working directory](#pwd-invocation), Up: [Working context](#Working-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]
