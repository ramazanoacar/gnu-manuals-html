### 9.2 User-Defined Functions [¶](#User_002dDefined-Functions)

Complicated `awk` programs can often be simplified by defining
your own functions. User-defined functions can be called just like
built-in ones (see [Function Calls](#Function-Calls)), but it is up to you to define
them (i.e., to tell `awk` what they should do).

* [Function Definition Syntax](#Definition-Syntax)
* [Function Definition Examples](#Function-Example)
* [Calling User-Defined Functions](#Function-Calling)
* [The `return` Statement](#Return-Statement)

---

Next: [Function Definition Examples](#Function-Example), Up: [User-Defined Functions](#User_002ddefined)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.2.1 Function Definition Syntax [¶](#Function-Definition-Syntax)

> *It’s entirely fair to say that the awk syntax for local
> variable definitions is appallingly awful.*

— *Brian Kernighan*

Definitions of functions can appear anywhere between the rules of an
`awk` program. Thus, the general form of an `awk` program is
extended to include sequences of rules *and* user-defined function
definitions.
There is no need to put the definition of a function
before all uses of the function. This is because `awk` reads the
entire program before starting to execute any of it.

The definition of a function named name looks like this:

```
function name([parameter-list])
{
     body-of-function
}
```

Here, name is the name of the function to define. A valid function
name is like a valid variable name: a sequence of letters, digits, and
underscores that doesn’t start with a digit.
Here too, only the 52 upper- and lowercase English letters may
be used in a function name.
Within a single `awk` program, any particular name can only be
used as a variable, array, or function.

parameter-list is an optional list of the function’s arguments and local
variable names, separated by commas. When the function is called,
the argument names are used to hold the argument values given in
the call.

A function cannot have two parameters with the same name, nor may it
have a parameter with the same name as the function itself.

> **CAUTION:** According to the POSIX standard, function parameters
> cannot have the same name as one of the special predefined variables
> (see [Predefined Variables](#Built_002din-Variables)), nor may a function parameter have the
> same name as another function.
>
> Not all versions of `awk` enforce
> these restrictions. (d.c.)
> `gawk` always enforces the first restriction.
> With `--posix` (see [Command-Line Options](#Options)),
> it also enforces the second restriction.

Local variables act like the empty string if referenced where a string
value is required, and like zero if referenced where a numeric value
is required. This is the same as the behavior of regular variables that have never been
assigned a value. (There is more to understand about local variables;
see [Variable Typing Is Dynamic](#Dynamic-Typing).)

The body-of-function consists of `awk` statements. It is the
most important part of the definition, because it says what the function
should actually *do*. The argument names exist to give the body a
way to talk about the arguments; local variables exist to give the body
places to keep temporary values.

Argument names are not distinguished syntactically from local variable
names. Instead, the number of arguments supplied when the function is
called determines how many argument variables there are. Thus, if three
argument values are given, the first three names in parameter-list
are arguments and the rest are local variables.

It follows that if the number of arguments is not the same in all calls
to the function, some of the names in parameter-list may be
arguments on some occasions and local variables on others. Another
way to think of this is that omitted arguments default to the
null string.

Usually when you write a function, you know how many names you intend to
use for arguments and how many you intend to use as local variables. It is
conventional to place some extra space between the arguments and
the local variables, in order to document how your function is supposed to be used.

During execution of the function body, the arguments and local variable
values hide, or *shadow*, any variables of the same names used in the
rest of the program. The shadowed variables are not accessible in the
function definition, because there is no way to name them while their
names have been taken away for the arguments and local variables. All other variables
used in the `awk` program can be referenced or set normally in the
function’s body.
(Also, see [A Note On Shadowed Variables](#Shadowed-Variables).)

The arguments and local variables last only as long as the function body
is executing. Once the body finishes, you can once again access the
variables that were shadowed while the function was running.

The function body can contain expressions that call functions. They
can even call this function, either directly or by way of another
function. When this happens, we say the function is *recursive*.
The act of a function calling itself is called *recursion*.

All the built-in functions return a value to their caller.
User-defined functions can do so also, using the `return` statement,
which is described in detail in [The `return` Statement](#Return-Statement).
Many of the subsequent examples in this section use
the `return` statement.

In many `awk` implementations, including `gawk`,
the keyword `function` may be
abbreviated `func`. (c.e.)
However, POSIX only specifies the use of
the keyword `function`. This actually has some practical implications.
If `gawk` is in POSIX-compatibility mode
(see [Command-Line Options](#Options)), then the following
statement does *not* define a function:

```
func foo() { a = sqrt($1) ; print a }
```

Instead, it defines a rule that, for each record, concatenates the value
of the variable ‘`func`’ with the return value of the function ‘`foo`’.
If the resulting string is non-null, the action is executed.
This is probably not what is desired. (`awk` accepts this input as
syntactically valid, because functions may be used before they are defined
in `awk` programs.[65](#FOOT65))

To ensure that your `awk` programs are portable, always use the
keyword `function` when defining a function.

---

Next: [Calling User-Defined Functions](#Function-Calling), Previous: [Function Definition Syntax](#Definition-Syntax), Up: [User-Defined Functions](#User_002ddefined)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.2.2 Function Definition Examples [¶](#Function-Definition-Examples)

Here is an example of a user-defined function, called `myprint()`, that
takes a number and prints it in a specific format:

```
function myprint(num)
{
     printf "%6.3g\n", num
}
```

To illustrate, here is an `awk` rule that uses our `myprint()`
function:

```
$3 > 0     { myprint($3) }
```

This program prints, in our special format, all the third fields that
contain a positive number in our input. Therefore, when given the following input:

```
 1.2   3.4    5.6   7.8
 9.10 11.12 -13.14 15.16
17.18 19.20  21.22 23.24
```

this program, using our function to format the results, prints:

```
   5.6
  21.2
```

This function deletes all the elements in an array (recall that the
extra whitespace signifies the start of the local variable list):

```
function delarray(a,    i)
{
    for (i in a)
        delete a[i]
}
```

When working with arrays, it is often necessary to delete all the elements
in an array and start over with a new list of elements
(see [The `delete` Statement](#Delete)).
Instead of having
to repeat this loop everywhere that you need to clear out
an array, your program can just call `delarray()`.
(This guarantees portability. The use of ‘`delete array`’ to delete
the contents of an entire array is a relatively recent[66](#FOOT66)
addition to the POSIX standard.)

The following is an example of a recursive function. It takes a string
as an input parameter and returns the string in reverse order.
Recursive functions must always have a test that stops the recursion.
In this case, the recursion terminates when the input string is
already empty:

```
function rev(str)
{
    if (str == "")
        return ""

    return (rev(substr(str, 2)) substr(str, 1, 1))
}
```

If this function is in a file named `rev.awk`, it can be tested
this way:

```
$ echo "Don't Panic!" |
> gawk -e '{ print rev($0) }' -f rev.awk
-| !cinaP t'noD
```

The C `ctime()` function takes a timestamp and returns it as a string,
formatted in a well-known fashion.
The following example uses the built-in `strftime()` function
(see [Time Functions](#Time-Functions))
to create an `awk` version of `ctime()`:

```
# ctime.awk
#
# awk version of C ctime(3) function

function ctime(ts,    format)
{
    format = "%a %b %e %H:%M:%S %Z %Y"

    if (ts == 0)
        ts = systime()       # use current time as default
    return strftime(format, ts)
}
```

You might think that `ctime()` could use `PROCINFO["strftime"]`
for its format string. That would be a mistake, because `ctime()` is
supposed to return the time formatted in a standard fashion, and user-level
code could have changed `PROCINFO["strftime"]`.

---

Next: [The `return` Statement](#Return-Statement), Previous: [Function Definition Examples](#Function-Example), Up: [User-Defined Functions](#User_002ddefined)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.2.3 Calling User-Defined Functions [¶](#Calling-User_002dDefined-Functions)

*Calling a function* means causing the function to run and do its job.
A function call is an expression and its value is the value returned by
the function.

* [Writing a Function Call](#Calling-A-Function)
* [Controlling Variable Scope](#Variable-Scope)
* [Passing Function Arguments by Value Or by Reference](#Pass-By-Value_002fReference)
* [Other Points About Calling Functions](#Function-Caveats)

---

Next: [Controlling Variable Scope](#Variable-Scope), Up: [Calling User-Defined Functions](#Function-Calling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.2.3.1 Writing a Function Call [¶](#Writing-a-Function-Call)

A function call consists of the function name followed by the arguments
in parentheses. `awk` expressions are what you write in the
call for the arguments. Each time the call is executed, these
expressions are evaluated, and the values become the actual arguments. For
example, here is a call to `foo()` with three arguments (the first
being a string concatenation):

```
foo(x y, "lose", 4 * z)
```

> **CAUTION:** Whitespace characters (spaces and TABs) are not allowed
> between the function name and the opening parenthesis of the argument list.
> If you write whitespace by mistake, `awk` might think that you mean
> to concatenate a variable with an expression in parentheses. However, it
> notices that you used a function name and not a variable name, and reports
> an error.

---

Next: [Passing Function Arguments by Value Or by Reference](#Pass-By-Value_002fReference), Previous: [Writing a Function Call](#Calling-A-Function), Up: [Calling User-Defined Functions](#Function-Calling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.2.3.2 Controlling Variable Scope [¶](#Controlling-Variable-Scope)

Unlike in many languages,
there is no way to make a variable local to a `{` … `}` block in
`awk`, but you can make a variable local to a function. It is
good practice to do so whenever a variable is needed only in that
function.

To make a variable local to a function, simply declare the variable as
an argument after the actual function arguments
(see [Function Definition Syntax](#Definition-Syntax)).
Look at the following example, where variable
`i` is a global variable used by both functions `foo()` and
`bar()`:

```
function bar()
{
    for (i = 0; i < 3; i++)
        print "bar's i=" i
}

function foo(j)
{
    i = j + 1
    print "foo's i=" i
    bar()
    print "foo's i=" i
}

BEGIN {
      i = 10
      print "top's i=" i
      foo(0)
      print "top's i=" i
}
```

Running this script produces the following, because the `i` in
functions `foo()` and `bar()` and at the top level refer to the same
variable instance:

```
top's i=10
foo's i=1
bar's i=0
bar's i=1
bar's i=2
foo's i=3
top's i=3
```

If you want `i` to be local to both `foo()` and `bar()`, do as
follows (the extra space before `i` is a coding convention to
indicate that `i` is a local variable, not an argument):

```
function bar(    i)
{
    for (i = 0; i < 3; i++)
        print "bar's i=" i
}

function foo(j,    i)
{
    i = j + 1
    print "foo's i=" i
    bar()
    print "foo's i=" i
}

BEGIN {
      i = 10
      print "top's i=" i
      foo(0)
      print "top's i=" i
}
```

Running the corrected script produces the following:

```
top's i=10
foo's i=1
bar's i=0
bar's i=1
bar's i=2
foo's i=1
top's i=10
```

Besides scalar values (strings and numbers), you may also have
local arrays. By using a parameter name as an array, `awk`
treats it as an array, and it is local to the function.
In addition, recursive calls create new arrays.
Consider this example:

```
function some_func(p1,      a)
{
    if (p1++ > 3)
        return
```

```
    a[p1] = p1

    some_func(p1)

    printf("At level %d, index %d %s found in a\n",
         p1, (p1 - 1), (p1 - 1) in a ? "is" : "is not")
    printf("At level %d, index %d %s found in a\n",
         p1, p1, p1 in a ? "is" : "is not")
    print ""
}

BEGIN {
    some_func(1)
}
```

When run, this program produces the following output:

```
At level 4, index 3 is not found in a
At level 4, index 4 is found in a

At level 3, index 2 is not found in a
At level 3, index 3 is found in a

At level 2, index 1 is not found in a
At level 2, index 2 is found in a
```

---

Next: [Other Points About Calling Functions](#Function-Caveats), Previous: [Controlling Variable Scope](#Variable-Scope), Up: [Calling User-Defined Functions](#Function-Calling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.2.3.3 Passing Function Arguments by Value Or by Reference [¶](#Passing-Function-Arguments-by-Value-Or-by-Reference)

In `awk`, when you declare a function, there is no way to
declare explicitly whether the arguments are passed *by value* or
*by reference*.

Instead, the passing convention is determined at runtime when
the function is called, according to the following rule:
if the argument is an array variable, then it is passed by reference.
Otherwise, the argument is passed by value.

Passing an argument by value means that when a function is called, it
is given a *copy* of the value of this argument.
The caller may use a variable as the expression for the argument, but
the called function does not know this—it only knows what value the
argument had. For example, if you write the following code:

```
foo = "bar"
z = myfunc(foo)
```

then you should not think of the argument to `myfunc()` as being
“the variable `foo`.” Instead, think of the argument as the
string value `"bar"`.
If the function `myfunc()` alters the values of its local variables,
this has no effect on any other variables. Thus, if `myfunc()`
does this:

```
function myfunc(str)
{
   print str
   str = "zzz"
   print str
}
```

to change its first argument variable `str`, it does *not*
change the value of `foo` in the caller. The role of `foo` in
calling `myfunc()` ended when its value (`"bar"`) was computed.
If `str` also exists outside of `myfunc()`, the function body
cannot alter this outer value, because it is shadowed during the
execution of `myfunc()` and cannot be seen or changed from there.

However, when arrays are the parameters to functions, they are *not*
copied. Instead, the array itself is made available for direct manipulation
by the function. This is usually termed *call by reference*.
Changes made to an array parameter inside the body of a function *are*
visible outside that function.

> **NOTE:** Changing an array parameter inside a function
> can be very dangerous if you do not watch what you are doing.
> For example:
>
> ```
> function changeit(array, ind, nvalue)
> {
>      array[ind] = nvalue
> }
>
> BEGIN {
>     a[1] = 1; a[2] = 2; a[3] = 3
>     changeit(a, 2, "two")
>     printf "a[1] = %s, a[2] = %s, a[3] = %s\n",
>             a[1], a[2], a[3]
> }
> ```
>
> prints ‘`a[1] = 1, a[2] = two, a[3] = 3`’, because
> `changeit()` stores `"two"` in the second element of `a`.

---

Previous: [Passing Function Arguments by Value Or by Reference](#Pass-By-Value_002fReference), Up: [Calling User-Defined Functions](#Function-Calling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.2.3.4 Other Points About Calling Functions [¶](#Other-Points-About-Calling-Functions)

Some `awk` implementations allow you to call a function that
has not been defined. They only report a problem at runtime, when the
program actually tries to call the function. For example:

```
BEGIN {
    if (0)
        foo()
    else
        bar()
}
function bar() { ... }
# note that `foo' is not defined
```

Because the ‘`if`’ statement will never be true, it is not really a
problem that `foo()` has not been defined. Usually, though, it is a
problem if a program calls an undefined function.

If `--lint` is specified
(see [Command-Line Options](#Options)),
`gawk` reports calls to undefined functions.

Some `awk` implementations generate a runtime
error if you use either the `next` statement
or the `nextfile` statement
(see [The `next` Statement](#Next-Statement), and
see [The `nextfile` Statement](#Nextfile-Statement))
inside a user-defined function.
`gawk` does not have this limitation.

You can call a function and pass it more parameters than it was declared
with, like so:

```
function foo(p1, p2)
{
    ...
}

BEGIN {
    foo(1, 2, 3, 4)
}
```

Doing so is bad practice, however. The called function cannot do
anything with the additional values being passed to it, so `awk`
evaluates the expressions but then just throws them away.

More importantly, such a call is confusing for whoever will next read your
program.[67](#FOOT67) Function
parameters generally are input items that influence the computation
performed by the function. Calling a function with more parameters than
it accepts gives the false impression that those values are important
to the function, when in fact they are not.

Because this is such a bad practice, `gawk` *unconditionally*
issues a warning whenever it executes such a function call. (If you
don’t like the warning, fix your code! It’s incorrect, after all.)

---

Previous: [Calling User-Defined Functions](#Function-Calling), Up: [User-Defined Functions](#User_002ddefined)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.2.4 The `return` Statement [¶](#The-return-Statement)

As seen in several earlier examples,
the body of a user-defined function can contain a `return` statement.
This statement returns control to the calling part of the `awk` program. It
can also be used to return a value for use in the rest of the `awk`
program. It looks like this:

```
return [expression]
```

The expression part is optional.
Due most likely to an oversight, POSIX does not define what the return
value is if you omit the expression. Technically speaking, this
makes the returned value undefined, and therefore, unpredictable.
In practice, though, all versions of `awk` simply return the
null string, which acts like zero if used in a numeric context.

A `return` statement without an expression is assumed at the end of
every function definition. So, if control reaches the end of the function
body, then technically the function returns an unpredictable value.
In practice, it returns the empty string. `awk`
does *not* warn you if you use the return value of such a function.

Sometimes, you want to write a function for what it does, not for
what it returns. Such a function corresponds to a `void` function
in C, C++, or Java, or to a `procedure` in Ada. Thus, it may be appropriate to not
return any value; simply bear in mind that you should not be using the
return value of such a function.

The following is an example of a user-defined function that returns a value
for the largest number among the elements of an array:

```
function maxelt(vec,   i, ret)
{
     for (i in vec) {
          if (ret == "" || vec[i] > ret)
               ret = vec[i]
     }
     return ret
}
```

You call `maxelt()` with one argument, which is an array name. The local
variables `i` and `ret` are not intended to be arguments;
there is nothing to stop you from passing more than one argument
to `maxelt()` but the results would be strange. The extra space before
`i` in the function parameter list indicates that `i` and
`ret` are local variables.
You should follow this convention when defining functions.

The following program uses the `maxelt()` function. It loads an
array, calls `maxelt()`, and then reports the maximum number in that
array:

```
function maxelt(vec,   i, ret)
{
     for (i in vec) {
          if (ret == "" || vec[i] > ret)
               ret = vec[i]
     }
     return ret
}
```

```
# Load all fields of each record into nums.
{
     for(i = 1; i <= NF; i++)
          nums[NR, i] = $i
}
```

```
END {
     print maxelt(nums)
}
```

Given the following input:

```
 1 5 23 8 16
44 3 5 2 8 26
256 291 1396 2962 100
-6 467 998 1101
99385 11 0 225
```

the program reports (predictably) that 99,385 is the largest value
in the array.

---

Next: [A Note On Shadowed Variables](#Shadowed-Variables), Previous: [User-Defined Functions](#User_002ddefined), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.3 Variable Typing Is Dynamic [¶](#Variable-Typing-Is-Dynamic)

> *It’s a desert topping!   
> It’s a floor wax!*

— *Saturday Night Live (back when it used to be funny)*

`awk` is a very fluid language.
It is possible that `awk` can’t tell if an identifier
represents a scalar variable or an array until runtime.

* [Dynamic Typing In Standard `awk`](#Dynamic-Typing-Awk)
* [Dynamic Typing In `gawk`](#Dynamic-Typing-Gawk)

---

Next: [Dynamic Typing In `gawk`](#Dynamic-Typing-Gawk), Up: [Variable Typing Is Dynamic](#Dynamic-Typing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.3.1 Dynamic Typing In Standard `awk` [¶](#Dynamic-Typing-In-Standard-awk)

Let’s first discuss standard `awk`.
Here is an annotated sample program:

```
function foo(a)
{
    a[1] = 1   # parameter is an array
}

BEGIN {
    b = 1
    foo(b)  # invalid: fatal type mismatch

    foo(x)  # x uninitialized, becomes an array dynamically
    x = 1   # now not allowed, runtime error
}
```

In this example, the first call to `foo()` generates
a fatal error, so `awk` will not report the second
error. If you comment out that call, though, then `awk`
does report the second error.

Here is a more extreme example:

```
BEGIN {
    funky(a)
    if (A == 0)
        print "<" a ">"
    else
        print a[1]
}

function funky(arr)
{
    if (A == 0)
        arr = 1
    else
        arr[1] = 1
}
```

Here, the function uses its parameter differently depending upon the
value of the global variable `A`. If `A` is zero, the
parameter `arr` is treated as a scalar. Otherwise it’s treated
as an array.

There are two ways this program might behave. `awk` could notice
that in the main program, `a` is subscripted, and so mark it as
an array before the program even begins to run. BWK `awk`, `mawk`,
and possibly others do this:

```
$ nawk -v A=0 -f funky.awk
error→ nawk: can't assign to a; it's an array name.
error→  source line number 11
$ nawk -v A=1 -f funky.awk
-| 1
```

Or `awk` could wait until runtime to set the type of `a`.
In this case, since `a` was never used before being
passed to the function, how the function uses it forces the type to
be resolved to either scalar or array. `gawk`
and the MKS `awk` do this:

```
$ gawk -v A=0 -f funky.awk
-| <>
$ gawk -v A=1 -f funky.awk 
-| 1
```

POSIX does not specify the correct behavior, so be aware that different
implementations work differently.

---

Previous: [Dynamic Typing In Standard `awk`](#Dynamic-Typing-Awk), Up: [Variable Typing Is Dynamic](#Dynamic-Typing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.3.2 Dynamic Typing In `gawk` [¶](#Dynamic-Typing-In-gawk)

> *Hc Svnt Dracones* (“Here be dragons”)

— *The Lenox Globe*

Things in `gawk` can be a little more unexpected.
Because `gawk` allows arrays of arrays, the same
dynamic typing can be applied to array elements that
have been created but not used.

```
BEGIN {
    funky(a[1])
    if (A == 0)
        print "<" a[1] ">"
    else
        print a[1][1]
}

function funky(arr)
{
    if (A == 0)
        arr = 1
    else
        arr[1] = 1
}
```

When run, the results are the same as in the earlier case:

```
$ gawk -v A=0 -f funky2.awk
-| <>
$ gawk -v A=1 -f funky2.awk
-| 1
```

The `typeof()` function provides us a “window” into `gawk`’s
inner workings.
Let’s see how using a variable or array element can change
its type dynamically.
Let’s start with using a variable as a scalar:

```
BEGIN {
    print typeof(a)         # we don't know what a is yet
    printf("a = %d\n", a)   # use it as a scalar
    print typeof(a)         # now we know it's not an array
}
```

When run:

```
$ gawk -f typing1.awk
-| untyped
-| a = 0
-| unassigned
```

Initially, `a` is `untyped`, since we don’t know yet if it’s
an array or scalar.
After using it in the call to `printf()`, we know it’s a scalar.
However, since it was never given a concrete value (number, string, or regexp),
its type is `unassigned`.

`gawk` is peculiar in that we can do the same thing, but
change `a` into an array:

```
BEGIN {
    print typeof(a)               # we don't know what a is yet
    a[1]                          # make a into an array
    print typeof(a[1])            # but we don't know what a[1] is yet
    printf("a[1] = %d\n", a[1])   # use it as a scalar
    print typeof(a[1])            # now we know it's not an array
}
```

When run:

```
$ gawk -f typing2.awk
-| untyped
-| untyped
-| a[1] = 0
-| unassigned
```

Normally, passing a variable that has never been used to a built-in
function causes it to become a scalar variable (`unassigned`).
However, `isarray()` and `typeof()` are different; they do
not change their arguments from `untyped` to `unassigned`.

As we saw, this applies to both variables denoted by simple identifiers
and array elements that come into existence simply by referencing them:

```
$ gawk 'BEGIN { print typeof(x) }'
-| untyped
$ gawk 'BEGIN { print typeof(x["foo"]) }'
-| untyped
```

Note that prior to version 5.2, array elements
that come into existence simply by referencing them
were different, they were automatically forced to be scalars:

```
$ gawk-5.1.1 'BEGIN { print typeof(x) }'
-| untyped
$ gawk-5.1.1 'BEGIN { print typeof(x["foo"]) }'
-| unassigned
```

To sum up, variables and array elements get their nature (array or scalar)
“fixed” upon first use. This can lead to some weird cases, and it is
best to avoid taking advantage of `gawk`’s dynamic nature, other
than in the standard manner of passing untyped variables and array
elements as function parameters.

---

Next: [Indirect Function Calls](#Indirect-Calls), Previous: [Variable Typing Is Dynamic](#Dynamic-Typing), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.4 A Note On Shadowed Variables [¶](#A-Note-On-Shadowed-Variables)

This section discusses some aspects of variable shadowing.
Feel free to skip it upon first reading.

The “shadowing” concept is important to know for several reasons.

*Name confusion*
:   Using global identifiers (names) locally can lead to very difficult
    debugging, it complicates writing, reading and maintaining source code,
    and it decreases the portability, (sharing or reuse) of a library of
    `awk` functions.

*Preventing shadowing in functions*
:   It is a best practice to consistently use a naming convention,
    such as “global variable and function names start with a capital letter
    and local names begin with lowercase one.” This also makes programs
    easier to follow (see [Naming Library Function Global Variables](#Library-Names)).

*Circumventing shadow restrictions in functions*
:   The `gawk` extension `SYMTAB` provides indirect access
    to global variables that are or may be shadowed (see `SYMTAB`
    in [Built-in Variables That Convey Information](#Auto_002dset)). For example:

    ```
    ...
    foo = "global value"
    ...

    function test(x,   foo)
    {
        # use global value of foo as default
        foo = (x > 0) ? SYMTAB["foo"] : "local value"
        ...
    }
    ```

*Solving complex shadowing problems*
:   The `gawk` namespace extension
    provides robust handling of potential name collisions with global
    variables and functions (see [Namespaces in `gawk`](#Namespaces)). Namespaces
    are useful to prevent shadowing by providing identifiers that are
    common to a group of functions and effectively shadowed from being
    referenced by global functions (See `FUNCTAB` in [Built-in Variables That Convey Information](#Auto_002dset).)

Finally, a shadowing caveat: Variables local to a function are *not*
“global” to anything. `SYMTAB` elements refer to all *global*
variables and arrays, but not to *local* variables and arrays.
If a function `A(argA, localA)` calls another function `B()`,
the two variables local to `A()` are *not* accessible in
function `B()` or any other function. The global/local distinction
is also important to remember when passing arguments to a function called
indirectly (see [Indirect Function Calls](#Indirect-Calls)).

---

Next: [Summary](#Functions-Summary), Previous: [A Note On Shadowed Variables](#Shadowed-Variables), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.5 Indirect Function Calls [¶](#Indirect-Function-Calls)

This section describes an advanced, `gawk`-specific extension.

Often, you may wish to defer the choice of function to call until runtime.
For example, you may have different kinds of records, each of which
should be processed differently.

Normally, you would have to use a series of `if`-`else`
statements to decide which function to call. By using *indirect*
function calls, you can specify the name of the function to call as a
string variable, and then call the function. Let’s look at an example.

Suppose you have a file with your test scores for the classes you
are taking, and
you wish to get the sum and the average of
your test scores.
The first field is the class name. The following fields
are the functions to call to process the data, up to a “marker”
field ‘`data:`’. Following the marker, to the end of the record,
are the various numeric test scores.

Here is the initial file:

```
Biology_101 sum average data: 87.0 92.4 78.5 94.9
Chemistry_305 sum average data: 75.2 98.3 94.7 88.2
English_401 sum average data: 100.0 95.6 87.1 93.4
```

To process the data, you might write initially:

```
{
    class = $1
    for (i = 2; $i != "data:"; i++) {
        if ($i == "sum")
            sum()   # processes the whole record
        else if ($i == "average")
            average()
        ...           # and so on
    }
}
```

This style of programming works, but can be awkward. With *indirect*
function calls, you tell `gawk` to use the *value* of a
variable as the *name* of the function to call.

The syntax is similar to that of a regular function call: an identifier
immediately followed by an opening parenthesis, any arguments, and then
a closing parenthesis, with the addition of a leading ‘`@`’
character:

```
the_function = "sum"
result = @the_function()   # calls the sum() function
```

Here is a full program that processes the previously shown data,
using indirect function calls:

```
# indirectcall.awk --- Demonstrate indirect function calls

# average --- return the average of the values in fields $first - $last

function average(first, last,   sum, i)
{
    sum = 0;
    for (i = first; i <= last; i++)
        sum += $i

    return sum / (last - first + 1)
}

# sum --- return the sum of the values in fields $first - $last

function sum(first, last,   ret, i)
{
    ret = 0;
    for (i = first; i <= last; i++)
        ret += $i

    return ret
}
```

These two functions expect to work on fields; thus, the parameters
`first` and `last` indicate where in the fields to start and end.
Otherwise, they perform the expected computations and are not unusual:

```
# For each record, print the class name and the requested statistics
{
    class_name = $1
    gsub(/_/, " ", class_name)  # Replace _ with spaces

    # find start
    for (i = 1; i <= NF; i++) {
        if ($i == "data:") {
            start = i + 1
            break
        }
    }

    printf("%s:\n", class_name)
    for (i = 2; $i != "data:"; i++) {
        the_function = $i
        printf("\t%s: <%s>\n", $i, @the_function(start, NF) "")
    }
    print ""
}
```

This is the main processing for each record. It prints the class name (with
underscores replaced with spaces). It then finds the start of the actual data,
saving it in `start`.
The last part of the code loops through each function name (from `$2` up to
the marker, ‘`data:`’), calling the function named by the field. The indirect
function call itself occurs as a parameter in the call to `printf`.
(The `printf` format string uses ‘`%s`’ as the format specifier so that we
can use functions that return strings, as well as numbers. Note that the result
from the indirect call is concatenated with the empty string, in order to force
it to be a string value.)

Here is the result of running the program:

```
$ gawk -f indirectcall.awk class_data1
-| Biology 101:
-|     sum: <352.8>
-|     average: <88.2>
-|
-| Chemistry 305:
-|     sum: <356.4>
-|     average: <89.1>
-|
-| English 401:
-|     sum: <376.1>
-|     average: <94.025>
```

The ability to use indirect function calls is more powerful than you may
think at first. The C and C++ languages provide “function pointers,” which
are a mechanism for calling a function chosen at runtime. One of the most
well-known uses of this ability is the C `qsort()` function, which sorts
an array using the famous “quicksort” algorithm
(see [the Wikipedia article](https://en.wikipedia.org/wiki/Quicksort)
for more information). To use this function, you supply a pointer to a comparison
function. This mechanism allows you to sort arbitrary data in an arbitrary
fashion.

We can do something similar using `gawk`, like this:

```
# quicksort.awk --- Quicksort algorithm, with user-supplied
#                   comparison function

# quicksort --- C.A.R. Hoare's quicksort algorithm. See Wikipedia
#               or almost any algorithms or computer science text.

function quicksort(data, left, right, less_than,    i, last)
{
    if (left >= right)  # do nothing if array contains fewer
        return          # than two elements

    quicksort_swap(data, left, int((left + right) / 2))
    last = left
    for (i = left + 1; i <= right; i++)
        if (@less_than(data[i], data[left]))
            quicksort_swap(data, ++last, i)
    quicksort_swap(data, left, last)
    quicksort(data, left, last - 1, less_than)
    quicksort(data, last + 1, right, less_than)
}

# quicksort_swap --- helper function for quicksort, should really be inline

function quicksort_swap(data, i, j,      temp)
{
    temp = data[i]
    data[i] = data[j]
    data[j] = temp
}
```

The `quicksort()` function receives the `data` array, the starting and ending
indices to sort (`left` and `right`), and the name of a function that
performs a “less than” comparison. It then implements the quicksort algorithm.

To make use of the sorting function, we return to our previous example. The
first thing to do is write some comparison functions:

```
# num_lt --- do a numeric less than comparison

function num_lt(left, right)
{
    return ((left + 0) < (right + 0))
}
```

```
# num_ge --- do a numeric greater than or equal to comparison

function num_ge(left, right)
{
    return ((left + 0) >= (right + 0))
}
```

The `num_ge()` function is needed to perform a descending sort; when used
to perform a “less than” test, it actually does the opposite (greater than
or equal to), which yields data sorted in descending order.

Next comes a sorting function. It is parameterized with the starting and
ending field numbers and the comparison function. It builds an array with
the data and calls `quicksort()` appropriately, and then formats the
results as a single string:

```
# do_sort --- sort the data according to `compare'
#             and return it as a string

function do_sort(first, last, compare,      data, i, retval)
{
    delete data
    for (i = 1; first <= last; first++) {
        data[i] = $first
        i++
    }

    quicksort(data, 1, i-1, compare)

    retval = data[1]
    for (i = 2; i in data; i++)
        retval = retval " " data[i]

    return retval
}
```

Finally, the two sorting functions call `do_sort()`, passing in the
names of the two comparison functions:

```
# sort --- sort the data in ascending order and return it as a string

function sort(first, last)
{
    return do_sort(first, last, "num_lt")
}
```

```

```

```
# rsort --- sort the data in descending order and return it as a string

function rsort(first, last)
{
    return do_sort(first, last, "num_ge")
}
```

Here is an extended version of the data file:

```
Biology_101 sum average sort rsort data: 87.0 92.4 78.5 94.9
Chemistry_305 sum average sort rsort data: 75.2 98.3 94.7 88.2
English_401 sum average sort rsort data: 100.0 95.6 87.1 93.4
```

Finally, here are the results when the enhanced program is run:

```
$ gawk -f quicksort.awk -f indirectcall.awk class_data2
-| Biology 101:
-|     sum: <352.8>
-|     average: <88.2>
-|     sort: <78.5 87.0 92.4 94.9>
-|     rsort: <94.9 92.4 87.0 78.5>
-|
-| Chemistry 305:
-|     sum: <356.4>
-|     average: <89.1>
-|     sort: <75.2 88.2 94.7 98.3>
-|     rsort: <98.3 94.7 88.2 75.2>
-|
-| English 401:
-|     sum: <376.1>
-|     average: <94.025>
-|     sort: <87.1 93.4 95.6 100.0>
-|     rsort: <100.0 95.6 93.4 87.1>
```

Another example where indirect functions calls are useful can be found in
processing arrays. This is described in [Traversing Arrays of Arrays](#Walking-Arrays).

Remember that you must supply a leading ‘`@`’ in front of an indirect function call.

Starting with version 4.1.2 of `gawk`, indirect function
calls may also be used with built-in functions and with extension functions
(see [Writing Extensions for `gawk`](#Dynamic-Extensions)). There are some limitations when calling
built-in functions indirectly, as follows.

* You cannot pass a regular expression constant to a built-in function
  through an indirect function call. This applies to the `sub()`,
  `gsub()`, `gensub()`, `match()`, `split()` and
  `patsplit()` functions. However, you can pass a strongly typed
  regexp constant (see [Strongly Typed Regexp Constants](#Strong-Regexp-Constants)).
* If calling `sub()` or `gsub()`, you may only pass two arguments,
  since those functions are unusual in that they update their third argument.
  This means that `$0` will be updated.
* You cannot indirectly call built-in functions that can take `$0` as
  a default parameter; you must supply an argument instead. For example,
  you must pass an argument to `length()` if calling it indirectly.
* Calling a built-in function indirectly with the wrong number of arguments
  for that function causes a fatal error. For example, calling
  `length()` with two arguments. These errors are found at runtime
  instead of when `gawk` parses your program, since `gawk`
  doesn’t know until runtime if you have passed the correct number of
  arguments or not.

`gawk` does its best to make indirect function calls efficient.
For example, in the following case:

```
for (i = 1; i <= n; i++)
    @the_function()
```

`gawk` looks up the actual function to call only once.

---

Previous: [Indirect Function Calls](#Indirect-Calls), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.6 Summary [¶](#Summary-8)

* `awk` provides built-in functions and lets you define your own
  functions.
* POSIX `awk` provides three kinds of built-in functions: numeric,
  string, and I/O. `gawk` provides functions that sort arrays, work
  with values representing time, do bit manipulation, determine variable
  type (array versus scalar), and internationalize and localize programs.
  `gawk` also provides several extensions to some of standard
  functions, typically in the form of additional arguments.
* Functions accept zero or more arguments and return a value. The
  expressions that provide the argument values are completely evaluated
  before the function is called. Order of evaluation is not defined.
  The return value can be ignored.
* The handling of backslash in `sub()` and `gsub()` is not simple.
  It is more straightforward in `gawk`’s `gensub()` function,
  but that function still requires care in its use.
* User-defined functions provide important capabilities but come with
  some syntactic inelegancies. In a function call, there cannot be any
  space between the function name and the opening left parenthesis of the
  argument list. Also, there is no provision for local variables, so the
  convention is to add extra parameters, and to separate them visually
  from the real parameters by extra whitespace.
* User-defined functions may call other user-defined (and built-in)
  functions and may call themselves recursively. Function parameters
  “hide” any global variables of the same names.
  You cannot use the name of a reserved variable (such as `ARGC`)
  as the name of a parameter in user-defined functions.
* Scalar values are passed to user-defined functions by value. Array
  parameters are passed by reference; any changes made by the function to
  array parameters are thus visible after the function has returned.
* Use the `return` statement to return from a user-defined function.
  An optional expression becomes the function’s return value. Only scalar
  values may be returned by a function.
* If a variable that has never been used is passed to a user-defined
  function, how that function treats the variable can set its nature:
  either scalar or array.
* `gawk` is even more fluid in its designation of variables
  and array elements as scalars or arrays. However, this can lead
  to weird situations, so you should tread carefully.
* `gawk` provides indirect function calls using a special syntax.
  By setting a variable to the name of a function, you can
  determine at runtime what function will be called at that point in the
  program. This is equivalent to function pointers in C and C++.

---

Next: [Practical `awk` Programs](#Sample-Programs), Previous: [Functions](#Functions), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]# Part II: Problem Solving with `awk` [¶](#Part-II_003a-Problem-Solving-with-awk)

---

* [A Library of `awk` Functions](#Library-Functions)
* [Practical `awk` Programs](#Sample-Programs)## 10 A Library of `awk` Functions [¶](#A-Library-of-awk-Functions)

[User-Defined Functions](#User_002ddefined) describes how to write
your own `awk` functions. Writing functions is important, because
it allows you to encapsulate algorithms and program tasks in a single
place. It simplifies programming, making program development more
manageable and making programs more readable.

In their seminal 1976 book, Software Tools,[68](#FOOT68) Brian Kernighan
and P.J. Plauger wrote:

> Good Programming is not learned from generalities, but by seeing how
> significant programs can be made clean, easy to read, easy to maintain and
> modify, human-engineered, efficient and reliable, by the application of
> common sense and good programming practices. Careful study and imitation
> of good programs leads to better writing.

In fact, they felt this idea was so important that they placed this
statement on the cover of their book. Because we believe strongly
that their statement is correct, this chapter and [Practical `awk` Programs](#Sample-Programs), provide a good-sized body of code for you to read and, we hope,
to learn from.

This chapter presents a library of useful `awk` functions.
Many of the sample programs presented later in this Web page
use these functions.
The functions are presented here in a progression from simple to complex.

[Extracting Programs from Texinfo Source Files](#Extract-Program)
presents a program that you can use to extract the source code for
these example library functions and programs from the Texinfo source
for this Web page.
(This has already been done as part of the `gawk` distribution.)

If you have written one or more useful, general-purpose `awk` functions
and would like to contribute them to the `awk` user community, see
[How to Contribute](#How-To-Contribute), for more information.

The programs in this chapter and in
[Practical `awk` Programs](#Sample-Programs),
freely use `gawk`-specific features.
Rewriting these programs for different implementations of `awk`
is pretty straightforward:

* Diagnostic error messages are sent to `/dev/stderr`.
  Use ‘`| "cat 1>&2"`’ instead of ‘`> "/dev/stderr"`’ if your system
  does not have a `/dev/stderr`, or if you cannot use `gawk`.
* Finally, some of the programs choose to ignore upper- and lowercase
  distinctions in their input. They do so by assigning one to `IGNORECASE`.
  You can achieve almost the same effect[69](#FOOT69) by adding the following rule to the
  beginning of the program:

  ```
  # ignore case
  { $0 = tolower($0) }
  ```

  Also, verify that all regexp and string constants used in
  comparisons use only lowercase letters.

* [Naming Library Function Global Variables](#Library-Names)
* [General Programming](#General-Functions)
* [Data file Management](#Data-File-Management)
* [Processing Command-Line Options](#Getopt-Function)
* [Reading the User Database](#Passwd-Functions)
* [Reading the Group Database](#Group-Functions)
* [Traversing Arrays of Arrays](#Walking-Arrays)
* [Summary](#Library-Functions-Summary)
* [Exercises](#Library-Exercises)

---

Next: [General Programming](#General-Functions), Up: [A Library of `awk` Functions](#Library-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.1 Naming Library Function Global Variables [¶](#Naming-Library-Function-Global-Variables)

Due to the way the `awk` language evolved, variables are either
*global* (usable by the entire program) or *local* (usable just by
a specific function). There is no intermediate state analogous to
`static` variables in C.

Library functions often need to have global variables that they can use to
preserve state information between calls to the function—for example,
`getopt()`’s variable `_opti`
(see [Processing Command-Line Options](#Getopt-Function)).
Such variables are called *private*, as the only functions that need to
use them are the ones in the library.

When writing a library function, you should try to choose names for your
private variables that will not conflict with any variables used by
either another library function or a user’s main program. For example, a
name like `i` or `j` is not a good choice, because user programs
often use variable names like these for their own purposes.

The example programs shown in this chapter all start the names of their
private variables with an underscore (‘`_`’). Users generally don’t use
leading underscores in their variable names, so this convention immediately
decreases the chances that the variable names will be accidentally shared
with the user’s program.

In addition, several of the library functions use a prefix that helps
indicate what function or set of functions use the variables—for example,
`_pw_byname()` in the user database routines
(see [Reading the User Database](#Passwd-Functions)).
This convention is recommended, as it even further decreases the
chance of inadvertent conflict among variable names. Note that this
convention is used equally well for variable names and for private
function names.[70](#FOOT70)

As a final note on variable naming, if a function makes global variables
available for use by a main program, it is a good convention to start those
variables’ names with a capital letter—for
example, `getopt()`’s `Opterr` and `Optind` variables
(see [Processing Command-Line Options](#Getopt-Function)).
The leading capital letter indicates that it is global, while the fact that
the variable name is not all capital letters indicates that the variable is
not one of `awk`’s predefined variables, such as `FS`.

It is also important that *all* variables in library
functions that do not need to save state are, in fact, declared
local.[71](#FOOT71) If this is not done, the variables
could accidentally be used in the user’s program, leading to bugs that
are very difficult to track down:

```
function lib_func(x, y,    l1, l2)
{
    ...
    # some_var should be local but by oversight is not
    use variable some_var
    ...
}
```

A different convention, common in the Tcl community, is to use a single
associative array to hold the values needed by the library function(s), or
“package.” This significantly decreases the number of actual global names
in use. For example, the functions described in
[Reading the User Database](#Passwd-Functions)
might have used array elements `PW_data["inited"]`, `PW_data["total"]`,
`PW_data["count"]`, and `PW_data["awklib"]`, instead of
`_pw_inited`, `_pw_awklib`, `_pw_total`,
and `_pw_count`.

The conventions presented in this section are exactly
that: conventions. You are not required to write your programs this
way—we merely recommend that you do so.

Beginning with version 5.0, `gawk` provides
a powerful mechanism for solving the problems described in this
section: *namespaces*. Namespaces and their use are described
in detail in [Namespaces in `gawk`](#Namespaces).

---

Next: [Data file Management](#Data-File-Management), Previous: [Naming Library Function Global Variables](#Library-Names), Up: [A Library of `awk` Functions](#Library-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.2 General Programming [¶](#General-Programming)

This section presents a number of functions that are of general
programming use.

* [Converting Strings to Numbers](#Strtonum-Function)
* [Assertions](#Assert-Function)
* [Rounding Numbers](#Round-Function)
* [The Cliff Random Number Generator](#Cliff-Random-Function)
* [Translating Between Characters and Numbers](#Ordinal-Functions)
* [Merging an Array into a String](#Join-Function)
* [Managing the Time of Day](#Getlocaltime-Function)
* [Reading a Whole File at Once](#Readfile-Function)
* [Quoting Strings to Pass to the Shell](#Shell-Quoting)
* [Checking Whether A Value Is Numeric](#Isnumeric-Function)
* [Producing CSV Data](#To-CSV-Function)

---

Next: [Assertions](#Assert-Function), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.1 Converting Strings to Numbers [¶](#Converting-Strings-to-Numbers)

The `strtonum()` function (see [String-Manipulation Functions](#String-Functions))
is a `gawk` extension. The following function
provides an implementation for other versions of `awk`:

```
# mystrtonum --- convert string to number

function mystrtonum(str,        ret, n, i, k, c)
{
    if (str ~ /^0[0-7]*$/) {
        # octal
        n = length(str)
        ret = 0
        for (i = 1; i <= n; i++) {
            c = substr(str, i, 1)
            # index() returns 0 if c not in string,
            # includes c == "0"
            k = index("1234567", c)

            ret = ret * 8 + k
        }
    } else if (str ~ /^0[xX][[:xdigit:]]+$/) {
        # hexadecimal
        str = substr(str, 3)    # lop off leading 0x
        n = length(str)
        ret = 0
        for (i = 1; i <= n; i++) {
            c = substr(str, i, 1)
            c = tolower(c)
            # index() returns 0 if c not in string,
            # includes c == "0"
            k = index("123456789abcdef", c)

            ret = ret * 16 + k
        }
    } else if (str ~ \
  /^[-+]?([0-9]+([.][0-9]*([Ee][0-9]+)?)?|([.][0-9]+([Ee][-+]?[0-9]+)?))$/) {
        # decimal number, possibly floating point
        ret = str + 0
    } else
        ret = "NOT-A-NUMBER"

    return ret
}

# BEGIN {     # gawk test harness
#     a[1] = "25"
#     a[2] = ".31"
#     a[3] = "0123"
#     a[4] = "0xdeadBEEF"
#     a[5] = "123.45"
#     a[6] = "1.e3"
#     a[7] = "1.32"
#     a[8] = "1.32E2"
#
#     for (i = 1; i in a; i++)
#         print a[i], strtonum(a[i]), mystrtonum(a[i])
# }
```

The function first looks for C-style octal numbers (base 8).
If the input string matches a regular expression describing octal
numbers, then `mystrtonum()` loops through each character in the
string. It sets `k` to the index in `"1234567"` of the current
octal digit.
The return value will either be the same number as the digit, or zero
if the character is not there, which will be true for a ‘`0`’.
This is safe, because the regexp test in the `if` ensures that
only octal values are converted.

Similar logic applies to the code that checks for and converts a
hexadecimal value, which starts with ‘`0x`’ or ‘`0X`’.
The use of `tolower()` simplifies the computation for finding
the correct numeric value for each hexadecimal digit.

Finally, if the string matches the (rather complicated) regexp for a
regular decimal integer or floating-point number, the computation
‘`ret = str + 0`’ lets `awk` convert the value to a
number.

A commented-out test program is included, so that the function can
be tested with `gawk` and the results compared to the built-in
`strtonum()` function.

---

Next: [Rounding Numbers](#Round-Function), Previous: [Converting Strings to Numbers](#Strtonum-Function), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.2 Assertions [¶](#Assertions)

> *Look both ways before crossing the Atlantic.*

— *William (Bill) Robbins*

When writing large programs, it is often useful to know
that a condition or set of conditions is true. Before proceeding with a
particular computation, you make a statement about what you believe to be
the case. Such a statement is known as an
*assertion*. The C language provides an `<assert.h>` header file
and corresponding `assert()` macro that a programmer can use to make
assertions. If an assertion fails, the `assert()` macro arranges to
print a diagnostic message describing the condition that should have
been true but was not, and then it kills the program. In C, using
`assert()` looks this:

```
#include <assert.h>

int myfunc(int a, double b)
{
     assert(a <= 5 && b >= 17.1);
     ...
}
```

If the assertion fails, the program prints a message similar to this:

```
prog.c:5: assertion failed: a <= 5 && b >= 17.1
```

The C language makes it possible to turn the condition into a string for use
in printing the diagnostic message. This is not possible in `awk`, so
this `assert()` function also requires a string version of the condition
that is being tested.
Following is the function:

```
# assert --- assert that a condition is true. Otherwise, exit.

function assert(condition, string)
{
    if (! condition) {
        printf("%s:%d: assertion failed: %s\n",
            FILENAME, FNR, string) > "/dev/stderr"
        _assert_exit = 1
        exit 1
    }
}
```

```
END {
    if (_assert_exit)
        exit 1
}
```

The `assert()` function tests the `condition` parameter. If it
is false, it prints a message to standard error, using the `string`
parameter to describe the failed condition. It then sets the variable
`_assert_exit` to one and executes the `exit` statement.
The `exit` statement jumps to the `END` rule. If the `END`
rule finds `_assert_exit` to be true, it exits immediately.

The purpose of the test in the `END` rule is to
keep any other `END` rules from running. When an assertion fails, the
program should exit immediately.
If no assertions fail, then `_assert_exit` is still
false when the `END` rule is run normally, and the rest of the
program’s `END` rules execute.
For all of this to work correctly, `assert.awk` must be the
first source file read by `awk`.
The function can be used in a program in the following way:

```
function myfunc(a, b)
{
     assert(a <= 5 && b >= 17.1, "a <= 5 && b >= 17.1")
     ...
}
```

If the assertion fails, you see a message similar to the following:

```
mydata:1357: assertion failed: a <= 5 && b >= 17.1
```

There is a small problem with this version of `assert()`.
An `END` rule is automatically added
to the program calling `assert()`. Normally, if a program consists
of just a `BEGIN` rule, the input files and/or standard input are
not read. However, now that the program has an `END` rule, `awk`
attempts to read the input data files or standard input
(see [Startup and Cleanup Actions](#Using-BEGIN_002fEND)),
most likely causing the program to hang as it waits for input.

There is a simple workaround to this:
make sure that such a `BEGIN` rule always ends
with an `exit` statement.

---

Next: [The Cliff Random Number Generator](#Cliff-Random-Function), Previous: [Assertions](#Assert-Function), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.3 Rounding Numbers [¶](#Rounding-Numbers)

The way `printf` and `sprintf()`
(see [Using `printf` Statements for Fancier Printing](#Printf))
perform rounding often depends upon the system’s C `sprintf()`
subroutine. On many machines, `sprintf()` rounding is *unbiased*,
which means it doesn’t always round a trailing .5 up, contrary
to naive expectations. In unbiased rounding, .5 rounds to even,
rather than always up, so 1.5 rounds to 2 but 4.5 rounds to 4. This means
that if you are using a format that does rounding (e.g., `"%.0f"`),
you should check what your system does. The following function does
traditional rounding; it might be useful if your `awk`’s `printf`
does unbiased rounding:

```
# round.awk --- do normal rounding

function round(x,   ival, aval, fraction)
{
   ival = int(x)    # integer part, int() truncates

   # see if fractional part
   if (ival == x)   # no fraction
      return ival   # ensure no decimals

   if (x < 0) {
      aval = -x     # absolute value
      ival = int(aval)
      fraction = aval - ival
      if (fraction >= .5)
         return int(x) - 1   # -2.5 --> -3
      else
         return int(x)       # -2.3 --> -2
   } else {
      fraction = x - ival
      if (fraction >= .5)
         return ival + 1
      else
         return ival
   }
}
```

```
# test harness
# { print $0, round($0) }
```

A different, more compact implementation of this function is
suggested by Jason C. Kwan in this
[message](https://mail.gnu.org/archive/html/bug-gawk/2026-02/msg00000.html)
to the “bug-gawk at gnu.org” mailing list.

---

Next: [Translating Between Characters and Numbers](#Ordinal-Functions), Previous: [Rounding Numbers](#Round-Function), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.4 The Cliff Random Number Generator [¶](#The-Cliff-Random-Number-Generator)

The
[Cliff random number generator](https://mathworld.wolfram.com/CliffRandomNumberGenerator.html)
is a very simple random number generator that “passes the noise sphere test
for randomness by showing no structure.”
It is easily programmed, in less than 10 lines of `awk` code:

```
# cliff_rand.awk --- generate Cliff random numbers

BEGIN { _cliff_seed = 0.1 }

function cliff_rand()
{
    _cliff_seed = (100 * log(_cliff_seed)) % 1
    if (_cliff_seed < 0)
        _cliff_seed = - _cliff_seed
    return _cliff_seed
}
```

This algorithm requires an initial “seed” of 0.1. Each new value
uses the current seed as input for the calculation.
If the built-in `rand()` function
(see [Numeric Functions](#Numeric-Functions))
isn’t random enough, you might try using this function instead.

---

Next: [Merging an Array into a String](#Join-Function), Previous: [The Cliff Random Number Generator](#Cliff-Random-Function), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.5 Translating Between Characters and Numbers [¶](#Translating-Between-Characters-and-Numbers)

One commercial implementation of `awk` supplies a built-in function,
`ord()`, which takes a character and returns the numeric value for that
character in the machine’s character set. If the string passed to
`ord()` has more than one character, only the first one is used.

The inverse of this function is `chr()` (from the function of the same
name in Pascal), which takes a number and returns the corresponding character.
Both functions are written very nicely in `awk`; there is no real
reason to build them into the `awk` interpreter:

```
# ord.awk --- do ord and chr

# Global identifiers:
#    _ord_:        numerical values indexed by characters
#    _ord_init:    function to initialize _ord_

BEGIN    { _ord_init() }

function _ord_init(    low, high, i, t)
{
    low = sprintf("%c", 7) # BEL is ascii 7
    if (low == "\a") {    # regular ascii
        low = 0
        high = 127
    } else if (sprintf("%c", 128 + 7) == "\a") {
        # ascii, mark parity
        low = 128
        high = 255
    } else {        # ebcdic(!)
        low = 0
        high = 255
    }

    for (i = low; i <= high; i++) {
        t = sprintf("%c", i)
        _ord_[t] = i
    }
}
```

Some explanation of the numbers used by `_ord_init()` is worthwhile.
The most prominent character set in use today is ASCII.[72](#FOOT72)
Although an
8-bit byte can hold 256 distinct values (from 0 to 255), ASCII only
defines characters that use the values from 0 to 127.[73](#FOOT73)
In the now distant past,
at least one minicomputer manufacturer
used ASCII, but with mark parity, meaning that the leftmost bit in the byte
is always 1. This means that on those systems, characters
have numeric values from 128 to 255.
Finally, large mainframe systems use the EBCDIC character set, which
uses all 256 values.
There are other character sets in use on some older systems, but
they are not really worth worrying about:

```
function ord(str,    c)
{
    # only first character is of interest
    c = substr(str, 1, 1)
    return _ord_[c]
}

function chr(c)
{
    # force c to be numeric by adding 0
    return sprintf("%c", c + 0)
}

#### test code ####
# BEGIN {
#    for (;;) {
#        printf("enter a character: ")
#        if (getline var <= 0)
#            break
#        printf("ord(%s) = %d\n", var, ord(var))
#    }
# }
```

An obvious improvement to these functions is to move the code for the
`_ord_init` function into the body of the `BEGIN` rule. It was
written this way initially for ease of development.
There is a “test program” in a `BEGIN` rule, to test the
function. It is commented out for production use.

---

Next: [Managing the Time of Day](#Getlocaltime-Function), Previous: [Translating Between Characters and Numbers](#Ordinal-Functions), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.6 Merging an Array into a String [¶](#Merging-an-Array-into-a-String)

When doing string processing, it is often useful to be able to join
all the strings in an array into one long string. The following function,
`join()`, accomplishes this task. It is used later in several of
the application programs
(see [Practical `awk` Programs](#Sample-Programs)).

Good function design is important; this function needs to be general, but it
should also have a reasonable default behavior. It is called with an array
as well as the beginning and ending indices of the elements in the array to be
merged. This assumes that the array indices are numeric—a reasonable
assumption, as the array was likely created with `split()`
(see [String-Manipulation Functions](#String-Functions)):

```
# join.awk --- join an array into a string

function join(array, start, end, sep,    result, i)
{
    if (sep == "")
       sep = " "
    else if (sep == SUBSEP) # magic value
       sep = ""
    result = array[start]
    for (i = start + 1; i <= end; i++)
        result = result sep array[i]
    return result
}
```

An optional additional argument is the separator to use when joining the
strings back together. If the caller supplies a nonempty value,
`join()` uses it; if it is not supplied, it has a null
value. In this case, `join()` uses a single space as a default
separator for the strings. If the value is equal to `SUBSEP`,
then `join()` joins the strings with no separator between them.
`SUBSEP` serves as a “magic” value to indicate that there should
be no separation between the component strings.[74](#FOOT74)

---

Next: [Reading a Whole File at Once](#Readfile-Function), Previous: [Merging an Array into a String](#Join-Function), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.7 Managing the Time of Day [¶](#Managing-the-Time-of-Day)

The `systime()` and `strftime()` functions described in
[Time Functions](#Time-Functions)
provide the minimum functionality necessary for dealing with the time of day
in human-readable form. Although `strftime()` is extensive, the control
formats are not necessarily easy to remember or intuitively obvious when
reading a program.

The following function, `getlocaltime()`, populates a user-supplied array
with preformatted time information. It returns a string with the current
time formatted in the same way as the `date` utility:

```
# getlocaltime.awk --- get the time of day in a usable format

# Returns a string in the format of output of date(1)
# Populates the array argument time with individual values:
#    time["second"]       -- seconds (0 - 59)
#    time["minute"]       -- minutes (0 - 59)
#    time["hour"]         -- hours (0 - 23)
#    time["althour"]      -- hours (0 - 12)
#    time["monthday"]     -- day of month (1 - 31)
#    time["month"]        -- month of year (1 - 12)
#    time["monthname"]    -- name of the month
#    time["shortmonth"]   -- short name of the month
#    time["year"]         -- year modulo 100 (0 - 99)
#    time["fullyear"]     -- full year
#    time["weekday"]      -- day of week (Sunday = 0)
#    time["altweekday"]   -- day of week (Monday = 0)
#    time["dayname"]      -- name of weekday
#    time["shortdayname"] -- short name of weekday
#    time["yearday"]      -- day of year (0 - 365)
#    time["timezone"]     -- abbreviation of timezone name
#    time["ampm"]         -- AM or PM designation
#    time["weeknum"]      -- week number, Sunday first day
#    time["altweeknum"]   -- week number, Monday first day

function getlocaltime(time,    ret, now, i)
{
    # get time once, avoids unnecessary system calls
    now = systime()

    # return date(1)-style output
    ret = strftime("%a %b %e %H:%M:%S %Z %Y", now)

    # clear out target array
    delete time

    # fill in values, force numeric values to be
    # numeric by adding 0
    time["second"]       = strftime("%S", now) + 0
    time["minute"]       = strftime("%M", now) + 0
    time["hour"]         = strftime("%H", now) + 0
    time["althour"]      = strftime("%I", now) + 0
    time["monthday"]     = strftime("%d", now) + 0
    time["month"]        = strftime("%m", now) + 0
    time["monthname"]    = strftime("%B", now)
    time["shortmonth"]   = strftime("%b", now)
    time["year"]         = strftime("%y", now) + 0
    time["fullyear"]     = strftime("%Y", now) + 0
    time["weekday"]      = strftime("%w", now) + 0
    time["altweekday"]   = strftime("%u", now) + 0
    time["dayname"]      = strftime("%A", now)
    time["shortdayname"] = strftime("%a", now)
    time["yearday"]      = strftime("%j", now) + 0
    time["timezone"]     = strftime("%Z", now)
    time["ampm"]         = strftime("%p", now)
    time["weeknum"]      = strftime("%U", now) + 0
    time["altweeknum"]   = strftime("%W", now) + 0

    return ret
}
```

The string indices are easier to use and read than the various formats
required by `strftime()`. The `alarm` program presented in
[An Alarm Clock Program](#Alarm-Program)
uses this function.
A more general design for the `getlocaltime()` function would have
allowed the user to supply an optional timestamp value to use instead
of the current time.

---

Next: [Quoting Strings to Pass to the Shell](#Shell-Quoting), Previous: [Managing the Time of Day](#Getlocaltime-Function), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.8 Reading a Whole File at Once [¶](#Reading-a-Whole-File-at-Once)

Often, it is convenient to have the entire contents of a file available
in memory as a single string. A straightforward but naive way to
do that might be as follows:

```
function readfile1(file,    tmp, contents)
{
    if ((getline tmp < file) < 0)
        return

    contents = tmp RT
    while ((getline tmp < file) > 0)
        contents = contents tmp RT

    close(file)
    return contents
}
```

This function reads from `file` one record at a time, building
up the full contents of the file in the local variable `contents`.
It works, but is not necessarily efficient.

The following function, based on a suggestion by Denis Shirokov,
reads the entire contents of the named file in one shot:

```
# readfile.awk --- read an entire file at once

function readfile(file,     tmp, save_rs)
{
    save_rs = RS
    RS = "^$"
    getline tmp < file
    close(file)
    RS = save_rs

    return tmp
}
```

It works by setting `RS` to ‘`^$`’, a regular expression that
will never match if the file has contents. `gawk` reads data from
the file into `tmp`, attempting to match `RS`. The match fails
after each read, but fails quickly, such that `gawk` fills
`tmp` with the entire contents of the file.
(See [How Input Is Split into Records](#Records) for information on `RT` and `RS`.)

In the case that `file` is empty, the return value is the null
string. Thus, calling code may use something like:

```
contents = readfile("/some/path")
if (length(contents) == 0)
    # file was empty ...
```

This tests the result to see if it is empty or not. An equivalent
test would be ‘`contents == ""`’.

See [Reading an Entire File](#Extension-Sample-Readfile) for an extension function that
also reads an entire file into memory.

---

Next: [Checking Whether A Value Is Numeric](#Isnumeric-Function), Previous: [Reading a Whole File at Once](#Readfile-Function), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.9 Quoting Strings to Pass to the Shell [¶](#Quoting-Strings-to-Pass-to-the-Shell)

Michael Brennan offers the following programming pattern,
which he uses frequently:

```
#! /bin/sh

awkp='
   ...
   '

input_program | awk "$awkp" | /bin/sh
```

For example, a program of his named `flac-edit` has this form:

```
$ flac-edit -song="Whoope! That's Great" file.flac
```

It generates the following output, which is to be piped to
the shell (`/bin/sh`):

```
chmod +w file.flac
metaflac --remove-tag=TITLE file.flac
LANG=en_US.88591 metaflac --set-tag=TITLE='Whoope! That'"'"'s Great' file.flac
chmod -w file.flac
```

Note the need for shell quoting. The function `shell_quote()`
does it. `SINGLE` is the one-character string `"'"` and
`QSINGLE` is the three-character string `"\"'\""`:

```
# shell_quote --- quote an argument for passing to the shell

function shell_quote(s,             # parameter
    SINGLE, QSINGLE, i, X, n, ret)  # locals
{
    if (s == "")
        return "\"\""

    SINGLE = "\x27"  # single quote
    QSINGLE = "\"\x27\""
    n = split(s, X, SINGLE)

    ret = SINGLE X[1] SINGLE
    for (i = 2; i <= n; i++)
        ret = ret QSINGLE SINGLE X[i] SINGLE

    return ret
}
```

---

Next: [Producing CSV Data](#To-CSV-Function), Previous: [Quoting Strings to Pass to the Shell](#Shell-Quoting), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.10 Checking Whether A Value Is Numeric [¶](#Checking-Whether-A-Value-Is-Numeric)

A frequent programming question is how to ascertain whether a value is numeric.
This can be solved by using this example function `isnumeric()`, which
employs the trick of converting a string value to user input by using the
`split()` function:

```
# isnumeric --- check whether a value is numeric

function isnumeric(x,  f)
{
    switch (typeof(x)) {
    case "strnum":
    case "number":
        return 1
    case "string":
        return (split(x, f, " ") == 1) && (typeof(f[1]) == "strnum")
    default:
        return 0
    }
}
```

Please note that leading or trailing white space is disregarded in deciding
whether a value is numeric or not, so if it matters to you, you may want
to add an additional check for that.

Traditionally, it has been recommended to check for numeric values using the
test ‘`x+0 == x`’. This function is superior in two ways: it will not
report that unassigned variables contain numeric values; and it recognizes
string values with numeric contents where `CONVFMT` does not yield
the original string.
On the other hand, it uses the `typeof()` function
(see [Getting Type Information](#Type-Functions)), which is specific to `gawk`.

---

Previous: [Checking Whether A Value Is Numeric](#Isnumeric-Function), Up: [General Programming](#General-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.11 Producing CSV Data [¶](#Producing-CSV-Data)

`gawk`’s `--csv` option causes `gawk`
to process CSV data (see [Working With Comma Separated Value Files](#Comma-Separated-Fields)).

But what if you have regular data that you want to output
in CSV format? This section provides functions for
doing that.

The first function, `tocsv()`, takes an array of data
fields as input. The array should be indexed starting from one.
The optional second parameter is the separator to use. If none
is supplied, the default is a comma.

The function takes care to quote fields that contain double
quotes, newlines, or the separator character. It then builds
up the final CSV record and returns it.

```
# tocsv.awk --- convert data to CSV format

function tocsv(fields, sep,     i, j, nfields, result)
{
    if (length(fields) == 0)
        return ""

    if (sep == "")
        sep = ","
    delete nfields
    for (i = 1; i in fields; i++) {
        nfields[i] = fields[i]
        if (nfields[i] ~ /["\n]/ || index(nfields[i], sep) != 0) {
            gsub(/"/, "\"\"", nfields[i])       # double up the double quotes
            nfields[i] = "\"" nfields[i] "\""   # wrap in double quotes
        }
    }

    result = nfields[1]
    j = length(nfields)
    for (i = 2; i <= j; i++)
        result = result sep nfields[i]

    return result
}
```

The next function, `tocsv_rec()` is a wrapper around
`tocsv()`. Its intended use is for when you want to convert the
current input record to CSV format. The function itself simply copies
the fields into an array to pass to `tocsv()` which does the work.
It accepts an optional separator character as its first parameter,
which it simply passes on to `tocsv()`.

```
function tocsv_rec(sep,     i, fields)
{
    delete fields
    for (i = 1; i <= NF; i++)
        fields[i] = $i

    return tocsv(fields, sep)
}
```

---

Next: [Processing Command-Line Options](#Getopt-Function), Previous: [General Programming](#General-Functions), Up: [A Library of `awk` Functions](#Library-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.3 Data file Management [¶](#Data-file-Management)

This section presents functions that are useful for managing
command-line data files.

* [Noting Data file Boundaries](#Filetrans-Function)
* [Rereading the Current File](#Rewind-Function)
* [Checking for Readable Data files](#File-Checking)
* [Checking for Zero-Length Files](#Empty-Files)
* [Treating Assignments as File names](#Ignoring-Assigns)

---

Next: [Rereading the Current File](#Rewind-Function), Up: [Data file Management](#Data-File-Management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.3.1 Noting Data file Boundaries [¶](#Noting-Data-file-Boundaries)

The `BEGIN` and `END` rules are each executed exactly once, at
the beginning and end of your `awk` program, respectively
(see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)).
We (the `gawk` authors) once had a user who mistakenly thought that the
`BEGIN` rules were executed at the beginning of each data file and the
`END` rules were executed at the end of each data file.

When informed
that this was not the case, the user requested that we add new special
patterns to `gawk`, named `BEGIN_FILE` and `END_FILE`, that
would have the desired behavior. He even supplied us the code to do so.

Adding these special patterns to `gawk` wasn’t necessary;
the job can be done cleanly in `awk` itself, as illustrated
by the following library program.
It arranges to call two user-supplied functions, `beginfile()` and
`endfile()`, at the beginning and end of each data file.
Besides solving the problem in only nine(!) lines of code, it does so
*portably*; this works with any implementation of `awk`:

```
# transfile.awk
#
# Give the user a hook for filename transitions
#
# The user must supply functions beginfile() and endfile()
# that each take the name of the file being started or
# finished, respectively.

FILENAME != _oldfilename {
    if (_oldfilename != "")
        endfile(_oldfilename)
    _oldfilename = FILENAME
    beginfile(FILENAME)
}

END { endfile(FILENAME) }
```

This file must be loaded before the user’s “main” program, so that the
rule it supplies is executed first.

This rule relies on `awk`’s `FILENAME` variable, which
automatically changes for each new data file. The current file name is
saved in a private variable, `_oldfilename`. If `FILENAME` does
not equal `_oldfilename`, then a new data file is being processed and
it is necessary to call `endfile()` for the old file. Because
`endfile()` should only be called if a file has been processed, the
program first checks to make sure that `_oldfilename` is not the null
string. The program then assigns the current file name to
`_oldfilename` and calls `beginfile()` for the file.
Because, like all `awk` variables, `_oldfilename` is
initialized to the null string, this rule executes correctly even for the
first data file.

The program also supplies an `END` rule to do the final processing for
the last file. Because this `END` rule comes before any `END` rules
supplied in the “main” program, `endfile()` is called first. Once
again, the value of multiple `BEGIN` and `END` rules should be clear.

If the same data file occurs twice in a row on the command line, then
`endfile()` and `beginfile()` are not executed at the end of the
first pass and at the beginning of the second pass.
The following version solves the problem:

```
# ftrans.awk --- handle datafile transitions
#
