### 11.14 Limitations of Shell Builtins [¶](#Limitations-of-Shell-Builtins)

No, no, we are serious: some shells do have limitations! :)

You should always keep in mind that any builtin or command may support
options, and therefore differ in behavior with arguments
starting with a dash. For instance, even the innocent ‘`echo "$word"`’
can give unexpected results when `word` starts with a dash. To avoid
this problem, use ‘`printf '%s\n' "$word"`’. Many of these limitations
can be worked around using M4sh (see [Programming in M4sh](#Programming-in-M4sh)).

`.`
:   Use `.` only with regular files (use ‘`test -f`’). Bash
    2.03, for instance, chokes on ‘`. /dev/null`’. Remember that
    `.` uses `PATH` if its argument contains no slashes. Also,
    some shells, including bash 3.2, implicitly append the current directory
    to this `PATH` search, even though POSIX forbids it. So if you want
    to use `.` on a file `foo` in the current directory, you
    must use ‘`. ./foo`’.

    Not all shells gracefully handle syntax errors within a sourced file.
    On one extreme, some non-interactive shells abort the entire script. On
    the other, `zsh` 4.3.10 has a bug where it fails to react to the
    syntax error.

    ```
    $ echo 'fi' > syntax
    $ bash -c '. ./syntax; echo $?'
    ./syntax: line 1: syntax error near unexpected token `fi'
    ./syntax: line 1: `fi'
    2
    $ ash -c '. ./syntax; echo $?'
    ./syntax: 1: Syntax error: "fi" unexpected
    $ zsh -c '. ./syntax; echo $?'
    ./syntax:1: parse error near `fi'
    0
    ```

`!`
:   The Unix version 7 shell did not support
    negating the exit status of commands with `!`, and this feature
    is still absent from some shells (e.g., Solaris 10 `/bin/sh`).
    Other shells, such as FreeBSD `/bin/sh` or `ash`, have
    bugs when using `!`:

    ```
    $ sh -c '! : | :'; echo $?
    1
    $ ash -c '! : | :'; echo $?
    0
    $ sh -c '! { :; }'; echo $?
    1
    $ ash -c '! { :; }'; echo $?
    {: not found
    Syntax error: "}" unexpected
    2
    ```

    Shell code like this:

    ```
    if ! cmp file1 file2 >/dev/null 2>&1; then
      echo files differ or trouble
    fi
    ```

    is therefore not portable in practice. Typically it is easy to rewrite
    such code, e.g.:

    ```
    cmp file1 file2 >/dev/null 2>&1 ||
      echo files differ or trouble
    ```

    In M4sh, the `AS_IF` macro provides an easy way to write these kinds
    of conditionals:

    ```
    AS_IF([cmp -s file file.new], [],
      [echo files differ or trouble])
    ```

    This kind of rewriting is needed in code outside macro definitions that
    calls other macros. See [Common Shell Constructs](#Common-Shell-Constructs). It is also useful
    inside macro definitions, where the *then* and *else* branches
    might contain macro arguments.

    More generally, one can always rewrite ‘`! command`’ as:

    ```
    AS_IF([command], [(exit 1)])
    ```

`&&` and `||`
:   If an AND-OR list is not inside `AC_DEFUN`, and it contains
    calls to Autoconf macros, it should be rewritten using `AS_IF`.
    See [Common Shell Constructs](#Common-Shell-Constructs). The operators `&&` and `||`
    have equal precedence and are left associative, so instead of:

    ```
    # This is dangerous outside AC_DEFUN.
    cmp a b >/dev/null 2>&1 &&
      AS_ECHO([files are same]) >$tmpfile ||
        AC_MSG_NOTICE([files differ, or echo failed])
    ```

    you can use:

    ```
    # This is OK outside AC_DEFUN.
    AS_IF([AS_IF([cmp a b >/dev/null 2>&1],
             [AS_ECHO([files are same]) >$tmpfile],
             [false])],
      [AC_MSG_NOTICE([files differ, or echo failed])])
    ```

`{...}`
:   Bash 3.2 (and earlier versions) sometimes does not properly set
    ‘`$?`’ when failing to write redirected output of a compound command.
    This problem is most commonly observed with ‘`{…}`’; it does
    not occur with ‘`(…)`’. For example:

    ```
    $ bash -c '{ echo foo; } >/bad; echo $?'
    bash: line 1: /bad: Permission denied
    0
    $ bash -c 'while :; do echo; done >/bad; echo $?'
    bash: line 1: /bad: Permission denied
    0
    ```

    To work around the bug, prepend ‘`:;`’:

    ```
    $ bash -c ':;{ echo foo; } >/bad; echo $?'
    bash: line 1: /bad: Permission denied
    1
    ```

    POSIX requires a syntax error if a brace list has no contents. However,
    not all shells obey this rule; and on shells where empty lists are
    permitted, the effect on ‘`$?`’ is inconsistent. To avoid problems,
    ensure that a brace list is never empty.

    ```
    $ bash -c 'false; { }; echo $?' || echo $?
    bash: line 1: syntax error near unexpected token `}'
    bash: line 1: `false; { }; echo $?'
    2
    $ zsh -c 'false; { }; echo $?' || echo $?
    1
    $ pdksh -c 'false; { }; echo $?' || echo $?
    0
    ```

`break`
:   The use of ‘`break 2`’ etc. is safe.

`case`
:   If a `case` command is not inside `AC_DEFUN`, and it contains
    calls to Autoconf macros, it should be rewritten using `AS_CASE`.
    See [Common Shell Constructs](#Common-Shell-Constructs). Instead of:

    ```
    # This is dangerous outside AC_DEFUN.
    case $filename in
      *.[ch]) AC_MSG_NOTICE([C source file]);;
    esac
    ```

    use:

    ```
    # This is OK outside AC_DEFUN.
    AS_CASE([$filename],
      [[*.[ch]]], [AC_MSG_NOTICE([C source file])])
    ```

    You don’t need to quote the argument; no splitting is performed.

    You don’t need the final ‘`;;`’, but you should use it.

    POSIX requires support for `case` patterns with opening
    parentheses like this:

    ```
    case $file_name in
      (*.c) echo "C source code";;
    esac
    ```

    but the `(` in this example is not portable to a few obsolescent Bourne
    shell implementations, which is a pity for those of us using tools that
    rely on balanced parentheses. For instance, with Solaris 10
    `/bin/sh`:

    ```
    $ case foo in (foo) echo foo;; esac
    error→syntax error: `(' unexpected
    ```

    The leading ‘`(`’ can be omitted safely. Unfortunately, there are
    contexts where unbalanced parentheses cause other problems, such as when
    using a syntax-highlighting editor that searches for the balancing
    counterpart, or more importantly, when using a case statement as an
    underquoted argument to an Autoconf macro. See [Dealing with unbalanced parentheses](#Balancing-Parentheses), for trade-offs involved in various styles of dealing with
    unbalanced ‘`)`’.

    Zsh handles pattern fragments derived from parameter expansions or
    command substitutions as though quoted:

    ```
    $ pat=\?; case aa in ?$pat) echo match;; esac
    $ pat=\?; case a? in ?$pat) echo match;; esac
    match
    ```

    Because of a bug in its `fnmatch`, Bash fails to properly
    handle backslashes in character classes:

    ```
    bash-2.02$ case /tmp in [/\\]*) echo OK;; esac
    bash-2.02$
    ```

    This is extremely unfortunate, since you are likely to use this code to
    handle POSIX or MS-DOS absolute file names. To work around this
    bug, always put the backslash first:

    ```
    bash-2.02$ case '\TMP' in [\\/]*) echo OK;; esac
    OK
    bash-2.02$ case /tmp in [\\/]*) echo OK;; esac
    OK
    ```

    Many Bourne shells cannot handle closing brackets in character classes
    correctly.

    Some shells also have problems with backslash escaping in case you do not want
    to match the backslash: both a backslash and the escaped character match this
    pattern. To work around this, specify the character class in a variable, so
    that quote removal does not apply afterwards, and the special characters don’t
    have to be backslash-escaped:

    ```
    $ case '\' in [\<]) echo OK;; esac
    OK
    $ scanset='[<]'; case '\' in $scanset) echo OK;; esac
    $
    ```

    Even with this, Solaris `ksh` matches a backslash if the set
    contains any
    of the characters ‘`|`’, ‘`&`’, ‘`(`’, or ‘`)`’.

    Some shells, such as Ash 0.3.8, are confused by an empty
    `case`/`esac`:

    ```
    ash-0.3.8 $ case foo in esac;
    error→Syntax error: ";" unexpected (expecting ")")
    ```

    POSIX requires `case` to give an exit status of 0 if no cases
    match. However, `/bin/sh` in Solaris 10 does not obey this
    rule. Meanwhile, it is unclear whether a case that matches, but
    contains no statements, must also change the exit status to 0. The M4sh
    macro `AS_CASE` works around these inconsistencies.

    ```
    $ bash -c 'case `false` in ?) ;; esac; echo $?'
    0
    $ /bin/sh -c 'case `false` in ?) ;; esac; echo $?'
    255
    ```

`cd`
:   POSIX requires that `cd` must support
    the `-L` (“logical”) and `-P` (“physical”) options,
    with `-L` being the default. However, traditional shells do
    not support these options, and their `cd` command has the
    `-P` behavior.

    Portable scripts should assume neither option is supported, and should
    assume neither behavior is the default. This can be a bit tricky,
    since the POSIX default behavior means that, for example,
    ‘`ls ..`’ and ‘`cd ..`’ may refer to different directories if
    the current logical directory is a symbolic link. It is safe to use
    `cd dir` if dir contains no `..` components.
    Also, Autoconf-generated scripts check for this problem when computing
    variables like `ac_top_srcdir` (see [Performing Configuration Actions](#Configuration-Actions)),
    so it is safe to `cd` to these variables.

    POSIX specifies an `-e` option that affects the exit status
    when the `-P` option is in effect. Portable scripts should
    avoid it for the same reason they avoid `-P`.

    POSIX says that a successful `cd` updates the values of the
    `OLDPWD` and `PWD` variables, and that `cd -` acts like
    `cd "$OLDPWD"`. Older shells, such as Solaris 10
    `/bin/sh`, do not support this and portable scripts should not
    rely on it.

    If `HOME` is unset or empty, the behavior of `cd` with no
    arguments is implementation-defined, so portable scripts should avoid it
    in that case.

    POSIX 1003.1-2024 states that `cd` must fail if given an
    explicit empty argument. However, earlier POSIX editions did not
    specify the behavior and some shells do nothing, some change to the
    first entry in `CDPATH`, some change to `HOME`, and some exit
    the shell rather than returning an error. Unfortunately, this means
    that if ‘`$var`’ is empty, then ‘`cd "$var"`’ is less predictable
    than ‘`cd $var`’ (at least the latter is well-behaved in all shells
    at changing to `HOME`, although this is probably not what you wanted
    in a script). You should check that a directory name was supplied
    before trying to change locations.

    If `cd` fails, POSIX requires the invoking shell to behave as if
    `cd` exited with status 1, which means the invoking shell can continue.
    However, Solaris 10 `/bin/sh` aborts the containing command if
    interactive, and exits otherwise.

    See [Special Shell Variables](#Special-Shell-Variables), for portability problems involving
    `cd` and the `CDPATH` environment variable.
    Also please see the discussion of the `pwd` command.

`echo`
:   The simple `echo` is probably the most surprising source of
    portability troubles. It is not possible to use ‘`echo`’ portably
    unless both options and escape sequences are omitted.

    Do not use options, as some shells support them and others do not.
    For example, POSIX says that the behavior of ‘`echo -n foo`’ is
    implementation-defined. On some platforms the output is ‘`foo`’
    without a trailing newline, on others it is ‘`-n foo`’ with a
    trailing newline, and POSIX allows even other behavior.

    Do not use backslashes in the arguments, as there is no consensus on
    their handling. For ‘`echo '\n' | wc -l`’, the `sh` of
    Solaris 10 outputs 2,
    but Bash and Zsh (in `sh` emulation mode) output 1.
    The problem is truly `echo`: all the shells
    understand ‘`'\n'`’ as the string composed of a backslash and an
    ‘`n`’.

    Because of these problems, do not pass a string containing arbitrary
    characters to `echo`. For example, ‘`echo "$foo"`’ is safe
    only if you know that foo’s value cannot contain backslashes and
    cannot start with ‘`-`’.

    Normally, `printf` is safer and easier to use than `echo`
    and `echo -n`. Thus, you should use `printf '%s\n'`
    instead of `echo`, and similarly use `printf %s` instead
    of `echo -n`.

    Older scripts, written before `printf` was portable,
    sometimes used a here-document as a safer alternative to `echo`,
    like this:

    ```
    cat <<EOF
    $foo
    EOF
    ```

    However, this usage is problematic, as even some modern shells have
    hard-to-reproduce bugs when dealing with here-documents.

`eval`
:   The `eval` command is useful in limited circumstances, e.g.,
    using commands like ‘`eval table_$key=\$value`’ and ‘`eval
    value=table_$key`’ to simulate a hash table when the key is known to be
    alphanumeric.

    You should also be wary of common bugs in `eval` implementations.
    In some shell implementations (e.g., older `ash`, OpenBSD 3.8
    `sh`, `pdksh` v5.2.14 99/07/13.2, and `zsh`
    4.2.5), the arguments of ‘`eval`’ are evaluated in a context where
    ‘`$?`’ is 0, so they exhibit behavior like this:

    ```
    $ false; eval 'echo $?'
    0
    ```

    The correct behavior here is to output a nonzero value,
    but portable scripts should not rely on this.

    You should not rely on `LINENO` within `eval`.
    See [Special Shell Variables](#Special-Shell-Variables).

    Note that, even though these bugs are easily avoided,
    `eval` is tricky to use on arbitrary arguments.
    It is obviously unwise to use ‘`eval $cmd`’ if the string value of
    ‘`cmd`’ was derived from an untrustworthy source. But even if the
    string value is valid, ‘`eval $cmd`’ might not work as intended,
    since it causes field splitting and file name expansion to occur twice,
    once for the `eval` and once for the command itself. It is
    therefore safer to use ‘`eval "$cmd"`’. For example, if cmd
    has the value ‘`cat test?.c`’, ‘`eval $cmd`’ might expand to the
    equivalent of ‘`cat test;.c`’ if there happens to be a file named
    `test;.c` in the current directory; and this in turn
    mistakenly attempts to invoke `cat` on the file `test` and
    then execute the command `.c`. To avoid this problem, use
    ‘`eval "$cmd"`’ rather than ‘`eval $cmd`’.

    However, suppose that you want to output the text of the evaluated
    command just before executing it. Assuming the previous example,
    ‘`printf '%s\n' "Executing: $cmd"`’ outputs ‘`Executing: cat test?.c`’,
    but this output doesn’t show the user that ‘`test;.c`’ is the actual
    name of the copied file.
    Conversely, ‘`printf 'Executing:'; eval "printf ' %s' $cmd"; printf '\n'`’
    works on this example, but it fails with ‘`cmd='cat foo >bar'`’,
    since it mistakenly replaces the contents of `bar` by the
    string ‘ `cat foo`’. No simple, general, and portable solution to
    this problem is known.

`exec`
:   POSIX describes several categories of shell built-ins. Special
    built-ins (such as `exit`) must impact the environment of the
    current shell, and need not be available through `exec`. All
    other built-ins are regular, and must not propagate variable assignments
    to the environment of the current shell. However, the group of regular
    built-ins is further distinguished by commands that do not require a
    `PATH` search (such as `cd`), in contrast to built-ins that
    are offered as a more efficient version of something that must still be
    found in a `PATH` search (such as `echo`). POSIX is not
    clear on whether `exec` must work with the list of 17 utilities
    that are invoked without a `PATH` search, and many platforms lack an
    executable for some of those built-ins:

    ```
    $ sh -c 'exec cd /tmp'
    sh: line 0: exec: cd: not found
    ```

    All other built-ins that provide utilities specified by POSIX must have
    a counterpart executable that exists on `PATH`, although POSIX
    allows `exec` to use the built-in instead of the executable.
    For example, contrast `bash` 3.2 and `pdksh` 5.2.14:

    ```
    $ bash -c 'pwd --version' | head -n1
    bash: line 0: pwd: --: invalid option
    pwd: usage: pwd [-LP]
    $ bash -c 'exec pwd --version' | head -n1
    pwd (GNU coreutils) 6.10
    $ pdksh -c 'exec pwd --version' | head -n1
    pdksh: pwd: --: unknown option
    ```

    When it is desired to avoid a regular shell built-in, the workaround is
    to use some other forwarding command, such as `env` or
    `nice`, that will ensure a path search:

    ```
    $ pdksh -c 'exec true --version' | head -n1
    $ pdksh -c 'nice true --version' | head -n1
    true (GNU coreutils) 6.10
    $ pdksh -c 'env true --version' | head -n1
    true (GNU coreutils) 6.10
    ```

`exit`
:   The default value of `exit` is supposed to be `$?`;
    unfortunately, some shells, such as the DJGPP port of Bash 2.04, just
    perform ‘`exit 0`’.

    ```
    bash-2.04$ foo=`exit 1` || echo fail
    fail
    bash-2.04$ foo=`(exit 1)` || echo fail
    fail
    bash-2.04$ foo=`(exit 1); exit` || echo fail
    bash-2.04$
    ```

    Using ‘`exit $?`’ restores the expected behavior.

    Some shell scripts, such as those generated by `autoconf`, use a
    trap to clean up before exiting. If the last shell command exited with
    nonzero status, the trap also exits with nonzero status so that the
    invoker can tell that an error occurred.

    Unfortunately, in some shells, such as Solaris 10 `/bin/sh`, an exit
    trap ignores the `exit` command’s argument. In these shells, a trap
    cannot determine whether it was invoked by plain `exit` or by
    `exit 1`. Instead of calling `exit` directly, use the
    `AC_MSG_ERROR` macro that has a workaround for this problem.

`export`
:   The builtin `export` dubs a shell variable *environment
    variable*. Each update of exported variables corresponds to an update
    of the environment variables. Conversely, each environment variable
    received by the shell when it is launched should be imported as a shell
    variable marked as exported.

    Alas, some older shells, such as Solaris 10 `/bin/sh`, forget to
    `export` the environment variables they receive. As a result,
    two variables coexist: the environment variable and the shell
    variable. The following code demonstrates this failure:

    ```
    #!/bin/sh
    echo $FOO
    FOO=bar
    echo $FOO
    exec /bin/sh $0
    ```

    when run with ‘`FOO=foo`’ in the environment, these shells print
    alternately ‘`foo`’ and ‘`bar`’, although they should print only
    ‘`foo`’ and then a sequence of ‘`bar`’s.

    Therefore you should `export` again each environment variable
    that you update; the export can occur before or after the assignment.

    POSIX is not clear on whether the `export` of an undefined
    variable causes the variable to be defined with the value of an empty
    string, or merely marks any future definition of a variable by that name
    for export. Various shells behave differently in this regard:

    ```
    $ sh -c 'export foo; env | grep foo'
    $ ash -c 'export foo; env | grep foo'
    foo=
    ```

    POSIX requires `export` to honor assignments made as arguments,
    but older shells do not support this, including `/bin/sh` in
    Solaris 10. Portable scripts should separate assignments and exports
    into different statements.

    ```
    $ bash -c 'export foo=bar; echo $foo'
    bar
    $ /bin/sh -c 'export foo=bar; echo $foo'
    /bin/sh: foo=bar: is not an identifier
    $ /bin/sh -c 'export foo; foo=bar; echo $foo'
    bar
    ```

    POSIX requires `export` to work with any arbitrary value for the
    contents of the variable being exported, as long as the total size of
    the environment combined with arguments doesn’t exceed `ARG_MAX`
    when executing a child process. However, some shells have extensions
    that involve interpreting some environment values specially, regardless
    of the variable name. We currently know of one case: all versions of
    Bash released prior to 27 September 2014 interpret an environment
    variable with an initial content substring of `() {` as an
    exported function definition (this is the “Shellshock” remote
    execution bug, CVE-2014-6271 and friends, where it was possible to
    exploit the function parser to cause remote code execution on child bash
    startup; newer versions of Bash use special environment variable
    *names* instead of values to implement the same feature).

    There may be entries inherited into the environment that are not valid
    as shell variable names; POSIX states that processes should be tolerant
    of these names. Some shells such as `dash` do this by removing
    those names from the environment at startup, while others such as
    `bash` hide the entry from shell access but still pass it on to
    child processes. While you can set such names using `env` for a
    direct child process, you cannot rely on them being preserved through an
    intermediate pass through the shell.

`false`
:   Don’t expect `false` to exit with status 1: in native
    Solaris `/bin/false` exits with status 255.

`for`
:   To loop over positional arguments, use:

    ```
    for arg
    do
      printf '%s\n' "$arg"
    done
    ```

    You may *not* leave the `do` on the same line as `for`,
    since some shells improperly grok:

    ```
    for arg; do
      printf '%s\n' "$arg"
    done
    ```

    If you want to explicitly refer to the positional arguments, use:

    ```
    for arg in "$@"; do
      printf '%s\n' "$arg"
    done
    ```

    POSIX requires support for a `for` loop with no list after
    `in`. However, Solaris 10 `/bin/sh` treats that as a syntax
    error. It is possible to work around this by providing any shell word
    that expands to nothing, or by ignoring an obvious sentinel.

    ```
    $ /bin/sh -c 'for a in $empty; do echo hi; done'
    $ /bin/sh -c 'for a in ; do echo hi; done'
    /bin/sh: syntax error at line 1: `;' unexpected
    ```

    This syntax problem is most frequently encountered in code that goes
    through several layers of expansion, such as an m4 macro or makefile
    variable used as a list body, where the first layer of expansion (m4 or
    make) can end up expanding to nothing in the version handed to the
    shell. In the makefile context, one common workaround is to use a shell
    variable rather than a make variable as the source of the list.

    ```
    $ cat Makefile
    list =
    bad:
    	@for arg in $(list); do \
    	  printf '%s\n' $$arg; \
    	done
    good:
    	@list='$(list)'; \
    	for arg in $$list; do \
    	  printf '%s\n' $$arg; \
    	done
    $ make bad 2&>1 | head -n1
    sh: syntax error at line 1: `;' unexpected
    $ make bad list='a b'
    a
    b
    $ make good
    $ make good list='a b'
    a
    b
    ```

    In Solaris 10 `/bin/sh`, when the list of arguments of a
    `for` loop starts with *unquoted* tokens looking like
    variable assignments, the loop is not executed on those tokens:

    ```
    $ /bin/sh -c 'for v in a=b c=d x e=f; do echo $v; done'
    x
    e=f
    ```

    Thankfully, quoting the assignment-like tokens, or starting the list
    with other tokens (including unquoted variable expansion that results in
    an assignment-like result), avoids the problem, so it is easy to work
    around:

    ```
    $ /bin/sh -c 'for v in "a=b"; do echo $v; done'
    a=b
    $ /bin/sh -c 'x=a=b; for v in $x c=d; do echo $v; done'
    a=b
    c=d
    ```

`if`
:   If an `if` command is not inside `AC_DEFUN`, and it contains
    calls to Autoconf macros, it should be rewritten using `AS_IF`.
    See [Common Shell Constructs](#Common-Shell-Constructs).

    Using `if ! …` is not portable. See [`!` notes](#g_t_0021).

    Some very old shells did not reset the exit status from an `if`
    with no `else`:

    ```
    $ if (exit 42); then true; fi; echo $?
    42
    ```

    whereas a proper shell should have printed ‘`0`’. Although this is no
    longer a portability problem, as any shell that supports functions gets it
    correct, it explains why some makefiles have lengthy
    constructs:

    ```
    if test -f "$file"; then
      install "$file" "$dest"
    else
      :
    fi
    ```

`printf`
:   A format string starting with a ‘`-`’ can cause problems.
    Bash interprets it as an option and
    gives an error. And ‘`--`’ to mark the end of options is not good
    in the NetBSD Almquist shell (e.g., 0.4.6) which takes that
    literally as the format string. Putting the ‘`-`’ in a ‘`%c`’
    or ‘`%s`’ is probably easiest:

    ```
    printf %s -foo
    ```

    AIX 7.2 `sh` mishandles octal escapes in multi-byte locales by
    treating them as characters instead of bytes. For example, in a locale
    using the UTF-8 encoding, ‘`printf '\351'`’ outputs the two bytes C3,
    A9 (the UTF-8 encoding for U+00E9) instead of the desired single byte E9.
    To work around the bug, use the C locale.

    Bash 2.03 mishandles an escape sequence that happens to evaluate to ‘`%`’:

    ```
    $ printf '\045'
    bash: printf: `%': missing format character
    ```

    Large outputs may cause trouble. On Solaris 10, for
    example, `/usr/bin/printf` is buggy, so when using
    `/bin/sh` the command ‘`printf %010000x 123`’ normally dumps
    core.

    Since `printf` is not always a shell builtin, there is a
    potential speed penalty for using `printf '%s\n'` as a replacement
    for an `echo` that does not interpret ‘`\`’ or leading
    ‘`-`’. With Solaris `ksh`, it is possible to use `print
    -r --` for this role instead.

    See [Limitations of Shell Builtins](#echo), for a discussion of
    portable alternatives to both `printf` and `echo`.

`pwd`
:   With modern shells, plain `pwd` outputs a “logical”
    directory name, some of whose components may be symbolic links. These
    directory names are in contrast to “physical” directory names, whose
    components are all directories.

    POSIX requires that `pwd` must support
    the `-L` (“logical”) and `-P` (“physical”) options,
    with `-L` being the default. However, traditional shells do
    not support these options, and their `pwd` command has the
    `-P` behavior.

    Portable scripts should assume neither option is supported, and should
    assume neither behavior is the default. Also, on many hosts
    ‘`/bin/pwd`’ is equivalent to ‘`pwd -P`’, but POSIX
    does not require this behavior and portable scripts should not rely on
    it.

    Typically it’s best to use plain `pwd`. On modern hosts this
    outputs logical directory names, which have the following advantages:

    * Logical names are what the user specified.
    * Physical names may not be portable from one installation
      host to another due to network file system gymnastics.
    * On modern hosts ‘`pwd -P`’ may fail due to lack of permissions to
      some parent directory, but plain `pwd` cannot fail for this
      reason.

    Also please see the discussion of the `cd` command.

`read`
:   No options are portable, not even support `-r` (Solaris 10
    `/bin/sh` for example).

`set`
:   With the FreeBSD 6.0 shell, the `set` command (without
    any options) does not sort its output.

    The `set` builtin faces the usual problem with arguments
    starting with a
    dash. Modern shells understand `--` to specify
    the end of the options (any argument after `--` is a parameter,
    even ‘`-x`’ for instance), but some ancient shells stop option
    processing only after a non-option argument is found. Therefore, use
    ‘`dummy`’ or simply ‘`x`’ to end the option processing, and use
    `shift` to pop it out:

    ```
    set x $my_list; shift
    ```

    Avoid ‘`set -`’, e.g., ‘`set - $my_list`’. POSIX no
    longer requires support for this command, and in traditional shells
    ‘`set - $my_list`’ resets the `-v` and `-x` options, which
    makes scripts harder to debug.

    Some nonstandard shells do not recognize more than one option
    (e.g., ‘`set -e -x`’ assigns ‘`-x`’ to the command line). It is
    better to combine them:

    ```
    set -ex
    ```

    The `-e` option has historically been under-specified, with enough
    ambiguities to cause numerous differences across various shell
    implementations; see for example
    [this overview](https://www.in-ulm.de/~mascheck/various/set-e/),
    or [this link](https://www.austingroupbugs.net/view.php?id=52),
    documenting a change to POSIX 2008 to match `ksh88` behavior.
    Note that mixing `set -e` and shell functions is asking for surprises:

    ```
    set -e
    doit()
    {
      rm file
      echo one
    }
    doit || echo two
    ```

    According to the recommendation, ‘`one`’ should always be output
    regardless of whether the `rm` failed, because it occurs within
    the body of the shell function ‘`doit`’ invoked on the left side of
    ‘`||`’, where the effects of ‘`set -e`’ are not enforced.
    Likewise, ‘`two`’ should never be printed, since the failure of
    `rm` does not abort the function, such that the status of
    ‘`doit`’ is 0.

    The BSD shell has had several problems with the `-e`
    option. Older versions of the BSD
    shell (circa 1990) mishandled ‘`&&`’, ‘`||`’, ‘`if`’, and
    ‘`case`’ when `-e` was in effect, causing the shell to exit
    unexpectedly in some cases. This was particularly a problem with
    makefiles, and led to circumlocutions like ‘`sh -c 'test -f file ||
    touch file'`’, where the seemingly-unnecessary ‘`sh -c '…'`’
    wrapper works around the bug (see [Failure in Make Rules](#Failure-in-Make-Rules)).

    Even relatively-recent versions of the BSD shell (e.g., OpenBSD 3.4)
    wrongly exit with `-e` if the last command within a compound
    statement fails and is guarded by an ‘`&&`’ only. For example:

    ```
    #! /bin/sh
    set -e
    foo=''
    test -n "$foo" && exit 1
    echo one
    if :; then
      test -n "$foo" && exit 1
      echo two
      test -n "$foo" && exit 1
    fi
    echo three
    ```

    does not print ‘`three`’. One workaround is to change the last
    instance of ‘`test -n "$foo" && exit 1`’ to be ‘`if test -n
    "$foo"; then exit 1; fi`’ instead. Another possibility is to warn BSD
    users not to use ‘`sh -e`’.

    When ‘`set -e`’ is in effect, a failed command substitution in
    Solaris 10 `/bin/sh` cannot be ignored, even with ‘`||`’.

    ```
    $ /bin/sh -c 'set -e; foo=`false` || echo foo; echo bar'
    $ bash -c 'set -e; foo=`false` || echo foo; echo bar'
    foo
    bar
    ```

    Moreover, a command substitution, successful or not, causes this shell to
    exit from a failing outer command even in presence of an ‘`&&`’ list:

    ```
    $ bash -c 'set -e; false `true` && echo notreached; echo ok'
    ok
    $ sh -c 'set -e; false `true` && echo notreached; echo ok'
    $
    ```

    Job control is not provided by all shells, so the use of ‘`set -m`’
    or ‘`set -b`’ must be done with care. When using `zsh` in
    native mode, asynchronous notification (‘`set -b`’) is enabled by
    default, and using ‘`emulate sh`’ to switch to POSIX mode does not
    clear this setting (although asynchronous notification has no impact
    unless job monitoring is also enabled). Also, `zsh` 4.3.10 and
    earlier have a bug where job control can be manipulated in interactive
    shells, but not in subshells or scripts. Furthermore, some shells, like
    `pdksh`, fail to treat subshells as interactive, even though the
    parent shell was.

    ```
    $ echo $ZSH_VERSION
    4.3.10
    $ set -m; echo $?
    0
    $ zsh -c 'set -m; echo $?'
    set: can't change option: -m
    $ (set -m); echo $?
    set: can't change option: -m
    1
    $ pdksh -ci 'echo $-; (echo $-)'
    cim
    c
    ```

    Use of `set -n` (typically via `sh -n script`) to
    validate a script is not foolproof. Modern `ksh93` tries to be
    helpful by informing you about better syntax, but switching the script
    to use the suggested syntax in order to silence the warnings would
    render the script no longer portable to older shells:

    ```
    $ ksh -nc '``'
    ksh: warning: line 1: `...` obsolete, use $(...)
    0
    ```

    Autoconf
    itself uses `sh -n` within its testsuite to check that correct
    scripts were generated, but only after first probing for other shell
    features (such as `test ${BASH_VERSION+y}`) that indicate
    a reasonably fast and working implementation.

`shift`
:   Not only is `shift`ing a bad idea when there is nothing left to
    shift, but in addition it is not portable: the shell of MIPS
    RISC/OS 4.52 refuses to do it.

    Don’t use ‘`shift 2`’ etc.; while it in the SVR1 shell (1983),
    it is also absent in many pre-POSIX shells.

`source`
:   This command is not portable, as POSIX does not require it; use
    `.` instead.

`test`
:   The `test` program is the way to perform many file and string
    tests. It is often invoked by the alternate name ‘`[`’, but using
    that name in Autoconf code is asking for trouble since it is an M4 quote
    character.

    The `-a`, `-o`, ‘`(`’, and ‘`)`’ operands are not
    present in all implementations, and have been marked obsolete by POSIX
    2008. This is because there are inherent ambiguities in using them.
    For example, ‘`test "$1" -a "$2"`’ looks like a binary operator to
    check whether two strings are both non-empty, but if ‘`$1`’ is the
    literal ‘`!`’, then some implementations of `test` treat it
    as a negation of the unary operator `-a`.

    Thus, portable uses of `test` should never have more than four
    arguments, and scripts should use shell constructs like ‘`&&`’ and
    ‘`||`’ instead. If you combine ‘`&&`’ and ‘`||`’ in the same
    statement, keep in mind that they have equal precedence, so it is often
    better to parenthesize even when this is redundant. For example:

    ```
    # Not portable:
    test "X$a" = "X$b" -a \
      '(' "X$c" != "X$d" -o "X$e" = "X$f" ')'

    # Portable:
    test "X$a" = "X$b" &&
      { test "X$c" != "X$d" || test "X$e" = "X$f"; }
    ```

    `test` does not process options like most other commands do; for
    example, it does not recognize the `--` argument as marking the
    end of options.

    It is safe to use ‘`!`’ as a `test` operator. For example,
    ‘`if test ! -d foo; …`’ is portable even though ‘`if ! test
    -d foo; …`’ is not.

`test` (files)
:   To enable `configure` scripts to support cross-compilation, they
    shouldn’t do anything that tests features of the build system instead of
    the host system. But occasionally you may find it necessary to check
    whether some arbitrary file exists. To do so, use ‘`test -f`’,
    ‘`test -r`’, or ‘`test -x`’. Do not use ‘`test -e`’, because
    Solaris 10 `/bin/sh`
    lacks it.

    To test for symbolic links on systems that have them, use
    ‘`test -h`’ rather than ‘`test -L`’; either form conforms to
    POSIX, but `-h` has been around longer.

    The commands ‘`test A -ot B`’ and ‘`test A -nt B`’ are not reliable
    on macOS `sh` through at least macOS Sequoia 15.1.1 (2024),
    where ‘`test`’ ignores the subsecond part of file timestamps.
    To work around this bug, arrange for the timestamps to be at least one
    second apart.

    For historical reasons, POSIX reluctantly allows implementations of
    ‘`test -x`’ that will succeed for the root user, even if no execute
    permissions are present. Furthermore, shells do not all agree on
    whether Access Control Lists should affect ‘`test -r`’, ‘`test
    -w`’, and ‘`test -x`’; some shells base test results strictly on the
    current user id compared to file owner and mode, as if by
    `stat(2)`; while other shells base test results on whether the
    current user has the given right, even if that right is only granted by
    an ACL, as if by `faccessat(2)`. Furthermore, there is a classic
    time of check to time of use race between any use of `test`
    followed by operating on the just-checked file. Therefore, it is a good
    idea to write scripts that actually attempt an operation, and are
    prepared for the resulting failure if permission is denied, rather than
    trying to avoid an operation based solely on whether `test`
    guessed that it might not be permitted.

`test` (strings)
:   POSIX says that ‘`test "string"`’ succeeds if string is
    not null, but this usage is not portable to traditional platforms like
    Solaris 10 `/bin/sh`, which mishandle strings like ‘`!`’ and
    ‘`-n`’. However, it *is* portable to test if a variable is set
    to a non-empty value, by using ‘`test ${var+y}`’, since all known
    implementations properly distinguish between no arguments and a
    known-safe string of ‘`y`’.

    POSIX also says that ‘`test ! "string"`’,
    ‘`test -n "string"`’ and
    ‘`test -z "string"`’ work with any string, but some
    shells (such as Solaris 10) get confused if
    string looks like an operator:

    ```
    $ test -n =
    test: argument expected
    $ test ! -n
    test: argument expected
    $ test -z ")"; echo $?
    0
    ```

    Similarly, POSIX says that both ‘`test "string1" = "string2"`’
    and ‘`test "string1" != "string2"`’ work for any pairs of
    strings, but in practice this is not true for troublesome strings that
    look like operators or parentheses, or that begin with ‘`-`’.

    It is best to protect such strings with a leading ‘`X`’, e.g.,
    ‘`test "Xstring" != X`’ rather than ‘`test -n
    "string"`’ or ‘`test ! "string"`’.

    It is common to find variations of the following idiom:

    ```
    test -n "`echo $ac_feature | sed 's/[a-zA-Z0-9_-]//g'`" &&
      action
    ```

    to take an action when a token matches a given pattern. Such constructs
    should be avoided by using:

    ```
    AS_CASE([$ac_feature],
      [[*[!-a-zA-Z0-9_]*]], [action])
    ```

    If the pattern is a complicated regular expression that cannot be
    expressed as a shell pattern, use something like this instead:

    ```
    expr "X$ac_feature" : 'X.*[^-a-zA-Z0-9_]' >/dev/null &&
      action
    ```

    ‘`expr "Xfoo" : "Xbar"`’ is more robust than ‘`echo
    "Xfoo" | grep "^Xbar"`’, because it avoids problems when
    ‘`foo`’ contains backslashes.

`trap`
:   It is safe to trap at least the signals 1, 2, 13, and 15. You can also
    trap 0, i.e., have the `trap` run when the script ends (either via an
    explicit `exit`, or the end of the script). The trap for 0 should be
    installed outside of a shell function, or AIX 7.3 `/bin/sh`
    will invoke the trap at the end of this function.

    POSIX says that ‘`trap - 1 2 13 15`’ resets the traps for the
    specified signals to their default values, but many common shells (e.g.,
    Solaris 10 `/bin/sh`) misinterpret this and attempt to execute a
    “command” named `-` when the specified conditions arise.
    POSIX 2008 also added a requirement to support ‘`trap 1 2 13 15`’ to
    reset traps, as this is supported by a larger set of shells, but there
    are still shells like `dash` that mistakenly try to execute
    `1` instead of resetting the traps. Therefore, there is no
    portable workaround, except for ‘`trap - 0`’, for which
    ‘`trap '' 0`’ is a portable substitute.

    Although POSIX is not absolutely clear on this point, it is widely
    admitted that when entering the trap ‘`$?`’ should be set to the exit
    status of the last command run before the trap. The ambiguity can be
    summarized as: “when the trap is launched by an `exit`, what is
    the *last* command run: that before `exit`, or
    `exit` itself?”

    Bash considers `exit` to be the last command, while Zsh and
    Solaris 10 `/bin/sh` consider that when the trap is run it is
    *still* in the `exit`, hence it is the previous exit status
    that the trap receives:

    ```
    $ cat trap.sh
    trap 'echo $?' 0
    (exit 42); exit 0
    $ zsh trap.sh
    42
    $ bash trap.sh
    0
    ```

    The portable solution is then simple: when you want to ‘`exit 42`’,
    run ‘`(exit 42); exit 42`’, the first `exit` being used to
    set the exit status to 42 for Zsh, and the second to trigger the trap
    and pass 42 as exit status for Bash. In M4sh, this is covered by using
    `AS_EXIT`.

    The shell in FreeBSD 4.0 has the following bug: ‘`$?`’ is
    reset to 0 by empty lines if the code is inside `trap`.

    ```
    $ trap 'false

    echo $?' 0
    $ exit
    0
    ```

    Fortunately, this bug only affects `trap`.

    Several shells fail to execute an exit trap that is defined inside a
    subshell, when the last command of that subshell is not a builtin. A
    workaround is to use ‘`exit $?`’ as the shell builtin.

    ```
    $ bash -c '(trap "echo hi" 0; /bin/true)'
    hi
    $ /bin/sh -c '(trap "echo hi" 0; /bin/true)'
    $ /bin/sh -c '(trap "echo hi" 0; /bin/true; exit $?)'
    hi
    ```

    Likewise, older implementations of `bash` failed to preserve
    ‘`$?`’ across an exit trap consisting of a single cleanup command.

    ```
    $ bash -c 'trap "/bin/true" 0; exit 2'; echo $?
    2
    $ bash-2.05b -c 'trap "/bin/true" 0; exit 2'; echo $?
    0
    $ bash-2.05b -c 'trap ":; /bin/true" 0; exit 2'; echo $?
    2
    ```

    Be aware that a trap can be called from any number of places in your
    script, and therefore the trap handler should not make assumptions about
    shell state. For some examples, if your script temporarily modifies
    `IFS`, then the trap should include an initialization back to its
    typical value of space-tab-newline (autoconf does this for generated
    `configure` files). Likewise, if your script changes the current
    working directory at some point after the trap is installed, then your
    trap cannot assume which directory it is in, and should begin by
    changing directories to an absolute path if that is important to the
    cleanup efforts (autotest does this for generated `testsuite`
    files).

`true`
:   Don’t worry: as far as we know `true` is portable.
    Nevertheless, it’s not always a builtin (e.g., Bash 1.x), and the
    portable shell community tends to prefer using `:`. This has a
    funny side effect: when asked whether `false` is more portable
    than `true` Alexandre Oliva answered:

    > In a sense, yes, because if it doesn’t exist, the shell will produce an
    > exit status of failure, which is correct for `false`, but not
    > for `true`.

    Remember that even though ‘`:`’ ignores its arguments, it still takes
    time to compute those arguments. It is a good idea to use double quotes
    around any arguments to ‘`:`’ to avoid time spent in field splitting
    and file name expansion.

`unset`
:   In some nonconforming shells (e.g., Solaris 10 `/bin/ksh` and
    `/usr/xpg4/bin/sh`, NetBSD 5.99.43 sh, or Bash 2.05a),
    `unset FOO` fails when `FOO` is not set. This can interfere
    with `set -e` operation. You can use

    ```
    FOO=; unset FOO
    ```

    if you are not sure that `FOO` is set.

    A few ancient shells lack `unset` entirely. For some variables
    such as `PS1`, you can use a neutralizing value instead:

    ```
    PS1='$ '
    ```

    Usually, shells that do not support `unset` need less effort to
    make the environment sane, so for example is not a problem if you cannot
    unset `CDPATH` on those shells. However, Bash 2.01 mishandles
    `unset MAIL` and `unset MAILPATH` in some cases and dumps core.
    So, you should do something like

    ```
    ( (unset MAIL) || exit 1) >/dev/null 2>&1 && unset MAIL || :
    ```

    See [Special Shell Variables](#Special-Shell-Variables), for some neutralizing values. Also, see
    [Limitations of Builtins](#export), for
    the case of environment variables.

`wait`
:   The exit status of `wait` is not always reliable.

---

Previous: [Limitations of Shell Builtins](#Limitations-of-Builtins), Up: [Portable Shell Programming](#Portable-Shell)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
