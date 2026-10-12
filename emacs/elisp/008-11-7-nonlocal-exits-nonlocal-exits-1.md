### 11.7 Nonlocal Exits [¶](#Nonlocal-Exits-1)

A *nonlocal exit* is a transfer of control from one point in a
program to another remote point. Nonlocal exits can occur in Emacs Lisp
as a result of errors; you can also use them under explicit control.
Nonlocal exits unbind all variable bindings made by the constructs being
exited.

* [Explicit Nonlocal Exits: `catch` and `throw`](#Catch-and-Throw)
* [Examples of `catch` and `throw`](#Examples-of-Catch)
* [Errors](#Errors)
* [Cleaning Up from Nonlocal Exits](#Cleanups)

---

Next: [Examples of `catch` and `throw`](#Examples-of-Catch), Up: [Nonlocal Exits](#Nonlocal-Exits)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.7.1 Explicit Nonlocal Exits: `catch` and `throw` [¶](#Explicit-Nonlocal-Exits_003a-catch-and-throw)

Most control constructs affect only the flow of control within the
construct itself. The function `throw` is the exception to this
rule of normal program execution: it performs a nonlocal exit on
request. (There are other exceptions, but they are for error handling
only.) `throw` is used inside a `catch`, and jumps back to
that `catch`. For example:

```
(defun foo-outer ()
  (catch 'foo
    (foo-inner)))

(defun foo-inner ()
  ...
  (if x
      (throw 'foo t))
  ...)
```

The `throw` form, if executed, transfers control straight back to
the corresponding `catch`, which returns immediately. The code
following the `throw` is not executed. The second argument of
`throw` is used as the return value of the `catch`.

The function `throw` finds the matching `catch` based on the
first argument: it searches for a `catch` whose first argument is
`eq` to the one specified in the `throw`. If there is more
than one applicable `catch`, the innermost one takes precedence.
Thus, in the above example, the `throw` specifies `foo`, and
the `catch` in `foo-outer` specifies the same symbol, so that
`catch` is the applicable one (assuming there is no other matching
`catch` in between).

Executing `throw` exits all Lisp constructs up to the matching
`catch`, including function calls. When binding constructs such
as `let` or function calls are exited in this way, the bindings
are unbound, just as they are when these constructs exit normally
(see [Local Variables](#Local-Variables)). Likewise, `throw` restores the buffer
and position saved by `save-excursion` (see [Excursions](#Excursions)), and
the narrowing status saved by `save-restriction`. It also runs
any cleanups established with the `unwind-protect` special form
when it exits that form (see [Cleaning Up from Nonlocal Exits](#Cleanups)).

The `throw` need not appear lexically within the `catch`
that it jumps to. It can equally well be called from another function
called within the `catch`. As long as the `throw` takes place
chronologically after entry to the `catch`, and chronologically
before exit from it, it has access to that `catch`. This is why
`throw` can be used in commands such as `exit-recursive-edit`
that throw back to the editor command loop (see [Recursive Editing](#Recursive-Editing)).

> **Common Lisp note:** Most other versions of Lisp, including Common Lisp,
> have several ways of transferring control nonsequentially: `return`,
> `return-from`, and `go`, for example. Emacs Lisp has only
> `throw`. The `cl-lib` library provides versions of some of
> these. See [Blocks and Exits](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Blocks-and-Exits) in Common Lisp Extensions.

Special Form: **catch** tag body… [¶](#index-catch)
:   `catch` establishes a return point for the `throw` function.
    The return point is distinguished from other such return points by
    tag, which may be any Lisp object except `nil`. The argument
    tag is evaluated normally before the return point is established.

    With the return point in effect, `catch` evaluates the forms of the
    body in textual order. If the forms execute normally (without
    error or nonlocal exit) the value of the last body form is returned from
    the `catch`.

    If a `throw` is executed during the execution of body,
    specifying the same value tag, the `catch` form exits
    immediately; the value it returns is whatever was specified as the
    second argument of `throw`.

Function: **throw** tag value [¶](#index-throw)
:   The purpose of `throw` is to return from a return point previously
    established with `catch`. The argument tag is used to choose
    among the various existing return points; it must be `eq` to the value
    specified in the `catch`. If multiple return points match tag,
    the innermost one is used.

    The argument value is used as the value to return from that
    `catch`.

    If no return point is in effect with tag tag, then a `no-catch`
    error is signaled with data `(tag value)`.

---

Next: [Errors](#Errors), Previous: [Explicit Nonlocal Exits: `catch` and `throw`](#Catch-and-Throw), Up: [Nonlocal Exits](#Nonlocal-Exits)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.7.2 Examples of `catch` and `throw` [¶](#Examples-of-catch-and-throw)

One way to use `catch` and `throw` is to exit from a doubly
nested loop. (In most languages, this would be done with a `goto`.)
Here we compute `(foo i j)` for i and j
varying from 0 to 9:

```
(defun search-foo ()
  (catch 'loop
    (let ((i 0))
      (while (< i 10)
        (let ((j 0))
          (while (< j 10)
            (if (foo i j)
                (throw 'loop (list i j)))
            (setq j (1+ j))))
        (setq i (1+ i))))))
```

If `foo` ever returns non-`nil`, we stop immediately and return a
list of i and j. If `foo` always returns `nil`, the
`catch` returns normally, and the value is `nil`, since that
is the result of the `while`.

Here are two tricky examples, slightly different, showing two
return points at once. First, two return points with the same tag,
`hack`:

```
(defun catch2 (tag)
  (catch tag
    (throw 'hack 'yes)))
⇒ catch2
```

```

```

```
(catch 'hack
  (print (catch2 'hack))
  'no)
-| yes
⇒ no
```

Since both return points have tags that match the `throw`, it goes to
the inner one, the one established in `catch2`. Therefore,
`catch2` returns normally with value `yes`, and this value is
printed. Finally the second body form in the outer `catch`, which is
`'no`, is evaluated and returned from the outer `catch`.

Now let’s change the argument given to `catch2`:

```
(catch 'hack
  (print (catch2 'quux))
  'no)
⇒ yes
```

We still have two return points, but this time only the outer one has
the tag `hack`; the inner one has the tag `quux` instead.
Therefore, `throw` makes the outer `catch` return the value
`yes`. The function `print` is never called, and the
body-form `'no` is never evaluated.

---

Next: [Cleaning Up from Nonlocal Exits](#Cleanups), Previous: [Examples of `catch` and `throw`](#Examples-of-Catch), Up: [Nonlocal Exits](#Nonlocal-Exits)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.7.3 Errors [¶](#Errors-1)

When Emacs Lisp attempts to evaluate a form that, for some reason,
cannot be evaluated, it *signals* an *error*.

When an error is signaled, Emacs’s default reaction is to print an
error message and terminate execution of the current command. This is
the right thing to do in most cases, such as if you type `C-f` at
the end of the buffer.

In complicated programs, simple termination may not be what you want.
For example, the program may have made temporary changes in data
structures, or created temporary buffers that should be deleted before
the program is finished. In such cases, you would use
`unwind-protect` to establish *cleanup expressions* to be
evaluated in case of error. (See [Cleaning Up from Nonlocal Exits](#Cleanups).) Occasionally, you may
wish the program to continue execution despite an error in a subroutine.
In these cases, you would use `condition-case` to establish
*error handlers* to recover control in case of error.

For reporting problems without terminating the execution of the
current command, consider issuing a warning instead. See [Reporting Warnings](#Warnings).

Resist the temptation to use error handling to transfer control from
one part of the program to another; use `catch` and `throw`
instead. See [Explicit Nonlocal Exits: `catch` and `throw`](#Catch-and-Throw).

* [How to Signal an Error](#Signaling-Errors)
* [How Emacs Processes Errors](#Processing-of-Errors)
* [Writing Code to Handle Errors](#Handling-Errors)
* [Error Symbols and Condition Names](#Error-Symbols)

---

Next: [How Emacs Processes Errors](#Processing-of-Errors), Up: [Errors](#Errors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.7.3.1 How to Signal an Error [¶](#How-to-Signal-an-Error)

*Signaling* an error means beginning error processing. Error
processing normally aborts all or part of the running program and
returns to a point that is set up to handle the error
(see [How Emacs Processes Errors](#Processing-of-Errors)). Here we describe how to signal an
error.

Most errors are signaled automatically within Lisp primitives
which you call for other purposes, such as if you try to take the
CAR of an integer or move forward a character at the end of the
buffer. You can also signal errors explicitly with the functions
`error` and `signal`.

Quitting, which happens when the user types `C-g`, is not
considered an error, but it is handled almost like an error.
See [Quitting](#Quitting).

Every error specifies an error message, one way or another. The
message should state what is wrong (“File does not exist”), not how
things ought to be (“File must exist”). The convention in Emacs
Lisp is that error messages should start with a capital letter, but
should not end with any sort of punctuation.

Function: **error** format-string &rest args [¶](#index-error)
:   This function signals an error with an error message constructed by
    applying `format-message` (see [Formatting Strings](#Formatting-Strings)) to
    format-string and args.

    These examples show typical uses of `error`:

    ```
    (error "That is an error -- try something else")
         error→ That is an error -- try something else
    ```

    ```

    ```

    ```
    (error "Invalid name `%s'" "A%%B")
         error→ Invalid name `A%%B'
    ```

    `error` works by calling `signal` with two arguments: the
    error symbol `error`, and a list containing the string returned by
    `format-message`.

    Typically grave accent and apostrophe in the format translate to
    matching curved quotes, e.g., `` "Missing `%s'" `` might result in
    `"Missing ‘foo’"`. See [Text Quoting Style](#Text-Quoting-Style), for how to influence
    or inhibit this translation.

    **Warning:** If you want to use your own string as an error message
    verbatim, don’t just write `(error string)`. If string
    string contains ‘`%`’, ‘`` ` ``’, or ‘`'`’ it may be
    reformatted, with undesirable results. Instead, use `(error "%s"
    string)`.

    When `noninteractive` is non-`nil` (see [Batch Mode](#Batch-Mode)),
    this function kills Emacs if the signaled error has no handler.

Function: **signal** error-symbol data [¶](#index-signal)
:   This function signals an error named by error-symbol. The
    argument data is a list of additional Lisp objects relevant to
    the circumstances of the error.

    The argument error-symbol must be an *error symbol*—a symbol
    defined with `define-error`. This is how Emacs Lisp classifies different
    sorts of errors. See [Error Symbols and Condition Names](#Error-Symbols), for a description of error symbols,
    error conditions and condition names.

    If the error is not handled, the two arguments are used in printing
    the error message. Normally, this error message is provided by the
    `error-message` property of error-symbol. If data is
    non-`nil`, this is followed by a colon and a comma separated list
    of the unevaluated elements of data. For `error`, the
    error message is the CAR of data (that must be a string).
    Subcategories of `file-error` are handled specially.

    The number and significance of the objects in data depends on
    error-symbol. For example, with a `wrong-type-argument` error,
    there should be two objects in the list: a predicate that describes the type
    that was expected, and the object that failed to fit that type.

    Both error-symbol and data are available to any error
    handlers that handle the error: `condition-case` binds a local
    variable to a list of the form `(error-symbol .
    data)` (see [Writing Code to Handle Errors](#Handling-Errors)).

    The function `signal` never returns.
    If the error error-symbol has no handler, and
    `noninteractive` is non-`nil` (see [Batch Mode](#Batch-Mode)),
    this function eventually kills Emacs.

    ```
    (signal 'wrong-number-of-arguments '(x y))
         error→ Wrong number of arguments: x, y
    ```

    ```

    ```

    ```
    (signal 'no-such-error '("My unknown error condition"))
         error→ peculiar error: "My unknown error condition"
    ```

Function: **user-error** format-string &rest args [¶](#index-user_002derror)
:   This function behaves exactly like `error`, except that it uses
    the error symbol `user-error` rather than `error`. As the
    name suggests, this is intended to report errors on the part of the
    user, rather than errors in the code itself. For example,
    if you try to use the command `Info-history-back` (`l`) to
    move back beyond the start of your Info browsing history, Emacs
    signals a `user-error`. Such errors do not cause entry to the
    debugger, even when `debug-on-error` is non-`nil`.
    See [Entering the Debugger on an Error](#Error-Debugging).

> **Common Lisp note:** Emacs Lisp has nothing like the Common Lisp
> concept of continuable errors.

---

Next: [Writing Code to Handle Errors](#Handling-Errors), Previous: [How to Signal an Error](#Signaling-Errors), Up: [Errors](#Errors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.7.3.2 How Emacs Processes Errors [¶](#How-Emacs-Processes-Errors)

When a Lisp program signals an error, it calls the function
`signal`, which searches for an active
*handler* for the error. A handler is a sequence of Lisp
expressions designated to be executed if an error happens in part of the
Lisp program. If the error has an applicable handler, the handler is
executed, and control resumes following the handler. The handler
executes in the environment of the `condition-case` that
established it; all functions called within that `condition-case`
have already been exited, and the handler cannot return to them.

If there is no applicable handler for the error, it terminates the
current command, flushes all pending unprocessed input events
(see [Input Events](#Input-Events))[11](#FOOT11), and returns control to the editor command loop.
(The command loop has an implicit handler for all kinds of errors.) The
command loop’s handler then uses the error symbol and associated data to
print an error message. You can use the variable
`command-error-function` to control how this is done:

Variable: **command-error-function** [¶](#index-command_002derror_002dfunction)
:   This variable, if non-`nil`, specifies a function to use to
    handle errors that return control to the Emacs command loop. The
    function should take three arguments: data, a list of the same
    form that `condition-case` would bind to its variable;
    context, a string describing the situation in which the error
    occurred, or (more often) `nil`; and caller, the Lisp
    function which called the primitive that signaled the error.

An error that has no explicit handler may call the Lisp debugger
(see [Invoking the Debugger](#Invoking-the-Debugger)). The debugger is enabled if the
variable `debug-on-error` (see [Entering the Debugger on an Error](#Error-Debugging)) is
non-`nil`. Unlike error handlers, the debugger runs in the
environment of the error, so that you can examine values of variables
precisely as they were at the time of the error. In batch mode
(see [Batch Mode](#Batch-Mode)), the Emacs process then normally exits with a
non-zero exit status.

---

Next: [Error Symbols and Condition Names](#Error-Symbols), Previous: [How Emacs Processes Errors](#Processing-of-Errors), Up: [Errors](#Errors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.7.3.3 Writing Code to Handle Errors [¶](#Writing-Code-to-Handle-Errors)

The usual effect of signaling an error is to terminate the command
that is running and return immediately to the Emacs editor command loop.
You can arrange to trap errors occurring in a part of your program by
establishing an error handler, with the special form
`condition-case`. A simple example looks like this:

```
(condition-case nil
    (delete-file filename)
  (error nil))
```

This deletes the file named filename, catching any error and
returning `nil` if an error occurs. (You can use the macro
`ignore-errors` for a simple case like this; see below.)

The `condition-case` construct is often used to trap errors that
are predictable, such as failure to open a file in a call to
`insert-file-contents`. It is also used to trap errors that are
totally unpredictable, such as when the program evaluates an expression
read from the user.

The second argument of `condition-case` is called the
*protected form*. (In the example above, the protected form is a
call to `delete-file`.) The error handlers go into effect when
this form begins execution and are deactivated when this form returns.
They remain in effect for all the intervening time. In particular, they
are in effect during the execution of functions called by this form, in
their subroutines, and so on. This is a good thing, since, strictly
speaking, errors can be signaled only by Lisp primitives (including
`signal` and `error`) called by the protected form, not by the
protected form itself.

The arguments after the protected form are handlers. Each handler
lists one or more *condition names* (which are symbols) to specify
which errors it will handle. The error symbol specified when an error
is signaled also defines a list of condition names. A handler applies
to an error if they have any condition names in common. In the example
above, there is one handler, and it specifies one condition name,
`error`, which covers all errors.

The search for an applicable handler checks all the established handlers
starting with the most recently established one. Thus, if two nested
`condition-case` forms offer to handle the same error, the inner of
the two gets to handle it.

If an error is handled by some `condition-case` form, this
ordinarily prevents the debugger from being run, even if
`debug-on-error` says this error should invoke the debugger.

If you want to be able to debug errors that are caught by a
`condition-case`, set the variable `debug-on-signal` to a
non-`nil` value. You can also specify that a particular handler
should let the debugger run first, by writing `debug` among the
conditions, like this:

```
(condition-case nil
    (delete-file filename)
  ((debug error) nil))
```

The effect of `debug` here is only to prevent
`condition-case` from suppressing the call to the debugger. Any
given error will invoke the debugger only if `debug-on-error` and
the other usual filtering mechanisms say it should. See [Entering the Debugger on an Error](#Error-Debugging).

Macro: **condition-case-unless-debug** var protected-form handlers… [¶](#index-condition_002dcase_002dunless_002ddebug)
:   The macro `condition-case-unless-debug` provides another way to
    handle debugging of such forms. It behaves exactly like
    `condition-case`, unless the variable `debug-on-error` is
    non-`nil`, in which case it causes Emacs to enter the debugger
    before executing any applicable handler. (The applicable handler, if
    any, will still run when the debugger exits.)

Once Emacs decides that a certain handler handles the error, it
returns control to that handler. To do so, Emacs unbinds all variable
bindings made by binding constructs that are being exited, and
executes the cleanups of all `unwind-protect` forms that are
being exited. Once control arrives at the handler, the body of the
handler executes normally.

After execution of the handler body, execution returns from the
`condition-case` form. Because the protected form is exited
completely before execution of the handler, the handler cannot resume
execution at the point of the error, nor can it examine variable
bindings that were made within the protected form. All it can do is
clean up and proceed.

Error signaling and handling have some resemblance to `throw` and
`catch` (see [Explicit Nonlocal Exits: `catch` and `throw`](#Catch-and-Throw)), but they are entirely separate
facilities. An error cannot be caught by a `catch`, and a
`throw` cannot be handled by an error handler (though using
`throw` when there is no suitable `catch` signals an error
that can be handled).

Special Form: **condition-case** var protected-form handlers… [¶](#index-condition_002dcase)
:   This special form establishes the error handlers handlers around
    the execution of protected-form. If protected-form executes
    without error, the value it returns becomes the value of the
    `condition-case` form (in the absence of a success handler; see below).
    In this case, the `condition-case` has
    no effect. The `condition-case` form makes a difference when an
    error occurs during protected-form.

    Each of the handlers is a list of the form `(conditions
    body…)`. Here conditions is an error condition name
    to be handled, or a list of condition names (which can include `debug`
    to allow the debugger to run before the handler). A condition name of
    `t` matches any condition. body is one or more Lisp
    expressions to be executed when this handler handles an error. Here
    are examples of handlers:

    ```
    (error nil)

    (arith-error (message "Division by zero"))

    ((arith-error file-error)
     (message
      "Either division by zero or failure to open a file"))
    ```

    Each error that occurs has an *error symbol* that describes what
    kind of error it is, and which describes also a list of condition names
    (see [Error Symbols and Condition Names](#Error-Symbols)). Emacs
    searches all the active `condition-case` forms for a handler that
    specifies one or more of these condition names; the innermost matching
    `condition-case` handles the error. Within this
    `condition-case`, the first applicable handler handles the error.

    After executing the body of the handler, the `condition-case`
    returns normally, using the value of the last form in the handler body
    as the overall value.

    The argument var is a variable. `condition-case` does not
    bind this variable when executing the protected-form, only when it
    handles an error. At that time, it binds var locally to an
    *error descriptor*, also sometimes called error description,
    which is a list giving the particulars of the error.
    The error descriptor has the form `(error-symbol
    . data)`. The handler can refer to this list to decide what to
    do. For example, if the error is for failure opening a file, the file
    name is the second element of data—the third element of the
    error descriptor.

    If var is `nil`, that means no variable is bound. Then the
    error symbol and associated data are not available to the handler.

    As a special case, one of the handlers can be a list of the
    form `(:success body…)`, where body is executed
    with var (if non-`nil`) bound to the return value of
    protected-form when that expression terminates without error.

    Sometimes it is necessary to re-throw a signal caught by
    `condition-case`, for some outer-level handler to catch. Here’s
    how to do that:

    ```
      (signal err)
    ```

    where `err` is the error descriptor variable, the first argument
    to `condition-case` whose error condition you want to re-throw.
    See [Definition of signal](#Definition-of-signal).

Function: **error-type** error [¶](#index-error_002dtype)
:   This function returns the error symbol of the error descriptor error.

Function: **error-slot-value** error pos [¶](#index-error_002dslot_002dvalue)
:   This function returns the value in the field number pos of the error
    descriptor error. The fields are numbered starting with 1. E.g.,
    for an error of type `wrong-type-argument`, `(error-slot-value
    error 2)` returns the object that failed the type test, and
    `(error-slot-value error 1)` returns the predicate that failed.

Function: **error-message-string** error-descriptor [¶](#index-error_002dmessage_002dstring)
:   This function returns the error message string for a given error
    descriptor. It is useful if you want to handle an error by printing the
    usual error message for that error. See [Definition of signal](#Definition-of-signal).

Here is an example of using `condition-case` to handle the error
that results from dividing by zero. The handler displays the error
message (but without a beep), then returns a very large number.

```
(defun safe-divide (dividend divisor)
  (condition-case err
      ;; Protected form.
      (/ dividend divisor)
```

```
    ;; The handler.
    (arith-error                        ; Condition.
     ;; Display the usual message for this error.
     (message "%s" (error-message-string err))
     1000000)))
⇒ safe-divide
```

```

```

```
(safe-divide 5 0)
     -| Arithmetic error: (arith-error)
⇒ 1000000
```

The handler specifies condition name `arith-error` so that it
will handle only division-by-zero errors. Other kinds of errors will
not be handled (by this `condition-case`). Thus:

```
(safe-divide nil 3)
     error→ Wrong type argument: number-or-marker-p, nil
```

Here is a `condition-case` that catches all kinds of errors,
including those from `error`:

```
(setq baz 34)
     ⇒ 34
```

```

```

```
(condition-case err
    (if (eq baz 35)
        t
      ;; This is a call to the function error.
      (error "Rats!  The variable %s was %s, not 35" 'baz baz))
  ;; This is the handler; it is not a form.
  (error (princ (format "The error was: %s" err))
         2))
-| The error was: (error "Rats!  The variable baz was 34, not 35")
⇒ 2
```

Macro: **ignore-errors** body… [¶](#index-ignore_002derrors)
:   This construct executes body, ignoring any errors that occur
    during its execution. If the execution is without error,
    `ignore-errors` returns the value of the last form in body;
    otherwise, it returns `nil`.

    Here’s the example at the beginning of this subsection rewritten using
    `ignore-errors`:

    ```
      (ignore-errors
       (delete-file filename))
    ```

Macro: **ignore-error** condition body… [¶](#index-ignore_002derror)
:   This macro is like `ignore-errors`, but will only ignore the
    specific error condition specified.

    ```
      (ignore-error end-of-file
        (read ""))
    ```

    condition can also be a list of error conditions.

Macro: **with-demoted-errors** format body… [¶](#index-with_002ddemoted_002derrors)
:   This macro is like a milder version of `ignore-errors`. Rather
    than suppressing errors altogether, it converts them into messages.
    It uses the string format to format the message.
    format should contain a single ‘`%`’-sequence; e.g.,
    `"Error: %S"`. Use `with-demoted-errors` around code
    that is not expected to signal errors, but
    should be robust if one does occur. Note that this macro uses
    `condition-case-unless-debug` rather than `condition-case`.

Occasionally, we want to catch some errors and record some information
about the conditions in which they occurred, such as the full
backtrace, or the current buffer. This kinds of information is sadly
not available in the handlers of a `condition-case` because the
stack is unwound before running that handler, so the handler is run in
the dynamic context of the `condition-case` rather than that of
the place where the error was signaled. For those circumstances, you
can use the following form:

Macro: **handler-bind** handlers body… [¶](#index-handler_002dbind)
:   This special form runs body and if it executes without error,
    the value it returns becomes the value of the `handler-bind`
    form. In this case, the `handler-bind` has no effect.

    handlers should be a list of elements of the form
    `(conditions handler)` where conditions is an
    error condition name to be handled, or a list of condition names, and
    handler should be a form whose evaluation should return a function.
    As with `condition-case`, condition names are symbols.

    Before running body, `handler-bind` evaluates all the
    handler forms and installs those handlers to be active during
    the evaluation of body. When an error is signaled,
    Emacs searches all the active `condition-case` and
    `handler-bind` forms for a handler that
    specifies one or more of these condition names. When the innermost
    matching handler is one installed by `handler-bind`, the
    handler function is called with a single argument holding the
    error descriptor.

    Contrary to what happens with `condition-case`, handler is
    called in the dynamic context where the error happened. This means it
    is executed without unbinding any variable bindings or running any
    cleanups of `unwind-protect`, so that all those dynamic bindings
    are still in effect. There is one exception: while running the
    handler function, all the error handlers between the code that
    signaled the error and the `handler-bind` are temporarily
    suspended, meaning that when an error is signaled, Emacs will only
    search the active `condition-case` and `handler-bind` forms
    that are inside the handler function or outside of the current
    `handler-bind`. Note also that lexically-bound variables
    (see [Lexical Binding](#Lexical-Binding)) are not affected, since they do not have
    dynamic extent.

    Like any normal function, handler can exit non-locally,
    typically via `throw`, or it can return normally.
    If handler returns normally, it means the handler
    *declined* to handle the error and the search for an error
    handler is continued where it left off.

    For example, if we wanted to keep a log of all the errors that occur
    during the execution of a particular piece of code together with the
    buffer that’s current when the error is signaled, but without
    otherwise affecting the behavior of that code, we can do it with:

    ```
    (handler-bind
        ((error
          (lambda (err)
            (push (cons err (current-buffer)) my-log-of-errors))))
      body-forms...)
    ```

    This will log only those errors that are not caught internally to
    body-forms…, in other words errors that “escape” from
    body-forms…, and it will not prevent those errors from
    being passed on to surrounding `condition-case` handlers (or
    `handler-bind` handlers for that matter) since the above handler
    returns normally.

    We can also use `handler-bind` to replace an error with another,
    as in the code below which turns all errors of type `user-error`
    that occur during the execution of body-forms… into plain
    `error`:

    ```
    (handler-bind
        ((user-error
          (lambda (err)
            (signal 'error (cdr err)))))
      body-forms...)
    ```

    We can get almost the same result with `condition-case`:

    ```
    (condition-case err
        (progn body-forms...)
      (user-error (signal 'error (cdr err))))
    ```

    but with the difference that when we (re)signal the new error in
    `handler-bind`, the dynamic environment from the original error
    is still active, which means for example that if we enter the debugger
    at this point, it will show us a complete backtrace including the
    point where we signaled the original error:

    ```
    Debugger entered--Lisp error: (error "Oops")
      signal(error ("Oops"))
      #f(lambda (err) [t] (signal 'error (cdr err)))((user-error "Oops"))
      user-error("Oops")
      ...
      eval((handler-bind ((user-error (lambda (err) ...
    ```

---

Previous: [Writing Code to Handle Errors](#Handling-Errors), Up: [Errors](#Errors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.7.3.4 Error Symbols and Condition Names [¶](#Error-Symbols-and-Condition-Names)

When you signal an error, you specify an *error symbol* to specify
the kind of error you have in mind. Each error has one and only one
error symbol to categorize it. This is the finest classification of
errors defined by the Emacs Lisp language.

These narrow classifications are grouped into a hierarchy of wider
classes called *error conditions*, identified by *condition
names*. The narrowest such classes belong to the error symbols
themselves: each error symbol is also a condition name. There are also
condition names for more extensive classes, up to the condition name
`error` which takes in all kinds of errors (but not `quit`).
Thus, each error has one or more condition names: `error`, the
error symbol if that is distinct from `error`, and perhaps some
intermediate classifications.

Function: **define-error** name message &optional parent [¶](#index-define_002derror)
:   In order for a symbol to be an error symbol, it must be defined with
    `define-error` which takes a parent condition (defaults to `error`).
    This parent defines the conditions that this kind of error belongs to.
    The transitive set of parents always includes the error symbol itself, and the
    symbol `error`. Because quitting is not considered an error, the set of
    parents of `quit` is just `(quit)`.

In addition to its parents, the error symbol has a message which
is a string to be printed when that error is signaled but not handled. If that
message is not valid, the error message ‘`peculiar error`’ is used.
See [Definition of signal](#Definition-of-signal).

Internally, the set of parents is stored in the `error-conditions`
property of the error symbol and the message is stored in the
`error-message` property of the error symbol.

Here is how we define a new error symbol, `new-error`:

```
(define-error 'new-error "A new error" 'my-own-errors)
```

This error has several condition names: `new-error`, the narrowest
classification; `my-own-errors`, which we imagine is a wider
classification; and all the conditions of `my-own-errors` which should
include `error`, which is the widest of all.

The error string should start with a capital letter but it should
not end with a period. This is for consistency with the rest of Emacs.

Naturally, Emacs will never signal `new-error` on its own; only
an explicit call to `signal` (see [Definition of signal](#Definition-of-signal)) in
your code can do this:

```
(signal 'new-error '(x y))
     error→ A new error: x, y
```

This error can be handled through any of its condition names.
This example handles `new-error` and any other errors in the class
`my-own-errors`:

```
(condition-case foo
    (bar nil t)
  (my-own-errors nil))
```

The significant way that errors are classified is by their condition
names—the names used to match errors with handlers. An error symbol
serves only as a convenient way to specify the intended error message
and list of condition names. It would be cumbersome to give
`signal` a list of condition names rather than one error symbol.

By contrast, using only error symbols without condition names would
seriously decrease the power of `condition-case`. Condition names
make it possible to categorize errors at various levels of generality
when you write an error handler. Using error symbols alone would
eliminate all but the narrowest level of classification.

Function: **error-type-p** symbol [¶](#index-error_002dtype_002dp)
:   This function returns non-`nil` if symbol is a valid
    error condition name.

Function: **error-has-type-p** error condition [¶](#index-error_002dhas_002dtype_002dp)
:   This function tests whether condition is a parent of the error
    symbol of the error descriptor error.
    It returns non-`nil` if the type of the error descriptor
    error belongs to the condition name condition.

See [Standard Errors](#Standard-Errors), for a list of the main error symbols
and their conditions.

---

Previous: [Errors](#Errors), Up: [Nonlocal Exits](#Nonlocal-Exits)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.7.4 Cleaning Up from Nonlocal Exits [¶](#Cleaning-Up-from-Nonlocal-Exits)

The `unwind-protect` construct is essential whenever you
temporarily put a data structure in an inconsistent state; it permits
you to make the data consistent again in the event of an error or
throw. (Another more specific cleanup construct that is used only for
changes in buffer contents is the atomic change group; [Atomic Change Groups](#Atomic-Changes).)

Special Form: **unwind-protect** body-form cleanup-forms… [¶](#index-unwind_002dprotect)
:   `unwind-protect` executes body-form with a guarantee that
    the cleanup-forms will be evaluated if control leaves
    body-form, no matter how that happens. body-form may
    complete normally, or execute a `throw` out of the
    `unwind-protect`, or cause an error; in all cases, the
    cleanup-forms will be evaluated.

    If body-form finishes normally, `unwind-protect` returns the
    value of body-form, after it evaluates the cleanup-forms.
    If body-form does not finish, `unwind-protect` does not
    return any value in the normal sense.

    Only body-form is protected by the `unwind-protect`. If any
    of the cleanup-forms themselves exits nonlocally (via a
    `throw` or an error), `unwind-protect` is *not*
    guaranteed to evaluate the rest of them. If the failure of one of the
    cleanup-forms has the potential to cause trouble, then protect
    it with another `unwind-protect` around that form.

For example, here we make an invisible buffer for temporary use, and
make sure to kill it before finishing:

```
(let ((buffer (get-buffer-create " *temp*")))
  (with-current-buffer buffer
    (unwind-protect
        body-form
      (kill-buffer buffer))))
```

You might think that we could just as well write `(kill-buffer
(current-buffer))` and dispense with the variable `buffer`.
However, the way shown above is safer, if body-form happens to
get an error after switching to a different buffer! (Alternatively,
you could write a `save-current-buffer` around body-form,
to ensure that the temporary buffer becomes current again in time to
kill it.)

Emacs includes a standard macro called `with-temp-buffer` which
expands into more or less the code shown above (see [Current Buffer](#Definition-of-with_002dtemp_002dbuffer)). Several of the macros defined in
this manual use `unwind-protect` in this way.

Here is an actual example derived from an FTP package. It creates a
process (see [Processes](#Processes)) to try to establish a connection to a remote
machine. As the function `ftp-login` is highly susceptible to
numerous problems that the writer of the function cannot anticipate, it
is protected with a form that guarantees deletion of the process in the
event of failure. Otherwise, Emacs might fill up with useless
subprocesses.

```
(let ((win nil))
  (unwind-protect
      (progn
        (setq process (ftp-setup-buffer host file))
        (if (setq win (ftp-login process host user password))
            (message "Logged in")
          (error "Ftp login failed")))
    (or win (and process (delete-process process)))))
```

This example has a small bug: if the user types `C-g` to
quit, and the quit happens immediately after the function
`ftp-setup-buffer` returns but before the variable `process` is
set, the process will not be killed. There is no easy way to fix this bug,
but at least it is very unlikely.

---

Previous: [Nonlocal Exits](#Nonlocal-Exits), Up: [Control Structures](#Control-Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.8 Conditional Compilation [¶](#Conditional-Compilation-1)

There will be times when you want certain code to be compiled only
when a certain condition holds. This is particularly the case when
maintaining Emacs packages; to keep the package compatible with older
versions of Emacs you may need to use a function or variable which has
become obsolete in the current version of Emacs.

You could just use a conditional form to select the old or new form
at run time, but this tends to output annoying warning messages about
the obsolete function/variable. For such situations, the macro
`static-if` comes in handy. It is patterned after the special
form `if` (see [Conditionals](#Conditionals)).

To use this facility for an older version of Emacs, copy the source
for `static-if` from the Emacs source file `lisp/subr.el`
into your package.

Macro: **static-if** condition then-form else-forms... [¶](#index-static_002dif)
:   Test condition at macro-expansion time. If its value is
    non-`nil`, expand the macro to then-form, otherwise expand
    it to else-forms enclosed in a `progn`. else-forms
    may be empty.

Macro: **static-when** condition body... [¶](#index-static_002dwhen)
:   Test condition at macro-expansion time. If its value is
    non-`nil`, expand the macro to evaluate all body forms
    sequentially and return the value of the last one, or `nil` if there
    are none.

Macro: **static-unless** condition body... [¶](#index-static_002dunless)
:   Test condition at macro-expansion time. If its value is `nil`,
    expand the macro to evaluate all body forms sequentially and return
    the value of the last one, or `nil` if there are none.

    Here is an example of its use from CC Mode, which prevents a
    `defadvice` form being compiled in newer versions of Emacs:

    ```
    (static-if (boundp 'comment-line-break-function)
        (progn)
      (defvar c-inside-line-break-advice nil)
      (defadvice indent-new-comment-line (around c-line-break-advice
                                                 activate preactivate)
        "Call `c-indent-new-comment-line' if in CC Mode."
        (if (or c-inside-line-break-advice
                (not c-buffer-is-cc-mode))
            ad-do-it
          (let ((c-inside-line-break-advice t))
            (c-indent-new-comment-line (ad-get-arg 0))))))
    ```

---

Next: [Functions](#Functions), Previous: [Control Structures](#Control-Structures), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 12 Variables [¶](#Variables-1)

A *variable* is a name used in a program to stand for a value.
In Lisp, each variable is represented by a Lisp symbol
(see [Symbols](#Symbols)). The variable name is simply the symbol’s name, and
the variable’s value is stored in the symbol’s value cell[12](#FOOT12). See [Symbol Components](#Symbol-Components). In Emacs Lisp, the use of a
symbol as a variable is independent of its use as a function name.

As previously noted in this manual, a Lisp program is represented
primarily by Lisp objects, and only secondarily as text. The textual
form of a Lisp program is given by the read syntax of the Lisp objects
that constitute the program. Hence, the textual form of a variable in
a Lisp program is written using the read syntax for the symbol
representing the variable.

* [Global Variables](#Global-Variables)
* [Variables that Never Change](#Constant-Variables)
* [Local Variables](#Local-Variables)
* [When a Variable is Void](#Void-Variables)
* [Defining Global Variables](#Defining-Variables)
* [Tips for Defining Variables Robustly](#Tips-for-Defining)
* [Accessing Variable Values](#Accessing-Variables)
* [Setting Variable Values](#Setting-Variables)
* [Running a function when a variable is changed.](#Watching-Variables)
* [Scoping Rules for Variable Bindings](#Variable-Scoping)
* [Buffer-Local Variables](#Buffer_002dLocal-Variables)
* [File Local Variables](#File-Local-Variables)
* [Directory Local Variables](#Directory-Local-Variables)
* [Connection Local Variables](#Connection-Local-Variables)
* [Variable Aliases](#Variable-Aliases)
* [Variables with Restricted Values](#Variables-with-Restricted-Values)
* [Generalized Variables](#Generalized-Variables)
* [Multisession Variables](#Multisession-Variables)

---

Next: [Variables that Never Change](#Constant-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.1 Global Variables [¶](#Global-Variables-1)

The simplest way to use a variable is *globally*. This means that
the variable has just one value at a time, and this value is in effect
(at least for the moment) throughout the Lisp system. The value remains
in effect until you specify a new one. When a new value replaces the
old one, no trace of the old value remains in the variable.

You specify a value for a symbol with `setq`. For example,

```
(setq x '(a b))
```

gives the variable `x` the value `(a b)`. Note that
`setq` is a special form (see [Special Forms](#Special-Forms)); it does not
evaluate its first argument, the name of the variable, but it does
evaluate the second argument, the new value.

Once the variable has a value, you can refer to it by using the
symbol itself as an expression. Thus,

```
x ⇒ (a b)
```

assuming the `setq` form shown above has already been executed.

If you do set the same variable again, the new value replaces the old
one:

```
x
     ⇒ (a b)
```

```
(setq x 4)
     ⇒ 4
```

```
x
     ⇒ 4
```

---

Next: [Local Variables](#Local-Variables), Previous: [Global Variables](#Global-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.2 Variables that Never Change [¶](#Variables-that-Never-Change)

In Emacs Lisp, certain symbols normally evaluate to themselves. These
include `nil` and `t`, as well as any symbol whose name starts
with ‘`:`’ (these are called *keywords*). These symbols cannot
be rebound, nor can their values be changed. Any attempt to set or bind
`nil` or `t` signals a `setting-constant` error. The
same is true for a keyword (a symbol whose name starts with ‘`:`’),
if it is interned in the standard obarray, except that setting such a
symbol to itself is not an error.

```
nil ≡ 'nil
     ⇒ nil
```

```
(setq nil 500)
error→ Attempt to set constant symbol: nil
```

Function: **keywordp** object [¶](#index-keywordp)
:   function returns `t` if object is a symbol whose name
    starts with ‘`:`’, interned in the standard obarray, and returns
    `nil` otherwise.

These constants are fundamentally different from the constants
defined using the `defconst` special form (see [Defining Global Variables](#Defining-Variables)). A `defconst` form serves to inform human readers
that you do not intend to change the value of a variable, but Emacs
does not raise an error if you actually change it.

A small number of additional symbols are made read-only for various
practical reasons. These include `enable-multibyte-characters`,
`most-positive-fixnum`, `most-negative-fixnum`, and a few
others. Any attempt to set or bind these also signals a
`setting-constant` error.

---

Next: [When a Variable is Void](#Void-Variables), Previous: [Variables that Never Change](#Constant-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.3 Local Variables [¶](#Local-Variables-1)

Global variables have values that last until explicitly superseded
with new values. Sometimes it is useful to give a variable a
*local value*—a value that takes effect only within a certain
part of a Lisp program. When a variable has a local value, we say
that it is *locally bound* to that value, and that it is a
*local variable*.

For example, when a function is called, its argument variables
receive local values, which are the actual arguments supplied to the
function call; these local bindings take effect within the body of the
function. To take another example, the `let` special form
explicitly establishes local bindings for specific variables, which
take effect only within the body of the `let` form.

We also speak of the *global binding*, which is where
(conceptually) the global value is kept.

Establishing a local binding saves away the variable’s previous
value (or lack of one). We say that the previous value is
*shadowed*. Both global and local values may be shadowed. If a
local binding is in effect, using `setq` on the local variable
stores the specified value in the local binding. When that local
binding is no longer in effect, the previously shadowed value (or lack
of one) comes back.

A variable can have more than one local binding at a time (e.g., if
there are nested `let` forms that bind the variable). The
*current binding* is the local binding that is actually in effect.
It determines the value returned by evaluating the variable symbol,
and it is the binding acted on by `setq`.

For most purposes, you can think of the current binding as the
innermost local binding, or the global binding if there is no local
binding. To be more precise, a rule called the *scoping rule*
determines where in a program a local binding takes effect. The
default scoping rule in Emacs Lisp is called *dynamic scoping*,
which simply states that the current binding at any given point in the
execution of a program is the most recently-created binding for that
variable that still exists. For details about dynamic scoping, and an
alternative scoping rule called *lexical scoping*, see [Scoping Rules for Variable Bindings](#Variable-Scoping). Lately Emacs is moving towards using lexical binding in
more and more places, with the goal of eventually making lexical
binding the default. In particular, all Emacs Lisp source files and
the `*scratch*` buffer use lexical scoping.

The special forms `let` and `let*` exist to create local
bindings:

Special Form: **let** (bindings…) forms… [¶](#index-let)
:   This special form sets up local bindings for a certain set of
    variables, as specified by bindings, and then evaluates all of
    the forms in textual order. Its return value is the value of
    the last form in forms. The local bindings set up by `let`
    will be in effect only within the body of forms.

    Each of the bindings is either (i) a symbol, in which case
    that symbol is locally bound to `nil`; or (ii) a list of the
    form `(symbol value-form)`, in which case
    symbol is locally bound to the result of evaluating
    value-form. If value-form is omitted, `nil` is used.

    All of the value-forms in bindings are evaluated in the
    order they appear and *before* binding any of the symbols to them.
    Here is an example of this: `z` is bound to the old value of
    `y`, which is 2, not the new value of `y`, which is 1.

    ```
    (setq y 2)
         ⇒ 2
    ```

    ```

    ```

    ```
    (let ((y 1)
          (z y))
      (list y z))
         ⇒ (1 2)
    ```

    On the other hand, the order of *bindings* is unspecified: in the
    following example, either 1 or 2 might be printed.

    ```
    (let ((x 1)
          (x 2))
      (print x))
    ```

    Therefore, avoid binding a variable more than once in a single
    `let` form.

Special Form: **let\*** (bindings…) forms… [¶](#index-let_002a)
:   This special form is like `let`, but it binds each variable right
    after computing its local value, before computing the local value for
    the next variable. Therefore, an expression in bindings can
    refer to the preceding symbols bound in this `let*` form.
    Compare the following example with the example above for `let`.

    ```
    (setq y 2)
         ⇒ 2
    ```

    ```

    ```

    ```
    (let* ((y 1)
           (z y))    ; Use the just-established value of y.
      (list y z))
         ⇒ (1 1)
    ```

    Basically, the `let*` binding of `x` and `y` in the
    previous example is equivalent to using nested `let` bindings:

    ```
    (let ((y 1))
      (let ((z y))
        (list y z)))
    ```

Macro: **letrec** (bindings…) forms… [¶](#index-letrec)
:   This special form is like `let*`, but all the variables are bound
    before any of the local values are computed. The values are then
    assigned to the locally bound variables. This is useful only when
    lexical binding is in effect, and you want to create closures that
    refer to bindings that would otherwise not yet be in effect when using
    `let*`.

    For instance, here’s a closure that removes itself from a hook after
    being run once:

    ```
    (letrec ((hookfun (lambda ()
                        (message "Run once")
                        (remove-hook 'post-command-hook hookfun))))
      (add-hook 'post-command-hook hookfun))
    ```

Macro: **dlet** (bindings…) forms… [¶](#index-dlet)
:   This special form is like `let`, but it binds all variables
    dynamically. This is rarely useful—you usually want to bind normal
    variables lexically, and special variables (i.e., variables that are
    defined with `defvar`) dynamically, and this is what `let`
    does.

    `dlet` can be useful when interfacing with old code that assumes
    that certain variables are dynamically bound (see [Dynamic Binding](#Dynamic-Binding)), but it’s impractical to `defvar` these variables.
    `dlet` will temporarily make the bound variables special, execute
    the forms, and then make the variables non-special again.

Macro: **named-let** name bindings &rest body [¶](#index-named_002dlet)
:   This special form is a looping construct inspired from the
    Scheme language. It is similar to `let`: It binds the variables in
    bindings, and then evaluates body. However,
    `named-let` also binds name to a
    local function whose formal arguments are the variables in bindings
    and whose body is body. This allows body to call itself
    recursively by calling
    name, where the arguments passed to name are used as the
    new values of the bound variables in the recursive invocation.

    Example of a loop summing a list of numbers:

    ```
    (named-let sum ((numbers '(1 2 3 4))
                    (running-sum 0))
      (if numbers
          (sum (cdr numbers) (+ running-sum (car numbers)))
        running-sum))
    ⇒ 10
    ```

    Recursive calls to name that occur in *tail
    positions* in body are guaranteed to be optimized as *tail
    calls*, which means that they will not consume any additional stack
    space no matter how deeply the recursion runs. Such recursive calls
    will effectively jump to the top of the loop with new values for the
    variables.

    A function call is in the tail position if it’s the very last thing
    done so that the value returned by the call is the value of body
    itself, as is the case in the recursive call to `sum` above.

    `named-let` can be used only when lexical-binding is enabled.
    See [Lexical Binding](#Lexical-Binding).

Here is a complete list of the other facilities that create local
bindings:

* Function calls (see [Functions](#Functions)).
* Macro calls (see [Macros](#Macros)).
* `condition-case` (see [Errors](#Errors)).

Variables can also have buffer-local bindings (see [Buffer-Local Variables](#Buffer_002dLocal-Variables)); a few variables have terminal-local bindings
(see [Multiple Terminals](#Multiple-Terminals)). These kinds of bindings work somewhat
like ordinary local bindings, but they are localized depending on
where you are in Emacs.

---

Next: [Defining Global Variables](#Defining-Variables), Previous: [Local Variables](#Local-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.4 When a Variable is Void [¶](#When-a-Variable-is-Void)

We say that a variable is void if its symbol has an unassigned value
cell (see [Symbol Components](#Symbol-Components)).

Under Emacs Lisp’s default dynamic scoping rule (see [Scoping Rules for Variable Bindings](#Variable-Scoping)), the value cell stores the variable’s current (local or
global) value. Note that an unassigned value cell is *not* the
same as having `nil` in the value cell. The symbol `nil` is
a Lisp object and can be the value of a variable, just as any other
object can be; but it is still a value. If a variable is void, trying
to evaluate the variable signals a `void-variable` error, instead
of returning a value.

Under the optional lexical scoping rule, the value cell holds only
the variable’s global value—the value outside of any lexical binding
construct. When a variable is lexically bound, the local value is
determined by the lexical environment; hence, variables can have local
values even if their symbols’ value cells are unassigned.

Function: **makunbound** symbol [¶](#index-makunbound)
:   This function empties out the value cell of symbol, making the
    variable void. It returns symbol.

    When applied to a variable alias (see [Variable Aliases](#Variable-Aliases)), it first
    disconnects the variable from its alias, thereby undoing the effect of
    `defvaralias`.

    If symbol has a dynamic local binding, `makunbound` voids
    the current binding, and this voidness lasts only as long as the local
    binding is in effect. Afterwards, the previously shadowed local or
    global binding is reexposed; then the variable will no longer be void,
    unless the reexposed binding is void too.

    Here are some examples (assuming dynamic binding is in effect):

    ```
    (setq x 1)               ; Put a value in the global binding.
         ⇒ 1
    (let ((x 2))             ; Locally bind it.
      (makunbound 'x)        ; Void the local binding.
      x)
    error→ Symbol's value as variable is void: x
    ```

    ```
    x                        ; The global binding is unchanged.
         ⇒ 1

    (let ((x 2))             ; Locally bind it.
      (let ((x 3))           ; And again.
        (makunbound 'x)      ; Void the innermost-local binding.
        x))                  ; And refer: it’s void.
    error→ Symbol's value as variable is void: x
    ```

    ```

    ```

    ```
    (let ((x 2))
      (let ((x 3))
        (makunbound 'x))     ; Void inner binding, then remove it.
      x)                     ; Now outer let binding is visible.
         ⇒ 2
    ```

Function: **boundp** variable [¶](#index-boundp)
:   This function returns `t` if variable (a symbol) is not
    void, and `nil` if it is void.

    Here are some examples (assuming dynamic binding is in effect):

    ```
    (boundp 'abracadabra)          ; Starts out void.
         ⇒ nil
    ```

    ```
    (let ((abracadabra 5))         ; Locally bind it.
      (boundp 'abracadabra))
         ⇒ t
    ```

    ```
    (boundp 'abracadabra)          ; Still globally void.
         ⇒ nil
    ```

    ```
    (setq abracadabra 5)           ; Make it globally nonvoid.
         ⇒ 5
    ```

    ```
    (boundp 'abracadabra)
         ⇒ t
    ```

---

Next: [Tips for Defining Variables Robustly](#Tips-for-Defining), Previous: [When a Variable is Void](#Void-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.5 Defining Global Variables [¶](#Defining-Global-Variables)

A *variable definition* is a construct that announces your
intention to use a symbol as a global variable. It uses the special
forms `defvar` or `defconst`, which are documented below.

A variable definition serves three purposes. First, it informs
people who read the code that the symbol is *intended* to be used
a certain way (as a variable). Second, it informs the Lisp system of
this, optionally supplying an initial value and a documentation
string. Third, it provides information to programming tools such as
`etags`, allowing them to find where the variable was defined.

The difference between `defconst` and `defvar` is mainly a
matter of intent, serving to inform human readers of whether the value
should ever change. Emacs Lisp does not actually prevent you from
changing the value of a variable defined with `defconst`. One
notable difference between the two forms is that `defconst`
unconditionally initializes the variable, whereas `defvar`
initializes it only if it is originally void.

To define a customizable variable, you should use `defcustom`
(which calls `defvar` as a subroutine). See [Defining Customization Variables](#Variable-Definitions).

Special Form: **defvar** symbol [value [doc-string]] [¶](#index-defvar)
:   This special form defines symbol as a variable and optionally
    initializes and documents it. Note that it doesn’t evaluate
    symbol; the symbol to be defined should appear explicitly in the
    `defvar` form. `defvar` also marks symbol as
    *special*, meaning that its bindings should always be dynamic
    (see [Scoping Rules for Variable Bindings](#Variable-Scoping)).

    If value is specified, and symbol is void (i.e., it has no
    dynamically bound value; see [When a Variable is Void](#Void-Variables)), then `defvar`
    evaluates value, and initializes symbol by setting it to the
    result of the evaluation. But if symbol is not void,
    `defvar` does not evaluate value, and leaves symbol’s
    value unchanged. If value is omitted, `defvar` doesn’t
    change the value of symbol in any case.

    Note that specifying a value, even `nil`, marks the variable as
    special permanently. Whereas if value is omitted, then
    `defvar` marks the variable special only locally (i.e. within the
    current lexical scope, or within the current file, if `defvar` is
    at the top-level). This can be useful for suppressing byte compilation
    warnings, see [Compiler Errors](#Compiler-Errors).

    If symbol has a buffer-local binding in the current buffer, and
    value is specified, `defvar` modifies the default value of
    symbol, which is buffer-independent, rather than the buffer-local
    binding. It sets the default value if the default value is void.
    See [Buffer-Local Variables](#Buffer_002dLocal-Variables).

    If symbol is already let bound (e.g., if the `defvar` form
    occurs in a `let` form), then `defvar` sets the toplevel
    default value of symbol, like `set-default-toplevel-value`.
    The let binding remains in effect until its binding construct exits.
    See [Scoping Rules for Variable Bindings](#Variable-Scoping).

    When you evaluate a top-level `defvar` form with `C-M-x`
    (`eval-defun`) or with `C-x C-e` (`eval-last-sexp`) in
    Emacs Lisp mode, a special feature of these two commands arranges to
    set the variable unconditionally, without testing whether its value is
    void.

    If the doc-string argument is supplied, it specifies the
    documentation string for the variable (stored in the symbol’s
    `variable-documentation` property). See [Documentation](#Documentation).

    Here are some examples. This form defines `foo` but does not
    initialize it:

    ```
    (defvar foo)
         ⇒ foo
    ```

    This example initializes the value of `bar` to `23`, and gives
    it a documentation string:

    ```
    (defvar bar 23
      "The normal weight of a bar.")
         ⇒ bar
    ```

    The `defvar` form returns symbol, but it is normally used
    at top level in a file where its value does not matter.

    For a more elaborate example of using `defvar` without a value,
    see [Local defvar example](#Local-defvar-example).

Special Form: **defconst** symbol value [doc-string] [¶](#index-defconst)
:   This special form defines symbol as a value and initializes it.
    It informs a person reading your code that symbol has a standard
    global value, established here, that should not be changed by the user
    or by other programs. Note that symbol is not evaluated; the
    symbol to be defined must appear explicitly in the `defconst`.

    The `defconst` form, like `defvar`, marks the variable as
    *special*, meaning that it should always be dynamically bound
    (see [Scoping Rules for Variable Bindings](#Variable-Scoping)). In addition, it marks the variable as
    risky (see [File Local Variables](#File-Local-Variables)).

    `defconst` always evaluates value, and sets the value of
    symbol to the result. If symbol does have a buffer-local
    binding in the current buffer, `defconst` sets the default value,
    not the buffer-local value. (But you should not be making
    buffer-local bindings for a symbol that is defined with
    `defconst`.)

    An example of the use of `defconst` is Emacs’s definition of
    `float-pi`—the mathematical constant *pi*, which ought not
    to be changed by anyone (attempts by the Indiana State Legislature
    notwithstanding). As the second form illustrates, however,
    `defconst` is only advisory.

    ```
    (defconst float-pi 3.141592653589793 "The value of Pi.")
         ⇒ float-pi
    ```

    ```
    (setq float-pi 3)
         ⇒ float-pi
    ```

    ```
    float-pi
         ⇒ 3
    ```

---

Next: [Accessing Variable Values](#Accessing-Variables), Previous: [Defining Global Variables](#Defining-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.6 Tips for Defining Variables Robustly [¶](#Tips-for-Defining-Variables-Robustly)

When you define a variable whose value is a function, or a list of
functions, use a name that ends in ‘`-function`’ or
‘`-functions`’, respectively.

There are several other variable name conventions;
here is a complete list:

‘`…-hook`’
:   The variable is a normal hook (see [Hooks](#Hooks)).

‘`…-function`’
:   The value is a function.

‘`…-functions`’
:   The value is a list of functions.

‘`…-form`’
:   The value is a form (an expression).

‘`…-forms`’
:   The value is a list of forms (expressions).

‘`…-predicate`’
:   The value is a predicate—a function of one argument that returns
    non-`nil` for success and `nil` for failure.

‘`…-flag`’
:   The value is significant only as to whether it is `nil` or not.
    Since such variables often end up acquiring more values over time,
    this convention is not strongly recommended.

‘`…-program`’
:   The value is a program name.

‘`…-command`’
:   The value is a whole shell command.

‘`…-switches`’
:   The value specifies options for a command.

‘`prefix--…`’ [¶](#index-internal-variables_002c-naming-conventions)
:   The variable is intended for internal use and is defined in the file
    `prefix.el`. (Emacs code contributed before 2018 may
    follow other conventions, which are being phased out.)

‘`…-internal`’
:   The variable is intended for internal use and is defined in C code.
    (Emacs code contributed before 2018 may follow other conventions,
    which are being phased out.)

When you define a variable, always consider whether you should mark
it as safe or risky; see [File Local Variables](#File-Local-Variables).

When defining and initializing a variable that holds a complicated
value (such as a syntax table for a major mode), it’s best to put the
entire computation of the value into the `defvar`, like this:

```
(defvar my-major-mode-syntax-table
  (let ((table (make-syntax-table)))
    (modify-syntax-entry ?# "<" table)
    ...
    table)
  docstring)
```

This method has several benefits. First, if the user quits while
loading the file, the variable is either still uninitialized or
initialized properly, never in-between. If it is still uninitialized,
reloading the file will initialize it properly. Second, reloading the
file once the variable is initialized will not alter it; that is
important if the user has changed its value. Third, evaluating the
`defvar` form with `C-M-x` will reinitialize the variable
completely.

---

Next: [Setting Variable Values](#Setting-Variables), Previous: [Tips for Defining Variables Robustly](#Tips-for-Defining), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.7 Accessing Variable Values [¶](#Accessing-Variable-Values)

The usual way to reference a variable is to write the symbol which
names it. See [Symbol Forms](#Symbol-Forms).

Occasionally, you may want to reference a variable which is determined
only at run time. In that case, you cannot specify the variable
name in the text of the program. You can use the `symbol-value`
function to extract the value.

Function: **symbol-value** symbol [¶](#index-symbol_002dvalue)
:   This function returns the value stored in symbol’s value cell.
    This is where the variable’s current (dynamic) value is stored. If
    the variable has no local binding, this is simply its global value.
    If the variable is void, a `void-variable` error is signaled.

    If the variable is lexically bound, the value reported by
    `symbol-value` is not necessarily the same as the variable’s
    lexical value, which is determined by the lexical environment rather
    than the symbol’s value cell. See [Scoping Rules for Variable Bindings](#Variable-Scoping).

    ```
    (setq abracadabra 5)
         ⇒ 5
    ```

    ```
    (setq foo 9)
         ⇒ 9
    ```

    ```

    ```

    ```
    ;; Here the symbol abracadabra
    ;;   is the symbol whose value is examined.
    (let ((abracadabra 'foo))
      (symbol-value 'abracadabra))
         ⇒ foo
    ```

    ```

    ```

    ```
    ;; Here, the value of abracadabra,
    ;;   which is foo,
    ;;   is the symbol whose value is examined.
    (let ((abracadabra 'foo))
      (symbol-value abracadabra))
         ⇒ 9
    ```

    ```

    ```

    ```
    (symbol-value 'abracadabra)
         ⇒ 5
    ```

---

Next: [Running a function when a variable is changed.](#Watching-Variables), Previous: [Accessing Variable Values](#Accessing-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.8 Setting Variable Values [¶](#Setting-Variable-Values)

The usual way to change the value of a variable is with the special
form `setq`. When you need to compute the choice of variable at
run time, use the function `set`.

Special Form: **setq** [symbol form]… [¶](#index-setq)
:   This special form is the most common method of changing a variable’s
    value. Each symbol is given a new value, which is the result of
    evaluating the corresponding form. The current binding of the
    symbol is changed.

    `setq` does not evaluate symbol; it sets the symbol that you
    write. We say that this argument is *automatically quoted*. The
    ‘`q`’ in `setq` stands for “quoted”.

    The value of the `setq` form is the value of the last form.

    ```
    (setq x (1+ 2))
         ⇒ 3
    ```

    ```
    x                   ; x now has a global value.
         ⇒ 3
    ```

    ```
    (let ((x 5))
      (setq x 6)        ; The local binding of x is set.
      x)
         ⇒ 6
    ```

    ```
    x                   ; The global value is unchanged.
         ⇒ 3
    ```

    Note that the first form is evaluated, then the first
    symbol is set, then the second form is evaluated, then the
    second symbol is set, and so on:

    ```
    (setq x 10          ; Notice that x is set before
          y (1+ x))     ;   the value of y is computed.
         ⇒ 11
    ```

Function: **set** symbol value [¶](#index-set)
:   This function puts value in the value cell of symbol.
    Since it is a function rather than a special form, the expression
    written for symbol is evaluated to obtain the symbol to set.
    The return value is value.

    When dynamic variable binding is in effect (the default), `set`
    has the same effect as `setq`, apart from the fact that
    `set` evaluates its symbol argument whereas `setq`
    does not. But when a variable is lexically bound, `set` affects
    its *dynamic* value, whereas `setq` affects its current
    (lexical) value. See [Scoping Rules for Variable Bindings](#Variable-Scoping).

    ```
    (set one 1)
    error→ Symbol's value as variable is void: one
    ```

    ```
    (set 'one 1)
         ⇒ 1
    ```

    ```
    (set 'two 'one)
         ⇒ one
    ```

    ```
    (set two 2)         ; two evaluates to symbol one.
         ⇒ 2
    ```

    ```
    one                 ; So it is one that was set.
         ⇒ 2
    (let ((one 1))      ; This binding of one is set,
      (set 'one 3)      ;   not the global value.
      one)
         ⇒ 3
    ```

    ```
    one
         ⇒ 2
    ```

    If symbol is not actually a symbol, a `wrong-type-argument`
    error is signaled.

    ```
    (set '(x y) 'z)
    error→ Wrong type argument: symbolp, (x y)
    ```

Macro: **setopt** [symbol form]… [¶](#index-setopt)
:   This is like `setq` (see above), but meant for user options.
    This macro uses the Customize machinery to set the variable(s)
    (see [Defining Customization Variables](#Variable-Definitions)). In particular, `setopt` will run
    the setter function associated with the variable. For instance, if
    you have:

    ```
    (defcustom my-var 1
      "My var."
      :type 'number
      :set (lambda (var val)
             (set-default var val)
             (message "We set %s to %s" var val)))
    ```

    then the following, in addition to setting `my-var` to ‘`2`’,
    will also issue a message:

    ```
    (setopt my-var 2)
    ```

    `setopt` also checks whether the value is valid for the user
    option. For instance, using `setopt` to set a user option
    defined with a `number` type to a string will signal an error.

    Unlike `defcustom` and related customization commands, such as
    `customize-variable`, `setopt` is meant for non-interactive
    use, in particular in the user init file. For that reason, it doesn’t
    record the standard, saved, and user-set values, and doesn’t mark the
    variable as candidate for saving in the custom file.

    The `setopt` macro can be used on regular, non-user option
    variables, but is much less efficient than `setq`. The main use
    case for this macro is setting user options in the user’s init file.

---

Next: [Scoping Rules for Variable Bindings](#Variable-Scoping), Previous: [Setting Variable Values](#Setting-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.9 Running a function when a variable is changed. [¶](#Running-a-function-when-a-variable-is-changed_002e)

It is sometimes useful to take some action when a variable changes its
value. The *variable watchpoint* facility provides the means to
do so. Some possible uses for this feature include keeping display in
sync with variable settings, and invoking the debugger to track down
unexpected changes to variables (see [Entering the debugger when a variable is modified](#Variable-Debugging)).

The following functions may be used to manipulate and query the watch
functions for a variable.

Function: **add-variable-watcher** symbol watch-function [¶](#index-add_002dvariable_002dwatcher)
:   This function arranges for watch-function to be called whenever
    symbol is modified. Modifications through aliases
    (see [Variable Aliases](#Variable-Aliases)) will have the same effect.

    watch-function will be called, just before changing the value of
    symbol, with 4 arguments: symbol, newval,
    operation, and where.
    symbol is the variable being changed. newval is the value
    it will be changed to. (The old value is available to
    watch-function as the value of symbol, since it was not
    yet changed to newval.) operation is a symbol
    representing the kind of change, one of: `set`, `let`,
    `unlet`, `makunbound`, or `defvaralias`. where
    is a buffer if the buffer-local value of the variable is being
    changed, `nil` otherwise.

Function: **remove-variable-watcher** symbol watch-function [¶](#index-remove_002dvariable_002dwatcher)
:   This function removes watch-function from symbol’s list of
    watchers.

Function: **get-variable-watchers** symbol [¶](#index-get_002dvariable_002dwatchers)
:   This function returns the list of symbol’s active watcher
    functions.

* [Limitations](#Limitations)

#### 12.9.1 Limitations [¶](#Limitations)

There are a couple of ways in which a variable could be modified (or at
least appear to be modified) without triggering a watchpoint.

Since watchpoints are attached to symbols, modification to the
objects contained within variables (e.g., by a list modification
function see [Modifying Existing List Structure](#Modifying-Lists)) is not caught by this mechanism.

Additionally, C code can modify the value of variables directly,
bypassing the watchpoint mechanism.

A minor limitation of this feature, again because it targets symbols,
is that only variables of dynamic scope may be watched. This poses
little difficulty, since modifications to lexical variables can be
discovered easily by inspecting the code within the scope of the
variable (unlike dynamic variables, which can be modified by any code
at all, see [Scoping Rules for Variable Bindings](#Variable-Scoping)).

---

Next: [Buffer-Local Variables](#Buffer_002dLocal-Variables), Previous: [Running a function when a variable is changed.](#Watching-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
