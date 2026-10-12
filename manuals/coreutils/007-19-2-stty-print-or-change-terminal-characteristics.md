### 19.2 `stty`: Print or change terminal characteristics [¶](#stty_003a-Print-or-change-terminal-characteristics)

`stty` prints or changes terminal characteristics, such as baud rate.
Synopses:

```
stty [option] [setting]...
stty [option]
```

If given no line settings, `stty` prints the baud rate, line
discipline number (on systems that support it), and line settings
that have been changed from the values set by ‘`stty sane`’.
By default, mode reading and setting are performed on the tty line
connected to standard input, although this can be modified by the
`--file` option.

`stty` accepts many non-option arguments that change aspects of
the terminal line operation, as described below.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-a`’ [¶](#index-_002da-14)

‘`--all`’
:   Print all current settings in human-readable form. This option may not
    be used in combination with any line settings.

‘`-F device`’ [¶](#index-_002dF-5)

‘`--file=device`’
:   Set the line opened by the file name specified in device instead of
    the tty line connected to standard input. This option is necessary
    because opening a POSIX tty requires use of the
    `O_NONDELAY` flag to prevent a POSIX tty from blocking
    until the carrier detect line is high if
    the `clocal` flag is not set. Hence, it is not always possible
    to allow the shell to open the device in the traditional manner.

‘`-g`’ [¶](#index-_002dg-6)

‘`--save`’
:   Print all current settings in a form that can be used as an argument to
    another `stty` command to restore the current settings. This option
    may not be used in combination with any line settings.

Many settings can be turned off by preceding them with a ‘`-`’.
Such arguments are marked below with “May be negated” in their
description. The descriptions themselves refer to the positive
case, that is, when *not* negated (unless stated otherwise,
of course).

Some settings are not available on all POSIX systems, since they use
extensions. Such arguments are marked below with
“Non-POSIX” in their description. On non-POSIX
systems, those or other settings also may not
be available, but it’s not feasible to document all the variations: just
try it and see.

`stty` is installed only on platforms with the POSIX terminal
interface, so portable scripts should not rely on its existence on
non-POSIX platforms.

An exit status of zero indicates success,
and a nonzero value indicates failure.

* [Control settings](#Control)
* [Input settings](#Input)
* [Output settings](#Output)
* [Local settings](#Local)
* [Combination settings](#Combination)
* [Special characters](#Characters)
* [Special settings](#Special)

---

Next: [Input settings](#Input), Up: [`stty`: Print or change terminal characteristics](#stty-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 19.2.1 Control settings [¶](#Control-settings)

Control settings:

‘`parenb`’ [¶](#index-parenb)
:   Generate parity bit in output and expect parity bit in input.
    May be negated.

‘`parodd`’ [¶](#index-parodd)
:   Set odd parity (even if negated). May be negated.

‘`cmspar`’ [¶](#index-cmspar)
:   Use "stick" (mark/space) parity. If parodd is set, the parity bit is
    always 1; if parodd is not set, the parity bit is always zero.
    Non-POSIX. May be negated.

‘`cs5`’ [¶](#index-csn)

‘`cs6`’

‘`cs7`’

‘`cs8`’
:   Set character size to 5, 6, 7, or 8 bits.

‘`hup`’ [¶](#index-hup_005bcl_005d)

‘`hupcl`’
:   Send a hangup signal when the last process closes the tty. May be
    negated.

‘`cstopb`’ [¶](#index-cstopb)
:   Use two stop bits per character (one if negated). May be negated.

‘`cread`’ [¶](#index-cread)
:   Allow input to be received. May be negated.

‘`clocal`’ [¶](#index-clocal)
:   Disable modem control signals. May be negated.

‘`crtscts`’ [¶](#index-crtscts)
:   Enable RTS/CTS flow control. Non-POSIX. May be negated.

‘`cdtrdsr`’ [¶](#index-cdtrdsr)
:   Enable DTR/DSR flow control. Non-POSIX. May be negated.

---

Next: [Output settings](#Output), Previous: [Control settings](#Control), Up: [`stty`: Print or change terminal characteristics](#stty-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 19.2.2 Input settings [¶](#Input-settings)

These settings control operations on data received from the terminal.

‘`ignbrk`’ [¶](#index-ignbrk)
:   Ignore break characters. May be negated.

‘`brkint`’ [¶](#index-brkint)
:   Make breaks cause an interrupt signal. May be negated.

‘`ignpar`’ [¶](#index-ignpar)
:   Ignore characters with parity errors. May be negated.

‘`parmrk`’ [¶](#index-parmrk)
:   Mark parity errors (with a 255-0-character sequence). May be negated.

‘`inpck`’ [¶](#index-inpck)
:   Enable input parity checking. May be negated.

‘`istrip`’ [¶](#index-istrip)
:   Clear high (8th) bit of input characters. May be negated.

‘`inlcr`’ [¶](#index-inlcr)
:   Translate newline to carriage return. May be negated.

‘`igncr`’ [¶](#index-igncr)
:   Ignore carriage return. May be negated.

‘`icrnl`’ [¶](#index-icrnl)
:   Translate carriage return to newline. May be negated.

‘`iutf8`’ [¶](#index-iutf8)
:   Assume input characters are UTF-8 encoded. May be negated.

‘`ixon`’ [¶](#index-ixon)
:   Enable XON/XOFF flow control (that is, `Ctrl-S`/`Ctrl-Q`). May
    be negated.

‘`ixoff`’ [¶](#index-ixoff)

‘`tandem`’
:   Enable sending of `stop` character when the system input buffer
    is almost full, and `start` character when it becomes almost
    empty again. May be negated.

‘`iuclc`’ [¶](#index-iuclc)
:   Translate uppercase characters to lowercase. Non-POSIX. May be
    negated. There is no ‘`ilcuc`’ setting, as one would not be able to issue
    almost any (lowercase) Unix command, after invoking it.

‘`ixany`’ [¶](#index-ixany)
:   Allow any character to restart output (only the start character
    if negated). Non-POSIX. May be negated.

‘`imaxbel`’ [¶](#index-imaxbel)
:   Enable beeping and not flushing input buffer if a character arrives
    when the input buffer is full. Non-POSIX. May be negated.

---

Next: [Local settings](#Local), Previous: [Input settings](#Input), Up: [`stty`: Print or change terminal characteristics](#stty-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 19.2.3 Output settings [¶](#Output-settings)

These settings control operations on data sent to the terminal.

‘`opost`’ [¶](#index-opost)
:   Postprocess output. May be negated.

‘`olcuc`’ [¶](#index-olcuc)
:   Translate lowercase characters to uppercase. Non-POSIX. May be
    negated. (There is no ‘`ouclc`’.)

‘`ocrnl`’ [¶](#index-ocrnl)
:   Translate carriage return to newline. Non-POSIX. May be negated.

‘`onlcr`’ [¶](#index-onlcr)
:   Translate newline to carriage return-newline. Non-POSIX. May be
    negated.

‘`onocr`’ [¶](#index-onocr)
:   Do not print carriage returns in the first column. Non-POSIX.
    May be negated.

‘`onlret`’ [¶](#index-onlret)
:   Newline performs a carriage return. Non-POSIX. May be negated.

‘`ofill`’ [¶](#index-ofill)
:   Use fill (padding) characters instead of timing for delays.
    Non-POSIX.
    May be negated.

‘`ofdel`’ [¶](#index-ofdel)
:   Use ASCII DEL characters for fill instead of
    ASCII NUL characters. Non-POSIX.
    May be negated.

‘`nl1`’ [¶](#index-nln)

‘`nl0`’
:   Newline delay style. Non-POSIX.

‘`cr3`’ [¶](#index-crn)

‘`cr2`’

‘`cr1`’

‘`cr0`’
:   Carriage return delay style. Non-POSIX.

‘`tab3`’ [¶](#index-tabn)

‘`tab2`’

‘`tab1`’

‘`tab0`’
:   Horizontal tab delay style. Non-POSIX.

‘`bs1`’ [¶](#index-bsn)

‘`bs0`’
:   Backspace delay style. Non-POSIX.

‘`vt1`’ [¶](#index-vtn)

‘`vt0`’
:   Vertical tab delay style. Non-POSIX.

‘`ff1`’ [¶](#index-ffn)

‘`ff0`’
:   Form feed delay style. Non-POSIX.

---

Next: [Combination settings](#Combination), Previous: [Output settings](#Output), Up: [`stty`: Print or change terminal characteristics](#stty-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 19.2.4 Local settings [¶](#Local-settings)

‘`isig`’ [¶](#index-isig)
:   Enable `interrupt`, `quit`, and `suspend` special
    characters. May be negated.

‘`icanon`’ [¶](#index-icanon)
:   Enable `erase`, `kill`, `werase`, and `rprnt`
    special characters. May be negated.

‘`iexten`’ [¶](#index-iexten)
:   Enable non-POSIX special characters. May be negated.

‘`echo`’ [¶](#index-echo)
:   Echo input characters. May be negated.

‘`echoe`’ [¶](#index-echoe)

‘`crterase`’
:   Echo `erase` characters as backspace-space-backspace. May be
    negated.

‘`echok`’ [¶](#index-echok)
:   Echo a newline after a `kill` character. May be negated.

‘`echonl`’ [¶](#index-echonl)
:   Echo newline even if not echoing other characters. May be negated.

‘`noflsh`’ [¶](#index-noflsh)
:   Disable flushing after `interrupt` and `quit` special
    characters. May be negated.

‘`xcase`’ [¶](#index-xcase)
:   Enable input and output of uppercase characters by preceding their
    lowercase equivalents with ‘`\`’, when `icanon` is set.
    Non-POSIX. May be negated.

‘`tostop`’ [¶](#index-tostop)
:   Stop background jobs that try to write to the terminal. Non-POSIX.
    May be negated.

‘`echoprt`’ [¶](#index-echoprt)

‘`prterase`’
:   Echo erased characters backward, between ‘`\`’ and ‘`/`’.
    Non-POSIX. May be negated.

‘`echoctl`’ [¶](#index-echoctl)

‘`ctlecho`’
:   Echo control characters in hat notation (‘`^c`’) instead
    of literally. Non-POSIX. May be negated.

‘`echoke`’ [¶](#index-echoke)

‘`crtkill`’
:   Echo the `kill` special character by erasing each character on
    the line as indicated by the `echoprt` and `echoe` settings,
    instead of by the `echoctl` and `echok` settings.
    Non-POSIX.
    May be negated.

‘`extproc`’ [¶](#index-extproc)
:   Enable ‘`LINEMODE`’, which is used to avoid echoing
    each character over high latency links. See also
    [Internet RFC 1116](https://datatracker.ietf.org/doc/rfc1116/).
    Non-POSIX.
    May be negated.

‘`flusho`’ [¶](#index-flusho)
:   Discard output.
    This setting is currently ignored on GNU/Linux systems.
    Non-POSIX.
    May be negated.

---

Next: [Special characters](#Characters), Previous: [Local settings](#Local), Up: [`stty`: Print or change terminal characteristics](#stty-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 19.2.5 Combination settings [¶](#Combination-settings)

Combination settings:

‘`evenp`’ [¶](#index-parity)

‘`parity`’
:   Same as `parenb -parodd cs7`. May be negated. If negated, same
    as `-parenb cs8`.

‘`oddp`’ [¶](#index-oddp)
:   Same as `parenb parodd cs7`. May be negated. If negated, same
    as `-parenb cs8`.

‘`nl`’ [¶](#index-nl)
:   Same as `-icrnl -onlcr`. May be negated. If negated, same as
    `icrnl -inlcr -igncr onlcr -ocrnl -onlret`.

‘`ek`’ [¶](#index-ek)
:   Reset the `erase` and `kill` special characters to their default
    values.

‘`sane`’ [¶](#index-sane)
:   Same as:

    ```
    cread -ignbrk brkint -inlcr -igncr icrnl
    icanon iexten echo echoe echok -echonl -noflsh
    -ixoff -iutf8 -iuclc -ixany imaxbel -xcase -olcuc -ocrnl
    opost -ofill onlcr -onocr -onlret nl0 cr0 tab0 bs0 vt0 ff0
    isig -tostop -ofdel -echoprt echoctl echoke -extproc
    ```

    and also sets all special characters to their default values.

‘`cooked`’ [¶](#index-cooked)
:   Same as `brkint ignpar istrip icrnl ixon opost isig icanon`, plus
    sets the `eof` and `eol` characters to their default values
    if they are the same as the `min` and `time` characters.
    May be negated. If negated, same as `raw`.

‘`raw`’ [¶](#index-raw)
:   Same as:

    ```
    -ignbrk -brkint -ignpar -parmrk -inpck -istrip
    -inlcr -igncr -icrnl -ixon -ixoff -icanon -opost
    -isig -iuclc -ixany -imaxbel -xcase min 1 time 0
    ```

    May be negated. If negated, same as `cooked`.

‘`cbreak`’ [¶](#index-cbreak)
:   Same as `-icanon`. May be negated. If negated, same as
    `icanon`.

‘`pass8`’ [¶](#index-pass8)
:   Same as `-parenb -istrip cs8`. May be negated. If negated,
    same as `parenb istrip cs7`.

‘`litout`’ [¶](#index-litout)
:   Same as `-parenb -istrip -opost cs8`. May be negated.
    If negated, same as `parenb istrip opost cs7`.

‘`decctlq`’ [¶](#index-decctlq)
:   Same as `-ixany`. Non-POSIX. May be negated.

‘`tabs`’ [¶](#index-tabs)
:   Same as `tab0`. Non-POSIX. May be negated. If negated, same
    as `tab3`.

‘`lcase`’ [¶](#index-lcase)

‘`LCASE`’
:   Same as `xcase iuclc olcuc`. Non-POSIX. May be negated.
    (Used for terminals with uppercase characters only.)

‘`crt`’ [¶](#index-crt)
:   Same as `echoe echoctl echoke`.

‘`dec`’ [¶](#index-dec)
:   Same as `echoe echoctl echoke -ixany intr ^C erase ^? kill C-u`.

---

Next: [Special settings](#Special), Previous: [Combination settings](#Combination), Up: [`stty`: Print or change terminal characteristics](#stty-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 19.2.6 Special characters [¶](#Special-characters)

The special characters’ default values vary from system to system.
They are set with the syntax ‘`name value`’, where the names are
listed below and the value can be given either literally, in hat
notation (‘`^c`’), or as an integer which may start with
‘`0x`’ to indicate hexadecimal, ‘`0`’ to indicate octal, or
any other digit to indicate decimal.

For GNU stty, giving a value of `^-` or `undef` disables that
special character. (This is incompatible with Ultrix `stty`,
which uses a value of ‘`u`’ to disable a special character. GNU
`stty` treats a value ‘`u`’ like any other, namely to set that
special character to `U`.)

‘`intr`’ [¶](#index-intr)
:   Send an interrupt signal.

‘`quit`’ [¶](#index-quit)
:   Send a quit signal.

‘`erase`’ [¶](#index-erase)
:   Erase the last character typed.

‘`kill`’ [¶](#index-kill)
:   Erase the current line.

‘`eof`’ [¶](#index-eof)
:   Send an end of file (terminate the input).

‘`eol`’ [¶](#index-eol)
:   End the line.

‘`eol2`’ [¶](#index-eol2)
:   Alternate character to end the line. Non-POSIX.

‘`discard`’ [¶](#index-discard)
:   Alternate character to toggle discarding of output. Non-POSIX.

‘`swtch`’ [¶](#index-swtch)
:   Switch to a different shell layer. Non-POSIX.

‘`status`’ [¶](#index-status-1)
:   Send an info signal. Not currently supported on GNU/Linux. Non-POSIX.

‘`start`’ [¶](#index-start)
:   Restart the output after stopping it.

‘`stop`’ [¶](#index-stop)
:   Stop the output.

‘`susp`’ [¶](#index-susp)
:   Send a terminal stop signal.

‘`dsusp`’ [¶](#index-dsusp)
:   Send a terminal stop signal after flushing the input. Non-POSIX.

‘`rprnt`’ [¶](#index-rprnt)
:   Redraw the current line. Non-POSIX.

‘`werase`’ [¶](#index-werase)
:   Erase the last word typed. Non-POSIX.

‘`lnext`’ [¶](#index-lnext)
:   Enter the next character typed literally, even if it is a special
    character. Non-POSIX.

---

Previous: [Special characters](#Characters), Up: [`stty`: Print or change terminal characteristics](#stty-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 19.2.7 Special settings [¶](#Special-settings)

‘`min n`’ [¶](#index-min)
:   Set the minimum number of characters that will satisfy a read until
    the time value has expired, when `-icanon` is set.

‘`time n`’ [¶](#index-time-1)
:   Set the number of tenths of a second before reads time out if the minimum
    number of characters have not been read, when `-icanon` is set.

‘`ispeed n`’ [¶](#index-ispeed)
:   Set the input speed to n.

‘`ospeed n`’ [¶](#index-ospeed)
:   Set the output speed to n.

‘`rows n`’ [¶](#index-rows)
:   Tell the tty kernel driver that the terminal has n rows.

‘`cols n`’ [¶](#index-cols)
:   Tell the kernel that the terminal has n columns.

‘`columns n`’ [¶](#index-columns)
:   Same as ‘`cols`’. Non-POSIX.

‘`drain`’ [¶](#index-drain)
:   Apply settings after first waiting for pending output to be transmitted.
    This is enabled by default for GNU `stty`.
    This is treated as an option rather than a line setting,
    and will follow the option processing rules described in the summary above.
    It is useful to disable this option
    in cases where the system may be in a state where serial transmission
    is not possible.
    For example, if the system has received the ‘`DC3`’ character
    with `ixon` (software flow control) enabled, then `stty` would
    block without `-drain` being specified.
    May be negated. Non-POSIX.

‘`size`’ [¶](#index-size)
:   Print the number of rows and columns that the kernel thinks the
    terminal has. (Systems that don’t support rows and columns in the kernel
    typically use the environment variables `LINES` and `COLUMNS`
    instead; however, GNU `stty` does not know anything about them.)

‘`line n`’ [¶](#index-line)
:   Use line discipline n. Non-POSIX.

‘`speed`’ [¶](#index-speed)
:   Print the terminal speed.

‘`n`’ [¶](#index-baud-rate_002c-setting)
:   Set the input and output speeds to n. n can be one of: 0
    50 75 110 134 134.5 150 200 300 600 1200 1800 2400 4800 9600 19200
    38400 `exta` `extb`. `exta` is the same as 19200;
    `extb` is the same as 38400. Many systems, including GNU/Linux,
    support higher speeds. The `stty` command includes support
    for speeds of
    57600,
    115200,
    230400,
    460800,
    500000,
    576000,
    921600,
    1000000,
    1152000,
    1500000,
    2000000,
    2500000,
    3000000,
    3500000,
    or
    4000000 where the system supports these.
    0 hangs up the line if `-clocal` is set.

---

Next: [`tty`: Print file name of terminal on standard input](#tty-invocation), Previous: [`stty`: Print or change terminal characteristics](#stty-invocation), Up: [Working context](#Working-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 19.3 `printenv`: Print all or some environment variables [¶](#printenv_003a-Print-all-or-some-environment-variables)

`printenv` prints environment variable values. Synopsis:

```
printenv [option] [variable]...
```

If no variables are specified, `printenv` prints the value of
every environment variable. Otherwise, it prints the value of each
variable that is set, and nothing for those that are not set.

The program accepts the following option. Also see [Common options](#Common-options).

‘`-0`’ [¶](#index-_002d0-1)

‘`--null`’
:   Output a zero byte (ASCII NUL) at the end of each line,
    rather than a newline. This option enables other programs to parse the
    output even when that output would contain data with embedded newlines.

Exit status:

```
0 if all variables specified were found
1 if at least one specified variable was not found
2 if a write error occurred
```

---

Previous: [`printenv`: Print all or some environment variables](#printenv-invocation), Up: [Working context](#Working-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 19.4 `tty`: Print file name of terminal on standard input [¶](#tty_003a-Print-file-name-of-terminal-on-standard-input)

`tty` prints the file name of the terminal connected to its standard
input. It prints ‘`not a tty`’ if standard input is not a terminal.
Synopsis:

```
tty [option]...
```

The program accepts the following option. Also see [Common options](#Common-options).

‘`-s`’ [¶](#index-_002ds-25)

‘`--silent`’

‘`--quiet`’
:   Print nothing; only return an exit status.

Exit status:

```
0 if standard input is a terminal
1 if standard input is a non-terminal file
2 if given incorrect arguments
3 if a write error occurs
```

---

Next: [System context](#System-context), Previous: [Working context](#Working-context), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 20 User information [¶](#User-information-1)

This section describes commands that print user-related information:
logins, groups, and so forth.

* [`id`: Print user identity](#id-invocation)
* [`logname`: Print current login name](#logname-invocation)
* [`whoami`: Print effective user name](#whoami-invocation)
* [`groups`: Print group names a user is in](#groups-invocation)
* [`users`: Print login names of users currently logged in](#users-invocation)
* [`who`: Print who is currently logged in](#who-invocation)
* [`pinky`: Print information about users](#pinky-invocation)

---

Next: [`logname`: Print current login name](#logname-invocation), Up: [User information](#User-information)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 20.1 `id`: Print user identity [¶](#id_003a-Print-user-identity)

`id` prints information about the given user, or the process
running it if no user is specified. Synopsis:

```
id [option]... [user]...
```

user can be either a user ID or a name, with name look-up
taking precedence unless the ID is specified with a leading ‘`+`’.
See [chown, chgrp, chroot, id: Disambiguating user names and IDs](#Disambiguating-names-and-IDs).

By default, it prints the real user ID, real group ID, effective user ID
if different from the real user ID, effective group ID if different from
the real group ID, and supplemental group IDs.
In addition, if SELinux
is enabled and the `POSIXLY_CORRECT` environment variable is not set,
then print ‘`context=c`’, where c is the security context.

Each of these numeric values is preceded by an identifying string and
followed by the corresponding user or group name in parentheses.

The options cause `id` to print only part of the above information.
Also see [Common options](#Common-options).

‘`-g`’ [¶](#index-_002dg-7)

‘`--group`’
:   Print only the group ID.

‘`-G`’ [¶](#index-_002dG-2)

‘`--groups`’
:   Print only the group ID and the supplementary groups.

‘`-n`’ [¶](#index-_002dn-17)

‘`--name`’
:   Print the user or group name instead of the ID number. Requires
    `-u`, `-g`, or `-G`.

‘`-r`’ [¶](#index-_002dr-13)

‘`--real`’
:   Print the real, instead of effective, user or group ID. Requires
    `-u`, `-g`, or `-G`.

‘`-u`’ [¶](#index-_002du-11)

‘`--user`’
:   Print only the user ID.

‘`-Z`’ [¶](#index-_002dZ-7)

‘`--context`’
:   Print only the security context of the process, which is generally
    the user’s security context inherited from the parent process.
    If neither SELinux or SMACK is enabled then print a warning and
    set the exit status to 1.

‘`-z`’ [¶](#index-_002dz-17)

‘`--zero`’
:   Delimit output items with ASCII NUL characters.
    This option is not permitted when using the default format.
    When multiple users are specified, and the `--groups` option
    is also in effect, groups are delimited with a single NUL character,
    while users are delimited with two NUL characters.

    Example:

    ```
    $ id -Gn --zero
    users <NUL> devs <NUL>
    ```

Primary and supplementary groups for a process are normally inherited
from its parent and are usually unchanged since login. This means
that if you change the group database after logging in, `id`
will not reflect your changes within your existing login session.
Running `id` with a user argument causes the user and group
database to be consulted afresh, and so will give a different result.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`whoami`: Print effective user name](#whoami-invocation), Previous: [`id`: Print user identity](#id-invocation), Up: [User information](#User-information)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 20.2 `logname`: Print current login name [¶](#logname_003a-Print-current-login-name)

`logname` prints the calling user’s name, as found in a
system-maintained file (often `/var/run/utmp` or
`/etc/utmp`), and exits with a status of 0. If there is no entry
for the calling process, `logname` prints
an error message and exits with a status of 1.

The only options are `--help` and `--version`. See [Common options](#Common-options).

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`groups`: Print group names a user is in](#groups-invocation), Previous: [`logname`: Print current login name](#logname-invocation), Up: [User information](#User-information)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 20.3 `whoami`: Print effective user name [¶](#whoami_003a-Print-effective-user-name)

`whoami` prints the user name associated with the current
effective user ID. It is equivalent to the command ‘`id -un`’.

The only options are `--help` and `--version`. See [Common options](#Common-options).

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`users`: Print login names of users currently logged in](#users-invocation), Previous: [`whoami`: Print effective user name](#whoami-invocation), Up: [User information](#User-information)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 20.4 `groups`: Print group names a user is in [¶](#groups_003a-Print-group-names-a-user-is-in)

`groups` prints the names of the primary and any supplementary
groups for each given username, or the current process if no names
are given. If more than one name is given, the name of each user is
printed before
the list of that user’s groups and the user name is separated from the
group list by a colon. Synopsis:

```
groups [username]...
```

The group lists are equivalent to the output of the command ‘`id -Gn`’.

The only options are `--help` and `--version`. See [Common options](#Common-options).

Primary and supplementary groups for a process are normally inherited
from its parent and are usually unchanged since login. This means
that if you change the group database after logging in, `groups`
will not reflect your changes within your existing login session.
Running `groups` with a list of users causes the user and group
database to be consulted afresh, and so will give a different result.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`who`: Print who is currently logged in](#who-invocation), Previous: [`groups`: Print group names a user is in](#groups-invocation), Up: [User information](#User-information)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 20.5 `users`: Print login names of users currently logged in [¶](#users_003a-Print-login-names-of-users-currently-logged-in)

`users` prints on a single line a blank-separated list of user
names of users currently logged in to the current host. Each user name
corresponds to a login session, so if a user has more than one login
session, that user’s name will appear the same number of times in the
output. Synopsis:

```
users [file]
```

With no file argument, `users` extracts its information from
a system-maintained file (often `/var/run/utmp` or
`/etc/utmp`). If a file argument is given, `users` uses
that file instead. A common choice is `/var/log/wtmp`.

The only options are `--help` and `--version`. See [Common options](#Common-options).

The `users` command is installed only on platforms with the
POSIX `<utmpx.h>` include file or equivalent, so portable scripts
should not rely on its existence on non-POSIX platforms.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`pinky`: Print information about users](#pinky-invocation), Previous: [`users`: Print login names of users currently logged in](#users-invocation), Up: [User information](#User-information)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 20.6 `who`: Print who is currently logged in [¶](#who_003a-Print-who-is-currently-logged-in)

`who` prints information about users who are currently logged on.
Synopsis:

```
who [option] [file] [am i]
```

If given no non-option arguments, `who` prints the following
information for each user currently logged on: login name, terminal
line, login time, and remote hostname or X display.

If given one non-option argument, `who` uses that instead of
a default system-maintained file (often `/var/run/utmp` or
`/etc/utmp`) as the name of the file containing the record of
users logged on. `/var/log/wtmp` is commonly given as an argument
to `who` to look at who has previously logged on.

If given two non-option arguments, `who` prints only the entry
for the user running it (determined from its standard input), preceded
by the hostname. Traditionally, the two arguments given are ‘`am
i`’, as in ‘`who am i`’.

Timestamps are listed according to the time zone rules specified by
the `TZ` environment variable, or by the system default rules if
`TZ` is not set. See [Specifying the Time Zone
with `TZ`](https://www.gnu.org/software/libc/manual/html_mono/libc.html#TZ-Variable) in The GNU C Library Reference Manual.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-a`’ [¶](#index-_002da-15)

‘`--all`’
:   Same as ‘`-b -d --login -p -r -t -T -u`’.

‘`-b`’ [¶](#index-_002db-20)

‘`--boot`’
:   Print the date and time of last system boot.

‘`-d`’ [¶](#index-_002dd-18)

‘`--dead`’
:   Print information corresponding to dead processes.

‘`-H`’ [¶](#index-_002dH-8)

‘`--heading`’
:   Print a line of column headings.

‘`-l`’ [¶](#index-_002dl-11)

‘`--login`’
:   List only the entries that correspond to processes via which the
    system is waiting for a user to login. The user name is always ‘`LOGIN`’.

‘`--lookup`’ [¶](#index-_002d_002dlookup)
:   Attempt to canonicalize hostnames found in utmp through a DNS lookup.
    This is not the default because of potential delays.

‘`-m`’ [¶](#index-_002dm-12)
:   Same as ‘`who am i`’.

‘`-p`’ [¶](#index-_002dp-11)

‘`--process`’
:   List active processes spawned by init.

‘`-q`’ [¶](#index-_002dq-7)

‘`--count`’
:   Print only the login names and the number of users logged on.
    Overrides all other options.

‘`-r`’ [¶](#index-_002dr-14)

‘`--runlevel`’
:   Print the current (and maybe previous) run-level of the init process.

‘`-s`’ [¶](#index-_002ds-26)
:   Ignored; for compatibility with other versions of `who`.

‘`-t`’ [¶](#index-_002dt-20)

‘`--time`’
:   Print last system clock change.

‘`-u`’ [¶](#index-_002du-12)
:   After the login time, print the number of hours and minutes that the
    user has been idle. ‘`.`’ means the user was active in the last minute.
    ‘`old`’ means the user has been idle for more than 24 hours.

‘`-w`’ [¶](#index-_002dw-12)

‘`-T`’

‘`--mesg`’

‘`--message`’

‘`--writable`’
:   After each login name print a character indicating the user’s message status:

    ```
    ‘+’ allowing write messages
    ‘-’ disallowing write messages
    ‘?’ cannot find terminal device
    ```

The `who` command is installed only on platforms with the
POSIX `<utmpx.h>` include file or equivalent, so portable scripts
should not rely on its existence on non-POSIX platforms.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`who`: Print who is currently logged in](#who-invocation), Up: [User information](#User-information)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 20.7 `pinky`: Print information about users [¶](#pinky_003a-Print-information-about-users)

`pinky` is a lightweight implementation of the `finger` command.
Synopsis:

```
pinky [option] [username]...
```

The program accepts the following options. Also see [Common options](#Common-options).

‘`-l`’ [¶](#index-_002dl-12)
:   Produce long format output.

    When producing long output at least one username must be given.
    If username cannot be found, the real name is printed as
    ‘`???`’ and the home directory and shell are omitted.

‘`-b`’ [¶](#index-_002db-21)
:   Omit the user’s home directory and shell when printing in long format.

‘`-h`’ [¶](#index-_002dh-12)
:   Omit the user’s project file when printing in long format.

‘`-p`’ [¶](#index-_002dp-12)
:   Omit the user’s plan file when printing in long format.

‘`-s`’ [¶](#index-_002ds-27)
:   Produce short format output. This is the default behavior when no
    options are given.

‘`-f`’ [¶](#index-_002df-22)
:   Omit the column headings when printing in short format.

‘`-w`’ [¶](#index-_002dw-13)
:   Omit the user’s full name when printing in short format.

‘`-i`’ [¶](#index-_002di-17)
:   Omit the user’s full name and remote host when printing in short
    format.

‘`-q`’ [¶](#index-_002dq-8)
:   Omit the user’s full name, remote host, and idle time when printing in
    short format.

‘`--lookup`’ [¶](#index-_002d_002dlookup-1)
:   Attempt to canonicalize hostnames found in utmp through a DNS lookup.
    This is not the default because of potential delays.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [SELinux context](#SELinux-context), Previous: [User information](#User-information), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 21 System context [¶](#System-context-1)

This section describes commands that print or change system-wide
information.

* [`date`: Print or set system date and time](#date-invocation)
* [`arch`: Print machine hardware name](#arch-invocation)
* [`nproc`: Print the number of available processors](#nproc-invocation)
* [`uname`: Print system information](#uname-invocation)
* [`hostname`: Print or set system name](#hostname-invocation)
* [`hostid`: Print numeric host identifier](#hostid-invocation)
* [`uptime`: Print system uptime and load](#uptime-invocation)

---

Next: [`arch`: Print machine hardware name](#arch-invocation), Up: [System context](#System-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 21.1 `date`: Print or set system date and time [¶](#date_003a-Print-or-set-system-date-and-time)

Synopses:

```
date [option]... [+format]
date [-u|--utc|--universal] [ MMDDhhmm[[CC]YY][.ss] ]
```

The `date` command displays the date and time.
With the `--set` (`-s`) option, or with
‘`MMDDhhmm[[CC]YY][.ss]`’,
it sets the date and time.

Invoking `date` with no format argument is equivalent to invoking
it with a default format that depends on the `LC_TIME` locale category.
In the default C locale, this format is ‘`'+%a %b %e %H:%M:%S %Z %Y'`’,
so the output looks like ‘`Thu Jul  9 17:00:00 EDT 2020`’.

Normally, `date` uses the time zone rules indicated by the
`TZ` environment variable, or the system default rules if `TZ`
is not set. See [Specifying the Time Zone with
`TZ`](https://www.gnu.org/software/libc/manual/html_mono/libc.html#TZ-Variable) in The GNU C Library Reference Manual.

An exit status of zero indicates success,
and a nonzero value indicates failure.

* [Specifying the format of `date` output](#Date-format-specifiers)
* [Setting the time](#Setting-the-time)
* [Options for `date`](#Options-for-date)
* [Examples of `date`](#Examples-of-date)

---

Next: [Setting the time](#Setting-the-time), Up: [`date`: Print or set system date and time](#date-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 21.1.1 Specifying the format of `date` output [¶](#Specifying-the-format-of-date-output)

If given an argument that starts with a ‘`+`’, `date` prints the
current date and time (or the date and time specified by the
`--date` option, see below) in the format defined by that argument,
which is similar to that of the `strftime` function. Except for
conversion specifiers, which start with ‘`%`’, characters in the
format string are printed unchanged. The conversion specifiers are
described below.

* [Time conversion specifiers](#Time-conversion-specifiers)
* [Date conversion specifiers](#Date-conversion-specifiers)
* [Literal conversion specifiers](#Literal-conversion-specifiers)
* [Padding and other flags](#Padding-and-other-flags)

---

Next: [Date conversion specifiers](#Date-conversion-specifiers), Up: [Specifying the format of `date` output](#Date-format-specifiers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 21.1.1.1 Time conversion specifiers [¶](#Time-conversion-specifiers-1)

`date` conversion specifiers related to times.

‘`%H`’
:   hour (‘`00`’…‘`23`’)

‘`%I`’
:   hour (‘`01`’…‘`12`’)

‘`%k`’
:   hour, space padded (‘ `0`’…‘`23`’); equivalent to ‘`%_H`’.
    This is a GNU extension.

‘`%l`’
:   hour, space padded (‘ `1`’…‘`12`’); equivalent to ‘`%_I`’.
    This is a GNU extension.

‘`%M`’
:   minute (‘`00`’…‘`59`’)

‘`%N`’
:   nanoseconds (‘`000000000`’…‘`999999999`’).
    This is a GNU extension.

‘`%p`’
:   locale’s equivalent of either ‘`AM`’ or ‘`PM`’;
    blank in many locales.
    Noon is treated as ‘`PM`’ and midnight as ‘`AM`’.

‘`%P`’
:   like ‘`%p`’, except lower case.
    This is a GNU extension.

‘`%r`’
:   locale’s 12-hour clock time (e.g., ‘`11:11:04 PM`’)

‘`%R`’
:   24-hour hour and minute. Same as ‘`%H:%M`’.

‘`%s`’ [¶](#index-Epoch_002c-seconds-since)
:   seconds since the Epoch, i.e., since 1970-01-01 00:00 UTC.
    Leap seconds are not counted unless leap second support is available.
    See [%s-examples](#g_t_0025s_002dexamples), for examples.
    This is a GNU extension.

‘`%S`’ [¶](#index-leap-seconds-2)
:   second (‘`00`’…‘`60`’).
    This may be ‘`60`’ if leap seconds are supported.

‘`%T`’
:   24-hour hour, minute, and second. Same as ‘`%H:%M:%S`’.

‘`%X`’
:   locale’s time representation (e.g., ‘`23:13:48`’)

‘`%z`’
:   Four-digit numeric time zone, e.g., ‘`-0600`’ or ‘`+0530`’, or
    ‘`-0000`’ if no
    time zone is determinable. This value reflects the numeric time zone
    appropriate for the current time, using the time zone rules specified
    by the `TZ` environment variable. A time zone is not determinable if
    its numeric offset is zero and its abbreviation begins with ‘`-`’.
    The time (and optionally, the time zone rules) can be overridden
    by the `--date` option.

‘`%:z`’
:   Numeric time zone with ‘`:`’, e.g., ‘`-06:00`’ or
    ‘`+05:30`’), or ‘`-00:00`’ if no time zone is determinable.
    This is a GNU extension.

‘`%::z`’
:   Numeric time zone to the nearest second with ‘`:`’ (e.g.,
    ‘`-06:00:00`’ or ‘`+05:30:00`’), or ‘`-00:00:00`’ if no time zone is
    determinable.
    This is a GNU extension.

‘`%:::z`’
:   Numeric time zone with ‘`:`’ using the minimum necessary precision
    (e.g., ‘`-06`’, ‘`+05:30`’, or ‘`-04:56:02`’), or ‘`-00`’ if
    no time zone is determinable.
    This is a GNU extension.

‘`%Z`’
:   alphabetic time zone abbreviation (e.g., ‘`EDT`’), or nothing if no
    time zone is determinable. See ‘`%z`’ for how it is determined.

---

Next: [Literal conversion specifiers](#Literal-conversion-specifiers), Previous: [Time conversion specifiers](#Time-conversion-specifiers), Up: [Specifying the format of `date` output](#Date-format-specifiers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 21.1.1.2 Date conversion specifiers [¶](#Date-conversion-specifiers-1)

`date` conversion specifiers related to dates.

‘`%a`’
:   locale’s abbreviated weekday name (e.g., ‘`Sun`’)

‘`%A`’
:   locale’s full weekday name, variable length (e.g., ‘`Sunday`’)

‘`%b`’
:   locale’s abbreviated month name (e.g., ‘`Jan`’)

‘`%B`’
:   locale’s full month name, variable length (e.g., ‘`January`’)

‘`%c`’
:   locale’s date and time (e.g., ‘`Thu Mar  3 23:05:25 2020`’)

‘`%C`’
:   century. This is like ‘`%Y`’, except the last two digits are omitted.
    For example, it is ‘`20`’ if ‘`%Y`’ is ‘`2019`’,
    and is ‘`-0`’ if ‘`%Y`’ is ‘`-001`’.
    It is normally at least two characters, but it may be more.

‘`%d`’
:   day of month (e.g., ‘`01`’)

‘`%D`’
:   date; same as ‘`%m/%d/%y`’.
    Although commonly used in the US, this format is confusing elsewhere,
    and it is ambiguous for dates in different centuries.

‘`%e`’
:   day of month, space padded; same as ‘`%_d`’

‘`%F`’
:   full date in ISO 8601 format; like ‘`%+4Y-%m-%d`’
    except that any flags or field width override the ‘`+`’
    and (after subtracting 6) the ‘`4`’.
    This is a good choice for a date format, as it is standard and
    is easy to sort in the usual case where years are in the range
    0000…9999.

‘`%g`’
:   year corresponding to the ISO week number, but without the century
    (range ‘`00`’ through ‘`99`’). This has the same format and value
    as ‘`%y`’, except that if the ISO week number (see
    ‘`%V`’) belongs
    to the previous or next year, that year is used instead.
    This format is ambiguous for dates in different centuries.

‘`%G`’
:   year corresponding to the ISO week number. This has the
    same format and value as ‘`%Y`’, except that if the ISO
    week number (see
    ‘`%V`’) belongs to the previous or next year, that year is used
    instead.
    It is normally useful only if ‘`%V`’ is also used;
    for example, the format ‘`%G-%m-%d`’ is probably a mistake,
    since it combines the ISO week number year with the conventional month and day.

‘`%h`’
:   same as ‘`%b`’

‘`%j`’
:   day of year (‘`001`’…‘`366`’)

‘`%m`’
:   month (‘`01`’…‘`12`’)

‘`%q`’
:   quarter of year (‘`1`’…‘`4`’)

‘`%u`’
:   day of week (‘`1`’…‘`7`’) with ‘`1`’ corresponding to Monday

‘`%U`’
:   week number of year, with Sunday as the first day of the week
    (‘`00`’…‘`53`’).
    Days in a new year preceding the first Sunday are in week zero.

‘`%V`’
:   ISO week number, that is, the
    week number of year, with Monday as the first day of the week
    (‘`01`’…‘`53`’).
    If the week containing January 1 has four or more days in
    the new year, then it is considered week 1; otherwise, it is week 53 of
    the previous year, and the next week is week 1. (See the ISO 8601
    standard.)

‘`%w`’
:   day of week (‘`0`’…‘`6`’) with 0 corresponding to Sunday

‘`%W`’
:   week number of year, with Monday as first day of week
    (‘`00`’…‘`53`’).
    Days in a new year preceding the first Monday are in week zero.

‘`%x`’
:   locale’s date representation (e.g., ‘`12/31/99`’).
    This format can be ambiguous for dates in different centuries.

‘`%y`’
:   last two digits of year (‘`00`’…‘`99`’).
    This format is ambiguous for dates in different centuries.

‘`%Y`’
:   year. This is normally at least four characters, but it may be more.
    Year ‘`0000`’ precedes year ‘`0001`’, and year ‘`-001`’
    precedes year ‘`0000`’.

---

Next: [Padding and other flags](#Padding-and-other-flags), Previous: [Date conversion specifiers](#Date-conversion-specifiers), Up: [Specifying the format of `date` output](#Date-format-specifiers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 21.1.1.3 Literal conversion specifiers [¶](#Literal-conversion-specifiers-1)

`date` conversion specifiers that produce literal strings.

‘`%%`’
:   a literal %

‘`%n`’
:   a newline

‘`%t`’
:   a horizontal tab

---

Previous: [Literal conversion specifiers](#Literal-conversion-specifiers), Up: [Specifying the format of `date` output](#Date-format-specifiers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 21.1.1.4 Padding and other flags [¶](#Padding-and-other-flags-1)

Unless otherwise specified, `date` normally pads numeric fields
with zeros, so that, for
example, numeric months are always output as two digits.
Most numeric fields are padded on the left.
However, nanoseconds are padded on the right since they are commonly
used after decimal points in formats like ‘`%s.%-N`’.
Also, seconds since the Epoch are not padded
since there is no natural width for them.

The following optional flags can appear after the ‘`%`’:

‘`-`’
:   (hyphen) Do not pad the field; useful if the output is intended for
    human consumption.
    This is a GNU extension.
    As a special case, ‘`%-N`’ outputs only enough trailing digits to
    not lose information, assuming that the timestamp’s resolution is the
    same as the current hardware clock. For example, if the hardware
    clock resolution is 1 microsecond, ‘`%s.%-N`’ outputs something
    like ‘`1640890100.395710`’.

‘`_`’
:   (underscore) Pad with spaces; useful if you need a fixed
    number of characters in the output, but zeros are too distracting.
    This is a GNU extension.

‘`0`’
:   (zero) Pad with zeros even if the conversion specifier
    would normally pad with spaces.

‘`+`’
:   Pad with zeros, like ‘`0`’. In addition, precede any year number
    with ‘`+`’ if it exceeds 9999 or if its field width exceeds 4;
    similarly, precede any century number with ‘`+`’ if it exceeds 99
    or if its field width exceeds 2. This supports ISO 8601 formats
    for dates far in the future; for example, the command `date
    --date=12019-02-25 +%+13F` outputs the string ‘`+012019-02-25`’.

‘`^`’
:   Use upper case characters if possible.
    This is a GNU extension.

‘`#`’
:   Use opposite case characters if possible.
    A field that is normally upper case becomes lower case, and vice versa.
    This is a GNU extension.

Here are some examples of padding:

```
date +%d/%m -d "Feb 1"
⇒ 01/02
date +%-d/%-m -d "Feb 1"
⇒ 1/2
date +%_d/%_m -d "Feb 1"
⇒  1/ 2
```

You can optionally specify the field width
(after any flag, if present) as a decimal number. If the natural size of the
output of the field has less than the specified number of characters,
the result is normally written right adjusted and padded to the given
size. For example, ‘`%9B`’ prints the right adjusted month name in
a field of width 9. Nanoseconds are left adjusted, and are truncated
or padded to the field width.

An optional modifier can follow the optional flag and width
specification. The modifiers are:

‘`E`’
:   Use the locale’s alternate representation for date and time. This
    modifier applies to the ‘`%c`’, ‘`%C`’, ‘`%x`’, ‘`%X`’,
    ‘`%y`’ and ‘`%Y`’ conversion specifiers. In a Japanese locale, for
    example, ‘`%Ex`’ might yield a date format based on the Japanese
    Emperors’ reigns.

‘`O`’
:   Use the locale’s alternate numeric symbols for numbers. This modifier
    applies only to numeric conversion specifiers.

If the format supports the modifier but no alternate representation
is available, it is ignored.

POSIX specifies the behavior of flags and field widths only for
‘`%C`’, ‘`%F`’, ‘`%G`’, and ‘`%Y`’ (all without
modifiers), and requires a flag to be present if and only if a field
width is also present. Other combinations of flags, field widths and
modifiers are GNU extensions.

---

Next: [Options for `date`](#Options-for-date), Previous: [Specifying the format of `date` output](#Date-format-specifiers), Up: [`date`: Print or set system date and time](#date-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 21.1.2 Setting the time [¶](#Setting-the-time-1)

You must have appropriate privileges to set the
system clock. For changes to persist across a reboot, the
hardware clock may need to be updated from the system clock, which
might not happen automatically on your system.

To set the clock, you can use the `--set` (`-s`) option
(see [Options for `date`](#Options-for-date)). To set the clock without using GNU
extensions, you can give `date` an argument of the form
‘`MMDDhhmm[[CC]YY][.ss]`’ where each two-letter
component stands for two digits with the following meanings:

MM
:   month

DD
:   day within month

hh
:   hour

mm
:   minute

CC
:   first two digits of year (optional)

YY
:   last two digits of year (optional)

ss
:   second (optional)

The `--date` and `--set` options may not be used with an
argument in the above format. The `--universal` option may be used
with such an argument to indicate that the specified date and time are
relative to Universal Time rather than to the local time zone.

---

Next: [Examples of `date`](#Examples-of-date), Previous: [Setting the time](#Setting-the-time), Up: [`date`: Print or set system date and time](#date-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 21.1.3 Options for `date` [¶](#Options-for-date-1)

The program accepts the following options. Also see [Common options](#Common-options).
Except for `-u`, these options are all GNU extensions to POSIX.

All options that specify the date to display are mutually exclusive.
I.e.: `--date`, `--file`, `--reference`,
`--resolution`.

‘`-d datestr`’ [¶](#index-_002dd-19)

‘`--date=datestr`’
:   Display the date and time specified in datestr instead of the
    current date and time. datestr can be in almost any common
    format. It can contain month names, time zones, ‘`am`’ and ‘`pm`’,
    ‘`yesterday`’, etc. For example, `--date="2020-07-21
    14:19:13.489392193 +0530"` specifies the instant of time that is
    489,392,193 nanoseconds after July 21, 2020 at 2:19:13 PM in a
    time zone that is 5 hours and 30 minutes east of UTC.  
    The datestr must be in locale independent format. E.g., the
    ‘`LC_TIME=C`’ below is needed to print the correct date in many locales:

    ```
    date -d "$(LC_TIME=C date)"
    ```

    See [Date input formats](#Date-input-formats).

‘`--debug`’ [¶](#index-_002d_002ddebug-4)
:   Annotate the parsed date, display the effective time zone, and warn about
    potential misuse.

‘`-f datefile`’ [¶](#index-_002df-23)

‘`--file=datefile`’
:   Parse each line in datefile as with `-d` and display the
    resulting date and time. If datefile is ‘`-`’, use standard
    input. This is useful when you have many dates to process, because the
    system overhead of starting up the `date` executable many times can
    be considerable.

‘`-I[timespec]`’ [¶](#index-_002dI_005btimespec_005d)

‘`--iso-8601[=timespec]`’
:   Display the date using an ISO 8601 format, ‘`%Y-%m-%d`’.

    The argument timespec specifies the number of additional
    terms of the time to include. It can be one of the following:

    ‘`auto`’
    :   Print just the date. This is the default if timespec is omitted.
        This is like the format `%Y-%m-%d`.

    ‘`hours`’
    :   Also print hours and time zone.
        This is like the format `%Y-%m-%dT%H%:z`.

    ‘`minutes`’
    :   Also print minutes.
        This is like the format `%Y-%m-%dT%H:%M%:z`.

    ‘`seconds`’
    :   Also print seconds.
        This is like the format `%Y-%m-%dT%H:%M:%S%:z`.

    ‘`ns`’
    :   Also print nanoseconds.
        This is like the format `%Y-%m-%dT%H:%M:%S,%N%:z`.

    This format is always suitable as input
    for the `--date` (`-d`) and `--file`
    (`-f`) options, regardless of the current locale.

‘`-r file`’ [¶](#index-_002dr-15)

‘`--reference=file`’
:   Display the date and time of the last modification of file,
    instead of the current date and time.

‘`--resolution`’ [¶](#index-_002d_002dresolution)
:   Display the timestamp resolution instead of the time.
    Current clock timestamps that are output by `date`
    are integer multiples of the timestamp resolution.
    With this option, the format defaults to ‘`%s.%N`’.
    For example, if the clock resolution is 1 millisecond,
    the output is:

    ```
    0.001000000
    ```

‘`-R`’ [¶](#index-_002dR-8)

‘`--rfc-email`’
:   Display the date and time using the format ‘`%a, %d %b %Y %H:%M:%S
    %z`’, evaluated in the C locale so abbreviations are always in English.
    For example:

    ```
    Mon, 09 Jul 2020 17:00:00 -0400
    ```

    This format conforms to Internet RFCs
    [5322](https://datatracker.ietf.org/doc/rfc5322/),
    [2822](https://datatracker.ietf.org/doc/rfc2822/) and
    [822](https://datatracker.ietf.org/doc/rfc822/), the
    current and previous standards for Internet email.
    For compatibility with older versions of `date`,
    `--rfc-2822` and `--rfc-822` are aliases for
    `--rfc-email`.

‘`--rfc-3339=timespec`’ [¶](#index-_002d_002drfc_002d3339_003dtimespec)
:   Display the date using a format specified by
    [Internet
    RFC 3339](https://datatracker.ietf.org/doc/rfc3339/). This is like `--iso-8601`, except that a space rather
    than a ‘`T`’ separates dates from times, and a period rather than
    a comma separates seconds from subseconds.
    This format is always suitable as input
    for the `--date` (`-d`) and `--file`
    (`-f`) options, regardless of the current locale.

    The argument timespec specifies how much of the time to include.
    It can be one of the following:

    ‘`date`’
    :   Print just the full-date, e.g., ‘`2020-07-21`’.
        This is like the format ‘`%Y-%m-%d`’.

    ‘`seconds`’
    :   Print the full-date and full-time separated by a space, e.g.,
        ‘`2020-07-21 04:30:37+05:30`’. The output ends with a numeric
        time-offset; here the ‘`+05:30`’ means that local time is five
        hours and thirty minutes east of UTC. This is like
        the format ‘`%Y-%m-%d %H:%M:%S%:z`’.

    ‘`ns`’
    :   Like ‘`seconds`’, but also print nanoseconds, e.g.,
        ‘`2020-07-21 04:30:37.998458565+05:30`’.
        This is like the format ‘`%Y-%m-%d %H:%M:%S.%N%:z`’.

‘`-s datestr`’ [¶](#index-_002ds-28)

‘`--set=datestr`’
:   Set the date and time to datestr. See `-d` above.
    See also [Setting the time](#Setting-the-time).

‘`-u`’ [¶](#index-_002du-13)

‘`--utc`’

‘`--universal`’
:   Use Universal Time by operating as if the
    `TZ` environment variable were set to the string ‘`UTC0`’.
    UTC stands for Coordinated Universal Time, established in 1960.
    Universal Time is often called “Greenwich Mean Time” (GMT) for
    historical reasons.
    Typically, systems ignore leap seconds and thus implement an
    approximation to UTC rather than true UTC.

---

Previous: [Options for `date`](#Options-for-date), Up: [`date`: Print or set system date and time](#date-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 21.1.4 Examples of `date` [¶](#Examples-of-date-1)

Here are a few examples. Also see the documentation for the `-d`
option in the previous section.

* To print the date of the day before yesterday:

  ```
  date --date='2 days ago'
  ```
* To print the date of the day three months and one day hence:

  ```
  date --date='3 months 1 day'
  ```
* To print the day of year of Christmas in the current year:

  ```
  date --date='25 Dec' +%j
  ```
* To print the current full month name and the day of the month:

  ```
  date '+%B %d'
  ```

  But this may not be what you want because for the first nine days of
  the month, the ‘`%d`’ expands to a zero-padded two-digit field,
  for example ‘`date -d 1may '+%B %d'`’ will print ‘`May 01`’.
* To print a date without the leading zero for one-digit days
  of the month, you can use the (GNU extension)
  ‘`-`’ flag to suppress
  the padding altogether:

  ```
  date -d 1may '+%B %-d'
  ```
* To print the current date and time in the format required by many
  non-GNU versions of `date` when setting the system clock:

  ```
  date +%m%d%H%M%Y.%S
  ```
* To set the system clock forward by two minutes:

  ```
  date --set='+2 minutes'
  ```
* To print the date in Internet RFC 5322 format,
  use ‘`date --rfc-email`’. Here is some example output:

  ```
  Tue, 09 Jul 2020 19:00:37 -0400
  ```
* To convert a date string to the number of seconds since the Epoch
  (which is 1970-01-01 00:00 UTC), use the `--date` option with
  the ‘`%s`’ format. That can be useful in sorting and/or graphing
  and/or comparing data by date. The following command outputs the
  number of the seconds since the Epoch for the time two minutes after the
  Epoch:

  ```
  date --date='1970-01-01 00:02:00 +0000' +%s
  120
  ```

  To convert a date string from one time zone from to another to,
  specify ‘`TZ="from"`’ in the environment and ‘`TZ="to"`’
  in the `--date` option. See [Specifying time zone rules](#Specifying-time-zone-rules).
  For example:

  ```
  TZ="Asia/Tokyo" date --date='TZ="America/New_York" 2023-05-07 12:23'
  Mon May  8 01:23:00 JST 2023
  ```

  If you do not specify time zone information in the date string,
  `date` uses your computer’s idea of the time zone when
  interpreting the string. For example, if your computer’s time zone is
  that of Cambridge, Massachusetts, which was then 5 hours (i.e., 18,000
  seconds) behind UTC:

  ```
  # local time zone used
  date --date='1970-01-01 00:02:00' +%s
  18120
  ```
* If you’re sorting or graphing dated data, your raw date values may be
  represented as seconds since the Epoch. But few people can look at
  the date ‘`1577836800`’ and casually note “Oh, that’s the first
  second of the year 2020 in Greenwich, England.”

  ```
  date --date='2020-01-01 UTC' +%s
  1577836800
  ```

  An alternative is to use the `--utc` (`-u`) option.
  Then you may omit ‘`UTC`’ from the date string. Although this
  produces the same result for ‘`%s`’ and many other format sequences,
  with a time zone offset different from zero, it would give a different
  result for zone-dependent formats like ‘`%z`’.

  ```
  date -u --date=2020-07-21 +%s
  1595289600
  ```

  To convert such an unwieldy number of seconds back to
  a more readable form, use a command like this:

  ```
  date -d @1595289600 +"%F %T %z"
  2020-07-20 20:00:00 -0400
  ```

  Often it is better to output UTC-relative date and time:

  ```
  date -u -d @1595289600 +"%F %T %z"
  2020-07-21 00:00:00 +0000
  ```
* Typically the seconds count omits leap seconds, but some systems are
  exceptions. Because leap seconds are not predictable, the mapping
  between the seconds count and a future timestamp is not reliable on
  the atypical systems that include leap seconds in their counts.

  Here is how the two kinds of systems handle the leap second at
  the end of the year 2016:

  ```
  # Typical systems ignore leap seconds:
  date --date='2016-12-31 23:59:59 +0000' +%s
  1483228799
  date --date='2016-12-31 23:59:60 +0000' +%s
  date: invalid date '2016-12-31 23:59:60 +0000'
  date --date='2017-01-01 00:00:00 +0000' +%s
  1483228800
  ```

  ```
  # Atypical systems count leap seconds:
  date --date='2016-12-31 23:59:59 +0000' +%s
  1483228825
  date --date='2016-12-31 23:59:60 +0000' +%s
  1483228826
  date --date='2017-01-01 00:00:00 +0000' +%s
  1483228827
  ```

---

Next: [`nproc`: Print the number of available processors](#nproc-invocation), Previous: [`date`: Print or set system date and time](#date-invocation), Up: [System context](#System-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 21.2 `arch`: Print machine hardware name [¶](#arch_003a-Print-machine-hardware-name)

`arch` prints the machine hardware name,
and is equivalent to ‘`uname -m`’.
Synopsis:

```
arch [option]
```

The program accepts the [Common options](#Common-options) only.

`arch` is not installed by default, so portable scripts should
not rely on its existence.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`uname`: Print system information](#uname-invocation), Previous: [`arch`: Print machine hardware name](#arch-invocation), Up: [System context](#System-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 21.3 `nproc`: Print the number of available processors [¶](#nproc_003a-Print-the-number-of-available-processors)

Print the number of processing units available to the current process,
which may be less than the number of online processors.
If this information is not accessible, then print the number of
processors installed. If the `OMP_NUM_THREADS` or `OMP_THREAD_LIMIT`
environment variables are set, then they will determine the minimum
and maximum returned value respectively. The result is guaranteed to be
greater than zero. Synopsis:

```
nproc [option]
```

The program accepts the following options. Also see [Common options](#Common-options).

‘`--all`’ [¶](#index-_002d_002dall-6)
:   Print the number of installed processors on the system, which may
    be greater than the number online or available to the current process.
    The `OMP_NUM_THREADS` or `OMP_THREAD_LIMIT` environment variables
    are not honored in this case.

‘`--ignore=number`’ [¶](#index-_002d_002dignore)
:   If possible, exclude this number of processing units.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`hostname`: Print or set system name](#hostname-invocation), Previous: [`nproc`: Print the number of available processors](#nproc-invocation), Up: [System context](#System-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 21.4 `uname`: Print system information [¶](#uname_003a-Print-system-information)

`uname` prints information about the machine and operating system
it is run on. If no options are given, `uname` acts as if the
`-s` option were given. Synopsis:

```
uname [option]...
```

If multiple options or `-a` are given, the selected information is
printed in this order:

```
kernel-name nodename kernel-release kernel-version
machine processor hardware-platform operating-system
```

The information may contain internal spaces, so such output cannot be
parsed reliably. In the following example, kernel-version is
‘`#1 SMP Fri Jul 17 17:18:38 UTC 2020`’:

```
uname -a
⇒ Linux dumdum.example.org 5.9.16-200.fc33.x86_64 #1 SMP Mon Dec 21 14:08:22 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

The program accepts the following options. Also see [Common options](#Common-options).

‘`-a`’ [¶](#index-_002da-16)

‘`--all`’
:   Print all of the below information, except omit the processor type
    and the hardware platform name if they are unknown.

‘`-i`’ [¶](#index-_002di-18)

‘`--hardware-platform`’
:   Print the hardware platform name
    (sometimes called the hardware implementation).
    Print ‘`unknown`’ if this information is not available.
    This is non-portable, even across GNU/Linux distributions.

‘`-m`’ [¶](#index-_002dm-13)

‘`--machine`’
:   Print the machine hardware name (sometimes called the hardware class
    or hardware type).

‘`-n`’ [¶](#index-_002dn-18)

‘`--nodename`’
:   Print the network node hostname.

‘`-p`’ [¶](#index-_002dp-13)

‘`--processor`’
:   Print the processor type (sometimes called the instruction set
    architecture or ISA).
    Print ‘`unknown`’ if this information is not available.
    This is non-portable, even across GNU/Linux distributions.

‘`-o`’ [¶](#index-_002do-9)

‘`--operating-system`’
:   Print the name of the operating system.

‘`-r`’ [¶](#index-_002dr-16)

‘`--kernel-release`’
:   Print the kernel release.

‘`-s`’ [¶](#index-_002ds-29)

‘`--kernel-name`’
:   Print the kernel name.
    POSIX 1003.1-2001 (see [Standards conformance](#Standards-conformance)) calls this
    “the implementation of the operating system”, because the
    POSIX specification itself has no notion of “kernel”.
    The kernel name might be the same as the operating system name printed
    by the `-o` or `--operating-system` option, but it might
    differ. Some operating systems (e.g., FreeBSD, HP-UX) have the same
    name as their underlying kernels; others (e.g., GNU/Linux, Solaris)
    do not.

‘`-v`’ [¶](#index-_002dv-19)

‘`--kernel-version`’
:   Print the kernel version.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`hostid`: Print numeric host identifier](#hostid-invocation), Previous: [`uname`: Print system information](#uname-invocation), Up: [System context](#System-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 21.5 `hostname`: Print or set system name [¶](#hostname_003a-Print-or-set-system-name)

With no arguments, `hostname` prints the name of the current host
system. With one argument, it sets the current host name to the
specified string. You must have appropriate privileges to set the host
name. Synopsis:

```
hostname [name]
```

The only options are `--help` and `--version`. See [Common options](#Common-options).

`hostname` is not installed by default, and other packages
also supply a `hostname` command, so portable scripts should
not rely on its existence or on the exact behavior documented above.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`uptime`: Print system uptime and load](#uptime-invocation), Previous: [`hostname`: Print or set system name](#hostname-invocation), Up: [System context](#System-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 21.6 `hostid`: Print numeric host identifier [¶](#hostid_003a-Print-numeric-host-identifier)

`hostid` prints the numeric identifier of the current host
in hexadecimal. This command accepts no arguments.
The only options are `--help` and `--version`.
See [Common options](#Common-options).

For example, here’s what it prints on one system I use:

```
$ hostid
1bac013d
```

On that system, the 32-bit quantity happens to be closely
related to the system’s Internet address, but that isn’t always
the case.

`hostid` is installed only on systems that have the
`gethostid` function, so portable scripts should not rely on its
existence.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`hostid`: Print numeric host identifier](#hostid-invocation), Up: [System context](#System-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 21.7 `uptime`: Print system uptime and load [¶](#uptime_003a-Print-system-uptime-and-load)

`uptime` prints the current time, the system’s uptime, the
number of logged-in users and the current load average.

If an argument is specified, it is used as the file to be read
to discover how many users are logged in. If no argument is
specified, a system default is used (`uptime --help` indicates
the default setting).

The only options are `--help` and `--version`.
See [Common options](#Common-options).

For example, here’s what it prints right now on one system I use:

```
$ uptime
 14:07  up   3:35,  3 users,  load average: 1.39, 1.15, 1.04
```

The precise method of calculation of load average varies somewhat
between systems. Some systems calculate it as the average number of
runnable processes over the last 1, 5 and 15 minutes, but some systems
also include processes in the uninterruptible sleep state (that is,
those processes which are waiting for device I/O). The Linux kernel
includes uninterruptible processes.

`uptime` is installed only on platforms with infrastructure
for obtaining the boot time, and other packages also supply an
`uptime` command, so portable scripts should not rely on its
existence or on the exact behavior documented above.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [Modified command invocation](#Modified-command-invocation), Previous: [System context](#System-context), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 22 SELinux context [¶](#SELinux-context-1)

This section describes commands for operations with SELinux
contexts.

* [`chcon`: Change SELinux context of file](#chcon-invocation)
* [`runcon`: Run a command in specified SELinux context](#runcon-invocation)

---

Next: [`runcon`: Run a command in specified SELinux context](#runcon-invocation), Up: [SELinux context](#SELinux-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 22.1 `chcon`: Change SELinux context of file [¶](#chcon_003a-Change-SELinux-context-of-file)

`chcon` changes the SELinux security context of the selected files.
Synopses:

```
chcon [option]... context file...
chcon [option]... [-u user] [-r role] [-l range] [-t type] file...
chcon [option]... --reference=rfile file...
```

Change the SELinux security context of each file to context.
With `--reference`, change the security context of each file
to that of rfile.

The program accepts the following options. Also see [Common options](#Common-options).

‘`--dereference`’ [¶](#index-_002d_002ddereference-7)
:   Do not affect symbolic links but what they refer to; this is the default.

‘`-h`’ [¶](#index-_002dh-13)

‘`--no-dereference`’
:   Affect the symbolic links themselves instead of any referenced file.

‘`--reference=rfile`’ [¶](#index-_002d_002dreference-6)
:   Use rfile’s security context rather than specifying a context value.

‘`-R`’ [¶](#index-_002dR-9)

‘`--recursive`’
:   Operate on files and directories recursively.

‘`--preserve-root`’ [¶](#index-_002d_002dpreserve_002droot-4)
:   Refuse to operate recursively on the root directory, `/`,
    when used together with the `--recursive` option.
    See [Treating `/` specially](#Treating-_002f-specially).

‘`--no-preserve-root`’ [¶](#index-_002d_002dno_002dpreserve_002droot-4)
:   Do not treat the root directory, `/`, specially when operating
    recursively; this is the default.
    See [Treating `/` specially](#Treating-_002f-specially).

‘`-H`’ [¶](#index-_002dH-9)
:   If `--recursive` (`-R`) is specified and
    a command line argument is a symbolic link to a directory, traverse it.
    See [Traversing symlinks](#Traversing-symlinks).

‘`-L`’ [¶](#index-_002dL-13)
:   In a recursive traversal, traverse every symbolic link to a directory
    that is encountered.
    See [Traversing symlinks](#Traversing-symlinks).

‘`-P`’ [¶](#index-_002dP-11)
:   Do not traverse any symbolic links.
    See [Traversing symlinks](#Traversing-symlinks).

‘`-v`’ [¶](#index-_002dv-20)

‘`--verbose`’
:   Output a diagnostic for every file processed.

‘`-u user`’ [¶](#index-_002du-14)

‘`--user=user`’
:   Set user user in the target security context.

‘`-r role`’ [¶](#index-_002dr-17)

‘`--role=role`’
:   Set role role in the target security context.

‘`-t type`’ [¶](#index-_002dt-21)

‘`--type=type`’
:   Set type type in the target security context.

‘`-l range`’ [¶](#index-_002dl-13)

‘`--range=range`’
:   Set range range in the target security context.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Previous: [`chcon`: Change SELinux context of file](#chcon-invocation), Up: [SELinux context](#SELinux-context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 22.2 `runcon`: Run a command in specified SELinux context [¶](#runcon_003a-Run-a-command-in-specified-SELinux-context)

`runcon` runs file in specified SELinux security context.

Synopses:

```
runcon context command [args]
runcon [ -c ] [-u user] [-r role] [-t type] [-l range] command [args]
```

Run command with completely-specified context, or with
current or transitioned security context modified by one or more of level,
role, type and user.

If none of `-c`, `-t`, `-u`, `-r`, or `-l`
is specified, the first argument is used as the complete context.
Any additional arguments after command
are interpreted as arguments to the command.

With neither context nor command, print the current
security context.

The `setpriv` command can be used to set the
NO\_NEW\_PRIVS bit using `setpriv --no-new-privs runcon ...`,
thus disallowing usage of a security context with more privileges
than the process would normally have.

`runcon` accepts the following options. Also see [Common options](#Common-options).

‘`-c`’ [¶](#index-_002dc-23)

‘`--compute`’
:   Compute process transition context before modifying.

‘`-u user`’ [¶](#index-_002du-15)

‘`--user=user`’
:   Set user user in the target security context.

‘`-r role`’ [¶](#index-_002dr-18)

‘`--role=role`’
:   Set role role in the target security context.

‘`-t type`’ [¶](#index-_002dt-22)

‘`--type=type`’
:   Set type type in the target security context.

‘`-l range`’ [¶](#index-_002dl-14)

‘`--range=range`’
:   Set range range in the target security context.

Exit status:

```
125 if runcon itself fails
126 if command is found but cannot be invoked
127 if command cannot be found
the exit status of command otherwise
```

---

Next: [Process control](#Process-control), Previous: [SELinux context](#SELinux-context), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 23 Modified command invocation [¶](#Modified-command-invocation-1)

This section describes commands that run other commands in some context
different than the current one: a modified environment, as a different
user, etc.

* [`chroot`: Run a command with a different root directory](#chroot-invocation)
* [`env`: Run a command in a modified environment](#env-invocation)
* [`nice`: Run a command with modified niceness](#nice-invocation)
* [`nohup`: Run a command immune to hangups](#nohup-invocation)
* [`stdbuf`: Run a command with modified I/O stream buffering](#stdbuf-invocation)
* [`timeout`: Run a command with a time limit](#timeout-invocation)

---

Next: [`env`: Run a command in a modified environment](#env-invocation), Up: [Modified command invocation](#Modified-command-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 23.1 `chroot`: Run a command with a different root directory [¶](#chroot_003a-Run-a-command-with-a-different-root-directory)

`chroot` runs a command with a specified root directory.
On many systems, only the super-user can do this.[4](#FOOT4).
Synopses:

```
chroot option newroot [command [args]...]
chroot option
```

Ordinarily, file names are looked up starting at the root of the
directory structure, i.e., `/`. `chroot` changes the root to
the directory newroot (which must exist), then changes the working
directory to `/`, and finally runs command with optional args.
If command is not specified, the default is the value of the `SHELL`
environment variable or `/bin/sh` if not set, invoked with the
`-i` option.
command must not be a special built-in utility
(see [Special built-in utilities](#Special-built_002din-utilities)).

The program accepts the following options. Also see [Common options](#Common-options).
Options must precede operands.

‘`--groups=groups`’ [¶](#index-_002d_002dgroups-1)
:   Use this option to override the supplementary groups to be
    used by the new process.
    The items in the list (names or numeric IDs) must be separated by commas.
    Use ‘`--groups=''`’ to disable the supplementary group look-up
    implicit in the `--userspec` option.

‘`--userspec=user[:group]`’ [¶](#index-_002d_002duserspec)
:   By default, command is run with the same credentials
    as the invoking process.
    Use this option to run it as a different user and/or with a
    different primary group.
    If a user is specified then the supplementary groups
    are set according to the system defined list for that user,
    unless overridden with the `--groups` option.

‘`--skip-chdir`’ [¶](#index-_002d_002dskip_002dchdir)
:   Use this option to not change the working directory to `/` after changing
    the root directory to newroot, i.e., inside the chroot.
    This option is only permitted when newroot is the old `/` directory,
    and therefore is mostly useful together with the `--groups` and
    `--userspec` options to retain the previous working directory.

The user and group name look-up performed by the `--userspec`
and `--groups` options, is done both outside and inside
the chroot, with successful look-ups inside the chroot taking precedence.
If the specified user or group items are intended to represent a numeric ID,
then a name to ID resolving step is avoided by specifying a leading ‘`+`’.
See [chown, chgrp, chroot, id: Disambiguating user names and IDs](#Disambiguating-names-and-IDs).

Here are a few tips to help avoid common problems in using chroot.
To start with a simple example, make command refer to a statically
linked binary. If you were to use a dynamically linked executable, then
you’d have to arrange to have the shared libraries in the right place under
your new root directory.

For example, if you create a statically linked `ls` executable,
and put it in `/tmp/empty`, you can run this command as root:

```
$ chroot /tmp/empty /ls -Rl /
```

Then you’ll see output like this:

```
/:
total 1023
-rwxr-xr-x 1 0 0 1041745 Aug 16 11:17 ls
```

If you want to use a dynamically linked executable, say `bash`,
then first run ‘`ldd bash`’ to see what shared objects it needs.
Then, in addition to copying the actual binary, also copy the listed
files to the required positions under your intended new root directory.
Finally, if the executable requires any other files (e.g., data, state,
device files), copy them into place, too.

`chroot` is installed only on systems that have the
`chroot` function, so portable scripts should not rely on its
existence.

Exit status:

```
125 if chroot itself fails
126 if command is found but cannot be invoked
127 if command cannot be found
the exit status of command otherwise
```

---

Next: [`nice`: Run a command with modified niceness](#nice-invocation), Previous: [`chroot`: Run a command with a different root directory](#chroot-invocation), Up: [Modified command invocation](#Modified-command-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]
