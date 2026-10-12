### 6.26 Debugging Infrastructure [¶](#Debugging-Infrastructure)

Guile provides facilities for simple print-based debugging as well as
more advanced debugging features. In order to understand Guile’s
advanced debugging facilities, one first must understand a little about
how Guile represents the Scheme control stack. With that in place, we
can explain the low level trap calls that the virtual machine can be
configured to make, and the trap and breakpoint infrastructure that
builds on top of those calls.

* [Simple Debugging](#Simple-Debugging)
* [Evaluation and the Scheme Stack](#Evaluation-Model)
* [Source Properties](#Source-Properties)
* [Programmatic Error Handling](#Programmatic-Error-Handling)
* [Traps](#Traps)
* [GDB Support](#GDB-Support)

---

Next: [Evaluation and the Scheme Stack](#Evaluation-Model), Up: [Debugging Infrastructure](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.1 Simple Debugging [¶](#Simple-Debugging-1)

Guile offers powerful tools for introspection and debugging at the REPL,
covered in the rest of this section and elsewhere in this manual
(see [Interactive Debugging](#Interactive-Debugging)). Here we deal with a more primitive
approach, commonly called “print debugging,” which is a quick way to
diagnose simple errors by printing values during a program’s execution.
Guile provides the `peek` procedure, more commonly known as
`pk` (pronounced by naming the letters), as a convenient and
powerful tool for this kind of debugging.

Scheme Procedure: **peek** stuff … [¶](#index-peek)

Scheme Procedure: **pk** stuff … [¶](#index-pk)
:   Print stuff to the current output port using `write`. Return
    the last argument.

`pk` improves on using `write` directly because it enables
inspection of the state of code as it runs without breaking the normal
code flow. It is also sometimes more practical than a full debugger
because it does not require the program to be stopped for inspection.
Here is a basic example:

```
(define fire 'burns)

(pk fire)
⇒

;;; (burns)
burns
```

Here is an example of inspecting a value in the midst of code flow:

```
(map (lambda (v)
       (if (number? v)
           (pk 'number->string (number->string v))
           v))
     '(1 "2" "3" 4))
⇒

;;; ("1")

;;; ("4")
("1" "2" "3" "4")
```

A common technique when using `pk` is to label values with symbols
to keep track of where they’re coming from. There’s no reason these
labels need to be symbols; symbols are just convenient. Here’s a
slightly more complex example demonstrating that pattern:

```
(define (pk-identity x)
  (pk 'arg-to-identity x))

(pk-identity 42)
⇒

;;; (arg-to-identity 42)
42
```

`pk` has one small quirk of note. Currently, it only returns the
first value returned from any multi-value returns. So for example:

```
(pk 'vals (values 1 2 3))
⇒

;;; (vals 1)
1
```

The way to get around this limitation is to bind such multi-value
returns then inspect the results. Still, `pk` can only return a
single value:

```
(use-modules (srfi srfi-11))

(let-values (((x y z)
              (values 1 2 3)))
  (pk 'vals x y z))
⇒

;;; (vals 1 2 3)
3
```

---

Next: [Source Properties](#Source-Properties), Previous: [Simple Debugging](#Simple-Debugging), Up: [Debugging Infrastructure](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.2 Evaluation and the Scheme Stack [¶](#Evaluation-and-the-Scheme-Stack)

The idea of the Scheme stack is central to a lot of debugging. The
Scheme stack is a reified representation of the pending function returns
in an expression’s continuation. As Guile implements function calls
using a stack, this reification takes the form of a number of nested
stack frames, each of which corresponds to the application of a
procedure to a set of arguments.

A Scheme stack always exists implicitly, and can be summoned into
concrete existence as a first-class Scheme value by the
`make-stack` call, so that an introspective Scheme program – such
as a debugger – can present it in some way and allow the user to query
its details. The first thing to understand, therefore, is how Guile’s
function call convention creates the stack.

Broadly speaking, Guile represents all control flow on a stack. Calling
a function involves pushing an empty frame on the stack, then evaluating
the procedure and its arguments, then fixing up the new frame so that it
points to the old one. Frames on the stack are thus linked together. A
tail call is the same, except it reuses the existing frame instead of
pushing on a new one.

In this way, the only frames that are on the stack are “active”
frames, frames which need to do some work before the computation is
complete. On the other hand, a function that has tail-called another
function will not be on the stack, as it has no work left to do.

Therefore, when an error occurs in a running program, or the program
hits a breakpoint, or in fact at any point that the programmer chooses,
its state at that point can be represented by a *stack* of all the
procedure applications that are logically in progress at that time, each
of which is known as a *frame*. The programmer can learn more about
the program’s state at that point by inspecting the stack and its
frames.

* [Stack Capture](#Stack-Capture)
* [Stacks](#Stacks)
* [Frames](#Frames)

---

Next: [Stacks](#Stacks), Up: [Evaluation and the Scheme Stack](#Evaluation-Model)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.2.1 Stack Capture [¶](#Stack-Capture-1)

A Scheme program can use the `make-stack` primitive anywhere in its
code, with first arg `#t`, to construct a Scheme value that
describes the Scheme stack at that point.

```
(make-stack #t)
⇒
#<stack 25205a0>
```

Use `start-stack` to limit the stack extent captured by future
`make-stack` calls.

Scheme Procedure: **make-stack** obj arg … [¶](#index-make_002dstack)

C Function: **scm\_make\_stack** (obj, args) [¶](#index-scm_005fmake_005fstack)
:   Create a new stack. If obj is `#t`, the current
    evaluation stack is used for creating the stack frames,
    otherwise the frames are taken from obj (which must be
    a continuation or a frame object).

    arg … can be any combination of integer, procedure, address
    range, and prompt tag values.

    These values specify various ways of cutting away uninteresting stack
    frames from the top and bottom of the stack that `make-stack`
    returns. They come in pairs like this: `(inner_cut_1
    outer_cut_1 inner_cut_2 outer_cut_2 …)`.

    Each inner\_cut\_i can be an integer, a procedure, an address range,
    or a prompt tag. An integer means to cut away exactly that number of
    frames. A procedure means to cut away all frames up to but excluding
    the frame whose procedure matches the specified one. An address range
    is a pair of integers indicating the low and high addresses of a
    procedure’s code, and is the same as cutting away to a procedure (though
    with less work). Anything else is interpreted as a prompt tag which
    cuts away all frames that are inside a prompt with the given tag.

    Each outer\_cut\_i can likewise be an integer, a procedure, an
    address range, or a prompt tag. An integer means to cut away that
    number of frames. A procedure means to cut away frames down to but
    excluding the frame whose procedure matches the specified one. An
    address range is the same, but with the procedure’s code specified as an
    address range. Anything else is taken to be a prompt tag, which cuts
    away all frames that are outside a prompt with the given tag.

    If the outer\_cut\_i of the last pair is missing, it is taken as 0.

Scheme Syntax: **start-stack** id exp [¶](#index-start_002dstack)
:   Evaluate exp on a new calling stack with identity id. If
    exp is interrupted during evaluation, backtraces will not display
    frames farther back than exp’s top-level form. This macro is a
    way of artificially limiting backtraces and stack procedures, largely as
    a convenience to the user.

---

Next: [Frames](#Frames), Previous: [Stack Capture](#Stack-Capture), Up: [Evaluation and the Scheme Stack](#Evaluation-Model)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.2.2 Stacks [¶](#Stacks-1)

Scheme Procedure: **stack?** obj [¶](#index-stack_003f)

C Function: **scm\_stack\_p** (obj) [¶](#index-scm_005fstack_005fp)
:   Return `#t` if obj is a calling stack.

Scheme Procedure: **stack-id** stack [¶](#index-stack_002did)

C Function: **scm\_stack\_id** (stack) [¶](#index-scm_005fstack_005fid)
:   Return the identifier given to stack by `start-stack`.

Scheme Procedure: **stack-length** stack [¶](#index-stack_002dlength)

C Function: **scm\_stack\_length** (stack) [¶](#index-scm_005fstack_005flength)
:   Return the length of stack.

Scheme Procedure: **stack-ref** stack index [¶](#index-stack_002dref)

C Function: **scm\_stack\_ref** (stack, index) [¶](#index-scm_005fstack_005fref)
:   Return the index’th frame from stack.

Scheme Procedure: **display-backtrace** stack port [first [depth [highlights]]] [¶](#index-display_002dbacktrace)

C Function: **scm\_display\_backtrace\_with\_highlights** (stack, port, first, depth, highlights) [¶](#index-scm_005fdisplay_005fbacktrace_005fwith_005fhighlights)

C Function: **scm\_display\_backtrace** (stack, port, first, depth) [¶](#index-scm_005fdisplay_005fbacktrace)
:   Display a backtrace to the output port port. stack
    is the stack to take the backtrace from, first specifies
    where in the stack to start and depth how many frames
    to display. first and depth can be `#f`,
    which means that default values will be used.
    If highlights is given it should be a list; the elements
    of this list will be highlighted wherever they appear in the
    backtrace.

---

Previous: [Stacks](#Stacks), Up: [Evaluation and the Scheme Stack](#Evaluation-Model)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.2.3 Frames [¶](#Frames-1)

Scheme Procedure: **frame?** obj [¶](#index-frame_003f)

C Function: **scm\_frame\_p** (obj) [¶](#index-scm_005fframe_005fp)
:   Return `#t` if obj is a stack frame.

Scheme Procedure: **frame-previous** frame [¶](#index-frame_002dprevious)

C Function: **scm\_frame\_previous** (frame) [¶](#index-scm_005fframe_005fprevious)
:   Return the previous frame of frame, or `#f` if
    frame is the first frame in its stack.

Scheme Procedure: **frame-procedure-name** frame [¶](#index-frame_002dprocedure_002dname)

C Function: **scm\_frame\_procedure\_name** (frame) [¶](#index-scm_005fframe_005fprocedure_005fname)
:   Return the name of the procedure being applied in frame, as a
    symbol, or `#f` if the procedure has no name.

Scheme Procedure: **frame-arguments** frame [¶](#index-frame_002darguments)

C Function: **scm\_frame\_arguments** (frame) [¶](#index-scm_005fframe_005farguments)
:   Return the arguments of frame.

Scheme Procedure: **frame-address** frame [¶](#index-frame_002daddress)

Scheme Procedure: **frame-instruction-pointer** frame [¶](#index-frame_002dinstruction_002dpointer)

Scheme Procedure: **frame-stack-pointer** frame [¶](#index-frame_002dstack_002dpointer)
:   Accessors for the three VM registers associated with this frame: the
    frame pointer (fp), instruction pointer (ip), and stack pointer (sp),
    respectively. See [VM Concepts](#VM-Concepts), for more information.

Scheme Procedure: **frame-dynamic-link** frame [¶](#index-frame_002ddynamic_002dlink)

Scheme Procedure: **frame-return-address** frame [¶](#index-frame_002dreturn_002daddress)

Scheme Procedure: **frame-mv-return-address** frame [¶](#index-frame_002dmv_002dreturn_002daddress)
:   Accessors for the three saved VM registers in a frame: the previous
    frame pointer, the single-value return address, and the multiple-value
    return address. See [Stack Layout](#Stack-Layout), for more information.

Scheme Procedure: **frame-bindings** frame [¶](#index-frame_002dbindings)
:   Return a list of binding records indicating the local variables that are
    live in a frame.

Scheme Procedure: **frame-lookup-binding** frame var [¶](#index-frame_002dlookup_002dbinding)
:   Fetch the bindings in frame, and return the first one whose name
    is var, or `#f` otherwise.

Scheme Procedure: **binding-index** binding [¶](#index-binding_002dindex)

Scheme Procedure: **binding-name** binding [¶](#index-binding_002dname)

Scheme Procedure: **binding-slot** binding [¶](#index-binding_002dslot)

Scheme Procedure: **binding-representation** binding [¶](#index-binding_002drepresentation)
:   Accessors for the various fields in a binding. The implicit “callee”
    argument is index 0, the first argument is index 1, and so on to the end
    of the arguments. After that are temporary variables. Note that if a
    variable is dead, it might not be available.

Scheme Procedure: **binding-ref** binding [¶](#index-binding_002dref)

Scheme Procedure: **binding-set!** binding val [¶](#index-binding_002dset_0021)
:   Accessors for the values of local variables in a frame.

Scheme Procedure: **display-application** frame [port [indent]] [¶](#index-display_002dapplication)

C Function: **scm\_display\_application** (frame, port, indent) [¶](#index-scm_005fdisplay_005fapplication)
:   Display a procedure application frame to the output port
    port. indent specifies the indentation of the
    output.

Additionally, the `(system vm frame)` module defines a number of
higher-level introspective procedures, for example to retrieve the names
of local variables, and the source location to correspond to a
frame. See its source code for more details.

---

Next: [Programmatic Error Handling](#Programmatic-Error-Handling), Previous: [Evaluation and the Scheme Stack](#Evaluation-Model), Up: [Debugging Infrastructure](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.3 Source Properties [¶](#Source-Properties-1)

How best to associate source locations with datums parsed from a port?
The right way to do this is to annotate all components of each parsed
datum. See [Reading Scheme Code, For the Compiler](#Annotated-Scheme-Read), for more on `read-syntax`.

Guile only switched to use `read-syntax` in 2021, however. For the
previous thirty years, it used a mechanism known as *source
properties*.

As Guile reads in Scheme code from file or from standard input, it can
record the file name, line number and column number where each
expression begins in a side table.

The way that this side table associates datums with source properties
has a limitation, however: Guile can only associate source properties
with freshly allocated objects. This notably excludes individual
symbols, keywords, characters, booleans, or small integers. This
limitation finally motivated the switch to `read-syntax`.

Scheme Procedure: **supports-source-properties?** obj [¶](#index-supports_002dsource_002dproperties_003f)

C Function: **scm\_supports\_source\_properties\_p** (obj) [¶](#index-scm_005fsupports_005fsource_005fproperties_005fp)
:   Return #t if source properties can be associated with obj,
    otherwise return #f.

The recording of source properties is controlled by the read option
named “positions” (see [Reading Scheme Code](#Scheme-Read)). This option is switched
*on* by default. Now that `read-syntax` is available,
however, Guile may change the default for this flag to off in the
future.

The following procedures can be used to access and set the source
properties of read expressions.

Scheme Procedure: **set-source-properties!** obj alist [¶](#index-set_002dsource_002dproperties_0021)

C Function: **scm\_set\_source\_properties\_x** (obj, alist) [¶](#index-scm_005fset_005fsource_005fproperties_005fx)
:   Install the association list alist as the source property
    list for obj.

Scheme Procedure: **set-source-property!** obj key datum [¶](#index-set_002dsource_002dproperty_0021)

C Function: **scm\_set\_source\_property\_x** (obj, key, datum) [¶](#index-scm_005fset_005fsource_005fproperty_005fx)
:   Set the source property of object obj, which is specified by
    key to datum. Normally, the key will be a symbol.

Scheme Procedure: **source-properties** obj [¶](#index-source_002dproperties)

C Function: **scm\_source\_properties** (obj) [¶](#index-scm_005fsource_005fproperties)
:   Return the source property association list of obj.

Scheme Procedure: **source-property** obj key [¶](#index-source_002dproperty)

C Function: **scm\_source\_property** (obj, key) [¶](#index-scm_005fsource_005fproperty)
:   Return the property specified by key from obj’s source
    properties.

If the `positions` reader option is enabled, supported expressions
will have values set for the `filename`, `line` and
`column` properties.

Source properties are also associated with syntax objects. Procedural
macros can get at the source location of their input using the
`syntax-source` accessor. See [Syntax Transformer Helpers](#Syntax-Transformer-Helpers), for
more.

Guile also defines a couple of convenience macros built on
`syntax-source`:

Scheme Syntax: **current-source-location** [¶](#index-current_002dsource_002dlocation)
:   Expands to the source properties corresponding to the location of the
    `(current-source-location)` form.

Scheme Syntax: **current-filename** [¶](#index-current_002dfilename)
:   Expands to the current filename: the filename that the
    `(current-filename)` form appears in. Expands to `#f` if this
    information is unavailable.

If you’re stuck with defmacros (see [Lisp-style Macro Definitions](#Defmacros)), and want to preserve
source information, the following helper function might be useful to
you:

Scheme Procedure: **cons-source** xorig x y [¶](#index-cons_002dsource)

C Function: **scm\_cons\_source** (xorig, x, y) [¶](#index-scm_005fcons_005fsource)
:   Create and return a new pair whose car and cdr are x and y.
    Any source properties associated with xorig are also associated
    with the new pair.

---

Next: [Traps](#Traps), Previous: [Source Properties](#Source-Properties), Up: [Debugging Infrastructure](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.4 Programmatic Error Handling [¶](#Programmatic-Error-Handling-1)

For better or for worse, all programs have bugs, and dealing with bugs
is part of programming. This section deals with that class of bugs that
causes an exception to be raised – from your own code, from within a
library, or from Guile itself.

* [Catching Exceptions](#Catching-Exceptions)
* [Pre-Unwind Debugging](#Pre_002dUnwind-Debugging)
* [call-with-error-handling](#Standard-Error-Handling)
* [Stack Overflow](#Stack-Overflow)
* [Debug options](#Debug-Options)

---

Next: [Pre-Unwind Debugging](#Pre_002dUnwind-Debugging), Up: [Programmatic Error Handling](#Programmatic-Error-Handling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.4.1 Catching Exceptions [¶](#Catching-Exceptions-1)

A common requirement is to be able to show as much useful context as
possible when a Scheme program hits an error. The most immediate
information about an error is the kind of error that it is – such as
“division by zero” – and any parameters that the code which signaled
the error chose explicitly to provide. This information originates with
the `error` or `raise-exception` call (or their C code
equivalents, if the error is detected by C code) that signals the error,
and is passed automatically to the handler procedure of the innermost
applicable exception handler.

Therefore, to catch errors that occur within a chunk of Scheme code, and
to intercept basic information about those errors, you need to execute
that code inside the dynamic context of a `with-exception-handler`,
or the equivalent in C.

For example, to print out a message and return #f when an error occurs,
you might use:

```
(define (catch-all thunk)
  (with-exception-handler
    (lambda (exn)
      (format (current-error-port)
              "Uncaught exception: ~s\n" exn)
      #f)
    thunk
    #:unwind? #t))

(catch-all
 (lambda () (error "Not a vegetable: tomato")))
⊣ Uncaught exception: #<&exception-with-kind-and-args ...>
⇒ #f
```

See [Exceptions](#Exceptions), for full details.

---

Next: [call-with-error-handling](#Standard-Error-Handling), Previous: [Catching Exceptions](#Catching-Exceptions), Up: [Programmatic Error Handling](#Programmatic-Error-Handling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.4.2 Pre-Unwind Debugging [¶](#Pre_002dUnwind-Debugging-1)

Sometimes when something goes wrong, what you want is not just a
representation of the exceptional situation, but the context that
brought about that situation. The example in the previous section
passed `#:unwind #t` to `with-exception-handler`, indicating
that `raise-exception` should unwind the stack before invoking the
exception handler. However if you don’t take this approach and instead
let the exception handler be invoked in the context of the
`raise-exception`, you can print a backtrace, launch a recursive
debugger, or take other “pre-unwind” actions.

The most basic idea would be to simply print a backtrace:

```
(define (call-with-backtrace thunk)
  (with-exception-handler
    (lambda (exn)
      (backtrace)
      (raise-exception exn))
    thunk))
```

Here we use the built-in `backtrace` procedure to print the
backtrace.

Scheme Procedure: **backtrace** [highlights] [¶](#index-backtrace-1)

C Function: **scm\_backtrace\_with\_highlights** (highlights) [¶](#index-scm_005fbacktrace_005fwith_005fhighlights)

C Function: **scm\_backtrace** () [¶](#index-scm_005fbacktrace)
:   Display a backtrace of the current stack to the current output port. If
    highlights is given it should be a list; the elements of this list
    will be highlighted wherever they appear in the backtrace.

By re-raising the exception, `call-with-backtrace` doesn’t actually
handle the error. We could define a version that instead aborts the
computation:

```
(use-modules (ice-9 control))
(define (call-with-backtrace thunk)
  (let/ec cancel
    (with-exception-handler
      (lambda (exn)
        (backtrace)
        (cancel #f))
      thunk)))
```

In this second example, we use an escape continuation to abort the
computation after printing the backtrace, returning `#f` instead.

It could be that you want to only print a limited backtrace. In that
case, use `start-stack`:

```
(use-modules (ice-9 control))
(define (call-with-backtrace thunk)
  (let/ec cancel
    (start-stack 'stack-with-backtrace
      (with-exception-handler
        (lambda (exn)
          (backtrace)
          (cancel #f))
        thunk))))
```

There are also more powerful, programmatic ways to walk the stack using
`make-stack` and friends; see the API described in [Stacks](#Stacks) and
[Frames](#Frames).

---

Next: [Stack Overflow](#Stack-Overflow), Previous: [Pre-Unwind Debugging](#Pre_002dUnwind-Debugging), Up: [Programmatic Error Handling](#Programmatic-Error-Handling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.4.3 call-with-error-handling [¶](#call_002dwith_002derror_002dhandling)

The Guile REPL code (in `system/repl/repl.scm` and related files)
uses a `catch` with a pre-unwind handler to capture the stack when
an error occurs in an expression that was typed into the REPL, and debug
that stack interactively in the context of the error.

These procedures are available for use by user programs, in the
`(system repl error-handling)` module.

```
(use-modules (system repl error-handling))
```

Scheme Procedure: **call-with-error-handling** thunk [#:on-error on-error=’debug] [#:post-error post-error=’catch] [#:pass-keys pass-keys=’(quit)] [#:report-keys report-keys=’(stack-overflow)] [#:trap-handler trap-handler=’debug] [¶](#index-call_002dwith_002derror_002dhandling)
:   Call a thunk in a context in which errors are handled.

    Note that this function was written when `throw`/`catch` were
    the fundamental exception handling primitives in Guile, and so exposes
    some aspects of that interface (notably in the form of the procedural
    handlers). Guile will probably replace this function with a
    `call-with-standard-exception-handling` in the future.

    There are five keyword arguments:

    on-error
    :   Specifies what to do before the stack is unwound.

        Valid options are `debug` (the default), which will enter a
        debugger; `pass`, in which case nothing is done, and the exception
        is rethrown; or a procedure, which will be the pre-unwind handler.

    post-error
    :   Specifies what to do after the stack is unwound.

        Valid options are `catch` (the default), which will silently catch
        errors, returning the unspecified value; `report`, which prints out
        a description of the error (via `display-error`), and then returns
        the unspecified value; or a procedure, which will be the catch handler.

    trap-handler
    :   Specifies a trap handler: what to do when a breakpoint is hit.

        Valid options are `debug`, which will enter the debugger;
        `pass`, which does nothing; or `disabled`, which disables
        traps entirely. See [Traps](#Traps), for more information.

    pass-keys
    :   A set of keys to ignore, as a list.

    report-keys
    :   A set of keys to always report even if the post-error handler is
        `catch`, as a list.

---

Next: [Debug options](#Debug-Options), Previous: [call-with-error-handling](#Standard-Error-Handling), Up: [Programmatic Error Handling](#Programmatic-Error-Handling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.4.4 Stack Overflow [¶](#Stack-Overflow-1)

Every time a Scheme program makes a call that is not in tail position,
it pushes a new frame onto the stack. Returning a value from a function
pops the top frame off the stack. Stack frames take up memory, and as
nobody has an infinite amount of memory, deep recursion could cause
Guile to run out of memory. Running out of stack memory is called
*stack overflow*.

#### Stack Limits [¶](#Stack-Limits)

Most languages have a terrible stack overflow story. For example, in C,
if you use too much stack, your program will exhibit “undefined
behavior”, which if you are lucky means that it will crash. It’s
especially bad in C, as you neither know ahead of time how much stack
your functions use, nor the stack limit imposed by the user’s system,
and the stack limit is often quite small relative to the total memory
size.

Managed languages like Python have a better error story, as they are
defined to raise an exception on stack overflow – but like C, Python
and most dynamic languages still have a fixed stack size limit that is
usually much smaller than the heap.

Arbitrary stack limits would have an unfortunate effect on Guile
programs. For example, the following implementation of the inner loop
of `map` is clean and elegant:

```
(define (map f l)
  (if (pair? l)
      (cons (f (car l))
            (map f (cdr l)))
      '()))
```

However, if there were a stack limit, that would limit the size of lists
that can be processed with this `map`. Eventually, you would have
to rewrite it to use iteration with an accumulator:

```
(define (map f l)
  (let lp ((l l) (out '()))
    (if (pair? l)
        (lp (cdr l) (cons (f (car l)) out))
        (reverse out))))
```

This second version is sadly not as clear, and it also allocates more
heap memory (once to build the list in reverse, and then again to
reverse the list). You would be tempted to use the destructive
`reverse!` to save memory and time, but then your code would not be
continuation-safe – if f returned again after the map had
finished, it would see an out list that had already been
reversed. The recursive `map` has none of these problems.

Guile has no stack limit for Scheme code. When a thread makes its first
Guile call, a small stack is allocated – just one page of memory.
Whenever that memory limit would be reached, Guile arranges to grow the
stack by a factor of two. When garbage collection happens, Guile
arranges to return the unused part of the stack to the operating system,
but without causing the stack to shrink. In this way, the stack can
grow to consume up to all memory available to the Guile process, and
when the recursive computation eventually finishes, that stack memory is
returned to the system.

#### Exceptional Situations [¶](#Exceptional-Situations)

Of course, it’s still possible to run out of stack memory. The most
common cause of this is program bugs that cause unbounded recursion, as
in:

```
(define (faulty-map f l)
  (if (pair? l)
      (cons (f (car l)) (faulty-map f l))
      '()))
```

Did you spot the bug? The recursive call to `faulty-map` recursed
on l, not `(cdr l)`. Running this program would cause
Guile to use up all memory in your system, and eventually Guile would
fail to grow the stack. At that point you have a problem: Guile needs
to raise an exception to unwind the stack and return memory to the
system, but the user might have exception handlers in place
(see [Raising and Handling Exceptions](#Raising-and-Handling-Exceptions)) that want to run before the
stack is unwound, and we don’t have any stack in which to run them.

Therefore in this case, Guile raises an unwind-only exception that does
not run pre-unwind handlers. Because this is such an odd case, Guile
prints out a message on the console, in case the user was expecting to
be able to get a backtrace from any pre-unwind handler.

#### Runaway Recursion [¶](#Runaway-Recursion)

Still, this failure mode is not so nice. If you are running an
environment in which you are interactively building a program while it
is running, such as at a REPL, you might want to impose an artificial
stack limit on the part of your program that you are building to detect
accidental runaway recursion. For that purpose, there is
`call-with-stack-overflow-handler`, from `(system vm vm)`.

```
(use-module (system vm vm))
```

Scheme Procedure: **call-with-stack-overflow-handler** limit thunk handler [¶](#index-call_002dwith_002dstack_002doverflow_002dhandler)
:   Call thunk in an environment in which the stack limit has been
    reduced to limit additional words. If the limit is reached,
    handler (a thunk) will be invoked in the dynamic environment of
    the error. For the extent of the call to handler, the stack limit
    and handler are restored to the values that were in place when
    `call-with-stack-overflow-handler` was called.

    Usually, handler should raise an exception or abort to an outer
    prompt. However if handler does return, it should return a number
    of additional words of stack space to allow to the inner environment.

A stack overflow handler may only ever “credit” the inner thunk with
stack space that was available when the handler was instated. When
Guile first starts, there is no stack limit in place, so the outer
handler may allow the inner thunk an arbitrary amount of space, but any
nested stack overflow handler will not be able to consume more than its
limit.

Unlike the unwind-only exception that is thrown if Guile is unable to
grow its stack, any exception thrown by a stack overflow handler might
invoke pre-unwind handlers. Indeed, the stack overflow handler is
itself a pre-unwind handler of sorts. If the code imposing the stack
limit wants to protect itself against malicious pre-unwind handlers from
the inner thunk, it should abort to a prompt of its own making instead
of throwing an exception that might be caught by the inner thunk.

#### C Stack Usage [¶](#C-Stack-Usage)

It is also possible for Guile to run out of space on the C stack. If
you call a primitive procedure which then calls a Scheme procedure in a
loop, you will consume C stack space. Guile tries to detect excessive
consumption of C stack space, throwing an error when you have hit 80% of
the process’ available stack (as allocated by the operating system), or
160 kilowords in the absence of a strict limit.

For example, looping through `call-with-vm`, a primitive that calls
a thunk, gives us the following:

```
scheme@(guile-user)> (use-modules (system vm vm))
scheme@(guile-user)> (let lp () (call-with-vm lp))
ERROR: Stack overflow
```

Unfortunately, that’s all the information we get. Overrunning the C
stack will throw an unwind-only exception, because it’s not safe to
do very much when you are close to the C stack limit.

If you get an error like this, you can either try rewriting your code to
use less stack space, or increase the maximum stack size. To increase
the maximum stack size, use `debug-set!`, for example:

```
(debug-set! stack 200000)
```

The next section describes `debug-set!` more thoroughly. Of course
the best thing is to have your code operate without so much resource
consumption by avoiding loops through C trampolines.

---

Previous: [Stack Overflow](#Stack-Overflow), Up: [Programmatic Error Handling](#Programmatic-Error-Handling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.4.5 Debug options [¶](#Debug-options)

The behavior of the `backtrace` procedure and of the default error
handler can be parameterized via the debug options.

Scheme Procedure: **debug-options** [setting] [¶](#index-debug_002doptions)
:   Display the current settings of the debug options. If setting is
    omitted, only a short form of the current read options is printed.
    Otherwise if setting is the symbol `help`, a complete options
    description is displayed.

The set of available options, and their default values, may be had by
invoking `debug-options` at the prompt.

```
scheme@(guile-user)>
backwards       no      Display backtrace in anti-chronological order.
width           79      Maximal width of backtrace.
depth           20      Maximal length of printed backtrace.
backtrace       yes     Show backtrace on error.
stack           1048576 Stack size limit (measured in words;
                        0 = no check). 
show-file-name  #t      Show file names and line numbers in backtraces
                        when not `#f'.  A value of `base' displays only
                        base names, while `#t' displays full names. 
warn-deprecated no      Warn when deprecated features are used.
```

The boolean options may be toggled with `debug-enable` and
`debug-disable`. The non-boolean options must be set using
`debug-set!`.

Scheme Procedure: **debug-enable** option-name [¶](#index-debug_002denable)

Scheme Procedure: **debug-disable** option-name [¶](#index-debug_002ddisable)

Scheme Syntax: **debug-set!** option-name value [¶](#index-debug_002dset_0021)
:   Modify the debug options. `debug-enable` should be used with boolean
    options and switches them on, `debug-disable` switches them off.

    `debug-set!` can be used to set an option to a specific value. Due
    to historical oddities, it is a macro that expects an unquoted option
    name.

---

Next: [GDB Support](#GDB-Support), Previous: [Programmatic Error Handling](#Programmatic-Error-Handling), Up: [Debugging Infrastructure](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.5 Traps [¶](#Traps-1)

Guile’s virtual machine can be configured to call out at key points to
arbitrary user-specified procedures.

In principle, these *hooks* allow Scheme code to implement any model
it chooses for examining the evaluation stack as program execution
proceeds, and for suspending execution to be resumed later.

VM hooks are very low-level, though, and so Guile also has a library of
higher-level *traps* on top of the VM hooks. A trap is an execution
condition that, when fulfilled, will fire a handler. For example, Guile
defines a trap that fires when control reaches a certain source
location.

Finally, Guile also defines a third level of abstractions: per-thread
*trap states*. A trap state exists to give names to traps, and to
hold on to the set of traps so that they can be enabled, disabled, or
removed. The trap state infrastructure defines the most useful
abstractions for most cases. For example, Guile’s REPL uses trap state
functions to set breakpoints and tracepoints.

The following subsections describe all this in detail, for both the
user wanting to use traps, and the developer interested in
understanding how the interface hangs together.

* [VM Hooks](#VM-Hooks)
* [Trap Interface](#Trap-Interface)
* [Low-Level Traps](#Low_002dLevel-Traps)
* [Tracing Traps](#Tracing-Traps)
* [Trap States](#Trap-States)
* [High-Level Traps](#High_002dLevel-Traps)

---

Next: [Trap Interface](#Trap-Interface), Up: [Traps](#Traps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.5.1 VM Hooks [¶](#VM-Hooks-1)

Everything that runs in Guile runs on its virtual machine, a C program
that defines a number of operations that Scheme programs can
perform.

Note that there are multiple VM “engines” for Guile. Only some of them
have support for hooks compiled in. Normally the deal is that you get
hooks if you are running interactively, and otherwise they are disabled,
as they do have some overhead (about 10 or 20 percent).

To ensure that you are running with hooks, pass `--debug` to Guile
when running your program, or otherwise use the `call-with-vm` and
`set-vm-engine!` procedures to ensure that you are running in a VM
with the `debug` engine.

To digress, Guile’s VM has 4 different hooks that can be fired at
different times. For implementation reasons, these hooks are not
actually implemented with first-class Scheme hooks (see [Hooks](#Hooks)); they
are managed using an ad-hoc interface.

VM hooks are called with one argument: the current frame.
See [Frames](#Frames). Since these hooks may be fired very frequently, Guile
does a terrible thing: it allocates the frames on the C stack instead of
the garbage-collected heap.

The upshot here is that the frames are only valid within the dynamic
extent of the call to the hook. If a hook procedure keeps a reference to
the frame outside the extent of the hook, bad things will happen.

The interface to hooks is provided by the `(system vm vm)` module:

```
(use-modules (system vm vm))
```

All of these functions implicitly act on the VM for the current thread
only.

Scheme Procedure: **vm-add-next-hook!** f [¶](#index-vm_002dadd_002dnext_002dhook_0021)
:   Arrange to call f when before an instruction is retired (and
    executed).

Scheme Procedure: **vm-add-apply-hook!** f [¶](#index-vm_002dadd_002dapply_002dhook_0021)
:   Arrange to call f whenever a procedure is applied. The frame
    locals will be the callee, followed by the arguments to the call.

    Note that procedure application is somewhat orthogonal to continuation
    pushes and pops. To know whether a call is a tail call or not, with
    respect to the frame previously in place, check the value of the frame
    pointer compared the previous frame pointer.

Scheme Procedure: **vm-add-return-hook!** f [¶](#index-vm_002dadd_002dreturn_002dhook_0021)
:   Arrange to call f before returning from a frame. The values in
    the frame will be the frame’s return values.

    Note that it’s possible to return from an “inner” frame: one that was
    not immediately proceeded by a call with that frame pointer. In that
    case, it corresponds to a non-local control flow jump, either because of
    applying a composable continuation or because of restoring a saved
    undelimited continuation.

Scheme Procedure: **vm-add-abort-hook!** [¶](#index-vm_002dadd_002dabort_002dhook_0021)
:   Arrange to call f after aborting to a prompt. See [Prompts](#Prompts).

    Unfortunately, the values passed to the prompt handler are not easily
    available to f.

Scheme Procedure: **vm-remove-next-hook!** f [¶](#index-vm_002dremove_002dnext_002dhook_0021)

Scheme Procedure: **vm-remove-apply-hook!** f [¶](#index-vm_002dremove_002dapply_002dhook_0021)

Scheme Procedure: **vm-remove-return-hook!** f [¶](#index-vm_002dremove_002dreturn_002dhook_0021)

Scheme Procedure: **vm-remove-abort-hook!** f [¶](#index-vm_002dremove_002dabort_002dhook_0021)
:   Remove f from the corresponding VM hook for the current thread.

These hooks do impose a performance penalty, if they are on. Obviously,
the `vm-next-hook` has quite an impact, performance-wise. Therefore
Guile exposes a single, heavy-handed knob to turn hooks on or off, the
*VM trace level*. If the trace level is positive, hooks run;
otherwise they don’t.

For convenience, when the VM fires a hook, it does so with the trap
level temporarily set to 0. That way the hooks don’t fire while you’re
handling a hook. The trace level is restored to whatever it was once the hook
procedure finishes.

Scheme Procedure: **vm-trace-level** [¶](#index-vm_002dtrace_002dlevel)
:   Retrieve the “trace level” of the VM. If positive, the trace hooks
    associated with vm will be run. The initial trace level is 0.

Scheme Procedure: **set-vm-trace-level!** level [¶](#index-set_002dvm_002dtrace_002dlevel_0021)
:   Set the “trace level” of the VM.

See [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile), for more information on Guile’s
virtual machine.

---

Next: [Low-Level Traps](#Low_002dLevel-Traps), Previous: [VM Hooks](#VM-Hooks), Up: [Traps](#Traps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.5.2 Trap Interface [¶](#Trap-Interface-1)

The capabilities provided by hooks are great, but hooks alone rarely
correspond to what users want to do.

For example, if a user wants to break when and if control reaches a
certain source location, how do you do it? If you install a “next”
hook, you get unacceptable overhead for the execution of the entire
program. It would be possible to install an “apply” hook, then if the
procedure encompasses those source locations, install a “next” hook,
but already you’re talking about one concept that might be implemented
by a varying number of lower-level concepts.

It’s best to be clear about things and define one abstraction for all
such conditions: the *trap*.

Considering the myriad capabilities offered by the hooks though, there
is only a minimum of functionality shared by all traps. Guile’s current
take is to reduce this to the absolute minimum, and have the only
standard interface of a trap be “turn yourself on” or “turn yourself
off”.

This interface sounds a bit strange, but it is useful to procedurally
compose higher-level traps from lower-level building blocks. For
example, Guile defines a trap that calls one handler when control enters
a procedure, and another when control leaves the procedure. Given that
trap, one can define a trap that adds to the next-hook only when within
a given procedure. Building further, one can define a trap that fires
when control reaches particular instructions within a procedure.

Or of course you can stop at any of these intermediate levels. For
example, one might only be interested in calls to a given procedure. But
the point is that a simple enable/disable interface is all the
commonality that exists between the various kinds of traps, and
furthermore that such an interface serves to allow “higher-level”
traps to be composed from more primitive ones.

Specifically, a trap, in Guile, is a procedure. When a trap is created,
by convention the trap is enabled; therefore, the procedure that is the
trap will, when called, disable the trap, and return a procedure that
will enable the trap, and so on.

Trap procedures take one optional argument: the current frame. (A trap
may want to add to different sets of hooks depending on the frame that
is current at enable-time.)

If this all sounds very complicated, it’s because it is. Some of it is
essential, but probably most of it is not. The advantage of using this
minimal interface is that composability is more lexically apparent than
when, for example, using a stateful interface based on GOOPS. But
perhaps this reflects the cognitive limitations of the programmer who
made the current interface more than anything else.

---

Next: [Tracing Traps](#Tracing-Traps), Previous: [Trap Interface](#Trap-Interface), Up: [Traps](#Traps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.5.3 Low-Level Traps [¶](#Low_002dLevel-Traps-1)

To summarize the last sections, traps are enabled or disabled, and when
they are enabled, they add to various VM hooks.

Note, however, that *traps do not increase the VM trace level*. So
if you create a trap, it will be enabled, but unless something else
increases the VM’s trace level (see [VM Hooks](#VM-Hooks)), the trap will not
fire. It turns out that getting the VM trace level right is tricky
without a global view of what traps are enabled. See [Trap States](#Trap-States),
for Guile’s answer to this problem.

Traps are created by calling procedures. Most of these procedures share
a set of common keyword arguments, so rather than document them
separately, we discuss them all together here:

`#:vm`
:   The VM to instrument. Defaults to the current thread’s VM.

`#:current-frame`
:   For traps that enable more hooks depending on their dynamic context,
    this argument gives the current frame that the trap is running in.
    Defaults to `#f`.

To have access to these procedures, you’ll need to have imported the
`(system vm traps)` module:

```
(use-modules (system vm traps))
```

Scheme Procedure: **trap-at-procedure-call** proc handler [#:vm] [¶](#index-trap_002dat_002dprocedure_002dcall)
:   A trap that calls handler when proc is applied.

Scheme Procedure: **trap-in-procedure** proc enter-handler exit-handler [#:current-frame] [#:vm] [¶](#index-trap_002din_002dprocedure)
:   A trap that calls enter-handler when control enters proc,
    and exit-handler when control leaves proc.

    Control can enter a procedure via:

    * A procedure call.
    * A return to a procedure’s frame on the stack.
    * A continuation returning directly to an application of this procedure.

    Control can leave a procedure via:

    * A normal return from the procedure.
    * An application of another procedure.
    * An invocation of a continuation.
    * An abort.

Scheme Procedure: **trap-instructions-in-procedure** proc next-handler exit-handler [#:current-frame] [#:vm] [¶](#index-trap_002dinstructions_002din_002dprocedure)
:   A trap that calls next-handler for every instruction executed in
    proc, and exit-handler when execution leaves proc.

Scheme Procedure: **trap-at-procedure-ip-in-range** proc range handler [#:current-frame] [#:vm] [¶](#index-trap_002dat_002dprocedure_002dip_002din_002drange)
:   A trap that calls handler when execution enters a range of
    instructions in proc. range is a simple of pairs,
    `((start . end) ...)`. The start addresses are
    inclusive, and end addresses are exclusive.

Scheme Procedure: **trap-at-source-location** file user-line handler [#:current-frame] [#:vm] [¶](#index-trap_002dat_002dsource_002dlocation)
:   A trap that fires when control reaches a given source location. The
    user-line parameter is one-indexed, as a user counts lines,
    instead of zero-indexed, as Guile counts lines.

Scheme Procedure: **trap-frame-finish** frame return-handler abort-handler [#:vm] [¶](#index-trap_002dframe_002dfinish)
:   A trap that fires when control leaves the given frame. frame
    should be a live frame in the current continuation. return-handler
    will be called on a normal return, and abort-handler on a nonlocal
    exit.

Scheme Procedure: **trap-in-dynamic-extent** proc enter-handler return-handler abort-handler [#:vm] [¶](#index-trap_002din_002ddynamic_002dextent)
:   A more traditional dynamic-wind trap, which fires enter-handler
    when control enters proc, return-handler on a normal return,
    and abort-handler on a nonlocal exit.

    Note that rewinds are not handled, so there is no rewind handler.

Scheme Procedure: **trap-calls-in-dynamic-extent** proc apply-handler return-handler [#:current-frame] [#:vm] [¶](#index-trap_002dcalls_002din_002ddynamic_002dextent)
:   A trap that calls apply-handler every time a procedure is applied,
    and return-handler for returns, but only during the dynamic extent
    of an application of proc.

Scheme Procedure: **trap-instructions-in-dynamic-extent** proc next-handler [#:current-frame] [#:vm] [¶](#index-trap_002dinstructions_002din_002ddynamic_002dextent)
:   A trap that calls next-handler for all retired instructions within
    the dynamic extent of a call to proc.

Scheme Procedure: **trap-calls-to-procedure** proc apply-handler return-handler [#:vm] [¶](#index-trap_002dcalls_002dto_002dprocedure)
:   A trap that calls apply-handler whenever proc is applied,
    and return-handler when it returns, but with an additional
    argument, the call depth.

    That is to say, the handlers will get two arguments: the frame in
    question, and the call depth (a non-negative integer).

Scheme Procedure: **trap-matching-instructions** frame-pred handler [#:vm] [¶](#index-trap_002dmatching_002dinstructions)
:   A trap that calls frame-pred at every instruction, and if
    frame-pred returns a true value, calls handler on the
    frame.

---

Next: [Trap States](#Trap-States), Previous: [Low-Level Traps](#Low_002dLevel-Traps), Up: [Traps](#Traps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.5.4 Tracing Traps [¶](#Tracing-Traps-1)

The `(system vm trace)` module defines a number of traps for
tracing of procedure applications. When a procedure is *traced*, it
means that every call to that procedure is reported to the user during a
program run. The idea is that you can mark a collection of procedures
for tracing, and Guile will subsequently print out a line of the form

```
|  |  (procedure args ...)
```

whenever a marked procedure is about to be applied to its arguments.
This can help a programmer determine whether a function is being called
at the wrong time or with the wrong set of arguments.

In addition, the indentation of the output is useful for demonstrating
how the traced applications are or are not tail recursive with respect
to each other. Thus, a trace of a non-tail recursive factorial
implementation looks like this:

```
scheme@(guile-user)> (define (fact1 n) 
                       (if (zero? n) 1
                           (* n (fact1 (1- n)))))
scheme@(guile-user)> ,trace (fact1 4)
trace: (fact1 4)
trace: |  (fact1 3)
trace: |  |  (fact1 2)
trace: |  |  |  (fact1 1)
trace: |  |  |  |  (fact1 0)
trace: |  |  |  |  1
trace: |  |  |  1
trace: |  |  2
trace: |  6
trace: 24
```

While a typical tail recursive implementation would look more like this:

```
scheme@(guile-user)> (define (facti acc n)
                       (if (zero? n) acc
                           (facti (* n acc) (1- n))))
scheme@(guile-user)> (define (fact2 n) (facti 1 n))
scheme@(guile-user)> ,trace (fact2 4)
trace: (fact2 4)
trace: (facti 1 4)
trace: (facti 4 3)
trace: (facti 12 2)
trace: (facti 24 1)
trace: (facti 24 0)
trace: 24
```

The low-level traps below (see [Low-Level Traps](#Low_002dLevel-Traps)) share some common
options:

`#:width`
:   The maximum width of trace output. Trace printouts will try not to
    exceed this column, but for highly nested procedure calls, it may be
    unavoidable. Defaults to 80.

`#:vm`
:   The VM on which to add the traps. Defaults to the current thread’s VM.

`#:prefix`
:   A string to print out before each trace line. As seen above in the
    examples, defaults to `"trace: "`.

To have access to these procedures, you’ll need to have imported the
`(system vm trace)` module:

```
(use-modules (system vm trace))
```

Scheme Procedure: **trace-calls-to-procedure** proc [#:width] [#:vm] [#:prefix] [¶](#index-trace_002dcalls_002dto_002dprocedure)
:   Print a trace at applications of and returns from proc.

Scheme Procedure: **trace-calls-in-procedure** proc [#:width] [#:vm] [#:prefix] [¶](#index-trace_002dcalls_002din_002dprocedure)
:   Print a trace at all applications and returns within the dynamic extent
    of calls to proc.

Scheme Procedure: **trace-instructions-in-procedure** proc [#:width] [#:vm] [¶](#index-trace_002dinstructions_002din_002dprocedure)
:   Print a trace at all instructions executed in the dynamic extent of
    calls to proc.

In addition, Guile defines a procedure to call a thunk, tracing all
procedure calls and returns within the thunk.

Scheme Procedure: **call-with-trace** thunk [#:calls?=#t] [#:instructions?=#f] [#:width=80] [¶](#index-call_002dwith_002dtrace)
:   Call thunk, tracing all execution within its dynamic extent.

    If calls? is true, Guile will print a brief report at each
    procedure call and return, as given above.

    If instructions? is true, Guile will also print a message each
    time an instruction is executed. This is a lot of output, but it is
    sometimes useful when doing low-level optimization.

    Note that because this procedure manipulates the VM trace level
    directly, it doesn’t compose well with traps at the REPL.

See [Profile Commands](#Profile-Commands), for more information on tracing at the REPL.

---

Next: [High-Level Traps](#High_002dLevel-Traps), Previous: [Tracing Traps](#Tracing-Traps), Up: [Traps](#Traps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.5.5 Trap States [¶](#Trap-States-1)

When multiple traps are present in a system, we begin to have a
bookkeeping problem. How are they named? How does one disable, enable,
or delete them?

Guile’s answer to this is to keep an implicit per-thread *trap
state*. The trap state object is not exposed to the user; rather, API
that works on trap states fetches the current trap state from the
dynamic environment.

Traps are identified by integers. A trap can be enabled, disabled, or
removed, and can have an associated user-visible name.

These procedures have their own module:

```
(use-modules (system vm trap-state))
```

Scheme Procedure: **add-trap!** trap name [¶](#index-add_002dtrap_0021)
:   Add a trap to the current trap state, associating the given name
    with it. Returns a fresh trap identifier (an integer).

    Note that usually the more specific functions detailed in
    [High-Level Traps](#High_002dLevel-Traps) are used in preference to this one.

Scheme Procedure: **list-traps** [¶](#index-list_002dtraps)
:   List the current set of traps, both enabled and disabled. Returns a list
    of integers.

Scheme Procedure: **trap-name** idx [¶](#index-trap_002dname)
:   Returns the name associated with trap idx, or `#f` if there
    is no such trap.

Scheme Procedure: **trap-enabled?** idx [¶](#index-trap_002denabled_003f)
:   Returns `#t` if trap idx is present and enabled, or `#f`
    otherwise.

Scheme Procedure: **enable-trap!** idx [¶](#index-enable_002dtrap_0021)
:   Enables trap idx.

Scheme Procedure: **disable-trap!** idx [¶](#index-disable_002dtrap_0021)
:   Disables trap idx.

Scheme Procedure: **delete-trap!** idx [¶](#index-delete_002dtrap_0021)
:   Removes trap idx, disabling it first, if necessary.

---

Previous: [Trap States](#Trap-States), Up: [Traps](#Traps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.5.6 High-Level Traps [¶](#High_002dLevel-Traps-1)

The low-level trap API allows one to make traps that call procedures,
and the trap state API allows one to keep track of what traps are
there. But neither of these APIs directly helps you when you want to
set a breakpoint, because it’s unclear what to do when the trap fires.
Do you enter a debugger, or mail a summary of the situation to your
great-aunt, or what?

So for the common case in which you just want to install breakpoints,
and then have them all result in calls to one parameterizable procedure,
we have the high-level trap interface.

Perhaps we should have started this section with this interface, as it’s
clearly the one most people should use. But as its capabilities and
limitations proceed from the lower layers, we felt that the
character-building exercise of building a mental model might be helpful.

These procedures share a module with trap states:

```
(use-modules (system vm trap-state))
```

Scheme Procedure: **with-default-trap-handler** handler thunk [¶](#index-with_002ddefault_002dtrap_002dhandler)
:   Call thunk in a dynamic context in which handler is the
    current trap handler.

    Additionally, during the execution of thunk, the VM trace level
    (see [VM Hooks](#VM-Hooks)) is set to the number of enabled traps. This ensures
    that traps will in fact fire.

    handler may be `#f`, in which case VM hooks are not enabled
    as they otherwise would be, as there is nothing to handle the traps.

The trace-level-setting behavior of `with-default-trap-handler` is
one of its more useful aspects, but if you are willing to forgo that,
and just want to install a global trap handler, there’s a function for
that too:

Scheme Procedure: **install-trap-handler!** handler [¶](#index-install_002dtrap_002dhandler_0021)
:   Set the current thread’s trap handler to handler.

Trap handlers are called when traps installed by procedures from this
module fire. The current “consumer” of this API is Guile’s REPL, but
one might easily imagine other trap handlers being used to integrate
with other debugging tools.

Scheme Procedure: **add-trap-at-procedure-call!** proc [¶](#index-add_002dtrap_002dat_002dprocedure_002dcall_0021)
:   Install a trap that will fire when proc is called.

    This is a breakpoint.

Scheme Procedure: **add-trace-at-procedure-call!** proc [¶](#index-add_002dtrace_002dat_002dprocedure_002dcall_0021)
:   Install a trap that will print a tracing message when proc is
    called. See [Tracing Traps](#Tracing-Traps), for more information.

    This is a tracepoint.

Scheme Procedure: **add-trap-at-source-location!** file user-line [¶](#index-add_002dtrap_002dat_002dsource_002dlocation_0021)
:   Install a trap that will fire when control reaches the given source
    location. user-line is one-indexed, as users count lines, instead
    of zero-indexed, as Guile counts lines.

    This is a source breakpoint.

Scheme Procedure: **add-ephemeral-trap-at-frame-finish!** frame handler [¶](#index-add_002dephemeral_002dtrap_002dat_002dframe_002dfinish_0021)
:   Install a trap that will call handler when frame finishes
    executing. The trap will be removed from the trap state after firing, or
    on nonlocal exit.

    This is a finish trap, used to implement the “finish” REPL command.

Scheme Procedure: **add-ephemeral-stepping-trap!** frame handler [#:into?] [#:instruction?] [¶](#index-add_002dephemeral_002dstepping_002dtrap_0021)
:   Install a trap that will call handler after stepping to a
    different source line or instruction. The trap will be removed from the
    trap state after firing, or on nonlocal exit.

    If instruction? is false (the default), the trap will fire when
    control reaches a new source line. Otherwise it will fire when control
    reaches a new instruction.

    Additionally, if into? is false (not the default), the trap will
    only fire for frames at or prior to the given frame. If into? is
    true (the default), the trap may step into nested procedure
    invocations.

    This is a stepping trap, used to implement the “step”, “next”,
    “step-instruction”, and “next-instruction” REPL commands.

---

Previous: [Traps](#Traps), Up: [Debugging Infrastructure](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.26.6 GDB Support [¶](#GDB-Support-1)

Sometimes, you may find it necessary to debug Guile applications at the
C level. Doing so can be tedious, in particular because the debugger is
oblivious to Guile’s `SCM` type, and thus unable to display
`SCM` values in any meaningful way:

```
(gdb) frame
#0  scm_display (obj=0xf04310, port=0x6f9f30) at print.c:1437
```

To address that, Guile comes with an extension of the GNU Debugger (GDB)
that contains a “pretty-printer” for `SCM` values. With this GDB
extension, the C frame in the example above shows up like this:

```
(gdb) frame
#0  scm_display (obj=("hello" GDB!), port=#<port file 6f9f30>) at print.c:1437
```

Here GDB was able to decode the list pointed to by obj, and to
print it using Scheme’s read syntax.

That extension is a `.scm` file installed alongside the
`libguile` shared library. When GDB 7.8 or later is installed and
compiled with support for extensions written in Guile, the extension is
automatically loaded when debugging a program linked against
`libguile` (see [Auto-loading](/gdb/latest/en/gdb.html#Auto_002dloading) in Debugging with GDB). Note
that the directory where `libguile` is installed must be among
GDB’s auto-loading “safe directories” (see [Auto-loading safe
path](/gdb/latest/en/gdb.html#Auto_002dloading-safe-path) in Debugging with GDB).

---

Previous: [Debugging Infrastructure](#Debugging), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.27 Code Coverage Reports [¶](#Code-Coverage-Reports)

When writing a test suite for a program or library, it is desirable to know what
part of the code is *covered* by the test suite. The `(system vm
coverage)` module provides tools to gather code coverage data and to present
them, as detailed below.

Scheme Procedure: **with-code-coverage** thunk [¶](#index-with_002dcode_002dcoverage)
:   Run thunk, a zero-argument procedure, while instrumenting Guile’s
    virtual machine to collect code coverage data. Return code coverage
    data and the values returned by thunk.

Scheme Procedure: **coverage-data?** obj [¶](#index-coverage_002ddata_003f)
:   Return `#t` if obj is a *coverage data* object as returned by
    `with-code-coverage`.

Scheme Procedure: **coverage-data->lcov** data port #:key modules [¶](#index-coverage_002ddata_002d_003elcov)
:   Traverse code coverage information data, as obtained with
    `with-code-coverage`, and write coverage information to port in the
    `.info` format used by [LCOV](http://ltp.sourceforge.net/coverage/lcov.php). The report will include all of modules (or, by default, all the
    currently loaded modules) even if their code was not executed.

    The generated data can be fed to LCOV’s `genhtml` command to produce an
    HTML report, which aids coverage data visualization.

Here’s an example use:

```
(use-modules (system vm coverage)
             (system vm vm))

(call-with-values (lambda ()
                    (with-code-coverage
                      (lambda ()
                        (do-something-tricky))))
  (lambda (data result)
    (let ((port (open-output-file "lcov.info")))
      (coverage-data->lcov data port)
      (close port))))
```

In addition, the module provides low-level procedures that would make it
possible to write other user interfaces to the coverage data.

Scheme Procedures: **instrumented-source-files** data [¶](#index-instrumented_002dsource_002dfiles)
:   Return the list of “instrumented” source files, i.e., source files whose
    code was loaded at the time data was collected.

Scheme Procedures: **line-execution-counts** data file [¶](#index-line_002dexecution_002dcounts)
:   Return a list of line number/execution count pairs for file, or
    `#f` if file is not among the files covered by data. This
    includes lines with zero count.

Scheme Procedures: **instrumented/executed-lines** data file [¶](#index-instrumented_002fexecuted_002dlines)
:   Return the number of instrumented and the number of executed source lines
    in file according to data.

Scheme Procedures: **procedure-execution-count** data proc [¶](#index-procedure_002dexecution_002dcount)
:   Return the number of times proc’s code was executed, according to
    data, or `#f` if proc was not executed. When proc
    is a closure, the number of times its code was executed is returned, not
    the number of times this code associated with this particular closure was
    executed.

---

Next: [GOOPS](#GOOPS), Previous: [API Reference](#API-Reference), Up: [The Guile Reference Manual](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]## 7 Guile Modules [¶](#Guile-Modules-1)

* [SLIB](#SLIB)
* [POSIX System Calls and Networking](#POSIX)
* [HTTP, the Web, and All That](#Web)
* [The (ice-9 getopt-long) Module](#getopt_002dlong)
* [SRFI Support Modules](#SRFI-Support)
* [R6RS Support](#R6RS-Support)
* [R7RS Support](#R7RS-Support)
* [Pattern Matching](#Pattern-Matching)
* [Readline Support](#Readline-Support)
* [Pretty Printing](#Pretty-Printing)
* [Formatted Output](#Formatted-Output)
* [File Tree Walk](#File-Tree-Walk)
* [Queues](#Queues)
* [Streams](#Streams)
* [Buffered Input](#Buffered-Input)
* [Expect](#Expect)
* [`sxml-match`: Pattern Matching of SXML](#sxml_002dmatch)
* [The Scheme shell (scsh)](#The-Scheme-shell-_0028scsh_0029)
* [Curried Definitions](#Curried-Definitions)
* [Statprof](#Statprof)
* [SXML](#SXML)
* [Texinfo Processing](#Texinfo-Processing)

---

Next: [POSIX System Calls and Networking](#POSIX), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 7.1 SLIB [¶](#SLIB-1)

SLIB is a portable library of Scheme packages which can be used with
Guile and other Scheme implementations. SLIB is not included in the
Guile distribution, but can be installed separately (see [SLIB installation](#SLIB-installation)). It is available from
<http://people.csail.mit.edu/jaffer/SLIB.html>.

After SLIB is installed, the following Scheme expression must be
executed before the SLIB facilities can be used:

```
(use-modules (ice-9 slib))
```

`require` can then be used in the usual way (see [Require](slib.html#Require) in The SLIB Manual). For example,

```
(use-modules (ice-9 slib))
(require 'primes)
(prime? 13)
⇒ #t
```

A few Guile core functions are overridden by the SLIB setups; for
example the SLIB version of `delete-file` returns a boolean
indicating success or failure, whereas the Guile core version throws
an error for failure. In general (and as might be expected) when SLIB
is loaded it’s the SLIB specifications that are followed.

* [SLIB installation](#SLIB-installation)
* [JACAL](#JACAL)

---

Next: [JACAL](#JACAL), Up: [SLIB](#SLIB)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.1.1 SLIB installation [¶](#SLIB-installation-1)

The following procedure works, e.g., with SLIB version 3a3
(see [SLIB installation](slib.html#Installation) in The SLIB Portable Scheme
Library):

1. Unpack SLIB and install it using `make install` from its directory.
   By default, this will install SLIB in `/usr/local/lib/slib/`.
   Running `make install-info` installs its documentation, by default
   under `/usr/local/info/`.
2. Define the `SCHEME_LIBRARY_PATH` environment variable:

   ```
   $ SCHEME_LIBRARY_PATH=/usr/local/lib/slib/
   $ export SCHEME_LIBRARY_PATH
   ```

   Alternatively, you can create a symlink in the Guile directory to SLIB,
   e.g.:

   ```
   ln -s /usr/local/lib/slib /usr/local/share/guile/3.0/slib
   ```
3. Use Guile to create the catalog file, e.g.,:

   ```
   # guile
   guile> (use-modules (ice-9 slib))
   guile> (require 'new-catalog)
   guile> (quit)
   ```

   The catalog data should now be in
   `/usr/local/share/guile/3.0/slibcat`.

   If instead you get an error such as:

   ```
   Unbound variable: scheme-implementation-type
   ```

   then a solution is to get a newer version of Guile,
   or to modify `ice-9/slib.scm` to use `define-public` for the
   offending variables.

---

Previous: [SLIB installation](#SLIB-installation), Up: [SLIB](#SLIB)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.1.2 JACAL [¶](#JACAL-1)

Jacal is a symbolic math package written in Scheme by Aubrey Jaffer.
It is usually installed as an extra package in SLIB.

You can use Guile’s interface to SLIB to invoke Jacal:

```
(use-modules (ice-9 slib))
(slib:load "math")
(math)
```

For complete documentation on Jacal, please read the Jacal manual. If
it has been installed on line, you can look at [Jacal](jacal.html#Top) in JACAL Symbolic Mathematics System. Otherwise you can find it on the web at
<http://www-swiss.ai.mit.edu/~jaffer/JACAL.html>

---

Next: [HTTP, the Web, and All That](#Web), Previous: [SLIB](#SLIB), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
