Objects which denote locations rather than values (cons cells, vectors,
    and non-zero-length strings in R5RS scheme; also Guile’s structs,
    bytevectors and ports and hash-tables), if they appear at more than one
    point in the data being written, are preceded by ‘`#N=`’ the
    first time they are written and replaced by ‘`#N#`’ all
    subsequent times they are written, where N is a natural number
    used to identify that particular object. If objects which denote
    locations occur only once in the structure, then
    `write-with-shared-structure` must produce the same external
    representation for those objects as `write`.

    `write-with-shared-structure` terminates in finite time and
    produces a finite representation when writing finite data.

    `write-with-shared-structure` returns an unspecified value. The
    port argument may be omitted, in which case it defaults to the
    value returned by `(current-output-port)`. The optarg
    argument may also be omitted. If present, its effects on the output and
    return value are unspecified but `write-with-shared-structure` must
    still write a representation that can be read by
    `read-with-shared-structure`. Some implementations may wish to use
    optarg to specify formatting conventions, numeric radixes, or
    return values. Guile’s implementation ignores optarg.

    For example, the code

    ```
    (begin (define a (cons 'val1 'val2))
           (set-cdr! a a)
           (write-with-shared-structure a))
    ```

    should produce the output `#1=(val1 . #1#)`. This shows a cons
    cell whose `cdr` contains itself.

