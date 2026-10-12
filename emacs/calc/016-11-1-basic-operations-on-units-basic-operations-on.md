### 11.1 Basic Operations on Units [¶](#Basic-Operations-on-Units-1)

A *units expression* is a formula which is basically a number
multiplied and/or divided by one or more *unit names*, which may
optionally be raised to integer powers. Actually, the value part need not
be a number; any product or quotient involving unit names is a units
expression. Many of the units commands will also accept any formula,
where the command applies to all units expressions which appear in the
formula.

A unit name is a variable whose name appears in the *unit table*,
or a variable whose name is a prefix character like ‘`k`’ (for “kilo”)
or ‘`u`’ (for “micro”) followed by a name in the unit table.
A substantial table of built-in units is provided with Calc;
see [Predefined Units](#Predefined-Units). You can also define your own unit names;
see [User-Defined Units](#User_002dDefined-Units).

Note that if the value part of a units expression is exactly ‘`1`’,
it will be removed by the Calculator’s automatic algebra routines: The
formula ‘`1 mm`’ is “simplified” to ‘`mm`’. This is only a
display anomaly, however; ‘`mm`’ will work just fine as a
representation of one millimeter.

You may find that Algebraic mode (see [Algebraic Entry](#Algebraic-Entry)) makes working
with units expressions easier. Otherwise, you will have to remember
to hit the apostrophe key every time you wish to enter units.

The `u s` (`calc-simplify-units`) [`usimplify`] command
simplifies a units
expression. It uses Calc’s algebraic simplifications to simplify the
expression first as a regular algebraic formula; it then looks for
features that can be further simplified by converting one object’s units
to be compatible with another’s. For example, ‘`5 m + 23 mm`’ will
simplify to ‘`5.023 m`’. When different but compatible units are
added, the righthand term’s units are converted to match those of the
lefthand term. See [Simplification Modes](#Simplification-Modes), for a way to have this done
automatically at all times.

Units simplification also handles quotients of two units with the same
dimensionality, as in ‘`2 in s/L cm`’ to ‘`5.08 s/L`’; fractional
powers of unit expressions, as in ‘`sqrt(9 mm^2)`’ to ‘`3 mm`’ and
‘`sqrt(9 acre)`’ to a quantity in meters; and `floor`,
`ceil`, `round`, `rounde`, `roundu`, `trunc`,
`float`, `frac`, `abs`, and `clean`
applied to units expressions, in which case
the operation in question is applied only to the numeric part of the
expression. Finally, trigonometric functions of quantities with units
of angle are evaluated, regardless of the current angular mode.

The `u c` (`calc-convert-units`) command converts a units
expression to new, compatible units. For example, given the units
expression ‘`55 mph`’, typing `u c m/s RET` produces
‘`24.5872 m/s`’. If you have previously converted a units expression
with the same type of units (in this case, distance over time), you will
be offered the previous choice of new units as a default. Continuing
the above example, entering the units expression ‘`100 km/hr`’ and
typing `u c RET` (without specifying new units) produces
‘`27.7777777778 m/s`’.

The `u c` command treats temperature units (like ‘`degC`’ and
‘`K`’) as relative temperatures. For example, `u c` converts
‘`10 degC`’ to ‘`18 degF`’: A change of 10 degrees Celsius
corresponds to a change of 18 degrees Fahrenheit. To convert absolute
temperatures, you can use the `u t`
(`calc-convert-temperature`) command. The value on the stack
must be a simple units expression with units of temperature only.
This command would convert ‘`10 degC`’ to ‘`50 degF`’, the
equivalent temperature on the Fahrenheit scale.

While many of Calc’s conversion factors are exact, some are necessarily
approximate. If Calc is in fraction mode (see [Fraction Mode](#Fraction-Mode)), then
unit conversions will try to give exact, rational conversions, but it
isn’t always possible. Given ‘`55 mph`’ in fraction mode, typing
`u c m/s RET` produces ‘`15367:625 m/s`’, for example,
while typing `u c au/yr RET` produces
‘`5.18665819999e-3 au/yr`’.

If the units you request are inconsistent with the original units, the
number will be converted into your units times whatever “remainder”
units are left over. For example, converting ‘`55 mph`’ into acres
produces ‘`6.08e-3 acre / (m s)`’. Remainder units are expressed in terms of
“fundamental” units like ‘`m`’ and ‘`s`’, regardless of the
input units.

If you intend that your new units be consistent with the original
units, the `u n` (`calc-convert-exact-units`) command will
check the units before the conversion. For example, to change
‘`mi/hr`’ to ‘`km/hr`’, you could type `u c km RET`,
but `u n km RET` would signal an error.
You would need to type `u n km/hr RET`.

One special exception is that if you specify a single unit name, and
a compatible unit appears somewhere in the units expression, then
that compatible unit will be converted to the new unit and the
remaining units in the expression will be left alone. For example,
given the input ‘`980 cm/s^2`’, the command `u c ms` will
change the ‘`s`’ to ‘`ms`’ to get ‘`9.8e-4 cm/ms^2`’.
The “remainder unit” ‘`cm`’ is left alone rather than being
changed to the base unit ‘`m`’.

You can use explicit unit conversion instead of the `u s` command
to gain more control over the units of the result of an expression.
For example, given ‘`5 m + 23 mm`’, you can type `u c m` or
`u c mm` to express the result in either meters or millimeters.
(For that matter, you could type `u c fath` to express the result
in fathoms, if you preferred!)

In place of a specific set of units, you can also enter one of the
units system names `si`, `mks` (equivalent), or `cgs`.
For example, `u c si RET` converts the expression into
International System of Units (SI) base units. Also, `u c base`
converts to Calc’s base units, which are the same as `si` units
except that `base` uses ‘`g`’ as the fundamental unit of mass
whereas `si` uses ‘`kg`’.

The `u c` command also accepts *composite units*, which
are expressed as the sum of several compatible unit names. For
example, converting ‘`30.5 in`’ to units ‘`mi+ft+in`’ (miles,
feet, and inches) produces ‘`2 ft + 6.5 in`’. Calc first
sorts the unit names into order of decreasing relative size.
It then accounts for as much of the input quantity as it can
using an integer number times the largest unit, then moves on
to the next smaller unit, and so on. Only the smallest unit
may have a non-integer amount attached in the result. A few
standard unit names exist for common combinations, such as
`mfi` for ‘`mi+ft+in`’, and `tpo` for ‘`ton+lb+oz`’.
Composite units are expanded as if by `a x`, so that
‘`(ft+in)/hr`’ is first converted to ‘`ft/hr+in/hr`’.

If the value on the stack does not contain any units, `u c` will
prompt first for the old units which this value should be considered
to have, then for the new units. (If the value on the stack can be
simplified so that it doesn’t contain any units, like ‘`ft/in`’ can
be simplified to 12, then `u c` will still prompt for both old
units and new units. Assuming the old and new units you give are
consistent with each other, the result also will not contain any
units. For example, `u c cm RET in RET` converts
the number 2 on the stack to 5.08.

The `u b` (`calc-base-units`) command is shorthand for
`u c base`; it converts the units expression on the top of the
stack into `base` units. If `u s` does not simplify a
units expression as far as you would like, try `u b`.

Like the `u c` command, the `u b` command treats temperature
units as relative temperatures.

The `u r` (`calc-remove-units`) command removes units from the
formula at the top of the stack. The `u x`
(`calc-extract-units`) command extracts only the units portion of a
formula. These commands essentially replace every term of the formula
that does or doesn’t (respectively) look like a unit name by the
constant 1, then resimplify the formula.

The `u a` (`calc-autorange-units`) command turns on and off a
mode in which unit prefixes like `k` (“kilo”) are automatically
applied to keep the numeric part of a units expression in a reasonable
range. This mode affects `u s` and all units conversion commands
except `u b`. For example, with autoranging on, ‘`12345 Hz`’
will be simplified to ‘`12.345 kHz`’. Autoranging is useful for
some kinds of units (like `Hz` and `m`), but is probably
undesirable for non-metric units like `ft` and `tbsp`.
(Composite units are more appropriate for those; see above.)

Autoranging always applies the prefix to the leftmost unit name.
Calc chooses the largest prefix that causes the number to be greater
than or equal to 1.0. Thus an increasing sequence of adjusted times
would be ‘`1 ms, 10 ms, 100 ms, 1 s, 10 s, 100 s, 1 ks`’.
Generally the rule of thumb is that the number will be adjusted
to be in the interval ‘`[1 .. 1000)`’, although there are several
exceptions to this rule. First, if the unit has a power then this
is not possible; ‘`0.1 s^2`’ simplifies to ‘`100000 ms^2`’.
Second, the “centi-” prefix is allowed to form `cm` (centimeters),
but will not apply to other units. The “deci-,” “deka-,” and
“hecto-” prefixes are never used. Thus the allowable interval is
‘`[1 .. 10)`’ for millimeters and ‘`[1 .. 100)`’ for centimeters.
Finally, a prefix will not be added to a unit if the resulting name
is also the actual name of another unit; ‘`1e-15 t`’ would normally
be considered a “femto-ton,” but it is written as ‘`1000 at`’
(1000 atto-tons) instead because `ft` would be confused with feet.

---

Next: [Predefined Units](#Predefined-Units), Previous: [Basic Operations on Units](#Basic-Operations-on-Units), Up: [Operating on Units](#Units)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 11.2 The Units Table [¶](#The-Units-Table-1)

The `u v` (`calc-enter-units-table`) command displays the units table
in another buffer called `*Units Table*`. Each entry in this table
gives the unit name as it would appear in an expression, the definition
of the unit in terms of simpler units, and a full name or description of
the unit. Fundamental units are defined as themselves; these are the
units produced by the `u b` command. The fundamental units are
meters, seconds, grams, kelvins, amperes, candelas, moles, radians,
and steradians.

The Units Table buffer also displays the Unit Prefix Table. Note that
two prefixes, “kilo” and “hecto,” accept either upper- or lower-case
prefix letters. ‘`Meg`’ is also accepted as a synonym for the ‘`M`’
prefix. Whenever a unit name can be interpreted as either a built-in name
or a prefix followed by another built-in name, the former interpretation
wins. For example, ‘`2 pt`’ means two pints, not two pico-tons.

The Units Table buffer, once created, is not rebuilt unless you define
new units. To force the buffer to be rebuilt, give any numeric prefix
argument to `u v`.

The `u V` (`calc-view-units-table`) command is like `u v` except
that the cursor is not moved into the Units Table buffer. You can
type `u V` again to remove the Units Table from the display. To
return from the Units Table buffer after a `u v`, type `C-x * c`
again or use the regular Emacs `C-x o` (`other-window`)
command. You can also kill the buffer with `C-x k` if you wish;
the actual units table is safely stored inside the Calculator.

The `u g` (`calc-get-unit-definition`) command retrieves a unit’s
defining expression and pushes it onto the Calculator stack. For example,
`u g in` will produce the expression ‘`2.54 cm`’. This is the
same definition for the unit that would appear in the Units Table buffer.
Note that this command works only for actual unit names; `u g km`
will report that no such unit exists, for example, because `km` is
really the unit `m` with a `k` (“kilo”) prefix. To see a
definition of a unit in terms of base units, it is easier to push the
unit name on the stack and then reduce it to base units with `u b`.

The `u e` (`calc-explain-units`) command displays an English
description of the units of the expression on the stack. For example,
for the expression ‘`62 km^2 g / s^2 mol K`’, the description is
“Square-Kilometer Gram per (Second-squared Mole Kelvin).” This
command uses the English descriptions that appear in the righthand
column of the Units Table.

---

Next: [User-Defined Units](#User_002dDefined-Units), Previous: [The Units Table](#The-Units-Table), Up: [Operating on Units](#Units)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 11.3 Predefined Units [¶](#Predefined-Units-1)

The definitions of many units have changed over the years. For example,
the meter was originally defined in 1791 as one ten-millionth of the
distance from the Equator to the North Pole. In order to be more
precise, the definition was adjusted several times, and now a meter is
defined as the distance that light will travel in a vacuum in
1/299792458 of a second; consequently, the speed of light in a
vacuum is exactly 299792458 m/s. Many other units have been
redefined in terms of fundamental physical processes; a second, for
example, is currently defined as 9192631770 periods of a certain
radiation related to the cesium-133 atom.
The British imperial units, once defined in terms of physical objects,
were redefined in 1963 in terms of SI units. The US customary units,
which were the same as British units until the British imperial system
was created in 1824, were also defined in terms of the SI units in 1893.
Because of these redefinitions, conversions between metric, British
Imperial, and US customary units can often be done precisely.

Since the exact definitions of many kinds of units have evolved over the
years, and since certain countries sometimes have local differences in
their definitions, it is a good idea to examine Calc’s definition of a
unit before depending on its exact value. For example, there are three
different units for gallons, corresponding to the US (`gal`),
Canadian (`galC`), and British (`galUK`) definitions. Also,
note that `oz` is a standard ounce of mass, `ozt` is a Troy
ounce, and `ozfl` is a fluid ounce.

The temperature units corresponding to Kelvin and degree Celsius
are the same in this table, since most units commands treat
temperatures as being relative. The `calc-convert-temperature`
command has special rules for handling the different absolute magnitudes
of the various temperature scales.

The unit of volume “liters” can be referred to by either the lower-case
`l` or the upper-case `L`.

The unit `A` stands for amperes; the name `Ang` is used
for angstroms.

The unit `pt` stands for pints; the name `point` stands for
a typographical point, defined by ‘`72 point = 1 in`’. This is
slightly different from the point defined by the American Typefounder’s
Association in 1886, but the point used by Calc has become standard
largely due to its use by the PostScript page description language.
There is also `texpt`, which stands for a printer’s point as
defined by the TeX typesetting system: ‘`72.27 texpt = 1 in`’.
Other units used by TeX are available; they are `texpc` (a pica),
`texbp` (a “big point”, equal to a standard point which is larger
than the point used by TeX), `texdd` (a Didot point),
`texcc` (a Cicero) and `texsp` (a scaled TeX point,
all dimensions representable in TeX are multiples of this value).

When Calc is using the TeX or LaTeX language mode (see [TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes)), the TeX specific unit names will not
use the ‘`tex`’ prefix; the unit name for a TeX point will be
‘`pt`’ instead of ‘`texpt`’, for example. To avoid conflicts,
the unit names for pint and parsec will simply be ‘`pint`’ and
‘`parsec`’ instead of ‘`pt`’ and ‘`pc`’.

The unit `e` stands for the elementary (electron) unit of charge;
because algebra command could mistake this for the special constant
‘`e`’, Calc provides the alternate unit name `ech` which is
preferable to `e`.

The name `g` stands for one gram of mass; there is also `gf`,
one gram of force. (Likewise for `lb`, pounds, and `lbf`.)
Meanwhile, one “‘`g`’” of acceleration is denoted `ga`.

The unit `ton` is a U.S. ton of ‘`2000 lb`’, and `t` is
a metric ton of ‘`1000 kg`’.

The names `s` (or `sec`) and `min` refer to units of
time; `arcsec` and `arcmin` are units of angle.

Some “units” are really physical constants; for example, `c`
represents the speed of light, and `h` represents Planck’s
constant. You can use these just like other units: converting
‘`.5 c`’ to ‘`m/s`’ expresses one-half the speed of light in
meters per second. You can also use this merely as a handy reference;
the `u g` command gets the definition of one of these constants
in its normal terms, and `u b` expresses the definition in base
units.

Two units, `pi` and `alpha` (the fine structure constant,
approximately *1/137*) are dimensionless. The units simplification
commands simply treat these names as equivalent to their corresponding
values. However you can, for example, use `u c` to convert a pure
number into multiples of the fine structure constant, or `u b` to
convert this back into a pure number. (When `u c` prompts for the
“old units,” just enter a blank line to signify that the value
really is unitless.)

---

Next: [Logarithmic Units](#Logarithmic-Units), Previous: [Predefined Units](#Predefined-Units), Up: [Operating on Units](#Units)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 11.4 User-Defined Units [¶](#User_002dDefined-Units-1)

Calc provides ways to get quick access to your selected “favorite”
units, as well as ways to define your own new units.

To select your favorite units, store a vector of unit names or
expressions in the Calc variable `Units`. The `u 1`
through `u 9` commands (`calc-quick-units`) provide access
to these units. If the value on the top of the stack is a plain
number (with no units attached), then `u 1` gives it the
specified units. (Basically, it multiplies the number by the
first item in the `Units` vector.) If the number on the
stack *does* have units, then `u 1` converts that number
to the new units. For example, suppose the vector ‘`[in, ft]`’
is stored in `Units`. Then `30 u 1` will create the
expression ‘`30 in`’, and `u 2` will convert that expression
to ‘`2.5 ft`’.

The `u 0` command accesses the tenth element of `Units`.
Only ten quick units may be defined at a time. If the `Units`
variable has no stored value (the default), or if its value is not
a vector, then the quick-units commands will not function. The
`s U` command is a convenient way to edit the `Units`
variable; see [Other Operations on Variables](#Operations-on-Variables).

The `u d` (`calc-define-unit`) command records the units
expression on the top of the stack as the definition for a new,
user-defined unit. For example, putting ‘`16.5 ft`’ on the stack and
typing `u d rod` defines the new unit ‘`rod`’ to be equivalent to
16.5 feet. The unit conversion and simplification commands will now
treat `rod` just like any other unit of length. You will also be
prompted for an optional English description of the unit, which will
appear in the Units Table. If you wish the definition of this unit to
be displayed in a special way in the Units Table buffer (such as with an
asterisk to indicate an approximate value), then you can call this
command with an argument, `C-u u d`; you will then also be prompted
for a string that will be used to display the definition.

The `u u` (`calc-undefine-unit`) command removes a user-defined
unit. It is not possible to remove one of the predefined units,
however.

If you define a unit with an existing unit name, your new definition
will replace the original definition of that unit. If the unit was a
predefined unit, the old definition will not be replaced, only
“shadowed.” The built-in definition will reappear if you later use
`u u` to remove the shadowing definition.

To create a new fundamental unit, use either 1 or the unit name itself
as the defining expression. Otherwise the expression can involve any
other units that you like (except for composite units like ‘`mfi`’).
You can create a new composite unit with a sum of other units as the
defining expression. The next unit operation like `u c` or `u v`
will rebuild the internal unit table incorporating your modifications.
Note that erroneous definitions (such as two units defined in terms of
each other) will not be detected until the unit table is next rebuilt;
`u v` is a convenient way to force this to happen.

Temperature units are treated specially inside the Calculator; it is not
possible to create user-defined temperature units.

The `u p` (`calc-permanent-units`) command stores the user-defined
units in your Calc init file (the file given by the variable
`calc-settings-file`, typically `~/.emacs.d/calc.el`), so that the
units will still be available in subsequent Emacs sessions. If there
was already a set of user-defined units in your Calc init file, it
is replaced by the new set. (See [General Mode Commands](#General-Mode-Commands), for a way to
tell Calc to use a different file for the Calc init file.)

---

Next: [Musical Notes](#Musical-Notes), Previous: [User-Defined Units](#User_002dDefined-Units), Up: [Operating on Units](#Units)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 11.5 Logarithmic Units [¶](#Logarithmic-Units-1)

The units `dB` (decibels) and `Np` (nepers) are logarithmic
units which are manipulated differently than standard units. Calc
provides commands to work with these logarithmic units.

Decibels and nepers are used to measure power quantities as well as
field quantities (quantities whose squares are proportional to power);
these two types of quantities are handled slightly different from each
other. By default the Calc commands work as if power quantities are
being used; with the `H` prefix the Calc commands work as if field
quantities are being used.

The decibel level of a power
*P1*,
relative to a reference power
*P0*,
is defined to be
*10 log10(P1/P0) dB*.
(The factor of 10 is because a decibel, as its name implies, is
one-tenth of a bel. The bel, named after Alexander Graham Bell, was
considered to be too large of a unit and was effectively replaced by
the decibel.) If *F* is a field quantity with power
*P=k F^2*, then a reference quantity of
*F0*
would correspond to a power of
*P0=k F0^2*.
If
*P1=k F1^2*,
then

```
10 log10(P1/P0) = 10 log10(F1^2/F0^2) = 20 log10(F1/F0).
```

In order to get the same decibel level regardless of whether a field
quantity or the corresponding power quantity is used, the decibel
level of a field quantity
*F1*,
relative to a reference
*F0*,
is defined as
*20 log10(F1/F0) dB*.
For example, the decibel value of a sound pressure level of
*60 uPa*
relative to
*20 uPa*
(the threshold of human hearing) is
*20 log10(60 uPa/ 20 uPa) dB = 20 log10(3) dB*,
which is about
*9.54 dB*.
Note that in taking the ratio, the original units cancel and so these
logarithmic units are dimensionless.

Nepers (named after John Napier, who is credited with inventing the
logarithm) are similar to bels except they use natural logarithms instead
of common logarithms. The neper level of a power
*P1*,
relative to a reference power
*P0*,
is
*(1/2) ln(P1/P0) Np*.
The neper level of a field
*F1*,
relative to a reference field
*F0*,
is
*ln(F1/F0) Np*.

For power quantities, Calc uses
*1 mW*
as the default reference quantity; this default can be changed by changing
the value of the customizable variable
`calc-lu-power-reference` (see [Customizing Calc](#Customizing-Calc)).
For field quantities, Calc uses
*20 uPa*
as the default reference quantity; this is the value used in acoustics
which is where decibels are commonly encountered. This default can be
changed by changing the value of the customizable variable
`calc-lu-field-reference` (see [Customizing Calc](#Customizing-Calc)). A
non-default reference quantity will be read from the stack if the
capital `O` prefix is used.

The `l q` (`calc-lu-quant`) [`lupquant`]
command computes the power quantity corresponding to a given number of
logarithmic units. With the capital `O` prefix, `O l q`, the
reference level will be read from the top of the stack. (In an
algebraic formula, `lupquant` can be given an optional second
argument which will be used for the reference level.) For example,
`20 dB RET l q` will return `100 mW`;
`20 dB RET 4 W RET O l q` will return `400 W`.
The `H l q` [`lufquant`] command behaves like `l q` but
computes field quantities instead of power quantities.

The `l d` (`calc-db`) [`dbpower`] command will compute
the decibel level of a power quantity using the default reference
level; `H l d` [`dbfield`] will compute the decibel level of
a field quantity. The commands `l n` (`calc-np`)
[`nppower`] and `H l n` [`npfield`] will similarly
compute neper levels. With the capital `O` prefix these commands
will read a reference level from the stack; in an algebraic formula
the reference level can be given as an optional second argument.

The sum of two power or field quantities doesn’t correspond to the sum
of the corresponding decibel or neper levels. If the powers
corresponding to decibel levels
*D1*
and
*D2*
are added, the corresponding decibel level “sum” will be

```
  10 log10(10^(D1/10) + 10^(D2/10)) dB.
```

When field quantities are combined, it often means the corresponding
powers are added and so the above formula might be used. In
acoustics, for example, the sound pressure level is a field quantity
and so the decibels are often defined using the field formula, but the
sound pressure levels are combined as the sound power levels, and so
the above formula should be used. If two field quantities themselves
are added, the new decibel level will be

```
  20 log10(10^(D1/20) + 10^(D2/20)) dB.
```

If the power corresponding to *D* dB is multiplied by a number *N*,
then the corresponding decibel level will be

```
  D + 10 log10(N) dB,
```

if a field quantity is multiplied by *N* the corresponding decibel level
will be

```
  D + 20 log10(N) dB.
```

There are similar formulas for combining nepers. The `l +`
(`calc-lu-plus`) [`lupadd`] command will “add” two
logarithmic unit power levels this way; with the `H` prefix,
`H l +` [`lufadd`] will add logarithmic unit field levels.
Similarly, logarithmic units can be “subtracted” with `l -`
(`calc-lu-minus`) [`lupsub`] or `H l -` [`lufsub`].
The `l *` (`calc-lu-times`) [`lupmul`] and `H l *`
[`lufmul`] commands will “multiply” a logarithmic unit by a
number; the `l /` (`calc-lu-divide`) [`lupdiv`] and
`H l /` [`lufdiv`] commands will “divide” a logarithmic
unit by a number. Note that the reference quantities don’t play a role
in this arithmetic.

---

Previous: [Logarithmic Units](#Logarithmic-Units), Up: [Operating on Units](#Units)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 11.6 Musical Notes [¶](#Musical-Notes-1)

Calc can convert between musical notes and their associated
frequencies. Notes can be given using either scientific pitch
notation or midi numbers. Since these note systems are basically
logarithmic scales, Calc uses the `l` prefix for functions
operating on notes.

Scientific pitch notation refers to a note by giving a letter
A through G, possibly followed by a flat or sharp, with a subscript
indicating an octave number. Each octave starts with C and ends with
B and
the octave numbered 0 was chosen to correspond to the lowest
audible frequency. Using this system, middle C (about 261.625 Hz)
corresponds to the note *C* in octave 4 and is denoted
*C* with subscript *4*.
Any frequency can be described by giving a note plus an
offset in cents (where a cent is a ratio of frequencies so that a
semitone consists of 100 cents).

The midi note number system assigns numbers to notes so that
*C* with subscript *-1* corresponds to the midi note
number 0, and *G* with subscript *9*
corresponds to the midi note number 127. A midi controller can have
up to 128 keys and each midi note number from 0 to 127 corresponds to
a possible key.

The `l s` (`calc-spn`) [`spn`] command converts either
a frequency or a midi number to scientific pitch notation. For
example, `500 Hz` gets converted to
`B_4 + 21.3094853649 cents` and `84` to `C_6`.

The `l m` (`calc-midi`) [`midi`] command converts either
a frequency or a note given in scientific pitch notation to the
corresponding midi number. For example, `C_6` gets converted to 84
and `440 Hz` to 69.

The `l f` (`calc-freq`) [`freq`] command converts either
either a midi number or a note given in scientific pitch notation to
the corresponding frequency. For example, `Asharp_2 + 30 cents`
gets converted to `118.578040134 Hz` and `55` to
`195.99771799 Hz`.

Since the frequencies of notes are not usually given exactly (and are
typically irrational), the customizable variable
`calc-note-threshold` determines how close (in cents) a frequency
needs to be to a note to be recognized as that note
(see [Customizing Calc](#Customizing-Calc)). This variable has a default value of
`1`. For example, middle C is approximately
‘`261.625565302 Hz`’; this frequency is often shortened to
‘`261.625 Hz`’. Without `calc-note-threshold` (or a value of
‘`0`’), Calc would convert `261.625 Hz` to scientific pitch
notation `B_3 + 99.9962592773 cents`; with the default value of
`1`, Calc converts `261.625 Hz` to `C_4`.

---

Next: [Graphics](#Graphics), Previous: [Operating on Units](#Units), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 12 Storing and Recalling [¶](#Storing-and-Recalling)

Calculator variables are really just Lisp variables that contain numbers
or formulas in a form that Calc can understand. The commands in this
section allow you to manipulate variables conveniently. Commands related
to variables use the `s` prefix key.

* [Storing Variables](#Storing-Variables)
* [Recalling Variables](#Recalling-Variables)
* [Other Operations on Variables](#Operations-on-Variables)
* [The Let Command](#Let-Command)
* [The Evaluates-To Operator](#Evaluates_002dTo-Operator)

---

Next: [Recalling Variables](#Recalling-Variables), Up: [Storing and Recalling](#Store-and-Recall)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 12.1 Storing Variables [¶](#Storing-Variables-1)

The `s s` (`calc-store`) command stores the value at the top of
the stack into a specified variable. It prompts you to enter the
name of the variable. If you press a single digit, the value is stored
immediately in one of the “quick” variables `q0` through
`q9`. Or you can enter any variable name.

The `s s` command leaves the stored value on the stack. There is
also an `s t` (`calc-store-into`) command, which removes a
value from the stack and stores it in a variable.

If the top of stack value is an equation ‘`a = 7`’ or assignment
‘`a := 7`’ with a variable on the lefthand side, then Calc will
assign that variable with that value by default, i.e., if you type
`s s RET` or `s t RET`. In this example, the
value 7 would be stored in the variable ‘`a`’. (If you do type
a variable name at the prompt, the top-of-stack value is stored in
its entirety, even if it is an equation: ‘`s s b RET`’
with ‘`a := 7`’ on the stack stores ‘`a := 7`’ in `b`.)

In fact, the top of stack value can be a vector of equations or
assignments with different variables on their lefthand sides; the
default will be to store all the variables with their corresponding
righthand sides simultaneously.

It is also possible to type an equation or assignment directly at
the prompt for the `s s` or `s t` command: `s s foo = 7`.
In this case the expression to the right of the `=` or `:=`
symbol is evaluated as if by the `=` command, and that value is
stored in the variable. No value is taken from the stack; `s s`
and `s t` are equivalent when used in this way.

The prefix keys `s` and `t` may be followed immediately by a
digit; `s 9` is equivalent to `s s 9`, and `t 9` is
equivalent to `s t 9`. (The `t` prefix is otherwise used
for trail and time/date commands.)

There are also several “arithmetic store” commands. For example,
`s +` removes a value from the stack and adds it to the specified
variable. The other arithmetic stores are `s -`, `s *`, `s /`,
`s ^`, and `s |` (vector concatenation), plus `s n` and
`s &` which negate or invert the value in a variable, and `s [`
and `s ]` which decrease or increase a variable by one.

All the arithmetic stores accept the Inverse prefix to reverse the
order of the operands. If ‘`v`’ represents the contents of the
variable, and ‘`a`’ is the value drawn from the stack, then regular
`s -` assigns
‘`v := v - a`’,
but `I s -` assigns
‘`v := a - v`’.
While `I s *` might seem pointless, it is
useful if matrix multiplication is involved. Actually, all the
arithmetic stores use formulas designed to behave usefully both
forwards and backwards:

```
s +        v := v + a          v := a + v
s -        v := v - a          v := a - v
s *        v := v * a          v := a * v
s /        v := v / a          v := a / v
s ^        v := v ^ a          v := a ^ v
s |        v := v | a          v := a | v
s n        v := v / (-1)       v := (-1) / v
s &        v := v ^ (-1)       v := (-1) ^ v
s [        v := v - 1          v := 1 - v
s ]        v := v - (-1)       v := (-1) - v
```

In the last four cases, a numeric prefix argument will be used in
place of the number one. (For example, `M-2 s ]` increases
a variable by 2, and `M-2 I s ]` replaces a variable by
minus-two minus the variable.

The first six arithmetic stores can also be typed `s t +`, `s t -`,
etc. The commands `s s +`, `s s -`, and so on are analogous
arithmetic stores that don’t remove the value ‘`a`’ from the stack.

All arithmetic stores report the new value of the variable in the
Trail for your information. They signal an error if the variable
previously had no stored value. If default simplifications have been
turned off, the arithmetic stores temporarily turn them on for numeric
arguments only (i.e., they temporarily do an `m N` command).
See [Simplification Modes](#Simplification-Modes). Large vectors put in the trail by
these commands always use abbreviated (`t .`) mode.

The `s m` command is a general way to adjust a variable’s value
using any Calc function. It is a “mapping” command analogous to
`V M`, `V R`, etc. See [Reducing and Mapping Vectors](#Reducing-and-Mapping), to see
how to specify a function for a mapping command. Basically,
all you do is type the Calc command key that would invoke that
function normally. For example, `s m n` applies the `n`
key to negate the contents of the variable, so `s m n` is
equivalent to `s n`. Also, `s m Q` takes the square root
of the value stored in a variable, `s m v v` uses `v v` to
reverse the vector stored in the variable, and `s m H I S`
takes the hyperbolic arcsine of the variable contents.

If the mapping function takes two or more arguments, the additional
arguments are taken from the stack; the old value of the variable
is provided as the first argument. Thus `s m -` with ‘`a`’
on the stack computes ‘`v - a`’, just like `s -`. With the
Inverse prefix, the variable’s original value becomes the *last*
argument instead of the first. Thus `I s m -` is also
equivalent to `I s -`.

The `s x` (`calc-store-exchange`) command exchanges the value
of a variable with the value on the top of the stack. Naturally, the
variable must already have a stored value for this to work.

You can type an equation or assignment at the `s x` prompt. The
command `s x a=6` takes no values from the stack; instead, it
pushes the old value of ‘`a`’ on the stack and stores ‘`a = 6`’.

Until you store something in them, most variables are “void,” that is,
they contain no value at all. If they appear in an algebraic formula
they will be left alone even if you press `=` (`calc-evaluate`).
The `s u` (`calc-unstore`) command returns a variable to the
void state.

The `s c` (`calc-copy-variable`) command copies the stored
value of one variable to another. One way it differs from a simple
`s r` followed by an `s t` (aside from saving keystrokes) is
that the value never goes on the stack and thus is never rounded,
evaluated, or simplified in any way; it is not even rounded down to the
current precision.

The only variables with predefined values are the “special constants”
`pi`, `e`, `i`, `phi`, and `gamma`. You are free
to unstore these variables or to store new values into them if you like,
although some of the algebraic-manipulation functions may assume these
variables represent their standard values. Calc displays a warning if
you change the value of one of these variables, or of one of the other
special variables `inf`, `uinf`, and `nan` (which are
normally void).

Note that `pi` doesn’t actually have 3.14159265359 stored in it,
but rather a special magic value that evaluates to ‘`pi`’ at the current
precision. Likewise `e`, `i`, and `phi` evaluate
according to the current precision or polar mode. If you recall a value
from `pi` and store it back, this magic property will be lost. The
magic property is preserved, however, when a variable is copied with
`s c`.

If one of the “special constants” is redefined (or undefined) so that
it no longer has its magic property, the property can be restored with
`s k` (`calc-copy-special-constant`). This command will prompt
for a special constant and a variable to store it in, and so a special
constant can be stored in any variable. Here, the special constant that
you enter doesn’t depend on the value of the corresponding variable;
`pi` will represent 3.14159… regardless of what is currently
stored in the Calc variable `pi`. If one of the other special
variables, `inf`, `uinf` or `nan`, is given a value, its
original behavior can be restored by voiding it with `s u`.

---

Next: [Other Operations on Variables](#Operations-on-Variables), Previous: [Storing Variables](#Storing-Variables), Up: [Storing and Recalling](#Store-and-Recall)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 12.2 Recalling Variables [¶](#Recalling-Variables-1)

The most straightforward way to extract the stored value from a variable
is to use the `s r` (`calc-recall`) command. This command prompts
for a variable name (similarly to `calc-store`), looks up the value
of the specified variable, and pushes that value onto the stack. It is
an error to try to recall a void variable.

It is also possible to recall the value from a variable by evaluating a
formula containing that variable. For example, `' a RET =` is
the same as `s r a RET` except that if the variable is void, the
former will simply leave the formula ‘`a`’ on the stack whereas the
latter will produce an error message.

The `r` prefix may be followed by a digit, so that `r 9` is
equivalent to `s r 9`.

---

Next: [The Let Command](#Let-Command), Previous: [Recalling Variables](#Recalling-Variables), Up: [Storing and Recalling](#Store-and-Recall)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 12.3 Other Operations on Variables [¶](#Other-Operations-on-Variables)

The `s e` (`calc-edit-variable`) command edits the stored
value of a variable without ever putting that value on the stack
or simplifying or evaluating the value. It prompts for the name of
the variable to edit. If the variable has no stored value, the
editing buffer will start out empty. If the editing buffer is
empty when you press `C-c C-c` to finish, the variable will
be made void. See [Editing Stack Entries](#Editing-Stack-Entries), for a general
description of editing.

The `s e` command is especially useful for creating and editing
rewrite rules which are stored in variables. Sometimes these rules
contain formulas which must not be evaluated until the rules are
actually used. (For example, they may refer to ‘`deriv(x,y)`’,
where `x` will someday become some expression involving `y`;
if you let Calc evaluate the rule while you are defining it, Calc will
replace ‘`deriv(x,y)`’ with 0 because the formula `x` does
not itself refer to `y`.) By contrast, recalling the variable,
editing with `` ` ``, and storing will evaluate the variable’s value
as a side effect of putting the value on the stack.

There are several special-purpose variable-editing commands that
use the `s` prefix followed by a shifted letter:

`s A`
:   Edit `AlgSimpRules`. See [Algebraic Simplifications](#Algebraic-Simplifications).

`s D`
:   Edit `Decls`. See [Declarations](#Declarations).

`s E`
:   Edit `EvalRules`. See [Basic Simplifications](#Basic-Simplifications).

`s F`
:   Edit `FitRules`. See [Curve Fitting](#Curve-Fitting).

`s G`
:   Edit `GenCount`. See [Solving Equations](#Solving-Equations).

`s H`
:   Edit `Holidays`. See [Business Days](#Business-Days).

`s I`
:   Edit `IntegLimit`. See [Calculus](#Calculus).

`s L`
:   Edit `LineStyles`. See [Graphics](#Graphics).

`s P`
:   Edit `PointStyles`. See [Graphics](#Graphics).

`s R`
:   Edit `PlotRejects`. See [Graphics](#Graphics).

`s T`
:   Edit `TimeZone`. See [Time Zones](#Time-Zones).

`s U`
:   Edit `Units`. See [User-Defined Units](#User_002dDefined-Units).

`s X`
:   Edit `ExtSimpRules`. See [“Unsafe” Simplifications](#Unsafe-Simplifications).

These commands are just versions of `s e` that use fixed variable
names rather than prompting for the variable name.

The `s p` (`calc-permanent-variable`) command saves a
variable’s value permanently in your Calc init file (the file given by
the variable `calc-settings-file`, typically `~/.emacs.d/calc.el`), so
that its value will still be available in future Emacs sessions. You
can re-execute `s p` later on to update the saved value, but the
only way to remove a saved variable is to edit your calc init file
by hand. (See [General Mode Commands](#General-Mode-Commands), for a way to tell Calc to
use a different file for the Calc init file.)

If you do not specify the name of a variable to save (i.e.,
`s p RET`), all Calc variables with defined values
are saved except for the special constants `pi`, `e`,
`i`, `phi`, and `gamma`; the variables `TimeZone`
and `PlotRejects`;
`FitRules`, `DistribRules`, and other built-in rewrite
rules; and `PlotDatan` variables generated
by the graphics commands. (You can still save these variables by
explicitly naming them in an `s p` command.)

The `s i` (`calc-insert-variables`) command writes
the values of all Calc variables into a specified buffer.
The variables are written with the prefix `var-` in the form of
Lisp `setq` commands
which store the values in string form. You can place these commands
in your Calc init file (or `.emacs`) if you wish, though in this case it
would be easier to use `s p RET`. (Note that `s i`
omits the same set of variables as `s p RET`; the difference
is that `s i` will store the variables in any buffer, and it also
stores in a more human-readable format.)

---

Next: [The Evaluates-To Operator](#Evaluates_002dTo-Operator), Previous: [Other Operations on Variables](#Operations-on-Variables), Up: [Storing and Recalling](#Store-and-Recall)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 12.4 The Let Command [¶](#The-Let-Command)

If you have an expression like ‘`a+b^2`’ on the stack and you wish to
compute its value where ‘`b=3`’, you can simply store 3 in ‘`b`’ and
then press `=` to reevaluate the formula. This has the side-effect
of leaving the stored value of 3 in ‘`b`’ for future operations.

The `s l` (`calc-let`) command evaluates a formula under a
*temporary* assignment of a variable. It stores the value on the
top of the stack into the specified variable, then evaluates the
second-to-top stack entry, then restores the original value (or lack of one)
in the variable. Thus after `' a+b^2 RET 3 s l b RET`,
the stack will contain the formula ‘`a + 9`’. The subsequent command
`5 s l a RET` will replace this formula with the number 14.
The variables ‘`a`’ and ‘`b`’ are not permanently affected in any way
by these commands.

The value on the top of the stack may be an equation or assignment, or
a vector of equations or assignments, in which case the default will be
analogous to the case of `s t RET`. See [Storing Variables](#Storing-Variables).

Also, you can answer the variable-name prompt with an equation or
assignment: `s l b=3 RET` is the same as storing 3 on the stack
and typing `s l b RET`.

The `a b` (`calc-substitute`) command is another way to substitute
a variable with a value in a formula. It does an actual substitution
rather than temporarily assigning the variable and evaluating. For
example, letting ‘`n=2`’ in ‘`f(n pi)`’ with `a b` will
produce ‘`f(2 pi)`’, whereas `s l` would give ‘`f(6.28)`’
since the evaluation step will also evaluate `pi`.

---

Previous: [The Let Command](#Let-Command), Up: [Storing and Recalling](#Store-and-Recall)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 12.5 The Evaluates-To Operator [¶](#The-Evaluates_002dTo-Operator)

The special algebraic symbol ‘`=>`’ is known as the *evaluates-to
operator*. (It will show up as an `evalto` function call in
other language modes like Pascal and LaTeX.) This is a binary
operator, that is, it has a lefthand and a righthand argument,
although it can be entered with the righthand argument omitted.

A formula like ‘`a => b`’ is evaluated by Calc as
follows: First, a is not simplified or modified in any
way. The previous value of argument b is thrown away; the
formula a is then copied and evaluated as if by the `=`
command according to all current modes and stored variable values,
and the result is installed as the new value of b.

For example, suppose you enter the algebraic formula ‘`2 + 3 => 17`’.
The number 17 is ignored, and the lefthand argument is left in its
unevaluated form; the result is the formula ‘`2 + 3 => 5`’.

You can enter an ‘`=>`’ formula either directly using algebraic
entry (in which case the righthand side may be omitted since it is
going to be replaced right away anyhow), or by using the `s =`
(`calc-evalto`) command, which takes a from the stack
and replaces it with ‘`a => b`’.

Calc keeps track of all ‘`=>`’ operators on the stack, and
recomputes them whenever anything changes that might affect their
values, i.e., a mode setting or variable value. This occurs only
if the ‘`=>`’ operator is at the top level of the formula, or
if it is part of a top-level vector. In other words, pushing
‘`2 + (a => 17)`’ will change the 17 to the actual value of
‘`a`’ when you enter the formula, but the result will not be
dynamically updated when ‘`a`’ is changed later because the
‘`=>`’ operator is buried inside a sum. However, a vector
of ‘`=>`’ operators will be recomputed, since it is convenient
to push a vector like ‘`[a =>, b =>, c =>]`’ on the stack to
make a concise display of all the variables in your problem.
(Another way to do this would be to use ‘`[a, b, c] =>`’,
which provides a slightly different format of display. You
can use whichever you find easiest to read.)

The `m C` (`calc-auto-recompute`) command allows you to
turn this automatic recomputation on or off. If you turn
recomputation off, you must explicitly recompute an ‘`=>`’
operator on the stack in one of the usual ways, such as by
pressing `=`. Turning recomputation off temporarily can save
a lot of time if you will be changing several modes or variables
before you look at the ‘`=>`’ entries again.

Most commands are not especially useful with ‘`=>`’ operators
as arguments. For example, given ‘`x + 2 => 17`’, it won’t
work to type `1 +` to get ‘`x + 3 => 18`’. If you want
to operate on the lefthand side of the ‘`=>`’ operator on
the top of the stack, type `j 1` (that’s the digit “one”)
to select the lefthand side, execute your commands, then type
`j u` to unselect.

All current modes apply when an ‘`=>`’ operator is computed,
including the current simplification mode. Recall that the
formula ‘`arcsin(sin(x))`’ will not be handled by Calc’s algebraic
simplifications, but Calc’s unsafe simplifications will reduce it to
‘`x`’. If you enter ‘`arcsin(sin(x)) =>`’ normally, the result
will be ‘`arcsin(sin(x)) => arcsin(sin(x))`’. If you change to
Extended Simplification mode, the result will be
‘`arcsin(sin(x)) => x`’. However, just pressing `a e`
once will have no effect on ‘`arcsin(sin(x)) => arcsin(sin(x))`’,
because the righthand side depends only on the lefthand side
and the current mode settings, and the lefthand side is not
affected by commands like `a e`.

The “let” command (`s l`) has an interesting interaction
with the ‘`=>`’ operator. The `s l` command evaluates the
second-to-top stack entry with the top stack entry supplying
a temporary value for a given variable. As you might expect,
if that stack entry is an ‘`=>`’ operator its righthand
side will temporarily show this value for the variable. In
fact, all ‘`=>`’s on the stack will be updated if they refer
to that variable. But this change is temporary in the sense
that the next command that causes Calc to look at those stack
entries will make them revert to the old variable value.

```
2:  a => a             2:  a => 17         2:  a => a
1:  a + 1 => a + 1     1:  a + 1 => 18     1:  a + 1 => a + 1
    .                      .                   .

                           17 s l a RET        p 8 RET
```

Here the `p 8` command changes the current precision,
thus causing the ‘`=>`’ forms to be recomputed after the
influence of the “let” is gone. The `d SPC` command
(`calc-refresh`) is a handy way to force the ‘`=>`’
operators on the stack to be recomputed without any other
side effects.

Embedded mode also uses ‘`=>`’ operators. In Embedded mode,
the lefthand side of an ‘`=>`’ operator can refer to variables
assigned elsewhere in the file by ‘`:=`’ operators. The
assignment operator ‘`a := 17`’ does not actually do anything
by itself. But Embedded mode recognizes it and marks it as a sort
of file-local definition of the variable. You can enter ‘`:=`’
operators in Algebraic mode, or by using the `s :`
(`calc-assign`) [`assign`] command which takes a variable
and value from the stack and replaces them with an assignment.

See [TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes), for the way ‘`=>`’ appears in
TeX language output. The *eqn* mode gives similar
treatment to ‘`=>`’.

---

Next: [Kill and Yank Functions](#Kill-and-Yank), Previous: [Storing and Recalling](#Store-and-Recall), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 13 Graphics [¶](#Graphics-1)

The commands for graphing data begin with the `g` prefix key. Calc
uses GNUPLOT 2.0 or later to do graphics. These commands will only work
if GNUPLOT is available on your system. (While GNUPLOT sounds like
a relative of GNU Emacs, it is actually completely unrelated.
However, it is free software. It can be obtained from
‘`http://www.gnuplot.info`’.)

If you have GNUPLOT installed on your system but Calc is unable to
find it, you may need to set the `calc-gnuplot-name` variable in
your Calc init file or `.emacs`. You may also need to set some
Lisp variables to show Calc how to run GNUPLOT on your system; these
are described under `g D` and `g O` below. If you are using
the X window system or MS-Windows, Calc will configure GNUPLOT for you
automatically. If you have GNUPLOT 3.0 or later and you are using a
Unix or GNU system without X, Calc will configure GNUPLOT to display
graphs using simple character graphics that will work on any
POSIX-compatible terminal.

* [Basic Graphics](#Basic-Graphics)
* [Three-Dimensional Graphics](#Three-Dimensional-Graphics)
* [Managing Curves](#Managing-Curves)
* [Graphics Options](#Graphics-Options)
* [Graphical Devices](#Devices)

---

Next: [Three-Dimensional Graphics](#Three-Dimensional-Graphics), Up: [Graphics](#Graphics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 13.1 Basic Graphics [¶](#Basic-Graphics-1)

The easiest graphics command is `g f` (`calc-graph-fast`).
This command takes two vectors of equal length from the stack.
The vector at the top of the stack represents the “y” values of
the various data points. The vector in the second-to-top position
represents the corresponding “x” values. This command runs
GNUPLOT (if it has not already been started by previous graphing
commands) and displays the set of data points. The points will
be connected by lines, and there will also be some kind of symbol
to indicate the points themselves.

The “x” entry may instead be an interval form, in which case suitable
“x” values are interpolated between the minimum and maximum values of
the interval (whether the interval is open or closed is ignored).

The “x” entry may also be a number, in which case Calc uses the
sequence of “x” values ‘`x`’, ‘`x+1`’, ‘`x+2`’, etc.
(Generally the number 0 or 1 would be used for ‘`x`’ in this case.)

The “y” entry may be any formula instead of a vector. Calc effectively
uses `N` (`calc-eval-num`) to evaluate variables in the formula;
the result of this must be a formula in a single (unassigned) variable.
The formula is plotted with this variable taking on the various “x”
values. Graphs of formulas by default use lines without symbols at the
computed data points. Note that if neither “x” nor “y” is a vector,
Calc guesses at a reasonable number of data points to use. See the
`g N` command below. (The “x” values must be either a vector
or an interval if “y” is a formula.)

If “y” is (or evaluates to) a formula of the form
‘`xy(x, y)`’ then the result is a
parametric plot. The two arguments of the fictitious `xy` function
are used as the “x” and “y” coordinates of the curve, respectively.
In this case the “x” vector or interval you specified is not directly
visible in the graph. For example, if “x” is the interval ‘`[0..360]`’
and “y” is the formula ‘`xy(sin(t), cos(t))`’, the resulting graph
will be a circle.

Also, “x” and “y” may each be variable names, in which case Calc
looks for suitable vectors, intervals, or formulas stored in those
variables.

The “x” and “y” values for the data points (as pulled from the vectors,
calculated from the formulas, or interpolated from the intervals) should
be real numbers (integers, fractions, or floats). One exception to this
is that the “y” entry can consist of a vector of numbers combined with
error forms, in which case the points will be plotted with the
appropriate error bars. Other than this, if either the “x”
value or the “y” value of a given data point is not a real number, that
data point will be omitted from the graph. The points on either side
of the invalid point will *not* be connected by a line.

See the documentation for `g a` below for a description of the way
numeric prefix arguments affect `g f`.

If you store an empty vector in the variable `PlotRejects`
(i.e., `[ ] s t PlotRejects`), Calc will append information to
this vector for every data point which was rejected because its
“x” or “y” values were not real numbers. The result will be
a matrix where each row holds the curve number, data point number,
“x” value, and “y” value for a rejected data point.
See [The Evaluates-To Operator](#Evaluates_002dTo-Operator), for a handy way to keep tabs on the
current value of `PlotRejects`. See [Other Operations on Variables](#Operations-on-Variables),
for the `s R` command which is another easy way to examine
`PlotRejects`.

To clear the graphics display, type `g c` (`calc-graph-clear`).
If the GNUPLOT output device is an X window, the window will go away.
Effects on other kinds of output devices will vary. You don’t need
to use `g c` if you don’t want to—if you give another `g f`
or `g p` command later on, it will reuse the existing graphics
window if there is one.

---

Next: [Managing Curves](#Managing-Curves), Previous: [Basic Graphics](#Basic-Graphics), Up: [Graphics](#Graphics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 13.2 Three-Dimensional Graphics [¶](#Three_002dDimensional-Graphics)

The `g F` (`calc-graph-fast-3d`) command makes a three-dimensional
graph. It works only if you have GNUPLOT 3.0 or later; with GNUPLOT 2.0,
you will see a GNUPLOT error message if you try this command.

The `g F` command takes three values from the stack, called “x”,
“y”, and “z”, respectively. As was the case for 2D graphs, there
are several options for these values.

In the first case, “x” and “y” are each vectors (not necessarily of
the same length); either or both may instead be interval forms. The
“z” value must be a matrix with the same number of rows as elements
in “x”, and the same number of columns as elements in “y”. The
result is a surface plot where
‘`z_ij`’
is the height of the point
at coordinate ‘`(x_i, y_j)`’ on the surface. The 3D graph will
be displayed from a certain default viewpoint; you can change this
viewpoint by adding a ‘`set view`’ to the `*Gnuplot Commands*`
buffer as described later. See the GNUPLOT documentation for a
description of the ‘`set view`’ command.

Each point in the matrix will be displayed as a dot in the graph,
and these points will be connected by a grid of lines (*isolines*).

In the second case, “x”, “y”, and “z” are all vectors of equal
length. The resulting graph displays a 3D line instead of a surface,
where the coordinates of points along the line are successive triplets
of values from the input vectors.

In the third case, “x” and “y” are vectors or interval forms, and
“z” is any formula involving two variables (not counting variables
with assigned values). These variables are sorted into alphabetical
order; the first takes on values from “x” and the second takes on
values from “y” to form a matrix of results that are graphed as a
3D surface.

If the “z” formula evaluates to a call to the fictitious function
‘`xyz(x, y, z)`’, then the result is a
“parametric surface.” In this case, the axes of the graph are
taken from the x and y values in these calls, and the
“x” and “y” values from the input vectors or intervals are used only
to specify the range of inputs to the formula. For example, plotting
‘`[0..360], [0..180], xyz(sin(x)*sin(y), cos(x)*sin(y), cos(y))`’
will draw a sphere. (Since the default resolution for 3D plots is
5 steps in each of “x” and “y”, this will draw a very crude
sphere. You could use the `g N` command, described below, to
increase this resolution, or specify the “x” and “y” values as
vectors with more than 5 elements.

It is also possible to have a function in a regular `g f` plot
evaluate to an `xyz` call. Since `g f` plots a line, not
a surface, the result will be a 3D parametric line. For example,
‘`[[0..720], xyz(sin(x), cos(x), x)]`’ will plot two turns of a
helix (a three-dimensional spiral).

As for `g f`, each of “x”, “y”, and “z” may instead be
variables containing the relevant data.

---

Next: [Graphics Options](#Graphics-Options), Previous: [Three-Dimensional Graphics](#Three-Dimensional-Graphics), Up: [Graphics](#Graphics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 13.3 Managing Curves [¶](#Managing-Curves-1)

The `g f` command is really shorthand for the following commands:
`C-u g d g a g p`. Likewise, `g F` is shorthand for
`C-u g d g A g p`. You can gain more control over your graph
by using these commands directly.

The `g a` (`calc-graph-add`) command adds the “curve”
represented by the two values on the top of the stack to the current
graph. You can have any number of curves in the same graph. When
you give the `g p` command, all the curves will be drawn superimposed
on the same axes.

The `g a` command (and many others that affect the current graph)
will cause a special buffer, `*Gnuplot Commands*`, to be displayed
in another window. This buffer is a template of the commands that will
be sent to GNUPLOT when it is time to draw the graph. The first
`g a` command adds a `plot` command to this buffer. Succeeding
`g a` commands add extra curves onto that `plot` command.
Other graph-related commands put other GNUPLOT commands into this
buffer. In normal usage you never need to work with this buffer
directly, but you can if you wish. The only constraint is that there
must be only one `plot` command, and it must be the last command
in the buffer. If you want to save and later restore a complete graph
configuration, you can use regular Emacs commands to save and restore
the contents of the `*Gnuplot Commands*` buffer.

If the values on the stack are not variable names, `g a` will invent
variable names for them (of the form ‘`PlotDatan`’) and store
the values in those variables. The “x” and “y” variables are what
go into the `plot` command in the template. If you add a curve
that uses a certain variable and then later change that variable, you
can replot the graph without having to delete and re-add the curve.
That’s because the variable name, not the vector, interval or formula
itself, is what was added by `g a`.

A numeric prefix argument on `g a` or `g f` changes the way
stack entries are interpreted as curves. With a positive prefix
argument ‘`n`’, the top ‘`n`’ stack entries are “y” values
for ‘`n`’ different curves which share a common “x” value in
the ‘`n+1`’st stack entry. (Thus `g a` with no prefix
argument is equivalent to `C-u 1 g a`.)

A prefix of zero or plain `C-u` means to take two stack entries,
“x” and “y” as usual, but to interpret “y” as a vector of
“y” values for several curves that share a common “x”.

A negative prefix argument tells Calc to read ‘`n`’ vectors from
the stack; each vector ‘`[x, y]`’ describes an independent curve.
This is the only form of `g a` that creates several curves at once
that don’t have common “x” values. (Of course, the range of “x”
values covered by all the curves ought to be roughly the same if
they are to look nice on the same graph.)

For example, to plot
‘`sin(n x)`’
for integers ‘`n`’
from 1 to 5, you could use `v x` to create a vector of integers
(‘`n`’), then `V M '` or `V M $` to map ‘`sin(n x)`’
across this vector. The resulting vector of formulas is suitable
for use as the “y” argument to a `C-u g a` or `C-u g f`
command.

The `g A` (`calc-graph-add-3d`) command adds a 3D curve
to the graph. It is not valid to intermix 2D and 3D curves in a
single graph. This command takes three arguments, “x”, “y”,
and “z”, from the stack. With a positive prefix ‘`n`’, it
takes ‘`n+2`’ arguments (common “x” and “y”, plus ‘`n`’
separate “z”s). With a zero prefix, it takes three stack entries
but the “z” entry is a vector of curve values. With a negative
prefix ‘`-n`’, it takes ‘`n`’ vectors of the form ‘`[x, y, z]`’.
The `g A` command works by adding a `splot` (surface-plot)
command to the `*Gnuplot Commands*` buffer.

(Although `g a` adds a 2D `plot` command to the
`*Gnuplot Commands*` buffer, Calc changes this to `splot`
before sending it to GNUPLOT if it notices that the data points are
evaluating to `xyz` calls. It will not work to mix 2D and 3D
`g a` curves in a single graph, although Calc does not currently
check for this.)

The `g d` (`calc-graph-delete`) command deletes the most
recently added curve from the graph. It has no effect if there are
no curves in the graph. With a numeric prefix argument of any kind,
it deletes all of the curves from the graph.

The `g H` (`calc-graph-hide`) command “hides” or “unhides”
the most recently added curve. A hidden curve will not appear in
the actual plot, but information about it such as its name and line and
point styles will be retained.

The `g j` (`calc-graph-juggle`) command moves the curve
at the end of the list (the “most recently added curve”) to the
front of the list. The next-most-recent curve is thus exposed for
`g d` or similar commands to use. With `g j` you can work
with any curve in the graph even though curve-related commands only
affect the last curve in the list.

The `g p` (`calc-graph-plot`) command uses GNUPLOT to draw
the graph described in the `*Gnuplot Commands*` buffer. Any
GNUPLOT parameters which are not defined by commands in this buffer
are reset to their default values. The variables named in the `plot`
command are written to a temporary data file and the variable names
are then replaced by the file name in the template. The resulting
plotting commands are fed to the GNUPLOT program. See the documentation
for the GNUPLOT program for more specific information. All temporary
files are removed when Emacs or GNUPLOT exits.

If you give a formula for “y”, Calc will remember all the values that
it calculates for the formula so that later plots can reuse these values.
Calc throws out these saved values when you change any circumstances
that may affect the data, such as switching from Degrees to Radians
mode, or changing the value of a parameter in the formula. You can
force Calc to recompute the data from scratch by giving a negative
numeric prefix argument to `g p`.

Calc uses a fairly rough step size when graphing formulas over intervals.
This is to ensure quick response. You can “refine” a plot by giving
a positive numeric prefix argument to `g p`. Calc goes through
the data points it has computed and saved from previous plots of the
function, and computes and inserts a new data point midway between
each of the existing points. You can refine a plot any number of times,
but beware that the amount of calculation involved doubles each time.

Calc does not remember computed values for 3D graphs. This means the
numerix prefix argument, if any, to `g p` is effectively ignored if
the current graph is three-dimensional.

The `g P` (`calc-graph-print`) command is like `g p`,
except that it sends the output to a printer instead of to the
screen. More precisely, `g p` looks for ‘`set terminal`’
or ‘`set output`’ commands in the `*Gnuplot Commands*` buffer;
lacking these it uses the default settings. However, `g P`
ignores ‘`set terminal`’ and ‘`set output`’ commands and
uses a different set of default values. All of these values are
controlled by the `g D` and `g O` commands discussed below.
Provided everything is set up properly, `g p` will plot to
the screen unless you have specified otherwise and `g P` will
always plot to the printer.

---

Next: [Graphical Devices](#Devices), Previous: [Managing Curves](#Managing-Curves), Up: [Graphics](#Graphics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 13.4 Graphics Options [¶](#Graphics-Options-1)

The `g g` (`calc-graph-grid`) command turns the “grid”
on and off. It is off by default; tick marks appear only at the
edges of the graph. With the grid turned on, dotted lines appear
across the graph at each tick mark. Note that this command only
changes the setting in `*Gnuplot Commands*`; to see the effects
of the change you must give another `g p` command.

The `g b` (`calc-graph-border`) command turns the border
(the box that surrounds the graph) on and off. It is on by default.
This command will only work with GNUPLOT 3.0 and later versions.

The `g k` (`calc-graph-key`) command turns the “key”
on and off. The key is a chart in the corner of the graph that
shows the correspondence between curves and line styles. It is
off by default, and is only really useful if you have several
curves on the same graph.

The `g N` (`calc-graph-num-points`) command allows you
to select the number of data points in the graph. This only affects
curves where neither “x” nor “y” is specified as a vector.
Enter a blank line to revert to the default value (initially 15).
With no prefix argument, this command affects only the current graph.
With a positive prefix argument this command changes or, if you enter
a blank line, displays the default number of points used for all
graphs created by `g a` that don’t specify the resolution explicitly.
With a negative prefix argument, this command changes or displays
the default value (initially 5) used for 3D graphs created by `g A`.
Note that a 3D setting of 5 means that a total of ‘`5^2 = 25`’ points
will be computed for the surface.

Data values in the graph of a function are normally computed to a
precision of five digits, regardless of the current precision at the
time. This is usually more than adequate, but there are cases where
it will not be. For example, plotting ‘`1 + x`’ with ‘`x`’ in the
interval ‘`[0 .. 1e-6]`’ will round all the data points down
to 1.0! Putting the command ‘`set precision n`’ in the
`*Gnuplot Commands*` buffer will cause the data to be computed
at precision n instead of 5. Since this is such a rare case,
there is no keystroke-based command to set the precision.

The `g h` (`calc-graph-header`) command sets the title
for the graph. This will show up centered above the graph.
The default title is blank (no title).

The `g n` (`calc-graph-name`) command sets the title of an
individual curve. Like the other curve-manipulating commands, it
affects the most recently added curve, i.e., the last curve on the
list in the `*Gnuplot Commands*` buffer. To set the title of
the other curves you must first juggle them to the end of the list
with `g j`, or edit the `*Gnuplot Commands*` buffer by hand.
Curve titles appear in the key; if the key is turned off they are
not used.

The `g t` (`calc-graph-title-x`) and `g T`
(`calc-graph-title-y`) commands set the titles on the “x”
and “y” axes, respectively. These titles appear next to the
tick marks on the left and bottom edges of the graph, respectively.
Calc does not have commands to control the tick marks themselves,
but you can edit them into the `*Gnuplot Commands*` buffer if
you wish. See the GNUPLOT documentation for details.

The `g r` (`calc-graph-range-x`) and `g R`
(`calc-graph-range-y`) commands set the range of values on the
“x” and “y” axes, respectively. You are prompted to enter a
suitable range. This should be either a pair of numbers of the
form, ‘`min:max`’, or a blank line to revert to the
default behavior of setting the range based on the range of values
in the data, or ‘`$`’ to take the range from the top of the stack.
Ranges on the stack can be represented as either interval forms or
vectors: ‘`[min .. max]`’ or ‘`[min, max]`’.

The `g l` (`calc-graph-log-x`) and `g L` (`calc-graph-log-y`)
commands allow you to set either or both of the axes of the graph to
be logarithmic instead of linear.

For 3D plots, `g C-t`, `g C-r`, and `g C-l` (those are
letters with the Control key held down) are the corresponding commands
for the “z” axis.

The `g z` (`calc-graph-zero-x`) and `g Z`
(`calc-graph-zero-y`) commands control whether a dotted line is
drawn to indicate the “x” and/or “y” zero axes. (These are the same
dotted lines that would be drawn there anyway if you used `g g` to
turn the “grid” feature on.) Zero-axis lines are on by default, and
may be turned off only in GNUPLOT 3.0 and later versions. They are
not available for 3D plots.

The `g s` (`calc-graph-line-style`) command turns the connecting
lines on or off for the most recently added curve, and optionally selects
the style of lines to be used for that curve. Plain `g s` simply
toggles the lines on and off. With a numeric prefix argument, `g s`
turns lines on and sets a particular line style. Line style numbers
start at one and their meanings vary depending on the output device.
GNUPLOT guarantees that there will be at least six different line styles
available for any device.

The `g S` (`calc-graph-point-style`) command similarly turns
the symbols at the data points on or off, or sets the point style.
If you turn both lines and points off, the data points will show as
tiny dots. If the “y” values being plotted contain error forms and
the connecting lines are turned off, then this command will also turn
the error bars on or off.

Another way to specify curve styles is with the `LineStyles` and
`PointStyles` variables. These variables initially have no stored
values, but if you store a vector of integers in one of these variables,
the `g a` and `g f` commands will use those style numbers
instead of the defaults for new curves that are added to the graph.
An entry should be a positive integer for a specific style, or 0 to let
the style be chosen automatically, or *-1* to turn off lines or points
altogether. If there are more curves than elements in the vector, the
last few curves will continue to have the default styles. Of course,
you can later use `g s` and `g S` to change any of these styles.

For example, `'[2 -1 3] RET s t LineStyles` causes the first curve
to have lines in style number 2, the second curve to have no connecting
lines, and the third curve to have lines in style 3. Point styles will
still be assigned automatically, but you could store another vector in
`PointStyles` to define them, too.

---

Previous: [Graphics Options](#Graphics-Options), Up: [Graphics](#Graphics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
