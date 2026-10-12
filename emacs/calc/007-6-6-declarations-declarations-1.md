### 6.6 Declarations [¶](#Declarations-1)

A *declaration* is a statement you make that promises you will
use a certain variable or function in a restricted way. This may
give Calc the freedom to do things that it couldn’t do if it had to
take the fully general situation into account.

* [Declaration Basics](#Declaration-Basics)
* [Kinds of Declarations](#Kinds-of-Declarations)
* [Functions for Declarations](#Functions-for-Declarations)

---

Next: [Kinds of Declarations](#Kinds-of-Declarations), Up: [Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.6.1 Declaration Basics [¶](#Declaration-Basics-1)

The `s d` (`calc-declare-variable`) command is the easiest
way to make a declaration for a variable. This command prompts for
the variable name, then prompts for the declaration. The default
at the declaration prompt is the previous declaration, if any.
You can edit this declaration, or press `C-k` to erase it and
type a new declaration. (Or, erase it and press `RET` to clear
the declaration, effectively “undeclaring” the variable.)

A declaration is in general a vector of *type symbols* and
*range* values. If there is only one type symbol or range value,
you can write it directly rather than enclosing it in a vector.
For example, `s d foo RET real RET` declares `foo` to
be a real number, and `s d bar RET [int, const, [1..6]] RET`
declares `bar` to be a constant integer between 1 and 6.
(Actually, you can omit the outermost brackets and Calc will
provide them for you: `s d bar RET int, const, [1..6] RET`.)

Declarations in Calc are kept in a special variable called `Decls`.
This variable encodes the set of all outstanding declarations in
the form of a matrix. Each row has two elements: A variable or
vector of variables declared by that row, and the declaration
specifier as described above. You can use the `s D` command to
edit this variable if you wish to see all the declarations at once.
See [Other Operations on Variables](#Operations-on-Variables), for a description of this command
and the `s p` command that allows you to save your declarations
permanently if you wish.

Items being declared can also be function calls. The arguments in
the call are ignored; the effect is to say that this function returns
values of the declared type for any valid arguments. The `s d`
command declares only variables, so if you wish to make a function
declaration you will have to edit the `Decls` matrix yourself.

For example, the declaration matrix

```
[ [ foo,       real       ]
  [ [j, k, n], int        ]
  [ f(1,2,3),  [0 .. inf) ] ]
```

declares that `foo` represents a real number, `j`, `k`
and `n` represent integers, and the function `f` always
returns a real number in the interval shown.

If there is a declaration for the variable `All`, then that
declaration applies to all variables that are not otherwise declared.
It does not apply to function names. For example, using the row
‘`[All, real]`’ says that all your variables are real unless they
are explicitly declared without `real` in some other row.
The `s d` command declares `All` if you give a blank
response to the variable-name prompt.

---

Next: [Functions for Declarations](#Functions-for-Declarations), Previous: [Declaration Basics](#Declaration-Basics), Up: [Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.6.2 Kinds of Declarations [¶](#Kinds-of-Declarations-1)

The type-specifier part of a declaration (that is, the second prompt
in the `s d` command) can be a type symbol, an interval, or a
vector consisting of zero or more type symbols followed by zero or
more intervals or numbers that represent the set of possible values
for the variable.

```
[ [ a, [1, 2, 3, 4, 5] ]
  [ b, [1 .. 5]        ]
  [ c, [int, 1 .. 5]   ] ]
```

Here `a` is declared to contain one of the five integers shown;
`b` is any number in the interval from 1 to 5 (any real number
since we haven’t specified), and `c` is any integer in that
interval. Thus the declarations for `a` and `c` are
nearly equivalent (see below).

The type-specifier can be the empty vector ‘`[]`’ to say that
nothing is known about a given variable’s value. This is the same
as not declaring the variable at all except that it overrides any
`All` declaration which would otherwise apply.

The initial value of `Decls` is the empty vector ‘`[]`’.
If `Decls` has no stored value or if the value stored in it
is not valid, it is ignored and there are no declarations as far
as Calc is concerned. (The `s d` command will replace such a
malformed value with a fresh empty matrix, ‘`[]`’, before recording
the new declaration.) Unrecognized type symbols are ignored.

The following type symbols describe what sorts of numbers will be
stored in a variable:

`int`
:   Integers.

`numint`
:   Numerical integers. (Integers or integer-valued floats.)

`frac`
:   Fractions. (Rational numbers which are not integers.)

`rat`
:   Rational numbers. (Either integers or fractions.)

`float`
:   Floating-point numbers.

`real`
:   Real numbers. (Integers, fractions, or floats. Actually,
    intervals and error forms with real components also count as
    reals here.)

`pos`
:   Positive real numbers. (Strictly greater than zero.)

`nonneg`
:   Nonnegative real numbers. (Greater than or equal to zero.)

`number`
:   Numbers. (Real or complex.)

Calc uses this information to determine when certain simplifications
of formulas are safe. For example, ‘`(x^y)^z`’ cannot be
simplified to ‘`x^(y z)`’ in general; for example,
‘`((-3)^2)^1:2`’ is 3, but ‘`(-3)^(2*1:2) = (-3)^1`’ is *-3*.
However, this simplification *is* safe if `z` is known
to be an integer, or if `x` is known to be a nonnegative
real number. If you have given declarations that allow Calc to
deduce either of these facts, Calc will perform this simplification
of the formula.

Calc can apply a certain amount of logic when using declarations.
For example, ‘`(x^y)^(2n+1)`’ will be simplified if `n`
has been declared `int`; Calc knows that an integer times an
integer, plus an integer, must always be an integer. (In fact,
Calc would simplify ‘`(-x)^(2n+1)`’ to ‘`-(x^(2n+1))`’ since
it is able to determine that ‘`2n+1`’ must be an odd integer.)

Similarly, ‘`(abs(x)^y)^z`’ will be simplified to ‘`abs(x)^(y z)`’
because Calc knows that the `abs` function always returns a
nonnegative real. If you had a `myabs` function that also had
this property, you could get Calc to recognize it by adding the row
‘`[myabs(), nonneg]`’ to the `Decls` matrix.

One instance of this simplification is ‘`sqrt(x^2)`’ (since the
`sqrt` function is effectively a one-half power). Normally
Calc leaves this formula alone. After the command
`s d x RET real RET`, however, it can simplify the formula to
‘`abs(x)`’. And after `s d x RET nonneg RET`, Calc can
simplify this formula all the way to ‘`x`’.

If there are any intervals or real numbers in the type specifier,
they comprise the set of possible values that the variable or
function being declared can have. In particular, the type symbol
`real` is effectively the same as the range ‘`[-inf .. inf]`’
(note that infinity is included in the range of possible values);
`pos` is the same as ‘`(0 .. inf]`’, and `nonneg` is
the same as ‘`[0 .. inf]`’. Saying ‘`[real, [-5 .. 5]]`’ is
redundant because the fact that the variable is real can be
deduced just from the interval, but ‘`[int, [-5 .. 5]]`’ and
‘`[rat, [-5 .. 5]]`’ are useful combinations.

Note that the vector of intervals or numbers is in the same format
used by Calc’s set-manipulation commands. See [Set Operations using Vectors](#Set-Operations).

The type specifier ‘`[1, 2, 3]`’ is equivalent to
‘`[numint, 1, 2, 3]`’, *not* to ‘`[int, 1, 2, 3]`’.
In other words, the range of possible values means only that
the variable’s value must be numerically equal to a number in
that range, but not that it must be equal in type as well.
Calc’s set operations act the same way; ‘`in(2, [1., 2., 3.])`’
and ‘`in(1.5, [1:2, 3:2, 5:2])`’ both report “true.”

If you use a conflicting combination of type specifiers, the
results are unpredictable. An example is ‘`[pos, [0 .. 5]]`’,
where the interval does not lie in the range described by the
type symbol.

“Real” declarations mostly affect simplifications involving powers
like the one described above. Another case where they are used
is in the `a P` command which returns a list of all roots of a
polynomial; if the variable has been declared real, only the real
roots (if any) will be included in the list.

“Integer” declarations are used for simplifications which are valid
only when certain values are integers (such as ‘`(x^y)^z`’
shown above).

Calc’s algebraic simplifications also make use of declarations when
simplifying equations and inequalities. They will cancel `x`
from both sides of ‘`a x = b x`’ only if it is sure `x`
is non-zero, say, because it has a `pos` declaration.
To declare specifically that `x` is real and non-zero,
use ‘`[[-inf .. 0), (0 .. inf]]`’. (There is no way in the
current notation to say that `x` is nonzero but not necessarily
real.) The `a e` command does “unsafe” simplifications,
including canceling ‘`x`’ from the equation when ‘`x`’ is
not known to be nonzero.

Another set of type symbols distinguish between scalars and vectors.

`scalar`
:   The value is not a vector.

`vector`
:   The value is a vector.

`matrix`
:   The value is a matrix (a rectangular vector of vectors).

`sqmatrix`
:   The value is a square matrix.

These type symbols can be combined with the other type symbols
described above; ‘`[int, matrix]`’ describes an object which
is a matrix of integers.

Scalar/vector declarations are used to determine whether certain
algebraic operations are safe. For example, ‘`[a, b, c] + x`’
is normally not simplified to ‘`[a + x, b + x, c + x]`’, but
it will be if `x` has been declared `scalar`. On the
other hand, multiplication is usually assumed to be commutative,
but the terms in ‘`x y`’ will never be exchanged if both `x`
and `y` are known to be vectors or matrices. (Calc currently
never distinguishes between `vector` and `matrix`
declarations.)

See [Matrix and Scalar Modes](#Matrix-Mode), for a discussion of Matrix mode and
Scalar mode, which are similar to declaring ‘`[All, matrix]`’
or ‘`[All, scalar]`’ but much more convenient.

One more type symbol that is recognized is used with the `H a d`
command for taking total derivatives of a formula. See [Calculus](#Calculus).

`const`
:   The value is a constant with respect to other variables.

Calc does not check the declarations for a variable when you store
a value in it. However, storing *-3.5* in a variable that has
been declared `pos`, `int`, or `matrix` may have
unexpected effects; Calc may evaluate ‘`sqrt(x^2)`’ to ‘`3.5`’
if it substitutes the value first, or to ‘`-3.5`’ if `x`
was declared `pos` and the formula ‘`sqrt(x^2)`’ is
simplified to ‘`x`’ before the value is substituted. Before
using a variable for a new purpose, it is best to use `s d`
or `s D` to check to make sure you don’t still have an old
declaration for the variable that will conflict with its new meaning.

---

Previous: [Kinds of Declarations](#Kinds-of-Declarations), Up: [Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.6.3 Functions for Declarations [¶](#Functions-for-Declarations-1)

Calc has a set of functions for accessing the current declarations
in a convenient manner. These functions return 1 if the argument
can be shown to have the specified property, or 0 if the argument
can be shown *not* to have that property; otherwise they are
left unevaluated. These functions are suitable for use with rewrite
rules (see [Conditional Rewrite Rules](#Conditional-Rewrite-Rules)) or programming constructs
(see [Conditionals in Keyboard Macros](#Conditionals-in-Macros)). They can be entered only using
algebraic notation. See [Logical Operations](#Logical-Operations), for functions
that perform other tests not related to declarations.

For example, ‘`dint(17)`’ returns 1 because 17 is an integer, as
do ‘`dint(n)`’ and ‘`dint(2 n - 3)`’ if `n` has been declared
`int`, but ‘`dint(2.5)`’ and ‘`dint(n + 0.5)`’ return 0.
Calc consults knowledge of its own built-in functions as well as your
own declarations: ‘`dint(floor(x))`’ returns 1.

The `dint` function checks if its argument is an integer.
The `dnatnum` function checks if its argument is a natural
number, i.e., a nonnegative integer. The `dnumint` function
checks if its argument is numerically an integer, i.e., either an
integer or an integer-valued float. Note that these and the other
data type functions also accept vectors or matrices composed of
suitable elements, and that real infinities ‘`inf`’ and ‘`-inf`’
are considered to be integers for the purposes of these functions.

The `drat` function checks if its argument is rational, i.e.,
an integer or fraction. Infinities count as rational, but intervals
and error forms do not.

The `dreal` function checks if its argument is real. This
includes integers, fractions, floats, real error forms, and intervals.

The `dimag` function checks if its argument is imaginary,
i.e., is mathematically equal to a real number times ‘`i`’.

The `dpos` function checks for positive (but nonzero) reals.
The `dneg` function checks for negative reals. The `dnonneg`
function checks for nonnegative reals, i.e., reals greater than or
equal to zero. Note that Calc’s algebraic simplifications, which are
effectively applied to all conditions in rewrite rules, can simplify
an expression like ‘`x > 0`’ to 1 or 0 using `dpos`.
So the actual functions `dpos`, `dneg`, and `dnonneg`
are rarely necessary.

The `dnonzero` function checks that its argument is nonzero.
This includes all nonzero real or complex numbers, all intervals that
do not include zero, all nonzero modulo forms, vectors all of whose
elements are nonzero, and variables or formulas whose values can be
deduced to be nonzero. It does not include error forms, since they
represent values which could be anything including zero. (This is
also the set of objects considered “true” in conditional contexts.)

The `deven` function returns 1 if its argument is known to be
an even integer (or integer-valued float); it returns 0 if its argument
is known not to be even (because it is known to be odd or a non-integer).
Calc’s algebraic simplifications use this to simplify a test of the form
‘`x % 2 = 0`’. There is also an analogous `dodd` function.

The `drange` function returns a set (an interval or a vector
of intervals and/or numbers; see [Set Operations using Vectors](#Set-Operations)) that describes
the set of possible values of its argument. If the argument is
a variable or a function with a declaration, the range is copied
from the declaration. Otherwise, the possible signs of the
expression are determined using a method similar to `dpos`,
etc., and a suitable set like ‘`[0 .. inf]`’ is returned. If
the expression is not provably real, the `drange` function
remains unevaluated.

The `dscalar` function returns 1 if its argument is provably
scalar, or 0 if its argument is provably non-scalar. It is left
unevaluated if this cannot be determined. (If Matrix mode or Scalar
mode is in effect, this function returns 1 or 0, respectively,
if it has no other information.) When Calc interprets a condition
(say, in a rewrite rule) it considers an unevaluated formula to be
“false.” Thus, ‘`dscalar(a)`’ is “true” only if `a` is
provably scalar, and ‘`!dscalar(a)`’ is “true” only if `a`
is provably non-scalar; both are “false” if there is insufficient
information to tell.

---

Next: [Language Modes](#Language-Modes), Previous: [Declarations](#Declarations), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 6.7 Display Modes [¶](#Display-Modes-1)

The commands in this section are two-key sequences beginning with the
`d` prefix. The `d l` (`calc-line-numbering`) and `d b`
(`calc-line-breaking`) commands are described elsewhere;
see [Stack Basics](#Stack-Basics) and see [Normal Language Modes](#Normal-Language-Modes), respectively.
Display formats for vectors and matrices are also covered elsewhere;
see [Vector and Matrix Display Formats](#Vector-and-Matrix-Formats).

One thing all display modes have in common is their treatment of the
`H` prefix. This prefix causes any mode command that would normally
refresh the stack to leave the stack display alone. The word “Dirty”
will appear in the mode line when Calc thinks the stack display may not
reflect the latest mode settings.

The `d RET` (`calc-refresh-top`) command reformats the
top stack entry according to all the current modes. Positive prefix
arguments reformat the top n entries; negative prefix arguments
reformat the specified entry, and a prefix of zero is equivalent to
`d SPC` (`calc-refresh`), which reformats the entire stack.
For example, `H d s M-2 d RET` changes to scientific notation
but reformats only the top two stack entries in the new mode.

The `I` prefix has another effect on the display modes. The mode
is set only temporarily; the top stack entry is reformatted according
to that mode, then the original mode setting is restored. In other
words, `I d s` is equivalent to `H d s d RET H d (old mode)`.

* [Radix Modes](#Radix-Modes)
* [Grouping Digits](#Grouping-Digits)
* [Float Formats](#Float-Formats)
* [Complex Formats](#Complex-Formats)
* [Fraction Formats](#Fraction-Formats)
* [HMS Formats](#HMS-Formats)
* [Date Formats](#Date-Formats)
* [Truncating the Stack](#Truncating-the-Stack)
* [Justification](#Justification)
* [Labels](#Labels)

---

Next: [Grouping Digits](#Grouping-Digits), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.1 Radix Modes [¶](#Radix-Modes-1)

Calc normally displays numbers in decimal (*base-10* or *radix-10*)
notation. Calc can actually display in any radix from two (binary) to 36.
When the radix is above 10, the letters `A` to `Z` are used as
digits. When entering such a number, letter keys are interpreted as
potential digits rather than terminating numeric entry mode.

The key sequences `d 2`, `d 8`, `d 6`, and `d 0` select
binary, octal, hexadecimal, and decimal as the current display radix,
respectively. Numbers can always be entered in any radix, though the
current radix is used as a default if you press `#` without any initial
digits. A number entered without a `#` is *always* interpreted
as decimal.

To set the radix generally, use `d r` (`calc-radix`) and enter
an integer from 2 to 36. You can specify the radix as a numeric prefix
argument; otherwise you will be prompted for it.

Integers normally are displayed with however many digits are necessary to
represent the integer and no more. The `d z` (`calc-leading-zeros`)
command causes integers to be padded out with leading zeros according to the
current binary word size. (See [Binary Number Functions](#Binary-Functions), for a discussion of
word size.) If the absolute value of the word size is ‘`w`’, all integers
are displayed with at least enough digits to represent
‘`(2^w)-1`’
in the current radix. (Larger integers will still be displayed in their
entirety.)

Calc can display ‘`w`’-bit integers using two’s complement
notation, although this is most useful with the binary, octal and
hexadecimal display modes. This option is selected by using the
`O` option prefix before setting the display radix, and a negative word
size might be appropriate (see [Binary Number Functions](#Binary-Functions)). In two’s
complement notation, the integers in the (nearly) symmetric interval
from
‘`-2^(w-1)`’
to
‘`2^(w-1)-1`’
are represented by the integers from ‘`0`’ to ‘`2^w-1`’:
the integers from ‘`0`’ to
‘`2^(w-1)-1`’
are represented by themselves and the integers from
‘`-2^(w-1)`’
to ‘`-1`’ are represented by the integers from
‘`2^(w-1)`’
to ‘`2^w-1`’ (the integer ‘`k`’ is represented by ‘`k+2^w`’).
Calc will display a two’s complement integer by the radix (either
‘`2`’, ‘`8`’ or ‘`16`’), two `#` symbols, and then its
representation (including any leading zeros necessary to include all
‘`w`’ bits). In a two’s complement display mode, numbers that
are not displayed in two’s complement notation (i.e., that aren’t
integers from
‘`-2^(w-1)`’
to
‘`2^(w-1)-1`’)
will be represented using Calc’s usual notation (in the appropriate
radix).

---

Next: [Float Formats](#Float-Formats), Previous: [Radix Modes](#Radix-Modes), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.2 Grouping Digits [¶](#Grouping-Digits-1)

Long numbers can be hard to read if they have too many digits. For
example, the factorial of 30 is 33 digits long! Press `d g`
(`calc-group-digits`) to enable *Grouping* mode, in which digits
are displayed in clumps of 3 or 4 (depending on the current radix)
separated by commas.

The `d g` command toggles grouping on and off.
With a numeric prefix of 0, this command displays the current state of
the grouping flag; with an argument of minus one it disables grouping;
with a positive argument ‘`N`’ it enables grouping on every ‘`N`’
digits. For floating-point numbers, grouping normally occurs only
before the decimal point. A negative prefix argument ‘`-N`’ enables
grouping every ‘`N`’ digits both before and after the decimal point.

The `d ,` (`calc-group-char`) command allows you to choose any
character as the grouping separator. The default is the comma character.
If you find it difficult to read vectors of large integers grouped with
commas, you may wish to use spaces or some other character instead.
This command takes the next character you type, whatever it is, and
uses it as the digit separator. As a special case, `d , \` selects
‘`\,`’ (TeX’s thin-space symbol) as the digit separator.

Please note that grouped numbers will not generally be parsed correctly
if re-read in textual form, say by the use of `C-x * y` and `C-x * g`.
(See [Kill and Yank Functions](#Kill-and-Yank), for details on these commands.) One exception is
the ‘`\,`’ separator, which doesn’t interfere with parsing because it
is ignored by TeX language mode.

---

Next: [Complex Formats](#Complex-Formats), Previous: [Grouping Digits](#Grouping-Digits), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.3 Float Formats [¶](#Float-Formats-1)

Floating-point quantities are normally displayed in standard decimal
form, with scientific notation used if the exponent is especially high
or low. All significant digits are normally displayed. The commands
in this section allow you to choose among several alternative display
formats for floats.

The `d n` (`calc-normal-notation`) command selects the normal
display format. All significant figures in a number are displayed.
With a positive numeric prefix, numbers are rounded if necessary to
that number of significant digits. With a negative numerix prefix,
the specified number of significant digits less than the current
precision is used. (Thus `C-u -2 d n` displays 10 digits if the
current precision is 12.)

The `d f` (`calc-fix-notation`) command selects fixed-point
notation. The numeric argument is the number of digits after the
decimal point, zero or more. This format will relax into scientific
notation if a nonzero number would otherwise have been rounded all the
way to zero. Specifying a negative number of digits is the same as
for a positive number, except that small nonzero numbers will be rounded
to zero rather than switching to scientific notation.

The `d s` (`calc-sci-notation`) command selects scientific
notation. A positive argument sets the number of significant figures
displayed, of which one will be before and the rest after the decimal
point. A negative argument works the same as for `d n` format.
The default is to display all significant digits.

The `d e` (`calc-eng-notation`) command selects engineering
notation. This is similar to scientific notation except that the
exponent is rounded down to a multiple of three, with from one to three
digits before the decimal point. An optional numeric prefix sets the
number of significant digits to display, as for `d s`.

It is important to distinguish between the current *precision* and
the current *display format*. After the commands `C-u 10 p`
and `C-u 6 d n` the Calculator computes all results to ten
significant figures but displays only six. (In fact, intermediate
calculations are often carried to one or two more significant figures,
but values placed on the stack will be rounded down to ten figures.)
Numbers are never actually rounded to the display precision for storage,
except by commands like `C-k` and `C-x * y` which operate on the
actual displayed text in the Calculator buffer.

The `d .` (`calc-point-char`) command selects the character used
as a decimal point. Normally this is a period; users in some countries
may wish to change this to a comma. Note that this is only a display
style; on entry, periods must always be used to denote floating-point
numbers, and commas to separate elements in a list.

---

Next: [Fraction Formats](#Fraction-Formats), Previous: [Float Formats](#Float-Formats), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.4 Complex Formats [¶](#Complex-Formats-1)

There are three supported notations for complex numbers in rectangular
form. The default is as a pair of real numbers enclosed in parentheses
and separated by a comma: ‘`(a,b)`’. The `d c`
(`calc-complex-notation`) command selects this style.

The other notations are `d i` (`calc-i-notation`), in which
numbers are displayed in ‘`a+bi`’ form, and `d j`
(`calc-j-notation`) which displays the form ‘`a+bj`’ preferred
in some disciplines.

Complex numbers are normally entered in ‘`(a,b)`’ format.
If you enter ‘`2+3i`’ as an algebraic formula, it will be stored as
the formula ‘`2 + 3 * i`’. However, if you use `=` to evaluate
this formula and you have not changed the variable ‘`i`’, the ‘`i`’
will be interpreted as ‘`(0,1)`’ and the formula will be simplified
to ‘`(2,3)`’. Other commands (like `calc-sin`) will *not*
interpret the formula ‘`2 + 3 * i`’ as a complex number.
See [Variables](#Variables), under “special constants.”

---

Next: [HMS Formats](#HMS-Formats), Previous: [Complex Formats](#Complex-Formats), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.5 Fraction Formats [¶](#Fraction-Formats-1)

Display of fractional numbers is controlled by the `d o`
(`calc-over-notation`) command. By default, a number like
eight thirds is displayed in the form ‘`8:3`’. The `d o` command
prompts for a one- or two-character format. If you give one character,
that character is used as the fraction separator. Common separators are
‘`:`’ and ‘`/`’. (During input of numbers, the `:` key must be
used regardless of the display format; in particular, the `/` is used
for RPN-style division, *not* for entering fractions.)

If you give two characters, fractions use “integer-plus-fractional-part”
notation. For example, the format ‘`+/`’ would display eight thirds
as ‘`2+2/3`’. If two colons are present in a number being entered,
the number is interpreted in this form (so that the entries `2:2:3`
and `8:3` are equivalent).

It is also possible to follow the one- or two-character format with
a number. For example: ‘`:10`’ or ‘`+/3`’. In this case,
Calc adjusts all fractions that are displayed to have the specified
denominator, if possible. Otherwise it adjusts the denominator to
be a multiple of the specified value. For example, in ‘`:6`’ mode
the fraction ‘`1:6`’ will be unaffected, but ‘`2:3`’ will be
displayed as ‘`4:6`’, ‘`1:2`’ will be displayed as ‘`3:6`’,
and ‘`1:8`’ will be displayed as ‘`3:24`’. Integers are also
affected by this mode: 3 is displayed as ‘`18:6`’. Note that the
format ‘`:1`’ writes fractions the same as ‘`:`’, but it writes
integers as ‘`n:1`’.

The fraction format does not affect the way fractions or integers are
stored, only the way they appear on the screen. The fraction format
never affects floats.

---

Next: [Date Formats](#Date-Formats), Previous: [Fraction Formats](#Fraction-Formats), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.6 HMS Formats [¶](#HMS-Formats-1)

The `d h` (`calc-hms-notation`) command controls the display of
HMS (hours-minutes-seconds) forms. It prompts for a string which
consists basically of an “hours” marker, optional punctuation, a
“minutes” marker, more optional punctuation, and a “seconds” marker.
Punctuation is zero or more spaces, commas, or semicolons. The hours
marker is one or more non-punctuation characters. The minutes and
seconds markers must be single non-punctuation characters.

The default HMS format is ‘`@ ' "`’, producing HMS values of the form
‘`23@ 30' 15.75"`’. The format ‘`deg, ms`’ would display this same
value as ‘`23deg, 30m15.75s`’. During numeric entry, the `h` or `o`
keys are recognized as synonyms for `@` regardless of display format.
The `m` and `s` keys are recognized as synonyms for `'` and
`"`, respectively, but only if an `@` (or `h` or `o`) has
already been typed; otherwise, they have their usual meanings
(`m-` prefix and `s-` prefix). Thus, `5 "`, `0 @ 5 "`, and
`0 h 5 s` are some of the ways to enter the quantity “five seconds.”
The `'` key is recognized as “minutes” only if `@` (or `h` or
`o`) has already been pressed; otherwise it means to switch to algebraic
entry.

---

Next: [Truncating the Stack](#Truncating-the-Stack), Previous: [HMS Formats](#HMS-Formats), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.7 Date Formats [¶](#Date-Formats-1)

The `d d` (`calc-date-notation`) command controls the display
of date forms (see [Date Forms](#Date-Forms)). It prompts for a string which
contains letters that represent the various parts of a date and time.
To show which parts should be omitted when the form represents a pure
date with no time, parts of the string can be enclosed in ‘`< >`’
marks. If you don’t include ‘`< >`’ markers in the format, Calc
guesses at which parts, if any, should be omitted when formatting
pure dates.

The default format is: ‘`<H:mm:SSpp >Www Mmm D, YYYY`’.
An example string in this format is ‘`3:32pm Wed Jan 9, 1991`’.
If you enter a blank format string, this default format is
reestablished.

Calc uses ‘`< >`’ notation for nameless functions as well as for
dates. See [Specifying Operators](#Specifying-Operators). To avoid confusion with nameless
functions, your date formats should avoid using the ‘`#`’ character.

* [ISO 8601](#ISO-8601)
* [Date Formatting Codes](#Date-Formatting-Codes)
* [Free-Form Dates](#Free_002dForm-Dates)
* [Standard Date Formats](#Standard-Date-Formats)

---

Next: [Date Formatting Codes](#Date-Formatting-Codes), Up: [Date Formats](#Date-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.7.1 ISO 8601 [¶](#ISO-8601-1)

The same date can be written down in different formats and Calc tries
to allow you to choose your preferred format. Some common formats are
ambiguous, however; for example, 10/11/2012 means October 11,
2012 in the United States but it means November 10, 2012 in
Europe. To help avoid such ambiguities, the International Organization
for Standardization (ISO) provides the ISO 8601 standard, which
provides three different but easily distinguishable and unambiguous
ways to represent a date.

The ISO 8601 calendar date representation is

```
   YYYY-MM-DD
```

where YYYY is the four digit year, MM is the two-digit month
number (01 for January to 12 for December), and DD is the
two-digit day of the month (01 to 31). (Note that YYYY does not
correspond to Calc’s date formatting code, which will be introduced
later.) The year, which should be padded with zeros to ensure it has at
least four digits, is the Gregorian year, except that the year before
0001 (1 AD) is the year 0000 (1 BC). The date October 11, 2012 is
written 2012-10-11 in this representation and November 10, 2012 is
written 2012-11-10.

The ISO 8601 ordinal date representation is

```
  YYYY-DDD
```

where YYYY is the year, as above, and DDD is the day of the year.
The date December 31, 2011 is written 2011-365 in this representation
and January 1, 2012 is written 2012-001.

The ISO 8601 week date representation is

```
 YYYY-Www-D
```

where YYYY is the ISO week-numbering year, ww is the two
digit week number (preceded by a literal “W”), and D is the day
of the week (1 for Monday through 7 for Sunday). The ISO week-numbering
year is based on the Gregorian year but can differ slightly. The first
week of an ISO week-numbering year is the week with the Gregorian year’s
first Thursday in it (equivalently, the week containing January 4);
any day of that week (Monday through Sunday) is part of the same ISO
week-numbering year, any day from the previous week is part of the
previous year. For example, January 4, 2013 is on a Friday, and so
the first week for the ISO week-numbering year 2013 starts on
Monday, December 31, 2012. The day December 31, 2012 is then part of the
Gregorian year 2012 but ISO week-numbering year 2013. In the week
date representation, this week goes from 2013-W01-1 (December 31,
2012) to 2013-W01-7 (January 6, 2013).

All three ISO 8601 representations arrange the numbers from most
significant to least significant; as well as being unambiguous
representations, they are easy to sort since chronological order in
this formats corresponds to lexicographical order. The hyphens are
sometimes omitted.

The ISO 8601 standard uses a 24 hour clock; a particular time is
represented by hh:mm:ss where hh is the
two-digit hour (from 00 to 24), mm is the two-digit minute (from
00 to 59) and ss is the two-digit second. The seconds or minutes
and seconds can be omitted, and decimals can be added. If a date with a
time is represented, they should be separated by a literal “T”, so noon
on December 13, 2012 can be represented as 2012-12-13T12:00.

---

Next: [Free-Form Dates](#Free_002dForm-Dates), Previous: [ISO 8601](#ISO-8601), Up: [Date Formats](#Date-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.7.2 Date Formatting Codes [¶](#Date-Formatting-Codes-1)

When displaying a date, the current date format is used. All
characters except for letters and ‘`<`’ and ‘`>`’ are
copied literally when dates are formatted. The portion between
‘`< >`’ markers is omitted for pure dates, or included for
date/time forms. Letters are interpreted according to the table
below.

When dates are read in during algebraic entry, Calc first tries to
match the input string to the current format either with or without
the time part. The punctuation characters (including spaces) must
match exactly; letter fields must correspond to suitable text in
the input. If this doesn’t work, Calc checks if the input is a
simple number; if so, the number is interpreted as a number of days
since Dec 31, 1 BC. Otherwise, Calc tries a much more relaxed and
flexible algorithm which is described in the next section.

Weekday names are ignored during reading.

Two-digit year numbers are interpreted as lying in the range
from 1941 to 2039. Years outside that range are always
entered and displayed in full. Year numbers with a leading
‘`+`’ sign are always interpreted exactly, allowing the
entry and display of the years 1 through 99 AD.

Here is a complete list of the formatting codes for dates:

Y
:   Year: “91” for 1991, “7” for 2007, “+23” for 23 AD.

YY
:   Year: “91” for 1991, “07” for 2007, “+23” for 23 AD.

BY
:   Year: “91” for 1991, “ 7” for 2007, “+23” for 23 AD.

YYY
:   Year: “1991” for 1991, “23” for 23 AD.

YYYY
:   Year: “1991” for 1991, “+23” for 23 AD.

ZYYY
:   Year: “1991” for 1991, “0023” for 23 AD, “0000” for 1 BC.

IYYY
:   Year: ISO 8601 week-numbering year.

aa
:   Year: “ad” or blank.

AA
:   Year: “AD” or blank.

aaa
:   Year: “ad ” or blank. (Note trailing space.)

AAA
:   Year: “AD ” or blank.

aaaa
:   Year: “a.d.” or blank.

AAAA
:   Year: “A.D.” or blank.

bb
:   Year: “bc” or blank.

BB
:   Year: “BC” or blank.

bbb
:   Year: “ bc” or blank. (Note leading space.)

BBB
:   Year: “ BC” or blank.

bbbb
:   Year: “b.c.” or blank.

BBBB
:   Year: “B.C.” or blank.

M
:   Month: “8” for August.

MM
:   Month: “08” for August.

BM
:   Month: “ 8” for August.

MMM
:   Month: “AUG” for August.

Mmm
:   Month: “Aug” for August.

mmm
:   Month: “aug” for August.

MMMM
:   Month: “AUGUST” for August.

Mmmm
:   Month: “August” for August.

D
:   Day: “7” for 7th day of month.

DD
:   Day: “07” for 7th day of month.

BD
:   Day: “ 7” for 7th day of month.

W
:   Weekday: “0” for Sunday, “6” for Saturday.

w
:   Weekday: “1” for Monday, “7” for Sunday.

WWW
:   Weekday: “SUN” for Sunday.

Www
:   Weekday: “Sun” for Sunday.

www
:   Weekday: “sun” for Sunday.

WWWW
:   Weekday: “SUNDAY” for Sunday.

Wwww
:   Weekday: “Sunday” for Sunday.

Iww
:   Week number: ISO 8601 week number, “W01” for week 1.

d
:   Day of year: “34” for Feb. 3.

ddd
:   Day of year: “034” for Feb. 3.

bdd
:   Day of year: “ 34” for Feb. 3.

T
:   Letter: Literal “T”.

h
:   Hour: “5” for 5 AM; “17” for 5 PM.

hh
:   Hour: “05” for 5 AM; “17” for 5 PM.

bh
:   Hour: “ 5” for 5 AM; “17” for 5 PM.

H
:   Hour: “5” for 5 AM and 5 PM.

HH
:   Hour: “05” for 5 AM and 5 PM.

BH
:   Hour: “ 5” for 5 AM and 5 PM.

p
:   AM/PM: “a” or “p”.

P
:   AM/PM: “A” or “P”.

pp
:   AM/PM: “am” or “pm”.

PP
:   AM/PM: “AM” or “PM”.

pppp
:   AM/PM: “a.m.” or “p.m.”.

PPPP
:   AM/PM: “A.M.” or “P.M.”.

m
:   Minutes: “7” for 7.

mm
:   Minutes: “07” for 7.

bm
:   Minutes: “ 7” for 7.

s
:   Seconds: “7” for 7; “7.23” for 7.23.

ss
:   Seconds: “07” for 7; “07.23” for 7.23.

bs
:   Seconds: “ 7” for 7; “ 7.23” for 7.23.

SS
:   Optional seconds: “07” for 7; blank for 0.

BS
:   Optional seconds: “ 7” for 7; blank for 0.

N
:   Numeric date/time: “726842.25” for 6:00am Wed Jan 9, 1991.

n
:   Numeric date: “726842” for any time on Wed Jan 9, 1991.

J
:   Julian date/time: “2448265.75” for 6:00am Wed Jan 9, 1991.

j
:   Julian date: “2448266” for any time on Wed Jan 9, 1991.

U
:   Unix time: “663400800” for 6:00am Wed Jan 9, 1991.

X
:   Brackets suppression. An “X” at the front of the format
    causes the surrounding ‘`< >`’ delimiters to be omitted
    when formatting dates. Note that the brackets are still
    required for algebraic entry.

If “SS” or “BS” (optional seconds) is preceded by a colon, the
colon is also omitted if the seconds part is zero.

If “bb,” “bbb” or “bbbb” or their upper-case equivalents
appear in the format, then negative year numbers are displayed
without a minus sign. Note that “aa” and “bb” are mutually
exclusive. Some typical usages would be ‘`YYYY AABB`’;
‘`AAAYYYYBBB`’; ‘`YYYYBBB`’.

The formats “YY,” “YYYY,” “MM,” “DD,” “ddd,” “hh,” “HH,”
“mm,” “ss,” and “SS” actually match any number of digits during
reading unless several of these codes are strung together with no
punctuation in between, in which case the input must have exactly as
many digits as there are letters in the format.

The “j,” “J,” and “U” formats do not make any time zone
adjustment. They effectively use ‘`julian(x,0)`’ and
‘`unixtime(x,0)`’ to make the conversion; see [Date Arithmetic](#Date-Arithmetic).

---

Next: [Standard Date Formats](#Standard-Date-Formats), Previous: [Date Formatting Codes](#Date-Formatting-Codes), Up: [Date Formats](#Date-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.7.3 Free-Form Dates [¶](#Free_002dForm-Dates-1)

When reading a date form during algebraic entry, Calc falls back
on the algorithm described here if the input does not exactly
match the current date format. This algorithm generally
“does the right thing” and you don’t have to worry about it,
but it is described here in full detail for the curious.

Calc does not distinguish between upper- and lower-case letters
while interpreting dates.

First, the time portion, if present, is located somewhere in the
text and then removed. The remaining text is then interpreted as
the date.

A time is of the form ‘`hh:mm:ss`’, possibly with the seconds
part omitted and possibly with an AM/PM indicator added to indicate
12-hour time. If the AM/PM is present, the minutes may also be
omitted. The AM/PM part may be any of the words ‘`am`’,
‘`pm`’, ‘`noon`’, or ‘`midnight`’; each of these may be
abbreviated to one letter, and the alternate forms ‘`a.m.`’,
‘`p.m.`’, and ‘`mid`’ are also understood. Obviously
‘`noon`’ and ‘`midnight`’ are allowed only on 12:00:00.
The words ‘`noon`’, ‘`mid`’, and ‘`midnight`’ are also
recognized with no number attached. Midnight will represent the
beginning of a day.

If there is no AM/PM indicator, the time is interpreted in 24-hour
format.

When reading the date portion, Calc first checks to see if it is an
ISO 8601 week-numbering date; if the string contains an integer
representing the year, a “W” followed by two digits for the week
number, and an integer from 1 to 7 representing the weekday (in that
order), then all other characters are ignored and this information
determines the date. Otherwise, all words and numbers are isolated
from the string; other characters are ignored. All words must be
either month names or day-of-week names (the latter of which are
ignored). Names can be written in full or as three-letter
abbreviations.

Large numbers, or numbers with ‘`+`’ or ‘`-`’ signs,
are interpreted as years. If one of the other numbers is
greater than 12, then that must be the day and the remaining
number in the input is therefore the month. Otherwise, Calc
assumes the month, day and year are in the same order that they
appear in the current date format. If the year is omitted, the
current year is taken from the system clock.

If there are too many or too few numbers, or any unrecognizable
words, then the input is rejected.

If there are any large numbers (of five digits or more) other than
the year, they are ignored on the assumption that they are something
like Julian dates that were included along with the traditional
date components when the date was formatted.

One of the words ‘`ad`’, ‘`a.d.`’, ‘`bc`’, or ‘`b.c.`’
may optionally be used; the latter two are equivalent to a
minus sign on the year value.

If you always enter a four-digit year, and use a name instead
of a number for the month, there is no danger of ambiguity.

---

Previous: [Free-Form Dates](#Free_002dForm-Dates), Up: [Date Formats](#Date-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.7.4 Standard Date Formats [¶](#Standard-Date-Formats-1)

There are actually ten standard date formats, numbered 0 through 9.
Entering a blank line at the `d d` command’s prompt gives
you format number 1, Calc’s usual format. You can enter any digit
to select the other formats.

To create your own standard date formats, give a numeric prefix
argument from 0 to 9 to the `d d` command. The format you
enter will be recorded as the new standard format of that
number, as well as becoming the new current date format.
You can save your formats permanently with the `m m`
command (see [Mode Settings](#Mode-Settings)).

0
:   ‘`N`’ (Numerical format)

1
:   ‘`<H:mm:SSpp >Www Mmm D, YYYY`’ (American format)

2
:   ‘`D Mmm YYYY<, h:mm:SS>`’ (European format)

3
:   ‘`Www Mmm BD< hh:mm:ss> YYYY`’ (Unix written date format)

4
:   ‘`M/D/Y< H:mm:SSpp>`’ (American slashed format)

5
:   ‘`D.M.Y< h:mm:SS>`’ (European dotted format)

6
:   ‘`M-D-Y< H:mm:SSpp>`’ (American dashed format)

7
:   ‘`D-M-Y< h:mm:SS>`’ (European dashed format)

8
:   ‘`j<, h:mm:ss>`’ (Julian day plus time)

9
:   ‘`YYddd< hh:mm:ss>`’ (Year-day format)

10
:   ‘`ZYYY-MM-DD Www< hh:mm>`’ (Org mode format)

11
:   ‘`IYYY-Iww-w<Thh:mm:ss>`’ (ISO 8601 week numbering format)

---

Next: [Justification](#Justification), Previous: [Date Formats](#Date-Formats), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.8 Truncating the Stack [¶](#Truncating-the-Stack-1)

The `d t` (`calc-truncate-stack`) command moves the ‘`.`’
line that marks the top-of-stack up or down in the Calculator buffer.
The number right above that line is considered to the be at the top of
the stack. Any numbers below that line are “hidden” from all stack
operations (although still visible to the user). This is similar to the
Emacs “narrowing” feature, except that the values below the ‘`.`’
are *visible*, just temporarily frozen. This feature allows you to
keep several independent calculations running at once in different parts
of the stack, or to apply a certain command to an element buried deep in
the stack.

Pressing `d t` by itself moves the ‘`.`’ to the line the cursor
is on. Thus, this line and all those below it become hidden. To un-hide
these lines, move down to the end of the buffer and press `d t`.
With a positive numeric prefix argument ‘`n`’, `d t` hides the
bottom ‘`n`’ values in the buffer. With a negative argument, it hides
all but the top ‘`n`’ values. With an argument of zero, it hides zero
values, i.e., moves the ‘`.`’ all the way down to the bottom.

The `d [` (`calc-truncate-up`) and `d ]`
(`calc-truncate-down`) commands move the ‘`.`’ up or down one
line at a time (or several lines with a prefix argument).

---

Next: [Labels](#Labels), Previous: [Truncating the Stack](#Truncating-the-Stack), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.9 Justification [¶](#Justification-1)

Values on the stack are normally left-justified in the window. You can
control this arrangement by typing `d <` (`calc-left-justify`),
`d >` (`calc-right-justify`), or `d =`
(`calc-center-justify`). For example, in Right-Justification mode,
stack entries are displayed flush-right against the right edge of the
window.

If you change the width of the Calculator window you may have to type
`d SPC` (`calc-refresh`) to re-align right-justified or centered
text.

Right-justification is especially useful together with fixed-point
notation (see `d f`; `calc-fix-notation`). With these modes
together, the decimal points on numbers will always line up.

With a numeric prefix argument, the justification commands give you
a little extra control over the display. The argument specifies the
horizontal “origin” of a display line. It is also possible to
specify a maximum line width using the `d b` command (see [Normal Language Modes](#Normal-Language-Modes)). For reference, the precise rules for formatting and
breaking lines are given below. Notice that the interaction between
origin and line width is slightly different in each justification
mode.

In Left-Justified mode, the line is indented by a number of spaces
given by the origin (default zero). If the result is longer than the
maximum line width, if given, or too wide to fit in the Calc window
otherwise, then it is broken into lines which will fit; each broken
line is indented to the origin.

In Right-Justified mode, lines are shifted right so that the rightmost
character is just before the origin, or just before the current
window width if no origin was specified. If the line is too long
for this, then it is broken; the current line width is used, if
specified, or else the origin is used as a width if that is
specified, or else the line is broken to fit in the window.

In Centering mode, the origin is the column number of the center of
each stack entry. If a line width is specified, lines will not be
allowed to go past that width; Calc will either indent less or
break the lines if necessary. If no origin is specified, half the
line width or Calc window width is used.

Note that, in each case, if line numbering is enabled the display
is indented an additional four spaces to make room for the line
number. The width of the line number is taken into account when
positioning according to the current Calc window width, but not
when positioning by explicit origins and widths. In the latter
case, the display is formatted as specified, and then uniformly
shifted over four spaces to fit the line numbers.

---

Previous: [Justification](#Justification), Up: [Display Modes](#Display-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.7.10 Labels [¶](#Labels-1)

The `d {` (`calc-left-label`) command prompts for a string,
then displays that string to the left of every stack entry. If the
entries are left-justified (see [Justification](#Justification)), then they will
appear immediately after the label (unless you specified an origin
greater than the length of the label). If the entries are centered
or right-justified, the label appears on the far left and does not
affect the horizontal position of the stack entry.

Give a blank string (with `d { RET`) to turn the label off.

The `d }` (`calc-right-label`) command similarly adds a
label on the righthand side. It does not affect positioning of
the stack entries unless they are right-justified. Also, if both
a line width and an origin are given in Right-Justified mode, the
stack entry is justified to the origin and the righthand label is
justified to the line width.

One application of labels would be to add equation numbers to
formulas you are manipulating in Calc and then copying into a
document (possibly using Embedded mode). The equations would
typically be centered, and the equation numbers would be on the
left or right as you prefer.

---

Next: [The `Modes` Variable](#Modes-Variable), Previous: [Display Modes](#Display-Modes), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
