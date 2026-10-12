### 8.4 Debugging via autom4te [¶](#Debugging-via-autom4te-1)

At times, it is desirable to see what was happening inside m4, to see
why output was not matching expectations. However, post-processing done
by `autom4te` means that directly using the m4 builtin
`m4_traceon` is likely to interfere with operation. Also, frequent
diversion changes and the concept of forbidden tokens make it difficult
to use `m4_defn` to generate inline comments in the final output.

There are a couple of tools to help with this. One is the use of the
`--trace` option provided by `autom4te` (as well as each
of the programs that wrap `autom4te`, such as
`autoconf`), in order to inspect when a macro is called and with
which arguments. For example, when this paragraph was written, the
autoconf version could be found by:

```
$ autoconf --trace=AC_INIT
configure.ac:23:AC_INIT:GNU Autoconf:2.63b.95-3963:bug-autoconf@gnu.org
$ autoconf --trace='AC_INIT:version is $2'
version is 2.63b.95-3963
```

Another trick is to print out the expansion of various m4 expressions to
standard error or to an independent file, with no further m4 expansion,
and without interfering with diversion changes or the post-processing
done to standard output. `m4_errprintn` shows a given expression
on standard error. For example, if you want to see the expansion of an
autoconf primitive or of one of your autoconf macros, you can do it like
this:

```
$ cat <<\EOF > configure.ac
AC_INIT
m4_errprintn([The definition of AC_DEFINE_UNQUOTED:])
m4_errprintn(m4_defn([AC_DEFINE_UNQUOTED]))
AC_OUTPUT
EOF
$ autoconf
error→The definition of AC_DEFINE_UNQUOTED:
error→_AC_DEFINE_Q([], $@)
```

---

