### 9.6 Set Operations using Vectors [¶](#Set-Operations-using-Vectors)

Calc includes several commands which interpret vectors as *sets* of
objects. A set is a collection of objects; any given object can appear
only once in the set. Calc stores sets as vectors of objects in
sorted order. Objects in a Calc set can be any of the usual things,
such as numbers, variables, or formulas. Two set elements are considered
equal if they are identical, except that numerically equal numbers like
the integer 4 and the float 4.0 are considered equal even though they
are not “identical.” Variables are treated like plain symbols without
attached values by the set operations; subtracting the set ‘`[b]`’
from ‘`[a, b]`’ always yields the set ‘`[a]`’ even though if
the variables ‘`a`’ and ‘`b`’ both equaled 17, you might
expect the answer ‘`[]`’.

If a set contains interval forms, then it is assumed to be a set of
real numbers. In this case, all set operations require the elements
of the set to be only things that are allowed in intervals: Real
numbers, plus and minus infinity, HMS forms, and date forms. If
there are variables or other non-real objects present in a real set,
all set operations on it will be left in unevaluated form.

If the input to a set operation is a plain number or interval form
a, it is treated like the one-element vector ‘`[a]`’.
The result is always a vector, except that if the set consists of a
single interval, the interval itself is returned instead.

See [Logical Operations](#Logical-Operations), for the `in` function which tests if
a certain value is a member of a given set. To test if the set ‘`A`’
is a subset of the set ‘`B`’, use ‘`vdiff(A, B) = []`’.

The `V +` (`calc-remove-duplicates`) [`rdup`] command
converts an arbitrary vector into set notation. It works by sorting
the vector as if by `V S`, then removing duplicates. (For example,
`[a, 5, 4, a, 4.0]` is sorted to ‘`[4, 4.0, 5, a, a]`’ and then
reduced to ‘`[4, 5, a]`’). Overlapping intervals are merged as
necessary. You rarely need to use `V +` explicitly, since all the
other set-based commands apply `V +` to their inputs before using
them.

The `V V` (`calc-set-union`) [`vunion`] command computes
the union of two sets. An object is in the union of two sets if and
only if it is in either (or both) of the input sets. (You could
accomplish the same thing by concatenating the sets with `|`,
then using `V +`.)

The `V ^` (`calc-set-intersect`) [`vint`] command computes
the intersection of two sets. An object is in the intersection if
and only if it is in both of the input sets. Thus if the input
sets are disjoint, i.e., if they share no common elements, the result
will be the empty vector ‘`[]`’. Note that the characters `V`
and `^` were chosen to be close to the conventional mathematical
notation for set
union
and
intersection.

The `V -` (`calc-set-difference`) [`vdiff`] command computes
the difference between two sets. An object is in the difference
‘`A - B`’ if and only if it is in ‘`A`’ but not in ‘`B`’.
Thus subtracting ‘`[y,z]`’ from a set will remove the elements
‘`y`’ and ‘`z`’ if they are present. You can also think of this
as a general *set complement* operator; if ‘`A`’ is the set of
all possible values, then ‘`A - B`’ is the “complement” of ‘`B`’.
Obviously this is only practical if the set of all possible values in
your problem is small enough to list in a Calc vector (or simple
enough to express in a few intervals).

The `V X` (`calc-set-xor`) [`vxor`] command computes
the “exclusive-or,” or “symmetric difference” of two sets.
An object is in the symmetric difference of two sets if and only
if it is in one, but *not* both, of the sets. Objects that
occur in both sets “cancel out.”

The `V ~` (`calc-set-complement`) [`vcompl`] command
computes the complement of a set with respect to the real numbers.
Thus ‘`vcompl(x)`’ is equivalent to ‘`vdiff([-inf .. inf], x)`’.
For example, ‘`vcompl([2, (3 .. 4]])`’ evaluates to
‘`[[-inf .. 2), (2 .. 3], (4 .. inf]]`’.

The `V F` (`calc-set-floor`) [`vfloor`] command
reinterprets a set as a set of integers. Any non-integer values,
and intervals that do not enclose any integers, are removed. Open
intervals are converted to equivalent closed intervals. Successive
integers are converted into intervals of integers. For example, the
complement of the set ‘`[2, 6, 7, 8]`’ is messy, but if you wanted
the complement with respect to the set of integers you could type
`V ~ V F` to get ‘`[[-inf .. 1], [3 .. 5], [9 .. inf]]`’.

The `V E` (`calc-set-enumerate`) [`venum`] command
converts a set of integers into an explicit vector. Intervals in
the set are expanded out to lists of all integers encompassed by
the intervals. This only works for finite sets (i.e., sets which
do not involve ‘`-inf`’ or ‘`inf`’).

The `V :` (`calc-set-span`) [`vspan`] command converts any
set of reals into an interval form that encompasses all its elements.
The lower limit will be the smallest element in the set; the upper
limit will be the largest element. For an empty set, ‘`vspan([])`’
returns the empty interval ‘`[0 .. 0)`’.

The `V #` (`calc-set-cardinality`) [`vcard`] command counts
the number of integers in a set. The result is the length of the vector
that would be produced by `V E`, although the computation is much
more efficient than actually producing that vector.

Another representation for sets that may be more appropriate in some
cases is binary numbers. If you are dealing with sets of integers
in the range 0 to 49, you can use a 50-bit binary number where a
particular bit is 1 if the corresponding element is in the set.
See [Binary Number Functions](#Binary-Functions), for a list of commands that operate on
binary numbers. Note that many of the above set operations have
direct equivalents in binary arithmetic: `b o` (`calc-or`),
`b a` (`calc-and`), `b d` (`calc-diff`),
`b x` (`calc-xor`), and `b n` (`calc-not`),
respectively. You can use whatever representation for sets is most
convenient to you.

The `b u` (`calc-unpack-bits`) [`vunpack`] command
converts an integer that represents a set in binary into a set
in vector/interval notation. For example, ‘`vunpack(67)`’
returns ‘`[[0 .. 1], 6]`’. If the input is negative, the set
it represents is semi-infinite: ‘`vunpack(-4) = [2 .. inf)`’.
Use `V E` afterwards to expand intervals to individual
values if you wish. Note that this command uses the `b`
(binary) prefix key.

The `b p` (`calc-pack-bits`) [`vpack`] command
converts the other way, from a vector or interval representing
a set of nonnegative integers into a binary integer describing
the same set. The set may include positive infinity, but must
not include any negative numbers. The input is interpreted as a
set of integers in the sense of `V F` (`vfloor`). Beware
that a simple input like ‘`[100]`’ can result in a huge integer
representation
(‘`2^100`’, a 31-digit integer, in this case).

---

Next: [Reducing and Mapping Vectors](#Reducing-and-Mapping), Previous: [Set Operations using Vectors](#Set-Operations), Up: [Vector/Matrix Functions](#Matrix-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 9.7 Statistical Operations on Vectors [¶](#Statistical-Operations-on-Vectors)

The commands in this section take vectors as arguments and compute
various statistical measures on the data stored in the vectors. The
references used in the definitions of these functions are Bevington’s
*Data Reduction and Error Analysis for the Physical Sciences*,
and *Numerical Recipes* by Press, Flannery, Teukolsky and
Vetterling.

The statistical commands use the `u` prefix key followed by
a shifted letter or other character.

See [Manipulating Vectors](#Manipulating-Vectors), for a description of `V H`
(`calc-histogram`).

See [Curve Fitting](#Curve-Fitting), for the `a F` command for doing
least-squares fits to statistical data.

See [Probability Distribution Functions](#Probability-Distribution-Functions), for several common
probability distribution functions.

* [Single-Variable Statistics](#Single_002dVariable-Statistics)
* [Paired-Sample Statistics](#Paired_002dSample-Statistics)

---

Next: [Paired-Sample Statistics](#Paired_002dSample-Statistics), Up: [Statistical Operations on Vectors](#Statistical-Operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 9.7.1 Single-Variable Statistics [¶](#Single_002dVariable-Statistics-1)

These functions do various statistical computations on single
vectors. Given a numeric prefix argument, they actually pop
n objects from the stack and combine them into a data
vector. Each object may be either a number or a vector; if a
vector, any sub-vectors inside it are “flattened” as if by
`v a 0`; see [Manipulating Vectors](#Manipulating-Vectors). By default one object
is popped, which (in order to be useful) is usually a vector.

If an argument is a variable name, and the value stored in that
variable is a vector, then the stored vector is used. This method
has the advantage that if your data vector is large, you can avoid
the slow process of manipulating it directly on the stack.

These functions are left in symbolic form if any of their arguments
are not numbers or vectors, e.g., if an argument is a formula, or
a non-vector variable. However, formulas embedded within vector
arguments are accepted; the result is a symbolic representation
of the computation, based on the assumption that the formula does
not itself represent a vector. All varieties of numbers such as
error forms and interval forms are acceptable.

Some of the functions in this section also accept a single error form
or interval as an argument. They then describe a property of the
normal or uniform (respectively) statistical distribution described
by the argument. The arguments are interpreted in the same way as
the M argument of the random number function `k r`. In
particular, an interval with integer limits is considered an integer
distribution, so that ‘`[2 .. 6)`’ is the same as ‘`[2 .. 5]`’.
An interval with at least one floating-point limit is a continuous
distribution: ‘`[2.0 .. 6.0)`’ is *not* the same as
‘`[2.0 .. 5.0]`’!

The `u #` (`calc-vector-count`) [`vcount`] command
computes the number of data values represented by the inputs.
For example, ‘`vcount(1, [2, 3], [[4, 5], [], x, y])`’ returns 7.
If the argument is a single vector with no sub-vectors, this
simply computes the length of the vector.

The `u +` (`calc-vector-sum`) [`vsum`] command
computes the sum of the data values. The `u *`
(`calc-vector-prod`) [`vprod`] command computes the
product of the data values. If the input is a single flat vector,
these are the same as `V R +` and `V R *`
(see [Reducing and Mapping Vectors](#Reducing-and-Mapping)).

The `u X` (`calc-vector-max`) [`vmax`] command
computes the maximum of the data values, and the `u N`
(`calc-vector-min`) [`vmin`] command computes the minimum.
If the argument is an interval, this finds the minimum or maximum
value in the interval. (Note that ‘`vmax([2..6)) = 5`’ as
described above.) If the argument is an error form, this returns
plus or minus infinity.

The `u M` (`calc-vector-mean`) [`vmean`] command
computes the average (arithmetic mean) of the data values.
If the inputs are error forms
‘`x +/- s`’,
this is the weighted mean of the ‘`x`’ values with weights
‘`1 / s^2`’.
If the inputs are not error forms, this is simply the sum of the
values divided by the count of the values.

Note that a plain number can be considered an error form with
error
‘`s = 0`’.
If the input to `u M` is a mixture of
plain numbers and error forms, the result is the mean of the
plain numbers, ignoring all values with non-zero errors. (By the
above definitions it’s clear that a plain number effectively
has an infinite weight, next to which an error form with a finite
weight is completely negligible.)

This function also works for distributions (error forms or
intervals). The mean of an error form ‘a `+/-` b’ is simply
‘`a`’. The mean of an interval is the mean of the minimum
and maximum values of the interval.

The `I u M` (`calc-vector-mean-error`) [`vmeane`]
command computes the mean of the data points expressed as an
error form. This includes the estimated error associated with
the mean. If the inputs are error forms, the error is the square
root of the reciprocal of the sum of the reciprocals of the squares
of the input errors. (I.e., the variance is the reciprocal of the
sum of the reciprocals of the variances.)
If the inputs are plain
numbers, the error is equal to the standard deviation of the values
divided by the square root of the number of values. (This works
out to be equivalent to calculating the standard deviation and
then assuming each value’s error is equal to this standard
deviation.)

The `H u M` (`calc-vector-median`) [`vmedian`]
command computes the median of the data values. The values are
first sorted into numerical order; the median is the middle
value after sorting. (If the number of data values is even,
the median is taken to be the average of the two middle values.)
The median function is different from the other functions in
this section in that the arguments must all be real numbers;
variables are not accepted even when nested inside vectors.
(Otherwise it is not possible to sort the data values.) If
any of the input values are error forms, their error parts are
ignored.

The median function also accepts distributions. For both normal
(error form) and uniform (interval) distributions, the median is
the same as the mean.

The `H I u M` (`calc-vector-harmonic-mean`) [`vhmean`]
command computes the harmonic mean of the data values. This is
defined as the reciprocal of the arithmetic mean of the reciprocals
of the values.

The `u G` (`calc-vector-geometric-mean`) [`vgmean`]
command computes the geometric mean of the data values. This
is the nth root of the product of the values. This is also
equal to the `exp` of the arithmetic mean of the logarithms
of the data values.

The `H u G` [`agmean`] command computes the “arithmetic-geometric
mean” of two numbers taken from the stack. This is computed by
replacing the two numbers with their arithmetic mean and geometric
mean, then repeating until the two values converge.

The `u R` (`calc-vector-rms`) [`rms`]
command computes the RMS (root-mean-square) of the data values.
As its name suggests, this is the square root of the mean of the
squares of the data values.

The `u S` (`calc-vector-sdev`) [`vsdev`] command
computes the standard
deviation
of the data values. If the values are error forms, the errors are used
as weights just as for `u M`. This is the *sample* standard
deviation, whose value is the square root of the sum of the squares of
the differences between the values and the mean of the ‘`N`’ values,
divided by ‘`N-1`’.

This function also applies to distributions. The standard deviation
of a single error form is simply the error part. The standard deviation
of a continuous interval happens to equal the difference between the
limits, divided by
‘`sqrt(12)`’.
The standard deviation of an integer interval is the same as the
standard deviation of a vector of those integers.

The `I u S` (`calc-vector-pop-sdev`) [`vpsdev`]
command computes the *population* standard deviation.
It is defined by the same formula as above but dividing
by ‘`N`’ instead of by ‘`N-1`’. The population standard
deviation is used when the input represents the entire set of
data values in the distribution; the sample standard deviation
is used when the input represents a sample of the set of all
data values, so that the mean computed from the input is itself
only an estimate of the true mean.

For error forms and continuous intervals, `vpsdev` works
exactly like `vsdev`. For integer intervals, it computes the
population standard deviation of the equivalent vector of integers.

The `H u S` (`calc-vector-variance`) [`vvar`] and
`H I u S` (`calc-vector-pop-variance`) [`vpvar`]
commands compute the variance of the data values. The variance
is the
square
of the standard deviation, i.e., the sum of the
squares of the deviations of the data values from the mean.
(This definition also applies when the argument is a distribution.)

The `vflat` algebraic function returns a vector of its
arguments, interpreted in the same way as the other functions
in this section. For example, ‘`vflat(1, [2, [3, 4]], 5)`’
returns ‘`[1, 2, 3, 4, 5]`’.

---

Previous: [Single-Variable Statistics](#Single_002dVariable-Statistics), Up: [Statistical Operations on Vectors](#Statistical-Operations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 9.7.2 Paired-Sample Statistics [¶](#Paired_002dSample-Statistics-1)

The functions in this section take two arguments, which must be
vectors of equal size. The vectors are each flattened in the same
way as by the single-variable statistical functions. Given a numeric
prefix argument of 1, these functions instead take one object from
the stack, which must be an
Nx2
matrix of data values. Once again, variable names can be used in place
of actual vectors and matrices.

The `u C` (`calc-vector-covariance`) [`vcov`] command
computes the sample covariance of two vectors. The covariance
of vectors x and y is the sum of the products of the
differences between the elements of x and the mean of x
times the differences between the corresponding elements of y
and the mean of y, all divided by ‘`N-1`’. Note that
the variance of a vector is just the covariance of the vector
with itself. Once again, if the inputs are error forms the
errors are used as weight factors. If both x and y
are composed of error forms, the error for a given data point
is taken as the square root of the sum of the squares of the two
input errors.

The `I u C` (`calc-vector-pop-covariance`) [`vpcov`]
command computes the population covariance, which is the same as the
sample covariance computed by `u C` except dividing by ‘`N`’
instead of ‘`N-1`’.

The `H u C` (`calc-vector-correlation`) [`vcorr`]
command computes the linear correlation coefficient of two vectors.
This is defined by the covariance of the vectors divided by the
product of their standard deviations. (There is no difference
between sample or population statistics here.)

---

Next: [Vector and Matrix Display Formats](#Vector-and-Matrix-Formats), Previous: [Statistical Operations on Vectors](#Statistical-Operations), Up: [Vector/Matrix Functions](#Matrix-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 9.8 Reducing and Mapping Vectors [¶](#Reducing-and-Mapping-Vectors)

The commands in this section allow for more general operations on the
elements of vectors.

The simplest of these operations is `V A` (`calc-apply`)
[`apply`], which applies a given operator to the elements of a vector.
For example, applying the hypothetical function `f` to the vector
‘`[1, 2, 3]`’ would produce the function call ‘`f(1, 2, 3)`’.
Applying the `+` function to the vector ‘`[a, b]`’ gives
‘`a + b`’. Applying `+` to the vector ‘`[a, b, c]`’ is an
error, since the `+` function expects exactly two arguments.

While `V A` is useful in some cases, you will usually find that either
`V R` or `V M`, described below, is closer to what you want.

* [Specifying Operators](#Specifying-Operators)
* [Mapping](#Mapping)
* [Reducing](#Reducing)
* [Nesting and Fixed Points](#Nesting-and-Fixed-Points)
* [Generalized Products](#Generalized-Products)

---

Next: [Mapping](#Mapping), Up: [Reducing and Mapping Vectors](#Reducing-and-Mapping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 9.8.1 Specifying Operators [¶](#Specifying-Operators-1)

Commands in this section (like `V A`) prompt you to press the key
corresponding to the desired operator. Press `?` for a partial
list of the available operators. Generally, an operator is any key or
sequence of keys that would normally take one or more arguments from
the stack and replace them with a result. For example, `V A H C`
uses the hyperbolic cosine operator, `cosh`. (Since `cosh`
expects one argument, `V A H C` requires a vector with a single
element as its argument.)

You can press `x` at the operator prompt to select any algebraic
function by name to use as the operator. This includes functions you
have defined yourself using the `Z F` command. (See [Programming with Formulas](#Algebraic-Definitions).) If you give a name for which no function has been
defined, the result is left in symbolic form, as in ‘`f(1, 2, 3)`’.
Calc will prompt for the number of arguments the function takes if it
can’t figure it out on its own (say, because you named a function that
is currently undefined). It is also possible to type a digit key before
the function name to specify the number of arguments, e.g.,
`V M 3 x f RET` calls `f` with three arguments even if it
looks like it ought to have only two. This technique may be necessary
if the function allows a variable number of arguments. For example,
the `v e` [`vexp`] function accepts two or three arguments;
if you want to map with the three-argument version, you will have to
type `V M 3 v e`.

It is also possible to apply any formula to a vector by treating that
formula as a function. When prompted for the operator to use, press
`'` (the apostrophe) and type your formula as an algebraic entry.
You will then be prompted for the argument list, which defaults to a
list of all variables that appear in the formula, sorted into alphabetic
order. For example, suppose you enter the formula ‘`x + 2y^x`’.
The default argument list would be ‘`(x y)`’, which means that if
this function is applied to the arguments ‘`[3, 10]`’ the result will
be ‘`3 + 2*10^3`’. (If you plan to use a certain formula in this
way often, you might consider defining it as a function with `Z F`.)

Another way to specify the arguments to the formula you enter is with
`$`, `$$`, and so on. For example, `V A ' $$ + 2$^$$`
has the same effect as the previous example. The argument list is
automatically taken to be ‘`($$ $)`’. (The order of the arguments
may seem backwards, but it is analogous to the way normal algebraic
entry interacts with the stack.)

If you press `$` at the operator prompt, the effect is similar to
the apostrophe except that the relevant formula is taken from top-of-stack
instead. The actual vector arguments of the `V A $` or related command
then start at the second-to-top stack position. You will still be
prompted for an argument list.

A function can be written without a name using the notation ‘`<#1 - #2>`’,
which means “a function of two arguments that computes the first
argument minus the second argument.” The symbols ‘`#1`’ and ‘`#2`’
are placeholders for the arguments. You can use any names for these
placeholders if you wish, by including an argument list followed by a
colon: ‘`<x, y : x - y>`’. When you type `V A ' $$ + 2$^$$ RET`,
Calc builds the nameless function ‘`<#1 + 2 #2^#1>`’ as the function
to map across the vectors. When you type `V A ' x + 2y^x RET RET`,
Calc builds the nameless function ‘`<x, y : x + 2 y^x>`’. In both
cases, Calc also writes the nameless function to the Trail so that you
can get it back later if you wish.

If there is only one argument, you can write ‘`#`’ in place of ‘`#1`’.
(Note that ‘`< >`’ notation is also used for date forms. Calc tells
that ‘`<stuff>`’ is a nameless function by the presence of
‘`#`’ signs inside stuff, or by the fact that stuff
begins with a list of variables followed by a colon.)

You can type a nameless function directly to `V A '`, or put one on
the stack and use it with `V A $`. Calc will not prompt for an
argument list in this case, since the nameless function specifies the
argument list as well as the function itself. In `V A '`, you can
omit the ‘`< >`’ marks if you use ‘`#`’ notation for the arguments,
so that `V A ' #1+#2 RET` is the same as `V A ' <#1+#2> RET`,
which in turn is the same as `V A ' $$+$ RET`.

The internal format for ‘`<x, y : x + y>`’ is ‘`lambda(x, y, x + y)`’.
(The word `lambda` derives from Lisp notation and the theory of
functions.) The internal format for ‘`<#1 + #2>`’ is ‘`lambda(ArgA,
ArgB, ArgA + ArgB)`’. Note that there is no actual Calc function called
`lambda`; the whole point is that the `lambda` expression is
used in its symbolic form, not evaluated for an answer until it is applied
to specific arguments by a command like `V A` or `V M`.

(Actually, `lambda` does have one special property: Its arguments
are never evaluated; for example, putting ‘`<(2/3) #>`’ on the stack
will not simplify the ‘`2/3`’ until the nameless function is actually
called.)

As usual, commands like `V A` have algebraic function name equivalents.
For example, `V A k g` with an argument of ‘`v`’ is equivalent to
‘`apply(gcd, v)`’. The first argument specifies the operator name,
and is either a variable whose name is the same as the function name,
or a nameless function like ‘`<#^3+1>`’. Operators that are normally
written as algebraic symbols have the names `add`, `sub`,
`mul`, `div`, `pow`, `neg`, `mod`, and
`vconcat`.

The `call` function builds a function call out of several arguments:
‘`call(gcd, x, y)`’ is the same as ‘`apply(gcd, [x, y])`’, which
in turn is the same as ‘`gcd(x, y)`’. The first argument of `call`,
like the other functions described here, may be either a variable naming a
function, or a nameless function (‘`call(<#1+2#2>, x, y)`’ is the same
as ‘`x + 2y`’).

(Experts will notice that it’s not quite proper to use a variable to name
a function, since the name `gcd` corresponds to the Lisp variable
`var-gcd` but to the Lisp function `calcFunc-gcd`. Calc
automatically makes this translation, so you don’t have to worry
about it.)

---

Next: [Reducing](#Reducing), Previous: [Specifying Operators](#Specifying-Operators), Up: [Reducing and Mapping Vectors](#Reducing-and-Mapping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 9.8.2 Mapping [¶](#Mapping-1)

The `V M` (`calc-map`) [`map`] command applies a given
operator elementwise to one or more vectors. For example, mapping
`A` [`abs`] produces a vector of the absolute values of the
elements in the input vector. Mapping `+` pops two vectors from
the stack, which must be of equal length, and produces a vector of the
pairwise sums of the elements. If either argument is a non-vector, it
is duplicated for each element of the other vector. For example,
`[1,2,3] 2 V M ^` squares the elements of the specified vector.
With the 2 listed first, it would have computed a vector of powers of
two. Mapping a user-defined function pops as many arguments from the
stack as the function requires. If you give an undefined name, you will
be prompted for the number of arguments to use.

If any argument to `V M` is a matrix, the operator is normally mapped
across all elements of the matrix. For example, given the matrix
‘`[[1, -2, 3], [-4, 5, -6]]`’, `V M A` takes six absolute values to
produce another
3x2
matrix, ‘`[[1, 2, 3], [4, 5, 6]]`’.

The command `V M _` [`mapr`] (i.e., type an underscore at the
operator prompt) maps by rows instead. For example, `V M _ A` views
the above matrix as a vector of two 3-element row vectors. It produces
a new vector which contains the absolute values of those row vectors,
namely ‘`[3.74, 8.77]`’. (Recall, the absolute value of a vector is
defined as the square root of the sum of the squares of the elements.)
Some operators accept vectors and return new vectors; for example,
`v v` reverses a vector, so `V M _ v v` would reverse each row
of the matrix to get a new matrix, ‘`[[3, -2, 1], [-6, 5, -4]]`’.

Sometimes a vector of vectors (representing, say, strings, sets, or lists)
happens to look like a matrix. If so, remember to use `V M _` if you
want to map a function across the whole strings or sets rather than across
their individual elements.

The command `V M :` [`mapc`] maps by columns. Basically, it
transposes the input matrix, maps by rows, and then, if the result is a
matrix, transposes again. For example, `V M : A` takes the absolute
values of the three columns of the matrix, treating each as a 2-vector,
and `V M : v v` reverses the columns to get the matrix
‘`[[-4, 5, -6], [1, -2, 3]]`’.

(The symbols `_` and `:` were chosen because they had row-like
and column-like appearances, and were not already taken by useful
operators. Also, they appear shifted on most keyboards so they are easy
to type after `V M`.)

The `_` and `:` modifiers have no effect on arguments that are
not matrices (so if none of the arguments are matrices, they have no
effect at all). If some of the arguments are matrices and others are
plain numbers, the plain numbers are held constant for all rows of the
matrix (so that `2 V M _ ^` squares every row of a matrix; squaring
a vector takes a dot product of the vector with itself).

If some of the arguments are vectors with the same lengths as the
rows (for `V M _`) or columns (for `V M :`) of the matrix
arguments, those vectors are also held constant for every row or
column.

Sometimes it is useful to specify another mapping command as the operator
to use with `V M`. For example, `V M _ V A +` applies `V A +`
to each row of the input matrix, which in turn adds the two values on that
row. If you give another vector-operator command as the operator for
`V M`, it automatically uses map-by-rows mode if you don’t specify
otherwise; thus `V M V A +` is equivalent to `V M _ V A +`. (If
you really want to map-by-elements another mapping command, you can use
a triple-nested mapping command: `V M V M V A +` means to map
`V M V A +` over the rows of the matrix; in turn, `V A +` is
mapped over the elements of each row.)

Previous versions of Calc had “map across” and “map down” modes
that are now considered obsolete; the old “map across” is now simply
`V M V A`, and “map down” is now `V M : V A`. The algebraic
functions `mapa` and `mapd` are still supported, though.
Note also that, while the old mapping modes were persistent (once you
set the mode, it would apply to later mapping commands until you reset
it), the new `:` and `_` modifiers apply only to the current
mapping command. The default `V M` always means map-by-elements.

See [Algebraic Manipulation](#Algebraic-Manipulation), for the `a M` command, which is like
`V M` but for equations and inequalities instead of vectors.
See [Storing Variables](#Storing-Variables), for the `s m` command which modifies a
variable’s stored value using a `V M`-like operator.

---

Next: [Nesting and Fixed Points](#Nesting-and-Fixed-Points), Previous: [Mapping](#Mapping), Up: [Reducing and Mapping Vectors](#Reducing-and-Mapping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 9.8.3 Reducing [¶](#Reducing-1)

The `V R` (`calc-reduce`) [`reduce`] command applies a given
binary operator across all the elements of a vector. A binary operator is
a function such as `+` or `max` which takes two arguments. For
example, reducing `+` over a vector computes the sum of the elements
of the vector. Reducing `-` computes the first element minus each of
the remaining elements. Reducing `max` computes the maximum element
and so on. In general, reducing `f` over the vector ‘`[a, b, c, d]`’
produces ‘`f(f(f(a, b), c), d)`’.

The `I V R` [`rreduce`] command is similar to `V R` except
that works from right to left through the vector. For example, plain
`V R -` on the vector ‘`[a, b, c, d]`’ produces ‘`a - b - c - d`’
but `I V R -` on the same vector produces ‘`a - (b - (c - d))`’,
or ‘`a - b + c - d`’. This “alternating sum” occurs frequently
in power series expansions.

The `V U` (`calc-accumulate`) [`accum`] command does an
accumulation operation. Here Calc does the corresponding reduction
operation, but instead of producing only the final result, it produces
a vector of all the intermediate results. Accumulating `+` over
the vector ‘`[a, b, c, d]`’ produces the vector
‘`[a, a + b, a + b + c, a + b + c + d]`’.

The `I V U` [`raccum`] command does a right-to-left accumulation.
For example, `I V U -` on the vector ‘`[a, b, c, d]`’ produces the
vector ‘`[a - b + c - d, b - c + d, c - d, d]`’.

As for `V M`, `V R` normally reduces a matrix elementwise. For
example, given the matrix ‘`[[a, b, c], [d, e, f]]`’, `V R +` will
compute ‘`a + b + c + d + e + f`’. You can type `V R _` or
`V R :` to modify this behavior. The `V R _` [`reducea`]
command reduces “across” the matrix; it reduces each row of the matrix
as a vector, then collects the results. Thus `V R _ +` of this
matrix would produce ‘`[a + b + c, d + e + f]`’. Similarly, `V R :`
[`reduced`] reduces down; `V R : +` would produce ‘`[a + d,
b + e, c + f]`’.

There is a third “by rows” mode for reduction that is occasionally
useful; `V R =` [`reducer`] simply reduces the operator over
the rows of the matrix themselves. Thus `V R = +` on the above
matrix would get the same result as `V R : +`, since adding two
row vectors is equivalent to adding their elements. But `V R = *`
would multiply the two rows (to get a single number, their dot product),
while `V R : *` would produce a vector of the products of the columns.

These three matrix reduction modes work with `V R` and `I V R`,
but they are not currently supported with `V U` or `I V U`.

The obsolete reduce-by-columns function, `reducec`, is still
supported but there is no way to get it through the `V R` command.

The commands `C-x * :` and `C-x * _` are equivalent to typing
`C-x * r` to grab a rectangle of data into Calc, and then typing
`V R : +` or `V R _ +`, respectively, to sum the columns or
rows of the matrix. See [Grabbing from Other Buffers](#Grabbing-From-Buffers).

---

Next: [Generalized Products](#Generalized-Products), Previous: [Reducing](#Reducing), Up: [Reducing and Mapping Vectors](#Reducing-and-Mapping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 9.8.4 Nesting and Fixed Points [¶](#Nesting-and-Fixed-Points-1)

The `H V R` [`nest`] command applies a function to a given
argument repeatedly. It takes two values, ‘`a`’ and ‘`n`’, from
the stack, where ‘`n`’ must be an integer. It then applies the
function nested ‘`n`’ times; if the function is ‘`f`’ and ‘`n`’
is 3, the result is ‘`f(f(f(a)))`’. The number ‘`n`’ may be
negative if Calc knows an inverse for the function ‘`f`’; for
example, ‘`nest(sin, a, -2)`’ returns ‘`arcsin(arcsin(a))`’.

The `H V U` [`anest`] command is an accumulating version of
`nest`: It returns a vector of ‘`n+1`’ values, e.g.,
‘`[a, f(a), f(f(a)), f(f(f(a)))]`’. If ‘`n`’ is negative and
‘`F`’ is the inverse of ‘`f`’, then the result is of the
form ‘`[a, F(a), F(F(a)), F(F(F(a)))]`’.

The `H I V R` [`fixp`] command is like `H V R`, except
that it takes only an ‘`a`’ value from the stack; the function is
applied until it reaches a “fixed point,” i.e., until the result
no longer changes.

The `H I V U` [`afixp`] command is an accumulating `fixp`.
The first element of the return vector will be the initial value ‘`a`’;
the last element will be the final result that would have been returned
by `fixp`.

For example, 0.739085 is a fixed point of the cosine function (in radians):
‘`cos(0.739085) = 0.739085`’. You can find this value by putting, say,
1.0 on the stack and typing `H I V U C`. (We use the accumulating
version so we can see the intermediate results: ‘`[1, 0.540302, 0.857553,
0.65329, ...]`’. With a precision of six, this command will take 36 steps
to converge to 0.739085.)

Newton’s method for finding roots is a classic example of iteration
to a fixed point. To find the square root of five starting with an
initial guess, Newton’s method would look for a fixed point of the
function ‘`(x + 5/x) / 2`’. Putting a guess of 1 on the stack
and typing `H I V R ' ($ + 5/$)/2 RET` quickly yields the result
2.23607. This is equivalent to using the `a R` (`calc-find-root`)
command to find a root of the equation ‘`x^2 = 5`’.

These examples used numbers for ‘`a`’ values. Calc keeps applying
the function until two successive results are equal to within the
current precision. For complex numbers, both the real parts and the
imaginary parts must be equal to within the current precision. If
‘`a`’ is a formula (say, a variable name), then the function is
applied until two successive results are exactly the same formula.
It is up to you to ensure that the function will eventually converge;
if it doesn’t, you may have to press `C-g` to stop the Calculator.

The algebraic `fixp` function takes two optional arguments, ‘`n`’
and ‘`tol`’. The first is the maximum number of steps to be allowed,
and must be either an integer or the symbol ‘`inf`’ (infinity, the
default). The second is a convergence tolerance. If a tolerance is
specified, all results during the calculation must be numbers, not
formulas, and the iteration stops when the magnitude of the difference
between two successive results is less than or equal to the tolerance.
(This implies that a tolerance of zero iterates until the results are
exactly equal.)

Putting it all together, ‘`fixp(<(# + A/#)/2>, B, 20, 1e-10)`’
computes the square root of ‘`A`’ given the initial guess ‘`B`’,
stopping when the result is correct within the specified tolerance, or
when 20 steps have been taken, whichever is sooner.

---

Previous: [Nesting and Fixed Points](#Nesting-and-Fixed-Points), Up: [Reducing and Mapping Vectors](#Reducing-and-Mapping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 9.8.5 Generalized Products [¶](#Generalized-Products-1)

The `V O` (`calc-outer-product`) [`outer`] command applies
a given binary operator to all possible pairs of elements from two
vectors, to produce a matrix. For example, `V O *` with ‘`[a, b]`’
and ‘`[x, y, z]`’ on the stack produces a multiplication table:
‘`[[a x, a y, a z], [b x, b y, b z]]`’. Element r,c of
the result matrix is obtained by applying the operator to element r
of the lefthand vector and element c of the righthand vector.

The `V I` (`calc-inner-product`) [`inner`] command computes
the generalized inner product of two vectors or matrices, given a
“multiplicative” operator and an “additive” operator. These can each
actually be any binary operators; if they are ‘`*`’ and ‘`+`’,
respectively, the result is a standard matrix multiplication. Element
r,c of the result matrix is obtained by mapping the
multiplicative operator across row r of the lefthand matrix and
column c of the righthand matrix, and then reducing with the additive
operator. Just as for the standard `*` command, this can also do a
vector-matrix or matrix-vector inner product, or a vector-vector
generalized dot product.

Since `V I` requires two operators, it prompts twice. In each case,
you can use any of the usual methods for entering the operator. If you
use `$` twice to take both operator formulas from the stack, the
first (multiplicative) operator is taken from the top of the stack
and the second (additive) operator is taken from second-to-top.

---

Previous: [Reducing and Mapping Vectors](#Reducing-and-Mapping), Up: [Vector/Matrix Functions](#Matrix-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 9.9 Vector and Matrix Display Formats [¶](#Vector-and-Matrix-Display-Formats)

Commands for controlling vector and matrix display use the `v` prefix
instead of the usual `d` prefix. But they are display modes; in
particular, they are influenced by the `I` and `H` prefix keys
in the same way (see [Display Modes](#Display-Modes)). Matrix display is also
influenced by the `d O` (`calc-flat-language`) mode;
see [Normal Language Modes](#Normal-Language-Modes).

The commands `v <` (`calc-matrix-left-justify`), `v >`
(`calc-matrix-right-justify`), and `v =`
(`calc-matrix-center-justify`) control whether matrix elements
are justified to the left, right, or center of their columns.

The `v [` (`calc-vector-brackets`) command turns the square
brackets that surround vectors and matrices displayed in the stack on
and off. The `v {` (`calc-vector-braces`) and `v (`
(`calc-vector-parens`) commands use curly braces or parentheses,
respectively, instead of square brackets. For example, `v {` might
be used in preparation for yanking a matrix into a buffer running
Mathematica. (In fact, the Mathematica language mode uses this mode;
see [Mathematica Language Mode](#Mathematica-Language-Mode).) Note that, regardless of the
display mode, either brackets or braces may be used to enter vectors,
and parentheses may never be used for this purpose.

The `v ]` (`calc-matrix-brackets`) command controls the
“big” style display of matrices, for matrices which have more than
one row. It prompts for a string of code letters; currently
implemented letters are `R`, which enables brackets on each row
of the matrix; `O`, which enables outer brackets in opposite
corners of the matrix; and `C`, which enables commas or
semicolons at the ends of all rows but the last. The default format
is ‘`RO`’. (Before Calc 2.00, the format was fixed at ‘`ROC`’.)
Here are some example matrices:

```
[ [ 123,  0,   0  ]       [ [ 123,  0,   0  ],
  [  0,  123,  0  ]         [  0,  123,  0  ],
  [  0,   0,  123 ] ]       [  0,   0,  123 ] ]

         RO                        ROC
```

```
  [ 123,  0,   0            [ 123,  0,   0 ;
     0,  123,  0               0,  123,  0 ;
     0,   0,  123 ]            0,   0,  123 ]

          O                        OC
```

```
  [ 123,  0,   0  ]           123,  0,   0
  [  0,  123,  0  ]            0,  123,  0
  [  0,   0,  123 ]            0,   0,  123

          R                       blank
```

Note that of the formats shown here, ‘`RO`’, ‘`ROC`’, and
‘`OC`’ are all recognized as matrices during reading, while
the others are useful for display only.

The `v ,` (`calc-vector-commas`) command turns commas on and
off in vector and matrix display.

In vectors of length one, and in all vectors when commas have been
turned off, Calc adds extra parentheses around formulas that might
otherwise be ambiguous. For example, ‘`[a b]`’ could be a vector
of the one formula ‘`a b`’, or it could be a vector of two
variables with commas turned off. Calc will display the former
case as ‘`[(a b)]`’. You can disable these extra parentheses
(to make the output less cluttered at the expense of allowing some
ambiguity) by adding the letter `P` to the control string you
give to `v ]` (as described above).

The `v .` (`calc-full-vectors`) command turns abbreviated
display of long vectors on and off. In this mode, vectors of six
or more elements, or matrices of six or more rows or columns, will
be displayed in an abbreviated form that displays only the first
three elements and the last element: ‘`[a, b, c, ..., z]`’.
When very large vectors are involved this will substantially
improve Calc’s display speed.

The `t .` (`calc-full-trail-vectors`) command controls a
similar mode for recording vectors in the Trail. If you turn on
this mode, vectors of six or more elements and matrices of six or
more rows or columns will be abbreviated when they are put in the
Trail. The `t y` (`calc-trail-yank`) command will be
unable to recover those vectors. If you are working with very
large vectors, this mode will improve the speed of all operations
that involve the trail.

The `v /` (`calc-break-vectors`) command turns multi-line
vector display on and off. Normally, matrices are displayed with one
row per line but all other types of vectors are displayed in a single
line. This mode causes all vectors, whether matrices or not, to be
displayed with a single element per line. Sub-vectors within the
vectors will still use the normal linear form.

---

Next: [Operating on Units](#Units), Previous: [Vector/Matrix Functions](#Matrix-Functions), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 10 Algebra [¶](#Algebra-1)

This section covers the Calc features that help you work with
algebraic formulas. First, the general sub-formula selection
mechanism is described; this works in conjunction with any Calc
commands. Then, commands for specific algebraic operations are
described. Finally, the flexible *rewrite rule* mechanism
is discussed.

The algebraic commands use the `a` key prefix; selection
commands use the `j` (for “just a letter that wasn’t used
for anything else”) prefix.

See [Editing Stack Entries](#Editing-Stack-Entries), to see how to manipulate formulas
using regular Emacs editing commands.

When doing algebraic work, you may find several of the Calculator’s
modes to be helpful, including Algebraic Simplification mode (`m A`)
or No-Simplification mode (`m O`),
Algebraic entry mode (`m a`), Fraction mode (`m f`), and
Symbolic mode (`m s`). See [Mode Settings](#Mode-Settings), for discussions
of these modes. You may also wish to select Big display mode (`d B`).
See [Normal Language Modes](#Normal-Language-Modes).

* [Selecting Sub-Formulas](#Selecting-Subformulas)
* [Algebraic Manipulation](#Algebraic-Manipulation)
* [Simplifying Formulas](#Simplifying-Formulas)
* [Polynomials](#Polynomials)
* [Calculus](#Calculus)
* [Solving Equations](#Solving-Equations)
* [Numerical Solutions](#Numerical-Solutions)
* [Curve Fitting](#Curve-Fitting)
* [Summations](#Summations)
* [Logical Operations](#Logical-Operations)
* [Rewrite Rules](#Rewrite-Rules)

---

Next: [Algebraic Manipulation](#Algebraic-Manipulation), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 10.1 Selecting Sub-Formulas [¶](#Selecting-Sub_002dFormulas)

When working with an algebraic formula it is often necessary to
manipulate a portion of the formula rather than the formula as a
whole. Calc allows you to “select” a portion of any formula on
the stack. Commands which would normally operate on that stack
entry will now operate only on the sub-formula, leaving the
surrounding part of the stack entry alone.

One common non-algebraic use for selection involves vectors. To work
on one element of a vector in-place, simply select that element as a
“sub-formula” of the vector.

* [Making Selections](#Making-Selections)
* [Changing Selections](#Changing-Selections)
* [Displaying Selections](#Displaying-Selections)
* [Operating on Selections](#Operating-on-Selections)
* [Rearranging Formulas using Selections](#Rearranging-with-Selections)

---

Next: [Changing Selections](#Changing-Selections), Up: [Selecting Sub-Formulas](#Selecting-Subformulas)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.1.1 Making Selections [¶](#Making-Selections-1)

To select a sub-formula, move the Emacs cursor to any character in that
sub-formula, and press `j s` (`calc-select-here`). Calc will
highlight the smallest portion of the formula that contains that
character. By default the sub-formula is highlighted by blanking out
all of the rest of the formula with dots. Selection works in any
display mode but is perhaps easiest in Big mode (`d B`).
Suppose you enter the following formula:

```
           3    ___
    (a + b)  + V c
1:  ---------------
        2 x + 1
```

(by typing `' ((a+b)^3 + sqrt(c)) / (2x+1)`). If you move the
cursor to the letter ‘`b`’ and press `j s`, the display changes
to

```
           .    ...
    .. . b.  . . .
1*  ...............
        . . . .
```

Every character not part of the sub-formula ‘`b`’ has been changed
to a dot. (If the customizable variable
`calc-highlight-selections-with-faces` is non-`nil`, then the characters
not part of the sub-formula are de-emphasized by using a less
noticeable face instead of using dots. see [Displaying Selections](#Displaying-Selections).)
The ‘`*`’ next to the line number is to remind you that
the formula has a portion of it selected. (In this case, it’s very
obvious, but it might not always be. If Embedded mode is enabled,
the word ‘`Sel`’ also appears in the mode line because the stack
may not be visible. see [Embedded Mode](#Embedded-Mode).)

If you had instead placed the cursor on the parenthesis immediately to
the right of the ‘`b`’, the selection would have been:

```
           .    ...
    (a + b)  . . .
1*  ...............
        . . . .
```

The portion selected is always large enough to be considered a complete
formula all by itself, so selecting the parenthesis selects the whole
formula that it encloses. Putting the cursor on the ‘`+`’ sign
would have had the same effect.

(Strictly speaking, the Emacs cursor is really the manifestation of
the Emacs “point,” which is a position *between* two characters
in the buffer. So purists would say that Calc selects the smallest
sub-formula which contains the character to the right of “point.”)

If you supply a numeric prefix argument n, the selection is
expanded to the nth enclosing sub-formula. Thus, positioning
the cursor on the ‘`b`’ and typing `C-u 1 j s` will select
‘`a + b`’; typing `C-u 2 j s` will select ‘`(a + b)^3`’,
and so on.

If the cursor is not on any part of the formula, or if you give a
numeric prefix that is too large, the entire formula is selected.

If the cursor is on the ‘`.`’ line that marks the top of the stack
(i.e., its normal “rest position”), this command selects the entire
formula at stack level 1. Most selection commands similarly operate
on the formula at the top of the stack if you haven’t positioned the
cursor on any stack entry.

The `j a` (`calc-select-additional`) command enlarges the
current selection to encompass the cursor. To select the smallest
sub-formula defined by two different points, move to the first and
press `j s`, then move to the other and press `j a`. This
is roughly analogous to using `C-@` (`set-mark-command`) to
select the two ends of a region of text during normal Emacs editing.

The `j o` (`calc-select-once`) command selects a formula in
exactly the same way as `j s`, except that the selection will
last only as long as the next command that uses it. For example,
`j o 1 +` is a handy way to add one to the sub-formula indicated
by the cursor.

(A somewhat more precise definition: The `j o` command sets a flag
such that the next command involving selected stack entries will clear
the selections on those stack entries afterwards. All other selection
commands except `j a` and `j O` clear this flag.)

The `j S` (`calc-select-here-maybe`) and `j O`
(`calc-select-once-maybe`) commands are equivalent to `j s`
and `j o`, respectively, except that if the formula already
has a selection they have no effect. This is analogous to the
behavior of some commands such as `j r` (`calc-rewrite-selection`;
see [Selections with Rewrite Rules](#Selections-with-Rewrite-Rules)) and is mainly intended to be
used in keyboard macros that implement your own selection-oriented
commands.

Selection of sub-formulas normally treats associative terms like
‘`a + b - c + d`’ and ‘`x * y * z`’ as single levels of the formula.
If you place the cursor anywhere inside ‘`a + b - c + d`’ except
on one of the variable names and use `j s`, you will select the
entire four-term sum.

The `j b` (`calc-break-selections`) command controls a mode
in which the “deep structure” of these associative formulas shows
through. Calc actually stores the above formulas as
‘`((a + b) - c) + d`’ and ‘`x * (y * z)`’. (Note that for certain
obscure reasons, by default Calc treats multiplication as
right-associative.) Once you have enabled `j b` mode, selecting
with the cursor on the ‘`-`’ sign would only select the ‘`a + b -
c`’ portion, which makes sense when the deep structure of the sum is
considered. There is no way to select the ‘`b - c + d`’ portion;
although this might initially look like just as legitimate a sub-formula
as ‘`a + b - c`’, the deep structure shows that it isn’t. The `d
U` command can be used to view the deep structure of any formula
(see [Normal Language Modes](#Normal-Language-Modes)).

When `j b` mode has not been enabled, the deep structure is
generally hidden by the selection commands—what you see is what
you get.

The `j u` (`calc-unselect`) command unselects the formula
that the cursor is on. If there was no selection in the formula,
this command has no effect. With a numeric prefix argument, it
unselects the nth stack element rather than using the cursor
position.

The `j c` (`calc-clear-selections`) command unselects all
stack elements.

---

Next: [Displaying Selections](#Displaying-Selections), Previous: [Making Selections](#Making-Selections), Up: [Selecting Sub-Formulas](#Selecting-Subformulas)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.1.2 Changing Selections [¶](#Changing-Selections-1)

Once you have selected a sub-formula, you can expand it using the
`j m` (`calc-select-more`) command. If ‘`a + b`’ is
selected, pressing `j m` repeatedly works as follows:

```
           3    ...                3    ___                3    ___
    (a + b)  . . .          (a + b)  + V c          (a + b)  + V c
1*  ...............     1*  ...............     1*  ---------------
        . . . .                 . . . .                 2 x + 1
```

In the last example, the entire formula is selected. This is roughly
the same as having no selection at all, but because there are subtle
differences the ‘`*`’ character is still there on the line number.

With a numeric prefix argument n, `j m` expands n
times (or until the entire formula is selected). Note that `j s`
with argument n is equivalent to plain `j s` followed by
`j m` with argument n. If `j m` is used when there
is no current selection, it is equivalent to `j s`.

Even though `j m` does not explicitly use the location of the
cursor within the formula, it nevertheless uses the cursor to determine
which stack element to operate on. As usual, `j m` when the cursor
is not on any stack element operates on the top stack element.

The `j l` (`calc-select-less`) command reduces the current
selection around the cursor position. That is, it selects the
immediate sub-formula of the current selection which contains the
cursor, the opposite of `j m`. If the cursor is not inside the
current selection, the command de-selects the formula.

The `j 1` through `j 9` (`calc-select-part`) commands
select the nth sub-formula of the current selection. They are
like `j l` (`calc-select-less`) except they use counting
rather than the cursor position to decide which sub-formula to select.
For example, if the current selection is `a + b + c` or
`f(a, b, c)` or `[a, b, c]`, then `j 1` selects ‘`a`’,
`j 2` selects ‘`b`’, and `j 3` selects ‘`c`’; in each of
these cases, `j 4` through `j 9` would be errors.

If there is no current selection, `j 1` through `j 9` select
the nth top-level sub-formula. (In other words, they act as if
the entire stack entry were selected first.) To select the nth
sub-formula where n is greater than nine, you must instead invoke
`j 1` with n as a numeric prefix argument.

The `j n` (`calc-select-next`) and `j p`
(`calc-select-previous`) commands change the current selection
to the next or previous sub-formula at the same level. For example,
if ‘`b`’ is selected in ‘`2 + a*b*c + x`’, then `j n`
selects ‘`c`’. Further `j n` commands would be in error because,
even though there is something to the right of ‘`c`’ (namely, ‘`x`’),
it is not at the same level; in this case, it is not a term of the
same product as ‘`b`’ and ‘`c`’. However, `j m` (to select
the whole product ‘`a*b*c`’ as a term of the sum) followed by
`j n` would successfully select the ‘`x`’.

Similarly, `j p` moves the selection from the ‘`b`’ in this
sample formula to the ‘`a`’. Both commands accept numeric prefix
arguments to move several steps at a time.

It is interesting to compare Calc’s selection commands with the
Emacs Info system’s commands for navigating through hierarchically
organized documentation. Calc’s `j n` command is completely
analogous to Info’s `n` command. Likewise, `j p` maps to
`p`, `j 2` maps to `2`, and Info’s `u` is like `j m`.
(Note that `j u` stands for `calc-unselect`, not “up”.)
The Info `m` command is somewhat similar to Calc’s `j s` and
`j l`; in each case, you can jump directly to a sub-component
of the hierarchy simply by pointing to it with the cursor.

---

Next: [Operating on Selections](#Operating-on-Selections), Previous: [Changing Selections](#Changing-Selections), Up: [Selecting Sub-Formulas](#Selecting-Subformulas)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.1.3 Displaying Selections [¶](#Displaying-Selections-1)

The `j d` (`calc-show-selections`) command controls how
selected sub-formulas are displayed. One of the alternatives is
illustrated in the above examples; if we press `j d` we switch
to the other style in which the selected portion itself is obscured
by ‘`#`’ signs:

```
           3    ...                  #    ___
    (a + b)  . . .            ## # ##  + V c
1*  ...............       1*  ---------------
        . . . .                   2 x + 1
```

If the customizable variable
`calc-highlight-selections-with-faces` is non-`nil`, then the
non-selected portion of the formula will be de-emphasized by using a
less noticeable face (`calc-nonselected-face`) instead of dots
and the selected sub-formula will be highlighted by using a more
noticeable face (`calc-selected-face`) instead of ‘`#`’
signs. (see [Customizing Calc](#Customizing-Calc).)

---

Next: [Rearranging Formulas using Selections](#Rearranging-with-Selections), Previous: [Displaying Selections](#Displaying-Selections), Up: [Selecting Sub-Formulas](#Selecting-Subformulas)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.1.4 Operating on Selections [¶](#Operating-on-Selections-1)

Once a selection is made, all Calc commands that manipulate items
on the stack will operate on the selected portions of the items
instead. (Note that several stack elements may have selections
at once, though there can be only one selection at a time in any
given stack element.)

The `j e` (`calc-enable-selections`) command disables the
effect that selections have on Calc commands. The current selections
still exist, but Calc commands operate on whole stack elements anyway.
This mode can be identified by the fact that the ‘`*`’ markers on
the line numbers are gone, even though selections are visible. To
reactivate the selections, press `j e` again.

To extract a sub-formula as a new formula, simply select the
sub-formula and press `RET`. This normally duplicates the top
stack element; here it duplicates only the selected portion of that
element.

To replace a sub-formula with something different, you can enter the
new value onto the stack and press `TAB`. This normally exchanges
the top two stack elements; here it swaps the value you entered into
the selected portion of the formula, returning the old selected
portion to the top of the stack.

```
           3    ...                    ...                    ___
    (a + b)  . . .           17 x y . . .           17 x y + V c
2*  ...............      2*  .............      2:  -------------
        . . . .                 . . . .                2 x + 1

                                    3                      3
1:  17 x y               1:  (a + b)            1:  (a + b)
```

In this example we select a sub-formula of our original example,
enter a new formula, `TAB` it into place, then deselect to see
the complete, edited formula.

If you want to swap whole formulas around even though they contain
selections, just use `j e` before and after.

The `j '` (`calc-enter-selection`) command is another way
to replace a selected sub-formula. This command does an algebraic
entry just like the regular `'` key. When you press `RET`,
the formula you type replaces the original selection. You can use
the ‘`$`’ symbol in the formula to refer to the original
selection. If there is no selection in the formula under the cursor,
the cursor is used to make a temporary selection for the purposes of
the command. Thus, to change a term of a formula, all you have to
do is move the Emacs cursor to that term and press `j '`.

The `` j ` `` (`calc-edit-selection`) command is a similar
analogue of the `` ` `` (`calc-edit`) command. It edits the
selected sub-formula in a separate buffer. If there is no
selection, it edits the sub-formula indicated by the cursor.

To delete a sub-formula, press `DEL`. This generally replaces
the sub-formula with the constant zero, but in a few suitable contexts
it uses the constant one instead. The `DEL` key automatically
deselects and re-simplifies the entire formula afterwards. Thus:

```
              ###
    17 x y + # #          17 x y         17 # y          17 y
1*  -------------     1:  -------    1*  -------    1:  -------
       2 x + 1            2 x + 1        2 x + 1        2 x + 1
```

In this example, we first delete the ‘`sqrt(c)`’ term; Calc
accomplishes this by replacing ‘`sqrt(c)`’ with zero and
resimplifying. We then delete the `x` in the numerator;
since this is part of a product, Calc replaces it with ‘`1`’
and resimplifies.

If you select an element of a vector and press `DEL`, that
element is deleted from the vector. If you delete one side of
an equation or inequality, only the opposite side remains.

The `j DEL` (`calc-del-selection`) command is like
`DEL` but with the auto-selecting behavior of `j '` and
`` j ` ``. It deletes the selected portion of the formula
indicated by the cursor, or, in the absence of a selection, it
deletes the sub-formula indicated by the cursor position.

(There is also an auto-selecting `j RET` (`calc-copy-selection`)
command.)

Normal arithmetic operations also apply to sub-formulas. Here we
select the denominator, press `5 -` to subtract five from the
denominator, press `n` to negate the denominator, then
press `Q` to take the square root.

```
     .. .           .. .           .. .             .. .
1*  .......    1*  .......    1*  .......    1*  ..........
    2 x + 1        2 x - 4        4 - 2 x         _________
                                                 V 4 - 2 x
```

Certain types of operations on selections are not allowed. For
example, for an arithmetic function like `-` no more than one of
the arguments may be a selected sub-formula. (As the above example
shows, the result of the subtraction is spliced back into the argument
which had the selection; if there were more than one selection involved,
this would not be well-defined.) If you try to subtract two selections,
the command will abort with an error message.

Operations on sub-formulas sometimes leave the formula as a whole
in an “un-natural” state. Consider negating the ‘`2 x`’ term
of our sample formula by selecting it and pressing `n`
(`calc-change-sign`).

```
       .. .                .. .
1*  ..........      1*  ...........
     .........           ..........
    . . . 2 x           . . . -2 x
```

Unselecting the sub-formula reveals that the minus sign, which would
normally have canceled out with the subtraction automatically, has
not been able to do so because the subtraction was not part of the
selected portion. Pressing `=` (`calc-evaluate`) or doing
any other mathematical operation on the whole formula will cause it
to be simplified.

```
       17 y                17 y
1:  -----------     1:  ----------
     __________          _________
    V 4 - -2 x          V 4 + 2 x
```

---

Previous: [Operating on Selections](#Operating-on-Selections), Up: [Selecting Sub-Formulas](#Selecting-Subformulas)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.1.5 Rearranging Formulas using Selections [¶](#Rearranging-Formulas-using-Selections)

The `j R` (`calc-commute-right`) command moves the selected
sub-formula to the right in its surrounding formula. Generally the
selection is one term of a sum or product; the sum or product is
rearranged according to the commutative laws of algebra.

As with `j '` and `j DEL`, the term under the cursor is used
if there is no selection in the current formula. All commands described
in this section share this property. In this example, we place the
cursor on the ‘`a`’ and type `j R`, then repeat.

```
1:  a + b - c          1:  b + a - c          1:  b - c + a
```

Note that in the final step above, the ‘`a`’ is switched with
the ‘`c`’ but the signs are adjusted accordingly. When moving
terms of sums and products, `j R` will never change the
mathematical meaning of the formula.

The selected term may also be an element of a vector or an argument
of a function. The term is exchanged with the one to its right.
In this case, the “meaning” of the vector or function may of
course be drastically changed.

```
1:  [a, b, c]          1:  [b, a, c]          1:  [b, c, a]

1:  f(a, b, c)         1:  f(b, a, c)         1:  f(b, c, a)
```

The `j L` (`calc-commute-left`) command is like `j R`
except that it swaps the selected term with the one to its left.

With numeric prefix arguments, these commands move the selected
term several steps at a time. It is an error to try to move a
term left or right past the end of its enclosing formula.
With numeric prefix arguments of zero, these commands move the
selected term as far as possible in the given direction.

The `j D` (`calc-sel-distribute`) command mixes the selected
sum or product into the surrounding formula using the distributive
law. For example, in ‘`a * (b - c)`’ with the ‘`b - c`’
selected, the result is ‘`a b - a c`’. This also distributes
products or quotients into surrounding powers, and can also do
transformations like ‘`exp(a + b)`’ to ‘`exp(a) exp(b)`’,
where ‘`a + b`’ is the selected term, and ‘`ln(a ^ b)`’
to ‘`ln(a) b`’, where ‘`a ^ b`’ is the selected term.

For multiple-term sums or products, `j D` takes off one term
at a time: ‘`a * (b + c - d)`’ goes to ‘`a * (c - d) + a b`’
with the ‘`c - d`’ selected so that you can type `j D`
repeatedly to expand completely. The `j D` command allows a
numeric prefix argument which specifies the maximum number of
times to expand at once; the default is one time only.

The `j D` command is implemented using rewrite rules.
See [Selections with Rewrite Rules](#Selections-with-Rewrite-Rules). The rules are stored in
the Calc variable `DistribRules`. A convenient way to view
these rules is to use `s e` (`calc-edit-variable`) which
displays and edits the stored value of a variable. Press `C-c C-c`
to return from editing mode; be careful not to make any actual changes
or else you will affect the behavior of future `j D` commands!

To extend `j D` to handle new cases, just edit `DistribRules`
as described above. You can then use the `s p` command to save
this variable’s value permanently for future Calc sessions.
See [Other Operations on Variables](#Operations-on-Variables).

The `j M` (`calc-sel-merge`) command is the complement
of `j D`; given ‘`a b - a c`’ with either ‘`a b`’ or
‘`a c`’ selected, the result is ‘`a * (b - c)`’. Once
again, `j M` can also merge calls to functions like `exp`
and `ln`; examine the variable `MergeRules` to see all
the relevant rules.

The `j C` (`calc-sel-commute`) command swaps the arguments
of the selected sum, product, or equation. It always behaves as
if `j b` mode were in effect, i.e., the sum ‘`a + b + c`’ is
treated as the nested sums ‘`(a + b) + c`’ by this command.
If you put the cursor on the first ‘`+`’, the result is
‘`(b + a) + c`’; if you put the cursor on the second ‘`+`’, the
result is ‘`c + (a + b)`’ (which the default simplifications
will rearrange to ‘`(c + a) + b`’). The relevant rules are stored
in the variable `CommuteRules`.

You may need to turn default simplifications off (with the `m O`
command) in order to get the full benefit of `j C`. For example,
commuting ‘`a - b`’ produces ‘`-b + a`’, but the default
simplifications will “simplify” this right back to ‘`a - b`’ if
you don’t turn them off. The same is true of some of the other
manipulations described in this section.

The `j N` (`calc-sel-negate`) command replaces the selected
term with the negative of that term, then adjusts the surrounding
formula in order to preserve the meaning. For example, given
‘`exp(a - b)`’ where ‘`a - b`’ is selected, the result is
‘`1 / exp(b - a)`’. By contrast, selecting a term and using the
regular `n` (`calc-change-sign`) command negates the
term without adjusting the surroundings, thus changing the meaning
of the formula as a whole. The rules variable is `NegateRules`.

The `j &` (`calc-sel-invert`) command is similar to `j N`
except it takes the reciprocal of the selected term. For example,
given ‘`a - ln(b)`’ with ‘`b`’ selected, the result is
‘`a + ln(1/b)`’. The rules variable is `InvertRules`.

The `j E` (`calc-sel-jump-equals`) command moves the
selected term from one side of an equation to the other. Given
‘`a + b = c + d`’ with ‘`c`’ selected, the result is
‘`a + b - c = d`’. This command also works if the selected
term is part of a ‘`*`’, ‘`/`’, or ‘`^`’ formula. The
relevant rules variable is `JumpRules`.

The `j I` (`calc-sel-isolate`) command isolates the
selected term on its side of an equation. It uses the `a S`
(`calc-solve-for`) command to solve the equation, and the
Hyperbolic flag affects it in the same way. See [Solving Equations](#Solving-Equations).
When it applies, `j I` is often easier to use than `j E`.
It understands more rules of algebra, and works for inequalities
as well as equations.

The `j *` (`calc-sel-mult-both-sides`) command prompts for a
formula using algebraic entry, then multiplies both sides of the
selected quotient or equation by that formula. It performs the
default algebraic simplifications before re-forming the
quotient or equation. You can suppress this simplification by
providing a prefix argument: `C-u j *`. There is also a `j /`
(`calc-sel-div-both-sides`) which is similar to `j *` but
dividing instead of multiplying by the factor you enter.

If the selection is a quotient with numerator 1, then Calc’s default
simplifications would normally cancel the new factors. To prevent
this, when the `j *` command is used on a selection whose numerator is
1 or -1, the denominator is expanded at the top level using the
distributive law (as if using the `C-u 1 a x` command). Suppose the
formula on the stack is ‘`1 / (a + 1)`’ and you wish to multiplying the
top and bottom by ‘`a - 1`’. Calc’s default simplifications would
normally change the result ‘`(a - 1) /(a + 1) (a - 1)`’ back
to the original form by cancellation; when `j *` is used, Calc
expands the denominator to ‘`a (a - 1) + a - 1`’ to prevent this.

If you wish the `j *` command to completely expand the denominator
of a quotient you can call it with a zero prefix: `C-u 0 j *`. For
example, if the formula on the stack is ‘`1 / (sqrt(a) + 1)`’, you may
wish to eliminate the square root in the denominator by multiplying
the top and bottom by ‘`sqrt(a) - 1`’. If you did this simply by using
a simple `j *` command, you would get
‘`(sqrt(a)-1)/ (sqrt(a) (sqrt(a) - 1) + sqrt(a) - 1)`’. Instead,
you would probably want to use `C-u 0 j *`, which would expand the
bottom and give you the desired result ‘`(sqrt(a)-1)/(a-1)`’. More
generally, if `j *` is called with an argument of a positive
integer n, then the denominator of the expression will be
expanded n times (as if with the `C-u n a x` command).

If the selection is an inequality, `j *` and `j /` will
accept any factor, but will warn unless they can prove the factor
is either positive or negative. (In the latter case the direction
of the inequality will be switched appropriately.) See [Declarations](#Declarations),
for ways to inform Calc that a given variable is positive or
negative. If Calc can’t tell for sure what the sign of the factor
will be, it will assume it is positive and display a warning
message.

For selections that are not quotients, equations, or inequalities,
these commands pull out a multiplicative factor: They divide (or
multiply) by the entered formula, simplify, then multiply (or divide)
back by the formula.

The `j +` (`calc-sel-add-both-sides`) and `j -`
(`calc-sel-sub-both-sides`) commands analogously add to or
subtract from both sides of an equation or inequality. For other
types of selections, they extract an additive factor. A numeric
prefix argument suppresses simplification of the intermediate
results.

The `j U` (`calc-sel-unpack`) command replaces the
selected function call with its argument. For example, given
‘`a + sin(x^2)`’ with ‘`sin(x^2)`’ selected, the result
is ‘`a + x^2`’. (The ‘`x^2`’ will remain selected; if you
wanted to change the `sin` to `cos`, just press `C`
now to take the cosine of the selected part.)

The `j v` (`calc-sel-evaluate`) command performs the
basic simplifications on the selected sub-formula.
These simplifications would normally be done automatically
on all results, but may have been partially inhibited by
previous selection-related operations, or turned off altogether
by the `m O` command. This command is just an auto-selecting
version of the `a v` command (see [Algebraic Manipulation](#Algebraic-Manipulation)).

With a numeric prefix argument of 2, `C-u 2 j v` applies
the default algebraic simplifications to the selected
sub-formula. With a prefix argument of 3 or more, e.g., `C-u j v`
applies the `a e` (`calc-simplify-extended`) command.
See [Simplifying Formulas](#Simplifying-Formulas). With a negative prefix argument
it simplifies at the top level only, just as with `a v`.
Here the “top” level refers to the top level of the selected
sub-formula.

The `j "` (`calc-sel-expand-formula`) command is to `a "`
(see [Algebraic Manipulation](#Algebraic-Manipulation)) what `j v` is to `a v`.

You can use the `j r` (`calc-rewrite-selection`) command
to define other algebraic operations on sub-formulas. See [Rewrite Rules](#Rewrite-Rules).

---

Next: [Simplifying Formulas](#Simplifying-Formulas), Previous: [Selecting Sub-Formulas](#Selecting-Subformulas), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
