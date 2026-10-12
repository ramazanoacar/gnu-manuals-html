C Function: **scm\_string\_unfold** (p, f, g, seed, base, make\_final) [¶](#index-scm_005fstring_005funfold)
:   * g is used to generate a series of *seed*
      values from the initial seed: seed, (g
      seed), (g^2 seed), (g^3 seed),
      …
    * p tells us when to stop – when it returns true
      when applied to one of these seed values.
    * f maps each seed value to the corresponding
      character in the result string. These chars are assembled
      into the string in a left-to-right order.
    * base is the optional initial/leftmost portion
      of the constructed string; it default to the empty
      string.
    * make\_final is applied to the terminal seed
      value (on which p returns true) to produce
      the final/rightmost portion of the constructed string.
      The default is nothing extra.

Scheme Procedure: **string-unfold-right** p f g seed [base [make\_final]] [¶](#index-string_002dunfold_002dright)

C Function: **scm\_string\_unfold\_right** (p, f, g, seed, base, make\_final) [¶](#index-scm_005fstring_005funfold_005fright)
:   * g is used to generate a series of *seed*
      values from the initial seed: seed, (g
      seed), (g^2 seed), (g^3 seed),
      …
    * p tells us when to stop – when it returns true
      when applied to one of these seed values.
    * f maps each seed value to the corresponding
      character in the result string. These chars are assembled
      into the string in a right-to-left order.
    * base is the optional initial/rightmost portion
      of the constructed string; it default to the empty
      string.
    * make\_final is applied to the terminal seed
      value (on which p returns true) to produce
      the final/leftmost portion of the constructed string.
      It defaults to `(lambda (x) )`.

---

Next: [Representing Strings as Bytes](#Representing-Strings-as-Bytes), Previous: [Mapping, Folding, and Unfolding](#Mapping-Folding-and-Unfolding), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.12 Miscellaneous String Operations [¶](#Miscellaneous-String-Operations-1)

Scheme Procedure: **xsubstring** s from [to [start [end]]] [¶](#index-xsubstring)

C Function: **scm\_xsubstring** (s, from, to, start, end) [¶](#index-scm_005fxsubstring)
:   This is the *extended substring* procedure that implements
    replicated copying of a substring of some string.

    s is a string, start and end are optional
    arguments that demarcate a substring of s, defaulting to
    0 and the length of s. Replicate this substring up and
    down index space, in both the positive and negative directions.
    `xsubstring` returns the substring of this string
    beginning at index from, and ending at to, which
    defaults to from + (end - start).

Scheme Procedure: **string-xcopy!** target tstart s sfrom [sto [start [end]]] [¶](#index-string_002dxcopy_0021)

C Function: **scm\_string\_xcopy\_x** (target, tstart, s, sfrom, sto, start, end) [¶](#index-scm_005fstring_005fxcopy_005fx)
:   Exactly the same as `xsubstring`, but the extracted text
    is written into the string target starting at index
    tstart. The operation is not defined if `(eq?
    target s)` or these arguments share storage – you
    cannot copy a string on top of itself.

Scheme Procedure: **string-replace** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dreplace)

C Function: **scm\_string\_replace** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005freplace)
:   Return the string s1, but with the characters
    start1 … end1 replaced by the characters
    start2 … end2 from s2.

Scheme Procedure: **string-tokenize** s [token\_set [start [end]]] [¶](#index-string_002dtokenize)

C Function: **scm\_string\_tokenize** (s, token\_set, start, end) [¶](#index-scm_005fstring_005ftokenize)
:   Split the string s into a list of substrings, where each
    substring is a maximal non-empty contiguous sequence of
    characters from the character set token\_set, which
    defaults to `char-set:graphic`.
    If start or end indices are provided, they restrict
    `string-tokenize` to operating on the indicated substring
    of s.

Scheme Procedure: **string-filter** char\_pred s [start [end]] [¶](#index-string_002dfilter)

C Function: **scm\_string\_filter** (char\_pred, s, start, end) [¶](#index-scm_005fstring_005ffilter)
:   Filter the string s, retaining only those characters which
    satisfy char\_pred.

    If char\_pred is a procedure, it is applied to each character as
    a predicate, if it is a character, it is tested for equality and if it
    is a character set, it is tested for membership.

Scheme Procedure: **string-delete** char\_pred s [start [end]] [¶](#index-string_002ddelete)

C Function: **scm\_string\_delete** (char\_pred, s, start, end) [¶](#index-scm_005fstring_005fdelete)
:   Delete characters satisfying char\_pred from s.

    If char\_pred is a procedure, it is applied to each character as
    a predicate, if it is a character, it is tested for equality and if it
    is a character set, it is tested for membership.

The following additional functions are available in the module
`(ice-9 string-fun)`. They can be used with:

```
(use-modules (ice-9 string-fun))
```

Scheme Procedure: **string-replace-substring** str substring replacement [¶](#index-string_002dreplace_002dsubstring)
:   Return a new string where every instance of substring in string
    str has been replaced by replacement. For example:

    ```
    (string-replace-substring "a ring of strings" "ring" "rut")
    ⇒ "a rut of struts"
    ```

---

Next: [Conversion to/from C](#Conversion-to_002ffrom-C), Previous: [Miscellaneous String Operations](#Miscellaneous-String-Operations), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.13 Representing Strings as Bytes [¶](#Representing-Strings-as-Bytes-1)

In the cold world outside of Guile, not all strings are treated in
the same way. Out there there are only bytes, and there are many ways
of representing a strings (sequences of characters) as binary data
(sequences of bytes).

As a user, usually you don’t have to think about this very much. When
you type on your keyboard, your system encodes your keystrokes as bytes
according to the locale that you have configured on your computer.
Guile uses the locale to decode those bytes back into characters –
hopefully the same characters that you typed in.

All is not so clear when dealing with a system with multiple users, such
as a web server. Your web server might get a request from one user for
data encoded in the ISO-8859-1 character set, and then another request
from a different user for UTF-8 data.

Guile provides an *iconv* module for converting between strings and
sequences of bytes. See [Bytevectors](#Bytevectors), for more on how Guile
represents raw byte sequences. This module gets its name from the
common UNIX command of the same name.

Note that often it is sufficient to just read and write strings from
ports instead of using these functions. To do this, specify the port
encoding using `set-port-encoding!`. See [Ports](#Ports), for more on
ports and character encodings.

Unlike the rest of the procedures in this section, you have to load the
`iconv` module before having access to these procedures:

```
(use-modules (ice-9 iconv))
```

Scheme Procedure: **string->bytevector** string encoding [conversion-strategy] [¶](#index-string_002d_003ebytevector)
:   Encode string as a sequence of bytes.

    The string will be encoded in the character set specified by the
    encoding string. If the string has characters that cannot be
    represented in the encoding, by default this procedure raises an
    `encoding-error`. Pass a conversion-strategy argument to
    specify other behaviors.

    The return value is a bytevector. See [Bytevectors](#Bytevectors), for more on
    bytevectors. See [Ports](#Ports), for more on character encodings and
    conversion strategies.

Scheme Procedure: **bytevector->string** bytevector encoding [conversion-strategy] [¶](#index-bytevector_002d_003estring)
:   Decode bytevector into a string.

    The bytes will be decoded from the character set by the encoding
    string. If the bytes do not form a valid encoding, by default this
    procedure raises an `decoding-error`. As with
    `string->bytevector`, pass the optional conversion-strategy
    argument to modify this behavior. See [Ports](#Ports), for more on character
    encodings and conversion strategies.

Scheme Procedure: **call-with-output-encoded-string** encoding proc [conversion-strategy] [¶](#index-call_002dwith_002doutput_002dencoded_002dstring)
:   Like `call-with-output-string`, but instead of returning a string,
    returns a encoding of the string according to encoding, as a
    bytevector. This procedure can be more efficient than collecting a
    string and then converting it via `string->bytevector`.

---

Next: [String Internals](#String-Internals), Previous: [Representing Strings as Bytes](#Representing-Strings-as-Bytes), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.14 Conversion to/from C [¶](#Conversion-to_002ffrom-C-1)

When creating a Scheme string from a C string or when converting a
Scheme string to a C string, the concept of character encoding becomes
important.

In C, a string is just a sequence of bytes, and the character encoding
describes the relation between these bytes and the actual characters
that make up the string. For Scheme strings, character encoding is not
an issue (most of the time), since in Scheme you usually treat strings
as character sequences, not byte sequences.

Converting to C and converting from C each have their own challenges.

When converting from C to Scheme, it is important that the sequence of
bytes in the C string be valid with respect to its encoding. ASCII
strings, for example, can’t have any bytes greater than 127. An ASCII
byte greater than 127 is considered *ill-formed* and cannot be
converted into a Scheme character.

Problems can occur in the reverse operation as well. Not all character
encodings can hold all possible Scheme characters. Some encodings, like
ASCII for example, can only describe a small subset of all possible
characters. So, when converting to C, one must first decide what to do
with Scheme characters that can’t be represented in the C string.

Converting a Scheme string to a C string will often allocate fresh
memory to hold the result. You must take care that this memory is
properly freed eventually. In many cases, this can be achieved by
using `scm_dynwind_free` inside an appropriate dynwind context,
See [Dynamic Wind](#Dynamic-Wind).

C Function: `SCM` **scm\_from\_locale\_string** `(const char *str)` [¶](#index-scm_005ffrom_005flocale_005fstring)

C Function: `SCM` **scm\_from\_locale\_stringn** `(const char *str, size_t len)` [¶](#index-scm_005ffrom_005flocale_005fstringn)
:   Creates a new Scheme string that has the same contents as str when
    interpreted in the character encoding of the current locale.

    For `scm_from_locale_string`, str must be null-terminated.

    For `scm_from_locale_stringn`, len specifies the length of
    str in bytes, and str does not need to be null-terminated.
    If len is `(size_t)-1`, then str does need to be
    null-terminated and the real length will be found with `strlen`.

    If the C string is ill-formed, an error will be raised.

    Note that these functions should *not* be used to convert C string
    constants, because there is no guarantee that the current locale will
    match that of the execution character set, used for string and character
    constants. Most modern C compilers use UTF-8 by default, so to convert
    C string constants we recommend `scm_from_utf8_string`.

C Function: `SCM` **scm\_take\_locale\_string** `(char *str)` [¶](#index-scm_005ftake_005flocale_005fstring)

C Function: `SCM` **scm\_take\_locale\_stringn** `(char *str, size_t len)` [¶](#index-scm_005ftake_005flocale_005fstringn)
:   Like `scm_from_locale_string` and `scm_from_locale_stringn`,
    respectively, but also frees str with `free` eventually.
    Thus, you can use this function when you would free str anyway
    immediately after creating the Scheme string. In certain cases, Guile
    can then use str directly as its internal representation.

C Function: `char *` **scm\_to\_locale\_string** `(SCM str)` [¶](#index-scm_005fto_005flocale_005fstring)

C Function: `char *` **scm\_to\_locale\_stringn** `(SCM str, size_t *lenp)` [¶](#index-scm_005fto_005flocale_005fstringn)
:   Returns a C string with the same contents as str in the character
    encoding of the current locale. The C string must be freed with
    `free` eventually, maybe by using `scm_dynwind_free`,
    See [Dynamic Wind](#Dynamic-Wind).

    For `scm_to_locale_string`, the returned string is
    null-terminated and an error is signaled when str contains
    `#\nul` characters.

    For `scm_to_locale_stringn` and lenp not `NULL`,
    str might contain `#\nul` characters and the length of the
    returned string in bytes is stored in `*lenp`. The
    returned string will not be null-terminated in this case. If
    lenp is `NULL`, `scm_to_locale_stringn` behaves like
    `scm_to_locale_string`.

    If a character in str cannot be represented in the character
    encoding of the current locale, the default port conversion strategy is
    used. See [Ports](#Ports), for more on conversion strategies.

    If the conversion strategy is `error`, an error will be raised. If
    it is `substitute`, a replacement character, such as a question
    mark, will be inserted in its place. If it is `escape`, a hex
    escape will be inserted in its place.

C Function: `size_t` **scm\_to\_locale\_stringbuf** `(SCM str, char *buf, size_t max_len)` [¶](#index-scm_005fto_005flocale_005fstringbuf)
:   Puts str as a C string in the current locale encoding into the
    memory pointed to by buf. The buffer at buf has room for
    max\_len bytes and `scm_to_local_stringbuf` will never store
    more than that. No terminating `'\0'` will be stored.

    The return value of `scm_to_locale_stringbuf` is the number of
    bytes that are needed for all of str, regardless of whether
    buf was large enough to hold them. Thus, when the return value
    is larger than max\_len, only max\_len bytes have been
    stored and you probably need to try again with a larger buffer.

For most situations, string conversion should occur using the current
locale, such as with the functions above. But there may be cases where
one wants to convert strings from a character encoding other than the
locale’s character encoding. For these cases, the lower-level functions
`scm_to_stringn` and `scm_from_stringn` are provided. These
functions should seldom be necessary if one is properly using locales.

C Type: **scm\_t\_string\_failed\_conversion\_handler** [¶](#index-scm_005ft_005fstring_005ffailed_005fconversion_005fhandler)
:   This is an enumerated type that can take one of three values:
    `SCM_FAILED_CONVERSION_ERROR`,
    `SCM_FAILED_CONVERSION_QUESTION_MARK`, and
    `SCM_FAILED_CONVERSION_ESCAPE_SEQUENCE`. They are used to indicate
    a strategy for handling characters that cannot be converted to or from a
    given character encoding. `SCM_FAILED_CONVERSION_ERROR` indicates
    that a conversion should throw an error if some characters cannot be
    converted. `SCM_FAILED_CONVERSION_QUESTION_MARK` indicates that a
    conversion should replace unconvertable characters with the question
    mark character. And, `SCM_FAILED_CONVERSION_ESCAPE_SEQUENCE`
    requests that a conversion should replace an unconvertable character
    with an escape sequence.

    While all three strategies apply when converting Scheme strings to C,
    only `SCM_FAILED_CONVERSION_ERROR` and
    `SCM_FAILED_CONVERSION_QUESTION_MARK` can be used when converting C
    strings to Scheme.

C Function: `char` **\*scm\_to\_stringn** `(SCM str, size_t *lenp, const char *encoding, scm_t_string_failed_conversion_handler handler)` [¶](#index-_002ascm_005fto_005fstringn)
:   This function returns a newly allocated C string from the Guile string
    str. The length of the returned string in bytes will be returned in
    lenp. The character encoding of the C string is passed as the ASCII,
    null-terminated C string encoding. The handler parameter
    gives a strategy for dealing with characters that cannot be converted
    into encoding.

    If lenp is `NULL`, this function will return a null-terminated C
    string. It will throw an error if the string contains a null
    character.

    The Scheme interface to this function is `string->bytevector`, from the
    `ice-9 iconv` module. See [Representing Strings as Bytes](#Representing-Strings-as-Bytes).

C Function: `SCM` **scm\_from\_stringn** `(const char *str, size_t len, const char *encoding, scm_t_string_failed_conversion_handler handler)` [¶](#index-scm_005ffrom_005fstringn)
:   This function returns a scheme string from the C string str. The
    length in bytes of the C string is input as len. The encoding of the C
    string is passed as the ASCII, null-terminated C string `encoding`.
    The handler parameters suggests a strategy for dealing with
    unconvertable characters.

    The Scheme interface to this function is `bytevector->string`.
    See [Representing Strings as Bytes](#Representing-Strings-as-Bytes).

The following conversion functions are provided as a convenience for the
most commonly used encodings.

C Function: `SCM` **scm\_from\_latin1\_string** `(const char *str)` [¶](#index-scm_005ffrom_005flatin1_005fstring)

C Function: `SCM` **scm\_from\_utf8\_string** `(const char *str)` [¶](#index-scm_005ffrom_005futf8_005fstring)

C Function: `SCM` **scm\_from\_utf32\_string** `(const scm_t_wchar *str)` [¶](#index-scm_005ffrom_005futf32_005fstring)
:   Return a scheme string from the null-terminated C string str,
    which is ISO-8859-1-, UTF-8-, or UTF-32-encoded. These functions should
    be used to convert hard-coded C string constants into Scheme strings.

C Function: `SCM` **scm\_from\_latin1\_stringn** `(const char *str, size_t len)` [¶](#index-scm_005ffrom_005flatin1_005fstringn)

C Function: `SCM` **scm\_from\_utf8\_stringn** `(const char *str, size_t len)` [¶](#index-scm_005ffrom_005futf8_005fstringn)

C Function: `SCM` **scm\_from\_utf32\_stringn** `(const scm_t_wchar *str, size_t len)` [¶](#index-scm_005ffrom_005futf32_005fstringn)
:   Return a scheme string from C string str, which is ISO-8859-1-,
    UTF-8-, or UTF-32-encoded, of length len. len is the number
    of bytes pointed to by str for `scm_from_latin1_stringn` and
    `scm_from_utf8_stringn`; it is the number of elements (code points)
    in str in the case of `scm_from_utf32_stringn`.

C function: `char` **\*scm\_to\_latin1\_stringn** `(SCM str, size_t *lenp)` [¶](#index-_002ascm_005fto_005flatin1_005fstringn)

C function: `char` **\*scm\_to\_utf8\_stringn** `(SCM str, size_t *lenp)` [¶](#index-_002ascm_005fto_005futf8_005fstringn)

C function: `scm_t_wchar` **\*scm\_to\_utf32\_stringn** `(SCM str, size_t *lenp)` [¶](#index-_002ascm_005fto_005futf32_005fstringn)
:   Return a newly allocated, ISO-8859-1-, UTF-8-, or UTF-32-encoded C string
    from Scheme string str. An error is thrown when str
    cannot be converted to the specified encoding. If lenp is
    `NULL`, the returned C string will be null terminated, and an error
    will be thrown if the C string would otherwise contain null
    characters. If lenp is not `NULL`, the string is not null terminated,
    and the length of the returned string is returned in lenp. The length
    returned is the number of bytes for `scm_to_latin1_stringn` and
    `scm_to_utf8_stringn`; it is the number of elements (code points)
    for `scm_to_utf32_stringn`.

It is not often the case, but sometimes when you are dealing with the
implementation details of a port, you need to encode and decode strings
according to the encoding and conversion strategy of the port. There
are some convenience functions for that purpose as well.

C Function: `SCM` **scm\_from\_port\_string** `(const char *str, SCM port)` [¶](#index-scm_005ffrom_005fport_005fstring)

C Function: `SCM` **scm\_from\_port\_stringn** `(const char *str, size_t len, SCM port)` [¶](#index-scm_005ffrom_005fport_005fstringn)

C Function: `char*` **scm\_to\_port\_string** `(SCM str, SCM port)` [¶](#index-scm_005fto_005fport_005fstring)

C Function: `char*` **scm\_to\_port\_stringn** `(SCM str, size_t *lenp, SCM port)` [¶](#index-scm_005fto_005fport_005fstringn)
:   Like `scm_from_stringn` and friends, except they take their
    encoding and conversion strategy from a given port object.

---

Previous: [Conversion to/from C](#Conversion-to_002ffrom-C), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.15 String Internals [¶](#String-Internals-1)

Guile stores each string in memory as a contiguous array of Unicode code
points along with an associated set of attributes. If all of the code
points of a string have an integer range between 0 and 255 inclusive,
the code point array is stored as one byte per code point: it is stored
as an ISO-8859-1 (aka Latin-1) string. If any of the code points of the
string has an integer value greater that 255, the code point array is
stored as four bytes per code point: it is stored as a UTF-32 string.

Conversion between the one-byte-per-code-point and
four-bytes-per-code-point representations happens automatically as
necessary.

No API is provided to set the internal representation of strings;
however, there are pair of procedures available to query it. These are
debugging procedures. Using them in production code is discouraged,
since the details of Guile’s internal representation of strings may
change from release to release.

Scheme Procedure: **string-bytes-per-char** str [¶](#index-string_002dbytes_002dper_002dchar)

C Function: **scm\_string\_bytes\_per\_char** (str) [¶](#index-scm_005fstring_005fbytes_005fper_005fchar)
:   Return the number of bytes used to encode a Unicode code point in string
    str. The result is one or four.

Scheme Procedure: **%string-dump** str [¶](#index-_0025string_002ddump)

C Function: **scm\_sys\_string\_dump** (str) [¶](#index-scm_005fsys_005fstring_005fdump)
:   Returns an association list containing debugging information for
    str. The association list has the following entries.

    `string`
    :   The string itself.

    `start`
    :   The start index of the string into its stringbuf

    `length`
    :   The length of the string

    `shared`
    :   If this string is a substring, it returns its
        parent string. Otherwise, it returns `#f`

    `read-only`
    :   `#t` if the string is read-only

    `stringbuf-chars`
    :   A new string containing this string’s stringbuf’s characters

    `stringbuf-length`
    :   The number of characters in this stringbuf

    `stringbuf-shared`
    :   `#t` if this stringbuf is shared

    `stringbuf-wide`
    :   `#t` if this stringbuf’s characters are stored in a 32-bit buffer,
        or `#f` if they are stored in an 8-bit buffer

---

Next: [Keywords](#Keywords), Previous: [Strings](#Strings), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.6 Symbols [¶](#Symbols-1)

Symbols in Scheme are widely used in three ways: as items of discrete
data, as lookup keys for alists and hash tables, and to denote variable
references.

A *symbol* is similar to a string in that it is defined by a
sequence of characters. The sequence of characters is known as the
symbol’s *name*. In the usual case — that is, where the symbol’s
name doesn’t include any characters that could be confused with other
elements of Scheme syntax — a symbol is written in a Scheme program by
writing the sequence of characters that make up the name, *without*
any quotation marks or other special syntax. For example, the symbol
whose name is “multiply-by-2” is written, simply:

```
multiply-by-2
```

Notice how this differs from a *string* with contents
“multiply-by-2”, which is written with double quotation marks, like
this:

```
"multiply-by-2"
```

Looking beyond how they are written, symbols are different from strings
in two important respects.

The first important difference is uniqueness. If the same-looking
string is read twice from two different places in a program, the result
is two *different* string objects whose contents just happen to be
the same. If, on the other hand, the same-looking symbol is read twice
from two different places in a program, the result is the *same*
symbol object both times.

Given two read symbols, you can use `eq?` to test whether they are
the same (that is, have the same name). `eq?` is the most
efficient comparison operator in Scheme, and comparing two symbols like
this is as fast as comparing, for example, two numbers. Given two
strings, on the other hand, you must use `equal?` or
`string=?`, which are much slower comparison operators, to
determine whether the strings have the same contents.

```
(define sym1 (quote hello))
(define sym2 (quote hello))
(eq? sym1 sym2) ⇒ #t

(define str1 "hello")
(define str2 "hello")
(eq? str1 str2) ⇒ #f
(equal? str1 str2) ⇒ #t
```

The second important difference is that symbols, unlike strings, are not
self-evaluating. This is why we need the `(quote …)`s in the
example above: `(quote hello)` evaluates to the symbol named
"hello" itself, whereas an unquoted `hello` is *read* as the
symbol named "hello" and evaluated as a variable reference … about
which more below (see [Symbols as Denoting Variables](#Symbol-Variables)).

* [Symbols as Discrete Data](#Symbol-Data)
* [Symbols as Lookup Keys](#Symbol-Keys)
* [Symbols as Denoting Variables](#Symbol-Variables)
* [Operations Related to Symbols](#Symbol-Primitives)
* [Extended Read Syntax for Symbols](#Symbol-Read-Syntax)
* [Uninterned Symbols](#Symbol-Uninterned)

---

Next: [Symbols as Lookup Keys](#Symbol-Keys), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.6.1 Symbols as Discrete Data [¶](#Symbols-as-Discrete-Data)

Numbers and symbols are similar to the extent that they both lend
themselves to `eq?` comparison. But symbols are more descriptive
than numbers, because a symbol’s name can be used directly to describe
the concept for which that symbol stands.

For example, imagine that you need to represent some colors in a
computer program. Using numbers, you would have to choose arbitrarily
some mapping between numbers and colors, and then take care to use that
mapping consistently:

```
;; 1=red, 2=green, 3=purple

(if (eq? (color-of vehicle) 1)
    ...)
```

You can make the mapping more explicit and the code more readable by
defining constants:

```
(define red 1)
(define green 2)
(define purple 3)

(if (eq? (color-of vehicle) red)
    ...)
```

But the simplest and clearest approach is not to use numbers at all, but
symbols whose names specify the colors that they refer to:

```
(if (eq? (color-of vehicle) 'red)
    ...)
```

The descriptive advantages of symbols over numbers increase as the set
of concepts that you want to describe grows. Suppose that a car object
can have other properties as well, such as whether it has or uses:

* automatic or manual transmission
* leaded or unleaded fuel
* power steering (or not).

Then a car’s combined property set could be naturally represented and
manipulated as a list of symbols:

```
(properties-of vehicle1)
⇒
(red manual unleaded power-steering)

(if (memq 'power-steering (properties-of vehicle1))
    (display "Unfit people can drive this vehicle.\n")
    (display "You'll need strong arms to drive this vehicle!\n"))
⊣
Unfit people can drive this vehicle.
```

Remember, the fundamental property of symbols that we are relying on
here is that an occurrence of `'red` in one part of a program is an
*indistinguishable* symbol from an occurrence of `'red` in
another part of a program; this means that symbols can usefully be
compared using `eq?`. At the same time, symbols have naturally
descriptive names. This combination of efficiency and descriptive power
makes them ideal for use as discrete data.

---

Next: [Symbols as Denoting Variables](#Symbol-Variables), Previous: [Symbols as Discrete Data](#Symbol-Data), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.6.2 Symbols as Lookup Keys [¶](#Symbols-as-Lookup-Keys)

Given their efficiency and descriptive power, it is natural to use
symbols as the keys in an association list or hash table.

To illustrate this, consider a more structured representation of the car
properties example from the preceding subsection. Rather than
mixing all the properties up together in a flat list, we could use an
association list like this:

```
(define car1-properties '((color . red)
                          (transmission . manual)
                          (fuel . unleaded)
                          (steering . power-assisted)))
```

Notice how this structure is more explicit and extensible than the flat
list. For example it makes clear that `manual` refers to the
transmission rather than, say, the windows or the locking of the car.
It also allows further properties to use the same symbols among their
possible values without becoming ambiguous:

```
(define car1-properties '((color . red)
                          (transmission . manual)
                          (fuel . unleaded)
                          (steering . power-assisted)
                          (seat-color . red)
                          (locking . manual)))
```

With a representation like this, it is easy to use the efficient
`assq-XXX` family of procedures (see [Association Lists](#Association-Lists)) to
extract or change individual pieces of information:

```
(assq-ref car1-properties 'fuel) ⇒ unleaded
(assq-ref car1-properties 'transmission) ⇒ manual

(assq-set! car1-properties 'seat-color 'black)
⇒
((color . red)
 (transmission . manual)
 (fuel . unleaded)
 (steering . power-assisted)
 (seat-color . black)
 (locking . manual)))
```

Hash tables also have keys, and exactly the same arguments apply to the
use of symbols in hash tables as in association lists. The hash value
that Guile uses to decide where to add a symbol-keyed entry to a hash
table can be obtained by calling the `symbol-hash` procedure:

Scheme Procedure: **symbol-hash** symbol [¶](#index-symbol_002dhash)

C Function: **scm\_symbol\_hash** (symbol) [¶](#index-scm_005fsymbol_005fhash)
:   Return a hash value for symbol.

See [Hash Tables](#Hash-Tables) for information about hash tables in general, and
for why you might choose to use a hash table rather than an association
list.

---

Next: [Operations Related to Symbols](#Symbol-Primitives), Previous: [Symbols as Lookup Keys](#Symbol-Keys), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.6.3 Symbols as Denoting Variables [¶](#Symbols-as-Denoting-Variables)

When an unquoted symbol in a Scheme program is evaluated, it is
interpreted as a variable reference, and the result of the evaluation is
the appropriate variable’s value.

For example, when the expression `(string-length "abcd")` is read
and evaluated, the sequence of characters `string-length` is read
as the symbol whose name is "string-length". This symbol is associated
with a variable whose value is the procedure that implements string
length calculation. Therefore evaluation of the `string-length`
symbol results in that procedure.

The details of the connection between an unquoted symbol and the
variable to which it refers are explained elsewhere. See [Definitions and Variable Bindings](#Binding-Constructs), for how associations between symbols and variables are
created, and [Modules](#Modules), for how those associations are affected by
Guile’s module system.

---

Next: [Extended Read Syntax for Symbols](#Symbol-Read-Syntax), Previous: [Symbols as Denoting Variables](#Symbol-Variables), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.6.4 Operations Related to Symbols [¶](#Operations-Related-to-Symbols)

Given any Scheme value, you can determine whether it is a symbol using
the `symbol?` primitive:

Scheme Procedure: **symbol?** obj [¶](#index-symbol_003f)

C Function: **scm\_symbol\_p** (obj) [¶](#index-scm_005fsymbol_005fp)
:   Return `#t` if obj is a symbol, otherwise return
    `#f`.

C Function: `int` **scm\_is\_symbol** `(SCM val)` [¶](#index-scm_005fis_005fsymbol)
:   Equivalent to `scm_is_true (scm_symbol_p (val))`.

Once you know that you have a symbol, you can obtain its name as a
string by calling `symbol->string`. Note that Guile differs by
default from R5RS on the details of `symbol->string` as regards
case-sensitivity:

Scheme Procedure: **symbol->string** s [¶](#index-symbol_002d_003estring)

C Function: **scm\_symbol\_to\_string** (s) [¶](#index-scm_005fsymbol_005fto_005fstring)
:   Return the name of symbol s as a string. By default, Guile reads
    symbols case-sensitively, so the string returned will have the same case
    variation as the sequence of characters that caused s to be
    created.

    If Guile is set to read symbols case-insensitively (as specified by
    R5RS), and s comes into being as part of a literal expression
    (see [Literal expressions](/r5rs/latest/en/r5rs.html#Literal-expressions) in The Revised^5 Report on Scheme) or
    by a call to the `read` or `string-ci->symbol` procedures,
    Guile converts any alphabetic characters in the symbol’s name to
    lower case before creating the symbol object, so the string returned
    here will be in lower case.

    If s was created by `string->symbol`, the case of characters
    in the string returned will be the same as that in the string that was
    passed to `string->symbol`, regardless of Guile’s case-sensitivity
    setting at the time s was created.

    It is an error to apply mutation procedures like `string-set!` to
    strings returned by this procedure.

Most symbols are created by writing them literally in code. However it
is also possible to create symbols programmatically using the following
procedures:

Scheme Procedure: **symbol** char… [¶](#index-symbol)
:   Return a newly allocated symbol made from the given character arguments.

    ```
    (symbol #\x #\y #\z) ⇒ xyz
    ```

Scheme Procedure: **list->symbol** lst [¶](#index-list_002d_003esymbol)
:   Return a newly allocated symbol made from a list of characters.

    ```
    (list->symbol '(#\a #\b #\c)) ⇒ abc
    ```

Scheme Procedure: **symbol-append** arg … [¶](#index-symbol_002dappend)
:   Return a newly allocated symbol whose characters form the
    concatenation of the given symbols, arg ....

    ```
    (let ((h 'hello))
      (symbol-append h 'world))
    ⇒ helloworld
    ```

Scheme Procedure: **string->symbol** string [¶](#index-string_002d_003esymbol)

C Function: **scm\_string\_to\_symbol** (string) [¶](#index-scm_005fstring_005fto_005fsymbol)
:   Return the symbol whose name is string. This procedure can create
    symbols with names containing special characters or letters in the
    non-standard case, but it is usually a bad idea to create such symbols
    because in some implementations of Scheme they cannot be read as
    themselves.

Scheme Procedure: **string-ci->symbol** str [¶](#index-string_002dci_002d_003esymbol)

C Function: **scm\_string\_ci\_to\_symbol** (str) [¶](#index-scm_005fstring_005fci_005fto_005fsymbol)
:   Return the symbol whose name is str. If Guile is currently
    reading symbols case-insensitively, str is converted to lowercase
    before the returned symbol is looked up or created.

The following examples illustrate Guile’s detailed behavior as regards
the case-sensitivity of symbols:

```
(read-enable 'case-insensitive)   ; R5RS compliant behavior

(symbol->string 'flying-fish)    ⇒ "flying-fish"
(symbol->string 'Martin)         ⇒ "martin"
(symbol->string
   (string->symbol "Malvina"))   ⇒ "Malvina"

(eq? 'mISSISSIppi 'mississippi)  ⇒ #t
(string->symbol "mISSISSIppi")   ⇒ mISSISSIppi
(eq? 'bitBlt (string->symbol "bitBlt")) ⇒ #f
(eq? 'LolliPop
  (string->symbol (symbol->string 'LolliPop))) ⇒ #t
(string=? "K. Harper, M.D."
  (symbol->string
    (string->symbol "K. Harper, M.D."))) ⇒ #t

(read-disable 'case-insensitive)   ; Guile default behavior

(symbol->string 'flying-fish)    ⇒ "flying-fish"
(symbol->string 'Martin)         ⇒ "Martin"
(symbol->string
   (string->symbol "Malvina"))   ⇒ "Malvina"

(eq? 'mISSISSIppi 'mississippi)  ⇒ #f
(string->symbol "mISSISSIppi")   ⇒ mISSISSIppi
(eq? 'bitBlt (string->symbol "bitBlt")) ⇒ #t
(eq? 'LolliPop
  (string->symbol (symbol->string 'LolliPop))) ⇒ #t
(string=? "K. Harper, M.D."
  (symbol->string
    (string->symbol "K. Harper, M.D."))) ⇒ #t
```

From C, there are lower level functions that construct a Scheme symbol
from a C string in the current locale encoding.

When you want to do more from C, you should convert between symbols
and strings using `scm_symbol_to_string` and
`scm_string_to_symbol` and work with the strings.

C Function: `SCM` **scm\_from\_latin1\_symbol** `(const char *name)` [¶](#index-scm_005ffrom_005flatin1_005fsymbol)

C Function: `SCM` **scm\_from\_utf8\_symbol** `(const char *name)` [¶](#index-scm_005ffrom_005futf8_005fsymbol)
:   Construct and return a Scheme symbol whose name is specified by the
    null-terminated C string name. These are appropriate when
    the C string is hard-coded in the source code.

C Function: `SCM` **scm\_from\_locale\_symbol** `(const char *name)` [¶](#index-scm_005ffrom_005flocale_005fsymbol)

C Function: `SCM` **scm\_from\_locale\_symboln** `(const char *name, size_t len)` [¶](#index-scm_005ffrom_005flocale_005fsymboln)
:   Construct and return a Scheme symbol whose name is specified by
    name. For `scm_from_locale_symbol`, name must be null
    terminated; for `scm_from_locale_symboln` the length of name is
    specified explicitly by len.

    Note that these functions should *not* be used when name is a
    C string constant, because there is no guarantee that the current locale
    will match that of the execution character set, used for string and
    character constants. Most modern C compilers use UTF-8 by default, so
    in such cases we recommend `scm_from_utf8_symbol`.

C Function: `SCM` **scm\_take\_locale\_symbol** `(char *str)` [¶](#index-scm_005ftake_005flocale_005fsymbol)

C Function: `SCM` **scm\_take\_locale\_symboln** `(char *str, size_t len)` [¶](#index-scm_005ftake_005flocale_005fsymboln)
:   Like `scm_from_locale_symbol` and `scm_from_locale_symboln`,
    respectively, but also frees str with `free` eventually.
    Thus, you can use this function when you would free str anyway
    immediately after creating the Scheme string. In certain cases, Guile
    can then use str directly as its internal representation.

The size of a symbol can also be obtained from C:

C Function: `size_t` **scm\_c\_symbol\_length** `(SCM sym)` [¶](#index-scm_005fc_005fsymbol_005flength)
:   Return the number of characters in sym.

Finally, some applications, especially those that generate new Scheme
code dynamically, need to generate symbols for use in the generated
code. The `gensym` primitive meets this need:

Scheme Procedure: **gensym** [prefix] [¶](#index-gensym)

C Function: **scm\_gensym** (prefix) [¶](#index-scm_005fgensym)
:   Create a new symbol with a name constructed from a prefix and a counter
    value. The string prefix can be specified as an optional
    argument. Default prefix is ‘`g`’. The counter is increased by 1
    at each call. There is no provision for resetting the counter.

The symbols generated by `gensym` are *likely* to be unique,
since their names begin with a space and it is only otherwise possible
to generate such symbols if a programmer goes out of their way to do
so. Uniqueness can be guaranteed by instead using uninterned symbols
(see [Uninterned Symbols](#Symbol-Uninterned)), though they can’t be usefully written out
and read back in.

---

Next: [Uninterned Symbols](#Symbol-Uninterned), Previous: [Operations Related to Symbols](#Symbol-Primitives), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.6.5 Extended Read Syntax for Symbols [¶](#Extended-Read-Syntax-for-Symbols)

The read syntax for a symbol is a sequence of letters, digits, and
*extended alphabetic characters*, beginning with a character that
cannot begin a number. In addition, the special cases of `+`,
`-`, and `...` are read as symbols even though numbers can
begin with `+`, `-` or `.`.

Extended alphabetic characters may be used within identifiers as if
they were letters. The set of extended alphabetic characters is:

```
! $ % & * + - . / : < = > ? @ ^ _ ~
```

In addition to the standard read syntax defined above (which is taken
from R5RS (see [Formal syntax](/r5rs/latest/en/r5rs.html#Formal-syntax) in The Revised^5 Report on
Scheme)), Guile provides an extended symbol read syntax that allows the
inclusion of unusual characters such as space characters, newlines and
parentheses. If (for whatever reason) you need to write a symbol
containing characters not mentioned above, you can do so as follows.

* Begin the symbol with the characters `#{`,
* write the characters of the symbol and
* finish the symbol with the characters `}#`.

Here are a few examples of this form of read syntax. The first symbol
needs to use extended syntax because it contains a space character, the
second because it contains a line break, and the last because it looks
like a number.

```
#{foo bar}#

#{what
ever}#

#{4242}#
```

Although Guile provides this extended read syntax for symbols,
widespread usage of it is discouraged because it is not portable and not
very readable.

Alternatively, if you enable the `r7rs-symbols` read option (see
see [Reading Scheme Code](#Scheme-Read)), you can write arbitrary symbols using the same
notation used for strings, except delimited by vertical bars instead of
double quotes.

```
|foo bar|
|\x3BB; is a greek lambda|
|\| is a vertical bar|
```

Note that there’s also an `r7rs-symbols` print option
(see [Writing Scheme Values](#Scheme-Write)). To enable the use of this notation, evaluate
one or both of the following expressions:

```
(read-enable  'r7rs-symbols)
(print-enable 'r7rs-symbols)
```

---

Previous: [Extended Read Syntax for Symbols](#Symbol-Read-Syntax), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.6.6 Uninterned Symbols [¶](#Uninterned-Symbols)

What makes symbols useful is that they are automatically kept unique.
There are no two symbols that are distinct objects but have the same
name. But of course, there is no rule without exception. In addition
to the normal symbols that have been discussed up to now, you can also
create special *uninterned* symbols that behave slightly
differently.

To understand what is different about them and why they might be useful,
we look at how normal symbols are actually kept unique.

Whenever Guile wants to find the symbol with a specific name, for
example during `read` or when executing `string->symbol`, it
first looks into a table of all existing symbols to find out whether a
symbol with the given name already exists. When this is the case, Guile
just returns that symbol. When not, a new symbol with the name is
created and entered into the table so that it can be found later.

Sometimes you might want to create a symbol that is guaranteed ‘fresh’,
i.e. a symbol that did not exist previously. You might also want to
somehow guarantee that no one else will ever unintentionally stumble
across your symbol in the future. These properties of a symbol are
often needed when generating code during macro expansion. When
introducing new temporary variables, you want to guarantee that they
don’t conflict with variables in other people’s code.

The simplest way to arrange for this is to create a new symbol but
not enter it into the global table of all symbols. That way, no one
will ever get access to your symbol by chance. Symbols that are not in
the table are called *uninterned*. Of course, symbols that
*are* in the table are called *interned*.

You create new uninterned symbols with the function `make-symbol`.
You can test whether a symbol is interned or not with
`symbol-interned?`.

Uninterned symbols break the rule that the name of a symbol uniquely
identifies the symbol object. Because of this, they can not be written
out and read back in like interned symbols. Currently, Guile has no
support for reading uninterned symbols. Note that the function
`gensym` does not return uninterned symbols for this reason.

Scheme Procedure: **make-symbol** name [¶](#index-make_002dsymbol)

C Function: **scm\_make\_symbol** (name) [¶](#index-scm_005fmake_005fsymbol)
:   Return a new uninterned symbol with the name name. The returned
    symbol is guaranteed to be unique and future calls to
    `string->symbol` will not return it.

Scheme Procedure: **symbol-interned?** symbol [¶](#index-symbol_002dinterned_003f)

C Function: **scm\_symbol\_interned\_p** (symbol) [¶](#index-scm_005fsymbol_005finterned_005fp)
:   Return `#t` if symbol is interned, otherwise return
    `#f`.

For example:

```
(define foo-1 (string->symbol "foo"))
(define foo-2 (string->symbol "foo"))
(define foo-3 (make-symbol "foo"))
(define foo-4 (make-symbol "foo"))

(eq? foo-1 foo-2)
⇒ #t
; Two interned symbols with the same name are the same object,

(eq? foo-1 foo-3)
⇒ #f
; but a call to make-symbol with the same name returns a
; distinct object.

(eq? foo-3 foo-4)
⇒ #f
; A call to make-symbol always returns a new object, even for
; the same name.

foo-3
⇒ #<uninterned-symbol foo 8085290>
; Uninterned symbols print differently from interned symbols,

(symbol? foo-3)
⇒ #t
; but they are still symbols,

(symbol-interned? foo-3)
⇒ #f
; just not interned.
```

---

Next: [Pairs](#Pairs), Previous: [Symbols](#Symbols), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.7 Keywords [¶](#Keywords-1)

Keywords are self-evaluating objects with a convenient read syntax that
makes them easy to type.

Guile’s keyword support conforms to R5RS, and adds a (switchable) read
syntax extension to permit keywords to begin with `:` as well as
`#:`, or to end with `:`.

* [Why Use Keywords?](#Why-Use-Keywords_003f)
* [Coding With Keywords](#Coding-With-Keywords)
* [Keyword Read Syntax](#Keyword-Read-Syntax)
* [Keyword Procedures](#Keyword-Procedures)

---

Next: [Coding With Keywords](#Coding-With-Keywords), Up: [Keywords](#Keywords)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.7.1 Why Use Keywords? [¶](#Why-Use-Keywords_003f-1)

Keywords are useful in contexts where a program or procedure wants to be
able to accept a large number of optional arguments without making its
interface unmanageable.

To illustrate this, consider a hypothetical `make-window`
procedure, which creates a new window on the screen for drawing into
using some graphical toolkit. There are many parameters that the caller
might like to specify, but which could also be sensibly defaulted, for
example:

* color depth – Default: the color depth for the screen
* background color – Default: white
* width – Default: 600
* height – Default: 400

If `make-window` did not use keywords, the caller would have to
pass in a value for each possible argument, remembering the correct
argument order and using a special value to indicate the default value
for that argument:

```
(make-window 'default              ;; Color depth
             'default              ;; Background color
             800                   ;; Width
             100                   ;; Height
             ...)                  ;; More make-window arguments
```

With keywords, on the other hand, defaulted arguments are omitted, and
non-default arguments are clearly tagged by the appropriate keyword. As
a result, the invocation becomes much clearer:

```
(make-window #:width 800 #:height 100)
```

On the other hand, for a simpler procedure with few arguments, the use
of keywords would be a hindrance rather than a help. The primitive
procedure `cons`, for example, would not be improved if it had to
be invoked as

```
(cons #:car x #:cdr y)
```

So the decision whether to use keywords or not is purely pragmatic: use
them if they will clarify the procedure invocation at point of call.

---

Next: [Keyword Read Syntax](#Keyword-Read-Syntax), Previous: [Why Use Keywords?](#Why-Use-Keywords_003f), Up: [Keywords](#Keywords)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.7.2 Coding With Keywords [¶](#Coding-With-Keywords-1)

If a procedure wants to support keywords, it should take a rest argument
and then use whatever means is convenient to extract keywords and their
corresponding arguments from the contents of that rest argument.

The following example illustrates the principle: the code for
`make-window` uses a helper procedure called
`get-keyword-value` to extract individual keyword arguments from
the rest argument.

```
(define (get-keyword-value args keyword default)
  (let ((kv (memq keyword args)))
    (if (and kv (>= (length kv) 2))
        (cadr kv)
        default)))

(define (make-window . args)
  (let ((depth  (get-keyword-value args #:depth  screen-depth))
        (bg     (get-keyword-value args #:bg     "white"))
        (width  (get-keyword-value args #:width  800))
        (height (get-keyword-value args #:height 100))
        ...)
    ...))
```

But you don’t need to write `get-keyword-value`. The `(ice-9
optargs)` module provides a set of powerful macros that you can use to
implement keyword-supporting procedures like this:

```
(use-modules (ice-9 optargs))

(define (make-window . args)
  (let-keywords args #f ((depth  screen-depth)
                         (bg     "white")
                         (width  800)
                         (height 100))
    ...))
```

Or, even more economically, like this:

```
(use-modules (ice-9 optargs))

(define* (make-window #:key (depth  screen-depth)
                            (bg     "white")
                            (width  800)
                            (height 100))
  ...)
```

For further details on `let-keywords`, `define*` and other
facilities provided by the `(ice-9 optargs)` module, see
[Optional Arguments](#Optional-Arguments).

To handle keyword arguments from procedures implemented in C,
use `scm_c_bind_keyword_arguments` (see [Keyword Procedures](#Keyword-Procedures)).

---

Next: [Keyword Procedures](#Keyword-Procedures), Previous: [Coding With Keywords](#Coding-With-Keywords), Up: [Keywords](#Keywords)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.7.3 Keyword Read Syntax [¶](#Keyword-Read-Syntax-1)

Guile, by default, only recognizes a keyword syntax that is compatible
with R5RS. A token of the form `#:NAME`, where `NAME` has the
same syntax as a Scheme symbol (see [Extended Read Syntax for Symbols](#Symbol-Read-Syntax)), is the
external representation of the keyword named `NAME`. Keyword
objects print using this syntax as well, so values containing keyword
objects can be read back into Guile. When used in an expression,
keywords are self-quoting objects.

If the `keywords` read option is set to `'prefix`, Guile also
recognizes the alternative read syntax `:NAME`. Otherwise, tokens
of the form `:NAME` are read as symbols, as required by R5RS.

If the `keywords` read option is set to `'postfix`, Guile
recognizes the SRFI-88 read syntax `NAME:` (see [SRFI-88 Keyword Objects](#SRFI_002d88)).
Otherwise, tokens of this form are read as symbols.

To enable and disable the alternative non-R5RS keyword syntax, you use
the `read-set!` procedure documented [Reading Scheme Code](#Scheme-Read). Note that
the `prefix` and `postfix` syntax are mutually exclusive.

```
(read-set! keywords 'prefix)

#:type
⇒
#:type

:type
⇒
#:type

(read-set! keywords 'postfix)

type:
⇒
#:type

:type
⇒
:type

(read-set! keywords #f)

#:type
⇒
#:type

:type
⊣
ERROR: In expression :type:
ERROR: Unbound variable: :type
ABORT: (unbound-variable)
```

---

Previous: [Keyword Read Syntax](#Keyword-Read-Syntax), Up: [Keywords](#Keywords)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.7.4 Keyword Procedures [¶](#Keyword-Procedures-1)

Scheme Procedure: **keyword?** obj [¶](#index-keyword_003f)

C Function: **scm\_keyword\_p** (obj) [¶](#index-scm_005fkeyword_005fp)
:   Return `#t` if the argument obj is a keyword, else
    `#f`.

Scheme Procedure: **keyword->symbol** keyword [¶](#index-keyword_002d_003esymbol)

C Function: **scm\_keyword\_to\_symbol** (keyword) [¶](#index-scm_005fkeyword_005fto_005fsymbol)
:   Return the symbol with the same name as keyword.

Scheme Procedure: **symbol->keyword** symbol [¶](#index-symbol_002d_003ekeyword)

C Function: **scm\_symbol\_to\_keyword** (symbol) [¶](#index-scm_005fsymbol_005fto_005fkeyword)
:   Return the keyword with the same name as symbol.

C Function: `int` **scm\_is\_keyword** `(SCM obj)` [¶](#index-scm_005fis_005fkeyword)
:   Equivalent to `scm_is_true (scm_keyword_p (obj))`.

C Function: `SCM` **scm\_from\_locale\_keyword** `(const char *name)` [¶](#index-scm_005ffrom_005flocale_005fkeyword)

C Function: `SCM` **scm\_from\_locale\_keywordn** `(const char *name, size_t len)` [¶](#index-scm_005ffrom_005flocale_005fkeywordn)
:   Equivalent to `scm_symbol_to_keyword (scm_from_locale_symbol
    (name))` and `scm_symbol_to_keyword (scm_from_locale_symboln
    (name, len))`, respectively.

    Note that these functions should *not* be used when name is a
    C string constant, because there is no guarantee that the current locale
    will match that of the execution character set, used for string and
    character constants. Most modern C compilers use UTF-8 by default, so
    in such cases we recommend `scm_from_utf8_keyword`.

C Function: `SCM` **scm\_from\_latin1\_keyword** `(const char *name)` [¶](#index-scm_005ffrom_005flatin1_005fkeyword)

C Function: `SCM` **scm\_from\_utf8\_keyword** `(const char *name)` [¶](#index-scm_005ffrom_005futf8_005fkeyword)
:   Equivalent to `scm_symbol_to_keyword (scm_from_latin1_symbol
    (name))` and `scm_symbol_to_keyword (scm_from_utf8_symbol
    (name))`, respectively.

C Function: `void` **scm\_c\_bind\_keyword\_arguments** `(const char *subr, SCM rest, scm_t_keyword_arguments_flags flags, SCM keyword1, SCM *argp1, …, SCM keywordN, SCM *argpN, SCM_UNDEFINED)` [¶](#index-scm_005fc_005fbind_005fkeyword_005farguments)
:   Extract the specified keyword arguments from rest, which is not
    modified. If the keyword argument keyword1 is present in
    rest with an associated value, that value is stored in the
    variable pointed to by argp1, otherwise the variable is left
    unchanged. Similarly for the other keywords and argument pointers up to
    keywordN and argpN. The argument list to
    `scm_c_bind_keyword_arguments` must be terminated by
    `SCM_UNDEFINED`.

    Note that since the variables pointed to by argp1 through
    argpN are left unchanged if the associated keyword argument is not
    present, they should be initialized to their default values before
    calling `scm_c_bind_keyword_arguments`. Alternatively, you can
    initialize them to `SCM_UNDEFINED` before the call, and then use
    `SCM_UNBNDP` after the call to see which ones were provided.

    If an unrecognized keyword argument is present in rest and
    flags does not contain `SCM_ALLOW_OTHER_KEYS`, or if
    non-keyword arguments are present and flags does not contain
    `SCM_ALLOW_NON_KEYWORD_ARGUMENTS`, an exception is raised.
    subr should be the name of the procedure receiving the keyword
    arguments, for purposes of error reporting.

    For example:

    ```
    SCM k_delimiter;
    SCM k_grammar;
    SCM sym_infix;

    SCM my_string_join (SCM strings, SCM rest)
    {
      SCM delimiter = SCM_UNDEFINED;
      SCM grammar   = sym_infix;

      scm_c_bind_keyword_arguments ("my-string-join", rest, 0,
                                    k_delimiter, &delimiter,
                                    k_grammar, &grammar,
                                    SCM_UNDEFINED);

      if (SCM_UNBNDP (delimiter))
        delimiter = scm_from_utf8_string (" ");

      return scm_string_join (strings, delimiter, grammar);
    }

    void my_init ()
    {
      k_delimiter = scm_from_utf8_keyword ("delimiter");
      k_grammar   = scm_from_utf8_keyword ("grammar");
      sym_infix   = scm_from_utf8_symbol  ("infix");
      scm_c_define_gsubr ("my-string-join", 1, 0, 1, my_string_join);
    }
    ```

---

Next: [Lists](#Lists), Previous: [Keywords](#Keywords), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.8 Pairs [¶](#Pairs-1)

Pairs are used to combine two Scheme objects into one compound object.
Hence the name: A pair stores a pair of objects.

The data type *pair* is extremely important in Scheme, just like in
any other Lisp dialect. The reason is that pairs are not only used to
make two values available as one object, but that pairs are used for
constructing lists of values. Because lists are so important in Scheme,
they are described in a section of their own (see [Lists](#Lists)).

Pairs can literally get entered in source code or at the REPL, in the
so-called *dotted list* syntax. This syntax consists of an opening
parentheses, the first element of the pair, a dot, the second element
and a closing parentheses. The following example shows how a pair
consisting of the two numbers 1 and 2, and a pair containing the symbols
`foo` and `bar` can be entered. It is very important to write
the whitespace before and after the dot, because otherwise the Scheme
parser would not be able to figure out where to split the tokens.

```
(1 . 2)
(foo . bar)
```

But beware, if you want to try out these examples, you have to
*quote* the expressions. More information about quotation is
available in the section [Expression Syntax](#Expression-Syntax). The correct way
to try these examples is as follows.

```
'(1 . 2)
⇒
(1 . 2)
'(foo . bar)
⇒
(foo . bar)
```

A new pair is made by calling the procedure `cons` with two
arguments. Then the argument values are stored into a newly allocated
pair, and the pair is returned. The name `cons` stands for
"construct". Use the procedure `pair?` to test whether a
given Scheme object is a pair or not.

Scheme Procedure: **cons** x y [¶](#index-cons)

C Function: **scm\_cons** (x, y) [¶](#index-scm_005fcons)
:   Return a newly allocated pair whose car is x and whose
    cdr is y. The pair is guaranteed to be different (in the
    sense of `eq?`) from every previously existing object.

Scheme Procedure: **pair?** x [¶](#index-pair_003f)

C Function: **scm\_pair\_p** (x) [¶](#index-scm_005fpair_005fp)
:   Return `#t` if x is a pair; otherwise return
    `#f`.

C Function: `int` **scm\_is\_pair** `(SCM x)` [¶](#index-scm_005fis_005fpair)
:   Return 1 when x is a pair; otherwise return 0.

The two parts of a pair are traditionally called *car* and
*cdr*. They can be retrieved with procedures of the same name
(`car` and `cdr`), and can be modified with the procedures
`set-car!` and `set-cdr!`.

Since a very common operation in Scheme programs is to access the car of
a car of a pair, or the car of the cdr of a pair, etc., the procedures
called `caar`, `cadr` and so on are also predefined. However,
using these procedures is often detrimental to readability, and
error-prone. Thus, accessing the contents of a list is usually better
achieved using pattern matching techniques (see [Pattern Matching](#Pattern-Matching)).

Scheme Procedure: **car** pair [¶](#index-car)

Scheme Procedure: **cdr** pair [¶](#index-cdr)

C Function: **scm\_car** (pair) [¶](#index-scm_005fcar)

C Function: **scm\_cdr** (pair) [¶](#index-scm_005fcdr)
:   Return the car or the cdr of pair, respectively.

C Macro: `SCM` **SCM\_CAR** `(SCM pair)` [¶](#index-SCM_005fCAR)

C Macro: `SCM` **SCM\_CDR** `(SCM pair)` [¶](#index-SCM_005fCDR)
:   These two macros are the fastest way to access the car or cdr of a
    pair; they can be thought of as compiling into a single memory
    reference.

    These macros do no checking at all. The argument pair must be a
    valid pair.

Scheme Procedure: **cddr** pair [¶](#index-cddr)

Scheme Procedure: **cdar** pair [¶](#index-cdar)

Scheme Procedure: **cadr** pair [¶](#index-cadr)

Scheme Procedure: **caar** pair [¶](#index-caar)

Scheme Procedure: **cdddr** pair [¶](#index-cdddr)

Scheme Procedure: **cddar** pair [¶](#index-cddar)

Scheme Procedure: **cdadr** pair [¶](#index-cdadr)

Scheme Procedure: **cdaar** pair [¶](#index-cdaar)

Scheme Procedure: **caddr** pair [¶](#index-caddr)

Scheme Procedure: **cadar** pair [¶](#index-cadar)

Scheme Procedure: **caadr** pair [¶](#index-caadr)

Scheme Procedure: **caaar** pair [¶](#index-caaar)

Scheme Procedure: **cddddr** pair [¶](#index-cddddr)

Scheme Procedure: **cdddar** pair [¶](#index-cdddar)

Scheme Procedure: **cddadr** pair [¶](#index-cddadr)

Scheme Procedure: **cddaar** pair [¶](#index-cddaar)

Scheme Procedure: **cdaddr** pair [¶](#index-cdaddr)

Scheme Procedure: **cdadar** pair [¶](#index-cdadar)

Scheme Procedure: **cdaadr** pair [¶](#index-cdaadr)

Scheme Procedure: **cdaaar** pair [¶](#index-cdaaar)

Scheme Procedure: **cadddr** pair [¶](#index-cadddr)

Scheme Procedure: **caddar** pair [¶](#index-caddar)

Scheme Procedure: **cadadr** pair [¶](#index-cadadr)

Scheme Procedure: **cadaar** pair [¶](#index-cadaar)

Scheme Procedure: **caaddr** pair [¶](#index-caaddr)

Scheme Procedure: **caadar** pair [¶](#index-caadar)

Scheme Procedure: **caaadr** pair [¶](#index-caaadr)

Scheme Procedure: **caaaar** pair [¶](#index-caaaar)

C Function: **scm\_cddr** (pair) [¶](#index-scm_005fcddr)

C Function: **scm\_cdar** (pair) [¶](#index-scm_005fcdar)

C Function: **scm\_cadr** (pair) [¶](#index-scm_005fcadr)

C Function: **scm\_caar** (pair) [¶](#index-scm_005fcaar)

C Function: **scm\_cdddr** (pair) [¶](#index-scm_005fcdddr)

C Function: **scm\_cddar** (pair) [¶](#index-scm_005fcddar)

C Function: **scm\_cdadr** (pair) [¶](#index-scm_005fcdadr)

C Function: **scm\_cdaar** (pair) [¶](#index-scm_005fcdaar)

C Function: **scm\_caddr** (pair) [¶](#index-scm_005fcaddr)

C Function: **scm\_cadar** (pair) [¶](#index-scm_005fcadar)

C Function: **scm\_caadr** (pair) [¶](#index-scm_005fcaadr)

C Function: **scm\_caaar** (pair) [¶](#index-scm_005fcaaar)

C Function: **scm\_cddddr** (pair) [¶](#index-scm_005fcddddr)

C Function: **scm\_cdddar** (pair) [¶](#index-scm_005fcdddar)

C Function: **scm\_cddadr** (pair) [¶](#index-scm_005fcddadr)

C Function: **scm\_cddaar** (pair) [¶](#index-scm_005fcddaar)

C Function: **scm\_cdaddr** (pair) [¶](#index-scm_005fcdaddr)

C Function: **scm\_cdadar** (pair) [¶](#index-scm_005fcdadar)

C Function: **scm\_cdaadr** (pair) [¶](#index-scm_005fcdaadr)

C Function: **scm\_cdaaar** (pair) [¶](#index-scm_005fcdaaar)

C Function: **scm\_cadddr** (pair) [¶](#index-scm_005fcadddr)

C Function: **scm\_caddar** (pair) [¶](#index-scm_005fcaddar)

C Function: **scm\_cadadr** (pair) [¶](#index-scm_005fcadadr)

C Function: **scm\_cadaar** (pair) [¶](#index-scm_005fcadaar)

C Function: **scm\_caaddr** (pair) [¶](#index-scm_005fcaaddr)

C Function: **scm\_caadar** (pair) [¶](#index-scm_005fcaadar)

C Function: **scm\_caaadr** (pair) [¶](#index-scm_005fcaaadr)

C Function: **scm\_caaaar** (pair) [¶](#index-scm_005fcaaaar)
:   These procedures are compositions of `car` and `cdr`, where
    for example `caddr` could be defined by

    ```
    (define caddr (lambda (x) (car (cdr (cdr x)))))
    ```

    `cadr`, `caddr` and `cadddr` pick out the second, third
    or fourth elements of a list, respectively. SRFI-1 provides the same
    under the names `second`, `third` and `fourth`
    (see [Selectors](#SRFI_002d1-Selectors)).

Scheme Procedure: **set-car!** pair value [¶](#index-set_002dcar_0021)

C Function: **scm\_set\_car\_x** (pair, value) [¶](#index-scm_005fset_005fcar_005fx)
:   Stores value in the car field of pair. The value returned
    by `set-car!` is unspecified.

Scheme Procedure: **set-cdr!** pair value [¶](#index-set_002dcdr_0021)

C Function: **scm\_set\_cdr\_x** (pair, value) [¶](#index-scm_005fset_005fcdr_005fx)
:   Stores value in the cdr field of pair. The value returned
    by `set-cdr!` is unspecified.

---

Next: [Vectors](#Vectors), Previous: [Pairs](#Pairs), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.9 Lists [¶](#Lists-1)

A very important data type in Scheme—as well as in all other Lisp
dialects—is the data type *list*.[9](#FOOT9)

This is the short definition of what a list is:

* Either the empty list `()`,
* or a pair which has a list in its cdr.

* [List Read Syntax](#List-Syntax)
* [List Predicates](#List-Predicates)
* [List Constructors](#List-Constructors)
* [List Selection](#List-Selection)
* [Append and Reverse](#Append_002fReverse)
* [List Modification](#List-Modification)
* [List Searching](#List-Searching)
* [List Mapping](#List-Mapping)

---

Next: [List Predicates](#List-Predicates), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.9.1 List Read Syntax [¶](#List-Read-Syntax)

The syntax for lists is an opening parentheses, then all the elements of
the list (separated by whitespace) and finally a closing
parentheses.[10](#FOOT10).

```
(1 2 3)            ; a list of the numbers 1, 2 and 3
("foo" bar 3.1415) ; a string, a symbol and a real number
()                 ; the empty list
```

The last example needs a bit more explanation. A list with no elements,
called the *empty list*, is special in some ways. It is used for
terminating lists by storing it into the cdr of the last pair that makes
up a list. An example will clear that up:

```
(car '(1))
⇒
1
(cdr '(1))
⇒
()
```

This example also shows that lists have to be quoted when written
(see [Expression Syntax](#Expression-Syntax)), because they would otherwise be
mistakenly taken as procedure applications (see [Simple Procedure Invocation](#Simple-Invocation)).

---

Next: [List Constructors](#List-Constructors), Previous: [List Read Syntax](#List-Syntax), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.9.2 List Predicates [¶](#List-Predicates-1)

Often it is useful to test whether a given Scheme object is a list or
not. List-processing procedures could use this information to test
whether their input is valid, or they could do different things
depending on the datatype of their arguments.

Scheme Procedure: **list?** x [¶](#index-list_003f)

C Function: **scm\_list\_p** (x) [¶](#index-scm_005flist_005fp)
:   Return `#t` if x is a proper list, else `#f`.

The predicate `null?` is often used in list-processing code to
tell whether a given list has run out of elements. That is, a loop
somehow deals with the elements of a list until the list satisfies
`null?`. Then, the algorithm terminates.

Scheme Procedure: **null?** x [¶](#index-null_003f)

C Function: **scm\_null\_p** (x) [¶](#index-scm_005fnull_005fp)
:   Return `#t` if x is the empty list, else `#f`.

C Function: `int` **scm\_is\_null** `(SCM x)` [¶](#index-scm_005fis_005fnull)
:   Return 1 when x is the empty list; otherwise return 0.

---

Next: [List Selection](#List-Selection), Previous: [List Predicates](#List-Predicates), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.9.3 List Constructors [¶](#List-Constructors-1)

This section describes the procedures for constructing new lists.
`list` simply returns a list where the elements are the arguments,
`cons*` is similar, but the last argument is stored in the cdr of
the last pair of the list.

Scheme Procedure: **list** elem … [¶](#index-list-1)

C Function: **scm\_list\_1** (elem1) [¶](#index-scm_005flist_005f1)

C Function: **scm\_list\_2** (elem1, elem2) [¶](#index-scm_005flist_005f2)

C Function: **scm\_list\_3** (elem1, elem2, elem3) [¶](#index-scm_005flist_005f3)

C Function: **scm\_list\_4** (elem1, elem2, elem3, elem4) [¶](#index-scm_005flist_005f4)

C Function: **scm\_list\_5** (elem1, elem2, elem3, elem4, elem5) [¶](#index-scm_005flist_005f5)

C Function: **scm\_list\_n** (elem1, …, elemN, `SCM_UNDEFINED`) [¶](#index-scm_005flist_005fn)
:   Return a new list containing elements elem ....

    `scm_list_n` takes a variable number of arguments, terminated by
    the special `SCM_UNDEFINED`. That final `SCM_UNDEFINED` is
    not included in the list. None of elem … can
    themselves be `SCM_UNDEFINED`, or `scm_list_n` will
    terminate at that point.

Scheme Procedure: **cons\*** arg1 arg2 … [¶](#index-cons_002a)
:   Like `list`, but the last arg provides the tail of the
    constructed list, returning `(cons arg1 (cons
    arg2 (cons … argn)))`. Requires at least one
    argument. If given one argument, that argument is returned as
    result. This function is called `list*` in some other
    Schemes and in Common LISP.

Scheme Procedure: **list-copy** lst [¶](#index-list_002dcopy)

C Function: **scm\_list\_copy** (lst) [¶](#index-scm_005flist_005fcopy)
:   Return a (newly-created) copy of lst.

Scheme Procedure: **make-list** n [init] [¶](#index-make_002dlist)
:   Create a list containing of n elements, where each element is
    initialized to init. init defaults to the empty list
    `()` if not given.

Note that `list-copy` only makes a copy of the pairs which make up
the spine of the lists. The list elements are not copied, which means
that modifying the elements of the new list also modifies the elements
of the old list. On the other hand, applying procedures like
`set-cdr!` or `delv!` to the new list will not alter the old
list. If you also need to copy the list elements (making a deep copy),
use the procedure `copy-tree` from `(ice-9 copy-tree)`
(see [Copying Deep Structures](#Copying)).

---

Next: [Append and Reverse](#Append_002fReverse), Previous: [List Constructors](#List-Constructors), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.9.4 List Selection [¶](#List-Selection-1)

These procedures are used to get some information about a list, or to
retrieve one or more elements of a list.

Scheme Procedure: **length** lst [¶](#index-length)

C Function: **scm\_length** (lst) [¶](#index-scm_005flength)
:   Return the number of elements in list lst.

Scheme Procedure: **last-pair** lst [¶](#index-last_002dpair)

C Function: **scm\_last\_pair** (lst) [¶](#index-scm_005flast_005fpair)
:   Return the last pair in lst, signaling an error if
    lst is circular.

Scheme Procedure: **list-ref** list k [¶](#index-list_002dref)

C Function: **scm\_list\_ref** (list, k) [¶](#index-scm_005flist_005fref)
:   Return the kth element from list.

Scheme Procedure: **list-tail** lst k [¶](#index-list_002dtail)

Scheme Procedure: **list-cdr-ref** lst k [¶](#index-list_002dcdr_002dref)

C Function: **scm\_list\_tail** (lst, k) [¶](#index-scm_005flist_005ftail)
:   Return the "tail" of lst beginning with its kth element.
    The first element of the list is considered to be element 0.

    `list-tail` and `list-cdr-ref` are identical. It may help to
    think of `list-cdr-ref` as accessing the kth cdr of the list,
    or returning the results of cdring k times down lst.

Scheme Procedure: **list-head** lst k [¶](#index-list_002dhead)

C Function: **scm\_list\_head** (lst, k) [¶](#index-scm_005flist_005fhead)
:   Copy the first k elements from lst into a new list, and
    return it.

---

Next: [List Modification](#List-Modification), Previous: [List Selection](#List-Selection), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.9.5 Append and Reverse [¶](#Append-and-Reverse)

`append` and `append!` are used to concatenate two or more
lists in order to form a new list. `reverse` and `reverse!`
return lists with the same elements as their arguments, but in reverse
order. The procedure variants with an `!` directly modify the
pairs which form the list, whereas the other procedures create new
pairs. This is why you should be careful when using the side-effecting
variants.

Scheme Procedure: **append** lst … obj [¶](#index-append)

Scheme Procedure: **append** [¶](#index-append-1)

Scheme Procedure: **append!** lst … obj [¶](#index-append_0021)

Scheme Procedure: **append!** [¶](#index-append_0021-1)

C Function: **scm\_append** (lstlst) [¶](#index-scm_005fappend)

C Function: **scm\_append\_x** (lstlst) [¶](#index-scm_005fappend_005fx)
:   Return a list comprising all the elements of lists lst …
    obj. If called with no arguments, return the empty list.

    ```
    (append '(x) '(y))          ⇒  (x y)
    (append '(a) '(b c d))      ⇒  (a b c d)
    (append '(a (b)) '((c)))    ⇒  (a (b) (c))
    ```

    The last argument obj may actually be any object; an improper
    list results if the last argument is not a proper list.

    ```
    (append '(a b) '(c . d))    ⇒  (a b c . d)
    (append '() 'a)             ⇒  a
    ```

    `append` doesn’t modify the given lists, but the return may share
    structure with the final obj. `append!` is permitted, but
    not required, to modify the given lists to form its return.

    For `scm_append` and `scm_append_x`, lstlst is a list
    of the list operands lst … obj. That lstlst
    itself is not modified or used in the return.

Scheme Procedure: **reverse** lst [¶](#index-reverse)

Scheme Procedure: **reverse!** lst [newtail] [¶](#index-reverse_0021)

C Function: **scm\_reverse** (lst) [¶](#index-scm_005freverse)

C Function: **scm\_reverse\_x** (lst, newtail) [¶](#index-scm_005freverse_005fx)
:   Return a list comprising the elements of lst, in reverse order.

    `reverse` constructs a new list. `reverse!` is permitted, but
    not required, to modify lst in constructing its return.

    For `reverse!`, the optional newtail is appended to the
    result. newtail isn’t reversed, it simply becomes the list
    tail. For `scm_reverse_x`, the newtail parameter is
    mandatory, but can be `SCM_EOL` if no further tail is required.

---

Next: [List Searching](#List-Searching), Previous: [Append and Reverse](#Append_002fReverse), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.9.6 List Modification [¶](#List-Modification-1)

The following procedures modify an existing list, either by changing
elements of the list, or by changing the list structure itself.

Scheme Procedure: **list-set!** list k val [¶](#index-list_002dset_0021)

C Function: **scm\_list\_set\_x** (list, k, val) [¶](#index-scm_005flist_005fset_005fx)
:   Set the kth element of list to val.

Scheme Procedure: **list-cdr-set!** list k val [¶](#index-list_002dcdr_002dset_0021)

C Function: **scm\_list\_cdr\_set\_x** (list, k, val) [¶](#index-scm_005flist_005fcdr_005fset_005fx)
:   Set the kth cdr of list to val.

Scheme Procedure: **delq** item lst [¶](#index-delq)

C Function: **scm\_delq** (item, lst) [¶](#index-scm_005fdelq)
:   Return a newly-created copy of lst with elements
    `eq?` to item removed. This procedure mirrors
    `memq`: `delq` compares elements of lst against
    item with `eq?`.

Scheme Procedure: **delv** item lst [¶](#index-delv)

C Function: **scm\_delv** (item, lst) [¶](#index-scm_005fdelv)
:   Return a newly-created copy of lst with elements
    `eqv?` to item removed. This procedure mirrors
    `memv`: `delv` compares elements of lst against
    item with `eqv?`.

Scheme Procedure: **delete** item lst [¶](#index-delete)

C Function: **scm\_delete** (item, lst) [¶](#index-scm_005fdelete)
:   Return a newly-created copy of lst with elements
    `equal?` to item removed. This procedure mirrors
    `member`: `delete` compares elements of lst
    against item with `equal?`.

    See also SRFI-1 which has an extended `delete` ([Deleting](#SRFI_002d1-Deleting)), and also an `lset-difference` which can delete
    multiple items in one call ([Set Operations on Lists](#SRFI_002d1-Set-Operations)).

Scheme Procedure: **delq!** item lst [¶](#index-delq_0021)

Scheme Procedure: **delv!** item lst [¶](#index-delv_0021)

Scheme Procedure: **delete!** item lst [¶](#index-delete_0021)

C Function: **scm\_delq\_x** (item, lst) [¶](#index-scm_005fdelq_005fx)

C Function: **scm\_delv\_x** (item, lst) [¶](#index-scm_005fdelv_005fx)

C Function: **scm\_delete\_x** (item, lst) [¶](#index-scm_005fdelete_005fx)
:   These procedures are destructive versions of `delq`, `delv`
    and `delete`: they modify the pointers in the existing lst
    rather than creating a new list. Caveat evaluator: Like other
    destructive list functions, these functions cannot modify the binding of
    lst, and so cannot be used to delete the first element of
    lst destructively.

Scheme Procedure: **delq1!** item lst [¶](#index-delq1_0021)

C Function: **scm\_delq1\_x** (item, lst) [¶](#index-scm_005fdelq1_005fx)
:   Like `delq!`, but only deletes the first occurrence of
    item from lst. Tests for equality using
    `eq?`. See also `delv1!` and `delete1!`.

Scheme Procedure: **delv1!** item lst [¶](#index-delv1_0021)

C Function: **scm\_delv1\_x** (item, lst) [¶](#index-scm_005fdelv1_005fx)
:   Like `delv!`, but only deletes the first occurrence of
    item from lst. Tests for equality using
    `eqv?`. See also `delq1!` and `delete1!`.

Scheme Procedure: **delete1!** item lst [¶](#index-delete1_0021)

C Function: **scm\_delete1\_x** (item, lst) [¶](#index-scm_005fdelete1_005fx)
:   Like `delete!`, but only deletes the first occurrence of
    item from lst. Tests for equality using
    `equal?`. See also `delq1!` and `delv1!`.

Scheme Procedure: **filter** pred lst [¶](#index-filter)

Scheme Procedure: **filter!** pred lst [¶](#index-filter_0021)
:   Return a list containing all elements from lst which satisfy the
    predicate pred. The elements in the result list have the same
    order as in lst. The order in which pred is applied to
    the list elements is not specified.

    `filter` does not change lst, but the result may share a
    tail with it. `filter!` may modify lst to construct its
    return.

---

Next: [List Mapping](#List-Mapping), Previous: [List Modification](#List-Modification), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.9.7 List Searching [¶](#List-Searching-1)

The following procedures search lists for particular elements. They use
different comparison predicates for comparing list elements with the
object to be searched. When they fail, they return `#f`, otherwise
they return the sublist whose car is equal to the search object, where
equality depends on the equality predicate used.

Scheme Procedure: **memq** x lst [¶](#index-memq)
