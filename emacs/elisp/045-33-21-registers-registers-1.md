### 33.21 Registers [¶](#Registers-1)

A register is a sort of variable used in Emacs editing that can hold a
variety of different kinds of values. Each register is named by a
single character. All ASCII characters and their meta variants
(but with the exception of `C-g`) can be used to name registers.
Thus, there are 255 possible registers. A register is designated in
Emacs Lisp by the character that is its name.

Variable: **register-alist** [¶](#index-register_002dalist)
:   This variable is an alist of elements of the form `(name .
    contents)`. Normally, there is one element for each Emacs
    register that has been used.

    The object name is a character (an integer) identifying the
    register.

The contents of a register can have several possible types:

a number
:   A number stands for itself. If `insert-register` finds a number
    in the register, it converts the number to decimal.

a marker
:   A marker represents a buffer position to jump to.

a string
:   A string is text saved in the register.

a rectangle [¶](#index-rectangle_002c-as-contents-of-a-register)
:   A rectangle is represented by a list of strings.

`(window-configuration position)`
:   This represents a window configuration to restore in one frame, and a
    position to jump to in the current buffer.

`(frame-configuration position)` [¶](#index-frameset)
:   This represents a frame configuration to restore, and a position
    to jump to in the current buffer. Frame configurations are also
    known as *framesets*.

`(file filename)`
:   This represents a file to visit; jumping to this value visits file
    filename.

`(file-query filename position)`
:   This represents a file to visit and a position in it; jumping to this
    value visits file filename and goes to buffer position
    position. Restoring this type of position asks the user for
    confirmation first.

`(buffer buffer)`
:   This represents a buffer; jumping to this value switches to that
    buffer.

The functions in this section return unpredictable values unless
otherwise stated.

Function: **get-register** reg [¶](#index-get_002dregister)
:   This function returns the contents of the register
    reg, or `nil` if it has no contents.

Function: **set-register** reg value [¶](#index-set_002dregister)
:   This function sets the contents of register reg to value.
    A register can be set to any value, but the other register functions
    expect only certain data types. The return value is value.

Command: **view-register** reg [¶](#index-view_002dregister)
:   This command displays what is contained in register reg.

Command: **insert-register** reg &optional beforep [¶](#index-insert_002dregister)
:   This command inserts contents of register reg into the current
    buffer.

    Normally, this command puts point before the inserted text, and the
    mark after it. However, if the optional second argument beforep
    is non-`nil`, it puts the mark before and point after.

    When called interactively, the command defaults to putting point after
    text, and a prefix argument inverts this behavior.

    If the register contains a rectangle, then the rectangle is inserted
    with its upper left corner at point. This means that text is inserted
    in the current line and underneath it on successive lines.

    If the register contains something other than saved text (a string) or
    a rectangle (a list), currently useless things happen. This may be
    changed in the future.

Function: **register-read-with-preview** prompt [¶](#index-register_002dread_002dwith_002dpreview)
:   This function reads and returns a register name, prompting with
    prompt and possibly showing a preview of the existing registers
    and their contents. The preview is shown in a temporary window, after
    the delay specified by the user option `register-preview-delay`,
    if its value and `register-alist` are both non-`nil`. The
    preview is also shown if the user requests help (e.g., by typing the
    help character). We recommend that all interactive commands which
    read register names use this function.

---

Next: [Replacing Buffer Text](#Replacing), Previous: [Registers](#Registers), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.22 Transposition of Text [¶](#Transposition-of-Text)

This function can be used to transpose stretches of text:

Function: **transpose-regions** start1 end1 start2 end2 &optional leave-markers [¶](#index-transpose_002dregions)
:   This function exchanges two nonoverlapping portions of the buffer (if
    they overlap, the function signals an error). Arguments start1
    and end1 specify the bounds of one portion and arguments
    start2 and end2 specify the bounds of the other portion.

    Normally, `transpose-regions` relocates markers with the transposed
    text; a marker previously positioned within one of the two transposed
    portions moves along with that portion, thus remaining between the same
    two characters in their new position. However, if leave-markers
    is non-`nil`, `transpose-regions` does not do this—it leaves
    all markers unrelocated.

---

Next: [Dealing With Compressed Data](#Decompression), Previous: [Transposition of Text](#Transposition), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.23 Replacing Buffer Text [¶](#Replacing-Buffer-Text)

You can use the following function to replace some the text of the
current buffer:

Function: **replace-region-contents** beg end source &optional max-secs max-costs inherit [¶](#index-replace_002dregion_002dcontents)
:   This function replaces the region between beg and end
    of the current buffer with the text found in source which
    is usually a string or a buffer, in which case it will use the
    accessible portion of that buffer.

    This function attempts to keep point, markers, text properties, and
    overlays in the current buffer intact. One potential case where this
    behavior is useful is external code formatting programs: they typically
    write the reformatted text into a temporary buffer or file, and using
    `insert` and `delete-region` would destroy these properties.

    However, in order to do that, `replace-region-contents` needs to
    compare the contents of the original buffer with that of source,
    using a costly algorithm which makes the operation much slower than
    a simple `insert` and `delete-region`. In many cases, you may
    not need that refinement, and you will then want to pass 0 as
    max-secs argument, so as to short-circuit that costly algorithm:
    It will then be just as fast as `insert` and `delete-region`
    while still preserving point and markers marginally better.

    Beyond that basic usage, if you need to use as source a subset of the
    accessible portion of a buffer, source can also be a vector
    `[sbuf sbeg send]` where the region between
    sbeg and send in buffer sbuf is the text
    you want to use as source.

    If you need the inserted text to inherit text-properties
    from the adjoining text, you can pass a non-`nil` value as
    inherit argument.

    When you do want the costly refined replacement, in order to keep
    `replace-region-contents`’s runtime in bounds, it has two
    optional arguments.

    max-secs defines a hard boundary in terms of seconds. If given
    and exceeded, it will fall back to `delete-region` and
    `insert-buffer-substring`.

    max-costs defines the quality of the difference computation. If
    the actual costs exceed this limit, heuristics are used to provide a
    faster but suboptimal solution. The default value is 1000000.

    `replace-region-contents` returns `t` if a non-destructive
    replacement could be performed. Otherwise, i.e., if max-secs
    was exceeded, it returns `nil`.

    Note: When using the refined replacement algorithm, if the replacement
    is a string, it will be internally copied to a temporary buffer.
    Therefore, all else being equal, it is preferable to pass a buffer than
    a string as source argument.

---

Next: [Base 64 Encoding](#Base-64), Previous: [Replacing Buffer Text](#Replacing), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.24 Dealing With Compressed Data [¶](#Dealing-With-Compressed-Data)

When `auto-compression-mode` is enabled, Emacs automatically
uncompresses compressed files when you visit them, and automatically
recompresses them if you alter and save them. See [Compressed
Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Compressed-Files) in The GNU Emacs Manual.

The above feature works by calling an external executable (e.g.,
`gzip`). Emacs can also be compiled with support for built-in
decompression using the zlib library, which is faster than calling an
external program.

Function: **zlib-available-p** [¶](#index-zlib_002davailable_002dp)
:   This function returns non-`nil` if built-in zlib decompression is
    available.

Function: **zlib-decompress-region** start end &optional allow-partial [¶](#index-zlib_002ddecompress_002dregion)
:   This function decompresses the region between start and
    end, using built-in zlib decompression. The region should
    contain data that were compressed with gzip or zlib. On success, the
    function replaces the contents of the region with the decompressed
    data. If allow-partial is `nil` or omitted, then on
    failure, the function leaves the region unchanged and returns
    `nil`. Otherwise, it returns the number of bytes that were not
    decompressed and replaces the region text by whatever data was
    successfully decompressed. This function can be called only in
    unibyte buffers.

---

Next: [Checksum/Hash](#Checksum_002fHash), Previous: [Dealing With Compressed Data](#Decompression), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.25 Base 64 Encoding [¶](#Base-64-Encoding)

Base 64 code is used in email to encode a sequence of 8-bit bytes as
a longer sequence of ASCII graphic characters. It is defined in
Internet RFC[26](#FOOT26)2045 and also in RFC 4648. This section describes the functions for
converting to and from this code.

Command: **base64-encode-region** beg end &optional no-line-break [¶](#index-base64_002dencode_002dregion)
:   This function converts the region from beg to end into base
    64 code. It returns the length of the encoded text. An error is
    signaled if a character in the region is multibyte, i.e., in a
    multibyte buffer the region must contain only ASCII characters or raw
    bytes.

    Normally, this function inserts newline characters into the encoded
    text, to avoid overlong lines. However, if the optional argument
    no-line-break is non-`nil`, these newlines are not added, so
    the output is just one long line.

Command: **base64url-encode-region** beg end &optional no-pad [¶](#index-base64url_002dencode_002dregion)
:   This function is like `base64-encode-region`, but it implements
    the URL variant of base 64 encoding, per RFC 4648, and it doesn’t
    insert newline characters into the encoded text, so the output is
    just one long line.

    If the optional argument no-pad is non-`nil` then this
    function doesn’t generate the padding (`=`).

Function: **base64-encode-string** string &optional no-line-break [¶](#index-base64_002dencode_002dstring)
:   This function converts the string string into base 64 code. It
    returns a string containing the encoded text. As for
    `base64-encode-region`, an error is signaled if a character in the
    string is multibyte.

    Normally, this function inserts newline characters into the encoded
    text, to avoid overlong lines. However, if the optional argument
    no-line-break is non-`nil`, these newlines are not added, so
    the result string is just one long line.

Function: **base64url-encode-string** string &optional no-pad [¶](#index-base64url_002dencode_002dstring)
:   Like `base64-encode-string`, but generates the URL variant of
    base 64, and doesn’t insert newline characters into the encoded text,
    so the result is just one long line.

    If the optional argument no-pad is non-`nil` then this
    function doesn’t generate the padding.

Command: **base64-decode-region** beg end &optional base64url ignore-invalid [¶](#index-base64_002ddecode_002dregion)
:   This function converts the region from beg to end from base
    64 code into the corresponding decoded text. It returns the length of
    the decoded text.

    The decoding functions ignore newline characters in the encoded text.

    If optional argument base64url is non-`nil`, then padding
    is optional, and the URL variant of base 64 encoding is used.
    If optional argument ignore-invalid is non-`nil`, then any
    unrecognized characters are ignored.

Function: **base64-decode-string** string &optional base64url ignore-invalid [¶](#index-base64_002ddecode_002dstring)
:   This function converts the string string from base 64 code into
    the corresponding decoded text. It returns a unibyte string containing the
    decoded text.

    The decoding functions ignore newline characters in the encoded text.

    If optional argument base64url is non-`nil`, then padding
    is optional, and the URL variant of base 64 encoding is used.
    If optional argument ignore-invalid is non-`nil`, then any
    unrecognized characters are ignored.

---

Next: [Suspicious Text](#Suspicious-Text), Previous: [Base 64 Encoding](#Base-64), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.26 Checksum/Hash [¶](#Checksum_002fHash-1)

Emacs has built-in support for computing *cryptographic hashes*.
A cryptographic hash, or *checksum*, is a digital fingerprint
of a piece of data (e.g., a block of text) which can be used to check
that you have an unaltered copy of that data.

Emacs supports several common cryptographic hash algorithms: MD5,
SHA-1, SHA-2, SHA-224, SHA-256, SHA-384, SHA-512, SHA3-224, SHA3-256,
SHA3-384, and SHA3-512. MD5 is the oldest of these algorithms, and is
commonly used in *message digests* to check the integrity of
messages transmitted over a network. MD5 and SHA-1 are not collision
resistant (i.e., it is possible to deliberately design different pieces
of data which have the same MD5 or SHA-1 hash), so you should not use
them for anything security-related. For security-related applications
you should use the other hash types, such as SHA-2 (e.g., `sha256`
or `sha512`) or SHA-3 (e.g., `sha3-256` or `sha3-512`).

Function: **secure-hash-algorithms** [¶](#index-secure_002dhash_002dalgorithms)
:   This function returns a list of symbols representing algorithms that
    `secure-hash` can use.

Function: **secure-hash** algorithm object &optional start end binary [¶](#index-secure_002dhash)
:   This function returns a hash for object. The argument
    algorithm is a symbol stating which hash to compute: one of
    `md5`, `sha1`, `sha224`, `sha256`, `sha384`,
    `sha512`, `sha3-224`, `sha3-256`, `sha3-384`, or
    `sha3-512`. The argument object should be a buffer or a
    string.

    The optional arguments start and end are character
    positions specifying the portion of object to compute the
    message digest for. If they are `nil` or omitted, the hash is
    computed for the whole of object.

    If the argument binary is omitted or `nil`, the function
    returns the *text form* of the hash, as an ordinary Lisp string.
    If binary is non-`nil`, it returns the hash in *binary
    form*, as a sequence of bytes stored in a unibyte string. The length
    of the returned string depends on algorithm:

    * For `md5`: 32 characters (16 bytes if binary is
      non-`nil`).
    * For `sha1`: 40 characters (20 bytes if binary is
      non-`nil`).
    * For `sha224`: 56 characters (28 bytes if binary is
      non-`nil`).
    * For `sha256`: 64 characters (32 bytes if binary is
      non-`nil`).
    * For `sha384`: 96 characters (48 bytes if binary is
      non-`nil`).
    * For `sha512`: 128 characters (64 bytes if binary is
      non-`nil`).
    * For `sha3-224`: 56 characters (28 bytes if binary is
      non-`nil`).
    * For `sha3-256`: 64 characters (32 bytes if binary is
      non-`nil`).
    * For `sha3-384`: 96 characters (48 bytes if binary is
      non-`nil`).
    * For `sha3-512`: 128 characters (64 bytes if binary is
      non-`nil`).

    This function does not compute the hash directly from the internal
    representation of object’s text (see [Text Representations](#Text-Representations)).
    Instead, it encodes the text using a coding system (see [Coding Systems](#Coding-Systems)), and computes the hash from that encoded text. If
    object is a buffer, the coding system used is the one which
    would be chosen by default for writing the text of that buffer into a
    file. If object is a string, the user’s preferred coding system
    is used (see [Recognize Coding](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Recognize-Coding) in GNU Emacs Manual).

Function: **md5** object &optional start end coding-system noerror [¶](#index-md5)
:   This function returns an MD5 hash. It is semi-obsolete, since for
    most purposes it is equivalent to calling `secure-hash` with
    `md5` as the algorithm argument. The object,
    start and end arguments have the same meanings as in
    `secure-hash`. The function returns a 32-character string.

    If coding-system is non-`nil`, it specifies a coding system
    to use to encode the text; if omitted or `nil`, the default
    coding system is used, like in `secure-hash`.

    Normally, `md5` signals an error if the text can’t be encoded
    using the specified or chosen coding system. However, if
    noerror is non-`nil`, it silently uses `raw-text`
    coding instead.

Function: **buffer-hash** &optional buffer-or-name [¶](#index-buffer_002dhash)
:   Return a hash of buffer-or-name. If `nil`, this defaults
    to the current buffer. As opposed to `secure-hash`, this
    function computes the hash based on the internal representation of the
    buffer, disregarding any coding systems. It’s therefore only useful
    when comparing two buffers running in the same Emacs, and is not
    guaranteed to return the same hash between different Emacs versions.
    It should be somewhat more efficient on larger buffers than
    `secure-hash` is, and should not allocate more memory.

Function: **sha1** object &optional start end binary [¶](#index-sha1)
:   This function is equivalent to calling `secure-hash` like this:

    ```
    (secure-hash 'sha1 object start end binary)
    ```

    It returns a 40-character string if binary is `nil`, or a
    20-byte unibyte string otherwise.

---

Next: [GnuTLS Cryptography](#GnuTLS-Cryptography), Previous: [Checksum/Hash](#Checksum_002fHash), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.27 Suspicious Text [¶](#Suspicious-Text-1)

Emacs can display text from many external sources, like email and Web
sites. Attackers may attempt to confuse the user reading this text by
using obfuscated URLs or email addresses, and tricking the
user into visiting a web page they didn’t intend to visit, or sending
an email to the wrong address.

This usually involves using characters from scripts that visually look
like ASCII characters (i.e., are homoglyphs), but there are
also other techniques used, like using bidirectional overrides, or
having an HTML link text that says one thing, while the
underlying URL points somewhere else.

To help identify these *suspicious text strings*, Emacs provides a
library to do a number of checks on text. (See
[UTS #39: Unicode Security
Mechanisms](https://www.unicode.org/reports/tr39/) for the rationale behind the checks that are available and
more details about them.) Packages that present data that might be
suspicious should use this library to flag suspicious text on display.

Function: **textsec-suspicious-p** object type [¶](#index-textsec_002dsuspicious_002dp)
:   This function is the high-level interface function that packages
    should use. It respects the `textsec-check` user option, which
    allows the user to disable the checks.

    This function checks object (whose data type depends on
    type) to see if it looks suspicious when interpreted as a thing
    of type. The available types and the corresponding object
    data types are:

    `domain`
    :   Check whether a domain (e.g., ‘`www.gnu.org`’ looks suspicious.
        object should be a string, the domain name.

    `url`
    :   Check whether an URL (e.g., ‘`http://gnu.org/foo/bar`’)
        looks suspicious. object should be a string, the URL
        to check.

    `link`
    :   Check whether an HTML link (e.g., ‘`<a
        href='http://gnu.org'>fsf.org</a>`’ looks suspicious. In this case,
        object should be a `cons` cell where the `car` is the
        URL string, and the `cdr` is the link text. The link
        is deemed suspicious if the link text contains a domain name, and that
        domain name points to something other than the URL.

    `email-address`
    :   Check whether an email address (e.g., ‘`foo@example.org`’) looks
        suspicious. object should be a string.

    `local-address`
    :   Check whether the local part of an email address (the bit before the
        ‘`@`’ sign) looks suspicious. object should be a string.

    `name`
    :   Check whether a name (used in an email address header) looks
        suspicious. object should be a string.

    `email-address-header`
    :   Check whether a full RFC2822 email address header (e.g.,
        ‘`=?utf-8?Q?=E2=84=AB?= <foo@example.com>`’) looks suspicious.
        object should be a string.

    If object is suspicious, this function returns a string that
    explains why it is suspicious. If object is not suspicious, the
    function returns `nil`.

If the text is suspicious, the application should mark the suspicious
text with the `textsec-suspicious` face, and make the explanation
returned by `textsec-suspicious-p` available to the user in some way
(for example, in a tooltip). The application might also prompt the
user for confirmation before taking any action on a suspicious string
(like sending an email to a suspicious email address).

---

Next: [Database](#Database), Previous: [Suspicious Text](#Suspicious-Text), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.28 GnuTLS Cryptography [¶](#GnuTLS-Cryptography-1)

If compiled with GnuTLS, Emacs offers built-in cryptographic
support. Following the GnuTLS API terminology, the available tools
are digests, MACs, symmetric ciphers, and AEAD ciphers.

The terms used herein, such as IV (Initialization Vector), require
some familiarity with cryptography and will not be defined in detail.
Please consult <https://www.gnutls.org/> for specific
documentation which may help you understand the terminology and
structure of the GnuTLS library.

* [Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs)
* [GnuTLS Cryptographic Functions](#GnuTLS-Cryptographic-Functions)

---

Next: [GnuTLS Cryptographic Functions](#GnuTLS-Cryptographic-Functions), Up: [GnuTLS Cryptography](#GnuTLS-Cryptography)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.28.1 Format of GnuTLS Cryptography Inputs [¶](#Format-of-GnuTLS-Cryptography-Inputs-1)

The inputs to GnuTLS cryptographic functions can be specified in
several ways, both as primitive Emacs Lisp types or as lists.

The list form is currently similar to how `md5` and
`secure-hash` operate.

`buffer`
:   Simply passing a buffer as input means the whole buffer should be used.

`string`
:   A string as input will be used directly. It may be modified by the
    function (unlike most other Emacs Lisp functions) to reduce the chance
    of exposing sensitive data after the function does its work.

`(buffer-or-string start end coding-system noerror)`
:   This specifies a buffer or a string as described above, but an
    optional range can be specified with start and end.

    In addition an optional coding-system can be specified if needed.

    The last optional item, noerror, overrides the normal error when
    the text can’t be encoded using the specified or chosen coding system.
    When noerror is non-`nil`, this function silently uses
    `raw-text` coding instead.

`(iv-auto length)`
:   This generates a random IV (Initialization Vector) of the specified
    length and passes it to the function. This ensures that the IV is
    unpredictable and unlikely to be reused in the same session.

---

Previous: [Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs), Up: [GnuTLS Cryptography](#GnuTLS-Cryptography)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.28.2 GnuTLS Cryptographic Functions [¶](#GnuTLS-Cryptographic-Functions-1)

Function: **gnutls-digests** [¶](#index-gnutls_002ddigests)
:   This function returns the alist of the GnuTLS digest algorithms.

    Each entry has a key which represents the algorithm, followed by a
    plist with internal details about the algorithm. The plist will have
    `:type gnutls-digest-algorithm` and also will have the key
    `:digest-algorithm-length 64` to indicate the size, in bytes, of
    the resulting digest.

    There is a name parallel between GnuTLS MAC and digest algorithms but
    they are separate things internally and should not be mixed.

Function: **gnutls-hash-digest** digest-method input [¶](#index-gnutls_002dhash_002ddigest)
:   The digest-method can be the whole plist from
    `gnutls-digests`, or just the symbol key, or a string with the
    name of that symbol.

    The input can be specified as a buffer or string or in other
    ways (see [Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs)).

    This function returns `nil` on error, and signals a Lisp error if
    the digest-method or input are invalid. On success, it
    returns a list of a binary string (the output) and the IV used.

Function: **gnutls-macs** [¶](#index-gnutls_002dmacs)
:   This function returns the alist of the GnuTLS MAC algorithms.

    Each entry has a key which represents the algorithm, followed by a
    plist with internal details about the algorithm. The plist will have
    `:type gnutls-mac-algorithm` and also will have the keys
    `:mac-algorithm-length` `:mac-algorithm-keysize`
    `:mac-algorithm-noncesize` to indicate the size, in bytes, of the
    resulting hash, the key, and the nonce respectively.

    The nonce is currently unused and only some MACs support it.

    There is a name parallel between GnuTLS MAC and digest algorithms but
    they are separate things internally and should not be mixed.

Function: **gnutls-hash-mac** hash-method key input [¶](#index-gnutls_002dhash_002dmac)
:   The hash-method can be the whole plist from
    `gnutls-macs`, or just the symbol key, or a string with the
    name of that symbol.

    The key can be specified as a buffer or string or in other ways
    (see [Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs)). The key will be
    wiped after use if it’s a string.

    The input can be specified as a buffer or string or in other
    ways (see [Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs)).

    This function returns `nil` on error, and signals a Lisp error if
    the hash-method or key or input are invalid.

    On success, it returns a list of a binary string (the output) and the
    IV used.

Function: **gnutls-ciphers** [¶](#index-gnutls_002dciphers)
:   This function returns the alist of the GnuTLS ciphers.

    Each entry has a key which represents the cipher, followed by a plist
    with internal details about the algorithm. The plist will have
    `:type gnutls-symmetric-cipher` and also will have the keys
    `:cipher-aead-capable` set to `nil` or `t` to indicate
    AEAD capability; and `:cipher-tagsize` `:cipher-blocksize`
    `:cipher-keysize` `:cipher-ivsize` to indicate the size, in
    bytes, of the tag, block size of the resulting data, the key, and the
    IV respectively.

Function: **gnutls-symmetric-encrypt** cipher key iv input &optional aead\_auth [¶](#index-gnutls_002dsymmetric_002dencrypt)
:   The cipher can be the whole plist from
    `gnutls-ciphers`, or just the symbol key, or a string with the
    name of that symbol.

    The key can be specified as a buffer or string or in other ways
    (see [Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs)). The key will be
    wiped after use if it’s a string.

    The iv and input and the optional aead\_auth can be
    specified as a buffer or string or in other ways (see [Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs)).

    aead\_auth is only checked with AEAD ciphers, that is, ciphers whose
    plist has `:cipher-aead-capable t`. Otherwise it’s ignored.

    This function returns `nil` on error, and signals a Lisp error if
    the cipher or key, iv, or input are invalid,
    or if aead\_auth was specified with an AEAD cipher and was
    invalid.

    On success, it returns a list of a binary string (the output) and the
    IV used.

Function: **gnutls-symmetric-decrypt** cipher key iv input &optional aead\_auth [¶](#index-gnutls_002dsymmetric_002ddecrypt)
:   The cipher can be the whole plist from
    `gnutls-ciphers`, or just the symbol key, or a string with the
    name of that symbol.

    The key can be specified as a buffer or string or in other ways
    (see [Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs)). The key will be
    wiped after use if it’s a string.

    The iv and input and the optional aead\_auth can be
    specified as a buffer or string or in other ways (see [Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs)).

    aead\_auth is only checked with AEAD ciphers, that is, ciphers whose
    plist has `:cipher-aead-capable t`. Otherwise it’s ignored.

    This function returns `nil` on decryption error, and signals a
    Lisp error if the cipher or key, iv, or input
    are invalid, or if aead\_auth was specified with an AEAD cipher
    and was invalid.

    On success, it returns a list of a binary string (the output) and the
    IV used.

---

Next: [Parsing HTML and XML](#Parsing-HTML_002fXML), Previous: [GnuTLS Cryptography](#GnuTLS-Cryptography), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.29 Database [¶](#Database-1)

Emacs can be compiled with built-in support for accessing SQLite
databases. This section describes the facilities available for
accessing SQLite databases from Lisp programs.

Function: **sqlite-available-p** [¶](#index-sqlite_002davailable_002dp)
:   The function returns non-`nil` if built-in SQLite support is
    available in this Emacs session.

When SQLite support is available, the following functions can be used.

Function: **sqlite-open** &optional file readonly disable-uri [¶](#index-sqlite_002dopen)
:   This function opens file as an SQLite database file. If
    file doesn’t exist, a new database will be created and stored in
    that file. If file is omitted or `nil`, a new in-memory
    database is created instead. Second optional argument readonly,
    if non-`nil`, means open the database only for reading; the
    database must already exist in that case. By default, file can be
    a `file://` URI as well as a file name; in the unusual case that
    you have a local file whose name begins with `file:`, specify a
    non-`nil` value for the third optional argument disable-uri
    to disable the automatic recognition and processing of URIs.

    The return value is a *database object* that can be used as the
    argument to most of the subsequent functions described below.

Function: **sqlitep** object [¶](#index-sqlitep)
:   This predicate returns non-`nil` if object is an SQLite
    database object. The database object returned by the
    `sqlite-open` function satisfies this predicate.

Function: **sqlite-close** db [¶](#index-sqlite_002dclose)
:   Close the database db. It’s usually not necessary to call this
    function explicitly—the database will automatically be closed if
    Emacs shuts down or the database object is garbage collected. If the
    connection identified by db is already closed, this function does
    nothing.

Function: **sqlite-execute** db statement &optional values [¶](#index-sqlite_002dexecute)
:   Execute the SQL statement. For instance:

    ```
    (sqlite-execute db "insert into foo values ('bar', 2)")
    ```

    If the optional values parameter is present, it should be either
    a list or a vector of values to bind while executing the statement.
    For instance:

    ```
    (sqlite-execute db "insert into foo values (?, ?)" '("bar" 2))
    ```

    This has exactly the same effect as the previous example, but is more
    efficient and safer (because it doesn’t involve any string parsing or
    interpolation).

    `sqlite-execute` usually returns the number of affected rows.
    For instance, an ‘`insert`’ statement will typically return
    ‘`1`’, whereas an ‘`update`’ statement may return zero or a
    higher number. However, when using SQL statements like
    ‘`insert into … returning …`’ and the like, the values
    specified by ‘`returning …`’ will be returned instead.

    Strings in SQLite are, by default, stored as `utf-8`, and
    selecting a text column will decode the string using that charset.
    Selecting a blob column will return the raw data without any decoding
    (i.e., it will return a unibyte string containing the bytes as stored
    in the database). Inserting binary data into blob columns, however,
    requires some care, as `sqlite-execute` will, by default,
    interpret all strings as `utf-8`.

    So if you have, for instance, GIF data in a unibyte string
    called gif, you have to mark it specially to let
    `sqlite-execute` know this:

    ```
    (put-text-property 0 1 'coding-system 'binary gif)
    (sqlite-execute db "insert into foo values (?, ?)" (list gif 2))
    ```

Function: **sqlite-execute-batch** db statements [¶](#index-sqlite_002dexecute_002dbatch)
:   Execute the SQL statements. statements is a
    string containing 0 or more SQL statements. This command
    might be useful when a Lisp program needs to execute multiple Data
    Definition Language (DDL) statements in one go.

Function: **sqlite-select** db query &optional values return-type [¶](#index-sqlite_002dselect)
:   Select some data from db and return them. For instance:

    ```
    (sqlite-select db "select * from foo where key = 2")
      ⇒ (("bar" 2))
    ```

    As with the `sqlite-execute`, you can optionally pass in a list
    or a vector of values that will be bound before executing the select:

    ```
    (sqlite-select db "select * from foo where key = ?" [2])
      ⇒ (("bar" 2))
    ```

    This is usually more efficient and safer than the method used by the
    previous example.

    By default, this function returns a list of matching rows, where each
    row is a list of column values. If return-type is `full`,
    the names of the columns (as a list of strings) will be returned as
    the first element in the return value.

    If return-type is `set`, this function will return a
    *statement object* instead. This object can be examined by using
    the `sqlite-next`, `sqlite-columns` and `sqlite-more-p`
    functions. If the result set is small, it’s often more convenient to
    just return the data directly, but if the result set is large (or if
    you won’t be using all the data from the set), using the `set`
    method will allocate a lot less memory, and is therefore more
    memory-efficient.

Function: **sqlite-next** statement [¶](#index-sqlite_002dnext)
:   This function returns the next row in the result set statement,
    typically an object returned by `sqlite-select`.

    ```
    (sqlite-next stmt)
        ⇒ ("bar" 2)
    ```

Function: **sqlite-columns** statement [¶](#index-sqlite_002dcolumns)
:   This function returns the column names of the result set
    statement, typically an object returned by `sqlite-select`.

    ```
    (sqlite-columns stmt)
        ⇒ ("name" "issue")
    ```

Function: **sqlite-more-p** statement [¶](#index-sqlite_002dmore_002dp)
:   This predicate says whether there is more data to be fetched from the
    result set statement, typically an object returned by
    `sqlite-select`.

Function: **sqlite-finalize** statement [¶](#index-sqlite_002dfinalize)
:   If statement is not going to be used any more, calling this
    function will free the resources used by statement. This is
    usually not necessary—when the statement object is
    garbage-collected, Emacs will automatically free its resources.

Function: **sqlite-transaction** db [¶](#index-sqlite_002dtransaction)
:   Start a transaction in db. When in a transaction, other readers
    of the database won’t access the results until the transaction has
    been committed by `sqlite-commit`.

Function: **sqlite-commit** db [¶](#index-sqlite_002dcommit)
:   End a transaction in db and write the data out to its file.

Function: **sqlite-rollback** db [¶](#index-sqlite_002drollback)
:   End a transaction in db and discard any changes that have been
    made by the transaction.

Macro: **with-sqlite-transaction** db body… [¶](#index-with_002dsqlite_002dtransaction)
:   Like `progn` (see [Sequencing](#Sequencing)), but executes body with a
    transaction held, and commits the transaction at the end if body
    completes normally. If body signals an error, or committing the
    transaction fails, the changes in db performed by body are
    rolled back. The macro returns the value of body if it
    completes normally and commit succeeds.

Function: **sqlite-pragma** db pragma [¶](#index-sqlite_002dpragma)
:   Execute pragma in db. A *pragma* is usually a command
    that affects the database overall, instead of any particular table.
    For instance, to make SQLite automatically garbage collect data that’s
    no longer needed, you can say:

    ```
    (sqlite-pragma db "auto_vacuum = FULL")
    ```

    This function returns non-`nil` on success and `nil` if the
    pragma failed. Many pragmas can only be issued when the database is
    brand new and empty.

Function: **sqlite-load-extension** db module [¶](#index-sqlite_002dload_002dextension)
:   Load the named extension module into the database db.
    Extensions are usually shared-library files; on GNU and Unix systems,
    they have the `.so` file-name extension.

Function: **sqlite-version** [¶](#index-sqlite_002dversion)
:   Return a string denoting the version of the SQLite library in use.

If you wish to list the contents of an SQLite file, you can use the
`sqlite-mode-open-file` command. This will pop to a buffer using
`sqlite-mode`, which allows you to examine (and alter) the
contents of an SQLite database.

---

Next: [Parsing and generating JSON values](#Parsing-JSON), Previous: [Database](#Database), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.30 Parsing HTML and XML [¶](#Parsing-HTML-and-XML)

Emacs can be compiled with built-in `libxml2` support.

Function: **libxml-available-p** [¶](#index-libxml_002davailable_002dp)
:   This function returns non-`nil` if built-in libxml2 support is
    available in this Emacs session.

When libxml2 support is available, the following functions can be used
to parse HTML or XML text into Lisp object trees.

Function: **libxml-parse-html-region** &optional start end base-url discard-comments [¶](#index-libxml_002dparse_002dhtml_002dregion)
:   This function parses the text between start and end as
    HTML, and returns a list representing the HTML *parse tree*. It
    attempts to handle real-world HTML by robustly coping with syntax
    mistakes.

    If start or end are `nil`, they default to the values
    from `point-min` and `point-max`, respectively.

    The optional argument base-url, if non-`nil`, should be
    used for warnings and errors reported by the `libxml2` library,
    but Emacs currently calls the library with errors and warnings
    disabled, so this argument is not used.

    If the optional argument discard-comments is non-`nil`,
    any top-level comment is discarded. (This argument is obsolete and
    will be removed in future Emacs versions. To remove comments, use the
    `xml-remove-comments` utility function on the data before you
    call the parsing function.)

    In the parse tree, each HTML node is represented by a list in which
    the first element is a symbol representing the node name, the second
    element is an alist of node attributes, and the remaining elements are
    the subnodes.

    The following example demonstrates this. Given this (malformed) HTML
    document:

    ```
    <html><head></head><body width=101><div class=thing>Foo<div>Yes
    ```

    A call to `libxml-parse-html-region` returns this DOM
    (document object model):

    ```
    (html nil
     (head nil)
     (body ((width . "101"))
      (div ((class . "thing"))
       "Foo"
       (div nil
        "Yes"))))
    ```

Function: **shr-insert-document** dom [¶](#index-shr_002dinsert_002ddocument)
:   This function renders the parsed HTML in dom into the current
    buffer. The argument dom should be a list as generated by
    `libxml-parse-html-region`. This function is, e.g., used by
    [EWW](https://www.gnu.org/software/emacs/manual/html_mono/eww.html#Top) in The Emacs Web Wowser Manual.

Function: **libxml-parse-xml-region** &optional start end base-url discard-comments [¶](#index-libxml_002dparse_002dxml_002dregion)
:   This function is the same as `libxml-parse-html-region`, except
    that it parses the text as XML rather than HTML (so it is stricter
    about syntax).

* [Document Object Model](#Document-Object-Model)

---

Up: [Parsing HTML and XML](#Parsing-HTML_002fXML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.30.1 Document Object Model [¶](#Document-Object-Model-1)

The DOM returned by `libxml-parse-html-region` (and
the other XML parsing functions) is a tree structure where
each node has a node name (called a *tag*), and optional key/value
*attribute* list, and then a list of *child nodes*. The child
nodes are either strings or DOM objects.

```
(body ((width . "101"))
 (div ((class . "thing"))
  "Foo"
  (div nil
   "Yes")))
```

Function: **dom-node** tag &optional attributes &rest children [¶](#index-dom_002dnode)
:   This function creates a DOM node of type tag. If
    given, attributes should be a key/value pair list.
    If given, children should be DOM nodes.

The following functions can be used to work with this structure. Each
function takes a DOM node, or a list of nodes. In the
latter case, only the first node in the list is used.

Simple accessors:

`dom-tag node`
:   Return the *tag* (also called “node name”) of the node.

`dom-attr node attribute`
:   Return the value of attribute in the node. A common usage
    would be:

    ```
    (dom-attr img 'href)
    => "https://fsf.org/logo.png"
    ```

`dom-children node`
:   Return all the children of the node.

`dom-non-text-children node`
:   Return all the non-string children of the node.

`dom-attributes node`
:   Return the key/value pair list of attributes of the node.

`dom-inner-text node`
:   Return all the textual elements of the node, as well as the textual
    elements of all the children of the node, recursively, as a
    concatenated string.

`dom-parent dom node`
:   Return the parent of node in dom.

`dom-remove dom node`
:   Remove node from dom.

The following are functions for altering the DOM.

`dom-set-attribute node attribute value`
:   Set the attribute of the node to value.

`dom-remove-attribute node attribute`
:   Remove attribute from node.

`dom-append-child node child`
:   Append child as the last child of node.

`dom-add-child-before node child before`
:   Add child to node’s child list before the before
    node. If before is `nil`, make child the first child.

`dom-set-attributes node attributes`
:   Replace all the attributes of the node with a new key/value list.

The following are functions for searching for elements in the
DOM. They all return lists of matching nodes.

`dom-by-tag dom tag`
:   Return all nodes in dom that are of type tag. A typical
    use would be:

    ```
    (dom-by-tag dom 'td)
    => '((td ...) (td ...) (td ...))
    ```

`dom-by-class dom match`
:   Return all nodes in dom that have class names that match
    match, which is a regular expression.

`dom-by-style dom style`
:   Return all nodes in dom that have styles that match match,
    which is a regular expression.

`dom-by-id dom style`
:   Return all nodes in dom that have IDs that match match,
    which is a regular expression.

`dom-search dom predicate`
:   Return all nodes in dom where predicate returns a
    non-`nil` value. predicate is called with the node to be
    tested as its parameter.

`dom-strings dom`
:   Return all strings in dom.

Utility functions:

`dom-pp dom &optional remove-empty`
:   Pretty-print dom at point. If remove-empty, don’t print
    textual nodes that just contain white-space.

`dom-print dom &optional pretty xml`
:   Print dom at point. If xml is non-`nil`, print as
    XML; otherwise, print as HTML. If pretty is
    non-`nil`, indent the HTML/XML logically.

---

Next: [JSONRPC communication](#JSONRPC), Previous: [Parsing HTML and XML](#Parsing-HTML_002fXML), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.31 Parsing and generating JSON values [¶](#Parsing-and-generating-JSON-values)

The Emacs JSON (*JavaScript Object Notation*) support
provides several functions to convert between Lisp objects and JSON
values. Any JSON value can be converted to a Lisp object, but not vice
versa. Specifically:

* JSON uses three keywords: `true`, `null`, `false`.
  `true` is represented by the symbol `t`. By default, the
  remaining two are represented, respectively, by the symbols
  `:null` and `:false`.
* JSON only has floating-point numbers. They can represent both Lisp
  integers and Lisp floating-point numbers.
* JSON strings are always Unicode strings encoded in UTF-8. Lisp
  strings can contain non-Unicode characters.
* JSON has only one sequence type, the array. JSON arrays are
  represented using Lisp vectors.
* JSON has only one map type, the object. JSON objects are represented
  using Lisp hashtables, alists or plists. When an alist or plist
  contains several elements with the same key, Emacs uses only the first
  element for serialization, in accordance with the behavior of
  `assq`.

Note that `nil`, being both a valid alist and a valid plist,
represents `{}`, the empty JSON object; not `null`,
`false`, or an empty array, all of which are different JSON
values.

If some Lisp object can’t be represented in JSON, the serialization
functions will signal an error of type `wrong-type-argument`.
The parsing functions can also signal the following errors:

`json-unavailable`
:   Signaled when the parsing library isn’t available.

`json-end-of-file`
:   Signaled when encountering a premature end of the input text.

`json-trailing-content`
:   Signaled when encountering unexpected input after the first JSON
    object parsed.

`json-parse-error`
:   Signaled when encountering invalid JSON syntax.

Top-level values and the subobjects within these top-level values
can be serialized to JSON. Likewise, the parsing functions will
return any of the possible types described above.

Function: **json-serialize** object &rest args [¶](#index-json_002dserialize)
:   This function returns a new Lisp unibyte string which contains the JSON
    representation of object. The argument args is a list of
    keyword/argument pairs. The following keywords are accepted:

    `:null-object`
    :   The value decides which Lisp object to use to represent the JSON
        keyword `null`. It defaults to the symbol `:null`.

    `:false-object`
    :   The value decides which Lisp object to use to represent the JSON
        keyword `false`. It defaults to the symbol `:false`.

Function: **json-insert** object &rest args [¶](#index-json_002dinsert)
:   This function inserts the JSON representation of object into the
    current buffer before point. The argument args are interpreted
    as in `json-serialize`.

Function: **json-parse-string** string &rest args [¶](#index-json_002dparse_002dstring)
:   This function parses the JSON value in string, which must be a
    Lisp string. If string doesn’t contain a valid JSON object,
    this function signals the `json-parse-error` error.

    The argument args is a list of keyword/argument pairs. The
    following keywords are accepted:

    `:object-type`
    :   The value decides which Lisp object to use for representing the
        key-value mappings of a JSON object. It can be either
        `hash-table`, the default, to make hashtables with strings as
        keys; `alist` to use alists with symbols as keys; or `plist`
        to use plists with keyword symbols as keys.

    `:array-type`
    :   The value decides which Lisp object to use for representing a JSON
        array. It can be either `array`, the default, to use Lisp
        arrays; or `list` to use lists.

    `:null-object`
    :   The value decides which Lisp object to use to represent the JSON
        keyword `null`. It defaults to the symbol `:null`.

    `:false-object`
    :   The value decides which Lisp object to use to represent the JSON
        keyword `false`. It defaults to the symbol `:false`.

Function: **json-parse-buffer** &rest args [¶](#index-json_002dparse_002dbuffer)
:   This function reads the next JSON value from the current buffer,
    starting at point. It moves point to the position immediately after
    the value if contains a valid JSON object; otherwise it signals the
    `json-parse-error` error and doesn’t move point. The arguments
    args are interpreted as in `json-parse-string`.

---

Next: [Atomic Change Groups](#Atomic-Changes), Previous: [Parsing and generating JSON values](#Parsing-JSON), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.32 JSONRPC communication [¶](#JSONRPC-communication)

The `jsonrpc` library implements the JSONRPC
specification, version 2.0, as it is described in
<https://www.jsonrpc.org/>. As the name suggests, JSONRPC is a
generic *Remote Procedure Call* protocol designed around
JSON objects, which you can convert to and from Lisp objects
(see [Parsing and generating JSON values](#Parsing-JSON)).

* [Overview](#JSONRPC-Overview)
* [Process-based JSONRPC connections](#Process_002dbased-JSONRPC-connections)
* [JSONRPC JSON object format](#JSONRPC-JSON-object-format)
* [Deferred JSONRPC requests](#JSONRPC-deferred-requests)

---

Next: [Process-based JSONRPC connections](#Process_002dbased-JSONRPC-connections), Up: [JSONRPC communication](#JSONRPC)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.32.1 Overview [¶](#Overview-1)

Quoting from the [spec](https://www.jsonrpc.org/), JSONRPC "is
transport agnostic in that the concepts can be used within the same
process, over sockets, over http, or in many various message passing
environments."

To model this agnosticism, the `jsonrpc` library uses objects of
a `jsonrpc-connection` class, which represent a connection to a
remote JSON endpoint (for details on Emacs’s object system,
see [EIEIO](https://www.gnu.org/software/emacs/manual/html_mono/eieio.html#Top) in EIEIO). In modern object-oriented parlance,
this class is “abstract”, i.e. the actual class of a useful
connection object is always a subclass of `jsonrpc-connection`.
Nevertheless, we can define two distinct APIs around the
`jsonrpc-connection` class:

1. An API for building JSONRPC applications

   In this scenario, a new aspiring JSONRPC-based application selects a
   concrete subclass of `jsonrpc-connection` that provides the
   transport for the JSONRPC messages to be exchanged between endpoints.

   The application creates objects of that subclass using
   `make-instance`. To initiate a contact to a remote endpoint, the
   application passes this object to the functions such as
   `jsonrpc-notify`, `jsonrpc-request`, or
   `jsonrpc-async-request`.

   For handling remotely initiated contacts, which generally come in
   asynchronously, the `make-instance` instantiation should
   initialize it the `:request-dispatcher` and
   `:notification-dispatcher` EIEIO keyword arguments. These are
   both functions of 3 arguments: the connection object; a symbol naming
   the JSONRPC method invoked remotely; and a JSONRPC `params`
   object.

   The function passed as `:request-dispatcher` is responsible for
   handling the remote endpoint’s requests, which expect a reply from the
   local endpoint (in this case, the application you’re building).
   Inside that function, you may either return locally (a regular return)
   or non-locally (throw an error). Both exits from the request
   dispatcher cause a reply to the remote endpoint’s request to be sent
   through the transport.

   A regular return determines a success response, and the return value
   must be a Lisp object that can be serialized as JSON (see [Parsing and generating JSON values](#Parsing-JSON)). The result is forwarded to the server as the JSONRPC
   `result` object. A non-local return, achieved by calling the
   function `jsonrpc-error`, causes an error response to be sent to
   the server. A non-local return triggered by an unexpected error of any
   other type also causes a response to be sent. The debugger is never
   called (unless you have set `debug-on-error`, in which case the
   Lisp debugger may be called, see [Entering the Debugger on an Error](#Error-Debugging)).

   The details of the accompanying JSONRPC `error` object are filled
   out automatically (in the case of unexpected errors) or with whatever
   was passed to `jsonrpc-error` (in the case of explicit calls).

   Exceptionally, an explicit call to `jsonrpc-error` which sets
   `:code` to 32000 and `:data` to any JSON object has the
   meaning of “no error” and triggers a normal response to the remote
   endpoint with `result` being set to `:data`. This is useful
   if the application wants to treat some non-local exits such as user
   quits as benign.

   It’s possible to use the `jsonrpc` library to build applications
   based on transport protocols that can be described as
   “quasi-JSONRPC”. These are similar, but not quite identical to
   JSONRPC, such as the [DAP (Debug
   Adapter Protocol)](https://www.jsonrpc.org/). These protocols also define request, response and
   notification messages but the format is not quite the same as JSONRPC.
   The generic functions `jsonrpc-convert-to-endpoint` and
   `jsonrpc-convert-from-endpoint` can be customized for converting
   between the internal representation of JSONRPC and whatever the
   endpoint accepts (see [Generic Functions](#Generic-Functions)).
2. An API for building JSONRPC transports

   In this scenario, `jsonrpc-connection` is sub-classed to implement
   a different underlying transport strategy (for details on how to
   subclass, see [Inheritance](https://www.gnu.org/software/emacs/manual/html_mono/eieio.html#Inheritance).). Users of the
   application-building interface can then instantiate objects of this
   concrete class (using the `make-instance` function) and connect
   to JSONRPC endpoints using that strategy. See [Process-based JSONRPC connections](#Process_002dbased-JSONRPC-connections), for a built-in transport implementation.

   This API has mandatory and optional parts.

   To allow its users to initiate JSONRPC contacts (notifications or
   requests) or reply to endpoint requests, the new transport
   implementation must equip the `jsonrpc-connection-send` generic
   function with a specialization for the new subclass
   (see [Generic Functions](#Generic-Functions)). This generic function is called
   automatically by primitives such as `jsonrpc-request` and
   `jsonrpc-notify`. The specialization should ensure that the
   message described in the argument list is sent through whatever
   underlying communication mechanism (a.k.a. “wire”) is used by the
   new transport to talk to endpoints. This “wire” may be a network
   socket, a serial interface, an HTTP connection, etc.

   Likewise, for handling the three types of remote contacts (requests,
   notifications, and responses to local requests), the transport
   implementation must arrange for the function
   `jsonrpc-connection-receive` to be called from Elisp after
   noticing some data on the “wire” that can be used to craft a JSONRPC
   (or quasi-JSONRPC) message.

   Finally, and optionally, the `jsonrpc-connection` subclass should
   add specializations to the `jsonrpc-shutdown` and
   `jsonrpc-running-p` generic functions if these concepts apply to
   the transport. The specialization of `jsonrpc-shutdown` should
   ensure the release of any system resources (e.g. processes, timers,
   etc.) used to listen for messages on the wire. The specialization of
   `jsonrpc-running-p` should tell if these resources are still
   active or have already been released (via `jsonrpc-shutdown` or
   otherwise).

---

Next: [JSONRPC JSON object format](#JSONRPC-JSON-object-format), Previous: [Overview](#JSONRPC-Overview), Up: [JSONRPC communication](#JSONRPC)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.32.2 Process-based JSONRPC connections [¶](#Process_002dbased-JSONRPC-connections-1)

For convenience, the `jsonrpc` library comes with a built-in
`jsonrpc-process-connection` transport implementation that can
talk to local subprocesses (using the standard input and standard
output); or TCP hosts (using sockets); or any other remote endpoint
that Emacs’s process object can represent (see [Processes](#Processes)).

Using this transport, the JSONRPC messages are encoded on the wire as
plain text and prefaced by some basic HTTP-style enveloping headers,
such as “Content-Length”.

For an example of an application using this transport scheme on top of
JSONRPC, see the
[Language Server Protocol](https://microsoft.github.io/language-server-protocol/specification).

Along with the mandatory `:request-dispatcher` and
`:notification-dispatcher` initargs, users of the
`jsonrpc-process-connection` class should pass the following
initargs as keyword-value pairs to `make-instance`:

`:process`
:   Value must be a live process object or a function of no arguments
    producing one such object. If passed a process object, the object is
    expected to contain a pre-established connection; otherwise, the
    function is called immediately after the object is made.

`:on-shutdown`
:   Value must be a function of a single argument, the
    `jsonrpc-process-connection` object. The function is called
    after the underlying process object has been deleted (either
    deliberately by `jsonrpc-shutdown`, or unexpectedly, because of
    some external cause).

---

Next: [Deferred JSONRPC requests](#JSONRPC-deferred-requests), Previous: [Process-based JSONRPC connections](#Process_002dbased-JSONRPC-connections), Up: [JSONRPC communication](#JSONRPC)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.32.3 JSONRPC JSON object format [¶](#JSONRPC-JSON-object-format-1)

JSONRPC JSON objects are exchanged as Lisp plists (see [Property Lists](#Property-Lists)): JSON-compatible plists are handed to the dispatcher functions
and, likewise, JSON-compatible plists should be given to
`jsonrpc-notify`, `jsonrpc-request`, and
`jsonrpc-async-request`.

To facilitate handling plists, this library makes liberal use of
`cl-lib` library (see [cl-lib](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Top) in Common Lisp Extensions
for GNU Emacs Lisp) and suggests (but doesn’t force) its clients to
do the same. A macro `jsonrpc-lambda` can be used to create a
lambda for destructuring a JSON-object like in this example:

```
(jsonrpc-async-request
 myproc :frobnicate `(:foo "trix")
 :success-fn (jsonrpc-lambda (&key bar baz &allow-other-keys)
               (message "Server replied back with %s and %s!"
                        bar baz))
 :error-fn (jsonrpc-lambda (&key code message _data)
             (message "Sadly, server reports %s: %s"
                      code message)))
```

---

Previous: [JSONRPC JSON object format](#JSONRPC-JSON-object-format), Up: [JSONRPC communication](#JSONRPC)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.32.4 Deferred JSONRPC requests [¶](#Deferred-JSONRPC-requests)

In many RPC situations, synchronization between the two
communicating endpoints is a matter of correctly designing the RPC
application: when synchronization is needed, requests (which are
blocking) should be used; when it isn’t, notifications should suffice.
However, when Emacs acts as one of these endpoints, asynchronous
events (e.g., timer- or process-related) may be triggered while there
is still uncertainty about the state of the remote endpoint.
Furthermore, acting on these events may only sometimes demand
synchronization, depending on the event’s specific nature.

The `:deferred` keyword argument to `jsonrpc-request` and
`jsonrpc-async-request` is designed to let the caller indicate
that the specific request needs synchronization and its actual
issuance may be delayed to the future, until some condition is
satisfied. Specifying `:deferred` for a request doesn’t mean it
*will* be delayed, only that it *can* be. If the request
isn’t sent immediately, `jsonrpc` will make renewed efforts to
send it at certain key times during communication, such as when
receiving or sending other messages to the endpoint.

Before any attempt to send the request, the application-specific
conditions are checked. Since the `jsonrpc` library can’t know
what these conditions are, the program can use the
`jsonrpc-connection-ready-p` generic function (see [Generic Functions](#Generic-Functions)) to specify them. The default method for this function
returns `t`, but you can add overriding methods that return
`nil` in some situations, based on the arguments passed to it,
which are the `jsonrpc-connection` object (see [Overview](#JSONRPC-Overview)) and whichever value you passed as the `:deferred`
keyword argument.

---

Next: [Change Hooks](#Change-Hooks), Previous: [JSONRPC communication](#JSONRPC), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 33.33 Atomic Change Groups [¶](#Atomic-Change-Groups)

In database terminology, an *atomic* change is an indivisible
change—it can succeed entirely or it can fail entirely, but it
cannot partly succeed. A Lisp program can make a series of changes to
one or several buffers as an *atomic change group*, meaning that
either the entire series of changes will be installed in their buffers
or, in case of an error, none of them will be.

To do this for one buffer, the one already current, simply write a
call to `atomic-change-group` around the code that makes the
changes, like this:

```
(atomic-change-group
  (insert foo)
  (delete-region x y))
```

If an error (or other nonlocal exit) occurs inside the body of
`atomic-change-group`, it unmakes all the changes in that buffer
that were during the execution of the body. This kind of change group
has no effect on any other buffers—any such changes remain.

If you need something more sophisticated, such as to make changes in
various buffers constitute one atomic group, you must directly call
lower-level functions that `atomic-change-group` uses.

Function: **prepare-change-group** &optional buffer [¶](#index-prepare_002dchange_002dgroup)
:   This function sets up a change group for buffer buffer, which
    defaults to the current buffer. It returns a handle that
    represents the change group. You must use this handle to activate the
    change group and subsequently to finish it.

To use the change group, you must *activate* it. You must do
this before making any changes in the text of buffer.

Function: **activate-change-group** handle [¶](#index-activate_002dchange_002dgroup)
:   This function activates the change group that handle designates.

After you activate the change group, any changes you make in that
buffer become part of it. Once you have made all the desired changes
in the buffer, you must *finish* the change group. There are two
ways to do this: you can either accept (and finalize) all the changes,
or cancel them all.

Function: **accept-change-group** handle [¶](#index-accept_002dchange_002dgroup)
:   This function accepts all the changes in the change group specified by
    handle, making them final.

Function: **cancel-change-group** handle [¶](#index-cancel_002dchange_002dgroup)
:   This function cancels and undoes all the changes in the change group
    specified by handle.

You can cause some or all of the changes in a change group to be
considered as a single unit for the purposes of the `undo`
commands (see [Undo](#Undo)) by using `undo-amalgamate-change-group`.

Function: **undo-amalgamate-change-group** [¶](#index-undo_002damalgamate_002dchange_002dgroup)
:   Amalgamate all the changes made in the change-group since the state
    identified by handle. This function removes all undo boundaries
    between undo records of changes since the state described by
    handle. Usually, handle is the handle returned by
    `prepare-change-group`, in which case all the changes since the
    beginning of the change-group are amalgamated into a single undo unit.

Your code should use `unwind-protect` to make sure the group is
always finished. The call to `activate-change-group` should be
inside the `unwind-protect`, in case the user types `C-g`
just after it runs. (This is one reason why
`prepare-change-group` and `activate-change-group` are
separate functions, because normally you would call
`prepare-change-group` before the start of that
`unwind-protect`.) Once you finish the group, don’t use the
handle again—in particular, don’t try to finish the same group
twice.

To make a multibuffer change group, call `prepare-change-group`
once for each buffer you want to cover, then use `nconc` to
combine the returned values, like this:

```
(nconc (prepare-change-group buffer-1)
       (prepare-change-group buffer-2))
```

You can then activate the multibuffer change group with a single call
to `activate-change-group`, and finish it with a single call to
`accept-change-group` or `cancel-change-group`.

Nested use of several change groups for the same buffer works as you
would expect. Non-nested use of change groups for the same buffer
will get Emacs confused, so don’t let it happen; the first change
group you start for any given buffer should be the last one finished.

Emacs keeps track of change groups by assuming that by following
each cdr in `buffer-undo-list`, it will eventually arrive at the
cons it was set to at the time `prepare-change-group` was called.

If `buffer-undo-list` no longer contains that cons, Emacs will
lose track of any change groups, resulting in an error when the change
group is canceled. To avoid this, do not call any functions which
may edit the undo list in such a manner, when a change group is
active: notably, “amalgamating” commands such as `delete-char`,
which call `undo-auto-amalgamate`.

---

Previous: [Atomic Change Groups](#Atomic-Changes), Up: [Text](#Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
