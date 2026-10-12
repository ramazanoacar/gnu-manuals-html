```
    (bytestring-join '(#u8"Heart" #u8"of" #u8"Gold") #x20)
      ⇒ #u8"Heart of Gold"
    (bytestring-join '(#u8(#xef #xbb) #u8(#xbf)) 0 'prefix)
      ⇒ #u8(0 #xef #xbb 0 #xbf)
    (bytestring-join '() 0 'strict-infix)
      ⇒ ⇒ raised &bytestring-error
    ```

Scheme Procedure: **bytestring-split** bytevector delimiter [grammar] [¶](#index-bytestring_002dsplit)
:   Divides the elements of bytevector and returns a list of newly
    allocated bytevectors using the delimiter (an ASCII character or
    exact integer in the range 0-255 inclusive). Delimiter bytes are not
    included in the result bytevectors.

    The grammar argument is used to control how bytevector is
    divided. It has the same default and meaning as in
    `bytestring-join`, except that `infix` and `strict-infix`
    mean the same thing. That is, if grammar is `prefix` or
    `suffix`, then ignore any delimiter in the first or last position
    of bytevector respectively.

    ```
    (bytestring-split #u8"Beeblebrox" #x62)
      ⇒ (#u8"Bee" #u8"le" #u8"rox")
    (bytestring-split #u8(1 0 2 0) 0 'suffix)
      ⇒ (#u8(1) #u8(2))
    ```

---

Next: [Exceptions](#SRFI_002d207-Exceptions), Previous: [Joining And Splitting](#SRFI_002d207-Joining-And-Splitting), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.9 I/O [¶](#I_002fO)

Scheme Procedure: **read-textual-bytestring** prefix [port] [¶](#index-read_002dtextual_002dbytestring)
:   Reads a string in the external format described in this SRFI from
    port and return it as a bytevector. If the prefix argument
    is false, this procedure assumes that "`#u8`" has already been read
    from port. If port is omitted, it defaults to the value of
    `(current-input-port)`. If the characters read are not in the
    external format, an error satisfying `bytestring-error?` is raised.

    ```
    (call-with-port
        (open-input-string "#u8\"AB\\xad;\\xf0;\\x0d;CD\"")
      (lambda (port) (read-textual-bytestring #t port)))
    ⇒ #u8(#x41 #x42 #xad #xf0 #x0d #x43 #x44)
    ```

Scheme Procedure: **write-textual-bytestring** bytevector [port] [¶](#index-write_002dtextual_002dbytestring)
:   Writes bytevector in the external format described in this SRFI to
    port. Bytes representing non-graphical ASCII characters are
    unencoded: all other bytes are encoded with a single letter if possible,
    otherwise with a `\x` escape. If port is omitted, it
    defaults to the value of `(current-output-port)`.

    ```
    (call-with-port
        (open-output-string)
      (lambda (port)
        (write-textual-bytestring
         #u8(#x9 #x41 #x72 #x74 #x68 #x75 #x72 #xa)
         port)
        (get-output-string port)))
    ⇒ "#u8\"\\tArthur\\n\""
    ```

Scheme Procedure: **write-binary-bytestring** port arg … [¶](#index-write_002dbinary_002dbytestring)
:   Outputs each arg to the binary output port port using the
    same interpretations as `bytestring`, but without creating any
    bytevectors. The args are validated before any bytes are written
    to port; if they are ill-formed, an error satisfying
    `bytestring-error?` is raised.

    ```
    (call-with-port
        (open-output-bytevector)
      (lambda (port)
        (write-binary-bytestring port #\Z #x61 #x70 "hod")
        (get-output-bytevector port)))
    ⇒ #u8"Zaphod"
    ```

---

Next: [Acknowledgements](#SRFI_002d207-Acknowledgements), Previous: [I/O](#SRFI_002d207-I_002fO), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.10 Exceptions [¶](#Exceptions-2)

Scheme Procedure: **bytestring-error?** obj [¶](#index-bytestring_002derror_003f)
:   Returns `#t` if obj is a `&bytestring-error` signaled by
    any of the following procedures, in the circumstances they describe:

    * `bytestring`
    * `hex-string->bytestring`
    * `base64->bytestring`
    * `make-bytestring`
    * `make-bytestring!`
    * `bytestring-join`
    * `read-textual-bytestring`
    * `write-binary-bytestring`
    * `make-bytestring-generator`

---

Previous: [Exceptions](#SRFI_002d207-Exceptions), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.11 Acknowledgements [¶](#Acknowledgements-1)

Daphne Preston-Kendal devised the string notation for bytevectors; John
Cowan, the procedure library; Wolfgang Corcoran-Mathe, the original,
sample implementation of the procedures.

The notation is inspired by the notation used in Python since version
2.6 for `bytes` objects, which are fundamentally similar in purpose
to Scheme bytevectors, especially in R7RS. In addition, many of the
procedures are closely analogous to those of
[SRFI 152](https://srfi.schemers.org/srfi-152/srfi-152.html).

Thanks is also due to the participants in the SRFI mailing list. In
particular: Lassi Kortela corrected an embarrassing technical error;
Marc Nieper-Wißkirchen explained why the `write` procedure ought
not to be allowed to use this notation by default.

---

Previous: [SRFI-207 String-notated bytevectors](#SRFI_002d207), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.50 SRFI-244 - Multiple-value Definitions [¶](#SRFI_002d244-_002d-Multiple_002dvalue-Definitions)

The implementation of this SRFI re-exports the `define-values` form
described in [Binding multiple return values](#Binding-Multiple-Values).

---

Next: [R7RS Support](#R7RS-Support), Previous: [SRFI Support Modules](#SRFI-Support), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
