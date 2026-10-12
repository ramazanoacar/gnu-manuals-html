### 7.1 Basic Arithmetic [¶](#Basic-Arithmetic-1)

The `+` (`calc-plus`) command adds two numbers. The numbers may
be any of the standard Calc data types. The resulting sum is pushed back
onto the stack.

If both arguments of `+` are vectors or matrices (of matching dimensions),
the result is a vector or matrix sum. If one argument is a vector and the
other a scalar (i.e., a non-vector), the scalar is added to each of the
elements of the vector to form a new vector. If the scalar is not a
number, the operation is left in symbolic form: Suppose you added ‘`x`’
to the vector ‘`[1,2]`’. You may want the result ‘`[1+x,2+x]`’, or
you may plan to substitute a 2-vector for ‘`x`’ in the future. Since
the Calculator can’t tell which interpretation you want, it makes the
safest assumption. See [Reducing and Mapping Vectors](#Reducing-and-Mapping), for a way to add ‘`x`’
to every element of a vector.

If either argument of `+` is a complex number, the result will in general
be complex. If one argument is in rectangular form and the other polar,
the current Polar mode determines the form of the result. If Symbolic
mode is enabled, the sum may be left as a formula if the necessary
conversions for polar addition are non-trivial.

If both arguments of `+` are HMS forms, the forms are added according to
the usual conventions of hours-minutes-seconds notation. If one argument
is an HMS form and the other is a number, that number is converted from
degrees or radians (depending on the current Angular mode) to HMS format
and then the two HMS forms are added.

If one argument of `+` is a date form, the other can be either a
real number, which advances the date by a certain number of days, or
an HMS form, which advances the date by a certain amount of time.
Subtracting two date forms yields the number of days between them.
Adding two date forms is meaningless, but Calc interprets it as the
subtraction of one date form and the negative of the other. (The
negative of a date form can be understood by remembering that dates
are stored as the number of days before or after Jan 1, 1 AD.)

If both arguments of `+` are error forms, the result is an error form
with an appropriately computed standard deviation. If one argument is an
error form and the other is a number, the number is taken to have zero error.
Error forms may have symbolic formulas as their mean and/or error parts;
adding these will produce a symbolic error form result. However, adding an
error form to a plain symbolic formula (as in ‘`(a +/- b) + c`’) will not
work, for the same reasons just mentioned for vectors. Instead you must
write ‘`(a +/- b) + (c +/- 0)`’.

If both arguments of `+` are modulo forms with equal values of ‘`M`’,
or if one argument is a modulo form and the other a plain number, the
result is a modulo form which represents the sum, modulo ‘`M`’, of
the two values.

If both arguments of `+` are intervals, the result is an interval
which describes all possible sums of the possible input values. If
one argument is a plain number, it is treated as the interval
‘`[x .. x]`’.

If one argument of `+` is an infinity and the other is not, the
result is that same infinity. If both arguments are infinite and in
the same direction, the result is the same infinity, but if they are
infinite in different directions the result is `nan`.

The `-` (`calc-minus`) command subtracts two values. The top
number on the stack is subtracted from the one behind it, so that the
computation `5 RET 2 -` produces 3, not *-3*. All options
available for `+` are available for `-` as well.

The `*` (`calc-times`) command multiplies two numbers. If one
argument is a vector and the other a scalar, the scalar is multiplied by
the elements of the vector to produce a new vector. If both arguments
are vectors, the interpretation depends on the dimensions of the
vectors: If both arguments are matrices, a matrix multiplication is
done. If one argument is a matrix and the other a plain vector, the
vector is interpreted as a row vector or column vector, whichever is
dimensionally correct. If both arguments are plain vectors, the result
is a single scalar number which is the dot product of the two vectors.

If one argument of `*` is an HMS form and the other a number, the
HMS form is multiplied by that amount. It is an error to multiply two
HMS forms together, or to attempt any multiplication involving date
forms. Error forms, modulo forms, and intervals can be multiplied;
see the comments for addition of those forms. When two error forms
or intervals are multiplied they are considered to be statistically
independent; thus, ‘`[-2 .. 3] * [-2 .. 3]`’ is ‘`[-6 .. 9]`’,
whereas ‘`[-2 .. 3] ^ 2`’ is ‘`[0 .. 9]`’.

The `/` (`calc-divide`) command divides two numbers.

When combining multiplication and division in an algebraic formula, it
is good style to use parentheses to distinguish between possible
interpretations; the expression ‘`a/b*c`’ should be written
‘`(a/b)*c`’ or ‘`a/(b*c)`’, as appropriate. Without the
parentheses, Calc will interpret ‘`a/b*c`’ as ‘`a/(b*c)`’, since
in algebraic entry Calc gives division a lower precedence than
multiplication. (This is not standard across all computer languages, and
Calc may change the precedence depending on the language mode being used.
See [Language Modes](#Language-Modes).) This default ordering can be changed by setting
the customizable variable `calc-multiplication-has-precedence` to
`nil` (see [Customizing Calc](#Customizing-Calc)); this will give multiplication and
division equal precedences. Note that Calc’s default choice of
precedence allows ‘`a b / c d`’ to be used as a shortcut for

```
a b
---.
c d
```

When dividing a scalar ‘`B`’ by a square matrix ‘`A`’, the
computation performed is ‘`B`’ times the inverse of ‘`A`’. This
also occurs if ‘`B`’ is itself a vector or matrix, in which case the
effect is to solve the set of linear equations represented by ‘`B`’.
If ‘`B`’ is a matrix with the same number of rows as ‘`A`’, or a
plain vector (which is interpreted here as a column vector), then the
equation ‘`A X = B`’ is solved for the vector or matrix ‘`X`’.
Otherwise, if ‘`B`’ is a non-square matrix with the same number of
*columns* as ‘`A`’, the equation ‘`X A = B`’ is solved. If
you wish a vector ‘`B`’ to be interpreted as a row vector to be
solved as ‘`X A = B`’, make it into a one-row matrix with `C-u 1
v p` first. To force a left-handed solution with a square matrix
‘`B`’, transpose ‘`A`’ and ‘`B`’ before dividing, then
transpose the result.

HMS forms can be divided by real numbers or by other HMS forms. Error
forms can be divided in any combination of ways. Modulo forms where both
values and the modulo are integers can be divided to get an integer modulo
form result. Intervals can be divided; dividing by an interval that
encompasses zero or has zero as a limit will result in an infinite
interval.

The `^` (`calc-power`) command raises a number to a power. If
the power is an integer, an exact result is computed using repeated
multiplications. For non-integer powers, Calc uses Newton’s method or
logarithms and exponentials. Square matrices can be raised to integer
powers. If either argument is an error (or interval or modulo) form,
the result is also an error (or interval or modulo) form.

If you press the `I` (inverse) key first, the `I ^` command
computes an Nth root: `125 RET 3 I ^` computes the number 5.
(This is entirely equivalent to `125 RET 1:3 ^`.)

The `\` (`calc-idiv`) command divides two numbers on the stack
to produce an integer result. It is equivalent to dividing with
`/`, then rounding down with `F` (`calc-floor`), only a bit
more convenient and efficient. Also, since it is an all-integer
operation when the arguments are integers, it avoids problems that
`/ F` would have with floating-point roundoff.

The `%` (`calc-mod`) command performs a “modulo” (or “remainder”)
operation. Mathematically, ‘`a%b = a - (a\b)*b`’, and is defined
for all real numbers ‘`a`’ and ‘`b`’ (except ‘`b=0`’). For
positive ‘`b`’, the result will always be between 0 (inclusive) and
‘`b`’ (exclusive). Modulo does not work for HMS forms and error forms.
If ‘`a`’ is a modulo form, its modulo is changed to ‘`b`’, which
must be positive real number.

The `:` (`calc-fdiv`) [`fdiv`] command
divides the two integers on the top of the stack to produce a fractional
result. This is a convenient shorthand for enabling Fraction mode (with
`m f`) temporarily and using ‘`/`’. Note that during numeric entry
the `:` key is interpreted as a fraction separator, so to divide 8 by 6
you would have to type `8 RET 6 RET :`. (Of course, in
this case, it would be much easier simply to enter the fraction directly
as `8:6 RET`!)

The `n` (`calc-change-sign`) command negates the number on the top
of the stack. It works on numbers, vectors and matrices, HMS forms, date
forms, error forms, intervals, and modulo forms.

The `A` (`calc-abs`) [`abs`] command computes the absolute
value of a number. The result of `abs` is always a nonnegative
real number: With a complex argument, it computes the complex magnitude.
With a vector or matrix argument, it computes the Frobenius norm, i.e.,
the square root of the sum of the squares of the absolute values of the
elements. The absolute value of an error form is defined by replacing
the mean part with its absolute value and leaving the error part the same.
The absolute value of a modulo form is undefined. The absolute value of
an interval is defined in the obvious way.

The `f A` (`calc-abssqr`) [`abssqr`] command computes the
absolute value squared of a number, vector or matrix, or error form.

The `f s` (`calc-sign`) [`sign`] command returns 1 if its
argument is positive, *-1* if its argument is negative, or 0 if its
argument is zero. In algebraic form, you can also write ‘`sign(a,x)`’
which evaluates to ‘`x * sign(a)`’, i.e., either ‘`x`’, ‘`-x`’, or
zero depending on the sign of ‘`a`’.

The `&` (`calc-inv`) [`inv`] command computes the
reciprocal of a number, i.e., ‘`1 / x`’. Operating on a square
matrix, it computes the inverse of that matrix.

The `Q` (`calc-sqrt`) [`sqrt`] command computes the square
root of a number. For a negative real argument, the result will be a
complex number whose form is determined by the current Polar mode.

The `f h` (`calc-hypot`) [`hypot`] command computes the square
root of the sum of the squares of two numbers. That is, ‘`hypot(a,b)`’
is the length of the hypotenuse of a right triangle with sides ‘`a`’
and ‘`b`’. If the arguments are complex numbers, their squared
magnitudes are used.

The `f Q` (`calc-isqrt`) [`isqrt`] command computes the
integer square root of an integer. This is the true square root of the
number, rounded down to an integer. For example, ‘`isqrt(10)`’
produces 3. Note that, like `\` [`idiv`], this uses exact
integer arithmetic throughout to avoid roundoff problems. If the input
is a floating-point number or other non-integer value, this is exactly
the same as ‘`floor(sqrt(x))`’.

The `f n` (`calc-min`) [`min`] and `f x` (`calc-max`)
[`max`] commands take the minimum or maximum of two real numbers,
respectively. These commands also work on HMS forms, date forms,
intervals, and infinities. (In algebraic expressions, these functions
take any number of arguments and return the maximum or minimum among
all the arguments.)

The `f M` (`calc-mant-part`) [`mant`] function extracts
the “mantissa” part ‘`m`’ of its floating-point argument; `f X`
(`calc-xpon-part`) [`xpon`] extracts the “exponent” part
‘`e`’. The original number is equal to
‘`m * 10^e`’,
where ‘`m`’ is in the interval ‘`[1.0 .. 10.0)`’ except that
‘`m=e=0`’ if the original number is zero. For integers
and fractions, `mant` returns the number unchanged and `xpon`
returns zero. The `v u` (`calc-unpack`) command can also be
used to “unpack” a floating-point number; this produces an integer
mantissa and exponent, with the constraint that the mantissa is not
a multiple of ten (again except for the ‘`m=e=0`’ case).

The `f S` (`calc-scale-float`) [`scf`] function scales a number
by a given power of ten. Thus, ‘`scf(mant(x), xpon(x)) = x`’ for any
real ‘`x`’. The second argument must be an integer, but the first
may actually be any numeric value. For example, ‘`scf(5,-2) = 0.05`’
or ‘`1:20`’ depending on the current Fraction mode.

The `f [` (`calc-decrement`) [`decr`] and `f ]`
(`calc-increment`) [`incr`] functions decrease or increase
a number by one unit. For integers, the effect is obvious. For
floating-point numbers, the change is by one unit in the last place.
For example, incrementing ‘`12.3456`’ when the current precision
is 6 digits yields ‘`12.3457`’. If the current precision had been
8 digits, the result would have been ‘`12.345601`’. Incrementing
‘`0.0`’ produces
‘`10^-p`’,
where ‘`p`’ is the current
precision. These operations are defined only on integers and floats.
With numeric prefix arguments, they change the number by ‘`n`’ units.

Note that incrementing followed by decrementing, or vice-versa, will
almost but not quite always cancel out. Suppose the precision is
6 digits and the number ‘`9.99999`’ is on the stack. Incrementing
will produce ‘`10.0000`’; decrementing will produce ‘`9.9999`’.
One digit has been dropped. This is an unavoidable consequence of the
way floating-point numbers work.

Incrementing a date/time form adjusts it by a certain number of seconds.
Incrementing a pure date form adjusts it by a certain number of days.

---

Next: [Complex Number Functions](#Complex-Number-Functions), Previous: [Basic Arithmetic](#Basic-Arithmetic), Up: [Arithmetic Functions](#Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 7.2 Integer Truncation [¶](#Integer-Truncation-1)

There are four commands for truncating a real number to an integer,
differing mainly in their treatment of negative numbers. All of these
commands have the property that if the argument is an integer, the result
is the same integer. An integer-valued floating-point argument is converted
to integer form.

If you press `H` (`calc-hyperbolic`) first, the result will be
expressed as an integer-valued floating-point number.

The `F` (`calc-floor`) [`floor` or `ffloor`] command
truncates a real number to the next lower integer, i.e., toward minus
infinity. Thus `3.6 F` produces 3, but `_3.6 F` produces
*-4*.

The `I F` (`calc-ceiling`) [`ceil` or `fceil`]
command truncates toward positive infinity. Thus `3.6 I F` produces
4, and `_3.6 I F` produces *-3*.

The `R` (`calc-round`) [`round` or `fround`] command
rounds to the nearest integer. When the fractional part is .5 exactly,
this command rounds away from zero. (All other rounding in the
Calculator uses this convention as well.) Thus `3.5 R` produces 4
but `3.4 R` produces 3; `_3.5 R` produces *-4*.

The `I R` (`calc-trunc`) [`trunc` or `ftrunc`]
command truncates toward zero. In other words, it “chops off”
everything after the decimal point. Thus `3.6 I R` produces 3 and
`_3.6 I R` produces *-3*.

These functions may not be applied meaningfully to error forms, but they
do work for intervals. As a convenience, applying `floor` to a
modulo form floors the value part of the form. Applied to a vector,
these functions operate on all elements of the vector one by one.
Applied to a date form, they operate on the internal numerical
representation of dates, converting a date/time form into a pure date.

There are two more rounding functions which can only be entered in
algebraic notation. The `roundu` function is like `round`
except that it rounds up, toward plus infinity, when the fractional
part is .5. This distinction matters only for negative arguments.
Also, `rounde` rounds to an even number in the case of a tie,
rounding up or down as necessary. For example, ‘`rounde(3.5)`’ and
‘`rounde(4.5)`’ both return 4, but ‘`rounde(5.5)`’ returns 6.
The advantage of round-to-even is that the net error due to rounding
after a long calculation tends to cancel out to zero. An important
subtle point here is that the number being fed to `rounde` will
already have been rounded to the current precision before `rounde`
begins. For example, ‘`rounde(2.500001)`’ with a current precision
of 6 will incorrectly, or at least surprisingly, yield 2 because the
argument will first have been rounded down to ‘`2.5`’ (which
`rounde` sees as an exact tie between 2 and 3).

Each of these functions, when written in algebraic formulas, allows
a second argument which specifies the number of digits after the
decimal point to keep. For example, ‘`round(123.4567, 2)`’ will
produce the answer 123.46, and ‘`round(123.4567, -1)`’ will
produce 120 (i.e., the cutoff is one digit to the *left* of
the decimal point). A second argument of zero is equivalent to
no second argument at all.

To compute the fractional part of a number (i.e., the amount which, when
added to ‘`floor(`n`)`’, will produce n) just take n
modulo 1 using the `%` command.

Note also the `\` (integer quotient), `f I` (integer logarithm),
and `f Q` (integer square root) commands, which are analogous to
`/`, `B`, and `Q`, respectively, except that they take integer
arguments and return the result rounded down to an integer.

---

Next: [Conversions](#Conversions), Previous: [Integer Truncation](#Integer-Truncation), Up: [Arithmetic Functions](#Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 7.3 Complex Number Functions [¶](#Complex-Number-Functions-1)

The `J` (`calc-conj`) [`conj`] command computes the
complex conjugate of a number. For complex number ‘`a+bi`’, the
complex conjugate is ‘`a-bi`’. If the argument is a real number,
this command leaves it the same. If the argument is a vector or matrix,
this command replaces each element by its complex conjugate.

The `G` (`calc-argument`) [`arg`] command computes the
“argument” or polar angle of a complex number. For a number in polar
notation, this is simply the second component of the pair
‘`(`r`;`theta`)`’.
The result is expressed according to the current angular mode and will
be in the range *-180* degrees (exclusive) to *+180* degrees
(inclusive), or the equivalent range in radians.

The `calc-imaginary` command multiplies the number on the
top of the stack by the imaginary number ‘`i = (0,1)`’. This
command is not normally bound to a key in Calc, but it is available
on the `IMAG` button in Keypad mode.

The `f r` (`calc-re`) [`re`] command replaces a complex number
by its real part. This command has no effect on real numbers. (As an
added convenience, `re` applied to a modulo form extracts
the value part.)

The `f i` (`calc-im`) [`im`] command replaces a complex number
by its imaginary part; real numbers are converted to zero. With a vector
or matrix argument, these functions operate element-wise.

The `v p` (`calc-pack`) command can pack the top two numbers on
the stack into a composite object such as a complex number. With
a prefix argument of *-1*, it produces a rectangular complex number;
with an argument of *-2*, it produces a polar complex number.
(Also, see [Building Vectors](#Building-Vectors).)

The `v u` (`calc-unpack`) command takes the complex number
(or other composite object) on the top of the stack and unpacks it
into its separate components.

---

Next: [Date Arithmetic](#Date-Arithmetic), Previous: [Complex Number Functions](#Complex-Number-Functions), Up: [Arithmetic Functions](#Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 7.4 Conversions [¶](#Conversions-1)

The commands described in this section convert numbers from one form
to another; they are two-key sequences beginning with the letter `c`.

The `c f` (`calc-float`) [`pfloat`] command converts the
number on the top of the stack to floating-point form. For example,
‘`23`’ is converted to ‘`23.0`’, ‘`3:2`’ is converted to
‘`1.5`’, and ‘`2.3`’ is left the same. If the value is a composite
object such as a complex number or vector, each of the components is
converted to floating-point. If the value is a formula, all numbers
in the formula are converted to floating-point. Note that depending
on the current floating-point precision, conversion to floating-point
format may lose information.

As a special exception, integers which appear as powers or subscripts
are not floated by `c f`. If you really want to float a power,
you can use a `j s` command to select the power followed by `c f`.
Because `c f` cannot examine the formula outside of the selection,
it does not notice that the thing being floated is a power.
See [Selecting Sub-Formulas](#Selecting-Subformulas).

The normal `c f` command is “pervasive” in the sense that it
applies to all numbers throughout the formula. The `pfloat`
algebraic function never stays around in a formula; ‘`pfloat(a + 1)`’
changes to ‘`a + 1.0`’ as soon as it is evaluated.

With the Hyperbolic flag, `H c f` [`float`] operates
only on the number or vector of numbers at the top level of its
argument. Thus, ‘`float(1)`’ is 1.0, but ‘`float(a + 1)`’
is left unevaluated because its argument is not a number.

You should use `H c f` if you wish to guarantee that the final
value, once all the variables have been assigned, is a float; you
would use `c f` if you wish to do the conversion on the numbers
that appear right now.

The `c F` (`calc-fraction`) [`pfrac`] command converts a
floating-point number into a fractional approximation. By default, it
produces a fraction whose decimal representation is the same as the
input number, to within the current precision. You can also give a
numeric prefix argument to specify a tolerance, either directly, or,
if the prefix argument is zero, by using the number on top of the stack
as the tolerance. If the tolerance is a positive integer, the fraction
is correct to within that many significant figures. If the tolerance is
a non-positive integer, it specifies how many digits fewer than the current
precision to use. If the tolerance is a floating-point number, the
fraction is correct to within that absolute amount.

The `pfrac` function is pervasive, like `pfloat`.
There is also a non-pervasive version, `H c F` [`frac`],
which is analogous to `H c f` discussed above.

The `c d` (`calc-to-degrees`) [`deg`] command converts a
number into degrees form. The value on the top of the stack may be an
HMS form (interpreted as degrees-minutes-seconds), or a real number which
will be interpreted in radians regardless of the current angular mode.

The `c r` (`calc-to-radians`) [`rad`] command converts an
HMS form or angle in degrees into an angle in radians.

The `c h` (`calc-to-hms`) [`hms`] command converts a real
number, interpreted according to the current angular mode, to an HMS
form describing the same angle. In algebraic notation, the `hms`
function also accepts three arguments: ‘`hms(h, m, s)`’.
(The three-argument version is independent of the current angular mode.)

The `calc-from-hms` command converts the HMS form on the top of the
stack into a real number according to the current angular mode.

The `c p` (`calc-polar`) command converts the complex number on
the top of the stack from polar to rectangular form, or from rectangular
to polar form, whichever is appropriate. Real numbers are left the same.
This command is equivalent to the `rect` or `polar`
functions in algebraic formulas, depending on the direction of
conversion. (It uses `polar`, except that if the argument is
already a polar complex number, it uses `rect` instead. The
`I c p` command always uses `rect`.)

The `c c` (`calc-clean`) [`pclean`] command “cleans” the
number on the top of the stack. Floating point numbers are re-rounded
according to the current precision. Polar numbers whose angular
components have strayed from the *-180* to *+180* degree range
are normalized. (Note that results will be undesirable if the current
angular mode is different from the one under which the number was
produced!) Integers and fractions are generally unaffected by this
operation. Vectors and formulas are cleaned by cleaning each component
number (i.e., pervasively).

If the simplification mode is set below basic simplification, it is raised
for the purposes of this command. Thus, `c c` applies the basic
simplifications even if their automatic application is disabled.
See [Simplification Modes](#Simplification-Modes).

A numeric prefix argument to `c c` sets the floating-point precision
to that value for the duration of the command. A positive prefix (of at
least 3) sets the precision to the specified value; a negative or zero
prefix decreases the precision by the specified amount.

The keystroke sequences `c 0` through `c 9` are equivalent
to `c c` with the corresponding negative prefix argument. If roundoff
errors have changed 2.0 into 1.999999, typing `c 1` to clip off one
decimal place often conveniently does the trick.

The `c c` command with a numeric prefix argument, and the `c 0`
through `c 9` commands, also “clip” very small floating-point
numbers to zero. If the exponent is less than or equal to the negative
of the specified precision, the number is changed to 0.0. For example,
if the current precision is 12, then `c 2` changes the vector
‘`[1e-8, 1e-9, 1e-10, 1e-11]`’ to ‘`[1e-8, 1e-9, 0, 0]`’.
Numbers this small generally arise from roundoff noise.

If the numbers you are using really are legitimately this small,
you should avoid using the `c 0` through `c 9` commands.
(The plain `c c` command rounds to the current precision but
does not clip small numbers.)

One more property of `c 0` through `c 9`, and of `c c` with
a prefix argument, is that integer-valued floats are converted to
plain integers, so that `c 1` on ‘`[1., 1.5, 2., 2.5, 3.]`’
produces ‘`[1, 1.5, 2, 2.5, 3]`’. This is not done for huge
numbers (‘`1e100`’ is technically an integer-valued float, but
you wouldn’t want it automatically converted to a 100-digit integer).

With the Hyperbolic flag, `H c c` and `H c 0` through `H c 9`
operate non-pervasively [`clean`].

---

Next: [Financial Functions](#Financial-Functions), Previous: [Conversions](#Conversions), Up: [Arithmetic Functions](#Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 7.5 Date Arithmetic [¶](#Date-Arithmetic-1)

The commands described in this section perform various conversions
and calculations involving date forms (see [Date Forms](#Date-Forms)). They
use the `t` (for time/date) prefix key followed by shifted
letters.

The simplest date arithmetic is done using the regular `+` and `-`
commands. In particular, adding a number to a date form advances the
date form by a certain number of days; adding an HMS form to a date
form advances the date by a certain amount of time; and subtracting two
date forms produces a difference measured in days. The commands
described here provide additional, more specialized operations on dates.

Many of these commands accept a numeric prefix argument; if you give
plain `C-u` as the prefix, these commands will instead take the
additional argument from the top of the stack.

* [Date Conversions](#Date-Conversions)
* [Date Functions](#Date-Functions)
* [Business Days](#Business-Days)
* [Time Zones](#Time-Zones)

---

Next: [Date Functions](#Date-Functions), Up: [Date Arithmetic](#Date-Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.5.1 Date Conversions [¶](#Date-Conversions-1)

The `t D` (`calc-date`) [`date`] command converts a
date form into a number, measured in days since Jan 1, 1 AD. The
result will be an integer if date is a pure date form, or a
fraction or float if date is a date/time form. Or, if its
argument is a number, it converts this number into a date form.

With a numeric prefix argument, `t D` takes that many objects
(up to six) from the top of the stack and interprets them in one
of the following ways:

The ‘`date(year, month, day)`’ function
builds a pure date form out of the specified year, month, and
day, which must all be integers. Year is a year number,
such as 1991 (*not* the same as 91!). Month must be
an integer in the range 1 to 12; day must be in the range
1 to 31. If the specified month has fewer than 31 days and
day is too large, the equivalent day in the following
month will be used.

The ‘`date(month, day)`’ function builds a
pure date form using the current year, as determined by the
real-time clock.

The ‘`date(year, month, day, hms)`’
function builds a date/time form using an hms form.

The ‘`date(year, month, day, hour,
minute, second)`’ function builds a date/time form.
hour should be an integer in the range 0 to 23;
minute should be an integer in the range 0 to 59;
second should be any real number in the range ‘`[0 .. 60)`’.
The last two arguments default to zero if omitted.

The `t J` (`calc-julian`) [`julian`] command converts
a date form into a Julian day count, which is the number of days
since noon (GMT) on Jan 1, 4713 BC. A pure date is converted to an
integer Julian count representing noon of that day. A date/time form
is converted to an exact floating-point Julian count, adjusted to
interpret the date form in the current time zone but the Julian
day count in Greenwich Mean Time. A numeric prefix argument allows
you to specify the time zone; see [Time Zones](#Time-Zones). Use a prefix of
zero to suppress the time zone adjustment. Note that pure date forms
are never time-zone adjusted.

This command can also do the opposite conversion, from a Julian day
count (either an integer day, or a floating-point day and time in
the GMT zone), into a pure date form or a date/time form in the
current or specified time zone.

The `t U` (`calc-unix-time`) [`unixtime`] command
converts a date form into a Unix time value, which is the number of
seconds since midnight on Jan 1, 1970, or vice-versa. The numeric result
will be an integer if the current precision is 12 or less; for higher
precision, the result may be a float with (precision−12)
digits after the decimal. Just as for `t J`, the numeric time
is interpreted in the GMT time zone and the date form is interpreted
in the current or specified zone. Some systems use Unix-like
numbering but with the local time zone; give a prefix of zero to
suppress the adjustment if so.

The `t C` (`calc-convert-time-zones`) [`tzconv`]
command converts a date form from one time zone to another. You
are prompted for each time zone name in turn; you can answer with
any suitable Calc time zone expression (see [Time Zones](#Time-Zones)).
If you answer either prompt with a blank line, the local time
zone is used for that prompt. You can also answer the first
prompt with `$` to take the two time zone names from the
stack (and the date to be converted from the third stack level).

---

Next: [Business Days](#Business-Days), Previous: [Date Conversions](#Date-Conversions), Up: [Date Arithmetic](#Date-Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.5.2 Date Functions [¶](#Date-Functions-1)

The `t N` (`calc-now`) [`now`] command pushes the
current date and time on the stack as a date form. The time is
reported in terms of the specified time zone; with no numeric prefix
argument, `t N` reports for the current time zone.

The `t P` (`calc-date-part`) command extracts one part
of a date form. The prefix argument specifies the part; with no
argument, this command prompts for a part code from 1 to 9.
The various part codes are described in the following paragraphs.

The `M-1 t P` [`year`] function extracts the year number
from a date form as an integer, e.g., 1991. This and the
following functions will also accept a real number for an
argument, which is interpreted as a standard Calc day number.
Note that this function will never return zero, since the year
1 BC immediately precedes the year 1 AD.

The `M-2 t P` [`month`] function extracts the month number
from a date form as an integer in the range 1 to 12.

The `M-3 t P` [`day`] function extracts the day number
from a date form as an integer in the range 1 to 31.

The `M-4 t P` [`hour`] function extracts the hour from
a date form as an integer in the range 0 (midnight) to 23. Note
that 24-hour time is always used. This returns zero for a pure
date form. This function (and the following two) also accept
HMS forms as input.

The `M-5 t P` [`minute`] function extracts the minute
from a date form as an integer in the range 0 to 59.

The `M-6 t P` [`second`] function extracts the second
from a date form. If the current precision is 12 or less,
the result is an integer in the range 0 to 59. For higher
precision, the result may instead be a floating-point number.

The `M-7 t P` [`weekday`] function extracts the weekday
number from a date form as an integer in the range 0 (Sunday)
to 6 (Saturday).

The `M-8 t P` [`yearday`] function extracts the day-of-year
number from a date form as an integer in the range 1 (January 1)
to 366 (December 31 of a leap year).

The `M-9 t P` [`time`] function extracts the time portion
of a date form as an HMS form. This returns ‘`0@ 0' 0"`’
for a pure date form.

The `t M` (`calc-new-month`) [`newmonth`] command
computes a new date form that represents the first day of the month
specified by the input date. The result is always a pure date
form; only the year and month numbers of the input are retained.
With a numeric prefix argument n in the range from 1 to 31,
`t M` computes the nth day of the month. (If n
is greater than the actual number of days in the month, or if
n is zero, the last day of the month is used.)

The `t Y` (`calc-new-year`) [`newyear`] command
computes a new pure date form that represents the first day of
the year specified by the input. The month, day, and time
of the input date form are lost. With a numeric prefix argument
n in the range from 1 to 366, `t Y` computes the
nth day of the year (366 is treated as 365 in non-leap
years). A prefix argument of 0 computes the last day of the
year (December 31). A negative prefix argument from *-1* to
*-12* computes the first day of the nth month of the year.

The `t W` (`calc-new-week`) [`newweek`] command
computes a new pure date form that represents the Sunday on or before
the input date. With a numeric prefix argument, it can be made to
use any day of the week as the starting day; the argument must be in
the range from 0 (Sunday) to 6 (Saturday). This function always
subtracts between 0 and 6 days from the input date.

Here’s an example use of `newweek`: Find the date of the next
Wednesday after a given date. Using `M-3 t W` or ‘`newweek(d, 3)`’
will give you the *preceding* Wednesday, so ‘`newweek(d+7, 3)`’
will give you the following Wednesday. A further look at the definition
of `newweek` shows that if the input date is itself a Wednesday,
this formula will return the Wednesday one week in the future. An
exercise for the reader is to modify this formula to yield the same day
if the input is already a Wednesday. Another interesting exercise is
to preserve the time-of-day portion of the input (`newweek` resets
the time to midnight; hint: how can `newweek` be defined in terms
of the `weekday` function?).

The ‘`pwday(date)`’ function (not on any key) computes the
day-of-month number of the Sunday on or before date. With
two arguments, ‘`pwday(date, day)`’ computes the day
number of the Sunday on or before day number day of the month
specified by date. The day must be in the range from
7 to 31; if the day number is greater than the actual number of days
in the month, the true number of days is used instead. Thus
‘`pwday(date, 7)`’ finds the first Sunday of the month, and
‘`pwday(date, 31)`’ finds the last Sunday of the month.
With a third weekday argument, `pwday` can be made to look
for any day of the week instead of Sunday.

The `t I` (`calc-inc-month`) [`incmonth`] command
increases a date form by one month, or by an arbitrary number of
months specified by a numeric prefix argument. The time portion,
if any, of the date form stays the same. The day also stays the
same, except that if the new month has fewer days the day
number may be reduced to lie in the valid range. For example,
‘`incmonth(<Jan 31, 1991>)`’ produces ‘`<Feb 28, 1991>`’.
Because of this, `t I t I` and `M-2 t I` do not always give
the same results (‘`<Mar 28, 1991>`’ versus ‘`<Mar 31, 1991>`’
in this case).

The ‘`incyear(date, step)`’ function increases
a date form by the specified number of years, which may be
any positive or negative integer. Note that ‘`incyear(d, n)`’
is equivalent to ‘`incmonth(d, 12*n)`’, but these do not have
simple equivalents in terms of day arithmetic because
months and years have varying lengths. If the step
argument is omitted, 1 year is assumed. There is no keyboard
command for this function; use `C-u 12 t I` instead.

There is no `newday` function at all because `F` [`floor`]
serves this purpose. Similarly, instead of `incday` and
`incweek` simply use ‘`d + n`’ or ‘`d + 7 n`’.

See [Basic Arithmetic](#Basic-Arithmetic), for the `f ]` [`incr`] command
which can adjust a date/time form by a certain number of seconds.

---

Next: [Time Zones](#Time-Zones), Previous: [Date Functions](#Date-Functions), Up: [Date Arithmetic](#Date-Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.5.3 Business Days [¶](#Business-Days-1)

Often time is measured in “business days” or “working days,”
where weekends and holidays are skipped. Calc’s normal date
arithmetic functions use calendar days, so that subtracting two
consecutive Mondays will yield a difference of 7 days. By contrast,
subtracting two consecutive Mondays would yield 5 business days
(assuming two-day weekends and the absence of holidays).

The `t +` (`calc-business-days-plus`) [`badd`]
and `t -` (`calc-business-days-minus`) [`bsub`]
commands perform arithmetic using business days. For `t +`,
one argument must be a date form and the other must be a real
number (positive or negative). If the number is not an integer,
then a certain amount of time is added as well as a number of
days; for example, adding 0.5 business days to a time in Friday
evening will produce a time in Monday morning. It is also
possible to add an HMS form; adding ‘`12@ 0' 0"`’ also adds
half a business day. For `t -`, the arguments are either a
date form and a number or HMS form, or two date forms, in which
case the result is the number of business days between the two
dates.

By default, Calc considers any day that is not a Saturday or
Sunday to be a business day. You can define any number of
additional holidays by editing the variable `Holidays`.
(There is an `s H` convenience command for editing this
variable.) Initially, `Holidays` contains the vector
‘`[sat, sun]`’. Entries in the `Holidays` vector may
be any of the following kinds of objects:

* Date forms (pure dates, not date/time forms). These specify
  particular days which are to be treated as holidays.
* Intervals of date forms. These specify a range of days, all of
  which are holidays (e.g., Christmas week). See [Interval Forms](#Interval-Forms).
* Nested vectors of date forms. Each date form in the vector is
  considered to be a holiday.
* Any Calc formula which evaluates to one of the above three things.
  If the formula involves the variable ‘`y`’, it stands for a
  yearly repeating holiday; ‘`y`’ will take on various year
  numbers like 1992. For example, ‘`date(y, 12, 25)`’ specifies
  Christmas day, and ‘`newweek(date(y, 11, 7), 4) + 21`’ specifies
  Thanksgiving (which is held on the fourth Thursday of November).
  If the formula involves the variable ‘`m`’, that variable
  takes on month numbers from 1 to 12: ‘`date(y, m, 15)`’ is
  a holiday that takes place on the 15th of every month.
* A weekday name, such as `sat` or `sun`. This is really
  a variable whose name is a three-letter, lower-case day name.
* An interval of year numbers (integers). This specifies the span of
  years over which this holiday list is to be considered valid. Any
  business-day arithmetic that goes outside this range will result
  in an error message. Use this if you are including an explicit
  list of holidays, rather than a formula to generate them, and you
  want to make sure you don’t accidentally go beyond the last point
  where the holidays you entered are complete. If there is no
  limiting interval in the `Holidays` vector, the default
  ‘`[1 .. 2737]`’ is used. (This is the absolute range of years
  for which Calc’s business-day algorithms will operate.)
* An interval of HMS forms. This specifies the span of hours that
  are to be considered one business day. For example, if this
  range is ‘`[9@ 0' 0" .. 17@ 0' 0"]`’ (i.e., 9am to 5pm), then
  the business day is only eight hours long, so that `1.5 t +`
  on ‘`<4:00pm Fri Dec 13, 1991>`’ will add one business day and
  four business hours to produce ‘`<12:00pm Tue Dec 17, 1991>`’.
  Likewise, `t -` will now express differences in time as
  fractions of an eight-hour day. Times before 9am will be treated
  as 9am by business date arithmetic, and times at or after 5pm will
  be treated as 4:59:59pm. If there is no HMS interval in `Holidays`,
  the full 24-hour day ‘`[0 0' 0" .. 24 0' 0"]`’ is assumed.
  (Regardless of the type of bounds you specify, the interval is
  treated as inclusive on the low end and exclusive on the high end,
  so that the work day goes from 9am up to, but not including, 5pm.)

If the `Holidays` vector is empty, then `t +` and
`t -` will act just like `+` and `-` because there will
then be no difference between business days and calendar days.

Calc expands the intervals and formulas you give into a complete
list of holidays for internal use. This is done mainly to make
sure it can detect multiple holidays. (For example,
‘`<Jan 1, 1989>`’ is both New Year’s Day and a Sunday, but
Calc’s algorithms take care to count it only once when figuring
the number of holidays between two dates.)

Since the complete list of holidays for all the years from 1 to
2737 would be huge, Calc actually computes only the part of the
list between the smallest and largest years that have been involved
in business-day calculations so far. Normally, you won’t have to
worry about this. Keep in mind, however, that if you do one
calculation for 1992, and another for 1792, even if both involve
only a small range of years, Calc will still work out all the
holidays that fall in that 200-year span.

If you add a (positive) number of days to a date form that falls on a
weekend or holiday, the date form is treated as if it were the most
recent business day. (Thus adding one business day to a Friday,
Saturday, or Sunday will all yield the following Monday.) If you
subtract a number of days from a weekend or holiday, the date is
effectively on the following business day. (So subtracting one business
day from Saturday, Sunday, or Monday yields the preceding Friday.) The
difference between two dates one or both of which fall on holidays
equals the number of actual business days between them. These
conventions are consistent in the sense that, if you add n
business days to any date, the difference between the result and the
original date will come out to n business days. (It can’t be
completely consistent though; a subtraction followed by an addition
might come out a bit differently, since `t +` is incapable of
producing a date that falls on a weekend or holiday.)

There is a `holiday` function, not on any keys, that takes
any date form and returns 1 if that date falls on a weekend or
holiday, as defined in `Holidays`, or 0 if the date is a
business day.

---

Previous: [Business Days](#Business-Days), Up: [Date Arithmetic](#Date-Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.5.4 Time Zones [¶](#Time-Zones-1)

Time zones and daylight saving time are a complicated business.
The conversions to and from Julian and Unix-style dates automatically
compute the correct time zone and daylight saving adjustment to use,
provided they can figure out this information. This section describes
Calc’s time zone adjustment algorithm in detail, in case you want to
do conversions in different time zones or in case Calc’s algorithms
can’t determine the right correction to use.

Adjustments for time zones and daylight saving time are done by
`t U`, `t J`, `t N`, and `t C`, but not by any other
commands. In particular, ‘`<may 1 1991> - <apr 1 1991>`’ evaluates
to exactly 30 days even though there is a daylight-saving
transition in between. This is also true for Julian pure dates:
‘`julian(<may 1 1991>) - julian(<apr 1 1991>)`’. But Julian
and Unix date/times will adjust for daylight saving time: using Calc’s
default daylight saving time rule (see the explanation below),
‘`julian(<12am may 1 1991>) - julian(<12am apr 1 1991>)`’
evaluates to ‘`29.95833`’ (that’s 29 days and 23 hours)
because one hour was lost when daylight saving commenced on
April 7, 1991.

In brief, the idiom ‘`julian(date1) - julian(date2)`’
computes the actual number of 24-hour periods between two dates, whereas
‘`date1 - date2`’ computes the number of calendar
days between two dates without taking daylight saving into account.

The `calc-time-zone` [`tzone`] command converts the time
zone specified by its numeric prefix argument into a number of
seconds difference from Greenwich mean time (GMT). If the argument
is a number, the result is simply that value multiplied by 3600.
Typical arguments for North America are 5 (Eastern) or 8 (Pacific). If
Daylight Saving time is in effect, one hour should be subtracted from
the normal difference.

If you give a prefix of plain `C-u`, `calc-time-zone` (like other
date arithmetic commands that include a time zone argument) takes the
zone argument from the top of the stack. (In the case of `t J`
and `t U`, the normal argument is then taken from the second-to-top
stack position.) This allows you to give a non-integer time zone
adjustment. The time-zone argument can also be an HMS form, or
it can be a variable which is a time zone name in upper- or lower-case.
For example ‘`tzone(PST) = tzone(8)`’ and ‘`tzone(pdt) = tzone(7)`’
(for Pacific standard and daylight saving times, respectively).

North American and European time zone names are defined as follows.
These names are obsolescent and new code should not rely on them:
the ‘`YST`’-related names have disagreed with time in Yukon since 1973,
and other names could well become confusing or wrong in the future
as countries change their time zone rules.
For each time zone there is one name for standard time,
another for daylight saving time, and a third for “generalized” time
in which the daylight saving adjustment is computed from context.

```
YST  PST  MST  CST  EST  AST    NST    GMT   WET     MET    MEZ
 9    8    7    6    5    4     3.5     0     -1      -2     -2

YDT  PDT  MDT  CDT  EDT  ADT    NDT    BST  WETDST  METDST  MESZ
 8    7    6    5    4    3     2.5     -1    -2      -3     -3

YGT  PGT  MGT  CGT  EGT  AGT    NGT    BGT   WEGT    MEGT   MEGZ
9/8  8/7  7/6  6/5  5/4  4/3  3.5/2.5  0/-1 -1/-2   -2/-3  -2/-3
```

To define time zone names that do not appear in the above table,
you must modify the Lisp variable `math-tzone-names`. This
is a list of lists describing the different time zone names; its
structure is best explained by an example. The three entries for
circa-2022 US Pacific Time look like this:

```
( ( "PST" 8 0 )    ; Name as an upper-case string, then standard
  ( "PDT" 8 -1 )   ; adjustment, then daylight saving adjustment.
  ( "PGT" 8 "PST" "PDT" ) )   ; Generalized time zone.
```

With no arguments, `calc-time-zone` or ‘`tzone()`’ will by
default get the time zone and daylight saving information from the
calendar (see [The Calendar and the Diary](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Daylight-Saving) in The GNU Emacs Manual). To use a different time zone, or if the
calendar does not give the desired result, you can set the Calc variable
`TimeZone` (which is by default `nil`) to an appropriate
time zone name. (The easiest way to do this is to edit the
`TimeZone` variable using Calc’s `s T` command, then use the
`s p` (`calc-permanent-variable`) command to save the value of
`TimeZone` permanently.)
If the time zone given by `TimeZone` is a generalized time zone,
e.g., `EGT`, Calc examines the date being converted to tell whether
to use standard or daylight saving time. But if the current time zone
is explicit, e.g., `EST` or `EDT`, then that adjustment is
used exactly and Calc’s daylight saving algorithm is not consulted.
The special time zone name `local`
is equivalent to no argument; i.e., it uses the information obtained
from the calendar.

The `t J` and `t U` commands with no numeric prefix
arguments do the same thing as ‘`tzone()`’; namely, use the
information from the calendar if `TimeZone` is `nil`,
otherwise use the time zone given by `TimeZone`.

When Calc computes the daylight saving information itself (i.e., when
the `TimeZone` variable is set), it will by default consider
daylight saving time to begin at 2 a.m. on the second Sunday of March
(for years from 2007 on) or on the last Sunday in April (for years
before 2007), and to end at 2 a.m. on the first Sunday of
November. (for years from 2007 on) or the last Sunday in October (for
years before 2007). These are the rules that have been in effect in
much of North America since 1966 and take into account the rule change
that began in 2007. If you are in a country that uses different rules
for computing daylight saving time, you have two choices: Write your own
daylight saving hook, or control time zones explicitly by setting the
`TimeZone` variable and/or always giving a time-zone argument for
the conversion functions.

The Lisp variable `math-daylight-savings-hook` holds the
name of a function that is used to compute the daylight saving
adjustment for a given date. The default is
`math-std-daylight-savings`, which computes an adjustment
(either 0 or *-1*) using the North American rules given above.

The daylight saving hook function is called with four arguments:
The date, as a floating-point number in standard Calc format;
a six-element list of the date decomposed into year, month, day,
hour, minute, and second, respectively; a string which contains
the generalized time zone name in upper-case, e.g., `"WEGT"`;
and a special adjustment to be applied to the hour value when
converting into a generalized time zone (see below).

The Lisp function `math-prev-weekday-in-month` is useful for
daylight saving computations. This is an internal version of
the user-level `pwday` function described in the previous
section. It takes four arguments: The floating-point date value,
the corresponding six-element date list, the day-of-month number,
and the weekday number (0–6).

The default daylight saving hook ignores the time zone name, but a
more sophisticated hook could use different algorithms for different
time zones. It would also be possible to use different algorithms
depending on the year number, but the default hook always uses the
algorithm for 1987 and later. Here is a listing of the default
daylight saving hook:

```
(defun math-std-daylight-savings (date dt zone bump)
  (cond ((< (nth 1 dt) 4) 0)
        ((= (nth 1 dt) 4)
         (let ((sunday (math-prev-weekday-in-month date dt 7 0)))
           (cond ((< (nth 2 dt) sunday) 0)
                 ((= (nth 2 dt) sunday)
                  (if (>= (nth 3 dt) (+ 3 bump)) -1 0))
                 (t -1))))
        ((< (nth 1 dt) 10) -1)
        ((= (nth 1 dt) 10)
         (let ((sunday (math-prev-weekday-in-month date dt 31 0)))
           (cond ((< (nth 2 dt) sunday) -1)
                 ((= (nth 2 dt) sunday)
                  (if (>= (nth 3 dt) (+ 2 bump)) 0 -1))
                 (t 0))))
        (t 0))
)
```

The `bump` parameter is equal to zero when Calc is converting
from a date form in a generalized time zone into a GMT date value.
It is *-1* when Calc is converting in the other direction. The
adjustments shown above ensure that the conversion behaves correctly
and reasonably around the 2 a.m. transition in each direction.

There is a “missing” hour between 2 a.m. and 3 a.m. at the
beginning of daylight saving time; converting a date/time form that
falls in this hour results in a time value for the following hour,
from 3 a.m. to 4 a.m. At the end of daylight saving time, the
hour from 1 a.m. to 2 a.m. repeats itself; converting a date/time
form that falls in this hour results in a time value for the first
manifestation of that time (*not* the one that occurs one hour
later).

If `math-daylight-savings-hook` is `nil`, then the
daylight saving adjustment is always taken to be zero.

In algebraic formulas, ‘`tzone(zone, date)`’
computes the time zone adjustment for a given zone name at a
given date. The date is ignored unless zone is a
generalized time zone. If date is a date form, the
daylight saving computation is applied to it as it appears.
If date is a numeric date value, it is adjusted for the
daylight-saving version of zone before being given to
the daylight saving hook. This odd-sounding rule ensures
that the daylight-saving computation is always done in
local time, not in the GMT time that a numeric date
is typically represented in.

The ‘`dsadj(date, zone)`’ function computes the
daylight saving adjustment that is appropriate for date in
time zone zone. If zone is explicitly in or not in
daylight saving time (e.g., `PDT` or `PST`) the
date is ignored. If zone is a generalized time zone,
the algorithms described above are used. If zone is omitted,
the computation is done for the current time zone.

---

Next: [Binary Number Functions](#Binary-Functions), Previous: [Date Arithmetic](#Date-Arithmetic), Up: [Arithmetic Functions](#Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 7.6 Financial Functions [¶](#Financial-Functions-1)

Calc’s financial or business functions use the `b` prefix
key followed by a shifted letter. (The `b` prefix followed by
a lower-case letter is used for operations on binary numbers.)

Note that the rate and the number of intervals given to these
functions must be on the same time scale, e.g., both months or
both years. Mixing an annual interest rate with a time expressed
in months will give you very wrong answers!

It is wise to compute these functions to a higher precision than
you really need, just to make sure your answer is correct to the
last penny; also, you may wish to check the definitions at the end
of this section to make sure the functions have the meaning you expect.

* [Percentages](#Percentages)
* [Future Value](#Future-Value)
* [Present Value](#Present-Value)
* [Related Financial Functions](#Related-Financial-Functions)
* [Depreciation Functions](#Depreciation-Functions)
* [Definitions](#Definitions-of-Financial-Functions)

---

Next: [Future Value](#Future-Value), Up: [Financial Functions](#Financial-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.6.1 Percentages [¶](#Percentages-1)

The `M-%` (`calc-percent`) command takes a percentage value,
say 5.4, and converts it to an equivalent actual number. For example,
`5.4 M-%` enters 0.054 on the stack. (That’s the `META` or
`ESC` key combined with `%`.)

Actually, `M-%` creates a formula of the form ‘`5.4%`’.
You can enter ‘`5.4%`’ yourself during algebraic entry. The
‘`%`’ operator simply means, “the preceding value divided by
100.” The ‘`%`’ operator has very high precedence, so that
‘`1+8%`’ is interpreted as ‘`1+(8%)`’, not as ‘`(1+8)%`’.
(The ‘`%`’ operator is just a postfix notation for the
`percent` function, just like ‘`20!`’ is the notation for
‘`fact(20)`’, or twenty-factorial.)

The formula ‘`5.4%`’ would normally evaluate immediately to
0.054, but the `M-%` command suppresses evaluation as it puts
the formula onto the stack. However, the next Calc command that
uses the formula ‘`5.4%`’ will evaluate it as its first step.
The net effect is that you get to look at ‘`5.4%`’ on the stack,
but Calc commands see it as ‘`0.054`’, which is what they expect.

In particular, ‘`5.4%`’ and ‘`0.054`’ are suitable values
for the rate arguments of the various financial functions,
but the number ‘`5.4`’ is probably *not* suitable—it
represents a rate of 540 percent!

The key sequence `M-% *` effectively means “percent-of.”
For example, `68 RET 25 M-% *` computes 17, which is 25% of
68 (and also 68% of 25, which comes out to the same thing).

The `c %` (`calc-convert-percent`) command converts the
value on the top of the stack from numeric to percentage form.
For example, if 0.08 is on the stack, `c %` converts it to
‘`8%`’. The quantity is the same, it’s just represented
differently. (Contrast this with `M-%`, which would convert
this number to ‘`0.08%`’.) The `=` key is a convenient way
to convert a formula like ‘`8%`’ back to numeric form, 0.08.

To compute what percentage one quantity is of another quantity,
use `/ c %`. For example, `17 RET 68 / c %` displays
‘`25%`’.

The `b %` (`calc-percent-change`) [`relch`] command
calculates the percentage change from one number to another.
For example, `40 RET 50 b %` produces the answer ‘`25%`’,
since 50 is 25% larger than 40. A negative result represents a
decrease: `50 RET 40 b %` produces ‘`-20%`’, since 40 is
20% smaller than 50. (The answers are different in magnitude
because, in the first case, we’re increasing by 25% of 40, but
in the second case, we’re decreasing by 20% of 50.) The effect
of `40 RET 50 b %` is to compute ‘`(50-40)/40`’, converting
the answer to percentage form as if by `c %`.

---

Next: [Present Value](#Present-Value), Previous: [Percentages](#Percentages), Up: [Financial Functions](#Financial-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.6.2 Future Value [¶](#Future-Value-1)

The `b F` (`calc-fin-fv`) [`fv`] command computes
the future value of an investment. It takes three arguments
from the stack: ‘`fv(rate, n, payment)`’.
If you give payments of payment every year for n
years, and the money you have paid earns interest at rate per
year, then this function tells you what your investment would be
worth at the end of the period. (The actual interval doesn’t
have to be years, as long as n and rate are expressed
in terms of the same intervals.) This function assumes payments
occur at the *end* of each interval.

The `I b F` [`fvb`] command does the same computation,
but assuming your payments are at the beginning of each interval.
Suppose you plan to deposit $1000 per year in a savings account
earning 5.4% interest, starting right now. How much will be
in the account after five years? `fvb(5.4%, 5, 1000) = 5870.73`.
Thus you will have earned $870 worth of interest over the years.
Using the stack, this calculation would have been
`5.4 M-% 5 RET 1000 I b F`. Note that the rate is expressed
as a number between 0 and 1, *not* as a percentage.

The `H b F` [`fvl`] command computes the future value
of an initial lump sum investment. Suppose you could deposit
those five thousand dollars in the bank right now; how much would
they be worth in five years? `fvl(5.4%, 5, 5000) = 6503.89`.

The algebraic functions `fv` and `fvb` accept an optional
fourth argument, which is used as an initial lump sum in the sense
of `fvl`. In other words, `fv(rate, n,
payment, initial) = fv(rate, n, payment)
+ fvl(rate, n, initial)`.

To illustrate the relationships between these functions, we could
do the `fvb` calculation “by hand” using `fvl`. The
final balance will be the sum of the contributions of our five
deposits at various times. The first deposit earns interest for
five years: `fvl(5.4%, 5, 1000) = 1300.78`. The second
deposit only earns interest for four years: `fvl(5.4%, 4, 1000) =
1234.13`. And so on down to the last deposit, which earns one
year’s interest: `fvl(5.4%, 1, 1000) = 1054.00`. The sum of
these five values is, sure enough, $5870.73, just as was computed
by `fvb` directly.

What does `fv(5.4%, 5, 1000) = 5569.96` mean? The payments
are now at the ends of the periods. The end of one year is the same
as the beginning of the next, so what this really means is that we’ve
lost the payment at year zero (which contributed $1300.78), but we’re
now counting the payment at year five (which, since it didn’t have
a chance to earn interest, counts as $1000). Indeed, ‘`5569.96 =
5870.73 - 1300.78 + 1000`’ (give or take a bit of roundoff error).

---

Next: [Related Financial Functions](#Related-Financial-Functions), Previous: [Future Value](#Future-Value), Up: [Financial Functions](#Financial-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.6.3 Present Value [¶](#Present-Value-1)

The `b P` (`calc-fin-pv`) [`pv`] command computes
the present value of an investment. Like `fv`, it takes
three arguments: `pv(rate, n, payment)`.
It computes the present value of a series of regular payments.
Suppose you have the chance to make an investment that will
pay $2000 per year over the next four years; as you receive
these payments you can put them in the bank at 9% interest.
You want to know whether it is better to make the investment, or
to keep the money in the bank where it earns 9% interest right
from the start. The calculation `pv(9%, 4, 2000)` gives the
result 6479.44. If your initial investment must be less than this,
say, $6000, then the investment is worthwhile. But if you had to
put up $7000, then it would be better just to leave it in the bank.

Here is the interpretation of the result of `pv`: You are
trying to compare the return from the investment you are
considering, which is `fv(9%, 4, 2000) = 9146.26`, with
the return from leaving the money in the bank, which is
`fvl(9%, 4, x)` where x is the amount of money
you would have to put up in advance. The `pv` function
finds the break-even point, ‘`x = 6479.44`’, at which
`fvl(9%, 4, 6479.44)` is also equal to 9146.26. This is
the largest amount you should be willing to invest.

The `I b P` [`pvb`] command solves the same problem,
but with payments occurring at the beginning of each interval.
It has the same relationship to `fvb` as `pv` has
to `fv`. For example `pvb(9%, 4, 2000) = 7062.59`,
a larger number than `pv` produced because we get to start
earning interest on the return from our investment sooner.

The `H b P` [`pvl`] command computes the present value of
an investment that will pay off in one lump sum at the end of the
period. For example, if we get our $8000 all at the end of the
four years, `pvl(9%, 4, 8000) = 5667.40`. This is much
less than `pv` reported, because we don’t earn any interest
on the return from this investment. Note that `pvl` and
`fvl` are simple inverses: `fvl(9%, 4, 5667.40) = 8000`.

You can give an optional fourth lump-sum argument to `pv`
and `pvb`; this is handled in exactly the same way as the
fourth argument for `fv` and `fvb`.

The `b N` (`calc-fin-npv`) [`npv`] command computes
the net present value of a series of irregular investments.
The first argument is the interest rate. The second argument is
a vector which represents the expected return from the investment
at the end of each interval. For example, if the rate represents
a yearly interest rate, then the vector elements are the return
from the first year, second year, and so on.

Thus, `npv(9%, [2000,2000,2000,2000]) = pv(9%, 4, 2000) = 6479.44`.
Obviously this function is more interesting when the payments are
not all the same!

The `npv` function can actually have two or more arguments.
Multiple arguments are interpreted in the same way as for the
vector statistical functions like `vsum`.
See [Single-Variable Statistics](#Single_002dVariable-Statistics). Basically, if there are several
payment arguments, each either a vector or a plain number, all these
values are collected left-to-right into the complete list of payments.
A numeric prefix argument on the `b N` command says how many
payment values or vectors to take from the stack.

The `I b N` [`npvb`] command computes the net present
value where payments occur at the beginning of each interval
rather than at the end.

---

Next: [Depreciation Functions](#Depreciation-Functions), Previous: [Present Value](#Present-Value), Up: [Financial Functions](#Financial-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.6.4 Related Financial Functions [¶](#Related-Financial-Functions-1)

The functions in this section are basically inverses of the
present value functions with respect to the various arguments.

The `b M` (`calc-fin-pmt`) [`pmt`] command computes
the amount of periodic payment necessary to amortize a loan.
Thus `pmt(rate, n, amount)` equals the
value of payment such that `pv(rate, n,
payment) = amount`.

The `I b M` [`pmtb`] command does the same computation
but using `pvb` instead of `pv`. Like `pv` and
`pvb`, these functions can also take a fourth argument which
represents an initial lump-sum investment.

The `H b M` key just invokes the `fvl` function, which is
the inverse of `pvl`. There is no explicit `pmtl` function.

The `b #` (`calc-fin-nper`) [`nper`] command computes
the number of regular payments necessary to amortize a loan.
Thus `nper(rate, payment, amount)` equals
the value of n such that `pv(rate, n,
payment) = amount`. If payment is too small
ever to amortize a loan for amount at interest rate rate,
the `nper` function is left in symbolic form.

The `I b #` [`nperb`] command does the same computation
but using `pvb` instead of `pv`. You can give a fourth
lump-sum argument to these functions, but the computation will be
rather slow in the four-argument case.

The `H b #` [`nperl`] command does the same computation
using `pvl`. By exchanging payment and amount you
can also get the solution for `fvl`. For example,
`nperl(8%, 2000, 1000) = 9.006`, so if you place $1000 in a
bank account earning 8%, it will take nine years to grow to $2000.

The `b T` (`calc-fin-rate`) [`rate`] command computes
the rate of return on an investment. This is also an inverse of `pv`:
`rate(n, payment, amount)` computes the value of
rate such that `pv(rate, n, payment) =
amount`. The result is expressed as a formula like ‘`6.3%`’.

The `I b T` [`rateb`] and `H b T` [`ratel`]
commands solve the analogous equations with `pvb` or `pvl`
in place of `pv`. Also, `rate` and `rateb` can
accept an optional fourth argument just like `pv` and `pvb`.
To redo the above example from a different perspective,
`ratel(9, 2000, 1000) = 8.00597%`, which says you will need an
interest rate of 8% in order to double your account in nine years.

The `b I` (`calc-fin-irr`) [`irr`] command is the
analogous function to `rate` but for net present value.
Its argument is a vector of payments. Thus `irr(payments)`
computes the rate such that `npv(rate, payments) = 0`;
this rate is known as the *internal rate of return*.

The `I b I` [`irrb`] command computes the internal rate of
return assuming payments occur at the beginning of each period.

---

Next: [Definitions](#Definitions-of-Financial-Functions), Previous: [Related Financial Functions](#Related-Financial-Functions), Up: [Financial Functions](#Financial-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.6.5 Depreciation Functions [¶](#Depreciation-Functions-1)

The functions in this section calculate *depreciation*, which is
the amount of value that a possession loses over time. These functions
are characterized by three parameters: cost, the original cost
of the asset; salvage, the value the asset will have at the end
of its expected “useful life”; and life, the number of years
(or other periods) of the expected useful life.

There are several methods for calculating depreciation that differ in
the way they spread the depreciation over the lifetime of the asset.

The `b S` (`calc-fin-sln`) [`sln`] command computes the
“straight-line” depreciation. In this method, the asset depreciates
by the same amount every year (or period). For example,
‘`sln(12000, 2000, 5)`’ returns 2000. The asset costs $12000
initially and will be worth $2000 after five years; it loses $2000
per year.

The `b Y` (`calc-fin-syd`) [`syd`] command computes the
accelerated “sum-of-years’-digits” depreciation. Here the depreciation
is higher during the early years of the asset’s life. Since the
depreciation is different each year, `b Y` takes a fourth period
parameter which specifies which year is requested, from 1 to life.
If period is outside this range, the `syd` function will
return zero.

The `b D` (`calc-fin-ddb`) [`ddb`] command computes an
accelerated depreciation using the double-declining balance method.
It also takes a fourth period parameter.

For symmetry, the `sln` function will accept a period
parameter as well, although it will ignore its value except that the
return value will as usual be zero if period is out of range.

For example, pushing the vector ‘`[1,2,3,4,5]`’ (perhaps with `v x 5`)
and then mapping `V M ' [sln(12000,2000,5,$), syd(12000,2000,5,$),
ddb(12000,2000,5,$)] RET` produces a matrix that allows us to compare
the three depreciation methods:

```
[ [ 2000, 3333, 4800 ]
  [ 2000, 2667, 2880 ]
  [ 2000, 2000, 1728 ]
  [ 2000, 1333,  592 ]
  [ 2000,  667,   0  ] ]
```

(Values have been rounded to nearest integers in this figure.)
We see that `sln` depreciates by the same amount each year,
`syd` depreciates more at the beginning and less at the end,
and `ddb` weights the depreciation even more toward the beginning.

Summing columns with `V R : +` yields ‘`[10000, 10000, 10000]`’;
the total depreciation in any method is (by definition) the
difference between the cost and the salvage value.

---

Previous: [Depreciation Functions](#Depreciation-Functions), Up: [Financial Functions](#Financial-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 7.6.6 Definitions [¶](#Definitions)

For your reference, here are the actual formulas used to compute
Calc’s financial functions.

Calc will not evaluate a financial function unless the rate or
n argument is known. However, payment or amount can
be a variable. Calc expands these functions according to the
formulas below for symbolic arguments only when you use the `a "`
(`calc-expand-formula`) command, or when taking derivatives or
integrals or solving equations involving the functions.

These formulas are shown using the conventions of Big display
mode (`d B`); for example, the formula for `fv` written
linearly is ‘`pmt * ((1 + rate)^n) - 1) / rate`’.

```
                                        n
                              (1 + rate)  - 1
fv(rate, n, pmt) =      pmt * ---------------
                                   rate

                                         n
                              ((1 + rate)  - 1) (1 + rate)
fvb(rate, n, pmt) =     pmt * ----------------------------
                                         rate

                                        n
fvl(rate, n, pmt) =     pmt * (1 + rate)

                                            -n
                              1 - (1 + rate)
pv(rate, n, pmt) =      pmt * ----------------
                                    rate

                                             -n
                              (1 - (1 + rate)  ) (1 + rate)
pvb(rate, n, pmt) =     pmt * -----------------------------
                                         rate

                                        -n
pvl(rate, n, pmt) =     pmt * (1 + rate)

                                    -1               -2               -3
npv(rate, [a, b, c]) =  a*(1 + rate)   + b*(1 + rate)   + c*(1 + rate)

                                        -1               -2
npvb(rate, [a, b, c]) = a + b*(1 + rate)   + c*(1 + rate)

                                             -n
                        (amt - x * (1 + rate)  ) * rate
pmt(rate, n, amt, x) =  -------------------------------
                                             -n
                               1 - (1 + rate)

                                             -n
                        (amt - x * (1 + rate)  ) * rate
pmtb(rate, n, amt, x) = -------------------------------
                                        -n
                         (1 - (1 + rate)  ) (1 + rate)

                                   amt * rate
nper(rate, pmt, amt) =  - log(1 - ------------, 1 + rate)
                                      pmt

                                    amt * rate
nperb(rate, pmt, amt) = - log(1 - ---------------, 1 + rate)
                                  pmt * (1 + rate)

                              amt
nperl(rate, pmt, amt) = - log(---, 1 + rate)
                              pmt

                           1/n
                        pmt
ratel(n, pmt, amt) =    ------ - 1
                           1/n
                        amt

                        cost - salv
sln(cost, salv, life) = -----------
                           life

                             (cost - salv) * (life - per + 1)
syd(cost, salv, life, per) = --------------------------------
                                  life * (life + 1) / 2

                             book * 2
ddb(cost, salv, life, per) = --------,  book = cost - depreciation so far
                               life
```

In `pmt` and `pmtb`, ‘`x=0`’ if omitted.

These functions accept any numeric objects, including error forms,
intervals, and even (though not very usefully) complex numbers. The
above formulas specify exactly the behavior of these functions with
all sorts of inputs.

Note that if the first argument to the `log` in `nper` is
negative, `nper` leaves itself in symbolic form rather than
returning a (financially meaningless) complex number.

‘`rate(num, pmt, amt)`’ solves the equation
‘`pv(rate, num, pmt) = amt`’ for ‘`rate`’ using `H a R`
(`calc-find-root`), with the interval ‘`[.01% .. 100%]`’
for an initial guess. The `rateb` function is the same except
that it uses `pvb`. Note that `ratel` can be solved
directly; its formula is shown in the above list.

Similarly, ‘`irr(pmts)`’ solves the equation ‘`npv(rate, pmts) = 0`’
for ‘`rate`’.

If you give a fourth argument to `nper` or `nperb`, Calc
will also use `H a R` to solve the equation using an initial
guess interval of ‘`[0 .. 100]`’.

A fourth argument to `fv` simply sums the two components
calculated from the above formulas for `fv` and `fvl`.
The same is true of `fvb`, `pv`, and `pvb`.

The `ddb` function is computed iteratively; the “book” value
starts out equal to cost, and decreases according to the above
formula for the specified number of periods. If the book value
would decrease below salvage, it only decreases to salvage
and the depreciation is zero for all subsequent periods. The `ddb`
function returns the amount the book value decreased in the specified
period.

---

Previous: [Financial Functions](#Financial-Functions), Up: [Arithmetic Functions](#Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
