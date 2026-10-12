### 6.22 Threads, Mutexes, Asyncs and Dynamic Roots [¶](#Threads_002c-Mutexes_002c-Asyncs-and-Dynamic-Roots)

* [Threads](#Threads)
* [Thread-Local Variables](#Thread-Local-Variables)
* [Asynchronous Interrupts](#Asyncs)
* [Atomics](#Atomics)
* [Mutexes and Condition Variables](#Mutexes-and-Condition-Variables)
* [Blocking in Guile Mode](#Blocking)
* [Futures](#Futures)
* [Parallel forms](#Parallel-Forms)

---

Next: [Thread-Local Variables](#Thread-Local-Variables), Up: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.22.1 Threads [¶](#Threads-1)

Guile supports POSIX threads, unless it was configured with
`--without-threads` or the host lacks POSIX thread support. When
thread support is available, the `threads` feature is provided
(see [`provided?`](#Feature-Manipulation)).

The procedures below manipulate Guile threads, which are wrappers around
the system’s POSIX threads. For application-level parallelism, using
higher-level constructs, such as futures, is recommended
(see [Futures](#Futures)).

To use these facilities, load the `(ice-9 threads)` module.

```
(use-modules (ice-9 threads))
```

Scheme Procedure: **all-threads** [¶](#index-all_002dthreads)

C Function: **scm\_all\_threads** () [¶](#index-scm_005fall_005fthreads)
:   Return a list of all threads.

Scheme Procedure: **current-thread** [¶](#index-current_002dthread)

C Function: **scm\_current\_thread** () [¶](#index-scm_005fcurrent_005fthread)
:   Return the thread that called this function.

Scheme Procedure: **call-with-new-thread** thunk [handler] [¶](#index-call_002dwith_002dnew_002dthread)
:   Call `thunk` in a new thread and with a new dynamic state,
    returning the new thread. The procedure thunk is called via
    `with-continuation-barrier`.

    When handler is specified, then thunk is called from
    within a `catch` with tag `#t` that has handler as its
    handler. This catch is established inside the continuation barrier.

    Once thunk or handler returns, the return value is made
    the *exit value* of the thread and the thread is terminated.

C Function: `SCM` **scm\_spawn\_thread** `(scm_t_catch_body body, void *body_data, scm_t_catch_handler handler, void *handler_data)` [¶](#index-scm_005fspawn_005fthread)
:   Call body in a new thread, passing it body\_data, returning
    the new thread. The function body is called via
    `scm_c_with_continuation_barrier`.

    When handler is non-`NULL`, body is called via
    `scm_internal_catch` with tag `SCM_BOOL_T` that has
    handler and handler\_data as the handler and its data. This
    catch is established inside the continuation barrier.

    Once body or handler returns, the return value is made the
    *exit value* of the thread and the thread is terminated.

Scheme Procedure: **thread?** obj [¶](#index-thread_003f)

C Function: **scm\_thread\_p** (obj) [¶](#index-scm_005fthread_005fp)
:   Return `#t` ff obj is a thread; otherwise, return
    `#f`.

Scheme Procedure: **join-thread** thread [timeout [timeoutval]] [¶](#index-join_002dthread)

C Function: **scm\_join\_thread** (thread) [¶](#index-scm_005fjoin_005fthread)

C Function: **scm\_join\_thread\_timed** (thread, timeout, timeoutval) [¶](#index-scm_005fjoin_005fthread_005ftimed)
:   Wait for thread to terminate and return its exit value. Only
    threads that were created with `call-with-new-thread` or
    `scm_spawn_thread` can be joinable; attempting to join a foreign
    thread will raise an error.

    When timeout is given, it specifies a point in time where the
    waiting should be aborted. It can be either an integer as returned by
    `current-time` or a pair as returned by `gettimeofday`. When
    the waiting is aborted, timeoutval is returned (if it is
    specified; `#f` is returned otherwise).

Scheme Procedure: **thread-exited?** thread [¶](#index-thread_002dexited_003f)

C Function: **scm\_thread\_exited\_p** (thread) [¶](#index-scm_005fthread_005fexited_005fp)
:   Return `#t` if thread has exited, or `#f` otherwise.

Scheme Procedure: **yield** [¶](#index-yield)

C Function: **scm\_yield** (thread) [¶](#index-scm_005fyield)
:   If one or more threads are waiting to execute, calling yield forces an
    immediate context switch to one of them. Otherwise, yield has no effect.

Scheme Procedure: **cancel-thread** thread . values [¶](#index-cancel_002dthread)

C Function: **scm\_cancel\_thread** (thread) [¶](#index-scm_005fcancel_005fthread)
:   Asynchronously interrupt thread and ask it to terminate.
    `dynamic-wind` post thunks will run, but throw handlers will not.
    If thread has already terminated or been signaled to terminate,
    this function is a no-op. Calling `join-thread` on the thread will
    return the given values, if the cancel succeeded.

    Under the hood, thread cancellation uses `system-async-mark` and
    `abort-to-prompt`. See [Asynchronous Interrupts](#Asyncs) for more on asynchronous
    interrupts.

macro: **make-thread** proc arg … [¶](#index-make_002dthread)
:   Apply proc to arg … in a new thread formed by
    `call-with-new-thread` using a default error handler that displays
    the error to the current error port. The arg …
    expressions are evaluated in the new thread.

macro: **begin-thread** expr1 expr2 … [¶](#index-begin_002dthread)
:   Evaluate forms expr1 expr2 … in a new thread formed by
    `call-with-new-thread` using a default error handler that displays
    the error to the current error port.

One often wants to limit the number of threads running to be
proportional to the number of available processors. These interfaces
are therefore exported by (ice-9 threads) as well.

Scheme Procedure: **total-processor-count** [¶](#index-total_002dprocessor_002dcount)

C Function: **scm\_total\_processor\_count** () [¶](#index-scm_005ftotal_005fprocessor_005fcount)
:   Return the total number of processors of the machine, which
    is guaranteed to be at least 1. A “processor” here is a
    thread execution unit, which can be either:

    * an execution core in a (possibly multi-core) chip, in a
      (possibly multi- chip) module, in a single computer, or
    * a thread execution unit inside a core in the case of
      *hyper-threaded* CPUs.

    Which of the two definitions is used, is unspecified.

Scheme Procedure: **current-processor-count** [¶](#index-current_002dprocessor_002dcount)

C Function: **scm\_current\_processor\_count** () [¶](#index-scm_005fcurrent_005fprocessor_005fcount)
:   Like `total-processor-count`, but return the number of
    processors available to the current process. See
    `setaffinity` and `getaffinity` for more
    information.

---

Next: [Asynchronous Interrupts](#Asyncs), Previous: [Threads](#Threads), Up: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.22.2 Thread-Local Variables [¶](#Thread_002dLocal-Variables)

Sometimes you want to establish a variable binding that is only valid
for a given thread: a “thread-local variable”.

You would think that fluids or parameters would be Guile’s answer for
thread-local variables, since establishing a new fluid binding doesn’t
affect bindings in other threads. See [Fluids and Dynamic States](#Fluids-and-Dynamic-States), or
See [Parameters](#Parameters). However, new threads inherit the fluid bindings that
were in place in their creator threads. In this way, a binding
established using a fluid (or a parameter) in a thread can escape to
other threads, which might not be what you want. Or, it might escape
via explicit reification via `current-dynamic-state`.

Of course, this dynamic scoping might be exactly what you want; that’s
why fluids and parameters work this way, and is what you want for
many common parameters such as the current input and output ports, the
current locale conversion parameters, and the like. Perhaps this is the
case for most parameters, even. If your use case for thread-local
bindings comes from a desire to isolate a binding from its setting in
unrelated threads, then fluids and parameters apply nicely.

On the other hand, if your use case is to prevent concurrent access to a
value from multiple threads, then using vanilla fluids or parameters is
not appropriate. For this purpose, Guile has *thread-local fluids*.
A fluid created with `make-thread-local-fluid` won’t be captured by
`current-dynamic-state` and won’t be propagated to new threads.

Scheme Procedure: **make-thread-local-fluid** [dflt] [¶](#index-make_002dthread_002dlocal_002dfluid)

C Function: **scm\_make\_thread\_local\_fluid** (dflt) [¶](#index-scm_005fmake_005fthread_005flocal_005ffluid)
:   Return a newly created fluid, whose initial value is dflt, or
    `#f` if dflt is not given. Unlike fluids made with
    `make-fluid`, thread local fluids are not captured by
    `make-dynamic-state`. Similarly, a newly spawned child thread does
    not inherit thread-local fluid values from the parent thread.

Scheme Procedure: **fluid-thread-local?** fluid [¶](#index-fluid_002dthread_002dlocal_003f)

C Function: **scm\_fluid\_thread\_local\_p** (fluid) [¶](#index-scm_005ffluid_005fthread_005flocal_005fp)
:   Return `#t` if the fluid fluid is thread-local, or
    `#f` otherwise.

For example:

```
(define %thread-local (make-thread-local-fluid))

(with-fluids ((%thread-local (compute-data)))
  ... (fluid-ref %thread-local) ...)
```

You can also make a thread-local parameter out of a thread-local fluid
using the normal `fluid->parameter`:

```
(define param (fluid->parameter (make-thread-local-fluid)))

(parameterize ((param (compute-data)))
  ... (param) ...)
```

---

Next: [Atomics](#Atomics), Previous: [Thread-Local Variables](#Thread-Local-Variables), Up: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.22.3 Asynchronous Interrupts [¶](#Asynchronous-Interrupts)

Every Guile thread can be interrupted. Threads running Guile code will
periodically check if there are pending interrupts and run them if
necessary. To interrupt a thread, call `system-async-mark` on that
thread.

Scheme Procedure: **system-async-mark** proc [thread] [¶](#index-system_002dasync_002dmark)

C Function: **scm\_system\_async\_mark** (proc) [¶](#index-scm_005fsystem_005fasync_005fmark)

C Function: **scm\_system\_async\_mark\_for\_thread** (proc, thread) [¶](#index-scm_005fsystem_005fasync_005fmark_005ffor_005fthread)
:   Enqueue proc (a procedure with zero arguments) for future
    execution in thread. When proc has already been enqueued
    for thread but has not been executed yet, this call has no effect.
    When thread is omitted, the thread that called
    `system-async-mark` is used.

Note that `scm_system_async_mark_for_thread` is not
“async-signal-safe” and so cannot be called from a C signal handler.
(Indeed in general, `libguile` functions are not safe to call from
C signal handlers.)

Though an interrupt procedure can have any side effect permitted to
Guile code, asynchronous interrupts are generally used either for
profiling or for prematurely canceling a computation. The former case
is mostly transparent to the program being run, by design, but the
latter case can introduce bugs. Like finalizers (see [Foreign Object Memory Management](#Foreign-Object-Memory-Management)), asynchronous interrupts introduce concurrency in a
program. An asynchronous interrupt can run in the middle of some
mutex-protected operation, for example, and potentially corrupt the
program’s state.

If some bit of Guile code needs to temporarily inhibit interrupts, it
can use `call-with-blocked-asyncs`. This function works by
temporarily increasing the *async blocking level* of the current
thread while a given procedure is running. The blocking level starts
out at zero, and whenever a safe point is reached, a blocking level
greater than zero will prevent the execution of queued asyncs.

Analogously, the procedure `call-with-unblocked-asyncs` will
temporarily decrease the blocking level of the current thread. You
can use it when you want to disable asyncs by default and only allow
them temporarily.

In addition to the C versions of `call-with-blocked-asyncs` and
`call-with-unblocked-asyncs`, C code can use
`scm_dynwind_block_asyncs` and `scm_dynwind_unblock_asyncs`
inside a *dynamic context* (see [Dynamic Wind](#Dynamic-Wind)) to block or
unblock asyncs temporarily.

Scheme Procedure: **call-with-blocked-asyncs** proc [¶](#index-call_002dwith_002dblocked_002dasyncs)

C Function: **scm\_call\_with\_blocked\_asyncs** (proc) [¶](#index-scm_005fcall_005fwith_005fblocked_005fasyncs)
:   Call proc and block the execution of asyncs by one level for the
    current thread while it is running. Return the value returned by
    proc. For the first two variants, call proc with no
    arguments; for the third, call it with data.

C Function: `void *` **scm\_c\_call\_with\_blocked\_asyncs** `(void * (*proc) (void *data), void *data)` [¶](#index-scm_005fc_005fcall_005fwith_005fblocked_005fasyncs)
:   The same but with a C function proc instead of a Scheme thunk.

Scheme Procedure: **call-with-unblocked-asyncs** proc [¶](#index-call_002dwith_002dunblocked_002dasyncs)

C Function: **scm\_call\_with\_unblocked\_asyncs** (proc) [¶](#index-scm_005fcall_005fwith_005funblocked_005fasyncs)
:   Call proc and unblock the execution of asyncs by one level for the
    current thread while it is running. Return the value returned by
    proc. For the first two variants, call proc with no
    arguments; for the third, call it with data.

C Function: `void *` **scm\_c\_call\_with\_unblocked\_asyncs** `(void *(*proc) (void *data), void *data)` [¶](#index-scm_005fc_005fcall_005fwith_005funblocked_005fasyncs)
:   The same but with a C function proc instead of a Scheme thunk.

C Function: `void` **scm\_dynwind\_block\_asyncs** `()` [¶](#index-scm_005fdynwind_005fblock_005fasyncs)
:   During the current dynwind context, increase the blocking of asyncs by
    one level. This function must be used inside a pair of calls to
    `scm_dynwind_begin` and `scm_dynwind_end` (see [Dynamic Wind](#Dynamic-Wind)).

C Function: `void` **scm\_dynwind\_unblock\_asyncs** `()` [¶](#index-scm_005fdynwind_005funblock_005fasyncs)
:   During the current dynwind context, decrease the blocking of asyncs by
    one level. This function must be used inside a pair of calls to
    `scm_dynwind_begin` and `scm_dynwind_end` (see [Dynamic Wind](#Dynamic-Wind)).

Sometimes you want to interrupt a thread that might be waiting for
something to happen, for example on a file descriptor or a condition
variable. In that case you can inform Guile of how to interrupt that
wait using the following procedures:

C Function: `int` **scm\_c\_prepare\_to\_wait\_on\_fd** `(int fd)` [¶](#index-scm_005fc_005fprepare_005fto_005fwait_005fon_005ffd)
:   Inform Guile that the current thread is about to sleep, and that if an
    asynchronous interrupt is signaled on this thread, Guile should wake up
    the thread by writing a zero byte to fd. Returns zero if the
    prepare succeeded, or nonzero if the thread already has a pending async
    and that it should avoid waiting.

C Function: `int` **scm\_c\_prepare\_to\_wait\_on\_cond** `(scm_i_pthread_mutex_t *mutex, scm_i_pthread_cond_t *cond)` [¶](#index-scm_005fc_005fprepare_005fto_005fwait_005fon_005fcond)
:   Inform Guile that the current thread is about to sleep, and that if an
    asynchronous interrupt is signaled on this thread, Guile should wake up
    the thread by acquiring mutex and signaling cond. The
    caller must already hold mutex and only drop it as part of the
    `pthread_cond_wait` call. Returns zero if the prepare succeeded,
    or nonzero if the thread already has a pending async and that it should
    avoid waiting.

C Function: `void` **scm\_c\_wait\_finished** `(void)` [¶](#index-scm_005fc_005fwait_005ffinished)
:   Inform Guile that the current thread has finished waiting, and that
    asynchronous interrupts no longer need any special wakeup action; the
    current thread will periodically poll its internal queue instead.

Guile’s own interface to `sleep`, `wait-condition-variable`,
`select`, and so on all call the above routines as appropriate.

Finally, note that threads can also be interrupted via POSIX signals.
See [Signals](#Signals). As an implementation detail, signal handlers will
effectively call `system-async-mark` in a signal-safe way,
eventually running the signal handler using the same async mechanism.
In this way you can temporarily inhibit signal handlers from running
using the above interfaces.

---

Next: [Mutexes and Condition Variables](#Mutexes-and-Condition-Variables), Previous: [Asynchronous Interrupts](#Asyncs), Up: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.22.4 Atomics [¶](#Atomics-1)

When accessing data in parallel from multiple threads, updates made by
one thread are not generally guaranteed to be visible by another thread.
It could be that your hardware requires special instructions to be
emitted to propagate a change from one CPU core to another. Or, it
could be that your hardware updates values with a sequence of
instructions, and a parallel thread could see a value that is in the
process of being updated but not fully updated.

Atomic references solve this problem. Atomics are a standard, primitive
facility to allow for concurrent access and update of mutable variables
from multiple threads with guaranteed forward-progress and well-defined
intermediate states.

Atomic references serve not only as a hardware memory barrier but also
as a compiler barrier. Normally a compiler might choose to reorder or
elide certain memory accesses due to optimizations like common
subexpression elimination. Atomic accesses however will not be
reordered relative to each other, and normal memory accesses will not be
reordered across atomic accesses.

As an implementation detail, currently all atomic accesses and updates
use the sequential consistency memory model from C11. We may relax this
in the future to the acquire/release semantics, which still issues a
memory barrier so that non-atomic updates are not reordered across
atomic accesses or updates.

To use Guile’s atomic operations, load the `(ice-9 atomic)` module:

```
(use-modules (ice-9 atomic))
```

Scheme Procedure: **make-atomic-box** init [¶](#index-make_002datomic_002dbox)
:   Return an atomic box initialized to value init.

Scheme Procedure: **atomic-box?** obj [¶](#index-atomic_002dbox_003f)
:   Return `#t` if obj is an atomic-box object, else
    return `#f`.

Scheme Procedure: **atomic-box-ref** box [¶](#index-atomic_002dbox_002dref)
:   Fetch the value stored in the atomic box box and return it.

Scheme Procedure: **atomic-box-set!** box val [¶](#index-atomic_002dbox_002dset_0021)
:   Store val into the atomic box box.

Scheme Procedure: **atomic-box-swap!** box val [¶](#index-atomic_002dbox_002dswap_0021)
:   Store val into the atomic box box, and return the value that
    was previously stored in the box.

Scheme Procedure: **atomic-box-compare-and-swap!** box expected desired [¶](#index-atomic_002dbox_002dcompare_002dand_002dswap_0021)
:   If the value of the atomic box box is the same as, expected
    (in the sense of `eq?`), replace the contents of the box with
    desired. Otherwise does not update the box. Returns the previous
    value of the box in either case, so you can know if the swap worked by
    checking if the return value is `eq?` to expected.

---

Next: [Blocking in Guile Mode](#Blocking), Previous: [Atomics](#Atomics), Up: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.22.5 Mutexes and Condition Variables [¶](#Mutexes-and-Condition-Variables-1)

Mutexes are low-level primitives used to coordinate concurrent access to
mutable data. Short for “mutual exclusion”, the name “mutex”
indicates that only one thread at a time can acquire access to data that
is protected by a mutex – threads are excluded from accessing data at
the same time. If one thread has locked a mutex, then another thread
attempting to lock that same mutex will wait until the first thread is
done.

Mutexes can be used to build robust multi-threaded programs that take
advantage of multiple cores. However, they provide very low-level
functionality and are somewhat dangerous; usually you end up wanting to
acquire multiple mutexes at the same time to perform a multi-object
access, but this can easily lead to deadlocks if the program is not
carefully written. For example, if objects A and B are protected by
associated mutexes M and N, respectively, then to access both of them
then you need to acquire both mutexes. But what if one thread acquires
M first and then N, at the same time that another thread acquires N them
M? You can easily end up in a situation where one is waiting for the
other.

There’s no easy way around this problem on the language level. A
function A that uses mutexes does not necessarily compose nicely with a
function B that uses mutexes. For this reason we suggest using atomic
variables when you can (see [Atomics](#Atomics)), as they do not have this problem.

Still, if you as a programmer are responsible for a whole system, then
you can use mutexes as a primitive to provide safe concurrent
abstractions to your users. (For example, given all locks in a system,
if you establish an order such that M is consistently acquired before N,
you can avoid the “deadly-embrace” deadlock described above. The
problem is enumerating all mutexes and establishing this order from a
system perspective.) Guile gives you the low-level facilities to build
such systems.

In Guile there are additional considerations beyond the usual ones in
other programming languages: non-local control flow and asynchronous
interrupts. What happens if you hold a mutex, but somehow you cause an
exception to be thrown? There is no one right answer. You might want
to keep the mutex locked to prevent any other code from ever entering
that critical section again. Or, your critical section might be fine if
you unlock the mutex “on the way out”, via an exception handler or
`dynamic-wind`. See [Exceptions](#Exceptions), and See [Dynamic Wind](#Dynamic-Wind).

But if you arrange to unlock the mutex when leaving a dynamic extent via
`dynamic-wind`, what to do if control re-enters that dynamic extent
via a continuation invocation? Surely re-entering the dynamic extent
without the lock is a bad idea, so there are two options on the table:
either prevent re-entry via `with-continuation-barrier` or similar,
or reacquire the lock in the entry thunk of a `dynamic-wind`.

You might think that because you don’t use continuations, that you don’t
have to think about this, and you might be right. If you control the
whole system, you can reason about continuation use globally. Or, if
you know all code that can be called in a dynamic extent, and none of
that code can call continuations, then you don’t have to worry about
re-entry, and you might not have to worry about early exit either.

However, do consider the possibility of asynchronous interrupts
(see [Asynchronous Interrupts](#Asyncs)). If the user interrupts your code interactively, that
can cause an exception; or your thread might be canceled, which does
the same; or the user could be running your code under some pre-emptive
system that periodically causes lightweight task switching. (Guile does
not currently include such a system, but it’s possible to implement as a
library.) Probably you also want to defer asynchronous interrupt
processing while you hold the mutex, and probably that also means that
you should not hold the mutex for very long.

All of these additional Guile-specific considerations mean that from a
system perspective, you would do well to avoid these hazards if you can
by not requiring mutexes. Instead, work with immutable data that can be
shared between threads without hazards, or use persistent data
structures with atomic updates based on the atomic variable library
(see [Atomics](#Atomics)).

There are three types of mutexes in Guile: “standard”, “recursive”,
and “unowned”.

Calling `make-mutex` with no arguments makes a standard mutex. A
standard mutex can only be locked once. If you try to lock it again
from the thread that locked it to begin with (the "owner" thread), it
throws an error. It can only be unlocked from the thread that locked it
in the first place.

Calling `make-mutex` with the symbol `recursive` as the
argument, or calling `make-recursive-mutex`, will give you a
recursive mutex. A recursive mutex can be locked multiple times by its
owner. It then has to be unlocked the corresponding number of times,
and like standard mutexes can only be unlocked by the owner thread.

Finally, calling `make-mutex` with the symbol
`allow-external-unlock` creates an unowned mutex. An unowned mutex
is like a standard mutex, except that it can be unlocked by any thread.
A corollary of this behavior is that a thread’s attempt to lock a mutex
that it already owns will block instead of signaling an error, as it
could be that some other thread unlocks the mutex, allowing the owner
thread to proceed. This kind of mutex is a bit strange and is here for
use by SRFI-18.

The mutex procedures in Guile can operate on all three kinds of mutexes.

To use these facilities, load the `(ice-9 threads)` module.

```
(use-modules (ice-9 threads))
```

  

Scheme Procedure: **make-mutex** [kind] [¶](#index-make_002dmutex)

C Function: **scm\_make\_mutex** () [¶](#index-scm_005fmake_005fmutex)

C Function: **scm\_make\_mutex\_with\_kind** (SCM kind) [¶](#index-scm_005fmake_005fmutex_005fwith_005fkind)
:   Return a new mutex. It will be a standard non-recursive mutex, unless
    the `recursive` symbol is passed as the optional kind
    argument, in which case it will be recursive. It’s also possible to
    pass `unowned` for semantics tailored to SRFI-18’s use case; see
    above for details.

Scheme Procedure: **mutex?** obj [¶](#index-mutex_003f)

C Function: **scm\_mutex\_p** (obj) [¶](#index-scm_005fmutex_005fp)
:   Return `#t` if obj is a mutex; otherwise, return
    `#f`.

Scheme Procedure: **make-recursive-mutex** [¶](#index-make_002drecursive_002dmutex)

C Function: **scm\_make\_recursive\_mutex** () [¶](#index-scm_005fmake_005frecursive_005fmutex)
:   Create a new recursive mutex. It is initially unlocked. Calling this
    function is equivalent to calling `make-mutex` with the
    `recursive` kind.

Scheme Procedure: **lock-mutex** mutex [timeout] [¶](#index-lock_002dmutex)

C Function: **scm\_lock\_mutex** (mutex) [¶](#index-scm_005flock_005fmutex)

C Function: **scm\_timed\_lock\_mutex** (mutex, timeout) [¶](#index-scm_005ftimed_005flock_005fmutex)
:   Lock mutex and return `#t`. If the mutex is already locked,
    then block and return only when mutex has been acquired.

    When timeout is given, it specifies a point in time where the
    waiting should be aborted. It can be either an integer as returned
    by `current-time` or a pair as returned by `gettimeofday`.
    When the waiting is aborted, `#f` is returned.

    For standard mutexes (`make-mutex`), an error is signaled if the
    thread has itself already locked mutex.

    For a recursive mutex (`make-recursive-mutex`), if the thread has
    itself already locked mutex, then a further `lock-mutex`
    call increments the lock count. An additional `unlock-mutex`
    will be required to finally release.

    When an asynchronous interrupt (see [Asynchronous Interrupts](#Asyncs)) is scheduled for a
    thread blocked in `lock-mutex`, Guile will interrupt the wait, run
    the interrupts, and then resume the wait.

C Function: `void` **scm\_dynwind\_lock\_mutex** `(SCM mutex)` [¶](#index-scm_005fdynwind_005flock_005fmutex)
:   Arrange for mutex to be locked whenever the current dynwind
    context is entered and to be unlocked when it is exited.

Scheme Procedure: **try-mutex** mx [¶](#index-try_002dmutex)

C Function: **scm\_try\_mutex** (mx) [¶](#index-scm_005ftry_005fmutex)
:   Try to lock mutex and return `#t` if successful, or `#f`
    otherwise. This is like calling `lock-mutex` with an expired
    timeout.

Scheme Procedure: **unlock-mutex** mutex [¶](#index-unlock_002dmutex)

C Function: **scm\_unlock\_mutex** (mutex) [¶](#index-scm_005funlock_005fmutex)
:   Unlock mutex. An error is signaled if mutex is not locked.

    “Standard” and “recursive” mutexes can only be unlocked by the
    thread that locked them; Guile detects this situation and signals an
    error. “Unowned” mutexes can be unlocked by any thread.

Scheme Procedure: **mutex-owner** mutex [¶](#index-mutex_002downer)

C Function: **scm\_mutex\_owner** (mutex) [¶](#index-scm_005fmutex_005fowner)
:   Return the current owner of mutex, in the form of a thread or
    `#f` (indicating no owner). Note that a mutex may be unowned but
    still locked.

Scheme Procedure: **mutex-level** mutex [¶](#index-mutex_002dlevel)

C Function: **scm\_mutex\_level** (mutex) [¶](#index-scm_005fmutex_005flevel)
:   Return the current lock level of mutex. If mutex is
    currently unlocked, this value will be 0; otherwise, it will be the
    number of times mutex has been recursively locked by its current
    owner.

Scheme Procedure: **mutex-locked?** mutex [¶](#index-mutex_002dlocked_003f)

C Function: **scm\_mutex\_locked\_p** (mutex) [¶](#index-scm_005fmutex_005flocked_005fp)
:   Return `#t` if mutex is locked, regardless of ownership;
    otherwise, return `#f`.

Scheme Procedure: **make-condition-variable** [¶](#index-make_002dcondition_002dvariable)

C Function: **scm\_make\_condition\_variable** () [¶](#index-scm_005fmake_005fcondition_005fvariable)
:   Return a new condition variable.

Scheme Procedure: **condition-variable?** obj [¶](#index-condition_002dvariable_003f)

C Function: **scm\_condition\_variable\_p** (obj) [¶](#index-scm_005fcondition_005fvariable_005fp)
:   Return `#t` if obj is a condition variable; otherwise,
    return `#f`.

Scheme Procedure: **wait-condition-variable** condvar mutex [time] [¶](#index-wait_002dcondition_002dvariable)

C Function: **scm\_wait\_condition\_variable** (condvar, mutex, time) [¶](#index-scm_005fwait_005fcondition_005fvariable)
:   Wait until condvar has been signaled. While waiting,
    mutex is atomically unlocked (as with `unlock-mutex`) and
    is locked again when this function returns. When time is given,
    it specifies a point in time where the waiting should be aborted. It
    can be either a integer as returned by `current-time` or a pair
    as returned by `gettimeofday`. When the waiting is aborted,
    `#f` is returned. When the condition variable has in fact been
    signaled, `#t` is returned. The mutex is re-locked in any case
    before `wait-condition-variable` returns.

    When an async is activated for a thread that is blocked in a call to
    `wait-condition-variable`, the waiting is interrupted, the mutex is
    locked, and the async is executed. When the async returns, the mutex is
    unlocked again and the waiting is resumed. When the thread block while
    re-acquiring the mutex, execution of asyncs is blocked.

Scheme Procedure: **signal-condition-variable** condvar [¶](#index-signal_002dcondition_002dvariable)

C Function: **scm\_signal\_condition\_variable** (condvar) [¶](#index-scm_005fsignal_005fcondition_005fvariable)
:   Wake up one thread that is waiting for condvar.

Scheme Procedure: **broadcast-condition-variable** condvar [¶](#index-broadcast_002dcondition_002dvariable)

C Function: **scm\_broadcast\_condition\_variable** (condvar) [¶](#index-scm_005fbroadcast_005fcondition_005fvariable)
:   Wake up all threads that are waiting for condvar.

Guile also includes some higher-level abstractions for working with
mutexes.

macro: **with-mutex** mutex body1 body2 … [¶](#index-with_002dmutex)
:   Lock mutex, evaluate the body body1 body2 …,
    then unlock mutex. The return value is that returned by the last
    body form.

    The lock, body and unlock form the branches of a `dynamic-wind`
    (see [Dynamic Wind](#Dynamic-Wind)), so mutex is automatically unlocked if an
    error or new continuation exits the body, and is re-locked if
    the body is re-entered by a captured continuation.

macro: **monitor** body1 body2 … [¶](#index-monitor)
:   Evaluate the body form body1 body2 … with a mutex
    locked so only one thread can execute that code at any one time. The
    return value is the return from the last body form.

    Each `monitor` form has its own private mutex and the locking and
    evaluation is as per `with-mutex` above. A standard mutex
    (`make-mutex`) is used, which means the body must not
    recursively re-enter the `monitor` form.

    The term “monitor” comes from operating system theory, where it
    means a particular bit of code managing access to some resource and
    which only ever executes on behalf of one process at any one time.

---

Next: [Futures](#Futures), Previous: [Mutexes and Condition Variables](#Mutexes-and-Condition-Variables), Up: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.22.6 Blocking in Guile Mode [¶](#Blocking-in-Guile-Mode)

Up to Guile version 1.8, a thread blocked in guile mode would prevent
the garbage collector from running. Thus threads had to explicitly
leave guile mode with `scm_without_guile ()` before making a
potentially blocking call such as a mutex lock, a `select ()`
system call, etc. The following functions could be used to temporarily
leave guile mode or to perform some common blocking operations in a
supported way.

Starting from Guile 2.0, blocked threads no longer hinder garbage
collection. Thus, the functions below are not needed anymore. They can
still be used to inform the GC that a thread is about to block, giving
it a (small) optimization opportunity for “stop the world” garbage
collections, should they occur while the thread is blocked.

C Function: `void *` **scm\_without\_guile** `(void *(*func) (void *), void *data)` [¶](#index-scm_005fwithout_005fguile)
:   Leave guile mode, call func on data, enter guile mode and
    return the result of calling func.

    While a thread has left guile mode, it must not call any libguile
    functions except `scm_with_guile` or `scm_without_guile` and
    must not use any libguile macros. Also, local variables of type
    `SCM` that are allocated while not in guile mode are not
    protected from the garbage collector.

    When used from non-guile mode, calling `scm_without_guile` is
    still allowed: it simply calls func. In that way, you can leave
    guile mode without having to know whether the current thread is in
    guile mode or not.

C Function: `int` **scm\_pthread\_mutex\_lock** `(pthread_mutex_t *mutex)` [¶](#index-scm_005fpthread_005fmutex_005flock)
:   Like `pthread_mutex_lock`, but leaves guile mode while waiting for
    the mutex.

C Function: `int` **scm\_pthread\_cond\_wait** `(pthread_cond_t *cond, pthread_mutex_t *mutex)` [¶](#index-scm_005fpthread_005fcond_005fwait)

C Function: `int` **scm\_pthread\_cond\_timedwait** `(pthread_cond_t *cond, pthread_mutex_t *mutex, struct timespec *abstime)` [¶](#index-scm_005fpthread_005fcond_005ftimedwait)
:   Like `pthread_cond_wait` and `pthread_cond_timedwait`, but
    leaves guile mode while waiting for the condition variable.

C Function: `int` **scm\_std\_select** `(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout)` [¶](#index-scm_005fstd_005fselect)
:   Like `select` but leaves guile mode while waiting. Also, the
    delivery of an async causes this function to be interrupted with error
    code `EINTR`.

C Function: `unsigned int` **scm\_std\_sleep** `(unsigned int seconds)` [¶](#index-scm_005fstd_005fsleep)
:   Like `sleep`, but leaves guile mode while sleeping. Also, the
    delivery of an async causes this function to be interrupted.

C Function: `unsigned long` **scm\_std\_usleep** `(unsigned long usecs)` [¶](#index-scm_005fstd_005fusleep)
:   Like `usleep`, but leaves guile mode while sleeping. Also, the
    delivery of an async causes this function to be interrupted.

---

Next: [Parallel forms](#Parallel-Forms), Previous: [Blocking in Guile Mode](#Blocking), Up: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.22.7 Futures [¶](#Futures-1)

The `(ice-9 futures)` module provides *futures*, a construct
for fine-grain parallelism. A future is a wrapper around an expression
whose computation may occur in parallel with the code of the calling
thread, and possibly in parallel with other futures. Like promises,
futures are essentially proxies that can be queried to obtain the value
of the enclosed expression:

```
(touch (future (+ 2 3)))
⇒ 5
```

However, unlike promises, the expression associated with a future may be
evaluated on another CPU core, should one be available. This supports
*fine-grain parallelism*, because even relatively small computations
can be embedded in futures. Consider this sequential code:

```
(define (find-prime lst1 lst2)
  (or (find prime? lst1)
      (find prime? lst2)))
```

The two arms of `or` are potentially computation-intensive. They
are independent of one another, yet, they are evaluated sequentially
when the first one returns `#f`. Using futures, one could rewrite
it like this:

```
(define (find-prime lst1 lst2)
  (let ((f (future (find prime? lst2))))
    (or (find prime? lst1)
        (touch f))))
```

This preserves the semantics of `find-prime`. On a multi-core
machine, though, the computation of `(find prime? lst2)` may be
done in parallel with that of the other `find` call, which can
reduce the execution time of `find-prime`.

Futures may be nested: a future can itself spawn and then `touch`
other futures, leading to a directed acyclic graph of futures. Using
this facility, a parallel `map` procedure can be defined along
these lines:

```
(use-modules (ice-9 futures) (ice-9 match))

(define (par-map proc lst)
  (match lst
    (()
     '())
    ((head tail ...)
     (let ((tail (future (par-map proc tail)))
           (head (proc head)))
       (cons head (touch tail))))))
```

Note that futures are intended for the evaluation of purely functional
expressions. Expressions that have side-effects or rely on I/O may
require additional care, such as explicit synchronization
(see [Mutexes and Condition Variables](#Mutexes-and-Condition-Variables)).

Guile’s futures are implemented on top of POSIX threads
(see [Threads](#Threads)). Internally, a fixed-size pool of threads is used to
evaluate futures, such that offloading the evaluation of an expression
to another thread doesn’t incur thread creation costs. By default, the
pool contains one thread per available CPU core, minus one, to account
for the main thread. The number of available CPU cores is determined
using `current-processor-count` (see [Processes](#Processes)).

When a thread touches a future that has not completed yet, it processes
any pending future while waiting for it to complete, or just waits if
there are no pending futures. When `touch` is called from within a
future, the execution of the calling future is suspended, allowing its
host thread to process other futures, and resumed when the touched
future has completed. This suspend/resume is achieved by capturing the
calling future’s continuation, and later reinstating it (see [delimited continuations](#Prompts)).

Scheme Syntax: **future** exp [¶](#index-future)
:   Return a future for expression exp. This is equivalent to:

    ```
    (make-future (lambda () exp))
    ```

Scheme Procedure: **make-future** thunk [¶](#index-make_002dfuture)
:   Return a future for thunk, a zero-argument procedure.

    This procedure returns immediately. Execution of thunk may begin
    in parallel with the calling thread’s computations, if idle CPU cores
    are available, or it may start when `touch` is invoked on the
    returned future.

    If the execution of thunk throws an exception, that exception will
    be re-thrown when `touch` is invoked on the returned future.

Scheme Procedure: **future?** obj [¶](#index-future_003f)
:   Return `#t` if obj is a future.

Scheme Procedure: **touch** f [¶](#index-touch)
:   Return the result of the expression embedded in future f.

    If the result was already computed in parallel, `touch` returns
    instantaneously. Otherwise, it waits for the computation to complete,
    if it already started, or initiates it. In the former case, the calling
    thread may process other futures in the meantime.

---

Previous: [Futures](#Futures), Up: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.22.8 Parallel forms [¶](#Parallel-forms)

The functions described in this section are available from

```
(use-modules (ice-9 threads))
```

They provide high-level parallel constructs. The following functions
are implemented in terms of futures (see [Futures](#Futures)). Thus they are
relatively cheap as they re-use existing threads, and portable, since
they automatically use one thread per available CPU core.

syntax: **parallel** expr … [¶](#index-parallel)
:   Evaluate each expr expression in parallel, each in its own thread.
    Return the results of n expressions as a set of n multiple
    values (see [Returning and Accepting Multiple Values](#Multiple-Values)).

syntax: **letpar** ((var expr) …) body1 body2 … [¶](#index-letpar)
:   Evaluate each expr in parallel, each in its own thread, then bind
    the results to the corresponding var variables, and then evaluate
    body1 body2 ...

    `letpar` is like `let` (see [Local Variable Bindings](#Local-Bindings)), but all the
    expressions for the bindings are evaluated in parallel.

Scheme Procedure: **par-map** proc lst1 lst2 … [¶](#index-par_002dmap)

Scheme Procedure: **par-for-each** proc lst1 lst2 … [¶](#index-par_002dfor_002deach)
:   Call proc on the elements of the given lists. `par-map`
    returns a list comprising the return values from proc.
    `par-for-each` returns an unspecified value, but waits for all
    calls to complete.

    The proc calls are `(proc elem1 elem2
    …)`, where each elem is from the corresponding lst .
    Each lst must be the same length. The calls are potentially made
    in parallel, depending on the number of CPU cores available.

    These functions are like `map` and `for-each` (see [List Mapping](#List-Mapping)), but make their proc calls in parallel.

Unlike those above, the functions described below take a number of
threads as an argument. This makes them inherently non-portable since
the specified number of threads may differ from the number of available
CPU cores as returned by `current-processor-count`
(see [Processes](#Processes)). In addition, these functions create the specified
number of threads when they are called and terminate them upon
completion, which makes them quite expensive.

Therefore, they should be avoided.

Scheme Procedure: **n-par-map** n proc lst1 lst2 … [¶](#index-n_002dpar_002dmap)

Scheme Procedure: **n-par-for-each** n proc lst1 lst2 … [¶](#index-n_002dpar_002dfor_002deach)
:   Call proc on the elements of the given lists, in the same way as
    `par-map` and `par-for-each` above, but use no more than
    n threads at any one time. The order in which calls are
    initiated within that threads limit is unspecified.

    These functions are good for controlling resource consumption if
    proc calls might be costly, or if there are many to be made. On
    a dual-CPU system for instance *n=4* might be enough to
    keep the CPUs utilized, and not consume too much memory.

Scheme Procedure: **n-for-each-par-map** n sproc pproc lst1 lst2 … [¶](#index-n_002dfor_002deach_002dpar_002dmap)
:   Apply pproc to the elements of the given lists, and apply
    sproc to each result returned by pproc. The final return
    value is unspecified, but all calls will have been completed before
    returning.

    The calls made are `(sproc (pproc elem1 …
    elemN))`, where each elem is from the corresponding
    lst. Each lst must have the same number of elements.

    The pproc calls are made in parallel, in separate threads. No more
    than n threads are used at any one time. The order in which
    pproc calls are initiated within that limit is unspecified.

    The sproc calls are made serially, in list element order, one at
    a time. pproc calls on later elements may execute in parallel
    with the sproc calls. Exactly which thread makes each
    sproc call is unspecified.

    This function is designed for individual calculations that can be done
    in parallel, but with results needing to be handled serially, for
    instance to write them to a file. The n limit on threads
    controls system resource usage when there are many calculations or
    when they might be costly.

    It will be seen that `n-for-each-par-map` is like a combination
    of `n-par-map` and `for-each`,

    ```
    (for-each sproc (n-par-map n pproc lst1 ... lstN))
    ```

    But the actual implementation is more efficient since each sproc
    call, in turn, can be initiated once the relevant pproc call has
    completed, it doesn’t need to wait for all to finish.

---

Next: [Support for Other Languages](#Other-Languages), Previous: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.23 Configuration, Features and Runtime Options [¶](#Configuration_002c-Features-and-Runtime-Options)

Why is my Guile different from your Guile? There are three kinds of
possible variation:

* build differences — different versions of the Guile source code,
  installation directories, configuration flags that control pieces of
  functionality being included or left out, etc.
* differences in dynamically loaded code — behavior and features
  provided by modules that can be dynamically loaded into a running Guile
* different runtime options — some of the options that are provided for
  controlling Guile’s behavior may be set differently.

Guile provides “introspective” variables and procedures to query all
of these possible variations at runtime. For runtime options, it also
provides procedures to change the settings of options and to obtain
documentation on what the options mean.

* [Configuration, Build and Installation](#Build-Config)
* [Feature Tracking](#Feature-Tracking)
* [Runtime Options](#Runtime-Options)

---

Next: [Feature Tracking](#Feature-Tracking), Up: [Configuration, Features and Runtime Options](#Options-and-Config)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.23.1 Configuration, Build and Installation [¶](#Configuration_002c-Build-and-Installation)

The following procedures and variables provide information about how
Guile was configured, built and installed on your system.

Scheme Procedure: **version** [¶](#index-version)

Scheme Procedure: **effective-version** [¶](#index-effective_002dversion)

Scheme Procedure: **major-version** [¶](#index-major_002dversion)

Scheme Procedure: **minor-version** [¶](#index-minor_002dversion)

Scheme Procedure: **micro-version** [¶](#index-micro_002dversion)

C Function: **scm\_version** () [¶](#index-scm_005fversion)

C Function: **scm\_effective\_version** () [¶](#index-scm_005feffective_005fversion)

C Function: **scm\_major\_version** () [¶](#index-scm_005fmajor_005fversion)

C Function: **scm\_minor\_version** () [¶](#index-scm_005fminor_005fversion)

C Function: **scm\_micro\_version** () [¶](#index-scm_005fmicro_005fversion)
:   Return a string describing Guile’s full version number, effective
    version number, major, minor or micro version number, respectively.
    The `effective-version` function returns the version name that
    should remain unchanged during a stable series. Currently that means
    that it omits the micro version. The effective version should be used
    for items like the versioned share directory name
    i.e. `/usr/share/guile/3.0/`

    ```
    (version) ⇒ "3.0.0"
    (effective-version) ⇒ "3.0"
    (major-version) ⇒ "3"
    (minor-version) ⇒ "0"
    (micro-version) ⇒ "0"
    ```

Scheme Procedure: **%package-data-dir** [¶](#index-_0025package_002ddata_002ddir)

C Function: **scm\_sys\_package\_data\_dir** () [¶](#index-scm_005fsys_005fpackage_005fdata_005fdir)
:   Return the name of the directory under which Guile Scheme files in
    general are stored. On Unix-like systems, this is usually
    `/usr/local/share/guile` or `/usr/share/guile`.

Scheme Procedure: **%library-dir** [¶](#index-_0025library_002ddir)

C Function: **scm\_sys\_library\_dir** () [¶](#index-scm_005fsys_005flibrary_005fdir)
:   Return the name of the directory where the Guile Scheme files that
    belong to the core Guile installation (as opposed to files from a 3rd
    party package) are installed. On Unix-like systems this is usually
    `/usr/local/share/guile/GUILE_EFFECTIVE_VERSION` or
    `/usr/share/guile/GUILE_EFFECTIVE_VERSION`;

    for example `/usr/local/share/guile/3.0`.

Scheme Procedure: **%site-dir** [¶](#index-_0025site_002ddir-1)

C Function: **scm\_sys\_site\_dir** () [¶](#index-scm_005fsys_005fsite_005fdir)
:   Return the name of the directory where Guile Scheme files specific to
    your site should be installed. On Unix-like systems, this is usually
    `/usr/local/share/guile/site` or `/usr/share/guile/site`.

Scheme Procedure: **%site-ccache-dir** [¶](#index-_0025site_002dccache_002ddir-1)

C Function: **scm\_sys\_site\_ccache\_dir** () [¶](#index-scm_005fsys_005fsite_005fccache_005fdir)
:   Return the directory where users should install compiled `.go`
    files for use with this version of Guile. Might look something like
    `/usr/lib/guile/3.0/site-ccache`.

Variable: **%guile-build-info** [¶](#index-_0025guile_002dbuild_002dinfo)
:   Alist of information collected during the building of a particular
    Guile. Entries can be grouped into one of several categories:
    directories, env vars, and versioning info.

    Briefly, here are the keys in `%guile-build-info`, by group:

    directories
    :   srcdir, top\_srcdir, prefix, exec\_prefix, bindir, sbindir, libexecdir,
        datadir, sysconfdir, sharedstatedir, localstatedir, libdir, infodir,
        mandir, includedir, pkgdatadir, pkglibdir, pkgincludedir

    env vars
    :   LIBS

    versioning info
    :   guileversion, libguileinterface, buildstamp

    Values are all strings. The value for `LIBS` is typically found
    also as a part of `pkg-config --libs
    guile-3.0` output. The value for
    `guileversion` has form X.Y.Z, and should be the same as returned
    by `(version)`. The value for `libguileinterface` is libtool
    compatible and has form CURRENT:REVISION:AGE (see [Library interface versions](https://www.gnu.org/software/libtool/manual/libtool.html#Versioning) in GNU Libtool). The value for
    `buildstamp` is the output of the command ‘`date -u +'%Y-%m-%d
    %T'`’ (UTC).

    In the source, `%guile-build-info` is initialized from
    libguile/libpath.h, which is completely generated, so deleting this file
    before a build guarantees up-to-date values for that build.

Variable: **%host-type** [¶](#index-_0025host_002dtype)
:   The canonical host type (GNU triplet) of the host Guile was configured
    for, e.g., `"x86_64-unknown-linux-gnu"` (see [Canonicalizing](https://www.gnu.org/software/autoconf/manual/autoconf.html#Canonicalizing) in The GNU Autoconf Manual).

---

Next: [Runtime Options](#Runtime-Options), Previous: [Configuration, Build and Installation](#Build-Config), Up: [Configuration, Features and Runtime Options](#Options-and-Config)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.23.2 Feature Tracking [¶](#Feature-Tracking-1)

Guile has a Scheme level variable `*features*` that keeps track to
some extent of the features that are available in a running Guile.
`*features*` is a list of symbols, for example `threads`, each
of which describes a feature of the running Guile process.

Variable: **\*features\*** [¶](#index-_002afeatures_002a)
:   A list of symbols describing available features of the Guile process.

You shouldn’t modify the `*features*` variable directly using
`set!`. Instead, see the procedures that are provided for this
purpose in the following subsection.

* [Feature Manipulation](#Feature-Manipulation)
* [Common Feature Symbols](#Common-Feature-Symbols)

---

Next: [Common Feature Symbols](#Common-Feature-Symbols), Up: [Feature Tracking](#Feature-Tracking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.23.2.1 Feature Manipulation [¶](#Feature-Manipulation-1)

To check whether a particular feature is available, use the
`provided?` procedure:

Scheme Procedure: **provided?** feature [¶](#index-provided_003f)

Deprecated Scheme Procedure: **feature?** feature [¶](#index-feature_003f)
:   Return `#t` if the specified feature is available, otherwise
    `#f`.

To advertise a feature from your own Scheme code, you can use the
`provide` procedure:

Scheme Procedure: **provide** feature [¶](#index-provide)
:   Add feature to the list of available features in this Guile
    process.

For C code, the equivalent function takes its feature name as a
`char *` argument for convenience:

C Function: `void` **scm\_add\_feature** `(const char *str)` [¶](#index-scm_005fadd_005ffeature)
:   Add a symbol with name str to the list of available features in
    this Guile process.

---

Previous: [Feature Manipulation](#Feature-Manipulation), Up: [Feature Tracking](#Feature-Tracking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.23.2.2 Common Feature Symbols [¶](#Common-Feature-Symbols-1)

In general, a particular feature may be available for one of two
reasons. Either because the Guile library was configured and compiled
with that feature enabled — i.e. the feature is built into the library
on your system. Or because some C or Scheme code that was dynamically
loaded by Guile has added that feature to the list.

In the first category, here are the features that the current version of
Guile may define (depending on how it is built), and what they mean.

`array`
:   Indicates support for arrays (see [Arrays](#Arrays)).

`array-for-each`
:   Indicates availability of `array-for-each` and other array mapping
    procedures (see [Arrays](#Arrays)).

`char-ready?`
:   Indicates that the `char-ready?` function is available
    (see [Venerable Port Interfaces](#Venerable-Port-Interfaces)).

`complex`
:   Indicates support for complex numbers.

`current-time`
:   Indicates availability of time-related functions: `times`,
    `get-internal-run-time` and so on (see [Time](#Time)).

`debug-extensions`
:   Indicates that the debugging evaluator is available, together with the
    options for controlling it.

`delay`
:   Indicates support for promises (see [Delayed Evaluation](#Delayed-Evaluation)).

`EIDs`
:   Indicates that the `geteuid` and `getegid` really return
    effective user and group IDs (see [Processes](#Processes)).

`inexact`
:   Indicates support for inexact numbers.

`i/o-extensions`
:   Indicates availability of the following extended I/O procedures:
    `ftell`, `redirect-port`, `dup->fdes`, `dup2`,
    `fileno`, `isatty?`, `fdopen`,
    `primitive-move->fdes` and `fdes->ports` (see [Ports and File Descriptors](#Ports-and-File-Descriptors)).

`net-db`
:   Indicates availability of network database functions:
    `scm_gethost`, `scm_getnet`, `scm_getproto`,
    `scm_getserv`, `scm_sethost`, `scm_setnet`, `scm_setproto`,
    `scm_setserv`, and their ‘byXXX’ variants (see [Network Databases](#Network-Databases)).

`posix`
:   Indicates support for POSIX functions: `pipe`, `getgroups`,
    `kill`, `execl` and so on (see [POSIX System Calls and Networking](#POSIX)).

`fork`
:   Indicates support for the POSIX `fork` function (see [`primitive-fork`](#Processes)).

`popen`
:   Indicates support for `open-pipe` in the `(ice-9 popen)`
    module (see [Pipes](#Pipes)).

`random`
:   Indicates availability of random number generation functions:
    `random`, `copy-random-state`, `random-uniform` and so on
    (see [Random Number Generation](#Random)).

`reckless`
:   Indicates that Guile was built with important checks omitted — you
    should never see this!

`regex`
:   Indicates support for POSIX regular expressions using
    `make-regexp`, `regexp-exec` and friends (see [Regexp Functions](#Regexp-Functions)).

`socket`
:   Indicates availability of socket-related functions: `socket`,
    `bind`, `connect` and so on (see [Network Sockets and Communication](#Network-Sockets-and-Communication)).

`sort`
:   Indicates availability of sorting and merging functions
    (see [Sorting](#Sorting)).

`system`
:   Indicates that the `system` function is available
    (see [Processes](#Processes)).

`threads`
:   Indicates support for multithreading (see [Threads](#Threads)).

`values`
:   Indicates support for multiple return values using `values` and
    `call-with-values` (see [Returning and Accepting Multiple Values](#Multiple-Values)).

Available features in the second category depend, by definition, on what
additional code your Guile process has loaded in. The following table
lists features that you might encounter for this reason.

`defmacro`
:   Indicates that the `defmacro` macro is available (see [Macros](#Macros)).

`describe`
:   Indicates that the `(oop goops describe)` module has been loaded,
    which provides a procedure for describing the contents of GOOPS
    instances.

`readline`
:   Indicates that Guile has loaded in Readline support, for command line
    editing (see [Readline Support](#Readline-Support)).

`record`
:   Indicates support for record definition using `make-record-type`
    and friends (see [Records](#Records)).

Although these tables may seem exhaustive, it is probably unwise in
practice to rely on them, as the correspondences between feature symbols
and available procedures/behavior are not strictly defined. If you are
writing code that needs to check for the existence of some procedure, it
is probably safer to do so directly using the `defined?` procedure
than to test for the corresponding feature using `provided?`.

---

Previous: [Feature Tracking](#Feature-Tracking), Up: [Configuration, Features and Runtime Options](#Options-and-Config)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.23.3 Runtime Options [¶](#Runtime-Options-1)

There are a number of runtime options available for parameterizing
built-in procedures, like `read`, and built-in behavior, like what
happens on an uncaught error.

For more information on reader options, See [Reading Scheme Code](#Scheme-Read).

For more information on print options, See [Writing Scheme Values](#Scheme-Write).

Finally, for more information on debugger options, See [Debug options](#Debug-Options).

* [Examples of option use](#Examples-of-option-use)

#### 6.23.3.1 Examples of option use [¶](#Examples-of-option-use)

Here is an example of a session in which some read and debug option
handling procedures are used. In this example, the user

1. Notices that the symbols `abc` and `aBc` are not the same
2. Examines the `read-options`, and sees that `case-insensitive`
   is set to “no”.
3. Enables `case-insensitive`
4. Quits the recursive prompt
5. Verifies that now `aBc` and `abc` are the same

```
scheme@(guile-user)> (define abc "hello")
scheme@(guile-user)> abc
$1 = "hello"
scheme@(guile-user)> aBc
<unknown-location>: warning: possibly unbound variable `aBc'
ERROR: In procedure module-lookup:
ERROR: Unbound variable: aBc
Entering a new prompt.  Type `,bt' for a backtrace or `,q' to continue.
scheme@(guile-user) [1]> (read-options 'help)
copy              no    Copy source code expressions.
positions         yes   Record positions of source code expressions.
case-insensitive  no    Convert symbols to lower case.
keywords          #f    Style of keyword recognition: #f, 'prefix or 'postfix.
r6rs-hex-escapes  no    Use R6RS variable-length character and string hex escapes.
square-brackets   yes   Treat `[' and `]' as parentheses, for R6RS compatibility.
hungry-eol-escapes no   In strings, consume leading whitespace after an
                        escaped end-of-line.
curly-infix       no    Support SRFI-105 curly infix expressions.
scheme@(guile-user) [1]> (read-enable 'case-insensitive)
$2 = (square-brackets keywords #f case-insensitive positions)
scheme@(guile-user) [1]> ,q
scheme@(guile-user)> aBc
$3 = "hello"
```

---

Next: [Support for Internationalization](#Internationalization), Previous: [Configuration, Features and Runtime Options](#Options-and-Config), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.24 Support for Other Languages [¶](#Support-for-Other-Languages)

In addition to Scheme, a user may write a Guile program in an increasing
number of other languages. Currently supported languages include Emacs
Lisp and ECMAScript.

Guile is still fundamentally a Scheme, but it tries to support a wide
variety of language building-blocks, so that other languages can be
implemented on top of Guile. This allows users to write or extend
applications in languages other than Scheme, too. This section describes
the languages that have been implemented.

(For details on how to implement a language, See [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine).)

* [Using Other Languages](#Using-Other-Languages)
* [Emacs Lisp](#Emacs-Lisp)
* [ECMAScript](#ECMAScript)

---

Next: [Emacs Lisp](#Emacs-Lisp), Up: [Support for Other Languages](#Other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.24.1 Using Other Languages [¶](#Using-Other-Languages-1)

There are currently only two ways to access other languages from within
Guile: at the REPL, and programmatically, via `compile`,
`read-and-compile`, and `compile-file`.

The REPL is Guile’s command prompt (see [Using Guile Interactively](#Using-Guile-Interactively)).
The REPL has a concept of the “current language”, which defaults to
Scheme. The user may change that language, via the meta-command
`,language`.

For example, the following meta-command enables Emacs Lisp input:

```
scheme@(guile-user)> ,language elisp
Happy hacking with Emacs Lisp!  To switch back, type `,L scheme'.
elisp@(guile-user)> (eq 1 2)
$1 = #nil
```

Each language has its short name: for example, `elisp`, for Elisp.
The same short name may be used to compile source code programmatically,
via `compile`:

```
elisp@(guile-user)> ,L scheme
Happy hacking with Guile Scheme!  To switch back, type `,L elisp'.
scheme@(guile-user)> (compile '(eq 1 2) #:from 'elisp)
$2 = #nil
```

Granted, as the input to `compile` is a datum, this works best for
Lispy languages, which have a straightforward datum representation.
Other languages that need more parsing are better dealt with as strings.

The easiest way to deal with syntax-heavy language is with files, via
`compile-file` and friends. However it is possible to invoke a
language’s reader on a port, and then compile the resulting expression
(which is a datum at that point). For more information,
See [Compiling Scheme Code](#Compilation).

For more details on introspecting aspects of different languages,
See [Compiler Tower](#Compiler-Tower).

---

Next: [ECMAScript](#ECMAScript), Previous: [Using Other Languages](#Using-Other-Languages), Up: [Support for Other Languages](#Other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.24.2 Emacs Lisp [¶](#Emacs-Lisp-1)

Emacs Lisp (Elisp) is a dynamically-scoped Lisp dialect used in the
Emacs editor. See [Overview](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Top) in Emacs Lisp, for more
information on Emacs Lisp.

We hope that eventually Guile’s implementation of Elisp will be good
enough to replace Emacs’ own implementation of Elisp. For that reason,
we have thought long and hard about how to support the various features
of Elisp in a performant and compatible manner.

Readers familiar with Emacs Lisp might be curious about how exactly
these various Elisp features are supported in Guile. The rest of this
section focuses on addressing these concerns of the Elisp elect.

* [Nil](#Nil)
* [Dynamic Binding](#Dynamic-Binding)
* [Other Elisp Features](#Other-Elisp-Features)

---

Next: [Dynamic Binding](#Dynamic-Binding), Up: [Emacs Lisp](#Emacs-Lisp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.24.2.1 Nil [¶](#Nil-1)

`nil` in ELisp is an amalgam of Scheme’s `#f` and `'()`.
It is false, and it is the end-of-list; thus it is a boolean, and a list
as well.

Guile has chosen to support `nil` as a separate value, distinct
from `#f` and `'()`. This allows existing Scheme and Elisp
code to maintain their current semantics. `nil`, which in Elisp
would just be written and read as `nil`, in Scheme has the external
representation `#nil`.

In Elisp code, `#nil`, `#f`, and `'()` behave like
`nil`, in the sense that they are all interpreted as `nil` by
Elisp `if`, `cond`, `when`, `not`, `null`, etc.
To test whether Elisp would interpret an object as `nil` from
within Scheme code, use `nil?`:

Scheme Procedure: **nil?** obj [¶](#index-nil_003f)
:   Return `#t` if obj would be interpreted as `nil` by
    Emacs Lisp code, else return `#f`.

    ```
    (nil? #nil) ⇒ #t
    (nil? #f)   ⇒ #t
    (nil? '())  ⇒ #t
    (nil? 3)    ⇒ #f
    ```

This decision to have `nil` as a low-level distinct value
facilitates interoperability between the two languages. Guile has chosen
to have Scheme deal with `nil` as follows:

```
(boolean? #nil) ⇒ #t
(not #nil) ⇒ #t
(null? #nil) ⇒ #t
```

And in C, one has:

```
scm_is_bool (SCM_ELISP_NIL) ⇒ 1
scm_is_false (SCM_ELISP_NIL) ⇒ 1
scm_is_null (SCM_ELISP_NIL) ⇒ 1
```

In this way, a version of `fold` written in Scheme can correctly
fold a function written in Elisp (or in fact any other language) over a
nil-terminated list, as Elisp makes. The converse holds as well; a
version of `fold` written in Elisp can fold over a
`'()`-terminated list, as made by Scheme.

On a low level, the bit representations for `#f`, `#t`,
`nil`, and `'()` are made in such a way that they differ by
only one bit, and so a test for, for example, `#f`-or-`nil`
may be made very efficiently. See `libguile/boolean.h`, for more
information.

#### Equality [¶](#Equality-2)

Since Scheme’s `equal?` must be transitive, and `'()`
is not `equal?` to `#f`, to Scheme `nil` is not
`equal?` to `#f` or `'()`.

```
(eq? #f '()) ⇒ #f
(eq? #nil '()) ⇒ #f
(eq? #nil #f) ⇒ #f
(eqv? #f '()) ⇒ #f
(eqv? #nil '()) ⇒ #f
(eqv? #nil #f) ⇒ #f
(equal? #f '()) ⇒ #f
(equal? #nil '()) ⇒ #f
(equal? #nil #f) ⇒ #f
```

However, in Elisp, `'()`, `#f`, and `nil` are all
`equal` (though not `eq`).

```
(defvar f (make-scheme-false))
(defvar eol (make-scheme-null))
(eq f eol) ⇒ nil
(eq nil eol) ⇒ nil
(eq nil f) ⇒ nil
(equal f eol) ⇒ t
(equal nil eol) ⇒ t
(equal nil f) ⇒ t
```

These choices facilitate interoperability between Elisp and Scheme code,
but they are not perfect. Some code that is correct standard Scheme is
not correct in the presence of a second false and null value. For
example:

```
(define (truthiness x)
  (if (eq? x #f)
      #f
      #t))
```

This code seems to be meant to test a value for truth, but now that
there are two false values, `#f` and `nil`, it is no longer
correct.

Similarly, there is the loop:

```
(define (my-length l)
  (let lp ((l l) (len 0))
    (if (eq? l '())
        len
        (lp (cdr l) (1+ len)))))
```

Here, `my-length` will raise an error if l is a
`nil`-terminated list.

Both of these examples are correct standard Scheme, but, depending on
what they really want to do, they are not correct Guile Scheme.
Correctly written, they would test the *properties* of falsehood or
nullity, not the individual members of that set. That is to say, they
should use `not` or `null?` to test for falsehood or nullity,
not `eq?` or `memv` or the like.

Fortunately, using `not` and `null?` is in good style, so all
well-written standard Scheme programs are correct, in Guile Scheme.

Here are correct versions of the above examples:

```
(define (truthiness* x)
  (if (not x)
      #f
      #t))
;; or: (define (t* x) (not (not x)))
;; or: (define (t** x) x)

(define (my-length* l)
  (let lp ((l l) (len 0))
    (if (null? l)
        len
        (lp (cdr l) (1+ len)))))
```

This problem has a mirror-image case in Elisp:

```
(defun my-falsep (x)
  (if (eq x nil)
      t
      nil))
```

Guile can warn when compiling code that has equality comparisons with
`#f`, `'()`, or `nil`. See [Compiling Scheme Code](#Compilation), for details.

---

Next: [Other Elisp Features](#Other-Elisp-Features), Previous: [Nil](#Nil), Up: [Emacs Lisp](#Emacs-Lisp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.24.2.2 Dynamic Binding [¶](#Dynamic-Binding-1)

In contrast to Scheme, which uses “lexical scoping”, Emacs Lisp scopes
its variables dynamically. Guile supports dynamic scoping with its
“fluids” facility. See [Fluids and Dynamic States](#Fluids-and-Dynamic-States), for more
information.

---

Previous: [Dynamic Binding](#Dynamic-Binding), Up: [Emacs Lisp](#Emacs-Lisp)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.24.2.3 Other Elisp Features [¶](#Other-Elisp-Features-1)

Buffer-local and mode-local variables should be mentioned here, along
with buckybits on characters, Emacs primitive data types, the
Lisp-2-ness of Elisp, and other things. Contributions to the
documentation are most welcome!

---

Previous: [Emacs Lisp](#Emacs-Lisp), Up: [Support for Other Languages](#Other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.24.3 ECMAScript [¶](#ECMAScript-1)

[ECMAScript](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf)
was not the first non-Schemey language implemented by Guile, but it was
the first implemented for Guile’s bytecode compiler. The goal was to
support ECMAScript version 3.1, a relatively small language, but the
implementer was completely irresponsible and got distracted by other
things before finishing the standard library, and even some bits of the
syntax. So, ECMAScript does deserve a mention in the manual, but it
doesn’t deserve an endorsement until its implementation is completed,
perhaps by some more responsible hacker.

In the meantime, the charitable user might investigate such invocations
as `,L ecmascript` and `cat test-suite/tests/ecmascript.test`.

---

Next: [Debugging Infrastructure](#Debugging), Previous: [Support for Other Languages](#Other-Languages), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
