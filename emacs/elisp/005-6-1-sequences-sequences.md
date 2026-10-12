### 6.1 Sequences [¶](#Sequences)

This section describes functions that accept any kind of sequence.

Function: **sequencep** object [¶](#index-sequencep)
:   This function returns `t` if object is a list, vector,
    string, bool-vector, or char-table, `nil` otherwise. See also
    `seqp` below.

Function: **length** sequence [¶](#index-length)
:   This function returns the number of elements in sequence. The
    function signals the `wrong-type-argument` error if the argument
    is not a sequence or is a dotted list; it signals the
    `circular-list` error if the argument is a circular list. For a
    char-table, the value returned is always one more than the maximum
    Emacs character code.

    See [Definition of safe-length](#Definition-of-safe_002dlength), for the related function `safe-length`.

    ```
    (length '(1 2 3))
        ⇒ 3
    ```

    ```
    (length ())
        ⇒ 0
    ```

    ```
    (length "foobar")
        ⇒ 6
    ```

    ```
    (length [1 2 3])
        ⇒ 3
    ```

    ```
    (length (make-bool-vector 5 nil))
        ⇒ 5
    ```

See also `string-bytes`, in [Text Representations](#Text-Representations).

If you need to compute the width of a string on display, you should use
`string-width` (see [Size of Displayed Text](#Size-of-Displayed-Text)), not `length`,
since `length` only counts the number of characters, but does not
account for the display width of each character.

Function: **length<** sequence length [¶](#index-length_003c)
:   Return non-`nil` if sequence is shorter than length.
    This may be more efficient than computing the length of sequence
    if sequence is a long list.

Function: **length>** sequence length [¶](#index-length_003e)
:   Return non-`nil` if sequence is longer than length.

Function: **length=** sequence length [¶](#index-length_003d)
:   Return non-`nil` if the length of sequence is equal to
    length.

Function: **elt** sequence index [¶](#index-elt)
:   This function returns the element of sequence indexed by
    index. Legitimate values of index are integers ranging
    from 0 up to one less than the length of sequence. If
    sequence is a list, out-of-range values behave as for
    `nth`. See [Definition of nth](#Definition-of-nth). Otherwise, out-of-range values
    trigger an `args-out-of-range` error.

    ```
    (elt [1 2 3 4] 2)
         ⇒ 3
    ```

    ```
    (elt '(1 2 3 4) 2)
         ⇒ 3
    ```

    ```
    ;; We use string to show clearly which character elt returns.
    (string (elt "1234" 2))
         ⇒ "3"
    ```

    ```
    (elt [1 2 3 4] 4)
         error→ Args out of range: [1 2 3 4], 4
    ```

    ```
    (elt [1 2 3 4] -1)
         error→ Args out of range: [1 2 3 4], -1
    ```

    This function generalizes `aref` (see [Functions that Operate on Arrays](#Array-Functions)) and
    `nth` (see [Definition of nth](#Definition-of-nth)).

Function: **copy-sequence** seqr [¶](#index-copy_002dsequence)
:   This function returns a copy of seqr, which should be either a
    sequence or a record. The copy is the same type of object as the
    original, and it has the same elements in the same order. However, if
    seqr is empty, like a string or a vector of zero length, the
    value returned by this function might not be a copy, but an empty
    object of the same type and identical to seqr.

    Storing a new element into the copy does not affect the original
    seqr, and vice versa. However, the elements of the copy
    are not copies; they are identical (`eq`) to the elements
    of the original. Therefore, changes made within these elements, as
    found via the copy, are also visible in the original.

    If the argument is a string with text properties, the property list in
    the copy is itself a copy, not shared with the original’s property
    list. However, the actual values of the properties are shared.
    See [Text Properties](#Text-Properties).

    This function does not work for dotted lists. Trying to copy a
    circular list may cause an infinite loop.

    See also `append` in [Building Cons Cells and Lists](#Building-Lists), `concat` in
    [Creating Strings](#Creating-Strings), and `vconcat` in [Functions for Vectors](#Vector-Functions),
    for other ways to copy sequences.

    ```
    (setq bar (list 1 2))
         ⇒ (1 2)
    ```

    ```
    (setq x (vector 'foo bar))
         ⇒ [foo (1 2)]
    ```

    ```
    (setq y (copy-sequence x))
         ⇒ [foo (1 2)]
    ```

    ```

    ```

    ```
    (eq x y)
         ⇒ nil
    ```

    ```
    (equal x y)
         ⇒ t
    ```

    ```
    (eq (elt x 1) (elt y 1))
         ⇒ t
    ```

    ```

    ```

    ```
    ;; Replacing an element of one sequence.
    (aset x 0 'quux)
    x ⇒ [quux (1 2)]
    y ⇒ [foo (1 2)]
    ```

    ```

    ```

    ```
    ;; Modifying the inside of a shared element.
    (setcar (aref x 1) 69)
    x ⇒ [quux (69 2)]
    y ⇒ [foo (69 2)]
    ```

Function: **reverse** sequence [¶](#index-reverse)
:   This function creates a new sequence whose elements are the elements
    of sequence, but in reverse order. The original argument sequence
    is *not* altered. Note that char-tables cannot be reversed.

    ```
    (setq x '(1 2 3 4))
         ⇒ (1 2 3 4)
    ```

    ```
    (reverse x)
         ⇒ (4 3 2 1)
    x
         ⇒ (1 2 3 4)
    ```

    ```
    (setq x [1 2 3 4])
         ⇒ [1 2 3 4]
    ```

    ```
    (reverse x)
         ⇒ [4 3 2 1]
    x
         ⇒ [1 2 3 4]
    ```

    ```
    (setq x "xyzzy")
         ⇒ "xyzzy"
    ```

    ```
    (reverse x)
         ⇒ "yzzyx"
    x
         ⇒ "xyzzy"
    ```

Function: **nreverse** sequence [¶](#index-nreverse)
:   This function reverses the order of the elements of sequence.
    Unlike `reverse` the original sequence may be modified.

    For example:

    ```
    (setq x (list 'a 'b 'c))
         ⇒ (a b c)
    ```

    ```
    x
         ⇒ (a b c)
    (nreverse x)
         ⇒ (c b a)
    ```

    ```
    ;; The cons cell that was first is now last.
    x
         ⇒ (a)
    ```

    To avoid confusion, we usually store the result of `nreverse`
    back in the same variable which held the original list:

    ```
    (setq x (nreverse x))
    ```

    Here is the `nreverse` of our favorite example, `(a b c)`,
    presented graphically:

    ```
    Original list head:                       Reversed list:
     -------------        -------------        ------------
    | car  | cdr  |      | car  | cdr  |      | car | cdr  |
    |   a  |  nil |<--   |   b  |   o  |<--   |   c |   o  |
    |      |      |   |  |      |   |  |   |  |     |   |  |
     -------------    |   --------- | -    |   -------- | -
                      |             |      |            |
                       -------------        ------------
    ```

    For the vector, it is even simpler because you don’t need setq:

    ```
    (setq x (copy-sequence [1 2 3 4]))
         ⇒ [1 2 3 4]
    (nreverse x)
         ⇒ [4 3 2 1]
    x
         ⇒ [4 3 2 1]
    ```

    Note that unlike `reverse`, this function doesn’t work with strings.
    Although you can alter string data by using `aset`, it is strongly
    encouraged to treat strings as immutable even when they are mutable.
    See [Mutability](#Mutability).

Function: **sort** sequence &rest keyword-args [¶](#index-sort)
:   This function sorts sequence, which must be a list or vector, and
    returns a sorted sequence of the same type.
    The sort is stable, which means that elements with equal sort keys maintain
    their relative order. It takes the following optional keyword arguments:

    `:key keyfunc`
    :   Use keyfunc, a function that takes a single element from
        sequence and returns its key value, to generate the keys used in
        comparison. If this argument is absent or if keyfunc is
        `nil` then `identity` is assumed; that is, the elements
        themselves are used as sorting keys.

    `:lessp predicate`
    :   Use predicate to order the keys. predicate is a function
        that takes two sort keys as arguments and returns non-`nil` if the
        first should come before the second. If this argument is absent or
        predicate is `nil`, then `value<` is used, which
        is applicable to many different Lisp types and generally sorts in
        ascending order (see [definition of value<](#definition-of-value_003c), below).

        For consistency, any predicate must obey the following rules:

        * It must be *antisymmetric*: it cannot both order a before
          b and b before a.
        * It must be *transitive*: if it orders a before b and
          b before c, then it must also order a before c.

    `:reverse flag`
    :   If flag is non-`nil`, the sorting order is reversed. With
        the default `:lessp` predicate this means sorting in descending order.

    `:in-place flag`
    :   If flag is non-`nil`, then sequence is sorted in-place
        (destructively) and returned. If `nil`, or if this argument is not
        given, a sorted copy of the input is returned and sequence itself
        remains unmodified. In-place sorting is slightly faster, but the
        original sequence is lost.

    If the default behavior is not suitable for your needs, it is usually
    easier and faster to supply a new `:key` function than a different
    `:lessp` predicate. For example, consider sorting these strings:

    ```
    (setq numbers '("one" "two" "three" "four" "five" "six"))
    (sort numbers)
         ⇒ ("five" "four" "one" "six" "three" "two")
    ```

    You can sort the strings by length instead by supplying a different key
    function:

    ```
    (sort numbers :key #'length)
         ⇒ ("one" "two" "six" "four" "five" "three")
    ```

    Note how strings of the same length keep their original order, thanks to
    the sorting stability. Now suppose you want to sort by length, but use
    the string contents to break ties. The easiest way is to specify a key
    function that transforms an element to a value that is sorted this way.
    Since `value<` orders compound objects (conses, lists,
    vectors and records) lexicographically, you could do:

    ```
    (sort numbers :key (lambda (x) (cons (length x) x)))
         ⇒ ("one" "six" "two" "five" "four" "three")
    ```

    because `(3 . "six")` is ordered before `(3 . "two")` and so on.

    For compatibility with previous versions of Emacs, the `sort`
    function can also be called using the fixed two-argument form:

    ```
    (sort sequence predicate)
    ```

    where predicate is the `:lessp` argument. When using this
    form, sorting is always done in-place.

See [Sorting Text](#Sorting), for more functions that perform sorting. See
`documentation` in [Access to Documentation Strings](#Accessing-Documentation), for a useful
example of `sort`.

Function: **value<** a b [¶](#index-value_003c)
:   This function returns non-`nil` if a comes before b in
    the standard sorting order; this means that it returns `nil` when
    b comes before a, or if they are equal or unordered.

    The arguments a and b must have the same type.
    Specifically:

    * Numbers are compared using `<` (see [definition of <](#definition-of-_003c)).
    * Strings are compared using `string-lessp` (see [definition of string-lessp](#definition-of-string_002dlessp)) and symbols are compared by comparing their names as
      strings.
    * Conses, lists, vectors and records are compared lexicographically. This
      means that the two sequences are compared element-wise from left to
      right until they differ, and the result is then that of `value<` on
      the first pair of differing elements. If one sequence runs out of
      elements before the other, the shorter sequence comes before the longer.
    * Markers are compared first by buffer, then by position.
    * Buffers and processes are compared by comparing their names as strings.
      Dead buffers (whose name is `nil`) will compare before any live
      buffer.
    * Other types are considered unordered and the return value will be
      `nil`.

    Examples:

    ```
    (value< -4 3.5) ⇒ t
    (value< "dog" "cat") ⇒ nil
    (value< 'yip 'yip) ⇒ nil
    (value< '(3 2) '(3 2 0)) ⇒ t
    (value< [3 2 "a"] [3 2 "b"]) ⇒ t
    ```

    Note that `nil` is treated as either a symbol or an empty list,
    depending on what it is compared against:

    ```
    (value< nil '(0)) ⇒ t
    (value< 'nib nil) ⇒ t
    ```

    There is no limit to the length of sequences (lists, vectors and so on)
    that can be compared, but `value<` may fail with an error if used
    to compare circular or deeply nested data structures.

The `seq.el` library provides the following additional sequence
manipulation macros and functions, prefixed with `seq-`.

All functions defined in this library are free of side-effects;
i.e., they do not modify any sequence (list, vector, or string) that
you pass as an argument. Unless otherwise stated, the result is a
sequence of the same type as the input. For those functions that take
a predicate, this should be a function of one argument.

The `seq.el` library can be extended to work with additional
types of sequential data-structures. For that purpose, all functions
are defined using `cl-defgeneric`. See [Generic Functions](#Generic-Functions), for
more details about using `cl-defgeneric` for adding extensions.

Function: **seq-elt** sequence index [¶](#index-seq_002delt)
:   This function returns the element of sequence at the specified
    index, which is an integer whose valid value range is zero to
    one less than the length of sequence. For out-of-range values
    on built-in sequence types, `seq-elt` behaves like `elt`.
    For the details, see [Definition of elt](#Definition-of-elt).

    ```
    (seq-elt [1 2 3 4] 2)
    ⇒ 3
    ```

    `seq-elt` returns places settable using `setf`
    (see [The `setf` Macro](#Setting-Generalized-Variables)).

    ```
    (setq vec [1 2 3 4])
    (setf (seq-elt vec 2) 5)
    vec
    ⇒ [1 2 5 4]
    ```

Function: **seq-length** sequence [¶](#index-seq_002dlength)
:   This function returns the number of elements in sequence. For
    built-in sequence types, `seq-length` behaves like `length`.
    See [Definition of length](#Definition-of-length).

Function: **seqp** object [¶](#index-seqp)
:   This function returns non-`nil` if object is a sequence
    (a list or array), or any additional type of sequence defined via
    `seq.el` generic functions. This is an extensible variant of
    `sequencep`.

    ```
    (seqp [1 2])
    ⇒ t
    ```

    ```
    (seqp 2)
    ⇒ nil
    ```

Function: **seq-drop** sequence n [¶](#index-seq_002ddrop)
:   This function returns all but the first n (an integer)
    elements of sequence. If n is negative or zero,
    the result is sequence.

    ```
    (seq-drop [1 2 3 4 5 6] 3)
    ⇒ [4 5 6]
    ```

    ```
    (seq-drop "hello world" -4)
    ⇒ "hello world"
    ```

Function: **seq-take** sequence n [¶](#index-seq_002dtake)
:   This function returns the first n (an integer) elements of
    sequence. If n is negative or zero, the result
    is `nil`.

    ```
    (seq-take '(1 2 3 4) 3)
    ⇒ (1 2 3)
    ```

    ```
    (seq-take [1 2 3 4] 0)
    ⇒ []
    ```

Function: **seq-take-while** predicate sequence [¶](#index-seq_002dtake_002dwhile)
:   This function returns the members of sequence in order,
    stopping before the first one for which predicate returns `nil`.

    ```
    (seq-take-while (lambda (elt) (> elt 0)) '(1 2 3 -1 -2))
    ⇒ (1 2 3)
    ```

    ```
    (seq-take-while (lambda (elt) (> elt 0)) [-1 4 6])
    ⇒ []
    ```

Function: **seq-drop-while** predicate sequence [¶](#index-seq_002ddrop_002dwhile)
:   This function returns the members of sequence in order,
    starting from the first one for which predicate returns `nil`.

    ```
    (seq-drop-while (lambda (elt) (> elt 0)) '(1 2 3 -1 -2))
    ⇒ (-1 -2)
    ```

    ```
    (seq-drop-while (lambda (elt) (< elt 0)) [1 4 6])
    ⇒ [1 4 6]
    ```

Function: **seq-split** sequence length [¶](#index-seq_002dsplit)
:   This function returns a list consisting of sub-sequences of
    sequence of (at most) length length. (The final element
    may be shorter than length if the length of sequence isn’t
    a multiple of length.

    ```
    (seq-split [0 1 2 3 4] 2)
    ⇒ ([0 1] [2 3] [4])
    ```

Function: **seq-do** function sequence [¶](#index-seq_002ddo)
:   This function applies function to each element of
    sequence in turn (presumably for side effects), and returns
    sequence.

Function: **seq-map** function sequence [¶](#index-seq_002dmap)
:   This function returns the result of applying function to each
    element of sequence. The returned value is a list.

    ```
    (seq-map #'1+ '(2 4 6))
    ⇒ (3 5 7)
    ```

    ```
    (seq-map #'symbol-name [foo bar])
    ⇒ ("foo" "bar")
    ```

Function: **seq-map-indexed** function sequence [¶](#index-seq_002dmap_002dindexed)
:   This function returns the result of applying function to each
    element of sequence and its index within seq. The
    returned value is a list.

    ```
    (seq-map-indexed (lambda (elt idx)
                       (list idx elt))
                     '(a b c))
    ⇒ ((0 a) (1 b) (2 c))
    ```

Function: **seq-mapn** function &rest sequences [¶](#index-seq_002dmapn)
:   This function returns the result of applying function to each
    element of sequences. The arity (see [subr-arity](#What-Is-a-Function)) of function must match the number of sequences.
    Mapping stops at the end of the shortest sequence, and the returned
    value is a list.

    ```
    (seq-mapn #'+ '(2 4 6) '(20 40 60))
    ⇒ (22 44 66)
    ```

    ```
    (seq-mapn #'concat '("moskito" "bite") ["bee" "sting"])
    ⇒ ("moskitobee" "bitesting")
    ```

Function: **seq-filter** predicate sequence [¶](#index-seq_002dfilter)
:   This function returns a list of all the elements in sequence
    for which predicate returns non-`nil`.

    ```
    (seq-filter (lambda (elt) (> elt 0)) [1 -1 3 -3 5])
    ⇒ (1 3 5)
    ```

    ```
    (seq-filter (lambda (elt) (> elt 0)) '(-1 -3 -5))
    ⇒ nil
    ```

Function: **seq-remove** predicate sequence [¶](#index-seq_002dremove)
:   This function returns a list of all the elements in sequence
    for which predicate returns `nil`.

    ```
    (seq-remove (lambda (elt) (> elt 0)) [1 -1 3 -3 5])
    ⇒ (-1 -3)
    ```

    ```
    (seq-remove (lambda (elt) (< elt 0)) '(-1 -3 -5))
    ⇒ nil
    ```

Function: **seq-remove-at-position** sequence n [¶](#index-seq_002dremove_002dat_002dposition)
:   This function returns a copy of sequence where the element at
    (zero-based) index n got removed. The result is a sequence of
    the same type as sequence.

    ```
    (seq-remove-at-position [1 -1 3 -3 5] 0)
    ⇒ [-1 3 -3 5]
    ```

    ```
    (seq-remove-at-position [1 -1 3 -3 5] 3)
    ⇒ [1 -1 3 5]
    ```

Function: **seq-keep** function sequence [¶](#index-seq_002dkeep)
:   This function returns a list of all non-`nil` results from
    calling function on the elements in sequence.

    ```
    (seq-keep #'cl-digit-char-p '(?6 ?a ?7))
    ⇒ (6 7)
    ```

Function: **seq-reduce** function sequence initial-value [¶](#index-seq_002dreduce)
:   This function returns the result of calling function with
    initial-value and the first element of sequence, then calling
    function with that result and the second element of sequence,
    then with that result and the third element of sequence, etc.
    function should be a function of two arguments.

    function is called with two arguments. initial-value
    (and then the accumulated value) is used as the first argument, and
    the elements in sequence are used for the second argument.

    If sequence is empty, this returns initial-value without
    calling function.

    ```
    (seq-reduce #'+ [1 2 3 4] 0)
    ⇒ 10
    ```

    ```
    (seq-reduce #'+ '(1 2 3 4) 5)
    ⇒ 15
    ```

    ```
    (seq-reduce #'+ '() 3)
    ⇒ 3
    ```

Function: **seq-some** predicate sequence [¶](#index-seq_002dsome)
:   This function returns the first non-`nil` value returned by
    applying predicate to each element of sequence in turn.

    ```
    (seq-some #'numberp ["abc" 1 nil])
    ⇒ t
    ```

    ```
    (seq-some #'numberp ["abc" "def"])
    ⇒ nil
    ```

    ```
    (seq-some #'null ["abc" 1 nil])
    ⇒ t
    ```

    ```
    (seq-some #'1+ [2 4 6])
    ⇒ 3
    ```

Function: **seq-find** predicate sequence &optional default [¶](#index-seq_002dfind)
:   This function returns the first element in sequence for which
    predicate returns non-`nil`. If no element matches
    predicate, the function returns default.

    Note that this function has an ambiguity if the found element is
    identical to default, as in that case it cannot be known whether
    an element was found or not.

    ```
    (seq-find #'numberp ["abc" 1 nil])
    ⇒ 1
    ```

    ```
    (seq-find #'numberp ["abc" "def"])
    ⇒ nil
    ```

Function: **seq-every-p** predicate sequence [¶](#index-seq_002devery_002dp)
:   This function returns non-`nil` if applying predicate
    to every element of sequence returns non-`nil`.

    ```
    (seq-every-p #'numberp [2 4 6])
    ⇒ t
    ```

    ```
    (seq-every-p #'numberp [2 4 "6"])
    ⇒ nil
    ```

Function: **seq-empty-p** sequence [¶](#index-seq_002dempty_002dp)
:   This function returns non-`nil` if sequence is empty.

    ```
    (seq-empty-p "not empty")
    ⇒ nil
    ```

    ```
    (seq-empty-p "")
    ⇒ t
    ```

Function: **seq-count** predicate sequence [¶](#index-seq_002dcount)
:   This function returns the number of elements in sequence for which
    predicate returns non-`nil`.

    ```
    (seq-count (lambda (elt) (> elt 0)) [-1 2 0 3 -2])
    ⇒ 2
    ```

Function: **seq-sort** function sequence [¶](#index-seq_002dsort)
:   This function returns a copy of sequence that is sorted
    according to function, a function of two arguments that returns
    non-`nil` if the first argument should sort before the second.

Function: **seq-sort-by** function predicate sequence [¶](#index-seq_002dsort_002dby)
:   This function is similar to `seq-sort`, but the elements of
    sequence are transformed by applying function on them
    before being sorted. function is a function of one argument.

    ```
    (seq-sort-by #'seq-length #'> ["a" "ab" "abc"])
    ⇒ ["abc" "ab" "a"]
    ```

Function: **seq-contains-p** sequence elt &optional function [¶](#index-seq_002dcontains_002dp)
:   This function returns non-`nil` if at least one element in
    sequence is equal to elt. If the optional argument
    function is non-`nil`, it is a function of two arguments to
    use instead of the default `equal`.

    ```
    (seq-contains-p '(symbol1 symbol2) 'symbol1)
    ⇒ t
    ```

    ```
    (seq-contains-p '(symbol1 symbol2) 'symbol3)
    ⇒ nil
    ```

Function: **seq-set-equal-p** sequence1 sequence2 &optional testfn [¶](#index-seq_002dset_002dequal_002dp)
:   This function checks whether sequence1 and sequence2
    contain the same elements, regardless of the order. If the optional
    argument testfn is non-`nil`, it is a function of two
    arguments to use instead of the default `equal`.

    ```
    (seq-set-equal-p '(a b c) '(c b a))
    ⇒ t
    ```

    ```
    (seq-set-equal-p '(a b c) '(c b))
    ⇒ nil
    ```

    ```
    (seq-set-equal-p '("a" "b" "c") '("c" "b" "a"))
    ⇒ t
    ```

    ```
    (seq-set-equal-p '("a" "b" "c") '("c" "b" "a") #'eq)
    ⇒ nil
    ```

Function: **seq-position** sequence elt &optional function [¶](#index-seq_002dposition)
:   This function returns the (zero-based) index of the first element in
    sequence that is equal to elt. If the optional argument
    function is non-`nil`, it is a function of two arguments to
    use instead of the default `equal`.

    ```
    (seq-position '(a b c) 'b)
    ⇒ 1
    ```

    ```
    (seq-position '(a b c) 'd)
    ⇒ nil
    ```

Function: **seq-positions** sequence elt &optional testfn [¶](#index-seq_002dpositions)
:   This function returns a list of the (zero-based) indices of the
    elements in sequence for which testfn returns
    non-`nil` when passed the element and elt as
    arguments. testfn defaults to `equal`.

    ```
    (seq-positions '(a b c a d) 'a)
    ⇒ (0 3)
    ```

    ```
    (seq-positions '(a b c a d) 'z)
    ⇒ nil
    ```

    ```
    (seq-positions '(11 5 7 12 9 15) 10 #'>=)
    ⇒ (0 3 5)
    ```

Function: **seq-uniq** sequence &optional function [¶](#index-seq_002duniq)
:   This function returns a list of the elements of sequence with
    duplicates removed. If the optional argument function is non-`nil`,
    it is a function of two arguments to use instead of the default `equal`.

    ```
    (seq-uniq '(1 2 2 1 3))
    ⇒ (1 2 3)
    ```

    ```
    (seq-uniq '(1 2 2.0 1.0) #'=)
    ⇒ (1 2)
    ```

Function: **seq-subseq** sequence start &optional end [¶](#index-seq_002dsubseq)
:   This function returns a subset of sequence from start
    to end, both integers (end defaults to the last element).
    If start or end is negative, it counts from the end of
    sequence.

    ```
    (seq-subseq '(1 2 3 4 5) 1)
    ⇒ (2 3 4 5)
    ```

    ```
    (seq-subseq '[1 2 3 4 5] 1 3)
    ⇒ [2 3]
    ```

    ```
    (seq-subseq '[1 2 3 4 5] -3 -1)
    ⇒ [3 4]
    ```

Function: **seq-concatenate** type &rest sequences [¶](#index-seq_002dconcatenate)
:   This function returns a sequence of type type made of the
    concatenation of sequences. type may be: `vector`,
    `list` or `string`.

    ```
    (seq-concatenate 'list '(1 2) '(3 4) [5 6])
    ⇒ (1 2 3 4 5 6)
    ```

    ```
    (seq-concatenate 'string "Hello " "world")
    ⇒ "Hello world"
    ```

Function: **seq-mapcat** function sequence &optional type [¶](#index-seq_002dmapcat)
:   This function returns the result of applying `seq-concatenate`
    to the result of applying function to each element of
    sequence. The result is a sequence of type type, or a
    list if type is `nil`.

    ```
    (seq-mapcat #'seq-reverse '((3 2 1) (6 5 4)))
    ⇒ (1 2 3 4 5 6)
    ```

Function: **seq-partition** sequence n [¶](#index-seq_002dpartition)
:   This function returns a list of the elements of sequence
    grouped into sub-sequences of length n. The last sequence may
    contain less elements than n. n must be an integer. If
    n is a negative integer or 0, the return value is `nil`.

    ```
    (seq-partition '(0 1 2 3 4 5 6 7) 3)
    ⇒ ((0 1 2) (3 4 5) (6 7))
    ```

Function: **seq-union** sequence1 sequence2 &optional function [¶](#index-seq_002dunion)
:   This function returns a list of the elements that appear either in
    sequence1 or sequence2. The elements of the returned list
    are all unique, in the sense that no two elements there will compare
    equal. If the optional argument function is non-`nil`, it
    should be a function of two arguments to use to compare elements,
    instead of the default `equal`.

    ```
    (seq-union [1 2 3] [3 5])
    ⇒ (1 2 3 5)
    ```

Function: **seq-intersection** sequence1 sequence2 &optional function [¶](#index-seq_002dintersection)
:   This function returns a copy of sequence1 from which the
    elements that do not appear in sequence2 were removed. If the
    optional argument function is non-`nil`, it is a function of
    two arguments to use to compare elements instead of the default
    `equal`.

    ```
    (seq-intersection [2 3 4 5] [1 3 5 6 7])
    ⇒ (3 5)
    ```

Function: **seq-difference** sequence1 sequence2 &optional function [¶](#index-seq_002ddifference)
:   This function returns a copy of sequence1 from which the
    elements that appear in sequence2 were removed. If the optional
    argument function is non-`nil`, it is a function of two
    arguments to use to compare elements instead of the default
    `equal`.

    ```
    (seq-difference '(2 3 4 5) [1 3 5 6 7])
    ⇒ (2 4)
    ```

Function: **seq-group-by** function sequence [¶](#index-seq_002dgroup_002dby)
:   This function separates the elements of sequence into an alist
    whose keys are the result of applying function to each element
    of sequence. Keys are compared using `equal`.

    ```
    (seq-group-by #'integerp '(1 2.1 3 2 3.2))
    ⇒ ((t 1 3 2) (nil 2.1 3.2))
    ```

    ```
    (seq-group-by #'car '((a 1) (b 2) (a 3) (c 4)))
    ⇒ ((b (b 2)) (a (a 1) (a 3)) (c (c 4)))
    ```

Function: **seq-into** sequence type [¶](#index-seq_002dinto)
:   This function converts the sequence sequence into a sequence
    of type type. type can be one of the following symbols:
    `vector`, `string` or `list`.

    ```
    (seq-into [1 2 3] 'list)
    ⇒ (1 2 3)
    ```

    ```
    (seq-into nil 'vector)
    ⇒ []
    ```

    ```
    (seq-into "hello" 'vector)
    ⇒ [104 101 108 108 111]
    ```

Function: **seq-min** sequence [¶](#index-seq_002dmin)
:   This function returns the smallest element of sequence. The
    elements of sequence must be numbers or markers
    (see [Markers](#Markers)).

    ```
    (seq-min [3 1 2])
    ⇒ 1
    ```

    ```
    (seq-min "Hello")
    ⇒ 72
    ```

Function: **seq-max** sequence [¶](#index-seq_002dmax)
:   This function returns the largest element of sequence. The
    elements of sequence must be numbers or markers.

    ```
    (seq-max [1 3 2])
    ⇒ 3
    ```

    ```
    (seq-max "Hello")
    ⇒ 111
    ```

Macro: **seq-doseq** (var sequence) body… [¶](#index-seq_002ddoseq)
:   This macro is like `dolist` (see [dolist](#Iteration)), except
    that sequence can be a list, vector or string. This is
    primarily useful for side-effects.

Macro: **seq-let** var-sequence val-sequence body… [¶](#index-seq_002dlet)
:   This macro binds the variables defined in var-sequence to the
    values that are the corresponding elements of val-sequence.
    This is known as *destructuring binding*. The elements of
    var-sequence can themselves include sequences, allowing for
    nested destructuring.

    The var-sequence sequence can also include the `&rest`
    marker followed by a variable name to be bound to the rest of
    val-sequence.

    ```
    (seq-let [first second] [1 2 3 4]
      (list first second))
    ⇒ (1 2)
    ```

    ```
    (seq-let (_ a _ b) '(1 2 3 4)
      (list a b))
    ⇒ (2 4)
    ```

    ```
    (seq-let [a [b [c]]] [1 [2 [3]]]
      (list a b c))
    ⇒ (1 2 3)
    ```

    ```
    (seq-let [a b &rest others] [1 2 3 4]
      others)
    ```

    ```
    ⇒ [3 4]
    ```

    The `pcase` patterns provide an alternative facility for
    destructuring binding, see [Destructuring with `pcase` Patterns](#Destructuring-with-pcase-Patterns).

Macro: **seq-setq** var-sequence val-sequence [¶](#index-seq_002dsetq)
:   This macro works similarly to `seq-let`, except that values are
    assigned to variables as if by `setq` instead of as in a
    `let` binding.

    ```
    (let ((a nil)
          (b nil))
      (seq-setq (_ a _ b) '(1 2 3 4))
      (list a b))
    ⇒ (2 4)
    ```

Function: **seq-random-elt** sequence [¶](#index-seq_002drandom_002delt)
:   This function returns an element of sequence taken at random.

    ```
    (seq-random-elt [1 2 3 4])
    ⇒ 3
    (seq-random-elt [1 2 3 4])
    ⇒ 2
    (seq-random-elt [1 2 3 4])
    ⇒ 4
    (seq-random-elt [1 2 3 4])
    ⇒ 2
    (seq-random-elt [1 2 3 4])
    ⇒ 1
    ```

    If sequence is empty, this function signals an error.

---

Next: [Functions that Operate on Arrays](#Array-Functions), Previous: [Sequences](#Sequence-Functions), Up: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.2 Arrays [¶](#Arrays-1)

An *array* object has slots that hold a number of other Lisp
objects, called the elements of the array. Any element of an array
may be accessed in constant time. In contrast, the time to access an
element of a list is proportional to the position of that element in
the list.

Emacs defines four types of array, all one-dimensional:
*strings* (see [String Type](#String-Type)), *vectors* (see [Vector Type](#Vector-Type)), *bool-vectors* (see [Bool-Vector Type](#Bool_002dVector-Type)), and
*char-tables* (see [Char-Table Type](#Char_002dTable-Type)). Vectors and char-tables
can hold elements of any type, but strings can only hold characters,
and bool-vectors can only hold `t` and `nil`.

All four kinds of array share these characteristics:

* The first element of an array has index zero, the second element has
  index 1, and so on. This is called *zero-origin* indexing. For
  example, an array of four elements has indices 0, 1, 2, and 3.
* The length of the array is fixed once you create it; you cannot
  change the length of an existing array.
* For purposes of evaluation, the array is a constant—i.e.,
  it evaluates to itself.
* The elements of an array may be referenced or changed with the functions
  `aref` and `aset`, respectively (see [Functions that Operate on Arrays](#Array-Functions)).

When you create an array, other than a char-table, you must specify
its length. You cannot specify the length of a char-table, because that
is determined by the range of character codes.

In principle, if you want an array of text characters, you could use
either a string or a vector. In practice, we always choose strings for
such applications, for four reasons:

* They occupy one-fourth the space of a vector of the same elements.
* Strings are printed in a way that shows the contents more clearly
  as text.
* Strings can hold text properties. See [Text Properties](#Text-Properties).
* Many of the specialized editing and I/O facilities of Emacs accept only
  strings. For example, you cannot insert a vector of characters into a
  buffer the way you can insert a string. See [Strings and Characters](#Strings-and-Characters).

By contrast, for an array of keyboard input characters (such as a key
sequence), a vector may be necessary, because many keyboard input
characters are outside the range that will fit in a string. See [Key Sequence Input](#Key-Sequence-Input).

---

Next: [Vectors](#Vectors), Previous: [Arrays](#Arrays), Up: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.3 Functions that Operate on Arrays [¶](#Functions-that-Operate-on-Arrays)

In this section, we describe the functions that accept all types of
arrays.

Function: **arrayp** object [¶](#index-arrayp)
:   This function returns `t` if object is an array (i.e., a
    vector, a string, a bool-vector or a char-table).

    ```
    (arrayp [a])
         ⇒ t
    (arrayp "asdf")
         ⇒ t
    (arrayp (syntax-table))    ;; A char-table.
         ⇒ t
    ```

Function: **aref** arr index [¶](#index-aref)
:   This function returns the indexth element of the array or record
    arr. The first element is at index zero.

    ```
    (setq primes [2 3 5 7 11 13])
         ⇒ [2 3 5 7 11 13]
    (aref primes 4)
         ⇒ 11
    ```

    ```
    (aref "abcdefg" 1)
         ⇒ 98           ; ‘b’ is ASCII code 98.
    ```

    See also the function `elt`, in [Sequences](#Sequence-Functions).

Function: **aset** array index object [¶](#index-aset)
:   This function sets the indexth element of array to be
    object. It returns object.

    ```
    (setq w (vector 'foo 'bar 'baz))
         ⇒ [foo bar baz]
    (aset w 0 'fu)
         ⇒ fu
    w
         ⇒ [fu bar baz]
    ```

    ```

    ```

    ```
    ;; copy-sequence copies the string to be modified later.
    (setq x (copy-sequence "asdfasfd"))
         ⇒ "asdfasfd"
    (aset x 3 ?Z)
         ⇒ 90
    x
         ⇒ "asdZasfd"
    ```

    The array should be mutable. See [Mutability](#Mutability).

    If array is a string and object is not a character, a
    `wrong-type-argument` error results. For more information about
    string mutation, see [Modifying Strings](#Modifying-Strings).

Function: **fillarray** array object [¶](#index-fillarray)
:   This function fills the array array with object, so that
    each element of array is object. It returns array.

    ```
    (setq a (copy-sequence [a b c d e f g]))
         ⇒ [a b c d e f g]
    (fillarray a 0)
         ⇒ [0 0 0 0 0 0 0]
    a
         ⇒ [0 0 0 0 0 0 0]
    ```

    ```
    (setq s (copy-sequence "When in the course"))
         ⇒ "When in the course"
    (fillarray s ?-)
         ⇒ "------------------"
    ```

    If array is a string and object is not a character, a
    `wrong-type-argument` error results.

The general sequence functions `copy-sequence` and `length`
are often useful for objects known to be arrays. See [Sequences](#Sequence-Functions).

---

Next: [Functions for Vectors](#Vector-Functions), Previous: [Functions that Operate on Arrays](#Array-Functions), Up: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.4 Vectors [¶](#Vectors-1)

A *vector* is a general-purpose array whose elements can be any
Lisp objects. (By contrast, the elements of a string can only be
characters. See [Strings and Characters](#Strings-and-Characters).) Vectors are used in
Emacs for many purposes: as key sequences (see [Key Sequences](#Key-Sequences)), as
symbol-lookup tables (see [Creating and Interning Symbols](#Creating-Symbols)), as part of the
representation of a byte-compiled function (see [Byte Compilation](#Byte-Compilation)),
and more.

Like other arrays, vectors use zero-origin indexing: the first
element has index 0.

Vectors are printed with square brackets surrounding the elements.
Thus, a vector whose elements are the symbols `a`, `b` and
`a` is printed as `[a b a]`. You can write vectors in the
same way in Lisp input.

A vector, like a string or a number, is considered a constant for
evaluation: the result of evaluating it is the same vector. This does
not evaluate or even examine the elements of the vector.
See [Self-Evaluating Forms](#Self_002dEvaluating-Forms). Vectors written with square brackets
should not be modified via `aset` or other destructive
operations. See [Mutability](#Mutability).

Here are examples illustrating these principles:

```
(setq avector [1 two '(three) "four" [five]])
     ⇒ [1 two '(three) "four" [five]]
(eval avector)
     ⇒ [1 two '(three) "four" [five]]
(eq avector (eval avector))
     ⇒ t
```

---

Next: [Char-Tables](#Char_002dTables), Previous: [Vectors](#Vectors), Up: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.5 Functions for Vectors [¶](#Functions-for-Vectors)

Here are some functions that relate to vectors:

Function: **vectorp** object [¶](#index-vectorp)
:   This function returns `t` if object is a vector.

    ```
    (vectorp [a])
         ⇒ t
    (vectorp "asdf")
         ⇒ nil
    ```

Function: **vector** &rest objects [¶](#index-vector)
:   This function creates and returns a vector whose elements are the
    arguments, objects.

    ```
    (vector 'foo 23 [bar baz] "rats")
         ⇒ [foo 23 [bar baz] "rats"]
    (vector)
         ⇒ []
    ```

Function: **make-vector** length object [¶](#index-make_002dvector)
:   This function returns a new vector consisting of length elements,
    each initialized to object.

    ```
    (setq sleepy (make-vector 9 'Z))
         ⇒ [Z Z Z Z Z Z Z Z Z]
    ```

Function: **vconcat** &rest sequences [¶](#index-vconcat)
:   This function returns a new vector containing all the elements of
    sequences. The arguments sequences may be proper lists,
    vectors, strings or bool-vectors. If no sequences are given,
    the empty vector is returned.

    The value is either the empty vector, or is a newly constructed
    nonempty vector that is not `eq` to any existing vector.

    ```
    (setq a (vconcat '(A B C) '(D E F)))
         ⇒ [A B C D E F]
    (eq a (vconcat a))
         ⇒ nil
    ```

    ```
    (vconcat)
         ⇒ []
    (vconcat [A B C] "aa" '(foo (6 7)))
         ⇒ [A B C 97 97 foo (6 7)]
    ```

    The `vconcat` function also allows byte-code function objects as
    arguments. This is a special feature to make it easy to access the entire
    contents of a byte-code function object. See [Closure Function Objects](#Closure-Objects).

    For other concatenation functions, see `mapconcat` in [Mapping Functions](#Mapping-Functions), `concat` in [Creating Strings](#Creating-Strings), and `append`
    in [Building Cons Cells and Lists](#Building-Lists).

The `append` function also provides a way to convert a vector into a
list with the same elements:

```
(setq avector [1 two (quote (three)) "four" [five]])
     ⇒ [1 two '(three) "four" [five]]
(append avector nil)
     ⇒ (1 two '(three) "four" [five])
```

---

Next: [Bool-vectors](#Bool_002dVectors), Previous: [Functions for Vectors](#Vector-Functions), Up: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.6 Char-Tables [¶](#Char_002dTables-1)

A char-table is much like a vector, except that it is indexed by
character codes. Any valid character code, without modifiers, can be
used as an index in a char-table. You can access a char-table’s
elements with `aref` and `aset`, as with any array. In
addition, a char-table can have *extra slots* to hold additional
data not associated with particular character codes. Like vectors,
char-tables are constants when evaluated, and can hold elements of any
type.

Each char-table has a *subtype*, a symbol, which serves two
purposes:

* The subtype provides an easy way to tell what the char-table is for.
  For instance, display tables are char-tables with `display-table`
  as the subtype, and syntax tables are char-tables with
  `syntax-table` as the subtype. The subtype can be queried using
  the function `char-table-subtype`, described below.
* The subtype controls the number of *extra slots* in the
  char-table. This number is specified by the subtype’s
  `char-table-extra-slots` symbol property (see [Symbol Properties](#Symbol-Properties)), whose value should be an integer between 0 and 10. If
  the subtype has no such symbol property, the char-table has no extra
  slots.

A char-table can have a *parent*, which is another char-table. If
it does, then whenever the char-table specifies `nil` for a
particular character c, it inherits the value specified in the
parent. In other words, `(aref char-table c)` returns
the value from the parent of char-table if char-table itself
specifies `nil`.

A char-table can also have a *default value*. If so, then
`(aref char-table c)` returns the default value
whenever the char-table does not specify any other non-`nil` value.

Function: **make-char-table** subtype &optional init [¶](#index-make_002dchar_002dtable)
:   Return a newly-created char-table, with subtype subtype (a
    symbol). Each element is initialized to init, which defaults to
    `nil`. You cannot alter the subtype of a char-table after the
    char-table is created.

    There is no argument to specify the length of the char-table, because
    all char-tables have room for any valid character code as an index.

    If subtype has the `char-table-extra-slots` symbol
    property, that specifies the number of extra slots in the char-table.
    This should be an integer between 0 and 10; otherwise,
    `make-char-table` raises an error. If subtype has no
    `char-table-extra-slots` symbol property (see [Property Lists](#Property-Lists)), the char-table has no extra slots.

Function: **char-table-p** object [¶](#index-char_002dtable_002dp)
:   This function returns `t` if object is a char-table, and
    `nil` otherwise.

Function: **char-table-subtype** char-table [¶](#index-char_002dtable_002dsubtype)
:   This function returns the subtype symbol of char-table.

There is no special function to access default values in a char-table.
To do that, use `char-table-range` (see below).

Function: **char-table-parent** char-table [¶](#index-char_002dtable_002dparent)
:   This function returns the parent of char-table. The parent is
    always either `nil` or another char-table.

Function: **set-char-table-parent** char-table new-parent [¶](#index-set_002dchar_002dtable_002dparent)
:   This function sets the parent of char-table to new-parent.

Function: **char-table-extra-slot** char-table n [¶](#index-char_002dtable_002dextra_002dslot)
:   This function returns the contents of extra slot n (zero based)
    of char-table. The number of extra slots in a char-table is
    determined by its subtype.

Function: **set-char-table-extra-slot** char-table n value [¶](#index-set_002dchar_002dtable_002dextra_002dslot)
:   This function stores value in extra slot n (zero based) of
    char-table.

A char-table can specify an element value for a single character code;
it can also specify a value for an entire character set.

Function: **char-table-range** char-table range [¶](#index-char_002dtable_002drange)
:   This returns the value specified in char-table for a range of
    characters range. Here are the possibilities for range:

    `nil`
    :   Refers to the default value.

    char
    :   Refers to the element for character char
        (supposing char is a valid character code).

    `(from . to)`
    :   A cons cell refers to all the characters in the inclusive range
        ‘`[from..to]`’. In this case, the function returns the
        value for the character specified by from.

Function: **set-char-table-range** char-table range value [¶](#index-set_002dchar_002dtable_002drange)
:   This function sets the value in char-table for a range of
    characters range. Here are the possibilities for range:

    `nil`
    :   Refers to the default value.

    `t`
    :   Refers to the whole range of character codes.

    char
    :   Refers to the element for character char
        (supposing char is a valid character code).

    `(from . to)`
    :   A cons cell refers to all the characters in the inclusive range
        ‘`[from..to]`’.

Function: **map-char-table** function char-table [¶](#index-map_002dchar_002dtable)
:   This function calls its argument function for each element of
    char-table that has a non-`nil` value. The call to
    function is with two arguments, a key and a value. The key
    is a possible range argument for `char-table-range`—either
    a valid character or a cons cell `(from . to)`,
    specifying a range of characters that share the same value. The value is
    what `(char-table-range char-table key)` returns.

    Overall, the key-value pairs passed to function describe all the
    values stored in char-table.

    The return value is always `nil`; to make calls to
    `map-char-table` useful, function should have side effects.
    For example, here is how to examine the elements of the syntax table:

    ```
    (let (accumulator)
       (map-char-table
        (lambda (key value)
          (setq accumulator
                (cons (list
                       (if (consp key)
                           (list (car key) (cdr key))
                         key)
                       value)
                      accumulator)))
        (syntax-table))
       accumulator)
    ⇒
    (((2597602 4194303) (2)) ((2597523 2597601) (3))
     ... (65379 (5 . 65378)) (65378 (4 . 65379)) (65377 (1))
     ... (12 (0)) (11 (3)) (10 (12)) (9 (0)) ((0 8) (3)))
    ```

---

Next: [Managing a Fixed-Size Ring of Objects](#Rings), Previous: [Char-Tables](#Char_002dTables), Up: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.7 Bool-vectors [¶](#Bool_002dvectors)

A bool-vector is much like a vector, except that it stores only the
values `t` and `nil`. If you try to store any non-`nil`
value into an element of the bool-vector, the effect is to store
`t` there. As with all arrays, bool-vector indices start from 0,
and the length cannot be changed once the bool-vector is created.
Bool-vectors are constants when evaluated.

Several functions work specifically with bool-vectors; aside
from that, you manipulate them with same functions used for other kinds
of arrays.

Function: **make-bool-vector** length initial [¶](#index-make_002dbool_002dvector)
:   Return a new bool-vector of length elements,
    each one initialized to initial.

Function: **bool-vector** &rest objects [¶](#index-bool_002dvector)
:   This function creates and returns a bool-vector whose elements are the
    arguments, objects.

Function: **bool-vector-p** object [¶](#index-bool_002dvector_002dp)
:   This returns `t` if object is a bool-vector,
    and `nil` otherwise.

There are also some bool-vector set operation functions, described below:

Function: **bool-vector-exclusive-or** a b &optional c [¶](#index-bool_002dvector_002dexclusive_002dor)
:   Return *bitwise exclusive or* of bool vectors a and b.
    If optional argument c is given, the result of this operation is
    stored into c. All arguments should be bool vectors of the same length.

Function: **bool-vector-union** a b &optional c [¶](#index-bool_002dvector_002dunion)
:   Return *bitwise or* of bool vectors a and b. If
    optional argument c is given, the result of this operation is
    stored into c. All arguments should be bool vectors of the same length.

Function: **bool-vector-intersection** a b &optional c [¶](#index-bool_002dvector_002dintersection)
:   Return *bitwise and* of bool vectors a and b. If
    optional argument c is given, the result of this operation is
    stored into c. All arguments should be bool vectors of the same length.

Function: **bool-vector-set-difference** a b &optional c [¶](#index-bool_002dvector_002dset_002ddifference)
:   Return *set difference* of bool vectors a and b. If
    optional argument c is given, the result of this operation is
    stored into c. All arguments should be bool vectors of the same length.

Function: **bool-vector-not** a &optional b [¶](#index-bool_002dvector_002dnot)
:   Return *set complement* of bool vector a. If optional
    argument b is given, the result of this operation is stored into
    b. All arguments should be bool vectors of the same length.

Function: **bool-vector-subsetp** a b [¶](#index-bool_002dvector_002dsubsetp)
:   Return `t` if every `t` value in a is also `t` in
    b, `nil` otherwise. All arguments should be bool vectors of the
    same length.

Function: **bool-vector-count-consecutive** a b i [¶](#index-bool_002dvector_002dcount_002dconsecutive)
:   Return the number of consecutive elements in a equal b
    starting at i. `a` is a bool vector, b is `t`
    or `nil`, and i is an index into `a`.

Function: **bool-vector-count-population** a [¶](#index-bool_002dvector_002dcount_002dpopulation)
:   Return the number of elements that are `t` in bool vector a.

The printed form represents up to 8 boolean values as a single
character:

```
(bool-vector t nil t nil)
     ⇒ #&4"^E"
(bool-vector)
     ⇒ #&0""
```

You can use `vconcat` to print a bool-vector like other vectors:

```
(vconcat (bool-vector nil t nil t))
     ⇒ [nil t nil t]
```

Here is another example of creating, examining, and updating a
bool-vector:

```
(setq bv (make-bool-vector 5 t))
     ⇒ #&5"^_"
(aref bv 1)
     ⇒ t
(aset bv 3 nil)
     ⇒ nil
bv
     ⇒ #&5"^W"
```

These results make sense because the binary codes for control-\_ and
control-W are 11111 and 10111, respectively.

---

Previous: [Bool-vectors](#Bool_002dVectors), Up: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.8 Managing a Fixed-Size Ring of Objects [¶](#Managing-a-Fixed_002dSize-Ring-of-Objects)

A *ring* is a fixed-size data structure that supports insertion,
deletion, rotation, and modulo-indexed reference and traversal. An
efficient ring data structure is implemented by the `ring`
package. It provides the functions listed in this section.

Note that several rings in Emacs, like the kill ring and the
mark ring, are actually implemented as simple lists, *not* using
the `ring` package; thus the following functions won’t work on
them.

Function: **make-ring** size [¶](#index-make_002dring)
:   This returns a new ring capable of holding size objects.
    size should be an integer.

Function: **ring-p** object [¶](#index-ring_002dp)
:   This returns `t` if object is a ring, `nil` otherwise.

Function: **ring-size** ring [¶](#index-ring_002dsize)
:   This returns the maximum capacity of the ring.

Function: **ring-length** ring [¶](#index-ring_002dlength)
:   This returns the number of objects that ring currently contains.
    The value will never exceed that returned by `ring-size`.

Function: **ring-elements** ring [¶](#index-ring_002delements)
:   This returns a list of the objects in ring, in order, newest first.

Function: **ring-copy** ring [¶](#index-ring_002dcopy)
:   This returns a new ring which is a copy of ring.
    The new ring contains the same (`eq`) objects as ring.

Function: **ring-empty-p** ring [¶](#index-ring_002dempty_002dp)
:   This returns `t` if ring is empty, `nil` otherwise.

The newest element in the ring always has index 0. Higher indices
correspond to older elements. Indices are computed modulo the ring
length. Index −1 corresponds to the oldest element, −2
to the next-oldest, and so forth.

Function: **ring-ref** ring index [¶](#index-ring_002dref)
:   This returns the object in ring found at index index.
    index may be negative or greater than the ring length. If
    ring is empty, `ring-ref` signals an error.

Function: **ring-insert** ring object [¶](#index-ring_002dinsert)
:   This inserts object into ring, making it the newest
    element, and returns object.

    If the ring is full, insertion removes the oldest element to
    make room for the new element.

Function: **ring-remove** ring &optional index [¶](#index-ring_002dremove)
:   Remove an object from ring, and return that object. The
    argument index specifies which item to remove; if it is
    `nil`, that means to remove the oldest item. If ring is
    empty, `ring-remove` signals an error.

Function: **ring-insert-at-beginning** ring object [¶](#index-ring_002dinsert_002dat_002dbeginning)
:   This inserts object into ring, treating it as the oldest
    element. The return value is not significant.

    If the ring is full, this function removes the newest element to make
    room for the inserted element.

Function: **ring-resize** ring size [¶](#index-ring_002dresize)
:   Set the size of ring to size. If the new size is smaller,
    then the oldest items in the ring are discarded.

If you are careful not to exceed the ring size, you can
use the ring as a first-in-first-out queue. For example:

```
(let ((fifo (make-ring 5)))
  (mapc (lambda (obj) (ring-insert fifo obj))
        '(0 one "two"))
  (list (ring-remove fifo) t
        (ring-remove fifo) t
        (ring-remove fifo)))
     ⇒ (0 t one t "two")
```

---

Next: [Hash Tables](#Hash-Tables), Previous: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 7 Records [¶](#Records-1)

The purpose of records is to allow programmers to create objects
with new types that are not built into Emacs. They are used as the
underlying representation of `cl-defstruct` and `defclass`
instances.

Internally, a record object is much like a vector; its slots can be
accessed using `aref` and it can be copied using
`copy-sequence`. However, the first slot is used to hold its
type as returned by `type-of`. Also, in the current
implementation records can have at most 4096 slots, whereas vectors
can be much larger. Like arrays, records use zero-origin indexing:
the first slot has index 0.

The type slot should be a symbol or a type descriptor. If it’s a
type descriptor, the symbol naming its type will be returned;
[Type Descriptors](#Type-Descriptors). Any other kind of object is returned as-is.

The printed representation of records is ‘`#s`’ followed by a
list specifying the contents. The first list element must be the
record type. The following elements are the record slots.

To avoid conflicts with other type names, Lisp programs that define
new types of records should normally use the naming conventions of the
package where these record types are introduced for the names of the
types. Note that the names of the types which could possibly conflict
might not be known at the time the package defining a record type is
loaded; they could be loaded at some future point in time.

A record is considered a constant for evaluation: the result of
evaluating it is the same record. This does not evaluate or even
examine the slots. See [Self-Evaluating Forms](#Self_002dEvaluating-Forms).

* [Record Functions](#Record-Functions)
* [Backward Compatibility](#Backward-Compatibility)

---

Next: [Backward Compatibility](#Backward-Compatibility), Up: [Records](#Records)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.1 Record Functions [¶](#Record-Functions-1)

Function: **recordp** object [¶](#index-recordp)
:   This function returns `t` if object is a record.

    ```
    (recordp #s(a))
         ⇒ t
    ```

Function: **record** type &rest objects [¶](#index-record)
:   This function creates and returns a record whose type is type
    and remaining slots are the rest of the arguments, objects.

    ```
    (record 'foo 23 [bar baz] "rats")
         ⇒ #s(foo 23 [bar baz] "rats")
    ```

Function: **make-record** type length object [¶](#index-make_002drecord)
:   This function returns a new record with type type and
    length more slots, each initialized to object.

    ```
    (setq sleepy (make-record 'foo 9 'Z))
         ⇒ #s(foo Z Z Z Z Z Z Z Z Z)
    ```

To copy trees consisting of records, vectors and conses (lists), use
`copy-tree` with its optional second argument non-`nil`.
See [copy-tree](#Building-Lists).

---

Previous: [Record Functions](#Record-Functions), Up: [Records](#Records)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.2 Backward Compatibility [¶](#Backward-Compatibility-1)

Code compiled with older versions of `cl-defstruct` that
doesn’t use records may run into problems when used in a new Emacs.
To alleviate this, Emacs detects when an old `cl-defstruct` is
used, and enables a mode in which `type-of` handles old struct
objects as if they were records.

Function: **cl-old-struct-compat-mode** arg [¶](#index-cl_002dold_002dstruct_002dcompat_002dmode)
:   If arg is positive, enable backward compatibility with old-style
    structs.

---

Next: [Symbols](#Symbols), Previous: [Records](#Records), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 8 Hash Tables [¶](#Hash-Tables-1)

A hash table is a very fast kind of lookup table, somewhat like an
alist (see [Association Lists](#Association-Lists)) in that it maps keys to
corresponding values. It differs from an alist in these ways:

* Lookup in a hash table is extremely fast for large tables—in fact, the
  time required is essentially *independent* of how many elements are
  stored in the table. For smaller tables (a few tens of elements)
  alists may still be faster because hash tables have a more-or-less
  constant overhead.
* The correspondences in a hash table are in no particular order.
* There is no way to share structure between two hash tables,
  the way two alists can share a common tail.

Emacs Lisp provides a general-purpose hash table data type, along
with a series of functions for operating on them. Hash tables have a
special printed representation, which consists of ‘`#s`’ followed
by a list specifying the hash table properties and contents.
See [Creating Hash Tables](#Creating-Hash).
(Hash notation, the initial ‘`#`’ character used in the printed
representations of objects with no read representation, has nothing to
do with hash tables. See [Printed Representation and Read Syntax](#Printed-Representation).)

Obarrays are also a kind of hash table, but they are a different type
of object and are used only for recording interned symbols
(see [Creating and Interning Symbols](#Creating-Symbols)).

* [Creating Hash Tables](#Creating-Hash)
* [Hash Table Access](#Hash-Access)
* [Defining Hash Comparisons](#Defining-Hash)
* [Other Hash Table Functions](#Other-Hash)

---

Next: [Hash Table Access](#Hash-Access), Up: [Hash Tables](#Hash-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.1 Creating Hash Tables [¶](#Creating-Hash-Tables)

The principal function for creating a hash table is
`make-hash-table`.

Function: **make-hash-table** &rest keyword-args [¶](#index-make_002dhash_002dtable)
:   This function creates a new hash table according to the specified
    arguments. The arguments should consist of alternating keywords
    (particular symbols recognized specially) and values corresponding to
    them.

    Several keywords make sense in `make-hash-table`, but the only two
    that you really need to know about are `:test` and `:weakness`.

    `:test test`
    :   This specifies the method of key lookup for this hash table. The
        default is `eql`; `eq` and `equal` are other
        alternatives:

        `eql`
        :   Keys which are numbers are the same if they are `equal`, that
            is, if they are equal in value and either both are integers or both
            are floating point; otherwise, two distinct objects are never
            the same.

        `eq`
        :   Any two distinct Lisp objects are different as keys.

        `equal`
        :   Two Lisp objects are the same, as keys, if they are equal
            according to `equal`.

        You can use `define-hash-table-test` (see [Defining Hash Comparisons](#Defining-Hash)) to
        define additional possibilities for test.

    `:weakness weak`
    :   The weakness of a hash table specifies whether the presence of a key or
        value in the hash table preserves it from garbage collection.

        The value, weak, must be one of `nil`, `key`,
        `value`, `key-or-value`, `key-and-value`, or `t`
        which is an alias for `key-and-value`. If weak is `key`
        then the hash table does not prevent its keys from being collected as
        garbage (if they are not referenced anywhere else); if a particular key
        does get collected, the corresponding association is removed from the
        hash table.

        If weak is `value`, then the hash table does not prevent
        values from being collected as garbage (if they are not referenced
        anywhere else); if a particular value does get collected, the
        corresponding association is removed from the hash table.

        If weak is `key-and-value` or `t`, both the key and
        the value must be live in order to preserve the association. Thus,
        the hash table does not protect either keys or values from garbage
        collection; if either one is collected as garbage, that removes the
        association.

        If weak is `key-or-value`, either the key or
        the value can preserve the association. Thus, associations are
        removed from the hash table when both their key and value would be
        collected as garbage (if not for references from weak hash tables).

        The default for weak is `nil`, so that all keys and values
        referenced in the hash table are preserved from garbage collection.

    `:size size`
    :   This specifies a hint for how many associations you plan to store in the
        hash table. If you know the approximate number, you can make things a
        little more efficient by specifying it this way but since the hash
        table memory is managed automatically, the gain in speed is rarely
        significant.

You can also create a hash table using the printed representation
for hash tables. The Lisp reader can read this printed
representation, provided each element in the specified hash table has
a valid read syntax (see [Printed Representation and Read Syntax](#Printed-Representation)). For instance,
the following specifies a hash table containing the keys
`key1` and `key2` (both symbols) associated with `val1`
(a symbol) and `300` (a number) respectively.

```
#s(hash-table data (key1 val1 key2 300))
```

Note, however, that when using this in Emacs Lisp code, it’s
undefined whether this creates a new hash table or not. If you want
to create a new hash table, you should always use
`make-hash-table` (see [Self-Evaluating Forms](#Self_002dEvaluating-Forms)).

The printed representation for a hash table consists of ‘`#s`’
followed by a list beginning with ‘`hash-table`’. The rest of the
list should consist of zero or more property-value pairs specifying
the hash table’s properties and initial contents. The properties and
values are read literally. Valid property names are `test`,
`weakness` and `data`. The `data` property
should be a list of key-value pairs for the initial contents; the
other properties have the same meanings as the matching
`make-hash-table` keywords (`:test` and `:weakness`),
described above.

Note that you cannot specify a hash table whose initial contents
include objects that have no read syntax, such as buffers and frames.
Such objects may be added to the hash table after it is created.

---

Next: [Defining Hash Comparisons](#Defining-Hash), Previous: [Creating Hash Tables](#Creating-Hash), Up: [Hash Tables](#Hash-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.2 Hash Table Access [¶](#Hash-Table-Access)

This section describes the functions for accessing and storing
associations in a hash table. In general, any Lisp object can be used
as a hash key, unless the comparison method imposes limits. Any Lisp
object can also be used as the value.

Function: **gethash** key table &optional default [¶](#index-gethash)
:   This function looks up key in table, and returns its
    associated value—or default, if key has no
    association in table.

Function: **puthash** key value table [¶](#index-puthash)
:   This function enters an association for key in table, with
    value value. If key already has an association in
    table, value replaces the old associated value. This
    function always returns value.

Function: **remhash** key table [¶](#index-remhash)
:   This function removes the association for key from table, if
    there is one. If key has no association, `remhash` does
    nothing.

    **Common Lisp note:** In Common Lisp, `remhash` returns
    non-`nil` if it actually removed an association and `nil`
    otherwise. In Emacs Lisp, `remhash` always returns `nil`.

Function: **clrhash** table [¶](#index-clrhash)
:   This function removes all the associations from hash table table,
    so that it becomes empty. This is also called *clearing* the hash
    table. `clrhash` returns the empty table.

Function: **maphash** function table [¶](#index-maphash)
:   This function calls function once for each of the associations in
    table. The function function should accept two
    arguments—a key listed in table, and its associated
    value. `maphash` returns `nil`.

    function is allowed to call `puthash` to set a new value
    for key and `remhash` to remove key, but should not
    add, remove or modify other associations in table.

---

Next: [Other Hash Table Functions](#Other-Hash), Previous: [Hash Table Access](#Hash-Access), Up: [Hash Tables](#Hash-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.3 Defining Hash Comparisons [¶](#Defining-Hash-Comparisons)

You can define new methods of key lookup by means of
`define-hash-table-test`. In order to use this feature, you need
to understand how hash tables work, and what a *hash code* means.

You can think of a hash table conceptually as a large array of many
slots, each capable of holding one association. To look up a key,
`gethash` first computes an integer, the hash code, from the key.
It can reduce this integer modulo the length of the array, to produce an
index in the array. Then it looks in that slot, and if necessary in
other nearby slots, to see if it has found the key being sought.

Thus, to define a new method of key lookup, you need to specify both a
function to compute the hash code from a key, and a function to compare
two keys directly. The two functions should be consistent with each
other: that is, two keys’ hash codes should be the same if the keys
compare as equal. Also, since the two functions can be called at any
time (such as by the garbage collector), the functions should be free
of side effects and should return quickly, and their behavior should
depend on only on properties of the keys that do not change.

Function: **define-hash-table-test** name test-fn hash-fn [¶](#index-define_002dhash_002dtable_002dtest)
:   This function defines a new hash table test, named name.

    After defining name in this way, you can use it as the test
    argument in `make-hash-table`. When you do that, the hash table
    will use test-fn to compare key values, and hash-fn to compute
    a hash code from a key value.

    The function test-fn should accept two arguments, two keys, and
    return non-`nil` if they are considered the same.

    The function hash-fn should accept one argument, a key, and return
    an integer that is the hash code of that key. For good results, the
    function should use the whole range of fixnums for hash codes,
    including negative fixnums.

    The specified functions are stored in the property list of name
    under the property `hash-table-test`; the property value’s form is
    `(test-fn hash-fn)`.

Function: **sxhash-equal** obj [¶](#index-sxhash_002dequal)
:   This function returns a hash code for Lisp object obj.
    This is an integer that reflects the contents of obj
    and the other Lisp objects it points to.

    If two objects obj1 and obj2 are `equal`, then
    `(sxhash-equal obj1)` and `(sxhash-equal obj2)`
    are the same integer.

    If the two objects are not `equal`, the values returned by
    `sxhash-equal` are usually different, but not always.
    `sxhash-equal` is designed to be reasonably fast (since it’s used
    for indexing hash tables) so it won’t recurse deeply into nested
    structures. In addition; once in a rare while, by luck, you will
    encounter two distinct-looking simple objects that give the same
    result from `sxhash-equal`. So you can’t, in general, use
    `sxhash-equal` to check whether an object has changed.

    **Common Lisp note:** In Common Lisp a similar function is called
    `sxhash`. Emacs provides this name as a compatibility alias for
    `sxhash-equal`.

Function: **sxhash-eq** obj [¶](#index-sxhash_002deq)
:   This function returns a hash code for Lisp object obj. Its
    result reflects identity of obj, but not its contents.

    If two objects obj1 and obj2 are `eq`, then
    `(sxhash-eq obj1)` and `(sxhash-eq obj2)` are
    the same integer.

Function: **sxhash-eql** obj [¶](#index-sxhash_002deql)
:   This function returns a hash code for Lisp object obj suitable for
    `eql` comparison. In other words, it reflects identity of
    obj except for the case where the object is a bignum or a float
    number, in which case a hash code is generated for the value.

    If two objects obj1 and obj2 are `eql`, then
    `(sxhash-eql obj1)` and `(sxhash-eql obj2)` are
    the same integer.

This example creates a hash table whose keys are strings that are
compared case-insensitively.

```
(defun string-hash-ignore-case (a)
  (sxhash-equal (upcase a)))

(define-hash-table-test 'ignore-case
  'string-equal-ignore-case 'string-hash-ignore-case)

(make-hash-table :test 'ignore-case)
```

Here is how you could define a hash table test equivalent to the
predefined test value `equal`. The keys can be any Lisp object,
and equal-looking objects are considered the same key.

```
(define-hash-table-test 'contents-hash 'equal 'sxhash-equal)

(make-hash-table :test 'contents-hash)
```

Lisp programs should *not* rely on hash codes being preserved
between Emacs sessions, as the implementation of the hash functions
uses some details of the object storage that can change between
sessions and between different architectures.

---

Previous: [Defining Hash Comparisons](#Defining-Hash), Up: [Hash Tables](#Hash-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.4 Other Hash Table Functions [¶](#Other-Hash-Table-Functions)

Here are some other functions for working with hash tables.

Function: **hash-table-p** table [¶](#index-hash_002dtable_002dp)
:   This returns non-`nil` if table is a hash table object.

Function: **copy-hash-table** table [¶](#index-copy_002dhash_002dtable)
:   This function creates and returns a copy of table. Only the table
    itself is copied—the keys and values are shared.

Function: **hash-table-count** table [¶](#index-hash_002dtable_002dcount)
:   This function returns the actual number of entries in table.

Function: **hash-table-contains-p** key table [¶](#index-hash_002dtable_002dcontains_002dp)
:   This returns non-`nil` if there is an association for key in
    table.

Function: **hash-table-test** table [¶](#index-hash_002dtable_002dtest)
:   This returns the test value that was given when table was
    created, to specify how to hash and compare keys. See
    `make-hash-table` (see [Creating Hash Tables](#Creating-Hash)).

Function: **hash-table-weakness** table [¶](#index-hash_002dtable_002dweakness)
:   This function returns the weak value that was specified for hash
    table table.

Function: **hash-table-size** table [¶](#index-hash_002dtable_002dsize)
:   This returns the current allocation size of table. Since hash table
    allocation is managed automatically, this is rarely of interest.

---

Next: [Evaluation](#Evaluation), Previous: [Hash Tables](#Hash-Tables), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 9 Symbols [¶](#Symbols-1)

A *symbol* is an object with a unique name. This chapter
describes symbols, their components, their property lists, and how they
are created and interned. Separate chapters describe the use of symbols
as variables and as function names; see [Variables](#Variables), and
[Functions](#Functions). For the precise read syntax for symbols, see
[Symbol Type](#Symbol-Type).

You can test whether an arbitrary Lisp object is a symbol with
`symbolp`:

Function: **symbolp** object [¶](#index-symbolp)
:   This function returns `t` if object is a symbol, `nil`
    otherwise.

* [Symbol Components](#Symbol-Components)
* [Defining Symbols](#Definitions)
* [Creating and Interning Symbols](#Creating-Symbols)
* [Symbol Properties](#Symbol-Properties)
* [Shorthands](#Shorthands)
* [Symbols with Position](#Symbols-with-Position)

---

Next: [Defining Symbols](#Definitions), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.1 Symbol Components [¶](#Symbol-Components-1)

Each symbol has four components (or “cells”), each of which
references another object:

Print name [¶](#index-print-name-cell)
:   The symbol’s name.

Value [¶](#index-value-cell)
:   The symbol’s current value as a variable.

Function [¶](#index-function-cell)
:   The symbol’s function definition. It can also hold a symbol, a
    keymap, or a keyboard macro.

Property list [¶](#index-property-list-cell)
:   The symbol’s property list.

The print name cell always holds a string, and cannot be changed.
Each of the other three cells can be set to any Lisp object.

The print name cell holds the string that is the name of a symbol.
Since symbols are represented textually by their names, it is
important not to have two symbols with the same name. The Lisp reader
ensures this: every time it reads a symbol, it looks for an existing
symbol with the specified name before it creates a new one. To get a
symbol’s name, use the function `symbol-name` (see [Creating and Interning Symbols](#Creating-Symbols)). However, although each symbol has only one unique
*print name*, it is nevertheless possible to refer to that same
symbol via different alias names called “shorthands”
(see [Shorthands](#Shorthands)).

The value cell holds a symbol’s value as a variable, which is what
you get if the symbol itself is evaluated as a Lisp expression.
See [Variables](#Variables), for details about how values are set and retrieved,
including complications such as *local bindings* and *scoping
rules*. Most symbols can have any Lisp object as a value, but certain
special symbols have values that cannot be changed; these include
`nil` and `t`, and any symbol whose name starts with
‘`:`’ (those are called *keywords*). See [Variables that Never Change](#Constant-Variables).

The function cell holds a symbol’s function definition. Often, we
refer to “the function `foo`” when we really mean the function
stored in the function cell of `foo`; we make the distinction
explicit only when necessary. Typically, the function cell is used to
hold a function (see [Functions](#Functions)) or a macro (see [Macros](#Macros)).
However, it can also be used to hold a symbol (see [Symbol Function Indirection](#Function-Indirection)), keyboard macro (see [Keyboard Macros](#Keyboard-Macros)), keymap
(see [Keymaps](#Keymaps)), or autoload object (see [Autoloading](#Autoloading)). To get
the contents of a symbol’s function cell, use the function
`symbol-function` (see [Accessing Function Cell Contents](#Function-Cells)).

The property list cell normally should hold a correctly formatted
property list. To get a symbol’s property list, use the function
`symbol-plist`. See [Symbol Properties](#Symbol-Properties).

The value cell may be *void*, which means that the cell does not
reference any object. (This is not the same thing as holding the symbol
`void`, nor the same as holding the symbol `nil`.) Examining
a value cell that is void results in an error, such as ‘`Symbol's
value as variable is void`’.

Because each symbol has separate value and function cells, the names
of variables and functions do not conflict. For example, the symbol
`buffer-file-name` has a value (the name of the file being visited
in the current buffer) as well as a function definition (a primitive
function that returns the name of the file):

```
buffer-file-name
     ⇒ "/gnu/elisp/symbols.texi"
(symbol-function 'buffer-file-name)
     ⇒ #<subr buffer-file-name>
```

---

Next: [Creating and Interning Symbols](#Creating-Symbols), Previous: [Symbol Components](#Symbol-Components), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
