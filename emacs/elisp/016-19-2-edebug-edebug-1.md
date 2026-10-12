### 19.2 Edebug [¶](#Edebug-1)

Edebug is a source-level debugger for Emacs Lisp programs, with which
you can:

* Step through evaluation, stopping before and after each expression.
* Set conditional or unconditional breakpoints.
* Stop when a specified condition is true (the global break event).
* Trace slow or fast, stopping briefly at each stop point, or
  at each breakpoint.
* Display expression results and evaluate expressions as if outside of
  Edebug.
* Automatically re-evaluate a list of expressions and
  display their results each time Edebug updates the display.
* Output trace information on function calls and returns.
* Stop when an error occurs.
* Display a backtrace, omitting Edebug’s own frames.
* Specify argument evaluation for macros and defining forms.
* Obtain rudimentary coverage testing and frequency counts.

The first three sections below should tell you enough about Edebug to
start using it.

* [Using Edebug](#Using-Edebug)
* [Instrumenting for Edebug](#Instrumenting)
* [Edebug Execution Modes](#Edebug-Execution-Modes)
* [Jumping](#Jumping)
* [Miscellaneous Edebug Commands](#Edebug-Misc)
* [Breaks](#Breaks)
* [Trapping Errors](#Trapping-Errors)
* [Edebug Views](#Edebug-Views)
* [Evaluation](#Edebug-Eval)
* [Evaluation List Buffer](#Eval-List)
* [Printing in Edebug](#Printing-in-Edebug)
* [Trace Buffer](#Trace-Buffer)
* [Coverage Testing](#Coverage-Testing)
* [The Outside Context](#The-Outside-Context)
* [Edebug and Macros](#Edebug-and-Macros)
* [Edebug Options](#Edebug-Options)

---

Next: [Instrumenting for Edebug](#Instrumenting), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.1 Using Edebug [¶](#Using-Edebug-1)

To debug a Lisp program with Edebug, you must first *instrument*
the Lisp code that you want to debug. A simple way to do this is to
first move point into the definition of a function or macro and then do
`C-u C-M-x` (`eval-defun` with a prefix argument).
See [Instrumenting for Edebug](#Instrumenting), for alternative ways to instrument code.

Once a function is instrumented, any call to the function activates
Edebug. Depending on which Edebug execution mode you have selected,
activating Edebug may stop execution and let you step through the
function, or it may update the display and continue execution while
checking for debugging commands. The default execution mode is step,
which stops execution. See [Edebug Execution Modes](#Edebug-Execution-Modes).

When you are done debugging and want to run the function without
instrumentation, you can remove the instrumentation using one of the
methods described in [Instrumenting for Edebug](#Instrumenting).

Within Edebug, you normally view an Emacs buffer showing the source of
the Lisp code you are debugging. This is referred to as the *source
code buffer*, and it is temporarily read-only.

An arrow in the left fringe indicates the line where the function is
executing. Point initially shows where within the line the function is
executing, but this ceases to be true if you move point yourself.

If you instrument the definition of `fac` (shown below) and then
execute `(fac 3)`, here is what you would normally see. Point is
at the open-parenthesis before `if`.

```
(defun fac (n)
=>∗(if (< 0 n)
      (* n (fac (1- n)))
    1))
```

The places within a function where Edebug can stop execution are called
*stop points*. These occur both before and after each subexpression
that is a list, and also after each variable reference.
Here we use periods to show the stop points in the function
`fac`:

```
(defun fac (n)
  .(if .(< 0 n.).
      .(* n. .(fac .(1- n.).).).
    1).)
```

The special commands of Edebug are available in the source code buffer
in addition to the commands of Emacs Lisp mode. For example, you can
type the Edebug command `SPC` to execute until the next stop point.
If you type `SPC` once after entry to `fac`, here is the
display you will see:

```
(defun fac (n)
=>(if ∗(< 0 n)
      (* n (fac (1- n)))
    1))
```

When Edebug stops execution after an expression, it displays the
expression’s value in the echo area.

Other frequently used commands are `b` to set a breakpoint at a stop
point, `g` to execute until a breakpoint is reached, and `q` to
exit Edebug and return to the top-level command loop. Type `?` to
display a list of all Edebug commands.

---

Next: [Edebug Execution Modes](#Edebug-Execution-Modes), Previous: [Using Edebug](#Using-Edebug), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.2 Instrumenting for Edebug [¶](#Instrumenting-for-Edebug)

In order to use Edebug to debug Lisp code, you must first
*instrument* the code. Instrumenting code inserts additional code
into it, to invoke Edebug at the proper places.

When you invoke command `C-M-x` (`eval-defun`) with a
prefix argument on a function definition, it instruments the
definition before evaluating it. (This does not modify the source
code itself.) If the variable `edebug-all-defs` is
non-`nil`, that inverts the meaning of the prefix argument: in
this case, `C-M-x` instruments the definition *unless* it has
a prefix argument. The default value of `edebug-all-defs` is
`nil`. The command `M-x edebug-all-defs` toggles the value
of the variable `edebug-all-defs`.

If `edebug-all-defs` is non-`nil`, then the commands
`eval-region`, and `eval-buffer` also instrument any
definitions they evaluate. Similarly, `edebug-all-forms`
controls whether `eval-region` should instrument *any* form,
even non-defining forms. This doesn’t apply to loading or evaluations
in the minibuffer. The command `M-x edebug-all-forms` toggles
this option.

Another command, `M-x edebug-eval-top-level-form`, is available to
instrument any top-level form regardless of the values of
`edebug-all-defs` and `edebug-all-forms`.
`edebug-defun` is an alias for `edebug-eval-top-level-form`.

While Edebug is active, the command `I`
(`edebug-instrument-callee`) instruments the definition of the
function or macro called by the list form after point, if it is not already
instrumented. This is possible only if Edebug knows where to find the
source for that function; for this reason, after loading Edebug,
`eval-region` records the position of every definition it
evaluates, even if not instrumenting it. See also the `i` command
(see [Jumping](#Jumping)), which steps into the call after instrumenting the
function.

Edebug knows how to instrument all the standard special forms,
`interactive` forms with an expression argument, anonymous lambda
expressions, and other defining forms. However, Edebug cannot determine
on its own what a user-defined macro will do with the arguments of a
macro call, so you must provide that information using Edebug
specifications; for details, see [Edebug and Macros](#Edebug-and-Macros).

When Edebug is about to instrument code for the first time in a
session, it runs the hook `edebug-setup-hook`, then sets it to
`nil`. You can use this to load Edebug specifications
associated with a package you are using, but only when you use Edebug.

If Edebug detects a syntax error while instrumenting, it leaves point
at the erroneous code and signals an `invalid-read-syntax` error.
Example:

```
error→ Invalid read syntax: "Expected lambda expression"
```

One potential reason for such a failure to instrument is that some
macro definitions are not yet known to Emacs. To work around this,
load the file which defines the function you are about to instrument.

To remove instrumentation from a definition, simply re-evaluate its
definition in a way that does not instrument. There are two ways of
evaluating forms that never instrument them: from a file with
`load`, and from the minibuffer with `eval-expression`
(`M-:`). In addition, `C-M-x` without prefix argument and with
`edebug-all-defs` set to `nil` also evaluates the function’s
definition without instrumenting it.

A different way to remove the instrumentation from a definition is
to use the `edebug-remove-instrumentation` command. It also
allows removing the instrumentation from everything that has been
instrumented.

See [Evaluation](#Edebug-Eval), for other evaluation functions available
inside of Edebug.

---

Next: [Jumping](#Jumping), Previous: [Instrumenting for Edebug](#Instrumenting), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.3 Edebug Execution Modes [¶](#Edebug-Execution-Modes-1)

Edebug supports several execution modes for running the program you are
debugging. We call these alternatives *Edebug execution modes*; do
not confuse them with major or minor modes. The current Edebug execution mode
determines how far Edebug continues execution before stopping—whether
it stops at each stop point, or continues to the next breakpoint, for
example—and how much Edebug displays the progress of the evaluation
before it stops.

Normally, you specify the Edebug execution mode by typing a command to
continue the program in a certain mode. Here is a table of these
commands; all except for `S` resume execution of the program, at
least for a certain distance.

`S` [¶](#index-edebug_002dstop)
:   Stop: don’t execute any more of the program, but wait for more
    Edebug commands (`edebug-stop`).

`SPC` [¶](#index-edebug_002dstep_002dmode)
:   Step: stop at the next stop point encountered (`edebug-step-mode`).

`n` [¶](#index-edebug_002dnext_002dmode)
:   Next: stop at the next stop point encountered after an expression
    (`edebug-next-mode`). Also see `edebug-forward-sexp` in
    [Jumping](#Jumping).

`t` [¶](#index-edebug_002dtrace_002dmode)
:   Trace: pause (normally one second) at each Edebug stop point
    (`edebug-trace-mode`).

`T` [¶](#index-edebug_002dTrace_002dfast_002dmode)
:   Rapid trace: update the display at each stop point, but don’t actually
    pause (`edebug-Trace-fast-mode`).

`g` [¶](#index-edebug_002dgo_002dmode)
:   Go: run until the next breakpoint (`edebug-go-mode`). See [Edebug Breakpoints](#Breakpoints).

`c` [¶](#index-edebug_002dcontinue_002dmode)
:   Continue: pause one second at each breakpoint, and then continue
    (`edebug-continue-mode`).

`C` [¶](#index-edebug_002dContinue_002dfast_002dmode)
:   Rapid continue: move point to each breakpoint, but don’t pause
    (`edebug-Continue-fast-mode`).

`G` [¶](#index-edebug_002dGo_002dnonstop_002dmode)
:   Go non-stop: ignore breakpoints (`edebug-Go-nonstop-mode`). You
    can still stop the program by typing `S`, or any editing command.

In general, the execution modes earlier in the above list run the
program more slowly or stop sooner than the modes later in the list.

When you enter a new Edebug level, Edebug will normally stop at the
first instrumented function it encounters. If you prefer to stop only
at a break point, or not at all (for example, when gathering coverage
data), change the value of `edebug-initial-mode` from its default
`step` to `go`, or `Go-nonstop`, or one of its other
values (see [Edebug Options](#Edebug-Options)). You can do this readily with
`C-x C-a C-m` (`edebug-set-initial-mode`):

Command: **edebug-set-initial-mode** [¶](#index-edebug_002dset_002dinitial_002dmode)
:   This command, bound to `C-x C-a C-m`, sets
    `edebug-initial-mode`. It prompts you for a key to indicate the
    mode. You should enter one of the eight keys listed above, which sets
    the corresponding mode.

Note that you may reenter the same Edebug level several times if, for
example, an instrumented function is called several times from one
command.

While executing or tracing, you can interrupt the execution by typing
any Edebug command. Edebug stops the program at the next stop point and
then executes the command you typed. For example, typing `t` during
execution switches to trace mode at the next stop point. You can use
`S` to stop execution without doing anything else.

If your function happens to read input, a character you type intending
to interrupt execution may be read by the function instead. You can
avoid such unintended results by paying attention to when your program
wants input.

Keyboard macros containing the commands in this section do not
completely work: exiting from Edebug, to resume the program, loses track
of the keyboard macro. This is not easy to fix. Also, defining or
executing a keyboard macro outside of Edebug does not affect commands
inside Edebug. This is usually an advantage. See also the
`edebug-continue-kbd-macro` option in [Edebug Options](#Edebug-Options).

User Option: **edebug-sit-for-seconds** [¶](#index-edebug_002dsit_002dfor_002dseconds)
:   This option specifies how many seconds to wait between execution steps
    in trace mode or continue mode. The default is 1 second.

---

Next: [Miscellaneous Edebug Commands](#Edebug-Misc), Previous: [Edebug Execution Modes](#Edebug-Execution-Modes), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.4 Jumping [¶](#Jumping-1)

The commands described in this section execute until they reach a
specified location. All except `i` make a temporary breakpoint to
establish the place to stop, then switch to go mode (see [Edebug Execution Modes](#Edebug-Execution-Modes)). Any other breakpoint reached before the intended
stop point will also stop execution. See [Edebug Breakpoints](#Breakpoints), for the
details on breakpoints.

These commands may fail to work as expected in case of nonlocal exit,
as that can bypass the temporary breakpoint where you expected the
program to stop.

`h` [¶](#index-edebug_002dgoto_002dhere)
:   Proceed to the stop point near where point is (`edebug-goto-here`).

`f` [¶](#index-edebug_002dforward_002dsexp)
:   Run the program for one expression
    (`edebug-forward-sexp`).

`o` [¶](#index-edebug_002dstep_002dout)
:   Run the program until the end of the containing sexp (`edebug-step-out`).

`i` [¶](#index-edebug_002dstep_002din)
:   Step into the function or macro called by the form after point
    (`edebug-step-in`).

The `h` command proceeds to the stop point at or after the current
location of point, using a temporary breakpoint.

The `f` command runs the program forward over one expression. More
precisely, it sets a temporary breakpoint at the position that
`forward-sexp` would reach, then executes in go mode so that
the program will stop at breakpoints.

With a prefix argument n, the temporary breakpoint is placed
n sexps beyond point. If the containing list ends before n
more elements, then the place to stop is after the containing
expression.

You must check that the position `forward-sexp` finds is a place
that the program will really get to. In `cond`, for example,
this may not be true.

For flexibility, the `f` command does `forward-sexp` starting
at point, rather than at the stop point. If you want to execute one
expression *from the current stop point*, first type `w`
(`edebug-where`) to move point there, and then type `f`.

The `o` command continues out of an expression. It places a
temporary breakpoint at the end of the sexp containing point. If the
containing sexp is a function definition itself, `o` continues until
just before the last sexp in the definition. If that is where you are
now, it returns from the function and then stops. In other words, this
command does not exit the currently executing function unless you are
positioned after the last sexp of that function.

Normally, the `h`, `f`, and `o` commands display “Break”
and pause for `edebug-sit-for-seconds` before showing the result
of the form just evaluated. You can avoid this pause by setting
`edebug-sit-on-break` to `nil`. See [Edebug Options](#Edebug-Options).

The `i` command steps into the function or macro called by the list
form after point, and stops at its first stop point. Note that the form
need not be the one about to be evaluated. But if the form is a
function call about to be evaluated, remember to use this command before
any of the arguments are evaluated, since otherwise it will be too late.

The `i` command instruments the function or macro it’s supposed to
step into, if it isn’t instrumented already. This is convenient, but keep
in mind that the function or macro remains instrumented unless you explicitly
arrange to deinstrument it.

---

Next: [Breaks](#Breaks), Previous: [Jumping](#Jumping), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.5 Miscellaneous Edebug Commands [¶](#Miscellaneous-Edebug-Commands)

Some miscellaneous Edebug commands are described here.

`?` [¶](#index-edebug_002dhelp)
:   Display the help message for Edebug (`edebug-help`).

`a` [¶](#index-abort_002drecursive_002dedit-_0028Edebug_0029)

`C-]`
:   Abort one level back to the previous command level
    (`abort-recursive-edit`). See [Recursive Editing](#Recursive-Editing).

`q` [¶](#index-top_002dlevel-_0028Edebug_0029)
:   Return to the top level editor command loop (`top-level`). This
    exits all recursive editing levels, including all levels of Edebug
    activity. However, instrumented code protected with
    `unwind-protect` or `condition-case` forms may resume
    debugging.

`Q` [¶](#index-edebug_002dtop_002dlevel_002dnonstop)
:   Like `q`, but don’t stop even for protected code
    (`edebug-top-level-nonstop`).

`r` [¶](#index-edebug_002dprevious_002dresult)
:   Redisplay the most recently known expression result in the echo area
    (`edebug-previous-result`).

`d` [¶](#index-edebug_002dpop_002dto_002dbacktrace)
:   Display a backtrace, excluding Edebug’s own functions for clarity
    (`edebug-pop-to-backtrace`).

    See [Backtraces](#Backtraces), for a description of backtraces
    and the commands which work on them.

    If you would like to see Edebug’s functions in the backtrace,
    use `M-x edebug-backtrace-show-instrumentation`. To hide them
    again use `M-x edebug-backtrace-hide-instrumentation`.

    If a backtrace frame starts with ‘`>`’ that means that Edebug knows
    where the source code for the frame is located. Use `s` to jump
    to the source code for the current frame.

    The backtrace buffer is killed automatically when you continue
    execution.

You can invoke commands from Edebug that activate Edebug again
recursively. Whenever Edebug is active, you can quit to the top level
with `q` or abort one recursive edit level with `C-]`. You can
display a backtrace of all the pending evaluations with `d`.

---

Next: [Trapping Errors](#Trapping-Errors), Previous: [Miscellaneous Edebug Commands](#Edebug-Misc), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.6 Breaks [¶](#Breaks-1)

Edebug’s step mode (see [Edebug Execution Modes](#Edebug-Execution-Modes)) stops execution when
the next stop point is reached. There are three other ways to stop
Edebug execution once it has started: breakpoints, the global break
condition, and source breakpoints.

* [Edebug Breakpoints](#Breakpoints)
* [Global Break Condition](#Global-Break-Condition)
* [Source Breakpoints](#Source-Breakpoints)

---

Next: [Global Break Condition](#Global-Break-Condition), Up: [Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.6.1 Edebug Breakpoints [¶](#Edebug-Breakpoints)

While using Edebug, you can specify *breakpoints* in the program you
are testing: these are places where execution should stop. You can set a
breakpoint at any stop point, as defined in [Using Edebug](#Using-Edebug). For
setting and unsetting breakpoints, the stop point that is affected is
the first one at or after point in the source code buffer. Here are the
Edebug commands for breakpoints:

`b` [¶](#index-edebug_002dset_002dbreakpoint)
:   Set a breakpoint at the stop point at or after point
    (`edebug-set-breakpoint`). If you use a prefix argument, the
    breakpoint is temporary—it turns off the first time it stops the
    program. An overlay with the `edebug-enabled-breakpoint` or
    `edebug-disabled-breakpoint` faces is put at the breakpoint.

`u` [¶](#index-edebug_002dunset_002dbreakpoint)
:   Unset the breakpoint (if any) at the stop point at or after
    point (`edebug-unset-breakpoint`).

`U` [¶](#index-edebug_002dunset_002dbreakpoints)
:   Unset any breakpoints in the current form
    (`edebug-unset-breakpoints`).

`D` [¶](#index-edebug_002dtoggle_002ddisable_002dbreakpoint)
:   Toggle whether to disable the breakpoint near point
    (`edebug-toggle-disable-breakpoint`). This command is mostly
    useful if the breakpoint is conditional and it would take some work to
    recreate the condition.

`x condition RET` [¶](#index-edebug_002dset_002dconditional_002dbreakpoint)
:   Set a conditional breakpoint which stops the program only if
    evaluating condition produces a non-`nil` value
    (`edebug-set-conditional-breakpoint`). With a prefix argument,
    the breakpoint is temporary.

`X condition RET`
:   Set `edebug-global-break-condition` to condition.

`B` [¶](#index-edebug_002dnext_002dbreakpoint)
:   Move point to the next breakpoint in the current definition
    (`edebug-next-breakpoint`).

While in Edebug, you can set a breakpoint with `b` and unset one
with `u`. First move point to the Edebug stop point of your choice,
then type `b` or `u` to set or unset a breakpoint there.
Unsetting a breakpoint where none has been set has no effect.

Re-evaluating or reinstrumenting a definition removes all of its
previous breakpoints.

A *conditional breakpoint* tests a condition each time the program
gets there. Any errors that occur as a result of evaluating the
condition are ignored, as if the result were `nil`. To set a
conditional breakpoint, use `x`, and specify the condition
expression in the minibuffer. Setting a conditional breakpoint at a
stop point that has a previously established conditional breakpoint puts
the previous condition expression in the minibuffer so you can edit it.
(You can also use `X` to set the global break condition, to be
evaluated at every stop point, see [Global Break Condition](#Global-Break-Condition).)

You can make a conditional or unconditional breakpoint
*temporary* by using a prefix argument with the command to set the
breakpoint. When a temporary breakpoint stops the program, it is
automatically unset.

Edebug always stops or pauses at a breakpoint, except when the Edebug
mode is Go-nonstop. In that mode, it ignores breakpoints entirely.

To find out where your breakpoints are, use the `B` command, which
moves point to the next breakpoint following point, within the same
function, or to the first breakpoint if there are no following
breakpoints. This command does not continue execution—it just moves
point in the buffer.

---

Next: [Source Breakpoints](#Source-Breakpoints), Previous: [Edebug Breakpoints](#Breakpoints), Up: [Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.6.2 Global Break Condition [¶](#Global-Break-Condition-1)

A *global break condition* stops execution when a specified
condition is satisfied, no matter where that may occur. Edebug
evaluates the global break condition at every stop point; if it
evaluates to a non-`nil` value, then execution stops or pauses
depending on the execution mode (see [Edebug Execution Modes](#Edebug-Execution-Modes)), as if
a breakpoint had been hit. If evaluating the condition gets an error,
execution does not stop.

The condition expression is stored in
`edebug-global-break-condition`. You can specify a new expression
using the `X` command from the source code buffer while Edebug is
active, or using `C-x X X` from any buffer at any time, as long as
Edebug is loaded (`edebug-set-global-break-condition`).

The global break condition is the simplest way to find where in your
code some event occurs, but it makes code run much more slowly. So you
should reset the condition to `nil` when not using it.

---

Previous: [Global Break Condition](#Global-Break-Condition), Up: [Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.6.3 Source Breakpoints [¶](#Source-Breakpoints-1)

All breakpoints in a definition are forgotten each time you
reinstrument it. If you wish to make a breakpoint that won’t be
forgotten, you can write a *source breakpoint*, which is simply a
call to the function `edebug` in your source code. You can, of
course, make such a call conditional. For example, in the `fac`
function, you can insert the first line as shown below, to stop when the
argument reaches zero:

```
(defun fac (n)
  (if (= n 0) (edebug))
  (if (< 0 n)
      (* n (fac (1- n)))
    1))
```

When the `fac` definition is instrumented and the function is
called, the call to `edebug` acts as a breakpoint. Depending on
the execution mode (see [Edebug Execution Modes](#Edebug-Execution-Modes)), Edebug stops or
pauses there.

If no instrumented code is being executed when `edebug` is called,
that function calls `debug`.

---

Next: [Edebug Views](#Edebug-Views), Previous: [Breaks](#Breaks), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.7 Trapping Errors [¶](#Trapping-Errors-1)

Emacs normally displays an error message when an error is signaled and
not handled with `condition-case`. While Edebug is active and
executing instrumented code, it normally responds to all unhandled
errors. You can customize this with the options `edebug-on-error`
and `edebug-on-quit`; see [Edebug Options](#Edebug-Options).

When Edebug responds to an error, it shows the last stop point
encountered before the error. This may be the location of a call to a
function which was not instrumented, and within which the error actually
occurred. For an unbound variable error, the last known stop point
might be quite distant from the offending variable reference. In that
case, you might want to display a full backtrace (see [Miscellaneous Edebug Commands](#Edebug-Misc)).

If you change `debug-on-error` or `debug-on-quit` while
Edebug is active, these changes will be forgotten when Edebug becomes
inactive. Furthermore, during Edebug’s recursive edit, these variables
are bound to the values they had outside of Edebug.

---

Next: [Evaluation](#Edebug-Eval), Previous: [Trapping Errors](#Trapping-Errors), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.8 Edebug Views [¶](#Edebug-Views-1)

These Edebug commands let you view aspects of the buffer and window
status as they were before entry to Edebug. The outside window
configuration is the collection of windows and contents that were in
effect outside of Edebug.

`v` [¶](#index-edebug_002dview_002doutside)
:   Switch to viewing the outside window configuration
    (`edebug-view-outside`). Type `C-x X w` to return to Edebug.

`p` [¶](#index-edebug_002dbounce_002dpoint)
:   Temporarily display the outside current buffer with point at its
    outside position (`edebug-bounce-point`), pausing for one second
    before returning to Edebug. With a prefix argument n, pause for
    n seconds instead.

`P` [¶](#index-edebug_002dbounce_002dto_002dprevious_002dvalue)
:   Temporarily display the outside current buffer with the outside point
    corresponding to the previously-evaluated value
    (`edebug-bounce-to-previous-value`), pausing for one second
    before returning to Edebug. With a prefix argument n, pause for
    n seconds instead.

`w` [¶](#index-edebug_002dwhere)
:   Move point back to the current stop point in the source code buffer
    (`edebug-where`).

    If you use this command in a different window displaying the same
    buffer, that window will be used instead to display the current
    definition in the future.

`W` [¶](#index-edebug_002dtoggle_002dsave_002dwindows)
:   Toggle whether Edebug saves and restores the outside window
    configuration (`edebug-toggle-save-windows`).

    With a prefix argument, `W` only toggles saving and restoring of
    the selected window. To specify a window that is not displaying the
    source code buffer, you must use `C-x X W` from the global keymap.

You can view the outside window configuration with `v` or just
bounce to the point in the current buffer with `p`, even if
it is not normally displayed.

You can also bounce to buffer positions other than the current point.
Suppose you are debugging the form

```
(make-overlay beg end)
```

and you would like to know where `beg` and `end` are located
in the outside buffer. Then you could either evaluate these, for
example, with `C-x C-e`, or step over them with `n`, and
immediately after that press `P`, to bounce to the position you have
previously evaluated. The previous value for the purpose of the `P`
command is what Edebug has evaluated before its last stop point or what
you have evaluated in the context outside of Edebug, for example, with
`C-x C-e`.

After moving point, you may wish to jump back to the stop point.
You can do that with `w` from a source code buffer. You can jump
back to the stop point in the source code buffer from any buffer using
`C-x X w`.

Each time you use `W` to turn saving *off*, Edebug forgets the
saved outside window configuration—so that even if you turn saving
back *on*, the current window configuration remains unchanged when
you next exit Edebug (by continuing the program). However, the
automatic redisplay of `*edebug*` and `*edebug-trace*` may
conflict with the buffers you wish to see unless you have enough windows
open.

---

Next: [Evaluation List Buffer](#Eval-List), Previous: [Edebug Views](#Edebug-Views), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.9 Evaluation [¶](#Evaluation-2)

While within Edebug, you can evaluate expressions as if Edebug
were not running. Edebug tries to be invisible to the expression’s
evaluation and printing. Evaluation of expressions that cause side
effects will work as expected, except for changes to data that Edebug
explicitly saves and restores. See [The Outside Context](#The-Outside-Context), for details
on this process.

`e exp RET` [¶](#index-edebug_002deval_002dexpression)
:   Evaluate expression exp in the context outside of Edebug
    (`edebug-eval-expression`). That is, Edebug tries to minimize
    its interference with the evaluation. The result is shown in the echo
    area, or, if this command is given a prefix, pop up a new buffer and
    pretty-print the result there.

    By default, this command
    suppresses the debugger during evaluation, so that an error in the
    evaluated expression won’t add a new error on top of the existing one.
    Set the `debug-allow-recursive-debug` user option (see [Entering the Debugger on an Error](#Error-Debugging)) to a non-`nil` value to override this.

`M-: exp RET` [¶](#index-eval_002dexpression-_0028Edebug_0029-1)
:   Evaluate expression exp in the context of Edebug itself
    (`eval-expression`).

`C-x C-e` [¶](#index-edebug_002deval_002dlast_002dsexp)
:   Evaluate the expression before point, in the context outside of Edebug
    (`edebug-eval-last-sexp`) and show the value in the minibuffer.
    With the prefix argument of zero (`C-u 0 C-x C-e`), don’t shorten
    long items (like strings and lists) when showing the value, due to
    `edebug-print-length` and `edebug-print-level`
    (see [Printing in Edebug](#Printing-in-Edebug)). Any other prefix will result in the value
    being pretty-printed in a separate buffer instead of the minibuffer.

See [Evaluation List Buffer](#Eval-List), for additional Edebug features related to evaluating
lists of expressions interactively.

Edebug supports evaluation of expressions containing references to
lexically bound symbols created by the following constructs in
`cl-lib.el`: `cl-macrolet` and `cl-symbol-macrolet`.

---

Next: [Printing in Edebug](#Printing-in-Edebug), Previous: [Evaluation](#Edebug-Eval), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.10 Evaluation List Buffer [¶](#Evaluation-List-Buffer)

You can use the *evaluation list buffer*, called `*edebug*`, to
evaluate expressions interactively. You can also set up the
*evaluation list* of expressions to be evaluated automatically each
time Edebug updates the display.

`E` [¶](#index-edebug_002dvisit_002deval_002dlist)
:   Switch to the evaluation list buffer `*edebug*`
    (`edebug-visit-eval-list`).

In the `*edebug*` buffer you can use the commands of Lisp
Interaction mode (see [Lisp Interaction](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Lisp-Interaction) in The GNU Emacs
Manual) as well as these special commands:

`C-j` [¶](#index-edebug_002deval_002dprint_002dlast_002dsexp)
:   Evaluate the expression before point, in the outside context, and
    insert the value in the buffer (`edebug-eval-print-last-sexp`).
    With prefix argument of zero (`C-u 0 C-j`), don’t shorten long
    items (like strings and lists) due to `edebug-print-length` and
    `edebug-print-level` (see [Printing in Edebug](#Printing-in-Edebug)).

`C-x C-e` [¶](#index-edebug_002deval_002dlast_002dsexp-1)
:   Evaluate the expression before point, in the context outside of Edebug
    (`edebug-eval-last-sexp`).

`C-c C-u` [¶](#index-edebug_002dupdate_002deval_002dlist)
:   Build a new evaluation list from the contents of the buffer
    (`edebug-update-eval-list`).

`C-c C-d` [¶](#index-edebug_002ddelete_002deval_002ditem)
:   Delete the evaluation list group that point is in
    (`edebug-delete-eval-item`).

`C-c C-w`
:   Switch back to the source code buffer at the current stop point
    (`edebug-where`).

You can evaluate expressions in the evaluation list window with
`C-j` or `C-x C-e`, just as you would in `*scratch*`;
but they are evaluated in the context outside of Edebug.

The expressions you enter interactively (and their results) are lost
when you continue execution; but you can set up an *evaluation list*
consisting of expressions to be evaluated each time execution stops.

To do this, write one or more *evaluation list groups* in the
evaluation list buffer. An evaluation list group consists of one or
more Lisp expressions. Groups are separated by comment lines.

The command `C-c C-u` (`edebug-update-eval-list`) rebuilds the
evaluation list, scanning the buffer and using the first expression of
each group. (The idea is that the second expression of the group is the
value previously computed and displayed.)

Each entry to Edebug redisplays the evaluation list by inserting each
expression in the buffer, followed by its current value. It also
inserts comment lines so that each expression becomes its own group.
Thus, if you type `C-c C-u` again without changing the buffer text,
the evaluation list is effectively unchanged.

If an error occurs during an evaluation from the evaluation list,
the error message is displayed in a string as if it were the result.
Therefore, expressions using variables that are not currently valid do
not interrupt your debugging.

Here is an example of what the evaluation list window looks like after
several expressions have been added to it:

```
(current-buffer)
#<buffer *scratch*>
;---------------------------------------------------------------
```

```
(selected-window)
#<window 16 on *scratch*>
;---------------------------------------------------------------
```

```
(point)
196
;---------------------------------------------------------------
```

```
bad-var
"Symbol's value as variable is void: bad-var"
;---------------------------------------------------------------
```

```
(recursion-depth)
0
;---------------------------------------------------------------
```

```
this-command
eval-last-sexp
;---------------------------------------------------------------
```

To delete a group, move point into it and type `C-c C-d`, or simply
delete the text for the group and update the evaluation list with
`C-c C-u`. To add a new expression to the evaluation list, insert
the expression at a suitable place, insert a new comment line, then type
`C-c C-u`. You need not insert dashes in the comment line—its
contents don’t matter.

After selecting `*edebug*`, you can return to the source code
buffer with `C-c C-w` (see [Edebug Views](#Edebug-Views)). The `*edebug*`
buffer is killed when you continue execution, and recreated next time it
is needed.

---

Next: [Trace Buffer](#Trace-Buffer), Previous: [Evaluation List Buffer](#Eval-List), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.11 Printing in Edebug [¶](#Printing-in-Edebug-1)

If an expression in your program produces a value containing circular
list structure, you may get an error when Edebug attempts to print it.

One way to cope with circular structure is to set `print-length`
or `print-level` to truncate the printing. Edebug does this for
you; it binds `print-length` and `print-level` to the values
of the variables `edebug-print-length` and
`edebug-print-level` (so long as they have non-`nil`
values). See [Variables Affecting Output](#Output-Variables).

User Option: **edebug-print-length** [¶](#index-edebug_002dprint_002dlength)
:   If non-`nil`, Edebug binds `print-length` to this value while
    printing results. The default value is `50`.

User Option: **edebug-print-level** [¶](#index-edebug_002dprint_002dlevel)
:   If non-`nil`, Edebug binds `print-level` to this value while
    printing results. The default value is `50`.

You can also print circular structures and structures that share
elements more informatively by binding `print-circle`
to a non-`nil` value.

Here is an example of code that creates a circular structure:

```
(setq a (list 'x 'y))
(setcar a a)
```

If `print-circle` is non-`nil`, printing functions (e.g.,
`prin1`) will print `a` as ‘`#1=(#1# y)`’. The
‘`#1=`’ notation labels the structure that follows it with the
label ‘`1`’, and the ‘`#1#`’ notation references the previously
labeled structure. This notation is used for any shared elements of
lists or vectors.

User Option: **edebug-print-circle** [¶](#index-edebug_002dprint_002dcircle)
:   If non-`nil`, Edebug binds `print-circle` to this value while
    printing results. The default value is `t`.

For further details about how printing can be customized, see
see [Output Functions](#Output-Functions).

---

Next: [Coverage Testing](#Coverage-Testing), Previous: [Printing in Edebug](#Printing-in-Edebug), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.12 Trace Buffer [¶](#Trace-Buffer-1)

Edebug can record an execution trace, storing it in a buffer named
`*edebug-trace*`. This is a log of function calls and returns,
showing the function names and their arguments and values. To enable
trace recording, set `edebug-trace` to a non-`nil` value
(see [Edebug Options](#Edebug-Options)).

Making a trace buffer is not the same thing as using trace execution
mode (see [Edebug Execution Modes](#Edebug-Execution-Modes)).

When trace recording is enabled, each function entry and exit adds
lines to the trace buffer. A function entry record consists of
‘`::::{`’, followed by the function name and argument values. A
function exit record consists of ‘`::::}`’, followed by the function
name and result of the function.

The number of ‘`:`’s in an entry shows its recursion depth. You
can use the braces in the trace buffer to find the matching beginning or
end of function calls.

You can customize trace recording for function entry and exit by
redefining the functions `edebug-print-trace-before` and
`edebug-print-trace-after`.

Macro: **edebug-tracing** string body… [¶](#index-edebug_002dtracing)
:   This macro requests additional trace information around the execution
    of the body forms. The argument string specifies text
    to put in the trace buffer, after the ‘`{`’ or ‘`}`’. All
    the arguments are evaluated, and `edebug-tracing` returns the
    value of the last form in body.

Function: **edebug-trace** format-string &rest format-args [¶](#index-edebug_002dtrace)
:   This function inserts text in the trace buffer. It computes the text
    with `(apply 'format format-string format-args)`.
    It also appends a newline to separate entries.

`edebug-tracing` and `edebug-trace` insert lines in the
trace buffer whenever they are called, even if Edebug is not active.
Adding text to the trace buffer also scrolls its window to show the last
lines inserted.

---

Next: [The Outside Context](#The-Outside-Context), Previous: [Trace Buffer](#Trace-Buffer), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.13 Coverage Testing [¶](#Coverage-Testing-1)

Edebug provides rudimentary coverage testing and display of execution
frequency.

Coverage testing works by comparing the result of each expression with
the previous result; each form in the program is considered covered
if it has returned two different values since you began testing coverage
in the current Emacs session. Thus, to do coverage testing on your
program, execute it under various conditions and note whether it behaves
correctly; Edebug will tell you when you have tried enough different
conditions that each form has returned two different values.

Coverage testing makes execution slower, so it is only done if
`edebug-test-coverage` is non-`nil` (see [Edebug Options](#Edebug-Options)).
Frequency counting is performed for all executions of an instrumented
function, even if the execution mode is Go-nonstop, and regardless of
whether coverage testing is enabled.

Use `C-x X =` (`edebug-display-freq-count`) to display both
the coverage information and the frequency counts for a definition.
Just `=` (`edebug-temp-display-freq-count`) displays the same
information temporarily, only until you type another key.

Command: **edebug-display-freq-count** [¶](#index-edebug_002ddisplay_002dfreq_002dcount)
:   This command displays the frequency count data for each line of the
    current definition.

    It inserts frequency counts as comment lines after each line of code.
    You can undo all insertions with one `undo` command. The counts
    appear under the ‘`(`’ before an expression or the ‘`)`’ after
    an expression, or on the last character of a variable. To simplify
    the display, a count is not shown if it is equal to the count of an
    earlier expression on the same line.

    The character ‘`=`’ following the count for an expression says that
    the expression has returned the same value each time it was evaluated.
    In other words, it is not yet covered for coverage testing purposes.

    To clear the frequency count and coverage data for a definition,
    simply reinstrument it with `eval-defun`.

For example, after evaluating `(fac 5)` with a source
breakpoint, and setting `edebug-test-coverage` to `t`, when
the breakpoint is reached, the frequency data looks like this:

```
(defun fac (n)
  (if (= n 0) (edebug))
;#6           1      = =5
  (if (< 0 n)
;#5         =
      (* n (fac (1- n)))
;#    5               0
    1))
a;#   0
```

The comment lines show that `fac` was called 6 times. The
first `if` statement returned 5 times with the same result each
time; the same is true of the condition on the second `if`.
The recursive call of `fac` did not return at all.

---

Next: [Edebug and Macros](#Edebug-and-Macros), Previous: [Coverage Testing](#Coverage-Testing), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.14 The Outside Context [¶](#The-Outside-Context-1)

Edebug tries to be transparent to the program you are debugging, but it
does not succeed completely. Edebug also tries to be transparent when
you evaluate expressions with `e` or with the evaluation list
buffer, by temporarily restoring the outside context. This section
explains precisely what context Edebug restores, and how Edebug fails to
be completely transparent.

* [Checking Whether to Stop](#Checking-Whether-to-Stop)
* [Edebug Display Update](#Edebug-Display-Update)
* [Edebug Recursive Edit](#Edebug-Recursive-Edit)

---

Next: [Edebug Display Update](#Edebug-Display-Update), Up: [The Outside Context](#The-Outside-Context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.14.1 Checking Whether to Stop [¶](#Checking-Whether-to-Stop-1)

Whenever Edebug is entered, it needs to save and restore certain data
before even deciding whether to make trace information or stop the
program.

* `max-lisp-eval-depth` (see [Eval](#Eval)) is increased to reduce Edebug’s
  impact on the stack. You could, however, still run out of stack space when
  using Edebug. You can also enlarge the value of
  `edebug-max-depth` if Edebug reaches the limit of recursion depth
  instrumenting code that contains very large quoted lists.
* The state of keyboard macro execution is saved and restored. While
  Edebug is active, `executing-kbd-macro` is bound to `nil`
  unless `edebug-continue-kbd-macro` is non-`nil` (see [Edebug Options](#Edebug-Options)).

---

Next: [Edebug Recursive Edit](#Edebug-Recursive-Edit), Previous: [Checking Whether to Stop](#Checking-Whether-to-Stop), Up: [The Outside Context](#The-Outside-Context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.14.2 Edebug Display Update [¶](#Edebug-Display-Update-1)

When Edebug needs to display something (e.g., in trace mode), it saves
the current window configuration from outside Edebug
(see [Window Configurations](#Window-Configurations)). When you exit Edebug, it restores
the previous window configuration.

Emacs redisplays only when it pauses. Usually, when you continue
execution, the program re-enters Edebug at a breakpoint or after
stepping, without pausing or reading input in between. In such cases,
Emacs never gets a chance to redisplay the outside configuration.
Consequently, what you see is the same window configuration as the last
time Edebug was active, with no interruption.

Entry to Edebug for displaying something also saves and restores the
following data (though some of them are deliberately not restored if an
error or quit signal occurs).

* Which buffer is current, and the positions of point and the mark in the
  current buffer, are saved and restored.
* The outside window configuration is saved and restored if
  `edebug-save-windows` is non-`nil` (see [Edebug Options](#Edebug-Options)).
  If the value of `edebug-save-windows` is a list, only the listed
  windows are saved and restored.

  The window configuration is not restored on error or quit, but the
  outside selected window *is* reselected even on error or quit in
  case a `save-excursion` is active.

  The window start and horizontal scrolling of the source code buffer are
  not restored, however, so that the display remains coherent within Edebug.

  Saving and restoring the outside window configuration can sometimes
  change the positions of point in the buffers on which the Lisp program
  you are debugging operates, especially if your program moves point.
  If this happens and interferes with your debugging, we recommend to
  set `edebug-save-windows` to `nil`
  (see [Edebug Options](#Edebug-Options)).
* The value of point in each displayed buffer is saved and restored if
  `edebug-save-displayed-buffer-points` is non-`nil`.
* The variables `overlay-arrow-position` and
  `overlay-arrow-string` are saved and restored, so you can safely
  invoke Edebug from the recursive edit elsewhere in the same buffer.
* `cursor-in-echo-area` is locally bound to `nil` so that
  the cursor shows up in the window.

---

Previous: [Edebug Display Update](#Edebug-Display-Update), Up: [The Outside Context](#The-Outside-Context)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.14.3 Edebug Recursive Edit [¶](#Edebug-Recursive-Edit-1)

When Edebug is entered and actually reads commands from the user, it
saves (and later restores) these additional data:

* The current match data. See [The Match Data](#Match-Data).
* The variables `last-command`, `this-command`,
  `last-command-event`, `last-input-event`,
  `last-event-frame`, `last-nonmenu-event`, and
  `track-mouse`. Commands in Edebug do not affect these variables
  outside of Edebug.

  Executing commands within Edebug can change the key sequence that
  would be returned by `this-command-keys`, and there is no way to
  reset the key sequence from Lisp.

  Edebug cannot save and restore the value of
  `unread-command-events`. Entering Edebug while this variable has a
  nontrivial value can interfere with execution of the program you are
  debugging.
* Complex commands executed while in Edebug are added to the variable
  `command-history`. In rare cases this can alter execution.
* Within Edebug, the recursion depth appears one deeper than the recursion
  depth outside Edebug. This is not true of the automatically updated
  evaluation list window.
* `standard-output` and `standard-input` are bound to `nil`
  by the `recursive-edit`, but Edebug temporarily restores them during
  evaluations.
* The state of keyboard macro definition is saved and restored. While
  Edebug is active, `defining-kbd-macro` is bound to
  `edebug-continue-kbd-macro`.

---

Next: [Edebug Options](#Edebug-Options), Previous: [The Outside Context](#The-Outside-Context), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.15 Edebug and Macros [¶](#Edebug-and-Macros-1)

To make Edebug properly instrument expressions that call macros, some
extra care is needed. This subsection explains the details.

* [Instrumenting Macro Calls](#Instrumenting-Macro-Calls)
* [Specification List](#Specification-List)
* [Backtracking in Specifications](#Backtracking)
* [Specification Examples](#Specification-Examples)

---

Next: [Specification List](#Specification-List), Up: [Edebug and Macros](#Edebug-and-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.15.1 Instrumenting Macro Calls [¶](#Instrumenting-Macro-Calls-1)

When Edebug instruments an expression that calls a Lisp macro, it needs
additional information about the macro to do the job properly. This is
because there is no a-priori way to tell which subexpressions of the
macro call are forms to be evaluated. (Evaluation may occur explicitly
in the macro body, or when the resulting expansion is evaluated, or any
time later.)

Therefore, you must define an Edebug specification for each macro
that Edebug will encounter, to explain the format of calls to that
macro. To do this, add a `debug` declaration (see [The `declare` Form](#Declare-Form)) to the macro definition. Here is a simple example that shows the
specification for the `for` example macro (see [Evaluating Macro Arguments Repeatedly](#Argument-Evaluation)).

```
(defmacro for (var from init to final do &rest body)
  "Execute a simple \"for\" loop.
For example, (for i from 1 to 10 do (print i))."
  (declare (debug (symbolp "from" form "to" form "do" &rest form)))
  ...)
```

The Edebug specification says which parts of a call to the macro are
forms to be evaluated. For simple macros, the specification
often looks very similar to the formal argument list of the macro
definition, but specifications are much more general than macro
arguments. See [The `declare` Form](#Declare-Form), for more details about
the `declare` form.

Take care to ensure that the specifications are known to Edebug when
you instrument code. If you are instrumenting a function which uses a
macro defined in another file, you may first need to either evaluate
the `require` forms in the file containing your function, or
explicitly load the file containing the macro. If the definition of a
macro is wrapped by `eval-when-compile`, you may need to evaluate
it.

You can also define an edebug specification for a macro separately
from the macro definition with `def-edebug-spec`. Adding
`debug` declarations is preferred, and more convenient, for macro
definitions in Lisp, but `def-edebug-spec` makes it possible to
define Edebug specifications for special forms implemented in C.

Macro: **def-edebug-spec** macro specification [¶](#index-def_002dedebug_002dspec)
:   Specify which expressions of a call to macro macro are forms to be
    evaluated. specification should be the Edebug specification.
    Neither argument is evaluated.

    The macro argument can actually be any symbol, not just a macro
    name.

Here is a table of the possibilities for specification and how each
directs processing of arguments.

`t`
:   All arguments are instrumented for evaluation.
    This is short for `(body)`.

a symbol
:   The symbol must have an Edebug specification, which is used instead.
    This indirection is repeated until another kind of specification is
    found. This allows you to inherit the specification from another macro.

a list
:   The elements of the list describe the types of the arguments of a
    calling form. The possible elements of a specification list are
    described in the following sections.

If a macro has no Edebug specification, neither through a `debug`
declaration nor through a `def-edebug-spec` call, the variable
`edebug-eval-macro-args` comes into play.

User Option: **edebug-eval-macro-args** [¶](#index-edebug_002deval_002dmacro_002dargs)
:   This controls the way Edebug treats macro arguments with no explicit
    Edebug specification. If it is `nil` (the default), none of the
    arguments is instrumented for evaluation. Otherwise, all arguments
    are instrumented.

---

Next: [Backtracking in Specifications](#Backtracking), Previous: [Instrumenting Macro Calls](#Instrumenting-Macro-Calls), Up: [Edebug and Macros](#Edebug-and-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.15.2 Specification List [¶](#Specification-List-1)

A *specification list* is required for an Edebug specification if
some arguments of a macro call are evaluated while others are not. Some
elements in a specification list match one or more arguments, but others
modify the processing of all following elements. The latter, called
*specification keywords*, are symbols beginning with ‘`&`’ (such
as `&optional`).

A specification list may contain sublists, which match arguments that are
themselves lists, or it may contain vectors used for grouping. Sublists
and groups thus subdivide the specification list into a hierarchy of
levels. Specification keywords apply only to the remainder of the
sublist or group they are contained in.

When a specification list involves alternatives or repetition, matching
it against an actual macro call may require backtracking. For more
details, see [Backtracking in Specifications](#Backtracking).

Edebug specifications provide the power of regular expression matching,
plus some context-free grammar constructs: the matching of sublists with
balanced parentheses, recursive processing of forms, and recursion via
indirect specifications.

Here’s a table of the possible elements of a specification list, with
their meanings (see [Specification Examples](#Specification-Examples), for the referenced
examples):

`sexp`
:   A single unevaluated Lisp object, which is not instrumented.
    If the macro evaluates an argument at macro-expansion time, you should
    use `sexp` for it rather than `form`.

`form`
:   A single evaluated expression, which is instrumented. If your macro
    wraps the expression with `lambda` before it is evaluated, use
    `def-form` instead. See `def-form` below.

`place`
:   A generalized variable. See [Generalized Variables](#Generalized-Variables).

`body`
:   Short for `&rest form`. See `&rest` below. If your macro
    wraps its body of code with `lambda` before it is evaluated, use
    `def-body` instead. See `def-body` below.

`lambda-expr`
:   A lambda expression with no quoting.

`&optional`
:   All following elements in the specification list are optional; as soon
    as one does not match, Edebug stops matching at this level.

    To make just a few elements optional, followed by non-optional elements,
    use `[&optional specs…]`. To specify that several
    elements must all match or none, use `&optional
    [specs…]`. See the `defun` example.

`&rest`
:   All following elements in the specification list are repeated zero or
    more times. In the last repetition, however, it is not a problem if the
    expression runs out before matching all of the elements of the
    specification list.

    To repeat only a few elements, use `[&rest specs…]`.
    To specify several elements that must all match on every repetition, use
    `&rest [specs…]`.

`&or`
:   Each of the following elements in the specification list is an
    alternative. One of the alternatives must match, or the `&or`
    specification fails.

    Each list element following `&or` is a single alternative. To
    group two or more list elements as a single alternative, enclose them in
    `[…]`.

`&not`
:   Each of the following elements is matched as alternatives as if by using
    `&or`, but if any of them match, the specification fails. If none
    of them match, nothing is matched, but the `&not` specification
    succeeds.

`&define`
:   Indicates that the specification is for a defining form. Edebug’s
    definition of a defining form is a form containing one or more code
    forms which are saved and executed later, after the execution of the
    defining form.

    The defining form itself is not instrumented (that is, Edebug does not
    stop before and after the defining form), but forms inside it
    typically will be instrumented. The `&define` keyword should be
    the first element in a list specification.

`nil`
:   This is successful when there are no more arguments to match at the
    current argument list level; otherwise it fails. See sublist
    specifications and the backquote example.

`gate` [¶](#index-preventing-backtracking_002c-in-Edebug-specification-list)
:   No argument is matched but backtracking through the gate is disabled
    while matching the remainder of the specifications at this level. This
    is primarily used to generate more specific syntax error messages.
    See [Backtracking in Specifications](#Backtracking), for more details. Also see the `let` example.

`&error`
:   `&error` should be followed by a string, an error message, in the
    edebug-spec; it aborts the instrumentation, displaying the message in
    the minibuffer.

`&interpose`
:   Lets a function control the parsing of the remaining code.
    It takes the form `&interpose spec fun args...`
    and means that Edebug will first match spec against the code and
    then call fun with the code that matched `spec`, a parsing
    function pf, and finally args.... The parsing
    function expects a single argument indicating the specification list
    to use to parse the remaining code. It should be called exactly once
    and returns the instrumented code that fun is expected to return.
    For example `(&interpose symbolp pcase--match-pat-args)` matches
    sexps whose first element is a symbol and then lets
    `pcase--match-pat-args` lookup the specs associated
    with that head symbol according to `pcase--match-pat-args` and
    pass them to the pf it received as argument.

`other-symbol` [¶](#index-indirect-specifications)
:   Any other symbol in a specification list may be a predicate or an
    indirect specification.

    If the symbol has an Edebug specification, this *indirect
    specification* should be either a list specification that is used in
    place of the symbol, or a function that is called to process the
    arguments. The specification may be defined with
    `def-edebug-elem-spec`:

    Function: **def-edebug-elem-spec** element specification [¶](#index-def_002dedebug_002delem_002dspec)
    :   Define the specification to use in place of the symbol element.
        specification has to be a list.

    Otherwise, the symbol should be a predicate. The predicate is called
    with the argument, and if the predicate returns `nil`, the
    specification fails and the argument is not instrumented.

    Some suitable predicates include `symbolp`, `integerp`,
    `stringp`, `vectorp`, and `atom`.

`[elements…]` [¶](#index-_005b_2026_005d-_0028Edebug-specification_0029)
:   A vector of elements groups the elements into a single *group
    specification*. Its meaning has nothing to do with vectors.

`"string"`
:   The argument should be a symbol named string. This specification
    is equivalent to the quoted symbol, `'symbol`, where the name
    of symbol is the string, but the string form is preferred.

`(vector elements…)`
:   The argument should be a vector whose elements must match the
    elements in the specification. See the backquote example.

`(elements…)`
:   Any other list is a *sublist specification* and the argument must be
    a list whose elements match the specification elements.

    A sublist specification may be a dotted list and the corresponding list
    argument may then be a dotted list. Alternatively, the last CDR of a
    dotted list specification may be another sublist specification (via a
    grouping or an indirect specification, e.g., `(spec . [(more
    specs…)])`) whose elements match the non-dotted list arguments.
    This is useful in recursive specifications such as in the backquote
    example. Also see the description of a `nil` specification
    above for terminating such recursion.

    Note that a sublist specification written as `(specs . nil)`
    is equivalent to `(specs)`, and `(specs .
    (sublist-elements…))` is equivalent to `(specs
    sublist-elements…)`.

Here is a list of additional specifications that may appear only after
`&define`. See the `defun` example.

`&name`
:   Extracts the name of the current defining form from the code.
    It takes the form `&name [prestring] spec
    [poststring] fun args...` and means that Edebug will
    match spec against the code and then call fun with the
    concatenation of the current name, args..., prestring,
    the code that matched `spec`, and poststring. If fun
    is absent, it defaults to a function that concatenates the arguments
    (with an `@` between the previous name and the new).

`name`
:   The argument, a symbol, is the name of the defining form.
    Shorthand for `[&name symbolp]`.

    A defining form is not required to have a name field; and it may have
    multiple name fields.

`arg`
:   The argument, a symbol, is the name of an argument of the defining form.
    However, lambda-list keywords (symbols starting with ‘`&`’)
    are not allowed.

`lambda-list` [¶](#index-lambda_002dlist-_0028Edebug_0029)
:   This matches a lambda list—the argument list of a lambda expression.

`def-body`
:   The argument is the body of code in a definition. This is like
    `body`, described above, but a definition body must be instrumented
    with a different Edebug call that looks up information associated with
    the definition. Use `def-body` for the highest level list of forms
    within the definition.

`def-form`
:   The argument is a single, highest-level form in a definition. This is
    like `def-body`, except it is used to match a single form rather than
    a list of forms. As a special case, `def-form` also means that
    tracing information is not output when the form is executed. See the
    `interactive` example.

---

Next: [Specification Examples](#Specification-Examples), Previous: [Specification List](#Specification-List), Up: [Edebug and Macros](#Edebug-and-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.15.3 Backtracking in Specifications [¶](#Backtracking-in-Specifications)

If a specification fails to match at some point, this does not
necessarily mean a syntax error will be signaled; instead,
*backtracking* will take place until all alternatives have been
exhausted. Eventually every element of the argument list must be
matched by some element in the specification, and every required element
in the specification must match some argument.

When a syntax error is detected, it might not be reported until much
later, after higher-level alternatives have been exhausted, and with the
point positioned further from the real error. But if backtracking is
disabled when an error occurs, it can be reported immediately. Note
that backtracking is also reenabled automatically in several situations;
when a new alternative is established by `&optional`,
`&rest`, or `&or`, or at the start of processing a sublist,
group, or indirect specification. The effect of enabling or disabling
backtracking is limited to the remainder of the level currently being
processed and lower levels.

Backtracking is disabled while matching any of the
form specifications (that is, `form`, `body`, `def-form`, and
`def-body`). These specifications will match any form so any error
must be in the form itself rather than at a higher level.

Backtracking is also disabled after successfully matching a quoted
symbol, string specification, or `&define` keyword, since this
usually indicates a recognized construct. But if you have a set of
alternative constructs that all begin with the same symbol, you can
usually work around this constraint by factoring the symbol out of the
alternatives, e.g., `["foo" &or [first case] [second case] ...]`.

Most needs are satisfied by these two ways that backtracking is
automatically disabled, but occasionally it is useful to explicitly
disable backtracking by using the `gate` specification. This is
useful when you know that no higher alternatives could apply. See the
example of the `let` specification.

---

Previous: [Backtracking in Specifications](#Backtracking), Up: [Edebug and Macros](#Edebug-and-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.15.4 Specification Examples [¶](#Specification-Examples-1)

It may be easier to understand Edebug specifications by studying
the examples provided here.

Consider a hypothetical macro `my-test-generator` that runs
tests on supplied lists of data. Although it is Edebug’s default
behavior to not instrument arguments as code, as controlled by
`edebug-eval-macro-args` (see [Instrumenting Macro Calls](#Instrumenting-Macro-Calls)),
it can be useful to explicitly document that the arguments are data:

```
(def-edebug-spec my-test-generator (&rest sexp))
```

A `let` special form has a sequence of bindings and a body. Each
of the bindings is either a symbol or a sublist with a symbol and
optional expression. In the specification below, notice the `gate`
inside of the sublist to prevent backtracking once a sublist is found.

```
(def-edebug-spec let
  ((&rest
    &or symbolp (gate symbolp &optional form))
   body))
```

Edebug uses the following specifications for `defun` and the
associated argument list and `interactive` specifications. It is
necessary to handle interactive forms specially since an expression
argument is actually evaluated outside of the function body. (The
specification for `defmacro` is very similar to that for
`defun`, but allows for the `declare` statement.)

```
(def-edebug-spec defun
  (&define name lambda-list
           [&optional stringp]   ; Match the doc string, if present.
           [&optional ("interactive" interactive)]
           def-body))

(def-edebug-elem-spec 'lambda-list
  '(([&rest arg]
     [&optional ["&optional" arg &rest arg]]
     &optional ["&rest" arg]
     )))

(def-edebug-elem-spec 'interactive
  '(&optional &or stringp def-form))    ; Notice: def-form
```

The specification for backquote below illustrates how to match
dotted lists and use `nil` to terminate recursion. It also
illustrates how components of a vector may be matched. (The actual
specification defined by Edebug is a little different, and does not
support dotted lists because doing so causes very deep recursion that
could fail.)

```
(def-edebug-spec \` (backquote-form))   ; Alias just for clarity.

(def-edebug-elem-spec 'backquote-form
  '(&or ([&or "," ",@"] &or ("quote" backquote-form) form)
        (backquote-form . [&or nil backquote-form])
        (vector &rest backquote-form)
        sexp))
```

---

Previous: [Edebug and Macros](#Edebug-and-Macros), Up: [Edebug](#Edebug)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.2.16 Edebug Options [¶](#Edebug-Options-1)

These options affect the behavior of Edebug:

User Option: **edebug-setup-hook** [¶](#index-edebug_002dsetup_002dhook)
:   Functions to call before Edebug is used. Each time it is set to a new
    value, Edebug will call those functions once and then
    reset `edebug-setup-hook` to `nil`. You could use this to
    load up Edebug specifications associated with a package you are using,
    but only when you also use Edebug.
    See [Instrumenting for Edebug](#Instrumenting).

User Option: **edebug-all-defs** [¶](#index-edebug_002dall_002ddefs)
:   If this is non-`nil`, normal evaluation of defining forms such as
    `defun` and `defmacro` instruments them for Edebug. This
    applies to `eval-defun`, `eval-region` and
    `eval-buffer`.

    Use the command `M-x edebug-all-defs` to toggle the value of this
    option. See [Instrumenting for Edebug](#Instrumenting).

User Option: **edebug-all-forms** [¶](#index-edebug_002dall_002dforms)
:   If this is non-`nil`, the commands `eval-defun`,
    `eval-region` and `eval-buffer` instrument all forms, even
    those that don’t define anything. This doesn’t apply to loading or
    evaluations in the minibuffer.

    Use the command `M-x edebug-all-forms` to toggle the value of this
    option. See [Instrumenting for Edebug](#Instrumenting).

User Option: **edebug-eval-macro-args** [¶](#index-edebug_002deval_002dmacro_002dargs-1)
:   When this is non-`nil`, all macro arguments will be instrumented
    in the generated code. For any macro, the `debug` declaration
    overrides this option. So to specify exceptions for macros that have
    some arguments evaluated and some not, use the `debug` declaration
    specify an Edebug form specification.

User Option: **edebug-save-windows** [¶](#index-edebug_002dsave_002dwindows)
:   If this is non-`nil`, Edebug saves and restores the window
    configuration. That takes some time, so if your program does not care
    what happens to the window configurations, it is better to set this
    variable to `nil`. We also recommend to set this to `nil`
    if the default value causes Edebug to overwrite the positions of point
    in buffers that are involved in the program you are debugging, as
    result of saving and restoring the window configuration; this could
    happen if your program moves point in one or more of those buffers.
    Another option to try to customize in this case is
    `edebug-save-displayed-buffer-points`, described below.

    If the value of `edebug-save-windows` is a list, only the listed
    windows are saved and restored.

    You can use the `W` command in Edebug to change this variable
    interactively. See [Edebug Display Update](#Edebug-Display-Update).

User Option: **edebug-save-displayed-buffer-points** [¶](#index-edebug_002dsave_002ddisplayed_002dbuffer_002dpoints)
:   If this is non-`nil`, Edebug saves and restores point in all
    displayed buffers.

    Saving and restoring point in other buffers is necessary if you are
    debugging code that changes the point of a buffer that is displayed in
    a non-selected window. If Edebug or the user then selects the window,
    point in that buffer will move to the window’s value of point.

    Saving and restoring point in all buffers is expensive, since it
    requires selecting each window twice, so enable this only if you need
    it. See [Edebug Display Update](#Edebug-Display-Update).

User Option: **edebug-initial-mode** [¶](#index-edebug_002dinitial_002dmode)
:   If this variable is non-`nil`, it specifies the initial execution
    mode for Edebug when it is first activated. Possible values are
    `step`, `next`, `go`, `Go-nonstop`, `trace`,
    `Trace-fast`, `continue`, and `Continue-fast`.

    The default value is `step`. This variable can be set
    interactively with `C-x C-a C-m` (`edebug-set-initial-mode`).
    See [Edebug Execution Modes](#Edebug-Execution-Modes).

User Option: **edebug-trace** [¶](#index-edebug_002dtrace-1)
:   If this is non-`nil`, trace each function entry and exit.
    Tracing output is displayed in a buffer named `*edebug-trace*`, one
    function entry or exit per line, indented by the recursion level.

    Also see `edebug-tracing`, in [Trace Buffer](#Trace-Buffer).

User Option: **edebug-test-coverage** [¶](#index-edebug_002dtest_002dcoverage)
:   If non-`nil`, Edebug tests coverage of all expressions debugged.
    See [Coverage Testing](#Coverage-Testing).

User Option: **edebug-continue-kbd-macro** [¶](#index-edebug_002dcontinue_002dkbd_002dmacro)
:   If non-`nil`, continue defining or executing any keyboard macro
    that is executing outside of Edebug. Use this with caution since it is not
    debugged.
    See [Edebug Execution Modes](#Edebug-Execution-Modes).

User Option: **edebug-print-length** [¶](#index-edebug_002dprint_002dlength-1)
:   If non-`nil`, the default value of `print-length` for
    printing results in Edebug. See [Variables Affecting Output](#Output-Variables).

User Option: **edebug-print-level** [¶](#index-edebug_002dprint_002dlevel-1)
:   If non-`nil`, the default value of `print-level` for
    printing results in Edebug. See [Variables Affecting Output](#Output-Variables).

User Option: **edebug-print-circle** [¶](#index-edebug_002dprint_002dcircle-1)
:   If non-`nil`, the default value of `print-circle` for
    printing results in Edebug. See [Variables Affecting Output](#Output-Variables).

User Option: **edebug-unwrap-results** [¶](#index-edebug_002dunwrap_002dresults)
:   If non-`nil`, Edebug tries to remove any of its own
    instrumentation when showing the results of expressions. This is
    relevant when debugging macros where the results of expressions are
    themselves instrumented expressions. As a very artificial example,
    suppose that the example function `fac` has been instrumented,
    and consider a macro of the form:

    ```
    (defmacro test () "Edebug example."
      (if (symbol-function 'fac)
          ...))
    ```

    If you instrument the `test` macro and step through it, then by
    default the result of the `symbol-function` call has numerous
    `edebug-after` and `edebug-before` forms, which can make it
    difficult to see the actual result. If
    `edebug-unwrap-results` is non-`nil`, Edebug tries to remove
    these forms from the result.

User Option: **edebug-on-error** [¶](#index-edebug_002don_002derror)
:   Edebug binds `debug-on-error` to this value, if
    `debug-on-error` was previously `nil`. See [Trapping Errors](#Trapping-Errors).

User Option: **edebug-on-quit** [¶](#index-edebug_002don_002dquit)
:   Edebug binds `debug-on-quit` to this value, if
    `debug-on-quit` was previously `nil`. See [Trapping Errors](#Trapping-Errors).

If you change the values of `edebug-on-error` or
`edebug-on-quit` while Edebug is active, their values won’t be used
until the *next* time Edebug is invoked via a new command.

User Option: **edebug-global-break-condition** [¶](#index-edebug_002dglobal_002dbreak_002dcondition)
:   If non-`nil`, an expression to test for at every stop point. If
    the result is non-`nil`, then break. Errors are ignored.
    See [Global Break Condition](#Global-Break-Condition).

User Option: **edebug-sit-for-seconds** [¶](#index-edebug_002dsit_002dfor_002dseconds-1)
:   Number of seconds to pause when a breakpoint is reached and the execution
    mode is trace or continue. See [Edebug Execution Modes](#Edebug-Execution-Modes).

User Option: **edebug-sit-on-break** [¶](#index-edebug_002dsit_002don_002dbreak)
:   Whether or not to pause for `edebug-sit-for-seconds` on reaching
    a breakpoint. Set to `nil` to prevent the pause, non-`nil`
    to allow it.

User Option: **edebug-behavior-alist** [¶](#index-edebug_002dbehavior_002dalist)
:   By default, this alist contains one entry with the key `edebug`
    and a list of three functions, which are the default implementations
    of the functions inserted in instrumented code: `edebug-enter`,
    `edebug-before` and `edebug-after`. To change Edebug’s
    behavior globally, modify the default entry.

    Edebug’s behavior may also be changed on a per-definition basis by
    adding an entry to this alist, with a key of your choice and three
    functions. Then set the `edebug-behavior` symbol property of an
    instrumented definition to the key of the new entry, and Edebug will
    call the new functions in place of its own for that definition.

User Option: **edebug-new-definition-function** [¶](#index-edebug_002dnew_002ddefinition_002dfunction)
:   A function run by Edebug after it wraps the body of a definition
    or closure. After Edebug has initialized its own data, this function
    is called with one argument, the symbol associated with the
    definition, which may be the actual symbol defined or one generated by
    Edebug. This function may be used to set the `edebug-behavior`
    symbol property of each definition instrumented by Edebug.

User Option: **edebug-after-instrumentation-function** [¶](#index-edebug_002dafter_002dinstrumentation_002dfunction)
:   To inspect or modify Edebug’s instrumentation before it is used, set
    this variable to a function which takes one argument, an instrumented
    top-level form, and returns either the same or a replacement form,
    which Edebug will then use as the final result of instrumentation.

---

Next: [Test Coverage](#Test-Coverage), Previous: [Edebug](#Edebug), Up: [Debugging Lisp Programs](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
