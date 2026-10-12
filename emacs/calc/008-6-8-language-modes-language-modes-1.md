### 6.8 Language Modes [¶](#Language-Modes-1)

The commands in this section change Calc to use a different notation for
entry and display of formulas, corresponding to the conventions of some
other common language such as Pascal or LaTeX. Objects displayed on the
stack or yanked from the Calculator to an editing buffer will be formatted
in the current language; objects entered in algebraic entry or yanked from
another buffer will be interpreted according to the current language.

The current language has no effect on things written to or read from the
trail buffer, nor does it affect numeric entry. Only algebraic entry is
affected. You can make even algebraic entry ignore the current language
and use the standard notation by giving a numeric prefix, e.g., `C-u '`.

For example, suppose the formula ‘`2*a[1] + atan(a[2])`’ occurs in a C
program; elsewhere in the program you need the derivatives of this formula
with respect to ‘`a[1]`’ and ‘`a[2]`’. First, type `d C`
to switch to C notation. Now use `C-u C-x * g` to grab the formula
into the Calculator, `a d a[1] RET` to differentiate with respect
to the first variable, and `C-x * y` to yank the formula for the derivative
back into your C program. Press `U` to undo the differentiation and
repeat with `a d a[2] RET` for the other derivative.

Without being switched into C mode first, Calc would have misinterpreted
the brackets in ‘`a[1]`’ and ‘`a[2]`’, would not have known that
`atan` was equivalent to Calc’s built-in `arctan` function,
and would have written the formula back with notations (like implicit
multiplication) which would not have been valid for a C program.

As another example, suppose you are maintaining a C program and a LaTeX
document, each of which needs a copy of the same formula. You can grab the
formula from the program in C mode, switch to LaTeX mode, and yank the
formula into the document in LaTeX math-mode format.

Language modes are selected by typing the letter `d` followed by a
shifted letter key.

