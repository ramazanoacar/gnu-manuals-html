### 7.6 R6RS Support [¶](#R6RS-Support-1)

See [R6RS Libraries](#R6RS-Libraries), for more information on how to define R6RS libraries, and
their integration with Guile modules.

* [Incompatibilities with the R6RS](#R6RS-Incompatibilities)
* [R6RS Standard Libraries](#R6RS-Standard-Libraries)

---

Next: [R6RS Standard Libraries](#R6RS-Standard-Libraries), Up: [R6RS Support](#R6RS-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.1 Incompatibilities with the R6RS [¶](#Incompatibilities-with-the-R6RS)

There are some incompatibilities between Guile and the R6RS. Some of
them are intentional, some of them are bugs, and some are simply
unimplemented features. Please let the Guile developers know if you
find one that is not on this list.

* The R6RS specifies many situations in which a conforming implementation
  must signal a specific error. Guile doesn’t really care about that too
  much—if a correct R6RS program would not hit that error, we don’t
  bother checking for it.
* Multiple `library` forms in one file are not yet supported. This
  is because the expansion of `library` sets the current module, but
  does not restore it. This is a bug.
* R6RS unicode escapes within strings are disabled by default, because
  they conflict with Guile’s already-existing escapes. The same is the
  case for R6RS treatment of escaped newlines in strings.

  R6RS behavior can be turned on via a reader option. See [String Read Syntax](#String-Syntax), for more information.
* Guile does not yet support Unicode escapes in symbols, such as
  `H\x65;llo` (the same as `Hello`), or `\x3BB;` (the same
  as `λ`).
* A `set!` to a variable transformer may only expand to an
  expression, not a definition—even if the original `set!`
  expression was in definition context.
* Instead of using the algorithm detailed in chapter 10 of the R6RS,
  expansion of toplevel forms happens sequentially.

  For example, while the expansion of the following set of toplevel
  definitions does the correct thing:

  ```
  (begin
   (define even?
     (lambda (x)
       (or (= x 0) (odd? (- x 1)))))
   (define-syntax odd?
     (syntax-rules ()
       ((odd? x) (not (even? x)))))
   (even? 10))
  ⇒ #t
  ```

  The same definitions outside of the `begin` wrapper do not:

  ```
  (define even?
    (lambda (x)
      (or (= x 0) (odd? (- x 1)))))
  (define-syntax odd?
    (syntax-rules ()
      ((odd? x) (not (even? x)))))
  (even? 10)
  <unnamed port>:4:18: In procedure even?:
  <unnamed port>:4:18: Wrong type to apply: #<syntax-transformer odd?>
  ```

  This is because when expanding the right-hand-side of `even?`, the
  reference to `odd?` is not yet marked as a syntax transformer, so
  it is assumed to be a function.

  This bug will only affect top-level programs, not code in `library`
  forms. Fixing it for toplevel forms seems doable, but tricky to
  implement in a backward-compatible way. Suggestions and/or patches would
  be appreciated.
* The `(rnrs io ports)` module is incomplete. Work is
  ongoing to fix this.
* Guile does not prevent use of textual I/O procedures on binary ports, or
  vice versa. All ports in Guile support both binary and textual I/O.
  See [Encoding](#Encoding), for full details.
* Guile’s implementation of `equal?` may fail to terminate when
  applied to arguments containing cycles.

Guile exposes a procedure in the root module to choose R6RS defaults
over Guile’s historical defaults.

Scheme Procedure: **install-r6rs!** [¶](#index-install_002dr6rs_0021)
:   Alter Guile’s default settings to better conform to the R6RS.

    While Guile’s defaults may evolve over time, the current changes that
    this procedure imposes are to add `.sls` and `.guile.sls` to
    the set of supported `%load-extensions`, to better support R6RS
    conventions. See [Load Paths](#Load-Paths). Also, enable R6RS unicode escapes in
    strings; see the discussion above.

Finally, note that the `--r6rs` command-line argument will call
`install-r6rs!` before calling user code. R6RS users probably want
to pass this argument to their Guile.

---

Previous: [Incompatibilities with the R6RS](#R6RS-Incompatibilities), Up: [R6RS Support](#R6RS-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2 R6RS Standard Libraries [¶](#R6RS-Standard-Libraries-1)

In contrast with earlier versions of the Revised Report, the R6RS
organizes the procedures and syntactic forms required of conforming
implementations into a set of “standard libraries” which can be
imported as necessary by user programs and libraries. Here we briefly
list the libraries that have been implemented for Guile.

We do not attempt to document these libraries fully here, as most of
their functionality is already available in Guile itself. The
expectation is that most Guile users will use the well-known and
well-documented Guile modules. These R6RS libraries are mostly useful
to users who want to port their code to other R6RS systems.

The documentation in the following sections reproduces some of the
content of the library section of the Report, but is mostly intended to
provide supplementary information about Guile’s implementation of the
R6RS standard libraries. For complete documentation, design rationales
and further examples, we advise you to consult the “Standard
Libraries” section of the Report (see [R6RS Standard Libraries](r6rs.html#Standard-Libraries) in The Revised^6 Report on the Algorithmic
Language Scheme).

* [Library Usage](#Library-Usage)
* [rnrs base](#rnrs-base)
* [rnrs unicode](#rnrs-unicode)
* [rnrs bytevectors](#rnrs-bytevectors)
* [rnrs lists](#rnrs-lists)
* [rnrs sorting](#rnrs-sorting)
* [rnrs control](#rnrs-control)
* [R6RS Records](#R6RS-Records)
* [rnrs records syntactic](#rnrs-records-syntactic)
* [rnrs records procedural](#rnrs-records-procedural)
* [rnrs records inspection](#rnrs-records-inspection)
* [rnrs exceptions](#rnrs-exceptions)
* [rnrs conditions](#rnrs-conditions)
* [I/O Conditions](#R6RS-I_002fO-Conditions)
* [Transcoders](#R6RS-Transcoders)
* [rnrs io ports](#rnrs-io-ports)
* [R6RS File Ports](#R6RS-File-Ports)
* [rnrs io simple](#rnrs-io-simple)
* [rnrs files](#rnrs-files)
* [rnrs programs](#rnrs-programs)
* [rnrs arithmetic fixnums](#rnrs-arithmetic-fixnums)
* [rnrs arithmetic flonums](#rnrs-arithmetic-flonums)
* [rnrs arithmetic bitwise](#rnrs-arithmetic-bitwise)
* [rnrs syntax-case](#rnrs-syntax_002dcase)
* [rnrs hashtables](#rnrs-hashtables)
* [rnrs enums](#rnrs-enums)
* [rnrs](#rnrs)
* [rnrs eval](#rnrs-eval)
* [rnrs mutable-pairs](#rnrs-mutable_002dpairs)
* [rnrs mutable-strings](#rnrs-mutable_002dstrings)
* [rnrs r5rs](#rnrs-r5rs)

---

Next: [rnrs base](#rnrs-base), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.1 Library Usage [¶](#Library-Usage-1)

Guile implements the R6RS ‘library’ form as a transformation to a native
Guile module definition. As a consequence of this, all of the libraries
described in the following subsections, in addition to being available
for use by R6RS libraries and top-level programs, can also be imported
as if they were normal Guile modules—via a `use-modules` form,
say. For example, the R6RS “composite” library can be imported by:

```
  (import (rnrs (6)))
```

```
  (use-modules ((rnrs) :version (6)))
```

For more information on Guile’s library implementation, see
(see [R6RS Libraries](#R6RS-Libraries)).

---

Next: [rnrs unicode](#rnrs-unicode), Previous: [Library Usage](#Library-Usage), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.2 rnrs base [¶](#rnrs-base-1)

The `(rnrs base (6))` library exports the procedures and syntactic
forms described in the main section of the Report
(see [R6RS Base library](r6rs.html#Base-library) in The Revised^6 Report on the Algorithmic Language Scheme). They are
grouped below by the existing manual sections to which they correspond.

Scheme Procedure: **boolean?** obj [¶](#index-boolean_003f-1)

Scheme Procedure: **not** x [¶](#index-not-1)
:   See [Booleans](#Booleans), for documentation.

Scheme Procedure: **symbol?** obj [¶](#index-symbol_003f-1)

Scheme Procedure: **symbol->string** sym [¶](#index-symbol_002d_003estring-1)

Scheme Procedure: **string->symbol** str [¶](#index-string_002d_003esymbol-1)
:   See [Operations Related to Symbols](#Symbol-Primitives), for documentation.

Scheme Procedure: **char?** obj [¶](#index-char_003f-1)

Scheme Procedure: **char=?** [¶](#index-char_003d_003f-1)

Scheme Procedure: **char<?** [¶](#index-char_003c_003f-1)

Scheme Procedure: **char>?** [¶](#index-char_003e_003f-1)

Scheme Procedure: **char<=?** [¶](#index-char_003c_003d_003f-1)

Scheme Procedure: **char>=?** [¶](#index-char_003e_003d_003f-1)

Scheme Procedure: **integer->char** n [¶](#index-integer_002d_003echar-1)

Scheme Procedure: **char->integer** chr [¶](#index-char_002d_003einteger-1)
:   See [Characters](#Characters), for documentation.

Scheme Procedure: **list?** x [¶](#index-list_003f-1)

Scheme Procedure: **null?** x [¶](#index-null_003f-1)
:   See [List Predicates](#List-Predicates), for documentation.

Scheme Procedure: **pair?** x [¶](#index-pair_003f-1)

Scheme Procedure: **cons** x y [¶](#index-cons-1)

Scheme Procedure: **car** pair [¶](#index-car-1)

Scheme Procedure: **cdr** pair [¶](#index-cdr-1)

Scheme Procedure: **caar** pair [¶](#index-caar-1)

Scheme Procedure: **cadr** pair [¶](#index-cadr-1)

Scheme Procedure: **cdar** pair [¶](#index-cdar-1)

Scheme Procedure: **cddr** pair [¶](#index-cddr-1)

Scheme Procedure: **caaar** pair [¶](#index-caaar-1)

Scheme Procedure: **caadr** pair [¶](#index-caadr-1)

Scheme Procedure: **cadar** pair [¶](#index-cadar-1)

Scheme Procedure: **cdaar** pair [¶](#index-cdaar-1)

Scheme Procedure: **caddr** pair [¶](#index-caddr-1)

Scheme Procedure: **cdadr** pair [¶](#index-cdadr-1)

Scheme Procedure: **cddar** pair [¶](#index-cddar-1)

Scheme Procedure: **cdddr** pair [¶](#index-cdddr-1)

Scheme Procedure: **caaaar** pair [¶](#index-caaaar-1)

Scheme Procedure: **caaadr** pair [¶](#index-caaadr-1)

Scheme Procedure: **caadar** pair [¶](#index-caadar-1)

Scheme Procedure: **cadaar** pair [¶](#index-cadaar-1)

Scheme Procedure: **cdaaar** pair [¶](#index-cdaaar-1)

Scheme Procedure: **cddaar** pair [¶](#index-cddaar-1)

Scheme Procedure: **cdadar** pair [¶](#index-cdadar-1)

Scheme Procedure: **cdaadr** pair [¶](#index-cdaadr-1)

Scheme Procedure: **cadadr** pair [¶](#index-cadadr-1)

Scheme Procedure: **caaddr** pair [¶](#index-caaddr-1)

Scheme Procedure: **caddar** pair [¶](#index-caddar-1)

Scheme Procedure: **cadddr** pair [¶](#index-cadddr-1)

Scheme Procedure: **cdaddr** pair [¶](#index-cdaddr-1)

Scheme Procedure: **cddadr** pair [¶](#index-cddadr-1)

Scheme Procedure: **cdddar** pair [¶](#index-cdddar-1)

Scheme Procedure: **cddddr** pair [¶](#index-cddddr-1)
:   See [Pairs](#Pairs), for documentation.

Scheme Procedure: **number?** obj [¶](#index-number_003f-1)
:   See [Scheme’s Numerical “Tower”](#Numerical-Tower), for documentation.

Scheme Procedure: **string?** obj [¶](#index-string_003f-1)
:   See [String Predicates](#String-Predicates), for documentation.

Scheme Procedure: **procedure?** obj [¶](#index-procedure_003f-1)
:   See [Procedure Properties and Meta-information](#Procedure-Properties), for documentation.

Scheme Syntax: **define** name value [¶](#index-define-1)

Scheme Syntax: **set!** variable-name value [¶](#index-set_0021)
:   See [Defining and Setting Variables](#Definition), for documentation.

Scheme Syntax: **define-syntax** keyword expression [¶](#index-define_002dsyntax-1)

Scheme Syntax: **let-syntax** ((keyword transformer) …) exp1 exp2 … [¶](#index-let_002dsyntax-1)

Scheme Syntax: **letrec-syntax** ((keyword transformer) …) exp1 exp2 … [¶](#index-letrec_002dsyntax-1)
:   See [Defining Macros](#Defining-Macros), for documentation.

Scheme Syntax: **identifier-syntax** exp [¶](#index-identifier_002dsyntax-2)
:   See [Identifier Macros](#Identifier-Macros), for documentation.

Scheme Syntax: **syntax-rules** literals (pattern template) ... [¶](#index-syntax_002drules-1)
:   See [Syntax-rules Macros](#Syntax-Rules), for documentation.

Scheme Syntax: **lambda** formals body [¶](#index-lambda-2)
:   See [Lambda: Basic Procedure Creation](#Lambda), for documentation.

Scheme Syntax: **let** bindings body [¶](#index-let-2)

Scheme Syntax: **let\*** bindings body [¶](#index-let_002a-1)

Scheme Syntax: **letrec** bindings body [¶](#index-letrec-1)

Scheme Syntax: **letrec\*** bindings body [¶](#index-letrec_002a-1)
:   See [Local Variable Bindings](#Local-Bindings), for documentation.

Scheme Syntax: **let-values** bindings body [¶](#index-let_002dvalues-1)

Scheme Syntax: **let\*-values** bindings body [¶](#index-let_002a_002dvalues-1)
:   See [SRFI-11 - let-values](#SRFI_002d11), for documentation.

Scheme Syntax: **begin** expr1 expr2 ... [¶](#index-begin-2)
:   See [Sequencing and Splicing](#begin), for documentation.

Scheme Syntax: **quote** expr [¶](#index-quote-1)

Scheme Syntax: **quasiquote** expr [¶](#index-quasiquote-1)

Scheme Syntax: **unquote** expr [¶](#index-unquote-1)

Scheme Syntax: **unquote-splicing** expr [¶](#index-unquote_002dsplicing-1)
:   See [Expression Syntax](#Expression-Syntax), for documentation.

Scheme Syntax: **if** test consequence [alternate] [¶](#index-if-2)

Scheme Syntax: **cond** clause1 clause2 ... [¶](#index-cond-2)

Scheme Syntax: **case** key clause1 clause2 ... [¶](#index-case-2)
:   See [Simple Conditional Evaluation](#Conditionals), for documentation.

Scheme Syntax: **and** expr ... [¶](#index-and-1)

Scheme Syntax: **or** expr ... [¶](#index-or-1)
:   See [Conditional Evaluation of a Sequence of Expressions](#and-or), for documentation.

Scheme Procedure: **eq?** x y [¶](#index-eq_003f-1)

Scheme Procedure: **eqv?** x y [¶](#index-eqv_003f-1)

Scheme Procedure: **equal?** x y [¶](#index-equal_003f-1)

Scheme Procedure: **symbol=?** symbol1 symbol2 ... [¶](#index-symbol_003d_003f)
:   See [Equality](#Equality), for documentation.

    `symbol=?` is identical to `eq?`.

Scheme Procedure: **complex?** z [¶](#index-complex_003f-1)
:   See [Complex Numbers](#Complex-Numbers), for documentation.

Scheme Procedure: **real-part** z [¶](#index-real_002dpart-1)

Scheme Procedure: **imag-part** z [¶](#index-imag_002dpart-1)

Scheme Procedure: **make-rectangular** real\_part imaginary\_part [¶](#index-make_002drectangular-1)

Scheme Procedure: **make-polar** x y [¶](#index-make_002dpolar-1)

Scheme Procedure: **magnitude** z [¶](#index-magnitude-1)

Scheme Procedure: **angle** z [¶](#index-angle-1)
:   See [Complex Number Operations](#Complex), for documentation.

Scheme Procedure: **sqrt** z [¶](#index-sqrt-1)

Scheme Procedure: **exp** z [¶](#index-exp-1)

Scheme Procedure: **expt** z1 z2 [¶](#index-expt-1)

Scheme Procedure: **log** z [¶](#index-log-1)

Scheme Procedure: **sin** z [¶](#index-sin-1)

Scheme Procedure: **cos** z [¶](#index-cos-1)

Scheme Procedure: **tan** z [¶](#index-tan-1)

Scheme Procedure: **asin** z [¶](#index-asin-1)

Scheme Procedure: **acos** z [¶](#index-acos-1)

Scheme Procedure: **atan** z [¶](#index-atan-2)
:   See [Scientific Functions](#Scientific), for documentation.

Scheme Procedure: **real?** x [¶](#index-real_003f-1)

Scheme Procedure: **rational?** x [¶](#index-rational_003f-1)

Scheme Procedure: **numerator** x [¶](#index-numerator-1)

Scheme Procedure: **denominator** x [¶](#index-denominator-1)

Scheme Procedure: **rationalize** x eps [¶](#index-rationalize-1)
:   See [Real and Rational Numbers](#Reals-and-Rationals), for documentation.

Scheme Procedure: **exact?** x [¶](#index-exact_003f-1)

Scheme Procedure: **inexact?** x [¶](#index-inexact_003f-1)

Scheme Procedure: **exact** z [¶](#index-exact)

Scheme Procedure: **inexact** z [¶](#index-inexact)
:   See [Exact and Inexact Numbers](#Exactness), for documentation. The `exact` and
    `inexact` procedures are identical to the `inexact->exact` and
    `exact->inexact` procedures provided by Guile’s code library.

Scheme Procedure: **integer?** x [¶](#index-integer_003f-1)
:   See [Integers](#Integers), for documentation.

Scheme Procedure: **odd?** n [¶](#index-odd_003f-1)

Scheme Procedure: **even?** n [¶](#index-even_003f-1)

Scheme Procedure: **gcd** x ... [¶](#index-gcd-1)

Scheme Procedure: **lcm** x ... [¶](#index-lcm-1)

Scheme Procedure: **exact-integer-sqrt** k [¶](#index-exact_002dinteger_002dsqrt-1)
:   See [Operations on Integer Values](#Integer-Operations), for documentation.

Scheme Procedure: **=** [¶](#index-_003d-1)

Scheme Procedure: **<** [¶](#index-_003c-1)

Scheme Procedure: **>** [¶](#index-_003e-1)

Scheme Procedure: **<=** [¶](#index-_003c_003d-1)

Scheme Procedure: **>=** [¶](#index-_003e_003d-1)

Scheme Procedure: **zero?** x [¶](#index-zero_003f-1)

Scheme Procedure: **positive?** x [¶](#index-positive_003f-1)

Scheme Procedure: **negative?** x [¶](#index-negative_003f-1)
:   See [Comparison Predicates](#Comparison), for documentation.

Scheme Procedure: **for-each** f lst1 lst2 ... [¶](#index-for_002deach-2)
:   See [Fold, Unfold & Map](#SRFI_002d1-Fold-and-Map), for documentation.

Scheme Procedure: **list** elem … [¶](#index-list-2)
:   See [List Constructors](#List-Constructors), for documentation.

Scheme Procedure: **length** lst [¶](#index-length-1)

Scheme Procedure: **list-ref** lst k [¶](#index-list_002dref-1)

Scheme Procedure: **list-tail** lst k [¶](#index-list_002dtail-1)
:   See [List Selection](#List-Selection), for documentation.

Scheme Procedure: **append** lst … obj [¶](#index-append-2)

Scheme Procedure: **append** [¶](#index-append-3)

Scheme Procedure: **reverse** lst [¶](#index-reverse-1)
:   See [Append and Reverse](#Append_002fReverse), for documentation.

Scheme Procedure: **number->string** n [radix] [¶](#index-number_002d_003estring-1)

Scheme Procedure: **string->number** str [radix] [¶](#index-string_002d_003enumber-1)
:   See [Converting Numbers To and From Strings](#Conversion), for documentation.

Scheme Procedure: **string** char ... [¶](#index-string-1)

Scheme Procedure: **make-string** k [chr] [¶](#index-make_002dstring-1)

Scheme Procedure: **list->string** lst [¶](#index-list_002d_003estring-1)
:   See [String Constructors](#String-Constructors), for documentation.

Scheme Procedure: **string->list** str [start [end]] [¶](#index-string_002d_003elist-1)
:   See [List/String conversion](#List_002fString-Conversion), for documentation.

Scheme Procedure: **string-length** str [¶](#index-string_002dlength-1)

Scheme Procedure: **string-ref** str k [¶](#index-string_002dref-1)

Scheme Procedure: **string-copy** str [start [end]] [¶](#index-string_002dcopy-1)

Scheme Procedure: **substring** str start [end] [¶](#index-substring-1)
:   See [String Selection](#String-Selection), for documentation.

Scheme Procedure: **string=?** s1 s2 s3 … [¶](#index-string_003d_003f-1)

Scheme Procedure: **string<?** s1 s2 s3 … [¶](#index-string_003c_003f-1)

Scheme Procedure: **string>?** s1 s2 s3 … [¶](#index-string_003e_003f-1)

Scheme Procedure: **string<=?** s1 s2 s3 … [¶](#index-string_003c_003d_003f-1)

Scheme Procedure: **string>=?** s1 s2 s3 … [¶](#index-string_003e_003d_003f-1)
:   See [String Comparison](#String-Comparison), for documentation.

Scheme Procedure: **string-append** arg … [¶](#index-string_002dappend-1)
:   See [Reversing and Appending Strings](#Reversing-and-Appending-Strings), for documentation.

Scheme Procedure: **string-for-each** proc s [start [end]] [¶](#index-string_002dfor_002deach-1)
:   See [Mapping, Folding, and Unfolding](#Mapping-Folding-and-Unfolding), for documentation.

Scheme Procedure: **+** z1 ... [¶](#index-_002b-1)

Scheme Procedure: **-** z1 z2 ... [¶](#index-_002d-1)

Scheme Procedure: **\*** z1 ... [¶](#index-_002a-1)

Scheme Procedure: **/** z1 z2 ... [¶](#index-_002f-1)

Scheme Procedure: **max** x1 x2 ... [¶](#index-max-1)

Scheme Procedure: **min** x1 x2 ... [¶](#index-min-1)

Scheme Procedure: **abs** x [¶](#index-abs-1)

Scheme Procedure: **truncate** x [¶](#index-truncate-2)

Scheme Procedure: **floor** x [¶](#index-floor-1)

Scheme Procedure: **ceiling** x [¶](#index-ceiling-1)

Scheme Procedure: **round** x [¶](#index-round-1)
:   See [Arithmetic Functions](#Arithmetic), for documentation.

Scheme Procedure: **div** x y [¶](#index-div)

Scheme Procedure: **mod** x y [¶](#index-mod)

Scheme Procedure: **div-and-mod** x y [¶](#index-div_002dand_002dmod)
:   These procedures accept two real numbers x and y, where the
    divisor y must be non-zero. `div` returns the integer q
    and `mod` returns the real number r such that
    *x = q\*y + r* and *0 <= r < abs(y)*.
    `div-and-mod` returns both q and r, and is more
    efficient than computing each separately. Note that when *y > 0*,
    `div` returns *floor(x/y)*, otherwise
    it returns *ceiling(x/y)*.

    ```
    (div 123 10) ⇒ 12
    (mod 123 10) ⇒ 3
    (div-and-mod 123 10) ⇒ 12 and 3
    (div-and-mod 123 -10) ⇒ -12 and 3
    (div-and-mod -123 10) ⇒ -13 and 7
    (div-and-mod -123 -10) ⇒ 13 and 7
    (div-and-mod -123.2 -63.5) ⇒ 2.0 and 3.8
    (div-and-mod 16/3 -10/7) ⇒ -3 and 22/21
    ```

Scheme Procedure: **div0** x y [¶](#index-div0)

Scheme Procedure: **mod0** x y [¶](#index-mod0)

Scheme Procedure: **div0-and-mod0** x y [¶](#index-div0_002dand_002dmod0)
:   These procedures accept two real numbers x and y, where the
    divisor y must be non-zero. `div0` returns the
    integer q and `mod0` returns the real number
    r such that *x = q\*y + r* and
    *-abs(y/2) <= r < abs(y/2)*. `div0-and-mod0`
    returns both q and r, and is more efficient than computing
    each separately.

    Note that `div0` returns *x/y* rounded to the
    nearest integer. When *x/y* lies exactly half-way
    between two integers, the tie is broken according to the sign of
    y. If *y > 0*, ties are rounded toward positive
    infinity, otherwise they are rounded toward negative infinity.
    This is a consequence of the requirement that
    *-abs(y/2) <= r < abs(y/2)*.

    ```
    (div0 123 10) ⇒ 12
    (mod0 123 10) ⇒ 3
    (div0-and-mod0 123 10) ⇒ 12 and 3
    (div0-and-mod0 123 -10) ⇒ -12 and 3
    (div0-and-mod0 -123 10) ⇒ -12 and -3
    (div0-and-mod0 -123 -10) ⇒ 12 and -3
    (div0-and-mod0 -123.2 -63.5) ⇒ 2.0 and 3.8
    (div0-and-mod0 16/3 -10/7) ⇒ -4 and -8/21
    ```

Scheme Procedure: **real-valued?** obj [¶](#index-real_002dvalued_003f)

Scheme Procedure: **rational-valued?** obj [¶](#index-rational_002dvalued_003f)

Scheme Procedure: **integer-valued?** obj [¶](#index-integer_002dvalued_003f)
:   These procedures return `#t` if and only if their arguments can,
    respectively, be coerced to a real, rational, or integer value without a
    loss of numerical precision.

    `real-valued?` will return `#t` for complex numbers whose
    imaginary parts are zero.

Scheme Procedure: **nan?** x [¶](#index-nan_003f-1)

Scheme Procedure: **infinite?** x [¶](#index-infinite_003f)

Scheme Procedure: **finite?** x [¶](#index-finite_003f-1)
:   `nan?` returns `#t` if x is a NaN value, `#f`
    otherwise. `infinite?` returns `#t` if x is an infinite
    value, `#f` otherwise. `finite?` returns `#t` if x
    is neither infinite nor a NaN value, otherwise it returns `#f`.
    Every real number satisfies exactly one of these predicates. An
    exception is raised if x is not real.

Scheme Syntax: **assert** expr [¶](#index-assert)
:   Raises an `&assertion` condition if expr evaluates to
    `#f`; otherwise evaluates to the value of expr.

Scheme Procedure: **error** who message irritant1 ... [¶](#index-error-2)

Scheme Procedure: **assertion-violation** who message irritant1 ... [¶](#index-assertion_002dviolation)
:   These procedures raise compound conditions based on their arguments:
    If who is not `#f`, the condition will include a `&who`
    condition whose `who` field is set to who; a `&message`
    condition will be included with a `message` field equal to
    message; an `&irritants` condition will be included with its
    `irritants` list given by `irritant1 ...`.

    `error` produces a compound condition with the simple conditions
    described above, as well as an `&error` condition;
    `assertion-violation` produces one that includes an
    `&assertion` condition.

Scheme Procedure: **vector-map** proc v [¶](#index-vector_002dmap-1)

Scheme Procedure: **vector-for-each** proc v [¶](#index-vector_002dfor_002deach-1)
:   These procedures implement the `map` and `for-each` contracts
    over vectors.

Scheme Procedure: **vector** arg … [¶](#index-vector-2)

Scheme Procedure: **vector?** obj [¶](#index-vector_003f-2)

Scheme Procedure: **make-vector** len [¶](#index-make_002dvector-2)

Scheme Procedure: **make-vector** len fill [¶](#index-make_002dvector-3)

Scheme Procedure: **list->vector** l [¶](#index-list_002d_003evector-2)

Scheme Procedure: **vector->list** v [¶](#index-vector_002d_003elist-2)
:   See [Dynamic Vector Creation and Validation](#Vector-Creation), for documentation.

Scheme Procedure: **vector-length** vector [¶](#index-vector_002dlength-2)

Scheme Procedure: **vector-ref** vector k [¶](#index-vector_002dref-2)

Scheme Procedure: **vector-set!** vector k obj [¶](#index-vector_002dset_0021-2)

Scheme Procedure: **vector-fill!** v fill [¶](#index-vector_002dfill_0021-2)
:   See [Accessing and Modifying Vector Contents](#Vector-Accessors), for documentation.

Scheme Procedure: **call-with-current-continuation** proc [¶](#index-call_002dwith_002dcurrent_002dcontinuation-1)

Scheme Procedure: **call/cc** proc [¶](#index-call_002fcc-1)
:   See [Continuations](#Continuations), for documentation.

Scheme Procedure: **values** arg … [¶](#index-values-1)

Scheme Procedure: **call-with-values** producer consumer [¶](#index-call_002dwith_002dvalues-1)
:   See [Returning and Accepting Multiple Values](#Multiple-Values), for documentation.

Scheme Procedure: **dynamic-wind** in\_guard thunk out\_guard [¶](#index-dynamic_002dwind-1)
:   See [Dynamic Wind](#Dynamic-Wind), for documentation.

Scheme Procedure: **apply** proc arg … arglst [¶](#index-apply-1)
:   See [Procedures for On the Fly Evaluation](#Fly-Evaluation), for documentation.

---

Next: [rnrs bytevectors](#rnrs-bytevectors), Previous: [rnrs base](#rnrs-base), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.3 rnrs unicode [¶](#rnrs-unicode-1)

The `(rnrs unicode (6))` library provides procedures for
manipulating Unicode characters and strings.

Scheme Procedure: **char-upcase** char [¶](#index-char_002dupcase-1)

Scheme Procedure: **char-downcase** char [¶](#index-char_002ddowncase-1)

Scheme Procedure: **char-titlecase** char [¶](#index-char_002dtitlecase-1)

Scheme Procedure: **char-foldcase** char [¶](#index-char_002dfoldcase)
:   These procedures translate their arguments from one Unicode character
    set to another. `char-upcase`, `char-downcase`, and
    `char-titlecase` are identical to their counterparts in the
    Guile core library; See [Characters](#Characters), for documentation.

    `char-foldcase` returns the result of applying `char-upcase`
    to its argument, followed by `char-downcase`—except in the case
    of the Turkic characters `U+0130` and `U+0131`, for which the
    procedure acts as the identity function.

Scheme Procedure: **char-ci=?** char1 char2 char3 ... [¶](#index-char_002dci_003d_003f-1)

Scheme Procedure: **char-ci<?** char1 char2 char3 ... [¶](#index-char_002dci_003c_003f-1)

Scheme Procedure: **char-ci>?** char1 char2 char3 ... [¶](#index-char_002dci_003e_003f-1)

Scheme Procedure: **char-ci<=?** char1 char2 char3 ... [¶](#index-char_002dci_003c_003d_003f-1)

Scheme Procedure: **char-ci>=?** char1 char2 char3 ... [¶](#index-char_002dci_003e_003d_003f-1)
:   These procedures facilitate case-insensitive comparison of Unicode
    characters. They are identical to the procedures provided by Guile’s
    core library. See [Characters](#Characters), for documentation.

Scheme Procedure: **char-alphabetic?** char [¶](#index-char_002dalphabetic_003f-1)

Scheme Procedure: **char-numeric?** char [¶](#index-char_002dnumeric_003f-1)

Scheme Procedure: **char-whitespace?** char [¶](#index-char_002dwhitespace_003f-1)

Scheme Procedure: **char-upper-case?** char [¶](#index-char_002dupper_002dcase_003f-1)

Scheme Procedure: **char-lower-case?** char [¶](#index-char_002dlower_002dcase_003f-1)

Scheme Procedure: **char-title-case?** char [¶](#index-char_002dtitle_002dcase_003f)
:   These procedures implement various Unicode character set predicates.
    They are identical to the procedures provided by Guile’s core library.
    See [Characters](#Characters), for documentation.

Scheme Procedure: **char-general-category** char [¶](#index-char_002dgeneral_002dcategory-1)
:   See [Characters](#Characters), for documentation.

Scheme Procedure: **string-upcase** string [¶](#index-string_002dupcase-1)

Scheme Procedure: **string-downcase** string [¶](#index-string_002ddowncase-1)

Scheme Procedure: **string-titlecase** string [¶](#index-string_002dtitlecase-1)

Scheme Procedure: **string-foldcase** string [¶](#index-string_002dfoldcase)
:   These procedures perform Unicode case folding operations on their input.
    See [Alphabetic Case Mapping](#Alphabetic-Case-Mapping), for documentation.

Scheme Procedure: **string-ci=?** string1 string2 string3 ... [¶](#index-string_002dci_003d_003f-1)

Scheme Procedure: **string-ci<?** string1 string2 string3 ... [¶](#index-string_002dci_003c_003f-1)

Scheme Procedure: **string-ci>?** string1 string2 string3 ... [¶](#index-string_002dci_003e_003f-1)

Scheme Procedure: **string-ci<=?** string1 string2 string3 ... [¶](#index-string_002dci_003c_003d_003f-1)

Scheme Procedure: **string-ci>=?** string1 string2 string3 ... [¶](#index-string_002dci_003e_003d_003f-1)
:   These procedures perform case-insensitive comparison on their input.
    See [String Comparison](#String-Comparison), for documentation.

Scheme Procedure: **string-normalize-nfd** string [¶](#index-string_002dnormalize_002dnfd-1)

Scheme Procedure: **string-normalize-nfkd** string [¶](#index-string_002dnormalize_002dnfkd-1)

Scheme Procedure: **string-normalize-nfc** string [¶](#index-string_002dnormalize_002dnfc-1)

Scheme Procedure: **string-normalize-nfkc** string [¶](#index-string_002dnormalize_002dnfkc-1)
:   These procedures perform Unicode string normalization operations on
    their input. See [String Comparison](#String-Comparison), for documentation.

---

Next: [rnrs lists](#rnrs-lists), Previous: [rnrs unicode](#rnrs-unicode), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.4 rnrs bytevectors [¶](#rnrs-bytevectors-1)

The `(rnrs bytevectors (6))` library provides procedures for
working with blocks of binary data. This functionality is documented
in its own section of the manual; See [Bytevectors](#Bytevectors).

---

Next: [rnrs sorting](#rnrs-sorting), Previous: [rnrs bytevectors](#rnrs-bytevectors), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.5 rnrs lists [¶](#rnrs-lists-1)

The `(rnrs lists (6))` library provides procedures additional
procedures for working with lists.

Scheme Procedure: **find** proc list [¶](#index-find-1)
:   This procedure is identical to the one defined in Guile’s SRFI-1
    implementation. See [Searching](#SRFI_002d1-Searching), for documentation.

Scheme Procedure: **for-all** proc list1 list2 ... [¶](#index-for_002dall)

Scheme Procedure: **exists** proc list1 list2 ... [¶](#index-exists)
:   The `for-all` procedure is identical to the `every` procedure
    defined by SRFI-1; the `exists` procedure is identical to SRFI-1’s
    `any`. See [Searching](#SRFI_002d1-Searching), for documentation.

Scheme Procedure: **filter** proc list [¶](#index-filter-1)

Scheme Procedure: **partition** proc list [¶](#index-partition-1)
:   These procedures are identical to the ones provided by SRFI-1.
    See [List Modification](#List-Modification), for a description of `filter`;
    See [Filtering and Partitioning](#SRFI_002d1-Filtering-and-Partitioning), for `partition`.

Scheme Procedure: **fold-right** combine nil list1 list2 … [¶](#index-fold_002dright-1)
:   This procedure is identical the `fold-right` procedure provided by
    SRFI-1. See [Fold, Unfold & Map](#SRFI_002d1-Fold-and-Map), for documentation.

Scheme Procedure: **fold-left** combine nil list1 list2 … [¶](#index-fold_002dleft)
:   This procedure is like `fold` from SRFI-1, but combine is
    called with the seed as the first argument. See [Fold, Unfold & Map](#SRFI_002d1-Fold-and-Map),
    for documentation.

Scheme Procedure: **remp** proc list [¶](#index-remp)

Scheme Procedure: **remove** obj list [¶](#index-remove-1)

Scheme Procedure: **remv** obj list [¶](#index-remv)

Scheme Procedure: **remq** obj list [¶](#index-remq)
:   `remove`, `remv`, and `remq` are identical to the
    `delete`, `delv`, and `delq` procedures provided by
    Guile’s core library, (see [List Modification](#List-Modification)). `remp` is
    identical to the alternate `remove` procedure provided by SRFI-1;
    See [Deleting](#SRFI_002d1-Deleting).

Scheme Procedure: **memp** proc list [¶](#index-memp)

Scheme Procedure: **member** obj list [¶](#index-member-2)

Scheme Procedure: **memv** obj list [¶](#index-memv-1)

Scheme Procedure: **memq** obj list [¶](#index-memq-1)
:   `member`, `memv`, and `memq` are identical to the
    procedures provided by Guile’s core library; See [List Searching](#List-Searching),
    for their documentation. `memp` uses the specified predicate
    function `proc` to test elements of the list list—it
    behaves similarly to `find`, except that it returns the first
    sublist of list whose `car` satisfies proc.

Scheme Procedure: **assp** proc alist [¶](#index-assp)

Scheme Procedure: **assoc** obj alist [¶](#index-assoc-2)

Scheme Procedure: **assv** obj alist [¶](#index-assv-1)

Scheme Procedure: **assq** obj alist [¶](#index-assq-1)
:   `assoc`, `assv`, and `assq` are identical to the
    procedures provided by Guile’s core library;
    See [Alist Key Equality](#Alist-Key-Equality), for their documentation. `assp` uses
    the specified predicate function `proc` to test keys in the
    association list alist.

Scheme Procedure: **cons\*** obj1 ... obj [¶](#index-cons_002a-1)

Scheme Procedure: **cons\*** obj [¶](#index-cons_002a-2)
:   This procedure is identical to the one exported by Guile’s core
    library. See [List Constructors](#List-Constructors), for documentation.

---

Next: [rnrs control](#rnrs-control), Previous: [rnrs lists](#rnrs-lists), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.6 rnrs sorting [¶](#rnrs-sorting-1)

The `(rnrs sorting (6))` library provides procedures for sorting
lists and vectors.

Scheme Procedure: **list-sort** proc list [¶](#index-list_002dsort)

Scheme Procedure: **vector-sort** proc vector [¶](#index-vector_002dsort)
:   These procedures return their input sorted in ascending order, without
    modifying the original data. proc must be a procedure that takes
    two elements from the input list or vector as arguments, and returns a
    true value if the first is “less” than the second, `#f`
    otherwise. `list-sort` returns a list; `vector-sort` returns
    a vector.

    Both `list-sort` and `vector-sort` are implemented in terms of
    the `stable-sort` procedure from Guile’s core library.
    See [Sorting](#Sorting), for a discussion of the behavior of that procedure.

Scheme Procedure: **vector-sort!** proc vector [¶](#index-vector_002dsort_0021)
:   Performs a destructive, “in-place” sort of vector, using
    proc as described above to determine an ascending ordering of
    elements. `vector-sort!` returns an unspecified value.

    This procedure is implemented in terms of the `sort!` procedure
    from Guile’s core library. See [Sorting](#Sorting), for more information.

---

Next: [R6RS Records](#R6RS-Records), Previous: [rnrs sorting](#rnrs-sorting), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.7 rnrs control [¶](#rnrs-control-1)

The `(rnrs control (6))` library provides syntactic forms useful
for constructing conditional expressions and controlling the flow of
execution.

Scheme Syntax: **when** test expression1 expression2 ... [¶](#index-when-2)

Scheme Syntax: **unless** test expression1 expression2 ... [¶](#index-unless-2)
:   The `when` form is evaluated by evaluating the specified test
    expression; if the result is a true value, the expressions that
    follow it are evaluated in order, and the value of the final
    expression becomes the value of the entire `when` expression.

    The `unless` form behaves similarly, with the exception that the
    specified expressions are only evaluated if the value of
    test is false.

Scheme Syntax: **do** ((variable init step) ...) (test expression ...) command ... [¶](#index-do-1)
:   This form is identical to the one provided by Guile’s core library.
    See [Iteration mechanisms](#while-do), for documentation.

Scheme Syntax: **case-lambda** clause ... [¶](#index-case_002dlambda-1)
:   This form is identical to the one provided by Guile’s core library.
    See [Case-lambda](#Case_002dlambda), for documentation.

---

Next: [rnrs records syntactic](#rnrs-records-syntactic), Previous: [rnrs control](#rnrs-control), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.8 R6RS Records [¶](#R6RS-Records-1)

The manual sections below describe Guile’s implementation of R6RS
records, which provide support for user-defined data types. The R6RS
records API provides a superset of the features provided by Guile’s
“native” records, as well as those of the SRFI-9 records API;
See [Records](#Records), and [SRFI-9 Records](#SRFI_002d9-Records), for a description of those
interfaces.

As with SRFI-9 and Guile’s native records, R6RS records are constructed
using a record-type descriptor that specifies attributes like the
record’s name, its fields, and the mutability of those fields.

R6RS records extend this framework to support single inheritance via the
specification of a “parent” type for a record type at definition time.
Accessors and mutator procedures for the fields of a parent type may be
applied to records of a subtype of this parent. A record type may be
*sealed*, in which case it cannot be used as the parent of another
record type.

The inheritance mechanism for record types also informs the process of
initializing the fields of a record and its parents. Constructor
procedures that generate new instances of a record type are obtained
from a record constructor descriptor, which encapsulates the record-type
descriptor of the record to be constructed along with a *protocol*
procedure that defines how constructors for record subtypes delegate to
the constructors of their parent types.

A protocol is a procedure used by the record system at construction time
to bind arguments to the fields of the record being constructed. The
protocol procedure is passed a procedure n that accepts the
arguments required to construct the record’s parent type; this
procedure, when invoked, will return a procedure p that accepts
the arguments required to construct a new instance of the record type
itself and returns a new instance of the record type.

The protocol should in turn return a procedure that uses n and
p to initialize the fields of the record type and its parent
type(s). This procedure will be the constructor returned by

As a trivial example, consider the hypothetical record type
`pixel`, which encapsulates an x-y location on a screen, and
`voxel`, which has `pixel` as its parent type and stores an
additional coordinate. The following protocol produces a constructor
procedure that accepts all three coordinates, uses the first two to
initialize the fields of `pixel`, and binds the third to the single
field of `voxel`.

```
  (lambda (n)
    (lambda (x y z)
      (let ((p (n x y)))
        (p z))))
```

It may be helpful to think of protocols as “constructor factories”
that produce chains of delegating constructors glued together by the
helper procedure n.

An R6RS record type may be declared to be *nongenerative* via the
use of a unique generated or user-supplied symbol—or
*uid*—such that subsequent record type declarations with the same
uid and attributes will return the previously-declared record-type
descriptor.

R6RS record types may also be declared to be *opaque*, in which case
the various predicates and introspection procedures defined in
`(rnrs records introspection)` will behave as if records of this
type are not records at all.

Note that while the R6RS records API shares much of its namespace with
both the SRFI-9 and native Guile records APIs, it is not currently
compatible with either.

---

Next: [rnrs records procedural](#rnrs-records-procedural), Previous: [R6RS Records](#R6RS-Records), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.9 rnrs records syntactic [¶](#rnrs-records-syntactic-1)

The `(rnrs records syntactic (6))` library exports the syntactic
API for working with R6RS records.

Scheme Syntax: **define-record-type** name-spec record-clause … [¶](#index-define_002drecord_002dtype-1)
:   Defines a new record type, introducing bindings for a record-type
    descriptor, a record constructor descriptor, a constructor procedure,
    a record predicate, and accessor and mutator procedures for the new
    record type’s fields.

    name-spec must either be an identifier or must take the form
    `(record-name constructor-name predicate-name)`, where
    record-name, constructor-name, and predicate-name are
    all identifiers and specify the names to which, respectively, the
    record-type descriptor, constructor, and predicate procedures will be
    bound. If name-spec is only an identifier, it specifies the name
    to which the generated record-type descriptor will be bound.

    Each record-clause must be one of the following:

    * `(fields field-spec*)`, where each field-spec specifies a
      field of the new record type and takes one of the following forms:
      + `(immutable field-name accessor-name)`, which specifies an
        immutable field with the name field-name and binds an accessor
        procedure for it to the name given by accessor-name
      + `(mutable field-name accessor-name mutator-name)`, which specifies
        a mutable field with the name field-name and binds accessor and
        mutator procedures to accessor-name and mutator-name,
        respectively
      + `(immutable field-name)`, which specifies an immutable field with
        the name field-name; an accessor procedure for it will be created
        and named by appending record name and field-name with a hyphen
        separator
      + `(mutable field-name`), which specifies a mutable field with the
        name field-name; an accessor procedure for it will be created and
        named as described above; a mutator procedure will also be created and
        named by appending `-set!` to the accessor name
      + `field-name`, which specifies an immutable field with the name
        field-name; an access procedure for it will be created and named
        as described above
    * `(parent parent-name)`, where parent-name is a symbol giving
      the name of the record type to be used as the parent of the new record
      type
    * `(protocol expression)`, where expression evaluates to a
      protocol procedure which behaves as described above, and is used to
      create a record constructor descriptor for the new record type
    * `(sealed sealed?)`, where sealed? is a boolean value that
      specifies whether or not the new record type is sealed
    * `(opaque opaque?)`, where opaque? is a boolean value that
      specifies whether or not the new record type is opaque
    * `(nongenerative [uid])`, which specifies that the record type is
      nongenerative via the optional uid uid. If uid is not
      specified, a unique uid will be generated at expansion time
    * `(parent-rtd parent-rtd parent-cd)`, a more explicit form of the
      `parent` form above; parent-rtd and parent-cd should
      evaluate to a record-type descriptor and a record constructor
      descriptor, respectively

Scheme Syntax: **record-type-descriptor** record-name [¶](#index-record_002dtype_002ddescriptor-1)
:   Evaluates to the record-type descriptor associated with the type
    specified by record-name.

Scheme Syntax: **record-constructor-descriptor** record-name [¶](#index-record_002dconstructor_002ddescriptor)
:   Evaluates to the record-constructor descriptor associated with the type
    specified by record-name.

---

Next: [rnrs records inspection](#rnrs-records-inspection), Previous: [rnrs records syntactic](#rnrs-records-syntactic), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.10 rnrs records procedural [¶](#rnrs-records-procedural-1)

The `(rnrs records procedural (6))` library exports the procedural
API for working with R6RS records.

Scheme Procedure: **make-record-type-descriptor** name parent uid sealed? opaque? fields [¶](#index-make_002drecord_002dtype_002ddescriptor)
:   Returns a new record-type descriptor with the specified characteristics:
    name must be a symbol giving the name of the new record type;
    parent must be either `#f` or a non-sealed record-type
    descriptor for the returned record type to extend; uid must be
    either `#f`, indicating that the record type is generative, or
    a symbol giving the type’s nongenerative uid; sealed? and
    opaque? must be boolean values that specify the sealedness and
    opaqueness of the record type; fields must be a vector of zero or
    more field specifiers of the form `(mutable name)` or
    `(immutable name)`, where name is a symbol giving a name for the
    field.

    If uid is not `#f`, it must be a symbol

Scheme Procedure: **record-type-descriptor?** obj [¶](#index-record_002dtype_002ddescriptor_003f)
:   Returns `#t` if obj is a record-type descriptor, `#f`
    otherwise.

Scheme Procedure: **make-record-constructor-descriptor** rtd parent-constructor-descriptor protocol [¶](#index-make_002drecord_002dconstructor_002ddescriptor)
:   Returns a new record constructor descriptor that can be used to produce
    constructors for the record type specified by the record-type descriptor
    rtd and whose delegation and binding behavior are specified by the
    protocol procedure protocol.

    parent-constructor-descriptor specifies a record constructor
    descriptor for the parent type of rtd, if one exists. If
    rtd represents a base type, then
    parent-constructor-descriptor must be `#f`. If rtd
    is an extension of another type, parent-constructor-descriptor may
    still be `#f`, but protocol must also be `#f` in this case.

Scheme Procedure: **record-constructor** rcd [¶](#index-record_002dconstructor-1)
:   Returns a record constructor procedure by invoking the protocol
    defined by the record-constructor descriptor rcd.

Scheme Procedure: **record-predicate** rtd [¶](#index-record_002dpredicate-1)
:   Returns the record predicate procedure for the record-type descriptor
    rtd.

Scheme Procedure: **record-accessor** rtd k [¶](#index-record_002daccessor-1)
:   Returns the record field accessor procedure for the kth field of
    the record-type descriptor rtd.

Scheme Procedure: **record-mutator** rtd k [¶](#index-record_002dmutator)
:   Returns the record field mutator procedure for the kth field of
    the record-type descriptor rtd. An `&assertion` condition
    will be raised if this field is not mutable.

---

Next: [rnrs exceptions](#rnrs-exceptions), Previous: [rnrs records procedural](#rnrs-records-procedural), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.11 rnrs records inspection [¶](#rnrs-records-inspection-1)

The `(rnrs records inspection (6))` library provides procedures
useful for accessing metadata about R6RS records.

Scheme Procedure: **record?** obj [¶](#index-record_003f-1)
:   Return `#t` if the specified object is a non-opaque R6RS record,
    `#f` otherwise.

Scheme Procedure: **record-rtd** record [¶](#index-record_002drtd)
:   Returns the record-type descriptor for record. An
    `&assertion` is raised if record is opaque.

Scheme Procedure: **record-type-name** rtd [¶](#index-record_002dtype_002dname-1)
:   Returns the name of the record-type descriptor rtd.

Scheme Procedure: **record-type-parent** rtd [¶](#index-record_002dtype_002dparent)
:   Returns the parent of the record-type descriptor rtd, or `#f`
    if it has none.

Scheme Procedure: **record-type-uid** rtd [¶](#index-record_002dtype_002duid)
:   Returns the uid of the record-type descriptor rtd, or `#f` if
    it has none.

Scheme Procedure: **record-type-generative?** rtd [¶](#index-record_002dtype_002dgenerative_003f)
:   Returns `#t` if the record-type descriptor rtd is generative,
    `#f` otherwise.

Scheme Procedure: **record-type-sealed?** rtd [¶](#index-record_002dtype_002dsealed_003f)
:   Returns `#t` if the record-type descriptor rtd is sealed,
    `#f` otherwise.

Scheme Procedure: **record-type-opaque?** rtd [¶](#index-record_002dtype_002dopaque_003f)
:   Returns `#t` if the record-type descriptor rtd is opaque,
    `#f` otherwise.

Scheme Procedure: **record-type-field-names** rtd [¶](#index-record_002dtype_002dfield_002dnames)
:   Returns a vector of symbols giving the names of the fields defined by
    the record-type descriptor rtd (and not any of its sub- or
    supertypes).

Scheme Procedure: **record-field-mutable?** rtd k [¶](#index-record_002dfield_002dmutable_003f)
:   Returns `#t` if the field at index k of the record-type
    descriptor rtd (and not any of its sub- or supertypes) is mutable.

---

Next: [rnrs conditions](#rnrs-conditions), Previous: [rnrs records inspection](#rnrs-records-inspection), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.12 rnrs exceptions [¶](#rnrs-exceptions-1)

The `(rnrs exceptions (6))` library provides functionality related
to signaling and handling exceptional situations. This functionality
re-exports Guile’s core exception-handling primitives.
See [Exceptions](#Exceptions), for a full discussion. See [SRFI-34 - Exception handling for programs](#SRFI_002d34), for a similar
pre-R6RS facility. In Guile, SRFI-34, SRFI-35, and R6RS exception
handling are all built on the same core facilities, and so are
interoperable.

Scheme Procedure: **with-exception-handler** handler thunk [¶](#index-with_002dexception_002dhandler-2)
:   See [Raising and Handling Exceptions](#Raising-and-Handling-Exceptions), for more information on
    `with-exception-handler`.

Scheme Syntax: **guard** (variable clause1 clause2 ...) body [¶](#index-guard-1)
:   Evaluates the expression given by body, first creating an ad hoc
    exception handler that binds a raised exception to variable and
    then evaluates the specified clauses as if they were part of a
    `cond` expression, with the value of the first matching clause
    becoming the value of the `guard` expression
    (see [Simple Conditional Evaluation](#Conditionals)). If none of the clause’s test expressions
    evaluates to `#t`, the exception is re-raised, with the exception
    handler that was current before the evaluation of the `guard` form.

    For example, the expression

    ```
    (guard (ex ((eq? ex 'foo) 'bar) ((eq? ex 'bar) 'baz)) 
      (raise 'bar))
    ```

    evaluates to `baz`.

Scheme Procedure: **raise** obj [¶](#index-raise-2)
:   Equivalent to core Guile `(raise-exception obj)`.
    See [Raising and Handling Exceptions](#Raising-and-Handling-Exceptions). (Unfortunately, `raise`
    is already bound to a different function in core Guile.
    See [Signals](#Signals).)

Scheme Procedure: **raise-continuable** obj [¶](#index-raise_002dcontinuable)
:   Equivalent to core Guile `(raise-exception obj #:continuable?
    #t)`. See [Raising and Handling Exceptions](#Raising-and-Handling-Exceptions).

---

Next: [I/O Conditions](#R6RS-I_002fO-Conditions), Previous: [rnrs exceptions](#rnrs-exceptions), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.13 rnrs conditions [¶](#rnrs-conditions-1)

The `(rnrs condition (6))` library provides forms and procedures
for constructing new condition types, as well as a library of
pre-defined condition types that represent a variety of common
exceptional situations. Conditions are records of a subtype of the
`&condition` record type, which is neither sealed nor opaque.
See [R6RS Records](#R6RS-Records).

Conditions may be manipulated singly, as *simple conditions*, or
when composed with other conditions to form *compound conditions*.
Compound conditions do not “nest”—constructing a new compound
condition out of existing compound conditions will “flatten” them
into their component simple conditions. For example, making a new
condition out of a `&message` condition and a compound condition
that contains an `&assertion` condition and another `&message`
condition will produce a compound condition that contains two
`&message` conditions and one `&assertion` condition.

The record type predicates and field accessors described below can
operate on either simple or compound conditions. In the latter case,
the predicate returns `#t` if the compound condition contains a
component simple condition of the appropriate type; the field accessors
return the requisite fields from the first component simple condition
found to be of the appropriate type.

Guile’s R6RS layer uses core exception types from the `(ice-9
exceptions)` module as the basis for its R6RS condition system. Guile
prefers to use the term “exception object” and “exception type”
rather than “condition” or “condition type”, but that’s just a
naming difference. Guile also has different names for the types in the
condition hierarchy. See [Exception Objects](#Exception-Objects), for full details.

This library is quite similar to the SRFI-35 conditions module
(see [SRFI-35 - Conditions](#SRFI_002d35)). Among other minor differences, the `(rnrs
conditions)` library features slightly different semantics around
condition field accessors, and comes with a larger number of pre-defined
condition types. The two APIs are compatible; the `condition?`
predicate from one API will return `#t` when applied to a condition
object created in the other. of the condition types are the same,
also.

Condition Type: **&condition** [¶](#index-_0026condition)

Scheme Procedure: **condition?** obj [¶](#index-condition_003f)
:   The base record type for conditions. Known as `&exception` in core
    Guile.

Scheme Procedure: **condition** condition1 ... [¶](#index-condition-1)

Scheme Procedure: **simple-conditions** condition [¶](#index-simple_002dconditions)
:   The `condition` procedure creates a new compound condition out of
    its condition arguments, flattening any specified compound conditions
    into their component simple conditions as described above.

    `simple-conditions` returns a list of the component simple
    conditions of the compound condition `condition`, in the order in
    which they were specified at construction time.

Scheme Procedure: **condition-predicate** rtd [¶](#index-condition_002dpredicate)

Scheme Procedure: **condition-accessor** rtd proc [¶](#index-condition_002daccessor)
:   These procedures return condition predicate and accessor procedures for
    the specified condition record type rtd.

Scheme Syntax: **define-condition-type** condition-type supertype constructor predicate field-spec ... [¶](#index-define_002dcondition_002dtype-1)
:   Evaluates to a new record type definition for a condition type with the
    name condition-type that has the condition type supertype as
    its parent. A default constructor, which binds its arguments to the
    fields of this type and its parent types, will be bound to the
    identifier constructor; a condition predicate will be bound to
    predicate. The fields of the new type, which are immutable, are
    specified by the field-specs, each of which must be of the form:

    ```
    (field accessor)
    ```

    where field gives the name of the field and accessor gives
    the name for a binding to an accessor procedure created for this field.

Condition Type: **&message** [¶](#index-_0026message)

Scheme Procedure: **make-message-condition** message [¶](#index-make_002dmessage_002dcondition)

Scheme Procedure: **message-condition?** obj [¶](#index-message_002dcondition_003f-1)

Scheme Procedure: **condition-message** condition [¶](#index-condition_002dmessage-1)
:   A type that includes a message describing the condition that occurred.

Condition Type: **&warning** [¶](#index-_0026warning)

Scheme Procedure: **make-warning** [¶](#index-make_002dwarning-1)

Scheme Procedure: **warning?** obj [¶](#index-warning_003f-1)
:   A base type for representing non-fatal conditions during execution.

Condition Type: **&serious** [¶](#index-_0026serious)

Scheme Procedure: **make-serious-condition** [¶](#index-make_002dserious_002dcondition)

Scheme Procedure: **serious-condition?** obj [¶](#index-serious_002dcondition_003f-1)
:   A base type for conditions representing errors serious enough that
    cannot be ignored. Known as `&error` in core Guile.

Condition Type: **&error** [¶](#index-_0026error)

Scheme Procedure: **make-error** [¶](#index-make_002derror-1)

Scheme Procedure: **error?** obj [¶](#index-error_003f-2)
:   A base type for conditions representing errors. Known as
    `&external-error` in core Guile.

Condition Type: **&violation** [¶](#index-_0026violation)

Scheme Procedure: **make-violation** [¶](#index-make_002dviolation)

Scheme Procedure: **violation?** [¶](#index-violation_003f)
:   A subtype of `&serious` that can be used to represent violations of
    a language or library standard. Known as `&programming-error` in
    core Guile.

Condition Type: **&assertion** [¶](#index-_0026assertion)

Scheme Procedure: **make-assertion-violation** [¶](#index-make_002dassertion_002dviolation)

Scheme Procedure: **assertion-violation?** obj [¶](#index-assertion_002dviolation_003f)
:   A subtype of `&violation` that indicates an invalid call to a
    procedure. Known as `&assertion-failure` in core Guile.

Condition Type: **&irritants** [¶](#index-_0026irritants)

Scheme Procedure: **make-irritants-condition** irritants [¶](#index-make_002dirritants_002dcondition)

Scheme Procedure: **irritants-condition?** obj [¶](#index-irritants_002dcondition_003f)

Scheme Procedure: **condition-irritants** condition [¶](#index-condition_002dirritants)
:   A base type used for storing information about the causes of another
    condition in a compound condition.

Condition Type: **&who** [¶](#index-_0026who)

Scheme Procedure: **make-who-condition** who [¶](#index-make_002dwho_002dcondition)

Scheme Procedure: **who-condition?** obj [¶](#index-who_002dcondition_003f)

Scheme Procedure: **condition-who** condition [¶](#index-condition_002dwho)
:   A base type used for storing the identity, a string or symbol, of the
    entity responsible for another condition in a compound condition.

Condition Type: **&non-continuable** [¶](#index-_0026non_002dcontinuable)

Scheme Procedure: **make-non-continuable-violation** [¶](#index-make_002dnon_002dcontinuable_002dviolation)

Scheme Procedure: **non-continuable-violation?** obj [¶](#index-non_002dcontinuable_002dviolation_003f)
:   A subtype of `&violation` used to indicate that an exception
    handler invoked by `raise` has returned locally.

Condition Type: **&implementation-restriction** [¶](#index-_0026implementation_002drestriction)

Scheme Procedure: **make-implementation-restriction-violation** [¶](#index-make_002dimplementation_002drestriction_002dviolation)

Scheme Procedure: **implementation-restriction-violation?** obj [¶](#index-implementation_002drestriction_002dviolation_003f)
:   A subtype of `&violation` used to indicate a violation of an
    implementation restriction.

Condition Type: **&lexical** [¶](#index-_0026lexical)

Scheme Procedure: **make-lexical-violation** [¶](#index-make_002dlexical_002dviolation)

Scheme Procedure: **lexical-violation?** obj [¶](#index-lexical_002dviolation_003f)
:   A subtype of `&violation` used to indicate a syntax violation at
    the level of the datum syntax.

Condition Type: **&syntax** [¶](#index-_0026syntax)

Scheme Procedure: **make-syntax-violation** form subform [¶](#index-make_002dsyntax_002dviolation)

Scheme Procedure: **syntax-violation?** obj [¶](#index-syntax_002dviolation_003f)

Scheme Procedure: **syntax-violation-form** condition [¶](#index-syntax_002dviolation_002dform)

Scheme Procedure: **syntax-violation-subform** condition [¶](#index-syntax_002dviolation_002dsubform)
:   A subtype of `&violation` that indicates a syntax violation. The
    form and subform fields, which must be datum values,
    indicate the syntactic form responsible for the condition.

Condition Type: **&undefined** [¶](#index-_0026undefined)

Scheme Procedure: **make-undefined-violation** [¶](#index-make_002dundefined_002dviolation)

Scheme Procedure: **undefined-violation?** obj [¶](#index-undefined_002dviolation_003f)
:   A subtype of `&violation` that indicates a reference to an unbound
    identifier. Known as `&undefined-variable` in core Guile.

---

Next: [Transcoders](#R6RS-Transcoders), Previous: [rnrs conditions](#rnrs-conditions), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.14 I/O Conditions [¶](#I_002fO-Conditions)

These condition types are exported by both the
`(rnrs io ports (6))` and `(rnrs io simple (6))` libraries.

Condition Type: **&i/o** [¶](#index-_0026i_002fo)

Scheme Procedure: **make-i/o-error** [¶](#index-make_002di_002fo_002derror)

Scheme Procedure: **i/o-error?** obj [¶](#index-i_002fo_002derror_003f)
:   A condition supertype for more specific I/O errors.

Condition Type: **&i/o-read** [¶](#index-_0026i_002fo_002dread)

Scheme Procedure: **make-i/o-read-error** [¶](#index-make_002di_002fo_002dread_002derror)

Scheme Procedure: **i/o-read-error?** obj [¶](#index-i_002fo_002dread_002derror_003f)
:   A subtype of `&i/o`; represents read-related I/O errors.

Condition Type: **&i/o-write** [¶](#index-_0026i_002fo_002dwrite)

Scheme Procedure: **make-i/o-write-error** [¶](#index-make_002di_002fo_002dwrite_002derror)

Scheme Procedure: **i/o-write-error?** obj [¶](#index-i_002fo_002dwrite_002derror_003f)
:   A subtype of `&i/o`; represents write-related I/O errors.

Condition Type: **&i/o-invalid-position** [¶](#index-_0026i_002fo_002dinvalid_002dposition)

Scheme Procedure: **make-i/o-invalid-position-error** position [¶](#index-make_002di_002fo_002dinvalid_002dposition_002derror)

Scheme Procedure: **i/o-invalid-position-error?** obj [¶](#index-i_002fo_002dinvalid_002dposition_002derror_003f)

Scheme Procedure: **i/o-error-position** condition [¶](#index-i_002fo_002derror_002dposition)
:   A subtype of `&i/o`; represents an error related to an attempt to
    set the file position to an invalid position.

Condition Type: **&i/o-filename** [¶](#index-_0026i_002fo_002dfilename)

Scheme Procedure: **make-io-filename-error** filename [¶](#index-make_002dio_002dfilename_002derror)

Scheme Procedure: **i/o-filename-error?** obj [¶](#index-i_002fo_002dfilename_002derror_003f)

Scheme Procedure: **i/o-error-filename** condition [¶](#index-i_002fo_002derror_002dfilename)
:   A subtype of `&i/o`; represents an error related to an operation on
    a named file.

Condition Type: **&i/o-file-protection** [¶](#index-_0026i_002fo_002dfile_002dprotection)

Scheme Procedure: **make-i/o-file-protection-error** filename [¶](#index-make_002di_002fo_002dfile_002dprotection_002derror)

Scheme Procedure: **i/o-file-protection-error?** obj [¶](#index-i_002fo_002dfile_002dprotection_002derror_003f)
:   A subtype of `&i/o-filename`; represents an error resulting from an
    attempt to access a named file for which the caller had insufficient
    permissions.

Condition Type: **&i/o-file-is-read-only** [¶](#index-_0026i_002fo_002dfile_002dis_002dread_002donly)

Scheme Procedure: **make-i/o-file-is-read-only-error** filename [¶](#index-make_002di_002fo_002dfile_002dis_002dread_002donly_002derror)

Scheme Procedure: **i/o-file-is-read-only-error?** obj [¶](#index-i_002fo_002dfile_002dis_002dread_002donly_002derror_003f)
:   A subtype of `&i/o-file-protection`; represents an error related to
    an attempt to write to a read-only file.

Condition Type: **&i/o-file-already-exists** [¶](#index-_0026i_002fo_002dfile_002dalready_002dexists)

Scheme Procedure: **make-i/o-file-already-exists-error** filename [¶](#index-make_002di_002fo_002dfile_002dalready_002dexists_002derror)

Scheme Procedure: **i/o-file-already-exists-error?** obj [¶](#index-i_002fo_002dfile_002dalready_002dexists_002derror_003f)
:   A subtype of `&i/o-filename`; represents an error related to an
    operation on an existing file that was assumed not to exist.

Condition Type: **&i/o-file-does-not-exist** [¶](#index-_0026i_002fo_002dfile_002ddoes_002dnot_002dexist)

Scheme Procedure: **make-i/o-file-does-not-exist-error** [¶](#index-make_002di_002fo_002dfile_002ddoes_002dnot_002dexist_002derror)

Scheme Procedure: **i/o-file-does-not-exist-error?** obj [¶](#index-i_002fo_002dfile_002ddoes_002dnot_002dexist_002derror_003f)
:   A subtype of `&i/o-filename`; represents an error related to an
    operation on a non-existent file that was assumed to exist.

Condition Type: **&i/o-port** [¶](#index-_0026i_002fo_002dport)

Scheme Procedure: **make-i/o-port-error** port [¶](#index-make_002di_002fo_002dport_002derror)

Scheme Procedure: **i/o-port-error?** obj [¶](#index-i_002fo_002dport_002derror_003f)

Scheme Procedure: **i/o-error-port** condition [¶](#index-i_002fo_002derror_002dport)
:   A subtype of `&i/o`; represents an error related to an operation on
    the port port.

---

Next: [rnrs io ports](#rnrs-io-ports), Previous: [I/O Conditions](#R6RS-I_002fO-Conditions), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.15 Transcoders [¶](#Transcoders)

The transcoder facilities are exported by `(rnrs io ports)`.

Several different Unicode encoding schemes describe standard ways to
encode characters and strings as byte sequences and to decode those
sequences. Within this document, a *codec* is an immutable Scheme
object that represents a Unicode or similar encoding scheme.

An *end-of-line style* is a symbol that, if it is not `none`,
describes how a textual port transcodes representations of line endings.

A *transcoder* is an immutable Scheme object that combines a codec
with an end-of-line style and a method for handling decoding errors.
Each transcoder represents some specific bidirectional (but not
necessarily lossless), possibly stateful translation between byte
sequences and Unicode characters and strings. Every transcoder can
operate in the input direction (bytes to characters) or in the output
direction (characters to bytes). A transcoder parameter name
means that the corresponding argument must be a transcoder.

A *binary port* is a port that supports binary I/O, does not have an
associated transcoder and does not support textual I/O. A *textual
port* is a port that supports textual I/O, and does not support binary
I/O. A textual port may or may not have an associated transcoder.

Scheme Procedure: **latin-1-codec** [¶](#index-latin_002d1_002dcodec)

Scheme Procedure: **utf-8-codec** [¶](#index-utf_002d8_002dcodec)

Scheme Procedure: **utf-16-codec** [¶](#index-utf_002d16_002dcodec)
:   These are predefined codecs for the ISO 8859-1, UTF-8, and UTF-16
    encoding schemes.

    A call to any of these procedures returns a value that is equal in the
    sense of `eqv?` to the result of any other call to the same
    procedure.

Scheme Syntax: **eol-style** eol-style-symbol [¶](#index-eol_002dstyle)
:   eol-style-symbol should be a symbol whose name is one of
    `lf`, `cr`, `crlf`, `nel`, `crnel`, `ls`,
    and `none`.

    The form evaluates to the corresponding symbol. If the name of
    eol-style-symbol is not one of these symbols, the effect and
    result are implementation-dependent; in particular, the result may be an
    eol-style symbol acceptable as an eol-style argument to
    `make-transcoder`. Otherwise, an exception is raised.

    All eol-style symbols except `none` describe a specific
    line-ending encoding:

    `lf`
    :   linefeed

    `cr`
    :   carriage return

    `crlf`
    :   carriage return, linefeed

    `nel`
    :   next line

    `crnel`
    :   carriage return, next line

    `ls`
    :   line separator

    For a textual port with a transcoder, and whose transcoder has an
    eol-style symbol `none`, no conversion occurs. For a textual input
    port, any eol-style symbol other than `none` means that all of the
    above line-ending encodings are recognized and are translated into a
    single linefeed. For a textual output port, `none` and `lf`
    are equivalent. Linefeed characters are encoded according to the
    specified eol-style symbol, and all other characters that participate in
    possible line endings are encoded as is.

    > **Note:** Only the name of eol-style-symbol is significant.

Scheme Procedure: **native-eol-style** [¶](#index-native_002deol_002dstyle)
:   Returns the default end-of-line style of the underlying platform, e.g.,
    `lf` on Unix and `crlf` on Windows.

Condition Type: **&i/o-decoding** [¶](#index-_0026i_002fo_002ddecoding)

Scheme Procedure: **make-i/o-decoding-error** port [¶](#index-make_002di_002fo_002ddecoding_002derror)

Scheme Procedure: **i/o-decoding-error?** obj [¶](#index-i_002fo_002ddecoding_002derror_003f)
:   This condition type could be defined by

    ```
    (define-condition-type &i/o-decoding &i/o-port
      make-i/o-decoding-error i/o-decoding-error?)
    ```

    An exception with this type is raised when one of the operations for
    textual input from a port encounters a sequence of bytes that cannot be
    translated into a character or string by the input direction of the
    port’s transcoder.

    When such an exception is raised, the port’s position is past the
    invalid encoding.

Condition Type: **&i/o-encoding** [¶](#index-_0026i_002fo_002dencoding)

Scheme Procedure: **make-i/o-encoding-error** port char [¶](#index-make_002di_002fo_002dencoding_002derror)

Scheme Procedure: **i/o-encoding-error?** obj [¶](#index-i_002fo_002dencoding_002derror_003f)

Scheme Procedure: **i/o-encoding-error-char** condition [¶](#index-i_002fo_002dencoding_002derror_002dchar)
:   This condition type could be defined by

    ```
    (define-condition-type &i/o-encoding &i/o-port
      make-i/o-encoding-error i/o-encoding-error?
      (char i/o-encoding-error-char))
    ```

    An exception with this type is raised when one of the operations for
    textual output to a port encounters a character that cannot be
    translated into bytes by the output direction of the port’s transcoder.
    char is the character that could not be encoded.

Scheme Syntax: **error-handling-mode** error-handling-mode-symbol [¶](#index-error_002dhandling_002dmode)
:   error-handling-mode-symbol should be a symbol whose name is one of
    `ignore`, `raise`, and `replace`. The form evaluates to
    the corresponding symbol. If error-handling-mode-symbol is not
    one of these identifiers, effect and result are
    implementation-dependent: The result may be an error-handling-mode
    symbol acceptable as a handling-mode argument to
    `make-transcoder`. If it is not acceptable as a
    handling-mode argument to `make-transcoder`, an exception is
    raised.

    > **Note:** Only the name of error-handling-mode-symbol is significant.

    The error-handling mode of a transcoder specifies the behavior
    of textual I/O operations in the presence of encoding or decoding
    errors.

    If a textual input operation encounters an invalid or incomplete
    character encoding, and the error-handling mode is `ignore`, an
    appropriate number of bytes of the invalid encoding are ignored and
    decoding continues with the following bytes.

    If the error-handling mode is `replace`, the replacement
    character U+FFFD is injected into the data stream, an appropriate
    number of bytes are ignored, and decoding
    continues with the following bytes.

    If the error-handling mode is `raise`, an exception with condition
    type `&i/o-decoding` is raised.

    If a textual output operation encounters a character it cannot encode,
    and the error-handling mode is `ignore`, the character is ignored
    and encoding continues with the next character. If the error-handling
    mode is `replace`, a codec-specific replacement character is
    emitted by the transcoder, and encoding continues with the next
    character. The replacement character is U+FFFD for transcoders whose
    codec is one of the Unicode encodings, but is the `?` character
    for the Latin-1 encoding. If the error-handling mode is `raise`,
    an exception with condition type `&i/o-encoding` is raised.

Scheme Procedure: **make-transcoder** codec [¶](#index-make_002dtranscoder)

Scheme Procedure: **make-transcoder** codec eol-style [¶](#index-make_002dtranscoder-1)

Scheme Procedure: **make-transcoder** codec eol-style handling-mode [¶](#index-make_002dtranscoder-2)
:   codec must be a codec; eol-style, if present, an eol-style
    symbol; and handling-mode, if present, an error-handling-mode
    symbol.

    eol-style may be omitted, in which case it defaults to the native
    end-of-line style of the underlying platform. handling-mode may
    be omitted, in which case it defaults to `replace`. The result is
    a transcoder with the behavior specified by its arguments.

Scheme procedure: **native-transcoder** [¶](#index-native_002dtranscoder)
:   Returns an implementation-dependent transcoder that represents a
    possibly locale-dependent “native” transcoding.

Scheme Procedure: **transcoder-codec** transcoder [¶](#index-transcoder_002dcodec)

Scheme Procedure: **transcoder-eol-style** transcoder [¶](#index-transcoder_002deol_002dstyle)

Scheme Procedure: **transcoder-error-handling-mode** transcoder [¶](#index-transcoder_002derror_002dhandling_002dmode)
:   These are accessors for transcoder objects; when applied to a
    transcoder returned by `make-transcoder`, they return the
    codec, eol-style, and handling-mode arguments,
    respectively.

Scheme Procedure: **bytevector->string** bytevector transcoder [¶](#index-bytevector_002d_003estring-1)
:   Returns the string that results from transcoding the
    bytevector according to the input direction of the transcoder.

Scheme Procedure: **string->bytevector** string transcoder [¶](#index-string_002d_003ebytevector-1)
:   Returns the bytevector that results from transcoding the
    string according to the output direction of the transcoder.

---

Next: [R6RS File Ports](#R6RS-File-Ports), Previous: [Transcoders](#R6RS-Transcoders), Up: [R6RS Standard Libraries](#R6RS-Standard-Libraries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.6.2.16 rnrs io ports [¶](#rnrs-io-ports-1)

Guile’s binary and textual port interface was heavily inspired by R6RS,
so many R6RS port interfaces are documented elsewhere. Note that R6RS
ports are not disjoint from Guile’s native ports, so Guile-specific
procedures will work on ports created using the R6RS API, and vice
versa. Also note that in Guile, all ports are both textual and binary.
See [Input and Output](#Input-and-Output), for more on Guile’s core port API. The R6RS
ports module wraps Guile’s I/O routines in a helper that will translate
native Guile exceptions to R6RS conditions; See [I/O Conditions](#R6RS-I_002fO-Conditions),
for more. See [R6RS File Ports](#R6RS-File-Ports), for documentation on the R6RS file
port interface.

*Note*: The implementation of this R6RS API is not complete yet.

Scheme Procedure: **eof-object?** obj [¶](#index-eof_002dobject_003f-1)
:   See [Binary I/O](#Binary-I_002fO), for documentation.

Scheme Procedure: **eof-object** [¶](#index-eof_002dobject)
:   Return the end-of-file (EOF) object.

    ```
    (eof-object? (eof-object))
    ⇒ #t
    ```

Scheme Procedure: **port?** obj [¶](#index-port_003f-1)

Scheme Procedure: **input-port?** obj [¶](#index-input_002dport_003f-1)

Scheme Procedure: **output-port?** obj [¶](#index-output_002dport_003f-1)

Scheme Procedure: **call-with-port** port proc [¶](#index-call_002dwith_002dport-1)
:   See [Ports](#Ports), for documentation.

Scheme Procedure: **port-transcoder** port [¶](#index-port_002dtranscoder)
:   Return a transcoder associated with the encoding of port.
    See [Encoding](#Encoding), and See [Transcoders](#R6RS-Transcoders).

Scheme Procedure: **binary-port?** port [¶](#index-binary_002dport_003f)
:   Return `#t` if port appears to be a binary port, else return
    `#f`. Note that Guile does not currently distinguish between
    binary and textual ports, so this predicate is not a reliable indicator
    of whether the port was created as a binary port. Currently, it returns
    `#t` if and only if the port encoding is “ISO-8859-1”, because
    Guile uses this encoding when creating a binary port. See [Encoding](#Encoding),
    for more details.

Scheme Procedure: **textual-port?** port [¶](#index-textual_002dport_003f)
:   Return `#t` if port appears to be a textual port, else return
    `#f`. Note that Guile does not currently distinguish between
    binary and textual ports, so this predicate is not a reliable indicator
    of whether the port was created as a textual port. Currently, it always
    returns `#t`, because all ports can be used for textual I/O in
    Guile. See [Encoding](#Encoding), for more details.

Scheme Procedure: **transcoded-port** binary-port transcoder [¶](#index-transcoded_002dport)
:   The `transcoded-port` procedure
    returns a new textual port with the specified transcoder.
    Otherwise the new textual port’s state is largely the same as
    that of binary-port.
    If binary-port is an input port, the new textual
    port will be an input port and
    will transcode the bytes that have not yet been read from
    binary-port.
    If binary-port is an output port, the new textual
    port will be an output port and
    will transcode output characters into bytes that are
    written to the byte sink represented by binary-port.
