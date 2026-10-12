Scheme Procedure: **exit** [status] [¶](#index-exit)
:   Terminate the current process with proper unwinding of the Scheme stack.
    The exit status zero if status is not supplied. If status
    is supplied, and it is an integer, that integer is used as the exit
    status. If status is `#t` or `#f`, the exit status is
    EXIT\_SUCCESS or EXIT\_FAILURE, respectively.

    The procedure `exit` is an alias of `quit`. They have the
    same functionality.

Scheme Variable: **EXIT\_SUCCESS** [¶](#index-EXIT_005fSUCCESS)

Scheme Variable: **EXIT\_FAILURE** [¶](#index-EXIT_005fFAILURE)
:   These constants represent the standard exit codes for success (zero) or
    failure (one.)

Scheme Procedure: **primitive-exit** [status] [¶](#index-primitive_002dexit)

Scheme Procedure: **primitive-\_exit** [status] [¶](#index-primitive_002d_005fexit)

C Function: **scm\_primitive\_exit** (status) [¶](#index-scm_005fprimitive_005fexit)

C Function: **scm\_primitive\_\_exit** (status) [¶](#index-scm_005fprimitive_005f_005fexit)
:   Terminate the current process without unwinding the Scheme stack. The
    exit status is status if supplied, otherwise zero.

    `primitive-exit` uses the C `exit` function and hence runs
    usual C level cleanups (flush output streams, call `atexit`
    functions, etc, see [Normal Termination](/libc/latest/en/libc.html#Normal-Termination) in The GNU C Library
    Reference Manual)).

    `primitive-_exit` is the `_exit` system call
    (see [Termination Internals](/libc/latest/en/libc.html#Termination-Internals) in The GNU C Library Reference
    Manual). This terminates the program immediately, with neither
    Scheme-level nor C-level cleanups.

    The typical use for `primitive-_exit` is from a child process
    created with `primitive-fork`. For example in a Gdk program the
    child process inherits the X server connection and a C-level
    `atexit` cleanup which will close that connection. But closing
    in the child would upset the protocol in the parent, so
    `primitive-_exit` should be used to exit without that.

Scheme Procedure: **execl** filename arg … [¶](#index-execl)

C Function: **scm\_execl** (filename, args) [¶](#index-scm_005fexecl)
:   Executes the file named by filename as a new process image.
    The remaining arguments are supplied to the process; from a C program
    they are accessible as the `argv` argument to `main`.
    Conventionally the first arg is the same as filename.
    All arguments must be strings.

    If arg is missing, filename is executed with a null
    argument list, which may have system-dependent side-effects.

    This procedure is currently implemented using the `execv` system
    call, but we call it `execl` because of its Scheme calling interface.

Scheme Procedure: **execlp** filename arg … [¶](#index-execlp)

C Function: **scm\_execlp** (filename, args) [¶](#index-scm_005fexeclp)
:   Similar to `execl`, however if
    filename does not contain a slash
    then the file to execute will be located by searching the
    directories listed in the `PATH` environment variable.

    This procedure is currently implemented using the `execvp` system
    call, but we call it `execlp` because of its Scheme calling interface.

Scheme Procedure: **execle** filename env arg … [¶](#index-execle)

C Function: **scm\_execle** (filename, env, args) [¶](#index-scm_005fexecle)
:   Similar to `execl`, but the environment of the new process is
    specified by env, which must be a list of strings as returned by the
    `environ` procedure.

    This procedure is currently implemented using the `execve` system
    call, but we call it `execle` because of its Scheme calling interface.

Scheme Procedure: **primitive-fork** [¶](#index-primitive_002dfork)

C Function: **scm\_fork** () [¶](#index-scm_005ffork)
:   Creates a new “child” process by duplicating the current “parent” process.
    In the child the return value is 0. In the parent the return value is
    the integer process ID of the child.

    Note that it is unsafe to fork a process that has multiple threads
    running, as only the thread that calls `primitive-fork` will
    persist in the child. Any resources that other threads held, such as
    locked mutexes or open file descriptors, are lost. Indeed,
    POSIX specifies that only async-signal-safe procedures are
    safe to call after a multithreaded fork, which is a very limited set.
    Guile issues a warning if it detects a fork from a multi-threaded
    program.

    > **Note:** If you are looking to spawn a process with some pipes set up, using the
    > `spawn` procedure described below will be more robust (in
    > particular in multi-threaded contexts), more portable, and usually more
    > efficient than the combination of `primitive-fork` and
    > `execl`.

    This procedure has been renamed from `fork` to avoid a naming conflict
    with the scsh fork.

Scheme Procedure: **spawn** program arguments [#:environment=(environ)] [#:input=(current-input-port)] [#:output=(current-output-port)] [#:error=(current-error-port)] [#:search-path?=#t] [¶](#index-spawn)
:   Spawn a new child process executing program with the
    given arguments, a list of one or more strings (by
    convention, the first argument is typically program),
    and return its PID. Raise a `system-error` exception if
    program could not be found or could not be executed.

    If the keyword argument `#:search-path?` is true, it
    selects whether the `PATH` environment variable should be
    inspected to find program. It is true by default.

    The `#:environment` keyword parameter specifies the
    list of environment variables of the child process. It
    defaults to `(environ)`.

    The keyword arguments `#:input`, `#:output`, and
    `#:error` specify the port or file descriptor for the
    child process to use as standard input, standard output, and
    standard error. No other file descriptors are inherited
    from the parent process.

The example below shows how to spawn the `uname` program with
the `-o` option (see [uname invocation](https://www.gnu.org/software/coreutils/manual/coreutils.html#uname-invocation) in GNU
Coreutils), redirect its standard output to a pipe, and read from it:

```
(use-modules (rnrs io ports))

(let* ((input+output (pipe))
       (pid (spawn "uname" '("uname" "-o")
                    #:output (cdr input+output))))
  (close-port (cdr input+output))
  (format #t "read ~s~%" (get-string-all (car input+output)))
  (close-port (car input+output))
  (waitpid pid))

⊣ read "GNU/Linux\n"
⇒ (1234 . 0)
```

Scheme Procedure: **nice** incr [¶](#index-nice)

C Function: **scm\_nice** (incr) [¶](#index-scm_005fnice)
:   Increment the priority of the current process by incr. A higher
    priority value means that the process runs less often.
    The return value is unspecified.

Scheme Procedure: **setpriority** which who prio [¶](#index-setpriority)

C Function: **scm\_setpriority** (which, who, prio) [¶](#index-scm_005fsetpriority)
:   Set the scheduling priority of the process, process group
    or user, as indicated by which and who. which
    is one of the variables `PRIO_PROCESS`, `PRIO_PGRP`
    or `PRIO_USER`, and who is interpreted relative to
    which (a process identifier for `PRIO_PROCESS`,
    process group identifier for `PRIO_PGRP`, and a user
    identifier for `PRIO_USER`. A zero value of who
    denotes the current process, process group, or user.
    prio is a value in the range [−20,20]. The default
    priority is 0; lower priorities (in numerical terms) cause more
    favorable scheduling. Sets the priority of all of the specified
    processes. Only the super-user may lower priorities. The return
    value is not specified.

Scheme Procedure: **getpriority** which who [¶](#index-getpriority)

C Function: **scm\_getpriority** (which, who) [¶](#index-scm_005fgetpriority)
:   Return the scheduling priority of the process, process group
    or user, as indicated by which and who. which
    is one of the variables `PRIO_PROCESS`, `PRIO_PGRP`
    or `PRIO_USER`, and who should be interpreted depending on
    which (a process identifier for `PRIO_PROCESS`,
    process group identifier for `PRIO_PGRP`, and a user
    identifier for `PRIO_USER`). A zero value of who
    denotes the current process, process group, or user. Return
    the highest priority (lowest numerical value) of any of the
    specified processes.

Scheme Procedure: **getaffinity** pid [¶](#index-getaffinity)

C Function: **scm\_getaffinity** (pid) [¶](#index-scm_005fgetaffinity)
:   Return a bitvector representing the CPU affinity mask for
    process pid. Each CPU the process has affinity with
    has its corresponding bit set in the returned bitvector.
    The number of bits set is a good estimate of how many CPUs
    Guile can use without stepping on other processes’ toes.

    Currently this procedure is only defined on GNU variants
    (see [`sched_getaffinity`](/libc/latest/en/libc.html#CPU-Affinity) in The
    GNU C Library Reference Manual).

Scheme Procedure: **setaffinity** pid mask [¶](#index-setaffinity)

C Function: **scm\_setaffinity** (pid, mask) [¶](#index-scm_005fsetaffinity)
:   Install the CPU affinity mask mask, a bitvector, for
    the process or thread with ID pid. The return value
    is unspecified.

    Currently this procedure is only defined on GNU variants
    (see [`sched_setaffinity`](/libc/latest/en/libc.html#CPU-Affinity) in The
    GNU C Library Reference Manual).

See [Threads](#Threads), for information on how get the number of processors
available on a system.

---

Next: [Terminals and Ptys](#Terminals-and-Ptys), Previous: [Processes](#Processes), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.8 Signals [¶](#Signals-1)

The following procedures raise, handle and wait for signals.

Scheme code signal handlers are run via an async (see [Asynchronous Interrupts](#Asyncs)), so
they’re called in the handler’s thread at the next safe opportunity.
Generally this is after any currently executing primitive procedure
finishes (which could be a long time for primitives that wait for an
external event).

Scheme Procedure: **kill** pid sig [¶](#index-kill)

C Function: **scm\_kill** (pid, sig) [¶](#index-scm_005fkill)
:   Sends a signal to the specified process or group of processes.

    pid specifies the processes to which the signal is sent:

    pid greater than 0
    :   The process whose identifier is pid.

    pid equal to 0
    :   All processes in the current process group.

    pid less than -1
    :   The process group whose identifier is -pid

    pid equal to -1
    :   If the process is privileged, all processes except for some special
        system processes. Otherwise, all processes with the current effective
        user ID.

    sig should be specified using a variable corresponding to
    the Unix symbolic name, e.g.,

    Variable: **SIGHUP** [¶](#index-SIGHUP)
    :   Hang-up signal.

    Variable: **SIGINT** [¶](#index-SIGINT)
    :   Interrupt signal.

    A full list of signals on the GNU system may be found in [Standard
    Signals](/libc/latest/en/libc.html#Standard-Signals) in The GNU C Library Reference Manual.

Scheme Procedure: **raise** sig [¶](#index-raise)

C Function: **scm\_raise** (sig) [¶](#index-scm_005fraise)
:   Sends a specified signal sig to the current process, where
    sig is as described for the `kill` procedure.

Scheme Procedure: **sigaction** signum [handler [flags [thread]]] [¶](#index-sigaction)

C Function: **scm\_sigaction** (signum, handler, flags) [¶](#index-scm_005fsigaction)

C Function: **scm\_sigaction\_for\_thread** (signum, handler, flags, thread) [¶](#index-scm_005fsigaction_005ffor_005fthread)
:   Install or report the signal handler for a specified signal.

    signum is the signal number, which can be specified using the value
    of variables such as `SIGINT`.

    If handler is omitted, `sigaction` returns a pair: the
    CAR is the current signal handler, which will be either an
    integer with the value `SIG_DFL` (default action) or
    `SIG_IGN` (ignore), or the Scheme procedure which handles the
    signal, or `#f` if a non-Scheme procedure handles the signal.
    The CDR contains the current `sigaction` flags for the
    handler.

    If handler is provided, it is installed as the new handler for
    signum. handler can be a Scheme procedure taking one
    argument, or the value of `SIG_DFL` (default action) or
    `SIG_IGN` (ignore), or `#f` to restore whatever signal handler
    was installed before `sigaction` was first used. When a scheme
    procedure has been specified, that procedure will run in the given
    thread. When no thread has been given, the thread that made this
    call to `sigaction` is used.

    flags is a `logior` (see [Bitwise Operations](#Bitwise-Operations)) of the
    following (where provided by the system), or `0` for none.

    Variable: **SA\_NOCLDSTOP** [¶](#index-SA_005fNOCLDSTOP)
    :   By default, `SIGCHLD` is signaled when a child process stops
        (ie. receives `SIGSTOP`), and when a child process terminates.
        With the `SA_NOCLDSTOP` flag, `SIGCHLD` is only signaled
        for termination, not stopping.

        `SA_NOCLDSTOP` has no effect on signals other than
        `SIGCHLD`.

    Variable: **SA\_RESTART** [¶](#index-SA_005fRESTART)
    :   If a signal occurs while in a system call, deliver the signal then
        restart the system call (as opposed to returning an `EINTR` error
        from that call).

    Guile handles signals asynchronously. When it receives a signal, the
    synchronous signal handler just records the fact that a signal was
    received and sets a flag to tell the relevant Guile thread that it has a
    pending signal. When the Guile thread checks the pending-interrupt
    flag, it will arrange to run the asynchronous part of the signal
    handler, which is the handler attached by `sigaction`.

    This strategy has some perhaps-unexpected interactions with the
    `SA_RESTART` flag, though: because the synchronous handler doesn’t
    do very much, and notably it doesn’t run the Guile handler, it’s
    impossible to interrupt a thread stuck in a long-running system call via
    a signal handler that is installed with `SA_RESTART`: the
    synchronous handler just records the pending interrupt, but then the
    system call resumes and Guile doesn’t have a chance to actually check
    the flag and run the asynchronous handler. That’s just how it is.

    The return value is a pair with information about the old handler as
    described above.

    This interface does not provide access to the “signal blocking”
    facility. Maybe this is not needed, since the thread support may
    provide solutions to the problem of consistent access to data
    structures.

Scheme Procedure: **restore-signals** [¶](#index-restore_002dsignals)

C Function: **scm\_restore\_signals** () [¶](#index-scm_005frestore_005fsignals)
:   Return all signal handlers to the values they had before any call to
    `sigaction` was made. The return value is unspecified.

Scheme Procedure: **alarm** i [¶](#index-alarm)

C Function: **scm\_alarm** (i) [¶](#index-scm_005falarm)
:   Set a timer to raise a `SIGALRM` signal after the specified
    number of seconds (an integer). It’s advisable to install a signal
    handler for
    `SIGALRM` beforehand, since the default action is to terminate
    the process.

    The return value indicates the time remaining for the previous alarm,
    if any. The new value replaces the previous alarm. If there was
    no previous alarm, the return value is zero.

Scheme Procedure: **pause** [¶](#index-pause)

C Function: **scm\_pause** () [¶](#index-scm_005fpause)
:   Pause the current process (thread?) until a signal arrives whose
    action is to either terminate the current process or invoke a
    handler procedure. The return value is unspecified.

Scheme Procedure: **sleep** secs [¶](#index-sleep)

Scheme Procedure: **usleep** usecs [¶](#index-usleep)

C Function: **scm\_sleep** (secs) [¶](#index-scm_005fsleep)

C Function: **scm\_usleep** (usecs) [¶](#index-scm_005fusleep)
:   Wait the given period secs seconds or usecs microseconds
    (both integers). If a signal arrives the wait stops and the return
    value is the time remaining, in seconds or microseconds respectively.
    If the period elapses with no signal the return is zero.

    On most systems the process scheduler is not microsecond accurate and
    the actual period slept by `usleep` might be rounded to a system
    clock tick boundary, which might be 10 milliseconds for instance.

    See `scm_std_sleep` and `scm_std_usleep` for equivalents at
    the C level (see [Blocking in Guile Mode](#Blocking)).

Scheme Procedure: **getitimer** which\_timer [¶](#index-getitimer)

Scheme Procedure: **setitimer** which\_timer interval\_seconds interval\_microseconds value\_seconds value\_microseconds [¶](#index-setitimer)

C Function: **scm\_getitimer** (which\_timer) [¶](#index-scm_005fgetitimer)

C Function: **scm\_setitimer** (which\_timer, interval\_seconds, interval\_microseconds, value\_seconds, value\_microseconds) [¶](#index-scm_005fsetitimer)
:   Get or set the periods programmed in certain system timers.

    These timers have two settings. The first setting, the interval, is the
    value at which the timer will be reset when the current timer expires.
    The second is the current value of the timer, indicating when the next
    expiry will be signaled.

    which\_timer is one of the following values:

    Variable: **ITIMER\_REAL** [¶](#index-ITIMER_005fREAL)
    :   A real-time timer, counting down elapsed real time. At zero it raises
        `SIGALRM`. This is like `alarm` above, but with a higher
        resolution period.

    Variable: **ITIMER\_VIRTUAL** [¶](#index-ITIMER_005fVIRTUAL)
    :   A virtual-time timer, counting down while the current process is
        actually using CPU. At zero it raises `SIGVTALRM`.

    Variable: **ITIMER\_PROF** [¶](#index-ITIMER_005fPROF)
    :   A profiling timer, counting down while the process is running (like
        `ITIMER_VIRTUAL`) and also while system calls are running on the
        process’s behalf. At zero it raises a `SIGPROF`.

        This timer is intended for profiling where a program is spending its
        time (by looking where it is when the timer goes off).

    `getitimer` returns the restart timer value and its current value,
    as a list containing two pairs. Each pair is a time in seconds and
    microseconds: `((interval_secs . interval_usecs)
    (value_secs . value_usecs))`.

    `setitimer` sets the timer values similarly, in seconds and
    microseconds (which must be integers). The interval value can be zero
    to have the timer run down just once. The return value is the timer’s
    previous setting, in the same form as `getitimer` returns.

    ```
    (setitimer ITIMER_REAL
               5 500000     ;; Raise SIGALRM every 5.5 seconds
               2 0)         ;; with the first SIGALRM in 2 seconds
    ```

    Although the timers are programmed in microseconds, the actual
    accuracy might not be that high.

    Note that `ITIMER_PROF` and `ITIMER_VIRTUAL` are not
    functional on all platforms and may always error when called.
    `(provided? 'ITIMER_PROF)` and `(provided? 'ITIMER_VIRTUAL)`
    can be used to test if the those itimers are supported on the given
    host. `ITIMER_REAL` is supported on all platforms that support
    `setitimer`.

---

Next: [Pipes](#Pipes), Previous: [Signals](#Signals), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.9 Terminals and Ptys [¶](#Terminals-and-Ptys-1)

Scheme Procedure: **isatty?** port [¶](#index-isatty_003f)

C Function: **scm\_isatty\_p** (port) [¶](#index-scm_005fisatty_005fp)
:   Return `#t` if port is using a serial non–file
    device, otherwise `#f`.

Scheme Procedure: **ttyname** port [¶](#index-ttyname)

C Function: **scm\_ttyname** (port) [¶](#index-scm_005fttyname)
:   Return a string with the name of the serial terminal device
    underlying port.

Scheme Procedure: **ctermid** [¶](#index-ctermid)

C Function: **scm\_ctermid** () [¶](#index-scm_005fctermid)
:   Return a string containing the file name of the controlling
    terminal for the current process.

Scheme Procedure: **tcgetpgrp** port [¶](#index-tcgetpgrp)

C Function: **scm\_tcgetpgrp** (port) [¶](#index-scm_005ftcgetpgrp)
:   Return the process group ID of the foreground process group
    associated with the terminal open on the file descriptor
    underlying port.

    If there is no foreground process group, the return value is a
    number greater than 1 that does not match the process group ID
    of any existing process group. This can happen if all of the
    processes in the job that was formerly the foreground job have
    terminated, and no other job has yet been moved into the
    foreground.

Scheme Procedure: **tcsetpgrp** port pgid [¶](#index-tcsetpgrp)

C Function: **scm\_tcsetpgrp** (port, pgid) [¶](#index-scm_005ftcsetpgrp)
:   Set the foreground process group ID for the terminal used by the file
    descriptor underlying port to the integer pgid.
    The calling process
    must be a member of the same session as pgid and must have the same
    controlling terminal. The return value is unspecified.

---

Next: [Networking](#Networking), Previous: [Terminals and Ptys](#Terminals-and-Ptys), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.10 Pipes [¶](#Pipes-1)

The following procedures are similar to the `popen` and
`pclose` system routines. The code is in a separate “popen”
module[25](#FOOT25):

```
(use-modules (ice-9 popen))
```

Scheme Procedure: **open-pipe** command mode [¶](#index-open_002dpipe)

Scheme Procedure: **open-pipe\*** mode prog [args...] [¶](#index-open_002dpipe_002a)
:   Execute a command in a subprocess, with a pipe to it or from it, or
    with pipes in both directions.

    `open-pipe` runs the shell command using ‘`/bin/sh -c`’.
    `open-pipe*` executes prog directly, with the optional
    args arguments (all strings).

    mode should be one of the following values. `OPEN_READ` is
    an input pipe, ie. to read from the subprocess. `OPEN_WRITE`
    is an output pipe, ie. to write to it.

    Variable: **OPEN\_READ** [¶](#index-OPEN_005fREAD)

    Variable: **OPEN\_WRITE** [¶](#index-OPEN_005fWRITE)

    Variable: **OPEN\_BOTH** [¶](#index-OPEN_005fBOTH)

    For an input pipe, the child’s standard output is the pipe and
    standard input is inherited from `current-input-port`. For an
    output pipe, the child’s standard input is the pipe and standard
    output is inherited from `current-output-port`. In all cases
    the child’s standard error is inherited from
    `current-error-port` (see [Default Ports for Input, Output and Errors](#Default-Ports)).

    If those `current-X-ports` are not files of some kind, and hence
    don’t have file descriptors for the child, then `/dev/null` is
    used instead.

    Care should be taken with `OPEN_BOTH`, a deadlock will occur if
    both parent and child are writing, and waiting until the write completes
    before doing any reading. Each direction has `PIPE_BUF` bytes of
    buffering (see [Buffering](#Buffering)), which will be enough for small writes,
    but not for say putting a big file through a filter.

Scheme Procedure: **open-input-pipe** command [¶](#index-open_002dinput_002dpipe)
:   Equivalent to `open-pipe` with mode `OPEN_READ`.

    ```
    (let* ((port (open-input-pipe "date --utc"))
           (str  (read-line port))) ; from (ice-9 rdelim)
      (close-pipe port)
      str)
    ⇒ "Mon Mar 11 20:10:44 UTC 2002"
    ```

Scheme Procedure: **open-output-pipe** command [¶](#index-open_002doutput_002dpipe)
:   Equivalent to `open-pipe` with mode `OPEN_WRITE`.

    ```
    (let ((port (open-output-pipe "lpr")))
      (display "Something for the line printer.\n" port)
      (if (not (eqv? 0 (status:exit-val (close-pipe port))))
          (error "Cannot print")))
    ```

Scheme Procedure: **open-input-output-pipe** command [¶](#index-open_002dinput_002doutput_002dpipe)
:   Equivalent to `open-pipe` with mode `OPEN_BOTH`.

Scheme Procedure: **close-pipe** port [¶](#index-close_002dpipe)
:   Close a pipe created by `open-pipe`, wait for the process to
    terminate, and return the wait status code. The status is as per
    `waitpid` and can be decoded with `status:exit-val` etc
    (see [Processes](#Processes))

  

`waitpid WAIT_ANY` should not be used when pipes are open, since
it can reap a pipe’s child process, causing an error from a subsequent
`close-pipe`.

`close-port` (see [Ports](#Ports)) can close a pipe, but it doesn’t reap
the child process.

The garbage collector will close a pipe no longer in use, and reap the
child process with `waitpid`. If the child hasn’t yet terminated
the garbage collector doesn’t block, but instead checks again in the
next GC.

Many systems have per-user and system-wide limits on the number of
processes, and a system-wide limit on the number of pipes, so pipes
should be closed explicitly when no longer needed, rather than letting
the garbage collector pick them up at some later time.

Scheme Procedure: **pipeline** commands [¶](#index-pipeline)
:   Execute a pipeline of commands, where each command is a
    list of a program and its arguments as strings, returning an input
    port to the end of the pipeline, an output port to the beginning of the
    pipeline and a list of PIDs of the processes executing the commands.

    ```
    (let ((commands '(("git" "ls-files")
                      ("tar" "-cf-" "-T-")
                      ("sha1sum" "-")))
          (success? (lambda (pid)
                      (zero?
                       (status:exit-val (cdr (waitpid pid)))))))
      (receive (from to pids) (pipeline commands)
        (let* ((sha1 (read-delimited " " from))
               (index (list-index (negate success?) (reverse pids))))
          (close to)
          (close from)
          (if (not index)
              sha1
              (string-append "pipeline failed in command: "
                             (string-join (list-ref commands index)))))))
    ⇒ "52f99d234503fca8c84ef94b1005a3a28d8b3bc1"
    ```

---

Next: [System Identification](#System-Identification), Previous: [Pipes](#Pipes), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.11 Networking [¶](#Networking-1)

* [Network Address Conversion](#Network-Address-Conversion)
* [Network Databases](#Network-Databases)
* [Network Socket Address](#Network-Socket-Address)
* [Network Sockets and Communication](#Network-Sockets-and-Communication)
* [Network Socket Examples](#Internet-Socket-Examples)

---

Next: [Network Databases](#Network-Databases), Up: [Networking](#Networking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.11.1 Network Address Conversion [¶](#Network-Address-Conversion-1)

This section describes procedures which convert internet addresses
between numeric and string formats.

#### IPv4 Address Conversion [¶](#IPv4-Address-Conversion)

An IPv4 Internet address is a 4-byte value, represented in Guile as an
integer in host byte order, so that say “0.0.0.1” is 1, or
“1.0.0.0” is 16777216.

Some underlying C functions use network byte order for addresses,
Guile converts as necessary so that at the Scheme level its host byte
order everywhere.

Variable: **INADDR\_ANY** [¶](#index-INADDR_005fANY)
:   For a server, this can be used with `bind` (see [Network Sockets and Communication](#Network-Sockets-and-Communication)) to allow connections from any interface on
    the machine.

Variable: **INADDR\_BROADCAST** [¶](#index-INADDR_005fBROADCAST)
:   The broadcast address on the local network.

Variable: **INADDR\_LOOPBACK** [¶](#index-INADDR_005fLOOPBACK)
:   The address of the local host using the loopback device, ie.
    ‘`127.0.0.1`’.

Scheme Procedure: **inet-netof** address [¶](#index-inet_002dnetof)

C Function: **scm\_inet\_netof** (address) [¶](#index-scm_005finet_005fnetof)
:   Return the network number part of the given IPv4
    Internet address. E.g.,

    ```
    (inet-netof 2130706433) ⇒ 127
    ```

Scheme Procedure: **inet-lnaof** address [¶](#index-inet_002dlnaof)

C Function: **scm\_lnaof** (address) [¶](#index-scm_005flnaof)
:   Return the local-address-with-network part of the given
    IPv4 Internet address, using the obsolete class A/B/C system.
    E.g.,

    ```
    (inet-lnaof 2130706433) ⇒ 1
    ```

Scheme Procedure: **inet-makeaddr** net lna [¶](#index-inet_002dmakeaddr)

C Function: **scm\_inet\_makeaddr** (net, lna) [¶](#index-scm_005finet_005fmakeaddr)
:   Make an IPv4 Internet address by combining the network number
    net with the local-address-within-network number
    lna. E.g.,

    ```
    (inet-makeaddr 127 1) ⇒ 2130706433
    ```

#### IPv6 Address Conversion [¶](#IPv6-Address-Conversion)

An IPv6 Internet address is a 16-byte value, represented in Guile as
an integer in host byte order, so that say “::1” is 1. The following
constants are defined for convenience.

Variable: **IN6ADDR\_ANY** [¶](#index-IN6ADDR_005fANY)
:   For a server, this can be used with `bind` (see [Network Sockets and Communication](#Network-Sockets-and-Communication)) to allow connections from any IPv6 interface on the
    machine.

Variable: **IN6ADDR\_LOOPBACK** [¶](#index-IN6ADDR_005fLOOPBACK)
:   The address of the local host using the loopback device, ie.
    ‘`::1`’.

The procedures below convert an IPv6 *or* an IPv4 address to and
from its textual representation.

Scheme Procedure: **inet-ntop** family address [¶](#index-inet_002dntop)

C Function: **scm\_inet\_ntop** (family, address) [¶](#index-scm_005finet_005fntop)
:   Convert a network address from an integer to a printable string.
    family can be `AF_INET` or `AF_INET6`. E.g.,

    ```
    (inet-ntop AF_INET 2130706433) ⇒ "127.0.0.1"
    (inet-ntop AF_INET6 (- (expt 2 128) 1))
      ⇒ "ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff"
    ```

Scheme Procedure: **inet-pton** family address [¶](#index-inet_002dpton)

C Function: **scm\_inet\_pton** (family, address) [¶](#index-scm_005finet_005fpton)
:   Convert a string containing a printable network address to an integer
    address. family can be `AF_INET` or `AF_INET6`.
    E.g.,

    ```
    (inet-pton AF_INET "127.0.0.1") ⇒ 2130706433
    (inet-pton AF_INET6 "::1") ⇒ 1
    ```

---

Next: [Network Socket Address](#Network-Socket-Address), Previous: [Network Address Conversion](#Network-Address-Conversion), Up: [Networking](#Networking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.11.2 Network Databases [¶](#Network-Databases-1)

This section describes procedures which query various network databases.
Care should be taken when using the database routines since they are not
reentrant.

#### `getaddrinfo` [¶](#getaddrinfo)

The `getaddrinfo` procedure maps host and service names to socket addresses
and associated information in a protocol-independent way.

Scheme Procedure: **getaddrinfo** name service [hint\_flags [hint\_family [hint\_socktype [hint\_protocol]]]] [¶](#index-getaddrinfo)

C Function: **scm\_getaddrinfo** (name, service, hint\_flags, hint\_family, hint\_socktype, hint\_protocol) [¶](#index-scm_005fgetaddrinfo)
:   Return a list of `addrinfo` structures containing
    a socket address and associated information for host name
    and/or service to be used in creating a socket with
    which to address the specified service.

    ```
    (let* ((ai (car (getaddrinfo "www.gnu.org" "http")))
           (s  (socket (addrinfo:fam ai) (addrinfo:socktype ai)
                       (addrinfo:protocol ai))))
      (connect s (addrinfo:addr ai))
      s)
    ```

    When service is omitted or is `#f`, return
    network-level addresses for name. When name
    is `#f` service must be provided and service
    locations local to the caller are returned.

    Additional hints can be provided. When specified,
    hint\_flags should be a bitwise-or of zero or more
    constants among the following:

    `AI_PASSIVE`
    :   Socket address is intended for `bind`.

    `AI_CANONNAME`
    :   Request for canonical host name, available via
        `addrinfo:canonname`. This makes sense mainly when
        DNS lookups are involved.

    `AI_NUMERICHOST`
    :   Specifies that name is a numeric host address string
        (e.g., `"127.0.0.1"`), meaning that name resolution
        will not be used.

    `AI_NUMERICSERV`
    :   Likewise, specifies that service is a numeric port
        string (e.g., `"80"`).

    `AI_ADDRCONFIG`
    :   Return only addresses configured on the local system It is
        highly recommended to provide this flag when the returned
        socket addresses are to be used to make connections;
        otherwise, some of the returned addresses could be unreachable
        or use a protocol that is not supported.

    `AI_V4MAPPED`
    :   When looking up IPv6 addresses, return mapped IPv4 addresses if
        there is no IPv6 address available at all.

    `AI_ALL`
    :   If this flag is set along with `AI_V4MAPPED` when looking up IPv6
        addresses, return all IPv6 addresses as well as all IPv4 addresses, the latter
        mapped to IPv6 format.

    When given, hint\_family should specify the requested
    address family, e.g., `AF_INET6`. Similarly,
    hint\_socktype should specify the requested socket type
    (e.g., `SOCK_DGRAM`), and hint\_protocol should
    specify the requested protocol (its value is interpreted
    as in calls to `socket`).

    On error, an exception with key `getaddrinfo-error` is
    thrown, with an error code (an integer) as its argument:

    ```
    (catch 'getaddrinfo-error
      (lambda ()
        (getaddrinfo "www.gnu.org" "gopher"))
      (lambda (key errcode)
        (cond ((= errcode EAI_SERVICE)
    	   (display "doesn't know about Gopher!\n"))
    	  ((= errcode EAI_NONAME)
    	   (display "www.gnu.org not found\\n"))
    	  (else
    	   (format #t "something wrong: ~a\n"
    		   (gai-strerror errcode))))))
    ```

    Error codes are:

    `EAI_AGAIN`
    :   The name or service could not be resolved at this time. Future
        attempts may succeed.

    `EAI_BADFLAGS`
    :   hint\_flags contains an invalid value.

    `EAI_FAIL`
    :   A non-recoverable error occurred when attempting to
        resolve the name.

    `EAI_FAMILY`
    :   hint\_family was not recognized.

    `EAI_NONAME`
    :   Either name does not resolve for the supplied parameters,
        or neither name nor service were supplied.

    `EAI_NODATA`
    :   This non-POSIX error code can be returned on some systems (GNU
        and Darwin, at least), for example when name is known
        but requests that were made turned out no data. Error handling
        code should be prepared to handle it when it is defined.

    `EAI_SERVICE`
    :   service was not recognized for the specified socket type.

    `EAI_SOCKTYPE`
    :   hint\_socktype was not recognized.

    `EAI_SYSTEM`
    :   A system error occurred. In C, the error code can be found in
        `errno`; this value is not accessible from Scheme, but in
        practice it provides little information about the actual error
        cause.

    Users are encouraged to read the
    ["POSIX specification](http://www.opengroup.org/onlinepubs/9699919799/functions/getaddrinfo.html) for more details.

The following procedures take an `addrinfo` object as returned by
`getaddrinfo`:

Scheme Procedure: **addrinfo:flags** ai [¶](#index-addrinfo_003aflags)
:   Return flags for ai as a bitwise or of `AI_` values (see above).

Scheme Procedure: **addrinfo:fam** ai [¶](#index-addrinfo_003afam)
:   Return the address family of ai (a `AF_` value).

Scheme Procedure: **addrinfo:socktype** ai [¶](#index-addrinfo_003asocktype)
:   Return the socket type for ai (a `SOCK_` value).

Scheme Procedure: **addrinfo:protocol** ai [¶](#index-addrinfo_003aprotocol)
:   Return the protocol of ai.

Scheme Procedure: **addrinfo:addr** ai [¶](#index-addrinfo_003aaddr)
:   Return the socket address associated with ai as a `sockaddr`
    object (see [Network Socket Address](#Network-Socket-Address)).

Scheme Procedure: **addrinfo:canonname** ai [¶](#index-addrinfo_003acanonname)
:   Return a string for the canonical name associated with ai if
    the `AI_CANONNAME` flag was supplied.

#### The Host Database [¶](#The-Host-Database)

A *host object* is a structure that represents what is known about a
network host, and is the usual way of representing a system’s network
identity inside software.

The following functions accept a host object and return a selected
component:

Scheme Procedure: **hostent:name** host [¶](#index-hostent_003aname)
:   The “official” hostname for host.

Scheme Procedure: **hostent:aliases** host [¶](#index-hostent_003aaliases)
:   A list of aliases for host.

Scheme Procedure: **hostent:addrtype** host [¶](#index-hostent_003aaddrtype)
:   The host address type, one of the `AF` constants, such as
    `AF_INET` or `AF_INET6`.

Scheme Procedure: **hostent:length** host [¶](#index-hostent_003alength)
:   The length of each address for host, in bytes.

Scheme Procedure: **hostent:addr-list** host [¶](#index-hostent_003aaddr_002dlist)
:   The list of network addresses associated with host. For
    `AF_INET` these are integer IPv4 address (see [Network Address Conversion](#Network-Address-Conversion)).

The following procedures can be used to search the host database. However,
`getaddrinfo` should be preferred over them since it’s more generic and
thread-safe.

Scheme Procedure: **gethost** [host] [¶](#index-gethost)

Scheme Procedure: **gethostbyname** hostname [¶](#index-gethostbyname)

Scheme Procedure: **gethostbyaddr** address [¶](#index-gethostbyaddr)

C Function: **scm\_gethost** (host) [¶](#index-scm_005fgethost)
:   Look up a host by name or address, returning a host object. The
    `gethost` procedure will accept either a string name or an integer
    address; if given no arguments, it behaves like `gethostent` (see
    below). If a name or address is supplied but the address can not be
    found, an error will be thrown to one of the keys:
    `host-not-found`, `try-again`, `no-recovery` or
    `no-data`, corresponding to the equivalent `h_error` values.
    Unusual conditions may result in errors thrown to the
    `system-error` or `misc_error` keys.

    ```
    (gethost "www.gnu.org")
    ⇒ #("www.gnu.org" () 2 4 (3353880842))

    (gethostbyname "www.emacs.org")
    ⇒ #("emacs.org" ("www.emacs.org") 2 4 (1073448978))
    ```

The following procedures may be used to step through the host
database from beginning to end.

Scheme Procedure: **sethostent** [stayopen] [¶](#index-sethostent)
:   Initialize an internal stream from which host objects may be read. This
    procedure must be called before any calls to `gethostent`, and may
    also be called afterward to reset the host entry stream. If
    stayopen is supplied and is not `#f`, the database is not
    closed by subsequent `gethostbyname` or `gethostbyaddr` calls,
    possibly giving an efficiency gain.

Scheme Procedure: **gethostent** [¶](#index-gethostent)
:   Return the next host object from the host database, or `#f` if
    there are no more hosts to be found (or an error has been encountered).
    This procedure may not be used before `sethostent` has been called.

Scheme Procedure: **endhostent** [¶](#index-endhostent)
:   Close the stream used by `gethostent`. The return value is unspecified.

Scheme Procedure: **sethost** [stayopen] [¶](#index-sethost)

C Function: **scm\_sethost** (stayopen) [¶](#index-scm_005fsethost)
:   If stayopen is omitted, this is equivalent to `endhostent`.
    Otherwise it is equivalent to `sethostent stayopen`.

#### The Network Database [¶](#The-Network-Database)

The following functions accept an object representing a network
and return a selected component:

Scheme Procedure: **netent:name** net [¶](#index-netent_003aname)
:   The “official” network name.

Scheme Procedure: **netent:aliases** net [¶](#index-netent_003aaliases)
:   A list of aliases for the network.

Scheme Procedure: **netent:addrtype** net [¶](#index-netent_003aaddrtype)
:   The type of the network number. Currently, this returns only
    `AF_INET`.

Scheme Procedure: **netent:net** net [¶](#index-netent_003anet)
:   The network number.

The following procedures are used to search the network database:

Scheme Procedure: **getnet** [net] [¶](#index-getnet)

Scheme Procedure: **getnetbyname** net-name [¶](#index-getnetbyname)

Scheme Procedure: **getnetbyaddr** net-number [¶](#index-getnetbyaddr)

C Function: **scm\_getnet** (net) [¶](#index-scm_005fgetnet)
:   Look up a network by name or net number in the network database. The
    net-name argument must be a string, and the net-number
    argument must be an integer. `getnet` will accept either type of
    argument, behaving like `getnetent` (see below) if no arguments are
    given.

The following procedures may be used to step through the network
database from beginning to end.

Scheme Procedure: **setnetent** [stayopen] [¶](#index-setnetent)
:   Initialize an internal stream from which network objects may be read. This
    procedure must be called before any calls to `getnetent`, and may
    also be called afterward to reset the net entry stream. If
    stayopen is supplied and is not `#f`, the database is not
    closed by subsequent `getnetbyname` or `getnetbyaddr` calls,
    possibly giving an efficiency gain.

Scheme Procedure: **getnetent** [¶](#index-getnetent)
:   Return the next entry from the network database.

Scheme Procedure: **endnetent** [¶](#index-endnetent)
:   Close the stream used by `getnetent`. The return value is unspecified.

Scheme Procedure: **setnet** [stayopen] [¶](#index-setnet)

C Function: **scm\_setnet** (stayopen) [¶](#index-scm_005fsetnet)
:   If stayopen is omitted, this is equivalent to `endnetent`.
    Otherwise it is equivalent to `setnetent stayopen`.

#### The Protocol Database [¶](#The-Protocol-Database)

The following functions accept an object representing a protocol
and return a selected component:

Scheme Procedure: **protoent:name** protocol [¶](#index-protoent_003aname)
:   The “official” protocol name.

Scheme Procedure: **protoent:aliases** protocol [¶](#index-protoent_003aaliases)
:   A list of aliases for the protocol.

Scheme Procedure: **protoent:proto** protocol [¶](#index-protoent_003aproto)
:   The protocol number.

The following procedures are used to search the protocol database:

Scheme Procedure: **getproto** [protocol] [¶](#index-getproto)

Scheme Procedure: **getprotobyname** name [¶](#index-getprotobyname)

Scheme Procedure: **getprotobynumber** number [¶](#index-getprotobynumber)

C Function: **scm\_getproto** (protocol) [¶](#index-scm_005fgetproto)
:   Look up a network protocol by name or by number. `getprotobyname`
    takes a string argument, and `getprotobynumber` takes an integer
    argument. `getproto` will accept either type, behaving like
    `getprotoent` (see below) if no arguments are supplied.

The following procedures may be used to step through the protocol
database from beginning to end.

Scheme Procedure: **setprotoent** [stayopen] [¶](#index-setprotoent)
:   Initialize an internal stream from which protocol objects may be read. This
    procedure must be called before any calls to `getprotoent`, and may
    also be called afterward to reset the protocol entry stream. If
    stayopen is supplied and is not `#f`, the database is not
    closed by subsequent `getprotobyname` or `getprotobynumber` calls,
    possibly giving an efficiency gain.

Scheme Procedure: **getprotoent** [¶](#index-getprotoent)
:   Return the next entry from the protocol database.

Scheme Procedure: **endprotoent** [¶](#index-endprotoent)
:   Close the stream used by `getprotoent`. The return value is unspecified.

Scheme Procedure: **setproto** [stayopen] [¶](#index-setproto)

C Function: **scm\_setproto** (stayopen) [¶](#index-scm_005fsetproto)
:   If stayopen is omitted, this is equivalent to `endprotoent`.
    Otherwise it is equivalent to `setprotoent stayopen`.

#### The Service Database [¶](#The-Service-Database)

The following functions accept an object representing a service
and return a selected component:

Scheme Procedure: **servent:name** serv [¶](#index-servent_003aname)
:   The “official” name of the network service.

Scheme Procedure: **servent:aliases** serv [¶](#index-servent_003aaliases)
:   A list of aliases for the network service.

Scheme Procedure: **servent:port** serv [¶](#index-servent_003aport)
:   The Internet port used by the service.

Scheme Procedure: **servent:proto** serv [¶](#index-servent_003aproto)
:   The protocol used by the service. A service may be listed many times
    in the database under different protocol names.

The following procedures are used to search the service database:

Scheme Procedure: **getserv** [name [protocol]] [¶](#index-getserv)

Scheme Procedure: **getservbyname** name protocol [¶](#index-getservbyname)

Scheme Procedure: **getservbyport** port protocol [¶](#index-getservbyport)

C Function: **scm\_getserv** (name, protocol) [¶](#index-scm_005fgetserv)
:   Look up a network service by name or by service number, and return a
    network service object. The protocol argument specifies the name
    of the desired protocol; if the protocol found in the network service
    database does not match this name, a system error is signaled.

    The `getserv` procedure will take either a service name or number
    as its first argument; if given no arguments, it behaves like
    `getservent` (see below).

    ```
    (getserv "imap" "tcp")
    ⇒ #("imap2" ("imap") 143 "tcp")

    (getservbyport 88 "udp")
    ⇒ #("kerberos" ("kerberos5" "krb5") 88 "udp")
    ```

The following procedures may be used to step through the service
database from beginning to end.

Scheme Procedure: **setservent** [stayopen] [¶](#index-setservent)
:   Initialize an internal stream from which service objects may be read. This
    procedure must be called before any calls to `getservent`, and may
    also be called afterward to reset the service entry stream. If
    stayopen is supplied and is not `#f`, the database is not
    closed by subsequent `getservbyname` or `getservbyport` calls,
    possibly giving an efficiency gain.

Scheme Procedure: **getservent** [¶](#index-getservent)
:   Return the next entry from the services database.

Scheme Procedure: **endservent** [¶](#index-endservent)
:   Close the stream used by `getservent`. The return value is unspecified.

Scheme Procedure: **setserv** [stayopen] [¶](#index-setserv)

C Function: **scm\_setserv** (stayopen) [¶](#index-scm_005fsetserv)
:   If stayopen is omitted, this is equivalent to `endservent`.
    Otherwise it is equivalent to `setservent stayopen`.

---

Next: [Network Sockets and Communication](#Network-Sockets-and-Communication), Previous: [Network Databases](#Network-Databases), Up: [Networking](#Networking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.11.3 Network Socket Address [¶](#Network-Socket-Address-1)

A *socket address* object identifies a socket endpoint for
communication. In the case of `AF_INET` for instance, the socket
address object comprises the host address (or interface on the host)
and a port number which specifies a particular open socket in a
running client or server process. A socket address object can be
created with,

Scheme Procedure: **make-socket-address** AF\_INET ipv4addr port [¶](#index-make_002dsocket_002daddress)

Scheme Procedure: **make-socket-address** AF\_INET6 ipv6addr port [flowinfo [scopeid]] [¶](#index-make_002dsocket_002daddress-1)

Scheme Procedure: **make-socket-address** AF\_UNIX path [¶](#index-make_002dsocket_002daddress-2)

C Function: **scm\_make\_socket\_address** (family, address, arglist) [¶](#index-scm_005fmake_005fsocket_005faddress)
:   Return a new socket address object. The first argument is the address
    family, one of the `AF` constants, then the arguments vary
    according to the family.

    For `AF_INET` the arguments are an IPv4 network address number
    (see [Network Address Conversion](#Network-Address-Conversion)), and a port number.

    For `AF_INET6` the arguments are an IPv6 network address number
    and a port number. Optional flowinfo and scopeid
    arguments may be given (both integers, default 0).

    For `AF_UNIX` the argument is a filename (a string).

    The C function `scm_make_socket_address` takes the family
    and address arguments directly, then arglist is a list of
    further arguments, being the port for IPv4, port and optional flowinfo
    and scopeid for IPv6, or the empty list `SCM_EOL` for Unix
    domain.

The following functions access the fields of a socket address object,

Scheme Procedure: **sockaddr:fam** sa [¶](#index-sockaddr_003afam)
:   Return the address family from socket address object sa. This
    is one of the `AF` constants (e.g. `AF_INET`).

Scheme Procedure: **sockaddr:path** sa [¶](#index-sockaddr_003apath)
:   For an `AF_UNIX` socket address object sa, return the
    filename.

Scheme Procedure: **sockaddr:addr** sa [¶](#index-sockaddr_003aaddr)
:   For an `AF_INET` or `AF_INET6` socket address object
    sa, return the network address number.

Scheme Procedure: **sockaddr:port** sa [¶](#index-sockaddr_003aport)
:   For an `AF_INET` or `AF_INET6` socket address object
    sa, return the port number.

Scheme Procedure: **sockaddr:flowinfo** sa [¶](#index-sockaddr_003aflowinfo)
:   For an `AF_INET6` socket address object sa, return the
    flowinfo value.

Scheme Procedure: **sockaddr:scopeid** sa [¶](#index-sockaddr_003ascopeid)
:   For an `AF_INET6` socket address object sa, return the
    scope ID value.

The functions below convert to and from the C `struct sockaddr`
(see [Address Formats](/libc/latest/en/libc.html#Address-Formats) in The GNU C Library Reference Manual).
That structure is a generic type, an application can cast to or from
`struct sockaddr_in`, `struct sockaddr_in6` or `struct
sockaddr_un` according to the address family.

In a `struct sockaddr` taken or returned, the byte ordering in
the fields follows the C conventions (see [Byte Order
Conversion](/libc/latest/en/libc.html#Byte-Order) in The GNU C Library Reference Manual). This means
network byte order for `AF_INET` host address
(`sin_addr.s_addr`) and port number (`sin_port`), and
`AF_INET6` port number (`sin6_port`). But at the Scheme
level these values are taken or returned in host byte order, so the
port is an ordinary integer, and the host address likewise is an
ordinary integer (as described in [Network Address Conversion](#Network-Address-Conversion)).

C Function: `struct sockaddr *` **scm\_c\_make\_socket\_address** `(SCM family, SCM address, SCM args, size_t *outsize)` [¶](#index-scm_005fc_005fmake_005fsocket_005faddress)
:   Return a newly-`malloc`ed `struct sockaddr` created from
    arguments like those taken by `scm_make_socket_address` above.

    The size (in bytes) of the `struct sockaddr` return is stored
    into `*outsize`. An application must call `free` to
    release the returned structure when no longer required.

C Function: `SCM` **scm\_from\_sockaddr** `(const struct sockaddr *address, unsigned address_size)` [¶](#index-scm_005ffrom_005fsockaddr)
:   Return a Scheme socket address object from the C address
    structure. address\_size is the size in bytes of address.

C Function: `struct sockaddr *` **scm\_to\_sockaddr** `(SCM address, size_t *address_size)` [¶](#index-scm_005fto_005fsockaddr)
:   Return a newly-`malloc`ed `struct sockaddr` from a Scheme
    level socket address object.

    The size (in bytes) of the `struct sockaddr` return is stored
    into `*outsize`. An application must call `free` to
    release the returned structure when no longer required.

---

Next: [Network Socket Examples](#Internet-Socket-Examples), Previous: [Network Socket Address](#Network-Socket-Address), Up: [Networking](#Networking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.11.4 Network Sockets and Communication [¶](#Network-Sockets-and-Communication-1)

Socket ports can be created using `socket` and `socketpair`.
The ports are initially unbuffered, to make reading and writing to the
same port more reliable. A buffer can be added to the port using
`setvbuf` (see [Buffering](#Buffering)).

Most systems have limits on how many files and sockets can be open, so
it’s strongly recommended that socket ports be closed explicitly when
no longer required (see [Ports](#Ports)).

Some of the underlying C functions take values in network byte order,
but the convention in Guile is that at the Scheme level everything is
ordinary host byte order and conversions are made automatically where
necessary.

Scheme Procedure: **socket** family style proto [¶](#index-socket-1)

C Function: **scm\_socket** (family, style, proto) [¶](#index-scm_005fsocket)
:   Return a new socket port of the type specified by family,
    style and proto. All three parameters are integers. The
    possible values for family are as follows, where supported by
    the system,

    Variable: **PF\_UNIX** [¶](#index-PF_005fUNIX)

    Variable: **PF\_INET** [¶](#index-PF_005fINET)

    Variable: **PF\_INET6** [¶](#index-PF_005fINET6)

    The possible values for style are as follows, again where
    supported by the system,

    Variable: **SOCK\_STREAM** [¶](#index-SOCK_005fSTREAM)

    Variable: **SOCK\_DGRAM** [¶](#index-SOCK_005fDGRAM)

    Variable: **SOCK\_RAW** [¶](#index-SOCK_005fRAW)

    Variable: **SOCK\_RDM** [¶](#index-SOCK_005fRDM)

    Variable: **SOCK\_SEQPACKET** [¶](#index-SOCK_005fSEQPACKET)

    proto can be obtained from a protocol name using
    `getprotobyname` (see [Network Databases](#Network-Databases)). A value of zero
    means the default protocol, which is usually right.

    A socket cannot by used for communication until it has been connected
    somewhere, usually with either `connect` or `accept` below.

Scheme Procedure: **socketpair** family style proto [¶](#index-socketpair)

C Function: **scm\_socketpair** (family, style, proto) [¶](#index-scm_005fsocketpair)
:   Return a pair, the `car` and `cdr` of which are two unnamed
    socket ports connected to each other. The connection is full-duplex,
    so data can be transferred in either direction between the two.

    family, style and proto are as per `socket`
    above. But many systems only support socket pairs in the
    `PF_UNIX` family. Zero is likely to be the only meaningful value
    for proto.

Scheme Procedure: **getsockopt** sock level optname [¶](#index-getsockopt)

Scheme Procedure: **setsockopt** sock level optname value [¶](#index-setsockopt)

C Function: **scm\_getsockopt** (sock, level, optname) [¶](#index-scm_005fgetsockopt)

C Function: **scm\_setsockopt** (sock, level, optname, value) [¶](#index-scm_005fsetsockopt)
:   Get or set an option on socket port sock. `getsockopt`
    returns the current value. `setsockopt` sets a value and the
    return is unspecified.

    level is an integer specifying a protocol layer, either
    `SOL_SOCKET` for socket level options, or a protocol number from
    the `IPPROTO` constants or `getprotoent` (see [Network Databases](#Network-Databases)).

    Variable: **SOL\_SOCKET** [¶](#index-SOL_005fSOCKET)

    Variable: **IPPROTO\_IP** [¶](#index-IPPROTO_005fIP)

    Variable: **IPPROTO\_IPV6** [¶](#index-IPPROTO_005fIPV6)

    Variable: **IPPROTO\_TCP** [¶](#index-IPPROTO_005fTCP)

    Variable: **IPPROTO\_UDP** [¶](#index-IPPROTO_005fUDP)

    optname is an integer specifying an option within the protocol
    layer.

    For `SOL_SOCKET` level the following optnames are defined
    (when provided by the system). For their meaning see
    [Socket-Level Options](/libc/latest/en/libc.html#Socket_002dLevel-Options) in The GNU C Library Reference
    Manual, or `man 7 socket`.

    Variable: **SO\_DEBUG** [¶](#index-SO_005fDEBUG)

    Variable: **SO\_REUSEADDR** [¶](#index-SO_005fREUSEADDR)

    Variable: **SO\_STYLE** [¶](#index-SO_005fSTYLE)

    Variable: **SO\_TYPE** [¶](#index-SO_005fTYPE)

    Variable: **SO\_ERROR** [¶](#index-SO_005fERROR)

    Variable: **SO\_DONTROUTE** [¶](#index-SO_005fDONTROUTE)

    Variable: **SO\_BROADCAST** [¶](#index-SO_005fBROADCAST)

    Variable: **SO\_SNDBUF** [¶](#index-SO_005fSNDBUF)

    Variable: **SO\_RCVBUF** [¶](#index-SO_005fRCVBUF)

    Variable: **SO\_KEEPALIVE** [¶](#index-SO_005fKEEPALIVE)

    Variable: **SO\_OOBINLINE** [¶](#index-SO_005fOOBINLINE)

    Variable: **SO\_NO\_CHECK** [¶](#index-SO_005fNO_005fCHECK)

    Variable: **SO\_PRIORITY** [¶](#index-SO_005fPRIORITY)

    Variable: **SO\_REUSEPORT** [¶](#index-SO_005fREUSEPORT)

    Variable: **SO\_RCVTIMEO** [¶](#index-SO_005fRCVTIMEO)

    Variable: **SO\_SNDTIMEO** [¶](#index-SO_005fSNDTIMEO)
    :   The value taken or returned is an integer.

    Variable: **SO\_LINGER** [¶](#index-SO_005fLINGER)
    :   The value taken or returned is a pair of integers
        `(ENABLE . TIMEOUT)`. On old systems without timeout
        support (ie. without `struct linger`), only ENABLE has an
        effect but the value in Guile is always a pair.

    For IP level (`IPPROTO_IP`) the following optnames are
    defined (when provided by the system). See `man ip` for what
    they mean.

    Variable: **IP\_MULTICAST\_IF** [¶](#index-IP_005fMULTICAST_005fIF)
    :   This sets the source interface used by multicast traffic.

    Variable: **IP\_MULTICAST\_TTL** [¶](#index-IP_005fMULTICAST_005fTTL)
    :   This sets the default TTL for multicast traffic. This defaults
        to 1 and should be increased to allow traffic to pass beyond the
        local network.

    Variable: **IP\_ADD\_MEMBERSHIP** [¶](#index-IP_005fADD_005fMEMBERSHIP)

    Variable: **IP\_DROP\_MEMBERSHIP** [¶](#index-IP_005fDROP_005fMEMBERSHIP)
    :   These can be used only with `setsockopt`, not `getsockopt`.
        value is a pair `(MULTIADDR . INTERFACEADDR)`
        of integer IPv4 addresses (see [Network Address Conversion](#Network-Address-Conversion)).
        MULTIADDR is a multicast address to be added to or dropped from
        the interface INTERFACEADDR. INTERFACEADDR can be
        `INADDR_ANY` to have the system select the interface.
        INTERFACEADDR can also be an interface index number, on systems
        supporting that.

    Last, for IPv6 level (`IPPROTO_IPV6`), the following optnames
    are defined. See `man 7 ipv6` for details.

    Variable: **IPV6\_V6ONLY** [¶](#index-IPV6_005fV6ONLY)
    :   Determines whether an `AF_INET6` socket is restricted to
        transmitting IPv6 packets only, or whether it can also transmit packets
        for an IPv4-mapped IPv6 address.

For `IPPROTO_TCP` level the following optnames are defined
(when provided by the system). For their meaning see `man 7
tcp`.

Variable: **TCP\_NODELAY** [¶](#index-TCP_005fNODELAY)

Variable: **TCP\_CORK** [¶](#index-TCP_005fCORK)
:   The value taken or returned is an integer.

Scheme Procedure: **shutdown** sock how [¶](#index-shutdown)

C Function: **scm\_shutdown** (sock, how) [¶](#index-scm_005fshutdown)
:   Sockets can be closed simply by using `close-port`. The
    `shutdown` procedure allows reception or transmission on a
    connection to be shut down individually, according to the parameter
    how:

    0
    :   Stop receiving data for this socket. If further data arrives, reject it.

    1
    :   Stop trying to transmit data from this socket. Discard any
        data waiting to be sent. Stop looking for acknowledgement of
        data already sent; don’t retransmit it if it is lost.

    2
    :   Stop both reception and transmission.

    The return value is unspecified.

Scheme Procedure: **connect** sock sockaddr [¶](#index-connect)

Scheme Procedure: **connect** sock AF\_INET ipv4addr port [¶](#index-connect-1)

Scheme Procedure: **connect** sock AF\_INET6 ipv6addr port [flowinfo [scopeid]] [¶](#index-connect-2)

Scheme Procedure: **connect** sock AF\_UNIX path [¶](#index-connect-3)

C Function: **scm\_connect** (sock, fam, address, args) [¶](#index-scm_005fconnect)
:   Initiate a connection on socket port sock to a given address. The
    destination is either a socket address object, or arguments the same as
    `make-socket-address` would take to make such an object
    (see [Network Socket Address](#Network-Socket-Address)). Return true unless the socket was
    configured as non-blocking and the connection could not be made
    immediately.

    ```
    (connect sock AF_INET INADDR_LOOPBACK 23)
    (connect sock (make-socket-address AF_INET INADDR_LOOPBACK 23))
    ```

Scheme Procedure: **bind** sock sockaddr [¶](#index-bind)

Scheme Procedure: **bind** sock AF\_INET ipv4addr port [¶](#index-bind-1)

Scheme Procedure: **bind** sock AF\_INET6 ipv6addr port [flowinfo [scopeid]] [¶](#index-bind-2)

Scheme Procedure: **bind** sock AF\_UNIX path [¶](#index-bind-3)

C Function: **scm\_bind** (sock, fam, address, args) [¶](#index-scm_005fbind)
:   Bind socket port sock to the given address. The address is
    either a socket address object, or arguments the same as
    `make-socket-address` would take to make such an object
    (see [Network Socket Address](#Network-Socket-Address)). The return value is unspecified.

    Generally a socket is only explicitly bound to a particular address
    when making a server, i.e. to listen on a particular port. For an
    outgoing connection the system will assign a local address
    automatically, if not already bound.

    ```
    (bind sock AF_INET INADDR_ANY 12345)
    (bind sock (make-socket-address AF_INET INADDR_ANY 12345))
    ```

Scheme Procedure: **listen** sock backlog [¶](#index-listen)

C Function: **scm\_listen** (sock, backlog) [¶](#index-scm_005flisten)
:   Enable sock to accept connection
    requests. backlog is an integer specifying
    the maximum length of the queue for pending connections.
    If the queue fills, new clients will fail to connect until
    the server calls `accept` to accept a connection from
    the queue.

    The return value is unspecified.

Scheme Procedure: **accept** sock [flags] [¶](#index-accept)

C Function: **scm\_accept** (sock) [¶](#index-scm_005faccept)
:   Accept a connection from socket port sock which has been enabled
    for listening with `listen` above.

    If there are no incoming connections in the queue, there are two
    possible behaviors, depending on whether sock has been configured
    for non-blocking operation or not:

    * If there is no connection waiting and the socket was set to non-blocking
      mode with the `O_NONBLOCK` port option (see [`fcntl`](#Ports-and-File-Descriptors)), return `#f` directly.
    * Otherwise wait until a connection is available.

    The return value is a pair. The `car` is a new socket port,
    connected and ready to communicate. The `cdr` is a socket address
    object (see [Network Socket Address](#Network-Socket-Address)) which is where the remote
    connection is from (like `getpeername` below).

    flags, if given, may include `SOCK_CLOEXEC` or
    `SOCK_NONBLOCK`, which like `O_CLOEXEC` and `O_NONBLOCK`
    apply to the newly accepted socket.

    All communication takes place using the new socket returned. The
    given sock remains bound and listening, and `accept` may be
    called on it again to get another incoming connection when desired.

Scheme Procedure: **getsockname** sock [¶](#index-getsockname)

C Function: **scm\_getsockname** (sock) [¶](#index-scm_005fgetsockname)
:   Return a socket address object which is the where sock is bound
    locally. sock may have obtained its local address from
    `bind` (above), or if a `connect` is done with an otherwise
    unbound socket (which is usual) then the system will have assigned an
    address.

    Note that on many systems the address of a socket in the
    `AF_UNIX` namespace cannot be read.

Scheme Procedure: **getpeername** sock [¶](#index-getpeername)

C Function: **scm\_getpeername** (sock) [¶](#index-scm_005fgetpeername)
:   Return a socket address object which is where sock is connected
    to, i.e. the remote endpoint.

    Note that on many systems the address of a socket in the
    `AF_UNIX` namespace cannot be read.

Scheme Procedure: **recv!** sock buf [flags] [¶](#index-recv_0021)

C Function: **scm\_recv** (sock, buf, flags) [¶](#index-scm_005frecv)
:   Receive data from a socket port.
    sock must already
    be bound to the address from which data is to be received.
    buf is a bytevector into which
    the data will be written. The size of buf limits
    the amount of
    data which can be received: in the case of packet
    protocols, if a packet larger than this limit is encountered
    then some data
    will be irrevocably lost.

    The optional flags argument is a value or bitwise OR of
    `MSG_OOB`, `MSG_PEEK`, `MSG_DONTROUTE` etc.

    The value returned is the number of bytes read from the
    socket.

    Note that the data is read directly from the socket file
    descriptor:
    any unread buffered port data is ignored.

Scheme Procedure: **send** sock message [flags] [¶](#index-send)

C Function: **scm\_send** (sock, message, flags) [¶](#index-scm_005fsend)
:   Transmit bytevector message on socket port sock.
    sock must already be bound to a destination address. The value
    returned is the number of bytes transmitted—it’s possible for this
    to be less than the length of message if the socket is set to be
    non-blocking. The optional flags argument is a value or bitwise
    OR of `MSG_OOB`, `MSG_PEEK`, `MSG_DONTROUTE` etc.

    Note that the data is written directly to the socket
    file descriptor:
    any unflushed buffered port data is ignored.

Scheme Procedure: **recvfrom!** sock buf [flags [start [end]]] [¶](#index-recvfrom_0021)

C Function: **scm\_recvfrom** (sock, buf, flags, start, end) [¶](#index-scm_005frecvfrom)
:   Receive data from socket port sock, returning the originating
    address as well as the data. This function is usually for datagram
    sockets, but can be used on stream-oriented sockets too.

    The data received is stored in bytevector buf, using
    either the whole bytevector or just the region between the optional
    start and end positions. The size of buf
    limits the amount of data that can be received. For datagram
    protocols if a packet larger than this is received then excess
    bytes are irrevocably lost.

    The return value is a pair. The `car` is the number of bytes
    read. The `cdr` is a socket address object (see [Network Socket Address](#Network-Socket-Address)) which is where the data came from, or `#f` if
    the origin is unknown.

    The optional flags argument is a or bitwise-OR (`logior`)
    of `MSG_OOB`, `MSG_PEEK`, `MSG_DONTROUTE` etc.

    Data is read directly from the socket file descriptor, any buffered
    port data is ignored.

    On a GNU/Linux system `recvfrom!` is not multi-threading, all
    threads stop while a `recvfrom!` call is in progress. An
    application may need to use `select`, `O_NONBLOCK` or
    `MSG_DONTWAIT` to avoid this.

Scheme Procedure: **sendto** sock message sockaddr [flags] [¶](#index-sendto)

Scheme Procedure: **sendto** sock message AF\_INET ipv4addr port [flags] [¶](#index-sendto-1)

Scheme Procedure: **sendto** sock message AF\_INET6 ipv6addr port [flowinfo [scopeid [flags]]] [¶](#index-sendto-2)

Scheme Procedure: **sendto** sock message AF\_UNIX path [flags] [¶](#index-sendto-3)

C Function: **scm\_sendto** (sock, message, fam, address, args\_and\_flags) [¶](#index-scm_005fsendto)
:   Transmit bytevector message as a datagram socket port
    sock. The destination is specified either as a socket address
    object, or as arguments the same as would be taken by
    `make-socket-address` to create such an object (see [Network Socket Address](#Network-Socket-Address)).

    The destination address may be followed by an optional flags
    argument which is a `logior` (see [Bitwise Operations](#Bitwise-Operations)) of
    `MSG_OOB`, `MSG_PEEK`, `MSG_DONTROUTE` etc.

    The value returned is the number of bytes transmitted –
    it’s possible for
    this to be less than the length of message if the
    socket is
    set to be non-blocking.
    Note that the data is written directly to the socket
    file descriptor:
    any unflushed buffered port data is ignored.

---

Previous: [Network Sockets and Communication](#Network-Sockets-and-Communication), Up: [Networking](#Networking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.11.5 Network Socket Examples [¶](#Network-Socket-Examples)

The following give examples of how to use network sockets.

#### Internet Socket Client Example [¶](#Internet-Socket-Client-Example)

The following example demonstrates an Internet socket client.
It connects to the HTTP daemon running on the local machine and
returns the contents of the root index URL.

```
(let ((s (socket PF_INET SOCK_STREAM 0)))
  (connect s AF_INET (inet-pton AF_INET "127.0.0.1") 80)
  (display "GET / HTTP/1.0\r\n\r\n" s)

  (do ((line (read-line s) (read-line s)))
      ((eof-object? line))
    (display line)
    (newline)))
```

#### Internet Socket Server Example [¶](#Internet-Socket-Server-Example)

The following example shows a simple Internet server which listens on
port 2904 for incoming connections and sends a greeting back to the
client.

```
(let ((s (socket PF_INET SOCK_STREAM 0)))
  (setsockopt s SOL_SOCKET SO_REUSEADDR 1)
  ;; Specific address?
  ;; (bind s AF_INET (inet-pton AF_INET "127.0.0.1") 2904)
  (bind s AF_INET INADDR_ANY 2904)
  (listen s 5)

  (simple-format #t "Listening for clients in pid: ~S" (getpid))
  (newline)

  (while #t
    (let* ((client-connection (accept s))
           (client-details (cdr client-connection))
           (client (car client-connection)))
      (simple-format #t "Got new client connection: ~S"
                     client-details)
      (newline)
      (simple-format #t "Client address: ~S"
                     (gethostbyaddr
                      (sockaddr:addr client-details)))
      (newline)
      ;; Send back the greeting to the client port
      (display "Hello client\r\n" client)
      (close client))))
```

---

Next: [Locales](#Locales), Previous: [Networking](#Networking), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.12 System Identification [¶](#System-Identification-1)

This section lists the various procedures Guile provides for accessing
information about the system it runs on.

Scheme Procedure: **uname** [¶](#index-uname)

C Function: **scm\_uname** () [¶](#index-scm_005funame)
:   Return an object with some information about the computer
    system the program is running on.

    The following procedures accept an object as returned by `uname`
    and return a selected component (all of which are strings).

    Scheme Procedure: **utsname:sysname** un [¶](#index-utsname_003asysname)
    :   The name of the operating system.

    Scheme Procedure: **utsname:nodename** un [¶](#index-utsname_003anodename)
    :   The network name of the computer.

    Scheme Procedure: **utsname:release** un [¶](#index-utsname_003arelease)
    :   The current release level of the operating system implementation.

    Scheme Procedure: **utsname:version** un [¶](#index-utsname_003aversion)
    :   The current version level within the release of the operating system.

    Scheme Procedure: **utsname:machine** un [¶](#index-utsname_003amachine)
    :   A description of the hardware.

Scheme Procedure: **gethostname** [¶](#index-gethostname)

C Function: **scm\_gethostname** () [¶](#index-scm_005fgethostname)
:   Return the host name of the current processor.

Scheme Procedure: **sethostname** name [¶](#index-sethostname)

C Function: **scm\_sethostname** (name) [¶](#index-scm_005fsethostname)
:   Set the host name of the current processor to name. May
    only be used by the superuser. The return value is not
    specified.

---

Next: [Encryption](#Encryption), Previous: [System Identification](#System-Identification), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.13 Locales [¶](#Locales-1)

Scheme Procedure: **setlocale** category [locale] [¶](#index-setlocale)

C Function: **scm\_setlocale** (category, locale) [¶](#index-scm_005fsetlocale)
:   Get or set the current locale, used for various internationalizations.
    Locales are strings, such as ‘`sv_SE`’.

    If locale is given then the locale for the given category
    is set and the new value returned. If locale is not given then
    the current value is returned. category should be one of the
    following values (see [Categories of Activities
    that Locales Affect](/libc/latest/en/libc.html#Locale-Categories) in The GNU C Library Reference Manual):

    Variable: **LC\_ALL** [¶](#index-LC_005fALL)

    Variable: **LC\_COLLATE** [¶](#index-LC_005fCOLLATE)

    Variable: **LC\_CTYPE** [¶](#index-LC_005fCTYPE)

    Variable: **LC\_MESSAGES** [¶](#index-LC_005fMESSAGES)

    Variable: **LC\_MONETARY** [¶](#index-LC_005fMONETARY)

    Variable: **LC\_NUMERIC** [¶](#index-LC_005fNUMERIC)

    Variable: **LC\_TIME** [¶](#index-LC_005fTIME)

    A common usage is ‘`(setlocale LC_ALL "")`’, which initializes all
    categories based on standard environment variables (`LANG` etc).
    For full details on categories and locale names see [Locales and Internationalization](/libc/latest/en/libc.html#Locales) in The GNU C Library Reference
    Manual.

    Note that `setlocale` affects locale settings for the whole
    process. See [locale objects and
    `make-locale`](#i18n-Introduction), for a thread-safe alternative.

A `system-error` exception (see [How to Handle Errors](#Handling-Errors)) is raised by
`setlocale` when locale-name does not match any of the
locales compiled on the system.

---

Previous: [Locales](#Locales), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.14 Encryption [¶](#Encryption-1)

Please note that the procedures in this section are not suited for
strong encryption, they are only interfaces to the well-known and
common system library functions of the same name. They are just as good
(or bad) as the underlying functions, so you should refer to your system
documentation before using them (see [Encrypting Passwords](/libc/latest/en/libc.html#crypt) in The GNU C Library Reference Manual).

Scheme Procedure: **crypt** key salt [¶](#index-crypt)

C Function: **scm\_crypt** (key, salt) [¶](#index-scm_005fcrypt)
:   Encrypt key, with the addition of salt (both strings),
    using the `crypt` C library call.

Although `getpass` is not an encryption procedure per se, it
appears here because it is often used in combination with `crypt`:

Scheme Procedure: **getpass** prompt [¶](#index-getpass)

C Function: **scm\_getpass** (prompt) [¶](#index-scm_005fgetpass)
:   Display prompt to the standard error output and read
    a password from `/dev/tty`. If this file is not
    accessible, it reads from standard input. The password may be
    up to 127 characters in length. Additional characters and the
    terminating newline character are discarded. While reading
    the password, echoing and the generation of signals by special
    characters is disabled.

---

Next: [The (ice-9 getopt-long) Module](#getopt_002dlong), Previous: [POSIX System Calls and Networking](#POSIX), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
