Function: **quotient** x y [¶](#index-quotient)
:   Divide integer x by integer y; return an integer quotient
    and discard the remainder. If x or y is negative, the
    direction of rounding is undefined.

Function: **idiv** x y [¶](#index-idiv)
:   Perform an integer division; if x and y are both nonnegative
    integers, this uses the `quotient` function, otherwise it computes
    ‘`floor(x/y)`’. Thus the result is well-defined but
    slower than for `quotient`.

Function: **imod** x y [¶](#index-imod)
:   Divide integer x by integer y; return the integer remainder
    and discard the quotient. Like `quotient`, this works only for
    integer arguments and is not well-defined for negative arguments.
    For a more well-defined result, use ‘`(% x y)`’.

Function: **idivmod** x y [¶](#index-idivmod)
:   Divide integer x by integer y; return a cons cell whose
    `car` is ‘`(quotient x y)`’ and whose `cdr`
    is ‘`(imod x y)`’.

Function: **pow** x y [¶](#index-pow)
:   Compute x to the power y. In `defmath` code, this can
    also be written ‘`(^ x y)`’ or
    ‘`(expt x y)`’.

Function: **abs-approx** x [¶](#index-abs_002dapprox)
:   Compute a fast approximation to the absolute value of x. For
    example, for a rectangular complex number the result is the sum of
    the absolute values of the components.

Function: **pi** [¶](#index-pi)
:   The function ‘`(pi)`’ computes ‘`pi`’ to the current precision.
    Other related constant-generating functions are `two-pi`,
    `pi-over-2`, `pi-over-4`, `pi-over-180`, `sqrt-two-pi`,
    `e`, `sqrt-e`, `ln-2`, `ln-10`, `phi` and
    `gamma-const`. Each function returns a floating-point value in the
    current precision, and each uses caching so that all calls after the
    first are essentially free.

Macro: **math-defcache** func initial form [¶](#index-math_002ddefcache)
:   This macro, usually used as a top-level call like `defun` or
    `defvar`, defines a new cached constant analogous to `pi`, etc.
    It defines a function `func` which returns the requested value;
    if initial is non-`nil` it must be a ‘`(float …)`’
    form which serves as an initial value for the cache. If func
    is called when the cache is empty or does not have enough digits to
    satisfy the current precision, the Lisp expression form is evaluated
    with the current precision increased by four, and the result minus its
    two least significant digits is stored in the cache. For example,
    calling ‘`(pi)`’ with a precision of 30 computes ‘`pi`’ to 34
    digits, rounds it down to 32 digits for future use, then rounds it
    again to 30 digits for use in the present request.

Function: **full-circle** symb [¶](#index-full_002dcircle)
:   If the current angular mode is Degrees or HMS, this function returns the
    integer 360. In Radians mode, this function returns either the
    corresponding value in radians to the current precision, or the formula
    ‘`2*pi`’, depending on the Symbolic mode. There are also similar
    function `half-circle` and `quarter-circle`.

Function: **power-of-2** n [¶](#index-power_002dof_002d2)
:   Compute two to the integer power n, as a (potentially very large)
    integer. Powers of two are cached, so only the first call for a
    particular n is expensive.

Function: **integer-log2** n [¶](#index-integer_002dlog2)
:   Compute the base-2 logarithm of n, which must be an integer which
    is a power of two. If n is not a power of two, this function will
    return `nil`.

Function: **div-mod** a b m [¶](#index-div_002dmod)
:   Divide a by b, modulo m. This returns `nil` if
    there is no solution, or if any of the arguments are not integers.

Function: **pow-mod** a b m [¶](#index-pow_002dmod)
:   Compute a to the power b, modulo m. If a,
    b, and m are integers, this uses an especially efficient
    algorithm. Otherwise, it simply computes ‘`(% (^ a b) m)`’.

Function: **isqrt** n [¶](#index-isqrt)
:   Compute the integer square root of n. This is the square root
    of n rounded down toward zero, i.e., ‘`floor(sqrt(n))`’.
    If n is itself an integer, the computation is especially efficient.

Function: **to-hms** a ang [¶](#index-to_002dhms)
:   Convert the argument a into an HMS form. If ang is specified,
    it is the angular mode in which to interpret a, either `deg`
    or `rad`. Otherwise, the current angular mode is used. If a
    is already an HMS form it is returned as-is.

Function: **from-hms** a ang [¶](#index-from_002dhms)
:   Convert the HMS form a into a real number. If ang is specified,
    it is the angular mode in which to express the result, otherwise the
    current angular mode is used. If a is already a real number, it
    is returned as-is.

Function: **to-radians** a [¶](#index-to_002dradians)
:   Convert the number or HMS form a to radians from the current
    angular mode.

Function: **from-radians** a [¶](#index-from_002dradians)
:   Convert the number a from radians to the current angular mode.
    If a is a formula, this returns the formula ‘`deg(a)`’.

Function: **to-radians-2** a [¶](#index-to_002dradians_002d2)
:   Like `to-radians`, except that in Symbolic mode a degrees to
    radians conversion yields a formula like ‘`a*pi/180`’.

Function: **from-radians-2** a [¶](#index-from_002dradians_002d2)
:   Like `from-radians`, except that in Symbolic mode a radians to
    degrees conversion yields a formula like ‘`a*180/pi`’.

Function: **random-digit** [¶](#index-random_002ddigit)
:   Produce a random base-1000 digit in the range 0 to 999.

Function: **random-digits** n [¶](#index-random_002ddigits)
:   Produce a random n-digit integer; this will be an integer
    in the interval ‘`[0, 10^n)`’.

Function: **random-float** [¶](#index-random_002dfloat)
:   Produce a random float in the interval ‘`[0, 1)`’.

Function: **prime-test** n iters [¶](#index-prime_002dtest)
:   Determine whether the integer n is prime. Return a list which has
    one of these forms: ‘`(nil f)`’ means the number is non-prime
    because it was found to be divisible by f; ‘`(nil)`’ means it
    was found to be non-prime by table look-up (so no factors are known);
    ‘`(nil unknown)`’ means it is definitely non-prime but no factors
    are known because n was large enough that Fermat’s probabilistic
    test had to be used; ‘`(t)`’ means the number is definitely prime;
    and ‘`(maybe i p)`’ means that Fermat’s test, after i
    iterations, is p percent sure that the number is prime. The
    iters parameter is the number of Fermat iterations to use, in the
    case that this is necessary. If `prime-test` returns “maybe,”
    you can call it again with the same n to get a greater certainty;
    `prime-test` remembers where it left off.

Function: **to-simple-fraction** f [¶](#index-to_002dsimple_002dfraction)
:   If f is a floating-point number which can be represented exactly
    as a small rational number, return that number, else return f.
    For example, 0.75 would be converted to 3:4. This function is very
    fast.

Function: **to-fraction** f tol [¶](#index-to_002dfraction)
:   Find a rational approximation to floating-point number f to within
    a specified tolerance tol; this corresponds to the algebraic
    function `frac`, and can be rather slow.

Function: **quarter-integer** n [¶](#index-quarter_002dinteger)
:   If n is an integer or integer-valued float, this function
    returns zero. If n is a half-integer (i.e., an integer plus
    *1:2* or 0.5), it returns 2. If n is a quarter-integer,
    it returns 1 or 3. If n is anything else, this function
    returns `nil`.

---

Next: [Symbolic Functions](#Symbolic-Lisp-Functions), Previous: [Computational Functions](#Computational-Lisp-Functions), Up: [Calculator Internals](#Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7.6 Vector Functions [¶](#Vector-Functions)

The functions described here perform various operations on vectors and
matrices.

Function: **math-concat** x y [¶](#index-math_002dconcat)
:   Do a vector concatenation; this operation is written ‘`x | y`’
    in a symbolic formula. See [Building Vectors](#Building-Vectors).

Function: **vec-length** v [¶](#index-vec_002dlength)
:   Return the length of vector v. If v is not a vector, the
    result is zero. If v is a matrix, this returns the number of
    rows in the matrix.

Function: **mat-dimens** m [¶](#index-mat_002ddimens)
:   Determine the dimensions of vector or matrix m. If m is not
    a vector, the result is an empty list. If m is a plain vector
    but not a matrix, the result is a one-element list containing the length
    of the vector. If m is a matrix with r rows and c columns,
    the result is the list ‘`(r c)`’. Higher-order tensors
    produce lists of more than two dimensions. Note that the object
    ‘`[[1, 2, 3], [4, 5]]`’ is a vector of vectors not all the same size,
    and is treated by this and other Calc routines as a plain vector of two
    elements.

Function: **dimension-error** [¶](#index-dimension_002derror)
:   Abort the current function with a message of “Dimension error.”
    The Calculator will leave the function being evaluated in symbolic
    form; this is really just a special case of `reject-arg`.

Function: **build-vector** args [¶](#index-build_002dvector)
:   Return a Calc vector with args as elements.
    For example, ‘`(build-vector 1 2 3)`’ returns the Calc vector
    ‘`[1, 2, 3]`’, stored internally as the list ‘`(vec 1 2 3)`’.

Function: **make-vec** obj dims [¶](#index-make_002dvec)
:   Return a Calc vector or matrix all of whose elements are equal to
    obj. For example, ‘`(make-vec 27 3 4)`’ returns a 3x4 matrix
    filled with 27’s.

Function: **row-matrix** v [¶](#index-row_002dmatrix)
:   If v is a plain vector, convert it into a row matrix, i.e.,
    a matrix whose single row is v. If v is already a matrix,
    leave it alone.

Function: **col-matrix** v [¶](#index-col_002dmatrix)
:   If v is a plain vector, convert it into a column matrix, i.e., a
    matrix with each element of v as a separate row. If v is
    already a matrix, leave it alone.

Function: **map-vec** f v [¶](#index-map_002dvec)
:   Map the Lisp function f over the Calc vector v. For example,
    ‘`(map-vec 'math-floor v)`’ returns a vector of the floored components
    of vector v.

Function: **map-vec-2** f a b [¶](#index-map_002dvec_002d2)
:   Map the Lisp function f over the two vectors a and b.
    If a and b are vectors of equal length, the result is a
    vector of the results of calling ‘`(f ai bi)`’
    for each pair of elements ai and bi. If either a or
    b is a scalar, it is matched with each value of the other vector.
    For example, ‘`(map-vec-2 'math-add v 1)`’ returns the vector v
    with each element increased by one. Note that using ‘`'+`’ would not
    work here, since `defmath` does not expand function names everywhere,
    just where they are in the function position of a Lisp expression.

Function: **reduce-vec** f v [¶](#index-reduce_002dvec)
:   Reduce the function f over the vector v. For example, if
    v is ‘`[10, 20, 30, 40]`’, this calls ‘`(f (f (f 10 20) 30) 40)`’.
    If v is a matrix, this reduces over the rows of v.

Function: **reduce-cols** f m [¶](#index-reduce_002dcols)
:   Reduce the function f over the columns of matrix m. For
    example, if m is ‘`[[1, 2], [3, 4], [5, 6]]`’, the result
    is a vector of the two elements ‘`(f (f 1 3) 5)`’ and ‘`(f (f 2 4) 6)`’.

Function: **mat-row** m n [¶](#index-mat_002drow)
:   Return the nth row of matrix m. This is equivalent to
    ‘`(elt m n)`’. For a slower but safer version, use `mrow`.
    (See [Extracting Vector Elements](#Extracting-Elements).)

Function: **mat-col** m n [¶](#index-mat_002dcol)
:   Return the nth column of matrix m, in the form of a vector.
    The arguments are not checked for correctness.

Function: **mat-less-row** m n [¶](#index-mat_002dless_002drow)
:   Return a copy of matrix m with its nth row deleted. The
    number n must be in range from 1 to the number of rows in m.

Function: **mat-less-col** m n [¶](#index-mat_002dless_002dcol)
:   Return a copy of matrix m with its nth column deleted.

Function: **transpose** m [¶](#index-transpose)
:   Return the transpose of matrix m.

Function: **flatten-vector** v [¶](#index-flatten_002dvector)
:   Flatten nested vector v into a vector of scalars. For example,
    if v is ‘`[[1, 2, 3], [4, 5]]`’ the result is ‘`[1, 2, 3, 4, 5]`’.

Function: **copy-matrix** m [¶](#index-copy_002dmatrix)
:   If m is a matrix, return a copy of m. This maps
    `copy-sequence` over the rows of m; in Lisp terms, each
    element of the result matrix will be `eq` to the corresponding
    element of m, but none of the `cons` cells that make up
    the structure of the matrix will be `eq`. If m is a plain
    vector, this is the same as `copy-sequence`.

Function: **swap-rows** m r1 r2 [¶](#index-swap_002drows)
:   Exchange rows r1 and r2 of matrix m in-place. In
    other words, unlike most of the other functions described here, this
    function changes m itself rather than building up a new result
    matrix. The return value is m, i.e., ‘`(eq (swap-rows m 1 2) m)`’
    is true, with the side effect of exchanging the first two rows of
    m.

---

Next: [I/O and Formatting Functions](#Formatting-Lisp-Functions), Previous: [Vector Functions](#Vector-Lisp-Functions), Up: [Calculator Internals](#Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7.7 Symbolic Functions [¶](#Symbolic-Functions)

The functions described here operate on symbolic formulas in the
Calculator.

Function: **calc-prepare-selection** num [¶](#index-calc_002dprepare_002dselection)
:   Prepare a stack entry for selection operations. If num is
    omitted, the stack entry containing the cursor is used; otherwise,
    it is the number of the stack entry to use. This function stores
    useful information about the current stack entry into a set of
    variables. `calc-selection-cache-num` contains the number of
    the stack entry involved (equal to num if you specified it);
    `calc-selection-cache-entry` contains the stack entry as a
    list (such as `calc-top-list` would return with `entry`
    as the selection mode); and `calc-selection-cache-comp` contains
    a special “tagged” composition (see [I/O and Formatting Functions](#Formatting-Lisp-Functions))
    which allows Calc to relate cursor positions in the buffer with
    their corresponding sub-formulas.

    A slight complication arises in the selection mechanism because
    formulas may contain small integers. For example, in the vector
    ‘`[1, 2, 1]`’ the first and last elements are `eq` to each
    other; selections are recorded as the actual Lisp object that
    appears somewhere in the tree of the whole formula, but storing
    `1` would falsely select both `1`’s in the vector. So
    `calc-prepare-selection` also checks the stack entry and
    replaces any plain integers with “complex number” lists of the form
    ‘`(cplx n 0)`’. This list will be displayed the same as a
    plain n and the change will be completely invisible to the
    user, but it will guarantee that no two sub-formulas of the stack
    entry will be `eq` to each other. Next time the stack entry
    is involved in a computation, `calc-normalize` will replace
    these lists with plain numbers again, again invisibly to the user.

Function: **calc-encase-atoms** x [¶](#index-calc_002dencase_002datoms)
:   This modifies the formula x to ensure that each part of the
    formula is a unique atom, using the ‘`(cplx n 0)`’ trick
    described above. This function may use `setcar` to modify
    the formula in-place.

Function: **calc-find-selected-part** [¶](#index-calc_002dfind_002dselected_002dpart)
:   Find the smallest sub-formula of the current formula that contains
    the cursor. This assumes `calc-prepare-selection` has been
    called already. If the cursor is not actually on any part of the
    formula, this returns `nil`.

Function: **calc-change-current-selection** selection [¶](#index-calc_002dchange_002dcurrent_002dselection)
:   Change the currently prepared stack element’s selection to
    selection, which should be `eq` to some sub-formula
    of the stack element, or `nil` to unselect the formula.
    The stack element’s appearance in the Calc buffer is adjusted
    to reflect the new selection.

Function: **calc-find-nth-part** expr n [¶](#index-calc_002dfind_002dnth_002dpart)
:   Return the nth sub-formula of expr. This function is used
    by the selection commands, and (unless `j b` has been used) treats
    sums and products as flat many-element formulas. Thus if expr
    is ‘`((a + b) - c) + d`’, calling `calc-find-nth-part` with
    n equal to four will return ‘`d`’.

Function: **calc-find-parent-formula** expr part [¶](#index-calc_002dfind_002dparent_002dformula)
:   Return the sub-formula of expr which immediately contains
    part. If expr is ‘`a*b + (c+1)*d`’ and part
    is `eq` to the ‘`c+1`’ term of expr, then this function
    will return ‘`(c+1)*d`’. If part turns out not to be a
    sub-formula of expr, the function returns `nil`. If
    part is `eq` to expr, the function returns `t`.
    This function does not take associativity into account.

Function: **calc-find-assoc-parent-formula** expr part [¶](#index-calc_002dfind_002dassoc_002dparent_002dformula)
:   This is the same as `calc-find-parent-formula`, except that
    (unless `j b` has been used) it continues widening the selection
    to contain a complete level of the formula. Given ‘`a`’ from
    ‘`((a + b) - c) + d`’, `calc-find-parent-formula` will
    return ‘`a + b`’ but `calc-find-assoc-parent-formula` will
    return the whole expression.

Function: **calc-grow-assoc-formula** expr part [¶](#index-calc_002dgrow_002dassoc_002dformula)
:   This expands sub-formula part of expr to encompass a
    complete level of the formula. If part and its immediate
    parent are not compatible associative operators, or if `j b`
    has been used, this simply returns part.

Function: **calc-find-sub-formula** expr part [¶](#index-calc_002dfind_002dsub_002dformula)
:   This finds the immediate sub-formula of expr which contains
    part. It returns an index n such that
    ‘`(calc-find-nth-part expr n)`’ would return part.
    If part is not a sub-formula of expr, it returns `nil`.
    If part is `eq` to expr, it returns `t`. This
    function does not take associativity into account.

Function: **calc-replace-sub-formula** expr old new [¶](#index-calc_002dreplace_002dsub_002dformula)
:   This function returns a copy of formula expr, with the
    sub-formula that is `eq` to old replaced by new.

Function: **simplify** expr [¶](#index-simplify)
:   Simplify the expression expr by applying Calc’s algebraic
    simplifications. This always returns a copy of the expression; the
    structure expr points to remains unchanged in memory.

    More precisely, here is what `simplify` does: The expression is
    first normalized and evaluated by calling `normalize`. If any
    `AlgSimpRules` have been defined, they are then applied. Then
    the expression is traversed in a depth-first, bottom-up fashion; at
    each level, any simplifications that can be made are made until no
    further changes are possible. Once the entire formula has been
    traversed in this way, it is compared with the original formula (from
    before the call to `normalize`) and, if it has changed,
    the entire procedure is repeated (starting with `normalize`)
    until no further changes occur. Usually only two iterations are
    needed: one to simplify the formula, and another to verify that no
    further simplifications were possible.

Function: **simplify-extended** expr [¶](#index-simplify_002dextended)
:   Simplify the expression expr, with additional rules enabled that
    help do a more thorough job, while not being entirely “safe” in all
    circumstances. (For example, this mode will simplify ‘`sqrt(x^2)`’
    to ‘`x`’, which is only valid when x is positive.) This is
    implemented by temporarily binding the variable `math-living-dangerously`
    to `t` (using a `let` form) and calling `simplify`.
    Dangerous simplification rules are written to check this variable
    before taking any action.

Function: **simplify-units** expr [¶](#index-simplify_002dunits)
:   Simplify the expression expr, treating variable names as units
    whenever possible. This works by binding the variable
    `math-simplifying-units` to `t` while calling `simplify`.

Macro: **math-defsimplify** funcs body [¶](#index-math_002ddefsimplify)
:   Register a new simplification rule; this is normally called as a top-level
    form, like `defun` or `defmath`. If funcs is a symbol
    (like `+` or `calcFunc-sqrt`), this simplification rule is
    applied to the formulas which are calls to the specified function. Or,
    funcs can be a list of such symbols; the rule applies to all
    functions on the list. The body is written like the body of a
    function with a single argument called `expr`. The body will be
    executed with `expr` bound to a formula which is a call to one of
    the functions funcs. If the function body returns `nil`, or
    if it returns a result `equal` to the original `expr`, it is
    ignored and Calc goes on to try the next simplification rule that applies.
    If the function body returns something different, that new formula is
    substituted for expr in the original formula.

    At each point in the formula, rules are tried in the order of the
    original calls to `math-defsimplify`; the search stops after the
    first rule that makes a change. Thus later rules for that same
    function will not have a chance to trigger until the next iteration
    of the main `simplify` loop.

    Note that, since `defmath` is not being used here, body must
    be written in true Lisp code without the conveniences that `defmath`
    provides. If you prefer, you can have body simply call another
    function (defined with `defmath`) which does the real work.

    The arguments of a function call will already have been simplified
    before any rules for the call itself are invoked. Since a new argument
    list is consed up when this happens, this means that the rule’s body is
    allowed to rearrange the function’s arguments destructively if that is
    convenient. Here is a typical example of a simplification rule:

    ```
    (math-defsimplify calcFunc-arcsinh
      (or (and (math-looks-negp (nth 1 expr))
               (math-neg (list 'calcFunc-arcsinh
                               (math-neg (nth 1 expr)))))
          (and (eq (car-safe (nth 1 expr)) 'calcFunc-sinh)
               (or math-living-dangerously
                   (math-known-realp (nth 1 (nth 1 expr))))
               (nth 1 (nth 1 expr)))))
    ```

    This is really a pair of rules written with one `math-defsimplify`
    for convenience; the first replaces ‘`arcsinh(-x)`’ with
    ‘`-arcsinh(x)`’, and the second, which is safe only for real ‘`x`’,
    replaces ‘`arcsinh(sinh(x))`’ with ‘`x`’.

Function: **common-constant-factor** expr [¶](#index-common_002dconstant_002dfactor)
:   Check expr to see if it is a sum of terms all multiplied by the
    same rational value. If so, return this value. If not, return `nil`.
    For example, if called on ‘`6x + 9y + 12z`’, it would return 3, since
    3 is a common factor of all the terms.

Function: **cancel-common-factor** expr factor [¶](#index-cancel_002dcommon_002dfactor)
:   Assuming expr is a sum with factor as a common factor,
    divide each term of the sum by factor. This is done by
    destructively modifying parts of expr, on the assumption that
    it is being used by a simplification rule (where such things are
    allowed; see above). For example, consider this built-in rule for
    square roots:

    ```
    (math-defsimplify calcFunc-sqrt
      (let ((fac (math-common-constant-factor (nth 1 expr))))
        (and fac (not (eq fac 1))
             (math-mul (math-normalize (list 'calcFunc-sqrt fac))
                       (math-normalize
                        (list 'calcFunc-sqrt
                              (math-cancel-common-factor
                               (nth 1 expr) fac)))))))
    ```

Function: **frac-gcd** a b [¶](#index-frac_002dgcd)
:   Compute a “rational GCD” of a and b, which must both be
    rational numbers. This is the fraction composed of the GCD of the
    numerators of a and b, over the GCD of the denominators.
    It is used by `common-constant-factor`. Note that the standard
    `gcd` function uses the LCM to combine the denominators.

Function: **map-tree** func expr many [¶](#index-map_002dtree)
:   Try applying Lisp function func to various sub-expressions of
    expr. Initially, call func with expr itself as an
    argument. If this returns an expression which is not `equal` to
    expr, apply func again until eventually it does return
    expr with no changes. Then, if expr is a function call,
    recursively apply func to each of the arguments. This keeps going
    until no changes occur anywhere in the expression; this final expression
    is returned by `map-tree`. Note that, unlike simplification rules,
    func functions may *not* make destructive changes to
    expr. If a third argument many is provided, it is an
    integer which says how many times func may be applied; the
    default, as described above, is infinitely many times.

Function: **compile-rewrites** rules [¶](#index-compile_002drewrites)
:   Compile the rewrite rule set specified by rules, which should
    be a formula that is either a vector or a variable name. If the latter,
    the compiled rules are saved so that later `compile-rules` calls
    for that same variable can return immediately. If there are problems
    with the rules, this function calls `error` with a suitable
    message.

Function: **apply-rewrites** expr crules heads [¶](#index-apply_002drewrites)
:   Apply the compiled rewrite rule set crules to the expression
    expr. This will make only one rewrite and only checks at the
    top level of the expression. The result `nil` if no rules
    matched, or if the only rules that matched did not actually change
    the expression. The heads argument is optional; if is given,
    it should be a list of all function names that (may) appear in
    expr. The rewrite compiler tags each rule with the
    rarest-looking function name in the rule; if you specify heads,
    `apply-rewrites` can use this information to narrow its search
    down to just a few rules in the rule set.

Function: **rewrite-heads** expr [¶](#index-rewrite_002dheads)
:   Compute a heads list for expr suitable for use with
    `apply-rewrites`, as discussed above.

Function: **rewrite** expr rules many [¶](#index-rewrite)
:   This is an all-in-one rewrite function. It compiles the rule set
    specified by rules, then uses `map-tree` to apply the
    rules throughout expr up to many (default infinity)
    times.

Function: **match-patterns** pat vec not-flag [¶](#index-match_002dpatterns)
:   Given a Calc vector vec and an uncompiled pattern set or
    pattern set variable pat, this function returns a new vector
    of all elements of vec which do (or don’t, if not-flag is
    non-`nil`) match any of the patterns in pat.

Function: **deriv** expr var value symb [¶](#index-deriv)
:   Compute the derivative of expr with respect to variable var
    (which may actually be any sub-expression). If value is specified,
    the derivative is evaluated at the value of var; otherwise, the
    derivative is left in terms of var. If the expression contains
    functions for which no derivative formula is known, new derivative
    functions are invented by adding primes to the names; see [Calculus](#Calculus).
    However, if symb is non-`nil`, the presence of nondifferentiable
    functions in expr instead cancels the whole differentiation, and
    `deriv` returns `nil` instead.

    Derivatives of an n-argument function can be defined by
    adding a `math-derivative-n` property to the property list
    of the symbol for the function’s derivative, which will be the
    function name followed by an apostrophe. The value of the property
    should be a Lisp function; it is called with the same arguments as the
    original function call that is being differentiated. It should return
    a formula for the derivative. For example, the derivative of `ln`
    is defined by

    ```
    (put 'calcFunc-ln\' 'math-derivative-1
         (lambda (u) (math-div 1 u)))
    ```

    The two-argument `log` function has two derivatives,

    ```
    (put 'calcFunc-log\' 'math-derivative-2     ; d(log(x,b)) / dx
         (lambda (x b) ... ))
    (put 'calcFunc-log\'2 'math-derivative-2    ; d(log(x,b)) / db
         (lambda (x b) ... ))
    ```

Function: **tderiv** expr var value symb [¶](#index-tderiv)
:   Compute the total derivative of expr. This is the same as
    `deriv`, except that variables other than var are not
    assumed to be constant with respect to var.

Function: **integ** expr var low high [¶](#index-integ)
:   Compute the integral of expr with respect to var.
    See [Calculus](#Calculus), for further details.

Macro: **math-defintegral** funcs body [¶](#index-math_002ddefintegral)
:   Define a rule for integrating a function or functions of one argument;
    this macro is very similar in format to `math-defsimplify`.
    The main difference is that here body is the body of a function
    with a single argument `u` which is bound to the argument to the
    function being integrated, not the function call itself. Also, the
    variable of integration is available as `math-integ-var`. If
    evaluation of the integral requires doing further integrals, the body
    should call ‘`(math-integral x)`’ to find the integral of
    x with respect to `math-integ-var`; this function returns
    `nil` if the integral could not be done. Some examples:

    ```
    (math-defintegral calcFunc-conj
      (let ((int (math-integral u)))
        (and int
             (list 'calcFunc-conj int))))

    (math-defintegral calcFunc-cos
      (and (equal u math-integ-var)
           (math-from-radians-2 (list 'calcFunc-sin u))))
    ```

    In the `cos` example, we define only the integral of ‘`cos(x) dx`’,
    relying on the general integration-by-substitution facility to handle
    cosines of more complicated arguments. An integration rule should return
    `nil` if it can’t do the integral; if several rules are defined for
    the same function, they are tried in order until one returns a non-`nil`
    result.

Macro: **math-defintegral-2** funcs body [¶](#index-math_002ddefintegral_002d2)
:   Define a rule for integrating a function or functions of two arguments.
    This is exactly analogous to `math-defintegral`, except that body
    is written as the body of a function with two arguments, u and
    v.

Function: **solve-for** lhs rhs var full [¶](#index-solve_002dfor)
:   Attempt to solve the equation ‘`lhs = rhs`’ by isolating
    the variable var on the lefthand side; return the resulting righthand
    side, or `nil` if the equation cannot be solved. The variable
    var must appear at least once in lhs or rhs. Note that
    the return value is a formula which does not contain var; this is
    different from the user-level `solve` and `finv` functions,
    which return a rearranged equation or a functional inverse, respectively.
    If full is non-`nil`, a full solution including dummy signs
    and dummy integers will be produced. User-defined inverses are provided
    as properties in a manner similar to derivatives:

    ```
    (put 'calcFunc-ln 'math-inverse
         (lambda (x) (list 'calcFunc-exp x)))
    ```

    This function can call ‘`(math-solve-get-sign x)`’ to create
    a new arbitrary sign variable, returning x times that sign, and
    ‘`(math-solve-get-int x)`’ to create a new arbitrary integer
    variable multiplied by x. These functions simply return x
    if the caller requested a non-“full” solution.

Function: **solve-eqn** expr var full [¶](#index-solve_002deqn)
:   This version of `solve-for` takes an expression which will
    typically be an equation or inequality. (If it is not, it will be
    interpreted as the equation ‘`expr = 0`’.) It returns an
    equation or inequality, or `nil` if no solution could be found.

Function: **solve-system** exprs vars full [¶](#index-solve_002dsystem)
:   This function solves a system of equations. Generally, exprs
    and vars will be vectors of equal length.
    See [Solving Systems of Equations](#Solving-Systems-of-Equations), for other options.

Function: **expr-contains** expr var [¶](#index-expr_002dcontains)
:   Returns a non-`nil` value if var occurs as a subexpression
    of expr.

    This function might seem at first to be identical to
    `calc-find-sub-formula`. The key difference is that
    `expr-contains` uses `equal` to test for matches, whereas
    `calc-find-sub-formula` uses `eq`. In the formula
    ‘`f(a, a)`’, the two ‘`a`’s will be `equal` but not
    `eq` to each other.

Function: **expr-contains-count** expr var [¶](#index-expr_002dcontains_002dcount)
:   Returns the number of occurrences of var as a subexpression
    of expr, or `nil` if there are no occurrences.

Function: **expr-depends** expr var [¶](#index-expr_002ddepends)
:   Returns true if expr refers to any variable the occurs in var.
    In other words, it checks if expr and var have any variables
    in common.

Function: **expr-contains-vars** expr [¶](#index-expr_002dcontains_002dvars)
:   Return true if expr contains any variables, or `nil` if expr
    contains only constants and functions with constant arguments.

Function: **expr-subst** expr old new [¶](#index-expr_002dsubst)
:   Returns a copy of expr, with all occurrences of old replaced
    by new. This treats `lambda` forms specially with respect
    to the dummy argument variables, so that the effect is always to return
    expr evaluated at old = new.

Function: **multi-subst** expr old new [¶](#index-multi_002dsubst)
:   This is like `expr-subst`, except that old and new
    are lists of expressions to be substituted simultaneously. If one
    list is shorter than the other, trailing elements of the longer list
    are ignored.

Function: **expr-weight** expr [¶](#index-expr_002dweight)
:   Returns the “weight” of expr, basically a count of the total
    number of objects and function calls that appear in expr. For
    “primitive” objects, this will be one.

Function: **expr-height** expr [¶](#index-expr_002dheight)
:   Returns the “height” of expr, which is the deepest level to
    which function calls are nested. (Note that ‘`a + b`’
    counts as a function call.) For primitive objects, this returns zero.

Function: **polynomial-p** expr var [¶](#index-polynomial_002dp)
:   Check if expr is a polynomial in variable (or sub-expression)
    var. If so, return the degree of the polynomial, that is, the
    highest power of var that appears in expr. For example,
    for ‘`(x^2 + 3)^3 + 4`’ this would return 6. This function returns
    `nil` unless expr, when expanded out by `a x`
    (`calc-expand`), would consist of a sum of terms in which var
    appears only raised to nonnegative integer powers. Note that if
    var does not occur in expr, then expr is considered
    a polynomial of degree 0.

Function: **is-polynomial** expr var degree loose [¶](#index-is_002dpolynomial)
:   Check if expr is a polynomial in variable or sub-expression
    var, and, if so, return a list representation of the polynomial
    where the elements of the list are coefficients of successive powers of
    var: ‘`a + b x + c x^3`’ would produce the
    list ‘`(a b 0 c)`’, and ‘`(x + 1)^2`’ would
    produce the list ‘`(1 2 1)`’. The highest element of the list will
    be non-zero, with the special exception that if expr is the
    constant zero, the returned value will be ‘`(0)`’. Return `nil`
    if expr is not a polynomial in var. If degree is
    specified, this will not consider polynomials of degree higher than that
    value. This is a good precaution because otherwise an input of
    ‘`(x+1)^1000`’ will cause a huge coefficient list to be built. If
    loose is non-`nil`, then a looser definition of a polynomial
    is used in which coefficients are no longer required not to depend on
    var, but are only required not to take the form of polynomials
    themselves. For example, ‘`sin(x) x^2 + cos(x)`’ is a loose
    polynomial with coefficients ‘`((calcFunc-cos x) 0 (calcFunc-sin
    x))`’. The result will never be `nil` in loose mode, since any
    expression can be interpreted as a “constant” loose polynomial.

Function: **polynomial-base** expr pred [¶](#index-polynomial_002dbase)
:   Check if expr is a polynomial in any variable that occurs in it;
    if so, return that variable. (If expr is a multivariate polynomial,
    this chooses one variable arbitrarily.) If pred is specified, it should
    be a Lisp function which is called as ‘`(pred subexpr)`’,
    and which should return true if `mpb-top-expr` (a global name for
    the original expr) is a suitable polynomial in subexpr.
    The default predicate uses ‘`(polynomial-p mpb-top-expr subexpr)`’;
    you can use pred to specify additional conditions. Or, you could
    have pred build up a list of every suitable subexpr that
    is found.

Function: **poly-simplify** poly [¶](#index-poly_002dsimplify)
:   Simplify polynomial coefficient list poly by (destructively)
    clipping off trailing zeros.

Function: **poly-mix** a ac b bc [¶](#index-poly_002dmix)
:   Mix two polynomial lists a and b (in the form returned by
    `is-polynomial`) in a linear combination with coefficient expressions
    ac and bc. The result is a (not necessarily simplified)
    polynomial list representing ‘`ac a + bc b`’.

Function: **poly-mul** a b [¶](#index-poly_002dmul)
:   Multiply two polynomial coefficient lists a and b. The
    result will be in simplified form if the inputs were simplified.

Function: **build-polynomial-expr** poly var [¶](#index-build_002dpolynomial_002dexpr)
:   Construct a Calc formula which represents the polynomial coefficient
    list poly applied to variable var. The `a c`
    (`calc-collect`) command uses `is-polynomial` to turn an
    expression into a coefficient list, then `build-polynomial-expr`
    to turn the list back into an expression in regular form.

Function: **check-unit-name** var [¶](#index-check_002dunit_002dname)
:   Check if var is a variable which can be interpreted as a unit
    name. If so, return the units table entry for that unit. This
    will be a list whose first element is the unit name (not counting
    prefix characters) as a symbol and whose second element is the
    Calc expression which defines the unit. (Refer to the Calc sources
    for details on the remaining elements of this list.) If var
    is not a variable or is not a unit name, return `nil`.

Function: **units-in-expr-p** expr sub-exprs [¶](#index-units_002din_002dexpr_002dp)
:   Return true if expr contains any variables which can be
    interpreted as units. If sub-exprs is `t`, the entire
    expression is searched. If sub-exprs is `nil`, this
    checks whether expr is directly a units expression.

Function: **single-units-in-expr-p** expr [¶](#index-single_002dunits_002din_002dexpr_002dp)
:   Check whether expr contains exactly one units variable. If so,
    return the units table entry for the variable. If expr does
    not contain any units, return `nil`. If expr contains
    two or more units, return the symbol `wrong`.

Function: **to-standard-units** expr which [¶](#index-to_002dstandard_002dunits)
:   Convert units expression expr to base units. If which
    is `nil`, use Calc’s native base units. Otherwise, which
    can specify a units system, which is a list of two-element lists,
    where the first element is a Calc base symbol name and the second
    is an expression to substitute for it.

Function: **remove-units** expr [¶](#index-remove_002dunits)
:   Return a copy of expr with all units variables replaced by ones.
    This expression is generally normalized before use.

Function: **extract-units** expr [¶](#index-extract_002dunits)
:   Return a copy of expr with everything but units variables replaced
    by ones.

---

Next: [Hooks](#Hooks), Previous: [Symbolic Functions](#Symbolic-Lisp-Functions), Up: [Calculator Internals](#Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7.8 I/O and Formatting Functions [¶](#I_002fO-and-Formatting-Functions)

The functions described here are responsible for parsing and formatting
Calc numbers and formulas.

Function: **calc-eval** str sep arg1 arg2 … [¶](#index-calc_002deval)
:   This is the simplest interface to the Calculator from another Lisp program.
    See [Calling Calc from Your Lisp Programs](#Calling-Calc-from-Your-Programs).

Function: **read-number** str [¶](#index-read_002dnumber)
:   If string str contains a valid Calc number, either integer,
    fraction, float, or HMS form, this function parses and returns that
    number. Otherwise, it returns `nil`.

Function: **read-expr** str [¶](#index-read_002dexpr)
:   Read an algebraic expression from string str. If str does
    not have the form of a valid expression, return a list of the form
    ‘`(error pos msg)`’ where pos is an integer index
    into str of the general location of the error, and msg is
    a string describing the problem.

Function: **read-exprs** str [¶](#index-read_002dexprs)
:   Read a list of expressions separated by commas, and return it as a
    Lisp list. If an error occurs in any expressions, an error list as
    shown above is returned instead.

Function: **calc-do-alg-entry** initial prompt no-norm [¶](#index-calc_002ddo_002dalg_002dentry)
:   Read an algebraic formula or formulas using the minibuffer. All
    conventions of regular algebraic entry are observed. The return value
    is a list of Calc formulas; there will be more than one if the user
    entered a list of values separated by commas. The result is `nil`
    if the user presses Return with a blank line. If initial is
    given, it is a string which the minibuffer will initially contain.
    If prompt is given, it is the prompt string to use; the default
    is “Algebraic:”. If no-norm is `t`, the formulas will
    be returned exactly as parsed; otherwise, they will be passed through
    `calc-normalize` first.

    To support the use of `$` characters in the algebraic entry, use
    `let` to bind `calc-dollar-values` to a list of the values
    to be substituted for `$`, `$$`, and so on, and bind
    `calc-dollar-used` to 0. Upon return, `calc-dollar-used`
    will have been changed to the highest number of consecutive `$`s
    that actually appeared in the input.

Function: **format-number** a [¶](#index-format_002dnumber)
:   Convert the real or complex number or HMS form a to string form.

Function: **format-flat-expr** a prec [¶](#index-format_002dflat_002dexpr)
:   Convert the arbitrary Calc number or formula a to string form,
    in the style used by the trail buffer and the `calc-edit` command.
    This is a simple format designed
    mostly to guarantee the string is of a form that can be re-parsed by
    `read-expr`. Most formatting modes, such as digit grouping,
    complex number format, and point character, are ignored to ensure the
    result will be re-readable. The prec parameter is normally 0; if
    you pass a large integer like 1000 instead, the expression will be
    surrounded by parentheses unless it is a plain number or variable name.

Function: **format-nice-expr** a width [¶](#index-format_002dnice_002dexpr)
:   This is like `format-flat-expr` (with prec equal to 0),
    except that newlines will be inserted to keep lines down to the
    specified width, and vectors that look like matrices or rewrite
    rules are written in a pseudo-matrix format. The `calc-edit`
    command uses this when only one stack entry is being edited.

Function: **format-value** a width [¶](#index-format_002dvalue)
:   Convert the Calc number or formula a to string form, using the
    format seen in the stack buffer. Beware the string returned may
    not be re-readable by `read-expr`, for example, because of digit
    grouping. Multi-line objects like matrices produce strings that
    contain newline characters to separate the lines. The w
    parameter, if given, is the target window size for which to format
    the expressions. If w is omitted, the width of the Calculator
    window is used.

Function: **compose-expr** a prec [¶](#index-compose_002dexpr)
:   Format the Calc number or formula a according to the current
    language mode, returning a “composition.” To learn about the
    structure of compositions, see the comments in the Calc source code.
    You can specify the format of a given type of function call by putting
    a `math-compose-lang` property on the function’s symbol,
    whose value is a Lisp function that takes a and prec as
    arguments and returns a composition. Here lang is a language
    mode name, one of `normal`, `big`, `c`, `pascal`,
    `fortran`, `tex`, `eqn`, `math`, or `maple`.
    In Big mode, Calc actually tries `math-compose-big` first, then
    tries `math-compose-normal`. If this property does not exist,
    or if the function returns `nil`, the function is written in the
    normal function-call notation for that language.

Function: **composition-to-string** c w [¶](#index-composition_002dto_002dstring)
:   Convert a composition structure returned by `compose-expr` into
    a string. Multi-line compositions convert to strings containing
    newline characters. The target window size is given by w.
    The `format-value` function basically calls `compose-expr`
    followed by `composition-to-string`.

Function: **comp-width** c [¶](#index-comp_002dwidth)
:   Compute the width in characters of composition c.

Function: **comp-height** c [¶](#index-comp_002dheight)
:   Compute the height in lines of composition c.

Function: **comp-ascent** c [¶](#index-comp_002dascent)
:   Compute the portion of the height of composition c which is on or
    above the baseline. For a one-line composition, this will be one.

Function: **comp-descent** c [¶](#index-comp_002ddescent)
:   Compute the portion of the height of composition c which is below
    the baseline. For a one-line composition, this will be zero.

Function: **comp-first-char** c [¶](#index-comp_002dfirst_002dchar)
:   If composition c is a “flat” composition, return the first
    (leftmost) character of the composition as an integer. Otherwise,
    return `nil`.

Function: **comp-last-char** c [¶](#index-comp_002dlast_002dchar)
:   If composition c is a “flat” composition, return the last
    (rightmost) character, otherwise return `nil`.

---

Previous: [I/O and Formatting Functions](#Formatting-Lisp-Functions), Up: [Calculator Internals](#Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.5.7.9 Hooks [¶](#Hooks-1)

Hooks are variables which contain Lisp functions (or lists of functions)
which are called at various times. Calc defines a number of hooks
that help you to customize it in various ways. Calc uses the Lisp
function `run-hooks` to invoke the hooks shown below. Several
other customization-related variables are also described here.
To run code after Calc has loaded, use `with-eval-after-load`.

Variable: **calc-start-hook** [¶](#index-calc_002dstart_002dhook)
:   This hook is called as the last step in a `M-x calc` command.
    At this point, the Calc buffer has been created and initialized if
    necessary, the Calc window and trail window have been created,
    and the “Welcome to Calc” message has been displayed.

Variable: **calc-mode-hook** [¶](#index-calc_002dmode_002dhook)
:   This hook is called when the Calc buffer is being created. Usually
    this will only happen once per Emacs session. The hook is called
    after Emacs has switched to the new buffer, the mode-settings file
    has been read if necessary, and all other buffer-local variables
    have been set up. After this hook returns, Calc will perform a
    `calc-refresh` operation, set up the mode line display, then
    evaluate any deferred `calc-define` properties that have not
    been evaluated yet.

Variable: **calc-trail-mode-hook** [¶](#index-calc_002dtrail_002dmode_002dhook)
:   This hook is called when the Calc Trail buffer is being created.
    It is called as the very last step of setting up the Trail buffer.
    Like `calc-mode-hook`, this will normally happen only once
    per Emacs session.

Variable: **calc-end-hook** [¶](#index-calc_002dend_002dhook)
:   This hook is called by `calc-quit`, generally because the user
    presses `q` or `C-x * c` while in Calc. The Calc buffer will
    be the current buffer. The hook is called as the very first
    step, before the Calc window is destroyed.

Variable: **calc-window-hook** [¶](#index-calc_002dwindow_002dhook)
:   If this hook is non-`nil`, it is called to create the Calc window.
    Upon return, this new Calc window should be the current window.
    (The Calc buffer will already be the current buffer when the
    hook is called.) If the hook is not defined, Calc will
    generally use `split-window`, `set-window-buffer`,
    and `select-window` to create the Calc window.

Variable: **calc-trail-window-hook** [¶](#index-calc_002dtrail_002dwindow_002dhook)
:   If this hook is non-`nil`, it is called to create the Calc Trail
    window. The variable `calc-trail-buffer` will contain the buffer
    which the window should use. Unlike `calc-window-hook`, this hook
    must *not* switch into the new window.

Variable: **calc-embedded-mode-hook** [¶](#index-calc_002dembedded_002dmode_002dhook)
:   This hook is called the first time that Embedded mode is entered.

Variable: **calc-embedded-new-buffer-hook** [¶](#index-calc_002dembedded_002dnew_002dbuffer_002dhook)
:   This hook is called each time that Embedded mode is entered in a
    new buffer.

Variable: **calc-embedded-new-formula-hook** [¶](#index-calc_002dembedded_002dnew_002dformula_002dhook)
:   This hook is called each time that Embedded mode is enabled for a
    new formula.

Variable: **calc-edit-mode-hook** [¶](#index-calc_002dedit_002dmode_002dhook)
:   This hook is called by `calc-edit` (and the other “edit”
    commands) when the temporary editing buffer is being created.
    The buffer will have been selected and set up to be in
    `calc-edit-mode`, but will not yet have been filled with
    text. (In fact it may still have leftover text from a previous
    `calc-edit` command.)

Variable: **calc-mode-save-hook** [¶](#index-calc_002dmode_002dsave_002dhook)
:   This hook is called by the `calc-save-modes` command,
    after Calc’s own mode features have been inserted into the
    Calc init file and just before the “End of mode settings”
    message is inserted.

Variable: **calc-reset-hook** [¶](#index-calc_002dreset_002dhook)
:   This hook is called after `C-x * 0` (`calc-reset`) has
    reset all modes. The Calc buffer will be the current buffer.

Variable: **calc-other-modes** [¶](#index-calc_002dother_002dmodes)
:   This variable contains a list of strings. The strings are
    concatenated at the end of the modes portion of the Calc
    mode line (after standard modes such as “Deg”, “Inv” and
    “Hyp”). Each string should be a short, single word followed
    by a space. The variable is `nil` by default.

Variable: **calc-mode-map** [¶](#index-calc_002dmode_002dmap)
:   This is the keymap that is used by Calc mode. The best time
    to adjust it is probably in a `calc-mode-hook`. If the
    Calc extensions package (`calc-ext.el`) has not yet been
    loaded, many of these keys will be bound to `calc-missing-key`,
    which is a command that loads the extensions package and
    “retypes” the key. If your `calc-mode-hook` rebinds
    one of these keys, it will probably be overridden when the
    extensions are loaded.

Variable: **calc-digit-map** [¶](#index-calc_002ddigit_002dmap)
:   This is the keymap that is used during numeric entry. Numeric
    entry uses the minibuffer, but this map binds every non-numeric
    key to `calcDigit-nondigit` which generally calls
    `exit-minibuffer` and “retypes” the key.

Variable: **calc-alg-ent-map** [¶](#index-calc_002dalg_002dent_002dmap)
:   This is the keymap that is used during algebraic entry. This is
    mostly a copy of `minibuffer-local-map`.

Variable: **calc-store-var-map** [¶](#index-calc_002dstore_002dvar_002dmap)
:   This is the keymap that is used during entry of variable names for
    commands like `calc-store` and `calc-recall`. This is
    mostly a copy of `minibuffer-local-completion-map`.

Variable: **calc-edit-mode-map** [¶](#index-calc_002dedit_002dmode_002dmap)
:   This is the (sparse) keymap used by `calc-edit` and other
    temporary editing commands. It binds `RET`, `LFD`,
    and `C-c C-c` to `calc-edit-finish`.

Variable: **calc-mode-var-list** [¶](#index-calc_002dmode_002dvar_002dlist)
:   This is a list of variables which are saved by `calc-save-modes`.
    Each entry is a list of two items, the variable (as a Lisp symbol)
    and its default value. When modes are being saved, each variable
    is compared with its default value (using `equal`) and any
    non-default variables are written out.

Variable: **calc-local-var-list** [¶](#index-calc_002dlocal_002dvar_002dlist)
:   This is a list of variables which should be buffer-local to the
    Calc buffer. Each entry is a variable name (as a Lisp symbol).
    These variables also have their default values manipulated by
    the `calc` and `calc-quit` commands; see [Multiple Calculators](#Multiple-Calculators).
    Since `calc-mode-hook` is called after this list has been
    used the first time, your hook should add a variable to the
    list and also call `make-local-variable` itself.

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [Programming](#Programming), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## Appendix A GNU GENERAL PUBLIC LICENSE [¶](#GNU-GENERAL-PUBLIC-LICENSE)

Version 3, 29 June 2007

```
Copyright © 2007 Free Software Foundation, Inc. https://fsf.org/

Everyone is permitted to copy and distribute verbatim copies of this
license document, but changing it is not allowed.
```

### Preamble [¶](#Preamble)

The GNU General Public License is a free, copyleft license for
software and other kinds of works.

The licenses for most software and other practical works are designed
to take away your freedom to share and change the works. By contrast,
the GNU General Public License is intended to guarantee your freedom
to share and change all versions of a program—to make sure it remains
free software for all its users. We, the Free Software Foundation,
use the GNU General Public License for most of our software; it
applies also to any other work released this way by its authors. You
can apply it to your programs, too.

When we speak of free software, we are referring to freedom, not
price. Our General Public Licenses are designed to make sure that you
have the freedom to distribute copies of free software (and charge for
them if you wish), that you receive source code or can get it if you
want it, that you can change the software or use pieces of it in new
free programs, and that you know you can do these things.

To protect your rights, we need to prevent others from denying you
these rights or asking you to surrender the rights. Therefore, you
have certain responsibilities if you distribute copies of the
software, or if you modify it: responsibilities to respect the freedom
of others.

For example, if you distribute copies of such a program, whether
gratis or for a fee, you must pass on to the recipients the same
freedoms that you received. You must make sure that they, too,
receive or can get the source code. And you must show them these
terms so they know their rights.

Developers that use the GNU GPL protect your rights with two steps:
(1) assert copyright on the software, and (2) offer you this License
giving you legal permission to copy, distribute and/or modify it.

For the developers’ and authors’ protection, the GPL clearly explains
that there is no warranty for this free software. For both users’ and
authors’ sake, the GPL requires that modified versions be marked as
changed, so that their problems will not be attributed erroneously to
authors of previous versions.

Some devices are designed to deny users access to install or run
modified versions of the software inside them, although the
manufacturer can do so. This is fundamentally incompatible with the
aim of protecting users’ freedom to change the software. The
systematic pattern of such abuse occurs in the area of products for
individuals to use, which is precisely where it is most unacceptable.
Therefore, we have designed this version of the GPL to prohibit the
practice for those products. If such problems arise substantially in
other domains, we stand ready to extend this provision to those
domains in future versions of the GPL, as needed to protect the
freedom of users.

Finally, every program is threatened constantly by software patents.
States should not allow patents to restrict development and use of
software on general-purpose computers, but in those that do, we wish
to avoid the special danger that patents applied to a free program
could make it effectively proprietary. To prevent this, the GPL
assures that patents cannot be used to render the program non-free.

The precise terms and conditions for copying, distribution and
modification follow.
