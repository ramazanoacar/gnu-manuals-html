C Function: **scm\_array\_cell\_ref** (array, idxlist) [¶](#index-scm_005farray_005fcell_005fref)
:   If the length of idxlist equals the rank *n* of array,
    return the element at `(idx …)`, just like `(array-ref
    array idx …)`. If, however, the length *k* of idxlist
    is smaller than *n*, then return the *(n-k)*-cell of
    array given by idxlist, as a shared array.

    For example:

    ```
    (array-cell-ref #2((a b) (c d)) 0) ⇒ #1(a b)
    (array-cell-ref #2((a b) (c d)) 1) ⇒ #1(c d)
    (array-cell-ref #2((a b) (c d)) 1 1) ⇒ d
    (array-cell-ref #2((a b) (c d))) ⇒ #2((a b) (c d))
    ```

    `(apply array-cell-ref array indices)` is equivalent to

    ```
    (let ((len (length indices)))
      (if (= (array-rank a) len)
        (apply array-ref a indices)
        (apply make-shared-array a
               (lambda t (append indices t))
               (drop (array-dimensions a) len))))
    ```

Scheme Procedure: **array-slice** array idx … [¶](#index-array_002dslice)

C Function: **scm\_array\_slice** (array, idxlist) [¶](#index-scm_005farray_005fslice)
:   Like `(array-cell-ref array idx …)`, but return a 0-rank
    shared array into ARRAY if the length of idxlist matches the
    rank of array. This can be useful when using ARRAY as a
    place to write to.

    Compare:

    ```
    (array-cell-ref #2((a b) (c d)) 1 1) ⇒ d
    (array-slice #2((a b) (c d)) 1 1) ⇒ #0(d)
    (define a (make-array 'a 2 2))
    (array-fill! (array-slice a 1 1) 'b)
    a ⇒ #2((a a) (a b)).
    (array-fill! (array-cell-ref a 1 1) 'b) ⇒ error: not an array
    ```

    `(apply array-slice array indices)` is equivalent to

    ```
    (apply make-shared-array a
      (lambda t (append indices t))
      (drop (array-dimensions a) (length indices)))
    ```

Scheme Procedure: **array-cell-set!** array x idx … [¶](#index-array_002dcell_002dset_0021)

C Function: **scm\_array\_cell\_set\_x** (array, x, idxlist) [¶](#index-scm_005farray_005fcell_005fset_005fx)
:   If the length of idxlist equals the rank *n* of
    array, set the element at `(idx …)` of array to
    x, just like `(array-set! array x idx …)`. If,
    however, the length *k* of idxlist is smaller than
    *n*, then copy the *(n-k)*-rank array x
    into the *(n-k)*-cell of array given by
    idxlist. In this case, the last *(n-k)* dimensions of
    array and the dimensions of x must match exactly.

    This function returns the modified array.

    For example:

    ```
    (array-cell-set! (make-array 'a 2 2) 'b 1 1)
      ⇒ #2((a a) (a b))
    (array-cell-set! (make-array 'a 2 2) #(x y) 1)
      ⇒ #2((a a) (x y))
    ```

    Note that `array-cell-set!` expects elements, not arrays, when
    the destination has rank 0. Use `array-slice` for the opposite
    behavior.

    ```
    (array-cell-set! (make-array 'a 2 2) #0(b) 1 1)
      ⇒ #2((a a) (a #0(b)))
    (let ((a (make-array 'a 2 2)))
      (array-copy! #0(b) (array-slice a 1 1)) a)
      ⇒ #2((a a) (a b))
    ```

    `(apply array-cell-set! array x indices)` is equivalent to

    ```
    (let ((len (length indices)))
      (if (= (array-rank array) len)
        (apply array-set! array x indices)
        (array-copy! x (apply array-cell-ref array indices)))
      array)
    ```

Scheme Procedure: **array-slice-for-each** frame-rank op x … [¶](#index-array_002dslice_002dfor_002deach)

C Function: **scm\_array\_slice\_for\_each** (array, frame\_rank, op, xlist) [¶](#index-scm_005farray_005fslice_005ffor_005feach)
:   Each x must be an array of rank ≥ frame-rank, and
    the first frame-rank dimensions of each x must all be the
    same. array-slice-for-each calls op with each set of
    (rank(x) - frame-rank)-cells from x, in unspecified order.

    array-slice-for-each allows you to loop over cells of any rank
    without having to carry an index list or construct shared arrays
    manually. The slices passed to op are always shared arrays of
    X, even if they are of rank 0, so it is possible to write to them.

    This function returns an unspecified value.

    For example, to sort each row of rank-2 array `a`:

    ```
    (array-slice-for-each 1 (lambda (x) (sort! x <)) a)
    ```

    As another example, let `a` be a rank-2 array where each row is a
    2-element vector *(x,y)*. Let’s compute the arguments of these
    vectors and store them in rank-1 array `b`.

    ```
    (array-slice-for-each 1
      (lambda (a b)
        (array-set! b (atan (array-ref a 1) (array-ref a 0))))
      a b)
    ```

    `(apply array-slice-for-each frame-rank op x)` is equivalent to

    ```
    (let ((frame (take (array-dimensions (car x)) frank)))
      (unless (every (lambda (x)
                       (equal? frame (take (array-dimensions x) frank)))
                     (cdr x))
        (error))
      (array-index-map!
        (apply make-shared-array (make-array #t) (const '()) frame)
        (lambda i (apply op (map (lambda (x) (apply array-slice x i)) x)))))
    ```

Scheme Procedure: **array-slice-for-each-in-order** frame-rank op x … [¶](#index-array_002dslice_002dfor_002deach_002din_002dorder)

C Function: **scm\_array\_slice\_for\_each\_in\_order** (array, frame\_rank, op, xlist) [¶](#index-scm_005farray_005fslice_005ffor_005feach_005fin_005forder)
:   Same as `array-slice-for-each`, but the arguments are traversed
    sequentially and in row-major order.

---

Previous: [Arrays as arrays of arrays](#Arrays-as-arrays-of-arrays), Up: [Arrays](#Arrays)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.13.5 Accessing Arrays from C [¶](#Accessing-Arrays-from-C-1)

For interworking with external C code, Guile provides an API to allow C
code to access the elements of a Scheme array. In particular, for
uniform numeric arrays, the API exposes the underlying uniform data as a
C array of numbers of the relevant type.

While pointers to the elements of an array are in use, the array itself
must be protected so that the pointer remains valid. Such a protected
array is said to be *reserved*. A reserved array can be read but
modifications to it that would cause the pointer to its elements to
become invalid are prevented. When you attempt such a modification, an
error is signaled.

(This is similar to locking the array while it is in use, but without
the danger of a deadlock. In a multi-threaded program, you will need
additional synchronization to avoid modifying reserved arrays.)

You must take care to always unreserve an array after reserving it,
even in the presence of non-local exits. If a non-local exit can
happen between these two calls, you should install a dynwind context
that releases the array when it is left (see [Dynamic Wind](#Dynamic-Wind)).

In addition, array reserving and unreserving must be properly
paired. For instance, when reserving two or more arrays in a certain
order, you need to unreserve them in the opposite order.

Once you have reserved an array and have retrieved the pointer to its
elements, you must figure out the layout of the elements in memory.
Guile allows slices to be taken out of arrays without actually making a
copy, such as making an alias for the diagonal of a matrix that can be
treated as a vector. Arrays that result from such an operation are not
stored contiguously in memory and when working with their elements
directly, you need to take this into account.

The layout of array elements in memory can be defined via a
*mapping function* that computes a scalar position from a vector of
indices. The scalar position then is the offset of the element with the
given indices from the start of the storage block of the array.

In Guile, this mapping function is restricted to be *affine*: all
mapping functions of Guile arrays can be written as `p = b +
c[0]*i[0] + c[1]*i[1] + ... + c[n-1]*i[n-1]` where `i[k]` is the
`k`th index and `n` is the rank of the array. For
example, a matrix of size 3x3 would have `b == 0`, `c[0] ==
3` and `c[1] == 1`. When you transpose this matrix (with
`transpose-array`, say), you will get an array whose mapping
function has `b == 0`, `c[0] == 1` and `c[1] == 3`.

The function `scm_array_handle_dims` gives you (indirect) access to
the coefficients `c[k]`.

Note that there are no functions for accessing the elements of a
character array yet. Once the string implementation of Guile has been
changed to use Unicode, we will provide them.

C Type: **scm\_t\_array\_handle** [¶](#index-scm_005ft_005farray_005fhandle)
:   This is a structure type that holds all information necessary to manage
    the reservation of arrays as explained above. Structures of this type
    must be allocated on the stack and must only be accessed by the
    functions listed below.

C Function: `void` **scm\_array\_get\_handle** `(SCM array, scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fget_005fhandle)
:   Reserve array, which must be an array, and prepare handle to
    be used with the functions below. You must eventually call
    `scm_array_handle_release` on handle, and do this in a
    properly nested fashion, as explained above. The structure pointed to
    by handle does not need to be initialized before calling this
    function.

C Function: `void` **scm\_array\_handle\_release** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005frelease)
:   End the array reservation represented by handle. After a call to
    this function, handle might be used for another reservation.

C Function: `size_t` **scm\_array\_handle\_rank** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005frank)
:   Return the rank of the array represented by handle.

C Type: **scm\_t\_array\_dim** [¶](#index-scm_005ft_005farray_005fdim)
:   This structure type holds information about the layout of one dimension
    of an array. It includes the following fields:

    `ssize_t lbnd`

    `ssize_t ubnd`
    :   The lower and upper bounds (both inclusive) of the permissible index
        range for the given dimension. Both values can be negative, but
        lbnd is always less than or equal to ubnd.

    `ssize_t inc`
    :   The distance from one element of this dimension to the next. Note, too,
        that this can be negative.

C Function: `const scm_t_array_dim *` **scm\_array\_handle\_dims** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fdims)
:   Return a pointer to a C vector of information about the dimensions of
    the array represented by handle. This pointer is valid as long as
    the array remains reserved. As explained above, the
    `scm_t_array_dim` structures returned by this function can be used
    calculate the position of an element in the storage block of the array
    from its indices.

    This position can then be used as an index into the C array pointer
    returned by the various `scm_array_handle_<foo>_elements`
    functions, or with `scm_array_handle_ref` and
    `scm_array_handle_set`.

    Here is how one can compute the position pos of an element given
    its indices in the vector indices:

    ```
    ssize_t indices[RANK];
    scm_t_array_dim *dims;
    ssize_t pos;
    size_t i;

    pos = 0;
    for (i = 0; i < RANK; i++)
      {
        if (indices[i] < dims[i].lbnd || indices[i] > dims[i].ubnd)
          out_of_range ();
        pos += (indices[i] - dims[i].lbnd) * dims[i].inc;
      }
    ```

C Function: `ssize_t` **scm\_array\_handle\_pos** `(scm_t_array_handle *handle, SCM indices)` [¶](#index-scm_005farray_005fhandle_005fpos)
:   Compute the position corresponding to indices, a list of
    indices. The position is computed as described above for
    `scm_array_handle_dims`. The number of the indices and their
    range is checked and an appropriate error is signaled for invalid
    indices.

C Function: `SCM` **scm\_array\_handle\_ref** `(scm_t_array_handle *handle, ssize_t pos)` [¶](#index-scm_005farray_005fhandle_005fref)
:   Return the element at position pos in the storage block of the
    array represented by handle. Any kind of array is acceptable. No
    range checking is done on pos.

C Function: `void` **scm\_array\_handle\_set** `(scm_t_array_handle *handle, ssize_t pos, SCM val)` [¶](#index-scm_005farray_005fhandle_005fset)
:   Set the element at position pos in the storage block of the array
    represented by handle to val. Any kind of array is
    acceptable. No range checking is done on pos. An error is
    signaled when the array can not store val.

C Function: `const SCM *` **scm\_array\_handle\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005felements)
:   Return a pointer to the elements of a ordinary array of general Scheme
    values (i.e., a non-uniform array) for reading. This pointer is valid
    as long as the array remains reserved.

C Function: `SCM *` **scm\_array\_handle\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fwritable_005felements)
:   Like `scm_array_handle_elements`, but the pointer is good for
    reading and writing.

C Function: `const void *` **scm\_array\_handle\_uniform\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005funiform_005felements)
:   Return a pointer to the elements of a uniform numeric array for reading.
    This pointer is valid as long as the array remains reserved. The size
    of each element is given by `scm_array_handle_uniform_element_size`.

C Function: `void *` **scm\_array\_handle\_uniform\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005funiform_005fwritable_005felements)
:   Like `scm_array_handle_uniform_elements`, but the pointer is good
    reading and writing.

C Function: `size_t` **scm\_array\_handle\_uniform\_element\_size** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005funiform_005felement_005fsize)
:   Return the size of one element of the uniform numeric array represented
    by handle.

C Function: `const scm_t_uint8 *` **scm\_array\_handle\_u8\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fu8_005felements)

C Function: `const scm_t_int8 *` **scm\_array\_handle\_s8\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fs8_005felements)

C Function: `const scm_t_uint16 *` **scm\_array\_handle\_u16\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fu16_005felements)

C Function: `const scm_t_int16 *` **scm\_array\_handle\_s16\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fs16_005felements)

C Function: `const scm_t_uint32 *` **scm\_array\_handle\_u32\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fu32_005felements)

C Function: `const scm_t_int32 *` **scm\_array\_handle\_s32\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fs32_005felements)

C Function: `const scm_t_uint64 *` **scm\_array\_handle\_u64\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fu64_005felements)

C Function: `const scm_t_int64 *` **scm\_array\_handle\_s64\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fs64_005felements)

C Function: `const float *` **scm\_array\_handle\_f32\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005ff32_005felements)

C Function: `const double *` **scm\_array\_handle\_f64\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005ff64_005felements)

C Function: `const float *` **scm\_array\_handle\_c32\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fc32_005felements)

C Function: `const double *` **scm\_array\_handle\_c64\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fc64_005felements)
:   Return a pointer to the elements of a uniform numeric array of the
    indicated kind for reading. This pointer is valid as long as the array
    remains reserved.

    The pointers for `c32` and `c64` uniform numeric arrays point
    to pairs of floating point numbers. The even index holds the real part,
    the odd index the imaginary part of the complex number.

C Function: `scm_t_uint8 *` **scm\_array\_handle\_u8\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fu8_005fwritable_005felements)

C Function: `scm_t_int8 *` **scm\_array\_handle\_s8\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fs8_005fwritable_005felements)

C Function: `scm_t_uint16 *` **scm\_array\_handle\_u16\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fu16_005fwritable_005felements)

C Function: `scm_t_int16 *` **scm\_array\_handle\_s16\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fs16_005fwritable_005felements)

C Function: `scm_t_uint32 *` **scm\_array\_handle\_u32\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fu32_005fwritable_005felements)

C Function: `scm_t_int32 *` **scm\_array\_handle\_s32\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fs32_005fwritable_005felements)

C Function: `scm_t_uint64 *` **scm\_array\_handle\_u64\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fu64_005fwritable_005felements)

C Function: `scm_t_int64 *` **scm\_array\_handle\_s64\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fs64_005fwritable_005felements)

C Function: `float *` **scm\_array\_handle\_f32\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005ff32_005fwritable_005felements)

C Function: `double *` **scm\_array\_handle\_f64\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005ff64_005fwritable_005felements)

C Function: `float *` **scm\_array\_handle\_c32\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fc32_005fwritable_005felements)

C Function: `double *` **scm\_array\_handle\_c64\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fc64_005fwritable_005felements)
:   Like `scm_array_handle_<kind>_elements`, but the pointer is good
    for reading and writing.

C Function: `const scm_t_uint32 *` **scm\_array\_handle\_bit\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fbit_005felements)
:   Return a pointer to the words that store the bits of the represented
    array, which must be a bit array.

    Unlike other arrays, bit arrays have an additional offset that must be
    figured into index calculations. That offset is returned by
    `scm_array_handle_bit_elements_offset`.

    To find a certain bit you first need to calculate its position as
    explained above for `scm_array_handle_dims` and then add the
    offset. This gives the absolute position of the bit, which is always a
    non-negative integer.

    Each word of the bit array storage block contains exactly 32 bits, with
    the least significant bit in that word having the lowest absolute
    position number. The next word contains the next 32 bits.

    Thus, the following code can be used to access a bit whose position
    according to `scm_array_handle_dims` is given in pos:

    ```
    SCM bit_array;
    scm_t_array_handle handle;
    scm_t_uint32 *bits;
    ssize_t pos;
    size_t abs_pos;
    size_t word_pos, mask;

    scm_array_get_handle (&bit_array, &handle);
    bits = scm_array_handle_bit_elements (&handle);

    pos = ...
    abs_pos = pos + scm_array_handle_bit_elements_offset (&handle);
    word_pos = abs_pos / 32;
    mask = 1L << (abs_pos % 32);

    if (bits[word_pos] & mask)
      /* bit is set. */

    scm_array_handle_release (&handle);
    ```

C Function: `scm_t_uint32 *` **scm\_array\_handle\_bit\_writable\_elements** `(scm_t_array_handle *handle)` [¶](#index-scm_005farray_005fhandle_005fbit_005fwritable_005felements)
:   Like `scm_array_handle_bit_elements` but the pointer is good for
    reading and writing. You must take care not to modify bits outside of
    the allowed index range of the array, even for contiguous arrays.

---

Next: [Record Overview](#Record-Overview), Previous: [Arrays](#Arrays), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.14 VLists [¶](#VLists-1)

The `(ice-9 vlist)` module provides an implementation of the *VList*
data structure designed by Phil Bagwell in 2002. VLists are immutable lists,
which can contain any Scheme object. They improve on standard Scheme linked
lists in several areas:

* Random access has typically constant-time complexity.
* Computing the length of a VList has time complexity logarithmic in the number of
  elements.
* VLists use less storage space than standard lists.
* VList elements are stored in contiguous regions, which improves memory locality
  and leads to more efficient use of hardware caches.

The idea behind VLists is to store vlist elements in increasingly large
contiguous blocks (implemented as vectors here). These blocks are linked to one
another using a pointer to the next block and an offset within that block. The
size of these blocks form a geometric series with ratio
`block-growth-factor` (2 by default).

The VList structure also serves as the basis for the *VList-based hash
lists* or “vhashes”, an immutable dictionary type (see [VList-Based Hash Lists or “VHashes”](#VHashes)).

However, the current implementation in `(ice-9 vlist)` has several
noteworthy shortcomings:

* It is *not* thread-safe. Although operations on vlists are all
  *referentially transparent* (i.e., purely functional), adding elements to a
  vlist with `vlist-cons` mutates part of its internal structure, which makes
  it non-thread-safe. This could be fixed, but it would slow down
  `vlist-cons`.
* `vlist-cons` always allocates at least as much memory as `cons`.
  Again, Phil Bagwell describes how to fix it, but that would require tuning the
  garbage collector in a way that may not be generally beneficial.
* `vlist-cons` is a Scheme procedure compiled to bytecode, and it does not
  compete with the straightforward C implementation of `cons`, and with the
  fact that the VM has a special `cons` instruction.

We hope to address these in the future.

The programming interface exported by `(ice-9 vlist)` is defined below.
Most of it is the same as SRFI-1 with an added `vlist-` prefix to function
names.

Scheme Procedure: **vlist?** obj [¶](#index-vlist_003f)
:   Return true if obj is a VList.

Scheme Variable: **vlist-null** [¶](#index-vlist_002dnull)
:   The empty VList. Note that it’s possible to create an empty VList not
    `eq?` to `vlist-null`; thus, callers should always use
    `vlist-null?` when testing whether a VList is empty.

Scheme Procedure: **vlist-null?** vlist [¶](#index-vlist_002dnull_003f)
:   Return true if vlist is empty.

Scheme Procedure: **vlist-cons** item vlist [¶](#index-vlist_002dcons)
:   Return a new vlist with item as its head and vlist as its tail.

Scheme Procedure: **vlist-head** vlist [¶](#index-vlist_002dhead)
:   Return the head of vlist.

Scheme Procedure: **vlist-tail** vlist [¶](#index-vlist_002dtail)
:   Return the tail of vlist.

Scheme Variable: **block-growth-factor** [¶](#index-block_002dgrowth_002dfactor)
:   A fluid that defines the growth factor of VList blocks, 2 by default.

The functions below provide the usual set of higher-level list operations.

Scheme Procedure: **vlist-fold** proc init vlist [¶](#index-vlist_002dfold)

Scheme Procedure: **vlist-fold-right** proc init vlist [¶](#index-vlist_002dfold_002dright)
:   Fold over vlist, calling proc for each element, as for SRFI-1
    `fold` and `fold-right` (see [`fold`](#SRFI_002d1)).

Scheme Procedure: **vlist-ref** vlist index [¶](#index-vlist_002dref)
:   Return the element at index index in vlist. This is typically a
    constant-time operation.

Scheme Procedure: **vlist-length** vlist [¶](#index-vlist_002dlength)
:   Return the length of vlist. This is typically logarithmic in the number
    of elements in vlist.

Scheme Procedure: **vlist-reverse** vlist [¶](#index-vlist_002dreverse)
:   Return a new vlist whose content are those of vlist in reverse
    order.

Scheme Procedure: **vlist-map** proc vlist [¶](#index-vlist_002dmap)
:   Map proc over the elements of vlist and return a new vlist.

Scheme Procedure: **vlist-for-each** proc vlist [¶](#index-vlist_002dfor_002deach)
:   Call proc on each element of vlist. The result is unspecified.

Scheme Procedure: **vlist-drop** vlist count [¶](#index-vlist_002ddrop)
:   Return a new vlist that does not contain the count first elements of
    vlist. This is typically a constant-time operation.

Scheme Procedure: **vlist-take** vlist count [¶](#index-vlist_002dtake)
:   Return a new vlist that contains only the count first elements of
    vlist.

Scheme Procedure: **vlist-filter** pred vlist [¶](#index-vlist_002dfilter)
:   Return a new vlist containing all the elements from vlist that satisfy
    pred.

Scheme Procedure: **vlist-delete** x vlist [equal?] [¶](#index-vlist_002ddelete)
:   Return a new vlist corresponding to vlist without the elements
    equal? to x.

Scheme Procedure: **vlist-unfold** p f g seed [tail-gen] [¶](#index-vlist_002dunfold)

Scheme Procedure: **vlist-unfold-right** p f g seed [tail] [¶](#index-vlist_002dunfold_002dright)
:   Return a new vlist, as for SRFI-1 `unfold` and `unfold-right`
    (see [`unfold`](#SRFI_002d1)).

Scheme Procedure: **vlist-append** vlist … [¶](#index-vlist_002dappend)
:   Append the given vlists and return the resulting vlist.

Scheme Procedure: **list->vlist** lst [¶](#index-list_002d_003evlist)
:   Return a new vlist whose contents correspond to lst.

Scheme Procedure: **vlist->list** vlist [¶](#index-vlist_002d_003elist)
:   Return a new list whose contents match those of vlist.

---

Next: [SRFI-9 Records](#SRFI_002d9-Records), Previous: [VLists](#VLists), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.15 Record Overview [¶](#Record-Overview-1)

*Records*, also called *structures*, are Scheme’s primary
mechanism to define new disjoint types. A *record type* defines a
list of *fields* that instances of the type consist of. This is like
C’s `struct`.

Historically, Guile has offered several different ways to define record
types and to create records, offering different features, and making
different trade-offs. Over the years, each “standard” has also come
with its own new record interface, leading to a maze of record APIs.

At the highest level is SRFI-9, a high-level record interface
implemented by most Scheme implementations (see [SRFI-9 Records](#SRFI_002d9-Records)). It
defines a simple and efficient syntactic abstraction of record types and
their associated type predicate, fields, and field accessors. SRFI-9 is
suitable for most uses, and this is the recommended way to create record
types in Guile. Similar high-level record APIs include SRFI-35
(see [SRFI-35 - Conditions](#SRFI_002d35)) and R6RS records (see [rnrs records syntactic](#rnrs-records-syntactic)).

Then comes Guile’s historical “records” API (see [Records](#Records)). Record
types defined this way are first-class objects. Introspection
facilities are available, allowing users to query the list of fields or
the value of a specific field at run-time, without prior knowledge of
the type.

Finally, the common denominator of these interfaces is Guile’s
*structure* API (see [Structures](#Structures)). Guile’s structures are the
low-level building block for all other record APIs. Application writers
will normally not need to use it.

Records created with these APIs may all be pattern-matched using Guile’s
standard pattern matcher (see [Pattern Matching](#Pattern-Matching)).

---

Next: [Records](#Records), Previous: [Record Overview](#Record-Overview), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.16 SRFI-9 Records [¶](#SRFI_002d9-Records-1)

SRFI-9 standardizes a syntax for defining new record types and creating
predicate, constructor, and field getter and setter functions. In Guile
this is the recommended option to create new record types (see [Record Overview](#Record-Overview)). It can be used with:

```
(use-modules (srfi srfi-9))
```

Scheme Syntax: **define-record-type** type (constructor fieldname …) predicate (fieldname accessor [modifier]) … [¶](#index-define_002drecord_002dtype)
:   Create a new record type, and make various `define`s for using
    it. This syntax can only occur at the top-level, not nested within
    some other form.

    type is bound to the record type, which is as per the return
    from the core `make-record-type`. type also provides the
    name for the record, as per `record-type-name`.

    constructor is bound to a function to be called as
    `(constructor fieldval …)` to create a new record of
    this type. The arguments are initial values for the fields, one
    argument for each field, in the order they appear in the
    `define-record-type` form.

    The fieldnames provide the names for the record fields, as per
    the core `record-type-fields` etc, and are referred to in the
    subsequent accessor/modifier forms.

    predicate is bound to a function to be called as
    `(predicate obj)`. It returns `#t` or `#f`
    according to whether obj is a record of this type.

    Each accessor is bound to a function to be called
    `(accessor record)` to retrieve the respective field from a
    record. Similarly each modifier is bound to a function to
    be called `(modifier record val)` to set the respective
    field in a record.

An example will illustrate typical usage,

```
(define-record-type <employee>
  (make-employee name age salary)
  employee?
  (name    employee-name)
  (age     employee-age    set-employee-age!)
  (salary  employee-salary set-employee-salary!))
```

This creates a new employee data type, with name, age and salary
fields. Accessor functions are created for each field, but no
modifier function for the name (the intention in this example being
that it’s established only when an employee object is created). These
can all then be used as for example,

```
<employee> ⇒ #<record-type <employee>>

(define fred (make-employee "Fred" 45 20000.00))

(employee? fred)        ⇒ #t
(employee-age fred)     ⇒ 45
(set-employee-salary! fred 25000.00)  ;; pay rise
```

The functions created by `define-record-type` are ordinary
top-level `define`s. They can be redefined or `set!` as
desired, exported from a module, etc.

* [Non-toplevel Record Definitions](#Non_002dtoplevel-Record-Definitions)
* [Custom Printers](#Custom-Printers)
* [Functional “Setters”](#Functional-_0060_0060Setters_0027_0027)

#### Non-toplevel Record Definitions [¶](#Non_002dtoplevel-Record-Definitions)

The SRFI-9 specification explicitly disallows record definitions in a
non-toplevel context, such as inside `lambda` body or inside a
let block. However, Guile’s implementation does not enforce that
restriction.

#### Custom Printers [¶](#Custom-Printers)

You may use `set-record-type-printer!` to customize the default printing
behavior of records. This is a Guile extension and is not part of SRFI-9. It
is located in the `(srfi srfi-9 gnu)` module.

Scheme Syntax: **set-record-type-printer!** type proc [¶](#index-set_002drecord_002dtype_002dprinter_0021)
:   Where type corresponds to the first argument of `define-record-type`,
    and proc is a procedure accepting two arguments, the record to print, and
    an output port.

This example prints the employee’s name in brackets, for instance `[Fred]`.

```
(set-record-type-printer! <employee>
  (lambda (record port)
    (write-char #\[ port)
    (display (employee-name record) port)
    (write-char #\] port)))
```

#### Functional “Setters” [¶](#Functional-_0060_0060Setters_0027_0027)

When writing code in a functional style, it is desirable to never alter
the contents of records. For such code, a simple way to return new
record instances based on existing ones is highly desirable.

The `(srfi srfi-9 gnu)` module extends SRFI-9 with facilities to
return new record instances based on existing ones, only with one or
more field values changed—*functional setters*. First, the
`define-immutable-record-type` works like
`define-record-type`, except that fields are immutable and setters
are defined as functional setters.

Scheme Syntax: **define-immutable-record-type** type (constructor fieldname …) predicate (fieldname accessor [modifier]) … [¶](#index-define_002dimmutable_002drecord_002dtype)
:   Define type as a new record type, like `define-record-type`.
    However, the record type is made *immutable* (records may not be
    mutated, even with `struct-set!`), and any modifier is
    defined to be a functional setter—a procedure that returns a new
    record instance with the specified field changed, and leaves the
    original unchanged (see example below.)

In addition, the generic `set-field` and `set-fields` macros
may be applied to any SRFI-9 record.

Scheme Syntax: **set-field** record (field sub-fields ...) value [¶](#index-set_002dfield)
:   Return a new record of record’s type whose fields are equal to
    the corresponding fields of record except for the one specified by
    field.

    field must be the name of the getter corresponding to the field of
    record being “set”. Subsequent sub-fields must be record
    getters designating sub-fields within that field value to be set (see
    example below.)

Scheme Syntax: **set-fields** record ((field sub-fields ...) value) ... [¶](#index-set_002dfields)
:   Like `set-field`, but can be used to set more than one field at a
    time. This expands to code that is more efficient than a series of
    single `set-field` calls.

To illustrate the use of functional setters, let’s assume these two
record type definitions:

```
(define-record-type <address>
  (address street city country)
  address?
  (street  address-street)
  (city    address-city)
  (country address-country))

(define-immutable-record-type <person>
  (person age email address)
  person?
  (age     person-age set-person-age)
  (email   person-email set-person-email)
  (address person-address set-person-address))
```

First, note that the `<person>` record type definition introduces
named functional setters. These may be used like this:

```
(define fsf-address
  (address "Franklin Street" "Boston" "USA"))

(define rms
  (person 30 "rms@gnu.org" fsf-address))

(and (equal? (set-person-age rms 60)
             (person 60 "rms@gnu.org" fsf-address))
     (= (person-age rms) 30))
⇒ #t
```

Here, the original `<person>` record, to which rms is bound,
is left unchanged.

Now, suppose we want to change both the street and age of rms.
This can be achieved using `set-fields`:

```
(set-fields rms
  ((person-age) 60)
  ((person-address address-street) "Temple Place"))
⇒ #<<person> age: 60 email: "rms@gnu.org"
  address: #<<address> street: "Temple Place" city: "Boston" country: "USA">>
```

Notice how the above changed two fields of rms, including the
`street` field of its `address` field, in a concise way. Also
note that `set-fields` works equally well for types defined with
just `define-record-type`.

---

Next: [Structures](#Structures), Previous: [SRFI-9 Records](#SRFI_002d9-Records), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.17 Records [¶](#Records-1)

A *record type* is a first class object representing a user-defined
data type. A *record* is an instance of a record type.

Note that in many ways, this interface is too low-level for every-day
use. Most uses of records are better served by SRFI-9 records.
See [SRFI-9 Records](#SRFI_002d9-Records).

Scheme Procedure: **record?** obj [¶](#index-record_003f)
:   Return `#t` if obj is a record of any type and `#f`
    otherwise.

    Note that `record?` may be true of any Scheme value; there is no
    promise that records are disjoint with other Scheme types.

Scheme Procedure: **make-record-type** type-name field-names [print] [#:parent=`#f`] [#:uid=`#f`] [#:extensible?=`#f`] [#:opaque?=`#f`] [#:allow-duplicate-field-names?=`#t`] [¶](#index-make_002drecord_002dtype)
:   Create and return a new *record-type descriptor*.

    type-name is a string naming the type. Currently it’s only used
    in the printed representation of records, and in diagnostics.
    field-names is a list of elements of the form `(immutable
    name)`, `(mutable name)`, or name, where
    name are symbols naming the fields of a record of the type.
    Duplicates are not allowed among these symbols, unless
    allow-duplicate-field-names? is true.

    ```
    (make-record-type "employee" '(name age salary))
    ```

    The optional print argument is a function used by
    `display`, `write`, etc, for printing a record of the new
    type. It’s called as `(print record port)` and should look
    at record and write to port.

    Pass the `#:parent` keyword to derive a record type from a
    supertype. A derived record type has the fields from its parent type,
    followed by fields declared in the `make-record-type` call. Record
    predicates and field accessors for instance of a parent type will also
    work on any instance of a subtype.

    Allowing record subtyping has a small amount of overhead. To avoid this
    overhead, prevent extensibility by passing `#:extensible? #f`.
    By default, record types in Guile are not extensible.

    Generally speaking, calling `make-record-type` returns a fresh
    record type; it *generates* new record types. However sometimes
    you only want to define a record type if one hasn’t been defined
    already. For a *nongenerative* record type definition, pass a
    symbol as the `#:uid` keyword parameter. If a record with the
    given uid was already defined, it will be returned instead. The
    type name, fields, parent (if any), and so on for the previously-defined
    type must be compatible.

    R6RS defines a notion of “opaque” record types. Given an instance of
    an opaque record type, one cannot obtain a run-time representation of
    the record type. See [rnrs records procedural](#rnrs-records-procedural), for full details. The
    `#:opaque?` flag is used by Guile’s R6RS layer to record this
    information. The default is determined by whether the parent type, if
    any, was opaque.

    Fields are mutable by default, meaning that `record-modifier` will
    return a procedure that can update a record in place. Specifying a
    field using the form `(immutable name)` instead marks a field
    as immutable.

Scheme Procedure: **record-constructor** rtd [¶](#index-record_002dconstructor)
:   Return a procedure for constructing new members of the type represented
    by rtd. The result will be a procedure accepting exactly as many
    arguments as there are fields in the record type.

Scheme Procedure: **record-predicate** rtd [¶](#index-record_002dpredicate)
:   Return a procedure for testing membership in the type represented by
    rtd. The returned procedure accepts exactly one argument and
    returns a true value if the argument is a member of the indicated record
    type; it returns a false value otherwise.

Scheme Procedure: **record-accessor** rtd field-name [¶](#index-record_002daccessor)
:   Return a procedure for reading the value of a particular field of a
    member of the type represented by rtd. The returned procedure
    accepts exactly one argument which must be a record of the appropriate
    type; it returns the current value of the field named by the symbol
    field-name in that record.

    If field-name is a symbol, it must be a member of the list of
    field-names in the call to `make-record-type` that created the type
    represented by rtd. If multiple fields in rtd have the same
    name, `record-accessor` returns the first one.

    If field-name is an integer, it should be an index into
    `(record-type-fields rtd)`. This allows accessing fields
    with duplicate names.

Scheme Procedure: **record-modifier** rtd field-name [¶](#index-record_002dmodifier)
:   Return a procedure for writing the value of a particular field of a
    member of the type represented by rtd. The returned procedure
    accepts exactly two arguments: first, a record of the appropriate type,
    and second, an arbitrary Scheme value; it modifies the field named by
    the symbol field-name in that record to contain the given value.
    The returned value of the modifier procedure is unspecified. The symbol
    field-name is a field name or a field index, as in
    `record-modifier`.

Scheme Procedure: **record-type-descriptor** record [¶](#index-record_002dtype_002ddescriptor)
:   Return a record-type descriptor representing the type of the given
    record. That is, for example, if the returned descriptor were passed to
    `record-predicate`, the resulting predicate would return a true
    value when passed the given record. Note that it is not necessarily the
    case that the returned descriptor is the one that was passed to
    `record-constructor` in the call that created the constructor
    procedure that created the given record.

Scheme Procedure: **record-type-name** rtd [¶](#index-record_002dtype_002dname)
:   Return the type-name associated with the type represented by rtd. The
    returned value is `eqv?` to the type-name argument given in
    the call to `make-record-type` that created the type represented by
    rtd.

Scheme Procedure: **record-type-fields** rtd [¶](#index-record_002dtype_002dfields)
:   Return a list of the symbols naming the fields in members of the type
    represented by rtd. The returned value is `equal?` to the
    field-names argument given in the call to `make-record-type` that
    created the type represented by rtd.

---

Next: [Dictionary Types](#Dictionary-Types), Previous: [Records](#Records), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.18 Structures [¶](#Structures-1)

A *structure* is a first class data type which holds Scheme values
or C words in fields numbered 0 upwards. A *vtable* is a structure
that represents a structure type, giving field types and permissions,
and an optional print function for `write` etc.

Structures are lower level than records (see [Records](#Records)). Usually,
when you need to represent structured data, you just want to use
records. But sometimes you need to implement new kinds of structured
data abstractions, and for that purpose structures are useful. Indeed,
records in Guile are implemented with structures.

* [Vtables](#Vtables)
* [Structure Basics](#Structure-Basics)
* [Vtable Contents](#Vtable-Contents)
* [Meta-Vtables](#Meta_002dVtables)
* [Vtable Example](#Vtable-Example)

---

Next: [Structure Basics](#Structure-Basics), Up: [Structures](#Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.18.1 Vtables [¶](#Vtables-1)

A vtable is a structure type, specifying its layout, and other
information. A vtable is actually itself a structure, but there’s no
need to worry about that initially (see [Vtable Contents](#Vtable-Contents).)

Scheme Procedure: **make-vtable** fields [print] [¶](#index-make_002dvtable)
:   Create a new vtable.

    fields is a string describing the fields in the structures to be
    created. Each field is represented by two characters, a type letter
    and a permissions letter, for example `"pw"`. The types are as
    follows.

    * `p` – a Scheme value. “p” stands for “protected” meaning
      it’s protected against garbage collection.
    * `u` – an arbitrary word of data (an `scm_t_bits`). At the
      Scheme level it’s read and written as an unsigned integer. “u” stands
      for “unboxed”, as it’s stored as a raw value without additional type
      annotations.

    It used to be that the second letter for each field was a permission
    code, such as `w` for writable or `r` for read-only. However
    over time structs have become more of a raw low-level facility; access
    control is better implemented as a layer on top. After all,
    `struct-set!` is a cross-cutting operator that can bypass
    abstractions made by higher-level record facilities; it’s not generally
    safe (in the sense of abstraction-preserving) to expose
    `struct-set!` to “untrusted” code, even if the fields happen to
    be writable. Additionally, permission checks added overhead to every
    structure access in a way that couldn’t be optimized out, hampering the
    ability of structs to act as a low-level building block. For all of
    these reasons, all fields in Guile structs are now writable; attempting
    to make a read-only field will now issue a deprecation warning, and the
    field will be writable regardless.

    ```
    (make-vtable "pw")      ;; one scheme field
    (make-vtable "pwuwuw")  ;; one scheme and two unboxed fields
    ```

    The optional print argument is a function called by
    `display` and `write` (etc) to give a printed representation
    of a structure created from this vtable. It’s called
    `(print struct port)` and should look at struct and
    write to port. The default print merely gives a form like
    ‘`#<struct ADDR:ADDR>`’ with a pair of machine addresses.

    The following print function for example shows the two fields of its
    structure.

    ```
    (make-vtable "pwpw"
                 (lambda (struct port)
                   (format port "#<~a and ~a>"
                           (struct-ref struct 0)
                           (struct-ref struct 1))))
    ```

---

Next: [Vtable Contents](#Vtable-Contents), Previous: [Vtables](#Vtables), Up: [Structures](#Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.18.2 Structure Basics [¶](#Structure-Basics-1)

This section describes the basic procedures for working with structures.
`make-struct/no-tail` creates a structure, and `struct-ref`
and `struct-set!` access its fields.

Scheme Procedure: **make-struct/no-tail** vtable init … [¶](#index-make_002dstruct_002fno_002dtail)
:   Create a new structure, with layout per the given vtable
    (see [Vtables](#Vtables)).

    The optional init… arguments are initial values for the
    fields of the structure. This is the only way to
    put values in read-only fields. If there are fewer init
    arguments than fields then the defaults are `#f` for a Scheme
    field (type `p`) or 0 for an unboxed field (type `u`).

    The name is a bit strange, we admit. The reason for it is that Guile
    used to have a `make-struct` that took an additional argument;
    while we deprecate that old interface, `make-struct/no-tail` is the
    new name for this functionality.

    For example,

    ```
    (define v (make-vtable "pwpwpw"))
    (define s (make-struct/no-tail v 123 "abc" 456))
    (struct-ref s 0) ⇒ 123
    (struct-ref s 1) ⇒ "abc"
    ```

C Function: `SCM` **scm\_make\_struct** `(SCM vtable, SCM tail_size, SCM init_list)` [¶](#index-scm_005fmake_005fstruct)

C Function: `SCM` **scm\_c\_make\_struct** `(SCM vtable, SCM tail_size, SCM init, ...)` [¶](#index-scm_005fc_005fmake_005fstruct)

C Function: `SCM` **scm\_c\_make\_structv** `(SCM vtable, SCM tail_size, size_t n_inits, scm_t_bits init[])` [¶](#index-scm_005fc_005fmake_005fstructv)
:   There are a few ways to make structures from C. `scm_make_struct`
    takes a list, `scm_c_make_struct` takes variable arguments
    terminated with SCM\_UNDEFINED, and `scm_c_make_structv` takes a
    packed array.

    For all of these, tail\_size should be zero (as a SCM value).

Scheme Procedure: **struct?** obj [¶](#index-struct_003f)

C Function: **scm\_struct\_p** (obj) [¶](#index-scm_005fstruct_005fp)
:   Return `#t` if obj is a structure, or `#f` if not.

Scheme Procedure: **struct-ref** struct n [¶](#index-struct_002dref)

C Function: **scm\_struct\_ref** (struct, n) [¶](#index-scm_005fstruct_005fref)
:   Return the contents of field number n in struct. The
    first field is number 0.

    An error is thrown if n is out of range.

Scheme Procedure: **struct-set!** struct n value [¶](#index-struct_002dset_0021)

C Function: **scm\_struct\_set\_x** (struct, n, value) [¶](#index-scm_005fstruct_005fset_005fx)
:   Set field number n in struct to value. The first
    field is number 0.

    An error is thrown if n is out of range, or if the field cannot
    be written because it’s `r` read-only.

Unboxed fields (those with type `u`) need to be accessed with
special procedures.

Scheme Procedure: **struct-ref/unboxed** struct n [¶](#index-struct_002dref_002funboxed)

Scheme Procedure: **struct-set!/unboxed** struct n value [¶](#index-struct_002dset_0021_002funboxed)

C Function: **scm\_struct\_ref\_unboxed** (struct, n) [¶](#index-scm_005fstruct_005fref_005funboxed)

C Function: **scm\_struct\_set\_x\_unboxed** (struct, n, value) [¶](#index-scm_005fstruct_005fset_005fx_005funboxed)
:   Like `struct-ref` and `struct-set!`, except that these may
    only be used on unboxed fields. `struct-ref/unboxed` will always
    return a positive integer. Likewise, `struct-set!/unboxed` takes
    an unsigned integer as the value argument, and will signal an
    error otherwise.

Scheme Procedure: **struct-vtable** struct [¶](#index-struct_002dvtable)

C Function: **scm\_struct\_vtable** (struct) [¶](#index-scm_005fstruct_005fvtable)
:   Return the vtable that describes struct.

    The vtable is effectively the type of the structure. See [Vtable Contents](#Vtable-Contents), for more on vtables.

---

Next: [Meta-Vtables](#Meta_002dVtables), Previous: [Structure Basics](#Structure-Basics), Up: [Structures](#Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.18.3 Vtable Contents [¶](#Vtable-Contents-1)

A vtable is itself a structure. It has a specific set of fields
describing various aspects of its *instances*: the structures
created from a vtable. Some of the fields are internal to Guile, some
of them are part of the public interface, and there may be additional
fields added on by the user.

Every vtable has a field for the layout of their instances, a field for
the procedure used to print its instances, and a field for the name of
the vtable itself. Access to the layout and printer is exposed directly
via field indexes. Access to the vtable name is exposed via accessor
procedures.

Scheme Variable: **vtable-index-layout** [¶](#index-vtable_002dindex_002dlayout)

C Macro: **scm\_vtable\_index\_layout** [¶](#index-scm_005fvtable_005findex_005flayout)
:   The field number of the layout specification in a vtable. The layout
    specification is a symbol like `pwpw` formed from the fields
    string passed to `make-vtable`, or created by
    `make-struct-layout` (see [Meta-Vtables](#Meta_002dVtables)).

    ```
    (define v (make-vtable "pwpw" 0))
    (struct-ref v vtable-index-layout) ⇒ pwpw
    ```

    This field is read-only, since the layout of structures using a vtable
    cannot be changed.

Scheme Variable: **vtable-index-printer** [¶](#index-vtable_002dindex_002dprinter)

C Macro: **scm\_vtable\_index\_printer** [¶](#index-scm_005fvtable_005findex_005fprinter)
:   The field number of the printer function. This field contains `#f`
    if the default print function should be used.

    ```
    (define (my-print-func struct port)
      ...)
    (define v (make-vtable "pwpw" my-print-func))
    (struct-ref v vtable-index-printer) ⇒ my-print-func
    ```

    This field is writable, allowing the print function to be changed
    dynamically.

Scheme Procedure: **struct-vtable-name** vtable [¶](#index-struct_002dvtable_002dname)

Scheme Procedure: **set-struct-vtable-name!** vtable name [¶](#index-set_002dstruct_002dvtable_002dname_0021)

C Function: **scm\_struct\_vtable\_name** (vtable) [¶](#index-scm_005fstruct_005fvtable_005fname)

C Function: **scm\_set\_struct\_vtable\_name\_x** (vtable, name) [¶](#index-scm_005fset_005fstruct_005fvtable_005fname_005fx)
:   Get or set the name of vtable. name is a symbol and is
    used in the default print function when printing structures created
    from vtable.

    ```
    (define v (make-vtable "pw"))
    (set-struct-vtable-name! v 'my-name)

    (define s (make-struct v 0))
    (display s) ⊣ #<my-name b7ab3ae0:b7ab3730>
    ```

---

Next: [Vtable Example](#Vtable-Example), Previous: [Vtable Contents](#Vtable-Contents), Up: [Structures](#Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.18.4 Meta-Vtables [¶](#Meta_002dVtables-1)

As a structure, a vtable also has a vtable, which is also a structure.
Structures, their vtables, the vtables of the vtables, and so on form a
tree of structures. Making a new structure adds a leaf to the tree, and
if that structure is a vtable, it may be used to create other leaves.

If you traverse up the tree of vtables, via calling
`struct-vtable`, eventually you reach a root which is the vtable of
itself:

```
scheme@(guile-user)> (current-module)
$1 = #<directory (guile-user) 221b090>
scheme@(guile-user)> (struct-vtable $1)
$2 = #<record-type module>
scheme@(guile-user)> (struct-vtable $2)
$3 = #<<standard-vtable> 12c30a0>
scheme@(guile-user)> (struct-vtable $3)
$4 = #<<standard-vtable> 12c3fa0>
scheme@(guile-user)> (struct-vtable $4)
$5 = #<<standard-vtable> 12c3fa0>
scheme@(guile-user)> <standard-vtable>
$6 = #<<standard-vtable> 12c3fa0>
```

In this example, we can say that `$1` is an instance of `$2`,
`$2` is an instance of `$3`, `$3` is an instance of
`$4`, and `$4`, strangely enough, is an instance of itself.
The value bound to `$4` in this console session also bound to
`<standard-vtable>` in the default environment.

Scheme Variable: **<standard-vtable>** [¶](#index-_003cstandard_002dvtable_003e)
:   A meta-vtable, useful for making new vtables.

All of these values are structures. All but `$1` are vtables. As
`$2` is an instance of `$3`, and `$3` is a vtable, we can
say that `$3` is a *meta-vtable*: a vtable that can create
vtables.

With this definition, we can specify more precisely what a vtable is: a
vtable is a structure made from a meta-vtable. Making a structure from
a meta-vtable runs some special checks to ensure that the first field of
the structure is a valid layout. Additionally, if these checks see that
the layout of the child vtable contains all the required fields of a
vtable, in the correct order, then the child vtable will also be a
meta-table, inheriting a magical bit from the parent.

Scheme Procedure: **struct-vtable?** obj [¶](#index-struct_002dvtable_003f)

C Function: **scm\_struct\_vtable\_p** (obj) [¶](#index-scm_005fstruct_005fvtable_005fp)
:   Return `#t` if obj is a vtable structure: an instance of a
    meta-vtable.

`<standard-vtable>` is a root of the vtable tree. (Normally there
is only one root in a given Guile process, but due to some legacy
interfaces there may be more than one.)

The set of required fields of a vtable is the set of fields in the
`<standard-vtable>`, and is bound to `standard-vtable-fields`
in the default environment. It is possible to create a meta-vtable that
with additional fields in its layout, which can be used to create
vtables with additional data:

```
scheme@(guile-user)> (struct-ref $3 vtable-index-layout)
$6 = pwuhuhpwphuhuhpwpwpw
scheme@(guile-user)> (struct-ref $4 vtable-index-layout)
$7 = pwuhuhpwphuhuh
scheme@(guile-user)> standard-vtable-fields
$8 = "pwuhuhpwphuhuh"
scheme@(guile-user)> (struct-ref $2 vtable-offset-user)
$9 = module
```

In this continuation of our earlier example, `$2` is a vtable that
has extra fields, because its vtable, `$3`, was made from a
meta-vtable with an extended layout. `vtable-offset-user` is a
convenient definition that indicates the number of fields in
`standard-vtable-fields`.

Scheme Variable: **standard-vtable-fields** [¶](#index-standard_002dvtable_002dfields)
:   A string containing the ordered set of fields that a vtable must have.

Scheme Variable: **vtable-offset-user** [¶](#index-vtable_002doffset_002duser)
:   The first index in a vtable that is available for a user.

Scheme Procedure: **make-struct-layout** fields [¶](#index-make_002dstruct_002dlayout)

C Function: **scm\_make\_struct\_layout** (fields) [¶](#index-scm_005fmake_005fstruct_005flayout)
:   Return a structure layout symbol, from a fields string.
    fields is as described under `make-vtable`
    (see [Vtables](#Vtables)). An invalid fields string is an error.

With these definitions, one can define `make-vtable` in this way:

```
(define* (make-vtable fields #:optional printer)
  (make-struct/no-tail <standard-vtable>
    (make-struct-layout fields)
    printer))
```

---

Previous: [Meta-Vtables](#Meta_002dVtables), Up: [Structures](#Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.18.5 Vtable Example [¶](#Vtable-Example-1)

Let us bring these points together with an example. Consider a simple
object system with single inheritance. Objects will be normal
structures, and classes will be vtables with three extra class fields:
the name of the class, the parent class, and the list of fields.

So, first we need a meta-vtable that allocates instances with these
extra class fields.

```
(define <class>
  (make-vtable
   (string-append standard-vtable-fields "pwpwpw")
   (lambda (x port)
     (format port "<<class> ~a>" (class-name x)))))

(define (class? x)
  (and (struct? x)
       (eq? (struct-vtable x) <class>)))
```

To make a structure with a specific meta-vtable, we will use
`make-struct/no-tail`, passing it the computed instance layout and
printer, as with `make-vtable`, and additionally the extra three
class fields.

```
(define (make-class name parent fields)
  (let* ((fields (compute-fields parent fields))
         (layout (compute-layout fields)))
    (make-struct/no-tail <class>
      layout
      (lambda (x port)
        (print-instance x port))
      name
      parent
      fields)))
```

Instances will store their associated data in slots in the structure: as
many slots as there are fields. The `compute-layout` procedure
below can compute a layout, and `field-index` returns the slot
corresponding to a field.

```
(define-syntax-rule (define-accessor name n)
  (define (name obj)
    (struct-ref obj n)))

;; Accessors for classes
(define-accessor class-name (+ vtable-offset-user 0))
(define-accessor class-parent (+ vtable-offset-user 1))
(define-accessor class-fields (+ vtable-offset-user 2))

(define (compute-fields parent fields)
  (if parent
      (append (class-fields parent) fields)
      fields))

(define (compute-layout fields)
  (make-struct-layout
   (string-concatenate (make-list (length fields) "pw"))))

(define (field-index class field)
  (list-index (class-fields class) field))

(define (print-instance x port)
  (format port "<~a" (class-name (struct-vtable x)))
  (for-each (lambda (field idx)
              (format port " ~a: ~a" field (struct-ref x idx)))
            (class-fields (struct-vtable x))
            (iota (length (class-fields (struct-vtable x)))))
  (format port ">"))
```

So, at this point we can actually make a few classes:

```
(define-syntax-rule (define-class name parent field ...)
  (define name (make-class 'name parent '(field ...))))

(define-class <surface> #f
  width height)

(define-class <window> <surface>
  x y)
```

And finally, make an instance:

```
(make-struct/no-tail <window> 400 300 10 20)
⇒ <<window> width: 400 height: 300 x: 10 y: 20>
```

And that’s that. Note that there are many possible optimizations and
feature enhancements that can be made to this object system, and the
included GOOPS system does make most of them. For more simple use
cases, the records facility is usually sufficient. But sometimes you
need to make new kinds of data abstractions, and for that purpose,
structs are here.

---

Next: [Association Lists](#Association-Lists), Previous: [Structures](#Structures), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.19 Dictionary Types [¶](#Dictionary-Types-1)

A *dictionary* object is a data structure used to index
information in a user-defined way. In standard Scheme, the main
aggregate data types are lists and vectors. Lists are not really
indexed at all, and vectors are indexed only by number
(e.g. `(vector-ref foo 5)`). Often you will find it useful
to index your data on some other type; for example, in a library
catalog you might want to look up a book by the name of its
author. Dictionaries are used to help you organize information in
such a way.

An *association list* (or *alist* for short) is a list of
key-value pairs. Each pair represents a single quantity or
object; the `car` of the pair is a key which is used to
identify the object, and the `cdr` is the object’s value.

A *hash table* also permits you to index objects with
arbitrary keys, but in a way that makes looking up any one object
extremely fast. A well-designed hash system makes hash table
lookups almost as fast as conventional array or vector references.

Alists are popular among Lisp programmers because they use only
the language’s primitive operations (lists, *car*, *cdr*
and the equality primitives). No changes to the language core are
necessary. Therefore, with Scheme’s built-in list manipulation
facilities, it is very convenient to handle data stored in an
association list. Also, alists are highly portable and can be
easily implemented on even the most minimal Lisp systems.

However, alists are inefficient, especially for storing large
quantities of data. Because we want Guile to be useful for large
software systems as well as small ones, Guile provides a rich set
of tools for using either association lists or hash tables.

---

Next: [VList-Based Hash Lists or “VHashes”](#VHashes), Previous: [Dictionary Types](#Dictionary-Types), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.20 Association Lists [¶](#Association-Lists-1)

An association list is a conventional data structure that is often used
to implement simple key-value databases. It consists of a list of
entries in which each entry is a pair. The *key* of each entry is
the `car` of the pair and the *value* of each entry is the
`cdr`.

```
ASSOCIATION LIST ::=  '( (KEY1 . VALUE1)
                         (KEY2 . VALUE2)
                         (KEY3 . VALUE3)
                         ...
                       )
```

Association lists are also known, for short, as *alists*.

The structure of an association list is just one example of the infinite
number of possible structures that can be built using pairs and lists.
As such, the keys and values in an association list can be manipulated
using the general list structure procedures `cons`, `car`,
`cdr`, `set-car!`, `set-cdr!` and so on. However,
because association lists are so useful, Guile also provides specific
procedures for manipulating them.

* [Alist Key Equality](#Alist-Key-Equality)
* [Adding or Setting Alist Entries](#Adding-or-Setting-Alist-Entries)
* [Retrieving Alist Entries](#Retrieving-Alist-Entries)
* [Removing Alist Entries](#Removing-Alist-Entries)
* [Sloppy Alist Functions](#Sloppy-Alist-Functions)
* [Alist Example](#Alist-Example)

---

Next: [Adding or Setting Alist Entries](#Adding-or-Setting-Alist-Entries), Up: [Association Lists](#Association-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.20.1 Alist Key Equality [¶](#Alist-Key-Equality-1)

All of Guile’s dedicated association list procedures, apart from
`acons`, come in three flavors, depending on the level of equality
that is required to decide whether an existing key in the association
list is the same as the key that the procedure call uses to identify the
required entry.

* Procedures with *assq* in their name use `eq?` to determine key
  equality.
* Procedures with *assv* in their name use `eqv?` to determine
  key equality.
* Procedures with *assoc* in their name use `equal?` to
  determine key equality.

`acons` is an exception because it is used to build association
lists which do not require their entries’ keys to be unique.

---

Next: [Retrieving Alist Entries](#Retrieving-Alist-Entries), Previous: [Alist Key Equality](#Alist-Key-Equality), Up: [Association Lists](#Association-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.20.2 Adding or Setting Alist Entries [¶](#Adding-or-Setting-Alist-Entries-1)

`acons` adds a new entry to an association list and returns the
combined association list. The combined alist is formed by consing the
new entry onto the head of the alist specified in the `acons`
procedure call. So the specified alist is not modified, but its
contents become shared with the tail of the combined alist that
`acons` returns.

In the most common usage of `acons`, a variable holding the
original association list is updated with the combined alist:

```
(set! address-list (acons name address address-list))
```

In such cases, it doesn’t matter that the old and new values of
`address-list` share some of their contents, since the old value is
usually no longer independently accessible.

Note that `acons` adds the specified new entry regardless of
whether the alist may already contain entries with keys that are, in
some sense, the same as that of the new entry. Thus `acons` is
ideal for building alists where there is no concept of key uniqueness.

```
(set! task-list (acons 3 "pay gas bill" '()))
task-list
⇒
((3 . "pay gas bill"))

(set! task-list (acons 3 "tidy bedroom" task-list))
task-list
⇒
((3 . "tidy bedroom") (3 . "pay gas bill"))
```

`assq-set!`, `assv-set!` and `assoc-set!` are used to add
or replace an entry in an association list where there *is* a
concept of key uniqueness. If the specified association list already
contains an entry whose key is the same as that specified in the
procedure call, the existing entry is replaced by the new one.
Otherwise, the new entry is consed onto the head of the old association
list to create the combined alist. In all cases, these procedures
return the combined alist.

`assq-set!` and friends *may* destructively modify the
structure of the old association list in such a way that an existing
variable is correctly updated without having to `set!` it to the
value returned:

```
address-list
⇒
(("mary" . "34 Elm Road") ("james" . "16 Bow Street"))

(assoc-set! address-list "james" "1a London Road")
⇒
(("mary" . "34 Elm Road") ("james" . "1a London Road"))

address-list
⇒
(("mary" . "34 Elm Road") ("james" . "1a London Road"))
```

Or they may not:

```
(assoc-set! address-list "bob" "11 Newington Avenue")
⇒
(("bob" . "11 Newington Avenue") ("mary" . "34 Elm Road")
 ("james" . "1a London Road"))

address-list
⇒
(("mary" . "34 Elm Road") ("james" . "1a London Road"))
```

The only safe way to update an association list variable when adding or
replacing an entry like this is to `set!` the variable to the
returned value:

```
(set! address-list
      (assoc-set! address-list "bob" "11 Newington Avenue"))
address-list
⇒
(("bob" . "11 Newington Avenue") ("mary" . "34 Elm Road")
 ("james" . "1a London Road"))
```

Because of this slight inconvenience, you may find it more convenient to
use hash tables to store dictionary data. If your application will not
be modifying the contents of an alist very often, this may not make much
difference to you.

If you need to keep the old value of an association list in a form
independent from the list that results from modification by
`acons`, `assq-set!`, `assv-set!` or `assoc-set!`,
use `alist-copy` to copy the old association list before modifying
it.

Scheme Procedure: **acons** key value alist [¶](#index-acons)

C Function: **scm\_acons** (key, value, alist) [¶](#index-scm_005facons)
:   Add a new key-value pair to alist. A new pair is
    created whose car is key and whose cdr is value, and the
    pair is consed onto alist, and the new list is returned. This
    function is *not* destructive; alist is not modified.

Scheme Procedure: **assq-set!** alist key val [¶](#index-assq_002dset_0021)

Scheme Procedure: **assv-set!** alist key value [¶](#index-assv_002dset_0021)

Scheme Procedure: **assoc-set!** alist key value [¶](#index-assoc_002dset_0021)

C Function: **scm\_assq\_set\_x** (alist, key, val) [¶](#index-scm_005fassq_005fset_005fx)

C Function: **scm\_assv\_set\_x** (alist, key, val) [¶](#index-scm_005fassv_005fset_005fx)

C Function: **scm\_assoc\_set\_x** (alist, key, val) [¶](#index-scm_005fassoc_005fset_005fx)
:   Reassociate key in alist with value: find any existing
    alist entry for key and associate it with the new
    value. If alist does not contain an entry for key,
    add a new one. Return the (possibly new) alist.

    These functions do not attempt to verify the structure of alist,
    and so may cause unusual results if passed an object that is not an
    association list.

---

Next: [Removing Alist Entries](#Removing-Alist-Entries), Previous: [Adding or Setting Alist Entries](#Adding-or-Setting-Alist-Entries), Up: [Association Lists](#Association-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.20.3 Retrieving Alist Entries [¶](#Retrieving-Alist-Entries-1)

`assq`, `assv` and `assoc` find the entry in an alist
for a given key, and return the `(key . value)` pair.
`assq-ref`, `assv-ref` and `assoc-ref` do a similar
lookup, but return just the value.

Scheme Procedure: **assq** key alist [¶](#index-assq)

Scheme Procedure: **assv** key alist [¶](#index-assv)

Scheme Procedure: **assoc** key alist [¶](#index-assoc)

C Function: **scm\_assq** (key, alist) [¶](#index-scm_005fassq)

C Function: **scm\_assv** (key, alist) [¶](#index-scm_005fassv)

C Function: **scm\_assoc** (key, alist) [¶](#index-scm_005fassoc)
:   Return the first entry in alist with the given key. The
    return is the pair `(KEY . VALUE)` from alist. If there’s
    no matching entry the return is `#f`.

    `assq` compares keys with `eq?`, `assv` uses
    `eqv?` and `assoc` uses `equal?`. See also SRFI-1
    which has an extended `assoc` ([Association Lists](#SRFI_002d1-Association-Lists)).

Scheme Procedure: **assq-ref** alist key [¶](#index-assq_002dref)

Scheme Procedure: **assv-ref** alist key [¶](#index-assv_002dref)

Scheme Procedure: **assoc-ref** alist key [¶](#index-assoc_002dref)

C Function: **scm\_assq\_ref** (alist, key) [¶](#index-scm_005fassq_005fref)

C Function: **scm\_assv\_ref** (alist, key) [¶](#index-scm_005fassv_005fref)

C Function: **scm\_assoc\_ref** (alist, key) [¶](#index-scm_005fassoc_005fref)
:   Return the value from the first entry in alist with the given
    key, or `#f` if there’s no such entry.

    `assq-ref` compares keys with `eq?`, `assv-ref` uses
    `eqv?` and `assoc-ref` uses `equal?`.

    Notice these functions have the key argument last, like other
    `-ref` functions, but this is opposite to what `assq`
    etc above use.

    When the return is `#f` it can be either key not found, or
    an entry which happens to have value `#f` in the `cdr`. Use
    `assq` etc above if you need to differentiate these cases.

---

Next: [Sloppy Alist Functions](#Sloppy-Alist-Functions), Previous: [Retrieving Alist Entries](#Retrieving-Alist-Entries), Up: [Association Lists](#Association-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.20.4 Removing Alist Entries [¶](#Removing-Alist-Entries-1)

To remove the element from an association list whose key matches a
specified key, use `assq-remove!`, `assv-remove!` or
`assoc-remove!` (depending, as usual, on the level of equality
required between the key that you specify and the keys in the
association list).

As with `assq-set!` and friends, the specified alist may or may not
be modified destructively, and the only safe way to update a variable
containing the alist is to `set!` it to the value that
`assq-remove!` and friends return.

```
address-list
⇒
(("bob" . "11 Newington Avenue") ("mary" . "34 Elm Road")
 ("james" . "1a London Road"))

(set! address-list (assoc-remove! address-list "mary"))
address-list
⇒
(("bob" . "11 Newington Avenue") ("james" . "1a London Road"))
```

Note that, when `assq/v/oc-remove!` is used to modify an
association list that has been constructed only using the corresponding
`assq/v/oc-set!`, there can be at most one matching entry in the
alist, so the question of multiple entries being removed in one go does
not arise. If `assq/v/oc-remove!` is applied to an association
list that has been constructed using `acons`, or an
`assq/v/oc-set!` with a different level of equality, or any mixture
of these, it removes only the first matching entry from the alist, even
if the alist might contain further matching entries. For example:

```
(define address-list '())
(set! address-list (assq-set! address-list "mary" "11 Elm Street"))
(set! address-list (assq-set! address-list "mary" "57 Pine Drive"))
address-list
⇒
(("mary" . "57 Pine Drive") ("mary" . "11 Elm Street"))

(set! address-list (assoc-remove! address-list "mary"))
address-list
⇒
(("mary" . "11 Elm Street"))
```

In this example, the two instances of the string "mary" are not the same
when compared using `eq?`, so the two `assq-set!` calls add
two distinct entries to `address-list`. When compared using
`equal?`, both "mary"s in `address-list` are the same as the
"mary" in the `assoc-remove!` call, but `assoc-remove!` stops
after removing the first matching entry that it finds, and so one of the
"mary" entries is left in place.

Scheme Procedure: **assq-remove!** alist key [¶](#index-assq_002dremove_0021)

Scheme Procedure: **assv-remove!** alist key [¶](#index-assv_002dremove_0021)

Scheme Procedure: **assoc-remove!** alist key [¶](#index-assoc_002dremove_0021)

C Function: **scm\_assq\_remove\_x** (alist, key) [¶](#index-scm_005fassq_005fremove_005fx)

C Function: **scm\_assv\_remove\_x** (alist, key) [¶](#index-scm_005fassv_005fremove_005fx)

C Function: **scm\_assoc\_remove\_x** (alist, key) [¶](#index-scm_005fassoc_005fremove_005fx)
:   Delete the first entry in alist associated with key, and return
    the resulting alist.

---

Next: [Alist Example](#Alist-Example), Previous: [Removing Alist Entries](#Removing-Alist-Entries), Up: [Association Lists](#Association-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.20.5 Sloppy Alist Functions [¶](#Sloppy-Alist-Functions-1)

`sloppy-assq`, `sloppy-assv` and `sloppy-assoc` behave
like the corresponding non-`sloppy-` procedures, except that they
return `#f` when the specified association list is not well-formed,
where the non-`sloppy-` versions would signal an error.

Specifically, there are two conditions for which the non-`sloppy-`
procedures signal an error, which the `sloppy-` procedures handle
instead by returning `#f`. Firstly, if the specified alist as a
whole is not a proper list:

```
(assoc "mary" '((1 . 2) ("key" . "door") . "open sesame"))
⇒
ERROR: In procedure assoc in expression (assoc "mary" (quote #)):
ERROR: Wrong type argument in position 2 (expecting
   association list): ((1 . 2) ("key" . "door") . "open sesame")

(sloppy-assoc "mary" '((1 . 2) ("key" . "door") . "open sesame"))
⇒
#f
```

Secondly, if one of the entries in the specified alist is not a pair:

```
(assoc 2 '((1 . 1) 2 (3 . 9)))
⇒
ERROR: In procedure assoc in expression (assoc 2 (quote #)):
ERROR: Wrong type argument in position 2 (expecting
   association list): ((1 . 1) 2 (3 . 9))

(sloppy-assoc 2 '((1 . 1) 2 (3 . 9)))
⇒
#f
```

Unless you are explicitly working with badly formed association lists,
it is much safer to use the non-`sloppy-` procedures, because they
help to highlight coding and data errors that the `sloppy-`
versions would silently cover up.

Scheme Procedure: **sloppy-assq** key alist [¶](#index-sloppy_002dassq)

C Function: **scm\_sloppy\_assq** (key, alist) [¶](#index-scm_005fsloppy_005fassq)
:   Behaves like `assq` but does not do any error checking.
    Recommended only for use in Guile internals.

Scheme Procedure: **sloppy-assv** key alist [¶](#index-sloppy_002dassv)

C Function: **scm\_sloppy\_assv** (key, alist) [¶](#index-scm_005fsloppy_005fassv)
:   Behaves like `assv` but does not do any error checking.
    Recommended only for use in Guile internals.

Scheme Procedure: **sloppy-assoc** key alist [¶](#index-sloppy_002dassoc)

C Function: **scm\_sloppy\_assoc** (key, alist) [¶](#index-scm_005fsloppy_005fassoc)
:   Behaves like `assoc` but does not do any error checking.
    Recommended only for use in Guile internals.

---

Previous: [Sloppy Alist Functions](#Sloppy-Alist-Functions), Up: [Association Lists](#Association-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.20.6 Alist Example [¶](#Alist-Example-1)

The following example shows how alists may be used in practice.

```
(define capitals (list (cons "New York" "Albany")
                       (cons "Oregon"   "Salem")
                       (cons "Florida"  "Miami")))
```

Other ways to create an alist are

```
(define capitals (acons "New York" "Albany"
                        (acons "Oregon" "Salem"
                               (acons "Florida"  "Miami" '()))))
```

or

```
(use-modules (srfi srfi-1)) ; for alist-copy
(define capitals (alist-copy
                   '(("New York" . "Albany")
                     ("Oregon"   . "Salem")
                     ("Florida"  . "Miami"))))
```

Here `alist-copy` is necessary if we intend to modify the alist, because a literal like `'(("New York" . "Albany") ...)` cannot be modified.

We can now operate on the alist.

```
;; What's the capital of Oregon?
(assoc "Oregon" capitals)       ⇒ ("Oregon" . "Salem")
(assoc-ref capitals "Oregon")   ⇒ "Salem"

;; We left out South Dakota.
(set! capitals
      (assoc-set! capitals "South Dakota" "Pierre"))
capitals
⇒ (("South Dakota" . "Pierre")
    ("New York" . "Albany")
    ("Oregon" . "Salem")
    ("Florida" . "Miami"))

;; And we got Florida wrong.
(set! capitals
      (assoc-set! capitals "Florida" "Tallahassee"))
capitals
⇒ (("South Dakota" . "Pierre")
    ("New York" . "Albany")
    ("Oregon" . "Salem")
    ("Florida" . "Tallahassee"))

;; After Oregon secedes, we can remove it.
(set! capitals
      (assoc-remove! capitals "Oregon"))
capitals
⇒ (("South Dakota" . "Pierre")
    ("New York" . "Albany")
    ("Florida" . "Tallahassee"))
```

---

Next: [Hash Tables](#Hash-Tables), Previous: [Association Lists](#Association-Lists), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.21 VList-Based Hash Lists or “VHashes” [¶](#VList_002dBased-Hash-Lists-or-_0060_0060VHashes_0027_0027)

The `(ice-9 vlist)` module provides an implementation of *VList-based
hash lists* (see [VLists](#VLists)). VList-based hash lists, or *vhashes*, are an
immutable dictionary type similar to association lists that maps *keys* to
*values*. However, unlike association lists, accessing a value given its
key is typically a constant-time operation.
