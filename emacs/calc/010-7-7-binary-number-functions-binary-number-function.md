### 7.7 Binary Number Functions [¶](#Binary-Number-Functions)

The commands in this chapter all use two-letter sequences beginning with
the `b` prefix.

The “binary” operations actually work regardless of the currently
displayed radix, although their results make the most sense in a radix
like 2, 8, or 16 (as obtained by the `d 2`, `d 8`, or `d 6`
commands, respectively). You may also wish to enable display of leading
zeros with `d z`. See [Radix Modes](#Radix-Modes).

The Calculator maintains a current *word size* ‘`w`’, an
arbitrary integer. For a positive word size, all
of the binary operations described here operate modulo ‘`2^w`’. In
particular, negative arguments are converted to positive integers modulo
‘`2^w`’ by all binary functions.

If the word size is negative, binary operations produce twos-complement
integers from
‘`-(2^(-w-1))`’
to
‘`2^(-w-1)-1`’
inclusive. Either mode accepts inputs in any range; the sign of
‘`w`’ affects only the results produced.

If the word size is zero, binary operations work on the entire number
without clipping, as if the word size had been negative infinity.

The `b c` (`calc-clip`)
[`clip`] command can be used to clip a number by reducing it modulo
‘`2^w`’. The commands described in this chapter automatically clip
their results to the current word size. Note that other operations like
addition do not use the current word size, since integer addition
generally is not “binary.” (However, see [Simplification Modes](#Simplification-Modes),
`calc-bin-simplify-mode`.) For example, with a word size of 8
bits `b c` converts a number to the range 0 to 255; with a word
size of *-8* `b c` converts to the range *-128* to 127.

The default word size is 32 bits. All operations except the shifts and
rotates allow you to specify a different word size for that one
operation by giving a numeric prefix argument: `C-u 8 b c` clips the
top of stack to the range 0 to 255 regardless of the current word size.
To set the word size permanently, use `b w` (`calc-word-size`).
This command displays a prompt with the current word size; press `RET`
immediately to keep this word size, or type a new word size at the prompt.

When the binary operations are written in symbolic form, they take an
optional second (or third) word-size parameter. When a formula like
‘`and(a,b)`’ is finally evaluated, the word size current at that time
will be used, but when ‘`and(a,b,-8)`’ is evaluated, a word size of
*-8* will always be used. A symbolic binary function will be left
in symbolic form unless the all of its argument(s) are integers or
integer-valued floats.

If either or both arguments are modulo forms for which ‘`M`’ is a
power of two, that power of two is taken as the word size unless a
numeric prefix argument overrides it. The current word size is never
consulted when modulo-power-of-two forms are involved.

The `b a` (`calc-and`) [`and`] command computes the bitwise
AND of the two numbers on the top of the stack. In other words, for each
of the ‘`w`’ binary digits of the two numbers (pairwise), the corresponding
bit of the result is 1 if and only if both input bits are 1:
‘`and(2#1100, 2#1010) = 2#1000`’.

The `b o` (`calc-or`) [`or`] command computes the bitwise
inclusive OR of two numbers. A bit is 1 if either of the input bits, or
both, are 1: ‘`or(2#1100, 2#1010) = 2#1110`’.

The `b x` (`calc-xor`) [`xor`] command computes the bitwise
exclusive OR of two numbers. A bit is 1 if exactly one of the input bits
is 1: ‘`xor(2#1100, 2#1010) = 2#0110`’.

The `b d` (`calc-diff`) [`diff`] command computes the bitwise
difference of two numbers; this is defined by ‘`diff(a,b) = and(a,not(b))`’,
so that ‘`diff(2#1100, 2#1010) = 2#0100`’.

The `b n` (`calc-not`) [`not`] command computes the bitwise
NOT of a number. A bit is 1 if the input bit is 0 and vice-versa.

The `b l` (`calc-lshift-binary`) [`lsh`] command shifts a
number left by one bit, or by the number of bits specified in the numeric
prefix argument. A negative prefix argument performs a logical right shift,
in which zeros are shifted in on the left. In symbolic form, ‘`lsh(a)`’
is short for ‘`lsh(a,1)`’, which in turn is short for ‘`lsh(a,n,w)`’.
Bits shifted “off the end,” according to the current word size, are lost.

The `H b l` command also does a left shift, but it takes two arguments
from the stack (the value to shift, and, at top-of-stack, the number of
bits to shift). This version interprets the prefix argument just like
the regular binary operations, i.e., as a word size. The Hyperbolic flag
has a similar effect on the rest of the binary shift and rotate commands.

The `b r` (`calc-rshift-binary`) [`rsh`] command shifts a
number right by one bit, or by the number of bits specified in the numeric
prefix argument: ‘`rsh(a,n) = lsh(a,-n)`’.

The `b L` (`calc-lshift-arith`) [`ash`] command shifts a
number left. It is analogous to `lsh`, except that if the shift
is rightward (the prefix argument is negative), an arithmetic shift
is performed as described below.

The `b R` (`calc-rshift-arith`) [`rash`] command performs
an “arithmetic” shift to the right, in which the leftmost bit (according
to the current word size) is duplicated rather than shifting in zeros.
This corresponds to dividing by a power of two where the input is interpreted
as a signed, twos-complement number. (The distinction between the ‘`rsh`’
and ‘`rash`’ operations is totally independent from whether the word
size is positive or negative.) With a negative prefix argument, this
performs a standard left shift.

When the word size is zero, logical and arithmetic shift operations
are identical: a negative value shifted right remains negative, since
there is an infinite supply of ones to shift in.

The `b t` (`calc-rotate-binary`) [`rot`] command rotates a
number one bit to the left. The leftmost bit (according to the current
word size) is dropped off the left and shifted in on the right. With a
numeric prefix argument, the number is rotated that many bits to the left
or right.

Rotation is not possible with a zero word size.

See [Set Operations using Vectors](#Set-Operations), for the `b p` and `b u` commands that
pack and unpack binary integers into sets. (For example, `b u`
unpacks the number ‘`2#11001`’ to the set of bit-numbers
‘`[0, 3, 4]`’.) Type `b u V #` to count the number of “1”
bits in a binary integer.

Another interesting use of the set representation of binary integers
is to reverse the bits in, say, a 32-bit integer. Type `b u` to
unpack; type `31 TAB -` to replace each bit-number in the set
with 31 minus that bit-number; type `b p` to pack the set back
into a binary integer.

---

Next: [Vector/Matrix Functions](#Matrix-Functions), Previous: [Arithmetic Functions](#Arithmetic), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 8 Scientific Functions [¶](#Scientific-Functions-1)

The functions described here perform trigonometric and other transcendental
calculations. They generally produce floating-point answers correct to the
full current precision. The `H` (Hyperbolic) and `I` (Inverse)
flag keys must be used to get some of these functions from the keyboard.

One miscellaneous command is shift-`P` (`calc-pi`), which pushes
the value of ‘`pi`’ (at the current precision) onto the stack. With the
Hyperbolic flag, it pushes the value ‘`e`’, the base of natural logarithms.
With the Inverse flag, it pushes Euler’s constant
‘`gamma`’
(about 0.5772). With both Inverse and Hyperbolic, it
pushes the “golden ratio”
‘`phi`’
(about 1.618). (At present, Euler’s constant is not available
to unlimited precision; Calc knows only the first 100 digits.)
In Symbolic mode, these commands push the
actual variables ‘`pi`’, ‘`e`’, ‘`gamma`’, and ‘`phi`’,
respectively, instead of their values; see [Symbolic Mode](#Symbolic-Mode).

The `Q` (`calc-sqrt`) [`sqrt`] function is described elsewhere;
see [Basic Arithmetic](#Basic-Arithmetic). With the Inverse flag [`sqr`], this command
computes the square of the argument.

See [Numeric Prefix Arguments](#Prefix-Arguments), for a discussion of the effect of numeric
prefix arguments on commands in this chapter which do not otherwise
interpret a prefix argument.

* [Logarithmic Functions](#Logarithmic-Functions)
* [Trigonometric/Hyperbolic Functions](#Trigonometric-and-Hyperbolic-Functions)
* [Advanced Mathematical Functions](#Advanced-Math-Functions)
* [Branch Cuts and Principal Values](#Branch-Cuts)
* [Random Numbers](#Random-Numbers)
* [Combinatorial Functions](#Combinatorial-Functions)
* [Probability Distribution Functions](#Probability-Distribution-Functions)

---

Next: [Trigonometric/Hyperbolic Functions](#Trigonometric-and-Hyperbolic-Functions), Up: [Scientific Functions](#Scientific-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 8.1 Logarithmic Functions [¶](#Logarithmic-Functions-1)

The shift-`L` (`calc-ln`) [`ln`] command computes the natural
logarithm of the real or complex number on the top of the stack. With
the Inverse flag it computes the exponential function instead, although
this is redundant with the `E` command.

The shift-`E` (`calc-exp`) [`exp`] command computes the
exponential, i.e., ‘`e`’ raised to the power of the number on the stack.
The meanings of the Inverse and Hyperbolic flags follow from those for
the `calc-ln` command.

The `H L` (`calc-log10`) [`log10`] command computes the common
(base-10) logarithm of a number. (With the Inverse flag [`exp10`],
it raises ten to a given power.) Note that the common logarithm of a
complex number is computed by taking the natural logarithm and dividing
by
‘`ln(10)`’.

The `B` (`calc-log`) [`log`] command computes a logarithm
to any base. For example, `1024 RET 2 B` produces 10, since
‘`2^10 = 1024`’.
In certain cases like ‘`log(3,9)`’, the result
will be either ‘`1:2`’ or ‘`0.5`’ depending on the current Fraction
mode setting. With the Inverse flag [`alog`], this command is
similar to `^` except that the order of the arguments is reversed.

The `f I` (`calc-ilog`) [`ilog`] command computes the
integer logarithm of a number to any base. The number and the base must
themselves be positive integers. This is the true logarithm, rounded
down to an integer. Thus `ilog(x,10)` is 3 for all ‘`x`’ in the
range from 1000 to 9999. If both arguments are positive integers, exact
integer arithmetic is used; otherwise, this is equivalent to
‘`floor(log(x,b))`’.

The `f E` (`calc-expm1`) [`expm1`] command computes
‘`exp(x)-1`’,
but using an algorithm that produces a more accurate
answer when the result is close to zero, i.e., when
‘`exp(x)`’
is close to one.

The `f L` (`calc-lnp1`) [`lnp1`] command computes
‘`ln(x+1)`’,
producing a more accurate answer when ‘`x`’ is close to zero.

---

Next: [Advanced Mathematical Functions](#Advanced-Math-Functions), Previous: [Logarithmic Functions](#Logarithmic-Functions), Up: [Scientific Functions](#Scientific-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 8.2 Trigonometric/Hyperbolic Functions [¶](#Trigonometric_002fHyperbolic-Functions)

The shift-`S` (`calc-sin`) [`sin`] command computes the sine
of an angle or complex number. If the input is an HMS form, it is interpreted
as degrees-minutes-seconds; otherwise, the input is interpreted according
to the current angular mode. It is best to use Radians mode when operating
on complex numbers.

Calc’s “units” mechanism includes angular units like `deg`,
`rad`, and `grad`. While ‘`sin(45 deg)`’ is not evaluated
all the time, the `u s` (`calc-simplify-units`) command will
simplify ‘`sin(45 deg)`’ by taking the sine of 45 degrees, regardless
of the current angular mode. See [Basic Operations on Units](#Basic-Operations-on-Units).

Also, the symbolic variable `pi` is not ordinarily recognized in
arguments to trigonometric functions, as in ‘`sin(3 pi / 4)`’, but
the default algebraic simplifications recognize many such
formulas when the current angular mode is Radians *and* Symbolic
mode is enabled; this example would be replaced by ‘`sqrt(2) / 2`’.
See [Symbolic Mode](#Symbolic-Mode). Beware, this simplification occurs even if you
have stored a different value in the variable ‘`pi`’; this is one
reason why changing built-in variables is a bad idea. Arguments of
the form ‘`x`’ plus a multiple of ‘`pi/2`’ are also simplified.
Calc includes similar formulas for `cos` and `tan`.

Calc’s algebraic simplifications know all angles which are integer multiples of
‘`pi/12`’, ‘`pi/10`’, or ‘`pi/8`’ radians. In Degrees mode,
analogous simplifications occur for integer multiples of 15 or 18
degrees, and for arguments plus multiples of 90 degrees.

With the Inverse flag, `calc-sin` computes an arcsine. This is also
available as the `calc-arcsin` command or `arcsin` algebraic
function. The returned argument is converted to degrees, radians, or HMS
notation depending on the current angular mode.

With the Hyperbolic flag, `calc-sin` computes the hyperbolic
sine, also available as `calc-sinh` [`sinh`]. With the
Hyperbolic and Inverse flags, it computes the hyperbolic arcsine
(`calc-arcsinh`) [`arcsinh`].

The shift-`C` (`calc-cos`) [`cos`] command computes the cosine
of an angle or complex number, and shift-`T` (`calc-tan`) [`tan`]
computes the tangent, along with all the various inverse and hyperbolic
variants of these functions.

The `f T` (`calc-arctan2`) [`arctan2`] command takes two
numbers from the stack and computes the arc tangent of their ratio. The
result is in the full range from *-180* (exclusive) to *+180*
(inclusive) degrees, or the analogous range in radians. A similar
result would be obtained with `/` followed by `I T`, but the
value would only be in the range from *-90* to *+90* degrees
since the division loses information about the signs of the two
components, and an error might result from an explicit division by zero
which `arctan2` would avoid. By (arbitrary) definition,
‘`arctan2(0,0)=0`’.

The `calc-sincos` [`sincos`] command computes the sine and
cosine of a number, returning them as a vector of the form
‘`[cos, sin]`’.
With the Inverse flag [`arcsincos`], this command takes a two-element
vector as an argument and computes `arctan2` of the elements.
(This command does not accept the Hyperbolic flag.)

The remaining trigonometric functions, `calc-sec` [`sec`],
`calc-csc` [`csc`] and `calc-cot` [`cot`], are also
available. With the Hyperbolic flag, these compute their hyperbolic
counterparts, which are also available separately as `calc-sech`
[`sech`], `calc-csch` [`csch`] and `calc-coth`
[`coth`]. (These commands do not accept the Inverse flag.)

---

Next: [Branch Cuts and Principal Values](#Branch-Cuts), Previous: [Trigonometric/Hyperbolic Functions](#Trigonometric-and-Hyperbolic-Functions), Up: [Scientific Functions](#Scientific-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 8.3 Advanced Mathematical Functions [¶](#Advanced-Mathematical-Functions)

Calc can compute a variety of less common functions that arise in
various branches of mathematics. All of the functions described in
this section allow arbitrary complex arguments and, except as noted,
will work to arbitrarily large precision. They can not at present
handle error forms or intervals as arguments.

NOTE: These functions are still experimental. In particular, their
accuracy is not guaranteed in all domains. It is advisable to set the
current precision comfortably higher than you actually need when
using these functions. Also, these functions may be impractically
slow for some values of the arguments.

The `f g` (`calc-gamma`) [`gamma`] command computes the Euler
gamma function. For positive integer arguments, this is related to the
factorial function: ‘`gamma(n+1) = fact(n)`’. For general complex
arguments the gamma function can be defined by the following definite
integral:
‘`gamma(a) = integ(t^(a-1) exp(-t), t, 0, inf)`’.
(The actual implementation uses far more efficient computational methods.)

The `f G` (`calc-inc-gamma`) [`gammaP`] command computes
the incomplete gamma function, denoted ‘`P(a,x)`’. This is defined by
the integral,
‘`gammaP(a,x) = integ(t^(a-1) exp(t), t, 0, x) / gamma(a)`’.
This implies that ‘`gammaP(a,inf) = 1`’ for any ‘`a`’ (see the
definition of the normal gamma function).

Several other varieties of incomplete gamma function are defined.
The complement of ‘`P(a,x)`’, called ‘`Q(a,x) = 1-P(a,x)`’ by
some authors, is computed by the `I f G` [`gammaQ`] command.
You can think of this as taking the other half of the integral, from
‘`x`’ to infinity.

The functions corresponding to the integrals that define ‘`P(a,x)`’
and ‘`Q(a,x)`’ but without the normalizing ‘`1/gamma(a)`’
factor are called ‘`g(a,x)`’ and ‘`G(a,x)`’, respectively
(where ‘`g`’ and ‘`G`’ represent the lower- and upper-case Greek
letter gamma). You can obtain these using the `H f G` [`gammag`]
and `H I f G` [`gammaG`] commands.

The `f b` (`calc-beta`) [`beta`] command computes the
Euler beta function, which is defined in terms of the gamma function as
‘`beta(a,b) = gamma(a) gamma(b) / gamma(a+b)`’,
or by
‘`beta(a,b) = integ(t^(a-1) (1-t)^(b-1), t, 0, 1)`’.

The `f B` (`calc-inc-beta`) [`betaI`] command computes
the incomplete beta function ‘`I(x,a,b)`’. It is defined by
‘`betaI(x,a,b) = integ(t^(a-1) (1-t)^(b-1), t, 0, x) / beta(a,b)`’.
Once again, the `H` (hyperbolic) prefix gives the corresponding
un-normalized version [`betaB`].

The `f e` (`calc-erf`) [`erf`] command computes the
error function
‘`erf(x) = 2 integ(exp(-(t^(2))), t, 0, x) / sqrt(pi)`’.
The complementary error function `I f e` (`calc-erfc`) [`erfc`]
is the corresponding integral from ‘`x`’ to infinity; the sum
‘`erf(x) + erfc(x) = 1`’.

The `f j` (`calc-bessel-J`) [`besJ`] and `f y`
(`calc-bessel-Y`) [`besY`] commands compute the Bessel
functions of the first and second kinds, respectively.
In ‘`besJ(n,x)`’ and ‘`besY(n,x)`’ the “order” parameter
‘`n`’ is often an integer, but is not required to be one.
Calc’s implementation of the Bessel functions currently limits the
precision to 8 digits, and may not be exact even to that precision.
Use with care!

---

Next: [Random Numbers](#Random-Numbers), Previous: [Advanced Mathematical Functions](#Advanced-Math-Functions), Up: [Scientific Functions](#Scientific-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 8.4 Branch Cuts and Principal Values [¶](#Branch-Cuts-and-Principal-Values)

All of the logarithmic, trigonometric, and other scientific functions are
defined for complex numbers as well as for reals.
This section describes the values
returned in cases where the general result is a family of possible values.
Calc follows section 12.5.3 of Steele’s *Common Lisp, the Language*,
second edition, in these matters. This section will describe each
function briefly; for a more detailed discussion (including some nifty
diagrams), consult Steele’s book.

Note that the branch cuts for `arctan` and `arctanh` were
changed between the first and second editions of Steele. Recent
versions of Calc follow the second edition.

The new branch cuts exactly match those of the HP-28/48 calculators.
They also match those of Mathematica 1.2, except that Mathematica’s
`arctan` cut is always in the right half of the complex plane,
and its `arctanh` cut is always in the top half of the plane.
Calc’s cuts are continuous with quadrants I and III for `arctan`,
or II and IV for `arctanh`.

Note: The current implementations of these functions with complex arguments
are designed with proper behavior around the branch cuts in mind, *not*
efficiency or accuracy. You may need to increase the floating precision
and wait a while to get suitable answers from them.

For ‘`sqrt(a+bi)`’: When ‘`a<0`’ and ‘`b`’ is small but positive
or zero, the result is close to the ‘`+i`’ axis. For ‘`b`’ small and
negative, the result is close to the ‘`-i`’ axis. The result always lies
in the right half of the complex plane.

For ‘`ln(a+bi)`’: The real part is defined as ‘`ln(abs(a+bi))`’.
The imaginary part is defined as ‘`arg(a+bi) = arctan2(b,a)`’.
Thus the branch cuts for `sqrt` and `ln` both lie on the
negative real axis.

The following table describes these branch cuts in another way.
If the real and imaginary parts of ‘`z`’ are as shown, then
the real and imaginary parts of ‘`f(z)`’ will be as shown.
Here `eps` stands for a small positive value; each
occurrence of `eps` may stand for a different small value.

```
     z           sqrt(z)       ln(z)
----------------------------------------
   +,   0         +,  0       any, 0
   -,   0         0,  +       any, pi
   -, +eps      +eps, +      +eps, +
   -, -eps      +eps, -      +eps, -
```

For ‘`z1^z2`’: This is defined by ‘`exp(ln(z1)*z2)`’.
One interesting consequence of this is that ‘`(-8)^1:3`’ does
not evaluate to *-2* as you might expect, but to the complex
number ‘`(1., 1.732)`’. Both of these are valid cube roots
of *-8* (as is ‘`(1., -1.732)`’); Calc chooses a perhaps
less-obvious root for the sake of mathematical consistency.

For ‘`arcsin(z)`’: This is defined by ‘`-i*ln(i*z + sqrt(1-z^2))`’.
The branch cuts are on the real axis, less than *-1* and greater than 1.

For ‘`arccos(z)`’: This is defined by ‘`-i*ln(z + i*sqrt(1-z^2))`’,
or equivalently by ‘`pi/2 - arcsin(z)`’. The branch cuts are on
the real axis, less than *-1* and greater than 1.

For ‘`arctan(z)`’: This is defined by
‘`(ln(1+i*z) - ln(1-i*z)) / (2*i)`’. The branch cuts are on the
imaginary axis, below ‘`-i`’ and above ‘`i`’.

For ‘`arcsinh(z)`’: This is defined by ‘`ln(z + sqrt(1+z^2))`’.
The branch cuts are on the imaginary axis, below ‘`-i`’ and
above ‘`i`’.

For ‘`arccosh(z)`’: This is defined by
‘`ln(z + (z+1)*sqrt((z-1)/(z+1)))`’. The branch cut is on the
real axis less than 1.

For ‘`arctanh(z)`’: This is defined by ‘`(ln(1+z) - ln(1-z)) / 2`’.
The branch cuts are on the real axis, less than *-1* and greater than 1.

The following tables for `arcsin`, `arccos`, and
`arctan` assume the current angular mode is Radians. The
hyperbolic functions operate independently of the angular mode.

```
       z             arcsin(z)            arccos(z)
-------------------------------------------------------
 (-1..1),  0      (-pi/2..pi/2), 0       (0..pi), 0
 (-1..1), +eps    (-pi/2..pi/2), +eps    (0..pi), -eps
 (-1..1), -eps    (-pi/2..pi/2), -eps    (0..pi), +eps
   <-1,    0          -pi/2,     +         pi,    -
   <-1,  +eps      -pi/2 + eps,  +      pi - eps, -
   <-1,  -eps      -pi/2 + eps,  -      pi - eps, +
    >1,    0           pi/2,     -          0,    +
    >1,  +eps       pi/2 - eps,  +        +eps,   -
    >1,  -eps       pi/2 - eps,  -        +eps,   +
```

```
       z            arccosh(z)         arctanh(z)
-----------------------------------------------------
 (-1..1),  0        0,  (0..pi)       any,     0
 (-1..1), +eps    +eps, (0..pi)       any,    +eps
 (-1..1), -eps    +eps, (-pi..0)      any,    -eps
   <-1,    0        +,    pi           -,     pi/2
   <-1,  +eps       +,  pi - eps       -,  pi/2 - eps
   <-1,  -eps       +, -pi + eps       -, -pi/2 + eps
    >1,    0        +,     0           +,    -pi/2
    >1,  +eps       +,   +eps          +,  pi/2 - eps
    >1,  -eps       +,   -eps          +, -pi/2 + eps
```

```
       z           arcsinh(z)           arctan(z)
-----------------------------------------------------
   0, (-1..1)    0, (-pi/2..pi/2)         0,     any
   0,   <-1      -,    -pi/2            -pi/2,    -
 +eps,  <-1      +, -pi/2 + eps       pi/2 - eps, -
 -eps,  <-1      -, -pi/2 + eps      -pi/2 + eps, -
   0,    >1      +,     pi/2             pi/2,    +
 +eps,   >1      +,  pi/2 - eps       pi/2 - eps, +
 -eps,   >1      -,  pi/2 - eps      -pi/2 + eps, +
```

Finally, the following identities help to illustrate the relationship
between the complex trigonometric and hyperbolic functions. They
are valid everywhere, including on the branch cuts.

```
sin(i*z)  = i*sinh(z)       arcsin(i*z)  = i*arcsinh(z)
cos(i*z)  =   cosh(z)       arcsinh(i*z) = i*arcsin(z)
tan(i*z)  = i*tanh(z)       arctan(i*z)  = i*arctanh(z)
sinh(i*z) = i*sin(z)        cosh(i*z)    =   cos(z)
```

The “advanced math” functions (gamma, Bessel, etc.) are also defined
for general complex arguments, but their branch cuts and principal values
are not rigorously specified at present.

---

Next: [Combinatorial Functions](#Combinatorial-Functions), Previous: [Branch Cuts and Principal Values](#Branch-Cuts), Up: [Scientific Functions](#Scientific-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 8.5 Random Numbers [¶](#Random-Numbers-1)

The `k r` (`calc-random`) [`random`] command produces
random numbers of various sorts.

Given a positive numeric prefix argument ‘`M`’, it produces a random
integer ‘`N`’ in the range
‘`0 <= N < M`’.
Each possible value ‘`N`’ appears with equal probability.

With no numeric prefix argument, the `k r` command takes its argument
from the stack instead. Once again, if this is a positive integer ‘`M`’
the result is a random integer less than ‘`M`’. If ‘`M`’ is negative,
the result is a random integer in the range
‘`M < N <= 0`’.

If the value on the stack is a floating-point number ‘`M`’, the result
is a random floating-point number ‘`N`’ in the range
‘`0 <= N < M`’
or
‘`M < N <= 0`’,
according to the sign of ‘`M`’.

If ‘`M`’ is zero, the result is a Gaussian-distributed random real
number; the distribution has a mean of zero and a standard deviation
of one. The algorithm used generates random numbers in pairs; thus,
every other call to this function will be especially fast.

If ‘`M`’ is an error form
‘`m +/- s`’
where m and
s
are both real numbers, the result uses a Gaussian distribution with mean
m and standard deviation
s.

If ‘`M`’ is an interval form, the lower and upper bounds specify the
acceptable limits of the random numbers. If both bounds are integers,
the result is a random integer in the specified range. If either bound
is floating-point, the result is a random real number in the specified
range. If the interval is open at either end, the result will be sure
not to equal that end value. (This makes a big difference for integer
intervals, but for floating-point intervals it’s relatively minor:
with a precision of 6, ‘`random([1.0..2.0))`’ will return any of one
million numbers from 1.00000 to 1.99999; ‘`random([1.0..2.0])`’ may
additionally return 2.00000, but the probability of this happening is
extremely small.)

If ‘`M`’ is a vector, the result is one element taken at random from
the vector. All elements of the vector are given equal probabilities.

The sequence of numbers produced by `k r` is completely random by
default, i.e., the sequence is seeded each time you start Calc using
the current time and other information. You can get a reproducible
sequence by storing a particular “seed value” in the Calc variable
`RandSeed`. Any integer will do for a seed; integers of from 1
to 12 digits are good. If you later store a different integer into
`RandSeed`, Calc will switch to a different pseudo-random
sequence. If you “unstore” `RandSeed`, Calc will re-seed itself
from the current time. If you store the same integer that you used
before back into `RandSeed`, you will get the exact same sequence
of random numbers as before.

The `calc-rrandom` command (not on any key) produces a random real
number between zero and one. It is equivalent to ‘`random(1.0)`’.

The `k a` (`calc-random-again`) command produces another random
number, reusing the most recent value of ‘`M`’. With a numeric
prefix argument n, it produces n more random numbers using
that value of ‘`M`’.

The `k h` (`calc-shuffle`) command produces a vector of several
random values with no duplicates. The value on the top of the stack
specifies the set from which the random values are drawn, and may be any
of the ‘`M`’ formats described above. The numeric prefix argument
gives the length of the desired list. (If you do not provide a numeric
prefix argument, the length of the list is taken from the top of the
stack, and ‘`M`’ from second-to-top.)

If ‘`M`’ is a floating-point number, zero, or an error form (so
that the random values are being drawn from the set of real numbers)
there is little practical difference between using `k h` and using
`k r` several times. But if the set of possible values consists
of just a few integers, or the elements of a vector, then there is
a very real chance that multiple `k r`’s will produce the same
number more than once. The `k h` command produces a vector whose
elements are always distinct. (Actually, there is a slight exception:
If ‘`M`’ is a vector, no given vector element will be drawn more
than once, but if several elements of ‘`M`’ are equal, they may
each make it into the result vector.)

One use of `k h` is to rearrange a list at random. This happens
if the prefix argument is equal to the number of values in the list:
`[1, 1.5, 2, 2.5, 3] 5 k h` might produce the permuted list
‘`[2.5, 1, 1.5, 3, 2]`’. As a convenient feature, if the argument
n is negative it is replaced by the size of the set represented
by ‘`M`’. Naturally, this is allowed only when ‘`M`’ specifies
a small discrete set of possibilities.

To do the equivalent of `k h` but with duplications allowed,
given ‘`M`’ on the stack and with n just entered as a numeric
prefix, use `v b` to build a vector of copies of ‘`M`’, then use
`V M k r` to “map” the normal `k r` function over the
elements of this vector. See [Vector/Matrix Functions](#Matrix-Functions).

* [Random Number Generator](#Random-Number-Generator)

---

Up: [Random Numbers](#Random-Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 8.5.1 Random Number Generator [¶](#Random-Number-Generator-1)

Calc’s random number generator uses several methods to ensure that
the numbers it produces are highly random. Knuth’s *Art of
Computer Programming*, Volume II, contains a thorough description
of the theory of random number generators and their measurement and
characterization.

If `RandSeed` has no stored value, Calc calls Emacs’s built-in
`random` function to get a stream of random numbers, which it
then treats in various ways to avoid problems inherent in the simple
random number generators that many systems use to implement `random`.

When Calc’s random number generator is first invoked, it “seeds”
the low-level random sequence using the time of day, so that the
random number sequence will be different every time you use Calc.

Since Emacs Lisp doesn’t specify the range of values that will be
returned by its `random` function, Calc exercises the function
several times to estimate the range. When Calc subsequently uses
the `random` function, it takes only 10 bits of the result
near the most-significant end. (It avoids at least the bottom
four bits, preferably more, and also tries to avoid the top two
bits.) This strategy works well with the linear congruential
generators that are typically used to implement `random`.

If `RandSeed` contains an integer, Calc uses this integer to
seed an “additive congruential” method (Knuth’s algorithm 3.2.2A,
computing
‘`X_n-55 - X_n-24`’).
This method expands the seed
value into a large table which is maintained internally; the variable
`RandSeed` is changed from, e.g., 42 to the vector ‘`[42]`’
to indicate that the seed has been absorbed into this table. When
`RandSeed` contains a vector, `k r` and related commands
continue to use the same internal table as last time. There is no
way to extract the complete state of the random number generator
so that you can restart it from any point; you can only restart it
from the same initial seed value. A simple way to restart from the
same seed is to type `s r RandSeed` to get the seed vector,
`v u` to unpack it back into a number, then `s t RandSeed`
to reseed the generator with that number.

Calc uses a “shuffling” method as described in algorithm 3.2.2B
of Knuth. It fills a table with 13 random 10-bit numbers. Then,
to generate a new random number, it uses the previous number to
index into the table, picks the value it finds there as the new
random number, then replaces that table entry with a new value
obtained from a call to the base random number generator (either
the additive congruential generator or the `random` function
supplied by the system). If there are any flaws in the base
generator, shuffling will tend to even them out. But if the system
provides an excellent `random` function, shuffling will not
damage its randomness.

To create a random integer of a certain number of digits, Calc
builds the integer three decimal digits at a time. For each group
of three digits, Calc calls its 10-bit shuffling random number generator
(which returns a value from 0 to 1023); if the random value is 1000
or more, Calc throws it out and tries again until it gets a suitable
value.

To create a random floating-point number with precision p, Calc
simply creates a random p-digit integer and multiplies by
‘`10^-p`’.
The resulting random numbers should be very clean, but note
that relatively small numbers will have few significant random digits.
In other words, with a precision of 12, you will occasionally get
numbers on the order of
‘`10^-9`’
or
‘`10^-10`’,
but those numbers will only have two or three random digits since they
correspond to small integers times
‘`10^-12`’.

To create a random integer in the interval ‘`[0 .. m)`’, Calc
counts the digits in m, creates a random integer with three
additional digits, then reduces modulo m. Unless m is a
power of ten the resulting values will be very slightly biased toward
the lower numbers, but this bias will be less than 0.1%. (For example,
if m is 42, Calc will reduce a random integer less than 100000
modulo 42 to get a result less than 42. It is easy to show that the
numbers 40 and 41 will be only 2380/2381 as likely to result from this
modulo operation as numbers 39 and below.) If m is a power of
ten, however, the numbers should be completely unbiased.

The Gaussian random numbers generated by ‘`random(0.0)`’ use the
“polar” method described in Knuth section 3.4.1C. This method
generates a pair of Gaussian random numbers at a time, so only every
other call to ‘`random(0.0)`’ will require significant calculations.

---

Next: [Probability Distribution Functions](#Probability-Distribution-Functions), Previous: [Random Numbers](#Random-Numbers), Up: [Scientific Functions](#Scientific-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 8.6 Combinatorial Functions [¶](#Combinatorial-Functions-1)

Commands relating to combinatorics and number theory begin with the
`k` key prefix.

The `k g` (`calc-gcd`) [`gcd`] command computes the
Greatest Common Divisor of two integers. It also accepts fractions;
the GCD of two fractions is defined by taking the GCD of the
numerators, and the LCM of the denominators. This definition is
consistent with the idea that ‘`a / gcd(a,x)`’ should yield an
integer for any ‘`a`’ and ‘`x`’. For other types of arguments,
the operation is left in symbolic form.

The `k l` (`calc-lcm`) [`lcm`] command computes the
Least Common Multiple of two integers or fractions. The product of
the LCM and GCD of two numbers is equal to the absolute value of the
product of the numbers.

The `k E` (`calc-extended-gcd`) [`egcd`] command computes
the GCD of two integers ‘`x`’ and ‘`y`’ and returns a vector
‘`[g, a, b]`’ where
‘`g = gcd(x,y) = a x + b y`’.

The `!` (`calc-factorial`) [`fact`] command computes the
factorial of the number at the top of the stack. If the number is an
integer, the result is an exact integer. If the number is an
integer-valued float, the result is a floating-point approximation. If
the number is a non-integral real number, the generalized factorial is used,
as defined by the Euler Gamma function. Please note that computation of
large factorials can be slow; using floating-point format will help
since fewer digits must be maintained. The same is true of many of
the commands in this section.

The `k d` (`calc-double-factorial`) [`dfact`] command
computes the “double factorial” of an integer. For an even integer,
this is the product of even integers from 2 to ‘`N`’. For an odd
integer, this is the product of odd integers from 3 to ‘`N`’. If
the argument is an integer-valued float, the result is a floating-point
approximation. This function is undefined for negative even integers.
The notation ‘`N!!`’ is also recognized for double factorials.

The `k c` (`calc-choose`) [`choose`] command computes the
binomial coefficient ‘`N`’-choose-‘`M`’, where ‘`M`’ is the number
on the top of the stack and ‘`N`’ is second-to-top. If both arguments
are integers, the result is an exact integer. Otherwise, the result is a
floating-point approximation. The binomial coefficient is defined for all
real numbers by
‘`N! / M! (N-M)!`’.

The `H k c` (`calc-perm`) [`perm`] command computes the
number-of-permutations function ‘`N! / (N-M)!`’.

The `k b` (`calc-bernoulli-number`) [`bern`] command
computes a given Bernoulli number. The value at the top of the stack
is a nonnegative integer ‘`n`’ that specifies which Bernoulli number
is desired. The `H k b` command computes a Bernoulli polynomial,
taking ‘`n`’ from the second-to-top position and ‘`x`’ from the
top of the stack. If ‘`x`’ is a variable or formula the result is
a polynomial in ‘`x`’; if ‘`x`’ is a number the result is a number.

The `k e` (`calc-euler-number`) [`euler`] command similarly
computes an Euler number, and `H k e` computes an Euler polynomial.
Bernoulli and Euler numbers occur in the Taylor expansions of several
functions.

The `k s` (`calc-stirling-number`) [`stir1`] command
computes a Stirling number of the first
kind,
given two integers ‘`n`’ and ‘`m`’ on the stack. The `H k s`
[`stir2`] command computes a Stirling number of the second
kind.
These are the number of ‘`m`’-cycle permutations of ‘`n`’ objects,
and the number of ways to partition ‘`n`’ objects into ‘`m`’
non-empty sets, respectively.

The `k p` (`calc-prime-test`) command checks if the integer on
the top of the stack is prime. For integers less than eight million, the
answer is always exact and reasonably fast. For larger integers, a
probabilistic method is used (see Knuth vol. II, section 4.5.4, algorithm P).
The number is first checked against small prime factors (up to 13). Then,
any number of iterations of the algorithm are performed. Each step either
discovers that the number is non-prime, or substantially increases the
certainty that the number is prime. After a few steps, the chance that
a number was mistakenly described as prime will be less than one percent.
(Indeed, this is a worst-case estimate of the probability; in practice
even a single iteration is quite reliable.) After the `k p` command,
the number will be reported as definitely prime or non-prime if possible,
or otherwise “probably” prime with a certain probability of error.

The normal `k p` command performs one iteration of the primality
test. Pressing `k p` repeatedly for the same integer will perform
additional iterations. Also, `k p` with a numeric prefix performs
the specified number of iterations. There is also an algebraic function
‘`prime(n)`’ or ‘`prime(n,iters)`’ which returns 1 if ‘`n`’
is (probably) prime and 0 if not.

The `k f` (`calc-prime-factors`) [`prfac`] command
attempts to decompose an integer into its prime factors. For numbers up
to 25 million, the answer is exact although it may take some time. The
result is a vector of the prime factors in increasing order. For larger
inputs, prime factors above 5000 may not be found, in which case the
last number in the vector will be an unfactored integer greater than 25
million (with a warning message). For negative integers, the first
element of the list will be *-1*. For inputs *-1*, *0*, and
*1*, the result is a list of the same number.

The `k n` (`calc-next-prime`) [`nextprime`] command finds
the next prime above a given number. Essentially, it searches by calling
`calc-prime-test` on successive integers until it finds one that
passes the test. This is quite fast for integers less than eight million,
but once the probabilistic test comes into play the search may be rather
slow. Ordinarily this command stops for any prime that passes one iteration
of the primality test. With a numeric prefix argument, a number must pass
the specified number of iterations before the search stops. (This only
matters when searching above eight million.) You can always use additional
`k p` commands to increase your certainty that the number is indeed
prime.

The `I k n` (`calc-prev-prime`) [`prevprime`] command
analogously finds the next prime less than a given number.

The `k t` (`calc-totient`) [`totient`] command computes the
Euler “totient”
function,
the number of integers less than ‘`n`’ which
are relatively prime to ‘`n`’.

The `k m` (`calc-moebius`) [`moebius`] command computes the
Möbius μ function. If the input number is a product of ‘`k`’
distinct factors, this is ‘`(-1)^k`’. If the input number has any
duplicate factors (i.e., can be divided by the same prime more than once),
the result is zero.

---

Previous: [Combinatorial Functions](#Combinatorial-Functions), Up: [Scientific Functions](#Scientific-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 8.7 Probability Distribution Functions [¶](#Probability-Distribution-Functions-1)

The functions in this section compute various probability distributions.
For continuous distributions, this is the integral of the probability
density function from ‘`x`’ to infinity. (These are the “upper
tail” distribution functions; there are also corresponding “lower
tail” functions which integrate from minus infinity to ‘`x`’.)
For discrete distributions, the upper tail function gives the sum
from ‘`x`’ to infinity; the lower tail function gives the sum
from minus infinity up to, but not including, ‘`x`’.

To integrate from ‘`x`’ to ‘`y`’, just use the distribution
function twice and subtract. For example, the probability that a
Gaussian random variable with mean 2 and standard deviation 1 will
lie in the range from 2.5 to 2.8 is ‘`utpn(2.5,2,1) - utpn(2.8,2,1)`’
(“the probability that it is greater than 2.5, but not greater than 2.8”),
or equivalently ‘`ltpn(2.8,2,1) - ltpn(2.5,2,1)`’.

The `k B` (`calc-utpb`) [`utpb`] function uses the
binomial distribution. Push the parameters n, p, and
then x onto the stack; the result (‘`utpb(x,n,p)`’) is the
probability that an event will occur x or more times out
of n trials, if its probability of occurring in any given
trial is p. The `I k B` [`ltpb`] function is
the probability that the event will occur fewer than x times.

The other probability distribution functions similarly take the
form `k X` (`calc-utpx`) [`utpx`]
and `I k X` [`ltpx`], for various letters
x. The arguments to the algebraic functions are the value of
the random variable first, then whatever other parameters define the
distribution. Note these are among the few Calc functions where the
order of the arguments in algebraic form differs from the order of
arguments as found on the stack. (The random variable comes last on
the stack, so that you can type, e.g., `2 RET 1 RET 2.5
k N M-RET DEL 2.8 k N -`, using `M-RET DEL` to
recover the original arguments but substitute a new value for ‘`x`’.)

The ‘`utpc(x,v)`’ function uses the chi-square distribution with
‘`v`’
degrees of freedom. It is the probability that a model is
correct if its chi-square statistic is ‘`x`’.

The ‘`utpf(F,v1,v2)`’ function uses the F distribution, used in
various statistical tests. The parameters
‘`v1`’
and
‘`v2`’
are the degrees of freedom in the numerator and denominator,
respectively, used in computing the statistic ‘`F`’.

The ‘`utpn(x,m,s)`’ function uses a normal (Gaussian) distribution
with mean ‘`m`’ and standard deviation
‘`s`’.
It is the probability that such a normal-distributed random variable
would exceed ‘`x`’.

The ‘`utpp(n,x)`’ function uses a Poisson distribution with
mean ‘`x`’. It is the probability that ‘`n`’ or more such
Poisson random events will occur.

The ‘`utpt(t,v)`’ function uses the Student’s “t” distribution
with
‘`v`’
degrees of freedom. It is the probability that a
t-distributed random variable will be greater than ‘`t`’.
(Note: This computes the distribution function
‘`A(t|v)`’
where
‘`A(0|v) = 1`’
and
‘`A(inf|v) -> 0`’.
The `UTPT` operation on the HP-48 uses a different definition which
returns half of Calc’s value: ‘`UTPT(t,v) = .5*utpt(t,v)`’.)

While Calc does not provide inverses of the probability distribution
functions, the `a R` command can be used to solve for the inverse.
Since the distribution functions are monotonic, `a R` is guaranteed
to be able to find a solution given any initial guess.
See [Numerical Solutions](#Numerical-Solutions).

---

Next: [Algebra](#Algebra), Previous: [Scientific Functions](#Scientific-Functions), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 9 Vector/Matrix Functions [¶](#Vector_002fMatrix-Functions)

Many of the commands described here begin with the `v` prefix.
(For convenience, the shift-`V` prefix is equivalent to `v`.)
The commands usually apply to both plain vectors and matrices; some
apply only to matrices or only to square matrices. If the argument
has the wrong dimensions the operation is left in symbolic form.

Vectors are entered and displayed using ‘`[a,b,c]`’ notation.
Matrices are vectors of which all elements are vectors of equal length.
(Though none of the standard Calc commands use this concept, a
three-dimensional matrix or rank-3 tensor could be defined as a
vector of matrices, and so on.)

* [Packing and Unpacking](#Packing-and-Unpacking)
* [Building Vectors](#Building-Vectors)
* [Extracting Vector Elements](#Extracting-Elements)
* [Manipulating Vectors](#Manipulating-Vectors)
* [Vector and Matrix Arithmetic](#Vector-and-Matrix-Arithmetic)
* [Set Operations using Vectors](#Set-Operations)
* [Statistical Operations on Vectors](#Statistical-Operations)
* [Reducing and Mapping Vectors](#Reducing-and-Mapping)
* [Vector and Matrix Display Formats](#Vector-and-Matrix-Formats)

---

Next: [Building Vectors](#Building-Vectors), Up: [Vector/Matrix Functions](#Matrix-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 9.1 Packing and Unpacking [¶](#Packing-and-Unpacking-1)

Calc’s “pack” and “unpack” commands collect stack entries to build
composite objects such as vectors and complex numbers. They are
described in this chapter because they are most often used to build
vectors.

The `v p` (`calc-pack`) [`pack`] command collects several
elements from the stack into a matrix, complex number, HMS form, error
form, etc. It uses a numeric prefix argument to specify the kind of
object to be built; this argument is referred to as the “packing mode.”
If the packing mode is a nonnegative integer, a vector of that
length is created. For example, `C-u 5 v p` will pop the top
five stack elements and push back a single vector of those five
elements. (`C-u 0 v p` simply creates an empty vector.)

The same effect can be had by pressing `[` to push an incomplete
vector on the stack, using `TAB` (`calc-roll-down`) to sneak
the incomplete object up past a certain number of elements, and
then pressing `]` to complete the vector.

Negative packing modes create other kinds of composite objects:

-1
:   Two values are collected to build a complex number. For example,
    `5 RET 7 C-u -1 v p` creates the complex number
    ‘`(5, 7)`’. The result is always a rectangular complex
    number. The two input values must both be real numbers,
    i.e., integers, fractions, or floats. If they are not, Calc
    will instead build a formula like ‘`a + (0, 1) b`’. (The
    other packing modes also create a symbolic answer if the
    components are not suitable.)

-2
:   Two values are collected to build a polar complex number.
    The first is the magnitude; the second is the phase expressed
    in either degrees or radians according to the current angular
    mode.

-3
:   Three values are collected into an HMS form. The first
    two values (hours and minutes) must be integers or
    integer-valued floats. The third value may be any real
    number.

-4
:   Two values are collected into an error form. The inputs
    may be real numbers or formulas.

-5
:   Two values are collected into a modulo form. The inputs
    must be real numbers.

-6
:   Two values are collected into the interval ‘`[a .. b]`’.
    The inputs may be real numbers, HMS or date forms, or formulas.

-7
:   Two values are collected into the interval ‘`[a .. b)`’.

-8
:   Two values are collected into the interval ‘`(a .. b]`’.

-9
:   Two values are collected into the interval ‘`(a .. b)`’.

-10
:   Two integer values are collected into a fraction.

-11
:   Two values are collected into a floating-point number.
    The first is the mantissa; the second, which must be an
    integer, is the exponent. The result is the mantissa
    times ten to the power of the exponent.

-12
:   This is treated the same as *-11* by the `v p` command.
    When unpacking, *-12* specifies that a floating-point mantissa
    is desired.

-13
:   A real number is converted into a date form.

-14
:   Three numbers (year, month, day) are packed into a pure date form.

-15
:   Six numbers are packed into a date/time form.

With any of the two-input negative packing modes, either or both
of the inputs may be vectors. If both are vectors of the same
length, the result is another vector made by packing corresponding
elements of the input vectors. If one input is a vector and the
other is a plain number, the number is packed along with each vector
element to produce a new vector. For example, `C-u -4 v p`
could be used to convert a vector of numbers and a vector of errors
into a single vector of error forms; `C-u -5 v p` could convert
a vector of numbers and a single number M into a vector of
numbers modulo M.

If you don’t give a prefix argument to `v p`, it takes
the packing mode from the top of the stack. The elements to
be packed then begin at stack level 2. Thus
`1 RET 2 RET 4 n v p` is another way to
enter the error form ‘`1 +/- 2`’.

If the packing mode taken from the stack is a vector, the result is a
matrix with the dimensions specified by the elements of the vector,
which must each be integers. For example, if the packing mode is
‘`[2, 3]`’, then six numbers will be taken from the stack and
returned in the form ‘`[[a, b, c], [d, e, f]]`’.

If any elements of the vector are negative, other kinds of
packing are done at that level as described above. For
example, ‘`[2, 3, -4]`’ takes 12 objects and creates a
2x3
matrix of error forms: ‘`[[a +/- b, c +/- d ... ]]`’.
Also, ‘`[-4, -10]`’ will convert four integers into an
error form consisting of two fractions: ‘`a:b +/- c:d`’.

There is an equivalent algebraic function,
‘`pack(mode, items)`’ where mode is a
packing mode (an integer or a vector of integers) and items
is a vector of objects to be packed (re-packed, really) according
to that mode. For example, ‘`pack([3, -4], [a,b,c,d,e,f])`’
yields ‘`[a +/- b, c +/- d, e +/- f]`’. The function is
left in symbolic form if the packing mode is invalid, or if the
number of data items does not match the number of items required
by the mode.

The `v u` (`calc-unpack`) command takes the vector, complex
number, HMS form, or other composite object on the top of the stack and
“unpacks” it, pushing each of its elements onto the stack as separate
objects. Thus, it is the “inverse” of `v p`. If the value
at the top of the stack is a formula, `v u` unpacks it by pushing
each of the arguments of the top-level operator onto the stack.

You can optionally give a numeric prefix argument to `v u`
to specify an explicit (un)packing mode. If the packing mode is
negative and the input is actually a vector or matrix, the result
will be two or more similar vectors or matrices of the elements.
For example, given the vector ‘`[a +/- b, c^2, d +/- 7]`’,
the result of `C-u -4 v u` will be the two vectors
‘`[a, c^2, d]`’ and ‘`[b, 0, 7]`’.

Note that the prefix argument can have an effect even when the input is
not a vector. For example, if the input is the number *-5*, then
`c-u -1 v u` yields *-5* and 0 (the components of *-5*
when viewed as a rectangular complex number); `C-u -2 v u` yields 5
and 180 (assuming Degrees mode); and `C-u -10 v u` yields *-5*
and 1 (the numerator and denominator of *-5*, viewed as a rational
number). Plain `v u` with this input would complain that the input
is not a composite object.

Unpacking mode *-11* converts a float into an integer mantissa and
an integer exponent, where the mantissa is not divisible by 10
(except that 0.0 is represented by a mantissa and exponent of 0).
Unpacking mode *-12* converts a float into a floating-point mantissa
and integer exponent, where the mantissa (for non-zero numbers)
is guaranteed to lie in the range [1 .. 10). In both cases,
the mantissa is shifted left or right (and the exponent adjusted
to compensate) in order to satisfy these constraints.

Positive unpacking modes are treated differently than for `v p`.
A mode of 1 is much like plain `v u` with no prefix argument,
except that in addition to the components of the input object,
a suitable packing mode to re-pack the object is also pushed.
Thus, `C-u 1 v u` followed by `v p` will re-build the
original object.

A mode of 2 unpacks two levels of the object; the resulting
re-packing mode will be a vector of length 2. This might be used
to unpack a matrix, say, or a vector of error forms. Higher
unpacking modes unpack the input even more deeply.

There are two algebraic functions analogous to `v u`.
The ‘`unpack(mode, item)`’ function unpacks the
item using the given mode, returning the result as
a vector of components. Here the mode must be an
integer, not a vector. For example, ‘`unpack(-4, a +/- b)`’
returns ‘`[a, b]`’, as does ‘`unpack(1, a +/- b)`’.

The `unpackt` function is like `unpack` but instead
of returning a simple vector of items, it returns a vector of
two things: The mode, and the vector of items. For example,
‘`unpackt(1, 2:3 +/- 1:4)`’ returns ‘`[-4, [2:3, 1:4]]`’,
and ‘`unpackt(2, 2:3 +/- 1:4)`’ returns ‘`[[-4, -10], [2, 3, 1, 4]]`’.
The identity for re-building the original object is
‘`apply(pack, unpackt(n, x)) = x`’. (The
`apply` function builds a function call given the function
name and a vector of arguments.)

Subscript notation is a useful way to extract a particular part
of an object. For example, to get the numerator of a rational
number, you can use ‘`unpack(-10, x)_1`’.

---

Next: [Extracting Vector Elements](#Extracting-Elements), Previous: [Packing and Unpacking](#Packing-and-Unpacking), Up: [Vector/Matrix Functions](#Matrix-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 9.2 Building Vectors [¶](#Building-Vectors-1)

Vectors and matrices can be added,
subtracted, multiplied, and divided; see [Basic Arithmetic](#Basic-Arithmetic).

The `|` (`calc-concat`) [`vconcat`] command “concatenates” two vectors
into one. For example, after `[ 1 , 2 ] [ 3 , 4 ] |`, the stack
will contain the single vector ‘`[1, 2, 3, 4]`’. If the arguments
are matrices, the rows of the first matrix are concatenated with the
rows of the second. (In other words, two matrices are just two vectors
of row-vectors as far as `|` is concerned.)

If either argument to `|` is a scalar (a non-vector), it is treated
like a one-element vector for purposes of concatenation: `1 [ 2 , 3 ] |`
produces the vector ‘`[1, 2, 3]`’. Likewise, if one argument is a
matrix and the other is a plain vector, the vector is treated as a
one-row matrix.

The `H |` (`calc-append`) [`append`] command concatenates
two vectors without any special cases. Both inputs must be vectors.
Whether or not they are matrices is not taken into account. If either
argument is a scalar, the `append` function is left in symbolic form.
See also `cons` and `rcons` below.

The `I |` and `H I |` commands are similar, but they use their
two stack arguments in the opposite order. Thus `I |` is equivalent
to `TAB |`, but possibly more convenient and also a bit faster.

The `v d` (`calc-diag`) [`diag`] function builds a diagonal
square matrix. The optional numeric prefix gives the number of rows
and columns in the matrix. If the value at the top of the stack is a
vector, the elements of the vector are used as the diagonal elements; the
prefix, if specified, must match the size of the vector. If the value on
the stack is a scalar, it is used for each element on the diagonal, and
the prefix argument is required.

To build a constant square matrix, e.g., a
3x3
matrix filled with ones, use `0 M-3 v d 1 +`, i.e., build a zero
matrix first and then add a constant value to that matrix. (Another
alternative would be to use `v b` and `v a`; see below.)

The `v i` (`calc-ident`) [`idn`] function builds an identity
matrix of the specified size. It is a convenient form of `v d`
where the diagonal element is always one. If no prefix argument is given,
this command prompts for one.

In algebraic notation, ‘`idn(a,n)`’ acts much like ‘`diag(a,n)`’,
except that ‘`a`’ is required to be a scalar (non-vector) quantity.
If ‘`n`’ is omitted, ‘`idn(a)`’ represents ‘`a`’ times an
identity matrix of unknown size. Calc can operate algebraically on
such generic identity matrices, and if one is combined with a matrix
whose size is known, it is converted automatically to an identity
matrix of a suitable matching size. The `v i` command with an
argument of zero creates a generic identity matrix, ‘`idn(1)`’.
Note that in dimensioned Matrix mode (see [Matrix and Scalar Modes](#Matrix-Mode)), generic
identity matrices are immediately expanded to the current default
dimensions.

The `v x` (`calc-index`) [`index`] function builds a vector
of consecutive integers from 1 to n, where n is the numeric
prefix argument. If you do not provide a prefix argument, you will be
prompted to enter a suitable number. If n is negative, the result
is a vector of negative integers from n to *-1*.

With a prefix argument of just `C-u`, the `v x` command takes
three values from the stack: n, start, and incr (with
incr at top-of-stack). Counting starts at start and increases
by incr for successive vector elements. If start or n
is in floating-point format, the resulting vector elements will also be
floats. Note that start and incr may in fact be any kind
of numbers or formulas.

When start and incr are specified, a negative n has a
different interpretation: It causes a geometric instead of arithmetic
sequence to be generated. For example, ‘`index(-3, a, b)`’ produces
‘`[a, a b, a b^2]`’. If you omit incr in the algebraic form,
‘`index(n, start)`’, the default value for incr
is one for positive n or two for negative n.

The `v b` (`calc-build-vector`) [`cvec`] function builds a
vector of n copies of the value on the top of the stack, where n
is the numeric prefix argument. In algebraic formulas, ‘`cvec(x,n,m)`’
can also be used to build an n-by-m matrix of copies of x.
(Interactively, just use `v b` twice: once to build a row, then again
to build a matrix of copies of that row.)

The `v h` (`calc-head`) [`head`] function returns the first
element of a vector. The `I v h` (`calc-tail`) [`tail`]
function returns the vector with its first element removed. In both
cases, the argument must be a non-empty vector.

The `v k` (`calc-cons`) [`cons`] function takes a value h
and a vector t from the stack, and produces the vector whose head is
h and whose tail is t. This is similar to `|`, except
if h is itself a vector, `|` will concatenate the two vectors
whereas `cons` will insert h at the front of the vector t.

Each of these three functions also accepts the Hyperbolic flag [`rhead`,
`rtail`, `rcons`] in which case t instead represents
the *last* single element of the vector, with h
representing the remainder of the vector. Thus the vector
‘`[a, b, c, d] = cons(a, [b, c, d]) = rcons([a, b, c], d)`’.
Also, ‘`head([a, b, c, d]) = a`’, ‘`tail([a, b, c, d]) = [b, c, d]`’,
‘`rhead([a, b, c, d]) = [a, b, c]`’, and ‘`rtail([a, b, c, d]) = d`’.

---

Next: [Manipulating Vectors](#Manipulating-Vectors), Previous: [Building Vectors](#Building-Vectors), Up: [Vector/Matrix Functions](#Matrix-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 9.3 Extracting Vector Elements [¶](#Extracting-Vector-Elements)

The `v r` (`calc-mrow`) [`mrow`] command extracts one row of
the matrix on the top of the stack, or one element of the plain vector on
the top of the stack. The row or element is specified by the numeric
prefix argument; the default is to prompt for the row or element number.
The matrix or vector is replaced by the specified row or element in the
form of a vector or scalar, respectively.

With a prefix argument of `C-u` only, `v r` takes the index of
the element or row from the top of the stack, and the vector or matrix
from the second-to-top position. If the index is itself a vector of
integers, the result is a vector of the corresponding elements of the
input vector, or a matrix of the corresponding rows of the input matrix.
This command can be used to obtain any permutation of a vector.

With `C-u`, if the index is an interval form with integer components,
it is interpreted as a range of indices and the corresponding subvector or
submatrix is returned.

Subscript notation in algebraic formulas (‘`a_b`’) stands for the
Calc function `subscr`, which is synonymous with `mrow`.
Thus, ‘`[x, y, z]_k`’ produces ‘`x`’, ‘`y`’, or ‘`z`’ if
‘`k`’ is one, two, or three, respectively. A double subscript
(‘`M_i_j`’, equivalent to ‘`subscr(subscr(M, i), j)`’) will
access the element at row ‘`i`’, column ‘`j`’ of a matrix.
The `a _` (`calc-subscript`) command creates a subscript
formula ‘`a_b`’ out of two stack entries. (It is on the `a`
“algebra” prefix because subscripted variables are often used
purely as an algebraic notation.)

Given a negative prefix argument, `v r` instead deletes one row or
element from the matrix or vector on the top of the stack. Thus
`C-u 2 v r` replaces a matrix with its second row, but `C-u -2 v r`
replaces the matrix with the same matrix with its second row removed.
In algebraic form this function is called `mrrow`.

Given a prefix argument of zero, `v r` extracts the diagonal elements
of a square matrix in the form of a vector. In algebraic form this
function is called `getdiag`.

The `v c` (`calc-mcol`) [`mcol` or `mrcol`] command is
the analogous operation on columns of a matrix. Given a plain vector
it extracts (or removes) one element, just like `v r`. If the
index in `C-u v c` is an interval or vector and the argument is a
matrix, the result is a submatrix with only the specified columns
retained (and possibly permuted in the case of a vector index).

To extract a matrix element at a given row and column, use `v r` to
extract the row as a vector, then `v c` to extract the column element
from that vector. In algebraic formulas, it is often more convenient to
use subscript notation: ‘`m_i_j`’ gives row ‘`i`’, column ‘`j`’
of matrix ‘`m`’.

The `v s` (`calc-subvector`) [`subvec`] command extracts
a subvector of a vector. The arguments are the vector, the starting
index, and the ending index, with the ending index in the top-of-stack
position. The starting index indicates the first element of the vector
to take. The ending index indicates the first element *past* the
range to be taken. Thus, ‘`subvec([a, b, c, d, e], 2, 4)`’ produces
the subvector ‘`[b, c]`’. You could get the same result using
‘`mrow([a, b, c, d, e], [2 .. 4))`’.

If either the start or the end index is zero or negative, it is
interpreted as relative to the end of the vector. Thus
‘`subvec([a, b, c, d, e], 2, -2)`’ also produces ‘`[b, c]`’. In
the algebraic form, the end index can be omitted in which case it
is taken as zero, i.e., elements from the starting element to the
end of the vector are used. The infinity symbol, `inf`, also
has this effect when used as the ending index.

With the Inverse flag, `I v s` [`rsubvec`] removes a subvector
from a vector. The arguments are interpreted the same as for the
normal `v s` command. Thus, ‘`rsubvec([a, b, c, d, e], 2, 4)`’
produces ‘`[a, d, e]`’. It is always true that `subvec` and
`rsubvec` return complementary parts of the input vector.

See [Selecting Sub-Formulas](#Selecting-Subformulas), for an alternative way to operate on
vectors one element at a time.

---

Next: [Vector and Matrix Arithmetic](#Vector-and-Matrix-Arithmetic), Previous: [Extracting Vector Elements](#Extracting-Elements), Up: [Vector/Matrix Functions](#Matrix-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 9.4 Manipulating Vectors [¶](#Manipulating-Vectors-1)

The `v l` (`calc-vlength`) [`vlen`] command computes the
length of a vector. The length of a non-vector is considered to be zero.
Note that matrices are just vectors of vectors for the purposes of this
command.

With the Hyperbolic flag, `H v l` [`mdims`] computes a vector
of the dimensions of a vector, matrix, or higher-order object. For
example, ‘`mdims([[a,b,c],[d,e,f]])`’ returns ‘`[2, 3]`’ since
its argument is a
2x3
matrix.

The `v f` (`calc-vector-find`) [`find`] command searches
along a vector for the first element equal to a given target. The target
is on the top of the stack; the vector is in the second-to-top position.
If a match is found, the result is the index of the matching element.
Otherwise, the result is zero. The numeric prefix argument, if given,
allows you to select any starting index for the search.

The `v a` (`calc-arrange-vector`) [`arrange`] command
rearranges a vector to have a certain number of columns and rows. The
numeric prefix argument specifies the number of columns; if you do not
provide an argument, you will be prompted for the number of columns.
The vector or matrix on the top of the stack is *flattened* into a
plain vector. If the number of columns is nonzero, this vector is
then formed into a matrix by taking successive groups of n elements.
If the number of columns does not evenly divide the number of elements
in the vector, the last row will be short and the result will not be
suitable for use as a matrix. For example, with the matrix
‘`[[1, 2], [3, 4]]`’ on the stack, `v a 4` produces
‘`[[1, 2, 3, 4]]`’ (a
1x4
matrix), `v a 1` produces ‘`[[1], [2], [3], [4]]`’ (a
4x1
matrix), `v a 2` produces ‘`[[1, 2], [3, 4]]`’ (the original
2x2
matrix), `v a 3` produces ‘`[[1, 2, 3], [4]]`’ (not a
matrix), and `v a 0` produces the flattened list
‘`[1, 2, 3, 4]`’.

The `V S` (`calc-sort`) [`sort`] command sorts the elements of
a vector into increasing order. Real numbers, real infinities, and
constant interval forms come first in this ordering; next come other
kinds of numbers, then variables (in alphabetical order), then finally
come formulas and other kinds of objects; these are sorted according
to a kind of lexicographic ordering with the useful property that
one vector is less or greater than another if the first corresponding
unequal elements are less or greater, respectively. Since quoted strings
are stored by Calc internally as vectors of ASCII character codes
(see [Strings](#Strings)), this means vectors of strings are also sorted into
alphabetical order by this command.

The `I V S` [`rsort`] command sorts a vector into decreasing order.

The `V G` (`calc-grade`) [`grade`, `rgrade`] command
produces an index table or permutation vector which, if applied to the
input vector (as the index of `C-u v r`, say), would sort the vector.
A permutation vector is just a vector of integers from 1 to n, where
each integer occurs exactly once. One application of this is to sort a
matrix of data rows using one column as the sort key; extract that column,
grade it with `V G`, then use the result to reorder the original matrix
with `C-u v r`. Another interesting property of the `V G` command
is that, if the input is itself a permutation vector, the result will
be the inverse of the permutation. The inverse of an index table is
a rank table, whose kth element says where the kth original
vector element will rest when the vector is sorted. To get a rank
table, just use `V G V G`.

With the Inverse flag, `I V G` produces an index table that would
sort the input into decreasing order. Note that `V S` and `V G`
use a “stable” sorting algorithm, i.e., any two elements which are equal
will not be moved out of their original order. Generally there is no way
to tell with `V S`, since two elements which are equal look the same,
but with `V G` this can be an important issue. In the matrix-of-rows
example, suppose you have names and telephone numbers as two columns and
you wish to sort by phone number primarily, and by name when the numbers
are equal. You can sort the data matrix by names first, and then again
by phone numbers. Because the sort is stable, any two rows with equal
phone numbers will remain sorted by name even after the second sort.

The `V H` (`calc-histogram`) [`histogram`] command builds a
histogram of a vector of numbers. Vector elements are assumed to be
integers or real numbers in the range [0..n) for some “number of
bins” n, which is the numeric prefix argument given to the
command. The result is a vector of n counts of how many times
each value appeared in the original vector. Non-integers in the input
are rounded down to integers. Any vector elements outside the specified
range are ignored. (You can tell if elements have been ignored by noting
that the counts in the result vector don’t add up to the length of the
input vector.)

If no prefix is given, then you will be prompted for a vector which
will be used to determine the bins. (If a positive integer is given at
this prompt, it will be still treated as if it were given as a
prefix.) Each bin will consist of the interval of numbers closest to
the corresponding number of this new vector; if the vector
‘`[a, b, c, ...]`’ is entered at the prompt, the bins will be
‘`(-inf, (a+b)/2]`’, ‘`((a+b)/2, (b+c)/2]`’, etc. The result of
this command will be a vector counting how many elements of the
original vector are in each bin.

The result will then be a vector with the same length as this new vector;
each element of the new vector will be replaced by the number of
elements of the original vector which are closest to it.

With the Hyperbolic flag, `H V H` pulls two vectors from the stack.
The second-to-top vector is the list of numbers as before. The top
vector is an equal-sized list of “weights” to attach to the elements
of the data vector. For example, if the first data element is 4.2 and
the first weight is 10, then 10 will be added to bin 4 of the result
vector. Without the hyperbolic flag, every element has a weight of one.

The `v t` (`calc-transpose`) [`trn`] command computes
the transpose of the matrix at the top of the stack. If the argument
is a plain vector, it is treated as a row vector and transposed into
a one-column matrix.

The `v v` (`calc-reverse-vector`) [`rev`] command reverses
a vector end-for-end. Given a matrix, it reverses the order of the rows.
(To reverse the columns instead, just use `v t v v v t`. The same
principle can be used to apply other vector commands to the columns of
a matrix.)

The `v m` (`calc-mask-vector`) [`vmask`] command uses
one vector as a mask to extract elements of another vector. The mask
is in the second-to-top position; the target vector is on the top of
the stack. These vectors must have the same length. The result is
the same as the target vector, but with all elements which correspond
to zeros in the mask vector deleted. Thus, for example,
‘`vmask([1, 0, 1, 0, 1], [a, b, c, d, e])`’ produces ‘`[a, c, e]`’.
See [Logical Operations](#Logical-Operations).

The `v e` (`calc-expand-vector`) [`vexp`] command
expands a vector according to another mask vector. The result is a
vector the same length as the mask, but with nonzero elements replaced
by successive elements from the target vector. The length of the target
vector is normally the number of nonzero elements in the mask. If the
target vector is longer, its last few elements are lost. If the target
vector is shorter, the last few nonzero mask elements are left
unreplaced in the result. Thus ‘`vexp([2, 0, 3, 0, 7], [a, b])`’
produces ‘`[a, 0, b, 0, 7]`’.

With the Hyperbolic flag, `H v e` takes a filler value from the
top of the stack; the mask and target vectors come from the third and
second elements of the stack. This filler is used where the mask is
zero: ‘`vexp([2, 0, 3, 0, 7], [a, b], z)`’ produces
‘`[a, z, c, z, 7]`’. If the filler value is itself a vector,
then successive values are taken from it, so that the effect is to
interleave two vectors according to the mask:
‘`vexp([2, 0, 3, 7, 0, 0], [a, b], [x, y])`’ produces
‘`[a, x, b, 7, y, 0]`’.

Another variation on the masking idea is to combine ‘`[a, b, c, d, e]`’
with the mask ‘`[1, 0, 1, 0, 1]`’ to produce ‘`[a, 0, c, 0, e]`’.
You can accomplish this with `V M a &`, mapping the logical “and”
operation across the two vectors. See [Logical Operations](#Logical-Operations). Note that
the `? :` operation also discussed there allows other types of
masking using vectors.

---

Next: [Set Operations using Vectors](#Set-Operations), Previous: [Manipulating Vectors](#Manipulating-Vectors), Up: [Vector/Matrix Functions](#Matrix-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 9.5 Vector and Matrix Arithmetic [¶](#Vector-and-Matrix-Arithmetic-1)

Basic arithmetic operations like addition and multiplication are defined
for vectors and matrices as well as for numbers. Division of matrices, in
the sense of multiplying by the inverse, is supported. (Division by a
matrix actually uses LU-decomposition for greater accuracy and speed.)
See [Basic Arithmetic](#Basic-Arithmetic).

The following functions are applied element-wise if their arguments are
vectors or matrices: `change-sign`, `conj`, `arg`,
`re`, `im`, `polar`, `rect`, `clean`,
`float`, `frac`. See [Index of Algebraic Functions](#Function-Index).

The `V J` (`calc-conj-transpose`) [`ctrn`] command computes
the conjugate transpose of its argument, i.e., ‘`conj(trn(x))`’.

The `A` (`calc-abs`) [`abs`] command computes the
Frobenius norm of a vector or matrix argument. This is the square
root of the sum of the squares of the absolute values of the
elements of the vector or matrix. If the vector is interpreted as
a point in two- or three-dimensional space, this is the distance
from that point to the origin.

The `v n` (`calc-rnorm`) [`rnorm`] command computes the
infinity-norm of a vector, or the row norm of a matrix. For a plain
vector, this is the maximum of the absolute values of the elements. For
a matrix, this is the maximum of the row-absolute-value-sums, i.e., of
the sums of the absolute values of the elements along the various rows.

The `V N` (`calc-cnorm`) [`cnorm`] command computes
the one-norm of a vector, or column norm of a matrix. For a plain
vector, this is the sum of the absolute values of the elements.
For a matrix, this is the maximum of the column-absolute-value-sums.
General ‘`k`’-norms for ‘`k`’ other than one or infinity are
not provided. However, the 2-norm (or Frobenius norm) is provided for
vectors by the `A` (`calc-abs`) command.

The `V C` (`calc-cross`) [`cross`] command computes the
right-handed cross product of two vectors, each of which must have
exactly three elements.

The `&` (`calc-inv`) [`inv`] command computes the
inverse of a square matrix. If the matrix is singular, the inverse
operation is left in symbolic form. Matrix inverses are recorded so
that once an inverse (or determinant) of a particular matrix has been
computed, the inverse and determinant of the matrix can be recomputed
quickly in the future.

If the argument to `&` is a plain number ‘`x`’, this
command simply computes ‘`1/x`’. This is okay, because the
‘`/`’ operator also does a matrix inversion when dividing one
by a matrix.

The `V D` (`calc-mdet`) [`det`] command computes the
determinant of a square matrix.

The `V L` (`calc-mlud`) [`lud`] command computes the
LU decomposition of a matrix. The result is a list of three matrices
which, when multiplied together left-to-right, form the original matrix.
The first is a permutation matrix that arises from pivoting in the
algorithm, the second is lower-triangular with ones on the diagonal,
and the third is upper-triangular.

The `V T` (`calc-mtrace`) [`tr`] command computes the
trace of a square matrix. This is defined as the sum of the diagonal
elements of the matrix.

The `V K` (`calc-kron`) [`kron`] command computes
the Kronecker product of two matrices.

---

Next: [Statistical Operations on Vectors](#Statistical-Operations), Previous: [Vector and Matrix Arithmetic](#Vector-and-Matrix-Arithmetic), Up: [Vector/Matrix Functions](#Matrix-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
