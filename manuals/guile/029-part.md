In this way, uniform numeric vectors may be written to and read from
input/output ports using the procedures that operate on bytevectors.

See [Bytevectors](#Bytevectors), for more information.

---

Previous: [SRFI-4 - Relation to bytevectors](#SRFI_002d4-and-Bytevectors), Up: [SRFI-4 - Homogeneous numeric vector datatypes](#SRFI_002d4)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.5.4 SRFI-4 - Guile extensions [¶](#SRFI_002d4-_002d-Guile-extensions)

Guile defines some useful extensions to SRFI-4, which are not available in the
default Guile environment. They may be imported by loading the extensions
module:

```
(use-modules (srfi srfi-4 gnu))
```

Scheme Procedure: **srfi-4-vector-type-size** obj [¶](#index-srfi_002d4_002dvector_002dtype_002dsize)
:   Return the size, in bytes, of each element of SRFI-4 vector
    obj. For example, `(srfi-4-vector-type-size #u32())` returns
    `4`.

Scheme Procedure: **any->u8vector** obj [¶](#index-any_002d_003eu8vector)

Scheme Procedure: **any->s8vector** obj [¶](#index-any_002d_003es8vector)

Scheme Procedure: **any->u16vector** obj [¶](#index-any_002d_003eu16vector)

Scheme Procedure: **any->s16vector** obj [¶](#index-any_002d_003es16vector)

Scheme Procedure: **any->u32vector** obj [¶](#index-any_002d_003eu32vector)

Scheme Procedure: **any->s32vector** obj [¶](#index-any_002d_003es32vector)

Scheme Procedure: **any->u64vector** obj [¶](#index-any_002d_003eu64vector)

Scheme Procedure: **any->s64vector** obj [¶](#index-any_002d_003es64vector)

Scheme Procedure: **any->f32vector** obj [¶](#index-any_002d_003ef32vector)

Scheme Procedure: **any->f64vector** obj [¶](#index-any_002d_003ef64vector)

Scheme Procedure: **any->c32vector** obj [¶](#index-any_002d_003ec32vector)

Scheme Procedure: **any->c64vector** obj [¶](#index-any_002d_003ec64vector)

C Function: **scm\_any\_to\_u8vector** (obj) [¶](#index-scm_005fany_005fto_005fu8vector)

C Function: **scm\_any\_to\_s8vector** (obj) [¶](#index-scm_005fany_005fto_005fs8vector)

C Function: **scm\_any\_to\_u16vector** (obj) [¶](#index-scm_005fany_005fto_005fu16vector)

C Function: **scm\_any\_to\_s16vector** (obj) [¶](#index-scm_005fany_005fto_005fs16vector)

C Function: **scm\_any\_to\_u32vector** (obj) [¶](#index-scm_005fany_005fto_005fu32vector)

C Function: **scm\_any\_to\_s32vector** (obj) [¶](#index-scm_005fany_005fto_005fs32vector)

C Function: **scm\_any\_to\_u64vector** (obj) [¶](#index-scm_005fany_005fto_005fu64vector)

C Function: **scm\_any\_to\_s64vector** (obj) [¶](#index-scm_005fany_005fto_005fs64vector)

C Function: **scm\_any\_to\_f32vector** (obj) [¶](#index-scm_005fany_005fto_005ff32vector)

C Function: **scm\_any\_to\_f64vector** (obj) [¶](#index-scm_005fany_005fto_005ff64vector)

C Function: **scm\_any\_to\_c32vector** (obj) [¶](#index-scm_005fany_005fto_005fc32vector)

C Function: **scm\_any\_to\_c64vector** (obj) [¶](#index-scm_005fany_005fto_005fc64vector)
:   Return a (maybe newly allocated) uniform numeric vector of the indicated
    type, initialized with the elements of obj, which must be a list,
    a vector, or a uniform vector. When obj is already a suitable
    uniform numeric vector, it is returned unchanged.

Scheme Procedure: **u8vector-copy!** dst at src [start [end]] [¶](#index-u8vector_002dcopy_0021)

Scheme Procedure: **s8vector-copy!** dst at src [start [end]] [¶](#index-s8vector_002dcopy_0021)

Scheme Procedure: **u16vector-copy!** dst at src [start [end]] [¶](#index-u16vector_002dcopy_0021)

Scheme Procedure: **s16vector-copy!** dst at src [start [end]] [¶](#index-s16vector_002dcopy_0021)

Scheme Procedure: **u32vector-copy!** dst at src [start [end]] [¶](#index-u32vector_002dcopy_0021)

Scheme Procedure: **s32vector-copy!** dst at src [start [end]] [¶](#index-s32vector_002dcopy_0021)

Scheme Procedure: **u64vector-copy!** dst at src [start [end]] [¶](#index-u64vector_002dcopy_0021)

Scheme Procedure: **s64vector-copy!** dst at src [start [end]] [¶](#index-s64vector_002dcopy_0021)

Scheme Procedure: **f32vector-copy!** dst at src [start [end]] [¶](#index-f32vector_002dcopy_0021)

Scheme Procedure: **f64vector-copy!** dst at src [start [end]] [¶](#index-f64vector_002dcopy_0021)

Scheme Procedure: **c32vector-copy!** dst at src [start [end]] [¶](#index-c32vector_002dcopy_0021)

Scheme Procedure: **c64vector-copy!** dst at src [start [end]] [¶](#index-c64vector_002dcopy_0021)
:   Copy a block of elements from src to dst, both of which must
    be vectors of the indicated type, starting in dst at at and
    starting in src at start and ending at end. It is an
    error for dst to have a length less than at + (end -
    start). at and start default to 0 and end
    defaults to the length of src.

    If source and destination overlap, copying takes place as if the
    source is first copied into a temporary vector and then into the
    destination.

    See also [`vector-copy!`](#x_002dvector_002dcopy_0021).

Scheme Procedure: **u8vector-copy** src [start [end]] [¶](#index-u8vector_002dcopy)

Scheme Procedure: **s8vector-copy** src [start [end]] [¶](#index-s8vector_002dcopy)

Scheme Procedure: **u16vector-copy** src [start [end]] [¶](#index-u16vector_002dcopy)

Scheme Procedure: **s16vector-copy** src [start [end]] [¶](#index-s16vector_002dcopy)

Scheme Procedure: **u32vector-copy** src [start [end]] [¶](#index-u32vector_002dcopy)

Scheme Procedure: **s32vector-copy** src [start [end]] [¶](#index-s32vector_002dcopy)

Scheme Procedure: **u64vector-copy** src [start [end]] [¶](#index-u64vector_002dcopy)

Scheme Procedure: **s64vector-copy** src [start [end]] [¶](#index-s64vector_002dcopy)

Scheme Procedure: **f32vector-copy** src [start [end]] [¶](#index-f32vector_002dcopy)

Scheme Procedure: **f64vector-copy** src [start [end]] [¶](#index-f64vector_002dcopy)

Scheme Procedure: **c32vector-copy** src [start [end]] [¶](#index-c32vector_002dcopy)

Scheme Procedure: **c64vector-copy** src [start [end]] [¶](#index-c64vector_002dcopy)
:   Returns a freshly allocated vector of the indicated type, which must be
    the same as that of src, containing the elements of src
    between start and end. start defaults to 0 and
    end defaults to the length of src.

    See also [`vector-copy`](#x_002dvector_002dcopy).

---

Next: [SRFI-8 - receive](#SRFI_002d8), Previous: [SRFI-4 - Homogeneous numeric vector datatypes](#SRFI_002d4), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.6 SRFI-6 - Basic String Ports [¶](#SRFI_002d6-_002d-Basic-String-Ports)

SRFI-6 defines the procedures `open-input-string`,
`open-output-string` and `get-output-string`. These
procedures are included in the Guile core, so using this module does not
make any difference at the moment. But it is possible that support for
SRFI-6 will be factored out of the core library in the future, so using
this module does not hurt, after all.

---

Next: [SRFI-9 - define-record-type](#SRFI_002d9), Previous: [SRFI-6 - Basic String Ports](#SRFI_002d6), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.7 SRFI-8 - receive [¶](#SRFI_002d8-_002d-receive)

`receive` is a syntax for making the handling of multiple-value
procedures easier. It is documented in See [Returning and Accepting Multiple Values](#Multiple-Values).

---

Next: [SRFI-10 - Hash-Comma Reader Extension](#SRFI_002d10), Previous: [SRFI-8 - receive](#SRFI_002d8), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.8 SRFI-9 - define-record-type [¶](#SRFI_002d9-_002d-define_002drecord_002dtype)

This SRFI is a syntax for defining new record types and creating
predicate, constructor, and field getter and setter functions. It is
documented in the “Data Types” section of the manual (see [SRFI-9 Records](#SRFI_002d9-Records)).

---

Next: [SRFI-11 - let-values](#SRFI_002d11), Previous: [SRFI-9 - define-record-type](#SRFI_002d9), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.9 SRFI-10 - Hash-Comma Reader Extension [¶](#SRFI_002d10-_002d-Hash_002dComma-Reader-Extension)

This SRFI implements a reader extension `#,()` called hash-comma.
It allows the reader to give new kinds of objects, for use both in data
and as constants or literals in source code. This feature is available
with

```
(use-modules (srfi srfi-10))
```

The new read syntax is of the form

```
#,(tag arg...)
```

where tag is a symbol and the args are objects taken as
parameters. tags are registered with the following procedure.

Scheme Procedure: **define-reader-ctor** tag proc [¶](#index-define_002dreader_002dctor)
:   Register proc as the constructor for a hash-comma read syntax
    starting with symbol tag, i.e. `#,(tag arg…)`.
    proc is called with the given arguments `(proc
    arg…)` and the object it returns is the result of the read.

For example, a syntax giving a list of N copies of an object.

```
(define-reader-ctor 'repeat
  (lambda (obj reps)
    (make-list reps obj)))

(display '#,(repeat 99 3))
⊣ (99 99 99)
```

Notice the quote `'` when the `#,( )` is used. The
`repeat` handler returns a list and the program must quote to use
it literally, the same as any other list. Ie.

```
(display '#,(repeat 99 3))
⇒
(display '(99 99 99))
```

When a handler returns an object which is self-evaluating, like a
number or a string, then there’s no need for quoting, just as there’s
no need when giving those directly as literals. For example an
addition,

```
(define-reader-ctor 'sum
  (lambda (x y)
    (+ x y)))
(display #,(sum 123 456)) ⊣ 579
```

Once `(srfi srfi-10)` has loaded, `#,()` is available
globally, there’s no need to use `(srfi srfi-10)` in later
modules. Similarly the tags registered are global and can be used
anywhere once registered.

We do not recommend `#,()` reader extensions, however, and for
three reasons.

First of all, this SRFI is not modular: the tag is matched by name, not
as an identifier within a scope. Defining a reader extension in one
part of a program can thus affect unrelated parts of a program because
the tag is not scoped.

Secondly, reader extensions can be hard to manage from a time
perspective: when does the reader extension take effect? See [Eval-when](#Eval-When), for more discussion.

Finally, reader extensions can easily produce objects that can’t be
reified to an object file by the compiler. For example if you define a
reader extension that makes a hash table (see [Hash Tables](#Hash-Tables)), then it
will work fine when run with the interpreter, and you think you have a
neat hack. But then if you try to compile your program, after wrangling
with the `eval-when` concerns mentioned above, the compiler will
carp that it doesn’t know how to serialize a hash table to disk.

In the specific case of hash tables, it would be possible for Guile to
know how to pack hash tables into compiled files, but this doesn’t work
in general. What if the object you produce is an instance of a record
type? Guile would then have to serialize the record type to disk too,
and then what happens if the program independently loads the code that
defines the record type? Does it define the same type or a different
type? Guile’s record types are nominal, not structural, so the answer
is not clear at all.

For all of these reasons we recommend macros over reader extensions.
Macros fulfill many of the same needs while preserving modular
composition, and their interaction with `eval-when` is well-known.
If you need brevity, instead use `read-hash-extend` and make your
reader extension expand to a macro invocation. In that way we preserve
scoping as much as possible. See [Reader Extensions](#Reader-Extensions).

---

Next: [SRFI-13 - String Library](#SRFI_002d13), Previous: [SRFI-10 - Hash-Comma Reader Extension](#SRFI_002d10), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.10 SRFI-11 - let-values [¶](#SRFI_002d11-_002d-let_002dvalues)

This module implements the binding forms for multiple values
`let-values` and `let*-values`. These forms are similar to
`let` and `let*` (see [Local Variable Bindings](#Local-Bindings)), but they support
binding of the values returned by multiple-valued expressions.

Write `(use-modules (srfi srfi-11))` to make the bindings
available.

```
(let-values (((x y) (values 1 2))
             ((z f) (values 3 4)))
   (+ x y z f))
⇒
10
```

`let-values` performs all bindings simultaneously, which means that
no expression in the binding clauses may refer to variables bound in the
same clause list. `let*-values`, on the other hand, performs the
bindings sequentially, just like `let*` does for single-valued
expressions.

---

Next: [SRFI-14 - Character-set Library](#SRFI_002d14), Previous: [SRFI-11 - let-values](#SRFI_002d11), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.11 SRFI-13 - String Library [¶](#SRFI_002d13-_002d-String-Library)

The SRFI-13 procedures are always available, See [Strings](#Strings).

---

Next: [SRFI-16 - case-lambda](#SRFI_002d16), Previous: [SRFI-13 - String Library](#SRFI_002d13), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.12 SRFI-14 - Character-set Library [¶](#SRFI_002d14-_002d-Character_002dset-Library)

The SRFI-14 data type and procedures are always available,
See [Character Sets](#Character-Sets).

---

Next: [SRFI-17 - Generalized set!](#SRFI_002d17), Previous: [SRFI-14 - Character-set Library](#SRFI_002d14), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.13 SRFI-16 - case-lambda [¶](#SRFI_002d16-_002d-case_002dlambda)

SRFI-16 defines a variable-arity `lambda` form,
`case-lambda`. This form is available in the default Guile
environment. See [Case-lambda](#Case_002dlambda), for more information.

---

Next: [SRFI-18 - Multithreading support](#SRFI_002d18), Previous: [SRFI-16 - case-lambda](#SRFI_002d16), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.14 SRFI-17 - Generalized set! [¶](#SRFI_002d17-_002d-Generalized-set_0021)

This SRFI implements a generalized `set!`, allowing some
“referencing” functions to be used as the target location of a
`set!`. This feature is available from

```
(use-modules (srfi srfi-17))
```

For example `vector-ref` is extended so that

```
(set! (vector-ref vec idx) new-value)
```

is equivalent to

```
(vector-set! vec idx new-value)
```

The idea is that a `vector-ref` expression identifies a location,
which may be either fetched or stored. The same form is used for the
location in both cases, encouraging visual clarity. This is similar
to the idea of an “lvalue” in C.

The mechanism for this kind of `set!` is in the Guile core
(see [Procedures with Setters](#Procedures-with-Setters)). This module adds definitions of
the following functions as procedures with setters, allowing them to
be targets of a `set!`,

> `car`, `cdr`, `caar`, `cadr`,
> `cdar`, `cddr`, `caaar`, `caadr`,
> `cadar`, `caddr`, `cdaar`, `cdadr`,
> `cddar`, `cdddr`, `caaaar`, `caaadr`,
> `caadar`, `caaddr`, `cadaar`, `cadadr`,
> `caddar`, `cadddr`, `cdaaar`, `cdaadr`,
> `cdadar`, `cdaddr`, `cddaar`, `cddadr`,
> `cdddar`, `cddddr`
>
> `string-ref`, `vector-ref`

The SRFI specifies `setter` (see [Procedures with Setters](#Procedures-with-Setters)) as
a procedure with setter, allowing the setter for a procedure to be
changed, eg. `(set! (setter foo) my-new-setter-handler)`.
Currently Guile does not implement this, a setter can only be
specified on creation (`getter-with-setter` below).

Function: **getter-with-setter** [¶](#index-getter_002dwith_002dsetter)
:   The same as the Guile core `make-procedure-with-setter`
    (see [Procedures with Setters](#Procedures-with-Setters)).

---

Next: [SRFI-19 - Time/Date Library](#SRFI_002d19), Previous: [SRFI-17 - Generalized set!](#SRFI_002d17), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.15 SRFI-18 - Multithreading support [¶](#SRFI_002d18-_002d-Multithreading-support)

This is an implementation of the SRFI-18 threading and synchronization
library. The functions and variables described here are provided by

```
(use-modules (srfi srfi-18))
```

SRFI-18 defines facilities for threads, mutexes, condition variables,
time, and exception handling. Because these facilities are at a higher
level than Guile’s primitives, they are implemented as a layer on top of
what Guile provides. In particular this means that a Guile mutex is not
a SRFI-18 mutex, and a Guile thread is not a SRFI-18 thread, and so on.
Guile provides a set of primitives and SRFI-18 is one of the systems built in terms of those primitives.

* [SRFI-18 Threads](#SRFI_002d18-Threads)
* [SRFI-18 Mutexes](#SRFI_002d18-Mutexes)
* [SRFI-18 Condition variables](#SRFI_002d18-Condition-variables)
* [SRFI-18 Time](#SRFI_002d18-Time)
* [SRFI-18 Exceptions](#SRFI_002d18-Exceptions)

---

Next: [SRFI-18 Mutexes](#SRFI_002d18-Mutexes), Up: [SRFI-18 - Multithreading support](#SRFI_002d18)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.15.1 SRFI-18 Threads [¶](#SRFI_002d18-Threads-1)

Threads created by SRFI-18 differ in two ways from threads created by
Guile’s built-in thread functions. First, a thread created by SRFI-18
`make-thread` begins in a blocked state and will not start
execution until `thread-start!` is called on it. Second, SRFI-18
threads are constructed with a top-level exception handler that
captures any exceptions that are thrown on thread exit.

SRFI-18 threads are disjoint from Guile’s primitive threads.
See [Threads](#Threads), for more on Guile’s primitive facility.

Function: **current-thread** [¶](#index-current_002dthread-1)
:   Returns the thread that called this function. This is the same
    procedure as the same-named built-in procedure `current-thread`
    (see [Threads](#Threads)).

Function: **thread?** obj [¶](#index-thread_003f-1)
:   Returns `#t` if obj is a thread, `#f` otherwise. This
    is the same procedure as the same-named built-in procedure
    `thread?` (see [Threads](#Threads)).

Function: **make-thread** thunk [name] [¶](#index-make_002dthread-1)
:   Call `thunk` in a new thread and with a new dynamic state,
    returning the new thread and optionally assigning it the object name
    name, which may be any Scheme object.

    Note that the name `make-thread` conflicts with the
    `(ice-9 threads)` function `make-thread`. Applications
    wanting to use both of these functions will need to refer to them by
    different names.

Function: **thread-name** thread [¶](#index-thread_002dname)
:   Returns the name assigned to thread at the time of its creation,
    or `#f` if it was not given a name.

Function: **thread-specific** thread [¶](#index-thread_002dspecific)

Function: **thread-specific-set!** thread obj [¶](#index-thread_002dspecific_002dset_0021)
:   Get or set the “object-specific” property of thread. In
    Guile’s implementation of SRFI-18, this value is stored as an object
    property, and will be `#f` if not set.

Function: **thread-start!** thread [¶](#index-thread_002dstart_0021)
:   Unblocks thread and allows it to begin execution if it has not
    done so already.

Function: **thread-yield!** [¶](#index-thread_002dyield_0021)
:   If one or more threads are waiting to execute, calling
    `thread-yield!` forces an immediate context switch to one of them.
    Otherwise, `thread-yield!` has no effect. `thread-yield!`
    behaves identically to the Guile built-in function `yield`.

Function: **thread-sleep!** timeout [¶](#index-thread_002dsleep_0021)
:   The current thread waits until the point specified by the time object
    timeout is reached (see [SRFI-18 Time](#SRFI_002d18-Time)). This blocks the
    thread only if timeout represents a point in the future. it is
    an error for timeout to be `#f`.

Function: **thread-terminate!** thread [¶](#index-thread_002dterminate_0021)
:   Causes an abnormal termination of thread. If thread is
    not already terminated, all mutexes owned by thread become
    unlocked/abandoned. If thread is the current thread,
    `thread-terminate!` does not return. Otherwise
    `thread-terminate!` returns an unspecified value; the termination
    of thread will occur before `thread-terminate!` returns.
    Subsequent attempts to join on thread will cause a “terminated
    thread exception” to be raised.

    `thread-terminate!` is compatible with the thread cancellation
    procedures in the core threads API (see [Threads](#Threads)) in that if a
    cleanup handler has been installed for the target thread, it will be
    called before the thread exits and its return value (or exception, if
    any) will be stored for later retrieval via a call to
    `thread-join!`.

Function: **thread-join!** thread [timeout [timeout-val]] [¶](#index-thread_002djoin_0021)
:   Wait for thread to terminate and return its exit value. When a
    time value timeout is given, it specifies a point in time where
    the waiting should be aborted. When the waiting is aborted,
    timeout-val is returned if it is specified; otherwise, a
    `join-timeout-exception` exception is raised
    (see [SRFI-18 Exceptions](#SRFI_002d18-Exceptions)). Exceptions may also be raised if the
    thread was terminated by a call to `thread-terminate!`
    (`terminated-thread-exception` will be raised) or if the thread
    exited by raising an exception that was handled by the top-level
    exception handler (`uncaught-exception` will be raised; the
    original exception can be retrieved using
    `uncaught-exception-reason`).

---

Next: [SRFI-18 Condition variables](#SRFI_002d18-Condition-variables), Previous: [SRFI-18 Threads](#SRFI_002d18-Threads), Up: [SRFI-18 - Multithreading support](#SRFI_002d18)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.15.2 SRFI-18 Mutexes [¶](#SRFI_002d18-Mutexes-1)

SRFI-18 mutexes are disjoint from Guile’s primitive mutexes.
See [Mutexes and Condition Variables](#Mutexes-and-Condition-Variables), for more on Guile’s primitive
facility.

Function: **make-mutex** [name] [¶](#index-make_002dmutex-1)
:   Returns a new mutex, optionally assigning it the object name name,
    which may be any Scheme object. The returned mutex will be created with
    the configuration described above.

Function: **mutex-name** mutex [¶](#index-mutex_002dname)
:   Returns the name assigned to mutex at the time of its creation, or
    `#f` if it was not given a name.

Function: **mutex-specific** mutex [¶](#index-mutex_002dspecific)
:   Return the “object-specific” property of mutex, or `#f` if
    none is set.

Function: **mutex-specific-set!** mutex obj [¶](#index-mutex_002dspecific_002dset_0021)
:   Set the “object-specific” property of mutex.

Function: **mutex-state** mutex [¶](#index-mutex_002dstate)
:   Returns information about the state of mutex. Possible values
    are:

    * thread t: the mutex is in the locked/owned state and thread
      t is the owner of the mutex
    * symbol `not-owned`: the mutex is in the locked/not-owned state
    * symbol `abandoned`: the mutex is in the unlocked/abandoned state
    * symbol `not-abandoned`: the mutex is in the
      unlocked/not-abandoned state

Function: **mutex-lock!** mutex [timeout [thread]] [¶](#index-mutex_002dlock_0021)
:   Lock mutex, optionally specifying a time object timeout
    after which to abort the lock attempt and a thread thread giving
    a new owner for mutex different than the current thread.

Function: **mutex-unlock!** mutex [condition-variable [timeout]] [¶](#index-mutex_002dunlock_0021)
:   Unlock mutex, optionally specifying a condition variable
    condition-variable on which to wait, either indefinitely or,
    optionally, until the time object timeout has passed, to be
    signaled.

---

Next: [SRFI-18 Time](#SRFI_002d18-Time), Previous: [SRFI-18 Mutexes](#SRFI_002d18-Mutexes), Up: [SRFI-18 - Multithreading support](#SRFI_002d18)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.15.3 SRFI-18 Condition variables [¶](#SRFI_002d18-Condition-variables-1)

SRFI-18 does not specify a “wait” function for condition variables.
Waiting on a condition variable can be simulated using the SRFI-18
`mutex-unlock!` function described in the previous section.

SRFI-18 condition variables are disjoint from Guile’s primitive
condition variables. See [Mutexes and Condition Variables](#Mutexes-and-Condition-Variables), for more
on Guile’s primitive facility.

Function: **condition-variable?** obj [¶](#index-condition_002dvariable_003f-1)
:   Returns `#t` if obj is a condition variable, `#f`
    otherwise.

Function: **make-condition-variable** [name] [¶](#index-make_002dcondition_002dvariable-1)
:   Returns a new condition variable, optionally assigning it the object
    name name, which may be any Scheme object.

Function: **condition-variable-name** condition-variable [¶](#index-condition_002dvariable_002dname)
:   Returns the name assigned to condition-variable at the time of its
    creation, or `#f` if it was not given a name.

Function: **condition-variable-specific** condition-variable [¶](#index-condition_002dvariable_002dspecific)
:   Return the “object-specific” property of condition-variable, or
    `#f` if none is set.

Function: **condition-variable-specific-set!** condition-variable obj [¶](#index-condition_002dvariable_002dspecific_002dset_0021)
:   Set the “object-specific” property of condition-variable.

Function: **condition-variable-signal!** condition-variable [¶](#index-condition_002dvariable_002dsignal_0021)

Function: **condition-variable-broadcast!** condition-variable [¶](#index-condition_002dvariable_002dbroadcast_0021)
:   Wake up one thread that is waiting for condition-variable, in
    the case of `condition-variable-signal!`, or all threads waiting
    for it, in the case of `condition-variable-broadcast!`.

---

Next: [SRFI-18 Exceptions](#SRFI_002d18-Exceptions), Previous: [SRFI-18 Condition variables](#SRFI_002d18-Condition-variables), Up: [SRFI-18 - Multithreading support](#SRFI_002d18)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.15.4 SRFI-18 Time [¶](#SRFI_002d18-Time-1)

The SRFI-18 time functions manipulate time in two formats: a
“time object” type that represents an absolute point in time in some
implementation-specific way; and the number of seconds since some
unspecified “epoch”. In Guile’s implementation, the epoch is the
Unix epoch, 00:00:00 UTC, January 1, 1970.

Function: **current-time** [¶](#index-current_002dtime-1)
:   Return the current time as a time object. This procedure replaces
    the procedure of the same name in the core library, which returns the
    current time in seconds since the epoch.

Function: **time?** obj [¶](#index-time_003f)
:   Returns `#t` if obj is a time object, `#f` otherwise.

Function: **time->seconds** time [¶](#index-time_002d_003eseconds)

Function: **seconds->time** seconds [¶](#index-seconds_002d_003etime)
:   Convert between time objects and numerical values representing the
    number of seconds since the epoch. When converting from a time object
    to seconds, the return value is the number of seconds between
    time and the epoch. When converting from seconds to a time
    object, the return value is a time object that represents a time
    seconds seconds after the epoch.

---

Previous: [SRFI-18 Time](#SRFI_002d18-Time), Up: [SRFI-18 - Multithreading support](#SRFI_002d18)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.15.5 SRFI-18 Exceptions [¶](#SRFI_002d18-Exceptions-1)

SRFI-18 exceptions are identical to the exceptions provided by
Guile’s implementation of SRFI-34. The behavior of exception
handlers invoked to handle exceptions thrown from SRFI-18 functions,
however, differs from the conventional behavior of SRFI-34 in that
the continuation of the handler is the same as that of the call to
the function. Handlers are called in a tail-recursive manner; the
exceptions do not “bubble up”.

Function: **current-exception-handler** [¶](#index-current_002dexception_002dhandler)
:   Returns the current exception handler.

Function: **with-exception-handler** handler thunk [¶](#index-with_002dexception_002dhandler-1)
:   Installs handler as the current exception handler and calls the
    procedure thunk with no arguments, returning its value as the
    value of the exception. handler must be a procedure that accepts
    a single argument. The current exception handler at the time this
    procedure is called will be restored after the call returns.

Function: **raise** obj [¶](#index-raise-1)
:   Raise obj as an exception. This is the same procedure as the
    same-named procedure defined in SRFI 34.

Function: **join-timeout-exception?** obj [¶](#index-join_002dtimeout_002dexception_003f)
:   Returns `#t` if obj is an exception raised as the result of
    performing a timed join on a thread that does not exit within the
    specified timeout, `#f` otherwise.

Function: **abandoned-mutex-exception?** obj [¶](#index-abandoned_002dmutex_002dexception_003f)
:   Returns `#t` if obj is an exception raised as the result of
    attempting to lock a mutex that has been abandoned by its owner thread,
    `#f` otherwise.

Function: **terminated-thread-exception?** obj [¶](#index-terminated_002dthread_002dexception_003f)
:   Returns `#t` if obj is an exception raised as the result of
    joining on a thread that exited as the result of a call to
    `thread-terminate!`.

Function: **uncaught-exception?** obj [¶](#index-uncaught_002dexception_003f)

Function: **uncaught-exception-reason** exc [¶](#index-uncaught_002dexception_002dreason)
:   `uncaught-exception?` returns `#t` if obj is an
    exception thrown as the result of joining a thread that exited by
    raising an exception that was handled by the top-level exception
    handler installed by `make-thread`. When this occurs, the
    original exception is preserved as part of the exception thrown by
    `thread-join!` and can be accessed by calling
    `uncaught-exception-reason` on that exception. Note that
    because this exception-preservation mechanism is a side-effect of
    `make-thread`, joining on threads that exited as described above
    but were created by other means will not raise this
    `uncaught-exception` error.

---

Next: [SRFI-23 - Error Reporting](#SRFI_002d23), Previous: [SRFI-18 - Multithreading support](#SRFI_002d18), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.16 SRFI-19 - Time/Date Library [¶](#SRFI_002d19-_002d-Time_002fDate-Library)

This is an implementation of the SRFI-19 time/date library. The
functions and variables described here are provided by

```
(use-modules (srfi srfi-19))
```

* [SRFI-19 Introduction](#SRFI_002d19-Introduction)
* [SRFI-19 Time](#SRFI_002d19-Time)
* [SRFI-19 Date](#SRFI_002d19-Date)
* [SRFI-19 Time/Date conversions](#SRFI_002d19-Time_002fDate-conversions)
* [SRFI-19 Date to string](#SRFI_002d19-Date-to-string)
* [SRFI-19 String to date](#SRFI_002d19-String-to-date)

---

Next: [SRFI-19 Time](#SRFI_002d19-Time), Up: [SRFI-19 - Time/Date Library](#SRFI_002d19)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.16.1 SRFI-19 Introduction [¶](#SRFI_002d19-Introduction-1)

This module implements time and date representations and calculations,
in various time systems, including Coordinated Universal Time (UTC)
and International Atomic Time (TAI).

For those not familiar with these time systems, TAI is based on a
fixed length second derived from oscillations of certain atoms. UTC
differs from TAI by an integral number of seconds, which is increased
or decreased at announced times to keep UTC aligned to a mean solar
day (the orbit and rotation of the earth are not quite constant).

So far, only increases in the TAI
<->
UTC difference have been needed. Such an increase is a “leap
second”, an extra second of TAI introduced at the end of a UTC day.
When working entirely within UTC this is never seen, every day simply
has 86400 seconds. But when converting from TAI to a UTC date, an
extra 23:59:60 is present, where normally a day would end at 23:59:59.
Effectively the UTC second from 23:59:59 to 00:00:00 has taken two TAI
seconds.

In the current implementation, the system clock is assumed to be UTC,
and a table of leap seconds in the code converts to TAI. See comments
in `srfi-19.scm` for how to update this table.

Also, for those not familiar with the terminology, a *Julian Day*
represents a point in time as a real number of days since
-4713-11-24T12:00:00Z, i.e. midday UT on 24 November 4714 BC in the
proleptic Gregorian calendar (1 January 4713 BC in the proleptic Julian
calendar).

A *Modified Julian Day* represents a point in time as a real number
of days since 1858-11-17T00:00:00Z, i.e. midnight UT on Wednesday 17
November AD 1858. That time is julian day 2400000.5.

---

Next: [SRFI-19 Date](#SRFI_002d19-Date), Previous: [SRFI-19 Introduction](#SRFI_002d19-Introduction), Up: [SRFI-19 - Time/Date Library](#SRFI_002d19)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.16.2 SRFI-19 Time [¶](#SRFI_002d19-Time-1)

A *time* object has type, seconds and nanoseconds fields
representing a point in time starting from some epoch. This is an
arbitrary point in time, not just a time of day. Although times are
represented in nanoseconds, the actual resolution may be lower.

The following variables hold the possible time types. For instance
`(current-time time-process)` would give the current CPU process
time.

Variable: **time-utc** [¶](#index-time_002dutc)
:   Universal Coordinated Time (UTC).

Variable: **time-tai** [¶](#index-time_002dtai)
:   International Atomic Time (TAI).

Variable: **time-monotonic** [¶](#index-time_002dmonotonic)
:   Monotonic time, meaning a monotonically increasing time starting from
    an unspecified epoch.

    Note that in the current implementation `time-monotonic` is the
    same as `time-tai`, and unfortunately is therefore affected by
    adjustments to the system clock. Perhaps this will change in the
    future.

Variable: **time-duration** [¶](#index-time_002dduration)
:   A duration, meaning simply a difference between two times.

Variable: **time-process** [¶](#index-time_002dprocess)
:   CPU time spent in the current process, starting from when the process
    began.

Variable: **time-thread** [¶](#index-time_002dthread)
:   CPU time spent in the current thread. Not currently implemented.

  

Function: **time?** obj [¶](#index-time_003f-1)
:   Return `#t` if obj is a time object, or `#f` if not.

Function: **make-time** type nanoseconds seconds [¶](#index-make_002dtime)
:   Create a time object with the given type, seconds and
    nanoseconds.

Function: **time-type** time [¶](#index-time_002dtype)

Function: **time-nanosecond** time [¶](#index-time_002dnanosecond)

Function: **time-second** time [¶](#index-time_002dsecond)

Function: **set-time-type!** time type [¶](#index-set_002dtime_002dtype_0021)

Function: **set-time-nanosecond!** time nsec [¶](#index-set_002dtime_002dnanosecond_0021)

Function: **set-time-second!** time sec [¶](#index-set_002dtime_002dsecond_0021)
:   Get or set the type, seconds or nanoseconds fields of a time object.

    `set-time-type!` merely changes the field, it doesn’t convert the
    time value. For conversions, see [SRFI-19 Time/Date conversions](#SRFI_002d19-Time_002fDate-conversions).

Function: **copy-time** time [¶](#index-copy_002dtime)
:   Return a new time object, which is a copy of the given time.

Function: **current-time** [type] [¶](#index-current_002dtime-2)
:   Return the current time of the given type. The default
    type is `time-utc`.

    Note that the name `current-time` conflicts with the Guile core
    `current-time` function (see [Time](#Time)) as well as the SRFI-18
    `current-time` function (see [SRFI-18 Time](#SRFI_002d18-Time)). Applications
    wanting to use more than one of these functions will need to refer to
    them by different names.

Function: **time-resolution** [type] [¶](#index-time_002dresolution)
:   Return the resolution, in nanoseconds, of the given time type.
    The default type is `time-utc`.

Function: **time<=?** t1 t2 [¶](#index-time_003c_003d_003f)

Function: **time<?** t1 t2 [¶](#index-time_003c_003f)

Function: **time=?** t1 t2 [¶](#index-time_003d_003f)

Function: **time>=?** t1 t2 [¶](#index-time_003e_003d_003f)

Function: **time>?** t1 t2 [¶](#index-time_003e_003f)
:   Return `#t` or `#f` according to the respective relation
    between time objects t1 and t2. t1 and t2
    must be the same time type.

Function: **time-difference** t1 t2 [¶](#index-time_002ddifference)

Function: **time-difference!** t1 t2 [¶](#index-time_002ddifference_0021)
:   Return a time object of type `time-duration` representing the
    period between t1 and t2. t1 and t2 must be
    the same time type.

    `time-difference` returns a new time object,
    `time-difference!` may modify t1 to form its return.

Function: **add-duration** time duration [¶](#index-add_002dduration)

Function: **add-duration!** time duration [¶](#index-add_002dduration_0021)

Function: **subtract-duration** time duration [¶](#index-subtract_002dduration)

Function: **subtract-duration!** time duration [¶](#index-subtract_002dduration_0021)
:   Return a time object which is time with the given duration
    added or subtracted. duration must be a time object of type
    `time-duration`.

    `add-duration` and `subtract-duration` return a new time
    object. `add-duration!` and `subtract-duration!` may modify
    the given time to form their return.

---

Next: [SRFI-19 Time/Date conversions](#SRFI_002d19-Time_002fDate-conversions), Previous: [SRFI-19 Time](#SRFI_002d19-Time), Up: [SRFI-19 - Time/Date Library](#SRFI_002d19)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.16.3 SRFI-19 Date [¶](#SRFI_002d19-Date-1)

A *date* object represents a date in the Gregorian calendar and a
time of day on that date in some timezone.

The fields are year, month, day, hour, minute, second, nanoseconds and
timezone. A date object is immutable, its fields can be read but they
cannot be modified once the object is created.

Historically, the Gregorian calendar was only used from the latter part
of the year 1582 onwards, and not until even later in many countries.
Prior to that most countries used the Julian calendar. SRFI-19 does
not deal with the Julian calendar at all, and so does not reflect this
historical calendar reform. Instead it projects the Gregorian calendar
back proleptically as far as necessary. When dealing with historical
data, especially prior to the British Empire’s adoption of the Gregorian
calendar in 1752, one should be mindful of which calendar is used in
each context, and apply non-SRFI-19 facilities to convert where necessary.

Function: **date?** obj [¶](#index-date_003f)
:   Return `#t` if obj is a date object, or `#f` if not.

Function: **make-date** nsecs seconds minutes hours day month year zone-offset [¶](#index-make_002ddate)
:   Create a new date object.

Function: **date-nanosecond** date [¶](#index-date_002dnanosecond)
:   Nanoseconds, 0 to 999999999.

Function: **date-second** date [¶](#index-date_002dsecond)
:   Seconds, 0 to 59, or 60 for a leap second. 60 is never seen when working
    entirely within UTC, it’s only when converting to or from TAI.

Function: **date-minute** date [¶](#index-date_002dminute)
:   Minutes, 0 to 59.

Function: **date-hour** date [¶](#index-date_002dhour)
:   Hour, 0 to 23.

Function: **date-day** date [¶](#index-date_002dday)
:   Day of the month, 1 to 31 (or less, according to the month).

Function: **date-month** date [¶](#index-date_002dmonth)
:   Month, 1 to 12.

Function: **date-year** date [¶](#index-date_002dyear)
:   Year, eg. 2003. Dates B.C. are negative, eg. *-46* is 46
    B.C. There is no year 0, year *-1* is followed by year 1.

Function: **date-zone-offset** date [¶](#index-date_002dzone_002doffset)
:   Time zone, an integer number of seconds east of Greenwich.

Function: **date-year-day** date [¶](#index-date_002dyear_002dday)
:   Day of the year, starting from 1 for 1st January.

Function: **date-week-day** date [¶](#index-date_002dweek_002dday)
:   Day of the week, starting from 0 for Sunday.

Function: **date-week-number** date dstartw [¶](#index-date_002dweek_002dnumber)
:   Week of the year, ignoring a first partial week. dstartw is the
    day of the week which is taken to start a week, 0 for Sunday, 1 for
    Monday, etc.

Function: **current-date** [tz-offset] [¶](#index-current_002ddate)
:   Return a date object representing the current date/time, in UTC offset
    by tz-offset. tz-offset is seconds east of Greenwich and
    defaults to the local timezone.

Function: **current-julian-day** [¶](#index-current_002djulian_002dday)
:   Return the current Julian Day.

Function: **current-modified-julian-day** [¶](#index-current_002dmodified_002djulian_002dday)
:   Return the current Modified Julian Day.

---

Next: [SRFI-19 Date to string](#SRFI_002d19-Date-to-string), Previous: [SRFI-19 Date](#SRFI_002d19-Date), Up: [SRFI-19 - Time/Date Library](#SRFI_002d19)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.16.4 SRFI-19 Time/Date conversions [¶](#SRFI_002d19-Time_002fDate-conversions-1)

Function: **date->julian-day** date [¶](#index-date_002d_003ejulian_002dday)

Function: **date->modified-julian-day** date [¶](#index-date_002d_003emodified_002djulian_002dday)

Function: **date->time-monotonic** date [¶](#index-date_002d_003etime_002dmonotonic)

Function: **date->time-tai** date [¶](#index-date_002d_003etime_002dtai)

Function: **date->time-utc** date [¶](#index-date_002d_003etime_002dutc)

Function: **julian-day->date** jdn [tz-offset] [¶](#index-julian_002dday_002d_003edate)

Function: **julian-day->time-monotonic** jdn [¶](#index-julian_002dday_002d_003etime_002dmonotonic)

Function: **julian-day->time-tai** jdn [¶](#index-julian_002dday_002d_003etime_002dtai)

Function: **julian-day->time-utc** jdn [¶](#index-julian_002dday_002d_003etime_002dutc)

Function: **modified-julian-day->date** jdn [tz-offset] [¶](#index-modified_002djulian_002dday_002d_003edate)

Function: **modified-julian-day->time-monotonic** jdn [¶](#index-modified_002djulian_002dday_002d_003etime_002dmonotonic)

Function: **modified-julian-day->time-tai** jdn [¶](#index-modified_002djulian_002dday_002d_003etime_002dtai)

Function: **modified-julian-day->time-utc** jdn [¶](#index-modified_002djulian_002dday_002d_003etime_002dutc)

Function: **time-monotonic->date** time [tz-offset] [¶](#index-time_002dmonotonic_002d_003edate)

Function: **time-monotonic->time-tai** time [¶](#index-time_002dmonotonic_002d_003etime_002dtai)

Function: **time-monotonic->time-tai!** time [¶](#index-time_002dmonotonic_002d_003etime_002dtai_0021)

Function: **time-monotonic->time-utc** time [¶](#index-time_002dmonotonic_002d_003etime_002dutc)

Function: **time-monotonic->time-utc!** time [¶](#index-time_002dmonotonic_002d_003etime_002dutc_0021)

Function: **time-tai->date** time [tz-offset] [¶](#index-time_002dtai_002d_003edate)

Function: **time-tai->julian-day** time [¶](#index-time_002dtai_002d_003ejulian_002dday)

Function: **time-tai->modified-julian-day** time [¶](#index-time_002dtai_002d_003emodified_002djulian_002dday)

Function: **time-tai->time-monotonic** time [¶](#index-time_002dtai_002d_003etime_002dmonotonic)

Function: **time-tai->time-monotonic!** time [¶](#index-time_002dtai_002d_003etime_002dmonotonic_0021)

Function: **time-tai->time-utc** time [¶](#index-time_002dtai_002d_003etime_002dutc)

Function: **time-tai->time-utc!** time [¶](#index-time_002dtai_002d_003etime_002dutc_0021)

Function: **time-utc->date** time [tz-offset] [¶](#index-time_002dutc_002d_003edate)

Function: **time-utc->julian-day** time [¶](#index-time_002dutc_002d_003ejulian_002dday)

Function: **time-utc->modified-julian-day** time [¶](#index-time_002dutc_002d_003emodified_002djulian_002dday)

Function: **time-utc->time-monotonic** time [¶](#index-time_002dutc_002d_003etime_002dmonotonic)

Function: **time-utc->time-monotonic!** time [¶](#index-time_002dutc_002d_003etime_002dmonotonic_0021)

Function: **time-utc->time-tai** time [¶](#index-time_002dutc_002d_003etime_002dtai)

Function: **time-utc->time-tai!** time [¶](#index-time_002dutc_002d_003etime_002dtai_0021)
:   Convert between dates, times and days of the respective types. For
    instance `time-tai->time-utc` accepts a time object of type
    `time-tai` and returns an object of type `time-utc`.

    The `!` variants may modify their time argument to form
    their return. The plain functions create a new object.

    For conversions to dates, tz-offset is seconds east of
    Greenwich. The default is the local timezone, at the given time, as
    provided by the system, using `localtime` (see [Time](#Time)).

    On 32-bit systems, `localtime` is limited to a 32-bit
    `time_t`, so a default tz-offset is only available for
    times between Dec 1901 and Jan 2038. For prior dates an application
    might like to use the value in 1902, though some locations have zone
    changes prior to that. For future dates an application might like to
    assume today’s rules extend indefinitely. But for correct daylight
    savings transitions it will be necessary to take an offset for the
    same day and time but a year in range and which has the same starting
    weekday and same leap/non-leap (to support rules like last Sunday in
    October).

---

Next: [SRFI-19 String to date](#SRFI_002d19-String-to-date), Previous: [SRFI-19 Time/Date conversions](#SRFI_002d19-Time_002fDate-conversions), Up: [SRFI-19 - Time/Date Library](#SRFI_002d19)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.16.5 SRFI-19 Date to string [¶](#SRFI_002d19-Date-to-string-1)

Function: **date->string** date [format] [¶](#index-date_002d_003estring)
:   Convert a date to a string under the control of a format.
    format should be a string containing ‘`~`’ escapes, which
    will be expanded as per the following conversion table. The default
    format is ‘`~c`’, a locale-dependent date and time.

    Many of these conversion characters are the same as POSIX
    `strftime` (see [Time](#Time)), but there are some extras and some
    variations.

    |  |  |
    | --- | --- |
    | `~~` | literal ~ |
    | `~a` | locale abbreviated weekday, eg. ‘`Sun`’ |
    | `~A` | locale full weekday, eg. ‘`Sunday`’ |
    | `~b` | locale abbreviated month, eg. ‘`Jan`’ |
    | `~B` | locale full month, eg. ‘`January`’ |
    | `~c` | locale date and time, eg.   ‘`Fri Jul 14 20:28:42-0400 2000`’ |
    | `~d` | day of month, zero padded, ‘`01`’ to ‘`31`’ |
    | `~e` | day of month, blank padded, ‘ `1`’ to ‘`31`’ |
    | `~f` | seconds and fractional seconds, with locale decimal point, eg. ‘`5.2`’ |
    | `~h` | same as `~b` |
    | `~H` | hour, 24-hour clock, zero padded, ‘`00`’ to ‘`23`’ |
    | `~I` | hour, 12-hour clock, zero padded, ‘`01`’ to ‘`12`’ |
    | `~j` | day of year, zero padded, ‘`001`’ to ‘`366`’ |
    | `~k` | hour, 24-hour clock, blank padded, ‘ `0`’ to ‘`23`’ |
    | `~l` | hour, 12-hour clock, blank padded, ‘ `1`’ to ‘`12`’ |
    | `~m` | month, zero padded, ‘`01`’ to ‘`12`’ |
    | `~M` | minute, zero padded, ‘`00`’ to ‘`59`’ |
    | `~n` | newline |
    | `~N` | nanosecond, zero padded, ‘`000000000`’ to ‘`999999999`’ |
    | `~p` | locale AM or PM |
    | `~r` | time, 12 hour clock, ‘`~I:~M:~S ~p`’ |
    | `~s` | number of full seconds since “the epoch” in UTC |
    | `~S` | second, zero padded ‘`00`’ to ‘`60`’   (usual limit is 59, 60 is a leap second) |
    | `~t` | horizontal tab character |
    | `~T` | time, 24 hour clock, ‘`~H:~M:~S`’ |
    | `~U` | week of year, Sunday first day of week, ‘`00`’ to ‘`52`’ |
    | `~V` | ISO 8601 week number of the year, Monday first day of week, ‘`01`’ to ‘`53`’ |
    | `~w` | day of week, 0 for Sunday, ‘`0`’ to ‘`6`’ |
    | `~W` | week of year, Monday first day of week, ‘`00`’ to ‘`52`’ |
    | `~y` | year, two digits, ‘`00`’ to ‘`99`’ |
    | `~Y` | year, full, eg. ‘`2003`’ |
    | `~z` | time zone, RFC-822 style |
    | `~Z` | time zone symbol (not currently implemented) |
    | `~1` | ISO-8601 date, ‘`~Y-~m-~d`’ |
    | `~2` | ISO-8601 time+zone, ‘`~H:~M:~S~z`’ |
    | `~3` | ISO-8601 time, ‘`~H:~M:~S`’ |
    | `~4` | ISO-8601 date/time+zone, ‘`~Y-~m-~dT~H:~M:~S~z`’ |
    | `~5` | ISO-8601 date/time, ‘`~Y-~m-~dT~H:~M:~S`’ |

Conversions ‘`~D`’, ‘`~x`’ and ‘`~X`’ are not currently
described here, since the specification and reference implementation
differ.

Conversion is locale-dependent on systems that support it
(see [Accessing Locale Information](#Accessing-Locale-Information)). See [`setlocale`](#Locales), for information on how to change the current
locale.

---

Previous: [SRFI-19 Date to string](#SRFI_002d19-Date-to-string), Up: [SRFI-19 - Time/Date Library](#SRFI_002d19)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.16.6 SRFI-19 String to date [¶](#SRFI_002d19-String-to-date-1)

Function: **string->date** input template [¶](#index-string_002d_003edate)
:   Convert an input string to a date under the control of a
    template string. Return a newly created date object.

    Literal characters in template must match characters in
    input and ‘`~`’ escapes must match the input forms described
    in the table below. “Skip to” means characters up to one of the
    given type are ignored, or “no skip” for no skipping. “Read” is
    what’s then read, and “Set” is the field affected in the date
    object.

    For example ‘`~Y`’ skips input characters until a digit is reached,
    at which point it expects a year and stores that to the year field of
    the date.

    |  |  |  |  |
    | --- | --- | --- | --- |
    |  | Skip to | Read | Set |
    | `~~` | no skip | literal ~ | nothing |
    | `~a` | `char-alphabetic?` | locale abbreviated weekday name | nothing |
    | `~A` | `char-alphabetic?` | locale full weekday name | nothing |
    | `~b` | `char-alphabetic?` | locale abbreviated month name | `date-month` |
    | `~B` | `char-alphabetic?` | locale full month name | `date-month` |
    | `~d` | `char-numeric?` | day of month | `date-day` |
    | `~e` | no skip | day of month, blank padded | `date-day` |
    | `~h` | same as ‘`~b`’ |
    | `~H` | `char-numeric?` | hour | `date-hour` |
    | `~k` | no skip | hour, blank padded | `date-hour` |
    | `~m` | `char-numeric?` | month | `date-month` |
    | `~M` | `char-numeric?` | minute | `date-minute` |
    | `~N` | `char-numeric?` | nanosecond | `date-nanosecond` |
    | `~S` | `char-numeric?` | second | `date-second` |
    | `~y` | no skip | 2-digit year | `date-year` within 50 years |
    | `~Y` | `char-numeric?` | year | `date-year` |
    | `~z` | no skip | time zone | date-zone-offset |

    Notice that the weekday matching forms don’t affect the date object
    returned, instead the weekday will be derived from the day, month and
    year.

    Conversion is locale-dependent on systems that support it
    (see [Accessing Locale Information](#Accessing-Locale-Information)). See [`setlocale`](#Locales), for information on how to change the current
    locale.

---

Next: [SRFI-26 - specializing parameters](#SRFI_002d26), Previous: [SRFI-19 - Time/Date Library](#SRFI_002d19), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.17 SRFI-23 - Error Reporting [¶](#SRFI_002d23-_002d-Error-Reporting)

The SRFI-23 `error` procedure is always available.

---

Next: [SRFI-27 - Sources of Random Bits](#SRFI_002d27), Previous: [SRFI-23 - Error Reporting](#SRFI_002d23), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.18 SRFI-26 - specializing parameters [¶](#SRFI_002d26-_002d-specializing-parameters)

This SRFI provides a syntax for conveniently specializing selected
parameters of a function. It can be used with,

```
(use-modules (srfi srfi-26))
```

library syntax: **cut** slot1 slot2 … [¶](#index-cut)

library syntax: **cute** slot1 slot2 … [¶](#index-cute)
:   Return a new procedure which will make a call (slot1 slot2
    …) but with selected parameters specialized to given expressions.

    An example will illustrate the idea. The following is a
    specialization of `write`, sending output to
    `my-output-port`,

    ```
    (cut write <> my-output-port)
    ⇒
    (lambda (obj) (write obj my-output-port))
    ```

    The special symbol `<>` indicates a slot to be filled by an
    argument to the new procedure. `my-output-port` on the other
    hand is an expression to be evaluated and passed, ie. it specializes
    the behavior of `write`.

    `<>`
    :   A slot to be filled by an argument from the created procedure.
        Arguments are assigned to `<>` slots in the order they appear in
        the `cut` form, there’s no way to re-arrange arguments.

        The first argument to `cut` is usually a procedure (or expression
        giving a procedure), but `<>` is allowed there too. For example,

        ```
        (cut <> 1 2 3)
        ⇒
        (lambda (proc) (proc 1 2 3))
        ```

    `<...>`
    :   A slot to be filled by all remaining arguments from the new procedure.
        This can only occur at the end of a `cut` form.

        For example, a procedure taking a variable number of arguments like
        `max` but in addition enforcing a lower bound,

        ```
        (define my-lower-bound 123)

        (cut max my-lower-bound <...>)
        ⇒
        (lambda arglist (apply max my-lower-bound arglist))
        ```

    For `cut` the specializing expressions are evaluated each time
    the new procedure is called. For `cute` they’re evaluated just
    once, when the new procedure is created. The name `cute` stands
    for “`cut` with evaluated arguments”. In all cases the
    evaluations take place in an unspecified order.

    The following illustrates the difference between `cut` and
    `cute`,

    ```
    (cut format <> "the time is ~s" (current-time))
    ⇒
    (lambda (port) (format port "the time is ~s" (current-time)))

    (cute format <> "the time is ~s" (current-time))
    ⇒
    (let ((val (current-time)))
      (lambda (port) (format port "the time is ~s" val))
    ```

    (There’s no provision for a mixture of `cut` and `cute`
    where some expressions would be evaluated every time but others
    evaluated only once.)

    `cut` is really just a shorthand for the sort of `lambda`
    forms shown in the above examples. But notice `cut` avoids the
    need to name unspecialized parameters, and is more compact. Use in
    functional programming style or just with `map`, `for-each`
    or similar is typical.

    ```
    (map (cut * 2 <>) '(1 2 3 4))

    (for-each (cut write <> my-port) my-list)
    ```

---

Next: [SRFI-28 - Basic Format Strings](#SRFI_002d28), Previous: [SRFI-26 - specializing parameters](#SRFI_002d26), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.19 SRFI-27 - Sources of Random Bits [¶](#SRFI_002d27-_002d-Sources-of-Random-Bits)

This subsection is based on the
[specification of
SRFI-27](http://srfi.schemers.org/srfi-27/srfi-27.html) written by Sebastian Egner.

This SRFI provides access to a (pseudo) random number generator; for
Guile’s built-in random number facilities, which SRFI-27 is implemented
upon, See [Random Number Generation](#Random). With SRFI-27, random numbers are obtained from a
*random source*, which encapsulates a random number generation
algorithm and its state.

* [The Default Random Source](#SRFI_002d27-Default-Random-Source)
* [Random Sources](#SRFI_002d27-Random-Sources)
* [Obtaining random number generator procedures](#SRFI_002d27-Random-Number-Generators)

---

Next: [Random Sources](#SRFI_002d27-Random-Sources), Up: [SRFI-27 - Sources of Random Bits](#SRFI_002d27)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.19.1 The Default Random Source [¶](#The-Default-Random-Source)

Function: **random-integer** n [¶](#index-random_002dinteger)
:   Return a random number between zero (inclusive) and n (exclusive),
    using the default random source. The numbers returned have a uniform
    distribution.

Function: **random-real** [¶](#index-random_002dreal)
:   Return a random number in (0,1), using the default random source. The
    numbers returned have a uniform distribution.

Function: **default-random-source** [¶](#index-default_002drandom_002dsource)
:   A random source from which `random-integer` and `random-real`
    have been derived using `random-source-make-integers` and
    `random-source-make-reals` (see [Obtaining random number generator procedures](#SRFI_002d27-Random-Number-Generators)
    for those procedures). Note that an assignment to
    `default-random-source` does not change `random-integer` or
    `random-real`; it is also strongly recommended not to assign a new
    value.

---

Next: [Obtaining random number generator procedures](#SRFI_002d27-Random-Number-Generators), Previous: [The Default Random Source](#SRFI_002d27-Default-Random-Source), Up: [SRFI-27 - Sources of Random Bits](#SRFI_002d27)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.19.2 Random Sources [¶](#Random-Sources)

Function: **make-random-source** [¶](#index-make_002drandom_002dsource)
:   Create a new random source. The stream of random numbers obtained from
    each random source created by this procedure will be identical, unless
    its state is changed by one of the procedures below.

Function: **random-source?** object [¶](#index-random_002dsource_003f)
:   Tests whether object is a random source. Random sources are a
    disjoint type.

Function: **random-source-randomize!** source [¶](#index-random_002dsource_002drandomize_0021)
:   Attempt to set the state of the random source to a truly random value.
    The current implementation uses a seed based on the current system time.

Function: **random-source-pseudo-randomize!** source i j [¶](#index-random_002dsource_002dpseudo_002drandomize_0021)
:   Changes the state of the random source s into the initial state of the
    (i, j)-th independent random source, where i and
    j are non-negative integers. This procedure provides a mechanism
    to obtain a large number of independent random sources (usually all
    derived from the same backbone generator), indexed by two integers. In
    contrast to `random-source-randomize!`, this procedure is entirely
    deterministic.

The state associated with a random state can be obtained an reinstated
with the following procedures:

Function: **random-source-state-ref** source [¶](#index-random_002dsource_002dstate_002dref)

Function: **random-source-state-set!** source state [¶](#index-random_002dsource_002dstate_002dset_0021)
:   Get and set the state of a random source. No assumptions should be made
    about the nature of the state object, besides it having an external
    representation (i.e. it can be passed to `write` and subsequently
    `read` back).

---

Previous: [Random Sources](#SRFI_002d27-Random-Sources), Up: [SRFI-27 - Sources of Random Bits](#SRFI_002d27)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.19.3 Obtaining random number generator procedures [¶](#Obtaining-random-number-generator-procedures)

Function: **random-source-make-integers** source [¶](#index-random_002dsource_002dmake_002dintegers)
:   Obtains a procedure to generate random integers using the random source
    source. The returned procedure takes a single argument n,
    which must be a positive integer, and returns the next uniformly
    distributed random integer from the interval {0, ..., n-1} by
    advancing the state of source.

    If an application obtains and uses several generators for the same
    random source source, a call to any of these generators advances
    the state of source. Hence, the generators do not produce the
    same sequence of random integers each but rather share a state. This
    also holds for all other types of generators derived from a fixed random
    sources.

    While the SRFI text specifies that “Implementations that support
    concurrency make sure that the state of a generator is properly
    advanced”, this is currently not the case in Guile’s implementation of
    SRFI-27, as it would cause a severe performance penalty. So in
    multi-threaded programs, you either must perform locking on random
    sources shared between threads yourself, or use different random sources
    for multiple threads.

Function: **random-source-make-reals** source [¶](#index-random_002dsource_002dmake_002dreals)

Function: **random-source-make-reals** source unit [¶](#index-random_002dsource_002dmake_002dreals-1)
:   Obtains a procedure to generate random real numbers *0 < x < 1*
    using the random source source. The procedure rand is called
    without arguments.

    The optional parameter unit determines the type of numbers being
    produced by the returned procedure and the quantization of the output.
    unit must be a number such that *0 < unit < 1*. The
    numbers created by the returned procedure are of the same numerical type
    as unit and the potential output values are spaced by at most
    unit. One can imagine rand to create numbers as x \*
    unit where x is a random integer in {1, ...,
    floor(1/unit)-1}. Note, however, that this need not be the way the
    values are actually created and that the actual resolution of rand can
    be much higher than unit. In case unit is absent it defaults to a
    reasonably small value (related to the width of the mantissa of an
    efficient number format).

---

Next: [SRFI-30 - Nested Multi-line Comments](#SRFI_002d30), Previous: [SRFI-27 - Sources of Random Bits](#SRFI_002d27), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.20 SRFI-28 - Basic Format Strings [¶](#SRFI_002d28-_002d-Basic-Format-Strings)

SRFI-28 provides a basic `format` procedure that provides only
the `~a`, `~s`, `~%`, and `~~` format specifiers.
You can import this procedure by using:

```
(use-modules (srfi srfi-28))
```

Scheme Procedure: **format** message arg … [¶](#index-format)
:   Returns a formatted message, using message as the format string,
    which can contain the following format specifiers:

    `~a`
    :   Insert the textual representation of the next arg, as if printed
        by `display`.

    `~s`
    :   Insert the textual representation of the next arg, as if printed
        by `write`.

    `~%`
    :   Insert a newline.

    `~~`
    :   Insert a tilde.

    This procedure is the same as calling `simple-format`
    (see [Simple Textual Output](#Simple-Output)) with `#f` as the destination.

---

Next: [SRFI-31 - A special form ‘rec’ for recursive evaluation](#SRFI_002d31), Previous: [SRFI-28 - Basic Format Strings](#SRFI_002d28), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.21 SRFI-30 - Nested Multi-line Comments [¶](#SRFI_002d30-_002d-Nested-Multi_002dline-Comments)

Starting from version 2.0, Guile’s `read` supports SRFI-30/R6RS
nested multi-line comments by default, [Block Comments](#Block-Comments).

---

Next: [SRFI-34 - Exception handling for programs](#SRFI_002d34), Previous: [SRFI-30 - Nested Multi-line Comments](#SRFI_002d30), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.22 SRFI-31 - A special form ‘rec’ for recursive evaluation [¶](#SRFI_002d31-_002d-A-special-form-_0060rec_0027-for-recursive-evaluation)

SRFI-31 defines a special form that can be used to create
self-referential expressions more conveniently. The syntax is as
follows:

```
<rec expression> --> (rec <variable> <expression>)
<rec expression> --> (rec (<variable>+) <body>)
```

The first syntax can be used to create self-referential expressions,
for example:

```
  guile> (define tmp (rec ones (cons 1 (delay ones))))
```

The second syntax can be used to create anonymous recursive functions:

```
  guile> (define tmp (rec (display-n item n)
                       (when (positive? n)
                         (display item) (newline)
                         (display-n item (- n 1)))))
  guile> (tmp 42 3)
  42
  42
  42
  guile>
```

---

Next: [SRFI-35 - Conditions](#SRFI_002d35), Previous: [SRFI-31 - A special form ‘rec’ for recursive evaluation](#SRFI_002d31), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.23 SRFI-34 - Exception handling for programs [¶](#SRFI_002d34-_002d-Exception-handling-for-programs)

Guile provides an implementation of
[SRFI-34’s exception
handling mechanisms](http://srfi.schemers.org/srfi-34/srfi-34.html) as an alternative to its own built-in mechanisms
(see [Exceptions](#Exceptions)). It can be made available as follows:

```
(use-modules (srfi srfi-34))
```

See [Raising and Handling Exceptions](#Raising-and-Handling-Exceptions), for more on
`with-exception-handler` and `raise` (known as
`raise-exception` in core Guile).

SRFI-34’s `guard` form is syntactic sugar over
`with-exception-handler`:

Syntax: **guard** (var clause …) body … [¶](#index-guard)
:   Evaluate body with an exception handler that binds the raised
    object to var and within the scope of that binding evaluates
    clause… as if they were the clauses of a cond expression.
    That implicit cond expression is evaluated with the continuation and
    dynamic environment of the guard expression.

    If every clause’s test evaluates to false and there is no
    `else` clause, then `raise` is re-invoked on the raised object
    within the dynamic environment of the original call to `raise`
    except that the current exception handler is that of the `guard`
    expression.

---

Next: [SRFI-37 - args-fold](#SRFI_002d37), Previous: [SRFI-34 - Exception handling for programs](#SRFI_002d34), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.24 SRFI-35 - Conditions [¶](#SRFI_002d35-_002d-Conditions)

[SRFI-35](http://srfi.schemers.org/srfi-35/srfi-35.html) defines
*conditions*, a data structure akin to records designed to convey
information about exceptional conditions between parts of a program. It
is normally used in conjunction with SRFI-34’s `raise`:

```
(raise (condition (&message
                    (message "An error occurred"))))
```

Users can define *condition types* containing arbitrary information.
Condition types may inherit from one another. This allows the part of
the program that handles (or “catches”) conditions to get accurate
information about the exceptional condition that arose.

SRFI-35 conditions are made available using:

```
(use-modules (srfi srfi-35))
```

The procedures available to manipulate condition types are the
following:

Scheme Procedure: **make-condition-type** id parent field-names [¶](#index-make_002dcondition_002dtype)
:   Return a new condition type named id, inheriting from
    parent, and with the fields whose names are listed in
    field-names. field-names must be a list of symbols and must
    not contain names already used by parent or one of its supertypes.

Scheme Procedure: **condition-type?** obj [¶](#index-condition_002dtype_003f)
:   Return true if obj is a condition type.

Conditions can be created and accessed with the following procedures:

Scheme Procedure: **make-condition** type . field+value [¶](#index-make_002dcondition)
:   Return a new condition of type type with fields initialized as
    specified by field+value, a sequence of field names (symbols) and
    values as in the following example:

    ```
    (let ((&ct (make-condition-type 'foo &condition '(a b c))))
      (make-condition &ct 'a 1 'b 2 'c 3))
    ```

    Note that all fields of type and its supertypes must be specified.

Scheme Procedure: **make-compound-condition** condition1 condition2 … [¶](#index-make_002dcompound_002dcondition)
:   Return a new compound condition composed of condition1
    condition2 .... The returned condition has the type of
    each condition of condition1 condition2 … (per
    `condition-has-type?`).

Scheme Procedure: **condition-has-type?** c type [¶](#index-condition_002dhas_002dtype_003f)
:   Return true if condition c has type type.

Scheme Procedure: **condition-ref** c field-name [¶](#index-condition_002dref)
:   Return the value of the field named field-name from condition c.

    If c is a compound condition and several underlying condition
    types contain a field named field-name, then the value of the
    first such field is returned, using the order in which conditions were
    passed to `make-compound-condition`.

Scheme Procedure: **extract-condition** c type [¶](#index-extract_002dcondition)
:   Return a condition of condition type type with the field values
    specified by c.

    If c is a compound condition, extract the field values from the
    subcondition belonging to type that appeared first in the call to
    `make-compound-condition` that created the condition.

Convenience macros are also available to create condition types and
conditions.

library syntax: **define-condition-type** type supertype predicate field-spec... [¶](#index-define_002dcondition_002dtype)
:   Define a new condition type named type that inherits from
    supertype. In addition, bind predicate to a type predicate
    that returns true when passed a condition of type type or any of
    its subtypes. field-spec must have the form `(field
    accessor)` where field is the name of field of type and
    accessor is the name of a procedure to access field field in
    conditions of type type.

    The example below defines condition type `&foo`, inheriting from
    `&condition` with fields `a`, `b` and `c`:

    ```
    (define-condition-type &foo &condition
      foo-condition?
      (a  foo-a)
      (b  foo-b)
      (c  foo-c))
    ```

library syntax: **condition** type-field-binding1 type-field-binding2 … [¶](#index-condition)
:   Return a new condition or compound condition, initialized according to
    type-field-binding1 type-field-binding2 .... Each
    type-field-binding must have the form `(type
    field-specs...)`, where type is the name of a variable bound to a
    condition type; each field-spec must have the form
    `(field-name value)` where field-name is a symbol denoting
    the field being initialized to value. As for
    `make-condition`, all fields must be specified.

    The following example returns a simple condition:

    ```
    (condition (&message (message "An error occurred")))
    ```

    The one below returns a compound condition:

    ```
    (condition (&message (message "An error occurred"))
               (&serious))
    ```

Finally, SRFI-35 defines a several standard condition types.

Variable: **&condition** [¶](#index-_0026condition-1)
:   This condition type is the root of all condition types. It has no
    fields.

Variable: **&message** [¶](#index-_0026message-2)
:   A condition type that carries a message describing the nature of the
    condition to humans.

Scheme Procedure: **message-condition?** c [¶](#index-message_002dcondition_003f)
:   Return true if c is of type `&message` or one of its
    subtypes.

Scheme Procedure: **condition-message** c [¶](#index-condition_002dmessage)
:   Return the message associated with message condition c.

Variable: **&serious** [¶](#index-_0026serious-1)
:   This type describes conditions serious enough that they cannot safely be
    ignored. It has no fields.

Scheme Procedure: **serious-condition?** c [¶](#index-serious_002dcondition_003f)
:   Return true if c is of type `&serious` or one of its
    subtypes.

Variable: **&error** [¶](#index-_0026error-2)
:   This condition describes errors, typically caused by something that has
    gone wrong in the interaction of the program with the external world or
    the user.

Scheme Procedure: **error?** c [¶](#index-error_003f-1)
:   Return true if c is of type `&error` or one of its subtypes.

As an implementation note, condition objects in Guile are the same as
“exception objects”. See [Exception Objects](#Exception-Objects). The
`&condition`, `&serious`, and `&error` condition types
are known in core Guile as `&exception`, `&error`, and
`&external-error`, respectively.

---

Next: [SRFI-38 - External Representation for Data With Shared Structure](#SRFI_002d38), Previous: [SRFI-35 - Conditions](#SRFI_002d35), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.25 SRFI-37 - args-fold [¶](#SRFI_002d37-_002d-args_002dfold)

This is a processor for GNU `getopt_long`-style program
arguments. It provides an alternative, less declarative interface
than `getopt-long` in `(ice-9 getopt-long)`
(see [The (ice-9 getopt-long) Module](#getopt_002dlong)). Unlike
`getopt-long`, it supports repeated options and any number of
short and long names per option. Access it with:

```
(use-modules (srfi srfi-37))
```

SRFI-37 principally provides an `option` type and the
`args-fold` function. To use the library, create a set of
options with `option` and use it as a specification for invoking
`args-fold`.

Here is an example of a simple argument processor for the typical
‘`--version`’ and ‘`--help`’ options, which returns a backwards
list of files given on the command line:

```
(args-fold (cdr (program-arguments))
           (let ((display-and-exit-proc
                  (lambda (msg)
                    (lambda (opt name arg loads)
                      (display msg) (quit)))))
             (list (option '(#\v "version") #f #f
                           (display-and-exit-proc "Foo version 42.0\n"))
                   (option '(#\h "help") #f #f
                           (display-and-exit-proc
                            "Usage: foo scheme-file ..."))))
           (lambda (opt name arg loads)
             (error "Unrecognized option `~A'" name))
           (lambda (op loads) (cons op loads))
           '())
```

Scheme Procedure: **option** names required-arg? optional-arg? processor [¶](#index-option-1)
:   Return an object that specifies a single kind of program option.

    names is a list of command-line option names, and should consist of
    characters for traditional `getopt` short options and strings for
    `getopt_long`-style long options.

    required-arg? and optional-arg? are mutually exclusive;
    one or both must be `#f`. If required-arg?, the option
    must be followed by an argument on the command line, such as
    ‘`--opt=value`’ for long options, or an error will be signaled.
    If optional-arg?, an argument will be taken if available.

    processor is a procedure that takes at least 3 arguments, called
    when `args-fold` encounters the option: the containing option
    object, the name used on the command line, and the argument given for
    the option (or `#f` if none). The rest of the arguments are
    `args-fold` “seeds”, and the processor should return
    seeds as well.

Scheme Procedure: **option-names** opt [¶](#index-option_002dnames)

Scheme Procedure: **option-required-arg?** opt [¶](#index-option_002drequired_002darg_003f)

Scheme Procedure: **option-optional-arg?** opt [¶](#index-option_002doptional_002darg_003f)

Scheme Procedure: **option-processor** opt [¶](#index-option_002dprocessor)
:   Return the specified field of opt, an option object, as
    described above for `option`.

Scheme Procedure: **args-fold** args options unrecognized-option-proc operand-proc seed … [¶](#index-args_002dfold)
:   Process args, a list of program arguments such as that returned by
    `(cdr (program-arguments))`, in order against options, a list
    of option objects as described above. All functions called take the
    “seeds”, or the last multiple-values as multiple arguments, starting
    with seed …, and must return the new seeds. Return the
    final seeds.

    Call `unrecognized-option-proc`, which is like an option object’s
    processor, for any options not found in options.

    Call `operand-proc` with any items on the command line that are
    not named options. This includes arguments after ‘`--`’. It is
    called with the argument in question, as well as the seeds.

---

Next: [SRFI-39 - Parameters](#SRFI_002d39), Previous: [SRFI-37 - args-fold](#SRFI_002d37), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.26 SRFI-38 - External Representation for Data With Shared Structure [¶](#SRFI_002d38-_002d-External-Representation-for-Data-With-Shared-Structure)

This subsection is based on
[the specification
of SRFI-38](http://srfi.schemers.org/srfi-38/srfi-38.html) written by Ray Dillinger.

This SRFI creates an alternative external representation for data
written and read using `write-with-shared-structure` and
`read-with-shared-structure`. It is identical to the grammar for
external representation for data written and read with `write` and
`read` given in section 7 of R5RS, except that the single
production

```
<datum> --> <simple datum> | <compound datum>
```

is replaced by the following five productions:

```
<datum> --> <defining datum> | <nondefining datum> | <defined datum>
<defining datum> -->  #<indexnum>=<nondefining datum>
<defined datum> --> #<indexnum>#
<nondefining datum> --> <simple datum> | <compound datum>
<indexnum> --> <digit 10>+
```

Scheme procedure: **write-with-shared-structure** obj [¶](#index-write_002dwith_002dshared_002dstructure)

Scheme procedure: **write-with-shared-structure** obj port [¶](#index-write_002dwith_002dshared_002dstructure-1)

Scheme procedure: **write-with-shared-structure** obj port optarg [¶](#index-write_002dwith_002dshared_002dstructure-2)
:   Writes an external representation of obj to the given port.
    Strings that appear in the written representation are enclosed in
    doublequotes, and within those strings backslash and doublequote
    characters are escaped by backslashes. Character objects are written
    using the `#\` notation.