* [Normal Language Modes](#Normal-Language-Modes)
* [C, FORTRAN, and Pascal Modes](#C-FORTRAN-Pascal)
* [TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes)
* [Eqn Language Mode](#Eqn-Language-Mode)
* [Yacas Language Mode](#Yacas-Language-Mode)
* [Maxima Language Mode](#Maxima-Language-Mode)
* [Giac Language Mode](#Giac-Language-Mode)
* [Mathematica Language Mode](#Mathematica-Language-Mode)
* [Maple Language Mode](#Maple-Language-Mode)
* [Compositions](#Compositions)
* [Syntax Tables](#Syntax-Tables)

---

Next: [C, FORTRAN, and Pascal Modes](#C-FORTRAN-Pascal), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.1 Normal Language Modes [¶](#Normal-Language-Modes-1)

The `d N` (`calc-normal-language`) command selects the usual
notation for Calc formulas, as described in the rest of this manual.
Matrices are displayed in a multi-line tabular format, but all other
objects are written in linear form, as they would be typed from the
keyboard.

The `d O` (`calc-flat-language`) command selects a language
identical with the normal one, except that matrices are written in
one-line form along with everything else. In some applications this
form may be more suitable for yanking data into other buffers.

Even in one-line mode, long formulas or vectors will still be split
across multiple lines if they exceed the width of the Calculator window.
The `d b` (`calc-line-breaking`) command turns this line-breaking
feature on and off. (It works independently of the current language.)
If you give a numeric prefix argument of five or greater to the `d b`
command, that argument will specify the line width used when breaking
long lines.

The `d B` (`calc-big-language`) command selects a language
which uses textual approximations to various mathematical notations,
such as powers, quotients, and square roots:

```
  ____________
 | a + 1    2
 | ----- + c
\|   b
```

in place of ‘`sqrt((a+1)/b + c^2)`’.

Subscripts like ‘`a_i`’ are displayed as actual subscripts in Big
mode. Double subscripts, ‘`a_i_j`’ (‘`subscr(subscr(a, i), j)`’)
are displayed as ‘`a`’ with subscripts separated by commas:
‘`i, j`’. They must still be entered in the usual underscore
notation.

One slight ambiguity of Big notation is that

```
  3
- -
  4
```

can represent either the negative rational number ‘`-3:4`’, or the
actual expression ‘`-(3/4)`’; but the latter formula would normally
never be displayed because it would immediately be evaluated to
‘`-3:4`’ or ‘`-0.75`’, so this ambiguity is not a problem in
typical use.

Non-decimal numbers are displayed with subscripts. Thus there is no
way to tell the difference between ‘`16#C2`’ and ‘`C2_16`’,
though generally you will know which interpretation is correct.
Logarithms ‘`log(x,b)`’ and ‘`log10(x)`’ also use subscripts
in Big mode.

In Big mode, stack entries often take up several lines. To aid
readability, stack entries are separated by a blank line in this mode.
You may find it useful to expand the Calc window’s height using
`C-x ^` (`enlarge-window`) or to make the Calc window the only
one on the screen with `C-x 1` (`delete-other-windows`).

Long lines are currently not rearranged to fit the window width in
Big mode, so you may need to use the `<` and `>` keys
to scroll across a wide formula. For really big formulas, you may
even need to use `{` and `}` to scroll up and down.

The `d U` (`calc-unformatted-language`) command altogether disables
the use of operator notation in formulas. In this mode, the formula
shown above would be displayed:

```
sqrt(add(div(add(a, 1), b), pow(c, 2)))
```

These four modes differ only in display format, not in the format
expected for algebraic entry. The standard Calc operators work in
all four modes, and unformatted notation works in any language mode
(except that Mathematica mode expects square brackets instead of
parentheses).

---

Next: [TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes), Previous: [Normal Language Modes](#Normal-Language-Modes), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.2 C, FORTRAN, and Pascal Modes [¶](#C_002c-FORTRAN_002c-and-Pascal-Modes)

The `d C` (`calc-c-language`) command selects the conventions
of the C language for display and entry of formulas. This differs from
the normal language mode in a variety of (mostly minor) ways. In
particular, C language operators and operator precedences are used in
place of Calc’s usual ones. For example, ‘`a^b`’ means ‘`xor(a,b)`’
in C mode; a value raised to a power is written as a function call,
‘`pow(a,b)`’.

In C mode, vectors and matrices use curly braces instead of brackets.
Octal and hexadecimal values are written with leading ‘`0`’ or ‘`0x`’
rather than using the ‘`#`’ symbol. Array subscripting is
translated into `subscr` calls, so that ‘`a[i]`’ in C
mode is the same as ‘`a_i`’ in Normal mode. Assignments
turn into the `assign` function, which Calc normally displays
using the ‘`:=`’ symbol.

The variables `pi` and `e` would be displayed ‘`pi`’
and ‘`e`’ in Normal mode, but in C mode they are displayed as
‘`M_PI`’ and ‘`M_E`’, corresponding to the names of constants
typically provided in the `<math.h>` header. Functions whose
names are different in C are translated automatically for entry and
display purposes. For example, entering ‘`asin(x)`’ will push the
formula ‘`arcsin(x)`’ onto the stack; this formula will be displayed
as ‘`asin(x)`’ as long as C mode is in effect.

The `d P` (`calc-pascal-language`) command selects Pascal
conventions. Like C mode, Pascal mode interprets array brackets and uses
a different table of operators. Hexadecimal numbers are entered and
displayed with a preceding dollar sign. (Thus the regular meaning of
`$2` during algebraic entry does not work in Pascal mode, though
`$` (and `$$`, etc.) not followed by digits works the same as
always.) No special provisions are made for other non-decimal numbers,
vectors, and so on, since there is no universally accepted standard way
of handling these in Pascal.

The `d F` (`calc-fortran-language`) command selects FORTRAN
conventions. Various function names are transformed into FORTRAN
equivalents. Vectors are written as ‘`/1, 2, 3/`’, and may be
entered this way or using square brackets. Since FORTRAN uses round
parentheses for both function calls and array subscripts, Calc displays
both in the same way; ‘`a(i)`’ is interpreted as a function call
upon reading, and subscripts must be entered as ‘`subscr(a, i)`’.
If the variable `a` has been declared to have type
`vector` or `matrix`, however, then ‘`a(i)`’ will be
parsed as a subscript. (See [Declarations](#Declarations).) Usually it doesn’t
matter, though; if you enter the subscript expression ‘`a(i)`’ and
Calc interprets it as a function call, you’ll never know the difference
unless you switch to another language mode or replace `a` with an
actual vector (or unless `a` happens to be the name of a built-in
function!).

Underscores are allowed in variable and function names in all of these
language modes. The underscore here is equivalent to the ‘`#`’ in
Normal mode, or to hyphens in the underlying Emacs Lisp variable names.

FORTRAN and Pascal modes normally do not adjust the case of letters in
formulas. Most built-in Calc names use lower-case letters. If you use a
positive numeric prefix argument with `d P` or `d F`, these
modes will use upper-case letters exclusively for display, and will
convert to lower-case on input. With a negative prefix, these modes
convert to lower-case for display and input.

---

Next: [Eqn Language Mode](#Eqn-Language-Mode), Previous: [C, FORTRAN, and Pascal Modes](#C-FORTRAN-Pascal), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.3 TeX and LaTeX Language Modes [¶](#TeX-and-LaTeX-Language-Modes-1)

The `d T` (`calc-tex-language`) command selects the conventions
of “math mode” in Donald Knuth’s TeX typesetting language,
and the `d L` (`calc-latex-language`) command selects the
conventions of “math mode” in LaTeX, a typesetting language that
uses TeX as its formatting engine. Calc’s LaTeX language mode can
read any formula that the TeX language mode can, although LaTeX
mode may display it differently.

Formulas are entered and displayed in the appropriate notation;
‘`sin(a/b)`’
will appear as ‘`\sin\left( {a \over b} \right)`’ in TeX mode and
‘`\sin\left(\frac{a}{b}\right)`’ in LaTeX mode.
Math formulas are often enclosed by ‘`$ $`’ signs in TeX and
LaTeX; these should be omitted when interfacing with Calc. To Calc,
the ‘`$`’ sign has the same meaning it always does in algebraic
formulas (a reference to an existing entry on the stack).

Complex numbers are displayed as in ‘`3 + 4i`’. Fractions and
quotients are written using `\over` in TeX mode (as in
`{a \over b}`) and `\frac` in LaTeX mode (as in
`\frac{a}{b}`); binomial coefficients are written with
`\choose` in TeX mode (as in `{a \choose b}`) and
`\binom` in LaTeX mode (as in `\binom{a}{b}`).
Interval forms are written with `\ldots`, and error forms are
written with `\pm`. Absolute values are written as in
‘`|x + 1|`’, and the floor and ceiling functions are written with
`\lfloor`, `\rfloor`, etc. The words `\left` and
`\right` are ignored when reading formulas in TeX and LaTeX
modes. Both `inf` and `uinf` are written as `\infty`;
when read, `\infty` always translates to `inf`.

Function calls are written the usual way, with the function name followed
by the arguments in parentheses. However, functions for which TeX
and LaTeX have special names (like `\sin`) will use curly braces
instead of parentheses for very simple arguments. During input, curly
braces and parentheses work equally well for grouping, but when the
document is formatted the curly braces will be invisible. Thus the
printed result is
‘`sin 2x`’
but
‘`sin(2 + x)`’.

The TeX specific unit names (see [Predefined Units](#Predefined-Units)) will not use
the ‘`tex`’ prefix; the unit name for a TeX point will be
‘`pt`’ instead of ‘`texpt`’, for example.

Function and variable names not treated specially by TeX and LaTeX
are simply written out as-is, which will cause them to come out in
italic letters in the printed document. If you invoke `d T` or
`d L` with a positive numeric prefix argument, names of more than
one character will instead be enclosed in a protective commands that
will prevent them from being typeset in the math italics; they will be
written ‘`\hbox{name}`’ in TeX mode and
‘`\text{name}`’ in LaTeX mode. The
‘`\hbox{ }`’ and ‘`\text{ }`’ notations are ignored during
reading. If you use a negative prefix argument, such function names are
written ‘`\name`’, and function names that begin with `\` during
reading have the `\` removed. (Note that in this mode, long
variable names are still written with `\hbox` or `\text`.
However, you can always make an actual variable name like `\bar` in
any TeX mode.)

During reading, text of the form ‘`\matrix{ ... }`’ is replaced
by ‘`[ ... ]`’. The same also applies to `\pmatrix` and
`\bmatrix`. In LaTeX mode this also applies to
‘`\begin{matrix} ... \end{matrix}`’,
‘`\begin{bmatrix} ... \end{bmatrix}`’,
‘`\begin{pmatrix} ... \end{pmatrix}`’, as well as
‘`\begin{smallmatrix} ... \end{smallmatrix}`’.
The symbol ‘`&`’ is interpreted as a comma,
and the symbols ‘`\cr`’ and ‘`\\`’ are interpreted as semicolons.
During output, matrices are displayed in ‘`\matrix{ a & b \\ c & d}`’
format in TeX mode and in
‘`\begin{pmatrix} a & b \\ c & d \end{pmatrix}`’ format in
LaTeX mode; you may need to edit this afterwards to change to your
preferred matrix form. If you invoke `d T` or `d L` with an
argument of 2 or −2, then matrices will be displayed in two-dimensional
form, such as

```
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
```

This may be convenient for isolated matrices, but could lead to
expressions being displayed like

```
\begin{pmatrix} \times x
a & b \\
c & d
\end{pmatrix}
```

While this wouldn’t bother Calc, it is incorrect LaTeX.
(Similarly for TeX.)

Accents like `\tilde` and `\bar` translate into function
calls internally (‘`tilde(x)`’, ‘`bar(x)`’). The `\underline`
sequence is treated as an accent. The `\vec` accent corresponds
to the function name `Vec`, because `vec` is the name of
a built-in Calc function. The following table shows the accents
in Calc, TeX, LaTeX and *eqn* (described in the next section):

```
Calc      TeX           LaTeX         eqn
----      ---           -----         ---
acute     \acute        \acute
Acute                   \Acute
bar       \bar          \bar          bar
Bar                     \Bar
breve     \breve        \breve
Breve                   \Breve
check     \check        \check
Check                   \Check
dddot                   \dddot
ddddot                  \ddddot
dot       \dot          \dot          dot
Dot                     \Dot
dotdot    \ddot         \ddot         dotdot
DotDot                  \Ddot
dyad                                  dyad
grave     \grave        \grave
Grave                   \Grave
hat       \hat          \hat          hat
Hat                     \Hat
Prime                                 prime
tilde     \tilde        \tilde        tilde
Tilde                   \Tilde
under     \underline    \underline    under
Vec       \vec          \vec          vec
VEC                     \Vec
```

The ‘`=>`’ (evaluates-to) operator appears as a `\to` symbol:
‘`{a \to b}`’. TeX defines `\to` as an
alias for `\rightarrow`. However, if the ‘`=>`’ is the
top-level expression being formatted, a slightly different notation
is used: ‘`\evalto a \to b`’. The `\evalto`
word is ignored by Calc’s input routines, and is undefined in TeX.
You will typically want to include one of the following definitions
at the top of a TeX file that uses `\evalto`:

```
\def\evalto{}
\def\evalto#1\to{}
```

The first definition formats evaluates-to operators in the usual
way. The second causes only the b part to appear in the
printed document; the a part and the arrow are hidden.
Another definition you may wish to use is ‘`\let\to=\Rightarrow`’
which causes `\to` to appear more like Calc’s ‘`=>`’ symbol.
See [The Evaluates-To Operator](#Evaluates_002dTo-Operator), for a discussion of `evalto`.

The complete set of TeX control sequences that are ignored during
reading is:

```
\hbox  \mbox  \text  \left  \right
\,  \>  \:  \;  \!  \quad  \qquad  \hfil  \hfill
\displaystyle  \textstyle  \dsize  \tsize
\scriptstyle  \scriptscriptstyle  \ssize  \ssize
\rm  \bf  \it  \sl  \roman  \bold  \italic  \slanted
\cal  \mit  \Cal  \Bbb  \frak  \goth
\evalto
```

Note that, because these symbols are ignored, reading a TeX or
LaTeX formula into Calc and writing it back out may lose spacing and
font information.

Also, the “discretionary multiplication sign” ‘`\*`’ is read
the same as ‘`*`’.

The TeX version of this manual includes some printed examples at the
end of this section.

---

Next: [Yacas Language Mode](#Yacas-Language-Mode), Previous: [TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.4 Eqn Language Mode [¶](#Eqn-Language-Mode-1)

*Eqn* is another popular formatter for math formulas. It is
designed for use with the TROFF text formatter, and comes standard
with many versions of Unix. The `d E` (`calc-eqn-language`)
command selects *eqn* notation.

The *eqn* language’s main idiosyncrasy is that whitespace plays
a significant part in the parsing of the language. For example,
‘`sqrt x+1 + y`’ treats ‘`x+1`’ as the argument of the
`sqrt` operator. *Eqn* also understands more conventional
grouping using curly braces: ‘`sqrt{x+1} + y`’. Braces are
required only when the argument contains spaces.

In Calc’s *eqn* mode, however, curly braces are required to
delimit arguments of operators like `sqrt`. The first of the
above examples would treat only the ‘`x`’ as the argument of
`sqrt`, and in fact ‘`sin x+1`’ would be interpreted as
‘`sin * x + 1`’, because `sin` is not a special operator
in the *eqn* language. If you always surround the argument
with curly braces, Calc will never misunderstand.

Calc also understands parentheses as grouping characters. Another
peculiarity of *eqn*’s syntax makes it advisable to separate
words with spaces from any surrounding characters that aren’t curly
braces, so Calc writes ‘`sin ( x + y )`’ in *eqn* mode.
(The spaces around `sin` are important to make *eqn*
recognize that `sin` should be typeset in a roman font, and
the spaces around `x` and `y` are a good idea just in
case the *eqn* document has defined special meanings for these
names, too.)

Powers and subscripts are written with the `sub` and `sup`
operators, respectively. Note that the caret symbol ‘`^`’ is
treated the same as a space in *eqn* mode, as is the ‘`~`’
symbol (these are used to introduce spaces of various widths into
the typeset output of *eqn*).

As in LaTeX mode, Calc’s formatter omits parentheses around the
arguments of functions like `ln` and `sin` if they are
“simple-looking”; in this case Calc surrounds the argument with
braces, separated by a ‘`~`’ from the function name: ‘`sin~{x}`’.

Font change codes (like ‘`roman x`’) and positioning codes
(like ‘`~`’ and ‘`down n x`’) are ignored by the
*eqn* reader. Also ignored are the words `left`, `right`,
`mark`, and `lineup`. Quotation marks in *eqn* mode input
are treated the same as curly braces: ‘`sqrt "1+x"`’ is equivalent to
‘`sqrt {1+x}`’; this is only an approximation to the true meaning
of quotes in *eqn*, but it is good enough for most uses.

Accent codes (‘`x dot`’) are handled by treating them as
function calls (‘`dot(x)`’) internally.
See [TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes), for a table of these accent
functions. The `prime` accent is treated specially if it occurs on
a variable or function name: ‘`f prime prime ( x prime )`’ is
stored internally as ‘`f''(x')`’. For example, taking the
derivative of ‘`f(2 x)`’ with `a d x` will produce ‘`2 f'(2
x)`’, which *eqn* mode will display as ‘`2 f prime ( 2 x )`’.

Assignments are written with the ‘`<-`’ (left-arrow) symbol,
and `evalto` operators are written with ‘`->`’ or
‘`evalto ... ->`’ (see [TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes), for a discussion
of this). The regular Calc symbols ‘`:=`’ and ‘`=>`’ are also
recognized for these operators during reading.

Vectors in *eqn* mode use regular Calc square brackets, but
matrices are formatted as ‘`matrix { ccol { a above b } ... }`’.
The words `lcol` and `rcol` are recognized as synonyms
for `ccol` during input, and are generated instead of `ccol`
if the matrix justification mode so specifies.

---

Next: [Maxima Language Mode](#Maxima-Language-Mode), Previous: [Eqn Language Mode](#Eqn-Language-Mode), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.5 Yacas Language Mode [¶](#Yacas-Language-Mode-1)

The `d Y` (`calc-yacas-language`) command selects the
conventions of Yacas, a free computer algebra system. While the
operators and functions in Yacas are similar to those of Calc, the names
of built-in functions in Yacas are capitalized. The Calc formula
‘`sin(2 x)`’, for example, is entered and displayed ‘`Sin(2 x)`’
in Yacas mode, and ‘`arcsin(x^2)`’ is ‘`ArcSin(x^2)`’ in Yacas
mode. Complex numbers are written are written ‘`3 + 4 I`’.
The standard special constants are written `Pi`, `E`,
`I`, `GoldenRatio` and `Gamma`. `Infinity`
represents both `inf` and `uinf`, and `Undefined`
represents `nan`.

Certain operators on functions, such as `D` for differentiation
and `Integrate` for integration, take a prefix form in Yacas. For
example, the derivative of ‘`e^x sin(x)`’ can be computed with
‘`D(x) Exp(x)*Sin(x)`’.

Other notable differences between Yacas and standard Calc expressions
are that vectors and matrices use curly braces in Yacas, and subscripts
use square brackets. If, for example, ‘`A`’ represents the list
‘`{a,2,c,4}`’, then ‘`A[3]`’ would equal ‘`c`’.

---

Next: [Giac Language Mode](#Giac-Language-Mode), Previous: [Yacas Language Mode](#Yacas-Language-Mode), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.6 Maxima Language Mode [¶](#Maxima-Language-Mode-1)

The `d X` (`calc-maxima-language`) command selects the
conventions of Maxima, another free computer algebra system. The
function names in Maxima are similar, but not always identical, to Calc.
For example, instead of ‘`arcsin(x)`’, Maxima will use
‘`asin(x)`’. Complex numbers are written ‘`3 + 4 %i`’. The
standard special constants are written `%pi`, `%e`,
`%i`, `%phi` and `%gamma`. In Maxima, `inf` means
the same as in Calc, but `infinity` represents Calc’s `uinf`.

Underscores as well as percent signs are allowed in function and
variable names in Maxima mode. The underscore again is equivalent to
the ‘`#`’ in Normal mode, and the percent sign is equivalent to
‘`o'o`’.

Maxima uses square brackets for lists and vectors, and matrices are
written as calls to the function `matrix`, given the row vectors of
the matrix as arguments. Square brackets are also used as subscripts.

---

Next: [Mathematica Language Mode](#Mathematica-Language-Mode), Previous: [Maxima Language Mode](#Maxima-Language-Mode), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.7 Giac Language Mode [¶](#Giac-Language-Mode-1)

The `d A` (`calc-giac-language`) command selects the
conventions of Giac, another free computer algebra system. The function
names in Giac are similar to Maxima. Complex numbers are written
‘`3 + 4 i`’. The standard special constants in Giac are the same as
in Calc, except that `infinity` represents both Calc’s `inf`
and `uinf`.

Underscores are allowed in function and variable names in Giac mode.
Brackets are used for subscripts. In Giac, indexing of lists begins at
0, instead of 1 as in Calc. So if ‘`A`’ represents the list
‘`[a,2,c,4]`’, then ‘`A[2]`’ would equal ‘`c`’. In general,
‘`A[n]`’ in Giac mode corresponds to ‘`A_(n+1)`’ in Normal mode.

The Giac interval notation ‘`2 .. 3`’ has no surrounding brackets;
Calc reads ‘`2 .. 3`’ as the closed interval ‘`[2 .. 3]`’ and
writes any kind of interval as ‘`2 .. 3`’. This means you cannot see
the difference between an open and a closed interval while in Giac mode.

---

Next: [Maple Language Mode](#Maple-Language-Mode), Previous: [Giac Language Mode](#Giac-Language-Mode), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.8 Mathematica Language Mode [¶](#Mathematica-Language-Mode-1)

The `d M` (`calc-mathematica-language`) command selects the
conventions of Mathematica. Notable differences in Mathematica mode
are that the names of built-in functions are capitalized, and function
calls use square brackets instead of parentheses. Thus the Calc
formula ‘`sin(2 x)`’ is entered and displayed ‘`Sin[2 x]`’ in
Mathematica mode.

Vectors and matrices use curly braces in Mathematica. Complex numbers
are written ‘`3 + 4 I`’. The standard special constants in Calc are
written `Pi`, `E`, `I`, `GoldenRatio`, `EulerGamma`,
`Infinity`, `ComplexInfinity`, and `Indeterminate` in
Mathematica mode.
Non-decimal numbers are written, e.g., ‘`16^^7fff`’. Floating-point
numbers in scientific notation are written ‘`1.23*10.^3`’.
Subscripts use double square brackets: ‘`a[[i]]`’.

---

Next: [Compositions](#Compositions), Previous: [Mathematica Language Mode](#Mathematica-Language-Mode), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.9 Maple Language Mode [¶](#Maple-Language-Mode-1)

The `d W` (`calc-maple-language`) command selects the
conventions of Maple.

Maple’s language is much like C. Underscores are allowed in symbol
names; square brackets are used for subscripts; explicit ‘`*`’s for
multiplications are required. Use either ‘`^`’ or ‘`**`’ to
denote powers.

Maple uses square brackets for lists and curly braces for sets. Calc
interprets both notations as vectors, and displays vectors with square
brackets. This means Maple sets will be converted to lists when they
pass through Calc. As a special case, matrices are written as calls
to the function `matrix`, given a list of lists as the argument,
and can be read in this form or with all-capitals `MATRIX`.

The Maple interval notation ‘`2 .. 3`’ is like Giac’s interval
notation, and is handled the same by Calc.

Maple writes complex numbers as ‘`3 + 4*I`’. Its special constants
are `Pi`, `E`, `I`, and `infinity` (all three of
`inf`, `uinf`, and `nan` display as `infinity`).
Floating-point numbers are written ‘`1.23*10.^3`’.

Among things not currently handled by Calc’s Maple mode are the
various quote symbols, procedures and functional operators, and
inert (‘`&`’) operators.

---

Next: [Syntax Tables](#Syntax-Tables), Previous: [Maple Language Mode](#Maple-Language-Mode), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.10 Compositions [¶](#Compositions-1)

There are several *composition functions* which allow you to get
displays in a variety of formats similar to those in Big language
mode. Most of these functions do not evaluate to anything; they are
placeholders which are left in symbolic form by Calc’s evaluator but
are recognized by Calc’s display formatting routines.

Two of these, `string` and `bstring`, are described elsewhere.
See [Strings](#Strings). For example, ‘`string("ABC")`’ is displayed as
‘`ABC`’. When viewed on the stack it will be indistinguishable from
the variable `ABC`, but internally it will be stored as
‘`string([65, 66, 67])`’ and can still be manipulated this way; for
example, the selection and vector commands `j 1 v v j u` would
select the vector portion of this object and reverse the elements, then
deselect to reveal a string whose characters had been reversed.

The composition functions do the same thing in all language modes
(although their components will of course be formatted in the current
language mode). The one exception is Unformatted mode (`d U`),
which does not give the composition functions any special treatment.
The functions are discussed here because of their relationship to
the language modes.

* [Composition Basics](#Composition-Basics)
* [Horizontal Compositions](#Horizontal-Compositions)
* [Vertical Compositions](#Vertical-Compositions)
* [Other Compositions](#Other-Compositions)
* [Information about Compositions](#Information-about-Compositions)
* [User-Defined Compositions](#User_002dDefined-Compositions)

---

Next: [Horizontal Compositions](#Horizontal-Compositions), Up: [Compositions](#Compositions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.10.1 Composition Basics [¶](#Composition-Basics-1)

Compositions are generally formed by stacking formulas together
horizontally or vertically in various ways. Those formulas are
themselves compositions. TeX users will find this analogous
to TeX’s “boxes.” Each multi-line composition has a
*baseline*; horizontal compositions use the baselines to
decide how formulas should be positioned relative to one another.
For example, in the Big mode formula

```
          2
     a + b
17 + ------
       c
```

the second term of the sum is four lines tall and has line three as
its baseline. Thus when the term is combined with 17, line three
is placed on the same level as the baseline of 17.

Another important composition concept is *precedence*. This is
an integer that represents the binding strength of various operators.
For example, ‘`*`’ has higher precedence (195) than ‘`+`’ (180),
which means that ‘`(a * b) + c`’ will be formatted without the
parentheses, but ‘`a * (b + c)`’ will keep the parentheses.

The operator table used by normal and Big language modes has the
following precedences:

```
_     1200    (subscripts)
%     1100    (as in n%)
!     1000    (as in !n)
mod    400
+/-    300
!!     210    (as in n!!)
!      210    (as in n!)
^      200
-      197    (as in -n)
*      195    (or implicit multiplication)
/ % \  190
+ -    180    (as in a+b)
|      170
< =    160    (and other relations)
&&     110
||     100
? :     90
!!!     85
&&&     80
|||     75
:=      50
::      45
=>      40
```

The general rule is that if an operator with precedence ‘`n`’
occurs as an argument to an operator with precedence ‘`m`’, then
the argument is enclosed in parentheses if ‘`n < m`’. Top-level
expressions and expressions which are function arguments, vector
components, etc., are formatted with precedence zero (so that they
normally never get additional parentheses).

For binary left-associative operators like ‘`+`’, the righthand
argument is actually formatted with one-higher precedence than shown
in the table. This makes sure ‘`(a + b) + c`’ omits the parentheses,
but the unnatural form ‘`a + (b + c)`’ keeps its parentheses.
Right-associative operators like ‘`^`’ format the lefthand argument
with one-higher precedence.

The `cprec` function formats an expression with an arbitrary
precedence. For example, ‘`cprec(abc, 185)`’ will combine into
sums and products as follows: ‘`7 + abc`’, ‘`7 (abc)`’ (because
this `cprec` form has higher precedence than addition, but lower
precedence than multiplication).

A final composition issue is *line breaking*. Calc uses two
different strategies for “flat” and “non-flat” compositions.
A non-flat composition is anything that appears on multiple lines
(not counting line breaking). Examples would be matrices and Big
mode powers and quotients. Non-flat compositions are displayed
exactly as specified. If they come out wider than the current
window, you must use horizontal scrolling (`<` and `>`) to
view them.

Flat compositions, on the other hand, will be broken across several
lines if they are too wide to fit the window. Certain points in a
composition are noted internally as *break points*. Calc’s
general strategy is to fill each line as much as possible, then to
move down to the next line starting at the first break point that
didn’t fit. However, the line breaker understands the hierarchical
structure of formulas. It will not break an “inner” formula if
it can use an earlier break point from an “outer” formula instead.
For example, a vector of sums might be formatted as:

```
[ a + b + c, d + e + f,
  g + h + i, j + k + l, m ]
```

If the ‘`m`’ can fit, then so, it seems, could the ‘`g`’.
But Calc prefers to break at the comma since the comma is part
of a “more outer” formula. Calc would break at a plus sign
only if it had to, say, if the very first sum in the vector had
itself been too large to fit.

Of the composition functions described below, only `choriz`
generates break points. The `bstring` function (see [Strings](#Strings))
also generates breakable items: A break point is added after every
space (or group of spaces) except for spaces at the very beginning or
end of the string.

Composition functions themselves count as levels in the formula
hierarchy, so a `choriz` that is a component of a larger
`choriz` will be less likely to be broken. As a special case,
if a `bstring` occurs as a component of a `choriz` or
`choriz`-like object (such as a vector or a list of arguments
in a function call), then the break points in that `bstring`
will be on the same level as the break points of the surrounding
object.

---

Next: [Vertical Compositions](#Vertical-Compositions), Previous: [Composition Basics](#Composition-Basics), Up: [Compositions](#Compositions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.10.2 Horizontal Compositions [¶](#Horizontal-Compositions-1)

The `choriz` function takes a vector of objects and composes
them horizontally. For example, ‘`choriz([17, a b/c, d])`’ formats
as ‘`17a b / cd`’ in Normal language mode, or as

```
  a b
17---d
   c
```

in Big language mode. This is actually one case of the general
function ‘`choriz(vec, sep, prec)`’, where
either or both of sep and prec may be omitted.
Prec gives the *precedence* to use when formatting
each of the components of vec. The default precedence is
the precedence from the surrounding environment.

Sep is a string (i.e., a vector of character codes as might
be entered with `" "` notation) which should separate components
of the composition. Also, if sep is given, the line breaker
will allow lines to be broken after each occurrence of sep.
If sep is omitted, the composition will not be breakable
(unless any of its component compositions are breakable).

For example, ‘`2 choriz([a, b c, d = e], " + ", 180)`’ is
formatted as ‘`2 a + b c + (d = e)`’. To get the `choriz`
to have precedence 180 “outwards” as well as “inwards,”
enclose it in a `cprec` form: ‘`2 cprec(choriz(...), 180)`’
formats as ‘`2 (a + b c + (d = e))`’.

The baseline of a horizontal composition is the same as the
baselines of the component compositions, which are all aligned.

---

Next: [Other Compositions](#Other-Compositions), Previous: [Horizontal Compositions](#Horizontal-Compositions), Up: [Compositions](#Compositions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.10.3 Vertical Compositions [¶](#Vertical-Compositions-1)

The `cvert` function makes a vertical composition. Each
component of the vector is centered in a column. The baseline of
the result is by default the top line of the resulting composition.
For example, ‘`f(cvert([a, bb, ccc]), cvert([a^2 + 1, b^2]))`’
formats in Big mode as

```
f( a ,  2    )
  bb   a  + 1
  ccc     2
         b
```

There are several special composition functions that work only as
components of a vertical composition. The `cbase` function
controls the baseline of the vertical composition; the baseline
will be the same as the baseline of whatever component is enclosed
in `cbase`. Thus ‘`f(cvert([a, cbase(bb), ccc]),
cvert([a^2 + 1, cbase(b^2)]))`’ displays as

```
        2
       a  + 1
   a      2
f(bb ,   b   )
  ccc
```

There are also `ctbase` and `cbbase` functions which
make the baseline of the vertical composition equal to the top
or bottom line (rather than the baseline) of that component.
Thus ‘`cvert([cbase(a / b)]) + cvert([ctbase(a / b)]) +
cvert([cbbase(a / b)])`’ gives

```
        a
a       -
- + a + b
b   -
    b
```

There should be only one `cbase`, `ctbase`, or `cbbase`
function in a given vertical composition. These functions can also
be written with no arguments: ‘`ctbase()`’ is a zero-height object
which means the baseline is the top line of the following item, and
‘`cbbase()`’ means the baseline is the bottom line of the preceding
item.

The `crule` function builds a “rule,” or horizontal line,
across a vertical composition. By itself ‘`crule()`’ uses ‘`-`’
characters to build the rule. You can specify any other character,
e.g., ‘`crule("=")`’. The argument must be a character code or
vector of exactly one character code. It is repeated to match the
width of the widest item in the stack. For example, a quotient
with a thick line is ‘`cvert([a + 1, cbase(crule("=")), b^2])`’:

```
a + 1
=====
  2
 b
```

Finally, the functions `clvert` and `crvert` act exactly
like `cvert` except that the items are left- or right-justified
in the stack. Thus ‘`clvert([a, bb, ccc]) + crvert([a, bb, ccc])`’
gives:

```
a   +   a
bb     bb
ccc   ccc
```

Like `choriz`, the vertical compositions accept a second argument
which gives the precedence to use when formatting the components.
Vertical compositions do not support separator strings.

---

Next: [Information about Compositions](#Information-about-Compositions), Previous: [Vertical Compositions](#Vertical-Compositions), Up: [Compositions](#Compositions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.10.4 Other Compositions [¶](#Other-Compositions-1)

The `csup` function builds a superscripted expression. For
example, ‘`csup(a, b)`’ looks the same as ‘`a^b`’ does in Big
language mode. This is essentially a horizontal composition of
‘`a`’ and ‘`b`’, where ‘`b`’ is shifted up so that its
bottom line is one above the baseline.

Likewise, the `csub` function builds a subscripted expression.
This shifts ‘`b`’ down so that its top line is one below the
bottom line of ‘`a`’ (note that this is not quite analogous to
`csup`). Other arrangements can be obtained by using
`choriz` and `cvert` directly.

The `cflat` function formats its argument in “flat” mode,
as obtained by ‘`d O`’, if the current language mode is normal
or Big. It has no effect in other language modes. For example,
‘`a^(b/c)`’ is formatted by Big mode like ‘`csup(a, cflat(b/c))`’
to improve its readability.

The `cspace` function creates horizontal space. For example,
‘`cspace(4)`’ is effectively the same as ‘`string(" ")`’.
A second string (i.e., vector of characters) argument is repeated
instead of the space character. For example, ‘`cspace(4, "ab")`’
looks like ‘`abababab`’. If the second argument is not a string,
it is formatted in the normal way and then several copies of that
are composed together: ‘`cspace(4, a^2)`’ yields

```
 2 2 2 2
a a a a
```

If the number argument is zero, this is a zero-width object.

The `cvspace` function creates vertical space, or a vertical
stack of copies of a certain string or formatted object. The
baseline is the center line of the resulting stack. A numerical
argument of zero will produce an object which contributes zero
height if used in a vertical composition.

There are also `ctspace` and `cbspace` functions which
create vertical space with the baseline the same as the baseline
of the top or bottom copy, respectively, of the second argument.
Thus ‘`cvspace(2, a/b) + ctspace(2, a/b) + cbspace(2, a/b)`’
displays as:

```
        a
        -
a       b
-   a   a
b + - + -
a   b   b
-   a
b   -
    b
```

---

Next: [User-Defined Compositions](#User_002dDefined-Compositions), Previous: [Other Compositions](#Other-Compositions), Up: [Compositions](#Compositions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.10.5 Information about Compositions [¶](#Information-about-Compositions-1)

The functions in this section are actual functions; they compose their
arguments according to the current language and other display modes,
then return a certain measurement of the composition as an integer.

The `cwidth` function measures the width, in characters, of a
composition. For example, ‘`cwidth(a + b)`’ is 5, and
‘`cwidth(a / b)`’ is 5 in Normal mode, 1 in Big mode, and 11 in
TeX mode (for ‘`{a \over b}`’). The argument may involve
the composition functions described in this section.

The `cheight` function measures the height of a composition.
This is the total number of lines in the argument’s printed form.

The functions `cascent` and `cdescent` measure the amount
of the height that is above (and including) the baseline, or below
the baseline, respectively. Thus ‘`cascent(x) + cdescent(x)`’
always equals ‘`cheight(x)`’. For a one-line formula like
‘`a + b`’, `cascent` returns 1 and `cdescent` returns 0.
For ‘`a / b`’ in Big mode, `cascent` returns 2 and `cdescent`
returns 1. The only formula for which `cascent` will return zero
is ‘`cvspace(0)`’ or equivalents.

---

Previous: [Information about Compositions](#Information-about-Compositions), Up: [Compositions](#Compositions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.10.6 User-Defined Compositions [¶](#User_002dDefined-Compositions-1)

The `Z C` (`calc-user-define-composition`) command lets you
define the display format for any algebraic function. You provide a
formula containing a certain number of argument variables on the stack.
Any time Calc formats a call to the specified function in the current
language mode and with that number of arguments, Calc effectively
replaces the function call with that formula with the arguments
replaced.

Calc builds the default argument list by sorting all the variable names
that appear in the formula into alphabetical order. You can edit this
argument list before pressing `RET` if you wish. Any variables in
the formula that do not appear in the argument list will be displayed
literally; any arguments that do not appear in the formula will not
affect the display at all.

You can define formats for built-in functions, for functions you have
defined with `Z F` (see [Programming with Formulas](#Algebraic-Definitions)), or for functions
which have no definitions but are being used as purely syntactic objects.
You can define different formats for each language mode, and for each
number of arguments, using a succession of `Z C` commands. When
Calc formats a function call, it first searches for a format defined
for the current language mode (and number of arguments); if there is
none, it uses the format defined for the Normal language mode. If
neither format exists, Calc uses its built-in standard format for that
function (usually just ‘`func(args)`’).

If you execute `Z C` with the number 0 on the stack instead of a
formula, any defined formats for the function in the current language
mode will be removed. The function will revert to its standard format.

For example, the default format for the binomial coefficient function
‘`choose(n, m)`’ in the Big language mode is

```
 n
( )
 m
```

You might prefer the notation,

```
 C
n m
```

To define this notation, first make sure you are in Big mode,
then put the formula

```
choriz([cvert([cvspace(1), n]), C, cvert([cvspace(1), m])])
```

on the stack and type `Z C`. Answer the first prompt with
`choose`. The second prompt will be the default argument list
of ‘`(C m n)`’. Edit this list to be ‘`(n m)`’ and press
`RET`. Now, try it out: For example, turn simplification
off with `m O` and enter ‘`choose(a,b) + choose(7,3)`’
as an algebraic entry.

```
 C  +  C
a b   7 3
```

As another example, let’s define the usual notation for Stirling
numbers of the first kind, ‘`stir1(n, m)`’. This is just like
the regular format for binomial coefficients but with square brackets
instead of parentheses.

```
choriz([string("["), cvert([n, cbase(cvspace(1)), m]), string("]")])
```

Now type `Z C stir1 RET`, edit the argument list to
‘`(n m)`’, and type `RET`.

The formula provided to `Z C` usually will involve composition
functions, but it doesn’t have to. Putting the formula ‘`a + b + c`’
onto the stack and typing `Z C foo RET RET` would define
the function ‘`foo(x,y,z)`’ to display like ‘`x + y + z`’.
This “sum” will act exactly like a real sum for all formatting
purposes (it will be parenthesized the same, and so on). However
it will be computationally unrelated to a sum. For example, the
formula ‘`2 * foo(1, 2, 3)`’ will display as ‘`2 (1 + 2 + 3)`’.
Operator precedences have caused the “sum” to be written in
parentheses, but the arguments have not actually been summed.
(Generally a display format like this would be undesirable, since
it can easily be confused with a real sum.)

The special function `eval` can be used inside a `Z C`
composition formula to cause all or part of the formula to be
evaluated at display time. For example, if the formula is
‘`a + eval(b + c)`’, then ‘`foo(1, 2, 3)`’ will be displayed
as ‘`1 + 5`’. Evaluation will use the default simplifications,
regardless of the current simplification mode. There are also
`evalsimp` and `evalextsimp` which simplify as if by
`a s` and `a e` (respectively). Note that these “functions”
operate only in the context of composition formulas (and also in
rewrite rules, where they serve a similar purpose; see [Rewrite Rules](#Rewrite-Rules)). On the stack, a call to `eval` will be left in
symbolic form.

It is not a good idea to use `eval` except as a last resort.
It can cause the display of formulas to be extremely slow. For
example, while ‘`eval(a + b)`’ might seem quite fast and simple,
there are several situations where it could be slow. For example,
‘`a`’ and/or ‘`b`’ could be polar complex numbers, in which
case doing the sum requires trigonometry. Or, ‘`a`’ could be
the factorial ‘`fact(100)`’ which is unevaluated because you
have typed `m O`; `eval` will evaluate it anyway to
produce a large, unwieldy integer.

You can save your display formats permanently using the `Z P`
command (see [Creating User Keys](#Creating-User-Keys)).

---

Previous: [Compositions](#Compositions), Up: [Language Modes](#Language-Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.11 Syntax Tables [¶](#Syntax-Tables-1)

Syntax tables do for input what compositions do for output: They
allow you to teach custom notations to Calc’s formula parser.
Calc keeps a separate syntax table for each language mode.

(Note that the Calc “syntax tables” discussed here are completely
unrelated to the syntax tables described in the Emacs manual.)

The `Z S` (`calc-edit-user-syntax`) command edits the
syntax table for the current language mode. If you want your
syntax to work in any language, define it in the Normal language
mode. Type `C-c C-c` to finish editing the syntax table, or
`C-x k` to cancel the edit. The `m m` command saves all
the syntax tables along with the other mode settings;
see [General Mode Commands](#General-Mode-Commands).

* [Syntax Table Basics](#Syntax-Table-Basics)
* [Precedence](#Precedence-in-Syntax-Tables)
* [Advanced Syntax Patterns](#Advanced-Syntax-Patterns)
* [Conditional Syntax Rules](#Conditional-Syntax-Rules)

---

Next: [Precedence](#Precedence-in-Syntax-Tables), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.11.1 Syntax Table Basics [¶](#Syntax-Table-Basics-1)

*Parsing* is the process of converting a raw string of characters,
such as you would type in during algebraic entry, into a Calc formula.
Calc’s parser works in two stages. First, the input is broken down
into *tokens*, such as words, numbers, and punctuation symbols
like ‘`+`’, ‘`:=`’, and ‘`+/-`’. Space between tokens is
ignored (except when it serves to separate adjacent words). Next,
the parser matches this string of tokens against various built-in
syntactic patterns, such as “an expression followed by ‘`+`’
followed by another expression” or “a name followed by ‘`(`’,
zero or more expressions separated by commas, and ‘`)`’.”

A *syntax table* is a list of user-defined *syntax rules*,
which allow you to specify new patterns to define your own
favorite input notations. Calc’s parser always checks the syntax
table for the current language mode, then the table for the Normal
language mode, before it uses its built-in rules to parse an
algebraic formula you have entered. Each syntax rule should go on
its own line; it consists of a *pattern*, a ‘`:=`’ symbol,
and a Calc formula with an optional *condition*. (Syntax rules
resemble algebraic rewrite rules, but the notation for patterns is
completely different.)

A syntax pattern is a list of tokens, separated by spaces.
Except for a few special symbols, tokens in syntax patterns are
matched literally, from left to right. For example, the rule,

```
foo ( ) := 2+3
```

would cause Calc to parse the formula ‘`4+foo()*5`’ as if it
were ‘`4+(2+3)*5`’. Notice that the parentheses were written
as two separate tokens in the rule. As a result, the rule works
for both ‘`foo()`’ and ‘`foo ( )`’. If we had written
the rule as ‘`foo () := 2+3`’, then Calc would treat ‘`()`’
as a single, indivisible token, so that ‘`foo( )`’ would
not be recognized by the rule. (It would be parsed as a regular
zero-argument function call instead.) In fact, this rule would
also make trouble for the rest of Calc’s parser: An unrelated
formula like ‘`bar()`’ would now be tokenized into ‘`bar ()`’
instead of ‘`bar ( )`’, so that the standard parser for function
calls would no longer recognize it!

While it is possible to make a token with a mixture of letters
and punctuation symbols, this is not recommended. It is better to
break it into several tokens, as we did with ‘`foo()`’ above.

The symbol ‘`#`’ in a syntax pattern matches any Calc expression.
On the righthand side, the things that matched the ‘`#`’s can
be referred to as ‘`#1`’, ‘`#2`’, and so on (where ‘`#1`’
matches the leftmost ‘`#`’ in the pattern). For example, these
rules match a user-defined function, prefix operator, infix operator,
and postfix operator, respectively:

```
foo ( # ) := myfunc(#1)
foo # := myprefix(#1)
# foo # := myinfix(#1,#2)
# foo := mypostfix(#1)
```

Thus ‘`foo(3)`’ will parse as ‘`myfunc(3)`’, and ‘`2+3 foo`’
will parse as ‘`mypostfix(2+3)`’.

It is important to write the first two rules in the order shown,
because Calc tries rules in order from first to last. If the
pattern ‘`foo #`’ came first, it would match anything that could
match the ‘`foo ( # )`’ rule, since an expression in parentheses
is itself a valid expression. Thus the ‘`foo ( # )`’ rule would
never get to match anything. Likewise, the last two rules must be
written in the order shown or else ‘`3 foo 4`’ will be parsed as
‘`mypostfix(3) * 4`’. (Of course, the best way to avoid these
ambiguities is not to use the same symbol in more than one way at
the same time! In case you’re not convinced, try the following
exercise: How will the above rules parse the input ‘`foo(3,4)`’,
if at all? Work it out for yourself, then try it in Calc and see.)

Calc is quite flexible about what sorts of patterns are allowed.
The only rule is that every pattern must begin with a literal
token (like ‘`foo`’ in the first two patterns above), or with
a ‘`#`’ followed by a literal token (as in the last two
patterns). After that, any mixture is allowed, although putting
two ‘`#`’s in a row will not be very useful since two
expressions with nothing between them will be parsed as one
expression that uses implicit multiplication.

As a more practical example, Maple uses the notation
‘`sum(a(i), i=1..10)`’ for sums, which Calc’s Maple mode doesn’t
recognize at present. To handle this syntax, we simply add the
rule,

```
sum ( # , # = # .. # ) := sum(#1,#2,#3,#4)
```

to the Maple mode syntax table. As another example, C mode can’t
read assignment operators like ‘`++`’ and ‘`*=`’. We can
define these operators quite easily:

```
# *= # := muleq(#1,#2)
# ++ := postinc(#1)
++ # := preinc(#1)
```

To complete the job, we would use corresponding composition functions
and `Z C` to cause these functions to display in their respective
Maple and C notations. (Note that the C example ignores issues of
operator precedence, which are discussed in the next section.)

You can enclose any token in quotes to prevent its usual
interpretation in syntax patterns:

```
# ":=" # := becomes(#1,#2)
```

Quotes also allow you to include spaces in a token, although once
again it is generally better to use two tokens than one token with
an embedded space. To include an actual quotation mark in a quoted
token, precede it with a backslash. (This also works to include
backslashes in tokens.)

```
# "bad token" # "/\"\\" # := silly(#1,#2,#3)
```

This will parse ‘`3 bad token 4 /"\ 5`’ to ‘`silly(3,4,5)`’.

The token `#` has a predefined meaning in Calc’s formula parser;
it is not valid to use ‘`"#"`’ in a syntax rule. However, longer
tokens that include the ‘`#`’ character are allowed. Also, while
‘`"$"`’ and ‘`"\""`’ are allowed as tokens, their presence in
the syntax table will prevent those characters from working in their
usual ways (referring to stack entries and quoting strings,
respectively).

Finally, the notation ‘`%%`’ anywhere in a syntax table causes
the rest of the line to be ignored as a comment.

---

Next: [Advanced Syntax Patterns](#Advanced-Syntax-Patterns), Previous: [Syntax Table Basics](#Syntax-Table-Basics), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.11.2 Precedence [¶](#Precedence)

Different operators are generally assigned different *precedences*.
By default, an operator defined by a rule like

```
# foo # := foo(#1,#2)
```

will have an extremely low precedence, so that ‘`2*3+4 foo 5 == 6`’
will be parsed as ‘`(2*3+4) foo (5 == 6)`’. To change the
precedence of an operator, use the notation ‘`#/p`’ in
place of ‘`#`’, where p is an integer precedence level.
For example, 185 lies between the precedences for ‘`+`’ and
‘`*`’, so if we change this rule to

```
#/185 foo #/186 := foo(#1,#2)
```

then ‘`2+3 foo 4*5`’ will be parsed as ‘`2+(3 foo (4*5))`’.
Also, because we’ve given the righthand expression slightly higher
precedence, our new operator will be left-associative:
‘`1 foo 2 foo 3`’ will be parsed as ‘`(1 foo 2) foo 3`’.
By raising the precedence of the lefthand expression instead, we
can create a right-associative operator.

See [Composition Basics](#Composition-Basics), for a table of precedences of the
standard Calc operators. For the precedences of operators in other
language modes, look in the Calc source file `calc-lang.el`.

---

Next: [Conditional Syntax Rules](#Conditional-Syntax-Rules), Previous: [Precedence](#Precedence-in-Syntax-Tables), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.11.3 Advanced Syntax Patterns [¶](#Advanced-Syntax-Patterns-1)

To match a function with a variable number of arguments, you could
write

```
foo ( # ) := myfunc(#1)
foo ( # , # ) := myfunc(#1,#2)
foo ( # , # , # ) := myfunc(#1,#2,#3)
```

but this isn’t very elegant. To match variable numbers of items,
Calc uses some notations inspired regular expressions and the
“extended BNF” style used by some language designers.

```
foo ( { # }*, ) := apply(myfunc,#1)
```

The token ‘`{`’ introduces a repeated or optional portion.
One of the three tokens ‘`}*`’, ‘`}+`’, or ‘`}?`’
ends the portion. These will match zero or more, one or more,
or zero or one copies of the enclosed pattern, respectively.
In addition, ‘`}*`’ and ‘`}+`’ can be followed by a
separator token (with no space in between, as shown above).
Thus ‘`{ # }*,`’ matches nothing, or one expression, or
several expressions separated by commas.

A complete ‘`{ ... }`’ item matches as a vector of the
items that matched inside it. For example, the above rule will
match ‘`foo(1,2,3)`’ to get ‘`apply(myfunc,[1,2,3])`’.
The Calc `apply` function takes a function name and a vector
of arguments and builds a call to the function with those
arguments, so the net result is the formula ‘`myfunc(1,2,3)`’.

If the body of a ‘`{ ... }`’ contains several ‘`#`’s
(or nested ‘`{ ... }`’ constructs), then the items will be
strung together into the resulting vector. If the body
does not contain anything but literal tokens, the result will
always be an empty vector.

```
foo ( { # , # }+, ) := bar(#1)
foo ( { { # }*, }*; ) := matrix(#1)
```

will parse ‘`foo(1, 2, 3, 4)`’ as ‘`bar([1, 2, 3, 4])`’, and
‘`foo(1, 2; 3, 4)`’ as ‘`matrix([[1, 2], [3, 4]])`’. Also, after
some thought it’s easy to see how this pair of rules will parse
‘`foo(1, 2, 3)`’ as ‘`matrix([[1, 2, 3]])`’, since the first
rule will only match an even number of arguments. The rule

```
foo ( # { , # , # }? ) := bar(#1,#2)
```

will parse ‘`foo(2,3,4)`’ as ‘`bar(2,[3,4])`’, and
‘`foo(2)`’ as ‘`bar(2,[])`’.

The notation ‘`{ ... }?.`’ (note the trailing period) works
just the same as regular ‘`{ ... }?`’, except that it does not
count as an argument; the following two rules are equivalent:

```
foo ( # , { also }? # ) := bar(#1,#3)
foo ( # , { also }?. # ) := bar(#1,#2)
```

Note that in the first case the optional text counts as ‘`#2`’,
which will always be an empty vector, but in the second case no
empty vector is produced.

Another variant is ‘`{ ... }?$`’, which means the body is
optional only at the end of the input formula. All built-in syntax
rules in Calc use this for closing delimiters, so that during
algebraic entry you can type `[sqrt(2), sqrt(3 RET`, omitting
the closing parenthesis and bracket. Calc does this automatically
for trailing ‘`)`’, ‘`]`’, and ‘`>`’ tokens in syntax
rules, but you can use ‘`{ ... }?$`’ explicitly to get
this effect with any token (such as ‘`"}"`’ or ‘`end`’).
Like ‘`{ ... }?.`’, this notation does not count as an
argument. Conversely, you can use quotes, as in ‘`")"`’, to
prevent a closing-delimiter token from being automatically treated
as optional.

Calc’s parser does not have full backtracking, which means some
patterns will not work as you might expect:

```
foo ( { # , }? # , # ) := bar(#1,#2,#3)
```

Here we are trying to make the first argument optional, so that
‘`foo(2,3)`’ parses as ‘`bar([],2,3)`’. Unfortunately, Calc
first tries to match ‘`2,`’ against the optional part of the
pattern, finds a match, and so goes ahead to match the rest of the
pattern. Later on it will fail to match the second comma, but it
doesn’t know how to go back and try the other alternative at that
point. One way to get around this would be to use two rules:

```
foo ( # , # , # ) := bar([#1],#2,#3)
foo ( # , # ) := bar([],#1,#2)
```

More precisely, when Calc wants to match an optional or repeated
part of a pattern, it scans forward attempting to match that part.
If it reaches the end of the optional part without failing, it
“finalizes” its choice and proceeds. If it fails, though, it
backs up and tries the other alternative. Thus Calc has “partial”
backtracking. A fully backtracking parser would go on to make sure
the rest of the pattern matched before finalizing the choice.

---

Previous: [Advanced Syntax Patterns](#Advanced-Syntax-Patterns), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 6.8.11.4 Conditional Syntax Rules [¶](#Conditional-Syntax-Rules-1)

It is possible to attach a *condition* to a syntax rule. For
example, the rules

```
foo ( # ) := ifoo(#1) :: integer(#1)
foo ( # ) := gfoo(#1)
```

will parse ‘`foo(3)`’ as ‘`ifoo(3)`’, but will parse
‘`foo(3.5)`’ and ‘`foo(x)`’ as calls to `gfoo`. Any
number of conditions may be attached; all must be true for the
rule to succeed. A condition is “true” if it evaluates to a
nonzero number. See [Logical Operations](#Logical-Operations), for a list of Calc
functions like `integer` that perform logical tests.

The exact sequence of events is as follows: When Calc tries a
rule, it first matches the pattern as usual. It then substitutes
‘`#1`’, ‘`#2`’, etc., in the conditions, if any. Next, the
conditions are simplified and evaluated in order from left to right,
using the algebraic simplifications (see [Simplifying Formulas](#Simplifying-Formulas)).
Each result is true if it is a nonzero number, or an expression
that can be proven to be nonzero (see [Declarations](#Declarations)). If the
results of all conditions are true, the expression (such as
‘`ifoo(#1)`’) has its ‘`#`’s substituted, and that is the
result of the parse. If the result of any condition is false, Calc
goes on to try the next rule in the syntax table.

Syntax rules also support `let` conditions, which operate in
exactly the same way as they do in algebraic rewrite rules.
See [Other Features of Rewrite Rules](#Other-Features-of-Rewrite-Rules), for details. A `let`
condition is always true, but as a side effect it defines a
variable which can be used in later conditions, and also in the
expression after the ‘`:=`’ sign:

```
foo ( # ) := hifoo(x) :: let(x := #1 + 0.5) :: dnumint(x)
```

The `dnumint` function tests if a value is numerically an
integer, i.e., either a true integer or an integer-valued float.
This rule will parse `foo` with a half-integer argument,
like ‘`foo(3.5)`’, to a call like ‘`hifoo(4.)`’.

The lefthand side of a syntax rule `let` must be a simple
variable, not the arbitrary pattern that is allowed in rewrite
rules.

The `matches` function is also treated specially in syntax
rule conditions (again, in the same way as in rewrite rules).
See [Matching Commands](#Matching-Commands). If the matching pattern contains
meta-variables, then those meta-variables may be used in later
conditions and in the result expression. The arguments to
`matches` are not evaluated in this situation.

```
sum ( # , # ) := sum(#1,a,b,c) :: matches(#2, a=[b..c])
```

This is another way to implement the Maple mode `sum` notation.
In this approach, we allow ‘`#2`’ to equal the whole expression
‘`i=1..10`’. Then, we use `matches` to break it apart into
its components. If the expression turns out not to match the pattern,
the syntax rule will fail. Note that `Z S` always uses Calc’s
Normal language mode for editing expressions in syntax rules, so we
must use regular Calc notation for the interval ‘`[b..c]`’ that
will correspond to the Maple mode interval ‘`1..10`’.

---

Next: [The Calc Mode Line](#Calc-Mode-Line), Previous: [Language Modes](#Language-Modes), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 6.9 The `Modes` Variable [¶](#The-Modes-Variable)

The `m g` (`calc-get-modes`) command pushes onto the stack
a vector of numbers that describes the various mode settings that
are in effect. With a numeric prefix argument, it pushes only the
nth mode, i.e., the nth element of this vector. Keyboard
macros can use the `m g` command to modify their behavior based
on the current mode settings.

The modes vector is also available in the special variable
`Modes`. In other words, `m g` is like `s r Modes RET`.
It will not work to store into this variable; in fact, if you do,
`Modes` will cease to track the current modes. (The `m g`
command will continue to work, however.)

In general, each number in this vector is suitable as a numeric
prefix argument to the associated mode-setting command. (Recall
that the `~` key takes a number from the stack and gives it as
a numeric prefix to the next command.)

The elements of the modes vector are as follows:

1. Current precision. Default is 12; associated command is `p`.
2. Binary word size. Default is 32; associated command is `b w`.
3. Stack size (not counting the value about to be pushed by `m g`).
   This is zero if `m g` is executed with an empty stack.
4. Number radix. Default is 10; command is `d r`.
5. Floating-point format. This is the number of digits, plus the
   constant 0 for normal notation, 10000 for scientific notation,
   20000 for engineering notation, or 30000 for fixed-point notation.
   These codes are acceptable as prefix arguments to the `d n`
   command, but note that this may lose information: For example,
   `d s` and `C-u 12 d s` have similar (but not quite
   identical) effects if the current precision is 12, but they both
   produce a code of 10012, which will be treated by `d n` as
   `C-u 12 d s`. If the precision then changes, the float format
   will still be frozen at 12 significant figures.
6. Angular mode. Default is 1 (degrees). Other values are 2 (radians)
   and 3 (HMS). The `m d` command accepts these prefixes.
7. Symbolic mode. Value is 0 or 1; default is 0. Command is `m s`.
8. Fraction mode. Value is 0 or 1; default is 0. Command is `m f`.
9. Polar mode. Value is 0 (rectangular) or 1 (polar); default is 0.
   Command is `m p`.
10. Matrix/Scalar mode. Default value is *-1*. Value is 0 for Scalar
    mode, *-2* for Matrix mode, *-3* for square Matrix mode,
    or N for
    NxN
    Matrix mode. Command is `m v`.
11. Simplification mode. Default is 1. Value is *-1* for off (`m O`),
    0 for `m N`, 2 for `m B`, 3 for `m A`, 4 for `m E`,
    or 5 for `m U`. The `m D` command accepts these prefixes.
12. Infinite mode. Default is *-1* (off). Value is 1 if the mode is on,
    or 0 if the mode is on with positive zeros. Command is `m i`.

For example, the sequence `M-1 m g RET 2 + ~ p` increases the
precision by two, leaving a copy of the old precision on the stack.
Later, `~ p` will restore the original precision using that
stack value. (This sequence might be especially useful inside a
keyboard macro.)

As another example, `M-3 m g 1 - ~ DEL` deletes all but the
oldest (bottommost) stack entry.

Yet another example: The HP-48 “round” command rounds a number
to the current displayed precision. You could roughly emulate this
in Calc with the sequence `M-5 m g 10000 % ~ c c`. (This
would not work for fixed-point mode, but it wouldn’t be hard to
do a full emulation with the help of the `Z [` and `Z ]`
programming commands. See [Conditionals in Keyboard Macros](#Conditionals-in-Macros).)

---

Previous: [The `Modes` Variable](#Modes-Variable), Up: [Mode Settings](#Mode-Settings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 6.10 The Calc Mode Line [¶](#The-Calc-Mode-Line)

This section is a summary of all symbols that can appear on the
Calc mode line, the highlighted bar that appears under the Calc
stack window (or under an editing window in Embedded mode).

The basic mode line format is:

```
--%*-Calc: 12 Deg other modes       (Calculator)
```

The ‘`%*`’ indicates that the buffer is “read-only”; it shows that
regular Emacs commands are not allowed to edit the stack buffer
as if it were text.

The word ‘`Calc:`’ changes to ‘`CalcEmbed:`’ if Embedded mode
is enabled. The words after this describe the various Calc modes
that are in effect.

The first mode is always the current precision, an integer.
The second mode is always the angular mode, either `Deg`,
`Rad`, or `Hms`.

Here is a complete list of the remaining symbols that can appear
on the mode line:

`Alg`
:   Algebraic mode (`m a`; see [Algebraic Entry](#Algebraic-Entry)).

`Alg[(`
:   Incomplete algebraic mode (`C-u m a`).

`Alg*`
:   Total algebraic mode (`m t`).

`Symb`
:   Symbolic mode (`m s`; see [Symbolic Mode](#Symbolic-Mode)).

`Matrix`
:   Matrix mode (`m v`; see [Matrix and Scalar Modes](#Matrix-Mode)).

`Matrixn`
:   Dimensioned Matrix mode (`C-u n m v`; see [Matrix and Scalar Modes](#Matrix-Mode)).

`SqMatrix`
:   Square Matrix mode (`C-u m v`; see [Matrix and Scalar Modes](#Matrix-Mode)).

`Scalar`
:   Scalar mode (`m v`; see [Matrix and Scalar Modes](#Matrix-Mode)).

`Polar`
:   Polar complex mode (`m p`; see [Polar Mode](#Polar-Mode)).

`Frac`
:   Fraction mode (`m f`; see [Fraction Mode](#Fraction-Mode)).

`Inf`
:   Infinite mode (`m i`; see [Infinite Mode](#Infinite-Mode)).

`+Inf`
:   Positive Infinite mode (`C-u 0 m i`).

`NoSimp`
:   Default simplifications off (`m O`; see [Simplification Modes](#Simplification-Modes)).

`NumSimp`
:   Default simplifications for numeric arguments only (`m N`).

`BinSimpw`
:   Binary-integer simplification mode; word size w (`m B`, `b w`).

`BasicSimp`
:   Basic simplification mode (`m I`).

`ExtSimp`
:   Extended algebraic simplification mode (`m E`).

`UnitSimp`
:   Units simplification mode (`m U`).

`Bin`
:   Current radix is 2 (`d 2`; see [Radix Modes](#Radix-Modes)).

`Oct`
:   Current radix is 8 (`d 8`).

`Hex`
:   Current radix is 16 (`d 6`).

`Radixn`
:   Current radix is n (`d r`).

`Zero`
:   Leading zeros (`d z`; see [Radix Modes](#Radix-Modes)).

`Big`
:   Big language mode (`d B`; see [Normal Language Modes](#Normal-Language-Modes)).

`Flat`
:   One-line normal language mode (`d O`).

`Unform`
:   Unformatted language mode (`d U`).

`C`
:   C language mode (`d C`; see [C, FORTRAN, and Pascal Modes](#C-FORTRAN-Pascal)).

`Pascal`
:   Pascal language mode (`d P`).

`Fortran`
:   FORTRAN language mode (`d F`).

`TeX`
:   TeX language mode (`d T`; see [TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes)).

`LaTeX`
:   LaTeX language mode (`d L`; see [TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes)).

`Eqn`
:   *Eqn* language mode (`d E`; see [Eqn Language Mode](#Eqn-Language-Mode)).

`Math`
:   Mathematica language mode (`d M`; see [Mathematica Language Mode](#Mathematica-Language-Mode)).

`Maple`
:   Maple language mode (`d W`; see [Maple Language Mode](#Maple-Language-Mode)).

`Normn`
:   Normal float mode with n digits (`d n`; see [Float Formats](#Float-Formats)).

`Fixn`
:   Fixed point mode with n digits after the point (`d f`).

`Sci`
:   Scientific notation mode (`d s`).

`Scin`
:   Scientific notation with n digits (`d s`).

`Eng`
:   Engineering notation mode (`d e`).

`Engn`
:   Engineering notation with n digits (`d e`).

`Leftn`
:   Left-justified display indented by n (`d <`; see [Justification](#Justification)).

`Right`
:   Right-justified display (`d >`).

`Rightn`
:   Right-justified display with width n (`d >`).

`Center`
:   Centered display (`d =`).

`Centern`
:   Centered display with center column n (`d =`).

`Widn`
:   Line breaking with width n (`d b`; see [Normal Language Modes](#Normal-Language-Modes)).

`Wide`
:   No line breaking (`d b`).

`Break`
:   Selections show deep structure (`j b`; see [Making Selections](#Making-Selections)).

`Save`
:   Record modes in `~/.emacs.d/calc.el` (`m R`; see [General Mode Commands](#General-Mode-Commands)).

`Local`
:   Record modes in Embedded buffer (`m R`).

`LocEdit`
:   Record modes as editing-only in Embedded buffer (`m R`).

`LocPerm`
:   Record modes as permanent-only in Embedded buffer (`m R`).

`Global`
:   Record modes as global in Embedded buffer (`m R`).

`Manual`
:   Automatic recomputation turned off (`m C`; see [Automatic Recomputation](#Automatic-Recomputation)).

`Graph`
:   GNUPLOT process is alive in background (see [Graphics](#Graphics)).

`Sel`
:   Top-of-stack has a selection (Embedded only; see [Making Selections](#Making-Selections)).

`Dirty`
:   The stack display may not be up-to-date (see [Display Modes](#Display-Modes)).

`Inv`
:   “Inverse” prefix was pressed (`I`; see [Inverse and Hyperbolic Flags](#Inverse-and-Hyperbolic)).

`Hyp`
:   “Hyperbolic” prefix was pressed (`H`).

`Keep`
:   “Keep-arguments” prefix was pressed (`K`).

`Narrow`
:   Stack is truncated (`d t`; see [Truncating the Stack](#Truncating-the-Stack)).

In addition, the symbols `Active` and `~Active` can appear
as minor modes on an Embedded buffer’s mode line. See [Embedded Mode](#Embedded-Mode).

---

Next: [Scientific Functions](#Scientific-Functions), Previous: [Mode Settings](#Mode-Settings), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 7 Arithmetic Functions [¶](#Arithmetic-Functions)

This chapter describes the Calc commands for doing simple calculations
on numbers, such as addition, absolute value, and square roots. These
commands work by removing the top one or two values from the stack,
performing the desired operation, and pushing the result back onto the
stack. If the operation cannot be performed, the result pushed is a
formula instead of a number, such as ‘`2/0`’ (because division by zero
is invalid) or ‘`sqrt(x)`’ (because the argument ‘`x`’ is a formula).

Most of the commands described here can be invoked by a single keystroke.
Some of the more obscure ones are two-letter sequences beginning with
the `f` (“functions”) prefix key.

See [Numeric Prefix Arguments](#Prefix-Arguments), for a discussion of the effect of numeric
prefix arguments on commands in this chapter which do not otherwise
interpret a prefix argument.

* [Basic Arithmetic](#Basic-Arithmetic)
* [Integer Truncation](#Integer-Truncation)
* [Complex Number Functions](#Complex-Number-Functions)
* [Conversions](#Conversions)
* [Date Arithmetic](#Date-Arithmetic)
* [Financial Functions](#Financial-Functions)
* [Binary Number Functions](#Binary-Functions)

---

Next: [Integer Truncation](#Integer-Truncation), Up: [Arithmetic Functions](#Arithmetic)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
