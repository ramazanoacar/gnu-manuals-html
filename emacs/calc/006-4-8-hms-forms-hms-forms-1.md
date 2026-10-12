### 4.8 HMS Forms [¶](#HMS-Forms-1)

*HMS* stands for Hours-Minutes-Seconds; when used as an angular
argument, the interpretation is Degrees-Minutes-Seconds. All functions
that operate on angles accept HMS forms. These are interpreted as
degrees regardless of the current angular mode. It is also possible to
use HMS as the angular mode so that calculated angles are expressed in
degrees, minutes, and seconds.

The default format for HMS values is
‘`hours@ mins' secs"`’. During entry, the letters
‘`h`’ (for “hours”) or
‘`o`’ (approximating the “degrees” symbol) are accepted as well as
‘`@`’, ‘`m`’ is accepted in place of ‘`'`’, and ‘`s`’ is
accepted in place of ‘`"`’.
The hours value is an integer (or integer-valued float).
The mins value is an integer or integer-valued float between 0 and 59.
The secs value is a real number between 0 (inclusive) and 60
(exclusive). A positive HMS form is interpreted as hours +
mins/60 + secs/3600. A negative HMS form is interpreted
as *- hours* *-* mins/60 *-* secs/3600.
Display format for HMS forms is quite flexible. See [HMS Formats](#HMS-Formats).

HMS forms can be added and subtracted. When they are added to numbers,
the numbers are interpreted according to the current angular mode. HMS
forms can also be multiplied and divided by real numbers. Dividing
two HMS forms produces a real-valued ratio of the two angles.

Just for kicks, `M-x calc-time` pushes the current time of day on
the stack as an HMS form.

---

Next: [Modulo Forms](#Modulo-Forms), Previous: [HMS Forms](#HMS-Forms), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.9 Date Forms [¶](#Date-Forms-1)

A *date form* represents a date and possibly an associated time.
Simple date arithmetic is supported: Adding a number to a date
produces a new date shifted by that many days; adding an HMS form to
a date shifts it by that many hours. Subtracting two date forms
computes the number of days between them (represented as a simple
number). Many other operations, such as multiplying two date forms,
are nonsensical and are not allowed by Calc.

Date forms are entered and displayed enclosed in ‘`< >`’ brackets.
The default format is, e.g., ‘`<Wed Jan 9, 1991>`’ for dates,
or ‘`<3:32:20pm Wed Jan 9, 1991>`’ for dates with times.
Input is flexible; date forms can be entered in any of the usual
notations for dates and times. See [Date Formats](#Date-Formats).

Date forms are stored internally as numbers, specifically the number
of days since midnight on the morning of December 31 of the year 1 BC.
If the internal number is an integer, the form represents a date only;
if the internal number is a fraction or float, the form represents
a date and time. For example, ‘`<6:00am Thu Jan 10, 1991>`’
is represented by the number 726842.25. The standard precision of
12 decimal digits is enough to ensure that a (reasonable) date and
time can be stored without roundoff error.

If the current precision is greater than 12, date forms will keep
additional digits in the seconds position. For example, if the
precision is 15, the seconds will keep three digits after the
decimal point. Decreasing the precision below 12 may cause the
time part of a date form to become inaccurate. This can also happen
if astronomically high years are used, though this will not be an
issue in everyday (or even everymillennium) use. Note that date
forms without times are stored as exact integers, so roundoff is
never an issue for them.

You can use the `v p` (`calc-pack`) and `v u`
(`calc-unpack`) commands to get at the numerical representation
of a date form. See [Packing and Unpacking](#Packing-and-Unpacking).

Date forms can go arbitrarily far into the future or past. Negative
year numbers represent years BC. There is no “year 0”; the day
before ‘`<Mon Jan 1, +1>`’ is ‘`<Sun Dec 31, -1>`’. These are
days 1 and 0 respectively in Calc’s internal numbering scheme. The
Gregorian calendar is used for all dates, including dates before the
Gregorian calendar was invented (although that can be configured; see
below). Thus Calc’s use of the day number *-10000* to
represent August 15, 28 BC should be taken with a grain of salt.

Some historical background: The Julian calendar was created by
Julius Caesar in the year 46 BC as an attempt to fix the confusion
caused by the irregular Roman calendar that was used before that time.
The Julian calendar introduced an extra day in all years divisible by
four. After some initial confusion, the calendar was adopted around
the year we call 8 AD. Some centuries later it became
apparent that the Julian year of 365.25 days was itself not quite
right. In 1582 Pope Gregory XIII introduced the Gregorian calendar,
which added the new rule that years divisible by 100, but not by 400,
were not to be considered leap years despite being divisible by four.
Many countries delayed adoption of the Gregorian calendar
because of religious differences. For example, Great Britain and the
British colonies switched to the Gregorian calendar in September
1752, when the Julian calendar was eleven days behind the
Gregorian calendar. That year in Britain, the day after September 2
was September 14. To take another example, Russia did not adopt the
Gregorian calendar until 1918, and that year in Russia the day after
January 31 was February 14. Calc’s reckoning therefore matches English
practice starting in 1752 and Russian practice starting in 1918, but
disagrees with earlier dates in both countries.

When the Julian calendar was introduced, it had January 1 as the first
day of the year. By the Middle Ages, many European countries
had changed the beginning of a new year to a different date, often to
a religious festival. Almost all countries reverted to using January 1
as the beginning of the year by the time they adopted the Gregorian
calendar.

Some calendars attempt to mimic the historical situation by using the
Gregorian calendar for recent dates and the Julian calendar for older
dates. The `cal` program in most Unix implementations does this,
for example. While January 1 wasn’t always the beginning of a calendar
year, these hybrid calendars still use January 1 as the beginning of
the year even for older dates. The customizable variable
`calc-gregorian-switch` (see [Customizing Calc](#Customizing-Calc)) can be set to
have Calc’s date forms switch from the Julian to Gregorian calendar at
any specified date.

A few platforms support leap seconds, such as the time stamp
1972-06-30 23:59:60 UTC, an extra second appended to June 1972.
These do not occur regularly and Calc does not take these minor
effects into account. (If it did, it would have to report a
non-integer number of days between, say,
‘`<12:00am Mon Jan 1, 1900>`’ and
‘`<12:00am Sat Jan 1, 2000>`’.)

Another day counting system in common use is, confusingly, also called
“Julian.” Julian days go from noon to noon. The Julian day number
is the numbers of days since 12:00 noon (GMT) on November 24, 4714 BC
in the Gregorian calendar (i.e., January 1, 4713 BC in the Julian
calendar). In Calc’s scheme (in GMT) the Julian day origin is
*-1721424.5*, because Calc starts at midnight instead of noon.
Thus to convert a Calc date code obtained by unpacking a
date form into a Julian day number, simply add 1721424.5 after
compensating for the time zone difference. The built-in `t J`
command performs this conversion for you.

The Julian day number is based on the Julian cycle, which was invented
in 1583 by Joseph Justus Scaliger. Scaliger named it the Julian cycle
since it involves the Julian calendar, but some have suggested that
Scaliger named it in honor of his father, Julius Caesar Scaliger. The
Julian cycle is based on three other cycles: the indiction cycle, the
Metonic cycle, and the solar cycle. The indiction cycle is a 15 year
cycle originally used by the Romans for tax purposes but later used to
date medieval documents. The Metonic cycle is a 19 year cycle; 19
years is close to being a common multiple of a solar year and a lunar
month, and so every 19 years the phases of the moon will occur on the
same days of the year. The solar cycle is a 28 year cycle; the Julian
calendar repeats itself every 28 years. The smallest time period
which contains multiples of all three cycles is the least common
multiple of 15 years, 19 years and 28 years, which (since they’re
pairwise relatively prime) is
15\*19\*28 = 7980 years.
This is the length of a Julian cycle. Working backwards, the previous
year in which all three cycles began was 4713 BC, and so Scaliger
chose that year as the beginning of a Julian cycle. Since at the time
there were no historical records from before 4713 BC, using this year
as a starting point had the advantage of avoiding negative year
numbers. In 1849, the astronomer John Herschel (son of William
Herschel) suggested using the number of days since the beginning of
the Julian cycle as an astronomical dating system; this idea was taken
up by other astronomers. (At the time, noon was the start of the
astronomical day. Herschel originally suggested counting the days
since Jan 1, 4713 BC at noon Alexandria time; this was later amended to
noon GMT.) Julian day numbering is largely used in astronomy.

The Unix operating system measures time as an integer number of
seconds since midnight, Jan 1, 1970. To convert a Calc date
value into a Unix time stamp, first subtract 719163 (the code
for ‘`<Jan 1, 1970>`’), then multiply by 86400 (the number of
seconds in a day) and press `R` to round to the nearest
integer. If you have a date form, you can simply subtract the
day ‘`<Jan 1, 1970>`’ instead of unpacking and subtracting
719163. Likewise, divide by 86400 and add ‘`<Jan 1, 1970>`’
to convert from Unix time to a Calc date form. (Note that
Unix normally maintains the time in the GMT time zone; you may
need to subtract five hours to get New York time, or eight hours
for California time. The same is usually true of Julian day
counts.) The built-in `t U` command performs these
conversions.

---

Next: [Error Forms](#Error-Forms), Previous: [Date Forms](#Date-Forms), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.10 Modulo Forms [¶](#Modulo-Forms-1)

A *modulo form* is a real number which is taken modulo (i.e., within
an integer multiple of) some value M. Arithmetic modulo M
often arises in number theory. Modulo forms are written
‘a `mod` M’,
where a and M are real numbers or HMS forms, and
‘`0 <= a < M`’.
In many applications ‘`a`’ and ‘`M`’ will be
integers but this is not required.

To create a modulo form during numeric entry, press the shift-`M`
key to enter the word ‘`mod`’. As a special convenience, pressing
shift-`M` a second time automatically enters the value of ‘`M`’
that was most recently used before. During algebraic entry, either
type ‘`mod`’ by hand or press `M-m` (that’s `META-m`).
Once again, pressing this a second time enters the current modulo.

Modulo forms are not to be confused with the modulo operator ‘`%`’.
The expression ‘`27 % 10`’ means to compute 27 modulo 10 to produce
the result 7. Further computations treat this 7 as just a regular integer.
The expression ‘`27 mod 10`’ produces the result ‘`7 mod 10`’;
further computations with this value are again reduced modulo 10 so that
the result always lies in the desired range.

When two modulo forms with identical ‘`M`’’s are added or multiplied,
the Calculator simply adds or multiplies the values, then reduces modulo
‘`M`’. If one argument is a modulo form and the other a plain number,
the plain number is treated like a compatible modulo form. It is also
possible to raise modulo forms to powers; the result is the value raised
to the power, then reduced modulo ‘`M`’. (When all values involved
are integers, this calculation is done much more efficiently than
actually computing the power and then reducing.)

Two modulo forms ‘a `mod` M’ and ‘b `mod` M’
can be divided if ‘`a`’, ‘`b`’, and ‘`M`’ are all
integers. The result is the modulo form which, when multiplied by
‘b `mod` M’, produces ‘a `mod` M’. If
there is no solution to this equation (which can happen only when
‘`M`’ is non-prime), or if any of the arguments are non-integers, the
division is left in symbolic form. Other operations, such as square
roots, are not yet supported for modulo forms. (Note that, although
‘`(`a `mod` M`)^.5`’ will compute a “modulo square root”
in the sense of reducing
‘`sqrt(a)`’
modulo ‘`M`’, this is not a useful definition from the
number-theoretical point of view.)

It is possible to mix HMS forms and modulo forms. For example, an
HMS form modulo 24 could be used to manipulate clock times; an HMS
form modulo 360 would be suitable for angles. Making the modulo ‘`M`’
also be an HMS form eliminates troubles that would arise if the angular
mode were inadvertently set to Radians, in which case
‘`2@ 0' 0" mod 24`’ would be interpreted as two degrees modulo
24 radians!

Modulo forms cannot have variables or formulas for components. If you
enter the formula ‘`(x + 2) mod 5`’, Calc propagates the modulus
to each of the coefficients: ‘`(1 mod 5) x + (2 mod 5)`’.

You can use `v p` and `%` to modify modulo forms.
See [Packing and Unpacking](#Packing-and-Unpacking). See [Basic Arithmetic](#Basic-Arithmetic).

The algebraic function ‘`makemod(a, m)`’ builds the modulo form
‘`a mod m`’.

---

Next: [Interval Forms](#Interval-Forms), Previous: [Modulo Forms](#Modulo-Forms), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.11 Error Forms [¶](#Error-Forms-1)

An *error form* is a number with an associated standard
deviation, as in ‘`2.3 +/- 0.12`’. The notation
‘x `+/-` sigma’
stands for an uncertain value which follows
a normal or Gaussian distribution of mean ‘`x`’ and standard
deviation or “error”
‘`sigma`’.
Both the mean and the error can be either numbers or
formulas. Generally these are real numbers but the mean may also be
complex. If the error is negative or complex, it is changed to its
absolute value. An error form with zero error is converted to a
regular number by the Calculator.

All arithmetic and transcendental functions accept error forms as input.
Operations on the mean-value part work just like operations on regular
numbers. The error part for any function ‘`f(x)`’ (such as
‘`sin(x)`’)
is defined by the error of ‘`x`’ times the derivative of ‘`f`’
evaluated at the mean value of ‘`x`’. For a two-argument function
‘`f(x,y)`’ (such as addition) the error is the square root of the sum
of the squares of the errors due to ‘`x`’ and ‘`y`’.
Note that this
definition assumes the errors in ‘`x`’ and ‘`y`’ are uncorrelated.
A side effect of this definition is that ‘`(2 +/- 1) * (2 +/- 1)`’
is not the same as ‘`(2 +/- 1)^2`’; the former represents the product
of two independent values which happen to have the same probability
distributions, and the latter is the product of one random value with itself.
The former will produce an answer with less error, since on the average
the two independent errors can be expected to cancel out.

Consult a good text on error analysis for a discussion of the proper use
of standard deviations. Actual errors often are neither Gaussian-distributed
nor uncorrelated, and the above formulas are valid only when errors
are small. As an example, the error arising from
‘`sin(`x `+/-` sigma`)`’
is
‘sigma `abs(cos(`x`))`’.
When ‘`x`’ is close to zero,
‘`cos(x)`’
is close to one so the error in the sine is close to
‘`sigma`’;
this makes sense, since
‘`sin(x)`’
is approximately ‘`x`’ near zero, so a given error in ‘`x`’ will
produce about the same error in the sine. Likewise, near 90 degrees
‘`cos(x)`’
is nearly zero and so the computed error is
small: The sine curve is nearly flat in that region, so an error in ‘`x`’
has relatively little effect on the value of
‘`sin(x)`’.
However, consider ‘`sin(90 +/- 1000)`’. The cosine of 90 is zero, so
Calc will report zero error! We get an obviously wrong result because
we have violated the small-error approximation underlying the error
analysis. If the error in ‘`x`’ had been small, the error in
‘`sin(x)`’
would indeed have been negligible.

To enter an error form during regular numeric entry, use the `p`
(“plus-or-minus”) key to type the ‘`+/-`’ symbol. (If you try actually
typing ‘`+/-`’ the `+` key will be interpreted as the Calculator’s
`+` command!) Within an algebraic formula, you can press `M-+` to
type the ‘`+/-`’ symbol, or type it out by hand.

Error forms and complex numbers can be mixed; the formulas shown above
are used for complex numbers, too; note that if the error part evaluates
to a complex number its absolute value (or the square root of the sum of
the squares of the absolute values of the two error contributions) is
used. Mathematically, this corresponds to a radially symmetric Gaussian
distribution of numbers on the complex plane. However, note that Calc
considers an error form with real components to represent a real number,
not a complex distribution around a real mean.

Error forms may also be composed of HMS forms. For best results, both
the mean and the error should be HMS forms if either one is.

The algebraic function ‘`sdev(a, b)`’ builds the error form ‘`a +/- b`’.

---

Next: [Incomplete Objects](#Incomplete-Objects), Previous: [Error Forms](#Error-Forms), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.12 Interval Forms [¶](#Interval-Forms-1)

An *interval* is a subset of consecutive real numbers. For example,
the interval ‘`[2 .. 4]`’ represents all the numbers from 2 to 4,
inclusive. If you multiply it by the interval ‘`[0.5 .. 2]`’ you
obtain ‘`[1 .. 8]`’. This calculation represents the fact that if
you multiply some number in the range ‘`[2 .. 4]`’ by some other
number in the range ‘`[0.5 .. 2]`’, your result will lie in the range
from 1 to 8. Interval arithmetic is used to get a worst-case estimate
of the possible range of values a computation will produce, given the
set of possible values of the input.

Calc supports several varieties of intervals, including *closed*
intervals of the type shown above, *open* intervals such as
‘`(2 .. 4)`’, which represents the range of numbers from 2 to 4
*exclusive*, and *semi-open* intervals in which one end
uses a round parenthesis and the other a square bracket. In mathematical
terms,
‘`[2 .. 4]`’ means ‘`2 <= x <= 4`’, whereas
‘`[2 .. 4)`’ represents ‘`2 <= x < 4`’,
‘`(2 .. 4]`’ represents ‘`2 < x <= 4`’, and
‘`(2 .. 4)`’ represents ‘`2 < x < 4`’.

The lower and upper limits of an interval must be either real numbers
(or HMS or date forms), or symbolic expressions which are assumed to be
real-valued, or ‘`-inf`’ and ‘`inf`’. In general the lower limit
must be less than the upper limit. A closed interval containing only
one value, ‘`[3 .. 3]`’, is converted to a plain number (3)
automatically. An interval containing no values at all (such as
‘`[3 .. 2]`’ or ‘`[2 .. 2)`’) can be represented but is not
guaranteed to behave well when used in arithmetic. Note that the
interval ‘`[3 .. inf)`’ represents all real numbers greater than
or equal to 3, and ‘`(-inf .. inf)`’ represents all real numbers.
In fact, ‘`[-inf .. inf]`’ represents all real numbers including
the real infinities.

Intervals are entered in the notation shown here, either as algebraic
formulas, or using incomplete forms. (See [Incomplete Objects](#Incomplete-Objects).)
In algebraic formulas, multiple periods in a row are collected from
left to right, so that ‘`1...1e2`’ is interpreted as ‘`1.0 .. 1e2`’
rather than ‘`1 .. 0.1e2`’. Add spaces or zeros if you want to
get the other interpretation. If you omit the lower or upper limit,
a default of ‘`-inf`’ or ‘`inf`’ (respectively) is furnished.

Infinite mode also affects operations on intervals
(see [Infinities](#Infinities)). Calc will always introduce an open infinity,
as in ‘`1 / (0 .. 2] = [0.5 .. inf)`’. But closed infinities,
‘`1 / [0 .. 2] = [0.5 .. inf]`’, arise only in Infinite mode;
otherwise they are left unevaluated. Note that the “direction” of
a zero is not an issue in this case since the zero is always assumed
to be continuous with the rest of the interval. For intervals that
contain zero inside them Calc is forced to give the result,
‘`1 / (-2 .. 2) = [-inf .. inf]`’.

While it may seem that intervals and error forms are similar, they are
based on entirely different concepts of inexact quantities. An error
form
‘x `+/-` sigma’
means a variable is random, and its value could
be anything but is “probably” within one
sigma
of the mean value ‘`x`’. An interval
‘`[`a `..` b`]`’ means a
variable’s value is unknown, but guaranteed to lie in the specified
range. Error forms are statistical or “average case” approximations;
interval arithmetic tends to produce “worst case” bounds on an
answer.

Intervals may not contain complex numbers, but they may contain
HMS forms or date forms.

See [Set Operations using Vectors](#Set-Operations), for commands that interpret interval forms
as subsets of the set of real numbers.

The algebraic function ‘`intv(n, a, b)`’ builds an interval form
from ‘`a`’ to ‘`b`’; ‘`n`’ is an integer code which must
be 0 for ‘`(..)`’, 1 for ‘`(..]`’, 2 for ‘`[..)`’, or
3 for ‘`[..]`’.

Please note that in fully rigorous interval arithmetic, care would be
taken to make sure that the computation of the lower bound rounds toward
minus infinity, while upper bound computations round toward plus
infinity. Calc’s arithmetic always uses a round-to-nearest mode,
which means that roundoff errors could creep into an interval
calculation to produce intervals slightly smaller than they ought to
be. For example, entering ‘`[1..2]`’ and pressing `Q 2 ^`
should yield the interval ‘`[1..2]`’ again, but in fact it yields the
(slightly too small) interval ‘`[1..1.9999999]`’ due to roundoff
error.

---

Next: [Variables](#Variables), Previous: [Interval Forms](#Interval-Forms), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.13 Incomplete Objects [¶](#Incomplete-Objects-1)

When `(` or `[` is typed to begin entering a complex number or
vector, respectively, the effect is to push an *incomplete* complex
number or vector onto the stack. The `,` key adds the value(s) at
the top of the stack onto the current incomplete object. The `)`
and `]` keys “close” the incomplete object after adding any values
on the top of the stack in front of the incomplete object.

As a result, the sequence of keystrokes `[ 2 , 3 RET 2 * , 9 ]`
pushes the vector ‘`[2, 6, 9]`’ onto the stack. Likewise, `( 1 , 2 Q )`
pushes the complex number ‘`(1, 1.414)`’ (approximately).

If several values lie on the stack in front of the incomplete object,
all are collected and appended to the object. Thus the `,` key
is redundant: `[ 2 RET 3 RET 2 * 9 ]`. Some people
prefer the equivalent `SPC` key to `RET`.

As a special case, typing `,` immediately after `(`, `[`, or
`,` adds a zero or duplicates the preceding value in the list being
formed. Typing `DEL` during incomplete entry removes the last item
from the list.

The `;` key is used in the same way as `,` to create polar complex
numbers: `( 1 ; 2 )`. When entering a vector, `;` is useful for
creating a matrix. In particular, `[ [ 1 , 2 ; 3 , 4 ; 5 , 6 ] ]` is
equivalent to `[ [ 1 , 2 ] , [ 3 , 4 ] , [ 5 , 6 ] ]`.

Incomplete entry is also used to enter intervals. For example,
`[ 2 .. 4 )` enters a semi-open interval. Note that when you type
the first period, it will be interpreted as a decimal point, but when
you type a second period immediately afterward, it is re-interpreted as
part of the interval symbol. Typing `..` corresponds to executing
the `calc-dots` command.

If you find incomplete entry distracting, you may wish to enter vectors
and complex numbers as algebraic formulas by pressing the apostrophe key.

---

Next: [Formulas](#Formulas), Previous: [Incomplete Objects](#Incomplete-Objects), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.14 Variables [¶](#Variables-1)

A *variable* is somewhere between a storage register on a conventional
calculator, and a variable in a programming language. (In fact, a Calc
variable is really just an Emacs Lisp variable that contains a Calc number
or formula.) A variable’s name is normally composed of letters and digits.
Calc also allows apostrophes and `#` signs in variable names.
(The Calc variable `foo` corresponds to the Emacs Lisp variable
`var-foo`, but unless you access the variable from within Emacs
Lisp, you don’t need to worry about it. Variable names in algebraic
formulas implicitly have ‘`var-`’ prefixed to their names. The
‘`#`’ character in variable names used in algebraic formulas
corresponds to a dash ‘`-`’ in the Lisp variable name. If the name
contains any dashes, the prefix ‘`var-`’ is *not* automatically
added. Thus the two formulas ‘`foo + 1`’ and ‘`var#foo + 1`’ both
refer to the same variable.)

In a command that takes a variable name, you can either type the full
name of a variable, or type a single digit to use one of the special
convenience variables `q0` through `q9`. For example,
`3 s s 2` stores the number 3 in variable `q2`, and
`3 s s foo RET` stores that number in variable
`foo`.

To push a variable itself (as opposed to the variable’s value) on the
stack, enter its name as an algebraic expression using the apostrophe
(`'`) key.

The `=` (`calc-evaluate`) key “evaluates” a formula by
replacing all variables in the formula which have been given values by a
`calc-store` or `calc-let` command by their stored values.
Other variables are left alone. Thus a variable that has not been
stored acts like an abstract variable in algebra; a variable that has
been stored acts more like a register in a traditional calculator.
With a positive numeric prefix argument, `=` evaluates the top
n stack entries; with a negative argument, `=` evaluates
the nth stack entry.

A few variables are called *special constants*. Their names are
‘`e`’, ‘`pi`’, ‘`i`’, ‘`phi`’, and ‘`gamma`’.
(See [Scientific Functions](#Scientific-Functions).) When they are evaluated with `=`,
their values are calculated if necessary according to the current precision
or complex polar mode. If you wish to use these symbols for other purposes,
simply undefine or redefine them using `calc-store`.

The variables ‘`inf`’, ‘`uinf`’, and ‘`nan`’ stand for
infinite or indeterminate values. It’s best not to use them as
regular variables, since Calc uses special algebraic rules when
it manipulates them. Calc displays a warning message if you store
a value into any of these special variables.

See [Storing and Recalling](#Store-and-Recall), for a discussion of commands dealing with variables.

---

Previous: [Variables](#Variables), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 4.15 Formulas [¶](#Formulas-1)

When you press the apostrophe key you may enter any expression or formula
in algebraic form. (Calc uses the terms “expression” and “formula”
interchangeably.) An expression is built up of numbers, variable names,
and function calls, combined with various arithmetic operators.
Parentheses may
be used to indicate grouping. Spaces are ignored within formulas, except
that spaces are not permitted within variable names or numbers.
Arithmetic operators, in order from highest to lowest precedence, and
with their equivalent function names, are:

‘`_`’ [`subscr`] (subscripts);

postfix ‘`%`’ [`percent`] (as in ‘`25% = 0.25`’);

prefix ‘`!`’ [`lnot`] (logical “not,” as in ‘`!x`’);

‘`+/-`’ [`sdev`] (the standard deviation symbol) and
‘`mod`’ [`makemod`] (the symbol for modulo forms);

postfix ‘`!`’ [`fact`] (factorial, as in ‘`n!`’)
and postfix ‘`!!`’ [`dfact`] (double factorial);

‘`^`’ [`pow`] (raised-to-the-power-of);

prefix ‘`+`’ and ‘`-`’ [`neg`] (as in ‘`-x`’);

‘`*`’ [`mul`];

‘`/`’ [`div`], ‘`%`’ [`mod`] (modulo), and
‘`\`’ [`idiv`] (integer division);

infix ‘`+`’ [`add`] and ‘`-`’ [`sub`] (as in ‘`x-y`’);

‘`|`’ [`vconcat`] (vector concatenation);

relations ‘`=`’ [`eq`], ‘`!=`’ [`neq`], ‘`<`’ [`lt`],
‘`>`’ [`gt`], ‘`<=`’ [`leq`], and ‘`>=`’ [`geq`];

‘`&&`’ [`land`] (logical “and”);

‘`||`’ [`lor`] (logical “or”);

the C-style “if” operator ‘`a?b:c`’ [`if`];

‘`!!!`’ [`pnot`] (rewrite pattern “not”);

‘`&&&`’ [`pand`] (rewrite pattern “and”);

‘`|||`’ [`por`] (rewrite pattern “or”);

‘`:=`’ [`assign`] (for assignments and rewrite rules);

‘`::`’ [`condition`] (rewrite pattern condition);

‘`=>`’ [`evalto`].

Note that, unlike in usual computer notation, multiplication binds more
strongly than division: ‘`a*b/c*d`’ is equivalent to
‘`(a*b)/(c*d)`’.

The multiplication sign ‘`*`’ may be omitted in many cases. In particular,
if the righthand side is a number, variable name, or parenthesized
expression, the ‘`*`’ may be omitted. Implicit multiplication has the
same precedence as the explicit ‘`*`’ operator. The one exception to
the rule is that a variable name followed by a parenthesized expression,
as in ‘`f(x)`’,
is interpreted as a function call, not an implicit ‘`*`’. In many
cases you must use a space if you omit the ‘`*`’: ‘`2a`’ is the
same as ‘`2*a`’, and ‘`a b`’ is the same as ‘`a*b`’, but ‘`ab`’
is a variable called `ab`, *not* the product of ‘`a`’ and
‘`b`’! Also note that ‘`f (x)`’ is still a function call.

The rules are slightly different for vectors written with square brackets.
In vectors, the space character is interpreted (like the comma) as a
separator of elements of the vector. Thus ‘`[ 2a b+c d ]`’ is
equivalent to ‘`[2*a, b+c, d]`’, whereas ‘`2a b+c d`’ is equivalent
to ‘`2*a*b + c*d`’.
Note that spaces around the brackets, and around explicit commas, are
ignored. To force spaces to be interpreted as multiplication you can
enclose a formula in parentheses as in ‘`[(a b) 2(c d)]`’, which is
interpreted as ‘`[a*b, 2*c*d]`’. An implicit comma is also inserted
between ‘`][`’, as in the matrix ‘`[[1 2][3 4]]`’.

Vectors that contain commas (not embedded within nested parentheses or
brackets) do not treat spaces specially: ‘`[a b, 2 c d]`’ is a vector
of two elements. Also, if it would be an error to treat spaces as
separators, but not otherwise, then Calc will ignore spaces:
‘`[a - b]`’ is a vector of one element, but ‘`[a -b]`’ is
a vector of two elements. Finally, vectors entered with curly braces
instead of square brackets do not give spaces any special treatment.
When Calc displays a vector that does not contain any commas, it will
insert parentheses if necessary to make the meaning clear:
‘`[(a b)]`’.

The expression ‘`5%-2`’ is ambiguous; is this five-percent minus two,
or five modulo minus-two? Calc always interprets the leftmost symbol as
an infix operator preferentially (modulo, in this case), so you would
need to write ‘`(5%)-2`’ to get the former interpretation.

A function call is, e.g., ‘`sin(1+x)`’. (The Calc algebraic function
`foo` corresponds to the Emacs Lisp function `calcFunc-foo`,
but unless you access the function from within Emacs Lisp, you don’t
need to worry about it.) Most mathematical Calculator commands like
`calc-sin` have function equivalents like `sin`.
If no Lisp function is defined for a function called by a formula, the
call is left as it is during algebraic manipulation: ‘`f(x+y)`’ is
left alone. Beware that many innocent-looking short names like `in`
and `re` have predefined meanings which could surprise you; however,
single letters or single letters followed by digits are always safe to
use for your own function names. See [Index of Algebraic Functions](#Function-Index).

In the documentation for particular commands, the notation `H S`
(`calc-sinh`) [`sinh`] means that the key sequence `H S`, the
command `M-x calc-sinh`, and the algebraic function `sinh(x)` all
represent the same operation.

Commands that interpret (“parse”) text as algebraic formulas include
algebraic entry (`'`), editing commands like `` ` `` which parse
the contents of the editing buffer when you finish, the `C-x * g`
and `C-x * r` commands, the `C-y` command, the X window system
“paste” mouse operation, and Embedded mode. All of these operations
use the same rules for parsing formulas; in particular, language modes
(see [Language Modes](#Language-Modes)) affect them all in the same way.

When you read a large amount of text into the Calculator (say a vector
which represents a big set of rewrite rules; see [Rewrite Rules](#Rewrite-Rules)),
you may wish to include comments in the text. Calc’s formula parser
ignores the symbol ‘`%%`’ and anything following it on a line:

```
[ a + b,   %% the sum of "a" and "b"
  c + d,
  %% last line is coming up:
  e + f ]
```

This is parsed exactly the same as ‘`[ a + b, c + d, e + f ]`’.

See [Syntax Tables](#Syntax-Tables), for a way to create your own operators and other
input notations. See [Compositions](#Compositions), for a way to create new display
formats.

See [Algebra](#Algebra), for commands for manipulating formulas symbolically.

---

Next: [Mode Settings](#Mode-Settings), Previous: [Data Types](#Data-Types), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 5 Stack and Trail Commands [¶](#Stack-and-Trail-Commands)

This chapter describes the Calc commands for manipulating objects on the
stack and in the trail buffer. (These commands operate on objects of any
type, such as numbers, vectors, formulas, and incomplete objects.)

* [Stack Manipulation Commands](#Stack-Manipulation)
* [Editing Stack Entries](#Editing-Stack-Entries)
* [Trail Commands](#Trail-Commands)
* [Keep Arguments](#Keep-Arguments)

---

Next: [Editing Stack Entries](#Editing-Stack-Entries), Up: [Stack and Trail Commands](#Stack-and-Trail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 5.1 Stack Manipulation Commands [¶](#Stack-Manipulation-Commands)

To duplicate the top object on the stack, press `RET` or `SPC`
(two equivalent keys for the `calc-enter` command).
Given a positive numeric prefix argument, these commands duplicate
several elements at the top of the stack.
Given a negative argument,
these commands duplicate the specified element of the stack.
Given an argument of zero, they duplicate the entire stack.
For example, with ‘`10 20 30`’ on the stack,
`RET` creates ‘`10 20 30 30`’,
`C-u 2 RET` creates ‘`10 20 30 20 30`’,
`C-u - 2 RET` creates ‘`10 20 30 20`’, and
`C-u 0 RET` creates ‘`10 20 30 10 20 30`’.

The `LFD` (`calc-over`) command (on a key marked Line-Feed if you
have it, else on `C-j`) is like `calc-enter`
except that the sign of the numeric prefix argument is interpreted
oppositely. Also, with no prefix argument the default argument is 2.
Thus with ‘`10 20 30`’ on the stack, `LFD` and `C-u 2 LFD`
are both equivalent to `C-u - 2 RET`, producing
‘`10 20 30 20`’.

To remove the top element from the stack, press `DEL` (`calc-pop`).
The `C-d` key is a synonym for `DEL`.
(If the top element is an incomplete object with at least one element, the
last element is removed from it.) Given a positive numeric prefix argument,
several elements are removed. Given a negative argument, the specified
element of the stack is deleted. Given an argument of zero, the entire
stack is emptied.
For example, with ‘`10 20 30`’ on the stack,
`DEL` leaves ‘`10 20`’,
`C-u 2 DEL` leaves ‘`10`’,
`C-u - 2 DEL` leaves ‘`10 30`’, and
`C-u 0 DEL` leaves an empty stack.

The `M-DEL` (`calc-pop-above`) command is to `DEL` what
`LFD` is to `RET`: It interprets the sign of the numeric
prefix argument in the opposite way, and the default argument is 2.
Thus `M-DEL` by itself removes the second-from-top stack element,
leaving the first, third, fourth, and so on; `M-3 M-DEL` deletes
the third stack element.

The above commands do not depend on the location of the cursor.
If the customizable variable `calc-context-sensitive-enter` is
non-`nil` (see [Customizing Calc](#Customizing-Calc)), these commands will become
context sensitive. For example, instead of duplicating the top of the stack,
`RET` will copy the element at the cursor to the top of the
stack. With a positive numeric prefix, a copy of the element at the
cursor and the appropriate number of preceding elements will be placed
at the top of the stack. A negative prefix will still duplicate the
specified element of the stack regardless of the cursor position.
Similarly, `DEL` will remove the corresponding elements from the
stack.

To exchange the top two elements of the stack, press `TAB`
(`calc-roll-down`). Given a positive numeric prefix argument, the
specified number of elements at the top of the stack are rotated downward.
Given a negative argument, the entire stack is rotated downward the specified
number of times. Given an argument of zero, the entire stack is reversed
top-for-bottom.
For example, with ‘`10 20 30 40 50`’ on the stack,
`TAB` creates ‘`10 20 30 50 40`’,
`C-u 3 TAB` creates ‘`10 20 50 30 40`’,
`C-u - 2 TAB` creates ‘`40 50 10 20 30`’, and
`C-u 0 TAB` creates ‘`50 40 30 20 10`’.

The command `M-TAB` (`calc-roll-up`) is analogous to `TAB`
except that it rotates upward instead of downward. Also, the default
with no prefix argument is to rotate the top 3 elements.
For example, with ‘`10 20 30 40 50`’ on the stack,
`M-TAB` creates ‘`10 20 40 50 30`’,
`C-u 4 M-TAB` creates ‘`10 30 40 50 20`’,
`C-u - 2 M-TAB` creates ‘`30 40 50 10 20`’, and
`C-u 0 M-TAB` creates ‘`50 40 30 20 10`’.

A good way to view the operation of `TAB` and `M-TAB` is in
terms of moving a particular element to a new position in the stack.
With a positive argument n, `TAB` moves the top stack
element down to level n, making room for it by pulling all the
intervening stack elements toward the top. `M-TAB` moves the
element at level n up to the top. (Compare with `LFD`,
which copies instead of moving the element in level n.)

With a negative argument *-n*, `TAB` rotates the stack
to move the object in level n to the deepest place in the
stack, and the object in level *n+1* to the top. `M-TAB`
rotates the deepest stack element to be in level n, also
putting the top stack element in level *n+1*.

See [Selecting Sub-Formulas](#Selecting-Subformulas), for a way to apply these commands to
any portion of a vector or formula on the stack.

The command `C-x C-t` (`calc-transpose-lines`) will transpose
the stack object determined by the point with the stack object at the
next higher level. For example, with ‘`10 20 30 40 50`’ on the
stack and the point on the line containing ‘`30`’, `C-x C-t`
creates ‘`10 20 40 30 50`’. More generally, `C-x C-t` acts on
the stack objects determined by the current point (and mark) similar
to how the text-mode command `transpose-lines` acts on
lines. With argument n, `C-x C-t` will move the stack object
at the level above the current point and move it past N other objects;
for example, with ‘`10 20 30 40 50`’ on the stack and the point on
the line containing ‘`30`’, `C-u 2 C-x C-t` creates
‘`10 40 20 30 50`’. With an argument of 0, `C-x C-t` will switch
the stack objects at the levels determined by the point and the mark.

---

Next: [Trail Commands](#Trail-Commands), Previous: [Stack Manipulation Commands](#Stack-Manipulation), Up: [Stack and Trail Commands](#Stack-and-Trail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 5.2 Editing Stack Entries [¶](#Editing-Stack-Entries-1)

The `` ` `` (`calc-edit`) command creates a temporary buffer
(`*Calc Edit*`) for editing the top-of-stack value using regular
Emacs commands. Note that `` ` `` is a grave accent, not an apostrophe.
With a numeric prefix argument, it edits the specified number of stack
entries at once. (An argument of zero edits the entire stack; a
negative argument edits one specific stack entry.)

When you are done editing, press `C-c C-c` to finish and return
to Calc. The `RET` and `LFD` keys also work to finish most
sorts of editing, though in some cases Calc leaves `RET` with its
usual meaning (“insert a newline”) if it’s a situation where you
might want to insert new lines into the editing buffer.

When you finish editing, the Calculator parses the lines of text in
the `*Calc Edit*` buffer as numbers or formulas, replaces the
original stack elements in the original buffer with these new values,
then kills the `*Calc Edit*` buffer. The original Calculator buffer
continues to exist during editing, but for best results you should be
careful not to change it until you have finished the edit. You can
also cancel the edit by killing the buffer with `C-x k`.

The formula is normally reevaluated as it is put onto the stack.
For example, editing ‘`a + 2`’ to ‘`3 + 2`’ and pressing
`C-c C-c` will push 5 on the stack. If you use `LFD` to
finish, Calc will put the result on the stack without evaluating it.

If you give a prefix argument to `C-c C-c`,
Calc will not kill the `*Calc Edit*` buffer. You can switch
back to that buffer and continue editing if you wish. However, you
should understand that if you initiated the edit with `` ` ``, the
`C-c C-c` operation will be programmed to replace the top of the
stack with the new edited value, and it will do this even if you have
rearranged the stack in the meanwhile. This is not so much of a problem
with other editing commands, though, such as `s e`
(`calc-edit-variable`; see [Other Operations on Variables](#Operations-on-Variables)).

If the `calc-edit` command involves more than one stack entry,
each line of the `*Calc Edit*` buffer is interpreted as a
separate formula. Otherwise, the entire buffer is interpreted as
one formula, with line breaks ignored. (You can use `C-o` or
`C-q C-j` to insert a newline in the buffer without pressing `RET`.)

The `` ` `` key also works during numeric or algebraic entry. The
text entered so far is moved to the `*Calc Edit*` buffer for
more extensive editing than is convenient in the minibuffer.

---

Next: [Keep Arguments](#Keep-Arguments), Previous: [Editing Stack Entries](#Editing-Stack-Entries), Up: [Stack and Trail Commands](#Stack-and-Trail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 5.3 Trail Commands [¶](#Trail-Commands-1)

The commands for manipulating the Calc Trail buffer are two-key sequences
beginning with the `t` prefix.

The `t d` (`calc-trail-display`) command turns display of the
trail on and off. Normally the trail display is toggled on if it was off,
off if it was on. With a numeric prefix of zero, this command always
turns the trail off; with a prefix of one, it always turns the trail on.
The other trail-manipulation commands described here automatically turn
the trail on. Note that when the trail is off values are still recorded
there; they are simply not displayed. To set Emacs to turn the trail
off by default, type `t d` and then save the mode settings with
`m m` (`calc-save-modes`).

The `t i` (`calc-trail-in`) and `t o`
(`calc-trail-out`) commands switch the cursor into and out of the
Calc Trail window. In practice they are rarely used, since the commands
shown below are a more convenient way to move around in the
trail, and they work “by remote control” when the cursor is still
in the Calculator window.

There is a *trail pointer* which selects some entry of the trail at
any given time. The trail pointer looks like a ‘`>`’ symbol right
before the selected number. The following commands operate on the
trail pointer in various ways.

The `t y` (`calc-trail-yank`) command reads the selected value in
the trail and pushes it onto the Calculator stack. It allows you to
reuse any previously computed value without retyping. With a numeric
prefix argument n, it yanks the value n lines above the current
trail pointer.

The `t <` (`calc-trail-scroll-left`) and `t >`
(`calc-trail-scroll-right`) commands horizontally scroll the trail
window left or right by one half of its width.

The `t n` (`calc-trail-next`) and `t p`
(`calc-trail-previous)` commands move the trail pointer down or up
one line. The `t f` (`calc-trail-forward`) and `t b`
(`calc-trail-backward`) commands move the trail pointer down or up
one screenful at a time. All of these commands accept numeric prefix
arguments to move several lines or screenfuls at a time.

The `t [` (`calc-trail-first`) and `t ]`
(`calc-trail-last`) commands move the trail pointer to the first or
last line of the trail. The `t h` (`calc-trail-here`) command
moves the trail pointer to the cursor position; unlike the other trail
commands, `t h` works only when Calc Trail is the selected window.

The `t s` (`calc-trail-isearch-forward`) and `t r`
(`calc-trail-isearch-backward`) commands perform an incremental
search forward or backward through the trail. You can press `RET`
to terminate the search; the trail pointer moves to the current line.
If you cancel the search with `C-g`, the trail pointer stays where
it was when the search began.

The `t m` (`calc-trail-marker`) command allows you to enter a
line of text of your own choosing into the trail. The text is inserted
after the line containing the trail pointer; this usually means it is
added to the end of the trail. Trail markers are useful mainly as the
targets for later incremental searches in the trail.

The `t k` (`calc-trail-kill`) command removes the selected line
from the trail. The line is saved in the Emacs kill ring suitable for
yanking into another buffer, but it is not easy to yank the text back
into the trail buffer. With a numeric prefix argument, this command
kills the n lines below or above the selected one.

The `t .` (`calc-full-trail-vectors`) command is described
elsewhere; see [Vector and Matrix Display Formats](#Vector-and-Matrix-Formats).

---

Previous: [Trail Commands](#Trail-Commands), Up: [Stack and Trail Commands](#Stack-and-Trail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 5.4 Keep Arguments [¶](#Keep-Arguments-1)

The `K` (`calc-keep-args`) command acts like a prefix for
the following command. It prevents that command from removing its
arguments from the stack. For example, after `2 RET 3 +`,
the stack contains the sole number 5, but after `2 RET 3 K +`,
the stack contains the arguments and the result: ‘`2 3 5`’.

With the exception of keyboard macros, this works for all commands that
take arguments off the stack. (To avoid potentially unpleasant behavior,
a `K` prefix before a keyboard macro will be ignored. A `K`
prefix called *within* the keyboard macro will still take effect.)
As another example, `K a s` simplifies a formula, pushing the
simplified version of the formula onto the stack after the original
formula (rather than replacing the original formula). Note that you
could get the same effect by typing `RET a s`, copying the
formula and then simplifying the copy. One difference is that for a very
large formula the time taken to format the intermediate copy in
`RET a s` could be noticeable; `K a s` would avoid this
extra work.

Even stack manipulation commands are affected. `TAB` works by
popping two values and pushing them back in the opposite order,
so `2 RET 3 K TAB` produces ‘`2 3 3 2`’.

A few Calc commands provide other ways of doing the same thing.
For example, `' sin($)` replaces the number on the stack with
its sine using algebraic entry; to push the sine and keep the
original argument you could use either `' sin($1)` or
`K ' sin($)`. See [Algebraic Entry](#Algebraic-Entry). Also, the `s s`
command is effectively the same as `K s t`. See [Storing Variables](#Storing-Variables).

If you execute a command and then decide you really wanted to keep
the argument, you can press `M-RET` (`calc-last-args`).
This command pushes the last arguments that were popped by any command
onto the stack. Note that the order of things on the stack will be
different than with `K`: `2 RET 3 + M-RET` leaves
‘`5 2 3`’ on the stack instead of ‘`2 3 5`’. See [Undoing Mistakes](#Undo).

---

Next: [Arithmetic Functions](#Arithmetic), Previous: [Stack and Trail Commands](#Stack-and-Trail), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 6 Mode Settings [¶](#Mode-Settings-1)

This chapter describes commands that set modes in the Calculator.
They do not affect the contents of the stack, although they may change
the *appearance* or *interpretation* of the stack’s contents.

* [General Mode Commands](#General-Mode-Commands)
* [Precision](#Precision)
* [Inverse and Hyperbolic Flags](#Inverse-and-Hyperbolic)
* [Calculation Modes](#Calculation-Modes)
* [Simplification Modes](#Simplification-Modes)
* [Declarations](#Declarations)
* [Display Modes](#Display-Modes)
* [Language Modes](#Language-Modes)
* [The `Modes` Variable](#Modes-Variable)
* [The Calc Mode Line](#Calc-Mode-Line)

---

Next: [Precision](#Precision), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 6.1 General Mode Commands [¶](#General-Mode-Commands-1)

You can save all of the current mode settings in your Calc init file
(the file given by the variable `calc-settings-file`, typically
`~/.emacs.d/calc.el`) with the `m m` (`calc-save-modes`)
command. This will cause Emacs to reestablish these modes each time
it starts up. The modes saved in the file include everything
controlled by the `m` and `d` prefix keys, the current
precision and binary word size, whether or not the trail is displayed,
the current height of the Calc window, and more. The current
interface (used when you type `C-x * *`) is also saved. If there
were already saved mode settings in the file, they are replaced.
Otherwise, the new mode information is appended to the end of the
file.

The `m R` (`calc-mode-record-mode`) command tells Calc to
record all the mode settings (as if by pressing `m m`) every
time a mode setting changes. If the modes are saved this way, then this
“automatic mode recording” mode is also saved.
Type `m R` again to disable this method of recording the mode
settings. To turn it off permanently, the `m m` command will also be
necessary. (If Embedded mode is enabled, other options for recording
the modes are available; see [Mode Settings in Embedded Mode](#Mode-Settings-in-Embedded-Mode).)

The `m F` (`calc-settings-file-name`) command allows you to
choose a different file than the current value of `calc-settings-file`
for `m m`, `Z P`, and similar commands to save permanent information.
You are prompted for a file name. All Calc modes are then reset to
their default values, then settings from the file you named are loaded
if this file exists, and this file becomes the one that Calc will
use in the future for commands like `m m`. The default settings
file name is `~/.emacs.d/calc.el`. You can see the current file name by
giving a blank response to the `m F` prompt. See also the
discussion of the `calc-settings-file` variable; see [Customizing Calc](#Customizing-Calc).

If the file name you give is your user init file (typically
`~/.emacs`), `m F` will not automatically load the new file. This
is because your user init file may contain other things you don’t want
to reread. You can give
a numeric prefix argument of 1 to `m F` to force it to read the
file no matter what. Conversely, an argument of *-1* tells
`m F` *not* to read the new file. An argument of 2 or *-2*
tells `m F` not to reset the modes to their defaults beforehand,
which is useful if you intend your new file to have a variant of the
modes present in the file you were using before.

The `m x` (`calc-always-load-extensions`) command enables a mode
in which the first use of Calc loads the entire program, including all
extensions modules. Otherwise, the extensions modules will not be loaded
until the various advanced Calc features are used. Since this mode only
has effect when Calc is first loaded, `m x` is usually followed by
`m m` to make the mode-setting permanent. To load all of Calc just
once, rather than always in the future, you can press `C-x * L`.

The `m S` (`calc-shift-prefix`) command enables a mode in which
all of Calc’s letter prefix keys may be typed shifted as well as unshifted.
If you are typing, say, `a S` (`calc-solve-for`) quite often
you might find it easier to turn this mode on so that you can type
`A S` instead. When this mode is enabled, the commands that used to
be on those single shifted letters (e.g., `A` (`calc-abs`)) can
now be invoked by pressing the shifted letter twice: `A A`. Note
that the `v` prefix key always works both shifted and unshifted, and
the `z` and `Z` prefix keys are always distinct. Also, the `h`
prefix is not affected by this mode. Press `m S` again to disable
shifted-prefix mode.

---

Next: [Inverse and Hyperbolic Flags](#Inverse-and-Hyperbolic), Previous: [General Mode Commands](#General-Mode-Commands), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 6.2 Precision [¶](#Precision-1)

The `p` (`calc-precision`) command controls the precision to
which floating-point calculations are carried. The precision must be
at least 3 digits and may be arbitrarily high, within the limits of
memory and time. This affects only floats: Integer and rational
calculations are always carried out with as many digits as necessary.

The `p` key prompts for the current precision. If you wish you
can instead give the precision as a numeric prefix argument.

Many internal calculations are carried to one or two digits higher
precision than normal. Results are rounded down afterward to the
current precision. Unless a special display mode has been selected,
floats are always displayed with their full stored precision, i.e.,
what you see is what you get. Reducing the current precision does not
round values already on the stack, but those values will be rounded
down before being used in any calculation. The `c 0` through
`c 9` commands (see [Conversions](#Conversions)) can be used to round an
existing value to a new precision.

It is important to distinguish the concepts of *precision* and
*accuracy*. In the normal usage of these words, the number
123.4567 has a precision of 7 digits but an accuracy of 4 digits.
The precision is the total number of digits not counting leading
or trailing zeros (regardless of the position of the decimal point).
The accuracy is simply the number of digits after the decimal point
(again not counting trailing zeros). In Calc you control the precision,
not the accuracy of computations. If you were to set the accuracy
instead, then calculations like ‘`exp(100)`’ would generate many
more digits than you would typically need, while ‘`exp(-100)`’ would
probably round to zero! In Calc, both these computations give you
exactly 12 (or the requested number of) significant digits.

The only Calc features that deal with accuracy instead of precision
are fixed-point display mode for floats (`d f`; see [Float Formats](#Float-Formats)),
and the rounding functions like `floor` and `round`
(see [Integer Truncation](#Integer-Truncation)). Also, `c 0` through `c 9`
deal with both precision and accuracy depending on the magnitudes
of the numbers involved.

If you need to work with a particular fixed accuracy (say, dollars and
cents with two digits after the decimal point), one solution is to work
with integers and an “implied” decimal point. For example, $8.99
divided by 6 would be entered `899 RET 6 /`, yielding 149.833
(actually $1.49833 with our implied decimal point); pressing `R`
would round this to 150 cents, i.e., $1.50.

See [Floats](#Floats), for still more on floating-point precision and related
issues.

---

Next: [Calculation Modes](#Calculation-Modes), Previous: [Precision](#Precision), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 6.3 Inverse and Hyperbolic Flags [¶](#Inverse-and-Hyperbolic-Flags)

There is no single-key equivalent to the `calc-arcsin` function.
Instead, you must first press `I` (`calc-inverse`) to set
the *Inverse Flag*, then press `S` (`calc-sin`).
The `I` key actually toggles the Inverse Flag. When this flag
is set, the word ‘`Inv`’ appears in the mode line.

Likewise, the `H` key (`calc-hyperbolic`) sets or clears the
Hyperbolic Flag, which transforms `calc-sin` into `calc-sinh`.
If both of these flags are set at once, the effect will be
`calc-arcsinh`. (The Hyperbolic flag is also used by some
non-trigonometric commands; for example `H L` computes a base-10,
instead of base-*e*, logarithm.)

Command names like `calc-arcsin` are provided for completeness, and
may be executed with `x` or `M-x`. Their effect is simply to
toggle the Inverse and/or Hyperbolic flags and then execute the
corresponding base command (`calc-sin` in this case).

The `O` key (`calc-option`) sets another flag, the
*Option Flag*, which also can alter the subsequent Calc command in
various ways.

The Inverse, Hyperbolic and Option flags apply only to the next
Calculator command, after which they are automatically cleared. (They
are also cleared if the next keystroke is not a Calc command.) Digits
you type after `I`, `H` or `O` (or `K`) are treated as
prefix arguments for the next command, not as numeric entries. The
same is true of `C-u`, but not of the minus sign (`K -` means
to subtract and keep arguments).

Another Calc prefix flag, `K` (keep-arguments), is discussed
elsewhere. See [Keep Arguments](#Keep-Arguments).

---

Next: [Simplification Modes](#Simplification-Modes), Previous: [Inverse and Hyperbolic Flags](#Inverse-and-Hyperbolic), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 6.4 Calculation Modes [¶](#Calculation-Modes-1)

The commands in this section are two-key sequences beginning with
the `m` prefix. (That’s the letter `m`, not the `META` key.)
The ‘`m a`’ (`calc-algebraic-mode`) command is described elsewhere
(see [Algebraic Entry](#Algebraic-Entry)).

* [Angular Modes](#Angular-Modes)
* [Polar Mode](#Polar-Mode)
* [Fraction Mode](#Fraction-Mode)
* [Infinite Mode](#Infinite-Mode)
* [Symbolic Mode](#Symbolic-Mode)
* [Matrix and Scalar Modes](#Matrix-Mode)
* [Automatic Recomputation](#Automatic-Recomputation)
* [Working Messages](#Working-Message)

---

Next: [Polar Mode](#Polar-Mode), Up: [Calculation Modes](#Calculation-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.4.1 Angular Modes [¶](#Angular-Modes-1)

The Calculator supports three notations for angles: radians, degrees,
and degrees-minutes-seconds. When a number is presented to a function
like `sin` that requires an angle, the current angular mode is
used to interpret the number as either radians or degrees. If an HMS
form is presented to `sin`, it is always interpreted as
degrees-minutes-seconds.

Functions that compute angles produce a number in radians, a number in
degrees, or an HMS form depending on the current angular mode. If the
result is a complex number and the current mode is HMS, the number is
instead expressed in degrees. (Complex-number calculations would
normally be done in Radians mode, though. Complex numbers are converted
to degrees by calculating the complex result in radians and then
multiplying by 180 over ‘`pi`’.)

The `m r` (`calc-radians-mode`), `m d` (`calc-degrees-mode`),
and `m h` (`calc-hms-mode`) commands control the angular mode.
The current angular mode is displayed on the Emacs mode line.
The default angular mode is Degrees.

---

Next: [Fraction Mode](#Fraction-Mode), Previous: [Angular Modes](#Angular-Modes), Up: [Calculation Modes](#Calculation-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.4.2 Polar Mode [¶](#Polar-Mode-1)

The Calculator normally “prefers” rectangular complex numbers in the
sense that rectangular form is used when the proper form can not be
decided from the input. This might happen by multiplying a rectangular
number by a polar one, by taking the square root of a negative real
number, or by entering `( 2 SPC 3 )`.

The `m p` (`calc-polar-mode`) command toggles complex-number
preference between rectangular and polar forms. In Polar mode, all
of the above example situations would produce polar complex numbers.

---

Next: [Infinite Mode](#Infinite-Mode), Previous: [Polar Mode](#Polar-Mode), Up: [Calculation Modes](#Calculation-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.4.3 Fraction Mode [¶](#Fraction-Mode-1)

Division of two integers normally yields a floating-point number if the
result cannot be expressed as an integer. In some cases you would
rather get an exact fractional answer. One way to accomplish this is
to use the `:` (`calc-fdiv`) [`fdiv`] command, which
divides the two integers on the top of the stack to produce a fraction:
`6 RET 4 :` produces ‘`3:2`’ even though
`6 RET 4 /` produces ‘`1.5`’.

To set the Calculator to produce fractional results for normal integer
divisions, use the `m f` (`calc-frac-mode`) command.
For example, ‘`8/4`’ produces ‘`2`’ in either mode,
but ‘`6/4`’ produces ‘`3:2`’ in Fraction mode, ‘`1.5`’ in
Float mode.

At any time you can use `c f` (`calc-float`) to convert a
fraction to a float, or `c F` (`calc-fraction`) to convert a
float to a fraction. See [Conversions](#Conversions).

---

Next: [Symbolic Mode](#Symbolic-Mode), Previous: [Fraction Mode](#Fraction-Mode), Up: [Calculation Modes](#Calculation-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.4.4 Infinite Mode [¶](#Infinite-Mode-1)

The Calculator normally treats results like ‘`1 / 0`’ as errors;
formulas like this are left in unsimplified form. But Calc can be
put into a mode where such calculations instead produce “infinite”
results.

The `m i` (`calc-infinite-mode`) command turns this mode
on and off. When the mode is off, infinities do not arise except
in calculations that already had infinities as inputs. (One exception
is that infinite open intervals like ‘`[0 .. inf)`’ can be
generated; however, intervals closed at infinity (‘`[0 .. inf]`’)
will not be generated when Infinite mode is off.)

With Infinite mode turned on, ‘`1 / 0`’ will generate `uinf`,
an undirected infinity. See [Infinities](#Infinities), for a discussion of the
difference between `inf` and `uinf`. Also, ‘`0 / 0`’
evaluates to `nan`, the “indeterminate” symbol. Various other
functions can also return infinities in this mode; for example,
‘`ln(0) = -inf`’, and ‘`gamma(-7) = uinf`’. Once again,
note that ‘`exp(inf) = inf`’ regardless of Infinite mode because
this calculation has infinity as an input.

The `m i` command with a numeric prefix argument of zero,
i.e., `C-u 0 m i`, turns on a Positive Infinite mode in
which zero is treated as positive instead of being directionless.
Thus, ‘`1 / 0 = inf`’ and ‘`-1 / 0 = -inf`’ in this mode.
Note that zero never actually has a sign in Calc; there are no
separate representations for *+0* and *-0*. Positive
Infinite mode merely changes the interpretation given to the
single symbol, ‘`0`’. One consequence of this is that, while
you might expect ‘`1 / -0 = -inf`’, actually ‘`1 / -0`’
is equivalent to ‘`1 / 0`’, which is equal to positive `inf`.

---

Next: [Matrix and Scalar Modes](#Matrix-Mode), Previous: [Infinite Mode](#Infinite-Mode), Up: [Calculation Modes](#Calculation-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.4.5 Symbolic Mode [¶](#Symbolic-Mode-1)

Calculations are normally performed numerically wherever possible.
For example, the `calc-sqrt` command, or `sqrt` function in an
algebraic expression, produces a numeric answer if the argument is a
number or a symbolic expression if the argument is an expression:
`2 Q` pushes 1.4142 but `' x+1 RET Q` pushes ‘`sqrt(x+1)`’.

In *Symbolic mode*, controlled by the `m s` (`calc-symbolic-mode`)
command, functions which would produce inexact, irrational results are
left in symbolic form. Thus `16 Q` pushes 4, but `2 Q` pushes
‘`sqrt(2)`’.

The shift-`N` (`calc-eval-num`) command evaluates numerically
the expression at the top of the stack, by temporarily disabling
`calc-symbolic-mode` and executing `=` (`calc-evaluate`).
Given a numeric prefix argument, it also
sets the floating-point precision to the specified value for the duration
of the command.

To evaluate a formula numerically without expanding the variables it
contains, you can use the key sequence `m s a v m s` (this uses
`calc-alg-evaluate`, which resimplifies but doesn’t evaluate
variables.)

---

Next: [Automatic Recomputation](#Automatic-Recomputation), Previous: [Symbolic Mode](#Symbolic-Mode), Up: [Calculation Modes](#Calculation-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.4.6 Matrix and Scalar Modes [¶](#Matrix-and-Scalar-Modes)

Calc sometimes makes assumptions during algebraic manipulation that
are awkward or incorrect when vectors and matrices are involved.
Calc has two modes, *Matrix mode* and *Scalar mode*, which
modify its behavior around vectors in useful ways.

Press `m v` (`calc-matrix-mode`) once to enter Matrix mode.
In this mode, all objects are assumed to be matrices unless provably
otherwise. One major effect is that Calc will no longer consider
multiplication to be commutative. (Recall that in matrix arithmetic,
‘`A*B`’ is not the same as ‘`B*A`’.) This assumption affects
rewrite rules and algebraic simplification. Another effect of this
mode is that calculations that would normally produce constants like
0 and 1 (e.g., ‘`a - a`’ and ‘`a / a`’, respectively) will now
produce function calls that represent “generic” zero or identity
matrices: ‘`idn(0)`’, ‘`idn(1)`’. The `idn` function
‘`idn(a,n)`’ returns a times an nxn
identity matrix; if n is omitted, it doesn’t know what
dimension to use and so the `idn` call remains in symbolic
form. However, if this generic identity matrix is later combined
with a matrix whose size is known, it will be converted into
a true identity matrix of the appropriate size. On the other hand,
if it is combined with a scalar (as in ‘`idn(1) + 2`’), Calc
will assume it really was a scalar after all and produce, e.g., 3.

Press `m v` a second time to get Scalar mode. Here, objects are
assumed *not* to be vectors or matrices unless provably so.
For example, normally adding a variable to a vector, as in
‘`[x, y, z] + a`’, will leave the sum in symbolic form because
as far as Calc knows, ‘`a`’ could represent either a number or
another 3-vector. In Scalar mode, ‘`a`’ is assumed to be a
non-vector, and the addition is evaluated to ‘`[x+a, y+a, z+a]`’.

Press `m v` a third time to return to the normal mode of operation.

If you press `m v` with a numeric prefix argument n, you
get a special “dimensioned” Matrix mode in which matrices of
unknown size are assumed to be nxn square matrices.
Then, the function call ‘`idn(1)`’ will expand into an actual
matrix rather than representing a “generic” matrix. Simply typing
`C-u m v` will get you a square Matrix mode, in which matrices of
unknown size are assumed to be square matrices of unspecified size.

Of course these modes are approximations to the true state of
affairs, which is probably that some quantities will be matrices
and others will be scalars. One solution is to “declare”
certain variables or functions to be scalar-valued.
See [Declarations](#Declarations), to see how to make declarations in Calc.

There is nothing stopping you from declaring a variable to be
scalar and then storing a matrix in it; however, if you do, the
results you get from Calc may not be valid. Suppose you let Calc
get the result ‘`[x+a, y+a, z+a]`’ shown above, and then stored
‘`[1, 2, 3]`’ in ‘`a`’. The result would not be the same as
for ‘`[x, y, z] + [1, 2, 3]`’, but that’s because you have broken
your earlier promise to Calc that ‘`a`’ would be scalar.

Another way to mix scalars and matrices is to use selections
(see [Selecting Sub-Formulas](#Selecting-Subformulas)). Use Matrix mode when operating on
your formula normally; then, to apply Scalar mode to a certain part
of the formula without affecting the rest just select that part,
change into Scalar mode and press `=` to resimplify the part
under this mode, then change back to Matrix mode before deselecting.

---

Next: [Working Messages](#Working-Message), Previous: [Matrix and Scalar Modes](#Matrix-Mode), Up: [Calculation Modes](#Calculation-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.4.7 Automatic Recomputation [¶](#Automatic-Recomputation-1)

The *evaluates-to* operator, ‘`=>`’, has the special
property that any ‘`=>`’ formulas on the stack are recomputed
whenever variable values or mode settings that might affect them
are changed. See [The Evaluates-To Operator](#Evaluates_002dTo-Operator).

The `m C` (`calc-auto-recompute`) command turns this
automatic recomputation on and off. If you turn it off, Calc will
not update ‘`=>`’ operators on the stack (nor those in the
attached Embedded mode buffer, if there is one). They will not
be updated unless you explicitly do so by pressing `=` or until
you press `m C` to turn recomputation back on. (While automatic
recomputation is off, you can think of `m C m C` as a command
to update all ‘`=>`’ operators while leaving recomputation off.)

To update ‘`=>`’ operators in an Embedded buffer while
automatic recomputation is off, use `C-x * u`.
See [Embedded Mode](#Embedded-Mode).

---

Previous: [Automatic Recomputation](#Automatic-Recomputation), Up: [Calculation Modes](#Calculation-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.4.8 Working Messages [¶](#Working-Messages)

Since the Calculator is written entirely in Emacs Lisp, which is not
designed for heavy numerical work, many operations are quite slow.
The Calculator normally displays the message ‘`Working...`’ in the
echo area during any command that may be slow. In addition, iterative
operations such as square roots and trigonometric functions display the
intermediate result at each step. Both of these types of messages can
be disabled if you find them distracting.

Type `m w` (`calc-working`) with a numeric prefix of 0 to
disable all “working” messages. Use a numeric prefix of 1 to enable
only the plain ‘`Working...`’ message. Use a numeric prefix of 2 to
see intermediate results as well. With no numeric prefix this displays
the current mode.

While it may seem that the “working” messages will slow Calc down
considerably, experiments have shown that their impact is actually
quite small. But if your terminal is slow you may find that it helps
to turn the messages off.

---

Next: [Declarations](#Declarations), Previous: [Calculation Modes](#Calculation-Modes), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 6.5 Simplification Modes [¶](#Simplification-Modes-1)

The current *simplification mode* controls how numbers and formulas
are “normalized” when being taken from or pushed onto the stack.
Some normalizations are unavoidable, such as rounding floating-point
results to the current precision, and reducing fractions to simplest
form. Others, such as simplifying a formula like ‘`a+a`’ (or ‘`2+3`’),
are done automatically but can be turned off when necessary.

When you press a key like `+` when ‘`2`’ and ‘`3`’ are on the
stack, Calc pops these numbers, normalizes them, creates the formula
‘`2+3`’, normalizes it, and pushes the result. Of course the standard
rules for normalizing ‘`2+3`’ will produce the result ‘`5`’.

Simplification mode commands consist of the lower-case `m` prefix key
followed by a shifted letter.

The `m O` (`calc-no-simplify-mode`) command turns off all optional
simplifications. These would leave a formula like ‘`2+3`’ alone. In
fact, nothing except simple numbers are ever affected by normalization
in this mode. Explicit simplification commands, such as `=` or
`a s`, can still be given to simplify any formulas.
See [Programming with Formulas](#Algebraic-Definitions), for a sample use of
No-Simplification mode.

The `m N` (`calc-num-simplify-mode`) command turns off simplification
of any formulas except those for which all arguments are constants. For
example, ‘`1+2`’ is simplified to ‘`3`’, and ‘`a+(2-2)`’ is
simplified to ‘`a+0`’ but no further, since one argument of the sum
is not a constant. Unfortunately, ‘`(a+2)-2`’ is *not* simplified
because the top-level ‘`-`’ operator’s arguments are not both
constant numbers (one of them is the formula ‘`a+2`’).
A constant is a number or other numeric object (such as a constant
error form or modulo form), or a vector all of whose
elements are constant.

The `m I` (`calc-basic-simplify-mode`) command does some basic
simplifications for all formulas. This includes many easy and
fast algebraic simplifications such as ‘`a+0`’ to ‘`a`’, and
‘`a + 2 a`’ to ‘`3 a`’, as well as evaluating functions like
‘`deriv(x^2, x)`’ to ‘`2 x`’.

The `m B` (`calc-bin-simplify-mode`) mode applies the basic
simplifications to a result and then, if the result is an integer,
uses the `b c` (`calc-clip`) command to clip the integer according
to the current binary word size. See [Binary Number Functions](#Binary-Functions). Real numbers
are rounded to the nearest integer and then clipped; other kinds of
results (after the basic simplifications) are left alone.

The `m A` (`calc-alg-simplify-mode`) mode does standard
algebraic simplifications. See [Algebraic Simplifications](#Algebraic-Simplifications).

The `m E` (`calc-ext-simplify-mode`) mode does “extended”, or
“unsafe”, algebraic simplification. See [“Unsafe” Simplifications](#Unsafe-Simplifications).

The `m U` (`calc-units-simplify-mode`) mode does units
simplification. See [Simplification of Units](#Simplification-of-Units). These include the
algebraic simplifications, plus variable names which
are identifiable as unit names (like ‘`mm`’ for “millimeters”)
are simplified with their unit definitions in mind.

A common technique is to set the simplification mode down to the lowest
amount of simplification you will allow to be applied automatically, then
use manual commands like `a s` and `c c` (`calc-clean`) to
perform higher types of simplifications on demand.

---

Next: [Display Modes](#Display-Modes), Previous: [Simplification Modes](#Simplification-Modes), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
