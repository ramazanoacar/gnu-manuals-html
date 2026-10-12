### 6.6 Data Types [¶](#Data-Types-1)

Guile’s data types form a powerful built-in library of representations
and functionality that you can apply to your problem domain. This
chapter surveys the data types built-in to Guile, from the simple to the
complex.

* [Booleans](#Booleans)
* [Numerical data types](#Numbers)
* [Characters](#Characters)
* [Character Sets](#Character-Sets)
* [Strings](#Strings)
* [Symbols](#Symbols)
* [Keywords](#Keywords)
* [Pairs](#Pairs)
* [Lists](#Lists)
* [Vectors](#Vectors)
* [Bit Vectors](#Bit-Vectors)
* [Bytevectors](#Bytevectors)
* [Arrays](#Arrays)
* [VLists](#VLists)
* [Record Overview](#Record-Overview)
* [SRFI-9 Records](#SRFI_002d9-Records)
* [Records](#Records)
* [Structures](#Structures)
* [Dictionary Types](#Dictionary-Types)
* [Association Lists](#Association-Lists)
* [VList-Based Hash Lists or “VHashes”](#VHashes)
* [Hash Tables](#Hash-Tables)
* [Other Types](#Other-Types)

---

Next: [Numerical data types](#Numbers), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.1 Booleans [¶](#Booleans-1)

The two boolean values are `#t` for true and `#f` for false.
They can also be written as `#true` and `#false`, as per R7RS.

Boolean values are returned by predicate procedures, such as the general
equality predicates `eq?`, `eqv?` and `equal?`
(see [Equality](#Equality)) and numerical and string comparison operators like
`string=?` (see [String Comparison](#String-Comparison)) and `<=`
(see [Comparison Predicates](#Comparison)).

```
(<= 3 8)
⇒ #t

(<= 3 -3)
⇒ #f

(equal? "house" "houses")
⇒ #f

(eq? #f #f)
⇒
#t
```

In test condition contexts like `if` and `cond`
(see [Simple Conditional Evaluation](#Conditionals)), where a group of subexpressions will be
evaluated only if a condition expression evaluates to “true”,
“true” means any value at all except `#f`.

```
(if #t "yes" "no")
⇒ "yes"

(if 0 "yes" "no")
⇒ "yes"

(if #f "yes" "no")
⇒ "no"
```

A result of this asymmetry is that typical Scheme source code more often
uses `#f` explicitly than `#t`: `#f` is necessary to
represent an `if` or `cond` false value, whereas `#t` is
not necessary to represent an `if` or `cond` true value.

It is important to note that `#f` is **not** equivalent to any
other Scheme value. In particular, `#f` is not the same as the
number 0 (like in C and C++), and not the same as the “empty list”
(like in some Lisp dialects).

In C, the two Scheme boolean values are available as the two constants
`SCM_BOOL_T` for `#t` and `SCM_BOOL_F` for `#f`.
Care must be taken with the false value `SCM_BOOL_F`: it is not
false when used in C conditionals. In order to test for it, use
`scm_is_false` or `scm_is_true`.

Scheme Procedure: **not** x [¶](#index-not)

C Function: **scm\_not** (x) [¶](#index-scm_005fnot)
:   Return `#t` if x is `#f`, else return `#f`.

Scheme Procedure: **boolean?** obj [¶](#index-boolean_003f)

C Function: **scm\_boolean\_p** (obj) [¶](#index-scm_005fboolean_005fp)
:   Return `#t` if obj is either `#t` or `#f`, else
    return `#f`.

C Macro: `SCM` **SCM\_BOOL\_T** [¶](#index-SCM_005fBOOL_005fT)
:   The `SCM` representation of the Scheme object `#t`.

C Macro: `SCM` **SCM\_BOOL\_F** [¶](#index-SCM_005fBOOL_005fF)
:   The `SCM` representation of the Scheme object `#f`.

C Function: `int` **scm\_is\_true** `(SCM obj)` [¶](#index-scm_005fis_005ftrue)
:   Return `0` if obj is `#f`, else return `1`.

C Function: `int` **scm\_is\_false** `(SCM obj)` [¶](#index-scm_005fis_005ffalse)
:   Return `1` if obj is `#f`, else return `0`.

C Function: `int` **scm\_is\_bool** `(SCM obj)` [¶](#index-scm_005fis_005fbool)
:   Return `1` if obj is either `#t` or `#f`, else
    return `0`.

C Function: `SCM` **scm\_from\_bool** `(int val)` [¶](#index-scm_005ffrom_005fbool)
:   Return `#f` if val is `0`, else return `#t`.

C Function: `int` **scm\_to\_bool** `(SCM val)` [¶](#index-scm_005fto_005fbool)
:   Return `1` if val is `SCM_BOOL_T`, return `0`
    when val is `SCM_BOOL_F`, else signal a ‘wrong type’ error.

    You should probably use `scm_is_true` instead of this function
    when you just want to test a `SCM` value for trueness.

---

Next: [Characters](#Characters), Previous: [Booleans](#Booleans), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2 Numerical data types [¶](#Numerical-data-types)

Guile supports a rich “tower” of numerical types — integer,
rational, real and complex — and provides an extensive set of
mathematical and scientific functions for operating on numerical
data. This section of the manual documents those types and functions.

You may also find it illuminating to read R5RS’s presentation of numbers
in Scheme, which is particularly clear and accessible: see
[Numbers](/r5rs/latest/en/r5rs.html#Numbers) in R5RS.

* [Scheme’s Numerical “Tower”](#Numerical-Tower)
* [Integers](#Integers)
* [Real and Rational Numbers](#Reals-and-Rationals)
* [Complex Numbers](#Complex-Numbers)
* [Exact and Inexact Numbers](#Exactness)
* [Read Syntax for Numerical Data](#Number-Syntax)
* [Operations on Integer Values](#Integer-Operations)
* [Comparison Predicates](#Comparison)
* [Converting Numbers To and From Strings](#Conversion)
* [Complex Number Operations](#Complex)
* [Arithmetic Functions](#Arithmetic)
* [Scientific Functions](#Scientific)
* [Bitwise Operations](#Bitwise-Operations)
* [Random Number Generation](#Random)

---

Next: [Integers](#Integers), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.1 Scheme’s Numerical “Tower” [¶](#Scheme_0027s-Numerical-_0060_0060Tower_0027_0027)

Scheme’s numerical “tower” consists of the following categories of
numbers:

*integers*
:   Whole numbers, positive or negative; e.g. –5, 0, 18.

*rationals*
:   The set of numbers that can be expressed as *p/q*
    where p and q are integers; e.g. *9/16* works, but
    pi (an irrational number) doesn’t. These include integers
    (*n/1*).

*real numbers*
:   The set of numbers that describes all possible positions along a
    one-dimensional line. This includes rationals as well as irrational
    numbers.

*complex numbers*
:   The set of numbers that describes all possible positions in a two
    dimensional space. This includes real as well as imaginary numbers
    (*a+bi*, where a is the *real part*,
    b is the *imaginary part*, and *i* is the square root of
    −1.)

It is called a tower because each category “sits on” the one that
follows it, in the sense that every integer is also a rational, every
rational is also real, and every real number is also a complex number
(but with zero imaginary part).

In addition to the classification into integers, rationals, reals and
complex numbers, Scheme also distinguishes between whether a number is
represented exactly or not. For example, the result of
*2\*sin(pi/4)* is exactly *2^(1/2)*, but Guile
can represent neither *pi/4* nor *2^(1/2)* exactly.
Instead, it stores an inexact approximation, using the C type
`double`.

Guile can represent exact rationals of any magnitude, inexact
rationals that fit into a C `double`, and inexact complex numbers
with `double` real and imaginary parts.

The `number?` predicate may be applied to any Scheme value to
discover whether the value is any of the supported numerical types.

Scheme Procedure: **number?** obj [¶](#index-number_003f)

C Function: **scm\_number\_p** (obj) [¶](#index-scm_005fnumber_005fp)
:   Return `#t` if obj is any kind of number, else `#f`.

For example:

```
(number? 3)
⇒ #t

(number? "hello there!")
⇒ #f

(define pi 3.141592654)
(number? pi)
⇒ #t
```

C Function: `int` **scm\_is\_number** `(SCM obj)` [¶](#index-scm_005fis_005fnumber)
:   This is equivalent to `scm_is_true (scm_number_p (obj))`.

The next few subsections document each of Guile’s numerical data types
in detail.

---

Next: [Real and Rational Numbers](#Reals-and-Rationals), Previous: [Scheme’s Numerical “Tower”](#Numerical-Tower), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.2 Integers [¶](#Integers-1)

Integers are whole numbers, that is numbers with no fractional part,
such as 2, 83, and −3789.

Integers in Guile can be arbitrarily big, as shown by the following
example.

```
(define (factorial n)
  (let loop ((n n) (product 1))
    (if (= n 0)
        product
        (loop (- n 1) (* product n)))))

(factorial 3)
⇒ 6

(factorial 20)
⇒ 2432902008176640000

(- (factorial 45))
⇒ -119622220865480194561963161495657715064383733760000000000
```

Readers whose background is in programming languages where integers are
limited by the need to fit into just 4 or 8 bytes of memory may find
this surprising, or suspect that Guile’s representation of integers is
inefficient. In fact, Guile achieves a near optimal balance of
convenience and efficiency by using the host computer’s native
representation of integers where possible, and a more general
representation where the required number does not fit in the native
form. Conversion between these two representations is automatic and
completely invisible to the Scheme level programmer.

C has a host of different integer types, and Guile offers a host of
functions to convert between them and the `SCM` representation.
For example, a C `int` can be handled with `scm_to_int` and
`scm_from_int`. Guile also defines a few C integer types of its
own, to help with differences between systems.

C integer types that are not covered can be handled with the generic
`scm_to_signed_integer` and `scm_from_signed_integer` for
signed types, or with `scm_to_unsigned_integer` and
`scm_from_unsigned_integer` for unsigned types.

Scheme integers can be exact and inexact. For example, a number
written as `3.0` with an explicit decimal-point is inexact, but
it is also an integer. The functions `integer?` and
`scm_is_integer` report true for such a number, but the functions
`exact-integer?`, `scm_is_exact_integer`,
`scm_is_signed_integer`, and `scm_is_unsigned_integer` only
allow exact integers and thus report false. Likewise, the conversion
functions like `scm_to_signed_integer` only accept exact
integers.

The motivation for this behavior is that the inexactness of a number
should not be lost silently. If you want to allow inexact integers,
you can explicitly insert a call to `inexact->exact` or to its C
equivalent `scm_inexact_to_exact`. (Only inexact integers will
be converted by this call into exact integers; inexact non-integers
will become exact fractions.)

Scheme Procedure: **integer?** x [¶](#index-integer_003f)

C Function: **scm\_integer\_p** (x) [¶](#index-scm_005finteger_005fp)
:   Return `#t` if x is an exact or inexact integer number, else
    return `#f`.

    ```
    (integer? 487)
    ⇒ #t

    (integer? 3.0)
    ⇒ #t

    (integer? -3.4)
    ⇒ #f

    (integer? +inf.0)
    ⇒ #f
    ```

C Function: `int` **scm\_is\_integer** `(SCM x)` [¶](#index-scm_005fis_005finteger)
:   This is equivalent to `scm_is_true (scm_integer_p (x))`.

Scheme Procedure: **exact-integer?** x [¶](#index-exact_002dinteger_003f)

C Function: **scm\_exact\_integer\_p** (x) [¶](#index-scm_005fexact_005finteger_005fp)
:   Return `#t` if x is an exact integer number, else
    return `#f`.

    ```
    (exact-integer? 37)
    ⇒ #t

    (exact-integer? 3.0)
    ⇒ #f
    ```

C Function: `int` **scm\_is\_exact\_integer** `(SCM x)` [¶](#index-scm_005fis_005fexact_005finteger)
:   This is equivalent to `scm_is_true (scm_exact_integer_p (x))`.

C Type: **scm\_t\_int8** [¶](#index-scm_005ft_005fint8)

C Type: **scm\_t\_uint8** [¶](#index-scm_005ft_005fuint8)

C Type: **scm\_t\_int16** [¶](#index-scm_005ft_005fint16)

C Type: **scm\_t\_uint16** [¶](#index-scm_005ft_005fuint16)

C Type: **scm\_t\_int32** [¶](#index-scm_005ft_005fint32)

C Type: **scm\_t\_uint32** [¶](#index-scm_005ft_005fuint32)

C Type: **scm\_t\_int64** [¶](#index-scm_005ft_005fint64)

C Type: **scm\_t\_uint64** [¶](#index-scm_005ft_005fuint64)

C Type: **scm\_t\_intmax** [¶](#index-scm_005ft_005fintmax)

C Type: **scm\_t\_uintmax** [¶](#index-scm_005ft_005fuintmax)
:   The C types are equivalent to the corresponding ISO C types but are
    defined on all platforms, with the exception of `scm_t_int64` and
    `scm_t_uint64`, which are only defined when a 64-bit type is
    available. For example, `scm_t_int8` is equivalent to
    `int8_t`.

    You can regard these definitions as a stop-gap measure until all
    platforms provide these types. If you know that all the platforms
    that you are interested in already provide these types, it is better
    to use them directly instead of the types provided by Guile.

C Function: `int` **scm\_is\_signed\_integer** `(SCM x, scm_t_intmax min, scm_t_intmax max)` [¶](#index-scm_005fis_005fsigned_005finteger)

C Function: `int` **scm\_is\_unsigned\_integer** `(SCM x, scm_t_uintmax min, scm_t_uintmax max)` [¶](#index-scm_005fis_005funsigned_005finteger)
:   Return `1` when x represents an exact integer that is
    between min and max, inclusive.

    These functions can be used to check whether a `SCM` value will
    fit into a given range, such as the range of a given C integer type.
    If you just want to convert a `SCM` value to a given C integer
    type, use one of the conversion functions directly.

C Function: `scm_t_intmax` **scm\_to\_signed\_integer** `(SCM x, scm_t_intmax min, scm_t_intmax max)` [¶](#index-scm_005fto_005fsigned_005finteger)

C Function: `scm_t_uintmax` **scm\_to\_unsigned\_integer** `(SCM x, scm_t_uintmax min, scm_t_uintmax max)` [¶](#index-scm_005fto_005funsigned_005finteger)
:   When x represents an exact integer that is between min and
    max inclusive, return that integer. Else signal an error,
    either a ‘wrong-type’ error when x is not an exact integer, or
    an ‘out-of-range’ error when it doesn’t fit the given range.

C Function: `SCM` **scm\_from\_signed\_integer** `(scm_t_intmax x)` [¶](#index-scm_005ffrom_005fsigned_005finteger)

C Function: `SCM` **scm\_from\_unsigned\_integer** `(scm_t_uintmax x)` [¶](#index-scm_005ffrom_005funsigned_005finteger)
:   Return the `SCM` value that represents the integer x. This
    function will always succeed and will always return an exact number.

C Function: `char` **scm\_to\_char** `(SCM x)` [¶](#index-scm_005fto_005fchar)

C Function: `signed char` **scm\_to\_schar** `(SCM x)` [¶](#index-scm_005fto_005fschar)

C Function: `unsigned char` **scm\_to\_uchar** `(SCM x)` [¶](#index-scm_005fto_005fuchar)

C Function: `short` **scm\_to\_short** `(SCM x)` [¶](#index-scm_005fto_005fshort)

C Function: `unsigned short` **scm\_to\_ushort** `(SCM x)` [¶](#index-scm_005fto_005fushort)

C Function: `int` **scm\_to\_int** `(SCM x)` [¶](#index-scm_005fto_005fint)

C Function: `unsigned int` **scm\_to\_uint** `(SCM x)` [¶](#index-scm_005fto_005fuint)

C Function: `long` **scm\_to\_long** `(SCM x)` [¶](#index-scm_005fto_005flong)

C Function: `unsigned long` **scm\_to\_ulong** `(SCM x)` [¶](#index-scm_005fto_005fulong)

C Function: `long long` **scm\_to\_long\_long** `(SCM x)` [¶](#index-scm_005fto_005flong_005flong)

C Function: `unsigned long long` **scm\_to\_ulong\_long** `(SCM x)` [¶](#index-scm_005fto_005fulong_005flong)

C Function: `size_t` **scm\_to\_size\_t** `(SCM x)` [¶](#index-scm_005fto_005fsize_005ft)

C Function: `ssize_t` **scm\_to\_ssize\_t** `(SCM x)` [¶](#index-scm_005fto_005fssize_005ft)

C Function: `scm_t_uintptr` **scm\_to\_uintptr\_t** `(SCM x)` [¶](#index-scm_005fto_005fuintptr_005ft)

C Function: `scm_t_ptrdiff` **scm\_to\_ptrdiff\_t** `(SCM x)` [¶](#index-scm_005fto_005fptrdiff_005ft)

C Function: `scm_t_int8` **scm\_to\_int8** `(SCM x)` [¶](#index-scm_005fto_005fint8)

C Function: `scm_t_uint8` **scm\_to\_uint8** `(SCM x)` [¶](#index-scm_005fto_005fuint8)

C Function: `scm_t_int16` **scm\_to\_int16** `(SCM x)` [¶](#index-scm_005fto_005fint16)

C Function: `scm_t_uint16` **scm\_to\_uint16** `(SCM x)` [¶](#index-scm_005fto_005fuint16)

C Function: `scm_t_int32` **scm\_to\_int32** `(SCM x)` [¶](#index-scm_005fto_005fint32)

C Function: `scm_t_uint32` **scm\_to\_uint32** `(SCM x)` [¶](#index-scm_005fto_005fuint32)

C Function: `scm_t_int64` **scm\_to\_int64** `(SCM x)` [¶](#index-scm_005fto_005fint64)

C Function: `scm_t_uint64` **scm\_to\_uint64** `(SCM x)` [¶](#index-scm_005fto_005fuint64)

C Function: `scm_t_intmax` **scm\_to\_intmax** `(SCM x)` [¶](#index-scm_005fto_005fintmax)

C Function: `scm_t_uintmax` **scm\_to\_uintmax** `(SCM x)` [¶](#index-scm_005fto_005fuintmax)

C Function: `scm_t_intptr` **scm\_to\_intptr\_t** `(SCM x)` [¶](#index-scm_005fto_005fintptr_005ft)

C Function: `scm_t_uintptr` **scm\_to\_uintptr\_t** `(SCM x)` [¶](#index-scm_005fto_005fuintptr_005ft-1)
:   When x represents an exact integer that fits into the indicated
    C type, return that integer. Else signal an error, either a
    ‘wrong-type’ error when x is not an exact integer, or an
    ‘out-of-range’ error when it doesn’t fit the given range.

    The functions `scm_to_long_long`, `scm_to_ulong_long`,
    `scm_to_int64`, and `scm_to_uint64` are only available when
    the corresponding types are.

C Function: `SCM` **scm\_from\_char** `(char x)` [¶](#index-scm_005ffrom_005fchar)

C Function: `SCM` **scm\_from\_schar** `(signed char x)` [¶](#index-scm_005ffrom_005fschar)

C Function: `SCM` **scm\_from\_uchar** `(unsigned char x)` [¶](#index-scm_005ffrom_005fuchar)

C Function: `SCM` **scm\_from\_short** `(short x)` [¶](#index-scm_005ffrom_005fshort)

C Function: `SCM` **scm\_from\_ushort** `(unsigned short x)` [¶](#index-scm_005ffrom_005fushort)

C Function: `SCM` **scm\_from\_int** `(int x)` [¶](#index-scm_005ffrom_005fint)

C Function: `SCM` **scm\_from\_uint** `(unsigned int x)` [¶](#index-scm_005ffrom_005fuint)

C Function: `SCM` **scm\_from\_long** `(long x)` [¶](#index-scm_005ffrom_005flong)

C Function: `SCM` **scm\_from\_ulong** `(unsigned long x)` [¶](#index-scm_005ffrom_005fulong)

C Function: `SCM` **scm\_from\_long\_long** `(long long x)` [¶](#index-scm_005ffrom_005flong_005flong)

C Function: `SCM` **scm\_from\_ulong\_long** `(unsigned long long x)` [¶](#index-scm_005ffrom_005fulong_005flong)

C Function: `SCM` **scm\_from\_size\_t** `(size_t x)` [¶](#index-scm_005ffrom_005fsize_005ft)

C Function: `SCM` **scm\_from\_ssize\_t** `(ssize_t x)` [¶](#index-scm_005ffrom_005fssize_005ft)

C Function: `SCM` **scm\_from\_uintptr\_t** `(uintptr_t x)` [¶](#index-scm_005ffrom_005fuintptr_005ft)

C Function: `SCM` **scm\_from\_ptrdiff\_t** `(scm_t_ptrdiff x)` [¶](#index-scm_005ffrom_005fptrdiff_005ft)

C Function: `SCM` **scm\_from\_int8** `(scm_t_int8 x)` [¶](#index-scm_005ffrom_005fint8)

C Function: `SCM` **scm\_from\_uint8** `(scm_t_uint8 x)` [¶](#index-scm_005ffrom_005fuint8)

C Function: `SCM` **scm\_from\_int16** `(scm_t_int16 x)` [¶](#index-scm_005ffrom_005fint16)

C Function: `SCM` **scm\_from\_uint16** `(scm_t_uint16 x)` [¶](#index-scm_005ffrom_005fuint16)

C Function: `SCM` **scm\_from\_int32** `(scm_t_int32 x)` [¶](#index-scm_005ffrom_005fint32)

C Function: `SCM` **scm\_from\_uint32** `(scm_t_uint32 x)` [¶](#index-scm_005ffrom_005fuint32)

C Function: `SCM` **scm\_from\_int64** `(scm_t_int64 x)` [¶](#index-scm_005ffrom_005fint64)

C Function: `SCM` **scm\_from\_uint64** `(scm_t_uint64 x)` [¶](#index-scm_005ffrom_005fuint64)

C Function: `SCM` **scm\_from\_intmax** `(scm_t_intmax x)` [¶](#index-scm_005ffrom_005fintmax)

C Function: `SCM` **scm\_from\_uintmax** `(scm_t_uintmax x)` [¶](#index-scm_005ffrom_005fuintmax)

C Function: `SCM` **scm\_from\_intptr\_t** `(scm_t_intptr x)` [¶](#index-scm_005ffrom_005fintptr_005ft)

C Function: `SCM` **scm\_from\_uintptr\_t** `(scm_t_uintptr x)` [¶](#index-scm_005ffrom_005fuintptr_005ft-1)
:   Return the `SCM` value that represents the integer x.
    These functions will always succeed and will always return an exact
    number.

C Function: `void` **scm\_to\_mpz** `(SCM val, mpz_t rop)` [¶](#index-scm_005fto_005fmpz)
:   Assign val to the multiple precision integer rop.
    val must be an exact integer, otherwise an error will be
    signaled. rop must have been initialized with `mpz_init`
    before this function is called. When rop is no longer needed
    the occupied space must be freed with `mpz_clear`.
    See [Initializing Integers](https://www.gmplib.org/manual/Initializing-Integers.html#Initializing-Integers) in GNU MP Manual, for details.

C Function: `SCM` **scm\_from\_mpz** `(mpz_t val)` [¶](#index-scm_005ffrom_005fmpz)
:   Return the `SCM` value that represents val.

---

Next: [Complex Numbers](#Complex-Numbers), Previous: [Integers](#Integers), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.3 Real and Rational Numbers [¶](#Real-and-Rational-Numbers)

Mathematically, the real numbers are the set of numbers that describe
all possible points along a continuous, infinite, one-dimensional line.
The rational numbers are the set of all numbers that can be written as
fractions p/q, where p and q are integers.
All rational numbers are also real, but there are real numbers that
are not rational, for example *the square root of 2*, and
*pi*.

Guile can represent both exact and inexact rational numbers, but it
cannot represent precise finite irrational numbers. Exact rationals are
represented by storing the numerator and denominator as two exact
integers. Inexact rationals are stored as floating point numbers using
the C type `double`.

Exact rationals are written as a fraction of integers. There must be
no whitespace around the slash:

```
1/2
-22/7
```

Even though the actual encoding of inexact rationals is in binary, it
may be helpful to think of it as a decimal number with a limited
number of significant figures and a decimal point somewhere, since
this corresponds to the standard notation for non-whole numbers. For
example:

```
0.34
-0.00000142857931198
-5648394822220000000000.0
4.0
```

The limited precision of Guile’s encoding means that any finite “real”
number in Guile can be written in a rational form, by multiplying and
then dividing by sufficient powers of 10 (or in fact, 2). For example,
‘`-0.00000142857931198`’ is the same as −142857931198 divided
by 100000000000000000. In Guile’s current incarnation, therefore, the
`rational?` and `real?` predicates are equivalent for finite
numbers.

Dividing by an exact zero leads to an error message, as one might
expect. However, dividing by an inexact zero does not produce an error.
Instead, the result of the division is either plus or minus infinity,
depending on the sign of the divided number and the sign of the zero
divisor (some platforms support signed zeroes ‘`-0.0`’ and
‘`+0.0`’; ‘`0.0`’ is the same as ‘`+0.0`’).

Dividing zero by an inexact zero yields a NaN (‘not a number’)
value, although they are actually considered numbers by Scheme.
Attempts to compare a NaN value with any number (including
itself) using `=`, `<`, `>`, `<=` or `>=`
always returns `#f`. Although a NaN value is not
`=` to itself, it is both `eqv?` and `equal?` to itself
and other NaN values. However, the preferred way to test for
them is by using `nan?`.

The real NaN values and infinities are written ‘`+nan.0`’,
‘`+inf.0`’ and ‘`-inf.0`’. This syntax is also recognized by
`read` as an extension to the usual Scheme syntax. These special
values are considered by Scheme to be inexact real numbers but not
rational. Note that non-real complex numbers may also contain
infinities or NaN values in their real or imaginary parts. To
test a real number to see if it is infinite, a NaN value, or
neither, use `inf?`, `nan?`, or `finite?`, respectively.
Every real number in Scheme belongs to precisely one of those three
classes.

On platforms that follow IEEE 754 for their floating point
arithmetic, the ‘`+inf.0`’, ‘`-inf.0`’, and ‘`+nan.0`’ values
are implemented using the corresponding IEEE 754 values.
They behave in arithmetic operations like IEEE 754 describes
it, i.e., `(= +nan.0 +nan.0)` ⇒ `#f`.

Scheme Procedure: **real?** obj [¶](#index-real_003f)

C Function: **scm\_real\_p** (obj) [¶](#index-scm_005freal_005fp)
:   Return `#t` if obj is a real number, else `#f`. Note
    that the sets of integer and rational values form subsets of the set
    of real numbers, so the predicate will also be fulfilled if obj
    is an integer number or a rational number.

Scheme Procedure: **rational?** x [¶](#index-rational_003f)

C Function: **scm\_rational\_p** (x) [¶](#index-scm_005frational_005fp)
:   Return `#t` if x is a rational number, `#f` otherwise.
    Note that the set of integer values forms a subset of the set of
    rational numbers, i.e. the predicate will also be fulfilled if
    x is an integer number.

Scheme Procedure: **rationalize** x eps [¶](#index-rationalize)

C Function: **scm\_rationalize** (x, eps) [¶](#index-scm_005frationalize)
:   Returns the *simplest* rational number differing
    from x by no more than eps.

    As required by R5RS, `rationalize` only returns an
    exact result when both its arguments are exact. Thus, you might need
    to use `inexact->exact` on the arguments.

    ```
    (rationalize (inexact->exact 1.2) 1/100)
    ⇒ 6/5
    ```

Scheme Procedure: **inf?** x [¶](#index-inf_003f)

C Function: **scm\_inf\_p** (x) [¶](#index-scm_005finf_005fp)
:   Return `#t` if the real number x is ‘`+inf.0`’ or
    ‘`-inf.0`’. Otherwise return `#f`.

Scheme Procedure: **nan?** x [¶](#index-nan_003f)

C Function: **scm\_nan\_p** (x) [¶](#index-scm_005fnan_005fp)
:   Return `#t` if the real number x is ‘`+nan.0`’, or
    `#f` otherwise.

Scheme Procedure: **finite?** x [¶](#index-finite_003f)

C Function: **scm\_finite\_p** (x) [¶](#index-scm_005ffinite_005fp)
:   Return `#t` if the real number x is neither infinite nor a
    NaN, `#f` otherwise.

Scheme Procedure: **nan** [¶](#index-nan)

C Function: **scm\_nan** () [¶](#index-scm_005fnan)
:   Return ‘`+nan.0`’, a NaN value.

Scheme Procedure: **inf** [¶](#index-inf)

C Function: **scm\_inf** () [¶](#index-scm_005finf)
:   Return ‘`+inf.0`’, positive infinity.

Scheme Procedure: **numerator** x [¶](#index-numerator)

C Function: **scm\_numerator** (x) [¶](#index-scm_005fnumerator)
:   Return the numerator of the rational number x.

Scheme Procedure: **denominator** x [¶](#index-denominator)

C Function: **scm\_denominator** (x) [¶](#index-scm_005fdenominator)
:   Return the denominator of the rational number x.

C Function: `int` **scm\_is\_real** `(SCM val)` [¶](#index-scm_005fis_005freal)

C Function: `int` **scm\_is\_rational** `(SCM val)` [¶](#index-scm_005fis_005frational)
:   Equivalent to `scm_is_true (scm_real_p (val))` and
    `scm_is_true (scm_rational_p (val))`, respectively.

C Function: `double` **scm\_to\_double** `(SCM val)` [¶](#index-scm_005fto_005fdouble)
:   Returns the number closest to val that is representable as a
    `double`. Returns infinity for a val that is too large in
    magnitude. The argument val must be a real number.

C Function: `SCM` **scm\_from\_double** `(double val)` [¶](#index-scm_005ffrom_005fdouble)
:   Return the `SCM` value that represents val. The returned
    value is inexact according to the predicate `inexact?`, but it
    will be exactly equal to val.

---

Next: [Exact and Inexact Numbers](#Exactness), Previous: [Real and Rational Numbers](#Reals-and-Rationals), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.4 Complex Numbers [¶](#Complex-Numbers-1)

Complex numbers are the set of numbers that describe all possible points
in a two-dimensional space. The two coordinates of a particular point
in this space are known as the *real* and *imaginary* parts of
the complex number that describes that point.

In Guile, complex numbers are written in rectangular form as the sum of
their real and imaginary parts, using the symbol `i` to indicate
the imaginary part.

```
3+4i
⇒
3.0+4.0i

(* 3-8i 2.3+0.3i)
⇒
9.3-17.5i
```

Polar form can also be used, with an ‘`@`’ between magnitude and
angle,

```
1@3.141592 ⇒ -1.0      (approx)
-1@1.57079 ⇒ 0.0-1.0i  (approx)
```

Guile represents a complex number as a pair of inexact reals, so the
real and imaginary parts of a complex number have the same properties of
inexactness and limited precision as single inexact real numbers.

Note that each part of a complex number may contain any inexact real
value, including the special values ‘`+nan.0`’, ‘`+inf.0`’ and
‘`-inf.0`’, as well as either of the signed zeroes ‘`0.0`’ or
‘`-0.0`’.

Scheme Procedure: **complex?** z [¶](#index-complex_003f)

C Function: **scm\_complex\_p** (z) [¶](#index-scm_005fcomplex_005fp)
:   Return `#t` if z is a complex number, `#f`
    otherwise. Note that the sets of real, rational and integer
    values form subsets of the set of complex numbers, i.e. the
    predicate will also be fulfilled if z is a real,
    rational or integer number.

C Function: `int` **scm\_is\_complex** `(SCM val)` [¶](#index-scm_005fis_005fcomplex)
:   Equivalent to `scm_is_true (scm_complex_p (val))`.

---

Next: [Read Syntax for Numerical Data](#Number-Syntax), Previous: [Complex Numbers](#Complex-Numbers), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.5 Exact and Inexact Numbers [¶](#Exact-and-Inexact-Numbers)

R5RS requires that, with few exceptions, a calculation involving inexact
numbers always produces an inexact result. To meet this requirement,
Guile distinguishes between an exact integer value such as ‘`5`’ and
the corresponding inexact integer value which, to the limited precision
available, has no fractional part, and is printed as ‘`5.0`’. Guile
will only convert the latter value to the former when forced to do so by
an invocation of the `inexact->exact` procedure.

The only exception to the above requirement is when the values of the
inexact numbers do not affect the result. For example `(expt n 0)`
is ‘`1`’ for any value of `n`, therefore `(expt 5.0 0)` is
permitted to return an exact ‘`1`’.

Scheme Procedure: **exact?** z [¶](#index-exact_003f)

C Function: **scm\_exact\_p** (z) [¶](#index-scm_005fexact_005fp)
:   Return `#t` if the number z is exact, `#f`
    otherwise.

    ```
    (exact? 2)
    ⇒ #t

    (exact? 0.5)
    ⇒ #f

    (exact? (/ 2))
    ⇒ #t
    ```

C Function: `int` **scm\_is\_exact** `(SCM z)` [¶](#index-scm_005fis_005fexact)
:   Return a `1` if the number z is exact, and `0`
    otherwise. This is equivalent to `scm_is_true (scm_exact_p (z))`.

    An alternate approach to testing the exactness of a number is to
    use `scm_is_signed_integer` or `scm_is_unsigned_integer`.

Scheme Procedure: **inexact?** z [¶](#index-inexact_003f)

C Function: **scm\_inexact\_p** (z) [¶](#index-scm_005finexact_005fp)
:   Return `#t` if the number z is inexact, `#f`
    else.

C Function: `int` **scm\_is\_inexact** `(SCM z)` [¶](#index-scm_005fis_005finexact)
:   Return a `1` if the number z is inexact, and `0`
    otherwise. This is equivalent to `scm_is_true (scm_inexact_p (z))`.

Scheme Procedure: **inexact->exact** z [¶](#index-inexact_002d_003eexact)

C Function: **scm\_inexact\_to\_exact** (z) [¶](#index-scm_005finexact_005fto_005fexact)
:   Return an exact number that is numerically closest to z, when
    there is one. For inexact rationals, Guile returns the exact rational
    that is numerically equal to the inexact rational. Inexact complex
    numbers with a non-zero imaginary part can not be made exact.

    ```
    (inexact->exact 0.5)
    ⇒ 1/2
    ```

    The following happens because 12/10 is not exactly representable as a
    `double` (on most platforms). However, when reading a decimal
    number that has been marked exact with the “#e” prefix, Guile is
    able to represent it correctly.

    ```
    (inexact->exact 1.2)
    ⇒ 5404319552844595/4503599627370496

    #e1.2
    ⇒ 6/5
    ```

Scheme Procedure: **exact->inexact** z [¶](#index-exact_002d_003einexact)

C Function: **scm\_exact\_to\_inexact** (z) [¶](#index-scm_005fexact_005fto_005finexact)
:   Convert the number z to its inexact representation.

---

Next: [Operations on Integer Values](#Integer-Operations), Previous: [Exact and Inexact Numbers](#Exactness), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.6 Read Syntax for Numerical Data [¶](#Read-Syntax-for-Numerical-Data)

The read syntax for integers is a string of digits, optionally
preceded by a minus or plus character, a code indicating the
base in which the integer is encoded, and a code indicating whether
the number is exact or inexact. The supported base codes are:

`#b`

`#B`
:   the integer is written in binary (base 2)

`#o`

`#O`
:   the integer is written in octal (base 8)

`#d`

`#D`
:   the integer is written in decimal (base 10)

`#x`

`#X`
:   the integer is written in hexadecimal (base 16)

If the base code is omitted, the integer is assumed to be decimal. The
following examples show how these base codes are used.

```
-13
⇒ -13

#d-13
⇒ -13

#x-13
⇒ -19

#b+1101
⇒ 13

#o377
⇒ 255
```

The codes for indicating exactness (which can, incidentally, be applied
to all numerical values) are:

`#e`

`#E`
:   the number is exact

`#i`

`#I`
:   the number is inexact.

If the exactness indicator is omitted, the number is exact unless it
contains a radix point. Since Guile can not represent exact complex
numbers, an error is signaled when asking for them.

```
(exact? 1.2)
⇒ #f

(exact? #e1.2)
⇒ #t

(exact? #e+1i)
ERROR: Wrong type argument
```

Guile also understands the syntax ‘`+inf.0`’ and ‘`-inf.0`’ for
plus and minus infinity, respectively. The value must be written
exactly as shown, that is, they always must have a sign and exactly
one zero digit after the decimal point. It also understands
‘`+nan.0`’ and ‘`-nan.0`’ for the special ‘not-a-number’ value.
The sign is ignored for ‘not-a-number’ and the value is always printed
as ‘`+nan.0`’.

---

Next: [Comparison Predicates](#Comparison), Previous: [Read Syntax for Numerical Data](#Number-Syntax), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.7 Operations on Integer Values [¶](#Operations-on-Integer-Values)

Scheme Procedure: **odd?** n [¶](#index-odd_003f)

C Function: **scm\_odd\_p** (n) [¶](#index-scm_005fodd_005fp)
:   Return `#t` if n is an odd number, `#f`
    otherwise.

Scheme Procedure: **even?** n [¶](#index-even_003f)

C Function: **scm\_even\_p** (n) [¶](#index-scm_005feven_005fp)
:   Return `#t` if n is an even number, `#f`
    otherwise.

Scheme Procedure: **quotient** n d [¶](#index-quotient)

Scheme Procedure: **remainder** n d [¶](#index-remainder)

C Function: **scm\_quotient** (n, d) [¶](#index-scm_005fquotient)

C Function: **scm\_remainder** (n, d) [¶](#index-scm_005fremainder)
:   Return the quotient or remainder from n divided by d. The
    quotient is rounded towards zero, and the remainder will have the same
    sign as n. In all cases quotient and remainder satisfy
    *n = q\*d + r*.

    ```
    (remainder 13 4) ⇒ 1
    (remainder -13 4) ⇒ -1
    ```

    See also `truncate-quotient`, `truncate-remainder` and
    related operations in [Arithmetic Functions](#Arithmetic).

Scheme Procedure: **modulo** n d [¶](#index-modulo)

C Function: **scm\_modulo** (n, d) [¶](#index-scm_005fmodulo)
:   Return the remainder from n divided by d, with the same
    sign as d.

    ```
    (modulo 13 4) ⇒ 1
    (modulo -13 4) ⇒ 3
    (modulo 13 -4) ⇒ -3
    (modulo -13 -4) ⇒ -1
    ```

    See also `floor-quotient`, `floor-remainder` and
    related operations in [Arithmetic Functions](#Arithmetic).

Scheme Procedure: **gcd** x… [¶](#index-gcd)

C Function: **scm\_gcd** (x, y) [¶](#index-scm_005fgcd)
:   Return the greatest common divisor of all arguments.
    If called without arguments, 0 is returned.

    The C function `scm_gcd` always takes two arguments, while the
    Scheme function can take an arbitrary number.

Scheme Procedure: **lcm** x… [¶](#index-lcm)

C Function: **scm\_lcm** (x, y) [¶](#index-scm_005flcm)
:   Return the least common multiple of the arguments.
    If called without arguments, 1 is returned.

    The C function `scm_lcm` always takes two arguments, while the
    Scheme function can take an arbitrary number.

Scheme Procedure: **modulo-expt** n k m [¶](#index-modulo_002dexpt)

C Function: **scm\_modulo\_expt** (n, k, m) [¶](#index-scm_005fmodulo_005fexpt)
:   Return n raised to the integer exponent
    k, modulo m.

    ```
    (modulo-expt 2 3 5)
       ⇒ 3
    ```

Scheme Procedure: **exact-integer-sqrt** `k` [¶](#index-exact_002dinteger_002dsqrt)

C Function: `void` **scm\_exact\_integer\_sqrt** `(SCM k, SCM *s, SCM *r)` [¶](#index-scm_005fexact_005finteger_005fsqrt)
:   Return two exact non-negative integers s and r
    such that *k = s^2 + r* and
    *s^2 <= k < (s + 1)^2*.
    An error is raised if k is not an exact non-negative integer.

    ```
    (exact-integer-sqrt 10) ⇒ 3 and 1
    ```

---

Next: [Converting Numbers To and From Strings](#Conversion), Previous: [Operations on Integer Values](#Integer-Operations), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.8 Comparison Predicates [¶](#Comparison-Predicates)

The C comparison functions below always takes two arguments, while the
Scheme functions can take an arbitrary number. Also keep in mind that
the C functions return one of the Scheme boolean values
`SCM_BOOL_T` or `SCM_BOOL_F` which are both true as far as C
is concerned. Thus, always write `scm_is_true (scm_num_eq_p (x,
y))` when testing the two Scheme numbers `x` and `y` for
equality, for example.

Scheme Procedure: **=** [¶](#index-_003d)

C Function: **scm\_num\_eq\_p** (x, y) [¶](#index-scm_005fnum_005feq_005fp)
:   Return `#t` if all parameters are numerically equal.

Scheme Procedure: **<** [¶](#index-_003c)

C Function: **scm\_less\_p** (x, y) [¶](#index-scm_005fless_005fp)
:   Return `#t` if the list of parameters is monotonically
    increasing.

Scheme Procedure: **>** [¶](#index-_003e)

C Function: **scm\_gr\_p** (x, y) [¶](#index-scm_005fgr_005fp)
:   Return `#t` if the list of parameters is monotonically
    decreasing.

Scheme Procedure: **<=** [¶](#index-_003c_003d)

C Function: **scm\_leq\_p** (x, y) [¶](#index-scm_005fleq_005fp)
:   Return `#t` if the list of parameters is monotonically
    non-decreasing.

Scheme Procedure: **>=** [¶](#index-_003e_003d)

C Function: **scm\_geq\_p** (x, y) [¶](#index-scm_005fgeq_005fp)
:   Return `#t` if the list of parameters is monotonically
    non-increasing.

Scheme Procedure: **zero?** z [¶](#index-zero_003f)

C Function: **scm\_zero\_p** (z) [¶](#index-scm_005fzero_005fp)
:   Return `#t` if z is an exact or inexact number equal to
    zero.

Scheme Procedure: **positive?** x [¶](#index-positive_003f)

C Function: **scm\_positive\_p** (x) [¶](#index-scm_005fpositive_005fp)
:   Return `#t` if x is an exact or inexact number greater than
    zero.

Scheme Procedure: **negative?** x [¶](#index-negative_003f)

C Function: **scm\_negative\_p** (x) [¶](#index-scm_005fnegative_005fp)
:   Return `#t` if x is an exact or inexact number less than
    zero.

---

Next: [Complex Number Operations](#Complex), Previous: [Comparison Predicates](#Comparison), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.9 Converting Numbers To and From Strings [¶](#Converting-Numbers-To-and-From-Strings)

The following procedures read and write numbers according to their
external representation as defined by R5RS (see [R5RS Lexical Structure](/r5rs/latest/en/r5rs.html#Lexical-structure) in The Revised^5 Report on the Algorithmic
Language Scheme). See [the `(ice-9
i18n)` module](#Number-Input-and-Output), for locale-dependent number parsing.

Scheme Procedure: **number->string** n [radix] [¶](#index-number_002d_003estring)

C Function: **scm\_number\_to\_string** (n, radix) [¶](#index-scm_005fnumber_005fto_005fstring)
:   Return a string holding the external representation of the
    number n in the given radix. If n is
    inexact, a radix of 10 will be used.

Scheme Procedure: **string->number** string [radix] [¶](#index-string_002d_003enumber)

C Function: **scm\_string\_to\_number** (string, radix) [¶](#index-scm_005fstring_005fto_005fnumber)
:   Return a number of the maximally precise representation
    expressed by the given string. radix must be an
    exact integer, either 2, 8, 10, or 16. If supplied, radix
    is a default radix that may be overridden by an explicit radix
    prefix in string (e.g. "#o177"). If radix is not
    supplied, then the default radix is 10. If string is not a
    syntactically valid notation for a number, then
    `string->number` returns `#f`.

C Function: `SCM` **scm\_c\_locale\_stringn\_to\_number** `(const char *string, size_t len, unsigned radix)` [¶](#index-scm_005fc_005flocale_005fstringn_005fto_005fnumber)
:   As per `string->number` above, but taking a C string, as pointer
    and length. The string characters should be in the current locale
    encoding (`locale` in the name refers only to that, there’s no
    locale-dependent parsing).

---

Next: [Arithmetic Functions](#Arithmetic), Previous: [Converting Numbers To and From Strings](#Conversion), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.10 Complex Number Operations [¶](#Complex-Number-Operations)

Scheme Procedure: **make-rectangular** real\_part imaginary\_part [¶](#index-make_002drectangular)

C Function: **scm\_make\_rectangular** (real\_part, imaginary\_part) [¶](#index-scm_005fmake_005frectangular)
:   Return a complex number constructed of the given real-part and imaginary-part parts.

Scheme Procedure: **make-polar** mag ang [¶](#index-make_002dpolar)

C Function: **scm\_make\_polar** (mag, ang) [¶](#index-scm_005fmake_005fpolar)
:   Return the complex number mag \* e^(i \* ang).

Scheme Procedure: **real-part** z [¶](#index-real_002dpart)

C Function: **scm\_real\_part** (z) [¶](#index-scm_005freal_005fpart)
:   Return the real part of the number z.

Scheme Procedure: **imag-part** z [¶](#index-imag_002dpart)

C Function: **scm\_imag\_part** (z) [¶](#index-scm_005fimag_005fpart)
:   Return the imaginary part of the number z.

Scheme Procedure: **magnitude** z [¶](#index-magnitude)

C Function: **scm\_magnitude** (z) [¶](#index-scm_005fmagnitude)
:   Return the magnitude of the number z. This is the same as
    `abs` for real arguments, but also allows complex numbers.

Scheme Procedure: **angle** z [¶](#index-angle)

C Function: **scm\_angle** (z) [¶](#index-scm_005fangle)
:   Return the angle of the complex number z.

C Function: `SCM` **scm\_c\_make\_rectangular** `(double re, double im)` [¶](#index-scm_005fc_005fmake_005frectangular)

C Function: `SCM` **scm\_c\_make\_polar** `(double x, double y)` [¶](#index-scm_005fc_005fmake_005fpolar)
:   Like `scm_make_rectangular` or `scm_make_polar`,
    respectively, but these functions take `double`s as their
    arguments.

C Function: `double` **scm\_c\_real\_part** `(z)` [¶](#index-scm_005fc_005freal_005fpart)

C Function: `double` **scm\_c\_imag\_part** `(z)` [¶](#index-scm_005fc_005fimag_005fpart)
:   Returns the real or imaginary part of z as a `double`.

C Function: `double` **scm\_c\_magnitude** `(z)` [¶](#index-scm_005fc_005fmagnitude)

C Function: `double` **scm\_c\_angle** `(z)` [¶](#index-scm_005fc_005fangle)
:   Returns the magnitude or angle of z as a `double`.

---

Next: [Scientific Functions](#Scientific), Previous: [Complex Number Operations](#Complex), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.11 Arithmetic Functions [¶](#Arithmetic-Functions)

The C arithmetic functions below always takes two arguments, while the
Scheme functions can take an arbitrary number. When you need to
invoke them with just one argument, for example to compute the
equivalent of `(- x)`, pass `SCM_UNDEFINED` as the second
one: `scm_difference (x, SCM_UNDEFINED)`.

Scheme Procedure: **+** z1 … [¶](#index-_002b)

C Function: **scm\_sum** (z1, z2) [¶](#index-scm_005fsum)
:   Return the sum of all parameter values. Return 0 if called without any
    parameters.

Scheme Procedure: **-** z1 z2 … [¶](#index-_002d)

C Function: **scm\_difference** (z1, z2) [¶](#index-scm_005fdifference)
:   If called with one argument z1, -z1 is returned. Otherwise
    the sum of all but the first argument are subtracted from the first
    argument.

Scheme Procedure: **\*** z1 … [¶](#index-_002a)

C Function: **scm\_product** (z1, z2) [¶](#index-scm_005fproduct)
:   Return the product of all arguments. If called without arguments, 1 is
    returned.

Scheme Procedure: **/** z1 z2 … [¶](#index-_002f)

C Function: **scm\_divide** (z1, z2) [¶](#index-scm_005fdivide)
:   Divide the first argument by the product of the remaining arguments. If
    called with one argument z1, 1/z1 is returned.

Scheme Procedure: **1+** z [¶](#index-1_002b-1)

C Function: **scm\_oneplus** (z) [¶](#index-scm_005foneplus)
:   Return *z + 1*.

Scheme Procedure: **1-** z [¶](#index-1_002d-1)

C function: **scm\_oneminus** (z) [¶](#index-scm_005foneminus)
:   Return *z - 1*.

Scheme Procedure: **abs** x [¶](#index-abs)

C Function: **scm\_abs** (x) [¶](#index-scm_005fabs)
:   Return the absolute value of x.

    x must be a number with zero imaginary part. To calculate the
    magnitude of a complex number, use `magnitude` instead.

Scheme Procedure: **max** x1 x2 … [¶](#index-max)

C Function: **scm\_max** (x1, x2) [¶](#index-scm_005fmax)
:   Return the maximum of all parameter values.

Scheme Procedure: **min** x1 x2 … [¶](#index-min)

C Function: **scm\_min** (x1, x2) [¶](#index-scm_005fmin)
:   Return the minimum of all parameter values.

Scheme Procedure: **truncate** x [¶](#index-truncate)

C Function: **scm\_truncate\_number** (x) [¶](#index-scm_005ftruncate_005fnumber)
:   Round the inexact number x towards zero.

Scheme Procedure: **round** x [¶](#index-round)

C Function: **scm\_round\_number** (x) [¶](#index-scm_005fround_005fnumber)
:   Round the inexact number x to the nearest integer. When exactly
    halfway between two integers, round to the even one.

Scheme Procedure: **floor** x [¶](#index-floor)

C Function: **scm\_floor** (x) [¶](#index-scm_005ffloor)
:   Round the number x towards minus infinity.

Scheme Procedure: **ceiling** x [¶](#index-ceiling)

C Function: **scm\_ceiling** (x) [¶](#index-scm_005fceiling)
:   Round the number x towards infinity.

C Function: `double` **scm\_c\_truncate** `(double x)` [¶](#index-scm_005fc_005ftruncate)

C Function: `double` **scm\_c\_round** `(double x)` [¶](#index-scm_005fc_005fround)
:   Like `scm_truncate_number` or `scm_round_number`,
    respectively, but these functions take and return `double`
    values.

Scheme Procedure: **euclidean/** `x y` [¶](#index-euclidean_002f)

Scheme Procedure: **euclidean-quotient** `x y` [¶](#index-euclidean_002dquotient)

Scheme Procedure: **euclidean-remainder** `x y` [¶](#index-euclidean_002dremainder)

C Function: `void` **scm\_euclidean\_divide** `(SCM x, SCM y, SCM *q, SCM *r)` [¶](#index-scm_005feuclidean_005fdivide)

C Function: `SCM` **scm\_euclidean\_quotient** `(SCM x, SCM y)` [¶](#index-scm_005feuclidean_005fquotient)

C Function: `SCM` **scm\_euclidean\_remainder** `(SCM x, SCM y)` [¶](#index-scm_005feuclidean_005fremainder)
:   These procedures accept two real numbers x and y, where the
    divisor y must be non-zero. `euclidean-quotient` returns the
    integer q and `euclidean-remainder` returns the real number
    r such that *x = q\*y + r* and
    *0 <= r < |y|*. `euclidean/` returns both q and
    r, and is more efficient than computing each separately. Note
    that when *y > 0*, `euclidean-quotient` returns
    *floor(x/y)*, otherwise it returns
    *ceiling(x/y)*.

    Note that these operators are equivalent to the R6RS operators
    `div`, `mod`, and `div-and-mod`.

    ```
    (euclidean-quotient 123 10) ⇒ 12
    (euclidean-remainder 123 10) ⇒ 3
    (euclidean/ 123 10) ⇒ 12 and 3
    (euclidean/ 123 -10) ⇒ -12 and 3
    (euclidean/ -123 10) ⇒ -13 and 7
    (euclidean/ -123 -10) ⇒ 13 and 7
    (euclidean/ -123.2 -63.5) ⇒ 2.0 and 3.8
    (euclidean/ 16/3 -10/7) ⇒ -3 and 22/21
    ```

Scheme Procedure: **floor/** `x y` [¶](#index-floor_002f)

Scheme Procedure: **floor-quotient** `x y` [¶](#index-floor_002dquotient)

Scheme Procedure: **floor-remainder** `x y` [¶](#index-floor_002dremainder)

C Function: `void` **scm\_floor\_divide** `(SCM x, SCM y, SCM *q, SCM *r)` [¶](#index-scm_005ffloor_005fdivide)

C Function: `SCM` **scm\_floor\_quotient** `(x, y)` [¶](#index-scm_005ffloor_005fquotient)

C Function: `SCM` **scm\_floor\_remainder** `(x, y)` [¶](#index-scm_005ffloor_005fremainder)
:   These procedures accept two real numbers x and y, where the
    divisor y must be non-zero. `floor-quotient` returns the
    integer q and `floor-remainder` returns the real number
    r such that *q = floor(x/y)* and
    *x = q\*y + r*. `floor/` returns
    both q and r, and is more efficient than computing each
    separately. Note that r, if non-zero, will have the same sign
    as y.

    When x and y are integers, `floor-remainder` is
    equivalent to the R5RS integer-only operator `modulo`.

    ```
    (floor-quotient 123 10) ⇒ 12
    (floor-remainder 123 10) ⇒ 3
    (floor/ 123 10) ⇒ 12 and 3
    (floor/ 123 -10) ⇒ -13 and -7
    (floor/ -123 10) ⇒ -13 and 7
    (floor/ -123 -10) ⇒ 12 and -3
    (floor/ -123.2 -63.5) ⇒ 1.0 and -59.7
    (floor/ 16/3 -10/7) ⇒ -4 and -8/21
    ```

Scheme Procedure: **ceiling/** `x y` [¶](#index-ceiling_002f)

Scheme Procedure: **ceiling-quotient** `x y` [¶](#index-ceiling_002dquotient)

Scheme Procedure: **ceiling-remainder** `x y` [¶](#index-ceiling_002dremainder)

C Function: `void` **scm\_ceiling\_divide** `(SCM x, SCM y, SCM *q, SCM *r)` [¶](#index-scm_005fceiling_005fdivide)

C Function: `SCM` **scm\_ceiling\_quotient** `(x, y)` [¶](#index-scm_005fceiling_005fquotient)

C Function: `SCM` **scm\_ceiling\_remainder** `(x, y)` [¶](#index-scm_005fceiling_005fremainder)
:   These procedures accept two real numbers x and y, where the
    divisor y must be non-zero. `ceiling-quotient` returns the
    integer q and `ceiling-remainder` returns the real number
    r such that *q = ceiling(x/y)* and
    *x = q\*y + r*. `ceiling/` returns
    both q and r, and is more efficient than computing each
    separately. Note that r, if non-zero, will have the opposite sign
    of y.

    ```
    (ceiling-quotient 123 10) ⇒ 13
    (ceiling-remainder 123 10) ⇒ -7
    (ceiling/ 123 10) ⇒ 13 and -7
    (ceiling/ 123 -10) ⇒ -12 and 3
    (ceiling/ -123 10) ⇒ -12 and -3
    (ceiling/ -123 -10) ⇒ 13 and 7
    (ceiling/ -123.2 -63.5) ⇒ 2.0 and 3.8
    (ceiling/ 16/3 -10/7) ⇒ -3 and 22/21
    ```

Scheme Procedure: **truncate/** `x y` [¶](#index-truncate_002f)

Scheme Procedure: **truncate-quotient** `x y` [¶](#index-truncate_002dquotient)

Scheme Procedure: **truncate-remainder** `x y` [¶](#index-truncate_002dremainder)

C Function: `void` **scm\_truncate\_divide** `(SCM x, SCM y, SCM *q, SCM *r)` [¶](#index-scm_005ftruncate_005fdivide)

C Function: `SCM` **scm\_truncate\_quotient** `(x, y)` [¶](#index-scm_005ftruncate_005fquotient)

C Function: `SCM` **scm\_truncate\_remainder** `(x, y)` [¶](#index-scm_005ftruncate_005fremainder)
:   These procedures accept two real numbers x and y, where the
    divisor y must be non-zero. `truncate-quotient` returns the
    integer q and `truncate-remainder` returns the real number
    r such that q is *x/y* rounded toward zero,
    and *x = q\*y + r*. `truncate/` returns
    both q and r, and is more efficient than computing each
    separately. Note that r, if non-zero, will have the same sign
    as x.

    When x and y are integers, these operators are
    equivalent to the R5RS integer-only operators `quotient` and
    `remainder`.

    ```
    (truncate-quotient 123 10) ⇒ 12
    (truncate-remainder 123 10) ⇒ 3
    (truncate/ 123 10) ⇒ 12 and 3
    (truncate/ 123 -10) ⇒ -12 and 3
    (truncate/ -123 10) ⇒ -12 and -3
    (truncate/ -123 -10) ⇒ 12 and -3
    (truncate/ -123.2 -63.5) ⇒ 1.0 and -59.7
    (truncate/ 16/3 -10/7) ⇒ -3 and 22/21
    ```

Scheme Procedure: **centered/** `x y` [¶](#index-centered_002f)

Scheme Procedure: **centered-quotient** `x y` [¶](#index-centered_002dquotient)

Scheme Procedure: **centered-remainder** `x y` [¶](#index-centered_002dremainder)

C Function: `void` **scm\_centered\_divide** `(SCM x, SCM y, SCM *q, SCM *r)` [¶](#index-scm_005fcentered_005fdivide)

C Function: `SCM` **scm\_centered\_quotient** `(SCM x, SCM y)` [¶](#index-scm_005fcentered_005fquotient)

C Function: `SCM` **scm\_centered\_remainder** `(SCM x, SCM y)` [¶](#index-scm_005fcentered_005fremainder)
:   These procedures accept two real numbers x and y, where the
    divisor y must be non-zero. `centered-quotient` returns the
    integer q and `centered-remainder` returns the real number
    r such that *x = q\*y + r* and
    *-|y/2| <= r < |y/2|*. `centered/`
    returns both q and r, and is more efficient than computing
    each separately.

    Note that `centered-quotient` returns *x/y*
    rounded to the nearest integer. When *x/y* lies
    exactly half-way between two integers, the tie is broken according to
    the sign of y. If *y > 0*, ties are rounded toward
    positive infinity, otherwise they are rounded toward negative infinity.
    This is a consequence of the requirement that
    *-|y/2| <= r < |y/2|*.

    Note that these operators are equivalent to the R6RS operators
    `div0`, `mod0`, and `div0-and-mod0`.

    ```
    (centered-quotient 123 10) ⇒ 12
    (centered-remainder 123 10) ⇒ 3
    (centered/ 123 10) ⇒ 12 and 3
    (centered/ 123 -10) ⇒ -12 and 3
    (centered/ -123 10) ⇒ -12 and -3
    (centered/ -123 -10) ⇒ 12 and -3
    (centered/ 125 10) ⇒ 13 and -5
    (centered/ 127 10) ⇒ 13 and -3
    (centered/ 135 10) ⇒ 14 and -5
    (centered/ -123.2 -63.5) ⇒ 2.0 and 3.8
    (centered/ 16/3 -10/7) ⇒ -4 and -8/21
    ```

Scheme Procedure: **round/** `x y` [¶](#index-round_002f)

Scheme Procedure: **round-quotient** `x y` [¶](#index-round_002dquotient)

Scheme Procedure: **round-remainder** `x y` [¶](#index-round_002dremainder)

C Function: `void` **scm\_round\_divide** `(SCM x, SCM y, SCM *q, SCM *r)` [¶](#index-scm_005fround_005fdivide)

C Function: `SCM` **scm\_round\_quotient** `(x, y)` [¶](#index-scm_005fround_005fquotient)

C Function: `SCM` **scm\_round\_remainder** `(x, y)` [¶](#index-scm_005fround_005fremainder)
:   These procedures accept two real numbers x and y, where the
    divisor y must be non-zero. `round-quotient` returns the
    integer q and `round-remainder` returns the real number
    r such that *x = q\*y + r* and
    q is *x/y* rounded to the nearest integer,
    with ties going to the nearest even integer. `round/`
    returns both q and r, and is more efficient than computing
    each separately.

    Note that `round/` and `centered/` are almost equivalent, but
    their behavior differs when *x/y* lies exactly half-way
    between two integers. In this case, `round/` chooses the nearest
    even integer, whereas `centered/` chooses in such a way to satisfy
    the constraint *-|y/2| <= r < |y/2|*, which
    is stronger than the corresponding constraint for `round/`,
    *-|y/2| <= r <= |y/2|*. In particular,
    when x and y are integers, the number of possible remainders
    returned by `centered/` is *|y|*, whereas the number of
    possible remainders returned by `round/` is *|y|+1* when
    y is even.

    ```
    (round-quotient 123 10) ⇒ 12
    (round-remainder 123 10) ⇒ 3
    (round/ 123 10) ⇒ 12 and 3
    (round/ 123 -10) ⇒ -12 and 3
    (round/ -123 10) ⇒ -12 and -3
    (round/ -123 -10) ⇒ 12 and -3
    (round/ 125 10) ⇒ 12 and 5
    (round/ 127 10) ⇒ 13 and -3
    (round/ 135 10) ⇒ 14 and -5
    (round/ -123.2 -63.5) ⇒ 2.0 and 3.8
    (round/ 16/3 -10/7) ⇒ -4 and -8/21
    ```

---

Next: [Bitwise Operations](#Bitwise-Operations), Previous: [Arithmetic Functions](#Arithmetic), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.12 Scientific Functions [¶](#Scientific-Functions)

The following procedures accept any kind of number as arguments,
including complex numbers.

Scheme Procedure: **sqrt** z [¶](#index-sqrt)
:   Return the square root of z. Of the two possible roots
    (positive and negative), the one with a positive real part is
    returned, or if that’s zero then a positive imaginary part. Thus,

    ```
    (sqrt 9.0)       ⇒ 3.0
    (sqrt -9.0)      ⇒ 0.0+3.0i
    (sqrt 1.0+1.0i)  ⇒ 1.09868411346781+0.455089860562227i
    (sqrt -1.0-1.0i) ⇒ 0.455089860562227-1.09868411346781i
    ```

Scheme Procedure: **expt** z1 z2 [¶](#index-expt)
:   Return z1 raised to the power of z2.

Scheme Procedure: **sin** z [¶](#index-sin)
:   Return the sine of z.

Scheme Procedure: **cos** z [¶](#index-cos)
:   Return the cosine of z.

Scheme Procedure: **tan** z [¶](#index-tan)
:   Return the tangent of z.

Scheme Procedure: **asin** z [¶](#index-asin)
:   Return the arcsine of z.

Scheme Procedure: **acos** z [¶](#index-acos)
:   Return the arccosine of z.

Scheme Procedure: **atan** z [¶](#index-atan)

Scheme Procedure: **atan** y x [¶](#index-atan-1)
:   Return the arctangent of z, or of *y/x*.

Scheme Procedure: **exp** z [¶](#index-exp)
:   Return e to the power of z, where e is the base of natural
    logarithms (2.71828…).

Scheme Procedure: **log** z [¶](#index-log)
:   Return the natural logarithm of z.

Scheme Procedure: **log10** z [¶](#index-log10)
:   Return the base 10 logarithm of z.

Scheme Procedure: **sinh** z [¶](#index-sinh)
:   Return the hyperbolic sine of z.

Scheme Procedure: **cosh** z [¶](#index-cosh)
:   Return the hyperbolic cosine of z.

Scheme Procedure: **tanh** z [¶](#index-tanh)
:   Return the hyperbolic tangent of z.

Scheme Procedure: **asinh** z [¶](#index-asinh)
:   Return the hyperbolic arcsine of z.

Scheme Procedure: **acosh** z [¶](#index-acosh)
:   Return the hyperbolic arccosine of z.

Scheme Procedure: **atanh** z [¶](#index-atanh)
:   Return the hyperbolic arctangent of z.

---

Next: [Random Number Generation](#Random), Previous: [Scientific Functions](#Scientific), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.13 Bitwise Operations [¶](#Bitwise-Operations-1)

For the following bitwise functions, negative numbers are treated as
infinite precision twos-complements. For instance *-6* is bits
*...111010*, with infinitely many ones on the left. It can
be seen that adding 6 (binary 110) to such a bit pattern gives all
zeros.

Scheme Procedure: **logand** n1 n2 … [¶](#index-logand)

C Function: **scm\_logand** (n1, n2) [¶](#index-scm_005flogand)
:   Return the bitwise AND of the integer arguments.

    ```
    (logand) ⇒ -1
    (logand 7) ⇒ 7
    (logand #b111 #b011 #b001) ⇒ 1
    ```

Scheme Procedure: **logior** n1 n2 … [¶](#index-logior)

C Function: **scm\_logior** (n1, n2) [¶](#index-scm_005flogior)
:   Return the bitwise OR of the integer arguments.

    ```
    (logior) ⇒ 0
    (logior 7) ⇒ 7
    (logior #b000 #b001 #b011) ⇒ 3
    ```

Scheme Procedure: **logxor** n1 n2 … [¶](#index-logxor)

C Function: **scm\_loxor** (n1, n2) [¶](#index-scm_005floxor)
:   Return the bitwise XOR of the integer arguments. A bit is
    set in the result if it is set in an odd number of arguments.

    ```
    (logxor) ⇒ 0
    (logxor 7) ⇒ 7
    (logxor #b000 #b001 #b011) ⇒ 2
    (logxor #b000 #b001 #b011 #b011) ⇒ 1
    ```

Scheme Procedure: **lognot** n [¶](#index-lognot)

C Function: **scm\_lognot** (n) [¶](#index-scm_005flognot)
:   Return the integer which is the ones-complement of the integer
    argument, ie. each 0 bit is changed to 1 and each 1 bit to 0.

    ```
    (number->string (lognot #b10000000) 2)
       ⇒ "-10000001"
    (number->string (lognot #b0) 2)
       ⇒ "-1"
    ```

Scheme Procedure: **logtest** j k [¶](#index-logtest)

C Function: **scm\_logtest** (j, k) [¶](#index-scm_005flogtest)
:   Test whether j and k have any 1 bits in common. This is
    equivalent to `(not (zero? (logand j k)))`, but without actually
    calculating the `logand`, just testing for non-zero.

    ```
    (logtest #b0100 #b1011) ⇒ #f
    (logtest #b0100 #b0111) ⇒ #t
    ```

Scheme Procedure: **logbit?** index j [¶](#index-logbit_003f)

C Function: **scm\_logbit\_p** (index, j) [¶](#index-scm_005flogbit_005fp)
:   Test whether bit number index in j is set. index
    starts from 0 for the least significant bit.

    ```
    (logbit? 0 #b1101) ⇒ #t
    (logbit? 1 #b1101) ⇒ #f
    (logbit? 2 #b1101) ⇒ #t
    (logbit? 3 #b1101) ⇒ #t
    (logbit? 4 #b1101) ⇒ #f
    ```

Scheme Procedure: **ash** n count [¶](#index-ash)

C Function: **scm\_ash** (n, count) [¶](#index-scm_005fash)
:   Return *floor(n \* 2^{count})*.
    n and count must be exact integers.

    With n viewed as an infinite-precision twos-complement
    integer, `ash` means a left shift introducing zero bits
    when count is positive, or a right shift dropping bits
    when count is negative. This is an “arithmetic” shift.

    ```
    (number->string (ash #b1 3) 2)     ⇒ "1000"
    (number->string (ash #b1010 -1) 2) ⇒ "101"

    ;; -23 is bits ...11101001, -6 is bits ...111010
    (ash -23 -2) ⇒ -6
    ```

Scheme Procedure: **round-ash** n count [¶](#index-round_002dash)

C Function: **scm\_round\_ash** (n, count) [¶](#index-scm_005fround_005fash)
:   Return *round(n \* 2^count)*.
    n and count must be exact integers.

    With n viewed as an infinite-precision twos-complement
    integer, `round-ash` means a left shift introducing zero
    bits when count is positive, or a right shift rounding
    to the nearest integer (with ties going to the nearest even
    integer) when count is negative. This is a rounded
    “arithmetic” shift.

    ```
    (number->string (round-ash #b1 3) 2)     ⇒ \"1000\"
    (number->string (round-ash #b1010 -1) 2) ⇒ \"101\"
    (number->string (round-ash #b1010 -2) 2) ⇒ \"10\"
    (number->string (round-ash #b1011 -2) 2) ⇒ \"11\"
    (number->string (round-ash #b1101 -2) 2) ⇒ \"11\"
    (number->string (round-ash #b1110 -2) 2) ⇒ \"100\"
    ```

Scheme Procedure: **logcount** n [¶](#index-logcount)

C Function: **scm\_logcount** (n) [¶](#index-scm_005flogcount)
:   Return the number of bits in integer n. If n is
    positive, the 1-bits in its binary representation are counted.
    If negative, the 0-bits in its two’s-complement binary
    representation are counted. If zero, 0 is returned.

    ```
    (logcount #b10101010)
       ⇒ 4
    (logcount 0)
       ⇒ 0
    (logcount -2)
       ⇒ 1
    ```

Scheme Procedure: **integer-length** n [¶](#index-integer_002dlength)

C Function: **scm\_integer\_length** (n) [¶](#index-scm_005finteger_005flength)
:   Return the number of bits necessary to represent n.

    For positive n this is how many bits to the most significant one
    bit. For negative n it’s how many bits to the most significant
    zero bit in twos complement form.

    ```
    (integer-length #b10101010) ⇒ 8
    (integer-length #b1111)     ⇒ 4
    (integer-length 0)          ⇒ 0
    (integer-length -1)         ⇒ 0
    (integer-length -256)       ⇒ 8
    (integer-length -257)       ⇒ 9
    ```

Scheme Procedure: **integer-expt** n k [¶](#index-integer_002dexpt)

C Function: **scm\_integer\_expt** (n, k) [¶](#index-scm_005finteger_005fexpt)
:   Return n raised to the power k. k must be an exact
    integer, n can be any number.

    Negative k is supported, and results in *1/n^abs(k)*
    in the usual way. *n^0* is 1, as usual, and that includes
    *0^0* is 1.

    ```
    (integer-expt 2 5)   ⇒ 32
    (integer-expt -3 3)  ⇒ -27
    (integer-expt 5 -3)  ⇒ 1/125
    (integer-expt 0 0)   ⇒ 1
    ```

Scheme Procedure: **bit-extract** n start end [¶](#index-bit_002dextract)

C Function: **scm\_bit\_extract** (n, start, end) [¶](#index-scm_005fbit_005fextract)
:   Return the integer composed of the start (inclusive)
    through end (exclusive) bits of n. The
    startth bit becomes the 0-th bit in the result.

    ```
    (number->string (bit-extract #b1101101010 0 4) 2)
       ⇒ "1010"
    (number->string (bit-extract #b1101101010 4 9) 2)
       ⇒ "10110"
    ```

---

Previous: [Bitwise Operations](#Bitwise-Operations), Up: [Numerical data types](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.2.14 Random Number Generation [¶](#Random-Number-Generation)

Pseudo-random numbers are generated from a random state object, which
can be created with `seed->random-state` or
`datum->random-state`. An external representation (i.e. one
which can written with `write` and read with `read`) of a
random state object can be obtained via
`random-state->datum`. The state parameter to the
various functions below is optional, it defaults to the state object
in the `*random-state*` variable.

Scheme Procedure: **copy-random-state** [state] [¶](#index-copy_002drandom_002dstate)

C Function: **scm\_copy\_random\_state** (state) [¶](#index-scm_005fcopy_005frandom_005fstate)
:   Return a copy of the random state state.

Scheme Procedure: **random** n [state] [¶](#index-random)

C Function: **scm\_random** (n, state) [¶](#index-scm_005frandom)
:   Return a number in [0, n).

    Accepts a positive integer or real n and returns a
    number of the same type between zero (inclusive) and
    n (exclusive). The values returned have a uniform
    distribution.

Scheme Procedure: **random:exp** [state] [¶](#index-random_003aexp)

C Function: **scm\_random\_exp** (state) [¶](#index-scm_005frandom_005fexp)
:   Return an inexact real in an exponential distribution with mean
    1. For an exponential distribution with mean u use `(*
    u (random:exp))`.

Scheme Procedure: **random:hollow-sphere!** vect [state] [¶](#index-random_003ahollow_002dsphere_0021)

C Function: **scm\_random\_hollow\_sphere\_x** (vect, state) [¶](#index-scm_005frandom_005fhollow_005fsphere_005fx)
:   Fills vect with inexact real random numbers the sum of whose
    squares is equal to 1.0. Thinking of vect as coordinates in
    space of dimension n *=* `(vector-length vect)`,
    the coordinates are uniformly distributed over the surface of the unit
    n-sphere.

Scheme Procedure: **random:normal** [state] [¶](#index-random_003anormal)

C Function: **scm\_random\_normal** (state) [¶](#index-scm_005frandom_005fnormal)
:   Return an inexact real in a normal distribution. The distribution
    used has mean 0 and standard deviation 1. For a normal distribution
    with mean m and standard deviation d use `(+ m
    (* d (random:normal)))`.

Scheme Procedure: **random:normal-vector!** vect [state] [¶](#index-random_003anormal_002dvector_0021)

C Function: **scm\_random\_normal\_vector\_x** (vect, state) [¶](#index-scm_005frandom_005fnormal_005fvector_005fx)
:   Fills vect with inexact real random numbers that are
    independent and standard normally distributed
    (i.e., with mean 0 and variance 1).

Scheme Procedure: **random:solid-sphere!** vect [state] [¶](#index-random_003asolid_002dsphere_0021)

C Function: **scm\_random\_solid\_sphere\_x** (vect, state) [¶](#index-scm_005frandom_005fsolid_005fsphere_005fx)
:   Fills vect with inexact real random numbers the sum of whose
    squares is less than 1.0. Thinking of vect as coordinates in
    space of dimension n *=* `(vector-length vect)`,
    the coordinates are uniformly distributed within the unit
    n-sphere.

Scheme Procedure: **random:uniform** [state] [¶](#index-random_003auniform)

C Function: **scm\_random\_uniform** (state) [¶](#index-scm_005frandom_005funiform)
:   Return a uniformly distributed inexact real random number in
    [0,1).

Scheme Procedure: **seed->random-state** seed [¶](#index-seed_002d_003erandom_002dstate)

C Function: **scm\_seed\_to\_random\_state** (seed) [¶](#index-scm_005fseed_005fto_005frandom_005fstate)
:   Return a new random state using seed.

Scheme Procedure: **datum->random-state** datum [¶](#index-datum_002d_003erandom_002dstate)

C Function: **scm\_datum\_to\_random\_state** (datum) [¶](#index-scm_005fdatum_005fto_005frandom_005fstate)
:   Return a new random state from datum, which should have been
    obtained by `random-state->datum`.

Scheme Procedure: **random-state->datum** state [¶](#index-random_002dstate_002d_003edatum)

C Function: **scm\_random\_state\_to\_datum** (state) [¶](#index-scm_005frandom_005fstate_005fto_005fdatum)
:   Return a datum representation of state that may be written out and
    read back with the Scheme reader.

Scheme Procedure: **random-state-from-platform** [¶](#index-random_002dstate_002dfrom_002dplatform)

C Function: **scm\_random\_state\_from\_platform** () [¶](#index-scm_005frandom_005fstate_005ffrom_005fplatform)
:   Construct a new random state seeded from a platform-specific source of
    entropy, appropriate for use in non-security-critical applications.
    Currently `/dev/urandom` is tried first, or else the seed is based
    on the time, date, process ID, an address from a freshly allocated heap
    cell, an address from the local stack frame, and a high-resolution timer
    if available.

Variable: **\*random-state\*** [¶](#index-_002arandom_002dstate_002a)
:   The global random state used by the above functions when the
    state parameter is not given.

Note that the initial value of `*random-state*` is the same every
time Guile starts up. Therefore, if you don’t pass a state
parameter to the above procedures, and you don’t set
`*random-state*` to `(seed->random-state your-seed)`, where
`your-seed` is something that *isn’t* the same every time,
you’ll get the same sequence of “random” numbers on every run.

For example, unless the relevant source code has changed, `(map
random (cdr (iota 30)))`, if the first use of random numbers since
Guile started up, will always give:

```
(map random (cdr (iota 19)))
⇒
(0 1 1 2 2 2 1 2 6 7 10 0 5 3 12 5 5 12)
```

To seed the random state in a sensible way for non-security-critical
applications, do this during initialization of your program:

```
(set! *random-state* (random-state-from-platform))
```

---

Next: [Character Sets](#Character-Sets), Previous: [Numerical data types](#Numbers), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.3 Characters [¶](#Characters-1)

In Scheme, there is a data type to describe a single character.

Defining what exactly a character *is* can be more complicated
than it seems. Guile follows the advice of R6RS and uses The Unicode
Standard to help define what a character is. So, for Guile, a
character is anything in the Unicode Character Database.

The Unicode Character Database is basically a table of characters
indexed using integers called ’code points’. Valid code points are in
the ranges 0 to `#xD7FF` inclusive or `#xE000` to
`#x10FFFF` inclusive, which is about 1.1 million code points.

Any code point that has been assigned to a character or that has
otherwise been given a meaning by Unicode is called a ’designated code
point’. Most of the designated code points, about 200,000 of them,
indicate characters, accents or other combining marks that modify
other characters, symbols, whitespace, and control characters. Some
are not characters but indicators that suggest how to format or
display neighboring characters.

If a code point is not a designated code point – if it has not been
assigned to a character by The Unicode Standard – it is a ’reserved
code point’, meaning that they are reserved for future use. Most of
the code points, about 800,000, are ’reserved code points’.

By convention, a Unicode code point is written as
“U+XXXX” where “XXXX” is a hexadecimal number. Please note that
this convenient notation is not valid code. Guile does not interpret
“U+XXXX” as a character.

In Scheme, a character literal is written as `#\name` where
name is the name of the character that you want. Printable
characters have their usual single character name; for example,
`#\a` is a lower case `a`.

Some of the code points are ’combining characters’ that are not meant
to be printed by themselves but are instead meant to modify the
appearance of the previous character. For combining characters, an
alternate form of the character literal is `#\` followed by
U+25CC (a small, dotted circle), followed by the combining character.
This allows the combining character to be drawn on the circle, not on
the backslash of `#\`.

Many of the non-printing characters, such as whitespace characters and
control characters, also have names.

The most commonly used non-printing characters have long character
names, described in the table below.

|  |  |
| --- | --- |
| Character Name | Codepoint |
| `#\nul` | U+0000 |
| `#\alarm` | U+0007 |
| `#\backspace` | U+0008 |
| `#\tab` | U+0009 |
| `#\linefeed` | U+000A |
| `#\newline` | U+000A |
| `#\vtab` | U+000B |
| `#\page` | U+000C |
| `#\return` | U+000D |
| `#\esc` | U+001B |
| `#\space` | U+0020 |
| `#\delete` | U+007F |

There are also short names for all of the “C0 control characters”
(those with code points below 32). The following table lists the short
name for each character.

|  |  |  |  |
| --- | --- | --- | --- |
| 0 = `#\nul` | 1 = `#\soh` | 2 = `#\stx` | 3 = `#\etx` |
| 4 = `#\eot` | 5 = `#\enq` | 6 = `#\ack` | 7 = `#\bel` |
| 8 = `#\bs` | 9 = `#\ht` | 10 = `#\lf` | 11 = `#\vt` |
| 12 = `#\ff` | 13 = `#\cr` | 14 = `#\so` | 15 = `#\si` |
| 16 = `#\dle` | 17 = `#\dc1` | 18 = `#\dc2` | 19 = `#\dc3` |
| 20 = `#\dc4` | 21 = `#\nak` | 22 = `#\syn` | 23 = `#\etb` |
| 24 = `#\can` | 25 = `#\em` | 26 = `#\sub` | 27 = `#\esc` |
| 28 = `#\fs` | 29 = `#\gs` | 30 = `#\rs` | 31 = `#\us` |
| 32 = `#\sp` |

The short name for the “delete” character (code point U+007F) is
`#\del`.

The R7RS name for the “escape” character (code point U+001B) is
`#\escape`.

There are also a few alternative names left over for compatibility with
previous versions of Guile.

|  |  |
| --- | --- |
| Alternate | Standard |
| `#\nl` | `#\newline` |
| `#\np` | `#\page` |
| `#\null` | `#\nul` |

Characters may also be written using their code point values. They can
be written with as an octal number, such as `#\10` for
`#\bs` or `#\177` for `#\del`.

If one prefers hex to octal, there is an additional syntax for character
escapes: `#\xHHHH` – the letter ’x’ followed by a hexadecimal
number of one to eight digits.

Scheme Procedure: **char?** x [¶](#index-char_003f)

C Function: **scm\_char\_p** (x) [¶](#index-scm_005fchar_005fp)
:   Return `#t` if x is a character, else `#f`.

Fundamentally, the character comparison operations below are
numeric comparisons of the character’s code points.

Scheme Procedure: **char=?** x y [¶](#index-char_003d_003f)
:   Return `#t` if code point of x is equal to the code point
    of y, else `#f`.

Scheme Procedure: **char<?** x y [¶](#index-char_003c_003f)
:   Return `#t` if the code point of x is less than the code
    point of y, else `#f`.

Scheme Procedure: **char<=?** x y [¶](#index-char_003c_003d_003f)
:   Return `#t` if the code point of x is less than or equal
    to the code point of y, else `#f`.

Scheme Procedure: **char>?** x y [¶](#index-char_003e_003f)
:   Return `#t` if the code point of x is greater than the
    code point of y, else `#f`.

Scheme Procedure: **char>=?** x y [¶](#index-char_003e_003d_003f)
:   Return `#t` if the code point of x is greater than or
    equal to the code point of y, else `#f`.

Case-insensitive character comparisons use *Unicode case
folding*. In case folding comparisons, if a character is lowercase
and has an uppercase form that can be expressed as a single character,
it is converted to uppercase before comparison. All other characters
undergo no conversion before the comparison occurs. This includes the
German sharp S (Eszett) which is not uppercased before conversion
because its uppercase form has two characters. Unicode case folding
is language independent: it uses rules that are generally true, but,
it cannot cover all cases for all languages.

Scheme Procedure: **char-ci=?** x y [¶](#index-char_002dci_003d_003f)
:   Return `#t` if the case-folded code point of x is the same
    as the case-folded code point of y, else `#f`.

Scheme Procedure: **char-ci<?** x y [¶](#index-char_002dci_003c_003f)
:   Return `#t` if the case-folded code point of x is less
    than the case-folded code point of y, else `#f`.

Scheme Procedure: **char-ci<=?** x y [¶](#index-char_002dci_003c_003d_003f)
:   Return `#t` if the case-folded code point of x is less
    than or equal to the case-folded code point of y, else
    `#f`.

Scheme Procedure: **char-ci>?** x y [¶](#index-char_002dci_003e_003f)
:   Return `#t` if the case-folded code point of x is greater
    than the case-folded code point of y, else `#f`.

Scheme Procedure: **char-ci>=?** x y [¶](#index-char_002dci_003e_003d_003f)
:   Return `#t` if the case-folded code point of x is greater
    than or equal to the case-folded code point of y, else
    `#f`.

Scheme Procedure: **char-alphabetic?** chr [¶](#index-char_002dalphabetic_003f)

C Function: **scm\_char\_alphabetic\_p** (chr) [¶](#index-scm_005fchar_005falphabetic_005fp)
:   Return `#t` if chr is alphabetic, else `#f`.

Scheme Procedure: **char-numeric?** chr [¶](#index-char_002dnumeric_003f)

C Function: **scm\_char\_numeric\_p** (chr) [¶](#index-scm_005fchar_005fnumeric_005fp)
:   Return `#t` if chr is numeric, else `#f`.

Scheme Procedure: **char-whitespace?** chr [¶](#index-char_002dwhitespace_003f)

C Function: **scm\_char\_whitespace\_p** (chr) [¶](#index-scm_005fchar_005fwhitespace_005fp)
:   Return `#t` if chr is whitespace, else `#f`.

Scheme Procedure: **char-upper-case?** chr [¶](#index-char_002dupper_002dcase_003f)

C Function: **scm\_char\_upper\_case\_p** (chr) [¶](#index-scm_005fchar_005fupper_005fcase_005fp)
:   Return `#t` if chr is uppercase, else `#f`.

Scheme Procedure: **char-lower-case?** chr [¶](#index-char_002dlower_002dcase_003f)

C Function: **scm\_char\_lower\_case\_p** (chr) [¶](#index-scm_005fchar_005flower_005fcase_005fp)
:   Return `#t` if chr is lowercase, else `#f`.

Scheme Procedure: **char-is-both?** chr [¶](#index-char_002dis_002dboth_003f)

C Function: **scm\_char\_is\_both\_p** (chr) [¶](#index-scm_005fchar_005fis_005fboth_005fp)
:   Return `#t` if chr is either uppercase or lowercase, else
    `#f`.

Scheme Procedure: **char-general-category** chr [¶](#index-char_002dgeneral_002dcategory)

C Function: **scm\_char\_general\_category** (chr) [¶](#index-scm_005fchar_005fgeneral_005fcategory)
:   Return a symbol giving the two-letter name of the Unicode general
    category assigned to chr or `#f` if no named category is
    assigned. The following table provides a list of category names along
    with their meanings.
