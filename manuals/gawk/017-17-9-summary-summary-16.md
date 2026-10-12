### 17.9 Summary [¶](#Summary-16)

* You can write extensions (sometimes called plug-ins) for `gawk`
  in C or C++ using the application programming interface (API) defined
  by the `gawk` developers.
* Extensions must have a license compatible with the GNU General Public
  License (GPL), and they must assert that fact by declaring a variable
  named `plugin_is_GPL_compatible`.
* Communication between `gawk` and an extension is two-way.
  `gawk` passes a `struct` to the extension that contains
  various data fields and function pointers. The extension can then call
  into `gawk` via the supplied function pointers to accomplish
  certain tasks.
* One of these tasks is to “register” the name and implementation of
  new `awk`-level functions with `gawk`. The implementation
  takes the form of a C function pointer with a defined signature.
  By convention, implementation functions are named `do_XXXX()`
  for some `awk`-level function `XXXX()`.
* The API is defined in a header file named `gawkapi.h`. You must include
  a number of standard header files *before* including it in your source file.
* API function pointers are provided for the following kinds of operations:
  + Allocating, reallocating, and releasing memory
  + Registration functions (you may register
    extension functions,
    exit callbacks,
    a version string,
    input parsers,
    output wrappers,
    and two-way processors)
  + Printing fatal, nonfatal, warning, and “lint” warning messages
  + Updating `ERRNO`, or unsetting it
  + Accessing parameters, including converting an undefined parameter into
    an array
  + Symbol table access (retrieving a global variable, creating one,
    or changing one)
  + Creating and releasing cached values; this provides an
    efficient way to use values for multiple variables and
    can be a big performance win
  + Manipulating arrays
    (retrieving, adding, deleting, and modifying elements;
    getting the count of elements in an array;
    creating a new array;
    clearing an array;
    and
    flattening an array for easy C-style looping over all its indices and elements)
* The API defines a number of standard data types for representing
  `awk` values, array elements, and arrays.
* The API provides convenience functions for constructing values.
  It also provides memory management functions to ensure compatibility
  between memory allocated by `gawk` and memory allocated by an
  extension.
* *All* memory passed from `gawk` to an extension must be
  treated as read-only by the extension.
* *All* memory passed from an extension to `gawk` must come from
  the API’s memory allocation functions. `gawk` takes responsibility for
  the memory and releases it when appropriate.
* The API provides information about the running version of `gawk` so
  that an extension can make sure it is compatible with the `gawk`
  that loaded it.
* It is easiest to start a new extension by copying the boilerplate code
  described in this chapter. Macros in the `gawkapi.h` header
  file make this easier to do.
* The `gawk` distribution includes a number of small but useful
  sample extensions. The `gawkextlib` project includes several more
  (larger) extensions. If you wish to write an extension and contribute it
  to the community of `gawk` users, the `gawkextlib` project
  is the place to do so.

---

