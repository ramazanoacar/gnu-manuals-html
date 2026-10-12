### 11.3 Recursion

A recursive function contains code that tells the Lisp interpreter to
call a program that runs exactly like itself, but with slightly
different arguments. The code runs exactly the same because it has
the same name. However, even though the program has the same name, it
is not the same entity. It is different. In the jargon, it is a
different “instance”.

Eventually, if the program is written correctly, the slightly
different arguments will become sufficiently different from the first
arguments that the final instance will stop.

* [Building Robots: Extending the Metaphor](#Building-Robots)
* [The Parts of a Recursive Definition](#Recursive-Definition-Parts)
* [Recursion with a List](#Recursion-with-list)
* [Recursion in Place of a Counter](#Recursive-triangle-function)
* [Recursion Example Using `cond`](#Recursion-with-cond)
* [Recursive Patterns](#Recursive-Patterns)
* [Recursion without Deferments](#No-Deferment)
* [No Deferment Solution](#No-deferment-solution)

---

Next: [The Parts of a Recursive Definition](#Recursive-Definition-Parts), Up: [Recursion](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.1 Building Robots: Extending the Metaphor

It is sometimes helpful to think of a running program as a robot that
does a job. In doing its job, a recursive function calls on a second
robot to help it. The second robot is identical to the first in every
way, except that the second robot helps the first and has been
passed different arguments than the first.

In a recursive function, the second robot may call a third; and the
third may call a fourth, and so on. Each of these is a different
entity; but all are clones.

Since each robot has slightly different instructions—the arguments
will differ from one robot to the next—the last robot should know
when to stop.

Let’s expand on the metaphor in which a computer program is a robot.

A function definition provides the blueprints for a robot. When you
install a function definition, that is, when you evaluate a
`defun` macro, you install the necessary equipment to build
robots. It is as if you were in a factory, setting up an assembly
line. Robots with the same name are built according to the same
blueprints. So they have the same model number, but a
different serial number.

We often say that a recursive function “calls itself”. What we mean
is that the instructions in a recursive function cause the Lisp
interpreter to run a different function that has the same name and
does the same job as the first, but with different arguments.

It is important that the arguments differ from one instance to the
next; otherwise, the process will never stop.

---

Next: [Recursion with a List](#Recursion-with-list), Previous: [Building Robots: Extending the Metaphor](#Building-Robots), Up: [Recursion](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.2 The Parts of a Recursive Definition

A recursive function typically contains a conditional expression which
has three parts:

1. A true-or-false-test that determines whether the function is called
   again, here called the *do-again-test*.
2. The name of the function. When this name is called, a new instance of
   the function—a new robot, as it were—is created and told what to do.
3. An expression that returns a different value each time the function is
   called, here called the *next-step-expression*. Consequently, the
   argument (or arguments) passed to the new instance of the function
   will be different from that passed to the previous instance. This
   causes the conditional expression, the *do-again-test*, to test
   false after the correct number of repetitions.

Recursive functions can be much simpler than any other kind of
function. Indeed, when people first start to use them, they often look
so mysteriously simple as to be incomprehensible. Like riding a
bicycle, reading a recursive function definition takes a certain knack
which is hard at first but then seems simple.

There are several different common recursive patterns. A very simple
pattern looks like this:

```
(defun name-of-recursive-function (argument-list)
  "documentation…"
  (if do-again-test
    body…
    (name-of-recursive-function
         next-step-expression)))
```

Each time a recursive function is evaluated, a new instance of it is
created and told what to do. The arguments tell the instance what to do.

An argument is bound to the value of the next-step-expression. Each
instance runs with a different value of the next-step-expression.

The value in the next-step-expression is used in the do-again-test.

The value returned by the next-step-expression is passed to the new
instance of the function, which evaluates it (or some
transmogrification of it) to determine whether to continue or stop.
The next-step-expression is designed so that the do-again-test returns
false when the function should no longer be repeated.

The do-again-test is sometimes called the *stop condition*,
since it stops the repetitions when it tests false.

---

Next: [Recursion in Place of a Counter](#Recursive-triangle-function), Previous: [The Parts of a Recursive Definition](#Recursive-Definition-Parts), Up: [Recursion](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.3 Recursion with a List

The example of a `while` loop that printed the elements of a list
of numbers can be written recursively. Here is the code, including
an expression to set the value of the variable `animals` to a list.

If you are reading this in Info in Emacs, you can evaluate this
expression directly in Info. Otherwise, you must copy the example
to the `*scratch*` buffer and evaluate each expression there.
Use `C-u C-x C-e` to evaluate the
`(print-elements-recursively animals)` expression so that the
results are printed in the buffer; otherwise the Lisp interpreter will
try to squeeze the results into the one line of the echo area.

Also, place your cursor immediately after the last closing parenthesis
of the `print-elements-recursively` function, before the comment.
Otherwise, the Lisp interpreter will try to evaluate the comment.

```
(setq animals '(gazelle giraffe lion tiger))

(defun print-elements-recursively (list)
  "Print each element of LIST on a line of its own.
Uses recursion."
  (when list                            ; do-again-test
        (print (car list))              ; body
        (print-elements-recursively     ; recursive call
         (cdr list))))                  ; next-step-expression

(print-elements-recursively animals)
```

The `print-elements-recursively` function first tests whether
there is any content in the list; if there is, the function prints the
first element of the list, the CAR of the list. Then the
function invokes itself, but gives itself as its argument, not the
whole list, but the second and subsequent elements of the list, the
CDR of the list.

Put another way, if the list is not empty, the function invokes
another instance of code that is similar to the initial code, but is a
different thread of execution, with different arguments than the first
instance.

Put in yet another way, if the list is not empty, the first robot
assembles a second robot and tells it what to do; the second robot is
a different individual from the first, but is the same model.

When the second evaluation occurs, the `when` expression is
evaluated and if true, prints the first element of the list it
receives as its argument (which is the second element of the original
list). Then the function calls itself with the CDR of the list
it is invoked with, which (the second time around) is the CDR of
the CDR of the original list.

Note that although we say that the function “calls itself”, what we
mean is that the Lisp interpreter assembles and instructs a new
instance of the program. The new instance is a clone of the first,
but is a separate individual.

Each time the function invokes itself, it does so on a
shorter version of the original list. It creates a new instance that
works on a shorter list.

Eventually, the function invokes itself on an empty list. It creates
a new instance whose argument is `nil`. The conditional expression
tests the value of `list`. Since the value of `list` is
`nil`, the `when` expression tests false so the then-part is
not evaluated. The function as a whole then returns `nil`.

When you evaluate the expression `(print-elements-recursively
animals)` in the `*scratch*` buffer, you see this result:

```
gazelle

giraffe

lion

tiger
nil
```

---

Next: [Recursion Example Using `cond`](#Recursion-with-cond), Previous: [Recursion with a List](#Recursion-with-list), Up: [Recursion](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.4 Recursion in Place of a Counter

The `triangle` function described in a previous section can also
be written recursively. It looks like this:

```
(defun triangle-recursively (number)
  "Return the sum of the numbers 1 through NUMBER inclusive.
Uses recursion."
  (if (= number 1)                    ; do-again-test
      1                               ; then-part
    (+ number                         ; else-part
       (triangle-recursively          ; recursive call
        (1- number)))))               ; next-step-expression

(triangle-recursively 7)
```

You can install this function by evaluating it and then try it by
evaluating `(triangle-recursively 7)`. (Remember to put your
cursor immediately after the last parenthesis of the function
definition, before the comment.) The function evaluates to 28.

To understand how this function works, let’s consider what happens in the
various cases when the function is passed 1, 2, 3, or 4 as the value of
its argument.

* [An argument of 1 or 2](#Recursive-Example-arg-of-1-or-2)
* [An argument of 3 or 4](#Recursive-Example-arg-of-3-or-4)

---

Next: [An argument of 3 or 4](#Recursive-Example-arg-of-3-or-4), Up: [Recursion in Place of a Counter](#Recursive-triangle-function)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### An argument of 1 or 2

First, what happens if the value of the argument is 1?

The function has an `if` expression after the documentation
string. It tests whether the value of `number` is equal to 1; if
so, Emacs evaluates the then-part of the `if` expression, which
returns the number 1 as the value of the function. (A triangle with
one row has one pebble in it.)

Suppose, however, that the value of the argument is 2. In this case,
Emacs evaluates the else-part of the `if` expression.

The else-part consists of an addition, the recursive call to
`triangle-recursively` and a decrementing action; and it looks like
this:

```
(+ number (triangle-recursively (1- number)))
```

When Emacs evaluates this expression, the innermost expression is
evaluated first; then the other parts in sequence. Here are the steps
in detail:

*Step 1    Evaluate the innermost expression.*
:   The innermost expression is `(1- number)` so Emacs decrements the
    value of `number` from 2 to 1.

*Step 2    Evaluate the `triangle-recursively` function.*
:   The Lisp interpreter creates an individual instance of
    `triangle-recursively`. It does not matter that this function is
    contained within itself. Emacs passes the result Step 1 as the
    argument used by this instance of the `triangle-recursively`
    function

    In this case, Emacs evaluates `triangle-recursively` with an
    argument of 1. This means that this evaluation of
    `triangle-recursively` returns 1.

*Step 3    Evaluate the value of `number`.*
:   The variable `number` is the second element of the list that
    starts with `+`; its value is 2.

*Step 4    Evaluate the `+` expression.*
:   The `+` expression receives two arguments, the first
    from the evaluation of `number` (Step 3) and the second from the
    evaluation of `triangle-recursively` (Step 2).

    The result of the addition is the sum of 2 plus 1, and the number 3 is
    returned, which is correct. A triangle with two rows has three
    pebbles in it.

---

Previous: [An argument of 1 or 2](#Recursive-Example-arg-of-1-or-2), Up: [Recursion in Place of a Counter](#Recursive-triangle-function)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### An argument of 3 or 4

Suppose that `triangle-recursively` is called with an argument of
3.

*Step 1    Evaluate the do-again-test.*
:   The `if` expression is evaluated first. This is the do-again
    test and returns false, so the else-part of the `if` expression
    is evaluated. (Note that in this example, the do-again-test causes
    the function to call itself when it tests false, not when it tests
    true.)

*Step 2    Evaluate the innermost expression of the else-part.*
:   The innermost expression of the else-part is evaluated, which decrements
    3 to 2. This is the next-step-expression.

*Step 3    Evaluate the `triangle-recursively` function.*
:   The number 2 is passed to the `triangle-recursively` function.

    We already know what happens when Emacs evaluates `triangle-recursively` with
    an argument of 2. After going through the sequence of actions described
    earlier, it returns a value of 3. So that is what will happen here.

*Step 4    Evaluate the addition.*
:   3 will be passed as an argument to the addition and will be added to the
    number with which the function was called, which is 3.

The value returned by the function as a whole will be 6.

Now that we know what will happen when `triangle-recursively` is
called with an argument of 3, it is evident what will happen if it is
called with an argument of 4:

> In the recursive call, the evaluation of
>
> ```
> (triangle-recursively (1- 4))
> ```
>
> will return the value of evaluating
>
> ```
> (triangle-recursively 3)
> ```
>
> which is 6 and this value will be added to 4 by the addition in the
> third line.

The value returned by the function as a whole will be 10.

Each time `triangle-recursively` is evaluated, it evaluates a
version of itself—a different instance of itself—with a smaller
argument, until the argument is small enough so that it does not
evaluate itself.

Note that this particular design for a recursive function
requires that operations be deferred.

Before `(triangle-recursively 7)` can calculate its answer, it
must call `(triangle-recursively 6)`; and before
`(triangle-recursively 6)` can calculate its answer, it must call
`(triangle-recursively 5)`; and so on. That is to say, the
calculation that `(triangle-recursively 7)` makes must be
deferred until `(triangle-recursively 6)` makes its calculation;
and `(triangle-recursively 6)` must defer until
`(triangle-recursively 5)` completes; and so on.

If each of these instances of `triangle-recursively` are thought
of as different robots, the first robot must wait for the second to
complete its job, which must wait until the third completes, and so
on.

There is a way around this kind of waiting, which we will discuss in
[Recursion without Deferments](#No-Deferment).

---

Next: [Recursive Patterns](#Recursive-Patterns), Previous: [Recursion in Place of a Counter](#Recursive-triangle-function), Up: [Recursion](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.5 Recursion Example Using `cond`

The version of `triangle-recursively` described earlier is written
with the `if` special form. It can also be written using another
special form called `cond`. The name of the special form
`cond` is an abbreviation of the word ‘`conditional`’.

Although the `cond` special form is not used as often in the
Emacs Lisp sources as `if`, it is used often enough to justify
explaining it.

The template for a `cond` expression looks like this:

```
(cond
 body…)
```

where the body is a series of lists.

Written out more fully, the template looks like this:

```
(cond
 (first-true-or-false-test first-consequent)
 (second-true-or-false-test second-consequent)
 (third-true-or-false-test third-consequent)
  …)
```

When the Lisp interpreter evaluates the `cond` expression, it
evaluates the first element (the CAR or true-or-false-test) of
the first expression in a series of expressions within the body of the
`cond`.

If the true-or-false-test returns `nil` the rest of that
expression, the consequent, is skipped and the true-or-false-test of the
next expression is evaluated. When an expression is found whose
true-or-false-test returns a value that is not `nil`, the
consequent of that expression is evaluated. The consequent can be one
or more expressions. If the consequent consists of more than one
expression, the expressions are evaluated in sequence and the value of
the last one is returned. If the expression does not have a consequent,
the value of the true-or-false-test is returned.

If none of the true-or-false-tests test true, the `cond` expression
returns `nil`.

Written using `cond`, the `triangle` function looks like this:

```
(defun triangle-using-cond (number)
  (cond ((<= number 0) 0)
        ((= number 1) 1)
        ((> number 1)
         (+ number (triangle-using-cond (1- number))))))
```

In this example, the `cond` returns 0 if the number is less than or
equal to 0, it returns 1 if the number is 1 and it evaluates `(+
number (triangle-using-cond (1- number)))` if the number is greater than
1.

---

Next: [Recursion without Deferments](#No-Deferment), Previous: [Recursion Example Using `cond`](#Recursion-with-cond), Up: [Recursion](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.6 Recursive Patterns

Here are three common recursive patterns. Each involves a list.
Recursion does not need to involve lists, but Lisp is designed for lists
and this provides a sense of its primal capabilities.

* [Recursive Pattern: *every*](#Every)
* [Recursive Pattern: *accumulate*](#Accumulate)
* [Recursive Pattern: *keep*](#Keep)

---

Next: [Recursive Pattern: *accumulate*](#Accumulate), Up: [Recursive Patterns](#Recursive-Patterns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Recursive Pattern: *every*

In the `every` recursive pattern, an action is performed on every
element of a list.

The basic pattern is:

* If a list be empty, return `nil`.
* Else, act on the beginning of the list (the CAR of the list)
  + through a recursive call by the function on the rest (the
    CDR) of the list,
  + and, optionally, combine the acted-on element, using `cons`,
    with the results of acting on the rest.

Here is an example:

```
(defun square-each (numbers-list)
  "Square each of a NUMBERS LIST, recursively."
  (if (not numbers-list)                ; do-again-test
      nil
    (cons
     (* (car numbers-list) (car numbers-list))
     (square-each (cdr numbers-list))))) ; next-step-expression
```

```

```

```
(square-each '(1 2 3))
    ⇒ (1 4 9)
```

If `numbers-list` is empty, do nothing. But if it has content,
construct a list combining the square of the first number in the list
with the result of the recursive call.

(The example follows the pattern exactly: `nil` is returned if
the numbers’ list is empty. In practice, you would write the
conditional so it carries out the action when the numbers’ list is not
empty.)

The `print-elements-recursively` function (see [Recursion with a List](#Recursion-with-list)) is another example of an `every`
pattern, except in this case, rather than bring the results together
using `cons`, we print each element of output.

The `print-elements-recursively` function looks like this:

```
(setq animals '(gazelle giraffe lion tiger))
```

```

```

```
(defun print-elements-recursively (list)
  "Print each element of LIST on a line of its own.
Uses recursion."
  (when list                            ; do-again-test
        (print (car list))              ; body
        (print-elements-recursively     ; recursive call
         (cdr list))))                  ; next-step-expression

(print-elements-recursively animals)
```

The pattern for `print-elements-recursively` is:

* When the list is empty, do nothing.
* But when the list has at least one element,
  + act on the beginning of the list (the CAR of the list),
  + and make a recursive call on the rest (the CDR) of the list.

---

Next: [Recursive Pattern: *keep*](#Keep), Previous: [Recursive Pattern: *every*](#Every), Up: [Recursive Patterns](#Recursive-Patterns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Recursive Pattern: *accumulate*

Another recursive pattern is called the `accumulate` pattern. In
the `accumulate` recursive pattern, an action is performed on
every element of a list and the result of that action is accumulated
with the results of performing the action on the other elements.

This is very like the `every` pattern using `cons`, except that
`cons` is not used, but some other combiner.

The pattern is:

* If a list be empty, return zero or some other constant.
* Else, act on the beginning of the list (the CAR of the list),
  + and combine that acted-on element, using `+` or
    some other combining function, with
  + a recursive call by the function on the rest (the CDR) of the list.

Here is an example:

```
(defun add-elements (numbers-list)
  "Add the elements of NUMBERS-LIST together."
  (if (not numbers-list)
      0
    (+ (car numbers-list) (add-elements (cdr numbers-list)))))
```

```

```

```
(add-elements '(1 2 3 4))
    ⇒ 10
```

See [Making a List of Files](#Files-List), for an example of the
accumulate pattern.

---

Previous: [Recursive Pattern: *accumulate*](#Accumulate), Up: [Recursive Patterns](#Recursive-Patterns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Recursive Pattern: *keep*

A third recursive pattern is called the `keep` pattern.
In the `keep` recursive pattern, each element of a list is tested;
the element is acted on and the results are kept only if the element
meets a criterion.

Again, this is very like the `every` pattern, except the element is
skipped unless it meets a criterion.

The pattern has three parts:

* If a list be empty, return `nil`.
* Else, if the beginning of the list (the CAR of the list) passes
  a test
  + act on that element and combine it, using `cons` with
  + a recursive call by the function on the rest (the CDR) of the list.
* Otherwise, if the beginning of the list (the CAR of the list) fails
  the test
  + skip on that element,
  + and, recursively call the function on the rest (the CDR) of the list.

Here is an example that uses `cond`:

```
(defun keep-three-letter-words (word-list)
  "Keep three letter words in WORD-LIST."
  (cond
   ;; First do-again-test: stop-condition
   ((not word-list) nil)

   ;; Second do-again-test: when to act
   ((eq 3 (length (symbol-name (car word-list))))
    ;; combine acted-on element with recursive call on shorter list
    (cons (car word-list) (keep-three-letter-words (cdr word-list))))

   ;; Third do-again-test: when to skip element;
   ;;   recursively call shorter list with next-step expression
   (t (keep-three-letter-words (cdr word-list)))))
```

```

```

```
(keep-three-letter-words '(one two three four five six))
    ⇒ (one two six)
```

It goes without saying that you need not use `nil` as the test for
when to stop; and you can, of course, combine these patterns.

---

Next: [No Deferment Solution](#No-deferment-solution), Previous: [Recursive Patterns](#Recursive-Patterns), Up: [Recursion](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.7 Recursion without Deferments

Let’s consider again what happens with the `triangle-recursively`
function. We will find that the intermediate calculations are
deferred until all can be done.

Here is the function definition:

```
(defun triangle-recursively (number)
  "Return the sum of the numbers 1 through NUMBER inclusive.
Uses recursion."
  (if (= number 1)                    ; do-again-test
      1                               ; then-part
    (+ number                         ; else-part
       (triangle-recursively          ; recursive call
        (1- number)))))               ; next-step-expression
```

What happens when we call this function with an argument of 7?

The first instance of the `triangle-recursively` function adds
the number 7 to the value returned by a second instance of
`triangle-recursively`, an instance that has been passed an
argument of 6. That is to say, the first calculation is:

```
(+ 7 (triangle-recursively 6))
```

The first instance of `triangle-recursively`—you may want to
think of it as a little robot—cannot complete its job. It must hand
off the calculation for `(triangle-recursively 6)` to a second
instance of the program, to a second robot. This second individual is
completely different from the first one; it is, in the jargon, a
“different instantiation”. Or, put another way, it is a different
robot. It is the same model as the first; it calculates triangle
numbers recursively; but it has a different serial number.

And what does `(triangle-recursively 6)` return? It returns the
number 6 added to the value returned by evaluating
`triangle-recursively` with an argument of 5. Using the robot
metaphor, it asks yet another robot to help it.

Now the total is:

```
(+ 7 6 (triangle-recursively 5))
```

And what happens next?

```
(+ 7 6 5 (triangle-recursively 4))
```

Each time `triangle-recursively` is called, except for the last
time, it creates another instance of the program—another robot—and
asks it to make a calculation.

Eventually, the full addition is set up and performed:

```
(+ 7 6 5 4 3 2 1)
```

This design for the function defers the calculation of the first step
until the second can be done, and defers that until the third can be
done, and so on. Each deferment means the computer must remember what
is being waited on. This is not a problem when there are only a few
steps, as in this example. But it can be a problem when there are
more steps.

---

Previous: [Recursion without Deferments](#No-Deferment), Up: [Recursion](#Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.3.8 No Deferment Solution

The solution to the problem of deferred operations is to write in a
manner that does not defer operations[15](#FOOT15). This requires
writing to a different pattern, often one that involves writing two
function definitions, an initialization function and a helper
function.

The initialization function sets up the job; the helper function
does the work.

Here are the two function definitions for adding up numbers. They are
so simple, I find them hard to understand.

```
(defun triangle-initialization (number)
  "Return the sum of the numbers 1 through NUMBER inclusive.
This is the initialization component of a two function
duo that uses recursion."
  (triangle-recursive-helper 0 0 number))
```

```
(defun triangle-recursive-helper (sum counter number)
  "Return SUM, using COUNTER, through NUMBER inclusive.
This is the helper component of a two function duo
that uses recursion."
  (if (> counter number)
      sum
    (triangle-recursive-helper (+ sum counter)  ; sum
                               (1+ counter)     ; counter
                               number)))        ; number
```

Install both function definitions by evaluating them, then call
`triangle-initialization` with 2 rows:

```
(triangle-initialization 2)
    ⇒ 3
```

The initialization function calls the first instance of the helper
function with three arguments: zero, zero, and a number which is the
number of rows in the triangle.

The first two arguments passed to the helper function are
initialization values. These values are changed when
`triangle-recursive-helper` invokes new instances.[16](#FOOT16)

Let’s see what happens when we have a triangle that has one row. (This
triangle will have one pebble in it!)

`triangle-initialization` will call its helper with
the arguments `0 0 1`. That function will run the conditional
test whether `(> counter number)`:

```
(> 0 1)
```

and find that the result is false, so it will invoke
the else-part of the `if` clause:

```
    (triangle-recursive-helper
     (+ sum counter)  ; sum plus counter ⇒ sum
     (1+ counter)     ; increment counter ⇒ counter
     number)          ; number stays the same
```

which will first compute:

```
(triangle-recursive-helper (+ 0 0)  ; sum
                           (1+ 0)   ; counter
                           1)       ; number
```

```
which is:
```

```
(triangle-recursive-helper 0 1 1)
```

Again, `(> counter number)` will be false, so again, the Lisp
interpreter will evaluate `triangle-recursive-helper`, creating a
new instance with new arguments.

This new instance will be;

```
    (triangle-recursive-helper
     (+ sum counter)  ; sum plus counter ⇒ sum
     (1+ counter)     ; increment counter ⇒ counter
     number)          ; number stays the same
```

```
which is:
```

```
(triangle-recursive-helper 1 2 1)
```

In this case, the `(> counter number)` test will be true! So the
instance will return the value of the sum, which will be 1, as
expected.

Now, let’s pass `triangle-initialization` an argument
of 2, to find out how many pebbles there are in a triangle with two rows.

That function calls `(triangle-recursive-helper 0 0 2)`.

In stages, the instances called will be:

```
                          sum counter number
(triangle-recursive-helper 0    1       2)

(triangle-recursive-helper 1    2       2)

(triangle-recursive-helper 3    3       2)
```

When the last instance is called, the `(> counter number)` test
will be true, so the instance will return the value of `sum`,
which will be 3.

This kind of pattern helps when you are writing functions that can use
many resources in a computer.

---

Previous: [Recursion](#Recursion), Up: [Loops and Recursion](#Loops-_0026-Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.4 Looping Exercise

* Write a function similar to `triangle` in which each row has a
  value which is the square of the row number. Use a `while` loop.
* Write a function similar to `triangle` that multiplies instead of
  adds the values.
* Rewrite these two functions recursively. Rewrite these functions
  using `cond`.
* Write a function for Texinfo mode that creates an index entry at the
  beginning of a paragraph for every ‘`@dfn`’ within the paragraph.
  (In a Texinfo file, ‘`@dfn`’ marks a definition. This book is
  written in Texinfo.)

  Many of the functions you will need are described in two of the
  previous chapters, [Cutting and Storing
  Text](#Cutting-_0026-Storing-Text), and [Yanking Text Back](#Yanking). If you use
  `forward-paragraph` to put the index entry at the beginning of
  the paragraph, you will have to use `C-h f`
  (`describe-function`) to find out how to make the command go
  backwards.

  For more information, see
  [Indicating](https://www.gnu.org/software/texinfo/manual/texinfo/texinfo.html#Indicating) in Texinfo Manual, which goes to
  a Texinfo manual in the current directory. Or, if you are on the
  Internet, see
  <https://www.gnu.org/software/texinfo/manual/texinfo/>

---

Next: [Counting via Repetition and Regexps](#Counting-Words), Previous: [Loops and Recursion](#Loops-_0026-Recursion), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 12 Regular Expression Searches

Regular expression searches are used extensively in GNU Emacs. The
two functions, `forward-sentence` and `forward-paragraph`,
illustrate these searches well. They use regular expressions to find
where to move point. The phrase “regular expression” is often written
as “regexp”.

Regular expression searches are described in [Regular Expression Search](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexp-Search) in The GNU Emacs Manual, as well as in
[Regular Expressions](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Regular-Expressions) in The GNU Emacs Lisp Reference
Manual. In writing this chapter, I am presuming that you have at
least a mild acquaintance with them. The major point to remember is
that regular expressions permit you to search for patterns as well as
for literal strings of characters. For example, the code in
`forward-sentence` searches for the pattern of possible
characters that could mark the end of a sentence, and moves point to
that spot.

Before looking at the code for the `forward-sentence` function, it
is worth considering what the pattern that marks the end of a sentence
must be. The pattern is discussed in the next section; following that
is a description of the regular expression search function,
`re-search-forward`. The `forward-sentence` function
is described in the section following. Finally, the
`forward-paragraph` function is described in the last section of
this chapter. `forward-paragraph` is a complex function that
introduces several new features.

* [The Regular Expression for `sentence-end`](#sentence_002dend)
* [The `re-search-forward` Function](#re_002dsearch_002dforward)
* [`forward-sentence`](#forward_002dsentence)
* [`forward-paragraph`: a Goldmine of Functions](#forward_002dparagraph)
* [Review](#Regexp-Review)
* [Exercises with `re-search-forward`](#re_002dsearch-Exercises)

---

Next: [The `re-search-forward` Function](#re_002dsearch_002dforward), Up: [Regular Expression Searches](#Regexp-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.1 The Regular Expression for `sentence-end`

The symbol `sentence-end` is bound to the pattern that marks the
end of a sentence. What should this regular expression be?

Clearly, a sentence may be ended by a period, a question mark, or an
exclamation mark. Indeed, in English, only clauses that end with one
of those three characters should be considered the end of a sentence.
This means that the pattern should include the character set:

```
[.?!]
```

However, we do not want `forward-sentence` merely to jump to a
period, a question mark, or an exclamation mark, because such a character
might be used in the middle of a sentence. A period, for example, is
used after abbreviations. So other information is needed.

According to convention, you type two spaces after every sentence, but
only one space after a period, a question mark, or an exclamation mark in
the body of a sentence. So a period, a question mark, or an exclamation
mark followed by two spaces is a good indicator of an end of sentence.
However, in a file, the two spaces may instead be a tab or the end of a
line. This means that the regular expression should include these three
items as alternatives.

This group of alternatives will look like this:

```
\\($\\| \\|  \\)
       ^   ^^
      TAB  SPC
```

Here, ‘`$`’ indicates the end of the line, and I have pointed out
where the tab and two spaces are inserted in the expression. Both are
inserted by putting the actual characters into the expression.

Two backslashes, ‘`\\`’, are required before the parentheses and
vertical bars: the first backslash quotes the following backslash in
Emacs; and the second indicates that the following character, the
parenthesis or the vertical bar, is special.

Also, a sentence may be followed by one or more carriage returns, like
this:

```
[
]*
```

Like tabs and spaces, a carriage return is inserted into a regular
expression by inserting it literally. The asterisk indicates that the
`RET` is repeated zero or more times.

But a sentence end does not consist only of a period, a question mark or
an exclamation mark followed by appropriate space: a closing quotation
mark or a closing brace of some kind may precede the space. Indeed more
than one such mark or brace may precede the space. These require a
expression that looks like this:

```
[]\"')}]*
```

In this expression, the first ‘`]`’ is the first character in the
expression; the second character is ‘`"`’, which is preceded by a
‘`\`’ to tell Emacs the ‘`"`’ is *not* special. The last
three characters are ‘`'`’, ‘`)`’, and ‘`}`’.

All this suggests what the regular expression pattern for matching the
end of a sentence should be; and, indeed, if we evaluate
`sentence-end` we find that it returns the following value:

```
sentence-end
     ⇒ "[.?!][]\"')}]*\\($\\|     \\|  \\)[
]*"
```

(Well, not in GNU Emacs 22; that is because of an effort to make the
process simpler and to handle more glyphs and languages. When the
value of `sentence-end` is `nil`, then use the value defined
by the function `sentence-end`. (Here is a use of the difference
between a value and a function in Emacs Lisp.) The function returns a
value constructed from the variables `sentence-end-base`,
`sentence-end-double-space`, `sentence-end-without-period`,
and `sentence-end-without-space`. The critical variable is
`sentence-end-base`; its global value is similar to the one
described above but it also contains two additional quotation marks.
These have differing degrees of curliness. The
`sentence-end-without-period` variable, when true, tells Emacs
that a sentence may end without a period, such as text in Thai.)

---

Next: [`forward-sentence`](#forward_002dsentence), Previous: [The Regular Expression for `sentence-end`](#sentence_002dend), Up: [Regular Expression Searches](#Regexp-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.2 The `re-search-forward` Function

The `re-search-forward` function is very like the
`search-forward` function. (See [The
`search-forward` Function](#search_002dforward).)

`re-search-forward` searches for a regular expression. If the
search is successful, it leaves point immediately after the last
character in the target. If the search is backwards, it leaves point
just before the first character in the target. You may tell
`re-search-forward` to return `t` for true. (Moving point
is therefore a side effect.)

Like `search-forward`, the `re-search-forward` function takes
four arguments:

1. The first argument is the regular expression that the function searches
   for. The regular expression will be a string between quotation marks.
2. The optional second argument limits how far the function will search; it is a
   bound, which is specified as a position in the buffer.
3. The optional third argument specifies how the function responds to
   failure: `nil` as the third argument causes the function to
   signal an error (and print a message) when the search fails; any other
   value causes it to return `nil` if the search fails and `t`
   if the search succeeds.
4. The optional fourth argument is the repeat count. A negative repeat
   count causes `re-search-forward` to search backwards.

The template for `re-search-forward` looks like this:

```
(re-search-forward "regular-expression"
                limit-of-search
                what-to-do-if-search-fails
                repeat-count)
```

The second, third, and fourth arguments are optional. However, if you
want to pass a value to either or both of the last two arguments, you
must also pass a value to all the preceding arguments. Otherwise, the
Lisp interpreter will mistake which argument you are passing the value
to.

In the `forward-sentence` function, the regular expression will be
the value of the variable `sentence-end`. In simple form, that is:

```
"[.?!][]\"')}]*\\($\\|  \\|  \\)[
]*"
```

The limit of the search will be the end of the paragraph (since a
sentence cannot go beyond a paragraph). If the search fails, the
function will return `nil`; and the repeat count will be provided
by the argument to the `forward-sentence` function.

---

Next: [`forward-paragraph`: a Goldmine of Functions](#forward_002dparagraph), Previous: [The `re-search-forward` Function](#re_002dsearch_002dforward), Up: [Regular Expression Searches](#Regexp-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.3 `forward-sentence`

The command to move the cursor forward a sentence is a straightforward
illustration of how to use regular expression searches in Emacs Lisp.
Indeed, the function looks longer and more complicated than it is; this
is because the function is designed to go backwards as well as forwards;
and, optionally, over more than one sentence. The function is usually
bound to the key command `M-e`.

* [Complete `forward-sentence` function definition](#Complete-forward_002dsentence)
* [The `while` loops](#fwd_002dsentence-while-loops)
* [The regular expression search](#fwd_002dsentence-re_002dsearch)

---

Next: [The `while` loops](#fwd_002dsentence-while-loops), Up: [`forward-sentence`](#forward_002dsentence)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Complete `forward-sentence` function definition

Here is the code for `forward-sentence`:

```
(defun forward-sentence (&optional arg)
  "Move forward to next end of sentence.  With argument, repeat.
With negative argument, move backward repeatedly to start of sentence.

The variable `sentence-end' is a regular expression that matches ends of
sentences.  Also, every paragraph boundary terminates sentences as well."
```

```
  (interactive "p")
  (or arg (setq arg 1))
  (let ((opoint (point))
        (sentence-end (sentence-end)))
    (while (< arg 0)
      (let ((pos (point))
            (par-beg (save-excursion (start-of-paragraph-text) (point))))
       (if (and (re-search-backward sentence-end par-beg t)
                (or (< (match-end 0) pos)
                    (re-search-backward sentence-end par-beg t)))
           (goto-char (match-end 0))
         (goto-char par-beg)))
      (setq arg (1+ arg)))
```

```
    (while (> arg 0)
      (let ((par-end (save-excursion (end-of-paragraph-text) (point))))
       (if (re-search-forward sentence-end par-end t)
           (skip-chars-backward " \t\n")
         (goto-char par-end)))
      (setq arg (1- arg)))
    (constrain-to-field nil opoint t)))
```

The function looks long at first sight and it is best to look at its
skeleton first, and then its muscle. The way to see the skeleton is to
look at the expressions that start in the left-most columns:

```
(defun forward-sentence (&optional arg)
  "documentation…"
  (interactive "p")
  (or arg (setq arg 1))
  (let ((opoint (point)) (sentence-end (sentence-end)))
    (while (< arg 0)
      (let ((pos (point))
            (par-beg (save-excursion (start-of-paragraph-text) (point))))
       rest-of-body-of-while-loop-when-going-backwards
    (while (> arg 0)
      (let ((par-end (save-excursion (end-of-paragraph-text) (point))))
       rest-of-body-of-while-loop-when-going-forwards
    handle-forms-and-equivalent
```

This looks much simpler! The function definition consists of
documentation, an `interactive` expression, an `or`
expression, a `let` expression, and `while` loops.

Let’s look at each of these parts in turn.

We note that the documentation is thorough and understandable.

The function has an `interactive "p"` declaration. This means
that the processed prefix argument, if any, is passed to the
function as its argument. (This will be a number.) If the function
is not passed an argument (it is optional) then the argument
`arg` will be bound to 1.

When `forward-sentence` is called non-interactively without an
argument, `arg` is bound to `nil`. The `or` expression
handles this. What it does is either leave the value of `arg` as
it is, but only if `arg` is bound to a value; or it sets the
value of `arg` to 1, in the case when `arg` is bound to
`nil`.

Next is a `let`. That specifies the values of two local
variables, `opoint` and `sentence-end`. The local value of
point, from before the search, is used in the
`constrain-to-field` function which handles forms and
equivalents. The `sentence-end` variable is set by the
`sentence-end` function.

---

Next: [The regular expression search](#fwd_002dsentence-re_002dsearch), Previous: [Complete `forward-sentence` function definition](#Complete-forward_002dsentence), Up: [`forward-sentence`](#forward_002dsentence)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The `while` loops

Two `while` loops follow. The first `while` has a
true-or-false-test that tests true if the prefix argument for
`forward-sentence` is a negative number. This is for going
backwards. The body of this loop is similar to the body of the second
`while` clause, but it is not exactly the same. We will skip
this `while` loop and concentrate on the second `while`
loop.

The second `while` loop is for moving point forward. Its skeleton
looks like this:

```
(while (> arg 0)            ; true-or-false-test
  (let varlist
    (if (true-or-false-test)
        then-part
      else-part
  (setq arg (1- arg))))     ; while loop decrementer
```

The `while` loop is of the decrementing kind.
(See [A Loop with a Decrementing Counter](#Decrementing-Loop).) It
has a true-or-false-test that tests true so long as the counter (in
this case, the variable `arg`) is greater than zero; and it has a
decrementer that subtracts 1 from the value of the counter every time
the loop repeats.

If no prefix argument is given to `forward-sentence`, which is
the most common way the command is used, this `while` loop will
run once, since the value of `arg` will be 1.

The body of the `while` loop consists of a `let` expression,
which creates and binds a local variable, and has, as its body, an
`if` expression.

The body of the `while` loop looks like this:

```
(let ((par-end
       (save-excursion (end-of-paragraph-text) (point))))
  (if (re-search-forward sentence-end par-end t)
      (skip-chars-backward " \t\n")
    (goto-char par-end)))
```

The `let` expression creates and binds the local variable
`par-end`. As we shall see, this local variable is designed to
provide a bound or limit to the regular expression search. If the
search fails to find a proper sentence ending in the paragraph, it will
stop on reaching the end of the paragraph.

But first, let us examine how `par-end` is bound to the value of
the end of the paragraph. What happens is that the `let` sets the
value of `par-end` to the value returned when the Lisp interpreter
evaluates the expression

```
(save-excursion (end-of-paragraph-text) (point))
```

In this expression, `(end-of-paragraph-text)` moves point to the
end of the paragraph, `(point)` returns the value of point, and then
`save-excursion` restores point to its original position. Thus,
the `let` binds `par-end` to the value returned by the
`save-excursion` expression, which is the position of the end of
the paragraph. (The `end-of-paragraph-text` function uses
`forward-paragraph`, which we will discuss shortly.)

Emacs next evaluates the body of the `let`, which is an `if`
expression that looks like this:

```
(if (re-search-forward sentence-end par-end t) ; if-part
    (skip-chars-backward " \t\n")              ; then-part
  (goto-char par-end)))                        ; else-part
```

The `if` tests whether its first argument is true and if so,
evaluates its then-part; otherwise, the Emacs Lisp interpreter
evaluates the else-part. The true-or-false-test of the `if`
expression is the regular expression search.

It may seem odd to have what looks like the real work of
the `forward-sentence` function buried here, but this is a common
way this kind of operation is carried out in Lisp.

---

Previous: [The `while` loops](#fwd_002dsentence-while-loops), Up: [`forward-sentence`](#forward_002dsentence)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The regular expression search

The `re-search-forward` function searches for the end of the
sentence, that is, for the pattern defined by the `sentence-end`
regular expression. If the pattern is found—if the end of the sentence is
found—then the `re-search-forward` function does two things:

1. The `re-search-forward` function carries out a side effect, which
   is to move point to the end of the occurrence found.
2. The `re-search-forward` function returns a value of true. This is
   the value received by the `if`, and means that the search was
   successful.

The side effect, the movement of point, is completed before the
`if` function is handed the value returned by the successful
conclusion of the search.

When the `if` function receives the value of true from a successful
call to `re-search-forward`, the `if` evaluates the then-part,
which is the expression `(skip-chars-backward " \t\n")`. This
expression moves backwards over any blank spaces, tabs or carriage
returns until a printed character is found and then leaves point after
the character. Since point has already been moved to the end of the
pattern that marks the end of the sentence, this action leaves point
right after the closing printed character of the sentence, which is
usually a period.

On the other hand, if the `re-search-forward` function fails to
find a pattern marking the end of the sentence, the function returns
false. The false then causes the `if` to evaluate its third
argument, which is `(goto-char par-end)`: it moves point to the
end of the paragraph.

(And if the text is in a form or equivalent, and point may not move
fully, then the `constrain-to-field` function comes into play.)

Regular expression searches are exceptionally useful and the pattern
illustrated by `re-search-forward`, in which the search is the
test of an `if` expression, is handy. You will see or write code
incorporating this pattern often.

---

Next: [Review](#Regexp-Review), Previous: [`forward-sentence`](#forward_002dsentence), Up: [Regular Expression Searches](#Regexp-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.4 `forward-paragraph`: a Goldmine of Functions

The `forward-paragraph` function moves point forward to the end
of the paragraph. It is usually bound to `M-}` and makes use of a
number of functions that are important in themselves, including
`let*`, `match-beginning`, and `looking-at`.

The function definition for `forward-paragraph` is considerably
longer than the function definition for `forward-sentence`
because it works with a paragraph, each line of which may begin with a
fill prefix.

A fill prefix consists of a string of characters that are repeated at
the beginning of each line. For example, in Lisp code, it is a
convention to start each line of a paragraph-long comment with
‘`;;;` ’. In Text mode, four blank spaces make up another common
fill prefix, creating an indented paragraph. (See [Fill Prefix](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fill-Prefix) in The GNU Emacs Manual, for more information about fill
prefixes.)

The existence of a fill prefix means that in addition to being able to
find the end of a paragraph whose lines begin on the left-most
column, the `forward-paragraph` function must be able to find the
end of a paragraph when all or many of the lines in the buffer begin
with the fill prefix.

Moreover, it is sometimes practical to ignore a fill prefix that
exists, especially when blank lines separate paragraphs.
This is an added complication.

* [Shortened `forward-paragraph` function definition](#forward_002dparagraph-in-brief)
* [The `let*` expression](#fwd_002dpara-let)
* [The forward motion `while` loop](#fwd_002dpara-while)

---

Next: [The `let*` expression](#fwd_002dpara-let), Up: [`forward-paragraph`: a Goldmine of Functions](#forward_002dparagraph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Shortened `forward-paragraph` function definition

Rather than print all of the `forward-paragraph` function, we
will only print parts of it. Read without preparation, the function
can be daunting!

In outline, the function looks like this:

```
(defun forward-paragraph (&optional arg)
  "documentation…"
  (interactive "p")
  (or arg (setq arg 1))
  (let*
      varlist
    (while (and (< arg 0) (not (bobp)))     ; backward-moving-code
      …
    (while (and (> arg 0) (not (eobp)))     ; forward-moving-code
      …
```

The first parts of the function are routine: the function’s argument
list consists of one optional argument. Documentation follows.

The lower case ‘`p`’ in the `interactive` declaration means
that the processed prefix argument, if any, is passed to the function.
This will be a number, and is the repeat count of how many paragraphs
point will move. The `or` expression in the next line handles
the common case when no argument is passed to the function, which occurs
if the function is called from other code rather than interactively.
This case was described earlier. (See [The
`forward-sentence` function](#forward_002dsentence).) Now we reach the end of the
familiar part of this function.

---

Next: [The forward motion `while` loop](#fwd_002dpara-while), Previous: [Shortened `forward-paragraph` function definition](#forward_002dparagraph-in-brief), Up: [`forward-paragraph`: a Goldmine of Functions](#forward_002dparagraph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The `let*` expression

The next line of the `forward-paragraph` function begins a
`let*` expression (see [`let*`
introduced](#let_002a-introduced)), in which Emacs binds a total of seven variables:
`opoint`, `fill-prefix-regexp`, `parstart`,
`parsep`, `sp-parstart`, `start`, and
`found-start`. The first part of the `let*` expression
looks like below:

```
(let* ((opoint (point))
       (fill-prefix-regexp
        (and fill-prefix (not (equal fill-prefix ""))
             (not paragraph-ignore-fill-prefix)
             (regexp-quote fill-prefix)))
       ;; Remove ^ from paragraph-start and paragraph-sep if they are there.
       ;; These regexps shouldn't be anchored, because we look for them
       ;; starting at the left-margin.  This allows paragraph commands to
       ;; work normally with indented text.
       ;; This hack will not find problem cases like "whatever\\|^something".
       (parstart (if (and (not (equal "" paragraph-start))
                          (equal ?^ (aref paragraph-start 0)))
                     (substring paragraph-start 1)
                   paragraph-start))
       (parsep (if (and (not (equal "" paragraph-separate))
                        (equal ?^ (aref paragraph-separate 0)))
                   (substring paragraph-separate 1)
                 paragraph-separate))
       (parsep
        (if fill-prefix-regexp
            (concat parsep "\\|"
                    fill-prefix-regexp "[ \t]*$")
          parsep))
       ;; This is used for searching.
       (sp-parstart (concat "^[ \t]*\\(?:" parstart "\\|" parsep "\\)"))
       start found-start)
  ...)
```

The variable `parsep` appears twice, first, to remove instances
of ‘`^`’, and second, to handle fill prefixes.

The variable `opoint` is just the value of `point`. As you
can guess, it is used in a `constrain-to-field` expression, just
as in `forward-sentence`.

The variable `fill-prefix-regexp` is set to the value returned by
evaluating the following list:

```
(and fill-prefix
     (not (equal fill-prefix ""))
     (not paragraph-ignore-fill-prefix)
     (regexp-quote fill-prefix))
```

This is an expression whose first element is the `and` special form.

As we learned earlier (see [The `kill-new`
function](#kill_002dnew-function)), the `and` special form evaluates each of its
arguments until one of the arguments returns a value of `nil`, in
which case the `and` expression returns `nil`; however, if
none of the arguments returns a value of `nil`, the value
resulting from evaluating the last argument is returned. (Since such
a value is not `nil`, it is considered true in Lisp.) In other
words, an `and` expression returns a true value only if all its
arguments are true.

In this case, the variable `fill-prefix-regexp` is bound to a
non-`nil` value only if the following four expressions produce a
true (i.e., a non-`nil`) value when they are evaluated; otherwise,
`fill-prefix-regexp` is bound to `nil`.

`fill-prefix`
:   When this variable is evaluated, the value of the fill prefix, if any,
    is returned. If there is no fill prefix, this variable returns
    `nil`.

`(not (equal fill-prefix "")`
:   This expression checks whether an existing fill prefix is an empty
    string, that is, a string with no characters in it. An empty string is
    not a useful fill prefix.

`(not paragraph-ignore-fill-prefix)`
:   This expression returns `nil` if the variable
    `paragraph-ignore-fill-prefix` has been turned on by being set to a
    true value such as `t`.

`(regexp-quote fill-prefix)`
:   This is the last argument to the `and` special form. If all the
    arguments to the `and` are true, the value resulting from
    evaluating this expression will be returned by the `and` expression
    and bound to the variable `fill-prefix-regexp`,

The result of evaluating this `and` expression successfully is that
`fill-prefix-regexp` will be bound to the value of
`fill-prefix` as modified by the `regexp-quote` function.
What `regexp-quote` does is read a string and return a regular
expression that will exactly match the string and match nothing else.
This means that `fill-prefix-regexp` will be set to a value that
will exactly match the fill prefix if the fill prefix exists.
Otherwise, the variable will be set to `nil`.

The next two local variables in the `let*` expression are
designed to remove instances of ‘`^`’ from `parstart` and
`parsep`, the local variables which indicate the paragraph start
and the paragraph separator. The next expression sets `parsep`
again. That is to handle fill prefixes.

This is the setting that requires the definition call `let*`
rather than `let`. The true-or-false-test for the `if`
depends on whether the variable `fill-prefix-regexp` evaluates to
`nil` or some other value.

If `fill-prefix-regexp` does not have a value, Emacs evaluates
the else-part of the `if` expression and binds `parsep` to
its local value. (`parsep` is a regular expression that matches
what separates paragraphs.)

But if `fill-prefix-regexp` does have a value, Emacs evaluates
the then-part of the `if` expression and binds `parsep` to a
regular expression that includes the `fill-prefix-regexp` as part
of the pattern.

Specifically, `parsep` is set to the original value of the
paragraph separate regular expression concatenated with an alternative
expression that consists of the `fill-prefix-regexp` followed by
optional whitespace to the end of the line. The whitespace is defined
by `"[ \t]*$"`.) The ‘`\\|`’ defines this portion of the
regexp as an alternative to `parsep`.

According to a comment in the code, the next local variable,
`sp-parstart`, is used for searching, and then the final two,
`start` and `found-start`, are set to `nil`.

Now we get into the body of the `let*`. The first part of the body
of the `let*` deals with the case when the function is given a
negative argument and is therefore moving backwards. We will skip this
section.

---

Previous: [The `let*` expression](#fwd_002dpara-let), Up: [`forward-paragraph`: a Goldmine of Functions](#forward_002dparagraph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The forward motion `while` loop

The second part of the body of the `let*` deals with forward
motion. It is a `while` loop that repeats itself so long as the
value of `arg` is greater than zero. In the most common use of
the function, the value of the argument is 1, so the body of the
`while` loop is evaluated exactly once, and the cursor moves
forward one paragraph.

This part handles three situations: when point is between paragraphs,
when there is a fill prefix and when there is no fill prefix.

The `while` loop looks like this:

```
;; going forwards and not at the end of the buffer
(while (and (> arg 0) (not (eobp)))

  ;; between paragraphs
  ;; Move forward over separator lines...
  (while (and (not (eobp))
              (progn (move-to-left-margin) (not (eobp)))
              (looking-at parsep))
    (forward-line 1))
  ;;  This decrements the loop
  (unless (eobp) (setq arg (1- arg)))
  ;; ... and one more line.
  (forward-line 1)
```

```

```

```
  (if fill-prefix-regexp
      ;; There is a fill prefix; it overrides parstart;
      ;; we go forward line by line
      (while (and (not (eobp))
                  (progn (move-to-left-margin) (not (eobp)))
                  (not (looking-at parsep))
                  (looking-at fill-prefix-regexp))
        (forward-line 1))
```

```

```

```
    ;; There is no fill prefix;
    ;; we go forward character by character
    (while (and (re-search-forward sp-parstart nil 1)
                (progn (setq start (match-beginning 0))
                       (goto-char start)
                       (not (eobp)))
                (progn (move-to-left-margin)
                       (not (looking-at parsep)))
                (or (not (looking-at parstart))
                    (and use-hard-newlines
                         (not (get-text-property (1- start) 'hard)))))
      (forward-char 1))
```

```

```

```
    ;; and if there is no fill prefix and if we are not at the end,
    ;;     go to whatever was found in the regular expression search
    ;;     for sp-parstart
    (if (< (point) (point-max))
        (goto-char start))))
```

We can see that this is a decrementing counter `while` loop,
using the expression `(setq arg (1- arg))` as the decrementer.
That expression is not far from the `while`, but is hidden in
another Lisp macro, an `unless` macro. Unless we are at the end
of the buffer—that is what the `eobp` function determines; it
is an abbreviation of ‘`End Of Buffer P`’—we decrease the value
of `arg` by one.

(If we are at the end of the buffer, we cannot go forward any more and
the next loop of the `while` expression will test false since the
test is an `and` with `(not (eobp))`. The `not`
function means exactly as you expect; it is another name for
`null`, a function that returns true when its argument is false.)

Interestingly, the loop count is not decremented until we leave the
space between paragraphs, unless we come to the end of buffer or stop
seeing the local value of the paragraph separator.

That second `while` also has a `(move-to-left-margin)`
expression. The function is self-explanatory. It is inside a
`progn` expression and not the last element of its body, so it is
only invoked for its side effect, which is to move point to the left
margin of the current line.

The `looking-at` function is also self-explanatory; it returns
true if the text after point matches the regular expression given as
its argument.

The rest of the body of the loop looks difficult at first, but makes
sense as you come to understand it.

First consider what happens if there is a fill prefix:

```
  (if fill-prefix-regexp
      ;; There is a fill prefix; it overrides parstart;
      ;; we go forward line by line
      (while (and (not (eobp))
                  (progn (move-to-left-margin) (not (eobp)))
                  (not (looking-at parsep))
                  (looking-at fill-prefix-regexp))
        (forward-line 1))
```

This expression moves point forward line by line so long
as four conditions are true:

1. Point is not at the end of the buffer.
2. We can move to the left margin of the text and are
   not at the end of the buffer.
3. The text following point does not separate paragraphs.
4. The pattern following point is the fill prefix regular expression.

The last condition may be puzzling, until you remember that point was
moved to the beginning of the line early in the `forward-paragraph`
function. This means that if the text has a fill prefix, the
`looking-at` function will see it.

Consider what happens when there is no fill prefix.

```
    (while (and (re-search-forward sp-parstart nil 1)
                (progn (setq start (match-beginning 0))
                       (goto-char start)
                       (not (eobp)))
                (progn (move-to-left-margin)
                       (not (looking-at parsep)))
                (or (not (looking-at parstart))
                    (and use-hard-newlines
                         (not (get-text-property (1- start) 'hard)))))
      (forward-char 1))
```

This `while` loop has us searching forward for
`sp-parstart`, which is the combination of possible whitespace
with the local value of the start of a paragraph or of a paragraph
separator. (The latter two are within an expression starting
`\(?:` so that they are not referenced by the
`match-beginning` function.)

The two expressions,

```
(setq start (match-beginning 0))
(goto-char start)
```

mean go to the start of the text matched by the regular expression
search.

The `(match-beginning 0)` expression is new. It returns a number
specifying the location of the start of the text that was matched by
the last search.

The `match-beginning` function is used here because of a
characteristic of a forward search: a successful forward search,
regardless of whether it is a plain search or a regular expression
search, moves point to the end of the text that is found. In this
case, a successful search moves point to the end of the pattern for
`sp-parstart`.

However, we want to put point at the end of the current paragraph, not
somewhere else. Indeed, since the search possibly includes the
paragraph separator, point may end up at the beginning of the next one
unless we use an expression that includes `match-beginning`.

When given an argument of 0, `match-beginning` returns the
position that is the start of the text matched by the most recent
search. In this case, the most recent search looks for
`sp-parstart`. The `(match-beginning 0)` expression returns
the beginning position of that pattern, rather than the end position
of that pattern.

(Incidentally, when passed a positive number as an argument, the
`match-beginning` function returns the location of point at that
parenthesized expression in the last search unless that parenthesized
expression begins with `\(?:`. I don’t know why `\(?:`
appears here since the argument is 0.)

The last expression when there is no fill prefix is

```
(if (< (point) (point-max))
    (goto-char start))))
```

(Note that this code snippet is copied verbatim from the original code,
so the two extra ending parentheses are matching the previous `if`
and `while`.)

This says that if there is no fill prefix and if we are not at the
end, point should move to the beginning of whatever was found by the
regular expression search for `sp-parstart`.

The full definition for the `forward-paragraph` function not only
includes code for going forwards, but also code for going backwards.

If you are reading this inside of GNU Emacs and you want to see the
whole function, you can type `C-h f` (`describe-function`)
and the name of the function. This gives you the function
documentation and the name of the library containing the function’s
source. Place point over the name of the library and press the `RET`
key; you will be taken directly to the source. (Be sure to install
your sources! Without them, you are like a person who tries to drive
a car with his eyes shut!)

---

Next: [Exercises with `re-search-forward`](#re_002dsearch-Exercises), Previous: [`forward-paragraph`: a Goldmine of Functions](#forward_002dparagraph), Up: [Regular Expression Searches](#Regexp-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.5 Review

Here is a brief summary of some recently introduced functions.

`while`
:   Repeatedly evaluate the body of the expression so long as the first
    element of the body tests true. Then return `nil`. (The
    expression is evaluated only for its side effects.)

    For example:

    ```
    (let ((foo 2))
      (while (> foo 0)
        (insert (format "foo is %d.\n" foo))
        (setq foo (1- foo))))

         ⇒      foo is 2.
                 foo is 1.
                 nil
    ```

    (The `insert` function inserts its arguments at point; the
    `format` function returns a string formatted from its arguments
    the way `message` formats its arguments; `\n` produces a new
    line.)

`re-search-forward`
:   Search for a pattern, and if the pattern is found, move point to rest
    just after it.

    Takes four arguments, like `search-forward`:

    1. A regular expression that specifies the pattern to search for.
       (Remember to put quotation marks around this argument!)
    2. Optionally, the limit of the search.
    3. Optionally, what to do if the search fails, return `nil` or an
       error message.
    4. Optionally, how many times to repeat the search; if negative, the
       search goes backwards.

`let*`
:   Bind some variables locally to particular values,
    and then evaluate the remaining arguments, returning the value of the
    last one. While binding the local variables, use the local values of
    variables bound earlier, if any.

    For example:

    ```
    (let* ((foo 7)
           (bar (* 3 foo)))
      (message "`bar' is %d." bar))
         ⇒ ‘bar’ is 21.
    ```

`match-beginning`
:   Return the position of the start of the text found by the last regular
    expression search.

`looking-at`
:   Return `t` for true if the text after point matches the argument,
    which should be a regular expression.

`eobp`
:   Return `t` for true if point is at the end of the accessible part
    of a buffer. The end of the accessible part is the end of the buffer
    if the buffer is not narrowed; it is the end of the narrowed part if
    the buffer is narrowed.

---

Previous: [Review](#Regexp-Review), Up: [Regular Expression Searches](#Regexp-Search)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.6 Exercises with `re-search-forward`

* Write a function to search for a regular expression that matches two
  or more blank lines in sequence.
* Write a function to search for duplicated words, such as “the the”.
  See [Syntax of Regular Expressions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexps) in The GNU Emacs
  Manual, for information on how to write a regexp (a regular
  expression) to match a string that is composed of two identical
  halves. You can devise several regexps; some are better than others.
  The function I use is described in an appendix, along with several
  regexps. See [`the-the` Duplicated Words Function](#the_002dthe).

---

Next: [Counting Words in a `defun`](#Words-in-a-defun), Previous: [Regular Expression Searches](#Regexp-Search), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 13 Counting via Repetition and Regexps

Repetition and regular expression searches are powerful tools that you
often use when you write code in Emacs Lisp. This chapter illustrates
the use of regular expression searches through the construction of
word count commands using `while` loops and recursion.

* [Counting words](#Why-Count-Words)
* [The `count-words-example` Function](#count_002dwords_002dexample)
* [Count Words Recursively](#recursive_002dcount_002dwords)
* [Exercise: Counting Punctuation](#Counting-Exercise)

---

Next: [The `count-words-example` Function](#count_002dwords_002dexample), Up: [Counting via Repetition and Regexps](#Counting-Words)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Counting words

The standard Emacs distribution contains functions for counting the
number of lines and words within a region.

Certain types of writing ask you to count words. Thus, if you write
an essay, you may be limited to 800 words; if you write a novel, you
may discipline yourself to write 1000 words a day. It seems odd, but
for a long time, Emacs lacked a word count command. Perhaps people used
Emacs mostly for code or types of documentation that did not require
word counts; or perhaps they restricted themselves to the operating
system word count command, `wc`. Alternatively, people may have
followed the publishers’ convention and computed a word count by
dividing the number of characters in a document by five.

There are many ways to implement a command to count words. Here are
some examples, which you may wish to compare with the standard Emacs
command, `count-words-region`.

---

Next: [Count Words Recursively](#recursive_002dcount_002dwords), Previous: [Counting words](#Why-Count-Words), Up: [Counting via Repetition and Regexps](#Counting-Words)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
