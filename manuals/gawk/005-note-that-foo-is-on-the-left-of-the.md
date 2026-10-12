# Note that /foo/ is on the left of the ~
if (/foo/ ~ $1) print "found foo"
```

This code is “obviously” testing `$1` for a match against the regexp
`/foo/`. But in fact, the expression ‘`/foo/ ~ $1`’ really means
‘`($0 ~ /foo/) ~ $1`’. In other words, first match the input record
against the regexp `/foo/`. The result is either zero or one,
depending upon the success or failure of the match. That result
is then matched against the first field in the record.
Because it is unlikely that you would ever really want to make this kind of
test, `gawk` issues a warning when it sees this construct in
a program.
Another consequence of this rule is that the assignment statement:

```
matches = /foo/
```

assigns either zero or one to the variable `matches`, depending
upon the contents of the current input record.

Constant regular expressions are also used as the first argument for
the `gensub()`, `sub()`, and `gsub()` functions, as the
second argument of the `match()` function,
and as the third argument of the `split()` and `patsplit()` functions
(see [String-Manipulation Functions](#String-Functions)).
Modern implementations of `awk`, including `gawk`, allow
the third argument of `split()` to be a regexp constant, but some
older implementations do not.
(d.c.)
Because some built-in functions accept regexp constants as arguments,
confusion can arise when attempting to use regexp constants as arguments
to user-defined functions (see [User-Defined Functions](#User_002ddefined)). For example:

```
function mysub(pat, repl, str, global)
{
    if (global)
        gsub(pat, repl, str)
    else
        sub(pat, repl, str)
    return str
}
```

```

