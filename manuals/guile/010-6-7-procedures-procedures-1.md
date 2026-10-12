The VHash programming interface of `(ice-9 vlist)` is mostly the same as
that of association lists found in SRFI-1, with procedure names prefixed by
`vhash-` instead of `alist-` (see [Association Lists](#SRFI_002d1-Association-Lists)).

In addition, vhashes can be manipulated using VList operations:

```
(vlist-head (vhash-consq 'a 1 vlist-null))
⇒ (a . 1)

(define vh1 (vhash-consq 'b 2 (vhash-consq 'a 1 vlist-null)))
(define vh2 (vhash-consq 'c 3 (vlist-tail vh1)))

(vhash-assq 'a vh2)
⇒ (a . 1)
(vhash-assq 'b vh2)
⇒ #f
(vhash-assq 'c vh2)
⇒ (c . 3)
(vlist->list vh2)
⇒ ((c . 3) (a . 1))
```

However, keep in mind that procedures that construct new VLists
(`vlist-map`, `vlist-filter`, etc.) return raw VLists, not vhashes:

```
(define vh (alist->vhash '((a . 1) (b . 2) (c . 3)) hashq))
(vhash-assq 'a vh)
⇒ (a . 1)

(define vl
  ;; This will create a raw vlist.
  (vlist-filter (lambda (key+value) (odd? (cdr key+value))) vh))
(vhash-assq 'a vl)
⇒ ERROR: Wrong type argument in position 2

(vlist->list vl)
⇒ ((a . 1) (c . 3))
```

Scheme Procedure: **vhash?** obj [¶](#index-vhash_003f)
:   Return true if obj is a vhash.

Scheme Procedure: **vhash-cons** key value vhash [hash-proc] [¶](#index-vhash_002dcons)

Scheme Procedure: **vhash-consq** key value vhash [¶](#index-vhash_002dconsq)

Scheme Procedure: **vhash-consv** key value vhash [¶](#index-vhash_002dconsv)
:   Return a new hash list based on vhash where key is associated with
    value, using hash-proc to compute the hash of key.
    vhash must be either `vlist-null` or a vhash returned by a previous
    call to `vhash-cons`. hash-proc defaults to `hash` (see [`hash` procedure](#Hash-Table-Reference)). With `vhash-consq`, the
    `hashq` hash function is used; with `vhash-consv` the `hashv`
    hash function is used.

    All `vhash-cons` calls made to construct a vhash should use the same
    hash-proc. Failing to do that, the result is undefined.

Scheme Procedure: **vhash-assoc** key vhash [equal? [hash-proc]] [¶](#index-vhash_002dassoc)

Scheme Procedure: **vhash-assq** key vhash [¶](#index-vhash_002dassq)

Scheme Procedure: **vhash-assv** key vhash [¶](#index-vhash_002dassv)
:   Return the first key/value pair from vhash whose key is equal to key
    according to the equal? equality predicate (which defaults to
    `equal?`), and using hash-proc (which defaults to `hash`) to
    compute the hash of key. The second form uses `eq?` as the equality
    predicate and `hashq` as the hash function; the last form uses `eqv?`
    and `hashv`.

    Note that it is important to consistently use the same hash function for
    hash-proc as was passed to `vhash-cons`. Failing to do that, the
    result is unpredictable.

Scheme Procedure: **vhash-delete** key vhash [equal? [hash-proc]] [¶](#index-vhash_002ddelete)

Scheme Procedure: **vhash-delq** key vhash [¶](#index-vhash_002ddelq)

Scheme Procedure: **vhash-delv** key vhash [¶](#index-vhash_002ddelv)
:   Remove all associations from vhash with key, comparing keys with
    equal? (which defaults to `equal?`), and computing the hash of
    key using hash-proc (which defaults to `hash`). The second
    form uses `eq?` as the equality predicate and `hashq` as the hash
    function; the last one uses `eqv?` and `hashv`.

    Again the choice of hash-proc must be consistent with previous calls to
    `vhash-cons`.

Scheme Procedure: **vhash-fold** proc init vhash [¶](#index-vhash_002dfold)

Scheme Procedure: **vhash-fold-right** proc init vhash [¶](#index-vhash_002dfold_002dright)
:   Fold over the key/value elements of vhash in the given direction,
    with each call to proc having the form `(proc key value
    result)`, where result is the result of the previous call to
    proc and init the value of result for the first call
    to proc.

Scheme Procedure: **vhash-fold\*** proc init key vhash [equal? [hash]] [¶](#index-vhash_002dfold_002a)

Scheme Procedure: **vhash-foldq\*** proc init key vhash [¶](#index-vhash_002dfoldq_002a)

Scheme Procedure: **vhash-foldv\*** proc init key vhash [¶](#index-vhash_002dfoldv_002a)
:   Fold over all the values associated with key in vhash, with each
    call to proc having the form `(proc value result)`, where
    result is the result of the previous call to proc and init the
    value of result for the first call to proc.

    Keys in vhash are hashed using hash are compared using equal?.
    The second form uses `eq?` as the equality predicate and `hashq` as
    the hash function; the third one uses `eqv?` and `hashv`.

    Example:

    ```
    (define vh
      (alist->vhash '((a . 1) (a . 2) (z . 0) (a . 3))))

    (vhash-fold* cons '() 'a vh)
    ⇒ (3 2 1)

    (vhash-fold* cons '() 'z vh)
    ⇒ (0)
    ```

Scheme Procedure: **alist->vhash** alist [hash-proc] [¶](#index-alist_002d_003evhash)
:   Return the vhash corresponding to alist, an association list, using
    hash-proc to compute key hashes. When omitted, hash-proc defaults
    to `hash`.

---

Next: [Other Types](#Other-Types), Previous: [VList-Based Hash Lists or “VHashes”](#VHashes), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.22 Hash Tables [¶](#Hash-Tables-1)

Hash tables are dictionaries which offer similar functionality as
association lists: They provide a mapping from keys to values. The
difference is that association lists need time linear in the size of
elements when searching for entries, whereas hash tables can normally
search in constant time. The drawback is that hash tables require a
little bit more memory, and that you can not use the normal list
procedures (see [Lists](#Lists)) for working with them.

* [Hash Table Examples](#Hash-Table-Examples)
* [Hash Table Reference](#Hash-Table-Reference)

---

Next: [Hash Table Reference](#Hash-Table-Reference), Up: [Hash Tables](#Hash-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.22.1 Hash Table Examples [¶](#Hash-Table-Examples-1)

For demonstration purposes, this section gives a few usage examples of
some hash table procedures, together with some explanation what they do.

First we start by creating a new hash table with 31 slots, and
populate it with two key/value pairs.

```
(define h (make-hash-table 31))

;; This is an opaque object
h
⇒
#<hash-table 0/31>

;; Inserting into a hash table can be done with hashq-set!
(hashq-set! h 'foo "bar")
⇒
"bar"

(hashq-set! h 'braz "zonk")
⇒
"zonk"

;; Or with hash-create-handle!
(hashq-create-handle! h 'frob #f)
⇒
(frob . #f)
```

You can get the value for a given key with the procedure
`hashq-ref`, but the problem with this procedure is that you
cannot reliably determine whether a key does exists in the table. The
reason is that the procedure returns `#f` if the key is not in
the table, but it will return the same value if the key is in the
table and just happens to have the value `#f`, as you can see in
the following examples.

```
(hashq-ref h 'foo)
⇒
"bar"

(hashq-ref h 'frob)
⇒
#f

(hashq-ref h 'not-there)
⇒
#f
```

It is often better is to use the procedure `hashq-get-handle`,
which makes a distinction between the two cases. Just like `assq`,
this procedure returns a key/value-pair on success, and `#f` if the
key is not found.

```
(hashq-get-handle h 'foo)
⇒
(foo . "bar")

(hashq-get-handle h 'not-there)
⇒
#f
```

Interesting results can be computed by using `hash-fold` to work
through each element. This example will count the total number of
elements:

```
(hash-fold (lambda (key value seed) (+ 1 seed)) 0 h)
⇒
3
```

The same thing can be done with the procedure `hash-count`, which
can also count the number of elements matching a particular predicate.
For example, count the number of elements with string values:

```
(hash-count (lambda (key value) (string? value)) h)
⇒
2
```

Counting all the elements is a simple task using `const`:

```
(hash-count (const #t) h)
⇒
3
```

---

Previous: [Hash Table Examples](#Hash-Table-Examples), Up: [Hash Tables](#Hash-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.22.2 Hash Table Reference [¶](#Hash-Table-Reference-1)

Like the association list functions, the hash table functions come in
several varieties, according to the equality test used for the keys.
Plain `hash-` functions use `equal?`, `hashq-`
functions use `eq?`, `hashv-` functions use `eqv?`, and
the `hashx-` functions use an application supplied test.

A single `make-hash-table` creates a hash table suitable for use
with any set of functions, but it’s imperative that just one set is
then used consistently, or results will be unpredictable.

Hash tables are implemented as a vector indexed by a hash value formed
from the key, with an association list of key/value pairs for each
bucket in case distinct keys hash together. Direct access to the
pairs in those lists is provided by the `-handle-` functions.

When the number of entries in a hash table goes above a threshold, the
vector is made larger and the entries are rehashed, to prevent the
bucket lists from becoming too long and slowing down accesses. When the
number of entries goes below a threshold, the vector is shrunk to save
space.

For the `hashx-` “extended” routines, an application supplies a
hash function producing an integer index like `hashq` etc
below, and an assoc alist search function like `assq` etc
(see [Retrieving Alist Entries](#Retrieving-Alist-Entries)). Here’s an example of such
functions implementing case-insensitive hashing of string keys,

```
(use-modules (srfi srfi-1)
             (srfi srfi-13))

(define (my-hash str size)
  (remainder (string-hash-ci str) size))
(define (my-assoc str alist)
  (find (lambda (pair) (string-ci=? str (car pair))) alist))

(define my-table (make-hash-table))
(hashx-set! my-hash my-assoc my-table "foo" 123)

(hashx-ref my-hash my-assoc my-table "FOO")
⇒ 123
```

In a `hashx-` hash function the aim is to spread keys
across the vector, so bucket lists don’t become long. But the actual
values are arbitrary as long as they’re in the range 0 to
*size-1*. Helpful functions for forming a hash value, in
addition to `hashq` etc below, include `symbol-hash`
(see [Symbols as Lookup Keys](#Symbol-Keys)), `string-hash` and `string-hash-ci`
(see [String Comparison](#String-Comparison)), and `char-set-hash`
(see [Character Set Predicates/Comparison](#Character-Set-Predicates_002fComparison)).

  

Scheme Procedure: **make-hash-table** [size] [¶](#index-make_002dhash_002dtable)
:   Create a new hash table object, with an optional minimum
    vector size.

    When size is given, the table vector will still grow and shrink
    automatically, as described above, but with size as a minimum.
    If an application knows roughly how many entries the table will hold
    then it can use size to avoid rehashing when initial entries are
    added.

Scheme Procedure: **alist->hash-table** alist [¶](#index-alist_002d_003ehash_002dtable)

Scheme Procedure: **alist->hashq-table** alist [¶](#index-alist_002d_003ehashq_002dtable)

Scheme Procedure: **alist->hashv-table** alist [¶](#index-alist_002d_003ehashv_002dtable)

Scheme Procedure: **alist->hashx-table** hash assoc alist [¶](#index-alist_002d_003ehashx_002dtable)
:   Convert alist into a hash table. When keys are repeated in
    alist, the leftmost association takes precedence.

    ```
    (use-modules (ice-9 hash-table))
    (alist->hash-table '((foo . 1) (bar . 2)))
    ```

    When converting to an extended hash table, custom hash and
    assoc procedures must be provided.

    ```
    (alist->hashx-table hash assoc '((foo . 1) (bar . 2)))
    ```

Scheme Procedure: **hash-table?** obj [¶](#index-hash_002dtable_003f)

C Function: **scm\_hash\_table\_p** (obj) [¶](#index-scm_005fhash_005ftable_005fp)
:   Return `#t` if obj is a abstract hash table object.

Scheme Procedure: **hash-clear!** table [¶](#index-hash_002dclear_0021)

C Function: **scm\_hash\_clear\_x** (table) [¶](#index-scm_005fhash_005fclear_005fx)
:   Remove all items from table (without triggering a resize).

Scheme Procedure: **hash-ref** table key [dflt] [¶](#index-hash_002dref)

Scheme Procedure: **hashq-ref** table key [dflt] [¶](#index-hashq_002dref)

Scheme Procedure: **hashv-ref** table key [dflt] [¶](#index-hashv_002dref)

Scheme Procedure: **hashx-ref** hash assoc table key [dflt] [¶](#index-hashx_002dref)

C Function: **scm\_hash\_ref** (table, key, dflt) [¶](#index-scm_005fhash_005fref)

C Function: **scm\_hashq\_ref** (table, key, dflt) [¶](#index-scm_005fhashq_005fref)

C Function: **scm\_hashv\_ref** (table, key, dflt) [¶](#index-scm_005fhashv_005fref)

C Function: **scm\_hashx\_ref** (hash, assoc, table, key, dflt) [¶](#index-scm_005fhashx_005fref)
:   Lookup key in the given hash table, and return the
    associated value. If key is not found, return dflt, or
    `#f` if dflt is not given.

Scheme Procedure: **hash-set!** table key val [¶](#index-hash_002dset_0021)

Scheme Procedure: **hashq-set!** table key val [¶](#index-hashq_002dset_0021)

Scheme Procedure: **hashv-set!** table key val [¶](#index-hashv_002dset_0021)

Scheme Procedure: **hashx-set!** hash assoc table key val [¶](#index-hashx_002dset_0021)

C Function: **scm\_hash\_set\_x** (table, key, val) [¶](#index-scm_005fhash_005fset_005fx)

C Function: **scm\_hashq\_set\_x** (table, key, val) [¶](#index-scm_005fhashq_005fset_005fx)

C Function: **scm\_hashv\_set\_x** (table, key, val) [¶](#index-scm_005fhashv_005fset_005fx)

C Function: **scm\_hashx\_set\_x** (hash, assoc, table, key, val) [¶](#index-scm_005fhashx_005fset_005fx)
:   Associate val with key in the given hash table. If
    key is already present then it’s associated value is changed.
    If it’s not present then a new entry is created.

Scheme Procedure: **hash-remove!** table key [¶](#index-hash_002dremove_0021)

Scheme Procedure: **hashq-remove!** table key [¶](#index-hashq_002dremove_0021)

Scheme Procedure: **hashv-remove!** table key [¶](#index-hashv_002dremove_0021)

Scheme Procedure: **hashx-remove!** hash assoc table key [¶](#index-hashx_002dremove_0021)

C Function: **scm\_hash\_remove\_x** (table, key) [¶](#index-scm_005fhash_005fremove_005fx)

C Function: **scm\_hashq\_remove\_x** (table, key) [¶](#index-scm_005fhashq_005fremove_005fx)

C Function: **scm\_hashv\_remove\_x** (table, key) [¶](#index-scm_005fhashv_005fremove_005fx)

C Function: **scm\_hashx\_remove\_x** (hash, assoc, table, key) [¶](#index-scm_005fhashx_005fremove_005fx)
:   Remove any association for key in the given hash table.
    If key is not in table then nothing is done.

Scheme Procedure: **hash** key size [¶](#index-hash)

Scheme Procedure: **hashq** key size [¶](#index-hashq)

Scheme Procedure: **hashv** key size [¶](#index-hashv)

C Function: **scm\_hash** (key, size) [¶](#index-scm_005fhash)

C Function: **scm\_hashq** (key, size) [¶](#index-scm_005fhashq)

C Function: **scm\_hashv** (key, size) [¶](#index-scm_005fhashv)
:   Return a hash value for key. This is a number in the range
    *0* to *size-1*, which is suitable for use in a hash
    table of the given size.

    Note that `hashq` and `hashv` may use internal addresses of
    objects, so if an object is garbage collected and re-created it can
    have a different hash value, even when the two are notionally
    `eq?`. For instance with symbols,

    ```
    (hashq 'something 123)   ⇒ 19
    (gc)
    (hashq 'something 123)   ⇒ 62
    ```

    In normal use this is not a problem, since an object entered into a
    hash table won’t be garbage collected until removed. It’s only if
    hashing calculations are somehow separated from normal references that
    its lifetime needs to be considered.

Scheme Procedure: **hash-get-handle** table key [¶](#index-hash_002dget_002dhandle)

Scheme Procedure: **hashq-get-handle** table key [¶](#index-hashq_002dget_002dhandle)

Scheme Procedure: **hashv-get-handle** table key [¶](#index-hashv_002dget_002dhandle)

Scheme Procedure: **hashx-get-handle** hash assoc table key [¶](#index-hashx_002dget_002dhandle)

C Function: **scm\_hash\_get\_handle** (table, key) [¶](#index-scm_005fhash_005fget_005fhandle)

C Function: **scm\_hashq\_get\_handle** (table, key) [¶](#index-scm_005fhashq_005fget_005fhandle)

C Function: **scm\_hashv\_get\_handle** (table, key) [¶](#index-scm_005fhashv_005fget_005fhandle)

C Function: **scm\_hashx\_get\_handle** (hash, assoc, table, key) [¶](#index-scm_005fhashx_005fget_005fhandle)
:   Return the `(key . value)` pair for key in the
    given hash table, or `#f` if key is not in
    table.

Scheme Procedure: **hash-create-handle!** table key init [¶](#index-hash_002dcreate_002dhandle_0021)

Scheme Procedure: **hashq-create-handle!** table key init [¶](#index-hashq_002dcreate_002dhandle_0021)

Scheme Procedure: **hashv-create-handle!** table key init [¶](#index-hashv_002dcreate_002dhandle_0021)

Scheme Procedure: **hashx-create-handle!** hash assoc table key init [¶](#index-hashx_002dcreate_002dhandle_0021)

C Function: **scm\_hash\_create\_handle\_x** (table, key, init) [¶](#index-scm_005fhash_005fcreate_005fhandle_005fx)

C Function: **scm\_hashq\_create\_handle\_x** (table, key, init) [¶](#index-scm_005fhashq_005fcreate_005fhandle_005fx)

C Function: **scm\_hashv\_create\_handle\_x** (table, key, init) [¶](#index-scm_005fhashv_005fcreate_005fhandle_005fx)

C Function: **scm\_hashx\_create\_handle\_x** (hash, assoc, table, key, init) [¶](#index-scm_005fhashx_005fcreate_005fhandle_005fx)
:   Return the `(key . value)` pair for key in the
    given hash table. If key is not in table then
    create an entry for it with init as the value, and return that
    pair.

Scheme Procedure: **hash-map->list** proc table [¶](#index-hash_002dmap_002d_003elist)

Scheme Procedure: **hash-for-each** proc table [¶](#index-hash_002dfor_002deach)

C Function: **scm\_hash\_map\_to\_list** (proc, table) [¶](#index-scm_005fhash_005fmap_005fto_005flist)

C Function: **scm\_hash\_for\_each** (proc, table) [¶](#index-scm_005fhash_005ffor_005feach)
:   Apply proc to the entries in the given hash table. Each
    call is `(proc key value)`. `hash-map->list`
    returns a list of the results from these calls, `hash-for-each`
    discards the results and returns an unspecified value.

    Calls are made over the table entries in an unspecified order, and for
    `hash-map->list` the order of the values in the returned list is
    unspecified. Results will be unpredictable if table is modified
    while iterating.

    For example the following returns a new alist comprising all the
    entries from `mytable`, in no particular order.

    ```
    (hash-map->list cons mytable)
    ```

Scheme Procedure: **hash-for-each-handle** proc table [¶](#index-hash_002dfor_002deach_002dhandle)

C Function: **scm\_hash\_for\_each\_handle** (proc, table) [¶](#index-scm_005fhash_005ffor_005feach_005fhandle)
:   Apply proc to the entries in the given hash table. Each
    call is `(proc handle)`, where handle is a
    `(key . value)` pair. Return an unspecified value.

    `hash-for-each-handle` differs from `hash-for-each` only in
    the argument list of proc.

Scheme Procedure: **hash-fold** proc init table [¶](#index-hash_002dfold)

C Function: **scm\_hash\_fold** (proc, init, table) [¶](#index-scm_005fhash_005ffold)
:   Accumulate a result by applying proc to the elements of the
    given hash table. Each call is `(proc key
    value prior-result)`, where key and value are
    from the table and prior-result is the return from the
    previous proc call. For the first call, prior-result is
    the given init value.

    Calls are made over the table entries in an unspecified order.
    Results will be unpredictable if table is modified while
    `hash-fold` is running.

    For example, the following returns a count of how many keys in
    `mytable` are strings.

    ```
    (hash-fold (lambda (key value prior)
                 (if (string? key) (1+ prior) prior))
               0 mytable)
    ```

Scheme Procedure: **hash-count** pred table [¶](#index-hash_002dcount)

C Function: **scm\_hash\_count** (pred, table) [¶](#index-scm_005fhash_005fcount)
:   Return the number of elements in the given hash table that cause
    `(pred key value)` to return true. To quickly
    determine the total number of elements, use `(const #t)` for
    pred.

---

Previous: [Hash Tables](#Hash-Tables), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.23 Other Types [¶](#Other-Types-1)

Procedures are documented in their own section. See [Procedures](#Procedures).

Variable objects are documented as part of the description of Guile’s
module system: see [Variables](#Variables).

See [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling), for discussion of threads, mutexes, and so on.

Ports are described in the section on I/O: see [Input and Output](#Input-and-Output).

Regular expressions are described in their own section: see [Regular Expressions](#Regular-Expressions).

There are quite a number of additional data types documented in this
manual; if you feel a link is missing here, please file a bug.

---

Next: [Macros](#Macros), Previous: [Data Types](#Data-Types), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.7 Procedures [¶](#Procedures-1)

* [Lambda: Basic Procedure Creation](#Lambda)
* [Primitive Procedures](#Primitive-Procedures)
* [Compiled Procedures](#Compiled-Procedures)
* [Optional Arguments](#Optional-Arguments)
* [Case-lambda](#Case_002dlambda)
* [Higher-Order Functions](#Higher_002dOrder-Functions)
* [Procedure Properties and Meta-information](#Procedure-Properties)
* [Procedures with Setters](#Procedures-with-Setters)
* [Inlinable Procedures](#Inlinable-Procedures)

---

Next: [Primitive Procedures](#Primitive-Procedures), Up: [Procedures](#Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.1 Lambda: Basic Procedure Creation [¶](#Lambda_003a-Basic-Procedure-Creation)

A `lambda` expression evaluates to a procedure. The environment
which is in effect when a `lambda` expression is evaluated is
enclosed in the newly created procedure, this is referred to as a
*closure* (see [The Concept of Closure](#About-Closure)).

When a procedure created by `lambda` is called with some actual
arguments, the environment enclosed in the procedure is extended by
binding the variables named in the formal argument list to new locations
and storing the actual arguments into these locations. Then the body of
the `lambda` expression is evaluated sequentially. The result of
the last expression in the procedure body is then the result of the
procedure invocation.

The following examples will show how procedures can be created using
`lambda`, and what you can do with these procedures.

```
(lambda (x) (+ x x))       ⇒ a procedure
((lambda (x) (+ x x)) 4)   ⇒ 8
```

The fact that the environment in effect when creating a procedure is
enclosed in the procedure is shown with this example:

```
(define add4
  (let ((x 4))
    (lambda (y) (+ x y))))
(add4 6)                   ⇒ 10
```

syntax: **lambda** formals body [¶](#index-lambda-1)
:   formals should be a formal argument list as described in the
    following table.

    `(variable1 …)`
    :   The procedure takes a fixed number of arguments; when the procedure is
        called, the arguments will be stored into the newly created location for
        the formal variables.

    `variable`
    :   The procedure takes any number of arguments; when the procedure is
        called, the sequence of actual arguments will be converted into a list
        and stored into the newly created location for the formal variable.

    `(variable1 … variablen . variablen+1)`
    :   If a space-delimited period precedes the last variable, then the
        procedure takes n or more variables where n is the number
        of formal arguments before the period. There must be at least one
        argument before the period. The first n actual arguments will be
        stored into the newly allocated locations for the first n formal
        arguments and the sequence of the remaining actual arguments is
        converted into a list and the stored into the location for the last
        formal argument. If there are exactly n actual arguments, the
        empty list is stored into the location of the last formal argument.

    The list in variable or variablen+1 is always newly
    created and the procedure can modify it if desired. This is the case
    even when the procedure is invoked via `apply`, the required part
    of the list argument there will be copied (see [Procedures for On the Fly Evaluation](#Fly-Evaluation)).

    body is a sequence of Scheme expressions which are evaluated in
    order when the procedure is invoked.

---

Next: [Compiled Procedures](#Compiled-Procedures), Previous: [Lambda: Basic Procedure Creation](#Lambda), Up: [Procedures](#Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.2 Primitive Procedures [¶](#Primitive-Procedures-1)

Procedures written in C can be registered for use from Scheme,
provided they take only arguments of type `SCM` and return
`SCM` values. `scm_c_define_gsubr` is likely to be the most
useful mechanism, combining the process of registration
(`scm_c_make_gsubr`) and definition (`scm_define`).

Function: `SCM` **scm\_c\_make\_gsubr** `(const char *name, int req, int opt, int rst, fcn)` [¶](#index-scm_005fc_005fmake_005fgsubr)
:   Register a C procedure fcn as a “subr” — a primitive
    subroutine that can be called from Scheme. It will be associated with
    the given name but no environment binding will be created. The
    arguments req, opt and rst specify the number of
    required, optional and “rest” arguments respectively. The total
    number of these arguments should match the actual number of arguments
    to fcn, but may not exceed 10. The number of rest arguments should be 0 or 1.
    `scm_c_make_gsubr` returns a value of type `SCM` which is a
    “handle” for the procedure.

Function: `SCM` **scm\_c\_define\_gsubr** `(const char *name, int req, int opt, int rst, fcn)` [¶](#index-scm_005fc_005fdefine_005fgsubr)
:   Register a C procedure fcn, as for `scm_c_make_gsubr`
    above, and additionally create a top-level Scheme binding for the
    procedure in the “current environment” using `scm_define`.
    `scm_c_define_gsubr` returns a handle for the procedure in the
    same way as `scm_c_make_gsubr`, which is usually not further
    required.

See [Foreign Functions](#Foreign-Functions), for another interface to call procedures
written in C from Scheme.

---

Next: [Optional Arguments](#Optional-Arguments), Previous: [Primitive Procedures](#Primitive-Procedures), Up: [Procedures](#Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.3 Compiled Procedures [¶](#Compiled-Procedures-1)

The evaluation strategy given in [Lambda: Basic Procedure Creation](#Lambda) describes how procedures
are *interpreted*. Interpretation operates directly on expanded
Scheme source code, recursively calling the evaluator to obtain the
value of nested expressions.

Most procedures are compiled, however. This means that Guile has done
some pre-computation on the procedure, to determine what it will need to
do each time the procedure runs. Compiled procedures run faster than
interpreted procedures.

Loading files is the normal way that compiled procedures come to
being. If Guile sees that a file is uncompiled, or that its compiled
file is out of date, it will attempt to compile the file when it is
loaded, and save the result to disk. Procedures can be compiled at
runtime as well. See [Reading and Evaluating Scheme Code](#Read_002fLoad_002fEval_002fCompile), for more information
on runtime compilation.

Compiled procedures, also known as *programs*, respond to all
procedures that operate on procedures: you can pass a program to
`procedure?`, `procedure-name`, and so on (see [Procedure Properties and Meta-information](#Procedure-Properties)). In addition, there are a few more accessors for low-level
details on programs.

Most people won’t need to use the routines described in this section,
but it’s good to have them documented. You’ll have to include the
appropriate module first, though:

```
(use-modules (system vm program))
```

Scheme Procedure: **program?** obj [¶](#index-program_003f)

C Function: **scm\_program\_p** (obj) [¶](#index-scm_005fprogram_005fp)
:   Returns `#t` if obj is a compiled procedure, or `#f`
    otherwise.

Scheme Procedure: **program-code** program [¶](#index-program_002dcode)

C Function: **scm\_program\_code** (program) [¶](#index-scm_005fprogram_005fcode)
:   Returns the address of the program’s entry, as an integer. This address
    is mostly useful to procedures in `(system vm debug)`.

Scheme Procedure: **program-num-free-variable** program [¶](#index-program_002dnum_002dfree_002dvariable)

C Function: **scm\_program\_num\_free\_variables** (program) [¶](#index-scm_005fprogram_005fnum_005ffree_005fvariables)
:   Return the number of free variables captured by this program.

Scheme Procedure: **program-free-variable-ref** program n [¶](#index-program_002dfree_002dvariable_002dref)

C Function: **scm\_program\_free\_variable-ref** (program, n) [¶](#index-scm_005fprogram_005ffree_005fvariable_002dref)

Scheme Procedure: **program-free-variable-set!** program n val [¶](#index-program_002dfree_002dvariable_002dset_0021)

C Function: **scm\_program\_free\_variable\_set\_x** (program, n, val) [¶](#index-scm_005fprogram_005ffree_005fvariable_005fset_005fx)
:   Accessors for a program’s free variables. Some of the values captured
    are actually in variable “boxes”. See [Variables and the VM](#Variables-and-the-VM), for
    more information.

    Users must not modify the returned value unless they think they’re
    really clever.

Scheme Procedure: **program-sources** program [¶](#index-program_002dsources)

Scheme Procedure: **source:addr** source [¶](#index-source_003aaddr)

Scheme Procedure: **source:line** source [¶](#index-source_003aline)

Scheme Procedure: **source:column** source [¶](#index-source_003acolumn)

Scheme Procedure: **source:file** source [¶](#index-source_003afile)
:   Source location annotations for programs, along with their accessors.

    Source location information propagates through the compiler and ends
    up being serialized to the program’s metadata. This information is
    keyed by the offset of the instruction pointer within the object code
    of the program. Specifically, it is keyed on the `ip` *just
    following* an instruction, so that backtraces can find the source
    location of a call that is in progress.

Scheme Procedure: **program-arities** program [¶](#index-program_002darities)

C Function: **scm\_program\_arities** (program) [¶](#index-scm_005fprogram_005farities)

Scheme Procedure: **program-arity** program ip [¶](#index-program_002darity)

Scheme Procedure: **arity:start** arity [¶](#index-arity_003astart)

Scheme Procedure: **arity:end** arity [¶](#index-arity_003aend)

Scheme Procedure: **arity:nreq** arity [¶](#index-arity_003anreq)

Scheme Procedure: **arity:nopt** arity [¶](#index-arity_003anopt)

Scheme Procedure: **arity:rest?** arity [¶](#index-arity_003arest_003f)

Scheme Procedure: **arity:kw** arity [¶](#index-arity_003akw)

Scheme Procedure: **arity:allow-other-keys?** arity [¶](#index-arity_003aallow_002dother_002dkeys_003f)
:   Accessors for a representation of the “arity” of a program.

    The normal case is that a procedure has one arity. For example,
    `(lambda (x) x)`, takes one required argument, and that’s it. One
    could access that number of required arguments via `(arity:nreq
    (program-arities (lambda (x) x)))`. Similarly, `arity:nopt` gets
    the number of optional arguments, and `arity:rest?` returns a true
    value if the procedure has a rest arg.

    `arity:kw` returns a list of `(kw . idx)` pairs,
    if the procedure has keyword arguments. The idx refers to the
    idxth local variable; See [Variables and the VM](#Variables-and-the-VM), for more
    information. Finally `arity:allow-other-keys?` returns a true
    value if other keys are allowed. See [Optional Arguments](#Optional-Arguments), for more
    information.

    So what about `arity:start` and `arity:end`, then? They
    return the range of bytes in the program’s bytecode for which a given
    arity is valid. You see, a procedure can actually have more than one
    arity. The question, “what is a procedure’s arity” only really makes
    sense at certain points in the program, delimited by these
    `arity:start` and `arity:end` values.

Scheme Procedure: **program-arguments-alist** program [ip] [¶](#index-program_002darguments_002dalist)
:   Return an association list describing the arguments that program accepts, or
    `#f` if the information cannot be obtained.

    The alist keys that are currently defined are ‘required’, ‘optional’,
    ‘keyword’, ‘allow-other-keys?’, and ‘rest’. For example:

    ```
    (program-arguments-alist
     (lambda* (a b #:optional c #:key (d 1) #:rest e)
       #t)) ⇒
    ((required . (a b))
     (optional . (c))
     (keyword . ((#:d . 4)))
     (allow-other-keys? . #f)
     (rest . d))
    ```

Scheme Procedure: **program-lambda-list** program [ip] [¶](#index-program_002dlambda_002dlist)
:   Return a representation of the arguments of program as a lambda
    list, or `#f` if this information is not available.

    For example:

    ```
    (program-lambda-list
     (lambda* (a b #:optional c #:key (d 1) #:rest e)
       #t)) ⇒
    ```

---

Next: [Case-lambda](#Case_002dlambda), Previous: [Compiled Procedures](#Compiled-Procedures), Up: [Procedures](#Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.4 Optional Arguments [¶](#Optional-Arguments-1)

Scheme procedures, as defined in R5RS, can either handle a fixed number
of actual arguments, or a fixed number of actual arguments followed by
arbitrarily many additional arguments. Writing procedures of variable
arity can be useful, but unfortunately, the syntactic means for handling
argument lists of varying length is a bit inconvenient. It is possible
to give names to the fixed number of arguments, but the remaining
(optional) arguments can be only referenced as a list of values
(see [Lambda: Basic Procedure Creation](#Lambda)).

For this reason, Guile provides an extension to `lambda`,
`lambda*`, which allows the user to define procedures with
optional and keyword arguments. In addition, Guile’s virtual machine
has low-level support for optional and keyword argument dispatch.
Calls to procedures with optional and keyword arguments can be made
cheaply, without allocating a rest list.

* [lambda\* and define\*.](#lambda_002a-and-define_002a)
* [(ice-9 optargs)](#ice_002d9-optargs)

---

Next: [(ice-9 optargs)](#ice_002d9-optargs), Up: [Optional Arguments](#Optional-Arguments)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.4.1 lambda\* and define\*. [¶](#lambda_002a-and-define_002a_002e)

`lambda*` is like `lambda`, except with some extensions to
allow optional and keyword arguments.

library syntax: **lambda\*** ([var…] [#:optional vardef…] [#:key vardef… [#:allow-other-keys]] [#:rest var | . var]) body1 body2 … [¶](#index-lambda_002a)
:   Create a procedure which takes optional and/or keyword arguments
    specified with `#:optional` and `#:key`. For example,

    ```
    (lambda* (a b #:optional c d . e) '())
    ```

    is a procedure with fixed arguments a and b, optional
    arguments c and d, and rest argument e. If the
    optional arguments are omitted in a call, the variables for them are
    bound to `#f`.

    Likewise, `define*` is syntactic sugar for defining procedures
    using `lambda*`.

    `lambda*` can also make procedures with keyword arguments. For
    example, a procedure defined like this:

    ```
    (define* (sir-yes-sir #:key action how-high)
      (list action how-high))
    ```

    can be called as `(sir-yes-sir #:action 'jump)`,
    `(sir-yes-sir #:how-high 13)`, `(sir-yes-sir #:action
    'lay-down #:how-high 0)`, or just `(sir-yes-sir)`. Whichever
    arguments are given as keywords are bound to values (and those not
    given are `#f`).

    Optional and keyword arguments can also have default values to take
    when not present in a call, by giving a two-element list of variable
    name and expression. For example in

    ```
    (define* (frob foo #:optional (bar 42) #:key (baz 73))
      (list foo bar baz))
    ```

    foo is a fixed argument, bar is an optional argument with
    default value 42, and baz is a keyword argument with default value 73.
    Default value expressions are not evaluated unless they are needed,
    and until the procedure is called.

    Normally it’s an error if a call has keywords other than those
    specified by `#:key`, but adding `#:allow-other-keys` to the
    definition (after the keyword argument declarations) will ignore
    unknown keywords.

    If a call has a keyword given twice, the last value is used. For
    example,

    ```
    (define* (flips #:key (heads 0) (tails 0))
      (display (list heads tails)))

    (flips #:heads 37 #:tails 42 #:heads 99)
    ⊣ (99 42)
    ```

    `#:rest` is a synonym for the dotted syntax rest argument. The
    argument lists `(a . b)` and `(a #:rest b)` are equivalent
    in all respects. This is provided for more similarity to DSSSL,
    MIT-Scheme and Kawa among others, as well as for refugees from other
    Lisp dialects.

    When `#:key` is used together with a rest argument, the keyword
    parameters in a call all remain in the rest list. This is the same as
    Common Lisp. For example,

    ```
    ((lambda* (#:key (x 0) #:allow-other-keys #:rest r)
       (display r))
     #:x 123 #:y 456)
    ⊣ (#:x 123 #:y 456)
    ```

    `#:optional` and `#:key` establish their bindings
    successively, from left to right. This means default expressions can
    refer back to prior parameters, for example

    ```
    (lambda* (start #:optional (end (+ 10 start)))
      (do ((i start (1+ i)))
          ((> i end))
        (display i)))
    ```

    The exception to this left-to-right scoping rule is the rest argument.
    If there is a rest argument, it is bound after the optional arguments,
    but before the keyword arguments.

---

Previous: [lambda\* and define\*.](#lambda_002a-and-define_002a), Up: [Optional Arguments](#Optional-Arguments)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.4.2 (ice-9 optargs) [¶](#g_t_0028ice_002d9-optargs_0029)

Before Guile 2.0, `lambda*` and `define*` were implemented
using macros that processed rest list arguments. This was not optimal,
as calling procedures with optional arguments had to allocate rest
lists at every procedure invocation. Guile 2.0 improved this
situation by bringing optional and keyword arguments into Guile’s
core.

However there are occasions in which you have a list and want to parse
it for optional or keyword arguments. Guile’s `(ice-9 optargs)`
provides some macros to help with that task.

The syntax `let-optional` and `let-optional*` are for
destructuring rest argument lists and giving names to the various list
elements. `let-optional` binds all variables simultaneously, while
`let-optional*` binds them sequentially, consistent with `let`
and `let*` (see [Local Variable Bindings](#Local-Bindings)).

library syntax: **let-optional** rest-arg (binding …) body1 body2 … [¶](#index-let_002doptional)

library syntax: **let-optional\*** rest-arg (binding …) body1 body2 … [¶](#index-let_002doptional_002a)
:   These two macros give you an optional argument interface that is very
    *Schemey* and introduces no fancy syntax. They are compatible with
    the scsh macros of the same name, but are slightly extended. Each of
    binding may be of one of the forms var or `(var
    default-value)`. rest-arg should be the rest-argument of the
    procedures these are used from. The items in rest-arg are
    sequentially bound to the variable names are given. When rest-arg
    runs out, the remaining vars are bound either to the default values or
    `#f` if no default value was specified. rest-arg remains
    bound to whatever may have been left of rest-arg.

    After binding the variables, the expressions body1 body2 …
    are evaluated in order.

Similarly, `let-keywords` and `let-keywords*` extract values
from keyword style argument lists, binding local variables to those
values or to defaults.

library syntax: **let-keywords** args allow-other-keys? (binding …) body1 body2 … [¶](#index-let_002dkeywords)

library syntax: **let-keywords\*** args allow-other-keys? (binding …) body1 body2 … [¶](#index-let_002dkeywords_002a)
:   args is evaluated and should give a list of the form
    `(#:keyword1 value1 #:keyword2 value2 …)`. The
    bindings are variables and default expressions, with the variables
    to be set (by name) from the keyword values. The body1
    body2 … forms are then evaluated and the last is the
    result. An example will make the syntax clearest,

    ```
    (define args '(#:xyzzy "hello" #:foo "world"))

    (let-keywords args #t
          ((foo  "default for foo")
           (bar  (string-append "default" "for" "bar")))
      (display foo)
      (display ", ")
      (display bar))
    ⊣ world, defaultforbar
    ```

    The binding for `foo` comes from the `#:foo` keyword in
    `args`. But the binding for `bar` is the default in the
    `let-keywords`, since there’s no `#:bar` in the args.

    allow-other-keys? is evaluated and controls whether unknown
    keywords are allowed in the args list. When true other keys are
    ignored (such as `#:xyzzy` in the example), when `#f` an
    error is thrown for anything unknown.

`(ice-9 optargs)` also provides some more `define*` sugar,
which is not so useful with modern Guile coding, but still supported:
`define*-public` is the `lambda*` version of
`define-public`; `defmacro*` and `defmacro*-public`
exist for defining macros with the improved argument list handling
possibilities. The `-public` versions not only define the
procedures/macros, but also export them from the current module.

library syntax: **define\*-public** formals body1 body2 … [¶](#index-define_002a_002dpublic)
:   Like a mix of `define*` and `define-public`.

library syntax: **defmacro\*** name formals body1 body2 … [¶](#index-defmacro_002a)

library syntax: **defmacro\*-public** name formals body1 body2 … [¶](#index-defmacro_002a_002dpublic)
:   These are just like `defmacro` and `defmacro-public` except that they
    take `lambda*`-style extended parameter lists, where `#:optional`,
    `#:key`, `#:allow-other-keys` and `#:rest` are allowed with the usual
    semantics. Here is an example of a macro with an optional argument:

    ```
    (defmacro* transmogrify (a #:optional b)
      (a 1))
    ```

---

Next: [Higher-Order Functions](#Higher_002dOrder-Functions), Previous: [Optional Arguments](#Optional-Arguments), Up: [Procedures](#Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.5 Case-lambda [¶](#Case_002dlambda-1)

R5RS’s rest arguments are indeed useful and very general, but they
often aren’t the most appropriate or efficient means to get the job
done. For example, `lambda*` is a much better solution to the
optional argument problem than `lambda` with rest arguments.

Likewise, `case-lambda` works well for when you want one
procedure to do double duty (or triple, or ...), without the penalty
of consing a rest list.

For example:

```
(define (make-accum n)
  (case-lambda
    (() n)
    ((m) (set! n (+ n m)) n)))

(define a (make-accum 20))
(a) ⇒ 20
(a 10) ⇒ 30
(a) ⇒ 30
```

The value returned by a `case-lambda` form is a procedure which
matches the number of actual arguments against the formals in the
various clauses, in order. The first matching clause is selected, the
corresponding values from the actual parameter list are bound to the
variable names in the clauses and the body of the clause is evaluated.
If no clause matches, an error is signaled.

The syntax of the `case-lambda` form is defined in the following
EBNF grammar. *Formals* means a formal argument list just like
with `lambda` (see [Lambda: Basic Procedure Creation](#Lambda)).

```
<case-lambda>
   --> (case-lambda <case-lambda-clause>*)
   --> (case-lambda <docstring> <case-lambda-clause>*)
<case-lambda-clause>
   --> (<formals> <definition-or-command>*)
<formals>
   --> (<identifier>*)
     | (<identifier>* . <identifier>)
     | <identifier>
```

Rest lists can be useful with `case-lambda`:

```
(define plus
  (case-lambda
    "Return the sum of all arguments."
    (() 0)
    ((a) a)
    ((a b) (+ a b))
    ((a b . rest) (apply plus (+ a b) rest))))
(plus 1 2 3) ⇒ 6
```

Also, for completeness. Guile defines `case-lambda*` as well,
which is like `case-lambda`, except with `lambda*` clauses.
A `case-lambda*` clause matches if the arguments fill the
required arguments, but are not too many for the optional and/or rest
arguments.

Keyword arguments are possible with `case-lambda*` as well, but
they do not contribute to the “matching” behavior, and their
interactions with required, optional, and rest arguments can be
surprising.

For the purposes of `case-lambda*` (and of `case-lambda`, as a
special case), a clause *matches* if it has enough required
arguments, and not too many positional arguments. The required
arguments are any arguments before the `#:optional`, `#:key`,
and `#:rest` arguments. *Positional* arguments are the
required arguments, together with the optional arguments.

In the absence of `#:key` or `#:rest` arguments, it’s easy to
see how there could be too many positional arguments: you pass 5
arguments to a function that only takes 4 arguments, including optional
arguments. If there is a `#:rest` argument, there can never be too
many positional arguments: any application with enough required
arguments for a clause will match that clause, even if there are also
`#:key` arguments.

Otherwise, for applications to a clause with `#:key` arguments (and
without a `#:rest` argument), a clause will match there only if
there are enough required arguments and if the next argument after
binding required and optional arguments, if any, is a keyword. For
efficiency reasons, Guile is currently unable to include keyword
arguments in the matching algorithm. Clauses match on positional
arguments only, not by comparing a given keyword to the available set of
keyword arguments that a function has.

Some examples follow.

```
(define f
  (case-lambda*
    ((a #:optional b) 'clause-1)
    ((a #:optional b #:key c) 'clause-2)
    ((a #:key d) 'clause-3)
    ((#:key e #:rest f) 'clause-4)))

(f) ⇒ clause-4
(f 1) ⇒ clause-1
(f) ⇒ clause-4
(f #:e 10) clause-1
(f 1 #:foo) clause-1
(f 1 #:c 2) clause-2
(f #:a #:b #:c #:d #:e) clause-4

;; clause-2 will match anything that clause-3 would match.
(f 1 #:d 2) ⇒ error: bad keyword args in clause 2
```

Don’t forget that the clauses are matched in order, and the first
matching clause will be taken. This can result in a keyword being bound
to a required argument, as in the case of `f #:e 10`.

---

Next: [Procedure Properties and Meta-information](#Procedure-Properties), Previous: [Case-lambda](#Case_002dlambda), Up: [Procedures](#Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.6 Higher-Order Functions [¶](#Higher_002dOrder-Functions-1)

As a functional programming language, Scheme allows the definition of
*higher-order functions*, i.e., functions that take functions as
arguments and/or return functions. Utilities to derive procedures from
other procedures are provided and described below.

Scheme Procedure: **const** value [¶](#index-const)
:   Return a procedure that accepts any number of arguments and returns
    value.

    ```
    (procedure? (const 3))        ⇒ #t
    ((const 'hello))              ⇒ hello
    ((const 'hello) 'world)       ⇒ hello
    ```

Scheme Procedure: **negate** proc [¶](#index-negate)
:   Return a procedure with the same arity as proc that returns the
    `not` of proc’s result.

    ```
    (procedure? (negate number?)) ⇒ #t
    ((negate odd?) 2)             ⇒ #t
    ((negate real?) 'dream)       ⇒ #t
    ((negate string-prefix?) "GNU" "GNU Guile")
                                  ⇒ #f
    (filter (negate number?) '(a 2 "b"))
                                  ⇒ (a "b")
    ```

Scheme Procedure: **compose** proc1 proc2 … [¶](#index-compose)
:   Compose proc1 with the procedures proc2 … such that
    the last proc argument is applied first and proc1 last, and
    return the resulting procedure. The given procedures must have
    compatible arity.

    ```
    (procedure? (compose 1+ 1-)) ⇒ #t
    ((compose sqrt 1+ 1+) 2)     ⇒ 2.0
    ((compose 1+ sqrt) 3)        ⇒ 2.73205080756888
    (eq? (compose 1+) 1+)        ⇒ #t

    ((compose zip unzip2) '((1 2) (a b)))
                                 ⇒ ((1 2) (a b))
    ```

Scheme Procedure: **identity** x [¶](#index-identity)
:   Return X.

Scheme Procedure: **and=>** value proc [¶](#index-and_003d_003e)
:   When value is `#f`, return `#f`. Otherwise, return
    `(proc value)`.

---

Next: [Procedures with Setters](#Procedures-with-Setters), Previous: [Higher-Order Functions](#Higher_002dOrder-Functions), Up: [Procedures](#Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.7 Procedure Properties and Meta-information [¶](#Procedure-Properties-and-Meta_002dinformation)

In addition to the information that is strictly necessary to run,
procedures may have other associated information. For example, the
name of a procedure is information not for the procedure, but about
the procedure. This meta-information can be accessed via the procedure
properties interface.

Scheme Procedure: **procedure?** obj [¶](#index-procedure_003f)

C Function: **scm\_procedure\_p** (obj) [¶](#index-scm_005fprocedure_005fp)
:   Return `#t` if obj is a procedure.

Scheme Procedure: **thunk?** obj [¶](#index-thunk_003f)

C Function: **scm\_thunk\_p** (obj) [¶](#index-scm_005fthunk_005fp)
:   Return `#t` if obj is a procedure that can be called with
    zero arguments. See [Compiled Procedures](#Compiled-Procedures), to get more information
    on what arguments a procedure will accept.

Procedure properties are general properties associated with
procedures. These can be the name of a procedure or other relevant
information, such as debug hints.

The most general way to associate a property of a procedure is
programmatically:

Scheme Procedure: **procedure-property** proc key [¶](#index-procedure_002dproperty)

C Function: **scm\_procedure\_property** (proc, key) [¶](#index-scm_005fprocedure_005fproperty)
:   Return the property of proc with name key, or `#f` if
    not found.

Scheme Procedure: **set-procedure-property!** proc key value [¶](#index-set_002dprocedure_002dproperty_0021)

C Function: **scm\_set\_procedure\_property\_x** (proc, key, value) [¶](#index-scm_005fset_005fprocedure_005fproperty_005fx)
:   Set proc’s property named key to value.

However, there is a more efficient interface that allows constant
properties to be embedded into compiled binaries in a way that does
not incur any overhead until someone asks for the property: initial
non-tail elements of the body of a lambda expression that are literal
vectors of pairs are interpreted as declaring procedure properties.
This is easiest to see with an example:

```
(define proc
  (lambda args
    #((a . "hey") (b . "ho")) ;; procedure properties!
    42))
(procedure-property proc 'a) ; ⇒ "hey"
(procedure-property proc 'b) ; ⇒ "ho"
```

There is a shorthand for declaring the `documentation` property,
which is a literal string instead of a literal vector:

```
(define proc
  (lambda args
    "This is a docstring."
    42))
(procedure-property proc 'documentation)
;; ⇒ "This is a docstring."
```

Calling `procedure-property` with a key of `documentation`
is exactly the same as calling `procedure-documentation`.
Similarly, `procedure-name` is the same as the `name`
procedure property, and `procedure-source` is for the
`source` property.

Scheme Procedure: **procedure-name** proc [¶](#index-procedure_002dname)

C Function: **scm\_procedure\_name** (proc) [¶](#index-scm_005fprocedure_005fname)

Scheme Procedure: **procedure-source** proc [¶](#index-procedure_002dsource)

C Function: **scm\_procedure\_source** (proc) [¶](#index-scm_005fprocedure_005fsource)

Scheme Procedure: **procedure-documentation** proc [¶](#index-procedure_002ddocumentation)

C Function: **scm\_procedure\_documentation** (proc) [¶](#index-scm_005fprocedure_005fdocumentation)
:   Return the value of the `name`, `source`, or
    `documentation` property for proc, or `#f` if no
    property is set.

One can also work on the entire set of procedure properties.

Scheme Procedure: **procedure-properties** proc [¶](#index-procedure_002dproperties)

C Function: **scm\_procedure\_properties** (proc) [¶](#index-scm_005fprocedure_005fproperties)
:   Return the properties associated with proc, as an association
    list.

Scheme Procedure: **set-procedure-properties!** proc alist [¶](#index-set_002dprocedure_002dproperties_0021)

C Function: **scm\_set\_procedure\_properties\_x** (proc, alist) [¶](#index-scm_005fset_005fprocedure_005fproperties_005fx)
:   Set proc’s property list to alist.

---

Next: [Inlinable Procedures](#Inlinable-Procedures), Previous: [Procedure Properties and Meta-information](#Procedure-Properties), Up: [Procedures](#Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.8 Procedures with Setters [¶](#Procedures-with-Setters-1)

A *procedure with setter* is a special kind of procedure which
normally behaves like any accessor procedure, that is a procedure which
accesses a data structure. The difference is that this kind of
procedure has a so-called *setter* attached, which is a procedure
for storing something into a data structure.

Procedures with setters are treated specially when the procedure appears
in the special form `set!`. How it works is best shown by example.

Suppose we have a procedure called `foo-ref`, which accepts two
arguments, a value of type `foo` and an integer. The procedure
returns the value stored at the given index in the `foo` object.
Let `f` be a variable containing such a `foo` data
structure.[13](#FOOT13)

```
(foo-ref f 0)       ⇒ bar
(foo-ref f 1)       ⇒ braz
```

Also suppose that a corresponding setter procedure called
`foo-set!` does exist.

```
(foo-set! f 0 'bla)
(foo-ref f 0)       ⇒ bla
```

Now we could create a new procedure called `foo`, which is a
procedure with setter, by calling `make-procedure-with-setter` with
the accessor and setter procedures `foo-ref` and `foo-set!`.
Let us call this new procedure `foo`.

```
(define foo (make-procedure-with-setter foo-ref foo-set!))
```

`foo` can from now on be used to either read from the data
structure stored in `f`, or to write into the structure.

```
(set! (foo f 0) 'dum)
(foo f 0)          ⇒ dum
```

Scheme Procedure: **make-procedure-with-setter** procedure setter [¶](#index-make_002dprocedure_002dwith_002dsetter)

C Function: **scm\_make\_procedure\_with\_setter** (procedure, setter) [¶](#index-scm_005fmake_005fprocedure_005fwith_005fsetter)
:   Create a new procedure which behaves like procedure, but
    with the associated setter setter.

Scheme Procedure: **procedure-with-setter?** obj [¶](#index-procedure_002dwith_002dsetter_003f)

C Function: **scm\_procedure\_with\_setter\_p** (obj) [¶](#index-scm_005fprocedure_005fwith_005fsetter_005fp)
:   Return `#t` if obj is a procedure with an
    associated setter procedure.

Scheme Procedure: **procedure** proc [¶](#index-procedure)

C Function: **scm\_procedure** (proc) [¶](#index-scm_005fprocedure)
:   Return the procedure of proc, which must be an
    applicable struct.

Scheme Procedure: **setter** proc [¶](#index-setter-1)
:   Return the setter of proc, which must be either a procedure with
    setter or an operator struct.

---

Previous: [Procedures with Setters](#Procedures-with-Setters), Up: [Procedures](#Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.7.9 Inlinable Procedures [¶](#Inlinable-Procedures-1)

You can define an *inlinable procedure* by using
`define-inlinable` instead of `define`. An inlinable
procedure behaves the same as a regular procedure, but direct calls will
result in the procedure body being inlined into the caller.

Bear in mind that starting from version 2.0.3, Guile has a partial
evaluator that can inline the body of inner procedures when deemed
appropriate:

```
scheme@(guile-user)> ,optimize (define (foo x)
                                 (define (bar) (+ x 3))
                                 (* (bar) 2))
$1 = (define foo
       (lambda (#{x 94}#) (* (+ #{x 94}# 3) 2)))
```

The partial evaluator does not inline top-level bindings, though, so
this is a situation where you may find it interesting to use
`define-inlinable`.

Procedures defined with `define-inlinable` are *always*
inlined, at all direct call sites. This eliminates function call
overhead at the expense of an increase in code size. Additionally, the
caller will not transparently use the new definition if the inline
procedure is redefined. It is not possible to trace an inlined
procedures or install a breakpoint in it (see [Traps](#Traps)). For these
reasons, you should not make a procedure inlinable unless it
demonstrably improves performance in a crucial way.

In general, only small procedures should be considered for inlining, as
making large procedures inlinable will probably result in an increase in
code size. Additionally, the elimination of the call overhead rarely
matters for large procedures.

Scheme Syntax: **define-inlinable** (name parameter …) body1 body2 … [¶](#index-define_002dinlinable)
:   Define name as a procedure with parameters parameters and
    bodies body1, body2, ....

---

Next: [General Utility Functions](#Utility-Functions), Previous: [Procedures](#Procedures), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
