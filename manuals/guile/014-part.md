C Function: **scm\_set\_current\_dynamic\_state** (state) [¶](#index-scm_005fset_005fcurrent_005fdynamic_005fstate)
:   Restore the saved fluid-value associations from state, replacing
    the current fluid-value associations. Return the current fluid-value
    associations as a dynamic state object, as in
    `current-dynamic-state`.

Scheme Procedure: **with-dynamic-state** state proc [¶](#index-with_002ddynamic_002dstate)

C Function: **scm\_with\_dynamic\_state** (state, proc) [¶](#index-scm_005fwith_005fdynamic_005fstate)
:   Call proc while the fluid bindings from state have been made
    current, saving the current fluid bindings. When control leaves the
    invocation of proc, restore the saved bindings, saving instead the
    fluid bindings from inside the call. If control later re-enters
    proc, restore those saved bindings, saving the current bindings,
    and so on.

C Procedure: `void` **scm\_dynwind\_current\_dynamic\_state** `(SCM state)` [¶](#index-scm_005fdynwind_005fcurrent_005fdynamic_005fstate)
:   Set the current dynamic state to state for the current dynwind
    context. Like `with-dynamic-state`, but in terms of Guile’s
    “dynwind” C API.

C Procedure: `void *` **scm\_c\_with\_dynamic\_state** `(SCM state, void *(*func)(void *), void *data)` [¶](#index-scm_005fc_005fwith_005fdynamic_005fstate)
:   Like `scm_with_dynamic_state`, but call func with
    data.

---

Next: [How to Handle Errors](#Handling-Errors), Previous: [Fluids and Dynamic States](#Fluids-and-Dynamic-States), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.12 Parameters [¶](#Parameters-1)

Parameters are Guile’s facility for dynamically bound variables.

On the most basic level, a parameter object is a procedure. Calling it
with no arguments returns its value. Calling it with one argument sets
the value.

```
(define my-param (make-parameter 123))
(my-param) ⇒ 123
(my-param 456)
(my-param) ⇒ 456
```

The `parameterize` special form establishes new locations for
parameters, those new locations having effect within the dynamic extent
of the `parameterize` body. Leaving restores the previous
locations. Re-entering (through a saved continuation) will again use
the new locations.

```
(parameterize ((my-param 789))
  (my-param)) ⇒ 789
(my-param) ⇒ 456
```

Parameters are like dynamically bound variables in other Lisp dialects.
They allow an application to establish parameter settings (as the name
suggests) just for the execution of a particular bit of code, restoring
when done. Examples of such parameters might be case-sensitivity for a
search, or a prompt for user input.

Global variables are not as good as parameter objects for this sort of
thing. Changes to them are visible to all threads, but in Guile
parameter object locations are per-thread, thereby truly limiting the
effect of `parameterize` to just its dynamic execution.

Passing arguments to functions is thread-safe, but that soon becomes
tedious when there’s more than a few or when they need to pass down
through several layers of calls before reaching the point they should
affect. Introducing a new setting to existing code is often easier with
a parameter object than adding arguments.

Scheme Procedure: **make-parameter** init [converter] [¶](#index-make_002dparameter)
:   Return a new parameter object, with initial value init.

    If a converter is given, then a call `(converter
    val)` is made for each value set, its return is the value stored.
    Such a call is made for the init initial value too.

    A converter allows values to be validated, or put into a
    canonical form. For example,

    ```
    (define my-param (make-parameter 123
                       (lambda (val)
                         (if (not (number? val))
                             (error "must be a number"))
                         (inexact->exact val))))
    (my-param 0.75)
    (my-param) ⇒ 3/4
    ```

library syntax: **parameterize** ((param value) …) body1 body2 … [¶](#index-parameterize)
:   Establish a new dynamic scope with the given params bound to new
    locations and set to the given values. body1 body2
    … is evaluated in that environment. The value returned is that of
    last body form.

    Each param is an expression which is evaluated to get the
    parameter object. Often this will just be the name of a variable
    holding the object, but it can be anything that evaluates to a
    parameter.

    The param expressions and value expressions are all
    evaluated before establishing the new dynamic bindings, and they’re
    evaluated in an unspecified order.

    For example,

    ```
    (define prompt (make-parameter "Type something: "))
    (define (get-input)
      (display (prompt))
      ...)

    (parameterize ((prompt "Type a number: "))
      (get-input)
      ...)
    ```

Parameter objects are implemented using fluids (see [Fluids and Dynamic States](#Fluids-and-Dynamic-States)), so each dynamic state has its own parameter
locations. That includes the separate locations when outside any
`parameterize` form. When a parameter is created it gets a
separate initial location in each dynamic state, all initialized to the
given init value.

New code should probably just use parameters instead of fluids, because
the interface is better. But for migrating old code or otherwise
providing interoperability, Guile provides the `fluid->parameter`
procedure:

Scheme Procedure: **fluid->parameter** fluid [conv] [¶](#index-fluid_002d_003eparameter)
:   Make a parameter that wraps a fluid.

    The value of the parameter will be the same as the value of the fluid.
    If the parameter is rebound in some dynamic extent, perhaps via
    `parameterize`, the new value will be run through the optional
    conv procedure, as with any parameter. Note that unlike
    `make-parameter`, conv is not applied to the initial value.

As alluded to above, because each thread usually has a separate dynamic
state, each thread has its own locations behind parameter objects, and
changes in one thread are not visible to any other. When a new dynamic
state or thread is created, the values of parameters in the originating
context are copied, into new locations.

Guile’s parameters conform to SRFI-39 (see [SRFI-39 - Parameters](#SRFI_002d39)).

---

Next: [Continuation Barriers](#Continuation-Barriers), Previous: [Parameters](#Parameters), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.13 How to Handle Errors [¶](#How-to-Handle-Errors)

Guile is currently in a transition from its historical `catch` and
`throw` error handling and signaling operators to the new
structured exception facility; See [Exceptions](#Exceptions). However in the
meantime, here is some documentation on errors and the older
`catch` and `throw` interface.

Errors are always thrown with a key and four arguments:

* key: a symbol which indicates the type of error. The symbols used
  by libguile are listed below.
* subr: the name of the procedure from which the error is thrown, or
  `#f`.
* message: a string (possibly language and system dependent)
  describing the error. The tokens `~A` and `~S` can be
  embedded within the message: they will be replaced with members of the
  args list when the message is printed. `~A` indicates an
  argument printed using `display`, while `~S` indicates an
  argument printed using `write`. message can also be
  `#f`, to allow it to be derived from the key by the error
  handler (may be useful if the key is to be thrown from both C and
  Scheme).
* args: a list of arguments to be used to expand `~A` and
  `~S` tokens in message. Can also be `#f` if no
  arguments are required.
* rest: a list of any additional objects required. e.g., when the
  key is `'system-error`, this contains the C errno value. Can also
  be `#f` if no additional objects are required.

In addition to `catch` and `throw`, the following Scheme
facilities are available:

Scheme Procedure: **display-error** frame port subr message args rest [¶](#index-display_002derror)

C Function: **scm\_display\_error** (frame, port, subr, message, args, rest) [¶](#index-scm_005fdisplay_005ferror)
:   Display an error message to the output port port.
    frame is the frame in which the error occurred, subr is
    the name of the procedure in which the error occurred and
    message is the actual error message, which may contain
    formatting instructions. These will format the arguments in
    the list args accordingly. rest is currently
    ignored.

The following are the error keys defined by libguile and the situations
in which they are used:

* `error-signal`: thrown after receiving an unhandled fatal signal
  such as SIGSEGV, SIGBUS, SIGFPE etc. The rest argument in the throw
  contains the coded signal number (at present this is not the same as the
  usual Unix signal number).
* `system-error`: thrown after the operating system indicates an
  error condition. The rest argument in the throw contains the
  errno value.
* `numerical-overflow`: numerical overflow.
* `out-of-range`: the arguments to a procedure do not fall within the
  accepted domain.
* `wrong-type-arg`: an argument to a procedure has the wrong type.
* `wrong-number-of-args`: a procedure was called with the wrong number
  of arguments.
* `memory-allocation-error`: memory allocation error.
* `stack-overflow`: stack overflow error.
* `regular-expression-syntax`: errors generated by the regular
  expression library.
* `misc-error`: other errors.

* [C Support](#C-Support)
* [Signaling Type Errors](#Signaling-Type-Errors)

#### 6.11.13.1 C Support [¶](#C-Support)

In the following C functions, SUBR and MESSAGE parameters
can be `NULL` to give the effect of `#f` described above.

C Function: `SCM` **scm\_error** `(SCM key, const char *subr, const char *message, SCM args, SCM rest)` [¶](#index-scm_005ferror)
:   Throw an error, as per `scm-error` (see [Procedures for Signaling Errors](#Error-Reporting)).

C Function: `void` **scm\_syserror** `(const char *subr)` [¶](#index-scm_005fsyserror)

C Function: `void` **scm\_syserror\_msg** `(const char *subr, const char *message, SCM args)` [¶](#index-scm_005fsyserror_005fmsg)
:   Throw an error with key `system-error` and supply `errno` in
    the rest argument. For `scm_syserror` the message is
    generated using `strerror`.

    Care should be taken that any code in between the failing operation
    and the call to these routines doesn’t change `errno`.

C Function: `void` **scm\_num\_overflow** `(const char *subr)` [¶](#index-scm_005fnum_005foverflow)

C Function: `void` **scm\_out\_of\_range** `(const char *subr, SCM bad_value)` [¶](#index-scm_005fout_005fof_005frange)

C Function: `void` **scm\_wrong\_num\_args** `(SCM proc)` [¶](#index-scm_005fwrong_005fnum_005fargs)

C Function: `void` **scm\_wrong\_type\_arg** `(const char *subr, int argnum, SCM bad_value)` [¶](#index-scm_005fwrong_005ftype_005farg)

C Function: `void` **scm\_wrong\_type\_arg\_msg** `(const char *subr, int argnum, SCM bad_value, const char *expected)` [¶](#index-scm_005fwrong_005ftype_005farg_005fmsg)

C Function: `void` **scm\_misc\_error** `(const char *subr, const char *message, SCM args)` [¶](#index-scm_005fmisc_005ferror)
:   Throw an error with the various keys described above.

    In `scm_wrong_num_args`, proc should be a Scheme symbol
    which is the name of the procedure incorrectly invoked. The other
    routines take the name of the invoked procedure as a C string.

    In `scm_wrong_type_arg_msg`, expected is a C string
    describing the type of argument that was expected.

    In `scm_misc_error`, message is the error message string,
    possibly containing `simple-format` escapes (see [Simple Textual Output](#Simple-Output)), and the corresponding arguments in the args list.

#### 6.11.13.2 Signaling Type Errors [¶](#Signaling-Type-Errors)

Every function visible at the Scheme level should aggressively check the
types of its arguments, to avoid misinterpreting a value, and perhaps
causing a segmentation fault. Guile provides some macros to make this
easier.

Macro: `void` **SCM\_ASSERT** `(int test, SCM obj, unsigned int position, const char *subr)` [¶](#index-SCM_005fASSERT)

Macro: `void` **SCM\_ASSERT\_TYPE** `(int test, SCM obj, unsigned int position, const char *subr, const char *expected)` [¶](#index-SCM_005fASSERT_005fTYPE)
:   If test is zero, signal a “wrong type argument” error,
    attributed to the subroutine named subr, operating on the value
    obj, which is the position’th argument of subr.

    In `SCM_ASSERT_TYPE`, expected is a C string describing the
    type of argument that was expected.

Macro: `int` **SCM\_ARG1** [¶](#index-SCM_005fARG1)

Macro: `int` **SCM\_ARG2** [¶](#index-SCM_005fARG2)

Macro: `int` **SCM\_ARG3** [¶](#index-SCM_005fARG3)

Macro: `int` **SCM\_ARG4** [¶](#index-SCM_005fARG4)

Macro: `int` **SCM\_ARG5** [¶](#index-SCM_005fARG5)

Macro: `int` **SCM\_ARG6** [¶](#index-SCM_005fARG6)

Macro: `int` **SCM\_ARG7** [¶](#index-SCM_005fARG7)
:   One of the above values can be used for position to indicate the
    number of the argument of subr which is being checked.
    Alternatively, a positive integer number can be used, which allows to
    check arguments after the seventh. However, for parameter numbers up to
    seven it is preferable to use `SCM_ARGN` instead of the
    corresponding raw number, since it will make the code easier to
    understand.

Macro: `int` **SCM\_ARGn** [¶](#index-SCM_005fARGn)
:   Passing a value of zero or `SCM_ARGn` for position allows to
    leave it unspecified which argument’s type is incorrect. Again,
    `SCM_ARGn` should be preferred over a raw zero constant.

---

Previous: [How to Handle Errors](#Handling-Errors), Up: [Controlling the Flow of Program Execution](#Control-Mechanisms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.11.14 Continuation Barriers [¶](#Continuation-Barriers-1)

The non-local flow of control caused by continuations might sometimes
not be wanted. You can use `with-continuation-barrier` to erect
fences that continuations can not pass.

Scheme Procedure: **with-continuation-barrier** proc [¶](#index-with_002dcontinuation_002dbarrier)

C Function: **scm\_with\_continuation\_barrier** (proc) [¶](#index-scm_005fwith_005fcontinuation_005fbarrier)
:   Call proc and return its result. Do not allow the invocation of
    continuations that would leave or enter the dynamic extent of the call
    to `with-continuation-barrier`. Such an attempt causes an error
    to be signaled.

    Throws (such as errors) that are not caught from within proc are
    caught by `with-continuation-barrier`. In that case, a short
    message is printed to the current error port and `#f` is returned.

    Thus, `with-continuation-barrier` returns exactly once.

C Function: `void *` **scm\_c\_with\_continuation\_barrier** `(void *(*func) (void *), void *data)` [¶](#index-scm_005fc_005fwith_005fcontinuation_005fbarrier)
:   Like `scm_with_continuation_barrier` but call func on
    data. When an error is caught, `NULL` is returned.

---

Next: [Regular Expressions](#Regular-Expressions), Previous: [Controlling the Flow of Program Execution](#Control-Mechanisms), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
