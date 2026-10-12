### 10.2 Algebraic Manipulation [¶](#Algebraic-Manipulation-1)

The commands in this section perform general-purpose algebraic
manipulations. They work on the whole formula at the top of the
stack (unless, of course, you have made a selection in that
formula).

Many algebra commands prompt for a variable name or formula. If you
answer the prompt with a blank line, the variable or formula is taken
from top-of-stack, and the normal argument for the command is taken
from the second-to-top stack level.

The `a v` (`calc-alg-evaluate`) command performs the normal
default simplifications on a formula; for example, ‘`a - -b`’ is
changed to ‘`a + b`’. These simplifications are normally done
automatically on all Calc results, so this command is useful only if
you have turned default simplifications off with an `m O`
command. See [Simplification Modes](#Simplification-Modes).

It is often more convenient to type `=`, which is like `a v`
but which also substitutes stored values for variables in the formula.
Use `a v` if you want the variables to ignore their stored values.

If you give a numeric prefix argument of 2 to `a v`, it simplifies
using Calc’s algebraic simplifications; see [Simplifying Formulas](#Simplifying-Formulas).
If you give a numeric prefix of 3 or more, it uses Extended
Simplification mode (`a e`).

If you give a negative prefix argument *-1*, *-2*, or *-3*,
it simplifies in the corresponding mode but only works on the top-level
function call of the formula. For example, ‘`(2 + 3) * (2 + 3)`’ will
simplify to ‘`(2 + 3)^2`’, without simplifying the sub-formulas
‘`2 + 3`’. As another example, typing `V R +` to sum the vector
‘`[1, 2, 3, 4]`’ produces the formula ‘`reduce(add, [1, 2, 3, 4])`’
in No-Simplify mode. Using `a v` will evaluate this all the way to
10; using `C-u - a v` will evaluate it only to ‘`1 + 2 + 3 + 4`’.
(See [Reducing and Mapping Vectors](#Reducing-and-Mapping).)

The `=` command corresponds to the `evalv` function, and
the related `N` command, which is like `=` but temporarily
disables Symbolic mode (`m s`) during the evaluation, corresponds
to the `evalvn` function. (These commands interpret their prefix
arguments differently than `a v`; `=` treats the prefix as
the number of stack elements to evaluate at once, and `N` treats
it as a temporary different working precision.)

The `evalvn` function can take an alternate working precision
as an optional second argument. This argument can be either an
integer, to set the precision absolutely, or a vector containing
a single integer, to adjust the precision relative to the current
precision. Note that `evalvn` with a larger than current
precision will do the calculation at this higher precision, but the
result will as usual be rounded back down to the current precision
afterward. For example, ‘`evalvn(pi - 3.1415)`’ at a precision
of 12 will return ‘`9.265359e-5`’; ‘`evalvn(pi - 3.1415, 30)`’
will return ‘`9.26535897932e-5`’ (computing a 25-digit result which
is then rounded down to 12); and ‘`evalvn(pi - 3.1415, [-2])`’
will return ‘`9.2654e-5`’.

The `a "` (`calc-expand-formula`) command expands functions
into their defining formulas wherever possible. For example,
‘`deg(x^2)`’ is changed to ‘`180 x^2 / pi`’. Most functions,
like `sin` and `gcd`, are not defined by simple formulas
and so are unaffected by this command. One important class of
functions which *can* be expanded is the user-defined functions
created by the `Z F` command. See [Programming with Formulas](#Algebraic-Definitions).
Other functions which `a "` can expand include the probability
distribution functions, most of the financial functions, and the
hyperbolic and inverse hyperbolic functions. A numeric prefix argument
affects `a "` in the same way as it does `a v`: A positive
argument expands all functions in the formula and then simplifies in
various ways; a negative argument expands and simplifies only the
top-level function call.

The `a M` (`calc-map-equation`) [`mapeq`] command applies
a given function or operator to one or more equations. It is analogous
to `V M`, which operates on vectors instead of equations.
see [Reducing and Mapping Vectors](#Reducing-and-Mapping). For example, `a M S` changes
‘`x = y+1`’ to ‘`sin(x) = sin(y+1)`’, and `a M +` with
‘`x = y+1`’ and ‘`6`’ on the stack produces ‘`x+6 = y+7`’.
With two equations on the stack, `a M +` would add the lefthand
sides together and the righthand sides together to get the two
respective sides of a new equation.

Mapping also works on inequalities. Mapping two similar inequalities
produces another inequality of the same type. Mapping an inequality
with an equation produces an inequality of the same type. Mapping a
‘`<=`’ with a ‘`<`’ or ‘`!=`’ (not-equal) produces a ‘`<`’.
If inequalities with opposite direction (e.g., ‘`<`’ and ‘`>`’)
are mapped, the direction of the second inequality is reversed to
match the first: Using `a M +` on ‘`a < b`’ and ‘`a > 2`’
reverses the latter to get ‘`2 < a`’, which then allows the
combination ‘`a + 2 < b + a`’, which the algebraic simplifications
can reduce to ‘`2 < b`’.

Using `a M *`, `a M /`, `a M n`, or `a M &` to negate
or invert an inequality will reverse the direction of the inequality.
Other adjustments to inequalities are *not* done automatically;
`a M S` will change ‘`x < y`’ to ‘`sin(x) < sin(y)`’ even
though this is not true for all values of the variables.

With the Hyperbolic flag, `H a M` [`mapeqp`] does a plain
mapping operation without reversing the direction of any inequalities.
Thus, `H a M &` would change `x > 2` to `1/x > 0.5`.
(This change is mathematically incorrect, but perhaps you were
fixing an inequality which was already incorrect.)

With the Inverse flag, `I a M` [`mapeqr`] always reverses
the direction of the inequality. You might use `I a M C` to
change ‘`x < y`’ to ‘`cos(x) > cos(y)`’ if you know you are
working with small positive angles.

The `a b` (`calc-substitute`) [`subst`] command substitutes
all occurrences
of some variable or sub-expression of an expression with a new
sub-expression. For example, substituting ‘`sin(x)`’ with ‘`cos(y)`’
in ‘`2 sin(x)^2 + x sin(x) + sin(2 x)`’ produces
‘`2 cos(y)^2 + x cos(y) + sin(2 x)`’.
Note that this is a purely structural substitution; the lone ‘`x`’ and
the ‘`sin(2 x)`’ stayed the same because they did not look like
‘`sin(x)`’. See [Rewrite Rules](#Rewrite-Rules), for a more general method for
doing substitutions.

The `a b` command normally prompts for two formulas, the old
one and the new one. If you enter a blank line for the first
prompt, all three arguments are taken from the stack (new, then old,
then target expression). If you type an old formula but then enter a
blank line for the new one, the new formula is taken from top-of-stack
and the target from second-to-top. If you answer both prompts, the
target is taken from top-of-stack as usual.

Note that `a b` has no understanding of commutativity or
associativity. The pattern ‘`x+y`’ will not match the formula
‘`y+x`’. Also, ‘`y+z`’ will not match inside the formula ‘`x+y+z`’
because the ‘`+`’ operator is left-associative, so the “deep
structure” of that formula is ‘`(x+y) + z`’. Use `d U`
(`calc-unformatted-language`) mode to see the true structure of
a formula. The rewrite rule mechanism, discussed later, does not have
these limitations.

As an algebraic function, `subst` takes three arguments:
Target expression, old, new. Note that `subst` is always
evaluated immediately, even if its arguments are variables, so if
you wish to put a call to `subst` onto the stack you must
turn the default simplifications off first (with `m O`).

---

Next: [Polynomials](#Polynomials), Previous: [Algebraic Manipulation](#Algebraic-Manipulation), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 10.3 Simplifying Formulas [¶](#Simplifying-Formulas-1)

The sections below describe all the various kinds of
simplifications Calc provides in full detail. None of Calc’s
simplification commands are designed to pull rabbits out of hats;
they simply apply certain specific rules to put formulas into
less redundant or more pleasing forms. Serious algebra in Calc
must be done manually, usually with a combination of selections
and rewrite rules. See [Rearranging Formulas using Selections](#Rearranging-with-Selections).
See [Rewrite Rules](#Rewrite-Rules).

See [Simplification Modes](#Simplification-Modes), for commands to control what level of
simplification occurs automatically. Normally the algebraic
simplifications described below occur. If you have turned on a
simplification mode which does not do these algebraic simplifications,
you can still apply them to a formula with the `a s`
(`calc-simplify`) [`simplify`] command.

There are some simplifications that, while sometimes useful, are never
done automatically. For example, the `I` prefix can be given to
`a s`; the `I a s` command will change any trigonometric
function to the appropriate combination of ‘`sin`’s and ‘`cos`’s
before simplifying. This can be useful in simplifying even mildly
complicated trigonometric expressions. For example, while the algebraic
simplifications can reduce ‘`sin(x) csc(x)`’ to ‘`1`’, they will not
simplify ‘`sin(x)^2 csc(x)`’. The command `I a s` can be used to
simplify this latter expression; it will transform ‘`sin(x)^2
csc(x)`’ into ‘`sin(x)`’. However, `I a s` will also perform
some “simplifications” which may not be desired; for example, it
will transform ‘`tan(x)^2`’ into ‘`sin(x)^2 / cos(x)^2`’. The
Hyperbolic prefix `H` can be used similarly; the `H a s` will
replace any hyperbolic functions in the formula with the appropriate
combinations of ‘`sinh`’s and ‘`cosh`’s before simplifying.

* [Basic Simplifications](#Basic-Simplifications)
* [Algebraic Simplifications](#Algebraic-Simplifications)
* [“Unsafe” Simplifications](#Unsafe-Simplifications)
* [Simplification of Units](#Simplification-of-Units)

---

Next: [Algebraic Simplifications](#Algebraic-Simplifications), Up: [Simplifying Formulas](#Simplifying-Formulas)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.3.1 Basic Simplifications [¶](#Basic-Simplifications-1)

This section describes basic simplifications which Calc performs in many
situations. For example, both binary simplifications and algebraic
simplifications begin by performing these basic simplifications. You
can type `m I` to restrict the simplifications done on the stack to
these simplifications.

The most basic simplification is the evaluation of functions.
For example, ‘`2 + 3`’ is evaluated to ‘`5`’, and ‘`sqrt(9)`’
is evaluated to ‘`3`’. Evaluation does not occur if the arguments
to a function are somehow of the wrong type ‘`tan([2,3,4])`’),
range (‘`tan(90)`’), or number (‘`tan(3,5)`’),
or if the function name is not recognized (‘`f(5)`’), or if
Symbolic mode (see [Symbolic Mode](#Symbolic-Mode)) prevents evaluation
(‘`sqrt(2)`’).

Calc simplifies (evaluates) the arguments to a function before it
simplifies the function itself. Thus ‘`sqrt(5+4)`’ is
simplified to ‘`sqrt(9)`’ before the `sqrt` function
itself is applied. There are very few exceptions to this rule:
`quote`, `lambda`, and `condition` (the `::`
operator) do not evaluate their arguments, `if` (the `? :`
operator) does not evaluate all of its arguments, and `evalto`
does not evaluate its lefthand argument.

Most commands apply at least these basic simplifications to all
arguments they take from the stack, perform a particular operation,
then simplify the result before pushing it back on the stack. In the
common special case of regular arithmetic commands like `+` and
`Q` [`sqrt`], the arguments are simply popped from the stack
and collected into a suitable function call, which is then simplified
(the arguments being simplified first as part of the process, as
described above).

Even the basic set of simplifications are too numerous to describe
completely here, but this section will describe the ones that apply to the
major arithmetic operators. This list will be rather technical in
nature, and will probably be interesting to you only if you are
a serious user of Calc’s algebra facilities.

As well as the simplifications described here, if you have stored
any rewrite rules in the variable `EvalRules` then these rules
will also be applied before any of the basic simplifications.
See [Automatic Rewrites](#Automatic-Rewrites), for details.

And now, on with the basic simplifications:

Arithmetic operators like `+` and `*` always take two
arguments in Calc’s internal form. Sums and products of three or
more terms are arranged by the associative law of algebra into
a left-associative form for sums, ‘`((a + b) + c) + d`’, and
(by default) a right-associative form for products,
‘`a * (b * (c * d))`’. Formulas like ‘`(a + b) + (c + d)`’ are
rearranged to left-associative form, though this rarely matters since
Calc’s algebra commands are designed to hide the inner structure of sums
and products as much as possible. Sums and products in their proper
associative form will be written without parentheses in the examples
below.

Sums and products are *not* rearranged according to the
commutative law (‘`a + b`’ to ‘`b + a`’) except in a few
special cases described below. Some algebra programs always
rearrange terms into a canonical order, which enables them to
see that ‘`a b + b a`’ can be simplified to ‘`2 a b`’.
If you are using Basic Simplification mode, Calc assumes you have put
the terms into the order you want and generally leaves that order alone,
with the consequence that formulas like the above will only be
simplified if you explicitly give the `a s` command.
See [Algebraic Simplifications](#Algebraic-Simplifications).

Differences ‘`a - b`’ are treated like sums ‘`a + (-b)`’
for purposes of simplification; one of the default simplifications
is to rewrite ‘`a + (-b)`’ or ‘`(-b) + a`’, where ‘`-b`’
represents a “negative-looking” term, into ‘`a - b`’ form.
“Negative-looking” means negative numbers, negated formulas like
‘`-x`’, and products or quotients in which either term is
negative-looking.

Other simplifications involving negation are ‘`-(-x)`’ to ‘`x`’;
‘`-(a b)`’ or ‘`-(a/b)`’ where either ‘`a`’ or ‘`b`’ is
negative-looking, simplified by negating that term, or else where
‘`a`’ or ‘`b`’ is any number, by negating that number;
‘`-(a + b)`’ to ‘`-a - b`’, and ‘`-(b - a)`’ to ‘`a - b`’.
(This, and rewriting ‘`(-b) + a`’ to ‘`a - b`’, are the only
cases where the order of terms in a sum is changed by the default
simplifications.)

The distributive law is used to simplify sums in some cases:
‘`a x + b x`’ to ‘`(a + b) x`’, where ‘`a`’ represents
a number or an implicit 1 or *-1* (as in ‘`x`’ or ‘`-x`’)
and similarly for ‘`b`’. Use the `a c`, `a f`, or
`j M` commands to merge sums with non-numeric coefficients
using the distributive law.

The distributive law is only used for sums of two terms, or
for adjacent terms in a larger sum. Thus ‘`a + b + b + c`’
is simplified to ‘`a + 2 b + c`’, but ‘`a + b + c + b`’
is not simplified. The reason is that comparing all terms of a
sum with one another would require time proportional to the
square of the number of terms; Calc omits potentially slow
operations like this in basic simplification mode.

Finally, ‘`a + 0`’ and ‘`0 + a`’ are simplified to ‘`a`’.
A consequence of the above rules is that ‘`0 - a`’ is simplified
to ‘`-a`’.

The products ‘`1 a`’ and ‘`a 1`’ are simplified to ‘`a`’;
‘`(-1) a`’ and ‘`a (-1)`’ are simplified to ‘`-a`’;
‘`0 a`’ and ‘`a 0`’ are simplified to ‘`0`’, except that
in Matrix mode where ‘`a`’ is not provably scalar the result
is the generic zero matrix ‘`idn(0)`’, and that if ‘`a`’ is
infinite the result is ‘`nan`’.

Also, ‘`(-a) b`’ and ‘`a (-b)`’ are simplified to ‘`-(a b)`’,
where this occurs for negated formulas but not for regular negative
numbers.

Products are commuted only to move numbers to the front:
‘`a b 2`’ is commuted to ‘`2 a b`’.

The product ‘`a (b + c)`’ is distributed over the sum only if
‘`a`’ and at least one of ‘`b`’ and ‘`c`’ are numbers:
‘`2 (x + 3)`’ goes to ‘`2 x + 6`’. The formula
‘`(-a) (b - c)`’, where ‘`-a`’ is a negative number, is
rewritten to ‘`a (c - b)`’.

The distributive law of products and powers is used for adjacent
terms of the product: ‘`x^a x^b`’ goes to
‘`x^(a+b)`’
where ‘`a`’ is a number, or an implicit 1 (as in ‘`x`’),
or the implicit one-half of ‘`sqrt(x)`’, and similarly for
‘`b`’. The result is written using ‘`sqrt`’ or ‘`1/sqrt`’
if the sum of the powers is ‘`1/2`’ or ‘`-1/2`’, respectively.
If the sum of the powers is zero, the product is simplified to
‘`1`’ or to ‘`idn(1)`’ if Matrix mode is enabled.

The product of a negative power times anything but another negative
power is changed to use division:
‘`x^(-2) y`’
goes to ‘`y / x^2`’ unless Matrix mode is
in effect and neither ‘`x`’ nor ‘`y`’ are scalar (in which
case it is considered unsafe to rearrange the order of the terms).

Finally, ‘`a (b/c)`’ is rewritten to ‘`(a b)/c`’, and also
‘`(a/b) c`’ is changed to ‘`(a c)/b`’ unless in Matrix mode.

Simplifications for quotients are analogous to those for products.
The quotient ‘`0 / x`’ is simplified to ‘`0`’, with the same
exceptions that were noted for ‘`0 x`’. Likewise, ‘`x / 1`’
and ‘`x / (-1)`’ are simplified to ‘`x`’ and ‘`-x`’,
respectively.

The quotient ‘`x / 0`’ is left unsimplified or changed to an
infinite quantity, as directed by the current infinite mode.
See [Infinite Mode](#Infinite-Mode).

The expression
‘`a / b^(-c)`’
is changed to ‘`a b^c`’, where ‘`-c`’ is any negative-looking
power. Also, ‘`1 / b^c`’ is changed to
‘`b^(-c)`’
for any power ‘`c`’.

Also, ‘`(-a) / b`’ and ‘`a / (-b)`’ go to ‘`-(a/b)`’;
‘`(a/b) / c`’ goes to ‘`a / (b c)`’; and ‘`a / (b/c)`’
goes to ‘`(a c) / b`’ unless Matrix mode prevents this
rearrangement. Similarly, ‘`a / (b:c)`’ is simplified to
‘`(c:b) a`’ for any fraction ‘`b:c`’.

The distributive law is applied to ‘`(a + b) / c`’ only if
‘`c`’ and at least one of ‘`a`’ and ‘`b`’ are numbers.
Quotients of powers and square roots are distributed just as
described for multiplication.

Quotients of products cancel only in the leading terms of the
numerator and denominator. In other words, ‘`a x b / a y b`’
is canceled to ‘`x b / y b`’ but not to ‘`x / y`’. Once
again this is because full cancellation can be slow; use `a s`
to cancel all terms of the quotient.

Quotients of negative-looking values are simplified according
to ‘`(-a) / (-b)`’ to ‘`a / b`’, ‘`(-a) / (b - c)`’
to ‘`a / (c - b)`’, and ‘`(a - b) / (-c)`’ to ‘`(b - a) / c`’.

The formula ‘`x^0`’ is simplified to ‘`1`’, or to ‘`idn(1)`’
in Matrix mode. The formula ‘`0^x`’ is simplified to ‘`0`’
unless ‘`x`’ is a negative number, complex number or zero.
If ‘`x`’ is negative, complex or ‘`0.0`’, ‘`0^x`’ is an
infinity or an unsimplified formula according to the current infinite
mode. The expression ‘`0^0`’ is simplified to ‘`1`’.

Powers of products or quotients ‘`(a b)^c`’, ‘`(a/b)^c`’
are distributed to ‘`a^c b^c`’, ‘`a^c / b^c`’ only if ‘`c`’
is an integer, or if either ‘`a`’ or ‘`b`’ are nonnegative
real numbers. Powers of powers ‘`(a^b)^c`’ are simplified to
‘`a^(b c)`’
only when ‘`c`’ is an integer and ‘`b c`’ also
evaluates to an integer. Without these restrictions these simplifications
would not be safe because of problems with principal values.
(In other words,
‘`((-3)^1:2)^2`’
is safe to simplify, but
‘`((-3)^2)^1:2`’
is not.) See [Declarations](#Declarations), for ways to inform Calc that your
variables satisfy these requirements.

As a special case of this rule, ‘`sqrt(x)^n`’ is simplified to
‘`x^(n/2)`’
only for even integers ‘`n`’.

If ‘`a`’ is known to be real, ‘`b`’ is an even integer, and
‘`c`’ is a half- or quarter-integer, then ‘`(a^b)^c`’ is
simplified to ‘`abs(a^(b c))`’.

Also, ‘`(-a)^b`’ is simplified to ‘`a^b`’ if ‘`b`’ is an
even integer, or to ‘`-(a^b)`’ if ‘`b`’ is an odd integer,
for any negative-looking expression ‘`-a`’.

Square roots ‘`sqrt(x)`’ generally act like one-half powers
‘`x^1:2`’
for the purposes of the above-listed simplifications.

Also, note that
‘`1 / x^1:2`’
is changed to
‘`x^(-1:2)`’,
but ‘`1 / sqrt(x)`’ is left alone.

Generic identity matrices (see [Matrix and Scalar Modes](#Matrix-Mode)) are simplified by the
following rules: ‘`idn(a) + b`’ to ‘`a + b`’ if ‘`b`’
is provably scalar, or expanded out if ‘`b`’ is a matrix;
‘`idn(a) + idn(b)`’ to ‘`idn(a + b)`’;
‘`-idn(a)`’ to ‘`idn(-a)`’; ‘`a idn(b)`’ to
‘`idn(a b)`’ if ‘`a`’ is provably scalar, or to ‘`a b`’
if ‘`a`’ is provably non-scalar; ‘`idn(a) idn(b)`’ to
‘`idn(a b)`’; analogous simplifications for quotients involving
`idn`; and ‘`idn(a)^n`’ to ‘`idn(a^n)`’ where
‘`n`’ is an integer.

The `floor` function and other integer truncation functions
vanish if the argument is provably integer-valued, so that
‘`floor(round(x))`’ simplifies to ‘`round(x)`’.
Also, combinations of `float`, `floor` and its friends,
and `ffloor` and its friends, are simplified in appropriate
ways. See [Integer Truncation](#Integer-Truncation).

The expression ‘`abs(-x)`’ changes to ‘`abs(x)`’.
The expression ‘`abs(abs(x))`’ changes to
‘`abs(x)`’; in fact, ‘`abs(x)`’ changes to ‘`x`’ or
‘`-x`’ if ‘`x`’ is provably nonnegative or nonpositive
(see [Declarations](#Declarations)).

While most functions do not recognize the variable `i` as an
imaginary number, the `arg` function does handle the two cases
‘`arg(i)`’ and ‘`arg(-i)`’ just for convenience.

The expression ‘`conj(conj(x))`’ simplifies to ‘`x`’.
Various other expressions involving `conj`, `re`, and
`im` are simplified, especially if some of the arguments are
provably real or involve the constant `i`. For example,
‘`conj(a + b i)`’ is changed to
‘`conj(a) - conj(b) i`’, or to ‘`a - b i`’ if ‘`a`’
and ‘`b`’ are known to be real.

Functions like `sin` and `arctan` generally don’t have
any default simplifications beyond simply evaluating the functions
for suitable numeric arguments and infinity. The algebraic
simplifications described in the next section do provide some
simplifications for these functions, though.

One important simplification that does occur is that
‘`ln(e)`’ is simplified to 1, and ‘`ln(e^x)`’ is
simplified to ‘`x`’ for any ‘`x`’. This occurs even if you have
stored a different value in the Calc variable ‘`e`’; but this would
be a bad idea in any case if you were also using natural logarithms!

Among the logical functions, `!(a <= b)` changes to
`a > b` and so on. Equations and inequalities where both sides
are either negative-looking or zero are simplified by negating both sides
and reversing the inequality. While it might seem reasonable to simplify
‘`!!x`’ to ‘`x`’, this would not be valid in general because
‘`!!2`’ is 1, not 2.

Most other Calc functions have few if any basic simplifications
defined, aside of course from evaluation when the arguments are
suitable numbers.

---

Next: [“Unsafe” Simplifications](#Unsafe-Simplifications), Previous: [Basic Simplifications](#Basic-Simplifications), Up: [Simplifying Formulas](#Simplifying-Formulas)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.3.2 Algebraic Simplifications [¶](#Algebraic-Simplifications-1)

This section describes all simplifications that are performed by
the algebraic simplification mode, which is the default simplification
mode. If you have switched to a different simplification mode, you can
switch back with the `m A` command. Even in other simplification
modes, the `a s` command will use these algebraic simplifications to
simplify the formula.

There is a variable, `AlgSimpRules`, in which you can put rewrites
to be applied. Its use is analogous to `EvalRules`,
but without the special restrictions. Basically, the simplifier does
‘`a r AlgSimpRules`’ with an infinite repeat count on the whole
expression being simplified, then it traverses the expression applying
the built-in rules described below. If the result is different from
the original expression, the process repeats with the basic
simplifications (including `EvalRules`), then `AlgSimpRules`,
then the built-in simplifications, and so on.

Sums are simplified in two ways. Constant terms are commuted to the
end of the sum, so that ‘`a + 2 + b`’ changes to ‘`a + b + 2`’.
The only exception is that a constant will not be commuted away
from the first position of a difference, i.e., ‘`2 - x`’ is not
commuted to ‘`-x + 2`’.

Also, terms of sums are combined by the distributive law, as in
‘`x + y + 2 x`’ to ‘`y + 3 x`’. This always occurs for
adjacent terms, but Calc’s algebraic simplifications compare all pairs
of terms including non-adjacent ones.

Products are sorted into a canonical order using the commutative
law. For example, ‘`b c a`’ is commuted to ‘`a b c`’.
This allows easier comparison of products; for example, the basic
simplifications will not change ‘`x y + y x`’ to ‘`2 x y`’,
but the algebraic simplifications; it first rewrites the sum to
‘`x y + x y`’ which can then be recognized as a sum of identical
terms.

The canonical ordering used to sort terms of products has the
property that real-valued numbers, interval forms and infinities
come first, and are sorted into increasing order. The `V S`
command uses the same ordering when sorting a vector.

Sorting of terms of products is inhibited when Matrix mode is
turned on; in this case, Calc will never exchange the order of
two terms unless it knows at least one of the terms is a scalar.

Products of powers are distributed by comparing all pairs of
terms, using the same method that the default simplifications
use for adjacent terms of products.

Even though sums are not sorted, the commutative law is still
taken into account when terms of a product are being compared.
Thus ‘`(x + y) (y + x)`’ will be simplified to ‘`(x + y)^2`’.
A subtle point is that ‘`(x - y) (y - x)`’ will *not*
be simplified to ‘`-(x - y)^2`’; Calc does not notice that
one term can be written as a constant times the other, even if
that constant is *-1*.

A fraction times any expression, ‘`(a:b) x`’, is changed to
a quotient involving integers: ‘`a x / b`’. This is not
done for floating-point numbers like ‘`0.5`’, however. This
is one reason why you may find it convenient to turn Fraction mode
on while doing algebra; see [Fraction Mode](#Fraction-Mode).

Quotients are simplified by comparing all terms in the numerator
with all terms in the denominator for possible cancellation using
the distributive law. For example, ‘`a x^2 b / c x^3 d`’ will
cancel ‘`x^2`’ from the top and bottom to get ‘`a b / c x d`’.
(The terms in the denominator will then be rearranged to ‘`c d x`’
as described above.) If there is any common integer or fractional
factor in the numerator and denominator, it is canceled out;
for example, ‘`(4 x + 6) / 8 x`’ simplifies to ‘`(2 x + 3) / 4 x`’.

Non-constant common factors are not found even by algebraic
simplifications. To cancel the factor ‘`a`’ in
‘`(a x + a) / a^2`’ you could first use `j M` on the product
‘`a x`’ to Merge the numerator to ‘`a (1+x)`’, which can then be
simplified successfully.

Integer powers of the variable `i` are simplified according
to the identity ‘`i^2 = -1`’. If you store a new value other
than the complex number ‘`(0,1)`’ in `i`, this simplification
will no longer occur. This is not done by the basic
simplifications; in case someone (unwisely) wants to use the name
`i` for a variable unrelated to complex numbers, they can use
basic simplification mode.

Square roots of integer or rational arguments are simplified in
several ways. (Note that these will be left unevaluated only in
Symbolic mode.) First, square integer or rational factors are
pulled out so that ‘`sqrt(8)`’ is rewritten as
‘`2 sqrt(2)`’.
Conceptually speaking this implies factoring the argument into primes
and moving pairs of primes out of the square root, but for reasons of
efficiency Calc only looks for primes up to 29.

Square roots in the denominator of a quotient are moved to the
numerator: ‘`1 / sqrt(3)`’ changes to ‘`sqrt(3) / 3`’.
The same effect occurs for the square root of a fraction:
‘`sqrt(2:3)`’ changes to ‘`sqrt(6) / 3`’.

The `%` (modulo) operator is simplified in several ways
when the modulus ‘`M`’ is a positive real number. First, if
the argument is of the form ‘`x + n`’ for some real number
‘`n`’, then ‘`n`’ is itself reduced modulo ‘`M`’. For
example, ‘`(x - 23) % 10`’ is simplified to ‘`(x + 7) % 10`’.

If the argument is multiplied by a constant, and this constant
has a common integer divisor with the modulus, then this factor is
canceled out. For example, ‘`12 x % 15`’ is changed to
‘`3 (4 x % 5)`’ by factoring out 3. Also, ‘`(12 x + 1) % 15`’
is changed to ‘`3 ((4 x + 1:3) % 5)`’. While these forms may
not seem “simpler,” they allow Calc to discover useful information
about modulo forms in the presence of declarations.

If the modulus is 1, then Calc can use `int` declarations to
evaluate the expression. For example, the idiom ‘`x % 2`’ is
often used to check whether a number is odd or even. As described
above, ‘`2 n % 2`’ and ‘`(2 n + 1) % 2`’ are simplified to
‘`2 (n % 1)`’ and ‘`2 ((n + 1:2) % 1)`’, respectively; Calc
can simplify these to 0 and 1 (respectively) if `n` has been
declared to be an integer.

Trigonometric functions are simplified in several ways. Whenever a
products of two trigonometric functions can be replaced by a single
function, the replacement is made; for example,
‘`tan(x) cos(x)`’ is simplified to ‘`sin(x)`’.
Reciprocals of trigonometric functions are replaced by their reciprocal
function; for example, ‘`1/sec(x)`’ is simplified to
‘`cos(x)`’. The corresponding simplifications for the
hyperbolic functions are also handled.

Trigonometric functions of their inverse functions are
simplified. The expression ‘`sin(arcsin(x))`’ is
simplified to ‘`x`’, and similarly for `cos` and `tan`.
Trigonometric functions of inverses of different trigonometric
functions can also be simplified, as in ‘`sin(arccos(x))`’
to ‘`sqrt(1 - x^2)`’.

If the argument to `sin` is negative-looking, it is simplified to
‘`-sin(x)`’, and similarly for `cos` and `tan`.
Finally, certain special values of the argument are recognized;
see [Trigonometric/Hyperbolic Functions](#Trigonometric-and-Hyperbolic-Functions).

Hyperbolic functions of their inverses and of negative-looking
arguments are also handled, as are exponentials of inverse
hyperbolic functions.

No simplifications for inverse trigonometric and hyperbolic
functions are known, except for negative arguments of `arcsin`,
`arctan`, `arcsinh`, and `arctanh`. Note that
‘`arcsin(sin(x))`’ can *not* safely change to
‘`x`’, since this only correct within an integer multiple of
‘`2 pi`’
radians or 360 degrees. However, ‘`arcsinh(sinh(x))`’ is
simplified to ‘`x`’ if ‘`x`’ is known to be real.

Several simplifications that apply to logarithms and exponentials
are that ‘`exp(ln(x))`’,
‘`e^ln(x)`’,
and
‘`10^log10(x)`’
all reduce to ‘`x`’. Also, ‘`ln(exp(x))`’, etc., can
reduce to ‘`x`’ if ‘`x`’ is provably real. The form
‘`exp(x)^y`’ is simplified to ‘`exp(x y)`’. If ‘`x`’
is a suitable multiple of
‘`pi i`’
(as described above for the trigonometric functions), then
‘`exp(x)`’ or ‘`e^x`’ will be expanded. Finally,
‘`ln(x)`’ is simplified to a form involving `pi` and
`i` where ‘`x`’ is provably negative, positive imaginary, or
negative imaginary.

The error functions `erf` and `erfc` are simplified when
their arguments are negative-looking or are calls to the `conj`
function.

Equations and inequalities are simplified by canceling factors
of products, quotients, or sums on both sides. Inequalities
change sign if a negative multiplicative factor is canceled.
Non-constant multiplicative factors as in ‘`a b = a c`’ are
canceled from equations only if they are provably nonzero (generally
because they were declared so; see [Declarations](#Declarations)). Factors
are canceled from inequalities only if they are nonzero and their
sign is known.

Simplification also replaces an equation or inequality with
1 or 0 (“true” or “false”) if it can through the use of
declarations. If ‘`x`’ is declared to be an integer greater
than 5, then ‘`x < 3`’, ‘`x = 3`’, and ‘`x = 7.5`’ are
all simplified to 0, but ‘`x > 3`’ is simplified to 1.
By a similar analysis, ‘`abs(x) >= 0`’ is simplified to 1,
as is ‘`x^2 >= 0`’ if ‘`x`’ is known to be real.

---

Next: [Simplification of Units](#Simplification-of-Units), Previous: [Algebraic Simplifications](#Algebraic-Simplifications), Up: [Simplifying Formulas](#Simplifying-Formulas)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.3.3 “Unsafe” Simplifications [¶](#g_t_0060_0060Unsafe_0027_0027-Simplifications)

Calc is capable of performing some simplifications which may sometimes
be desired but which are not “safe” in all cases. The `a e`
(`calc-simplify-extended`) [`esimplify`] command
applies the algebraic simplifications as well as these extended, or
“unsafe”, simplifications. Use this only if you know the values in
your formula lie in the restricted ranges for which these
simplifications are valid. You can use Extended Simplification mode
(`m E`) to have these simplifications done automatically.

The symbolic integrator uses these extended simplifications; one effect
of this is that the integrator’s results must be used with caution.
Where an integral table will often attach conditions like “for positive
‘`a`’ only,” Calc (like most other symbolic integration programs)
will simply produce an unqualified result.

Because `a e`’s simplifications are unsafe, it is sometimes better
to type `C-u -3 a v`, which does extended simplification only
on the top level of the formula without affecting the sub-formulas.
In fact, `C-u -3 j v` allows you to target extended simplification
to any specific part of a formula.

The variable `ExtSimpRules` contains rewrites to be applied when
the extended simplifications are used. These are applied in addition to
`EvalRules` and `AlgSimpRules`. (The `a r AlgSimpRules`
step described above is simply followed by an `a r ExtSimpRules` step.)

Following is a complete list of the “unsafe” simplifications.

Inverse trigonometric or hyperbolic functions, called with their
corresponding non-inverse functions as arguments, are simplified.
For example, ‘`arcsin(sin(x))`’ changes
to ‘`x`’. Also, ‘`arcsin(cos(x))`’ and
‘`arccos(sin(x))`’ both change to ‘`pi/2 - x`’.
These simplifications are unsafe because they are valid only for
values of ‘`x`’ in a certain range; outside that range, values
are folded down to the 360-degree range that the inverse trigonometric
functions always produce.

Powers of powers ‘`(x^a)^b`’ are simplified to
‘`x^(a b)`’
for all ‘`a`’ and ‘`b`’. These results will be valid only
in a restricted range of ‘`x`’; for example, in
‘`(x^2)^1:2`’
the powers cancel to get ‘`x`’, which is valid for positive values
of ‘`x`’ but not for negative or complex values.

Similarly, ‘`sqrt(x^a)`’ and ‘`sqrt(x)^a`’ are both
simplified (possibly unsafely) to
‘`x^(a/2)`’.

Forms like ‘`sqrt(1 - sin(x)^2)`’ are simplified to, e.g.,
‘`cos(x)`’. Calc has identities of this sort for `sin`,
`cos`, `tan`, `sinh`, and `cosh`.

Arguments of square roots are partially factored to look for
squared terms that can be extracted. For example,
‘`sqrt(a^2 b^3 + a^3 b^2)`’ simplifies to
‘`a b sqrt(a+b)`’.

The simplifications of ‘`ln(exp(x))`’,
‘`ln(e^x)`’, and ‘`log10(10^x)`’ to ‘`x`’ are also
unsafe because of problems with principal values (although these
simplifications are safe if ‘`x`’ is known to be real).

Common factors are canceled from products on both sides of an
equation, even if those factors may be zero: ‘`a x / b x`’
to ‘`a / b`’. Such factors are never canceled from
inequalities: Even the extended simplifications are not bold enough to
reduce ‘`a x < b x`’ to ‘`a < b`’ (or ‘`a > b`’, depending
on whether you believe ‘`x`’ is positive or negative).
The `a M /` command can be used to divide a factor out of
both sides of an inequality.

---

Previous: [“Unsafe” Simplifications](#Unsafe-Simplifications), Up: [Simplifying Formulas](#Simplifying-Formulas)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.3.4 Simplification of Units [¶](#Simplification-of-Units-1)

The simplifications described in this section (as well as the algebraic
simplifications) are applied when units need to be simplified. They can
be applied using the `u s` (`calc-simplify-units`) command, or
will be done automatically in Units Simplification mode (`m U`).
See [Basic Operations on Units](#Basic-Operations-on-Units).

The variable `UnitSimpRules` contains rewrites to be applied by
units simplifications. These are applied in addition to `EvalRules`
and `AlgSimpRules`.

Scalar mode is automatically put into effect when simplifying units.
See [Matrix and Scalar Modes](#Matrix-Mode).

Sums ‘`a + b`’ involving units are simplified by extracting the
units of ‘`a`’ as if by the `u x` command (call the result
‘`u_a`’), then simplifying the expression ‘`b / u_a`’
using `u b` and `u s`. If the result has units then the sum
is inconsistent and is left alone. Otherwise, it is rewritten
in terms of the units ‘`u_a`’.

If units auto-ranging mode is enabled, products or quotients in
which the first argument is a number which is out of range for the
leading unit are modified accordingly.

When canceling and combining units in products and quotients,
Calc accounts for unit names that differ only in the prefix letter.
For example, ‘`2 km m`’ is simplified to ‘`2000 m^2`’.
However, compatible but different units like `ft` and `in`
are not combined in this way.

Quotients ‘`a / b`’ are simplified in three additional ways. First,
if ‘`b`’ is a number or a product beginning with a number, Calc
computes the reciprocal of this number and moves it to the numerator.

Second, for each pair of unit names from the numerator and denominator
of a quotient, if the units are compatible (e.g., they are both
units of area) then they are replaced by the ratio between those
units. For example, in ‘`3 s in N / kg cm`’ the units
‘`in / cm`’ will be replaced by ‘`2.54`’.

Third, if the units in the quotient exactly cancel out, so that
a `u b` command on the quotient would produce a dimensionless
number for an answer, then the quotient simplifies to that number.

For powers and square roots, the “unsafe” simplifications
‘`(a b)^c`’ to ‘`a^c b^c`’, ‘`(a/b)^c`’ to ‘`a^c / b^c`’,
and ‘`(a^b)^c`’ to
‘`a^(b c)`’
are done if the powers are real numbers. (These are safe in the context
of units because all numbers involved can reasonably be assumed to be
real.)

Also, if a unit name is raised to a fractional power, and the
base units in that unit name all occur to powers which are a
multiple of the denominator of the power, then the unit name
is expanded out into its base units, which can then be simplified
according to the previous paragraph. For example, ‘`acre^1.5`’
is simplified by noting that ‘`1.5 = 3:2`’, that ‘`acre`’
is defined in terms of ‘`m^2`’, and that the 2 in the power of
`m` is a multiple of 2 in ‘`3:2`’. Thus, `acre^1.5` is
replaced by approximately
‘`(4046 m^2)^1.5`’,
which is then changed to
‘`4046^1.5 (m^2)^1.5`’,
then to ‘`257440 m^3`’.

The functions `float`, `frac`, `clean`, `abs`,
as well as `floor` and the other integer truncation functions,
applied to unit names or products or quotients involving units, are
simplified. For example, ‘`round(1.6 in)`’ is changed to
‘`round(1.6) round(in)`’; the lefthand term evaluates to 2,
and the righthand term simplifies to `in`.

The functions `sin`, `cos`, and `tan` with arguments
that have angular units like `rad` or `arcmin` are
simplified by converting to base units (radians), then evaluating
with the angular mode temporarily set to radians.

---

Next: [Calculus](#Calculus), Previous: [Simplifying Formulas](#Simplifying-Formulas), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 10.4 Polynomials [¶](#Polynomials-1)

A *polynomial* is a sum of terms which are coefficients times
various powers of a “base” variable. For example, ‘`2 x^2 + 3 x - 4`’
is a polynomial in ‘`x`’. Some formulas can be considered
polynomials in several different variables: ‘`1 + 2 x + 3 y + 4 x y^2`’
is a polynomial in both ‘`x`’ and ‘`y`’. Polynomial coefficients
are often numbers, but they may in general be any formulas not
involving the base variable.

The `a f` (`calc-factor`) [`factor`] command factors a
polynomial into a product of terms. For example, the polynomial
‘`x^3 + 2 x^2 + x`’ is factored into ‘`x*(x+1)^2`’. As another
example, ‘`a c + b d + b c + a d`’ is factored into the product
‘`(a + b) (c + d)`’.

Calc currently has three algorithms for factoring. Formulas which are
linear in several variables, such as the second example above, are
merged according to the distributive law. Formulas which are
polynomials in a single variable, with constant integer or fractional
coefficients, are factored into irreducible linear and/or quadratic
terms. The first example above factors into three linear terms
(‘`x`’, ‘`x+1`’, and ‘`x+1`’ again). Finally, formulas
which do not fit the above criteria are handled by the algebraic
rewrite mechanism.

Calc’s polynomial factorization algorithm works by using the general
root-finding command (`a P`) to solve for the roots of the
polynomial. It then looks for roots which are rational numbers
or complex-conjugate pairs, and converts these into linear and
quadratic terms, respectively. Because it uses floating-point
arithmetic, it may be unable to find terms that involve large
integers (whose number of digits approaches the current precision).
Also, irreducible factors of degree higher than quadratic are not
found, and polynomials in more than one variable are not treated.
(A more robust factorization algorithm may be included in a future
version of Calc.)

The rewrite-based factorization method uses rules stored in the variable
`FactorRules`. See [Rewrite Rules](#Rewrite-Rules), for a discussion of the
operation of rewrite rules. The default `FactorRules` are able
to factor quadratic forms symbolically into two linear terms,
‘`(a x + b) (c x + d)`’. You can edit these rules to include other
cases if you wish. To use the rules, Calc builds the formula
‘`thecoefs(x, [a, b, c, ...])`’ where `x` is the polynomial
base variable and `a`, `b`, etc., are polynomial coefficients
(which may be numbers or formulas). The constant term is written first,
i.e., in the `a` position. When the rules complete, they should have
changed the formula into the form ‘`thefactors(x, [f1, f2, f3, ...])`’
where each `fi` should be a factored term, e.g., ‘`x - ai`’.
Calc then multiplies these terms together to get the complete
factored form of the polynomial. If the rules do not change the
`thecoefs` call to a `thefactors` call, `a f` leaves the
polynomial alone on the assumption that it is unfactorable. (Note that
the function names `thecoefs` and `thefactors` are used only
as placeholders; there are no actual Calc functions by those names.)

The `H a f` [`factors`] command also factors a polynomial,
but it returns a list of factors instead of an expression which is the
product of the factors. Each factor is represented by a sub-vector
of the factor, and the power with which it appears. For example,
‘`x^5 + x^4 - 33 x^3 + 63 x^2`’ factors to ‘`(x + 7) x^2 (x - 3)^2`’
in `a f`, or to ‘`[ [x, 2], [x+7, 1], [x-3, 2] ]`’ in `H a f`.
If there is an overall numeric factor, it always comes first in the list.
The functions `factor` and `factors` allow a second argument
when written in algebraic form; ‘`factor(x,v)`’ factors ‘`x`’ with
respect to the specific variable ‘`v`’. The default is to factor with
respect to all the variables that appear in ‘`x`’.

The `a c` (`calc-collect`) [`collect`] command rearranges a
formula as a
polynomial in a given variable, ordered in decreasing powers of that
variable. For example, given ‘`1 + 2 x + 3 y + 4 x y^2`’ on
the stack, `a c x` would produce ‘`(2 + 4 y^2) x + (1 + 3 y)`’,
and `a c y` would produce ‘`(4 x) y^2 + 3 y + (1 + 2 x)`’.
The polynomial will be expanded out using the distributive law as
necessary: Collecting ‘`x`’ in ‘`(x - 1)^3`’ produces
‘`x^3 - 3 x^2 + 3 x - 1`’. Terms not involving ‘`x`’ will
not be expanded.

The “variable” you specify at the prompt can actually be any
expression: `a c ln(x+1)` will collect together all terms multiplied
by ‘`ln(x+1)`’ or integer powers thereof. If ‘`x`’ also appears
in the formula in a context other than ‘`ln(x+1)`’, `a c` will
treat those occurrences as unrelated to ‘`ln(x+1)`’, i.e., as constants.

The `a x` (`calc-expand`) [`expand`] command expands an
expression by applying the distributive law everywhere. It applies to
products, quotients, and powers involving sums. By default, it fully
distributes all parts of the expression. With a numeric prefix argument,
the distributive law is applied only the specified number of times, then
the partially expanded expression is left on the stack.

The `a x` and `j D` commands are somewhat redundant. Use
`a x` if you want to expand all products of sums in your formula.
Use `j D` if you want to expand a particular specified term of
the formula. There is an exactly analogous correspondence between
`a f` and `j M`. (The `j D` and `j M` commands
also know many other kinds of expansions, such as
‘`exp(a + b) = exp(a) exp(b)`’, which `a x` and `a f`
do not do.)

Calc’s automatic simplifications will sometimes reverse a partial
expansion. For example, the first step in expanding ‘`(x+1)^3`’ is
to write ‘`(x+1) (x+1)^2`’. If `a x` stops there and tries
to put this formula onto the stack, though, Calc will automatically
simplify it back to ‘`(x+1)^3`’ form. The solution is to turn
simplification off first (see [Simplification Modes](#Simplification-Modes)), or to run
`a x` without a numeric prefix argument so that it expands all
the way in one step.

The `a a` (`calc-apart`) [`apart`] command expands a
rational function by partial fractions. A rational function is the
quotient of two polynomials; `apart` pulls this apart into a
sum of rational functions with simple denominators. In algebraic
notation, the `apart` function allows a second argument that
specifies which variable to use as the “base”; by default, Calc
chooses the base variable automatically.

The `a n` (`calc-normalize-rat`) [`nrat`] command
attempts to arrange a formula into a quotient of two polynomials.
For example, given ‘`1 + (a + b/c) / d`’, the result would be
‘`(b + a c + c d) / c d`’. The quotient is reduced, so that
`a n` will simplify ‘`(x^2 + 2x + 1) / (x^2 - 1)`’ by dividing
out the common factor ‘`x + 1`’, yielding ‘`(x + 1) / (x - 1)`’.

The `a \` (`calc-poly-div`) [`pdiv`] command divides
two polynomials ‘`u`’ and ‘`v`’, yielding a new polynomial
‘`q`’. If several variables occur in the inputs, the inputs are
considered multivariate polynomials. (Calc divides by the variable
with the largest power in ‘`u`’ first, or, in the case of equal
powers, chooses the variables in alphabetical order.) For example,
dividing ‘`x^2 + 3 x + 2`’ by ‘`x + 2`’ yields ‘`x + 1`’.
The remainder from the division, if any, is reported at the bottom
of the screen and is also placed in the Trail along with the quotient.

Using `pdiv` in algebraic notation, you can specify the particular
variable to be used as the base: `pdiv(a,b,x)`.
If `pdiv` is given only two arguments (as is always the case with
the `a \` command), then it does a multivariate division as outlined
above.

The `a %` (`calc-poly-rem`) [`prem`] command divides
two polynomials and keeps the remainder ‘`r`’. The quotient
‘`q`’ is discarded. For any formulas ‘`a`’ and ‘`b`’, the
results of `a \` and `a %` satisfy ‘`a = q b + r`’.
(This is analogous to plain `\` and `%`, which compute the
integer quotient and remainder from dividing two numbers.)

The `a /` (`calc-poly-div-rem`) [`pdivrem`] command
divides two polynomials and reports both the quotient and the
remainder as a vector ‘`[q, r]`’. The `H a /` [`pdivide`]
command divides two polynomials and constructs the formula
‘`q + r/b`’ on the stack. (Naturally if the remainder is zero,
this will immediately simplify to ‘`q`’.)

The `a g` (`calc-poly-gcd`) [`pgcd`] command computes
the greatest common divisor of two polynomials. (The GCD actually
is unique only to within a constant multiplier; Calc attempts to
choose a GCD which will be unsurprising.) For example, the `a n`
command uses `a g` to take the GCD of the numerator and denominator
of a quotient, then divides each by the result using `a \`. (The
definition of GCD ensures that this division can take place without
leaving a remainder.)

While the polynomials used in operations like `a /` and `a g`
often have integer coefficients, this is not required. Calc can also
deal with polynomials over the rationals or floating-point reals.
Polynomials with modulo-form coefficients are also useful in many
applications; if you enter ‘`(x^2 + 3 x - 1) mod 5`’, Calc
automatically transforms this into a polynomial over the field of
integers mod 5: ‘`(1 mod 5) x^2 + (3 mod 5) x + (4 mod 5)`’.

Congratulations and thanks go to Ove Ewerlid
(`ewerlid@mizar.DoCS.UU.SE`), who contributed many of the
polynomial routines used in the above commands.

See [Decomposing Polynomials](#Decomposing-Polynomials), for several useful functions for
extracting the individual coefficients of a polynomial.

---

Next: [Solving Equations](#Solving-Equations), Previous: [Polynomials](#Polynomials), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 10.5 Calculus [¶](#Calculus-1)

The following calculus commands do not automatically simplify their
inputs or outputs using `calc-simplify`. You may find it helps
to do this by hand by typing `a s` or `a e`. It may also help
to use `a x` and/or `a c` to arrange a result in the most
readable way.

* [Differentiation](#Differentiation)
* [Integration](#Integration)
* [Customizing the Integrator](#Customizing-the-Integrator)
* [Numerical Integration](#Numerical-Integration)
* [Taylor Series](#Taylor-Series)

---

Next: [Integration](#Integration), Up: [Calculus](#Calculus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.5.1 Differentiation [¶](#Differentiation-1)

The `a d` (`calc-derivative`) [`deriv`] command computes
the derivative of the expression on the top of the stack with respect to
some variable, which it will prompt you to enter. Normally, variables
in the formula other than the specified differentiation variable are
considered constant, i.e., ‘`deriv(y,x)`’ is reduced to zero. With
the Hyperbolic flag, the `tderiv` (total derivative) operation is used
instead, in which derivatives of variables are not reduced to zero
unless those variables are known to be “constant,” i.e., independent
of any other variables. (The built-in special variables like `pi`
are considered constant, as are variables that have been declared
`const`; see [Declarations](#Declarations).)

With a numeric prefix argument n, this command computes the
nth derivative.

When working with trigonometric functions, it is best to switch to
Radians mode first (with `m r`). The derivative of ‘`sin(x)`’
in degrees is ‘`(pi/180) cos(x)`’, probably not the expected
answer!

If you use the `deriv` function directly in an algebraic formula,
you can write ‘`deriv(f,x,x0)`’ which represents the derivative
of ‘`f`’ with respect to ‘`x`’, evaluated at the point
‘`x=x0`’.

If the formula being differentiated contains functions which Calc does
not know, the derivatives of those functions are produced by adding
primes (apostrophe characters). For example, ‘`deriv(f(2x), x)`’
produces ‘`2 f'(2 x)`’, where the function `f'` represents the
derivative of `f`.

For functions you have defined with the `Z F` command, Calc expands
the functions according to their defining formulas unless you have
also defined `f'` suitably. For example, suppose we define
‘`sinc(x) = sin(x)/x`’ using `Z F`. If we then differentiate
the formula ‘`sinc(2 x)`’, the formula will be expanded to
‘`sin(2 x) / (2 x)`’ and differentiated. However, if we also
define ‘`sinc'(x) = dsinc(x)`’, say, then Calc will write the
result as ‘`2 dsinc(2 x)`’. See [Programming with Formulas](#Algebraic-Definitions).

For multi-argument functions ‘`f(x,y,z)`’, the derivative with respect
to the first argument is written ‘`f'(x,y,z)`’; derivatives with
respect to the other arguments are ‘`f'2(x,y,z)`’ and ‘`f'3(x,y,z)`’.
Various higher-order derivatives can be formed in the obvious way, e.g.,
‘`f''(x)`’ (the second derivative of `f`) or
‘`f''2'3(x,y,z)`’ (`f` differentiated with respect to each
argument once).

---

Next: [Customizing the Integrator](#Customizing-the-Integrator), Previous: [Differentiation](#Differentiation), Up: [Calculus](#Calculus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.5.2 Integration [¶](#Integration-1)

The `a i` (`calc-integral`) [`integ`] command computes the
indefinite integral of the expression on the top of the stack with
respect to a prompted-for variable. The integrator is not guaranteed to
work for all integrable functions, but it is able to integrate several
large classes of formulas. In particular, any polynomial or rational
function (a polynomial divided by a polynomial) is acceptable.
(Rational functions don’t have to be in explicit quotient form, however;
‘`x/(1+x^-2)`’
is not strictly a quotient of polynomials, but it is equivalent to
‘`x^3/(x^2+1)`’, which is.) Also, square roots of terms involving
‘`x`’ and ‘`x^2`’ may appear in rational functions being
integrated. Finally, rational functions involving trigonometric or
hyperbolic functions can be integrated.

With an argument (`C-u a i`), this command will compute the definite
integral of the expression on top of the stack. In this case, the
command will again prompt for an integration variable, then prompt for a
lower limit and an upper limit.

If you use the `integ` function directly in an algebraic formula,
you can also write ‘`integ(f,x,v)`’ which expresses the resulting
indefinite integral in terms of variable `v` instead of `x`.
With four arguments, ‘`integ(f(x),x,a,b)`’ represents a definite
integral from `a` to `b`.

Please note that the current implementation of Calc’s integrator sometimes
produces results that are significantly more complex than they need to
be. For example, the integral Calc finds for
‘`1/(x+sqrt(x^2+1))`’
is several times more complicated than the answer Mathematica
returns for the same input, although the two forms are numerically
equivalent. Also, any indefinite integral should be considered to have
an arbitrary constant of integration added to it, although Calc does not
write an explicit constant of integration in its result. For example,
Calc’s solution for
‘`1/(1+tan(x))`’
differs from the solution given in the *CRC Math Tables* by a
constant factor of
‘`pi i / 2`’,
due to a different choice of constant of integration.

The Calculator remembers all the integrals it has done. If conditions
change in a way that would invalidate the old integrals, say, a switch
from Degrees to Radians mode, then they will be thrown out. If you
suspect this is not happening when it should, use the
`calc-flush-caches` command; see [Caches](#Caches).

Calc normally will pursue integration by substitution or integration by
parts up to 3 nested times before abandoning an approach as fruitless.
If the integrator is taking too long, you can lower this limit by storing
a number (like 2) in the variable `IntegLimit`. (The `s I`
command is a convenient way to edit `IntegLimit`.) If this variable
has no stored value or does not contain a nonnegative integer, a limit
of 3 is used. The lower this limit is, the greater the chance that Calc
will be unable to integrate a function it could otherwise handle. Raising
this limit allows the Calculator to solve more integrals, though the time
it takes may grow exponentially. You can monitor the integrator’s actions
by creating an Emacs buffer called `*Trace*`. If such a buffer
exists, the `a i` command will write a log of its actions there.

If you want to manipulate integrals in a purely symbolic way, you can
set the integration nesting limit to 0 to prevent all but fast
table-lookup solutions of integrals. You might then wish to define
rewrite rules for integration by parts, various kinds of substitutions,
and so on. See [Rewrite Rules](#Rewrite-Rules).

---

Next: [Numerical Integration](#Numerical-Integration), Previous: [Integration](#Integration), Up: [Calculus](#Calculus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.5.3 Customizing the Integrator [¶](#Customizing-the-Integrator-1)

Calc has two built-in rewrite rules called `IntegRules` and
`IntegAfterRules` which you can edit to define new integration
methods. See [Rewrite Rules](#Rewrite-Rules). At each step of the integration process,
Calc wraps the current integrand in a call to the fictitious function
‘`integtry(expr,var)`’, where expr is the
integrand and var is the integration variable. If your rules
rewrite this to be a plain formula (not a call to `integtry`), then
Calc will use this formula as the integral of expr. For example,
the rule ‘`integtry(mysin(x),x) := -mycos(x)`’ would define a rule to
integrate a function `mysin` that acts like the sine function.
Then, putting ‘`4 mysin(2y+1)`’ on the stack and typing `a i y`
will produce the integral ‘`-2 mycos(2y+1)`’. Note that Calc has
automatically made various transformations on the integral to allow it
to use your rule; integral tables generally give rules for
‘`mysin(a x + b)`’, but you don’t need to use this much generality
in your `IntegRules`.

As a more serious example, the expression ‘`exp(x)/x`’ cannot be
integrated in terms of the standard functions, so the “exponential
integral” function
‘`Ei(x)`’
was invented to describe it.
We can get Calc to do this integral in terms of a made-up `Ei`
function by adding the rule ‘`[integtry(exp(x)/x, x) := Ei(x)]`’
to `IntegRules`. Now entering ‘`exp(2x)/x`’ on the stack
and typing `a i x` yields ‘`Ei(2 x)`’. This new rule will
work with Calc’s various built-in integration methods (such as
integration by substitution) to solve a variety of other problems
involving `Ei`: For example, now Calc will also be able to
integrate ‘`exp(exp(x))`’ and ‘`ln(ln(x))`’ (to get ‘`Ei(exp(x))`’
and ‘`x ln(ln(x)) - Ei(ln(x))`’, respectively).

Your rule may do further integration by calling `integ`. For
example, ‘`integtry(twice(u),x) := twice(integ(u))`’ allows Calc
to integrate ‘`twice(sin(x))`’ to get ‘`twice(-cos(x))`’.
Note that `integ` was called with only one argument. This notation
is allowed only within `IntegRules`; it means “integrate this
with respect to the same integration variable.” If Calc is unable
to integrate `u`, the integration that invoked `IntegRules`
also fails. Thus integrating ‘`twice(f(x))`’ fails, returning the
unevaluated integral ‘`integ(twice(f(x)), x)`’. It is still valid
to call `integ` with two or more arguments, however; in this case,
if `u` is not integrable, `twice` itself will still be
integrated: If the above rule is changed to ‘`... := twice(integ(u,x))`’,
then integrating ‘`twice(f(x))`’ will yield ‘`twice(integ(f(x),x))`’.

If a rule instead produces the formula ‘`integsubst(sexpr,
svar)`’, either replacing the top-level `integtry` call or
nested anywhere inside the expression, then Calc will apply the
substitution ‘`u = sexpr(svar)`’ to try to
integrate the original expr. For example, the rule
‘`sqrt(a) := integsubst(sqrt(x),x)`’ says that if Calc ever finds
a square root in the integrand, it should attempt the substitution
‘`u = sqrt(x)`’. (This particular rule is unnecessary because
Calc always tries “obvious” substitutions where sexpr actually
appears in the integrand.) The variable svar may be the same
as the var that appeared in the call to `integtry`, but
it need not be.

When integrating according to an `integsubst`, Calc uses the
equation solver to find the inverse of sexpr (if the integrand
refers to var anywhere except in subexpressions that exactly
match sexpr). It uses the differentiator to find the derivative
of sexpr and/or its inverse (it has two methods that use one
derivative or the other). You can also specify these items by adding
extra arguments to the `integsubst` your rules construct; the
general form is ‘`integsubst(sexpr, svar, sinv,
sprime)`’, where sinv is the inverse of sexpr (still
written as a function of svar), and sprime is the
derivative of sexpr with respect to svar. If you don’t
specify these things, and Calc is not able to work them out on its
own with the information it knows, then your substitution rule will
work only in very specific, simple cases.

Calc applies `IntegRules` as if by `C-u 1 a r IntegRules`;
in other words, Calc stops rewriting as soon as any rule in your rule
set succeeds. (If it weren’t for this, the ‘`integsubst(sqrt(x),x)`’
example above would keep on adding layers of `integsubst` calls
forever!)

Another set of rules, stored in `IntegSimpRules`, are applied
every time the integrator uses algebraic simplifications to simplify an
intermediate result. For example, putting the rule
‘`twice(x) := 2 x`’ into `IntegSimpRules` would tell Calc to
convert the `twice` function into a form it knows whenever
integration is attempted.

One more way to influence the integrator is to define a function with
the `Z F` command (see [Programming with Formulas](#Algebraic-Definitions)). Calc’s
integrator automatically expands such functions according to their
defining formulas, even if you originally asked for the function to
be left unevaluated for symbolic arguments. (Certain other Calc
systems, such as the differentiator and the equation solver, also
do this.)

Sometimes Calc is able to find a solution to your integral, but it
expresses the result in a way that is unnecessarily complicated. If
this happens, you can either use `integsubst` as described
above to try to hint at a more direct path to the desired result, or
you can use `IntegAfterRules`. This is an extra rule set that
runs after the main integrator returns its result; basically, Calc does
an `a r IntegAfterRules` on the result before showing it to you.
(It also does algebraic simplifications, without `IntegSimpRules`,
after that to further simplify the result.) For example, Calc’s integrator
sometimes produces expressions of the form ‘`ln(1+x) - ln(1-x)`’;
the default `IntegAfterRules` rewrite this into the more readable
form ‘`2 arctanh(x)`’. Note that, unlike `IntegRules`,
`IntegSimpRules` and `IntegAfterRules` are applied any number
of times until no further changes are possible. Rewriting by
`IntegAfterRules` occurs only after the main integrator has
finished, not at every step as for `IntegRules` and
`IntegSimpRules`.

---

Next: [Taylor Series](#Taylor-Series), Previous: [Customizing the Integrator](#Customizing-the-Integrator), Up: [Calculus](#Calculus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.5.4 Numerical Integration [¶](#Numerical-Integration-1)

If you want a purely numerical answer to an integration problem, you can
use the `a I` (`calc-num-integral`) [`ninteg`] command. This
command prompts for an integration variable, a lower limit, and an
upper limit. Except for the integration variable, all other variables
that appear in the integrand formula must have stored values. (A stored
value, if any, for the integration variable itself is ignored.)

Numerical integration works by evaluating your formula at many points in
the specified interval. Calc uses an “open Romberg” method; this means
that it does not evaluate the formula actually at the endpoints (so that
it is safe to integrate ‘`sin(x)/x`’ from zero, for example). Also,
the Romberg method works especially well when the function being
integrated is fairly smooth. If the function is not smooth, Calc will
have to evaluate it at quite a few points before it can accurately
determine the value of the integral.

Integration is much faster when the current precision is small. It is
best to set the precision to the smallest acceptable number of digits
before you use `a I`. If Calc appears to be taking too long, press
`C-g` to halt it and try a lower precision. If Calc still appears
to need hundreds of evaluations, check to make sure your function is
well-behaved in the specified interval.

It is possible for the lower integration limit to be ‘`-inf`’ (minus
infinity). Likewise, the upper limit may be plus infinity. Calc
internally transforms the integral into an equivalent one with finite
limits. However, integration to or across singularities is not supported:
The integral of ‘`1/sqrt(x)`’ from 0 to 1 exists (it can be found
by Calc’s symbolic integrator, for example), but `a I` will fail
because the integrand goes to infinity at one of the endpoints.

---

Previous: [Numerical Integration](#Numerical-Integration), Up: [Calculus](#Calculus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.5.5 Taylor Series [¶](#Taylor-Series-1)

The `a t` (`calc-taylor`) [`taylor`] command computes a
power series expansion or Taylor series of a function. You specify the
variable and the desired number of terms. You may give an expression of
the form ‘`var = a`’ or ‘`var - a`’ instead
of just a variable to produce a Taylor expansion about the point a.
You may specify the number of terms with a numeric prefix argument;
otherwise the command will prompt you for the number of terms. Note that
many series expansions have coefficients of zero for some terms, so you
may appear to get fewer terms than you asked for.

If the `a i` command is unable to find a symbolic integral for a
function, you can get an approximation by integrating the function’s
Taylor series.

---

Next: [Numerical Solutions](#Numerical-Solutions), Previous: [Calculus](#Calculus), Up: [Algebra](#Algebra)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