```

```
{
    ...
    text = "hi! hi yourself!"
    mysub(/hi/, "howdy", text, 1)
    ...
}
```

In this example, the programmer wants to pass a regexp constant to the
user-defined function `mysub()`, which in turn passes it on to
either `sub()` or `gsub()`. However, what really happens is that
the `pat` parameter is assigned a value of either one or zero, depending upon whether
or not `$0` matches `/hi/`.
`gawk` issues a warning when it sees a regexp constant used as
a parameter to a user-defined function, because passing a truth value in
this way is probably not what was intended.

---

Previous: [Standard Regular Expression Constants](#Standard-Regexp-Constants), Up: [Using Regular Expression Constants](#Using-Constant-Regexps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.2.2 Strongly Typed Regexp Constants [¶](#Strongly-Typed-Regexp-Constants)

This section describes a `gawk`-specific feature.

As we saw in the previous section,
regexp constants (`/…/`) hold a strange position in the
`awk` language. In most contexts, they act like an expression:
‘`$0 ~ /…/`’. In other contexts, they denote only a regexp to
be matched. In no case are they really a “first class citizen” of the
language. That is, you cannot define a scalar variable whose type is
“regexp” in the same sense that you can define a variable to be a
number or a string:

```
num = 42        Numeric variable
str = "hi"      String variable
re = /foo/      Wrong! re is the result of $0 ~ /foo/
```

For a number of more advanced use cases,
it would be nice to have regexp constants that
are *strongly typed*; in other words, that denote a regexp useful
for matching, and not an expression.

`gawk` provides this feature. A strongly typed regexp constant
looks almost like a regular regexp constant, except that it is preceded
by an ‘`@`’ sign:

```
re = @/foo/     Regexp variable
```

Strongly typed regexp constants *cannot* be used everywhere that a
regular regexp constant can, because this would make the language even more
confusing. Instead, you may use them only in certain contexts:

* On the righthand side of the ‘`~`’ and ‘`!~`’ operators: ‘`some_var ~ @/foo/`’
  (see [How to Use Regular Expressions](#Regexp-Usage)).
* In the `case` part of a `switch` statement
  (see [The `switch` Statement](#Switch-Statement)).
* As an argument to one of the built-in functions that accept regexp constants:
  `gensub()`,
  `gsub()`,
  `match()`,
  `patsplit()`,
  `split()`,
  and
  `sub()`
  (see [String-Manipulation Functions](#String-Functions)).
* As a parameter in a call to a user-defined function
  (see [User-Defined Functions](#User_002ddefined)).
* As the return value of a user-defined function.
* On the righthand side of an assignment to a variable: ‘`some_var = @/foo/`’.
  In this case, the type of `some_var` is regexp. Additionally, `some_var`
  can be used with ‘`~`’ and ‘`!~`’, passed to one of the built-in functions
  listed above, or passed as a parameter to a user-defined function.

You may use the `-v` option (see [Command-Line Options](#Options)) to assign a
strongly-typed regexp constant to a variable on the command line, like so:

```
gawk -v pattern='@/something(interesting)+/' ...
```

You may also make such assignments as regular command-line arguments
(see [Other Command-Line Arguments](#Other-Arguments)).

You may use the `typeof()` built-in function
(see [Getting Type Information](#Type-Functions))
to determine if a variable or function parameter is
a regexp variable.

The true power of this feature comes from the ability to create variables that
have regexp type. Such variables can be passed on to user-defined functions,
without the confusing aspects of computed regular expressions created from
strings or string constants. They may also be passed through indirect function
calls (see [Indirect Function Calls](#Indirect-Calls))
and on to the built-in functions that accept regexp constants.

When used in numeric conversions, strongly typed regexp variables convert
to zero. When used in string conversions, they convert to the string
value of the original regexp text.

There is an additional, interesting corner case. When used as the third
argument to `sub()` or `gsub()`, they retain their type. Thus,
if you have something like this:

```
re = @/don't panic/
sub(/don't/, "do", re)
print typeof(re), re
```

then `re` retains its type, but now attempts to match the string
‘`do panic`’. This provides a (very indirect) way to create regexp-typed
variables at runtime.

---

Next: [Conversion of Strings and Numbers](#Conversion), Previous: [Using Regular Expression Constants](#Using-Constant-Regexps), Up: [Constants, Variables, and Conversions](#Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.3 Variables [¶](#Variables-1)

*Variables* are ways of storing values at one point in your program for
use later in another part of your program. They can be manipulated
entirely within the program text, and they can also be assigned values
on the `awk` command line.

* [Using Variables in a Program](#Using-Variables)
* [Assigning Variables on the Command Line](#Assignment-Options)

---

Next: [Assigning Variables on the Command Line](#Assignment-Options), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.3.1 Using Variables in a Program [¶](#Using-Variables-in-a-Program)

Variables let you give names to values and refer to them later. Variables
have already been used in many of the examples. The name of a variable
must be a sequence of letters, digits, or underscores, and it may not begin
with a digit.
Here, a *letter* is any one of the 52 upper- and lowercase
English letters. Other characters that may be defined as letters
in non-English locales are not valid in variable names.
Case is significant in variable names; `a` and `A`
are distinct variables.

A variable name is a valid expression by itself; it represents the
variable’s current value. Variables are given new values with
*assignment operators*, *increment operators*, and
*decrement operators*
(see [Assignment Expressions](#Assignment-Ops)).
In addition, the `sub()` and `gsub()` functions can
change a variable’s value, and the `match()`, `split()`,
and `patsplit()` functions can change the contents of their
array parameters (see [String-Manipulation Functions](#String-Functions)).

A few variables have special built-in meanings, such as `FS` (the
field separator) and `NF` (the number of fields in the current input
record). See [Predefined Variables](#Built_002din-Variables) for a list of the predefined variables.
These predefined variables can be used and assigned just like all other
variables, but their values are also used or changed automatically by
`awk`. All predefined variables’ names are entirely uppercase.

Variables in `awk` can be assigned either numeric or string values.
The kind of value a variable holds can change over the life of a program.
By default, variables are initialized to the empty string, which
is zero if converted to a number. There is no need to explicitly
initialize a variable in `awk`,
which is what you would do in C and in most other traditional languages.

---

Previous: [Using Variables in a Program](#Using-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.3.2 Assigning Variables on the Command Line [¶](#Assigning-Variables-on-the-Command-Line)

Any `awk` variable can be set by including a *variable assignment*
among the arguments on the command line when `awk` is invoked
(see [Other Command-Line Arguments](#Other-Arguments)).
Such an assignment has the following form:

```
variable=text
```

With it, a variable is set either at the beginning of the
`awk` run or in between input files.
When the assignment is preceded with the `-v` option,
as in the following:

```
-v variable=text
```

the variable is set at the very beginning, even before the
`BEGIN` rules execute. The `-v` option and its assignment
must precede all the file name arguments, as well as the program text.
(See [Command-Line Options](#Options) for more information about
the `-v` option.)
Otherwise, the variable assignment is performed at a time determined by
its position among the input file arguments—after the processing of the
preceding input file argument. For example:

```
awk '{ print $n }' n=4 inventory-shipped n=2 mail-list
```

prints the value of field number `n` for all input records. Before
the first file is read, the command line sets the variable `n`
equal to four. This causes the fourth field to be printed in lines from
`inventory-shipped`. After the first file has finished,
but before the second file is started, `n` is set to two, so that the
second field is printed in lines from `mail-list`:

```
$ awk '{ print $n }' n=4 inventory-shipped n=2 mail-list
-| 15
-| 24
...
-| 555-5553
-| 555-3412
...
```

Command-line arguments are made available for explicit examination by
the `awk` program in the `ARGV` array
(see [Using `ARGC` and `ARGV`](#ARGC-and-ARGV)).
`awk` processes the values of command-line assignments for escape
sequences
(see [Escape Sequences](#Escape-Sequences)).
(d.c.)

Normally, variables assigned on the command line (with or without the
`-v` option) are treated as strings. When such variables are
used as numbers, `awk`’s normal automatic conversion of strings
to numbers takes place, and everything “just works.”

However, `gawk` supports variables whose types are “regexp”.
You can assign variables of this type using the following syntax:

```
gawk -v 're1=@/foo|bar/' '...' /path/to/file1 're2=@/baz|quux/' /path/to/file2
```

Strongly typed regexps are an advanced feature (see [Strongly Typed Regexp Constants](#Strong-Regexp-Constants)).
We mention them here only for completeness.

---

Previous: [Variables](#Variables), Up: [Constants, Variables, and Conversions](#Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.4 Conversion of Strings and Numbers [¶](#Conversion-of-Strings-and-Numbers)

Number-to-string and string-to-number conversion are generally
straightforward. There can be subtleties to be aware of;
this section discusses this important facet of `awk`.

* [How `awk` Converts Between Strings and Numbers](#Strings-And-Numbers)
* [Locales Can Influence Conversion](#Locale-influences-conversions)

---

Next: [Locales Can Influence Conversion](#Locale-influences-conversions), Up: [Conversion of Strings and Numbers](#Conversion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.4.1 How `awk` Converts Between Strings and Numbers [¶](#How-awk-Converts-Between-Strings-and-Numbers)

Strings are converted to numbers and numbers are converted to strings, if the context
of the `awk` program demands it. For example, if the value of
either `foo` or `bar` in the expression ‘`foo + bar`’
happens to be a string, it is converted to a number before the addition
is performed. If numeric values appear in string concatenation, they
are converted to strings. Consider the following:

```
two = 2; three = 3
print (two three) + 4
```

This prints the (numeric) value 27. The numeric values of
the variables `two` and `three` are converted to strings and
concatenated together. The resulting string is converted back to the
number 23, to which 4 is then added.

If, for some reason, you need to force a number to be converted to a
string, concatenate that number with the empty string, `""`.
To force a string to be converted to a number, add zero to that string.
A string is converted to a number by interpreting any numeric prefix
of the string as numerals:
`"2.5"` converts to 2.5, `"1e3"` converts to 1,000, and `"25fix"`
has a numeric value of 25.
Strings that can’t be interpreted as valid numbers convert to zero.

The exact manner in which numbers are converted into strings is controlled
by the `awk` predefined variable `CONVFMT` (see [Predefined Variables](#Built_002din-Variables)).
Numbers are converted using the `sprintf()` function
with `CONVFMT` as the format
specifier
(see [String-Manipulation Functions](#String-Functions)).

`CONVFMT`’s default value is `"%.6g"`, which creates a value with
at most six significant digits. For some applications, you might want to
change it to specify more precision.
On most modern machines,
17 digits is usually enough to capture a floating-point number’s
value exactly.[35](#FOOT35)

Strange results can occur if you set `CONVFMT` to a string that doesn’t
tell `sprintf()` how to format floating-point numbers in a useful way.
For example, if you forget the ‘`%`’ in the format, `awk` converts
all numbers to the same constant string.

As a special case, if a number is an integer, then the result of converting
it to a string is *always* an integer, no matter what the value of
`CONVFMT` may be. Given the following code fragment:

```
CONVFMT = "%2.2f"
a = 12
b = a ""
```

`b` has the value `"12"`, not `"12.00"`.
(d.c.)

| Pre-POSIX `awk` Used `OFMT` for String Conversion |
| --- |
| Prior to the POSIX standard, `awk` used the value of `OFMT` for converting numbers to strings. `OFMT` specifies the output format to use when printing numbers with `print`. `CONVFMT` was introduced in order to separate the semantics of conversion from the semantics of printing. Both `CONVFMT` and `OFMT` have the same default value: `"%.6g"`. In the vast majority of cases, old `awk` programs do not change their behavior. See [The `print` Statement](#Print) for more information on the `print` statement. |

---

Previous: [How `awk` Converts Between Strings and Numbers](#Strings-And-Numbers), Up: [Conversion of Strings and Numbers](#Conversion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.1.4.2 Locales Can Influence Conversion [¶](#Locales-Can-Influence-Conversion)

Where you are can matter when it comes to converting between numbers and
strings. The local character set and language—the *locale*—can
affect numeric formats. In particular, for `awk` programs,
it affects the decimal point character and the thousands-separator
character. The `"C"` locale, and most English-language locales,
use the period character (‘`.`’) as the decimal point and don’t
have a thousands separator. However, many (if not most) European and
non-English locales use the comma (‘`,`’) as the decimal point
character. European locales often use either a space or a period as
the thousands separator, if they have one.

The POSIX standard says that `awk` always uses the period as the decimal
point when reading the `awk` program source code, and for
command-line variable assignments (see [Other Command-Line Arguments](#Other-Arguments)). However,
when interpreting input data, for `print` and `printf` output,
and for number-to-string conversion, the local decimal point character
is used. (d.c.) In all cases, numbers in source code and
in input data cannot have a thousands separator. Here are some examples
indicating the difference in behavior, on a GNU/Linux system:

```
$ export POSIXLY_CORRECT=1                        Force POSIX behavior
$ gawk 'BEGIN { printf "%g\n", 3.1415927 }'
-| 3.14159
$ LC_ALL=en_DK.utf-8 gawk 'BEGIN { printf "%g\n", 3.1415927 }'
-| 3,14159
$ echo 4,321 | gawk '{ print $1 + 1 }'
-| 5
$ echo 4,321 | LC_ALL=en_DK.utf-8 gawk '{ print $1 + 1 }'
-| 5,321
```

The `en_DK.utf-8` locale is for English in Denmark, where the comma acts as
the decimal point separator. In the normal `"C"` locale, `gawk`
treats ‘`4,321`’ as 4, while in the Danish locale, it’s treated
as the full number including the fractional part, 4.321.

Some earlier versions of `gawk` fully complied with this aspect
of the standard. However, many users in non-English locales complained
about this behavior, because their data used a period as the decimal
point, so the default behavior was restored to use a period as the
decimal point character. You can use the `--use-lc-numeric`
option (see [Command-Line Options](#Options)) to force `gawk` to use the locale’s
decimal point character. (`gawk` also uses the locale’s decimal
point character when in POSIX mode, either via `--posix` or the
`POSIXLY_CORRECT` environment variable, as shown previously.)

[Table 6.1](#table_002dlocale_002daffects) describes the cases in which the locale’s decimal
point character is used and when a period is used. Some of these
features have not been described yet.

| Feature | Default | `--posix` or `--use-lc-numeric` |
| --- | --- | --- |
| `%'g` | Use locale | Use locale |
| `%g` | Use period | Use locale |
| Input | Use period | Use locale |
| `strtonum()` | Use period | Use locale |

**Table 6.1:** Locale decimal point versus a period

Finally, modern-day formal standards and the IEEE standard floating-point
representation can have an unusual but important effect on the way
`gawk` converts some special string values to numbers. The details
are presented in [Standards Versus Existing Practice](#POSIX-Floating-Point-Problems).

---

Next: [Truth Values and Conditions](#Truth-Values-and-Conditions), Previous: [Constants, Variables, and Conversions](#Values), Up: [Expressions](#Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.2 Operators: Doing Something with Values [¶](#Operators_003a-Doing-Something-with-Values)

This section introduces the *operators* that make use
of the values provided by constants and variables.

* [Arithmetic Operators](#Arithmetic-Ops)
* [String Concatenation](#Concatenation)
* [Assignment Expressions](#Assignment-Ops)
* [Increment and Decrement Operators](#Increment-Ops)

---

Next: [String Concatenation](#Concatenation), Up: [Operators: Doing Something with Values](#All-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.2.1 Arithmetic Operators [¶](#Arithmetic-Operators)

The `awk` language uses the common arithmetic operators when
evaluating expressions. All of these arithmetic operators follow normal
precedence rules and work as you would expect them to.

The following example uses a file named `grades`, which contains
a list of student names as well as three test scores per student (it’s
a small class):

```
Pat   100 97 58
Sandy  84 72 93
Chris  72 92 89
```

This program takes the file `grades` and prints the average
of the scores:

```
$ awk '{ sum = $2 + $3 + $4 ; avg = sum / 3
>        print $1, avg }' grades
-| Pat 85
-| Sandy 83
-| Chris 84.3333
```

The following list provides the arithmetic operators in `awk`,
in order from the highest precedence to the lowest:

`x ^ y`

`x ** y`
:   Exponentiation; x raised to the y power. ‘`2 ^ 3`’ has
    the value eight; the character sequence ‘`**`’ is equivalent to
    ‘`^`’. (c.e.)

`- x`
:   Negation.

`+ x`
:   Unary plus; the expression is converted to a number.

`x * y`
:   Multiplication.

`x / y` [¶](#index-division)
:   Division; because all numbers in `awk` are floating-point
    numbers, the result is *not* rounded to an integer—‘`3 / 4`’ has
    the value 0.75. (It is a common mistake, especially for C programmers,
    to forget that *all* numbers in `awk` are floating point,
    and that division of integer-looking constants produces a real number,
    not an integer.)

`x % y`
:   Remainder; further discussion is provided in the text, just
    after this list.

`x + y`
:   Addition.

`x - y`
:   Subtraction.

Unary plus and minus have the same precedence,
the multiplication operators all have the same precedence, and
addition and subtraction have the same precedence.

When computing the remainder of ‘`x % y`’,
the quotient is rounded toward zero to an integer and
multiplied by y. This result is subtracted from x;
this operation is sometimes known as “trunc-mod.” The following
relation always holds:

```
b * int(a / b) + (a % b) == a
```

One possibly undesirable effect of this definition of remainder is that
‘`x % y`’ is negative if x is negative. Thus:

```
-17 % 8 = -1
```

This definition is compliant with the POSIX standard, which says that the `%`
operator produces results equivalent to using the standard C
`fmod()` function, and that function in turn works as just
described.

In other `awk` implementations, the signedness of the remainder
may be machine-dependent.

> **NOTE:** The POSIX standard only specifies the use of ‘`^`’
> for exponentiation.
> For maximum portability, do not use the ‘`**`’ operator.

---

Next: [Assignment Expressions](#Assignment-Ops), Previous: [Arithmetic Operators](#Arithmetic-Ops), Up: [Operators: Doing Something with Values](#All-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.2.2 String Concatenation [¶](#String-Concatenation)

> *It seemed like a good idea at the time.*

— *Brian Kernighan*

There is only one string operation: concatenation. It does not have a
specific operator to represent it. Instead, concatenation is performed by
writing expressions next to one another, with no operator. For example:

```
$ awk '{ print "Field number one: " $1 }' mail-list
-| Field number one: Amelia
-| Field number one: Anthony
...
```

Without the space in the string constant after the ‘`:`’, the line
runs together. For example:

```
$ awk '{ print "Field number one:" $1 }' mail-list
-| Field number one:Amelia
-| Field number one:Anthony
...
```

Because string concatenation does not have an explicit operator, it is
often necessary to ensure that it happens at the right time by using
parentheses to enclose the items to concatenate. For example,
you might expect that the
following code fragment concatenates `file` and `name`:

```
file = "file"
name = "name"
print "something meaningful" > file name
```

This produces a syntax error with some versions of Unix
`awk`.[36](#FOOT36)
It is necessary to use the following:

```
print "something meaningful" > (file name)
```

Parentheses should be used around concatenation in all but the
most common contexts, such as on the righthand side of ‘`=`’.
Be careful about the kinds of expressions used in string concatenation.
In particular, the order of evaluation of expressions used for concatenation
is undefined in the `awk` language. Consider this example:

```
BEGIN {
    a = "don't"
    print (a " " (a = "panic"))
}
```

It is not defined whether the second assignment to `a` happens
before or after the value of `a` is retrieved for producing the
concatenated value. The result could be either ‘`don't panic`’,
or ‘`panic panic`’.

The precedence of concatenation, when mixed with other operators, is often
counter-intuitive. Consider this example:

```
$ awk 'BEGIN { print -12 " " -24 }'
-| -12-24
```

This “obviously” is concatenating −12, a space, and −24.
But where did the space disappear to?
The answer lies in the combination of operator precedences and
`awk`’s automatic conversion rules. To get the desired result,
write the program this way:

```
$ awk 'BEGIN { print -12 " " (-24) }'
-| -12 -24
```

This forces `awk` to treat the ‘`-`’ on the ‘`-24`’ as unary.
Otherwise, it’s parsed as follows:

```
    −12 (" " − 24)
⇒ −12 (0 − 24)
⇒ −12 (−24)
⇒ −12−24
```

As mentioned earlier,
when mixing concatenation with other operators, *parenthesize*. Otherwise,
you’re never quite sure what you’ll get.

---

Next: [Increment and Decrement Operators](#Increment-Ops), Previous: [String Concatenation](#Concatenation), Up: [Operators: Doing Something with Values](#All-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.2.3 Assignment Expressions [¶](#Assignment-Expressions)

An *assignment* is an expression that stores a (usually different)
value into a variable. For example, let’s assign the value one to the variable
`z`:

```
z = 1
```

After this expression is executed, the variable `z` has the value one.
Whatever old value `z` had before the assignment is forgotten.

Assignments can also store string values. For example, the
following stores
the value `"this food is good"` in the variable `message`:

```
thing = "food"
predicate = "good"
message = "this " thing " is " predicate
```

This also illustrates string concatenation.
The ‘`=`’ sign is called an *assignment operator*. It is the
simplest assignment operator because the value of the righthand
operand is stored unchanged.
Most operators (addition, concatenation, and so on) have no effect
except to compute a value. If the value isn’t used, there’s no reason to
use the operator. An assignment operator is different; it does
produce a value, but even if you ignore it, the assignment still
makes itself felt through the alteration of the variable. We call this
a *side effect*.

The lefthand operand of an assignment need not be a variable
(see [Variables](#Variables)); it can also be a field
(see [Changing the Contents of a Field](#Changing-Fields)) or
an array element (see [Arrays in `awk`](#Arrays)).
These are all called *lvalues*,
which means they can appear on the lefthand side of an assignment operator.
The righthand operand may be any expression; it produces the new value
that the assignment stores in the specified variable, field, or array
element. (Such values are called *rvalues*.)

It is important to note that variables do *not* have permanent types.
A variable’s type is simply the type of whatever value was last assigned
to it. In the following program fragment, the variable
`foo` has a numeric value at first, and a string value later on:

```
foo = 1
print foo
```

```
foo = "bar"
print foo
```

When the second assignment gives `foo` a string value, the fact that
it previously had a numeric value is forgotten.

String values that do not begin with a digit have a numeric value of
zero. After executing the following code, the value of `foo` is five:

```
foo = "a string"
foo = foo + 5
```

> **NOTE:** Using a variable as a number and then later as a string
> can be confusing and is poor programming style. The previous two examples
> illustrate how `awk` works, *not* how you should write your
> programs!

An assignment is an expression, so it has a value—the same value that
is assigned. Thus, ‘`z = 1`’ is an expression with the value one.
One consequence of this is that you can write multiple assignments together,
such as:

```
x = y = z = 5
```

This example stores the value five in all three variables
(`x`, `y`, and `z`).
It does so because the
value of ‘`z = 5`’, which is five, is stored into `y` and then
the value of ‘`y = z = 5`’, which is five, is stored into `x`.

Assignments may be used anywhere an expression is called for. For
example, it is valid to write ‘`x != (y = 1)`’ to set `y` to one,
and then test whether `x` equals one. But this style tends to make
programs hard to read; such nesting of assignments should be avoided,
except perhaps in a one-shot program.

Aside from ‘`=`’, there are several other assignment operators that
do arithmetic with the old value of the variable. For example, the
operator ‘`+=`’ computes a new value by adding the righthand value
to the old value of the variable. Thus, the following assignment adds
five to the value of `foo`:

```
foo += 5
```

This is equivalent to the following:

```
foo = foo + 5
```

Use whichever makes the meaning of your program clearer.

There are situations where using ‘`+=`’ (or any assignment operator)
is *not* the same as simply repeating the lefthand operand in the
righthand expression. For example:

```
# Thanks to Pat Rankin for this example
BEGIN  {
    foo[rand()] += 5
    for (x in foo)
       print x, foo[x]
```

```

```

```
    bar[rand()] = bar[rand()] + 5
    for (x in bar)
       print x, bar[x]
}
```

The indices of `bar` are practically guaranteed to be different, because
`rand()` returns different values each time it is called.
(Arrays and the `rand()` function haven’t been covered yet.
See [Arrays in `awk`](#Arrays),
and
see [Numeric Functions](#Numeric-Functions)
for more information.)
This example illustrates an important fact about assignment
operators: the lefthand expression is only evaluated *once*.

It is up to the implementation as to which expression is evaluated
first, the lefthand or the righthand.
Consider this example:

```
i = 1
a[i += 2] = i + 1
```

The value of `a[3]` could be either two or four.

[Table 6.2](#table_002dassign_002dops) lists the arithmetic assignment operators. In each
case, the righthand operand is an expression whose value is converted
to a number.

| Operator | Effect |
| --- | --- |
| lvalue `+=` increment | Add increment to the value of lvalue. |
| lvalue `-=` decrement | Subtract decrement from the value of lvalue. |
| lvalue `*=` coefficient | Multiply the value of lvalue by coefficient. |
| lvalue `/=` divisor | Divide the value of lvalue by divisor. |
| lvalue `%=` modulus | Set lvalue to its remainder by modulus. |
| lvalue `^=` power | Raise lvalue to the power power. |
| lvalue `**=` power | Raise lvalue to the power power. (c.e.) |

**Table 6.2:** Arithmetic assignment operators

> **NOTE:** Only the ‘`^=`’ operator is specified by POSIX.
> For maximum portability, do not use the ‘`**=`’ operator.

| Syntactic Ambiguities Between ‘`/=`’ and Regular Expressions |
| --- |
| There is a syntactic ambiguity between the `/=` assignment operator and regexp constants whose first character is an ‘`=`’. (d.c.) This is most notable in some commercial `awk` versions. For example:  ``` $ awk /==/ /dev/null error→ awk: syntax error at source line 1 error→  context is error→         >>> /= <<< error→ awk: bailing out at source line 1 ```  A workaround is:  ``` awk '/[=]=/' /dev/null ```  `gawk` does not have this problem; BWK `awk` and `mawk` also do not. |

---

Previous: [Assignment Expressions](#Assignment-Ops), Up: [Operators: Doing Something with Values](#All-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.2.4 Increment and Decrement Operators [¶](#Increment-and-Decrement-Operators)

*Increment* and *decrement operators* increase or decrease the value of
a variable by one. An assignment operator can do the same thing, so
the increment operators add no power to the `awk` language; however, they
are convenient abbreviations for very common operations.

The operator used for adding one is written ‘`++`’. It can be used to increment
a variable either before or after taking its value.
To *pre-increment* a variable `v`, write ‘`++v`’. This adds
one to the value of `v`—that new value is also the value of the
expression. (The assignment expression ‘`v += 1`’ is completely equivalent.)
Writing the ‘`++`’ after the variable specifies *post-increment*. This
increments the variable value just the same; the difference is that the
value of the increment expression itself is the variable’s *old*
value. Thus, if `foo` has the value four, then the expression ‘`foo++`’
has the value four, but it changes the value of `foo` to five.
In other words, the operator returns the old value of the variable,
but with the side effect of incrementing it.

The post-increment ‘`foo++`’ is nearly the same as writing ‘`(foo
+= 1) - 1`’. It is not perfectly equivalent because all numbers in
`awk` are floating point—in floating point, ‘`foo + 1 - 1`’ does
not necessarily equal `foo`. But the difference is minute as
long as you stick to numbers that are fairly small (less than
1012).

Fields and array elements are incremented
just like variables. (Use ‘`$(i++)`’ when you want to do a field reference
and a variable increment at the same time. The parentheses are necessary
because of the precedence of the field reference operator ‘`$`’.)

The decrement operator ‘`--`’ works just like ‘`++`’, except that
it subtracts one instead of adding it. As with ‘`++`’, it can be used before
the lvalue to pre-decrement or after it to post-decrement.
Following is a summary of increment and decrement expressions:

`++lvalue`
:   Increment lvalue, returning the new value as the
    value of the expression.

`lvalue++`
:   Increment lvalue, returning the *old* value of lvalue
    as the value of the expression.

`--lvalue`
:   Decrement lvalue, returning the new value as the
    value of the expression.
    (This expression is
    like ‘`++lvalue`’, but instead of adding, it subtracts.)

`lvalue--`
:   Decrement lvalue, returning the *old* value of lvalue
    as the value of the expression.
    (This expression is
    like ‘`lvalue++`’, but instead of adding, it subtracts.)

| Operator Evaluation Order |
| --- |
| *Doctor, it hurts when I do this!  Then don’t do that!*  — *Groucho Marx*  What happens for something like the following?  ``` b = 6 print b += b++ ```  Or something even stranger?  ``` b = 6 b += ++b + b++ print b ```  In other words, when do the various side effects prescribed by the postfix operators (‘`b++`’) take effect? When side effects happen is *implementation-defined*. In other words, it is up to the particular version of `awk`. The result for the first example may be 12 or 13, and for the second, it may be 22 or 23.  In short, doing things like this is not recommended and definitely not anything that you can rely upon for portability. You should avoid such things in your own programs. |

---

Next: [Function Calls](#Function-Calls), Previous: [Operators: Doing Something with Values](#All-Operators), Up: [Expressions](#Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.3 Truth Values and Conditions [¶](#Truth-Values-and-Conditions-1)

In certain contexts, expression values also serve as “truth values”; i.e.,
they determine what should happen next as the program runs. This
section describes how `awk` defines “true” and “false”
and how values are compared.

* [True and False in `awk`](#Truth-Values)
* [Variable Typing and Comparison Expressions](#Typing-and-Comparison)
* [Boolean Expressions](#Boolean-Ops)
* [Conditional Expressions](#Conditional-Exp)

---

Next: [Variable Typing and Comparison Expressions](#Typing-and-Comparison), Up: [Truth Values and Conditions](#Truth-Values-and-Conditions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.3.1 True and False in `awk` [¶](#True-and-False-in-awk)

Many programming languages have a special representation for the concepts
of “true” and “false.” Such languages usually use the special
constants `true` and `false`, or perhaps their uppercase
equivalents.
However, `awk` is different.
It borrows a very simple concept of true and
false from C. In `awk`, any nonzero numeric value *or* any
nonempty string value is true. Any other value (zero or the null
string, `""`) is false. The following program prints ‘`A strange
truth value`’ three times:

```
BEGIN {
   if (3.1415927)
       print "A strange truth value"
   if ("Four Score And Seven Years Ago")
       print "A strange truth value"
   if (j = 57)
       print "A strange truth value"
}
```

There is a surprising consequence of the “nonzero or non-null” rule:
the string constant `"0"` is actually true, because it is non-null.
(d.c.)

---

Next: [Boolean Expressions](#Boolean-Ops), Previous: [True and False in `awk`](#Truth-Values), Up: [Truth Values and Conditions](#Truth-Values-and-Conditions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.3.2 Variable Typing and Comparison Expressions [¶](#Variable-Typing-and-Comparison-Expressions)

> *The Guide is definitive. Reality is frequently inaccurate.*

— *Douglas Adams, The Hitchhiker’s Guide to the Galaxy*

Unlike in other programming languages, in `awk` variables do not have a
fixed type. Instead, they can be either a number or a string, depending
upon the value that is assigned to them.
We look now at how variables are typed, and how `awk`
compares variables.

* [String Type versus Numeric Type](#Variable-Typing)
* [Comparison Operators](#Comparison-Operators)
* [String Comparison Based on Locale Collating Order](#POSIX-String-Comparison)

---

Next: [Comparison Operators](#Comparison-Operators), Up: [Variable Typing and Comparison Expressions](#Typing-and-Comparison)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.3.2.1 String Type versus Numeric Type [¶](#String-Type-versus-Numeric-Type)

Scalar objects in `awk` (variables, array elements, and fields)
are *dynamically* typed. This means their type can change as the
program runs, from *untyped* before any use,[37](#FOOT37) to string
or number, and then from string to number or number to string, as the
program progresses. (`gawk` also provides regexp-typed scalars,
but let’s ignore that for now; see [Strongly Typed Regexp Constants](#Strong-Regexp-Constants).)

You can’t do much with untyped variables, other than tell that they
are untyped. The following program tests `a` against `""`
and `0`; the test succeeds when `a` has never been assigned
a value. It also uses the built-in `typeof()` function
(not presented yet; see [Getting Type Information](#Type-Functions)) to show `a`’s type:

```
$ gawk 'BEGIN { print (a == "" && a == 0 ?
> "a is untyped" : "a has a type!") ; print typeof(a) }'
-| a is untyped
-| unassigned
```

A scalar has numeric type when assigned a numeric value,
such as from a numeric constant, or from another scalar
with numeric type:

```
$ gawk 'BEGIN { a = 42 ; print typeof(a)
> b = a ; print typeof(b) }'
number
number
```

Similarly, a scalar has string type when assigned a string
value, such as from a string constant, or from another scalar
with string type:

```
$ gawk 'BEGIN { a = "forty two" ; print typeof(a)
> b = a ; print typeof(b) }'
string
string
```

So far, this is all simple and straightforward. What happens, though,
when `awk` has to process data from a user? Let’s start with
field data. What should the following command produce as output?

```
echo hello | awk '{ printf("%s %s < 42\n", $1,
                           ($1 < 42 ? "is" : "is not")) }'
```

Since ‘`hello`’ is alphabetic data, `awk` can only do a string
comparison. Internally, it converts `42` into `"42"` and compares
the two string values `"hello"` and `"42"`. Here’s the result:

```
$ echo hello | awk '{ printf("%s %s < 42\n", $1,
>                            ($1 < 42 ? "is" : "is not")) }'
-| hello is not < 42
```

However, what happens when data from a user *looks like* a number?
On the one hand, in reality, the input data consists of characters, not
binary numeric
values. But, on the other hand, the data looks numeric, and `awk`
really ought to treat it as such. And indeed, it does:

```
$ echo 37 | awk '{ printf("%s %s < 42\n", $1,
>                         ($1 < 42 ? "is" : "is not")) }'
-| 37 is < 42
```

Here are the rules for when `awk`
treats data as a number, and for when it treats data as a string.

The POSIX standard uses the term *numeric string* for input data that
looks numeric. The ‘`37`’ in the previous example is a numeric string.
So what is the type of a numeric string? Answer: numeric.

The type of a variable is important because the types of two variables
determine how they are compared.
Variable typing follows these definitions and rules:

* A numeric constant or the result of a numeric operation has the *numeric*
  attribute.
* A string constant or the result of a string operation has the *string*
  attribute.
* Fields, `getline` input, `FILENAME`, `ARGV` elements,
  `ENVIRON` elements, and the elements of an array created by
  `match()`, `split()`, and `patsplit()` that are numeric
  strings have the *strnum* attribute.[38](#FOOT38)
  Otherwise, they have
  the *string* attribute. Uninitialized variables also have the
  *strnum* attribute.
* Attributes propagate across assignments but are not changed by
  any use.

The last rule is particularly important. In the following program,
`a` has numeric type, even though it is later used in a string
operation:

```
BEGIN {
     a = 12.345
     b = a " is a cute number"
     print b
}
```

When two operands are compared, either string comparison or numeric comparison
may be used. This depends upon the attributes of the operands, according to the
following symmetric matrix:

```
        +----------------------------------------------
        |       STRING          NUMERIC         STRNUM
--------+----------------------------------------------
        |
STRING  |       string          string          string
        |
NUMERIC |       string          numeric         numeric
        |
STRNUM  |       string          numeric         numeric
--------+----------------------------------------------
```

The basic idea is that user input that looks numeric—and *only*
user input—should be treated as numeric, even though it is actually
made of characters and is therefore also a string.
Thus, for example, the string constant `" +3.14"`,
when it appears in program source code,
is a string—even though it looks numeric—and
is *never* treated as a number for comparison
purposes.

In short, when one operand is a “pure” string, such as a string
constant, then a string comparison is performed. Otherwise, a
numeric comparison is performed.
(The primary difference between a number and a strnum is that
for strnums `gawk` preserves the original string value that
the scalar had when it came in.)

This point bears additional emphasis:
Input that looks numeric *is* numeric.
All other input is treated as strings.

Thus, the six-character input string ‘`+3.14`’ receives the
strnum attribute. In contrast, the eight characters
`" +3.14"` appearing in program text comprise a string constant.
The following examples print ‘`1`’ when the comparison between
the two different constants is true, and ‘`0`’ otherwise:

```
$ echo ' +3.14' | awk '{ print($0 == " +3.14") }'    True
-| 1
$ echo ' +3.14' | awk '{ print($0 == "+3.14") }'     False
-| 0
$ echo ' +3.14' | awk '{ print($0 == "3.14") }'      False
-| 0
$ echo ' +3.14' | awk '{ print($0 == 3.14) }'        True
-| 1
$ echo ' +3.14' | awk '{ print($1 == " +3.14") }'    False
-| 0
$ echo ' +3.14' | awk '{ print($1 == "+3.14") }'     True
-| 1
$ echo ' +3.14' | awk '{ print($1 == "3.14") }'      False
-| 0
$ echo ' +3.14' | awk '{ print($1 == 3.14) }'        True
-| 1
```

You can see the type of an input field (or other user input)
using `typeof()`:

```
$ echo hello 37 | gawk '{ print typeof($1), typeof($2) }'
-| string strnum
```

---

Next: [String Comparison Based on Locale Collating Order](#POSIX-String-Comparison), Previous: [String Type versus Numeric Type](#Variable-Typing), Up: [Variable Typing and Comparison Expressions](#Typing-and-Comparison)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.3.2.2 Comparison Operators [¶](#Comparison-Operators-1)

*Comparison expressions* compare strings or numbers for
relationships such as equality. They are written using *relational
operators*, which are a superset of those in C.
[Table 6.3](#table_002drelational_002dops) describes them.

| Expression | Result |
| --- | --- |
| x `<` y | True if x is less than y |
| x `<=` y | True if x is less than or equal to y |
| x `>` y | True if x is greater than y |
| x `>=` y | True if x is greater than or equal to y |
| x `==` y | True if x is equal to y |
| x `!=` y | True if x is not equal to y |
| x `~` y | True if the string x matches the regexp denoted by y |
| x `!~` y | True if the string x does not match the regexp denoted by y |
| subscript `in` array | True if the array array has an element with the subscript subscript |

**Table 6.3:** Relational operators

Comparison expressions have the value one if true and zero if false.
When comparing operands of mixed types, numeric operands are converted
to strings using the value of `CONVFMT`
(see [Conversion of Strings and Numbers](#Conversion)).

Strings are compared
by comparing the first character of each, then the second character of each,
and so on. Thus, `"10"` is less than `"9"`. If there are two
strings where one is a prefix of the other, the shorter string is less than
the longer one. Thus, `"abc"` is less than `"abcd"`.

It is very easy to accidentally mistype the ‘`==`’ operator and
leave off one of the ‘`=`’ characters. The result is still valid
`awk` code, but the program does not do what is intended:

```
if (a = b)   # oops! should be a == b
   ...
else
   ...
```

Unless `b` happens to be zero or the null string, the `if`
part of the test always succeeds. Because the operators are
so similar, this kind of error is very difficult to spot when
scanning the source code.

The following list of expressions illustrates the kinds of comparisons
`awk` performs, as well as what the result of each comparison is:

`1.5 <= 2.0`
:   Numeric comparison (true)

`"abc" >= "xyz"`
:   String comparison (false)

`1.5 != " +2"`
:   String comparison (true)

`"1e2" < "3"`
:   String comparison (true)

`a = 2; b = "2"`

`a == b`
:   String comparison (true)

`a = 2; b = " +2"`

`a == b`
:   String comparison (false)

In this example:

```
$ echo 1e2 3 | awk '{ print ($1 < $2) ? "true" : "false" }'
-| false
```

the result is ‘`false`’ because both `$1` and `$2`
are user input. They are numeric strings—therefore both have
the strnum attribute, dictating a numeric comparison.
The purpose of the comparison rules and the use of numeric strings is
to attempt to produce the behavior that is “least surprising,” while
still “doing the right thing.”

String comparisons and regular expression comparisons are very different.
For example:

```
x == "foo"
```

has the value one, or is true if the variable `x`
is precisely ‘`foo`’. By contrast:

```
x ~ /foo/
```

has the value one if `x` contains ‘`foo`’, such as
`"Oh, what a fool am I!"`.

The righthand operand of the ‘`~`’ and ‘`!~`’ operators may be
either a regexp constant (`/`…`/`) or an ordinary
expression. In the latter case, the value of the expression as a string is used as a
dynamic regexp (see [How to Use Regular Expressions](#Regexp-Usage); also
see [Using Dynamic Regexps](#Computed-Regexps)).

A constant regular
expression in slashes by itself is also an expression.
`/regexp/` is an abbreviation for the following comparison expression:

```
$0 ~ /regexp/
```

One special place where `/foo/` is *not* an abbreviation for
‘`$0 ~ /foo/`’ is when it is the righthand operand of ‘`~`’ or
‘`!~`’.
See [Using Regular Expression Constants](#Using-Constant-Regexps),
where this is discussed in more detail.

---

Previous: [Comparison Operators](#Comparison-Operators), Up: [Variable Typing and Comparison Expressions](#Typing-and-Comparison)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.3.2.3 String Comparison Based on Locale Collating Order [¶](#String-Comparison-Based-on-Locale-Collating-Order)

The POSIX standard used to say that all string comparisons are
performed based on the locale’s *collating order*. This
is the order in which characters sort, as defined by the locale
(for more discussion, see [Where You Are Makes a Difference](#Locales)). This order is usually very
different from the results obtained when doing straight byte-by-byte
comparison.[39](#FOOT39)

Because this behavior differs considerably from existing practice,
`gawk` only implemented it when in POSIX mode (see [Command-Line Options](#Options)).
Here is an example to illustrate the difference, in an `en_US.UTF-8`
locale:

```
$ gawk 'BEGIN { printf("ABC < abc = %s\n",
>                     ("ABC" < "abc" ? "TRUE" : "FALSE")) }'
-| ABC < abc = TRUE
$ gawk --posix 'BEGIN { printf("ABC < abc = %s\n",
>                             ("ABC" < "abc" ? "TRUE" : "FALSE")) }'
-| ABC < abc = FALSE
```

Fortunately, as of August 2016, comparison based on locale
collating order is no longer required for the `==` and `!=`
operators.[40](#FOOT40) However, comparison based on locales is still
required for `<`, `<=`, `>`, and `>=`. POSIX thus
recommends as follows:

> Since the `==` operator checks whether strings are identical,
> not whether they collate equally, applications needing to check whether
> strings collate equally can use:
>
> ```
> a <= b && a >= b
> ```

As of version 4.2, `gawk` continues to use locale
collating order for `<`, `<=`, `>`, and `>=` only
in POSIX mode.

---

Next: [Conditional Expressions](#Conditional-Exp), Previous: [Variable Typing and Comparison Expressions](#Typing-and-Comparison), Up: [Truth Values and Conditions](#Truth-Values-and-Conditions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.3.3 Boolean Expressions [¶](#Boolean-Expressions)

A *Boolean expression* is a combination of comparison expressions or
matching expressions, using the Boolean operators “or”
(‘`||`’), “and” (‘`&&`’), and “not” (‘`!`’), along with
parentheses to control nesting. The truth value of the Boolean expression is
computed by combining the truth values of the component expressions.
Boolean expressions are also referred to as *logical expressions*.
The terms are equivalent.

Boolean expressions can be used wherever comparison and matching
expressions can be used. They can be used in `if`, `while`,
`do`, and `for` statements
(see [Control Statements in Actions](#Statements)).
They have numeric values (one if true, zero if false) that come into play
if the result of the Boolean expression is stored in a variable or
used in arithmetic.

In addition, every Boolean expression is also a valid pattern, so
you can use one as a pattern to control the execution of rules.
The Boolean operators are:

`boolean1 && boolean2`
:   True if both boolean1 and boolean2 are true. For example,
    the following statement prints the current input record if it contains
    both ‘`edu`’ and ‘`li`’:

    ```
    if ($0 ~ /edu/ && $0 ~ /li/) print
    ```

    The subexpression boolean2 is evaluated only if boolean1
    is true. This can make a difference when boolean2 contains
    expressions that have side effects. In the case of ‘`$0 ~ /foo/ &&
    ($2 == bar++)`’, the variable `bar` is not incremented if there is
    no substring ‘`foo`’ in the record.

`boolean1 || boolean2`
:   True if at least one of boolean1 or boolean2 is true.
    For example, the following statement prints all records in the input
    that contain *either* ‘`edu`’ or
    ‘`li`’:

    ```
    if ($0 ~ /edu/ || $0 ~ /li/) print
    ```

    The subexpression boolean2 is evaluated only if boolean1
    is false. This can make a difference when boolean2 contains
    expressions that have side effects.
    (Thus, this test never really distinguishes records that contain both
    ‘`edu`’ and ‘`li`’—as soon as ‘`edu`’ is matched,
    the full test succeeds.)

`! boolean`
:   True if boolean is false. For example,
    the following program prints ‘`no home!`’ in
    the unusual event that the `HOME` environment
    variable is not defined:

    ```
    BEGIN { if (! ("HOME" in ENVIRON))
                print "no home!" }
    ```

    (The `in` operator is described in
    [Referring to an Array Element](#Reference-to-Elements).)

The ‘`&&`’ and ‘`||`’ operators are called *short-circuit*
operators because of the way they work. Evaluation of the full expression
is “short-circuited” if the result can be determined partway through
its evaluation.

Statements that end with ‘`&&`’ or ‘`||`’ can be continued simply
by putting a newline after them. But you cannot put a newline in front
of either of these operators without using backslash continuation
(see [`awk` Statements Versus Lines](#Statements_002fLines)).

The actual value of an expression using the ‘`!`’ operator is
either one or zero, depending upon the truth value of the expression it
is applied to.
The ‘`!`’ operator is often useful for changing the sense of a flag
variable from false to true and back again. For example, the following
program is one way to print lines in between special bracketing lines:

```
$1 == "START"   { interested = ! interested; next }
interested      { print }
$1 == "END"     { interested = ! interested; next }
```

The variable `interested`, as with all `awk` variables, starts
out initialized to zero, which is also false. When a line is seen whose
first field is ‘`START`’, the value of `interested` is toggled
to true, using ‘`!`’. The next rule prints lines as long as
`interested` is true. When a line is seen whose first field is
‘`END`’, `interested` is toggled back to false.[41](#FOOT41)

Most commonly, the ‘`!`’ operator is used in the conditions of
`if` and `while` statements, where it often makes more
sense to phrase the logic in the negative:

```
if (! some condition || some other condition) {
    ... do whatever processing ...
}
```

> **NOTE:** The `next` statement is discussed in
> [The `next` Statement](#Next-Statement).
> `next` tells `awk` to skip the rest of the rules, get the
> next record, and start processing the rules over again at the top.
> The reason it’s there is to avoid printing the bracketing
> ‘`START`’ and ‘`END`’ lines.

---

Previous: [Boolean Expressions](#Boolean-Ops), Up: [Truth Values and Conditions](#Truth-Values-and-Conditions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 6.3.4 Conditional Expressions [¶](#Conditional-Expressions)

A *conditional expression* is a special kind of expression that has
three operands. It allows you to use one expression’s value to select
one of two other expressions.
The conditional expression in `awk` is the same as in the C
language, as shown here:

```
selector ? if-true-exp : if-false-exp
```

There are three subexpressions. The first, selector, is always
computed first. If it is “true” (not zero or not null), then
if-true-exp is computed next, and its value becomes the value of
the whole expression. Otherwise, if-false-exp is computed next,
and its value becomes the value of the whole expression.
For example, the following expression produces the absolute value of `x`:

```
x >= 0 ? x : -x
```

Each time the conditional expression is computed, only one of
if-true-exp and if-false-exp is used; the other is ignored.
This is important when the expressions have side effects. For example,
this conditional expression examines element `i` of either array
`a` or array `b`, and increments `i`:

```
x == y ? a[i++] : b[i++]
```

This is guaranteed to increment `i` exactly once, because each time
only one of the two increment expressions is executed
and the other is not.
See [Arrays in `awk`](#Arrays),
for more information about arrays.

As a minor `gawk` extension,
a statement that uses ‘`?:`’ can be continued simply
by putting a newline after either character.
However, putting a newline in front
of either character does not work without using backslash continuation
(see [`awk` Statements Versus Lines](#Statements_002fLines)).
If `--posix` is specified
(see [Command-Line Options](#Options)), this extension is disabled.

---

Next: [Operator Precedence (How Operators Nest)](#Precedence), Previous: [Truth Values and Conditions](#Truth-Values-and-Conditions), Up: [Expressions](#Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.4 Function Calls [¶](#Function-Calls-1)

A *function* is a name for a particular calculation.
This enables you to
ask for it by name at any point in the program. For
example, the function `sqrt()` computes the square root of a number.

A fixed set of functions are *built in*, which means they are
available in every `awk` program. The `sqrt()` function is one
of these. See [Built-in Functions](#Built_002din) for a list of built-in
functions and their descriptions. In addition, you can define
functions for use in your program.
See [User-Defined Functions](#User_002ddefined)
for instructions on how to do this.
Finally, `gawk` lets you write functions in C or C++
that may be called from your program (see [Writing Extensions for `gawk`](#Dynamic-Extensions)).

The way to use a function is with a *function call* expression,
which consists of the function name followed immediately by a list of
*arguments* in parentheses. The arguments are expressions that
provide the raw materials for the function’s calculations.
When there is more than one argument, they are separated by commas. If
there are no arguments, just write ‘`()`’ after the function name.
The following examples show function calls with and without arguments:

```
sqrt(x^2 + y^2)        one argument
atan2(y, x)            two arguments
rand()                 no arguments
```

> **CAUTION:** Do not put any space between the function name and the opening parenthesis!
> A user-defined function name looks just like the name of a
> variable—a space would make the expression look like concatenation of
> a variable with an expression inside parentheses.
> With built-in functions, space before the parenthesis is harmless, but
> it is best not to get into the habit of using space to avoid mistakes
> with user-defined functions.

Each function expects a particular number
of arguments. For example, the `sqrt()` function must be called with
a single argument, the number of which to take the square root:

```
sqrt(argument)
```

Some of the built-in functions have one or
more optional arguments.
If those arguments are not supplied, the functions
use a reasonable default value.
See [Built-in Functions](#Built_002din) for full details. If arguments
are omitted in calls to user-defined functions, then those arguments are
treated as local variables. Such local variables act like the
empty string if referenced where a string value is required,
and like zero if referenced where a numeric value is required
(see [User-Defined Functions](#User_002ddefined)).

As an advanced feature, `gawk` provides indirect function calls,
which is a way to choose the function to call at runtime, instead of
when you write the source code to your program. We defer discussion of
this feature until later; see [Indirect Function Calls](#Indirect-Calls).

Like every other expression, the function call has a value, often
called the *return value*, which is computed by the function
based on the arguments you give it. In this example, the return value
of ‘`sqrt(argument)`’ is the square root of argument.
The following program reads numbers, one number per line, and prints
the square root of each one:

```
$ awk '{ print "The square root of", $1, "is", sqrt($1) }'
1
-| The square root of 1 is 1
3
-| The square root of 3 is 1.73205
5
-| The square root of 5 is 2.23607
Ctrl-d
```

A function can also have side effects, such as assigning
values to certain variables or doing I/O.
This program shows how the `match()` function
(see [String-Manipulation Functions](#String-Functions))
changes the variables `RSTART` and `RLENGTH`:

```
{
    if (match($1, $2))
        print RSTART, RLENGTH
    else
        print "no match"
}
```

Here is a sample run:

```
$ awk -f matchit.awk
aaccdd  c+
-| 3 2
foo     bar
-| no match
abcdefg e
-| 5 1
```

---

Next: [Where You Are Makes a Difference](#Locales), Previous: [Function Calls](#Function-Calls), Up: [Expressions](#Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.5 Operator Precedence (How Operators Nest) [¶](#Operator-Precedence-_0028How-Operators-Nest_0029)

*Operator precedence* determines how operators are grouped when
different operators appear close by in one expression. For example,
‘`*`’ has higher precedence than ‘`+`’; thus, ‘`a + b * c`’
means to multiply `b` and `c`, and then add `a` to the
product (i.e., ‘`a + (b * c)`’).

The normal precedence of the operators can be overruled by using parentheses.
Think of the precedence rules as saying where the
parentheses are assumed to be. In
fact, it is wise to always use parentheses whenever there is an unusual
combination of operators, because other people who read the program may
not remember what the precedence is in this case.
Even experienced programmers occasionally forget the exact rules,
which leads to mistakes.
Explicit parentheses help prevent
any such mistakes.

When operators of equal precedence are used together, the leftmost
operator groups first, except for the assignment, conditional, and
exponentiation operators, which group in the opposite order.
Thus, ‘`a - b + c`’ groups as ‘`(a - b) + c`’ and
‘`a = b = c`’ groups as ‘`a = (b = c)`’.

Normally the precedence of prefix unary operators does not matter,
because there is only one way to interpret
them: innermost first. Thus, ‘`$++i`’ means ‘`$(++i)`’ and
‘`++$x`’ means ‘`++($x)`’. However, when another operator follows
the operand, then the precedence of the unary operators can matter.
‘`$x^2`’ means ‘`($x)^2`’, but ‘`-x^2`’ means
‘`-(x^2)`’, because ‘`-`’ has lower precedence than ‘`^`’,
whereas ‘`$`’ has higher precedence.
Also, operators cannot be combined in a way that violates the
precedence rules; for example, ‘`$$0++--`’ is not a valid
expression because the first ‘`$`’ has higher precedence than the
‘`++`’; to avoid the problem the expression can be rewritten as
‘`$($0++)--`’.

This list presents `awk`’s operators, in order of highest
to lowest precedence:

`(`…`)`
:   Grouping.

`$`
:   Field reference.

`++ --`
:   Increment, decrement.

`^ **`
:   Exponentiation. These operators group right to left.

`+ - !`
:   Unary plus, minus, logical “not.”

`* / %`
:   Multiplication, division, remainder.

`+ -`
:   Addition, subtraction.

String concatenation
:   There is no special symbol for concatenation.
    The operands are simply written side by side
    (see [String Concatenation](#Concatenation)).

`< <= == != > >= >> | |&`
:   Relational and redirection.
    The relational operators and the redirections have the same precedence
    level. Characters such as ‘`>`’ serve both as relationals and as
    redirections; the context distinguishes between the two meanings.

    Note that the I/O redirection operators in `print` and `printf`
    statements belong to the statement level, not to expressions. The
    redirection does not produce an expression that could be the operand of
    another operator. As a result, it does not make sense to use a
    redirection operator near another operator of lower precedence without
    parentheses. Such combinations (e.g., ‘`print foo > a ? b : c`’)
    result in syntax errors.
    The correct way to write this statement is ‘`print foo > (a ? b : c)`’.

`~ !~`
:   Matching, nonmatching.

`in` [¶](#index-in-operator-1)
:   Array membership.

`&&`
:   Logical “and.”

`||`
:   Logical “or.”

`?:`
:   Conditional. This operator groups right to left.

`= += -= *= /= %= ^= **=`
:   Assignment. These operators group right to left.

> **NOTE:** The ‘`|&`’, ‘`**`’, and ‘`**=`’ operators are not specified by POSIX.
> For maximum portability, do not use them.

---

Next: [Summary](#Expressions-Summary), Previous: [Operator Precedence (How Operators Nest)](#Precedence), Up: [Expressions](#Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.6 Where You Are Makes a Difference [¶](#Where-You-Are-Makes-a-Difference)

Modern systems support the notion of *locales*: a way to tell the
system about the local character set and language. The ISO C standard
defines a default `"C"` locale, which is an environment that is
typical of what many C programmers are used to.

Once upon a time, the locale setting used to affect regexp matching,
but this is no longer true (see [Regexp Ranges and Locales: A Long Sad Story](#Ranges-and-Locales)).

Locales can affect record splitting. For the normal case of ‘`RS =
"\n"`’, the locale is largely irrelevant. For other single-character
record separators, setting ‘`LC_ALL=C`’ in the environment will
give you much better performance when reading records. Otherwise,
`gawk` has to make several function calls, *per input
character*, to find the record terminator.

Locales can affect how dates and times are formatted (see [Time Functions](#Time-Functions)). For example, a common way to abbreviate the date September
4, 2015, in the United States is “9/4/15.” In many countries in
Europe, however, it is abbreviated “4.9.15.” Thus, the ‘`%x`’
specification in a `"US"` locale might produce ‘`9/4/15`’,
while in a `"EUROPE"` locale, it might produce ‘`4.9.15`’.

According to POSIX, string comparison is also affected by locales (similar
to regular expressions). The details are presented in [String Comparison Based on Locale Collating Order](#POSIX-String-Comparison).

Finally, the locale affects the value of the decimal point character
used when `gawk` parses input data. This is discussed in detail
in [Conversion of Strings and Numbers](#Conversion).

---

Previous: [Where You Are Makes a Difference](#Locales), Up: [Expressions](#Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.7 Summary [¶](#Summary-5)

* Expressions are the basic elements of computation in programs. They are
  built from constants, variables, function calls, and combinations of the
  various kinds of values with operators.
* `awk` supplies three kinds of constants: numeric, string, and
  regexp. `gawk` lets you specify numeric constants in octal
  and hexadecimal (bases 8 and 16) as well as decimal (base 10).
  In certain contexts, a standalone regexp constant such as `/foo/`
  has the same meaning as ‘`$0 ~ /foo/`’.
* Variables hold values between uses in computations. A number of built-in
  variables provide information to your `awk` program, and a number
  of others let you control how `awk` behaves.
* Numbers are automatically converted to strings, and strings to numbers,
  as needed by `awk`. Numeric values are converted as if they were
  formatted with `sprintf()` using the format in `CONVFMT`.
  Locales can influence the conversions.
* `awk` provides the usual arithmetic operators (addition,
  subtraction, multiplication, division, modulus), and unary plus and minus.
  It also provides comparison operators, Boolean operators, an array membership
  testing operator, and regexp
  matching operators. String concatenation is accomplished by placing
  two expressions next to each other; there is no explicit operator.
  The three-operand ‘`?:`’ operator provides an “if-else” test within
  expressions.
* Assignment operators provide convenient shorthands for common arithmetic
  operations.
* In `awk`, a value is considered to be true if it is nonzero
  *or* non-null. Otherwise, the value is false.
* A variable’s type is set upon each assignment and may change over its
  lifetime. The type determines how it behaves in comparisons (string
  or numeric).
* Function calls return a value that may be used as part of a larger
  expression. Expressions used to pass parameter values are fully
  evaluated before the function is called. `awk` provides
  built-in and user-defined functions; this is described in
  [Functions](#Functions).
* Operator precedence specifies the order in which operations are performed,
  unless explicitly overridden by parentheses. `awk`’s operator
  precedence is compatible with that of C.
* Locales can affect the format of data as output by an `awk`
  program, and occasionally the format for data read as input.

---

Next: [Arrays in `awk`](#Arrays), Previous: [Expressions](#Expressions), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 7 Patterns, Actions, and Variables [¶](#Patterns_002c-Actions_002c-and-Variables)

As you have already seen, each `awk` statement consists of
a pattern with an associated action. This chapter describes how
you build patterns and actions, what kinds of things you can do within
actions, and `awk`’s predefined variables.

The pattern–action rules and the statements available for use
within actions form the core of `awk` programming.
In a sense, everything covered
up to here has been the foundation
that programs are built on top of. Now it’s time to start
building something useful.

* [Pattern Elements](#Pattern-Overview)
* [Using Shell Variables in Programs](#Using-Shell-Variables)
* [Actions](#Action-Overview)
* [Control Statements in Actions](#Statements)
* [Predefined Variables](#Built_002din-Variables)
* [Summary](#Pattern-Action-Summary)

---

Next: [Using Shell Variables in Programs](#Using-Shell-Variables), Up: [Patterns, Actions, and Variables](#Patterns-and-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
