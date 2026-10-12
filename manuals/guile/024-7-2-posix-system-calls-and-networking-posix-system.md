### 7.2 POSIX System Calls and Networking [¶](#POSIX-System-Calls-and-Networking)

* [POSIX Interface Conventions](#Conventions)
* [Ports and File Descriptors](#Ports-and-File-Descriptors)
* [File System](#File-System)
* [User Information](#User-Information)
* [Time](#Time)
* [Runtime Environment](#Runtime-Environment)
* [Processes](#Processes)
* [Signals](#Signals)
* [Terminals and Ptys](#Terminals-and-Ptys)
* [Pipes](#Pipes)
* [Networking](#Networking)
* [System Identification](#System-Identification)
* [Locales](#Locales)
* [Encryption](#Encryption)

---

Next: [Ports and File Descriptors](#Ports-and-File-Descriptors), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.1 POSIX Interface Conventions [¶](#POSIX-Interface-Conventions)

These interfaces provide access to operating system facilities.
They provide a simple wrapping around the underlying C interfaces
to make usage from Scheme more convenient. They are also used
to implement the Guile port of scsh (see [The Scheme shell (scsh)](#The-Scheme-shell-_0028scsh_0029)).

Generally there is a single procedure for each corresponding Unix
facility. There are some exceptions, such as procedures implemented for
speed and convenience in Scheme with no primitive Unix equivalent,
e.g. `copy-file`.

The interfaces are intended as far as possible to be portable across
different versions of Unix. In some cases procedures which can’t be
implemented on particular systems may become no-ops, or perform limited
actions. In other cases they may throw errors.

General naming conventions are as follows:

* The Scheme name is often identical to the name of the underlying Unix
  facility.
* Underscores in Unix procedure names are converted to hyphens.
* Procedures which destructively modify Scheme data have exclamation
  marks appended, e.g., `recv!`.
* Predicates (returning only `#t` or `#f`) have question marks
  appended, e.g., `access?`.
* Some names are changed to avoid conflict with dissimilar interfaces
  defined by scsh, e.g., `primitive-fork`.
* Unix preprocessor names such as `EPERM` or `R_OK` are converted
  to Scheme variables of the same name (underscores are not replaced
  with hyphens).

Unexpected conditions are generally handled by raising exceptions.
There are a few procedures which return a special value if they don’t
succeed, e.g., `getenv` returns `#f` if it the requested
string is not found in the environment. These cases are noted in
the documentation.

For ways to deal with exceptions, see [Exceptions](#Exceptions).

Errors which the C library would report by returning a null pointer or
through some other means are reported by raising a `system-error`
exception with `scm-error` (see [Procedures for Signaling Errors](#Error-Reporting)). The
data parameter is a list containing the Unix `errno` value
(an integer). For example,

```
(define (my-handler key func fmt fmtargs data)
  (display key) (newline)
  (display func) (newline)
  (apply format #t fmt fmtargs) (newline)
  (display data) (newline))

(catch 'system-error
  (lambda () (dup2 -123 -456))
  my-handler)

⊣
system-error
dup2
Bad file descriptor
(9)
```

  

Function: **system-error-errno** arglist [¶](#index-system_002derror_002derrno)
:   Return the `errno` value from a list which is the arguments to an
    exception handler. If the exception is not a `system-error`,
    then the return is `#f`. For example,

    ```
    (catch
     'system-error
     (lambda ()
       (mkdir "/this-ought-to-fail-if-I'm-not-root"))
     (lambda stuff
       (let ((errno (system-error-errno stuff)))
         (cond
          ((= errno EACCES)
           (display "You're not allowed to do that."))
          ((= errno EEXIST)
           (display "Already exists."))
          (#t
           (display (strerror errno))))
         (newline))))
    ```

---

Next: [File System](#File-System), Previous: [POSIX Interface Conventions](#Conventions), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.2 Ports and File Descriptors [¶](#Ports-and-File-Descriptors-1)

Conventions generally follow those of scsh, [The Scheme shell (scsh)](#The-Scheme-shell-_0028scsh_0029).

Each open file port has an associated operating system file descriptor.
File descriptors are generally not useful in Scheme programs; however
they may be needed when interfacing with foreign code and the Unix
environment.

A file descriptor can be extracted from a port and a new port can be
created from a file descriptor. However a file descriptor is just an
integer and the garbage collector doesn’t recognize it as a reference
to the port. If all other references to the port were dropped, then
it’s likely that the garbage collector would free the port, with the
side-effect of closing the file descriptor prematurely.

To assist the programmer in avoiding this problem, each port has an
associated *revealed count* which can be used to keep track of how many
times the underlying file descriptor has been stored in other places.
If a port’s revealed count is greater than zero, the file descriptor
will not be closed when the port is garbage collected. A programmer
can therefore ensure that the revealed count will be greater than
zero if the file descriptor is needed elsewhere.

For the simple case where a file descriptor is “imported” once to become
a port, it does not matter if the file descriptor is closed when the
port is garbage collected. There is no need to maintain a revealed
count. Likewise when “exporting” a file descriptor to the external
environment, setting the revealed count is not required provided the
port is kept open (i.e., is pointed to by a live Scheme binding) while
the file descriptor is in use.

To correspond with traditional Unix behavior, three file descriptors
(0, 1, and 2) are automatically imported when a program starts up and
assigned to the initial values of the current/standard input, output,
and error ports, respectively. The revealed count for each is
initially set to one, so that dropping references to one of these
ports will not result in its garbage collection: it could be retrieved
with `fdopen` or `fdes->ports`.

Guile’s ports can be buffered. This means that writing a byte to a file
port goes to the internal buffer first, and only when the buffer is full
(or the user invokes `force-output` on the port) is the data
actually written to the file descriptor. Likewise on input, bytes are
read in from the file descriptor in blocks and placed in a buffer.
Reading a character via `read-char` first goes to the buffer,
filling it as needed. Usually read buffering is more or less
transparent, but write buffering can sometimes cause writes to be
delayed unexpectedly, if you forget to call `force-output`.
See [Buffering](#Buffering), for more on how to control port buffers.

Note however that some procedures (e.g., `recv!`) will accept ports
as arguments, but will actually operate directly on the file descriptor
underlying the port. Any port buffering is ignored, including the
buffer which implements `peek-char` and `unread-char`.

Scheme Procedure: **port-revealed** port [¶](#index-port_002drevealed)

C Function: **scm\_port\_revealed** (port) [¶](#index-scm_005fport_005frevealed)
:   Return the revealed count for port.

Scheme Procedure: **set-port-revealed!** port rcount [¶](#index-set_002dport_002drevealed_0021)

C Function: **scm\_set\_port\_revealed\_x** (port, rcount) [¶](#index-scm_005fset_005fport_005frevealed_005fx)
:   Sets the revealed count for a port to rcount.
    The return value is unspecified.

Scheme Procedure: **fileno** port [¶](#index-fileno)

C Function: **scm\_fileno** (port) [¶](#index-scm_005ffileno)
:   Return the integer file descriptor underlying port. Does
    not change its revealed count.

Scheme Procedure: **port->fdes** port [¶](#index-port_002d_003efdes)
:   Returns the integer file descriptor underlying port. As a
    side effect the revealed count of port is incremented.

Scheme Procedure: **fdopen** fdes modes [¶](#index-fdopen)

C Function: **scm\_fdopen** (fdes, modes) [¶](#index-scm_005ffdopen)
:   Return a new port based on the file descriptor fdes. Modes are
    given by the string modes. The revealed count of the port is
    initialized to zero. The modes string is the same as that
    accepted by `open-file` (see [open-file](#File-Ports)).

Scheme Procedure: **fdes->ports** fdes [¶](#index-fdes_002d_003eports)

C Function: **scm\_fdes\_to\_ports** (fdes) [¶](#index-scm_005ffdes_005fto_005fports)
:   Return a list of existing ports which have fdes as an
    underlying file descriptor, without changing their revealed
    counts.

Scheme Procedure: **fdes->inport** fdes [¶](#index-fdes_002d_003einport)
:   Returns an existing input port which has fdes as its underlying file
    descriptor, if one exists, and increments its revealed count.
    Otherwise, returns a new input port with a revealed count of 1.

Scheme Procedure: **fdes->outport** fdes [¶](#index-fdes_002d_003eoutport)
:   Returns an existing output port which has fdes as its underlying file
    descriptor, if one exists, and increments its revealed count.
    Otherwise, returns a new output port with a revealed count of 1.

Scheme Procedure: **primitive-move->fdes** port fdes [¶](#index-primitive_002dmove_002d_003efdes)

C Function: **scm\_primitive\_move\_to\_fdes** (port, fdes) [¶](#index-scm_005fprimitive_005fmove_005fto_005ffdes)
:   Moves the underlying file descriptor for port to the integer
    value fdes without changing the revealed count of port.
    Any other ports already using this descriptor will be automatically
    shifted to new descriptors and their revealed counts reset to zero.
    The return value is `#f` if the file descriptor already had the
    required value or `#t` if it was moved.

Scheme Procedure: **move->fdes** port fdes [¶](#index-move_002d_003efdes)
:   Moves the underlying file descriptor for port to the integer
    value fdes and sets its revealed count to one. Any other ports
    already using this descriptor will be automatically
    shifted to new descriptors and their revealed counts reset to zero.
    The return value is unspecified.

Scheme Procedure: **release-port-handle** port [¶](#index-release_002dport_002dhandle)
:   Decrements the revealed count for a port.

Scheme Procedure: **fsync** port\_or\_fd [¶](#index-fsync)

C Function: **scm\_fsync** (port\_or\_fd) [¶](#index-scm_005ffsync)
:   Copies any unwritten data for the specified output file descriptor to disk.
    If port\_or\_fd is a port, its buffer is flushed before the underlying
    file descriptor is fsync’d.
    The return value is unspecified.

Scheme Procedure: **open** path flags [mode] [¶](#index-open)

C Function: **scm\_open** (path, flags, mode) [¶](#index-scm_005fopen)
:   Open the file named by path for reading and/or writing.
    flags is an integer specifying how the file should be opened.
    mode is an integer specifying the permission bits of the file,
    if it needs to be created, before the umask (see [Processes](#Processes)) is
    applied. The default is 666 (Unix itself has no default).

    flags can be constructed by combining variables using `logior`.
    Basic flags are:

    Variable: **O\_RDONLY** [¶](#index-O_005fRDONLY)
    :   Open the file read-only.

    Variable: **O\_WRONLY** [¶](#index-O_005fWRONLY)
    :   Open the file write-only.

    Variable: **O\_RDWR** [¶](#index-O_005fRDWR)
    :   Open the file read/write.

    Variable: **O\_APPEND** [¶](#index-O_005fAPPEND)
    :   Append to the file instead of truncating.

    Variable: **O\_CREAT** [¶](#index-O_005fCREAT)
    :   Create the file if it does not already exist.

    See [File Status Flags](/libc/latest/en/libc.html#File-Status-Flags) in The GNU C Library Reference Manual,
    for additional flags.

Scheme Procedure: **openat** dir path flags [mode] [¶](#index-openat)

C Function: **scm\_openat** (dir, path, flags, mode) [¶](#index-scm_005fopenat)
:   Similar to `open`, but resolve the file name path
    relative to the directory referred to by the file port dir
    instead.

Scheme Procedure: **open-fdes** path flags [mode] [¶](#index-open_002dfdes)

C Function: **scm\_open\_fdes** (path, flags, mode) [¶](#index-scm_005fopen_005ffdes)
:   Similar to `open` but return a file descriptor instead of
    a port.

Scheme Procedure: **open-fdes-at** dir path flags [mode] [¶](#index-open_002dfdes_002dat)

C Function: **scm\_open\_fdes\_at** (dir, path, flags, mode) [¶](#index-scm_005fopen_005ffdes_005fat)
:   Similar to `openat`, but return a file descriptor instead
    of a port.

Scheme Procedure: **close** fd\_or\_port [¶](#index-close)

C Function: **scm\_close** (fd\_or\_port) [¶](#index-scm_005fclose)
:   Similar to `close-port` (see [close-port](#Ports)),
    but also works on file descriptors. A side
    effect of closing a file descriptor is that any ports using that file
    descriptor are moved to a different file descriptor and have
    their revealed counts set to zero.

Scheme Procedure: **close-fdes** fd [¶](#index-close_002dfdes)

C Function: **scm\_close\_fdes** (fd) [¶](#index-scm_005fclose_005ffdes)
:   A simple wrapper for the `close` system call. Close file
    descriptor fd, which must be an integer. Unlike `close`,
    the file descriptor will be closed even if a port is using it. The
    return value is unspecified.

Scheme Procedure: **pipe** [flags] [¶](#index-pipe-2)

C Function: **scm\_pipe** () [¶](#index-scm_005fpipe)
:   Return a newly created pipe: a pair of ports which are linked together
    on the local machine. The CAR is the input port and the
    CDR is the output port. Data written (and flushed) to the
    output port can be read from the input port. Pipes are commonly used
    for communication with a newly forked child process. The need to flush
    the output port can be avoided by making it unbuffered using
    `setvbuf` (see [Buffering](#Buffering)).

    Optionally, on systems that support it such as GNU/Linux and
    GNU/Hurd, flags can specify a bitwise-or of the following
    constants:

    `O_CLOEXEC`
    :   Mark the returned file descriptors as close-on-exec;

    `O_DIRECT`
    :   Create a pipe that performs input/output in “packet"
        mode—see `man 2 pipe` for details;

    `O_NONBLOCK`
    :   Set the `O_NONBLOCK` status flag (non-blocking input and
        output) on the file descriptors.

    On systems that do *not* support it, passing a non-zero
    flags value triggers a `system-error` exception.

    Variable: **PIPE\_BUF** [¶](#index-PIPE_005fBUF)
    :   A write of up to `PIPE_BUF` many bytes to a pipe is atomic,
        meaning when done it goes into the pipe instantaneously and as a
        contiguous block (see [Atomicity of Pipe I/O](/libc/latest/en/libc.html#Pipe-Atomicity) in The GNU C Library Reference Manual).

    Note that the output port is likely to block if too much data has been
    written but not yet read from the input port. Typically the capacity
    is `PIPE_BUF` bytes.

The next group of procedures perform a `dup2`
system call, if newfd (an
integer) is supplied, otherwise a `dup`. The file descriptor to be
duplicated can be supplied as an integer or contained in a port. The
type of value returned varies depending on which procedure is used.

All procedures also have the side effect when performing `dup2` that any
ports using newfd are moved to a different file descriptor and have
their revealed counts set to zero.

Scheme Procedure: **dup->fdes** fd\_or\_port [fd] [¶](#index-dup_002d_003efdes)

C Function: **scm\_dup\_to\_fdes** (fd\_or\_port, fd) [¶](#index-scm_005fdup_005fto_005ffdes)
:   Return a new integer file descriptor referring to the open file
    designated by fd\_or\_port, which must be either an open
    file port or a file descriptor.

Scheme Procedure: **dup->inport** port/fd [newfd] [¶](#index-dup_002d_003einport)
:   Returns a new input port using the new file descriptor.

Scheme Procedure: **dup->outport** port/fd [newfd] [¶](#index-dup_002d_003eoutport)
:   Returns a new output port using the new file descriptor.

Scheme Procedure: **dup** port/fd [newfd] [¶](#index-dup)
:   Returns a new port if port/fd is a port, with the same mode as the
    supplied port, otherwise returns an integer file descriptor.

Scheme Procedure: **dup->port** port/fd mode [newfd] [¶](#index-dup_002d_003eport)
:   Returns a new port using the new file descriptor. mode supplies a
    mode string for the port (see [open-file](#File-Ports)).

Scheme Procedure: **duplicate-port** port modes [¶](#index-duplicate_002dport)
:   Returns a new port which is opened on a duplicate of the file
    descriptor underlying port, with mode string modes
    as for [open-file](#File-Ports). The two ports
    will share a file position and file status flags.

    Unexpected behavior can result if both ports are subsequently used
    and the original and/or duplicate ports are buffered.
    The mode string can include `0` to obtain an unbuffered duplicate
    port.

    This procedure is equivalent to `(dup->port port modes)`.

Scheme Procedure: **redirect-port** old\_port new\_port [¶](#index-redirect_002dport)

C Function: **scm\_redirect\_port** (old\_port, new\_port) [¶](#index-scm_005fredirect_005fport)
:   This procedure takes two ports and duplicates the underlying file
    descriptor from old\_port into new\_port. The
    current file descriptor in new\_port will be closed.
    After the redirection the two ports will share a file position
    and file status flags.

    The return value is unspecified.

    Unexpected behavior can result if both ports are subsequently used
    and the original and/or duplicate ports are buffered.

    This procedure does not have any side effects on other ports or
    revealed counts.

Scheme Procedure: **dup2** oldfd newfd [¶](#index-dup2)

C Function: **scm\_dup2** (oldfd, newfd) [¶](#index-scm_005fdup2)
:   A simple wrapper for the `dup2` system call.
    Copies the file descriptor oldfd to descriptor
    number newfd, replacing the previous meaning
    of newfd. Both oldfd and newfd must
    be integers.
    Unlike for `dup->fdes` or `primitive-move->fdes`, no attempt
    is made to move away ports which are using newfd.
    The return value is unspecified.

Scheme Procedure: **port-for-each** proc [¶](#index-port_002dfor_002deach)

C Function: **scm\_port\_for\_each** (SCM proc) [¶](#index-scm_005fport_005ffor_005feach)

C Function: **scm\_c\_port\_for\_each** (void (\*proc)(void \*, SCM), void \*data) [¶](#index-scm_005fc_005fport_005ffor_005feach)
:   Apply proc to each port in the Guile port table
    (FIXME: what is the Guile port table?)
    in turn. The return value is unspecified. More specifically,
    proc is applied exactly once to every port that exists in the
    system at the time `port-for-each` is invoked. Changes to the
    port table while `port-for-each` is running have no effect as far
    as `port-for-each` is concerned.

    The C function `scm_port_for_each` takes a Scheme procedure
    encoded as a `SCM` value, while `scm_c_port_for_each` takes
    a pointer to a C function and passes along a arbitrary data
    cookie.

Scheme Procedure: **fcntl** port/fd cmd [value] [¶](#index-fcntl)

C Function: **scm\_fcntl** (object, cmd, value) [¶](#index-scm_005ffcntl)
:   Apply cmd on port/fd, either a port or file descriptor.
    The value argument is used by the `SET` commands described
    below, it’s an integer value.

    Values for cmd are:

    Variable: **F\_DUPFD** [¶](#index-F_005fDUPFD)
    :   Duplicate the file descriptor, the same as `dup->fdes` above
        does.

    Variable: **F\_GETFD** [¶](#index-F_005fGETFD)

    Variable: **F\_SETFD** [¶](#index-F_005fSETFD)
    :   Get or set flags associated with the file descriptor. The only flag
        is the following,

        Variable: **FD\_CLOEXEC** [¶](#index-FD_005fCLOEXEC)
        :   “Close on exec”, meaning the file descriptor will be closed on an
            `exec` call (a successful such call). For example to set that
            flag,

            ```
            (fcntl port F_SETFD FD_CLOEXEC)
            ```

            Or better, set it but leave any other possible future flags unchanged,

            ```
            (fcntl port F_SETFD (logior FD_CLOEXEC
                                        (fcntl port F_GETFD)))
            ```

    Variable: **F\_GETFL** [¶](#index-F_005fGETFL)

    Variable: **F\_SETFL** [¶](#index-F_005fSETFL)
    :   Get or set flags associated with the open file. These flags are
        `O_RDONLY` etc described under `open` above.

        A common use is to set `O_NONBLOCK` on a network socket. The
        following sets that flag, and leaves other flags unchanged.

        ```
        (fcntl sock F_SETFL (logior O_NONBLOCK
                                    (fcntl sock F_GETFL)))
        ```

    Variable: **F\_GETOWN** [¶](#index-F_005fGETOWN)

    Variable: **F\_SETOWN** [¶](#index-F_005fSETOWN)
    :   Get or set the process ID of a socket’s owner, for `SIGIO` signals.

Scheme Procedure: **flock** file operation [¶](#index-flock)

C Function: **scm\_flock** (file, operation) [¶](#index-scm_005fflock)
:   Apply or remove an advisory lock on an open file.
    operation specifies the action to be done:

    Variable: **LOCK\_SH** [¶](#index-LOCK_005fSH)
    :   Shared lock. More than one process may hold a shared lock
        for a given file at a given time.

    Variable: **LOCK\_EX** [¶](#index-LOCK_005fEX)
    :   Exclusive lock. Only one process may hold an exclusive lock
        for a given file at a given time.

    Variable: **LOCK\_UN** [¶](#index-LOCK_005fUN)
    :   Unlock the file.

    Variable: **LOCK\_NB** [¶](#index-LOCK_005fNB)
    :   Don’t block when locking. This is combined with one of the other
        operations using `logior` (see [Bitwise Operations](#Bitwise-Operations)). If
        `flock` would block an `EWOULDBLOCK` error is thrown
        (see [POSIX Interface Conventions](#Conventions)).

    The return value is not specified. file may be an open
    file descriptor or an open file descriptor port.

    Note that `flock` does not lock files across NFS.

Scheme Procedure: **select** reads writes excepts [secs [usecs]] [¶](#index-select)

C Function: **scm\_select** (reads, writes, excepts, secs, usecs) [¶](#index-scm_005fselect)
:   This procedure has a variety of uses: waiting for the ability
    to provide input, accept output, or the existence of
    exceptional conditions on a collection of ports or file
    descriptors, or waiting for a timeout to occur.

    When an error occurs, this procedure throws a `system-error`
    exception (see [`system-error`](#Conventions)). Note that
    `select` may return early for other reasons, for example due to
    pending interrupts. See [Asynchronous Interrupts](#Asyncs), for more on interrupts.

    reads, writes and excepts can be lists or
    vectors, with each member a port or a file descriptor.
    The value returned is a list of three corresponding
    lists or vectors containing only the members which meet the
    specified requirement. The ability of port buffers to
    provide input or accept output is taken into account.
    Ordering of the input lists or vectors is not preserved.

    The optional arguments secs and usecs specify the
    timeout. Either secs can be specified alone, as
    either an integer or a real number, or both secs and
    usecs can be specified as integers, in which case
    usecs is an additional timeout expressed in
    microseconds. If secs is omitted or is `#f` then
    select will wait for as long as it takes for one of the other
    conditions to be satisfied.

    The scsh version of `select` differs as follows:
    Only vectors are accepted for the first three arguments.
    The usecs argument is not supported.
    Multiple values are returned instead of a list.
    Duplicates in the input vectors appear only once in output.
    An additional `select!` interface is provided.

While it is sometimes necessary to operate at the level of file
descriptors, this is an operation whose correctness can only be
considered as part of a whole program. So for example while the effects
of `(string-set! x 34 #\y)` are limited to the bits of code that
can access x, `(close-fdes 34)` mutates the state of the
entire process. In particular if another thread is using file
descriptor 34 then their state might be corrupted; and another thread
which opens a file might cause file descriptor 34 to be re-used, so that
corruption could manifest itself in a strange way.

However when working with file descriptors, it’s common to want to
associate information with the file descriptor, perhaps in a side table.
To support this use case and to allow user code to remove an association
when a file descriptor is closed, Guile offers *fdes finalizers*.

As the name indicates, fdes finalizers are finalizers – they can run in
response to garbage collection, and they can also run in response to
explicit calls to `close-port`, `close-fdes`, or the like. As
such they inherit many of the pitfalls of finalizers: they may be
invoked from concurrent threads, or not at all. See [Foreign Object Memory Management](#Foreign-Object-Memory-Management), for more on finalizers.

To use fdes finalizers, import their module;

```
(use-modules (ice-9 fdes-finalizers))
```

Scheme Procedure: **add-fdes-finalizer!** fdes finalizer [¶](#index-add_002dfdes_002dfinalizer_0021)

Scheme Procedure: **remove-fdes-finalizer!** fdes finalizer [¶](#index-remove_002dfdes_002dfinalizer_0021)
:   Add or remove a finalizer for fdes. A finalizer is a procedure
    that is called by Guile when a file descriptor is closed. The file
    descriptor being closed is passed as the one argument to the finalizer.
    If a finalizer has been added multiple times to a file descriptor, to
    remove it would require that number of calls to
    `remove-fdes-finalizer!`.

    The finalizers added to a file descriptor are called by Guile in an
    unspecified order, and their return values are ignored.

---

Next: [User Information](#User-Information), Previous: [Ports and File Descriptors](#Ports-and-File-Descriptors), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.3 File System [¶](#File-System-1)

These procedures allow querying and setting file system attributes
(such as owner,
permissions, sizes and types of files); deleting, copying, renaming and
linking files; creating and removing directories and querying their
contents; syncing the file system and creating special files.

Scheme Procedure: **access?** path how [¶](#index-access_003f)

C Function: **scm\_access** (path, how) [¶](#index-scm_005faccess)
:   Test accessibility of a file under the real UID and GID of the calling
    process. The return is `#t` if path exists and the
    permissions requested by how are all allowed, or `#f` if
    not.

    how is an integer which is one of the following values, or a
    bitwise-OR (`logior`) of multiple values.

    Variable: **R\_OK** [¶](#index-R_005fOK)
    :   Test for read permission.

    Variable: **W\_OK** [¶](#index-W_005fOK)
    :   Test for write permission.

    Variable: **X\_OK** [¶](#index-X_005fOK)
    :   Test for execute permission.

    Variable: **F\_OK** [¶](#index-F_005fOK)
    :   Test for existence of the file. This is implied by each of the other
        tests, so there’s no need to combine it with them.

    It’s important to note that `access?` does not simply indicate
    what will happen on attempting to read or write a file. In normal
    circumstances it does, but in a set-UID or set-GID program it doesn’t
    because `access?` tests the real ID, whereas an open or execute
    attempt uses the effective ID.

    A program which will never run set-UID/GID can ignore the difference
    between real and effective IDs, but for maximum generality, especially
    in library functions, it’s best not to use `access?` to predict
    the result of an open or execute, instead simply attempt that and
    catch any exception.

    The main use for `access?` is to let a set-UID/GID program
    determine what the invoking user would have been allowed to do,
    without the greater (or perhaps lesser) privileges afforded by the
    effective ID. For more on this, see [Testing File Access](/libc/latest/en/libc.html#Testing-File-Access) in The GNU C Library Reference Manual.

Scheme Procedure: **stat** object [exception-on-error?] [¶](#index-stat)

C Function: **scm\_stat** (object, exception\_on\_error) [¶](#index-scm_005fstat)
:   Return an object containing various information about the file
    determined by object. object can be a string containing
    a file name or a port or integer file descriptor which is open
    on a file (in which case `fstat` is used as the underlying
    system call).

    If the optional exception\_on\_error argument is true, which
    is the default, an exception will be raised if the underlying
    system call returns an error, for example if the file is not
    found or is not readable. Otherwise, an error will cause
    `stat` to return `#f`.

    The object returned by `stat` can be passed as a single
    parameter to the following procedures, all of which return
    integers:

    Scheme Procedure: **stat:dev** st [¶](#index-stat_003adev)
    :   The device number containing the file.

    Scheme Procedure: **stat:ino** st [¶](#index-stat_003aino)
    :   The file serial number, which distinguishes this file from all
        other files on the same device.

    Scheme Procedure: **stat:mode** st [¶](#index-stat_003amode)
    :   The mode of the file. This is an integer which incorporates file type
        information and file permission bits. See also `stat:type` and
        `stat:perms` below.

    Scheme Procedure: **stat:nlink** st [¶](#index-stat_003anlink)
    :   The number of hard links to the file.

    Scheme Procedure: **stat:uid** st [¶](#index-stat_003auid)
    :   The user ID of the file’s owner.

    Scheme Procedure: **stat:gid** st [¶](#index-stat_003agid)
    :   The group ID of the file.

    Scheme Procedure: **stat:rdev** st [¶](#index-stat_003ardev)
    :   Device ID; this entry is defined only for character or block special
        files. On some systems this field is not available at all, in which
        case `stat:rdev` returns `#f`.

    Scheme Procedure: **stat:size** st [¶](#index-stat_003asize)
    :   The size of a regular file in bytes.

    Scheme Procedure: **stat:atime** st [¶](#index-stat_003aatime)
    :   The last access time for the file, in seconds.

    Scheme Procedure: **stat:mtime** st [¶](#index-stat_003amtime)
    :   The last modification time for the file, in seconds.

    Scheme Procedure: **stat:ctime** st [¶](#index-stat_003actime)
    :   The last modification time for the attributes of the file, in seconds.

    Scheme Procedure: **stat:atimensec** st [¶](#index-stat_003aatimensec)

    Scheme Procedure: **stat:mtimensec** st [¶](#index-stat_003amtimensec)

    Scheme Procedure: **stat:ctimensec** st [¶](#index-stat_003actimensec)
    :   The fractional part of a file’s access, modification, or attribute modification
        time, in nanoseconds. Nanosecond timestamps are only available on some operating
        systems and file systems. If Guile cannot retrieve nanosecond-level timestamps
        for a file, these fields will be set to 0.

    Scheme Procedure: **stat:blksize** st [¶](#index-stat_003ablksize)
    :   The optimal block size for reading or writing the file, in bytes. On
        some systems this field is not available, in which case
        `stat:blksize` returns a sensible suggested block size.

    Scheme Procedure: **stat:blocks** st [¶](#index-stat_003ablocks)
    :   The amount of disk space that the file occupies measured in units of
        512 byte blocks. On some systems this field is not available, in
        which case `stat:blocks` returns `#f`.

    In addition, the following procedures return the information
    from `stat:mode` in a more convenient form:

    Scheme Procedure: **stat:type** st [¶](#index-stat_003atype)
    :   A symbol representing the type of file. Possible values are
        ‘`regular`’, ‘`directory`’, ‘`symlink`’,
        ‘`block-special`’, ‘`char-special`’, ‘`fifo`’, ‘`socket`’,
        and ‘`unknown`’.

    Scheme Procedure: **stat:perms** st [¶](#index-stat_003aperms)
    :   An integer representing the access permission bits.

Scheme Procedure: **lstat** path [¶](#index-lstat)

C Function: **scm\_lstat** (path) [¶](#index-scm_005flstat)
:   Similar to `stat`, but does not follow symbolic links, i.e.,
    it will return information about a symbolic link itself, not the
    file it points to. path must be a string.

Scheme Procedure: **statat** dir filename [flags] [¶](#index-statat)

C Function: **scm\_statat** dir filename flags [¶](#index-scm_005fstatat)
:   Like `stat`, but resolve filename relative to the directory
    referred to by the file port dir instead. The optional argument
    flags argument can be `AT_SYMLINK_NOFOLLOW`, in which case
    filename will not be dereferenced even if it is a symbolic link.

Scheme Procedure: **readlink** path [¶](#index-readlink)

C Function: **scm\_readlink** (path) [¶](#index-scm_005freadlink)
:   Return the value of the symbolic link named by path (a string, or
    a port if supported by the system), i.e., the file that the link points
    to.

    To read a symbolic link represented by a port, the symbolic link must
    have been opened with the `O_NOFOLLOW` and `O_PATH` flags.
    `(provided? 'readlink-port)` reports whether ports are supported.

Scheme Procedure: **chown** object owner group [¶](#index-chown)

C Function: **scm\_chown** (object, owner, group) [¶](#index-scm_005fchown)
:   Change the ownership and group of the file referred to by object
    to the integer values owner and group. object can
    be a string containing a file name or, if the platform supports
    `fchown` (see [File Owner](/libc/latest/en/libc.html#File-Owner) in The GNU C Library Reference
    Manual), a port or integer file descriptor which is open on the file.
    The return value is unspecified.

    If object is a symbolic link, either the
    ownership of the link or the ownership of the referenced file will be
    changed depending on the operating system (lchown is
    unsupported at present). If owner or group is specified
    as `-1`, then that ID is not changed.

Scheme Procedure: **chownat** dir name owner group [flags] [¶](#index-chownat)

C Function: **scm\_chownat** (dir, name, owner, group, flags) [¶](#index-scm_005fchownat)
:   Like `chown`, but modify the owner and/or group of
    the file named name in the directory referred to
    by the file port dir instead. The optional argument
    flags is a bitmask. If `AT_SYMLINK_NOFOLLOW` is
    present, then name will not be dereferenced if it is a
    symbolic link.

Scheme Procedure: **chmod** object mode [¶](#index-chmod)

C Function: **scm\_chmod** (object, mode) [¶](#index-scm_005fchmod)
:   Changes the permissions of the file referred to by object.
    object can be a string containing a file name or a port or integer file
    descriptor which is open on a file (in which case `fchmod` is used
    as the underlying system call).
    mode specifies
    the new permissions as a decimal number, e.g., `(chmod "foo" #o755)`.
    The return value is unspecified.

Scheme Procedure: **utime** object [actime [modtime [actimens [modtimens [flags]]]]] [¶](#index-utime)

C Function: **scm\_utime** (object, actime, modtime, actimens, modtimens, flags) [¶](#index-scm_005futime)
:   `utime` sets the access and modification times for the
    file named by object. If actime or modtime is
    not supplied, then the current time is used. actime and
    modtime must be integer time values as returned by the
    `current-time` procedure.

    object must be a file name or a port (if supported by the system).

    The optional actimens and modtimens are nanoseconds
    to add actime and modtime. Nanosecond precision is
    only supported on some combinations of file systems and operating
    systems.

    ```
    (utime "foo" (- (current-time) 3600))
    ```

    will set the access time to one hour in the past and the
    modification time to the current time.

    Last, flags may be either `0` or the
    `AT_SYMLINK_NOFOLLOW` constant, to set the time of
    object even if it is a symbolic link.

On GNU/Linux systems, at least when using the Linux kernel 5.10.46,
if object is a port, it may not be a symbolic link,
even if `AT_SYMLINK_NOFOLLOW` is set. This is either a bug
in Linux or Guile’s wrappers. The exact cause is unclear.

Scheme Procedure: **delete-file** str [¶](#index-delete_002dfile)

C Function: **scm\_delete\_file** (str) [¶](#index-scm_005fdelete_005ffile)
:   Deletes (or “unlinks”) the file whose path is specified by
    str.

Scheme Procedure: **delete-file-at** dir str [flags] [¶](#index-delete_002dfile_002dat)

C Function: **scm\_delete\_file\_at** (dir, str, flags) [¶](#index-scm_005fdelete_005ffile_005fat)
:   Like `unlink`, but resolve str relative to the
    directory referred to by the file port dir instead.

    The optional flags argument can be `AT_REMOVEDIR`,
    in which case `delete-file-at` will act like `rmdir` instead
    of `delete-file`. Why doesn’t POSIX have a `rmdirat` function
    for this instead? No idea!

Scheme Procedure: **copy-file** oldfile newfile [#:copy-on-write=’auto] [¶](#index-copy_002dfile)

C Function: **scm\_copy\_file** (oldfile, newfile) [¶](#index-scm_005fcopy_005ffile)
:   Copy the file specified by oldfile to newfile.
    The return value is unspecified.

    `#:copy-on-write` keyword argument determines whether copy-on-write
    copy should be attempted and the behavior in case of failure. Possible
    values are `'always` (attempt the copy-on-write, return error if it
    fails), `'auto` (attempt the copy-on-write, fallback to regular
    copy if it fails) and `'never` (perform the regular copy).

Scheme Procedure: **sendfile** out in count [offset] [¶](#index-sendfile)

C Function: **scm\_sendfile** (out, in, count, offset) [¶](#index-scm_005fsendfile)
:   Send count bytes from in to out, both of which
    must be either open file ports or file descriptors. When
    offset is omitted, start reading from in’s current
    position; otherwise, start reading at offset. Return
    the number of bytes actually sent.

    When in is a port, it is often preferable to specify offset,
    because in’s offset as a port may be different from the offset of
    its underlying file descriptor.

    On systems that support it, such as GNU/Linux, this procedure uses the
    `sendfile` libc function, which usually corresponds to a system
    call. This is faster than doing a series of `read` and
    `write` system calls. A typical application is to send a file over
    a socket.

    In some cases, the `sendfile` libc function may return
    `EINVAL` or `ENOSYS`. In that case, Guile’s `sendfile`
    procedure automatically falls back to doing a series of `read` and
    `write` calls.

    In other cases, the libc function may send fewer bytes than
    count—for instance because out is a slow or limited
    device, such as a pipe. When that happens, Guile’s `sendfile`
    automatically retries until exactly count bytes were sent or an
    error occurs.

Scheme Procedure: **rename-file** oldname newname [¶](#index-rename_002dfile)

C Function: **scm\_rename** (oldname, newname) [¶](#index-scm_005frename)
:   Renames the file specified by oldname to newname.
    The return value is unspecified.

Scheme Procedure: **rename-file-at** olddir oldname newdir newname [¶](#index-rename_002dfile_002dat)

C Function: **scm\_renameat** (olddir, oldname, newdir, newname) [¶](#index-scm_005frenameat)
:   Like `rename-file`, but when olddir or newdir is true,
    resolve oldname or newname relative to the directory
    specified by the file port olddir or newdir instead of the
    current working directory.

Scheme Procedure: **link** oldpath newpath [¶](#index-link)

C Function: **scm\_link** (oldpath, newpath) [¶](#index-scm_005flink)
:   Creates a new name newpath in the file system for the
    file named by oldpath. If oldpath is a symbolic
    link, the link may or may not be followed depending on the
    system.

Scheme Procedure: **symlink** oldpath newpath [¶](#index-symlink)

C Function: **scm\_symlink** (oldpath, newpath) [¶](#index-scm_005fsymlink)
:   Create a symbolic link named newpath with the value (i.e., pointing to)
    oldpath. The return value is unspecified.

Scheme Procedure: **symlinkat** dir oldpath newpath [¶](#index-symlinkat)

C Function: **scm\_symlinkat** (dir, oldpath, newpath) [¶](#index-scm_005fsymlinkat)
:   Like `symlink`, but resolve newpath relative to
    the directory referred to by the file port dir.

Scheme Procedure: **mkdir** path [mode] [¶](#index-mkdir)

C Function: **scm\_mkdir** (path, mode) [¶](#index-scm_005fmkdir)
:   Create a new directory named by path. If mode is omitted
    then the permissions of the directory are set to `#o777`
    masked with the current umask (see [`umask`](#Processes)).
    Otherwise they are set to the value specified with mode
    masked with the current umask.
    The return value is unspecified.

Scheme Procedure: **mkdirat** dir path [mode] [¶](#index-mkdirat)

C Function: **scm\_mkdirat** (dir, path, mode) [¶](#index-scm_005fmkdirat)
:   Like `mkdir`, but resolve path relative to the directory
    referred to by the file port dir instead.

Scheme Procedure: **rmdir** path [¶](#index-rmdir)

C Function: **scm\_rmdir** (path) [¶](#index-scm_005frmdir)
:   Remove the existing directory named by path. The directory must
    be empty for this to succeed. The return value is unspecified.

Scheme Procedure: **opendir** dirname [¶](#index-opendir)

C Function: **scm\_opendir** (dirname) [¶](#index-scm_005fopendir)
:   Open the directory specified by dirname and return a directory
    stream.

    Before using this and the procedures below, make sure to see the
    higher-level procedures for directory traversal that are available
    (see [File Tree Walk](#File-Tree-Walk)).

Scheme Procedure: **directory-stream?** object [¶](#index-directory_002dstream_003f)

C Function: **scm\_directory\_stream\_p** (object) [¶](#index-scm_005fdirectory_005fstream_005fp)
:   Return a boolean indicating whether object is a directory
    stream as returned by `opendir`.

Scheme Procedure: **readdir** stream [¶](#index-readdir)

C Function: **scm\_readdir** (stream) [¶](#index-scm_005freaddir)
:   Return (as a string) the next directory entry from the directory stream
    stream. If there is no remaining entry to be read then the
    end of file object is returned.

Scheme Procedure: **rewinddir** stream [¶](#index-rewinddir)

C Function: **scm\_rewinddir** (stream) [¶](#index-scm_005frewinddir)
:   Reset the directory port stream so that the next call to
    `readdir` will return the first directory entry.

Scheme Procedure: **closedir** stream [¶](#index-closedir)

C Function: **scm\_closedir** (stream) [¶](#index-scm_005fclosedir)
:   Close the directory stream stream.
    The return value is unspecified.

Here is an example showing how to display all the entries in a
directory:

```
(define dir (opendir "/usr/lib"))
(do ((entry (readdir dir) (readdir dir)))
    ((eof-object? entry))
  (display entry)(newline))
(closedir dir)
```

Scheme Procedure: **sync** [¶](#index-sync)

C Function: **scm\_sync** () [¶](#index-scm_005fsync)
:   Flush the operating system disk buffers.
    The return value is unspecified.

Scheme Procedure: **mknod** path type perms dev [¶](#index-mknod)

C Function: **scm\_mknod** (path, type, perms, dev) [¶](#index-scm_005fmknod)
:   Creates a new special file, such as a file corresponding to a device.
    path specifies the name of the file. type should be one
    of the following symbols: ‘`regular`’, ‘`directory`’,
    ‘`symlink`’, ‘`block-special`’, ‘`char-special`’,
    ‘`fifo`’, or ‘`socket`’. perms (an integer) specifies the
    file permissions. dev (an integer) specifies which device the
    special file refers to. Its exact interpretation depends on the kind
    of special file being created.

    E.g.,

    ```
    (mknod "/dev/fd0" 'block-special #o660 (+ (* 2 256) 2))
    ```

    The return value is unspecified.

Scheme Procedure: **tmpnam** [¶](#index-tmpnam)

C Function: **scm\_tmpnam** () [¶](#index-scm_005ftmpnam)
:   Return an auto-generated name of a temporary file, a file which
    doesn’t already exist. The name includes a path, it’s usually in
    `/tmp` but that’s system dependent.

    Care must be taken when using `tmpnam`. In between choosing the
    name and creating the file another program might use that name, or an
    attacker might even make it a symlink pointing at something important
    and causing you to overwrite that.

    The safe way is to create the file using `open` with
    `O_EXCL` to avoid any overwriting. A loop can try again with
    another name if the file exists (error `EEXIST`).
    `mkstemp` below does that.

Scheme Procedure: **mkstemp** tmpl [mode] [¶](#index-mkstemp)
:   Create a new unique file in the file system and return a new buffered
    port open for reading and writing to the file.

    tmpl is a string specifying where the file should be created: it
    must end with ‘`XXXXXX`’. The name of the newly created file will be
    the same as tmpl, but with those ‘`X`’s changed, and can be
    determined by calling `port-filename` on the returned port.

    Note that the newly created file is not deleted automatically by Guile;
    probably the caller should arrange to call `delete-file` when the
    file is no longer needed.

    POSIX doesn’t specify the permissions mode of the file. On GNU and most
    systems it’s `#o600`; an application can use `chmod` to relax
    that if desired. For example `#o666` less `umask`, which is
    usual for ordinary file creation,

    ```
    (let ((port (mkstemp "/tmp/myfile-XXXXXX")))
      (chmod port (logand #o666 (lognot (umask))))
      ...)
    ```

    The optional mode argument specifies a mode with which to open the
    new file, as a string in the same format that `open-file` takes.
    It defaults to `"w+"`.

Scheme Procedure: **tmpfile** [¶](#index-tmpfile)

C Function: **scm\_tmpfile** () [¶](#index-scm_005ftmpfile)
:   Return an input/output port to a unique temporary file
    named using the path prefix `P_tmpdir` defined in
    `stdio.h`.
    The file is automatically deleted when the port is closed
    or the program terminates.

Scheme Procedure: **mkdtemp** tmpl [¶](#index-mkdtemp)

C Function: **scm\_mkdtemp** (tmpl) [¶](#index-scm_005fmkdtemp)
:   Create a new directory named in accordance with the template string
    tmpl.

    tmpl is a string specifying the directory’s name. The last six
    characters of tmpl must be ‘`XXXXXX`’. Upon successful
    execution, the name of the new directory is returned which has the same
    form as tmpl but with the ‘`XXXXXX`’ characters modified to
    ensure the directory name is unique.

    The permissions of the directory created are OS dependent, but, are
    usually `#o700`.

    An error may be thrown if the template has the wrong format or if the
    directory cannot be created.

Scheme Procedure: **dirname** filename [¶](#index-dirname)

C Function: **scm\_dirname** (filename) [¶](#index-scm_005fdirname)
:   Return the directory name component of the file name
    filename. If filename does not contain a directory
    component, `.` is returned.

Scheme Procedure: **basename** filename [suffix] [¶](#index-basename)

C Function: **scm\_basename** (filename, suffix) [¶](#index-scm_005fbasename)
:   Return the base name of the file name filename. The
    base name is the file name without any directory components.
    If suffix is provided, and is equal to the end of
    basename, it is removed also.

    ```
    (basename "/tmp/test.xml" ".xml")
    ⇒ "test"
    ```

Scheme Procedure: **canonicalize-path** path [¶](#index-canonicalize_002dpath)

C Function: **scm\_canonicalize\_path** (path) [¶](#index-scm_005fcanonicalize_005fpath)
:   Return the canonical (absolute) path of path.
    A canonical path has no `.` or `..` components,
    nor any repeated path separators (`/`) nor symlinks.

    Raises an error if any component of path does not
    exist.

    ```
    (canonicalize-path "test.xml")
    ⇒ "/tmp/test.xml"
    ```

Scheme Procedure: **file-exists?** filename [¶](#index-file_002dexists_003f)
:   Return `#t` if the file named filename exists, `#f` if
    not.

Many operating systems, such as GNU, use `/` (forward slash) to
separate the components of a file name; any file name starting with
`/` is considered an *absolute file name*. These conventions
are specified by the POSIX Base Definitions, which refer to conforming
file names as “pathnames”. Some operating systems use a different
convention; in particular, Windows uses `\` (backslash) as the file
name separator, and also has the notion of *volume names* like
`C:\` for absolute file names. The following procedures and
variables provide support for portable file name manipulations.

Scheme Procedure: **system-file-name-convention** [¶](#index-system_002dfile_002dname_002dconvention)
:   Return either `posix` or `windows`, depending on
    what kind of system this Guile is running on.

Scheme Procedure: **file-name-separator?** c [¶](#index-file_002dname_002dseparator_003f)
:   Return true if character c is a file name separator on the host
    platform.

Scheme Procedure: **absolute-file-name?** file-name [¶](#index-absolute_002dfile_002dname_003f)
:   Return true if file-name denotes an absolute file name on the host
    platform.

Scheme Variable: **file-name-separator-string** [¶](#index-file_002dname_002dseparator_002dstring)
:   The preferred file name separator.

    Note that on MinGW builds for Windows, both `/` and `\` are
    valid separators. Thus, programs should not assume that
    `file-name-separator-string` is the *only* file name
    separator—e.g., when extracting the components of a file name.

Scheme Procedure: **in-vicinity** directory file [¶](#index-in_002dvicinity)
:   Concatenate directory and file, adding
    `file-name-separator-string` (by default slash) in between if it is
    not already present. This helps create file names.

---

Next: [Time](#Time), Previous: [File System](#File-System), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.4 User Information [¶](#User-Information-1)

The facilities in this section provide an interface to the user and
group database.
They should be used with care since they are not reentrant.

The following functions accept an object representing user information
and return a selected component:

Scheme Procedure: **passwd:name** pw [¶](#index-passwd_003aname)
:   The name of the userid.

Scheme Procedure: **passwd:passwd** pw [¶](#index-passwd_003apasswd)
:   The encrypted passwd.

Scheme Procedure: **passwd:uid** pw [¶](#index-passwd_003auid)
:   The user id number.

Scheme Procedure: **passwd:gid** pw [¶](#index-passwd_003agid)
:   The group id number.

Scheme Procedure: **passwd:gecos** pw [¶](#index-passwd_003agecos)
:   The full name.

Scheme Procedure: **passwd:dir** pw [¶](#index-passwd_003adir)
:   The home directory.

Scheme Procedure: **passwd:shell** pw [¶](#index-passwd_003ashell)
:   The login shell.

  

Scheme Procedure: **getpwuid** uid [¶](#index-getpwuid)
:   Look up an integer userid in the user database.

Scheme Procedure: **getpwnam** name [¶](#index-getpwnam)
:   Look up a user name string in the user database.

Scheme Procedure: **setpwent** [¶](#index-setpwent)
:   Initializes a stream used by `getpwent` to read from the user database.
    The next use of `getpwent` will return the first entry. The
    return value is unspecified.

Scheme Procedure: **getpwent** [¶](#index-getpwent)
:   Read the next entry in the user database stream. The return is a
    passwd user object as above, or `#f` when no more entries.

Scheme Procedure: **endpwent** [¶](#index-endpwent)
:   Closes the stream used by `getpwent`. The return value is unspecified.

Scheme Procedure: **setpw** [arg] [¶](#index-setpw)

C Function: **scm\_setpwent** (arg) [¶](#index-scm_005fsetpwent)
:   If called with a true argument, initialize or reset the password data
    stream. Otherwise, close the stream. The `setpwent` and
    `endpwent` procedures are implemented on top of this.

Scheme Procedure: **getpw** [user] [¶](#index-getpw)

C Function: **scm\_getpwuid** (user) [¶](#index-scm_005fgetpwuid)
:   Look up an entry in the user database. user can be an integer,
    a string, or omitted, giving the behavior of getpwuid, getpwnam
    or getpwent respectively.

The following functions accept an object representing group information
and return a selected component:

Scheme Procedure: **group:name** gr [¶](#index-group_003aname)
:   The group name.

Scheme Procedure: **group:passwd** gr [¶](#index-group_003apasswd)
:   The encrypted group password.

Scheme Procedure: **group:gid** gr [¶](#index-group_003agid)
:   The group id number.

Scheme Procedure: **group:mem** gr [¶](#index-group_003amem)
:   A list of userids which have this group as a supplementary group.

  

Scheme Procedure: **getgrgid** gid [¶](#index-getgrgid)
:   Look up an integer group id in the group database.

Scheme Procedure: **getgrnam** name [¶](#index-getgrnam)
:   Look up a group name in the group database.

Scheme Procedure: **setgrent** [¶](#index-setgrent)
:   Initializes a stream used by `getgrent` to read from the group database.
    The next use of `getgrent` will return the first entry.
    The return value is unspecified.

Scheme Procedure: **getgrent** [¶](#index-getgrent)
:   Return the next entry in the group database, using the stream set by
    `setgrent`.

Scheme Procedure: **endgrent** [¶](#index-endgrent)
:   Closes the stream used by `getgrent`.
    The return value is unspecified.

Scheme Procedure: **setgr** [arg] [¶](#index-setgr)

C Function: **scm\_setgrent** (arg) [¶](#index-scm_005fsetgrent)
:   If called with a true argument, initialize or reset the group data
    stream. Otherwise, close the stream. The `setgrent` and
    `endgrent` procedures are implemented on top of this.

Scheme Procedure: **getgr** [group] [¶](#index-getgr)

C Function: **scm\_getgrgid** (group) [¶](#index-scm_005fgetgrgid)
:   Look up an entry in the group database. group can be an integer,
    a string, or omitted, giving the behavior of getgrgid, getgrnam
    or getgrent respectively.

In addition to the accessor procedures for the user database, the
following shortcut procedure is also available.

Scheme Procedure: **getlogin** [¶](#index-getlogin)

C Function: **scm\_getlogin** () [¶](#index-scm_005fgetlogin)
:   Return a string containing the name of the user logged in on
    the controlling terminal of the process, or `#f` if this
    information cannot be obtained.

---

Next: [Runtime Environment](#Runtime-Environment), Previous: [User Information](#User-Information), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.5 Time [¶](#Time-1)

Scheme Procedure: **current-time** [¶](#index-current_002dtime)

C Function: **scm\_current\_time** () [¶](#index-scm_005fcurrent_005ftime)
:   Return the number of seconds since 1970-01-01 00:00:00 UTC,
    excluding leap seconds.

Scheme Procedure: **gettimeofday** [¶](#index-gettimeofday)

C Function: **scm\_gettimeofday** () [¶](#index-scm_005fgettimeofday)
:   Return a pair containing the number of seconds and microseconds
    since 1970-01-01 00:00:00 UTC, excluding leap seconds. Note:
    whether true microsecond resolution is available depends on the
    operating system.

The following procedures either accept an object representing a broken down
time and return a selected component, or accept an object representing
a broken down time and a value and set the component to the value.
The numbers in parentheses give the usual range.

Scheme Procedure: **tm:sec** tm [¶](#index-tm_003asec)

Scheme Procedure: **set-tm:sec** tm val [¶](#index-set_002dtm_003asec)
:   Seconds (0-59).

Scheme Procedure: **tm:min** tm [¶](#index-tm_003amin)

Scheme Procedure: **set-tm:min** tm val [¶](#index-set_002dtm_003amin)
:   Minutes (0-59).

Scheme Procedure: **tm:hour** tm [¶](#index-tm_003ahour)

Scheme Procedure: **set-tm:hour** tm val [¶](#index-set_002dtm_003ahour)
:   Hours (0-23).

Scheme Procedure: **tm:mday** tm [¶](#index-tm_003amday)

Scheme Procedure: **set-tm:mday** tm val [¶](#index-set_002dtm_003amday)
:   Day of the month (1-31).

Scheme Procedure: **tm:mon** tm [¶](#index-tm_003amon)

Scheme Procedure: **set-tm:mon** tm val [¶](#index-set_002dtm_003amon)
:   Month (0-11).

Scheme Procedure: **tm:year** tm [¶](#index-tm_003ayear)

Scheme Procedure: **set-tm:year** tm val [¶](#index-set_002dtm_003ayear)
:   Year (70-), the year minus 1900.

Scheme Procedure: **tm:wday** tm [¶](#index-tm_003awday)

Scheme Procedure: **set-tm:wday** tm val [¶](#index-set_002dtm_003awday)
:   Day of the week (0-6) with Sunday represented as 0.

Scheme Procedure: **tm:yday** tm [¶](#index-tm_003ayday)

Scheme Procedure: **set-tm:yday** tm val [¶](#index-set_002dtm_003ayday)
:   Day of the year (0-364, 365 in leap years).

Scheme Procedure: **tm:isdst** tm [¶](#index-tm_003aisdst)

Scheme Procedure: **set-tm:isdst** tm val [¶](#index-set_002dtm_003aisdst)
:   Daylight saving indicator (0 for “no”, greater than 0 for “yes”, less than
    0 for “unknown”).

Scheme Procedure: **tm:gmtoff** tm [¶](#index-tm_003agmtoff)

Scheme Procedure: **set-tm:gmtoff** tm val [¶](#index-set_002dtm_003agmtoff)
:   Time zone offset in seconds west of UTC (-46800 to 43200).
    For example on East coast USA (zone ‘`EST+5`’) this would be 18000
    (ie. *5\*60\*60*) in winter, or 14400
    (ie. *4\*60\*60*) during daylight savings.

    Note `tm:gmtoff` is not the same as `tm_gmtoff` in the C
    `tm` structure. `tm_gmtoff` is seconds east and hence the
    negative of the value here.

Scheme Procedure: **tm:zone** tm [¶](#index-tm_003azone)

Scheme Procedure: **set-tm:zone** tm val [¶](#index-set_002dtm_003azone)
:   Time zone label (a string), not necessarily unique.

  

Scheme Procedure: **localtime** time [zone] [¶](#index-localtime)

C Function: **scm\_localtime** (time, zone) [¶](#index-scm_005flocaltime)
:   Return an object representing the broken down components of
    time, an integer like the one returned by
    `current-time`. The time zone for the calculation is
    optionally specified by zone (a string), otherwise the
    `TZ` environment variable or the system default is used.

Scheme Procedure: **gmtime** time [¶](#index-gmtime)

C Function: **scm\_gmtime** (time) [¶](#index-scm_005fgmtime)
:   Return an object representing the broken down components of
    time, an integer like the one returned by
    `current-time`. The values are calculated for UTC.

Scheme Procedure: **mktime** sbd-time [zone] [¶](#index-mktime)

C Function: **scm\_mktime** (sbd\_time, zone) [¶](#index-scm_005fmktime)
:   For a broken down time object sbd-time, return a pair the
    `car` of which is an integer time like `current-time`, and
    the `cdr` of which is a new broken down time with normalized
    fields.

    zone is a timezone string, or the default is the `TZ`
    environment variable or the system default (see [Specifying the Time Zone with `TZ`](/libc/latest/en/libc.html#TZ-Variable) in GNU C Library Reference
    Manual). sbd-time is taken to be in that zone.

    The following fields of sbd-time are used: `tm:year`,
    `tm:mon`, `tm:mday`, `tm:hour`, `tm:min`,
    `tm:sec`, `tm:isdst`. The values can be outside their usual
    ranges. For example `tm:hour` normally goes up to 23, but a
    value say 33 would mean 9 the following day.

    `tm:isdst` in sbd-time says whether the time given is with
    daylight savings or not. This is ignored if zone doesn’t have
    any daylight savings adjustment amount.

    The broken down time in the return normalizes the values of
    sbd-time by bringing them into their usual ranges, and using the
    actual daylight savings rule for that time in zone (which may
    differ from what sbd-time had). The easiest way to think of
    this is that sbd-time plus zone converts to the integer
    UTC time, then a `localtime` is applied to get the normal
    presentation of that time, in zone.

Scheme Procedure: **tzset** [¶](#index-tzset)

C Function: **scm\_tzset** () [¶](#index-scm_005ftzset)
:   Initialize the timezone from the `TZ` environment variable
    or the system default. It’s not usually necessary to call this procedure
    since it’s done automatically by other procedures that depend on the
    timezone.

Scheme Procedure: **strftime** format tm [¶](#index-strftime)

C Function: **scm\_strftime** (format, tm) [¶](#index-scm_005fstrftime)
:   Return a string which is broken-down time structure tm formatted
    according to the given format string.

    format contains field specifications introduced by a ‘`%`’
    character. See [Formatting Calendar Time](/libc/latest/en/libc.html#Formatting-Calendar-Time) in The GNU C
    Library Reference Manual, or ‘`man 3 strftime`’, for the available
    formatting.

    ```
    (strftime "%c" (localtime (current-time)))
    ⇒ "Mon Mar 11 20:17:43 2002"
    ```

    If `setlocale` has been called (see [Locales](#Locales)), month and day
    names are from the current locale and in the locale character set.

Scheme Procedure: **strptime** format string [¶](#index-strptime)

C Function: **scm\_strptime** (format, string) [¶](#index-scm_005fstrptime)
:   Performs the reverse action to `strftime`, parsing
    string according to the specification supplied in
    format. The interpretation of month and day names is
    dependent on the current locale. The value returned is a pair.
    The CAR has an object with time components
    in the form returned by `localtime` or `gmtime`,
    but the time zone components
    are not usefully set.
    The CDR reports the number of characters from string
    which were used for the conversion.

Variable: **internal-time-units-per-second** [¶](#index-internal_002dtime_002dunits_002dper_002dsecond)
:   The value of this variable is the number of time units per second
    reported by the following procedures.

Scheme Procedure: **times** [¶](#index-times)

C Function: **scm\_times** () [¶](#index-scm_005ftimes)
:   Return an object with information about real and processor
    time. The following procedures accept such an object as an
    argument and return a selected component:

    Scheme Procedure: **tms:clock** tms [¶](#index-tms_003aclock)
    :   The current real time, expressed as time units relative to an
        arbitrary base.

    Scheme Procedure: **tms:utime** tms [¶](#index-tms_003autime)
    :   The CPU time units used by the calling process.

    Scheme Procedure: **tms:stime** tms [¶](#index-tms_003astime)
    :   The CPU time units used by the system on behalf of the calling
        process.

    Scheme Procedure: **tms:cutime** tms [¶](#index-tms_003acutime)
    :   The CPU time units used by terminated child processes of the
        calling process, whose status has been collected (e.g., using
        `waitpid`).

    Scheme Procedure: **tms:cstime** tms [¶](#index-tms_003acstime)
    :   Similarly, the CPU times units used by the system on behalf of
        terminated child processes.

Scheme Procedure: **get-internal-real-time** [¶](#index-get_002dinternal_002dreal_002dtime)

C Function: **scm\_get\_internal\_real\_time** () [¶](#index-scm_005fget_005finternal_005freal_005ftime)
:   Return the number of time units since the interpreter was
    started.

Scheme Procedure: **get-internal-run-time** [¶](#index-get_002dinternal_002drun_002dtime)

C Function: **scm\_get\_internal\_run\_time** () [¶](#index-scm_005fget_005finternal_005frun_005ftime)
:   Return the number of time units of processor time used by the
    interpreter. Both *system* and *user* time are
    included but subprocesses are not.

---

Next: [Processes](#Processes), Previous: [Time](#Time), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.6 Runtime Environment [¶](#Runtime-Environment-1)

Scheme Procedure: **program-arguments** [¶](#index-program_002darguments)

Scheme Procedure: **command-line** [¶](#index-command_002dline)

Scheme Procedure: **set-program-arguments** [¶](#index-set_002dprogram_002darguments)

C Function: **scm\_program\_arguments** () [¶](#index-scm_005fprogram_005farguments)

C Function: **scm\_set\_program\_arguments\_scm** (lst) [¶](#index-scm_005fset_005fprogram_005farguments_005fscm)
:   Get the command line arguments passed to Guile, or set new arguments.

    The arguments are a list of strings, the first of which is the invoked
    program name. This is just `"guile"` (or the executable path)
    when run interactively, or it’s the script name when running a script
    with `-s` (see [Invoking Guile](#Invoking-Guile)).

    ```
    guile -L /my/extra/dir -s foo.scm abc def

    (program-arguments) ⇒ ("foo.scm" "abc" "def")
    ```

    `set-program-arguments` allows a library module or similar to
    modify the arguments, for example to strip options it recognises,
    leaving the rest for the mainline.

    The argument list is held in a fluid, which means it’s separate for
    each thread. Neither the list nor the strings within it are copied at
    any point and normally should not be mutated.

    The two names `program-arguments` and `command-line` are an
    historical accident, they both do exactly the same thing. The name
    `scm_set_program_arguments_scm` has an extra `_scm` on the
    end to avoid clashing with the C function below.

C Function: `void` **scm\_set\_program\_arguments** `(int argc, char **argv, char *first)` [¶](#index-scm_005fset_005fprogram_005farguments)
:   Set the list of command line arguments for `program-arguments`
    and `command-line` above.

    argv is an array of null-terminated strings, as in a C
    `main` function. argc is the number of strings in
    argv, or if it’s negative then a `NULL` in argv marks
    its end.

    first is an extra string put at the start of the arguments, or
    `NULL` for no such extra. This is a convenient way to pass the
    program name after advancing argv to strip option arguments.
    Eg.

    ```
    {
      char *progname = argv[0];
      for (argv++; argv[0] != NULL && argv[0][0] == '-'; argv++)
        {
          /* munch option ... */
        }
      /* remaining args for scheme level use */
      scm_set_program_arguments (-1, argv, progname);
    }
    ```

    This sort of thing is often done at startup under
    `scm_boot_guile` with options handled at the C level removed.
    The given strings are all copied, so the C data is not accessed again
    once `scm_set_program_arguments` returns.

Scheme Procedure: **getenv** name [¶](#index-getenv)

C Function: **scm\_getenv** (name) [¶](#index-scm_005fgetenv)
:   Looks up the string name in the current environment. The return
    value is `#f` unless a string of the form `NAME=VALUE` is
    found, in which case the string `VALUE` is returned.

Scheme Procedure: **setenv** name value [¶](#index-setenv)
:   Modifies the environment of the current process, which is
    also the default environment inherited by child processes.

    If value is `#f`, then name is removed from the
    environment. Otherwise, the string name=value is added
    to the environment, replacing any existing string with name matching
    name.

    The return value is unspecified.

Scheme Procedure: **unsetenv** name [¶](#index-unsetenv)
:   Remove variable name from the environment. The
    name can not contain a ‘`=`’ character.

Scheme Procedure: **environ** [env] [¶](#index-environ)

C Function: **scm\_environ** (env) [¶](#index-scm_005fenviron)
:   If env is omitted, return the current environment (in the
    Unix sense) as a list of strings. Otherwise set the current
    environment, which is also the default environment for child
    processes, to the supplied list of strings. Each member of
    env should be of the form name=value and values of
    name should not be duplicated. If env is supplied
    then the return value is unspecified.

Scheme Procedure: **putenv** str [¶](#index-putenv)

C Function: **scm\_putenv** (str) [¶](#index-scm_005fputenv)
:   Modifies the environment of the current process, which is
    also the default environment inherited by child processes.

    If str is of the form `NAME=VALUE` then it will be written
    directly into the environment, replacing any existing environment string
    with
    name matching `NAME`. If str does not contain an equal
    sign, then any existing string with name matching str will
    be removed.

    The return value is unspecified.

---

Next: [Signals](#Signals), Previous: [Runtime Environment](#Runtime-Environment), Up: [POSIX System Calls and Networking](#POSIX)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.2.7 Processes [¶](#Processes-1)

Scheme Procedure: **chdir** str [¶](#index-chdir)

C Function: **scm\_chdir** (str) [¶](#index-scm_005fchdir)
:   Change the current working directory to str. str can be a
    string containing a file name, or a port if supported by the system.
    `(provided? 'chdir-port)` reports whether ports are supported.
    The return value is unspecified.

Scheme Procedure: **getcwd** [¶](#index-getcwd)

C Function: **scm\_getcwd** () [¶](#index-scm_005fgetcwd)
:   Return the name of the current working directory.

Scheme Procedure: **umask** [mode] [¶](#index-umask)

C Function: **scm\_umask** (mode) [¶](#index-scm_005fumask)
:   If mode is omitted, returns a decimal number representing the
    current file creation mask. Otherwise the file creation mask is set
    to mode and the previous value is returned. See [Assigning File Permissions](/libc/latest/en/libc.html#Setting-Permissions) in The GNU C Library
    Reference Manual, for more on how to use umasks.

    E.g., `(umask #o022)` sets the mask to octal 22/decimal 18.

Scheme Procedure: **chroot** path [¶](#index-chroot)

C Function: **scm\_chroot** (path) [¶](#index-scm_005fchroot)
:   Change the root directory to that specified in path.
    This directory will be used for path names beginning with
    `/`. The root directory is inherited by all children
    of the current process. Only the superuser may change the
    root directory.

Scheme Procedure: **getpid** [¶](#index-getpid)

C Function: **scm\_getpid** () [¶](#index-scm_005fgetpid)
:   Return an integer representing the current process ID.

Scheme Procedure: **getgroups** [¶](#index-getgroups)

C Function: **scm\_getgroups** () [¶](#index-scm_005fgetgroups)
:   Return a vector of integers representing the current
    supplementary group IDs.

Scheme Procedure: **getppid** [¶](#index-getppid)

C Function: **scm\_getppid** () [¶](#index-scm_005fgetppid)
:   Return an integer representing the process ID of the parent
    process.

Scheme Procedure: **getuid** [¶](#index-getuid)

C Function: **scm\_getuid** () [¶](#index-scm_005fgetuid)
:   Return an integer representing the current real user ID.

Scheme Procedure: **getgid** [¶](#index-getgid)

C Function: **scm\_getgid** () [¶](#index-scm_005fgetgid)
:   Return an integer representing the current real group ID.

Scheme Procedure: **geteuid** [¶](#index-geteuid)

C Function: **scm\_geteuid** () [¶](#index-scm_005fgeteuid)
:   Return an integer representing the current effective user ID.
    If the system does not support effective IDs, then the real ID
    is returned. `(provided? 'EIDs)` reports whether the
    system supports effective IDs.

Scheme Procedure: **getegid** [¶](#index-getegid)

C Function: **scm\_getegid** () [¶](#index-scm_005fgetegid)
:   Return an integer representing the current effective group ID.
    If the system does not support effective IDs, then the real ID
    is returned. `(provided? 'EIDs)` reports whether the
    system supports effective IDs.

Scheme Procedure: **setgroups** vec [¶](#index-setgroups)

C Function: **scm\_setgroups** (vec) [¶](#index-scm_005fsetgroups)
:   Set the current set of supplementary group IDs to the integers in the
    given vector vec. The return value is unspecified.

    Generally only the superuser can set the process group IDs
    (see [Setting the Group IDs](/libc/latest/en/libc.html#Setting-Groups) in The GNU C
    Library Reference Manual).

Scheme Procedure: **setuid** id [¶](#index-setuid)

C Function: **scm\_setuid** (id) [¶](#index-scm_005fsetuid)
:   Sets both the real and effective user IDs to the integer id, provided
    the process has appropriate privileges.
    The return value is unspecified.

Scheme Procedure: **setgid** id [¶](#index-setgid)

C Function: **scm\_setgid** (id) [¶](#index-scm_005fsetgid)
:   Sets both the real and effective group IDs to the integer id, provided
    the process has appropriate privileges.
    The return value is unspecified.

Scheme Procedure: **seteuid** id [¶](#index-seteuid)

C Function: **scm\_seteuid** (id) [¶](#index-scm_005fseteuid)
:   Sets the effective user ID to the integer id, provided the process
    has appropriate privileges. If effective IDs are not supported, the
    real ID is set instead—`(provided? 'EIDs)` reports whether the
    system supports effective IDs.
    The return value is unspecified.

Scheme Procedure: **setegid** id [¶](#index-setegid)

C Function: **scm\_setegid** (id) [¶](#index-scm_005fsetegid)
:   Sets the effective group ID to the integer id, provided the process
    has appropriate privileges. If effective IDs are not supported, the
    real ID is set instead—`(provided? 'EIDs)` reports whether the
    system supports effective IDs.
    The return value is unspecified.

Scheme Procedure: **getpgrp** [¶](#index-getpgrp)

C Function: **scm\_getpgrp** () [¶](#index-scm_005fgetpgrp)
:   Return an integer representing the current process group ID.
    This is the POSIX definition, not BSD.

Scheme Procedure: **setpgid** pid pgid [¶](#index-setpgid)

C Function: **scm\_setpgid** (pid, pgid) [¶](#index-scm_005fsetpgid)
:   Move the process pid into the process group pgid. pid or
    pgid must be integers: they can be zero to indicate the ID of the
    current process.
    Fails on systems that do not support job control.
    The return value is unspecified.

Scheme Procedure: **setsid** [¶](#index-setsid)

C Function: **scm\_setsid** () [¶](#index-scm_005fsetsid)
:   Creates a new session. The current process becomes the session leader
    and is put in a new process group. The process will be detached
    from its controlling terminal if it has one.
    The return value is an integer representing the new process group ID.

Scheme Procedure: **getsid** pid [¶](#index-getsid)

C Function: **scm\_getsid** (pid) [¶](#index-scm_005fgetsid)
:   Returns the session ID of process pid. (The session
    ID of a process is the process group ID of its session leader.)

Scheme Procedure: **waitpid** pid [options] [¶](#index-waitpid)

C Function: **scm\_waitpid** (pid, options) [¶](#index-scm_005fwaitpid)
:   This procedure collects status information from a child process which
    has terminated or (optionally) stopped. Normally it will
    suspend the calling process until this can be done. If more than one
    child process is eligible then one will be chosen by the operating system.

    The value of pid determines the behavior:

    pid greater than 0
    :   Request status information from the specified child process.

    pid equal to -1 or `WAIT_ANY` [¶](#index-WAIT_005fANY)
    :   Request status information for any child process.

    pid equal to 0 or `WAIT_MYPGRP` [¶](#index-WAIT_005fMYPGRP)
    :   Request status information for any child process in the current process
        group.

    pid less than -1
    :   Request status information for any child process whose process group ID
        is −pid.

    The options argument, if supplied, should be the bitwise OR of the
    values of zero or more of the following variables:

    Variable: **WNOHANG** [¶](#index-WNOHANG)
    :   Return immediately even if there are no child processes to be collected.

    Variable: **WUNTRACED** [¶](#index-WUNTRACED)
    :   Report status information for stopped processes as well as terminated
        processes.

    The return value is a pair containing:

    1. The process ID of the child process, or 0 if `WNOHANG` was
       specified and no process was collected.
    2. The integer status value (see [Process Completion Status](/libc/latest/en/libc.html#Process-Completion-Status) in The
       GNU C Library Reference Manual).

The following three
functions can be used to decode the integer status value returned by
`waitpid`.

Scheme Procedure: **status:exit-val** status [¶](#index-status_003aexit_002dval)

C Function: **scm\_status\_exit\_val** (status) [¶](#index-scm_005fstatus_005fexit_005fval)
:   Return the exit status value, as would be set if a process
    ended normally through a call to `exit` or `_exit`,
    if any, otherwise `#f`.

Scheme Procedure: **status:term-sig** status [¶](#index-status_003aterm_002dsig)

C Function: **scm\_status\_term\_sig** (status) [¶](#index-scm_005fstatus_005fterm_005fsig)
:   Return the signal number which terminated the process, if any,
    otherwise `#f`.

Scheme Procedure: **status:stop-sig** status [¶](#index-status_003astop_002dsig)

C Function: **scm\_status\_stop\_sig** (status) [¶](#index-scm_005fstatus_005fstop_005fsig)
:   Return the signal number which stopped the process, if any,
    otherwise `#f`.

Scheme Procedure: **system** [cmd] [¶](#index-system)

C Function: **scm\_system** (cmd) [¶](#index-scm_005fsystem)
:   Execute cmd using the operating system’s “command
    processor”. Under Unix this is usually the default shell
    `sh`. The value returned is cmd’s exit status as
    returned by `waitpid`, which can be interpreted using the
    functions above.

    If `system` is called without arguments, return a boolean
    indicating whether the command processor is available.

Scheme Procedure: **system\*** arg1 arg2 … [¶](#index-system_002a)

C Function: **scm\_system\_star** (args) [¶](#index-scm_005fsystem_005fstar)
:   Execute the command indicated by arg1 arg2 .... The
    first element must be a string indicating the command to be executed,
    and the remaining items must be strings representing each of the
    arguments to that command.

    This function returns the exit status of the command as provided by
    `waitpid`. This value can be handled with `status:exit-val`
    and the related functions.

    `system*` is similar to `system`, but accepts only one
    string per-argument, and performs no shell interpretation. The
    command is executed using fork and execlp. Accordingly this function
    may be safer than `system` in situations where shell
    interpretation is not required.

    Example: (system\* "echo" "foo" "bar")

Scheme Procedure: **quit** [status] [¶](#index-quit-1)
