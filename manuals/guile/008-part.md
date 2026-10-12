C Function: **scm\_memq** (x, lst) [¶](#index-scm_005fmemq)
:   Return the first sublist of lst whose car is `eq?`
    to x where the sublists of lst are the non-empty
    lists returned by `(list-tail lst k)` for
    k less than the length of lst. If x does not
    occur in lst, then `#f` (not the empty list) is
    returned.

Scheme Procedure: **memv** x lst [¶](#index-memv)

C Function: **scm\_memv** (x, lst) [¶](#index-scm_005fmemv)
:   Return the first sublist of lst whose car is `eqv?`
    to x where the sublists of lst are the non-empty
    lists returned by `(list-tail lst k)` for
    k less than the length of lst. If x does not
    occur in lst, then `#f` (not the empty list) is
    returned.

Scheme Procedure: **member** x lst [¶](#index-member)

C Function: **scm\_member** (x, lst) [¶](#index-scm_005fmember)
:   Return the first sublist of lst whose car is
    `equal?` to x where the sublists of lst are
    the non-empty lists returned by `(list-tail lst
    k)` for k less than the length of lst. If
    x does not occur in lst, then `#f` (not the
    empty list) is returned.

    See also SRFI-1 which has an extended `member` function
    ([Searching](#SRFI_002d1-Searching)).

---

Previous: [List Searching](#List-Searching), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.9.8 List Mapping [¶](#List-Mapping-1)

List processing is very convenient in Scheme because the process of
iterating over the elements of a list can be highly abstracted. The
procedures in this section are the most basic iterating procedures for
lists. They take a procedure and one or more lists as arguments, and
apply the procedure to each element of the list. They differ in their
return value.

Scheme Procedure: **map** proc arg1 arg2 … [¶](#index-map)

Scheme Procedure: **map-in-order** proc arg1 arg2 … [¶](#index-map_002din_002dorder)

C Function: **scm\_map** (proc, arg1, args) [¶](#index-scm_005fmap)
:   Apply proc to each element of the list arg1 (if only two
    arguments are given), or to the corresponding elements of the argument
    lists (if more than two arguments are given). The result(s) of the
    procedure applications are saved and returned in a list. For
    `map`, the order of procedure applications is not specified,
    `map-in-order` applies the procedure from left to right to the list
    elements.

Scheme Procedure: **for-each** proc arg1 arg2 … [¶](#index-for_002deach)
:   Like `map`, but the procedure is always applied from left to right,
    and the result(s) of the procedure applications are thrown away. The
    return value is not specified.

See also SRFI-1 which extends these functions to take lists of unequal
lengths ([Fold, Unfold & Map](#SRFI_002d1-Fold-and-Map)).

---

Next: [Bit Vectors](#Bit-Vectors), Previous: [Lists](#Lists), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.10 Vectors [¶](#Vectors-1)

Vectors are sequences of Scheme objects. Unlike lists, the length of a
vector, once the vector is created, cannot be changed. The advantage of
vectors over lists is that the time required to access one element of a vector
given its *position* (synonymous with *index*), a zero-origin number,
is constant, whereas lists have an access time linear to the position of the
accessed element in the list.

Vectors can contain any kind of Scheme object; it is even possible to
have different types of objects in the same vector. For vectors
containing vectors, you may wish to use [Arrays](#Arrays) instead.
Note, too, that vectors are a special case of one dimensional
non-uniform arrays and that array procedures operate happily on vectors.

Also see [SRFI-43 - Vector Library](#SRFI_002d43), [R6RS Support](#R6RS-Support), or [R7RS Support](#R7RS-Support), for
more comprehensive vector libraries.

* [Read Syntax for Vectors](#Vector-Syntax)
* [Dynamic Vector Creation and Validation](#Vector-Creation)
* [Accessing and Modifying Vector Contents](#Vector-Accessors)
* [Vector Accessing from C](#Vector-Accessing-from-C)
* [Uniform Numeric Vectors](#Uniform-Numeric-Vectors)

---

Next: [Dynamic Vector Creation and Validation](#Vector-Creation), Up: [Vectors](#Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.10.1 Read Syntax for Vectors [¶](#Read-Syntax-for-Vectors)

Vectors can literally be entered in source code, just like strings,
characters or some of the other data types. The read syntax for vectors
is as follows: A sharp sign (`#`), followed by an opening
parentheses, all elements of the vector in their respective read syntax,
and finally a closing parentheses. Like strings, vectors do not have to
be quoted.

The following are examples of the read syntax for vectors; where the
first vector only contains numbers and the second three different object
types: a string, a symbol and a number in hexadecimal notation.

```
#(1 2 3)
#("Hello" foo #xdeadbeef)
```

---

Next: [Accessing and Modifying Vector Contents](#Vector-Accessors), Previous: [Read Syntax for Vectors](#Vector-Syntax), Up: [Vectors](#Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.10.2 Dynamic Vector Creation and Validation [¶](#Dynamic-Vector-Creation-and-Validation)

Instead of creating a vector implicitly by using the read syntax just
described, you can create a vector dynamically by calling one of the
`vector` and `list->vector` primitives with the list of Scheme
values that you want to place into a vector. The size of the vector
thus created is determined implicitly by the number of arguments given.

Scheme Procedure: **vector** arg … [¶](#index-vector)

Scheme Procedure: **list->vector** l [¶](#index-list_002d_003evector)

C Function: **scm\_vector** (l) [¶](#index-scm_005fvector)
:   Return a newly allocated vector composed of the
    given arguments. Analogous to `list`.

    ```
    (vector 'a 'b 'c) ⇒ #(a b c)
    ```

The inverse operation is `vector->list`:

Scheme Procedure: **vector->list** v [¶](#index-vector_002d_003elist)

C Function: **scm\_vector\_to\_list** (v) [¶](#index-scm_005fvector_005fto_005flist)
:   Return a newly allocated list composed of the elements of v.

    ```
    (vector->list #(dah dah didah)) ⇒  (dah dah didah)
    (list->vector '(dididit dah)) ⇒  #(dididit dah)
    ```

To allocate a vector with an explicitly specified size, use
`make-vector`. With this primitive you can also specify an initial
value for the vector elements (the same value for all elements, that
is):

Scheme Procedure: **make-vector** len [fill] [¶](#index-make_002dvector)

C Function: **scm\_make\_vector** (len, fill) [¶](#index-scm_005fmake_005fvector)
:   Return a newly allocated vector of len elements. If a
    second argument is given, then each position is initialized to
    fill. Otherwise the initial contents of each position is
    unspecified.

C Function: `SCM` **scm\_c\_make\_vector** `(size_t k, SCM fill)` [¶](#index-scm_005fc_005fmake_005fvector)
:   Like `scm_make_vector`, but the length is given as a `size_t`.

To check whether an arbitrary Scheme value *is* a vector, use the
`vector?` primitive:

Scheme Procedure: **vector?** obj [¶](#index-vector_003f)

C Function: **scm\_vector\_p** (obj) [¶](#index-scm_005fvector_005fp)
:   Return `#t` if obj is a vector, otherwise return
    `#f`.

C Function: `int` **scm\_is\_vector** `(SCM obj)` [¶](#index-scm_005fis_005fvector)
:   Return non-zero when obj is a vector, otherwise return
    `zero`.

---

Next: [Vector Accessing from C](#Vector-Accessing-from-C), Previous: [Dynamic Vector Creation and Validation](#Vector-Creation), Up: [Vectors](#Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.10.3 Accessing and Modifying Vector Contents [¶](#Accessing-and-Modifying-Vector-Contents)

`vector-length` and `vector-ref` return information about a
given vector, respectively its size and the elements that are contained
in the vector.

Scheme Procedure: **vector-length** vector [¶](#index-vector_002dlength)

C Function: **scm\_vector\_length** (vector) [¶](#index-scm_005fvector_005flength)
:   Return the number of elements in vector as an exact integer.

C Function: `size_t` **scm\_c\_vector\_length** `(SCM vec)` [¶](#index-scm_005fc_005fvector_005flength)
:   Return the number of elements in vec as a `size_t`.

Scheme Procedure: **vector-ref** vec k [¶](#index-vector_002dref)

C Function: **scm\_vector\_ref** (vec, k) [¶](#index-scm_005fvector_005fref)
:   Return the contents of position k of vec.
    k must be a valid index of vec.

    ```
    (vector-ref #(1 1 2 3 5 8 13 21) 5) ⇒ 8
    (vector-ref #(1 1 2 3 5 8 13 21)
        (let ((i (round (* 2 (acos -1)))))
          (if (inexact? i)
            (inexact->exact i)
               i))) ⇒ 13
    ```

C Function: `SCM` **scm\_c\_vector\_ref** `(SCM vec, size_t k)` [¶](#index-scm_005fc_005fvector_005fref)
:   Return the contents of position k (a `size_t`) of
    vec.

A vector created by one of the dynamic vector constructor procedures
(see [Dynamic Vector Creation and Validation](#Vector-Creation)) can be modified using the following
procedures.

*NOTE:* According to R5RS, it is an error to use any of these
procedures on a literally read vector, because such vectors should be
considered as constants. Currently, however, Guile does not detect this
error.

Scheme Procedure: **vector-set!** vec k obj [¶](#index-vector_002dset_0021)

C Function: **scm\_vector\_set\_x** (vec, k, obj) [¶](#index-scm_005fvector_005fset_005fx)
:   Store obj in position k of vec.
    k must be a valid index of vec.
    The value returned by ‘`vector-set!`’ is unspecified.

    ```
    (let ((vec (vector 0 '(2 2 2 2) "Anna")))
      (vector-set! vec 1 '("Sue" "Sue"))
      vec) ⇒  #(0 ("Sue" "Sue") "Anna")
    ```

C Function: `void` **scm\_c\_vector\_set\_x** `(SCM vec, size_t k, SCM obj)` [¶](#index-scm_005fc_005fvector_005fset_005fx)
:   Store obj in position k (a `size_t`) of vec.

Scheme Procedure: **vector-fill!** vec fill [start [end]] [¶](#index-vector_002dfill_0021)

C Function: **scm\_vector\_fill\_x** (vec, fill) [¶](#index-scm_005fvector_005ffill_005fx)
:   Store fill in every position of vec in the range
    [start ... end). start defaults to 0 and end
    defaults to the length of vec.

    The value returned by `vector-fill!` is unspecified.

Scheme Procedure: **vector-copy** vec [start [end]] [¶](#index-vector_002dcopy)

C Function: **scm\_vector\_copy** (vec) [¶](#index-scm_005fvector_005fcopy)
:   Returns a freshly allocated vector containing the elements of vec
    in the range [start ... end). start defaults to 0 and
    end defaults to the length of vec.

Scheme Procedure: **vector-copy!** dst at src [start [end]] [¶](#index-vector_002dcopy_0021)
:   Copy the block of elements from vector src in the range
    [start ... end) into vector dst, starting at position
    at. at and start default to 0 and end defaults
    to the length of src.

    It is an error for dst to have a length less than at +
    (end - start).

    The order in which elements are copied is unspecified, except that if the
    source and destination overlap, copying takes place as if the source is
    first copied into a temporary vector and then into the destination.

    The value returned by `vector-copy!` is unspecified.

Scheme Procedure: **vector-move-left!** vec1 start1 end1 vec2 start2 [¶](#index-vector_002dmove_002dleft_0021)

C Function: **scm\_vector\_move\_left\_x** (vec1, start1, end1, vec2, start2) [¶](#index-scm_005fvector_005fmove_005fleft_005fx)
:   Copy elements from vec1, positions start1 to end1,
    to vec2 starting at position start2. start1 and
    start2 are inclusive indices; end1 is exclusive.

    `vector-move-left!` copies elements in leftmost order.
    Therefore, in the case where vec1 and vec2 refer to the
    same vector, `vector-move-left!` is usually appropriate when
    start1 is greater than start2.

    The value returned by `vector-move-left!` is unspecified.

Scheme Procedure: **vector-move-right!** vec1 start1 end1 vec2 start2 [¶](#index-vector_002dmove_002dright_0021)

C Function: **scm\_vector\_move\_right\_x** (vec1, start1, end1, vec2, start2) [¶](#index-scm_005fvector_005fmove_005fright_005fx)
:   Copy elements from vec1, positions start1 to end1,
    to vec2 starting at position start2. start1 and
    start2 are inclusive indices; end1 is exclusive.

    `vector-move-right!` copies elements in rightmost order.
    Therefore, in the case where vec1 and vec2 refer to the
    same vector, `vector-move-right!` is usually appropriate when
    start1 is less than start2.

    The value returned by `vector-move-right!` is unspecified.

---

Next: [Uniform Numeric Vectors](#Uniform-Numeric-Vectors), Previous: [Accessing and Modifying Vector Contents](#Vector-Accessors), Up: [Vectors](#Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.10.4 Vector Accessing from C [¶](#Vector-Accessing-from-C-1)

A vector can be read and modified from C with the functions
[`scm_c_vector_ref`](#x_002dscm_005fc_005fvector_005fref) and
[`scm_c_vector_set_x`](#x_002dscm_005fc_005fvector_005fset_005fx). In addition to
these functions, there are two other ways to access vectors from C that
might be more efficient in certain situations: you can use the unsafe
*vector macros*; or you can use the general framework for accessing
all kinds of arrays (see [Accessing Arrays from C](#Accessing-Arrays-from-C)), which is more
verbose, but can deal efficiently with all kinds of vectors (and
arrays). For arrays of rank 1 whose backing store is a vector, you can
use the `scm_vector_elements` and
`scm_vector_writable_elements` functions as shortcuts.

C Macro: `size_t` **SCM\_SIMPLE\_VECTOR\_LENGTH** `(SCM vec)` [¶](#index-SCM_005fSIMPLE_005fVECTOR_005fLENGTH)
:   Evaluates to the length of the vector vec. No type
    checking is done.

C Macro: `SCM` **SCM\_SIMPLE\_VECTOR\_REF** `(SCM vec, size_t idx)` [¶](#index-SCM_005fSIMPLE_005fVECTOR_005fREF)
:   Evaluates to the element at position idx in the vector vec.
    No type or range checking is done.

C Macro: `void` **SCM\_SIMPLE\_VECTOR\_SET** `(SCM vec, size_t idx, SCM val)` [¶](#index-SCM_005fSIMPLE_005fVECTOR_005fSET)
:   Sets the element at position idx in the vector vec to
    val. No type or range checking is done.

C Function: `const SCM *` **scm\_vector\_elements** `(SCM array, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fvector_005felements)
:   Acquire a [handle](#Accessing-Arrays-from-C) for array and
    return a read-only pointer to its elements. array must be either
    a vector, or an array of rank 1 whose backing store is a vector;
    otherwise an error is signaled. The handle must eventually be released
    with [`scm_array_handle_release`](#x_002dscm_005farray_005fhandle_005frelease).

    The variables pointed to by lenp and incp are filled with
    the number of elements of the array and the increment (number of
    elements) between successive elements, respectively. Successive
    elements of array need not be contiguous in their underlying
    “root vector” returned here; hence the increment is not necessarily
    equal to 1 and may well be negative too (see [Shared Arrays](#Shared-Arrays)).

    The following example shows the typical way to use this function. It
    creates a list of all elements of array (in reverse order).

    ```
    scm_t_array_handle handle;
    size_t i, len;
    ssize_t inc;
    const SCM *elt;
    SCM list;

    elt = scm_vector_elements (array, &handle, &len, &inc);
    list = SCM_EOL;
    for (i = 0; i < len; i++, elt += inc)
      list = scm_cons (*elt, list);
    scm_array_handle_release (&handle);
    ```

C Function: `SCM *` **scm\_vector\_writable\_elements** `(SCM array, scm_t_array_handle *handle, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fvector_005fwritable_005felements)
:   Like `scm_vector_elements` but the pointer can be used to modify
    the array.

    The following example shows the typical way to use this function. It
    fills an array with `#t`.

    ```
    scm_t_array_handle handle;
    size_t i, len;
    ssize_t inc;
    SCM *elt;

    elt = scm_vector_writable_elements (array, &handle, &len, &inc);
    for (i = 0; i < len; i++, elt += inc)
      *elt = SCM_BOOL_T;
    scm_array_handle_release (&handle);
    ```

---

Previous: [Vector Accessing from C](#Vector-Accessing-from-C), Up: [Vectors](#Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.10.5 Uniform Numeric Vectors [¶](#Uniform-Numeric-Vectors-1)

A uniform numeric vector is a vector whose elements are all of a single
numeric type. Guile offers uniform numeric vectors for signed and
unsigned 8-bit, 16-bit, 32-bit, and 64-bit integers, two sizes of
floating point values, and complex floating-point numbers of these two
sizes. See [SRFI-4 - Homogeneous numeric vector datatypes](#SRFI_002d4), for more information.

For many purposes, bytevectors work just as well as uniform vectors, and have
the advantage that they integrate well with binary input and output.
See [Bytevectors](#Bytevectors), for more information on bytevectors.

---

Next: [Bytevectors](#Bytevectors), Previous: [Vectors](#Vectors), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.11 Bit Vectors [¶](#Bit-Vectors-1)

Bit vectors are zero-origin, one-dimensional arrays of booleans. They
are displayed as a sequence of `0`s and `1`s prefixed by
`#*`, e.g.,

```
(make-bitvector 8 #f) ⇒
#*00000000
```

Bit vectors are the special case of one dimensional bit arrays, and can
thus be used with the array procedures, See [Arrays](#Arrays).

Scheme Procedure: **bitvector?** obj [¶](#index-bitvector_003f)
:   Return `#t` when obj is a bitvector, else
    return `#f`.

Scheme Procedure: **make-bitvector** len [fill] [¶](#index-make_002dbitvector)
:   Create a new bitvector of length len and
    optionally initialize all elements to fill.

Scheme Procedure: **bitvector** bit … [¶](#index-bitvector)
:   Create a new bitvector with the arguments as elements.

Scheme Procedure: **bitvector-length** vec [¶](#index-bitvector_002dlength)
:   Return the length of the bitvector vec.

Scheme Procedure: **bitvector-bit-set?** vec idx [¶](#index-bitvector_002dbit_002dset_003f)

Scheme Procedure: **bitvector-bit-clear?** vec idx [¶](#index-bitvector_002dbit_002dclear_003f)
:   Return `#t` if the bit at index idx of the bitvector
    vec is set (for `bitvector-bit-set?`) or clear (for
    `bitvector-bit-clear?`).

Scheme Procedure: **bitvector-set-bit!** vec idx [¶](#index-bitvector_002dset_002dbit_0021)

Scheme Procedure: **bitvector-clear-bit!** vec idx [¶](#index-bitvector_002dclear_002dbit_0021)
:   Set (for `bitvector-set-bit!`) or clear (for
    `bitvector-clear-bit!`) the bit at index idx of the bitvector
    vec.

Scheme Procedure: **bitvector-set-all-bits!** vec [¶](#index-bitvector_002dset_002dall_002dbits_0021)

Scheme Procedure: **bitvector-clear-all-bits!** vec [¶](#index-bitvector_002dclear_002dall_002dbits_0021)

Scheme Procedure: **bitvector-flip-all-bits!** vec [¶](#index-bitvector_002dflip_002dall_002dbits_0021)
:   Set, clear, or flip all bits of vec.

Scheme Procedure: **list->bitvector** list [¶](#index-list_002d_003ebitvector)

C Function: **scm\_list\_to\_bitvector** (list) [¶](#index-scm_005flist_005fto_005fbitvector)
:   Return a new bitvector initialized with the elements
    of list.

Scheme Procedure: **bitvector->list** vec [¶](#index-bitvector_002d_003elist)

C Function: **scm\_bitvector\_to\_list** (vec) [¶](#index-scm_005fbitvector_005fto_005flist)
:   Return a new list initialized with the elements
    of the bitvector vec.

Scheme Procedure: **bitvector-copy** bitvector [start [end]] [¶](#index-bitvector_002dcopy)

C Function: **scm\_bitvector\_copy** (bitvector, start, end) [¶](#index-scm_005fbitvector_005fcopy)
:   Returns a freshly allocated bitvector containing the elements of bitvector
    in the range [start ... end). start defaults to 0 and
    end defaults to the length of bitvector.

Scheme Procedure: **bitvector-count** bitvector [¶](#index-bitvector_002dcount)
:   Return a count of how many entries in bitvector are set.

    ```
    (bitvector-count #*000111000)  ⇒ 3
    ```

Scheme Procedure: **bitvector-count-bits** bitvector bits [¶](#index-bitvector_002dcount_002dbits)
:   Return a count of how many entries in bitvector are set, with the
    bitvector bits selecting the entries to consider. bitvector
    must be at least as long as bits.

    For example,

    ```
    (bitvector-count-bits #*01110111 #*11001101) ⇒ 3
    ```

Scheme Procedure: **bitvector-position** bitvector bool start [¶](#index-bitvector_002dposition)

C Function: **scm\_bitvector\_position** (bitvector, bool, start) [¶](#index-scm_005fbitvector_005fposition)
:   Return the index of the first occurrence of bool in
    bitvector, starting from start. If there is no bool
    entry between start and the end of bitvector, then return
    `#f`. For example,

    ```
    (bitvector-position #*000101 #t 0)  ⇒ 3
    (bitvector-position #*0001111 #f 3) ⇒ #f
    ```

Scheme Procedure: **bitvector-set-bits!** bitvector bits [¶](#index-bitvector_002dset_002dbits_0021)
:   Set entries of bitvector to `#t`, with bits selecting
    the bits to set. The return value is unspecified. bitvector must
    be at least as long as bits.

    ```
    (define bv (bitvector-copy #*11000010))
    (bitvector-set-bits! bv #*10010001)
    bv
    ⇒ #*11010011
    ```

Scheme Procedure: **bitvector-clear-bits!** bitvector bits [¶](#index-bitvector_002dclear_002dbits_0021)
:   Set entries of bitvector to `#f`, with bits selecting
    the bits to clear. The return value is unspecified. bitvector
    must be at least as long as bits.

    ```
    (define bv (bitvector-copy #*11000010))
    (bitvector-clear-bits! bv #*10010001)
    bv
    ⇒ #*01000010
    ```

C Function: `int` **scm\_is\_bitvector** `(SCM obj)` [¶](#index-scm_005fis_005fbitvector)

C Function: `SCM` **scm\_c\_make\_bitvector** `(size_t len, SCM fill)` [¶](#index-scm_005fc_005fmake_005fbitvector)

C Function: `int` **scm\_bitvector\_bit\_is\_set** `(SCM vec, size_t idx)` [¶](#index-scm_005fbitvector_005fbit_005fis_005fset)

C Function: `int` **scm\_bitvector\_bit\_is\_clear** `(SCM vec, size_t idx)` [¶](#index-scm_005fbitvector_005fbit_005fis_005fclear)

C Function: `void` **scm\_c\_bitvector\_set\_bit\_x** `(SCM vec, size_t idx)` [¶](#index-scm_005fc_005fbitvector_005fset_005fbit_005fx)

C Function: `void` **scm\_c\_bitvector\_clear\_bit\_x** `(SCM vec, size_t idx)` [¶](#index-scm_005fc_005fbitvector_005fclear_005fbit_005fx)

C Function: `void` **scm\_c\_bitvector\_set\_bits\_x** `(SCM vec, SCM bits)` [¶](#index-scm_005fc_005fbitvector_005fset_005fbits_005fx)

C Function: `void` **scm\_c\_bitvector\_clear\_bits\_x** `(SCM vec, SCM bits)` [¶](#index-scm_005fc_005fbitvector_005fclear_005fbits_005fx)

C Function: `void` **scm\_c\_bitvector\_set\_all\_bits\_x** `(SCM vec)` [¶](#index-scm_005fc_005fbitvector_005fset_005fall_005fbits_005fx)

C Function: `void` **scm\_c\_bitvector\_clear\_all\_bits\_x** `(SCM vec)` [¶](#index-scm_005fc_005fbitvector_005fclear_005fall_005fbits_005fx)

C Function: `void` **scm\_c\_bitvector\_flip\_all\_bits\_x** `(SCM vec)` [¶](#index-scm_005fc_005fbitvector_005fflip_005fall_005fbits_005fx)

C Function: `size_t` **scm\_c\_bitvector\_length** `(SCM bitvector)` [¶](#index-scm_005fc_005fbitvector_005flength)

C Function: `size_t` **scm\_c\_bitvector\_count** `(SCM bitvector)` [¶](#index-scm_005fc_005fbitvector_005fcount)

C Function: `size_t` **scm\_c\_bitvector\_count\_bits** `(SCM bitvector, SCM bits)` [¶](#index-scm_005fc_005fbitvector_005fcount_005fbits)
:   C API for the corresponding Scheme bitvector interfaces.

C Function: `const scm_t_uint32 *` **scm\_bitvector\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *offp, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fbitvector_005felements)
:   Like `scm_vector_elements` (see [Vector Accessing from C](#Vector-Accessing-from-C)), but
    for bitvectors. The variable pointed to by offp is set to the
    value returned by `scm_array_handle_bit_elements_offset`. See
    `scm_array_handle_bit_elements` for how to use the returned
    pointer and the offset.

C Function: `scm_t_uint32 *` **scm\_bitvector\_writable\_elements** `(SCM vec, scm_t_array_handle *handle, size_t *offp, size_t *lenp, ssize_t *incp)` [¶](#index-scm_005fbitvector_005fwritable_005felements)
:   Like `scm_bitvector_elements`, but the pointer is good for reading
    and writing.

---

Next: [Arrays](#Arrays), Previous: [Bit Vectors](#Bit-Vectors), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12 Bytevectors [¶](#Bytevectors-1)

A *bytevector* is a raw byte string. The `(rnrs bytevectors)`
module provides the programming interface specified by the
[Revised^6 Report on the Algorithmic Language
Scheme (R6RS)](http://www.r6rs.org/). It contains procedures to manipulate bytevectors and
interpret their contents in a number of ways: as signed or unsigned
integer of various sizes and endianness, as IEEE-754 floating point
numbers, or as strings. It is a useful tool to encode and decode binary
data. The [R7RS](#R7RS-Support) offers its own set of bytevector
procedures (see [Bytevector Procedures in R7RS](#Bytevector-Procedures-in-R7RS)).

The R6RS (Section 4.3.4) specifies an external representation for
bytevectors, whereby the octets (integers in the range 0–255) contained
in the bytevector are represented as a list prefixed by `#vu8`:

```
#vu8(1 53 204)
```

denotes a 3-byte bytevector containing the octets 1, 53, and 204. Like
string literals, booleans, etc., bytevectors are “self-quoting”, i.e.,
they do not need to be quoted:

```
#vu8(1 53 204)
⇒ #vu8(1 53 204)
```

Bytevectors can be used with the binary input/output primitives
(see [Binary I/O](#Binary-I_002fO)).

* [Endianness](#Bytevector-Endianness)
* [Manipulating Bytevectors](#Bytevector-Manipulation)
* [Interpreting Bytevector Contents as Integers](#Bytevectors-as-Integers)
* [Converting Bytevectors to/from Integer Lists](#Bytevectors-and-Integer-Lists)
* [Interpreting Bytevector Contents as Floating Point Numbers](#Bytevectors-as-Floats)
* [Interpreting Bytevector Contents as Unicode Strings](#Bytevectors-as-Strings)
* [Accessing Bytevectors with the Array API](#Bytevectors-as-Arrays)
* [Accessing Bytevectors with the SRFI-4 API](#Bytevectors-as-Uniform-Vectors)
* [Bytevector Procedures in R7RS](#Bytevector-Procedures-in-R7RS)
* [Bytevector Slices](#Bytevector-Slices)

---

Next: [Manipulating Bytevectors](#Bytevector-Manipulation), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.1 Endianness [¶](#Endianness)

Some of the following procedures take an endianness parameter.
The *endianness* is defined as the order of bytes in multi-byte
numbers: numbers encoded in *big endian* have their most
significant bytes written first, whereas numbers encoded in
*little endian* have their least significant bytes
first[11](#FOOT11).

Little-endian is the native endianness of the IA32 architecture and
its derivatives, while big-endian is native to SPARC and PowerPC,
among others. The `native-endianness` procedure returns the
native endianness of the machine it runs on.

Scheme Procedure: **native-endianness** [¶](#index-native_002dendianness)

C Function: **scm\_native\_endianness** () [¶](#index-scm_005fnative_005fendianness)
:   Return a value denoting the native endianness of the host machine.

Scheme Macro: **endianness** symbol [¶](#index-endianness-1)
:   Return an object denoting the endianness specified by symbol. If
    symbol is neither `big` nor `little` then an error is
    raised at expand-time.

C Variable: **scm\_endianness\_big** [¶](#index-scm_005fendianness_005fbig)

C Variable: **scm\_endianness\_little** [¶](#index-scm_005fendianness_005flittle)
:   The objects denoting big- and little-endianness, respectively.

---

Next: [Interpreting Bytevector Contents as Integers](#Bytevectors-as-Integers), Previous: [Endianness](#Bytevector-Endianness), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.2 Manipulating Bytevectors [¶](#Manipulating-Bytevectors)

Bytevectors can be created, copied, and analyzed with the following
procedures and C functions.

Scheme Procedure: **make-bytevector** len [fill] [¶](#index-make_002dbytevector)

C Function: **scm\_make\_bytevector** (len, fill) [¶](#index-scm_005fmake_005fbytevector)

C Function: **scm\_c\_make\_bytevector** (size\_t len) [¶](#index-scm_005fc_005fmake_005fbytevector)
:   Return a new bytevector of len bytes. Optionally, if fill
    is given, fill it with fill; fill must be in the range
    [-128,255].

Scheme Procedure: **bytevector?** obj [¶](#index-bytevector_003f)

C Function: **scm\_bytevector\_p** (obj) [¶](#index-scm_005fbytevector_005fp)
:   Return true if obj is a bytevector.

C Function: `int` **scm\_is\_bytevector** `(SCM obj)` [¶](#index-scm_005fis_005fbytevector)
:   Equivalent to `scm_is_true (scm_bytevector_p (obj))`.

Scheme Procedure: **bytevector-length** bv [¶](#index-bytevector_002dlength)

C Function: **scm\_bytevector\_length** (bv) [¶](#index-scm_005fbytevector_005flength)
:   Return the length in bytes of bytevector bv.

C Function: `size_t` **scm\_c\_bytevector\_length** `(SCM bv)` [¶](#index-scm_005fc_005fbytevector_005flength)
:   Likewise, return the length in bytes of bytevector bv.

Scheme Procedure: **bytevector=?** bv1 bv2 [¶](#index-bytevector_003d_003f)

C Function: **scm\_bytevector\_eq\_p** (bv1, bv2) [¶](#index-scm_005fbytevector_005feq_005fp)
:   Return `#t` if bv1 equals bv2—i.e., if they have the same
    length and contents.

Scheme Procedure: **bytevector-fill!** bv fill [start [end]] [¶](#index-bytevector_002dfill_0021)

C Function: **scm\_bytevector\_fill\_x** (bv, fill) [¶](#index-scm_005fbytevector_005ffill_005fx)
:   Fill positions [start ... end) of bytevector bv with
    byte fill. start defaults to 0 and end defaults to the
    length of bv.[12](#FOOT12)

Scheme Procedure: **bytevector-copy!** source source-start target target-start len [¶](#index-bytevector_002dcopy_0021)

C Function: **scm\_bytevector\_copy\_x** (source, source\_start, target, target\_start, len) [¶](#index-scm_005fbytevector_005fcopy_005fx)
:   Copy len bytes from source into target, starting
    reading from source-start (an index index within source)
    and writing at target-start.

    It is permitted for the source and target regions to
    overlap. In that case, copying takes place as if the source is first
    copied into a temporary bytevector and then into the destination.

Scheme Procedure: **bytevector-copy** bv [¶](#index-bytevector_002dcopy)

C Function: **scm\_bytevector\_copy** (bv) [¶](#index-scm_005fbytevector_005fcopy)
:   Return a newly allocated copy of bv.

C Function: `scm_t_uint8` **scm\_c\_bytevector\_ref** `(SCM bv, size_t index)` [¶](#index-scm_005fc_005fbytevector_005fref)
:   Return the byte at index in bytevector bv.

C Function: `void` **scm\_c\_bytevector\_set\_x** `(SCM bv, size_t index, scm_t_uint8 value)` [¶](#index-scm_005fc_005fbytevector_005fset_005fx)
:   Set the byte at index in bv to value.

Low-level C macros are available. They do not perform any
type-checking; as such they should be used with care.

C Macro: `size_t` **SCM\_BYTEVECTOR\_LENGTH** `(bv)` [¶](#index-SCM_005fBYTEVECTOR_005fLENGTH)
:   Return the length in bytes of bytevector bv.

C Macro: `signed char *` **SCM\_BYTEVECTOR\_CONTENTS** `(bv)` [¶](#index-SCM_005fBYTEVECTOR_005fCONTENTS)
:   Return a pointer to the contents of bytevector bv.

---

Next: [Converting Bytevectors to/from Integer Lists](#Bytevectors-and-Integer-Lists), Previous: [Manipulating Bytevectors](#Bytevector-Manipulation), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.3 Interpreting Bytevector Contents as Integers [¶](#Interpreting-Bytevector-Contents-as-Integers)

The contents of a bytevector can be interpreted as a sequence of
integers of any given size, sign, and endianness.

```
(let ((bv (make-bytevector 4)))
  (bytevector-u8-set! bv 0 #x12)
  (bytevector-u8-set! bv 1 #x34)
  (bytevector-u8-set! bv 2 #x56)
  (bytevector-u8-set! bv 3 #x78)

  (map (lambda (number)
         (number->string number 16))
       (list (bytevector-u8-ref bv 0)
             (bytevector-u16-ref bv 0 (endianness big))
             (bytevector-u32-ref bv 0 (endianness little)))))

⇒ ("12" "1234" "78563412")
```

The most generic procedures to interpret bytevector contents as integers
are described below.

Scheme Procedure: **bytevector-uint-ref** bv index endianness size [¶](#index-bytevector_002duint_002dref)

C Function: **scm\_bytevector\_uint\_ref** (bv, index, endianness, size) [¶](#index-scm_005fbytevector_005fuint_005fref)
:   Return the size-byte long unsigned integer at index index in
    bv, decoded according to endianness.

Scheme Procedure: **bytevector-sint-ref** bv index endianness size [¶](#index-bytevector_002dsint_002dref)

C Function: **scm\_bytevector\_sint\_ref** (bv, index, endianness, size) [¶](#index-scm_005fbytevector_005fsint_005fref)
:   Return the size-byte long signed integer at index index in
    bv, decoded according to endianness.

Scheme Procedure: **bytevector-uint-set!** bv index value endianness size [¶](#index-bytevector_002duint_002dset_0021)

C Function: **scm\_bytevector\_uint\_set\_x** (bv, index, value, endianness, size) [¶](#index-scm_005fbytevector_005fuint_005fset_005fx)
:   Set the size-byte long unsigned integer at index to
    value, encoded according to endianness.

Scheme Procedure: **bytevector-sint-set!** bv index value endianness size [¶](#index-bytevector_002dsint_002dset_0021)

C Function: **scm\_bytevector\_sint\_set\_x** (bv, index, value, endianness, size) [¶](#index-scm_005fbytevector_005fsint_005fset_005fx)
:   Set the size-byte long signed integer at index to
    value, encoded according to endianness.

The following procedures are similar to the ones above, but specialized
to a given integer size:

Scheme Procedure: **bytevector-u8-ref** bv index [¶](#index-bytevector_002du8_002dref)

Scheme Procedure: **bytevector-s8-ref** bv index [¶](#index-bytevector_002ds8_002dref)

Scheme Procedure: **bytevector-u16-ref** bv index endianness [¶](#index-bytevector_002du16_002dref)

Scheme Procedure: **bytevector-s16-ref** bv index endianness [¶](#index-bytevector_002ds16_002dref)

Scheme Procedure: **bytevector-u32-ref** bv index endianness [¶](#index-bytevector_002du32_002dref)

Scheme Procedure: **bytevector-s32-ref** bv index endianness [¶](#index-bytevector_002ds32_002dref)

Scheme Procedure: **bytevector-u64-ref** bv index endianness [¶](#index-bytevector_002du64_002dref)

Scheme Procedure: **bytevector-s64-ref** bv index endianness [¶](#index-bytevector_002ds64_002dref)

C Function: **scm\_bytevector\_u8\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fu8_005fref)

C Function: **scm\_bytevector\_s8\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fs8_005fref)

C Function: **scm\_bytevector\_u16\_ref** (bv, index, endianness) [¶](#index-scm_005fbytevector_005fu16_005fref)

C Function: **scm\_bytevector\_s16\_ref** (bv, index, endianness) [¶](#index-scm_005fbytevector_005fs16_005fref)

C Function: **scm\_bytevector\_u32\_ref** (bv, index, endianness) [¶](#index-scm_005fbytevector_005fu32_005fref)

C Function: **scm\_bytevector\_s32\_ref** (bv, index, endianness) [¶](#index-scm_005fbytevector_005fs32_005fref)

C Function: **scm\_bytevector\_u64\_ref** (bv, index, endianness) [¶](#index-scm_005fbytevector_005fu64_005fref)

C Function: **scm\_bytevector\_s64\_ref** (bv, index, endianness) [¶](#index-scm_005fbytevector_005fs64_005fref)
:   Return the unsigned n-bit (signed) integer (where n is 8,
    16, 32 or 64) from bv at index, decoded according to
    endianness.

Scheme Procedure: **bytevector-u8-set!** bv index value [¶](#index-bytevector_002du8_002dset_0021)

Scheme Procedure: **bytevector-s8-set!** bv index value [¶](#index-bytevector_002ds8_002dset_0021)

Scheme Procedure: **bytevector-u16-set!** bv index value endianness [¶](#index-bytevector_002du16_002dset_0021)

Scheme Procedure: **bytevector-s16-set!** bv index value endianness [¶](#index-bytevector_002ds16_002dset_0021)

Scheme Procedure: **bytevector-u32-set!** bv index value endianness [¶](#index-bytevector_002du32_002dset_0021)

Scheme Procedure: **bytevector-s32-set!** bv index value endianness [¶](#index-bytevector_002ds32_002dset_0021)

Scheme Procedure: **bytevector-u64-set!** bv index value endianness [¶](#index-bytevector_002du64_002dset_0021)

Scheme Procedure: **bytevector-s64-set!** bv index value endianness [¶](#index-bytevector_002ds64_002dset_0021)

C Function: **scm\_bytevector\_u8\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fu8_005fset_005fx)

C Function: **scm\_bytevector\_s8\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fs8_005fset_005fx)

C Function: **scm\_bytevector\_u16\_set\_x** (bv, index, value, endianness) [¶](#index-scm_005fbytevector_005fu16_005fset_005fx)

C Function: **scm\_bytevector\_s16\_set\_x** (bv, index, value, endianness) [¶](#index-scm_005fbytevector_005fs16_005fset_005fx)

C Function: **scm\_bytevector\_u32\_set\_x** (bv, index, value, endianness) [¶](#index-scm_005fbytevector_005fu32_005fset_005fx)

C Function: **scm\_bytevector\_s32\_set\_x** (bv, index, value, endianness) [¶](#index-scm_005fbytevector_005fs32_005fset_005fx)

C Function: **scm\_bytevector\_u64\_set\_x** (bv, index, value, endianness) [¶](#index-scm_005fbytevector_005fu64_005fset_005fx)

C Function: **scm\_bytevector\_s64\_set\_x** (bv, index, value, endianness) [¶](#index-scm_005fbytevector_005fs64_005fset_005fx)
:   Store value as an n-bit (signed) integer (where n is
    8, 16, 32 or 64) in bv at index, encoded according to
    endianness.

Finally, a variant specialized for the host’s endianness is available
for each of these functions (with the exception of the `u8` and
`s8` accessors, as endianness is about byte order and there is only
1 byte):

Scheme Procedure: **bytevector-u16-native-ref** bv index [¶](#index-bytevector_002du16_002dnative_002dref)

Scheme Procedure: **bytevector-s16-native-ref** bv index [¶](#index-bytevector_002ds16_002dnative_002dref)

Scheme Procedure: **bytevector-u32-native-ref** bv index [¶](#index-bytevector_002du32_002dnative_002dref)

Scheme Procedure: **bytevector-s32-native-ref** bv index [¶](#index-bytevector_002ds32_002dnative_002dref)

Scheme Procedure: **bytevector-u64-native-ref** bv index [¶](#index-bytevector_002du64_002dnative_002dref)

Scheme Procedure: **bytevector-s64-native-ref** bv index [¶](#index-bytevector_002ds64_002dnative_002dref)

C Function: **scm\_bytevector\_u16\_native\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fu16_005fnative_005fref)

C Function: **scm\_bytevector\_s16\_native\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fs16_005fnative_005fref)

C Function: **scm\_bytevector\_u32\_native\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fu32_005fnative_005fref)

C Function: **scm\_bytevector\_s32\_native\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fs32_005fnative_005fref)

C Function: **scm\_bytevector\_u64\_native\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fu64_005fnative_005fref)

C Function: **scm\_bytevector\_s64\_native\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fs64_005fnative_005fref)
:   Return the unsigned n-bit (signed) integer (where n is 8,
    16, 32 or 64) from bv at index, decoded according to the
    host’s native endianness.

Scheme Procedure: **bytevector-u16-native-set!** bv index value [¶](#index-bytevector_002du16_002dnative_002dset_0021)

Scheme Procedure: **bytevector-s16-native-set!** bv index value [¶](#index-bytevector_002ds16_002dnative_002dset_0021)

Scheme Procedure: **bytevector-u32-native-set!** bv index value [¶](#index-bytevector_002du32_002dnative_002dset_0021)

Scheme Procedure: **bytevector-s32-native-set!** bv index value [¶](#index-bytevector_002ds32_002dnative_002dset_0021)

Scheme Procedure: **bytevector-u64-native-set!** bv index value [¶](#index-bytevector_002du64_002dnative_002dset_0021)

Scheme Procedure: **bytevector-s64-native-set!** bv index value [¶](#index-bytevector_002ds64_002dnative_002dset_0021)

C Function: **scm\_bytevector\_u16\_native\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fu16_005fnative_005fset_005fx)

C Function: **scm\_bytevector\_s16\_native\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fs16_005fnative_005fset_005fx)

C Function: **scm\_bytevector\_u32\_native\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fu32_005fnative_005fset_005fx)

C Function: **scm\_bytevector\_s32\_native\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fs32_005fnative_005fset_005fx)

C Function: **scm\_bytevector\_u64\_native\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fu64_005fnative_005fset_005fx)

C Function: **scm\_bytevector\_s64\_native\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fs64_005fnative_005fset_005fx)
:   Store value as an n-bit (signed) integer (where n is
    8, 16, 32 or 64) in bv at index, encoded according to the
    host’s native endianness.

---

Next: [Interpreting Bytevector Contents as Floating Point Numbers](#Bytevectors-as-Floats), Previous: [Interpreting Bytevector Contents as Integers](#Bytevectors-as-Integers), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.4 Converting Bytevectors to/from Integer Lists [¶](#Converting-Bytevectors-to_002ffrom-Integer-Lists)

Bytevector contents can readily be converted to/from lists of signed or
unsigned integers:

```
(bytevector->sint-list (u8-list->bytevector (make-list 4 255))
                       (endianness little) 2)
⇒ (-1 -1)
```

Scheme Procedure: **bytevector->u8-list** bv [¶](#index-bytevector_002d_003eu8_002dlist)

C Function: **scm\_bytevector\_to\_u8\_list** (bv) [¶](#index-scm_005fbytevector_005fto_005fu8_005flist)
:   Return a newly allocated list of unsigned 8-bit integers from the
    contents of bv.

Scheme Procedure: **u8-list->bytevector** lst [¶](#index-u8_002dlist_002d_003ebytevector)

C Function: **scm\_u8\_list\_to\_bytevector** (lst) [¶](#index-scm_005fu8_005flist_005fto_005fbytevector)
:   Return a newly allocated bytevector consisting of the unsigned 8-bit
    integers listed in lst.

Scheme Procedure: **bytevector->uint-list** bv endianness size [¶](#index-bytevector_002d_003euint_002dlist)

C Function: **scm\_bytevector\_to\_uint\_list** (bv, endianness, size) [¶](#index-scm_005fbytevector_005fto_005fuint_005flist)
:   Return a list of unsigned integers of size bytes representing the
    contents of bv, decoded according to endianness.

Scheme Procedure: **bytevector->sint-list** bv endianness size [¶](#index-bytevector_002d_003esint_002dlist)

C Function: **scm\_bytevector\_to\_sint\_list** (bv, endianness, size) [¶](#index-scm_005fbytevector_005fto_005fsint_005flist)
:   Return a list of signed integers of size bytes representing the
    contents of bv, decoded according to endianness.

Scheme Procedure: **uint-list->bytevector** lst endianness size [¶](#index-uint_002dlist_002d_003ebytevector)

C Function: **scm\_uint\_list\_to\_bytevector** (lst, endianness, size) [¶](#index-scm_005fuint_005flist_005fto_005fbytevector)
:   Return a new bytevector containing the unsigned integers listed in
    lst and encoded on size bytes according to endianness.

Scheme Procedure: **sint-list->bytevector** lst endianness size [¶](#index-sint_002dlist_002d_003ebytevector)

C Function: **scm\_sint\_list\_to\_bytevector** (lst, endianness, size) [¶](#index-scm_005fsint_005flist_005fto_005fbytevector)
:   Return a new bytevector containing the signed integers listed in
    lst and encoded on size bytes according to endianness.

---

Next: [Interpreting Bytevector Contents as Unicode Strings](#Bytevectors-as-Strings), Previous: [Converting Bytevectors to/from Integer Lists](#Bytevectors-and-Integer-Lists), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.5 Interpreting Bytevector Contents as Floating Point Numbers [¶](#Interpreting-Bytevector-Contents-as-Floating-Point-Numbers)

Bytevector contents can also be accessed as IEEE-754 single- or
double-precision floating point numbers (respectively 32 and 64-bit
long) using the procedures described here.

Scheme Procedure: **bytevector-ieee-single-ref** bv index endianness [¶](#index-bytevector_002dieee_002dsingle_002dref)

Scheme Procedure: **bytevector-ieee-double-ref** bv index endianness [¶](#index-bytevector_002dieee_002ddouble_002dref)

C Function: **scm\_bytevector\_ieee\_single\_ref** (bv, index, endianness) [¶](#index-scm_005fbytevector_005fieee_005fsingle_005fref)

C Function: **scm\_bytevector\_ieee\_double\_ref** (bv, index, endianness) [¶](#index-scm_005fbytevector_005fieee_005fdouble_005fref)
:   Return the IEEE-754 single-precision floating point number from bv
    at index according to endianness.

Scheme Procedure: **bytevector-ieee-single-set!** bv index value endianness [¶](#index-bytevector_002dieee_002dsingle_002dset_0021)

Scheme Procedure: **bytevector-ieee-double-set!** bv index value endianness [¶](#index-bytevector_002dieee_002ddouble_002dset_0021)

C Function: **scm\_bytevector\_ieee\_single\_set\_x** (bv, index, value, endianness) [¶](#index-scm_005fbytevector_005fieee_005fsingle_005fset_005fx)

C Function: **scm\_bytevector\_ieee\_double\_set\_x** (bv, index, value, endianness) [¶](#index-scm_005fbytevector_005fieee_005fdouble_005fset_005fx)
:   Store real number value in bv at index according to
    endianness.

Specialized procedures are also available:

Scheme Procedure: **bytevector-ieee-single-native-ref** bv index [¶](#index-bytevector_002dieee_002dsingle_002dnative_002dref)

Scheme Procedure: **bytevector-ieee-double-native-ref** bv index [¶](#index-bytevector_002dieee_002ddouble_002dnative_002dref)

C Function: **scm\_bytevector\_ieee\_single\_native\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fieee_005fsingle_005fnative_005fref)

C Function: **scm\_bytevector\_ieee\_double\_native\_ref** (bv, index) [¶](#index-scm_005fbytevector_005fieee_005fdouble_005fnative_005fref)
:   Return the IEEE-754 single-precision floating point number from bv
    at index according to the host’s native endianness.

Scheme Procedure: **bytevector-ieee-single-native-set!** bv index value [¶](#index-bytevector_002dieee_002dsingle_002dnative_002dset_0021)

Scheme Procedure: **bytevector-ieee-double-native-set!** bv index value [¶](#index-bytevector_002dieee_002ddouble_002dnative_002dset_0021)

C Function: **scm\_bytevector\_ieee\_single\_native\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fieee_005fsingle_005fnative_005fset_005fx)

C Function: **scm\_bytevector\_ieee\_double\_native\_set\_x** (bv, index, value) [¶](#index-scm_005fbytevector_005fieee_005fdouble_005fnative_005fset_005fx)
:   Store real number value in bv at index according to
    the host’s native endianness.

---

Next: [Accessing Bytevectors with the Array API](#Bytevectors-as-Arrays), Previous: [Interpreting Bytevector Contents as Floating Point Numbers](#Bytevectors-as-Floats), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.6 Interpreting Bytevector Contents as Unicode Strings [¶](#Interpreting-Bytevector-Contents-as-Unicode-Strings)

Bytevector contents can also be interpreted as Unicode strings encoded
in one of the most commonly available encoding formats.
See [Representing Strings as Bytes](#Representing-Strings-as-Bytes), for a more generic interface.

```
(utf8->string (u8-list->bytevector '(99 97 102 101)))
⇒ "cafe"

(string->utf8 "café") ;; SMALL LATIN LETTER E WITH ACUTE ACCENT
⇒ #vu8(99 97 102 195 169)
```

Scheme Procedure: **string-utf8-length** `str` [¶](#index-string_002dutf8_002dlength)

C function: `SCM` **scm\_string\_utf8\_length** `(str)` [¶](#index-scm_005fstring_005futf8_005flength)

C function: `size_t` **scm\_c\_string\_utf8\_length** `(str)` [¶](#index-scm_005fc_005fstring_005futf8_005flength)
:   Return the number of bytes in the UTF-8 representation of str.

Scheme Procedure: **string->utf8** str [¶](#index-string_002d_003eutf8)

Scheme Procedure: **string->utf16** str [endianness] [¶](#index-string_002d_003eutf16)

Scheme Procedure: **string->utf32** str [endianness] [¶](#index-string_002d_003eutf32)

C Function: **scm\_string\_to\_utf8** (str) [¶](#index-scm_005fstring_005fto_005futf8)

C Function: **scm\_string\_to\_utf16** (str, endianness) [¶](#index-scm_005fstring_005fto_005futf16)

C Function: **scm\_string\_to\_utf32** (str, endianness) [¶](#index-scm_005fstring_005fto_005futf32)
:   Return a newly allocated bytevector that contains the UTF-8, UTF-16, or
    UTF-32 (aka. UCS-4) encoding of str. For UTF-16 and UTF-32,
    endianness should be the symbol `big` or `little`; when omitted,
    it defaults to big endian.

Scheme Procedure: **utf8->string** utf [¶](#index-utf8_002d_003estring)

Scheme Procedure: **utf16->string** utf [endianness] [¶](#index-utf16_002d_003estring)

Scheme Procedure: **utf32->string** utf [endianness] [¶](#index-utf32_002d_003estring)

C Function: **scm\_utf8\_to\_string** (utf) [¶](#index-scm_005futf8_005fto_005fstring)

C Function: **scm\_utf16\_to\_string** (utf, endianness) [¶](#index-scm_005futf16_005fto_005fstring)

C Function: **scm\_utf32\_to\_string** (utf, endianness) [¶](#index-scm_005futf32_005fto_005fstring)
:   Return a newly allocated string that contains from the UTF-8-, UTF-16-,
    or UTF-32-decoded contents of bytevector utf. For UTF-16 and UTF-32,
    endianness should be the symbol `big` or `little`; when omitted,
    it defaults to big endian.

---

Next: [Accessing Bytevectors with the SRFI-4 API](#Bytevectors-as-Uniform-Vectors), Previous: [Interpreting Bytevector Contents as Unicode Strings](#Bytevectors-as-Strings), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.7 Accessing Bytevectors with the Array API [¶](#Accessing-Bytevectors-with-the-Array-API)

As an extension to the R6RS, Guile allows bytevectors to be manipulated
with the *array* procedures (see [Arrays](#Arrays)). When using these
APIs, bytes are accessed one at a time as 8-bit unsigned integers:

```
(define bv #vu8(0 1 2 3))

(array? bv)
⇒ #t

(array-rank bv)
⇒ 1

(array-ref bv 2)
⇒ 2

;; Note the different argument order on array-set!.
(array-set! bv 77 2)
(array-ref bv 2)
⇒ 77

(array-type bv)
⇒ vu8
```

---

Next: [Bytevector Procedures in R7RS](#Bytevector-Procedures-in-R7RS), Previous: [Accessing Bytevectors with the Array API](#Bytevectors-as-Arrays), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.8 Accessing Bytevectors with the SRFI-4 API [¶](#Accessing-Bytevectors-with-the-SRFI_002d4-API)

Bytevectors may also be accessed with the SRFI-4 API. See [SRFI-4 - Relation to bytevectors](#SRFI_002d4-and-Bytevectors), for more information.

---

Next: [Bytevector Slices](#Bytevector-Slices), Previous: [Accessing Bytevectors with the SRFI-4 API](#Bytevectors-as-Uniform-Vectors), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.9 Bytevector Procedures in R7RS [¶](#Bytevector-Procedures-in-R7RS-1)

The [R7RS](#R7RS-Support) (Section 6.9) defines a set of
bytevector manipulation procedures, accessible with

```
(use-modules (scheme base))
```

Of these, [`make-bytevector`](#x_002dmake_002dbytevector),
[`bytevector?`](#x_002dbytevector_003f),
[`bytevector-length`](#x_002dbytevector_002dlength),
[`bytevector-u8-ref`](#x_002dbytevector_002du8_002dref) and
[`bytevector-u8-set!`](#x_002dbytevector_002du8_002dset_0021) have the same
definition as in R6RS. The procedures listed below either have a
different definition in R7RS and R6RS, or are not defined in R6RS.

Scheme Procedure: **bytevector** arg … [¶](#index-bytevector-1)
:   Return a newly allocated bytevector composed of the given arguments.
    Analogous to `list`.

    ```
    (bytevector 2 3 4) ⇒ #vu8(2 3 4)
    ```

    See also [`u8-list->bytevector`](#x_002du8_002dlist_002d_003ebytevector).

Scheme Procedure: **bytevector-copy** bv [start [end]] [¶](#index-bytevector_002dcopy-1)
:   Returns a newly allocated bytevector containing the elements of bv
    in the range [start ... end). start defaults to 0 and
    end defaults to the length of bv.

    ```
    (define bv #vu8(0 1 2 3 4 5))
    (bytevector-copy bv) ⇒ #vu8(0 1 2 3 4 5)
    (bytevector-copy bv 2) ⇒ #vu8(2 3 4 5)
    (bytevector-copy bv 2 4) ⇒ #vu8(2 3)
    ```

    See also [the R6RS version](#x_002dr6_003abytevector_002dcopy).

Scheme Procedure: **bytevector-copy!** dst at src [start [end]] [¶](#index-bytevector_002dcopy_0021-1)
:   Copy the block of elements from bytevector src in the range
    [start ... end) into bytevector dst, starting at
    position at. start defaults to 0 and end defaults to
    the length of src. It is an error for dst
    to have a length less than at + (end - start).

    See also [the R6RS version](#x_002dr6_003abytevector_002dcopy_0021). With

    ```
    (use-modules ((rnrs bytevectors) #:prefix r6:)
                 ((scheme base) #:prefix r7:))
    ```

    the following calls are equivalent:

    ```
    (r6:bytevector-copy! source source-start target target-start len)
    (r7:bytevector-copy! target target-start source source-start (+ source-start len))
    ```

Scheme Procedure: **bytevector-append** arg … [¶](#index-bytevector_002dappend)
:   Return a newly allocated bytevector whose characters form the
    concatenation of the given bytevectors arg ...

    ```
    (bytevector-append #vu8(0 1 2) #vu8(3 4 5))
    ⇒ #vu8(0 1 2 3 4 5)
    ```

---

Previous: [Bytevector Procedures in R7RS](#Bytevector-Procedures-in-R7RS), Up: [Bytevectors](#Bytevectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.12.10 Bytevector Slices [¶](#Bytevector-Slices-1)

As an extension to the R6RS specification, the `(rnrs bytevectors
gnu)` module provides the `bytevector-slice` procedure, which
returns a bytevector aliasing part of an existing bytevector.

Scheme Procedure: **bytevector-slice** bv offset [size] [¶](#index-bytevector_002dslice)

C Function: **scm\_bytevector\_slice** (bv, offset, size) [¶](#index-scm_005fbytevector_005fslice)
:   Return the slice of bv starting at offset and counting
    size bytes. When size is omitted, the slice covers all
    of bv starting from offset. The returned slice shares
    storage with bv: changes to the slice are visible in bv
    and vice-versa.

    When bv is actually a SRFI-4 uniform vector, its element
    type is preserved unless offset and size are not aligned
    on its element type size.

Here is an example showing how to use it:

```
(use-modules (rnrs bytevectors)
             (rnrs bytevectors gnu))

(define bv (u8-list->bytevector (iota 10)))
(define slice (bytevector-slice bv 2 3))

slice
⇒ #vu8(2 3 4)

(bytevector-u8-set! slice 0 77)
slice
⇒ #vu8(77 3 4)

bv
⇒ #vu8(0 1 77 3 4 5 6 7 8 9)
```

---

Next: [VLists](#VLists), Previous: [Bytevectors](#Bytevectors), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.13 Arrays [¶](#Arrays-1)

*Arrays* are a collection of cells organized into an arbitrary
number of dimensions. Each cell can be accessed in constant time by
supplying an index for each dimension.

In the current implementation, an array uses a vector of some kind for
the actual storage of its elements. Any kind of vector will do, so you
can have arrays of uniform numeric values, arrays of characters, arrays
of bits, and of course, arrays of arbitrary Scheme values. For example,
arrays with an underlying `c64vector` might be nice for digital
signal processing, while arrays made from a `u8vector` might be
used to hold gray-scale images.

The number of dimensions of an array is called its *rank*. Thus,
a matrix is an array of rank 2, while a vector has rank 1. When
accessing an array element, you have to specify one exact integer for
each dimension. These integers are called the *indices* of the
element. An array specifies the allowed range of indices for each
dimension via an inclusive lower and upper bound. These bounds can
well be negative, but the upper bound must be greater than or equal to
the lower bound minus one. When all lower bounds of an array are
zero, it is called a *zero-origin* array.

Arrays can be of rank 0, which could be interpreted as a scalar.
Thus, a zero-rank array can store exactly one object and the list of
indices of this element is the empty list.

Arrays contain zero elements when one of their dimensions has a zero
length. These empty arrays maintain information about their shape: a
matrix with zero columns and 3 rows is different from a matrix with 3
columns and zero rows, which again is different from a vector of
length zero.

The array procedures are all polymorphic, treating strings, uniform
numeric vectors, bytevectors, bit vectors and ordinary vectors as one
dimensional arrays.

* [Array Syntax](#Array-Syntax)
* [Array Procedures](#Array-Procedures)
* [Shared Arrays](#Shared-Arrays)
* [Arrays as arrays of arrays](#Arrays-as-arrays-of-arrays)
* [Accessing Arrays from C](#Accessing-Arrays-from-C)

---

Next: [Array Procedures](#Array-Procedures), Up: [Arrays](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.13.1 Array Syntax [¶](#Array-Syntax-1)

An array is displayed as `#` followed by its rank, followed by a
tag that describes the underlying vector, optionally followed by
information about its shape, and finally followed by the cells,
organized into dimensions using parentheses.

In more words, the array tag is of the form

```
  #<rank><vectag><@lower><:len><@lower><:len>...
```

where `<rank>` is a positive integer in decimal giving the rank of
the array. It is omitted when the rank is 1 and the array is non-shared
and has zero-origin (see below). For shared arrays and for a non-zero
origin, the rank is always printed even when it is 1 to distinguish
them from ordinary vectors.

The `<vectag>` part is the tag for a uniform numeric vector, like
`u8`, `s16`, etc, `b` for bitvectors, or `a` for
strings. It is empty for ordinary vectors.

The `<@lower>` part is a ‘`@`’ character followed by a signed
integer in decimal giving the lower bound of a dimension. There is one
`<@lower>` for each dimension. When all lower bounds are zero,
all `<@lower>` parts are omitted.

The `<:len>` part is a ‘`:`’ character followed by an unsigned
integer in decimal giving the length of a dimension. Like for the lower
bounds, there is one `<:len>` for each dimension, and the
`<:len>` part always follows the `<@lower>` part for a
dimension. Lengths are only then printed when they can’t be deduced
from the nested lists of elements of the array literal, which can happen
when at least one length is zero.

As a special case, an array of rank 0 is printed as
`#0<vectag>(<scalar>)`, where `<scalar>` is the result of
printing the single element of the array.

Thus,

`#(1 2 3)`
:   is an ordinary array of rank 1 with lower bound 0 in dimension 0.
    (I.e., a regular vector.)

`#@2(1 2 3)`
:   is an ordinary array of rank 1 with lower bound 2 in dimension 0.

`#2((1 2 3) (4 5 6))`
:   is a non-uniform array of rank 2; a 2x3 matrix with index ranges 0..1
    and 0..2.

`#u8(0 1 2)`
:   is a uniform u8 array of rank 1.

`#2u32@2@3((1 2) (2 3))`
:   is a uniform u32 array of rank 2 with index ranges 2..3 and 3..4.

`#2()`
:   is a two-dimensional array with index ranges 0..-1 and 0..-1, i.e.
    both dimensions have length zero.

`#2:0:2()`
:   is a two-dimensional array with index ranges 0..-1 and 0..1, i.e. the
    first dimension has length zero, but the second has length 2.

`#0(12)`
:   is a rank-zero array with contents 12.

In addition, bytevectors are also arrays, but use a different syntax
(see [Bytevectors](#Bytevectors)):

`#vu8(1 2 3)`
:   is a 3-byte long bytevector, with contents 1, 2, 3.

---

Next: [Shared Arrays](#Shared-Arrays), Previous: [Array Syntax](#Array-Syntax), Up: [Arrays](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.13.2 Array Procedures [¶](#Array-Procedures-1)

When an array is created, the range of each dimension must be
specified, e.g., to create a 2x3 array with a zero-based index:

```
(make-array 'ho 2 3) ⇒ #2((ho ho ho) (ho ho ho))
```

The range of each dimension can also be given explicitly, e.g., another
way to create the same array:

```
(make-array 'ho '(0 1) '(0 2)) ⇒ #2((ho ho ho) (ho ho ho))
```

The following procedures can be used with arrays (or vectors). An
argument shown as idx… means one parameter for each
dimension in the array. A idxlist argument means a list of such
values, one for each dimension.

Scheme Procedure: **array?** obj [¶](#index-array_003f)

C Function: **scm\_array\_p** (obj, unused) [¶](#index-scm_005farray_005fp)
:   Return `#t` if the obj is an array, and `#f` if
    not.

    The second argument to scm\_array\_p is there for historical reasons,
    but it is not used. You should always pass `SCM_UNDEFINED` as
    its value.

Scheme Procedure: **typed-array?** obj type [¶](#index-typed_002darray_003f)

C Function: **scm\_typed\_array\_p** (obj, type) [¶](#index-scm_005ftyped_005farray_005fp)
:   Return `#t` if the obj is an array of type type, and
    `#f` if not.

C Function: `int` **scm\_is\_array** `(SCM obj)` [¶](#index-scm_005fis_005farray)
:   Return `1` if the obj is an array and `0` if not.

C Function: `int` **scm\_is\_typed\_array** `(SCM obj, SCM type)` [¶](#index-scm_005fis_005ftyped_005farray)
:   Return `0` if the obj is an array of type type, and
    `1` if not.

Scheme Procedure: **make-array** fill bound … [¶](#index-make_002darray)

C Function: **scm\_make\_array** (fill, bounds) [¶](#index-scm_005fmake_005farray)
:   Equivalent to `(make-typed-array #t fill bound ...)`.

Scheme Procedure: **make-typed-array** type fill bound … [¶](#index-make_002dtyped_002darray)

C Function: **scm\_make\_typed\_array** (type, fill, bounds) [¶](#index-scm_005fmake_005ftyped_005farray)
:   Create and return an array that has as many dimensions as there are
    bounds and (maybe) fill it with fill.

    The underlying storage vector is created according to type,
    which must be a symbol whose name is the ‘vectag’ of the array as
    explained above, or `#t` for ordinary, non-specialized arrays.

    For example, using the symbol `f64` for type will create an
    array that uses a `f64vector` for storing its elements, and
    `a` will use a string.

    When fill is not the special *unspecified* value, the new
    array is filled with fill. Otherwise, the initial contents of
    the array is unspecified. The special *unspecified* value is
    stored in the variable `*unspecified*` so that for example
    `(make-typed-array 'u32 *unspecified* 4)` creates a uninitialized
    `u32` vector of length 4.

    Each bound may be a positive non-zero integer n, in which
    case the index for that dimension can range from 0 through n-1; or
    an explicit index range specifier in the form `(LOWER UPPER)`,
    where both lower and upper are integers, possibly less than
    zero, and possibly the same number (however, lower cannot be
    greater than upper).

Scheme Procedure: **list->array** dimspec list [¶](#index-list_002d_003earray)
:   Equivalent to `(list->typed-array #t dimspec
    list)`.

Scheme Procedure: **list->typed-array** type dimspec list [¶](#index-list_002d_003etyped_002darray)

C Function: **scm\_list\_to\_typed\_array** (type, dimspec, list) [¶](#index-scm_005flist_005fto_005ftyped_005farray)
:   Return an array of the type indicated by type with elements the
    same as those of list.

    The argument dimspec determines the number of dimensions of the
    array and their lower bounds. When dimspec is an exact integer,
    it gives the number of dimensions directly and all lower bounds are
    zero. When it is a list of exact integers, then each element is the
    lower index bound of a dimension, and there will be as many dimensions
    as elements in the list.

Scheme Procedure: **array-type** array [¶](#index-array_002dtype)

C Function: **scm\_array\_type** (array) [¶](#index-scm_005farray_005ftype)
:   Return the type of array. This is the ‘vectag’ used for
    printing array (or `#t` for ordinary arrays) and can be
    used with `make-typed-array` to create an array of the same kind
    as array.

Scheme Procedure: **array-ref** array idx … [¶](#index-array_002dref)

C Function: **scm\_array\_ref** (array, idxlist) [¶](#index-scm_005farray_005fref)
:   Return the element at `(idx …)` in array.

    ```
    (define a (make-array 999 '(1 2) '(3 4)))
    (array-ref a 2 4) ⇒ 999
    ```

Scheme Procedure: **array-in-bounds?** array idx … [¶](#index-array_002din_002dbounds_003f)

C Function: **scm\_array\_in\_bounds\_p** (array, idxlist) [¶](#index-scm_005farray_005fin_005fbounds_005fp)
:   Return `#t` if the given indices would be acceptable to
    `array-ref`.

    ```
    (define a (make-array #f '(1 2) '(3 4)))
    (array-in-bounds? a 2 3) ⇒ #t
    (array-in-bounds? a 0 0) ⇒ #f
    ```

Scheme Procedure: **array-set!** array obj idx … [¶](#index-array_002dset_0021)

C Function: **scm\_array\_set\_x** (array, obj, idxlist) [¶](#index-scm_005farray_005fset_005fx)
:   Set the element at `(idx …)` in array to obj.
    The return value is unspecified.

    ```
    (define a (make-array #f '(0 1) '(0 1)))
    (array-set! a #t 1 1)
    a ⇒ #2((#f #f) (#f #t))
    ```

Scheme Procedure: **array-shape** array [¶](#index-array_002dshape)

Scheme Procedure: **array-dimensions** array [¶](#index-array_002ddimensions)

C Function: **scm\_array\_dimensions** (array) [¶](#index-scm_005farray_005fdimensions)
:   Return a list of the bounds for each dimension of array.

    `array-shape` gives `(lower upper)` for each
    dimension. `array-dimensions` instead returns just
    *upper+1* for dimensions with a 0 lower bound. Both are
    suitable as input to `make-array`.

    For example,

    ```
    (define a (make-array 'foo '(-1 3) 5))
    (array-shape a)      ⇒ ((-1 3) (0 4))
    (array-dimensions a) ⇒ ((-1 3) 5)
    ```

Scheme Procedure: **array-length** array [¶](#index-array_002dlength)

C Function: **scm\_array\_length** (array) [¶](#index-scm_005farray_005flength)

C Function: **size\_t** scm\_c\_array\_length (array) [¶](#index-size_005ft)
:   Return the length of an array: its first dimension. It is an error to
    ask for the length of an array of rank 0.

Scheme Procedure: **array-rank** array [¶](#index-array_002drank)

C Function: **scm\_array\_rank** (array) [¶](#index-scm_005farray_005frank)
:   Return the rank of array.

C Function: `size_t` **scm\_c\_array\_rank** `(SCM array)` [¶](#index-scm_005fc_005farray_005frank)
:   Return the rank of array as a `size_t`.

Scheme Procedure: **array->list** array [¶](#index-array_002d_003elist)

C Function: **scm\_array\_to\_list** (array) [¶](#index-scm_005farray_005fto_005flist)
:   Return a list consisting of all the elements, in order, of
    array.

Scheme Procedure: **array-copy!** src dst [¶](#index-array_002dcopy_0021)

Scheme Procedure: **array-copy-in-order!** src dst [¶](#index-array_002dcopy_002din_002dorder_0021)

C Function: **scm\_array\_copy\_x** (src, dst) [¶](#index-scm_005farray_005fcopy_005fx)
:   Copy every element from vector or array src to the corresponding
    element of dst. dst must have the same rank as src,
    and be at least as large in each dimension. The return value is
    unspecified.

Scheme Procedure: **array-fill!** array fill [¶](#index-array_002dfill_0021)

C Function: **scm\_array\_fill\_x** (array, fill) [¶](#index-scm_005farray_005ffill_005fx)
:   Store fill in every element of array. The value returned
    is unspecified.

Scheme Procedure: **array-equal?** array … [¶](#index-array_002dequal_003f)
:   Return `#t` if all arguments are arrays with the same shape, the
    same type, and have corresponding elements which are either
    `equal?` or `array-equal?`. This function differs from
    `equal?` (see [Equality](#Equality)) in that all arguments must be arrays.

Scheme Procedure: **array-map!** dst proc src … [¶](#index-array_002dmap_0021)

Scheme Procedure: **array-map-in-order!** dst proc src … [¶](#index-array_002dmap_002din_002dorder_0021)

C Function: **scm\_array\_map\_x** (dst, proc, srclist) [¶](#index-scm_005farray_005fmap_005fx)
:   Set each element of the dst array to values obtained from calls to
    proc. The list of src arguments may be empty. The value
    returned is unspecified.

    Each call is `(proc elem …)`, where each
    elem is from the corresponding src array, at the
    dst index. `array-map-in-order!` makes the calls in
    row-major order, `array-map!` makes them in an unspecified order.

    The src arrays must have the same number of dimensions as
    dst, and must have a range for each dimension which covers the
    range in dst. This ensures all dst indices are valid in
    each src.

Scheme Procedure: **array-for-each** proc src1 src2 … [¶](#index-array_002dfor_002deach)

C Function: **scm\_array\_for\_each** (proc, src1, srclist) [¶](#index-scm_005farray_005ffor_005feach)
:   Apply proc to each tuple of elements of src1 src2
    …, in row-major order. The value returned is unspecified.

Scheme Procedure: **array-index-map!** dst proc [¶](#index-array_002dindex_002dmap_0021)

C Function: **scm\_array\_index\_map\_x** (dst, proc) [¶](#index-scm_005farray_005findex_005fmap_005fx)
:   Set each element of the dst array to values returned by calls to
    proc. The value returned is unspecified.

    Each call is `(proc i1 … iN)`, where
    i1…iN is the destination index, one parameter for
    each dimension. The order in which the calls are made is unspecified.

    For example, to create a *4x4* matrix representing a
    cyclic group,

    ```
        / 0 1 2 3 \
        | 1 2 3 0 |
        | 2 3 0 1 |
        \ 3 0 1 2 /
    ```

    ```
    (define a (make-array #f 4 4))
    (array-index-map! a (lambda (i j)
                          (modulo (+ i j) 4)))
    ```

An additional array function is available in the module
`(ice-9 arrays)`. It can be used with:

```
(use-modules (ice-9 arrays))
```

Scheme Procedure: **array-copy** src [¶](#index-array_002dcopy)
:   Return a new array with the same elements, type and shape as
    src. However, the array increments may be different from those of
    src. In the current implementation, the returned array will be in
    row-major order, but that might change in the future. Use
    `array-copy!` on an array of known order if that is a concern.

---

Next: [Arrays as arrays of arrays](#Arrays-as-arrays-of-arrays), Previous: [Array Procedures](#Array-Procedures), Up: [Arrays](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.13.3 Shared Arrays [¶](#Shared-Arrays-1)

Scheme Procedure: **make-shared-array** oldarray mapfunc bound … [¶](#index-make_002dshared_002darray)

C Function: **scm\_make\_shared\_array** (oldarray, mapfunc, boundlist) [¶](#index-scm_005fmake_005fshared_005farray)
:   Return a new array which shares the storage of oldarray. Changes
    made through either array affect the same underlying storage. The
    bound … arguments are the shape of the new array, the same
    as `make-array` (see [Array Procedures](#Array-Procedures)).

    mapfunc translates coordinates from the new array to the
    oldarray. It’s called as `(mapfunc newidx1 …)`
    with one parameter for each dimension of the new array, and should
    return a list of indices for oldarray, one for each dimension of
    oldarray.

    mapfunc must be affine linear, meaning that each oldarray
    index must be formed by adding integer multiples (possibly negative)
    of some or all of newidx1 etc, plus a possible integer offset.
    The multiples and offset must be the same in each call.

      

    One good use for a shared array is to restrict the range of some
    dimensions, so as to apply say `array-for-each` or
    `array-fill!` to only part of an array. The plain `list`
    function can be used for mapfunc in this case, making no changes
    to the index values. For example,

    ```
    (make-shared-array #2((a b c) (d e f) (g h i)) list 3 2)
    ⇒ #2((a b) (d e) (g h))
    ```

    The new array can have fewer dimensions than oldarray, for
    example to take a column from an array.

    ```
    (make-shared-array #2((a b c) (d e f) (g h i))
                       (lambda (i) (list i 2))
                       '(0 2))
    ⇒ #1(c f i)
    ```

    A diagonal can be taken by using the single new array index for both
    row and column in the old array. For example,

    ```
    (make-shared-array #2((a b c) (d e f) (g h i))
                       (lambda (i) (list i i))
                       '(0 2))
    ⇒ #1(a e i)
    ```

    Dimensions can be increased by for instance considering portions of a
    one dimensional array as rows in a two dimensional array.
    (`array-contents` below can do the opposite, flattening an
    array.)

    ```
    (make-shared-array #1(a b c d e f g h i j k l)
                       (lambda (i j) (list (+ (* i 3) j)))
                       4 3)
    ⇒ #2((a b c) (d e f) (g h i) (j k l))
    ```

    By negating an index the order that elements appear can be reversed.
    The following just reverses the column order,

    ```
    (make-shared-array #2((a b c) (d e f) (g h i))
                       (lambda (i j) (list i (- 2 j)))
                       3 3)
    ⇒ #2((c b a) (f e d) (i h g))
    ```

    A fixed offset on indexes allows for instance a change from a 0 based
    to a 1 based array,

    ```
    (define x #2((a b c) (d e f) (g h i)))
    (define y (make-shared-array x
                                 (lambda (i j) (list (1- i) (1- j)))
                                 '(1 3) '(1 3)))
    (array-ref x 0 0) ⇒ a
    (array-ref y 1 1) ⇒ a
    ```

    A multiple on an index allows every Nth element of an array to be
    taken. The following is every third element,

    ```
    (make-shared-array #1(a b c d e f g h i j k l)
                       (lambda (i) (list (* i 3)))
                       4)
    ⇒ #1(a d g j)
    ```

    The above examples can be combined to make weird and wonderful
    selections from an array, but it’s important to note that because
    mapfunc must be affine linear, arbitrary permutations are not
    possible.

    In the current implementation, mapfunc is not called for every
    access to the new array but only on some sample points to establish a
    base and stride for new array indices in oldarray data. A few
    sample points are enough because mapfunc is linear.

Scheme Procedure: **shared-array-increments** array [¶](#index-shared_002darray_002dincrements)

C Function: **scm\_shared\_array\_increments** (array) [¶](#index-scm_005fshared_005farray_005fincrements)
:   For each dimension, return the distance between elements in the root vector.

Scheme Procedure: **shared-array-offset** array [¶](#index-shared_002darray_002doffset)

C Function: **scm\_shared\_array\_offset** (array) [¶](#index-scm_005fshared_005farray_005foffset)
:   Return the root vector index of the first element in the array.

Scheme Procedure: **shared-array-root** array [¶](#index-shared_002darray_002droot)

C Function: **scm\_shared\_array\_root** (array) [¶](#index-scm_005fshared_005farray_005froot)
:   Return the root vector of a shared array.

Scheme Procedure: **array-contents** array [strict] [¶](#index-array_002dcontents)

C Function: **scm\_array\_contents** (array, strict) [¶](#index-scm_005farray_005fcontents)
:   If array may be *unrolled* into a one dimensional shared array
    without changing their order (last subscript changing fastest), then
    `array-contents` returns that shared array, otherwise it returns
    `#f`. All arrays made by `make-array` and
    `make-typed-array` may be unrolled, some arrays made by
    `make-shared-array` may not be.

    If the optional argument strict is provided, a shared array will
    be returned only if its elements are stored internally contiguous in
    memory.

Scheme Procedure: **transpose-array** array dim1 dim2 … [¶](#index-transpose_002darray)

C Function: **scm\_transpose\_array** (array, dimlist) [¶](#index-scm_005ftranspose_005farray)
:   Return an array sharing contents with array, but with
    dimensions arranged in a different order. There must be one
    dim argument for each dimension of array.
    dim1, dim2, … should be integers between 0
    and the rank of the array to be returned. Each integer in that
    range must appear at least once in the argument list.

    The values of dim1, dim2, … correspond to
    dimensions in the array to be returned, and their positions in the
    argument list to dimensions of array. Several dims
    may have the same value, in which case the returned array will
    have smaller rank than array.

    ```
    (transpose-array '#2((a b) (c d)) 1 0) ⇒ #2((a c) (b d))
    (transpose-array '#2((a b) (c d)) 0 0) ⇒ #1(a d)
    (transpose-array '#3(((a b c) (d e f)) ((1 2 3) (4 5 6))) 1 1 0) ⇒
                    #2((a 4) (b 5) (c 6))
    ```

---

Next: [Accessing Arrays from C](#Accessing-Arrays-from-C), Previous: [Shared Arrays](#Shared-Arrays), Up: [Arrays](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.13.4 Arrays as arrays of arrays [¶](#Arrays-as-arrays-of-arrays-1)

One can see an array of rank *n* (an
*n*-array) as an array of lower rank where the elements are
themselves arrays (‘cells’).

We speak of the first *n-k* dimensions of the array as the
*n-k*-‘frame’ of the array, while the last *k* dimensions are
the dimensions of the *k*-‘cells’. For example, a 3-array can be
seen as a 2-array of vectors (1-arrays) or as a 1-array of matrices
(2-arrays). In each case, the vectors or matrices are the 1-cells or
2-cells of the array. This terminology originates in the J language.

The more vague concept of a ‘slice’ refers to a subset of the array
where some indices are fixed and others are left free. As a Guile data
object, a cell is the same as a ‘prefix slice’ (the first *n-k*
indices into the original array are fixed), except that a 0-cell is not
a shared array of the original array, but a 0-slice (where all the
indices into the original array are fixed) is.

Before version 2.0, Guile had a feature called ‘enclosed arrays’ to
create special ‘array of arrays’ objects. The functions in this section
do not need special types; instead, the frame rank is given in the
arguments, either implicitly (through the number of indices) or
explicitly.

Scheme Procedure: **array-cell-ref** array idx … [¶](#index-array_002dcell_002dref)
