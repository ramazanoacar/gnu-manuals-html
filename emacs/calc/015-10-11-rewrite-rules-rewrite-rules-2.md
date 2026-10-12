### 10.11 Rewrite Rules [¶](#Rewrite-Rules-2)

The `a r` (`calc-rewrite`) [`rewrite`] command makes
substitutions in a formula according to a specified pattern or patterns
known as *rewrite rules*. Whereas `a b` (`calc-substitute`)
matches literally, so that substituting ‘`sin(x)`’ with ‘`cos(x)`’
matches only the `sin` function applied to the variable `x`,
rewrite rules match general kinds of formulas; rewriting using the rule
‘`sin(x) := cos(x)`’ matches `sin` of any argument and replaces
it with `cos` of that same argument. The only significance of the
name `x` is that the same name is used on both sides of the rule.

Rewrite rules rearrange formulas already in Calc’s memory.
See [Syntax Tables](#Syntax-Tables), to read about *syntax rules*, which are
similar to algebraic rewrite rules but operate when new algebraic
entries are being parsed, converting strings of characters into
Calc formulas.

* [Entering Rewrite Rules](#Entering-Rewrite-Rules)
* [Basic Rewrite Rules](#Basic-Rewrite-Rules)
* [Conditional Rewrite Rules](#Conditional-Rewrite-Rules)
* [Algebraic Properties of Rewrite Rules](#Algebraic-Properties-of-Rewrite-Rules)
* [Other Features of Rewrite Rules](#Other-Features-of-Rewrite-Rules)
* [Composing Patterns in Rewrite Rules](#Composing-Patterns-in-Rewrite-Rules)
* [Nested Formulas with Rewrite Rules](#Nested-Formulas-with-Rewrite-Rules)
* [Multi-Phase Rewrite Rules](#Multi_002dPhase-Rewrite-Rules)
* [Selections with Rewrite Rules](#Selections-with-Rewrite-Rules)
* [Matching Commands](#Matching-Commands)
* [Automatic Rewrites](#Automatic-Rewrites)
* [Debugging Rewrites](#Debugging-Rewrites)
* [Examples of Rewrite Rules](#Examples-of-Rewrite-Rules)

---

Next: [Basic Rewrite Rules](#Basic-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.1 Entering Rewrite Rules [¶](#Entering-Rewrite-Rules-1)

Rewrite rules normally use the “assignment” operator
‘`old := new`’.
This operator is equivalent to the function call ‘`assign(old, new)`’.
The `assign` function is undefined by itself in Calc, so an
assignment formula such as a rewrite rule will be left alone by ordinary
Calc commands. But certain commands, like the rewrite system, interpret
assignments in special ways.

For example, the rule ‘`sin(x)^2 := 1-cos(x)^2`’ says to replace
every occurrence of the sine of something, squared, with one minus the
square of the cosine of that same thing. All by itself as a formula
on the stack it does nothing, but when given to the `a r` command
it turns that command into a sine-squared-to-cosine-squared converter.

To specify a set of rules to be applied all at once, make a vector of
rules.

When `a r` prompts you to enter the rewrite rules, you can answer
in several ways:

1. With a rule: `f(x) := g(x) RET`.
2. With a vector of rules: `[f1(x) := g1(x), f2(x) := g2(x)] RET`.
   (You can omit the enclosing square brackets if you wish.)
3. With the name of a variable that contains the rule or rules vector:
   `myrules RET`.
4. With any formula except a rule, a vector, or a variable name; this
   will be interpreted as the old half of a rewrite rule,
   and you will be prompted a second time for the new half:
   `f(x) RET g(x) RET`.
5. With a blank line, in which case the rule, rules vector, or variable
   will be taken from the top of the stack (and the formula to be
   rewritten will come from the second-to-top position).

If you enter the rules directly (as opposed to using rules stored
in a variable), those rules will be put into the Trail so that you
can retrieve them later. See [Trail Commands](#Trail-Commands).

It is most convenient to store rules you use often in a variable and
invoke them by giving the variable name. The `s e`
(`calc-edit-variable`) command is an easy way to create or edit a
rule set stored in a variable. You may also wish to use `s p`
(`calc-permanent-variable`) to save your rules permanently;
see [Other Operations on Variables](#Operations-on-Variables).

Rewrite rules are compiled into a special internal form for faster
matching. If you enter a rule set directly it must be recompiled
every time. If you store the rules in a variable and refer to them
through that variable, they will be compiled once and saved away
along with the variable for later reference. This is another good
reason to store your rules in a variable.

Calc also accepts an obsolete notation for rules, as vectors
‘`[old, new]`’. But because it is easily confused with a
vector of two rules, the use of this notation is no longer recommended.

---

Next: [Conditional Rewrite Rules](#Conditional-Rewrite-Rules), Previous: [Entering Rewrite Rules](#Entering-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.2 Basic Rewrite Rules [¶](#Basic-Rewrite-Rules-1)

To match a particular formula ‘`x`’ with a particular rewrite rule
‘`old := new`’, Calc compares the structure of ‘`x`’ with
the structure of old. Variables that appear in old are
treated as *meta-variables*; the corresponding positions in ‘`x`’
may contain any sub-formulas. For example, the pattern ‘`f(x,y)`’
would match the expression ‘`f(12, a+1)`’ with the meta-variable
‘`x`’ corresponding to 12 and with ‘`y`’ corresponding to
‘`a+1`’. However, this pattern would not match ‘`f(12)`’ or
‘`g(12, a+1)`’, since there is no assignment of the meta-variables
that will make the pattern match these expressions. Notice that if
the pattern is a single meta-variable, it will match any expression.

If a given meta-variable appears more than once in old, the
corresponding sub-formulas of ‘`x`’ must be identical. Thus
the pattern ‘`f(x,x)`’ would match ‘`f(12, 12)`’ and
‘`f(a+1, a+1)`’ but not ‘`f(12, a+1)`’ or ‘`f(a+b, b+a)`’.
(See [Conditional Rewrite Rules](#Conditional-Rewrite-Rules), for a way to match the latter.)

Things other than variables must match exactly between the pattern
and the target formula. To match a particular variable exactly, use
the pseudo-function ‘`quote(v)`’ in the pattern. For example, the
pattern ‘`x+quote(y)`’ matches ‘`x+y`’, ‘`2+y`’, or
‘`sin(a)+y`’.

The special variable names ‘`e`’, ‘`pi`’, ‘`i`’, ‘`phi`’,
‘`gamma`’, ‘`inf`’, ‘`uinf`’, and ‘`nan`’ always match
literally. Thus the pattern ‘`sin(d + e + f)`’ acts exactly like
‘`sin(d + quote(e) + f)`’.

If the old pattern is found to match a given formula, that
formula is replaced by new, where any occurrences in new
of meta-variables from the pattern are replaced with the sub-formulas
that they matched. Thus, applying the rule ‘`f(x,y) := g(y+x,x)`’
to ‘`f(12, a+1)`’ would produce ‘`g(a+13, 12)`’.

The normal `a r` command applies rewrite rules over and over
throughout the target formula until no further changes are possible
(up to a limit of 100 times). Use `C-u 1 a r` to make only one
change at a time.

---

Next: [Algebraic Properties of Rewrite Rules](#Algebraic-Properties-of-Rewrite-Rules), Previous: [Basic Rewrite Rules](#Basic-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.3 Conditional Rewrite Rules [¶](#Conditional-Rewrite-Rules-1)

A rewrite rule can also be *conditional*, written in the form
‘`old := new :: cond`’. (There is also the obsolete
form ‘`[old, new, cond]`’.) If a cond part
is present in the
rule, this is an additional condition that must be satisfied before
the rule is accepted. Once old has been successfully matched
to the target expression, cond is evaluated (with all the
meta-variables substituted for the values they matched) and simplified
with Calc’s algebraic simplifications. If the result is a nonzero
number or any other object known to be nonzero (see [Declarations](#Declarations)),
the rule is accepted. If the result is zero or if it is a symbolic
formula that is not known to be nonzero, the rule is rejected.
See [Logical Operations](#Logical-Operations), for a number of functions that return
1 or 0 according to the results of various tests.

For example, the formula ‘`n > 0`’ simplifies to 1 or 0 if ‘`n`’
is replaced by a positive or nonpositive number, respectively (or if
‘`n`’ has been declared to be positive or nonpositive). Thus,
the rule ‘`f(x,y) := g(y+x,x) :: x+y > 0`’ would apply to
‘`f(0, 4)`’ but not to ‘`f(-3, 2)`’ or ‘`f(12, a+1)`’
(assuming no outstanding declarations for ‘`a`’). In the case of
‘`f(-3, 2)`’, the condition can be shown not to be satisfied; in
the case of ‘`f(12, a+1)`’, the condition merely cannot be shown
to be satisfied, but that is enough to reject the rule.

While Calc will use declarations to reason about variables in the
formula being rewritten, declarations do not apply to meta-variables.
For example, the rule ‘`f(a) := g(a+1)`’ will match for any values
of ‘`a`’, such as complex numbers, vectors, or formulas, even if
‘`a`’ has been declared to be real or scalar. If you want the
meta-variable ‘`a`’ to match only literal real numbers, use
‘`f(a) := g(a+1) :: real(a)`’. If you want ‘`a`’ to match only
reals and formulas which are provably real, use ‘`dreal(a)`’ as
the condition.

The ‘`::`’ operator is a shorthand for the `condition`
function; ‘`old := new :: cond`’ is equivalent to
the formula ‘`condition(assign(old, new), cond)`’.

If you have several conditions, you can use ‘`... :: c1 :: c2 :: c3`’
or ‘`... :: c1 && c2 && c3`’. The two are entirely equivalent.

It is also possible to embed conditions inside the pattern:
‘`f(x :: x>0, y) := g(y+x, x)`’. This is purely a notational
convenience, though; where a condition appears in a rule has no
effect on when it is tested. The rewrite-rule compiler automatically
decides when it is best to test each condition while a rule is being
matched.

Certain conditions are handled as special cases by the rewrite rule
system and are tested very efficiently: Where ‘`x`’ is any
meta-variable, these conditions are ‘`integer(x)`’, ‘`real(x)`’,
‘`constant(x)`’, ‘`negative(x)`’, ‘`x >= y`’ where ‘`y`’
is either a constant or another meta-variable and ‘`>=`’ may be
replaced by any of the six relational operators, and ‘`x % a = b`’
where ‘`a`’ and ‘`b`’ are constants. Other conditions, like
‘`x >= y+1`’ or ‘`dreal(x)`’, will be less efficient to check
since Calc must bring the whole evaluator and simplifier into play.

An interesting property of ‘`::`’ is that neither of its arguments
will be touched by Calc’s default simplifications. This is important
because conditions often are expressions that cannot safely be
evaluated early. For example, the `typeof` function never
remains in symbolic form; entering ‘`typeof(a)`’ will put the
number 100 (the type code for variables like ‘`a`’) on the stack.
But putting the condition ‘`... :: typeof(a) = 6`’ on the stack
is safe since ‘`::`’ prevents the `typeof` from being
evaluated until the condition is actually used by the rewrite system.

Since ‘`::`’ protects its lefthand side, too, you can use a dummy
condition to protect a rule that must itself not evaluate early.
For example, it’s not safe to put ‘`a(f,x) := apply(f, [x])`’ on
the stack because it will immediately evaluate to ‘`a(f,x) := f(x)`’,
where the meta-variable-ness of `f` on the righthand side has been
lost. But ‘`a(f,x) := apply(f, [x]) :: 1`’ is safe, and of course
the condition ‘`1`’ is always true (nonzero) so it has no effect on
the functioning of the rule. (The rewrite compiler will ensure that
it doesn’t even impact the speed of matching the rule.)

---

Next: [Other Features of Rewrite Rules](#Other-Features-of-Rewrite-Rules), Previous: [Conditional Rewrite Rules](#Conditional-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.4 Algebraic Properties of Rewrite Rules [¶](#Algebraic-Properties-of-Rewrite-Rules-1)

The rewrite mechanism understands the algebraic properties of functions
like ‘`+`’ and ‘`*`’. In particular, pattern matching takes
the associativity and commutativity of the following functions into
account:

```
+ - *  = !=  && ||  and or xor  vint vunion vxor  gcd lcm  max min  beta
```

For example, the rewrite rule:

```
a x + b x  :=  (a + b) x
```

will match formulas of the form,

```
a x + b x,  x a + x b,  a x + x b,  x a + b x
```

Rewrites also understand the relationship between the ‘`+`’ and ‘`-`’
operators. The above rewrite rule will also match the formulas,

```
a x - b x,  x a - x b,  a x - x b,  x a - b x
```

by matching ‘`b`’ in the pattern to ‘`-b`’ from the formula.

Applied to a sum of many terms like ‘`r + a x + s + b x + t`’, this
pattern will check all pairs of terms for possible matches. The rewrite
will take whichever suitable pair it discovers first.

In general, a pattern using an associative operator like ‘`a + b`’
will try 2 n different ways to match a sum of n terms
like ‘`x + y + z - w`’. First, ‘`a`’ is matched against each
of ‘`x`’, ‘`y`’, ‘`z`’, and ‘`-w`’ in turn, with ‘`b`’
being matched to the remainders ‘`y + z - w`’, ‘`x + z - w`’, etc.
If none of these succeed, then ‘`b`’ is matched against each of the
four terms with ‘`a`’ matching the remainder. Half-and-half matches,
like ‘`(x + y) + (z - w)`’, are not tried.

Note that ‘`*`’ is not commutative when applied to matrices, but
rewrite rules pretend that it is. If you type `m v` to enable
Matrix mode (see [Matrix and Scalar Modes](#Matrix-Mode)), rewrite rules will match ‘`*`’
literally, ignoring its usual commutativity property. (In the
current implementation, the associativity also vanishes—it is as
if the pattern had been enclosed in a `plain` marker; see below.)
If you are applying rewrites to formulas with matrices, it’s best to
enable Matrix mode first to prevent algebraically incorrect rewrites
from occurring.

The pattern ‘`-x`’ will actually match any expression. For example,
the rule

```
f(-x)  :=  -f(x)
```

will rewrite ‘`f(a)`’ to ‘`-f(-a)`’. To avoid this, either use
a `plain` marker as described below, or add a ‘`negative(x)`’
condition. The `negative` function is true if its argument
“looks” negative, for example, because it is a negative number or
because it is a formula like ‘`-x`’. The new rule using this
condition is:

```
f(x)  :=  -f(-x)  :: negative(x)    or, equivalently,
f(-x)  :=  -f(x)  :: negative(-x)
```

In the same way, the pattern ‘`x - y`’ will match the sum ‘`a + b`’
by matching ‘`y`’ to ‘`-b`’.

The pattern ‘`a b`’ will also match the formula ‘`x/y`’ if
‘`y`’ is a number. Thus the rule ‘`a x + b x := (a+b) x`’
will also convert ‘`a x + x / 2`’ to ‘`(a + 0.5) x`’ (or
‘`(a + 1:2) x`’, depending on the current fraction mode).

Calc will *not* take other liberties with ‘`*`’, ‘`/`’, and
‘`^`’. For example, the pattern ‘`f(a b)`’ will not match
‘`f(x^2)`’, and ‘`f(a + b)`’ will not match ‘`f(2 x)`’, even
though conceivably these patterns could match with ‘`a = b = x`’.
Nor will ‘`f(a b)`’ match ‘`f(x / y)`’ if ‘`y`’ is not a
constant, even though it could be considered to match with ‘`a = x`’
and ‘`b = 1/y`’. The reasons are partly for efficiency, and partly
because while few mathematical operations are substantively different
for addition and subtraction, often it is preferable to treat the cases
of multiplication, division, and integer powers separately.

Even more subtle is the rule set

```
[ f(a) + f(b) := f(a + b),  -f(a) := f(-a) ]
```

attempting to match ‘`f(x) - f(y)`’. You might think that Calc
will view this subtraction as ‘`f(x) + (-f(y))`’ and then apply
the above two rules in turn, but actually this will not work because
Calc only does this when considering rules for ‘`+`’ (like the
first rule in this set). So it will see first that ‘`f(x) + (-f(y))`’
does not match ‘`f(a) + f(b)`’ for any assignments of the
meta-variables, and then it will see that ‘`f(x) - f(y)`’ does
not match ‘`-f(a)`’ for any assignment of ‘`a`’. Because Calc
tries only one rule at a time, it will not be able to rewrite
‘`f(x) - f(y)`’ with this rule set. An explicit ‘`f(a) - f(b)`’
rule will have to be added.

Another thing patterns will *not* do is break up complex numbers.
The pattern ‘`myconj(a + b i) := a - b i`’ will work for formulas
involving the special constant ‘`i`’ (such as ‘`3 - 4 i`’), but
it will not match actual complex numbers like ‘`(3, -4)`’. A version
of the above rule for complex numbers would be

```
myconj(a)  :=  re(a) - im(a) (0,1)  :: im(a) != 0
```

(Because the `re` and `im` functions understand the properties
of the special constant ‘`i`’, this rule will also work for
‘`3 - 4 i`’. In fact, this particular rule would probably be better
without the ‘`im(a) != 0`’ condition, since if ‘`im(a) = 0`’ the
righthand side of the rule will still give the correct answer for the
conjugate of a real number.)

It is also possible to specify optional arguments in patterns. The rule

```
opt(a) x + opt(b) (x^opt(c) + opt(d))  :=  f(a, b, c, d)
```

will match the formula

```
5 (x^2 - 4) + 3 x
```

in a fairly straightforward manner, but it will also match reduced
formulas like

```
x + x^2,    2(x + 1) - x,    x + x
```

producing, respectively,

```
f(1, 1, 2, 0),   f(-1, 2, 1, 1),   f(1, 1, 1, 0)
```

(The latter two formulas can be entered only if default simplifications
have been turned off with `m O`.)

The default value for a term of a sum is zero. The default value
for a part of a product, for a power, or for the denominator of a
quotient, is one. Also, ‘`-x`’ matches the pattern ‘`opt(a) b`’
with ‘`a = -1`’.

In particular, the distributive-law rule can be refined to

```
opt(a) x + opt(b) x  :=  (a + b) x
```

so that it will convert, e.g., ‘`a x - x`’, to ‘`(a - 1) x`’.

The pattern ‘`opt(a) + opt(b) x`’ matches almost any formulas which
are linear in ‘`x`’. You can also use the `lin` and `islin`
functions with rewrite conditions to test for this; see [Logical Operations](#Logical-Operations). These functions are not as convenient to use in rewrite
rules, but they recognize more kinds of formulas as linear:
‘`x/z`’ is considered linear with ‘`b = 1/z`’ by `lin`,
but it will not match the above pattern because that pattern calls
for a multiplication, not a division.

As another example, the obvious rule to replace ‘`sin(x)^2 + cos(x)^2`’
by 1,

```
sin(x)^2 + cos(x)^2  :=  1
```

misses many cases because the sine and cosine may both be multiplied by
an equal factor. Here’s a more successful rule:

```
opt(a) sin(x)^2 + opt(a) cos(x)^2  :=  a
```

Note that this rule will *not* match ‘`sin(x)^2 + 6 cos(x)^2`’
because one ‘`a`’ would have “matched” 1 while the other matched 6.

Calc automatically converts a rule like

```
f(x-1, x)  :=  g(x)
```

into the form

```
f(temp, x)  :=  g(x)  :: temp = x-1
```

(where `temp` stands for a new, invented meta-variable that
doesn’t actually have a name). This modified rule will successfully
match ‘`f(6, 7)`’, binding ‘`temp`’ and ‘`x`’ to 6 and 7,
respectively, then verifying that they differ by one even though
‘`6`’ does not superficially look like ‘`x-1`’.

However, Calc does not solve equations to interpret a rule. The
following rule,

```
f(x-1, x+1)  :=  g(x)
```

will not work. That is, it will match ‘`f(a - 1 + b, a + 1 + b)`’
but not ‘`f(6, 8)`’. Calc always interprets at least one occurrence
of a variable by literal matching. If the variable appears “isolated”
then Calc is smart enough to use it for literal matching. But in this
last example, Calc is forced to rewrite the rule to ‘`f(x-1, temp)
:= g(x) :: temp = x+1`’ where the ‘`x-1`’ term must correspond to an
actual “something-minus-one” in the target formula.

A successful way to write this would be ‘`f(x, x+2) := g(x+1)`’.
You could make this resemble the original form more closely by using
`let` notation, which is described in the next section:

```
f(xm1, x+1)  :=  g(x)  :: let(x := xm1+1)
```

Calc does this rewriting or “conditionalizing” for any sub-pattern
which involves only the functions in the following list, operating
only on constants and meta-variables which have already been matched
elsewhere in the pattern. When matching a function call, Calc is
careful to match arguments which are plain variables before arguments
which are calls to any of the functions below, so that a pattern like
‘`f(x-1, x)`’ can be conditionalized even though the isolated
‘`x`’ comes after the ‘`x-1`’.

```
+ - * / \ % ^  abs sign  round rounde roundu trunc floor ceil
max min  re im conj arg
```

You can suppress all of the special treatments described in this
section by surrounding a function call with a `plain` marker.
This marker causes the function call which is its argument to be
matched literally, without regard to commutativity, associativity,
negation, or conditionalization. When you use `plain`, the
“deep structure” of the formula being matched can show through.
For example,

```
plain(a - a b)  :=  f(a, b)
```

will match only literal subtractions. However, the `plain`
marker does not affect its arguments’ arguments. In this case,
commutativity and associativity is still considered while matching
the ‘`a b`’ sub-pattern, so the whole pattern will match
‘`x - y x`’ as well as ‘`x - x y`’. We could go still
further and use

```
plain(a - plain(a b))  :=  f(a, b)
```

which would do a completely strict match for the pattern.

By contrast, the `quote` marker means that not only the
function name but also the arguments must be literally the same.
The above pattern will match ‘`x - x y`’ but

```
quote(a - a b)  :=  f(a, b)
```

will match only the single formula ‘`a - a b`’. Also,

```
quote(a - quote(a b))  :=  f(a, b)
```

will match only ‘`a - quote(a b)`’—probably not the desired
effect!

A certain amount of algebra is also done when substituting the
meta-variables on the righthand side of a rule. For example,
in the rule

```
a + f(b)  :=  f(a + b)
```

matching ‘`f(x) - y`’ would produce ‘`f((-y) + x)`’ if
taken literally, but the rewrite mechanism will simplify the
righthand side to ‘`f(x - y)`’ automatically. (Of course,
the default simplifications would do this anyway, so this
special simplification is only noticeable if you have turned the
default simplifications off.) This rewriting is done only when
a meta-variable expands to a “negative-looking” expression.
If this simplification is not desirable, you can use a `plain`
marker on the righthand side:

```
a + f(b)  :=  f(plain(a + b))
```

In this example, we are still allowing the pattern-matcher to
use all the algebra it can muster, but the righthand side will
always simplify to a literal addition like ‘`f((-y) + x)`’.

---

Next: [Composing Patterns in Rewrite Rules](#Composing-Patterns-in-Rewrite-Rules), Previous: [Algebraic Properties of Rewrite Rules](#Algebraic-Properties-of-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.5 Other Features of Rewrite Rules [¶](#Other-Features-of-Rewrite-Rules-1)

Certain “function names” serve as markers in rewrite rules.
Here is a complete list of these markers. First are listed the
markers that work inside a pattern; then come the markers that
work in the righthand side of a rule.

One kind of marker, ‘`import(x)`’, takes the place of a whole
rule. Here ‘`x`’ is the name of a variable containing another
rule set; those rules are “spliced into” the rule set that
imports them. For example, if ‘`[f(a+b) := f(a) + f(b),
f(a b) := a f(b) :: real(a)]`’ is stored in variable ‘`linearF`’,
then the rule set ‘`[f(0) := 0, import(linearF)]`’ will apply
all three rules. It is possible to modify the imported rules
slightly: ‘`import(x, v1, x1, v2, x2, …)`’ imports
the rule set ‘`x`’ with all occurrences of
‘`v1`’,
as either a variable name or a function name, replaced with
‘`x1`’
and so on. (If
‘`v1`’
is used as a function name, then
‘`x1`’
must be either a function name itself or a ‘`< >`’ nameless
function; see [Specifying Operators](#Specifying-Operators).) For example, ‘`[g(0) := 0,
import(linearF, f, g)]`’ applies the linearity rules to the function
‘`g`’ instead of ‘`f`’. Imports can be nested, but the
import-with-renaming feature may fail to rename sub-imports properly.

The special functions allowed in patterns are:

‘`quote(x)`’
:   This pattern matches exactly ‘`x`’; variable names in ‘`x`’ are
    not interpreted as meta-variables. The only flexibility is that
    numbers are compared for numeric equality, so that the pattern
    ‘`f(quote(12))`’ will match both ‘`f(12)`’ and ‘`f(12.0)`’.
    (Numbers are always treated this way by the rewrite mechanism:
    The rule ‘`f(x,x) := g(x)`’ will match ‘`f(12, 12.0)`’.
    The rewrite may produce either ‘`g(12)`’ or ‘`g(12.0)`’
    as a result in this case.)

‘`plain(x)`’
:   Here ‘`x`’ must be a function call ‘`f(x1,x2,…)`’. This
    pattern matches a call to function ‘`f`’ with the specified
    argument patterns. No special knowledge of the properties of the
    function ‘`f`’ is used in this case; ‘`+`’ is not commutative or
    associative. Unlike `quote`, the arguments ‘`x1,x2,…`’
    are treated as patterns. If you wish them to be treated “plainly”
    as well, you must enclose them with more `plain` markers:
    ‘`plain(plain(-a) + plain(b c))`’.

‘`opt(x,def)`’
:   Here ‘`x`’ must be a variable name. This must appear as an
    argument to a function or an element of a vector; it specifies that
    the argument or element is optional.
    As an argument to ‘`+`’, ‘`-`’, ‘`*`’, ‘`&&`’, or ‘`||`’,
    or as the second argument to ‘`/`’ or ‘`^`’, the value def
    may be omitted. The pattern ‘`x + opt(y)`’ matches a sum by
    binding one summand to ‘`x`’ and the other to ‘`y`’, and it
    matches anything else by binding the whole expression to ‘`x`’ and
    zero to ‘`y`’. The other operators above work similarly.

    For general miscellaneous functions, the default value `def`
    must be specified. Optional arguments are dropped starting with
    the rightmost one during matching. For example, the pattern
    ‘`f(opt(a,0), b, opt(c,b))`’ will match ‘`f(b)`’, ‘`f(a,b)`’,
    or ‘`f(a,b,c)`’. Default values of zero and ‘`b`’ are
    supplied in this example for the omitted arguments. Note that
    the literal variable ‘`b`’ will be the default in the latter
    case, *not* the value that matched the meta-variable ‘`b`’.
    In other words, the default def is effectively quoted.

‘`condition(x,c)`’
:   This matches the pattern ‘`x`’, with the attached condition
    ‘`c`’. It is the same as ‘`x :: c`’.

‘`pand(x,y)`’
:   This matches anything that matches both pattern ‘`x`’ and
    pattern ‘`y`’. It is the same as ‘`x &&& y`’.
    see [Composing Patterns in Rewrite Rules](#Composing-Patterns-in-Rewrite-Rules).

‘`por(x,y)`’
:   This matches anything that matches either pattern ‘`x`’ or
    pattern ‘`y`’. It is the same as ‘`x ||| y`’.

‘`pnot(x)`’
:   This matches anything that does not match pattern ‘`x`’.
    It is the same as ‘`!!! x`’.

‘`cons(h,t)`’
:   This matches any vector of one or more elements. The first
    element is matched to ‘`h`’; a vector of the remaining
    elements is matched to ‘`t`’. Note that vectors of fixed
    length can also be matched as actual vectors: The rule
    ‘`cons(a,cons(b,[])) := cons(a+b,[])`’ is equivalent
    to the rule ‘`[a,b] := [a+b]`’.

‘`rcons(t,h)`’
:   This is like `cons`, except that the *last* element
    is matched to ‘`h`’, with the remaining elements matched
    to ‘`t`’.

‘`apply(f,args)`’
:   This matches any function call. The name of the function, in
    the form of a variable, is matched to ‘`f`’. The arguments
    of the function, as a vector of zero or more objects, are
    matched to ‘`args`’. Constants, variables, and vectors
    do *not* match an `apply` pattern. For example,
    ‘`apply(f,x)`’ matches any function call, ‘`apply(quote(f),x)`’
    matches any call to the function ‘`f`’, ‘`apply(f,[a,b])`’
    matches any function call with exactly two arguments, and
    ‘`apply(quote(f), cons(a,cons(b,x)))`’ matches any call
    to the function ‘`f`’ with two or more arguments. Another
    way to implement the latter, if the rest of the rule does not
    need to refer to the first two arguments of ‘`f`’ by name,
    would be ‘`apply(quote(f), x :: vlen(x) >= 2)`’.
    Here’s a more interesting sample use of `apply`:

    ```
    apply(f,[x+n])  :=  n + apply(f,[x])
       :: in(f, [floor,ceil,round,trunc]) :: integer(n)
    ```

    Note, however, that this will be slower to match than a rule
    set with four separate rules. The reason is that Calc sorts
    the rules of a rule set according to top-level function name;
    if the top-level function is `apply`, Calc must try the
    rule for every single formula and sub-formula. If the top-level
    function in the pattern is, say, `floor`, then Calc invokes
    the rule only for sub-formulas which are calls to `floor`.

    Formulas normally written with operators like `+` are still
    considered function calls: `apply(f,x)` matches ‘`a+b`’
    with ‘`f = add`’, ‘`x = [a,b]`’.

    You must use `apply` for meta-variables with function names
    on both sides of a rewrite rule: ‘`apply(f, [x]) := f(x+1)`’
    is *not* correct, because it rewrites ‘`spam(6)`’ into
    ‘`f(7)`’. The righthand side should be ‘`apply(f, [x+1])`’.
    Also note that you will have to use No-Simplify mode (`m O`)
    when entering this rule so that the `apply` isn’t
    evaluated immediately to get the new rule ‘`f(x) := f(x+1)`’.
    Or, use `s e` to enter the rule without going through the stack,
    or enter the rule as ‘`apply(f, [x]) := apply(f, [x+1]) :: 1`’.
    See [Conditional Rewrite Rules](#Conditional-Rewrite-Rules).

‘`select(x)`’
:   This is used for applying rules to formulas with selections;
    see [Selections with Rewrite Rules](#Selections-with-Rewrite-Rules).

Special functions for the righthand sides of rules are:

‘`quote(x)`’
:   The notation ‘`quote(x)`’ is changed to ‘`x`’ when the
    righthand side is used. As far as the rewrite rule is concerned,
    `quote` is invisible. However, `quote` has the special
    property in Calc that its argument is not evaluated. Thus,
    while it will not work to put the rule ‘`t(a) := typeof(a)`’
    on the stack because ‘`typeof(a)`’ is evaluated immediately
    to produce ‘`t(a) := 100`’, you can use `quote` to
    protect the righthand side: ‘`t(a) := quote(typeof(a))`’.
    (See [Conditional Rewrite Rules](#Conditional-Rewrite-Rules), for another trick for
    protecting rules from evaluation.)

‘`plain(x)`’
:   Special properties of and simplifications for the function call
    ‘`x`’ are not used. One interesting case where `plain`
    is useful is the rule, ‘`q(x) := quote(x)`’, trying to expand a
    shorthand notation for the `quote` function. This rule will
    not work as shown; instead of replacing ‘`q(foo)`’ with
    ‘`quote(foo)`’, it will replace it with ‘`foo`’! The correct
    rule would be ‘`q(x) := plain(quote(x))`’.

‘`cons(h,t)`’
:   Where ‘`t`’ is a vector, this is converted into an expanded
    vector during rewrite processing. Note that `cons` is a regular
    Calc function which normally does this anyway; the only way `cons`
    is treated specially by rewrites is that `cons` on the righthand
    side of a rule will be evaluated even if default simplifications
    have been turned off.

‘`rcons(t,h)`’
:   Analogous to `cons` except putting ‘`h`’ at the *end* of
    the vector ‘`t`’.

‘`apply(f,args)`’
:   Where ‘`f`’ is a variable and args is a vector, this
    is converted to a function call. Once again, note that `apply`
    is also a regular Calc function.

‘`eval(x)`’
:   The formula ‘`x`’ is handled in the usual way, then the
    default simplifications are applied to it even if they have
    been turned off normally. This allows you to treat any function
    similarly to the way `cons` and `apply` are always
    treated. However, there is a slight difference: ‘`cons(2+3, [])`’
    with default simplifications off will be converted to ‘`[2+3]`’,
    whereas ‘`eval(cons(2+3, []))`’ will be converted to ‘`[5]`’.

‘`evalsimp(x)`’
:   The formula ‘`x`’ has meta-variables substituted in the usual
    way, then algebraically simplified.

‘`evalextsimp(x)`’
:   The formula ‘`x`’ has meta-variables substituted in the normal
    way, then “extendedly” simplified as if by the `a e` command.

‘`select(x)`’
:   See [Selections with Rewrite Rules](#Selections-with-Rewrite-Rules).

There are also some special functions you can use in conditions.

‘`let(v := x)`’
:   The expression ‘`x`’ is evaluated with meta-variables substituted.
    The algebraic simplifications are *not* applied by
    default, but ‘`x`’ can include calls to `evalsimp` or
    `evalextsimp` as described above to invoke higher levels
    of simplification. The result of ‘`x`’ is then bound to the
    meta-variable ‘`v`’. As usual, if this meta-variable has already
    been matched to something else the two values must be equal; if the
    meta-variable is new then it is bound to the result of the expression.
    This variable can then appear in later conditions, and on the righthand
    side of the rule.
    In fact, ‘`v`’ may be any pattern in which case the result of
    evaluating ‘`x`’ is matched to that pattern, binding any
    meta-variables that appear in that pattern. Note that `let`
    can only appear by itself as a condition, or as one term of an
    ‘`&&`’ which is a whole condition: It cannot be inside
    an ‘`||`’ term or otherwise buried.

    The alternate, equivalent form ‘`let(v, x)`’ is also recognized.
    Note that the use of ‘`:=`’ by `let`, while still being
    assignment-like in character, is unrelated to the use of ‘`:=`’
    in the main part of a rewrite rule.

    As an example, ‘`f(a) := g(ia) :: let(ia := 1/a) :: constant(ia)`’
    replaces ‘`f(a)`’ with ‘`g`’ of the inverse of ‘`a`’, if
    that inverse exists and is constant. For example, if ‘`a`’ is a
    singular matrix the operation ‘`1/a`’ is left unsimplified and
    ‘`constant(ia)`’ fails, but if ‘`a`’ is an invertible matrix
    then the rule succeeds. Without `let` there would be no way
    to express this rule that didn’t have to invert the matrix twice.
    Note that, because the meta-variable ‘`ia`’ is otherwise unbound
    in this rule, the `let` condition itself always “succeeds”
    because no matter what ‘`1/a`’ evaluates to, it can successfully
    be bound to `ia`.

    Here’s another example, for integrating cosines of linear
    terms: ‘`myint(cos(y),x) := sin(y)/b :: let([a,b,x] := lin(y,x))`’.
    The `lin` function returns a 3-vector if its argument is linear,
    or leaves itself unevaluated if not. But an unevaluated `lin`
    call will not match the 3-vector on the lefthand side of the `let`,
    so this `let` both verifies that `y` is linear, and binds
    the coefficients `a` and `b` for use elsewhere in the rule.
    (It would have been possible to use ‘`sin(a x + b)/b`’ for the
    righthand side instead, but using ‘`sin(y)/b`’ avoids gratuitous
    rearrangement of the argument of the sine.)

    Similarly, here is a rule that implements an inverse-`erf`
    function. It uses `root` to search for a solution. If
    `root` succeeds, it will return a vector of two numbers
    where the first number is the desired solution. If no solution
    is found, `root` remains in symbolic form. So we use
    `let` to check that the result was indeed a vector.

    ```
    ierf(x)  :=  y  :: let([y,z] := root(erf(a) = x, a, .5))
    ```

‘`matches(v,p)`’
:   The meta-variable v, which must already have been matched
    to something elsewhere in the rule, is compared against pattern
    p. Since `matches` is a standard Calc function, it
    can appear anywhere in a condition. But if it appears alone or
    as a term of a top-level ‘`&&`’, then you get the special
    extra feature that meta-variables which are bound to things
    inside p can be used elsewhere in the surrounding rewrite
    rule.

    The only real difference between ‘`let(p := v)`’ and
    ‘`matches(v, p)`’ is that the former evaluates ‘`v`’ using
    the default simplifications, while the latter does not.

‘`remember`’ [¶](#index-remember-1)
:   This is actually a variable, not a function. If `remember`
    appears as a condition in a rule, then when that rule succeeds
    the original expression and rewritten expression are added to the
    front of the rule set that contained the rule. If the rule set
    was not stored in a variable, `remember` is ignored. The
    lefthand side is enclosed in `quote` in the added rule if it
    contains any variables.

    For example, the rule ‘`f(n) := n f(n-1) :: remember`’ applied
    to ‘`f(7)`’ will add the rule ‘`f(7) := 7 f(6)`’ to the front
    of the rule set. The rule set `EvalRules` works slightly
    differently: There, the evaluation of ‘`f(6)`’ will complete before
    the result is added to the rule set, in this case as ‘`f(7) := 5040`’.
    Thus `remember` is most useful inside `EvalRules`.

    It is up to you to ensure that the optimization performed by
    `remember` is safe. For example, the rule ‘`foo(n) := n
    :: evalv(eatfoo) > 0 :: remember`’ is a bad idea (`evalv` is
    the function equivalent of the `=` command); if the variable
    `eatfoo` ever contains 1, rules like ‘`foo(7) := 7`’ will
    be added to the rule set and will continue to operate even if
    `eatfoo` is later changed to 0.

‘`remember(c)`’
:   Remember the match as described above, but only if condition ‘`c`’
    is true. For example, ‘`remember(n % 4 = 0)`’ in the above factorial
    rule remembers only every fourth result. Note that ‘`remember(1)`’
    is equivalent to ‘`remember`’, and ‘`remember(0)`’ has no effect.

---

Next: [Nested Formulas with Rewrite Rules](#Nested-Formulas-with-Rewrite-Rules), Previous: [Other Features of Rewrite Rules](#Other-Features-of-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.6 Composing Patterns in Rewrite Rules [¶](#Composing-Patterns-in-Rewrite-Rules-1)

There are three operators, ‘`&&&`’, ‘`|||`’, and ‘`!!!`’,
that combine rewrite patterns to make larger patterns. The
combinations are “and,” “or,” and “not,” respectively, and
these operators are the pattern equivalents of ‘`&&`’, ‘`||`’
and ‘`!`’ (which operate on zero-or-nonzero logical values).

Note that ‘`&&&`’, ‘`|||`’, and ‘`!!!`’ are left in symbolic
form by all regular Calc features; they have special meaning only in
the context of rewrite rule patterns.

The pattern ‘`p1 &&& p2`’ matches anything that
matches both p1 and p2. One especially useful case is
when one of p1 or p2 is a meta-variable. For example,
here is a rule that operates on error forms:

```
f(x &&& a +/- b, x)  :=  g(x)
```

This does the same thing, but is arguably simpler than, the rule

```
f(a +/- b, a +/- b)  :=  g(a +/- b)
```

Here’s another interesting example:

```
ends(cons(a, x) &&& rcons(y, b))  :=  [a, b]
```

which effectively clips out the middle of a vector leaving just
the first and last elements. This rule will change a one-element
vector ‘`[a]`’ to ‘`[a, a]`’. The similar rule

```
ends(cons(a, rcons(y, b)))  :=  [a, b]
```

would do the same thing except that it would fail to match a
one-element vector.

The pattern ‘`p1 ||| p2`’ matches anything that
matches either p1 or p2. Calc first tries matching
against p1; if that fails, it goes on to try p2.

A simple example of ‘`|||`’ is

```
curve(inf ||| -inf)  :=  0
```

which converts both ‘`curve(inf)`’ and ‘`curve(-inf)`’ to zero.

Here is a larger example:

```
log(a, b) ||| (ln(a) :: let(b := e))  :=  mylog(a, b)
```

This matches both generalized and natural logarithms in a single rule.
Note that the ‘`::`’ term must be enclosed in parentheses because
that operator has lower precedence than ‘`|||`’ or ‘`:=`’.

(In practice this rule would probably include a third alternative,
omitted here for brevity, to take care of `log10`.)

While Calc generally treats interior conditions exactly the same as
conditions on the outside of a rule, it does guarantee that if all the
variables in the condition are special names like `e`, or already
bound in the pattern to which the condition is attached (say, if
‘`a`’ had appeared in this condition), then Calc will process this
condition right after matching the pattern to the left of the ‘`::`’.
Thus, we know that ‘`b`’ will be bound to ‘`e`’ only if the
`ln` branch of the ‘`|||`’ was taken.

Note that this rule was careful to bind the same set of meta-variables
on both sides of the ‘`|||`’. Calc does not check this, but if
you bind a certain meta-variable only in one branch and then use that
meta-variable elsewhere in the rule, results are unpredictable:

```
f(a,b) ||| g(b)  :=  h(a,b)
```

Here if the pattern matches ‘`g(17)`’, Calc makes no promises about
the value that will be substituted for ‘`a`’ on the righthand side.

The pattern ‘`!!! pat`’ matches anything that does not
match pat. Any meta-variables that are bound while matching
pat remain unbound outside of pat.

For example,

```
f(x &&& !!! a +/- b, !!![])  :=  g(x)
```

converts `f` whose first argument is anything *except* an
error form, and whose second argument is not the empty vector, into
a similar call to `g` (but without the second argument).

If we know that the second argument will be a vector (empty or not),
then an equivalent rule would be:

```
f(x, y)  :=  g(x)  :: typeof(x) != 7 :: vlen(y) > 0
```

where of course 7 is the `typeof` code for error forms.
Another final condition, that works for any kind of ‘`y`’,
would be ‘`!istrue(y == [])`’. (The `istrue` function
returns an explicit 0 if its argument was left in symbolic form;
plain ‘`!(y == [])`’ or ‘`y != []`’ would not work to replace
‘`!!![]`’ since these would be left unsimplified, and thus cause
the rule to fail, if ‘`y`’ was something like a variable name.)

It is possible for a ‘`!!!`’ to refer to meta-variables bound
elsewhere in the pattern. For example,

```
f(a, !!!a)  :=  g(a)
```

matches any call to `f` with different arguments, changing
this to `g` with only the first argument.

If a function call is to be matched and one of the argument patterns
contains a ‘`!!!`’ somewhere inside it, that argument will be
matched last. Thus

```
f(!!!a, a)  :=  g(a)
```

will be careful to bind ‘`a`’ to the second argument of `f`
before testing the first argument. If Calc had tried to match the
first argument of `f` first, the results would have been
disastrous: since `a` was unbound so far, the pattern ‘`a`’
would have matched anything at all, and the pattern ‘`!!!a`’
therefore would *not* have matched anything at all!

---

Next: [Multi-Phase Rewrite Rules](#Multi_002dPhase-Rewrite-Rules), Previous: [Composing Patterns in Rewrite Rules](#Composing-Patterns-in-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.7 Nested Formulas with Rewrite Rules [¶](#Nested-Formulas-with-Rewrite-Rules-1)

When `a r` (`calc-rewrite`) is used, it takes an expression from
the top of the stack and attempts to match any of the specified rules
to any part of the expression, starting with the whole expression
and then, if that fails, trying deeper and deeper sub-expressions.
For each part of the expression, the rules are tried in the order
they appear in the rules vector. The first rule to match the first
sub-expression wins; it replaces the matched sub-expression according
to the new part of the rule.

Often, the rule set will match and change the formula several times.
The top-level formula is first matched and substituted repeatedly until
it no longer matches the pattern; then, sub-formulas are tried, and
so on. Once every part of the formula has gotten its chance, the
rewrite mechanism starts over again with the top-level formula
(in case a substitution of one of its arguments has caused it again
to match). This continues until no further matches can be made
anywhere in the formula.

It is possible for a rule set to get into an infinite loop. The
most obvious case, replacing a formula with itself, is not a problem
because a rule is not considered to “succeed” unless the righthand
side actually comes out to something different from the original
formula or sub-formula that was matched. But if you accidentally
had both ‘`ln(a b) := ln(a) + ln(b)`’ and the reverse
‘`ln(a) + ln(b) := ln(a b)`’ in your rule set, Calc would
run forever switching a formula back and forth between the two
forms.

To avoid disaster, Calc normally stops after 100 changes have been
made to the formula. This will be enough for most multiple rewrites,
but it will keep an endless loop of rewrites from locking up the
computer forever. (On most systems, you can also type `C-g` to
halt any Emacs command prematurely.)

To change this limit, give a positive numeric prefix argument.
In particular, `M-1 a r` applies only one rewrite at a time,
useful when you are first testing your rule (or just if repeated
rewriting is not what is called for by your application).

You can also put a “function call” ‘`iterations(n)`’
in place of a rule anywhere in your rules vector (but usually at
the top). Then, n will be used instead of 100 as the default
number of iterations for this rule set. You can use
‘`iterations(inf)`’ if you want no iteration limit by default.
A prefix argument will override the `iterations` limit in the
rule set.

```
[ iterations(1),
  f(x) := f(x+1) ]
```

More precisely, the limit controls the number of “iterations,”
where each iteration is a successful matching of a rule pattern whose
righthand side, after substituting meta-variables and applying the
default simplifications, is different from the original sub-formula
that was matched.

A prefix argument of zero sets the limit to infinity. Use with caution!

Given a negative numeric prefix argument, `a r` will match and
substitute the top-level expression up to that many times, but
will not attempt to match the rules to any sub-expressions.

In a formula, `rewrite(expr, rules, n)`
does a rewriting operation. Here expr is the expression
being rewritten, rules is the rule, vector of rules, or
variable containing the rules, and n is the optional
iteration limit, which may be a positive integer, a negative
integer, or ‘`inf`’ or ‘`-inf`’. If n is omitted
the `iterations` value from the rule set is used; if both
are omitted, 100 is used.

---

Next: [Selections with Rewrite Rules](#Selections-with-Rewrite-Rules), Previous: [Nested Formulas with Rewrite Rules](#Nested-Formulas-with-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.8 Multi-Phase Rewrite Rules [¶](#Multi_002dPhase-Rewrite-Rules-1)

It is possible to separate a rewrite rule set into several *phases*.
During each phase, certain rules will be enabled while certain others
will be disabled. A *phase schedule* controls the order in which
phases occur during the rewriting process.

If a call to the marker function `phase` appears in the rules
vector in place of a rule, all rules following that point will be
members of the phase(s) identified in the arguments to `phase`.
Phases are given integer numbers. The markers ‘`phase()`’ and
‘`phase(all)`’ both mean the following rules belong to all phases;
this is the default at the start of the rule set.

If you do not explicitly schedule the phases, Calc sorts all phase
numbers that appear in the rule set and executes the phases in
ascending order. For example, the rule set

```
[ f0(x) := g0(x),
  phase(1),
  f1(x) := g1(x),
  phase(2),
  f2(x) := g2(x),
  phase(3),
  f3(x) := g3(x),
  phase(1,2),
  f4(x) := g4(x) ]
```

has three phases, 1 through 3. Phase 1 consists of the `f0`,
`f1`, and `f4` rules (in that order). Phase 2 consists of
`f0`, `f2`, and `f4`. Phase 3 consists of `f0`
and `f3`.

When Calc rewrites a formula using this rule set, it first rewrites
the formula using only the phase 1 rules until no further changes are
possible. Then it switches to the phase 2 rule set and continues
until no further changes occur, then finally rewrites with phase 3.
When no more phase 3 rules apply, rewriting finishes. (This is
assuming `a r` with a large enough prefix argument to allow the
rewriting to run to completion; the sequence just described stops
early if the number of iterations specified in the prefix argument,
100 by default, is reached.)

During each phase, Calc descends through the nested levels of the
formula as described previously. (See [Nested Formulas with Rewrite Rules](#Nested-Formulas-with-Rewrite-Rules).) Rewriting starts at the top of the formula, then works its
way down to the parts, then goes back to the top and works down again.
The phase 2 rules do not begin until no phase 1 rules apply anywhere
in the formula.

A `schedule` marker appearing in the rule set (anywhere, but
conventionally at the top) changes the default schedule of phases.
In the simplest case, `schedule` has a sequence of phase numbers
for arguments; each phase number is invoked in turn until the
arguments to `schedule` are exhausted. Thus adding
‘`schedule(3,2,1)`’ at the top of the above rule set would
reverse the order of the phases; ‘`schedule(1,2,3)`’ would have
no effect since this is the default schedule; and ‘`schedule(1,2,1,3)`’
would give phase 1 a second chance after phase 2 has completed, before
moving on to phase 3.

Any argument to `schedule` can instead be a vector of phase
numbers (or even of sub-vectors). Then the sub-sequence of phases
described by the vector are tried repeatedly until no change occurs
in any phase in the sequence. For example, ‘`schedule([1, 2], 3)`’
tries phase 1, then phase 2, then, if either phase made any changes
to the formula, repeats these two phases until they can make no
further progress. Finally, it goes on to phase 3 for finishing
touches.

Also, items in `schedule` can be variable names as well as
numbers. A variable name is interpreted as the name of a function
to call on the whole formula. For example, ‘`schedule(1, simplify)`’
says to apply the phase-1 rules (presumably, all of them), then to
call `simplify` which is the function name equivalent of `a s`.
Likewise, ‘`schedule([1, simplify])`’ says to alternate between
phase 1 and `a s` until no further changes occur.

Phases can be used purely to improve efficiency; if it is known that
a certain group of rules will apply only at the beginning of rewriting,
and a certain other group will apply only at the end, then rewriting
will be faster if these groups are identified as separate phases.
Once the phase 1 rules are done, Calc can put them aside and no longer
spend any time on them while it works on phase 2.

There are also some problems that can only be solved with several
rewrite phases. For a real-world example of a multi-phase rule set,
examine the set `FitRules`, which is used by the curve-fitting
command to convert a model expression to linear form.
See [Curve Fitting Details](#Curve-Fitting-Details). This set is divided into four phases.
The first phase rewrites certain kinds of expressions to be more
easily linearizable, but less computationally efficient. After the
linear components have been picked out, the final phase includes the
opposite rewrites to put each component back into an efficient form.
If both sets of rules were included in one big phase, Calc could get
into an infinite loop going back and forth between the two forms.

Elsewhere in `FitRules`, the components are first isolated,
then recombined where possible to reduce the complexity of the linear
fit, then finally packaged one component at a time into vectors.
If the packaging rules were allowed to begin before the recombining
rules were finished, some components might be put away into vectors
before they had a chance to recombine. By putting these rules in
two separate phases, this problem is neatly avoided.

---

Next: [Matching Commands](#Matching-Commands), Previous: [Multi-Phase Rewrite Rules](#Multi_002dPhase-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.9 Selections with Rewrite Rules [¶](#Selections-with-Rewrite-Rules-1)

If a sub-formula of the current formula is selected (as by `j s`;
see [Selecting Sub-Formulas](#Selecting-Subformulas)), the `a r` (`calc-rewrite`)
command applies only to that sub-formula. Together with a negative
prefix argument, you can use this fact to apply a rewrite to one
specific part of a formula without affecting any other parts.

The `j r` (`calc-rewrite-selection`) command allows more
sophisticated operations on selections. This command prompts for
the rules in the same way as `a r`, but it then applies those
rules to the whole formula in question even though a sub-formula
of it has been selected. However, the selected sub-formula will
first have been surrounded by a ‘`select( )`’ function call.
(Calc’s evaluator does not understand the function name `select`;
this is only a tag used by the `j r` command.)

For example, suppose the formula on the stack is ‘`2 (a + b)^2`’
and the sub-formula ‘`a + b`’ is selected. This formula will
be rewritten to ‘`2 select(a + b)^2`’ and then the rewrite
rules will be applied in the usual way. The rewrite rules can
include references to `select` to tell where in the pattern
the selected sub-formula should appear.

If there is still exactly one ‘`select( )`’ function call in
the formula after rewriting is done, it indicates which part of
the formula should be selected afterwards. Otherwise, the
formula will be unselected.

You can make `j r` act much like `a r` by enclosing both parts
of the rewrite rule with ‘`select()`’. However, `j r`
allows you to use the current selection in more flexible ways.
Suppose you wished to make a rule which removed the exponent from
the selected term; the rule ‘`select(a)^x := select(a)`’ would
work. In the above example, it would rewrite ‘`2 select(a + b)^2`’
to ‘`2 select(a + b)`’. This would then be returned to the
stack as ‘`2 (a + b)`’ with the ‘`a + b`’ selected.

The `j r` command uses one iteration by default, unlike
`a r` which defaults to 100 iterations. A numeric prefix
argument affects `j r` in the same way as `a r`.
See [Nested Formulas with Rewrite Rules](#Nested-Formulas-with-Rewrite-Rules).

As with other selection commands, `j r` operates on the stack
entry that contains the cursor. (If the cursor is on the top-of-stack
‘`.`’ marker, it works as if the cursor were on the formula
at stack level 1.)

If you don’t specify a set of rules, the rules are taken from the
top of the stack, just as with `a r`. In this case, the
cursor must indicate stack entry 2 or above as the formula to be
rewritten (otherwise the same formula would be used as both the
target and the rewrite rules).

If the indicated formula has no selection, the cursor position within
the formula temporarily selects a sub-formula for the purposes of this
command. If the cursor is not on any sub-formula (e.g., it is in
the line-number area to the left of the formula), the ‘`select( )`’
markers are ignored by the rewrite mechanism and the rules are allowed
to apply anywhere in the formula.

As a special feature, the normal `a r` command also ignores
‘`select( )`’ calls in rewrite rules. For example, if you used the
above rule ‘`select(a)^x := select(a)`’ with `a r`, it would apply
the rule as if it were ‘`a^x := a`’. Thus, you can write general
purpose rules with ‘`select( )`’ hints inside them so that they
will “do the right thing” in both `a r` and `j r`,
both with and without selections.

---

Next: [Automatic Rewrites](#Automatic-Rewrites), Previous: [Selections with Rewrite Rules](#Selections-with-Rewrite-Rules), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.10 Matching Commands [¶](#Matching-Commands-1)

The `a m` (`calc-match`) [`match`] function takes a
vector of formulas and a rewrite-rule-style pattern, and produces
a vector of all formulas which match the pattern. The command
prompts you to enter the pattern; as for `a r`, you can enter
a single pattern (i.e., a formula with meta-variables), or a
vector of patterns, or a variable which contains patterns, or
you can give a blank response in which case the patterns are taken
from the top of the stack. The pattern set will be compiled once
and saved if it is stored in a variable. If there are several
patterns in the set, vector elements are kept if they match any
of the patterns.

For example, ‘`match(a+b, [x, x+y, x-y, 7, x+y+z])`’
will return ‘`[x+y, x-y, x+y+z]`’.

The `import` mechanism is not available for pattern sets.

The `a m` command can also be used to extract all vector elements
which satisfy any condition: The pattern ‘`x :: x>0`’ will select
all the positive vector elements.

With the Inverse flag [`matchnot`], this command extracts all
vector elements which do *not* match the given pattern.

There is also a function ‘`matches(x, p)`’ which
evaluates to 1 if expression x matches pattern p, or
to 0 otherwise. This is sometimes useful for including into the
conditional clauses of other rewrite rules.

The function `vmatches` is just like `matches`, except
that if the match succeeds it returns a vector of assignments to
the meta-variables instead of the number 1. For example,
‘`vmatches(f(1,2), f(a,b))`’ returns ‘`[a := 1, b := 2]`’.
If the match fails, the function returns the number 0.

---

Next: [Debugging Rewrites](#Debugging-Rewrites), Previous: [Matching Commands](#Matching-Commands), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.11 Automatic Rewrites [¶](#Automatic-Rewrites-1)

It is possible to get Calc to apply a set of rewrite rules on all
results, effectively adding to the built-in set of default
simplifications. To do this, simply store your rule set in the
variable `EvalRules`. There is a convenient `s E` command
for editing `EvalRules`; see [Other Operations on Variables](#Operations-on-Variables).

For example, suppose you want ‘`sin(a + b)`’ to be expanded out
to ‘`sin(b) cos(a) + cos(b) sin(a)`’ wherever it appears, and
similarly for ‘`cos(a + b)`’. The corresponding rewrite rule
set would be,

```
[ sin(a + b)  :=  cos(a) sin(b) + sin(a) cos(b),
  cos(a + b)  :=  cos(a) cos(b) - sin(a) sin(b) ]
```

To apply these manually, you could put them in a variable called
`trigexp` and then use `a r trigexp` every time you wanted
to expand trig functions. But if instead you store them in the
variable `EvalRules`, they will automatically be applied to all
sines and cosines of sums. Then, with ‘`2 x`’ and ‘`45`’ on
the stack, typing `+ S` will (assuming Degrees mode) result in
‘`0.7071 sin(2 x) + 0.7071 cos(2 x)`’ automatically.

As each level of a formula is evaluated, the rules from
`EvalRules` are applied before the default simplifications.
Rewriting continues until no further `EvalRules` apply.
Note that this is different from the usual order of application of
rewrite rules: `EvalRules` works from the bottom up, simplifying
the arguments to a function before the function itself, while `a r`
applies rules from the top down.

Because the `EvalRules` are tried first, you can use them to
override the normal behavior of any built-in Calc function.

It is important not to write a rule that will get into an infinite
loop. For example, the rule set ‘`[f(0) := 1, f(n) := n f(n-1)]`’
appears to be a good definition of a factorial function, but it is
unsafe. Imagine what happens if ‘`f(2.5)`’ is simplified. Calc
will continue to subtract 1 from this argument forever without reaching
zero. A safer second rule would be ‘`f(n) := n f(n-1) :: n>0`’.
Another dangerous rule is ‘`g(x, y) := g(y, x)`’. Rewriting
‘`g(2, 4)`’, this would bounce back and forth between that and
‘`g(4, 2)`’ forever. If an infinite loop in `EvalRules`
occurs, Emacs will eventually stop with a “Computation got stuck
or ran too long” message.

Another subtle difference between `EvalRules` and regular rewrites
concerns rules that rewrite a formula into an identical formula. For
example, ‘`f(n) := f(floor(n))`’ “fails to match” when ‘`n`’ is
already an integer. But in `EvalRules` this case is detected only
if the righthand side literally becomes the original formula before any
further simplification. This means that ‘`f(n) := f(floor(n))`’ will
get into an infinite loop if it occurs in `EvalRules`. Calc will
replace ‘`f(6)`’ with ‘`f(floor(6))`’, which is different from
‘`f(6)`’, so it will consider the rule to have matched and will
continue simplifying that formula; first the argument is simplified
to get ‘`f(6)`’, then the rule matches again to get ‘`f(floor(6))`’
again, ad infinitum. A much safer rule would check its argument first,
say, with ‘`f(n) := f(floor(n)) :: !dint(n)`’.

(What really happens is that the rewrite mechanism substitutes the
meta-variables in the righthand side of a rule, compares to see if the
result is the same as the original formula and fails if so, then uses
the default simplifications to simplify the result and compares again
(and again fails if the formula has simplified back to its original
form). The only special wrinkle for the `EvalRules` is that the
same rules will come back into play when the default simplifications
are used. What Calc wants to do is build ‘`f(floor(6))`’, see that
this is different from the original formula, simplify to ‘`f(6)`’,
see that this is the same as the original formula, and thus halt the
rewriting. But while simplifying, ‘`f(6)`’ will again trigger
the same `EvalRules` rule and Calc will get into a loop inside
the rewrite mechanism itself.)

The `phase`, `schedule`, and `iterations` markers do
not work in `EvalRules`. If the rule set is divided into phases,
only the phase 1 rules are applied, and the schedule is ignored.
The rules are always repeated as many times as possible.

The `EvalRules` are applied to all function calls in a formula,
but not to numbers (and other number-like objects like error forms),
nor to vectors or individual variable names. (Though they will apply
to *components* of vectors and error forms when appropriate.) You
might try to make a variable `phihat` which automatically expands
to its definition without the need to press `=` by writing the
rule ‘`quote(phihat) := (1-sqrt(5))/2`’, but unfortunately this rule
will not work as part of `EvalRules`.

Finally, another limitation is that Calc sometimes calls its built-in
functions directly rather than going through the default simplifications.
When it does this, `EvalRules` will not be able to override those
functions. For example, when you take the absolute value of the complex
number ‘`(2, 3)`’, Calc computes ‘`sqrt(2*2 + 3*3)`’ by calling
the multiplication, addition, and square root functions directly rather
than applying the default simplifications to this formula. So an
`EvalRules` rule that (perversely) rewrites ‘`sqrt(13) := 6`’
would not apply. (However, if you put Calc into Symbolic mode so that
‘`sqrt(13)`’ will be left in symbolic form by the built-in square
root function, your rule will be able to apply. But if the complex
number were ‘`(3,4)`’, so that ‘`sqrt(25)`’ must be calculated,
then Symbolic mode will not help because ‘`sqrt(25)`’ can be
evaluated exactly to 5.)

One subtle restriction that normally only manifests itself with
`EvalRules` is that while a given rewrite rule is in the process
of being checked, that same rule cannot be recursively applied. Calc
effectively removes the rule from its rule set while checking the rule,
then puts it back once the match succeeds or fails. (The technical
reason for this is that compiled pattern programs are not reentrant.)
For example, consider the rule ‘`foo(x) := x :: foo(x/2) > 0`’
attempting to match ‘`foo(8)`’. This rule will be inactive while
the condition ‘`foo(4) > 0`’ is checked, even though it might be
an integral part of evaluating that condition. Note that this is not
a problem for the more usual recursive type of rule, such as
‘`foo(x) := foo(x/2)`’, because there the rule has succeeded and
been reactivated by the time the righthand side is evaluated.

If `EvalRules` has no stored value (its default state), or if
anything but a vector is stored in it, then it is ignored.

Even though Calc’s rewrite mechanism is designed to compare rewrite
rules to formulas as quickly as possible, storing rules in
`EvalRules` may make Calc run substantially slower. This is
particularly true of rules where the top-level call is a commonly used
function, or is not fixed. The rule ‘`f(n) := n f(n-1) :: n>0`’ will
only activate the rewrite mechanism for calls to the function `f`,
but ‘`lg(n) + lg(m) := lg(n m)`’ will check every ‘`+`’ operator.

```
apply(f, [a*b]) := apply(f, [a]) + apply(f, [b]) :: in(f, [ln, log10])
```

may seem more “efficient” than two separate rules for `ln` and
`log10`, but actually it is vastly less efficient because rules
with `apply` as the top-level pattern must be tested against
*every* function call that is simplified.

Suppose you want ‘`sin(a + b)`’ to be expanded out not all the time,
but only when algebraic simplifications are used to simplify the
formula. The variable `AlgSimpRules` holds rules for this purpose.
The `a s` command will apply `EvalRules` and
`AlgSimpRules` to the formula, as well as all of its built-in
simplifications.

Most of the special limitations for `EvalRules` don’t apply to
`AlgSimpRules`. Calc simply does an `a r AlgSimpRules`
command with an infinite repeat count as the first step of algebraic
simplifications. It then applies its own built-in simplifications
throughout the formula, and then repeats these two steps (along with
applying the default simplifications) until no further changes are
possible.

There are also `ExtSimpRules` and `UnitSimpRules` variables
that are used by `a e` and `u s`, respectively; these commands
also apply `EvalRules` and `AlgSimpRules`. The variable
`IntegSimpRules` contains simplification rules that are used
only during integration by `a i`.

---

Next: [Examples of Rewrite Rules](#Examples-of-Rewrite-Rules), Previous: [Automatic Rewrites](#Automatic-Rewrites), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.12 Debugging Rewrites [¶](#Debugging-Rewrites-1)

If a buffer named `*Trace*` exists, the rewrite mechanism will
record some useful information there as it operates. The original
formula is written there, as is the result of each successful rewrite,
and the final result of the rewriting. All phase changes are also
noted.

Calc always appends to `*Trace*`. You must empty this buffer
yourself periodically if it is in danger of growing unwieldy.

Note that the rewriting mechanism is substantially slower when the
`*Trace*` buffer exists, even if the buffer is not visible on
the screen. Once you are done, you will probably want to kill this
buffer (with `C-x k *Trace* RET`). If you leave it in
existence and forget about it, all your future rewrite commands will
be needlessly slow.

---

Previous: [Debugging Rewrites](#Debugging-Rewrites), Up: [Rewrite Rules](#Rewrite-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 10.11.13 Examples of Rewrite Rules [¶](#Examples-of-Rewrite-Rules-1)

Returning to the example of substituting the pattern
‘`sin(x)^2 + cos(x)^2`’ with 1, we saw that the rule
‘`opt(a) sin(x)^2 + opt(a) cos(x)^2 := a`’ does a good job of
finding suitable cases. Another solution would be to use the rule
‘`cos(x)^2 := 1 - sin(x)^2`’, followed by algebraic simplification
if necessary. This rule will be the most effective way to do the job,
but at the expense of making some changes that you might not desire.

Another algebraic rewrite rule is ‘`exp(x+y) := exp(x) exp(y)`’.
To make this work with the `j r` command so that it can be
easily targeted to a particular exponential in a large formula,
you might wish to write the rule as ‘`select(exp(x+y)) :=
select(exp(x) exp(y))`’. The ‘`select`’ markers will be
ignored by the regular `a r` command
(see [Selections with Rewrite Rules](#Selections-with-Rewrite-Rules)).

A surprisingly useful rewrite rule is ‘`a/(b-c) := a*(b+c)/(b^2-c^2)`’.
This will simplify the formula whenever ‘`b`’ and/or ‘`c`’ can
be made simpler by squaring. For example, applying this rule to
‘`2 / (sqrt(2) + 3)`’ yields ‘`6:7 - 2:7 sqrt(2)`’ (assuming
Symbolic mode has been enabled to keep the square root from being
evaluated to a floating-point approximation). This rule is also
useful when working with symbolic complex numbers, e.g.,
‘`(a + b i) / (c + d i)`’.

As another example, we could define our own “triangular numbers” function
with the rules ‘`[tri(0) := 0, tri(n) := n + tri(n-1) :: n>0]`’. Enter
this vector and store it in a variable: `s t trirules`. Now, given
a suitable formula like ‘`tri(5)`’ on the stack, type ‘`a r trirules`’
to apply these rules repeatedly. After six applications, `a r` will
stop with 15 on the stack. Once these rules are debugged, it would probably
be most useful to add them to `EvalRules` so that Calc will evaluate
the new `tri` function automatically. We could then use `Z K` on
the keyboard macro `' tri($) RET` to make a command that applies
`tri` to the value on the top of the stack. See [Programming](#Programming).

The following rule set, contributed by François
Pinard, implements *quaternions*, a generalization of the concept of
complex numbers. Quaternions have four components, and are here
represented by function calls ‘`quat(w, [x, y,
z])`’ with “real part” w and the three “imaginary” parts
collected into a vector. Various arithmetical operations on quaternions
are supported. To use these rules, either add them to `EvalRules`,
or create a command based on `a r` for simplifying quaternion
formulas. A convenient way to enter quaternions would be a command
defined by a keyboard macro containing: `' quat($$$$, [$$$, $$, $])
RET`.

```
[ quat(w, x, y, z) := quat(w, [x, y, z]),
  quat(w, [0, 0, 0]) := w,
  abs(quat(w, v)) := hypot(w, v),
  -quat(w, v) := quat(-w, -v),
  r + quat(w, v) := quat(r + w, v) :: real(r),
  r - quat(w, v) := quat(r - w, -v) :: real(r),
  quat(w1, v1) + quat(w2, v2) := quat(w1 + w2, v1 + v2),
  r * quat(w, v) := quat(r * w, r * v) :: real(r),
  plain(quat(w1, v1) * quat(w2, v2))
     := quat(w1 * w2 - v1 * v2, w1 * v2 + w2 * v1 + cross(v1, v2)),
  quat(w1, v1) / r := quat(w1 / r, v1 / r) :: real(r),
  z / quat(w, v) := z * quatinv(quat(w, v)),
  quatinv(quat(w, v)) := quat(w, -v) / (w^2 + v^2),
  quatsqr(quat(w, v)) := quat(w^2 - v^2, 2 * w * v),
  quat(w, v)^k := quatsqr(quat(w, v)^(k / 2))
               :: integer(k) :: k > 0 :: k % 2 = 0,
  quat(w, v)^k := quatsqr(quat(w, v)^((k - 1) / 2)) * quat(w, v)
               :: integer(k) :: k > 2,
  quat(w, v)^-k := quatinv(quat(w, v)^k) :: integer(k) :: k > 0 ]
```

Quaternions, like matrices, have non-commutative multiplication.
In other words, ‘`q1 * q2 = q2 * q1`’ is not necessarily true if
‘`q1`’ and ‘`q2`’ are `quat` forms. The ‘`quat*quat`’
rule above uses `plain` to prevent Calc from rearranging the
product. It may also be wise to add the line ‘`[quat(), matrix]`’
to the `Decls` matrix, to ensure that Calc’s other algebraic
operations will not rearrange a quaternion product. See [Declarations](#Declarations).

These rules also accept a four-argument `quat` form, converting
it to the preferred form in the first rule. If you would rather see
results in the four-argument form, just append the two items
‘`phase(2), quat(w, [x, y, z]) := quat(w, x, y, z)`’ to the end
of the rule set. (But remember that multi-phase rule sets don’t work
in `EvalRules`.)

---

Next: [Storing and Recalling](#Store-and-Recall), Previous: [Algebra](#Algebra), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 11 Operating on Units [¶](#Operating-on-Units)

One special interpretation of algebraic formulas is as numbers with units.
For example, the formula ‘`5 m / s^2`’ can be read “five meters
per second squared.” The commands in this chapter help you
manipulate units expressions in this form. Units-related commands
begin with the `u` prefix key.

* [Basic Operations on Units](#Basic-Operations-on-Units)
* [The Units Table](#The-Units-Table)
* [Predefined Units](#Predefined-Units)
* [User-Defined Units](#User_002dDefined-Units)
* [Logarithmic Units](#Logarithmic-Units)
* [Musical Notes](#Musical-Notes)

---

Next: [The Units Table](#The-Units-Table), Up: [Operating on Units](#Units)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
