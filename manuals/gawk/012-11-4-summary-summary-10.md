### 11.4 Summary [¶](#Summary-10)

* The programs provided in this chapter
  continue on the theme that reading programs is an excellent way to learn
  Good Programming.
* Using ‘`#!`’ to make `awk` programs directly runnable makes
  them easier to use. Otherwise, invoke the program using ‘`awk
  -f …`’.
* Reimplementing standard POSIX programs in `awk` is a pleasant
  exercise; `awk`’s expressive power lets you write such programs
  in relatively few lines of code, yet they are functionally complete
  and usable.
* One of standard `awk`’s weaknesses is working with individual
  characters. The ability to use `split()` with the empty string as
  the separator can considerably simplify such tasks.
* The examples here demonstrate the usefulness of the library
  functions from [A Library of `awk` Functions](#Library-Functions)
  for a number of real (if small) programs.
* Besides reinventing POSIX wheels, other programs solved a selection of
  interesting problems, such as finding duplicate words in text, printing
  mailing labels, and finding anagrams.

---

Previous: [Summary](#Programs-Summary), Up: [Practical `awk` Programs](#Sample-Programs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.5 Exercises [¶](#Exercises-3)

1. Rewrite `cut.awk` (see [Cutting Out Fields and Columns](#Cut-Program))
   using `split()` with `""` as the separator.
2. In [Searching for Regular Expressions in Files](#Egrep-Program), we mentioned that ‘`egrep -i`’ could be
   simulated in versions of `awk` without `IGNORECASE` by
   using `tolower()` on the line and the pattern. In a footnote there,
   we also mentioned that this solution has a bug: the translated line is
   output, and not the original one. Fix this problem.
3. POSIX versions of `grep` accept a `-F` option which
   causes `grep` to match fixed strings. Add support for
   this to `egrep.awk`.
4. Similarly, POSIX versions of `grep` allow you to
   provide multiple patterns to match, in either of two ways.
   You may provide a quoted string on the command line where
   the patterns are separated by newlines. Or you may use the
   `-f` option to provide a file containing the patterns,
   one per line.
   Implement both of these features.
5. The POSIX version of `id` takes options that control which
   information is printed. Modify the `awk` version
   (see [Printing Out User Information](#Id-Program)) to accept the same arguments and perform in the
   same way.
6. The `split.awk` program (see [Splitting a Large File into Pieces](#Split-Program)) assumes
   that letters are contiguous in the character set,
   which isn’t true for EBCDIC systems.
   Fix this problem.
   (Hint: Consider a different way to work through the alphabet,
   without relying on `ord()` and `chr()`.)
7. In `uniq.awk` (see [Printing Nonduplicated Lines of Text](#Uniq-Program), the
   logic for choosing which lines to print represents a *state
   machine*, which is “a device which can be in one of a set number of stable
   conditions depending on its previous condition and on the present values
   of its inputs.”[85](#FOOT85)
   Brian Kernighan suggests that
   “an alternative approach to state machines is to just read
   the input into an array, then use indexing. It’s almost always
   easier code, and for most inputs where you would use this, just
   as fast.” Rewrite the logic to follow this
   suggestion.
8. Why can’t the `wc.awk` program (see [Counting Things](#Wc-Program)) just
   use the value of `FNR` in `endfile()`?
   Hint: Examine the code in [Noting Data file Boundaries](#Filetrans-Function).
9. Manipulation of individual characters in the `translate` program
   (see [Transliterating Characters](#Translate-Program)) is painful using standard `awk`
   functions. Given that `gawk` can split strings into individual
   characters using `""` as the separator, how might you use this
   feature to simplify the program?
10. The `extract.awk` program (see [Extracting Programs from Texinfo Source Files](#Extract-Program)) was written
    before `gawk` had the `gensub()` function. Use it
    to simplify the code.
11. Compare the performance of the `awksed.awk` program
    (see [A Simple Stream Editor](#Simple-Sed)) with the more straightforward:

    ```
    BEGIN {
        pat = ARGV[1]
        repl = ARGV[2]
        ARGV[1] = ARGV[2] = ""
    }

    { gsub(pat, repl); print }
    ```
12. What are the advantages and disadvantages of `awksed.awk` versus
    the real `sed` utility?
13. In [An Easy Way to Use Library Functions](#Igawk-Program), we mentioned that not trying to save the line
    read with `getline` in the `pathto()` function when testing
    for the file’s accessibility for use with the main program simplifies
    things considerably. What problem does this engender though?
14. As an additional example of the idea that it is not always necessary to
    add new features to a program, consider the idea of having two files in
    a directory in the search path:

    `default.awk`
    :   This file contains a set of default library functions, such
        as `getopt()` and `assert()`.

    `site.awk`
    :   This file contains library functions that are specific to a site or
        installation; i.e., locally developed functions.
        Having a separate file allows `default.awk` to change with
        new `gawk` releases, without requiring the system administrator to
        update it each time by adding the local functions.

    One user
    suggested that `gawk` be modified to automatically read these files
    upon startup. Instead, it would be very simple to modify `igawk`
    to do this. Since `igawk` can process nested `@include`
    directives, `default.awk` could simply contain `@include`
    directives for the desired library functions.
    Make this change.
15. Modify `anagram.awk` (see [Finding Anagrams from a Dictionary](#Anagram-Program)), to avoid
    the use of the external `sort` utility.

---

Next: [Internationalization with `gawk`](#Internationalization), Previous: [Practical `awk` Programs](#Sample-Programs), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]# Part III: Moving Beyond Standard `awk` with `gawk` [¶](#Part-III_003a-Moving-Beyond-Standard-awk-with-gawk)

---

* [Advanced Features of `gawk`](#Advanced-Features)
* [Internationalization with `gawk`](#Internationalization)
* [Debugging `awk` Programs](#Debugger)
* [Namespaces in `gawk`](#Namespaces)
* [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic)
* [Writing Extensions for `gawk`](#Dynamic-Extensions)## 12 Advanced Features of `gawk` [¶](#Advanced-Features-of-gawk)

> *Write documentation as if whoever reads it is
> a violent psychopath who knows where you live.*

— *Steve English, as quoted by Peter Langston*

This chapter discusses advanced features in `gawk`.
It’s a bit of a “grab bag” of items that are otherwise unrelated
to each other.
First, we look at a command-line option that allows `gawk` to recognize
nondecimal numbers in input data, not just in `awk`
programs.
Then, `gawk`’s special features for sorting arrays are presented.
Next, two-way I/O, discussed briefly in earlier parts of this
Web page, is described in full detail, along with the basics
of TCP/IP networking. We then see how `gawk`
can *profile* an `awk` program, making it possible to tune
it for performance.
Next, we present an experimental feature that allows you to preserve
the values of `awk` variables and arrays between runs of `gawk`.
Finally, we discuss the philosophy behind `gawk`’s extension
mechanism.

Additional advanced features are discussed in separate chapters of their
own:

* [Internationalization with `gawk`](#Internationalization), discusses how to internationalize
  your `awk` programs, so that they can speak multiple
  national languages.
* [Debugging `awk` Programs](#Debugger), describes `gawk`’s built-in command-line
  debugger for debugging `awk` programs.
* [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic), describes how you can use
  `gawk` to perform arbitrary-precision arithmetic.
* [Writing Extensions for `gawk`](#Dynamic-Extensions),
  discusses the ability to dynamically add new built-in functions to
  `gawk`.

* [Allowing Nondecimal Input Data](#Nondecimal-Data)
* [Boolean Typed Values](#Boolean-Typed-Values)
* [Controlling Array Traversal and Array Sorting](#Array-Sorting)
* [Two-Way Communications with Another Process](#Two_002dway-I_002fO)
* [Using `gawk` for Network Programming](#TCP_002fIP-Networking)
* [Profiling Your `awk` Programs](#Profiling)
* [Preserving Data Between Runs](#Persistent-Memory)
* [Builtin Features versus Extensions](#Extension-Philosophy)
* [Summary](#Advanced-Features-Summary)

---

Next: [Boolean Typed Values](#Boolean-Typed-Values), Up: [Advanced Features of `gawk`](#Advanced-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.1 Allowing Nondecimal Input Data [¶](#Allowing-Nondecimal-Input-Data)

If you run `gawk` with the `--non-decimal-data` option,
you can have nondecimal values in your input data:

```
$ echo 0123 123 0x123 |
> gawk --non-decimal-data '{ printf "%d, %d, %d\n", $1, $2, $3 }'
-| 83, 123, 291
```

For this feature to work, write your program so that
`gawk` treats your data as numeric:

```
$ echo 0123 123 0x123 | gawk '{ print $1, $2, $3 }'
-| 0123 123 0x123
```

The `print` statement treats its expressions as strings.
Although the fields can act as numbers when necessary,
they are still strings, so `print` does not try to treat them
numerically. You need to add zero to a field to force it to
be treated as a number. For example:

```
$ echo 0123 123 0x123 | gawk --non-decimal-data '
> { print $1, $2, $3
>   print $1 + 0, $2 + 0, $3 + 0 }'
-| 0123 123 0x123
-| 83 123 291
```

In [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers), we described how octal and hexadecimal values
may be used in program source code to represent numeric values.
That section also discussed hexadecimal floating-point values.

As of version 5.4 of `gawk`, the `--non-decimal-data` option and
the `strtonum()` function accept such values and convert them correctly.
For example:

```
$ echo 0x1p-3 | ./gawk -n '{ print $1 + 0 }'
-| 0.125
```

Because it is common to have decimal data with leading zeros, and because
allowing nondecimal data could lead to surprising results, the default is to leave it
disabled. If you want it, you must explicitly request it.

> **CAUTION:** *Use of this option is not recommended.*
> It can break old programs very badly.
> Instead, use the `strtonum()` function to convert your data
> (see [String-Manipulation Functions](#String-Functions)).
> This makes your programs easier to write and easier to read, and
> leads to less surprising results.

---

Next: [Controlling Array Traversal and Array Sorting](#Array-Sorting), Previous: [Allowing Nondecimal Input Data](#Nondecimal-Data), Up: [Advanced Features of `gawk`](#Advanced-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.2 Boolean Typed Values [¶](#Boolean-Typed-Values-1)

Scalar values in `awk` are either numbers or strings.
`gawk` also supports values of type `regexp`
(see [Strongly Typed Regexp Constants](#Strong-Regexp-Constants)).

As described in [True and False in `awk`](#Truth-Values), Boolean values in `awk`
don’t have a separate type: a value counts as “true” if it is nonzero
or non-null, and as “false” otherwise.

When interchanging data with languages that do have a real Boolean type,
using a standard format such as JSON or XML, the lack of a true Boolean
type in `awk` is problematic.
(See, for example, the `json` extension provided by
[the `gawkextlib` project](https://sourceforge.net/projects/gawkextlib).)

It’s easy to import Boolean data into `awk`, but then the fact
that it was originally Boolean is lost. Exporting data is even harder;
there’s no way to indicate that a value is really Boolean.

To solve this problem, `gawk` provides a function named `mkbool()`.
It takes one argument, which is any `awk` expression, and it
returns a value of Boolean type.

The returned values are normal `awk` numeric values, with
values of either one or zero,
depending upon the truth
value of the original expression passed in the call to `mkbool()`.

The `typeof()` function (see [Getting Type Information](#Type-Functions)) returns
`"number|bool"` for these values.

Thus Boolean-typed values *are* numbers as far as `gawk`
is concerned, except that extension code can treat them as Booleans
if desired.

While it would have been possible to add two new built-in variables
of Boolean type named `TRUE` and `FALSE`, doing so would
undoubtedly have broken many existing `awk` programs. Instead,
having a “generator” function that creates Boolean values gives
flexibility, without breaking as much existing code.

---

Next: [Two-Way Communications with Another Process](#Two_002dway-I_002fO), Previous: [Boolean Typed Values](#Boolean-Typed-Values), Up: [Advanced Features of `gawk`](#Advanced-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.3 Controlling Array Traversal and Array Sorting [¶](#Controlling-Array-Traversal-and-Array-Sorting)

`gawk` lets you control the order in which a
‘`for (indx in array)`’
loop traverses an array.

In addition, two built-in functions, `asort()` and `asorti()`,
let you sort arrays based on the array values and indices, respectively.
These two functions also provide control over the sorting criteria used
to order the elements during sorting.

* [Controlling Array Traversal](#Controlling-Array-Traversal)
* [Sorting Array Values and Indices with `gawk`](#Array-Sorting-Functions)

---

Next: [Sorting Array Values and Indices with `gawk`](#Array-Sorting-Functions), Up: [Controlling Array Traversal and Array Sorting](#Array-Sorting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.3.1 Controlling Array Traversal [¶](#Controlling-Array-Traversal-1)

By default, the order in which a ‘`for (indx in array)`’ loop
scans an array is not defined; it is generally based upon
the internal implementation of arrays inside `awk`.

Often, though, it is desirable to be able to loop over the elements
in a particular order that you, the programmer, choose. `gawk`
lets you do this.

[Using Predefined Array Scanning Orders with `gawk`](#Controlling-Scanning) describes how you can assign special,
predefined values to `PROCINFO["sorted_in"]` in order to
control the order in which `gawk` traverses an array
during a `for` loop.

In addition, the value of `PROCINFO["sorted_in"]` can be a
function name.[86](#FOOT86)
This lets you traverse an array based on any custom criterion.
The array elements are ordered according to the return value of this
function. The comparison function should be defined with at least
four arguments:

```
function comp_func(i1, v1, i2, v2)
{
    compare elements 1 and 2 in some fashion
    return < 0; 0; or > 0
}
```

Here, `i1` and `i2` are the indices, and `v1` and `v2`
are the corresponding values of the two elements being compared.
Either `v1` or `v2`, or both, can be arrays if the array being
traversed contains subarrays as values.
(See [Arrays of Arrays](#Arrays-of-Arrays) for more information about subarrays.)
The three possible return values are interpreted as follows:

`comp_func(i1, v1, i2, v2) < 0`
:   Index `i1` comes before index `i2` during loop traversal.

`comp_func(i1, v1, i2, v2) == 0`
:   Indices `i1` and `i2`
    come together, but the relative order with respect to each other is undefined.

`comp_func(i1, v1, i2, v2) > 0`
:   Index `i1` comes after index `i2` during loop traversal.

Our first comparison function can be used to scan an array in
numerical order of the indices:

```
function cmp_num_idx(i1, v1, i2, v2)
{
     # numerical index comparison, ascending order
     return (i1 - i2)
}
```

Our second function traverses an array based on the string order of
the element values rather than by indices:

```
function cmp_str_val(i1, v1, i2, v2)
{
    # string value comparison, ascending order
    v1 = v1 ""
    v2 = v2 ""
    if (v1 < v2)
        return -1
    return (v1 != v2)
}
```

The third
comparison function makes all numbers, and numeric strings without
any leading or trailing spaces, come out first during loop traversal:

```
function cmp_num_str_val(i1, v1, i2, v2,   n1, n2)
{
     # numbers before string value comparison, ascending order
     n1 = v1 + 0
     n2 = v2 + 0
     if (n1 == v1)
         return (n2 == v2) ? (n1 - n2) : -1
     else if (n2 == v2)
         return 1
     return (v1 < v2) ? -1 : (v1 != v2)
}
```

Here is a main program to demonstrate how `gawk`
behaves using each of the previous functions:

```
BEGIN {
    data["one"] = 10
    data["two"] = 20
    data[10] = "one"
    data[100] = 100
    data[20] = "two"

    f[1] = "cmp_num_idx"
    f[2] = "cmp_str_val"
    f[3] = "cmp_num_str_val"
    for (i = 1; i <= 3; i++) {
        printf("Sort function: %s\n", f[i])
        PROCINFO["sorted_in"] = f[i]
        for (j in data)
            printf("\tdata[%s] = %s\n", j, data[j])
        print ""
    }
}
```

Here are the results when the program is run:

```
$ gawk -f compdemo.awk
-| Sort function: cmp_num_idx      Sort by numeric index
-|     data[two] = 20
-|     data[one] = 10              Both strings are numerically zero
-|     data[10] = one
-|     data[20] = two
-|     data[100] = 100
-|
-| Sort function: cmp_str_val      Sort by element values as strings
-|     data[one] = 10
-|     data[100] = 100             String 100 is less than string 20
-|     data[two] = 20
-|     data[10] = one
-|     data[20] = two
-|
-| Sort function: cmp_num_str_val  Sort all numeric values before all strings
-|     data[one] = 10
-|     data[two] = 20
-|     data[100] = 100
-|     data[10] = one
-|     data[20] = two
```

Consider sorting the entries of a GNU/Linux system password file
according to login name. The following program sorts records
by a specific field position and can be used for this purpose:

```
# passwd-sort.awk --- simple program to sort by field position
# field position is specified by the global variable POS

function cmp_field(i1, v1, i2, v2)
{
    # comparison by value, as string, and ascending order
    return v1[POS] < v2[POS] ? -1 : (v1[POS] != v2[POS])
}

{
    for (i = 1; i <= NF; i++)
        a[NR][i] = $i
}
```

```
END {
    PROCINFO["sorted_in"] = "cmp_field"
```

```
    if (POS < 1 || POS > NF)
        POS = 1

    for (i in a) {
        for (j = 1; j <= NF; j++)
            printf("%s%c", a[i][j], j < NF ? ":" : "")
        print ""
    }
}
```

The first field in each entry of the password file is the user’s login name,
and the fields are separated by colons.
Each record defines a subarray,
with each field as an element in the subarray.
Running the program produces the
following output:

```
$ gawk -v POS=1 -F: -f sort.awk /etc/passwd
-| adm:x:3:4:adm:/var/adm:/sbin/nologin
-| apache:x:48:48:Apache:/var/www:/sbin/nologin
-| avahi:x:70:70:Avahi daemon:/:/sbin/nologin
...
```

The comparison should normally always return the same value when given a
specific pair of array elements as its arguments. If inconsistent
results are returned, then the order is undefined. This behavior can be
exploited to introduce random order into otherwise seemingly
ordered data:

```
function cmp_randomize(i1, v1, i2, v2)
{
    # random order (caution: this may never terminate!)
    return (2 - 4 * rand())
}
```

As already mentioned, the order of the indices is arbitrary if two
elements compare equal. This is usually not a problem, but letting
the tied elements come out in arbitrary order can be an issue, especially
when comparing item values. The partial ordering of the equal elements
may change the next time the array is traversed, if other elements are added to or
removed from the array. One way to resolve ties when comparing elements
with otherwise equal values is to include the indices in the comparison
rules. Note that doing this may make the loop traversal less efficient,
so consider it only if necessary. The following comparison functions
force a deterministic order, and are based on the fact that the
(string) indices of two elements are never equal:

```
function cmp_numeric(i1, v1, i2, v2)
{
    # numerical value (and index) comparison, descending order
    return (v1 != v2) ? (v2 - v1) : (i2 - i1)
}
```

```
function cmp_string(i1, v1, i2, v2)
{
    # string value (and index) comparison, descending order
    v1 = v1 i1
    v2 = v2 i2
    return (v1 > v2) ? -1 : (v1 != v2)
}
```

A custom comparison function can often simplify ordered loop
traversal, and the sky is really the limit when it comes to
designing such a function.

When string comparisons are made during a sort, either for element
values where one or both aren’t numbers, or for element indices
handled as strings, the value of `IGNORECASE`
(see [Predefined Variables](#Built_002din-Variables)) controls whether
the comparisons treat corresponding upper- and lowercase letters as
equivalent or distinct.

Another point to keep in mind is that in the case of subarrays,
the element values can themselves be arrays; a production comparison
function should use the `isarray()` function
(see [Getting Type Information](#Type-Functions))
to check for this, and choose a defined sorting order for subarrays.

All sorting based on `PROCINFO["sorted_in"]`
is disabled in POSIX mode,
because the `PROCINFO` array is not special in that case.

As a side note, sorting the array indices before traversing
the array has been reported to add a 15% to 20% overhead to the
execution time of `awk` programs. For this reason,
sorted array traversal is not the default.

---

Previous: [Controlling Array Traversal](#Controlling-Array-Traversal), Up: [Controlling Array Traversal and Array Sorting](#Array-Sorting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.3.2 Sorting Array Values and Indices with `gawk` [¶](#Sorting-Array-Values-and-Indices-with-gawk)

In most `awk` implementations, sorting an array requires writing
a `sort()` function. This can be educational for exploring
different sorting algorithms, but usually that’s not the point of the program.
`gawk` provides the built-in `asort()` and `asorti()`
functions (see [String-Manipulation Functions](#String-Functions)) for sorting arrays. For example:

```
populate the array data
n = asort(data)
for (i = 1; i <= n; i++)
    do something with data[i]
```

After the call to `asort()`, the array `data` is indexed from 1
to some number n, the total number of elements in `data`.
(This count is `asort()`’s return value.)
`data[1]` <= `data[2]` <= `data[3]`, and so on.
The default comparison is based on the type of the elements
(see [Variable Typing and Comparison Expressions](#Typing-and-Comparison)).
All numeric values come before all string values,
which in turn come before all subarrays.

An important side effect of calling `asort()` is that
*the array’s original indices are irrevocably lost*.
As this isn’t always desirable, `asort()` accepts a
second argument:

```
populate the array source
n = asort(source, dest)
for (i = 1; i <= n; i++)
    do something with dest[i]
```

In this case, `gawk` copies the `source` array into the
`dest` array and then sorts `dest`, destroying its indices.
However, the `source` array is not affected.

Often, what’s needed is to sort on the values of the *indices*
instead of the values of the elements. To do that, use the
`asorti()` function. The interface and behavior are identical to
that of `asort()`, except that the index values are used for sorting
and become the values of the result array:

```
{ source[$0] = some_func($0) }

END {
    n = asorti(source, dest)
    for (i = 1; i <= n; i++) {
        Work with sorted indices directly:
        do something with dest[i]
        ...
        Access original array via sorted indices:
        do something with source[dest[i]]
    }
}
```

So far, so good. Now it starts to get interesting. Both `asort()`
and `asorti()` accept a third string argument to control comparison
of array elements. When we introduced `asort()` and `asorti()`
in [String-Manipulation Functions](#String-Functions), we ignored this third argument; however,
now is the time to describe how this argument affects these two functions.

Basically, the third argument specifies how the array is to be sorted.
There are two possibilities. As with `PROCINFO["sorted_in"]`,
this argument may be one of the predefined names that `gawk`
provides (see [Using Predefined Array Scanning Orders with `gawk`](#Controlling-Scanning)), or it may be the name of a
user-defined function (see [Controlling Array Traversal](#Controlling-Array-Traversal)).

In the latter case, *the function can compare elements in any way
it chooses*, taking into account just the indices, just the values,
or both. This is extremely powerful.

Once the array is sorted, `asort()` takes the *values* in
their final order and uses them to fill in the result array, whereas
`asorti()` takes the *indices* in their final order and uses
them to fill in the result array.

> **NOTE:** Copying array indices and elements isn’t expensive in terms of memory.
> Internally, `gawk` maintains *reference counts* to data.
> For example, when `asort()` copies the first array to the second one,
> there is only one copy of the original array elements’ data, even though
> both arrays use the values.

You may use the same array for both the first and second arguments to
`asort()` and `asorti()`. Doing so only makes sense if you
are also supplying the third argument, since `awk` doesn’t
provide a way to pass that third argument without also passing the first
and second ones.

Because `IGNORECASE` affects string comparisons, the value
of `IGNORECASE` also affects sorting for both `asort()` and `asorti()`.
Note also that the locale’s sorting order does *not*
come into play; comparisons are based on character values only.[87](#FOOT87)

The following example demonstrates the use of a comparison function with
`asort()`. The comparison function, `case_fold_compare()`, maps
both values to lowercase in order to compare them ignoring case.

```
# case_fold_compare --- compare as strings, ignoring case

function case_fold_compare(i1, v1, i2, v2,    l, r)
{
    l = tolower(v1)
```

```
    r = tolower(v2)

    if (l < r)
        return -1
    else if (l == r)
        return 0
    else
        return 1
}
```

And here is the test program for it:

```
# Test program

BEGIN {
    Letters = "abcdefghijklmnopqrstuvwxyz" \
              "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    split(Letters, data, "")

    asort(data, result, "case_fold_compare")

    j = length(result)
    for (i = 1; i <= j; i++) {
        printf("%s", result[i])
        if (i % (j/2) == 0)
            printf("\n")
        else
            printf(" ")
    }
}
```

When run, we get the following:

```
$ gawk -f case_fold_compare.awk
-| A a B b c C D d e E F f g G H h i I J j k K l L M m
-| n N O o p P Q q r R S s t T u U V v w W X x y Y z Z
```

> **NOTE:** “Under the hood,” `gawk` uses the C library `qsort()`
> function to manage the sorting. `qsort()` can call itself
> recursively. This means that when you write a comparison function,
> you should be careful to avoid the use of global variables and arrays;
> use only local variables and arrays that you declare as additional
> parameters to the comparison function. Otherwise, you are likely to
> cause unintentional memory corruption in your global arrays and possibly
> cause `gawk` itself to fail.

---

Next: [Using `gawk` for Network Programming](#TCP_002fIP-Networking), Previous: [Controlling Array Traversal and Array Sorting](#Array-Sorting), Up: [Advanced Features of `gawk`](#Advanced-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.4 Two-Way Communications with Another Process [¶](#Two_002dWay-Communications-with-Another-Process)

It is often useful to be able to
send data to a separate program for
processing and then read the result. This can always be
done with temporary files:

```
# Write the data for processing
tempfile = ("mydata." PROCINFO["pid"])
while (not done with data)
    print data | ("subprogram > " tempfile)
close("subprogram > " tempfile)

# Read the results, remove tempfile when done
while ((getline newdata < tempfile) > 0)
    process newdata appropriately
close(tempfile)
system("rm " tempfile)
```

This works, but not elegantly. Among other things, it requires that
the program be run in a directory that cannot be shared among users;
for example, `/tmp` will not do, as another user might happen
to be using a temporary file with the same name.[88](#FOOT88)

However, with `gawk`, it is possible to
open a *two-way* pipe to another process. The second process is
termed a *coprocess*, as it runs in parallel with `gawk`.
The two-way connection is created using the ‘`|&`’ operator
(borrowed from the Korn shell, `ksh`):[89](#FOOT89)

```
do {
    print data |& "subprogram"
    "subprogram" |& getline results
} while (data left to process)
close("subprogram")
```

The first time an I/O operation is executed using the ‘`|&`’
operator, `gawk` creates a two-way pipeline to a child process
that runs the other program. Output created with `print`
or `printf` is written to the program’s standard input, and
output from the program’s standard output can be read by the `gawk`
program using `getline`.
As is the case with processes started by ‘`|`’, the subprogram
can be any program, or pipeline of programs, that can be started by
the shell.

There are some cautionary items to be aware of:

* As the code inside `gawk` currently stands, the coprocess’s
  standard error goes to the same place that the parent `gawk`’s
  standard error goes. It is not possible to read the child’s
  standard error separately.
* I/O buffering may be a problem. `gawk` automatically
  flushes all output down the pipe to the coprocess.
  However, if the coprocess does not flush its output,
  `gawk` may hang when doing a `getline` in order to read
  the coprocess’s results. This could lead to a situation
  known as *deadlock*, where each process is waiting for the
  other one to do something.

It is possible to close just one end of the two-way pipe to
a coprocess, by supplying a second argument to the `close()`
function of either `"to"` or `"from"`
(see [Closing Input and Output Redirections](#Close-Files-And-Pipes)).
These strings tell `gawk` to close the end of the pipe
that sends data to the coprocess or the end that reads from it,
respectively.

This is particularly necessary in order to use
the system `sort` utility as part of a coprocess;
`sort` must read *all* of its input
data before it can produce any output.
The `sort` program does not receive an end-of-file indication
until `gawk` closes the write end of the pipe.

When you have finished writing data to the `sort`
utility, you can close the `"to"` end of the pipe, and
then start reading sorted data via `getline`.
For example:

```
BEGIN {
    command = "LC_ALL=C sort"
    n = split("abcdefghijklmnopqrstuvwxyz", a, "")

    for (i = n; i > 0; i--)
        print a[i] |& command
    close(command, "to")

    while ((command |& getline line) > 0)
        print "got", line
    close(command)
}
```

This program writes the letters of the alphabet in reverse order, one
per line, down the two-way pipe to `sort`. It then closes the
write end of the pipe, so that `sort` receives an end-of-file
indication. This causes `sort` to sort the data and write the
sorted data back to the `gawk` program. Once all of the data
has been read, `gawk` terminates the coprocess and exits.

As a side note, the assignment ‘`LC_ALL=C`’ in the `sort`
command ensures traditional Unix (ASCII) sorting from `sort`.
This is not strictly necessary here, but it’s good to know how to do this.

Be careful when closing the `"from"` end of a two-way pipe; in this
case `gawk` waits for the child process to exit, which may cause
your program to hang. (Thus, this particular feature is of much less
use in practice than being able to close the `"to"` end.)

> **CAUTION:** Normally,
> it is a fatal error to write to the `"to"` end of a two-way
> pipe which has been closed, and it is also a fatal error to read
> from the `"from"` end of a two-way pipe that has been closed.
>
> You may set `PROCINFO["command", "NONFATAL"]` to
> make such operations become nonfatal. If you do so, you then need
> to check `ERRNO` after each `print`, `printf`,
> or `getline`.
> See [Enabling Nonfatal Output](#Nonfatal), for more information.

You may also use pseudo-ttys (ptys) for
two-way communication instead of pipes, if your system supports them.
This is done on a per-command basis, by setting a special element
in the `PROCINFO` array
(see [Built-in Variables That Convey Information](#Auto_002dset)),
like so:

```
command = "sort -nr"           # command, save in convenience variable
PROCINFO[command, "pty"] = 1   # update PROCINFO
print ... |& command           # start two-way pipe
...
```

If your system does not have ptys, or if all the system’s ptys are in use,
`gawk` automatically falls back to using regular pipes.

Using ptys usually avoids the buffer deadlock issues described earlier,
at some loss in performance. This is because the tty driver buffers
and sends data line-by-line. On systems with the `stdbuf`
(part of the [GNU Coreutils package](https://www.gnu.org/software/coreutils/coreutils.html)), you can use that program instead of ptys.

Note also that ptys are not fully transparent. Certain binary control
codes, such `Ctrl-d` for end-of-file, are interpreted by the tty
driver and not passed through.

> **CAUTION:** Finally, coprocesses open up the possibility of *deadlock* between
> `gawk` and the program running in the coprocess. This can occur
> if you send “too much” data to the coprocess before reading any back;
> each process is blocked writing data with no one available to read what
> they’ve already written. There is no workaround for deadlock; careful
> programming and knowledge of the behavior of the coprocess are required.

The following example, due to Andrew Schorr, demonstrates how
using ptys can help deal with buffering deadlocks.

Suppose `gawk` were unable to add numbers.
You could use a coprocess to do it. Here’s an exceedingly
simple program written for that purpose:

```
$ cat add.c
#include <stdio.h>

int
main(void)
{
    int x, y;
    while (scanf("%d %d", & x, & y) == 2)
        printf("%d\n", x + y);
    return 0;
}
$ cc -O add.c -o add      Compile the program
```

You could then write an exceedingly simple `gawk` program
to add numbers by passing them to the coprocess:

```
$ echo 1 2 |
> gawk -v cmd=./add '{ print |& cmd; cmd |& getline x; print x }'
```

And it would deadlock, because `add.c` fails to call
‘`setlinebuf(stdout)`’. The `add` program freezes.

Now try instead:

```
$ echo 1 2 |
> gawk -v cmd=add 'BEGIN { PROCINFO[cmd, "pty"] = 1 }
>                  { print |& cmd; cmd |& getline x; print x }'
-| 3
```

By using a pty, `gawk` fools the standard I/O library into
thinking it has an interactive session, so it defaults to line buffering.
And now, magically, it works!

---

Next: [Profiling Your `awk` Programs](#Profiling), Previous: [Two-Way Communications with Another Process](#Two_002dway-I_002fO), Up: [Advanced Features of `gawk`](#Advanced-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.5 Using `gawk` for Network Programming [¶](#Using-gawk-for-Network-Programming)

> `EMRED`:  
>     *A host is a host from coast to coast,  
>     and nobody talks to a host that’s close,  
>     unless the host that isn’t close  
>     is busy, hung, or dead.*

— *Mike O’Brien (aka Mr. Protocol)*

In addition to being able to open a two-way pipeline to a coprocess
on the same system
(see [Two-Way Communications with Another Process](#Two_002dway-I_002fO)),
it is possible to make a two-way connection to
another process on another system across an IP network connection.

You can think of this as just a *very long* two-way pipeline to
a coprocess.
The way `gawk` decides that you want to use TCP/IP networking is
by recognizing special file names that begin with one of ‘`/inet/`’,
‘`/inet4/`’, or ‘`/inet6/`’.

The full syntax of the special file name is
`/net-type/protocol/local-port/remote-host/remote-port`.
The components are:

net-type
:   Specifies the kind of Internet connection to make.
    Use ‘`/inet4/`’ to force IPv4, and
    ‘`/inet6/`’ to force IPv6.
    Plain ‘`/inet/`’ (which used to be the only option) uses
    the system default, most likely IPv4.

protocol
:   The protocol to use over IP. This must be either ‘`tcp`’, or
    ‘`udp`’, for a TCP or UDP IP connection,
    respectively. TCP should be used for most applications.
    As of version 5.4 of `gawk`, use of UDP is considered
    obsolete, as it never worked very well. `gawk` issues
    a warning if UDP is used. Support for it will be removed in
    version 6.0.

local-port [¶](#index-getaddrinfo_0028_0029-function-_0028C-library_0029)
:   The local TCP or UDP port number to use. Use a port number of ‘`0`’
    when you want the system to pick a port. This is what you should do
    when writing a TCP or UDP client.
    You may also use a well-known service name, such as ‘`smtp`’
    or ‘`http`’, in which case `gawk` attempts to determine
    the predefined port number using the C `getaddrinfo()` function.

remote-host
:   The IP address or fully qualified domain name of the Internet
    host to which you want to connect.

remote-port
:   The TCP or UDP port number to use on the given remote-host.
    Again, use ‘`0`’ if you don’t care, or else a well-known
    service name.

> **NOTE:** Failure in opening a two-way socket will result in a nonfatal error
> being returned to the calling code. The value of `ERRNO` indicates
> the error (see [Built-in Variables That Convey Information](#Auto_002dset)).

Consider the following very simple example:

```
BEGIN {
    Service = "/inet/tcp/0/localhost/daytime"
    Service |& getline
    print $0
    close(Service)
}
```

This program reads the current date and time from the local system’s
TCP `daytime` server.
It then prints the results and closes the connection.

Because this topic is extensive, the use of `gawk` for
TCP/IP programming is documented separately.
See
[TCP/IP Internetworking with `gawk`](https://www.gnu.org/software/gawk/manual/gawkinet/),
which comes as part of the `gawk` distribution,
for a much more complete introduction and discussion, as well as
extensive examples.

> **NOTE:** `gawk` can only open direct sockets. There is currently
> no way to access services available over Secure Socket Layer
> (SSL); this includes any web service whose URL starts with ‘`https://`’.

---

Next: [Preserving Data Between Runs](#Persistent-Memory), Previous: [Using `gawk` for Network Programming](#TCP_002fIP-Networking), Up: [Advanced Features of `gawk`](#Advanced-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.6 Profiling Your `awk` Programs [¶](#Profiling-Your-awk-Programs)

You may produce execution traces of your `awk` programs.
This is done by passing the option `--profile` to `gawk`.
When `gawk` has finished running, it creates a profile of your program in a file
named `awkprof.out`. Because it is profiling, it also executes up to 45% slower than
`gawk` normally does.

As shown in the following example,
the `--profile` option can be used to change the name of the file
where `gawk` will write the profile:

```
gawk --profile=myprog.prof -f myprog.awk data1 data2
```

In the preceding example, `gawk` places the profile in
`myprog.prof` instead of in `awkprof.out`.

Here is a sample session showing a simple `awk` program,
its input data, and the results from running `gawk` with the
`--profile` option. First, the `awk` program:

```
BEGIN { print "First BEGIN rule" }

END { print "First END rule" }

/foo/ {
    print "matched /foo/, gosh"
    for (i = 1; i <= 3; i++)
        sing()
}

{
    if (/foo/)
        print "if is true"
    else
        print "else is true"
}

BEGIN { print "Second BEGIN rule" }

END { print "Second END rule" }

function sing(    dummy)
{
    print "I gotta be me!"
}
```

Following is the input data:

```
foo
bar
baz
foo
junk
```

Here is the `awkprof.out` that results from running the
`gawk` profiler on this program and data (this example also
illustrates that `awk` programmers sometimes get up very early
in the morning to work):

```
    # gawk profile, created Mon Sep 29 05:16:21 2014

    # BEGIN rule(s)

    BEGIN {
 1          print "First BEGIN rule"
    }

    BEGIN {
 1          print "Second BEGIN rule"
    }

    # Rule(s)

 5  /foo/ { # 2
 2          print "matched /foo/, gosh"
 6          for (i = 1; i <= 3; i++) {
 6                  sing()
            }
    }

 5  {
 5          if (/foo/) { # 2
 2                  print "if is true"
 3          } else {
 3                  print "else is true"
            }
    }

    # END rule(s)

    END {
 1          print "First END rule"
    }

    END {
 1          print "Second END rule"
    }

    # Functions, listed alphabetically

 6  function sing(dummy)
    {
 6          print "I gotta be me!"
    }
```

This example illustrates many of the basic features of profiling output.
They are as follows:

* The program is printed in the order `BEGIN` rules,
  `BEGINFILE` rules,
  pattern–action rules,
  `ENDFILE` rules, `END` rules, and functions, listed
  alphabetically.
  Multiple `BEGIN` and `END` rules retain their
  separate identities, as do
  multiple `BEGINFILE` and `ENDFILE` rules.
* Pattern–action rules have two counts.
  The first count, to the left of the rule, shows how many times
  the rule’s pattern was *tested*.
  The second count, to the right of the rule’s opening left brace
  in a comment,
  shows how many times the rule’s action was *executed*.
  The difference between the two indicates how many times the rule’s
  pattern evaluated to false.
* Similarly,
  the count for an `if`-`else` statement shows how many times
  the condition was tested.
  To the right of the opening left brace for the `if`’s body
  is a count showing how many times the condition was true.
  The count for the `else`
  indicates how many times the test failed.
* The count for a loop header (such as `for`
  or `while`) shows how many times the loop test was executed.
  (Because of this, you can’t just look at the count on the first
  statement in a rule to determine how many times the rule was executed.
  If the first statement is a loop, the count is misleading.)
* For user-defined functions, the count next to the `function`
  keyword indicates how many times the function was called.
  The counts next to the statements in the body show how many times
  those statements were executed.
* The layout uses “K&R” style with TABs.
  Braces are used everywhere, even when
  the body of an `if`, `else`, or loop is only a single statement.
* Parentheses are used only where needed, as indicated by the structure
  of the program and the precedence rules.
  For example, ‘`(3 + 5) * 4`’ means add three and five, then multiply
  the total by four. However, ‘`3 + 5 * 4`’ has no parentheses, and
  means ‘`3 + (5 * 4)`’.
  However, explicit parentheses in the source program are retained.
* Parentheses are used around the arguments to `print`
  and `printf` only when
  the `print` or `printf` statement is followed by a redirection.
  Similarly, if
  the target of a redirection isn’t a scalar, it gets parenthesized.
* `gawk` supplies leading comments in
  front of the `BEGIN` and `END` rules,
  the `BEGINFILE` and `ENDFILE` rules,
  the pattern–action rules, and the functions.
* Functions are listed alphabetically. All functions in the `awk`
  namespace are listed first, in alphabetical order. Then come the
  functions in namespaces. The namespaces are listed in alphabetical order,
  and the functions within each namespace are listed alphabetically.

The profiled version of your program may not look exactly like what you
typed when you wrote it. This is because `gawk` creates the
profiled version by “pretty-printing” its internal representation of
the program. The advantage to this is that `gawk` can produce
a standard representation.
Also, things such as:

```
/foo/
```

come out as:

```
/foo/   {
    print
}
```

which is correct, but possibly unexpected.
(If a program uses both ‘`print $0`’ and plain
‘`print`’, that distinction is retained.)

Besides creating profiles when a program has completed,
`gawk` can produce a profile while it is running.
This is useful if your `awk` program goes into an
infinite loop and you want to see what has been executed.
To use this feature, run `gawk` with the `--profile`
option in the background:

```
$ gawk --profile -f myprog &
[1] 13992
```

The shell prints a job number and process ID number; in this case, 13992.
Use the `kill` command to send the `USR1` signal
to `gawk`:

```
$ kill -USR1 13992
```

As usual, the profiled version of the program is written to
`awkprof.out`, or to a different file if one was specified with
the `--profile` option.

Along with the regular profile, as shown earlier, the profile file
includes a trace of any active functions:

```
# Function Call Stack:

#   3. baz
#   2. bar
#   1. foo
# -- main --
```

You may send `gawk` the `USR1` signal as many times as you like.
Each time, the profile and function call trace are appended to the output
profile file.

If you use the `HUP` signal instead of the `USR1` signal,
`gawk` produces the profile and the function call trace and then exits.

When `gawk` runs on MS-Windows systems, it uses the
`INT` and `QUIT` signals for producing the profile, and in
the case of the `INT` signal, `gawk` exits. This is
because these systems don’t support the `kill` command, so the
only signals you can deliver to a program are those generated by the
keyboard. The `INT` signal is generated by the
`Ctrl-c` or `Ctrl-BREAK` key, while the
`QUIT` signal is generated by the `Ctrl-\` key.

Finally, `gawk` also accepts another option, `--pretty-print`.
When called this way, `gawk` “pretty-prints” the program into
`awkprof.out`, without any execution counts.

> **NOTE:** Once upon a time, the `--pretty-print` option would also run
> your program. This is no longer the case.

There is a significant difference between the output created when
profiling, and that created when pretty-printing. Pretty-printed output
preserves the original comments that were in the program, although their
placement may not correspond exactly to their original locations in the
source code. However, no comments should be lost.
Also, `gawk` does the best it can to preserve
the distinction between comments at the end of a statement and comments
on lines by themselves. This isn’t always perfect, though.

However, as a deliberate design decision, profiling output *omits*
the original program’s comments. This allows you to focus on the
execution count data and helps you avoid the temptation to use the
profiler for pretty-printing.

Additionally, pretty-printed output does not have the leading indentation
that the profiling output does. This makes it easy to pretty-print your
code once development is completed, and then use the result as the final
version of your program.

Because the internal representation of your program is formatted to
recreate an `awk` program, profiling and pretty-printing
automatically disable `gawk`’s default optimizations.

Profiling and pretty-printing also preserve the original format of numeric
constants; if you used an octal or hexadecimal value in your source
code, it will appear that way in the output.

---

Next: [Builtin Features versus Extensions](#Extension-Philosophy), Previous: [Profiling Your `awk` Programs](#Profiling), Up: [Advanced Features of `gawk`](#Advanced-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.7 Preserving Data Between Runs [¶](#Preserving-Data-Between-Runs)

Starting with version 5.2, `gawk` supports
*persistent memory*. This feature stores the values of
all of `gawk`’s variables, arrays and user-defined functions
in a persistent heap, which resides in a file in
the filesystem. When persistent memory is not in use (the normal case),
`gawk`’s data resides in ephemeral system memory.

Persistent memory is enabled on certain 64-bit systems supporting the
`mmap()` and `munmap()` system calls. Since the persistent
store ends up holding pointers to functions held within the `gawk`
executable, in order to use persistent memory, you must use the same
`gawk` executable from run to run.

You can see if your version of `gawk` supports persistent
memory like so:

```
$ gawk --version
-| GNU Awk 5.4.0, API 4.1, PMA Avon 8-g1, (GNU MPFR 4.2.1, GNU MP 6.3.0)
-| Copyright (C) 1989, 1991-2026 Free Software Foundation.
...
```

If you see the ‘`PMA`’ with a version indicator, then it’s supported.

As of this writing, persistent memory has only been tested on GNU/Linux,
Cygwin,
FreeBSD 12–16,
macOS,
MidnightBSD 3 and 4,
NetBSD 10 and 11,
OpenBSD 7,
and
Solaris 2.11.
On all others, persistent memory is disabled by default. You can force
it to be enabled by exporting the shell variable
`REALLY_USE_PERSIST_MALLOC` with a nonempty value before
running `configure` (see [Compiling `gawk` for Unix-Like Systems](#Quick-Installation)).
If you do so and all the tests pass, please let the maintainer know.

To use persistent memory, follow these steps:

1. Create a new, empty sparse file of the desired size. For example, four
   gigabytes. On a GNU/Linux system, you can use the `truncate`
   utility:

   ```
   $ truncate -s 4G data.pma
   ```
2. It is recommended (but not required) to change the permissions on the
   file so that only the owner can read and write it:

   ```
   $ chmod 0600 data.pma
   ```
3. Provide the path to the data file in the `GAWK_PERSIST_FILE`
   environment variable. This is best done by placing the value in the
   environment just for the run of `gawk`, like so:

   ```
   $ GAWK_PERSIST_FILE=data.pma gawk 'BEGIN { print ++i }'
   1
   ```
4. Use the same data file in subsequent runs to use the preserved data values:

   ```
   $ GAWK_PERSIST_FILE=data.pma gawk 'BEGIN { print ++i }'
   2
   $ GAWK_PERSIST_FILE=data.pma gawk 'BEGIN { print ++i }'
   3
   ```

   As shown, in subsequent runs using the same data file, the values of
   `gawk`’s variables are preserved. However, `gawk`’s
   special variables, such as `NR`, are reset upon each run.
   Only the variables defined by the program are preserved across runs.

Interestingly, the `awk` program that you execute need not be the same from
run to run; the persistent store only maintains the values of variables,
arrays, and user-defined functions, not the totality of `gawk`’s
internal state. This lets you share data between unrelated `awk` programs,
eliminating the need for scripts to communicate via text files.

Terence Kelly, the author of the persistent memory allocator
`gawk` uses, provides the following advice about the backing file:

> Regarding backing file size, I recommend making it far larger
> than all of the data that will ever reside in it, assuming
> that the file system supports sparse files. The “pay only
> for what you use” aspect of sparse files ensures that the
> actual storage resource footprint of the backing file will
> meet the application’s needs but will be as small as
> possible. If the file system does *not* support sparse
> files, there’s a dilemma: Making the backing file too large
> is wasteful, but making it too small risks memory exhaustion,
> i.e., `pma_malloc()` returns `NULL`.
> But persistent `gawk` should
> still work even without sparse files.

You can disable the use of the persistent memory allocator in
`gawk` with the `--disable-pma` option to the `configure`
command at the time that you build `gawk` (see [Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)).

You can set the `PMA_VERBOSITY` environment variable to a
value between zero and three to control how much debugging
and error information the persistent memory allocator will print.
`gawk` sets the default to one. See the `support/pma.c`
source code to understand what the different verbosity levels are.

There are a few constraints on the use of persistent memory:

* If you use MPFR mode (the `-M` option) on the first run
  of a program using persistent memory, you *must* continue
  to use it on all subsequent runs. Similarly, if you don’t use `-M`
  on the first run, do not use it on any subsequent runs.

  Mixing and matching MPFR mode and regular mode with the same backing
  file is not allowed. `gawk` detects such a situation and issues
  a fatal error message.
* The GNU/Linux CIFS filesystem is known to not
  work well with the PMA allocator.
  Don’t use a backing file on a CIFS filesystem.
* If `gawk` is run by the `root` user, then
  persistent memory is not allowed. This is to avoid the possibility
  of private data “leaking” into the backing file and being
  recovered later by an attacker.
* Over time, the backing file will be filled with memory “leaked”
  by `gawk` as it runs. Most notably this is the memory used
  to compile your program into an internal form before running it,
  which happens each time, but there are other leakages as well.
  (For an extreme example of this, see
  [this thread](https://lists.gnu.org/archive/html/bug-gawk/2023-04/msg00025.html) in the “bug-gawk at gnu.org”
  mailing list archives.) It is up to you to use ‘`du -sh
  pmafile`’ occasionally to monitor how full the file is, and
  arrange to dump any data you may need before the backing file becomes
  full.
* You can use dynamically loaded extensions (see [Writing Extensions for `gawk`](#Dynamic-Extensions))
  with persistent memory. However, `gawk` notices if an extension
  is being loaded from a different path than previously, and in such a
  case it generates a fatal error.
* `gawk` produces a warning if the current version of `gawk`
  doesn’t match the one that was first used with a particular backing
  storage file.

Terence Kelly has provided a separate Persistent-Memory `gawk` User Manual
document, which is included in the `gawk`
distribution. It is worth reading.

Here are additional articles and web links that provide more information about
persistent memory and why it’s useful in a scripting language like
`gawk`.

<https://web.eecs.umich.edu/~tpkelly/pma/>
:   This is the canonical source for Terence Kelly’s Persistent Memory
    Allocator (PMA). The latest source code and user manual
    will always be available at this location.
    Kelly may be reached directly at any of the following
    email addresses:
    “tpkelly AT acm.org”,
    “tpkelly AT cs.princeton.edu”, or
    “tpkelly AT eecs.umich.edu”.

Persistent Memory Allocation
:   Terence Kelly, Zi Fan Tan, Jianan Li, and Haris Volos,
    ACM Queue magazine, Vol. 20 No. 2 (March/April 2022),
    [PDF](https://dl.acm.org/doi/pdf/10.1145/3534855),
    [HTML](https://queue.acm.org/detail.cfm?id=3534855).
    This paper explains the design of the PMA
    allocator used in persistent `gawk`.

Persistent Scripting
:   Zi Fan Tan, Jianan Li, Haris Volos, and Terence Kelly,
    Non-Volatile Memory Workshop (NVMW) 2022,
    <http://nvmw.ucsd.edu/program/>.
    This paper motivates and describes a research prototype of persistent
    `gawk` and presents performance evaluations on Intel Optane
    non-volatile memory; note that the interface differs slightly.

Persistent Memory Programming on Conventional Hardware
:   Terence Kelly,
    ACM Queue magazine Vol. 17 No. 4 (July/Aug 2019),
    [PDF](https://dl.acm.org/doi/pdf/10.1145/3358955.3358957),
    [HTML](https://queue.acm.org/detail.cfm?id=3358957).
    This paper describes simple techniques for persistent memory for C/C++
    code on conventional computers that lack non-volatile memory hardware.

Is Persistent Memory Persistent?
:   Terence Kelly,
    ACM Queue magazine Vol. 18 No. 2 (March/April 2020),
    [PDF](https://dl.acm.org/doi/pdf/10.1145/3400899.3400902),
    [HTML](https://queue.acm.org/detail.cfm?id=3400902).
    This paper describes a simple and robust testbed for testing software
    against real power failures.

Crashproofing the Original NoSQL Key/Value Store
:   Terence Kelly,
    ACM Queue magazine Vol. 19 No. 4 (July/Aug 2021),
    [PDF](https://dl.acm.org/doi/pdf/10.1145/3487019.3487353),
    [HTML](https://queue.acm.org/detail.cfm?id=3487353).
    This paper describes a crash-tolerance feature added to GNU DBM’
    (`gdbm`).

When Terence Kelly published his papers, his collaborators produced
a prototype integration of PMA with `gawk`. That version used
a (mandatory!) option `--persist=file` to specify the file
for storing the persistent heap. If this option is given to `gawk`,
it produces a fatal error message instructing the user to use the
`GAWK_PERSIST_FILE` environment variable instead. Except for this
paragraph, that option is otherwise undocumented.

The prototype only supported persistent data; it did not
support persistent functions.

> **NOTE:** The maintainer reserves the right to remove the persistent memory
> feature should it become burdensome.[90](#FOOT90)

---

Next: [Summary](#Advanced-Features-Summary), Previous: [Preserving Data Between Runs](#Persistent-Memory), Up: [Advanced Features of `gawk`](#Advanced-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.8 Builtin Features versus Extensions [¶](#Builtin-Features-versus-Extensions)

As this and subsequent chapters show, `gawk` has a
large number of extensions over standard `awk` built-in to
the program. These have developed over time. More recently, the
focus has moved to using the extension mechanism (see [Writing Extensions for `gawk`](#Dynamic-Extensions))
for adding features. This section discusses the “guiding philosophy”
behind what should be added to the interpreter as a built-in
feature versus what should be done in extensions.

There are several goals:

1. Keep the language `awk`; it should not become unrecognizable, even
   if programs in it will only run on `gawk`.
2. Keep the core from getting any larger unless absolutely necessary.
3. Add new functionality either in `awk` scripts (`-f`,
   `@include`) or in loadable extensions written in C or C++
   (`-l`, `@load`).
4. Extend the core interpreter only if some feature is:
   1. Truly desirable.
   2. Cannot be done via library files or loadable extensions.
   3. Can be implemented without too much pain in the core.

Combining modules with `awk` files is a powerful technique.
Some of the sample extensions demonstrate this.

Loading extensions and library files should not be done automatically,
because then there’s overhead that most users don’t want or need.

---

Previous: [Builtin Features versus Extensions](#Extension-Philosophy), Up: [Advanced Features of `gawk`](#Advanced-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.9 Summary [¶](#Summary-11)

* The `--non-decimal-data` option causes `gawk` to treat
  octal- and hexadecimal-looking input data as octal and hexadecimal.
  This option should be used with caution or not at all; use of `strtonum()`
  is preferable.
  Note that this option may disappear in a future version of `gawk`.
* You can take over complete control of sorting in ‘`for (indx in array)`’
  array traversal by setting `PROCINFO["sorted_in"]` to the name of a user-defined
  function that does the comparison of array elements based on index and value.
* Similarly, you can supply the name of a user-defined comparison function as the
  third argument to either `asort()` or `asorti()` to control how
  those functions sort arrays. Or you may provide one of the predefined control
  strings that work for `PROCINFO["sorted_in"]`.
* You can use the ‘`|&`’ operator to create a two-way pipe to a coprocess.
  You read from the coprocess with `getline` and write to it with `print`
  or `printf`. Use `close()` to close off the coprocess completely, or
  optionally, close off one side of the two-way communications.
* By using special file names with the ‘`|&`’ operator, you can open a
  TCP/IP (or UDP/IP) connection to remote hosts on the Internet. `gawk`
  supports both IPv4 and IPv6.
* You can generate statement count profiles of your program. This can help you
  determine which parts of your program may be taking the most time and let
  you tune them more easily. Sending the `USR1` signal while profiling causes
  `gawk` to dump the profile and keep going, including a function call stack.
* You can also just “pretty-print” the program.
* Persistent memory allows you to preserve the values of variables and arrays
  between runs of `gawk`. This feature is currently experimental.
* New features should be developed using the extension mechanism if possible;
  they should be added to the core interpreter only as a last resort.

---

Next: [Debugging `awk` Programs](#Debugger), Previous: [Advanced Features of `gawk`](#Advanced-Features), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 13 Internationalization with `gawk` [¶](#Internationalization-with-gawk)

> *Moon… Gorgeous… MEDITATION!*

— *Pretty Guardian Sailor Moon Eternal, The Movie*

> *It probably sounded better in Japanese.*

— *Malka Robbins*

Once upon a time, computer makers
wrote software that worked only in English.
Eventually, hardware and software vendors noticed that if their
systems worked in the native languages of non-English-speaking
countries, they were able to sell more systems.
As a result, internationalization and localization
of programs and software systems became a common practice.

For many years, the ability to provide internationalization
was largely restricted to programs written in C and C++.
This chapter describes the underlying library `gawk`
uses for internationalization, as well as how
`gawk` makes internationalization
features available at the `awk` program level.
Having internationalization available at the `awk` level
gives software developers additional flexibility—they are no
longer forced to write in C or C++ when internationalization is
a requirement.

* [Internationalization and Localization](#I18N-and-L10N)
* [GNU `gettext`](#Explaining-gettext)
* [Internationalizing `awk` Programs](#Programmer-i18n)
* [Translating `awk` Programs](#Translator-i18n)
* [A Simple Internationalization Example](#I18N-Example)
* [`gawk` Can Speak Your Language](#Gawk-I18N)
* [Summary](#I18N-Summary)

---

Next: [GNU `gettext`](#Explaining-gettext), Up: [Internationalization with `gawk`](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.1 Internationalization and Localization [¶](#Internationalization-and-Localization)

*Internationalization* means writing (or modifying) a program once,
in such a way that it can use multiple languages without requiring
further source code changes.
*Localization* means providing the data necessary for an
internationalized program to work in a particular language.
Most typically, these terms refer to features such as the language
used for printing error messages, the language used to read
responses, and information related to how numerical and
monetary values are printed and read.

---

Next: [Internationalizing `awk` Programs](#Programmer-i18n), Previous: [Internationalization and Localization](#I18N-and-L10N), Up: [Internationalization with `gawk`](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.2 GNU `gettext` [¶](#GNU-gettext)

`gawk` uses GNU `gettext` to provide its internationalization
features.
The facilities in GNU `gettext` focus on messages: strings printed
by a program, either directly or via formatting with `printf` or
`sprintf()`.[91](#FOOT91)

When using GNU `gettext`, each application has its own
*text domain*. This is a unique name, such as ‘`kpilot`’ or ‘`gawk`’,
that identifies the application.
A complete application may have multiple components—programs written
in C or C++, as well as scripts written in `sh` or `awk`.
All of the components use the same text domain.

To make the discussion concrete, assume we’re writing an application
named `guide`. Internationalization consists of the
following steps, in this order:

1. The programmer reviews the source for all of `guide`’s components
   and marks each string that is a candidate for translation.
   For example, `` "`-F': option required" `` is a good candidate for translation.
   A table with strings of option names is not (e.g., `gawk`’s
   `--profile` option should remain the same, no matter what the local
   language).
2. The programmer indicates the application’s text domain
   (`"guide"`) to the `gettext` library,
   by calling the `textdomain()` function.
3. Messages from the application are extracted from the source code and
   collected into a portable object template file (`guide.pot`),
   which lists the strings and their translations.
   The translations are initially empty.
   The original (usually English) messages serve as the key for
   lookup of the translations.
4. For each language with a translator, `guide.pot`
   is copied to a portable object file (`.po`)
   and translations are created and shipped with the application.
   For example, there might be a `fr.po` for a French translation.
5. Each language’s `.po` file is converted into a binary
   message object (`.gmo`) file.
   A message object file contains the original messages and their
   translations in a binary format that allows fast lookup of translations
   at runtime.
6. When `guide` is built and installed, the binary translation files
   are installed in a standard place.
7. For testing and development, it is possible to tell `gettext`
   to use `.gmo` files in a different directory than the standard
   one by using the `bindtextdomain()` function.
8. At runtime, `guide` looks up each string via a call
   to `gettext()`. The returned string is the translated string
   if available, or the original string if not.
9. If necessary, it is possible to access messages from a different
   text domain than the one belonging to the application, without
   having to switch the application’s default text domain back
   and forth.

In C (or C++), the string marking and dynamic translation lookup
are accomplished by wrapping each string in a call to `gettext()`:

```
printf("%s", gettext("Don't Panic!\n"));
```

The tools that extract messages from source code pull out all
strings enclosed in calls to `gettext()`.

The GNU `gettext` developers, recognizing that typing
‘`gettext(…)`’ over and over again is both painful and ugly to look
at, use the macro ‘`_`’ (an underscore) to make things easier:

```
/* In the standard header file: */
#define _(str) gettext(str)

/* In the program text: */
printf("%s", _("Don't Panic!\n"));
```

This reduces the typing overhead to just three extra characters per string
and is considerably easier to read as well.

There are locale *categories*
for different types of locale-related information.
The defined locale categories that `gettext` knows about are:

`LC_MESSAGES` [¶](#index-LC_005fMESSAGES-locale-category)
:   Text messages. This is the default category for `gettext`
    operations, but it is possible to supply a different one explicitly,
    if necessary. (It is almost never necessary to supply a different category.)

`LC_COLLATE` [¶](#index-sorting-characters-in-different-languages)
:   Text-collation information (i.e., how different characters
    and/or groups of characters sort in a given language).

`LC_CTYPE` [¶](#index-LC_005fCTYPE-locale-category)
:   Character-type information (alphabetic, digit, upper- or lowercase, and
    so on) as well as character encoding.
    This information is accessed via the
    POSIX character classes in regular expressions,
    such as `/[[:alnum:]]/`
    (see [Using Bracket Expressions](#Bracket-Expressions)).

`LC_MONETARY` [¶](#index-LC_005fMONETARY-locale-category)
:   Monetary information, such as the currency symbol, and whether the
    symbol goes before or after a number.

`LC_NUMERIC` [¶](#index-LC_005fNUMERIC-locale-category)
:   Numeric information, such as which characters to use for the decimal
    point and the thousands separator.[92](#FOOT92)

`LC_TIME` [¶](#index-LC_005fTIME-locale-category)
:   Time- and date-related information, such as 12- or 24-hour clock, month printed
    before or after the day in a date, local month abbreviations, and so on.

`LC_ALL` [¶](#index-LC_005fALL-locale-category)
:   All of the above. (Not too useful in the context of `gettext`.)

> **NOTE:** As described in [Where You Are Makes a Difference](#Locales), environment variables with the same
> name as the locale categories (`LC_CTYPE`, `LC_ALL`, etc.)
> influence `gawk`’s behavior (and that of other utilities).
>
> Normally, these variables also affect how the `gettext` library
> finds translations. However, the `LANGUAGE` environment variable
> overrides the `LC_xxx` variables. Many GNU/Linux systems
> may define this variable without your knowledge, causing `gawk`
> to not find the correct translations. If this happens to you,
> look to see if `LANGUAGE` is defined, and if so, use the shell’s
> `unset` command to remove it.

For testing translations of `gawk` itself, you can set
the `GAWK_LOCALE_DIR` environment variable. See the documentation
for the C `bindtextdomain()` function and also see
[Other Environment Variables](#Other-Environment-Variables).

---

Next: [Translating `awk` Programs](#Translator-i18n), Previous: [GNU `gettext`](#Explaining-gettext), Up: [Internationalization with `gawk`](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.3 Internationalizing `awk` Programs [¶](#Internationalizing-awk-Programs)

`gawk` provides the following variables for
internationalization:

`TEXTDOMAIN` [¶](#index-TEXTDOMAIN-variable-1)
:   This variable indicates the application’s text domain.
    For compatibility with GNU `gettext`, the default
    value is `"messages"`.

`_"your message here"`
:   String constants marked with a leading underscore
    are candidates for translation at runtime.
    String constants without a leading underscore are not translated.

`gawk` provides the following functions for
internationalization:

`dcgettext(string [, domain [, category]])` [¶](#index-dcgettext_0028_0029-function-_0028gawk_0029-1)
:   Return the translation of string in
    text domain domain for locale category category.
    The default value for domain is the current value of `TEXTDOMAIN`.
    The default value for category is `"LC_MESSAGES"`.

    If you supply a value for category, it must be a string equal to
    one of the known locale categories described in
    the previous section.
    You must also supply a text domain. Use `TEXTDOMAIN` if
    you want to use the current domain.

    > **CAUTION:** The order of arguments to the `awk` version
    > of the `dcgettext()` function is purposely different from the order for
    > the C version. The `awk` version’s order was
    > chosen to be simple and to allow for reasonable `awk`-style
    > default arguments.

`dcngettext(string1, string2, number [, domain [, category]])` [¶](#index-dcngettext_0028_0029-function-_0028gawk_0029-1)
:   Return the plural form used for number of the
    translation of string1 and string2 in text domain
    domain for locale category category. string1 is the
    English singular variant of a message, and string2 is the English plural
    variant of the same message.
    The default value for domain is the current value of `TEXTDOMAIN`.
    The default value for category is `"LC_MESSAGES"`.

    The same remarks about argument order as for the `dcgettext()` function apply.

`bindtextdomain(directory [, domain ])` [¶](#index-bindtextdomain_0028_0029-function-_0028gawk_0029-1)
:   Change the directory in which
    `gettext` looks for `.gmo` files, in case they
    will not or cannot be placed in the standard locations
    (e.g., during testing).
    Return the directory in which domain is “bound.”

    The default domain is the value of `TEXTDOMAIN`.
    If directory is the null string (`""`), then
    `bindtextdomain()` returns the current binding for the
    given domain.

To use these facilities in your `awk` program, follow these steps:

1. Set the variable `TEXTDOMAIN` to the text domain of
   your program. This is best done in a `BEGIN` rule
   (see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)),
   or it can also be done via the `-v` command-line
   option (see [Command-Line Options](#Options)):

   ```
   BEGIN {
       TEXTDOMAIN = "guide"
       ...
   }
   ```
2. Mark all translatable strings with a leading underscore (‘`_`’)
   character. It *must* be adjacent to the opening
   double quote of the string. For example:

   ```
   print _"hello, world"
   x = _"you goofed"
   printf(_"Number of users is %d\n", nusers)
   ```
3. If you are creating strings dynamically, you can
   still translate them, using the `dcgettext()`
   built-in function:[93](#FOOT93)

   ```
   if (groggy)
       message = dcgettext("%d customers disturbing me\n", "adminprog")
   else
       message = dcgettext("enjoying %d customers\n", "adminprog")
   printf(message, ncustomers)
   ```

   Here, the call to `dcgettext()` supplies a different
   text domain (`"adminprog"`) in which to find the
   message, but it uses the default `"LC_MESSAGES"` category.

   The previous example only works if `ncustomers` is greater than one.
   This example would be better done with `dcngettext()`:

   ```
   if (groggy)
       message = dcngettext("%d customer disturbing me\n",
                            "%d customers disturbing me\n",
                            ncustomers, "adminprog")
   else
       message = dcngettext("enjoying %d customer\n",
                            "enjoying %d customers\n",
                            ncustomers, "adminprog")
   printf(message, ncustomers)
   ```
4. During development, you might want to put the `.gmo`
   file in a private directory for testing. This is done
   with the `bindtextdomain()` built-in function:

   ```
   BEGIN {
      TEXTDOMAIN = "guide"   # our text domain
      if (Testing) {
          # where to find our files
          bindtextdomain("testdir")
          # joe is in charge of adminprog
          bindtextdomain("../joe/testdir", "adminprog")
      }
      ...
   }
   ```

See [A Simple Internationalization Example](#I18N-Example)
for an example program showing the steps to create
and use translations from `awk`.

---

Next: [A Simple Internationalization Example](#I18N-Example), Previous: [Internationalizing `awk` Programs](#Programmer-i18n), Up: [Internationalization with `gawk`](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
