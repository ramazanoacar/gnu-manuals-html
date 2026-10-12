If we really want the `+` and `*` keys to operate naturally
on power series, we should put these rules in `EvalRules`. For
testing purposes, it is better to put them in a different variable,
say, `O`, first.

The first rule just expands products of sums so that the rest of the
rules can assume they have an expanded-out polynomial to work with.
Note that this rule does not mention ‘`O`’ at all, so it will
apply to any product-of-sum it encounters—this rule may surprise
you if you put it into `EvalRules`!

In the second rule, the sum of two O’s is changed to the smaller O.
The optional constant coefficients are there mostly so that
‘`O(x^2) - O(x^3)`’ and ‘`O(x^3) - O(x^2)`’ are handled
as well as ‘`O(x^2) + O(x^3)`’.

The third rule absorbs higher powers of ‘`x`’ into O’s.

The fourth rule says that a constant times a negligible quantity
is still negligible. (This rule will also match ‘`O(x^3) / 4`’,
with ‘`a = 1/4`’.)

The fifth rule rewrites, for example, ‘`x^2 O(x^3)`’ to ‘`O(x^5)`’.
(It is easy to see that if one of these forms is negligible, the other
is, too.) Notice the ‘`x^opt(m)`’ to pick up terms like
‘`x O(x^3)`’. Optional powers will match ‘`x`’ as ‘`x^1`’
but not 1 as ‘`x^0`’. This turns out to be exactly what we want here.

The sixth rule is the corresponding rule for products of two O’s.

Another way to solve this problem would be to create a new “data type”
that represents truncated power series. We might represent these as
function calls ‘`series(coefs, x)`’ where coefs is
a vector of coefficients for ‘`x^0`’, ‘`x^1`’, ‘`x^2`’, and so
on. Rules would exist for sums and products of such `series`
objects, and as an optional convenience could also know how to combine a
`series` object with a normal polynomial. (With this, and with a
rule that rewrites ‘`O(x^n)`’ to the equivalent `series` form,
you could still enter power series in exactly the same notation as
before.) Operations on such objects would probably be more efficient,
although the objects would be a bit harder to read.

