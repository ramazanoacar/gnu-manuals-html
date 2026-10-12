### 9.1 Built-in Functions [¶](#Built_002din-Functions)

*Built-in* functions are always available for your `awk`
program to call. This section defines all the built-in functions
in `awk`; some of these are mentioned in other sections
but are summarized here for your convenience.

* [Calling Built-in Functions](#Calling-Built_002din)
* [Generating Boolean Values](#Boolean-Functions)
* [Numeric Functions](#Numeric-Functions)
* [String-Manipulation Functions](#String-Functions)
* [Input/Output Functions](#I_002fO-Functions)
* [Time Functions](#Time-Functions)
* [Bit-Manipulation Functions](#Bitwise-Functions)
* [Getting Type Information](#Type-Functions)
* [String-Translation Functions](#I18N-Functions)

---

Next: [Generating Boolean Values](#Boolean-Functions), Up: [Built-in Functions](#Built_002din)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.1 Calling Built-in Functions [¶](#Calling-Built_002din-Functions)

To call one of `awk`’s built-in functions, write the name of
the function followed
by arguments in parentheses. For example, ‘`atan2(y + z, 1)`’
is a call to the function `atan2()` and has two arguments.

Whitespace is ignored between the built-in function name and the
opening parenthesis, but nonetheless it is good practice to avoid using whitespace
there. User-defined functions do not permit whitespace in this way, and
it is easier to avoid mistakes by following a simple
convention that always works—no whitespace after a function name.

Each built-in function accepts a certain number of arguments.
In some cases, arguments can be omitted. The defaults for omitted
arguments vary from function to function and are described under the
individual functions. In some `awk` implementations, extra
arguments given to built-in functions are ignored. However, in `gawk`,
it is a fatal error to give extra arguments to a built-in function.

When a function is called, expressions that create the function’s actual
parameters are evaluated completely before the call is performed.
For example, in the following code fragment:

```
i = 4
j = sqrt(i++)
```

the variable `i` is incremented to the value five before `sqrt()`
is called with a value of four for its actual parameter.
The order of evaluation of the expressions used for the function’s
parameters is undefined. Thus, avoid writing programs that
assume that parameters are evaluated from left to right or from
right to left. For example:

```
i = 5
j = atan2(++i, i *= 2)
```

If the order of evaluation is left to right, then `i` first becomes
six, and then 12, and `atan2()` is called with the two arguments six
and 12. But if the order of evaluation is right to left, `i`
first becomes 10, then 11, and `atan2()` is called with the
two arguments 11 and 10.

---

Next: [Numeric Functions](#Numeric-Functions), Previous: [Calling Built-in Functions](#Calling-Built_002din), Up: [Built-in Functions](#Built_002din)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.2 Generating Boolean Values [¶](#Generating-Boolean-Values)

This function is specific to `gawk`. It is not
available in compatibility mode (see [Command-Line Options](#Options)):

`mkbool(expression)` [¶](#index-mkbool_0028_0029-function-_0028gawk_0029)
:   Return a Boolean-typed value based on the regular Boolean value
    of expression. Boolean “true” values have numeric value one.
    Boolean “false” values have numeric
    zero. This is discussed in more
    detail in [Boolean Typed Values](#Boolean-Typed-Values).

---

Next: [String-Manipulation Functions](#String-Functions), Previous: [Generating Boolean Values](#Boolean-Functions), Up: [Built-in Functions](#Built_002din)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.3 Numeric Functions [¶](#Numeric-Functions-1)

The following list describes all of
the built-in functions that work with numbers.
Optional parameters are enclosed in square brackets ([ ]):

`atan2(y, x)` [¶](#index-atan2_0028_0029-function)
:   Return the arctangent of `y / x` in radians.
    You can use ‘`pi = atan2(0, -1)`’ to retrieve the value of
    *pi*.

`cos(x)` [¶](#index-cos_0028_0029-function)
:   Return the cosine of x, with x in radians.

`exp(x)` [¶](#index-exp_0028_0029-function)
:   Return the exponential of x (`e ^ x`) or report
    an error if x is out of range. The range of values x can have
    depends on your machine’s floating-point representation.

`int(x)` [¶](#index-int_0028_0029-function)
:   Return the nearest integer to x, located between x and zero and
    truncated toward zero.
    For example, `int(3)` is 3, `int(3.9)` is 3, `int(-3.9)`
    is −3, and `int(-3)` is −3 as well.

`log(x)` [¶](#index-log_0028_0029-function)
:   Return the natural logarithm of x, if x is positive;
    otherwise, return NaN (“not a number”) on IEEE 754 systems.
    Additionally, `gawk` prints a warning message when `x`
    is negative.

`rand()` [¶](#index-rand_0028_0029-function)
:   Return a random number. The values of `rand()` are
    uniformly distributed between zero and one.
    The value could be zero but is never one.[48](#FOOT48)

    Often random integers are needed instead. Following is a user-defined function
    that can be used to obtain a random nonnegative integer less than n:

    ```
    function randint(n)
    {
        return int(n * rand())
    }
    ```

    The multiplication produces a random number greater than or equal to
    zero and less than `n`. Using `int()`, this result is made into
    an integer between zero and `n` − 1, inclusive.

    The following example uses a similar function to produce random integers
    between one and n. This program prints a new random number for
    each input record:

    ```
    # Function to roll a simulated die.
    function roll(n) { return 1 + int(rand() * n) }

    # Roll 3 six-sided dice and
    # print total number of points.
    {
        printf("%d points\n", roll(6) + roll(6) + roll(6))
    }
    ```

    > **CAUTION:** In most `awk` implementations, including `gawk`,
    > `rand()` starts generating numbers from the same
    > starting number, or *seed*, each time you run `awk`.[49](#FOOT49) Thus,
    > a program generates the same results each time you run it.
    > The numbers are random within one `awk` run but predictable
    > from run to run. This is convenient for debugging, but if you want
    > a program to do different things each time it is used, you must change
    > the seed to a value that is different in each run. To do this,
    > use `srand()`.

`sin(x)` [¶](#index-sin_0028_0029-function)
:   Return the sine of x, with x in radians.

`sqrt(x)` [¶](#index-sqrt_0028_0029-function)
:   Return the positive square root of x.
    `gawk` prints a warning message
    if x is negative. Thus, `sqrt(4)` is 2.

`srand(`[x]`)` [¶](#index-srand_0028_0029-function)
:   Set the starting point, or seed,
    for generating random numbers to the value x.

    Each seed value leads to a particular sequence of random
    numbers.[50](#FOOT50)
    Thus, if the seed is set to the same value a second time,
    the same sequence of random numbers is produced again.

    > **CAUTION:** Different `awk` implementations use different random-number
    > generators internally. Don’t expect the same `awk` program
    > to produce the same series of random numbers when executed by
    > different versions of `awk`.

    If the argument x is omitted, as in ‘`srand()`’, then the current
    date and time of day are used for a seed. This is the way to get random
    numbers that are truly unpredictable.

    The return value of `srand()` is the previous seed. This makes it
    easy to keep track of the seeds in case you need to consistently reproduce
    sequences of random numbers.

    POSIX does not specify the initial seed; it differs among `awk`
    implementations.

---

Next: [Input/Output Functions](#I_002fO-Functions), Previous: [Numeric Functions](#Numeric-Functions), Up: [Built-in Functions](#Built_002din)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.4 String-Manipulation Functions [¶](#String_002dManipulation-Functions)

The functions in this section look at or change the text of one
or more strings.

`gawk` understands locales (see [Where You Are Makes a Difference](#Locales)) and does all
string processing in terms of *characters*, not *bytes*.
This distinction is particularly important to understand for locales
where one character may be represented by multiple bytes. Thus, for
example, `length()` returns the number of characters in a string,
and not the number of bytes used to represent those characters. Similarly,
`index()` works with character indices, and not byte indices.

> **CAUTION:** A number of functions deal with indices into strings. For these
> functions, the first character of a string is at position (index) one.
> This is different from C and the languages descended from it, where the
> first character is at position zero. You need to remember this when
> doing index calculations, particularly if you are used to C.

In the following list, optional parameters are enclosed in square brackets ([ ]).
Several functions perform string substitution; the full discussion is
provided in the description of the `sub()` function, which comes
toward the end, because the list is presented alphabetically.

Those functions that are specific to `gawk` are marked with a
pound sign (‘`#`’). They are not available in compatibility mode
(see [Command-Line Options](#Options)):

`asort(`source [`,` dest [`,` how ] ]`) #` [¶](#index-asorti_0028_0029-function-_0028gawk_0029)

`asorti(`source [`,` dest [`,` how ] ]`) #`
:   These two functions are similar in behavior, so they are described
    together.

    > **NOTE:** The following description ignores the third argument, how, as it
    > requires understanding features that we have not discussed yet. Thus,
    > the discussion here is a deliberate simplification. (We do provide all
    > the details later on; see [Sorting Array Values and Indices with `gawk`](#Array-Sorting-Functions) for the full story.)

    Both functions return the number of elements in the array source.
    For `asort()`, `gawk` sorts the values of source
    and replaces the indices of the sorted values of source with
    sequential integers starting with one. If the optional array dest
    is specified, then source is duplicated into dest. dest
    is then sorted, leaving the indices of source unchanged.

    When comparing strings, `IGNORECASE` affects the sorting
    (see [Sorting Array Values and Indices with `gawk`](#Array-Sorting-Functions)). If the
    source array contains subarrays as values (see [Arrays of Arrays](#Arrays-of-Arrays)), they will come last, after all scalar values.
    Subarrays are *not* recursively sorted.

    For example, if the contents of `a` are as follows:

    ```
    a["last"] = "de"
    a["first"] = "sac"
    a["middle"] = "cul"
    ```

    A call to `asort()`:

    ```
    asort(a)
    ```

    results in the following contents of `a`:

    ```
    a[1] = "cul"
    a[2] = "de"
    a[3] = "sac"
    ```

    The `asorti()` function works similarly to `asort()`; however,
    the *indices* are sorted, instead of the values. Thus, in the
    previous example, starting with the same initial set of indices and
    values in `a`, calling ‘`asorti(a)`’ would yield:

    ```
    a[1] = "first"
    a[2] = "last"
    a[3] = "middle"
    ```

    > **NOTE:** You may not use either `SYMTAB` or `FUNCTAB` as the second
    > argument to these functions. Attempting to do so produces a fatal error.
    > You may use them as the first argument, but only if providing a second
    > array to use for the actual sorting.

    You are allowed to use the same array for both the source and dest
    arguments, but doing so only makes sense if you’re also supplying the third argument.

`gensub(regexp, replacement, how` [`, target`]`) #` [¶](#index-gensub_0028_0029-function-_0028gawk_0029-1)
:   Search the target string target for matches of the regular
    expression regexp. If how is a string beginning with
    ‘`g`’ or ‘`G`’ (short for “global”), then replace all matches
    of regexp with replacement. Otherwise, treat how
    as a number indicating which match of regexp to replace. Treat
    numeric values less than one as if they were one. If no target
    is supplied, use `$0`. Return the modified string as the result
    of the function. The original target string is *not* changed.

    The returned value is *always* a string, even if the original
    target was a number or a regexp value.

    `gensub()` is a general substitution function. Its purpose is
    to provide more features than the standard `sub()` and `gsub()`
    functions.

    `gensub()` provides an additional feature that is not available
    in `sub()` or `gsub()`: the ability to specify components of a
    regexp in the replacement text. This is done by using parentheses in
    the regexp to mark the components and then specifying ‘`\N`’
    in the replacement text, where N is a digit from 1 to 9.
    For example:

    ```
    $ gawk '
    > BEGIN {
    >      a = "abc def"
    >      b = gensub(/(.+) (.+)/, "\\2 \\1", "g", a)
    >      print b
    > }'
    -| def abc
    ```

    As with `sub()`, you must type two backslashes in order
    to get one into the string.
    In the replacement text, the sequence ‘`\0`’ represents the entire
    matched text, as does the character ‘`&`’.

    The following example shows how you can use the third argument to control
    which match of the regexp should be changed:

    ```
    $ echo a b c a b c |
    > gawk '{ print gensub(/a/, "AA", 2) }'
    -| a b c AA b c
    ```

    In this case, `$0` is the default target string.
    `gensub()` returns the new string as its result, which is
    passed directly to `print` for printing.

    If the how argument is a string that does not begin with ‘`g`’ or
    ‘`G`’, or if it is a number that is less than or equal to zero, only one
    substitution is performed. If how is zero, `gawk` issues
    a warning message.

    If regexp does not match target, `gensub()`’s return value
    is the original unchanged value of target. Note that, as mentioned
    above, the returned value is a string, even if target was not.

    In the replacement string, a backslash before a non-digit character
    is simply elided. For example, ‘`\q`’ becomes ‘`q`’ in the result.
    If the final character in the replacement string is a backslash,
    it is left alone.

`gsub(regexp, replacement` [`, target`]`)` [¶](#index-gsub_0028_0029-function-1)
:   Search target for
    *all* of the longest, leftmost, *nonoverlapping* matching
    substrings it can find and replace them with replacement.
    The ‘`g`’ in `gsub()` stands for
    “global,” which means replace everywhere. For example:

    ```
    { gsub(/Britain/, "United Kingdom"); print }
    ```

    replaces all occurrences of the string ‘`Britain`’ with ‘`United
    Kingdom`’ for all input records.

    The `gsub()` function returns the number of substitutions made. If
    the variable to search and alter (target) is
    omitted, then the entire input record (`$0`) is used.
    As in `sub()`, the characters ‘`&`’ and ‘`\`’ are special,
    and the third argument must be assignable.

`index(in, find)` [¶](#index-index_0028_0029-function)
:   Search the string in for the first occurrence of the string
    find, and return the position in characters where that occurrence
    begins in the string in. Consider the following example:

    ```
    $ awk 'BEGIN { print index("peanut", "an") }'
    -| 3
    ```

    If find is not found, `index()` returns zero.

    With BWK `awk` and `gawk`,
    it is a fatal error to use a regexp constant for find.
    Other implementations allow it, simply treating the regexp
    constant as an expression meaning ‘`$0 ~ /regexp/`’. (d.c.)

`length(`[string]`)` [¶](#index-length_0028_0029-function)
:   Return the number of characters in string. If
    string is a number, the length of the digit string representing
    that number is returned. For example, `length("abcde")` is five. By
    contrast, `length(15 * 35)` works out to three. In this example,
    15 \* 35 = 525,
    and 525 is then converted to the string `"525"`, which has
    three characters.

    If no argument is supplied, `length()` returns the length of `$0`.

    > **NOTE:** In older versions of `awk`, the `length()` function could
    > be called
    > without any parentheses. Doing so is considered poor practice,
    > although the 2008 POSIX standard explicitly allows it, to
    > support historical practice. For programs to be maximally portable,
    > always supply the parentheses.

    For historical compatibility with Unix `awk`,
    if `length()` is called with a variable that has not been used,
    `gawk` forces the variable to be a scalar. Other
    implementations of `awk` leave the variable without a type.
    (d.c.)
    Consider:

    ```
    $ gawk 'BEGIN { print length(x) ; x[1] = 1 }'
    -| 0
    error→ gawk: fatal: attempt to use scalar `x' as array

    $ nawk 'BEGIN { print length(x) ; x[1] = 1 }'
    -| 0
    ```

    If `--lint` has
    been specified on the command line, `gawk` issues a
    warning about this.

    With `gawk` and several other `awk` implementations, when given an
    array argument, the `length()` function returns the number of elements
    in the array.
    This is less useful than it might seem at first, as the
    array is not guaranteed to be indexed from one to the number of elements
    in it.

    Applying `length()` to an array was standardized by POSIX in 2024.
    Thus this usage should become more portable over time.

`match(string, regexp` [`, array`]`)` [¶](#index-match_0028_0029-function)
:   Search string for the
    longest, leftmost substring matched by the regular expression
    regexp and return the character position (index)
    at which that substring begins (one, if it starts at the beginning of
    string). If no match is found, return zero.

    The regexp argument may be either a regexp constant
    (`/`…`/`) or a string constant (`"`…`"`).
    In the latter case, the string is treated as a regexp to be matched.
    See [Using Dynamic Regexps](#Computed-Regexps) for a
    discussion of the difference between the two forms, and the
    implications for writing your program correctly.

    The order of the first two arguments is the opposite of most other string
    functions that work with regular expressions, such as
    `sub()` and `gsub()`. It might help to remember that
    for `match()`, the order is the same as for the ‘`~`’ operator:
    ‘`string ~ regexp`’.

    The `match()` function sets the predefined variable `RSTART` to
    the index. It also sets the predefined variable `RLENGTH` to the
    length in characters of the matched substring. If no match is found,
    `RSTART` is set to zero, and `RLENGTH` to −1.

    For example:

    ```
    {
        if ($1 == "FIND")
            regex = $2
        else {
            where = match($0, regex)
            if (where != 0)
                print "Match of", regex, "found at", where, "in", $0
           }
    }
    ```

    This program looks for lines that match the regular expression stored in
    the variable `regex`. This regular expression can be changed. If the
    first word on a line is ‘`FIND`’, `regex` is changed to be the
    second word on that line. Therefore, if given:

    ```
    FIND ru+n
    My program runs
    but not very quickly
    FIND Melvin
    JF+KM
    This line is property of Reality Engineering Co.
    Melvin was here.
    ```

    `awk` prints:

    ```
    Match of ru+n found at 12 in My program runs
    Match of Melvin found at 1 in Melvin was here.
    ```

    If array is present, it is cleared, and then the zeroth element
    of array is set to the entire portion of string
    matched by regexp. If regexp contains parentheses,
    the integer-indexed elements of array are set to contain the
    portion of string matching the corresponding parenthesized
    subexpression.
    For example:

    ```
    $ echo foooobazbarrrrr |
    > gawk '{ match($0, /(fo+).+(bar*)/, arr)
    >         print arr[1], arr[2] }'
    -| foooo barrrrr
    ```

    In addition,
    multidimensional subscripts are available providing
    the start index and length of each matched subexpression:

    ```
    $ echo foooobazbarrrrr |
    > gawk '{ match($0, /(fo+).+(bar*)/, arr)
    >           print arr[1], arr[2]
    >           print arr[1, "start"], arr[1, "length"]
    >           print arr[2, "start"], arr[2, "length"]
    > }'
    -| foooo barrrrr
    -| 1 5
    -| 9 7
    ```

    There may not be subscripts for the start and index for every parenthesized
    subexpression, because they may not all have matched text; thus, they
    should be tested for with the `in` operator
    (see [Referring to an Array Element](#Reference-to-Elements)).

    The array argument to `match()` is a
    `gawk` extension. In compatibility mode
    (see [Command-Line Options](#Options)),
    using a third argument is a fatal error.

`patsplit(string, array` [`, fieldpat` [`, seps` ] ]`) #` [¶](#index-patsplit_0028_0029-function-_0028gawk_0029)
:   Divide
    string into pieces (or “fields”) defined by fieldpat
    and store the pieces in array and the separator strings in the
    seps array. The first piece is stored in
    `array[1]`, the second piece in `array[2]`, and so
    forth. The third argument, fieldpat, is
    a regexp describing the fields in string (just as `FPAT` is
    a regexp describing the fields in input records).
    It may be either a regexp constant or a string.
    If fieldpat is omitted, the value of `FPAT` is used.
    `patsplit()` returns the number of elements created.
    `seps[i]` is
    the possibly null separator string
    after `array[i]`.
    The possibly null leading separator will be in `seps[0]`.
    So a non-null string with n fields will have n+1 separators.
    A null string has no fields or separators.

    The `patsplit()` function splits strings into pieces in a
    manner similar to the way input lines are split into fields using `FPAT`
    (see [Defining Fields by Content](#Splitting-By-Content)).

    Before splitting the string, `patsplit()` deletes any previously existing
    elements in the arrays array and seps.

`split(string, array` [`, fieldsep` [`, seps` ] ]`)` [¶](#index-split_0028_0029-function)
:   Divide string into pieces separated by fieldsep
    and store the pieces in array and the separator strings in the
    seps array. The first piece is stored in
    `array[1]`, the second piece in `array[2]`, and so
    forth. The string value of the third argument, fieldsep, is
    a regexp describing where to split string (much as `FS` can
    be a regexp describing where to split input records).
    If fieldsep is omitted, the value of `FS` is used.
    `split()` returns the number of elements created.
    seps is a `gawk` extension, with `seps[i]`
    being the separator string
    between `array[i]` and `array[i+1]`.
    If fieldsep is a single
    space, then any leading whitespace goes into `seps[0]` and
    any trailing
    whitespace goes into `seps[n]`, where n is the
    return value of
    `split()` (i.e., the number of elements in array).

    The `split()` function splits strings into pieces in the same way
    that input lines are split into fields. For example:

    ```
    split("cul-de-sac", a, "-", seps)
    ```

    splits the string `"cul-de-sac"` into three fields using ‘`-`’ as the
    separator. It sets the contents of the array `a` as follows:

    ```
    a[1] = "cul"
    a[2] = "de"
    a[3] = "sac"
    ```

    and sets the contents of the array `seps` as follows:

    ```
    seps[1] = "-"
    seps[2] = "-"
    ```

    The value returned by this call to `split()` is three.

    If `gawk` is invoked with `--csv`, then a two-argument
    call to `split()` splits the string using the CSV parsing rules as
    described in [Working With Comma Separated Value Files](#Comma-Separated-Fields). With three and four arguments,
    `split()` works as just described. The four-argument call makes
    no sense, since each element of seps would simply consist of a
    string containing a comma.

    As with input field-splitting, when the value of fieldsep is
    `" "`, leading and trailing whitespace is ignored in values assigned to
    the elements of
    array but not in seps, and the elements
    are separated by runs of whitespace.
    Also, as with input field splitting, if fieldsep is the null string, each
    individual character in the string is split into its own array element.
    (c.e.)
    Additionally, if fieldsep is a single-character string, that string acts
    as the separator, even if its value is a regular expression metacharacter.

    Note, however, that `RS` has no effect on the way `split()`
    works. Even though ‘`RS = ""`’ causes the newline character to also be an input
    field separator, this does not affect how `split()` splits strings.

    Modern implementations of `awk`, including `gawk`, allow
    the third argument to be a regexp constant (`/`…`/`)
    as well as a string. (d.c.)
    The POSIX standard allows this as well.
    See [Using Dynamic Regexps](#Computed-Regexps) for a
    discussion of the difference between using a string constant or a regexp constant,
    and the implications for writing your program correctly.

    Before splitting the string, `split()` deletes any previously existing
    elements in the arrays array and seps.

    If string is null, the array has no elements. (So this is a portable
    way to delete an entire array with one statement.
    See [The `delete` Statement](#Delete).)

    If string does not match fieldsep at all (but is not null),
    array has one element only. The value of that element is the original
    string.

    In POSIX mode (see [Command-Line Options](#Options)), the fourth argument is not allowed.

`sprintf(format, expression1, …)` [¶](#index-sprintf_0028_0029-function-1)
:   Return (without printing) the string that `printf` would
    have printed out with the same arguments
    (see [Using `printf` Statements for Fancier Printing](#Printf)).
    For example:

    ```
    pival = sprintf("pi = %.2f (approx.)", 22/7)
    ```

    assigns the string ‘`pi = 3.14 (approx.)`’ to the variable `pival`.

`strtonum(str) #`
:   Examine str and return its numeric value. If str
    begins with a leading ‘`0`’, `strtonum()` assumes that str
    is an octal number. If str begins with a leading ‘`0x`’ or
    ‘`0X`’, `strtonum()` assumes that str is a hexadecimal number.
    For example:

    ```
    $ echo 0x11 |
    > gawk '{ printf "%d\n", strtonum($1) }'
    -| 17
    ```

    Using the `strtonum()` function is *not* the same as adding zero
    to a string value; the automatic coercion of strings to numbers
    works only for decimal data, not for octal or hexadecimal.[51](#FOOT51)

    Note also that `strtonum()` uses the current locale’s decimal point
    for recognizing numbers (see [Where You Are Makes a Difference](#Locales)).

`sub(regexp, replacement` [`, target`]`)` [¶](#index-sub_0028_0029-function-1)
:   Search target, which is treated as a string, for the
    leftmost, longest substring matched by the regular expression regexp.
    Modify the entire string
    by replacing the matched text with replacement.
    The modified string becomes the new value of target.
    Return the number of substitutions made (zero or one).

    The regexp argument may be either a regexp constant
    (`/`…`/`) or a string constant (`"`…`"`).
    In the latter case, the string is treated as a regexp to be matched.
    See [Using Dynamic Regexps](#Computed-Regexps) for a
    discussion of the difference between the two forms, and the
    implications for writing your program correctly.

    This function is peculiar because target is not simply
    used to compute a value, and not just any expression will do—it
    must be a variable, field, or array element so that `sub()` can
    store a modified value there. If this argument is omitted, then the
    default is to use and alter `$0`.[52](#FOOT52)
    For example:

    ```
    str = "water, water, everywhere"
    sub(/at/, "ith", str)
    ```

    sets `str` to ‘`wither, water, everywhere`’, by replacing the
    leftmost longest occurrence of ‘`at`’ with ‘`ith`’.

    If the special character ‘`&`’ appears in replacement, it
    stands for the precise substring that was matched by regexp. (If
    the regexp can match more than one string, then this precise substring
    may vary.) For example:

    ```
    { sub(/candidate/, "& and his wife"); print }
    ```

    changes the first occurrence of ‘`candidate`’ to ‘`candidate
    and his wife`’ on each input line.
    Here is another example:

    ```
    $ awk 'BEGIN {
    >         str = "daabaaa"
    >         sub(/a+/, "C&C", str)
    >         print str
    > }'
    -| dCaaCbaaa
    ```

    This shows how ‘`&`’ can represent a nonconstant string and also
    illustrates the “leftmost, longest” rule in regexp matching
    (see [How Much Text Matches?](#Leftmost-Longest)).

    The effect of this special character (‘`&`’) can be turned off by putting a
    backslash before it in the string. As usual, to insert one backslash in
    the string, you must write two backslashes. Therefore, write ‘`\\&`’
    in a string constant to include a literal ‘`&`’ in the replacement.
    For example, the following shows how to replace the first ‘`|`’ on each line with
    an ‘`&`’:

    ```
    { sub(/\|/, "\\&"); print }
    ```

    As mentioned, the third argument to `sub()` must
    be a variable, field, or array element.
    Some versions of `awk` allow the third argument to
    be an expression that is not an lvalue. In such a case, `sub()`
    still searches for the pattern and returns zero or one, but the result of
    the substitution (if any) is thrown away because there is no place
    to put it. Such versions of `awk` accept expressions
    like the following:

    ```
    sub(/USA/, "United States", "the USA and Canada")
    ```

    For historical compatibility, `gawk` accepts such erroneous code.
    However, using any other nonchangeable
    object as the third parameter causes a fatal error and your program
    will not run.

    Finally, if the regexp is not a regexp constant, it is converted into a
    string, and then the value of that string is treated as the regexp to match.

`substr(string, start` [`, length` ]`)` [¶](#index-substr_0028_0029-function)
:   Return a length-character-long substring of string,
    starting at character number start. The first character of a
    string is character number one.[53](#FOOT53)
    For example, `substr("washington", 5, 3)` returns `"ing"`.

    If length is not present, `substr()` returns the whole suffix of
    string that begins at character number start. For example,
    `substr("washington", 5)` returns `"ington"`. The whole
    suffix is also returned
    if length is greater than the number of characters remaining
    in the string, counting from character start.

    If start is less than one, `substr()` treats it as
    if it was one. (POSIX doesn’t specify what to do in this case:
    BWK `awk` acts this way, and therefore `gawk`
    does too.)
    If start is greater than the number of characters
    in the string, `substr()` returns the null string.
    Similarly, if length is present but less than or equal to zero,
    the null string is returned.

    The string returned by `substr()` *cannot* be
    assigned. Thus, it is a mistake to attempt to change a portion of
    a string, as shown in the following example:

    ```
    string = "abcdef"
    # try to get "abCDEf", won't work
    substr(string, 3, 3) = "CDE"
    ```

    It is also a mistake to use `substr()` as the third argument
    of `sub()` or `gsub()`:

    ```
    gsub(/xyz/, "pdq", substr($0, 5, 20))  # WRONG
    ```

    (Some commercial versions of `awk` treat
    `substr()` as assignable, but doing so is not portable.)

    If you need to replace bits and pieces of a string, combine `substr()`
    with string concatenation, in the following manner:

    ```
    string = "abcdef"
    ...
    string = substr(string, 1, 2) "CDE" substr(string, 6)
    ```

`tolower(string)` [¶](#index-tolower_0028_0029-function)
:   Return a copy of string, with each uppercase character
    in the string replaced with its corresponding lowercase character.
    Nonalphabetic characters are left unchanged. For example,
    `tolower("MiXeD cAsE 123")` returns `"mixed case 123"`.

`toupper(string)` [¶](#index-toupper_0028_0029-function)
:   Return a copy of string, with each lowercase character
    in the string replaced with its corresponding uppercase character.
    Nonalphabetic characters are left unchanged. For example,
    `toupper("MiXeD cAsE 123")` returns `"MIXED CASE 123"`.

At first glance, the `split()` and `patsplit()` functions appear to be
mirror images of each other. But there are differences:

* `split()` treats its third argument like `FS`, with all the
  special rules involved for `FS`.
* Matching of null strings differs. This is discussed in [`FS` Versus `FPAT`: A Subtle Difference](#FS-versus-FPAT).

| Matching the Null String |
| --- |
| In `awk`, the ‘`*`’ operator can match the null string. This is particularly important for the `sub()`, `gsub()`, and `gensub()` functions. For example:  ``` $ echo abc | awk '{ gsub(/m*/, "X"); print }' -| XaXbXcX ```  Although this makes a certain amount of sense, it can be surprising. |

* [More about ‘`\`’ and ‘`&`’ with `sub()`, `gsub()`, and `gensub()`](#Gory-Details)

---

Up: [String-Manipulation Functions](#String-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.4.1 More about ‘`\`’ and ‘`&`’ with `sub()`, `gsub()`, and `gensub()` [¶](#More-about-_005c-and-_0026-with-sub_0028_0029_002c-gsub_0028_0029_002c-and-gensub_0028_0029)

> *I collect spores, molds, and fungus.*

— *Dr. Egon Spengler (“Ghostbusters,” 1984)*

> **CAUTION:** This subsubsection has been reported to cause headaches.
> You might want to skip it upon first reading.

When using `sub()`, `gsub()`, or `gensub()`, and trying to get literal
backslashes and ampersands into the replacement text, you need to remember
that there are several levels of *escape processing* going on.

First, there is the *lexical* level, which is when `awk` reads
your program
and builds an internal copy of it to execute.
Then there is the runtime level, which is when `awk` actually scans the
replacement string to determine what to generate.

At both levels, `awk` looks for a defined set of characters that
can come after a backslash. At the lexical level, it looks for the
escape sequences listed in [Escape Sequences](#Escape-Sequences).
Thus, for every ‘`\`’ that `awk` processes at the runtime
level, you must type two backslashes at the lexical level.
When a character that is not valid for an escape sequence follows the
‘`\`’, BWK `awk` and `gawk` both simply remove the initial
‘`\`’ and put the next character into the string. Thus, for
example, `"a\qb"` is treated as `"aqb"`.

At the runtime level, the various functions handle sequences of
‘`\`’ and ‘`&`’ differently. The situation is (sadly) somewhat complex.
Historically, the `sub()` and `gsub()` functions treated the
two-character sequence ‘`\&`’ specially; this sequence was replaced in
the generated text with a single ‘`&`’. Any other ‘`\`’ within
the replacement string that did not precede an ‘`&`’ was passed
through unchanged. This is illustrated in [Table 9.1](#table_002dsub_002descapes).

| You type | `sub()` sees | `sub()` generates |
| --- | --- | --- |
| `\&` | `&` | The matched text |
| `\\&` | `\&` | A literal ‘`&`’ |
| `\\\&` | `\&` | A literal ‘`&`’ |
| `\\\\&` | `\\&` | A literal ‘`\&`’ |
| `\\\\\&` | `\\&` | A literal ‘`\&`’ |
| `\\\\\\&` | `\\\&` | A literal ‘`\\&`’ |
| `\\q` | `\q` | A literal ‘`\q`’ |

**Table 9.1:** Historical escape sequence processing for `sub()` and `gsub()`

This table shows the lexical-level processing, where
an odd number of backslashes becomes an even number at the runtime level,
as well as the runtime processing done by `sub()`.
(For the sake of simplicity, the rest of the following tables only show the
case of even numbers of backslashes entered at the lexical level.)

The problem with the historical approach is that there is no way to get
a literal ‘`\`’ followed by the matched text.

Several editions of the POSIX standard attempted to fix this problem
but weren’t successful. The details are irrelevant at this point in time.

At one point, the `gawk` maintainer submitted
proposed text for a revised standard that
reverts to rules that correspond more closely to the original existing
practice. The proposed rules have special cases that make it possible
to produce a ‘`\`’ preceding the matched text.
This is shown in
[Table 9.2](#table_002dsub_002dproposed).

| You type | `sub()` sees | `sub()` generates |
| --- | --- | --- |
| `\\\\\\&` | `\\\&` | A literal ‘`\&`’ |
| `\\\\&` | `\\&` | A literal ‘`\`’, followed by the matched text |
| `\\&` | `\&` | A literal ‘`&`’ |
| `\\q` | `\q` | A literal ‘`\q`’ |
| `\\\\` | `\\` | `\\` |

**Table 9.2:** `gawk` rules for `sub()` and backslash

In a nutshell, at the runtime level, there are now three special sequences
of characters (‘`\\\&`’, ‘`\\&`’, and ‘`\&`’) whereas historically
there was only one. However, as in the historical case, any ‘`\`’ that
is not part of one of these three sequences is not special and appears
in the output literally.

`gawk` 3.0 and 3.1 follow these rules for `sub()` and
`gsub()`. The POSIX standard took much longer to be revised than
was expected. In addition, the `gawk` maintainer’s proposal was
lost during the standardization process. The final rules are
somewhat simpler. The results are similar except for one case.

The POSIX rules state that ‘`\&`’ in the replacement string produces
a literal ‘`&`’, ‘`\\`’ produces a literal ‘`\`’, and ‘`\`’ followed
by anything else is not special; the ‘`\`’ is placed straight into the output.
These rules are presented in [Table 9.3](#table_002dposix_002dsub).

| You type | `sub()` sees | `sub()` generates |
| --- | --- | --- |
| `\\\\\\&` | `\\\&` | A literal ‘`\&`’ |
| `\\\\&` | `\\&` | A literal ‘`\`’, followed by the matched text |
| `\\&` | `\&` | A literal ‘`&`’ |
| `\\q` | `\q` | A literal ‘`\q`’ |
| `\\\\` | `\\` | `\` |

**Table 9.3:** POSIX rules for `sub()` and `gsub()`

The only case where the difference is noticeable is the last one: ‘`\\\\`’
is seen as ‘`\\`’ and produces ‘`\`’ instead of ‘`\\`’.

Starting with version 3.1.4, `gawk` followed the POSIX rules
when `--posix` was specified (see [Command-Line Options](#Options)). Otherwise,
it continued to follow the proposed rules, as
that had been its behavior for many years.

When version 4.0.0 was released, the `gawk` maintainer
made the POSIX rules the default, breaking well over a decade’s worth
of backward compatibility.[54](#FOOT54) Needless to say, this was a bad idea,
and as of version 4.0.1, `gawk` resumed its historical
behavior, and only follows the POSIX rules when `--posix` is given.

The rules for `gensub()` are considerably simpler. At the runtime
level, whenever `gawk` sees a ‘`\`’, if the following character
is a digit, then the text that matched the corresponding parenthesized
subexpression is placed in the generated output. Otherwise,
no matter what character follows the ‘`\`’, it
appears in the generated text and the ‘`\`’ does not,
as shown in [Table 9.4](#table_002dgensub_002descapes).

| You type | `gensub()` sees | `gensub()` generates |
| --- | --- | --- |
| `&` | `&` | The matched text |
| `\\&` | `\&` | A literal ‘`&`’ |
| `\\\\` | `\\` | A literal ‘`\`’ |
| `\\\\&` | `\\&` | A literal ‘`\`’, then the matched text |
| `\\\\\\&` | `\\\&` | A literal ‘`\&`’ |
| `\\q` | `\q` | A literal ‘`q`’ |

**Table 9.4:** Escape sequence processing for `gensub()`

Because of the complexity of the lexical- and runtime-level processing
and the special cases for `sub()` and `gsub()`,
we recommend the use of `gawk` and `gensub()` when you have
to do substitutions.

---

Next: [Time Functions](#Time-Functions), Previous: [String-Manipulation Functions](#String-Functions), Up: [Built-in Functions](#Built_002din)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.5 Input/Output Functions [¶](#Input_002fOutput-Functions)

The following functions relate to input/output (I/O).
Optional parameters are enclosed in square brackets ([ ]):

`close(`filename [`,` how]`)` [¶](#index-close_0028_0029-function-1)
:   Close the file filename for input or output. Alternatively, the
    argument may be a shell command that was used for creating a coprocess, or
    for redirecting to or from a pipe; then the coprocess or pipe is closed.
    See [Closing Input and Output Redirections](#Close-Files-And-Pipes)
    for more information.

    When closing a coprocess, it is occasionally useful to first close
    one end of the two-way pipe and then to close the other. This is done
    by providing a second argument to `close()`. This second argument
    (how)
    should be one of the two string values `"to"` or `"from"`,
    indicating which end of the pipe to close. Case in the string does
    not matter.
    See [Two-Way Communications with Another Process](#Two_002dway-I_002fO),
    which discusses this feature in more detail and gives an example.

    Note that the second argument to `close()` is a `gawk`
    extension; it is not available in compatibility mode (see [Command-Line Options](#Options)).

`fflush(`[filename]`)` [¶](#index-fflush_0028_0029-function)
:   Flush any buffered output associated with filename, which is either a
    file opened for writing or a shell command for redirecting output to
    a pipe or coprocess.

    Many utility programs *buffer* their output (i.e., they save information
    to write to a disk file or the screen in memory until there is enough
    for it to be worthwhile to send the data to the output device).
    This is often more efficient than writing
    every little bit of information as soon as it is ready. However, sometimes
    it is necessary to force a program to *flush* its buffers (i.e.,
    write the information to its destination, even if a buffer is not full).
    This is the purpose of the `fflush()` function—`gawk` also
    buffers its output, and the `fflush()` function forces
    `gawk` to flush its buffers.

    Brian Kernighan added `fflush()` to his `awk` in April
    1992. For two decades, it was a common extension. In December
    2012, it was accepted for inclusion into the POSIX standard.
    See [the Austin Group website](https://austingroupbugs.net/view.php?id=634).

    POSIX standardizes `fflush()` as follows: if there
    is no argument, or if the argument is the null string (`""`),
    then `awk` flushes the buffers for *all* open output files
    and pipes.

    > **NOTE:** Prior to version 4.0.2, `gawk`
    > would flush only the standard output if there was no argument,
    > and flush all output files and pipes if the argument was the null
    > string. This was changed in order to be compatible with BWK
    > `awk`, in the hope that standardizing this
    > feature in POSIX would then be easier (which indeed proved to be the case).
    >
    > With `gawk`,
    > you can use ‘`fflush("/dev/stdout")`’ if you wish to flush
    > only the standard output.

    `fflush()` returns zero if the buffer is successfully flushed;
    otherwise, it returns a nonzero value. (`gawk` returns −1.)
    In the case where all buffers are flushed, the return value is zero
    only if all buffers were flushed successfully. Otherwise, it is
    −1, and `gawk` warns about the problem filename.

    `gawk` also issues a warning message if you attempt to flush
    a file or pipe that was opened for reading (such as with `getline`),
    or if filename is not an open file, pipe, or coprocess.
    In such a case, `fflush()` returns −1, as well.

| Interactive Versus Noninteractive Buffering |
| --- |
| As a side point, buffering issues can be even more confusing if your program is *interactive* (i.e., communicating with a user sitting at a keyboard).[55](#FOOT55)  Interactive programs generally *line buffer* their output (i.e., they write out every line). Noninteractive programs wait until they have a full buffer, which may be many lines of output. Here is an example of the difference:  ``` $ awk '{ print $1 + $2 }' 1 1 -| 2 2 3 -| 5 Ctrl-d ```  Each line of output is printed immediately. Compare that behavior with this example:  ``` $ awk '{ print $1 + $2 }' | cat 1 1 2 3 Ctrl-d -| 2 -| 5 ```  Here, no output is printed until after the `Ctrl-d` is typed, because it is all buffered and sent down the pipe to `cat` in one shot. |

`system(command)` [¶](#index-system_0028_0029-function)
:   Execute the operating system
    command command and then return to the `awk` program.
    Return command’s exit status (see further on).

    For example, if the following fragment of code is put in your `awk`
    program:

    ```
    END {
         system("date | mail -s 'awk run done' root")
    }
    ```

    the system administrator is sent mail when the `awk` program
    finishes processing input and begins its end-of-input processing.

    Note that redirecting `print` or `printf` into a pipe is often
    enough to accomplish your task. If you need to run many commands, it
    is more efficient to simply print them down a pipeline to the shell:

    ```
    while (more stuff to do)
        print command | "/bin/sh"
    close("/bin/sh")
    ```

    However, if your `awk`
    program is interactive, `system()` is useful for running large
    self-contained programs, such as a shell or an editor.
    Some operating systems cannot implement the `system()` function.
    `system()` causes a fatal error if it is not supported.

    > **NOTE:** When `--sandbox` is specified, the `system()` function is disabled
    > (see [Command-Line Options](#Options)).

    On POSIX systems, a command’s exit status is a 16-bit number. The exit
    value passed to the C `exit()` function is held in the high-order
    eight bits. The low-order bits indicate if the process was killed by a
    signal (bit 7) and if so, the guilty signal number (bits 0–6).

    In the past, `awk`’s `system()` function simply
    returned the exit status value divided by 256. In the normal case this
    gives the exit status but in the case of death-by-signal it yields
    a fractional floating-point value.[56](#FOOT56) POSIX states that `awk`’s
    `system()` should return the full 16-bit value.

    `gawk` steers a middle ground.
    The return values are summarized in [Table 9.5](#table_002dsystem_002dreturn_002dvalues).

    | Situation | Return value from `system()` |
    | --- | --- |
    | `--posix` | C `system()`’s value |
    | Normal exit of command | Command’s exit status |
    | Death by signal of command | 256 + number of murderous signal |
    | Death by signal of command with core dump | 512 + number of murderous signal |
    | Some kind of error | −1 |

    **Table 9.5:** Return values from `system()`

As of August, 2018, BWK `awk` now follows `gawk`’s behavior
for the return value of `system()`.

| Controlling Output Buffering with `system()` |
| --- |
| The `fflush()` function provides explicit control over output buffering for individual files and pipes. However, its use is not portable to many older `awk` implementations. An alternative method to flush output buffers is to call `system()` with a null string as its argument:  ``` system("")   # flush output ```  `gawk` treats this use of the `system()` function as a special case and is smart enough not to run a shell (or other command interpreter) with the empty command. Therefore, with `gawk`, this idiom is not only useful, it is also efficient. Although this method should work with other `awk` implementations, it does not necessarily avoid starting an unnecessary shell. (Other implementations may only flush the buffer associated with the standard output and not necessarily all buffered output.)  If you think about what a programmer expects, it makes sense that `system()` should flush any pending output. The following program:  ``` BEGIN {      print "first print"      system("echo system echo")      print "second print" } ```  must print:  ``` first print system echo second print ```  and not:  ``` system echo first print second print ```  If `awk` did not flush its buffers before calling `system()`, you would see the latter (undesirable) output. |

---

Next: [Bit-Manipulation Functions](#Bitwise-Functions), Previous: [Input/Output Functions](#I_002fO-Functions), Up: [Built-in Functions](#Built_002din)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.6 Time Functions [¶](#Time-Functions-1)

`awk` programs are commonly used to process log files
containing timestamp information, indicating when a
particular log record was written. Many programs log their timestamps
in the form returned by the `time()` system call, which is the
number of seconds since a particular epoch. On POSIX-compliant systems,
it is the number of seconds since
1970-01-01 00:00:00 UTC, not counting leap
seconds.[57](#FOOT57)
All known POSIX-compliant systems support timestamps from 0 through
231 − 1,
which is sufficient to represent times through
2038-01-19 03:14:07 UTC. Many systems support a wider range of timestamps,
including negative timestamps that represent times before the
epoch.

In order to make it easier to process such log files and to produce
useful reports, `gawk` provides the following functions for
working with timestamps. They are `gawk` extensions; they are
not specified in the POSIX standard.[58](#FOOT58)
However, recent versions
of `mawk` (see [Other Freely Available `awk` Implementations](#Other-Versions)) also support these functions.
Optional parameters are enclosed in square brackets ([ ]):

`mktime(datespec` [`, utc-flag` ]`)` [¶](#index-mktime_0028_0029-function-_0028gawk_0029)
:   Turn datespec into a timestamp in the same form
    as is returned by `systime()`. It is similar to the function of the
    same name in ISO C. The argument, datespec, is a string of the form
    `"YYYY MM DD HH MM SS [DST]"`.
    The string consists of six or seven numbers representing, respectively,
    the full year including century, the month from 1 to 12, the day of the month
    from 1 to 31, the hour of the day from 0 to 23, the minute from 0 to
    59, the second from 0 to 60,[59](#FOOT59)
    and an optional daylight-savings flag.

    The values of these numbers need not be within the ranges specified;
    for example, an hour of −1 means 1 hour before midnight.
    The origin-zero Gregorian calendar is assumed, with year 0 preceding
    year 1 and year −1 preceding year 0.
    If utc-flag is present and is either nonzero or non-null, the time
    is assumed to be in the UTC time zone; otherwise, the
    time is assumed to be in the local time zone.
    If the DST daylight-savings flag is positive, the time is assumed to be
    daylight savings time; if zero, the time is assumed to be standard
    time; and if negative (the default), `mktime()` attempts to determine
    whether daylight savings time is in effect for the specified time.

    If datespec does not contain enough elements or if the resulting time
    is out of range, `mktime()` returns −1.

`strftime(`[format [`,` timestamp [`,` utc-flag] ] ]`)` [¶](#index-strftime_0028_0029-function-_0028gawk_0029)
:   Format the time specified by timestamp
    based on the contents of the format string and return the result.
    It is similar to the function of the same name in ISO C.
    If utc-flag is present and is either nonzero or non-null, the value
    is formatted as UTC (Coordinated Universal Time, formerly GMT or Greenwich
    Mean Time). Otherwise, the value is formatted for the local time zone.
    The timestamp is in the same format as the value returned by the
    `systime()` function. If no timestamp argument is supplied,
    `gawk` uses the current time of day as the timestamp.
    Without a format argument, `strftime()` uses
    the value of `PROCINFO["strftime"]` as the format string
    (see [Predefined Variables](#Built_002din-Variables)).
    The default string value is
    `"%a %b %e %H:%M:%S %Z %Y"`. This format string produces
    output that is equivalent to that of the `date` utility.
    You can assign a new value to `PROCINFO["strftime"]` to
    change the default format; see the following list for the various format directives.

`systime()` [¶](#index-systime_0028_0029-function-_0028gawk_0029)
:   Return the current time as the number of seconds since
    the system epoch. On POSIX systems, this is the number of seconds
    since 1970-01-01 00:00:00 UTC, not counting leap seconds.
    It may be a different number on other systems.

The `systime()` function allows you to compare a timestamp from a
log file with the current time of day. In particular, it is easy to
determine how long ago a particular record was logged. It also allows
you to produce log records using the “seconds since the epoch” format.

The `mktime()` function allows you to convert a textual representation
of a date and time into a timestamp. This makes it easy to do before/after
comparisons of dates and times, particularly when dealing with date and
time data coming from an external source, such as a log file.

The `strftime()` function allows you to easily turn a timestamp
into human-readable information. It is similar in nature to the `sprintf()`
function
(see [String-Manipulation Functions](#String-Functions)),
in that it copies nonformat specification characters verbatim to the
returned string, while substituting date and time values for format
specifications in the format string.

`strftime()` is guaranteed by the 1999 ISO C
standard[60](#FOOT60)
to support the following date format specifications:

`%a`
:   The locale’s abbreviated weekday name.

`%A`
:   The locale’s full weekday name.

`%b`
:   The locale’s abbreviated month name.

`%B`
:   The locale’s full month name.

`%c`
:   The locale’s “appropriate” date and time representation.
    (This is ‘`%A %B %d %T %Y`’ in the `"C"` locale.)

`%C`
:   The century part of the current year.
    This is the year divided by 100 and truncated to the next
    lower integer.

`%d`
:   The day of the month as a decimal number (01–31).

`%D`
:   Equivalent to specifying ‘`%m/%d/%y`’.

`%e`
:   The day of the month, padded with a space if it is only one digit.

`%F`
:   Equivalent to specifying ‘`%Y-%m-%d`’.
    This is the ISO 8601 date format.

`%g`
:   The year modulo 100 of the ISO 8601 week number, as a decimal number (00–99).
    For example, January 1, 2012, is in week 53 of 2011. Thus, the year
    of its ISO 8601 week number is 2011, even though its year is 2012.
    Similarly, December 31, 2012, is in week 1 of 2013. Thus, the year
    of its ISO week number is 2013, even though its year is 2012.

`%G`
:   The full year of the ISO week number, as a decimal number.

`%h`
:   Equivalent to ‘`%b`’.

`%H`
:   The hour (24-hour clock) as a decimal number (00–23).

`%I`
:   The hour (12-hour clock) as a decimal number (01–12).

`%j`
:   The day of the year as a decimal number (001–366).

`%m`
:   The month as a decimal number (01–12).

`%M`
:   The minute as a decimal number (00–59).

`%n`
:   A newline character (ASCII LF).

`%p`
:   The locale’s equivalent of the AM/PM designations associated
    with a 12-hour clock.

`%r`
:   The locale’s 12-hour clock time.
    (This is ‘`%I:%M:%S %p`’ in the `"C"` locale.)

`%R`
:   Equivalent to specifying ‘`%H:%M`’.

`%S`
:   The second as a decimal number (00–60).

`%t`
:   A TAB character.

`%T`
:   Equivalent to specifying ‘`%H:%M:%S`’.

`%u`
:   The weekday as a decimal number (1–7). Monday is day one.

`%U`
:   The week number of the year (with the first Sunday as the first day of week one)
    as a decimal number (00–53).

`%V`
:   The week number of the year (with the first Monday as the first
    day of week one) as a decimal number (01–53).
    The method for determining the week number is as specified by ISO 8601.
    (To wit: if the week containing January 1 has four or more days in the
    new year, then it is week one; otherwise it is the last week
    [52 or 53] of the previous year and the next week is week one.)

`%w`
:   The weekday as a decimal number (0–6). Sunday is day zero.

`%W`
:   The week number of the year (with the first Monday as the first day of week one)
    as a decimal number (00–53).

`%x`
:   The locale’s “appropriate” date representation.
    (This is ‘`%A %B %d %Y`’ in the `"C"` locale.)

`%X`
:   The locale’s “appropriate” time representation.
    (This is ‘`%T`’ in the `"C"` locale.)

`%y`
:   The year modulo 100 as a decimal number (00–99).

`%Y`
:   The full year as a decimal number (e.g., 2015).

`%z`
:   The time zone offset in a ‘`+HHMM`’ format (e.g., the format
    necessary to produce RFC 822/RFC 1036 date headers).

`%Z`
:   The time zone name or abbreviation; no characters if
    no time zone is determinable.

`%Ec %EC %Ex %EX %Ey %EY %Od %Oe %OH`

`%OI %Om %OM %OS %Ou %OU %OV %Ow %OW %Oy`
:   “Alternative representations” for the specifications
    that use only the second letter (‘`%c`’, ‘`%C`’,
    and so on).[61](#FOOT61)
    (These facilitate compliance with the POSIX `date` utility.)

`%%`
:   A literal ‘`%`’.

If a conversion specifier is not one of those just listed, the behavior is
undefined.[62](#FOOT62)

For systems that are not yet fully standards-compliant,
`gawk` supplies a copy of
`strftime()` from the GNU C Library.
It supports all of the just-listed format specifications.
If that version is
used to compile `gawk` (see [Installing `gawk`](#Installation)),
then the following additional format specifications are available:

`%k`
:   The hour (24-hour clock) as a decimal number (0–23).
    Single-digit numbers are padded with a space.

`%l`
:   The hour (12-hour clock) as a decimal number (1–12).
    Single-digit numbers are padded with a space.

`%s`
:   The time as a decimal timestamp in seconds since the epoch.

Additionally, the alternative representations are recognized but their
normal representations are used.

> **NOTE:** Similar to `printf()`, some versions of `strftime()` support
> the use of flags between the `%` and the format specification
> letter. Check your local man page for `strftime()` to see if it
> supports flags or not, and what they are. Be aware, however, that using
> any such flags is likely to make your script less portable to other systems.

The following example is an `awk` implementation of the POSIX
`date` utility. Normally, the `date` utility prints the
current date and time of day in a well-known format. However, if you
provide an argument to it that begins with a ‘`+`’, `date`
copies nonformat specifier characters to the standard output and
interprets the current time according to the format specifiers in
the string. For example:

```
$ date '+Today is %A, %B %d, %Y.'
-| Today is Monday, September 22, 2014.
```

Here is the `gawk` version of the `date` utility.
It has a shell “wrapper” to handle the `-u` option,
which requires that `date` run as if the time zone
is set to UTC:

```
#! /bin/sh
#
# date --- approximate the POSIX 'date' command

case $1 in
-u)  TZ=UTC0     # use UTC
     export TZ
     shift ;;
esac

gawk 'BEGIN  {
    format = PROCINFO["strftime"]
    exitval = 0

    if (ARGC > 2)
        exitval = 1
    else if (ARGC == 2) {
        format = ARGV[1]
        if (format ~ /^\+/)
            format = substr(format, 2)   # remove leading +
    }
    print strftime(format)
    exit exitval
}' "$@"
```

This script was written before the `strftime()` function acquired
its third argument, utc-flag. Consider how you might modify the
program to work entirely in `awk` and process the `-u` option
for printing the time in UTC.

---

Next: [Getting Type Information](#Type-Functions), Previous: [Time Functions](#Time-Functions), Up: [Built-in Functions](#Built_002din)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.7 Bit-Manipulation Functions [¶](#Bit_002dManipulation-Functions)

> *I can explain it for you, but I can’t understand it for you.*

— *Anonymous*

Many languages provide the ability to perform *bitwise* operations
on two integer numbers. In other words, the operation is performed on
each successive pair of bits in the operands.
Three common operations are bitwise AND, OR, and XOR.
The operations are described in [Table 9.6](#table_002dbitwise_002dops).

```
                Bit operator
          |  AND  |   OR  |  XOR
          |---+---+---+---+---+---
Operands  | 0 | 1 | 0 | 1 | 0 | 1
----------+---+---+---+---+---+---
    0     | 0   0 | 0   1 | 0   1
    1     | 0   1 | 1   1 | 1   0
```

**Table 9.6:** Bitwise operations

As you can see, the result of an AND operation is 1 only when *both*
bits are 1.
The result of an OR operation is 1 if *either* bit is 1.
The result of an XOR operation is 1 if either bit is 1,
but not both.
The next operation is the *complement*; the complement of 1 is 0 and
the complement of 0 is 1. Thus, this operation “flips” all the bits
of a given value.

Finally, two other common operations are to shift the bits left or right.
For example, if you have a bit string ‘`10111001`’ and you shift it
right by three bits, you end up with ‘`00010111`’.[63](#FOOT63)
If you start over again with ‘`10111001`’ and shift it left by three
bits, you end up with ‘`11001000`’. The following list describes
`gawk`’s built-in functions that implement the bitwise operations.
Optional parameters are enclosed in square brackets ([ ]):

`and(`v1`,` v2 [`,` …]`)`
:   Return the bitwise AND of the arguments. There must be at least two.

`compl(val)`
:   Return the bitwise complement of val.

`lshift(val, count)` [¶](#index-lshift_0028_0029-function-_0028gawk_0029)
:   Return the value of val, shifted left by count bits.

`or(`v1`,` v2 [`,` …]`)`
:   Return the bitwise OR of the arguments. There must be at least two.

`rshift(val, count)` [¶](#index-rshift_0028_0029-function-_0028gawk_0029)
:   Return the value of val, shifted right by count bits.

`xor(`v1`,` v2 [`,` …]`)`
:   Return the bitwise XOR of the arguments. There must be at least two.

> **CAUTION:** Beginning with `gawk` version 4.2, negative
> operands are not allowed for any of these functions. A negative
> operand produces a fatal error. See the sidebar
> “Beware The Smoke and Mirrors!” for more information as to why.
>
> Beginning with `gawk` version 5.4, if not in MPFR mode
> (see [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic)), shifting with `lshift()`
> or `rshift()` by as many or more bits than are available in the
> largest unsigned integer returns zero, instead of whatever the underlying
> C compiler might do.

Here is a user-defined function (see [User-Defined Functions](#User_002ddefined))
that illustrates the use of these functions:

```
# bits2str --- turn an integer into readable ones and zeros

function bits2str(bits,        data, mask)
{
    if (bits == 0)
        return "0"

    mask = 1
    for (; bits != 0; bits = rshift(bits, 1))
        data = (and(bits, mask) ? "1" : "0") data

    while ((length(data) % 8) != 0)
        data = "0" data

    return data
}
```

```
BEGIN {
    printf "123 = %s\n", bits2str(123)
    printf "0123 = %s\n", bits2str(0123)
    printf "0x99 = %s\n", bits2str(0x99)
    comp = compl(0x99)
    printf "compl(0x99) = %#x = %s\n", comp, bits2str(comp)
    shift = lshift(0x99, 2)
    printf "lshift(0x99, 2) = %#x = %s\n", shift, bits2str(shift)
    shift = rshift(0x99, 2)
    printf "rshift(0x99, 2) = %#x = %s\n", shift, bits2str(shift)
}
```

This program produces the following output when run:

```
$ gawk -f testbits.awk
-| 123 = 01111011
-| 0123 = 01010011
-| 0x99 = 10011001
-| compl(0x99) = 0x3fffffffffff66 =
-| 00111111111111111111111111111111111111111111111101100110
-| lshift(0x99, 2) = 0x264 = 0000001001100100
-| rshift(0x99, 2) = 0x26 = 00100110
```

The `bits2str()` function turns a binary number into a string.
Initializing `mask` to one creates
a binary value where the rightmost bit
is set to one. Using this mask,
the function repeatedly checks the rightmost bit.
ANDing the mask with the value indicates whether the
rightmost bit is one or not. If so, a `"1"` is concatenated onto the front
of the string.
Otherwise, a `"0"` is added.
The value is then shifted right by one bit and the loop continues
until there are no more one bits.

If the initial value is zero, it returns a simple `"0"`.
Otherwise, at the end, it pads the value with zeros to represent multiples
of 8-bit quantities. This is typical in modern computers.

The main code in the `BEGIN` rule shows the difference between the
decimal and octal values for the same numbers
(see [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers)),
and then demonstrates the
results of the `compl()`, `lshift()`, and `rshift()` functions.

| Beware The Smoke and Mirrors! |
| --- |
| In other languages, bitwise operations are performed on integer values, not floating-point values. As a general statement, such operations work best when performed on unsigned integers.  `gawk` attempts to treat the arguments to the bitwise functions as unsigned integers. For this reason, negative arguments produce a fatal error.  In normal operation, for all of these functions, first the double-precision floating-point value is converted to the widest C unsigned integer type, then the bitwise operation is performed. If the result cannot be represented exactly as a C `double`, leading nonzero bits are removed one by one until it can be represented exactly. The result is then converted back into a C `double`.[64](#FOOT64)  However, when using arbitrary precision arithmetic with the `-M` option (see [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic)), the results may differ. This is particularly noticeable with the `compl()` function:  ``` $ gawk 'BEGIN { print compl(42) }' -| 9007199254740949 $ gawk -M 'BEGIN { print compl(42) }' -| -43 ```  What’s going on becomes clear when printing the results in hexadecimal:  ``` $ gawk 'BEGIN { printf "%#x\n", compl(42) }' -| 0x1fffffffffffd5 $ gawk -M 'BEGIN { printf "%#x\n", compl(42) }' -| 0xffffffffffffffd5 ```  When using the `-M` option, under the hood, `gawk` uses GNU MP arbitrary precision integers which have at least 64 bits of precision. When not using `-M`, `gawk` stores integral values in regular double-precision floating point, which only maintain 53 bits of precision. Furthermore, the GNU MP library treats (or at least seems to treat) the leading bit as a sign bit; thus the result with `-M` in this case is a negative number.  In short, using `gawk` for any but the simplest kind of bitwise operations is probably a bad idea; caveat emptor! |

---

Next: [String-Translation Functions](#I18N-Functions), Previous: [Bit-Manipulation Functions](#Bitwise-Functions), Up: [Built-in Functions](#Built_002din)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.8 Getting Type Information [¶](#Getting-Type-Information)

`gawk` provides two functions that let you distinguish
the type of a variable.
This is necessary for writing code
that traverses every element of an array of arrays
(see [Arrays of Arrays](#Arrays-of-Arrays)), and in other contexts.

`isarray(x)` [¶](#index-isarray_0028_0029-function-_0028gawk_0029)
:   Return a true value if x is an array. Otherwise, return false.

`typeof(x)`
:   Return one of the following strings, depending upon the type of x:

    `"array"`
    :   x is an array.

    `"regexp"`
    :   x is a strongly typed regexp (see [Strongly Typed Regexp Constants](#Strong-Regexp-Constants)).

    `"number"`
    :   x is a number.

    `"number|bool"`
    :   x is a Boolean typed value (see [Boolean Typed Values](#Boolean-Typed-Values)).

    `"string"`
    :   x is a string.

    `"strnum"`
    :   x is a number that started life as user input, such as a field or
        the result of calling `split()`. (I.e., x has the strnum
        attribute; see [String Type versus Numeric Type](#Variable-Typing).)

    `"unassigned"`
    :   x is a *scalar* variable that has not been assigned a value yet.

    `"untyped"`
    :   x has not yet been used yet at all; it can become a scalar or an
        array.

`isarray()` is meant for use in two circumstances. The first is when
traversing a multidimensional array: you can test if an element is itself
an array or not. The second is inside the body of a user-defined function
(not discussed yet; see [User-Defined Functions](#User_002ddefined)), to test if a parameter is an
array or not.

> **NOTE:** While you can use `isarray()` at the global level to test variables,
> doing so makes no sense. Because *you* are the one writing the
> program, *you* are supposed to know if your variables are arrays
> or not.

The `typeof()` function is general; it allows you to determine
if a variable or function parameter is a scalar (number, string,
or strongly typed regexp) or an array.

We delay further discussion of `unassigned` vs. `untyped`
to [Dynamic Typing In `gawk`](#Dynamic-Typing-Gawk).

---

Previous: [Getting Type Information](#Type-Functions), Up: [Built-in Functions](#Built_002din)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.1.9 String-Translation Functions [¶](#String_002dTranslation-Functions)

`gawk` provides facilities for internationalizing `awk` programs.
These include the functions described in the following list.
The descriptions here are purposely brief.
See [Internationalization with `gawk`](#Internationalization),
for the full story.
Optional parameters are enclosed in square brackets ([ ]):

`bindtextdomain(directory` [`,` domain]`)` [¶](#index-bindtextdomain_0028_0029-function-_0028gawk_0029)
:   Set the directory in which
    `gawk` will look for message translation files, in case they
    will not or cannot be placed in the “standard” locations
    (e.g., during testing).
    It returns the directory in which domain is “bound.”

    The default domain is the value of `TEXTDOMAIN`.
    If directory is the null string (`""`), then
    `bindtextdomain()` returns the current binding for the
    given domain.

`dcgettext(string` [`,` domain [`,` category] ]`)` [¶](#index-dcgettext_0028_0029-function-_0028gawk_0029)
:   Return the translation of string in
    text domain domain for locale category category.
    The default value for domain is the current value of `TEXTDOMAIN`.
    The default value for category is `"LC_MESSAGES"`.

`dcngettext(string1, string2, number` [`,` domain [`,` category] ]`)` [¶](#index-dcngettext_0028_0029-function-_0028gawk_0029)
:   Return the plural form used for number of the
    translation of string1 and string2 in text domain
    domain for locale category category. string1 is the
    English singular variant of a message, and string2 is the English plural
    variant of the same message.
    The default value for domain is the current value of `TEXTDOMAIN`.
    The default value for category is `"LC_MESSAGES"`.

---

Next: [Variable Typing Is Dynamic](#Dynamic-Typing), Previous: [Built-in Functions](#Built_002din), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
