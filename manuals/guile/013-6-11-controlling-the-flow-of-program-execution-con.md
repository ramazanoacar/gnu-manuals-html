### 6.11 Controlling the Flow of Program Execution [¶](#Controlling-the-Flow-of-Program-Execution)

See [Control Flow](#Control-Flow) for a discussion of how the more general control
flow of Scheme affects C code.

* [Sequencing and Splicing](#begin)
* [Simple Conditional Evaluation](#Conditionals)
* [Conditional Evaluation of a Sequence of Expressions](#and-or)
* [Iteration mechanisms](#while-do)
* [Prompts](#Prompts)
* [Continuations](#Continuations)
* [Returning and Accepting Multiple Values](#Multiple-Values)
* [Exceptions](#Exceptions)
* [Procedures for Signaling Errors](#Error-Reporting)
* [Dynamic Wind](#Dynamic-Wind)
* [Fluids and Dynamic States](#Fluids-and-Dynamic-States)
* [Parameters](#Parameters)
* [How to Handle Errors](#Handling-Errors)
* [Continuation Barriers](#Continuation-Barriers)

---

Next: [Simple Conditional Evaluation](#Conditionals), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.1 Sequencing and Splicing [¶](#Sequencing-and-Splicing)

As an expression, the `begin` syntax is used to evaluate a sequence
of sub-expressions in order. Consider the conditional expression below:

```
(if (> x 0)
    (begin (display "greater") (newline)))
```

If the test is true, we want to display “greater” to the current
output port, then display a newline. We use `begin` to form a
compound expression out of this sequence of sub-expressions.

syntax: **begin** expr … [¶](#index-begin-1)
:   The expression(s) are evaluated in left-to-right order and the values of
    the last expression are returned as the result of the
    `begin`-expression. This expression type is used when the
    expressions before the last one are evaluated for their side effects.

The `begin` syntax has another role in definition context
(see [Internal definitions](#Internal-Definitions)). A `begin` form in a definition
context *splices* its subforms into its place. For example,
consider the following procedure:

```
(define (make-seal)
  (define-sealant seal open)
  (values seal open))
```

Let us assume the existence of a `define-sealant` macro that
expands out to some definitions wrapped in a `begin`, like so:

```
(define (make-seal)
  (begin
    (define seal-tag
      (list 'seal))
    (define (seal x)
      (cons seal-tag x))
    (define (sealed? x)
      (and (pair? x) (eq? (car x) seal-tag)))
    (define (open x)
      (if (sealed? x)
          (cdr x)
          (error "Expected a sealed value:" x))))
  (values seal open))
```

Here, because the `begin` is in definition context, its subforms
are *spliced* into the place of the `begin`. This allows the
definitions created by the macro to be visible to the following
expression, the `values` form.

It is a fine point, but splicing and sequencing are different. It can
make sense to splice zero forms, because it can make sense to have zero
internal definitions before the expressions in a procedure or lexical
binding form. However it does not make sense to have a sequence of zero
expressions, because in that case it would not be clear what the value
of the sequence would be, because in a sequence of zero expressions,
there can be no last value. Sequencing zero expressions is an error.

It would be more elegant in some ways to eliminate splicing from the
Scheme language, and without macros (see [Macros](#Macros)), that would be a
good idea. But it is useful to be able to write macros that expand out
to multiple definitions, as in `define-sealant` above, so Scheme
abuses the `begin` form for these two tasks.

---

Next: [Conditional Evaluation of a Sequence of Expressions](#and-or), Previous: [Sequencing and Splicing](#begin), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.2 Simple Conditional Evaluation [¶](#Simple-Conditional-Evaluation)

Guile provides three syntactic constructs for conditional evaluation.
`if` is the normal if-then-else expression (with an optional else
branch), `cond` is a conditional expression with multiple branches,
and `case` branches if an expression has one of a set of constant
values.

syntax: **if** test consequent [alternate] [¶](#index-if-1)
:   All arguments may be arbitrary expressions. First, test is
    evaluated. If it returns a true value, the expression consequent
    is evaluated and alternate is ignored. If test evaluates to
    `#f`, alternate is evaluated instead. The values of the
    evaluated branch (consequent or alternate) are returned as
    the values of the `if` expression.

    When alternate is omitted and the test evaluates to
    `#f`, the value of the expression is not specified.

When you go to write an `if` without an alternate (a *one-armed
`if`*), part of what you are expressing is that you don’t care
about the return value (or values) of the expression. As such, you are
more interested in the *effect* of evaluating the consequent
expression. (By convention, we use the word *statement* to refer to
an expression that is evaluated for effect, not for value).

In such a case, it is considered more clear to express these intentions
with the special forms `when` and `unless`. As an added
bonus, these forms take a *body* like in a `let` expression,
which can contain internal definitions and multiple statements to
evaluate (see [Local Variable Bindings](#Local-Bindings)).

Scheme Syntax: **when** test body [¶](#index-when-1)

Scheme Syntax: **unless** test body [¶](#index-unless-1)
:   The actual definitions of these forms may be their most clear documentation:

    ```
    (define-syntax-rule (when test stmt stmt* ...)
      (if test (let () stmt stmt* ...)))

    (define-syntax-rule (unless test stmt stmt* ...)
      (if (not test) (let () stmt stmt* ...)))
    ```

    That is to say, `when` evaluates its consequent statements in order
    if test is true. `unless` is the opposite: it evaluates the
    statements if test is false.

syntax: **cond** clause1 clause2 … [¶](#index-cond-1)
:   Each `cond`-clause must look like this:

    ```
    (test body)
    ```

    where test is an arbitrary expression, or like this

    ```
    (test => expression)
    ```

    where expression must evaluate to a procedure.

    The tests of the clauses are evaluated in order and as soon as one
    of them evaluates to a true value, the corresponding body is
    evaluated to produce the result of the `cond`-expression. For the
    `=>` clause type,
    expression is evaluated and the resulting procedure is applied to
    the value of test. The result of this procedure application is
    then the result of the `cond`-expression.

    One additional `cond`-clause is available as an extension to
    standard Scheme:

    ```
    (test guard => expression)
    ```

    where guard and expression must evaluate to procedures.
    For this clause type, test may return multiple values, and
    `cond` ignores its boolean state; instead, `cond` evaluates
    guard and applies the resulting procedure to the value(s) of
    test, as if guard were the consumer argument of
    `call-with-values`. If the result of that procedure call is a
    true value, it evaluates expression and applies the resulting
    procedure to the value(s) of test, in the same manner as the
    guard was called.

    The test of the last clause may be the symbol `else`.
    Then, if none of the preceding tests is true, the
    body following the `else` is evaluated to produce the
    result of the `cond`-expression.

syntax: **case** key clause1 clause2 … [¶](#index-case-1)
:   key may be any expression, and the clauses must have the form

    ```
    ((datum1 ...) body)
    ```

    or

    ```
    ((datum1 ...) => expression)
    ```

    and the last clause may have the form

    ```
    (else body)
    ```

    or

    ```
    (else => expression)
    ```

    All datums must be distinct. First, key is evaluated. The
    result of this evaluation is compared against all datum values
    using `eqv?`. When this comparison succeeds, the body
    following the datum is evaluated to produce the result of the
    `case` expression.

    If the key matches no datum and there is an
    `else`-clause, the body following the `else` is
    evaluated to produce the result of the `case` expression. If there
    is no such clause, the result of the expression is unspecified.

    For the `=>` clause types, expression is evaluated and the
    resulting procedure is applied to the value of key. The result of
    this procedure application is then the result of the
    `case`-expression.

---

Next: [Iteration mechanisms](#while-do), Previous: [Simple Conditional Evaluation](#Conditionals), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.3 Conditional Evaluation of a Sequence of Expressions [¶](#Conditional-Evaluation-of-a-Sequence-of-Expressions)

`and` and `or` evaluate all their arguments in order, similar
to `begin`, but evaluation stops as soon as one of the expressions
evaluates to false or true, respectively.

syntax: **and** expr … [¶](#index-and)
:   Evaluate the exprs from left to right and stop evaluation as soon
    as one expression evaluates to `#f`; the remaining expressions are
    not evaluated. The value of the last evaluated expression is returned.
    If no expression evaluates to `#f`, the value of the last
    expression is returned.

    If used without expressions, `#t` is returned.

syntax: **or** expr … [¶](#index-or)
:   Evaluate the exprs from left to right and stop evaluation as soon
    as one expression evaluates to a true value (that is, a value different
    from `#f`); the remaining expressions are not evaluated. The value
    of the last evaluated expression is returned. If all expressions
    evaluate to `#f`, `#f` is returned.

    If used without expressions, `#f` is returned.

---

Next: [Prompts](#Prompts), Previous: [Conditional Evaluation of a Sequence of Expressions](#and-or), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.4 Iteration mechanisms [¶](#Iteration-mechanisms)

Scheme has only few iteration mechanisms, mainly because iteration in
Scheme programs is normally expressed using recursion. Nevertheless,
R5RS defines a construct for programming loops, calling `do`. In
addition, Guile has an explicit looping syntax called `while`.

syntax: **do** ((variable init [step]) …) (test expr …) body … [¶](#index-do)
:   Bind variables and evaluate body until test is true.
    The return value is the last expr after test, if given. A
    simple example will illustrate the basic form,

    ```
    (do ((i 1 (1+ i)))
        ((> i 4))
      (display i))
    ⊣ 1234
    ```

    Or with two variables and a final return value,

    ```
    (do ((i 1 (1+ i))
         (p 3 (* 3 p)))
        ((> i 4)
         p)
      (format #t "3**~s is ~s\n" i p))
    ⊣
    3**1 is 3
    3**2 is 9
    3**3 is 27
    3**4 is 81
    ⇒
    243
    ```

    The variable bindings are established like a `let`, in that
    the expressions are all evaluated and then all bindings made. When
    iterating, the optional step expressions are evaluated with the
    previous bindings in scope, then new bindings all made.

    The test expression is a termination condition. Looping stops
    when the test is true. It’s evaluated before running the
    body each time, so if it’s true the first time then body
    is not run at all.

    The optional exprs after the test are evaluated at the end
    of looping, with the final variable bindings available. The
    last expr gives the return value, or if there are no exprs
    the return value is unspecified.

    Each iteration establishes bindings to fresh locations for the
    variables, like a new `let` for each iteration. This is
    done for variables without step expressions too. The
    following illustrates this, showing how a new `i` is captured by
    the `lambda` in each iteration (see [The
    Concept of Closure](#About-Closure)).

    ```
    (define lst '())
    (do ((i 1 (1+ i)))
        ((> i 4))
      (set! lst (cons (lambda () i) lst)))
    (map (lambda (proc) (proc)) lst)
    ⇒
    (4 3 2 1)
    ```

syntax: **while** cond body … [¶](#index-while)
:   Run a loop executing the body forms while cond is true.
    cond is tested at the start of each iteration, so if it’s
    `#f` the first time then body is not executed at all.

    Within `while`, two extra bindings are provided, they can be used
    from both cond and body.

    Scheme Procedure: **break** break-arg … [¶](#index-break-1)
    :   Break out of the `while` form.

    Scheme Procedure: **continue** [¶](#index-continue)
    :   Abandon the current iteration, go back to the start and test
        cond again, etc.

    If the loop terminates normally, by the cond evaluating to
    `#f`, then the `while` expression as a whole evaluates to
    `#f`. If it terminates by a call to `break` with some number
    of arguments, those arguments are returned from the `while`
    expression, as multiple values. Otherwise if it terminates by a call to
    `break` with no arguments, then return value is `#t`.

    ```
    (while #f (error "not reached")) ⇒ #f
    (while #t (break)) ⇒ #t
    (while #t (break 1 2 3)) ⇒ 1 2 3
    ```

    Each `while` form gets its own `break` and `continue`
    procedures, operating on that `while`. This means when loops are
    nested the outer `break` can be used to escape all the way out.
    For example,

    ```
    (while (test1)
      (let ((outer-break break))
        (while (test2)
          (if (something)
            (outer-break #f))
          ...)))
    ```

    Note that each `break` and `continue` procedure can only be
    used within the dynamic extent of its `while`. Outside the
    `while` their behavior is unspecified.

Another very common way of expressing iteration in Scheme programs is
the use of the so-called *named let*.

Named let is a variant of `let` which creates a procedure and calls
it in one step. Because of the newly created procedure, named let is
more powerful than `do`–it can be used for iteration, but also
for arbitrary recursion.

syntax: **let** variable bindings body [¶](#index-let-1)
:   For the definition of bindings see the documentation about
    `let` (see [Local Variable Bindings](#Local-Bindings)).

    Named `let` works as follows:

    * A new procedure which accepts as many arguments as are in bindings
      is created and bound locally (using `let`) to variable. The
      new procedure’s formal argument names are the name of the
      variables.
    * The body expressions are inserted into the newly created procedure.
    * The procedure is called with the init expressions as the formal
      arguments.

    The next example implements a loop which iterates (by recursion) 1000
    times.

    ```
    (let lp ((x 1000))
      (if (positive? x)
          (lp (- x 1))
          x))
    ⇒
    0
    ```

---

Next: [Continuations](#Continuations), Previous: [Iteration mechanisms](#while-do), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.5 Prompts [¶](#Prompts-1)

Prompts are control-flow barriers between different parts of a program. In the
same way that a user sees a shell prompt (e.g., the Bash prompt) as a barrier
between the operating system and her programs, Scheme prompts allow the Scheme
programmer to treat parts of programs as if they were running in different
operating systems.

We use this roundabout explanation because, unless you’re a functional
programming junkie, you probably haven’t heard the term, “delimited, composable
continuation”. That’s OK; it’s a relatively recent topic, but a very useful
one to know about.

* [Prompt Primitives](#Prompt-Primitives)
* [Shift, Reset, and All That](#Shift-and-Reset)

---

Next: [Shift, Reset, and All That](#Shift-and-Reset), Up: [Prompts](#Prompts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.5.1 Prompt Primitives [¶](#Prompt-Primitives-1)

Guile’s primitive delimited control operators are
`call-with-prompt` and `abort-to-prompt`.

Scheme Procedure: **call-with-prompt** tag thunk handler [¶](#index-call_002dwith_002dprompt)
:   Set up a prompt, and call thunk within that prompt.

    During the dynamic extent of the call to thunk, a prompt named tag
    will be present in the dynamic context, such that if a user calls
    `abort-to-prompt` (see below) with that tag, control rewinds back to the
    prompt, and the handler is run.

    handler must be a procedure. The first argument to handler will be
    the state of the computation begun when thunk was called, and ending with
    the call to `abort-to-prompt`. The remaining arguments to handler are
    those passed to `abort-to-prompt`.

Scheme Procedure: **make-prompt-tag** [stem] [¶](#index-make_002dprompt_002dtag)
:   Make a new prompt tag. A prompt tag is simply a unique object.
    Currently, a prompt tag is a fresh pair. This may change in some future
    Guile version.

Scheme Procedure: **default-prompt-tag** [¶](#index-default_002dprompt_002dtag)
:   Return the default prompt tag. Having a distinguished default prompt
    tag allows some useful prompt and abort idioms, discussed in the next
    section. Note that `default-prompt-tag` is actually a parameter,
    and so may be dynamically rebound using `parameterize`.
    See [Parameters](#Parameters).

Scheme Procedure: **abort-to-prompt** tag val1 val2 … [¶](#index-abort_002dto_002dprompt)
:   Unwind the dynamic and control context to the nearest prompt named tag,
    also passing the given values.

C programmers may recognize `call-with-prompt` and
`abort-to-prompt` as a fancy kind of `setjmp` and
`longjmp`, respectively. Prompts are indeed quite useful as
non-local escape mechanisms. Guile’s `with-exception-handler` and
`raise-exception` are implemented in terms of prompts. Prompts are
more convenient than `longjmp`, in that one has the opportunity to
pass multiple values to the jump target.

Also unlike `longjmp`, the prompt handler is given the full state of the
process that was aborted, as the first argument to the prompt’s handler. That
state is the *continuation* of the computation wrapped by the prompt. It is
a *delimited continuation*, because it is not the whole continuation of the
program; rather, just the computation initiated by the call to
`call-with-prompt`.

The continuation is a procedure, and may be reinstated simply by invoking it,
with any number of values. Here’s where things get interesting, and complicated
as well. Besides being described as delimited, continuations reified by prompts
are also *composable*, because invoking a prompt-saved continuation composes
that continuation with the current one.

Imagine you have saved a continuation via call-with-prompt:

```
(define cont
  (call-with-prompt
   ;; tag
   'foo
   ;; thunk
   (lambda ()
     (+ 34 (abort-to-prompt 'foo)))
   ;; handler
   (lambda (k) k)))
```

The resulting continuation is the addition of 34. It’s as if you had written:

```
(define cont
  (lambda (x)
    (+ 34 x)))
```

So, if we call `cont` with one numeric value, we get that number,
incremented by 34:

```
(cont 8)
⇒ 42
(* 2 (cont 8))
⇒ 84
```

The last example illustrates what we mean when we say, "composes with the
current continuation". We mean that there is a current continuation – some
remaining things to compute, like `(lambda (x) (* x 2))` – and that
calling the saved continuation doesn’t wipe out the current continuation, it
composes the saved continuation with the current one.

We’re belaboring the point here because traditional Scheme continuations, as
discussed in the next section, aren’t composable, and are actually less
expressive than continuations captured by prompts. But there’s a place for them
both.

Before moving on, we should mention that if the handler of a prompt is a
`lambda` expression, and the first argument isn’t referenced, an abort to
that prompt will not cause a continuation to be reified. This can be an
important efficiency consideration to keep in mind.

One example where this optimization matters is *escape
continuations*. Escape continuations are delimited continuations whose
only use is to make a non-local exit—i.e., to escape from the current
continuation. A common use of escape continuations is when handling an
exception (see [Exceptions](#Exceptions)).

The constructs below are syntactic sugar atop prompts to simplify the
use of escape continuations.

Scheme Procedure: **call-with-escape-continuation** proc [¶](#index-call_002dwith_002descape_002dcontinuation)

Scheme Procedure: **call/ec** proc [¶](#index-call_002fec)
:   Call proc with an escape continuation.

    In the example below, the return continuation is used to escape
    the continuation of the call to `fold`.

    ```
    (use-modules (ice-9 control)
                 (srfi srfi-1))

    (define (prefix x lst)
      ;; Return all the elements before the first occurrence
      ;; of X in LST.
      (call/ec
        (lambda (return)
          (fold (lambda (element prefix)
                  (if (equal? element x)
                      (return (reverse prefix))  ; escape `fold'
                      (cons element prefix)))
                '()
                lst))))

    (prefix 'a '(0 1 2 a 3 4 5))
    ⇒ (0 1 2)
    ```

Scheme Syntax: **let-escape-continuation** k body … [¶](#index-let_002descape_002dcontinuation)

Scheme Syntax: **let/ec** k body … [¶](#index-let_002fec)
:   Bind k within body to an escape continuation.

    This is equivalent to
    `(call/ec (lambda (k) body …))`.

Additionally there is another helper primitive exported by `(ice-9
control)`, so load up that module for `suspendable-continuation?`:

```
(use-modules (ice-9 control))
```

Scheme Procedure: **suspendable-continuation?** tag [¶](#index-suspendable_002dcontinuation_003f)
:   Return `#t` if a call to `abort-to-prompt` with the prompt tag
    tag would produce a delimited continuation that could be resumed
    later.

    Almost all continuations have this property. The exception is where
    some code between the `call-with-prompt` and the
    `abort-to-prompt` recursed through C for some reason, the
    `abort-to-prompt` will succeed but any attempt to resume the
    continuation (by calling it) would fail. This is because composing a
    saved continuation with the current continuation involves relocating the
    stack frames that were saved from the old stack onto a (possibly) new
    position on the new stack, and Guile can only do this for stack frames
    that it created for Scheme code, not stack frames created by the C
    compiler. It’s a bit gnarly but if you stick with Scheme, you won’t
    have any problem.

    If no prompt is found with the given tag, this procedure just returns
    `#f`.

---

Previous: [Prompt Primitives](#Prompt-Primitives), Up: [Prompts](#Prompts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.5.2 Shift, Reset, and All That [¶](#Shift_002c-Reset_002c-and-All-That)

There is a whole zoo of delimited control operators, and as it does not
seem to be a bounded set, Guile implements support for them in a
separate module:

```
(use-modules (ice-9 control))
```

Firstly, we have a helpful abbreviation for the `call-with-prompt`
operator.

Scheme Syntax: **%** expr [¶](#index-_0025)

Scheme Syntax: **%** expr handler [¶](#index-_0025-1)

Scheme Syntax: **%** tag expr handler [¶](#index-_0025-2)
:   Evaluate expr in a prompt, optionally specifying a tag and a
    handler. If no tag is given, the default prompt tag is used.

    If no handler is given, a default handler is installed. The default
    handler accepts a procedure of one argument, which will be called on
    the captured continuation, within a prompt.

    Sometimes it’s easier just to show code, as in this case:

    ```
    (define (default-prompt-handler k proc)
      (% (default-prompt-tag)
         (proc k)
         default-prompt-handler))
    ```

    The `%` symbol is chosen because it looks like a prompt.

Likewise there is an abbreviation for `abort-to-prompt`, which
assumes the default prompt tag:

Scheme Procedure: **abort** val1 val2 … [¶](#index-abort)
:   Abort to the default prompt tag, passing val1 val2 …
    to the handler.

As mentioned before, `(ice-9 control)` also provides other
delimited control operators. This section is a bit technical, and
first-time users of delimited continuations should probably come back to
it after some practice with `%`.

Still here? So, when one implements a delimited control operator like
`call-with-prompt`, one needs to make two decisions. Firstly, does
the handler run within or outside the prompt? Having the handler run
within the prompt allows an abort inside the handler to return to the
same prompt handler, which is often useful. However it prevents tail
calls from the handler, so it is less general.

Similarly, does invoking a captured continuation reinstate a prompt?
Again we have the tradeoff of convenience versus proper tail calls.

These decisions are captured in the Felleisen *F* operator. If
neither the continuations nor the handlers implicitly add a prompt, the
operator is known as *–F–*. This is the case for Guile’s
`call-with-prompt` and `abort-to-prompt`.

If both continuation and handler implicitly add prompts, then the
operator is *+F+*. `shift` and `reset` are such
operators.

Scheme Syntax: **reset** body1 body2 … [¶](#index-reset)
:   Establish a prompt, and evaluate body1 body2 … within
    that prompt.

    The prompt handler is designed to work with `shift`, described
    below.

Scheme Syntax: **shift** cont body1 body2 … [¶](#index-shift)
:   Abort to the nearest `reset`, and evaluate body1 body2
    … in a context in which the captured continuation is bound to
    cont.

    As mentioned above, taken together, the body1 body2 …
    expressions and the invocations of cont implicitly establish a
    prompt.

Interested readers are invited to explore Oleg Kiselyov’s wonderful web
site at <http://okmij.org/ftp/>, for more information on these
operators.

---

Next: [Returning and Accepting Multiple Values](#Multiple-Values), Previous: [Prompts](#Prompts), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.6 Continuations [¶](#Continuations-1)

A “continuation” is the code that will execute when a given function
or expression returns. For example, consider

```
(define (foo)
  (display "hello\n")
  (display (bar)) (newline)
  (exit))
```

The continuation from the call to `bar` comprises a
`display` of the value returned, a `newline` and an
`exit`. This can be expressed as a function of one argument.

```
(lambda (r)
  (display r) (newline)
  (exit))
```

In Scheme, continuations are represented as special procedures just
like this. The special property is that when a continuation is called
it abandons the current program location and jumps directly to that
represented by the continuation.

A continuation is like a dynamic label, capturing at run-time a point
in program execution, including all the nested calls that have lead to
it (or rather the code that will execute when those calls return).

Continuations are created with the following functions.

Scheme Procedure: **call-with-current-continuation** proc [¶](#index-call_002dwith_002dcurrent_002dcontinuation)

Scheme Procedure: **call/cc** proc [¶](#index-call_002fcc)
:   Capture the current continuation and call `(proc
    cont)` with it. The return value is the value returned by
    proc, or when `(cont value)` is later invoked,
    the return is the value passed.

    Normally cont should be called with one argument, but when the
    location resumed is expecting multiple values (see [Returning and Accepting Multiple Values](#Multiple-Values)) then they should be passed as multiple arguments, for
    instance `(cont x y z)`.

    cont may only be used from the same side of a continuation
    barrier as it was created (see [Continuation Barriers](#Continuation-Barriers)), and in a
    multi-threaded program only from the thread in which it was created.

    The call to proc is not part of the continuation captured, it runs
    only when the continuation is created. Often a program will want to
    store cont somewhere for later use; this can be done in
    proc.

    The `call` in the name `call-with-current-continuation`
    refers to the way a call to proc gives the newly created
    continuation. It’s not related to the way a call is used later to
    invoke that continuation.

    `call/cc` is an alias for `call-with-current-continuation`.
    This is in common use since the latter is rather long.

  

Here is a simple example,

```
(define kont #f)
(format #t "the return is ~a\n"
        (call/cc (lambda (k)
                   (set! kont k)
                   1)))
⇒ the return is 1

(kont 2)
⇒ the return is 2
```

`call/cc` captures a continuation in which the value returned is
going to be displayed by `format`. The `lambda` stores this
in `kont` and gives an initial return `1` which is
displayed. The later invocation of `kont` resumes the captured
point, but this time returning `2`, which is displayed.

When Guile is run interactively, a call to `format` like this has
an implicit return back to the read-eval-print loop. `call/cc`
captures that like any other return, which is why interactively
`kont` will come back to read more input.

  

C programmers may note that `call/cc` is like `setjmp` in
the way it records at runtime a point in program execution. A call to
a continuation is like a `longjmp` in that it abandons the
present location and goes to the recorded one. Like `longjmp`,
the value passed to the continuation is the value returned by
`call/cc` on resuming there. However `longjmp` can only go
up the program stack, but the continuation mechanism can go anywhere.

When a continuation is invoked, `call/cc` and subsequent code
effectively “returns” a second time. It can be confusing to imagine
a function returning more times than it was called. It may help
instead to think of it being stealthily re-entered and then program
flow going on as normal.

`dynamic-wind` (see [Dynamic Wind](#Dynamic-Wind)) can be used to ensure setup
and cleanup code is run when a program locus is resumed or abandoned
through the continuation mechanism.

  

Continuations are a powerful mechanism, and can be used to implement
almost any sort of control structure, such as loops, coroutines, or
exception handlers.

However the implementation of continuations in Guile is not as
efficient as one might hope, because Guile is designed to cooperate
with programs written in other languages, such as C, which do not know
about continuations. Basically continuations are captured by a block
copy of the stack, and resumed by copying back.

For this reason, continuations captured by `call/cc` should be used only
when there is no other simple way to achieve the desired result, or when the
elegance of the continuation mechanism outweighs the need for performance.

Escapes upwards from loops or nested functions are generally best
handled with prompts (see [Prompts](#Prompts)). Coroutines can be
efficiently implemented with cooperating threads (a thread holds a
full program stack but doesn’t copy it around the way continuations
do).

---

Next: [Exceptions](#Exceptions), Previous: [Continuations](#Continuations), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.7 Returning and Accepting Multiple Values [¶](#Returning-and-Accepting-Multiple-Values)

Scheme allows a procedure to return more than one value to its caller.
This is quite different to other languages which only allow
single-value returns. Returning multiple values is different from
returning a list (or pair or vector) of values to the caller, because
conceptually not *one* compound object is returned, but several
distinct values.

The primitive procedures for handling multiple values are `values`
and `call-with-values`. `values` is used for returning
multiple values from a procedure. This is done by placing a call to
`values` with zero or more arguments in tail position in a
procedure body. `call-with-values` combines a procedure returning
multiple values with a procedure which accepts these values as
parameters.

Scheme Procedure: **values** arg … [¶](#index-values)

C Function: **scm\_values** (args) [¶](#index-scm_005fvalues)
:   Delivers all of its arguments to its continuation. Except for
    continuations created by the `call-with-values` procedure,
    all continuations take exactly one value. The effect of
    passing no value or more than one value to continuations that
    were not created by `call-with-values` is unspecified.

    For `scm_values`, args is a list of arguments and the
    return is a multiple-values object which the caller can return. In
    the current implementation that object shares structure with
    args, so args should not be modified subsequently.

C Function: `SCM` **scm\_c\_values** `(SCM *base, size_t n)` [¶](#index-scm_005fc_005fvalues)
:   `scm_c_values` is an alternative to `scm_values`. It creates
    a new values object, and copies into it the n values starting from
    base.

    Currently this creates a list and passes it to `scm_values`, but we
    expect that in the future we will be able to use a more efficient
    representation.

C Function: `size_t` **scm\_c\_nvalues** `(SCM obj)` [¶](#index-scm_005fc_005fnvalues)
:   If obj is a multiple-values object, returns the number of values
    it contains. Otherwise returns 1.

C Function: `SCM` **scm\_c\_value\_ref** `(SCM obj, size_t idx)` [¶](#index-scm_005fc_005fvalue_005fref)
:   Returns the value at the position specified by idx in
    obj. Note that obj will ordinarily be a
    multiple-values object, but it need not be. Any other object
    represents a single value (itself), and is handled appropriately.

Scheme Procedure: **call-with-values** producer consumer [¶](#index-call_002dwith_002dvalues)
:   Calls its producer argument with no values and a
    continuation that, when passed some values, calls the
    consumer procedure with those values as arguments. The
    continuation for the call to consumer is the continuation
    of the call to `call-with-values`.

    ```
    (call-with-values (lambda () (values 4 5))
                      (lambda (a b) b))
    ⇒ 5
    ```

    ```
    (call-with-values * -)
    ⇒ -1
    ```

In addition to the fundamental procedures described above, Guile has a
module which exports a syntax called `receive`, which is much
more convenient. This is in the `(ice-9 receive)` and is the
same as specified by SRFI-8 (see [SRFI-8 - receive](#SRFI_002d8)).

```
(use-modules (ice-9 receive))
```

library syntax: **receive** formals expr body [¶](#index-receive-1)
:   Evaluate the expression expr, and bind the result values (zero
    or more) to the formal arguments in formals. formals is a
    list of symbols, like the argument list in a `lambda`
    (see [Lambda: Basic Procedure Creation](#Lambda)). After binding the variables, the body is
    evaluated to produce the result of the `receive` expression.

    For example getting results from `partition` in SRFI-1
    (see [SRFI-1 - List library](#SRFI_002d1)),

    ```
    (receive (odds evens)
        (partition odd? '(7 4 2 8 3))
      (display odds)
      (display " and ")
      (display evens))
    ⊣ (7 3) and (4 2 8)
    ```

---

Next: [Procedures for Signaling Errors](#Error-Reporting), Previous: [Returning and Accepting Multiple Values](#Multiple-Values), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.8 Exceptions [¶](#Exceptions-1)

What happens when things go wrong? Guile’s exception facility exists to
help answer this question, allowing programs to describe the problem and
to handle the situation in a flexible way.

When a program runs into a problem, such as division by zero, it will
raise an exception. Sometimes exceptions get raised by Guile on a
program’s behalf. Sometimes a program will want to raise exceptions of
its own. Raising an exception stops the current computation and instead
invokes the current exception handler, passing it an exception object
describing the unexpected situation.

Usually an exception handler will unwind the computation back to some
kind of safe point. For example, typical logic for a key press driven
application might look something like this:

```
main-loop:
  read the next key press and call dispatch-key

dispatch-key:
  lookup the key in a keymap and call an appropriate procedure,
  say find-file

find-file:
  interactively read the required file name, then call
  find-specified-file

find-specified-file:
  check whether file exists; if not, raise an exception
  ...
```

In this case, `main-loop` can install an exception handler that
would cause any exception raised inside `dispatch-key` to print a
warning and jump back to the main loop.

The following subsections go into more detail about exception objects,
raising exceptions, and handling exceptions. It also presents a
historical interface that was used in Guile’s first 25 years and which
won’t be going away any time soon.

* [Exception Objects](#Exception-Objects)
* [Raising and Handling Exceptions](#Raising-and-Handling-Exceptions)
* [Throw and Catch](#Throw-and-Catch)
* [Exceptions and C](#Exceptions-and-C)

---

Next: [Raising and Handling Exceptions](#Raising-and-Handling-Exceptions), Up: [Exceptions](#Exceptions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.8.1 Exception Objects [¶](#Exception-Objects-1)

When Guile encounters an exceptional situation, it raises an exception,
where the exception is an object that describes the exceptional
situation. Exception objects are structured data, built on the record
facility (see [Records](#Records)).

Exception Type: **&exception** [¶](#index-_0026exception)
:   The base exception type. All exception objects are composed of
    instances of subtypes of `&exception`.

Scheme Procedure: **exception-type?** obj [¶](#index-exception_002dtype_003f)
:   Return true if obj is an exception type.

Exception types exist in a hierarchy. New exception types can be
defined using `make-exception-type`.

Scheme Procedure: **make-exception-type** id parent field-names [¶](#index-make_002dexception_002dtype)
:   Return a new exception type named id, inheriting from
    parent, and with the fields whose names are listed in
    field-names. field-names must be a list of symbols and must
    not contain names already used by parent or one of its supertypes.

Exception type objects are record type objects, and as such, one can use
`record-constructor` on an exception type to get its constructor.
The constructor will take as many arguments as the exception has fields
(including supertypes). See [Records](#Records).

However, `record-predicate` and `record-accessor` aren’t
usually what you want to use as exception type predicates and field
accessors. The reason is, instances of exception types can be composed
into *compound exceptions*. Exception accessors should pick out the
specific component of a compound exception, and then access the field on
that specific component.

Scheme Procedure: **make-exception** exceptions … [¶](#index-make_002dexception)
:   Return an exception object composed of exceptions.

Scheme Procedure: **exception?** obj [¶](#index-exception_003f)
:   Return true if obj is an exception object.

Scheme Procedure: **exception-predicate** type [¶](#index-exception_002dpredicate)
:   Return a procedure that will return true if its argument is a simple
    exception that is an instance of type, or a compound exception
    composed of such an instance.

Scheme Procedure: **exception-accessor** rtd proc [¶](#index-exception_002daccessor)
:   Return a procedure that will tail-call proc on an instance of the
    exception type rtd, or on the component of a compound exception
    that is an instance of rtd.

Compound exceptions are useful to separately express the different
aspects of a situation. For example, compound exceptions allow a
programmer to say that “this situation is a programming error, and also
here’s a useful message to show to the user, and here are some relevant
objects that can give more information about the error”. This error
could be composed of instances of the `&programming-error`,
`&message`, and `&irritants` exception types.

The subtyping relationship in exceptions is useful to let
different-but-similar situations to be treated the same; for example
there are many varieties of programming errors (for example,
divide-by-zero or type mismatches), but perhaps there are common ways
that the user would like to handle them all, and that common way might
be different than how one might handle an error originating outside the
program (for example, a file-not-found error).

The standard exception hierarchy in Guile takes its cues from R6RS,
though the names of some of the types are different. See [rnrs exceptions](#rnrs-exceptions), for more details.

To have access to Guile’s exception type hierarchy, import the
`(ice-9 exceptions)` module:

```
(use-modules (ice-9 exceptions))
```

The following diagram gives an overview of the standard exception type
hierarchy.

```
&exception
|- &warning
|- &message
|- &irritants
|- &origin
\- &error
   |- &external-error
   \- &programming-error
      |- &assertion-failure
      |- &non-continuable
      |- &implementation-restriction
      |- &lexical
      |- &syntax
      \- &undefined-variable
```

Exception Type: **&warning** [¶](#index-_0026warning-1)
:   An exception type denoting warnings. These are usually raised using
    `#:continuable? #t`; see the `raise-exception` documentation
    for more.

Scheme Procedure: **make-warning** [¶](#index-make_002dwarning)

Scheme Procedure: **warning?** obj [¶](#index-warning_003f)
:   Constructor and predicate for `&warning` exception objects.

Exception Type: **&message** message [¶](#index-_0026message-1)
:   An exception type that provides a message to display to the user.
    Usually used as a component of a compound exception.

Scheme Procedure: **make-exception-with-message** message [¶](#index-make_002dexception_002dwith_002dmessage)

Scheme Procedure: **exception-with-message?** obj [¶](#index-exception_002dwith_002dmessage_003f)

Scheme Procedure: **exception-message** exn [¶](#index-exception_002dmessage)
:   Constructor, predicate, and accessor for `&message` exception
    objects.

Exception Type: **&irritants** irritants [¶](#index-_0026irritants-1)
:   An exception type that provides a list of objects that were unexpected
    in some way. Usually used as a component of a compound exception.

Scheme Procedure: **make-exception-with-irritants** irritants [¶](#index-make_002dexception_002dwith_002dirritants)

Scheme Procedure: **exception-with-irritants?** obj [¶](#index-exception_002dwith_002dirritants_003f)

Scheme Procedure: **exception-irritants** exn [¶](#index-exception_002dirritants)
:   Constructor, predicate, and accessor for `&irritants` exception
    objects.

Exception Type: **&origin** origin [¶](#index-_0026origin)
:   An exception type that indicates the origin of an exception, typically
    expressed as a procedure name, as a symbol. Usually used as a component
    of a compound exception.

Scheme Procedure: **make-exception-with-origin** origin [¶](#index-make_002dexception_002dwith_002dorigin)

Scheme Procedure: **exception-with-origin?** obj [¶](#index-exception_002dwith_002dorigin_003f)

Scheme Procedure: **exception-origin** exn [¶](#index-exception_002dorigin)
:   Constructor, predicate, and accessor for `&origin` exception
    objects.

Exception Type: **&error** [¶](#index-_0026error-1)
:   An exception type denoting errors: situations that are not just
    exceptional, but wrong.

Scheme Procedure: **make-error** [¶](#index-make_002derror)

Scheme Procedure: **error?** obj [¶](#index-error_003f)
:   Constructor and predicate for `&error` exception objects.

Exception Type: **&external-error** [¶](#index-_0026external_002derror)
:   An exception type denoting errors that proceed from the interaction of
    the program with the world, for example a “file not found” error.

Scheme Procedure: **make-external-error** [¶](#index-make_002dexternal_002derror)

Scheme Procedure: **external-error?** obj [¶](#index-external_002derror_003f)
:   Constructor and predicate for `&external-error` exception objects.

Exception Type: **&programming-error** [¶](#index-_0026programming_002derror)
:   An exception type denoting errors that proceed from inside a program:
    type mismatches and so on.

Scheme Procedure: **make-programming-error** [¶](#index-make_002dprogramming_002derror)

Scheme Procedure: **programming-error?** obj [¶](#index-programming_002derror_003f)
:   Constructor and predicate for `&programming-error` exception
    objects.

Exception Type: **&non-continuable** [¶](#index-_0026non_002dcontinuable-1)
:   An exception type denoting errors that proceed from inside a program:
    type mismatches and so on.

Scheme Procedure: **make-non-continuable-error** [¶](#index-make_002dnon_002dcontinuable_002derror)

Scheme Procedure: **non-continuable-error?** obj [¶](#index-non_002dcontinuable_002derror_003f)
:   Constructor and predicate for `&non-continuable` exception objects.

Exception Type: **&lexical** [¶](#index-_0026lexical-1)
:   An exception type denoting lexical errors, for example unbalanced
    parentheses.

Scheme Procedure: **make-lexical-error** [¶](#index-make_002dlexical_002derror)

Scheme Procedure: **lexical-error?** obj [¶](#index-lexical_002derror_003f)
:   Constructor and predicate for `&lexical` exception objects.

Exception Type: **&syntax** form subform [¶](#index-_0026syntax-1)
:   An exception type denoting syntax errors, for example a `cond`
    expression with invalid syntax. The form field indicates the form
    containing the error, and subform indicates the unexpected
    subcomponent, or `#f` if unavailable.

Scheme Procedure: **make-syntax-error** form subform [¶](#index-make_002dsyntax_002derror)

Scheme Procedure: **syntax-error?** obj [¶](#index-syntax_002derror_003f)

Scheme Procedure: **syntax-error-form** exn [¶](#index-syntax_002derror_002dform)

Scheme Procedure: **syntax-error-subform** exn [¶](#index-syntax_002derror_002dsubform)
:   Constructor, predicate, and accessors for `&syntax` exception
    objects.

Exception Type: **&undefined-variable** [¶](#index-_0026undefined_002dvariable)
:   An exception type denoting undefined variables.

Scheme Procedure: **make-undefine-variable-error** [¶](#index-make_002dundefine_002dvariable_002derror)

Scheme Procedure: **undefined-variable-error?** obj [¶](#index-undefined_002dvariable_002derror_003f)
:   Constructor and predicate for `&undefined-variable` exception
    objects.

Incidentally, the `(ice-9 exceptions)` module also includes a
`define-exception-type` macro that can be used to conveniently add
new exception types to the hierarchy.

Syntax: **define-exception-type** name parent constructor predicate (field accessor) … [¶](#index-define_002dexception_002dtype)
:   Define name to be a new exception type, inheriting from
    parent. Define constructor and predicate to be the
    exception constructor and predicate, respectively, and define an
    accessor for each field.

---

Next: [Throw and Catch](#Throw-and-Catch), Previous: [Exception Objects](#Exception-Objects), Up: [Exceptions](#Exceptions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.8.2 Raising and Handling Exceptions [¶](#Raising-and-Handling-Exceptions-1)

An exception object describes an exceptional situation. To bring that
description to the attention of the user or to handle the situation
programmatically, the first step is to *raise* the exception.

Scheme Procedure: **raise-exception** obj [#:continuable?=#f] [¶](#index-raise_002dexception)
:   Raise an exception by invoking the current exception handler on
    obj. The handler is called with a continuation whose dynamic
    environment is that of the call to `raise`, except that the current
    exception handler is the one that was in place when the handler being
    called was installed.

    If continuable? is true, the handler is invoked in tail position
    relative to the `raise-exception` call. Otherwise if the handler
    returns, a non-continuable exception of type `&non-continuable` is
    raised in the same dynamic environment as the handler.

As the above description notes, Guile has a notion of a *current
exception handler*. At the REPL, this exception handler may enter a
recursive debugger; in a standalone program, it may simply print a
representation of the error and exit.

To establish an exception handler within the dynamic extent of a call,
use `with-exception-handler`.

Scheme Procedure: **with-exception-handler** handler thunk [#:unwind?=#f] [#:unwind-for-type=#t] [¶](#index-with_002dexception_002dhandler)
:   Establish handler, a procedure of one argument, as the current
    exception handler during the dynamic extent of invoking thunk.

    If `raise-exception` is called during the dynamic extent of
    invoking thunk, handler will be invoked on the argument of
    `raise-exception`.

There are two kinds of exception handlers: unwinding and non-unwinding.

By default, exception handlers are non-unwinding. Unless
`with-exception-handler` was invoked with `#:unwind? #t`,
exception handlers are invoked within the continuation of the error,
without unwinding the stack. The dynamic environment of the handler
call will be that of the `raise-exception` call, with the
difference that the current exception handler will be “unwound” to the
“outer” handler (the one that was in place when the corresponding
`with-exception-handler` was called).

However, it’s often the case that one would like to handle an exception
by unwinding the computation to an earlier state and running the error
handler there. After all, unless the `raise-exception` call is
continuable, the exception handler needs to abort the continuation. To
support this use case, if `with-exception-handler` was invoked with
`#:unwind? #t` is true, `raise-exception` will first unwind
the stack by invoking an *escape continuation* (see [`call/ec`](#Prompt-Primitives)), and then invoke the handler with the
continuation of the `with-exception-handler` call.

Finally, one more wrinkle: for unwinding exception handlers, it can be
useful to Guile if it can determine whether an exception handler would
indeed handle a particular exception or not. This is especially the
case for exceptions raised in resource-exhaustion scenarios like
`stack-overflow` or `out-of-memory`, where you want to
immediately shrink resource use before recovering. See [Stack Overflow](#Stack-Overflow). For this purpose, the `#:unwind-for-type` keyword
argument allows users to specify the kind of exception handled by an
exception handler; if `#t`, all exceptions will be handled; if an
exception type object, only exceptions of that type will be handled;
otherwise if a symbol, only that exceptions with the given
`exception-kind` will be handled.

---

Next: [Exceptions and C](#Exceptions-and-C), Previous: [Raising and Handling Exceptions](#Raising-and-Handling-Exceptions), Up: [Exceptions](#Exceptions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.8.3 Throw and Catch [¶](#Throw-and-Catch-1)

Guile only adopted `with-exception-handler` and
`raise-exception` as its primary exception-handling facility in
2019. Before then, exception handling was fundamentally based on three
other primitives with a somewhat more complex interface: `catch`,
`with-throw-handler`, and `throw`.

Scheme Procedure: **catch** key thunk handler [pre-unwind-handler] [¶](#index-catch)

C Function: **scm\_catch\_with\_pre\_unwind\_handler** (key, thunk, handler, pre\_unwind\_handler) [¶](#index-scm_005fcatch_005fwith_005fpre_005funwind_005fhandler)

C Function: **scm\_catch** (key, thunk, handler) [¶](#index-scm_005fcatch)
:   Establish an exception handler during the dynamic extent of the call to
    thunk. key is either `#t`, indicating that all
    exceptions should be handled, or a symbol, restricting the exceptions
    handled to those having the key as their `exception-kind`.

    If thunk executes normally, meaning without throwing any
    exceptions, the handler procedures are not called at all and the result
    of the `thunk` call is the result of the `catch`. Otherwise
    if an exception is thrown that matches key, handler is
    called with the continuation of the `catch` call.

Given the discussion from the previous section, it is most precise and
concise to specify what `catch` does by expressing it in terms of
`with-exception-handler`. Calling `catch` with the three
arguments is the same as:

```
(define (catch key thunk handler)
  (with-exception-handler
   (lambda (exn)
     (apply handler (exception-kind exn) (exception-args exn)))
   thunk
   #:unwind? #t
   #:unwind-for-type key))
```

By invoking `with-exception-handler` with `#:unwind? #t`,
`catch` sets up an escape continuation that will be invoked in an
exceptional situation before the handler is called.

If `catch` is called with four arguments, then the use of
thunk should be replaced with:

```
   (lambda ()
     (with-throw-handler key thunk pre-unwind-handler))
```

As can be seen above, if a pre-unwind-handler is passed to `catch`,
it’s like calling `with-throw-handler` inside the body thunk.

`with-throw-handler` is the second of the older primitives, and is
used to be able to intercept an exception that is being thrown before
the stack is unwound. This could be to clean up some related state, to
print a backtrace, or to pass information about the exception to a
debugger, for example.

Scheme Procedure: **with-throw-handler** key thunk handler [¶](#index-with_002dthrow_002dhandler)

C Function: **scm\_with\_throw\_handler** (key, thunk, handler) [¶](#index-scm_005fwith_005fthrow_005fhandler)
:   Add handler to the dynamic context as a throw handler
    for key key, then invoke thunk.

It’s not possible to exactly express `with-throw-handler` in terms
of `with-exception-handler`, but we can get close.

```
(define (with-throw-handler key thunk handler)
  (with-exception-handler
   (lambda (exn)
     (when (or (eq? key #t) (eq? key (exception-kind exn)))
       (apply handler (exception-kind exn) (exception-args exn)))
     (raise-exception exn))
   thunk))
```

As you can see, unlike in the case of `catch`, the handler for
`with-throw-handler` is invoked within the continuation of
`raise-exception`, before unwinding the stack. If the throw
handler returns normally, the exception will be re-raised, to be handled
by the next exception handler.

The special wrinkle of `with-throw-handler` that can’t be shown
above is that if invoking the handler causes a `raise-exception`
instead of completing normally, the exception is thrown in the
*original* dynamic environment of the `raise-exception`. Any
inner exception handler will get another shot at handling the exception.
Here is an example to illustrate this behavior:

```
(catch 'a
  (lambda ()
    (with-throw-handler 'b
      (lambda ()
        (catch 'a
          (lambda ()
            (throw 'b))
          inner-handler))
      (lambda (key . args)
        (throw 'a))))
  outer-handler)
```

This code will call `inner-handler` and then continue with the
continuation of the inner `catch`.

Finally, we get to `throw`, which is the older equivalent to
`raise-exception`.

Scheme Procedure: **throw** key arg … [¶](#index-throw)

C Function: **scm\_throw** (key, args) [¶](#index-scm_005fthrow)
:   Raise an exception with kind key and arguments args.
    key is a symbol, denoting the “kind” of the exception.

Again, we can specify what `throw` does by expressing it in terms
of `raise-exception`.

```
(define (throw key . args)
  (raise-exception (make-exception-from-throw key args)))
```

At this point, we should mention the primitive that manage the
relationship between structured exception objects `throw`.

Scheme Procedure: **make-exception-from-throw** key args [¶](#index-make_002dexception_002dfrom_002dthrow)
:   Create an exception object for the given key and args passed
    to `throw`. This may be a specific type of exception, for example
    `&programming-error`; Guile maintains a set of custom transformers
    for the various key values that have been used historically.

Scheme Procedure: **exception-kind** exn [¶](#index-exception_002dkind)
:   If exn is an exception created via
    `make-exception-from-throw`, return the corresponding key for
    the exception. Otherwise, unless exn is an exception of a type
    with a known mapping to `throw`, return the symbol
    `%exception`.

Scheme Procedure: **exception-args** exn [¶](#index-exception_002dargs)
:   If exn is an exception created via
    `make-exception-from-throw`, return the corresponding args
    for the exception. Otherwise, unless exn is an exception of a
    type with a known mapping to `throw`, return `(list exn)`.

---

Previous: [Throw and Catch](#Throw-and-Catch), Up: [Exceptions](#Exceptions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.8.4 Exceptions and C [¶](#Exceptions-and-C-1)

The primary function to deal with Guile’s exceptions from C is
`scm_c_catch`.

C Function: `SCM` **scm\_c\_catch** `(SCM tag, scm_t_catch_body body, void *body_data, scm_t_catch_handler handler, void *handler_data, scm_t_catch_handler pre_unwind_handler, void *pre_unwind_handler_data)` [¶](#index-scm_005fc_005fcatch)

C Function: `SCM` **scm\_internal\_catch** `(SCM tag, scm_t_catch_body body, void *body_data, scm_t_catch_handler handler, void *handler_data)` [¶](#index-scm_005finternal_005fcatch)
:   `scm_c_catch` can register a C functions as a body thunk and
    exception handler as opposed to `scm_catch_with_pre_unwind_handler`
    and `scm_catch` which take Scheme procedures as body thunk and
    handler arguments. `scm_internal_catch` is retained for backward
    compatibility and is a specialization of `scm_c_catch` with the
    pre-unwind handler and corresponding data set to `NULL`.

    body is called as `body (body_data)` with a catch
    on exceptions of the given tag type. If an exception is caught,
    pre\_unwind\_handler and handler are called as
    `handler (handler_data, key, args)`.
    key and args are the `SCM` key and argument list from
    the `throw`.

    body and handler should have the following prototypes.
    `scm_t_catch_body` and `scm_t_catch_handler` are pointer
    typedefs for these.

    ```
    SCM body (void *data);
    SCM handler (void *data, SCM key, SCM args);
    ```

    The body\_data and handler\_data parameters are passed to
    the respective calls so an application can communicate extra
    information to those functions.

    If the data consists of an `SCM` object, care should be taken that
    it isn’t garbage collected while still required. If the `SCM` is a
    local C variable, one way to protect it is to pass a pointer to that
    variable as the data parameter, since the C compiler will then know the
    value must be held on the stack. Another way is to use
    `scm_remember_upto_here_1` (see [Foreign Object Memory Management](#Foreign-Object-Memory-Management)).

C Function: `SCM` **scm\_c\_with\_throw\_handler** `(SCM tag, scm_t_catch_body body, void *body_data, scm_t_catch_handler handler, void *handler_data, int lazy_catch_p)` [¶](#index-scm_005fc_005fwith_005fthrow_005fhandler)
:   The above `scm_with_throw_handler` takes Scheme procedures as body
    (thunk) and handler arguments. `scm_c_with_throw_handler` is an
    equivalent taking C functions. See `scm_c_catch`
    (see [Exceptions and C](#Exceptions-and-C)) for a description of the parameters, the
    behavior however of course follows `with-throw-handler`.

---

Next: [Dynamic Wind](#Dynamic-Wind), Previous: [Exceptions](#Exceptions), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.9 Procedures for Signaling Errors [¶](#Procedures-for-Signaling-Errors)

Guile provides a set of convenience procedures for signaling error
conditions that are implemented on top of the exception primitives just
described.

Scheme Procedure: **error** msg arg … [¶](#index-error-1)
:   Raise an error with key `misc-error` and a message constructed by
    displaying msg and writing arg ....

Scheme Procedure: **scm-error** key subr message args data [¶](#index-scm_002derror)

C Function: **scm\_error\_scm** (key, subr, message, args, data) [¶](#index-scm_005ferror_005fscm)
:   Raise an error with key key. subr can be a string
    naming the procedure associated with the error, or `#f`.
    message is the error message string, possibly containing
    `~S` and `~A` escapes. When an error is reported,
    these are replaced by formatting the corresponding members of
    args: `~A` (was `%s` in older versions of
    Guile) formats using `display` and `~S` (was
    `%S`) formats using `write`. data is a list or
    `#f` depending on key: if key is
    `system-error` then it should be a list containing the
    Unix `errno` value; If key is `signal` then it
    should be a list containing the Unix signal number; If
    key is `out-of-range`, `wrong-type-arg`,
    or `keyword-argument-error`,
    it is a list containing the bad value; otherwise
    it will usually be `#f`.

Scheme Procedure: **strerror** err [¶](#index-strerror)

C Function: **scm\_strerror** (err) [¶](#index-scm_005fstrerror)
:   Return the Unix error message corresponding to err, an integer
    `errno` value.

    When `setlocale` has been called (see [Locales](#Locales)), the message
    is in the language and charset of `LC_MESSAGES`. (This is done
    by the C library.)

syntax: **false-if-exception** expr [¶](#index-false_002dif_002dexception)
:   Returns the result of evaluating its argument; however
    if an exception occurs then `#f` is returned instead.

---

Next: [Fluids and Dynamic States](#Fluids-and-Dynamic-States), Previous: [Procedures for Signaling Errors](#Error-Reporting), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.10 Dynamic Wind [¶](#Dynamic-Wind-1)

For Scheme code, the fundamental procedure to react to non-local entry
and exits of dynamic contexts is `dynamic-wind`. C code could
use `scm_internal_dynamic_wind`, but since C does not allow the
convenient construction of anonymous procedures that close over
lexical variables, this will be, well, inconvenient.

Therefore, Guile offers the functions `scm_dynwind_begin` and
`scm_dynwind_end` to delimit a dynamic extent. Within this
dynamic extent, which is called a *dynwind context*, you can
perform various *dynwind actions* that control what happens when
the dynwind context is entered or left. For example, you can register
a cleanup routine with `scm_dynwind_unwind_handler` that is
executed when the context is left. There are several other more
specialized dynwind actions as well, for example to temporarily block
the execution of asyncs or to temporarily change the current output
port. They are described elsewhere in this manual.

Here is an example that shows how to prevent memory leaks.

```
/* Suppose there is a function called FOO in some library that you
   would like to make available to Scheme code (or to C code that
   follows the Scheme conventions).

   FOO takes two C strings and returns a new string.  When an error has
   occurred in FOO, it returns NULL.
*/

char *foo (char *s1, char *s2);

/* SCM_FOO interfaces the C function FOO to the Scheme way of life.
   It takes care to free up all temporary strings in the case of
   non-local exits.
 */

SCM
scm_foo (SCM s1, SCM s2)
{
  char *c_s1, *c_s2, *c_res;

  scm_dynwind_begin (0);

  c_s1 = scm_to_locale_string (s1);

  /* Call 'free (c_s1)' when the dynwind context is left.
  */
  scm_dynwind_unwind_handler (free, c_s1, SCM_F_WIND_EXPLICITLY);

  c_s2 = scm_to_locale_string (s2);

  /* Same as above, but more concisely.
  */
  scm_dynwind_free (c_s2);

  c_res = foo (c_s1, c_s2);
  if (c_res == NULL)
    scm_report_out_of_memory ();

  scm_dynwind_end ();

  return scm_take_locale_string (c_res);
}
```

Scheme Procedure: **dynamic-wind** in\_guard thunk out\_guard [¶](#index-dynamic_002dwind)

C Function: **scm\_dynamic\_wind** (in\_guard, thunk, out\_guard) [¶](#index-scm_005fdynamic_005fwind)
:   All three arguments must be 0-argument procedures.
    in\_guard is called, then thunk, then
    out\_guard.

    If, any time during the execution of thunk, the
    dynamic extent of the `dynamic-wind` expression is escaped
    non-locally, out\_guard is called. If the dynamic extent of
    the dynamic-wind is re-entered, in\_guard is called. Thus
    in\_guard and out\_guard may be called any number of
    times.

    ```
    (define x 'normal-binding)
    ⇒ x
    (define a-cont
      (call-with-current-continuation
       (lambda (escape)
         (let ((old-x x))
           (dynamic-wind
               ;; in-guard:
               ;;
               (lambda () (set! x 'special-binding))

               ;; thunk
               ;;
               (lambda () (display x) (newline)
                          (call-with-current-continuation escape)
                          (display x) (newline)
                          x)

               ;; out-guard:
               ;;
               (lambda () (set! x old-x)))))))
    ;; Prints:
    special-binding
    ;; Evaluates to:
    ⇒ a-cont
    x
    ⇒ normal-binding
    (a-cont #f)
    ;; Prints:
    special-binding
    ;; Evaluates to:
    ⇒ a-cont  ;; the value of the (define a-cont...)
    x
    ⇒ normal-binding
    a-cont
    ⇒ special-binding
    ```

C Type: **scm\_t\_dynwind\_flags** [¶](#index-scm_005ft_005fdynwind_005fflags)
:   This is an enumeration of several flags that modify the behavior of
    `scm_dynwind_begin`. The flags are listed in the following
    table.

    `SCM_F_DYNWIND_REWINDABLE`
    :   The dynamic context is *rewindable*. This means that it can be
        reentered non-locally (via the invocation of a continuation). The
        default is that a dynwind context can not be reentered non-locally.

C Function: `void` **scm\_dynwind\_begin** `(scm_t_dynwind_flags flags)` [¶](#index-scm_005fdynwind_005fbegin)
:   The function `scm_dynwind_begin` starts a new dynamic context and
    makes it the ‘current’ one.

    The flags argument determines the default behavior of the
    context. Normally, use 0. This will result in a context that can not
    be reentered with a captured continuation. When you are prepared to
    handle reentries, include `SCM_F_DYNWIND_REWINDABLE` in
    flags.

    Being prepared for reentry means that the effects of unwind handlers
    can be undone on reentry. In the example above, we want to prevent a
    memory leak on non-local exit and thus register an unwind handler that
    frees the memory. But once the memory is freed, we can not get it
    back on reentry. Thus reentry can not be allowed.

    The consequence is that continuations become less useful when
    non-reentrant contexts are captured, but you don’t need to worry
    about that too much.

    The context is ended either implicitly when a non-local exit happens,
    or explicitly with `scm_dynwind_end`. You must make sure that a
    dynwind context is indeed ended properly. If you fail to call
    `scm_dynwind_end` for each `scm_dynwind_begin`, the behavior
    is undefined.

C Function: `void` **scm\_dynwind\_end** `()` [¶](#index-scm_005fdynwind_005fend)
:   End the current dynamic context explicitly and make the previous one
    current.

C Type: **scm\_t\_wind\_flags** [¶](#index-scm_005ft_005fwind_005fflags)
:   This is an enumeration of several flags that modify the behavior of
    `scm_dynwind_unwind_handler` and
    `scm_dynwind_rewind_handler`. The flags are listed in the
    following table.

    `SCM_F_WIND_EXPLICITLY` [¶](#index-SCM_005fF_005fWIND_005fEXPLICITLY)
    :   The registered action is also carried out when the dynwind context is
        entered or left locally.

C Function: `void` **scm\_dynwind\_unwind\_handler** `(void (*func)(void *), void *data, scm_t_wind_flags flags)` [¶](#index-scm_005fdynwind_005funwind_005fhandler)

C Function: `void` **scm\_dynwind\_unwind\_handler\_with\_scm** `(void (*func)(SCM), SCM data, scm_t_wind_flags flags)` [¶](#index-scm_005fdynwind_005funwind_005fhandler_005fwith_005fscm)
:   Arranges for func to be called with data as its arguments
    when the current context ends implicitly. If flags contains
    `SCM_F_WIND_EXPLICITLY`, func is also called when the
    context ends explicitly with `scm_dynwind_end`.

    The function `scm_dynwind_unwind_handler_with_scm` takes care that
    data is protected from garbage collection.

C Function: `void` **scm\_dynwind\_rewind\_handler** `(void (*func)(void *), void *data, scm_t_wind_flags flags)` [¶](#index-scm_005fdynwind_005frewind_005fhandler)

C Function: `void` **scm\_dynwind\_rewind\_handler\_with\_scm** `(void (*func)(SCM), SCM data, scm_t_wind_flags flags)` [¶](#index-scm_005fdynwind_005frewind_005fhandler_005fwith_005fscm)
:   Arrange for func to be called with data as its argument when
    the current context is restarted by rewinding the stack. When flags
    contains `SCM_F_WIND_EXPLICITLY`, func is called immediately
    as well.

    The function `scm_dynwind_rewind_handler_with_scm` takes care that
    data is protected from garbage collection.

C Function: `void` **scm\_dynwind\_free** `(void *mem)` [¶](#index-scm_005fdynwind_005ffree)
:   Arrange for mem to be freed automatically whenever the current
    context is exited, whether normally or non-locally.
    `scm_dynwind_free (mem)` is an equivalent shorthand for
    `scm_dynwind_unwind_handler (free, mem, SCM_F_WIND_EXPLICITLY)`.

---

Next: [Parameters](#Parameters), Previous: [Dynamic Wind](#Dynamic-Wind), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.11 Fluids and Dynamic States [¶](#Fluids-and-Dynamic-States-1)

A *fluid* is a variable whose value is associated with the dynamic
extent of a function call. In the same way that an operating system
runs a process with a given set of current input and output ports (or
file descriptors), in Guile you can arrange to call a function while
binding a fluid to a particular value. That association between fluid
and value will exist during the dynamic extent of the function call.

Fluids are therefore a building block for implementing dynamically
scoped variables. Dynamically scoped variables are useful when you want
to set a variable to a value during some dynamic extent in the execution
of your program and have them revert to their original value when the
control flow is outside of this dynamic extent. See the description of
`with-fluids` below for details. This association between fluids,
values, and dynamic extents is robust to multiple entries (as when a
captured continuation is invoked more than once) and early exits (for
example, when throwing exceptions).

Guile uses fluids to implement parameters (see [Parameters](#Parameters)). Usually
you just want to use parameters directly. However it can be useful to
know what a fluid is and how it works, so that’s what this section is
about.

The current set of fluid-value associations can be captured in a
*dynamic state* object. A dynamic extent is simply that: a
snapshot of the current fluid-value associations. Guile users can
capture the current dynamic state with `current-dynamic-state` and
restore it later via `with-dynamic-state` or similar procedures.
This facility is especially useful when implementing lightweight
thread-like abstractions.

New fluids are created with `make-fluid` and `fluid?` is
used for testing whether an object is actually a fluid. The values
stored in a fluid can be accessed with `fluid-ref` and
`fluid-set!`.

See [Thread-Local Variables](#Thread-Local-Variables), for further notes on fluids, threads,
parameters, and dynamic states.

Scheme Procedure: **make-fluid** [dflt] [¶](#index-make_002dfluid)

C Function: **scm\_make\_fluid** () [¶](#index-scm_005fmake_005ffluid)

C Function: **scm\_make\_fluid\_with\_default** (dflt) [¶](#index-scm_005fmake_005ffluid_005fwith_005fdefault)
:   Return a newly created fluid, whose initial value is dflt, or
    `#f` if dflt is not given.
    Fluids are objects that can hold one
    value per dynamic state. That is, modifications to this value are
    only visible to code that executes with the same dynamic state as
    the modifying code. When a new dynamic state is constructed, it
    inherits the values from its parent. Because each thread normally executes
    with its own dynamic state, you can use fluids for thread local storage.

Scheme Procedure: **make-unbound-fluid** [¶](#index-make_002dunbound_002dfluid)

C Function: **scm\_make\_unbound\_fluid** () [¶](#index-scm_005fmake_005funbound_005ffluid)
:   Return a new fluid that is initially unbound (instead of being
    implicitly bound to some definite value).

Scheme Procedure: **fluid?** obj [¶](#index-fluid_003f)

C Function: **scm\_fluid\_p** (obj) [¶](#index-scm_005ffluid_005fp)
:   Return `#t` if obj is a fluid; otherwise, return
    `#f`.

Scheme Procedure: **fluid-ref** fluid [¶](#index-fluid_002dref)

C Function: **scm\_fluid\_ref** (fluid) [¶](#index-scm_005ffluid_005fref)
:   Return the value associated with fluid in the current
    dynamic root. If fluid has not been set, then return
    its default value. Calling `fluid-ref` on an unbound fluid produces
    a runtime error.

Scheme Procedure: **fluid-set!** fluid value [¶](#index-fluid_002dset_0021)

C Function: **scm\_fluid\_set\_x** (fluid, value) [¶](#index-scm_005ffluid_005fset_005fx)
:   Set the value associated with fluid in the current dynamic root.

Scheme Procedure: **fluid-ref\*** fluid depth [¶](#index-fluid_002dref_002a)

C Function: **scm\_fluid\_ref\_star** (fluid, depth) [¶](#index-scm_005ffluid_005fref_005fstar)
:   Return the depthth oldest value associated with fluid in the
    current thread. If depth equals or exceeds the number of values
    that have been assigned to fluid, return the default value of the
    fluid. `(fluid-ref* f 0)` is equivalent to `(fluid-ref f)`.

    `fluid-ref*` is useful when you want to maintain a stack-like
    structure in a fluid, such as the stack of current exception handlers.
    Using `fluid-ref*` instead of an explicit stack allows any partial
    continuation captured by `call-with-prompt` to only capture the
    bindings made within the limits of the prompt instead of the entire
    continuation. See [Prompts](#Prompts), for more on delimited continuations.

Scheme Procedure: **fluid-unset!** fluid [¶](#index-fluid_002dunset_0021)

C Function: **scm\_fluid\_unset\_x** (fluid) [¶](#index-scm_005ffluid_005funset_005fx)
:   Disassociate the given fluid from any value, making it unbound.

Scheme Procedure: **fluid-bound?** fluid [¶](#index-fluid_002dbound_003f)

C Function: **scm\_fluid\_bound\_p** (fluid) [¶](#index-scm_005ffluid_005fbound_005fp)
:   Returns `#t` if the given fluid is bound to a value, otherwise
    `#f`.

`with-fluids*` temporarily changes the values of one or more fluids,
so that the given procedure and each procedure called by it access the
given values. After the procedure returns, the old values are restored.

Scheme Procedure: **with-fluid\*** fluid value thunk [¶](#index-with_002dfluid_002a)

C Function: **scm\_with\_fluid** (fluid, value, thunk) [¶](#index-scm_005fwith_005ffluid)
:   Set fluid to value temporarily, and call thunk.
    thunk must be a procedure with no argument.

Scheme Procedure: **with-fluids\*** fluids values thunk [¶](#index-with_002dfluids_002a)

C Function: **scm\_with\_fluids** (fluids, values, thunk) [¶](#index-scm_005fwith_005ffluids)
:   Set fluids to values temporary, and call thunk.
    fluids must be a list of fluids and values must be the
    same number of their values to be applied. Each substitution is done
    in the order given. thunk must be a procedure with no argument.
    It is called inside a `dynamic-wind` and the fluids are
    set/restored when control enter or leaves the established dynamic
    extent.

Scheme Macro: **with-fluids** ((fluid value) …) body [¶](#index-with_002dfluids)
:   Execute body (see [Local Variable Bindings](#Local-Bindings)) while each fluid is
    set to the corresponding value. Both fluid and value
    are evaluated and fluid must yield a fluid. The body is executed
    inside a `dynamic-wind` and the fluids are set/restored when
    control enter or leaves the established dynamic extent.

C Function: `SCM` **scm\_c\_with\_fluids** `(SCM fluids, SCM vals, SCM (*cproc)(void *), void *data)` [¶](#index-scm_005fc_005fwith_005ffluids)

C Function: `SCM` **scm\_c\_with\_fluid** `(SCM fluid, SCM val, SCM (*cproc)(void *), void *data)` [¶](#index-scm_005fc_005fwith_005ffluid)
:   The function `scm_c_with_fluids` is like `scm_with_fluids`
    except that it takes a C function to call instead of a Scheme thunk.

    The function `scm_c_with_fluid` is similar but only allows one
    fluid to be set instead of a list.

C Function: `void` **scm\_dynwind\_fluid** `(SCM fluid, SCM val)` [¶](#index-scm_005fdynwind_005ffluid)
:   This function must be used inside a pair of calls to
    `scm_dynwind_begin` and `scm_dynwind_end` (see [Dynamic Wind](#Dynamic-Wind)). During the dynwind context, the fluid fluid is set to
    val.

    More precisely, the value of the fluid is swapped with a ‘backup’
    value whenever the dynwind context is entered or left. The backup
    value is initialized with the val argument.

Scheme Procedure: **dynamic-state?** obj [¶](#index-dynamic_002dstate_003f)

C Function: **scm\_dynamic\_state\_p** (obj) [¶](#index-scm_005fdynamic_005fstate_005fp)
:   Return `#t` if obj is a dynamic state object;
    return `#f` otherwise.

C Procedure: `int` **scm\_is\_dynamic\_state** `(SCM obj)` [¶](#index-scm_005fis_005fdynamic_005fstate)
:   Return non-zero if obj is a dynamic state object;
    return zero otherwise.

Scheme Procedure: **current-dynamic-state** [¶](#index-current_002ddynamic_002dstate)

C Function: **scm\_current\_dynamic\_state** () [¶](#index-scm_005fcurrent_005fdynamic_005fstate)
:   Return a snapshot of the current fluid-value associations as a fresh
    dynamic state object.

Scheme Procedure: **set-current-dynamic-state** state [¶](#index-set_002dcurrent_002ddynamic_002dstate)
