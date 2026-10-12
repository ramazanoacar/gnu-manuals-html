### 19.1 The Lisp Debugger [¶](#The-Lisp-Debugger)

The ordinary *Lisp debugger* provides the ability to suspend
evaluation of a form. While evaluation is suspended (a state that is
commonly known as a *break*), you may examine the run time stack,
examine the values of local or global variables, or change those values.
Since a break is a recursive edit, all the usual editing facilities of
Emacs are available; you can even run programs that will enter the
debugger recursively. See [Recursive Editing](#Recursive-Editing).

* [Entering the Debugger on an Error](#Error-Debugging)
* [Debugging Redisplay Errors](#Debugging-Redisplay)
* [Debugging Infinite Loops](#Infinite-Loops)
* [Entering the Debugger on a Function Call](#Function-Debugging)
* [Entering the debugger when a variable is modified](#Variable-Debugging)
* [Explicit Entry to the Debugger](#Explicit-Debug)
* [Using the Debugger](#Using-Debugger)
* [Backtraces](#Backtraces)
* [Debugger Commands](#Debugger-Commands)
* [Invoking the Debugger](#Invoking-the-Debugger)
* [Internals of the Debugger](#Internals-of-Debugger)

---

Next: [Debugging Redisplay Errors](#Debugging-Redisplay), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.1 Entering the Debugger on an Error [¶](#Entering-the-Debugger-on-an-Error)

The most important time to enter the debugger is when a Lisp error
happens. This allows you to investigate the immediate causes of the
error.

However, entry to the debugger is not a normal consequence of an
error. Many commands signal Lisp errors when invoked inappropriately,
and during ordinary editing it would be very inconvenient to enter the
debugger each time this happens. So if you want errors to enter the
debugger, set the variable `debug-on-error` to non-`nil`.
(The command `toggle-debug-on-error` provides an easy way to do
this.)

Note that, for technical reasons, you cannot use the facilities
defined in this subsection to debug errors in Lisp that the redisplay
code has invoked. See [Debugging Redisplay Errors](#Debugging-Redisplay), for help with these.

User Option: **debug-on-error** [¶](#index-debug_002don_002derror)
:   This variable determines whether the debugger is called when an error
    is signaled and not handled. If `debug-on-error` is `t`,
    all kinds of errors call the debugger, except those listed in
    `debug-ignored-errors` (see below). If it is `nil`, none
    call the debugger.

    The value can also be a list of error conditions (see [How to Signal an Error](#Signaling-Errors)). Then the debugger is called only for error conditions in
    this list (except those also listed in `debug-ignored-errors`).
    For example, if you set `debug-on-error` to the list
    `(void-variable)`, the debugger is only called for errors about a
    variable that has no value.

    Note that `eval-expression-debug-on-error` overrides this
    variable in some cases; see below.

    When this variable is non-`nil`, Emacs does not create an error
    handler around process filter functions and sentinels. Therefore,
    errors in these functions also invoke the debugger. See [Processes](#Processes).

User Option: **debug-ignored-errors** [¶](#index-debug_002dignored_002derrors)
:   This variable specifies errors which should not enter the debugger,
    regardless of the value of `debug-on-error`. Its value is a list
    of error condition symbols and/or regular expressions. If the error
    has any of those condition symbols, or if the error message matches
    any of the regular expressions, then that error does not enter the
    debugger.

    The normal value of this variable includes `user-error`, as well
    as several errors that happen often during editing but rarely result
    from bugs in Lisp programs. However, “rarely” is not “never”; if
    your program fails with an error that matches this list, you may try
    changing this list to debug the error. The easiest way is usually to
    set `debug-ignored-errors` to `nil`.

User Option: **eval-expression-debug-on-error** [¶](#index-eval_002dexpression_002ddebug_002don_002derror)
:   If this variable has a non-`nil` value (the default), running the
    command `eval-expression` causes `debug-on-error` to be
    temporarily bound to `t`. See [Evaluating
    Emacs Lisp Expressions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Lisp-Eval) in The GNU Emacs Manual.

    If `eval-expression-debug-on-error` is `nil`, then the value
    of `debug-on-error` is not changed during `eval-expression`.

User Option: **debug-on-signal** [¶](#index-debug_002don_002dsignal)
:   Normally, errors caught by `condition-case` never invoke the
    debugger. The `condition-case` gets a chance to handle the error
    before the debugger gets a chance.

    If you change `debug-on-signal` to a non-`nil` value, the
    debugger gets the first chance at every error, regardless of the
    presence of `condition-case`. (To invoke the debugger, the error
    must still fulfill the criteria specified by `debug-on-error` and
    `debug-ignored-errors`.)

    For example, setting this variable is useful to get a backtrace from
    code evaluated by emacsclient’s `--eval` option. If Lisp code
    evaluated by emacsclient signals an error while this variable is
    non-`nil`, the backtrace will popup in the running Emacs.

    **Warning:** Setting this variable to non-`nil` may have
    annoying effects. Various parts of Emacs catch errors in the normal
    course of affairs, and you may not even realize that errors happen
    there. If you need to debug code wrapped in `condition-case`,
    consider using `condition-case-unless-debug` (see [Writing Code to Handle Errors](#Handling-Errors)).

User Option: **debug-on-event** [¶](#index-debug_002don_002devent)
:   If you set `debug-on-event` to a special event (see [Special Events](#Special-Events)), Emacs will try to enter the debugger as soon as it receives
    this event, bypassing `special-event-map`. At present, the only
    supported values correspond to the signals `SIGUSR1` and
    `SIGUSR2` (this is the default). This can be helpful when
    `inhibit-quit` is set and Emacs is not otherwise responding.

Variable: **debug-on-message** [¶](#index-debug_002don_002dmessage)
:   If you set `debug-on-message` to a regular expression,
    Emacs will enter the debugger if it displays a matching message in the
    echo area. For example, this can be useful when trying to find the
    cause of a particular message.

Variable: **debug-allow-recursive-debug** [¶](#index-debug_002dallow_002drecursive_002ddebug)
:   You can evaluate forms in the current stack frame in the
    ‘`*Backtrace*`’ buffer with the `e` command, and while
    edebugging you can use the `e` and `C-x C-e` commands to do
    something similar. By default, the debugger is inhibited by these
    commands (because (re-)entering the debugger at this point will
    usually take you out of the debugging context you’re in). Set
    `debug-allow-recursive-debug` to a non-`nil` value to allow
    these commands to enter the debugger recursively.

To debug an error that happens during loading of the init
file, use the option ‘`--debug-init`’. This binds
`debug-on-error` to `t` while loading the init file, and
bypasses the `condition-case` which normally catches errors in the
init file.

---

Next: [Debugging Infinite Loops](#Infinite-Loops), Previous: [Entering the Debugger on an Error](#Error-Debugging), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.2 Debugging Redisplay Errors [¶](#Debugging-Redisplay-Errors)

When an error occurs in Lisp code which redisplay has invoked, Emacs’s
usual debugging mechanisms are unusable, for technical reasons. This
subsection describes how to get a backtrace from such an error, which
should be helpful in debugging it.

These directions apply to Lisp forms used, for example, in
`:eval` mode line constructs (see [The Data Structure of the Mode Line](#Mode-Line-Data)), and in all
hooks invoked from redisplay, such as:

* `fontification-functions` (see [Automatic Face Assignment](#Auto-Faces)).
* `window-scroll-functions` (see [Hooks for Window Scrolling and Changes](#Window-Hooks)).

Note that if you have had an error in a hook function called from
redisplay, the error handling might have removed this function from
the hook. You will thus need to reinitialize that hook somehow,
perhaps with `add-hook`, to be able to replay the bug.

To generate a backtrace in these circumstances, set the variable
`backtrace-on-redisplay-error` to non-`nil`. When the error
occurs, Emacs will dump the backtrace to the buffer
`*Redisplay-trace*`, but won’t automatically display it in a
window. This is to avoid needlessly corrupting the redisplay you are
debugging. You will thus need to display the buffer yourself, with a
command such as `switch-to-buffer-other-frame` `C-x 5 b`.

Variable: **backtrace-on-redisplay-error** [¶](#index-backtrace_002don_002dredisplay_002derror)
:   Set this variable to non-`nil` to enable the generation of a
    backtrace when an error occurs in any Lisp called from redisplay.

---

Next: [Entering the Debugger on a Function Call](#Function-Debugging), Previous: [Debugging Redisplay Errors](#Debugging-Redisplay), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.3 Debugging Infinite Loops [¶](#Debugging-Infinite-Loops)

When a program loops infinitely and fails to return, your first
problem is to stop the loop. On most operating systems, you can do
this with `C-g`, which causes a *quit*. See [Quitting](#Quitting).

Ordinary quitting gives no information about why the program was
looping. To get more information, you can set the variable
`debug-on-quit` to non-`nil`. Once you have the debugger
running in the middle of the infinite loop, you can proceed from the
debugger using the stepping commands. If you step through the entire
loop, you may get enough information to solve the problem.

Quitting with `C-g` is not considered an error, and
`debug-on-error` has no effect on the handling of `C-g`.
Likewise, `debug-on-quit` has no effect on errors.

User Option: **debug-on-quit** [¶](#index-debug_002don_002dquit)
:   This variable determines whether the debugger is called when
    `quit` is signaled and not handled. If `debug-on-quit` is
    non-`nil`, then the debugger is called whenever you quit (that
    is, type `C-g`). If `debug-on-quit` is `nil` (the
    default), then the debugger is not called when you quit.

---

Next: [Entering the debugger when a variable is modified](#Variable-Debugging), Previous: [Debugging Infinite Loops](#Infinite-Loops), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.4 Entering the Debugger on a Function Call [¶](#Entering-the-Debugger-on-a-Function-Call)

To investigate a problem that happens in the middle of a program, one
useful technique is to enter the debugger whenever a certain function is
called. You can do this to the function in which the problem occurs,
and then step through the function, or you can do this to a function
called shortly before the problem, step quickly over the call to that
function, and then step through its caller.

Command: **debug-on-entry** function-name [¶](#index-debug_002don_002dentry)
:   This function requests function-name to invoke the debugger each
    time it is called.

    Any function or macro defined as Lisp code may be set to break on
    entry, regardless of whether it is interpreted code or compiled code.
    If the function is a command, it will enter the debugger when called
    from Lisp and when called interactively (after the reading of the
    arguments). You can also set debug-on-entry for primitive functions
    (i.e., those written in C) this way, but it only takes effect when the
    primitive is called from Lisp code. Debug-on-entry is not allowed for
    special forms.

    When `debug-on-entry` is called interactively, it prompts for
    function-name in the minibuffer. If the function is already set
    up to invoke the debugger on entry, `debug-on-entry` does nothing.
    `debug-on-entry` always returns function-name.

    Here’s an example to illustrate use of this function:

    ```
    (defun fact (n)
      (if (zerop n) 1
          (* n (fact (1- n)))))
         ⇒ fact
    ```

    ```
    (debug-on-entry 'fact)
         ⇒ fact
    ```

    ```
    (fact 3)
    ```

    ```

    ```

    ```
    ------ Buffer: *Backtrace* ------
    Debugger entered--entering a function:
    * fact(3)
      eval((fact 3))
      eval-last-sexp-1(nil)
      eval-last-sexp(nil)
      call-interactively(eval-last-sexp)
    ------ Buffer: *Backtrace* ------
    ```

    ```

    ```

Command: **cancel-debug-on-entry** &optional function-name [¶](#index-cancel_002ddebug_002don_002dentry)
:   This function undoes the effect of `debug-on-entry` on
    function-name. When called interactively, it prompts for
    function-name in the minibuffer. If function-name is
    omitted or `nil`, it cancels break-on-entry for all functions.
    Calling `cancel-debug-on-entry` does nothing to a function which is
    not currently set up to break on entry.

---

Next: [Explicit Entry to the Debugger](#Explicit-Debug), Previous: [Entering the Debugger on a Function Call](#Function-Debugging), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.5 Entering the debugger when a variable is modified [¶](#Entering-the-debugger-when-a-variable-is-modified)

Sometimes a problem with a function is due to a wrong setting of a
variable. Setting up the debugger to trigger whenever the variable is
changed is a quick way to find the origin of the setting.

Command: **debug-on-variable-change** variable [¶](#index-debug_002don_002dvariable_002dchange)
:   This function arranges for the debugger to be called whenever
    variable is modified.

    It is implemented using the watchpoint mechanism, so it inherits the
    same characteristics and limitations: all aliases of variable
    will be watched together, only dynamic variables can be watched, and
    changes to the objects referenced by variables are not detected. For
    details, see [Running a function when a variable is changed.](#Watching-Variables).

Command: **cancel-debug-on-variable-change** &optional variable [¶](#index-cancel_002ddebug_002don_002dvariable_002dchange)
:   This function undoes the effect of `debug-on-variable-change` on
    variable. When called interactively, it prompts for
    variable in the minibuffer. If variable is omitted or
    `nil`, it cancels break-on-change for all variables. Calling
    `cancel-debug-on-variable-change` does nothing to a variable
    which is not currently set up to break on change.

---

Next: [Using the Debugger](#Using-Debugger), Previous: [Entering the debugger when a variable is modified](#Variable-Debugging), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.6 Explicit Entry to the Debugger [¶](#Explicit-Entry-to-the-Debugger)

You can cause the debugger to be called at a certain point in your
program by writing the expression `(debug)` at that point. To do
this, visit the source file, insert the text ‘`(debug)`’ at the
proper place, and type `C-M-x` (`eval-defun`, a Lisp mode key
binding). **Warning:** if you do this for temporary debugging
purposes, be sure to undo this insertion before you save the file!

The place where you insert ‘`(debug)`’ must be a place where an
additional form can be evaluated and its value ignored. (If the value
of `(debug)` isn’t ignored, it will alter the execution of the
program!) The most common suitable places are inside a `progn` or
an implicit `progn` (see [Sequencing](#Sequencing)).

If you don’t know exactly where in the source code you want to put
the debug statement, but you want to display a backtrace when a
certain message is displayed, you can set `debug-on-message` to a
regular expression matching the desired message.

---

Next: [Backtraces](#Backtraces), Previous: [Explicit Entry to the Debugger](#Explicit-Debug), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.7 Using the Debugger [¶](#Using-the-Debugger)

When the debugger is entered, it displays the previously selected
buffer in one window and a buffer named `*Backtrace*` in another
window. The backtrace buffer contains one line for each level of Lisp
function execution currently going on. At the beginning of this buffer
is a message describing the reason that the debugger was invoked (such
as the error message and associated data, if it was invoked due to an
error).

The backtrace buffer is read-only and uses a special major mode,
Debugger mode, in which letters are defined as debugger commands. The
usual Emacs editing commands are available; thus, you can switch windows
to examine the buffer that was being edited at the time of the error,
switch buffers, visit files, or do any other sort of editing. However,
the debugger is a recursive editing level (see [Recursive Editing](#Recursive-Editing))
and it is wise to go back to the backtrace buffer and exit the debugger
(with the `q` command) when you are finished with it. Exiting
the debugger gets out of the recursive edit and buries the backtrace
buffer. (You can customize what the `q` command does with the
backtrace buffer by setting the variable `debugger-bury-or-kill`.
For example, set it to `kill` if you prefer to kill the buffer
rather than bury it. Consult the variable’s documentation for more
possibilities.)

When the debugger has been entered, the `debug-on-error`
variable is temporarily set according to
`eval-expression-debug-on-error`. If the latter variable is
non-`nil`, `debug-on-error` will temporarily be set to
`t`. However, further errors that occur while debugging won’t
(by default) trigger another debugger, because `inhibit-debugger`
will also be bound to non-`nil`.

The debugger itself must be run byte-compiled, since it makes
assumptions about the state of the Lisp interpreter. These
assumptions are false if the debugger is running interpreted.

---

Next: [Debugger Commands](#Debugger-Commands), Previous: [Using the Debugger](#Using-Debugger), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.8 Backtraces [¶](#Backtraces-1)

Debugger mode is derived from Backtrace mode, which is also used to
show backtraces by Edebug and ERT. (see [Edebug](#Edebug), and [the
ERT manual](https://www.gnu.org/software/emacs/manual/html_mono/ert.html#Top) in ERT: Emacs Lisp Regression Testing.)

The backtrace buffer shows you the functions that are executing and
their argument values. When a backtrace buffer is created, it shows
each stack frame on one, possibly very long, line. (A stack frame is
the place where the Lisp interpreter records information about a
particular invocation of a function.) The most recently called
function will be at the top.

In a backtrace you can specify a stack frame by moving point to a line
describing that frame. The frame whose line point is on is considered
the *current frame*.

If a function name is underlined, that means Emacs knows where its
source code is located. You can click with the mouse on that name, or
move to it and type `RET`, to visit the source code. You can also
type `RET` while point is on any name of a function or variable
which is not underlined, to see help information for that symbol in a
help buffer, if any exists. The `xref-find-definitions` command,
bound to `M-.`, can also be used on any identifier in a backtrace
(see [Looking Up Identifiers](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Looking-Up-Identifiers) in The GNU Emacs Manual).

In backtraces, the tails of long lists and the ends of long strings,
vectors or structures, as well as objects which are deeply nested,
will be printed as underlined “...”. You can click with the mouse
on a “...”, or type `RET` while point is on it, to show the part
of the object that was hidden. To control how much abbreviation is
done, customize `backtrace-line-length`.

Here is a list of commands for navigating and viewing backtraces:

`v`
:   Toggle the display of local variables of the current stack frame.

`p`
:   Move to the beginning of the frame, or to the beginning
    of the previous frame.

`n`
:   Move to the beginning of the next frame.

`+`
:   Add line breaks and indentation to the top-level Lisp form at point to
    make it more readable.

`-`
:   Collapse the top-level Lisp form at point back to a single line.

`#`
:   Toggle `print-circle` for the frame at point.

`:`
:   Toggle `print-gensym` for the frame at point.

`.`
:   Expand all the forms abbreviated with “...” in the frame at point.

---

Next: [Invoking the Debugger](#Invoking-the-Debugger), Previous: [Backtraces](#Backtraces), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.9 Debugger Commands [¶](#Debugger-Commands-1)

The debugger buffer (in Debugger mode) provides special commands in
addition to the usual Emacs commands and to the Backtrace mode commands
described in the previous section. The most important use of
debugger commands is for stepping through code, so that you can see
how control flows. The debugger can step through the control
structures of an interpreted function, but cannot do so in a
byte-compiled function. If you would like to step through a
byte-compiled function, replace it with an interpreted definition of
the same function. (To do this, visit the source for the function and
type `C-M-x` on its definition.) You cannot use the Lisp debugger
to step through a primitive function.

Some of the debugger commands operate on the current frame. If a
frame starts with a star, that means that exiting that frame will call the
debugger again. This is useful for examining the return value of a
function.

Here is a list of Debugger mode commands:

`c`
:   Exit the debugger and continue execution. This resumes execution of
    the program as if the debugger had never been entered (aside from any
    side-effects that you caused by changing variable values or data
    structures while inside the debugger).

`d`
:   Continue execution, but enter the debugger the next time any Lisp
    function is called. This allows you to step through the
    subexpressions of an expression, seeing what values the subexpressions
    compute, and what else they do.

    The stack frame made for the function call which enters the debugger in
    this way will be flagged automatically so that the debugger will be
    called again when the frame is exited. You can use the `u` command
    to cancel this flag.

`b`
:   Flag the current frame so that the debugger will be entered when the
    frame is exited. Frames flagged in this way are marked with stars
    in the backtrace buffer.

`u`
:   Don’t enter the debugger when the current frame is exited. This
    cancels a `b` command on that frame. The visible effect is to
    remove the star from the line in the backtrace buffer.

`j`
:   Flag the current frame like `b`. Then continue execution like
    `c`, but temporarily disable break-on-entry for all functions that
    are set up to do so by `debug-on-entry`.

`e` [¶](#index-debug_002dallow_002drecursive_002ddebug-1)
:   Read a Lisp expression in the minibuffer, evaluate it (with the
    relevant lexical environment, if applicable), and print the
    value in the echo area. The debugger alters certain important
    variables, and the current buffer, as part of its operation; `e`
    temporarily restores their values from outside the debugger, so you can
    examine and change them. This makes the debugger more transparent. By
    contrast, `M-:` does nothing special in the debugger; it shows you
    the variable values within the debugger. By default, this command
    suppresses the debugger during evaluation, so that an error in the
    evaluated expression won’t add a new error on top of the existing one.
    Set the `debug-allow-recursive-debug` user option to a
    non-`nil` value to override this.

`R`
:   Like `e`, but also save the result of evaluation in the
    buffer `*Debugger-record*`.

`q`
:   Terminate the program being debugged; return to top-level Emacs
    command execution.

    If the debugger was entered due to a `C-g` but you really want
    to quit, and not debug, use the `q` command.

`r`
:   Return a value from the debugger. The value is computed by reading an
    expression with the minibuffer and evaluating it.

    The `r` command is useful when the debugger was invoked due to exit
    from a Lisp call frame (as requested with `b` or by entering the
    frame with `d`); then the value specified in the `r` command is
    used as the value of that frame. It is also useful if you call
    `debug` and use its return value. Otherwise, `r` has the same
    effect as `c`, and the specified return value does not matter.

    You can’t use `r` when the debugger was entered due to an error.

`l`
:   Display a list of functions that will invoke the debugger when called.
    This is a list of functions that are set to break on entry by means of
    `debug-on-entry`.

---

Next: [Internals of the Debugger](#Internals-of-Debugger), Previous: [Debugger Commands](#Debugger-Commands), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.10 Invoking the Debugger [¶](#Invoking-the-Debugger-1)

Here we describe in full detail the function `debug` that is used
to invoke the debugger.

Command: **debug** &rest debugger-args [¶](#index-debug)
:   This function enters the debugger. In interactive sessions, it
    switches to a buffer named `*Backtrace*` (or
    `*Backtrace*<2>` if it is the second recursive entry to the
    debugger, etc.), and fills it with information about the stack of Lisp
    function calls. It then enters a recursive edit, showing the
    backtrace buffer in Debugger mode. In batch mode (more generally,
    when `noninteractive` is non-`nil`, see [Batch Mode](#Batch-Mode)),
    this function shows the Lisp backtrace on the standard error stream.
    Binding `backtrace-on-error-noninteractive` to `nil`
    suppresses the backtrace in batch mode, see below.

    The Debugger mode `c`, `d`, `j`, and `r` commands exit
    the recursive edit; then `debug` switches back to the previous
    buffer and returns to whatever called `debug`. This is the only
    way the function `debug` can return to its caller.

    The use of the debugger-args is that `debug` displays the
    rest of its arguments at the top of the `*Backtrace*` buffer, so
    that the user can see them. Except as described below, this is the
    *only* way these arguments are used.

    However, certain values for first argument to `debug` have a
    special significance. (Normally, these values are used only by the
    internals of Emacs, and not by programmers calling `debug`.) Here
    is a table of these special values:

    `lambda` [¶](#index-lambda-in-debug)
    :   A first argument of `lambda` means `debug` was called
        because of entry to a function when `debug-on-next-call` was
        non-`nil`. The debugger displays ‘`Debugger
        entered--entering a function:`’ as a line of text at the top of the
        buffer.

    `debug`
    :   `debug` as first argument means `debug` was called because
        of entry to a function that was set to debug on entry. The debugger
        displays the string ‘`Debugger entered--entering a function:`’,
        just as in the `lambda` case. It also marks the stack frame for
        that function so that it will invoke the debugger when exited.

    `t`
    :   When the first argument is `t`, this indicates a call to
        `debug` due to evaluation of a function call form when
        `debug-on-next-call` is non-`nil`. The debugger displays
        ‘`Debugger entered--beginning evaluation of function call form:`’
        as the top line in the buffer.

    `exit`
    :   When the first argument is `exit`, it indicates the exit of a
        stack frame previously marked to invoke the debugger on exit. The
        second argument given to `debug` in this case is the value being
        returned from the frame. The debugger displays ‘`Debugger
        entered--returning value:`’ in the top line of the buffer, followed by
        the value being returned.

    `error` [¶](#index-error-in-debug)
    :   When the first argument is `error`, the debugger indicates that
        it is being entered because an error or `quit` was signaled and
        not handled, by displaying ‘`Debugger entered--Lisp error:`’
        followed by the error signaled and any arguments to `signal`.
        For example,

        ```
        (let ((debug-on-error t))
          (/ 1 0))
        ```

        ```

        ```

        ```
        ------ Buffer: *Backtrace* ------
        Debugger entered--Lisp error: (arith-error)
          /(1 0)
        ...
        ------ Buffer: *Backtrace* ------
        ```

        If an error was signaled, presumably the variable
        `debug-on-error` is non-`nil`. If `quit` was signaled,
        then presumably the variable `debug-on-quit` is non-`nil`.

    `nil`
    :   Use `nil` as the first of the debugger-args when you want
        to enter the debugger explicitly. The rest of the debugger-args
        are printed on the top line of the buffer. You can use this feature to
        display messages—for example, to remind yourself of the conditions
        under which `debug` is called.

Variable: **backtrace-on-error-noninteractive** [¶](#index-backtrace_002don_002derror_002dnoninteractive)
:   If this variable is non-`nil`, the default, entering the debugger
    in batch mode shows the backtrace of Lisp functions calls. Binding
    the variable to the `nil` value suppresses the backtrace and
    shows only the error message.

---

Previous: [Invoking the Debugger](#Invoking-the-Debugger), Up: [The Lisp Debugger](#Debugger)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.1.11 Internals of the Debugger [¶](#Internals-of-the-Debugger)

This section describes functions and variables used internally by the
debugger.

Variable: **debugger** [¶](#index-debugger)
:   The value of this variable is the function to call to invoke the
    debugger. Its value must be a function of any number of arguments, or,
    more typically, the name of a function. This function should invoke
    some kind of debugger. The default value of the variable is
    `debug`.

    The first argument that Lisp hands to the function indicates why it
    was called. The convention for arguments is detailed in the description
    of `debug` (see [Invoking the Debugger](#Invoking-the-Debugger)).

Function: **backtrace** [¶](#index-backtrace)
:   This function prints a trace of Lisp function calls currently active.
    The trace is identical to the one that `debug` would show in the
    `*Backtrace*` buffer. The return value is always `nil`.

    In the following example, a Lisp expression calls `backtrace`
    explicitly. This prints the backtrace to the stream
    `standard-output`, which, in this case, is the buffer
    ‘`backtrace-output`’.

    Each line of the backtrace represents one function call. The line
    shows the function followed by a list of the values of the function’s
    arguments if they are all known; if they are still being computed, the
    line consists of a list containing the function and its unevaluated
    arguments. Long lists or deeply nested structures may be elided.

    ```
    (with-output-to-temp-buffer "backtrace-output"
      (let ((var 1))
        (save-excursion
          (setq var (eval '(progn
                             (1+ var)
                             (list 'testing (backtrace))))))))

         ⇒ (testing nil)
    ```

    ```

    ```

    ```
    ----------- Buffer: backtrace-output ------------
      backtrace()
      (list 'testing (backtrace))
    ```

    ```
      (progn ...)
      eval((progn (1+ var) (list 'testing (backtrace))))
      (setq ...)
      (save-excursion ...)
      (let ...)
      (with-output-to-temp-buffer ...)
      eval((with-output-to-temp-buffer ...))
      eval-last-sexp-1(nil)
    ```

    ```
      eval-last-sexp(nil)
      call-interactively(eval-last-sexp)
    ----------- Buffer: backtrace-output ------------
    ```

User Option: **debugger-stack-frame-as-list** [¶](#index-debugger_002dstack_002dframe_002das_002dlist)
:   If this variable is non-`nil`, every stack frame of the backtrace
    is displayed as a list. This aims at improving the backtrace
    readability at the cost of special forms no longer being visually
    different from regular function calls.

    With `debugger-stack-frame-as-list` non-`nil`, the above
    example would look as follows:

    ```
    ----------- Buffer: backtrace-output ------------
      (backtrace)
      (list 'testing (backtrace))
    ```

    ```
      (progn ...)
      (eval (progn (1+ var) (list 'testing (backtrace))))
      (setq ...)
      (save-excursion ...)
      (let ...)
      (with-output-to-temp-buffer ...)
      (eval (with-output-to-temp-buffer ...))
      (eval-last-sexp-1 nil)
    ```

    ```
      (eval-last-sexp nil)
      (call-interactively eval-last-sexp)
    ----------- Buffer: backtrace-output ------------
    ```

Variable: **debug-on-next-call** [¶](#index-debug_002don_002dnext_002dcall)
:   If this variable is non-`nil`, it says to call the debugger before
    the next `eval`, `apply` or `funcall`. Entering the
    debugger sets `debug-on-next-call` to `nil`.

    The `d` command in the debugger works by setting this variable.

Function: **backtrace-debug** level flag [¶](#index-backtrace_002ddebug)
:   This function sets the debug-on-exit flag of the stack frame level
    levels down the stack, giving it the value flag. If flag is
    non-`nil`, this will cause the debugger to be entered when that
    frame later exits. Even a nonlocal exit through that frame will enter
    the debugger.

    This function is used only by the debugger.

Variable: **command-debug-status** [¶](#index-command_002ddebug_002dstatus)
:   This variable records the debugging status of the current interactive
    command. Each time a command is called interactively, this variable is
    bound to `nil`. The debugger can set this variable to leave
    information for future debugger invocations during the same command
    invocation.

    The advantage of using this variable rather than an ordinary global
    variable is that the data will never carry over to a subsequent command
    invocation.

    This variable is obsolete and will be removed in future versions.

Function: **backtrace-frame** frame-number &optional base [¶](#index-backtrace_002dframe)
:   The function `backtrace-frame` is intended for use in Lisp
    debuggers. It returns information about what computation is happening
    in the stack frame frame-number levels down.

    If that frame has not evaluated the arguments yet, or is a special
    form, the value is `(nil function arg-forms…)`.

    If that frame has evaluated its arguments and called its function
    already, the return value is `(t function
    arg-values…)`.

    In the return value, function is whatever was supplied as the
    CAR of the evaluated list, or a `lambda` expression in the
    case of a macro call. If the function has a `&rest` argument, that
    is represented as the tail of the list arg-values.

    If base is specified, frame-number counts relative to
    the topmost frame whose function is base.

    If frame-number is out of range, `backtrace-frame` returns
    `nil`.

Function: **mapbacktrace** function &optional base [¶](#index-mapbacktrace)
:   The function `mapbacktrace` calls function once for each
    frame in the backtrace, starting at the first frame whose function is
    base (or from the top if base is omitted or `nil`).

    function is called with four arguments: evald, func,
    args, and flags.

    If a frame has not evaluated its arguments yet or is a special form,
    evald is `nil` and args is a list of forms.

    If a frame has evaluated its arguments and called its function
    already, evald is `t` and args is a list of values.
    flags is a plist of properties of the current frame: currently,
    the only supported property is `:debug-on-exit`, which is
    `t` if the stack frame’s `debug-on-exit` flag is set.

---

Next: [Debugging Invalid Lisp Syntax](#Syntax-Errors), Previous: [The Lisp Debugger](#Debugger), Up: [Debugging Lisp Programs](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
