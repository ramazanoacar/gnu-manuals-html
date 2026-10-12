### 2.7 Answers to Exercises [¶](#Answers-to-Exercises-1)

This section includes answers to all the exercises in the Calc tutorial.

* [RPN Tutorial Exercise 1](#RPN-Answer-1)
* [RPN Tutorial Exercise 2](#RPN-Answer-2)
* [RPN Tutorial Exercise 3](#RPN-Answer-3)
* [RPN Tutorial Exercise 4](#RPN-Answer-4)
* [Algebraic Entry Tutorial Exercise 1](#Algebraic-Answer-1)
* [Algebraic Entry Tutorial Exercise 2](#Algebraic-Answer-2)
* [Algebraic Entry Tutorial Exercise 3](#Algebraic-Answer-3)
* [Modes Tutorial Exercise 1](#Modes-Answer-1)
* [Modes Tutorial Exercise 2](#Modes-Answer-2)
* [Modes Tutorial Exercise 3](#Modes-Answer-3)
* [Modes Tutorial Exercise 4](#Modes-Answer-4)
* [Arithmetic Tutorial Exercise 1](#Arithmetic-Answer-1)
* [Arithmetic Tutorial Exercise 2](#Arithmetic-Answer-2)
* [Vector Tutorial Exercise 1](#Vector-Answer-1)
* [Vector Tutorial Exercise 2](#Vector-Answer-2)
* [Matrix Tutorial Exercise 1](#Matrix-Answer-1)
* [Matrix Tutorial Exercise 2](#Matrix-Answer-2)
* [Matrix Tutorial Exercise 3](#Matrix-Answer-3)
* [List Tutorial Exercise 1](#List-Answer-1)
* [List Tutorial Exercise 2](#List-Answer-2)
* [List Tutorial Exercise 3](#List-Answer-3)
* [List Tutorial Exercise 4](#List-Answer-4)
* [List Tutorial Exercise 5](#List-Answer-5)
* [List Tutorial Exercise 6](#List-Answer-6)
* [List Tutorial Exercise 7](#List-Answer-7)
* [List Tutorial Exercise 8](#List-Answer-8)
* [List Tutorial Exercise 9](#List-Answer-9)
* [List Tutorial Exercise 10](#List-Answer-10)
* [List Tutorial Exercise 11](#List-Answer-11)
* [List Tutorial Exercise 12](#List-Answer-12)
* [List Tutorial Exercise 13](#List-Answer-13)
* [List Tutorial Exercise 14](#List-Answer-14)
* [Types Tutorial Exercise 1](#Types-Answer-1)
* [Types Tutorial Exercise 2](#Types-Answer-2)
* [Types Tutorial Exercise 3](#Types-Answer-3)
* [Types Tutorial Exercise 4](#Types-Answer-4)
* [Types Tutorial Exercise 5](#Types-Answer-5)
* [Types Tutorial Exercise 6](#Types-Answer-6)
* [Types Tutorial Exercise 7](#Types-Answer-7)
* [Types Tutorial Exercise 8](#Types-Answer-8)
* [Types Tutorial Exercise 9](#Types-Answer-9)
* [Types Tutorial Exercise 10](#Types-Answer-10)
* [Types Tutorial Exercise 11](#Types-Answer-11)
* [Types Tutorial Exercise 12](#Types-Answer-12)
* [Types Tutorial Exercise 13](#Types-Answer-13)
* [Types Tutorial Exercise 14](#Types-Answer-14)
* [Types Tutorial Exercise 15](#Types-Answer-15)
* [Algebra Tutorial Exercise 1](#Algebra-Answer-1)
* [Algebra Tutorial Exercise 2](#Algebra-Answer-2)
* [Algebra Tutorial Exercise 3](#Algebra-Answer-3)
* [Algebra Tutorial Exercise 4](#Algebra-Answer-4)
* [Rewrites Tutorial Exercise 1](#Rewrites-Answer-1)
* [Rewrites Tutorial Exercise 2](#Rewrites-Answer-2)
* [Rewrites Tutorial Exercise 3](#Rewrites-Answer-3)
* [Rewrites Tutorial Exercise 4](#Rewrites-Answer-4)
* [Rewrites Tutorial Exercise 5](#Rewrites-Answer-5)
* [Rewrites Tutorial Exercise 6](#Rewrites-Answer-6)
* [Programming Tutorial Exercise 1](#Programming-Answer-1)
* [Programming Tutorial Exercise 2](#Programming-Answer-2)
* [Programming Tutorial Exercise 3](#Programming-Answer-3)
* [Programming Tutorial Exercise 4](#Programming-Answer-4)
* [Programming Tutorial Exercise 5](#Programming-Answer-5)
* [Programming Tutorial Exercise 6](#Programming-Answer-6)
* [Programming Tutorial Exercise 7](#Programming-Answer-7)
* [Programming Tutorial Exercise 8](#Programming-Answer-8)
* [Programming Tutorial Exercise 9](#Programming-Answer-9)
* [Programming Tutorial Exercise 10](#Programming-Answer-10)
* [Programming Tutorial Exercise 11](#Programming-Answer-11)
* [Programming Tutorial Exercise 12](#Programming-Answer-12)

---

Next: [RPN Tutorial Exercise 2](#RPN-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.1 RPN Tutorial Exercise 1 [¶](#RPN-Tutorial-Exercise-1)

`1 RET 2 RET 3 RET 4 + * -`

The result is
‘`1 - (2 * (3 + 4)) = -13`’.

---

Next: [RPN Tutorial Exercise 3](#RPN-Answer-3), Previous: [RPN Tutorial Exercise 1](#RPN-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.2 RPN Tutorial Exercise 2 [¶](#RPN-Tutorial-Exercise-2)

‘`2*4 + 7*9.5 + 5/4 = 75.75`’

After computing the intermediate term
‘`2*4 = 8`’,
you can leave that result on the stack while you compute the second
term. With both of these results waiting on the stack you can then
compute the final term, then press `+ +` to add everything up.

```
2:  2          1:  8          3:  8          2:  8
1:  4              .          2:  7          1:  66.5
    .                         1:  9.5            .
                                  .

  2 RET 4          *          7 RET 9.5          *
```

```
4:  8          3:  8          2:  8          1:  75.75
3:  66.5       2:  66.5       1:  67.75          .
2:  5          1:  1.25           .
1:  4              .
    .

  5 RET 4          /              +              +
```

Alternatively, you could add the first two terms before going on
with the third term.

```
2:  8          1:  74.5       3:  74.5       2:  74.5       1:  75.75
1:  66.5           .          2:  5          1:  1.25           .
    .                         1:  4              .
                                  .

   ...             +            5 RET 4          /              +
```

On an old-style RPN calculator this second method would have the
advantage of using only three stack levels. But since Calc’s stack
can grow arbitrarily large this isn’t really an issue. Which method
you choose is purely a matter of taste.

---

Next: [RPN Tutorial Exercise 4](#RPN-Answer-4), Previous: [RPN Tutorial Exercise 2](#RPN-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.3 RPN Tutorial Exercise 3 [¶](#RPN-Tutorial-Exercise-3)

The `TAB` key provides a way to operate on the number in level 2.

```
3:  10         3:  10         4:  10         3:  10         3:  10
2:  20         2:  30         3:  30         2:  30         2:  21
1:  30         1:  20         2:  20         1:  21         1:  30
    .              .          1:  1              .              .
                                  .

                  TAB             1              +             TAB
```

Similarly, `M-TAB` gives you access to the number in level 3.

```
3:  10         3:  21         3:  21         3:  30         3:  11
2:  21         2:  30         2:  30         2:  11         2:  21
1:  30         1:  10         1:  11         1:  21         1:  30
    .              .              .              .              .

                  M-TAB           1 +           M-TAB          M-TAB
```

---

Next: [Algebraic Entry Tutorial Exercise 1](#Algebraic-Answer-1), Previous: [RPN Tutorial Exercise 3](#RPN-Answer-3), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.4 RPN Tutorial Exercise 4 [¶](#RPN-Tutorial-Exercise-4)

Either `( 2 , 3 )` or `( 2 SPC 3 )` would have worked,
but using both the comma and the space at once yields:

```
1:  ( ...      2:  ( ...      1:  (2, ...    2:  (2, ...    2:  (2, ...
    .          1:  2              .          1:  (2, ...    1:  (2, 3)
                   .                             .              .

    (              2              ,             SPC            3 )
```

Joe probably tried to type `TAB DEL` to swap the
extra incomplete object to the top of the stack and delete it.
But a feature of Calc is that `DEL` on an incomplete object
deletes just one component out of that object, so he had to press
`DEL` twice to finish the job.

```
2:  (2, ...    2:  (2, 3)     2:  (2, 3)     1:  (2, 3)
1:  (2, 3)     1:  (2, ...    1:  ( ...          .
    .              .              .

                  TAB            DEL            DEL
```

(As it turns out, deleting the second-to-top stack entry happens often
enough that Calc provides a special key, `M-DEL`, to do just that.
`M-DEL` is just like `TAB DEL`, except that it doesn’t exhibit
the “feature” that tripped poor Joe.)

---

Next: [Algebraic Entry Tutorial Exercise 2](#Algebraic-Answer-2), Previous: [RPN Tutorial Exercise 4](#RPN-Answer-4), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.5 Algebraic Entry Tutorial Exercise 1 [¶](#Algebraic-Entry-Tutorial-Exercise-1)

Type `' sqrt($) RET`.

If the `Q` key is broken, you could use `' $^0.5 RET`.
Or, RPN style, `0.5 ^`.

(Actually, ‘`$^1:2`’, using the fraction one-half as the power, is
a closer equivalent, since ‘`9^0.5`’ yields ‘`3.0`’ whereas
‘`sqrt(9)`’ and ‘`9^1:2`’ yield the exact integer ‘`3`’.)

---

Next: [Algebraic Entry Tutorial Exercise 3](#Algebraic-Answer-3), Previous: [Algebraic Entry Tutorial Exercise 1](#Algebraic-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.6 Algebraic Entry Tutorial Exercise 2 [¶](#Algebraic-Entry-Tutorial-Exercise-2)

In the formula ‘`2 x (1+y)`’, ‘`x`’ was interpreted as a function
name with ‘`1+y`’ as its argument. Assigning a value to a variable
has no relation to a function by the same name. Joe needed to use an
explicit ‘`*`’ symbol here: ‘`2 x*(1+y)`’.

---

Next: [Modes Tutorial Exercise 1](#Modes-Answer-1), Previous: [Algebraic Entry Tutorial Exercise 2](#Algebraic-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.7 Algebraic Entry Tutorial Exercise 3 [¶](#Algebraic-Entry-Tutorial-Exercise-3)

The result from `1 RET 0 /` will be the formula ‘`1 / 0`’.
The “function” ‘`/`’ cannot be evaluated when its second argument
is zero, so it is left in symbolic form. When you now type `0 *`,
the result will be zero because Calc uses the general rule that “zero
times anything is zero.”

The `m i` command enables an *Infinite mode* in which ‘`1 / 0`’
results in a special symbol that represents “infinity.” If you
multiply infinity by zero, Calc uses another special new symbol to
show that the answer is “indeterminate.” See [Infinities](#Infinities), for
further discussion of infinite and indeterminate values.

---

Next: [Modes Tutorial Exercise 2](#Modes-Answer-2), Previous: [Algebraic Entry Tutorial Exercise 3](#Algebraic-Answer-3), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.8 Modes Tutorial Exercise 1 [¶](#Modes-Tutorial-Exercise-1)

Calc always stores its floating-point numbers in decimal,
so even though one-third has
an exact base-3 representation (‘`3#0.1`’), it is still stored as
0.3333333 (chopped off after 12 or however many decimal digits) inside
the calculator’s memory. When this inexact number is converted back
to base 3 for display, it may still be slightly inexact. When we
multiply this number by 3, we get 0.999999, also an inexact value.

When Calc displays a number in base 3, it has to decide how many digits
to show. If the current precision is 12 (decimal) digits, that corresponds
to ‘`12 / log10(3) = 25.15`’ base-3 digits. Because 25.15 is not an
exact integer, Calc shows only 25 digits, with the result that stored
numbers carry a little bit of extra information that may not show up on
the screen. When Joe entered ‘`3#0.2`’, the stored number 0.666666
happened to round to a pleasing value when it lost that last 0.15 of a
digit, but it was still inexact in Calc’s memory. When he divided by 2,
he still got the dreaded inexact value 0.333333. (Actually, he divided
0.666667 by 2 to get 0.333334, which is why he got something a little
higher than `3#0.1` instead of a little lower.)

If Joe didn’t want to be bothered with all this, he could have typed
`M-24 d n` to display with one less digit than the default. (If
you give `d n` a negative argument, it uses default-minus-that,
so `M-- d n` would be an easier way to get the same effect.) Those
inexact results would still be lurking there, but they would now be
rounded to nice, natural-looking values for display purposes. (Remember,
‘`0.022222`’ in base 3 is like ‘`0.099999`’ in base 10; rounding
off one digit will round the number up to ‘`0.1`’.) Depending on the
nature of your work, this hiding of the inexactness may be a benefit or
a danger. With the `d n` command, Calc gives you the choice.

Incidentally, another consequence of all this is that if you type
`M-30 d n` to display more digits than are “really there,”
you’ll see garbage digits at the end of the number. (In decimal
display mode, with decimally-stored numbers, these garbage digits are
always zero so they vanish and you don’t notice them.) Because Calc
rounds off that 0.15 digit, there is the danger that two numbers could
be slightly different internally but still look the same. If you feel
uneasy about this, set the `d n` precision to be a little higher
than normal; you’ll get ugly garbage digits, but you’ll always be able
to tell two distinct numbers apart.

An interesting side note is that most computers store their
floating-point numbers in binary, and convert to decimal for display.
Thus everyday programs have the same problem: Decimal 0.1 cannot be
represented exactly in binary (try it: `0.1 d 2`), so ‘`0.1 * 10`’
comes out as an inexact approximation to 1 on some machines (though
they generally arrange to hide it from you by rounding off one digit as
we did above). Because Calc works in decimal instead of binary, you can
be sure that numbers that look exact *are* exact as long as you stay
in decimal display mode.

It’s not hard to show that any number that can be represented exactly
in binary, octal, or hexadecimal is also exact in decimal, so the kinds
of problems we saw in this exercise are likely to be severe only when
you use a relatively unusual radix like 3.

---

Next: [Modes Tutorial Exercise 3](#Modes-Answer-3), Previous: [Modes Tutorial Exercise 1](#Modes-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.9 Modes Tutorial Exercise 2 [¶](#Modes-Tutorial-Exercise-2)

If the radix is 15 or higher, we can’t use the letter ‘`e`’ to mark
the exponent because ‘`e`’ is interpreted as a digit. When Calc
needs to display scientific notation in a high radix, it writes
‘`16#F.E8F*16.^15`’. You can enter a number like this as an
algebraic entry. Also, pressing `e` without any digits before it
normally types `1e`, but in a high radix it types `16.^` and
puts you in algebraic entry: `16#f.e8f RET e 15 RET *` is another
way to enter this number.

The reason Calc puts a decimal point in the ‘`16.^`’ is to prevent
huge integers from being generated if the exponent is large (consider
‘`16#1.23*16^1000`’, where we compute ‘`16^1000`’ as a giant
exact integer and then throw away most of the digits when we multiply
it by the floating-point ‘`16#1.23`’). While this wouldn’t normally
matter for display purposes, it could give you a nasty surprise if you
copied that number into a file and later moved it back into Calc.

---

Next: [Modes Tutorial Exercise 4](#Modes-Answer-4), Previous: [Modes Tutorial Exercise 2](#Modes-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.10 Modes Tutorial Exercise 3 [¶](#Modes-Tutorial-Exercise-3)

The answer he got was ‘`0.5000000000006399`’.

The problem is not that the square operation is inexact, but that the
sine of 45 that was already on the stack was accurate to only 12 places.
Arbitrary-precision calculations still only give answers as good as
their inputs.

The real problem is that there is no 12-digit number which, when
squared, comes out to 0.5 exactly. The `f [` and `f ]`
commands decrease or increase a number by one unit in the last
place (according to the current precision). They are useful for
determining facts like this.

```
1:  0.707106781187      1:  0.500000000001
    .                       .

    45 S                    2 ^
```

```
1:  0.707106781187      1:  0.707106781186      1:  0.499999999999
    .                       .                       .

    U  DEL                  f [                     2 ^
```

A high-precision calculation must be carried out in high precision
all the way. The only number in the original problem which was known
exactly was the quantity 45 degrees, so the precision must be raised
before anything is done after the number 45 has been entered in order
for the higher precision to be meaningful.

---

Next: [Arithmetic Tutorial Exercise 1](#Arithmetic-Answer-1), Previous: [Modes Tutorial Exercise 3](#Modes-Answer-3), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.11 Modes Tutorial Exercise 4 [¶](#Modes-Tutorial-Exercise-4)

Many calculations involve real-world quantities, like the width and
height of a piece of wood or the volume of a jar. Such quantities
can’t be measured exactly anyway, and if the data that is input to
a calculation is inexact, doing exact arithmetic on it is a waste
of time.

Fractions become unwieldy after too many calculations have been
done with them. For example, the sum of the reciprocals of the
integers from 1 to 10 is 7381:2520. The sum from 1 to 30 is
9304682830147:2329089562800. After a point it will take a long
time to add even one more term to this sum, but a floating-point
calculation of the sum will not have this problem.

Also, rational numbers cannot express the results of all calculations.
There is no fractional form for the square root of two, so if you type
`2 Q`, Calc has no choice but to give you a floating-point answer.

---

Next: [Arithmetic Tutorial Exercise 2](#Arithmetic-Answer-2), Previous: [Modes Tutorial Exercise 4](#Modes-Answer-4), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.12 Arithmetic Tutorial Exercise 1 [¶](#Arithmetic-Tutorial-Exercise-1)

Dividing two integers that are larger than the current precision may
give a floating-point result that is inaccurate even when rounded
down to an integer. Consider ‘`123456789 / 2`’ when the current
precision is 6 digits. The true answer is ‘`61728394.5`’, but
with a precision of 6 this will be rounded to
‘`12345700. / 2. = 61728500.`’.
The result, when converted to an integer, will be off by 106.

Here are two solutions: Raise the precision enough that the
floating-point round-off error is strictly to the right of the
decimal point. Or, convert to Fraction mode so that ‘`123456789 / 2`’
produces the exact fraction ‘`123456789:2`’, which can be rounded
down by the `F` command without ever switching to floating-point
format.

---

Next: [Vector Tutorial Exercise 1](#Vector-Answer-1), Previous: [Arithmetic Tutorial Exercise 1](#Arithmetic-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.13 Arithmetic Tutorial Exercise 2 [¶](#Arithmetic-Tutorial-Exercise-2)

`27 RET 9 B` could give the exact result ‘`3:2`’, but it
does a floating-point calculation instead and produces ‘`1.5`’.

Calc will find an exact result for a logarithm if the result is an integer
or (when in Fraction mode) the reciprocal of an integer. But there is
no efficient way to search the space of all possible rational numbers
for an exact answer, so Calc doesn’t try.

---

Next: [Vector Tutorial Exercise 2](#Vector-Answer-2), Previous: [Arithmetic Tutorial Exercise 2](#Arithmetic-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.14 Vector Tutorial Exercise 1 [¶](#Vector-Tutorial-Exercise-1)

Duplicate the vector, compute its length, then divide the vector
by its length: `RET A /`.

```
1:  [1, 2, 3]  2:  [1, 2, 3]      1:  [0.27, 0.53, 0.80]  1:  1.
    .          1:  3.74165738677      .                       .
                   .

    r 1            RET A              /                       A
```

The final `A` command shows that the normalized vector does
indeed have unit length.

---

Next: [Matrix Tutorial Exercise 1](#Matrix-Answer-1), Previous: [Vector Tutorial Exercise 1](#Vector-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.15 Vector Tutorial Exercise 2 [¶](#Vector-Tutorial-Exercise-2)

The average position is equal to the sum of the products of the
positions times their corresponding probabilities. This is the
definition of the dot product operation. So all you need to do
is to put the two vectors on the stack and press `*`.

---

Next: [Matrix Tutorial Exercise 2](#Matrix-Answer-2), Previous: [Vector Tutorial Exercise 2](#Vector-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.16 Matrix Tutorial Exercise 1 [¶](#Matrix-Tutorial-Exercise-1)

The trick is to multiply by a vector of ones. Use `r 4 [1 1 1] *` to
get the row sum. Similarly, use `[1 1] r 4 *` to get the column sum.

---

Next: [Matrix Tutorial Exercise 3](#Matrix-Answer-3), Previous: [Matrix Tutorial Exercise 1](#Matrix-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.17 Matrix Tutorial Exercise 2 [¶](#Matrix-Tutorial-Exercise-2)

```
   x + a y = 6
   x + b y = 10
```

Just enter the righthand side vector, then divide by the lefthand side
matrix as usual.

```
1:  [6, 10]    2:  [6, 10]         1:  [4 a / (a - b) + 6, 4 / (b - a) ]
    .          1:  [ [ 1, a ]          .
                     [ 1, b ] ]
                   .

' [6 10] RET     ' [1 a; 1 b] RET      /
```

This can be made more readable using `d B` to enable Big display
mode:

```
      4 a         4
1:  [----- + 6, -----]
     a - b      b - a
```

Type `d N` to return to Normal display mode afterwards.

---

Next: [List Tutorial Exercise 1](#List-Answer-1), Previous: [Matrix Tutorial Exercise 2](#Matrix-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.18 Matrix Tutorial Exercise 3 [¶](#Matrix-Tutorial-Exercise-3)

To solve
‘`trn(A) * A * X = trn(A) * B`’,
first we compute
‘`A2 = trn(A) * A`’
and
‘`B2 = trn(A) * B`’;
now, we have a system
‘`A2 * X = B2`’
which we can solve using Calc’s ‘`/`’ command.

```
    a + 2b + 3c = 6
   4a + 5b + 6c = 2
   7a + 6b      = 3
   2a + 4b + 6c = 11
```

The first step is to enter the coefficient matrix. We’ll store it in
quick variable number 7 for later reference. Next, we compute the
‘`B2`’
vector.

```
1:  [ [ 1, 2, 3 ]             2:  [ [ 1, 4, 7, 2 ]     1:  [57, 84, 96]
      [ 4, 5, 6 ]                   [ 2, 5, 6, 4 ]         .
      [ 7, 6, 0 ]                   [ 3, 6, 0, 6 ] ]
      [ 2, 4, 6 ] ]           1:  [6, 2, 3, 11]
    .                             .

' [1 2 3; 4 5 6; 7 6 0; 2 4 6] RET  s 7  v t  [6 2 3 11]   *
```

Now we compute the matrix
‘`A2`’
and divide.

```
2:  [57, 84, 96]          1:  [-11.64, 14.08, -3.64]
1:  [ [ 70, 72, 39 ]          .
      [ 72, 81, 60 ]
      [ 39, 60, 81 ] ]
    .

    r 7 v t r 7 *             /
```

(The actual computed answer will be slightly inexact due to
round-off error.)

Notice that the answers are similar to those for the
3x3
system solved in the text. That’s because the fourth equation that was
added to the system is almost identical to the first one multiplied
by two. (If it were identical, we would have gotten the exact same
answer since the
4x3
system would be equivalent to the original
3x3
system.)

Since the first and fourth equations aren’t quite equivalent, they
can’t both be satisfied at once. Let’s plug our answers back into
the original system of equations to see how well they match.

```
2:  [-11.64, 14.08, -3.64]     1:  [5.6, 2., 3., 11.2]
1:  [ [ 1, 2, 3 ]                  .
      [ 4, 5, 6 ]
      [ 7, 6, 0 ]
      [ 2, 4, 6 ] ]
    .

    r 7                            TAB *
```

This is reasonably close to our original ‘`B`’ vector,
‘`[6, 2, 3, 11]`’.

---

Next: [List Tutorial Exercise 2](#List-Answer-2), Previous: [Matrix Tutorial Exercise 3](#Matrix-Answer-3), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.19 List Tutorial Exercise 1 [¶](#List-Tutorial-Exercise-1)

We can use `v x` to build a vector of integers. This needs to be
adjusted to get the range of integers we desire. Mapping ‘`-`’
across the vector will accomplish this, although it turns out the
plain ‘`-`’ key will work just as well.

```
2:  2                              2:  2
1:  [1, 2, 3, 4, 5, 6, 7, 8, 9]    1:  [-4, -3, -2, -1, 0, 1, 2, 3, 4]
    .                                  .

    2  v x 9 RET                       5 V M -   or   5 -
```

Now we use `V M ^` to map the exponentiation operator across the
vector.

```
1:  [0.0625, 0.125, 0.25, 0.5, 1, 2, 4, 8, 16]
    .

    V M ^
```

---

Next: [List Tutorial Exercise 3](#List-Answer-3), Previous: [List Tutorial Exercise 1](#List-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.20 List Tutorial Exercise 2 [¶](#List-Tutorial-Exercise-2)

Given ‘`x`’ and ‘`y`’ vectors in quick variables 1 and 2 as before,
the first job is to form the matrix that describes the problem.

```
   m*x + b*1 = y
```

Thus we want a
19x2
matrix with our ‘`x`’ vector as one column and
ones as the other column. So, first we build the column of ones, then
we combine the two columns to form our ‘`A`’ matrix.

```
2:  [1.34, 1.41, 1.49, ... ]    1:  [ [ 1.34, 1 ]
1:  [1, 1, 1, ...]                    [ 1.41, 1 ]
    .                                 [ 1.49, 1 ]
                                      ...

    r 1 1 v b 19 RET                M-2 v p v t   s 3
```

Now we compute
‘`trn(A) * y`’
and
‘`trn(A) * A`’
and divide.

```
1:  [33.36554, 13.613]    2:  [33.36554, 13.613]
    .                     1:  [ [ 98.0003, 41.63 ]
                                [  41.63,   19   ] ]
                              .

 v t r 2 *                    r 3 v t r 3 *
```

(Hey, those numbers look familiar!)

```
1:  [0.52141679, -0.425978]
    .

    /
```

Since we were solving equations of the form
‘`m*x + b*1 = y`’,
these numbers should be ‘`m`’ and ‘`b`’, respectively. Sure
enough, they agree exactly with the result computed using `V M` and
`V R`!

The moral of this story: `V M` and `V R` will probably solve
your problem, but there is often an easier way using the higher-level
arithmetic functions!

In fact, there is a built-in `a F` command that does least-squares
fits. See [Curve Fitting](#Curve-Fitting).

---

Next: [List Tutorial Exercise 4](#List-Answer-4), Previous: [List Tutorial Exercise 2](#List-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.21 List Tutorial Exercise 3 [¶](#List-Tutorial-Exercise-3)

Move to one end of the list and press `C-@` (or `C-SPC` or
whatever) to set the mark, then move to the other end of the list
and type `C-x * g`.

```
1:  [2.3, 6, 22, 15.1, 7, 15, 14, 7.5, 2.5]
    .
```

To make things interesting, let’s assume we don’t know at a glance
how many numbers are in this list. Then we could type:

```
2:  [2.3, 6, 22, ... ]     2:  [2.3, 6, 22, ... ]
1:  [2.3, 6, 22, ... ]     1:  126356422.5
    .                          .

    RET                        V R *
```

```
2:  126356422.5            2:  126356422.5     1:  7.94652913734
1:  [2.3, 6, 22, ... ]     1:  9                   .
    .                          .

    TAB                        v l                 I ^
```

(The `I ^` command computes the nth root of a number.
You could also type `& ^` to take the reciprocal of 9 and
then raise the number to that power.)

---

Next: [List Tutorial Exercise 5](#List-Answer-5), Previous: [List Tutorial Exercise 3](#List-Answer-3), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.22 List Tutorial Exercise 4 [¶](#List-Tutorial-Exercise-4)

A number ‘`j`’ is a divisor of ‘`n`’ if
‘`n % j = 0`’.
The first step is to get a vector that identifies the divisors.

```
2:  30                  2:  [0, 0, 0, 2, ...]    1:  [1, 1, 1, 0, ...]
1:  [1, 2, 3, 4, ...]   1:  0                        .
    .                       .

 30 RET v x 30 RET   s 1    V M %  0                 V M a =  s 2
```

This vector has 1’s marking divisors of 30 and 0’s marking non-divisors.

The zeroth divisor function is just the total number of divisors.
The first divisor function is the sum of the divisors.

```
1:  8      3:  8                    2:  8                    2:  8
           2:  [1, 2, 3, 4, ...]    1:  [1, 2, 3, 0, ...]    1:  72
           1:  [1, 1, 1, 0, ...]        .                        .
               .

   V R +       r 1 r 2                  V M *                  V R +
```

Once again, the last two steps just compute a dot product for which
a simple `*` would have worked equally well.

---

Next: [List Tutorial Exercise 6](#List-Answer-6), Previous: [List Tutorial Exercise 4](#List-Answer-4), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.23 List Tutorial Exercise 5 [¶](#List-Tutorial-Exercise-5)

The obvious first step is to obtain the list of factors with `k f`.
This list will always be in sorted order, so if there are duplicates
they will be right next to each other. A suitable method is to compare
the list with a copy of itself shifted over by one.

```
1:  [3, 7, 7, 7, 19]   2:  [3, 7, 7, 7, 19]     2:  [3, 7, 7, 7, 19, 0]
    .                  1:  [3, 7, 7, 7, 19, 0]  1:  [0, 3, 7, 7, 7, 19]
                           .                        .

    19551 k f              RET 0 |                  TAB 0 TAB |
```

```
1:  [0, 0, 1, 1, 0, 0]   1:  2          1:  0
    .                        .              .

    V M a =                  V R +          0 a =
```

Note that we have to arrange for both vectors to have the same length
so that the mapping operation works; no prime factor will ever be
zero, so adding zeros on the left and right is safe. From then on
the job is pretty straightforward.

Incidentally, Calc provides the *Möbius μ*
function which is zero if and only if its argument is square-free. It
would be a much more convenient way to do the above test in practice.

---

Next: [List Tutorial Exercise 7](#List-Answer-7), Previous: [List Tutorial Exercise 5](#List-Answer-5), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.24 List Tutorial Exercise 6 [¶](#List-Tutorial-Exercise-6)

First use `v x 6 RET` to get a list of integers, then `V M v x`
to get a list of lists of integers!

---

Next: [List Tutorial Exercise 8](#List-Answer-8), Previous: [List Tutorial Exercise 6](#List-Answer-6), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.25 List Tutorial Exercise 7 [¶](#List-Tutorial-Exercise-7)

Here’s one solution. First, compute the triangular list from the previous
exercise and type `1 -` to subtract one from all the elements.

```
1:  [ [0],
      [0, 1],
      [0, 1, 2],
      ...

    1 -
```

The numbers down the lefthand edge of the list we desire are called
the “triangular numbers” (now you know why!). The ‘`n`’th
triangular number is the sum of the integers from 1 to ‘`n`’, and
can be computed directly by the formula
‘`n * (n+1) / 2`’.

```
2:  [ [0], [0, 1], ... ]    2:  [ [0], [0, 1], ... ]
1:  [0, 1, 2, 3, 4, 5]      1:  [0, 1, 3, 6, 10, 15]
    .                           .

    v x 6 RET 1 -               V M ' $ ($+1)/2 RET
```

Adding this list to the above list of lists produces the desired
result:

```
1:  [ [0],
      [1, 2],
      [3, 4, 5],
      [6, 7, 8, 9],
      [10, 11, 12, 13, 14],
      [15, 16, 17, 18, 19, 20] ]
      .

      V M +
```

If we did not know the formula for triangular numbers, we could have
computed them using a `V U +` command. We could also have
gotten them the hard way by mapping a reduction across the original
triangular list.

```
2:  [ [0], [0, 1], ... ]    2:  [ [0], [0, 1], ... ]
1:  [ [0], [0, 1], ... ]    1:  [0, 1, 3, 6, 10, 15]
    .                           .

    RET                         V M V R +
```

(This means “map a `V R +` command across the vector,” and
since each element of the main vector is itself a small vector,
`V R +` computes the sum of its elements.)

---

Next: [List Tutorial Exercise 9](#List-Answer-9), Previous: [List Tutorial Exercise 7](#List-Answer-7), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.26 List Tutorial Exercise 8 [¶](#List-Tutorial-Exercise-8)

The first step is to build a list of values of ‘`x`’.

```
1:  [1, 2, 3, ..., 21]  1:  [0, 1, 2, ..., 20]  1:  [0, 0.25, 0.5, ..., 5]
    .                       .                       .

    v x 21 RET              1 -                     4 /  s 1
```

Next, we compute the Bessel function values.

```
1:  [0., 0.124, 0.242, ..., -0.328]
    .

    V M ' besJ(1,$) RET
```

(Another way to do this would be `1 TAB V M f j`.)

A way to isolate the maximum value is to compute the maximum using
`V R X`, then compare all the Bessel values with that maximum.

```
2:  [0., 0.124, 0.242, ... ]   1:  [0, 0, 0, ... ]    2:  [0, 0, 0, ... ]
1:  0.5801562                      .                  1:  1
    .                                                     .

    RET V R X                      V M a =                RET V R +    DEL
```

It’s a good idea to verify, as in the last step above, that only
one value is equal to the maximum. (After all, a plot of
‘`sin(x)`’
might have many points all equal to the maximum value, 1.)

The vector we have now has a single 1 in the position that indicates
the maximum value of ‘`x`’. Now it is a simple matter to convert
this back into the corresponding value itself.

```
2:  [0, 0, 0, ... ]         1:  [0, 0., 0., ... ]    1:  1.75
1:  [0, 0.25, 0.5, ... ]        .                        .
    .

    r 1                         V M *                    V R +
```

If `a =` had produced more than one ‘`1`’ value, this method
would have given the sum of all maximum ‘`x`’ values; not very
useful! In this case we could have used `v m` (`calc-mask-vector`)
instead. This command deletes all elements of a “data” vector that
correspond to zeros in a “mask” vector, leaving us with, in this
example, a vector of maximum ‘`x`’ values.

The built-in `a X` command maximizes a function using more
efficient methods. Just for illustration, let’s use `a X`
to maximize ‘`besJ(1,x)`’ over this same interval.

```
2:  besJ(1, x)                 1:  [1.84115, 0.581865]
1:  [0 .. 5]                       .
    .

' besJ(1,x), [0..5] RET            a X x RET
```

The output from `a X` is a vector containing the value of ‘`x`’
that maximizes the function, and the function’s value at that maximum.
As you can see, our simple search got quite close to the right answer.

---

Next: [List Tutorial Exercise 10](#List-Answer-10), Previous: [List Tutorial Exercise 8](#List-Answer-8), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.27 List Tutorial Exercise 9 [¶](#List-Tutorial-Exercise-9)

Step one is to convert our integer into vector notation.

```
1:  25129925999           3:  25129925999
    .                     2:  10
                          1:  [11, 10, 9, ..., 1, 0]
                              .

    25129925999 RET           10 RET 12 RET v x 12 RET -
```

```
1:  25129925999              1:  [0, 2, 25, 251, 2512, ... ]
2:  [100000000000, ... ]         .
    .

    V M ^   s 1                  V M \
```

(Recall, the `\` command computes an integer quotient.)

```
1:  [0, 2, 5, 1, 2, 9, 9, 2, 5, 9, 9, 9]
    .

    10 V M %   s 2
```

Next we must increment this number. This involves adding one to
the last digit, plus handling carries. There is a carry to the
left out of a digit if that digit is a nine and all the digits to
the right of it are nines.

```
1:  [0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 1, 1]   1:  [1, 1, 1, 0, 0, 1, ... ]
    .                                          .

    9 V M a =                                  v v
```

```
1:  [1, 1, 1, 0, 0, 0, ... ]   1:  [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1]
    .                              .

    V U *                          v v 1 |
```

Accumulating `*` across a vector of ones and zeros will preserve
only the initial run of ones. These are the carries into all digits
except the rightmost digit. Concatenating a one on the right takes
care of aligning the carries properly, and also adding one to the
rightmost digit.

```
2:  [0, 0, 0, 0, ... ]     1:  [0, 0, 2, 5, 1, 2, 9, 9, 2, 6, 0, 0, 0]
1:  [0, 0, 2, 5, ... ]         .
    .

    0 r 2 |                    V M +  10 V M %
```

Here we have concatenated 0 to the *left* of the original number;
this takes care of shifting the carries by one with respect to the
digits that generated them.

Finally, we must convert this list back into an integer.

```
3:  [0, 0, 2, 5, ... ]        2:  [0, 0, 2, 5, ... ]
2:  1000000000000             1:  [1000000000000, 100000000000, ... ]
1:  [100000000000, ... ]          .
    .

    10 RET 12 ^  r 1              |
```

```
1:  [0, 0, 20000000000, 5000000000, ... ]    1:  25129926000
    .                                            .

    V M *                                        V R +
```

Another way to do this final step would be to reduce the formula
‘`10 $$ + $`’ across the vector of digits.

```
1:  [0, 0, 2, 5, ... ]        1:  25129926000
    .                             .

                                  V R ' 10 $$ + $ RET
```

---

Next: [List Tutorial Exercise 11](#List-Answer-11), Previous: [List Tutorial Exercise 9](#List-Answer-9), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.28 List Tutorial Exercise 10 [¶](#List-Tutorial-Exercise-10)

For the list ‘`[a, b, c, d]`’, the result is ‘`((a = b) = c) = d`’,
which will compare ‘`a`’ and ‘`b`’ to produce a 1 or 0, which is
then compared with ‘`c`’ to produce another 1 or 0, which is then
compared with ‘`d`’. This is not at all what Joe wanted.

Here’s a more correct method:

```
1:  [7, 7, 7, 8, 7]      2:  [7, 7, 7, 8, 7]
    .                    1:  7
                             .

  ' [7,7,7,8,7] RET          RET v r 1 RET
```

```
1:  [1, 1, 1, 0, 1]      1:  0
    .                        .

    V M a =                  V R *
```

---

Next: [List Tutorial Exercise 12](#List-Answer-12), Previous: [List Tutorial Exercise 10](#List-Answer-10), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.29 List Tutorial Exercise 11 [¶](#List-Tutorial-Exercise-11)

The circle of unit radius consists of those points ‘`(x,y)`’ for which
‘`x^2 + y^2 < 1`’. We start by generating a vector of ‘`x^2`’
and a vector of ‘`y^2`’.

We can make this go a bit faster by using the `v .` and `t .`
commands.

```
2:  [2., 2., ..., 2.]          2:  [2., 2., ..., 2.]
1:  [2., 2., ..., 2.]          1:  [1.16, 1.98, ..., 0.81]
    .                              .

 v . t .  2. v b 100 RET RET       V M k r
```

```
2:  [2., 2., ..., 2.]          1:  [0.026, 0.96, ..., 0.036]
1:  [0.026, 0.96, ..., 0.036]  2:  [0.53, 0.81, ..., 0.094]
    .                              .

    1 -  2 V M ^                   TAB  V M k r  1 -  2 V M ^
```

Now we sum the ‘`x^2`’ and ‘`y^2`’ values, compare with 1 to
get a vector of 1/0 truth values, then sum the truth values.

```
1:  [0.56, 1.78, ..., 0.13]    1:  [1, 0, ..., 1]    1:  84
    .                              .                     .

    +                              1 V M a <             V R +
```

The ratio ‘`84/100`’ should approximate the ratio ‘`pi/4`’.

```
1:  0.84       1:  3.36       2:  3.36       1:  1.0695
    .              .          1:  3.14159        .

    100 /          4 *            P              /
```

Our estimate, 3.36, is off by about 7%. We could get a better estimate
by taking more points (say, 1000), but it’s clear that this method is
not very efficient!

(Naturally, since this example uses random numbers your own answer
will be slightly different from the one shown here!)

If you typed `v .` and `t .` before, type them again to
return to full-sized display of vectors.

---

Next: [List Tutorial Exercise 13](#List-Answer-13), Previous: [List Tutorial Exercise 11](#List-Answer-11), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.30 List Tutorial Exercise 12 [¶](#List-Tutorial-Exercise-12)

This problem can be made a lot easier by taking advantage of some
symmetries. First of all, after some thought it’s clear that the
‘`y`’ axis can be ignored altogether. Just pick a random ‘`x`’
component for one end of the match, pick a random direction
‘`theta`’,
and see if ‘`x`’ and
‘`x + cos(theta)`’
(which is the ‘`x`’ coordinate of the other endpoint) cross a line.
The lines are at integer coordinates, so this happens when the two
numbers surround an integer.

Since the two endpoints are equivalent, we may as well choose the leftmost
of the two endpoints as ‘`x`’. Then ‘`theta`’ is an angle pointing
to the right, in the range -90 to 90 degrees. (We could use radians, but
it would feel like cheating to refer to ‘`pi/2`’ radians while trying
to estimate ‘`pi`’!)

In fact, since the field of lines is infinite we can choose the
coordinates 0 and 1 for the lines on either side of the leftmost
endpoint. The rightmost endpoint will be between 0 and 1 if the
match does not cross a line, or between 1 and 2 if it does. So:
Pick random ‘`x`’ and
‘`theta`’,
compute
‘`x + cos(theta)`’,
and count how many of the results are greater than one. Simple!

We can make this go a bit faster by using the `v .` and `t .`
commands.

```
1:  [0.52, 0.71, ..., 0.72]    2:  [0.52, 0.71, ..., 0.72]
    .                          1:  [78.4, 64.5, ..., -42.9]
                                   .

v . t . 1. v b 100 RET  V M k r    180. v b 100 RET  V M k r  90 -
```

(The next step may be slow, depending on the speed of your computer.)

```
2:  [0.52, 0.71, ..., 0.72]    1:  [0.72, 1.14, ..., 1.45]
1:  [0.20, 0.43, ..., 0.73]        .
    .

    m d  V M C                     +
```

```
1:  [0, 1, ..., 1]       1:  0.64            1:  3.125
    .                        .                   .

    1 V M a >                V R + 100 /         2 TAB /
```

Let’s try the third method, too. We’ll use random integers up to
one million. The `k r` command with an integer argument picks
a random integer.

```
2:  [1000000, 1000000, ..., 1000000]   2:  [78489, 527587, ..., 814975]
1:  [1000000, 1000000, ..., 1000000]   1:  [324014, 358783, ..., 955450]
    .                                      .

    1000000 v b 100 RET RET                V M k r  TAB  V M k r
```

```
1:  [1, 1, ..., 25]      1:  [1, 1, ..., 0]     1:  0.56
    .                        .                      .

    V M k g                  1 V M a =              V R + 100 /
```

```
1:  10.714        1:  3.273
    .                 .

    6 TAB /           Q
```

For a proof of this property of the GCD function, see section 4.5.2,
exercise 10, of Knuth’s *Art of Computer Programming*, volume II.

If you typed `v .` and `t .` before, type them again to
return to full-sized display of vectors.

---

Next: [List Tutorial Exercise 14](#List-Answer-14), Previous: [List Tutorial Exercise 12](#List-Answer-12), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.31 List Tutorial Exercise 13 [¶](#List-Tutorial-Exercise-13)

First, we put the string on the stack as a vector of ASCII codes.

```
1:  [84, 101, 115, ..., 51]
    .

    "Testing, 1, 2, 3 RET
```

Note that the `"` key, like `$`, initiates algebraic entry so
there was no need to type an apostrophe. Also, Calc didn’t mind that
we omitted the closing `"`. (The same goes for all closing delimiters
like `)` and `]` at the end of a formula.

We’ll show two different approaches here. In the first, we note that
if the input vector is ‘`[a, b, c, d]`’, then the hash code is
‘`3 (3 (3a + b) + c) + d = 27a + 9b + 3c + d`’. In other words,
it’s a sum of descending powers of three times the ASCII codes.

```
2:  [84, 101, 115, ..., 51]    2:  [84, 101, 115, ..., 51]
1:  16                         1:  [15, 14, 13, ..., 0]
    .                              .

    RET v l                        v x 16 RET -
```

```
2:  [84, 101, 115, ..., 51]    1:  1960915098    1:  121
1:  [14348907, ..., 1]             .                 .
    .

    3 TAB V M ^                    *                 511 %
```

Once again, `*` elegantly summarizes most of the computation.
But there’s an even more elegant approach: Reduce the formula
`3 $$ + $` across the vector. Recall that this represents a
function of two arguments that computes its first argument times three
plus its second argument.

```
1:  [84, 101, 115, ..., 51]    1:  1960915098
    .                              .

    "Testing, 1, 2, 3 RET          V R ' 3$$+$ RET
```

If you did the decimal arithmetic exercise, this will be familiar.
Basically, we’re turning a base-3 vector of digits into an integer,
except that our “digits” are much larger than real digits.

Instead of typing `511 %` again to reduce the result, we can be
cleverer still and notice that rather than computing a huge integer
and taking the modulo at the end, we can take the modulo at each step
without affecting the result. While this means there are more
arithmetic operations, the numbers we operate on remain small so
the operations are faster.

```
1:  [84, 101, 115, ..., 51]    1:  121
    .                              .

    "Testing, 1, 2, 3 RET          V R ' (3$$+$)%511 RET
```

Why does this work? Think about a two-step computation:
‘`3 (3a + b) + c`’. Taking a result modulo 511 basically means
subtracting off enough 511’s to put the result in the desired range.
So the result when we take the modulo after every step is,

```
3 (3 a + b - 511 m) + c - 511 n
```

for some suitable integers ‘`m`’ and ‘`n`’. Expanding out by
the distributive law yields

```
9 a + 3 b + c - 511*3 m - 511 n
```

The ‘`m`’ term in the latter formula is redundant because any
contribution it makes could just as easily be made by the ‘`n`’
term. So we can take it out to get an equivalent formula with
‘`n' = 3m + n`’,

```
9 a + 3 b + c - 511 n'
```

which is just the formula for taking the modulo only at the end of
the calculation. Therefore the two methods are essentially the same.

Later in the tutorial we will encounter *modulo forms*, which
basically automate the idea of reducing every intermediate result
modulo some value m.

---

Next: [Types Tutorial Exercise 1](#Types-Answer-1), Previous: [List Tutorial Exercise 13](#List-Answer-13), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.32 List Tutorial Exercise 14 [¶](#List-Tutorial-Exercise-14)

We want to use `H V U` to nest a function which adds a random
step to an ‘`(x,y)`’ coordinate. The function is a bit long, but
otherwise the problem is quite straightforward.

```
2:  [0, 0]     1:  [ [    0,       0    ]
1:  50               [  0.4288, -0.1695 ]
    .                [ -0.4787, -0.9027 ]
                     ...

    [0,0] 50       H V U ' <# + [random(2.0)-1, random(2.0)-1]> RET
```

Just as the text recommended, we used ‘`< >`’ nameless function
notation to keep the two `random` calls from being evaluated
before nesting even begins.

We now have a vector of ‘`[x, y]`’ sub-vectors, which by Calc’s
rules acts like a matrix. We can transpose this matrix and unpack
to get a pair of vectors, ‘`x`’ and ‘`y`’, suitable for graphing.

```
2:  [ 0, 0.4288, -0.4787, ... ]
1:  [ 0, -0.1696, -0.9027, ... ]
    .

    v t  v u  g f
```

Incidentally, because the ‘`x`’ and ‘`y`’ are completely
independent in this case, we could have done two separate commands
to create our ‘`x`’ and ‘`y`’ vectors of numbers directly.

To make a random walk of unit steps, we note that `sincos` of
a random direction exactly gives us an ‘`[x, y]`’ step of unit
length; in fact, the new nesting function is even briefer, though
we might want to lower the precision a bit for it.

```
2:  [0, 0]     1:  [ [    0,      0    ]
1:  50               [  0.1318, 0.9912 ]
    .                [ -0.5965, 0.3061 ]
                     ...

    [0,0] 50   m d  p 6 RET   H V U ' <# + sincos(random(360.0))> RET
```

Another `v t v u g f` sequence will graph this new random walk.

An interesting twist on these random walk functions would be to use
complex numbers instead of 2-vectors to represent points on the plane.
In the first example, we’d use something like ‘`random + random*(0,1)`’,
and in the second we could use polar complex numbers with random phase
angles. (This exercise was first suggested in this form by Randal
Schwartz.)

---

Next: [Types Tutorial Exercise 2](#Types-Answer-2), Previous: [List Tutorial Exercise 14](#List-Answer-14), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.33 Types Tutorial Exercise 1 [¶](#Types-Tutorial-Exercise-1)

If the number is the square root of ‘`pi`’ times a rational number,
then its square, divided by ‘`pi`’, should be a rational number.

```
1:  1.26508260337    1:  0.509433962268   1:  2486645810:4881193627
    .                    .                    .

                         2 ^ P /              c F
```

Technically speaking this is a rational number, but not one that is
likely to have arisen in the original problem. More likely, it just
happens to be the fraction which most closely represents some
irrational number to within 12 digits.

But perhaps our result was not quite exact. Let’s reduce the
precision slightly and try again:

```
1:  0.509433962268     1:  27:53
    .                      .

    U p 10 RET             c F
```

Aha! It’s unlikely that an irrational number would equal a fraction
this simple to within ten digits, so our original number was probably
‘`sqrt(27 pi / 53)`’.

Notice that we didn’t need to re-round the number when we reduced the
precision. Remember, arithmetic operations always round their inputs
to the current precision before they begin.

---

Next: [Types Tutorial Exercise 3](#Types-Answer-3), Previous: [Types Tutorial Exercise 1](#Types-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.34 Types Tutorial Exercise 2 [¶](#Types-Tutorial-Exercise-2)

‘`inf / inf = nan`’. Perhaps ‘`1`’ is the “obvious” answer.
But if ‘`17 inf = inf`’, then ‘`17 inf / inf = inf / inf = 17`’, too.

‘`exp(inf) = inf`’. It’s tempting to say that the exponential
of infinity must be “bigger” than “regular” infinity, but as
far as Calc is concerned all infinities are the same size.
In other words, as ‘`x`’ goes to infinity, ‘`e^x`’ also goes
to infinity, but the fact the ‘`e^x`’ grows much faster than
‘`x`’ is not relevant here.

‘`exp(-inf) = 0`’. Here we have a finite answer even though
the input is infinite.

‘`sqrt(-inf) = (0, 1) inf`’. Remember that ‘`(0, 1)`’
represents the imaginary number ‘`i`’. Here’s a derivation:
‘`sqrt(-inf) = sqrt((-1) * inf) = sqrt(-1) * sqrt(inf)`’.
The first part is, by definition, ‘`i`’; the second is `inf`
because, once again, all infinities are the same size.

‘`sqrt(uinf) = uinf`’. In fact, we do know something about the
direction because `sqrt` is defined to return a value in the
right half of the complex plane. But Calc has no notation for this,
so it settles for the conservative answer `uinf`.

‘`abs(uinf) = inf`’. No matter which direction ‘`x`’ points,
‘`abs(x)`’ always points along the positive real axis.

‘`ln(0) = -inf`’. Here we have an infinite answer to a finite
input. As in the ‘`1 / 0`’ case, Calc will only use infinities
here if you have turned on Infinite mode. Otherwise, it will
treat ‘`ln(0)`’ as an error.

---

Next: [Types Tutorial Exercise 4](#Types-Answer-4), Previous: [Types Tutorial Exercise 2](#Types-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.35 Types Tutorial Exercise 3 [¶](#Types-Tutorial-Exercise-3)

We can make ‘`inf - inf`’ be any real number we like, say,
‘`a`’, just by claiming that we added ‘`a`’ to the first
infinity but not to the second. This is just as true for complex
values of ‘`a`’, so `nan` can stand for a complex number.
(And, similarly, `uinf` can stand for an infinity that points
in any direction in the complex plane, such as ‘`(0, 1) inf`’).

In fact, we can multiply the first `inf` by two. Surely
‘`2 inf - inf = inf`’, but also ‘`2 inf - inf = inf - inf = nan`’.
So `nan` can even stand for infinity. Obviously it’s just
as easy to make it stand for minus infinity as for plus infinity.

The moral of this story is that “infinity” is a slippery fish
indeed, and Calc tries to handle it by having a very simple model
for infinities (only the direction counts, not the “size”); but
Calc is careful to write `nan` any time this simple model is
unable to tell what the true answer is.

---

Next: [Types Tutorial Exercise 5](#Types-Answer-5), Previous: [Types Tutorial Exercise 3](#Types-Answer-3), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.36 Types Tutorial Exercise 4 [¶](#Types-Tutorial-Exercise-4)

```
2:  0@ 47' 26"              1:  0@ 2' 47.411765"
1:  17                          .
    .

    0@ 47' 26" RET 17           /
```

The average song length is two minutes and 47.4 seconds.

```
2:  0@ 2' 47.411765"     1:  0@ 3' 7.411765"    1:  0@ 53' 6.000005"
1:  0@ 0' 20"                .                      .
    .

    20"                      +                      17 *
```

The album would be 53 minutes and 6 seconds long.

---

Next: [Types Tutorial Exercise 6](#Types-Answer-6), Previous: [Types Tutorial Exercise 4](#Types-Answer-4), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.37 Types Tutorial Exercise 5 [¶](#Types-Tutorial-Exercise-5)

Let’s suppose it’s January 14, 1991. The easiest thing to do is
to keep trying 13ths of months until Calc reports a Friday.
We can do this by manually entering dates, or by using `t I`:

```
1:  <Wed Feb 13, 1991>    1:  <Wed Mar 13, 1991>   1:  <Sat Apr 13, 1991>
    .                         .                        .

    ' <2/13> RET       DEL    ' <3/13> RET             t I
```

(Calc assumes the current year if you don’t say otherwise.)

This is getting tedious—we can keep advancing the date by typing
`t I` over and over again, but let’s automate the job by using
vector mapping. The `t I` command actually takes a second
“how-many-months” argument, which defaults to one. This
argument is exactly what we want to map over:

```
2:  <Sat Apr 13, 1991>     1:  [<Mon May 13, 1991>, <Thu Jun 13, 1991>,
1:  [1, 2, 3, 4, 5, 6]          <Sat Jul 13, 1991>, <Tue Aug 13, 1991>,
    .                           <Fri Sep 13, 1991>, <Sun Oct 13, 1991>]
                               .

    v x 6 RET                  V M t I
```

Et voilà, September 13, 1991 is a Friday.

```
1:  242
    .

' <sep 13> - <jan 14> RET
```

And the answer to our original question: 242 days to go.

---

Next: [Types Tutorial Exercise 7](#Types-Answer-7), Previous: [Types Tutorial Exercise 5](#Types-Answer-5), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.38 Types Tutorial Exercise 6 [¶](#Types-Tutorial-Exercise-6)

The full rule for leap years is that they occur in every year divisible
by four, except that they don’t occur in years divisible by 100, except
that they *do* in years divisible by 400. We could work out the
answer by carefully counting the years divisible by four and the
exceptions, but there is a much simpler way that works even if we
don’t know the leap year rule.

Let’s assume the present year is 1991. Years have 365 days, except
that leap years (whenever they occur) have 366 days. So let’s count
the number of days between now and then, and compare that to the
number of years times 365. The number of extra days we find must be
equal to the number of leap years there were.

```
1:  <Mon Jan 1, 10001>     2:  <Mon Jan 1, 10001>     1:  2925593
    .                      1:  <Tue Jan 1, 1991>          .
                               .

  ' <jan 1 10001> RET         ' <jan 1 1991> RET          -
```

```
3:  2925593       2:  2925593     2:  2925593     1:  1943
2:  10001         1:  8010        1:  2923650         .
1:  1991              .               .
    .

  10001 RET 1991      -               365 *           -
```

There will be 1943 leap years before the year 10001. (Assuming,
of course, that the algorithm for computing leap years remains
unchanged for that long. See [Date Forms](#Date-Forms), for some interesting
background information in that regard.)

---

Next: [Types Tutorial Exercise 8](#Types-Answer-8), Previous: [Types Tutorial Exercise 6](#Types-Answer-6), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.39 Types Tutorial Exercise 7 [¶](#Types-Tutorial-Exercise-7)

The relative errors must be converted to absolute errors so that
‘`+/-`’ notation may be used.

```
1:  1.              2:  1.
    .               1:  0.2
                        .

    20 RET .05 *        4 RET .05 *
```

Now we simply chug through the formula.

```
1:  19.7392088022    1:  394.78 +/- 19.739    1:  6316.5 +/- 706.21
    .                    .                        .

    2 P 2 ^ *            20 p 1 *                 4 p .2 RET 2 ^ *
```

It turns out the `v u` command will unpack an error form as
well as a vector. This saves us some retyping of numbers.

```
3:  6316.5 +/- 706.21     2:  6316.5 +/- 706.21
2:  6316.5                1:  0.1118
1:  706.21                    .
    .

    RET v u                   TAB /
```

Thus the volume is 6316 cubic centimeters, within about 11 percent.

---

Next: [Types Tutorial Exercise 9](#Types-Answer-9), Previous: [Types Tutorial Exercise 7](#Types-Answer-7), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.40 Types Tutorial Exercise 8 [¶](#Types-Tutorial-Exercise-8)

The first answer is pretty simple: ‘`1 / (0 .. 10) = (0.1 .. inf)`’.
Since a number in the interval ‘`(0 .. 10)`’ can get arbitrarily
close to zero, its reciprocal can get arbitrarily large, so the answer
is an interval that effectively means, “any number greater than 0.1”
but with no upper bound.

The second answer, similarly, is ‘`1 / (-10 .. 0) = (-inf .. -0.1)`’.

Calc normally treats division by zero as an error, so that the formula
‘`1 / 0`’ is left unsimplified. Our third problem,
‘`1 / [0 .. 10]`’, also (potentially) divides by zero because zero
is now a member of the interval. So Calc leaves this one unevaluated, too.

If you turn on Infinite mode by pressing `m i`, you will
instead get the answer ‘`[0.1 .. inf]`’, which includes infinity
as a possible value.

The fourth calculation, ‘`1 / (-10 .. 10)`’, has the same problem.
Zero is buried inside the interval, but it’s still a possible value.
It’s not hard to see that the actual result of ‘`1 / (-10 .. 10)`’
will be either greater than *0.1*, or less than *-0.1*. Thus
the interval goes from minus infinity to plus infinity, with a “hole”
in it from *-0.1* to *0.1*. Calc doesn’t have any way to
represent this, so it just reports ‘`[-inf .. inf]`’ as the answer.
It may be disappointing to hear “the answer lies somewhere between
minus infinity and plus infinity, inclusive,” but that’s the best
that interval arithmetic can do in this case.

---

Next: [Types Tutorial Exercise 10](#Types-Answer-10), Previous: [Types Tutorial Exercise 8](#Types-Answer-8), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.41 Types Tutorial Exercise 9 [¶](#Types-Tutorial-Exercise-9)

```
1:  [-3 .. 3]       2:  [-3 .. 3]     2:  [0 .. 9]
    .               1:  [0 .. 9]      1:  [-9 .. 9]
                        .                 .

    [ 3 n .. 3 ]        RET 2 ^           TAB RET *
```

In the first case the result says, “if a number is between *-3* and
3, its square is between 0 and 9.” The second case says, “the product
of two numbers each between *-3* and 3 is between *-9* and 9.”

An interval form is not a number; it is a symbol that can stand for
many different numbers. Two identical-looking interval forms can stand
for different numbers.

The same issue arises when you try to square an error form.

---

Next: [Types Tutorial Exercise 11](#Types-Answer-11), Previous: [Types Tutorial Exercise 9](#Types-Answer-9), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.42 Types Tutorial Exercise 10 [¶](#Types-Tutorial-Exercise-10)

Testing the first number, we might arbitrarily choose 17 for ‘`x`’.

```
1:  17 mod 811749613   2:  17 mod 811749613   1:  533694123 mod 811749613
    .                      811749612              .
                           .

    17 M 811749613 RET     811749612              ^
```

Since 533694123 is (considerably) different from 1, the number 811749613
must not be prime.

It’s awkward to type the number in twice as we did above. There are
various ways to avoid this, and algebraic entry is one. In fact, using
a vector mapping operation we can perform several tests at once. Let’s
use this method to test the second number.

```
2:  [17, 42, 100000]               1:  [1 mod 15485863, 1 mod ... ]
1:  15485863                           .
    .

 [17 42 100000] 15485863 RET           V M ' ($$ mod $)^($-1) RET
```

The result is three ones (modulo ‘`n`’), so it’s very probable that
15485863 is prime. (In fact, this number is the millionth prime.)

Note that the functions ‘`($$^($-1)) mod $`’ or ‘`$$^($-1) % $`’
would have been hopelessly inefficient, since they would have calculated
the power using full integer arithmetic.

Calc has a `k p` command that does primality testing. For small
numbers it does an exact test; for large numbers it uses a variant
of the Fermat test we used here. You can use `k p` repeatedly
to prove that a large integer is prime with any desired probability.

---

Next: [Types Tutorial Exercise 12](#Types-Answer-12), Previous: [Types Tutorial Exercise 10](#Types-Answer-10), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.43 Types Tutorial Exercise 11 [¶](#Types-Tutorial-Exercise-11)

There are several ways to insert a calculated number into an HMS form.
One way to convert a number of seconds to an HMS form is simply to
multiply the number by an HMS form representing one second:

```
1:  31415926.5359     2:  31415926.5359     1:  8726@ 38' 46.5359"
    .                 1:  0@ 0' 1"              .
                          .

    P 1e7 *               0@ 0' 1"              *
```

```
2:  8726@ 38' 46.5359"             1:  6@ 6' 2.5359" mod 24@ 0' 0"
1:  15@ 27' 16" mod 24@ 0' 0"          .
    .

    x time RET                         +
```

It will be just after six in the morning.

The algebraic `hms` function can also be used to build an
HMS form:

```
1:  hms(0, 0, 10000000. pi)       1:  8726@ 38' 46.5359"
    .                                 .

  ' hms(0, 0, 1e7 pi) RET             =
```

The `=` key is necessary to evaluate the symbol ‘`pi`’ to
the actual number 3.14159...

---

Next: [Types Tutorial Exercise 13](#Types-Answer-13), Previous: [Types Tutorial Exercise 11](#Types-Answer-11), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.44 Types Tutorial Exercise 12 [¶](#Types-Tutorial-Exercise-12)

As we recall, there are 17 songs of about 2 minutes and 47 seconds
each.

```
2:  0@ 2' 47"                    1:  [0@ 3' 7" .. 0@ 3' 47"]
1:  [0@ 0' 20" .. 0@ 1' 0"]          .
    .

    [ 0@ 20" .. 0@ 1' ]              +
```

```
1:  [0@ 52' 59." .. 1@ 4' 19."]
    .

    17 *
```

No matter how long it is, the album will fit nicely on one CD.

---

Next: [Types Tutorial Exercise 14](#Types-Answer-14), Previous: [Types Tutorial Exercise 12](#Types-Answer-12), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.45 Types Tutorial Exercise 13 [¶](#Types-Tutorial-Exercise-13)

Type `' 1 yr RET u c s RET`. The answer is 31557600 seconds.

---

Next: [Types Tutorial Exercise 15](#Types-Answer-15), Previous: [Types Tutorial Exercise 13](#Types-Answer-13), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.46 Types Tutorial Exercise 14 [¶](#Types-Tutorial-Exercise-14)

How long will it take for a signal to get from one end of the computer
to the other?

```
1:  m / c         1:  3.3356 ns
    .                 .

 ' 1 m / c RET        u c ns RET
```

(Recall, ‘`c`’ is a “unit” corresponding to the speed of light.)

```
1:  3.3356 ns     1:  0.81356
2:  4.1 ns            .
    .

  ' 4.1 ns RET        /
```

Thus a signal could take up to 81 percent of a clock cycle just to
go from one place to another inside the computer, assuming the signal
could actually attain the full speed of light. Pretty tight!

---

Next: [Algebra Tutorial Exercise 1](#Algebra-Answer-1), Previous: [Types Tutorial Exercise 14](#Types-Answer-14), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.47 Types Tutorial Exercise 15 [¶](#Types-Tutorial-Exercise-15)

The speed limit is 55 miles per hour on most highways. We want to
find the ratio of Sam’s speed to the US speed limit.

```
1:  55 mph         2:  55 mph           3:  11 hr mph / yd
    .              1:  5 yd / hr            .
                       .

  ' 55 mph RET       ' 5 yd/hr RET          /
```

The `u s` command cancels out these units to get a plain
number. Now we take the logarithm base two to find the final
answer, assuming that each successive pill doubles his speed.

```
1:  19360.       2:  19360.       1:  14.24
    .            1:  2                .
                     .

    u s              2                B
```

Thus Sam can take up to 14 pills without a worry.

---

Next: [Algebra Tutorial Exercise 2](#Algebra-Answer-2), Previous: [Types Tutorial Exercise 15](#Types-Answer-15), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.48 Algebra Tutorial Exercise 1 [¶](#Algebra-Tutorial-Exercise-1)

The result ‘`sqrt(x)^2`’ is simplified back to ‘`x`’ by the
Calculator, but ‘`sqrt(x^2)`’ is not. (Consider what happens
if ‘`x = -4`’.) If ‘`x`’ is real, this formula could be
simplified to ‘`abs(x)`’, but for general complex arguments even
that is not safe. (See [Declarations](#Declarations), for a way to tell Calc
that ‘`x`’ is known to be real.)

---

Next: [Algebra Tutorial Exercise 3](#Algebra-Answer-3), Previous: [Algebra Tutorial Exercise 1](#Algebra-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.49 Algebra Tutorial Exercise 2 [¶](#Algebra-Tutorial-Exercise-2)

Suppose our roots are ‘`[a, b, c]`’. We want a polynomial which
is zero when ‘`x`’ is any of these values. The trivial polynomial
‘`x-a`’ is zero when ‘`x=a`’, so the product ‘`(x-a)(x-b)(x-c)`’
will do the job. We can use `a c x` to write this in a more
familiar form.

```
1:  34 x - 24 x^3          1:  [1.19023, -1.19023, 0]
    .                          .

    r 2                        a P x RET
```

```
1:  [x - 1.19023, x + 1.19023, x]     1:  x*(x + 1.19023) (x - 1.19023)
    .                                     .

    V M ' x-$ RET                         V R *
```

```
1:  x^3 - 1.41666 x        1:  34 x - 24 x^3
    .                          .

    a c x RET                  24 n *  a x
```

Sure enough, our answer (multiplied by a suitable constant) is the
same as the original polynomial.

---

Next: [Algebra Tutorial Exercise 4](#Algebra-Answer-4), Previous: [Algebra Tutorial Exercise 2](#Algebra-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.50 Algebra Tutorial Exercise 3 [¶](#Algebra-Tutorial-Exercise-3)

```
1:  x sin(pi x)         1:  sin(pi x) / pi^2 - x cos(pi x) / pi
    .                       .

  ' x sin(pi x) RET   m r   a i x RET
```

```
1:  [y, 1]
2:  sin(pi x) / pi^2 - x cos(pi x) / pi
    .

  ' [y,1] RET TAB
```

```
1:  [sin(pi y) / pi^2 - y cos(pi y) / pi, 1 / pi]
    .

    V M $ RET
```

```
1:  sin(pi y) / pi^2 - y cos(pi y) / pi - 1 / pi
    .

    V R -
```

```
1:  sin(3.14159 y) / 9.8696 - y cos(3.14159 y) / 3.14159 - 0.3183
    .

    =
```

```
1:  [0., -0.95493, 0.63662, -1.5915, 1.2732]
    .

    v x 5 RET  TAB  V M $ RET
```

---

Next: [Rewrites Tutorial Exercise 1](#Rewrites-Answer-1), Previous: [Algebra Tutorial Exercise 3](#Algebra-Answer-3), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.51 Algebra Tutorial Exercise 4 [¶](#Algebra-Tutorial-Exercise-4)

The hard part is that `V R +` is no longer sufficient to add up all
the contributions from the slices, since the slices have varying
coefficients. So first we must come up with a vector of these
coefficients. Here’s one way:

```
2:  -1                 2:  3                    1:  [4, 2, ..., 4]
1:  [1, 2, ..., 9]     1:  [-1, 1, ..., -1]         .
    .                      .

    1 n v x 9 RET          V M ^  3 TAB             -
```

```
1:  [4, 2, ..., 4, 1]      1:  [1, 4, 2, ..., 4, 1]
    .                          .

    1 |                        1 TAB |
```

Now we compute the function values. Note that for this method we need
eleven values, including both endpoints of the desired interval.

```
2:  [1, 4, 2, ..., 4, 1]
1:  [1, 1.1, 1.2,  ...  , 1.8, 1.9, 2.]
    .

 11 RET 1 RET .1 RET  C-u v x
```

```
2:  [1, 4, 2, ..., 4, 1]
1:  [0., 0.084941, 0.16993, ... ]
    .

    ' sin(x) ln(x) RET   m r  p 5 RET   V M $ RET
```

Once again this calls for `V M * V R +`; a simple `*` does the
same thing.

```
1:  11.22      1:  1.122      1:  0.374
    .              .              .

    *              .1 *           3 /
```

Wow! That’s even better than the result from the Taylor series method.

---

Next: [Rewrites Tutorial Exercise 2](#Rewrites-Answer-2), Previous: [Algebra Tutorial Exercise 4](#Algebra-Answer-4), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.52 Rewrites Tutorial Exercise 1 [¶](#Rewrites-Tutorial-Exercise-1)

We’ll use Big mode to make the formulas more readable.

```
                                           ___
                                          V 2  + 2
1:  (2 + sqrt(2)) / (1 + sqrt(2))     1:  ---------
    .                                      ___
                                          V 2  + 1

                                          .

  ' (2+sqrt(2)) / (1+sqrt(2)) RET         d B
```

Multiplying by the conjugate helps because ‘`(a+b) (a-b) = a^2 - b^2`’.

```
          ___    ___
1:  (2 + V 2 ) (V 2  - 1)
    .

  a r a/(b+c) := a*(b-c) / (b^2-c^2) RET
```

```
     ___
1:  V 2
    .

  a r a*(b+c) := a*b + a*c
```

(We could have used `a x` instead of a rewrite rule for the
second step.)

The multiply-by-conjugate rule turns out to be useful in many
different circumstances, such as when the denominator involves
sines and cosines or the imaginary constant `i`.

---

Next: [Rewrites Tutorial Exercise 3](#Rewrites-Answer-3), Previous: [Rewrites Tutorial Exercise 1](#Rewrites-Answer-1), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.53 Rewrites Tutorial Exercise 2 [¶](#Rewrites-Tutorial-Exercise-2)

Here is the rule set:

```
[ fib(n) := fib(n, 1, 1) :: integer(n) :: n >= 1,
  fib(1, x, y) := x,
  fib(n, x, y) := fib(n-1, y, x+y) ]
```

The first rule turns a one-argument `fib` that people like to write
into a three-argument `fib` that makes computation easier. The
second rule converts back from three-argument form once the computation
is done. The third rule does the computation itself. It basically
says that if ‘`x`’ and ‘`y`’ are two consecutive Fibonacci numbers,
then ‘`y`’ and ‘`x+y`’ are the next (overlapping) pair of Fibonacci
numbers.

Notice that because the number ‘`n`’ was “validated” by the
conditions on the first rule, there is no need to put conditions on
the other rules because the rule set would never get that far unless
the input were valid. That further speeds computation, since no
extra conditions need to be checked at every step.

Actually, a user with a nasty sense of humor could enter a bad
three-argument `fib` call directly, say, ‘`fib(0, 1, 1)`’,
which would get the rules into an infinite loop. One thing that would
help keep this from happening by accident would be to use something like
‘`ZzFib`’ instead of `fib` as the name of the three-argument
function.

---

Next: [Rewrites Tutorial Exercise 4](#Rewrites-Answer-4), Previous: [Rewrites Tutorial Exercise 2](#Rewrites-Answer-2), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.54 Rewrites Tutorial Exercise 3 [¶](#Rewrites-Tutorial-Exercise-3)

He got an infinite loop. First, Calc did as expected and rewrote
‘`2 + 3 x`’ to ‘`f(2, 3, x)`’. Then it looked for ways to
apply the rule again, and found that ‘`f(2, 3, x)`’ looks like
‘`a + b x`’ with ‘`a = 0`’ and ‘`b = 1`’, so it rewrote to
‘`f(0, 1, f(2, 3, x))`’. It then wrapped another ‘`f(0, 1, ...)`’
around that, and so on, ad infinitum. Joe should have used `M-1 a r`
to make sure the rule applied only once.

(Actually, even the first step didn’t work as he expected. What Calc
really gives for `M-1 a r` in this situation is ‘`f(3 x, 1, 2)`’,
treating 2 as the “variable,” and ‘`3 x`’ as a constant being added
to it. While this may seem odd, it’s just as valid a solution as the
“obvious” one. One way to fix this would be to add the condition
‘`:: variable(x)`’ to the rule, to make sure the thing that matches
‘`x`’ is indeed a variable, or to change ‘`x`’ to ‘`quote(x)`’
on the lefthand side, so that the rule matches the actual variable
‘`x`’ rather than letting ‘`x`’ stand for something else.)

---

Next: [Rewrites Tutorial Exercise 5](#Rewrites-Answer-5), Previous: [Rewrites Tutorial Exercise 3](#Rewrites-Answer-3), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.55 Rewrites Tutorial Exercise 4 [¶](#Rewrites-Tutorial-Exercise-4)

Here is a suitable set of rules to solve the first part of the problem:

```
[ seq(n, c) := seq(n/2,  c+1) :: n%2 = 0,
  seq(n, c) := seq(3n+1, c+1) :: n%2 = 1 :: n > 1 ]
```

Given the initial formula ‘`seq(6, 0)`’, application of these
rules produces the following sequence of formulas:

```
seq( 3, 1)
seq(10, 2)
seq( 5, 3)
seq(16, 4)
seq( 8, 5)
seq( 4, 6)
seq( 2, 7)
seq( 1, 8)
```

whereupon neither of the rules match, and rewriting stops.

We can pretty this up a bit with a couple more rules:

```
[ seq(n) := seq(n, 0),
  seq(1, c) := c,
  ... ]
```

Now, given ‘`seq(6)`’ as the starting configuration, we get 8
as the result.

The change to return a vector is quite simple:

```
[ seq(n) := seq(n, []) :: integer(n) :: n > 0,
  seq(1, v) := v | 1,
  seq(n, v) := seq(n/2,  v | n) :: n%2 = 0,
  seq(n, v) := seq(3n+1, v | n) :: n%2 = 1 ]
```

Given ‘`seq(6)`’, the result is ‘`[6, 3, 10, 5, 16, 8, 4, 2, 1]`’.

Notice that the ‘`n > 1`’ guard is no longer necessary on the last
rule since the ‘`n = 1`’ case is now detected by another rule.
But a guard has been added to the initial rule to make sure the
initial value is suitable before the computation begins.

While still a good idea, this guard is not as vitally important as it
was for the `fib` function, since calling, say, ‘`seq(x, [])`’
will not get into an infinite loop. Calc will not be able to prove
the symbol ‘`x`’ is either even or odd, so none of the rules will
apply and the rewrites will stop right away.

---

Next: [Rewrites Tutorial Exercise 6](#Rewrites-Answer-6), Previous: [Rewrites Tutorial Exercise 4](#Rewrites-Answer-4), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.56 Rewrites Tutorial Exercise 5 [¶](#Rewrites-Tutorial-Exercise-5)

If ‘`x`’ is the sum ‘`a + b`’, then ‘`nterms(`x`)`’ must
be ‘`nterms(`a`)`’ plus ‘`nterms(`b`)`’. If ‘`x`’
is not a sum, then ‘`nterms(`x`)`’ = 1.

```
[ nterms(a + b) := nterms(a) + nterms(b),
  nterms(x)     := 1 ]
```

Here we have taken advantage of the fact that earlier rules always
match before later rules; ‘`nterms(x)`’ will only be tried if we
already know that ‘`x`’ is not a sum.

---

Next: [Programming Tutorial Exercise 1](#Programming-Answer-1), Previous: [Rewrites Tutorial Exercise 5](#Rewrites-Answer-5), Up: [Answers to Exercises](#Answers-to-Exercises)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 2.7.57 Rewrites Tutorial Exercise 6 [¶](#Rewrites-Tutorial-Exercise-6)

Here is a rule set that will do the job:

```
[ a*(b + c) := a*b + a*c,
  opt(a) O(x^n) + opt(b) O(x^m) := O(x^n) :: n <= m
     :: constant(a) :: constant(b),
  opt(a) O(x^n) + opt(b) x^m := O(x^n) :: n <= m
     :: constant(a) :: constant(b),
  a O(x^n) := O(x^n) :: constant(a),
  x^opt(m) O(x^n) := O(x^(n+m)),
  O(x^n) O(x^m) := O(x^(n+m)) ]
```
