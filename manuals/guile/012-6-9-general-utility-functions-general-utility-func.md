### 6.9 General Utility Functions [¶](#General-Utility-Functions)

This chapter contains information about procedures which are not cleanly
tied to a specific data type. Because of their wide range of
applications, they are collected in a *utility* chapter.

* [Equality](#Equality)
* [Object Properties](#Object-Properties)
* [Sorting](#Sorting)
* [Copying Deep Structures](#Copying)
* [General String Conversion](#General-Conversion)
* [Hooks](#Hooks)

---

Next: [Object Properties](#Object-Properties), Up: [General Utility Functions](#Utility-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.1 Equality [¶](#Equality-1)

There are three kinds of core equality predicates in Scheme, described
below. The same kinds of comparisons arise in other functions, like
`memq` and friends (see [List Searching](#List-Searching)).

For all three tests, objects of different types are never equal. So
for instance a list and a vector are not `equal?`, even if their
contents are the same. Exact and inexact numbers are considered
different types too, and are hence not equal even if their values are
the same.

`eq?` tests just for the same object (essentially a pointer
comparison). This is fast, and can be used when searching for a
particular object, or when working with symbols or keywords (which are
always unique objects).

`eqv?` extends `eq?` to look at the value of numbers and
characters. It can for instance be used somewhat like `=`
(see [Comparison Predicates](#Comparison)) but without an error if one operand isn’t a
number.

`equal?` goes further, it looks (recursively) into the contents
of lists, vectors, etc. This is good for instance on lists that have
been read or calculated in various places and are the same, just not
made up of the same pairs. Such lists look the same (when printed),
and `equal?` will consider them the same.

  

Scheme Procedure: **eq?** … [¶](#index-eq_003f)

C Function: **scm\_eq\_p** (x, y) [¶](#index-scm_005feq_005fp)
:   The Scheme procedure returns `#t` if all of its arguments are the
    same object, except for numbers and characters. The C function does the
    same but takes exactly two arguments. For example,

    ```
    (define x (vector 1 2 3))
    (define y (vector 1 2 3))

    (eq? x x)  ⇒ #t
    (eq? x y)  ⇒ #f
    ```

    Numbers and characters are not equal to any other object, but the
    problem is they’re not necessarily `eq?` to themselves either.
    This is even so when the number comes directly from a variable,

    ```
    (let ((n (+ 2 3)))
      (eq? n n))       ⇒ *unspecified*
    ```

    Generally `eqv?` below should be used when comparing numbers or
    characters. `=` (see [Comparison Predicates](#Comparison)) or `char=?`
    (see [Characters](#Characters)) can be used too.

    It’s worth noting that end-of-list `()`, `#t`, `#f`, a
    symbol of a given name, and a keyword of a given name, are unique
    objects. There’s just one of each, so for instance no matter how
    `()` arises in a program, it’s the same object and can be
    compared with `eq?`,

    ```
    (define x (cdr '(123)))
    (define y (cdr '(456)))
    (eq? x y) ⇒ #t

    (define x (string->symbol "foo"))
    (eq? x 'foo) ⇒ #t
    ```

C Function: `int` **scm\_is\_eq** `(SCM x, SCM y)` [¶](#index-scm_005fis_005feq)
:   Return `1` when x and y are equal in the sense of
    `eq?`, otherwise return `0`.

    The `==` operator should not be used on `SCM` values, an
    `SCM` is a C type which cannot necessarily be compared using
    `==` (see [The SCM Type](#The-SCM-Type)).

  

Scheme Procedure: **eqv?** … [¶](#index-eqv_003f)

C Function: **scm\_eqv\_p** (x, y) [¶](#index-scm_005feqv_005fp)
:   The Scheme procedure returns `#t` if all of its arguments are the
    same object, or for characters and numbers the same value. The C function
    is similar but takes exactly two arguments.

    On objects except characters and numbers, `eqv?` is the same as
    `eq?` above. `(eqv? x y)` is true if x and y are
    the same object.

    If x and y are numbers or characters, `eqv?` compares
    their type and value. An exact number is not `eqv?` to an
    inexact number (even if their value is the same).

    ```
    (eqv? 3 (+ 1 2)) ⇒ #t
    (eqv? 1 1.0)     ⇒ #f
    ```

  

Scheme Procedure: **equal?** … [¶](#index-equal_003f)

C Function: **scm\_equal\_p** (x, y) [¶](#index-scm_005fequal_005fp)
:   The Scheme procedure returns `#t` if all of its arguments are the
    same type, and their contents or value are equal. The C function is
    similar, but takes exactly two arguments.

    For a pair, string, vector, array or structure, `equal?` compares the
    contents, and does so using the same `equal?` recursively,
    so a deep structure can be traversed.

    ```
    (equal? (list 1 2 3) (list 1 2 3))   ⇒ #t
    (equal? (list 1 2 3) (vector 1 2 3)) ⇒ #f
    ```

    For other objects, `equal?` compares as per `eqv?` above,
    which means characters and numbers are compared by type and value (and
    like `eqv?`, exact and inexact numbers are not `equal?`,
    even if their value is the same).

    ```
    (equal? 3 (+ 1 2)) ⇒ #t
    (equal? 1 1.0)     ⇒ #f
    ```

    Hash tables are currently only compared as per `eq?`, so two
    different tables are not `equal?`, even if their contents are the
    same.

    `equal?` does not support circular data structures, it may go
    into an infinite loop if asked to compare two circular lists or
    similar.

    GOOPS object types (see [GOOPS](#GOOPS)), including foreign object types
    (see [Defining New Foreign Object Types](#Defining-New-Foreign-Object-Types)), can have an `equal?`
    implementation specialized on two values of the same type. If
    `equal?` is called on two GOOPS objects of the same type,
    `equal?` will dispatch out to a generic function. This lets an
    application traverse the contents or control what is considered
    `equal?` for two objects of such a type. If there’s no such
    handler, the default is to just compare as per `eq?`.

---

Next: [Sorting](#Sorting), Previous: [Equality](#Equality), Up: [General Utility Functions](#Utility-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.2 Object Properties [¶](#Object-Properties-1)

It’s often useful to associate a piece of additional information with a
Scheme object even though that object does not have a dedicated slot
available in which the additional information could be stored. Object
properties allow you to do just that.

Guile’s representation of an object property is a procedure-with-setter
(see [Procedures with Setters](#Procedures-with-Setters)) that can be used with the generalized
form of `set!` to set and retrieve that property for any Scheme object. So, setting a
property looks like this:

```
(set! (my-property obj1) value-for-obj1)
(set! (my-property obj2) value-for-obj2)
```

And retrieving values of the same property looks like this:

```
(my-property obj1)
⇒
value-for-obj1

(my-property obj2)
⇒
value-for-obj2
```

To create an object property in the first place, use the
`make-object-property` procedure:

```
(define my-property (make-object-property))
```

Scheme Procedure: **make-object-property** [¶](#index-make_002dobject_002dproperty)
:   Create and return an object property. An object property is a
    procedure-with-setter that can be called in two ways. `(set!
    (property obj) val)` sets obj’s property
    to val. `(property obj)` returns the current
    setting of obj’s property.

A single object property created by `make-object-property` can
associate distinct property values with all Scheme values that are
distinguishable by `eq?` (ruling out numeric values).

Internally, object properties are implemented using a weak key hash
table. This means that, as long as a Scheme value with property values
is protected from garbage collection, its property values are also
protected. When the Scheme value is collected, its entry in the
property table is removed and so the (ex-) property values are no longer
protected by the table.

Guile also implements a more traditional Lispy interface to properties,
in which each object has an list of key-value pairs associated with it.
Properties in that list are keyed by symbols. This is a legacy
interface; you should use weak hash tables or object properties instead.

Scheme Procedure: **object-properties** obj [¶](#index-object_002dproperties)

C Function: **scm\_object\_properties** (obj) [¶](#index-scm_005fobject_005fproperties)
:   Return obj’s property list.

Scheme Procedure: **set-object-properties!** obj alist [¶](#index-set_002dobject_002dproperties_0021)

C Function: **scm\_set\_object\_properties\_x** (obj, alist) [¶](#index-scm_005fset_005fobject_005fproperties_005fx)
:   Set obj’s property list to alist.

Scheme Procedure: **object-property** obj key [¶](#index-object_002dproperty)

C Function: **scm\_object\_property** (obj, key) [¶](#index-scm_005fobject_005fproperty)
:   Return the property of obj with name key.

Scheme Procedure: **set-object-property!** obj key value [¶](#index-set_002dobject_002dproperty_0021)

C Function: **scm\_set\_object\_property\_x** (obj, key, value) [¶](#index-scm_005fset_005fobject_005fproperty_005fx)
:   In obj’s property list, set the property named key
    to value.

---

Next: [Copying Deep Structures](#Copying), Previous: [Object Properties](#Object-Properties), Up: [General Utility Functions](#Utility-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.3 Sorting [¶](#Sorting-1)

Sorting is very important in computer programs. Therefore, Guile comes
with several sorting procedures built-in. As always, procedures with
names ending in `!` are side-effecting, that means that they may
modify their parameters in order to produce their results. The predicate
less passed as second or third argument to the procedures below is
assumed to define a strict weak order on the elements to be merged or
sorted.

The first group of procedures can be used to merge two lists (which must
be already sorted on their own) and produce sorted lists containing
all elements of the input lists.

Scheme Procedure: **merge** alist blist less [¶](#index-merge)

C Function: **scm\_merge** (alist, blist, less) [¶](#index-scm_005fmerge)
:   Merge two already sorted lists into one.
    Given two lists alist and blist, such that
    `(sorted? alist less?)` and `(sorted? blist less?)`,
    return a new list in which the elements of alist and
    blist have been stably interleaved so that
    `(sorted? (merge alist blist less?) less?)`.
    Note: this does \_not\_ accept vectors.

Scheme Procedure: **merge!** alist blist less [¶](#index-merge_0021)

C Function: **scm\_merge\_x** (alist, blist, less) [¶](#index-scm_005fmerge_005fx)
:   Takes two lists alist and blist such that
    `(sorted? alist less?)` and `(sorted? blist less?)` and
    returns a new list in which the elements of alist and
    blist have been stably interleaved so that
    `(sorted? (merge alist blist less?) less?)`.
    This is the destructive variant of `merge`
    Note: this does \_not\_ accept vectors.

The following procedures can operate on sequences which are either
vectors or list. According to the given arguments, they return sorted
vectors or lists, respectively. The first of the following procedures
determines whether a sequence is already sorted, the other sort a given
sequence. The variants with names starting with `stable-` are
special in that they maintain a special property of the input sequences:
If two or more elements are the same according to the comparison
predicate, they are left in the same order as they appeared in the
input.

Scheme Procedure: **sorted?** items less [¶](#index-sorted_003f)

C Function: **scm\_sorted\_p** (items, less) [¶](#index-scm_005fsorted_005fp)
:   Return `#t` if items is a list or vector such that,
    for each element x and the next element y of
    items, `(less y x)` returns
    `#f`. Otherwise return `#f`.

Scheme Procedure: **sort** items less [¶](#index-sort)

C Function: **scm\_sort** (items, less) [¶](#index-scm_005fsort)
:   Sort the sequence items, which may be a list or a
    vector. less is used for comparing the sequence
    elements. This is not a stable sort.

Scheme Procedure: **sort!** items less [¶](#index-sort_0021)

C Function: **scm\_sort\_x** (items, less) [¶](#index-scm_005fsort_005fx)
:   Sort the sequence items, which may be a list or a
    vector. less is used for comparing the sequence
    elements. The sorting is destructive, that means that the
    input sequence is modified to produce the sorted result.
    This is not a stable sort.

Scheme Procedure: **stable-sort** items less [¶](#index-stable_002dsort)

C Function: **scm\_stable\_sort** (items, less) [¶](#index-scm_005fstable_005fsort)
:   Sort the sequence items, which may be a list or a
    vector. less is used for comparing the sequence elements.
    This is a stable sort.

Scheme Procedure: **stable-sort!** items less [¶](#index-stable_002dsort_0021)

C Function: **scm\_stable\_sort\_x** (items, less) [¶](#index-scm_005fstable_005fsort_005fx)
:   Sort the sequence items, which may be a list or a
    vector. less is used for comparing the sequence elements.
    The sorting is destructive, that means that the input sequence
    is modified to produce the sorted result.
    This is a stable sort.

The procedures in the last group only accept lists or vectors as input,
as their names indicate.

Scheme Procedure: **sort-list** items less [¶](#index-sort_002dlist)

C Function: **scm\_sort\_list** (items, less) [¶](#index-scm_005fsort_005flist)
:   Sort the list items, using less for comparing the
    list elements. This is a stable sort.

Scheme Procedure: **sort-list!** items less [¶](#index-sort_002dlist_0021)

C Function: **scm\_sort\_list\_x** (items, less) [¶](#index-scm_005fsort_005flist_005fx)
:   Sort the list items, using less for comparing the
    list elements. The sorting is destructive, that means that the
    input list is modified to produce the sorted result.
    This is a stable sort.

Scheme Procedure: **restricted-vector-sort!** vec less startpos endpos [¶](#index-restricted_002dvector_002dsort_0021)

C Function: **scm\_restricted\_vector\_sort\_x** (vec, less, startpos, endpos) [¶](#index-scm_005frestricted_005fvector_005fsort_005fx)
:   Sort the vector vec, using less for comparing
    the vector elements. startpos (inclusively) and
    endpos (exclusively) delimit
    the range of the vector which gets sorted. The return value
    is not specified.

---

Next: [General String Conversion](#General-Conversion), Previous: [Sorting](#Sorting), Up: [General Utility Functions](#Utility-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.4 Copying Deep Structures [¶](#Copying-Deep-Structures)

The procedures for copying lists (see [Lists](#Lists)) only produce a flat
copy of the input list, and currently Guile does not even contain
procedures for copying vectors. The `(ice-9 copy-tree)` module
contains a `copy-tree` function that can be used for this purpose,
as it does not only copy the spine of a list, but also copies any pairs
in the cars of the input lists.

```
(use-modules (ice-9 copy-tree))
```

Scheme Procedure: **copy-tree** obj [¶](#index-copy_002dtree)

C Function: **scm\_copy\_tree** (obj) [¶](#index-scm_005fcopy_005ftree)
:   Recursively copy the data tree that is bound to obj, and return
    the new data structure. `copy-tree` recurses down the
    contents of both pairs and vectors (since both cons cells and vector
    cells may point to arbitrary objects), and stops recursing when it hits
    any other object.

---

Next: [Hooks](#Hooks), Previous: [Copying Deep Structures](#Copying), Up: [General Utility Functions](#Utility-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.5 General String Conversion [¶](#General-String-Conversion)

When debugging Scheme programs, but also for providing a human-friendly
interface, a procedure for converting any Scheme object into string
format is very useful. Conversion from/to strings can of course be done
with specialized procedures when the data type of the object to convert
is known, but with this procedure, it is often more comfortable.

`object->string` converts an object by using a print procedure for
writing to a string port, and then returning the resulting string.
Converting an object back from the string is only possible if the object
type has a read syntax and the read syntax is preserved by the printing
procedure.

Scheme Procedure: **object->string** obj [printer] [¶](#index-object_002d_003estring)

C Function: **scm\_object\_to\_string** (obj, printer) [¶](#index-scm_005fobject_005fto_005fstring)
:   Return a Scheme string obtained by printing obj.
    Printing function can be specified by the optional second
    argument printer (default: `write`).

---

Previous: [General String Conversion](#General-Conversion), Up: [General Utility Functions](#Utility-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.6 Hooks [¶](#Hooks-1)

A hook is a list of procedures to be called at well defined points in
time. Typically, an application provides a hook h and promises
its users that it will call all of the procedures in h at a
defined point in the application’s processing. By adding its own
procedure to h, an application user can tap into or even influence
the progress of the application.

Guile itself provides several such hooks for debugging and customization
purposes: these are listed in a subsection below.

When an application first creates a hook, it needs to know how many
arguments will be passed to the hook’s procedures when the hook is run.
The chosen number of arguments (which may be none) is declared when the
hook is created, and all the procedures that are added to that hook must
be capable of accepting that number of arguments.

A hook is created using `make-hook`. A procedure can be added to
or removed from a hook using `add-hook!` or `remove-hook!`,
and all of a hook’s procedures can be removed together using
`reset-hook!`. When an application wants to run a hook, it does so
using `run-hook`.

* [Hook Usage by Example](#Hook-Example)
* [Hook Reference](#Hook-Reference)
* [Hooks For C Code.](#C-Hooks)
* [Hooks for Garbage Collection](#GC-Hooks)
* [Hooks into the Guile REPL](#REPL-Hooks)

---

Next: [Hook Reference](#Hook-Reference), Up: [Hooks](#Hooks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.6.1 Hook Usage by Example [¶](#Hook-Usage-by-Example)

Hook usage is shown by some examples in this section. First, we will
define a hook of arity 2 — that is, the procedures stored in the hook
will have to accept two arguments.

```
(define hook (make-hook 2))
hook
⇒ #<hook 2 40286c90>
```

Now we are ready to add some procedures to the newly created hook with
`add-hook!`. In the following example, two procedures are added,
which print different messages and do different things with their
arguments.

```
(add-hook! hook (lambda (x y)
                    (display "Foo: ")
                    (display (+ x y))
                    (newline)))
(add-hook! hook (lambda (x y)
                    (display "Bar: ")
                    (display (* x y))
                    (newline)))
```

Once the procedures have been added, we can invoke the hook using
`run-hook`.

```
(run-hook hook 3 4)
⊣ Bar: 12
⊣ Foo: 7
```

Note that the procedures are called in the reverse of the order with
which they were added. This is because the default behavior of
`add-hook!` is to add its procedure to the *front* of the
hook’s procedure list. You can force `add-hook!` to add its
procedure to the *end* of the list instead by providing a third
`#t` argument on the second call to `add-hook!`.

```
(add-hook! hook (lambda (x y)
                    (display "Foo: ")
                    (display (+ x y))
                    (newline)))
(add-hook! hook (lambda (x y)
                    (display "Bar: ")
                    (display (* x y))
                    (newline))
                    #t)             ; <- Change here!

(run-hook hook 3 4)
⊣ Foo: 7
⊣ Bar: 12
```

---

Next: [Hooks For C Code.](#C-Hooks), Previous: [Hook Usage by Example](#Hook-Example), Up: [Hooks](#Hooks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.6.2 Hook Reference [¶](#Hook-Reference-1)

When you create a hook with `make-hook`, you must specify the arity
of the procedures which can be added to the hook. If the arity is not
given explicitly as an argument to `make-hook`, it defaults to
zero. All procedures of a given hook must have the same arity, and when
the procedures are invoked using `run-hook`, the number of
arguments passed must match the arity specified at hook creation time.

The order in which procedures are added to a hook matters. If the third
parameter to `add-hook!` is omitted or is equal to `#f`, the
procedure is added in front of the procedures which might already be on
that hook, otherwise the procedure is added at the end. The procedures
are always called from the front to the end of the list when they are
invoked via `run-hook`.

The ordering of the list of procedures returned by `hook->list`
matches the order in which those procedures would be called if the hook
was run using `run-hook`.

Note that the C functions in the following entries are for handling
*Scheme-level* hooks in C. There are also *C-level* hooks which
have their own interface (see [Hooks For C Code.](#C-Hooks)).

Scheme Procedure: **make-hook** [n\_args] [¶](#index-make_002dhook)

C Function: **scm\_make\_hook** (n\_args) [¶](#index-scm_005fmake_005fhook)
:   Create a hook for storing procedure of arity n\_args.
    n\_args defaults to zero. The returned value is a hook
    object to be used with the other hook procedures.

Scheme Procedure: **hook?** x [¶](#index-hook_003f)

C Function: **scm\_hook\_p** (x) [¶](#index-scm_005fhook_005fp)
:   Return `#t` if x is a hook, `#f` otherwise.

Scheme Procedure: **hook-empty?** hook [¶](#index-hook_002dempty_003f)

C Function: **scm\_hook\_empty\_p** (hook) [¶](#index-scm_005fhook_005fempty_005fp)
:   Return `#t` if hook is an empty hook, `#f`
    otherwise.

Scheme Procedure: **add-hook!** hook proc [append\_p] [¶](#index-add_002dhook_0021)

C Function: **scm\_add\_hook\_x** (hook, proc, append\_p) [¶](#index-scm_005fadd_005fhook_005fx)
:   Add the procedure proc to the hook hook. The
    procedure is added to the end if append\_p is true,
    otherwise it is added to the front. The return value of this
    procedure is not specified.

Scheme Procedure: **remove-hook!** hook proc [¶](#index-remove_002dhook_0021)

C Function: **scm\_remove\_hook\_x** (hook, proc) [¶](#index-scm_005fremove_005fhook_005fx)
:   Remove the procedure proc from the hook hook. The
    return value of this procedure is not specified.

Scheme Procedure: **reset-hook!** hook [¶](#index-reset_002dhook_0021)

C Function: **scm\_reset\_hook\_x** (hook) [¶](#index-scm_005freset_005fhook_005fx)
:   Remove all procedures from the hook hook. The return
    value of this procedure is not specified.

Scheme Procedure: **hook->list** hook [¶](#index-hook_002d_003elist)

C Function: **scm\_hook\_to\_list** (hook) [¶](#index-scm_005fhook_005fto_005flist)
:   Convert the procedure list of hook to a list.

Scheme Procedure: **run-hook** hook arg … [¶](#index-run_002dhook)

C Function: **scm\_run\_hook** (hook, args) [¶](#index-scm_005frun_005fhook)
:   Apply all procedures from the hook hook to the arguments arg
    .... The order of the procedure application is first to last.
    The return value of this procedure is not specified.

If, in C code, you are certain that you have a hook object and well
formed argument list for that hook, you can also use
`scm_c_run_hook`, which is identical to `scm_run_hook` but
does no type checking.

C Function: `void` **scm\_c\_run\_hook** `(SCM hook, SCM args)` [¶](#index-scm_005fc_005frun_005fhook)
:   The same as `scm_run_hook` but without any type checking to confirm
    that hook is actually a hook object and that args is a
    well-formed list matching the arity of the hook.

For C code, `SCM_HOOKP` is a faster alternative to
`scm_hook_p`:

C Macro: `int` **SCM\_HOOKP** `(x)` [¶](#index-SCM_005fHOOKP)
:   Return 1 if x is a Scheme-level hook, 0 otherwise.

---

Next: [Hooks for Garbage Collection](#GC-Hooks), Previous: [Hook Reference](#Hook-Reference), Up: [Hooks](#Hooks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.6.3 Hooks For C Code. [¶](#Hooks-For-C-Code_002e)

The hooks already described are intended to be populated by Scheme-level
procedures. In addition to this, the Guile library provides an
independent set of interfaces for the creation and manipulation of hooks
that are designed to be populated by functions implemented in C.

The original motivation here was to provide a kind of hook that could
safely be invoked at various points during garbage collection.
Scheme-level hooks are unsuitable for this purpose as running them could
itself require memory allocation, which would then invoke garbage
collection recursively … However, it is also the case that these
hooks are easier to work with than the Scheme-level ones if you only
want to register C functions with them. So if that is mainly what your
code needs to do, you may prefer to use this interface.

To create a C hook, you should allocate storage for a structure of type
`scm_t_c_hook` and then initialize it using `scm_c_hook_init`.

C Type: **scm\_t\_c\_hook** [¶](#index-scm_005ft_005fc_005fhook)
:   Data type for a C hook. The internals of this type should be treated as
    opaque.

C Enum: **scm\_t\_c\_hook\_type** [¶](#index-scm_005ft_005fc_005fhook_005ftype)
:   Enumeration of possible hook types, which are:

    `SCM_C_HOOK_NORMAL` [¶](#index-SCM_005fC_005fHOOK_005fNORMAL)
    :   Type of hook for which all the registered functions will always be called.

    `SCM_C_HOOK_OR` [¶](#index-SCM_005fC_005fHOOK_005fOR)
    :   Type of hook for which the sequence of registered functions will be
        called only until one of them returns C true (a non-NULL pointer).

    `SCM_C_HOOK_AND` [¶](#index-SCM_005fC_005fHOOK_005fAND)
    :   Type of hook for which the sequence of registered functions will be
        called only until one of them returns C false (a NULL pointer).

C Function: `void` **scm\_c\_hook\_init** `(scm_t_c_hook *hook, void *hook_data, scm_t_c_hook_type type)` [¶](#index-scm_005fc_005fhook_005finit)
:   Initialize the C hook at memory pointed to by hook. type
    should be one of the values of the `scm_t_c_hook_type` enumeration,
    and controls how the hook functions will be called. hook\_data is
    a closure parameter that will be passed to all registered hook functions
    when they are called.

To add or remove a C function from a C hook, use `scm_c_hook_add`
or `scm_c_hook_remove`. A hook function must expect three
`void *` parameters which are, respectively:

hook\_data
:   The hook closure data that was specified at the time the hook was
    initialized by `scm_c_hook_init`.

func\_data
:   The function closure data that was specified at the time that that
    function was registered with the hook by `scm_c_hook_add`.

data
:   The call closure data specified by the `scm_c_hook_run` call that
    runs the hook.

C Type: **scm\_t\_c\_hook\_function** [¶](#index-scm_005ft_005fc_005fhook_005ffunction)
:   Function type for a C hook function: takes three `void *`
    parameters and returns a `void *` result.

C Function: `void` **scm\_c\_hook\_add** `(scm_t_c_hook *hook, scm_t_c_hook_function func, void *func_data, int appendp)` [¶](#index-scm_005fc_005fhook_005fadd)
:   Add function func, with function closure data func\_data, to
    the C hook hook. The new function is appended to the hook’s list
    of functions if appendp is non-zero, otherwise prepended.

C Function: `void` **scm\_c\_hook\_remove** `(scm_t_c_hook *hook, scm_t_c_hook_function func, void *func_data)` [¶](#index-scm_005fc_005fhook_005fremove)
:   Remove function func, with function closure data func\_data,
    from the C hook hook. `scm_c_hook_remove` checks both
    func and func\_data so as to allow for the same func
    being registered multiple times with different closure data.

Finally, to invoke a C hook, call the `scm_c_hook_run` function
specifying the hook and the call closure data for this run:

C Function: `void *` **scm\_c\_hook\_run** `(scm_t_c_hook *hook, void *data)` [¶](#index-scm_005fc_005fhook_005frun)
:   Run the C hook hook will call closure data data. Subject to
    the variations for hook types `SCM_C_HOOK_OR` and
    `SCM_C_HOOK_AND`, `scm_c_hook_run` calls hook’s
    registered functions in turn, passing them the hook’s closure data, each
    function’s closure data, and the call closure data.

    `scm_c_hook_run`’s return value is the return value of the last
    function to be called.

---

Next: [Hooks into the Guile REPL](#REPL-Hooks), Previous: [Hooks For C Code.](#C-Hooks), Up: [Hooks](#Hooks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.6.4 Hooks for Garbage Collection [¶](#Hooks-for-Garbage-Collection)

Whenever Guile performs a garbage collection, it calls the following
hooks in the order shown.

C Hook: **scm\_before\_gc\_c\_hook** [¶](#index-scm_005fbefore_005fgc_005fc_005fhook)
:   C hook called at the very start of a garbage collection, after setting
    `scm_gc_running_p` to 1, but before entering the GC critical
    section.

    If garbage collection is blocked because `scm_block_gc` is
    non-zero, GC exits early soon after calling this hook, and no further
    hooks will be called.

C Hook: **scm\_before\_mark\_c\_hook** [¶](#index-scm_005fbefore_005fmark_005fc_005fhook)
:   C hook called before beginning the mark phase of garbage collection,
    after the GC thread has entered a critical section.

C Hook: **scm\_before\_sweep\_c\_hook** [¶](#index-scm_005fbefore_005fsweep_005fc_005fhook)
:   C hook called before beginning the sweep phase of garbage collection.
    This is the same as at the end of the mark phase, since nothing else
    happens between marking and sweeping.

C Hook: **scm\_after\_sweep\_c\_hook** [¶](#index-scm_005fafter_005fsweep_005fc_005fhook)
:   C hook called after the end of the sweep phase of garbage collection,
    but while the GC thread is still inside its critical section.

C Hook: **scm\_after\_gc\_c\_hook** [¶](#index-scm_005fafter_005fgc_005fc_005fhook)
:   C hook called at the very end of a garbage collection, after the GC
    thread has left its critical section.

Scheme Hook: **after-gc-hook** [¶](#index-after_002dgc_002dhook)
:   Scheme hook with arity 0. This hook is run asynchronously
    (see [Asynchronous Interrupts](#Asyncs)) soon after the GC has completed and any other events
    that were deferred during garbage collection have been processed. (Also
    accessible from C with the name `scm_after_gc_hook`.)

All the C hooks listed here have type `SCM_C_HOOK_NORMAL`, are
initialized with hook closure data NULL, are invoked by
`scm_c_hook_run` with call closure data NULL.

The Scheme hook `after-gc-hook` is particularly useful in
conjunction with guardians (see [Guardians](#Guardians)). Typically, if you are
using a guardian, you want to call the guardian after garbage collection
to see if any of the objects added to the guardian have been collected.
By adding a thunk that performs this call to `after-gc-hook`, you
can ensure that your guardian is tested after every garbage collection
cycle.

---

Previous: [Hooks for Garbage Collection](#GC-Hooks), Up: [Hooks](#Hooks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.9.6.5 Hooks into the Guile REPL [¶](#Hooks-into-the-Guile-REPL)

---

Next: [Controlling the Flow of Program Execution](#Control-Mechanisms), Previous: [General Utility Functions](#Utility-Functions), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.10 Definitions and Variable Bindings [¶](#Definitions-and-Variable-Bindings)

Scheme supports the definition of variables in different contexts.
Variables can be defined at the top level, so that they are visible in
the entire program, and variables can be defined locally to procedures
and expressions. This is important for modularity and data abstraction.

* [Top Level Variable Definitions](#Top-Level)
* [Local Variable Bindings](#Local-Bindings)
* [Internal definitions](#Internal-Definitions)
* [Querying variable bindings](#Binding-Reflection)
* [Binding multiple return values](#Binding-Multiple-Values)

---

Next: [Local Variable Bindings](#Local-Bindings), Up: [Definitions and Variable Bindings](#Binding-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.10.1 Top Level Variable Definitions [¶](#Top-Level-Variable-Definitions)

At the top level of a program (i.e., not nested within any other
expression), a definition of the form

```
(define a value)
```

defines a variable called `a` and sets it to the value value.

If the variable already exists in the current module, because it has
already been created by a previous `define` expression with the
same name, its value is simply changed to the new value. In this
case, then, the above form is completely equivalent to

```
(set! a value)
```

This equivalence means that `define` can be used interchangeably
with `set!` to change the value of variables at the top level of
the REPL or a Scheme source file. It is useful during interactive
development when reloading a Scheme file that you have modified, because
it allows the `define` expressions in that file to work as expected
both the first time that the file is loaded and on subsequent occasions.

Note, though, that `define` and `set!` are not always
equivalent. For example, a `set!` is not allowed if the named
variable does not already exist, and the two expressions can behave
differently in the case where there are imported variables visible from
another module.

Scheme Syntax: **define** name value [¶](#index-define)
:   Create a top level variable named name with value value.
    If the named variable already exists, just change its value. The return
    value of a `define` expression is unspecified.

The C API equivalents of `define` are `scm_define` and
`scm_c_define`, which differ from each other in whether the
variable name is specified as a `SCM` symbol or as a
null-terminated C string.

C Function: **scm\_define** (sym, value) [¶](#index-scm_005fdefine)

C Function: **scm\_c\_define** (const char \*name, value) [¶](#index-scm_005fc_005fdefine)
:   C equivalents of `define`, with variable name specified either by
    sym, a symbol, or by name, a null-terminated C string. Both
    variants return the new or preexisting variable object.

`define` (when it occurs at top level), `scm_define` and
`scm_c_define` all create or set the value of a variable in the top
level environment of the current module. If there was not already a
variable with the specified name belonging to the current module, but a
similarly named variable from another module was visible through having
been imported, the newly created variable in the current module will
shadow the imported variable, such that the imported variable is no
longer visible.

Attention: Scheme definitions inside local binding constructs
(see [Local Variable Bindings](#Local-Bindings)) act differently (see [Internal definitions](#Internal-Definitions)).

Many people end up in a development style of adding and changing
definitions at runtime, building out their program without restarting
it. (You can do this using `reload-module`, the `reload` REPL
command, the `load` procedure, or even just pasting code into a
REPL.) If you are one of these people, you will find that sometimes
there are some variables that you *don’t* want to redefine all the
time. For these, use `define-once`.

Scheme Syntax: **define-once** name value [¶](#index-define_002donce)
:   Create a top level variable named name with value value, but
    only if name is not already bound in the current module.

Old Lispers probably know `define-once` under its Lisp name,
`defvar`.

---

Next: [Internal definitions](#Internal-Definitions), Previous: [Top Level Variable Definitions](#Top-Level), Up: [Definitions and Variable Bindings](#Binding-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.10.2 Local Variable Bindings [¶](#Local-Variable-Bindings)

As opposed to definitions at the top level, which creates bindings that
are visible to all code in a module, it is also possible to define
variables which are only visible in a well-defined part of the program.
Normally, this part of a program will be a procedure or a subexpression
of a procedure.

With the constructs for local binding (`let`, `let*`,
`letrec`, and `letrec*`), the Scheme language has a block
structure like most other programming languages since the days of
ALGOL 60. Readers familiar to languages like C or Java should
already be used to this concept, but the family of `let`
expressions has a few properties which are well worth knowing.

The most basic local binding construct is `let`.

syntax: **let** bindings body [¶](#index-let)
:   bindings has the form

    ```
    ((variable1 init1) ...)
    ```

    that is zero or more two-element lists of a variable and an arbitrary
    expression each. All variable names must be distinct.

    body is a sequence of expressions and definitions, ending in an
    expression.

    A `let` expression is evaluated as follows.

    * All init expressions are evaluated.
    * New storage is allocated for the variables.
    * The values of the init expressions are stored into the variables.
    * The expressions and definitions in body are evaluated in order
      (see [Internal definitions](#Internal-Definitions)), and the values of the last expression
      are returned as the result of the `let` expression.

    The init expressions are not allowed to refer to any of the
    variables.

The other binding constructs are variations on the same theme: making new
values, binding them to variables, and executing a body in that new,
extended lexical context.

syntax: **let\*** bindings body [¶](#index-let_002a)
:   Similar to `let`, but the variable bindings are performed
    sequentially, that means that all init expression are allowed to
    use the variables defined on their left in the binding list.

    A `let*` expression can always be expressed with nested `let`
    expressions.

    ```
    (let* ((a 1) (b a))
       b)
    ≍
    (let ((a 1))
      (let ((b a))
        b))
    ```

syntax: **letrec** bindings body [¶](#index-letrec)
:   Similar to `let`, but it is possible to refer to the variable
    from lambda expression created in any of the inits. That is,
    procedures created in the init expression can recursively refer to
    the defined variables.

    ```
    (letrec ((even? (lambda (n)
                      (if (zero? n)
                          #t
                          (odd? (- n 1)))))
             (odd? (lambda (n)
                      (if (zero? n)
                          #f
                          (even? (- n 1))))))
      (even? 88))
    ⇒
    #t
    ```

    Note that while the init expressions may refer to the new
    variables, they may not access their values. For example, making the
    `even?` function above creates a closure (see [The Concept of Closure](#About-Closure))
    referencing the `odd?` variable. But `odd?` can’t be called
    until after execution has entered the body.

syntax: **letrec\*** bindings body [¶](#index-letrec_002a)
:   Similar to `letrec`, except the init expressions are bound to
    their variables in order.

    `letrec*` thus relaxes the letrec restriction, in that later
    init expressions may refer to the values of previously bound
    variables.

    ```
    (letrec ((a 42)
             (b (+ a 10)))  ;; Illegal access
      (* a b))
    ;; The behavior of the expression above is unspecified

    (letrec* ((a 42)
              (b (+ a 10)))
      (* a b))
    ⇒ 2184
    ```

There is also an alternative form of the `let` form, which is used
for expressing iteration. Because of the use as a looping construct,
this form (the *named let*) is documented in the section about
iteration (see [Iteration](#while-do))

---

Next: [Querying variable bindings](#Binding-Reflection), Previous: [Local Variable Bindings](#Local-Bindings), Up: [Definitions and Variable Bindings](#Binding-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.10.3 Internal definitions [¶](#Internal-definitions)

A `define` form which appears inside the body of a `lambda`,
`let`, `let*`, `letrec`, `letrec*` or equivalent
expression is called an *internal definition*. An internal
definition differs from a top level definition (see [Top Level Variable Definitions](#Top-Level)),
because the definition is only visible inside the complete body of the
enclosing form. Let us examine the following example.

```
(let ((frumble "froz"))
  (define banana (lambda () (apple 'peach)))
  (define apple (lambda (x) x))
  (banana))
⇒
peach
```

Here the enclosing form is a `let`, so the `define`s in the
`let`-body are internal definitions. Because the scope of the
internal definitions is the **complete** body of the
`let`-expression, the `lambda`-expression which gets bound to
the variable `banana` may refer to the variable `apple`, even
though its definition appears lexically *after* the definition of
`banana`. This is because a sequence of internal definition acts
as if it were a `letrec*` expression.

```
(let ()
  (define a 1)
  (define b 2)
  (+ a b))
```

is equivalent to

```
(let ()
  (letrec* ((a 1) (b 2))
    (+ a b)))
```

Internal definitions may be mixed with non-definition expressions. If
an expression precedes a definition, it is treated as if it were a
definition of an unreferenced variable. So this:

```
(let ()
  (define a 1)
  (foo)
  (define b 2)
  (+ a b))
```

is equivalent to

```
(let ()
  (letrec* ((a 1) (_ (begin (foo) #f)) (b 2))
    (+ a b)))
```

Another noteworthy difference to top level definitions is that within
one group of internal definitions all variable names must be distinct.
Whereas on the top level a second define for a given variable acts like
a `set!`, for internal definitions, duplicate bound identifiers
signals an error.

As a historical note, it used to be that internal bindings were expanded
in terms of `letrec`, not `letrec*`. This was the situation
for the R5RS report and before. However with the R6RS, it was recognized
that sequential definition was a more intuitive expansion, as in the
following case:

```
(let ()
  (define a 1)
  (define b (+ a a))
  (+ a b))
```

Guile decided to follow the R6RS in this regard, and now expands
internal definitions using `letrec*`. Relatedly, it used to be
that internal definitions had to precede all expressions in the body;
this restriction was relaxed in Guile 3.0.

---

Next: [Binding multiple return values](#Binding-Multiple-Values), Previous: [Internal definitions](#Internal-Definitions), Up: [Definitions and Variable Bindings](#Binding-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.10.4 Querying variable bindings [¶](#Querying-variable-bindings)

Guile provides a procedure for checking whether a symbol is bound in the
top level environment.

Scheme Procedure: **defined?** sym [module] [¶](#index-defined_003f)

C Function: **scm\_defined\_p** (sym, module) [¶](#index-scm_005fdefined_005fp)
:   Return `#t` if sym is defined in the module module or
    the current module when module is not specified; otherwise return
    `#f`.

---

Previous: [Querying variable bindings](#Binding-Reflection), Up: [Definitions and Variable Bindings](#Binding-Constructs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.10.5 Binding multiple return values [¶](#Binding-multiple-return-values)

Syntax: **define-values** formals expression [¶](#index-define_002dvalues)
:   The expression is evaluated, and the formals are bound to
    the return values in the same way that the formals in a `lambda`
    expression are matched to the arguments in a procedure call.

```
(define-values (q r) (floor/ 10 3))
(list q r) ⇒ (3 1)

(define-values (x . y) (values 1 2 3))
x ⇒ 1
y ⇒ (2 3)

(define-values x (values 1 2 3))
x ⇒ (1 2 3)
```

---

Next: [Input and Output](#Input-and-Output), Previous: [Definitions and Variable Bindings](#Binding-Constructs), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
