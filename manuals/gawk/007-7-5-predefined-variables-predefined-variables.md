### 7.5 Predefined Variables [¶](#Predefined-Variables)

Most `awk` variables are available to use for your own
purposes; they never change unless your program assigns values to
them, and they never affect anything unless your program examines them.
However, a few variables in `awk` have special built-in meanings.
`awk` examines some of these automatically, so that they enable you
to tell `awk` how to do certain things. Others are set
automatically by `awk`, so that they carry information from the
internal workings of `awk` to your program.

This section documents all of `gawk`’s predefined variables,
most of which are also documented in the chapters describing
their areas of activity.

* [Built-in Variables That Control `awk`](#User_002dmodified)
* [Built-in Variables That Convey Information](#Auto_002dset)
* [Using `ARGC` and `ARGV`](#ARGC-and-ARGV)

---

Next: [Built-in Variables That Convey Information](#Auto_002dset), Up: [Predefined Variables](#Built_002din-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.5.1 Built-in Variables That Control `awk` [¶](#Built_002din-Variables-That-Control-awk)

The following is an alphabetical list of variables that you can change to
control how `awk` does certain things.

The variables that are specific to `gawk` are marked with a pound
sign (‘`#`’). These variables are `gawk` extensions. In other
`awk` implementations or if `gawk` is in compatibility
mode (see [Command-Line Options](#Options)), they are not special. (Any exceptions are noted
in the description of each variable.)

`BINMODE #`
:   On non-POSIX systems, this variable specifies use of binary mode
    for all I/O. Numeric values of one, two, or three specify that input
    files, output files, or all files, respectively, should use binary I/O.
    A numeric value less than zero is treated as zero, and a numeric value
    greater than three is treated as three. Alternatively, string values
    of `"r"` or `"w"` specify that input files and output files,
    respectively, should use binary I/O. A string value of `"rw"` or
    `"wr"` indicates that all files should use binary I/O. Any other
    string value is treated the same as `"rw"`, but causes `gawk`
    to generate a warning message. `BINMODE` is described in more
    detail in [Using `gawk` on PC Operating Systems](#PC-Using). `mawk` (see [Other Freely Available `awk` Implementations](#Other-Versions))
    also supports this variable, but only using numeric values.

`CONVFMT`
:   A string that controls the conversion of numbers to
    strings (see [Conversion of Strings and Numbers](#Conversion)).
    It works by being passed, in effect, as the first argument to the
    `sprintf()` function
    (see [String-Manipulation Functions](#String-Functions)).
    Its default value is `"%.6g"`.
    `CONVFMT` was introduced by the POSIX standard.

`FIELDWIDTHS #`
:   A space-separated list of columns that tells `gawk`
    how to split input with fixed columnar boundaries.
    Starting in version 4.2, each field width may optionally be
    preceded by a colon-separated value specifying the number of characters to skip
    before the field starts.
    Assigning a value to `FIELDWIDTHS`
    overrides the use of `FS` and `FPAT` for field splitting.
    See [Reading Fixed-Width Data](#Constant-Size) for more information.

`FPAT #`
:   A regular expression (as a string) that tells `gawk`
    to create the fields based on text that matches the regular expression.
    Assigning a value to `FPAT`
    overrides the use of `FS` and `FIELDWIDTHS` for field splitting.
    See [Defining Fields by Content](#Splitting-By-Content) for more information.

`FS` [¶](#index-field-separator-1)
:   The input field separator (see [Specifying How Fields Are Separated](#Field-Separators)).
    The value is a single-character string or a multicharacter regular
    expression that matches the separations between fields in an input
    record. If the value is the null string (`""`), then each
    character in the record becomes a separate field.
    (This behavior is a `gawk` extension. POSIX `awk` does not
    specify the behavior when `FS` is the null string.
    Nonetheless, some other versions of `awk` also treat
    `""` specially.)

    The default value is `" "`, a string consisting of a single
    space. As a special exception, this value means that any sequence of
    spaces, TABs, and/or newlines is a single separator. It also causes
    spaces, TABs, and newlines at the beginning and end of a record to
    be ignored.

    You can set the value of `FS` on the command line using the
    `-F` option:

    ```
    awk -F, 'program' input-files
    ```

    If `gawk` is using `FIELDWIDTHS` or `FPAT`
    for field splitting,
    assigning a value to `FS` causes `gawk` to return to
    the normal, `FS`-based field splitting. An easy way to do this
    is to simply say ‘`FS = FS`’, perhaps with an explanatory comment.

`IGNORECASE #`
:   If `IGNORECASE` is nonzero or non-null, then all string comparisons
    and all regular expression matching are case-independent.
    This applies to
    regexp matching with ‘`~`’ and ‘`!~`’,
    the `gensub()`, `gsub()`, `index()`, `match()`,
    `patsplit()`, `split()`, and `sub()` functions,
    record termination with `RS`, and field splitting with
    `FS` and `FPAT`.
    However, the value of `IGNORECASE` does *not* affect array subscripting
    and it does not affect field splitting when using a single-character
    field separator.
    See [Case Sensitivity in Matching](#Case_002dsensitivity).

`LINT #` [¶](#index-lint-checking)
:   When this variable is true (nonzero or non-null), `gawk`
    behaves as if the `--lint` command-line option is in effect
    (see [Command-Line Options](#Options)).
    With a value of `"fatal"`, lint warnings become fatal errors.
    With a value of `"invalid"`, only warnings about things that are
    actually invalid are issued. (This is not fully implemented yet.)
    Any other true value prints nonfatal warnings.
    Assigning a false value to `LINT` turns off the lint warnings.

    This variable is a `gawk` extension. It is not special
    in other `awk` implementations. Unlike with the other special variables,
    changing `LINT` does affect the production of lint warnings,
    even if `gawk` is in compatibility mode. Much as
    the `--lint` and `--traditional` options independently
    control different aspects of `gawk`’s behavior, the control
    of lint warnings during program execution is independent of the flavor
    of `awk` being executed.

`OFMT`
:   A string that controls conversion of numbers to
    strings (see [Conversion of Strings and Numbers](#Conversion)) for
    printing with the `print` statement. It works by being passed
    as the first argument to the `sprintf()` function
    (see [String-Manipulation Functions](#String-Functions)).
    Its default value is `"%.6g"`. Earlier versions of `awk`
    used `OFMT` to specify the format for converting numbers to
    strings in general expressions; this is now done by `CONVFMT`.

`OFS` [¶](#index-field-separator-2)
:   The output field separator (see [Output Separators](#Output-Separators)). It is
    output between the fields printed by a `print` statement. Its
    default value is `" "`, a string consisting of a single space.

`ORS` [¶](#index-ORS-variable-1)
:   The output record separator. It is output at the end of every
    `print` statement. Its default value is `"\n"`, the newline
    character. (See [Output Separators](#Output-Separators).)

`PREC #` [¶](#index-PREC-variable)
:   The working precision of arbitrary-precision floating-point numbers,
    53 bits by default (see [Setting the Precision](#Setting-precision)).

`ROUNDMODE #` [¶](#index-ROUNDMODE-variable)
:   The rounding mode to use for arbitrary-precision arithmetic on
    numbers, by default `"N"` (`roundTiesToEven` in
    the IEEE 754 standard; see [Setting the Rounding Mode](#Setting-the-rounding-mode)).

`RS` [¶](#index-record-separators-1)
:   The input record separator. Its default value is a string
    containing a single newline character, which means that an input record
    consists of a single line of text.
    It can also be the null string, in which case records are separated by
    runs of blank lines.
    If it is a regexp, records are separated by
    matches of the regexp in the input text.
    (See [How Input Is Split into Records](#Records).)

    The ability for `RS` to be a regular expression
    is a `gawk` extension.
    In most other `awk` implementations,
    or if `gawk` is in compatibility mode
    (see [Command-Line Options](#Options)),
    just the first character of `RS`’s value is used.

`SUBSEP` [¶](#index-subscript-separators)
:   The subscript separator. It has the default value of
    `"\034"` and is used to separate the parts of the indices of a
    multidimensional array. Thus, the expression ‘`foo["A", "B"]`’
    really accesses `foo["A\034B"]`
    (see [Multidimensional Arrays](#Multidimensional)).

`TEXTDOMAIN #`
:   Used for internationalization of programs at the
    `awk` level. It sets the default text domain for specially
    marked string constants in the source text, as well as for the
    `dcgettext()`, `dcngettext()`, and `bindtextdomain()` functions
    (see [Internationalization with `gawk`](#Internationalization)).
    The default value of `TEXTDOMAIN` is `"messages"`.

---

Next: [Using `ARGC` and `ARGV`](#ARGC-and-ARGV), Previous: [Built-in Variables That Control `awk`](#User_002dmodified), Up: [Predefined Variables](#Built_002din-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.5.2 Built-in Variables That Convey Information [¶](#Built_002din-Variables-That-Convey-Information)

The following is an alphabetical list of variables that `awk`
sets automatically on certain occasions in order to provide
information to your program.

The variables that are specific to `gawk` are marked with a pound
sign (‘`#`’). These variables are `gawk` extensions. In other
`awk` implementations or if `gawk` is in compatibility
mode (see [Command-Line Options](#Options)), they are not special:

`ARGC`, `ARGV`
:   The command-line arguments available to `awk` programs are stored in
    an array called `ARGV`. `ARGC` is the number of command-line
    arguments present. See [Other Command-Line Arguments](#Other-Arguments).
    Unlike most `awk` arrays,
    `ARGV` is indexed from 0 to `ARGC` − 1.
    In the following example:

    ```
    $ awk 'BEGIN {
    >         for (i = 0; i < ARGC; i++)
    >             print ARGV[i]
    >      }' inventory-shipped mail-list
    -| awk
    -| inventory-shipped
    -| mail-list
    ```

    `ARGV[0]` contains ‘`awk`’, `ARGV[1]`
    contains ‘`inventory-shipped`’, and `ARGV[2]` contains
    ‘`mail-list`’. The value of `ARGC` is three, one more than the
    index of the last element in `ARGV`, because the elements are numbered
    from zero.

    The names `ARGC` and `ARGV`, as well as the convention of indexing
    the array from 0 to `ARGC` − 1, are derived from the C language’s
    method of accessing command-line arguments.

    The value of `ARGV[0]` can vary from system to system.
    Also, you should note that the program text is *not* included in
    `ARGV`, nor are any of `awk`’s command-line options.
    See [Using `ARGC` and `ARGV`](#ARGC-and-ARGV) for information
    about how `awk` uses these variables.
    (d.c.)

`ARGIND #`
:   The index in `ARGV` of the current file being processed.
    Every time `gawk` opens a new data file for processing, it sets
    `ARGIND` to the index in `ARGV` of the file name.
    When `gawk` is processing the input files,
    ‘`FILENAME == ARGV[ARGIND]`’ is always true.

    This variable is useful in file processing; it allows you to tell how far
    along you are in the list of data files as well as to distinguish between
    successive instances of the same file name on the command line.

    While you can change the value of `ARGIND` within your `awk`
    program, `gawk` automatically sets it to a new value when it
    opens the next file.

`ENVIRON`
:   An associative array containing the values of the environment. The array
    indices are the environment variable names; the elements are the values of
    the particular environment variables. For example,
    `ENVIRON["HOME"]` might be `/home/arnold`.

    For POSIX `awk`, changing this array does not affect the
    environment passed on to any programs that `awk` may spawn via
    redirection or the `system()` function.

    However, beginning with version 4.2, if not in POSIX
    compatibility mode, `gawk` does update its own environment when
    `ENVIRON` is changed, thus changing the environment seen by programs
    that it creates. You should therefore be especially careful if you
    modify `ENVIRON["PATH"]`, which is the search path for finding
    executable programs.

    This can also affect the running `gawk` program, since some of the
    built-in functions may pay attention to certain environment variables.
    The most notable instance of this is `mktime()` (see [Time Functions](#Time-Functions)), which pays attention the value of the `TZ` environment
    variable on many systems.

    Some operating systems may not have environment variables.
    On such systems, the `ENVIRON` array is empty (except for
    `ENVIRON["AWKPATH"]` and
    `ENVIRON["AWKLIBPATH"]`;
    see [The `AWKPATH` Environment Variable](#AWKPATH-Variable) and
    see [The `AWKLIBPATH` Environment Variable](#AWKLIBPATH-Variable)).

`ERRNO #`
:   If a system error occurs during a redirection for `getline`, during
    a read for `getline`, or during a `close()` operation, then
    `ERRNO` contains a string describing the error.

    In addition, `gawk` clears `ERRNO` before opening each
    command-line input file. This enables checking if the file is readable
    inside a `BEGINFILE` pattern (see [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE)).

    Otherwise, `ERRNO` works similarly to the C variable `errno`.
    Except for the case just mentioned, `gawk` *never* clears
    it (sets it to zero or `""`). Thus, you should only expect its
    value to be meaningful when an I/O operation returns a failure value,
    such as `getline` returning −1. You are, of course, free
    to clear it yourself before doing an I/O operation.

    If the value of `ERRNO` corresponds to a system error in the C
    `errno` variable, then `PROCINFO["errno"]` will be set to the value
    of `errno`. For non-system errors, `PROCINFO["errno"]` will
    be zero.

`FILENAME`
:   The name of the current input file. When no data files are listed
    on the command line, `awk` reads from the standard input and
    `FILENAME` is set to `"-"`. `FILENAME` changes each
    time a new file is read (see [Reading Input Files](#Reading-Files)). Inside a `BEGIN`
    rule, the value of `FILENAME` is `""`, because there are no input
    files being processed yet.[43](#FOOT43) (d.c.) Note, though,
    that using `getline` (see [Explicit Input with `getline`](#Getline)) inside a `BEGIN` rule
    can give `FILENAME` a value.

`FNR` [¶](#index-FNR-variable-1)
:   The current record number in the current file. `awk` increments
    `FNR` each time it reads a new record (see [How Input Is Split into Records](#Records)).
    `awk` resets `FNR` to zero each time it starts a new
    input file.

`NF` [¶](#index-NF-variable-1)
:   The number of fields in the current input record.
    `NF` is set each time a new record is read, when a new field is
    created, or when `$0` changes (see [Examining Fields](#Fields)).

    Unlike most of the variables described in this subsection,
    assigning a value to `NF` has the potential to affect
    `awk`’s internal workings. In particular, assignments
    to `NF` can be used to create fields in or remove fields from the
    current record. See [Changing the Contents of a Field](#Changing-Fields).

`FUNCTAB #`
:   An array whose indices and corresponding values are the names of all
    the built-in, user-defined, and extension functions in the program.

    > **NOTE:** Attempting to use the `delete` statement with the `FUNCTAB`
    > array causes a fatal error. Any attempt to assign to an element of
    > `FUNCTAB` also causes a fatal error.

`NR` [¶](#index-NR-variable-1)
:   The number of input records `awk` has processed since
    the beginning of the program’s execution
    (see [How Input Is Split into Records](#Records)).
    `awk` increments `NR` each time it reads a new record.

`PROCINFO #`
:   The elements of this array provide access to information about the
    running `awk` program.
    The following elements (listed alphabetically)
    are guaranteed to be available:

    `PROCINFO["argv"]` [¶](#index-command-line-arguments-2)
    :   The `PROCINFO["argv"]` array contains all of the command-line arguments
        (after glob expansion and redirection processing on platforms where that must
        be done manually by the program) with subscripts ranging from 0 through
        `argc` − 1. For example, `PROCINFO["argv"][0]` will contain
        the name by which `gawk` was invoked. Here is an example of how this
        feature may be used:

        ```
        gawk '
        BEGIN {
                for (i = 0; i < length(PROCINFO["argv"]); i++)
                        print i, PROCINFO["argv"][i]
        }'
        ```

        Please note that this differs from the standard `ARGV` array which does
        not include command-line arguments that have already been processed by
        `gawk` (see [Using `ARGC` and `ARGV`](#ARGC-and-ARGV)).

    `PROCINFO["egid"]` [¶](#index-effective-group-ID-of-gawk-user)
    :   The value of the `getegid()` system call.

    `PROCINFO["errno"]`
    :   The value of the C `errno` variable when `ERRNO` is set to
        the associated error message.

    `PROCINFO["euid"]` [¶](#index-effective-user-ID-of-gawk-user)
    :   The value of the `geteuid()` system call.

    `PROCINFO["FS"]`
    :   This is
        `"FS"` if field splitting with `FS` is in effect,
        `"FIELDWIDTHS"` if field splitting with `FIELDWIDTHS` is in effect,
        `"FPAT"` if field matching with `FPAT` is in effect,
        or `"API"` if field splitting is controlled by an API input parser.

    `PROCINFO["gid"]` [¶](#index-group-ID-of-gawk-user)
    :   The value of the `getgid()` system call.

    `PROCINFO["identifiers"]` [¶](#index-program-identifiers)
    :   A subarray, indexed by the names of all identifiers used in the text of
        the `awk` program. An *identifier* is simply the name of a variable
        (be it scalar or array), built-in function, user-defined function, or
        extension function. For each identifier, the value of the element is
        one of the following:

        `"array"`
        :   The identifier is an array.

        `"builtin"`
        :   The identifier is a built-in function.

        `"extension"` [¶](#index-_0040load-directive-3)
        :   The identifier is an extension function loaded via
            `@load` or `-l`.

        `"scalar"`
        :   The identifier is a scalar.

        `"untyped"`
        :   The identifier is untyped (could be used as a scalar or an array;
            `gawk` doesn’t know yet).

        `"user"`
        :   The identifier is a user-defined function.

        The values indicate what `gawk` knows about the identifiers
        after it has finished parsing the program; they are *not* updated
        while the program runs.

    `PROCINFO["platform"]` [¶](#index-platform-running-on)
    :   This element gives a string indicating the platform for which
        `gawk` was compiled. The value will be one of the following:

        `"mingw"`
        :   Microsoft Windows, using MinGW.

        `"os390"`
        :   OS/390 (also known as z/OS).

        `"posix"`
        :   GNU/Linux, Cygwin, macOS, and legacy Unix systems.

        `"vms"`
        :   OpenVMS.

    `PROCINFO["pgrpid"]` [¶](#index-process-group-ID-of-gawk-process)
    :   The process group ID of the current process.

    `PROCINFO["pid"]` [¶](#index-process-ID-of-gawk-process)
    :   The process ID of the current process.

    `PROCINFO["pma"]` [¶](#index-persistent-memory-compiled-into-gawk)
    :   The version of the PMA memory allocator compiled into `gawk`.
        This element will not be present if the PMA allocator is not available
        for use. See [Preserving Data Between Runs](#Persistent-Memory).

    `PROCINFO["ppid"]` [¶](#index-parent-process-ID-of-gawk-process)
    :   The parent process ID of the current process.

    `PROCINFO["strftime"]`
    :   The default time format string for `strftime()`.
        Assigning a new value to this element changes the default.
        See [Time Functions](#Time-Functions).

    `PROCINFO["uid"]`
    :   The value of the `getuid()` system call.

    `PROCINFO["version"]` [¶](#index-version-of-gawk)
    :   The version of `gawk`.

    The following additional elements in the array
    are available to provide information about the MPFR and GMP libraries
    if your version of `gawk` supports arbitrary-precision arithmetic
    (see [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic)):

    `PROCINFO["gmp_version"]` [¶](#index-version-of-GNU-MP-library)
    :   The version of the GNU MP library.

    `PROCINFO["mpfr_version"]`
    :   The version of the GNU MPFR library.

    `PROCINFO["prec_max"]` [¶](#index-maximum-precision-supported-by-MPFR-library)
    :   The maximum precision supported by MPFR.

    `PROCINFO["prec_min"]` [¶](#index-minimum-precision-required-by-MPFR-library)
    :   The minimum precision required by MPFR.

    The following additional elements in the array are available to provide
    information about the version of the extension API, if your version
    of `gawk` supports dynamic loading of extension functions
    (see [Writing Extensions for `gawk`](#Dynamic-Extensions)):

    `PROCINFO["api_major"]` [¶](#index-version-of-gawk-extension-API)
    :   The major version of the extension API.

    `PROCINFO["api_minor"]`
    :   The minor version of the extension API.

    On some systems, there may be elements in the array, `"group1"`
    through `"groupN"` for some N. N is the number of
    supplementary groups that the process has. Use the `in` operator
    to test for these elements
    (see [Referring to an Array Element](#Reference-to-Elements)).

    The following elements allow you to change `gawk`’s behavior:

    `PROCINFO["BUFFERPIPE"]`
    :   If this element exists, all output to pipelines becomes buffered.
        See [Speeding Up Pipe Output](#Noflush).

    `PROCINFO["command", "BUFFERPIPE"]`
    :   Make output to command buffered.
        See [Speeding Up Pipe Output](#Noflush).

    `PROCINFO["NONFATAL"]`
    :   If this element exists, then I/O errors for all redirections become nonfatal.
        See [Enabling Nonfatal Output](#Nonfatal).

    `PROCINFO["name", "NONFATAL"]`
    :   Make I/O errors for name be nonfatal.
        See [Enabling Nonfatal Output](#Nonfatal).

    `PROCINFO["command", "pty"]`
    :   For two-way communication to command, use a pseudo-tty instead
        of setting up a two-way pipe.
        See [Two-Way Communications with Another Process](#Two_002dway-I_002fO) for more information.

    `PROCINFO["input_name", "READ_TIMEOUT"]`
    :   Set a timeout for reading from input redirection input\_name.
        See [Reading Input with a Timeout](#Read-Timeout) for more information.

    `PROCINFO["input_name", "RETRY"]`
    :   If an I/O error that may be retried occurs when reading data from
        input\_name, and this array entry exists, then `getline` returns
        −2 instead of following the default behavior of returning −1
        and configuring input\_name to return no further data. An I/O error
        that may be retried is one where `errno` has the value `EAGAIN`,
        `EWOULDBLOCK`, `EINTR`, or `ETIMEDOUT`. This may be useful
        in conjunction with `PROCINFO["input_name", "READ_TIMEOUT"]`
        or situations where a file descriptor has been configured to behave in
        a non-blocking fashion.
        See [Retrying Reads After Certain Input Errors](#Retrying-Input) for more information.

    `PROCINFO["sorted_in"]`
    :   If this element exists in `PROCINFO`, its value controls the
        order in which array indices will be processed by
        ‘`for (indx in array)`’ loops.
        This is an advanced feature, so we defer the
        full description until later; see
        [Using Predefined Array Scanning Orders with `gawk`](#Controlling-Scanning).

`RLENGTH` [¶](#index-RLENGTH-variable)
:   The length of the substring matched by the
    `match()` function
    (see [String-Manipulation Functions](#String-Functions)).
    `RLENGTH` is set by invoking the `match()` function. Its value
    is the length of the matched string, or −1 if no match is found.

`RSTART` [¶](#index-RSTART-variable)
:   The start index in characters of the substring that is matched by the
    `match()` function
    (see [String-Manipulation Functions](#String-Functions)).
    `RSTART` is set by invoking the `match()` function. Its value
    is the position of the string where the matched substring starts, or zero
    if no match was found.

`RT #`
:   The input text that matched the text denoted by `RS`,
    the record separator. It is set every time a record is read.

`SYMTAB #`
:   An array whose indices are the names of all defined global variables and
    arrays in the program. `SYMTAB` makes `gawk`’s symbol table
    visible to the `awk` programmer. It is built as `gawk`
    parses the program and is complete before the program starts to run.

    The array may be used for indirect access to read or write the value of
    a variable:

    ```
    foo = 5
    SYMTAB["foo"] = 4
    print foo    # prints 4
    ```

    The `isarray()` function (see [Getting Type Information](#Type-Functions)) may be used to test
    if an element in `SYMTAB` is an array.
    Also, you may not use the `delete` statement with the
    `SYMTAB` array.

    Prior to version 5.0 of `gawk`, you could
    use an index for `SYMTAB` that was not a predefined identifier:

    ```
    SYMTAB["xxx"] = 5
    print SYMTAB["xxx"]
    ```

    This no longer works, instead producing a fatal error, as it led
    to rampant confusion.

    The `SYMTAB` array is more interesting than it looks. Andrew Schorr
    points out that it effectively gives `awk` data pointers. Consider his
    example:

    ```
    # Indirect multiply of any variable by amount, return result

    function multiply(variable, amount)
    {
        return SYMTAB[variable] *= amount
    }
    ```

    You would use it like this:

    ```
    BEGIN {
        answer = 10.5
        multiply("answer", 4)
        print "The answer is", answer
    }
    ```

    When run, this produces:

    ```
    $ gawk -f answer.awk
    -| The answer is 42
    ```

    > **NOTE:** In order to avoid severe time-travel paradoxes,[44](#FOOT44) neither `FUNCTAB` nor `SYMTAB`
    > is available as an element within the `SYMTAB` array.

| Changing `NR` and `FNR` |
| --- |
| `awk` increments `NR` and `FNR` each time it reads a record, instead of setting them to the absolute value of the number of records read. This means that a program can change these variables and their new values are incremented for each record. (d.c.) The following example shows this:  ``` $ echo '1 > 2 > 3 > 4' | awk 'NR == 2 { NR = 17 } > { print NR }' -| 1 -| 17 -| 18 -| 19 ```  Before `FNR` was added to the `awk` language (see [Major Changes Between V7 and SVR3.1](#V7_002fSVR3_002e1)), many `awk` programs used this feature to track the number of records in a file by resetting `NR` to zero when `FILENAME` changed. |

---

Previous: [Built-in Variables That Convey Information](#Auto_002dset), Up: [Predefined Variables](#Built_002din-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.5.3 Using `ARGC` and `ARGV` [¶](#Using-ARGC-and-ARGV)

[Built-in Variables That Convey Information](#Auto_002dset)
presented the following program describing the information contained in `ARGC`
and `ARGV`:

```
$ awk 'BEGIN {
>        for (i = 0; i < ARGC; i++)
>            print ARGV[i]
>      }' inventory-shipped mail-list
-| awk
-| inventory-shipped
-| mail-list
```

In this example, `ARGV[0]` contains ‘`awk`’, `ARGV[1]`
contains ‘`inventory-shipped`’, and `ARGV[2]` contains
‘`mail-list`’.
Notice that the `awk` program is not entered in `ARGV`. The
other command-line options, with their arguments, are also not
entered. This includes variable assignments done with the `-v`
option (see [Command-Line Options](#Options)).
Normal variable assignments on the command line *are*
treated as arguments and do show up in the `ARGV` array.
Given the following program in a file named `showargs.awk`:

```
BEGIN {
    printf "A=%d, B=%d\n", A, B
    for (i = 0; i < ARGC; i++)
        printf "\tARGV[%d] = %s\n", i, ARGV[i]
}
END   { printf "A=%d, B=%d\n", A, B }
```

Running it produces the following:

```
$ awk -v A=1 -f showargs.awk B=2 /dev/null
-| A=1, B=0
-|        ARGV[0] = awk
-|        ARGV[1] = B=2
-|        ARGV[2] = /dev/null
-| A=1, B=2
```

A program can alter `ARGC` and the elements of `ARGV`.
Each time `awk` reaches the end of an input file, it uses the next
element of `ARGV` as the name of the next input file. By storing a
different string there, a program can change which files are read.
Use `"-"` to represent the standard input. Storing
additional elements and incrementing `ARGC` causes
additional files to be read.

If the value of `ARGC` is decreased, that eliminates input files
from the end of the list. By recording the old value of `ARGC`
elsewhere, a program can treat the eliminated arguments as
something other than file names.

To eliminate a file from the middle of the list, store the null string
(`""`) into `ARGV` in place of the file’s name. As a
special feature, `awk` ignores file names that have been
replaced with the null string.
Another option is to
use the `delete` statement to remove elements from
`ARGV` (see [The `delete` Statement](#Delete)).

All of these actions are typically done in the `BEGIN` rule,
before actual processing of the input begins.
See [Splitting a Large File into Pieces](#Split-Program) and
see [Duplicating Output into Multiple Files](#Tee-Program)
for examples
of each way of removing elements from `ARGV`.

To actually get options into an `awk` program,
end the `awk` options with `--` and then supply
the `awk` program’s options, in the following manner:

```
awk -f myprog.awk -- -v -q file1 file2 ...
```

The following fragment processes `ARGV` in order to examine, and
then remove, the previously mentioned command-line options:

```
BEGIN {
    for (i = 1; i < ARGC; i++) {
        if (ARGV[i] == "-v")
            verbose = 1
        else if (ARGV[i] == "-q")
            debug = 1
        else if (ARGV[i] ~ /^-./) {
            e = sprintf("%s: unrecognized option -- %c",
                    ARGV[0], substr(ARGV[i], 2, 1))
            print e > "/dev/stderr"
        } else
            break
        delete ARGV[i]
    }
}
```

Ending the `awk` options with `--` isn’t
necessary in `gawk`. Unless `--posix` has
been specified, `gawk` silently puts any unrecognized options
into `ARGV` for the `awk` program to deal with. As soon
as it sees an unknown option, `gawk` stops looking for other
options that it might otherwise recognize. The previous command line with
`gawk` would be:

```
gawk -f myprog.awk -q -v file1 file2 ...
```

Because `-q` is not a valid `gawk` option, it and the
following `-v` are passed on to the `awk` program.
(See [Processing Command-Line Options](#Getopt-Function) for an `awk` library function that
parses command-line options.)

When designing your program, you should choose options that don’t
conflict with `gawk`’s, because it will process any options
that it accepts before passing the rest of the command line on to
your program. Using ‘`#!`’ with the `-E` option may help
(see [Executable `awk` Programs](#Executable-Scripts)
and
see [Command-Line Options](#Options)).

---

Previous: [Predefined Variables](#Built_002din-Variables), Up: [Patterns, Actions, and Variables](#Patterns-and-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.6 Summary [¶](#Summary-6)

* Pattern–action pairs make up the basic elements of an `awk`
  program. Patterns are either normal expressions, range expressions,
  or regexp constants; one of the special keywords `BEGIN`, `END`,
  `BEGINFILE`, or `ENDFILE`; or empty. The action executes if
  the current record matches the pattern. Empty (missing) patterns match
  all records.
* I/O from `BEGIN` and `END` rules has certain constraints.
  This is also true, only more so, for `BEGINFILE` and `ENDFILE`
  rules. The latter two give you “hooks” into `gawk`’s file
  processing, allowing you to recover from a file that otherwise would
  cause a fatal error (such as a file that cannot be opened).
* Shell variables can be used in `awk` programs by careful
  use of shell quoting. It is easier to pass a shell variable into
  `awk` by using the `-v` option and an `awk`
  variable.
* Actions consist of statements enclosed in curly braces. Statements
  are built up from expressions, control statements, compound statements,
  input and output statements, and deletion statements.
* The control statements in `awk` are `if`-`else`,
  `while`, `for`, and `do`-`while`. `gawk`
  adds the `switch` statement. There are two flavors of `for`
  statement: one for performing general looping, and the other for iterating
  through an array.
* `break` and `continue` let you exit early or start the next
  iteration of a loop (or get out of a `switch`).
* `next` and `nextfile` let you read the next record and start
  over at the top of your program or skip to the next input file and
  start over, respectively.
* The `exit` statement terminates your program. When executed
  from an action (or function body), it transfers control to the
  `END` statements. From an `END` statement body, it exits
  immediately. You may pass an optional numeric value to be used
  as `awk`’s exit status.
* Some predefined variables provide control over `awk`, mainly for I/O.
  Other variables convey information from `awk` to your program.
* `ARGC` and `ARGV` make the command-line arguments available
  to your program. Manipulating them from a `BEGIN` rule lets you
  control how `awk` will process the provided data files.

---

Next: [Functions](#Functions), Previous: [Patterns, Actions, and Variables](#Patterns-and-Actions), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 8 Arrays in `awk` [¶](#Arrays-in-awk)

An *array* is a table of values called *elements*. The
elements of an array are distinguished by their *indices*. Indices
may be either numbers or strings.

This chapter describes how arrays work in `awk`,
how to use array elements, how to scan through every element in an array,
and how to remove array elements.
It also describes how `awk` simulates multidimensional
arrays, as well as some of the less obvious points about array usage.
The chapter moves on to discuss `gawk`’s facility
for sorting arrays, and ends with a brief description of `gawk`’s
ability to support true arrays of arrays.

* [The Basics of Arrays](#Array-Basics)
* [Using Numbers to Subscript Arrays](#Numeric-Array-Subscripts)
* [Using Uninitialized Variables as Subscripts](#Uninitialized-Subscripts)
* [The `delete` Statement](#Delete)
* [Multidimensional Arrays](#Multidimensional)
* [Arrays of Arrays](#Arrays-of-Arrays)
* [Summary](#Arrays-Summary)

---

Next: [Using Numbers to Subscript Arrays](#Numeric-Array-Subscripts), Up: [Arrays in `awk`](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.1 The Basics of Arrays [¶](#The-Basics-of-Arrays)

This section presents the basics: working with elements
in arrays one at a time, and traversing all of the elements in
an array.

* [Introduction to Arrays](#Array-Intro)
* [Referring to an Array Element](#Reference-to-Elements)
* [Assigning Array Elements](#Assigning-Elements)
* [Basic Array Example](#Array-Example)
* [Scanning All Elements of an Array](#Scanning-an-Array)
* [Using Predefined Array Scanning Orders with `gawk`](#Controlling-Scanning)

---

Next: [Referring to an Array Element](#Reference-to-Elements), Up: [The Basics of Arrays](#Array-Basics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.1 Introduction to Arrays [¶](#Introduction-to-Arrays)

> *Doing linear scans over an associative array is like trying to club someone
> to death with a loaded Uzi.*

— *Larry Wall*

The `awk` language provides one-dimensional arrays
for storing groups of related strings or numbers.
Every `awk` array must have a name. Array names have the same
syntax as variable names; any valid variable name would also be a valid
array name. But one name cannot be used in both ways (as an array and
as a variable) in the same `awk` program.

Arrays in `awk` superficially resemble arrays in other programming
languages, but there are fundamental differences. In `awk`, it
isn’t necessary to specify the size of an array before starting to use it.
Additionally, any number or string, not just consecutive integers,
may be used as an array index.

In most other languages, arrays must be *declared* before use,
including a specification of
how many elements or components they contain. In such languages, the
declaration causes a contiguous block of memory to be allocated for that
many elements. Usually, an index in the array must be a nonnegative integer.
For example, the index zero specifies the first element in the array, which is
actually stored at the beginning of the block of memory. Index one
specifies the second element, which is stored in memory right after the
first element, and so on. It is impossible to add more elements to the
array, because it has room only for as many elements as given in
the declaration.
(Some languages allow arbitrary starting and ending
indices—e.g., ‘`15 .. 27`’—but the size of the array is still fixed when
the array is declared.)

A contiguous array of four elements might look like
[Figure 8.1](#figure_002darray_002delements),
conceptually, if the element values are eight, `"foo"`,
`""`, and 30.

**Figure 8.1:** A contiguous array

Only the values are stored; the indices are implicit from the order of
the values. Here, eight is the value at index zero, because eight appears in the
position with zero elements before it.

Arrays in `awk` are different—they are *associative*. This means
that each array is a collection of pairs—an index and its corresponding
array element value:

|  | Index | Value |
| --- | --- | --- |
|  | `3` | `30` |
|  | `1` | `"foo"` |
|  | `0` | `8` |
|  | `2` | `""` |

The pairs are shown in jumbled order because their order is
irrelevant.[45](#FOOT45)

One advantage of associative arrays is that new pairs can be added
at any time. For example, suppose a tenth element is added to the array
whose value is `"number ten"`. The result is:

|  | Index | Value |
| --- | --- | --- |
|  | `10` | `"number ten"` |
|  | `3` | `30` |
|  | `1` | `"foo"` |
|  | `0` | `8` |
|  | `2` | `""` |

Now the array is *sparse*, which just means some indices are missing.
It has elements 0–3 and 10, but doesn’t have elements 4, 5, 6, 7, 8, or 9.

Another consequence of associative arrays is that the indices don’t
have to be nonnegative integers. Any number, or even a string, can be
an index. For example, the following is an array that translates words from
English to French:

|  | Index | Value |
| --- | --- | --- |
|  | `"dog"` | `"chien"` |
|  | `"cat"` | `"chat"` |
|  | `"one"` | `"un"` |
|  | `1` | `"un"` |

Here we decided to translate the number one in both spelled-out and
numeric form—thus illustrating that a single array can have both
numbers and strings as indices.
(In fact, array subscripts are always strings.
There are some subtleties to how numbers work when used as
array subscripts; this is discussed in more detail in
[Using Numbers to Subscript Arrays](#Numeric-Array-Subscripts).)
Here, the number `1` isn’t double-quoted, because `awk`
automatically converts it to a string.

The value of `IGNORECASE` has no effect upon array subscripting.
The identical string value used to store an array element must be used
to retrieve it.
When `awk` creates an array (e.g., with the `split()`
built-in function),
that array’s indices are consecutive integers starting at one.
(See [String-Manipulation Functions](#String-Functions).)

`awk`’s arrays are efficient—the time to access an element
is independent of the number of elements in the array.

---

Next: [Assigning Array Elements](#Assigning-Elements), Previous: [Introduction to Arrays](#Array-Intro), Up: [The Basics of Arrays](#Array-Basics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.2 Referring to an Array Element [¶](#Referring-to-an-Array-Element)

The principal way to use an array is to refer to one of its elements.
An *array reference* is an expression as follows:

```
array[index-expression]
```

Here, array is the name of an array. The expression index-expression is
the index of the desired element of the array.

The value of the array reference is the current value of that array
element. For example, `foo[4.3]` is an expression referencing the element
of array `foo` at index ‘`4.3`’.

A reference to an array element that has no recorded value yields a value of
`""`, the null string. This includes elements
that have not been assigned any value as well as elements that have been
deleted (see [The `delete` Statement](#Delete)).

> **NOTE:** A reference to an element that does not exist *automatically* creates
> that array element, with the null string as its value. (In some cases,
> this is unfortunate, because it might waste memory inside `awk`.)
>
> Novice `awk` programmers often make the mistake of checking if
> an element exists by checking if the value is empty:
>
> ```
> # Check if "foo" exists in a:         Incorrect!
> if (a["foo"] != "") ...
> ```
>
> This is incorrect for two reasons. First, it *creates* `a["foo"]`
> if it didn’t exist before! Second, it is valid (if a bit unusual) to set
> an array element equal to the empty string.

To determine whether an element exists in an array at a certain index, use
the following expression:

```
indx in array
```

This expression tests whether the particular index indx exists,
without the side effect of creating that element if it is not present.
The expression has the value one (true) if `array[indx]`
exists and zero (false) if it does not exist.
(We use indx here, because ‘`index`’ is the name of a built-in
function.)
For example, this statement tests whether the array `frequencies`
contains the index ‘`2`’:

```
if (2 in frequencies)
    print "Subscript 2 is present."
```

Note that this is *not* a test of whether the array
`frequencies` contains an element whose *value* is two.
There is no way to do that except to scan all the elements. Also, this
*does not* create `frequencies[2]`, while the following
(incorrect) alternative does:

```
if (frequencies[2] != "")
    print "Subscript 2 is present."
```

---

Next: [Basic Array Example](#Array-Example), Previous: [Referring to an Array Element](#Reference-to-Elements), Up: [The Basics of Arrays](#Array-Basics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.3 Assigning Array Elements [¶](#Assigning-Array-Elements)

Array elements can be assigned values just like
`awk` variables:

```
array[index-expression] = value
```

array is the name of an array. The expression
index-expression is the index of the element of the array that is
assigned a value. The expression value is the value to
assign to that element of the array.

---

Next: [Scanning All Elements of an Array](#Scanning-an-Array), Previous: [Assigning Array Elements](#Assigning-Elements), Up: [The Basics of Arrays](#Array-Basics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.4 Basic Array Example [¶](#Basic-Array-Example)

The following program takes a list of lines, each beginning with a line
number, and prints them out in order of line number. The line numbers
are not in order when they are first read—instead, they
are scrambled. This program sorts the lines by making an array using
the line numbers as subscripts. The program then prints out the lines
in sorted order of their numbers. It is a very simple program and gets
confused upon encountering repeated numbers, gaps, or lines that don’t
begin with a number:

```
{
    if ($1 > max)
        max = $1
    arr[$1] = $0
}

END {
    for (x = 1; x <= max; x++)
        print arr[x]
}
```

The first rule keeps track of the largest line number seen so far;
it also stores each line into the array `arr`, at an index that
is the line’s number.
The second rule runs after all the input has been read, to print out
all the lines.
When this program is run with the following input:

```
5  I am the Five man
2  Who are you?  The new number two!
4  . . . And four on the floor
1  Who is number one?
3  I three you.
```

Its output is:

```
1  Who is number one?
2  Who are you?  The new number two!
3  I three you.
4  . . . And four on the floor
5  I am the Five man
```

If a line number is repeated, the last line with a given number overrides
the others.
Gaps in the line numbers can be handled with an easy improvement to the
program’s `END` rule, as follows:

```
END {
    for (x = 1; x <= max; x++)
        if (x in arr)
            print arr[x]
}
```

As mentioned, the program is simplistic. It can be easily
confused; for example, by using negative or nonalphabetic line numbers.
The point here is merely to demonstrate basic array usage.

---

Next: [Using Predefined Array Scanning Orders with `gawk`](#Controlling-Scanning), Previous: [Basic Array Example](#Array-Example), Up: [The Basics of Arrays](#Array-Basics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.5 Scanning All Elements of an Array [¶](#Scanning-All-Elements-of-an-Array)

In programs that use arrays, it is often necessary to use a loop that
executes once for each element of an array. In other languages, where
arrays are contiguous and indices are limited to nonnegative integers,
this is easy: all the valid indices can be found by counting from
the lowest index up to the highest. This technique won’t do the job
in `awk`, because any number or string can be an array index.
So `awk` has a special kind of `for` statement for scanning
an array:

```
for (var in array)
    body
```

This loop executes body once for each index in array that the
program has previously used, with the variable var set to that index.

The following program uses this form of the `for` statement. The
first rule scans the input records and notes which words appear (at
least once) in the input, by storing a one into the array `used` with
the word as the index. The second rule scans the elements of `used` to
find all the distinct words that appear in the input. It prints each
word that is more than 10 characters long and also prints the number of
such words.
See [String-Manipulation Functions](#String-Functions)
for more information on the built-in function `length()`.

```
# Record a 1 for each word that is used at least once
{
    for (i = 1; i <= NF; i++)
        used[$i] = 1
}
```

```

```

```
# Find number of distinct words more than 10 characters long
END {
    for (x in used) {
        if (length(x) > 10) {
            ++num_long_words
            print x
        }
    }
    print num_long_words, "words longer than 10 characters"
}
```

See [Generating Word-Usage Counts](#Word-Sorting)
for a more detailed example of this type.

The order in which elements of the array are accessed by this statement
is determined by the internal arrangement of the array elements within
`awk` and in standard `awk` cannot be controlled
or changed. This can lead to problems if new elements are added to
array by statements in the loop body; it is not predictable whether
the `for` loop will reach them. Similarly, changing var inside
the loop may produce strange results. It is best to avoid such things.

As a point of information, `gawk` sets up the list of elements
to be iterated over before the loop starts, and does not change it.
But not all `awk` versions do so. Consider this program, named
`loopcheck.awk`:

```
BEGIN {
    a["here"] = "here"
    a["is"] = "is"
    a["a"] = "a"
    a["loop"] = "loop"
    for (i in a) {
        j++
        a[j] = j
        print i
    }
}
```

Here is what happens when run with `gawk` (and `mawk`):

```
$ gawk -f loopcheck.awk
-| here
-| loop
-| a
-| is
```

Contrast this to BWK `awk`:

```
$ nawk -f loopcheck.awk
-| loop
-| here
-| is
-| a
-| 1
```

---

Previous: [Scanning All Elements of an Array](#Scanning-an-Array), Up: [The Basics of Arrays](#Array-Basics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.6 Using Predefined Array Scanning Orders with `gawk` [¶](#Using-Predefined-Array-Scanning-Orders-with-gawk)

This subsection describes a feature that is specific to `gawk`.

By default, when a `for` loop traverses an array, the order
is undefined, meaning that the `awk` implementation
determines the order in which the array is traversed.
This order is usually based on the internal implementation of arrays
and will vary from one version of `awk` to the next.

Often, though, you may wish to do something simple, such as
“traverse the array by comparing the indices in ascending order,”
or “traverse the array by comparing the values in descending order.”
`gawk` provides two mechanisms that give you this control:

* Set `PROCINFO["sorted_in"]` to one of a set of predefined values.
  We describe this now.
* Set `PROCINFO["sorted_in"]` to the name of a user-defined function
  to use for comparison of array elements. This advanced feature
  is described later in [Controlling Array Traversal and Array Sorting](#Array-Sorting).

The following special values for `PROCINFO["sorted_in"]` are available:

`"@unsorted"`
:   Array elements are processed in arbitrary order, which is the default
    `awk` behavior.

`"@ind_str_asc"`
:   Order by indices in ascending order compared as strings; this is the most basic sort.
    (Internally, array indices are always strings, so with ‘`a[2*5] = 1`’
    the index is `"10"` rather than numeric 10.)

`"@ind_num_asc"`
:   Order by indices in ascending order but force them to be treated as numbers in the process.
    Any index with a non-numeric value will end up positioned as if it were zero.

`"@val_type_asc"`
:   Order by element values in ascending order (rather than by indices).
    Ordering is by the type assigned to the element
    (see [Variable Typing and Comparison Expressions](#Typing-and-Comparison)).
    All numeric values come before all string values,
    which in turn come before all subarrays.
    (Subarrays have not been described yet;
    see [Arrays of Arrays](#Arrays-of-Arrays).)

    If you choose to use this feature in traversing `FUNCTAB`
    (see [Built-in Variables That Convey Information](#Auto_002dset)), then the order is built-in functions first
    (see [Built-in Functions](#Built_002din)), then user-defined functions (see [User-Defined Functions](#User_002ddefined))
    next, and finally functions loaded from an extension
    (see [Writing Extensions for `gawk`](#Dynamic-Extensions)).

`"@val_str_asc"`
:   Order by element values in ascending order (rather than by indices). Scalar values are
    compared as strings.
    If the string values are identical,
    the index string values are compared instead.
    When comparing non-scalar values,
    `"@val_type_asc"` sort ordering is used, so subarrays, if present,
    come out last.

`"@val_num_asc"`
:   Order by element values in ascending order (rather than by indices). Scalar values are
    compared as numbers.
    Non-scalar values are compared using `"@val_type_asc"` sort ordering,
    so subarrays, if present, come out last.
    When numeric values are equal, the string values are used to provide
    an ordering: this guarantees consistent results across different
    versions of the C `qsort()` function,[46](#FOOT46) which `gawk` uses internally
    to perform the sorting.
    If the string values are also identical,
    the index string values are compared instead.

`"@ind_str_desc"`
:   Like `"@ind_str_asc"`, but the
    string indices are ordered from high to low.

`"@ind_num_desc"`
:   Like `"@ind_num_asc"`, but the
    numeric indices are ordered from high to low.

`"@val_type_desc"`
:   Like `"@val_type_asc"`, but the
    element values, based on type, are ordered from high to low.
    Subarrays, if present, come out first.

`"@val_str_desc"`
:   Like `"@val_str_asc"`, but the
    element values, treated as strings, are ordered from high to low.
    If the string values are identical,
    the index string values are compared instead.
    When comparing non-scalar values,
    `"@val_type_desc"` sort ordering is used, so subarrays, if present,
    come out first.

`"@val_num_desc"`
:   Like `"@val_num_asc"`, but the
    element values, treated as numbers, are ordered from high to low.
    If the numeric values are equal, the string values are compared instead.
    If they are also identical, the index string values are compared instead.
    Non-scalar values are compared using `"@val_type_desc"` sort ordering,
    so subarrays, if present, come out first.

The array traversal order is determined before the `for` loop
starts to run. Changing `PROCINFO["sorted_in"]` in the loop body
does not affect the loop.
For example:

```
$ gawk '
> BEGIN {
>    a[4] = 4
>    a[3] = 3
>    for (i in a)
>        print i, a[i]
> }'
-| 4 4
-| 3 3
$ gawk '
> BEGIN {
>    PROCINFO["sorted_in"] = "@ind_str_asc"
>    a[4] = 4
>    a[3] = 3
>    for (i in a)
>        print i, a[i]
> }'
-| 3 3
-| 4 4
```

When sorting an array by element values, if a value happens to be
a subarray then it is considered to be greater than any string or
numeric value, regardless of what the subarray itself contains,
and all subarrays are treated as being equal to each other. Their
order relative to each other is determined by their index strings.

Here are some additional things to bear in mind about sorted
array traversal:

* The value of `PROCINFO["sorted_in"]` is global. That is, it affects
  all array traversal `for` loops. If you need to change it within your
  own code, you should see if it’s defined and save and restore the value:

  ```
  ...
  if ("sorted_in" in PROCINFO)
      save_sorted = PROCINFO["sorted_in"]

  PROCINFO["sorted_in"] = "@val_str_desc" # or whatever
  ...
  if (save_sorted)
      PROCINFO["sorted_in"] = save_sorted
  ```
* As already mentioned, the default array traversal order is represented by
  `"@unsorted"`. You can also get the default behavior by assigning
  the null string to `PROCINFO["sorted_in"]` or by just deleting the
  `"sorted_in"` element from the `PROCINFO` array with
  the `delete` statement.
  (The `delete` statement hasn’t been described yet; see [The `delete` Statement](#Delete).)

In addition, `gawk` provides built-in functions for
sorting arrays; see [Sorting Array Values and Indices with `gawk`](#Array-Sorting-Functions).

---

Next: [Using Uninitialized Variables as Subscripts](#Uninitialized-Subscripts), Previous: [The Basics of Arrays](#Array-Basics), Up: [Arrays in `awk`](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.2 Using Numbers to Subscript Arrays [¶](#Using-Numbers-to-Subscript-Arrays)

An important aspect to remember about arrays is that *array subscripts
are always strings*. When a numeric value is used as a subscript,
it is converted to a string value before being used for subscripting
(see [Conversion of Strings and Numbers](#Conversion)).
This means that the value of the predefined variable `CONVFMT` can
affect how your program accesses elements of an array. For example:

```
xyz = 12.153
data[xyz] = 1
CONVFMT = "%2.2f"
if (xyz in data)
    printf "%s is in data\n", xyz
else
    printf "%s is not in data\n", xyz
```

This prints ‘`12.15 is not in data`’. The first statement gives
`xyz` a numeric value. Assigning to
`data[xyz]` subscripts `data` with the string value `"12.153"`
(using the default conversion value of `CONVFMT`, `"%.6g"`).
Thus, the array element `data["12.153"]` is assigned the value one.
The program then changes
the value of `CONVFMT`. The test ‘`(xyz in data)`’ generates a new
string value from `xyz`—this time `"12.15"`—because the value of
`CONVFMT` only allows two significant digits. This test fails,
because `"12.15"` is different from `"12.153"`.

According to the rules for conversions
(see [Conversion of Strings and Numbers](#Conversion)), integer
values always convert to strings as integers, no matter what the
value of `CONVFMT` may happen to be. So the usual case of
the following works:

```
for (i = 1; i <= maxsub; i++)
    do something with array[i]
```

The “integer values always convert to strings as integers” rule
has an additional consequence for array indexing.
Octal and hexadecimal constants
(see [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers))
are converted internally into numbers, and their original form
is forgotten. This means, for example, that `array[17]`,
`array[021]`, and `array[0x11]` all refer to the same element!

As with many things in `awk`, the majority of the time
things work as you would expect them to. But it is useful to have a precise
knowledge of the actual rules, as they can sometimes have a subtle
effect on your programs.

---

Next: [The `delete` Statement](#Delete), Previous: [Using Numbers to Subscript Arrays](#Numeric-Array-Subscripts), Up: [Arrays in `awk`](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.3 Using Uninitialized Variables as Subscripts [¶](#Using-Uninitialized-Variables-as-Subscripts)

Suppose it’s necessary to write a program
to print the input data in reverse order.
A reasonable attempt to do so (with some test
data) might look like this:

```
$ echo 'line 1
> line 2
> line 3' | awk '{ l[lines] = $0; ++lines }
> END {
>     for (i = lines - 1; i >= 0; i--)
>        print l[i]
> }'
-| line 3
-| line 2
```

Unfortunately, the very first line of input data did not appear in the
output!

Upon first glance, we would think that this program should have worked.
The variable `lines`
is uninitialized, and uninitialized variables have the numeric value zero.
So, `awk` should have printed the value of `l[0]`.

The issue here is that subscripts for `awk` arrays are *always*
strings. Uninitialized variables, when used as strings, have the
value `""`, not zero. Thus, ‘`line 1`’ ends up stored in
`l[""]`.
The following version of the program works correctly:

```
{ l[lines++] = $0 }
END {
    for (i = lines - 1; i >= 0; i--)
       print l[i]
}
```

Here, the ‘`++`’ forces `lines` to be numeric, thus making
the “old value” numeric zero. This is then converted to `"0"`
as the array subscript.

Even though it is somewhat unusual, the null string
(`""`) is a valid array subscript.
(d.c.)
`gawk` warns about the use of the null string as a subscript
if `--lint` is provided
on the command line (see [Command-Line Options](#Options)).

---

Next: [Multidimensional Arrays](#Multidimensional), Previous: [Using Uninitialized Variables as Subscripts](#Uninitialized-Subscripts), Up: [Arrays in `awk`](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.4 The `delete` Statement [¶](#The-delete-Statement)

To remove an individual element of an array, use the `delete`
statement:

```
delete array[index-expression]
```

Once an array element has been deleted, any value the element once
had is no longer available. It is as if the element had never
been referred to or been given a value.
The following is an example of deleting elements in an array:

```
for (i in frequencies)
    delete frequencies[i]
```

This example removes all the elements from the array `frequencies`.
Once an element is deleted, a subsequent `for` statement to scan the array
does not report that element and using the `in` operator to check for
the presence of that element returns zero (i.e., false):

```
delete foo[4]
if (4 in foo)
    print "This will never be printed"
```

It is important to note that deleting an element is *not* the
same as assigning it a null value (the empty string, `""`).
For example:

```
foo[4] = ""
if (4 in foo)
  print "This is printed, even though foo[4] is empty"
```

It is not an error to delete an element that does not exist.
However, if `--lint` is provided on the command line
(see [Command-Line Options](#Options)),
`gawk` issues a warning message when an element that
is not in the array is deleted.

All the elements of an array may be deleted with a single statement
by leaving off the subscript in the `delete` statement,
as follows:

```
delete array
```

Using this version of the `delete` statement is about three times
more efficient than the equivalent loop that deletes each element one
at a time.

This form of the `delete` statement is also supported
by BWK `awk` and `mawk`, as well as
by a number of other implementations.

> **NOTE:** For many years, using `delete` without a subscript was a common
> extension. In September 2012, it was accepted for inclusion into the
> POSIX standard. See [the Austin Group website](https://austingroupbugs.net/view.php?id=544).

The following statement provides a portable but nonobvious way to clear
out an array:[47](#FOOT47)

```
split("", array)
```

The `split()` function
(see [String-Manipulation Functions](#String-Functions))
clears out the target array first. This call asks it to split
apart the null string. Because there is no data to split out, the
function simply clears the array and then returns.

> **CAUTION:** Deleting all the elements from an array does not change its type; you cannot
> clear an array and then use the array’s name as a scalar
> (i.e., a regular variable). For example, the following does not work:
>
> ```
> a[1] = 3
> delete a
> a = 3
> ```

---

Next: [Arrays of Arrays](#Arrays-of-Arrays), Previous: [The `delete` Statement](#Delete), Up: [Arrays in `awk`](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.5 Multidimensional Arrays [¶](#Multidimensional-Arrays)

A *multidimensional array* is an array in which an element is identified
by a sequence of indices instead of a single index. For example, a
two-dimensional array requires two indices. The usual way (in many
languages, including `awk`) to refer to an element of a
two-dimensional array named `grid` is with
`grid[x,y]`.

Multidimensional arrays are supported in `awk` through
concatenation of indices into one string.
`awk` converts the indices into strings
(see [Conversion of Strings and Numbers](#Conversion)) and
concatenates them together, with a separator between them. This creates
a single string that describes the values of the separate indices. The
combined string is used as a single index into an ordinary,
one-dimensional array. The separator used is the value of the built-in
variable `SUBSEP`.

For example, suppose we evaluate the expression ‘`foo[5,12] = "value"`’
when the value of `SUBSEP` is `"@"`. The numbers 5 and 12 are
converted to strings and
concatenated with an ‘`@`’ between them, yielding `"5@12"`; thus,
the array element `foo["5@12"]` is set to `"value"`.

Once the element’s value is stored, `awk` has no record of whether
it was stored with a single index or a sequence of indices. The two
expressions ‘`foo[5,12]`’ and ‘`foo[5 SUBSEP 12]`’ are always
equivalent.

The default value of `SUBSEP` is the string `"\034"`,
which contains a nonprinting character that is unlikely to appear in an
`awk` program or in most input data.
The usefulness of choosing an unlikely character comes from the fact
that index values that contain a string matching `SUBSEP` can lead to
combined strings that are ambiguous. Suppose that `SUBSEP` is
`"@"`; then ‘`foo["a@b", "c"]`’ and ‘`foo["a", "b@c"]`’ are indistinguishable because both are actually
stored as ‘`foo["a@b@c"]`’.

To test whether a particular index sequence exists in a
multidimensional array, use the same operator (`in`) that is
used for single-dimensional arrays. Write the whole sequence of indices
in parentheses, separated by commas, as the left operand:

```
if ((subscript1, subscript2, ...) in array)
    ...
```

Here is an example that treats its input as a two-dimensional array of
fields; it rotates this array 90 degrees clockwise and prints the
result. It assumes that all lines have the same number of
elements:

```
{
     if (max_nf < NF)
          max_nf = NF
     max_nr = NR
     for (x = 1; x <= NF; x++)
          vector[x, NR] = $x
}

END {
     for (x = 1; x <= max_nf; x++) {
          for (y = max_nr; y >= 1; --y)
               printf("%s ", vector[x, y])
          printf("\n")
     }
}
```

When given the input:

```
1 2 3 4 5 6
2 3 4 5 6 1
3 4 5 6 1 2
4 5 6 1 2 3
```

the program produces the following output:

```
4 3 2 1
5 4 3 2
6 5 4 3
1 6 5 4
2 1 6 5
3 2 1 6
```

* [Scanning Multidimensional Arrays](#Multiscanning)

---

Up: [Multidimensional Arrays](#Multidimensional)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.5.1 Scanning Multidimensional Arrays [¶](#Scanning-Multidimensional-Arrays)

There is no special `for` statement for scanning a
“multidimensional” array. There cannot be one, because, in truth,
`awk` does not have
multidimensional arrays or elements—there is only a
multidimensional *way of accessing* an array.

However, if your program has an array that is always accessed as
multidimensional, you can get the effect of scanning it by combining
the scanning `for` statement
(see [Scanning All Elements of an Array](#Scanning-an-Array)) with the
built-in `split()` function
(see [String-Manipulation Functions](#String-Functions)).
It works in the following manner:

```
for (combined in array) {
    split(combined, separate, SUBSEP)
    ...
}
```

This sets the variable `combined` to
each concatenated combined index in the array, and splits it
into the individual indices by breaking it apart where the value of
`SUBSEP` appears. The individual indices then become the elements of
the array `separate`.

Thus, if a value is previously stored in `array[1, "foo"]`, then
an element with index `"1\034foo"` exists in `array`. (Recall
that the default value of `SUBSEP` is the character with code 034.)
Sooner or later, the `for` statement finds that index and does an
iteration with the variable `combined` set to `"1\034foo"`.
Then the `split()` function is called as follows:

```
split("1\034foo", separate, "\034")
```

The result is to set `separate[1]` to `"1"` and
`separate[2]` to `"foo"`. Presto! The original sequence of
separate indices is recovered.

---

Next: [Summary](#Arrays-Summary), Previous: [Multidimensional Arrays](#Multidimensional), Up: [Arrays in `awk`](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.6 Arrays of Arrays [¶](#Arrays-of-Arrays-1)

`gawk` goes beyond standard `awk`’s multidimensional
array access and provides true arrays of
arrays. Elements of a subarray are referred to by their own indices
enclosed in square brackets, just like the elements of the main array.
For example, the following creates a two-element subarray at index `1`
of the main array `a`:

```
a[1][1] = 1
a[1][2] = 2
```

This simulates a true two-dimensional array. Each subarray element can
contain another subarray as a value, which in turn can hold other arrays
as well. In this way, you can create arrays of three or more dimensions.
The indices can be any `awk` expressions, including scalars
separated by commas (i.e., a regular `awk` simulated
multidimensional subscript). So the following is valid in
`gawk`:

```
a[1][3][1, "name"] = "barney"
```

Each subarray and the main array can be of different length. In fact, the
elements of an array or its subarray do not all have to have the same
type. This means that the main array and any of its subarrays can be
nonrectangular, or jagged in structure. You can assign a scalar value to
the index `4` of the main array `a`, even though `a[1]`
is itself an array and not a scalar:

```
a[4] = "An element in a jagged array"
```

The terms *dimension*, *row*, and *column* are
meaningless when applied
to such an array, but we will use “dimension” henceforth to imply the
maximum number of indices needed to refer to an existing element. The
type of any element that has already been assigned cannot be changed
by assigning a value of a different type. You have to first delete the
current element, which effectively makes `gawk` forget about
the element at that index:

```
delete a[4]
a[4][5][6][7] = "An element in a four-dimensional array"
```

This removes the scalar value from index `4` and then inserts a
three-level nested subarray
containing a scalar. You can also
delete an entire subarray or subarray of subarrays:

```
delete a[4][5]
a[4][5] = "An element in subarray a[4]"
```

But recall that you can not delete the main array `a` and then use it
as a scalar.

The built-in functions that take array arguments can also be used
with subarrays. For example, the following code fragment uses `length()`
(see [String-Manipulation Functions](#String-Functions))
to determine the number of elements in the main array `a` and
its subarrays:

```
print length(a), length(a[1]), length(a[1][3])
```

This results in the following output for our main array `a`:

```
2, 3, 1
```

The ‘`subscript in array`’ expression
(see [Referring to an Array Element](#Reference-to-Elements)) works similarly for both
regular `awk`-style
arrays and arrays of arrays. For example, the tests ‘`1 in a`’,
‘`3 in a[1]`’, and ‘`(1, "name") in a[1][3]`’ all evaluate to
one (true) for our array `a`.

The ‘`for (item in array)`’ statement (see [Scanning All Elements of an Array](#Scanning-an-Array))
can be nested to scan all the
elements of an array of arrays if it is rectangular in structure. In order
to print the contents (scalar values) of a two-dimensional array of arrays
(i.e., in which each first-level element is itself an
array, not necessarily of the same length),
you could use the following code:

```
for (i in array)
    for (j in array[i])
        print array[i][j]
```

The `isarray()` function (see [Getting Type Information](#Type-Functions))
lets you test if an array element is itself an array:

```
for (i in array) {
    if (isarray(array[i])) {
        for (j in array[i]) {
            print array[i][j]
        }
    }
    else
        print array[i]
}
```

If the structure of a jagged array of arrays is known in advance,
you can often devise workarounds using control statements. For example,
the following code prints the elements of our main array `a`:

```
for (i in a) {
    for (j in a[i]) {
        if (j == 3) {
            for (k in a[i][j])
                print a[i][j][k]
```

```
        } else
            print a[i][j]
    }
}
```

See [Traversing Arrays of Arrays](#Walking-Arrays) for a user-defined function that “walks” an
arbitrarily dimensioned array of arrays.

---

Previous: [Arrays of Arrays](#Arrays-of-Arrays), Up: [Arrays in `awk`](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.7 Summary [¶](#Summary-7)

* Standard `awk` provides one-dimensional associative arrays
  (arrays indexed by string values). All arrays are associative; numeric
  indices are converted automatically to strings.
* Array elements are referenced as `array[indx]`.
  Referencing an element creates it if it did not exist previously.
* The proper way to see if an array has an element with a given index
  is to use the `in` operator: ‘`indx in array`’.
* Use ‘`for (indx in array) …`’ to scan through all the
  individual elements of an array. In the body of the loop, indx takes
  on the value of each element’s index in turn.
* The order in which a ‘`for (indx in array)`’ loop
  traverses an array is undefined in POSIX `awk` and varies among
  implementations. `gawk` lets you control the order by assigning
  special predefined values to `PROCINFO["sorted_in"]`.
* Use ‘`delete array[indx]`’ to delete an individual element.
  To delete all of the elements in an array,
  use ‘`delete array`’.
  This latter feature has been a common extension for many
  years and is now standard, but may not be supported by all commercial
  versions of `awk`.
* Standard `awk` simulates multidimensional arrays by separating
  subscript values with commas. The values are concatenated into a
  single string, separated by the value of `SUBSEP`. The fact
  that such a subscript was created in this way is not retained; thus,
  changing `SUBSEP` may have unexpected consequences. You can use
  ‘`(sub1, sub2, …) in array`’ to see if such
  a multidimensional subscript exists in array.
* `gawk` provides true arrays of arrays. You use a separate
  set of square brackets for each dimension in such an array:
  `data[row][col]`, for example. Array elements may thus be either
  scalar values (number or string) or other arrays.
* Use the `isarray()` built-in function to determine if an array
  element is itself a subarray.

---

Next: [A Library of `awk` Functions](#Library-Functions), Previous: [Arrays in `awk`](#Arrays), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 9 Functions [¶](#Functions-1)

This chapter describes `awk`’s built-in functions,
which fall into three categories: numeric, string, and I/O.
`gawk` provides additional groups of functions
to work with values that represent time, do
bit manipulation, sort arrays,
provide type information, and internationalize and localize programs.

Besides the built-in functions, `awk` has provisions for
writing new functions that the rest of a program can use.
The second half of this chapter describes these
*user-defined* functions.
Finally, we explore indirect function calls, a `gawk`-specific
extension that lets you determine at runtime what function is to
be called.

* [Built-in Functions](#Built_002din)
* [User-Defined Functions](#User_002ddefined)
* [Variable Typing Is Dynamic](#Dynamic-Typing)
* [A Note On Shadowed Variables](#Shadowed-Variables)
* [Indirect Function Calls](#Indirect-Calls)
* [Summary](#Functions-Summary)

---

Next: [User-Defined Functions](#User_002ddefined), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
