### 11.2 Conditionals [¶](#Conditionals-1)

Conditional control structures choose among alternatives. Emacs Lisp
has five conditional forms: `if`, which is much the same as in
other languages; `when` and `unless`, which are variants of
`if`; `cond`, which is a generalized case statement;
and `pcase`, which is a generalization of `cond`
(see [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional)).

Special Form: **if** condition then-form else-forms… [¶](#index-if)
:   `if` chooses between the then-form and the else-forms
    based on the value of condition. If the evaluated condition is
    non-`nil`, then-form is evaluated and the result returned.
    Otherwise, the else-forms are evaluated in textual order, and the
    value of the last one is returned. (The else part of `if` is
    an example of an implicit `progn`. See [Sequencing](#Sequencing).)

    If condition has the value `nil`, and no else-forms are
    given, `if` returns `nil`.

    `if` is a special form because the branch that is not selected is
    never evaluated—it is ignored. Thus, in this example,
    `true` is not printed because `print` is never called:

    ```
    (if nil
        (print 'true)
      'very-false)
    ⇒ very-false
    ```

Macro: **when** condition then-forms… [¶](#index-when)
:   This is a variant of `if` where there are no else-forms,
    and possibly several then-forms. In particular,

    ```
    (when condition a b c)
    ```

    is entirely equivalent to

    ```
    (if condition (progn a b c) nil)
    ```

Macro: **unless** condition forms… [¶](#index-unless)
:   This is a variant of `if` where there is no then-form:

    ```
    (unless condition a b c)
    ```

    is entirely equivalent to

    ```
    (if condition nil
       a b c)
    ```

Special Form: **cond** clause… [¶](#index-cond)
:   `cond` chooses among an arbitrary number of alternatives. Each
    clause in the `cond` must be a list. The CAR of this
    list is the condition; the remaining elements, if any, the
    body-forms. Thus, a clause looks like this:

    ```
    (condition body-forms...)
    ```

    `cond` tries the clauses in textual order, by evaluating the
    condition of each clause. If the value of condition is
    non-`nil`, the clause succeeds; then `cond` evaluates its
    body-forms, and returns the value of the last of body-forms.
    Any remaining clauses are ignored.

    If the value of condition is `nil`, the clause fails, so
    the `cond` moves on to the following clause, trying its condition.

    A clause may also look like this:

    ```
    (condition)
    ```

    Then, if condition is non-`nil` when tested, the `cond`
    form returns the value of condition.

    If every condition evaluates to `nil`, so that every clause
    fails, `cond` returns `nil`.

    The following example has four clauses, which test for the cases where
    the value of `x` is a number, string, buffer and symbol,
    respectively:

    ```
    (cond ((numberp x) x)
          ((stringp x) x)
          ((bufferp x)
           (setq temporary-hack x) ; multiple body-forms
           (buffer-name x))        ; in one clause
          ((symbolp x) (symbol-value x)))
    ```

    Often we want to execute the last clause whenever none of the previous
    clauses was successful. To do this, we use `t` as the
    condition of the last clause, like this: `(t
    body-forms)`. The form `t` evaluates to `t`, which is
    never `nil`, so this clause never fails, provided the `cond`
    gets to it at all. For example:

    ```
    (setq a 5)
    (cond ((eq a 'hack) 'foo)
          (t "default"))
    ⇒ "default"
    ```

    This `cond` expression returns `foo` if the value of `a`
    is `hack`, and returns the string `"default"` otherwise.

Any conditional construct can be expressed with `cond` or with
`if`. Therefore, the choice between them is a matter of style.
For example:

```
(if a b c)
≡
(cond (a b) (t c))
```

It can be convenient to bind variables in conjunction with using a
conditional. It’s often the case that you compute a value, and then
want to do something with that value if it’s non-`nil`. The
straightforward way to do that is to just write, for instance:

```
(let ((result1 (do-computation)))
  (when result1
    (let ((result2 (do-more result1)))
      (when result2
        (do-something result2)))))
```

Since this is a very common pattern, Emacs provides a number of macros
to make this easier and more readable. The above can be written the
following way instead:

```
(when-let* ((result1 (do-computation))
            (result2 (do-more result1)))
  (do-something result2))
```

There’s a number of variations on this theme, and they’re described
below.

Macro: **if-let\*** varlist then-form else-forms... [¶](#index-if_002dlet_002a)
:   Evaluate each binding in varlist, stopping if a binding value is
    `nil`. If all are non-`nil`, return the value of
    then-form, otherwise the last form in else-forms.

    Each element of `varlist` has the form `(symbol value-form)`: value-form is evaluated and symbol is
    locally bound to the result. Bindings are sequential, as in `let*`
    (see [Local Variables](#Local-Variables)). If only the test result of value-form
    is of interest, use `_` for symbol. Then value-form is
    evaluated and checked for `nil`, but its value is not bound. As a
    special case, an entry of `varlist` may be of the form just
    `symbol`, which means to check the current binding of symbol
    for `nil`.

Macro: **when-let\*** varlist then-forms... [¶](#index-when_002dlet_002a)
:   Evaluate each binding in varlist, stopping if a binding value is
    `nil`. If all are non-`nil`, return the value of the last
    form in then-forms.

    varlist has the same form as in `if-let*`: Each element of
    `varlist` has the form `(symbol value-form)`,
    in which value-form is evaluated and symbol is locally bound
    to the result. Bindings are sequential, as in `let*` (see [Local Variables](#Local-Variables)). If only the test result of value-form is of
    interest, use `_` for symbol. Then value-form is
    evaluated and checked for `nil`, but its value is not bound. As a
    special case, an entry of `varlist` may be of the form just
    `symbol`, which means to check the current binding of symbol
    for `nil`.

Macro: **and-let\*** varlist then-forms... [¶](#index-and_002dlet_002a)
:   Evaluate each binding in varlist, stopping if a binding value is
    `nil`. If all are non-`nil`, return the value of the last
    form in then-forms, or, if there are no then-forms, return
    the value of the last binding.

    varlist has the same form as in `if-let*`: Each element of
    `varlist` has the form `(symbol value-form)`,
    in which value-form is evaluated and symbol is locally bound
    to the result. Bindings are sequential, as in `let*` (see [Local Variables](#Local-Variables)). If only the test result of value-form is of
    interest, use `_` for symbol. Then value-form is
    evaluated and checked for `nil`, but its value is not bound. As a
    special case, an entry of `varlist` may be of the form just
    `symbol`, which means to check the current binding of symbol
    for `nil`.

Some Lisp programmers follow the convention that `and` and
`and-let*` are for forms evaluated for return value, and
`when` and `when-let*` are for forms evaluated for side-effect
with returned values ignored.

As a matter of style, it is best not to use these macros to bind values
that will always be non-`nil`. For example, suppose that
`foo-p` and `bar-p` might return `nil`, and the code
should not proceed in that case. Instead of

```
(when-let* ((foo-val (foo-p))
            (num (+ 2 foo-val))
            (bar-val (bar-p num)))
  ...)
```

consider using

```
(when-let* ((foo-val (foo-p)))
  (let ((num (+ 2 foo-val))
    (when-let* ((bar-val (bar-p num)))
      ...)))
```

because this makes it clearer that the execution of the code is
conditional on `foo-val` and `bar-val` being non-`nil`,
but not directly conditional on `num`, which moreover can never be
`nil`.

There is no `cond-let*` macro because extending the structure
shared by `if-let*`, `when-let*` and `and-let*` to the
case of `cond` is not simple.[10](#FOOT10) However, you can use the `cond*` macro’s
`bind-and*` clauses (see [The `cond*` macro](#cond_002a-Macro)) to achieve something
similar:

```
(cond* ((bind-and* (result1 (do-computation))
                   (result2 (do-more result1)))
        (do-something result2))
       ...)
```

A similar macro exists to run a loop until one binding evaluates to
`nil`:

Macro: **while-let** spec then-forms... [¶](#index-while_002dlet)
:   Evaluate each binding in spec in turn, stopping if a binding value
    is `nil`. If all are non-`nil`, execute then-forms,
    then repeat the loop. Note that when the loop is repeated, the
    value-forms in spec are re-evaluated and the bindings are
    established anew.

    varlist has the same form as in `if-let*`: Each element of
    `varlist` has the form `(symbol value-form)`,
    in which value-form is evaluated and symbol is locally bound
    to the result. Bindings are sequential, as in `let*` (see [Local Variables](#Local-Variables)). If only the test result of value-form is of
    interest, use `_` for symbol. Then value-form is
    evaluated and checked for `nil`, but its value is not bound. As a
    special case, an entry of `varlist` may be of the form just
    `symbol`, which means to check the current binding of symbol
    for `nil`.

    The return value of `while-let` is always `nil`.

---

Next: [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional), Previous: [Conditionals](#Conditionals), Up: [Control Structures](#Control-Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.3 Constructs for Combining Conditions [¶](#Constructs-for-Combining-Conditions)

This section describes constructs that are often used together with
`if` and `cond` to express complicated conditions. The
constructs `and` and `or` can also be used individually as
kinds of multiple conditional constructs.

Function: **not** condition [¶](#index-not)
:   This function tests for the falsehood of condition. It returns
    `t` if condition is `nil`, and `nil` otherwise.
    The function `not` is identical to `null`, and we recommend
    using the name `null` if you are testing for an empty list or
    `nil` value.

Special Form: **and** conditions… [¶](#index-and)
:   The `and` special form tests whether all the conditions are
    true. It works by evaluating the conditions one by one in the
    order written.

    If any of the conditions evaluates to `nil`, then the result
    of the `and` must be `nil` regardless of the remaining
    conditions; so `and` returns `nil` right away, ignoring
    the remaining conditions.

    If all the conditions turn out non-`nil`, then the value of
    the last of them becomes the value of the `and` form. Just
    `(and)`, with no conditions, returns `t`, appropriate
    because all the conditions turned out non-`nil`. (Think
    about it; which one did not?)

    Here is an example. The first condition returns the integer 1, which is
    not `nil`. Similarly, the second condition returns the integer 2,
    which is not `nil`. The third condition is `nil`, so the
    remaining condition is never evaluated.

    ```
    (and (print 1) (print 2) nil (print 3))
         -| 1
         -| 2
    ⇒ nil
    ```

    Here is a more realistic example of using `and`:

    ```
    (if (and (consp foo) (eq (car foo) 'x))
        (message "foo is a list starting with x"))
    ```

    Note that `(car foo)` is not executed if `(consp foo)` returns
    `nil`, thus avoiding an error.

    `and` expressions can also be written using either `if` or
    `cond`. Here’s how:

    ```
    (and arg1 arg2 arg3)
    ≡
    (if arg1 (if arg2 arg3))
    ≡
    (cond (arg1 (cond (arg2 arg3))))
    ```

Special Form: **or** conditions… [¶](#index-or)
:   The `or` special form tests whether at least one of the
    conditions is true. It works by evaluating all the
    conditions one by one in the order written.

    If any of the conditions evaluates to a non-`nil` value, then
    the result of the `or` must be non-`nil`; so `or` returns
    right away, ignoring the remaining conditions. The value it
    returns is the non-`nil` value of the condition just evaluated.

    If all the conditions turn out `nil`, then the `or`
    expression returns `nil`. Just `(or)`, with no
    conditions, returns `nil`, appropriate because all the
    conditions turned out `nil`. (Think about it; which one
    did not?)

    For example, this expression tests whether `x` is either
    `nil` or the integer zero:

    ```
    (or (eq x nil) (eq x 0))
    ```

    Like the `and` construct, `or` can be written in terms of
    `cond`. For example:

    ```
    (or arg1 arg2 arg3)
    ≡
    (cond (arg1)
          (arg2)
          (arg3))
    ```

    You could almost write `or` in terms of `if`, but not quite:

    ```
    (if arg1 arg1
      (if arg2 arg2
        arg3))
    ```

    This is not completely equivalent because it can evaluate arg1 or
    arg2 twice. By contrast, `(or arg1 arg2
    arg3)` never evaluates any argument more than once.

Function: **xor** condition1 condition2 [¶](#index-xor)
:   This function returns the boolean exclusive-or of condition1 and
    condition2. That is, `xor` returns `nil` if either
    both arguments are `nil`, or both are non-`nil`. Otherwise,
    it returns the value of that argument which is non-`nil`.

    Note that in contrast to `or`, both arguments are always evaluated.

---

Next: [Iteration](#Iteration), Previous: [Constructs for Combining Conditions](#Combining-Conditions), Up: [Control Structures](#Control-Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.4 Pattern-Matching Conditional [¶](#Pattern_002dMatching-Conditional-1)

Aside from the four basic conditional forms, Emacs Lisp also
has a pattern-matching conditional form, the `pcase` macro,
a hybrid of `cond` and `cl-case`
(see [Conditionals](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Conditionals) in Common Lisp Extensions)
that overcomes their limitations and introduces
the *pattern matching programming style*.
The limitations that `pcase` overcomes are:

* The `cond` form chooses among alternatives by evaluating the
  predicate condition of each of its clauses
  (see [Conditionals](#Conditionals)). The primary limitation is that variables
  let-bound in condition are not available to the clause’s
  body-forms.

  Another annoyance (more an inconvenience than a limitation)
  is that when a series of condition predicates implement
  equality tests, there is a lot of repeated code. (`cl-case`
  solves this inconvenience.)
* The `cl-case` macro chooses among alternatives by evaluating
  the equality of its first argument against a set of specific
  values.

  Its limitations are two-fold:

  1. The equality tests use `eql`.
  2. The values must be known and written in advance.

  These render `cl-case` unsuitable for strings or compound
  data structures (e.g., lists or vectors). (`cond` doesn’t have
  these limitations, but it has others, see above.)

Conceptually, the `pcase` macro borrows the first-arg focus
of `cl-case` and the clause-processing flow of `cond`,
replacing condition with a generalization of
the equality test which is a variant of *pattern matching*,
and adding facilities so that you can concisely express a
clause’s predicate, and arrange to share let-bindings between
a clause’s predicate and body-forms.

The concise expression of a predicate is known as a *pattern*.
When the predicate, called on the value of the first arg, returns
non-`nil`, we say that “the pattern matches the value” (or
sometimes “the value matches the pattern”).

* [The `pcase` macro](#pcase-Macro)
* [Extending `pcase`](#Extending-pcase)
* [Backquote-Style Patterns](#Backquote-Patterns)
* [Destructuring with `pcase` Patterns](#Destructuring-with-pcase-Patterns)
* [The `cond*` macro](#cond_002a-Macro)

---

Next: [Extending `pcase`](#Extending-pcase), Up: [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.4.1 The `pcase` macro [¶](#The-pcase-macro)

For background, See [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional).

Macro: **pcase** expression &rest clauses [¶](#index-pcase-1)
:   Each clause in clauses has the form:
    `(pattern body-forms…)`.

    Evaluate expression to determine its value, expval.
    Find the first clause in clauses whose pattern matches
    expval and pass control to that clause’s body-forms.

    If there is a match, the value of `pcase` is the value
    of the last of body-forms in the successful clause.
    Otherwise, `pcase` evaluates to `nil`.

Each pattern has to be a *pcase pattern*, which can use
either one of the core patterns defined below, or one of the patterns
defined via `pcase-defmacro` (see [Extending `pcase`](#Extending-pcase)).

The rest of this subsection describes different forms of core
patterns, presents some examples, and concludes with important caveats
on using the let-binding facility provided by some pattern forms. A
core pattern can have the following forms:

`_ (underscore)`
:   Matches any expval.
    This is also known as *don’t care* or *wildcard*.

`'val`
:   Matches if expval equals val. The comparison is done as
    if by `equal` (see [Equality Predicates](#Equality-Predicates)).

`keyword`

`integer`

`string`
:   Matches if expval equals the literal object.
    This is a special case of `'val`, above,
    possible because literal objects of these types are self-quoting.

`symbol`
:   Matches any expval, and additionally let-binds symbol to
    expval, such that this binding is available to
    body-forms (see [Dynamic Binding](#Dynamic-Binding)).

    If symbol is part of a sequencing pattern seqpat
    (e.g., by using `and`, below), the binding is also available to
    the portion of seqpat following the appearance of symbol.
    This usage has some caveats, see [caveats](#pcase_002dsymbol_002dcaveats).

    Two symbols to avoid are `t`, which behaves like `_`
    (above) and is deprecated, and `nil`, which signals an error.
    Likewise, it makes no sense to bind keyword symbols
    (see [Variables that Never Change](#Constant-Variables)).

`` `qpat ``
:   A backquote-style pattern. See [Backquote-Style Patterns](#Backquote-Patterns), for the
    details.

`(cl-type type)`
:   Matches if expval is of type type, which is a type
    descriptor as accepted by `cl-typep` (see [Type Predicates](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Type-Predicates) in Common
    Lisp Extensions). Examples:

    ```
    (cl-type integer)
    (cl-type (integer 0 10))
    ```

`(pred function)`
:   Matches if the predicate function returns non-`nil`
    when called on expval. The test can be negated with the syntax
    `(pred (not function))`.
    The predicate function can have one of the following forms:

    function name (a symbol)
    :   Call the named function with one argument, expval.

        Example: `integerp`

    lambda expression
    :   Call the anonymous function with one argument,
        expval (see [Lambda Expressions](#Lambda-Expressions)).

        Example: `(lambda (n) (= 42 n))`

    function call with n args
    :   Call the function (the first element of the function call)
        with n arguments (the other elements) and an additional
        n+1-th argument that is expval.

        Example: `(= 42)`  
        In this example, the function is `=`, n is one, and
        the actual function call becomes: `(= 42 expval)`.

    function call with an `_` arg
    :   Call the function (the first element of the function call)
        with the specified arguments (the other elements) and replacing
        `_` with expval.

        Example: `(gethash _ memo-table)`
        In this example, the function is `gethash`, and
        the actual function call becomes: `(gethash expval memo-table)`.

`(app function pattern)`
:   Matches if function called on expval returns a
    value that matches pattern.
    function can take one of the forms described for `pred`,
    above. Unlike `pred`, however, `app` tests the result
    against pattern, rather than against a boolean truth value.

`(guard boolean-expression)`
:   Matches if boolean-expression evaluates to non-`nil`.

`(let pattern expr)`
:   Evaluates expr to get exprval and matches if exprval
    matches pattern. (It is called `let` because pattern
    can bind symbols to values using symbol.)

A *sequencing pattern* (also known as seqpat) is a
pattern that processes its sub-pattern arguments in sequence.
There are two for `pcase`: `and` and `or`.
They behave in a similar manner to the special forms
that share their name (see [Constructs for Combining Conditions](#Combining-Conditions)),
but instead of processing values, they process sub-patterns.

`(and pattern1…)`
:   Attempts to match pattern1…, in order, until one of them
    fails to match. In that case, `and` likewise fails to match, and
    the rest of the sub-patterns are not tested. If all sub-patterns
    match, `and` matches.

`(or pattern1 pattern2…)`
:   Attempts to match pattern1, pattern2, …, in order,
    until one of them succeeds. In that case, `or` likewise matches,
    and the rest of the sub-patterns are not tested.

    To present a consistent environment
    (see [Introduction to Evaluation](#Intro-Eval))
    to body-forms (thus avoiding an evaluation error on match),
    the set of variables bound by the pattern is the union of the
    variables bound by each sub-pattern. If a variable is not bound by
    the sub-pattern that matched, then it is bound to `nil`.

`(rx rx-expr…)`
:   Matches strings against the regexp rx-expr…, using the
    `rx` regexp notation (see [The `rx` Structured Regexp Notation](#Rx-Notation)), as if by
    `string-match`.

    In addition to the usual `rx` syntax, rx-expr… can
    contain the following constructs:

    `(let ref rx-expr…)`
    :   Bind the symbol ref to a submatch that matches
        rx-expr.... ref is bound in body-forms to
        the string of the submatch or `nil`, but can also be used in
        `backref`.

    `(backref ref)`
    :   Like the standard `backref` construct, but ref can here
        also be a name introduced by a previous `(let ref …)`
        construct.

#### Example: Advantage Over `cl-case` [¶](#Example_003a-Advantage-Over-cl_002dcase)

Here’s an example that highlights some advantages `pcase`
has over `cl-case`
(see [Conditionals](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Conditionals) in Common Lisp Extensions).

```
(pcase (get-return-code x)
  ;; string
  ((and (pred stringp) msg)
   (message "%s" msg))
```

```
  ;; symbol
  ('success       (message "Done!"))
  ('would-block   (message "Sorry, can't do it now"))
  ('read-only     (message "The schmilblick is read-only"))
  ('access-denied (message "You do not have the needed rights"))
```

```
  ;; default
  (code           (message "Unknown return code %S" code)))
```

With `cl-case`, you would need to explicitly declare a local
variable `code` to hold the return value of `get-return-code`.
Also `cl-case` is difficult to use with strings because it
uses `eql` for comparison.

#### Example: Using `and` [¶](#Example_003a-Using-and)

A common idiom is to write a pattern starting with `and`,
with one or more symbol sub-patterns providing bindings
to the sub-patterns that follow (as well as to the body forms).
For example, the following pattern matches single-digit integers.

```
(and
  (pred integerp)
  n                     ; bind n to expval
  (guard (<= -9 n 9)))
```

First, `pred` matches if `(integerp expval)`
evaluates to non-`nil`.
Next, `n` is a symbol pattern that matches
anything and binds `n` to expval.
Lastly, `guard` matches if the boolean expression
`(<= -9 n 9)` (note the reference to `n`)
evaluates to non-`nil`.
If all these sub-patterns match, `and` matches.

#### Example: Reformulation with `pcase` [¶](#Example_003a-Reformulation-with-pcase)

Here is another example that shows how to reformulate a simple
matching task from its traditional implementation
(function `grok/traditional`) to one using
`pcase` (function `grok/pcase`).
The docstring for both these functions is:
“If OBJ is a string of the form "key:NUMBER", return NUMBER
(a string). Otherwise, return the list ("149" default).”
First, the traditional implementation (see [Regular Expressions](#Regular-Expressions)):

```
(defun grok/traditional (obj)
  (if (and (stringp obj)
           (string-match "^key:\\([[:digit:]]+\\)$" obj))
      (match-string 1 obj)
    (list "149" 'default)))
```

```

```

```
(grok/traditional "key:0")   ⇒ "0"
(grok/traditional "key:149") ⇒ "149"
(grok/traditional 'monolith) ⇒ ("149" default)
```

The reformulation demonstrates symbol binding as well as
`or`, `and`, `pred`, `app` and `let`.

```
(defun grok/pcase (obj)
  (pcase obj
    ((or                                     ; line 1
      (and                                   ; line 2
       (pred stringp)                        ; line 3
       (pred (string-match                   ; line 4
              "^key:\\([[:digit:]]+\\)$"))   ; line 5
       (app (match-string 1)                 ; line 6
            val))                            ; line 7
      (let val (list "149" 'default)))       ; line 8
     val)))                                  ; line 9
```

```

```

```
(grok/pcase "key:0")   ⇒ "0"
(grok/pcase "key:149") ⇒ "149"
(grok/pcase 'monolith) ⇒ ("149" default)
```

The bulk of `grok/pcase` is a single clause of a `pcase`
form, the pattern on lines 1-8, the (single) body form on line 9.
The pattern is `or`, which tries to match in turn its argument
sub-patterns, first `and` (lines 2-7), then `let` (line 8),
until one of them succeeds.

As in the previous example (see [Example 1](#pcase_002dexample_002d1)),
`and` begins with a `pred` sub-pattern to ensure
the following sub-patterns work with an object of the correct
type (string, in this case). If `(stringp expval)`
returns `nil`, `pred` fails, and thus `and` fails, too.

The next `pred` (lines 4-5) evaluates
`(string-match RX expval)`
and matches if the result is non-`nil`, which means
that expval has the desired form: `key:NUMBER`.
Again, failing this, `pred` fails and `and`, too.

Lastly (in this series of `and` sub-patterns), `app`
evaluates `(match-string 1 expval)` (line 6)
to get a temporary value tmp (i.e., the “NUMBER” substring)
and tries to match tmp against pattern `val` (line 7).
Since that is a symbol pattern, it matches unconditionally
and additionally binds `val` to tmp.

Now that `app` has matched, all `and` sub-patterns
have matched, and so `and` matches.
Likewise, once `and` has matched, `or` matches
and does not proceed to try sub-pattern `let` (line 8).

Let’s consider the situation where `obj` is not a string,
or it is a string but has the wrong form.
In this case, one of the `pred` (lines 3-5) fails to match,
thus `and` (line 2) fails to match,
thus `or` (line 1) proceeds to try sub-pattern `let` (line 8).

First, `let` evaluates `(list "149" 'default)`
to get `("149" default)`, the exprval, and then
tries to match exprval against pattern `val`.
Since that is a symbol pattern, it matches unconditionally
and additionally binds `val` to exprval.
Now that `let` has matched, `or` matches.

Note how both `and` and `let` sub-patterns finish in the
same way: by trying (always successfully) to match against the
symbol pattern `val`, in the process binding `val`.
Thus, `or` always matches and control always passes
to the body form (line 9).
Because that is the last body form in a successfully matched
`pcase` clause, it is the value of `pcase` and likewise
the return value of `grok/pcase` (see [What Is a Function?](#What-Is-a-Function)).

#### Caveats for symbol in Sequencing Patterns [¶](#Caveats-for-symbol-in-Sequencing-Patterns)

The preceding examples all use sequencing patterns
which include the symbol
sub-pattern in some way.
Here are some important details about that usage.

1. When symbol occurs more than once in seqpat,
   the second and subsequent occurrences do not expand to re-binding,
   but instead expand to an equality test using `eq`.

   The following example features a `pcase` form
   with two clauses and two seqpat, A and B.
   Both A and B first check that expval is a
   pair (using `pred`),
   and then bind symbols to the `car` and `cdr`
   of expval (using one `app` each).

   For A, because symbol `st` is mentioned twice, the second
   mention becomes an equality test using `eq`.
   On the other hand, B uses two separate symbols, `s1` and
   `s2`, both of which become independent bindings.

   ```
   (defun grok (object)
     (pcase object
       ((and (pred consp)        ; seqpat A
             (app car st)        ; first mention: st
             (app cdr st))       ; second mention: st
        (list 'eq st))
   ```

   ```
       ((and (pred consp)        ; seqpat B
             (app car s1)        ; first mention: s1
             (app cdr s2))       ; first mention: s2
        (list 'not-eq s1 s2))))
   ```

   ```

   ```

   ```
   (let ((s "yow!"))
     (grok (cons s s)))      ⇒ (eq "yow!")
   (grok (cons "yo!" "yo!")) ⇒ (not-eq "yo!" "yo!")
   (grok '(4 2))             ⇒ (not-eq 4 (2))
   ```
2. Side-effecting code referencing symbol is undefined.
   Avoid.
   For example, here are two similar functions.
   Both use `and`, symbol and `guard`:

   ```
   (defun square-double-digit-p/CLEAN (integer)
     (pcase (* integer integer)
       ((and n (guard (< 9 n 100))) (list 'yes n))
       (sorry (list 'no sorry))))

   (square-double-digit-p/CLEAN 9) ⇒ (yes 81)
   (square-double-digit-p/CLEAN 3) ⇒ (no 9)
   ```

   ```

   ```

   ```
   (defun square-double-digit-p/MAYBE (integer)
     (pcase (* integer integer)
       ((and n (guard (< 9 (incf n) 100))) (list 'yes n))
       (sorry (list 'no sorry))))

   (square-double-digit-p/MAYBE 9) ⇒ (yes 81)
   (square-double-digit-p/MAYBE 3) ⇒ (yes 9)  ; WRONG!
   ```

   The difference is in boolean-expression in `guard`:
   `CLEAN` references `n` simply and directly,
   while `MAYBE` references `n` with a side-effect,
   in the expression `(incf n)`.
   When `integer` is 3, here’s what happens:

   * The first `n` binds it to expval,
     i.e., the result of evaluating `(* 3 3)`, or 9.
   * boolean-expression is evaluated:

     ```
     start:   (< 9 (incf n)        100)
     becomes: (< 9 (setq n (1+ n)) 100)
     becomes: (< 9 (setq n (1+ 9)) 100)
     ```

     ```
     becomes: (< 9 (setq n 10)     100)
                                        ; side-effect here!
     becomes: (< 9       n         100) ; n now bound to 10
     becomes: (< 9      10         100)
     becomes: t
     ```
   * Because the result of the evaluation is non-`nil`,
     `guard` matches, `and` matches, and
     control passes to that clause’s body forms.

   Aside from the mathematical incorrectness of asserting that 9 is a
   double-digit integer, there is another problem with `MAYBE`.
   The body form references `n` once more, yet we do not see
   the updated value—10—at all. What happened to it?

   To sum up, it’s best to avoid side-effecting references to
   symbol patterns entirely, not only
   in boolean-expression (in `guard`),
   but also in expr (in `let`)
   and function (in `pred` and `app`).
3. On match, the clause’s body forms can reference the set
   of symbols the pattern let-binds.
   When seqpat is `and`, this set is
   the union of all the symbols each of its sub-patterns let-binds.
   This makes sense because, for `and` to match,
   all the sub-patterns must match.

   When seqpat is `or`, things are different:
   `or` matches at the first sub-pattern that matches;
   the rest of the sub-patterns are ignored.
   It makes no sense for each sub-pattern to let-bind a different
   set of symbols because the body forms have no way to distinguish
   which sub-pattern matched and choose among the different sets.
   For example, the following is invalid:

   ```
   (require 'cl-lib)
   (pcase (read-number "Enter an integer: ")
     ((or (and (pred cl-evenp)
               e-num)      ; bind e-num to expval
          o-num)           ; bind o-num to expval
      (list e-num o-num)))
   ```

   ```

   ```

   ```
   Enter an integer: 42
   error→ Symbol's value as variable is void: o-num
   ```

   ```
   Enter an integer: 149
   error→ Symbol's value as variable is void: e-num
   ```

   Evaluating body form `(list e-num o-num)` signals error.
   To distinguish between sub-patterns, you can use another symbol,
   identical in name in all sub-patterns but differing in value.
   Reworking the above example:

   ```
   (require 'cl-lib)
   (pcase (read-number "Enter an integer: ")
     ((and num                                ; line 1
           (or (and (pred cl-evenp)           ; line 2
                    (let spin 'even))         ; line 3
               (let spin 'odd)))              ; line 4
      (list spin num)))                       ; line 5
   ```

   ```

   ```

   ```
   Enter an integer: 42
   ⇒ (even 42)
   ```

   ```
   Enter an integer: 149
   ⇒ (odd 149)
   ```

   Line 1 “factors out” the expval binding with
   `and` and symbol (in this case, `num`).
   On line 2, `or` begins in the same way as before,
   but instead of binding different symbols, uses `let` twice
   (lines 3-4) to bind the same symbol `spin` in both sub-patterns.
   The value of `spin` distinguishes the sub-patterns.
   The body form references both symbols (line 5).

---

Next: [Backquote-Style Patterns](#Backquote-Patterns), Previous: [The `pcase` macro](#pcase-Macro), Up: [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.4.2 Extending `pcase` [¶](#Extending-pcase-1)

The `pcase` macro supports several kinds of patterns
(see [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional)).
You can add support for other kinds of patterns
using the `pcase-defmacro` macro.

Macro: **pcase-defmacro** name args [doc] &rest body [¶](#index-pcase_002ddefmacro)
:   Define a new kind of pattern for `pcase`, to be invoked
    as `(name actual-args)`.
    The `pcase` macro expands this into a function call
    that evaluates body, whose job it is to
    rewrite the invoked pattern into some other pattern,
    in an environment where args are bound to actual-args.

    Additionally, arrange to display doc along with
    the docstring of `pcase`.
    By convention, doc should use `EXPVAL`
    to stand for the result of
    evaluating expression (first arg to `pcase`).

Typically, body rewrites the invoked pattern
to use more basic patterns.
Although all patterns eventually reduce to core patterns,
`body` need not use core patterns straight away.
The following example defines two patterns, named
`less-than` and `integer-less-than`.

```
(pcase-defmacro less-than (n)
  "Matches if EXPVAL is a number less than N."
  `(pred (> ,n)))
```

```

```

```
(pcase-defmacro integer-less-than (n)
  "Matches if EXPVAL is an integer less than N."
  `(and (pred integerp)
        (less-than ,n)))
```

Note that the docstrings mention args
(in this case, only one: `n`) in the usual way,
and also mention `EXPVAL` by convention.
The first rewrite (i.e., body for `less-than`)
uses one core pattern: `pred`.
The second uses two core patterns: `and` and `pred`,
as well as the newly-defined pattern `less-than`.
Both use a single backquote construct (see [Backquote](#Backquote)).

---

Next: [Destructuring with `pcase` Patterns](#Destructuring-with-pcase-Patterns), Previous: [Extending `pcase`](#Extending-pcase), Up: [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.4.3 Backquote-Style Patterns [¶](#Backquote_002dStyle-Patterns)

This subsection describes *backquote-style patterns*,
a set of builtin patterns that eases structural matching.
For background, see [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional).

Backquote-style patterns are a powerful set of `pcase` pattern
extensions (created using `pcase-defmacro`) that make it easy to
match expval against specifications of its *structure*.

For example, to match expval that must be a list of two
elements whose first element is a specific string and the second
element is any value, you can write a core pattern:

```
(and (pred listp)
     ls
```

```
     (guard (= 2 (length ls)))
     (guard (string= "first" (car ls)))
     (let second-elem (cadr ls)))
```

or you can write the equivalent backquote-style pattern:

```
`("first" ,second-elem)
```

The backquote-style pattern is more concise,
resembles the structure of expval,
and avoids binding `ls`.

A backquote-style pattern has the form `` `qpat `` where
qpat can have the following forms:

`(qpat1 . qpat2)`
:   Matches if expval is a cons cell whose `car`
    matches qpat1 and whose `cdr` matches qpat2.
    This readily generalizes to lists as in
    `(qpat1 qpat2 …)`.

`[qpat1 qpat2 … qpatm]`
:   Matches if expval is a vector of length m whose
    `0`..`(m-1)`th elements match qpat1,
    qpat2 … qpatm, respectively.

`symbol`

`keyword`

`number`

`string`
:   Matches if the corresponding element of expval is
    `equal` to the specified literal object.

`,pattern`
:   Matches if the corresponding element of expval
    matches pattern.
    Note that pattern is any kind that `pcase` supports.
    (In the example above, `second-elem` is a symbol
    core pattern; it therefore matches anything,
    and let-binds `second-elem`.)

The *corresponding element* is the portion of expval
that is in the same structural position as the structural position
of qpat in the backquote-style pattern.
(In the example above, the corresponding element of
`second-elem` is the second element of expval.)

Here is an example of using `pcase` to implement a simple
interpreter for a little expression language
(note that this requires lexical binding for the
lambda expression in the `fn` clause to properly
capture `body` and `arg` (see [Lexical Binding](#Lexical-Binding)):

```
(defun evaluate (form env)
  (pcase form
    (`(add ,x ,y)       (+ (evaluate x env)
                           (evaluate y env)))
```

```
    (`(call ,fun ,arg)  (funcall (evaluate fun env)
                                 (evaluate arg env)))
    (`(fn ,arg ,body)   (lambda (val)
                          (evaluate body (cons (cons arg val)
                                               env))))
```

```
    ((pred numberp)     form)
    ((pred symbolp)     (cdr (assq form env)))
    (_                  (error "Syntax error: %S" form))))
```

The first three clauses use backquote-style patterns.
`` `(add ,x ,y) `` is a pattern that checks that `form`
is a three-element list starting with the literal symbol `add`,
then extracts the second and third elements and binds them
to symbols `x` and `y`, respectively. This is known as
*destructuring*, see [Destructuring with `pcase` Patterns](#Destructuring-with-pcase-Patterns).
The clause body evaluates `x` and `y` and adds the results.
Similarly, the `call` clause implements a function call,
and the `fn` clause implements an anonymous function definition.

The remaining clauses use core patterns.
`(pred numberp)` matches if `form` is a number.
On match, the body evaluates it.
`(pred symbolp)` matches if `form` is a symbol.
On match, the body looks up the symbol in `env` and
returns its association.
Finally, `_` is the catch-all pattern that
matches anything, so it’s suitable for reporting syntax errors.

Here are some sample programs in this small language, including their
evaluation results:

```
(evaluate '(add 1 2) nil)                 ⇒ 3
(evaluate '(add x y) '((x . 1) (y . 2)))  ⇒ 3
(evaluate '(call (fn x (add 1 x)) 2) nil) ⇒ 3
(evaluate '(sub 1 2) nil)                 ⇒ error
```

---

Next: [The `cond*` macro](#cond_002a-Macro), Previous: [Backquote-Style Patterns](#Backquote-Patterns), Up: [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.4.4 Destructuring with `pcase` Patterns [¶](#Destructuring-with-pcase-Patterns-1)

Pcase patterns not only express a condition on the form of the objects
they can match, but they can also extract sub-fields of those objects.
For example we can extract 2 elements from a list that is the value of
the variable `my-list` with the following code:

```
  (pcase my-list
    (`(add ,x ,y)  (message "Contains %S and %S" x y)))
```

This will not only extract `x` and `y` but will additionally
test that `my-list` is a list containing exactly 3 elements and
whose first element is the symbol `add`. If any of those tests
fail, `pcase` will immediately return `nil` without calling
`message`.

Extraction of multiple values stored in an object is known as
*destructuring*. Using `pcase` patterns allows you to
perform *destructuring binding*, which is similar to a local
binding (see [Local Variables](#Local-Variables)), but gives values to multiple
elements of a variable by extracting those values from an object of
compatible structure.

The macros described in this section use `pcase` patterns to
perform destructuring binding. The condition of the object to be of
compatible structure means that the object must match the pattern,
because only then the object’s subfields can be extracted. For
example:

```
  (pcase-let ((`(add ,x ,y) my-list))
    (message "Contains %S and %S" x y))
```

does the same as the previous example, except that it directly tries
to extract `x` and `y` from `my-list` without first
verifying if `my-list` is a list which has the right number of
elements and has `add` as its first element.

The precise behavior when the object does not actually match the pattern
depends on the types, although the body will not be silently skipped:
either an error is signaled or the body is run with some of the
variables bound to arbitrary values like `nil`.
For example, the above pattern will result in x and y
being extracted with operations like `car` or `nth`, so they
will get value `nil` when my-list is too short. In contrast,
with a pattern like `` `[add ,x ,y] ``, those same variables would
be extracted using `aref` which would signal an error if
my-list is not an array or is too short.

The pcase patterns that are useful for destructuring bindings are
generally those described in [Backquote-Style Patterns](#Backquote-Patterns), since they
express a specification of the structure of objects that will match.

For an alternative facility for destructuring binding, see
[seq-let](#seq_002dlet).

Macro: **pcase-let** bindings body… [¶](#index-pcase_002dlet)
:   Perform destructuring binding of variables according to
    bindings, and then evaluate body.

    bindings is a list of bindings of the form `(pattern exp)`, where exp is an expression to evaluate and
    pattern is a `pcase` pattern.

    All exps are evaluated first, after which they are matched
    against their respective pattern, introducing new variable
    bindings that can then be used inside body. The variable
    bindings are produced by destructuring binding of elements of
    pattern to the values of the corresponding elements of the
    evaluated exp.

    Here’s a trivial example:

    ```
    (pcase-let ((`(,major ,minor)
    	     (split-string "image/png" "/")))
      minor)
         ⇒ "png"
    ```

Macro: **pcase-let\*** bindings body… [¶](#index-pcase_002dlet_002a)
:   Perform destructuring binding of variables according to
    bindings, and then evaluate body.

    bindings is a list of bindings of the form `(pattern
    exp)`, where exp is an expression to evaluate and
    pattern is a `pcase` pattern. The variable bindings are
    produced by destructuring binding of elements of pattern to the
    values of the corresponding elements of the evaluated exp.

    Unlike `pcase-let`, but similarly to `let*`, each exp
    is matched against its corresponding pattern before processing
    the next element of bindings, so the variable bindings
    introduced in each one of the bindings are available in the
    exps of the bindings that follow it, additionally to
    being available in body.

Macro: **pcase-dolist** (pattern list) body… [¶](#index-pcase_002ddolist)
:   Execute body once for each element of list, on each
    iteration performing a destructuring binding of variables in
    pattern to the values of the corresponding subfields of the
    element of list. The bindings are performed as if by
    `pcase-let`. When pattern is a simple variable, this ends
    up being equivalent to `dolist` (see [Iteration](#Iteration)).

Macro: **pcase-setq** pattern value… [¶](#index-pcase_002dsetq)
:   Assign values to variables in a `setq` form, destructuring each
    value according to its respective pattern.

Macro: **pcase-lambda** lambda-list &rest body [¶](#index-pcase_002dlambda)
:   This is like `lambda`, but allows each argument to be a pattern.
    For instance, here’s a simple function that takes a cons cell as the
    argument:

    ```
    (setq fun
          (pcase-lambda (`(,key . ,val))
            (vector key (* val 10))))
    (funcall fun '(foo . 2))
        ⇒ [foo 20]
    ```

---

Previous: [Destructuring with `pcase` Patterns](#Destructuring-with-pcase-Patterns), Up: [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.4.5 The `cond*` macro [¶](#The-cond_002a-macro)

You can use the `cond*` macro as an alternative to `pcase`
if you find `pcase`’s syntax too cryptic. In addition,
`cond*` offers some new forms of control flow that aren’t related
to being an alternative to `pcase`.

Macro: **cond\*** &rest clauses [¶](#index-cond_002a)
:   The `cond*` macro is an extended form of the traditional
    `cond`. A `cond*` expression contains a series of
    clauses, each of which can use `bind*` or `bind-and*` to
    specify binding variables, use `match*` or `pcase*` to specify
    matching a pattern as a condition, or specify an expression as a
    condition to evaluate as a test.

    Each clause normally has the form `(condition body…)`.

    condition can be a Lisp expression, as in `cond`
    (see [Conditionals](#Conditionals)). Or it can be `(bind* bindings…)`, `(match* pattern datum)`,
    `(bind-and* bindings…)` or `(pcase* pattern datum)`

    `(bind* bindings…)` means to bind bindings (like
    the bindings list in `let*`, see [Local Variables](#Local-Variables)) for the body
    of the clause, and all subsequent clauses. As a condition, it counts as
    true if the first binding’s value is non-`nil`.

    `(bind-and* bindings…)` means to bind bindings
    (like the bindings list in `if-let*`, see [Conditionals](#Conditionals)) for
    only the body of the clause. As a condition, it counts as true if none
    of the bindings evaluate to `nil`. In addition, if any binding
    evaluates to `nil`, the expressions for the values of subsequent
    bindings are not evaluated.

    `(match* pattern datum)` means to match datum
    against the specified pattern. The condition counts as true if
    pattern matches datum. The pattern can specify variables to
    bind to the parts of datum that they match.
    `(pcase* pattern datum)` works in the same way except it
    uses the Pcase syntax for pattern.

    `match*`, and `pcase*` normally bind their bindings over the
    execution of the whole containing clause. However, if the clause is
    written to specify “non-exit” (see below), the clause’s bindings cover
    the whole rest of the `cond*`.

    When a clause’s condition is true, and it exits the `cond*` or is
    the last clause, the value of the last expression in the clause’s body
    becomes the return value of the `cond*` construct.

    #### Non-exit clauses [¶](#Non_002dexit-clauses)

    If the first element of a clause is `t` or a `bind*` form, or
    if it has only one element and that element is a `match*` or
    `pcase*` form, or if it ends with the keyword `:non-exit`,
    then this clause never exits the `cond*` construct. Instead,
    control falls through to the next clause (if any). Except for a
    `bind-and*` clause, the bindings made in condition for the
    body of the non-exit clause are passed along to the rest of the
    clauses in this `cond*` construct.

    Note: `pcase*` does not support `:non-exit`, and when used in
    a non-exit clause, it follows the semantics of `pcase-let`, see
    [Destructuring with `pcase` Patterns](#Destructuring-with-pcase-Patterns).

    #### Matching clauses [¶](#Matching-clauses)

    A matching clause looks like `(match* pattern datum)`.
    It evaluates the expression datum and matches the pattern
    pattern (which is not evaluated) against it.

    pattern allows these kinds of patterns, and those that are lists
    often include other patterns within them:

    `_`
    :   Matches any value.

    `keyword`
    :   Matches that keyword.

    `nil`
    :   Matches `nil`.

    `t`
    :   Matches `t`.

    `symbol`
    :   Matches any value and binds symbol to that value. If symbol
        has been matched and bound earlier in this pattern, it matches here the
        same value that it matched before.

    `regexp`
    :   Matches a string if regexp matches it. The match must cover the
        entire string from its first char to its last.

    `atom`
    :   (Meaning any other kind of non-list not described above.) Matches
        anything ‘equal’ to it.

    `(rx regexp)`
    :   Uses a regexp specified in s-expression form, as in the function
        `rx` (see [The `rx` Structured Regexp Notation](#Rx-Notation), and matches the data that way.

    `(rx regexp sym0 sym1…)`
    :   Uses a regexp specified in s-expression form, and binds the symbols
        sym0, sym1, and so on to `(match-string 0 datum)`, `(match-string 1 datum)`, and so on. You
        can use as many syms as regexp matching supports.

    `` `object ``
    :   Matches any value `equal` to object.

    `(cons carpat cdrpat)`
    :   Matches a cons cell if carpat matches its `car` and
        cdrpat matches its `cdr`.

    `(list eltpats…)`
    :   Matches a list if the eltpats match its elements. The first
        eltpat should match the list’s first element. The second
        eltpat should match the list’s second element. And so on.

    `(vector eltpats…)`
    :   Matches a vector if the eltpats match its elements. The first
        eltpat should match the vector’s first element. The second
        eltpat should match the vector’s second element. And so on.

    `(cdr pattern)`
    :   Matches pattern with strict checking of `cdr`s. That means
        that `list` patterns verify that the final `cdr` is
        `nil`. Strict checking is the default.

    `(cdr-ignore pattern)`
    :   Matches pattern with lax checking of `cdr`s. That means that
        `list` patterns do not examine the final `cdr`.

    `(and conjuncts…)`
    :   Matches each of the conjuncts against the same data. If all of
        them match, this pattern succeeds. If one conjunct fails, this
        pattern fails and does not try more conjuncts.

    `(or disjuncts…)`
    :   Matches each of the disjuncts against the same data. If one
        disjunct succeeds, this pattern succeeds and does not try more
        disjuncts. If all of them fail, this pattern fails.

    `(cond*-expander …)`
    :   Here the `car` is a symbol that has a `cond*-expander`
        property which defines how to handle it in a pattern. The property
        value is a function. Trying to match such a pattern calls that function
        with one argument, the pattern in question (including its `car`).
        The function should return an equivalent pattern to be matched instead.

    `(predicate symbol)`
    :   Matches datum if `(predicate datum)` is true, then
        binds symbol to datum.

    `(predicate SYMBOL more-args…)`
    :   Matches datum if `(predicate datum more-args…)` is true, then binds symbol to
        datum. more-args… can refer to symbols bound earlier
        in the pattern.

    `(constrain symbol exp)`
    :   Matches datum if the form exp is true. exp can refer to
        symbols bound earlier in the pattern.

---

Next: [Generators](#Generators), Previous: [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional), Up: [Control Structures](#Control-Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.5 Iteration [¶](#Iteration-1)

Iteration means executing part of a program repetitively. For
example, you might want to repeat some computation once for each element
of a list, or once for each integer from 0 to n. You can do this
in Emacs Lisp with the special form `while`:

Special Form: **while** condition forms… [¶](#index-while)
:   `while` first evaluates condition. If the result is
    non-`nil`, it evaluates forms in textual order. Then it
    reevaluates condition, and if the result is non-`nil`, it
    evaluates forms again. This process repeats until condition
    evaluates to `nil`.

    There is no limit on the number of iterations that may occur. The loop
    will continue until either condition evaluates to `nil` or
    until an error or `throw` jumps out of it (see [Nonlocal Exits](#Nonlocal-Exits)).

    The value of a `while` form is always `nil`.

    ```
    (setq num 0)
         ⇒ 0
    ```

    ```
    (while (< num 4)
      (princ (format "Iteration %d." num))
      (setq num (1+ num)))
         -| Iteration 0.
         -| Iteration 1.
         -| Iteration 2.
         -| Iteration 3.
         ⇒ nil
    ```

    To write a repeat-until loop, which will execute something on each
    iteration and then do the end-test, put the body followed by the
    end-test in a `progn` as the first argument of `while`, as
    shown here:

    ```
    (while (progn
             (forward-line 1)
             (not (looking-at "^$"))))
    ```

    This moves forward one line and continues moving by lines until it
    reaches an empty line. It is peculiar in that the `while` has no
    body, just the end test (which also does the real work of moving point).

The `dolist` and `dotimes` macros provide convenient ways to
write two common kinds of loops.

Macro: **dolist** (var list [result]) body… [¶](#index-dolist)
:   This construct executes body once for each element of
    list, binding the variable var locally to hold the current
    element. Then it returns the value of evaluating result, or
    `nil` if result is omitted. For example, here is how you
    could use `dolist` to define the `reverse` function:

    ```
    (defun reverse (list)
      (let (value)
        (dolist (elt list value)
          (setq value (cons elt value)))))
    ```

Macro: **dotimes** (var count [result]) body… [¶](#index-dotimes)
:   This construct executes body once for each integer from 0
    (inclusive) to count (exclusive), binding the variable var
    to the integer for the current iteration. Then it returns the value
    of evaluating result, or `nil` if result is omitted.
    Use of result is deprecated. Here is an example of using
    `dotimes` to do something 100 times:

    ```
    (dotimes (i 100)
      (insert "I will not obey absurd orders\n"))
    ```

---

Next: [Nonlocal Exits](#Nonlocal-Exits), Previous: [Iteration](#Iteration), Up: [Control Structures](#Control-Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.6 Generators [¶](#Generators-1)

A *generator* is a function that produces a potentially-infinite
stream of values. Each time the function produces a value, it
suspends itself and waits for a caller to request the next value.

Macro: **iter-defun** name args [doc] [declare] [interactive] body… [¶](#index-iter_002ddefun)
:   `iter-defun` defines a generator function. A generator function
    has the same signature as a normal function, but works differently.
    Instead of executing body when called, a generator function
    returns an iterator object. That iterator runs body to generate
    values, emitting a value and pausing where `iter-yield` or
    `iter-yield-from` appears. When body returns normally,
    `iter-next` signals `iter-end-of-sequence` with body’s
    result as its condition data.

    Any kind of Lisp code is valid inside body, but
    `iter-yield` and `iter-yield-from` cannot appear inside
    `unwind-protect` forms.

Macro: **iter-lambda** args [doc] [interactive] body… [¶](#index-iter_002dlambda)
:   `iter-lambda` produces an unnamed generator function that works
    just like a generator function produced with `iter-defun`.

Macro: **iter-yield** value [¶](#index-iter_002dyield)
:   When it appears inside a generator function, `iter-yield`
    indicates that the current iterator should pause and return
    value from `iter-next`. `iter-yield` evaluates to the
    `value` parameter of next call to `iter-next`.

Macro: **iter-yield-from** iterator [¶](#index-iter_002dyield_002dfrom)
:   `iter-yield-from` yields all the values that iterator
    produces and evaluates to the value that iterator’s generator
    function returns normally. While it has control, iterator
    receives values sent to the iterator using `iter-next`.

To use a generator function, first call it normally, producing a
*iterator* object. An iterator is a specific instance of a
generator. Then use `iter-next` to retrieve values from this
iterator. When there are no more values to pull from an iterator,
`iter-next` raises an `iter-end-of-sequence` condition with
the iterator’s final value.

It’s important to note that generator function bodies only execute
inside calls to `iter-next`. A call to a function defined with
`iter-defun` produces an iterator; you must drive this
iterator with `iter-next` for anything interesting to happen.
Each call to a generator function produces a *different*
iterator, each with its own state.

Function: **iter-next** iterator &optional value [¶](#index-iter_002dnext)
:   Retrieve the next value from iterator. If there are no more
    values to be generated (because iterator’s generator function
    returned), `iter-next` signals the `iter-end-of-sequence`
    condition; the data value associated with this condition is the value
    with which iterator’s generator function returned.

    value is sent into the iterator and becomes the value to which
    `iter-yield` evaluates. value is ignored for the first
    `iter-next` call to a given iterator, since at the start of
    iterator’s generator function, the generator function is not
    evaluating any `iter-yield` form.

Function: **iter-close** iterator [¶](#index-iter_002dclose)
:   If iterator is suspended inside an `unwind-protect`’s
    `bodyform` and becomes unreachable, Emacs will eventually run
    unwind handlers after a garbage collection pass. (Note that
    `iter-yield` is illegal inside an `unwind-protect`’s
    `unwindforms`.) To ensure that these handlers are run before
    then, use `iter-close`.

Some convenience functions are provided to make working with
iterators easier:

Macro: **iter-do** (var iterator) body … [¶](#index-iter_002ddo)
:   Run body with var bound to each value that
    iterator produces.

The Common Lisp loop facility also contains features for working with
iterators. See [Loop Facility](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Loop-Facility) in Common Lisp Extensions.

The following piece of code demonstrates some important principles of
working with iterators.

```
(require 'generator)
(iter-defun my-iter (x)
  (iter-yield (1+ (iter-yield (1+ x))))
   ;; Return normally
  -1)

(let* ((iter (my-iter 5))
       (iter2 (my-iter 0)))
  ;; Prints 6
  (print (iter-next iter))
  ;; Prints 9
  (print (iter-next iter 8))
  ;; Prints 1; iter and iter2 have distinct states
  (print (iter-next iter2 nil))

  ;; We expect the iter sequence to end now
  (condition-case x
      (iter-next iter)
    (iter-end-of-sequence
      ;; Prints -1, which my-iter returned normally
      (print (cdr x)))))
```

---

Next: [Conditional Compilation](#Conditional-Compilation), Previous: [Generators](#Generators), Up: [Control Structures](#Control-Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
