### 23.2 `env`: Run a command in a modified environment [¶](#env_003a-Run-a-command-in-a-modified-environment)

`env` runs a command with a modified environment. Synopses:

```
env [option]... [name=value]... [command [args]...]
env -[v]S'[option]... [name=value]... [command [args]...]'
env
```

`env` is commonly used on first line of scripts (shebang line):

```
#!/usr/bin/env command
#!/usr/bin/env -[v]S[option]... [name=value]... command [args]...
```

Operands of the form ‘`variable=value`’ set
the environment variable variable to value value.
value may be empty (‘`variable=`’). Setting a variable
to an empty value is different from unsetting it.
These operands are evaluated left-to-right, so if two operands
mention the same variable the earlier is ignored.

Environment variable names can be empty, and can contain any
characters other than ‘`=`’ and ASCII NUL.
However, it is wise to limit yourself to names that
consist solely of underscores, digits, and ASCII letters,
and that begin with a non-digit, as applications like the shell do not
work well with other names.

The first operand that does not contain the character ‘`=`’
specifies the program to invoke; it is
searched for according to the `PATH` environment variable. Any
remaining arguments are passed as arguments to that program.
The program should not be a special built-in utility
(see [Special built-in utilities](#Special-built_002din-utilities)).

Modifications to `PATH` take effect prior to searching for
command. Use caution when reducing `PATH`; behavior is
not portable when `PATH` is undefined or omits key directories
such as `/bin`.

In the rare case that a utility contains a ‘`=`’ in the name, the
only way to disambiguate it from a variable assignment is to use an
intermediate command for command, and pass the problematic
program name via args. For example, if `./prog=` is an
executable in the current `PATH`:

```
env prog= true # runs 'true', with prog= in environment
env ./prog= true # runs 'true', with ./prog= in environment
env -- prog= true # runs 'true', with prog= in environment
env sh -c '\prog= true' # runs 'prog=' with argument 'true'
env sh -c 'exec "$@"' sh prog= true # also runs 'prog='
```

If no command name is specified following the environment
specifications, the resulting environment is printed. This is like
specifying the `printenv` program.

For some examples, suppose the environment passed to `env`
contains ‘`LOGNAME=rms`’, ‘`EDITOR=emacs`’, and
‘`PATH=.:/gnubin:/hacks`’:

* Output the current environment.

  ```
  $ env | LC_ALL=C sort
  EDITOR=emacs
  LOGNAME=rms
  PATH=.:/gnubin:/hacks
  ```
* Run `foo` with a reduced environment, preserving only the
  original `PATH` to avoid problems in locating `foo`.

  ```
  env - PATH="$PATH" foo
  ```
* Run `foo` with the environment containing ‘`LOGNAME=rms`’,
  ‘`EDITOR=emacs`’, and ‘`PATH=.:/gnubin:/hacks`’, and guarantees
  that `foo` was found in the file system rather than as a shell
  built-in.

  ```
  env foo
  ```
* Run `nemacs` with the environment containing ‘`LOGNAME=foo`’,
  ‘`EDITOR=emacs`’, ‘`PATH=.:/gnubin:/hacks`’, and
  ‘`DISPLAY=gnu:0`’.

  ```
  env DISPLAY=gnu:0 LOGNAME=foo nemacs
  ```
* Attempt to run the program `/energy/--` (as that is the only
  possible path search result); if the command exists, the environment
  will contain ‘`LOGNAME=rms`’ and ‘`PATH=/energy`’, and the
  arguments will be ‘`e=mc2`’, ‘`bar`’, and ‘`baz`’.

  ```
  env -u EDITOR PATH=/energy -- e=mc2 bar baz
  ```

* [General options](#General-options-2)
* [`-S`/`--split-string` usage in scripts](#g_t_002dS_002f_002d_002dsplit_002dstring-usage-in-scripts)
* [`-S`/`--split-string` syntax](#g_t_002dS_002f_002d_002dsplit_002dstring-syntax)

#### 23.2.1 General options [¶](#General-options-2)

The program accepts the following options. Also see [Common options](#Common-options).
Options must precede operands.

‘`-0`’ [¶](#index-_002d0-2)

‘`--null`’
:   Output a zero byte (ASCII NUL) at the end of each line,
    rather than a newline. This option enables other programs to parse the
    output even when that output would contain data with embedded newlines.

‘`-a arg`’ [¶](#index-_002da-17)

‘`--argv0=arg`’
:   Override the zeroth argument passed to the command being executed.
    Without this option a default value of command is used.

‘`-u name`’ [¶](#index-_002du-16)

‘`--unset=name`’
:   Remove variable name from the environment, if it was in the
    environment.

‘`-`’ [¶](#index-_002d-1)

‘`-i`’

‘`--ignore-environment`’
:   Start with an empty environment, ignoring the inherited environment.

‘`-C dir`’ [¶](#index-_002dC-4)

‘`--chdir=dir`’
:   Change the working directory to dir before invoking command.
    This differs from the shell built-in `cd` in that it starts
    command as a subprocess rather than altering the shell’s own working
    directory; this allows it to be chained with other commands that run commands
    in a different context. For example:

    ```
    # Run 'true' with /chroot as its root directory and /srv as its working
    # directory.
    chroot /chroot env --chdir=/srv true
    # Run 'true' with /build as its working directory, FOO=bar in its
    # environment, and a time limit of five seconds.
    env --chdir=/build FOO=bar timeout 5 true
    ```

‘`--default-signal[=sig]`’
:   Unblock and reset signal sig to its default signal handler.
    Without sig all known signals are unblocked and reset to their defaults.
    Multiple signals can be comma-separated. An empty sig argument is a no-op.
    The following command runs `seq` with SIGINT and SIGPIPE set to their
    default (which is to terminate the program):

    ```
    env --default-signal=PIPE,INT seq 1000 | head -n1
    ```

    In the following example, we see how this is not
    possible to do with traditional shells.
    Here the first trap command sets SIGPIPE to ignore.
    The second trap command ostensibly sets it back to its default,
    but POSIX mandates that the shell must not change inherited
    state of the signal – so it is a no-op.

    ```
    trap '' PIPE && sh -c 'trap - PIPE ; seq inf | head -n1'
    ```

    Using `--default-signal=PIPE` we can
    ensure the signal handling is set to its default behavior:

    ```
    trap '' PIPE && sh -c 'env --default-signal=PIPE seq inf | head -n1'
    ```

‘`--ignore-signal[=sig]`’
:   Ignore signal sig when running a program. Without sig all
    known signals are set to ignore. Multiple signals can be comma-separated.
    An empty sig argument is a no-op. The following command runs `seq`
    with SIGINT set to be ignored – pressing `Ctrl-C` will not terminate it:

    ```
    env --ignore-signal=INT seq inf > /dev/null
    ```

    ‘`SIGCHLD`’ is special, in that `--ignore-signal=CHLD` might have
    no effect (POSIX says it’s unspecified).

    Most operating systems do not allow ignoring ‘`SIGKILL`’, ‘`SIGSTOP`’
    (and possibly other signals). Attempting to ignore these signals will fail.

    Multiple (and contradictory) `--default-signal=SIG` and
    `--ignore-signal=SIG` options are processed left-to-right,
    with the latter taking precedence. In the following example, ‘`SIGPIPE`’ is
    set to default while ‘`SIGINT`’ is ignored:

    ```
    env --default-signal=INT,PIPE --ignore-signal=INT
    ```

‘`--block-signal[=sig]`’
:   Block signal(s) sig from being delivered. Without sig all
    known signals are set to blocked. Multiple signals can be comma-separated.
    An empty sig argument is a no-op.

‘`--list-signal-handling`’
:   List blocked or ignored signals to standard error, before executing a command.

‘`-v`’ [¶](#index-_002dv-21)

‘`--debug`’
:   Show verbose information for each processing step.

    ```
    $ env -v -uTERM A=B uname -s
    unset:    TERM
    setenv:   A=B
    executing: uname
       arg[0]= 'uname'
       arg[1]= '-s'
    Linux
    ```

    When combined with `-S` it is recommended to list `-v`
    first, e.g. `env -vS'string'`.

‘`-S string`’ [¶](#index-_002dS-12)

‘`--split-string=string`’
:   process and split string into separate arguments used to pass
    multiple arguments on shebang lines. `env` supports FreeBSD’s
    syntax of several escape sequences and environment variable
    expansions. See below for details and examples.

Exit status:

```
0   if no command is specified and the environment is output
125 if env itself fails
126 if command is found but cannot be invoked
127 if command cannot be found
the exit status of command otherwise
```

#### 23.2.2 `-S`/`--split-string` usage in scripts [¶](#g_t_002dS_002f_002d_002dsplit_002dstring-usage-in-scripts)

The `-S`/`--split-string` option enables use of multiple
arguments on the first line of scripts (the shebang line, ‘`#!`’).

When a script’s interpreter is in a known location, scripts typically
contain the absolute file name in their first line:

|  |  |
| --- | --- |
| Shell script: | ``` #!/bin/sh echo hello ``` |
| Perl script: | ``` #!/usr/bin/perl print "hello\n"; ``` |
| Python script: | ``` #!/usr/bin/python3 print("hello") ``` |

When a script’s interpreter is in a non-standard location
in the `PATH` environment variable, it is recommended
to use `env` on the first line of the script to
find the executable and run it:

|  |  |
| --- | --- |
| Shell script: | ``` #!/usr/bin/env bash echo hello ``` |
| Perl script: | ``` #!/usr/bin/env perl print "hello\n"; ``` |
| Python script: | ``` #!/usr/bin/env python3 print("hello") ``` |

Most operating systems (e.g. GNU/Linux, BSDs) treat all text after the
first space as a single argument. When using `env` in a script
it is thus not possible to specify multiple arguments.

In the following example:

```
#!/usr/bin/env perl -T -w
print "hello\n";
```

The operating system treats ‘`perl -T -w`’ as one argument (the
program’s name), and executing the script fails with:

```
/usr/bin/env: 'perl -T -w': No such file or directory
```

The `-S` option instructs `env` to split the single string
into multiple arguments. The following example works as expected:

```
$ cat hello.pl
#!/usr/bin/env -S perl -T -w
print "hello\n";

$ chmod a+x hello.pl
$ ./hello.pl
hello
```

And is equivalent to running `perl -T -w hello.pl` on the command line
prompt.

* [Testing and troubleshooting](#Testing-and-troubleshooting)

#### Testing and troubleshooting [¶](#Testing-and-troubleshooting)

To test `env -S` on the command line, use single quotes for the
`-S` string to emulate a single parameter. Single quotes are not
needed when using `env -S` in a shebang line on the first line of a
script (the operating system already treats it as one argument).

The following command is equivalent to the `hello.pl` script above:

```
$ env -S'perl -T -w' hello.pl
```

To troubleshoot `-S` usage add the `-v` as the first
argument (before `-S`).

Using `-vS` on a shebang line in a script:

```
$ cat hello-debug.pl
#!/usr/bin/env -vS perl -T -w
print "hello\n";

$ chmod a+x hello-debug.pl
$ ./hello-debug.pl
split -S:  'perl -T -w'
 into:    'perl'
     &    '-T'
     &    '-w'
executing: perl
   arg[0]= 'perl'
   arg[1]= '-T'
   arg[2]= '-w'
   arg[3]= './hello-debug.pl'
hello
```

Using `-vS` on the command line prompt (adding single quotes):

```
$ env -vS'perl -T -w' hello-debug.pl
split -S:  'perl -T -w'
 into:    'perl'
     &    '-T'
     &    '-w'
executing: perl
   arg[0]= 'perl'
   arg[1]= '-T'
   arg[2]= '-w'
   arg[3]= 'hello-debug.pl'
hello
```

#### 23.2.3 `-S`/`--split-string` syntax [¶](#g_t_002dS_002f_002d_002dsplit_002dstring-syntax)

* [Splitting arguments by whitespace](#Splitting-arguments-by-whitespace)
* [Escape sequences](#Escape-sequences)
* [Comments](#Comments)
* [Environment variable expansion](#Environment-variable-expansion)

#### Splitting arguments by whitespace [¶](#Splitting-arguments-by-whitespace)

Running `env -Sstring` splits the string into
arguments based on unquoted spaces or tab characters.
(Newlines, carriage returns, vertical tabs and form feeds are treated
like spaces and tabs.)

In the following contrived example the `awk` variable
‘`OFS`’ will be `<space>xyz<space>` as these spaces are inside
double quotes. The other space characters are used as argument separators:

```
$ cat one.awk
#!/usr/bin/env -S awk -v OFS=" xyz " -f
BEGIN {print 1,2,3}

$ chmod a+x one.awk
$ ./one.awk
1 xyz 2 xyz 3
```

When using `-S` on the command line prompt, remember to add
single quotes around the entire string:

```
$ env -S'awk -v OFS=" xyz " -f' one.awk
1 xyz 2 xyz 3
```

#### Escape sequences [¶](#Escape-sequences)

`env` supports several escape sequences. These sequences
are processed when unquoted or inside double quotes (unless otherwise noted).
Single quotes disable escape sequences except ‘`\'`’ and ‘`\\`’.

|  |  |
| --- | --- |
| `\c` | Ignore the remaining characters in the string. Cannot be used inside double quotes. |
| `\f` | form-feed character (ASCII 0x0C) |
| `\n` | new-line character (ASCII 0x0A) |
| `\r` | carriage-return character (ASCII 0x0D) |
| `\t` | tab character (ASCII 0x09) |
| `\v` | vertical tab character (ASCII 0x0B) |
| `\#` | A hash ‘`#`’ character. Used when a ‘`#`’ character is needed as the first character of an argument (see ’comments’ section below). |
| `\$` | A dollar-sign character ‘`$`’. Unescaped ‘`$`’ characters are used to expand environment variables (see ’variables’ section below). |
| `\_` | Inside double-quotes, replaced with a single space character. Outside quotes, treated as an argument separator. ‘`\_`’ can be used to avoid space characters in a shebang line (see examples below). |
| `\"` | A double-quote character. |
| `\'` | A single-quote character. This escape sequence works inside single-quoted strings. |
| `\\` | A backslash character. This escape sequence works inside single-quoted strings. |

The following `awk` script will use tab character as input and output
field separator (instead of spaces and tabs):

```
$ cat tabs.awk
#!/usr/bin/env -S awk -v FS="\t" -v OFS="\t" -f
...
```

#### Comments [¶](#Comments)

The escape sequence ‘`\c`’ (used outside single/double quotes)
causes `env` to ignore the rest of the string.

The ‘`#`’ character causes `env` to ignore the rest of
the string when it appears as the first character of an argument.
Use ‘`\#`’ to reverse this behavior.

```
$ env -S'printf %s\n A B C'
A
B
C

$ env -S'printf %s\n A# B C'
A#
B
C

$ env -S'printf %s\n A #B C'
A

$ env -S'printf %s\n A \#B C'
A
#B
C

$ env -S'printf %s\n A\cB C'
A
```

The above examples use single quotes as they are executed
on the command-line.

#### Environment variable expansion [¶](#Environment-variable-expansion)

The pattern ‘`${VARNAME}`’ is used to substitute a value from
the environment variable. The pattern must include the curly braces
(‘`{`’,‘`}`’). Without them `env` will reject the string.
Special shell variables (such as ‘`$@`’, ‘`$*`’, ‘`$$`’) are
not supported.

If the environment variable is empty or not set, the pattern will be replaced
by an empty string. The value of ‘`${VARNAME}`’ will be that of
the executed `env`, before any modifications using
`-i`/`--ignore-environment`/`-u`/`--unset` or
setting new values using ‘`VAR=VALUE`’.

The following python script prepends `/opt/custom/modules` to the python
module search path environment variable (‘`PYTHONPATH`’):

```
$ cat custom.py
#!/usr/bin/env -S PYTHONPATH=/opt/custom/modules/:${PYTHONPATH} python
print "hello"
...
```

The expansion of ‘`${PYTHONPATH}`’ is performed by `env`,
not by a shell. If the curly braces are omitted, `env` will fail:

```
$ cat custom.py
#!/usr/bin/env -S PYTHONPATH=/opt/custom/modules/:$PYTHONPATH python
print "hello"
...

$ chmod a+x custom.py
$ custom.py
/usr/bin/env: only ${VARNAME} expansion is supported, error at: $PYTHONPATH python
```

Environment variable expansion happens before clearing the environment
(with `-i`) or unsetting specific variables (with `-u`):

```
$ env -S'-i OLDUSER=${USER} env'
OLDUSER=gordon
```

Use `-v` to diagnose the operations step-by-step:

```
$ env -vS'-i OLDUSER=${USER} env'
expanding ${USER} into 'gordon'
split -S:  '-i OLDUSER=${USER} env'
 into:    '-i'
     &    'OLDUSER=gordon'
     &    'env'
cleaning environ
setenv:   OLDUSER=gordon
executing: env
   arg[0]= 'env'
OLDUSER=gordon
```

---

Next: [`nohup`: Run a command immune to hangups](#nohup-invocation), Previous: [`env`: Run a command in a modified environment](#env-invocation), Up: [Modified command invocation](#Modified-command-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 23.3 `nice`: Run a command with modified niceness [¶](#nice_003a-Run-a-command-with-modified-niceness)

`nice` prints a process’s *niceness*, or runs
a command with modified niceness. *niceness* affects how
favorably the process is scheduled in the system.
Synopsis:

```
nice [option]... [command [arg]...]
```

If no arguments are given, `nice` prints the current niceness.
Otherwise, `nice` runs the given command with its
niceness adjusted. By default, its niceness is incremented by 10.

Niceness values range at least from −20 (process has high priority
and gets more resources, thus slowing down other processes) through 19
(process has lower priority and runs slowly itself, but has less impact
on the speed of other running processes). Some systems
may have a wider range of niceness values; conversely, other systems may
enforce more restrictive limits. An attempt to set the niceness
outside the supported range is treated as an attempt to use the
minimum or maximum supported value.

A niceness should not be confused with a scheduling priority, which
lets applications determine the order in which threads are scheduled
to run. Unlike a priority, a niceness is merely advice to the
scheduler, which the scheduler is free to ignore. Also, as a point of
terminology, POSIX defines the behavior of `nice` in
terms of a *nice value*, which is the non-negative difference
between a niceness and the minimum niceness. Though `nice`
conforms to POSIX, its documentation and diagnostics use the
term “niceness” for compatibility with historical practice.

command must not be a special built-in utility (see [Special built-in utilities](#Special-built_002din-utilities)).

Due to shell aliases and built-in `nice` functions, using an
unadorned `nice` interactively or in a script may get you
different functionality than that described here. Invoke it via
`env` (i.e., `env nice …`) to avoid interference
from the shell.

To change the *niceness* of an existing process,
one needs to use the `renice` command.

The program accepts the following option. Also see [Common options](#Common-options).
Options must precede operands.

‘`-n adjustment`’ [¶](#index-_002dn-19)

‘`--adjustment=adjustment`’
:   Add adjustment instead of 10 to the command’s niceness. If
    adjustment is negative and you lack appropriate privileges,
    `nice` issues a warning but otherwise acts as if you specified
    a zero adjustment.

    For compatibility `nice` also supports an obsolete
    option syntax `-adjustment`. New scripts should use
    `-n adjustment` instead.

`nice` is installed only on systems that have the POSIX
`setpriority` function, so portable scripts should not rely on
its existence on non-POSIX platforms.

Exit status:

```
0   if no command is specified and the niceness is output
125 if nice itself fails
126 if command is found but cannot be invoked
127 if command cannot be found
the exit status of command otherwise
```

It is sometimes useful to run a non-interactive program with reduced niceness.

```
$ nice factor 4611686018427387903
```

Since `nice` prints the current niceness,
you can invoke it through itself to demonstrate how it works.

The default behavior is to increase the niceness by ‘`10`’:

```
$ nice
0
$ nice nice
10
$ nice -n 10 nice
10
```

The adjustment is relative to the current niceness. In the
next example, the first `nice` invocation runs the second one
with niceness 10, and it in turn runs the final one with a niceness
that is 3 more:

```
$ nice nice -n 3 nice
13
```

Specifying a niceness larger than the supported range
is the same as specifying the maximum supported value:

```
$ nice -n 10000000000 nice
19
```

Only a privileged user may run a process with lower niceness:

```
$ nice -n -1 nice
nice: cannot set niceness: Permission denied
0
$ sudo nice -n -1 nice
-1
```

---

Next: [`stdbuf`: Run a command with modified I/O stream buffering](#stdbuf-invocation), Previous: [`nice`: Run a command with modified niceness](#nice-invocation), Up: [Modified command invocation](#Modified-command-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 23.4 `nohup`: Run a command immune to hangups [¶](#nohup_003a-Run-a-command-immune-to-hangups)

`nohup` runs the given command with hangup signals ignored,
so that the command can continue running in the background after you log
out. Synopsis:

```
nohup command [arg]...
```

If standard input is a terminal, redirect it so that terminal sessions
do not mistakenly consider the terminal to be used by the command.
Make the substitute file descriptor unreadable, so that commands that
mistakenly attempt to read from standard input can report an error.
This redirection is a GNU extension; programs intended to be portable
to non-GNU hosts can use ‘`nohup command [arg]…
0>/dev/null`’ instead.

If standard output is a terminal, the command’s standard output is appended
to the file `nohup.out`; if that cannot be written to, it is appended
to the file `$HOME/nohup.out`; and if that cannot be written to, the
command is not run.
Any `nohup.out` or `$HOME/nohup.out` file created by
`nohup` is made readable and writable only to the user,
regardless of the current umask settings.

If standard error is a terminal, it is normally redirected to the same file
descriptor as the (possibly-redirected) standard output.
However, if standard output is closed, standard error terminal output
is instead appended to the file `nohup.out` or
`$HOME/nohup.out` as above.

To capture the command’s output to a file other than `nohup.out`
you can redirect it. For example, to capture the output of
`make`:

```
nohup make > make.log
```

`nohup` does not automatically put the command it runs in the
background; you must do that explicitly, by ending the command line
with an ‘`&`’. Also, `nohup` does not alter the
niceness of command; use `nice` for that,
e.g., ‘`nohup nice command`’.

command must not be a special built-in utility (see [Special built-in utilities](#Special-built_002din-utilities)).

The only options are `--help` and `--version`. See [Common options](#Common-options). Options must precede operands.

Exit status:

```
125 if nohup itself fails, and POSIXLY_CORRECT is not set
126 if command is found but cannot be invoked
127 if command cannot be found
the exit status of command otherwise
```

If `POSIXLY_CORRECT` is set, internal failures give status 127
instead of 125.

---

Next: [`timeout`: Run a command with a time limit](#timeout-invocation), Previous: [`nohup`: Run a command immune to hangups](#nohup-invocation), Up: [Modified command invocation](#Modified-command-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 23.5 `stdbuf`: Run a command with modified I/O stream buffering [¶](#stdbuf_003a-Run-a-command-with-modified-I_002fO-stream-buffering)

`stdbuf` allows one to modify the buffering operations of the
three standard I/O streams associated with a program. Synopsis:

```
stdbuf option... command
```

command must start with the name of a program that

1. uses the ISO C `FILE` streams for input/output, and
2. does not adjust the buffering of its standard streams.

Not every command operates in this way.
For example, `dd` does not use `FILE` streams,
and `tee` adjusts its streams’ buffering.

Any additional args are passed as additional arguments to the
command.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-i mode`’ [¶](#index-_002di-20)

‘`--input=mode`’
:   Adjust the standard input stream buffering.

‘`-o mode`’ [¶](#index-_002do-10)

‘`--output=mode`’
:   Adjust the standard output stream buffering.

‘`-e mode`’ [¶](#index-_002de-8)

‘`--error=mode`’
:   Adjust the standard error stream buffering.

The mode can be specified as follows:

‘`L`’
:   Set the stream to line buffered mode.
    In this mode data is coalesced until a newline is output or
    input is read from any stream attached to a terminal device.
    This option is invalid with standard input.

‘`0`’
:   Disable buffering of the selected stream.
    In this mode, data is output immediately and only the
    amount of data requested is read from input.
    Disabling buffering for input does not necessarily influence the responsiveness
    or blocking behavior of the stream input functions.
    For example, `fread` will still block until `EOF` or error
    or the amount requested is read,
    even if the underlying `read` returns less data than requested.

‘`size`’
:   Specify the size of the buffer to use in fully buffered mode.
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

`stdbuf` is installed only on platforms that use the
Executable and Linkable Format (ELF) and support the
`constructor` attribute, so portable scripts should not rely on
its existence.

Exit status:

```
125 if stdbuf itself fails
126 if command is found but cannot be invoked
127 if command cannot be found
the exit status of command otherwise
```

---

Previous: [`stdbuf`: Run a command with modified I/O stream buffering](#stdbuf-invocation), Up: [Modified command invocation](#Modified-command-invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 23.6 `timeout`: Run a command with a time limit [¶](#timeout_003a-Run-a-command-with-a-time-limit)

`timeout` runs the given command and kills it if it is
still running after the specified time interval. Synopsis:

```
timeout [option] duration command [arg]...
```

command must not be a special built-in utility (see [Special built-in utilities](#Special-built_002din-utilities)).

The program accepts the following options. Also see [Common options](#Common-options).
Options must precede operands.

‘`-f`’ [¶](#index-_002df-24)

‘`--foreground`’
:   Don’t create a separate background program group, so that
    the managed command can use the foreground TTY normally.
    This is needed to support two situations when timing out commands,
    when not invoking `timeout` from an interactive shell.

    1. command is interactive and needs to read from the terminal for example
    2. the user wants to support sending signals directly to command
       from the terminal (like Ctrl-C for example)

    In this mode of operation, any children of command
    will not be timed out. Also SIGCONT will not be sent to command,
    as it’s generally not needed with foreground processes, and can
    cause intermittent signal delivery issues with programs that are monitors
    themselves (like GDB for example).

‘`-k duration`’ [¶](#index-_002dk-7)

‘`--kill-after=duration`’
:   Ensure the monitored command is killed by also sending a ‘`KILL`’
    signal.

    The specified duration starts from the point in time when
    `timeout` sends the initial signal to command, i.e.,
    not from the beginning when the command is started.

    This option has no effect if either the main duration
    of the `timeout` command, or the duration specified
    to this option, is 0.

    This option may be useful if the selected signal did not kill the command,
    either because the signal was blocked or ignored, or if the command takes
    too long (e.g. for cleanup work) to terminate itself within a certain amount
    of time.

‘`-p`’ [¶](#index-_002dp-14)

‘`--preserve-status`’
:   Return the exit status of the managed command on timeout, rather than
    a specific exit status indicating a timeout. This is useful if the
    managed command supports running for an indeterminate amount of time.

‘`-s signal`’ [¶](#index-_002ds-30)

‘`--signal=signal`’
:   Send this signal to command on timeout, rather than the
    default ‘`TERM`’ signal. signal may be a name like ‘`HUP`’
    or a number. See [Signal specifications](#Signal-specifications).

‘`-v`’ [¶](#index-_002dv-22)

‘`--verbose`’
:   Diagnose to standard error, any signal sent upon timeout.

duration is a floating point number in either the current or the
C locale (see [Floating point numbers](#Floating-point)) followed by an optional unit:

```
‘s’ for seconds (the default)
‘m’ for minutes
‘h’ for hours
‘d’ for days
```

A hexadecimal number can precede a ‘`d`’ suffix only if the number
has a ‘`p`’ style exponent, e.g., ‘`0x1p0d`’ means one day.
A duration of 0 disables the associated timeout.
The actual timeout duration is dependent on system conditions,
which should be especially considered when specifying sub-second timeouts.

Exit status:

```
124 if command times out, and --preserve-status is not specified
125 if timeout itself fails
126 if command is found but cannot be invoked
127 if command cannot be found
137 if command or timeout is sent the KILL(9) signal (128+9)
the exit status of command otherwise
```

In the case of the ‘`KILL(9)`’ signal, `timeout` returns with
exit status 137, regardless of whether that signal is sent to command
or to `timeout` itself, i.e., these cases cannot be distinguished.
In the latter case, the command process may still be alive after
`timeout` has forcefully been terminated.

Examples:

```
# Send the default TERM signal after 20s to a short-living 'sleep 1'.
# As that terminates long before the given duration, 'timeout' returns
# with the same exit status as the command, 0 in this case.
timeout 20 sleep 1

# Send the INT signal after 5s to the 'sleep' command.  Returns after
# 5 seconds with exit status 124 to indicate the sending of the signal.
timeout -s INT 5 sleep 20

# Likewise, but the command ignoring the INT signal due to being started
# via 'env --ignore-signal'.  Thus, 'sleep' terminates regularly after
# the full 20 seconds, still 'timeout' returns with exit status 124.
timeout -s INT 5s env --ignore-signal=INT sleep 20

# Likewise, but sending the KILL signal 3 seconds after the initial
# INT signal.  Hence, 'sleep' is forcefully terminated after about
# 8 seconds (5+3), and 'timeout' returns with an exit status of 137.
timeout -s INT -k 3s 5s env --ignore-signal=INT sleep 20
```

---

Next: [Delaying](#Delaying), Previous: [Modified command invocation](#Modified-command-invocation), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 24 Process control [¶](#Process-control-1)

* [`kill`: Send a signal to processes](#kill-invocation)

---

Up: [Process control](#Process-control)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 24.1 `kill`: Send a signal to processes [¶](#kill_003a-Send-a-signal-to-processes)

The `kill` command sends a signal to processes, causing them
to terminate or otherwise act upon receiving the signal in some way.
Alternatively, it lists information about signals. Synopses:

```
kill [-s signal | --signal signal | -signal] pid...
kill [-l | --list | -t | --table] [signal]...
```

Due to shell aliases and built-in `kill` functions, using an
unadorned `kill` interactively or in a script may get you
different functionality than that described here. Invoke it via
`env` (i.e., `env kill …`) to avoid interference
from the shell.

The first form of the `kill` command sends a signal to all
pid arguments. The default signal to send if none is specified
is ‘`TERM`’. The special signal number ‘`0`’ does not denote a
valid signal, but can be used to test whether the pid arguments
specify processes to which a signal could be sent.

If pid is positive, the signal is sent to the process with the
process ID pid. If pid is zero, the signal is sent to all
processes in the process group of the current process. If pid
is −1, the signal is sent to all processes for which the user has
permission to send a signal. If pid is less than −1, the signal
is sent to all processes in the process group that equals the absolute
value of pid.

If pid is not positive, a system-dependent set of system
processes is excluded from the list of processes to which the signal
is sent.

If a negative pid argument is desired as the first one, it
should be preceded by `--`. However, as a common extension to
POSIX, `--` is not required with ‘`kill
-signal -pid`’. The following commands are equivalent:

```
kill -15 -1
kill -TERM -1
kill -s TERM -- -1
kill -- -1
```

The first form of the `kill` command succeeds if every pid
argument specifies at least one process that the signal was sent to.

The second form of the `kill` command lists signal information.
Either the `-l` or `--list` option, or the `-t`
or `--table` option must be specified. Without any
signal argument, all supported signals are listed. The output
of `-l` or `--list` is a list of the signal names, one
per line; if signal is already a name, the signal number is
printed instead. The output of `-t` or `--table` is a
table of signal numbers, names, and descriptions. This form of the
`kill` command succeeds if all signal arguments are valid
and if there is no output error.

The `kill` command also supports the `--help` and
`--version` options. See [Common options](#Common-options).

A signal may be a signal name like ‘`HUP`’, or a signal
number like ‘`1`’, or an exit status of a process terminated by the
signal. A signal name can be given in canonical form or prefixed by
‘`SIG`’. The case of the letters is ignored, except for the
`-signal` option which must use upper case to avoid
ambiguity with lower case option letters.
See [Signal specifications](#Signal-specifications), for a list of supported
signal names and numbers.

---

Next: [Numeric operations](#Numeric-operations), Previous: [Process control](#Process-control), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 25 Delaying [¶](#Delaying-1)

* [`sleep`: Delay for a specified time](#sleep-invocation)

---

Up: [Delaying](#Delaying)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 25.1 `sleep`: Delay for a specified time [¶](#sleep_003a-Delay-for-a-specified-time)

`sleep` pauses for an amount of time specified by the sum of
the values of the command line arguments.
Synopsis:

```
sleep number[smhd]...
```

Each argument is a non-negative number followed by an optional unit; the default
is seconds. The units are:

‘`s`’
:   seconds

‘`m`’
:   minutes

‘`h`’
:   hours

‘`d`’
:   days

Although portable POSIX scripts must give `sleep` a single
non-negative integer argument without a suffix, GNU `sleep`
also accepts two or more arguments, unit suffixes, and floating-point
numbers in either the current or the C locale. See [Floating point numbers](#Floating-point).

A hexadecimal number can precede a ‘`d`’ suffix only if the number
has a ‘`p`’ style exponent, e.g., ‘`0x1p0d`’ means one day.

For instance, the following could be used to `sleep` for
1 second, 234 milli-, 567 micro- and 890 nanoseconds:

```
sleep 1234e-3 567.89e-6
```

Also one could sleep indefinitely like:

```
sleep inf
```

The only options are `--help` and `--version`. See [Common options](#Common-options).

Due to shell aliases and built-in `sleep` functions, using an
unadorned `sleep` interactively or in a script may get you
different functionality than that described here. Invoke it via
`env` (i.e., `env sleep …`) to avoid interference
from the shell.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [File permissions](#File-permissions), Previous: [Delaying](#Delaying), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 26 Numeric operations [¶](#Numeric-operations-1)

These programs do numerically-related operations.

* [`factor`: Print prime factors](#factor-invocation)
* [`numfmt`: Reformat numbers](#numfmt-invocation)
* [`seq`: Print numeric sequences](#seq-invocation)

---

Next: [`numfmt`: Reformat numbers](#numfmt-invocation), Up: [Numeric operations](#Numeric-operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 26.1 `factor`: Print prime factors [¶](#factor_003a-Print-prime-factors)

`factor` prints prime factors. Synopsis:

```
factor [option]... [number]...
```

If no number is specified on the command line, `factor` reads
numbers from standard input, delimited by newlines, tabs, or spaces.

The program accepts the following options. Also see [Common options](#Common-options).

‘`-h`’ [¶](#index-_002dh-14)

‘`--exponents`’
:   print factors in the form *p^e*, rather than repeating
    the prime ‘`p`’, ‘`e`’ times. If the exponent ‘`e`’ is 1,
    then it is omitted.

    ```
    $ factor --exponents 3000
    3000: 2^3 3 5^3
    ```

If the number to be factored is small (less than *2^{127}* on
typical machines), `factor` uses a faster algorithm.
For example, on a circa-2017 Intel Xeon Silver 4116, factoring the
product of the eighth and ninth Mersenne primes (approximately
*2^{92}*) takes about 4 ms of CPU time:

```
$ M8=$(echo 2^31-1 | bc)
$ M9=$(echo 2^61-1 | bc)
$ n=$(echo "$M8 * $M9" | bc)
$ bash -c "time factor $n"
4951760154835678088235319297: 2147483647 2305843009213693951

real	0m0.004s
user	0m0.004s
sys	0m0.000s
```

For larger numbers, `factor` uses a slower algorithm. On the
same platform, factoring the eighth Fermat number *2^{256} + 1*
takes about 14 seconds, and the slower algorithm would have taken
about 750 ms to factor *2^{127} - 3* instead of the 50 ms needed by
the faster algorithm.

Factoring large numbers is, in general, hard. The Pollard-Brent rho
algorithm used by `factor` is particularly effective for
numbers with relatively small factors. If you wish to factor large
numbers which do not have small factors (for example, numbers which
are the product of two large primes), other methods are far better.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [`seq`: Print numeric sequences](#seq-invocation), Previous: [`factor`: Print prime factors](#factor-invocation), Up: [Numeric operations](#Numeric-operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 26.2 `numfmt`: Reformat numbers [¶](#numfmt_003a-Reformat-numbers)

`numfmt` reads numbers in various representations and reformats them
as requested. The most common usage is converting numbers to/from *human*
representation (e.g. ‘`4G`’ → ‘`4,000,000,000`’).

```
numfmt [option]... [number]
```

`numfmt` converts each number on the command-line according to the
specified options (see below). If no numbers are given, it reads numbers
from standard input. `numfmt` can optionally extract numbers from
specific columns, maintaining proper line padding and alignment.

An exit status of zero indicates success,
and a nonzero value indicates failure.

See `--invalid` for additional information regarding exit status.

* [General options](#General-options-3)
* [Possible units:](#Possible-units_003a)
* [Examples of using `numfmt`](#Examples-of-using-numfmt)

#### 26.2.1 General options [¶](#General-options-3)

The program accepts the following options. Also see [Common options](#Common-options).

‘`--debug`’ [¶](#index-_002d_002ddebug-6)
:   Print (to standard error) warning messages about possible erroneous usage.

‘`-d d`’ [¶](#index-_002dd-20)

‘`--delimiter=d`’
:   Use the character d as input field separator (default: whitespace).
    Using non-default delimiter turns off automatic padding.

‘`--field=fields`’ [¶](#index-_002d_002dfield)
:   Convert the number in input field fields (default: 1).
    fields supports `cut` style field ranges:

    ```
    N    N'th field, counted from 1
    N-   from N'th field, to end of line
    N-M  from N'th to M'th field (inclusive)
    -M   from first to M'th field (inclusive)
    -    all fields
    ```

‘`--format=format`’ [¶](#index-_002d_002dformat-6)
:   Use printf-style floating FORMAT string. The format string must contain
    one ‘`%f`’ directive, optionally with ‘`'`’, ‘`-`’, ‘`0`’, width
    or precision modifiers. The ‘`'`’ modifier will enable `--grouping`,
    the ‘`-`’ modifier will enable left-aligned `--padding` and the width
    modifier will enable right-aligned `--padding`. The ‘`0`’ width
    modifier (without the ‘`-`’ modifier) will generate leading zeros on the
    number, up to the specified width. A precision specification like ‘`%.1f`’
    will override the precision determined from the input data or set due to
    `--to` option auto scaling.

‘`--from=unit`’ [¶](#index-_002d_002dfrom-2)
:   Auto-scales input numbers according to unit. See UNITS below.
    The default is no scaling, meaning suffixes (e.g. ‘`M`’, ‘`G`’) will
    trigger an error.

‘`--from-unit=n`’ [¶](#index-_002d_002dfrom_002dunit)
:   Specify the input unit size (instead of the default 1). Use this option when
    the input numbers represent other units (e.g. if the input number ‘`10`’
    represents 10 units of 512 bytes, use ‘`--from-unit=512`’).
    Suffixes are handled as with ‘`--from=auto`’.

‘`--grouping`’ [¶](#index-_002d_002dgrouping)
:   Group digits in output numbers according to the current locale’s grouping rules
    (e.g *Thousands Separator* character, commonly ‘`.`’ (dot) or ‘`,`’
    comma). This option has no effect in ‘`POSIX/C`’ locale.

‘`--header[=n]`’ [¶](#index-_002d_002dheader-2)
:   Print the first n (default: 1) lines without any conversion.

‘`--invalid=mode`’ [¶](#index-_002d_002dinvalid)
:   The default action on input errors is to exit immediately with status code 2.
    `--invalid=‘abort’` explicitly specifies this default mode.
    With a mode of ‘`fail`’, print a warning for *each* conversion
    error, and exit with status 2. With a mode of ‘`warn`’, exit with
    status 0, even in the presence of conversion errors, and with a mode of
    ‘`ignore`’ do not even print diagnostics.

‘`--padding=n`’ [¶](#index-_002d_002dpadding)
:   Pad the output numbers to n characters, by adding spaces. If n is
    a positive number, numbers will be right-aligned. If n is a negative
    number, numbers will be left-aligned. By default, numbers are automatically
    aligned based on the input line’s width (only with the default delimiter).

‘`--round=method`’ [¶](#index-_002d_002dround)
:   When converting number representations, round the number according to
    method, which can be ‘`up`’, ‘`down`’,
    ‘`from-zero`’ (the default), ‘`towards-zero`’, ‘`nearest`’.

‘`--suffix=suffix`’ [¶](#index-_002d_002dsuffix-7)
:   Add ‘`SUFFIX`’ to the output numbers, and accept optional ‘`SUFFIX`’ in
    input numbers.

‘`--to=unit`’ [¶](#index-_002d_002dto)
:   Auto-scales output numbers according to unit. See *Units* below.
    The default is no scaling, meaning all the digits of the number are printed.

‘`--to-unit=n`’ [¶](#index-_002d_002dto_002dunit)
:   Specify the output unit size (instead of the default 1). Use this option when
    the output numbers represent other units (e.g. to represent ‘`4,000,000`’
    bytes in blocks of 1kB, use ‘`--to=si --to-unit=1000`’).
    Suffixes are handled as with ‘`--from=auto`’.

‘`-z`’ [¶](#index-_002dz-18)

‘`--zero-terminated`’
:   Delimit items with a zero byte rather than a newline (ASCII LF).
    I.e., treat input as items separated by ASCII NUL
    and terminate output items with ASCII NUL.
    This option can be useful in conjunction with ‘`perl -0`’ or
    ‘`find -print0`’ and ‘`xargs -0`’ which do the same in order to
    reliably handle arbitrary file names (even those containing blanks
    or other special characters).
    With `-z` the newline character is treated as a field separator.

#### 26.2.2 Possible units: [¶](#Possible-units_003a)

The following are the possible unit options with `--from=UNITS` and
`--to=UNITS`:

none
:   No scaling is performed. For input numbers, no suffixes are accepted, and any
    trailing characters following the number will trigger an error. For output
    numbers, all digits of the numbers will be printed.

si
:   Auto-scale numbers according to the *International System of Units (SI)*
    standard.
    For input numbers, accept one of the following suffixes.
    For output numbers, values larger than 1000 will be rounded, and printed with
    one of the following suffixes:

    ```
    ‘K’  =>  1000^1 = 10^3 (Kilo) (uppercase accepted on input)
    ‘k’  =>  1000^1 = 10^3 (Kilo) (lowercase used on output)
    ‘M’  =>  1000^2 = 10^6 (Mega)
    ‘G’  =>  1000^3 = 10^9 (Giga)
    ‘T’  =>  1000^4 = 10^{12} (Tera)
    ‘P’  =>  1000^5 = 10^{15} (Peta)
    ‘E’  =>  1000^6 = 10^{18} (Exa)
    ‘Z’  =>  1000^7 = 10^{21} (Zetta)
    ‘Y’  =>  1000^8 = 10^{24} (Yotta)
    ‘R’  =>  1000^9 = 10^{27} (Ronna)
    ‘Q’  =>  1000^{10} = 10^{30} (Quetta)
    ```

iec
:   Auto-scale numbers according to the *International Electrotechnical
    Commission (IEC)* standard.
    For input numbers, accept one of the following suffixes.
    For output numbers, values larger than 1024 will be rounded, and printed with
    one of the following suffixes:

    ```
    ‘K’  =>  1024^1 = 2^{10} (Kibi) (uppercase used on output)
    ‘k’  =>  1024^1 = 2^{10} (Kibi) (lowercase accepted on input)
    ‘M’  =>  1024^2 = 2^{20} (Mebi)
    ‘G’  =>  1024^3 = 2^{30} (Gibi)
    ‘T’  =>  1024^4 = 2^{40} (Tebi)
    ‘P’  =>  1024^5 = 2^{50} (Pebi)
    ‘E’  =>  1024^6 = 2^{60} (Exbi)
    ‘Z’  =>  1024^7 = 2^{70} (Zebi)
    ‘Y’  =>  1024^8 = 2^{80} (Yobi)
    ‘R’  =>  1024^9 = 2^{90} (Robi)
    ‘Q’  =>  1024^{10} = 2^{100} (Quebi)
    ```

    The `iec` option uses a single letter suffix (e.g. ‘`G`’), which is
    not fully standard, as the *iec* standard recommends a two-letter symbol
    (e.g ‘`Gi`’) – but in practice, this method is common. Compare with
    the `iec-i` option.

iec-i
:   Auto-scale numbers according to the *International Electrotechnical
    Commission (IEC)* standard.
    For input numbers, accept one of the following suffixes.
    For output numbers, values larger than 1024 will be rounded, and printed with
    one of the following suffixes:

    ```
    ‘Ki’  =>  1024^1 = 2^{10} (Kibi) (uppercase used on output)
    ‘ki’  =>  1024^1 = 2^{10} (Kibi) (lowercase accepted on input)
    ‘Mi’  =>  1024^2 = 2^{20} (Mebi)
    ‘Gi’  =>  1024^3 = 2^{30} (Gibi)
    ‘Ti’  =>  1024^4 = 2^{40} (Tebi)
    ‘Pi’  =>  1024^5 = 2^{50} (Pebi)
    ‘Ei’  =>  1024^6 = 2^{60} (Exbi)
    ‘Zi’  =>  1024^7 = 2^{70} (Zebi)
    ‘Yi’  =>  1024^8 = 2^{80} (Yobi)
    ‘Ri’  =>  1024^9 = 2^{90} (Robi)
    ‘Qi’  =>  1024^{10} = 2^{100} (Quebi)
    ```

    The `iec-i` option uses a two-letter suffix symbol (e.g. ‘`Gi`’),
    as the *iec* standard recommends, but this is not always common in
    practice. Compare with the `iec` option.

auto
:   ‘`auto`’ can only be used with `--from`. With this method, numbers
    with single-letter suffixes like ‘`K`’
    suffixes are interpreted as *SI* values, and numbers with
    two-letter suffixes like ‘`Ki`’
    are interpreted as *IEC* values.

#### 26.2.3 Examples of using `numfmt` [¶](#Examples-of-using-numfmt)

Converting a single number from/to *human* representation:

```
$ numfmt --to=si 500000
500k

$ numfmt --to=iec 500000
489K

$ numfmt --to=iec-i 500000
489Ki

$ numfmt --from=si 1M
1000000

$ numfmt --from=iec 1M
1048576

# with '--from=auto', M=Mega, Mi=Mebi
$ numfmt --from=auto 1M
1000000
$ numfmt --from=auto 1Mi
1048576
```

Converting from ‘`SI`’ to ‘`IEC`’ scales (e.g. when a drive’s capacity is
advertised as ‘`1TB`’, while checking the drive’s capacity gives lower
values):

```
$ numfmt --from=si --to=iec 1T
932G
```

With both input and output scales specified,
the largest defined prefixes are supported:

```
$ numfmt --from=si --to=iec-i 2000R
1.6Qi
```

Converting a single field from an input file / piped input (these contrived
examples are for demonstration purposes only, as both `ls` and
`df` support the `--human-readable` option to
output sizes in human-readable format):

```
# Third field (file size) will be shown in SI representation
$ ls -log | numfmt --field 3 --header --to=si | head -n4
-rw-r--r--  1     94k Aug 23  2011 ABOUT-NLS
-rw-r--r--  1    3.7k Jan  7 16:15 AUTHORS
-rw-r--r--  1     36k Jun  1  2011 COPYING
-rw-r--r--  1       0 Jan  7 15:15 ChangeLog

# Second field (size) will be shown in IEC representation
$ df --block-size=1 | numfmt --field 2 --header --to=iec | head -n4
File system   1B-blocks        Used  Available Use% Mounted on
rootfs             132G   104741408   26554036  80% /
tmpfs              794M        7580     804960   1% /run/shm
/dev/sdb1          694G   651424756   46074696  94% /home
```

Output can be tweaked using `--padding` or `--format`:

```
# Pad to 10 characters, right-aligned
$ du -s * | numfmt --to=si --padding=10
      2.5k config.log
       108 config.status
      1.7k configure
        20 configure.ac

# Pad to 10 characters, left-aligned
$ du -s * | numfmt --to=si --padding=-10
2.5k       config.log
108        config.status
1.7k       configure
20         configure.ac

# Pad to 10 characters, left-aligned, using 'format'
$ du -s * | numfmt --to=si --format="%10f"
      2.5k config.log
       108 config.status
      1.7k configure
        20 configure.ac

# Pad to 10 characters, left-aligned, using 'format'
$ du -s * | numfmt --to=si --padding="%-10f"
2.5k       config.log
108        config.status
1.7k       configure
20         configure.ac
```

With locales that support grouping digits, using `--grouping` or
`--format` enables grouping. In ‘`POSIX`’ locale, grouping is
silently ignored:

```
$ LC_ALL=C numfmt --from=iec --grouping 2G
2147483648

$ LC_ALL=en_US.utf8 numfmt --from=iec --grouping 2G
2,147,483,648

$ LC_ALL=ta_IN numfmt --from=iec --grouping 2G
2,14,74,83,648

$ LC_ALL=C numfmt --from=iec --format="==%'15f==" 2G
==     2147483648==

$ LC_ALL=en_US.utf8 numfmt --from=iec --format="==%'15f==" 2G
==  2,147,483,648==

$ LC_ALL=en_US.utf8 numfmt --from=iec --format="==%'-15f==" 2G
==2,147,483,648  ==

$ LC_ALL=ta_IN numfmt --from=iec --format="==%'15f==" 2G
== 2,14,74,83,648==
```

---

Previous: [`numfmt`: Reformat numbers](#numfmt-invocation), Up: [Numeric operations](#Numeric-operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 26.3 `seq`: Print numeric sequences [¶](#seq_003a-Print-numeric-sequences)

`seq` prints a sequence of numbers to standard output. Synopses:

```
seq [option]... last
seq [option]... first last
seq [option]... first increment last
```

`seq` prints the numbers from first to last by
increment. By default, each number is printed on a separate line.
When increment is not specified, it defaults to ‘`1`’,
even when first is larger than last.
first also defaults to ‘`1`’. So `seq 1` prints
‘`1`’, but `seq 0` and `seq 10 5` produce no output.
The sequence of numbers ends when the sum of the current number and
increment would become greater than last,
so `seq 1 10 10` only produces ‘`1`’.
increment must not be ‘`0`’; use the tool `yes` to get
repeated output of a constant number.
first, increment and last must not be `NaN`,
but `inf` is supported.
Floating-point numbers may be specified in either the current or
the C locale. See [Floating point numbers](#Floating-point).

The program accepts the following options. Also see [Common options](#Common-options).
Options must precede operands.

‘`-f format`’ [¶](#index-_002df-25)

‘`--format=format`’
:   Print all numbers using format.
    format must contain exactly one of the ‘`printf`’-style
    floating point conversion specifications ‘`%a`’, ‘`%e`’,
    ‘`%f`’, ‘`%g`’, ‘`%A`’, ‘`%E`’, ‘`%F`’, ‘`%G`’.
    The ‘`%`’ may be followed by zero or more flags taken from the set
    ‘`-+#0 '`’, then an optional width containing one or more digits,
    then an optional precision consisting of a ‘`.`’ followed by zero
    or more digits. format may also contain any number of ‘`%%`’
    conversion specifications. All conversion specifications have the
    same meaning as with ‘`printf`’.

    The default format is derived from first, step, and
    last. If these all use a fixed point decimal representation,
    the default format is ‘`%.pf`’, where p is the minimum
    precision that can represent the output numbers exactly. Otherwise,
    the default format is ‘`%g`’.

‘`-s string`’ [¶](#index-_002ds-31)

‘`--separator=string`’
:   Separate numbers with string; default is a newline.
    The output always terminates with a newline.

‘`-w`’ [¶](#index-_002dw-14)

‘`--equal-width`’
:   Print all numbers with the same width, by padding with leading zeros.
    first, step, and last should all use a fixed point
    decimal representation.
    (To have other kinds of padding, use `--format`).

You can get finer-grained control over output with `-f`:

```
$ seq -f '(%9.2E)' -9e5 1.1e6 1.3e6
(-9.00E+05)
( 2.00E+05)
( 1.30E+06)
```

If you want hexadecimal integer output, you can use `printf`
to perform the conversion:

```
$ printf '%x\n' $(seq 1048575 1024 1050623)
fffff
1003ff
1007ff
```

For very long lists of numbers, use xargs to avoid
system limitations on the length of an argument list:

```
$ seq 1000000 | xargs printf '%x\n' | tail -n 3
f423e
f423f
f4240
```

To generate octal output, use the printf `%o` format instead
of `%x`.

On most systems, seq can produce whole-number output for values up to
at least *2^{53}*. Larger integers are approximated. The details
differ depending on your floating-point implementation.
See [Floating point numbers](#Floating-point). A common
case is that `seq` works with integers through *2^{64}*,
and larger integers may not be numerically correct:

```
$ seq 50000000000000000000 2 50000000000000000004
50000000000000000000
50000000000000000000
50000000000000000004
```

However, when limited to non-negative whole numbers,
an increment of less than 200, and no format-specifying option,
seq can print arbitrarily large numbers.
Therefore `seq inf` can be used to
generate an infinite sequence of numbers.

Be careful when using `seq` with outlandish values: otherwise
you may see surprising results, as `seq` uses floating point
internally. For example, on the x86 platform, where the internal
representation uses a 64-bit fraction, the command:

```
seq 1 0.0000000000000000001 1.0000000000000000009
```

outputs 1.0000000000000000007 twice and skips 1.0000000000000000008.

An exit status of zero indicates success,
and a nonzero value indicates failure.

---

Next: [File timestamps](#File-timestamps), Previous: [Numeric operations](#Numeric-operations), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 27 File permissions [¶](#File-permissions-1)

Each file has a set of *file mode bits* that control the kinds of
access that users have to that file. They can be represented either in
symbolic form or as an octal number.

* [Structure of File Mode Bits](#Mode-Structure)
* [Symbolic Modes](#Symbolic-Modes)
* [Numeric Modes](#Numeric-Modes)
* [Operator Numeric Modes](#Operator-Numeric-Modes)
* [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid)

---

Next: [Symbolic Modes](#Symbolic-Modes), Up: [File permissions](#File-permissions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 27.1 Structure of File Mode Bits [¶](#Structure-of-File-Mode-Bits)

The file mode bits have two parts: the *file permission bits*,
which control ordinary access to the file, and *special mode
bits*, which affect only some files.

There are three kinds of permissions that a user can have for a file:

1. permission to read the file. For directories, this means permission to
   list the contents of the directory.
2. permission to write to (change) the file. For directories, this means
   permission to create and remove files in the directory.
3. permission to execute the file (run it as a program). For directories,
   this means permission to access files in the directory.

There are three categories of users who may have different permissions
to perform any of the above operations on a file:

1. the file’s owner;
2. other users who are in the file’s group;
3. everyone else.

Files are given an owner and group when they are created. Usually the
owner is the current user and the group is the group of the directory
the file is in, but this varies with the operating system, the
file system the file is created on, and the way the file is created. You
can change the owner and group of a file by using the `chown` and
`chgrp` commands.

In addition to the three sets of three permissions listed above, the
file mode bits have three special components, which affect only
executable files (programs) and, on most systems, directories:

The *set-user-ID bit* (*setuid bit*). [¶](#index-set_002duser_002dID)
:   On execution, set the process’s effective user ID to that of the file.
    For directories on a few systems, give files created in the directory
    the same owner as the directory, no matter who creates them, and set
    the set-user-ID bit of newly-created subdirectories.

The *set-group-ID bit* (*setgid bit*). [¶](#index-set_002dgroup_002dID)
:   On execution, set the process’s effective group ID to that of the file.
    For directories on most systems, give files created in the directory
    the same group as the directory, no matter what group the user who
    creates them is in, and set the set-group-ID bit of newly-created
    subdirectories.

The *restricted deletion flag* or *sticky bit*. [¶](#index-sticky)
:   Prevent unprivileged users from removing or renaming a file in a directory
    unless they own the file or the directory; this is commonly
    found on world-writable directories like `/tmp`.
    For regular files on some older systems, save the program’s text image on the
    swap device so it will load more quickly when run, so that the image
    is “sticky”.

In addition to the file mode bits listed above, there may be file attributes
specific to the file system, e.g., access control lists (ACLs), whether a
file is compressed, whether a file can be modified (immutability), and whether
a file can be dumped. These are usually set using programs
specific to the file system. For example:

ext2
:   On GNU and GNU/Linux the file attributes specific to
    the ext2 file system are set using `chattr`.

FFS
:   On FreeBSD the file flags specific to the FFS
    file system are set using `chflags`.

Even if a file’s mode bits allow an operation on that file,
that operation may still fail, because:

* the file-system-specific attributes or flags do not permit it; or
* the file system is mounted as read-only.

For example, if the immutable attribute is set on a file,
it cannot be modified, regardless of the fact that you
may have just run `chmod a+w FILE`.

---

Next: [Numeric Modes](#Numeric-Modes), Previous: [Structure of File Mode Bits](#Mode-Structure), Up: [File permissions](#File-permissions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 27.2 Symbolic Modes [¶](#Symbolic-Modes-1)

*Symbolic modes* represent changes to files’ mode bits as
operations on single-character symbols. They allow you to modify either
all or selected parts of files’ mode bits, optionally based on
their previous values, and perhaps on the current `umask` as well
(see [The Umask and Protection](#Umask-and-Protection)).

The format of symbolic modes is:

```
[ugoa...][-+=]perms...[,...]
```

where perms is either zero or more letters from the set
‘`rwxXst`’, or a single letter from the set ‘`ugo`’.

The following sections describe the operators and other details of
symbolic modes.

* [Setting Permissions](#Setting-Permissions)
* [Copying Existing Permissions](#Copying-Permissions)
* [Changing Special Mode Bits](#Changing-Special-Mode-Bits)
* [Conditional Executability](#Conditional-Executability)
* [Making Multiple Changes](#Multiple-Changes)
* [The Umask and Protection](#Umask-and-Protection)

---

Next: [Copying Existing Permissions](#Copying-Permissions), Up: [Symbolic Modes](#Symbolic-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 27.2.1 Setting Permissions [¶](#Setting-Permissions-1)

The basic symbolic operations on a file’s permissions are adding,
removing, and setting the permission that certain users have to read,
write, and execute or search the file. These operations have the following
format:

```
users operation permissions
```

The spaces between the three parts above are shown for readability only;
symbolic modes cannot contain spaces.

The users part tells which users’ access to the file is changed.
It consists of one or more of the following letters (or it can be empty;
see [The Umask and Protection](#Umask-and-Protection), for a description of what happens then). When
more than one of these letters is given, the order that they are in does
not matter.

`u` [¶](#index-owner-of-file_002c-permissions-for)
:   the user who owns the file;

`g` [¶](#index-group_002c-permissions-for)
:   other users who are in the file’s group;

`o` [¶](#index-other-permissions)
:   all other users;

`a`
:   all users; the same as ‘`ugo`’.

The operation part tells how to change the affected users’ access
to the file, and is one of the following symbols:

`+` [¶](#index-adding-permissions)
:   to add the permissions to whatever permissions the users
    already have for the file;

`-` [¶](#index-removing-permissions)
:   to remove the permissions from whatever permissions the
    users already have for the file;

`=` [¶](#index-setting-permissions)
:   to make the permissions the only permissions that the users
    have for the file.

The permissions part tells what kind of access to the file should
be changed; it is normally zero or more of the following letters. As with the
users part, the order does not matter when more than one letter is
given. Omitting the permissions part is useful only with the
‘`=`’ operation, where it gives the specified users no access
at all to the file.

`r` [¶](#index-read-permission_002c-symbolic)
:   the permission the users have to read the file;

`w` [¶](#index-write-permission_002c-symbolic)
:   the permission the users have to write to the file;

`x` [¶](#index-execute_002fsearch-permission_002c-symbolic)
:   the permission the users have to execute the file,
    or search it if it is a directory.

For example, to give everyone permission to read and write a regular file,
but not to execute it, use:

```
a=rw
```

To remove write permission for all users other than the file’s
owner, use:

```
go-w
```

The above command does not affect the access that the owner of
the file has to it, nor does it affect whether other users can
read or execute the file.

To give everyone except a file’s owner no permission to do anything with
that file, use the mode below. Other users could still remove the file,
if they have write permission on the directory it is in.

```
go=
```

Another way to specify the same thing is:

```
og-rwx
```

---

Next: [Changing Special Mode Bits](#Changing-Special-Mode-Bits), Previous: [Setting Permissions](#Setting-Permissions), Up: [Symbolic Modes](#Symbolic-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 27.2.2 Copying Existing Permissions [¶](#Copying-Existing-Permissions)

You can base a file’s permissions on its existing permissions. To do
this, instead of using a series of ‘`r`’, ‘`w`’, or ‘`x`’
letters after the
operator, you use the letter ‘`u`’, ‘`g`’, or ‘`o`’. For
example, the mode

```
o+g
```

adds the permissions for users who are in a file’s group to the
permissions that other users have for the file. Thus, if the file
started out as mode 664 (‘`rw-rw-r--`’), the above mode would change
it to mode 666 (‘`rw-rw-rw-`’). If the file had started out as mode
741 (‘`rwxr----x`’), the above mode would change it to mode 745
(‘`rwxr--r-x`’). The ‘`-`’ and ‘`=`’ operations work
analogously.

---

Next: [Conditional Executability](#Conditional-Executability), Previous: [Copying Existing Permissions](#Copying-Permissions), Up: [Symbolic Modes](#Symbolic-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 27.2.3 Changing Special Mode Bits [¶](#Changing-Special-Mode-Bits-1)

In addition to changing a file’s read, write, and execute/search permissions,
you can change its special mode bits. See [Structure of File Mode Bits](#Mode-Structure), for a
summary of these special mode bits.

To change the file mode bits to set the user ID on execution, use
‘`u`’ in the users part of the symbolic mode and
‘`s`’ in the permissions part.

To change the file mode bits to set the group ID on execution, use
‘`g`’ in the users part of the symbolic mode and
‘`s`’ in the permissions part.

To set both user and group ID on execution, omit the users part
of the symbolic mode (or use ‘`a`’) and use ‘`s`’ in the
permissions part.

To change the file mode bits to set the restricted deletion flag or sticky bit,
omit the users part of the symbolic mode (or use ‘`a`’) and use
‘`t`’ in the permissions part.

For example, to set the set-user-ID mode bit of a program,
you can use the mode:

```
u+s
```

To remove both set-user-ID and set-group-ID mode bits from
it, you can use the mode:

```
a-s
```

To set the restricted deletion flag or sticky bit, you can use
the mode:

```
+t
```

The combination ‘`o+s`’ has no effect. On GNU systems
the combinations ‘`u+t`’ and ‘`g+t`’ have no effect, and
‘`o+t`’ acts like plain ‘`+t`’.

The ‘`=`’ operator is not very useful with special mode bits.
For example, the mode:

```
o=t
```

does set the restricted deletion flag or sticky bit, but it also
removes all read, write, and execute/search permissions that users not in the
file’s group might have had for it.

See [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid), for additional rules concerning
set-user-ID and set-group-ID bits and directories.

---

Next: [Making Multiple Changes](#Multiple-Changes), Previous: [Changing Special Mode Bits](#Changing-Special-Mode-Bits), Up: [Symbolic Modes](#Symbolic-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 27.2.4 Conditional Executability [¶](#Conditional-Executability-1)

There is one more special type of symbolic permission: if you use
‘`X`’ instead of ‘`x`’, execute/search permission is affected only if the
file is a directory or already had execute permission.

For example, this mode:

```
a+X
```

gives all users permission to search directories, or to execute files if
anyone could execute them before.

---

Next: [The Umask and Protection](#Umask-and-Protection), Previous: [Conditional Executability](#Conditional-Executability), Up: [Symbolic Modes](#Symbolic-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 27.2.5 Making Multiple Changes [¶](#Making-Multiple-Changes)

The format of symbolic modes is actually more complex than described
above (see [Setting Permissions](#Setting-Permissions)). It provides two ways to make
multiple changes to files’ mode bits.

The first way is to specify multiple operation and
permissions parts after a users part in the symbolic mode.

For example, the mode:

```
og+rX-w
```

gives users other than the owner of the file read permission and, if
it is a directory or if someone already had execute permission
to it, gives them execute/search permission; and it also denies them write
permission to the file. It does not affect the permission that the
owner of the file has for it. The above mode is equivalent to
the two modes:

```
og+rX
og-w
```

The second way to make multiple changes is to specify more than one
simple symbolic mode, separated by commas. For example, the mode:

```
a+r,go-w
```

gives everyone permission to read the file and removes write
permission on it for all users except its owner. Another example:

```
u=rwx,g=rx,o=
```

sets all of the permission bits for the file explicitly. (It
gives users who are not in the file’s group no permission at all for
it.)

The two methods can be combined. The mode:

```
a+r,g+x-w
```

gives all users permission to read the file, and gives users who are in
the file’s group permission to execute/search it as well, but not permission
to write to it. The above mode could be written in several different
ways; another is:

```
u+r,g+rx,o+r,g-w
```

---

Previous: [Making Multiple Changes](#Multiple-Changes), Up: [Symbolic Modes](#Symbolic-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 27.2.6 The Umask and Protection [¶](#The-Umask-and-Protection)

If the users part of a symbolic mode is omitted, it defaults to
‘`a`’ (affect all users), except that any permissions that are
*set* in the system variable `umask` are *not affected*.
The value of `umask` can be set using the
`umask` command. Its default value varies from system to system.

Omitting the users part of a symbolic mode is generally not useful
with operations other than ‘`+`’. It is useful with ‘`+`’ because
it allows you to use `umask` as an easily customizable protection
against giving away more permission to files than you intended to.

As an example, if `umask` has the value 2, which removes write
permission for users who are not in the file’s group, then the mode:

```
+w
```

adds permission to write to the file to its owner and to other users who
are in the file’s group, but *not* to other users. In contrast,
the mode:

```
a+w
```

ignores `umask`, and *does* give write permission for
the file to all users.

---

Next: [Operator Numeric Modes](#Operator-Numeric-Modes), Previous: [Symbolic Modes](#Symbolic-Modes), Up: [File permissions](#File-permissions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 27.3 Numeric Modes [¶](#Numeric-Modes-1)

As an
alternative to giving a symbolic mode, you can give an octal (base 8)
number that represents the mode.

The permissions granted to the user,
to other users in the file’s group,
and to other users not in the file’s group each require three
bits: one bit for read, one for write, and one for execute/search permission.
These three bits are represented as one octal digit;
for example, if all three are present, the resulting 111 (in binary)
is represented as the digit 7 (in octal). The three special
mode bits also require one bit each, and they are as a group
represented as another octal digit. Here is how the bits are arranged,
starting with the highest valued bit:

```
Value in  Corresponding
Mode      Mode Bit

          Special mode bits:
4000      Set user ID
2000      Set group ID
1000      Restricted deletion flag or sticky bit

          The file's owner:
 400      Read
 200      Write
 100      Execute/search

          Other users in the file's group:
  40      Read
  20      Write
  10      Execute/search

          Other users not in the file's group:
   4      Read
   2      Write
   1      Execute/search
```

For example, numeric mode ‘`4751`’ corresponds to symbolic mode
‘`u=srwx,g=rx,o=x`’, and numeric mode ‘`664`’ corresponds to symbolic mode
‘`ug=rw,o=r`’. Numeric mode ‘`0`’ corresponds to symbolic mode
‘`a=`’.

A numeric mode is usually shorter than the corresponding symbolic
mode, but it is limited in that normally it cannot take into account the
previous file mode bits; it can only set them absolutely.
The set-user-ID and set-group-ID bits of directories are an exception
to this general limitation. See [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid).
Also, operator numeric modes can take previous file mode bits into
account. See [Operator Numeric Modes](#Operator-Numeric-Modes).

Numeric modes are always interpreted in octal; you do not have to add a
leading ‘`0`’, as you do in C. Mode ‘`0055`’ is the same as
mode ‘`55`’. However, modes of five digits or more, such as
‘`00055`’, are sometimes special (see [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid)).

---

Next: [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid), Previous: [Numeric Modes](#Numeric-Modes), Up: [File permissions](#File-permissions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 27.4 Operator Numeric Modes [¶](#Operator-Numeric-Modes-1)

An operator numeric mode is a numeric mode that is prefixed by a
‘`-`’, ‘`+`’, or ‘`=`’ operator, which has the same
interpretation as in symbolic modes. For example, ‘`+440`’ enables
read permission for the file’s owner and group, ‘`-1`’ disables
execute permission for other users, and ‘`=600`’ clears all
permissions except for enabling read-write permissions for the file’s
owner. Operator numeric modes can be combined with symbolic modes by
separating them with a comma; for example, ‘`=0,u+r`’ clears all
permissions except for enabling read permission for the file’s owner.

The commands ‘`chmod =755 dir`’ and ‘`chmod 755
dir`’ differ in that the former clears the directory dir’s
setuid and setgid bits, whereas the latter preserves them.
See [Directories and the Set-User-ID and Set-Group-ID Bits](#Directory-Setuid-and-Setgid).

Operator numeric modes are a GNU extension.

---

Previous: [Operator Numeric Modes](#Operator-Numeric-Modes), Up: [File permissions](#File-permissions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 27.5 Directories and the Set-User-ID and Set-Group-ID Bits [¶](#Directories-and-the-Set_002dUser_002dID-and-Set_002dGroup_002dID-Bits)

On most systems, if a directory’s set-group-ID bit is set, newly
created subfiles inherit the same group as the directory, and newly
created subdirectories inherit the set-group-ID bit of the parent
directory. On a few systems, a directory’s set-user-ID bit has a
similar effect on the ownership of new subfiles and the set-user-ID
bits of new subdirectories. These mechanisms let users share files
more easily, by lessening the need to use `chmod` or
`chown` to share new files.

These convenience mechanisms rely on the set-user-ID and set-group-ID
bits of directories. If commands like `chmod` and
`mkdir` routinely cleared these bits on directories, the
mechanisms would be less convenient and it would be harder to share
files. Therefore, a command like `chmod` does not affect the
set-user-ID or set-group-ID bits of a directory unless the user
specifically mentions them in a symbolic mode, or uses an operator
numeric mode such as ‘`=755`’, or sets them in a numeric mode, or
clears them in a numeric mode that has five or more octal digits.
For example, on systems that support
set-group-ID inheritance:

```
# These commands leave the set-user-ID and
# set-group-ID bits of the subdirectories alone,
# so that they retain their default values.
mkdir A B C
chmod 755 A
chmod 0755 B
chmod u=rwx,go=rx C
mkdir -m 755 D
mkdir -m 0755 E
mkdir -m u=rwx,go=rx F
```

If you want to try to set these bits, you must mention them
explicitly in the symbolic or numeric modes, e.g.:

```
# These commands try to set the set-user-ID
# and set-group-ID bits of the subdirectories.
mkdir G
chmod 6755 G
chmod +6000 G
chmod u=rwx,go=rx,a+s G
mkdir -m 6755 H
mkdir -m +6000 I
mkdir -m u=rwx,go=rx,a+s J
```

If you want to try to clear these bits, you must mention them
explicitly in a symbolic mode, or use an operator numeric mode, or
specify a numeric mode with five or more octal digits, e.g.:

```
# These commands try to clear the set-user-ID
