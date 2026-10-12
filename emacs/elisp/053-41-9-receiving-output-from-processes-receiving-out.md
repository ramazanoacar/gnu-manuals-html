### 41.9 Receiving Output from Processes [¶](#Receiving-Output-from-Processes)

The output that an asynchronous subprocess writes to its standard
output stream is passed to a function called the *filter
function*. The default filter function simply inserts the output into
a buffer, which is called the associated buffer of the process
(see [Process Buffers](#Process-Buffers)). If the process has no buffer then the
default filter discards the output.

If the subprocess writes to its standard error stream, by default
the error output is also passed to the process filter function.
Alternatively, you could use the `:stderr` parameter with a
non-`nil` value in a call to `make-process`
(see [make-process](#Asynchronous-Processes)) to make the destination
of the error output separate from the standard output.

When a subprocess terminates, Emacs reads any pending output,
then stops reading output from that subprocess. Therefore, if the
subprocess has children that are still live and still producing
output, Emacs won’t receive that output.

Output from a subprocess can arrive only while Emacs is waiting: when
reading terminal input (see the function `waiting-for-user-input-p`),
in `sit-for` and `sleep-for` (see [Waiting for Elapsed Time or Input](#Waiting)), in
`accept-process-output` (see [Accepting Output from Processes](#Accepting-Output)), and in
functions which send data to processes (see [Sending Input to Processes](#Input-to-Processes)).
This minimizes the problem of timing errors that usually plague parallel
programming. For example, you can safely create a process and only
then specify its buffer or filter function; no output can arrive
before you finish, if the code in between does not call any primitive
that waits.

Variable: **process-adaptive-read-buffering** [¶](#index-process_002dadaptive_002dread_002dbuffering)
:   On some systems, when Emacs reads the output from a subprocess, the
    output data is read in very small blocks, potentially resulting in
    very poor performance. This behavior can be remedied to some extent
    by setting the variable `process-adaptive-read-buffering` to a
    non-`nil` value, as it will automatically delay reading
    from such processes, thus allowing them to produce more output before
    Emacs tries to read it. The default is `nil`, since a
    non-`nil` value reduces performance.

* [Process Buffers](#Process-Buffers)
* [Process Filter Functions](#Filter-Functions)
* [Decoding Process Output](#Decoding-Output)
* [Accepting Output from Processes](#Accepting-Output)
* [Processes and Threads](#Processes-and-Threads)

---

Next: [Process Filter Functions](#Filter-Functions), Up: [Receiving Output from Processes](#Output-from-Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.9.1 Process Buffers [¶](#Process-Buffers-1)

A process can (and usually does) have an *associated buffer*,
which is an ordinary Emacs buffer that is used for two purposes: storing
the output from the process, and deciding when to kill the process. You
can also use the buffer to identify a process to operate on, since in
normal practice only one process is associated with any given buffer.
Many applications of processes also use the buffer for editing input to
be sent to the process, but this is not built into Emacs Lisp.

By default, process output is inserted in the associated buffer.
(You can change this by defining a custom filter function,
see [Process Filter Functions](#Filter-Functions).) The position to insert the output is
determined by the `process-mark`, which is then updated to point
to the end of the text just inserted. Usually, but not always, the
`process-mark` is at the end of the buffer.

Killing the associated buffer of a process also kills the process.
Emacs asks for confirmation first, if the process’s
`process-query-on-exit-flag` is non-`nil` (see [Querying Before Exit](#Query-Before-Exit)). This confirmation is done by the function
`process-kill-buffer-query-function`, which is run from
`kill-buffer-query-functions` (see [Killing Buffers](#Killing-Buffers)).

Function: **process-buffer** process [¶](#index-process_002dbuffer)
:   This function returns the associated buffer of the specified
    process.

    ```
    (process-buffer (get-process "shell"))
         ⇒ #<buffer *shell*>
    ```

Function: **process-mark** process [¶](#index-process_002dmark)
:   This function returns the process marker for process, which is the
    marker that says where to insert output from the process.

    If process does not have a buffer, `process-mark` returns a
    marker that points nowhere.

    The default filter function uses this marker to decide where to
    insert process output, and updates it to point after the inserted text.
    That is why successive batches of output are inserted consecutively.

    Custom filter functions normally should use this marker in the same fashion.
    For an example of a filter function that uses `process-mark`,
    see [Process Filter Example](#Process-Filter-Example).

    When the user is expected to enter input in the process buffer for
    transmission to the process, the process marker separates the new input
    from previous output.

Function: **set-process-buffer** process buffer [¶](#index-set_002dprocess_002dbuffer)
:   This function sets the buffer associated with process to
    buffer. If buffer is `nil`, the process becomes
    associated with no buffer; if non-`nil` and different from the
    buffer associated with the process, the process mark will be set to
    point to the end of buffer (unless the process mark is already
    associated with buffer).

Function: **get-buffer-process** buffer-or-name [¶](#index-get_002dbuffer_002dprocess)
:   This function returns a nondeleted process associated with the buffer
    specified by buffer-or-name. If there are several processes
    associated with it, this function chooses one (currently, the one most
    recently created, but don’t count on that). Deletion of a process
    (see `delete-process`) makes it ineligible for this function to
    return.

    It is usually a bad idea to have more than one process associated with
    the same buffer.

    ```
    (get-buffer-process "*shell*")
         ⇒ #<process shell>
    ```

    Killing the process’s buffer deletes the process, which kills the
    subprocess with a `SIGHUP` signal (see [Sending Signals to Processes](#Signals-to-Processes)).

If the process’s buffer is displayed in a window, your Lisp program
may wish to tell the process the dimensions of that window, so that
the process could adapt its output to those dimensions, much as it
adapts to the screen dimensions. The following functions allow
communicating this kind of information to processes; however, not all
systems support the underlying functionality, so it is best to provide
fallbacks, e.g., via command-line arguments or environment variables.

Function: **set-process-window-size** process height width [¶](#index-set_002dprocess_002dwindow_002dsize)
:   Tell process that its logical window size has dimensions
    width by height, in character units. If this function
    succeeds in communicating this information to the process, it returns
    `t`; otherwise it returns `nil`.

When windows that display buffers associated with process change their
dimensions, the affected processes should be told about these changes.
By default, when the window configuration changes, Emacs will
automatically call `set-process-window-size` on behalf of every
process whose buffer is displayed in a window, passing it the smallest
dimensions of all the windows displaying the process’s buffer. This
works via `window-configuration-change-hook` (see [Hooks for Window Scrolling and Changes](#Window-Hooks)), which is told to invoke the function that is the value of
the variable `window-adjust-process-window-size-function` for
each process whose buffer is displayed in at least one window. You
can customize this behavior by setting the value of that variable.

User Option: **window-adjust-process-window-size-function** [¶](#index-window_002dadjust_002dprocess_002dwindow_002dsize_002dfunction)
:   The value of this variable should be a function of two arguments: a
    process and the list of windows displaying the process’s buffer. When
    the function is called, the process’s buffer is the current buffer.
    The function should return a cons cell `(width . height)` that describes the dimensions of the logical process
    window to be passed via a call to `set-process-window-size`. The
    function can also return `nil`, in which case Emacs will not call
    `set-process-window-size` for this process.

    Emacs supplies two predefined values for this variable:
    `window-adjust-process-window-size-smallest`, which returns the
    smallest of all the dimensions of the windows that display a process’s
    buffer; and `window-adjust-process-window-size-largest`, which
    returns the largest dimensions. For more complex strategies, write
    your own function.

    This variable can be buffer-local.

If the process has the `adjust-window-size-function` property
(see [Process Information](#Process-Information)), its value overrides the global and
buffer-local values of
`window-adjust-process-window-size-function`.

---

Next: [Decoding Process Output](#Decoding-Output), Previous: [Process Buffers](#Process-Buffers), Up: [Receiving Output from Processes](#Output-from-Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.9.2 Process Filter Functions [¶](#Process-Filter-Functions)

A process *filter function* is a function that receives the
standard output from the associated process. *All* output from
that process is passed to the filter. The default filter simply
outputs directly to the process buffer.

By default, the error output from the process, if any, is also
passed to the filter function, unless the destination for the standard
error stream of the process was separated from the standard output
when the process was created. Emacs will only call the filter
function during certain function calls. See [Receiving Output from Processes](#Output-from-Processes).
Note that if any of those functions are called by the filter, the
filter may be called recursively.

A filter function must accept two arguments: the associated process
and a string, which is output just received from it. The function is
then free to do whatever it chooses with the output.

Quitting is normally inhibited within a filter function—otherwise,
the effect of typing `C-g` at command level or to quit a user
command would be unpredictable. If you want to permit quitting inside
a filter function, bind `inhibit-quit` to `nil`. In most
cases, the right way to do this is with the macro
`with-local-quit`. See [Quitting](#Quitting).

If an error happens during execution of a filter function, it is
caught automatically, so that it doesn’t stop the execution of whatever
program was running when the filter function was started. However, if
`debug-on-error` is non-`nil`, errors are not caught.
This makes it possible to use the Lisp debugger to debug filter
functions. See [The Lisp Debugger](#Debugger). If an error is caught, Emacs pauses for
`process-error-pause-time` seconds so that the user sees the
error. See [Creating an Asynchronous Process](#Asynchronous-Processes).

Many filter functions sometimes (or always) insert the output in the
process’s buffer, mimicking the actions of the default filter.
Such filter functions need to make sure that they save the
current buffer, select the correct buffer (if different) before
inserting output, and then restore the original buffer.
They should also check whether the buffer is still alive, update the
process marker, and in some cases update the value of point. Here is
how to do these things:

```
(defun ordinary-insertion-filter (proc string)
  (when (buffer-live-p (process-buffer proc))
    (with-current-buffer (process-buffer proc)
      (let ((moving (= (point) (process-mark proc))))
```

```
        (save-excursion
          ;; Insert the text, advancing the process marker.
          (goto-char (process-mark proc))
          (insert string)
          (set-marker (process-mark proc) (point)))
        (if moving (goto-char (process-mark proc)))))))
```

To make the filter force the process buffer to be visible whenever new
text arrives, you could insert a line like the following just before the
`with-current-buffer` construct:

```
(display-buffer (process-buffer proc))
```

To force point to the end of the new output, no matter where it was
previously, eliminate the variable `moving` from the example and
call `goto-char` unconditionally. Note that this doesn’t
necessarily move window point. The default filter actually uses
`insert-before-markers` which moves all markers, including window
point. This may move unrelated markers, so it’s generally better to
move window point explicitly, or set its insertion type to `t`
(see [Windows and Point](#Window-Point)).

Note that Emacs automatically saves and restores the match data
while executing filter functions. See [The Match Data](#Match-Data).

The output to the filter may come in chunks of any size. A program
that produces the same output twice in a row may send it as one batch of
200 characters one time, and five batches of 40 characters the next. If
the filter looks for certain text strings in the subprocess output, make
sure to handle the case where one of these strings is split across two
or more batches of output; one way to do this is to insert the
received text into a temporary buffer, which can then be searched.

Function: **set-process-filter** process filter [¶](#index-set_002dprocess_002dfilter)
:   This function gives process the filter function filter.
    If filter is `nil`, it gives the process the default
    filter, which inserts the process output into the process buffer. If
    filter is `t`, Emacs stops accepting output from the
    process, unless it’s a network server process that listens for
    incoming connections.

Function: **process-filter** process [¶](#index-process_002dfilter)
:   This function returns the filter function of process.

In case the process’s output needs to be passed to several filters, you can
use `add-function` to combine an existing filter with a new one.
See [Advising Emacs Lisp Functions](#Advising-Functions).

Here is an example of the use of a filter function:

```
(defun keep-output (process output)
   (setq kept (cons output kept)))
     ⇒ keep-output
```

```
(setq kept nil)
     ⇒ nil
```

```
(set-process-filter (get-process "shell") 'keep-output)
     ⇒ keep-output
```

```
(process-send-string "shell" "ls ~/other\n")
     ⇒ nil
kept
     ⇒ ("lewis@slug:$ "
```

```
"FINAL-W87-SHORT.MSS    backup.otl              kolstad.mss~
address.txt             backup.psf              kolstad.psf
backup.bib~             david.mss               resume-Dec-86.mss~
backup.err              david.psf               resume-Dec.psf
backup.mss              dland                   syllabus.mss
"
"#backups.mss#          backup.mss~             kolstad.mss
")
```

---

Next: [Accepting Output from Processes](#Accepting-Output), Previous: [Process Filter Functions](#Filter-Functions), Up: [Receiving Output from Processes](#Output-from-Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.9.3 Decoding Process Output [¶](#Decoding-Process-Output)

When Emacs writes process output directly into a multibyte buffer,
it decodes the output according to the process output coding system.
If the coding system is `raw-text` or `no-conversion`, Emacs
converts the unibyte output to multibyte using
`string-to-multibyte`, and inserts the resulting multibyte text.

You can use `set-process-coding-system` to specify which coding
system to use (see [Process Information](#Process-Information)). Otherwise, the coding
system comes from `coding-system-for-read`, if that is
non-`nil`; or else from the defaulting mechanism (see [Default Coding Systems](#Default-Coding-Systems)). If the text output by a process contains null
bytes, Emacs by default uses `no-conversion` for it; see
[inhibit-null-byte-detection](#Lisp-and-Coding-Systems), for how to
control this behavior.

**Warning:** Coding systems such as `undecided`, which
determine the coding system from the data, do not work entirely
reliably with asynchronous subprocess output. This is because Emacs
has to process asynchronous subprocess output in batches, as it
arrives. Emacs must try to detect the proper coding system from one
batch at a time, and this does not always work. Therefore, if at all
possible, specify a coding system that determines both the character
code conversion and the end of line conversion—that is, one like
`latin-1-unix`, rather than `undecided` or `latin-1`.

When Emacs calls a process filter function, it provides the process
output as a multibyte string or as a unibyte string according to the
process’s filter coding system. Emacs
decodes the output according to the process output coding system,
which usually produces a multibyte string, except for coding systems
such as `binary` and `raw-text`.

---

Next: [Processes and Threads](#Processes-and-Threads), Previous: [Decoding Process Output](#Decoding-Output), Up: [Receiving Output from Processes](#Output-from-Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.9.4 Accepting Output from Processes [¶](#Accepting-Output-from-Processes)

Output from asynchronous subprocesses normally arrives only while
Emacs is waiting for some sort of external event, such as elapsed time
or terminal input. Occasionally it is useful in a Lisp program to
explicitly permit output to arrive at a specific point, or even to wait
until output arrives from a process.

Function: **accept-process-output** &optional process seconds millisec just-this-one [¶](#index-accept_002dprocess_002doutput)
:   This function allows Emacs to read pending output from processes. The
    output is given to the filter functions set up by the processes
    (see [Process Filter Functions](#Filter-Functions)). If process is
    non-`nil` then this function does not return until some output
    has been received from process or process has closed the
    connection.

    The arguments seconds and millisec let you specify timeout
    periods. The former specifies a period measured in seconds and the
    latter specifies one measured in milliseconds. The two time periods
    thus specified are added together, and `accept-process-output`
    returns after that much time, even if there is no
    subprocess output.

    The argument millisec is obsolete (and should not be used),
    because seconds can be floating point to specify
    waiting a fractional number of seconds. If seconds is 0, the
    function accepts whatever output is pending but does not wait.

    If process is a process, and the argument just-this-one is
    non-`nil`, only output from that process is handled, suspending output
    from other processes until some output has been received from that
    process or the timeout expires. If just-this-one is an integer,
    also inhibit running timers. This feature is generally not
    recommended, but may be necessary for specific applications, such as
    speech synthesis.

    The function `accept-process-output` returns non-`nil` if it
    got output from process, or from any process if process is
    `nil`; this can occur even after a process has exited if the
    corresponding connection contains buffered data. The function returns
    `nil` if the timeout expired or the connection was closed before output
    arrived.

    Note that this function is not guaranteed to return as soon as some
    output is available from a subprocess, because the main purpose of the
    function is to let Emacs read output from subprocesses, not to wait as
    little as possible. In particular, if process is `nil`, the
    function should not be expected to return before the specified timeout
    expires. Thus, if a Lisp program needs to minimize the wait for the
    subprocess output, it should call this function with a non-`nil`
    value of a specific process.

If a connection from a process contains buffered data,
`accept-process-output` can return non-`nil` even after the
process has exited. Therefore, although the following loop:

```
;; This loop contains a bug.
(while (process-live-p process)
  (accept-process-output process))
```

will often read all output from process, it has a race condition
and can miss some output if `process-live-p` returns `nil`
while the connection still contains data. Better is to write the loop
like this:

```
(while (accept-process-output process))
```

If you have passed a non-`nil` stderr to
`make-process`, it will have a standard error process.
See [Creating an Asynchronous Process](#Asynchronous-Processes). In that case, waiting for process
output from the main process doesn’t wait for output from the standard
error process. To make sure you have received both all of standard
output and all of standard error from a process, use the following
code:

```
(while (accept-process-output process))
(while (accept-process-output stderr-process))
```

If you passed a buffer to the stderr argument of
`make-process`, you still have to wait for the standard error
process, like so:

```
(let* ((stdout (generate-new-buffer "stdout"))
       (stderr (generate-new-buffer "stderr"))
       (process (make-process :name "test"
                              :command '("my-program")
                              :buffer stdout
                              :stderr stderr))
       (stderr-process (get-buffer-process stderr)))
  (unless (and process stderr-process)
    (error "Process unexpectedly nil"))
  (while (accept-process-output process))
  (while (accept-process-output stderr-process)))
```

Only when both `accept-process-output` forms return `nil`,
you can be sure that the process has exited and Emacs has read all its
output.

Reading pending standard error from a process running on a remote host
is not possible this way.

---

Previous: [Accepting Output from Processes](#Accepting-Output), Up: [Receiving Output from Processes](#Output-from-Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.9.5 Processes and Threads [¶](#Processes-and-Threads-1)

Because threads were a relatively late addition to Emacs Lisp, and
due to the way dynamic binding was sometimes used in conjunction with
`accept-process-output`, by default a process is locked to the
thread that created it. When a process is locked to a thread, output
from the process can only be accepted by that thread.

A Lisp program can specify to which thread a process is to be
locked, or instruct Emacs to unlock a process, in which case its
output can be processed by any thread. Only a single thread will wait
for output from a given process at one time—once one thread begins
waiting for output, the process is temporarily locked until
`accept-process-output` or `sit-for` returns.

If the thread exits, all the processes locked to it are unlocked.

Function: **process-thread** process [¶](#index-process_002dthread)
:   Return the thread to which process is locked. If process
    is unlocked, return `nil`.

Function: **set-process-thread** process thread [¶](#index-set_002dprocess_002dthread)
:   Set the locking thread of process to thread. thread
    may be `nil`, in which case the process is unlocked.

---

Next: [Querying Before Exit](#Query-Before-Exit), Previous: [Receiving Output from Processes](#Output-from-Processes), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.10 Sentinels: Detecting Process Status Changes [¶](#Sentinels_003a-Detecting-Process-Status-Changes)

A *process sentinel* is a function that is called whenever the
associated process changes status for any reason, including signals
(whether sent by Emacs or caused by the process’s own actions) that
terminate, stop, or continue the process. The process sentinel is
also called if the process exits. The sentinel receives two
arguments: the process for which the event occurred, and a string
describing the type of event.

If no sentinel function was specified for a process, it will use the
default sentinel function, which inserts a message in the process’s
buffer with the process name and the string describing the event.

The string describing the event looks like one of the following (but
this is not an exhaustive list of event strings):

* `"finished\n"`.
* `"deleted\n"`.
* `"exited abnormally with code exitcode (core dumped)\n"`.
  The “core dumped” part is optional, and only appears if the process
  dumped core.
* `"failed with code fail-code\n"`.
* `"signal-description (core dumped)\n"`. The
  signal-description is a system-dependent textual description of
  a signal, e.g., `"killed"` for `SIGKILL`. The “core
  dumped” part is optional, and only appears if the process dumped
  core.
* `"open from host-name\n"`.
* `"open\n"`.
* `"run\n"`.
* `"connection broken by remote peer\n"`.

A sentinel runs only while Emacs is waiting (e.g., for terminal
input, or for time to elapse, or for process output). This avoids the
timing errors that could result from running sentinels at random places in
the middle of other Lisp programs. A program can wait, so that
sentinels will run, by calling `sit-for` or `sleep-for`
(see [Waiting for Elapsed Time or Input](#Waiting)), or `accept-process-output` (see [Accepting Output from Processes](#Accepting-Output)). Emacs also allows sentinels to run when the command loop is
reading input. `delete-process` calls the sentinel when it
terminates a running process.

Emacs does not keep a queue of multiple reasons to call the sentinel
of one process; it records just the current status and the fact that
there has been a change. Therefore two changes in status, coming in
quick succession, can call the sentinel just once. However, process
termination will always run the sentinel exactly once. This is
because the process status can’t change again after termination.

Emacs explicitly checks for output from the process before running
the process sentinel. Once the sentinel runs due to process
termination, no further output can arrive from the process.

A sentinel that writes the output into the buffer of the process
should check whether the buffer is still alive. If it tries to insert
into a dead buffer, it will get an error. If the buffer is dead,
`(buffer-name (process-buffer process))` returns `nil`.

Quitting is normally inhibited within a sentinel—otherwise, the
effect of typing `C-g` at command level or to quit a user command
would be unpredictable. If you want to permit quitting inside a
sentinel, bind `inhibit-quit` to `nil`. In most cases, the
right way to do this is with the macro `with-local-quit`.
See [Quitting](#Quitting).

If an error happens during execution of a sentinel, it is caught
automatically, so that it doesn’t stop the execution of whatever
programs was running when the sentinel was started. However, if
`debug-on-error` is non-`nil`, errors are not caught.
This makes it possible to use the Lisp debugger to debug the
sentinel. See [The Lisp Debugger](#Debugger). If an error is caught, Emacs pauses for
`process-error-pause-time` seconds so that the user sees the
error. See [Creating an Asynchronous Process](#Asynchronous-Processes).

While a sentinel is running, the process sentinel is temporarily
set to `nil` so that the sentinel won’t run recursively.
For this reason it is not possible for a sentinel to specify
a new sentinel.

Note that Emacs automatically saves and restores the match data
while executing sentinels. See [The Match Data](#Match-Data).

Function: **set-process-sentinel** process sentinel [¶](#index-set_002dprocess_002dsentinel)
:   This function associates sentinel with process. If
    sentinel is `nil`, then the process will have the default
    sentinel, which inserts a message in the process’s buffer when the
    process status changes.

    Changes in process sentinels take effect immediately—if the sentinel
    is slated to be run but has not been called yet, and you specify a new
    sentinel, the eventual call to the sentinel will use the new one.

    ```
    (defun msg-me (process event)
       (princ
         (format "Process: %s had the event '%s'" process event)))
    (set-process-sentinel (get-process "shell") 'msg-me)
         ⇒ msg-me
    ```

    ```
    (kill-process (get-process "shell"))
         -| Process: #<process shell> had the event 'killed'
         ⇒ #<process shell>
    ```

Function: **process-sentinel** process [¶](#index-process_002dsentinel)
:   This function returns the sentinel of process.

In case a process status changes need to be passed to several sentinels, you
can use `add-function` to combine an existing sentinel with a new one.
See [Advising Emacs Lisp Functions](#Advising-Functions).

Function: **waiting-for-user-input-p** [¶](#index-waiting_002dfor_002duser_002dinput_002dp)
:   While a sentinel or filter function is running, this function returns
    non-`nil` if Emacs was waiting for keyboard input from the user at
    the time the sentinel or filter function was called, or `nil` if it
    was not.

---

Next: [Accessing Other Processes](#System-Processes), Previous: [Sentinels: Detecting Process Status Changes](#Sentinels), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.11 Querying Before Exit [¶](#Querying-Before-Exit)

When Emacs exits, it terminates all its subprocesses. For
subprocesses that run a program, it sends them the `SIGHUP`
signal; connections are simply closed. Because subprocesses may be
doing valuable work, Emacs normally asks the user to confirm that it
is ok to terminate them. Each process has a query flag, which, if
non-`nil`, says that Emacs should ask for confirmation before
exiting and thus killing that process. The default for the query flag
is `t`, meaning *do* query.

Function: **process-query-on-exit-flag** process [¶](#index-process_002dquery_002don_002dexit_002dflag)
:   This returns the query flag of process.

Function: **set-process-query-on-exit-flag** process flag [¶](#index-set_002dprocess_002dquery_002don_002dexit_002dflag)
:   This function sets the query flag of process to flag. It
    returns flag.

    Here is an example of using `set-process-query-on-exit-flag` on a
    shell process to avoid querying:

    ```
    (set-process-query-on-exit-flag (get-process "shell") nil)
         ⇒ nil
    ```

User Option: **confirm-kill-processes** [¶](#index-confirm_002dkill_002dprocesses)
:   If this user option is set to `t` (the default), then Emacs asks
    for confirmation before killing processes on exit. If it is
    `nil`, Emacs kills processes without confirmation, i.e., the
    query flag of all processes is ignored.

---

Next: [Transaction Queues](#Transaction-Queues), Previous: [Querying Before Exit](#Query-Before-Exit), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.12 Accessing Other Processes [¶](#Accessing-Other-Processes)

In addition to accessing and manipulating processes that are
subprocesses of the current Emacs session, Emacs Lisp programs can
also access other processes. We call these *system processes*, to
distinguish them from Emacs subprocesses.

Emacs provides several primitives for accessing system processes.
Not all platforms support these primitives; on those which don’t,
these primitives return `nil`.

Function: **list-system-processes** [¶](#index-list_002dsystem_002dprocesses)
:   This function returns a list of all the processes running on the
    system. Each process is identified by its PID, a numerical
    process ID that is assigned by the OS and distinguishes the process
    from all the other processes running on the same machine at the same
    time.

    If `default-directory` points to a remote host, processes of that
    host are returned.

Function: **process-attributes** pid [¶](#index-process_002dattributes)
:   This function returns an alist of attributes for the process specified
    by its process ID pid. Each association in the alist is of the
    form `(key . value)`, where key designates the
    attribute and value is the value of that attribute. The various
    attribute keys that this function can return are listed below.
    Not all platforms support all of these attributes; if an attribute is
    not supported, its association will not appear in the returned alist.

    If `default-directory` points to a remote host, pid is
    regarded as process of that host.

    `euid`
    :   The effective user ID of the user who invoked the process. The
        corresponding value is a number. If the process was invoked by
        the same user who runs the current Emacs session, the value is
        identical to what `user-uid` returns (see [User Identification](#User-Identification)).

    `user`
    :   User name corresponding to the process’s effective user ID, a string.

    `egid`
    :   The group ID of the effective user ID, a number.

    `group`
    :   Group name corresponding to the effective user’s group ID, a string.

    `comm`
    :   The name of the command that runs in the process. This is a string
        that usually specifies the name of the executable file of the process,
        without the leading directories. However, some special system
        processes can report strings that do not correspond to an executable
        file of a program.

    `state`
    :   The state code of the process. This is a short string that encodes
        the scheduling state of the process. Here’s a list of the most
        frequently seen codes:

        `"D"`
        :   uninterruptible sleep (usually I/O)

        `"R"`
        :   running

        `"S"`
        :   interruptible sleep (waiting for some event)

        `"T"`
        :   stopped, e.g., by a job control signal

        `"Z"`
        :   zombie: a process that terminated, but was not reaped by its parent

        For the full list of the possible states, see the manual page of the
        `ps` command.

    `ppid`
    :   The process ID of the parent process, a number.

    `pgrp`
    :   The process group ID of the process, a number.

    `sess`
    :   The session ID of the process. This is a number that is the process
        ID of the process’s *session leader*.

    `ttname`
    :   A string that is the name of the process’s controlling terminal. On
        Unix and GNU systems, this is normally the file name of the
        corresponding terminal device, such as `/dev/pts65`.

    `tpgid`
    :   The numerical process group ID of the foreground process group that
        uses the process’s terminal.

    `minflt`
    :   The number of minor page faults caused by the process since its
        beginning. (Minor page faults are those that don’t involve reading
        from disk.)

    `majflt`
    :   The number of major page faults caused by the process since its
        beginning. (Major page faults require a disk to be read, and are thus
        more expensive than minor page faults.)

    `cminflt`

    `cmajflt`
    :   Like `minflt` and `majflt`, but include the number of page
        faults for all the child processes of the given process.

    `utime`
    :   Time spent by the process in the user context, for running the
        application’s code. The corresponding value is a Lisp
        timestamp (see [Time of Day](#Time-of-Day)).

    `stime`
    :   Time spent by the process in the system (kernel) context, for
        processing system calls. The corresponding value is a Lisp
        timestamp.

    `time`
    :   The sum of `utime` and `stime`. The corresponding
        value is a Lisp timestamp.

    `cutime`

    `cstime`

    `ctime`
    :   Like `utime`, `stime`, and `time`, but include the
        times of all the child processes of the given process.

    `pri`
    :   The numerical priority of the process.

    `nice`
    :   The *nice value* of the process, a number. (Processes with smaller
        nice values get scheduled more favorably.)

    `thcount`
    :   The number of threads in the process.

    `start`
    :   The time when the process was started, as a Lisp timestamp.

    `etime`
    :   The time elapsed since the process started, as a Lisp timestamp.

    `vsize`
    :   The virtual memory size of the process, measured in kilobytes.

    `rss`
    :   The size of the process’s *resident set*, the number of kilobytes
        occupied by the process in the machine’s physical memory.

    `pcpu`
    :   The percentage of the CPU time used by the process since it started.
        The corresponding value is a nonnegative floating-point number.
        Although in theory the number can exceed 100 on a multicore platform,
        it is usually less than 100 because Emacs is typically single-threaded.

    `pmem`
    :   The percentage of the total physical memory installed on the machine
        used by the process’s resident set. The value is a floating-point
        number between 0 and 100.

    `args`
    :   The command-line with which the process was invoked. This is a string
        in which individual command-line arguments are separated by blanks;
        whitespace characters that are embedded in the arguments are quoted as
        appropriate for the system’s shell: escaped by backslash characters on
        GNU and Unix, and enclosed in double quote characters on Windows.
        Thus, this command-line string can be directly used in primitives such
        as `shell-command`.

---

Next: [Network Connections](#Network), Previous: [Accessing Other Processes](#System-Processes), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.13 Transaction Queues [¶](#Transaction-Queues-1)

You can use a *transaction queue* to communicate with a subprocess
using transactions. First use `tq-create` to create a transaction
queue communicating with a specified process. Then you can call
`tq-enqueue` to send a transaction.

Function: **tq-create** process [¶](#index-tq_002dcreate)
:   This function creates and returns a transaction queue communicating with
    process. The argument process should be a subprocess
    capable of sending and receiving streams of bytes. It may be a child
    process, or it may be a TCP connection to a server, possibly on another
    machine.

Function: **tq-enqueue** queue question regexp closure fn &optional delay-question [¶](#index-tq_002denqueue)
:   This function sends a transaction to queue queue. Specifying the
    queue has the effect of specifying the subprocess to talk to.

    The argument question is the outgoing message that starts the
    transaction. The argument fn is the function to call when the
    corresponding answer comes back; it is called with two arguments:
    closure, and the answer received.

    The argument regexp is a regular expression that should match
    text at the end of the entire answer, but nothing before; that’s how
    `tq-enqueue` determines where the answer ends.

    If the argument delay-question is non-`nil`, delay sending
    this question until the process has finished replying to any previous
    questions. This produces more reliable results with some processes.

Function: **tq-close** queue [¶](#index-tq_002dclose)
:   Shut down transaction queue queue, waiting for all pending transactions
    to complete, and then terminate the connection or child process.

Transaction queues are implemented by means of a filter function.
See [Process Filter Functions](#Filter-Functions).

---

Next: [Network Servers](#Network-Servers), Previous: [Transaction Queues](#Transaction-Queues), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.14 Network Connections [¶](#Network-Connections)

Emacs Lisp programs can open stream (TCP) and datagram (UDP) network
connections (see [Datagrams](#Datagrams)) to other processes on the same machine
or other machines.
A network connection is handled by Lisp much like a subprocess, and is
represented by a process object. However, the process you are
communicating with is not a child of the Emacs process, has no
process ID, and you can’t kill it or send it signals. All you
can do is send and receive data. `delete-process` closes the
connection, but does not kill the program at the other end; that
program must decide what to do about closure of the connection.

Lisp programs can listen for connections by creating network
servers. A network server is also represented by a kind of process
object, but unlike a network connection, the network server never
transfers data itself. When it receives a connection request, it
creates a new network connection to represent the connection just
made. (The network connection inherits certain information, including
the process plist, from the server.) The network server then goes
back to listening for more connection requests.

Network connections and servers are created by calling
`make-network-process` with an argument list consisting of
keyword/argument pairs, for example `:server t` to create a
server process, or `:type 'datagram` to create a datagram
connection. See [Low-Level Network Access](#Low_002dLevel-Network), for details. You can also use
the `open-network-stream` function described below.

To distinguish the different types of processes, the
`process-type` function returns the symbol `network` for a
network connection or server, `serial` for a serial port
connection, `pipe` for a pipe connection, or `real` for a
real subprocess.

The `process-status` function returns `open`,
`closed`, `connect`, `stop`, or `failed` for
network connections. For a network server, the status is always
`listen`. Except for `stop`, none of those values is
possible for a real subprocess. See [Process Information](#Process-Information).

You can stop and resume operation of a network process by calling
`stop-process` and `continue-process`. For a server
process, being stopped means not accepting new connections. (Up to 5
connection requests will be queued for when you resume the server; you
can increase this limit, unless it is imposed by the operating
system—see the `:server` keyword of `make-network-process`,
[`make-network-process`](#Network-Processes).) For a network stream connection, being
stopped means not processing input (any arriving input waits until you
resume the connection). For a datagram connection, some number of
packets may be queued but input may be lost. You can use the function
`process-command` to determine whether a network connection or
server is stopped; a non-`nil` value means yes.

Emacs can create encrypted network connections, using the built-in
support for the GnuTLS Transport Layer Security Library; see
[the GnuTLS project page](https://www.gnu.org/software/gnutls/).
If your Emacs was compiled with GnuTLS support, the function
`gnutls-available-p` is defined and returns non-`nil`. For
more details, see [Overview](https://www.gnu.org/software/emacs/manual/html_mono/emacs-gnutls.html#Top) in The Emacs-GnuTLS manual.
The `open-network-stream` function can transparently handle the
details of creating encrypted connections for you, using whatever
support is available.

Function: **open-network-stream** name buffer host service &rest parameters [¶](#index-open_002dnetwork_002dstream)
:   This function opens a TCP connection, with optional encryption, and
    returns a process object that represents the connection.

    The name argument specifies the name for the process object. It
    is modified as necessary to make it unique.

    The buffer argument is the buffer to associate with the
    connection. Output from the connection is inserted in the buffer,
    unless you specify your own filter function to handle the output. If
    buffer is `nil`, it means that the connection is not
    associated with any buffer.

    The arguments host and service specify where to connect to;
    host is the host name (a string), and service is the name of
    a defined network service (a string) or a port number (an integer like
    `80` or an integer string like `"80"`).

    The remaining arguments parameters are keyword/argument pairs
    that are mainly relevant to encrypted connections:

    `:nowait boolean`
    :   If non-`nil`, try to make an asynchronous connection.

    `:noquery query-flag`
    :   Initialize the process query flag to query-flag.
        See [Querying Before Exit](#Query-Before-Exit).

    `:coding coding`
    :   Use this to set the coding systems used by the network process, in
        preference to binding `coding-system-for-read` or
        `coding-system-for-write`. See [`make-network-process`](#Network-Processes), for
        details.

    `:type type`
    :   The type of connection. Options are:

        `plain`
        :   An ordinary, unencrypted connection.

        `tls`

        `ssl`
        :   A TLS (Transport Layer Security) connection.

        `nil`

        `network`
        :   Start with a plain connection, and if parameters ‘`:success`’
            and ‘`:capability-command`’ are supplied, try to upgrade to an encrypted
            connection via STARTTLS. If that fails, retain the
            unencrypted connection.

        `starttls`
        :   As for `nil`, but if STARTTLS fails, drop the connection.

        `shell`
        :   A shell connection.

    `:always-query-capabilities boolean`
    :   If non-`nil`, always ask for the server’s capabilities, even when
        doing a ‘`plain`’ connection.

    `:capability-command capability-command`
    :   Command to query the host capabilities. This can either be a string
        (which will then be sent verbatim to the server) or a function
        (called with a single parameter: the “greeting” from the server when
        connecting) that should return a string.

    `:end-of-command regexp`

    `:end-of-capability regexp`
    :   Regular expression matching the end of a command, or the end of the
        command capability-command. The latter defaults to the former.

    `:starttls-function function`
    :   Function of one argument (the response to capability-command),
        which returns either `nil` or the command to activate
        STARTTLS, if supported.

    `:success regexp`
    :   Regular expression matching a successful STARTTLS negotiation.

    `:use-starttls-if-possible boolean`
    :   If non-`nil`, do opportunistic STARTTLS upgrades even if Emacs
        doesn’t have built-in TLS support.

    `:warn-unless-encrypted boolean`
    :   If non-`nil`, warn the user if the final connection type is not
        encrypted. This is useful for protocols like IMAP and the
        like, where most users would expect the network traffic to be encrypted.
        This may be due to STARTTLS upgrade failure, specifying
        `:return-list` non-`nil` allows you to capture any error
        encountered.

    `:client-certificate list-or-t` [¶](#index-network_002dstream_002duse_002dclient_002dcertificates)
    :   Either a list of the form `(key-file cert-file)`,
        naming the certificate key file and certificate file itself, or
        `t`, meaning to query `auth-source` for this information
        (see [auth-source](https://www.gnu.org/software/emacs/manual/html_mono/auth.html#Help-for-users) in Emacs auth-source
        Library). Only used for TLS or STARTTLS. To
        enable automatic queries of `auth-source` when
        `:client-certificate` is not specified customize
        `network-stream-use-client-certificates` to `t`.

    `:return-list cons-or-nil`
    :   The return value of this function. If omitted or `nil`, return a
        process object. Otherwise, a cons of the form
        `(process-object . plist)`, where plist can
        include the following keywords:

        `:greeting string-or-nil`
        :   If non-`nil`, the greeting string returned by the host.

        `:capabilities string-or-nil`
        :   If non-`nil`, the host’s capability string.

        `:type symbol`
        :   The connection type: ‘`plain`’ or ‘`tls`’.

        `:error symbol`
        :   A string describing any error encountered when performing
            STARTTLS upgrade.

    `:shell-command string-or-nil`
    :   If the connection `type` is `shell`, this parameter will be
        interpreted as a format-spec string (see [Custom Format Strings](#Custom-Format-Strings))
        that will be executed to make the connection. The specs available are
        ‘`%s`’ for the host name and ‘`%p`’ for the port number. For
        instance, if you want to first ssh to ‘`gateway`’ before making a
        plain connection, then this parameter’s value could be something like
        ‘`ssh gateway nc %s %p`’.

---

Next: [Datagrams](#Datagrams), Previous: [Network Connections](#Network), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.15 Network Servers [¶](#Network-Servers-1)

You create a server by calling `make-network-process`
(see [`make-network-process`](#Network-Processes)) with `:server t`. The server will
listen for connection requests from clients. When it accepts a client
connection request, that creates a new network connection, itself a
process object, with the following parameters:

* The connection’s process name is constructed by concatenating the
  server process’s name with a client identification string. The
  client identification string for an IPv4 connection looks like
  ‘`<a.b.c.d:p>`’, which represents an
  address and port number. Otherwise, it is a
  unique number in brackets, as in ‘`<nnn>`’. The number
  is unique for each connection in the Emacs session.
* If the server has a non-default filter, the connection process does
  not get a separate process buffer; otherwise, Emacs creates a new
  buffer for the purpose. The buffer name is the server’s buffer name
  or process name, concatenated with the client identification string.

  The server’s process buffer value is never used directly, but the log
  function can retrieve it and use it to log connections by inserting
  text there.
* The communication type and the process filter and sentinel are
  inherited from those of the server. The server never directly
  uses its filter and sentinel; their sole purpose is to initialize
  connections made to the server.
* The connection’s process contact information is set according to the client’s
  addressing information (typically an IP address and a port number).
  This information is associated with the `process-contact`
  keywords `:host`, `:service`, `:remote`.
* The connection’s local address is set up according to the port
  number used for the connection.
* The client process’s plist is initialized from the server’s plist.

---

Next: [Low-Level Network Access](#Low_002dLevel-Network), Previous: [Network Servers](#Network-Servers), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.16 Datagrams [¶](#Datagrams-1)

A *datagram* connection communicates with individual packets rather
than streams of data. Each call to `process-send` sends one
datagram packet (see [Sending Input to Processes](#Input-to-Processes)), and each datagram
received results in one call to the filter function.

The datagram connection doesn’t have to talk with the same remote
peer all the time. It has a *remote peer address* which specifies
where to send datagrams to. Each time an incoming datagram is passed
to the filter function, the peer address is set to the address that
datagram came from; that way, if the filter function sends a datagram,
it will go back to that place. You can specify the remote peer
address when you create the datagram connection using the
`:remote` keyword. You can change it later on by calling
`set-process-datagram-address`.

Function: **process-datagram-address** process [¶](#index-process_002ddatagram_002daddress)
:   If process is a datagram connection or server, this function
    returns its remote peer address.

Function: **set-process-datagram-address** process address [¶](#index-set_002dprocess_002ddatagram_002daddress)
:   If process is a datagram connection or server, this function
    sets its remote peer address to address.

---

Next: [Misc Network Facilities](#Misc-Network), Previous: [Datagrams](#Datagrams), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.17 Low-Level Network Access [¶](#Low_002dLevel-Network-Access)

You can also create network connections by operating at a lower
level than that of `open-network-stream`, using
`make-network-process`.

* [`make-network-process`](#Network-Processes)
* [Network Options](#Network-Options)
* [Testing Availability of Network Features](#Network-Feature-Testing)

---

Next: [Network Options](#Network-Options), Up: [Low-Level Network Access](#Low_002dLevel-Network)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.17.1 `make-network-process` [¶](#make_002dnetwork_002dprocess)

The basic function for creating network connections and network
servers is `make-network-process`. It can do either of those
jobs, depending on the arguments you give it.

Function: **make-network-process** &rest args [¶](#index-make_002dnetwork_002dprocess)
:   This function creates a network connection or server and returns the
    process object that represents it. The arguments args are a
    list of keyword/argument pairs. Omitting a keyword is always
    equivalent to specifying it with value `nil`, except for
    `:coding`, `:filter-multibyte`, and `:reuseaddr`. Here
    are the meaningful keywords (those corresponding to network options
    are listed in the following section):

    `:name name`
    :   Use the string name as the process name. It is modified if
        necessary to make it unique.

    `:type type`
    :   Specify the communication type. A value of `nil` specifies a
        stream connection (the default); `datagram` specifies a datagram
        connection; `seqpacket` specifies a sequenced packet stream
        connection. Both connections and servers can be of these types.

    `:server server-flag`
    :   If server-flag is non-`nil`, create a server. Otherwise,
        create a connection. For a stream type server, server-flag may
        be an integer, which then specifies the length of the queue of pending
        connections to the server. The default queue length is 5.

    `:host host`
    :   Specify the host to connect to. host should be a host name or
        Internet address, as a string, or the symbol `local` to specify
        the local host. If you specify host for a server, it must
        specify a valid address for the local host, and only clients
        connecting to that address will be accepted. When using `local`,
        by default IPv4 will be used, specify a family of `ipv6` to
        override this. To listen on all interfaces, specify an address of
        ‘`"0.0.0.0"`’ for IPv4 or ‘`"::"`’ for IPv6. Note that on some
        operating systems, listening on ‘`"::"`’ will also listen on IPv4,
        so attempting to then listen separately on IPv4 will result in
        `EADDRINUSE` errors (‘`"Address already in use"`’).

    `:service service`
    :   service specifies a port number to connect to; or, for a server,
        the port number to listen on. It should be a service name like
        ‘`"https"`’ that translates to a port number, or an integer like ‘`443`’
        or an integer string like ‘`"443"`’ that specifies the port number
        directly. For a server, it can also be `t`, which means to let
        the system select an unused port number.

    `:family family`
    :   family specifies the address (and protocol) family for
        communication. `nil` means determine the proper address family
        automatically for the given host and service.
        `local` specifies a Unix socket, in which case host is
        ignored. `ipv4` and `ipv6` specify to use IPv4 and IPv6,
        respectively.

    `:use-external-socket use-external-socket`
    :   If use-external-socket is non-`nil` use any sockets passed
        to Emacs on invocation instead of allocating one. This is used by the
        Emacs server code to allow on-demand socket activation. If Emacs
        wasn’t passed a socket, this option is silently ignored.

    `:local local-address`
    :   For a server process, local-address is the address to listen on.
        It overrides family, host and service, so you
        might as well not specify them.

    `:remote remote-address`
    :   For a connection, remote-address is the address to connect to.
        It overrides family, host and service, so you
        might as well not specify them.

        For a datagram server, remote-address specifies the initial
        setting of the remote datagram address.

        The format of local-address or remote-address depends on
        the address family:

        * An IPv4 address is represented as a five-element vector of four 8-bit
          integers and one 16-bit integer
          `[a b c d p]` corresponding to
          numeric IPv4 address a.b.c.d and port number
          p.
        * An IPv6 address is represented as a nine-element vector of 16-bit
          integers `[a b c d e f
          g h p]` corresponding to numeric IPv6 address
          a:b:c:d:e:f:g:h and
          port number p.
        * A local address is represented as a string, which specifies the address
          in the local address space.
        * An unsupported-family address is represented by a cons
          `(f . av)`, where f is the family number and
          av is a vector specifying the socket address using one element
          per address data byte. Do not rely on this format in portable code,
          as it may depend on implementation defined constants, data sizes, and
          data structure alignment.

    `:nowait bool`
    :   If bool is non-`nil` for a stream connection, return
        without waiting for the connection to complete. When the connection
        succeeds or fails, Emacs will call the sentinel function, with a
        second argument matching `"open"` (if successful) or
        `"failed"`. The default is to block, so that
        `make-network-process` does not return until the connection has
        succeeded or failed.

        If you’re setting up an asynchronous TLS connection, you have to also
        provide the `:tls-parameters` parameter (see below).

        Depending on the capabilities of Emacs, how asynchronous
        `:nowait` is may vary. The three elements that may (or may not)
        be done asynchronously are domain name resolution, socket setup, and
        (for TLS connections) TLS negotiation.

        Many functions that interact with process objects, (for instance,
        `process-datagram-address`) rely on them at least having a socket
        before they can return a useful value. These functions will block
        until the socket has achieved the desired status. The recommended way
        of interacting with asynchronous sockets is to place a sentinel on the
        process, and not try to interact with it before it has changed status
        to ‘`"run"`’. That way, none of these functions will block.

    `:tls-parameters`
    :   When opening a TLS connection, this should be where the first element
        is the TLS type (which should either be `gnutls-x509pki` or
        `gnutls-anon`, and the remaining elements should form a keyword
        list acceptable for `gnutls-boot`. (This keyword list can be
        obtained from the `gnutls-boot-parameters` function.) The TLS
        connection will then be negotiated after completing the connection to
        the host.

    `:stop stopped`
    :   If stopped is non-`nil`, start the network connection or
        server in the stopped state.

    `:buffer buffer`
    :   Use buffer as the process buffer.

    `:coding coding`
    :   Use coding as the coding system for this process. To specify
        different coding systems for decoding data from the connection and for
        encoding data sent to it, specify `(decoding .
        encoding)` for coding.

        If you don’t specify this keyword at all, the default
        is to determine the coding systems from the data.

    `:noquery query-flag`
    :   Initialize the process query flag to query-flag.
        See [Querying Before Exit](#Query-Before-Exit).

    `:filter filter`
    :   Initialize the process filter to filter.

    `:filter-multibyte multibyte`
    :   If multibyte is non-`nil`, strings given to the process
        filter are multibyte, otherwise they are unibyte. The default is `t`.

    `:sentinel sentinel`
    :   Initialize the process sentinel to sentinel.

    `:log log`
    :   Initialize the log function of a server process to log. The log
        function is called each time the server accepts a network connection
        from a client. The arguments passed to the log function are
        server, connection, and message; where server
        is the server process, connection is the new process for the
        connection, and message is a string describing what has
        happened.

    `:plist plist`
    :   Initialize the process plist to plist.

    The original argument list, modified with the actual connection
    information, is available via the `process-contact` function.

---

Next: [Testing Availability of Network Features](#Network-Feature-Testing), Previous: [`make-network-process`](#Network-Processes), Up: [Low-Level Network Access](#Low_002dLevel-Network)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.17.2 Network Options [¶](#Network-Options-1)

The following network options can be specified when you create a
network process. Except for `:reuseaddr`, you can also set or
modify these options later, using `set-network-process-option`.

For a server process, the options specified with
`make-network-process` are not inherited by the client
connections, so you will need to set the necessary options for each
child connection as it is created.

`:bindtodevice device-name`
:   If device-name is a non-empty string identifying a network
    interface name (see `network-interface-list`), only handle
    packets received on that interface. If device-name is `nil`
    (the default), handle packets received on any interface.

    Using this option may require special privileges on some systems.

`:broadcast broadcast-flag`
:   If broadcast-flag is non-`nil` for a datagram process, the
    process will receive datagram packet sent to a broadcast address, and
    be able to send packets to a broadcast address. This is ignored for a stream
    connection.

`:dontroute dontroute-flag`
:   If dontroute-flag is non-`nil`, the process can only send
    to hosts on the same network as the local host.

`:keepalive keepalive-flag`
:   If keepalive-flag is non-`nil` for a stream connection,
    enable exchange of low-level keep-alive messages.

`:linger linger-arg`
:   If linger-arg is non-`nil`, wait for successful
    transmission of all queued packets on the connection before it is
    deleted (see `delete-process`). If linger-arg is an
    integer, it specifies the maximum time in seconds to wait for queued
    packets to be sent before closing the connection. The default is
    `nil`, which means to discard unsent queued packets when the
    process is deleted.

`:oobinline oobinline-flag`
:   If oobinline-flag is non-`nil` for a stream connection,
    receive out-of-band data in the normal data stream. Otherwise, ignore
    out-of-band data.

`:priority priority`
:   Set the priority for packets sent on this connection to the integer
    priority. The interpretation of this number is protocol
    specific; such as setting the TOS (type of service) field on IP
    packets sent on this connection. It may also have system dependent
    effects, such as selecting a specific output queue on the network
    interface.

`:reuseaddr reuseaddr-flag`
:   If reuseaddr-flag is non-`nil` (the default) for a stream
    server process, allow this server to reuse a specific port number (see
    `:service`), unless another process on this host is already
    listening on that port. If reuseaddr-flag is `nil`, there
    may be a period of time after the last use of that port (by any
    process on the host) where it is not possible to make a new server on
    that port.

`:nodelay nodelay-flag`
:   If nodelay-flag is non-`nil`, the `TCP_NODELAY` option
    is enabled on the socket. This disables the Nagle algorithm, meaning
    that network segments are sent as soon as possible, even when they
    contain little data. This reduces network latency on the network
    connection, but can lead to many small packets being sent.

Function: **set-network-process-option** process option value &optional no-error [¶](#index-set_002dnetwork_002dprocess_002doption)
:   This function sets or modifies a network option for network process
    process. The accepted options and values are as for
    `make-network-process`. If no-error is non-`nil`,
    this function returns `nil` instead of signaling an error if
    option is not a supported option. If the function successfully
    completes, it returns `t`.

    The current setting of an option is available via the
    `process-contact` function.

---

Previous: [Network Options](#Network-Options), Up: [Low-Level Network Access](#Low_002dLevel-Network)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.17.3 Testing Availability of Network Features [¶](#Testing-Availability-of-Network-Features)

To test for the availability of a given network feature, use
`featurep` like this:

```
(featurep 'make-network-process '(keyword value))
```

The result of this form is `t` if it works to specify
keyword with value value in `make-network-process`.
Here are some of the keyword—value pairs you can test in
this way.

`(:nowait t)`
:   Non-`nil` if non-blocking connect is supported.

`(:type datagram)`
:   Non-`nil` if datagrams are supported.

`(:family local)`
:   Non-`nil` if local (a.k.a. “UNIX domain”) sockets are supported.

`(:family ipv6)`
:   Non-`nil` if IPv6 is supported.

`(:service t)`
:   Non-`nil` if the system can select the port for a server.

To test for the availability of a given network option, use
`featurep` like this:

```
(featurep 'make-network-process 'keyword)
```

The accepted keyword values are `:bindtodevice`, etc.
For the complete list, see [Network Options](#Network-Options). This form returns
non-`nil` if that particular network option is supported by
`make-network-process` (or `set-network-process-option`).

---

Next: [Communicating with Serial Ports](#Serial-Ports), Previous: [Low-Level Network Access](#Low_002dLevel-Network), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.18 Misc Network Facilities [¶](#Misc-Network-Facilities)

These additional functions are useful for creating and operating
on network connections. Note that they are supported only on some
systems.

Function: **network-interface-list** &optional full family [¶](#index-network_002dinterface_002dlist)
:   This function returns a list describing the network interfaces of the
    machine you are using. The value is an alist whose elements have the
    form `(ifname . address)`. ifname is a string
    naming the interface, address has the same form as the
    local-address and remote-address arguments to
    `make-network-process`, i.e. a vector of integers. By default
    both IPv4 and IPv6 addresses are returned if possible.

    Optional argument full non-`nil` means to instead return a
    list of one or more elements of the form `(ifname addr bcast netmask)`. ifname is a non-unique
    string naming the interface. addr, bcast, and
    netmask are vectors of integers detailing the IP address,
    broadcast address, and network mask.

    Optional argument family specified as symbol `ipv4` or
    `ipv6` restricts the returned information to IPv4 and IPv6
    addresses respectively, independently of the value of full.
    Specifying `ipv6` when IPv6 support is not available will result
    in an error being signaled.

    Some examples:

    ```
    (network-interface-list) ⇒
    (("vmnet8" .
      [172 16 76 1 0])
     ("vmnet1" .
      [172 16 206 1 0])
     ("lo0" .
      [65152 0 0 0 0 0 0 1 0])
     ("lo0" .
      [0 0 0 0 0 0 0 1 0])
     ("lo0" .
      [127 0 0 1 0]))
    ```

    ```
    (network-interface-list t) ⇒
    (("vmnet8"
      [172 16 76 1 0]
      [172 16 76 255 0]
      [255 255 255 0 0])
     ("vmnet1"
      [172 16 206 1 0]
      [172 16 206 255 0]
      [255 255 255 0 0])
     ("lo0"
      [65152 0 0 0 0 0 0 1 0]
      [65152 0 0 0 65535 65535 65535 65535 0]
      [65535 65535 65535 65535 0 0 0 0 0])
     ("lo0"
      [0 0 0 0 0 0 0 1 0]
      [0 0 0 0 0 0 0 1 0]
      [65535 65535 65535 65535 65535 65535 65535 65535 0])
     ("lo0"
      [127 0 0 1 0]
      [127 255 255 255 0]
      [255 0 0 0 0]))
    ```

Function: **network-interface-info** ifname [¶](#index-network_002dinterface_002dinfo)
:   This function returns information about the network interface named
    ifname. The value is a list of the form
    `(addr bcast netmask hwaddr flags)`.

    addr
    :   The Internet protocol address.

    bcast
    :   The broadcast address.

    netmask
    :   The network mask.

    hwaddr
    :   The layer 2 address (Ethernet MAC address, for instance).

    flags
    :   The current flags of the interface.

    Note that this function returns only IPv4 information.

Function: **format-network-address** address &optional omit-port [¶](#index-format_002dnetwork_002daddress)
:   This function converts the Lisp representation of a network address to
    a string.

    A five-element vector `[a b c d p]`
    represents an IPv4 address a.b.c.d and port
    number p. `format-network-address` converts that to the
    string `"a.b.c.d:p"`.

    A nine-element vector `[a b c d e
    f g h p]` represents an IPv6 address along
    with a port number. `format-network-address` converts that to
    the string
    `"[a:b:c:d:e:f:g:h]:p"`.

    If the vector does not include the port number, p, or if
    omit-port is non-`nil`, the result does not include the
    `:p` suffix.

Function: **network-lookup-address-info** name &optional family hints [¶](#index-network_002dlookup_002daddress_002dinfo)
:   This function perform hostname lookups on name, which is
    expected to be an ASCII-only string, otherwise it signals an error. Call
    `puny-encode-domain` on name first if you wish to lookup
    internationalized hostnames.

    If successful, this function returns a list of Lisp representations of network
    addresses (see [`make-network-process`](#Network-Processes), for a description of the
    format), otherwise return `nil`. In the latter case, it also logs
    an error message hopefully explaining what went wrong.

    By default, this function attempts both IPv4 and IPv6 lookups. The
    optional argument family controls this behavior, specifying the
    symbol `ipv4` or `ipv6` restricts lookups to IPv4 and IPv6
    respectively.

    If optional argument hints is `numeric`, the function
    treats the name as a numerical IP address (and does not perform DNS
    lookups). This can be used to check whether a string is a valid
    numerical representation of an IP address, or to convert a numerical
    string to its canonical representation. e.g.

    ```
    (network-lookup-address-info "127.1" 'ipv4 'numeric)
        ⇒ ([127 0 0 1 0])

    (network-lookup-address-info "::1" nil 'numeric)
        ⇒ ([0 0 0 0 0 0 0 1 0])
    ```

    Be warned that there are some surprising valid forms,
    especially for IPv4, e.g ‘`0xe3010203`’ and ‘`0343.1.2.3`’ are both
    valid, as are ‘`0`’ and ‘`1`’ (but they are invalid for IPv6).

---

Next: [Packing and Unpacking Byte Arrays](#Byte-Packing), Previous: [Misc Network Facilities](#Misc-Network), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
