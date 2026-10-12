### 5.3 Accessing Elements of Lists [¶](#Accessing-Elements-of-Lists)

Function: **car** cons-cell [¶](#index-car)
:   This function returns the value referred to by the first slot of the
    cons cell cons-cell. In other words, it returns the CAR of
    cons-cell.

    As a special case, if cons-cell is `nil`, this function
    returns `nil`. Therefore, any list is a valid argument. An
    error is signaled if the argument is not a cons cell or `nil`.

    ```
    (car '(a b c))
         ⇒ a
    ```

    ```
    (car '())
         ⇒ nil
    ```

Function: **cdr** cons-cell [¶](#index-cdr)
:   This function returns the value referred to by the second slot of the
    cons cell cons-cell. In other words, it returns the CDR of
    cons-cell.

    As a special case, if cons-cell is `nil`, this function
    returns `nil`; therefore, any list is a valid argument. An error
    is signaled if the argument is not a cons cell or `nil`.

    ```
    (cdr '(a b c))
         ⇒ (b c)
    ```

    ```
    (cdr '())
         ⇒ nil
    ```

Function: **car-safe** object [¶](#index-car_002dsafe)
:   This function lets you take the CAR of a cons cell while avoiding
    errors for other data types. It returns the CAR of object if
    object is a cons cell, `nil` otherwise. This is in contrast
    to `car`, which signals an error if object is not a list.

    ```
    (car-safe object)
    ≡
    (let ((x object))
      (if (consp x)
          (car x)
        nil))
    ```

Function: **cdr-safe** object [¶](#index-cdr_002dsafe)
:   This function lets you take the CDR of a cons cell while
    avoiding errors for other data types. It returns the CDR of
    object if object is a cons cell, `nil` otherwise.
    This is in contrast to `cdr`, which signals an error if
    object is not a list.

    ```
    (cdr-safe object)
    ≡
    (let ((x object))
      (if (consp x)
          (cdr x)
        nil))
    ```

Macro: **pop** listname [¶](#index-pop)
:   This macro provides a convenient way to examine the CAR of a
    list, and take it off the list, all at once. It operates on the list
    stored in listname. It removes the first element from the list,
    saves the CDR into listname, then returns the removed
    element.

    In the simplest case, listname is an unquoted symbol naming a
    list; in that case, this macro is equivalent to `(prog1 (car listname) (setq listname (cdr listname)))`.

    ```
    x
         ⇒ (a b c)
    (pop x)
         ⇒ a
    x
         ⇒ (b c)
    ```

    More generally, listname can be a generalized variable. In that
    case, this macro saves into listname using `setf`.
    See [Generalized Variables](#Generalized-Variables).

    For the `push` macro, which adds an element to a list,
    See [Modifying List Variables](#List-Variables).

Function: **nth** n list [¶](#index-nth)
:   This function returns the nth element of list. Elements
    are numbered starting with zero, so the CAR of list is
    element number zero. If the length of list is n or less,
    the value is `nil`.

    ```
    (nth 2 '(1 2 3 4))
         ⇒ 3
    ```

    ```
    (nth 10 '(1 2 3 4))
         ⇒ nil

    (nth n x) ≡ (car (nthcdr n x))
    ```

    The function `elt` is similar, but applies to any kind of sequence.
    For historical reasons, it takes its arguments in the opposite order.
    See [Sequences](#Sequence-Functions).

Function: **nthcdr** n list [¶](#index-nthcdr)
:   This function returns the nth CDR of list. In other
    words, it skips past the first n links of list and returns
    what follows.

    If n is zero, `nthcdr` returns all of
    list. If the length of list is n or less,
    `nthcdr` returns `nil`.

    An alias for `nthcdr` is `drop`.

    ```
    (nthcdr 1 '(1 2 3 4))
         ⇒ (2 3 4)
    ```

    ```
    (nthcdr 10 '(1 2 3 4))
         ⇒ nil
    ```

    ```
    (nthcdr 0 '(1 2 3 4))
         ⇒ (1 2 3 4)
    ```

Function: **take** n list [¶](#index-take)
:   This function returns the n first elements of list. Essentially,
    it returns the part of list that `nthcdr` skips.

    `take` returns list if shorter than n elements;
    it returns `nil` if n is zero or negative.

    In general, `(append (take n list) (drop n list))`
    will return a list equal to list.

    ```
    (take 3 '(a b c d))
         ⇒ (a b c)
    ```

    ```
    (take 10 '(a b c d))
         ⇒ (a b c d)
    ```

    ```
    (take 0 '(a b c d))
         ⇒ nil
    ```

Function: **ntake** n list [¶](#index-ntake)
:   This is a version of `take` that works by destructively modifying
    the list structure of the argument. That makes it faster, but the
    original value of list may be lost.

    `ntake` returns list unmodified if shorter than n
    elements; it returns `nil` if n is zero or negative.
    Otherwise, it returns list truncated to its first n
    elements.

    This means that it is usually a good idea to use the return value and
    not just rely on the truncation effect unless n is known to be
    positive.

Function: **drop-while** pred list [¶](#index-drop_002dwhile)
:   This function skips leading list elements for which the predicate pred
    returns non-`nil`, and returns the rest.

    ```
    (drop-while #'numberp '(1 2 a b 3 4))
         ⇒ (a b 3 4)
    ```

Function: **take-while** pred list [¶](#index-take_002dwhile)
:   This function returns the leading list elements for which the predicate
    pred returns non-`nil`, and ignores the rest.

    In general,
    `(append (take-while p list) (drop-while p list))`
    will return a list equal to list.

    ```
    (take-while #'numberp '(1 2 a b 3 4))
         ⇒ (1 2)
    ```

Function: **all** pred list [¶](#index-all)
:   This function returns `t` if pred is true for all elements in
    list.

    ```
    (all #'numberp '(1 2 3 4))     ⇒ t
    (all #'numberp '(1 2 a b 3 4)) ⇒ nil
    (all #'numberp '())            ⇒ t
    ```

Function: **member-if** pred list [¶](#index-member_002dif)
:   This function returns non-`nil` if pred is true for at least
    one element in list. The returned value is the longest list
    suffix whose first element satisfies pred.

    ```
    (member-if #'symbolp '(1 2 3 4))     ⇒ nil
    (member-if #'symbolp '(1 2 a b 3 4)) ⇒ (a b 3 4)
    (member-if #'symbolp '())            ⇒ nil
    ```

Function: **any** pred list [¶](#index-any)
:   This function is an alias for `member-if`. It may be preferable in
    contexts which do not make use of the returned value but only whether or
    not it was nil.

Function: **last** list &optional n [¶](#index-last)
:   This function returns the last link of list. The `car` of
    this link is the list’s last element. If list is null,
    `nil` is returned. If n is non-`nil`, the
    nth-to-last link is returned instead, or the whole of list
    if n is bigger than list’s length.

Function: **safe-length** list [¶](#index-safe_002dlength)
:   This function returns the length of list, with no risk of either
    an error or an infinite loop. It generally returns the number of
    distinct cons cells in the list. However, for circular lists,
    the value is just an upper bound; it is often too large.

    If list is not `nil` or a cons cell, `safe-length`
    returns 0.

The most common way to compute the length of a list, when you are not
worried that it may be circular, is with `length`. See [Sequences](#Sequence-Functions).

Function: **caar** cons-cell [¶](#index-caar)
:   This is the same as `(car (car cons-cell))`.

Function: **cadr** cons-cell [¶](#index-cadr)
:   This is the same as `(car (cdr cons-cell))`
    or `(nth 1 cons-cell)`.

Function: **cdar** cons-cell [¶](#index-cdar)
:   This is the same as `(cdr (car cons-cell))`.

Function: **cddr** cons-cell [¶](#index-cddr)
:   This is the same as `(cdr (cdr cons-cell))`
    or `(nthcdr 2 cons-cell)`.

In addition to the above, 24 additional compositions of `car` and
`cdr` are defined as `cxxxr` and `cxxxxr`,
where each `x` is either `a` or `d`. `cadr`,
`caddr`, and `cadddr` pick out the second, third or fourth
elements of a list, respectively. `cl-lib` provides the same
under the names `cl-second`, `cl-third`, and
`cl-fourth`. See [List Functions](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#List-Functions) in Common Lisp
Extensions.

Function: **butlast** x &optional n [¶](#index-butlast)
:   This function returns the list x with the last element,
    or the last n elements, removed. If n is greater
    than zero it makes a copy of the list so as not to damage the
    original list. In general, `(append (butlast x n)
    (last x n))` will return a list equal to x.

Function: **nbutlast** x &optional n [¶](#index-nbutlast)
:   This is a version of `butlast` that works by destructively
    modifying the `cdr` of the appropriate element, rather than
    making a copy of the list.

---

Next: [Modifying List Variables](#List-Variables), Previous: [Accessing Elements of Lists](#List-Elements), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.4 Building Cons Cells and Lists [¶](#Building-Cons-Cells-and-Lists)

Many functions build lists, as lists reside at the very heart of Lisp.
`cons` is the fundamental list-building function; however, it is
interesting to note that `list` is used more times in the source
code for Emacs than `cons`.

Function: **cons** object1 object2 [¶](#index-cons)
:   This function is the most basic function for building new list
    structure. It creates a new cons cell, making object1 the
    CAR, and object2 the CDR. It then returns the new
    cons cell. The arguments object1 and object2 may be any
    Lisp objects, but most often object2 is a list.

    ```
    (cons 1 '(2))
         ⇒ (1 2)
    ```

    ```
    (cons 1 '())
         ⇒ (1)
    ```

    ```
    (cons 1 2)
         ⇒ (1 . 2)
    ```

    `cons` is often used to add a single element to the front of a
    list. This is called *consing the element onto the list*.
    [6](#FOOT6)
    For example:

    ```
    (setq list (cons newelt list))
    ```

    Note that there is no conflict between the variable named `list`
    used in this example and the function named `list` described below;
    any symbol can serve both purposes.

Function: **list** &rest objects [¶](#index-list)
:   This function creates a list with objects as its elements. The
    resulting list is always `nil`-terminated. If no objects
    are given, the empty list is returned.

    ```
    (list 1 2 3 4 5)
         ⇒ (1 2 3 4 5)
    ```

    ```
    (list 1 2 '(3 4 5) 'foo)
         ⇒ (1 2 (3 4 5) foo)
    ```

    ```
    (list)
         ⇒ nil
    ```

Function: **make-list** length object [¶](#index-make_002dlist)
:   This function creates a list of length elements, in which each
    element is object. Compare `make-list` with
    `make-string` (see [Creating Strings](#Creating-Strings)).

    ```
    (make-list 3 'pigs)
         ⇒ (pigs pigs pigs)
    ```

    ```
    (make-list 0 'pigs)
         ⇒ nil
    ```

    ```
    (setq l (make-list 3 '(a b)))
         ⇒ ((a b) (a b) (a b))
    (eq (car l) (cadr l))
         ⇒ t
    ```

Function: **append** &rest sequences [¶](#index-append)
:   This function returns a list containing all the elements of
    sequences. The sequences may be lists, vectors,
    bool-vectors, or strings, but the last one should usually be a list.
    All arguments except the last one are copied, so none of the arguments
    is altered. (See `nconc` in [Functions that Rearrange Lists](#Rearrangement), for a way to join
    lists with no copying.)

    More generally, the final argument to `append` may be any Lisp
    object. The final argument is not copied or converted; it becomes the
    CDR of the last cons cell in the new list. If the final argument
    is itself a list, then its elements become in effect elements of the
    result list. If the final element is not a list, the result is a
    dotted list since its final CDR is not `nil` as required
    in a proper list (see [Lists and Cons Cells](#Cons-Cells)).

Here is an example of using `append`:

```
(setq trees '(pine oak))
     ⇒ (pine oak)
(setq more-trees (append '(maple birch) trees))
     ⇒ (maple birch pine oak)
```

```

```

```
trees
     ⇒ (pine oak)
more-trees
     ⇒ (maple birch pine oak)
```

```
(eq trees (cdr (cdr more-trees)))
     ⇒ t
```

You can see how `append` works by looking at a box diagram. The
variable `trees` is set to the list `(pine oak)` and then the
variable `more-trees` is set to the list `(maple birch pine
oak)`. However, the variable `trees` continues to refer to the
original list:

```
more-trees                trees
|                           |
|     --- ---      --- ---   -> --- ---      --- ---
 --> |   |   |--> |   |   |--> |   |   |--> |   |   |--> nil
      --- ---      --- ---      --- ---      --- ---
       |            |            |            |
       |            |            |            |
        --> maple    -->birch     --> pine     --> oak
```

An empty sequence contributes nothing to the value returned by
`append`. As a consequence of this, a final `nil` argument
forces a copy of the previous argument:

```
trees
     ⇒ (pine oak)
```

```
(setq wood (append trees nil))
     ⇒ (pine oak)
```

```
wood
     ⇒ (pine oak)
```

```
(eq wood trees)
     ⇒ nil
```

This once was the usual way to copy a list, before the function
`copy-sequence` was invented. See [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors).

Here we show the use of vectors and strings as arguments to `append`:

```
(append [a b] "cd" nil)
     ⇒ (a b 99 100)
```

Here’s how to convert a string into a list of its characters:

```
(append "abcd" nil)
     ⇒ (97 98 99 100)
```

The function `string-to-list` is a handy shortcut for the above.

With the help of `apply` (see [Calling Functions](#Calling-Functions)), we can append
all the lists in a list of lists:

```
(apply 'append '((a b c) nil (x y z) nil))
     ⇒ (a b c x y z)
```

If no sequences are given, `nil` is returned:

```
(append)
     ⇒ nil
```

Here are some examples where the final argument is not a list:

```
(append '(x y) 'z)
     ⇒ (x y . z)
(append '(x y) [z])
     ⇒ (x y . [z])
```

The second example shows that when the final argument is a sequence but
not a list, the sequence’s elements do not become elements of the
resulting list. Instead, the sequence becomes the final CDR, like
any other non-list final argument.

As an exception, if all the arguments but the last are `nil` and
the last argument is not a list, the return value is that last argument
unchanged (i.e., in this case the return value is not a list):

```
(append nil nil "abcd")
     ⇒ "abcd"
```

Function: **copy-tree** tree &optional vectors-and-records [¶](#index-copy_002dtree)
:   This function returns a copy of the tree tree. If tree is a
    cons cell, this makes a new cons cell with the same CAR and
    CDR, then recursively copies the CAR and CDR in the
    same way.

    Normally, when tree is anything other than a cons cell,
    `copy-tree` simply returns tree. However, if
    vectors-and-records is non-`nil`, it copies vectors and records
    too (and operates recursively on their elements). The tree
    argument must not contain cycles.

Function: **flatten-tree** tree [¶](#index-flatten_002dtree)
:   This function returns a “flattened” copy of tree, that is,
    a list containing all the non-`nil` terminal nodes, or leaves, of
    the tree of cons cells rooted at tree. Leaves in the returned
    list are in the same order as in tree.

```
(flatten-tree '(1 (2 . 3) nil (4 5 (6)) 7))
    ⇒(1 2 3 4 5 6 7)
```

Function: **ensure-list** object [¶](#index-ensure_002dlist)
:   This function returns object as a list. If object is
    already a list, the function returns it; otherwise, the function
    returns a one-element list containing object.

    This is usually useful if you have a variable that may or may not be a
    list, and you can then say, for instance:

    ```
    (dolist (elem (ensure-list foo))
      (princ elem))
    ```

Function: **ensure-proper-list** object [¶](#index-ensure_002dproper_002dlist)
:   This function returns object as a proper list (see [(elisp)Cons
    Cells](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Cons-Cells)). If object is already a proper list, the function returns
    it; otherwise, the function returns a one-element list containing
    object.

    If object might be a long list, prefer `ensure-list`, because
    the latter function runs in constant time, whereas
    `ensure-proper-list` runs in linear time. For short lists this
    function is a convenient way to treat cons-cells as non-lists:

    ```
    (ensure-list '(1 . 2))
        ⇒(1 . 2)

    (ensure-proper-list '(1 . 2))
        ⇒((1 . 2))
    ```

Function: **number-sequence** from &optional to separation [¶](#index-number_002dsequence)
:   This function returns a list of numbers starting with from and
    incrementing by separation, and ending at or just before
    to. separation can be positive or negative and defaults
    to 1. If to is `nil` or numerically equal to from,
    the value is the one-element list `(from)`. If to is
    less than from with a positive separation, or greater than
    from with a negative separation, the value is `nil`
    because those arguments specify an empty sequence.

    If separation is 0 and to is neither `nil` nor
    numerically equal to from, `number-sequence` signals an
    error, since those arguments specify an infinite sequence.

    All arguments are numbers.
    Floating-point arguments can be tricky, because floating-point
    arithmetic is inexact. For instance, depending on the machine, it may
    quite well happen that `(number-sequence 0.4 0.6 0.2)` returns
    the one element list `(0.4)`, whereas
    `(number-sequence 0.4 0.8 0.2)` returns a list with three
    elements. The nth element of the list is computed by the exact
    formula `(+ from (* n separation))`. Thus, if
    one wants to make sure that to is included in the list, one can
    pass an expression of this exact type for to. Alternatively,
    one can replace to with a slightly larger value (or a slightly
    more negative value if separation is negative).

    Some examples:

    ```
    (number-sequence 4 9)
         ⇒ (4 5 6 7 8 9)
    (number-sequence 9 4 -1)
         ⇒ (9 8 7 6 5 4)
    (number-sequence 9 4 -2)
         ⇒ (9 7 5)
    (number-sequence 8)
         ⇒ (8)
    (number-sequence 8 5)
         ⇒ nil
    (number-sequence 5 8 -1)
         ⇒ nil
    (number-sequence 1.5 6 2)
         ⇒ (1.5 3.5 5.5)
    ```

---

Next: [Modifying Existing List Structure](#Modifying-Lists), Previous: [Building Cons Cells and Lists](#Building-Lists), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.5 Modifying List Variables [¶](#Modifying-List-Variables)

These functions, and one macro, provide convenient ways
to modify a list which is stored in a variable.

Macro: **push** element listname [¶](#index-push)
:   This macro creates a new list whose CAR is element and
    whose CDR is the list specified by listname, and saves that
    list in listname. In the simplest case, listname is an
    unquoted symbol naming a list, and this macro is equivalent
    to `(setq listname (cons element listname))`.

    ```
    (setq l '(a b))
         ⇒ (a b)
    (push 'c l)
         ⇒ (c a b)
    l
         ⇒ (c a b)
    ```

    More generally, `listname` can be a generalized variable. In
    that case, this macro does the equivalent of `(setf listname (cons element listname))`.
    See [Generalized Variables](#Generalized-Variables).

    For the `pop` macro, which removes the first element from a list,
    See [Accessing Elements of Lists](#List-Elements).

Two functions modify lists that are the values of variables.

Function: **add-to-list** symbol element &optional append compare-fn [¶](#index-add_002dto_002dlist)
:   This function sets the variable symbol by consing element
    onto the old value, if element is not already a member of that
    value. It returns the resulting list, whether updated or not. The
    value of symbol had better be a list already before the call.
    `add-to-list` uses compare-fn to compare element
    against existing list members; if compare-fn is `nil`, it
    uses `equal`.

    Normally, if element is added, it is added to the front of
    symbol, but if the optional argument append is
    non-`nil`, it is added at the end.

    The argument symbol is not implicitly quoted; `add-to-list`
    is an ordinary function, like `set` and unlike `setq`. Quote
    the argument yourself if that is what you want.

    This function is for adding elements to configuration variables such as
    `load-path` (see [Library Search](#Library-Search)), `image-load-path`
    (see [Defining Images](#Defining-Images)), etc. Its code includes quite a few special
    checks for these uses, and emits warnings in support of them. For this
    reason, we recommend against using it in Lisp programs for constructing
    arbitrary lists; use `push` instead. See [Modifying List Variables](#List-Variables).

    Do not use this function when symbol refers to a lexical
    variable.

Here’s a scenario showing how to use `add-to-list`:

```
(setq foo '(a b))
     ⇒ (a b)

(add-to-list 'foo 'c)     ;; Add c.
     ⇒ (c a b)

(add-to-list 'foo 'b)     ;; No effect.
     ⇒ (c a b)

foo                       ;; foo was changed.
     ⇒ (c a b)
```

An equivalent expression for `(add-to-list 'var
value)` is this:

```
(if (member value var)
    var
  (setq var (cons value var)))
```

Function: **add-to-ordered-list** symbol element &optional order [¶](#index-add_002dto_002dordered_002dlist)
:   This function sets the variable symbol by inserting
    element into the old value, which must be a list, at the
    position specified by order. If element is already a
    member of the list, its position in the list is adjusted according
    to order. Membership is tested using `eq`.
    This function returns the resulting list, whether updated or not.

    The order is typically a number (integer or float), and the
    elements of the list are sorted in non-decreasing numerical order.

    order may also be omitted or `nil`. Then the numeric order
    of element stays unchanged if it already has one; otherwise,
    element has no numeric order. Elements without a numeric list
    order are placed at the end of the list, in no particular order.

    Any other value for order removes the numeric order of element
    if it already has one; otherwise, it is equivalent to `nil`.

    The argument symbol is not implicitly quoted;
    `add-to-ordered-list` is an ordinary function, like `set`
    and unlike `setq`. Quote the argument yourself if necessary.

    The ordering information is stored in a hash table on symbol’s
    `list-order` property.
    symbol cannot refer to a lexical variable.

Here’s a scenario showing how to use `add-to-ordered-list`:

```
(setq foo '())
     ⇒ nil

(add-to-ordered-list 'foo 'a 1)     ;; Add a.
     ⇒ (a)

(add-to-ordered-list 'foo 'c 3)     ;; Add c.
     ⇒ (a c)

(add-to-ordered-list 'foo 'b 2)     ;; Add b.
     ⇒ (a b c)

(add-to-ordered-list 'foo 'b 4)     ;; Move b.
     ⇒ (a c b)

(add-to-ordered-list 'foo 'd)       ;; Append d.
     ⇒ (a c b d)

(add-to-ordered-list 'foo 'e)       ;; Add e.
     ⇒ (a c b e d)

foo                       ;; foo was changed.
     ⇒ (a c b e d)
```

---

Next: [Using Lists as Sets](#Sets-And-Lists), Previous: [Modifying List Variables](#List-Variables), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.6 Modifying Existing List Structure [¶](#Modifying-Existing-List-Structure)

You can modify the CAR and CDR contents of a cons cell with the
primitives `setcar` and `setcdr`. These are destructive
operations because they change existing list structure.
Destructive operations should be applied only to mutable lists,
that is, lists constructed via `cons`, `list` or similar
operations. Lists created by quoting are part of the program and
should not be changed by destructive operations. See [Mutability](#Mutability).

> **Common Lisp note:** Common Lisp uses functions `rplaca` and
> `rplacd` to alter list structure; they change structure the same
> way as `setcar` and `setcdr`, but the Common Lisp functions
> return the cons cell while `setcar` and `setcdr` return the
> new CAR or CDR.

* [Altering List Elements with `setcar`](#Setcar)
* [Altering the CDR of a List](#Setcdr)
* [Functions that Rearrange Lists](#Rearrangement)

---

Next: [Altering the CDR of a List](#Setcdr), Up: [Modifying Existing List Structure](#Modifying-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.6.1 Altering List Elements with `setcar` [¶](#Altering-List-Elements-with-setcar)

Changing the CAR of a cons cell is done with `setcar`. When
used on a list, `setcar` replaces one element of a list with a
different element.

Function: **setcar** cons object [¶](#index-setcar)
:   This function stores object as the new CAR of cons,
    replacing its previous CAR. In other words, it changes the
    CAR slot of cons to refer to object. It returns the
    value object. For example:

    ```
    (setq x (list 1 2))
         ⇒ (1 2)
    ```

    ```
    (setcar x 4)
         ⇒ 4
    ```

    ```
    x
         ⇒ (4 2)
    ```

When a cons cell is part of the shared structure of several lists,
storing a new CAR into the cons changes one element of each of
these lists. Here is an example:

```
;; Create two lists that are partly shared.
(setq x1 (list 'a 'b 'c))
     ⇒ (a b c)
(setq x2 (cons 'z (cdr x1)))
     ⇒ (z b c)
```

```

```

```
;; Replace the CAR of a shared link.
(setcar (cdr x1) 'foo)
     ⇒ foo
x1                           ; Both lists are changed.
     ⇒ (a foo c)
x2
     ⇒ (z foo c)
```

```

```

```
;; Replace the CAR of a link that is not shared.
(setcar x1 'baz)
     ⇒ baz
x1                           ; Only one list is changed.
     ⇒ (baz foo c)
x2
     ⇒ (z foo c)
```

Here is a graphical depiction of the shared structure of the two lists
in the variables `x1` and `x2`, showing why replacing `b`
changes them both:

```
        --- ---        --- ---      --- ---
x1---> |   |   |----> |   |   |--> |   |   |--> nil
        --- ---        --- ---      --- ---
         |        -->   |            |
         |       |      |            |
          --> a  |       --> b        --> c
                 |
       --- ---   |
x2--> |   |   |--
       --- ---
        |
        |
         --> z
```

Here is an alternative form of box diagram, showing the same relationship:

```
x1:
 --------------       --------------       --------------
| car   | cdr  |     | car   | cdr  |     | car   | cdr  |
|   a   |   o------->|   b   |   o------->|   c   |  nil |
|       |      |  -->|       |      |     |       |      |
 --------------  |    --------------       --------------
                 |
x2:              |
 --------------  |
| car   | cdr  | |
|   z   |   o----
|       |      |
 --------------
```

---

Next: [Functions that Rearrange Lists](#Rearrangement), Previous: [Altering List Elements with `setcar`](#Setcar), Up: [Modifying Existing List Structure](#Modifying-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.6.2 Altering the CDR of a List [¶](#Altering-the-CDR-of-a-List)

The lowest-level primitive for modifying a CDR is `setcdr`:

Function: **setcdr** cons object [¶](#index-setcdr)
:   This function stores object as the new CDR of cons,
    replacing its previous CDR. In other words, it changes the
    CDR slot of cons to refer to object. It returns the
    value object.

Here is an example of replacing the CDR of a list with a
different list. All but the first element of the list are removed in
favor of a different sequence of elements. The first element is
unchanged, because it resides in the CAR of the list, and is not
reached via the CDR.

```
(setq x (list 1 2 3))
     ⇒ (1 2 3)
```

```
(setcdr x '(4))
     ⇒ (4)
```

```
x
     ⇒ (1 4)
```

You can delete elements from the middle of a list by altering the
CDRs of the cons cells in the list. For example, here we delete
the second element, `b`, from the list `(a b c)`, by changing
the CDR of the first cons cell:

```
(setq x1 (list 'a 'b 'c))
     ⇒ (a b c)
(setcdr x1 (cdr (cdr x1)))
     ⇒ (c)
x1
     ⇒ (a c)
```

Here is the result in box notation:

```
                   --------------------
                  |                    |
 --------------   |   --------------   |    --------------
| car   | cdr  |  |  | car   | cdr  |   -->| car   | cdr  |
|   a   |   o-----   |   b   |   o-------->|   c   |  nil |
|       |      |     |       |      |      |       |      |
 --------------       --------------        --------------
```

The second cons cell, which previously held the element `b`, still
exists and its CAR is still `b`, but it no longer forms part
of this list.

It is equally easy to insert a new element by changing CDRs:

```
(setq x1 (list 'a 'b 'c))
     ⇒ (a b c)
(setcdr x1 (cons 'd (cdr x1)))
     ⇒ (d b c)
x1
     ⇒ (a d b c)
```

Here is this result in box notation:

```
 --------------        -------------       -------------
| car  | cdr   |      | car  | cdr  |     | car  | cdr  |
|   a  |   o   |   -->|   b  |   o------->|   c  |  nil |
|      |   |   |  |   |      |      |     |      |      |
 --------- | --   |    -------------       -------------
           |      |
     -----         --------
    |                      |
    |    ---------------   |
    |   | car   | cdr   |  |
     -->|   d   |   o------
        |       |       |
         ---------------
```

---

Previous: [Altering the CDR of a List](#Setcdr), Up: [Modifying Existing List Structure](#Modifying-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.6.3 Functions that Rearrange Lists [¶](#Functions-that-Rearrange-Lists)

Here are some functions that rearrange lists destructively by
modifying the CDRs of their component cons cells. These functions
are destructive because they chew up the original lists passed
to them as arguments, relinking their cons cells to form a new list that
is the returned value.

See `delq`, in [Using Lists as Sets](#Sets-And-Lists), for another function
that modifies cons cells.

Function: **nconc** &rest lists [¶](#index-nconc)
:   This function returns a list containing all the elements of lists.
    Unlike `append` (see [Building Cons Cells and Lists](#Building-Lists)), the lists are
    *not* copied. Instead, the last CDR of each of the
    lists is changed to refer to the following list. The last of the
    lists is not altered. For example:

    ```
    (setq x (list 1 2 3))
         ⇒ (1 2 3)
    ```

    ```
    (nconc x '(4 5))
         ⇒ (1 2 3 4 5)
    ```

    ```
    x
         ⇒ (1 2 3 4 5)
    ```

    Since the last argument of `nconc` is not itself modified, it is
    reasonable to use a constant list, such as `'(4 5)`, as in the
    above example. For the same reason, the last argument need not be a
    list:

    ```
    (setq x (list 1 2 3))
         ⇒ (1 2 3)
    ```

    ```
    (nconc x 'z)
         ⇒ (1 2 3 . z)
    ```

    ```
    x
         ⇒ (1 2 3 . z)
    ```

    However, the other arguments (all but the last) should be mutable
    lists. They can be dotted lists, whose last CDRs are then
    replaced with the next argument:

    ```
    (nconc (cons 1 2) (cons 3 (cons 4 5)) 'z)
         ⇒ (1 3 4 . z)
    ```

    A common pitfall is to use a constant list as a non-last argument to
    `nconc`. If you do this, the resulting behavior is undefined
    (see [Self-Evaluating Forms](#Self_002dEvaluating-Forms)). It is possible that your program
    will change each time you run it! Here is what might happen (though
    this is not guaranteed to happen):

    ```
    (defun add-foo (x)            ; We want this function to add
      (nconc '(foo) x))           ;   foo to the front of its arg.
    ```

    ```

    ```

    ```
    (symbol-function 'add-foo)
         ⇒ #f(lambda (x) [t] (nconc '(foo) x))
    ```

    ```

    ```

    ```
    (setq xx (add-foo '(1 2)))    ; It seems to work.
         ⇒ (foo 1 2)
    ```

    ```
    (setq xy (add-foo '(3 4)))    ; What happened?
         ⇒ (foo 1 2 3 4)
    ```

    ```
    (eq xx xy)
         ⇒ t
    ```

    ```

    ```

    ```
    (symbol-function 'add-foo)
         ⇒ #f(lambda (x) [t] (nconc '(foo 1 2 3 4) x))
    ```

---

Next: [Association Lists](#Association-Lists), Previous: [Modifying Existing List Structure](#Modifying-Lists), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.7 Using Lists as Sets [¶](#Using-Lists-as-Sets)

A list can represent an unordered mathematical set—simply consider
a value an element of a set if it appears in the list, and ignore the
order of the list. To form the union of two sets, use `append`
(as long as you don’t mind having duplicate elements). You can remove
`equal` duplicates using `delete-dups` or `seq-uniq`.
Other useful functions for sets include `memq` and `delq`,
and their `equal` versions, `member` and `delete`.

> **Common Lisp note:** Common Lisp has functions `union` (which
> avoids duplicate elements) and `intersection` for set operations.
> In Emacs Lisp, variants of these facilities are provided by the
> `cl-lib` library. See [Lists as Sets](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Lists-as-Sets) in Common Lisp Extensions.

Function: **memq** object list [¶](#index-memq)
:   This function tests to see whether object is a member of
    list. If it is, `memq` returns a list starting with the
    first occurrence of object. Otherwise, it returns `nil`.
    The letter ‘`q`’ in `memq` says that it uses `eq` to
    compare object against the elements of the list. For example:

    ```
    (memq 'b '(a b c b a))
         ⇒ (b c b a)
    ```

    ```
    (memq '(2) '((1) (2)))    ; The two (2)s need not be eq.
         ⇒ Unspecified; might be nil or ((2)).
    ```

Function: **delq** object list [¶](#index-delq)
:   This function destructively removes all elements `eq` to
    object from list, and returns the resulting list. The
    letter ‘`q`’ in `delq` says that it uses `eq` to compare
    object against the elements of the list, like `memq` and
    `remq`.

    Typically, when you invoke `delq`, you should use the return
    value by assigning it to the variable which held the original list.
    The reason for this is explained below.

The `delq` function deletes elements from the front of the list
by simply advancing down the list, and returning a sublist that starts
after those elements. For example:

```
(delq 'a '(a b c)) ≡ (cdr '(a b c))
```

When an element to be deleted appears in the middle of the list,
removing it involves changing the CDRs (see [Altering the CDR of a List](#Setcdr)).

```
(setq sample-list (list 'a 'b 'c '(4)))
     ⇒ (a b c (4))
```

```
(delq 'a sample-list)
     ⇒ (b c (4))
```

```
sample-list
     ⇒ (a b c (4))
```

```
(delq 'c sample-list)
     ⇒ (a b (4))
```

```
sample-list
     ⇒ (a b (4))
```

Note that `(delq 'c sample-list)` modifies `sample-list` to
splice out the third element, but `(delq 'a sample-list)` does not
splice anything—it just returns a shorter list. Don’t assume that a
variable which formerly held the argument list now has fewer
elements, or that it still holds the original list! Instead, save the
result of `delq` and use that. Most often we store the result back
into the variable that held the original list:

```
(setq flowers (delq 'rose flowers))
```

In the following example, the `(list 4)` that `delq` attempts to match
and the `(4)` in the `sample-list` are `equal` but not `eq`:

```
(delq (list 4) sample-list)
     ⇒ (a c (4))
```

If you want to delete elements that are `equal` to a given value,
use `delete` (see below).

Function: **remq** object list [¶](#index-remq)
:   This function returns a copy of list, with all elements removed
    which are `eq` to object. The letter ‘`q`’ in `remq`
    says that it uses `eq` to compare object against the elements
    of `list`.

    ```
    (setq sample-list (list 'a 'b 'c 'a 'b 'c))
         ⇒ (a b c a b c)
    ```

    ```
    (remq 'a sample-list)
         ⇒ (b c b c)
    ```

    ```
    sample-list
         ⇒ (a b c a b c)
    ```

Function: **memql** object list [¶](#index-memql)
:   The function `memql` tests to see whether object is a member
    of list, comparing members with object using `eql`,
    so floating-point elements are compared by value.
    If object is a member, `memql` returns a list starting with
    its first occurrence in list. Otherwise, it returns `nil`.

    Compare this with `memq`:

    ```
    (memql 1.2 '(1.1 1.2 1.3))  ; 1.2 and 1.2 are eql.
         ⇒ (1.2 1.3)
    ```

    ```
    (memq 1.2 '(1.1 1.2 1.3))  ; The two 1.2s need not be eq.
         ⇒ Unspecified; might be nil or (1.2 1.3).
    ```

The following three functions are like `memq`, `delq` and
`remq`, but use `equal` rather than `eq` to compare
elements. See [Equality Predicates](#Equality-Predicates).

Function: **member** object list [¶](#index-member)
:   The function `member` tests to see whether object is a member
    of list, comparing members with object using `equal`.
    If object is a member, `member` returns a list starting with
    its first occurrence in list. Otherwise, it returns `nil`.

    Compare this with `memq`:

    ```
    (member '(2) '((1) (2)))  ; (2) and (2) are equal.
         ⇒ ((2))
    ```

    ```
    (memq '(2) '((1) (2)))    ; The two (2)s need not be eq.
         ⇒ Unspecified; might be nil or (2).
    ```

    ```
    ;; Two strings with the same contents are equal.
    (member "foo" '("foo" "bar"))
         ⇒ ("foo" "bar")
    ```

Function: **delete** object sequence [¶](#index-delete)
:   This function removes all elements `equal` to object from
    sequence, and returns the resulting sequence.

    If sequence is a list, `delete` is to `delq` as
    `member` is to `memq`: it uses `equal` to compare
    elements with object, like `member`; when it finds an
    element that matches, it cuts the element out just as `delq`
    would. As with `delq`, you should typically use the return value
    by assigning it to the variable which held the original list.

    If `sequence` is a vector or string, `delete` returns a copy
    of `sequence` with all elements `equal` to `object`
    removed.

    For example:

    ```
    (setq l (list '(2) '(1) '(2)))
    (delete '(2) l)
         ⇒ ((1))
    l
         ⇒ ((2) (1))
    ;; If you want to change l reliably,
    ;; write (setq l (delete '(2) l)).
    ```

    ```
    (setq l (list '(2) '(1) '(2)))
    (delete '(1) l)
         ⇒ ((2) (2))
    l
         ⇒ ((2) (2))
    ;; In this case, it makes no difference whether you set l,
    ;; but you should do so for the sake of the other case.
    ```

    ```
    (delete '(2) [(2) (1) (2)])
         ⇒ [(1)]
    ```

Function: **remove** object sequence [¶](#index-remove)
:   This function is the non-destructive counterpart of `delete`. It
    returns a copy of `sequence`, a list, vector, or string, with
    elements `equal` to `object` removed. For example:

    ```
    (remove '(2) '((2) (1) (2)))
         ⇒ ((1))
    ```

    ```
    (remove '(2) [(2) (1) (2)])
         ⇒ [(1)]
    ```

> **Common Lisp note:** The functions `member`, `delete` and
> `remove` in GNU Emacs Lisp are derived from Maclisp, not Common
> Lisp. The Common Lisp versions do not use `equal` to compare
> elements.

Function: **member-ignore-case** object list [¶](#index-member_002dignore_002dcase)
:   This function is like `member`, except that object should
    be a string and that it ignores differences in letter-case and text
    representation: upper-case and lower-case letters are treated as
    equal, and unibyte strings are converted to multibyte prior to
    comparison.

Function: **delete-dups** list [¶](#index-delete_002ddups)
:   This function destructively removes all `equal` duplicates from
    list, stores the result in list and returns it. Of
    several `equal` occurrences of an element in list,
    `delete-dups` keeps the first one. See `seq-uniq` for
    non-destructive operation (see [Sequences](#Sequence-Functions)).

See also the function `add-to-list`, in [Modifying List Variables](#List-Variables),
for a way to add an element to a list stored in a variable and used as a
set.

---

Next: [Property Lists](#Property-Lists), Previous: [Using Lists as Sets](#Sets-And-Lists), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.8 Association Lists [¶](#Association-Lists-1)

An *association list*, or *alist* for short, records a mapping
from keys to values. It is a list of cons cells called
*associations*: the CAR of each cons cell is the *key*, and the
CDR is the *associated value*.[7](#FOOT7)

Here is an example of an alist. The key `pine` is associated with
the value `cones`; the key `oak` is associated with
`acorns`; and the key `maple` is associated with `seeds`.

```
((pine . cones)
 (oak . acorns)
 (maple . seeds))
```

Both the values and the keys in an alist may be any Lisp objects.
For example, in the following alist, the symbol `a` is
associated with the number `1`, and the string `"b"` is
associated with the *list* `(2 3)`, which is the CDR of
the alist element:

```
((a . 1) ("b" 2 3))
```

Sometimes it is better to design an alist to store the associated
value in the CAR of the CDR of the element. Here is an
example of such an alist:

```
((rose red) (lily white) (buttercup yellow))
```

Here we regard `red` as the value associated with `rose`. One
advantage of this kind of alist is that you can store other related
information—even a list of other items—in the CDR of the
CDR. One disadvantage is that you cannot use `rassq` (see
below) to find the element containing a given value. When neither of
these considerations is important, the choice is a matter of taste, as
long as you are consistent about it for any given alist.

The same alist shown above could be regarded as having the
associated value in the CDR of the element; the value associated
with `rose` would be the list `(red)`.

Association lists are often used to record information that you might
otherwise keep on a stack, since new associations may be added easily to
the front of the list. When searching an association list for an
association with a given key, the first one found is returned, if there
is more than one.

In Emacs Lisp, it is *not* an error if an element of an
association list is not a cons cell. The alist search functions simply
ignore such elements. Many other versions of Lisp signal errors in such
cases.

Note that property lists are similar to association lists in several
respects. A property list behaves like an association list in which
each key can occur only once. See [Property Lists](#Property-Lists), for a comparison
of property lists and association lists.

Function: **assoc** key alist &optional testfn [¶](#index-assoc)
:   This function returns the first association for key in
    alist, comparing key against the alist elements using
    testfn if it is a function, and `equal` otherwise
    (see [Equality Predicates](#Equality-Predicates)). If testfn is a function, it is
    called with two arguments: the CAR of an element from alist
    and key. The function returns `nil` if no
    association in alist has a CAR equal to key, as
    tested by testfn. For example:

    ```
    (setq trees '((pine . cones) (oak . acorns) (maple . seeds)))
         ⇒ ((pine . cones) (oak . acorns) (maple . seeds))
    (assoc 'oak trees)
         ⇒ (oak . acorns)
    (cdr (assoc 'oak trees))
         ⇒ acorns
    (assoc 'birch trees)
         ⇒ nil
    ```

    Here is another example, in which the keys and values are not symbols:

    ```
    (setq needles-per-cluster
          '((2 "Austrian Pine" "Red Pine")
            (3 "Pitch Pine")
            (5 "White Pine")))

    (cdr (assoc 3 needles-per-cluster))
         ⇒ ("Pitch Pine")
    (cdr (assoc 2 needles-per-cluster))
         ⇒ ("Austrian Pine" "Red Pine")
    ```

The function `assoc-string` is much like `assoc` except
that it ignores certain differences between strings. See [Comparison of Characters and Strings](#Text-Comparison).

Function: **rassoc** value alist [¶](#index-rassoc)
:   This function returns the first association with value value in
    alist. It returns `nil` if no association in alist has
    a CDR `equal` to value.

    `rassoc` is like `assoc` except that it compares the CDR of
    each alist association instead of the CAR. You can think of
    this as reverse `assoc`, finding the key for a given value.

Function: **assq** key alist [¶](#index-assq)
:   This function is like `assoc` in that it returns the first
    association for key in alist, but it makes the comparison
    using `eq`. `assq` returns `nil` if no association in
    alist has a CAR `eq` to key. This function is
    used more often than `assoc`, since `eq` is faster than
    `equal` and most alists use symbols as keys. See [Equality Predicates](#Equality-Predicates).

    ```
    (setq trees '((pine . cones) (oak . acorns) (maple . seeds)))
         ⇒ ((pine . cones) (oak . acorns) (maple . seeds))
    (assq 'pine trees)
         ⇒ (pine . cones)
    ```

    On the other hand, `assq` is not usually useful in alists where the
    keys may not be symbols:

    ```
    (setq leaves
          '(("simple leaves" . oak)
            ("compound leaves" . horsechestnut)))

    (assq "simple leaves" leaves)
         ⇒ Unspecified; might be nil or ("simple leaves" . oak).
    (assoc "simple leaves" leaves)
         ⇒ ("simple leaves" . oak)
    ```

Function: **alist-get** key alist &optional default remove testfn [¶](#index-alist_002dget)
:   This function is similar to `assq`. It finds the first
    association `(key . value)` by comparing
    key with alist elements, and, if found, returns the
    value of that association. If no association is found, the
    function returns default. Comparison of key against
    alist elements uses the function specified by testfn,
    defaulting to `eq`.

    This is a generalized variable (see [Generalized Variables](#Generalized-Variables))
    that can be used to change a value with `setf`. When
    using it to set a value, optional argument remove non-`nil`
    means to remove key’s association from alist if the new
    value is `eql` to default.

Function: **rassq** value alist [¶](#index-rassq)
:   This function returns the first association with value value in
    alist. It returns `nil` if no association in alist has
    a CDR `eq` to value.

    `rassq` is like `assq` except that it compares the CDR of
    each alist association instead of the CAR. You can think of
    this as reverse `assq`, finding the key for a given value.

    For example:

    ```
    (setq trees '((pine . cones) (oak . acorns) (maple . seeds)))

    (rassq 'acorns trees)
         ⇒ (oak . acorns)
    (rassq 'spores trees)
         ⇒ nil
    ```

    `rassq` cannot search for a value stored in the CAR
    of the CDR of an element:

    ```
    (setq colors '((rose red) (lily white) (buttercup yellow)))

    (rassq 'white colors)
         ⇒ nil
    ```

    In this case, the CDR of the association `(lily white)` is not
    the symbol `white`, but rather the list `(white)`. This
    becomes clearer if the association is written in dotted pair notation:

    ```
    (lily white) ≡ (lily . (white))
    ```

Function: **assoc-default** key alist &optional test default [¶](#index-assoc_002ddefault)
:   This function searches alist for a match for key. For each
    element of alist, it compares the element (if it is an atom) or
    the element’s CAR (if it is a cons) against key, by calling
    test with two arguments: the element or its CAR, and
    key. The arguments are passed in that order so that you can get
    useful results using `string-match` with an alist that contains
    regular expressions (see [Regular Expression Searching](#Regexp-Search)). If test is omitted
    or `nil`, `equal` is used for comparison.

    If an alist element matches key by this criterion,
    then `assoc-default` returns a value based on this element.
    If the element is a cons, then the value is the element’s CDR.
    Otherwise, the return value is default.

    If no alist element matches key, `assoc-default` returns
    `nil`.

Function: **copy-alist** alist [¶](#index-copy_002dalist)
:   This function returns a two-level deep copy of alist: it creates a
    new copy of each association, so that you can alter the associations of
    the new alist without changing the old one.

    ```
    (setq needles-per-cluster
          '((2 . ("Austrian Pine" "Red Pine"))
            (3 . ("Pitch Pine"))
    ```

    ```
            (5 . ("White Pine"))))
    ⇒
    ((2 "Austrian Pine" "Red Pine")
     (3 "Pitch Pine")
     (5 "White Pine"))

    (setq copy (copy-alist needles-per-cluster))
    ⇒
    ((2 "Austrian Pine" "Red Pine")
     (3 "Pitch Pine")
     (5 "White Pine"))

    (eq needles-per-cluster copy)
         ⇒ nil
    (equal needles-per-cluster copy)
         ⇒ t
    (eq (car needles-per-cluster) (car copy))
         ⇒ nil
    (cdr (car (cdr needles-per-cluster)))
         ⇒ ("Pitch Pine")
    ```

    ```
    (eq (cdr (car (cdr needles-per-cluster)))
        (cdr (car (cdr copy))))
         ⇒ t
    ```

    This example shows how `copy-alist` makes it possible to change
    the associations of one copy without affecting the other:

    ```
    (setcdr (assq 3 copy) '("Martian Vacuum Pine"))
    (cdr (assq 3 needles-per-cluster))
         ⇒ ("Pitch Pine")
    ```

Function: **assq-delete-all** key alist [¶](#index-assq_002ddelete_002dall)
:   This function deletes from alist all the elements whose CAR
    is `eq` to key, much as if you used `delq` to delete
    each such element one by one. It returns the shortened alist, and
    often modifies the original list structure of alist. For
    correct results, use the return value of `assq-delete-all` rather
    than looking at the saved value of alist.

    ```
    (setq alist (list '(foo 1) '(bar 2) '(foo 3) '(lose 4)))
         ⇒ ((foo 1) (bar 2) (foo 3) (lose 4))
    (assq-delete-all 'foo alist)
         ⇒ ((bar 2) (lose 4))
    alist
         ⇒ ((foo 1) (bar 2) (lose 4))
    ```

Function: **assoc-delete-all** key alist &optional test [¶](#index-assoc_002ddelete_002dall)
:   This function is like `assq-delete-all` except that it accepts
    an optional argument test, a predicate function to compare the
    keys in alist. If omitted or `nil`, test defaults to
    `equal`. As `assq-delete-all`, this function often modifies
    the original list structure of alist.

Function: **rassq-delete-all** value alist [¶](#index-rassq_002ddelete_002dall)
:   This function deletes from alist all the elements whose CDR
    is `eq` to value. It returns the shortened alist, and
    often modifies the original list structure of alist.
    `rassq-delete-all` is like `assq-delete-all` except that it
    compares the CDR of each alist association instead of the
    CAR.

Macro: **let-alist** alist body [¶](#index-let_002dalist)
:   Creates a binding for each symbol used as keys the association list
    alist, prefixed with dot. This can be useful when accessing
    several items in the same association list, and it’s best understood
    through a simple example:

    ```
    (setq colors '((rose . red) (lily . white) (buttercup . yellow)))
    (let-alist colors
      (if (eq .rose 'red)
          .lily))
         ⇒ white
    ```

    The body is inspected during macro-expansion, and the symbols that
    appear literally in body with a ‘`.`’ as the first character in
    the symbol name will be bound. Finding the keys is done with
    `assq`, and the `cdr` of the return value of this `assq`
    is assigned as the value for the binding. The generated code performs
    all alist lookups before any other part of body, so a symbol
    `.foo` that appears anywhere in body will be looked up (using key
    `foo`) and bound whether or not it is actually used in body
    at runtime.

    Nested association lists is supported:

    ```
    (setq colors '((rose . red) (lily (belladonna . yellow) (brindisi . pink))))
    (let-alist colors
      (if (eq .rose 'red)
          .lily.belladonna))
         ⇒ yellow
    ```

    Nesting `let-alist` inside each other is allowed, but the code in
    the inner `let-alist` can’t access the variables bound by the
    outer `let-alist`.

    Indexing into lists is also supported:

    ```
    (setq colors '((rose . red) (lily . (yellow pink))))
    (let-alist colors .lily.1)
         ⇒ pink
    ```

    Note that forms like ‘`.0`’ or ‘`.3`’ are interpreted as numbers
    rather than as symbols, so they won’t be bound to the corresponding
    values in alist.

---

Previous: [Association Lists](#Association-Lists), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.9 Property Lists [¶](#Property-Lists-1)

A *property list* (*plist* for short) is a list of paired
elements. Each of the pairs associates a property name (usually a
symbol) with a property or value. Here is an example of a property
list:

```
(pine cones numbers (1 2 3) color "blue")
```

This property list associates `pine` with `cones`,
`numbers` with `(1 2 3)`, and `color` with
`"blue"`. The property names and values can be any Lisp objects,
but the names are usually symbols (as they are in this example).

Property lists are used in several contexts. For instance, the
function `put-text-property` takes an argument which is a
property list, specifying text properties and associated values which
are to be applied to text in a string or buffer. See [Text Properties](#Text-Properties).

Another prominent use of property lists is for storing symbol
properties. Every symbol possesses a list of properties, used to
record miscellaneous information about the symbol; these properties
are stored in the form of a property list. See [Symbol Properties](#Symbol-Properties).

Function: **plistp** object [¶](#index-plistp)
:   This predicate function returns non-`nil` if object is a
    valid property list.

* [Property Lists and Association Lists](#Plists-and-Alists)
* [Property Lists Outside Symbols](#Plist-Access)

---

Next: [Property Lists Outside Symbols](#Plist-Access), Up: [Property Lists](#Property-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.9.1 Property Lists and Association Lists [¶](#Property-Lists-and-Association-Lists)

Association lists (see [Association Lists](#Association-Lists)) are very similar to
property lists. In contrast to association lists, the order of the
pairs in the property list is not significant, since the property
names must be distinct.

Property lists are better than association lists for attaching
information to various Lisp function names or variables. If your
program keeps all such information in one association list, it will
typically need to search that entire list each time it checks for an
association for a particular Lisp function name or variable, which
could be slow. By contrast, if you keep the same information in the
property lists of the function names or variables themselves, each
search will scan only the length of one property list, which is
usually short. This is why the documentation for a variable is
recorded in a property named `variable-documentation`. The byte
compiler likewise uses properties to record those functions needing
special treatment.

However, association lists have their own advantages. Depending on
your application, it may be faster to add an association to the front of
an association list than to update a property. All properties for a
symbol are stored in the same property list, so there is a possibility
of a conflict between different uses of a property name. (For this
reason, it is a good idea to choose property names that are probably
unique, such as by beginning the property name with the program’s usual
name-prefix for variables and functions.) An association list may be
used like a stack where associations are pushed on the front of the list
and later discarded; this is not possible with a property list.

---

Previous: [Property Lists and Association Lists](#Plists-and-Alists), Up: [Property Lists](#Property-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 5.9.2 Property Lists Outside Symbols [¶](#Property-Lists-Outside-Symbols)

The following functions can be used to manipulate property lists.
They all default to comparing property names using `eq`.

Function: **plist-get** plist property &optional predicate [¶](#index-plist_002dget)
:   This returns the value of the property property stored in the
    property list plist. Comparisons are done with predicate,
    which defaults to `eq`. It accepts a malformed plist
    argument. If property is not found in the plist, it
    returns `nil`. For example,

    ```
    (plist-get '(foo 4) 'foo)
         ⇒ 4
    (plist-get '(foo 4 bad) 'foo)
         ⇒ 4
    (plist-get '(foo 4 bad) 'bad)
         ⇒ nil
    (plist-get '(foo 4 bad) 'bar)
         ⇒ nil
    ```

Function: **plist-put** plist property value &optional predicate [¶](#index-plist_002dput)
:   This stores value as the value of the property property in
    the property list plist. Comparisons are done with predicate,
    which defaults to `eq`. It may modify plist destructively,
    or it may construct a new list structure without altering the old. The
    function returns the modified property list, so you can store that back
    in the place where you got plist. For example,

    ```
    (setq my-plist (list 'bar t 'foo 4))
         ⇒ (bar t foo 4)
    (setq my-plist (plist-put my-plist 'foo 69))
         ⇒ (bar t foo 69)
    (setq my-plist (plist-put my-plist 'quux '(a)))
         ⇒ (bar t foo 69 quux (a))
    ```

Function: **lax-plist-get** plist property [¶](#index-lax_002dplist_002dget)
:   This obsolete function is like `plist-get` except that it
    compares properties using `equal` instead of `eq`.

Function: **lax-plist-put** plist property value [¶](#index-lax_002dplist_002dput)
:   This obsolete function is like `plist-put` except that it
    compares properties using `equal` instead of `eq`.

Function: **plist-member** plist property &optional predicate [¶](#index-plist_002dmember)
:   This returns non-`nil` if plist contains the given
    property. Comparisons are done with predicate, which
    defaults to `eq`. Unlike `plist-get`, this allows you to
    distinguish between a missing property and a property with the value
    `nil`. The value is actually the tail of plist whose
    `car` is property.

---

Next: [Records](#Records), Previous: [Lists](#Lists), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 6 Sequences, Arrays, and Vectors [¶](#Sequences_002c-Arrays_002c-and-Vectors)

The *sequence* type is the union of two other Lisp types: lists
and arrays. In other words, any list is a sequence, and any array is
a sequence. The common property that all sequences have is that each
is an ordered collection of elements.

An *array* is a fixed-length object with a slot for each of its
elements. All the elements are accessible in constant time. The four
types of arrays are strings, vectors, char-tables and bool-vectors.

A list is a sequence of elements, but it is not a single primitive
object; it is made of cons cells, one cell per element. Finding the
nth element requires looking through n cons cells, so
elements farther from the beginning of the list take longer to access.
But it is possible to add elements to the list, or remove elements.

The following diagram shows the relationship between these types:

```
          _____________________________________________
         |                                             |
         |          Sequence                           |
         |  ______   ________________________________  |
         | |      | |                                | |
         | | List | |             Array              | |
         | |      | |    ________       ________     | |
         | |______| |   |        |     |        |    | |
         |          |   | Vector |     | String |    | |
         |          |   |________|     |________|    | |
         |          |  ____________   _____________  | |
         |          | |            | |             | | |
         |          | | Char-table | | Bool-vector | | |
         |          | |____________| |_____________| | |
         |          |________________________________| |
         |_____________________________________________|
```

* [Sequences](#Sequence-Functions)
* [Arrays](#Arrays)
* [Functions that Operate on Arrays](#Array-Functions)
* [Vectors](#Vectors)
* [Functions for Vectors](#Vector-Functions)
* [Char-Tables](#Char_002dTables)
* [Bool-vectors](#Bool_002dVectors)
* [Managing a Fixed-Size Ring of Objects](#Rings)

---

Next: [Arrays](#Arrays), Up: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