Scheme procedure: **read-with-shared-structure** [¶](#index-read_002dwith_002dshared_002dstructure)

Scheme procedure: **read-with-shared-structure** port [¶](#index-read_002dwith_002dshared_002dstructure-1)
:   `read-with-shared-structure` converts the external representations
    of Scheme objects produced by `write-with-shared-structure` into
    Scheme objects. That is, it is a parser for the nonterminal
    ‘`<datum>`’ in the augmented external representation grammar defined
    above. `read-with-shared-structure` returns the next object
    parsable from the given input port, updating port to point to the
    first character past the end of the external representation of the
    object.

    If an end-of-file is encountered in the input before any characters are
    found that can begin an object, then an end-of-file object is returned.
    The port remains open, and further attempts to read it (by
    `read-with-shared-structure` or `read` will also return an
    end-of-file object. If an end of file is encountered after the
    beginning of an object’s external representation, but the external
    representation is incomplete and therefore not parsable, an error is
    signaled.

    The port argument may be omitted, in which case it defaults to the
    value returned by `(current-input-port)`. It is an error to read
    from a closed port.

---

Next: [SRFI-41 - Streams](#SRFI_002d41), Previous: [SRFI-38 - External Representation for Data With Shared Structure](#SRFI_002d38), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.27 SRFI-39 - Parameters [¶](#SRFI_002d39-_002d-Parameters)

This SRFI adds support for dynamically-scoped parameters. SRFI 39 is
implemented in the Guile core; there’s no module needed to get SRFI-39
itself. Parameters are documented in [Parameters](#Parameters).

This module does export one extra function: `with-parameters*`.
This is a Guile-specific addition to the SRFI, similar to the core
`with-fluids*` (see [Fluids and Dynamic States](#Fluids-and-Dynamic-States)).

Function: **with-parameters\*** param-list value-list thunk [¶](#index-with_002dparameters_002a)
:   Establish a new dynamic scope, as per `parameterize` above,
    taking parameters from param-list and corresponding values from
    value-list. A call `(thunk)` is made in the new
    scope and the result from that thunk is the return from
    `with-parameters*`.

---

Next: [SRFI-42 - Eager Comprehensions](#SRFI_002d42), Previous: [SRFI-39 - Parameters](#SRFI_002d39), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.28 SRFI-41 - Streams [¶](#SRFI_002d41-_002d-Streams)

This subsection is based on the
[specification of
SRFI-41](http://srfi.schemers.org/srfi-41/srfi-41.html) by Philip L. Bewig.

This SRFI implements streams, sometimes called lazy lists, a sequential
data structure containing elements computed only on demand. A stream is
either null or is a pair with a stream in its cdr. Since elements of a
stream are computed only when accessed, streams can be infinite. Once
computed, the value of a stream element is cached in case it is needed
again. SRFI-41 can be made available with:

```
(use-modules (srfi srfi-41))
```

* [SRFI-41 Stream Fundamentals](#SRFI_002d41-Stream-Fundamentals)
* [SRFI-41 Stream Primitives](#SRFI_002d41-Stream-Primitives)
* [SRFI-41 Stream Library](#SRFI_002d41-Stream-Library)

---

Next: [SRFI-41 Stream Primitives](#SRFI_002d41-Stream-Primitives), Up: [SRFI-41 - Streams](#SRFI_002d41)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.28.1 SRFI-41 Stream Fundamentals [¶](#SRFI_002d41-Stream-Fundamentals-1)

SRFI-41 Streams are based on two mutually-recursive abstract data types:
An object of the `stream` abstract data type is a promise that,
when forced, is either `stream-null` or is an object of type
`stream-pair`. An object of the `stream-pair` abstract data
type contains a `stream-car` and a `stream-cdr`, which must be
a `stream`. The essential feature of streams is the systematic
suspensions of the recursive promises between the two data types.

The object stored in the `stream-car` of a `stream-pair` is a
promise that is forced the first time the `stream-car` is accessed;
its value is cached in case it is needed again. The object may have any
type, and different stream elements may have different types. If the
`stream-car` is never accessed, the object stored there is never
evaluated. Likewise, the `stream-cdr` is a promise to return a
stream, and is only forced on demand.

---

Next: [SRFI-41 Stream Library](#SRFI_002d41-Stream-Library), Previous: [SRFI-41 Stream Fundamentals](#SRFI_002d41-Stream-Fundamentals), Up: [SRFI-41 - Streams](#SRFI_002d41)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.28.2 SRFI-41 Stream Primitives [¶](#SRFI_002d41-Stream-Primitives-1)

This library provides eight operators: constructors for
`stream-null` and `stream-pair`s, type predicates for streams
and the two kinds of streams, accessors for both fields of a
`stream-pair`, and a lambda that creates procedures that return
streams.

Scheme Variable: **stream-null** [¶](#index-stream_002dnull)
:   A promise that, when forced, is a single object, distinguishable from
    all other objects, that represents the null stream. `stream-null`
    is immutable and unique.

Scheme Syntax: **stream-cons** object-expr stream-expr [¶](#index-stream_002dcons)
:   Creates a newly-allocated stream containing a promise that, when forced,
    is a `stream-pair` with object-expr in its `stream-car`
    and stream-expr in its `stream-cdr`. Neither
    object-expr nor stream-expr is evaluated when
    `stream-cons` is called.

    Once created, a `stream-pair` is immutable; there is no
    `stream-set-car!` or `stream-set-cdr!` that modifies an
    existing stream-pair. There is no dotted-pair or improper stream as
    with lists.

Scheme Procedure: **stream?** object [¶](#index-stream_003f)
:   Returns true if object is a stream, otherwise returns false. If
    object is a stream, its promise will not be forced. If
    `(stream? obj)` returns true, then one of `(stream-null? obj)`
    or `(stream-pair? obj)` will return true and the other will return
    false.

Scheme Procedure: **stream-null?** object [¶](#index-stream_002dnull_003f)
:   Returns true if object is the distinguished null stream, otherwise
    returns false. If object is a stream, its promise will be forced.

Scheme Procedure: **stream-pair?** object [¶](#index-stream_002dpair_003f)
:   Returns true if object is a `stream-pair` constructed by
    `stream-cons`, otherwise returns false. If object is a
    stream, its promise will be forced.

Scheme Procedure: **stream-car** stream [¶](#index-stream_002dcar)
:   Returns the object stored in the `stream-car` of stream. An
    error is signaled if the argument is not a `stream-pair`. This
    causes the object-expr passed to `stream-cons` to be
    evaluated if it had not yet been; the value is cached in case it is
    needed again.

Scheme Procedure: **stream-cdr** stream [¶](#index-stream_002dcdr)
:   Returns the stream stored in the `stream-cdr` of stream. An
    error is signaled if the argument is not a `stream-pair`.

Scheme Syntax: **stream-lambda** formals body … [¶](#index-stream_002dlambda)
:   Creates a procedure that returns a promise to evaluate the body of
    the procedure. The last body expression to be evaluated must
    yield a stream. As with normal `lambda`, formals may be a
    single variable name, in which case all the formal arguments are
    collected into a single list, or a list of variable names, which may be
    null if there are no arguments, proper if there are an exact number of
    arguments, or dotted if a fixed number of arguments is to be followed by
    zero or more arguments collected into a list. Body must contain
    at least one expression, and may contain internal definitions preceding
    any expressions to be evaluated.

```
(define strm123
  (stream-cons 1
    (stream-cons 2
      (stream-cons 3
        stream-null))))

(stream-car strm123) ⇒ 1
(stream-car (stream-cdr strm123) ⇒ 2

(stream-pair?
  (stream-cdr
    (stream-cons (/ 1 0) stream-null))) ⇒ #f

(stream? (list 1 2 3)) ⇒ #f

(define iter
  (stream-lambda (f x)
    (stream-cons x (iter f (f x)))))

(define nats (iter (lambda (x) (+ x 1)) 0))

(stream-car (stream-cdr nats)) ⇒ 1

(define stream-add
  (stream-lambda (s1 s2)
    (stream-cons
      (+ (stream-car s1) (stream-car s2))
      (stream-add (stream-cdr s1)
                  (stream-cdr s2)))))

(define evens (stream-add nats nats))

(stream-car evens) ⇒ 0
(stream-car (stream-cdr evens)) ⇒ 2
(stream-car (stream-cdr (stream-cdr evens))) ⇒ 4
```

---

Previous: [SRFI-41 Stream Primitives](#SRFI_002d41-Stream-Primitives), Up: [SRFI-41 - Streams](#SRFI_002d41)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.28.3 SRFI-41 Stream Library [¶](#SRFI_002d41-Stream-Library-1)

Scheme Syntax: **define-stream** (name args …) body … [¶](#index-define_002dstream)
:   Creates a procedure that returns a stream, and may appear anywhere a
    normal `define` may appear, including as an internal definition.
    It may contain internal definitions of its own. The defined procedure
    takes arguments in the same way as `stream-lambda`.
    `define-stream` is syntactic sugar on `stream-lambda`; see
    also `stream-let`, which is also a sugaring of
    `stream-lambda`.

    A simple version of `stream-map` that takes only a single input
    stream calls itself recursively:

    ```
    (define-stream (stream-map proc strm)
      (if (stream-null? strm)
          stream-null
          (stream-cons
            (proc (stream-car strm))
            (stream-map proc (stream-cdr strm))))))
    ```

Scheme Procedure: **list->stream** list [¶](#index-list_002d_003estream)
:   Returns a newly-allocated stream containing the elements from
    list.

Scheme Procedure: **port->stream** [port] [¶](#index-port_002d_003estream)
:   Returns a newly-allocated stream containing in its elements the
    characters on the port. If port is not given it defaults to the
    current input port. The returned stream has finite length and is
    terminated by `stream-null`.

    It looks like one use of `port->stream` would be this:

    ```
    (define s ;wrong!
      (with-input-from-file filename
        (lambda () (port->stream))))
    ```

    But that fails, because `with-input-from-file` is eager, and closes
    the input port prematurely, before the first character is read. To read
    a file into a stream, say:

    ```
    (define-stream (file->stream filename)
      (let ((p (open-input-file filename)))
        (stream-let loop ((c (read-char p)))
          (if (eof-object? c)
              (begin (close-input-port p)
                     stream-null)
              (stream-cons c
                (loop (read-char p)))))))
    ```

Scheme Syntax: **stream** object-expr … [¶](#index-stream)
:   Creates a newly-allocated stream containing in its elements the objects,
    in order. The object-exprs are evaluated when they are accessed,
    not when the stream is created. If no objects are given, as in
    (stream), the null stream is returned. See also `list->stream`.

    ```
    (define strm123 (stream 1 2 3))

    ; (/ 1 0) not evaluated when stream is created
    (define s (stream 1 (/ 1 0) -1))
    ```

Scheme Procedure: **stream->list** [n] stream [¶](#index-stream_002d_003elist)
:   Returns a newly-allocated list containing in its elements the first
    n items in stream. If stream has less than n
    items, all the items in the stream will be included in the returned
    list. If n is not given it defaults to infinity, which means that
    unless stream is finite `stream->list` will never return.

    ```
    (stream->list 10
      (stream-map (lambda (x) (* x x))
        (stream-from 0)))
      ⇒ (0 1 4 9 16 25 36 49 64 81)
    ```

Scheme Procedure: **stream-append** stream … [¶](#index-stream_002dappend)
:   Returns a newly-allocated stream containing in its elements those
    elements contained in its input streams, in order of input. If
    any of the input streams is infinite, no elements of any of the
    succeeding input streams will appear in the output stream. See also
    `stream-concat`.

Scheme Procedure: **stream-concat** stream [¶](#index-stream_002dconcat)
:   Takes a stream consisting of one or more streams and returns a
    newly-allocated stream containing all the elements of the input streams.
    If any of the streams in the input stream is infinite, any
    remaining streams in the input stream will never appear in the output
    stream. See also `stream-append`.

Scheme Procedure: **stream-constant** object … [¶](#index-stream_002dconstant)
:   Returns a newly-allocated stream containing in its elements the
    objects, repeating in succession forever.

    ```
    (stream-constant 1) ⇒ 1 1 1 ...
    (stream-constant #t #f) ⇒ #t #f #t #f #t #f ...
    ```

Scheme Procedure: **stream-drop** n stream [¶](#index-stream_002ddrop)
:   Returns the suffix of the input stream that starts at the next
    element after the first n elements. The output stream shares
    structure with the input stream; thus, promises forced in one
    instance of the stream are also forced in the other instance of the
    stream. If the input stream has less than n elements,
    `stream-drop` returns the null stream. See also
    `stream-take`.

Scheme Procedure: **stream-drop-while** pred stream [¶](#index-stream_002ddrop_002dwhile)
:   Returns the suffix of the input stream that starts at the first
    element x for which `(pred x)` returns false. The output
    stream shares structure with the input stream. See also
    `stream-take-while`.

Scheme Procedure: **stream-filter** pred stream [¶](#index-stream_002dfilter)
:   Returns a newly-allocated stream that contains only those elements
    x of the input stream which satisfy the predicate
    `pred`.

    ```
    (stream-filter odd? (stream-from 0))
       ⇒ 1 3 5 7 9 ...
    ```

Scheme Procedure: **stream-fold** proc base stream [¶](#index-stream_002dfold)
:   Applies a binary procedure proc to base and the first
    element of stream to compute a new base, then applies the
    procedure to the new base and the next element of stream to
    compute a succeeding base, and so on, accumulating a value that is
    finally returned as the value of `stream-fold` when the end of the
    stream is reached. stream must be finite, or `stream-fold`
    will enter an infinite loop. See also `stream-scan`, which is
    similar to `stream-fold`, but useful for infinite streams. For
    readers familiar with other functional languages, this is a left-fold;
    there is no corresponding right-fold, since right-fold relies on finite
    streams that are fully-evaluated, in which case they may as well be
    converted to a list.

Scheme Procedure: **stream-for-each** proc stream … [¶](#index-stream_002dfor_002deach)
:   Applies proc element-wise to corresponding elements of the input
    streams for side-effects; it returns nothing.
    `stream-for-each` stops as soon as any of its input streams is
    exhausted.

Scheme Procedure: **stream-from** first [step] [¶](#index-stream_002dfrom)
:   Creates a newly-allocated stream that contains first as its first
    element and increments each succeeding element by step. If
    step is not given it defaults to 1. first and step
    may be of any numeric type. `stream-from` is frequently useful as
    a generator in `stream-of` expressions. See also
    `stream-range` for a similar procedure that creates finite streams.

Scheme Procedure: **stream-iterate** proc base [¶](#index-stream_002diterate)
:   Creates a newly-allocated stream containing base in its first
    element and applies proc to each element in turn to determine the
    succeeding element. See also `stream-unfold` and
    `stream-unfolds`.

Scheme Procedure: **stream-length** stream [¶](#index-stream_002dlength)
:   Returns the number of elements in the stream; it does not evaluate
    its elements. `stream-length` may only be used on finite streams;
    it enters an infinite loop with infinite streams.

Scheme Syntax: **stream-let** tag ((var expr) …) body … [¶](#index-stream_002dlet)
:   Creates a local scope that binds each variable to the value of its
    corresponding expression. It additionally binds tag to a
    procedure which takes the bound variables as arguments and body as
    its defining expressions, binding the tag with
    `stream-lambda`. tag is in scope within body, and may be
    called recursively. When the expanded expression defined by the
    `stream-let` is evaluated, `stream-let` evaluates the
    expressions in its body in an environment containing the
    newly-bound variables, returning the value of the last expression
    evaluated, which must yield a stream.

    `stream-let` provides syntactic sugar on `stream-lambda`, in
    the same manner as normal `let` provides syntactic sugar on normal
    `lambda`. However, unlike normal `let`, the tag is
    required, not optional, because unnamed `stream-let` is
    meaningless.

    For example, `stream-member` returns the first `stream-pair`
    of the input strm with a `stream-car` x that satisfies
    `(eql? obj x)`, or the null stream if x is not present in
    strm.

    ```
    (define-stream (stream-member eql? obj strm)
      (stream-let loop ((strm strm))
        (cond ((stream-null? strm) strm)
              ((eql? obj (stream-car strm)) strm)
              (else (loop (stream-cdr strm))))))
    ```

Scheme Procedure: **stream-map** proc stream … [¶](#index-stream_002dmap)
:   Applies proc element-wise to corresponding elements of the input
    streams, returning a newly-allocated stream containing elements
    that are the results of those procedure applications. The output stream
    has as many elements as the minimum-length input stream, and may be
    infinite.

Scheme Syntax: **stream-match** stream clause … [¶](#index-stream_002dmatch)
:   Provides pattern-matching for streams. The input stream is an
    expression that evaluates to a stream. Clauses are of the form
    `(pattern [fender] expression)`, consisting of a pattern that
    matches a stream of a particular shape, an optional fender that
    must succeed if the pattern is to match, and an expression that is
    evaluated if the pattern matches. There are four types of patterns:

    * () matches the null stream.
    * (pat0 pat1 …) matches a finite stream with length
      exactly equal to the number of pattern elements.
    * (pat0 pat1 … `.` pat-rest) matches an
      infinite stream, or a finite stream with length at least as great as the
      number of pattern elements before the literal dot.
    * pat matches an entire stream. Should always appear last in the
      list of clauses; it’s not an error to appear elsewhere, but subsequent
      clauses could never match.

    Each pattern element may be either:

    * An identifier, which matches any stream element. Additionally, the
      value of the stream element is bound to the variable named by the
      identifier, which is in scope in the fender and expression
      of the corresponding clause. Each identifier in a single pattern
      must be unique.
    * A literal underscore (`_`), which matches any stream element but
      creates no bindings.

    The patterns are tested in order, left-to-right, until a matching
    pattern is found; if fender is present, it must evaluate to a true
    value for the match to be successful. Pattern variables are bound in
    the corresponding fender and expression. Once the matching
    pattern is found, the corresponding expression is evaluated
    and returned as the result of the match. An error is signaled if no
    pattern matches the input stream.

    `stream-match` is often used to distinguish null streams from
    non-null streams, binding head and tail:

    ```
    (define (len strm)
      (stream-match strm
        (() 0)
        ((head . tail) (+ 1 (len tail)))))
    ```

    Fenders can test the common case where two stream elements must be
    identical; the `else` pattern is an identifier bound to the entire
    stream, not a keyword as in `cond`.

    ```
    (stream-match strm
      ((x y . _) (equal? x y) 'ok)
      (else 'error))
    ```

    A more complex example uses two nested matchers to match two different
    stream arguments; `(stream-merge lt? . strms)` stably merges two or
    more streams ordered by the `lt?` predicate:

    ```
    (define-stream (stream-merge lt? . strms)
      (define-stream (merge xx yy)
        (stream-match xx (() yy) ((x . xs)
          (stream-match yy (() xx) ((y . ys)
            (if (lt? y x)
                (stream-cons y (merge xx ys))
                (stream-cons x (merge xs yy))))))))
      (stream-let loop ((strms strms))
        (cond ((null? strms) stream-null)
              ((null? (cdr strms)) (car strms))
              (else (merge (car strms)
                           (apply stream-merge lt?
                             (cdr strms)))))))
    ```

Scheme Syntax: **stream-of** expr clause … [¶](#index-stream_002dof)
:   Provides the syntax of stream comprehensions, which generate streams by
    means of looping expressions. The result is a stream of objects of the
    type returned by expr. There are four types of clauses:

    * (var `in` stream-expr) loops over the elements of
      stream-expr, in order from the start of the stream, binding each
      element of the stream in turn to var. `stream-from` and
      `stream-range` are frequently useful as generators for
      stream-expr.
    * (var `is` expr) binds var to the value obtained
      by evaluating expr.
    * (pred expr) includes in the output stream only those
      elements x which satisfy the predicate pred.

    The scope of variables bound in the stream comprehension is the clauses
    to the right of the binding clause (but not the binding clause itself)
    plus the result expression.

    When two or more generators are present, the loops are processed as if
    they are nested from left to right; that is, the rightmost generator
    varies fastest. A consequence of this is that only the first generator
    may be infinite and all subsequent generators must be finite. If no
    generators are present, the result of a stream comprehension is a stream
    containing the result expression; thus, ‘`(stream-of 1)`’ produces a
    finite stream containing only the element 1.

    ```
    (stream-of (* x x)
      (x in (stream-range 0 10))
      (even? x))
      ⇒ 0 4 16 36 64

    (stream-of (list a b)
      (a in (stream-range 1 4))
      (b in (stream-range 1 3)))
      ⇒ (1 1) (1 2) (2 1) (2 2) (3 1) (3 2)

    (stream-of (list i j)
      (i in (stream-range 1 5))
      (j in (stream-range (+ i 1) 5)))
      ⇒ (1 2) (1 3) (1 4) (2 3) (2 4) (3 4)
    ```

Scheme Procedure: **stream-range** first past [step] [¶](#index-stream_002drange)
:   Creates a newly-allocated stream that contains first as its first
    element and increments each succeeding element by step. The
    stream is finite and ends before past, which is not an element of
    the stream. If step is not given it defaults to 1 if first
    is less than past and -1 otherwise. first, past and
    step may be of any real numeric type. `stream-range` is
    frequently useful as a generator in `stream-of` expressions. See
    also `stream-from` for a similar procedure that creates infinite
    streams.

    ```
    (stream-range 0 10) ⇒ 0 1 2 3 4 5 6 7 8 9
    (stream-range 0 10 2) ⇒ 0 2 4 6 8
    ```

    Successive elements of the stream are calculated by adding step to
    first, so if any of first, past or step are
    inexact, the length of the output stream may differ from
    `(ceiling (- (/ (- past first) step) 1)`.

Scheme Procedure: **stream-ref** stream n [¶](#index-stream_002dref)
:   Returns the nth element of stream, counting from zero. An error
    is signaled if n is greater than or equal to the length of stream.

    ```
    (define (fact n)
      (stream-ref
        (stream-scan * 1 (stream-from 1))
        n))
    ```

Scheme Procedure: **stream-reverse** stream [¶](#index-stream_002dreverse)
:   Returns a newly-allocated stream containing the elements of the input
    stream but in reverse order. `stream-reverse` may only be
    used with finite streams; it enters an infinite loop with infinite
    streams. `stream-reverse` does not force evaluation of the
    elements of the stream.

Scheme Procedure: **stream-scan** proc base stream [¶](#index-stream_002dscan)
:   Accumulates the partial folds of an input stream into a
    newly-allocated output stream. The output stream is the base
    followed by `(stream-fold proc base (stream-take i stream))` for
    each of the first i elements of stream.

    ```
    (stream-scan + 0 (stream-from 1))
      ⇒ (stream 0 1 3 6 10 15 ...)

    (stream-scan * 1 (stream-from 1))
      ⇒ (stream 1 1 2 6 24 120 ...)
    ```

Scheme Procedure: **stream-take** n stream [¶](#index-stream_002dtake)
:   Returns a newly-allocated stream containing the first n elements
    of the input stream. If the input stream has less than
    n elements, so does the output stream. See also
    `stream-drop`.

Scheme Procedure: **stream-take-while** pred stream [¶](#index-stream_002dtake_002dwhile)
:   Takes a predicate and a `stream` and returns a newly-allocated
    stream containing those elements `x` that form the maximal prefix
    of the input stream which satisfy pred. See also
    `stream-drop-while`.

Scheme Procedure: **stream-unfold** map pred gen base [¶](#index-stream_002dunfold)
:   The fundamental recursive stream constructor. It constructs a stream by
    repeatedly applying gen to successive values of base, in the
    manner of `stream-iterate`, then applying map to each of the
    values so generated, appending each of the mapped values to the output
    stream as long as `(pred? base)` returns a true value. See also
    `stream-iterate` and `stream-unfolds`.

    The expression below creates the finite stream ‘`0 1 4 9 16 25 36 49
    64 81`’. Initially the base is 0, which is less than 10, so
    map squares the base and the mapped value becomes the first
    element of the output stream. Then gen increments the base
    by 1, so it becomes 1; this is less than 10, so map squares the
    new base and 1 becomes the second element of the output stream.
    And so on, until the base becomes 10, when pred stops the
    recursion and stream-null ends the output stream.

    ```
    (stream-unfold
      (lambda (x) (expt x 2)) ; map
      (lambda (x) (< x 10))   ; pred?
      (lambda (x) (+ x 1))    ; gen
      0)                      ; base
    ```

Scheme Procedure: **stream-unfolds** proc seed [¶](#index-stream_002dunfolds)
:   Returns n newly-allocated streams containing those elements
    produced by successive calls to the generator proc, which takes
    the current seed as its argument and returns n+1 values

    (proc seed) ⇒ seed result\_0 … result\_n-1

    where the returned seed is the input seed to the next call
    to the generator and result\_i indicates how to produce the next
    element of the ith result stream:

    * (value): value is the next car of the result stream.
    * `#f`: no value produced by this iteration of the generator
      proc for the result stream.
    * (): the end of the result stream.

    It may require multiple calls of proc to produce the next element
    of any particular result stream. See also `stream-iterate` and
    `stream-unfold`.

    ```
    (define (stream-partition pred? strm)
      (stream-unfolds
        (lambda (s)
          (if (stream-null? s)
              (values s '() '())
              (let ((a (stream-car s))
                    (d (stream-cdr s)))
                (if (pred? a)
                    (values d (list a) #f)
                    (values d #f (list a))))))
        strm))

    (call-with-values
      (lambda ()
        (stream-partition odd?
          (stream-range 1 6)))
      (lambda (odds evens)
        (list (stream->list odds)
              (stream->list evens))))
      ⇒ ((1 3 5) (2 4))
    ```

Scheme Procedure: **stream-zip** stream … [¶](#index-stream_002dzip)
:   Returns a newly-allocated stream in which each element is a list (not a
    stream) of the corresponding elements of the input streams. The
    output stream is as long as the shortest input stream, if any of
    the input streams is finite, or is infinite if all the input
    streams are infinite.

---

Next: [SRFI-43 - Vector Library](#SRFI_002d43), Previous: [SRFI-41 - Streams](#SRFI_002d41), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.29 SRFI-42 - Eager Comprehensions [¶](#SRFI_002d42-_002d-Eager-Comprehensions)

See [the
specification of SRFI-42](http://srfi.schemers.org/srfi-42/srfi-42.html).

---

Next: [SRFI-45 - Primitives for Expressing Iterative Lazy Algorithms](#SRFI_002d45), Previous: [SRFI-42 - Eager Comprehensions](#SRFI_002d42), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.30 SRFI-43 - Vector Library [¶](#SRFI_002d43-_002d-Vector-Library)

This subsection is based on the
[specification of
SRFI-43](http://srfi.schemers.org/srfi-43/srfi-43.html) by Taylor Campbell.

SRFI-43 implements a comprehensive library of vector operations. It can
be made available with:

```
(use-modules (srfi srfi-43))
```

* [SRFI-43 Constructors](#SRFI_002d43-Constructors)
* [SRFI-43 Predicates](#SRFI_002d43-Predicates)
* [SRFI-43 Selectors](#SRFI_002d43-Selectors)
* [SRFI-43 Iteration](#SRFI_002d43-Iteration)
* [SRFI-43 Searching](#SRFI_002d43-Searching)
* [SRFI-43 Mutators](#SRFI_002d43-Mutators)
* [SRFI-43 Conversion](#SRFI_002d43-Conversion)

---

Next: [SRFI-43 Predicates](#SRFI_002d43-Predicates), Up: [SRFI-43 - Vector Library](#SRFI_002d43)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.30.1 SRFI-43 Constructors [¶](#SRFI_002d43-Constructors-1)

Scheme Procedure: **make-vector** size [fill] [¶](#index-make_002dvector-1)
:   Create and return a vector of size size, optionally filling it
    with fill. The default value of fill is unspecified.

    ```
    (make-vector 5 3) ⇒ #(3 3 3 3 3)
    ```

Scheme Procedure: **vector** x … [¶](#index-vector-1)
:   Create and return a vector whose elements are x ....

    ```
    (vector 0 1 2 3 4) ⇒ #(0 1 2 3 4)
    ```

Scheme Procedure: **vector-unfold** f length initial-seed … [¶](#index-vector_002dunfold)
:   The fundamental vector constructor. Create a vector whose length
    is length and iterates across each index k from 0 up to
    length - 1, applying f at each iteration to the current
    index and current seeds, in that order, to receive n + 1 values: the
    element to put in the kth slot of the new vector, and n new seeds for
    the next iteration. It is an error for the number of seeds to vary
    between iterations.

    ```
    (vector-unfold (lambda (i x) (values x (- x 1)))
                   10 0)
    ⇒ #(0 -1 -2 -3 -4 -5 -6 -7 -8 -9)

    (vector-unfold values 10)
    ⇒ #(0 1 2 3 4 5 6 7 8 9)
    ```

Scheme Procedure: **vector-unfold-right** f length initial-seed … [¶](#index-vector_002dunfold_002dright)
:   Like `vector-unfold`, but it uses f to generate elements from
    right-to-left, rather than left-to-right.

    ```
    (vector-unfold-right (lambda (i x) (values x (+ x 1)))
                         10 0)
    ⇒ #(9 8 7 6 5 4 3 2 1 0)
    ```

Scheme Procedure: **vector-copy** vec [start [end [fill]]] [¶](#index-vector_002dcopy-1)
:   Allocate a new vector whose length is end - start and fills
    it with elements from vec, taking elements from vec starting
    at index start and stopping at index end. start
    defaults to 0 and end defaults to the value of
    `(vector-length vec)`. If end extends beyond the length of
    vec, the slots in the new vector that obviously cannot be filled
    by elements from vec are filled with fill, whose default
    value is unspecified.

    ```
    (vector-copy '#(a b c d e f g h i))
    ⇒ #(a b c d e f g h i)

    (vector-copy '#(a b c d e f g h i) 6)
    ⇒ #(g h i)

    (vector-copy '#(a b c d e f g h i) 3 6)
    ⇒ #(d e f)

    (vector-copy '#(a b c d e f g h i) 6 12 'x)
    ⇒ #(g h i x x x)
    ```

Scheme Procedure: **vector-reverse-copy** vec [start [end]] [¶](#index-vector_002dreverse_002dcopy)
:   Like `vector-copy`, but it copies the elements in the reverse order
    from vec.

    ```
    (vector-reverse-copy '#(5 4 3 2 1 0) 1 5)
    ⇒ #(1 2 3 4)
    ```

Scheme Procedure: **vector-append** vec … [¶](#index-vector_002dappend)
:   Return a newly allocated vector that contains all elements in order from
    the subsequent locations in vec ....

    ```
    (vector-append '#(a) '#(b c d))
    ⇒ #(a b c d)
    ```

Scheme Procedure: **vector-concatenate** list-of-vectors [¶](#index-vector_002dconcatenate)
:   Append each vector in list-of-vectors. Equivalent to
    `(apply vector-append list-of-vectors)`.

    ```
    (vector-concatenate '(#(a b) #(c d)))
    ⇒ #(a b c d)
    ```

---

Next: [SRFI-43 Selectors](#SRFI_002d43-Selectors), Previous: [SRFI-43 Constructors](#SRFI_002d43-Constructors), Up: [SRFI-43 - Vector Library](#SRFI_002d43)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.30.2 SRFI-43 Predicates [¶](#SRFI_002d43-Predicates-1)

Scheme Procedure: **vector?** obj [¶](#index-vector_003f-1)
:   Return true if obj is a vector, else return false.

Scheme Procedure: **vector-empty?** vec [¶](#index-vector_002dempty_003f)
:   Return true if vec is empty, i.e. its length is 0, else return
    false.

Scheme Procedure: **vector=** elt=? vec … [¶](#index-vector_003d)
:   Return true if the vectors vec … have equal lengths and
    equal elements according to elt=?. elt=? is always applied
    to two arguments. Element comparison must be consistent with `eq?`
    in the following sense: if `(eq? a b)` returns true, then
    `(elt=? a b)` must also return true. The order in which
    comparisons are performed is unspecified.

---

Next: [SRFI-43 Iteration](#SRFI_002d43-Iteration), Previous: [SRFI-43 Predicates](#SRFI_002d43-Predicates), Up: [SRFI-43 - Vector Library](#SRFI_002d43)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.30.3 SRFI-43 Selectors [¶](#SRFI_002d43-Selectors-1)

Scheme Procedure: **vector-ref** vec i [¶](#index-vector_002dref-1)
:   Return the element at index i in vec. Indexing is based on
    zero.

Scheme Procedure: **vector-length** vec [¶](#index-vector_002dlength-1)
:   Return the length of vec.

---

Next: [SRFI-43 Searching](#SRFI_002d43-Searching), Previous: [SRFI-43 Selectors](#SRFI_002d43-Selectors), Up: [SRFI-43 - Vector Library](#SRFI_002d43)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.30.4 SRFI-43 Iteration [¶](#SRFI_002d43-Iteration-1)

Scheme Procedure: **vector-fold** kons knil vec1 vec2 … [¶](#index-vector_002dfold)
:   The fundamental vector iterator. kons is iterated over each index
    in all of the vectors, stopping at the end of the shortest; kons
    is applied as

    ```
    (kons i state (vector-ref vec1 i) (vector-ref vec2 i) ...)
    ```

    where state is the current state value, and i is the current
    index. The current state value begins with knil, and becomes
    whatever kons returned at the respective iteration. The iteration
    is strictly left-to-right.

Scheme Procedure: **vector-fold-right** kons knil vec1 vec2 … [¶](#index-vector_002dfold_002dright)
:   Similar to `vector-fold`, but it iterates right-to-left instead of
    left-to-right.

Scheme Procedure: **vector-map** f vec1 vec2 … [¶](#index-vector_002dmap)
:   Return a new vector of the shortest size of the vector arguments. Each
    element at index i of the new vector is mapped from the old vectors by

    ```
    (f i (vector-ref vec1 i) (vector-ref vec2 i) ...)
    ```

    The dynamic order of application of f is unspecified.

Scheme Procedure: **vector-map!** f vec1 vec2 … [¶](#index-vector_002dmap_0021)
:   Similar to `vector-map`, but rather than mapping the new elements
    into a new vector, the new mapped elements are destructively inserted
    into vec1. The dynamic order of application of f is
    unspecified.

Scheme Procedure: **vector-for-each** f vec1 vec2 … [¶](#index-vector_002dfor_002deach)
:   Call `(f i (vector-ref vec1 i) (vector-ref vec2 i) ...)` for each
    index i less than the length of the shortest vector passed. The
    iteration is strictly left-to-right.

Scheme Procedure: **vector-count** pred? vec1 vec2 … [¶](#index-vector_002dcount)
:   Count the number of parallel elements in the vectors that satisfy
    pred?, which is applied, for each index i less than the length of
    the smallest vector, to i and each parallel element in the vectors at
    that index, in order.

    ```
    (vector-count (lambda (i elt) (even? elt))
                  '#(3 1 4 1 5 9 2 5 6))
    ⇒ 3
    (vector-count (lambda (i x y) (< x y))
                  '#(1 3 6 9) '#(2 4 6 8 10 12))
    ⇒ 2
    ```

---

Next: [SRFI-43 Mutators](#SRFI_002d43-Mutators), Previous: [SRFI-43 Iteration](#SRFI_002d43-Iteration), Up: [SRFI-43 - Vector Library](#SRFI_002d43)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.30.5 SRFI-43 Searching [¶](#SRFI_002d43-Searching-1)

Scheme Procedure: **vector-index** pred? vec1 vec2 … [¶](#index-vector_002dindex)
:   Find and return the index of the first elements in vec1 vec2
    … that satisfy pred?. If no matching element is found by
    the end of the shortest vector, return `#f`.

    ```
    (vector-index even? '#(3 1 4 1 5 9))
    ⇒ 2
    (vector-index < '#(3 1 4 1 5 9 2 5 6) '#(2 7 1 8 2))
    ⇒ 1
    (vector-index = '#(3 1 4 1 5 9 2 5 6) '#(2 7 1 8 2))
    ⇒ #f
    ```

Scheme Procedure: **vector-index-right** pred? vec1 vec2 … [¶](#index-vector_002dindex_002dright)
:   Like `vector-index`, but it searches right-to-left, rather than
    left-to-right. Note that the SRFI 43 specification requires that all
    the vectors must have the same length, but both the SRFI 43 reference
    implementation and Guile’s implementation allow vectors with unequal
    lengths, and start searching from the last index of the shortest vector.

Scheme Procedure: **vector-skip** pred? vec1 vec2 … [¶](#index-vector_002dskip)
:   Find and return the index of the first elements in vec1 vec2
    … that do not satisfy pred?. If no matching element is
    found by the end of the shortest vector, return `#f`. Equivalent
    to `vector-index` but with the predicate inverted.

    ```
    (vector-skip number? '#(1 2 a b 3 4 c d)) ⇒ 2
    ```

Scheme Procedure: **vector-skip-right** pred? vec1 vec2 … [¶](#index-vector_002dskip_002dright)
:   Like `vector-skip`, but it searches for a non-matching element
    right-to-left, rather than left-to-right. Note that the SRFI 43
    specification requires that all the vectors must have the same length,
    but both the SRFI 43 reference implementation and Guile’s implementation
    allow vectors with unequal lengths, and start searching from the last
    index of the shortest vector.

Scheme Procedure: **vector-binary-search** vec value cmp [start [end]] [¶](#index-vector_002dbinary_002dsearch)
:   Find and return an index of vec between start and end
    whose value is value using a binary search. If no matching
    element is found, return `#f`. The default start is 0 and
    the default end is the length of vec.

    cmp must be a procedure of two arguments such that `(cmp a
    b)` returns a negative integer if *a < b*, a positive integer if
    *a > b*, or zero if *a = b*. The elements of vec must
    be sorted in non-decreasing order according to cmp.

    Note that SRFI 43 does not document the start and end
    arguments, but both its reference implementation and Guile’s
    implementation support them.

    ```
    (define (char-cmp c1 c2)
      (cond ((char<? c1 c2) -1)
            ((char>? c1 c2) 1)
            (else 0)))

    (vector-binary-search '#(#\a #\b #\c #\d #\e #\f #\g #\h)
                          #\g
                          char-cmp)
    ⇒ 6
    ```

Scheme Procedure: **vector-any** pred? vec1 vec2 … [¶](#index-vector_002dany)
:   Find the first parallel set of elements from vec1 vec2
    … for which pred? returns a true value. If such a parallel
    set of elements exists, `vector-any` returns the value that
    pred? returned for that set of elements. The iteration is
    strictly left-to-right.

Scheme Procedure: **vector-every** pred? vec1 vec2 … [¶](#index-vector_002devery)
:   If, for every index i between 0 and the length of the shortest vector
    argument, the set of elements `(vector-ref vec1 i)`
    `(vector-ref vec2 i)` … satisfies pred?,
    `vector-every` returns the value that pred? returned for the
    last set of elements, at the last index of the shortest vector.
    Otherwise it returns `#f`. The iteration is strictly
    left-to-right.

---

Next: [SRFI-43 Conversion](#SRFI_002d43-Conversion), Previous: [SRFI-43 Searching](#SRFI_002d43-Searching), Up: [SRFI-43 - Vector Library](#SRFI_002d43)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.30.6 SRFI-43 Mutators [¶](#SRFI_002d43-Mutators-1)

Scheme Procedure: **vector-set!** vec i value [¶](#index-vector_002dset_0021-1)
:   Assign the contents of the location at i in vec to
    value.

Scheme Procedure: **vector-swap!** vec i j [¶](#index-vector_002dswap_0021)
:   Swap the values of the locations in vec at i and j.

Scheme Procedure: **vector-fill!** vec fill [start [end]] [¶](#index-vector_002dfill_0021-1)
:   Assign the value of every location in vec between start and
    end to fill. start defaults to 0 and end
    defaults to the length of vec.

Scheme Procedure: **vector-reverse!** vec [start [end]] [¶](#index-vector_002dreverse_0021)
:   Destructively reverse the contents of vec between start and
    end. start defaults to 0 and end defaults to the
    length of vec.

Scheme Procedure: **vector-copy!** target tstart source [sstart [send]] [¶](#index-vector_002dcopy_0021-1)
:   Copy a block of elements from source to target, both of
    which must be vectors, starting in target at tstart and
    starting in source at sstart, ending when (send -
    sstart) elements have been copied. It is an error for
    target to have a length less than (tstart + send -
    sstart). sstart defaults to 0 and send defaults to
    the length of source.

Scheme Procedure: **vector-reverse-copy!** target tstart source [sstart [send]] [¶](#index-vector_002dreverse_002dcopy_0021)
:   Like `vector-copy!`, but this copies the elements in the reverse
    order. It is an error if target and source are identical
    vectors and the target and source ranges overlap; however,
    if tstart = sstart, `vector-reverse-copy!` behaves as
    `(vector-reverse! target tstart send)` would.

---

Previous: [SRFI-43 Mutators](#SRFI_002d43-Mutators), Up: [SRFI-43 - Vector Library](#SRFI_002d43)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.30.7 SRFI-43 Conversion [¶](#SRFI_002d43-Conversion-1)

Scheme Procedure: **vector->list** vec [start [end]] [¶](#index-vector_002d_003elist-1)
:   Return a newly allocated list containing the elements in vec
    between start and end. start defaults to 0 and
    end defaults to the length of vec.

Scheme Procedure: **reverse-vector->list** vec [start [end]] [¶](#index-reverse_002dvector_002d_003elist)
:   Like `vector->list`, but the resulting list contains the specified
    range of elements of vec in reverse order.

Scheme Procedure: **list->vector** proper-list [start [end]] [¶](#index-list_002d_003evector-1)
:   Return a newly allocated vector of the elements from proper-list
    with indices between start and end. start defaults to
    0 and end defaults to the length of proper-list. Note that
    SRFI 43 does not document the start and end arguments, but
    both its reference implementation and Guile’s implementation support
    them.

Scheme Procedure: **reverse-list->vector** proper-list [start [end]] [¶](#index-reverse_002dlist_002d_003evector)
:   Like `list->vector`, but the resulting vector contains the specified
    range of elements of proper-list in reverse order. Note that SRFI
    43 does not document the start and end arguments, but both
    its reference implementation and Guile’s implementation support them.

---

Next: [SRFI-46 Basic syntax-rules Extensions](#SRFI_002d46), Previous: [SRFI-43 - Vector Library](#SRFI_002d43), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.31 SRFI-45 - Primitives for Expressing Iterative Lazy Algorithms [¶](#SRFI_002d45-_002d-Primitives-for-Expressing-Iterative-Lazy-Algorithms)

This subsection is based on [the
specification of SRFI-45](http://srfi.schemers.org/srfi-45/srfi-45.html) written by André van Tonder.

Lazy evaluation is traditionally simulated in Scheme using `delay`
and `force`. However, these primitives are not powerful enough to
express a large class of lazy algorithms that are iterative. Indeed, it
is folklore in the Scheme community that typical iterative lazy
algorithms written using delay and force will often require unbounded
memory.

This SRFI provides set of three operations: {`lazy`, `delay`,
`force`}, which allow the programmer to succinctly express lazy
algorithms while retaining bounded space behavior in cases that are
properly tail-recursive. A general recipe for using these primitives is
provided. An additional procedure `eager` is provided for the
construction of eager promises in cases where efficiency is a concern.

Although this SRFI redefines `delay` and `force`, the
extension is conservative in the sense that the semantics of the subset
{`delay`, `force`} in isolation (i.e., as long as the
program does not use `lazy`) agrees with that in R5RS. In other
words, no program that uses the R5RS definitions of delay and force will
break if those definition are replaced by the SRFI-45 definitions of
delay and force.

Guile also adds `promise?` to the list of exports, which is not
part of the official SRFI-45.

Scheme Procedure: **promise?** obj [¶](#index-promise_003f-1)
:   Return true if obj is an SRFI-45 promise, otherwise return false.

Scheme Syntax: **delay** expression [¶](#index-delay-1)
:   Takes an expression of arbitrary type a and returns a promise of
    type `(Promise a)` which at some point in the future may be
    asked (by the `force` procedure) to evaluate the expression and
    deliver the resulting value.

Scheme Syntax: **lazy** expression [¶](#index-lazy)
:   Takes an expression of type `(Promise a)` and returns a
    promise of type `(Promise a)` which at some point in the
    future may be asked (by the `force` procedure) to evaluate the
    expression and deliver the resulting promise.

Scheme Procedure: **force** expression [¶](#index-force-1)
:   Takes an argument of type `(Promise a)` and returns a value
    of type a as follows: If a value of type a has been computed
    for the promise, this value is returned. Otherwise, the promise is
    first evaluated, then overwritten by the obtained promise or value, and
    then force is again applied (iteratively) to the promise.

Scheme Procedure: **eager** expression [¶](#index-eager)
:   Takes an argument of type a and returns a value of type
    `(Promise a)`. As opposed to `delay`, the argument is
    evaluated eagerly. Semantically, writing `(eager expression)` is
    equivalent to writing

    ```
    (let ((value expression)) (delay value)).
    ```

    However, the former is more efficient since it does not require
    unnecessary creation and evaluation of thunks. We also have the
    equivalence

    ```
    (delay expression) = (lazy (eager expression))
    ```

The following reduction rules may be helpful for reasoning about these
primitives. However, they do not express the memoization and memory
usage semantics specified above:

```
(force (delay expression)) -> expression
(force (lazy  expression)) -> (force expression)
(force (eager value))      -> value
```

#### Correct usage [¶](#Correct-usage)

We now provide a general recipe for using the primitives {`lazy`,
`delay`, `force`} to express lazy algorithms in Scheme. The
transformation is best described by way of an example: Consider the
stream-filter algorithm, expressed in a hypothetical lazy language as

```
(define (stream-filter p? s)
  (if (null? s) '()
      (let ((h (car s))
            (t (cdr s)))
        (if (p? h)
            (cons h (stream-filter p? t))
            (stream-filter p? t)))))
```

This algorithm can be expressed as follows in Scheme:

```
(define (stream-filter p? s)
  (lazy
     (if (null? (force s)) (delay '())
         (let ((h (car (force s)))
               (t (cdr (force s))))
           (if (p? h)
               (delay (cons h (stream-filter p? t)))
               (stream-filter p? t))))))
```

In other words, we

* wrap all constructors (e.g., `'()`, `cons`) with `delay`,
* apply `force` to arguments of deconstructors (e.g., `car`,
  `cdr` and `null?`),
* wrap procedure bodies with `(lazy ...)`.

---

Next: [SRFI-55 - Requiring Features](#SRFI_002d55), Previous: [SRFI-45 - Primitives for Expressing Iterative Lazy Algorithms](#SRFI_002d45), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.32 SRFI-46 Basic syntax-rules Extensions [¶](#SRFI_002d46-Basic-syntax_002drules-Extensions)

Guile’s core `syntax-rules` supports the extensions specified by
SRFI-46/R7RS. Tail patterns have been supported since at least Guile
2.0, and custom ellipsis identifiers have been supported since Guile
2.0.10. See [Syntax-rules Macros](#Syntax-Rules).

---

Next: [SRFI-60 - Integers as Bits](#SRFI_002d60), Previous: [SRFI-46 Basic syntax-rules Extensions](#SRFI_002d46), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.33 SRFI-55 - Requiring Features [¶](#SRFI_002d55-_002d-Requiring-Features)

SRFI-55 provides `require-extension` which is a portable
mechanism to load selected SRFI modules. This is implemented in the
Guile core, there’s no module needed to get SRFI-55 itself.

library syntax: **require-extension** clause1 clause2 … [¶](#index-require_002dextension)
:   Require the features of clause1 clause2 … , throwing
    an error if any are unavailable.

    A clause is of the form `(identifier arg...)`. The
    only identifier currently supported is `srfi` and the
    arguments are SRFI numbers. For example to get SRFI-1 and SRFI-6,

    ```
    (require-extension (srfi 1 6))
    ```

    `require-extension` can only be used at the top-level.

    A Guile-specific program can simply `use-modules` to load SRFIs
    not already in the core, `require-extension` is for programs
    designed to be portable to other Scheme implementations.

---

Next: [SRFI-61 - A more general `cond` clause](#SRFI_002d61), Previous: [SRFI-55 - Requiring Features](#SRFI_002d55), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.34 SRFI-60 - Integers as Bits [¶](#SRFI_002d60-_002d-Integers-as-Bits)

This SRFI provides various functions for treating integers as bits and
for bitwise manipulations. These functions can be obtained with,

```
(use-modules (srfi srfi-60))
```

Integers are treated as infinite precision twos-complement, the same
as in the core logical functions (see [Bitwise Operations](#Bitwise-Operations)). And
likewise bit indexes start from 0 for the least significant bit. The
following functions in this SRFI are already in the Guile core,

> `logand`,
> `logior`,
> `logxor`,
> `lognot`,
> `logtest`,
> `logcount`,
> `integer-length`,
> `logbit?`,
> `ash`

  

Function: **bitwise-and** n1 ... [¶](#index-bitwise_002dand)

Function: **bitwise-ior** n1 ... [¶](#index-bitwise_002dior)

Function: **bitwise-xor** n1 ... [¶](#index-bitwise_002dxor)

Function: **bitwise-not** n [¶](#index-bitwise_002dnot)

Function: **any-bits-set?** j k [¶](#index-any_002dbits_002dset_003f)

Function: **bit-set?** index n [¶](#index-bit_002dset_003f)

Function: **arithmetic-shift** n count [¶](#index-arithmetic_002dshift)

Function: **bit-field** n start end [¶](#index-bit_002dfield)

Function: **bit-count** n [¶](#index-bit_002dcount)
:   Aliases for `logand`, `logior`, `logxor`,
    `lognot`, `logtest`, `logbit?`, `ash`,
    `bit-extract` and `logcount` respectively.

    Note that the name `bit-count` conflicts with `bit-count` in
    the core (see [Bit Vectors](#Bit-Vectors)).

Function: **bitwise-if** mask n1 n0 [¶](#index-bitwise_002dif)

Function: **bitwise-merge** mask n1 n0 [¶](#index-bitwise_002dmerge)
:   Return an integer with bits selected from n1 and n0
    according to mask. Those bits where mask has 1s are taken
    from n1, and those where mask has 0s are taken from
    n0.

    ```
    (bitwise-if 3 #b0101 #b1010) ⇒ 9
    ```

Function: **log2-binary-factors** n [¶](#index-log2_002dbinary_002dfactors)

Function: **first-set-bit** n [¶](#index-first_002dset_002dbit)
:   Return a count of how many factors of 2 are present in n. This
    is also the bit index of the lowest 1 bit in n. If n is
    0, the return is *-1*.

    ```
    (log2-binary-factors 6) ⇒ 1
    (log2-binary-factors -8) ⇒ 3
    ```

Function: **copy-bit** index n newbit [¶](#index-copy_002dbit)
:   Return n with the bit at index set according to
    newbit. newbit should be `#t` to set the bit to 1,
    or `#f` to set it to 0. Bits other than at index are
    unchanged in the return.

    ```
    (copy-bit 1 #b0101 #t) ⇒ 7
    ```

Function: **copy-bit-field** n newbits start end [¶](#index-copy_002dbit_002dfield)
:   Return n with the bits from start (inclusive) to end
    (exclusive) changed to the value newbits.

    The least significant bit in newbits goes to start, the
    next to *start+1*, etc. Anything in newbits past the
    end given is ignored.

    ```
    (copy-bit-field #b10000 #b11 1 3) ⇒ #b10110
    ```

Function: **rotate-bit-field** n count start end [¶](#index-rotate_002dbit_002dfield)
:   Return n with the bit field from start (inclusive) to
    end (exclusive) rotated upwards by count bits.

    count can be positive or negative, and it can be more than the
    field width (it’ll be reduced modulo the width).

    ```
    (rotate-bit-field #b0110 2 1 4) ⇒ #b1010
    ```

Function: **reverse-bit-field** n start end [¶](#index-reverse_002dbit_002dfield)
:   Return n with the bits from start (inclusive) to end
    (exclusive) reversed.

    ```
    (reverse-bit-field #b101001 2 4) ⇒ #b100101
    ```

Function: **integer->list** n [len] [¶](#index-integer_002d_003elist)
:   Return bits from n in the form of a list of `#t` for 1 and
    `#f` for 0. The least significant len bits are returned,
    and the first list element is the most significant of those bits. If
    len is not given, the default is `(integer-length n)`
    (see [Bitwise Operations](#Bitwise-Operations)).

    ```
    (integer->list 6)   ⇒ (#t #t #f)
    (integer->list 1 4) ⇒ (#f #f #f #t)
    ```

Function: **list->integer** lst [¶](#index-list_002d_003einteger)

Function: **booleans->integer** bool… [¶](#index-booleans_002d_003einteger)
:   Return an integer formed bitwise from the given lst list of
    booleans, or for `booleans->integer` from the bool
    arguments.

    Each boolean is `#t` for a 1 and `#f` for a 0. The first
    element becomes the most significant bit in the return.

    ```
    (list->integer '(#t #f #t #f)) ⇒ 10
    ```

---

Next: [SRFI-62 - S-expression comments.](#SRFI_002d62), Previous: [SRFI-60 - Integers as Bits](#SRFI_002d60), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.35 SRFI-61 - A more general `cond` clause [¶](#SRFI_002d61-_002d-A-more-general-cond-clause)

This SRFI extends RnRS `cond` to support test expressions that
return multiple values, as well as arbitrary definitions of test
success. SRFI 61 is implemented in the Guile core; there’s no module
needed to get SRFI-61 itself. Extended `cond` is documented in
[Simple Conditional Evaluation](#Conditionals).

---

Next: [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64), Previous: [SRFI-61 - A more general `cond` clause](#SRFI_002d61), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.36 SRFI-62 - S-expression comments. [¶](#SRFI_002d62-_002d-S_002dexpression-comments_002e)

Starting from version 2.0, Guile’s `read` supports SRFI-62/R7RS
S-expression comments by default.

---

Next: [SRFI-67 - Compare procedures](#SRFI_002d67), Previous: [SRFI-62 - S-expression comments.](#SRFI_002d62), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.37 SRFI-64: A Scheme API for Test Suites [¶](#SRFI_002d64_003a-A-Scheme-API-for-Test-Suites)

* [SRFI-64 Abstract](#SRFI_002d64-Abstract)
* [SRFI-64 Rationale](#SRFI_002d64-Rationale)
* [SRFI-64 Writing Basic Test Suites](#SRFI_002d64-Writing-Basic-Test-Suites)
* [SRFI-64 Conditonal Test Suites and Other Advanced Features](#SRFI_002d64-Conditonal-Test-Suites-and-Other-Advanced-Features)
* [SRFI-64 Test Runner](#SRFI_002d64-Test-Runner)
* [SRFI-64 Test Results](#SRFI_002d64-Test-Results)
* [SRFI-64 Writing a New Test Runner](#SRFI_002d64-Writing-a-New-Test-Runner)

---

Next: [SRFI-64 Rationale](#SRFI_002d64-Rationale), Up: [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.37.1 SRFI-64 Abstract [¶](#SRFI_002d64-Abstract-1)

This defines an API for writing *test suites*, to make it easy to
portably test Scheme APIs, libraries, applications, and implementations.
A test suite is a collection of *test cases* that execute in the
context of a *test-runner*. This specification also supports
writing new test-runners, to allow customization of reporting and
processing the result of running test suites.

---

Next: [SRFI-64 Writing Basic Test Suites](#SRFI_002d64-Writing-Basic-Test-Suites), Previous: [SRFI-64 Abstract](#SRFI_002d64-Abstract), Up: [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.37.2 SRFI-64 Rationale [¶](#SRFI_002d64-Rationale-1)

The Scheme community needs a standard for writing test suites. Every
SRFI or other library should come with a test suite. Such a test suite
must be portable, without requiring any non-standard features, such as
modules. The test suite implementation or "runner" need not be
portable, but it is desirable that it be possible to write a portable
basic implementation.

There are other testing frameworks written in Scheme, including
[RackUnit](https://docs.racket-lang.org/rackunit/). However
RackUnit is not portable. It is also a bit on the verbose side. It
would be useful to have a bridge between this framework and RackUnit so
RackUnit tests could run under this framework and vice versa. There
exists also at least one Scheme wrapper providing a Scheme interface to
the “standard” [JUnit](https://www.junit.org/) API for Java. It
would be useful to have a bridge so that tests written using this
framework can run under a JUnit runner. Neither of these features are
part of this specification.

This API makes use of implicit dynamic state, including an implicit
“test runner”. This makes the API convenient and terse to use, but it
may be a little less elegant and “compositional” than using explicit
test objects, such as JUnit-style frameworks. It is not claimed to
follow either object-oriented or functional design principles, but I
hope it is useful and convenient to use and extend.

This proposal allows converting a Scheme source file to a
test suite by just adding a few macros. You don’t have to
write the entire file in a new form, thus you don’t have to
re-indent it.

All names defined by the API start with the prefix ‘`test-`’. All
function-like forms are defined as syntax. They may be implemented as
functions or macros or built-ins. The reason for specifying them as
syntax is to allow specific tests to be skipped without evaluating
sub-expressions, or for implementations to add features such as printing
line numbers or catching exceptions.

---

Next: [SRFI-64 Conditonal Test Suites and Other Advanced Features](#SRFI_002d64-Conditonal-Test-Suites-and-Other-Advanced-Features), Previous: [SRFI-64 Rationale](#SRFI_002d64-Rationale), Up: [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.37.3 SRFI-64 Writing Basic Test Suites [¶](#SRFI_002d64-Writing-Basic-Test-Suites-1)

Let’s start with a simple example. This is a complete self-contained
test-suite.

```
;; Initialize and give a name to a simple testsuite.
(test-begin "vec-test")
(define v (make-vector 5 99))
;; Require that an expression evaluate to true.
(test-assert (vector? v))
;; Test that an expression is eqv? to some other expression.
(test-eqv 99 (vector-ref v 2))
(vector-set! v 2 7)
(test-eqv 7 (vector-ref v 2))
;; Finish the testsuite, and report results.
(test-end "vec-test")
```

This testsuite could be saved in its own source file. Nothing else is
needed: We do not require any top-level forms, so it is easy to wrap an
existing program or test to this form, without adding indentation. It
is also easy to add new tests, without having to name individual tests
(though that is optional).

Test cases are executed in the context of a *test runner*, which is
an object that accumulates and reports test results. This specification
defines how to create and use custom test runners, but implementations
should also provide a default test runner. It is suggested (but not
required) that loading the above file in a top-level environment will
cause the tests to be executed using an implementation-specified default
test runner, and `test-end` will cause a summary to be displayed in
an implementation-specified manner. The SRFI 64 implementation used in
Guile provides such a default test runner; running the above snippet at
the REPL prints:

```
*** Entering test group: vec-test ***
$1 = #t
* PASS:
$2 = ((pass . 1))
* PASS:
$3 = ((pass . 2))
* PASS:
$4 = ((pass . 3))
*** Leaving test group: vec-test ***
*** Test suite finished. ***
*** # of expected passes    : 3
```

It also returns the `<test-runner>` object.

#### Simple test-cases [¶](#Simple-test_002dcases)

Primitive test cases test that a given condition is true. They may have
a name. The core test case form is `test-assert`:

Scheme Syntax: **test-assert** [test-name] expression [¶](#index-test_002dassert)
:   This evaluates the expression. The test passes if the result is
    true; if the result is false, a test failure is reported. The test also
    fails if an exception is raised, assuming the implementation has a way
    to catch exceptions. How the failure is reported depends on the test
    runner environment. The test-name is a string that names the test
    case. (Though the test-name is a string literal in the examples,
    it is an expression. It is evaluated only once.) It is used when
    reporting errors, and also when skipping tests, as described below. It
    is an error to invoke `test-assert`if there is no current test
    runner.

The following forms may be more convenient than using `test-assert`
directly:

Scheme Syntax: **test-eqv** [test-name] expected test-expr [¶](#index-test_002deqv)
:   This is equivalent to:

    ```
    (test-assert [test-name] (eqv? expected test-expr))
    ```

Similarly `test-equal` and `test-eq` are shorthand for
`test-assert` combined with `equal?` or `eq?`,
respectively:

Scheme Syntax: **test-equal** [test-name] expected test-expr [¶](#index-test_002dequal)

Scheme Syntax: **test-eq** [test-name] expected test-expr [¶](#index-test_002deq)
:   Here is a simple example:

    ```
    (define (mean x y) (/ (+ x y) 2.0))
    (test-eqv 4 (mean 3 5))
    ```

For testing approximate equality of inexact reals
we can use `test-approximate`:

Scheme Syntax: **test-approximate** [test-name] expected test-expr error [¶](#index-test_002dapproximate)
:   This is equivalent to (except that each argument is only evaluated
    once):

    ```
    (test-assert [test-name]
      (and (>= test-expr (- expected error))
           (<= test-expr (+ expected error))))
    ```

    Here’s an example:

    ```
    (test-approximate "is 22/7 within 1% of π?"
     3.1415926535
     22/7
     1/100)
    ```

#### Tests for catching errors [¶](#Tests-for-catching-errors)

We need a way to specify that evaluation *should* fail. This
verifies that errors are detected when required.

Scheme Syntax: **test-error** [[test-name] error-type] test-expr [¶](#index-test_002derror)
:   Evaluating test-expr is expected to signal an error. The kind of
    error is indicated by error-type.

    If the error-type is left out, or it is `#t`, it means "some
    kind of unspecified error should be signaled". For example:

    ```
    (test-error #t (vector-ref '#(1 2) 9))
    ```

    This specification leaves it implementation-defined (or for a future
    specification) what form test-error may take, though all
    implementations must allow `#t`. Some implementations may support
    [SRFI-35’s
    conditions](https://srfi.schemers.org/srfi-35/srfi-35.html), but these are only standardized for
    [SRFI-36’s I/O
    conditions](https://srfi.schemers.org/srfi-36/srfi-36.html), which are seldom useful in test suites. An implementation
    may also allow implementation-specific “exception types”. For example
    Java-based implementations may allow the names of Java exception
    classes:

    ```
    ;; Kawa-specific example
    (test-error <java.lang.IndexOutOfBoundsException> (vector-ref '#(1 2) 9))
    ```

    An implementation that cannot catch exceptions should skip
    `test-error` forms.

    The SRFI-64 implementation in Guile supports specifying error-type
    as either:

    * `#f`, meaning the test is *not* expected to produce an error
    * `#t`, meaning the test is expected to produce an error, of any type
    * A native exception type, as created via `make-exception-type` or
      `make-condition-type` from SRFI-35
    * A predicate, which will be applied to the exception caught
      to determine whether is it of the right type
    * A symbol, for the exception kind of legacy
      `make-exception-from-throw` style exceptions.

Below are some examples valid in Guile:

```
(test-error "expect old-style exception kind"
 'numerical-overflow
 (/ 1 0))
```

```
(use-modules (ice-9 exceptions)) ;for standard exception types

(test-error "expect a native exception type"
 &warning
 (raise-exception (make-warning)))

(test-error "expect a native exception, using predicate"
 warning?
 (raise-exception (make-warning)))
```

```
(use-modules (srfi srfi-35))

(test-error "expect a serious SRFI 35 condition type"
 &serious
 (raise-exception (condition (&serious))))

(test-error "expect a serious SRFI 35 condition type, using predicate"
 serious-condition?
 (raise-exception (condition (&serious))))
```

#### Testing syntax [¶](#Testing-syntax)

Testing syntax is tricky, especially if we want to check that invalid
syntax is causing an error. The following utility function can help:

Scheme Procedure: **test-read-eval-string** string [¶](#index-test_002dread_002deval_002dstring)
:   This function parses string (using `read`) and evaluates the
    result. The result of evaluation is returned from
    `test-read-eval-string`. An error is signalled if there are unread
    characters after the `read` is done. For example:
    `(test-read-eval-string "(+ 3 4)")` *evaluates to* `7`.
    `(test-read-eval-string "(+ 3 4")` *signals an error*.
    `(test-read-eval-string "(+ 3 4) ")` *signals an error*, because
    there is extra “junk” (*i.e.* a space) after the list is read.

    The `test-read-eval-string` used in tests:

    ```
    (test-equal 7 (test-read-eval-string "(+ 3 4)"))
    (test-error (test-read-eval-string "(+ 3"))
    (test-equal #\newline (test-read-eval-string "#\\newline"))
    (test-error (test-read-eval-string "#\\newlin"))
    ;; Skip the next 2 tests unless srfi-62 is available.
    (test-skip (cond-expand (srfi-62 0) (else 2)))
    (test-equal 5 (test-read-eval-string "(+ 1 #;(* 2 3) 4)"))
    (test-equal '(x z) (test-read-string "(list 'x #;'y 'z)"))
    ```

#### Test groups and paths [¶](#Test-groups-and-paths)

A *test group* is a named sequence of forms containing testcases,
expressions, and definitions. Entering a group sets the *test group
name*; leaving a group restores the previous group name. These are
dynamic (run-time) operations, and a group has no other effect or
identity. Test groups are informal groupings: they are neither Scheme
values, nor are they syntactic forms.
A test group may contain nested inner test groups.
The *test group path* is a list of the currently-active
(entered) test group names, oldest (outermost) first.

Scheme Syntax: **test-begin** suite-name [count] [¶](#index-test_002dbegin)
:   A `test-begin` enters a new test group. The suite-name
    becomes the current test group name, and is added to the end of the test
    group path. Portable test suites should use a string literal for
    suite-name; the effect of expressions or other kinds of literals
    is unspecified.

    *Rationale:* In some ways using symbols would be preferable.
    However, we want human-readable names, and standard Scheme does not
    provide a way to include spaces or mixed-case text in literal symbols.

    The optional count must match the number of test-cases executed by
    this group. (Nested test groups count as a single test case for this
    count.) This extra test may be useful to catch cases where a test
    doesn’t get executed because of some unexpected error.

    Additionally, if there is no currently executing test runner,
    one is installed in an implementation-defined manner.

Scheme Syntax: **test-end** [suite-name] [¶](#index-test_002dend)
:   A `test-end` leaves the current test group.
    An error is reported if the suite-name does not
    match the current test group name.

    Additionally, if the matching `test-begin`installed a new
    test-runner, then the `test-end` will uninstall it, after reporting
    the accumulated test results in an implementation-defined manner.

Scheme Syntax: **test-group** suite-name decl-or-expr … [¶](#index-test_002dgroup)
:   Equivalent to:

    ```
    (if (not (test-to-skip% (var suite-name)))
      (dynamic-wind
        (lambda () (test-begin (var suite-name)))
        (lambda () (var decl-or-expr) ...)
        (lambda () (test-end (var suite-name)))))
    ```

    This is usually equivalent to executing the decl-or-exprs
    within the named test group. However, the entire group is skipped
    if it matched an active `test-skip` (see later).
    Also, the `test-end` is executed in case of an exception.

#### Handling set-up and cleanup [¶](#Handling-set_002dup-and-cleanup)

Scheme Syntax: **test-group-with-cleanup** suite-name decl-or-expr … cleanup-form [¶](#index-test_002dgroup_002dwith_002dcleanup)
:   Execute each of the decl-or-expr forms in order (as in a
    <body>), and then execute the cleanup-form. The latter
    should be executed even if one of a decl-or-expr forms raises an
    exception (assuming the implementation has a way to catch exceptions).

    For example:

    ```
    (let ((f (open-output-file "log")))
      (test-group-with-cleanup "test-file"
        (do-a-bunch-of-tests f)
        (close-output-port f)))
    ```

---

Next: [SRFI-64 Test Runner](#SRFI_002d64-Test-Runner), Previous: [SRFI-64 Writing Basic Test Suites](#SRFI_002d64-Writing-Basic-Test-Suites), Up: [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.37.4 SRFI-64 Conditonal Test Suites and Other Advanced Features [¶](#SRFI_002d64-Conditonal-Test-Suites-and-Other-Advanced-Features-1)

The following describes features for controlling which tests to execute,
or specifying that some tests are *expected* to fail.

#### Test specifiers [¶](#Test-specifiers)

Sometimes we want to only run certain tests, or we know that certain
tests are expected to fail. A *test specifier* is a one-argument
function that takes a test-runner and returns a boolean. The specifier
may be run before a test is performed, and the result may control
whether the test is executed. For convenience, a specifier may also be
a non-procedure value, which is coerced to a specifier procedure, as
described below for count and name.

A simple example is:

```
(if (var some-condition)  (test-skip 2)) ;; skip next 2 tests
```

Scheme Procedure: **test-match-name** name [¶](#index-test_002dmatch_002dname)
:   The resulting specifier matches if the current test name (as returned by
    `test-runner-test-name`) is `equal?` to name.

Scheme Syntax: **test-match-nth** n [count] [¶](#index-test_002dmatch_002dnth)
:   This evaluates to a *stateful* predicate: A counter keeps track of
    how many times it has been called. The predicate matches the n’th
    time it is called (where `1` is the first time), and the next
    ‘`(- count 1)`’ times, where count defaults to `1`.

Scheme Syntax: **test-match-any** specifier … [¶](#index-test_002dmatch_002dany)
:   The resulting specifier matches if any specifier matches. Each
    specifier is applied, in order, so side-effects from a later
    specifier happen even if an earlier specifier is true.

Scheme Syntax: **test-match-all** specifier … [¶](#index-test_002dmatch_002dall)
:   The resulting specifier matches if each specifier matches. Each
    specifier is applied, in order, so side-effects from a later
    specifier happen even if an earlier specifier is false.

count *(i.e. an integer)*
Convenience short-hand for: ‘`(test-match-nth 1 count)`’.

name *(i.e. a string)*
Convenience short-hand for ‘`(test-match-name name)`’.

#### Skipping selected tests [¶](#Skipping-selected-tests)

In some cases you may want to skip a test.

Scheme Syntax: **test-skip** specifier [¶](#index-test_002dskip)
:   Evaluating `test-skip` adds the resulting specifier to the
    set of currently active skip-specifiers. Before each test (or
    `test-group`) the set of active skip-specifiers are applied to the
    active test-runner. If any specifier matches, then the test is skipped.

    For convenience, if the specifier is a string that is syntactic
    sugar for `(test-match-name specifier)`. For example:

    ```
    (test-skip "test-b")
    (test-assert "test-a")   ;; executed
    (test-assert "test-b")   ;; skipped
    ```

    Any skip specifiers introduced by a `test-skip` are removed by a
    following non-nested `test-end`.

    ```
    (test-begin "group1")
    (test-skip "test-a")
    (test-assert "test-a")   ;; skipped
    (test-end "group1")      ;; Undoes the prior test-skip
    (test-assert "test-a")   ;; executed
    ```

#### Expected failures [¶](#Expected-failures)

Sometimes you know a test case will fail, but you don’t have time to or
can’t fix it. Maybe a certain feature only works on certain platforms.
However, you want the test-case to be there to remind you to fix it.
You want to note that such tests are expected to fail.

Scheme Syntax: **test-expect-fail** specifier [¶](#index-test_002dexpect_002dfail)
:   Matching tests (where matching is defined as in `test-skip`)
    are expected to fail. This only affects test reporting,
    not test execution. For example:

    ```
    (test-expect-fail 2)
    (test-eqv ...) ;; expected to fail
    (test-eqv ...) ;; expected to fail
    (test-eqv ...) ;; expected to pass
    ```

---

Next: [SRFI-64 Test Results](#SRFI_002d64-Test-Results), Previous: [SRFI-64 Conditonal Test Suites and Other Advanced Features](#SRFI_002d64-Conditonal-Test-Suites-and-Other-Advanced-Features), Up: [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.37.5 SRFI-64 Test Runner [¶](#SRFI_002d64-Test-Runner-1)

A *test-runner* is an object that runs a test-suite, and manages the
state. The test group path, and the sets skip and expected-fail
specifiers are part of the test-runner. A test-runner will also
typically accumulate statistics about executed tests,

Scheme Procedure: **test-runner?** value [¶](#index-test_002drunner_003f)
:   True if and only if value is a test-runner object.

Scheme Parameter: **test-runner-current** [¶](#index-test_002drunner_002dcurrent)

Scheme Parameter: **test-runner-current** runner [¶](#index-test_002drunner_002dcurrent-1)
:   Get or set the current test-runner.

Scheme Procedure: **test-runner-get** [¶](#index-test_002drunner_002dget)
:   Same as `(test-runner-current)`, but throws an exception if there
    is no current test-runner.

Scheme Procedure: **test-runner-simple** [¶](#index-test_002drunner_002dsimple)
:   Creates a new simple test-runner, that prints errors and a summary on
    the standard output port.