Previous: [Summary](#Extension-summary), Up: [Writing Extensions for `gawk`](#Dynamic-Extensions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.10 Exercises [¶](#Exercises-4)

1. Add functions to implement system calls such as `chown()`,
   `chmod()`, and `umask()` to the file operations extension
   presented in [C Code for `chdir()` and `stat()`](#Internal-File-Ops).
2. Write an input parser that prints a prompt if the input is
   a from a “terminal” device. You can use the `isatty()`
   function to tell if the input file is a terminal. (Hint: this function
   is usually expensive to call; try to call it just once.)
   The content of the prompt should come from a variable settable
   by `awk`-level code.
   You can write the prompt to standard error. However,
   for best results, open a new file descriptor (or file pointer)
   on `/dev/tty` and print the prompt there, in case standard
   error has been redirected.

   Why is standard error a better
   choice than standard output for writing the prompt?
   Which reading mechanism should you replace, the one to get
   a record, or the one to read raw bytes?
3. Write a wrapper script that provides an interface similar to
   ‘`sed -i`’ for the “inplace” extension presented in
   [Enabling In-Place File Editing](#Extension-Sample-Inplace).

---

Next: [Installing `gawk`](#Installation), Previous: [Writing Extensions for `gawk`](#Dynamic-Extensions), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]# Part IV: Appendices [¶](#Part-IV_003a-Appendices)

---

* [The Evolution of the `awk` Language](#Language-History)
* [Installing `gawk`](#Installation)
* [Implementation Notes](#Notes)
* [Basic Programming Concepts](#Basic-Concepts)
* [Glossary](#Glossary)
* [GNU General Public License](#Copying)
* [GNU Free Documentation License](#GNU-Free-Documentation-License)
* [Index](#Index)## Appendix A The Evolution of the `awk` Language [¶](#The-Evolution-of-the-awk-Language)

This Web page describes the GNU implementation of `awk`,
which follows the POSIX specification. Many longtime `awk`
users learned `awk` programming with the original `awk`
implementation in Version 7 Unix. (This implementation was the basis for
`awk` in Berkeley Unix, through 4.3-Reno. Subsequent versions
of Berkeley Unix, and, for a while, some systems derived from 4.4BSD-Lite, used various
versions of `gawk` for their `awk`.) This chapter
briefly describes the evolution of the `awk` language, with
cross-references to other parts of the Web page where you can
find more information.

* [Major Changes Between V7 and SVR3.1](#V7_002fSVR3_002e1)
* [Changes Between SVR3.1 and SVR4](#SVR4)
* [Changes Between SVR4 and POSIX `awk`](#POSIX)
* [Extensions in Brian Kernighan’s `awk`](#BTL)
* [Extensions in `gawk` Not in POSIX `awk`](#POSIX_002fGNU)
* [History of `gawk` Features](#Feature-History)
* [Common Extensions Summary](#Common-Extensions)
* [Regexp Ranges and Locales: A Long Sad Story](#Ranges-and-Locales)
* [Major Contributors to `gawk`](#Contributors)
* [Summary](#History-summary)

---

Next: [Changes Between SVR3.1 and SVR4](#SVR4), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.1 Major Changes Between V7 and SVR3.1 [¶](#Major-Changes-Between-V7-and-SVR3_002e1)

The `awk` language evolved considerably between the release of
Version 7 Unix (1978) and the new version that was first made generally available in
System V Release 3.1 (1987). This section summarizes the changes, with
cross-references to further details:

* The requirement for ‘`;`’ to separate rules on a line
  (see [`awk` Statements Versus Lines](#Statements_002fLines))
* User-defined functions and the `return` statement
  (see [User-Defined Functions](#User_002ddefined))
* The `delete` statement (see [The `delete` Statement](#Delete))
* The `do`-`while` statement
  (see [The `do`-`while` Statement](#Do-Statement))
* The built-in functions `atan2()`, `cos()`, `sin()`, `rand()`, and
  `srand()` (see [Numeric Functions](#Numeric-Functions))
* The built-in functions `gsub()`, `sub()`, and `match()`
  (see [String-Manipulation Functions](#String-Functions))
* The built-in functions `close()` and `system()`
  (see [Input/Output Functions](#I_002fO-Functions))
* The `ARGC`, `ARGV`, `FNR`, `RLENGTH`, `RSTART`,
  and `SUBSEP` predefined variables (see [Predefined Variables](#Built_002din-Variables))
* Assignable `$0` (see [Changing the Contents of a Field](#Changing-Fields))
* The conditional expression using the ternary operator ‘`?:`’
  (see [Conditional Expressions](#Conditional-Exp))
* The expression ‘`indx in array`’ outside of `for`
  statements (see [Referring to an Array Element](#Reference-to-Elements))
* The exponentiation operator ‘`^`’
  (see [Arithmetic Operators](#Arithmetic-Ops)) and its assignment operator
  form ‘`^=`’ (see [Assignment Expressions](#Assignment-Ops))
* C-compatible operator precedence, which breaks some old `awk`
  programs (see [Operator Precedence (How Operators Nest)](#Precedence))
* Regexps as the value of `FS`
  (see [Specifying How Fields Are Separated](#Field-Separators)) and as the
  third argument to the `split()` function
  (see [String-Manipulation Functions](#String-Functions)), rather than using only the first character
  of `FS`
* Dynamic regexps as operands of the ‘`~`’ and ‘`!~`’ operators
  (see [Using Dynamic Regexps](#Computed-Regexps))
* The escape sequences ‘`\b`’, ‘`\f`’, and ‘`\r`’
  (see [Escape Sequences](#Escape-Sequences))
* Redirection of input for the `getline` function
  (see [Explicit Input with `getline`](#Getline))
* Multiple `BEGIN` and `END` rules
  (see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND))
* Multidimensional arrays
  (see [Multidimensional Arrays](#Multidimensional))

---

Next: [Changes Between SVR4 and POSIX `awk`](#POSIX), Previous: [Major Changes Between V7 and SVR3.1](#V7_002fSVR3_002e1), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.2 Changes Between SVR3.1 and SVR4 [¶](#Changes-Between-SVR3_002e1-and-SVR4)

The System V Release 4 (1989) version of Unix `awk` added these features
(some of which originated in `gawk`):

* The `ENVIRON` array (see [Predefined Variables](#Built_002din-Variables))
* Multiple `-f` options on the command line
  (see [Command-Line Options](#Options))
* The `-v` option for assigning variables before program execution begins
  (see [Command-Line Options](#Options))
* The `--` signal for terminating command-line options
* The ‘`\a`’, ‘`\v`’, and ‘`\x`’ escape sequences
  (see [Escape Sequences](#Escape-Sequences))
* A defined return value for the `srand()` built-in function
  (see [Numeric Functions](#Numeric-Functions))
* The `toupper()` and `tolower()` built-in string functions
  for case translation
  (see [String-Manipulation Functions](#String-Functions))
* A cleaner specification for the ‘`%c`’ format-control letter in the
  `printf` function
  (see [Format-Control Letters](#Control-Letters))
* The ability to dynamically pass the field width and precision (`"%*.*d"`)
  in the argument list of `printf` and `sprintf()`
  (see [Format-Control Letters](#Control-Letters))
* The use of regexp constants, such as `/foo/`, as expressions, where
  they are equivalent to using the matching operator, as in ‘`$0 ~ /foo/`’
  (see [Using Regular Expression Constants](#Using-Constant-Regexps))
* Processing of escape sequences inside command-line variable assignments
  (see [Assigning Variables on the Command Line](#Assignment-Options))

---

Next: [Extensions in Brian Kernighan’s `awk`](#BTL), Previous: [Changes Between SVR3.1 and SVR4](#SVR4), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.3 Changes Between SVR4 and POSIX `awk` [¶](#Changes-Between-SVR4-and-POSIX-awk)

The POSIX Command Language and Utilities standard for `awk` (1992)
introduced the following changes into the language:

* The use of `-W` for implementation-specific options
  (see [Command-Line Options](#Options))
* The use of `CONVFMT` for controlling the conversion of numbers
  to strings (see [Conversion of Strings and Numbers](#Conversion))
* The concept of a numeric string and tighter comparison rules to go
  with it (see [Variable Typing and Comparison Expressions](#Typing-and-Comparison))
* The use of predefined variables as function parameter names is forbidden
  (see [Function Definition Syntax](#Definition-Syntax))
* More complete documentation of many of the previously undocumented
  features of the language

In 2012, a number of extensions that had been commonly available for
many years were finally added to POSIX. They are:

* The `fflush()` built-in function for flushing buffered output
  (see [Input/Output Functions](#I_002fO-Functions))
* The `nextfile` statement
  (see [The `nextfile` Statement](#Nextfile-Statement))
* The ability to delete all of an array at once with ‘`delete array`’
  (see [The `delete` Statement](#Delete))

See [Common Extensions Summary](#Common-Extensions) for a list of common extensions
not permitted by the POSIX standard.

The 2018 POSIX standard can be found online at
<https://pubs.opengroup.org/onlinepubs/9699919799/>.

---

Next: [Extensions in `gawk` Not in POSIX `awk`](#POSIX_002fGNU), Previous: [Changes Between SVR4 and POSIX `awk`](#POSIX), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.4 Extensions in Brian Kernighan’s `awk` [¶](#Extensions-in-Brian-Kernighan_0027s-awk)

Brian Kernighan
has made his version available via his home page
(see [Other Freely Available `awk` Implementations](#Other-Versions)).

This section describes common extensions that
originally appeared in his version of `awk`:

* The ‘`**`’ and ‘`**=`’ operators
  (see [Arithmetic Operators](#Arithmetic-Ops)
  and
  [Assignment Expressions](#Assignment-Ops))
* The use of `func` as an abbreviation for `function`
  (see [Function Definition Syntax](#Definition-Syntax))
* The `fflush()` built-in function for flushing buffered output
  (see [Input/Output Functions](#I_002fO-Functions))

See [Common Extensions Summary](#Common-Extensions) for a full list of the extensions
available in his `awk`.

---

Next: [History of `gawk` Features](#Feature-History), Previous: [Extensions in Brian Kernighan’s `awk`](#BTL), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.5 Extensions in `gawk` Not in POSIX `awk` [¶](#Extensions-in-gawk-Not-in-POSIX-awk)

The GNU implementation, `gawk`, adds a large number of features.
They can all be disabled with either the `--traditional` or
`--posix` options
(see [Command-Line Options](#Options)).

A number of features have come and gone over the years. This section
summarizes the additional features over POSIX `awk` that are
in the current version of `gawk`.

* Additional predefined variables:
  + The
    `ARGIND`,
    `BINMODE`,
    `ERRNO`,
    `FIELDWIDTHS`,
    `FPAT`,
    `IGNORECASE`,
    `LINT`,
    `PROCINFO`,
    `RT`,
    and
    `TEXTDOMAIN`
    variables
    (see [Predefined Variables](#Built_002din-Variables))
* Special files in I/O redirections:
  + The `/dev/stdin`, `/dev/stdout`, `/dev/stderr`, and
    `/dev/fd/N` special file names
    (see [Special File names in `gawk`](#Special-Files))
  + The `/inet`, `/inet4`, and `/inet6` special files for
    TCP/IP networking using ‘`|&`’ to specify which version of the
    IP protocol to use
    (see [Using `gawk` for Network Programming](#TCP_002fIP-Networking))
* Changes and/or additions to the language:
  + The ‘`\x`’ escape sequence
    (see [Escape Sequences](#Escape-Sequences))
  + Full support for both POSIX and GNU regexps
    (see [Regular Expressions](#Regexp))
  + The ability for `FS` and for the third
    argument to `split()` to be null strings
    (see [Making Each Character a Separate Field](#Single-Character-Fields))
  + The ability for `RS` to be a regexp
    (see [How Input Is Split into Records](#Records))
  + The ability to use octal and hexadecimal constants in `awk`
    program source code
    (see [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers))
  + The ‘`|&`’ operator for two-way I/O to a coprocess
    (see [Two-Way Communications with Another Process](#Two_002dway-I_002fO))
  + Indirect function calls
    (see [Indirect Function Calls](#Indirect-Calls))
  + Directories on the command line produce a warning and are skipped
    (see [Directories on the Command Line](#Command_002dline-directories))
  + Output with `print` and `printf` need not be fatal
    (see [Enabling Nonfatal Output](#Nonfatal))
  + Namespaces and the use of `::` to designate an identifier
    within a namespace
    (see [Namespaces in `gawk`](#Namespaces)).
* New keywords:
  + The `BEGINFILE` and `ENDFILE` special patterns
    (see [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE))
  + The `switch` statement
    (see [The `switch` Statement](#Switch-Statement))
* Changes to standard `awk` functions:
  + The optional second argument to `close()` that allows closing one end
    of a two-way pipe to a coprocess
    (see [Two-Way Communications with Another Process](#Two_002dway-I_002fO))
  + POSIX compliance for `gsub()` and `sub()` with `--posix`
  + The `length()` function accepts an array argument
    and returns the number of elements in the array
    (see [String-Manipulation Functions](#String-Functions))
  + The optional third argument to the `match()` function
    for capturing text-matching subexpressions within a regexp
    (see [String-Manipulation Functions](#String-Functions))
  + Positional specifiers in `printf` formats for
    making translations easier
    (see [Rearranging `printf` Arguments](#Printf-Ordering))
  + The `split()` function’s additional optional fourth
    argument, which is an array to hold the text of the field separators
    (see [String-Manipulation Functions](#String-Functions))
* Additional functions only in `gawk`:
  + The `gensub()`, `patsplit()`, and `strtonum()` functions
    for more powerful text manipulation
    (see [String-Manipulation Functions](#String-Functions))
  + The `asort()` and `asorti()` functions for sorting arrays
    (see [Controlling Array Traversal and Array Sorting](#Array-Sorting))
  + The `mktime()`, `systime()`, and `strftime()`
    functions for working with timestamps
    (see [Time Functions](#Time-Functions))
  + The
    `and()`,
    `compl()`,
    `lshift()`,
    `or()`,
    `rshift()`,
    and
    `xor()`
    functions for bit manipulation
    (see [Bit-Manipulation Functions](#Bitwise-Functions))
  + The `isarray()` function to check whether a variable is an array, and
    the more general `typeof()` function
    (see [Getting Type Information](#Type-Functions))
  + The `mkbool()` function to create a Boolean value
    (see [Generating Boolean Values](#Boolean-Functions))
  + The `bindtextdomain()`, `dcgettext()`, and `dcngettext()`
    functions for internationalization
    (see [Internationalizing `awk` Programs](#Programmer-i18n))
* Changes and/or additions in the command-line options:

  + The `AWKPATH` environment variable for specifying a path search for
    the `-f` command-line option
    (see [Command-Line Options](#Options))
  + The `AWKLIBPATH` environment variable for specifying a path search for
    the `-l` command-line option
    (see [Command-Line Options](#Options))
  + The
    `-b`,
    `-c`,
    `-C`,
    `-d`,
    `-D`,
    `-e`,
    `-E`,
    `-g`,
    `-h`,
    `-i`,
    `-l`,
    `-L`,
    `-M`,
    `-n`,
    `-N`,
    `-o`,
    `-O`,
    `-p`,
    `-P`,
    `-r`,
    `-s`,
    `-S`,
    `-t`,
    and
    `-V`
    short options. Also, the
    ability to use GNU-style long-named options that start with `--`,
    and the
    `--assign`,
    `--bignum`,
    `--characters-as-bytes`,
    `--copyright`,
    `--debug`,
    `--dump-variables`,
    `--exec`,
    `--field-separator`,
    `--file`,
    `--gen-pot`,
    `--help`,
    `--include`,
    `--lint`,
    `--lint-old`,
    `--load`,
    `--non-decimal-data`,
    `--optimize`,
    `--no-optimize`,
    `--posix`,
    `--pretty-print`,
    `--profile`,
    `--re-interval`,
    `--sandbox`,
    `--source`,
    `--traditional`,
    `--use-lc-numeric`,
    and
    `--version`
    long options
    (see [Command-Line Options](#Options)).
* Support for the following obsolete systems was removed from the code
  and the documentation for `gawk` version 4.0:
  + Amiga
  + Atari
  + BeOS
  + Cray
  + MIPS RiscOS
  + MS-DOS with the Microsoft Compiler
  + MS-Windows with the Microsoft Compiler
  + NeXT
  + SunOS 3.x, Sun 386 (Road Runner)
  + Tandem (non-POSIX)
  + Prestandard VAX C compiler for VAX/VMS
  + GCC for Alpha has not been tested for a while.
* Support for the following obsolete system was removed from the code
  for `gawk` version 4.1:
  + Ultrix
* Support for the following systems was removed from the code
  for `gawk` version 4.2:
  + MirBSD
  + GNU/Linux on Alpha
* Support for the following systems was removed from the code
  for `gawk` version 5.2:
  + OS/2
  + DJGPP
  + VAX/VMS

---

Next: [Common Extensions Summary](#Common-Extensions), Previous: [Extensions in `gawk` Not in POSIX `awk`](#POSIX_002fGNU), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.6 History of `gawk` Features [¶](#History-of-gawk-Features)

This section describes the features in `gawk`
over and above those in POSIX `awk`,
in the order they were added to `gawk`.

Version 2.10 of `gawk` introduced the following features:

* The `AWKPATH` environment variable for specifying a path search for
  the `-f` command-line option
  (see [Command-Line Options](#Options)).
* The `IGNORECASE` variable and its effects
  (see [Case Sensitivity in Matching](#Case_002dsensitivity)).
* The `/dev/stdin`, `/dev/stdout`, `/dev/stderr` and
  `/dev/fd/N` special file names
  (see [Special File names in `gawk`](#Special-Files)).

Version 2.13 of `gawk` introduced the following features:

* The `FIELDWIDTHS` variable and its effects
  (see [Reading Fixed-Width Data](#Constant-Size)).
* The `systime()` and `strftime()` built-in functions for obtaining
  and printing timestamps
  (see [Time Functions](#Time-Functions)).
* Additional command-line options
  (see [Command-Line Options](#Options)):
  + The `-W lint` option to provide error and portability checking
    for both the source code and at runtime.
  + The `-W compat` option to turn off the GNU extensions.
  + The `-W posix` option for full POSIX compliance.

Version 2.14 of `gawk` introduced the following feature:

* The `next file` statement for skipping to the next data file
  (see [The `nextfile` Statement](#Nextfile-Statement)).

Version 2.15 of `gawk` introduced the following features:

* New variables (see [Predefined Variables](#Built_002din-Variables)):
  + `ARGIND`, which tracks the movement of `FILENAME`
    through `ARGV`.
  + `ERRNO`, which contains the system error message when
    `getline` returns −1 or `close()` fails.
* The `/dev/pid`, `/dev/ppid`, `/dev/pgrpid`, and
  `/dev/user` special file names. These have since been removed.
* The ability to delete all of an array at once with ‘`delete array`’
  (see [The `delete` Statement](#Delete)).
* Command-line option changes
  (see [Command-Line Options](#Options)):
  + The ability to use GNU-style long-named options that start with `--`.
  + The `--source` option for mixing command-line and library-file
    source code.

Version 3.0 of `gawk` introduced the following features:

* New or changed variables:
  + `IGNORECASE` changed, now applying to string comparison as well
    as regexp operations
    (see [Case Sensitivity in Matching](#Case_002dsensitivity)).
  + `RT`, which contains the input text that matched `RS`
    (see [How Input Is Split into Records](#Records)).
* Full support for both POSIX and GNU regexps
  (see [Regular Expressions](#Regexp)).
* The `gensub()` function for more powerful text manipulation
  (see [String-Manipulation Functions](#String-Functions)).
* The `strftime()` function acquired a default time format,
  allowing it to be called with no arguments
  (see [Time Functions](#Time-Functions)).
* The ability for `FS` and for the third
  argument to `split()` to be null strings
  (see [Making Each Character a Separate Field](#Single-Character-Fields)).
* The ability for `RS` to be a regexp
  (see [How Input Is Split into Records](#Records)).
* The `next file` statement became `nextfile`
  (see [The `nextfile` Statement](#Nextfile-Statement)).
* The `fflush()` function from
  BWK `awk`
  (then at Bell Laboratories;
  see [Input/Output Functions](#I_002fO-Functions)).
* New command-line options:
  + The `--lint-old` option to
    warn about constructs that are not available in
    the original Version 7 Unix version of `awk`
    (see [Major Changes Between V7 and SVR3.1](#V7_002fSVR3_002e1)).
  + The `-m` option from BWK `awk`. (Brian was
    still at Bell Laboratories at the time.) This was later removed from
    both his `awk` and from `gawk`.
  + The `--re-interval` option to provide interval expressions in regexps
    (see [Regular Expression Operators](#Regexp-Operators)).
  + The `--traditional` option was added as a better name for
    `--compat` (see [Command-Line Options](#Options)).
* The use of GNU Autoconf to control the configuration process
  (see [Compiling `gawk` for Unix-Like Systems](#Quick-Installation)).
* Amiga support.
  This has since been removed.

Version 3.1 of `gawk` introduced the following features:

* New variables
  (see [Predefined Variables](#Built_002din-Variables)):
  + `BINMODE`, for non-POSIX systems,
    which allows binary I/O for input and/or output files
    (see [Using `gawk` on PC Operating Systems](#PC-Using)).
  + `LINT`, which dynamically controls lint warnings.
  + `PROCINFO`, an array for providing process-related information.
  + `TEXTDOMAIN`, for setting an application’s internationalization text domain
    (see [Internationalization with `gawk`](#Internationalization)).
* The ability to use octal and hexadecimal constants in `awk`
  program source code
  (see [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers)).
* The ‘`|&`’ operator for two-way I/O to a coprocess
  (see [Two-Way Communications with Another Process](#Two_002dway-I_002fO)).
* The `/inet` special files for TCP/IP networking using ‘`|&`’
  (see [Using `gawk` for Network Programming](#TCP_002fIP-Networking)).
* The optional second argument to `close()` that allows closing one end
  of a two-way pipe to a coprocess
  (see [Two-Way Communications with Another Process](#Two_002dway-I_002fO)).
* The optional third argument to the `match()` function
  for capturing text-matching subexpressions within a regexp
  (see [String-Manipulation Functions](#String-Functions)).
* Positional specifiers in `printf` formats for
  making translations easier
  (see [Rearranging `printf` Arguments](#Printf-Ordering)).
* A number of new built-in functions:
  + The `asort()` and `asorti()` functions for sorting arrays
    (see [Controlling Array Traversal and Array Sorting](#Array-Sorting)).
  + The `bindtextdomain()`, `dcgettext()` and `dcngettext()` functions
    for internationalization
    (see [Internationalizing `awk` Programs](#Programmer-i18n)).
  + The `extension()` function and the ability to add
    new built-in functions dynamically. This has seen removed.
    It was replaced by the new extension mechanism.
    See [Writing Extensions for `gawk`](#Dynamic-Extensions).
  + The `mktime()` function for creating timestamps
    (see [Time Functions](#Time-Functions)).
  + The `and()`, `or()`, `xor()`, `compl()`,
    `lshift()`, `rshift()`, and `strtonum()` functions
    (see [Bit-Manipulation Functions](#Bitwise-Functions)).
* The support for ‘`next file`’ as two words was removed completely
  (see [The `nextfile` Statement](#Nextfile-Statement)).
* Additional command-line options
  (see [Command-Line Options](#Options)):
  + The `--dump-variables` option to print a list of all global variables.
  + The `--exec` option, for use in CGI scripts.
  + The `--gen-po` command-line option and the use of a leading
    underscore to mark strings that should be translated
    (see [Extracting Marked Strings](#String-Extraction)).
  + The `--non-decimal-data` option to allow non-decimal
    input data
    (see [Allowing Nondecimal Input Data](#Nondecimal-Data)).
  + The `--profile` option and `pgawk`, the
    profiling version of `gawk`, for producing execution
    profiles of `awk` programs
    (see [Profiling Your `awk` Programs](#Profiling)).
  + The `--use-lc-numeric` option to force `gawk`
    to use the locale’s decimal point for parsing input data
    (see [Conversion of Strings and Numbers](#Conversion)).
* The use of GNU Automake to help in standardizing the configuration process
  (see [Compiling `gawk` for Unix-Like Systems](#Quick-Installation)).
* The use of GNU `gettext` for `gawk`’s own message output
  (see [`gawk` Can Speak Your Language](#Gawk-I18N)).
* BeOS support. This was later removed.
* Tandem support. This was later removed.
* The Atari port became officially unsupported and was
  later removed entirely.
* The source code changed to use ISO C standard-style function definitions.
* POSIX compliance for `sub()` and `gsub()`
  (see [More about ‘`\`’ and ‘`&`’ with `sub()`, `gsub()`, and `gensub()`](#Gory-Details)).
* The `length()` function was extended to accept an array argument
  and return the number of elements in the array
  (see [String-Manipulation Functions](#String-Functions)).
* The `strftime()` function acquired a third argument to
  enable printing times as UTC
  (see [Time Functions](#Time-Functions)).

Version 4.0 of `gawk` introduced the following features:

* Variable additions:
  + `FPAT`, which allows you to specify a regexp that matches
    the fields, instead of matching the field separator
    (see [Defining Fields by Content](#Splitting-By-Content)).
  + If `PROCINFO["sorted_in"]` exists, ‘`for (iggy in foo)`’ loops sort the
    indices before looping over them. The value of this element
    provides control over how the indices are sorted before the loop
    traversal starts
    (see [Using Predefined Array Scanning Orders with `gawk`](#Controlling-Scanning)).
  + `PROCINFO["strftime"]`, which holds
    the default format for `strftime()`
    (see [Time Functions](#Time-Functions)).
* The special files `/dev/pid`, `/dev/ppid`, `/dev/pgrpid`
  and `/dev/user` were removed.
* Support for IPv6 was added via the `/inet6` special file.
  `/inet4` forces IPv4 and `/inet` chooses the system
  default, which is probably IPv4
  (see [Using `gawk` for Network Programming](#TCP_002fIP-Networking)).
* The use of ‘`\s`’ and ‘`\S`’ escape sequences in regular expressions
  (see [`gawk`-Specific Regexp Operators](#GNU-Regexp-Operators)).
* Interval expressions became part of default regular expressions
  (see [Regular Expression Operators](#Regexp-Operators)).
* POSIX character classes work even with `--traditional`
  (see [Regular Expression Operators](#Regexp-Operators)).
* `break` and `continue` became invalid outside a loop,
  even with `--traditional`
  (see [The `break` Statement](#Break-Statement), and also see
  [The `continue` Statement](#Continue-Statement)).
* `fflush()`, `nextfile`, and ‘`delete array`’
  are allowed if `--posix` or `--traditional`, since they
  are all now part of POSIX.
* An optional third argument to
  `asort()` and `asorti()`, specifying how to sort
  (see [String-Manipulation Functions](#String-Functions)).
* The behavior of `fflush()` changed to match BWK `awk`
  and for POSIX; now both ‘`fflush()`’ and ‘`fflush("")`’
  flush all open output redirections
  (see [Input/Output Functions](#I_002fO-Functions)).
* The `isarray()`
  function which distinguishes if an item is an array
  or not, to make it possible to traverse arrays of arrays
  (see [Getting Type Information](#Type-Functions)).
* The `patsplit()`
  function which gives the same capability as `FPAT`, for splitting
  (see [String-Manipulation Functions](#String-Functions)).
* An optional fourth argument to the `split()` function,
  which is an array to hold the values of the separators
  (see [String-Manipulation Functions](#String-Functions)).
* Arrays of arrays
  (see [Arrays of Arrays](#Arrays-of-Arrays)).
* The `BEGINFILE` and `ENDFILE` special patterns
  (see [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE)).
* Indirect function calls
  (see [Indirect Function Calls](#Indirect-Calls)).
* `switch` / `case` are enabled by default
  (see [The `switch` Statement](#Switch-Statement)).
* Command-line option changes
  (see [Command-Line Options](#Options)):
  + The `-b` and `--characters-as-bytes` options
    which prevent `gawk` from treating input as a multibyte string.
  + The redundant `--compat`, `--copyleft`, and `--usage`
    long options were removed.
  + The `--gen-po` option was finally renamed to the correct `--gen-pot`.
  + The `--sandbox` option which disables certain features.
  + All long options acquired corresponding short options, for use in ‘`#!`’ scripts.
* Directories named on the command line now produce a warning, not a fatal
  error, unless `--posix` or `--traditional` are used
  (see [Directories on the Command Line](#Command_002dline-directories)).
* The `gawk` internals were rewritten, bringing the `dgawk`
  debugger and possibly improved performance
  (see [Debugging `awk` Programs](#Debugger)).
* Per the GNU Coding Standards, dynamic extensions must now define
  a global symbol indicating that they are GPL-compatible
  (see [Extension Licensing](#Plugin-License)).
* In POSIX mode, string comparisons use `strcoll()` / `wcscoll()`
  (see [String Comparison Based on Locale Collating Order](#POSIX-String-Comparison)).
* The option for raw sockets was removed, since it was never implemented
  (see [Using `gawk` for Network Programming](#TCP_002fIP-Networking)).
* Ranges of the form ‘`[d-h]`’ are treated as if they were in the
  C locale, no matter what kind of regexp is being used, and even if
  `--posix`
  (see [Regexp Ranges and Locales: A Long Sad Story](#Ranges-and-Locales)).
* Support was removed for the following systems:
  + Atari
  + Amiga
  + BeOS
  + Cray
  + MIPS RiscOS
  + MS-DOS with the Microsoft Compiler
  + MS-Windows with the Microsoft Compiler
  + NeXT
  + SunOS 3.x, Sun 386 (Road Runner)
  + Tandem (non-POSIX)
  + Prestandard VAX C compiler for VAX/VMS

Version 4.1 of `gawk` introduced the following features:

* Three new arrays:
  `SYMTAB`, `FUNCTAB`, and `PROCINFO["identifiers"]`
  (see [Built-in Variables That Convey Information](#Auto_002dset)).
* The three executables `gawk`, `pgawk`, and `dgawk`, were merged into
  one, named just `gawk`. As a result the command-line options changed.
* Command-line option changes
  (see [Command-Line Options](#Options)):
  + The `-D` option invokes the debugger.
  + The `-i` and `--include` options
    load `awk` library files.
  + The `-l` and `--load` options load compiled dynamic extensions.
  + The `-M` and `--bignum` options enable MPFR.
  + The `-o` option only does pretty-printing.
  + The `-p` option is used for profiling.
  + The `-R` option was removed.
* Support for high precision arithmetic with MPFR
  (see [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic)).
* The `and()`, `or()` and `xor()` functions
  changed to allow any number of arguments,
  with a minimum of two
  (see [Bit-Manipulation Functions](#Bitwise-Functions)).
* The dynamic extension interface was completely redone
  (see [Writing Extensions for `gawk`](#Dynamic-Extensions)).
* Redirected `getline` became allowed inside
  `BEGINFILE` and `ENDFILE`
  (see [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE)).
* Support for nonfatal I/O
  (see [Enabling Nonfatal Output](#Nonfatal)).
* The `where` command was added to the debugger
  (see [Working with the Stack](#Execution-Stack)).
* Support for Ultrix was removed.

Version 4.2 of `gawk` introduced the following changes:

* Changes to `ENVIRON` are reflected into `gawk`’s
  environment and that of programs that it runs.
  See [Built-in Variables That Convey Information](#Auto_002dset).
* `FIELDWIDTHS` was enhanced to allow skipping characters
  before assigning a value to a field
  (see [Defining Fields by Content](#Splitting-By-Content)).
* The `PROCINFO["argv"]` array.
  See [Built-in Variables That Convey Information](#Auto_002dset).
* The maximum number of hexadecimal digits in ‘`\x`’ escapes
  is now two.
  See [Escape Sequences](#Escape-Sequences).
* Strongly typed regexp constants of the form ‘`@/…/`’
  (see [Strongly Typed Regexp Constants](#Strong-Regexp-Constants)).
* The bitwise functions changed, making negative arguments into
  a fatal error (see [Bit-Manipulation Functions](#Bitwise-Functions)).
* The `mktime()` function now accepts an optional
  second argument
  (see [Time Functions](#Time-Functions)).
* The `typeof()` function (see [Getting Type Information](#Type-Functions)).
* Optimizations are enabled by default. Use `-s` /
  `--no-optimize` to disable optimizations.
* For many years, POSIX specified that default field splitting
  only allowed spaces and tabs to separate fields, and this was
  how `gawk` behaved with `--posix`. As of 2013,
  the standard restored historical behavior, and now default
  field splitting with `--posix` also allows newlines to
  separate fields.
* Nonfatal output with `print` and `printf`.
  See [Enabling Nonfatal Output](#Nonfatal).
* Retryable I/O via `PROCINFO[input-file, "RETRY"]`;
  (see [Retrying Reads After Certain Input Errors](#Retrying-Input)).
* Changes to the pretty-printer (see [Profiling Your `awk` Programs](#Profiling)):
  + The `--pretty-print` option no longer runs the `awk`
    program too.
  + Comments in the source program are preserved and placed into the
    output file.
  + Explicit parentheses for expressions
    in the input are preserved in the generated output.
* Improvements to the extension API
  (see [Writing Extensions for `gawk`](#Dynamic-Extensions)):
  + The `get_file()` function to access open redirections.
  + The `nonfatal()` function for generating nonfatal error messages.
  + Support for GMP and MPFR values.
  + Input parsers can now override the default field parsing mechanism
    by specifying explicit locations.
* Shell startup files are supplied with the distribution and
  installed by ‘`make install`’ (see [Shell Startup Files](#Shell-Startup-Files)).
* The `igawk` program and its manual page are no longer
  installed when `gawk` is built.
  See [An Easy Way to Use Library Functions](#Igawk-Program).
* Support for MirBSD was removed.
* Support for GNU/Linux on Alpha was removed.

Version 5.0 added the following features:

* The `PROCINFO["platform"]` array element, which allows you
  to write code that takes the operating system / platform into account.

Version 5.1 was created to release `gawk` with a correct
major version number for the API. This was overlooked for version 5.0,
unfortunately. It added the following features:

* The index for this manual was completely reworked.
* Support was added for MSYS2.
* `asort()` and `asorti()` were changed to
  allow `FUNCTAB` and `SYMTAB` as the first argument if a
  second destination array is supplied (see [String-Manipulation Functions](#String-Functions)).
* The `-I`/`--trace` options were added to
  print a trace of the byte codes as they execute (see [Command-Line Options](#Options)).
* `$0` and the fields are now cleared before starting a
  `BEGINFILE` rule (see [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE)).
* Several example programs in the manual were updated to their modern
  POSIX equivalents.
* The “no effect” lint warnings from `--lint` were fixed up
  and now behave more sanely (see [Command-Line Options](#Options)).
* Handling of Infinity and NaN values were improved.
  See [Other Stuff to Know](#Math-Definitions), and also see
  [Standards Versus Existing Practice](#POSIX-Floating-Point-Problems).

Version 5.2 added the following features:

* The `mkbool()` built-in function
  (see [Generating Boolean Values](#Boolean-Functions)).
* Interval expressions in regular expressions are
  enabled by default (see [Some Notes On Interval Expressions](#Interval-Expressions)).
* Support for the FNV1-A hash algorithm for its hash function
  (see [Other Environment Variables](#Other-Environment-Variables)).
* The `gawkbug` script for reporting bugs
  (see [Submitting Bug Reports](#Bug-address)).
* Terence Kelly’s persistent memory allocator (PMA) was
  added, allowing the use of persistent data on certain systems
  (see [Preserving Data Between Runs](#Persistent-Memory)).
* `PROCINFO["pma"]` exists if the PMA allocator is compiled
  in (see [Built-in Variables That Convey Information](#Auto_002dset)).

Version 5.3 added the following features:

* Comma separated value (CSV) field splitting and the `--csv`
  command-line option
  (see [Working With Comma Separated Value Files](#Comma-Separated-Fields)).
* `PROCINFO["CSV"]` exists if `gawk` was invoked
  with `--csv` (see [Built-in Variables That Convey Information](#Auto_002dset)).
* The `do_csv` API information variable
  (see [Informational Variables](#Extension-API-Informational-Variables)).
* The ability to make `gawk` buffer output to pipes
  (see [Speeding Up Pipe Output](#Noflush)).
* The ‘`\u`’ escape sequence
  (see [Escape Sequences](#Escape-Sequences)).
* Support for OpenVMS 9.2-2 x86\_64 (at version 5.3.1).
* The need for GNU `libsigsegv` was removed from `gawk`.
  The value-add was never very much and it caused problems in some
  environments.
* Support for OSF/1 was removed.

Version 5.4 added the following features:

* The `--enable-O3` option to `configure` was added
  (see [Additional Configuration Options](#Additional-Configuration-Options)).
* Use of the new MinRX regular expression matcher as the default
  matching engine
  (see [Selecting the Regexp Matching Engine](#Choosing-Matchers)).
  This brings full POSIX compliance to `gawk`’s
  regular expression matching.
* The `@nsinclude` directive for including files without
  resetting the namespace to ‘`awk`’
  (see [Including A File Without Changing The Namespace](#Inclusion-For-Namespaces)).
* The ability to call `length()` on an array is no longer
  an extension, as it became part of POSIX in 2004
  (see [String-Manipulation Functions](#String-Functions)).
* Persistent memory was enabled on FreeBSD, MidnightBSD, NetBSD and OpenBSD
  (see [Preserving Data Between Runs](#Persistent-Memory)).
* Hexadecimal floating-point values may be used in program source
  code, with the `strtonum()`
  function, and with the `--non-decimal-data` option
  (see [Allowing Nondecimal Input Data](#Nondecimal-Data)).
* Support for UDP in `gawk`’s networking was marked as obsolete
  (see [Using `gawk` for Network Programming](#TCP_002fIP-Networking)).
* The MinGW port of `gawk` for MS-Windows gained support for
  UTF-8 encoding of non-ASCII text when codepage 65001 is used by the
  Windows terminal (see [Using `gawk` on PC Operating Systems](#PC-Using)).
* Support was added for OpenVMS 9 on x86\_64 hardware, and
  support for OpenVMS 8 on Alpha and Itanium was updated.

---

Next: [Regexp Ranges and Locales: A Long Sad Story](#Ranges-and-Locales), Previous: [History of `gawk` Features](#Feature-History), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.7 Common Extensions Summary [¶](#Common-Extensions-Summary)

The following table summarizes the common extensions supported
by `gawk`, Brian Kernighan’s `awk`, and `mawk`,
the three most widely used freely available versions of `awk`
(see [Other Freely Available `awk` Implementations](#Other-Versions)).

| Feature | BWK `awk` | `mawk` | `gawk` | Now standard |
| --- | --- | --- | --- | --- |
| `**` and `**=` operators | X |  | X |  |
| ‘`\x`’ escape sequence | X | X | X |  |
| ‘`\u`’ escape sequence | X |  | X |  |
| `/dev/stdin` special file | X | X | X |  |
| `/dev/stdout` special file | X | X | X |  |
| `/dev/stderr` special file | X | X | X |  |
| `BINMODE` variable |  | X | X |  |
| CSV support | X |  | X |  |
| `FS` as null string | X | X | X |  |
| `delete` without subscript | X | X | X | X |
| `fflush()` function | X | X | X | X |
| `func` keyword | X |  | X |  |
| `nextfile` statement | X | X | X | X |
| `RS` as regexp | X | X | X |  |
| Time-related functions |  | X | X |  |

---

Next: [Major Contributors to `gawk`](#Contributors), Previous: [Common Extensions Summary](#Common-Extensions), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.8 Regexp Ranges and Locales: A Long Sad Story [¶](#Regexp-Ranges-and-Locales_003a-A-Long-Sad-Story)

This section describes the confusing history of ranges within
regular expressions and their interactions with locales, and how this
affected different versions of `gawk`.

The original Unix tools that worked with regular expressions defined
character ranges (such as ‘`[a-z]`’) to match any character between
the first character in the range and the last character in the range,
inclusive. Ordering was based on the numeric value of each character
in the machine’s native character set. Thus, on ASCII-based systems,
‘`[a-z]`’ matched all the lowercase letters, and only the lowercase
letters, as the numeric values for the letters from ‘`a`’ through
‘`z`’ were contiguous. (On an EBCDIC system, the range ‘`[a-z]`’
includes additional nonalphabetic characters as well.)

Almost all introductory Unix literature explained range expressions
as working in this fashion, and in particular, would teach that the
“correct” way to match lowercase letters was with ‘`[a-z]`’, and
that ‘`[A-Z]`’ was the “correct” way to match uppercase letters.
And indeed, this was true.[118](#FOOT118)

The 1992 POSIX standard introduced the idea of locales (see [Where You Are Makes a Difference](#Locales)).
Because many locales include other letters besides the plain 26
letters of the English alphabet, the POSIX standard added
character classes (see [Using Bracket Expressions](#Bracket-Expressions)) as a way to match
different kinds of characters besides the traditional ones in the ASCII
character set.

However, the standard *changed* the interpretation of range expressions.
In the `"C"` and `"POSIX"` locales, a range expression like
‘`[a-dx-z]`’ is still equivalent to ‘`[abcdxyz]`’, as in ASCII.
But outside those locales, the ordering was defined to be based on
*collation order*.

What does that mean?
In many locales, ‘`A`’ and ‘`a`’ are both less than ‘`B`’.
In other words, these locales sort characters in dictionary order,
and ‘`[a-dx-z]`’ is typically not equivalent to ‘`[abcdxyz]`’;
instead, it might be equivalent to ‘`[ABCXYabcdxyz]`’, for example.

This point needs to be emphasized: much literature teaches that you should
use ‘`[a-z]`’ to match a lowercase character. But on systems with
non-ASCII locales, this also matches all of the uppercase characters
except ‘`A`’ or ‘`Z`’! This was a continuous cause of confusion, even well
into the twenty-first century.

To demonstrate these issues, the following example uses the `sub()`
function, which does text replacement (see [String-Manipulation Functions](#String-Functions)). Here,
the intent is to remove trailing uppercase characters:

```
$ echo something1234abc | gawk-3.1.8 '{ sub("[A-Z]*$", ""); print }'
-| something1234a
```

This output is unexpected, as the ‘`bc`’ at the end of
‘`something1234abc`’ should not normally match ‘`[A-Z]*`’.
This result is due to the locale setting (and thus you may not see
it on your system).

Similar considerations apply to other ranges. For example, ‘`["-/]`’
is perfectly valid in ASCII, but is not valid in many Unicode locales,
such as `en_US.UTF-8`.

Early versions of `gawk` used regexp matching code that was not
locale-aware, so ranges had their traditional interpretation.

When `gawk` switched to using locale-aware regexp matchers,
the problems began; especially as both GNU/Linux and commercial Unix
vendors started implementing non-ASCII locales, *and making them
the default*. Perhaps the most frequently asked question became something
like, “Why does ‘`[A-Z]`’ match lowercase letters?!?”

This situation existed for close to 10 years, if not more, and
the `gawk` maintainer grew weary of trying to explain that
`gawk` was being nicely standards-compliant, and that the issue
was in the user’s locale. During the development of version 4.0,
he modified `gawk` to always treat ranges in the original,
pre-POSIX fashion, unless `--posix` was used (see [Command-Line Options](#Options)).[119](#FOOT119)

Fortunately, shortly before the final release of `gawk` 4.0,
the maintainer learned that the 2008 standard had changed the
definition of ranges, such that outside the `"C"` and `"POSIX"`
locales, the meaning of range expressions was *undefined*.[120](#FOOT120)

By using this lovely technical term, the standard gives license
to implementers to implement ranges in whatever way they choose.
The `gawk` maintainer chose to apply the pre-POSIX meaning
both with the default regexp matching and when `--traditional` or
`--posix` are used.
In all cases `gawk` remains POSIX-compliant.

---

Next: [Summary](#History-summary), Previous: [Regexp Ranges and Locales: A Long Sad Story](#Ranges-and-Locales), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.9 Major Contributors to `gawk` [¶](#Major-Contributors-to-gawk)

> *Always give credit where credit is due.*

— *Anonymous*

This section names the major contributors to `gawk`
and/or this Web page, in approximate chronological order:

* Dr. Alfred V. Aho,
  Dr. Peter J. Weinberger, and
  Dr. Brian W. Kernighan, all of Bell Laboratories,
  designed and implemented Unix `awk`,
  from which `gawk` gets the majority of its feature set.
* Paul Rubin
  did the initial design and implementation in 1986, and wrote
  the first draft (around 40 pages) of this Web page.
* Jay Fenlason
  finished the initial implementation.
* Diane Close
  revised the first draft of this Web page, bringing it
  to around 90 pages.
* Richard Stallman
  helped finish the implementation and the initial draft of this
  Web page.
  He is also the founder of the FSF and the GNU Project.
* John Woods
  contributed parts of the code (mostly fixes) in
  the initial version of `gawk`.
* In 1988,
  David Trueman
  took over primary maintenance of `gawk`,
  making it compatible with “new” `awk`, and
  greatly improving its performance.
* Conrad Kwok,
  Scott Garfinkle,
  and
  Kent Williams
  did the initial ports to MS-DOS with various versions of MSC.
* Pat Rankin
  provided the VMS port and its documentation.
* Hal Peterson
  provided help in porting `gawk` to Cray systems.
  (This is no longer supported.)
* Kai Uwe Rommel
  provided the initial port to OS/2 and its documentation.
* Michal Jaegermann
  provided the port to Atari systems and its documentation.
  (This port is no longer supported.)
  He continues to provide portability checking,
  and has done a lot of work to make sure `gawk`
  works on non-32-bit systems.
* Fred Fish
  provided the port to Amiga systems and its documentation.
  (With Fred’s sad passing, this is no longer supported.)
* Scott Deifik
  formerly maintained the MS-DOS port using DJGPP.
* Eli Zaretskii
  currently maintains the MS-Windows port using MinGW.
* Juan Grigera
  provided a port to Windows32 systems.
  (This is no longer supported.)
* For many years,
  Dr. Darrel Hankerson
  acted as coordinator for the various ports to different PC platforms
  and created binary distributions for various PC operating systems.
  He was also instrumental in keeping the documentation up to date for
  the various PC platforms.
* Christos Zoulas
  provided the `extension()`
  built-in function for dynamically adding new functions.
  (This was obsoleted at `gawk` 4.1.)
* Jürgen Kahrs
  contributed the initial version of the TCP/IP networking
  code and documentation, and motivated the inclusion of the ‘`|&`’ operator.
* Stephen Davies
  provided the initial port to Tandem systems and its documentation.
  (However, this is no longer supported.)
  He was also instrumental in the initial work to integrate the
  byte-code internals into the `gawk` code base.
  Additionally, he did most of the work enabling the pretty-printer
  to preserve and output comments.
* Matthew Woehlke
  provided improvements for Tandem’s POSIX-compliant systems.
* Martin Brown
  provided the port to BeOS and its documentation.
  (This is no longer supported.)
* Arno Peters
  did the initial work to convert `gawk` to use
  GNU Automake and GNU `gettext`.
* Alan J. Broder
  provided the initial version of the `asort()` function
  as well as the code for the optional third argument to the
  `match()` function.
* Andreas Buening
  updated the `gawk` port for OS/2.
* Isamu Hasegawa,
  of IBM in Japan, contributed support for multibyte characters.
* Michael Benzinger contributed the initial code for `switch` statements.
* Patrick T.J. McPhee contributed the code for dynamic loading in Windows32
  environments.
  (This is no longer supported.)
* Anders Wallin helped keep the VMS port going for several years.
* Assaf Gordon contributed the initial code to implement the
  `--sandbox` option.
* John Haque made the following contributions:
  + The modifications to convert `gawk`
    into a byte-code interpreter, including the debugger
  + The addition of true arrays of arrays
  + The additional modifications for support of arbitrary-precision arithmetic
  + The initial text of
    [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic)
  + The work to merge the three versions of `gawk`
    into one, for the 4.1 release
  + Improved array internals for arrays indexed by integers
  + The improved array sorting features were also driven by John, together
    with Pat Rankin
* Panos Papadopoulos contributed the original text for [Including Other Files into Your Program](#Include-Files).
* Efraim Yawitz contributed the original text for [Debugging `awk` Programs](#Debugger).
* John Naman contributed the original text for [A Note On Shadowed Variables](#Shadowed-Variables).
* Stuart Ferguson reworked the text in [Defining Fields by Content](#Splitting-By-Content).
* The development of the extension API first released with
  `gawk` 4.1 was driven primarily by
  Arnold Robbins and Andrew Schorr, with notable contributions from
  the rest of the development team.
* John Malmberg contributed significant improvements to the
  OpenVMS port and the related documentation.
* Antonio Giovanni Colombo rewrote a number of examples in the early
  chapters that were severely dated, for which I am incredibly grateful.
  He also provided and maintains the Italian translation.
* Marco Curreli, together with Antonio Colombo, translated this
  Web page into Italian. It is included in the `gawk`
  distribution.
* Juan Manuel Guerrero took over maintenance of the DJGPP port.
* “Jannick” provided support for MSYS2.
* Arnold Robbins
  has been working on `gawk` since 1988, at first
  helping David Trueman, and as the primary maintainer since around 1994.

---

Previous: [Major Contributors to `gawk`](#Contributors), Up: [The Evolution of the `awk` Language](#Language-History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A.10 Summary [¶](#Summary-17)

* The `awk` language has evolved over time. The first release
  was with V7 Unix, circa 1978. In 1987, for System V Release 3.1,
  major additions, including user-defined functions, were made to the language.
  Additional changes were made for System V Release 4, in 1989.
  Since then, further minor changes have happened under the auspices of the
  POSIX standard.
* Brian Kernighan’s `awk` provides a small number of extensions
  that are implemented in common with other versions of `awk`.
* `gawk` provides a large number of extensions over POSIX `awk`.
  They can be disabled with either the `--traditional` or `--posix`
  options.
* The interaction of POSIX locales and regexp matching in `gawk` has been confusing over
  the years. Today, `gawk` implements Rational Range Interpretation, where
  ranges of the form ‘`[a-z]`’ match *only* the characters numerically between
  ‘`a`’ through ‘`z`’ in the machine’s native character set. Usually this is ASCII,
  but it can be EBCDIC on IBM S/390 systems.
* Many people have contributed to `gawk` development over the years.
  We hope that the list provided in this chapter is complete and gives
  the appropriate credit where credit is due.

---

Next: [Implementation Notes](#Notes), Previous: [The Evolution of the `awk` Language](#Language-History), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix B Installing `gawk` [¶](#Installing-gawk)

This appendix provides instructions for installing `gawk` on the
various platforms that are supported by the developers. The primary
developer supports GNU/Linux (and Unix), whereas the other ports are
contributed.
See [Reporting Problems and Bugs](#Bugs)
for the email addresses of the people who maintain
the respective ports.

* [The `gawk` Distribution](#Gawk-Distribution)
* [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)
* [Installation on Other Operating Systems](#Non_002dUnix-Installation)
* [Reporting Problems and Bugs](#Bugs)
* [Other Freely Available `awk` Implementations](#Other-Versions)
* [Summary](#Installation-summary)

---

Next: [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation), Up: [Installing `gawk`](#Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### B.1 The `gawk` Distribution [¶](#The-gawk-Distribution)

This section describes how to get the `gawk`
distribution, how to extract it, and then what is in the various files and
subdirectories.

* [Getting the `gawk` Distribution](#Getting)
* [Extracting the Distribution](#Extracting)
* [Contents of the `gawk` Distribution](#Distribution-contents)

---

Next: [Extracting the Distribution](#Extracting), Up: [The `gawk` Distribution](#Gawk-Distribution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.1.1 Getting the `gawk` Distribution [¶](#Getting-the-gawk-Distribution)

There are two ways to get GNU software:

* Copy it from someone else who already has it.
* Retrieve `gawk`
  from the Internet host
  `ftp.gnu.org`, in the directory `/gnu/gawk`.
  Both anonymous `ftp` and `http` access are supported.
  If you have the `wget` program, you can use a command like
  the following:

  ```
  wget https://ftp.gnu.org/gnu/gawk/gawk-5.4.0.tar.gz
  ```

The GNU software archive is mirrored around the world.
The up-to-date list of mirror sites is available from
[the main FSF website](https://www.gnu.org/order/ftp.html).
Try to use one of the mirrors; they
will be less busy, and you can usually find one closer to your site.

You may also retrieve the `gawk` source code from the official
Git repository; for more information see [Accessing The `gawk` Git Repository](#Accessing-The-Source).

---

Next: [Contents of the `gawk` Distribution](#Distribution-contents), Previous: [Getting the `gawk` Distribution](#Getting), Up: [The `gawk` Distribution](#Gawk-Distribution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.1.2 Extracting the Distribution [¶](#Extracting-the-Distribution)

`gawk` is distributed as several `tar` files compressed with
different compression programs: `gzip`, `bzip2`,
and `xz`. For simplicity, the rest of these instructions assume
you are using the one compressed with the GNU Gzip program (`gzip`).

Once you have the distribution (e.g.,
`gawk-5.4.0.tar.gz`),
use `gzip` to expand the
file and then use `tar` to extract it. You can use the following
pipeline to produce the `gawk` distribution:

```
gzip -d -c gawk-5.4.0.tar.gz | tar -xvpf -
```

On a system with GNU `tar`, you can let `tar`
do the decompression for you:

```
tar -xvpzf gawk-5.4.0.tar.gz
```

Extracting the archive
creates a directory named `gawk-5.4.0`
in the current directory.

The distribution file name is of the form
`gawk-V.R.P.tar.gz`.
The V represents the major version of `gawk`,
the R represents the current release of version V, and
the P represents a *patch level*, meaning that minor bugs have
been fixed in the release. The current patch level is 0,
but when retrieving distributions, you should get the version with the highest
version, release, and patch level. (Note, however, that patch levels greater than
or equal to 60 denote “beta” or nonproduction software; you might not want
to retrieve such a version unless you don’t mind experimenting.)
If you are not on a Unix or GNU/Linux system, you need to make other arrangements
for getting and extracting the `gawk` distribution. You should consult
a local expert.

---

Previous: [Extracting the Distribution](#Extracting), Up: [The `gawk` Distribution](#Gawk-Distribution)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### B.1.3 Contents of the `gawk` Distribution [¶](#Contents-of-the-gawk-Distribution)

The `gawk` distribution has a number of C source files,
documentation files,
subdirectories, and files related to the configuration process
(see [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)),
as well as several subdirectories related to different non-Unix
operating systems:

Various ‘`.c`’, ‘`.y`’, and ‘`.h`’ files
:   These files contain the actual `gawk` source code.

`support/*`
:   C header and source files for routines that `gawk`
    uses, but that are not part of its core functionality.
    For example, argument parsing, regular expression matching,
    and random number generating routines are all kept here.

`ABOUT-NLS`
:   A file containing information about GNU `gettext` and translations.

`AUTHORS`
:   A file with some information about the authorship of `gawk`.
    It exists only to satisfy the pedants at the Free Software Foundation.

`README`

`README_d/README.*`
:   Descriptive files: `README` for `gawk` under Unix and the
    rest for the various hardware and software combinations.

`INSTALL`
:   A file providing an overview of the configuration and installation process.

`ChangeLog`
:   A detailed list of source code changes as bugs are fixed or improvements made.
    There are similar files in all of the subdirectories.

`ChangeLog.0`

`ChangeLog.1`
:   Older lists of source code changes.
    There are similar files in all of the subdirectories.

`NEWS`
:   A list of changes to `gawk` since the last release or patch.
    There may be similar files in other subdirectories.

`NEWS.0`

`NEWS.1`
:   Older lists of changes to `gawk`.
    There may be similar files in other subdirectories.

`COPYING`
:   The GNU General Public License.

`POSIX.STD`
:   A description of behaviors in the POSIX standard for `awk` that
    are left undefined, or where `gawk` may not comply fully, as well
    as a list of things that the POSIX standard should describe but does not.

`doc/awkforai.txt` [¶](#index-artificial-intelligence_002c-gawk-and)
:   Pointers to the original draft of
    a short article describing why `gawk` is a good language for
    artificial intelligence (AI) programming.

`doc/bc_notes`
:   A brief description of `gawk`’s “byte code” internals.

`doc/README.card`

`doc/ad.block`

`doc/awkcard.in`

`doc/cardfonts`

`doc/colors`

`doc/macros`

`doc/no.colors`

`doc/setter.outline`
:   The `troff` source for a five-color `awk` reference card.
    A modern version of `troff` such as GNU `troff` (`groff`) is
    needed to produce the color version. See the file `README.card`
    for instructions if you have an older `troff`.

`doc/gawk.1`
:   The `troff` source for a manual page describing `gawk`.
    This is distributed for the convenience of Unix users.

`doc/gawk.texi` [¶](#index-Texinfo-4)
:   The Texinfo source file for this Web page.
    It should be processed by
    `texi2dvi` or `texi2pdf`
    to produce a printed document, and
    by `makeinfo` to produce an Info or HTML file.
    The `Makefile` takes care of this processing and produces
    printable output via `texi2dvi` or `texi2pdf`.

`doc/gawk.info`
:   The generated Info file for this Web page.

`doc/gawkinet.texi`
:   The Texinfo source file for
    TCP/IP Internetworking with `gawk`.
    It should be processed with TeX
    (via `texi2dvi` or `texi2pdf`)
    to produce a printed document and
    with `makeinfo` to produce an Info or HTML file.

`doc/gawkinet.info`
:   The generated Info file for
    TCP/IP Internetworking with `gawk`.

`doc/gawkworkflow.texi`
:   The Texinfo source file for
    Participating in `gawk` Development.
    It should be processed with TeX
    (via `texi2dvi` or `texi2pdf`)
    to produce a printed document and
    with `makeinfo` to produce an Info or HTML file.

`doc/gawkworkflow.info`
:   The generated Info file for
    Participating in `gawk` Development.

`doc/pm-gawk.texi`
:   The Texinfo source file for
    Persistent-Memory `gawk` User Manual.
    It should be processed with TeX
    (via `texi2dvi` or `texi2pdf`)
    to produce a printed document and
    with `makeinfo` to produce an Info or HTML file.

`doc/pm-gawk.info`
:   The generated Info file for
    Persistent-Memory `gawk` User Manual.

`doc/pm-gawk.1`
:   The `troff` source for a manual page describing the
    persistent memory features presented in [Preserving Data Between Runs](#Persistent-Memory).

`doc/igawk.1`
:   The `troff` source for a manual page describing the `igawk`
    program presented in
    [An Easy Way to Use Library Functions](#Igawk-Program).
    (Since `gawk` can do its own `@include` processing,
    neither `igawk` nor `igawk.1` are installed.)

`doc/it/*`
:   Files for the Italian translation of this Web page, produced and
    contributed by Antonio Colombo and Marco Curreli.

`doc/Makefile.in`
:   The input file used during the configuration process to generate the
    actual `Makefile` for creating the documentation.

`Makefile.am`

`*/Makefile.am`
:   Files used by the GNU Automake software for generating
    the `Makefile.in` files used by Autoconf and
    `configure`.

`Makefile.in`

`aclocal.m4`

`build-aux/*`

`configh.in`

`configure.ac`

`configure`

`custom.h`

`missing_d/*`

`m4/*`
:   These files and subdirectories are used when configuring and compiling
    `gawk` for various Unix systems. Most of them are explained
    in [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation). The rest are there to support the main
    infrastructure.

`po/*`
:   The `po` library contains message translations.

`awklib/extract.awk`

`awklib/Makefile.am`

`awklib/Makefile.in`

`awklib/eg/*`
:   The `awklib` directory contains a copy of `extract.awk`
    (see [Extracting Programs from Texinfo Source Files](#Extract-Program)),
    which can be used to extract the sample programs from the Texinfo
    source file for this Web page. It also contains a `Makefile.in` file, which
    `configure` uses to generate a `Makefile`.
    `Makefile.am` is used by GNU Automake to create `Makefile.in`.
    The library functions from
    [A Library of `awk` Functions](#Library-Functions),
    are included as ready-to-use files in the `gawk` distribution.
    They are installed as part of the installation process.
    The rest of the programs in this Web page are available in appropriate
    subdirectories of `awklib/eg`.

`extension/*`
:   The source code, manual pages, and infrastructure files for
    the sample extensions included with `gawk`.
    See [Writing Extensions for `gawk`](#Dynamic-Extensions), for more information.

`extras/*`
:   Additional non-essential files. Currently, this directory contains some shell
    startup files to be installed in `/etc/profile.d` to aid in manipulating
    the `AWKPATH` and `AWKLIBPATH` environment variables.
    See [Shell Startup Files](#Shell-Startup-Files), for more information.

`posix/*`
:   Files needed for building `gawk` on POSIX-compliant systems.

`pc/*`
:   Files needed for building `gawk` under MS-Windows
    (see [Installation on MS-Windows](#PC-Installation) for details).

`vms/*`
:   Files needed for building `gawk` under OpenVMS
    (see [Compiling and Installing `gawk` on OpenVMS](#OpenVMS-Installation) for details).

`test/*`
:   A test suite for
    `gawk`. You can use ‘`make check`’ from the top-level `gawk`
    directory to run your version of `gawk` against the test suite.
    If `gawk` successfully passes ‘`make check`’, then you can
    be confident of a successful port.

---

Next: [Installation on Other Operating Systems](#Non_002dUnix-Installation), Previous: [The `gawk` Distribution](#Gawk-Distribution), Up: [Installing `gawk`](#Installation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
