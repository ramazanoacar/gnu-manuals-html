### 10.6 Solving Equations [¶](#Solving-Equations-1)

The `a S` (`calc-solve-for`) [`solve`] command rearranges
an equation to solve for a specific variable. An equation is an
expression of the form ‘`L = R`’. For example, the command `a S x`
will rearrange ‘`y = 3x + 6`’ to the form, ‘`x = y/3 - 2`’. If the
input is not an equation, it is treated like an equation of the
form ‘`X = 0`’.

This command also works for inequalities, as in ‘`y < 3x + 6`’.
Some inequalities cannot be solved where the analogous equation could
be; for example, solving
‘`a < b c`’
for ‘`b`’ is impossible
without knowing the sign of ‘`c`’. In this case, `a S` will
produce the result
‘`b != a/c`’
(using the not-equal-to operator) to signify that the direction of the
inequality is now unknown. The inequality
‘`a <= b c`’
is not even partially solved. See [Declarations](#Declarations), for a way to tell
Calc that the signs of the variables in a formula are in fact known.

Two useful commands for working with the result of `a S` are
`a .` (see [Logical Operations](#Logical-Operations)), which converts ‘`x = y/3 - 2`’
to ‘`y/3 - 2`’, and `s l` (see [The Let Command](#Let-Command)) which evaluates
another formula with ‘`x`’ set equal to ‘`y/3 - 2`’.

* [Multiple Solutions](#Multiple-Solutions)
* [Solving Systems of Equations](#Solving-Systems-of-Equations)
* [Decomposing Polynomials](#Decomposing-Polynomials)

---

Next: [Solving Systems of Equations](#Solving-Systems-of-Equations), Up: [Solving Equations](#Solving-Equations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.6.1 Multiple Solutions [¶](#Multiple-Solutions-1)

Some equations have more than one solution. The Hyperbolic flag
(`H a S`) [`fsolve`] tells the solver to report the fully
general family of solutions. It will invent variables `n1`,
`n2`, …, which represent independent arbitrary integers, and
`s1`, `s2`, …, which represent independent arbitrary
signs (either *+1* or *-1*). If you don’t use the Hyperbolic
flag, Calc will use zero in place of all arbitrary integers, and plus
one in place of all arbitrary signs. Note that variables like `n1`
and `s1` are not given any special interpretation in Calc except by
the equation solver itself. As usual, you can use the `s l`
(`calc-let`) command to obtain solutions for various actual values
of these variables.

For example, `' x^2 = y RET H a S x RET` solves to
get ‘`x = s1 sqrt(y)`’, indicating that the two solutions to the
equation are ‘`sqrt(y)`’ and ‘`-sqrt(y)`’. Another way to
think about it is that the square-root operation is really a
two-valued function; since every Calc function must return a
single result, `sqrt` chooses to return the positive result.
Then `H a S` doctors this result using `s1` to indicate
the full set of possible values of the mathematical square-root.

There is a similar phenomenon going the other direction: Suppose
we solve ‘`sqrt(y) = x`’ for `y`. Calc squares both sides
to get ‘`y = x^2`’. This is correct, except that it introduces
some dubious solutions. Consider solving ‘`sqrt(y) = -3`’:
Calc will report ‘`y = 9`’ as a valid solution, which is true
in the mathematical sense of square-root, but false (there is no
solution) for the actual Calc positive-valued `sqrt`. This
happens for both `a S` and `H a S`.

If you store a positive integer in the Calc variable `GenCount`,
then Calc will generate formulas of the form ‘`as(n)`’ for
arbitrary signs, and ‘`an(n)`’ for arbitrary integers,
where n represents successive values taken by incrementing
`GenCount` by one. While the normal arbitrary sign and
integer symbols start over at `s1` and `n1` with each
new Calc command, the `GenCount` approach will give each
arbitrary value a name that is unique throughout the entire Calc
session. Also, the arbitrary values are function calls instead
of variables, which is advantageous in some cases. For example,
you can make a rewrite rule that recognizes all arbitrary signs
using a pattern like ‘`as(n)`’. The `s l` command only works
on variables, but you can use the `a b` (`calc-substitute`)
command to substitute actual values for function calls like ‘`as(3)`’.

The `s G` (`calc-edit-GenCount`) command is a convenient
way to create or edit this variable. Press `C-c C-c` to finish.

If you have not stored a value in `GenCount`, or if the value
in that variable is not a positive integer, the regular
`s1`/`n1` notation is used.

With the Inverse flag, `I a S` [`finv`] treats the expression
on top of the stack as a function of the specified variable and solves
to find the inverse function, written in terms of the same variable.
For example, `I a S x` inverts ‘`2x + 6`’ to ‘`x/2 - 3`’.
You can use both Inverse and Hyperbolic [`ffinv`] to obtain a
fully general inverse, as described above.

Some equations, specifically polynomials, have a known, finite number
of solutions. The `a P` (`calc-poly-roots`) [`roots`]
command uses `H a S` to solve an equation in general form, then, for
all arbitrary-sign variables like `s1`, and all arbitrary-integer
variables like `n1` for which `n1` only usefully varies over
a finite range, it expands these variables out to all their possible
values. The results are collected into a vector, which is returned.
For example, ‘`roots(x^4 = 1, x)`’ returns the four solutions
‘`[1, -1, (0, 1), (0, -1)]`’. Generally an nth degree
polynomial will always have n roots on the complex plane.
(If you have given a `real` declaration for the solution
variable, then only the real-valued solutions, if any, will be
reported; see [Declarations](#Declarations).)

Note that because `a P` uses `H a S`, it is able to deliver
symbolic solutions if the polynomial has symbolic coefficients. Also
note that Calc’s solver is not able to get exact symbolic solutions
to all polynomials. Polynomials containing powers up to ‘`x^4`’
can always be solved exactly; polynomials of higher degree sometimes
can be: ‘`x^6 + x^3 + 1`’ is converted to ‘`(x^3)^2 + (x^3) + 1`’,
which can be solved for ‘`x^3`’ using the quadratic equation, and then
for ‘`x`’ by taking cube roots. But in many cases, like
‘`x^6 + x + 1`’, Calc does not know how to rewrite the polynomial
into a form it can solve. The `a P` command can still deliver a
list of numerical roots, however, provided that Symbolic mode (`m s`)
is not turned on. (If you work with Symbolic mode on, recall that the
`N` (`calc-eval-num`) key is a handy way to reevaluate the
formula on the stack with Symbolic mode temporarily off.) Naturally,
`a P` can only provide numerical roots if the polynomial coefficients
are all numbers (real or complex).

---

Next: [Decomposing Polynomials](#Decomposing-Polynomials), Previous: [Multiple Solutions](#Multiple-Solutions), Up: [Solving Equations](#Solving-Equations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.6.2 Solving Systems of Equations [¶](#Solving-Systems-of-Equations-1)

You can also use the commands described above to solve systems of
simultaneous equations. Just create a vector of equations, then
specify a vector of variables for which to solve. (You can omit
the surrounding brackets when entering the vector of variables
at the prompt.)

For example, putting ‘`[x + y = a, x - y = b]`’ on the stack
and typing `a S x,y RET` produces the vector of solutions
‘`[x = a - (a-b)/2, y = (a-b)/2]`’. The result vector will
have the same length as the variables vector, and the variables
will be listed in the same order there. Note that the solutions
are not always simplified as far as possible; the solution for
‘`x`’ here could be improved by an application of the `a n`
command.

Calc’s algorithm works by trying to eliminate one variable at a
time by solving one of the equations for that variable and then
substituting into the other equations. Calc will try all the
possibilities, but you can speed things up by noting that Calc
first tries to eliminate the first variable with the first
equation, then the second variable with the second equation,
and so on. It also helps to put the simpler (e.g., more linear)
equations toward the front of the list. Calc’s algorithm will
solve any system of linear equations, and also many kinds of
nonlinear systems.

Normally there will be as many variables as equations. If you
give fewer variables than equations (an “over-determined” system
of equations), Calc will find a partial solution. For example,
typing `a S y RET` with the above system of equations
would produce ‘`[y = a - x]`’. There are now several ways to
express this solution in terms of the original variables; Calc uses
the first one that it finds. You can control the choice by adding
variable specifiers of the form ‘`elim(v)`’ to the
variables list. This says that v should be eliminated from
the equations; the variable will not appear at all in the solution.
For example, typing `a S y,elim(x)` would yield
‘`[y = a - (b+a)/2]`’.

If the variables list contains only `elim` specifiers,
Calc simply eliminates those variables from the equations
and then returns the resulting set of equations. For example,
`a S elim(x)` produces ‘`[a - 2 y = b]`’. Every variable
eliminated will reduce the number of equations in the system
by one.

Again, `a S` gives you one solution to the system of
equations. If there are several solutions, you can use `H a S`
to get a general family of solutions, or, if there is a finite
number of solutions, you can use `a P` to get a list. (In
the latter case, the result will take the form of a matrix where
the rows are different solutions and the columns correspond to the
variables you requested.)

Another way to deal with certain kinds of overdetermined systems of
equations is the `a F` command, which does least-squares fitting
to satisfy the equations. See [Curve Fitting](#Curve-Fitting).

---

Previous: [Solving Systems of Equations](#Solving-Systems-of-Equations), Up: [Solving Equations](#Solving-Equations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.6.3 Decomposing Polynomials [¶](#Decomposing-Polynomials-1)

The `poly` function takes a polynomial and a variable as
arguments, and returns a vector of polynomial coefficients (constant
coefficient first). For example, ‘`poly(x^3 + 2 x, x)`’ returns
‘`[0, 2, 0, 1]`’. If the input is not a polynomial in ‘`x`’,
the call to `poly` is left in symbolic form. If the input does
not involve the variable ‘`x`’, the input is returned in a list
of length one, representing a polynomial with only a constant
coefficient. The call ‘`poly(x, x)`’ returns the vector ‘`[0, 1]`’.
The last element of the returned vector is guaranteed to be nonzero;
note that ‘`poly(0, x)`’ returns the empty vector ‘`[]`’.
Note also that ‘`x`’ may actually be any formula; for example,
‘`poly(sin(x)^2 - sin(x) + 3, sin(x))`’ returns ‘`[3, -1, 1]`’.

To get the ‘`x^k`’ coefficient of polynomial ‘`p`’, use
‘`poly(p, x)_(k+1)`’. To get the degree of polynomial ‘`p`’,
use ‘`vlen(poly(p, x)) - 1`’. For example, ‘`poly((x+1)^4, x)`’
returns ‘`[1, 4, 6, 4, 1]`’, so ‘`poly((x+1)^4, x)_(2+1)`’
gives the ‘`x^2`’ coefficient of this polynomial, 6.

One important feature of the solver is its ability to recognize
formulas which are “essentially” polynomials. This ability is
made available to the user through the `gpoly` function, which
is used just like `poly`: ‘`gpoly(expr, var)`’.
If expr is a polynomial in some term which includes var, then
this function will return a vector ‘`[x, c, a]`’
where x is the term that depends on var, c is a
vector of polynomial coefficients (like the one returned by `poly`),
and a is a multiplier which is usually 1. Basically,
‘`expr = a*(c_1 + c_2 x +
c_3 x^2 + ...)`’. The last element of c is
guaranteed to be non-zero, and c will not equal ‘`[1]`’
(i.e., the trivial decomposition expr = x is not
considered a polynomial). One side effect is that ‘`gpoly(x, x)`’
and ‘`gpoly(6, x)`’, both of which might be expected to recognize
their arguments as polynomials, will not because the decomposition
is considered trivial.

For example, ‘`gpoly((x-2)^2, x)`’ returns ‘`[x, [4, -4, 1], 1]`’,
since the expanded form of this polynomial is ‘`4 - 4 x + x^2`’.

The term x may itself be a polynomial in var. This is
done to reduce the size of the c vector. For example,
‘`gpoly(x^4 + x^2 - 1, x)`’ returns ‘`[x^2, [-1, 1, 1], 1]`’,
since a quadratic polynomial in ‘`x^2`’ is easier to solve than
a quartic polynomial in ‘`x`’.

A few more examples of the kinds of polynomials `gpoly` can
discover:

```
sin(x) - 1               [sin(x), [-1, 1], 1]
x + 1/x - 1              [x, [1, -1, 1], 1/x]
x + 1/x                  [x^2, [1, 1], 1/x]
x^3 + 2 x                [x^2, [2, 1], x]
x + x^2:3 + sqrt(x)      [x^1:6, [1, 1, 0, 1], x^1:2]
x^(2a) + 2 x^a + 5       [x^a, [5, 2, 1], 1]
(exp(-x) + exp(x)) / 2   [e^(2 x), [0.5, 0.5], e^-x]
```

The `poly` and `gpoly` functions accept a third integer argument
which specifies the largest degree of polynomial that is acceptable.
If this is ‘`n`’, then only c vectors of length ‘`n+1`’
or less will be returned. Otherwise, the `poly` or `gpoly`
call will remain in symbolic form. For example, the equation solver
can handle quartics and smaller polynomials, so it calls
‘`gpoly(expr, var, 4)`’ to discover whether expr
can be treated by its linear, quadratic, cubic, or quartic formulas.

The `pdeg` function computes the degree of a polynomial;
‘`pdeg(p,x)`’ is the highest power of `x` that appears in
`p`. This is the same as ‘`vlen(poly(p,x))-1`’, but is
much more efficient. If `p` is constant with respect to `x`,
then ‘`pdeg(p,x) = 0`’. If `p` is not a polynomial in `x`
(e.g., ‘`pdeg(2 cos(x), x)`’, the function remains unevaluated.
It is possible to omit the second argument `x`, in which case
‘`pdeg(p)`’ returns the highest total degree of any term of the
polynomial, counting all variables that appear in `p`. Note
that `pdeg(c) = pdeg(c,x) = 0` for any nonzero constant `c`;
the degree of the constant zero is considered to be `-inf`
(minus infinity).

The `plead` function finds the leading term of a polynomial.
Thus ‘`plead(p,x)`’ is equivalent to ‘`poly(p,x)_vlen(poly(p,x))`’,
though again more efficient. In particular, ‘`plead((2x+1)^10, x)`’
returns 1024 without expanding out the list of coefficients. The
value of `plead(p,x)` will be zero only if ‘`p = 0`’.

The `pcont` function finds the *content* of a polynomial. This
is the greatest common divisor of all the coefficients of the polynomial.
With two arguments, `pcont(p,x)` effectively uses ‘`poly(p,x)`’
to get a list of coefficients, then uses `pgcd` (the polynomial
GCD function) to combine these into an answer. For example,
‘`pcont(4 x y^2 + 6 x^2 y, x)`’ is ‘`2 y`’. The content is
basically the “biggest” polynomial that can be divided into `p`
exactly. The sign of the content is the same as the sign of the leading
coefficient.

With only one argument, ‘`pcont(p)`’ computes the numerical
content of the polynomial, i.e., the `gcd` of the numerical
coefficients of all the terms in the formula. Note that `gcd`
is defined on rational numbers as well as integers; it computes
the `gcd` of the numerators and the `lcm` of the
denominators. Thus ‘`pcont(4:3 x y^2 + 6 x^2 y)`’ returns 2:3.
Dividing the polynomial by this number will clear all the
denominators, as well as dividing by any common content in the
numerators. The numerical content of a polynomial is negative only
if all the coefficients in the polynomial are negative.

The `pprim` function finds the *primitive part* of a
polynomial, which is simply the polynomial divided (using `pdiv`
if necessary) by its content. If the input polynomial has rational
coefficients, the result will have integer coefficients in simplest
terms.

---

Next: [Curve Fitting](#Curve-Fitting), Previous: [Solving Equations](#Solving-Equations), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 10.7 Numerical Solutions [¶](#Numerical-Solutions-1)

Not all equations can be solved symbolically. The commands in this
section use numerical algorithms that can find a solution to a specific
instance of an equation to any desired accuracy. Note that the
numerical commands are slower than their algebraic cousins; it is a
good idea to try `a S` before resorting to these commands.

(See [Curve Fitting](#Curve-Fitting), for some other, more specialized, operations
on numerical data.)

* [Root Finding](#Root-Finding)
* [Minimization](#Minimization)
* [Systems of Equations](#Numerical-Systems-of-Equations)

---

Next: [Minimization](#Minimization), Up: [Numerical Solutions](#Numerical-Solutions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.7.1 Root Finding [¶](#Root-Finding-1)

The `a R` (`calc-find-root`) [`root`] command finds a
numerical solution (or *root*) of an equation. (This command treats
inequalities the same as equations. If the input is any other kind
of formula, it is interpreted as an equation of the form ‘`X = 0`’.)

The `a R` command requires an initial guess on the top of the
stack, and a formula in the second-to-top position. It prompts for a
solution variable, which must appear in the formula. All other variables
that appear in the formula must have assigned values, i.e., when
a value is assigned to the solution variable and the formula is
evaluated with `=`, it should evaluate to a number. Any assigned
value for the solution variable itself is ignored and unaffected by
this command.

When the command completes, the initial guess is replaced on the stack
by a vector of two numbers: The value of the solution variable that
solves the equation, and the difference between the lefthand and
righthand sides of the equation at that value. Ordinarily, the second
number will be zero or very nearly zero. (Note that Calc uses a
slightly higher precision while finding the root, and thus the second
number may be slightly different from the value you would compute from
the equation yourself.)

The `v h` (`calc-head`) command is a handy way to extract
the first element of the result vector, discarding the error term.

The initial guess can be a real number, in which case Calc searches
for a real solution near that number, or a complex number, in which
case Calc searches the whole complex plane near that number for a
solution, or it can be an interval form which restricts the search
to real numbers inside that interval.

Calc tries to use `a d` to take the derivative of the equation.
If this succeeds, it uses Newton’s method. If the equation is not
differentiable Calc uses a bisection method. (If Newton’s method
appears to be going astray, Calc switches over to bisection if it
can, or otherwise gives up. In this case it may help to try again
with a slightly different initial guess.) If the initial guess is a
complex number, the function must be differentiable.

If the formula (or the difference between the sides of an equation)
is negative at one end of the interval you specify and positive at
the other end, the root finder is guaranteed to find a root.
Otherwise, Calc subdivides the interval into small parts looking for
positive and negative values to bracket the root. When your guess is
an interval, Calc will not look outside that interval for a root.

The `H a R` [`wroot`] command is similar to `a R`, except
that if the initial guess is an interval for which the function has
the same sign at both ends, then rather than subdividing the interval
Calc attempts to widen it to enclose a root. Use this mode if
you are not sure if the function has a root in your interval.

If the function is not differentiable, and you give a simple number
instead of an interval as your initial guess, Calc uses this widening
process even if you did not type the Hyperbolic flag. (If the function
*is* differentiable, Calc uses Newton’s method which does not
require a bounding interval in order to work.)

If Calc leaves the `root` or `wroot` function in symbolic
form on the stack, it will normally display an explanation for why
no root was found. If you miss this explanation, press `w`
(`calc-why`) to get it back.

---

Next: [Systems of Equations](#Numerical-Systems-of-Equations), Previous: [Root Finding](#Root-Finding), Up: [Numerical Solutions](#Numerical-Solutions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.7.2 Minimization [¶](#Minimization-1)

The `a N` (`calc-find-minimum`) [`minimize`] command
finds a minimum value for a formula. It is very similar in operation
to `a R` (`calc-find-root`): You give the formula and an initial
guess on the stack, and are prompted for the name of a variable. The guess
may be either a number near the desired minimum, or an interval enclosing
the desired minimum. The function returns a vector containing the
value of the variable which minimizes the formula’s value, along
with the minimum value itself.

Note that this command looks for a *local* minimum. Many functions
have more than one minimum; some, like
‘`x sin(x)`’,
have infinitely many. In fact, there is no easy way to define the
“global” minimum of
‘`x sin(x)`’
but Calc can still locate any particular local minimum
for you. Calc basically goes downhill from the initial guess until it
finds a point at which the function’s value is greater both to the left
and to the right. Calc does not use derivatives when minimizing a function.

If your initial guess is an interval and it looks like the minimum
occurs at one or the other endpoint of the interval, Calc will return
that endpoint only if that endpoint is closed; thus, minimizing ‘`17 x`’
over ‘`[2..3]`’ will return ‘`[2, 38]`’, but minimizing over
‘`(2..3]`’ would report no minimum found. In general, you should
use closed intervals to find literally the minimum value in that
range of ‘`x`’, or open intervals to find the local minimum, if
any, that happens to lie in that range.

Most functions are smooth and flat near their minimum values. Because
of this flatness, if the current precision is, say, 12 digits, the
variable can only be determined meaningfully to about six digits. Thus
you should set the precision to twice as many digits as you need in your
answer.

The `H a N` [`wminimize`] command, analogously to `H a R`,
expands the guess interval to enclose a minimum rather than requiring
that the minimum lie inside the interval you supply.

The `a X` (`calc-find-maximum`) [`maximize`] and
`H a X` [`wmaximize`] commands effectively minimize the
negative of the formula you supply.

The formula must evaluate to a real number at all points inside the
interval (or near the initial guess if the guess is a number). If
the initial guess is a complex number the variable will be minimized
over the complex numbers; if it is real or an interval it will
be minimized over the reals.

---

Previous: [Minimization](#Minimization), Up: [Numerical Solutions](#Numerical-Solutions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.7.3 Systems of Equations [¶](#Systems-of-Equations)

The `a R` command can also solve systems of equations. In this
case, the equation should instead be a vector of equations, the
guess should instead be a vector of numbers (intervals are not
supported), and the variable should be a vector of variables. You
can omit the brackets while entering the list of variables. Each
equation must be differentiable by each variable for this mode to
work. The result will be a vector of two vectors: The variable
values that solved the system of equations, and the differences
between the sides of the equations with those variable values.
There must be the same number of equations as variables. Since
only plain numbers are allowed as guesses, the Hyperbolic flag has
no effect when solving a system of equations.

It is also possible to minimize over many variables with `a N`
(or maximize with `a X`). Once again the variable name should
be replaced by a vector of variables, and the initial guess should
be an equal-sized vector of initial guesses. But, unlike the case of
multidimensional `a R`, the formula being minimized should
still be a single formula, *not* a vector. Beware that
multidimensional minimization is currently *very* slow.

---

Next: [Summations](#Summations), Previous: [Numerical Solutions](#Numerical-Solutions), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 10.8 Curve Fitting [¶](#Curve-Fitting-1)

The `a F` command fits a set of data to a *model formula*,
such as ‘`y = m x + b`’ where ‘`m`’ and ‘`b`’ are parameters
to be determined. For a typical set of measured data there will be
no single ‘`m`’ and ‘`b`’ that exactly fit the data; in this
case, Calc chooses values of the parameters that provide the closest
possible fit. The model formula can be entered in various ways after
the key sequence `a F` is pressed.

If the letter `P` is pressed after `a F` but before the model
description is entered, the data as well as the model formula will be
plotted after the formula is determined. This will be indicated by a
“P” in the minibuffer after the help message.

* [Linear Fits](#Linear-Fits)
* [Polynomial and Multilinear Fits](#Polynomial-and-Multilinear-Fits)
* [Error Estimates for Fits](#Error-Estimates-for-Fits)
* [Standard Nonlinear Models](#Standard-Nonlinear-Models)
* [Curve Fitting Details](#Curve-Fitting-Details)
* [Polynomial Interpolation](#Interpolation)

---

Next: [Polynomial and Multilinear Fits](#Polynomial-and-Multilinear-Fits), Up: [Curve Fitting](#Curve-Fitting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.8.1 Linear Fits [¶](#Linear-Fits-1)

The `a F` (`calc-curve-fit`) [`fit`] command attempts
to fit a set of data (‘`x`’ and ‘`y`’ vectors of numbers) to a
straight line, polynomial, or other function of ‘`x`’. For the
moment we will consider only the case of fitting to a line, and we
will ignore the issue of whether or not the model was in fact a good
fit for the data.

In a standard linear least-squares fit, we have a set of ‘`(x,y)`’
data points that we wish to fit to the model ‘`y = m x + b`’
by adjusting the parameters ‘`m`’ and ‘`b`’ to make the ‘`y`’
values calculated from the formula be as close as possible to the actual
‘`y`’ values in the data set. (In a polynomial fit, the model is
instead, say, ‘`y = a x^3 + b x^2 + c x + d`’. In a multilinear fit,
we have data points of the form ‘`(x_1,x_2,x_3,y)`’ and our model is
‘`y = a x_1 + b x_2 + c x_3 + d`’. These will be discussed later.)

In the model formula, variables like ‘`x`’ and ‘`x_2`’ are called
the *independent variables*, and ‘`y`’ is the *dependent
variable*. Variables like ‘`m`’, ‘`a`’, and ‘`b`’ are called
the *parameters* of the model.

The `a F` command takes the data set to be fitted from the stack.
By default, it expects the data in the form of a matrix. For example,
for a linear or polynomial fit, this would be a
2xN
matrix where the first row is a list of ‘`x`’ values and the second
row has the corresponding ‘`y`’ values. For the multilinear fit
shown above, the matrix would have four rows (‘`x_1`’, ‘`x_2`’,
‘`x_3`’, and ‘`y`’, respectively).

If you happen to have an
Nx2
matrix instead of a
2xN
matrix, just press `v t` first to transpose the matrix.

After you type `a F`, Calc prompts you to select a model. For a
linear fit, press the digit `1`.

Calc then prompts for you to name the variables. By default it chooses
high letters like ‘`x`’ and ‘`y`’ for independent variables and
low letters like ‘`a`’ and ‘`b`’ for parameters. (The dependent
variable doesn’t need a name.) The two kinds of variables are separated
by a semicolon. Since you generally care more about the names of the
independent variables than of the parameters, Calc also allows you to
name only those and let the parameters use default names.

For example, suppose the data matrix

```
[ [ 1, 2, 3, 4,  5  ]
  [ 5, 7, 9, 11, 13 ] ]
```

is on the stack and we wish to do a simple linear fit. Type
`a F`, then `1` for the model, then `RET` to use
the default names. The result will be the formula ‘`3. + 2. x`’
on the stack. Calc has created the model expression `a + b x`,
then found the optimal values of ‘`a`’ and ‘`b`’ to fit the
data. (In this case, it was able to find an exact fit.) Calc then
substituted those values for ‘`a`’ and ‘`b`’ in the model
formula.

The `a F` command puts two entries in the trail. One is, as
always, a copy of the result that went to the stack; the other is
a vector of the actual parameter values, written as equations:
‘`[a = 3, b = 2]`’, in case you’d rather read them in a list
than pick them out of the formula. (You can type `t y`
to move this vector to the stack; see [Trail Commands](#Trail-Commands).

Specifying a different independent variable name will affect the
resulting formula: `a F 1 k RET` produces `3 + 2 k`.
Changing the parameter names (say, `a F 1 k;b,m RET`) will affect
the equations that go into the trail.

To see what happens when the fit is not exact, we could change
the number 13 in the data matrix to 14 and try the fit again.
The result is:

```
2.6 + 2.2 x
```

Evaluating this formula, say with `v x 5 RET TAB V M $ RET`, shows
a reasonably close match to the y-values in the data.

```
[4.8, 7., 9.2, 11.4, 13.6]
```

Since there is no line which passes through all the n data points,
Calc has chosen a line that best approximates the data points using
the method of least squares. The idea is to define the *chi-square*
error measure

```
chi^2 = sum((y_i - (a + b x_i))^2, i, 1, N)
```

which is clearly zero if ‘`a + b x`’ exactly fits all data points,
and increases as various ‘`a + b x_i`’ values fail to match the
corresponding ‘`y_i`’ values. There are several reasons why the
summand is squared, one of them being to ensure that
‘`chi^2 >= 0`’.
Least-squares fitting simply chooses the values of ‘`a`’ and ‘`b`’
for which the error
‘`chi^2`’
is as small as possible.

Other kinds of models do the same thing but with a different model
formula in place of ‘`a + b x_i`’.

A numeric prefix argument causes the `a F` command to take the
data in some other form than one big matrix. A positive argument n
will take N items from the stack, corresponding to the n rows
of a data matrix. In the linear case, n must be 2 since there
is always one independent variable and one dependent variable.

A prefix of zero or plain `C-u` is a compromise; Calc takes two
items from the stack, an n-row matrix of ‘`x`’ values, and a
vector of ‘`y`’ values. If there is only one independent variable,
the ‘`x`’ values can be either a one-row matrix or a plain vector,
in which case the `C-u` prefix is the same as a `C-u 2` prefix.

---

Next: [Error Estimates for Fits](#Error-Estimates-for-Fits), Previous: [Linear Fits](#Linear-Fits), Up: [Curve Fitting](#Curve-Fitting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.8.2 Polynomial and Multilinear Fits [¶](#Polynomial-and-Multilinear-Fits-1)

To fit the data to higher-order polynomials, just type one of the
digits `2` through `9` when prompted for a model. For example,
we could fit the original data matrix from the previous section
(with 13, not 14) to a parabola instead of a line by typing
`a F 2 RET`.

```
2.00000000001 x - 1.5e-12 x^2 + 2.99999999999
```

Note that since the constant and linear terms are enough to fit the
data exactly, it’s no surprise that Calc chose a tiny contribution
for ‘`x^2`’. (The fact that it’s not exactly zero is due only
to roundoff error. Since our data are exact integers, we could get
an exact answer by typing `m f` first to get Fraction mode.
Then the ‘`x^2`’ term would vanish altogether. Usually, though,
the data being fitted will be approximate floats so Fraction mode
won’t help.)

Doing the `a F 2` fit on the data set with 14 instead of 13
gives a much larger ‘`x^2`’ contribution, as Calc bends the
line slightly to improve the fit.

```
0.142857142855 x^2 + 1.34285714287 x + 3.59999999998
```

An important result from the theory of polynomial fitting is that it
is always possible to fit n data points exactly using a polynomial
of degree *n-1*, sometimes called an *interpolating polynomial*.
Using the modified (14) data matrix, a model number of 4 gives
a polynomial that exactly matches all five data points:

```
0.04167 x^4 - 0.4167 x^3 + 1.458 x^2 - 0.08333 x + 4.
```

The actual coefficients we get with a precision of 12, like
‘`0.0416666663588`’, clearly suffer from loss of precision.
It is a good idea to increase the working precision to several
digits beyond what you need when you do a fitting operation.
Or, if your data are exact, use Fraction mode to get exact
results.

You can type `i` instead of a digit at the model prompt to fit
the data exactly to a polynomial. This just counts the number of
columns of the data matrix to choose the degree of the polynomial
automatically.

Fitting data “exactly” to high-degree polynomials is not always
a good idea, though. High-degree polynomials have a tendency to
wiggle uncontrollably in between the fitting data points. Also,
if the exact-fit polynomial is going to be used to interpolate or
extrapolate the data, it is numerically better to use the `a p`
command described below. See [Polynomial Interpolation](#Interpolation).

Another generalization of the linear model is to assume the
‘`y`’ values are a sum of linear contributions from several
‘`x`’ values. This is a *multilinear* fit, and it is also
selected by the `1` digit key. (Calc decides whether the fit
is linear or multilinear by counting the rows in the data matrix.)

Given the data matrix,

```
[ [  1,   2,   3,    4,   5  ]
  [  7,   2,   3,    5,   2  ]
  [ 14.5, 15, 18.5, 22.5, 24 ] ]
```

the command `a F 1 RET` will call the first row ‘`x`’ and the
second row ‘`y`’, and will fit the values in the third row to the
model ‘`a + b x + c y`’.

```
8. + 3. x + 0.5 y
```

Calc can do multilinear fits with any number of independent variables
(i.e., with any number of data rows).

Yet another variation is *homogeneous* linear models, in which
the constant term is known to be zero. In the linear case, this
means the model formula is simply ‘`a x`’; in the multilinear
case, the model might be ‘`a x + b y + c z`’; and in the polynomial
case, the model could be ‘`a x + b x^2 + c x^3`’. You can get
a homogeneous linear or multilinear model by pressing the letter
`h` followed by a regular model key, like `1` or `2`.
This will be indicated by an “h” in the minibuffer after the help
message.

It is certainly possible to have other constrained linear models,
like ‘`2.3 + a x`’ or ‘`a - 4 x`’. While there is no single
key to select models like these, a later section shows how to enter
any desired model by hand. In the first case, for example, you
would enter `a F ' 2.3 + a x`.

Another class of models that will work but must be entered by hand
are multinomial fits, e.g., ‘`a + b x + c y + d x^2 + e y^2 + f x y`’.

---

Next: [Standard Nonlinear Models](#Standard-Nonlinear-Models), Previous: [Polynomial and Multilinear Fits](#Polynomial-and-Multilinear-Fits), Up: [Curve Fitting](#Curve-Fitting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.8.3 Error Estimates for Fits [¶](#Error-Estimates-for-Fits-1)

With the Hyperbolic flag, `H a F` [`efit`] performs the same
fitting operation as `a F`, but reports the coefficients as error
forms instead of plain numbers. Fitting our two data matrices (first
with 13, then with 14) to a line with `H a F` gives the results,

```
3. + 2. x
2.6 +/- 0.382970843103 + 2.2 +/- 0.115470053838 x
```

In the first case the estimated errors are zero because the linear
fit is perfect. In the second case, the errors are nonzero but
moderately small, because the data are still very close to linear.

It is also possible for the *input* to a fitting operation to
contain error forms. The data values must either all include errors
or all be plain numbers. Error forms can go anywhere but generally
go on the numbers in the last row of the data matrix. If the last
row contains error forms
‘y\_i `+/-` sigma\_i’,
then the
‘`chi^2`’
statistic is now,

```
chi^2 = sum(((y_i - (a + b x_i)) / sigma_i)^2, i, 1, N)
```

so that data points with larger error estimates contribute less to
the fitting operation.

If there are error forms on other rows of the data matrix, all the
errors for a given data point are combined; the square root of the
sum of the squares of the errors forms the
‘`sigma_i`’
used for the data point.

Both `a F` and `H a F` can accept error forms in the input
matrix, although if you are concerned about error analysis you will
probably use `H a F` so that the output also contains error
estimates.

If the input contains error forms but all the
‘`sigma_i`’
values are the same, it is easy to see that the resulting fitted model
will be the same as if the input did not have error forms at all
(‘`chi^2`’
is simply scaled uniformly by
‘`1 / sigma^2`’,
which doesn’t affect where it has a minimum). But there *will* be
a difference in the estimated errors of the coefficients reported by
`H a F`.

Consult any text on statistical modeling of data for a discussion
of where these error estimates come from and how they should be
interpreted.

With the Inverse flag, `I a F` [`xfit`] produces even more
information. The result is a vector of six items:

1. The model formula with error forms for its coefficients or
   parameters. This is the result that `H a F` would have
   produced.
2. A vector of “raw” parameter values for the model. These are the
   polynomial coefficients or other parameters as plain numbers, in the
   same order as the parameters appeared in the final prompt of the
   `I a F` command. For polynomials of degree ‘`d`’, this vector
   will have length ‘`M = d+1`’ with the constant term first.
3. The covariance matrix ‘`C`’ computed from the fit. This is
   an mxm symmetric matrix; the diagonal elements
   ‘`C_j_j`’
   are the variances
   ‘`sigma_j^2`’
   of the parameters. The other elements are covariances
   ‘`sigma_i_j^2`’
   that describe the correlation between pairs of parameters. (A related
   set of numbers, the *linear correlation coefficients*
   ‘`r_i_j`’,
   are defined as
   ‘`sigma_i_j^2 / sigma_i sigma_j`’.)
4. A vector of ‘`M`’ “parameter filter” functions whose
   meanings are described below. If no filters are necessary this
   will instead be an empty vector; this is always the case for the
   polynomial and multilinear fits described so far.
5. The value of
   ‘`chi^2`’
   for the fit, calculated by the formulas shown above. This gives a
   measure of the quality of the fit; statisticians consider
   ‘`chi^2 = N - M`’
   to indicate a moderately good fit (where again ‘`N`’ is the number of
   data points and ‘`M`’ is the number of parameters).
6. A measure of goodness of fit expressed as a probability ‘`Q`’.
   This is computed from the `utpc` probability distribution
   function using
   ‘`chi^2`’
   with ‘`N - M`’ degrees of freedom. A
   value of 0.5 implies a good fit; some texts recommend that often
   ‘`Q = 0.1`’ or even 0.001 can signify an acceptable fit. In
   particular,
   ‘`chi^2`’
   statistics assume the errors in your inputs
   follow a normal (Gaussian) distribution; if they don’t, you may
   have to accept smaller values of ‘`Q`’.

   The ‘`Q`’ value is computed only if the input included error
   estimates. Otherwise, Calc will report the symbol `nan`
   for ‘`Q`’. The reason is that in this case the
   ‘`chi^2`’
   value has effectively been used to estimate the original errors
   in the input, and thus there is no redundant information left
   over to use for a confidence test.

---

Next: [Curve Fitting Details](#Curve-Fitting-Details), Previous: [Error Estimates for Fits](#Error-Estimates-for-Fits), Up: [Curve Fitting](#Curve-Fitting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.8.4 Standard Nonlinear Models [¶](#Standard-Nonlinear-Models-1)

The `a F` command also accepts other kinds of models besides
lines and polynomials. Some common models have quick single-key
abbreviations; others must be entered by hand as algebraic formulas.

Here is a complete list of the standard models recognized by `a F`:

`1`
:   Linear or multilinear. *a + b x + c y + d z*.

`2-9`
:   Polynomials. *a + b x + c x^2 + d x^3*.

`e`
:   Exponential. *a* `exp`*(b x)* `exp`*(c y)*.

`E`
:   Base-10 exponential. *a* `10^`*(b x)* `10^`*(c y)*.

`x`
:   Exponential (alternate notation). `exp`*(a + b x + c y)*.

`X`
:   Base-10 exponential (alternate). `10^`*(a + b x + c y)*.

`l`
:   Logarithmic. *a + b* `ln`*(x) + c* `ln`*(y)*.

`L`
:   Base-10 logarithmic. *a + b* `log10`*(x) + c* `log10`*(y)*.

`^`
:   General exponential. *a b^x c^y*.

`p`
:   Power law. *a x^b y^c*.

`q`
:   Quadratic. *a + b (x-c)^2 + d (x-e)^2*.

`g`
:   Gaussian.
    *(a / b sqrt(2 pi)) exp(-0.5\*((x-c)/b)^2)*.

`s`
:   Logistic *s* curve.
    *a/(1 + exp(b (x - c)))*.

`b`
:   Logistic bell curve.
    *a exp(b (x - c))/(1 + exp(b (x - c)))^2*.

`o`
:   Hubbert linearization.
    *(y/x) = a (1 - x/b)*.

All of these models are used in the usual way; just press the appropriate
letter at the model prompt, and choose variable names if you wish. The
result will be a formula as shown in the above table, with the best-fit
values of the parameters substituted. (You may find it easier to read
the parameter values from the vector that is placed in the trail.)

All models except Gaussian, logistics, Hubbert and polynomials can
generalize as shown to any number of independent variables. Also, all
the built-in models except for the logistic and Hubbert curves have an
additive or multiplicative parameter shown as ‘`a`’ in the above table
which can be replaced by zero or one, as appropriate, by typing `h`
before the model key.

Note that many of these models are essentially equivalent, but express
the parameters slightly differently. For example, ‘`a b^x`’ and
the other two exponential models are all algebraic rearrangements of
each other. Also, the “quadratic” model is just a degree-2 polynomial
with the parameters expressed differently. Use whichever form best
matches the problem.

The HP-28/48 calculators support four different models for curve
fitting, called `LIN`, `LOG`, `EXP`, and `PWR`.
These correspond to Calc models ‘`a + b x`’, ‘`a + b ln(x)`’,
‘`a exp(b x)`’, and ‘`a x^b`’, respectively. In each case,
‘`a`’ is what the HP-48 identifies as the “intercept,” and
‘`b`’ is what it calls the “slope.”

If the model you want doesn’t appear on this list, press `'`
(the apostrophe key) at the model prompt to enter any algebraic
formula, such as `m x - b`, as the model. (Not all models
will work, though—see the next section for details.)

The model can also be an equation like ‘`y = m x + b`’.
In this case, Calc thinks of all the rows of the data matrix on
equal terms; this model effectively has two parameters
(‘`m`’ and ‘`b`’) and two independent variables (‘`x`’
and ‘`y`’), with no “dependent” variables. Model equations
do not need to take this ‘`y =`’ form. For example, the
implicit line equation ‘`a x + b y = 1`’ works fine as a
model.

When you enter a model, Calc makes an alphabetical list of all
the variables that appear in the model. These are used for the
default parameters, independent variables, and dependent variable
(in that order). If you enter a plain formula (not an equation),
Calc assumes the dependent variable does not appear in the formula
and thus does not need a name.

For example, if the model formula has the variables ‘`a,mu,sigma,t,x`’,
and the data matrix has three rows (meaning two independent variables),
Calc will use ‘`a,mu,sigma`’ as the default parameters, and the
data rows will be named ‘`t`’ and ‘`x`’, respectively. If you
enter an equation instead of a plain formula, Calc will use ‘`a,mu`’
as the parameters, and ‘`sigma,t,x`’ as the three independent
variables.

You can, of course, override these choices by entering something
different at the prompt. If you leave some variables out of the list,
those variables must have stored values and those stored values will
be used as constants in the model. (Stored values for the parameters
and independent variables are ignored by the `a F` command.)
If you list only independent variables, all the remaining variables
in the model formula will become parameters.

If there are `$` signs in the model you type, they will stand
for parameters and all other variables (in alphabetical order)
will be independent. Use `$` for one parameter, `$$` for
another, and so on. Thus `$ x + $$` is another way to describe
a linear model.

If you type a `$` instead of `'` at the model prompt itself,
Calc will take the model formula from the stack. (The data must then
appear at the second stack level.) The same conventions are used to
choose which variables in the formula are independent by default and
which are parameters.

Models taken from the stack can also be expressed as vectors of
two or three elements, ‘`[model, vars]`’ or
‘`[model, vars, params]`’. Each of vars
and params may be either a variable or a vector of variables.
(If params is omitted, all variables in model except
those listed as vars are parameters.)

When you enter a model manually with `'`, Calc puts a 3-vector
describing the model in the trail so you can get it back if you wish.

Finally, you can store a model in one of the Calc variables
`Model1` or `Model2`, then use this model by typing
`a F u` or `a F U` (respectively). The value stored in
the variable can be any of the formats that `a F $` would
accept for a model on the stack.

Calc uses the principal values of inverse functions like `ln`
and `arcsin` when doing fits. For example, when you enter
the model ‘`y = sin(a t + b)`’ Calc actually uses the easier
form ‘`arcsin(y) = a t + b`’. The `arcsin` function always
returns results in the range from *-90* to 90 degrees (or the
equivalent range in radians). Suppose you had data that you
believed to represent roughly three oscillations of a sine wave,
so that the argument of the sine might go from zero to
*3\*360*
degrees.
The above model would appear to be a good way to determine the
true frequency and phase of the sine wave, but in practice it
would fail utterly. The righthand side of the actual model
‘`arcsin(y) = a t + b`’ will grow smoothly with ‘`t`’, but
the lefthand side will bounce back and forth between *-90* and 90.
No values of ‘`a`’ and ‘`b`’ can make the two sides match,
even approximately.

There is no good solution to this problem at present. You could
restrict your data to small enough ranges so that the above problem
doesn’t occur (i.e., not straddling any peaks in the sine wave).
Or, in this case, you could use a totally different method such as
Fourier analysis, which is beyond the scope of the `a F` command.
(Unfortunately, Calc does not currently have any facilities for
taking Fourier and related transforms.)

---

Next: [Polynomial Interpolation](#Interpolation), Previous: [Standard Nonlinear Models](#Standard-Nonlinear-Models), Up: [Curve Fitting](#Curve-Fitting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.8.5 Curve Fitting Details [¶](#Curve-Fitting-Details-1)

Calc’s internal least-squares fitter can only handle multilinear
models. More precisely, it can handle any model of the form
‘`a f(x,y,z) + b g(x,y,z) + c h(x,y,z)`’, where ‘`a,b,c`’
are the parameters and ‘`x,y,z`’ are the independent variables
(of course there can be any number of each, not just three).

In a simple multilinear or polynomial fit, it is easy to see how
to convert the model into this form. For example, if the model
is ‘`a + b x + c x^2`’, then ‘`f(x) = 1`’, ‘`g(x) = x`’,
and ‘`h(x) = x^2`’ are suitable functions.

For most other models, Calc uses a variety of algebraic manipulations
to try to put the problem into the form

```
Y(x,y,z) = A(a,b,c) F(x,y,z) + B(a,b,c) G(x,y,z) + C(a,b,c) H(x,y,z)
```

where ‘`Y,A,B,C,F,G,H`’ are arbitrary functions. It computes
‘`Y`’, ‘`F`’, ‘`G`’, and ‘`H`’ for all the data points,
does a standard linear fit to find the values of ‘`A`’, ‘`B`’,
and ‘`C`’, then uses the equation solver to solve for ‘`a,b,c`’
in terms of ‘`A,B,C`’.

A remarkable number of models can be cast into this general form.
We’ll look at two examples here to see how it works. The power-law
model ‘`y = a x^b`’ with two independent variables and two parameters
can be rewritten as follows:

```
y = a x^b
y = a exp(b ln(x))
y = exp(ln(a) + b ln(x))
ln(y) = ln(a) + b ln(x)
```

which matches the desired form with
‘`Y = ln(y)`’,
‘`A = ln(a)`’,
‘`F = 1`’, ‘`B = b`’, and
‘`G = ln(x)`’.
Calc thus computes the logarithms of your ‘`y`’ and ‘`x`’ values,
does a linear fit for ‘`A`’ and ‘`B`’, then solves to get
‘`a = exp(A)`’
and ‘`b = B`’.

Another interesting example is the “quadratic” model, which can
be handled by expanding according to the distributive law.

```
y = a + b*(x - c)^2
y = a + b c^2 - 2 b c x + b x^2
```

which matches with ‘`Y = y`’, ‘`A = a + b c^2`’, ‘`F = 1`’,
‘`B = -2 b c`’, ‘`G = x`’ (the *-2* factor could just as easily
have been put into ‘`G`’ instead of ‘`B`’), ‘`C = b`’, and
‘`H = x^2`’.

The Gaussian model looks quite complicated, but a closer examination
shows that it’s actually similar to the quadratic model but with an
exponential that can be brought to the top and moved into ‘`Y`’.

The logistic models cannot be put into general linear form. For these
models, and the Hubbert linearization, Calc computes a rough
approximation for the parameters, then uses the Levenberg-Marquardt
iterative method to refine the approximations.

Another model that cannot be put into general linear
form is a Gaussian with a constant background added on, i.e.,
‘`d`’ + the regular Gaussian formula. If you have a model like
this, your best bet is to replace enough of your parameters with
constants to make the model linearizable, then adjust the constants
manually by doing a series of fits. You can compare the fits by
graphing them, by examining the goodness-of-fit measures returned by
`I a F`, or by some other method suitable to your application.
Note that some models can be linearized in several ways. The
Gaussian-plus-d model can be linearized by setting ‘`d`’
(the background) to a constant, or by setting ‘`b`’ (the standard
deviation) and ‘`c`’ (the mean) to constants.

To fit a model with constants substituted for some parameters, just
store suitable values in those parameter variables, then omit them
from the list of parameters when you answer the variables prompt.

A last desperate step would be to use the general-purpose
`minimize` function rather than `fit`. After all, both
functions solve the problem of minimizing an expression (the
‘`chi^2`’
sum) by adjusting certain parameters in the expression. The `a F`
command is able to use a vastly more efficient algorithm due to its
special knowledge about linear chi-square sums, but the `a N`
command can do the same thing by brute force.

A compromise would be to pick out a few parameters without which the
fit is linearizable, and use `minimize` on a call to `fit`
which efficiently takes care of the rest of the parameters. The thing
to be minimized would be the value of
‘`chi^2`’
returned as the fifth result of the `xfit` function:

```
minimize(xfit(gaus(a,b,c,d,x), x, [a,b,c], data)_5, d, guess)
```

where `gaus` represents the Gaussian model with background,
`data` represents the data matrix, and `guess` represents
the initial guess for ‘`d`’ that `minimize` requires.
This operation will only be, shall we say, extraordinarily slow
rather than astronomically slow (as would be the case if `minimize`
were used by itself to solve the problem).

The `I a F` [`xfit`] command is somewhat trickier when
nonlinear models are used. The second item in the result is the
vector of “raw” parameters ‘`A`’, ‘`B`’, ‘`C`’. The
covariance matrix is written in terms of those raw parameters.
The fifth item is a vector of *filter* expressions. This
is the empty vector ‘`[]`’ if the raw parameters were the same
as the requested parameters, i.e., if ‘`A = a`’, ‘`B = b`’,
and so on (which is always true if the model is already linear
in the parameters as written, e.g., for polynomial fits). If the
parameters had to be rearranged, the fifth item is instead a vector
of one formula per parameter in the original model. The raw
parameters are expressed in these “filter” formulas as
‘`fitdummy(1)`’ for ‘`A`’, ‘`fitdummy(2)`’ for ‘`B`’,
and so on.

When Calc needs to modify the model to return the result, it replaces
‘`fitdummy(1)`’ in all the filters with the first item in the raw
parameters list, and so on for the other raw parameters, then
evaluates the resulting filter formulas to get the actual parameter
values to be substituted into the original model. In the case of
`H a F` and `I a F` where the parameters must be error forms,
Calc uses the square roots of the diagonal entries of the covariance
matrix as error values for the raw parameters, then lets Calc’s
standard error-form arithmetic take it from there.

If you use `I a F` with a nonlinear model, be sure to remember
that the covariance matrix is in terms of the raw parameters,
*not* the actual requested parameters. It’s up to you to
figure out how to interpret the covariances in the presence of
nontrivial filter functions.

Things are also complicated when the input contains error forms.
Suppose there are three independent and dependent variables, ‘`x`’,
‘`y`’, and ‘`z`’, one or more of which are error forms in the
data. Calc combines all the error values by taking the square root
of the sum of the squares of the errors. It then changes ‘`x`’
and ‘`y`’ to be plain numbers, and makes ‘`z`’ into an error
form with this combined error. The ‘`Y(x,y,z)`’ part of the
linearized model is evaluated, and the result should be an error
form. The error part of that result is used for
‘`sigma_i`’
for the data point. If for some reason ‘`Y(x,y,z)`’ does not return
an error form, the combined error from ‘`z`’ is used directly for
‘`sigma_i`’.
Finally, ‘`z`’ is also stripped of its error
for use in computing ‘`F(x,y,z)`’, ‘`G(x,y,z)`’ and so on;
the righthand side of the linearized model is computed in regular
arithmetic with no error forms.

(While these rules may seem complicated, they are designed to do
the most reasonable thing in the typical case that ‘`Y(x,y,z)`’
depends only on the dependent variable ‘`z`’, and in fact is
often simply equal to ‘`z`’. For common cases like polynomials
and multilinear models, the combined error is simply used as the
‘`sigma`’
for the data point with no further ado.)

It may be the case that the model you wish to use is linearizable,
but Calc’s built-in rules are unable to figure it out. Calc uses
its algebraic rewrite mechanism to linearize a model. The rewrite
rules are kept in the variable `FitRules`. You can edit this
variable using the `s e FitRules` command; in fact, there is
a special `s F` command just for editing `FitRules`.
See [Other Operations on Variables](#Operations-on-Variables).

See [Rewrite Rules](#Rewrite-Rules), for a discussion of rewrite rules.

Calc uses `FitRules` as follows. First, it converts the model
to an equation if necessary and encloses the model equation in a
call to the function `fitmodel` (which is not actually a defined
function in Calc; it is only used as a placeholder by the rewrite rules).
Parameter variables are renamed to function calls ‘`fitparam(1)`’,
‘`fitparam(2)`’, and so on, and independent variables are renamed
to ‘`fitvar(1)`’, ‘`fitvar(2)`’, etc. The dependent variable
is the highest-numbered `fitvar`. For example, the power law
model ‘`a x^b`’ is converted to ‘`y = a x^b`’, then to

```
fitmodel(fitvar(2) = fitparam(1) fitvar(1)^fitparam(2))
```

Calc then applies the rewrites as if by ‘`C-u 0 a r FitRules`’.
(The zero prefix means that rewriting should continue until no further
changes are possible.)

When rewriting is complete, the `fitmodel` call should have
been replaced by a `fitsystem` call that looks like this:

```
fitsystem(Y, FGH, abc)
```

where Y is a formula that describes the function ‘`Y(x,y,z)`’,
FGH is the vector of formulas ‘`[F(x,y,z), G(x,y,z), H(x,y,z)]`’,
and abc is the vector of parameter filters which refer to the
raw parameters as ‘`fitdummy(1)`’ for ‘`A`’, ‘`fitdummy(2)`’
for ‘`B`’, etc. While the number of raw parameters (the length of
the FGH vector) is usually the same as the number of original
parameters (the length of the abc vector), this is not required.

The power law model eventually boils down to

```
fitsystem(ln(fitvar(2)),
          [1, ln(fitvar(1))],
          [exp(fitdummy(1)), fitdummy(2)])
```

The actual implementation of `FitRules` is complicated; it
proceeds in four phases. First, common rearrangements are done
to try to bring linear terms together and to isolate functions like
`exp` and `ln` either all the way “out” (so that they
can be put into Y) or all the way “in” (so that they can
be put into abc or FGH). In particular, all
non-constant powers are converted to logs-and-exponentials form,
and the distributive law is used to expand products of sums.
Quotients are rewritten to use the ‘`fitinv`’ function, where
‘`fitinv(x)`’ represents ‘`1/x`’ while the `FitRules`
are operating. (The use of `fitinv` makes recognition of
linear-looking forms easier.) If you modify `FitRules`, you
will probably only need to modify the rules for this phase.

Phase two, whose rules can actually also apply during phases one
and three, first rewrites `fitmodel` to a two-argument
form ‘`fitmodel(Y, model)`’, where Y is
initially zero and model has been changed from ‘`a=b`’
to ‘`a-b`’ form. It then tries to peel off invertible functions
from the outside of model and put them into Y instead,
calling the equation solver to invert the functions. Finally, when
this is no longer possible, the `fitmodel` is changed to a
four-argument `fitsystem`, where the fourth argument is
model and the FGH and abc vectors are initially
empty. (The last vector is really ABC, corresponding to
raw parameters, for now.)

Phase three converts a sum of items in the model to a sum
of ‘`fitpart(a, b, c)`’ terms which represent
terms ‘`a*b*c`’ of the sum, where a
is all factors that do not involve any variables, b is all
factors that involve only parameters, and c is the factors
that involve only independent variables. (If this decomposition
is not possible, the rule set will not complete and Calc will
complain that the model is too complex.) Then `fitpart`s
with equal b or c components are merged back together
using the distributive law in order to minimize the number of
raw parameters needed.

Phase four moves the `fitpart` terms into the FGH and
ABC vectors. Also, some of the algebraic expansions that
were done in phase 1 are undone now to make the formulas more
computationally efficient. Finally, it calls the solver one more
time to convert the ABC vector to an abc vector, and
removes the fourth model argument (which by now will be zero)
to obtain the three-argument `fitsystem` that the linear
least-squares solver wants to see.

Two functions which are useful in connection with `FitRules`
are ‘`hasfitparams(x)`’ and ‘`hasfitvars(x)`’, which check
whether ‘`x`’ refers to any parameters or independent variables,
respectively. Specifically, these functions return “true” if the
argument contains any `fitparam` (or `fitvar`) function
calls, and “false” otherwise. (Recall that “true” means a
nonzero number, and “false” means zero. The actual nonzero number
returned is the largest n from all the ‘`fitparam(n)`’s
or ‘`fitvar(n)`’s, respectively, that appear in the formula.)

The `fit` function in algebraic notation normally takes four
arguments, ‘`fit(model, vars, params, data)`’,
where model is the model formula as it would be typed after
`a F '`, vars is the independent variable or a vector of
independent variables, params likewise gives the parameter(s),
and data is the data matrix. Note that the length of vars
must be equal to the number of rows in data if model is
an equation, or one less than the number of rows if model is
a plain formula. (Actually, a name for the dependent variable is
allowed but will be ignored in the plain-formula case.)

If params is omitted, the parameters are all variables in
model except those that appear in vars. If vars
is also omitted, Calc sorts all the variables that appear in
model alphabetically and uses the higher ones for vars
and the lower ones for params.

Alternatively, ‘`fit(modelvec, data)`’ is allowed
where modelvec is a 2- or 3-vector describing the model
and variables, as discussed previously.

If Calc is unable to do the fit, the `fit` function is left
in symbolic form, ordinarily with an explanatory message. The
message will be “Model expression is too complex” if the
linearizer was unable to put the model into the required form.

The `efit` (corresponding to `H a F`) and `xfit`
(for `I a F`) functions are completely analogous.

---

Previous: [Curve Fitting Details](#Curve-Fitting-Details), Up: [Curve Fitting](#Curve-Fitting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.8.6 Polynomial Interpolation [¶](#Polynomial-Interpolation)

The `a p` (`calc-poly-interp`) [`polint`] command does
a polynomial interpolation at a particular ‘`x`’ value. It takes
two arguments from the stack: A data matrix of the sort used by
`a F`, and a single number which represents the desired ‘`x`’
value. Calc effectively does an exact polynomial fit as if by `a F i`,
then substitutes the ‘`x`’ value into the result in order to get an
approximate ‘`y`’ value based on the fit. (Calc does not actually
use `a F i`, however; it uses a direct method which is both more
efficient and more numerically stable.)

The result of `a p` is actually a vector of two values: The ‘`y`’
value approximation, and an error measure ‘`dy`’ that reflects Calc’s
estimation of the probable error of the approximation at that value of
‘`x`’. If the input ‘`x`’ is equal to any of the ‘`x`’ values
in the data matrix, the output ‘`y`’ will be the corresponding ‘`y`’
value from the matrix, and the output ‘`dy`’ will be exactly zero.

A prefix argument of 2 causes `a p` to take separate x- and
y-vectors from the stack instead of one data matrix.

If ‘`x`’ is a vector of numbers, `a p` will return a matrix of
interpolated results for each of those ‘`x`’ values. (The matrix will
have two columns, the ‘`y`’ values and the ‘`dy`’ values.)
If ‘`x`’ is a formula instead of a number, the `polint` function
remains in symbolic form; use the `a "` command to expand it out to
a formula that describes the fit in symbolic terms.

In all cases, the `a p` command leaves the data vectors or matrix
on the stack. Only the ‘`x`’ value is replaced by the result.

The `H a p` [`ratint`] command does a rational function
interpolation. It is used exactly like `a p`, except that it
uses as its model the quotient of two polynomials. If there are
‘`N`’ data points, the numerator and denominator polynomials will
each have degree ‘`N/2`’ (if ‘`N`’ is odd, the denominator will
have degree one higher than the numerator).

Rational approximations have the advantage that they can accurately
describe functions that have poles (points at which the function’s value
goes to infinity, so that the denominator polynomial of the approximation
goes to zero). If ‘`x`’ corresponds to a pole of the fitted rational
function, then the result will be a division by zero. If Infinite mode
is enabled, the result will be ‘`[uinf, uinf]`’.

There is no way to get the actual coefficients of the rational function
used by `H a p`. (The algorithm never generates these coefficients
explicitly, and quotients of polynomials are beyond `a F`’s
capabilities to fit.)

---

Next: [Logical Operations](#Logical-Operations), Previous: [Curve Fitting](#Curve-Fitting), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 10.9 Summations [¶](#Summations-1)

The `a +` (`calc-summation`) [`sum`] command computes
the sum of a formula over a certain range of index values. The formula
is taken from the top of the stack; the command prompts for the
name of the summation index variable, the lower limit of the
sum (any formula), and the upper limit of the sum. If you
enter a blank line at any of these prompts, that prompt and
any later ones are answered by reading additional elements from
the stack. Thus, `' k^2 RET ' k RET 1 RET 5 RET a + RET`
produces the result 55.

The choice of index variable is arbitrary, but it’s best not to
use a variable with a stored value. In particular, while
`i` is often a favorite index variable, it should be avoided
in Calc because `i` has the imaginary constant ‘`(0, 1)`’
as a value. If you pressed `=` on a sum over `i`, it would
be changed to a nonsensical sum over the “variable” ‘`(0, 1)`’!
If you really want to use `i` as an index variable, use
`s u i RET` first to “unstore” this variable.
(See [Storing Variables](#Storing-Variables).)

A numeric prefix argument steps the index by that amount rather
than by one. Thus `' a_k RET C-u -2 a + k RET 10 RET 0 RET`
yields ‘`a_10 + a_8 + a_6 + a_4 + a_2 + a_0`’. A prefix
argument of plain `C-u` causes `a +` to prompt for the
step value, in which case you can enter any formula or enter
a blank line to take the step value from the stack. With the
`C-u` prefix, `a +` can take up to five arguments from
the stack: The formula, the variable, the lower limit, the
upper limit, and (at the top of the stack), the step value.

Calc knows how to do certain sums in closed form. For example,
‘`sum(6 k^2, k, 1, n) = 2 n^3 + 3 n^2 + n`’. In particular,
this is possible if the formula being summed is polynomial or
exponential in the index variable. Sums of logarithms are
transformed into logarithms of products. Sums of trigonometric
and hyperbolic functions are transformed to sums of exponentials
and then done in closed form. Also, of course, sums in which the
lower and upper limits are both numbers can always be evaluated
just by grinding them out, although Calc will use closed forms
whenever it can for the sake of efficiency.

The notation for sums in algebraic formulas is
‘`sum(expr, var, low, high, step)`’.
If step is omitted, it defaults to one. If high is
omitted, low is actually the upper limit and the lower limit
is one. If low is also omitted, the limits are ‘`-inf`’
and ‘`inf`’, respectively.

Infinite sums can sometimes be evaluated: ‘`sum(.5^k, k, 1, inf)`’
returns ‘`1`’. This is done by evaluating the sum in closed
form (to ‘`1. - 0.5^n`’ in this case), then evaluating this
formula with `n` set to `inf`. Calc’s usual rules
for “infinite” arithmetic can find the answer from there. If
infinite arithmetic yields a ‘`nan`’, or if the sum cannot be
solved in closed form, Calc leaves the `sum` function in
symbolic form. See [Infinities](#Infinities).

As a special feature, if the limits are infinite (or omitted, as
described above) but the formula includes vectors subscripted by
expressions that involve the iteration variable, Calc narrows
the limits to include only the range of integers which result in
valid subscripts for the vector. For example, the sum
‘`sum(k [a,b,c,d,e,f,g]_(2k),k)`’ evaluates to ‘`b + 2 d + 3 f`’.

The limits of a sum do not need to be integers. For example,
‘`sum(a_k, k, 0, 2 n, n)`’ produces ‘`a_0 + a_n + a_(2 n)`’.
Calc computes the number of iterations using the formula
‘`1 + (high - low) / step`’, which must,
after algebraic simplification, evaluate to an integer.

If the number of iterations according to the above formula does
not come out to an integer, the sum is invalid and will be left
in symbolic form. However, closed forms are still supplied, and
you are on your honor not to misuse the resulting formulas by
substituting mismatched bounds into them. For example,
‘`sum(k, k, 1, 10, 2)`’ is invalid, but Calc will go ahead and
evaluate the closed form solution for the limits 1 and 10 to get
the rather dubious answer, 29.25.

If the lower limit is greater than the upper limit (assuming a
positive step size), the result is generally zero. However,
Calc only guarantees a zero result when the upper limit is
exactly one step less than the lower limit, i.e., if the number
of iterations is *-1*. Thus ‘`sum(f(k), k, n, n-1)`’ is zero
but the sum from ‘`n`’ to ‘`n-2`’ may report a nonzero value
if Calc used a closed form solution.

Calc’s logical predicates like ‘`a < b`’ return 1 for “true”
and 0 for “false.” See [Logical Operations](#Logical-Operations). This can be
used to advantage for building conditional sums. For example,
‘`sum(prime(k)*k^2, k, 1, 20)`’ is the sum of the squares of all
prime numbers from 1 to 20; the `prime` predicate returns 1 if
its argument is prime and 0 otherwise. You can read this expression
as “the sum of ‘`k^2`’, where ‘`k`’ is prime.” Indeed,
‘`sum(prime(k)*k^2, k)`’ would represent the sum of *all* primes
squared, since the limits default to plus and minus infinity, but
there are no such sums that Calc’s built-in rules can do in
closed form.

As another example, ‘`sum((k != k_0) * f(k), k, 1, n)`’ is the
sum of ‘`f(k)`’ for all ‘`k`’ from 1 to ‘`n`’, excluding
one value ‘`k_0`’. Slightly more tricky is the summand
‘`(k != k_0) / (k - k_0)`’, which is an attempt to describe
the sum of all ‘`1/(k-k_0)`’ except at ‘`k = k_0`’, where
this would be a division by zero. But at ‘`k = k_0`’, this
formula works out to the indeterminate form ‘`0 / 0`’, which
Calc will not assume is zero. Better would be to use
‘`(k != k_0) ? 1/(k-k_0) : 0`’; the ‘`? :`’ operator does
an “if-then-else” test: This expression says, “if
‘`k != k_0`’,
then ‘`1/(k-k_0)`’, else zero.” Now the formula ‘`1/(k-k_0)`’
will not even be evaluated by Calc when ‘`k = k_0`’.

The `a -` (`calc-alt-summation`) [`asum`] command
computes an alternating sum. Successive terms of the sequence
are given alternating signs, with the first term (corresponding
to the lower index value) being positive. Alternating sums
are converted to normal sums with an extra term of the form
‘`(-1)^(k-low)`’. This formula is adjusted appropriately
if the step value is other than one. For example, the Taylor
series for the sine function is ‘`asum(x^k / k!, k, 1, inf, 2)`’.
(Calc cannot evaluate this infinite series, but it can approximate
it if you replace `inf` with any particular odd number.)
Calc converts this series to a regular sum with a step of one,
namely ‘`sum((-1)^k x^(2k+1) / (2k+1)!, k, 0, inf)`’.

The `a *` (`calc-product`) [`prod`] command is
the analogous way to take a product of many terms. Calc also knows
some closed forms for products, such as ‘`prod(k, k, 1, n) = n!`’.
Conditional products can be written ‘`prod(k^prime(k), k, 1, n)`’
or ‘`prod(prime(k) ? k : 1, k, 1, n)`’.

The `a T` (`calc-tabulate`) [`table`] command
evaluates a formula at a series of iterated index values, just
like `sum` and `prod`, but its result is simply a
vector of the results. For example, ‘`table(a_i, i, 1, 7, 2)`’
produces ‘`[a_1, a_3, a_5, a_7]`’.

---

Next: [Rewrite Rules](#Rewrite-Rules), Previous: [Summations](#Summations), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
