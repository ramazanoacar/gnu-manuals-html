### 17.5 Programming Calc, with Lisp [¶](#Programming-Calc_002c-with-Lisp)

The Calculator can be programmed quite extensively in Lisp. All you
do is write a normal Lisp function definition, but with `defmath`
in place of `defun`. This has the same form as `defun`, but it
automagically replaces calls to standard Lisp functions like `+` and
`zerop` with calls to the corresponding functions in Calc’s own library.
Thus you can write natural-looking Lisp code which operates on all of the
standard Calculator data types. You can then use `Z D` if you wish to
bind your new command to a `z`-prefix key sequence. The `Z E` command
will not edit a Lisp-based definition.

Emacs Lisp is described in the GNU Emacs Lisp Reference Manual. This section
assumes a familiarity with Lisp programming concepts; if you do not know
Lisp, you may find keyboard macros or rewrite rules to be an easier way
to program the Calculator.

This section first discusses ways to write commands, functions, or
small programs to be executed inside of Calc. Then it discusses how
your own separate programs are able to call Calc from the outside.
Finally, there is a list of internal Calc functions and data structures
for the true Lisp enthusiast.

* [Defining New Functions](#Defining-Functions)
* [Defining New Simple Commands](#Defining-Simple-Commands)
* [Defining New Stack-Based Commands](#Defining-Stack-Commands)
* [Argument Qualifiers](#Argument-Qualifiers)
* [Example Definitions](#Example-Definitions)
* [Calling Calc from Your Lisp Programs](#Calling-Calc-from-Your-Programs)
* [Calculator Internals](#Internals)

---

Next: [Defining New Simple Commands](#Defining-Simple-Commands), Up: [Programming Calc, with Lisp](#Lisp-Definitions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.1 Defining New Functions [¶](#Defining-New-Functions)

The `defmath` function (actually a Lisp macro) is like `defun`
except that code in the body of the definition can make use of the full
range of Calculator data types. The prefix ‘`calcFunc-`’ is added
to the specified name to get the actual Lisp function name. As a simple
example,

```
(defmath myfact (n)
  (if (> n 0)
      (* n (myfact (1- n)))
    1))
```

This actually expands to the code,

```
(defun calcFunc-myfact (n)
  (if (math-posp n)
      (math-mul n (calcFunc-myfact (math-add n -1)))
    1))
```

This function can be used in algebraic expressions, e.g., ‘`myfact(5)`’.

The ‘`myfact`’ function as it is defined above has the bug that an
expression ‘`myfact(a+b)`’ will be simplified to 1 because the
formula ‘`a+b`’ is not considered to be `posp`. A robust
factorial function would be written along the following lines:

```
(defmath myfact (n)
  (if (> n 0)
      (* n (myfact (1- n)))
    (if (= n 0)
        1
      nil)))    ; this could be simplified as: (and (= n 0) 1)
```

If a function returns `nil`, it is left unsimplified by the Calculator
(except that its arguments will be simplified). Thus, ‘`myfact(a+1+2)`’
will be simplified to ‘`myfact(a+3)`’ but no further. Beware that every
time the Calculator reexamines this formula it will attempt to resimplify
it, so your function ought to detect the returning-`nil` case as
efficiently as possible.

The following standard Lisp functions are treated by `defmath`:
`+`, `-`, `*`, `/`, `%`, `^` or
`expt`, `=`, `<`, `>`, `<=`, `>=`,
`/=`, `1+`, `1-`, `logand`, `logior`, `logxor`,
`logandc2`, `lognot`. Also, `~=` is an abbreviation for
`math-nearly-equal`, which is useful in implementing Taylor series.

For other functions func, if a function by the name
‘`calcFunc-func`’ exists it is used, otherwise if a function by the
name ‘`math-func`’ exists it is used, otherwise if func itself
is defined as a function it is used, otherwise ‘`calcFunc-func`’ is
used on the assumption that this is a to-be-defined math function. Also, if
the function name is quoted as in ‘`('integerp a)`’ the function name is
always used exactly as written (but not quoted).

Variable names have ‘`var-`’ prepended to them unless they appear in
the function’s argument list or in an enclosing `let`, `let*`,
`for`, or `foreach` form,
or their names already contain a ‘`-`’ character. Thus a reference to
‘`foo`’ is the same as a reference to ‘`var-foo`’.

A few other Lisp extensions are available in `defmath` definitions:

* The `elt` function accepts any number of index variables.
  Note that Calc vectors are stored as Lisp lists whose first
  element is the symbol `vec`; thus, ‘`(elt v 2)`’ yields
  the second element of vector `v`, and ‘`(elt m i j)`’
  yields one element of a Calc matrix.
* The `setq` function has been extended to act like the Common
  Lisp `setf` function. (The name `setf` is recognized as
  a synonym of `setq`.) Specifically, the first argument of
  `setq` can be an `nth`, `elt`, `car`, or `cdr` form,
  in which case the effect is to store into the specified
  element of a list. Thus, ‘`(setq (elt m i j) x)`’ stores ‘`x`’
  into one element of a matrix.
* A `for` looping construct is available. For example,
  ‘`(for ((i 0 10)) body)`’ executes `body` once for each
  binding of ‘`i`’ from zero to 10. This is like a `let`
  form in that ‘`i`’ is temporarily bound to the loop count
  without disturbing its value outside the `for` construct.
  Nested loops, as in ‘`(for ((i 0 10) (j 0 (1- i) 2)) body)`’,
  are also available. For each value of ‘`i`’ from zero to 10,
  ‘`j`’ counts from 0 to ‘`i-1`’ in steps of two. Note that
  `for` has the same general outline as `let*`, except
  that each element of the header is a list of three or four
  things, not just two.
* The `foreach` construct loops over elements of a list.
  For example, ‘`(foreach ((x (cdr v))) body)`’ executes
  `body` with ‘`x`’ bound to each element of Calc vector
  ‘`v`’ in turn. The purpose of `cdr` here is to skip over
  the initial `vec` symbol in the vector.
* The `break` function breaks out of the innermost enclosing
  `while`, `for`, or `foreach` loop. If given a
  value, as in ‘`(break x)`’, this value is returned by the
  loop. (Lisp loops otherwise always return `nil`.)
* The `return` function prematurely returns from the enclosing
  function. For example, ‘`(return (+ x y))`’ returns ‘`x+y`’
  as the value of a function. You can use `return` anywhere
  inside the body of the function.

Non-integer numbers cannot be included
directly into a `defmath` definition. This is because the Lisp
reader will fail to parse them long before `defmath` ever gets control.
Instead, use the notation, ‘`:"3.1415"`’. In fact, any algebraic
formula can go between the quotes. For example,

```
(defmath sqexp (x)     ; sqexp(x) == sqrt(exp(x)) == exp(x*0.5)
  (and (numberp x)
       (exp :"x * 0.5")))
```

expands to

```
(defun calcFunc-sqexp (x)
  (and (math-numberp x)
       (calcFunc-exp (math-mul x '(float 5 -1)))))
```

Note the use of `numberp` as a guard to ensure that the argument is
a number first, returning `nil` if not. The exponential function
could itself have been included in the expression, if we had preferred:
‘`:"exp(x * 0.5)"`’. As another example, the multiplication-and-recursion
step of `myfact` could have been written

```
:"n * myfact(n-1)"
```

A good place to put your `defmath` commands is your Calc init file
(the file given by `calc-settings-file`, typically
`~/.emacs.d/calc.el`), which will not be loaded until Calc starts.
If a file named `.emacs` exists in your home directory, Emacs reads
and executes the Lisp forms in this file as it starts up. While it may
seem reasonable to put your favorite `defmath` commands there,
this has the unfortunate side-effect that parts of the Calculator must be
loaded in to process the `defmath` commands whether or not you will
actually use the Calculator! If you want to put the `defmath`
commands there (for example, if you redefine `calc-settings-file`
to be `.emacs`), a better effect can be had by writing

```
(put 'calc-define 'thing '(progn
 (defmath ... )
 (defmath ... )
))
```

The `put` function adds a *property* to a symbol. Each Lisp
symbol has a list of properties associated with it. Here we add a
property with a name of `thing` and a ‘`(progn ...)`’ form as
its value. When Calc starts up, and at the start of every Calc command,
the property list for the symbol `calc-define` is checked and the
values of any properties found are evaluated as Lisp forms. The
properties are removed as they are evaluated. The property names
(like `thing`) are not used; you should choose something like the
name of your project so as not to conflict with other properties.

The net effect is that you can put the above code in your `.emacs`
file and it will not be executed until Calc is loaded. Or, you can put
that same code in another file which you load by hand either before or
after Calc itself is loaded.

The properties of `calc-define` are evaluated in the same order
that they were added. They can assume that the Calc modules `calc.el`,
`calc-ext.el`, and `calc-macs.el` have been fully loaded, and
that the `*Calculator*` buffer will be the current buffer.

If your `calc-define` property only defines algebraic functions,
you can be sure that it will have been evaluated before Calc tries to
call your function, even if the file defining the property is loaded
after Calc is loaded. But if the property defines commands or key
sequences, it may not be evaluated soon enough. (Suppose it defines the
new command `tweak-calc`; the user can load your file, then type
`M-x tweak-calc` before Calc has had chance to do anything.) To
protect against this situation, you can put

```
(run-hooks 'calc-check-defines)
```

at the end of your file. The `calc-check-defines` function is what
looks for and evaluates properties on `calc-define`; `run-hooks`
has the advantage that it is quietly ignored if `calc-check-defines`
is not yet defined because Calc has not yet been loaded.

Examples of things that ought to be enclosed in a `calc-define`
property are `defmath` calls, `keymap-set` calls that modify
the Calc key map, and any calls that redefine things defined inside Calc.
Ordinary `defun`s need not be enclosed with `calc-define`.

---

Next: [Defining New Stack-Based Commands](#Defining-Stack-Commands), Previous: [Defining New Functions](#Defining-Functions), Up: [Programming Calc, with Lisp](#Lisp-Definitions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.2 Defining New Simple Commands [¶](#Defining-New-Simple-Commands)

If a `defmath` form contains an `interactive` clause, it defines
a Calculator command. Actually such a `defmath` results in *two*
function definitions: One, a ‘`calcFunc-`’ function as was just described,
with the `interactive` clause removed. Two, a ‘`calc-`’ function
with a suitable `interactive` clause and some sort of wrapper to make
the command work in the Calc environment.

In the simple case, the `interactive` clause has the same form as
for normal Emacs Lisp commands:

```
(defmath increase-precision (delta)
  "Increase precision by DELTA."     ; This is the "documentation string"
  (interactive "p")                  ; Register this as a M-x-able command
  (setq calc-internal-prec (+ calc-internal-prec delta)))
```

This expands to the pair of definitions,

```
(defun calc-increase-precision (delta)
  "Increase precision by DELTA."
  (interactive "p")
  (calc-wrapper
   (setq calc-internal-prec (math-add calc-internal-prec delta))))

(defun calcFunc-increase-precision (delta)
  "Increase precision by DELTA."
  (setq calc-internal-prec (math-add calc-internal-prec delta)))
```

where in this case the latter function would never really be used! Note
that since the Calculator stores integers as plain Lisp integers,
the `math-add` function will work just as well as the native
`+` even when the intent is to operate on native Lisp integers.

The ‘`calc-wrapper`’ call invokes a macro which surrounds the body of
the function with code that looks roughly like this:

```
(let ((calc-command-flags nil))
  (unwind-protect
      (save-current-buffer
        (calc-select-buffer)
        body of function
        renumber stack
        clear Working message)
    realign cursor and window
    clear Inverse, Hyperbolic, and Keep Args flags
    update Emacs mode line))
```

The `calc-select-buffer` function selects the `*Calculator*`
buffer if necessary, say, because the command was invoked from inside
the `*Calc Trail*` window.

You can call, for example, `(calc-set-command-flag 'no-align)` to
set the above-mentioned command flags. Calc routines recognize the
following command flags:

`renum-stack`
:   Stack line numbers ‘`1:`’, ‘`2:`’, and so on must be renumbered
    after this command completes. This is set by routines like
    `calc-push`.

`clear-message`
:   Calc should call ‘`(message "")`’ if this command completes normally
    (to clear a “Working…” message out of the echo area).

`no-align`
:   Do not move the cursor back to the ‘`.`’ top-of-stack marker.

`position-point`
:   Use the variables `calc-position-point-line` and
    `calc-position-point-column` to position the cursor after
    this command finishes.

`keep-flags`
:   Do not clear `calc-inverse-flag`, `calc-hyperbolic-flag`,
    and `calc-keep-args-flag` at the end of this command.

`do-edit`
:   Switch to buffer `*Calc Edit*` after this command.

`hold-trail`
:   Do not move trail pointer to end of trail when something is recorded
    there.

Calc reserves a special prefix key, shift-`Y`, for user-written
extensions to Calc. There are no built-in commands that work with
this prefix key; you must call `keymap-set` from Lisp (probably
from inside a `calc-define` property) to add to it. Initially only
`Y ?` is defined; it takes help messages from a list of strings
(initially `nil`) in the variable `calc-Y-help-msgs`. All
other undefined keys except for `Y` are reserved for use by
future versions of Calc.

If you are writing a Calc enhancement which you expect to give to
others, it is best to minimize the number of `Y`-key sequences
you use. In fact, if you have more than one key sequence you should
consider defining three-key sequences with a `Y`, then a key that
stands for your package, then a third key for the particular command
within your package.

Users may wish to install several Calc enhancements, and it is possible
that several enhancements will choose to use the same key. In the
example below, a variable `inc-prec-base-key` has been defined
to contain the key that identifies the `inc-prec` package. Its
value is initially `"P"`, but a user can change this variable
if necessary without having to modify the file.

Here is a complete file, `inc-prec.el`, which makes a `Y P I`
command that increases the precision, and a `Y P D` command that
decreases the precision.

```
;;; Increase and decrease Calc precision.  Dave Gillespie, 5/31/91.
;; (Include copyright or copyleft stuff here.)

(defvar inc-prec-base-key "P"
  "Base key for inc-prec.el commands.")

(put 'calc-define 'inc-prec '(progn

(keymap-set calc-mode-map (format "Y %s I" inc-prec-base-key)
            'calc-increase-precision)
(keymap-set calc-mode-map (format "Y %s D" inc-prec-base-key)
            'calc-decrease-precision)

(setq calc-Y-help-msgs
      (cons (format "%s + Inc-prec, Dec-prec" inc-prec-base-key)
            calc-Y-help-msgs))

(defmath increase-precision (delta)
  "Increase precision by DELTA."
  (interactive "p")
  (setq calc-internal-prec (+ calc-internal-prec delta)))

(defmath decrease-precision (delta)
  "Decrease precision by DELTA."
  (interactive "p")
  (setq calc-internal-prec (- calc-internal-prec delta)))

))  ; end of calc-define property

(run-hooks 'calc-check-defines)
```

---

Next: [Argument Qualifiers](#Argument-Qualifiers), Previous: [Defining New Simple Commands](#Defining-Simple-Commands), Up: [Programming Calc, with Lisp](#Lisp-Definitions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.3 Defining New Stack-Based Commands [¶](#Defining-New-Stack_002dBased-Commands)

To define a new computational command which takes and/or leaves arguments
on the stack, a special form of `interactive` clause is used.

```
(interactive num tag)
```

where num is an integer, and tag is a string. The effect is
to pop num values off the stack, resimplify them by calling
`calc-normalize`, and hand them to your function according to the
function’s argument list. Your function may include `&optional` and
`&rest` parameters, so long as calling the function with num
parameters is valid.

Your function must return either a number or a formula in a form
acceptable to Calc, or a list of such numbers or formulas. These value(s)
are pushed onto the stack when the function completes. They are also
recorded in the Calc Trail buffer on a line beginning with tag,
a string of (normally) four characters or less. If you omit tag
or use `nil` as a tag, the result is not recorded in the trail.

As an example, the definition

```
(defmath myfact (n)
  "Compute the factorial of the integer at the top of the stack."
  (interactive 1 "fact")
  (if (> n 0)
      (* n (myfact (1- n)))
    (and (= n 0) 1)))
```

is a version of the factorial function shown previously which can be used
as a command as well as an algebraic function. It expands to

```
(defun calc-myfact ()
  "Compute the factorial of the integer at the top of the stack."
  (interactive)
  (calc-slow-wrapper
   (calc-enter-result 1 "fact"
     (cons 'calcFunc-myfact (calc-top-list-n 1)))))

(defun calcFunc-myfact (n)
  "Compute the factorial of the integer at the top of the stack."
  (if (math-posp n)
      (math-mul n (calcFunc-myfact (math-add n -1)))
    (and (math-zerop n) 1)))
```

The `calc-slow-wrapper` function is a version of `calc-wrapper`
that automatically puts up a ‘`Working...`’ message before the
computation begins. (This message can be turned off by the user
with an `m w` (`calc-working`) command.)

The `calc-top-list-n` function returns a list of the specified number
of values from the top of the stack. It resimplifies each value by
calling `calc-normalize`. If its argument is zero it returns an
empty list. It does not actually remove these values from the stack.

The `calc-enter-result` function takes an integer num and string
tag as described above, plus a third argument which is either a
Calculator data object or a list of such objects. These objects are
resimplified and pushed onto the stack after popping the specified number
of values from the stack. If tag is non-`nil`, the values
being pushed are also recorded in the trail.

Note that if `calcFunc-myfact` returns `nil` this represents
“leave the function in symbolic form.” To return an actual empty list,
in the sense that `calc-enter-result` will push zero elements back
onto the stack, you should return the special value ‘`'(nil)`’, a list
containing the single symbol `nil`.

The `interactive` declaration can actually contain a limited
Emacs-style code string as well which comes just before num and
tag. Currently the only Emacs code supported is ‘`"p"`’, as in

```
(defmath foo (a b &optional c)
  (interactive "p" 2 "foo")
  body)
```

In this example, the command `calc-foo` will evaluate the expression
‘`foo(a,b)`’ if executed with no argument, or ‘`foo(a,b,n)`’ if
executed with a numeric prefix argument of ‘`n`’.

The other code string allowed is ‘`"m"`’ (unrelated to the usual ‘`"m"`’
code as used with `defun`). It uses the numeric prefix argument as the
number of objects to remove from the stack and pass to the function.
In this case, the integer num serves as a default number of
arguments to be used when no prefix is supplied.

---

Next: [Example Definitions](#Example-Definitions), Previous: [Defining New Stack-Based Commands](#Defining-Stack-Commands), Up: [Programming Calc, with Lisp](#Lisp-Definitions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.4 Argument Qualifiers [¶](#Argument-Qualifiers-1)

Anywhere a parameter name can appear in the parameter list you can also use
an *argument qualifier*. Thus the general form of a definition is:

```
(defmath name (param param...
               &optional param param...
               &rest param)
  body)
```

where each param is either a symbol or a list of the form

```
(qual param)
```

The following qualifiers are recognized:

‘`complete`’ [¶](#index-complete)
:   The argument must not be an incomplete vector, interval, or complex number.
    (This is rarely needed since the Calculator itself will never call your
    function with an incomplete argument. But there is nothing stopping your
    own Lisp code from calling your function with an incomplete argument.)

‘`integer`’ [¶](#index-integer)
:   The argument must be an integer. If it is an integer-valued float
    it will be accepted but converted to integer form. Non-integers and
    formulas are rejected.

‘`natnum`’ [¶](#index-natnum)
:   Like ‘`integer`’, but the argument must be non-negative.

‘`fixnum`’ [¶](#index-fixnum)
:   Like ‘`integer`’, but the argument must fit into a native Lisp fixnum,
    which on most systems means less than 2^61 in absolute value. The
    argument is converted into Lisp-integer form if necessary.

‘`float`’ [¶](#index-float)
:   The argument is converted to floating-point format if it is a number or
    vector. If it is a formula it is left alone. (The argument is never
    actually rejected by this qualifier.)

‘`pred`’
:   The argument must satisfy predicate pred, which is one of the
    standard Calculator predicates. See [Predicates](#Predicates).

‘`not-pred`’
:   The argument must *not* satisfy predicate pred.

For example,

```
(defmath foo (a (constp (not-matrixp b)) &optional (float c)
              &rest (integer d))
  body)
```

expands to

```
(defun calcFunc-foo (a b &optional c &rest d)
  (and (math-matrixp b)
       (math-reject-arg b 'not-matrixp))
  (or (math-constp b)
      (math-reject-arg b 'constp))
  (and c (setq c (math-check-float c)))
  (setq d (mapcar 'math-check-integer d))
  body)
```

which performs the necessary checks and conversions before executing the
body of the function.

---

Next: [Calling Calc from Your Lisp Programs](#Calling-Calc-from-Your-Programs), Previous: [Argument Qualifiers](#Argument-Qualifiers), Up: [Programming Calc, with Lisp](#Lisp-Definitions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.5 Example Definitions [¶](#Example-Definitions-1)

This section includes some Lisp programming examples on a larger scale.
These programs make use of some of the Calculator’s internal functions;
see [Calculator Internals](#Internals).

* [Bit-Counting](#Bit-Counting-Example)
* [The Sine Function](#Sine-Example)

---

Next: [The Sine Function](#Sine-Example), Up: [Example Definitions](#Example-Definitions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.5.1 Bit-Counting [¶](#Bit_002dCounting)

Calc does not include a built-in function for counting the number of
“one” bits in a binary integer. It’s easy to invent one using `b u`
to convert the integer to a set, and `V #` to count the elements of
that set; let’s write a function that counts the bits without having to
create an intermediate set.

```
(defmath bcount ((natnum n))
  (interactive 1 "bcnt")
  (let ((count 0))
    (while (> n 0)
      (if (oddp n)
          (setq count (1+ count)))
      (setq n (ash n -1)))
    count))
```

When this is expanded by `defmath`, it will become the following
Emacs Lisp function:

```
(defun calcFunc-bcount (n)
  (setq n (math-check-natnum n))
  (let ((count 0))
    (while (math-posp n)
      (if (math-oddp n)
          (setq count (math-add count 1)))
      (setq n (calcFunc-lsh n -1)))
    count))
```

---

Previous: [Bit-Counting](#Bit-Counting-Example), Up: [Example Definitions](#Example-Definitions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.5.2 The Sine Function [¶](#The-Sine-Function)

A somewhat limited sine function could be defined as follows, using the
well-known Taylor series expansion for
‘`sin(x)`’:

```
(defmath mysin ((float (anglep x)))
  (interactive 1 "mysn")
  (setq x (to-radians x))    ; Convert from current angular mode.
  (let ((sum x)              ; Initial term of Taylor expansion of sin.
        newsum
        (nfact 1)            ; "nfact" equals "n" factorial at all times.
        (xnegsqr :"-(x^2)")) ; "xnegsqr" equals -x^2.
    (for ((n 3 100 2))       ; Upper limit of 100 is a good precaution.
      (working "mysin" sum)  ; Display "Working" message, if enabled.
      (setq nfact (* nfact (1- n) n)
            x (* x xnegsqr)
            newsum (+ sum (/ x nfact)))
      (if (~= newsum sum)    ; If newsum is "nearly equal to" sum,
          (break))           ;  then we are done.
      (setq sum newsum))
    sum))
```

The actual `sin` function in Calc works by first reducing the problem
to a sine or cosine of a nonnegative number less than ‘`pi/4`’. This
ensures that the Taylor series will converge quickly. Also, the calculation
is carried out with two extra digits of precision to guard against cumulative
round-off in ‘`sum`’. Finally, complex arguments are allowed and handled
by a separate algorithm.

```
(defmath mysin ((float (scalarp x)))
  (interactive 1 "mysn")
  (setq x (to-radians x))    ; Convert from current angular mode.
  (with-extra-prec 2         ; Evaluate with extra precision.
    (cond ((complexp x)
           (mysin-complex x))
          ((< x 0)
           (- (mysin-raw (- x)))    ; Always call mysin-raw with x >= 0.
          (t (mysin-raw x))))))

(defmath mysin-raw (x)
  (cond ((>= x 7)
         (mysin-raw (% x (two-pi))))     ; Now x < 7.
        ((> x (pi-over-2))
         (- (mysin-raw (- x (pi)))))     ; Now -pi/2 <= x <= pi/2.
        ((> x (pi-over-4))
         (mycos-raw (- x (pi-over-2))))  ; Now -pi/2 <= x <= pi/4.
        ((< x (- (pi-over-4)))
         (- (mycos-raw (+ x (pi-over-2)))))  ; Now -pi/4 <= x <= pi/4,
        (t (mysin-series x))))           ; so the series will be efficient.
```

where `mysin-complex` is an appropriate function to handle complex
numbers, `mysin-series` is the routine to compute the sine Taylor
series as before, and `mycos-raw` is a function analogous to
`mysin-raw` for cosines.

The strategy is to ensure that ‘`x`’ is nonnegative before calling
`mysin-raw`. This function then recursively reduces its argument
to a suitable range, namely, plus-or-minus ‘`pi/4`’. Note that each
test, and particularly the first comparison against 7, is designed so
that small roundoff errors cannot produce an infinite loop. (Suppose
we compared with ‘`(two-pi)`’ instead; if due to roundoff problems
the modulo operator ever returned ‘`(two-pi)`’ exactly, an infinite
recursion could result!) We use modulo only for arguments that will
clearly get reduced, knowing that the next rule will catch any reductions
that this rule misses.

If a program is being written for general use, it is important to code
it carefully as shown in this second example. For quick-and-dirty programs,
when you know that your own use of the sine function will never encounter
a large argument, a simpler program like the first one shown is fine.

---

Next: [Calculator Internals](#Internals), Previous: [Example Definitions](#Example-Definitions), Up: [Programming Calc, with Lisp](#Lisp-Definitions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.6 Calling Calc from Your Lisp Programs [¶](#Calling-Calc-from-Your-Lisp-Programs)

A later section (see [Calculator Internals](#Internals)) gives a full description of
Calc’s internal Lisp functions. It’s not hard to call Calc from
inside your programs, but the number of these functions can be daunting.
So Calc provides one special “programmer-friendly” function called
`calc-eval` that can be made to do just about everything you
need. It’s not as fast as the low-level Calc functions, but it’s
much simpler to use!

It may seem that `calc-eval` itself has a daunting number of
options, but they all stem from one simple operation.

In its simplest manifestation, ‘`(calc-eval "1+2")`’ parses the
string `"1+2"` as if it were a Calc algebraic entry and returns
the result formatted as a string: `"3"`.

Since `calc-eval` is on the list of recommended `autoload`
functions, you don’t need to make any special preparations to load
Calc before calling `calc-eval` the first time. Calc will be
loaded and initialized for you.

All the Calc modes that are currently in effect will be used when
evaluating the expression and formatting the result.

* [Additional Arguments to `calc-eval`](#Additional-Arguments-to-calc_002deval)
* [Error Handling](#Error-Handling)
* [Numbers Only](#Numbers-Only)
* [Default Modes](#Default-Modes)
* [Raw Numbers](#Raw-Numbers)
* [Predicates](#Predicates-1)
* [Variable Values](#Variable-Values)
* [Stack Access](#Stack-Access)
* [Keyboard Macros](#Keyboard-Macros-1)
* [Lisp Evaluation](#Lisp-Evaluation)
* [Example](#Example)

#### 17.5.6.1 Additional Arguments to `calc-eval` [¶](#Additional-Arguments-to-calc_002deval)

If the input string parses to a list of expressions, Calc returns
the results separated by `", "`. You can specify a different
separator by giving a second string argument to `calc-eval`:
‘`(calc-eval "1+2,3+4" ";")`’ returns `"3;7"`.

The “separator” can also be any of several Lisp symbols which
request other behaviors from `calc-eval`. These are discussed
one by one below.

You can give additional arguments to be substituted for
‘`$`’, ‘`$$`’, and so on in the main expression. For
example, ‘`(calc-eval "$/$$" nil "7" "1+1")`’ evaluates the
expression `"7/(1+1)"` to yield the result `"3.5"`
(assuming Fraction mode is not in effect). Note the `nil`
used as a placeholder for the item-separator argument.

#### 17.5.6.2 Error Handling [¶](#Error-Handling)

If `calc-eval` encounters an error, it returns a list containing
the character position of the error, plus a suitable message as a
string. Note that ‘`1 / 0`’ is *not* an error by Calc’s
standards; it simply returns the string `"1 / 0"` which is the
division left in symbolic form. But ‘`(calc-eval "1/")`’ will
return the list ‘`(2 "Expected a number")`’.

If you bind the variable `calc-eval-error` to `t`
using a `let` form surrounding the call to `calc-eval`,
errors instead call the Emacs `error` function which aborts
to the Emacs command loop with a beep and an error message.

If you bind this variable to the symbol `string`, error messages
are returned as strings instead of lists. The character position is
ignored.

As a courtesy to other Lisp code which may be using Calc, be sure
to bind `calc-eval-error` using `let` rather than changing
it permanently with `setq`.

#### 17.5.6.3 Numbers Only [¶](#Numbers-Only)

Sometimes it is preferable to treat ‘`1 / 0`’ as an error
rather than returning a symbolic result. If you pass the symbol
`num` as the second argument to `calc-eval`, results
that are not constants are treated as errors. The error message
reported is the first `calc-why` message if there is one,
or otherwise “Number expected.”

A result is “constant” if it is a number, vector, or other
object that does not include variables or function calls. If it
is a vector, the components must themselves be constants.

#### 17.5.6.4 Default Modes [¶](#Default-Modes)

If the first argument to `calc-eval` is a list whose first
element is a formula string, then `calc-eval` sets all the
various Calc modes to their default values while the formula is
evaluated and formatted. For example, the precision is set to 12
digits, digit grouping is turned off, and the Normal language
mode is used.

This same principle applies to the other options discussed below.
If the first argument would normally be x, then it can also
be the list ‘`(x)`’ to use the default mode settings.

If there are other elements in the list, they are taken as
variable-name/value pairs which override the default mode
settings. Look at the documentation at the front of the
`calc.el` file to find the names of the Lisp variables for
the various modes. The mode settings are restored to their
original values when `calc-eval` is done.

For example, ‘`(calc-eval '("$+$$" calc-internal-prec 8) 'num a b)`’
computes the sum of two numbers, requiring a numeric result, and
using default mode settings except that the precision is 8 instead
of the default of 12.

It’s usually best to use this form of `calc-eval` unless your
program actually considers the interaction with Calc’s mode settings
to be a feature. This will avoid all sorts of potential “gotchas”;
consider what happens with ‘`(calc-eval "sqrt(2)" 'num)`’
when the user has left Calc in Symbolic mode or No-Simplify mode.

As another example, ‘`(equal (calc-eval '("$<$$") nil a b) "1")`’
checks if the number in string ‘`a`’ is less than the one in
string ‘`b`’. Without using a list, the integer 1 might
come out in a variety of formats which would be hard to test for
conveniently: `"1"`, `"8#1"`, `"00001"`. (But
see “Predicates” mode, below.)

#### 17.5.6.5 Raw Numbers [¶](#Raw-Numbers)

Normally all input and output for `calc-eval` is done with strings.
You can do arithmetic with, say, ‘`(calc-eval "$+$$" nil a b)`’
in place of ‘`(+ a b)`’, but this is very inefficient since the
numbers must be converted to and from string format as they are passed
from one `calc-eval` to the next.

If the separator is the symbol `raw`, the result will be returned
as a raw Calc data structure rather than a string. You can read about
how these objects look in the following sections, but usually you can
treat them as “black box” objects with no important internal
structure.

There is also a `rawnum` symbol, which is a combination of
`raw` (returning a raw Calc object) and `num` (signaling
an error if that object is not a constant).

You can pass a raw Calc object to `calc-eval` in place of a
string, either as the formula itself or as one of the ‘`$`’
arguments. Thus ‘`(calc-eval "$+$$" 'raw a b)`’ is an
addition function that operates on raw Calc objects. Of course
in this case it would be easier to call the low-level `math-add`
function in Calc, if you can remember its name.

In particular, note that a plain Lisp integer is acceptable to Calc
as a raw object.

When it comes time to display the object, just use ‘`(calc-eval a)`’
to format it as a string.

It is an error if the input expression evaluates to a list of
values. The separator symbol `list` is like `raw`
except that it returns a list of one or more raw Calc objects.

Note that a Lisp string is not a valid Calc object, nor is a list
containing a string. Thus you can still safely distinguish all the
various kinds of error returns discussed above.

#### 17.5.6.6 Predicates [¶](#Predicates-1)

If the separator symbol is `pred`, the result of the formula is
treated as a true/false value; `calc-eval` returns `t` or
`nil`, respectively. A value is considered “true” if it is a
non-zero number, or false if it is zero or if it is not a number.

For example, ‘`(calc-eval "$<$$" 'pred a b)`’ tests whether
one value is less than another.

As usual, it is also possible for `calc-eval` to return one of
the error indicators described above. Lisp will interpret such an
indicator as “true” if you don’t check for it explicitly. If you
wish to have an error register as “false”, use something like
‘`(eq (calc-eval ...) t)`’.

#### 17.5.6.7 Variable Values [¶](#Variable-Values)

Variables in the formula passed to `calc-eval` are not normally
replaced by their values. If you wish this, you can use the
`evalv` function (see [Algebraic Manipulation](#Algebraic-Manipulation)). For example,
if 4 is stored in Calc variable `a` (i.e., in Lisp variable
`var-a`), then ‘`(calc-eval "a+pi")`’ will return the
formula `"a + pi"`, but ‘`(calc-eval "evalv(a+pi)")`’
will return `"7.14159265359"`.

To store in a Calc variable, just use `setq` to store in the
corresponding Lisp variable. (This is obtained by prepending
‘`var-`’ to the Calc variable name.) Calc routines will
understand either string or raw form values stored in variables,
although raw data objects are much more efficient. For example,
to increment the Calc variable `a`:

```
(setq var-a (calc-eval "evalv(a+1)" 'raw))
```

#### 17.5.6.8 Stack Access [¶](#Stack-Access)

If the separator symbol is `push`, the formula argument is
evaluated (with possible ‘`$`’ expansions, as usual). The
result is pushed onto the Calc stack. The return value is `nil`
(unless there is an error from evaluating the formula, in which
case the return value depends on `calc-eval-error` in the
usual way).

If the separator symbol is `pop`, the first argument to
`calc-eval` must be an integer instead of a string. That
many values are popped from the stack and thrown away. A negative
argument deletes the entry at that stack level. The return value
is the number of elements remaining in the stack after popping;
‘`(calc-eval 0 'pop)`’ is a good way to measure the size of
the stack.

If the separator symbol is `top`, the first argument to
`calc-eval` must again be an integer. The value at that
stack level is formatted as a string and returned. Thus
‘`(calc-eval 1 'top)`’ returns the top-of-stack value. If the
integer is out of range, `nil` is returned.

The separator symbol `rawtop` is just like `top` except
that the stack entry is returned as a raw Calc object instead of
as a string.

In all of these cases the first argument can be made a list in
order to force the default mode settings, as described above.
Thus ‘`(calc-eval '(2 calc-number-radix 16) 'top)`’ returns the
second-to-top stack entry, formatted as a string using the default
instead of current display modes, except that the radix is
hexadecimal instead of decimal.

It is, of course, polite to put the Calc stack back the way you
found it when you are done, unless the user of your program is
actually expecting it to affect the stack.

Note that you do not actually have to switch into the `*Calculator*`
buffer in order to use `calc-eval`; it temporarily switches into
the stack buffer if necessary.

#### 17.5.6.9 Keyboard Macros [¶](#Keyboard-Macros-1)

If the separator symbol is `macro`, the first argument must be a
string of characters which Calc can execute as a sequence of keystrokes.
This switches into the Calc buffer for the duration of the macro.
For example, ‘`(calc-eval "vx5\rVR+" 'macro)`’ pushes the
vector ‘`[1,2,3,4,5]`’ on the stack and then replaces it
with the sum of those numbers. Note that ‘`\r`’ is the Lisp
notation for the carriage return, `RET`, character.

If your keyboard macro wishes to pop the stack, ‘`\C-d`’ is
safer than ‘`\177`’ (the `DEL` character) because some
installations may have switched the meanings of `DEL` and
`C-h`. Calc always interprets `C-d` as a synonym for
“pop-stack” regardless of key mapping.

If you provide a third argument to `calc-eval`, evaluation
of the keyboard macro will leave a record in the Trail using
that argument as a tag string. Normally the Trail is unaffected.

The return value in this case is always `nil`.

#### 17.5.6.10 Lisp Evaluation [¶](#Lisp-Evaluation)

Finally, if the separator symbol is `eval`, then the Lisp
`eval` function is called on the first argument, which must
be a Lisp expression rather than a Calc formula. Remember to
quote the expression so that it is not evaluated until inside
`calc-eval`.

The difference from plain `eval` is that `calc-eval`
switches to the Calc buffer before evaluating the expression.
For example, ‘`(calc-eval '(setq calc-internal-prec 17) 'eval)`’
will correctly affect the buffer-local Calc precision variable.

An alternative would be ‘`(calc-eval '(calc-precision 17) 'eval)`’.
This is evaluating a call to the function that is normally invoked
by the `p` key, giving it 17 as its “numeric prefix argument.”
Note that this function will leave a message in the echo area as
a side effect. Also, all Calc functions switch to the Calc buffer
automatically if not invoked from there, so the above call is
also equivalent to ‘`(calc-precision 17)`’ by itself.
In all cases, Calc uses `save-excursion` to switch back to
your original buffer when it is done.

As usual the first argument can be a list that begins with a Lisp
expression to use default instead of current mode settings.

The result of `calc-eval` in this usage is just the result
returned by the evaluated Lisp expression.

#### 17.5.6.11 Example [¶](#Example)

Here is a sample Emacs command that uses `calc-eval`. Suppose
you have a document with lots of references to temperatures on the
Fahrenheit scale, say “98.6 F”, and you wish to convert these
references to Centigrade. The following command does this conversion.
Place the Emacs cursor right after the letter “F” and invoke the
command to change “98.6 F” to “37 C”. Or, if the temperature is
already in Centigrade form, the command changes it back to Fahrenheit.

```
(defun convert-temp ()
  (interactive)
  (save-excursion
    (re-search-backward "[^-.0-9]\\([-.0-9]+\\) *\\([FC]\\)")
    (let* ((top1 (match-beginning 1))
           (bot1 (match-end 1))
           (number (buffer-substring top1 bot1))
           (top2 (match-beginning 2))
           (bot2 (match-end 2))
           (type (buffer-substring top2 bot2)))
      (if (equal type "F")
          (setq type "C"
                number (calc-eval "($ - 32)*5/9" nil number))
        (setq type "F"
              number (calc-eval "$*9/5 + 32" nil number)))
      (goto-char top2)
      (delete-region top2 bot2)
      (insert-before-markers type)
      (goto-char top1)
      (delete-region top1 bot1)
      (if (string-match "\\.$" number)   ; change "37." to "37"
          (setq number (substring number 0 -1)))
      (insert number))))
```

Note the use of `insert-before-markers` when changing between
“F” and “C”, so that the character winds up before the cursor
instead of after it.

---

Previous: [Calling Calc from Your Lisp Programs](#Calling-Calc-from-Your-Programs), Up: [Programming Calc, with Lisp](#Lisp-Definitions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7 Calculator Internals [¶](#Calculator-Internals)

This section describes the Lisp functions defined by the Calculator that
may be of use to user-written Calculator programs (as described in the
rest of this chapter). These functions are shown by their names as they
conventionally appear in `defmath`. Their full Lisp names are
generally gotten by prepending ‘`calcFunc-`’ or ‘`math-`’ to their
apparent names. (Names that begin with ‘`calc-`’ are already in
their full Lisp form.) You can use the actual full names instead if you
prefer them, or if you are calling these functions from regular Lisp.

The functions described here are scattered throughout the various
Calc component files. Note that `calc.el` includes `autoload`s
for only a few component files; to get autoloads of the more advanced
function, one needs to load `calc-ext.el`, which in turn
autoloads all the functions in the remaining component files.

Because `defmath` itself uses the extensions, user-written code
generally always executes with the extensions already loaded, so
normally you can use any Calc function and be confident that it will
be autoloaded for you when necessary. If you are doing something
special, check carefully to make sure each function you are using is
from `calc.el` or its components, and use `(require 'calc-ext)` before using any function based in `calc-ext.el` if
you can’t prove this file will already be loaded.

* [Data Type Formats](#Data-Type-Formats)
* [Interactive Functions](#Interactive-Lisp-Functions)
* [Stack-Oriented Functions](#Stack-Lisp-Functions)
* [Predicates](#Predicates)
* [Computational Functions](#Computational-Lisp-Functions)
* [Vector Functions](#Vector-Lisp-Functions)
* [Symbolic Functions](#Symbolic-Lisp-Functions)
* [I/O and Formatting Functions](#Formatting-Lisp-Functions)
* [Hooks](#Hooks)

---

Next: [Interactive Functions](#Interactive-Lisp-Functions), Up: [Calculator Internals](#Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7.1 Data Type Formats [¶](#Data-Type-Formats-1)

Integers are stored as standard Lisp integers. This is the only
storage format for Calc data objects which is not a Lisp list.

Fractions are stored as a list of the form, ‘`(frac n d)`’
where n is an integer numerator, d is an
integer denominator greater than one, and n and d are relatively
prime. Note that fractions where d is one are automatically converted
to plain integers by all math routines; fractions where d is negative
are normalized by negating the numerator and denominator.

Floating-point numbers are stored in the form, ‘`(float mant
exp)`’, where mant (the “mantissa”) is an integer less than
‘`10^p`’ in absolute value (p represents the current
precision), and exp (the “exponent”) is an integer. The value of
the float is ‘`mant * 10^exp`’. For example, the number
*-3.14* is stored as ‘`(float -314 -2) = -314*10^-2`’. Other constraints
are that the number 0.0 is always stored as ‘`(float 0 0)`’, and,
except for the 0.0 case, the rightmost base-10 digit of mant is
always nonzero. (If the rightmost digit is zero, the number is
rearranged by dividing mant by ten and incrementing exp.)

Rectangular complex numbers are stored in the form ‘`(cplx re
im)`’, where re and im are each real numbers, either
integers, fractions, or floats. The value is ‘`re + imi`’.
The im part is nonzero; complex numbers with zero imaginary
components are converted to real numbers automatically.

Polar complex numbers are stored in the form ‘`(polar r
theta)`’, where r is a positive real value and theta
is a real value or HMS form representing an angle. This angle is
usually normalized to lie in the interval ‘`(-180 .. 180)`’ degrees,
or ‘`(-pi .. pi)`’ radians, according to the current angular mode.
If the angle is 0 the value is converted to a real number automatically.
(If the angle is 180 degrees, the value is usually also converted to a
negative real number.)

Hours-minutes-seconds forms are stored as ‘`(hms h m
s)`’, where h is an integer or an integer-valued float (i.e.,
a float with ‘`exp >= 0`’), m is an integer or integer-valued
float in the range ‘`[0 .. 60)`’, and s is any real number
in the range ‘`[0 .. 60)`’.

Date forms are stored as ‘`(date n)`’, where n is
a real number that counts days since midnight on the morning of
January 1, 1 AD. If n is an integer, this is a pure date
form. If n is a fraction or float, this is a date/time form.

Modulo forms are stored as ‘`(mod n m)`’, where m is a
positive real number or HMS form, and n is a real number or HMS
form in the range ‘`[0 .. m)`’.

Error forms are stored as ‘`(sdev x sigma)`’, where x
is the mean value and sigma is the standard deviation. Each
component is either a number, an HMS form, or a symbolic object
(a variable or function call). If sigma is zero, the value is
converted to a plain real number. If sigma is negative or
complex, it is automatically normalized to be a positive real.

Interval forms are stored as ‘`(intv mask lo hi)`’,
where mask is one of the integers 0, 1, 2, or 3, and lo and
hi are real numbers, HMS forms, or symbolic objects. The mask
is a binary integer where 1 represents the fact that the interval is
closed on the high end, and 2 represents the fact that it is closed on
the low end. (Thus 3 represents a fully closed interval.) The interval
‘`(intv 3 x x)`’ is converted to the plain number x;
intervals ‘`(intv mask x x)`’ for any other mask
represent empty intervals. If hi is less than lo, the interval
is converted to a standard empty interval by replacing hi with lo.

Vectors are stored as ‘`(vec v1 v2 …)`’, where v1
is the first element of the vector, v2 is the second, and so on.
An empty vector is stored as ‘`(vec)`’. A matrix is simply a vector
where all v’s are themselves vectors of equal lengths. Note that
Calc vectors are unrelated to the Emacs Lisp “vector” type, which is
generally unused by Calc data structures.

Variables are stored as ‘`(var name sym)`’, where
name is a Lisp symbol whose print name is used as the visible name
of the variable, and sym is a Lisp symbol in which the variable’s
value is actually stored. Thus, ‘`(var pi var-pi)`’ represents the
special constant ‘`pi`’. Almost always, the form is ‘`(var
v var-v)`’. If the variable name was entered with `#`
signs (which are converted to hyphens internally), the form is
‘`(var u v)`’, where u is a symbol whose name
contains `#` characters, and v is a symbol that contains
`-` characters instead. The value of a variable is the Calc
object stored in its sym symbol’s value cell. If the symbol’s
value cell is void or if it contains `nil`, the variable has no
value. Special constants have the form ‘`(special-const
value)`’ stored in their value cell, where value is a formula
which is evaluated when the constant’s value is requested. Variables
which represent units are not stored in any special way; they are units
only because their names appear in the units table. If the value
cell contains a string, it is parsed to get the variable’s value when
the variable is used.

A Lisp list with any other symbol as the first element is a function call.
The symbols `+`, `-`, `*`, `/`, `%`, `^`,
and `|` represent special binary operators; these lists are always
of the form ‘`(op lhs rhs)`’ where lhs is the
sub-formula on the lefthand side and rhs is the sub-formula on the
right. The symbol `neg` represents unary negation; this list is always
of the form ‘`(neg arg)`’. Any other symbol func represents a
function that would be displayed in function-call notation; the symbol
func is in general always of the form ‘`calcFunc-name`’.
The function cell of the symbol func should contain a Lisp function
for evaluating a call to func. This function is passed the remaining
elements of the list (themselves already evaluated) as arguments; such
functions should return `nil` or call `reject-arg` to signify
that they should be left in symbolic form, or they should return a Calc
object which represents their value, or a list of such objects if they
wish to return multiple values. (The latter case is allowed only for
functions which are the outer-level call in an expression whose value is
about to be pushed on the stack; this feature is considered obsolete
and is not used by any built-in Calc functions.)

---

Next: [Stack-Oriented Functions](#Stack-Lisp-Functions), Previous: [Data Type Formats](#Data-Type-Formats), Up: [Calculator Internals](#Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7.2 Interactive Functions [¶](#Interactive-Functions)

The functions described here are used in implementing interactive Calc
commands. Note that this list is not exhaustive! If there is an
existing command that behaves similarly to the one you want to define,
you may find helpful tricks by checking the source code for that command.

Function: **calc-set-command-flag** flag [¶](#index-calc_002dset_002dcommand_002dflag-1)
:   Set the command flag flag. This is generally a Lisp symbol, but
    may in fact be anything. The effect is to add flag to the list
    stored in the variable `calc-command-flags`, unless it is already
    there. See [Defining New Simple Commands](#Defining-Simple-Commands).

Function: **calc-clear-command-flag** flag [¶](#index-calc_002dclear_002dcommand_002dflag)
:   If flag appears among the list of currently-set command flags,
    remove it from that list.

Function: **calc-record-undo** rec [¶](#index-calc_002drecord_002dundo)
:   Add the “undo record” rec to the list of steps to take if the
    current operation should need to be undone. Stack push and pop functions
    automatically call `calc-record-undo`, so the kinds of undo records
    you might need to create take the form ‘`(set sym value)`’,
    which says that the Lisp variable sym was changed and had previously
    contained value; ‘`(store var value)`’ which says that
    the Calc variable var (a string which is the name of the symbol that
    contains the variable’s value) was stored and its previous value was
    value (either a Calc data object, or `nil` if the variable was
    previously void); or ‘`(eval undo redo args …)`’,
    which means that to undo requires calling the function ‘`(undo
    args …)`’ and, if the undo is later redone, calling
    ‘`(redo args …)`’.

Function: **calc-record-why** msg args [¶](#index-calc_002drecord_002dwhy)
:   Record the error or warning message msg, which is normally a string.
    This message will be replayed if the user types `w` (`calc-why`);
    if the message string begins with a ‘`*`’, it is considered important
    enough to display even if the user doesn’t type `w`. If one or more
    args are present, the displayed message will be of the form,
    ‘`msg: arg1, arg2, …`’, where the arguments are
    formatted on the assumption that they are either strings or Calc objects of
    some sort. If msg is a symbol, it is the name of a Calc predicate
    (such as `integerp` or `numvecp`) which the arguments did not
    satisfy; it is expanded to a suitable string such as “Expected an
    integer.” The `reject-arg` function calls `calc-record-why`
    automatically; see [Predicates](#Predicates).

Function: **calc-is-inverse** [¶](#index-calc_002dis_002dinverse)
:   This predicate returns true if the current command is inverse,
    i.e., if the Inverse (`I` key) flag was set.

Function: **calc-is-hyperbolic** [¶](#index-calc_002dis_002dhyperbolic)
:   This predicate is the analogous function for the `H` key.

---

Next: [Predicates](#Predicates), Previous: [Interactive Functions](#Interactive-Lisp-Functions), Up: [Calculator Internals](#Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7.3 Stack-Oriented Functions [¶](#Stack_002dOriented-Functions)

The functions described here perform various operations on the Calc
stack and trail. They are to be used in interactive Calc commands.

Function: **calc-push-list** vals n [¶](#index-calc_002dpush_002dlist)
:   Push the Calc objects in list vals onto the stack at stack level
    n. If n is omitted it defaults to 1, so that the elements
    are pushed at the top of the stack. If n is greater than 1, the
    elements will be inserted into the stack so that the last element will
    end up at level n, the next-to-last at level n+1, etc.
    The elements of vals are assumed to be valid Calc objects, and
    are not evaluated, rounded, or renormalized in any way. If vals
    is an empty list, nothing happens.

    The stack elements are pushed without any sub-formula selections.
    You can give an optional third argument to this function, which must
    be a list the same size as vals of selections. Each selection
    must be `eq` to some sub-formula of the corresponding formula
    in vals, or `nil` if that formula should have no selection.

Function: **calc-top-list** n m [¶](#index-calc_002dtop_002dlist)
:   Return a list of the n objects starting at level m of the
    stack. If m is omitted it defaults to 1, so that the elements are
    taken from the top of the stack. If n is omitted, it also
    defaults to 1, so that the top stack element (in the form of a
    one-element list) is returned. If m is greater than 1, the
    mth stack element will be at the end of the list, the m+1st
    element will be next-to-last, etc. If n or m are out of
    range, the command is aborted with a suitable error message. If n
    is zero, the function returns an empty list. The stack elements are not
    evaluated, rounded, or renormalized.

    If any stack elements contain selections, and selections have not
    been disabled by the `j e` (`calc-enable-selections`) command,
    this function returns the selected portions rather than the entire
    stack elements. It can be given a third “selection-mode” argument
    which selects other behaviors. If it is the symbol `t`, then
    a selection in any of the requested stack elements produces an
    “invalid operation on selections” error. If it is the symbol `full`,
    the whole stack entry is always returned regardless of selections.
    If it is the symbol `sel`, the selected portion is always returned,
    or `nil` if there is no selection. (This mode ignores the `j e`
    command.) If the symbol is `entry`, the complete stack entry in
    list form is returned; the first element of this list will be the whole
    formula, and the third element will be the selection (or `nil`).

Function: **calc-pop-stack** n m [¶](#index-calc_002dpop_002dstack)
:   Remove the specified elements from the stack. The parameters n
    and m are defined the same as for `calc-top-list`. The return
    value of `calc-pop-stack` is uninteresting.

    If there are any selected sub-formulas among the popped elements, and
    `j e` has not been used to disable selections, this produces an
    error without changing the stack. If you supply an optional third
    argument of `t`, the stack elements are popped even if they
    contain selections.

Function: **calc-record-list** vals tag [¶](#index-calc_002drecord_002dlist)
:   This function records one or more results in the trail. The vals
    are a list of strings or Calc objects. The tag is the four-character
    tag string to identify the values. If tag is omitted, a blank tag
    will be used.

Function: **calc-normalize** n [¶](#index-calc_002dnormalize)
:   This function takes a Calc object and “normalizes” it. At the very
    least this involves re-rounding floating-point values according to the
    current precision and other similar jobs. Also, unless the user has
    selected No-Simplify mode (see [Simplification Modes](#Simplification-Modes)), this involves
    actually evaluating a formula object by executing the function calls
    it contains, and possibly also doing algebraic simplification, etc.

Function: **calc-top-list-n** n m [¶](#index-calc_002dtop_002dlist_002dn-1)
:   This function is identical to `calc-top-list`, except that it calls
    `calc-normalize` on the values that it takes from the stack. They
    are also passed through `check-complete`, so that incomplete
    objects will be rejected with an error message. All computational
    commands should use this in preference to `calc-top-list`; the only
    standard Calc commands that operate on the stack without normalizing
    are stack management commands like `calc-enter` and `calc-roll-up`.
    This function accepts the same optional selection-mode argument as
    `calc-top-list`.

Function: **calc-top-n** m [¶](#index-calc_002dtop_002dn)
:   This function is a convenient form of `calc-top-list-n` in which only
    a single element of the stack is taken and returned, rather than a list
    of elements. This also accepts an optional selection-mode argument.

Function: **calc-enter-result** n tag vals [¶](#index-calc_002denter_002dresult-1)
:   This function is a convenient interface to most of the above functions.
    The vals argument should be either a single Calc object, or a list
    of Calc objects; the object or objects are normalized, and the top n
    stack entries are replaced by the normalized objects. If tag is
    non-`nil`, the normalized objects are also recorded in the trail.
    A typical stack-based computational command would take the form,

    ```
    (calc-enter-result n tag (cons 'calcFunc-func
                                   (calc-top-list-n n)))
    ```

    If any of the n stack elements replaced contain sub-formula
    selections, and selections have not been disabled by `j e`,
    this function takes one of two courses of action. If n is
    equal to the number of elements in vals, then each element of
    vals is spliced into the corresponding selection; this is what
    happens when you use the `TAB` key, or when you use a unary
    arithmetic operation like `sqrt`. If vals has only one
    element but n is greater than one, there must be only one
    selection among the top n stack elements; the element from
    vals is spliced into that selection. This is what happens when
    you use a binary arithmetic operation like `+`. Any other
    combination of n and vals is an error when selections
    are present.

Function: **calc-unary-op** tag func arg [¶](#index-calc_002dunary_002dop)
:   This function implements a unary operator that allows a numeric prefix
    argument to apply the operator over many stack entries. If the prefix
    argument arg is `nil`, this uses `calc-enter-result`
    as outlined above. Otherwise, it maps the function over several stack
    elements; see [Numeric Prefix Arguments](#Prefix-Arguments). For example,

    ```
    (defun calc-zeta (arg)
      (interactive "P")
      (calc-unary-op "zeta" 'calcFunc-zeta arg))
    ```

Function: **calc-binary-op** tag func arg ident unary [¶](#index-calc_002dbinary_002dop)
:   This function implements a binary operator, analogously to
    `calc-unary-op`. The optional ident and unary
    arguments specify the behavior when the prefix argument is zero or
    one, respectively. If the prefix is zero, the value ident
    is pushed onto the stack, if specified, otherwise an error message
    is displayed. If the prefix is one, the unary function unary
    is applied to the top stack element, or, if unary is not
    specified, nothing happens. When the argument is two or more,
    the binary function func is reduced across the top arg
    stack elements; when the argument is negative, the function is
    mapped between the next-to-top *-arg* stack elements and the
    top element.

Function: **calc-stack-size** [¶](#index-calc_002dstack_002dsize)
:   Return the number of elements on the stack as an integer. This count
    does not include elements that have been temporarily hidden by stack
    truncation; see [Truncating the Stack](#Truncating-the-Stack).

Function: **calc-cursor-stack-index** n [¶](#index-calc_002dcursor_002dstack_002dindex)
:   Move the point to the nth stack entry. If n is zero, this
    will be the ‘`.`’ line. If n is from 1 to the current stack size,
    this will be the beginning of the first line of that stack entry’s display.
    If line numbers are enabled, this will move to the first character of the
    line number, not the stack entry itself.

Function: **calc-substack-height** n [¶](#index-calc_002dsubstack_002dheight)
:   Return the number of lines between the beginning of the nth stack
    entry and the bottom of the buffer. If n is zero, this
    will be one (assuming no stack truncation). If all stack entries are
    one line long (i.e., no matrices are displayed), the return value will
    be equal n+1 as long as n is in range. (Note that in Big
    mode, the return value includes the blank lines that separate stack
    entries.)

Function: **calc-refresh** [¶](#index-calc_002drefresh)
:   Erase the `*Calculator*` buffer and reformat its contents from memory.
    This must be called after changing any parameter, such as the current
    display radix, which might change the appearance of existing stack
    entries. (During a keyboard macro invoked by the `X` key, refreshing
    is suppressed, but a flag is set so that the entire stack will be refreshed
    rather than just the top few elements when the macro finishes.)

---

Next: [Computational Functions](#Computational-Lisp-Functions), Previous: [Stack-Oriented Functions](#Stack-Lisp-Functions), Up: [Calculator Internals](#Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7.4 Predicates [¶](#Predicates-2)

The functions described here are predicates, that is, they return a
true/false value where `nil` means false and anything else means
true. These predicates are expanded by `defmath`, for example,
from `zerop` to `math-zerop`. In many cases they correspond
to native Lisp functions by the same name, but are extended to cover
the full range of Calc data types.

Function: **zerop** x [¶](#index-zerop)
:   Returns true if x is numerically zero, in any of the Calc data
    types. (Note that for some types, such as error forms and intervals,
    it never makes sense to return true.) In `defmath`, the expression
    ‘`(= x 0)`’ will automatically be converted to ‘`(math-zerop x)`’,
    and ‘`(/= x 0)`’ will be converted to ‘`(not (math-zerop x))`’.

Function: **negp** x [¶](#index-negp)
:   Returns true if x is negative. This accepts negative real numbers
    of various types, negative HMS and date forms, and intervals in which
    all included values are negative. In `defmath`, the expression
    ‘`(< x 0)`’ will automatically be converted to ‘`(math-negp x)`’,
    and ‘`(>= x 0)`’ will be converted to ‘`(not (math-negp x))`’.

Function: **posp** x [¶](#index-posp)
:   Returns true if x is positive (and non-zero). For complex
    numbers, none of these three predicates will return true.

Function: **looks-negp** x [¶](#index-looks_002dnegp)
:   Returns true if x is “negative-looking.” This returns true if
    x is a negative number, or a formula with a leading minus sign
    such as ‘`-a/b`’. In other words, this is an object which can be
    made simpler by calling `(- x)`.

Function: **integerp** x [¶](#index-integerp)
:   Returns true if x is an integer of any size.

Function: **fixnump** x [¶](#index-fixnump)
:   Returns true if x is a native Lisp fixnum.

Function: **natnump** x [¶](#index-natnump)
:   Returns true if x is a nonnegative integer of any size.

Function: **fixnatnump** x [¶](#index-fixnatnump)
:   Returns true if x is a nonnegative Lisp fixnum.

Function: **num-integerp** x [¶](#index-num_002dintegerp)
:   Returns true if x is numerically an integer, i.e., either a
    true integer or a float with no significant digits to the right of
    the decimal point.

Function: **messy-integerp** x [¶](#index-messy_002dintegerp)
:   Returns true if x is numerically, but not literally, an integer.
    A value is `num-integerp` if it is `integerp` or
    `messy-integerp` (but it is never both at once).

Function: **num-natnump** x [¶](#index-num_002dnatnump)
:   Returns true if x is numerically a nonnegative integer.

Function: **evenp** x [¶](#index-evenp)
:   Returns true if x is an even integer.

Function: **looks-evenp** x [¶](#index-looks_002devenp)
:   Returns true if x is an even integer, or a formula with a leading
    multiplicative coefficient which is an even integer.

Function: **oddp** x [¶](#index-oddp)
:   Returns true if x is an odd integer.

Function: **ratp** x [¶](#index-ratp)
:   Returns true if x is a rational number, i.e., an integer or a
    fraction.

Function: **realp** x [¶](#index-realp)
:   Returns true if x is a real number, i.e., an integer, fraction,
    or floating-point number.

Function: **anglep** x [¶](#index-anglep)
:   Returns true if x is a real number or HMS form.

Function: **floatp** x [¶](#index-floatp)
:   Returns true if x is a float, or a complex number, error form,
    interval, date form, or modulo form in which at least one component
    is a float.

Function: **complexp** x [¶](#index-complexp)
:   Returns true if x is a rectangular or polar complex number
    (but not a real number).

Function: **rect-complexp** x [¶](#index-rect_002dcomplexp)
:   Returns true if x is a rectangular complex number.

Function: **polar-complexp** x [¶](#index-polar_002dcomplexp)
:   Returns true if x is a polar complex number.

Function: **numberp** x [¶](#index-numberp)
:   Returns true if x is a real number or a complex number.

Function: **scalarp** x [¶](#index-scalarp)
:   Returns true if x is a real or complex number or an HMS form.

Function: **vectorp** x [¶](#index-vectorp)
:   Returns true if x is a vector (this simply checks if its argument
    is a list whose first element is the symbol `vec`).

Function: **numvecp** x [¶](#index-numvecp)
:   Returns true if x is a number or vector.

Function: **matrixp** x [¶](#index-matrixp)
:   Returns true if x is a matrix, i.e., a vector of one or more vectors,
    all of the same size.

Function: **square-matrixp** x [¶](#index-square_002dmatrixp)
:   Returns true if x is a square matrix.

Function: **objectp** x [¶](#index-objectp)
:   Returns true if x is any numeric Calc object, including real and
    complex numbers, HMS forms, date forms, error forms, intervals, and
    modulo forms. (Note that error forms and intervals may include formulas
    as their components; see `constp` below.)

Function: **objvecp** x [¶](#index-objvecp)
:   Returns true if x is an object or a vector. This also accepts
    incomplete objects, but it rejects variables and formulas (except as
    mentioned above for `objectp`).

Function: **primp** x [¶](#index-primp)
:   Returns true if x is a “primitive” or “atomic” Calc object,
    i.e., one whose components cannot be regarded as sub-formulas. This
    includes variables, and all `objectp` types except error forms
    and intervals.

Function: **constp** x [¶](#index-constp)
:   Returns true if x is constant, i.e., a real or complex number,
    HMS form, date form, or error form, interval, or vector all of whose
    components are `constp`.

Function: **lessp** x y [¶](#index-lessp)
:   Returns true if x is numerically less than y. Returns false
    if x is greater than or equal to y, or if the order is
    undefined or cannot be determined. Generally speaking, this works
    by checking whether ‘`x - y`’ is `negp`. In
    `defmath`, the expression ‘`(< x y)`’ will automatically be
    converted to ‘`(lessp x y)`’; expressions involving `>`, `<=`,
    and `>=` are similarly converted in terms of `lessp`.

Function: **beforep** x y [¶](#index-beforep)
:   Returns true if x comes before y in a canonical ordering
    of Calc objects. If x and y are both real numbers, this
    will be the same as `lessp`. But whereas `lessp` considers
    other types of objects to be unordered, `beforep` puts any two
    objects into a definite, consistent order. The `beforep`
    function is used by the `V S` vector-sorting command, and also
    by Calc’s algebraic simplifications to put the terms of a product into
    canonical order: This allows ‘`x y + y x`’ to be simplified easily to
    ‘`2 x y`’.

Function: **equal** x y [¶](#index-equal)
:   This is the standard Lisp `equal` predicate; it returns true if
    x and y are structurally identical. This is the usual way
    to compare numbers for equality, but note that `equal` will treat
    0 and 0.0 as different.

Function: **math-equal** x y [¶](#index-math_002dequal)
:   Returns true if x and y are numerically equal, either because
    they are `equal`, or because their difference is `zerop`. In
    `defmath`, the expression ‘`(= x y)`’ will automatically be
    converted to ‘`(math-equal x y)`’.

Function: **equal-int** x n [¶](#index-equal_002dint)
:   Returns true if x and n are numerically equal, where n
    is an integer which is not a multiple of 10. This will automatically be
    used by `defmath` in place of the more general `math-equal`
    whenever possible.

Function: **nearly-equal** x y [¶](#index-nearly_002dequal)
:   Returns true if x and y, as floating-point numbers, are
    equal except possibly in the last decimal place. For example,
    314.159 and 314.166 are considered nearly equal if the current
    precision is 6 (since they differ by 7 units), but not if the current
    precision is 7 (since they differ by 70 units). Most functions which
    use series expansions use `with-extra-prec` to evaluate the
    series with 2 extra digits of precision, then use `nearly-equal`
    to decide when the series has converged; this guards against cumulative
    error in the series evaluation without doing extra work which would be
    lost when the result is rounded back down to the current precision.
    In `defmath`, this can be written ‘`(~= x y)`’.
    The x and y can be numbers of any kind, including complex.

Function: **nearly-zerop** x y [¶](#index-nearly_002dzerop)
:   Returns true if x is nearly zero, compared to y. This
    checks whether x plus y would by be `nearly-equal`
    to y itself, to within the current precision, in other words,
    if adding x to y would have a negligible effect on y
    due to roundoff error. X may be a real or complex number, but
    y must be real.

Function: **is-true** x [¶](#index-is_002dtrue)
:   Return true if the formula x represents a true value in
    Calc, not Lisp, terms. It tests if x is a non-zero number
    or a provably non-zero formula.

Function: **reject-arg** val pred [¶](#index-reject_002darg)
:   Abort the current function evaluation due to unacceptable argument values.
    This calls ‘`(calc-record-why pred val)`’, then signals a
    Lisp error which `normalize` will trap. The net effect is that the
    function call which led here will be left in symbolic form.

Function: **inexact-value** [¶](#index-inexact_002dvalue)
:   If Symbolic mode is enabled, this will signal an error that causes
    `normalize` to leave the formula in symbolic form, with the message
    “Inexact result.” (This function has no effect when not in Symbolic mode.)
    Note that if your function calls ‘`(sin 5)`’ in Symbolic mode, the
    `sin` function will call `inexact-value`, which will cause your
    function to be left unsimplified. You may instead wish to call
    ‘`(normalize (list 'calcFunc-sin 5))`’, which in Symbolic mode will
    return the formula ‘`sin(5)`’ to your function.

Function: **overflow** [¶](#index-overflow)
:   This signals an error that will be reported as a floating-point overflow.

Function: **underflow** [¶](#index-underflow)
:   This signals a floating-point underflow.

---

Next: [Vector Functions](#Vector-Lisp-Functions), Previous: [Predicates](#Predicates), Up: [Calculator Internals](#Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7.5 Computational Functions [¶](#Computational-Functions)

The functions described here do the actual computational work of the
Calculator. In addition to these, note that any function described in
the main body of this manual may be called from Lisp; for example, if
the documentation refers to the `calc-sqrt` [`sqrt`] command,
this means `calc-sqrt` is an interactive stack-based square-root
command and `sqrt` (which `defmath` expands to `calcFunc-sqrt`)
is the actual Lisp function for taking square roots.

The functions `math-add`, `math-sub`, `math-mul`,
`math-div`, `math-mod`, and `math-neg` are not included
in this list, since `defmath` allows you to write native Lisp
`+`, `-`, `*`, `/`, `%`, and unary `-`,
respectively, instead.

Function: **normalize** val [¶](#index-normalize)
:   (Full form: `math-normalize`.)
    Reduce the value val to standard form.
    Variables are left alone, but function calls are actually evaluated
    in formulas. For example, normalizing ‘`(+ 2 (calcFunc-abs -4))`’ will
    return 6.

    If a function call fails, because the function is void or has the wrong
    number of parameters, or because it returns `nil` or calls
    `reject-arg` or `inexact-result`, `normalize` returns
    the formula still in symbolic form.

    If the current simplification mode is “none” or “numeric arguments
    only,” `normalize` will act appropriately. However, the more
    powerful simplification modes (like Algebraic Simplification) are
    not handled by `normalize`. They are handled by `calc-normalize`,
    which calls `normalize` and possibly some other routines, such
    as `simplify` or `simplify-units`. Programs generally will
    never call `calc-normalize` except when popping or pushing values
    on the stack.

Function: **evaluate-expr** expr [¶](#index-evaluate_002dexpr)
:   Replace all variables in expr that have values with their values,
    then use `normalize` to simplify the result. This is what happens
    when you press the `=` key interactively.

Macro: **with-extra-prec** n body [¶](#index-with_002dextra_002dprec)
:   Evaluate the Lisp forms in body with precision increased by n
    digits. This is a macro which expands to

    ```
    (math-normalize
      (let ((calc-internal-prec (+ calc-internal-prec n)))
        body))
    ```

    The surrounding call to `math-normalize` causes a floating-point
    result to be rounded down to the original precision afterwards. This
    is important because some arithmetic operations assume a number’s
    mantissa contains no more digits than the current precision allows.

Function: **make-frac** n d [¶](#index-make_002dfrac)
:   Build a fraction ‘`n:d`’. This is equivalent to calling
    ‘`(normalize (list 'frac n d))`’, but more efficient.

Function: **make-float** mant exp [¶](#index-make_002dfloat)
:   Build a floating-point value out of mant and exp, both
    of which are arbitrary integers. This function will return a
    properly normalized float value, or signal an overflow or underflow
    if exp is out of range.

Function: **make-sdev** x sigma [¶](#index-make_002dsdev)
:   Build an error form out of x and the absolute value of sigma.
    If sigma is zero, the result is the number x directly.
    If sigma is negative or complex, its absolute value is used.
    If x or sigma is not a valid type of object for use in
    error forms, this calls `reject-arg`.

Function: **make-intv** mask lo hi [¶](#index-make_002dintv)
:   Build an interval form out of mask (which is assumed to be an
    integer from 0 to 3), and the limits lo and hi. If
    lo is greater than hi, an empty interval form is returned.
    This calls `reject-arg` if lo or hi is unsuitable.

Function: **sort-intv** mask lo hi [¶](#index-sort_002dintv)
:   Build an interval form, similar to `make-intv`, except that if
    lo is less than hi they are simply exchanged, and the
    bits of mask are swapped accordingly.

Function: **make-mod** n m [¶](#index-make_002dmod)
:   Build a modulo form out of n and the modulus m. Since modulo
    forms do not allow formulas as their components, if n or m
    is not a real number or HMS form the result will be a formula which
    is a call to `makemod`, the algebraic version of this function.

Function: **float** x [¶](#index-float-1)
:   Convert x to floating-point form. Integers and fractions are
    converted to numerically equivalent floats; components of complex
    numbers, vectors, HMS forms, date forms, error forms, intervals, and
    modulo forms are recursively floated. If the argument is a variable
    or formula, this calls `reject-arg`.

Function: **compare** x y [¶](#index-compare)
:   Compare the numbers x and y, and return *-1* if
    ‘`(lessp x y)`’, 1 if ‘`(lessp y x)`’,
    0 if ‘`(math-equal x y)`’, or 2 if the order is
    undefined or cannot be determined.

Function: **numdigs** n [¶](#index-numdigs)
:   Return the number of digits of integer n, effectively
    ‘`ceil(log10(n))`’, but much more efficient. Zero is
    considered to have zero digits.

Function: **scale-int** x n [¶](#index-scale_002dint)
:   Shift integer x left n decimal digits, or right *-n*
    digits with truncation toward zero.

Function: **scale-rounding** x n [¶](#index-scale_002drounding)
:   Like `scale-int`, except that a right shift rounds to the nearest
    integer rather than truncating.

Function: **fixnum** n [¶](#index-fixnum-1)
:   Return the integer n as a fixnum, i.e., a small Lisp integer.
    If n is outside the permissible range for Lisp fixnums (usually
    62 binary bits) the result is undefined.

Function: **sqr** x [¶](#index-sqr)
:   Compute the square of x; short for ‘`(* x x)`’.
