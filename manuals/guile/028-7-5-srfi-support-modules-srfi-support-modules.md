### 7.5 SRFI Support Modules [¶](#SRFI-Support-Modules)

SRFI is an acronym for Scheme Request For Implementation. The SRFI
documents define a lot of syntactic and procedure extensions to standard
Scheme as defined in R5RS.

Guile has support for a number of SRFIs. This chapter gives an overview
over the available SRFIs and some usage hints. For complete
documentation, design rationales and further examples, we advise you to
get the relevant SRFI documents from the SRFI home page
<http://srfi.schemers.org/>.

* [About SRFI Usage](#About-SRFI-Usage)
* [SRFI-0 - cond-expand](#SRFI_002d0)
* [SRFI-1 - List library](#SRFI_002d1)
* [SRFI-2 - and-let\*](#SRFI_002d2)
* [SRFI-4 - Homogeneous numeric vector datatypes](#SRFI_002d4)
* [SRFI-6 - Basic String Ports](#SRFI_002d6)
* [SRFI-8 - receive](#SRFI_002d8)
* [SRFI-9 - define-record-type](#SRFI_002d9)
* [SRFI-10 - Hash-Comma Reader Extension](#SRFI_002d10)
* [SRFI-11 - let-values](#SRFI_002d11)
* [SRFI-13 - String Library](#SRFI_002d13)
* [SRFI-14 - Character-set Library](#SRFI_002d14)
* [SRFI-16 - case-lambda](#SRFI_002d16)
* [SRFI-17 - Generalized set!](#SRFI_002d17)
* [SRFI-18 - Multithreading support](#SRFI_002d18)
* [SRFI-19 - Time/Date Library](#SRFI_002d19)
* [SRFI-23 - Error Reporting](#SRFI_002d23)
* [SRFI-26 - specializing parameters](#SRFI_002d26)
* [SRFI-27 - Sources of Random Bits](#SRFI_002d27)
* [SRFI-28 - Basic Format Strings](#SRFI_002d28)
* [SRFI-30 - Nested Multi-line Comments](#SRFI_002d30)
* [SRFI-31 - A special form ‘rec’ for recursive evaluation](#SRFI_002d31)
* [SRFI-34 - Exception handling for programs](#SRFI_002d34)
* [SRFI-35 - Conditions](#SRFI_002d35)
* [SRFI-37 - args-fold](#SRFI_002d37)
* [SRFI-38 - External Representation for Data With Shared Structure](#SRFI_002d38)
* [SRFI-39 - Parameters](#SRFI_002d39)
* [SRFI-41 - Streams](#SRFI_002d41)
* [SRFI-42 - Eager Comprehensions](#SRFI_002d42)
* [SRFI-43 - Vector Library](#SRFI_002d43)
* [SRFI-45 - Primitives for Expressing Iterative Lazy Algorithms](#SRFI_002d45)
* [SRFI-46 Basic syntax-rules Extensions](#SRFI_002d46)
* [SRFI-55 - Requiring Features](#SRFI_002d55)
* [SRFI-60 - Integers as Bits](#SRFI_002d60)
* [SRFI-61 - A more general `cond` clause](#SRFI_002d61)
* [SRFI-62 - S-expression comments.](#SRFI_002d62)
* [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64)
* [SRFI-67 - Compare procedures](#SRFI_002d67)
* [SRFI-69 - Basic hash tables](#SRFI_002d69)
* [SRFI-71 - Extended let-syntax for multiple values](#SRFI_002d71)
* [SRFI-87 => in case clauses](#SRFI_002d87)
* [SRFI-88 Keyword Objects](#SRFI_002d88)
* [SRFI-98 Accessing environment variables.](#SRFI_002d98)
* [SRFI-105 Curly-infix expressions.](#SRFI_002d105)
* [SRFI-111 Boxes.](#SRFI_002d111)
* [SRFI-119 Wisp: simpler indentation-sensitive Scheme.](#SRFI_002d119)
* [Transducers](#SRFI_002d171)
* [SRFI-197: Pipeline Operators](#SRFI_002d197)
* [SRFI-207 String-notated bytevectors](#SRFI_002d207)
* [SRFI-244 - Multiple-value Definitions](#SRFI_002d244)

---

Next: [SRFI-0 - cond-expand](#SRFI_002d0), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.1 About SRFI Usage [¶](#About-SRFI-Usage-1)

SRFI support in Guile is currently implemented partly in the core
library, and partly as add-on modules. That means that some SRFIs are
automatically available when the interpreter is started, whereas the
other SRFIs require you to use the appropriate support module
explicitly.

There are several reasons for this inconsistency. First, the feature
checking syntactic form `cond-expand` (see [SRFI-0 - cond-expand](#SRFI_002d0)) must be
available immediately, because it must be there when the user wants to
check for the Scheme implementation, that is, before she can know that
it is safe to use `use-modules` to load SRFI support modules. The
second reason is that some features defined in SRFIs had been
implemented in Guile before the developers started to add SRFI
implementations as modules (for example SRFI-13 (see [SRFI-13 - String Library](#SRFI_002d13))). In
the future, it is possible that SRFIs in the core library might be
factored out into separate modules, requiring explicit module loading
when they are needed. So you should be prepared to have to use
`use-modules` someday in the future to access SRFI-13 bindings. If
you want, you can do that already. We have included the module
`(srfi srfi-13)` in the distribution, which currently does nothing,
but ensures that you can write future-safe code.

Generally, support for a specific SRFI is made available by using
modules named `(srfi srfi-number)`, where number is the
number of the SRFI needed. Another possibility is to use the command
line option `--use-srfi`, which will load the necessary modules
automatically (see [Invoking Guile](#Invoking-Guile)).

---

Next: [SRFI-1 - List library](#SRFI_002d1), Previous: [About SRFI Usage](#About-SRFI-Usage), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.2 SRFI-0 - cond-expand [¶](#SRFI_002d0-_002d-cond_002dexpand)

This SRFI lets a portable Scheme program test for the presence of
certain features, and adapt itself by using different blocks of code,
or fail if the necessary features are not available. There’s no
module to load, this is in the Guile core.

A program designed only for Guile will generally not need this
mechanism, such a program can of course directly use the various
documented parts of Guile.

syntax: **cond-expand** (feature body…) … [¶](#index-cond_002dexpand)
:   Expand to the body of the first clause whose feature
    specification is satisfied. It is an error if no feature is
    satisfied.

    Features are symbols such as `srfi-1`, and a feature
    specification can use `and`, `or` and `not` forms to
    test combinations. The last clause can be an `else`, to be used
    if no other passes.

    For example, define a private version of `alist-cons` if SRFI-1
    is not available.

    ```
    (cond-expand (srfi-1
                  )
                 (else
                  (define (alist-cons key val alist)
                    (cons (cons key val) alist))))
    ```

    Or demand a certain set of SRFIs (list operations, string ports,
    `receive` and string operations), failing if they’re not
    available.

    ```
    (cond-expand ((and srfi-1 srfi-6 srfi-8 srfi-13)
                  ))
    ```

The Guile core has the following features,

```
guile
guile-2   ;; starting from Guile 2.x
guile-2.2 ;; starting from Guile 2.2
guile-3   ;; starting from Guile 3.x
guile-3.0 ;; starting from Guile 3.0
r5rs
r6rs
r7rs
exact-closed ieee-float full-unicode ratios ;; R7RS features
srfi-0
srfi-4
srfi-6
srfi-13
srfi-14
srfi-16
srfi-23
srfi-30
srfi-39
srfi-46
srfi-55
srfi-61
srfi-62
srfi-87
srfi-105
```

Other SRFI feature symbols are defined once their code has been loaded
with `use-modules`, since only then are their bindings available.

The ‘`--use-srfi`’ command line option (see [Invoking Guile](#Invoking-Guile)) is
a good way to load SRFIs to satisfy `cond-expand` when running a
portable program.

Testing the `guile` feature allows a program to adapt itself to
the Guile module system, but still run on other Scheme systems. For
example the following demands SRFI-8 (`receive`), but also knows
how to load it with the Guile mechanism.

```
(cond-expand (srfi-8
              )
             (guile
              (use-modules (srfi srfi-8))))
```

Likewise, testing the `guile-2` feature allows code to be portable
between Guile 2.x and previous versions of Guile. For instance, it
makes it possible to write code that accounts for Guile 2.x’s compiler,
yet be correctly interpreted on 1.8 and earlier versions:

```
(cond-expand (guile-2 (eval-when (compile)
                        ;; This must be evaluated at compile time.
                        (fluid-set! current-reader my-reader)))
             (guile
                      ;; Earlier versions of Guile do not have a
                      ;; separate compilation phase.
                      (fluid-set! current-reader my-reader)))
```

It should be noted that `cond-expand` is separate from the
`*features*` mechanism (see [Feature Tracking](#Feature-Tracking)), feature
symbols in one are unrelated to those in the other.

---

Next: [SRFI-2 - and-let\*](#SRFI_002d2), Previous: [SRFI-0 - cond-expand](#SRFI_002d0), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3 SRFI-1 - List library [¶](#SRFI_002d1-_002d-List-library)

The list library defined in SRFI-1 contains a lot of useful list
processing procedures for construction, examining, destructuring and
manipulating lists and pairs.

Since SRFI-1 also defines some procedures which are already contained
in R5RS and thus are supported by the Guile core library, some list
and pair procedures which appear in the SRFI-1 document may not appear
in this section. So when looking for a particular list/pair
processing procedure, you should also have a look at the sections
[Lists](#Lists) and [Pairs](#Pairs).

* [Constructors](#SRFI_002d1-Constructors)
* [Predicates](#SRFI_002d1-Predicates)
* [Selectors](#SRFI_002d1-Selectors)
* [Length, Append, Concatenate, etc.](#SRFI_002d1-Length-Append-etc)
* [Fold, Unfold & Map](#SRFI_002d1-Fold-and-Map)
* [Filtering and Partitioning](#SRFI_002d1-Filtering-and-Partitioning)
* [Searching](#SRFI_002d1-Searching)
* [Deleting](#SRFI_002d1-Deleting)
* [Association Lists](#SRFI_002d1-Association-Lists)
* [Set Operations on Lists](#SRFI_002d1-Set-Operations)

---

Next: [Predicates](#SRFI_002d1-Predicates), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.1 Constructors [¶](#Constructors)

New lists can be constructed by calling one of the following
procedures.

Scheme Procedure: **xcons** d a [¶](#index-xcons)
:   Like `cons`, but with interchanged arguments. Useful mostly when
    passed to higher-order procedures.

Scheme Procedure: **list-tabulate** n init-proc [¶](#index-list_002dtabulate)
:   Return an n-element list, where each list element is produced by
    applying the procedure init-proc to the corresponding list
    index. The order in which init-proc is applied to the indices
    is not specified.

Scheme Procedure: **list-copy** lst [¶](#index-list_002dcopy-1)
:   Return a new list containing the elements of the list lst.

    This function differs from the core `list-copy` (see [List Constructors](#List-Constructors)) in accepting improper lists too. And if lst is
    not a pair at all then it’s treated as the final tail of an improper
    list and simply returned.

Scheme Procedure: **circular-list** elt1 elt2 … [¶](#index-circular_002dlist)
:   Return a circular list containing the given arguments elt1
    elt2 ….

Scheme Procedure: **iota** count [start step] [¶](#index-iota)
:   Return a list containing count numbers, starting from
    start and adding step each time. The default start
    is 0, the default step is 1. For example,

    ```
    (iota 6)        ⇒ (0 1 2 3 4 5)
    (iota 4 2.5 -2) ⇒ (2.5 0.5 -1.5 -3.5)
    ```

    This function takes its name from the corresponding primitive in the
    APL language.

---

Next: [Selectors](#SRFI_002d1-Selectors), Previous: [Constructors](#SRFI_002d1-Constructors), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.2 Predicates [¶](#Predicates)

The procedures in this section test specific properties of lists.

Scheme Procedure: **proper-list?** obj [¶](#index-proper_002dlist_003f)
:   Return `#t` if obj is a proper list, or `#f`
    otherwise. This is the same as the core `list?` (see [List Predicates](#List-Predicates)).

    A proper list is a list which ends with the empty list `()` in
    the usual way. The empty list `()` itself is a proper list too.

    ```
    (proper-list? '(1 2 3))  ⇒ #t
    (proper-list? '())       ⇒ #t
    ```

Scheme Procedure: **circular-list?** obj [¶](#index-circular_002dlist_003f)
:   Return `#t` if obj is a circular list, or `#f`
    otherwise.

    A circular list is a list where at some point the `cdr` refers
    back to a previous pair in the list (either the start or some later
    point), so that following the `cdr`s takes you around in a
    circle, with no end.

    ```
    (define x (list 1 2 3 4))
    (set-cdr! (last-pair x) (cddr x))
    x ⇒ (1 2 3 4 3 4 3 4 ...)
    (circular-list? x)  ⇒ #t
    ```

Scheme Procedure: **dotted-list?** obj [¶](#index-dotted_002dlist_003f)
:   Return `#t` if obj is a dotted list, or `#f`
    otherwise.

    A dotted list is a list where the `cdr` of the last pair is not
    the empty list `()`. Any non-pair obj is also considered a
    dotted list, with length zero.

    ```
    (dotted-list? '(1 2 . 3))  ⇒ #t
    (dotted-list? 99)          ⇒ #t
    ```

It will be noted that any Scheme object passes exactly one of the
above three tests `proper-list?`, `circular-list?` and
`dotted-list?`. Non-lists are `dotted-list?`, finite lists
are either `proper-list?` or `dotted-list?`, and infinite
lists are `circular-list?`.

  

Scheme Procedure: **null-list?** lst [¶](#index-null_002dlist_003f)
:   Return `#t` if lst is the empty list `()`, `#f`
    otherwise. If something else than a proper or circular list is passed
    as lst, an error is signaled. This procedure is recommended
    for checking for the end of a list in contexts where dotted lists are
    not allowed.

Scheme Procedure: **not-pair?** obj [¶](#index-not_002dpair_003f)
:   Return `#t` is obj is not a pair, `#f` otherwise.
    This is shorthand notation `(not (pair? obj))` and is
    supposed to be used for end-of-list checking in contexts where dotted
    lists are allowed.

Scheme Procedure: **list=** elt= list1 … [¶](#index-list_003d)
:   Return `#t` if all argument lists are equal, `#f` otherwise.
    List equality is determined by testing whether all lists have the same
    length and the corresponding elements are equal in the sense of the
    equality predicate elt=. If no or only one list is given,
    `#t` is returned.

---

Next: [Length, Append, Concatenate, etc.](#SRFI_002d1-Length-Append-etc), Previous: [Predicates](#SRFI_002d1-Predicates), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.3 Selectors [¶](#Selectors)

Scheme Procedure: **first** pair [¶](#index-first)

Scheme Procedure: **second** pair [¶](#index-second)

Scheme Procedure: **third** pair [¶](#index-third)

Scheme Procedure: **fourth** pair [¶](#index-fourth)

Scheme Procedure: **fifth** pair [¶](#index-fifth)

Scheme Procedure: **sixth** pair [¶](#index-sixth)

Scheme Procedure: **seventh** pair [¶](#index-seventh)

Scheme Procedure: **eighth** pair [¶](#index-eighth)

Scheme Procedure: **ninth** pair [¶](#index-ninth)

Scheme Procedure: **tenth** pair [¶](#index-tenth)
:   These are synonyms for `car`, `cadr`, `caddr`, ….

Scheme Procedure: **car+cdr** pair [¶](#index-car_002bcdr)
:   Return two values, the CAR and the CDR of pair.

```
(car+cdr '(0 1 2 3))
⇒
0
(1 2 3)
```

Scheme Procedure: **take** lst i [¶](#index-take)

Scheme Procedure: **take!** lst i [¶](#index-take_0021)
:   Return a list containing the first i elements of lst.

    `take!` may modify the structure of the argument list lst
    in order to produce the result.

Scheme Procedure: **drop** lst i [¶](#index-drop)
:   Return a list containing all but the first i elements of
    lst.

Scheme Procedure: **take-right** lst i [¶](#index-take_002dright)
:   Return a list containing the i last elements of lst.
    The return shares a common tail with lst.

Scheme Procedure: **drop-right** lst i [¶](#index-drop_002dright)

Scheme Procedure: **drop-right!** lst i [¶](#index-drop_002dright_0021)
:   Return a list containing all but the i last elements of
    lst.

    `drop-right` always returns a new list, even when i is
    zero. `drop-right!` may modify the structure of the argument
    list lst in order to produce the result.

Scheme Procedure: **split-at** lst i [¶](#index-split_002dat)

Scheme Procedure: **split-at!** lst i [¶](#index-split_002dat_0021)
:   Return two values, a list containing the first i elements of the
    list lst and a list containing the remaining elements.

    `split-at!` may modify the structure of the argument list
    lst in order to produce the result.

Scheme Procedure: **last** lst [¶](#index-last)
:   Return the last element of the non-empty, finite list lst.

---

Next: [Fold, Unfold & Map](#SRFI_002d1-Fold-and-Map), Previous: [Selectors](#SRFI_002d1-Selectors), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.4 Length, Append, Concatenate, etc. [¶](#Length_002c-Append_002c-Concatenate_002c-etc_002e)

Scheme Procedure: **length+** lst [¶](#index-length_002b)
:   Return the length of the argument list lst. When lst is a
    circular list, `#f` is returned.

Scheme Procedure: **concatenate** list-of-lists [¶](#index-concatenate)

Scheme Procedure: **concatenate!** list-of-lists [¶](#index-concatenate_0021)
:   Construct a list by appending all lists in list-of-lists.

    `concatenate!` may modify the structure of the given lists in
    order to produce the result.

    `concatenate` is the same as `(apply append
    list-of-lists)`. It exists because some Scheme implementations
    have a limit on the number of arguments a function takes, which the
    `apply` might exceed. In Guile there is no such limit.

Scheme Procedure: **append-reverse** rev-head tail [¶](#index-append_002dreverse)

Scheme Procedure: **append-reverse!** rev-head tail [¶](#index-append_002dreverse_0021)
:   Reverse rev-head, append tail to it, and return the
    result. This is equivalent to `(append (reverse rev-head)
    tail)`, but its implementation is more efficient.

    ```
    (append-reverse '(1 2 3) '(4 5 6)) ⇒ (3 2 1 4 5 6)
    ```

    `append-reverse!` may modify rev-head in order to produce
    the result.

Scheme Procedure: **zip** lst1 lst2 … [¶](#index-zip)
:   Return a list as long as the shortest of the argument lists, where
    each element is a list. The first list contains the first elements of
    the argument lists, the second list contains the second elements, and
    so on.

Scheme Procedure: **unzip1** lst [¶](#index-unzip1)

Scheme Procedure: **unzip2** lst [¶](#index-unzip2)

Scheme Procedure: **unzip3** lst [¶](#index-unzip3)

Scheme Procedure: **unzip4** lst [¶](#index-unzip4)

Scheme Procedure: **unzip5** lst [¶](#index-unzip5)
:   `unzip1` takes a list of lists, and returns a list containing the
    first elements of each list, `unzip2` returns two lists, the
    first containing the first elements of each lists and the second
    containing the second elements of each lists, and so on.

Scheme Procedure: **count** pred lst1 lst2 … [¶](#index-count)
:   Return a count of the number of times pred returns true when
    called on elements from the given lists.

    pred is called with N parameters `(pred
    elem1 … elemN )`, each element being from the
    corresponding list. The first call is with the first element of each
    list, the second with the second element from each, and so on.

    Counting stops when the end of the shortest list is reached. At least
    one list must be non-circular.

---

Next: [Filtering and Partitioning](#SRFI_002d1-Filtering-and-Partitioning), Previous: [Length, Append, Concatenate, etc.](#SRFI_002d1-Length-Append-etc), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.5 Fold, Unfold & Map [¶](#Fold_002c-Unfold-_0026-Map)

Scheme Procedure: **fold** proc init lst1 lst2 … [¶](#index-fold)

Scheme Procedure: **fold-right** proc init lst1 lst2 … [¶](#index-fold_002dright)
:   Apply proc to the elements of lst1 lst2 … to
    build a result, and return that result.

    Each proc call is `(proc elem1 elem2
    … previous)`, where elem1 is from lst1,
    elem2 is from lst2, and so on. previous is the return
    from the previous call to proc, or the given init for the
    first call. If any list is empty, just init is returned.

    `fold` works through the list elements from first to last. The
    following shows a list reversal and the calls it makes,

    ```
    (fold cons '() '(1 2 3))

    (cons 1 '())
    (cons 2 '(1))
    (cons 3 '(2 1)
    ⇒ (3 2 1)
    ```

    `fold-right` works through the list elements from last to first,
    ie. from the right. So for example the following finds the longest
    string, and the last among equal longest,

    ```
    (fold-right (lambda (str prev)
                  (if (> (string-length str) (string-length prev))
                      str
                      prev))
                ""
                '("x" "abc" "xyz" "jk"))
    ⇒ "xyz"
    ```

    If lst1 lst2 … have different lengths, `fold`
    stops when the end of the shortest is reached; `fold-right`
    commences at the last element of the shortest. Ie. elements past the
    length of the shortest are ignored in the other lsts. At least
    one lst must be non-circular.

    `fold` should be preferred over `fold-right` if the order of
    processing doesn’t matter, or can be arranged either way, since
    `fold` is a little more efficient.

    The way `fold` builds a result from iterating is quite general,
    it can do more than other iterations like say `map` or
    `filter`. The following for example removes adjacent duplicate
    elements from a list,

    ```
    (define (delete-adjacent-duplicates lst)
      (fold-right (lambda (elem ret)
                    (if (equal? elem (first ret))
                        ret
                        (cons elem ret)))
                  (list (last lst))
                  lst))
    (delete-adjacent-duplicates '(1 2 3 3 4 4 4 5))
    ⇒ (1 2 3 4 5)
    ```

    Clearly the same sort of thing can be done with a `for-each` and
    a variable in which to build the result, but a self-contained
    proc can be re-used in multiple contexts, where a
    `for-each` would have to be written out each time.

Scheme Procedure: **pair-fold** proc init lst1 lst2 … [¶](#index-pair_002dfold)

Scheme Procedure: **pair-fold-right** proc init lst1 lst2 … [¶](#index-pair_002dfold_002dright)
:   The same as `fold` and `fold-right`, but apply proc to
    the pairs of the lists instead of the list elements.

Scheme Procedure: **reduce** proc default lst [¶](#index-reduce)

Scheme Procedure: **reduce-right** proc default lst [¶](#index-reduce_002dright)
:   `reduce` is a variant of `fold`, where the first call to
    proc is on two elements from lst, rather than one element
    and a given initial value.

    If lst is empty, `reduce` returns default (this is
    the only use for default). If lst has just one element
    then that’s the return value. Otherwise proc is called on the
    elements of lst.

    Each proc call is `(proc elem previous)`,
    where elem is from lst (the second and subsequent elements
    of lst), and previous is the return from the previous call
    to proc. The first element of lst is the previous
    for the first call to proc.

    For example, the following adds a list of numbers, the calls made to
    `+` are shown. (Of course `+` accepts multiple arguments
    and can add a list directly, with `apply`.)

    ```
    (reduce + 0 '(5 6 7)) ⇒ 18

    (+ 6 5)  ⇒ 11
    (+ 7 11) ⇒ 18
    ```

    `reduce` can be used instead of `fold` where the init
    value is an “identity”, meaning a value which under proc
    doesn’t change the result, in this case 0 is an identity since
    `(+ 5 0)` is just 5. `reduce` avoids that unnecessary call.

    `reduce-right` is a similar variation on `fold-right`,
    working from the end (ie. the right) of lst. The last element
    of lst is the previous for the first call to proc,
    and the elem values go from the second last.

    `reduce` should be preferred over `reduce-right` if the
    order of processing doesn’t matter, or can be arranged either way,
    since `reduce` is a little more efficient.

Scheme Procedure: **unfold** p f g seed [tail-gen] [¶](#index-unfold)
:   `unfold` is defined as follows:

    ```
    (unfold p f g seed) =
       (if (p seed) (tail-gen seed)
           (cons (f seed)
                 (unfold p f g (g seed))))
    ```

    p
    :   Determines when to stop unfolding.

    f
    :   Maps each seed value to the corresponding list element.

    g
    :   Maps each seed value to next seed value.

    seed
    :   The state value for the unfold.

    tail-gen
    :   Creates the tail of the list; defaults to `(lambda (x) '())`.

    g produces a series of seed values, which are mapped to list
    elements by f. These elements are put into a list in
    left-to-right order, and p tells when to stop unfolding.

Scheme Procedure: **unfold-right** p f g seed [tail] [¶](#index-unfold_002dright)
:   Construct a list with the following loop.

    ```
    (let lp ((seed seed) (lis tail))
       (if (p seed) lis
           (lp (g seed)
               (cons (f seed) lis))))
    ```

    p
    :   Determines when to stop unfolding.

    f
    :   Maps each seed value to the corresponding list element.

    g
    :   Maps each seed value to next seed value.

    seed
    :   The state value for the unfold.

    tail
    :   The tail of the list; defaults to `'()`.

Scheme Procedure: **map** f lst1 lst2 … [¶](#index-map-1)
:   Map the procedure over the list(s) lst1, lst2, … and
    return a list containing the results of the procedure applications.
    This procedure is extended with respect to R5RS, because the argument
    lists may have different lengths. The result list will have the same
    length as the shortest argument lists. The order in which f
    will be applied to the list element(s) is not specified.

Scheme Procedure: **for-each** f lst1 lst2 … [¶](#index-for_002deach-1)
:   Apply the procedure f to each pair of corresponding elements of
    the list(s) lst1, lst2, …. The return value is not
    specified. This procedure is extended with respect to R5RS, because
    the argument lists may have different lengths. The shortest argument
    list determines the number of times f is called. f will
    be applied to the list elements in left-to-right order.

Scheme Procedure: **append-map** f lst1 lst2 … [¶](#index-append_002dmap)

Scheme Procedure: **append-map!** f lst1 lst2 … [¶](#index-append_002dmap_0021)
:   Equivalent to

    ```
    (apply append (map f clist1 clist2 ...))
    ```

    and

    ```
    (apply append! (map f clist1 clist2 ...))
    ```

    Map f over the elements of the lists, just as in the `map`
    function. However, the results of the applications are appended
    together to make the final result. `append-map` uses
    `append` to append the results together; `append-map!` uses
    `append!`.

    The dynamic order in which the various applications of f are
    made is not specified.

Scheme Procedure: **map!** f lst1 lst2 … [¶](#index-map_0021)
:   Linear-update variant of `map` – `map!` is allowed, but not
    required, to alter the cons cells of lst1 to construct the
    result list.

    The dynamic order in which the various applications of f are
    made is not specified. In the n-ary case, lst2, lst3,
    … must have at least as many elements as lst1.

Scheme Procedure: **pair-for-each** f lst1 lst2 … [¶](#index-pair_002dfor_002deach)
:   Like `for-each`, but applies the procedure f to the pairs
    from which the argument lists are constructed, instead of the list
    elements. The return value is not specified.

Scheme Procedure: **filter-map** f lst1 lst2 … [¶](#index-filter_002dmap)
:   Like `map`, but only results from the applications of f
    which are true are saved in the result list.

---

Next: [Searching](#SRFI_002d1-Searching), Previous: [Fold, Unfold & Map](#SRFI_002d1-Fold-and-Map), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.6 Filtering and Partitioning [¶](#Filtering-and-Partitioning)

Filtering means to collect all elements from a list which satisfy a
specific condition. Partitioning a list means to make two groups of
list elements, one which contains the elements satisfying a condition,
and the other for the elements which don’t.

The `filter` and `filter!` functions are implemented in the
Guile core, See [List Modification](#List-Modification).

Scheme Procedure: **partition** pred lst [¶](#index-partition)

Scheme Procedure: **partition!** pred lst [¶](#index-partition_0021)
:   Split lst into those elements which do and don’t satisfy the
    predicate pred.

    The return is two values (see [Returning and Accepting Multiple Values](#Multiple-Values)), the first being a
    list of all elements from lst which satisfy pred, the
    second a list of those which do not.

    The elements in the result lists are in the same order as in lst
    but the order in which the calls `(pred elem)` are made on
    the list elements is unspecified.

    `partition` does not change lst, but one of the returned
    lists may share a tail with it. `partition!` may modify
    lst to construct its return.

Scheme Procedure: **remove** pred lst [¶](#index-remove)

Scheme Procedure: **remove!** pred lst [¶](#index-remove_0021)
:   Return a list containing all elements from lst which do not
    satisfy the predicate pred. The elements in the result list
    have the same order as in lst. The order in which pred is
    applied to the list elements is not specified.

    `remove!` is allowed, but not required to modify the structure of
    the input list.

---

Next: [Deleting](#SRFI_002d1-Deleting), Previous: [Filtering and Partitioning](#SRFI_002d1-Filtering-and-Partitioning), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.7 Searching [¶](#Searching)

The procedures for searching elements in lists either accept a
predicate or a comparison object for determining which elements are to
be searched.

Scheme Procedure: **find** pred lst [¶](#index-find)
:   Return the first element of lst that satisfies the predicate
    pred and `#f` if no such element is found.

Scheme Procedure: **find-tail** pred lst [¶](#index-find_002dtail)
:   Return the first pair of lst whose CAR satisfies the
    predicate pred and `#f` if no such element is found.

Scheme Procedure: **take-while** pred lst [¶](#index-take_002dwhile)

Scheme Procedure: **take-while!** pred lst [¶](#index-take_002dwhile_0021)
:   Return the longest initial prefix of lst whose elements all
    satisfy the predicate pred.

    `take-while!` is allowed, but not required to modify the input
    list while producing the result.

Scheme Procedure: **drop-while** pred lst [¶](#index-drop_002dwhile)
:   Drop the longest initial prefix of lst whose elements all
    satisfy the predicate pred.

Scheme Procedure: **span** pred lst [¶](#index-span)

Scheme Procedure: **span!** pred lst [¶](#index-span_0021)

Scheme Procedure: **break** pred lst [¶](#index-break-2)

Scheme Procedure: **break!** pred lst [¶](#index-break_0021)
:   `span` splits the list lst into the longest initial prefix
    whose elements all satisfy the predicate pred, and the remaining
    tail. `break` inverts the sense of the predicate.

    `span!` and `break!` are allowed, but not required to modify
    the structure of the input list lst in order to produce the
    result.

    Note that the name `break` conflicts with the `break`
    binding established by `while` (see [Iteration mechanisms](#while-do)). Applications
    wanting to use `break` from within a `while` loop will need
    to make a new define under a different name.

Scheme Procedure: **any** pred lst1 lst2 … [¶](#index-any)
:   Test whether any set of elements from lst1 lst2 …
    satisfies pred. If so, the return value is the return value from
    the successful pred call, or if not, the return value is
    `#f`.

    If there are n list arguments, then pred must be a predicate
    taking n arguments. Each pred call is `(pred
    elem1 elem2 … )` taking an element from each
    lst. The calls are made successively for the first, second, etc.
    elements of the lists, stopping when pred returns non-`#f`,
    or when the end of the shortest list is reached.

    The pred call on the last set of elements (i.e., when the end of
    the shortest list has been reached), if that point is reached, is a
    tail call.

Scheme Procedure: **every** pred lst1 lst2 … [¶](#index-every)
:   Test whether every set of elements from lst1 lst2 …
    satisfies pred. If so, the return value is the return from the
    final pred call, or if not, the return value is `#f`.

    If there are n list arguments, then pred must be a predicate
    taking n arguments. Each pred call is `(pred
    elem1 elem2 …)` taking an element from each
    lst. The calls are made successively for the first, second, etc.
    elements of the lists, stopping if pred returns `#f`, or when
    the end of any of the lists is reached.

    The pred call on the last set of elements (i.e., when the end of
    the shortest list has been reached) is a tail call.

    If one of lst1 lst2 …is empty then no calls to
    pred are made, and the return value is `#t`.

Scheme Procedure: **list-index** pred lst1 lst2 … [¶](#index-list_002dindex)
:   Return the index of the first set of elements, one from each of
    lst1 lst2 …, which satisfies pred.

    pred is called as `(elem1 elem2 …)`.
    Searching stops when the end of the shortest lst is reached.
    The return index starts from 0 for the first set of elements. If no
    set of elements pass, then the return value is `#f`.

    ```
    (list-index odd? '(2 4 6 9))      ⇒ 3
    (list-index = '(1 2 3) '(3 1 2))  ⇒ #f
    ```

Scheme Procedure: **member** x lst [=] [¶](#index-member-1)
:   Return the first sublist of lst whose CAR is equal to
    x. If x does not appear in lst, return `#f`.

    Equality is determined by `equal?`, or by the equality predicate
    = if given. = is called `(= x elem)`,
    ie. with the given x first, so for example to find the first
    element greater than 5,

    ```
    (member 5 '(3 5 1 7 2 9) <) ⇒ (7 2 9)
    ```

    This version of `member` extends the core `member`
    (see [List Searching](#List-Searching)) by accepting an equality predicate.

---

Next: [Association Lists](#SRFI_002d1-Association-Lists), Previous: [Searching](#SRFI_002d1-Searching), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.8 Deleting [¶](#Deleting)

Scheme Procedure: **delete** x lst [=] [¶](#index-delete-1)

Scheme Procedure: **delete!** x lst [=] [¶](#index-delete_0021-1)
:   Return a list containing the elements of lst but with those
    equal to x deleted. The returned elements will be in the same
    order as they were in lst.

    Equality is determined by the = predicate, or `equal?` if
    not given. An equality call is made just once for each element, but
    the order in which the calls are made on the elements is unspecified.

    The equality calls are always `(= x elem)`, ie. the given x
    is first. This means for instance elements greater than 5 can be
    deleted with `(delete 5 lst <)`.

    `delete` does not modify lst, but the return might share a
    common tail with lst. `delete!` may modify the structure
    of lst to construct its return.

    These functions extend the core `delete` and `delete!`
    (see [List Modification](#List-Modification)) in accepting an equality predicate. See
    also `lset-difference` (see [Set Operations on Lists](#SRFI_002d1-Set-Operations)) for
    deleting multiple elements from a list.

Scheme Procedure: **delete-duplicates** lst [=] [¶](#index-delete_002dduplicates)

Scheme Procedure: **delete-duplicates!** lst [=] [¶](#index-delete_002dduplicates_0021)
:   Return a list containing the elements of lst but without
    duplicates.

    When elements are equal, only the first in lst is retained.
    Equal elements can be anywhere in lst, they don’t have to be
    adjacent. The returned list will have the retained elements in the
    same order as they were in lst.

    Equality is determined by the = predicate, or `equal?` if
    not given. Calls `(= x y)` are made with element x being
    before y in lst. A call is made at most once for each
    combination, but the sequence of the calls across the elements is
    unspecified.

    `delete-duplicates` does not modify lst, but the return
    might share a common tail with lst. `delete-duplicates!`
    may modify the structure of lst to construct its return.

    In the worst case, this is an *O(N^2)* algorithm because it must
    check each element against all those preceding it. For long lists it
    is more efficient to sort and then compare only adjacent elements.

---

Next: [Set Operations on Lists](#SRFI_002d1-Set-Operations), Previous: [Deleting](#SRFI_002d1-Deleting), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.9 Association Lists [¶](#Association-Lists-2)

Association lists are described in detail in section [Association Lists](#Association-Lists). The present section only documents the additional procedures
for dealing with association lists defined by SRFI-1.

Scheme Procedure: **assoc** key alist [=] [¶](#index-assoc-1)
:   Return the pair from alist which matches key. This
    extends the core `assoc` (see [Retrieving Alist Entries](#Retrieving-Alist-Entries)) by
    taking an optional = comparison procedure.

    The default comparison is `equal?`. If an = parameter is
    given it’s called `(= key alistcar)`, i.e. the
    given target key is the first argument, and a `car` from
    alist is second.

    For example a case-insensitive string lookup,

    ```
    (assoc "yy" '(("XX" . 1) ("YY" . 2)) string-ci=?)
    ⇒ ("YY" . 2)
    ```

Scheme Procedure: **alist-cons** key datum alist [¶](#index-alist_002dcons)
:   Cons a new association key and datum onto alist and
    return the result. This is equivalent to

    ```
    (cons (cons key datum) alist)
    ```

    `acons` (see [Adding or Setting Alist Entries](#Adding-or-Setting-Alist-Entries)) in the Guile
    core does the same thing.

Scheme Procedure: **alist-copy** alist [¶](#index-alist_002dcopy)
:   Return a newly allocated copy of alist, that means that the
    spine of the list as well as the pairs are copied.

Scheme Procedure: **alist-delete** key alist [=] [¶](#index-alist_002ddelete)

Scheme Procedure: **alist-delete!** key alist [=] [¶](#index-alist_002ddelete_0021)
:   Return a list containing the elements of alist but with those
    elements whose keys are equal to key deleted. The returned
    elements will be in the same order as they were in alist.

    Equality is determined by the = predicate, or `equal?` if
    not given. The order in which elements are tested is unspecified, but
    each equality call is made `(= key alistkey)`, i.e. the given
    key parameter is first and the key from alist second.
    This means for instance all associations with a key greater than 5 can
    be removed with `(alist-delete 5 alist <)`.

    `alist-delete` does not modify alist, but the return might
    share a common tail with alist. `alist-delete!` may modify
    the list structure of alist to construct its return.

---

Previous: [Association Lists](#SRFI_002d1-Association-Lists), Up: [SRFI-1 - List library](#SRFI_002d1)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.3.10 Set Operations on Lists [¶](#Set-Operations-on-Lists)

Lists can be used to represent sets of objects. The procedures in
this section operate on such lists as sets.

Note that lists are not an efficient way to implement large sets. The
procedures here typically take time *mxn* when
operating on m and n element lists. Other data structures
like trees, bitsets (see [Bit Vectors](#Bit-Vectors)) or hash tables (see [Hash Tables](#Hash-Tables)) are faster.

All these procedures take an equality predicate as the first argument.
This predicate is used for testing the objects in the list sets for
sameness. This predicate must be consistent with `eq?`
(see [Equality](#Equality)) in the sense that if two list elements are
`eq?` then they must also be equal under the predicate. This
simply means a given object must be equal to itself.

Scheme Procedure: **lset<=** = list … [¶](#index-lset_003c_003d)
:   Return `#t` if each list is a subset of the one following it.
    I.e., list1 is a subset of list2, list2 is a subset of
    list3, etc., for as many lists as given. If only one list or no
    lists are given, the return value is `#t`.

    A list x is a subset of y if each element of x is
    equal to some element in y. Elements are compared using the
    given = procedure, called as `(= xelem yelem)`.

    ```
    (lset<= eq?)                      ⇒ #t
    (lset<= eqv? '(1 2 3) '(1))       ⇒ #f
    (lset<= eqv? '(1 3 2) '(4 3 1 2)) ⇒ #t
    ```

Scheme Procedure: **lset=** = list … [¶](#index-lset_003d)
:   Return `#t` if all argument lists are set-equal. list1 is
    compared to list2, list2 to list3, etc., for as many
    lists as given. If only one list or no lists are given, the return
    value is `#t`.

    Two lists x and y are set-equal if each element of x
    is equal to some element of y and conversely each element of
    y is equal to some element of x. The order of the
    elements in the lists doesn’t matter. Element equality is determined
    with the given = procedure, called as `(= xelem
    yelem)`, but exactly which calls are made is unspecified.

    ```
    (lset= eq?)                      ⇒ #t
    (lset= eqv? '(1 2 3) '(3 2 1))   ⇒ #t
    (lset= string-ci=? '("a" "A" "b") '("B" "b" "a")) ⇒ #t
    ```

Scheme Procedure: **lset-adjoin** = list elem … [¶](#index-lset_002dadjoin)
:   Add to list any of the given elems not already in the list.
    elems are `cons`ed onto the start of list (so the
    return value shares a common tail with list), but the order that
    the elems are added is unspecified.

    The given = procedure is used for comparing elements, called as
    `(= listelem elem)`, i.e., the second argument is one of
    the given elem parameters.

    ```
    (lset-adjoin eqv? '(1 2 3) 4 1 5) ⇒ (5 4 1 2 3)
    ```

Scheme Procedure: **lset-union** = list … [¶](#index-lset_002dunion)

Scheme Procedure: **lset-union!** = list … [¶](#index-lset_002dunion_0021)
:   Return the union of the argument list sets. The result is built by
    taking the union of list1 and list2, then the union of
    that with list3, etc., for as many lists as given. For one list
    argument that list itself is the result, for no list arguments the
    result is the empty list.

    The union of two lists x and y is formed as follows. If
    x is empty then the result is y. Otherwise start with
    x as the result and consider each y element (from first to
    last). A y element not equal to something already in the result
    is `cons`ed onto the result.

    The given = procedure is used for comparing elements, called as
    `(= relem yelem)`. The first argument is from the result
    accumulated so far, and the second is from the list being union-ed in.
    But exactly which calls are made is otherwise unspecified.

    Notice that duplicate elements in list1 (or the first non-empty
    list) are preserved, but that repeated elements in subsequent lists
    are only added once.

    ```
    (lset-union eqv?)                          ⇒ ()
    (lset-union eqv? '(1 2 3))                 ⇒ (1 2 3)
    (lset-union eqv? '(1 2 1 3) '(2 4 5) '(5)) ⇒ (5 4 1 2 1 3)
    ```

    `lset-union` doesn’t change the given lists but the result may
    share a tail with the first non-empty list. `lset-union!` can
    modify all of the given lists to form the result.

Scheme Procedure: **lset-intersection** = list1 list2 … [¶](#index-lset_002dintersection)

Scheme Procedure: **lset-intersection!** = list1 list2 … [¶](#index-lset_002dintersection_0021)
:   Return the intersection of list1 with the other argument lists,
    meaning those elements of list1 which are also in all of
    list2 etc. For one list argument, just that list is returned.

    The test for an element of list1 to be in the return is simply
    that it’s equal to some element in each of list2 etc. Notice
    this means an element appearing twice in list1 but only once in
    each of list2 etc will go into the return twice. The return has
    its elements in the same order as they were in list1.

    The given = procedure is used for comparing elements, called as
    `(= elem1 elemN)`. The first argument is from list1
    and the second is from one of the subsequent lists. But exactly which
    calls are made and in what order is unspecified.

    ```
    (lset-intersection eqv? '(x y))                        ⇒ (x y)
    (lset-intersection eqv? '(1 2 3) '(4 3 2))             ⇒ (2 3)
    (lset-intersection eqv? '(1 1 2 2) '(1 2) '(2 1) '(2)) ⇒ (2 2)
    ```

    The return from `lset-intersection` may share a tail with
    list1. `lset-intersection!` may modify list1 to form
    its result.

Scheme Procedure: **lset-difference** = list1 list2 … [¶](#index-lset_002ddifference)

Scheme Procedure: **lset-difference!** = list1 list2 … [¶](#index-lset_002ddifference_0021)
:   Return list1 with any elements in list2, list3 etc
    removed (ie. subtracted). For one list argument, just that list is
    returned.

    The given = procedure is used for comparing elements, called as
    `(= elem1 elemN)`. The first argument is from list1
    and the second from one of the subsequent lists. But exactly which
    calls are made and in what order is unspecified.

    ```
    (lset-difference eqv? '(x y))             ⇒ (x y)
    (lset-difference eqv? '(1 2 3) '(3 1))    ⇒ (2)
    (lset-difference eqv? '(1 2 3) '(3) '(2)) ⇒ (1)
    ```

    The return from `lset-difference` may share a tail with
    list1. `lset-difference!` may modify list1 to form
    its result.

Scheme Procedure: **lset-diff+intersection** = list1 list2 … [¶](#index-lset_002ddiff_002bintersection)

Scheme Procedure: **lset-diff+intersection!** = list1 list2 … [¶](#index-lset_002ddiff_002bintersection_0021)
:   Return two values (see [Returning and Accepting Multiple Values](#Multiple-Values)), the difference and
    intersection of the argument lists as per `lset-difference` and
    `lset-intersection` above.

    For two list arguments this partitions list1 into those elements
    of list1 which are in list2 and not in list2. (But
    for more than two arguments there can be elements of list1 which
    are neither part of the difference nor the intersection.)

    One of the return values from `lset-diff+intersection` may share
    a tail with list1. `lset-diff+intersection!` may modify
    list1 to form its results.

Scheme Procedure: **lset-xor** = list … [¶](#index-lset_002dxor)

Scheme Procedure: **lset-xor!** = list … [¶](#index-lset_002dxor_0021)
:   Return an XOR of the argument lists. For two lists this means those
    elements which are in exactly one of the lists. For more than two
    lists it means those elements which appear in an odd number of the
    lists.

    To be precise, the XOR of two lists x and y is formed by
    taking those elements of x not equal to any element of y,
    plus those elements of y not equal to any element of x.
    Equality is determined with the given = procedure, called as
    `(= e1 e2)`. One argument is from x and the other
    from y, but which way around is unspecified. Exactly which
    calls are made is also unspecified, as is the order of the elements in
    the result.

    ```
    (lset-xor eqv? '(x y))             ⇒ (x y)
    (lset-xor eqv? '(1 2 3) '(4 3 2))  ⇒ (4 1)
    ```

    The return from `lset-xor` may share a tail with one of the list
    arguments. `lset-xor!` may modify list1 to form its
    result.

---

Next: [SRFI-4 - Homogeneous numeric vector datatypes](#SRFI_002d4), Previous: [SRFI-1 - List library](#SRFI_002d1), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.4 SRFI-2 - and-let\* [¶](#SRFI_002d2-_002d-and_002dlet_002a)

The following syntax can be obtained with

```
(use-modules (srfi srfi-2))
```

or alternatively

```
(use-modules (ice-9 and-let-star))
```

library syntax: **and-let\*** (clause …) body … [¶](#index-and_002dlet_002a)
:   A combination of `and` and `let*`.

    Each clause is evaluated in turn, and if `#f` is obtained
    then evaluation stops and `#f` is returned. If all are
    non-`#f` then body is evaluated and the last form gives the
    return value, or if body is empty then the result is `#t`.
    Each clause should be one of the following,

    `(symbol expr)`
    :   Evaluate expr, check for `#f`, and bind it to symbol.
        Like `let*`, that binding is available to subsequent clauses.

    `(expr)`
    :   Evaluate expr and check for `#f`.

    `symbol`
    :   Get the value bound to symbol and check for `#f`.

    Notice that `(expr)` has an “extra” pair of parentheses, for
    instance `((eq? x y))`. One way to remember this is to imagine
    the `symbol` in `(symbol expr)` is omitted.

    `and-let*` is good for calculations where a `#f` value means
    termination, but where a non-`#f` value is going to be needed in
    subsequent expressions.

    The following illustrates this, it returns text between brackets
    ‘`[...]`’ in a string, or `#f` if there are no such brackets
    (ie. either `string-index` gives `#f`).

    ```
    (define (extract-brackets str)
      (and-let* ((start (string-index str #\[))
                 (end   (string-index str #\] start)))
        (substring str (1+ start) end)))
    ```

    The following shows plain variables and expressions tested too.
    `diagnostic-levels` is taken to be an alist associating a
    diagnostic type with a level. `str` is printed only if the type
    is known and its level is high enough.

    ```
    (define (show-diagnostic type str)
      (and-let* (want-diagnostics
                 (level (assq-ref diagnostic-levels type))
                 ((>= level current-diagnostic-level)))
        (display str)))
    ```

    The advantage of `and-let*` is that an extended sequence of
    expressions and tests doesn’t require lots of nesting as would arise
    from separate `and` and `let*`, or from `cond` with
    `=>`.

---

Next: [SRFI-6 - Basic String Ports](#SRFI_002d6), Previous: [SRFI-2 - and-let\*](#SRFI_002d2), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.5 SRFI-4 - Homogeneous numeric vector datatypes [¶](#SRFI_002d4-_002d-Homogeneous-numeric-vector-datatypes)

SRFI-4 provides an interface to uniform numeric vectors: vectors whose elements
are all of a single numeric type. Guile offers uniform numeric vectors for
signed and unsigned 8-bit, 16-bit, 32-bit, and 64-bit integers, two sizes of
floating point values, and, as an extension to SRFI-4, complex floating-point
numbers of these two sizes.

The standard SRFI-4 procedures and data types may be included via loading the
appropriate module:

```
(use-modules (srfi srfi-4))
```

This module is currently a part of the default Guile environment, but it is a
good practice to explicitly import the module. In the future, using SRFI-4
procedures without importing the SRFI-4 module will cause a deprecation message
to be printed. (Of course, one may call the C functions at any time. Would that
C had modules!)

* [SRFI-4 - Overview](#SRFI_002d4-Overview)
* [SRFI-4 - API](#SRFI_002d4-API)
* [SRFI-4 - Relation to bytevectors](#SRFI_002d4-and-Bytevectors)
* [SRFI-4 - Guile extensions](#SRFI_002d4-Extensions)

---

Next: [SRFI-4 - API](#SRFI_002d4-API), Up: [SRFI-4 - Homogeneous numeric vector datatypes](#SRFI_002d4)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.5.1 SRFI-4 - Overview [¶](#SRFI_002d4-_002d-Overview)

Uniform numeric vectors can be useful since they consume less memory
than the non-uniform, general vectors. Also, since the types they can
store correspond directly to C types, it is easier to work with them
efficiently on a low level. Consider image processing as an example,
where you want to apply a filter to some image. While you could store
the pixels of an image in a general vector and write a general
convolution function, things are much more efficient with uniform
vectors: the convolution function knows that all pixels are unsigned
8-bit values (say), and can use a very tight inner loop.

This is implemented in Scheme by having the compiler notice calls to the SRFI-4
accessors, and inline them to appropriate compiled code. From C you have access
to the raw array; functions for efficiently working with uniform numeric vectors
from C are listed at the end of this section.

Uniform numeric vectors are the special case of one dimensional uniform
numeric arrays.

There are 12 standard kinds of uniform numeric vectors, and they all have their
own complement of constructors, accessors, and so on. Procedures that operate on
a specific kind of uniform numeric vector have a “tag” in their name,
indicating the element type.

`u8`
:   unsigned 8-bit integers

`s8`
:   signed 8-bit integers

`u16`
:   unsigned 16-bit integers

`s16`
:   signed 16-bit integers

`u32`
:   unsigned 32-bit integers

`s32`
:   signed 32-bit integers

`u64`
:   unsigned 64-bit integers

`s64`
:   signed 64-bit integers

`f32`
:   the C type `float`

`f64`
:   the C type `double`

In addition, Guile supports uniform arrays of complex numbers, with the
nonstandard tags:

`c32`
:   complex numbers in rectangular form with the real and imaginary part
    being a `float`

`c64`
:   complex numbers in rectangular form with the real and imaginary part
    being a `double`

The external representation (ie. read syntax) for these vectors is
similar to normal Scheme vectors, but with an additional tag from the
tables above indicating the vector’s type. For example,

```
#u16(1 2 3)
#f64(3.1415 2.71)
```

Note that the read syntax for floating-point here conflicts with
`#f` for false. In Standard Scheme one can write `(1 #f3)`
for a three element list `(1 #f 3)`, but for Guile `(1 #f3)`
is invalid. `(1 #f 3)` is almost certainly what one should write
anyway to make the intention clear, so this is rarely a problem.

---

Next: [SRFI-4 - Relation to bytevectors](#SRFI_002d4-and-Bytevectors), Previous: [SRFI-4 - Overview](#SRFI_002d4-Overview), Up: [SRFI-4 - Homogeneous numeric vector datatypes](#SRFI_002d4)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.5.2 SRFI-4 - API [¶](#SRFI_002d4-_002d-API)

Note that the `c32` and `c64` functions are only available from
`(srfi srfi-4 gnu)`.

Scheme Procedure: **u8vector?** obj [¶](#index-u8vector_003f)

Scheme Procedure: **s8vector?** obj [¶](#index-s8vector_003f)

Scheme Procedure: **u16vector?** obj [¶](#index-u16vector_003f)

Scheme Procedure: **s16vector?** obj [¶](#index-s16vector_003f)

Scheme Procedure: **u32vector?** obj [¶](#index-u32vector_003f)

Scheme Procedure: **s32vector?** obj [¶](#index-s32vector_003f)

Scheme Procedure: **u64vector?** obj [¶](#index-u64vector_003f)

Scheme Procedure: **s64vector?** obj [¶](#index-s64vector_003f)

Scheme Procedure: **f32vector?** obj [¶](#index-f32vector_003f)

Scheme Procedure: **f64vector?** obj [¶](#index-f64vector_003f)

Scheme Procedure: **c32vector?** obj [¶](#index-c32vector_003f)

Scheme Procedure: **c64vector?** obj [¶](#index-c64vector_003f)

C Function: **scm\_u8vector\_p** (obj) [¶](#index-scm_005fu8vector_005fp)

C Function: **scm\_s8vector\_p** (obj) [¶](#index-scm_005fs8vector_005fp)

C Function: **scm\_u16vector\_p** (obj) [¶](#index-scm_005fu16vector_005fp)

C Function: **scm\_s16vector\_p** (obj) [¶](#index-scm_005fs16vector_005fp)

C Function: **scm\_u32vector\_p** (obj) [¶](#index-scm_005fu32vector_005fp)

C Function: **scm\_s32vector\_p** (obj) [¶](#index-scm_005fs32vector_005fp)

C Function: **scm\_u64vector\_p** (obj) [¶](#index-scm_005fu64vector_005fp)

C Function: **scm\_s64vector\_p** (obj) [¶](#index-scm_005fs64vector_005fp)

C Function: **scm\_f32vector\_p** (obj) [¶](#index-scm_005ff32vector_005fp)

C Function: **scm\_f64vector\_p** (obj) [¶](#index-scm_005ff64vector_005fp)

C Function: **scm\_c32vector\_p** (obj) [¶](#index-scm_005fc32vector_005fp)

C Function: **scm\_c64vector\_p** (obj) [¶](#index-scm_005fc64vector_005fp)
:   Return `#t` if obj is a homogeneous numeric vector of the
    indicated type.

Scheme Procedure: **make-u8vector** n [value] [¶](#index-make_002du8vector)

Scheme Procedure: **make-s8vector** n [value] [¶](#index-make_002ds8vector)

Scheme Procedure: **make-u16vector** n [value] [¶](#index-make_002du16vector)

Scheme Procedure: **make-s16vector** n [value] [¶](#index-make_002ds16vector)

Scheme Procedure: **make-u32vector** n [value] [¶](#index-make_002du32vector)

Scheme Procedure: **make-s32vector** n [value] [¶](#index-make_002ds32vector)

Scheme Procedure: **make-u64vector** n [value] [¶](#index-make_002du64vector)

Scheme Procedure: **make-s64vector** n [value] [¶](#index-make_002ds64vector)

Scheme Procedure: **make-f32vector** n [value] [¶](#index-make_002df32vector)

Scheme Procedure: **make-f64vector** n [value] [¶](#index-make_002df64vector)

Scheme Procedure: **make-c32vector** n [value] [¶](#index-make_002dc32vector)

Scheme Procedure: **make-c64vector** n [value] [¶](#index-make_002dc64vector)

C Function: **scm\_make\_u8vector** (n, value) [¶](#index-scm_005fmake_005fu8vector)

C Function: **scm\_make\_s8vector** (n, value) [¶](#index-scm_005fmake_005fs8vector)

C Function: **scm\_make\_u16vector** (n, value) [¶](#index-scm_005fmake_005fu16vector)

C Function: **scm\_make\_s16vector** (n, value) [¶](#index-scm_005fmake_005fs16vector)

C Function: **scm\_make\_u32vector** (n, value) [¶](#index-scm_005fmake_005fu32vector)

C Function: **scm\_make\_s32vector** (n, value) [¶](#index-scm_005fmake_005fs32vector)

C Function: **scm\_make\_u64vector** (n, value) [¶](#index-scm_005fmake_005fu64vector)

C Function: **scm\_make\_s64vector** (n, value) [¶](#index-scm_005fmake_005fs64vector)

C Function: **scm\_make\_f32vector** (n, value) [¶](#index-scm_005fmake_005ff32vector)

C Function: **scm\_make\_f64vector** (n, value) [¶](#index-scm_005fmake_005ff64vector)

C Function: **scm\_make\_c32vector** (n, value) [¶](#index-scm_005fmake_005fc32vector)

C Function: **scm\_make\_c64vector** (n, value) [¶](#index-scm_005fmake_005fc64vector)
:   Return a newly allocated homogeneous numeric vector holding n
    elements of the indicated type. If value is given, the vector
    is initialized with that value, otherwise the contents are
    unspecified.

Scheme Procedure: **u8vector** value … [¶](#index-u8vector)

Scheme Procedure: **s8vector** value … [¶](#index-s8vector)

Scheme Procedure: **u16vector** value … [¶](#index-u16vector)

Scheme Procedure: **s16vector** value … [¶](#index-s16vector)

Scheme Procedure: **u32vector** value … [¶](#index-u32vector)

Scheme Procedure: **s32vector** value … [¶](#index-s32vector)

Scheme Procedure: **u64vector** value … [¶](#index-u64vector)

Scheme Procedure: **s64vector** value … [¶](#index-s64vector)

Scheme Procedure: **f32vector** value … [¶](#index-f32vector)

Scheme Procedure: **f64vector** value … [¶](#index-f64vector)

Scheme Procedure: **c32vector** value … [¶](#index-c32vector)

Scheme Procedure: **c64vector** value … [¶](#index-c64vector)

C Function: **scm\_u8vector** (values) [¶](#index-scm_005fu8vector)

C Function: **scm\_s8vector** (values) [¶](#index-scm_005fs8vector)

C Function: **scm\_u16vector** (values) [¶](#index-scm_005fu16vector)

C Function: **scm\_s16vector** (values) [¶](#index-scm_005fs16vector)

C Function: **scm\_u32vector** (values) [¶](#index-scm_005fu32vector)

C Function: **scm\_s32vector** (values) [¶](#index-scm_005fs32vector)

C Function: **scm\_u64vector** (values) [¶](#index-scm_005fu64vector)

C Function: **scm\_s64vector** (values) [¶](#index-scm_005fs64vector)

C Function: **scm\_f32vector** (values) [¶](#index-scm_005ff32vector)

C Function: **scm\_f64vector** (values) [¶](#index-scm_005ff64vector)

C Function: **scm\_c32vector** (values) [¶](#index-scm_005fc32vector)

C Function: **scm\_c64vector** (values) [¶](#index-scm_005fc64vector)
:   Return a newly allocated homogeneous numeric vector of the indicated
    type, holding the given parameter values. The vector length is
    the number of parameters given.

Scheme Procedure: **u8vector-length** vec [¶](#index-u8vector_002dlength)

Scheme Procedure: **s8vector-length** vec [¶](#index-s8vector_002dlength)

Scheme Procedure: **u16vector-length** vec [¶](#index-u16vector_002dlength)

Scheme Procedure: **s16vector-length** vec [¶](#index-s16vector_002dlength)

Scheme Procedure: **u32vector-length** vec [¶](#index-u32vector_002dlength)

Scheme Procedure: **s32vector-length** vec [¶](#index-s32vector_002dlength)

Scheme Procedure: **u64vector-length** vec [¶](#index-u64vector_002dlength)

Scheme Procedure: **s64vector-length** vec [¶](#index-s64vector_002dlength)

Scheme Procedure: **f32vector-length** vec [¶](#index-f32vector_002dlength)

Scheme Procedure: **f64vector-length** vec [¶](#index-f64vector_002dlength)

Scheme Procedure: **c32vector-length** vec [¶](#index-c32vector_002dlength)

Scheme Procedure: **c64vector-length** vec [¶](#index-c64vector_002dlength)

C Function: **scm\_u8vector\_length** (vec) [¶](#index-scm_005fu8vector_005flength)

C Function: **scm\_s8vector\_length** (vec) [¶](#index-scm_005fs8vector_005flength)

C Function: **scm\_u16vector\_length** (vec) [¶](#index-scm_005fu16vector_005flength)

C Function: **scm\_s16vector\_length** (vec) [¶](#index-scm_005fs16vector_005flength)

C Function: **scm\_u32vector\_length** (vec) [¶](#index-scm_005fu32vector_005flength)

C Function: **scm\_s32vector\_length** (vec) [¶](#index-scm_005fs32vector_005flength)

C Function: **scm\_u64vector\_length** (vec) [¶](#index-scm_005fu64vector_005flength)

C Function: **scm\_s64vector\_length** (vec) [¶](#index-scm_005fs64vector_005flength)

C Function: **scm\_f32vector\_length** (vec) [¶](#index-scm_005ff32vector_005flength)

C Function: **scm\_f64vector\_length** (vec) [¶](#index-scm_005ff64vector_005flength)

C Function: **scm\_c32vector\_length** (vec) [¶](#index-scm_005fc32vector_005flength)

C Function: **scm\_c64vector\_length** (vec) [¶](#index-scm_005fc64vector_005flength)
:   Return the number of elements in vec.

Scheme Procedure: **u8vector-ref** vec i [¶](#index-u8vector_002dref)

Scheme Procedure: **s8vector-ref** vec i [¶](#index-s8vector_002dref)

Scheme Procedure: **u16vector-ref** vec i [¶](#index-u16vector_002dref)

Scheme Procedure: **s16vector-ref** vec i [¶](#index-s16vector_002dref)

Scheme Procedure: **u32vector-ref** vec i [¶](#index-u32vector_002dref)

Scheme Procedure: **s32vector-ref** vec i [¶](#index-s32vector_002dref)

Scheme Procedure: **u64vector-ref** vec i [¶](#index-u64vector_002dref)

Scheme Procedure: **s64vector-ref** vec i [¶](#index-s64vector_002dref)

Scheme Procedure: **f32vector-ref** vec i [¶](#index-f32vector_002dref)

Scheme Procedure: **f64vector-ref** vec i [¶](#index-f64vector_002dref)

Scheme Procedure: **c32vector-ref** vec i [¶](#index-c32vector_002dref)

Scheme Procedure: **c64vector-ref** vec i [¶](#index-c64vector_002dref)

C Function: **scm\_u8vector\_ref** (vec, i) [¶](#index-scm_005fu8vector_005fref)

C Function: **scm\_s8vector\_ref** (vec, i) [¶](#index-scm_005fs8vector_005fref)

C Function: **scm\_u16vector\_ref** (vec, i) [¶](#index-scm_005fu16vector_005fref)

C Function: **scm\_s16vector\_ref** (vec, i) [¶](#index-scm_005fs16vector_005fref)

C Function: **scm\_u32vector\_ref** (vec, i) [¶](#index-scm_005fu32vector_005fref)

C Function: **scm\_s32vector\_ref** (vec, i) [¶](#index-scm_005fs32vector_005fref)

C Function: **scm\_u64vector\_ref** (vec, i) [¶](#index-scm_005fu64vector_005fref)

C Function: **scm\_s64vector\_ref** (vec, i) [¶](#index-scm_005fs64vector_005fref)

C Function: **scm\_f32vector\_ref** (vec, i) [¶](#index-scm_005ff32vector_005fref)

C Function: **scm\_f64vector\_ref** (vec, i) [¶](#index-scm_005ff64vector_005fref)

C Function: **scm\_c32vector\_ref** (vec, i) [¶](#index-scm_005fc32vector_005fref)

C Function: **scm\_c64vector\_ref** (vec, i) [¶](#index-scm_005fc64vector_005fref)
:   Return the element at index i in vec. The first element
    in vec is index 0.

Scheme Procedure: **u8vector-set!** vec i value [¶](#index-u8vector_002dset_0021)

Scheme Procedure: **s8vector-set!** vec i value [¶](#index-s8vector_002dset_0021)

Scheme Procedure: **u16vector-set!** vec i value [¶](#index-u16vector_002dset_0021)

Scheme Procedure: **s16vector-set!** vec i value [¶](#index-s16vector_002dset_0021)

Scheme Procedure: **u32vector-set!** vec i value [¶](#index-u32vector_002dset_0021)

Scheme Procedure: **s32vector-set!** vec i value [¶](#index-s32vector_002dset_0021)

Scheme Procedure: **u64vector-set!** vec i value [¶](#index-u64vector_002dset_0021)

Scheme Procedure: **s64vector-set!** vec i value [¶](#index-s64vector_002dset_0021)

Scheme Procedure: **f32vector-set!** vec i value [¶](#index-f32vector_002dset_0021)

Scheme Procedure: **f64vector-set!** vec i value [¶](#index-f64vector_002dset_0021)

Scheme Procedure: **c32vector-set!** vec i value [¶](#index-c32vector_002dset_0021)

Scheme Procedure: **c64vector-set!** vec i value [¶](#index-c64vector_002dset_0021)

C Function: **scm\_u8vector\_set\_x** (vec, i, value) [¶](#index-scm_005fu8vector_005fset_005fx)

C Function: **scm\_s8vector\_set\_x** (vec, i, value) [¶](#index-scm_005fs8vector_005fset_005fx)

C Function: **scm\_u16vector\_set\_x** (vec, i, value) [¶](#index-scm_005fu16vector_005fset_005fx)

C Function: **scm\_s16vector\_set\_x** (vec, i, value) [¶](#index-scm_005fs16vector_005fset_005fx)

C Function: **scm\_u32vector\_set\_x** (vec, i, value) [¶](#index-scm_005fu32vector_005fset_005fx)

C Function: **scm\_s32vector\_set\_x** (vec, i, value) [¶](#index-scm_005fs32vector_005fset_005fx)

C Function: **scm\_u64vector\_set\_x** (vec, i, value) [¶](#index-scm_005fu64vector_005fset_005fx)

C Function: **scm\_s64vector\_set\_x** (vec, i, value) [¶](#index-scm_005fs64vector_005fset_005fx)

C Function: **scm\_f32vector\_set\_x** (vec, i, value) [¶](#index-scm_005ff32vector_005fset_005fx)

C Function: **scm\_f64vector\_set\_x** (vec, i, value) [¶](#index-scm_005ff64vector_005fset_005fx)

C Function: **scm\_c32vector\_set\_x** (vec, i, value) [¶](#index-scm_005fc32vector_005fset_005fx)

C Function: **scm\_c64vector\_set\_x** (vec, i, value) [¶](#index-scm_005fc64vector_005fset_005fx)
:   Set the element at index i in vec to value. The
    first element in vec is index 0. The return value is
    unspecified.

Scheme Procedure: **u8vector->list** vec [¶](#index-u8vector_002d_003elist)

Scheme Procedure: **s8vector->list** vec [¶](#index-s8vector_002d_003elist)

Scheme Procedure: **u16vector->list** vec [¶](#index-u16vector_002d_003elist)

Scheme Procedure: **s16vector->list** vec [¶](#index-s16vector_002d_003elist)

Scheme Procedure: **u32vector->list** vec [¶](#index-u32vector_002d_003elist)

Scheme Procedure: **s32vector->list** vec [¶](#index-s32vector_002d_003elist)

Scheme Procedure: **u64vector->list** vec [¶](#index-u64vector_002d_003elist)

Scheme Procedure: **s64vector->list** vec [¶](#index-s64vector_002d_003elist)

Scheme Procedure: **f32vector->list** vec [¶](#index-f32vector_002d_003elist)

Scheme Procedure: **f64vector->list** vec [¶](#index-f64vector_002d_003elist)

Scheme Procedure: **c32vector->list** vec [¶](#index-c32vector_002d_003elist)

Scheme Procedure: **c64vector->list** vec [¶](#index-c64vector_002d_003elist)

C Function: **scm\_u8vector\_to\_list** (vec) [¶](#index-scm_005fu8vector_005fto_005flist)

C Function: **scm\_s8vector\_to\_list** (vec) [¶](#index-scm_005fs8vector_005fto_005flist)

C Function: **scm\_u16vector\_to\_list** (vec) [¶](#index-scm_005fu16vector_005fto_005flist)

C Function: **scm\_s16vector\_to\_list** (vec) [¶](#index-scm_005fs16vector_005fto_005flist)

C Function: **scm\_u32vector\_to\_list** (vec) [¶](#index-scm_005fu32vector_005fto_005flist)

C Function: **scm\_s32vector\_to\_list** (vec) [¶](#index-scm_005fs32vector_005fto_005flist)

C Function: **scm\_u64vector\_to\_list** (vec) [¶](#index-scm_005fu64vector_005fto_005flist)

C Function: **scm\_s64vector\_to\_list** (vec) [¶](#index-scm_005fs64vector_005fto_005flist)

C Function: **scm\_f32vector\_to\_list** (vec) [¶](#index-scm_005ff32vector_005fto_005flist)

C Function: **scm\_f64vector\_to\_list** (vec) [¶](#index-scm_005ff64vector_005fto_005flist)

C Function: **scm\_c32vector\_to\_list** (vec) [¶](#index-scm_005fc32vector_005fto_005flist)

C Function: **scm\_c64vector\_to\_list** (vec) [¶](#index-scm_005fc64vector_005fto_005flist)
:   Return a newly allocated list holding all elements of vec.

Scheme Procedure: **list->u8vector** lst [¶](#index-list_002d_003eu8vector)

Scheme Procedure: **list->s8vector** lst [¶](#index-list_002d_003es8vector)

Scheme Procedure: **list->u16vector** lst [¶](#index-list_002d_003eu16vector)

Scheme Procedure: **list->s16vector** lst [¶](#index-list_002d_003es16vector)

Scheme Procedure: **list->u32vector** lst [¶](#index-list_002d_003eu32vector)

Scheme Procedure: **list->s32vector** lst [¶](#index-list_002d_003es32vector)

Scheme Procedure: **list->u64vector** lst [¶](#index-list_002d_003eu64vector)

Scheme Procedure: **list->s64vector** lst [¶](#index-list_002d_003es64vector)

Scheme Procedure: **list->f32vector** lst [¶](#index-list_002d_003ef32vector)

Scheme Procedure: **list->f64vector** lst [¶](#index-list_002d_003ef64vector)

Scheme Procedure: **list->c32vector** lst [¶](#index-list_002d_003ec32vector)

Scheme Procedure: **list->c64vector** lst [¶](#index-list_002d_003ec64vector)

C Function: **scm\_list\_to\_u8vector** (lst) [¶](#index-scm_005flist_005fto_005fu8vector)

C Function: **scm\_list\_to\_s8vector** (lst) [¶](#index-scm_005flist_005fto_005fs8vector)

C Function: **scm\_list\_to\_u16vector** (lst) [¶](#index-scm_005flist_005fto_005fu16vector)

C Function: **scm\_list\_to\_s16vector** (lst) [¶](#index-scm_005flist_005fto_005fs16vector)

C Function: **scm\_list\_to\_u32vector** (lst) [¶](#index-scm_005flist_005fto_005fu32vector)

C Function: **scm\_list\_to\_s32vector** (lst) [¶](#index-scm_005flist_005fto_005fs32vector)

C Function: **scm\_list\_to\_u64vector** (lst) [¶](#index-scm_005flist_005fto_005fu64vector)

C Function: **scm\_list\_to\_s64vector** (lst) [¶](#index-scm_005flist_005fto_005fs64vector)

C Function: **scm\_list\_to\_f32vector** (lst) [¶](#index-scm_005flist_005fto_005ff32vector)

C Function: **scm\_list\_to\_f64vector** (lst) [¶](#index-scm_005flist_005fto_005ff64vector)

C Function: **scm\_list\_to\_c32vector** (lst) [¶](#index-scm_005flist_005fto_005fc32vector)

C Function: **scm\_list\_to\_c64vector** (lst) [¶](#index-scm_005flist_005fto_005fc64vector)
:   Return a newly allocated homogeneous numeric vector of the indicated type,
    initialized with the elements of the list lst.

C Function: `SCM` **scm\_take\_u8vector** `(const scm_t_uint8 *data, size_t len)` [¶](#index-scm_005ftake_005fu8vector)

C Function: `SCM` **scm\_take\_s8vector** `(const scm_t_int8 *data, size_t len)` [¶](#index-scm_005ftake_005fs8vector)

C Function: `SCM` **scm\_take\_u16vector** `(const scm_t_uint16 *data, size_t len)` [¶](#index-scm_005ftake_005fu16vector)

C Function: `SCM` **scm\_take\_s16vector** `(const scm_t_int16 *data, size_t len)` [¶](#index-scm_005ftake_005fs16vector)

C Function: `SCM` **scm\_take\_u32vector** `(const scm_t_uint32 *data, size_t len)` [¶](#index-scm_005ftake_005fu32vector)

C Function: `SCM` **scm\_take\_s32vector** `(const scm_t_int32 *data, size_t len)` [¶](#index-scm_005ftake_005fs32vector)

C Function: `SCM` **scm\_take\_u64vector** `(const scm_t_uint64 *data, size_t len)` [¶](#index-scm_005ftake_005fu64vector)

C Function: `SCM` **scm\_take\_s64vector** `(const scm_t_int64 *data, size_t len)` [¶](#index-scm_005ftake_005fs64vector)

C Function: `SCM` **scm\_take\_f32vector** `(const float *data, size_t len)` [¶](#index-scm_005ftake_005ff32vector)

C Function: `SCM` **scm\_take\_f64vector** `(const double *data, size_t len)` [¶](#index-scm_005ftake_005ff64vector)

C Function: `SCM` **scm\_take\_c32vector** `(const float *data, size_t len)` [¶](#index-scm_005ftake_005fc32vector)

C Function: `SCM` **scm\_take\_c64vector** `(const double *data, size_t len)` [¶](#index-scm_005ftake_005fc64vector)
:   Return a new uniform numeric vector of the indicated type and length
    that uses the memory pointed to by data to store its elements.
    This memory will eventually be freed with `free`. The argument
    len specifies the number of elements in data, not its size
    in bytes.

    The `c32` and `c64` variants take a pointer to a C array of
    `float`s or `double`s. The real parts of the complex numbers
    are at even indices in that array, the corresponding imaginary parts are
    at the following odd index.

C Function: `const scm_t_uint8 *` **scm\_u8vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fu8vector_005felements)

C Function: `const scm_t_int8 *` **scm\_s8vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fs8vector_005felements)

C Function: `const scm_t_uint16 *` **scm\_u16vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fu16vector_005felements)

C Function: `const scm_t_int16 *` **scm\_s16vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fs16vector_005felements)

C Function: `const scm_t_uint32 *` **scm\_u32vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fu32vector_005felements)

C Function: `const scm_t_int32 *` **scm\_s32vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fs32vector_005felements)

C Function: `const scm_t_uint64 *` **scm\_u64vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fu64vector_005felements)

C Function: `const scm_t_int64 *` **scm\_s64vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fs64vector_005felements)

C Function: `const float *` **scm\_f32vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005ff32vector_005felements)

C Function: `const double *` **scm\_f64vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005ff64vector_005felements)

C Function: `const float *` **scm\_c32vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fc32vector_005felements)

C Function: `const double *` **scm\_c64vector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fc64vector_005felements)
:   Like `scm_vector_elements` (see [Vector Accessing from C](#Vector-Accessing-from-C)), but
    returns a pointer to the elements of a uniform numeric vector of the
    indicated kind.

C Function: `scm_t_uint8 *` **scm\_u8vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fu8vector_005fwritable_005felements)

C Function: `scm_t_int8 *` **scm\_s8vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fs8vector_005fwritable_005felements)

C Function: `scm_t_uint16 *` **scm\_u16vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fu16vector_005fwritable_005felements)

C Function: `scm_t_int16 *` **scm\_s16vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fs16vector_005fwritable_005felements)

C Function: `scm_t_uint32 *` **scm\_u32vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fu32vector_005fwritable_005felements)

C Function: `scm_t_int32 *` **scm\_s32vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fs32vector_005fwritable_005felements)

C Function: `scm_t_uint64 *` **scm\_u64vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fu64vector_005fwritable_005felements)

C Function: `scm_t_int64 *` **scm\_s64vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fs64vector_005fwritable_005felements)

C Function: `float *` **scm\_f32vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005ff32vector_005fwritable_005felements)

C Function: `double *` **scm\_f64vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005ff64vector_005fwritable_005felements)

C Function: `float *` **scm\_c32vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fc32vector_005fwritable_005felements)

C Function: `double *` **scm\_c64vector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fc64vector_005fwritable_005felements)
:   Like `scm_vector_writable_elements` (see [Vector Accessing from C](#Vector-Accessing-from-C)),
    but returns a pointer to the elements of a uniform numeric vector of the
    indicated kind.

---

Next: [SRFI-4 - Guile extensions](#SRFI_002d4-Extensions), Previous: [SRFI-4 - API](#SRFI_002d4-API), Up: [SRFI-4 - Homogeneous numeric vector datatypes](#SRFI_002d4)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.5.3 SRFI-4 - Relation to bytevectors [¶](#SRFI_002d4-_002d-Relation-to-bytevectors)

Guile implements SRFI-4 vectors using bytevectors (see [Bytevectors](#Bytevectors)). Often
when you have a numeric vector, you end up wanting to write its bytes somewhere,
or have access to the underlying bytes, or read in bytes from somewhere else.
Bytevectors are very good at this sort of thing. But the SRFI-4 APIs are nicer
to use when doing number-crunching, because they are addressed by element and
not by byte.

So as a compromise, Guile allows all bytevector functions to operate on numeric
vectors. They address the underlying bytes in the native endianness, as one
would expect.

Following the same reasoning, that it’s just bytes underneath, Guile also allows
uniform vectors of a given type to be accessed as if they were of any type. One
can fill a `u32vector`, and access its elements with
`u8vector-ref`. One can use `f64vector-ref` on bytevectors. It’s
all the same to Guile.
