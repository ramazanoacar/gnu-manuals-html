### 10.10 Logical Operations [¶](#Logical-Operations-1)

The following commands and algebraic functions return true/false values,
where 1 represents “true” and 0 represents “false.” In cases where
a truth value is required (such as for the condition part of a rewrite
rule, or as the condition for a `Z [ Z ]` control structure), any
nonzero value is accepted to mean “true.” (Specifically, anything
for which `dnonzero` returns 1 is “true,” and anything for
which `dnonzero` returns 0 or cannot decide is assumed “false.”
Note that this means that `Z [ Z ]` will execute the “then”
portion if its condition is provably true, but it will execute the
“else” portion for any condition like ‘`a = b`’ that is not
provably true, even if it might be true. Algebraic functions that
have conditions as arguments, like `? :` and `&&`, remain
unevaluated if the condition is neither provably true nor provably
false. See [Declarations](#Declarations).)

The `a =` (`calc-equal-to`) command, or ‘`eq(a,b)`’ function
(which can also be written ‘`a = b`’ or ‘`a == b`’ in an algebraic
formula) is true if ‘`a`’ and ‘`b`’ are equal, either because they
are identical expressions, or because they are numbers which are
numerically equal. (Thus the integer 1 is considered equal to the float
1.0.) If the equality of ‘`a`’ and ‘`b`’ cannot be determined,
the comparison is left in symbolic form. Note that as a command, this
operation pops two values from the stack and pushes back either a 1 or
a 0, or a formula ‘`a = b`’ if the values’ equality cannot be determined.

Many Calc commands use ‘`=`’ formulas to represent *equations*.
For example, the `a S` (`calc-solve-for`) command rearranges
an equation to solve for a given variable. The `a M`
(`calc-map-equation`) command can be used to apply any
function to both sides of an equation; for example, `2 a M *`
multiplies both sides of the equation by two. Note that just
`2 *` would not do the same thing; it would produce the formula
‘`2 (a = b)`’ which represents 2 if the equality is true or
zero if not.

The `eq` function with more than two arguments (e.g., `C-u 3 a =`
or ‘`a = b = c`’) tests if all of its arguments are equal. In
algebraic notation, the ‘`=`’ operator is unusual in that it is
neither left- nor right-associative: ‘`a = b = c`’ is not the
same as ‘`(a = b) = c`’ or ‘`a = (b = c)`’ (which each compare
one variable with the 1 or 0 that results from comparing two other
variables).

The `a #` (`calc-not-equal-to`) command, or ‘`neq(a,b)`’ or
‘`a != b`’ function, is true if ‘`a`’ and ‘`b`’ are not equal.
This also works with more than two arguments; ‘`a != b != c != d`’
tests that all four of ‘`a`’, ‘`b`’, ‘`c`’, and ‘`d`’ are
distinct numbers.

The `a <` (`calc-less-than`) [‘`lt(a,b)`’ or ‘`a < b`’]
operation is true if ‘`a`’ is less than ‘`b`’. Similar functions
are `a >` (`calc-greater-than`) [‘`gt(a,b)`’ or ‘`a > b`’],
`a [` (`calc-less-equal`) [‘`leq(a,b)`’ or ‘`a <= b`’], and
`a ]` (`calc-greater-equal`) [‘`geq(a,b)`’ or ‘`a >= b`’].

While the inequality functions like `lt` do not accept more
than two arguments, the syntax ‘`a <= b < c`’ is translated to an
equivalent expression involving intervals: ‘`b in [a .. c)`’.
(See the description of `in` below.) All four combinations
of ‘`<`’ and ‘`<=`’ are allowed, or any of the four combinations
of ‘`>`’ and ‘`>=`’. Four-argument constructions like
‘`a < b < c < d`’, and mixtures like ‘`a < b = c`’ that
involve both equations and inequalities, are not allowed.

The `a .` (`calc-remove-equal`) [`rmeq`] command extracts
the righthand side of the equation or inequality on the top of the
stack. It also works elementwise on vectors. For example, if
‘`[x = 2.34, y = z / 2]`’ is on the stack, then `a .` produces
‘`[2.34, z / 2]`’. As a special case, if the righthand side is a
variable and the lefthand side is a number (as in ‘`2.34 = x`’), then
Calc keeps the lefthand side instead. Finally, this command works with
assignments ‘`x := 2.34`’ as well as equations, always taking the
righthand side, and for ‘`=>`’ (evaluates-to) operators, always
taking the lefthand side.

The `a &` (`calc-logical-and`) [‘`land(a,b)`’ or ‘`a && b`’]
function is true if both of its arguments are true, i.e., are
non-zero numbers. In this case, the result will be either ‘`a`’ or
‘`b`’, chosen arbitrarily. If either argument is zero, the result is
zero. Otherwise, the formula is left in symbolic form.

The `a |` (`calc-logical-or`) [‘`lor(a,b)`’ or ‘`a || b`’]
function is true if either or both of its arguments are true (nonzero).
The result is whichever argument was nonzero, choosing arbitrarily if both
are nonzero. If both ‘`a`’ and ‘`b`’ are zero, the result is
zero.

The `a !` (`calc-logical-not`) [‘`lnot(a)`’ or ‘`! a`’]
function is true if ‘`a`’ is false (zero), or false if ‘`a`’ is
true (nonzero). It is left in symbolic form if ‘`a`’ is not a
number.

The `a :` (`calc-logical-if`) [‘`if(a,b,c)`’ or ‘`a ? b : c`’]
function is equal to either ‘`b`’ or ‘`c`’ if ‘`a`’ is a nonzero
number or zero, respectively. If ‘`a`’ is not a number, the test is
left in symbolic form and neither ‘`b`’ nor ‘`c`’ is evaluated in
any way. In algebraic formulas, this is one of the few Calc functions
whose arguments are not automatically evaluated when the function itself
is evaluated. The others are `lambda`, `quote`, and
`condition`.

One minor surprise to watch out for is that the formula ‘`a?3:4`’
will not work because the ‘`3:4`’ is parsed as a fraction instead of
as three separate symbols. Type something like ‘`a ? 3 : 4`’ or
‘`a?(3):4`’ instead.

As a special case, if ‘`a`’ evaluates to a vector, then both ‘`b`’
and ‘`c`’ are evaluated; the result is a vector of the same length
as ‘`a`’ whose elements are chosen from corresponding elements of
‘`b`’ and ‘`c`’ according to whether each element of ‘`a`’
is zero or nonzero. Each of ‘`b`’ and ‘`c`’ must be either a
vector of the same length as ‘`a`’, or a non-vector which is matched
with all elements of ‘`a`’.

The `a {` (`calc-in-set`) [‘`in(a,b)`’] function is true if
the number ‘`a`’ is in the set of numbers represented by ‘`b`’.
If ‘`b`’ is an interval form, ‘`a`’ must be one of the values
encompassed by the interval. If ‘`b`’ is a vector, ‘`a`’ must be
equal to one of the elements of the vector. (If any vector elements are
intervals, ‘`a`’ must be in any of the intervals.) If ‘`b`’ is a
plain number, ‘`a`’ must be numerically equal to ‘`b`’.
See [Set Operations using Vectors](#Set-Operations), for a group of commands that manipulate sets
of this sort.

The ‘`typeof(a)`’ function produces an integer or variable which
characterizes ‘`a`’. If ‘`a`’ is a number, vector, or variable,
the result will be one of the following numbers:

```
 1   Integer
 2   Fraction
 3   Floating-point number
 4   HMS form
 5   Rectangular complex number
 6   Polar complex number
 7   Error form
 8   Interval form
 9   Modulo form
10   Date-only form
11   Date/time form
12   Infinity (inf, uinf, or nan)
100  Variable
101  Vector (but not a matrix)
102  Matrix
```

Otherwise, ‘`a`’ is a formula, and the result is a variable which
represents the name of the top-level function call.

The ‘`integer(a)`’ function returns true if ‘`a`’ is an integer.
The ‘`real(a)`’ function
is true if ‘`a`’ is a real number, either integer, fraction, or
float. The ‘`constant(a)`’ function returns true if ‘`a`’ is
any of the objects for which `typeof` would produce an integer
code result except for variables, and provided that the components of
an object like a vector or error form are themselves constant.
Note that infinities do not satisfy any of these tests, nor do
special constants like `pi` and `e`.

See [Declarations](#Declarations), for a set of similar functions that recognize
formulas as well as actual numbers. For example, ‘`dint(floor(x))`’
is true because ‘`floor(x)`’ is provably integer-valued, but
‘`integer(floor(x))`’ does not because ‘`floor(x)`’ is not
literally an integer constant.

The ‘`refers(a,b)`’ function is true if the variable (or sub-expression)
‘`b`’ appears in ‘`a`’, or false otherwise. Unlike the other
tests described here, this function returns a definite “no” answer
even if its arguments are still in symbolic form. The only case where
`refers` will be left unevaluated is if ‘`a`’ is a plain
variable (different from ‘`b`’).

The ‘`negative(a)`’ function returns true if ‘`a`’ “looks” negative,
because it is a negative number, because it is of the form ‘`-x`’,
or because it is a product or quotient with a term that looks negative.
This is most useful in rewrite rules. Beware that ‘`negative(a)`’
evaluates to 1 or 0 for *any* argument ‘`a`’, so it can only
be stored in a formula if the default simplifications are turned off
first with `m O` (or if it appears in an unevaluated context such
as a rewrite rule condition).

The ‘`variable(a)`’ function is true if ‘`a`’ is a variable,
or false if not. If ‘`a`’ is a function call, this test is left
in symbolic form. Built-in variables like `pi` and `inf`
are considered variables like any others by this test.

The ‘`nonvar(a)`’ function is true if ‘`a`’ is a non-variable.
If its argument is a variable it is left unsimplified; it never
actually returns zero. However, since Calc’s condition-testing
commands consider “false” anything not provably true, this is
often good enough.

The functions `lin`, `linnt`, `islin`, and `islinnt`
check if an expression is “linear,” i.e., can be written in the form
‘`a + b x`’ for some constants ‘`a`’ and ‘`b`’, and some
variable or subformula ‘`x`’. The function ‘`islin(f,x)`’ checks
if formula ‘`f`’ is linear in ‘`x`’, returning 1 if so. For
example, ‘`islin(x,x)`’, ‘`islin(-x,x)`’, ‘`islin(3,x)`’, and
‘`islin(x y / 3 - 2, x)`’ all return 1. The ‘`lin(f,x)`’ function
is similar, except that instead of returning 1 it returns the vector
‘`[a, b, x]`’. For the above examples, this vector would be
‘`[0, 1, x]`’, ‘`[0, -1, x]`’, ‘`[3, 0, x]`’, and
‘`[-2, y/3, x]`’, respectively. Both `lin` and `islin`
generally remain unevaluated for expressions which are not linear,
e.g., ‘`lin(2 x^2, x)`’ and ‘`lin(sin(x), x)`’. The second
argument can also be a formula; ‘`islin(2 + 3 sin(x), sin(x))`’
returns true.

The `linnt` and `islinnt` functions perform a similar check,
but require a “non-trivial” linear form, which means that the
‘`b`’ coefficient must be non-zero. For example, ‘`lin(2,x)`’
returns ‘`[2, 0, x]`’ and ‘`lin(y,x)`’ returns ‘`[y, 0, x]`’,
but ‘`linnt(2,x)`’ and ‘`linnt(y,x)`’ are left unevaluated
(in other words, these formulas are considered to be only “trivially”
linear in ‘`x`’).

All four linearity-testing functions allow you to omit the second
argument, in which case the input may be linear in any non-constant
formula. Here, the ‘`a=0`’, ‘`b=1`’ case is also considered
trivial, and only constant values for ‘`a`’ and ‘`b`’ are
recognized. Thus, ‘`lin(2 x y)`’ returns ‘`[0, 2, x y]`’,
‘`lin(2 - x y)`’ returns ‘`[2, -1, x y]`’, and ‘`lin(x y)`’
returns ‘`[0, 1, x y]`’. The `linnt` function would allow the
first two cases but not the third. Also, neither `lin` nor
`linnt` accept plain constants as linear in the one-argument
case: ‘`islin(2,x)`’ is true, but ‘`islin(2)`’ is false.

The ‘`istrue(a)`’ function returns 1 if ‘`a`’ is a nonzero
number or provably nonzero formula, or 0 if ‘`a`’ is anything else.
Calls to `istrue` can only be manipulated if `m O` mode is
used to make sure they are not evaluated prematurely. (Note that
declarations are used when deciding whether a formula is true;
`istrue` returns 1 when `dnonzero` would return 1, and
it returns 0 when `dnonzero` would return 0 or leave itself
in symbolic form.)

---

Previous: [Logical Operations](#Logical-Operations), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
