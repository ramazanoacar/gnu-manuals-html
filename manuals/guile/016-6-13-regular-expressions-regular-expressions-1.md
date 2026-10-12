Seeking on a random-access port with buffered input, or switching to
    writing after reading, will cause the buffered input to be discarded and
    Guile will seek the port back the buffered number of bytes. Likewise
    seeking on a random-access port with buffered output, or switching to
    reading after writing, will flush pending bytes with a call to the
    `write` procedure. See [Buffering](#Buffering).

    Indicate to Guile that your port needs this behavior by returning true
    from your `#:random-access?` method. The default implementation of
    this function returns `#t` if the port has a `#:seek`
    implementation.

Scheme Port Method: **#:get-natural-buffer-sizes** read-buf-size write-buf-size [¶](#index-_0023_003aget_002dnatural_002dbuffer_002dsizes)
:   Guile will internally attach buffers to ports. An input port always has
    a read buffer, and an output port always has a write buffer.
    See [Buffering](#Buffering). A port buffer consists of a bytevector, along with
    some cursors into that bytevector denoting where to get and put data.

    Port implementations generally don’t have to be concerned with
    buffering: a port’s `#:read` or `#:write` method will receive
    the buffer’s bytevector as an argument, along with an offset and a
    length into that bytevector, and should then either fill or empty that
    bytevector. However in some cases, port implementations may be able to
    provide an appropriate default buffer size to Guile. For example file
    ports implement `#:get-natural-buffer-sizes` to let the operating
    system inform Guile about the appropriate buffer sizes for the
    particular file opened by the port.

    This method returns two values, corresponding to the natural read and
    write buffer sizes for the ports. The two parameters
    read-buf-size and write-buf-size are Guile’s guesses for
    what sizes might be good. A custom `#:get-natural-buffer-sizes`
    method could override Guile’s choices, or just pass them on, as the
    default implementation does.

Scheme Port Method: **#:print** port out [¶](#index-_0023_003aprint)
:   Called when the port port is written to out, e.g. via
    `(write port out)`.

    If `#:print` is not explicitly supplied, the default implementation
    prints something like `#<mode:id address>`, where
    mode is either `input`, `output`, or
    `input-output`, id comes from the `#:id` keyword
    argument (defaulting to `"custom-port"`), and address is a
    unique integer associated with the port.

Scheme Port Method: **#:close** port [¶](#index-_0023_003aclose)
:   Called when port is closed. It should release any
    explicitly-managed resources used by the port.

By default, ports that are garbage collected just go away without
closing or flushing any buffered output. If your port needs to release
some external resource like a file descriptor, or needs to make sure
that its internal buffers are flushed even if the port is collected
while it was open, then pass `#:close-on-gc? #t` to
`make-custom-port`. Note that in that case, the `#:close`
method will probably be called on a separate thread.

Note that calls to all of these methods can proceed in parallel and
concurrently and from any thread up until the point that the port is
closed. The call to `close` will happen when no other method is
running, and no method will be called after the `close` method is
called. If your port implementation needs mutual exclusion to prevent
concurrency, it is responsible for locking appropriately.

---

Previous: [Low-Level Custom Ports](#Low_002dLevel-Custom-Ports), Up: [Types of Port](#Port-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.10.8 Low-Level Custom Ports in C [¶](#Low_002dLevel-Custom-Ports-in-C-1)

The `make-custom-port` procedure described in the previous section
has similar functionality on the C level, though it is organized a bit
differently.

In C, the mechanism is that one creates a new *port type object*.
The methods are then associated with the port type object instead of the
port itself. The port type object is an opaque pointer allocated when
defining the port type, which serves as a key into the port API.

Ports themselves have associated *stream* values. The stream is a
pointer controlled by the user, which is set when the port is created.
Given a port, the `SCM_STREAM` macro returns its associated stream
value, as a `scm_t_bits`. Note that your port methods are only
ever called with ports of your type, so port methods can safely cast
this value to the expected type. Contrast this to Scheme, which doesn’t
need access to the stream because the `make-custom-port` methods
can be closures that share port-specific data directly.

A port type is created by calling `scm_make_port_type`.

Function: `scm_t_port_type*` **scm\_make\_port\_type** `(char *name, size_t (*read) (SCM port, SCM dst, size_t start, size_t count), size_t (*write) (SCM port, SCM src, size_t start, size_t count))` [¶](#index-scm_005fmake_005fport_005ftype)
:   Define a new port type. The name parameter is like the
    `#:id` parameter to `make-custom-port`; and read and
    write are like `make-custom-port`’s `#:read` and
    `#:write`, except that they should return `(size_t)-1` if the
    read or write operation would block, instead of `#f`.

Function: `void` **scm\_set\_port\_read\_wait\_fd** `(scm_t_port_type *type, int (*wait_fd) (SCM port))` [¶](#index-scm_005fset_005fport_005fread_005fwait_005ffd)

Function: `void` **scm\_set\_port\_write\_wait\_fd** `(scm_t_port_type *type, int (*wait_fd) (SCM port))` [¶](#index-scm_005fset_005fport_005fwrite_005fwait_005ffd)

Function: `void` **scm\_set\_port\_print** `(scm_t_port_type *type, int (*print) (SCM port, SCM dest_port, scm_print_state *pstate))` [¶](#index-scm_005fset_005fport_005fprint)

Function: `void` **scm\_set\_port\_close** `(scm_t_port_type *type, void (*close) (SCM port))` [¶](#index-scm_005fset_005fport_005fclose)

Function: `void` **scm\_set\_port\_needs\_close\_on\_gc** `(scm_t_port_type *type, int needs_close_p)` [¶](#index-scm_005fset_005fport_005fneeds_005fclose_005fon_005fgc)

Function: `void` **scm\_set\_port\_seek** `(scm_t_port_type *type, scm_t_off (*seek) (SCM port, scm_t_off offset, int whence))` [¶](#index-scm_005fset_005fport_005fseek)

Function: `void` **scm\_set\_port\_truncate** `(scm_t_port_type *type, void (*truncate) (SCM port, scm_t_off length))` [¶](#index-scm_005fset_005fport_005ftruncate)

Function: `void` **scm\_set\_port\_random\_access\_p** `(scm_t_port_type *type, int (*random_access_p) (SCM port));` [¶](#index-scm_005fset_005fport_005frandom_005faccess_005fp)

Function: `void` **scm\_set\_port\_input\_waiting** `(scm_t_port_type *type, int (*input_waiting) (SCM port));` [¶](#index-scm_005fset_005fport_005finput_005fwaiting)

Function: `void` **scm\_set\_port\_get\_natural\_buffer\_sizes** `(scm_t_port_type *type, void (*get_natural_buffer_sizes) (SCM, size_t *read_buf_size, size_t *write_buf_size))` [¶](#index-scm_005fset_005fport_005fget_005fnatural_005fbuffer_005fsizes)
:   Port method definitions. See [Low-Level Custom Ports](#Low_002dLevel-Custom-Ports), for more
    details on each of these methods.

Once you have your port type, you can create ports with
`scm_c_make_port`, or `scm_c_make_port_with_encoding`.

Function: `SCM` **scm\_c\_make\_port\_with\_encoding** `(scm_t_port_type *type, unsigned long mode_bits, SCM encoding, SCM conversion_strategy, scm_t_bits stream)` [¶](#index-scm_005fc_005fmake_005fport_005fwith_005fencoding)

Function: `SCM` **scm\_c\_make\_port** `(scm_t_port_type *type, unsigned long mode_bits, scm_t_bits stream)` [¶](#index-scm_005fc_005fmake_005fport)
:   Make a port with the given type. The stream indicates the
    private data associated with the port, which your port implementation
    may later retrieve with `SCM_STREAM`. The mode bits should include
    one or more of the flags `SCM_RDNG` or `SCM_WRTNG`, indicating
    that the port is an input and/or an output port, respectively. The mode
    bits may also include `SCM_BUF0` or `SCM_BUFLINE`, indicating
    that the port should be unbuffered or line-buffered, respectively. The
    default is that the port will be block-buffered. See [Buffering](#Buffering).

    As you would imagine, encoding and conversion\_strategy
    specify the port’s initial textual encoding and conversion strategy.
    Both are symbols. `scm_c_make_port` is the same as
    `scm_c_make_port_with_encoding`, except it uses the default port
    encoding and conversion strategy.

At this point you may be wondering whether to implement your custom port
type in C or Scheme. The answer is that probably you want to use
Scheme’s `make-custom-port`. The speed is similar between C and
Scheme, and ports implemented in C have the disadvantage of not being
suspendable. See [Non-Blocking I/O](#Non_002dBlocking-I_002fO).

---

Next: [Using Ports from C](#Using-Ports-from-C), Previous: [Types of Port](#Port-Types), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.11 Venerable Port Interfaces [¶](#Venerable-Port-Interfaces-1)

Over the 25 years or so that Guile has been around, its port system has
evolved, adding many useful features. At the same time there have been
four major Scheme standards released in those 25 years, which also
evolve the common Scheme understanding of what a port interface should
be. Alas, it would be too much to ask for all of these evolutionary
branches to be consistent. Some of Guile’s original interfaces don’t
mesh with the later Scheme standards, and yet Guile can’t just drop old
interfaces. Sadly as well, the R6RS and R7RS standards both part from a
base of R5RS, but end up in different and somewhat incompatible designs.

Guile’s approach is to pick a set of port primitives that make sense
together. We document that set of primitives, design our internal
interfaces around them, and recommend them to users. As the R6RS I/O
system is the most capable standard that Scheme has yet produced in this
domain, we mostly recommend that; `(ice-9 binary-ports)` and
`(ice-9 textual-ports)` are wholly modeled on `(rnrs io
ports)`. Guile does not wholly copy R6RS, however; See [Incompatibilities with the R6RS](#R6RS-Incompatibilities).

At the same time, we have many venerable port interfaces, lore handed
down to us from our hacker ancestors. Most of these interfaces even
predate the expectation that Scheme should have modules, so they are
present in the default environment. In Guile we support them as well
and we have no plans to remove them, but again we don’t recommend them
for new users.

Scheme Procedure: **char-ready?** [port] [¶](#index-char_002dready_003f)
:   Return `#t` if a character is ready on input port
    and return `#f` otherwise. If `char-ready?` returns
    `#t` then the next `read-char` operation on
    port is guaranteed not to hang. If port is a file
    port at end of file then `char-ready?` returns `#t`.

    `char-ready?` exists to make it possible for a
    program to accept characters from interactive ports without
    getting stuck waiting for input. Any input editors associated
    with such ports must make sure that characters whose existence
    has been asserted by `char-ready?` cannot be rubbed out.
    If `char-ready?` were to return `#f` at end of file,
    a port at end of file would be indistinguishable from an
    interactive port that has no ready characters.

    Note that `char-ready?` only works reliably for terminals and
    sockets with one-byte encodings. Under the hood it will return
    `#t` if the port has any input buffered, or if the file descriptor
    that backs the port polls as readable, indicating that Guile can fetch
    more bytes from the kernel. However being able to fetch one byte
    doesn’t mean that a full character is available; See [Encoding](#Encoding). Also,
    on many systems it’s possible for a file descriptor to poll as readable,
    but then block when it comes time to read bytes. Note also that on
    Linux kernels, all file ports backed by files always poll as readable.
    For non-file ports, this procedure always returns `#t`, except for
    soft ports, which have a `char-ready?` handler. See [Soft Ports](#Soft-Ports).

    In short, this is a legacy procedure whose semantics are hard to
    provide. However it is a useful check to see if any input is buffered.
    See [Non-Blocking I/O](#Non_002dBlocking-I_002fO).

Scheme Procedure: **read-char** [port] [¶](#index-read_002dchar)
:   The same as `get-char`, except that port defaults to the
    current input port. See [Textual I/O](#Textual-I_002fO).

Scheme Procedure: **peek-char** [port] [¶](#index-peek_002dchar)
:   The same as `lookahead-char`, except that port defaults to
    the current input port. See [Textual I/O](#Textual-I_002fO).

Scheme Procedure: **unread-char** cobj [port] [¶](#index-unread_002dchar)
:   The same as `unget-char`, except that port defaults to the
    current input port, and the arguments are swapped. See [Textual I/O](#Textual-I_002fO).

Scheme Procedure: **unread-string** str [port] [¶](#index-unread_002dstring)

C Function: **scm\_unread\_string** (str, port) [¶](#index-scm_005funread_005fstring)
:   The same as `unget-string`, except that port defaults to the
    current input port, and the arguments are swapped. See [Textual I/O](#Textual-I_002fO).

Scheme Procedure: **newline** [port] [¶](#index-newline)
:   Send a newline to port. If port is omitted, send to the
    current output port. Equivalent to `(put-char port #\newline)`.

Scheme Procedure: **write-char** chr [port] [¶](#index-write_002dchar)
:   The same as `put-char`, except that port defaults to the
    current input port, and the arguments are swapped. See [Textual I/O](#Textual-I_002fO).

---

Next: [Non-Blocking I/O](#Non_002dBlocking-I_002fO), Previous: [Venerable Port Interfaces](#Venerable-Port-Interfaces), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.12 Using Ports from C [¶](#Using-Ports-from-C-1)

Guile’s C interfaces provides some niceties for sending and receiving
bytes and characters in a way that works better with C.

C Function: `size_t` **scm\_c\_read** `(SCM port, void *buffer, size_t size)` [¶](#index-scm_005fc_005fread)
:   Read up to size bytes from port and store them in
    buffer. The return value is the number of bytes actually read,
    which can be less than size if end-of-file has been reached.

    Note that as this is a binary input procedure, this function does not
    update `port-line` and `port-column` (see [Textual I/O](#Textual-I_002fO)).

C Function: `void` **scm\_c\_write** `(SCM port, const void *buffer, size_t size)` [¶](#index-scm_005fc_005fwrite)
:   Write size bytes at buffer to port.

    Note that as this is a binary output procedure, this function does not
    update `port-line` and `port-column` (see [Textual I/O](#Textual-I_002fO)).

C Function: `size_t` **scm\_c\_read\_bytes** `(SCM port, SCM bv, size_t start, size_t count)` [¶](#index-scm_005fc_005fread_005fbytes)

C Function: `void` **scm\_c\_write\_bytes** `(SCM port, SCM bv, size_t start, size_t count)` [¶](#index-scm_005fc_005fwrite_005fbytes)
:   Like `scm_c_read` and `scm_c_write`, but reading into or
    writing from the bytevector bv. count indicates the byte
    index at which to start in the bytevector, and the read or write will
    continue for count bytes.

C Function: `void` **scm\_unget\_bytes** `(const unsigned char *buf, size_t len, SCM port)` [¶](#index-scm_005funget_005fbytes)

C Function: `void` **scm\_unget\_byte** `(int c, SCM port)` [¶](#index-scm_005funget_005fbyte)

C Function: `void` **scm\_ungetc** `(scm_t_wchar c, SCM port)` [¶](#index-scm_005fungetc)
:   Like `unget-bytevector`, `unget-byte`, and `unget-char`,
    respectively. See [Textual I/O](#Textual-I_002fO).

C Function: `void` **scm\_c\_put\_latin1\_chars** `(SCM port, const scm_t_uint8 *buf, size_t len)` [¶](#index-scm_005fc_005fput_005flatin1_005fchars)

C Function: `void` **scm\_c\_put\_utf32\_chars** `(SCM port, const scm_t_uint32 *buf, size_t len);` [¶](#index-scm_005fc_005fput_005futf32_005fchars)
:   Write a string to port. In the first case, the
    `scm_t_uint8*` buffer is a string in the latin-1 encoding. In the
    second, the `scm_t_uint32*` buffer is a string in the UTF-32
    encoding. These routines will update the port’s line and column.

---

Next: [Handling of Unicode Byte Order Marks](#BOM-Handling), Previous: [Using Ports from C](#Using-Ports-from-C), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.13 Non-Blocking I/O [¶](#Non_002dBlocking-I_002fO-1)

Most ports in Guile are *blocking*: when you try to read a character
from a port, Guile will block on the read until a character is ready, or
end-of-stream is detected. Likewise whenever Guile goes to write
(possibly buffered) data to an output port, Guile will block until all
the data is written.

Interacting with ports in blocking mode is very convenient: you can
write straightforward, sequential algorithms whose code flow reflects
the flow of data. However, blocking I/O has two main limitations.

The first is that it’s easy to get into a situation where code is
waiting on data. Time spent waiting on data when code could be doing
something else is wasteful and prevents your program from reaching its
peak throughput. If you implement a web server that sequentially
handles requests from clients, it’s very easy for the server to end up
waiting on a client to finish its HTTP request, or waiting on it to
consume the response. The end result is that you are able to serve
fewer requests per second than you’d like to serve.

The second limitation is related: a blocking parser over user-controlled
input is a denial-of-service vulnerability. Indeed the so-called “slow
loris” attack of the early 2010s was just that: an attack on common web
servers that drip-fed HTTP requests, one character at a time. All it
took was a handful of slow loris connections to occupy an entire web
server.

In Guile we would like to preserve the ability to write straightforward
blocking networking processes of all kinds, but under the hood to allow
those processes to suspend their requests if they would block.

To do this, the first piece is to allow Guile ports to declare
themselves as being nonblocking. This is currently supported only for
file ports, which also includes sockets, terminals, or any other port
that is backed by a file descriptor. To do that, we use an arcane UNIX
incantation:

```
(let ((flags (fcntl socket F_GETFL)))
  (fcntl socket F_SETFL (logior O_NONBLOCK flags)))
```

Now the file descriptor is open in non-blocking mode. If Guile tries to
read or write from this file and the read or write returns a result
indicating that more data can only be had by doing a blocking read or
write, Guile will block by polling on the socket’s `read-wait-fd`
or `write-wait-fd`, to preserve the illusion of a blocking read or
write. See [Low-Level Custom Ports](#Low_002dLevel-Custom-Ports) for more on those internal
interfaces.

So far we have just reproduced the status quo: the file descriptor is
non-blocking, but the operations on the port do block. To go farther,
it would be nice if we could suspend the “thread” using delimited
continuations, and only resume the thread once the file descriptor is
readable or writable. (See [Prompts](#Prompts)).

But here we run into a difficulty. The ports code is implemented in C,
which means that although we can suspend the computation to some outer
prompt, we can’t resume it because Guile can’t resume delimited
continuations that capture the C stack.

To overcome this difficulty we have created a compatible but entirely
parallel implementation of port operations. To use this implementation,
do the following:

```
(use-modules (ice-9 suspendable-ports))
(install-suspendable-ports!)
```

This will replace the core I/O primitives like `get-char` and
`put-bytevector` with new versions that are exactly the same as the
ones in the standard library, but with two differences. One is that
when a read or a write would block, the suspendable port operations call
out the value of the `current-read-waiter` or
`current-write-waiter` parameter, as appropriate.
See [Parameters](#Parameters). The default read and write waiters do the same thing
that the C read and write waiters do, which is to poll. User code can
parameterize the waiters, though, enabling the computation to suspend
and allow the program to process other I/O operations. Because the new
suspendable ports implementation is written in Scheme, that suspended
computation can resume again later when it is able to make progress.
Success!

The other main difference is that because the new ports implementation
is written in Scheme, it is slower than C, currently by a factor of 3 or
4, though it depends on many factors. For this reason we have to keep
the C implementations as the default ones. One day when Guile’s
compiler is better, we can close this gap and have only one port
operation implementation again.

Note that Guile does not currently include an implementation of the
facility to suspend the current thread and schedule other threads in the
meantime. Before adding such a thing, we want to make sure that we’re
providing the right primitives that can be used to build schedulers and
other user-space concurrency patterns, and that the patterns that we
settle on are the right patterns. In the meantime, have a look at 8sync
(<https://gnu.org/software/8sync>) for a prototype of an
asynchronous I/O and concurrency facility.

Scheme Procedure: **install-suspendable-ports!** [¶](#index-install_002dsuspendable_002dports_0021)
:   Replace the core ports implementation with suspendable ports, as
    described above. This will mutate the values of the bindings like
    `get-char`, `put-u8`, and so on in place.

Scheme Procedure: **uninstall-suspendable-ports!** [¶](#index-uninstall_002dsuspendable_002dports_0021)
:   Restore the original core ports implementation, un-doing the effect of
    `install-suspendable-ports!`.

Scheme Parameter: **current-read-waiter** [¶](#index-current_002dread_002dwaiter)

Scheme Parameter: **current-write-waiter** [¶](#index-current_002dwrite_002dwaiter)
:   Parameters whose values are procedures of one argument, called when a
    suspendable port operation would block on a port while reading or
    writing, respectively. The default values of these parameters do a
    blocking `poll` on the port’s file descriptor. The procedures are
    passed the port in question as their one argument.

---

Previous: [Non-Blocking I/O](#Non_002dBlocking-I_002fO), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.14 Handling of Unicode Byte Order Marks [¶](#Handling-of-Unicode-Byte-Order-Marks)

This section documents the finer points of Guile’s handling of Unicode
byte order marks (BOMs). A byte order mark (U+FEFF) is typically found
at the start of a UTF-16 or UTF-32 stream, to allow readers to reliably
determine the byte order. Occasionally, a BOM is found at the start of
a UTF-8 stream, but this is much less common and not generally
recommended.

Guile attempts to handle BOMs automatically, and in accordance with the
recommendations of the Unicode Standard, when the port encoding is set
to `UTF-8`, `UTF-16`, or `UTF-32`. In brief, Guile
automatically writes a BOM at the start of a UTF-16 or UTF-32 stream,
and automatically consumes one from the start of a UTF-8, UTF-16, or
UTF-32 stream.

As specified in the Unicode Standard, a BOM is only handled specially at
the start of a stream, and only if the port encoding is set to
`UTF-8`, `UTF-16` or `UTF-32`. If the port encoding is
set to `UTF-16BE`, `UTF-16LE`, `UTF-32BE`, or
`UTF-32LE`, then BOMs are *not* handled specially, and none of
the special handling described in this section applies.

* To ensure that Guile will properly detect the byte order of a UTF-16 or
  UTF-32 stream, you must perform a textual read before any writes, seeks,
  or binary I/O. Guile will not attempt to read a BOM unless a read is
  explicitly requested at the start of the stream.
* If a textual write is performed before the first read, then an arbitrary
  byte order will be chosen. Currently, big endian is the default on all
  platforms, but that may change in the future. If you wish to explicitly
  control the byte order of an output stream, set the port encoding to
  `UTF-16BE`, `UTF-16LE`, `UTF-32BE`, or `UTF-32LE`,
  and explicitly write a BOM (`#\xFEFF`) if desired.
* If `set-port-encoding!` is called in the middle of a stream, Guile
  treats this as a new logical “start of stream” for purposes of BOM
  handling, and will forget about any BOMs that had previously been seen.
  Therefore, it may choose a different byte order than had been used
  previously. This is intended to support multiple logical text streams
  embedded within a larger binary stream.
* Binary I/O operations are not guaranteed to update Guile’s notion of
  whether the port is at the “start of the stream”, nor are they
  guaranteed to produce or consume BOMs.
* For ports that support seeking (e.g. normal files), the input and output
  streams are considered linked: if the user reads first, then a BOM will
  be consumed (if appropriate), but later writes will *not* produce a
  BOM. Similarly, if the user writes first, then later reads will
  *not* consume a BOM.
* For ports that are not random access (e.g. pipes, sockets, and
  terminals), the input and output streams are considered
  *independent* for purposes of BOM handling: the first read will
  consume a BOM (if appropriate), and the first write will *also*
  produce a BOM (if appropriate). However, the input and output streams
  will always use the same byte order.
* Seeks to the beginning of a file will set the “start of stream” flags.
  Therefore, a subsequent textual read or write will consume or produce a
  BOM. However, unlike `set-port-encoding!`, if a byte order had
  already been chosen for the port, it will remain in effect after a seek,
  and cannot be changed by the presence of a BOM. Seeks anywhere other
  than the beginning of a file clear the “start of stream” flags.

---

Next: [LALR(1) Parsing](#LALR_00281_0029-Parsing), Previous: [Input and Output](#Input-and-Output), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.13 Regular Expressions [¶](#Regular-Expressions-1)

A *regular expression* (or *regexp*) is a pattern that
describes a whole class of strings. A full description of regular
expressions and their syntax is beyond the scope of this manual.

If your system does not include a POSIX regular expression library,
and you have not linked Guile with a third-party regexp library such
as Rx, these functions will not be available. You can tell whether
your Guile installation includes regular expression support by
checking whether `(provided? 'regex)` returns true.

The following regexp and string matching features are provided by the
`(ice-9 regex)` module. Before using the described functions,
you should load this module by executing `(use-modules (ice-9
regex))`.

* [Regexp Functions](#Regexp-Functions)
* [Match Structures](#Match-Structures)
* [Backslash Escapes](#Backslash-Escapes)

---

Next: [Match Structures](#Match-Structures), Up: [Regular Expressions](#Regular-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.13.1 Regexp Functions [¶](#Regexp-Functions-1)

By default, Guile supports POSIX extended regular expressions. That
means that the characters ‘`(`’, ‘`)`’, ‘`+`’ and ‘`?`’ are
special, and must be escaped if you wish to match the literal characters
and there is no support for “non-greedy” variants of ‘`*`’,
‘`+`’ or ‘`?`’.

This regular expression interface was modeled after that
implemented by SCSH, the Scheme Shell. It is intended to be
upwardly compatible with SCSH regular expressions.

Zero bytes (`#\nul`) cannot be used in regex patterns or input
strings, since the underlying C functions treat that as the end of
string. If there’s a zero byte an error is thrown.

Internally, patterns and input strings are converted to the current
locale’s encoding, and then passed to the C library’s regular expression
routines (see [Regular Expressions](/libc/latest/en/libc.html#Regular-Expressions) in The GNU C Library
Reference Manual). The returned match structures always point to
characters in the strings, not to individual bytes, even in the case of
multi-byte encodings. This ensures that the match structures are
correct when performing matching with characters that have a multi-byte
representation in the locale encoding. Note, however, that using
characters which cannot be represented in the locale encoding can
lead to surprising results.

Scheme Procedure: **string-match** pattern str [start] [¶](#index-string_002dmatch)
:   Compile the string pattern into a regular expression and compare
    it with str. The optional numeric argument start specifies
    the position of str at which to begin matching.

    `string-match` returns a *match structure* which
    describes what, if anything, was matched by the regular
    expression. See [Match Structures](#Match-Structures). If str does not match
    pattern at all, `string-match` returns `#f`.

Two examples of a match follow. In the first example, the pattern
matches the four digits in the match string. In the second, the pattern
matches nothing.

```
(string-match "[0-9][0-9][0-9][0-9]" "blah2002")
⇒ #("blah2002" (4 . 8))

(string-match "[A-Za-z]" "123456")
⇒ #f
```

Each time `string-match` is called, it must compile its
pattern argument into a regular expression structure. This
operation is expensive, which makes `string-match` inefficient if
the same regular expression is used several times (for example, in a
loop). For better performance, you can compile a regular expression in
advance and then match strings against the compiled regexp.

Scheme Procedure: **make-regexp** pat flag… [¶](#index-make_002dregexp)

C Function: **scm\_make\_regexp** (pat, flaglst) [¶](#index-scm_005fmake_005fregexp)
:   Compile the regular expression described by pat, and
    return the compiled regexp structure. If pat does not
    describe a legal regular expression, `make-regexp` throws
    a `regular-expression-syntax` error.

    The flag arguments change the behavior of the compiled
    regular expression. The following values may be supplied:

    Variable: **regexp/icase** [¶](#index-regexp_002ficase)
    :   Consider uppercase and lowercase letters to be the same when
        matching.

    Variable: **regexp/newline** [¶](#index-regexp_002fnewline)
    :   If a newline appears in the target string, then permit the
        ‘`^`’ and ‘`$`’ operators to match immediately after or
        immediately before the newline, respectively. Also, the
        ‘`.`’ and ‘`[^...]`’ operators will never match a newline
        character. The intent of this flag is to treat the target
        string as a buffer containing many lines of text, and the
        regular expression as a pattern that may match a single one of
        those lines.

    Variable: **regexp/basic** [¶](#index-regexp_002fbasic)
    :   Compile a basic (“obsolete”) regexp instead of the extended
        (“modern”) regexps that are the default. Basic regexps do
        not consider ‘`|`’, ‘`+`’ or ‘`?`’ to be special
        characters, and require the ‘`{...}`’ and ‘`(...)`’
        metacharacters to be backslash-escaped (see [Backslash Escapes](#Backslash-Escapes)). There are several other differences between basic
        and extended regular expressions, but these are the most
        significant.

    Variable: **regexp/extended** [¶](#index-regexp_002fextended)
    :   Compile an extended regular expression rather than a basic
        regexp. This is the default behavior; this flag will not
        usually be needed. If a call to `make-regexp` includes
        both `regexp/basic` and `regexp/extended` flags, the
        one which comes last will override the earlier one.

Scheme Procedure: **regexp-exec** rx str [start [flags]] [¶](#index-regexp_002dexec)

C Function: **scm\_regexp\_exec** (rx, str, start, flags) [¶](#index-scm_005fregexp_005fexec)
:   Match the compiled regular expression rx against
    `str`. If the optional integer start argument is
    provided, begin matching from that position in the string.
    Return a match structure describing the results of the match,
    or `#f` if no match could be found.

    The flags argument changes the matching behavior. The following
    flag values may be supplied, use `logior` (see [Bitwise Operations](#Bitwise-Operations)) to combine them,

    Variable: **regexp/notbol** [¶](#index-regexp_002fnotbol)
    :   Consider that the start offset into str is not the
        beginning of a line and should not match operator ‘`^`’.

        If rx was created with the `regexp/newline` option above,
        ‘`^`’ will still match after a newline in str.

    Variable: **regexp/noteol** [¶](#index-regexp_002fnoteol)
    :   Consider that the end of str is not the end of a line and should
        not match operator ‘`$`’.

        If rx was created with the `regexp/newline` option above,
        ‘`$`’ will still match before a newline in str.

```
;; Regexp to match uppercase letters
(define r (make-regexp "[A-Z]*"))

;; Regexp to match letters, ignoring case
(define ri (make-regexp "[A-Z]*" regexp/icase))

;; Search for bob using regexp r
(match:substring (regexp-exec r "bob"))
⇒ ""                  ; no match

;; Search for bob using regexp ri
(match:substring (regexp-exec ri "Bob"))
⇒ "Bob"               ; matched case insensitive
```

Scheme Procedure: **regexp?** obj [¶](#index-regexp_003f)

C Function: **scm\_regexp\_p** (obj) [¶](#index-scm_005fregexp_005fp)
:   Return `#t` if obj is a compiled regular expression,
    or `#f` otherwise.

  

Scheme Procedure: **list-matches** regexp str [flags] [¶](#index-list_002dmatches)
:   Return a list of match structures which are the non-overlapping
    matches of regexp in str. regexp can be either a
    pattern string or a compiled regexp. The flags argument is as
    per `regexp-exec` above.

    ```
    (map match:substring (list-matches "[a-z]+" "abc 42 def 78"))
    ⇒ ("abc" "def")
    ```

Scheme Procedure: **fold-matches** regexp str init proc [flags] [¶](#index-fold_002dmatches)
:   Apply proc to the non-overlapping matches of regexp in
    str, to build a result. regexp can be either a pattern
    string or a compiled regexp. The flags argument is as per
    `regexp-exec` above.

    proc is called as `(proc match prev)` where
    match is a match structure and prev is the previous return
    from proc. For the first call prev is the given
    init parameter. `fold-matches` returns the final value
    from proc.

    For example to count matches,

    ```
    (fold-matches "[a-z][0-9]" "abc x1 def y2" 0
                  (lambda (match count)
                    (1+ count)))
    ⇒ 2
    ```

  

Regular expressions are commonly used to find patterns in one string
and replace them with the contents of another string. The following
functions are convenient ways to do this.

Scheme Procedure: **regexp-substitute** port match item … [¶](#index-regexp_002dsubstitute)
:   Write to port selected parts of the match structure match.
    Or if port is `#f` then form a string from those parts and
    return that.

    Each item specifies a part to be written, and may be one of the
    following,

    * A string. String arguments are written out verbatim.
    * An integer. The submatch with that number is written
      (`match:substring`). Zero is the entire match.
    * The symbol ‘`pre`’. The portion of the matched string preceding
      the regexp match is written (`match:prefix`).
    * The symbol ‘`post`’. The portion of the matched string following
      the regexp match is written (`match:suffix`).

    For example, changing a match and retaining the text before and after,

    ```
    (regexp-substitute #f (string-match "[0-9]+" "number 25 is good")
                       'pre "37" 'post)
    ⇒ "number 37 is good"
    ```

    Or matching a YYYYMMDD format date such as ‘`20020828`’ and
    re-ordering and hyphenating the fields.

    ```
    (define date-regex
       "([0-9][0-9][0-9][0-9])([0-9][0-9])([0-9][0-9])")
    (define s "Date 20020429 12am.")
    (regexp-substitute #f (string-match date-regex s)
                       'pre 2 "-" 3 "-" 1 'post " (" 0 ")")
    ⇒ "Date 04-29-2002 12am. (20020429)"
    ```

Scheme Procedure: **regexp-substitute/global** port regexp target item… [¶](#index-regexp_002dsubstitute_002fglobal)
:   Write to port selected parts of matches of regexp in
    target. If port is `#f` then form a string from
    those parts and return that. regexp can be a string or a
    compiled regex.

    This is similar to `regexp-substitute`, but allows global
    substitutions on target. Each item behaves as per
    `regexp-substitute`, with the following differences,

    * A function. Called as `(item match)` with the match
      structure for the regexp match, it should return a string to be
      written to port.
    * The symbol ‘`post`’. This doesn’t output anything, but instead
      causes `regexp-substitute/global` to recurse on the unmatched
      portion of target.

      This *must* be supplied to perform a global search and replace on
      target; without it `regexp-substitute/global` returns after
      a single match and output.

    For example, to collapse runs of tabs and spaces to a single hyphen
    each,

    ```
    (regexp-substitute/global #f "[ \t]+"  "this   is   the text"
                              'pre "-" 'post)
    ⇒ "this-is-the-text"
    ```

    Or using a function to reverse the letters in each word,

    ```
    (regexp-substitute/global #f "[a-z]+"  "to do and not-do"
      'pre (lambda (m) (string-reverse (match:substring m))) 'post)
    ⇒ "ot od dna ton-od"
    ```

    Without the `post` symbol, just one regexp match is made. For
    example the following is the date example from
    `regexp-substitute` above, without the need for the separate
    `string-match` call.

    ```
    (define date-regex
       "([0-9][0-9][0-9][0-9])([0-9][0-9])([0-9][0-9])")
    (define s "Date 20020429 12am.")
    (regexp-substitute/global #f date-regex s
                              'pre 2 "-" 3 "-" 1 'post " (" 0 ")")

    ⇒ "Date 04-29-2002 12am. (20020429)"
    ```

---

Next: [Backslash Escapes](#Backslash-Escapes), Previous: [Regexp Functions](#Regexp-Functions), Up: [Regular Expressions](#Regular-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.13.2 Match Structures [¶](#Match-Structures-1)

A *match structure* is the object returned by `string-match` and
`regexp-exec`. It describes which portion of a string, if any,
matched the given regular expression. Match structures include: a
reference to the string that was checked for matches; the starting and
ending positions of the regexp match; and, if the regexp included any
parenthesized subexpressions, the starting and ending positions of each
submatch.

In each of the regexp match functions described below, the `match`
argument must be a match structure returned by a previous call to
`string-match` or `regexp-exec`. Most of these functions
return some information about the original target string that was
matched against a regular expression; we will call that string
target for easy reference.

Scheme Procedure: **regexp-match?** obj [¶](#index-regexp_002dmatch_003f)
:   Return `#t` if obj is a match structure returned by a
    previous call to `regexp-exec`, or `#f` otherwise.

Scheme Procedure: **match:substring** match [n] [¶](#index-match_003asubstring)
:   Return the portion of target matched by subexpression number
    n. Submatch 0 (the default) represents the entire regexp match.
    If the regular expression as a whole matched, but the subexpression
    number n did not match, return `#f`.

```
(define s (string-match "[0-9][0-9][0-9][0-9]" "blah2002foo"))
(match:substring s)
⇒ "2002"

;; match starting at offset 6 in the string
(match:substring
  (string-match "[0-9][0-9][0-9][0-9]" "blah987654" 6))
⇒ "7654"
```

Scheme Procedure: **match:start** match [n] [¶](#index-match_003astart)
:   Return the starting position of submatch number n.

In the following example, the result is 4, since the match starts at
character index 4:

```
(define s (string-match "[0-9][0-9][0-9][0-9]" "blah2002foo"))
(match:start s)
⇒ 4
```

Scheme Procedure: **match:end** match [n] [¶](#index-match_003aend)
:   Return the ending position of submatch number n.

In the following example, the result is 8, since the match runs between
characters 4 and 8 (i.e. the “2002”).

```
(define s (string-match "[0-9][0-9][0-9][0-9]" "blah2002foo"))
(match:end s)
⇒ 8
```

Scheme Procedure: **match:prefix** match [¶](#index-match_003aprefix)
:   Return the unmatched portion of target preceding the regexp match.

    ```
    (define s (string-match "[0-9][0-9][0-9][0-9]" "blah2002foo"))
    (match:prefix s)
    ⇒ "blah"
    ```

Scheme Procedure: **match:suffix** match [¶](#index-match_003asuffix)
:   Return the unmatched portion of target following the regexp match.

```
(define s (string-match "[0-9][0-9][0-9][0-9]" "blah2002foo"))
(match:suffix s)
⇒ "foo"
```

Scheme Procedure: **match:count** match [¶](#index-match_003acount)
:   Return the number of parenthesized subexpressions from match.
    Note that the entire regular expression match itself counts as a
    subexpression, and failed submatches are included in the count.

Scheme Procedure: **match:string** match [¶](#index-match_003astring)
:   Return the original target string.

```
(define s (string-match "[0-9][0-9][0-9][0-9]" "blah2002foo"))
(match:string s)
⇒ "blah2002foo"
```

---

Previous: [Match Structures](#Match-Structures), Up: [Regular Expressions](#Regular-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.13.3 Backslash Escapes [¶](#Backslash-Escapes-1)

Sometimes you will want a regexp to match characters like ‘`*`’ or
‘`$`’ exactly. For example, to check whether a particular string
represents a menu entry from an Info node, it would be useful to match
it against a regexp like ‘`^* [^:]*::`’. However, this won’t work;
because the asterisk is a metacharacter, it won’t match the ‘`*`’ at
the beginning of the string. In this case, we want to make the first
asterisk un-magic.

You can do this by preceding the metacharacter with a backslash
character ‘`\`’. (This is also called *quoting* the
metacharacter, and is known as a *backslash escape*.) When Guile
sees a backslash in a regular expression, it considers the following
glyph to be an ordinary character, no matter what special meaning it
would ordinarily have. Therefore, we can make the above example work by
changing the regexp to ‘`^\* [^:]*::`’. The ‘`\*`’ sequence tells
the regular expression engine to match only a single asterisk in the
target string.

Since the backslash is itself a metacharacter, you may force a regexp to
match a backslash in the target string by preceding the backslash with
itself. For example, to find variable references in a TeX program,
you might want to find occurrences of the string ‘`\let\`’ followed
by any number of alphabetic characters. The regular expression
‘`\\let\\[A-Za-z]*`’ would do this: the double backslashes in the
regexp each match a single backslash in the target string.

Scheme Procedure: **regexp-quote** str [¶](#index-regexp_002dquote)
:   Quote each special character found in str with a backslash, and
    return the resulting string.

**Very important:** Using backslash escapes in Guile source code
(as in Emacs Lisp or C) can be tricky, because the backslash character
has special meaning for the Guile reader. For example, if Guile
encounters the character sequence ‘`\n`’ in the middle of a string
while processing Scheme code, it replaces those characters with a
newline character. Similarly, the character sequence ‘`\t`’ is
replaced by a horizontal tab. Several of these *escape sequences*
are processed by the Guile reader before your code is executed.
Unrecognized escape sequences are ignored: if the characters ‘`\*`’
appear in a string, they will be translated to the single character
‘`*`’.

This translation is obviously undesirable for regular expressions, since
we want to be able to include backslashes in a string in order to
escape regexp metacharacters. Therefore, to make sure that a backslash
is preserved in a string in your Guile program, you must use *two*
consecutive backslashes:

```
(define Info-menu-entry-pattern (make-regexp "^\\* [^:]*"))
```

The string in this example is preprocessed by the Guile reader before
any code is executed. The resulting argument to `make-regexp` is
the string ‘`^\* [^:]*`’, which is what we really want.

This also means that in order to write a regular expression that matches
a single backslash character, the regular expression string in the
source code must include *four* backslashes. Each consecutive pair
of backslashes gets translated by the Guile reader to a single
backslash, and the resulting double-backslash is interpreted by the
regexp engine as matching a single backslash character. Hence:

```
(define tex-variable-pattern (make-regexp "\\\\let\\\\=[A-Za-z]*"))
```

The reason for the unwieldiness of this syntax is historical. Both
regular expression pattern matchers and Unix string processing systems
have traditionally used backslashes with the special meanings
described above. The POSIX regular expression specification and ANSI C
standard both require these semantics. Attempting to abandon either
convention would cause other kinds of compatibility problems, possibly
more severe ones. Therefore, without extending the Scheme reader to
support strings with different quoting conventions (an ungainly and
confusing extension when implemented in other languages), we must adhere
to this cumbersome escape syntax.

---

Next: [PEG Parsing](#PEG-Parsing), Previous: [Regular Expressions](#Regular-Expressions), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.14 LALR(1) Parsing [¶](#LALR_00281_0029-Parsing-1)

The `(system base lalr)` module provides the
[`lalr-scm` LALR(1) parser
generator by Dominique Boucher](https://github.com/schemeway/lalr-scm/). `lalr-scm` uses the same algorithm as GNU
Bison (see [Introduction to Bison](https://www.gnu.org/software/bison/manual/bison.html#Introduction) in Bison, The
Yacc-compatible Parser Generator). Parsers are defined using the
`lalr-parser` macro.

Scheme Syntax: **lalr-parser** [options] tokens rules... [¶](#index-lalr_002dparser)
:   Generate an LALR(1) syntax analyzer. tokens is a list of symbols
    representing the terminal symbols of the grammar. rules are the grammar
    production rules.

    Each rule has the form `(non-terminal (rhs ...) : action
    ...)`, where non-terminal is the name of the rule, rhs are the
    right-hand sides, i.e., the production rule, and action is a semantic
    action associated with the rule.

    The generated parser is a two-argument procedure that takes a
    *tokenizer* and a *syntax error procedure*. The tokenizer
    should be a thunk that returns lexical tokens as produced by
    `make-lexical-token`. The syntax error procedure may be called
    with at least an error message (a string), and optionally the lexical
    token that caused the error.

Please refer to the `lalr-scm` documentation for details.

---

Next: [Reading and Evaluating Scheme Code](#Read_002fLoad_002fEval_002fCompile), Previous: [LALR(1) Parsing](#LALR_00281_0029-Parsing), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