Some other symbolic math programs provide a power series data type
similar to this. Mathematica, for example, has an object that looks
like ‘`PowerSeries[x, x0, coefs, nmin,
nmax, den]`’, where x0 is the point about which the
power series is taken (we’ve been assuming this was always zero),
and nmin, nmax, and den allow pseudo-power-series
with fractional or negative powers. Also, the `PowerSeries`
objects have a special display format that makes them look like
‘`2 x^2 + O(x^4)`’ when they are printed out. (See [Compositions](#Compositions),
for a way to do this in Calc, although for something as involved as
this it would probably be better to write the formatting routine
in Lisp.)

---

Next: [Programming Tutorial Exercise 2](#Programming-Answer-2), Previous: [Rewrites Tutorial Exercise 6](#Rewrites-Answer-6), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.58 Programming Tutorial Exercise 1 [¶](#Programming-Tutorial-Exercise-1)

Just enter the formula ‘`ninteg(sin(t)/t, t, 0, x)`’, type
`Z F`, and answer the questions. Since this formula contains two
variables, the default argument list will be ‘`(t x)`’. We want to
change this to ‘`(x)`’ since ‘`t`’ is really a dummy variable
to be used within `ninteg`.

The exact keystrokes are `Z F s Si RET RET C-b C-b DEL DEL RET y`.
(The `C-b C-b DEL DEL` are what fix the argument list.)

---

Next: [Programming Tutorial Exercise 3](#Programming-Answer-3), Previous: [Programming Tutorial Exercise 1](#Programming-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.59 Programming Tutorial Exercise 2 [¶](#Programming-Tutorial-Exercise-2)

One way is to move the number to the top of the stack, operate on
it, then move it back: `C-x ( M-TAB n M-TAB M-TAB C-x )`.

Another way is to negate the top three stack entries, then negate
again the top two stack entries: `C-x ( M-3 n M-2 n C-x )`.

Finally, it turns out that a negative prefix argument causes a
command like `n` to operate on the specified stack entry only,
which is just what we want: `C-x ( M-- 3 n C-x )`.

Just for kicks, let’s also do it algebraically:
`C-x ( ' -$$$, $$, $ RET C-x )`.

---

Next: [Programming Tutorial Exercise 4](#Programming-Answer-4), Previous: [Programming Tutorial Exercise 2](#Programming-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.60 Programming Tutorial Exercise 3 [¶](#Programming-Tutorial-Exercise-3)

Each of these functions can be computed using the stack, or using
algebraic entry, whichever way you prefer:

Computing
‘`sin(x) / x`’:

Using the stack: `C-x ( RET S TAB / C-x )`.

Using algebraic entry: `C-x ( ' sin($)/$ RET C-x )`.

Computing the logarithm:

Using the stack: `C-x ( TAB B C-x )`

Using algebraic entry: `C-x ( ' log($,$$) RET C-x )`.

Computing the vector of integers:

Using the stack: `C-x ( 1 RET 1 C-u v x C-x )`. (Recall that
`C-u v x` takes the vector size, starting value, and increment
from the stack.)

Alternatively: `C-x ( ~ v x C-x )`. (The `~` key pops a
number from the stack and uses it as the prefix argument for the
next command.)

Using algebraic entry: `C-x ( ' index($) RET C-x )`.

---

Next: [Programming Tutorial Exercise 5](#Programming-Answer-5), Previous: [Programming Tutorial Exercise 3](#Programming-Answer-3), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.61 Programming Tutorial Exercise 4 [¶](#Programming-Tutorial-Exercise-4)

Here’s one way: `C-x ( RET V R + TAB v l / C-x )`.

---

Next: [Programming Tutorial Exercise 6](#Programming-Answer-6), Previous: [Programming Tutorial Exercise 4](#Programming-Answer-4), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.62 Programming Tutorial Exercise 5 [¶](#Programming-Tutorial-Exercise-5)

```
2:  1              1:  1.61803398502         2:  1.61803398502
1:  20                 .                     1:  1.61803398875
    .                                            .

   1 RET 20         Z < & 1 + Z >                I H P
```

This answer is quite accurate.

---

Next: [Programming Tutorial Exercise 7](#Programming-Answer-7), Previous: [Programming Tutorial Exercise 5](#Programming-Answer-5), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.63 Programming Tutorial Exercise 6 [¶](#Programming-Tutorial-Exercise-6)

Here is the matrix:

```
[ [ 0, 1 ]   * [a, b] = [b, a + b]
  [ 1, 1 ] ]
```

Thus ‘`[0, 1; 1, 1]^n * [1, 1]`’ computes Fibonacci numbers ‘`n+1`’
and ‘`n+2`’. Here’s one program that does the job:

```
C-x ( ' [0, 1; 1, 1] ^ ($-1) * [1, 1] RET v u DEL C-x )
```

This program is quite efficient because Calc knows how to raise a
matrix (or other value) to the power ‘`n`’ in only
‘`log(n,2)`’
steps. For example, this program can compute the 1000th Fibonacci
number (a 209-digit integer!) in about 10 steps; even though the
`Z < ... Z >` solution had much simpler steps, it would have
required so many steps that it would not have been practical.

---

Next: [Programming Tutorial Exercise 8](#Programming-Answer-8), Previous: [Programming Tutorial Exercise 6](#Programming-Answer-6), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.64 Programming Tutorial Exercise 7 [¶](#Programming-Tutorial-Exercise-7)

The trick here is to compute the harmonic numbers differently, so that
the loop counter itself accumulates the sum of reciprocals. We use
a separate variable to hold the integer counter.

```
1:  1          2:  1       1:  .
    .          1:  4
                   .

    1 t 1       1 RET 4      Z ( t 2 r 1 1 + s 1 & Z )
```

The body of the loop goes as follows: First save the harmonic sum
so far in variable 2. Then delete it from the stack; the for loop
itself will take care of remembering it for us. Next, recall the
count from variable 1, add one to it, and feed its reciprocal to
the for loop to use as the step value. The for loop will increase
the “loop counter” by that amount and keep going until the
loop counter exceeds 4.

```
2:  31                  3:  31
1:  3.99498713092       2:  3.99498713092
    .                   1:  4.02724519544
                            .

    r 1 r 2                 RET 31 & +
```

Thus we find that the 30th harmonic number is 3.99, and the 31st
harmonic number is 4.02.

---

Next: [Programming Tutorial Exercise 9](#Programming-Answer-9), Previous: [Programming Tutorial Exercise 7](#Programming-Answer-7), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.65 Programming Tutorial Exercise 8 [¶](#Programming-Tutorial-Exercise-8)

The first step is to compute the derivative ‘`f'(x)`’ and thus
the formula
‘`x - f(x)/f'(x)`’.

(Because this definition is long, it will be repeated in concise form
below. You can use `C-x * m` to load it from there. While you are
entering a `` Z ` Z ' `` body in a macro, Calc simply collects
keystrokes without executing them. In the following diagrams we’ll
pretend Calc actually executed the keystrokes as you typed them,
just for purposes of illustration.)

```
2:  sin(cos(x)) - 0.5            3:  4.5
1:  4.5                          2:  sin(cos(x)) - 0.5
    .                            1:  -(sin(x) cos(cos(x)))
                                     .

' sin(cos(x))-0.5 RET 4.5  m r  C-x ( Z `  TAB RET a d x RET
```

```
2:  4.5
1:  x + (sin(cos(x)) - 0.5) / sin(x) cos(cos(x))
    .

    /  ' x RET TAB -   t 1
```

Now, we enter the loop. We’ll use a repeat loop with a 20-repetition
limit just in case the method fails to converge for some reason.
(Normally, the `Z /` command will stop the loop before all 20
repetitions are done.)

```
1:  4.5         3:  4.5                     2:  4.5
    .           2:  x + (sin(cos(x)) ...    1:  5.24196456928
                1:  4.5                         .
                    .

  20 Z <          RET r 1 TAB                 s l x RET
```

This is the new guess for ‘`x`’. Now we compare it with the
old one to see if we’ve converged.

```
3:  5.24196     2:  5.24196     1:  5.24196     1:  5.26345856348
2:  5.24196     1:  0               .               .
1:  4.5             .
    .

  RET M-TAB         a =             Z /             Z > Z ' C-x )
```

The loop converges in just a few steps to this value. To check
the result, we can simply substitute it back into the equation.

```
2:  5.26345856348
1:  0.499999999997
    .

 RET ' sin(cos($)) RET
```

Let’s test the new definition again:

```
2:  x^2 - 9           1:  3.
1:  1                     .
    .

  ' x^2-9 RET 1           X
```

Once again, here’s the full Newton’s Method definition:

```
C-x ( Z `  TAB RET a d x RET  /  ' x RET TAB -  t 1
           20 Z <  RET r 1 TAB  s l x RET
                   RET M-TAB  a =  Z /
              Z >
      Z '
C-x )
```

It turns out that Calc has a built-in command for applying a formula
repeatedly until it converges to a number. See [Nesting and Fixed Points](#Nesting-and-Fixed-Points),
to see how to use it.

Also, of course, `a R` is a built-in command that uses Newton’s
method (among others) to look for numerical solutions to any equation.
See [Root Finding](#Root-Finding).

---

Next: [Programming Tutorial Exercise 10](#Programming-Answer-10), Previous: [Programming Tutorial Exercise 8](#Programming-Answer-8), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.66 Programming Tutorial Exercise 9 [¶](#Programming-Tutorial-Exercise-9)

The first step is to adjust ‘`z`’ to be greater than 5. A simple
“for” loop will do the job here. If ‘`z`’ is less than 5, we
reduce the problem using
‘`psi(z) = psi(z+1) - 1/z`’. We go
on to compute
‘`psi(z+1)`’,
and remember to add back a factor of ‘`-1/z`’ when we’re done. This
step is repeated until ‘`z > 5`’.

(Because this definition is long, it will be repeated in concise form
below. You can use `C-x * m` to load it from there. While you are
entering a `` Z ` Z ' `` body in a macro, Calc simply collects
keystrokes without executing them. In the following diagrams we’ll
pretend Calc actually executed the keystrokes as you typed them,
just for purposes of illustration.)

```
1:  1.             1:  1.
    .                  .

 1.0 RET       C-x ( Z `  s 1  0 t 2
```

Here, variable 1 holds ‘`z`’ and variable 2 holds the adjustment
factor. If ‘`z < 5`’, we use a loop to increase it.

(By the way, we started with ‘`1.0`’ instead of the integer 1 because
otherwise the calculation below will try to do exact fractional arithmetic,
and will never converge because fractions compare equal only if they
are exactly equal, not just equal to within the current precision.)

```
3:  1.      2:  1.       1:  6.
2:  1.      1:  1            .
1:  5           .
    .

  RET 5        a <    Z [  5 Z (  & s + 2  1 s + 1  1 Z ) r 1  Z ]
```

Now we compute the initial part of the sum:
‘`ln(z) - 1/2z`’
minus the adjustment factor.

```
2:  1.79175946923      2:  1.7084261359      1:  -0.57490719743
1:  0.0833333333333    1:  2.28333333333         .
    .                      .

    L  r 1 2 * &           -  r 2                -
```

Now we evaluate the series. We’ll use another “for” loop counting
up the value of ‘`2 n`’. (Calc does have a summation command,
`a +`, but we’ll use loops just to get more practice with them.)

```
3:  -0.5749       3:  -0.5749        4:  -0.5749      2:  -0.5749
2:  2             2:  1:6            3:  1:6          1:  2.3148e-3
1:  40            1:  2              2:  2                .
    .                 .              1:  36.
                                         .

   2 RET 40        Z ( RET k b TAB     RET r 1 TAB ^      * /
```

```
3:  -0.5749       3:  -0.5772      2:  -0.5772     1:  -0.577215664892
2:  -0.5749       2:  -0.5772      1:  0               .
1:  2.3148e-3     1:  -0.5749          .
    .                 .

  TAB RET M-TAB       - RET M-TAB      a =     Z /    2  Z )  Z ' C-x )
```

This is the value of
‘`- gamma`’,
with a slight bit of roundoff error. To get a full 12 digits, let’s use
a higher precision:

```
2:  -0.577215664892      2:  -0.577215664892
1:  1.                   1:  -0.577215664901532

    1. RET                   p 16 RET X
```

Here’s the complete sequence of keystrokes:

```
C-x ( Z `  s 1  0 t 2
           RET 5 a <  Z [  5 Z (  & s + 2  1 s + 1  1 Z ) r 1  Z ]
           L r 1 2 * & - r 2 -
           2 RET 40  Z (  RET k b TAB RET r 1 TAB ^ * /
                          TAB RET M-TAB - RET M-TAB a = Z /
                  2  Z )
      Z '
C-x )
```

---

Next: [Programming Tutorial Exercise 11](#Programming-Answer-11), Previous: [Programming Tutorial Exercise 9](#Programming-Answer-9), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.67 Programming Tutorial Exercise 10 [¶](#Programming-Tutorial-Exercise-10)

Taking the derivative of a term of the form ‘`x^n`’ will produce
a term like
‘`n x^(n-1)`’.
Taking the derivative of a constant
produces zero. From this it is easy to see that the ‘`n`’th
derivative of a polynomial, evaluated at ‘`x = 0`’, will equal the
coefficient on the ‘`x^n`’ term times ‘`n!`’.

(Because this definition is long, it will be repeated in concise form
below. You can use `C-x * m` to load it from there. While you are
entering a `` Z ` Z ' `` body in a macro, Calc simply collects
keystrokes without executing them. In the following diagrams we’ll
pretend Calc actually executed the keystrokes as you typed them,
just for purposes of illustration.)

```
2:  5 x^4 + (x + 1)^2          3:  5 x^4 + (x + 1)^2
1:  6                          2:  0
    .                          1:  6
                                   .

  ' 5 x^4 + (x+1)^2 RET 6        C-x ( Z `  [ ] t 1  0 TAB
```

Variable 1 will accumulate the vector of coefficients.

```
2:  0              3:  0                  2:  5 x^4 + ...
1:  5 x^4 + ...    2:  5 x^4 + ...        1:  1
    .              1:  1                      .
                       .

   Z ( TAB         RET 0 s l x RET            M-TAB ! /  s | 1
```

Note that `s | 1` appends the top-of-stack value to the vector
in a variable; it is completely analogous to `s + 1`. We could
have written instead, `r 1 TAB | t 1`.

```
1:  20 x^3 + 2 x + 2      1:  0         1:  [1, 2, 1, 0, 5, 0, 0]
    .                         .             .

    a d x RET                 1 Z )         DEL r 1  Z ' C-x )
```

To convert back, a simple method is just to map the coefficients
against a table of powers of ‘`x`’.

```
2:  [1, 2, 1, 0, 5, 0, 0]    2:  [1, 2, 1, 0, 5, 0, 0]
1:  6                        1:  [0, 1, 2, 3, 4, 5, 6]
    .                            .

    6 RET                        1 + 0 RET 1 C-u v x
```

```
2:  [1, 2, 1, 0, 5, 0, 0]    2:  1 + 2 x + x^2 + 5 x^4
1:  [1, x, x^2, x^3, ... ]       .
    .

    ' x RET TAB V M ^            *
```

Once again, here are the whole polynomial to/from vector programs:

```
C-x ( Z `  [ ] t 1  0 TAB
           Z (  TAB RET 0 s l x RET M-TAB ! /  s | 1
                a d x RET
         1 Z ) r 1
      Z '
C-x )

C-x (  1 + 0 RET 1 C-u v x ' x RET TAB V M ^ *  C-x )
```

---

Next: [Programming Tutorial Exercise 12](#Programming-Answer-12), Previous: [Programming Tutorial Exercise 10](#Programming-Answer-10), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.68 Programming Tutorial Exercise 11 [¶](#Programming-Tutorial-Exercise-11)

First we define a dummy program to go on the `z s` key. The true
`z s` key is supposed to take two numbers from the stack and
return one number, so `DEL` as a dummy definition will make
sure the stack comes out right.

```
2:  4          1:  4                         2:  4
1:  2              .                         1:  2
    .                                            .

  4 RET 2       C-x ( DEL C-x )  Z K s RET       2
```

The last step replaces the 2 that was eaten during the creation
of the dummy `z s` command. Now we move on to the real
definition. The recurrence needs to be rewritten slightly,
to the form ‘`s(n,m) = s(n-1,m-1) - (n-1) s(n-1,m)`’.

(Because this definition is long, it will be repeated in concise form
below. You can use `C-x * m` to load it from there.)

```
2:  4        4:  4       3:  4       2:  4
1:  2        3:  2       2:  2       1:  2
    .        2:  4       1:  0           .
             1:  2           .
                 .

  C-x (       M-2 RET        a =         Z [  DEL DEL 1  Z :
```

```
4:  4       2:  4                     2:  3      4:  3    4:  3    3:  3
3:  2       1:  2                     1:  2      3:  2    3:  2    2:  2
2:  2           .                         .      2:  3    2:  3    1:  3
1:  0                                            1:  2    1:  1        .
    .                                                .        .

  RET 0   a = Z [  DEL DEL 0  Z :  TAB 1 - TAB   M-2 RET     1 -      z s
```

(Note that the value 3 that our dummy `z s` produces is not correct;
it is merely a placeholder that will do just as well for now.)

```
3:  3               4:  3           3:  3       2:  3      1:  -6
2:  3               3:  3           2:  3       1:  9          .
1:  2               2:  3           1:  3           .
    .               1:  2               .
                        .

 M-TAB M-TAB     TAB RET M-TAB         z s          *          -
```

```
1:  -6                          2:  4          1:  11      2:  11
    .                           1:  2              .       1:  11
                                    .                          .

  Z ] Z ] C-x )   Z K s RET      DEL 4 RET 2       z s      M-RET k s
```

Even though the result that we got during the definition was highly
bogus, once the definition is complete the `z s` command gets
the right answers.

Here’s the full program once again:

```
C-x (  M-2 RET a =
       Z [  DEL DEL 1
       Z :  RET 0 a =
            Z [  DEL DEL 0
            Z :  TAB 1 - TAB M-2 RET 1 - z s
                 M-TAB M-TAB TAB RET M-TAB z s * -
            Z ]
       Z ]
C-x )
```

You can read this definition using `C-x * m` (`read-kbd-macro`)
followed by `Z K s`, without having to make a dummy definition
first, because `read-kbd-macro` doesn’t need to execute the
definition as it reads it in. For this reason, `C-x * m` is often
the easiest way to create recursive programs in Calc.

---

Previous: [Programming Tutorial Exercise 11](#Programming-Answer-11), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.69 Programming Tutorial Exercise 12 [¶](#Programming-Tutorial-Exercise-12)

This turns out to be a much easier way to solve the problem. Let’s
denote Stirling numbers as calls of the function ‘`s`’.

First, we store the rewrite rules corresponding to the definition of
Stirling numbers in a convenient variable:

```
s e StirlingRules RET
[ s(n,n) := 1  :: n >= 0,
  s(n,0) := 0  :: n > 0,
  s(n,m) := s(n-1,m-1) - (n-1) s(n-1,m) :: n >= m :: m >= 1 ]
C-c C-c
```

Now, it’s just a matter of applying the rules:

```
2:  4          1:  s(4, 2)              1:  11
1:  2              .                        .
    .

  4 RET 2       C-x (  ' s($$,$) RET     a r StirlingRules RET  C-x )
```

As in the case of the `fib` rules, it would be useful to put these
rules in `EvalRules` and to add a ‘`:: remember`’ condition to
the last rule.

---

Next: [Data Types](#Data-Types), Previous: [Tutorial](#Tutorial), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 3 Introduction [¶](#Introduction-1)

This chapter is the beginning of the Calc reference manual.
It covers basic concepts such as the stack, algebraic and
numeric entry, undo, numeric prefix arguments, etc.

* [Basic Commands](#Basic-Commands)
* [Help Commands](#Help-Commands)
* [Stack Basics](#Stack-Basics)
* [Numeric Entry](#Numeric-Entry)
* [Algebraic Entry](#Algebraic-Entry)
* [“Quick Calculator” Mode](#Quick-Calculator)
* [Numeric Prefix Arguments](#Prefix-Arguments)
* [Undoing Mistakes](#Undo)
* [Error Messages](#Error-Messages)
* [Multiple Calculators](#Multiple-Calculators)
* [Troubleshooting Commands](#Troubleshooting-Commands)

---

Next: [Help Commands](#Help-Commands), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.1 Basic Commands [¶](#Basic-Commands-1)

To start the Calculator in its standard interface, type `M-x calc`.
By default this creates a pair of small windows, `*Calculator*`
and `*Calc Trail*`. The former displays the contents of the
Calculator stack and is manipulated exclusively through Calc commands.
It is possible (though not usually necessary) to create several Calc
mode buffers each of which has an independent stack, undo list, and
mode settings. There is exactly one Calc Trail buffer; it records a
list of the results of all calculations that have been done. The
Calc Trail buffer uses a variant of Calc mode, so Calculator commands
still work when the trail buffer’s window is selected. It is possible
to turn the trail window off, but the `*Calc Trail*` buffer itself
still exists and is updated silently. See [Trail Commands](#Trail-Commands).

In most installations, the `C-x * c` key sequence is a more
convenient way to start the Calculator. Also, `C-x * *`
is a synonym for `C-x * c` unless you last used Calc
in its Keypad mode.

Most Calc commands use one or two keystrokes. Lower- and upper-case
letters are distinct. Commands may also be entered in full `M-x` form;
for some commands this is the only form. As a convenience, the `x`
key (`calc-execute-extended-command`)
is like `M-x` except that it enters the initial string ‘`calc-`’
for you. For example, the following key sequences are equivalent:
`S`, `M-x calc-sin RET`, `x sin RET`.

Although Calc is designed to be used from the keyboard, some of
Calc’s more common commands are available from a menu. In the menu, the
arguments to the functions are given by referring to their stack level
numbers.

The Calculator exists in many parts. When you type `C-x * c`, the
Emacs “auto-load” mechanism will bring in only the first part, which
contains the basic arithmetic functions. The other parts will be
auto-loaded the first time you use the more advanced commands like trig
functions or matrix operations. This is done to improve the response time
of the Calculator in the common case when all you need to do is a
little arithmetic. If for some reason the Calculator fails to load an
extension module automatically, you can force it to load all the
extensions by using the `C-x * L` (`calc-load-everything`)
command. See [Mode Settings](#Mode-Settings).

If you type `M-x calc` or `C-x * c` with any numeric prefix argument,
the Calculator is loaded if necessary, but it is not actually started.
If the argument is positive, the `calc-ext` extensions are also
loaded if necessary. User-written Lisp code that wishes to make use
of Calc’s arithmetic routines can use ‘`(calc 0)`’ or ‘`(calc 1)`’
to auto-load the Calculator.

If you type `C-x * b`, then next time you use `C-x * c` you
will get a Calculator that uses the full height of the Emacs screen.
When full-screen mode is on, `C-x * c` runs the `full-calc`
command instead of `calc`. From the Unix shell you can type
‘`emacs -f full-calc`’ to start a new Emacs specifically for use
as a calculator. When Calc is started from the Emacs command line
like this, Calc’s normal “quit” commands actually quit Emacs itself.

The `C-x * o` command is like `C-x * c` except that the Calc
window is not actually selected. If you are already in the Calc
window, `C-x * o` switches you out of it. (The regular Emacs
`C-x o` command would also work for this, but it has a
tendency to drop you into the Calc Trail window instead, which
`C-x * o` takes care not to do.)

For one quick calculation, you can type `C-x * q` (`quick-calc`)
which prompts you for a formula (like ‘`2+3/4`’). The result is
displayed at the bottom of the Emacs screen without ever creating
any special Calculator windows. See [“Quick Calculator” Mode](#Quick-Calculator).

Finally, if you are using the X window system you may want to try
`C-x * k` (`calc-keypad`) which runs Calc with a
“calculator keypad” picture as well as a stack display. Click on
the keys with the mouse to operate the calculator. See [Keypad Mode](#Keypad-Mode).

The `q` key (`calc-quit`) exits Calc mode and closes the
Calculator’s window(s). It does not delete the Calculator buffers.
If you type `M-x calc` again, the Calculator will reappear with the
contents of the stack intact. Typing `C-x * c` or `C-x * *`
again from inside the Calculator buffer is equivalent to executing
`calc-quit`; you can think of `C-x * *` as toggling the
Calculator on and off.

The `C-x * x` command also turns the Calculator off, no matter which
user interface (standard, Keypad, or Embedded) is currently active.
It also cancels `calc-edit` mode if used from there.

The `d SPC` key sequence (`calc-refresh`) redraws the contents
of the Calculator buffer from memory. Use this if the contents of the
buffer have been damaged somehow.

The `o` key (`calc-realign`) moves the cursor back to its
“home” position at the bottom of the Calculator buffer.

The `<` and `>` keys are bound to `calc-scroll-left` and
`calc-scroll-right`. These are just like the normal horizontal
scrolling commands except that they scroll one half-screen at a time by
default. (Calc formats its output to fit within the bounds of the
window whenever it can.)

The `{` and `}` keys are bound to `calc-scroll-down`
and `calc-scroll-up`. They scroll up or down by one-half the
height of the Calc window.

The `C-x * 0` command (`calc-reset`; that’s `C-x *` followed
by a zero) resets the Calculator to its initial state. This clears
the stack, resets all the modes to their initial values (the values
that were saved with `m m` (`calc-save-modes`)), clears the
caches (see [Caches](#Caches)), and so on. (It does *not* erase the
values of any variables.) With an argument of 0, Calc will be reset to
its default state; namely, the modes will be given their default values.
With a positive prefix argument, `C-x * 0` preserves the contents of
the stack but resets everything else to its initial state; with a
negative prefix argument, `C-x * 0` preserves the contents of the
stack but resets everything else to its default state.

---

Next: [Stack Basics](#Stack-Basics), Previous: [Basic Commands](#Basic-Commands), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.2 Help Commands [¶](#Help-Commands-1)

The `?` key (`calc-help`) displays a series of brief help messages.
Some keys (such as `b` and `d`) are prefix keys, like Emacs’s
`ESC` and `C-x` prefixes. You can type
`?` after a prefix to see a list of commands beginning with that
prefix. (If the message includes ‘`[MORE]`’, press `?` again
to see additional commands for that prefix.)

The `h h` (`calc-full-help`) command displays all the `?`
responses at once. When printed, this makes a nice, compact (three pages)
summary of Calc keystrokes.

In general, the `h` key prefix introduces various commands that
provide help within Calc. Many of the `h` key functions are
Calc-specific analogues to the `C-h` functions for Emacs help.

The `h i` (`calc-info`) command runs the Emacs Info system
to read this manual on-line. This is basically the same as typing
`C-h i` (the regular way to run the Info system), then, if Info
is not already in the Calc manual, selecting the beginning of the
manual. The `C-x * i` command is another way to read the Calc
manual; it is different from `h i` in that it works any time,
not just inside Calc. The plain `i` key is also equivalent to
`h i`, though this key is obsolete and may be replaced with a
different command in a future version of Calc.

The `h t` (`calc-tutorial`) command runs the Info system on
the Tutorial section of the Calc manual. It is like `h i`,
except that it selects the starting node of the tutorial rather
than the beginning of the whole manual. (It actually selects the
node “Interactive Tutorial” which tells a few things about
using the Info system before going on to the actual tutorial.)
The `C-x * t` key is equivalent to `h t` (but it works at
all times).

The `h s` (`calc-info-summary`) command runs the Info system
on the Summary node of the Calc manual. See [Calc Summary](#Summary). The `C-x * s`
key is equivalent to `h s`.

The `h k` (`calc-describe-key`) command looks up a key
sequence in the Calc manual. For example, `h k H a S` looks
up the documentation on the `H a S` (`calc-solve-for`)
command. This works by looking up the textual description of
the key(s) in the Key Index of the manual, then jumping to the
node indicated by the index.

Most Calc commands do not have traditional Emacs documentation
strings, since the `h k` command is both more convenient and
more instructive. This means the regular Emacs `C-h k`
(`describe-key`) command will not be useful for Calc keystrokes.

The `h c` (`calc-describe-key-briefly`) command reads a
key sequence and displays a brief one-line description of it at
the bottom of the screen. It looks for the key sequence in the
Summary node of the Calc manual; if it doesn’t find the sequence
there, it acts just like its regular Emacs counterpart `C-h c`
(`describe-key-briefly`). For example, `h c H a S`
gives the description:

```
H a S runs calc-solve-for:  a `H a S' v  => fsolve(a,v)  (?=notes)
```

which means the command `H a S` or `H M-x calc-solve-for`
takes a value ‘`a`’ from the stack, prompts for a value ‘`v`’,
then applies the algebraic function `fsolve` to these values.
The ‘`?=notes`’ message means you can now type `?` to see
additional notes from the summary that apply to this command.

The `h f` (`calc-describe-function`) command looks up an
algebraic function or a command name in the Calc manual. Enter an
algebraic function name to look up that function in the Function
Index or enter a command name beginning with ‘`calc-`’ to look it
up in the Command Index. This command will also look up operator
symbols that can appear in algebraic formulas, like ‘`%`’ and
‘`=>`’.

The `h v` (`calc-describe-variable`) command looks up a
variable in the Calc manual. Enter a variable name like `pi` or
`PlotRejects`.

The `h b` (`calc-describe-bindings`) command is just like
`C-h b`, except that only local (Calc-related) key bindings are
listed.

The `h n` or `h C-n` (`calc-view-news`) command displays
the “news” or change history of Emacs, and jumps to the most recent
portion concerning Calc (if present). For older history, see the file
`etc/CALC-NEWS` in the Emacs distribution.

The `h C-c`, `h C-d`, and `h C-w` keys display copying,
distribution, and warranty information about Calc. These work by
pulling up the appropriate parts of the “Copying” or “Reporting
Bugs” sections of the manual.

---

Next: [Numeric Entry](#Numeric-Entry), Previous: [Help Commands](#Help-Commands), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.3 Stack Basics [¶](#Stack-Basics-1)

Calc uses RPN notation. If you are not familiar with RPN, see [RPN Calculations and the Stack](#RPN-Tutorial).

To add the numbers 1 and 2 in Calc you would type the keys:
`1 RET 2 +`.
(`RET` corresponds to the `ENTER` key on most calculators.)
The first three keystrokes “push” the numbers 1 and 2 onto the stack. The
`+` key always “pops” the top two numbers from the stack, adds them,
and pushes the result (3) back onto the stack. This number is ready for
further calculations: `5 -` pushes 5 onto the stack, then pops the
3 and 5, subtracts them, and pushes the result (*-2*).

Note that the “top” of the stack actually appears at the *bottom*
of the buffer. A line containing a single ‘`.`’ character signifies
the end of the buffer; Calculator commands operate on the number(s)
directly above this line. The `d t` (`calc-truncate-stack`)
command allows you to move the ‘`.`’ marker up and down in the stack;
see [Truncating the Stack](#Truncating-the-Stack).

Stack elements are numbered consecutively, with number 1 being the top of
the stack. These line numbers are ordinarily displayed on the lefthand side
of the window. The `d l` (`calc-line-numbering`) command controls
whether these numbers appear. (Line numbers may be turned off since they
slow the Calculator down a bit and also clutter the display.)

The unshifted letter `o` (`calc-realign`) command repositions
the cursor to its top-of-stack “home” position. It also undoes any
horizontal scrolling in the window. If you give it a numeric prefix
argument, it instead moves the cursor to the specified stack element.

The `RET` (or equivalent `SPC`) key is only required to separate
two consecutive numbers.
(After all, if you typed `1 2` by themselves the Calculator
would enter the number 12.) If you press `RET` or `SPC` *not*
right after typing a number, the key duplicates the number on the top of
the stack. `RET *` is thus a handy way to square a number.

The `DEL` key pops and throws away the top number on the stack.
The `TAB` key swaps the top two objects on the stack.
See [Stack and Trail Commands](#Stack-and-Trail), for descriptions of these and other stack-related
commands.

---

Next: [Algebraic Entry](#Algebraic-Entry), Previous: [Stack Basics](#Stack-Basics), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.4 Numeric Entry [¶](#Numeric-Entry-1)

Pressing a digit or other numeric key begins numeric entry using the
minibuffer. The number is pushed on the stack when you press the `RET`
or `SPC` keys. If you press any other non-numeric key, the number is
pushed onto the stack and the appropriate operation is performed. If
you press a numeric key which is not valid, the key is ignored.

There are three different concepts corresponding to the word “minus,”
typified by ‘`a-b`’ (subtraction), ‘`-x`’
(change-sign), and ‘`-5`’ (negative number). Calc uses three
different keys for these operations, respectively:
`-`, `n`, and `_` (the underscore). The `-` key subtracts
the two numbers on the top of the stack. The `n` key changes the sign
of the number on the top of the stack or the number currently being entered.
The `_` key begins entry of a negative number or changes the sign of
the number currently being entered. The following sequences all enter the
number *-5* onto the stack: `0 RET 5 -`, `5 n RET`,
`5 RET n`, `_ 5 RET`, `5 _ RET`.

Some other keys are active during numeric entry, such as `#` for
non-decimal numbers, `:` for fractions, and `@` for HMS forms.
These notations are described later in this manual with the corresponding
data types. See [Data Types](#Data-Types).

During numeric entry, the only editing key available is `DEL`.

---

Next: [“Quick Calculator” Mode](#Quick-Calculator), Previous: [Numeric Entry](#Numeric-Entry), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.5 Algebraic Entry [¶](#Algebraic-Entry-1)

The `'` (`calc-algebraic-entry`) command can be used to enter
calculations in algebraic form. This is accomplished by typing the
apostrophe key, ’, followed by the expression in standard format:

```
' 2+3*4 RET.
```

This will compute
‘`2+(3*4) = 14`’
and push it on the stack. If you wish you can
ignore the RPN aspect of Calc altogether and simply enter algebraic
expressions in this way. You may want to use `DEL` every so often to
clear previous results off the stack.

You can press the apostrophe key during normal numeric entry to switch
the half-entered number into Algebraic entry mode. One reason to do
this would be to fix a typo, as the full Emacs cursor motion and editing
keys are available during algebraic entry but not during numeric entry.

In the same vein, during either numeric or algebraic entry you can
press `` ` `` (grave accent) to switch to `calc-edit` mode, where
you complete your half-finished entry in a separate buffer.
See [Editing Stack Entries](#Editing-Stack-Entries).

If you prefer algebraic entry, you can use the command `m a`
(`calc-algebraic-mode`) to set Algebraic mode. In this mode,
digits and other keys that would normally start numeric entry instead
start full algebraic entry; as long as your formula begins with a digit
you can omit the apostrophe. Open parentheses and square brackets also
begin algebraic entry. You can still do RPN calculations in this mode,
but you will have to press `RET` to terminate every number:
`2 RET 3 RET * 4 RET +` would accomplish the same
thing as `2*3+4 RET`.

If you give a numeric prefix argument like `C-u` to the `m a`
command, it enables Incomplete Algebraic mode; this is like regular
Algebraic mode except that it applies to the `(` and `[` keys
only. Numeric keys still begin a numeric entry in this mode.

The `m t` (`calc-total-algebraic-mode`) gives you an even
stronger algebraic-entry mode, in which *all* regular letter and
punctuation keys begin algebraic entry. Use this if you prefer typing
`sqrt( )` instead of `Q`, `factor( )` instead of
`a f`, and so on. To type regular Calc commands when you are in
Total Algebraic mode, hold down the `META` key. Thus `M-q`
is the command to quit Calc, `M-p` sets the precision, and
`M-m t` (or `M-m M-t`, if you prefer) turns Total Algebraic
mode back off again. Meta keys also terminate algebraic entry, so
that `2+3 M-S` is equivalent to `2+3 RET M-S`. The symbol
‘`Alg*`’ will appear in the mode line whenever you are in this mode.

Pressing `'` (the apostrophe) a second time re-enters the previous
algebraic formula. You can then use the normal Emacs editing keys to
modify this formula to your liking before pressing `RET`.

Within a formula entered from the keyboard, the symbol `$`
represents the number on the top of the stack. If an entered formula
contains any `$` characters, the Calculator replaces the top of
stack with that formula rather than simply pushing the formula onto the
stack. Thus, `' 1+2 RET` pushes 3 on the stack, and `$*2
RET` replaces it with 6. Note that the `$` key always
initiates algebraic entry; the `'` is unnecessary if `$` is the
first character in the new formula.

Higher stack elements can be accessed from an entered formula with the
symbols `$$`, `$$$`, and so on. The number of stack elements
removed (to be replaced by the entered values) equals the number of dollar
signs in the longest such symbol in the formula. For example, ‘`$$+$$$`’
adds the second and third stack elements, replacing the top three elements
with the answer. (All information about the top stack element is thus lost
since no single ‘`$`’ appears in this formula.)

A slightly different way to refer to stack elements is with a dollar
sign followed by a number: ‘`$1`’, ‘`$2`’, and so on are much
like ‘`$`’, ‘`$$`’, etc., except that stack entries referred
to numerically are not replaced by the algebraic entry. That is, while
‘`$+1`’ replaces 5 on the stack with 6, ‘`$1+1`’ leaves the 5
on the stack and pushes an additional 6.

If a sequence of formulas are entered separated by commas, each formula
is pushed onto the stack in turn. For example, ‘`1,2,3`’ pushes
those three numbers onto the stack (leaving the 3 at the top), and
‘`$+1,$-1`’ replaces a 5 on the stack with 4 followed by 6. Also,
‘`$,$$`’ exchanges the top two elements of the stack, just like the
`TAB` key.

You can finish an algebraic entry with `M-=` or `M-RET` instead
of `RET`. This uses `=` to evaluate the variables in each
formula that goes onto the stack. (Thus `' pi RET` pushes
the variable ‘`pi`’, but `' pi M-RET` pushes 3.1415.)

If you finish your algebraic entry by pressing `LFD` (or `C-j`)
instead of `RET`, Calc disables simplification
(as if by `m O`; see [Simplification Modes](#Simplification-Modes)) while the entry
is being pushed on the stack. Thus `' 1+2 RET` pushes 3
on the stack, but `' 1+2 LFD` pushes the formula ‘`1+2`’;
you might then press `=` when it is time to evaluate this formula.

---

Next: [Numeric Prefix Arguments](#Prefix-Arguments), Previous: [Algebraic Entry](#Algebraic-Entry), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.6 “Quick Calculator” Mode [¶](#g_t_0060_0060Quick-Calculator_0027_0027-Mode)

There is another way to invoke the Calculator if all you need to do
is make one or two quick calculations. Type `C-x * q` (or
`M-x quick-calc`), then type any formula as an algebraic entry.
The Calculator will compute the result and display it in the echo
area, without ever actually putting up a Calc window.

You can use the `$` character in a Quick Calculator formula to
refer to the previous Quick Calculator result. Older results are
not retained; the Quick Calculator has no effect on the full
Calculator’s stack or trail. If you compute a result and then
forget what it was, just run `C-x * q` again and enter
‘`$`’ as the formula.

If this is the first time you have used the Calculator in this Emacs
session, the `C-x * q` command will create the `*Calculator*`
buffer and perform all the usual initializations; it simply will
refrain from putting that buffer up in a new window. The Quick
Calculator refers to the `*Calculator*` buffer for all mode
settings. Thus, for example, to set the precision that the Quick
Calculator uses, simply run the full Calculator momentarily and use
the regular `p` command.

If you use `C-x * q` from inside the Calculator buffer, the
effect is the same as pressing the apostrophe key (algebraic entry).

The result of a Quick calculation is placed in the Emacs “kill ring”
as well as being displayed. A subsequent `C-y` command will
yank the result into the editing buffer. You can also use this
to yank the result into the next `C-x * q` input line as a more
explicit alternative to `$` notation, or to yank the result
into the Calculator stack after typing `C-x * c`.

If you give a prefix argument to `C-x * q` or finish your formula
by typing `LFD` (or `C-j`) instead of `RET`, the result is
inserted immediately into the current buffer rather than going into
the kill ring.

Quick Calculator results are actually evaluated as if by the `=`
key (which replaces variable names by their stored values, if any).
If the formula you enter is an assignment to a variable using the
‘`:=`’ operator, say, ‘`foo := 2 + 3`’ or ‘`foo := foo + 1`’,
then the result of the evaluation is stored in that Calc variable.
See [Storing and Recalling](#Store-and-Recall).

If the result is an integer and the current display radix is decimal,
the number will also be displayed in hex, octal and binary formats. If
the integer is in the range from 1 to 126, it will also be displayed as
an ASCII character.

For example, the quoted character ‘`"x"`’ produces the vector
result ‘`[120]`’ (because 120 is the ASCII code of the lower-case
“x”; see [Strings](#Strings)). Since this is a vector, not an integer, it
is displayed only according to the current mode settings. But
running Quick Calc again and entering ‘`120`’ will produce the
result ‘`120 (16#78, 8#170, x)`’ which shows the number in its
decimal, hexadecimal, octal, and character forms.

Please note that the Quick Calculator is not any faster at loading
or computing the answer than the full Calculator; the name “quick”
merely refers to the fact that it’s much less hassle to use for
small calculations.

---

Next: [Undoing Mistakes](#Undo), Previous: [“Quick Calculator” Mode](#Quick-Calculator), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.7 Numeric Prefix Arguments [¶](#Numeric-Prefix-Arguments)

Many Calculator commands use numeric prefix arguments. Some, such as
`d s` (`calc-sci-notation`), set a parameter to the value of
the prefix argument or use a default if you don’t use a prefix.
Others (like `d f` (`calc-fix-notation`)) require an argument
and prompt for a number if you don’t give one as a prefix.

As a rule, stack-manipulation commands accept a numeric prefix argument
which is interpreted as an index into the stack. A positive argument
operates on the top n stack entries; a negative argument operates
on the nth stack entry in isolation; and a zero argument operates
on the entire stack.

Most commands that perform computations (such as the arithmetic and
scientific functions) accept a numeric prefix argument that allows the
operation to be applied across many stack elements. For unary operations
(that is, functions of one argument like absolute value or complex
conjugate), a positive prefix argument applies that function to the top
n stack entries simultaneously, and a negative argument applies it
to the nth stack entry only. For binary operations (functions of
two arguments like addition, GCD, and vector concatenation), a positive
prefix argument “reduces” the function across the top n
stack elements (for example, `C-u 5 +` sums the top 5 stack entries;
see [Reducing and Mapping Vectors](#Reducing-and-Mapping)), and a negative argument maps the next-to-top
n stack elements with the top stack element as a second argument
(for example, `7 c-u -5 +` adds 7 to the top 5 stack elements).
This feature is not available for operations which use the numeric prefix
argument for some other purpose.

Numeric prefixes are specified the same way as always in Emacs: Press
a sequence of `META`-digits, or press `ESC` followed by digits,
or press `C-u` followed by digits. Some commands treat plain
`C-u` (without any actual digits) specially.

You can type `~` (`calc-num-prefix`) to pop an integer from the
top of the stack and enter it as the numeric prefix for the next command.
For example, `C-u 16 p` sets the precision to 16 digits; an alternate
(silly) way to do this would be `2 RET 4 ^ ~ p`, i.e., compute 2
to the fourth power and set the precision to that value.

Conversely, if you have typed a numeric prefix argument the `~` key
pushes it onto the stack in the form of an integer.

---

Next: [Error Messages](#Error-Messages), Previous: [Numeric Prefix Arguments](#Prefix-Arguments), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.8 Undoing Mistakes [¶](#Undoing-Mistakes)

The shift-`U` key (`calc-undo`) undoes the most recent operation.
If that operation added or dropped objects from the stack, those objects
are removed or restored. If it was a “store” operation, you are
queried whether or not to restore the variable to its original value.
The `U` key may be pressed any number of times to undo successively
farther back in time; with a numeric prefix argument it undoes a
specified number of operations. When the Calculator is quit, as with
the `q` (`calc-quit`) command, the undo history will be
truncated to the length of the customizable variable
`calc-undo-length` (see [Customizing Calc](#Customizing-Calc)), which by default
is ‘`100`’. (Recall that `C-x * c` is synonymous with
`calc-quit` while inside the Calculator; this also truncates the
undo history.)

Currently the mode-setting commands (like `calc-precision`) are not
undoable. You can undo past a point where you changed a mode, but you
will need to reset the mode yourself.

The shift-`D` key (`calc-redo`) redoes an operation that was
mistakenly undone. Pressing `U` with a negative prefix argument is
equivalent to executing `calc-redo`. You can redo any number of
times, up to the number of recent consecutive undo commands. Redo
information is cleared whenever you give any command that adds new undo
information, i.e., if you undo, then enter a number on the stack or make
any other change, then it will be too late to redo.

The `M-RET` key (`calc-last-args`) is like undo in that
it restores the arguments of the most recent command onto the stack;
however, it does not remove the result of that command. Given a numeric
prefix argument, this command applies to the ‘`n`’th most recent
command which removed items from the stack; it pushes those items back
onto the stack.

The `K` (`calc-keep-args`) command provides a related function
to `M-RET`. See [Stack and Trail Commands](#Stack-and-Trail).

It is also possible to recall previous results or inputs using the trail.
See [Trail Commands](#Trail-Commands).

The standard Emacs `C-_` undo key is recognized as a synonym for `U`.

---

Next: [Multiple Calculators](#Multiple-Calculators), Previous: [Undoing Mistakes](#Undo), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.9 Error Messages [¶](#Error-Messages-1)

Many situations that would produce an error message in other calculators
simply create unsimplified formulas in the Emacs Calculator. For example,
`1 RET 0 /` pushes the formula ‘`1 / 0`’; `0 L` pushes
the formula ‘`ln(0)`’. Floating-point overflow and underflow are also
reasons for this to happen.

When a function call must be left in symbolic form, Calc usually
produces a message explaining why. Messages that are probably
surprising or indicative of user errors are displayed automatically.
Other messages are simply kept in Calc’s memory and are displayed only
if you type `w` (`calc-why`). You can also press `w` if
the same computation results in several messages. (The first message
will end with ‘`[w=more]`’ in this case.)

The `d w` (`calc-auto-why`) command controls when error messages
are displayed automatically. (Calc effectively presses `w` for you
after your computation finishes.) By default, this occurs only for
“important” messages. The other possible modes are to report
*all* messages automatically, or to report none automatically (so
that you must always press `w` yourself to see the messages).

---

Next: [Troubleshooting Commands](#Troubleshooting-Commands), Previous: [Error Messages](#Error-Messages), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.10 Multiple Calculators [¶](#Multiple-Calculators-1)

It is possible to have any number of Calc mode buffers at once.
Usually this is done by executing `M-x another-calc`, which
is similar to `C-x * c` except that if a `*Calculator*`
buffer already exists, a new, independent one with a name of the
form `*Calculator*<n>` is created. You can also use the
command `calc-mode` to put any buffer into Calculator mode, but
this would ordinarily never be done.

The `q` (`calc-quit`) command does not destroy a Calculator buffer;
it only closes its window. Use `M-x kill-buffer` to destroy a
Calculator buffer.

Each Calculator buffer keeps its own stack, undo list, and mode settings
such as precision, angular mode, and display formats. In Emacs terms,
variables such as `calc-stack` are buffer-local variables. The
global default values of these variables are used only when a new
Calculator buffer is created. The `calc-quit` command saves
the stack and mode settings of the buffer being quit as the new defaults.

There is only one trail buffer, `*Calc Trail*`, used by all
Calculator buffers.

---

Previous: [Multiple Calculators](#Multiple-Calculators), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 3.11 Troubleshooting Commands [¶](#Troubleshooting-Commands-1)

This section describes commands you can use in case a computation
incorrectly fails or gives the wrong answer.

See [Reporting Bugs](#Reporting-Bugs), if you find a problem that appears to be due
to a bug or deficiency in Calc.

* [Autoloading Problems](#Autoloading-Problems)
* [Recursion Depth](#Recursion-Depth)
* [Caches](#Caches)
* [Debugging Calc](#Debugging-Calc)

---

Next: [Recursion Depth](#Recursion-Depth), Up: [Troubleshooting Commands](#Troubleshooting-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 3.11.1 Autoloading Problems [¶](#Autoloading-Problems-1)

The Calc program is split into many component files; components are
loaded automatically as you use various commands that require them.
Occasionally Calc may lose track of when a certain component is
necessary; typically this means you will type a command and it won’t
work because some function you’ve never heard of was undefined.

If this happens, the easiest workaround is to type `C-x * L`
(`calc-load-everything`) to force all the parts of Calc to be
loaded right away. This will cause Emacs to take up a lot more
memory than it would otherwise, but it’s guaranteed to fix the problem.

---

Next: [Caches](#Caches), Previous: [Autoloading Problems](#Autoloading-Problems), Up: [Troubleshooting Commands](#Troubleshooting-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 3.11.2 Recursion Depth [¶](#Recursion-Depth-1)

Calc uses recursion in many of its calculations. Emacs Lisp keeps a
variable `max-lisp-eval-depth` which limits the amount of recursion
possible in an attempt to recover from program bugs. If a calculation
ever halts incorrectly with the message “Computation got stuck or
ran too long,” use the `M` command (`calc-more-recursion-depth`)
to increase this limit. (Of course, this will not help if the
calculation really did get stuck due to some problem inside Calc.)

The limit is always increased (multiplied) by a factor of two. There
is also an `I M` (`calc-less-recursion-depth`) command which
decreases this limit by a factor of two, down to a minimum value of 200.
The default value is 1000.

---

Next: [Debugging Calc](#Debugging-Calc), Previous: [Recursion Depth](#Recursion-Depth), Up: [Troubleshooting Commands](#Troubleshooting-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 3.11.3 Caches [¶](#Caches-1)

Calc saves certain values after they have been computed once. For
example, the `P` (`calc-pi`) command initially “knows” the
constant ‘`pi`’ to about 20 decimal places; if the current precision
is greater than this, it will recompute ‘`pi`’ using a series
approximation. This value will not need to be recomputed ever again
unless you raise the precision still further. Many operations such as
logarithms and sines make use of similarly cached values such as
‘`pi/4`’ and
‘`ln(2)`’.
The visible effect of caching is that
high-precision computations may seem to do extra work the first time.
Other things cached include powers of two (for the binary arithmetic
functions), matrix inverses and determinants, symbolic integrals, and
data points computed by the graphing commands.

If you suspect a Calculator cache has become corrupt, you can use the
`calc-flush-caches` command to reset all caches to the empty state.
(This should only be necessary in the event of bugs in the Calculator.)
The `C-x * 0` (with the zero key) command also resets caches along
with all other aspects of the Calculator’s state.

---

Previous: [Caches](#Caches), Up: [Troubleshooting Commands](#Troubleshooting-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 3.11.4 Debugging Calc [¶](#Debugging-Calc-1)

A few commands exist to help in the debugging of Calc commands.
See [Programming](#Programming), to see the various ways that you can write
your own Calc commands.

The `Z T` (`calc-timing`) command turns on and off a mode
in which the timing of slow commands is reported in the Trail.
Any Calc command that takes two seconds or longer writes a line
to the Trail showing how many seconds it took. This value is
accurate only to within one second.

All steps of executing a command are included; in particular, time
taken to format the result for display in the stack and trail is
counted. Some prompts also count time taken waiting for them to
be answered, while others do not; this depends on the exact
implementation of the command. For best results, if you are timing
a sequence that includes prompts or multiple commands, define a
keyboard macro to run the whole sequence at once. Calc’s `X`
command (see [Programming with Keyboard Macros](#Keyboard-Macros)) will then report the time taken
to execute the whole macro.

Another advantage of the `X` command is that while it is
executing, the stack and trail are not updated from step to step.
So if you expect the output of your test sequence to leave a result
that may take a long time to format and you don’t wish to count
this formatting time, end your sequence with a `DEL` keystroke
to clear the result from the stack. When you run the sequence with
`X`, Calc will never bother to format the large result.

Another thing `Z T` does is to increase the Emacs variable
`gc-cons-threshold` to a much higher value (two million; the
usual default in Calc is 250,000) for the duration of each command.
This generally prevents garbage collection during the timing of
the command, though it may cause your Emacs process to grow
abnormally large. (Garbage collection time is a major unpredictable
factor in the timing of Emacs operations.)

Another command that is useful when debugging your own Lisp
extensions to Calc is `M-x calc-pass-errors`, which disables
the error handler that changes the “`max-lisp-eval-depth`
exceeded” message to the much more friendly “Computation got
stuck or ran too long.” This handler interferes with the Emacs
Lisp debugger’s `debug-on-error` mode. Errors are reported
in the handler itself rather than at the true location of the
error. After you have executed `calc-pass-errors`, Lisp
errors will be reported correctly but the user-friendly message
will be lost.

---

Next: [Stack and Trail Commands](#Stack-and-Trail), Previous: [Introduction](#Introduction), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 4 Data Types [¶](#Data-Types-1)

This chapter discusses the various types of objects that can be placed
on the Calculator stack, how they are displayed, and how they are
entered. (See [Data Type Formats](#Data-Type-Formats), for information on how these data
types are represented as underlying Lisp objects.)

Integers, fractions, and floats are various ways of describing real
numbers. HMS forms also for many purposes act as real numbers. These
types can be combined to form complex numbers, modulo forms, error forms,
or interval forms. (But these last four types cannot be combined
arbitrarily: error forms may not contain modulo forms, for example.)
Finally, all these types of numbers may be combined into vectors,
matrices, or algebraic formulas.

* [Integers](#Integers)
* [Fractions](#Fractions)
* [Floats](#Floats)
* [Complex Numbers](#Complex-Numbers)
* [Infinities](#Infinities)
* [Vectors and Matrices](#Vectors-and-Matrices)
* [Strings](#Strings)
* [HMS Forms](#HMS-Forms)
* [Date Forms](#Date-Forms)
* [Modulo Forms](#Modulo-Forms)
* [Error Forms](#Error-Forms)
* [Interval Forms](#Interval-Forms)
* [Incomplete Objects](#Incomplete-Objects)
* [Variables](#Variables)
* [Formulas](#Formulas)

---

Next: [Fractions](#Fractions), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.1 Integers [¶](#Integers-1)

The Calculator stores integers to arbitrary precision. Addition,
subtraction, and multiplication of integers always yields an exact
integer result. (If the result of a division or exponentiation of
integers is not an integer, it is expressed in fractional or
floating-point form according to the current Fraction mode.
See [Fraction Mode](#Fraction-Mode).)

A decimal integer is represented as an optional sign followed by a
sequence of digits. Grouping (see [Grouping Digits](#Grouping-Digits)) can be used to
insert a comma at every third digit for display purposes, but you
must not type commas during the entry of numbers.

A non-decimal integer is represented as an optional sign, a radix
between 2 and 36, a ‘`#`’ symbol, and one or more digits. For radix 11
and above, the letters A through Z (upper- or lower-case) count as
digits and do not terminate numeric entry mode. See [Radix Modes](#Radix-Modes), for how
to set the default radix for display of integers. Numbers of any radix
may be entered at any time. If you press `#` at the beginning of a
number, the current display radix is used.

---

Next: [Floats](#Floats), Previous: [Integers](#Integers), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.2 Fractions [¶](#Fractions-1)

A *fraction* is a ratio of two integers. Fractions are traditionally
written “2/3” but Calc uses the notation ‘`2:3`’. (The `/` key
performs RPN division; the following two sequences push the number
‘`2:3`’ on the stack: `2 : 3 RET`, or `2 RET 3 /`
assuming Fraction mode has been enabled.)
When the Calculator produces a fractional result it always reduces it to
simplest form, which may in fact be an integer.

Fractions may also be entered in a three-part form, where ‘`2:3:4`’
represents two-and-three-quarters. See [Fraction Formats](#Fraction-Formats), for fraction
display formats.

Non-decimal fractions are entered and displayed as
‘`radix#num:denom`’ (or in the analogous three-part
form). The numerator and denominator always use the same radix.

Fractions may also be entered with `⁄` (U+2044 FRACTION SLASH) in
place of any `:`. Precomposed fraction characters from `½`
(U+00BD VULGAR FRACTION ONE HALF) through `⅞` (U+215E VULGAR
FRACTION SEVEN EIGHTHS) are supported as well. Thus, ‘`2:3`’,
‘`2⁄3`’, and ‘`⅞`’ are all equivalent.

---

Next: [Complex Numbers](#Complex-Numbers), Previous: [Fractions](#Fractions), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.3 Floats [¶](#Floats-1)

A floating-point number or *float* is a number stored in scientific
notation. The number of significant digits in the fractional part is
governed by the current floating precision (see [Precision](#Precision)). The
range of acceptable values is from
‘`10^-3999999`’
(inclusive) to
‘`10^4000000`’
(exclusive), plus the corresponding negative values and zero.

Calculations that would exceed the allowable range of values (such
as ‘`exp(exp(20))`’) are left in symbolic form by Calc. The
messages “floating-point overflow” or “floating-point underflow”
indicate that during the calculation a number would have been produced
that was too large or too close to zero, respectively, to be represented
by Calc. This does not necessarily mean the final result would have
overflowed, just that an overflow occurred while computing the result.
(In fact, it could report an underflow even though the final result
would have overflowed!)

If a rational number and a float are mixed in a calculation, the result
will in general be expressed as a float. Commands that require an integer
value (such as `k g` [`gcd`]) will also accept integer-valued
floats, i.e., floating-point numbers with nothing after the decimal point.

Floats are identified by the presence of a decimal point and/or an
exponent. In general a float consists of an optional sign, digits
including an optional decimal point, and an optional exponent consisting
of an ‘`e`’, an optional sign, and up to seven exponent digits.
For example, ‘`23.5e-2`’ is 23.5 times ten to the minus-second power,
or 0.235.

Floating-point numbers are normally displayed in decimal notation with
all significant figures shown. Exceedingly large or small numbers are
displayed in scientific notation. Various other display options are
available. See [Float Formats](#Float-Formats).

Floating-point numbers are stored in decimal, not binary. The result
of each operation is rounded to the nearest value representable in the
number of significant digits specified by the current precision,
rounding away from zero in the case of a tie. Thus (in the default
display mode) what you see is exactly what you get. Some operations such
as square roots and transcendental functions are performed with several
digits of extra precision and then rounded down, in an effort to make the
final result accurate to the full requested precision. However,
accuracy is not rigorously guaranteed. If you suspect the validity of a
result, try doing the same calculation in a higher precision. The
Calculator’s arithmetic is not intended to be IEEE-conformant in any
way.

While floats are always *stored* in decimal, they can be entered
and displayed in any radix just like integers and fractions. Since a
float that is entered in a radix other that 10 will be converted to
decimal, the number that Calc stores may not be exactly the number that
was entered, it will be the closest decimal approximation given the
current precision. The notation ‘`radix#ddd.ddd`’
is a floating-point number whose digits are in the specified radix.
Note that the ‘`.`’ is more aptly referred to as a “radix point”
than as a decimal point in this case. The number ‘`8#123.4567`’ is
defined as ‘`8#1234567 * 8^-4`’. If the radix is 14 or less, you can
use ‘`e`’ notation to write a non-decimal number in scientific
notation. The exponent is written in decimal, and is considered to be a
power of the radix: ‘`8#1234567e-4`’. If the radix is 15 or above,
the letter ‘`e`’ is a digit, so scientific notation must be written
out, e.g., ‘`16#123.4567*16^2`’. The first two exercises of the
Modes Tutorial explore some of the properties of non-decimal floats.

---

Next: [Infinities](#Infinities), Previous: [Floats](#Floats), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.4 Complex Numbers [¶](#Complex-Numbers-1)

There are two supported formats for complex numbers: rectangular and
polar. The default format is rectangular, displayed in the form
‘`(real,imag)`’ where real is the real part and
imag is the imaginary part, each of which may be any real number.
Rectangular complex numbers can also be displayed in ‘`a+bi`’
notation; see [Complex Formats](#Complex-Formats).

Polar complex numbers are displayed in the form
‘`(`r`;`theta`)`’
where r is the nonnegative magnitude and
theta
is the argument or phase angle. The range of
theta
depends on the current angular mode (see [Angular Modes](#Angular-Modes)); it is
generally between *-180* and *+180* degrees or the equivalent range
in radians.

Complex numbers are entered in stages using incomplete objects.
See [Incomplete Objects](#Incomplete-Objects).

Operations on rectangular complex numbers yield rectangular complex
results, and similarly for polar complex numbers. Where the two types
are mixed, or where new complex numbers arise (as for the square root of
a negative real), the current *Polar mode* is used to determine the
type. See [Polar Mode](#Polar-Mode).

A complex result in which the imaginary part is zero (or the phase angle
is 0 or 180 degrees or ‘`pi`’ radians) is automatically converted to a real
number.

---

Next: [Vectors and Matrices](#Vectors-and-Matrices), Previous: [Complex Numbers](#Complex-Numbers), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.5 Infinities [¶](#Infinities-1)

The word `inf` represents the mathematical concept of *infinity*.
Calc actually has three slightly different infinity-like values:
`inf`, `uinf`, and `nan`. These are just regular
variable names (see [Variables](#Variables)); you should avoid using these
names for your own variables because Calc gives them special
treatment. Infinities, like all variable names, are normally
entered using algebraic entry.

Mathematically speaking, it is not rigorously correct to treat
“infinity” as if it were a number, but mathematicians often do
so informally. When they say that ‘`1 / inf = 0`’, what they
really mean is that ‘`1 / x`’, as ‘`x`’ becomes larger and
larger, becomes arbitrarily close to zero. So you can imagine
that if ‘`x`’ got “all the way to infinity,” then ‘`1 / x`’
would go all the way to zero. Similarly, when they say that
‘`exp(inf) = inf`’, they mean that
‘`exp(x)`’
grows without bound as ‘`x`’ grows. The symbol ‘`-inf`’ likewise
stands for an infinitely negative real value; for example, we say that
‘`exp(-inf) = 0`’. You can have an infinity pointing in any
direction on the complex plane: ‘`sqrt(-inf) = i inf`’.

The same concept of limits can be used to define ‘`1 / 0`’. We
really want the value that ‘`1 / x`’ approaches as ‘`x`’
approaches zero. But if all we have is ‘`1 / 0`’, we can’t
tell which direction ‘`x`’ was coming from. If ‘`x`’ was
positive and decreasing toward zero, then we should say that
‘`1 / 0 = inf`’. But if ‘`x`’ was negative and increasing
toward zero, the answer is ‘`1 / 0 = -inf`’. In fact, ‘`x`’
could be an imaginary number, giving the answer ‘`i inf`’ or
‘`-i inf`’. Calc uses the special symbol ‘`uinf`’ to mean
*undirected infinity*, i.e., a value which is infinitely
large but with an unknown sign (or direction on the complex plane).

Calc actually has three modes that say how infinities are handled.
Normally, infinities never arise from calculations that didn’t
already have them. Thus, ‘`1 / 0`’ is treated simply as an
error and left unevaluated. The `m i` (`calc-infinite-mode`)
command (see [Infinite Mode](#Infinite-Mode)) enables a mode in which
‘`1 / 0`’ evaluates to `uinf` instead. There is also
an alternative type of infinite mode which says to treat zeros
as if they were positive, so that ‘`1 / 0 = inf`’. While this
is less mathematically correct, it may be the answer you want in
some cases.

Since all infinities are “as large” as all others, Calc simplifies,
e.g., ‘`5 inf`’ to ‘`inf`’. Another example is
‘`5 - inf = -inf`’, where the ‘`-inf`’ is so large that
adding a finite number like five to it does not affect it.
Note that ‘`a - inf`’ also results in ‘`-inf`’; Calc assumes
that variables like `a` always stand for finite quantities.
Just to show that infinities really are all the same size,
note that ‘`sqrt(inf) = inf^2 = exp(inf) = inf`’ in Calc’s
notation.

It’s not so easy to define certain formulas like ‘`0 * inf`’ and
‘`inf / inf`’. Depending on where these zeros and infinities
came from, the answer could be literally anything. The latter
formula could be the limit of ‘`x / x`’ (giving a result of one),
or ‘`2 x / x`’ (giving two), or ‘`x^2 / x`’ (giving `inf`),
or ‘`x / x^2`’ (giving zero). Calc uses the symbol `nan`
to represent such an *indeterminate* value. (The name “nan”
comes from analogy with the “NAN” concept of IEEE standard
arithmetic; it stands for “Not A Number.” This is somewhat of a
misnomer, since `nan` *does* stand for some number or
infinity, it’s just that *which* number it stands for
cannot be determined.) In Calc’s notation, ‘`0 * inf = nan`’
and ‘`inf / inf = nan`’. A few other common indeterminate
expressions are ‘`inf - inf`’ and ‘`inf ^ 0`’. Also,
‘`0 / 0 = nan`’ if you have turned on Infinite mode
(as described above).

Infinities are especially useful as parts of *intervals*.
See [Interval Forms](#Interval-Forms).

---

Next: [Strings](#Strings), Previous: [Infinities](#Infinities), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.6 Vectors and Matrices [¶](#Vectors-and-Matrices-1)

The *vector* data type is flexible and general. A vector is simply a
list of zero or more data objects. When these objects are numbers, the
whole is a vector in the mathematical sense. When these objects are
themselves vectors of equal (nonzero) length, the whole is a *matrix*.
A vector which is not a matrix is referred to here as a *plain vector*.

A vector is displayed as a list of values separated by commas and enclosed
in square brackets: ‘`[1, 2, 3]`’. Thus the following is a 2 row by
3 column matrix: ‘`[[1, 2, 3], [4, 5, 6]]`’. Vectors, like complex
numbers, are entered as incomplete objects. See [Incomplete Objects](#Incomplete-Objects).
During algebraic entry, vectors are entered all at once in the usual
brackets-and-commas form. Matrices may be entered algebraically as nested
vectors, or using the shortcut notation ‘`[1, 2, 3; 4, 5, 6]`’,
with rows separated by semicolons. The commas may usually be omitted
when entering vectors: ‘`[1 2 3]`’. Curly braces may be used in
place of brackets: ‘`{1, 2, 3}`’, but the commas are required in
this case.

Traditional vector and matrix arithmetic is also supported;
see [Basic Arithmetic](#Basic-Arithmetic) and see [Vector/Matrix Functions](#Matrix-Functions).
Many other operations are applied to vectors element-wise. For example,
the complex conjugate of a vector is a vector of the complex conjugates
of its elements.

Algebraic functions for building vectors include ‘`vec(a, b, c)`’
to build ‘`[a, b, c]`’, ‘`cvec(a, n, m)`’ to build an
nxm
matrix of ‘`a`’s, and ‘`index(n)`’ to build a vector of integers
from 1 to ‘`n`’.

---

Next: [HMS Forms](#HMS-Forms), Previous: [Vectors and Matrices](#Vectors-and-Matrices), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.7 Strings [¶](#Strings-1)

Character strings are not a special data type in the Calculator.
Rather, a string is represented simply as a vector all of whose elements
are integers in the Latin-1 range 0 to 255. You can enter a string at
any time by pressing the `"` key. Quotation marks and backslashes
are written ‘`\"`’ and ‘`\\`’, respectively, inside strings.
Other notations introduced by backslashes are:

```
\a     7          \^@    0
\b     8          \^a-z  1-26
\e     27         \^[    27
\f     12         \^\\   28
\n     10         \^]    29
\r     13         \^^    30
\t     9          \^_    31
                  \^?    127
```

Finally, a backslash followed by three octal digits produces any
character from its code.

Strings are normally displayed in vector-of-integers form. The
`d "` (`calc-display-strings`) command toggles a mode in
which any vectors of small integers are displayed as quoted strings
instead. The display of strings containing higher character codes can
be enabled by increasing the custom variable
`calc-string-maximum-character` (see [Customizing Calc](#Customizing-Calc)).

The backslash notations shown above are also used for displaying
strings. For ASCII control characters (below 32), and for the
`DEL` character (127), Calc uses the backslash-letter combination
if there is one, or otherwise uses a ‘`\^`’ sequence. Control
characters above 127 are not translated by Calc, and will show up in
backslash-octal-digits notation. The display of higher character codes
will depend on your display settings and system font coverage.

The only Calc feature that uses strings is *compositions*;
see [Compositions](#Compositions). Strings also provide a convenient
way to do conversions between ASCII characters and integers.

There is a `string` function which provides a different display
format for strings. Basically, ‘`string(s)`’, where s
is a vector of integers in the proper range, is displayed as the
corresponding string of characters with no surrounding quotation
marks or other modifications. Thus ‘`string("ABC")`’ (or
‘`string([65 66 67])`’) will look like ‘`ABC`’ on the stack.
This happens regardless of whether `d "` has been used. The
only way to turn it off is to use `d U` (unformatted language
mode) which will display ‘`string("ABC")`’ instead.

Control characters are displayed somewhat differently by `string`.
Characters below 32, and character 127, are shown using ‘`^`’ notation
(same as shown above, but without the backslash). The quote and
backslash characters are left alone, as are characters 128 and above.

The `bstring` function is just like `string` except that
the resulting string is breakable across multiple lines if it doesn’t
fit all on one line. Potential break points occur at every space
character in the string.

---

Next: [Date Forms](#Date-Forms), Previous: [Strings](#Strings), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
