### 6.12 Input and Output [¶](#Input-and-Output-1)

* [Ports](#Ports)
* [Binary I/O](#Binary-I_002fO)
* [Encoding](#Encoding)
* [Textual I/O](#Textual-I_002fO)
* [Simple Textual Output](#Simple-Output)
* [Buffering](#Buffering)
* [Random Access](#Random-Access)
* [Line Oriented and Delimited Text](#Line_002fDelimited)
* [Default Ports for Input, Output and Errors](#Default-Ports)
* [Types of Port](#Port-Types)
* [Venerable Port Interfaces](#Venerable-Port-Interfaces)
* [Using Ports from C](#Using-Ports-from-C)
* [Non-Blocking I/O](#Non_002dBlocking-I_002fO)
* [Handling of Unicode Byte Order Marks](#BOM-Handling)

---

Next: [Binary I/O](#Binary-I_002fO), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.1 Ports [¶](#Ports-1)

Ports are the way that Guile performs input and output. Guile can read
in characters or bytes from an *input port*, or write them out to an
*output port*. Some ports support both interfaces.

There are a number of different port types implemented in Guile. File
ports provide input and output over files, as you might imagine. For
example, we might display a string to a file like this:

```
(let ((port (open-output-file "foo.txt")))
  (display "Hello, world!\n" port)
  (close-port port))
```

There are also string ports, for taking input from a string, or
collecting output to a string; bytevector ports, for doing the same but
using a bytevector as a source or sink of data; and custom ports, for
arranging to call Scheme functions to provide input or handle output.
See [Types of Port](#Port-Types).

Ports should be *closed* when they are not needed by calling
`close-port` on them, as in the example above. This will make sure
that any pending output is successfully written out to disk, in the case
of a file port, or otherwise to whatever mutable store is backed by the
port. Any error that occurs while writing out that buffered data would
also be raised promptly at the `close-port`, and not later when the
port is closed by the garbage collector. See [Buffering](#Buffering), for more on
buffered output.

Closing a port also releases any precious resource the file might have.
Usually in Scheme a programmer doesn’t have to clean up after their data
structures (see [Memory Management and Garbage Collection](#Memory-Management)), but most systems have strict
limits on how many files can be open, both on a per-process and a
system-wide basis. A program that uses many files should take care not
to hit those limits. The same applies to similar system resources such
as pipes and sockets.

Indeed for these reasons the above example is not the most idiomatic way
to use ports. It is more common to acquire ports via procedures like
`call-with-output-file`, which handle the `close-port`
automatically:

```
(call-with-output-file "foo.txt"
  (lambda (port)
    (display "Hello, world!\n" port)))
```

Finally, all ports have associated input and output buffers, as
appropriate. Buffering is a common strategy to limit the overhead of
small reads and writes: without buffering, each character fetched from a
file would involve at least one call into the kernel, and maybe more
depending on the character and the encoding. Instead, Guile will batch
reads and writes into internal buffers. However, sometimes you want to
make output on a port show up immediately. See [Buffering](#Buffering), for more
on interfaces to control port buffering.

Scheme Procedure: **port?** x [¶](#index-port_003f)

C Function: **scm\_port\_p** (x) [¶](#index-scm_005fport_005fp)
:   Return a boolean indicating whether x is a port.
    Equivalent to `(or (input-port? x) (output-port? x))`.

Scheme Procedure: **input-port?** x [¶](#index-input_002dport_003f)

C Function: **scm\_input\_port\_p** (x) [¶](#index-scm_005finput_005fport_005fp)
:   Return `#t` if x is an input port, otherwise return
    `#f`. Any object satisfying this predicate also satisfies
    `port?`.

Scheme Procedure: **output-port?** x [¶](#index-output_002dport_003f)

C Function: **scm\_output\_port\_p** (x) [¶](#index-scm_005foutput_005fport_005fp)
:   Return `#t` if x is an output port, otherwise return
    `#f`. Any object satisfying this predicate also satisfies
    `port?`.

Scheme Procedure: **close-port** port [¶](#index-close_002dport)

C Function: **scm\_close\_port** (port) [¶](#index-scm_005fclose_005fport)
:   Close the specified port object. Return `#t` if it successfully
    closes a port or `#f` if it was already closed. An exception may
    be raised if an error occurs, for example when flushing buffered output.
    See [Buffering](#Buffering), for more on buffered output. See [close](#Ports-and-File-Descriptors), for a procedure which can close file descriptors.

Scheme Procedure: **port-closed?** port [¶](#index-port_002dclosed_003f)

C Function: **scm\_port\_closed\_p** (port) [¶](#index-scm_005fport_005fclosed_005fp)
:   Return `#t` if port is closed or `#f` if it is
    open.

Scheme Procedure: **call-with-port** port proc [¶](#index-call_002dwith_002dport)
:   Call proc, passing it port and closing port upon exit
    of proc. Return the return values of proc.

---

Next: [Encoding](#Encoding), Previous: [Ports](#Ports), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.2 Binary I/O [¶](#Binary-I_002fO-1)

Guile’s ports are fundamentally binary in nature: at the lowest level,
they work on bytes. This section describes Guile’s core binary I/O
operations. See [Textual I/O](#Textual-I_002fO), for input and output of strings and
characters.

To use these routines, first include the binary I/O module:

```
(use-modules (ice-9 binary-ports))
```

Note that although this module’s name suggests that binary ports are
some different kind of port, that’s not the case: all ports in Guile are
both binary and textual ports.

Scheme Procedure: **get-u8** port [¶](#index-get_002du8)

C Function: **scm\_get\_u8** (port) [¶](#index-scm_005fget_005fu8)
:   Return an octet read from port, an input port, blocking as
    necessary, or the end-of-file object.

Scheme Procedure: **lookahead-u8** port [¶](#index-lookahead_002du8)

C Function: **scm\_lookahead\_u8** (port) [¶](#index-scm_005flookahead_005fu8)
:   Like `get-u8` but does not update port’s position to point
    past the octet.

The end-of-file object is unlike any other kind of object: it’s not a
pair, a symbol, or anything else. To check if a value is the
end-of-file object, use the `eof-object?` predicate.

Scheme Procedure: **eof-object?** x [¶](#index-eof_002dobject_003f)

C Function: **scm\_eof\_object\_p** (x) [¶](#index-scm_005feof_005fobject_005fp)
:   Return `#t` if x is an end-of-file object, or `#f`
    otherwise.

Note that unlike other procedures in this module, `eof-object?` is
defined in the default environment.

Scheme Procedure: **get-bytevector-n** port count [¶](#index-get_002dbytevector_002dn)

C Function: **scm\_get\_bytevector\_n** (port, count) [¶](#index-scm_005fget_005fbytevector_005fn)
:   Read count octets from port, blocking as necessary and
    return a bytevector containing the octets read. If fewer bytes are
    available, a bytevector smaller than count is returned.

Scheme Procedure: **get-bytevector-n!** port bv start count [¶](#index-get_002dbytevector_002dn_0021)

C Function: **scm\_get\_bytevector\_n\_x** (port, bv, start, count) [¶](#index-scm_005fget_005fbytevector_005fn_005fx)
:   Read count bytes from port and store them in bv
    starting at index start. Return either the number of bytes
    actually read or the end-of-file object.

Scheme Procedure: **get-bytevector-some** port [¶](#index-get_002dbytevector_002dsome)

C Function: **scm\_get\_bytevector\_some** (port) [¶](#index-scm_005fget_005fbytevector_005fsome)
:   Read from port, blocking as necessary, until bytes are available
    or an end-of-file is reached. Return either the end-of-file object or a
    new bytevector containing some of the available bytes (at least one),
    and update the port position to point just past these bytes.

Scheme Procedure: **get-bytevector-some!** port bv start count [¶](#index-get_002dbytevector_002dsome_0021)

C Function: **scm\_get\_bytevector\_some\_x** (port, bv, start, count) [¶](#index-scm_005fget_005fbytevector_005fsome_005fx)
:   Read up to count bytes from port, blocking as necessary
    until at least one byte is available or an end-of-file is reached.
    Store them in bv starting at index start. Return the number
    of bytes actually read, or an end-of-file object.

Scheme Procedure: **get-bytevector-all** port [¶](#index-get_002dbytevector_002dall)

C Function: **scm\_get\_bytevector\_all** (port) [¶](#index-scm_005fget_005fbytevector_005fall)
:   Read from port, blocking as necessary, until the end-of-file is
    reached. Return either a new bytevector containing the data read or the
    end-of-file object (if no data were available).

Scheme Procedure: **unget-bytevector** port bv [start [count]] [¶](#index-unget_002dbytevector)

C Function: **scm\_unget\_bytevector** (port, bv, start, count) [¶](#index-scm_005funget_005fbytevector)
:   Place the contents of bv in port, optionally starting at
    index start and limiting to count octets, so that its bytes
    will be read from left-to-right as the next bytes from port during
    subsequent read operations. If called multiple times, the unread bytes
    will be read again in last-in first-out order.

To perform binary output on a port, use `put-u8` or
`put-bytevector`.

Scheme Procedure: **put-u8** port octet [¶](#index-put_002du8)

C Function: **scm\_put\_u8** (port, octet) [¶](#index-scm_005fput_005fu8)
:   Write octet, an integer in the 0–255 range, to port, a
    binary output port.

Scheme Procedure: **put-bytevector** port bv [start [count]] [¶](#index-put_002dbytevector)

C Function: **scm\_put\_bytevector** (port, bv, start, count) [¶](#index-scm_005fput_005fbytevector)
:   Write the contents of bv to port, optionally starting at
    index start and limiting to count octets.

#### Binary I/O in R7RS [¶](#Binary-I_002fO-in-R7RS)

[R7RS](#R7RS-Standard-Libraries) defines the following binary I/O
procedures. Access them with

```
(use-modules (scheme base))
```

Scheme Procedure: **open-output-bytevector** [¶](#index-open_002doutput_002dbytevector)
:   Returns a binary output port that will accumulate bytes
    for retrieval by [`get-output-bytevector`](#x_002dget_002doutput_002dbytevector).

Scheme Procedure: **write-u8** byte [out] [¶](#index-write_002du8)
:   Writes byte to the given binary output port out and returns
    an unspecified value. out defaults to `(current-output-port)`.

    See also [`put-u8`](#x_002dput_002du8).

Scheme Procedure: **read-u8** [in] [¶](#index-read_002du8)
:   Returns the next byte available from the binary input port in,
    updating the port to point to the following byte. If no more bytes are
    available, an end-of-file object is returned. in defaults to
    `(current-input-port)`.

    See also [`get-u8`](#x_002dget_002du8).

Scheme Procedure: **peek-u8** [in] [¶](#index-peek_002du8)
:   Returns the next byte available from the binary input port in,
    but without updating the port to point to the following
    byte. If no more bytes are available, an end-of-file object
    is returned. in defaults to `(current-input-port)`.

    See also [`lookahead-u8`](#x_002dlookahead_002du8).

Scheme Procedure: **get-output-bytevector** port [¶](#index-get_002doutput_002dbytevector)
:   Returns a bytevector consisting of the bytes that have been output to
    port so far in the order they were output. It is an error if
    port was not created with
    [`open-output-bytevector`](#x_002dopen_002doutput_002dbytevector).

    ```
    (define out (open-output-bytevector))
    (write-u8 1 out)
    (write-u8 2 out)
    (write-u8 3 out)
    (get-output-bytevector out) ⇒ #vu8(1 2 3)
    ```

Scheme Procedure: **open-input-bytevector** bv [¶](#index-open_002dinput_002dbytevector)
:   Takes a bytevector bv and returns a binary input port that
    delivers bytes from bv.

    ```
    (define in (open-input-bytevector #vu8(1 2 3)))
    (read-u8 in) ⇒ 1
    (peek-u8 in) ⇒ 2
    (read-u8 in) ⇒ 2
    (read-u8 in) ⇒ 3
    (read-u8 in) ⇒ #<eof>
    ```

Scheme Procedure: **read-bytevector!** bv [port [start [end]]] [¶](#index-read_002dbytevector_0021)
:   Reads the next end - start bytes, or as many as are
    available before the end of file, from the binary input port into the
    bytevector bv in left-to-right order beginning at the start
    position. If end is not supplied, reads until the end of bv
    has been reached. If start is not supplied, reads beginning at
    position 0.

    Returns the number of bytes read. If no bytes are available, an
    end-of-file object is returned.

    ```
    (define in (open-input-bytevector #vu8(1 2 3)))
    (define bv (make-bytevector 5 0))
    (read-bytevector! bv in 1 3) ⇒ 2
    bv ⇒ #vu8(0 1 2 0 0 0)
    ```

Scheme Procedure: **read-bytevector** k in [¶](#index-read_002dbytevector)
:   Reads the next k bytes, or as many as are available before the end
    of file if that is less than k, from the binary input port
    in into a newly allocated bytevector in left-to-right order, and
    returns the bytevector. If no bytes are available before the end of
    file, an end-of-file object is returned.

    ```
    (define bv #vu8(1 2 3))
    (read-bytevector 2 (open-input-bytevector bv)) ⇒ #vu8(1 2)
    (read-bytevector 10 (open-input-bytevector bv)) ⇒ #vu8(1 2 3)
    ```

Scheme Procedure: **write-bytevector** bv [port [start [end]]] [¶](#index-write_002dbytevector)
:   Writes the bytes of bytevector bv from start to end in
    left-to-right order to the binary output port. start
    defaults to 0 and end defaults to the length of bv.

    ```
    (define out (open-output-bytevector))
    (write-bytevector #vu8(0 1 2 3 4) out 2 4)
    (get-output-bytevector out) ⇒ #vu8(2 3)
    ```

---

Next: [Textual I/O](#Textual-I_002fO), Previous: [Binary I/O](#Binary-I_002fO), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.3 Encoding [¶](#Encoding-1)

Textual input and output on Guile ports is layered on top of binary
operations. To this end, each port has an associated character encoding
that controls how bytes read from the port are converted to characters,
and how characters written to the port are converted to bytes.

Scheme Procedure: **port-encoding** port [¶](#index-port_002dencoding)

C Function: **scm\_port\_encoding** (port) [¶](#index-scm_005fport_005fencoding)
:   Returns, as a string, the character encoding that port uses to
    interpret its input and output.

Scheme Procedure: **set-port-encoding!** port enc [¶](#index-set_002dport_002dencoding_0021)

C Function: **scm\_set\_port\_encoding\_x** (port, enc) [¶](#index-scm_005fset_005fport_005fencoding_005fx)
:   Sets the character encoding that will be used to interpret I/O to
    port. enc is a string containing the name of an encoding.
    Valid encoding names are those
    [defined by IANA](http://www.iana.org/assignments/character-sets),
    for example `"UTF-8"` or `"ISO-8859-1"`.

When ports are created, they are assigned an encoding. The usual
process to determine the initial encoding for a port is to take the
value of the `%default-port-encoding` fluid.

Scheme Variable: **%default-port-encoding** [¶](#index-_0025default_002dport_002dencoding)
:   A fluid containing name of the encoding to be used by default for newly
    created ports (see [Fluids and Dynamic States](#Fluids-and-Dynamic-States)). As a special case,
    the value `#f` is equivalent to `"ISO-8859-1"`.

The `%default-port-encoding` itself defaults to the encoding
appropriate for the current locale, if `setlocale` has been called.
See [Locales](#Locales), for more on locales and when you might need to call
`setlocale` explicitly.

Some port types have other ways of determining their initial locales.
String ports, for example, default to the UTF-8 encoding, in order to be
able to represent all characters regardless of the current locale. File
ports can optionally sniff their file for a `coding:` declaration;
See [File Ports](#File-Ports). Binary ports might be initialized to the ISO-8859-1
encoding in which each codepoint between 0 and 255 corresponds to a byte
with that value.

Currently, the ports only work with *non-modal* encodings. Most
encodings are non-modal, meaning that the conversion of bytes to a
string doesn’t depend on its context: the same byte sequence will always
return the same string. A couple of modal encodings are in common use,
like ISO-2022-JP and ISO-2022-KR, and they are not yet supported.

Each port also has an associated conversion strategy, which determines
what to do when a Guile character can’t be converted to the port’s
encoded character representation for output. There are three possible
strategies: to raise an error, to replace the character with a hex
escape, or to replace the character with a substitute character. Port
conversion strategies are also used when decoding characters from an
input port.

Scheme Procedure: **port-conversion-strategy** port [¶](#index-port_002dconversion_002dstrategy)

C Function: **scm\_port\_conversion\_strategy** (port) [¶](#index-scm_005fport_005fconversion_005fstrategy)
:   Returns the behavior of the port when outputting a character that is not
    representable in the port’s current encoding.

    If port is `#f`, then the current default behavior will be
    returned. New ports will have this default behavior when they are
    created.

Scheme Procedure: **set-port-conversion-strategy!** port sym [¶](#index-set_002dport_002dconversion_002dstrategy_0021)

C Function: **scm\_set\_port\_conversion\_strategy\_x** (port, sym) [¶](#index-scm_005fset_005fport_005fconversion_005fstrategy_005fx)
:   Sets the behavior of Guile when outputting a character that is not
    representable in the port’s current encoding, or when Guile encounters a
    decoding error when trying to read a character. sym can be either
    `error`, `substitute`, or `escape`.

    If port is an open port, the conversion error behavior is set for
    that port. If it is `#f`, it is set as the default behavior for
    any future ports that get created in this thread.

As with port encodings, there is a fluid which determines the initial
conversion strategy for a port.

Scheme Variable: **%default-port-conversion-strategy** [¶](#index-_0025default_002dport_002dconversion_002dstrategy)
:   The fluid that defines the conversion strategy for newly created ports,
    and also for other conversion routines such as `scm_to_stringn`,
    `scm_from_stringn`, `string->pointer`, and
    `pointer->string`.

    Its value must be one of the symbols described above, with the same
    semantics: `error`, `substitute`, or `escape`.

    When Guile starts, its value is `substitute`.

    Note that `(set-port-conversion-strategy! #f sym)` is
    equivalent to `(fluid-set! %default-port-conversion-strategy
    sym)`.

As mentioned above, for an output port there are three possible port
conversion strategies. The `error` strategy will throw an error
when a nonconvertible character is encountered. The `substitute`
strategy will replace nonconvertible characters with a question mark
(‘`?`’). Finally the `escape` strategy will print
nonconvertible characters as a hex escape, using the escaping that is
recognized by Guile’s string syntax. Note that if the port’s encoding
is a Unicode encoding, like `UTF-8`, then encoding errors are
impossible.

For an input port, the `error` strategy will cause Guile to throw
an error if it encounters an invalid encoding, such as might happen if
you tried to read `ISO-8859-1` as `UTF-8`. The error is
thrown before advancing the read position. The `substitute`
strategy will replace the bad bytes with a U+FFFD replacement character,
in accordance with Unicode recommendations. When reading from an input
port, the `escape` strategy is treated as if it were `error`.

---

Next: [Simple Textual Output](#Simple-Output), Previous: [Encoding](#Encoding), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.4 Textual I/O [¶](#Textual-I_002fO-1)

This section describes Guile’s core textual I/O operations on characters
and strings. See [Binary I/O](#Binary-I_002fO), for input and output of bytes and
bytevectors. See [Encoding](#Encoding), for more on how characters relate to
bytes. To read general S-expressions from ports, See [Reading Scheme Code](#Scheme-Read).
See [Writing Scheme Values](#Scheme-Write), for interfaces that write generic Scheme datums.

To use these routines, first include the textual I/O module:

```
(use-modules (ice-9 textual-ports))
```

Note that although this module’s name suggests that textual ports are
some different kind of port, that’s not the case: all ports in Guile are
both binary and textual ports.

Scheme Procedure: **get-char** input-port [¶](#index-get_002dchar)
:   Reads from input-port, blocking as necessary, until a
    complete character is available from input-port,
    or until an end of file is reached.

    If a complete character is available before the next end of file,
    `get-char` returns that character and updates the input port to
    point past the character. If an end of file is reached before any
    character is read, `get-char` returns the end-of-file object.

Scheme Procedure: **lookahead-char** input-port [¶](#index-lookahead_002dchar)
:   The `lookahead-char` procedure is like `get-char`, but it does
    not update input-port to point past the character.

In the same way that it’s possible to "unget" a byte or bytes, it’s
possible to "unget" the bytes corresponding to an encoded character.

Scheme Procedure: **unget-char** port char [¶](#index-unget_002dchar)
:   Place character char in port so that it will be read by the
    next read operation. If called multiple times, the unread characters
    will be read again in last-in first-out order.

Scheme Procedure: **unget-string** port str [¶](#index-unget_002dstring)
:   Place the string str in port so that its characters will
    be read from left-to-right as the next characters from port
    during subsequent read operations. If called multiple times, the
    unread characters will be read again in last-in first-out order.

Reading in a character at a time can be inefficient. If it’s possible
to perform I/O over multiple characters at a time, via strings, that
might be faster.

Scheme Procedure: **get-string-n** input-port count [¶](#index-get_002dstring_002dn)
:   The `get-string-n` procedure reads from input-port, blocking
    as necessary, until count characters are available, or until an
    end of file is reached. count must be an exact, non-negative
    integer, representing the number of characters to be read.

    If count characters are available before end of file,
    `get-string-n` returns a string consisting of those count
    characters. If fewer characters are available before an end of file, but
    one or more characters can be read, `get-string-n` returns a string
    containing those characters. In either case, the input port is updated
    to point just past the characters read. If no characters can be read
    before an end of file, the end-of-file object is returned.

Scheme Procedure: **get-string-n!** input-port string start count [¶](#index-get_002dstring_002dn_0021)
:   The `get-string-n!` procedure reads from input-port in the
    same manner as `get-string-n`. start and count must be
    exact, non-negative integer objects, with count representing the
    number of characters to be read. string must be a string with at
    least $start + count$ characters.

    If count characters are available before an end of file, they are
    written into string starting at index start, and count
    is returned. If fewer characters are available before an end of file,
    but one or more can be read, those characters are written into
    string starting at index start and the number of characters
    actually read is returned as an exact integer object. If no characters
    can be read before an end of file, the end-of-file object is returned.

Scheme Procedure: **get-string-all** input-port [¶](#index-get_002dstring_002dall)
:   Reads from input-port until an end of file, decoding characters in
    the same manner as `get-string-n` and `get-string-n!`.

    If characters are available before the end of file, a string containing
    all the characters decoded from that data are returned. If no character
    precedes the end of file, the end-of-file object is returned.

Scheme Procedure: **get-line** input-port [¶](#index-get_002dline)
:   Reads from input-port up to and including the linefeed
    character or end of file, decoding characters in the same manner as
    `get-string-n` and `get-string-n!`.

    If a linefeed character is read, a string containing all of the text up
    to (but not including) the linefeed character is returned, and the port
    is updated to point just past the linefeed character. If an end of file
    is encountered before any linefeed character is read, but some
    characters have been read and decoded as characters, a string containing
    those characters is returned. If an end of file is encountered before
    any characters are read, the end-of-file object is returned.

Finally, there are just two core procedures to write characters to a
port.

Scheme Procedure: **put-char** port char [¶](#index-put_002dchar)
:   Writes char to the port. The `put-char` procedure returns
    an unspecified value.

Scheme Procedure: **put-string** port string [¶](#index-put_002dstring)

Scheme Procedure: **put-string** port string start [¶](#index-put_002dstring-1)

Scheme Procedure: **put-string** port string start count [¶](#index-put_002dstring-2)
:   Write the count characters of string starting at index
    start to the port.

    start and count must be non-negative exact integer objects.
    string must have a length of at least *start +
    count*. start defaults to 0. count defaults to
    *`(string-length string)` - start*$.

    Calling `put-string` is equivalent in all respects to calling
    `put-char` on the relevant sequence of characters, except that it
    will attempt to write multiple characters to the port at a time, even if
    the port is unbuffered.

    The `put-string` procedure returns an unspecified value.

Textual ports have a textual position associated with them: a line and a
column. Reading in characters or writing them out advances the line and
the column appropriately.

Scheme Procedure: **port-column** port [¶](#index-port_002dcolumn)

Scheme Procedure: **port-line** port [¶](#index-port_002dline)

C Function: **scm\_port\_column** (port) [¶](#index-scm_005fport_005fcolumn)

C Function: **scm\_port\_line** (port) [¶](#index-scm_005fport_005fline)
:   Return the current column number or line number of port.

Port lines and positions are represented as 0-origin integers, which is
to say that the first character of the first line is line 0, column
0. However, when you display a line number, for example in an error
message, we recommend you add 1 to get 1-origin integers. This is
because lines numbers traditionally start with 1, and that is what
non-programmers will find most natural.

Scheme Procedure: **set-port-column!** port column [¶](#index-set_002dport_002dcolumn_0021)

Scheme Procedure: **set-port-line!** port line [¶](#index-set_002dport_002dline_0021)

C Function: **scm\_set\_port\_column\_x** (port, column) [¶](#index-scm_005fset_005fport_005fcolumn_005fx)

C Function: **scm\_set\_port\_line\_x** (port, line) [¶](#index-scm_005fset_005fport_005fline_005fx)
:   Set the current column or line number of port.

---

Next: [Buffering](#Buffering), Previous: [Textual I/O](#Textual-I_002fO), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.5 Simple Textual Output [¶](#Simple-Textual-Output)

Guile exports a simple formatted output function, `simple-format`.
For a more capable formatted output facility, See [Formatted Output](#Formatted-Output).

Scheme Procedure: **simple-format** destination message . args [¶](#index-simple_002dformat)

C Function: **scm\_simple\_format** (destination, message, args) [¶](#index-scm_005fsimple_005fformat)
:   Write message to destination, defaulting to the current
    output port. message can contain `~A` and `~S` escapes.
    When printed, the escapes are replaced with corresponding members of
    args: `~A` formats using `display` and `~S` formats
    using `write`. If destination is `#t`, then use the
    current output port, if destination is `#f`, then return a
    string containing the formatted text. Does not add a trailing newline.

Somewhat confusingly, Guile binds the `format` identifier to
`simple-format` at startup. Once `(ice-9 format)` loads, it
actually replaces the core `format` binding, so depending on
whether you or a module you use has loaded `(ice-9 format)`, you
may be using the simple or the more capable version.

---

Next: [Random Access](#Random-Access), Previous: [Simple Textual Output](#Simple-Output), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.6 Buffering [¶](#Buffering-1)

Every port has associated input and output buffers. You can think of
ports as being backed by some mutable store, and that store might be far
away. For example, ports backed by file descriptors have to go all the
way to the kernel to read and write their data. To avoid this
round-trip cost, Guile usually reads in data from the mutable store in
chunks, and then services small requests like `get-char` out of
that intermediate buffer. Similarly, small writes like
`write-char` first go to a buffer, and are sent to the store when
the buffer is full (or when port is flushed). Buffered ports speed up
your program by reducing the number of round-trips to the mutable store,
and they do so in a way that is mostly transparent to the user.

There are two major ways, however, in which buffering affects program
semantics. Building correct, performant programs requires understanding
these situations.

The first case is in random-access read/write ports (see [Random Access](#Random-Access)). These ports, usually backed by a file, logically operate over
the same mutable store when both reading and writing. So, if you read a
character, causing the buffer to fill, then write a character, the bytes
you filled in your read buffer are now invalid. Every time you switch
between reading and writing, Guile has to flush any pending buffer. If
this happens frequently, the cost can be high. In that case you should
reduce the amount that you buffer, in both directions. Similarly, Guile
has to flush buffers before seeking. None of these considerations apply
to sockets, which don’t logically read from and write to the same
mutable store, and are not seekable. Note also that sockets are
unbuffered by default. See [Network Sockets and Communication](#Network-Sockets-and-Communication).

The second case is the more pernicious one. If you write data to a
buffered port, it probably doesn’t go out to the mutable store directly.
(This “probably” introduces some indeterminism in your program: what
goes to the store, and when, depends on how full the buffer is. It is
something that the user needs to explicitly be aware of.) The data is
written to the store later – when the buffer fills up due to another
write, or when `force-output` is called, or when `close-port`
is called, or when the program exits, or even when the garbage collector
runs. The salient point is, *the errors are signaled then too*.
Buffered writes defer error detection (and defer the side effects to the
mutable store), perhaps indefinitely if the port type does not need to
be closed at GC.

One common heuristic that works well for textual ports is to flush
output when a newline (`\n`) is written. This *line buffering*
mode is on by default for TTY ports. Most other ports are *block
buffered*, meaning that once the output buffer reaches the block size,
which depends on the port and its configuration, the output is flushed
as a block, without regard to what is in the block. Likewise reads are
read in at the block size, though if there are fewer bytes available to
read, the buffer may not be entirely filled.

Note that binary reads or writes that are larger than the buffer size go
directly to the mutable store without passing through the buffers. If
your access pattern involves many big reads or writes, buffering might
not matter so much to you.

To control the buffering behavior of a port, use `setvbuf`.

Scheme Procedure: **setvbuf** port mode [size] [¶](#index-setvbuf)

C Function: **scm\_setvbuf** (port, mode, size) [¶](#index-scm_005fsetvbuf)
:   Set the buffering mode for port. mode can be one of the
    following symbols:

    `none`
    :   non-buffered

    `line`
    :   line buffered

    `block`
    :   block buffered, using a newly allocated buffer of size bytes.
        If size is omitted, a default size will be used.

Another way to set the buffering, for file ports, is to open the file
with `0` or `l` as part of the mode string, for unbuffered or
line-buffered ports, respectively. See [File Ports](#File-Ports), for more.

Any buffered output data will be written out when the port is closed.
To make sure to flush it at specific points in your program, use
`force-output`.

Scheme Procedure: **force-output** [port] [¶](#index-force_002doutput)

C Function: **scm\_force\_output** (port) [¶](#index-scm_005fforce_005foutput)
:   Flush the specified output port, or the current output port if
    port is omitted. The current output buffer contents, if any, are
    passed to the underlying port implementation.

    The return value is unspecified.

Scheme Procedure: **flush-all-ports** [¶](#index-flush_002dall_002dports)

C Function: **scm\_flush\_all\_ports** () [¶](#index-scm_005fflush_005fall_005fports)
:   Equivalent to calling `force-output` on all open output ports. The
    return value is unspecified.

Similarly, sometimes you might want to switch from using Guile’s ports
to working directly on file descriptors. In that case, for input ports
use `drain-input` to get any buffered input from that port.

Scheme Procedure: **drain-input** port [¶](#index-drain_002dinput)

C Function: **scm\_drain\_input** (port) [¶](#index-scm_005fdrain_005finput)
:   This procedure clears a port’s input buffers, similar
    to the way that force-output clears the output buffer. The
    contents of the buffers are returned as a single string, e.g.,

    ```
    (define p (open-input-file ...))
    (drain-input p) => empty string, nothing buffered yet.
    (unread-char (read-char p) p)
    (drain-input p) => initial chars from p, up to the buffer size.
    ```

All of these considerations are very similar to those of streams in the
C library, although Guile’s ports are not built on top of C streams.
Still, it is useful to read what other systems do.
See [Streams](/libc/latest/en/libc.html#Streams) in The GNU C Library Reference Manual, for more
discussion on C streams.

---

Next: [Line Oriented and Delimited Text](#Line_002fDelimited), Previous: [Buffering](#Buffering), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.7 Random Access [¶](#Random-Access-1)

Scheme Procedure: **seek** fd\_port offset whence [¶](#index-seek)

C Function: **scm\_seek** (fd\_port, offset, whence) [¶](#index-scm_005fseek)
:   Sets the current position of fd\_port to the integer
    offset. For a file port, offset is expressed
    as a number of bytes; for other types of ports, such as string
    ports, offset is an abstract representation of the
    position within the port’s data, not necessarily expressed
    as a number of bytes. offset is interpreted according to
    the value of whence.

    One of the following variables should be supplied for
    whence:

    Variable: **SEEK\_SET** [¶](#index-SEEK_005fSET)
    :   Seek from the beginning of the file.

    Variable: **SEEK\_CUR** [¶](#index-SEEK_005fCUR)
    :   Seek from the current position.

    Variable: **SEEK\_END** [¶](#index-SEEK_005fEND)
    :   Seek from the end of the file.

    On systems that support it, such as GNU/Linux, the following
    constants can be used for whence to navigate “holes” in
    sparse files:

    Variable: **SEEK\_DATA** [¶](#index-SEEK_005fDATA)
    :   Seek to the next location in the file greater than or equal to
        offset containing data. If offset points to data,
        then the file offset is set to offset.

    Variable: **SEEK\_HOLE** [¶](#index-SEEK_005fHOLE)
    :   Seek to the next hole in the file greater than or equal to the
        offset. If offset points into the middle of a hole,
        then the file offset is set to offset. If there is no hole
        past offset, then the file offset is adjusted to the end of
        the file—i.e., there is an implicit hole at the end of any file.

    If fd\_port is a file descriptor, the underlying system call
    is `lseek` (see [File Position Primitive](/libc/latest/en/libc.html#File-Position-Primitive) in The GNU C
    Library Reference Manual). port may be a string port.

    The value returned is the new position in fd\_port. This means
    that the current position of a port can be obtained using:

    ```
    (seek port 0 SEEK_CUR)
    ```

Scheme Procedure: **ftell** fd\_port [¶](#index-ftell)

C Function: **scm\_ftell** (fd\_port) [¶](#index-scm_005fftell)
:   Return an integer representing the current position of
    fd\_port, measured from the beginning. Equivalent to:

    ```
    (seek port 0 SEEK_CUR)
    ```

Scheme Procedure: **truncate-file** file [length] [¶](#index-truncate_002dfile)

C Function: **scm\_truncate\_file** (file, length) [¶](#index-scm_005ftruncate_005ffile)
:   Truncate file to length bytes. file can be a
    filename string, a port object, or an integer file descriptor. The
    return value is unspecified.

    For a port or file descriptor length can be omitted, in which
    case the file is truncated at the current position (per `ftell`
    above).

    On most systems a file can be extended by giving a length greater than
    the current size, but this is not mandatory in the POSIX standard.

---

Next: [Default Ports for Input, Output and Errors](#Default-Ports), Previous: [Random Access](#Random-Access), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.8 Line Oriented and Delimited Text [¶](#Line-Oriented-and-Delimited-Text)

The delimited-I/O module can be accessed with:

```
(use-modules (ice-9 rdelim))
```

It can be used to read or write lines of text, or read text delimited by
a specified set of characters.

Scheme Procedure: **read-line** [port] [handle-delim] [¶](#index-read_002dline)
:   Return a line of text from port if specified, otherwise from the
    value returned by `(current-input-port)`. Under Unix, a line of text
    is terminated by the first end-of-line character or by end-of-file.

    If handle-delim is specified, it should be one of the following
    symbols:

    `trim`
    :   Discard the terminating delimiter. This is the default, but it will
        be impossible to tell whether the read terminated with a delimiter or
        end-of-file.

    `concat`
    :   Append the terminating delimiter (if any) to the returned string.

    `peek`
    :   Push the terminating delimiter (if any) back on to the port.

    `split`
    :   Return a pair containing the string read from the port and the
        terminating delimiter or end-of-file object.

Scheme Procedure: **read-line!** buf [port] [¶](#index-read_002dline_0021)
:   Read a line of text into the supplied string buf and return the
    number of characters added to buf. If buf is filled, then
    `#f` is returned. Read from port if specified, otherwise
    from the value returned by `(current-input-port)`.

Scheme Procedure: **read-delimited** delims [port] [handle-delim] [¶](#index-read_002ddelimited)
:   Read text until one of the characters in the string delims is
    found or end-of-file is reached. Read from port if supplied,
    otherwise from the value returned by `(current-input-port)`.
    handle-delim takes the same values as described for
    `read-line`.

Scheme Procedure: **read-delimited!** delims buf [port] [handle-delim] [start] [end] [¶](#index-read_002ddelimited_0021)
:   Read text into the supplied string buf.

    If a delimiter was found, return the number of characters written,
    except if handle-delim is `split`, in which case the return
    value is a pair, as noted above.

    As a special case, if port was already at end-of-stream, the EOF
    object is returned. Also, if no characters were written because the
    buffer was full, `#f` is returned.

    It’s something of a wacky interface, to be honest.

Scheme Procedure: **%read-delimited!** delims str gobble [port [start [end]]] [¶](#index-_0025read_002ddelimited_0021)

C Function: **scm\_read\_delimited\_x** (delims, str, gobble, port, start, end) [¶](#index-scm_005fread_005fdelimited_005fx)
:   Read characters from port into str until one of the
    characters in the delims string is encountered. If
    gobble is true, discard the delimiter character;
    otherwise, leave it in the input stream for the next read. If
    port is not specified, use the value of
    `(current-input-port)`. If start or end are
    specified, store data only into the substring of str
    bounded by start and end (which default to the
    beginning and end of the string, respectively).

    Return a pair consisting of the delimiter that terminated the
    string and the number of characters read. If reading stopped
    at the end of file, the delimiter returned is the
    eof-object; if the string was filled without encountering
    a delimiter, this value is `#f`.

Scheme Procedure: **%read-line** [port] [¶](#index-_0025read_002dline)

C Function: **scm\_read\_line** (port) [¶](#index-scm_005fread_005fline)
:   Read a newline-terminated line from port, allocating storage as
    necessary. The newline terminator (if any) is removed from the string,
    and a pair consisting of the line and its delimiter is returned. The
    delimiter may be either a newline or the eof-object; if
    `%read-line` is called at the end of file, it returns the pair
    `(#<eof> . #<eof>)`.

Scheme Procedure: **write-line** obj [port] [¶](#index-write_002dline)

C Function: **scm\_write\_line** (obj, port) [¶](#index-scm_005fwrite_005fline)
:   Display obj and a newline character to port. If
    port is not specified, `(current-output-port)` is
    used. This procedure is equivalent to:

    ```
    (display obj [port])
    (newline [port])
    ```

Scheme Procedure: **for-rdelim-from-port** port proc rdelim-proc [#:stop-pred=eof-object?] [¶](#index-for_002drdelim_002dfrom_002dport)
:   For every unit provided by `(rdelim-proc port)`, provide
    this unit(rdelim) to proc to be processed. This will continue throughout
    port until stop-pred returns `#t`.
    stop-pred is `eof-object?` by default.
    rdelim-proc has to advance through port with every call made to it.

Scheme Procedure: **for-delimited-from-port** port proc [#:delims=”\n”] [#:handle-delim=’trim] [¶](#index-for_002ddelimited_002dfrom_002dport)
:   Call proc for every line delimited by delims from port.

Scheme Procedure: **for-line-in-file** file proc [#:encoding=#f] [#:guess-encoding=#f] [¶](#index-for_002dline_002din_002dfile)
:   Call proc for every line in file.
    file must be a filename string.

    The line provided to proc is guaranteed to be a string.

---

Next: [Types of Port](#Port-Types), Previous: [Line Oriented and Delimited Text](#Line_002fDelimited), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.9 Default Ports for Input, Output and Errors [¶](#Default-Ports-for-Input_002c-Output-and-Errors)

Scheme Procedure: **current-input-port** [¶](#index-current_002dinput_002dport)

C Function: **scm\_current\_input\_port** () [¶](#index-scm_005fcurrent_005finput_005fport)
:   Return the current input port. This is the default port used
    by many input procedures.

    Initially this is the *standard input* in Unix and C terminology.
    When the standard input is a TTY the port is unbuffered, otherwise
    it’s fully buffered.

    Unbuffered input is good if an application runs an interactive
    subprocess, since any type-ahead input won’t go into Guile’s buffer
    and be unavailable to the subprocess.

    Note that Guile buffering is completely separate from the TTY “line
    discipline”. In the usual cooked mode on a TTY Guile only sees a
    line of input once the user presses `Return`.

Scheme Procedure: **current-output-port** [¶](#index-current_002doutput_002dport)

C Function: **scm\_current\_output\_port** () [¶](#index-scm_005fcurrent_005foutput_005fport)
:   Return the current output port. This is the default port used
    by many output procedures.

    Initially this is the *standard output* in Unix and C terminology.
    When the standard output is a TTY this port is unbuffered, otherwise
    it’s fully buffered.

    Unbuffered output to a TTY is good for ensuring progress output or a
    prompt is seen. But an application which always prints whole lines
    could change to line buffered, or an application with a lot of output
    could go fully buffered and perhaps make explicit `force-output`
    calls (see [Buffering](#Buffering)) at selected points.

Scheme Procedure: **current-error-port** [¶](#index-current_002derror_002dport)

C Function: **scm\_current\_error\_port** () [¶](#index-scm_005fcurrent_005ferror_005fport)
:   Return the port to which errors and warnings should be sent.

    Initially this is the *standard error* in Unix and C terminology.
    When the standard error is a TTY this port is unbuffered, otherwise
    it’s fully buffered.

Scheme Procedure: **set-current-input-port** port [¶](#index-set_002dcurrent_002dinput_002dport)

Scheme Procedure: **set-current-output-port** port [¶](#index-set_002dcurrent_002doutput_002dport)

Scheme Procedure: **set-current-error-port** port [¶](#index-set_002dcurrent_002derror_002dport)

C Function: **scm\_set\_current\_input\_port** (port) [¶](#index-scm_005fset_005fcurrent_005finput_005fport)

C Function: **scm\_set\_current\_output\_port** (port) [¶](#index-scm_005fset_005fcurrent_005foutput_005fport)

C Function: **scm\_set\_current\_error\_port** (port) [¶](#index-scm_005fset_005fcurrent_005ferror_005fport)
:   Change the ports returned by `current-input-port`,
    `current-output-port` and `current-error-port`, respectively,
    so that they use the supplied port for input or output.

Scheme Procedure: **with-input-from-port** port thunk [¶](#index-with_002dinput_002dfrom_002dport)

Scheme Procedure: **with-output-to-port** port thunk [¶](#index-with_002doutput_002dto_002dport)

Scheme Procedure: **with-error-to-port** port thunk [¶](#index-with_002derror_002dto_002dport)
:   Call thunk in a dynamic environment in which
    `current-input-port`, `current-output-port` or
    `current-error-port` is rebound to the given port.

C Function: `void` **scm\_dynwind\_current\_input\_port** `(SCM port)` [¶](#index-scm_005fdynwind_005fcurrent_005finput_005fport)

C Function: `void` **scm\_dynwind\_current\_output\_port** `(SCM port)` [¶](#index-scm_005fdynwind_005fcurrent_005foutput_005fport)

C Function: `void` **scm\_dynwind\_current\_error\_port** `(SCM port)` [¶](#index-scm_005fdynwind_005fcurrent_005ferror_005fport)
:   These functions must be used inside a pair of calls to
    `scm_dynwind_begin` and `scm_dynwind_end` (see [Dynamic Wind](#Dynamic-Wind)). During the dynwind context, the indicated port is set to
    port.

    More precisely, the current port is swapped with a ‘backup’ value
    whenever the dynwind context is entered or left. The backup value is
    initialized with the port argument.

---

Next: [Venerable Port Interfaces](#Venerable-Port-Interfaces), Previous: [Default Ports for Input, Output and Errors](#Default-Ports), Up: [Input and Output](#Input-and-Output)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.10 Types of Port [¶](#Types-of-Port)

* [File Ports](#File-Ports)
* [Bytevector Ports](#Bytevector-Ports)
* [String Ports](#String-Ports)
* [Custom Ports](#Custom-Ports)
* [Soft Ports](#Soft-Ports)
* [Void Ports](#Void-Ports)
* [Low-Level Custom Ports](#Low_002dLevel-Custom-Ports)
* [Low-Level Custom Ports in C](#Low_002dLevel-Custom-Ports-in-C)

---

Next: [Bytevector Ports](#Bytevector-Ports), Up: [Types of Port](#Port-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.10.1 File Ports [¶](#File-Ports-1)

The following procedures are used to open file ports.
See also [open](#Ports-and-File-Descriptors), for an interface
to the Unix `open` system call.

All file access uses the “LFS” large file support functions when
available, so files bigger than 2 gibibytes (*2^31* bytes) can be
read and written on a 32-bit system.

Most systems have limits on how many files can be open, so it’s
strongly recommended that file ports be closed explicitly when no
longer required (see [Ports](#Ports)).

Scheme Procedure: **open-file** filename mode [#:guess-encoding=#f] [#:encoding=#f] [¶](#index-open_002dfile)

C Function: **scm\_open\_file\_with\_encoding** (filename, mode, guess\_encoding, encoding) [¶](#index-scm_005fopen_005ffile_005fwith_005fencoding)

C Function: **scm\_open\_file** (filename, mode) [¶](#index-scm_005fopen_005ffile)
:   Open the file whose name is filename, and return a port
    representing that file. The attributes of the port are
    determined by the mode string. The way in which this is
    interpreted is similar to C stdio. The first character must be
    one of the following:

    ‘`r`’
    :   Open an existing file for input.

    ‘`w`’
    :   Open a file for output, creating it if it doesn’t already exist
        or removing its contents if it does.

    ‘`a`’
    :   Open a file for output, creating it if it doesn’t already
        exist. All writes to the port will go to the end of the file.
        The "append mode" can be turned off while the port is in use
        see [fcntl](#Ports-and-File-Descriptors)

    The following additional characters can be appended:

    ‘`b`’
    :   Open the underlying file in binary mode, if supported by the system.
        Also, open the file using the binary-compatible character encoding
        "ISO-8859-1", ignoring the default port encoding.

    ‘`+`’
    :   Open the port for both input and output. E.g., `r+`: open
        an existing file for both input and output.

    ‘`e`’
    :   Mark the underlying file descriptor as close-on-exec, as per the
        `O_CLOEXEC` flag.

    ‘`0`’
    :   Create an "unbuffered" port. In this case input and output
        operations are passed directly to the underlying port
        implementation without additional buffering. This is likely to
        slow down I/O operations. The buffering mode can be changed
        while a port is in use (see [Buffering](#Buffering)).

    ‘`l`’
    :   Add line-buffering to the port. The port output buffer will be
        automatically flushed whenever a newline character is written.

    ‘`b`’
    :   Use binary mode, ensuring that each byte in the file will be read as one
        Scheme character.

        To provide this property, the file will be opened with the 8-bit
        character encoding "ISO-8859-1", ignoring the default port encoding.
        See [Ports](#Ports), for more information on port encodings.

        Note that while it is possible to read and write binary data as
        characters or strings, it is usually better to treat bytes as octets,
        and byte sequences as bytevectors. See [Binary I/O](#Binary-I_002fO), for more.

        This option had another historical meaning, for DOS compatibility: in
        the default (textual) mode, DOS reads a CR-LF sequence as one LF byte.
        The `b` flag prevents this from happening, adding `O_BINARY`
        to the underlying `open` call. Still, the flag is generally useful
        because of its port encoding ramifications.

    Unless binary mode is requested, the character encoding of the new port
    is determined as follows: First, if guess-encoding is true, the
    `file-encoding` procedure is used to guess the encoding of the file
    (see [Character Encoding of Source Files](#Character-Encoding-of-Source-Files)). If guess-encoding
    is false or if `file-encoding` fails, encoding is used unless
    it is also false. As a last resort, the default port encoding is used.
    See [Ports](#Ports), for more information on port encodings. It is an error to
    pass a non-false guess-encoding or encoding if binary mode
    is requested.

    If a file cannot be opened with the access requested, `open-file`
    throws an exception.

Scheme Procedure: **open-input-file** filename [#:guess-encoding=#f] [#:encoding=#f] [#:binary=#f] [¶](#index-open_002dinput_002dfile)
:   Open filename for input. If binary is true, open the port
    in binary mode, otherwise use text mode. encoding and
    guess-encoding determine the character encoding as described above
    for `open-file`. Equivalent to

    ```
    (open-file filename
               (if binary "rb" "r")
               #:guess-encoding guess-encoding
               #:encoding encoding)
    ```

Scheme Procedure: **open-output-file** filename [#:encoding=#f] [#:binary=#f] [¶](#index-open_002doutput_002dfile)
:   Open filename for output. If binary is true, open the port
    in binary mode, otherwise use text mode. encoding specifies the
    character encoding as described above for `open-file`. Equivalent
    to

    ```
    (open-file filename
               (if binary "wb" "w")
               #:encoding encoding)
    ```

Scheme Procedure: **call-with-input-file** filename proc [#:guess-encoding=#f] [#:encoding=#f] [#:binary=#f] [¶](#index-call_002dwith_002dinput_002dfile)

Scheme Procedure: **call-with-output-file** filename proc [#:encoding=#f] [#:binary=#f] [¶](#index-call_002dwith_002doutput_002dfile)
:   Open filename for input or output, and call `(proc
    port)` with the resulting port. Return the value returned by
    proc. filename is opened as per `open-input-file` or
    `open-output-file` respectively, and an error is signaled if it
    cannot be opened.

    When proc returns, the port is closed. If proc does not
    return (e.g. if it throws an error), then the port might not be
    closed automatically, though it will be garbage collected in the usual
    way if not otherwise referenced.

Scheme Procedure: **with-input-from-file** filename thunk [#:guess-encoding=#f] [#:encoding=#f] [#:binary=#f] [¶](#index-with_002dinput_002dfrom_002dfile)

Scheme Procedure: **with-output-to-file** filename thunk [#:encoding=#f] [#:binary=#f] [¶](#index-with_002doutput_002dto_002dfile)

Scheme Procedure: **with-error-to-file** filename thunk [#:encoding=#f] [#:binary=#f] [¶](#index-with_002derror_002dto_002dfile)
:   Open filename and call `(thunk)` with the new port
    setup as respectively the `current-input-port`,
    `current-output-port`, or `current-error-port`. Return the
    value returned by thunk. filename is opened as per
    `open-input-file` or `open-output-file` respectively, and an
    error is signaled if it cannot be opened.

    When thunk returns, the port is closed and the previous setting
    of the respective current port is restored.

    The current port setting is managed with `dynamic-wind`, so the
    previous value is restored no matter how thunk exits (eg. an
    exception), and if thunk is re-entered (via a captured
    continuation) then it’s set again to the filename port.

    The port is closed when thunk returns normally, but not when
    exited via an exception or new continuation. This ensures it’s still
    ready for use if thunk is re-entered by a captured continuation.
    Of course the port is always garbage collected and closed in the usual
    way when no longer referenced anywhere.

Scheme Procedure: **port-mode** port [¶](#index-port_002dmode)

C Function: **scm\_port\_mode** (port) [¶](#index-scm_005fport_005fmode)
:   Return the port modes associated with the open port port.
    These will not necessarily be identical to the modes used when
    the port was opened, since modes such as "append" which are
    used only during port creation are not retained.

Scheme Procedure: **port-filename** port [¶](#index-port_002dfilename)

C Function: **scm\_port\_filename** (port) [¶](#index-scm_005fport_005ffilename)
:   Return the filename associated with port, or `#f` if no
    filename is associated with the port.

    port must be open; `port-filename` cannot be used once the
    port is closed.

Scheme Procedure: **set-port-filename!** port filename [¶](#index-set_002dport_002dfilename_0021)

C Function: **scm\_set\_port\_filename\_x** (port, filename) [¶](#index-scm_005fset_005fport_005ffilename_005fx)
:   Change the filename associated with port, using the current input
    port if none is specified. Note that this does not change the port’s
    source of data, but only the value that is returned by
    `port-filename` and reported in diagnostic output.

Scheme Procedure: **file-port?** obj [¶](#index-file_002dport_003f)

C Function: **scm\_file\_port\_p** (obj) [¶](#index-scm_005ffile_005fport_005fp)
:   Determine whether obj is a port that is related to a file.

---

Next: [String Ports](#String-Ports), Previous: [File Ports](#File-Ports), Up: [Types of Port](#Port-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.10.2 Bytevector Ports [¶](#Bytevector-Ports-1)

Scheme Procedure: **open-bytevector-input-port** bv [transcoder] [¶](#index-open_002dbytevector_002dinput_002dport)

C Function: **scm\_open\_bytevector\_input\_port** (bv, transcoder) [¶](#index-scm_005fopen_005fbytevector_005finput_005fport)
:   Return an input port whose contents are drawn from bytevector bv
    (see [Bytevectors](#Bytevectors)).

    The transcoder argument is currently not supported.

Scheme Procedure: **open-bytevector-output-port** [transcoder] [¶](#index-open_002dbytevector_002doutput_002dport)

C Function: **scm\_open\_bytevector\_output\_port** (transcoder) [¶](#index-scm_005fopen_005fbytevector_005foutput_005fport)
:   Return two values: a binary output port and a procedure. The latter
    should be called with zero arguments to obtain a bytevector containing
    the data accumulated by the port, as illustrated below.

    ```
    (call-with-values
      (lambda ()
        (open-bytevector-output-port))
      (lambda (port get-bytevector)
        (display "hello" port)
        (get-bytevector)))

    ⇒ #vu8(104 101 108 108 111)
    ```

    The transcoder argument is currently not supported.

Scheme Procedure: **call-with-output-bytevector** proc [¶](#index-call_002dwith_002doutput_002dbytevector)
:   Call the one-argument procedure proc with a newly created
    bytevector output port. When the function returns, the bytevector
    composed of the characters written into the port is returned.
    proc should not close the port.

Scheme Procedure: **call-with-input-bytevector** bytevector proc [¶](#index-call_002dwith_002dinput_002dbytevector)
:   Call the one-argument procedure proc with a newly created input
    port from which bytevector’s contents may be read. The values
    yielded by the proc is returned.

---

Next: [Custom Ports](#Custom-Ports), Previous: [Bytevector Ports](#Bytevector-Ports), Up: [Types of Port](#Port-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.10.3 String Ports [¶](#String-Ports-1)

Scheme Procedure: **call-with-output-string** proc [¶](#index-call_002dwith_002doutput_002dstring)

C Function: **scm\_call\_with\_output\_string** (proc) [¶](#index-scm_005fcall_005fwith_005foutput_005fstring)
:   Calls the one-argument procedure proc with a newly created output
    port. When the function returns, the string composed of the characters
    written into the port is returned. proc should not close the port.

Scheme Procedure: **call-with-input-string** string proc [¶](#index-call_002dwith_002dinput_002dstring)

C Function: **scm\_call\_with\_input\_string** (string, proc) [¶](#index-scm_005fcall_005fwith_005finput_005fstring)
:   Calls the one-argument procedure proc with a newly
    created input port from which string’s contents may be
    read. The value yielded by the proc is returned.

Scheme Procedure: **with-output-to-string** thunk [¶](#index-with_002doutput_002dto_002dstring)
:   Calls the zero-argument procedure thunk with the current output
    port set temporarily to a new string port. It returns a string
    composed of the characters written to the current output.

Scheme Procedure: **with-input-from-string** string thunk [¶](#index-with_002dinput_002dfrom_002dstring)
:   Calls the zero-argument procedure thunk with the current input
    port set temporarily to a string port opened on the specified
    string. The value yielded by thunk is returned.

Scheme Procedure: **open-input-string** str [¶](#index-open_002dinput_002dstring)

C Function: **scm\_open\_input\_string** (str) [¶](#index-scm_005fopen_005finput_005fstring)
:   Take a string and return an input port that delivers characters
    from the string. The port can be closed by
    `close-input-port`, though its storage will be reclaimed
    by the garbage collector if it becomes inaccessible.

Scheme Procedure: **open-output-string** [¶](#index-open_002doutput_002dstring)

C Function: **scm\_open\_output\_string** () [¶](#index-scm_005fopen_005foutput_005fstring)
:   Return an output port that will accumulate characters for
    retrieval by `get-output-string`. The port can be closed
    by the procedure `close-output-port`, though its storage
    will be reclaimed by the garbage collector if it becomes
    inaccessible.

Scheme Procedure: **get-output-string** port [¶](#index-get_002doutput_002dstring)

C Function: **scm\_get\_output\_string** (port) [¶](#index-scm_005fget_005foutput_005fstring)
:   Given an output port created by `open-output-string`,
    return a string consisting of the characters that have been
    output to the port so far.

    `get-output-string` must be used before closing port, once
    closed the string cannot be obtained.

With string ports, the port-encoding is treated differently than other
types of ports. When string ports are created, they do not inherit a
character encoding from the current locale. They are given a
default locale that allows them to handle all valid string characters.
Typically one should not modify a string port’s character encoding
away from its default. See [Encoding](#Encoding).

---

Next: [Soft Ports](#Soft-Ports), Previous: [String Ports](#String-Ports), Up: [Types of Port](#Port-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.10.4 Custom Ports [¶](#Custom-Ports-1)

Custom ports allow the user to provide input and handle output via
user-supplied procedures. The most basic of these operates on the level
of bytes, calling user-supplied functions to supply bytes for input and
accept bytes for output. In Guile, textual ports are built on top of
binary ports, encoding and decoding their codepoint sequences from the
bytes; the higher-level textual layer for custom ports allows users to
deal in characters instead of bytes.

Before using these procedures, import the appropriate module:

```
(use-modules (ice-9 binary-ports))
(use-modules (ice-9 textual-ports))
```

Scheme Procedure: **make-custom-binary-input-port** id read! get-position set-position! close [¶](#index-make_002dcustom_002dbinary_002dinput_002dport)
:   Return a new custom binary input port named id (a string) whose
    input is drained by invoking read! and passing it a bytevector, an
    index where bytes should be written, and the number of bytes to read.
    The `read!` procedure must return an integer indicating the number
    of bytes read, or `0` to indicate the end-of-file.

    Optionally, if get-position is not `#f`, it must be a thunk
    that will be called when `port-position` is invoked on the custom
    binary port and should return an integer indicating the position within
    the underlying data stream; if get-position was not supplied, the
    returned port does not support `port-position`.

    Likewise, if set-position! is not `#f`, it should be a
    one-argument procedure. When `set-port-position!` is invoked on the
    custom binary input port, set-position! is passed an integer
    indicating the position of the next byte is to read.

    Finally, if close is not `#f`, it must be a thunk. It is
    invoked when the custom binary input port is closed.

    The returned port is fully buffered by default, but its buffering mode
    can be changed using `setvbuf` (see [Buffering](#Buffering)).

    Using a custom binary input port, the `open-bytevector-input-port`
    procedure (see [Bytevector Ports](#Bytevector-Ports)) could be implemented as follows:

    ```
    (define (open-bytevector-input-port source)
      (define position 0)
      (define length (bytevector-length source))

      (define (read! bv start count)
        (let ((count (min count (- length position))))
          (bytevector-copy! source position
                            bv start count)
          (set! position (+ position count))
          count))

      (define (get-position) position)

      (define (set-position! new-position)
        (set! position new-position))

      (make-custom-binary-input-port "the port" read!
                                      get-position set-position!
                                      #f))

    (read (open-bytevector-input-port (string->utf8 "hello")))
    ⇒ hello
    ```

Scheme Procedure: **make-custom-binary-output-port** id write! get-position set-position! close [¶](#index-make_002dcustom_002dbinary_002doutput_002dport)
:   Return a new custom binary output port named id (a string) whose
    output is sunk by invoking write! and passing it a bytevector, an
    index where bytes should be read from this bytevector, and the number of
    bytes to be “written”. The `write!` procedure must return an
    integer indicating the number of bytes actually written; when it is
    passed `0` as the number of bytes to write, it should behave as
    though an end-of-file was sent to the byte sink.

    The other arguments are as for `make-custom-binary-input-port`.

Scheme Procedure: **make-custom-binary-input/output-port** id read! write! get-position set-position! close [¶](#index-make_002dcustom_002dbinary_002dinput_002foutput_002dport)
:   Return a new custom binary input/output port named id (a string).
    The various arguments are the same as for The other arguments are as for
    `make-custom-binary-input-port` and
    `make-custom-binary-output-port`. If buffering is enabled on the
    port, as is the case by default, input will be buffered in both
    directions; See [Buffering](#Buffering). If the set-position! function is
    provided and not `#f`, then the port will also be marked as
    random-access, causing the buffer to be flushed between reads and
    writes.

Scheme Procedure: **make-custom-textual-input-port** id read! get-position set-position! close [¶](#index-make_002dcustom_002dtextual_002dinput_002dport)

Scheme Procedure: **make-custom-textual-output-port** id write! get-position set-position! close [¶](#index-make_002dcustom_002dtextual_002doutput_002dport)

Scheme Procedure: **make-custom-textual-input/output-port** id read! write! get-position set-position! close [¶](#index-make_002dcustom_002dtextual_002dinput_002foutput_002dport)
:   Like their custom binary port counterparts, but for textual ports.
    Concretely this means that instead of being passed a bytevector, the
    read function is passed a mutable string to fill, and likewise for
    the buffer supplied to write. Port positions are still expressed
    in bytes, however.

    If string ports were not supplied with Guile, we could implement them
    With custom textual ports:

    ```
    (define (open-string-input-port source)
      (define position 0)
      (define length (string-length source))

      (define (read! dst start count)
        (let ((count (min count (- length position))))
          (string-copy! dst start source position (+ position count))
          (set! position (+ position count))
          count))

      (make-custom-textual-input-port "strport" read! #f #f #f))

    (read (open-string-input-port "hello"))
    ```

---

Next: [Void Ports](#Void-Ports), Previous: [Custom Ports](#Custom-Ports), Up: [Types of Port](#Port-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.10.5 Soft Ports [¶](#Soft-Ports-1)

Soft ports are what Guile had before it had custom binary and textual
ports, and allow for customizable textual input and output.

We recommend soft ports over R6RS custom textual ports because they are
easier to use while also being more expressive. R6RS custom textual
ports operate under the principle that a port has a mutable string
buffer, and this is reflected in the `read` and `write`
procedures which take a buffer, offset, and length. However in Guile as
all ports have a byte buffer rather than some having a string buffer,
the R6RS interface imposes overhead and complexity.

Additionally, and unlike the R6RS interfaces, `make-soft-port` from
the `(ice-9 soft-ports)` module accepts keyword arguments, allowing
for its functionality to be extended over time.

If you find yourself needing more power, notably the ability to seek,
probably you want to use low-level custom ports. See [Low-Level Custom Ports](#Low_002dLevel-Custom-Ports).

```
(use-modules (ice-9 soft-ports))
```

Scheme Procedure: **make-soft-port** [#:id] [#:read-string] [#:write-string] [#:input-waiting?] [#:close] [#:close-on-gc?] [¶](#index-make_002dsoft_002dport)
:   Return a new port. If the read-string keyword argument is
    present, the port will be an input port. If write-string is
    present, the port will be an output port. If both are supplied, the
    port will be open for input and output.

    When the port’s internal buffers are empty, read-string will be
    called with no arguments, and should return a string, or `#f` to
    indicate end-of-stream. Similarly when a port flushes its write buffer,
    the characters in that buffer will be passed to the write-string
    procedure as its single argument. write-string returns
    unspecified values.

    If supplied, input-waiting? should return `#t` if the soft
    port has input which would be returned directly by read-string.

    If supplied, close will be called when the port is closed, with no
    arguments. If close-on-gc? is `#t`, close will
    additionally be called when the port becomes unreachable, after flushing
    any pending write buffers.

With soft ports, the `open-string-input-port` example from the
previous section is more simple:

```
(define (open-string-input-port source)
  (define already-read? #f)

  (define (read-string)
    (cond
     (already-read? "")
     (else
      (set! already-read? #t)
      source)))

  (make-soft-port #:id "strport" #:read-string read-string))
```

Note that there was an earlier form of `make-soft-port` which was
exposed in Guile’s default environment, and which is still there. Its
interface is more clumsy and its users historically expect unbuffered
input. This interface will be deprecated, but we document it here.

Scheme Procedure: **deprecated-make-soft-port** pv modes [¶](#index-deprecated_002dmake_002dsoft_002dport)
:   Return a port capable of receiving or delivering characters as
    specified by the modes string (see [open-file](#File-Ports)). pv must be a vector of length 5 or 6. Its
    components are as follows:

    0. procedure accepting one character for output
    1. procedure accepting a string for output
    2. thunk for flushing output
    3. thunk for getting one character
    4. thunk for closing port (not by garbage collection)
    5. (if present and not `#f`) thunk for computing the number of
       characters that can be read from the port without blocking.

    For an output-only port only elements 0, 1, 2, and 4 need be
    procedures. For an input-only port only elements 3 and 4 need
    be procedures. Thunks 2 and 4 can instead be `#f` if
    there is no useful operation for them to perform.

    If thunk 3 returns `#f` or an `eof-object`
    (see [eof-object?](/r5rs/latest/en/r5rs.html#Input) in The Revised^5 Report on
    Scheme) it indicates that the port has reached end-of-file.
    For example:

    ```
    (define stdout (current-output-port))
    (define p (deprecated-make-soft-port
               (vector
                (lambda (c) (write c stdout))
                (lambda (s) (display s stdout))
                (lambda () (display "." stdout))
                (lambda () (char-upcase (read-char)))
                (lambda () (display "@" stdout)))
               "rw"))

    (write p p) ⇒ #<input-output: soft 8081e20>
    ```

---

Next: [Low-Level Custom Ports](#Low_002dLevel-Custom-Ports), Previous: [Soft Ports](#Soft-Ports), Up: [Types of Port](#Port-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.10.6 Void Ports [¶](#Void-Ports-1)

This kind of port causes any data to be discarded when written to, and
always returns the end-of-file object when read from.

Scheme Procedure: **%make-void-port** mode [¶](#index-_0025make_002dvoid_002dport)

C Function: **scm\_sys\_make\_void\_port** (mode) [¶](#index-scm_005fsys_005fmake_005fvoid_005fport)
:   Create and return a new void port. A void port acts like
    `/dev/null`. The mode argument
    specifies the input/output modes for this port: see the
    documentation for `open-file` in [File Ports](#File-Ports).

---

Next: [Low-Level Custom Ports in C](#Low_002dLevel-Custom-Ports-in-C), Previous: [Void Ports](#Void-Ports), Up: [Types of Port](#Port-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.12.10.7 Low-Level Custom Ports [¶](#Low_002dLevel-Custom-Ports-1)

This section describes how to implement a new kind of port using Guile’s
lowest-level, most primitive interfaces. First, load the `(ice-9
custom-ports)` module:

```
(use-modules (ice-9 custom-ports))
```

Then to make a new port, call `make-custom-port`:

Scheme Procedure: **make-custom-port** [#:read] [#:write] [#:read-wait-fd] [#:write-wait-fd] [#:input-waiting?] [#:seek] [#:random-access?] [#:get-natural-buffer-sizes] [#:id] [#:print] [#:close] [#:close-on-gc?] [#:truncate] [#:encoding] [#:conversion-strategy] [¶](#index-make_002dcustom_002dport)
:   Make a new custom port.

    See [Encoding](#Encoding), for more on `#:encoding` and
    `#:conversion-strategy`.

A port has a number of associated procedures and properties which
collectively implement its behavior. Creating a new custom port mostly
involves writing these procedures, which are passed as keyword arguments
to `make-custom-port`.

Scheme Port Method: **#:read** port dst start count [¶](#index-_0023_003aread)
:   A port’s `#:read` implementation fills read buffers. It should
    copy bytes to the supplied bytevector dst, starting at offset
    start and continuing for count bytes, and return the number
    of bytes that were read, or `#f` to indicate that reading any bytes
    would block.

Scheme Port Method: **#:write** port src start count [¶](#index-_0023_003awrite)
:   A port’s `#:write` implementation flushes write buffers to the
    mutable store. It should write out bytes from the supplied bytevector
    src, starting at offset start and continuing for count
    bytes, and return the number of bytes that were written, or `#f` to
    indicate writing any bytes would block.

If `make-custom-port` is passed a `#:read` argument, the port
will be an input port. Passing a `#:write` argument will make an
output port, and passing both will make an input-output port.

Scheme Port Method: **#:read-wait-fd** port [¶](#index-_0023_003aread_002dwait_002dfd)

Scheme Port Method: **#:write-wait-fd** port [¶](#index-_0023_003awrite_002dwait_002dfd)
:   If a port’s `#:read` or `#:write` method returns `#f`,
    that indicates that reading or writing would block, and that Guile
    should instead `poll` on the file descriptor returned by the port’s
    `#:read-wait-fd` or `#:write-wait-fd` method, respectively,
    until the operation can complete. See [Non-Blocking I/O](#Non_002dBlocking-I_002fO), for a more
    in-depth discussion.

    These methods must be implemented if the `#:read` or `#:write`
    method can return `#f`, and should return a non-negative integer
    file descriptor. However they may be called explicitly by a user, for
    example to determine if a port may eventually be readable or writable.
    If there is no associated file descriptor with the port, they should
    return `#f`. The default implementation returns `#f`.

Scheme Port Method: **#:input-waiting?** port [¶](#index-_0023_003ainput_002dwaiting_003f)
:   In rare cases it is useful to be able to know whether data can be read
    from a port. For example, if the user inputs `1 2 3` at the
    interactive console, after reading and evaluating `1` the console
    shouldn’t then print another prompt before reading and evaluating
    `2` because there is input already waiting. If the port can look
    ahead, then it should implement the `#:input-waiting?` method,
    which returns `#t` if input is available, or `#f` reading the
    next byte would block. The default implementation returns `#t`.

Scheme Port Method: **#:seek** port offset whence [¶](#index-_0023_003aseek)
:   Set or get the current byte position of the port. Guile will flush read
    and/or write buffers before seeking, as appropriate. The offset
    and whence parameters are as for the `seek` procedure;
    See [Random Access](#Random-Access).

    The `#:seek` method returns the byte position after seeking. To
    query the current position, `#:seek` will be called with an
    offset of 0 and `SEEK_CUR` for whence. Other values of
    offset and/or whence will actually perform the seek. The
    `#:seek` method should throw an error if the port is not seekable,
    which is what the default implementation does.

Scheme Port Method: **#:truncate** port [¶](#index-_0023_003atruncate)
:   Truncate the port data to be specified length. Guile will flush buffers
    beforehand, as appropriate. The default implementation throws an error,
    indicating that truncation is not supported for this port.

Scheme Port Method: **#:random-access?** port [¶](#index-_0023_003arandom_002daccess_003f)
:   Return `#t` if port is open for random access, or `#f`
    otherwise.
