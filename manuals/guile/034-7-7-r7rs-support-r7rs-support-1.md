As a side effect, however, `transcoded-port`
    closes binary-port in
    a special way that allows the new textual port to continue to
    use the byte source or sink represented by binary-port,
    even though binary-port itself is closed and cannot
    be used by the input and output operations described in this
    chapter.

Scheme Procedure: **port-position** port [¶](#index-port_002dposition)
:   Equivalent to `(seek port 0 SEEK_CUR)`. See [Random Access](#Random-Access).

Scheme Procedure: **port-has-port-position?** port [¶](#index-port_002dhas_002dport_002dposition_003f)
:   Return `#t` is port supports `port-position`.

Scheme Procedure: **set-port-position!** port offset [¶](#index-set_002dport_002dposition_0021)
:   Equivalent to `(seek port offset SEEK_SET)`.
    See [Random Access](#Random-Access).

Scheme Procedure: **port-has-set-port-position!?** port [¶](#index-port_002dhas_002dset_002dport_002dposition_0021_003f)
:   Return `#t` is port supports `set-port-position!`.

Scheme Procedure: **port-eof?** input-port [¶](#index-port_002deof_003f)
:   Equivalent to `(eof-object? (lookahead-u8 input-port))`.

Scheme Procedure: **standard-input-port** [¶](#index-standard_002dinput_002dport)

Scheme Procedure: **standard-output-port** [¶](#index-standard_002doutput_002dport)

Scheme Procedure: **standard-error-port** [¶](#index-standard_002derror_002dport)
:   Returns a fresh binary input port connected to standard input, or a
    binary output port connected to the standard output or standard error,
    respectively. Whether the port supports the `port-position` and
    `set-port-position!` operations is implementation-dependent.

Scheme Procedure: **current-input-port** [¶](#index-current_002dinput_002dport-1)

Scheme Procedure: **current-output-port** [¶](#index-current_002doutput_002dport-1)

Scheme Procedure: **current-error-port** [¶](#index-current_002derror_002dport-1)
:   See [Default Ports for Input, Output and Errors](#Default-Ports).

Scheme Procedure: **open-bytevector-input-port** bv [transcoder] [¶](#index-open_002dbytevector_002dinput_002dport-1)

Scheme Procedure: **open-bytevector-output-port** [transcoder] [¶](#index-open_002dbytevector_002doutput_002dport-1)
:   See [Bytevector Ports](#Bytevector-Ports).

Scheme Procedure: **make-custom-binary-input-port** id read! get-position set-position! close [¶](#index-make_002dcustom_002dbinary_002dinput_002dport-1)

Scheme Procedure: **make-custom-binary-output-port** id write! get-position set-position! close [¶](#index-make_002dcustom_002dbinary_002doutput_002dport-1)

Scheme Procedure: **make-custom-binary-input/output-port** id read! write! get-position set-position! close [¶](#index-make_002dcustom_002dbinary_002dinput_002foutput_002dport-1)
:   See [Custom Ports](#Custom-Ports).

Scheme Procedure: **make-custom-textual-input-port** id read! get-position set-position! close [¶](#index-make_002dcustom_002dtextual_002dinput_002dport-1)

Scheme Procedure: **make-custom-textual-output-port** id write! get-position set-position! close [¶](#index-make_002dcustom_002dtextual_002doutput_002dport-1)

Scheme Procedure: **make-custom-textual-input/output-port** id read! write! get-position set-position! close [¶](#index-make_002dcustom_002dtextual_002dinput_002foutput_002dport-1)
:   See [Custom Ports](#Custom-Ports).

Scheme Procedure: **get-u8** port [¶](#index-get_002du8-1)

Scheme Procedure: **lookahead-u8** port [¶](#index-lookahead_002du8-1)

Scheme Procedure: **get-bytevector-n** port count [¶](#index-get_002dbytevector_002dn-1)

Scheme Procedure: **get-bytevector-n!** port bv start count [¶](#index-get_002dbytevector_002dn_0021-1)

Scheme Procedure: **get-bytevector-some** port [¶](#index-get_002dbytevector_002dsome-1)

Scheme Procedure: **get-bytevector-all** port [¶](#index-get_002dbytevector_002dall-1)

Scheme Procedure: **put-u8** port octet [¶](#index-put_002du8-1)

Scheme Procedure: **put-bytevector** port bv [start [count]] [¶](#index-put_002dbytevector-1)
:   See [Binary I/O](#Binary-I_002fO).

Scheme Procedure: **get-char** textual-input-port [¶](#index-get_002dchar-1)

Scheme Procedure: **lookahead-char** textual-input-port [¶](#index-lookahead_002dchar-1)

Scheme Procedure: **get-string-n** textual-input-port count [¶](#index-get_002dstring_002dn-1)

Scheme Procedure: **get-string-n!** textual-input-port string start count [¶](#index-get_002dstring_002dn_0021-1)

Scheme Procedure: **get-string-all** textual-input-port [¶](#index-get_002dstring_002dall-1)

Scheme Procedure: **get-line** textual-input-port [¶](#index-get_002dline-1)

Scheme Procedure: **put-char** port char [¶](#index-put_002dchar-1)

Scheme Procedure: **put-string** port string [start [count]] [¶](#index-put_002dstring-3)
:   See [Textual I/O](#Textual-I_002fO).

Scheme Procedure: **get-datum** textual-input-port count [¶](#index-get_002ddatum)
:   Reads an external representation from textual-input-port and returns the
    datum it represents. The `get-datum` procedure returns the next
    datum that can be parsed from the given textual-input-port, updating
    textual-input-port to point exactly past the end of the external
    representation of the object.

    Any *interlexeme space* (comment or whitespace, see [Scheme Syntax: Standard and Guile Extensions](#Scheme-Syntax)) in the input is first skipped. If an end of file occurs after
    the interlexeme space, the end-of-file object is returned.

    If a character inconsistent with an external representation is
    encountered in the input, an exception with condition types
    `&lexical` and `&i/o-read` is raised. Also, if the end of
    file is encountered after the beginning of an external representation,
    but the external representation is incomplete and therefore cannot be
    parsed, an exception with condition types `&lexical` and
    `&i/o-read` is raised.

Scheme Procedure: **put-datum** textual-output-port datum [¶](#index-put_002ddatum)
:   datum should be a datum value. The `put-datum` procedure
    writes an external representation of datum to
    textual-output-port. The specific external representation is
    implementation-dependent. However, whenever possible, an implementation
    should produce a representation for which `get-datum`, when reading
    the representation, will return an object equal (in the sense of
    `equal?`) to datum.

    > **Note:** Not all datums may allow producing an external representation for which
    > `get-datum` will produce an object that is equal to the
    > original. Specifically, NaNs contained in datum may make
    > this impossible.

    > **Note:** The `put-datum` procedure merely writes the external
    > representation, but no trailing delimiter. If `put-datum` is
    > used to write several subsequent external representations to an
    > output port, care should be taken to delimit them properly so they can
    > be read back in by subsequent calls to `get-datum`.

Scheme Procedure: **flush-output-port** port [¶](#index-flush_002doutput_002dport)
:   See [Buffering](#Buffering), for documentation on `force-output`.

---

Next: [rnrs io simple](#rnrs-io-simple), Previous: [rnrs io ports](#rnrs-io-ports), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.17 R6RS File Ports [¶](#R6RS-File-Ports-1)

The facilities described in this section are exported by the `(rnrs
io ports)` module.

Scheme Syntax: **buffer-mode** buffer-mode-symbol [¶](#index-buffer_002dmode)
:   buffer-mode-symbol must be a symbol whose name is one of
    `none`, `line`, and `block`. The result is the
    corresponding symbol, and specifies the associated buffer mode.
    See [Buffering](#Buffering), for a discussion of these different buffer modes. To
    control the amount of buffering, use `setvbuf` instead. Note that
    only the name of buffer-mode-symbol is significant.

    See [Buffering](#Buffering), for a discussion of port buffering.

Scheme Procedure: **buffer-mode?** obj [¶](#index-buffer_002dmode_003f)
:   Returns `#t` if the argument is a valid buffer-mode symbol, and
    returns `#f` otherwise.

When opening a file, the various procedures accept a `file-options`
object that encapsulates flags to specify how the file is to be
opened. A `file-options` object is an enum-set (see [rnrs enums](#rnrs-enums))
over the symbols constituting valid file options.

A file-options parameter name means that the corresponding
argument must be a file-options object.

Scheme Syntax: **file-options** file-options-symbol ... [¶](#index-file_002doptions)
:   Each file-options-symbol must be a symbol.

    The `file-options` syntax returns a file-options object that
    encapsulates the specified options.

    When supplied to an operation that opens a file for output, the
    file-options object returned by `(file-options)` specifies that the
    file is created if it does not exist and an exception with condition
    type `&i/o-file-already-exists` is raised if it does exist. The
    following standard options can be included to modify the default
    behavior.

    `no-create`
    :   If the file does not already exist, it is not created;
        instead, an exception with condition type `&i/o-file-does-not-exist`
        is raised.
        If the file already exists, the exception with condition type
        `&i/o-file-already-exists` is not raised
        and the file is truncated to zero length.

    `no-fail`
    :   If the file already exists, the exception with condition type
        `&i/o-file-already-exists` is not raised,
        even if `no-create` is not included,
        and the file is truncated to zero length.

    `no-truncate`
    :   If the file already exists and the exception with condition type
        `&i/o-file-already-exists` has been inhibited by inclusion of
        `no-create` or `no-fail`, the file is not truncated, but
        the port’s current position is still set to the beginning of the
        file.

    These options have no effect when a file is opened only for input.
    Symbols other than those listed above may be used as
    file-options-symbols; they have implementation-specific meaning,
    if any.

    > **Note:** Only the name of file-options-symbol is significant.

Scheme Procedure: **open-file-input-port** filename [¶](#index-open_002dfile_002dinput_002dport)

Scheme Procedure: **open-file-input-port** filename file-options [¶](#index-open_002dfile_002dinput_002dport-1)

Scheme Procedure: **open-file-input-port** filename file-options buffer-mode [¶](#index-open_002dfile_002dinput_002dport-2)

Scheme Procedure: **open-file-input-port** filename file-options buffer-mode maybe-transcoder [¶](#index-open_002dfile_002dinput_002dport-3)
:   maybe-transcoder must be either a transcoder or `#f`.

    The `open-file-input-port` procedure returns an
    input port for the named file. The file-options and
    maybe-transcoder arguments are optional.

    The file-options argument, which may determine various aspects of
    the returned port, defaults to the value of `(file-options)`.

    The buffer-mode argument, if supplied,
    must be one of the symbols that name a buffer mode.
    The buffer-mode argument defaults to `block`.

    If maybe-transcoder is a transcoder, it becomes the transcoder associated
    with the returned port.

    If maybe-transcoder is `#f` or absent,
    the port will be a binary port and will support the
    `port-position` and `set-port-position!` operations.
    Otherwise the port will be a textual port, and whether it supports
    the `port-position` and `set-port-position!` operations
    is implementation-dependent (and possibly transcoder-dependent).

Scheme Procedure: **open-file-output-port** filename [¶](#index-open_002dfile_002doutput_002dport)

Scheme Procedure: **open-file-output-port** filename file-options [¶](#index-open_002dfile_002doutput_002dport-1)

Scheme Procedure: **open-file-output-port** filename file-options buffer-mode [¶](#index-open_002dfile_002doutput_002dport-2)

Scheme Procedure: **open-file-output-port** filename file-options buffer-mode maybe-transcoder [¶](#index-open_002dfile_002doutput_002dport-3)
:   maybe-transcoder must be either a transcoder or `#f`.

    The `open-file-output-port` procedure returns an output port for the named file.

    The file-options argument, which may determine various aspects of
    the returned port, defaults to the value of `(file-options)`.

    The buffer-mode argument, if supplied,
    must be one of the symbols that name a buffer mode.
    The buffer-mode argument defaults to `block`.

    If maybe-transcoder is a transcoder, it becomes the transcoder
    associated with the port.

    If maybe-transcoder is `#f` or absent,
    the port will be a binary port and will support the
    `port-position` and `set-port-position!` operations.
    Otherwise the port will be a textual port, and whether it supports
    the `port-position` and `set-port-position!` operations
    is implementation-dependent (and possibly transcoder-dependent).

---

Next: [rnrs files](#rnrs-files), Previous: [R6RS File Ports](#R6RS-File-Ports), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.18 rnrs io simple [¶](#rnrs-io-simple-1)

The `(rnrs io simple (6))` library provides convenience functions
for performing textual I/O on ports. This library also exports all of
the condition types and associated procedures described in (see [I/O Conditions](#R6RS-I_002fO-Conditions)). In the context of this section, when stating that a
procedure behaves “identically” to the corresponding procedure in
Guile’s core library, this is modulo the behavior wrt. conditions: such
procedures raise the appropriate R6RS conditions in case of error, but
otherwise behave identically.

> **Note:** There are still known issues regarding condition-correctness; some
> errors may still be thrown as native Guile exceptions instead of the
> appropriate R6RS conditions.

Scheme Procedure: **eof-object** [¶](#index-eof_002dobject-1)

Scheme Procedure: **eof-object?** obj [¶](#index-eof_002dobject_003f-2)
:   These procedures are identical to the ones provided by the `(rnrs
    io ports (6))` library. See [rnrs io ports](#rnrs-io-ports), for documentation.

Scheme Procedure: **input-port?** obj [¶](#index-input_002dport_003f-2)

Scheme Procedure: **output-port?** obj [¶](#index-output_002dport_003f-2)
:   These procedures are identical to the ones provided by Guile’s core
    library. See [Ports](#Ports), for documentation.

Scheme Procedure: **call-with-input-file** filename proc [¶](#index-call_002dwith_002dinput_002dfile-1)

Scheme Procedure: **call-with-output-file** filename proc [¶](#index-call_002dwith_002doutput_002dfile-1)

Scheme Procedure: **open-input-file** filename [¶](#index-open_002dinput_002dfile-1)

Scheme Procedure: **open-output-file** filename [¶](#index-open_002doutput_002dfile-1)

Scheme Procedure: **with-input-from-file** filename thunk [¶](#index-with_002dinput_002dfrom_002dfile-1)

Scheme Procedure: **with-output-to-file** filename thunk [¶](#index-with_002doutput_002dto_002dfile-1)
:   These procedures are identical to the ones provided by Guile’s core
    library. See [File Ports](#File-Ports), for documentation.

Scheme Procedure: **close-input-port** input-port [¶](#index-close_002dinput_002dport)

Scheme Procedure: **close-output-port** output-port [¶](#index-close_002doutput_002dport)
:   Closes the given input-port or output-port. These are
    legacy interfaces; just use `close-port`.

Scheme Procedure: **peek-char** [¶](#index-peek_002dchar-1)

Scheme Procedure: **peek-char** textual-input-port [¶](#index-peek_002dchar-2)

Scheme Procedure: **read-char** [¶](#index-read_002dchar-1)

Scheme Procedure: **read-char** textual-input-port [¶](#index-read_002dchar-2)
:   These procedures are identical to the ones provided by Guile’s core
    library. See [Venerable Port Interfaces](#Venerable-Port-Interfaces), for documentation.

Scheme Procedure: **read** [¶](#index-read-2)

Scheme Procedure: **read** textual-input-port [¶](#index-read-3)
:   This procedure is identical to the one provided by Guile’s core library.
    See [Reading Scheme Code](#Scheme-Read), for documentation.

Scheme Procedure: **display** obj [¶](#index-display-1)

Scheme Procedure: **display** obj textual-output-port [¶](#index-display-2)

Scheme Procedure: **newline** [¶](#index-newline-1)

Scheme Procedure: **newline** textual-output-port [¶](#index-newline-2)

Scheme Procedure: **write** obj [¶](#index-write-1)

Scheme Procedure: **write** obj textual-output-port [¶](#index-write-2)

Scheme Procedure: **write-char** char [¶](#index-write_002dchar-1)

Scheme Procedure: **write-char** char textual-output-port [¶](#index-write_002dchar-2)
:   These procedures are identical to the ones provided by Guile’s core
    library. See [Venerable Port Interfaces](#Venerable-Port-Interfaces), and See [Writing Scheme Values](#Scheme-Write), for
    documentation.

---

Next: [rnrs programs](#rnrs-programs), Previous: [rnrs io simple](#rnrs-io-simple), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.19 rnrs files [¶](#rnrs-files-1)

The `(rnrs files (6))` library provides the `file-exists?` and
`delete-file` procedures, which test for the existence of a file
and allow the deletion of files from the file system, respectively.

These procedures are identical to the ones provided by Guile’s core
library. See [File System](#File-System), for documentation.

---

Next: [rnrs arithmetic fixnums](#rnrs-arithmetic-fixnums), Previous: [rnrs files](#rnrs-files), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.20 rnrs programs [¶](#rnrs-programs-1)

The `(rnrs programs (6))` library provides procedures for
process management and introspection.

Scheme Procedure: **command-line** [¶](#index-command_002dline-1)
:   This procedure is identical to the one provided by Guile’s core library.
    See [Runtime Environment](#Runtime-Environment), for documentation.

Scheme Procedure: **exit** [status] [¶](#index-exit-1)
:   This procedure is identical to the one provided by Guile’s core
    library. See [Processes](#Processes), for documentation.

---

Next: [rnrs arithmetic flonums](#rnrs-arithmetic-flonums), Previous: [rnrs programs](#rnrs-programs), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.21 rnrs arithmetic fixnums [¶](#rnrs-arithmetic-fixnums-1)

The `(rnrs arithmetic fixnums (6))` library provides procedures for
performing arithmetic operations on an implementation-dependent range of
exact integer values, which R6RS refers to as *fixnums*. In Guile,
the size of a fixnum is determined by the size of the `SCM` type; a
single SCM struct is guaranteed to be able to hold an entire fixnum,
making fixnum computations particularly
efficient—(see [The SCM Type](#The-SCM-Type)). On 32-bit systems, the most
negative and most positive fixnum values are, respectively, -536870912
and 536870911.

Unless otherwise specified, all of the procedures below take fixnums as
arguments, and will raise an `&assertion` condition if passed a
non-fixnum argument or an `&implementation-restriction` condition
if their result is not itself a fixnum.

Scheme Procedure: **fixnum?** obj [¶](#index-fixnum_003f)
:   Returns `#t` if obj is a fixnum, `#f` otherwise.

Scheme Procedure: **fixnum-width** [¶](#index-fixnum_002dwidth)

Scheme Procedure: **least-fixnum** [¶](#index-least_002dfixnum)

Scheme Procedure: **greatest-fixnum** [¶](#index-greatest_002dfixnum)
:   These procedures return, respectively, the maximum number of bits
    necessary to represent a fixnum value in Guile, the minimum fixnum
    value, and the maximum fixnum value.

Scheme Procedure: **fx=?** fx1 fx2 fx3 ... [¶](#index-fx_003d_003f)

Scheme Procedure: **fx>?** fx1 fx2 fx3 ... [¶](#index-fx_003e_003f)

Scheme Procedure: **fx<?** fx1 fx2 fx3 ... [¶](#index-fx_003c_003f)

Scheme Procedure: **fx>=?** fx1 fx2 fx3 ... [¶](#index-fx_003e_003d_003f)

Scheme Procedure: **fx<=?** fx1 fx2 fx3 ... [¶](#index-fx_003c_003d_003f)
:   These procedures return `#t` if their fixnum arguments are
    (respectively): equal, monotonically increasing, monotonically
    decreasing, monotonically nondecreasing, or monotonically nonincreasing;
    `#f` otherwise.

Scheme Procedure: **fxzero?** fx [¶](#index-fxzero_003f)

Scheme Procedure: **fxpositive?** fx [¶](#index-fxpositive_003f)

Scheme Procedure: **fxnegative?** fx [¶](#index-fxnegative_003f)

Scheme Procedure: **fxodd?** fx [¶](#index-fxodd_003f)

Scheme Procedure: **fxeven?** fx [¶](#index-fxeven_003f)
:   These numerical predicates return `#t` if fx is,
    respectively, zero, greater than zero, less than zero, odd, or even;
    `#f` otherwise.

Scheme Procedure: **fxmax** fx1 fx2 ... [¶](#index-fxmax)

Scheme Procedure: **fxmin** fx1 fx2 ... [¶](#index-fxmin)
:   These procedures return the maximum or minimum of their arguments.

Scheme Procedure: **fx+** fx1 fx2 [¶](#index-fx_002b)

Scheme Procedure: **fx\*** fx1 fx2 [¶](#index-fx_002a)
:   These procedures return the sum or product of their arguments.

Scheme Procedure: **fx-** fx1 fx2 [¶](#index-fx_002d)

Scheme Procedure: **fx-** fx [¶](#index-fx_002d-1)
:   Returns the difference of fx1 and fx2, or the negation of
    fx, if called with a single argument.

    An `&assertion` condition is raised if the result is not itself a
    fixnum.

Scheme Procedure: **fxdiv-and-mod** fx1 fx2 [¶](#index-fxdiv_002dand_002dmod)

Scheme Procedure: **fxdiv** fx1 fx2 [¶](#index-fxdiv)

Scheme Procedure: **fxmod** fx1 fx2 [¶](#index-fxmod)

Scheme Procedure: **fxdiv0-and-mod0** fx1 fx2 [¶](#index-fxdiv0_002dand_002dmod0)

Scheme Procedure: **fxdiv0** fx1 fx2 [¶](#index-fxdiv0)

Scheme Procedure: **fxmod0** fx1 fx2 [¶](#index-fxmod0)
:   These procedures implement number-theoretic division on fixnums;
    See [(rnrs base)](rnrs%20base.html#Top), for a description of their semantics.

Scheme Procedure: **fx+/carry** fx1 fx2 fx3 [¶](#index-fx_002b_002fcarry)
:   Returns the two fixnum results of the following computation:

    ```
    (let* ((s (+ fx1 fx2 fx3))
           (s0 (mod0 s (expt 2 (fixnum-width))))
           (s1 (div0 s (expt 2 (fixnum-width)))))
      (values s0 s1))
    ```

Scheme Procedure: **fx-/carry** fx1 fx2 fx3 [¶](#index-fx_002d_002fcarry)
:   Returns the two fixnum results of the following computation:

    ```
    (let* ((d (- fx1 fx2 fx3))
           (d0 (mod0 d (expt 2 (fixnum-width))))
           (d1 (div0 d (expt 2 (fixnum-width)))))
      (values d0 d1))
    ```

Scheme Procedure: **fx\*/carry** fx1 fx2 fx3 [¶](#index-fx_002a_002fcarry)
:   ```
    Returns the two fixnum results of the following computation:
    (let* ((s (+ (* fx1 fx2) fx3))
           (s0 (mod0 s (expt 2 (fixnum-width))))
           (s1 (div0 s (expt 2 (fixnum-width)))))
      (values s0 s1))
    ```

Scheme Procedure: **fxnot** fx [¶](#index-fxnot)

Scheme Procedure: **fxand** fx1 ... [¶](#index-fxand)

Scheme Procedure: **fxior** fx1 ... [¶](#index-fxior)

Scheme Procedure: **fxxor** fx1 ... [¶](#index-fxxor)
:   These procedures are identical to the `lognot`, `logand`,
    `logior`, and `logxor` procedures provided by Guile’s core
    library. See [Bitwise Operations](#Bitwise-Operations), for documentation.

Scheme Procedure: **fxif** fx1 fx2 fx3 [¶](#index-fxif)
:   Returns the bitwise “if” of its fixnum arguments. The bit at position
    `i` in the return value will be the `i`th bit from fx2
    if the `i`th bit of fx1 is 1, the `i`th bit from
    fx3.

Scheme Procedure: **fxbit-count** fx [¶](#index-fxbit_002dcount)
:   Returns the number of 1 bits in the two’s complement representation of
    fx.

Scheme Procedure: **fxlength** fx [¶](#index-fxlength)
:   Returns the number of bits necessary to represent fx.

Scheme Procedure: **fxfirst-bit-set** fx [¶](#index-fxfirst_002dbit_002dset)
:   Returns the index of the least significant 1 bit in the two’s complement
    representation of fx.

Scheme Procedure: **fxbit-set?** fx1 fx2 [¶](#index-fxbit_002dset_003f)
:   Returns `#t` if the fx2th bit in the two’s complement
    representation of fx1 is 1, `#f` otherwise.

Scheme Procedure: **fxcopy-bit** fx1 fx2 fx3 [¶](#index-fxcopy_002dbit)
:   Returns the result of setting the fx2th bit of fx1 to the
    fx2th bit of fx3.

Scheme Procedure: **fxbit-field** fx1 fx2 fx3 [¶](#index-fxbit_002dfield)
:   Returns the integer representation of the contiguous sequence of bits in
    fx1 that starts at position fx2 (inclusive) and ends at
    position fx3 (exclusive).

Scheme Procedure: **fxcopy-bit-field** fx1 fx2 fx3 fx4 [¶](#index-fxcopy_002dbit_002dfield)
:   Returns the result of replacing the bit field in fx1 with start
    and end positions fx2 and fx3 with the corresponding bit
    field from fx4.

Scheme Procedure: **fxarithmetic-shift** fx1 fx2 [¶](#index-fxarithmetic_002dshift)

Scheme Procedure: **fxarithmetic-shift-left** fx1 fx2 [¶](#index-fxarithmetic_002dshift_002dleft)

Scheme Procedure: **fxarithmetic-shift-right** fx1 fx2 [¶](#index-fxarithmetic_002dshift_002dright)
:   Returns the result of shifting the bits of fx1 right or left by
    the fx2 positions. `fxarithmetic-shift` is identical
    to `fxarithmetic-shift-left`.

Scheme Procedure: **fxrotate-bit-field** fx1 fx2 fx3 fx4 [¶](#index-fxrotate_002dbit_002dfield)
:   Returns the result of cyclically permuting the bit field in fx1
    with start and end positions fx2 and fx3 by fx4 bits
    in the direction of more significant bits.

Scheme Procedure: **fxreverse-bit-field** fx1 fx2 fx3 [¶](#index-fxreverse_002dbit_002dfield)
:   Returns the result of reversing the order of the bits of fx1
    between position fx2 (inclusive) and position fx3
    (exclusive).

---

Next: [rnrs arithmetic bitwise](#rnrs-arithmetic-bitwise), Previous: [rnrs arithmetic fixnums](#rnrs-arithmetic-fixnums), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.22 rnrs arithmetic flonums [¶](#rnrs-arithmetic-flonums-1)

The `(rnrs arithmetic flonums (6))` library provides procedures for
performing arithmetic operations on inexact representations of real
numbers, which R6RS refers to as *flonums*.

Unless otherwise specified, all of the procedures below take flonums as
arguments, and will raise an `&assertion` condition if passed a
non-flonum argument.

Scheme Procedure: **flonum?** obj [¶](#index-flonum_003f)
:   Returns `#t` if obj is a flonum, `#f` otherwise.

Scheme Procedure: **real->flonum** x [¶](#index-real_002d_003eflonum)
:   Returns the flonum that is numerically closest to the real number
    x.

Scheme Procedure: **fl=?** fl1 fl2 fl3 ... [¶](#index-fl_003d_003f)

Scheme Procedure: **fl<?** fl1 fl2 fl3 ... [¶](#index-fl_003c_003f)

Scheme Procedure: **fl<=?** fl1 fl2 fl3 ... [¶](#index-fl_003c_003d_003f)

Scheme Procedure: **fl>?** fl1 fl2 fl3 ... [¶](#index-fl_003e_003f)

Scheme Procedure: **fl>=?** fl1 fl2 fl3 ... [¶](#index-fl_003e_003d_003f)
:   These procedures return `#t` if their flonum arguments are
    (respectively): equal, monotonically increasing, monotonically
    decreasing, monotonically nondecreasing, or monotonically nonincreasing;
    `#f` otherwise.

Scheme Procedure: **flinteger?** fl [¶](#index-flinteger_003f)

Scheme Procedure: **flzero?** fl [¶](#index-flzero_003f)

Scheme Procedure: **flpositive?** fl [¶](#index-flpositive_003f)

Scheme Procedure: **flnegative?** fl [¶](#index-flnegative_003f)

Scheme Procedure: **flodd?** fl [¶](#index-flodd_003f)

Scheme Procedure: **fleven?** fl [¶](#index-fleven_003f)
:   These numerical predicates return `#t` if fl is,
    respectively, an integer, zero, greater than zero, less than zero, odd,
    even, `#f` otherwise. In the case of `flodd?` and
    `fleven?`, fl must be an integer-valued flonum.

Scheme Procedure: **flfinite?** fl [¶](#index-flfinite_003f)

Scheme Procedure: **flinfinite?** fl [¶](#index-flinfinite_003f)

Scheme Procedure: **flnan?** fl [¶](#index-flnan_003f)
:   These numerical predicates return `#t` if fl is,
    respectively, not infinite, infinite, or a `NaN` value.

Scheme Procedure: **flmax** fl1 fl2 ... [¶](#index-flmax)

Scheme Procedure: **flmin** fl1 fl2 ... [¶](#index-flmin)
:   These procedures return the maximum or minimum of their arguments.

Scheme Procedure: **fl+** fl1 ... [¶](#index-fl_002b)

Scheme Procedure: **fl\*** fl ... [¶](#index-fl_002a)
:   These procedures return the sum or product of their arguments.

Scheme Procedure: **fl-** fl1 fl2 ... [¶](#index-fl_002d)

Scheme Procedure: **fl-** fl [¶](#index-fl_002d-1)

Scheme Procedure: **fl/** fl1 fl2 ... [¶](#index-fl_002f)

Scheme Procedure: **fl/** fl [¶](#index-fl_002f-1)
:   These procedures return, respectively, the difference or quotient of
    their arguments when called with two arguments; when called with a
    single argument, they return the additive or multiplicative inverse of
    fl.

Scheme Procedure: **flabs** fl [¶](#index-flabs)
:   Returns the absolute value of fl.

Scheme Procedure: **fldiv-and-mod** fl1 fl2 [¶](#index-fldiv_002dand_002dmod)

Scheme Procedure: **fldiv** fl1 fl2 [¶](#index-fldiv)

Scheme Procedure: **fldmod** fl1 fl2 [¶](#index-fldmod)

Scheme Procedure: **fldiv0-and-mod0** fl1 fl2 [¶](#index-fldiv0_002dand_002dmod0)

Scheme Procedure: **fldiv0** fl1 fl2 [¶](#index-fldiv0)

Scheme Procedure: **flmod0** fl1 fl2 [¶](#index-flmod0)
:   These procedures implement number-theoretic division on flonums;
    See [(rnrs base)](rnrs%20base.html#Top), for a description for their semantics.

Scheme Procedure: **flnumerator** fl [¶](#index-flnumerator)

Scheme Procedure: **fldenominator** fl [¶](#index-fldenominator)
:   These procedures return the numerator or denominator of fl as a
    flonum.

Scheme Procedure: **flfloor** fl1 [¶](#index-flfloor)

Scheme Procedure: **flceiling** fl [¶](#index-flceiling)

Scheme Procedure: **fltruncate** fl [¶](#index-fltruncate)

Scheme Procedure: **flround** fl [¶](#index-flround)
:   These procedures are identical to the `floor`, `ceiling`,
    `truncate`, and `round` procedures provided by Guile’s core
    library. See [Arithmetic Functions](#Arithmetic), for documentation.

Scheme Procedure: **flexp** fl [¶](#index-flexp)

Scheme Procedure: **fllog** fl [¶](#index-fllog)

Scheme Procedure: **fllog** fl1 fl2 [¶](#index-fllog-1)

Scheme Procedure: **flsin** fl [¶](#index-flsin)

Scheme Procedure: **flcos** fl [¶](#index-flcos)

Scheme Procedure: **fltan** fl [¶](#index-fltan)

Scheme Procedure: **flasin** fl [¶](#index-flasin)

Scheme Procedure: **flacos** fl [¶](#index-flacos)

Scheme Procedure: **flatan** fl [¶](#index-flatan)

Scheme Procedure: **flatan** fl1 fl2 [¶](#index-flatan-1)
:   These procedures, which compute the usual transcendental functions, are
    the flonum variants of the procedures provided by the R6RS base library
    (see [(rnrs base)](rnrs%20base.html#Top)).

Scheme Procedure: **flsqrt** fl [¶](#index-flsqrt)
:   Returns the square root of fl. If fl is `-0.0`,
    -0.0 is returned; for other negative values, a `NaN` value
    is returned.

Scheme Procedure: **flexpt** fl1 fl2 [¶](#index-flexpt)
:   Returns the value of fl1 raised to the power of fl2.

The following condition types are provided to allow Scheme
implementations that do not support infinities or `NaN` values
to indicate that a computation resulted in such a value. Guile supports
both of these, so these conditions will never be raised by Guile’s
standard libraries implementation.

Condition Type: **&no-infinities** [¶](#index-_0026no_002dinfinities)

Scheme Procedure: **make-no-infinities-violation** obj [¶](#index-make_002dno_002dinfinities_002dviolation)

Scheme Procedure: **no-infinities-violation?** [¶](#index-no_002dinfinities_002dviolation_003f)
:   A condition type indicating that a computation resulted in an infinite
    value on a Scheme implementation incapable of representing infinities.

Condition Type: **&no-nans** [¶](#index-_0026no_002dnans)

Scheme Procedure: **make-no-nans-violation** obj [¶](#index-make_002dno_002dnans_002dviolation)

Scheme Procedure: **no-nans-violation?** obj [¶](#index-no_002dnans_002dviolation_003f)
:   A condition type indicating that a computation resulted in a `NaN`
    value on a Scheme implementation incapable of representing `NaN`s.

Scheme Procedure: **fixnum->flonum** fx [¶](#index-fixnum_002d_003eflonum)
:   Returns the flonum that is numerically closest to the fixnum fx.

---

Next: [rnrs syntax-case](#rnrs-syntax_002dcase), Previous: [rnrs arithmetic flonums](#rnrs-arithmetic-flonums), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.23 rnrs arithmetic bitwise [¶](#rnrs-arithmetic-bitwise-1)

The `(rnrs arithmetic bitwise (6))` library provides procedures for
performing bitwise arithmetic operations on the two’s complement
representations of fixnums.

This library and the procedures it exports share functionality with
SRFI-60, which provides support for bitwise manipulation of integers
(see [SRFI-60 - Integers as Bits](#SRFI_002d60)).

Scheme Procedure: **bitwise-not** ei [¶](#index-bitwise_002dnot-1)

Scheme Procedure: **bitwise-and** ei1 ... [¶](#index-bitwise_002dand-1)

Scheme Procedure: **bitwise-ior** ei1 ... [¶](#index-bitwise_002dior-1)

Scheme Procedure: **bitwise-xor** ei1 ... [¶](#index-bitwise_002dxor-1)
:   These procedures are identical to the `lognot`, `logand`,
    `logior`, and `logxor` procedures provided by Guile’s core
    library. See [Bitwise Operations](#Bitwise-Operations), for documentation.

Scheme Procedure: **bitwise-if** ei1 ei2 ei3 [¶](#index-bitwise_002dif-1)
:   Returns the bitwise “if” of its arguments. The bit at position
    `i` in the return value will be the `i`th bit from ei2
    if the `i`th bit of ei1 is 1, the `i`th bit from
    ei3.

Scheme Procedure: **bitwise-bit-count** ei [¶](#index-bitwise_002dbit_002dcount)
:   Returns the number of 1 bits in the two’s complement representation of
    ei.

Scheme Procedure: **bitwise-length** ei [¶](#index-bitwise_002dlength)
:   Returns the number of bits necessary to represent ei.

Scheme Procedure: **bitwise-first-bit-set** ei [¶](#index-bitwise_002dfirst_002dbit_002dset)
:   Returns the index of the least significant 1 bit in the two’s complement
    representation of ei.

Scheme Procedure: **bitwise-bit-set?** ei1 ei2 [¶](#index-bitwise_002dbit_002dset_003f)
:   Returns `#t` if the ei2th bit in the two’s complement
    representation of ei1 is 1, `#f` otherwise.

Scheme Procedure: **bitwise-copy-bit** ei1 ei2 ei3 [¶](#index-bitwise_002dcopy_002dbit)
:   Returns the result of setting the ei2th bit of ei1 to the
    ei2th bit of ei3.

Scheme Procedure: **bitwise-bit-field** ei1 ei2 ei3 [¶](#index-bitwise_002dbit_002dfield)
:   Returns the integer representation of the contiguous sequence of bits in
    ei1 that starts at position ei2 (inclusive) and ends at
    position ei3 (exclusive).

Scheme Procedure: **bitwise-copy-bit-field** ei1 ei2 ei3 ei4 [¶](#index-bitwise_002dcopy_002dbit_002dfield)
:   Returns the result of replacing the bit field in ei1 with start
    and end positions ei2 and ei3 with the corresponding bit
    field from ei4.

Scheme Procedure: **bitwise-arithmetic-shift** ei1 ei2 [¶](#index-bitwise_002darithmetic_002dshift)

Scheme Procedure: **bitwise-arithmetic-shift-left** ei1 ei2 [¶](#index-bitwise_002darithmetic_002dshift_002dleft)

Scheme Procedure: **bitwise-arithmetic-shift-right** ei1 ei2 [¶](#index-bitwise_002darithmetic_002dshift_002dright)
:   Returns the result of shifting the bits of ei1 right or left by
    the ei2 positions. `bitwise-arithmetic-shift` is identical
    to `bitwise-arithmetic-shift-left`.

Scheme Procedure: **bitwise-rotate-bit-field** ei1 ei2 ei3 ei4 [¶](#index-bitwise_002drotate_002dbit_002dfield)
:   Returns the result of cyclically permuting the bit field in ei1
    with start and end positions ei2 and ei3 by ei4 bits
    in the direction of more significant bits.

Scheme Procedure: **bitwise-reverse-bit-field** ei1 ei2 ei3 [¶](#index-bitwise_002dreverse_002dbit_002dfield)
:   Returns the result of reversing the order of the bits of ei1
    between position ei2 (inclusive) and position ei3
    (exclusive).

---

Next: [rnrs hashtables](#rnrs-hashtables), Previous: [rnrs arithmetic bitwise](#rnrs-arithmetic-bitwise), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.24 rnrs syntax-case [¶](#rnrs-syntax_002dcase-1)

The `(rnrs syntax-case (6))` library provides access to the
`syntax-case` system for writing hygienic macros. With one
exception, all of the forms and procedures exported by this library
are “re-exports” of Guile’s native support for `syntax-case`;
See [Support for the `syntax-case` System](#Syntax-Case), for documentation, examples, and rationale.

Scheme Procedure: **make-variable-transformer** proc [¶](#index-make_002dvariable_002dtransformer-1)
:   Creates a new variable transformer out of proc, a procedure that
    takes a syntax object as input and returns a syntax object. If an
    identifier to which the result of this procedure is bound appears on the
    left-hand side of a `set!` expression, proc will be called
    with a syntax object representing the entire `set!` expression,
    and its return value will replace that `set!` expression.

Scheme Syntax: **syntax-case** expression (literal ...) clause ... [¶](#index-syntax_002dcase-1)
:   The `syntax-case` pattern matching form.

Scheme Syntax: **syntax** template [¶](#index-syntax-1)

Scheme Syntax: **quasisyntax** template [¶](#index-quasisyntax)

Scheme Syntax: **unsyntax** template [¶](#index-unsyntax)

Scheme Syntax: **unsyntax-splicing** template [¶](#index-unsyntax_002dsplicing)
:   These forms allow references to be made in the body of a syntax-case
    output expression subform to datum and non-datum values. They are
    identical to the forms provided by Guile’s core library;
    See [Support for the `syntax-case` System](#Syntax-Case), for documentation.

Scheme Procedure: **identifier?** obj [¶](#index-identifier_003f-1)

Scheme Procedure: **bound-identifier=?** id1 id2 [¶](#index-bound_002didentifier_003d_003f-1)

Scheme Procedure: **free-identifier=?** id1 id2 [¶](#index-free_002didentifier_003d_003f-1)
:   These predicate procedures operate on syntax objects representing
    Scheme identifiers. `identifier?` returns `#t` if obj
    represents an identifier, `#f` otherwise.
    `bound-identifier=?` returns `#t` if and only if a binding for
    id1 would capture a reference to id2 in the transformer’s
    output, or vice-versa. `free-identifier=?` returns `#t` if
    and only id1 and id2 would refer to the same binding in the
    output of the transformer, independent of any bindings introduced by the
    transformer.

Scheme Procedure: **generate-temporaries** l [¶](#index-generate_002dtemporaries-1)
:   Returns a list, of the same length as l, which must be a list or
    a syntax object representing a list, of globally unique symbols.

Scheme Procedure: **syntax->datum** syntax-object [¶](#index-syntax_002d_003edatum-1)

Scheme Procedure: **datum->syntax** template-id datum [¶](#index-datum_002d_003esyntax-1)
:   These procedures convert wrapped syntax objects to and from Scheme datum
    values. The syntax object returned by `datum->syntax` shares
    contextual information with the syntax object template-id.

Scheme Procedure: **syntax-violation** whom message form [¶](#index-syntax_002dviolation)

Scheme Procedure: **syntax-violation** whom message form subform [¶](#index-syntax_002dviolation-1)
:   Constructs a new compound condition that includes the following
    simple conditions:

    * If whom is not `#f`, a `&who` condition with the
      whom as its field
    * A `&message` condition with the specified message
    * A `&syntax` condition with the specified form and optional
      subform fields

---

Next: [rnrs enums](#rnrs-enums), Previous: [rnrs syntax-case](#rnrs-syntax_002dcase), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.25 rnrs hashtables [¶](#rnrs-hashtables-1)

The `(rnrs hashtables (6))` library provides structures and
procedures for creating and accessing hash tables. The hash tables API
defined by R6RS is substantially similar to both Guile’s native hash
tables implementation as well as the one provided by SRFI-69;
See [Hash Tables](#Hash-Tables), and [SRFI-69 - Basic hash tables](#SRFI_002d69), respectively. Note that you can
write portable R6RS library code that manipulates SRFI-69 hash tables
(by importing the `(srfi :69)` library); however, hash tables
created by one API cannot be used by another.

Like SRFI-69 hash tables—and unlike Guile’s native ones—R6RS hash
tables associate hash and equality functions with a hash table at the
time of its creation. Additionally, R6RS allows for the creation
(via `hashtable-copy`; see below) of immutable hash tables.

Scheme Procedure: **make-eq-hashtable** [¶](#index-make_002deq_002dhashtable)

Scheme Procedure: **make-eq-hashtable** k [¶](#index-make_002deq_002dhashtable-1)
:   Returns a new hash table that uses `eq?` to compare keys and
    Guile’s `hashq` procedure as a hash function. If k is given,
    it specifies the initial capacity of the hash table.

Scheme Procedure: **make-eqv-hashtable** [¶](#index-make_002deqv_002dhashtable)

Scheme Procedure: **make-eqv-hashtable** k [¶](#index-make_002deqv_002dhashtable-1)
:   Returns a new hash table that uses `eqv?` to compare keys and
    Guile’s `hashv` procedure as a hash function. If k is given,
    it specifies the initial capacity of the hash table.

Scheme Procedure: **make-hashtable** hash-function equiv [¶](#index-make_002dhashtable)

Scheme Procedure: **make-hashtable** hash-function equiv k [¶](#index-make_002dhashtable-1)
:   Returns a new hash table that uses equiv to compare keys and
    hash-function as a hash function. equiv must be a procedure
    that accepts two arguments and returns a true value if they are
    equivalent, `#f` otherwise; hash-function must be a procedure
    that accepts one argument and returns a non-negative integer.

    If k is given, it specifies the initial capacity of the hash
    table.

Scheme Procedure: **hashtable?** obj [¶](#index-hashtable_003f)
:   Returns `#t` if obj is an R6RS hash table, `#f`
    otherwise.

Scheme Procedure: **hashtable-size** hashtable [¶](#index-hashtable_002dsize)
:   Returns the number of keys currently in the hash table hashtable.

Scheme Procedure: **hashtable-ref** hashtable key default [¶](#index-hashtable_002dref)
:   Returns the value associated with key in the hash table
    hashtable, or default if none is found.

Scheme Procedure: **hashtable-set!** hashtable key obj [¶](#index-hashtable_002dset_0021)
:   Associates the key key with the value obj in the hash table
    hashtable, and returns an unspecified value. An `&assertion`
    condition is raised if hashtable is immutable.

Scheme Procedure: **hashtable-delete!** hashtable key [¶](#index-hashtable_002ddelete_0021)
:   Removes any association found for the key key in the hash table
    hashtable, and returns an unspecified value. An `&assertion`
    condition is raised if hashtable is immutable.

Scheme Procedure: **hashtable-contains?** hashtable key [¶](#index-hashtable_002dcontains_003f)
:   Returns `#t` if the hash table hashtable contains an
    association for the key key, `#f` otherwise.

Scheme Procedure: **hashtable-update!** hashtable key proc default [¶](#index-hashtable_002dupdate_0021)
:   Associates with key in the hash table hashtable the result
    of calling proc, which must be a procedure that takes one
    argument, on the value currently associated key in
    hashtable—or on default if no such association exists.
    An `&assertion` condition is raised if hashtable is
    immutable.

Scheme Procedure: **hashtable-copy** hashtable [¶](#index-hashtable_002dcopy)

Scheme Procedure: **hashtable-copy** hashtable mutable [¶](#index-hashtable_002dcopy-1)
:   Returns a copy of the hash table hashtable. If the optional
    argument mutable is provided and is a true value, the new hash
    table will be mutable.

Scheme Procedure: **hashtable-clear!** hashtable [¶](#index-hashtable_002dclear_0021)

Scheme Procedure: **hashtable-clear!** hashtable k [¶](#index-hashtable_002dclear_0021-1)
:   Removes all of the associations from the hash table hashtable.
    The optional argument k, which specifies a new capacity for the
    hash table, is accepted by Guile’s `(rnrs hashtables)`
    implementation, but is ignored.

Scheme Procedure: **hashtable-keys** hashtable [¶](#index-hashtable_002dkeys)
:   Returns a vector of the keys with associations in the hash table
    hashtable, in an unspecified order.

Scheme Procedure: **hashtable-entries** hashtable [¶](#index-hashtable_002dentries)
:   Return two values—a vector of the keys with associations in the hash
    table hashtable, and a vector of the values to which these keys
    are mapped, in corresponding but unspecified order.

Scheme Procedure: **hashtable-equivalence-function** hashtable [¶](#index-hashtable_002dequivalence_002dfunction)
:   Returns the equivalence predicated use by hashtable. This
    procedure returns `eq?` and `eqv?`, respectively, for hash
    tables created by `make-eq-hashtable` and
    `make-eqv-hashtable`.

Scheme Procedure: **hashtable-hash-function** hashtable [¶](#index-hashtable_002dhash_002dfunction)
:   Returns the hash function used by hashtable. For hash tables
    created by `make-eq-hashtable` or `make-eqv-hashtable`,
    `#f` is returned.

Scheme Procedure: **hashtable-mutable?** hashtable [¶](#index-hashtable_002dmutable_003f)
:   Returns `#t` if hashtable is mutable, `#f` otherwise.

A number of hash functions are provided for convenience:

Scheme Procedure: **equal-hash** obj [¶](#index-equal_002dhash)
:   Returns an integer hash value for obj, based on its structure and
    current contents. This hash function is suitable for use with
    `equal?` as an equivalence function.

Scheme Procedure: **string-hash** string [¶](#index-string_002dhash-2)

Scheme Procedure: **symbol-hash** symbol [¶](#index-symbol_002dhash-1)
:   These procedures are identical to the ones provided by Guile’s core
    library. See [Hash Table Reference](#Hash-Table-Reference), for documentation.

Scheme Procedure: **string-ci-hash** string [¶](#index-string_002dci_002dhash-1)
:   Returns an integer hash value for string based on its contents,
    ignoring case. This hash function is suitable for use with
    `string-ci=?` as an equivalence function.

---

Next: [rnrs](#rnrs), Previous: [rnrs hashtables](#rnrs-hashtables), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.26 rnrs enums [¶](#rnrs-enums-1)

The `(rnrs enums (6))` library provides structures and procedures
for working with enumerable sets of symbols. Guile’s implementation
defines an *enum-set* record type that encapsulates a finite set of
distinct symbols, the *universe*, and a subset of these symbols,
which define the enumeration set.

The SRFI-1 list library provides a number of procedures for performing
set operations on lists; Guile’s `(rnrs enums)` implementation
makes use of several of them. See [Set Operations on Lists](#SRFI_002d1-Set-Operations), for
more information.

Scheme Procedure: **make-enumeration** symbol-list [¶](#index-make_002denumeration)
:   Returns a new enum-set whose universe and enumeration set are both equal
    to symbol-list, a list of symbols.

Scheme Procedure: **enum-set-universe** enum-set [¶](#index-enum_002dset_002duniverse)
:   Returns an enum-set representing the universe of enum-set,
    an enum-set.

Scheme Procedure: **enum-set-indexer** enum-set [¶](#index-enum_002dset_002dindexer)
:   Returns a procedure that takes a single argument and returns the
    zero-indexed position of that argument in the universe of
    enum-set, or `#f` if its argument is not a member of that
    universe.

Scheme Procedure: **enum-set-constructor** enum-set [¶](#index-enum_002dset_002dconstructor)
:   Returns a procedure that takes a single argument, a list of symbols
    from the universe of enum-set, an enum-set, and returns a new
    enum-set with the same universe that represents a subset containing the
    specified symbols.

Scheme Procedure: **enum-set->list** enum-set [¶](#index-enum_002dset_002d_003elist)
:   Returns a list containing the symbols of the set represented by
    enum-set, an enum-set, in the order that they appear in the
    universe of enum-set.

Scheme Procedure: **enum-set-member?** symbol enum-set [¶](#index-enum_002dset_002dmember_003f)

Scheme Procedure: **enum-set-subset?** enum-set1 enum-set2 [¶](#index-enum_002dset_002dsubset_003f)

Scheme Procedure: **enum-set=?** enum-set1 enum-set2 [¶](#index-enum_002dset_003d_003f)
:   These procedures test for membership of symbols and enum-sets in other
    enum-sets. `enum-set-member?` returns `#t` if and only if
    symbol is a member of the subset specified by enum-set.
    `enum-set-subset?` returns `#t` if and only if the universe of
    enum-set1 is a subset of the universe of enum-set2 and
    every symbol in enum-set1 is present in enum-set2.
    `enum-set=?` returns `#t` if and only if enum-set1 is a
    subset, as per `enum-set-subset?` of enum-set2 and vice
    versa.

Scheme Procedure: **enum-set-union** enum-set1 enum-set2 [¶](#index-enum_002dset_002dunion)

Scheme Procedure: **enum-set-intersection** enum-set1 enum-set2 [¶](#index-enum_002dset_002dintersection)

Scheme Procedure: **enum-set-difference** enum-set1 enum-set2 [¶](#index-enum_002dset_002ddifference)
:   These procedures return, respectively, the union, intersection, and
    difference of their enum-set arguments.

Scheme Procedure: **enum-set-complement** enum-set [¶](#index-enum_002dset_002dcomplement)
:   Returns enum-set’s complement (an enum-set), with regard to its
    universe.

Scheme Procedure: **enum-set-projection** enum-set1 enum-set2 [¶](#index-enum_002dset_002dprojection)
:   Returns the projection of the enum-set enum-set1 onto the universe
    of the enum-set enum-set2.

Scheme Syntax: **define-enumeration** type-name (symbol ...) constructor-syntax [¶](#index-define_002denumeration)
:   Evaluates to two new definitions: A constructor bound to
    constructor-syntax that behaves similarly to constructors created
    by `enum-set-constructor`, above, and creates new enum-sets
    in the universe specified by `(symbol ...)`; and a “predicate
    macro” bound to type-name, which has the following form:

    ```
    (type-name sym)
    ```

    If sym is a member of the universe specified by the symbols
    above, this form evaluates to sym. Otherwise, a `&syntax`
    condition is raised.

---

Next: [rnrs eval](#rnrs-eval), Previous: [rnrs enums](#rnrs-enums), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.27 rnrs [¶](#rnrs-1)

The `(rnrs (6))` library is a composite of all of the other R6RS
standard libraries—it imports and re-exports all of their exported
procedures and syntactic forms—with the exception of the following
libraries:

* `(rnrs eval (6))`
* `(rnrs mutable-pairs (6))`
* `(rnrs mutable-strings (6))`
* `(rnrs r5rs (6))`

---

Next: [rnrs mutable-pairs](#rnrs-mutable_002dpairs), Previous: [rnrs](#rnrs), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.28 rnrs eval [¶](#rnrs-eval-1)

The `(rnrs eval (6)` library provides procedures for performing
“on-the-fly” evaluation of expressions.

Scheme Procedure: **eval** expression environment [¶](#index-eval-1)
:   Evaluates expression, which must be a datum representation of a
    valid Scheme expression, in the environment specified by
    environment. This procedure is identical to the one provided by
    Guile’s code library; See [Procedures for On the Fly Evaluation](#Fly-Evaluation), for documentation.

Scheme Procedure: **environment** import-spec ... [¶](#index-environment-3)
:   Constructs and returns a new environment based on the specified
    import-specs, which must be datum representations of the import
    specifications used with the `import` form. See [R6RS Libraries](#R6RS-Libraries),
    for documentation.

---

Next: [rnrs mutable-strings](#rnrs-mutable_002dstrings), Previous: [rnrs eval](#rnrs-eval), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.29 rnrs mutable-pairs [¶](#rnrs-mutable_002dpairs-1)

The `(rnrs mutable-pairs (6))` library provides the `set-car!`
and `set-cdr!` procedures, which allow the `car` and
`cdr` fields of a pair to be modified.

These procedures are identical to the ones provide by Guile’s core
library. See [Pairs](#Pairs), for documentation. All pairs in Guile are
mutable; consequently, these procedures will never throw the
`&assertion` condition described in the R6RS libraries
specification.

---

Next: [rnrs r5rs](#rnrs-r5rs), Previous: [rnrs mutable-pairs](#rnrs-mutable_002dpairs), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.30 rnrs mutable-strings [¶](#rnrs-mutable_002dstrings-1)

The `(rnrs mutable-strings (6))` library provides the
`string-set!` and `string-fill!` procedures, which allow the
content of strings to be modified “in-place.”

These procedures are identical to the ones provided by Guile’s core
library. See [String Modification](#String-Modification), for documentation. All strings in
Guile are mutable; consequently, these procedures will never throw the
`&assertion` condition described in the R6RS libraries
specification.

---

Previous: [rnrs mutable-strings](#rnrs-mutable_002dstrings), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.31 rnrs r5rs [¶](#rnrs-r5rs-1)

The `(rnrs r5rs (6))` library exports bindings for some procedures
present in R5RS but omitted from the R6RS base library specification.

Scheme Procedure: **exact->inexact** z [¶](#index-exact_002d_003einexact-1)

Scheme Procedure: **inexact->exact** z [¶](#index-inexact_002d_003eexact-1)
:   These procedures are identical to the ones provided by Guile’s core
    library. See [Exact and Inexact Numbers](#Exactness), for documentation.

Scheme Procedure: **quotient** n1 n2 [¶](#index-quotient-1)

Scheme Procedure: **remainder** n1 n2 [¶](#index-remainder-1)

Scheme Procedure: **modulo** n1 n2 [¶](#index-modulo-1)
:   These procedures are identical to the ones provided by Guile’s core
    library. See [Operations on Integer Values](#Integer-Operations), for documentation.

Scheme Syntax: **delay** expr [¶](#index-delay-2)

Scheme Procedure: **force** promise [¶](#index-force-2)
:   The `delay` form and the `force` procedure are identical to
    their counterparts in Guile’s core library. See [Delayed Evaluation](#Delayed-Evaluation),
    for documentation.

Scheme Procedure: **null-environment** n [¶](#index-null_002denvironment-1)

Scheme Procedure: **scheme-report-environment** n [¶](#index-scheme_002dreport_002denvironment-1)
:   These procedures are identical to the ones provided by the
    `(ice-9 r5rs)` Guile module. See [Environments](#Environments), for
    documentation.

---

Next: [Pattern Matching](#Pattern-Matching), Previous: [R6RS Support](#R6RS-Support), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 7.7 R7RS Support [¶](#R7RS-Support-1)

The [R7RS](https://small.r7rs.org/) standard is essentially R5RS
(directly supported by Guile), plus a module facility, plus an
organization of bindings into a standard set of modules.

Happily, the syntax for R7RS modules was chosen to be compatible with
R6RS, and so Guile’s documentation there applies. See [R6RS Libraries](#R6RS-Libraries), for more information on how to define R6RS libraries, and
their integration with Guile modules. See [Library Usage](#Library-Usage), also.

* [Incompatibilities with the R7RS](#R7RS-Incompatibilities)
* [R7RS Standard Libraries](#R7RS-Standard-Libraries)

---

Next: [R7RS Standard Libraries](#R7RS-Standard-Libraries), Up: [R7RS Support](#R7RS-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.7.1 Incompatibilities with the R7RS [¶](#Incompatibilities-with-the-R7RS)

As the R7RS is a much less ambitious standard than the R6RS
(see [Guile and Scheme](#Guile-and-Scheme)), it is very easy for Guile to support. As
such, Guile is a fully conforming implementation of R7RS, with the
exception of the occasional bug and a couple of unimplemented features:

* The R7RS specifies a syntax for reading circular data structures using
  *datum labels*, such as `#0=(1 2 3 . #0#)`. Guile’s reader
  does not support this syntax currently;
  <https://bugs.gnu.org/38236>.
* As with R6RS, a number of lexical features of R7RS conflict with Guile’s
  historical syntax. In addition to `r6rs-hex-escapes` and
  `hungry-eol-escapes` (see [Incompatibilities with the R6RS](#R6RS-Incompatibilities)), the
  `r7rs-symbols` reader feature needs to be explicitly enabled.

Guile exposes a procedure in the root module to choose R7RS defaults
over Guile’s historical defaults.

Scheme Procedure: **install-r7rs!** [¶](#index-install_002dr7rs_0021)
:   Alter Guile’s default settings to better conform to the R7RS.

    While Guile’s defaults may evolve over time, the current changes that
    this procedure imposes are to add `.sls` and `.guile.sls` to
    the set of supported `%load-extensions`, to better support R7RS
    conventions. See [Load Paths](#Load-Paths). `install-r7rs!` will also enable
    the reader options mentioned above.

Finally, note that the `--r7rs` command-line argument will call
`install-r7rs!` before calling user code. R7RS users probably want
to pass this argument to their Guile.

---

Previous: [Incompatibilities with the R7RS](#R7RS-Incompatibilities), Up: [R7RS Support](#R7RS-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.7.2 R7RS Standard Libraries [¶](#R7RS-Standard-Libraries-1)

The R7RS organizes the definitions from R5RS into modules, and also adds
a few new definitions.

We do not attempt to document these libraries fully here, as unlike
R6RS, there are few new definitions in R7RS relative to R5RS. Most of
their functionality is already in Guile’s standard environment. Again,
the expectation is that most Guile users will use the well-known and
well-documented Guile modules; these R7RS libraries are mostly useful to
users who want to port their code to other R7RS systems.

As a brief overview, we note that the libraries defined by the R7RS are
as follows:

`(scheme base)`
:   The core functions, mostly corresponding to R5RS minus the elements
    listed separately below, but plus SRFI-34 error handling
    (see [SRFI-34 - Exception handling for programs](#SRFI_002d34)), bytevectors and bytevector ports
    (see [Bytevectors](#Bytevectors)), and some miscellaneous other new procedures.

`(scheme case-lambda)`
:   `case-lambda`.

`(scheme char)`
:   Converting strings and characters to upper or lower case, predicates for
    if a characer is numeric, and so on.

`(scheme complex)`
:   Constructors and accessors for complex numbers.

`(scheme cxr)`
:   `cddr`, `cadadr`, and all that.

`(scheme eval)`
:   `eval`, but also an `environment` routine allowing a user to
    specify an environment using a module import set.

`(scheme file)`
:   `call-with-input-file` and so on.

`(scheme inexact)`
:   Routines that operate on inexact numbers: `sin`, `finite?`,
    and so on.

`(scheme lazy)`
:   Promises.

`(scheme load)`
:   The `load` procedure.

`(scheme process-context)`
:   Environment variables. See [SRFI-98 Accessing environment variables.](#SRFI_002d98). Also, `commmand-line`,
    `emergency-exit` (like Guile’s `primitive-_exit`), and
    `exit`.

`(scheme r5rs)`
:   The precise set of bindings exported by `r5rs`, but without
    `transcript-off` / `transcript-on`, and also with the
    auxiliary syntax definitions like `_` or `else`. See [Syntax-rules Macros](#Syntax-Rules), for more on auxiliary syntax.

`(scheme read)`
:   The `read` procedure.

`(scheme repl)`
:   The `interaction-environment` procedure.

`(scheme time)`
:   `current-second`, as well as `current-jiffy` and
    `jiffies-per-second`. Guile uses the term “internal time unit”
    for what R7RS calls “jiffies”.

`(scheme write)`
:   `display`, `write`, as well as `write-shared` and
    `write-simple`.

For complete documentation, we advise the interested user to consult the
R7RS directly (see [R7RS](r7rs.html#R7RS) in The Revised^7 Report on the
Algorithmic Language Scheme).

---

Next: [Readline Support](#Readline-Support), Previous: [R7RS Support](#R7RS-Support), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 7.8 Pattern Matching [¶](#Pattern-Matching-1)

The `(ice-9 match)` module provides a *pattern matcher*,
written by Alex Shinn, and compatible with Andrew K. Wright’s pattern
matcher found in many Scheme implementations.

A pattern matcher can match an object against several patterns and
extract the elements that make it up. Patterns can represent any Scheme
object: lists, strings, symbols, records, etc. They can optionally contain
*pattern variables*. When a matching pattern is found, an
expression associated with the pattern is evaluated, optionally with all
pattern variables bound to the corresponding elements of the object:

```
(let ((l '(hello (world))))
  (match l           ;; <- the input object
    (('hello (who))  ;; <- the pattern
     who)))          ;; <- the expression evaluated upon matching
⇒ world
```

In this example, list l matches the pattern `('hello (who))`,
because it is a two-element list whose first element is the symbol
`hello` and whose second element is a one-element list. Here
who is a pattern variable. `match`, the pattern matcher,
locally binds who to the value contained in this one-element
list—i.e., the symbol `world`. An error would be raised if
l did not match the pattern.

The same object can be matched against a simpler pattern:

```
(let ((l '(hello (world))))
  (match l
    ((x y)
     (values x y))))
⇒ hello
⇒ (world)
```

Here pattern `(x y)` matches any two-element list, regardless of
the types of these elements. Pattern variables x and y are
bound to, respectively, the first and second element of l.

Patterns can be composed, and nested. For instance, `...`
(ellipsis) means that the previous pattern may be matched zero or more
times in a list:

```
(match lst
  (((heads tails ...) ...)
   heads))
```

This expression returns the first element of each list within lst.
For proper lists of proper lists, it is equivalent to `(map car
lst)`. However, it performs additional checks to make sure that
lst and the lists therein are proper lists, as prescribed by the
pattern, raising an error if they are not.

Compared to hand-written code, pattern matching noticeably improves
clarity and conciseness—no need to resort to series of `car` and
`cdr` calls when matching lists, for instance. It also improves
robustness, by making sure the input *completely* matches the
pattern—conversely, hand-written code often trades robustness for
conciseness. And of course, `match` is a macro, and the code it
expands to is just as efficient as equivalent hand-written code.

The pattern matcher is defined as follows:

Scheme Syntax: **match** exp clause1 clause2 … [¶](#index-match)
:   Match object exp against the patterns in clause1
    clause2 … in the order in which they appear. Return the
    value produced by the first matching clause. If no clause matches,
    throw an exception with key `match-error`.

    Each clause has the form `(pattern body1 body2 …)`. Each
    pattern must follow the syntax described below. Each body is an
    arbitrary Scheme expression, possibly referring to pattern variables of
    pattern.

The syntax and interpretation of patterns is as follows:

```
        patterns:                       matches:

pat ::= identifier                      anything, and binds identifier
      | _                               anything
      | ()                              the empty list
      | #t                              #t
      | #f                              #f
      | string                          a string
      | number                          a number
      | character                       a character
      | 'sexp                           an s-expression
      | 'symbol                         a symbol (special case of s-expr)
      | (pat_1 ... pat_n)               list of n elements
      | (pat_1 ... pat_n . pat_{n+1})   list of n or more
      | (pat_1 ... pat_n pat_n+1 ooo)   list of n or more, each element
                                          of remainder must match pat_n+1
      | #(pat_1 ... pat_n)              vector of n elements
      | #(pat_1 ... pat_n pat_n+1 ooo)  vector of n or more, each element
                                          of remainder must match pat_n+1
      | #&pat                           box
      | ($ record-name pat_1 ... pat_n) a record
      | (= field pat)                   a ``field'' of an object
      | (and pat_1 ... pat_n)           if all of pat_1 thru pat_n match
      | (or pat_1 ... pat_n)            if any of pat_1 thru pat_n match
      | (not pat_1 ... pat_n)           if all pat_1 thru pat_n don't match
      | (? predicate pat_1 ... pat_n)   if predicate true and all of
                                          pat_1 thru pat_n match
      | (set! identifier)               anything, and binds setter
      | (get! identifier)               anything, and binds getter
      | `qp                             a quasi-pattern
      | (identifier *** pat)            matches pat in a tree and binds
                                        identifier to the path leading
                                        to the object that matches pat

ooo ::= ...                             zero or more
      | ___                             zero or more
      | ..1                             1 or more

        quasi-patterns:                 matches:

qp  ::= ()                              the empty list
      | #t                              #t
      | #f                              #f
      | string                          a string
      | number                          a number
      | character                       a character
      | identifier                      a symbol
      | (qp_1 ... qp_n)                 list of n elements
      | (qp_1 ... qp_n . qp_{n+1})      list of n or more
      | (qp_1 ... qp_n qp_n+1 ooo)      list of n or more, each element
                                          of remainder must match qp_n+1
      | #(qp_1 ... qp_n)                vector of n elements
      | #(qp_1 ... qp_n qp_n+1 ooo)     vector of n or more, each element
                                          of remainder must match qp_n+1
      | #&qp                            box
      | ,pat                            a pattern
      | ,@pat                           a pattern
```

The names `quote`, `quasiquote`, `unquote`,
`unquote-splicing`, `?`, `_`, `$`, `and`,
`or`, `not`, `set!`, `get!`, `...`, and
`___` cannot be used as pattern variables.

Here is a more complex example:

```
(use-modules (srfi srfi-9))

(let ()
  (define-record-type person
    (make-person name friends)
    person?
    (name    person-name)
    (friends person-friends))

  (letrec ((alice (make-person "Alice" (delay (list bob))))
           (bob   (make-person "Bob" (delay (list alice)))))
    (match alice
      (($ person name (= force (($ person "Bob"))))
       (list 'friend-of-bob name))
      (_ #f))))

⇒ (friend-of-bob "Alice")
```

Here the `$` pattern is used to match a SRFI-9 record of type
person containing two or more slots. The value of the first slot
is bound to name. The `=` pattern is used to apply
`force` on the second slot, and then checking that the result
matches the given pattern. In other words, the complete pattern matches
any person whose second slot is a promise that evaluates to a
one-element list containing a person whose first slot is
`"Bob"`.

The `(ice-9 match)` module also provides the following convenient
syntactic sugar macros wrapping around `match`.

Scheme Syntax: **match-lambda** clause1 clause2 … [¶](#index-match_002dlambda)
:   Create a procedure of one argument that matches its argument against
    each clause, and returns the result of evaluating the corresponding
    expressions.

    ```
    (match-lambda clause1 clause2 ...)
    ≍
    (lambda (arg) (match arg clause1 clause2 ...))
    ```

```
((match-lambda
   (('hello (who))
    who))
 '(hello (world)))
⇒ world
```

Scheme Syntax: **match-lambda\*** clause1 clause2 … [¶](#index-match_002dlambda_002a)
:   Create a procedure of any number of arguments that matches its argument
    list against each clause, and returns the result of evaluating the
    corresponding expressions.

    ```
    (match-lambda* clause1 clause2 ...)
    ≍
    (lambda args (match args clause1 clause2 ...))
    ```

```
((match-lambda*
   (('hello (who))
    who))
 'hello '(world))
⇒ world
```

Scheme Syntax: **match-let** ((pattern expression) …) body [¶](#index-match_002dlet)
:   Match each pattern to the corresponding expression, and evaluate the
    body with all matched variables in scope. Raise an error if any of the
    expressions fail to match. `match-let` is analogous to named let
    and can also be used for recursive functions which match on their
    arguments as in `match-lambda*`.

    ```
    (match-let (((x y) (list 1 2))
                ((a b) (list 3 4)))
      (list a b x y))
    ⇒
    (3 4 1 2)
    ```

Scheme Syntax: **match-let** variable ((pattern init) …) body [¶](#index-match_002dlet-1)
:   Similar to `match-let`, but analogously to *named let*, locally
    bind VARIABLE to a new procedure which accepts as many arguments as
    there are INIT expressions. The procedure is initially applied to the
    results of evaluating the INIT expressions. When called, the procedure
    matches each argument against the corresponding PATTERN, and returns the
    result(s) of evaluating the BODY expressions. See [Iteration](#while-do), for more on *named let*.

Scheme Syntax: **match-let\*** ((variable expression) …) body [¶](#index-match_002dlet_002a)
:   Similar to `match-let`, but analogously to `let*`, match and
    bind the variables in sequence, with preceding match variables in scope.

    ```
    (match-let* (((x y) (list 1 2))
                 ((a b) (list x 4)))
      (list a b x y))
    ≍
    (match-let (((x y) (list 1 2)))
      (match-let (((a b) (list x 4)))
        (list a b x y)))
    ⇒
    (1 4 1 2)
    ```

Scheme Syntax: **match-letrec** ((variable expression) …) body [¶](#index-match_002dletrec)
:   Similar to `match-let`, but analogously to `letrec`, match and
    bind the variables with all match variables in scope.

Guile also comes with a pattern matcher specifically tailored to SXML
trees, See [`sxml-match`: Pattern Matching of SXML](#sxml_002dmatch).

---

Next: [Pretty Printing](#Pretty-Printing), Previous: [Pattern Matching](#Pattern-Matching), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