Next: [Writing Autoconf Macros](#Writing-Autoconf-Macros), Previous: [Programming in M4](#Programming-in-M4), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 9 Programming in M4sh [¶](#Programming-in-M4sh-1)

M4sh, pronounced “mash”, is aiming at producing portable Bourne shell
scripts. This name was coined by Lars J. Aas, who notes that,
according to the Webster’s Revised Unabridged Dictionary (1913):

> Mash \Mash\, n. [Akin to G. meisch, maisch, meische, maische, mash,
> wash, and prob. to AS. miscian to mix. See “Mix”.]
>
> 1. A mass of mixed ingredients reduced to a soft pulpy state by beating or
>    pressure...
> 2. A mixture of meal or bran and water fed to animals.
> 3. A mess; trouble. [Obs.] –Beau. & Fl.

M4sh reserves the M4 macro namespace ‘`^_AS_`’ for internal use, and
the namespace ‘`^AS_`’ for M4sh macros. It also reserves the shell
and environment variable namespace ‘`^as_`’, and the here-document
delimiter namespace ‘`^_AS[A-Z]`’ in the output file. You should not
define your own macros or output shell code that conflicts with these
namespaces.

* [Common Shell Constructs](#Common-Shell-Constructs)
* [Support for indirect variable names](#Polymorphic-Variables)
* [Initialization Macros](#Initialization-Macros)
* [File Descriptor Macros](#File-Descriptor-Macros)

---

Next: [Support for indirect variable names](#Polymorphic-Variables), Up: [Programming in M4sh](#Programming-in-M4sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 9.1 Common Shell Constructs [¶](#Common-Shell-Constructs-1)

M4sh provides portable alternatives for some common shell constructs
that unfortunately are not portable in practice.

Macro: **AS\_BOX**(text, [char = ‘`-`’]) [¶](#index-AS_005fBOX-1)
:   Expand into shell code that will output text surrounded by a box
    with char in the top and bottom border. text should not
    contain a newline, but may contain shell expansions valid for unquoted
    here-documents. char defaults to ‘`-`’, but can be any
    character except ‘`/`’, ‘`'`’, ‘`"`’, ‘`\`’,
    ‘`&`’, or ‘`` ` ``’. This is useful for outputting a comment box into
    log files to separate distinct phases of script operation.

Macro: **AS\_CASE**(word, [pattern1], [if-matched1], …, [default]) [¶](#index-AS_005fCASE-1)
:   Expand into a shell ‘`case`’ statement, where word is matched
    against one or more patterns. if-matched is run if the
    corresponding pattern matched word, else default is run.
    See [Prerequisite Macros](#Prerequisite-Macros) for why
    this macro should be used instead of plain ‘`case`’ in code
    outside of an `AC_DEFUN` macro, when the contents of the
    ‘`case`’ use `AC_REQUIRE` directly or indirectly.
    See [Limitations of Shell Builtins](#case),
    for how this macro avoids some portability issues.
    See [Dealing with unbalanced parentheses](#Balancing-Parentheses)
    for how this macro lets you write code with balanced parentheses
    even if your code must run on obsolescent shells.

Macro: **AS\_DIRNAME**(file-name) [¶](#index-AS_005fDIRNAME-1)
:   Output the directory portion of file-name. For example,
    if `$file` is ‘`/one/two/three`’, the command
    `` dir=`AS_DIRNAME(["$file"])` `` sets `dir` to ‘`/one/two`’.

    `AS_DIRNAME` was designed long ago when
    the `dirname` command was not universally supported.
    Nowadays one can safely use `` dir=`dirname -- "$file"` `` instead.
    This interface may be improved in the future to avoid forks and losing
    trailing newlines.

Macro: **AS\_ECHO**(word) [¶](#index-AS_005fECHO-1)
:   Emit word to the standard output, followed by a newline.
    The word must be a single shell word (typically a quoted string).
    Output the shell expansion of word as-is,
    even if it starts with ‘`-`’ or contains ‘`\`’.
    Redirections can be placed outside the macro invocation.

    If the shell variable foo could contain ‘`\`’ or leading ‘`-`’.
    `AS_ECHO(["$foo"])` is more portable than `echo "$foo"`.
    See [Limitations of Shell Builtins](#echo).

    Also, `AS_ECHO(["$foo"])` is often easier to read than the
    ‘`printf '%s\n' "$foo"`’ that it stands for.
    However, because it employs ‘`'`’ characters,
    in contexts where ‘`'`’ is not allowed
    it is better to use `printf` directly.
    For example, ‘`` `eval 'foo=${'AS_ESCAPE([[$1]], [`\])'};printf
    "%s\\n" "$foo")'` ``’ would not work if `printf` were replaced
    with `AS_ECHO`.

Macro: **AS\_ECHO\_N**(word) [¶](#index-AS_005fECHO_005fN-1)
:   Act like `AS_ECHO(word)`, except do not output a following newline.

Macro: **AS\_ESCAPE**(string, [chars = ‘`` `\"$ ``’]) [¶](#index-AS_005fESCAPE-1)
:   Expands to string, with any characters in chars escaped with
    a backslash (‘`\`’). chars should be at most four bytes long,
    and only contain characters from the set ‘`` `\"$ ``’; however,
    characters may be safely listed more than once in chars for the
    sake of syntax highlighting editors. The current implementation expands
    string after adding escapes; if string contains macro calls
    that in turn expand to text needing shell quoting, you can use
    `AS_ESCAPE(m4_dquote(m4_expand([string])))`.

    The default for chars (‘`` \"$` ``’) is the set of characters
    needing escapes when string will be used literally within double
    quotes. One common variant is the set of characters to protect when
    string will be used literally within back-ticks or an unquoted
    here-document (‘`` \$` ``’). Another common variant is ‘`""`’, which can
    be used to form a double-quoted string containing the same expansions
    that would have occurred if string were expanded in an unquoted
    here-document; however, when using this variant, care must be taken that
    string does not use double quotes within complex variable
    expansions (such as ‘`` ${foo-`echo "hi"`} ``’) that would be broken
    with improper escapes.

    This macro is often used with `AS_ECHO`. For an example, observe
    the output generated by the shell code generated from this snippet:

    ```
    foo=bar
    AS_ECHO(["AS_ESCAPE(["$foo" = ])AS_ESCAPE(["$foo"], [""])"])
    ⇒"$foo" = "bar"
    m4_define([macro], [a, [\b]])
    AS_ECHO(["AS_ESCAPE([[macro]])"])
    ⇒macro
    AS_ECHO(["AS_ESCAPE([macro])"])
    ⇒a, b
    AS_ECHO(["AS_ESCAPE(m4_dquote(m4_expand([macro])))"])
    ⇒a, \b
    ```

    To escape a string that will be placed within single quotes, use:

    ```
    m4_bpatsubst([[string]], ['], ['\\''])
    ```

Macro: **AS\_EXECUTABLE\_P**(file) [¶](#index-AS_005fEXECUTABLE_005fP-1)
:   Emit code to probe whether file is a regular file with executable
    permissions (and not a directory with search permissions). The caller
    is responsible for quoting file.

Macro: **AS\_EXIT**([status = ‘`$?`’]) [¶](#index-AS_005fEXIT-1)
:   Emit code to exit the shell with status, defaulting to ‘`$?`’.
    This macro
    works around shells that see the exit status of the command prior to
    `exit` inside a ‘`trap 0`’ handler (see [Limitations
    of Shell Builtins](#trap)).

Macro: **AS\_IF**(test1, [run-if-true1], …, [run-if-false]) [¶](#index-AS_005fIF-1)
:   Run shell code test1. If test1 exits with a zero status then
    run shell code run-if-true1, else examine further tests. If no test
    exits with a zero status, run shell code run-if-false, with
    simplifications if either run-if-true1 or run-if-false
    is empty. For example,

    ```
    AS_IF([test "x$foo" = xyes], [HANDLE_FOO([yes])],
          [test "x$foo" != xno], [HANDLE_FOO([maybe])],
          [echo foo not specified])
    ```

    ensures any required macros of `HANDLE_FOO`
    are expanded before the first test.

    This macro should be used instead of plain ‘`if`’ in code
    outside of an `AC_DEFUN` macro, when the contents of the ‘`if`’
    use `AC_REQUIRE` directly or indirectly (see [Prerequisite Macros](#Prerequisite-Macros)).

Macro: **AS\_MKDIR\_P**(file-name) [¶](#index-AS_005fMKDIR_005fP-1)
:   Make the directory file-name, including intervening directories
    as necessary. This is equivalent to ‘`mkdir -p -- file-name`’.
    If creation of file-name fails, exit the script.

    Also see the `AC_PROG_MKDIR_P` macro (see [Particular Program Checks](#Particular-Programs)).

Macro: **AS\_SET\_STATUS**(status) [¶](#index-AS_005fSET_005fSTATUS-1)
:   Emit shell code to set the value of ‘`$?`’ to status, as
    efficiently as possible. However, this is not guaranteed to abort a
    shell running with `set -e` (see [Limitations of Shell
    Builtins](#set)). This should also be used at the end of a complex shell
    function instead of ‘`return`’ (see [Shell Functions](#Shell-Functions)) to avoid
    a DJGPP shell bug.

Macro: **AS\_TR\_CPP**(expression) [¶](#index-AS_005fTR_005fCPP-1)
:   Transform expression into a valid right-hand side for a C `#define`.
    For example:

    ```
    # This outputs "#define HAVE_CHAR_P 1".
    # Notice the m4 quoting around #, to prevent an m4 comment
    type="char *"
    echo "[#]define AS_TR_CPP([HAVE_$type]) 1"
    ```

Macro: **AS\_TR\_SH**(expression) [¶](#index-AS_005fTR_005fSH-1)
:   Transform expression into shell code that generates a valid shell
    variable name. The result is literal when possible at m4 time, but must
    be used with `eval` if expression causes shell indirections.
    For example:

    ```
    # This outputs "Have it!".
    header="sys/some file.h"
    eval AS_TR_SH([HAVE_$header])=yes
    if test "x$HAVE_sys_some_file_h" = xyes; then echo "Have it!"; fi
    ```

Macro: **AS\_SET\_CATFILE**(var, dir, file) [¶](#index-AS_005fSET_005fCATFILE-1)
:   Set the polymorphic shell variable var to dir/file,
    but optimizing the common cases (dir or file is ‘`.`’,
    file is absolute, etc.).

Macro: **AS\_UNSET**(var) [¶](#index-AS_005fUNSET-1)
:   Unsets the shell variable var, working around bugs in older
    shells (see [Limitations of Shell
    Builtins](#unset)). var can be a literal or indirect variable name.

Macro: **AS\_VERSION\_COMPARE**(version-1, version-2, [action-if-less], [action-if-equal], [action-if-greater]) [¶](#index-AS_005fVERSION_005fCOMPARE-1)
:   Compare two strings version-1 and version-2, possibly
    containing shell variables, as version strings, and expand
    action-if-less, action-if-equal, or action-if-greater
    depending upon the result.
    The algorithm to compare is similar to the one used by strverscmp in
    glibc (see [String/Array Comparison](https://www.gnu.org/software/libc/manual/html_mono/libc.html#String_002fArray-Comparison) in The GNU C Library).

---

Next: [Initialization Macros](#Initialization-Macros), Previous: [Common Shell Constructs](#Common-Shell-Constructs), Up: [Programming in M4sh](#Programming-in-M4sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 9.2 Support for indirect variable names [¶](#Support-for-indirect-variable-names)

Often, it is convenient to write a macro that will emit shell code
operating on a shell variable. The simplest case is when the variable
name is known. But a more powerful idiom is writing shell code that can
work through an indirection, where another variable or command
substitution produces the name of the variable to actually manipulate.
M4sh supports the notion of polymorphic shell variables, making it easy
to write a macro that can deal with either literal or indirect variable
names and output shell code appropriate for both use cases. Behavior is
undefined if expansion of an indirect variable does not result in a
literal variable name.

Macro: **AS\_LITERAL\_IF**(expression, [if-literal], [if-not], [if-simple-ref = if-not]) [¶](#index-AS_005fLITERAL_005fIF-1)

Macro: **AS\_LITERAL\_WORD\_IF**(expression, [if-literal], [if-not], [if-simple-ref = if-not]) [¶](#index-AS_005fLITERAL_005fWORD_005fIF-1)
:   If the expansion of expression is definitely a shell literal,
    expand if-literal. If the expansion of expression looks
    like it might contain shell indirections (such as `$var` or
    `` `expr` ``), then if-not is expanded. Sometimes, it is
    possible to output optimized code if expression consists only of
    shell variable expansions (such as `${var}`), in which case
    if-simple-ref can be provided; but defaulting to if-not
    should always be safe. `AS_LITERAL_WORD_IF` only expands
    if-literal if expression looks like a single shell word,
    containing no whitespace; while `AS_LITERAL_IF` allows whitespace
    in expression.

    In order to reduce the time spent recognizing whether an
    expression qualifies as a literal or a simple indirection, the
    implementation is somewhat conservative: expression must be a
    single shell word (possibly after stripping whitespace), consisting only
    of bytes that would have the same meaning whether unquoted or enclosed
    in double quotes (for example, ‘`a.b`’ results in if-literal,
    even though it is not a valid shell variable name; while both ‘`'a'`’
    and ‘`[$]`’ result in if-not, because they behave differently
    than ‘`"'a'"`’ and ‘`"[$]"`’). This macro can be used in contexts
    for recognizing portable file names (such as in the implementation of
    `AC_LIBSOURCE`), or coupled with some transliterations for forming
    valid variable names (such as in the implementation of `AS_TR_SH`,
    which uses an additional `m4_translit` to convert ‘`.`’ to
    ‘`_`’).

    This example shows how to read the contents of the shell variable
    `bar`, exercising all three arguments to `AS_LITERAL_IF`. It
    results in a script that will output the line ‘`hello`’ three times.

    ```
    AC_DEFUN([MY_ACTION],
    [AS_LITERAL_IF([$1],
      [AS_ECHO(["$$1"])],
      [AS_VAR_COPY([var], [$1])
       AS_ECHO(["$var"])],
      [AS_ECHO(["$'"$1"\"])])])
    foo=bar bar=hello
    MY_ACTION([bar])
    MY_ACTION([`echo bar`])
    MY_ACTION([$foo])
    ```

Macro: **AS\_VAR\_APPEND**(var, text) [¶](#index-AS_005fVAR_005fAPPEND-1)
:   Emit shell code to append the shell expansion of text to the end
    of the current contents of the polymorphic shell variable var,
    taking advantage of shells that provide the ‘`+=`’ extension for more
    efficient scaling.

    For situations where the final contents of var are relatively
    short (less than 256 bytes), it is more efficient to use the simpler
    code sequence of `var=${var}text` (or its
    polymorphic equivalent of `AS_VAR_COPY([t], [var])` and
    `AS_VAR_SET([var], ["$t"text])`). But in the case
    when the script will be repeatedly appending text into `var`,
    issues of scaling start to become apparent. A naive implementation
    requires execution time linear to the length of the current contents of
    var as well as the length of text for a single append, for
    an overall quadratic scaling with multiple appends. This macro takes
    advantage of shells which provide the extension
    `var+=text`, which can provide amortized constant time
    for a single append, for an overall linear scaling with multiple
    appends. Note that unlike `AS_VAR_SET`, this macro requires that
    text be quoted properly to avoid field splitting and file name
    expansion.

Macro: **AS\_VAR\_ARITH**(var, expression) [¶](#index-AS_005fVAR_005fARITH-1)
:   Emit shell code to compute the arithmetic expansion of expression,
    assigning the result as the contents of the polymorphic shell variable
    var. The code takes advantage of shells that provide ‘`$(())`’
    for fewer forks, but uses `expr` as a fallback. Therefore, the
    syntax for a valid expression is rather limited: all operators
    must occur as separate shell arguments and with proper quoting;
    the only operators supported are ‘`*`’, ‘`/`’, ‘`%`’, binary
    ‘`+`’, binary ‘`-`’, ‘`>`’, ‘`>=`’, ‘`<`’, ‘`<=`’,
    ‘`!=`’, ‘`&`’, and ‘`|`’;
    all variables containing numbers must be expanded prior to the computation;
    the first shell argument must not start with ‘`-`’;
    and each number must be an optional ‘`-`’ followed by one or more
    decimal digits, where the first digit is nonzero if there is more than
    one digit. In the following example, this snippet
    will print ‘`(2+3)*4 == 20`’.

    ```
    bar=3
    AS_VAR_ARITH([foo], [\( 2 + $bar \) \* 4])
    echo "(2+$bar)*4 == $foo"
    ```

Macro: **AS\_VAR\_COPY**(dest, source) [¶](#index-AS_005fVAR_005fCOPY-1)
:   Emit shell code to assign the contents of the polymorphic shell variable
    source to the polymorphic shell variable dest. For example,
    executing this M4sh snippet will output ‘`bar hi`’:

    ```
    foo=bar bar=hi
    AS_VAR_COPY([a], [foo])
    AS_VAR_COPY([b], [$foo])
    echo "$a $b"
    ```

    When it is necessary to access the contents of an indirect variable
    inside a shell double-quoted context, the recommended idiom is to first
    copy the contents into a temporary literal shell variable.

    ```
    for header in stdint_h inttypes_h ; do
      AS_VAR_COPY([var], [ac_cv_header_$header])
      AS_ECHO(["$header detected: $var"])
    done
    ```

Macro: **AS\_VAR\_IF**(var, [word], [if-equal], [if-not-equal]) [¶](#index-AS_005fVAR_005fIF-1)
:   Output a shell conditional statement. If the contents of the
    polymorphic shell variable var match the string word,
    execute if-equal; otherwise execute if-not-equal. word
    must be a single shell word (typically a quoted string). Avoids
    shell bugs if an interrupt signal arrives while a command substitution
    in var is being expanded.

Macro: **AS\_VAR\_PUSHDEF**(m4-name, value) [¶](#index-AS_005fVAR_005fPUSHDEF-1)

Macro: **AS\_VAR\_POPDEF**(m4-name) [¶](#index-AS_005fVAR_005fPOPDEF-1)
:   A common M4sh idiom involves composing shell variable names from an m4
    argument (for example, writing a macro that uses a cache variable).
    value can be an arbitrary string, which will be transliterated
    into a valid shell name by `AS_TR_SH`. In order to access the
    composed variable name based on value, it is easier to declare a
    temporary m4 macro m4-name with `AS_VAR_PUSHDEF`, then use
    that macro as the argument to subsequent `AS_VAR` macros as a
    polymorphic variable name, and finally free the temporary macro with
    `AS_VAR_POPDEF`. These macros are often followed with `dnl`,
    to avoid excess newlines in the output.

    Here is an involved example, that shows the power of writing macros that
    can handle composed shell variable names:

    ```
    m4_define([MY_CHECK_HEADER],
    [AS_VAR_PUSHDEF([my_Header], [ac_cv_header_$1])dnl
    AS_VAR_IF([my_Header], [yes], [echo "header $1 detected"])dnl
    AS_VAR_POPDEF([my_Header])dnl
    ])
    MY_CHECK_HEADER([stdint.h])
    for header in inttypes.h stdlib.h ; do
      MY_CHECK_HEADER([$header])
    done
    ```

    In the above example, `MY_CHECK_HEADER` can operate on polymorphic
    variable names. In the first invocation, the m4 argument is
    `stdint.h`, which transliterates into a literal `stdint_h`.
    As a result, the temporary macro `my_Header` expands to the literal
    shell name ‘`ac_cv_header_stdint_h`’. In the second invocation, the
    m4 argument to `MY_CHECK_HEADER` is `$header`, and the
    temporary macro `my_Header` expands to the indirect shell name
    ‘`$as_my_Header`’. During the shell execution of the for loop, when
    ‘`$header`’ contains ‘`inttypes.h`’, then ‘`$as_my_Header`’
    contains ‘`ac_cv_header_inttypes_h`’. If this script is then run on a
    platform where all three headers have been previously detected, the
    output of the script will include:

    ```
    header stdint.h detected
    header inttypes.h detected
    header stdlib.h detected
    ```

Macro: **AS\_VAR\_SET**(var, [value]) [¶](#index-AS_005fVAR_005fSET-1)
:   Emit shell code to assign the contents of the polymorphic shell variable
    var to the shell expansion of value. value is not
    subject to field splitting or file name expansion, so if command
    substitution is used, it may be done with ‘`` `""` ``’ rather than using
    an intermediate variable (see [Shell Substitutions](#Shell-Substitutions)). However,
    value does undergo rescanning for additional macro names; behavior
    is unspecified if late expansion results in any shell meta-characters.

Macro: **AS\_VAR\_SET\_IF**(var, [if-set], [if-undef]) [¶](#index-AS_005fVAR_005fSET_005fIF-1)
:   Emit a shell conditional statement, which executes if-set if the
    polymorphic shell variable `var` is set to any value, and
    if-undef otherwise.

Macro: **AS\_VAR\_TEST\_SET**(var) [¶](#index-AS_005fVAR_005fTEST_005fSET-1)
:   Emit a shell statement that results in a successful exit status only if
    the polymorphic shell variable `var` is set.

---

Next: [File Descriptor Macros](#File-Descriptor-Macros), Previous: [Support for indirect variable names](#Polymorphic-Variables), Up: [Programming in M4sh](#Programming-in-M4sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 9.3 Initialization Macros [¶](#Initialization-Macros-1)

Macro: **AS\_BOURNE\_COMPATIBLE** [¶](#index-AS_005fBOURNE_005fCOMPATIBLE-1)
:   Set up the shell to be more compatible with the Bourne shell as
    standardized by POSIX, if possible. This may involve setting
    environment variables, or setting options, or similar
    implementation-specific actions. This macro is deprecated, since
    `AS_INIT` already invokes it.

Macro: **AS\_INIT** [¶](#index-AS_005fINIT-1)
:   Initialize the M4sh environment. This macro calls `m4_init`, then
    outputs the `#! /bin/sh` line, a notice about where the output was
    generated from, and code to sanitize the environment for the rest of the
    script. Among other initializations, this sets `SHELL` to the shell
    chosen to run the script (see [CONFIG\_SHELL](#CONFIG_005fSHELL)), and `LC_ALL` to
    ensure the C locale. Finally, it changes the current diversion to
    `BODY`. `AS_INIT` is called automatically by `AC_INIT`
    and `AT_INIT`, so shell code in `configure`,
    `config.status`, and `testsuite` all benefit from a sanitized
    shell environment.

Macro: **AS\_INIT\_GENERATED**(file, [comment]) [¶](#index-AS_005fINIT_005fGENERATED-1)
:   Emit shell code to start the creation of a subsidiary shell script in
    file, including changing file to be executable. This macro
    populates the child script with information learned from the parent
    (thus, the emitted code is equivalent in effect, but more efficient,
    than the code output by `AS_INIT`, `AS_BOURNE_COMPATIBLE`, and
    `AS_SHELL_SANITIZE`). If present, comment is output near the
    beginning of the child, prior to the shell initialization code, and is
    subject to parameter expansion, command substitution, and backslash
    quote removal. The
    parent script should check the exit status after this macro, in case
    file could not be properly created (for example, if the disk was
    full). If successfully created, the parent script can then proceed to
    append additional M4sh constructs into the child script.

    Note that the child script starts life without a log file open, so if
    the parent script uses logging (see [AS\_MESSAGE\_LOG\_FD](#AS_005fMESSAGE_005fLOG_005fFD)), you
    must temporarily disable any attempts to use the log file until after
    emitting code to open a log within the child. On the other hand, if the
    parent script has `AS_MESSAGE_FD` redirected somewhere besides
    ‘`1`’, then the child script already has code that copies stdout to
    that descriptor. Currently, the suggested
    idiom for writing a M4sh shell script from within another script is:

    ```
    AS_INIT_GENERATED([file], [[# My child script.
    ]]) || { AS_ECHO(["Failed to create child script"]); AS_EXIT; }
    m4_pushdef([AS_MESSAGE_LOG_FD])dnl
    cat >> "file" <<\__EOF__
    # Code to initialize AS_MESSAGE_LOG_FD
    m4_popdef([AS_MESSAGE_LOG_FD])dnl
    # Additional code
    __EOF__
    ```

    This, however, may change in the future as the M4sh interface is
    stabilized further.

    Also, be aware that use of `LINENO` within the child script may
    report line numbers relative to their location in the parent script,
    even when using `AS_LINENO_PREPARE`, if the parent script was
    unable to locate a shell with working `LINENO` support.

Macro: **AS\_LINENO\_PREPARE** [¶](#index-AS_005fLINENO_005fPREPARE-1)
:   Find a shell that supports the special variable `LINENO`, which
    contains the number of the currently executing line. This macro is
    automatically invoked by `AC_INIT` in configure scripts.

Macro: **AS\_ME\_PREPARE** [¶](#index-AS_005fME_005fPREPARE-1)
:   Set up variable `as_me` to be the basename of the currently executing
    script. This macro is automatically invoked by `AC_INIT` in
    configure scripts.

Macro: **AS\_TMPDIR**(prefix, [dir = ‘`${TMPDIR:=/tmp}`’]) [¶](#index-AS_005fTMPDIR-1)
:   Create, as safely as possible, a temporary sub-directory within
    dir with a name starting with prefix. prefix should
    be 2–4 characters, to make it slightly easier to identify the owner of
    the directory. If dir is omitted, then the value of `TMPDIR`
    will be used (defaulting to ‘`/tmp`’). On success, the name of the
    newly created directory is stored in the shell variable `tmp`. On
    error, the script is aborted.

    Typically, this macro is coupled with some exit traps to delete the created
    directory and its contents on exit or interrupt. However, there is a
    slight window between when the directory is created and when the name is
    actually known to the shell, so an interrupt at the right moment might
    leave the temporary directory behind. Hence it is important to use a
    prefix that makes it easier to determine if a leftover temporary
    directory from an interrupted script is safe to delete.

    If you set `TMPDIR=$tmp` after invoking this macro, you should
    reset `TMPDIR` before deleting the created directory, to avoid
    breaking commands that rely on `$TMPDIR`.

    The use of the output variable ‘`$tmp`’ rather than something in the
    ‘`as_`’ namespace is historical; it has the unfortunate consequence
    that reusing this otherwise common name for any other purpose inside
    your script has the potential to break any cleanup traps designed to
    remove the temporary directory.

Macro: **AS\_SHELL\_SANITIZE** [¶](#index-AS_005fSHELL_005fSANITIZE-1)
:   Initialize the shell suitably for `configure` scripts. This has
    the effect of `AS_BOURNE_COMPATIBLE`, and sets some other
    environment variables for predictable results from configuration tests.
    For example, it sets `LC_ALL` to change to the default C locale.
    See [Special Shell Variables](#Special-Shell-Variables). This macro is deprecated, since
    `AS_INIT` already invokes it.

---

Previous: [Initialization Macros](#Initialization-Macros), Up: [Programming in M4sh](#Programming-in-M4sh)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 9.4 File Descriptor Macros [¶](#File-Descriptor-Macros-1)

The following macros define file descriptors used to output messages
(or input values) from `configure` scripts.
For example:

```
AS_ECHO(["$wombats found"]) >&AS_MESSAGE_LOG_FD
AS_ECHO_N(['Enter desired kangaroo count: ']) >&AS_MESSAGE_FD
read kangaroos <&AS_ORIGINAL_STDIN_FD
```

However doing so is seldom needed, because Autoconf provides higher
level macros as described below.

Macro: **AS\_MESSAGE\_FD** [¶](#index-AS_005fMESSAGE_005fFD-1)
:   The file descriptor for ‘`checking for...`’ messages and results.
    By default, `AS_INIT` sets this to ‘`1`’ for standalone M4sh
    clients. However, `AC_INIT` shuffles things around to another file
    descriptor, in order to allow the `-q` option of
    `configure` to choose whether messages should go to the script’s
    standard output or be discarded.

    If you want to display some messages, consider using one of the printing
    macros (see [Printing Messages](#Printing-Messages)) instead. Copies of messages output
    via these macros are also recorded in `config.log`.

Macro: **AS\_MESSAGE\_LOG\_FD** [¶](#index-AS_005fMESSAGE_005fLOG_005fFD-1)
:   This must either be empty, or expand to a file descriptor for log
    messages. By default, `AS_INIT` sets this macro to the empty
    string for standalone M4sh clients, thus disabling logging. However,
    `AC_INIT` shuffles things around so that both `configure`
    and `config.status` use `config.log` for log messages.
    Macros that run tools, like `AC_COMPILE_IFELSE` (see [Running the Compiler](#Running-the-Compiler)), redirect all output to this descriptor. You may want to do
    so if you develop such a low-level macro.

Macro: **AS\_ORIGINAL\_STDIN\_FD** [¶](#index-AS_005fORIGINAL_005fSTDIN_005fFD-1)
:   This must expand to a file descriptor for the original standard input.
    By default, `AS_INIT` sets this macro to ‘`0`’ for standalone
    M4sh clients. However, `AC_INIT` shuffles things around for
    safety.

    When `configure` runs, it may accidentally execute an
    interactive command that has the same name as the non-interactive meant
    to be used or checked. If the standard input was the terminal, such
    interactive programs would cause `configure` to stop, pending
    some user input. Therefore `configure` redirects its standard
    input from `/dev/null` during its initialization. This is not
    normally a problem, since `configure` normally does not need
    user input.

    In the extreme case where your `configure` script really needs to
    obtain some values from the original standard input, you can read them
    explicitly from `AS_ORIGINAL_STDIN_FD`.

---

Next: [Portable Shell Programming](#Portable-Shell), Previous: [Programming in M4sh](#Programming-in-M4sh), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 10 Writing Autoconf Macros [¶](#Writing-Autoconf-Macros-1)

When you write a feature test that could be applicable to more than one
software package, the best thing to do is encapsulate it in a new macro.
Here are some instructions and guidelines for writing Autoconf macros.
You should also familiarize yourself with M4sugar (see [Programming in M4](#Programming-in-M4))
and M4sh (see [Programming in M4sh](#Programming-in-M4sh)).

* [Macro Definitions](#Macro-Definitions)
* [Macro Names](#Macro-Names)
* [Dependencies Between Macros](#Dependencies-Between-Macros)
* [Obsoleting Macros](#Obsoleting-Macros)
* [Coding Style](#Coding-Style)

---

Next: [Macro Names](#Macro-Names), Up: [Writing Autoconf Macros](#Writing-Autoconf-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 10.1 Macro Definitions [¶](#Macro-Definitions-1)

Macro: **AC\_DEFUN**(name, [body]) [¶](#index-AC_005fDEFUN-1)
:   Autoconf macros are defined using the `AC_DEFUN` macro, which is
    similar to the M4 builtin `m4_define` macro; this creates a macro
    named name and with body as its expansion. In addition to
    defining a macro, `AC_DEFUN` adds to it some code that is used to
    constrain the order in which macros are called, while avoiding redundant
    output (see [Prerequisite Macros](#Prerequisite-Macros)).

An Autoconf macro definition looks like this:

```
AC_DEFUN(macro-name, macro-body)
```

You can refer to any arguments passed to the macro as ‘`$1`’,
‘`$2`’, etc. See [How to define new macros](https://www.gnu.org/software/m4/manual/m4.html#Definitions) in GNU M4, for more complete information on writing M4 macros.

Most macros fall in one of two general categories. The first category
includes macros which take arguments, in order to generate output
parameterized by those arguments. Macros in this category are designed
to be directly expanded, often multiple times, and should not be used as
the argument to `AC_REQUIRE`. The other category includes macros
which are shorthand for a fixed block of text, and therefore do not take
arguments. For this category of macros, directly expanding the macro
multiple times results in redundant output, so it is more common to use
the macro as the argument to `AC_REQUIRE`, or to declare the macro
with `AC_DEFUN_ONCE` (see [One-Shot Macros](#One_002dShot-Macros)).

Be sure to properly quote both the macro-body *and* the
macro-name to avoid any problems if the macro happens to have
been previously defined.

Each macro should have a header comment that gives its prototype, and a
brief description. When arguments have default values, display them in
the prototype. For example:

```
# AC_MSG_ERROR(ERROR, [EXIT-STATUS = 1])
# --------------------------------------
m4_define([AC_MSG_ERROR],
  [{ AS_MESSAGE([error: $1], [2])
     exit m4_default([$2], [1]); }])
```

Comments about the macro should be left in the header comment. Most
other comments make their way into `configure`, so just keep
using ‘`#`’ to introduce comments.

If you have some special comments about pure M4 code, comments
that make no sense in `configure` and in the header comment, then
use the builtin `dnl`: it causes M4 to discard the text
through the next newline.

Keep in mind that `dnl` is rarely needed to introduce comments;
`dnl` is more useful to get rid of the newlines following macros
that produce no output, such as `AC_REQUIRE`.

Public third-party macros need to use `AC_DEFUN`, and not
`m4_define`, in order to be found by `aclocal`
(see [Extending aclocal](https://www.gnu.org/software/automake/manual/automake.html#Extending-aclocal) in GNU Automake).
Additionally, if it is ever determined that a macro should be made
obsolete, it is easy to convert from `AC_DEFUN` to `AU_DEFUN`
in order to have `autoupdate` assist the user in choosing a
better alternative, but there is no corresponding way to make
`m4_define` issue an upgrade notice (see [AU\_DEFUN](#AU_005fDEFUN)).

There is another subtle, but important, difference between using
`m4_define` and `AC_DEFUN`: only the former is unaffected by
`AC_REQUIRE`. When writing a file, it is always safe to replace a
block of text with a `m4_define` macro that will expand to the same
text. But replacing a block of text with an `AC_DEFUN` macro with
the same content does not necessarily give the same results, because it
changes the location where any embedded but unsatisfied
`AC_REQUIRE` invocations within the block will be expanded. For an
example of this, see [Expanded Before Required](#Expanded-Before-Required).

---

Next: [Dependencies Between Macros](#Dependencies-Between-Macros), Previous: [Macro Definitions](#Macro-Definitions), Up: [Writing Autoconf Macros](#Writing-Autoconf-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 10.2 Macro Names [¶](#Macro-Names-1)

All of the public Autoconf macros have all-uppercase names in the
namespace ‘`^AC_`’ to prevent them from accidentally conflicting with
other text; Autoconf also reserves the namespace ‘`^_AC_`’ for
internal macros. All shell variables that they use for internal
purposes have mostly-lowercase names starting with ‘`ac_`’. Autoconf
also uses here-document delimiters in the namespace ‘`^_AC[A-Z]`’. During
`configure`, files produced by Autoconf make heavy use of the
file system namespace ‘`^conf`’.

Since Autoconf is built on top of M4sugar (see [Programming in M4sugar](#Programming-in-M4sugar)) and M4sh (see [Programming in M4sh](#Programming-in-M4sh)), you must also be aware
of those namespaces (‘`^_?\(m4\|AS\)_`’). And since
`configure.ac` is also designed to be scanned by Autoheader,
Autoscan, Autoupdate, and Automake, you should be aware of the
‘`^_?A[HNUM]_`’ namespaces. In general, you *should not use*
the namespace of a package that does not own the macro or shell code you
are writing.

To ensure that your macros don’t conflict with present or future
Autoconf macros, you should prefix your own macro names and any shell
variables they use with some other sequence. Possibilities include your
initials, or an abbreviation for the name of your organization or
software package. Historically, people have not always followed the
rule of using a namespace appropriate for their package, and this has
made it difficult for determining the origin of a macro (and where to
report bugs about that macro), as well as difficult for the true
namespace owner to add new macros without interference from pre-existing
uses of third-party macros. Perhaps the best example of this confusion
is the `AM_GNU_GETTEXT` macro, which belongs, not to Automake, but
to Gettext.

Most of the Autoconf macros’ names follow a structured naming convention
that indicates the kind of feature check by the name. The macro names
consist of several words, separated by underscores, going from most
general to most specific. The names of their cache variables use the
same convention (see [Cache Variable Names](#Cache-Variable-Names), for more information on
them).

The first word of the name after the namespace initials (such as
‘`AC_`’) usually tells the category
of the feature being tested. Here are the categories used in Autoconf for
specific test macros, the kind of macro that you are more likely to
write. They are also used for cache variables, in all-lowercase. Use
them where applicable; where they’re not, invent your own categories.

`C`
:   C language builtin features.

`DECL`
:   Declarations of C variables in header files.

`FUNC`
:   Functions in libraries.

`GROUP`
:   POSIX group owners of files.

`HEADER`
:   Header files.

`LIB`
:   C libraries.

`PROG`
:   The base names of programs.

`MEMBER`
:   Members of aggregates.

`SYS`
:   Operating system features.

`TYPE`
:   C builtin or declared types.

`VAR`
:   C variables in libraries.

After the category comes the name of the particular feature being
tested. Any further words in the macro name indicate particular aspects
of the feature. For example, `AC_PROG_MAKE_SET` checks whether
`make` sets a variable to its own name.

An internal macro should have a name that starts with an underscore;
Autoconf internals should therefore start with ‘`_AC_`’.
Additionally, a macro that is an internal subroutine of another macro
should have a name that starts with an underscore and the name of that
other macro, followed by one or more words saying what the internal
macro does. For example, `AC_PATH_X` has internal macros
`_AC_PATH_X_XMKMF` and `_AC_PATH_X_DIRECT`.

---

Next: [Obsoleting Macros](#Obsoleting-Macros), Previous: [Macro Names](#Macro-Names), Up: [Writing Autoconf Macros](#Writing-Autoconf-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 10.3 Dependencies Between Macros [¶](#Dependencies-Between-Macros-1)

Some Autoconf macros depend on other macros having been called first in
order to work correctly. Autoconf provides a way to ensure that certain
macros are called if needed and a way to warn the user if macros are
called in an order that might cause incorrect operation.

* [Prerequisite Macros](#Prerequisite-Macros)
* [Suggested Ordering](#Suggested-Ordering)
* [One-Shot Macros](#One_002dShot-Macros)

---

Next: [Suggested Ordering](#Suggested-Ordering), Up: [Dependencies Between Macros](#Dependencies-Between-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 10.3.1 Prerequisite Macros [¶](#Prerequisite-Macros-1)

A macro that you write might need to use values that have previously
been computed by other macros. For example, `AC_DECL_YYTEXT`
examines the output of `flex` or `lex`, so it depends on
`AC_PROG_LEX` having been called first to set the shell variable
`LEX`.

Rather than forcing the user of the macros to keep track of the
dependencies between them, you can use the `AC_REQUIRE` macro to do
it automatically. `AC_REQUIRE` can ensure that a macro is only
called if it is needed, and only called once.

Macro: **AC\_REQUIRE**(macro-name) [¶](#index-AC_005fREQUIRE-1)
:   If the M4 macro macro-name has not already been called, call it
    (without any arguments). Make sure to quote macro-name with
    square brackets. macro-name must have been defined using
    `AC_DEFUN` or else contain a call to `AC_PROVIDE` to indicate
    that it has been called.

    `AC_REQUIRE` must be used inside a macro defined by `AC_DEFUN`; it
    must not be called from the top level. Also, it does not make sense to
    require a macro that takes parameters.

`AC_REQUIRE` is often misunderstood. It really implements
dependencies between macros in the sense that if one macro depends upon
another, the latter is expanded *before* the body of the
former. To be more precise, the required macro is expanded before
the outermost defined macro in the current expansion stack.
In particular, ‘`AC_REQUIRE([FOO])`’ is not replaced with the body of
`FOO`. For instance, this definition of macros:

```
AC_DEFUN([TRAVOLTA],
[test "$body_temperature_in_Celsius" -gt 38 &&
  dance_floor=occupied])
AC_DEFUN([NEWTON_JOHN],
[test "x$hair_style" = xcurly &&
  dance_floor=occupied])
```

```

```

```
AC_DEFUN([RESERVE_DANCE_FLOOR],
[if test "x`date +%A`" = xSaturday; then
  AC_REQUIRE([TRAVOLTA])
  AC_REQUIRE([NEWTON_JOHN])
fi])
```

with this `configure.ac`

```
AC_INIT([Dance Manager], [1.0], [bug-dance@example.org])
RESERVE_DANCE_FLOOR
if test "x$dance_floor" = xoccupied; then
  AC_MSG_ERROR([cannot pick up here, let's move])
fi
```

does not leave you with a better chance to meet a kindred soul on
days other than Saturday, since the call to `RESERVE_DANCE_FLOOR`
expands to:

```
test "$body_temperature_in_Celsius" -gt 38 &&
  dance_floor=occupied
test "x$hair_style" = xcurly &&
  dance_floor=occupied
if test "x`date +%A`" = xSaturday; then

fi
```

This behavior was chosen on purpose: (i) it prevents messages in
required macros from interrupting the messages in the requiring macros;
(ii) it avoids bad surprises when shell conditionals are used, as in:

```
if ...; then
  AC_REQUIRE([SOME_CHECK])
fi
...
SOME_CHECK
```

However, this implementation can lead to another class of problems.
Consider the case where an outer macro first expands, then indirectly
requires, an inner macro:

```
AC_DEFUN([TESTA], [[echo in A
if test -n "$SEEN_A" ; then echo duplicate ; fi
SEEN_A=:]])
AC_DEFUN([TESTB], [AC_REQUIRE([TESTA])[echo in B
if test -z "$SEEN_A" ; then echo bug ; fi]])
AC_DEFUN([TESTC], [AC_REQUIRE([TESTB])[echo in C]])
AC_DEFUN([OUTER], [[echo in OUTER]
TESTA
TESTC])
OUTER
```

Prior to Autoconf 2.64, the implementation of `AC_REQUIRE`
recognized that `TESTB` needed to be hoisted prior to the expansion
of `OUTER`, but because `TESTA` had already been directly
expanded, it failed to hoist `TESTA`. Therefore, the expansion of
`TESTB` occurs prior to its prerequisites, leading to the following
output:

```
in B
bug
in OUTER
in A
in C
```

Newer Autoconf is smart enough to recognize this situation, and hoists
`TESTA` even though it has already been expanded, but issues a
syntax warning in the process. This is because the hoisted expansion of
`TESTA` defeats the purpose of using `AC_REQUIRE` to avoid
redundant code, and causes its own set of problems if the hoisted macro
is not idempotent:

```
in A
in B
in OUTER
in A
duplicate
in C
```

The bug is not in Autoconf, but in the macro definitions. If you ever
pass a particular macro name to `AC_REQUIRE`, then you are implying
that the macro only needs to be expanded once. But to enforce this,
either the macro must be declared with `AC_DEFUN_ONCE` (although
this only helps in Autoconf 2.64 or newer), or all
uses of that macro should be through `AC_REQUIRE`; directly
expanding the macro defeats the point of using `AC_REQUIRE` to
eliminate redundant expansion. In the example, this rule of thumb was
violated because `TESTB` requires `TESTA` while `OUTER`
directly expands it. One way of fixing the bug is to factor
`TESTA` into two macros, the portion designed for direct and
repeated use (here, named `TESTA`), and the portion designed for
one-shot output and used only inside `AC_REQUIRE` (here, named
`TESTA_PREREQ`). Then, by fixing all clients to use the correct
calling convention according to their needs:

```
AC_DEFUN([TESTA], [AC_REQUIRE([TESTA_PREREQ])[echo in A]])
AC_DEFUN([TESTA_PREREQ], [[echo in A_PREREQ
if test -n "$SEEN_A" ; then echo duplicate ; fi
SEEN_A=:]])
AC_DEFUN([TESTB], [AC_REQUIRE([TESTA_PREREQ])[echo in B
if test -z "$SEEN_A" ; then echo bug ; fi]])
AC_DEFUN([TESTC], [AC_REQUIRE([TESTB])[echo in C]])
AC_DEFUN([OUTER], [[echo in OUTER]
TESTA
TESTC])
OUTER
```

the resulting output will then obey all dependency rules and avoid any
syntax warnings, whether the script is built with old or new Autoconf
versions:

```
in A_PREREQ
in B
in OUTER
in A
in C
```

You can use the helper macros `AS_IF` and `AS_CASE` in
top-level code to enforce expansion of required macros outside of shell
conditional constructs; these helpers are not needed in the bodies of
macros defined by `AC_DEFUN`.
You are furthermore encouraged, although not required, to
put all `AC_REQUIRE` calls
at the beginning of a macro. You can use `dnl` to avoid the empty
lines they leave.

Autoconf will normally warn if an `AC_REQUIRE` call refers to a
macro that has not been defined. However, the `aclocal` tool
relies on parsing an incomplete set of input files to trace which macros
have been required, in order to then pull in additional files that
provide those macros; for this particular use case, pre-defining the
macro `m4_require_silent_probe` will avoid the warnings.

---

Next: [One-Shot Macros](#One_002dShot-Macros), Previous: [Prerequisite Macros](#Prerequisite-Macros), Up: [Dependencies Between Macros](#Dependencies-Between-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 10.3.2 Suggested Ordering [¶](#Suggested-Ordering-1)

Some macros should be run before another macro if both are called, but
neither *requires* that the other be called. For example, a macro
that changes the behavior of the C compiler should be called before any
macros that run the C compiler. Many of these dependencies are noted in
the documentation.

Autoconf provides the `AC_BEFORE` macro to warn users when macros
with this kind of dependency appear out of order in a
`configure.ac` file. The warning occurs when creating
`configure` from `configure.ac`, not when running
`configure`.

For example, `AC_PROG_CPP` checks whether the C compiler
can run the C preprocessor when given the `-E` option. It should
therefore be called after any macros that change which C compiler is
being used, such as `AC_PROG_CC`. So `AC_PROG_CC` contains:

```
AC_BEFORE([$0], [AC_PROG_CPP])dnl
```

This warns the user if a call to `AC_PROG_CPP` has already occurred
when `AC_PROG_CC` is called.

Macro: **AC\_BEFORE**(this-macro-name, called-macro-name) [¶](#index-AC_005fBEFORE-1)
:   Make M4 print a warning message to the standard error output if
    called-macro-name has already been called. this-macro-name
    should be the name of the macro that is calling `AC_BEFORE`. The
    macro called-macro-name must have been defined using
    `AC_DEFUN` or else contain a call to `AC_PROVIDE` to indicate
    that it has been called.

---

Previous: [Suggested Ordering](#Suggested-Ordering), Up: [Dependencies Between Macros](#Dependencies-Between-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 10.3.3 One-Shot Macros [¶](#One_002dShot-Macros-1)

Some macros should be called only once, either because calling them
multiple time is unsafe, or because it is bad style. For instance
Autoconf ensures that `AC_CANONICAL_BUILD` and cousins
(see [Getting the Canonical System Type](#Canonicalizing)) are evaluated only once, because it makes no
sense to run these expensive checks more than once. Such one-shot
macros can be defined using `AC_DEFUN_ONCE`.

Macro: **AC\_DEFUN\_ONCE**(macro-name, macro-body) [¶](#index-AC_005fDEFUN_005fONCE-1)
:   Declare macro macro-name like `AC_DEFUN` would (see [Macro Definitions](#Macro-Definitions)), but add additional logic that guarantees that only the
    first use of the macro (whether by direct expansion or
    `AC_REQUIRE`) causes an expansion of macro-body; the
    expansion will occur before the start of any enclosing macro defined by
    `AC_DEFUN`. Subsequent expansions are silently ignored.
    Generally, it does not make sense for macro-body to use parameters
    such as `$1`.

Prior to Autoconf 2.64, a macro defined by `AC_DEFUN_ONCE` would
emit a warning if it was directly expanded a second time, so for
portability, it is better to use `AC_REQUIRE` than direct
invocation of macro-name inside a macro defined by `AC_DEFUN`
(see [Prerequisite Macros](#Prerequisite-Macros)).

---

Next: [Coding Style](#Coding-Style), Previous: [Dependencies Between Macros](#Dependencies-Between-Macros), Up: [Writing Autoconf Macros](#Writing-Autoconf-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 10.4 Obsoleting Macros [¶](#Obsoleting-Macros-1)

Configuration and portability technology has evolved over the years.
Often better ways of solving a particular problem are developed, or
ad-hoc approaches are systematized. This process has occurred in many
parts of Autoconf. One result is that some of the macros are now
considered *obsolete*; they still work, but are no longer considered
the best thing to do, hence they should be replaced with more modern
macros. Ideally, `autoupdate` should replace the old macro calls
with their modern implementation.

Autoconf provides a simple means to obsolete a macro.

Macro: **AU\_DEFUN**(old-macro, implementation, [message], [silent]) [¶](#index-AU_005fDEFUN-1)
:   Define old-macro as implementation, just like
    `AC_DEFUN`, but also declare old-macro to be obsolete.
    When `autoupdate` is run, occurrences of old-macro will
    be replaced by the text of implementation in the updated
    `configure.ac` file.

    If a simple textual replacement is not enough to finish the job of
    updating a `configure.ac` to modern style, provide instructions for
    whatever additional manual work is required as message. These
    instructions will be printed by `autoupdate`, and embedded in the
    updated `configure.ac` file, next to the text of implementation.

    Normally, `autoconf` will also issue a warning (in the
    “obsolete” category) when it expands old-macro. This warning
    does not include message; it only advises the maintainer to run
    `autoupdate`. If it is inappropriate to issue this warning, set
    the silent argument to the word `silent`. One might want to
    use a silent `AU_DEFUN` when old-macro is used in a
    widely-distributed third-party macro. If that macro’s maintainers are
    aware of the need to update their code, it’s unnecessary to nag all
    of the transitive users of old-macro as well. This capability
    was added to `AU_DEFUN` in Autoconf 2.70; older versions of
    autoconf will ignore the silent argument and issue the warning
    anyway.

    **Caution:** If implementation contains M4 or M4sugar macros,
    they will be evaluated when `autoupdate` is run, not emitted
    verbatim like the rest of implementation. This cannot be avoided
    with extra quotation, because then old-macro will not work when
    it is called normally. See the definition of `AC_FOREACH` in
    `general.m4` for a workaround.

Macro: **AU\_ALIAS**(old-name, new-name, [silent]) [¶](#index-AU_005fALIAS-1)
:   A shorthand version of `AU_DEFUN`, to be used when a macro has
    simply been renamed. `autoupdate` will replace calls to
    old-name with calls to new-name, keeping any arguments
    intact. No instructions for additional manual work will be printed.

    The silent argument works the same as the silent argument
    to `AU_DEFUN`. It was added to `AU_ALIAS` in Autoconf 2.70.

    **Caution:** `AU_ALIAS` cannot be used when new-name is
    an M4 or M4sugar macro. See above.

---

Previous: [Obsoleting Macros](#Obsoleting-Macros), Up: [Writing Autoconf Macros](#Writing-Autoconf-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 10.5 Coding Style [¶](#Coding-Style-1)

The Autoconf macros follow a strict coding style. You are encouraged to
follow this style, especially if you intend to distribute your macro,
either by contributing it to Autoconf itself or the
[Autoconf Macro
Archive](https://www.gnu.org/software/autoconf-archive/), or by other means.

The first requirement is to pay great attention to the quotation. For
more details, see [The Autoconf Language](#Autoconf-Language), and [M4 Quotation](#M4-Quotation).

Do not try to invent new interfaces. It is likely that there is a macro
in Autoconf that resembles the macro you are defining: try to stick to
this existing interface (order of arguments, default values, etc.). We
*are* conscious that some of these interfaces are not perfect;
nevertheless, when harmless, homogeneity should be preferred over
creativity.

Be careful about clashes both between M4 symbols and between shell
variables.

If you stick to the suggested M4 naming scheme (see [Macro Names](#Macro-Names)),
you are unlikely to generate conflicts. Nevertheless, when you need to
set a special value, *avoid using a regular macro name*; rather,
use an “impossible” name. For instance, up to version 2.13, the macro
`AC_SUBST` used to remember what symbol macros were already defined
by setting `AC_SUBST_symbol`, which is a regular macro name.
But since there is a macro named `AC_SUBST_FILE`, it was just
impossible to ‘`AC_SUBST(FILE)`’! In this case,
`AC_SUBST(symbol)` or `_AC_SUBST(symbol)` should
have been used (yes, with the parentheses).

No Autoconf macro should ever enter the user-variable name space; i.e.,
except for the variables that are the actual result of running the
macro, all shell variables should start with `ac_`. In
addition, small macros or any macro that is likely to be embedded in
other macros should be careful not to use obvious names.

Do not use `dnl` to introduce comments: most of the comments you
are likely to write are either header comments which are not output
anyway, or comments that should make their way into `configure`.
There are exceptional cases where you do want to comment special M4
constructs, in which case `dnl` is right, but keep in mind that it
is unlikely.

M4 ignores the leading blanks and newlines before each argument.
Use this feature to
indent in such a way that arguments are (more or less) aligned with the
opening parenthesis of the macro being called. For instance, instead of

```
AC_CACHE_CHECK(for EMX OS/2 environment,
ac_cv_emxos2,
[AC_COMPILE_IFELSE([AC_LANG_PROGRAM(, [return __EMX__;])],
[ac_cv_emxos2=yes], [ac_cv_emxos2=no])])
```

write

```
AC_CACHE_CHECK([for EMX OS/2 environment], [ac_cv_emxos2],
[AC_COMPILE_IFELSE([AC_LANG_PROGRAM([], [return __EMX__;])],
                   [ac_cv_emxos2=yes],
                   [ac_cv_emxos2=no])])
```

or even

```
AC_CACHE_CHECK([for EMX OS/2 environment],
               [ac_cv_emxos2],
               [AC_COMPILE_IFELSE([AC_LANG_PROGRAM([],
                                                   [return __EMX__;])],
                                  [ac_cv_emxos2=yes],
                                  [ac_cv_emxos2=no])])
```

When using `AC_RUN_IFELSE` or any macro that cannot work when
cross-compiling, provide a pessimistic value (typically ‘`no`’).

Feel free to use various tricks to prevent auxiliary tools, such as
syntax-highlighting editors, from behaving improperly. For instance,
instead of:

```
m4_bpatsubst([$1], [$"])
```

use

```
m4_bpatsubst([$1], [$""])
```

so that Emacsen do not open an endless “string” at the first quote.
For the same reasons, avoid:

```
test $[#] != 0
```

and use:

```
test $[@%:@] != 0
```

Otherwise, the closing bracket would be hidden inside a ‘`#`’-comment,
breaking the bracket-matching highlighting from Emacsen. Note the
preferred style to escape from M4: ‘`$[1]`’, ‘`$[@]`’, etc. Do
not escape when it is unnecessary. Common examples of useless quotation
are ‘`[$]$1`’ (write ‘`$$1`’), ‘`[$]var`’ (use ‘`$var`’),
etc.

When using `sed`, don’t use `-e` except for indenting
purposes. With the `s` and `y` commands, the preferred
separator is ‘`/`’ unless ‘`/`’ itself might appear in the pattern
or replacement, in which case you should use ‘`|`’, or optionally
‘`,`’ if you know the pattern and replacement cannot contain a file
name. If none of these characters will do, choose a printable character
that cannot appear in the pattern or replacement. Characters from the
set ‘`` "#$&'()*;<=>?`|~ ``’ are good choices if the pattern or
replacement might contain a file name, since they have special meaning
to the shell and are less likely to occur in file names.

See [Macro Definitions](#Macro-Definitions), for details on how to define a macro. If a
macro doesn’t use `AC_REQUIRE`, is expected to never be the object
of an `AC_REQUIRE` directive, and macros required by other macros
inside arguments do not need to be expanded before this macro, then
use `m4_define`. In case of doubt, use `AC_DEFUN`.
Also take into account that public third-party macros need to use
`AC_DEFUN` in order to be found by `aclocal`
(see [Extending aclocal](https://www.gnu.org/software/automake/manual/automake.html#Extending-aclocal) in GNU Automake).
All the `AC_REQUIRE` statements should be at the beginning of the
macro, and each statement should be followed by `dnl`.

You should not rely on the number of arguments: instead of checking
whether an argument is missing, test that it is not empty. It provides
both a simpler and a more predictable interface to the user, and saves
room for further arguments.

Unless the macro is short, try to leave the closing ‘`])`’ at the
beginning of a line, followed by a comment that repeats the name of the
macro being defined. This introduces an additional newline in
`configure`; normally, that is not a problem, but if you want to
remove it you can use ‘`[]dnl`’ on the last line. You can similarly
use ‘`[]dnl`’ after a macro call to remove its newline. ‘`[]dnl`’
is recommended instead of ‘`dnl`’ to ensure that M4 does not
interpret the ‘`dnl`’ as being attached to the preceding text or
macro output. For example, instead of:

```
AC_DEFUN([AC_PATH_X],
[AC_MSG_CHECKING([for X])
AC_REQUIRE_CPP()
# ...omitted...
  AC_MSG_RESULT([libraries $x_libraries, headers $x_includes])
fi])
```

you would write:

```
AC_DEFUN([AC_PATH_X],
[AC_REQUIRE_CPP()[]dnl
AC_MSG_CHECKING([for X])
# ...omitted...
  AC_MSG_RESULT([libraries $x_libraries, headers $x_includes])
fi[]dnl
])# AC_PATH_X
```

If the macro is long, try to split it into logical chunks. Typically,
macros that check for a bug in a function and prepare its
`AC_LIBOBJ` replacement should have an auxiliary macro to perform
this setup. Do not hesitate to introduce auxiliary macros to factor
your code.

In order to highlight the recommended coding style, here is a macro
written the old way:

```
dnl Check for EMX on OS/2.
dnl _AC_EMXOS2
AC_DEFUN(_AC_EMXOS2,
[AC_CACHE_CHECK(for EMX OS/2 environment, ac_cv_emxos2,
[AC_COMPILE_IFELSE([AC_LANG_PROGRAM(, return __EMX__;)],
ac_cv_emxos2=yes, ac_cv_emxos2=no)])
test "x$ac_cv_emxos2" = xyes && EMXOS2=yes])
```

and the new way:

```
# _AC_EMXOS2
# ----------
# Check for EMX on OS/2.
m4_define([_AC_EMXOS2],
[AC_CACHE_CHECK([for EMX OS/2 environment], [ac_cv_emxos2],
[AC_COMPILE_IFELSE([AC_LANG_PROGRAM([], [return __EMX__;])],
                   [ac_cv_emxos2=yes],
                   [ac_cv_emxos2=no])])
test "x$ac_cv_emxos2" = xyes && EMXOS2=yes[]dnl
])# _AC_EMXOS2
```

---

Next: [Portable Make Programming](#Portable-Make), Previous: [Writing Autoconf Macros](#Writing-Autoconf-Macros), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 11 Portable Shell Programming [¶](#Portable-Shell-Programming)

When writing your own checks, there are some shell-script programming
techniques you should avoid in order to make your code portable. The
Bourne shell and upward-compatible shells like the Korn shell and Bash
have evolved over the years, and many features added to the original
System7 shell are now supported on all interesting porting targets.
However, the following discussion between Russ Allbery and Robert Lipe
is worth reading:

Russ Allbery:

> The GNU assumption that `/bin/sh` is the one and only shell
> leads to a permanent deadlock. Vendors don’t want to break users’
> existing shell scripts, and there are some corner cases in the Bourne
> shell that are not completely compatible with a POSIX shell. Thus,
> vendors who have taken this route will *never* (OK…“never say
> never”) replace the Bourne shell (as `/bin/sh`) with a
> POSIX shell.

Robert Lipe:

> This is exactly the problem. While most (at least most System V’s) do
> have a Bourne shell that accepts shell functions most vendor
> `/bin/sh` programs are not the POSIX shell.
>
> So while most modern systems do have a shell *somewhere* that meets the
> POSIX standard, the challenge is to find it.

For this reason, part of the job of M4sh (see [Programming in M4sh](#Programming-in-M4sh))
is to find such a shell. But to prevent trouble, if you’re not using
M4sh you should not take advantage of features that were added after Unix
version 7, circa 1977 (see [Systemology](#Systemology)); you should not use aliases,
negated character classes, or even `unset`. `#` comments,
while not in Unix version 7, were retrofitted in the original Bourne
shell and can be assumed to be part of the least common denominator.

On the other hand, if you’re using M4sh you can assume that the shell
has the features that were added in SVR2 (circa 1984), including shell
functions,
`return`, `unset`, and I/O redirection for builtins. For
more information, refer to <https://www.in-ulm.de/~mascheck/bourne/>.
However, some pitfalls have to be avoided for portable use of these
constructs; these will be documented in the rest of this chapter.
See in particular [Shell Functions](#Shell-Functions) and [Limitations of Shell Builtins](#Limitations-of-Builtins).

The set of external programs you should run in a `configure` script
is fairly small. See [Utilities in
Makefiles](https://www.gnu.org/prep/standards/standards.html#Utilities-in-Makefiles) in The GNU Coding Standards, for the list. This
restriction allows users to start out with a fairly small set of
programs and build the rest, avoiding too many interdependencies between
packages.

Some of these external utilities have a portable subset of features; see
[Limitations of Usual Tools](#Limitations-of-Usual-Tools).

There are other sources of documentation about shells. The
specification for the POSIX
[Shell Command Language](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html), though more generous than the restrictive shell
subset described above, is fairly portable nowadays. Also please see
[the Shell FAQs](http://www.faqs.org/faqs/unix-faq/shell/).

* [Systemology](#Systemology)
* [Shellology](#Shellology)
* [Invoking the Shell](#Invoking-the-Shell)
* [Here-Documents](#Here_002dDocuments)
* [File Descriptors](#File-Descriptors)
* [Signal Handling](#Signal-Handling)
* [File System Conventions](#File-System-Conventions)
* [Shell Pattern Matching](#Shell-Pattern-Matching)
* [Shell Substitutions](#Shell-Substitutions)
* [Assignments](#Assignments)
* [Parentheses in Shell Scripts](#Parentheses)
* [Special Shell Variables](#Special-Shell-Variables)
* [Shell Functions](#Shell-Functions)
* [Limitations of Shell Builtins](#Limitations-of-Builtins)
* [Limitations of Usual Tools](#Limitations-of-Usual-Tools)

---

Next: [Shellology](#Shellology), Up: [Portable Shell Programming](#Portable-Shell)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 11.1 Systemology [¶](#Systemology-1)

This section aims at presenting some systems and pointers to
documentation. It may help you addressing particular problems reported
by users.

[POSIX-conforming
systems](https://en.wikipedia.org/wiki/POSIX) are derived from the
[Unix operating system](https://en.wikipedia.org/wiki/Unix).

The [Rosetta Stone for Unix](https://bhami.com/rosetta.html)
contains a table correlating the features of various POSIX-conforming
systems. [Unix History](https://www.levenez.com/unix/) is a
simplified diagram of how many Unix systems were derived from each
other.

[The Heirloom Project](https://heirloom.sourceforge.net/)
provides some variants of traditional implementations of Unix utilities.

Darwin [¶](#index-Darwin)
:   Darwin is a partially proprietary operating system maintained by Apple
    Computer and used by most of their products. It is also known as macOS,
    iOS, etc. depending on the exact variant. Older versions were called
    “Mac OS X.”

    By default the file system will be case insensitive, albeit case
    preserving. This can cause nasty problems: for instance, the
    installation attempt for a package having an `INSTALL` file can
    result in ‘`make install`’ reporting that nothing is to be done!

    Darwin does support case-sensitive file systems, but they must be
    formatted specially as such, and Apple discourages use of a
    case-sensitive volume for the base operating system. To build software
    that expects case-sensitive filenames, it is best to create a separate
    disk volume or disk image formatted as case sensitive; this can be done
    using the `diskutil` command or the Disk Utility application.

QNX [¶](#index-QNX)
:   QNX is a realtime operating system running on Intel architecture
    meant to be scalable from the small embedded systems to the hundred
    processor super-computer. More
    information is available on the
    [QNX home page](https://blackberry.qnx.com/en).

Unix version 7 [¶](#index-Unix-version-7)
:   Officially this was called the “Seventh Edition” of “the UNIX
    time-sharing system” but we use the more-common name “Unix version 7”.
    Documentation is available in the
    [Unix Seventh Edition Manual](https://s3.amazonaws.com/plan9-bell-labs/7thEdMan/index.html).
    Previous versions of Unix are called “Unix version 6”, etc., but
    they were not as widely used.

---

Next: [Invoking the Shell](#Invoking-the-Shell), Previous: [Systemology](#Systemology), Up: [Portable Shell Programming](#Portable-Shell)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 11.2 Shellology [¶](#Shellology-1)

There are several families of shells, most prominently the Bourne family
and the C shell family which are deeply incompatible. If you want to
write portable shell scripts, avoid members of the C shell family. The
[the
Shell difference FAQ](http://www.faqs.org/faqs/unix-faq/shell/shell-differences/) includes a small history of POSIX shells, and a
comparison between several of them.

Below we describe some of the members of the Bourne shell family.

Ash [¶](#index-Ash)
:   Ash is often used on GNU/Linux and BSD
    systems as a light-weight Bourne-compatible shell. Ash 0.2 has some
    bugs that are fixed in the 0.3.x series, but portable shell scripts
    should work around them, since version 0.2 is still shipped with many
    GNU/Linux distributions.

    To be compatible with Ash 0.2:

    * don’t use ‘`$?`’ after expanding empty or unset variables,
      or at the start of an `eval`:

      ```
      foo=
      false
      $foo
      echo "Do not use it: $?"
      false
      eval 'echo "Do not use it: $?"'
      ```
    * don’t use command substitution within variable expansion:

      ```
      cat ${FOO=`bar`}
      ```
    * beware that single builtin substitutions are not performed by a
      subshell, hence their effect applies to the current shell! See [Shell Substitutions](#Shell-Substitutions), item “Command Substitution”.

Bash [¶](#index-Bash)
:   To detect whether you are running Bash, test whether
    `BASH_VERSION` is set. To require
    POSIX compatibility, run ‘`set -o posix`’. See [Bash POSIX Mode](https://www.gnu.org/software/bash/manual/bash.html#Bash-POSIX-Mode) in The GNU Bash Reference
    Manual, for details.

Bash 2.05 and later [¶](#index-Bash-2_002e05-and-later)
:   Versions 2.05 and later of Bash use a different format for the
    output of the `set` builtin, designed to make evaluating its
    output easier. However, this output is not compatible with earlier
    versions of Bash (or with many other shells, probably). So if
    you use Bash 2.05 or higher to execute `configure`,
    you’ll need to use Bash 2.05 for all other build tasks as well.

Ksh [¶](#index-Ksh)
:   The Korn shell is compatible with the Bourne family and it mostly
    conforms to POSIX. It has two major variants commonly
    called ‘`ksh88`’ and ‘`ksh93`’, named after the years of initial
    release. It is usually called `ksh`, but is called `sh`
    on some hosts if you set your path appropriately.

    On Solaris 11, `/bin/sh` and `/usr/bin/ksh` are both
    ‘`ksh93`’. On Solaris 10 and earlier, `/bin/sh` is a
    pre-POSIX Bourne shell and the Korn shell is found elsewhere:
    `/usr/bin/ksh` is ‘`ksh88`’ on Solaris 10,
    `/usr/xpg4/bin/sh` is a POSIX-compliant variant of
    ‘`ksh88`’ on Solaris 10 and later,
    and `/usr/dt/bin/dtksh` is ‘`ksh93`’.
    Variants that are not standard may be parts of optional
    packages. There is no extra charge for these packages, but they are
    not part of a minimal OS install and therefore some installations may
    not have it.

Pdksh [¶](#index-pdksh)
:   A public-domain clone of the Korn shell called `pdksh` is widely
    available: it has most of the ‘`ksh88`’ features along with a few of
    its own. It usually sets `KSH_VERSION`, except if invoked as
    `/bin/sh` on OpenBSD, and similarly to Bash you can require
    POSIX compatibility by running ‘`set -o posix`’. Unfortunately, with
    `pdksh` 5.2.14 (the latest stable version as of 2025)
    POSIX mode is buggy and causes `pdksh` to depart from POSIX in
    at least one respect, see [Shell Substitutions](#Shell-Substitutions).

Zsh [¶](#index-Zsh)
:   To detect whether you are running `zsh`, test whether
    `ZSH_VERSION` is set. By default `zsh` is *not*
    compatible with the Bourne shell: you must execute ‘`emulate sh`’,
    and for `zsh` versions before 3.1.6-dev-18 you must also
    set `NULLCMD` to ‘`:`’. See [Compatibility](zsh.html#Compatibility) in The Z Shell Manual, for details.

    The default Mac OS X `sh` was originally Zsh; it was changed to
    Bash in Mac OS X 10.2 (2002) and changed back to Zsh in macOS 10.15 (2019).

---

Next: [Here-Documents](#Here_002dDocuments), Previous: [Shellology](#Shellology), Up: [Portable Shell Programming](#Portable-Shell)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 11.3 Invoking the Shell [¶](#Invoking-the-Shell-1)

The Korn shell (up to at least version M-12/28/93d) has a bug when
invoked on a file whose name does not contain a slash. It first
searches for the file’s name in `PATH`, and if found it executes
that rather than the original file. For example, assuming there is a
binary executable `/usr/bin/script` in your `PATH`, the last
command in the following example fails because the Korn shell finds
`/usr/bin/script` and refuses to execute it as a shell script:

```
$ touch xxyzzyz script
$ ksh xxyzzyz
$ ksh ./script
$ ksh script
ksh: script: cannot execute
```

Bash 2.03 has a bug when invoked with the `-c` option: if the
option-argument ends in backslash-newline, Bash incorrectly reports a
syntax error. The problem does not occur if a character follows the
backslash:

```
$ $ bash -c 'echo foo \
> '
bash: -c: line 2: syntax error: unexpected end of file
$ bash -c 'echo foo \
>  '
foo
```

See [Backslash-Newline Before Empty Lines](#Backslash_002dNewline_002dEmpty), for how this can cause problems in makefiles.

---

Next: [File Descriptors](#File-Descriptors), Previous: [Invoking the Shell](#Invoking-the-Shell), Up: [Portable Shell Programming](#Portable-Shell)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 11.4 Here-Documents [¶](#Here_002dDocuments-1)

Because unquoted here-documents are subject to parameter expansion and
command substitution, the characters ‘`$`’ and ‘`` ` ``’ are special
in unquoted here-documents and should be escaped by ‘`\`’ if you want
them as-is. Also, ‘`\`’ is special if it precedes ‘`$`’,
‘`` ` ``’, newline or ‘`\`’ itself, so ‘`\`’ should be doubled if
it appears before these characters and you want it as-is.

Using command substitutions in a here-document that is fed to a shell
function is not portable. For example, with Solaris 10 `/bin/sh`:

```
$ kitty () { cat; }
$ kitty <<EOF
> `echo ok`
> EOF
/tmp/sh199886: cannot open
$ echo $?
1
```

Some shells mishandle large here-documents: for example,
Solaris 10 `dtksh` and the UnixWare 7.1.1 POSIX shell, which are
derived from Korn shell version M-12/28/93d, mishandle braced variable
expansion that crosses a 1024- or 4096-byte buffer boundary
within a here-document. Only the part of the variable name after the boundary
is used. For example, `${variable}` could be replaced by the expansion
of `${ble}`. If the end of the variable name is aligned with the block
boundary, the shell reports an error, as if you used `${}`.
Instead of `${variable-default}`, the shell may expand
`${riable-default}`, or even `${fault}`. This bug can often
be worked around by omitting the braces: `$variable`. The bug was
fixed in
‘`ksh93g`’ (1998-04-30) but as of 2006 many operating systems were
still shipping older versions with the bug.

Empty here-documents are not portable either; with the following code,
`zsh` up to at least version 4.3.10 creates a file with a single
newline, whereas other shells create an empty file:

```
cat >file <<EOF
EOF
```

Many shells (including the Bourne shell) implement here-documents
inefficiently. In particular, some shells can be extremely inefficient when
a single statement contains many here-documents. For instance if your
`configure.ac` includes something like:

```
AS_IF([<cross_compiling>],
  [assume this and that],
  [check this
   check that
   check something else
   ...
   on and on forever
   ...])
```

A shell parses the whole `if`/`fi` construct generated by
`AS_IF`, creating
temporary files for each here-document in it. Some shells create links
for such here-documents on every `fork`, so that the clean-up code
they had installed correctly removes them. It is creating the links
that can take the shell forever.

Moving the tests out of the `if`/`fi`, or creating multiple
`if`/`fi` constructs, would improve the performance
significantly. Anyway, this kind of construct is not exactly the
typical use of Autoconf. In fact, it’s even not recommended, because M4
macros can’t look into shell conditionals, so we may fail to expand a
macro when it was expanded before in a conditional path, and the
condition turned out to be false at runtime, and we end up not
executing the macro at all.

Be careful with the use of ‘`<<-`’ to unindent here-documents. The
behavior is only portable for stripping leading `TAB`s, and things
can silently break if an overzealous editor converts to using leading
spaces (not all shells are nice enough to warn about unterminated
here-documents).

```
$ printf 'cat <<-x\n\t1\n\t 2\n\tx\n' | bash && echo done
1
 2
done
$ printf 'cat <<-x\n 1\n  2\n x\n' | bash-3.2 && echo done
 1
  2
 x
done
```

---

Next: [Signal Handling](#Signal-Handling), Previous: [Here-Documents](#Here_002dDocuments), Up: [Portable Shell Programming](#Portable-Shell)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 11.5 File Descriptors [¶](#File-Descriptors-1)

Most shells, if not all (including Bash, Zsh, Ash), output traces on
stderr, even for subshells. This might result in undesirable content
if you meant to capture the standard-error output of the inner command:

```
$ ash -x -c '(eval "echo foo >&2") 2>stderr'
$ cat stderr
+ eval echo foo >&2
+ echo foo
foo
$ bash -x -c '(eval "echo foo >&2") 2>stderr'
$ cat stderr
+ eval 'echo foo >&2'
++ echo foo
foo
$ zsh -x -c '(eval "echo foo >&2") 2>stderr'
