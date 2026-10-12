### 6.17 Memory Management and Garbage Collection [¶](#Memory-Management-and-Garbage-Collection)

Guile uses a *garbage collector* to manage most of its objects.
While the garbage collector is designed to be mostly invisible, you
sometimes need to interact with it explicitly.

See [Garbage Collection](#Garbage-Collection) for a general discussion of how garbage
collection relates to using Guile from C.

* [Function related to Garbage Collection](#Garbage-Collection-Functions)
* [Memory Blocks](#Memory-Blocks)
* [Weak References](#Weak-References)
* [Guardians](#Guardians)

---

Next: [Memory Blocks](#Memory-Blocks), Up: [Memory Management and Garbage Collection](#Memory-Management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.17.1 Function related to Garbage Collection [¶](#Function-related-to-Garbage-Collection)

Scheme Procedure: **gc** [¶](#index-gc-1)

C Function: **scm\_gc** () [¶](#index-scm_005fgc)
:   Finds all of the “live” `SCM` objects and reclaims for further
    use those that are no longer accessible. You normally don’t need to
    call this function explicitly. Its functionality is invoked
    automatically as needed.

C Function: `SCM` **scm\_gc\_protect\_object** `(SCM obj)` [¶](#index-scm_005fgc_005fprotect_005fobject)
:   Protects obj from being freed by the garbage collector, when it
    otherwise might be. When you are done with the object, call
    `scm_gc_unprotect_object` on the object. Calls to
    `scm_gc_protect_object`/`scm_gc_unprotect_object` can be nested, and
    the object remains protected until it has been unprotected as many times
    as it was protected. It is an error to unprotect an object more times
    than it has been protected. Returns the SCM object it was passed.

    Note that storing obj in a C global variable has the same
    effect[18](#FOOT18).

C Function: `SCM` **scm\_gc\_unprotect\_object** `(SCM obj)` [¶](#index-scm_005fgc_005funprotect_005fobject)
:   Unprotects an object from the garbage collector which was protected by
    `scm_gc_unprotect_object`. Returns the SCM object it was passed.

C Function: `SCM` **scm\_permanent\_object** `(SCM obj)` [¶](#index-scm_005fpermanent_005fobject)
:   Similar to `scm_gc_protect_object` in that it causes the
    collector to always mark the object, except that it should not be
    nested (only call `scm_permanent_object` on an object once), and
    it has no corresponding unpermanent function. Once an object is
    declared permanent, it will never be freed. Returns the SCM object it
    was passed.

C Macro: `void` **scm\_remember\_upto\_here\_1** `(SCM obj)` [¶](#index-scm_005fremember_005fupto_005fhere_005f1)

C Macro: `void` **scm\_remember\_upto\_here\_2** `(SCM obj1, SCM obj2)` [¶](#index-scm_005fremember_005fupto_005fhere_005f2)
:   Create a reference to the given object or objects, so they’re certain
    to be present on the stack or in a register and hence will not be
    freed by the garbage collector before this point.

    Note that these functions can only be applied to ordinary C local
    variables (ie. “automatics”). Objects held in global or static
    variables or some malloced block or the like cannot be protected with
    this mechanism.

Scheme Procedure: **gc-stats** [¶](#index-gc_002dstats)

C Function: **scm\_gc\_stats** () [¶](#index-scm_005fgc_005fstats)
:   Return an association list of statistics about Guile’s current
    use of storage.

Scheme Procedure: **gc-live-object-stats** [¶](#index-gc_002dlive_002dobject_002dstats)

C Function: **scm\_gc\_live\_object\_stats** () [¶](#index-scm_005fgc_005flive_005fobject_005fstats)
:   Return an alist of statistics of the current live objects.

Function: `void` **scm\_gc\_mark** `(SCM x)` [¶](#index-scm_005fgc_005fmark)
:   Mark the object x, and recurse on any objects x refers to.
    If x’s mark bit is already set, return immediately. This function
    must only be called during the mark-phase of garbage collection,
    typically from a smob *mark* function.

---

Next: [Weak References](#Weak-References), Previous: [Function related to Garbage Collection](#Garbage-Collection-Functions), Up: [Memory Management and Garbage Collection](#Memory-Management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.17.2 Memory Blocks [¶](#Memory-Blocks-1)

In C programs, dynamic management of memory blocks is normally done
with the functions malloc, realloc, and free. Guile has additional
functions for dynamic memory allocation that are integrated into the
garbage collector and the error reporting system.

Memory blocks that are associated with Scheme objects (for example a
foreign object) should be allocated with `scm_gc_malloc` or
`scm_gc_malloc_pointerless`. These two functions will either
return a valid pointer or signal an error. Memory blocks allocated this
way may be released explicitly; however, this is not strictly needed,
and we recommend *not* calling `scm_gc_free`. All memory
allocated with `scm_gc_malloc` or `scm_gc_malloc_pointerless`
is automatically reclaimed when the garbage collector no longer sees any
live reference to it[19](#FOOT19).

When garbage collection occurs, Guile will visit the words in memory
allocated with `scm_gc_malloc`, looking for live pointers. This
means that if `scm_gc_malloc`-allocated memory contains a pointer
to some other part of the memory, the garbage collector notices it and
prevents it from being reclaimed[20](#FOOT20). Conversely, memory allocated with
`scm_gc_malloc_pointerless` is assumed to be “pointer-less” and
is not scanned for pointers.

For memory that is not associated with a Scheme object, you can use
`scm_malloc` instead of `malloc`. Like
`scm_gc_malloc`, it will either return a valid pointer or signal
an error. However, it will not assume that the new memory block can
be freed by a garbage collection. The memory must be explicitly freed
with `free`.

There is also `scm_gc_realloc` and `scm_realloc`, to be used
in place of `realloc` when appropriate, and `scm_gc_calloc`
and `scm_calloc`, to be used in place of `calloc` when
appropriate.

The function `scm_dynwind_free` can be useful when memory should be
freed with libc’s `free` when leaving a dynwind context,
See [Dynamic Wind](#Dynamic-Wind).

C Function: `void *` **scm\_malloc** `(size_t size)` [¶](#index-scm_005fmalloc)

C Function: `void *` **scm\_calloc** `(size_t size)` [¶](#index-scm_005fcalloc)
:   Allocate size bytes of memory and return a pointer to it. When
    size is 0, return `NULL`. When not enough memory is
    available, signal an error. This function runs the GC to free up some
    memory when it deems it appropriate.

    The memory is allocated by the libc `malloc` function and can be
    freed with `free`. There is no `scm_free` function to go
    with `scm_malloc` to make it easier to pass memory back and forth
    between different modules.

    The function `scm_calloc` is similar to `scm_malloc`, but
    initializes the block of memory to zero as well.

    These functions will (indirectly) call
    `scm_gc_register_allocation`.

C Function: `void *` **scm\_realloc** `(void *mem, size_t new_size)` [¶](#index-scm_005frealloc)
:   Change the size of the memory block at mem to new\_size and
    return its new location. When new\_size is 0, this is the same
    as calling `free` on mem and `NULL` is returned. When
    mem is `NULL`, this function behaves like `scm_malloc`
    and allocates a new block of size new\_size.

    When not enough memory is available, signal an error. This function
    runs the GC to free up some memory when it deems it appropriate.

    This function will call `scm_gc_register_allocation`.

C Function: `void *` **scm\_gc\_malloc** `(size_t size, const char *what)` [¶](#index-scm_005fgc_005fmalloc)

C Function: `void *` **scm\_gc\_malloc\_pointerless** `(size_t size, const char *what)` [¶](#index-scm_005fgc_005fmalloc_005fpointerless)

C Function: `void *` **scm\_gc\_realloc** `(void *mem, size_t old_size, size_t new_size, const char *what);` [¶](#index-scm_005fgc_005frealloc)

C Function: `void *` **scm\_gc\_calloc** `(size_t size, const char *what)` [¶](#index-scm_005fgc_005fcalloc)
:   Allocate size bytes of automatically-managed memory. The memory
    is automatically freed when no longer referenced from any live memory
    block.

    When garbage collection occurs, Guile will visit the words in memory
    allocated with `scm_gc_malloc` or `scm_gc_calloc`, looking for
    pointers to other memory allocations that are managed by the GC. In
    contrast, memory allocated by `scm_gc_malloc_pointerless` is not
    scanned for pointers.

    The `scm_gc_realloc` call preserves the “pointerlessness” of the
    memory area pointed to by mem. Note that you need to pass the old
    size of a reallocated memory block as well. See below for a motivation.

C Function: `void` **scm\_gc\_free** `(void *mem, size_t size, const char *what)` [¶](#index-scm_005fgc_005ffree)
:   Explicitly free the memory block pointed to by mem, which was
    previously allocated by one of the above `scm_gc` functions. This
    function is almost always unnecessary, except for codebases that still
    need to compile on Guile 1.8.

    Note that you need to explicitly pass the size parameter. This
    is done since it should normally be easy to provide this parameter
    (for memory that is associated with GC controlled objects) and help keep
    the memory management overhead very low. However, in Guile 2.x,
    size is always ignored.

C Function: `void` **scm\_gc\_register\_allocation** `(size_t size)` [¶](#index-scm_005fgc_005fregister_005fallocation)
:   Informs the garbage collector that size bytes have been allocated,
    which the collector would otherwise not have known about.

    In general, Scheme will decide to collect garbage only after some amount
    of memory has been allocated. Calling this function will make the
    Scheme garbage collector know about more allocation, and thus run more
    often (as appropriate).

    It is especially important to call this function when large unmanaged
    allocations, like images, may be freed by small Scheme allocations, like
    foreign objects.

C Function: `void` **scm\_dynwind\_free** `(void *mem)` [¶](#index-scm_005fdynwind_005ffree-1)
:   Equivalent to `scm_dynwind_unwind_handler (free, mem,
    SCM_F_WIND_EXPLICITLY)`. That is, the memory block at mem will be
    freed (using `free` from the C library) when the current dynwind is
    left.

Scheme Procedure: **malloc-stats** [¶](#index-malloc_002dstats)
:   Return an alist ((what . n) ...) describing number
    of malloced objects.
    what is the second argument to `scm_gc_malloc`,
    n is the number of objects of that type currently
    allocated.

    This function is only available if the `GUILE_DEBUG_MALLOC`
    preprocessor macro was defined when Guile was compiled.

---

Next: [Guardians](#Guardians), Previous: [Memory Blocks](#Memory-Blocks), Up: [Memory Management and Garbage Collection](#Memory-Management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.17.3 Weak References [¶](#Weak-References-1)

[FIXME: This chapter is based on Mikael Djurfeldt’s answer to a
question by Michael Livshin. Any mistakes are not theirs, of course. ]

Weak references let you attach bookkeeping information to data so that
the additional information automatically disappears when the original
data is no longer in use and gets garbage collected. In a weak key hash,
the hash entry for that key disappears as soon as the key is no longer
referenced from anywhere else. For weak value hashes, the same happens
as soon as the value is no longer in use. Entries in a doubly weak hash
disappear when either the key or the value are not used anywhere else
anymore.

Object properties offer the same kind of functionality as weak key
hashes in many situations. (see [Object Properties](#Object-Properties))

Here’s an example (a little bit strained perhaps, but one of the
examples is actually used in Guile):

Assume that you’re implementing a debugging system where you want to
associate information about filename and position of source code
expressions with the expressions themselves.

Hashtables can be used for that, but if you use ordinary hash tables
it will be impossible for the scheme interpreter to "forget" old
source when, for example, a file is reloaded.

To implement the mapping from source code expressions to positional
information it is necessary to use weak-key tables since we don’t want
the expressions to be remembered just because they are in our table.

To implement a mapping from source file line numbers to source code
expressions you would use a weak-value table.

To implement a mapping from source code expressions to the procedures
they constitute a doubly-weak table has to be used.

* [Weak hash tables](#Weak-hash-tables)
* [Weak vectors](#Weak-vectors)

---

Next: [Weak vectors](#Weak-vectors), Up: [Weak References](#Weak-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.17.3.1 Weak hash tables [¶](#Weak-hash-tables-1)

Scheme Procedure: **make-weak-key-hash-table** [size] [¶](#index-make_002dweak_002dkey_002dhash_002dtable)

Scheme Procedure: **make-weak-value-hash-table** [size] [¶](#index-make_002dweak_002dvalue_002dhash_002dtable)

Scheme Procedure: **make-doubly-weak-hash-table** [size] [¶](#index-make_002ddoubly_002dweak_002dhash_002dtable)

C Function: **scm\_make\_weak\_key\_hash\_table** (size) [¶](#index-scm_005fmake_005fweak_005fkey_005fhash_005ftable)

C Function: **scm\_make\_weak\_value\_hash\_table** (size) [¶](#index-scm_005fmake_005fweak_005fvalue_005fhash_005ftable)

C Function: **scm\_make\_doubly\_weak\_hash\_table** (size) [¶](#index-scm_005fmake_005fdoubly_005fweak_005fhash_005ftable)
:   Return a weak hash table with size buckets. As with any
    hash table, choosing a good size for the table requires some
    caution.

    You can modify weak hash tables in exactly the same way you would modify
    regular hash tables, with the exception of the routines that act on
    handles. Weak tables have a different implementation behind the scenes
    that doesn’t have handles. see [Hash Tables](#Hash-Tables), for more on
    `hashq-ref` et al.

Note that in a weak-key hash table, the reference to the value is
strong. This means that if the value references the key, even
indirectly, the key will never be collected, which can lead to a memory
leak. The reverse is true for weak value tables.

Scheme Procedure: **weak-key-hash-table?** obj [¶](#index-weak_002dkey_002dhash_002dtable_003f)

Scheme Procedure: **weak-value-hash-table?** obj [¶](#index-weak_002dvalue_002dhash_002dtable_003f)

Scheme Procedure: **doubly-weak-hash-table?** obj [¶](#index-doubly_002dweak_002dhash_002dtable_003f)

C Function: **scm\_weak\_key\_hash\_table\_p** (obj) [¶](#index-scm_005fweak_005fkey_005fhash_005ftable_005fp)

C Function: **scm\_weak\_value\_hash\_table\_p** (obj) [¶](#index-scm_005fweak_005fvalue_005fhash_005ftable_005fp)

C Function: **scm\_doubly\_weak\_hash\_table\_p** (obj) [¶](#index-scm_005fdoubly_005fweak_005fhash_005ftable_005fp)
:   Return `#t` if obj is the specified weak hash
    table. Note that a doubly weak hash table is neither a weak key
    nor a weak value hash table.

---

Previous: [Weak hash tables](#Weak-hash-tables), Up: [Weak References](#Weak-References)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.17.3.2 Weak vectors [¶](#Weak-vectors-1)

Scheme Procedure: **make-weak-vector** size [fill] [¶](#index-make_002dweak_002dvector)

C Function: **scm\_make\_weak\_vector** (size, fill) [¶](#index-scm_005fmake_005fweak_005fvector)
:   Return a weak vector with size elements. If the optional
    argument fill is given, all entries in the vector will be
    set to fill. The default value for fill is the
    empty list.

Scheme Procedure: **weak-vector** elem … [¶](#index-weak_002dvector)

Scheme Procedure: **list->weak-vector** l [¶](#index-list_002d_003eweak_002dvector)

C Function: **scm\_weak\_vector** (l) [¶](#index-scm_005fweak_005fvector)
:   Construct a weak vector from a list: `weak-vector` uses
    the list of its arguments while `list->weak-vector` uses
    its only argument l (a list) to construct a weak vector
    the same way `list->vector` would.

Scheme Procedure: **weak-vector?** obj [¶](#index-weak_002dvector_003f)

C Function: **scm\_weak\_vector\_p** (obj) [¶](#index-scm_005fweak_005fvector_005fp)
:   Return `#t` if obj is a weak vector.

Scheme Procedure: **weak-vector-ref** wvect k [¶](#index-weak_002dvector_002dref)

C Function: **scm\_weak\_vector\_ref** (wvect, k) [¶](#index-scm_005fweak_005fvector_005fref)
:   Return the kth element of the weak vector wvect, or
    `#f` if that element has been collected.

Scheme Procedure: **weak-vector-set!** wvect k elt [¶](#index-weak_002dvector_002dset_0021)

C Function: **scm\_weak\_vector\_set\_x** (wvect, k, elt) [¶](#index-scm_005fweak_005fvector_005fset_005fx)
:   Set the kth element of the weak vector wvect to elt.

---

Previous: [Weak References](#Weak-References), Up: [Memory Management and Garbage Collection](#Memory-Management)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.17.4 Guardians [¶](#Guardians-1)

Guardians provide a way to be notified about objects that would
otherwise be collected as garbage. Guarding them prevents the objects
from being collected and cleanup actions can be performed on them, for
example.

See R. Kent Dybvig, Carl Bruggeman, and David Eby (1993) "Guardians in
a Generation-Based Garbage Collector". ACM SIGPLAN Conference on
Programming Language Design and Implementation, June 1993.

Scheme Procedure: **make-guardian** [¶](#index-make_002dguardian)

C Function: **scm\_make\_guardian** () [¶](#index-scm_005fmake_005fguardian)
:   Create a new guardian. A guardian protects a set of objects from
    garbage collection, allowing a program to apply cleanup or other
    actions.

    `make-guardian` returns a procedure representing the guardian.
    Calling the guardian procedure with an argument adds the argument to
    the guardian’s set of protected objects. Calling the guardian
    procedure without an argument returns one of the protected objects
    which are ready for garbage collection, or `#f` if no such object
    is available. Objects which are returned in this way are removed from
    the guardian.

    You can put a single object into a guardian more than once and you can
    put a single object into more than one guardian. The object will then
    be returned multiple times by the guardian procedures.

    An object is eligible to be returned from a guardian when it is no
    longer referenced from outside any guardian.

    There is no guarantee about the order in which objects are returned
    from a guardian. If you want to impose an order on finalization
    actions, for example, you can do that by keeping objects alive in some
    global data structure until they are no longer needed for finalizing
    other objects.

    Being an element in a weak vector, a key in a hash table with weak
    keys, or a value in a hash table with weak values does not prevent an
    object from being returned by a guardian. But as long as an object
    can be returned from a guardian it will not be removed from such a
    weak vector or hash table. In other words, a weak link does not
    prevent an object from being considered collectible, but being inside
    a guardian prevents a weak link from being broken.

    A key in a weak key hash table can be thought of as having a strong
    reference to its associated value as long as the key is accessible.
    Consequently, when the key is only accessible from within a guardian,
    the reference from the key to the value is also considered to be
    coming from within a guardian. Thus, if there is no other reference
    to the value, it is eligible to be returned from a guardian.

---

Next: [Foreign Function Interface](#Foreign-Function-Interface), Previous: [Memory Management and Garbage Collection](#Memory-Management), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.18 Modules [¶](#Modules-1)

When programs become large, naming conflicts can occur when a function
or global variable defined in one file has the same name as a function
or global variable in another file. Even just a *similarity*
between function names can cause hard-to-find bugs, since a programmer
might type the wrong function name.

The approach used to tackle this problem is called *information
encapsulation*, which consists of packaging functional units into a
given name space that is clearly separated from other name spaces.

The language features that allow this are usually called *the
module system* because programs are broken up into modules that are
compiled separately (or loaded separately in an interpreter).

Older languages, like C, have limited support for name space
manipulation and protection. In C a variable or function is public by
default, and can be made local to a module with the `static`
keyword. But you cannot reference public variables and functions from
another module with different names.

More advanced module systems have become a common feature in recently
designed languages: ML, Python, Perl, and Modula 3 all allow the
*renaming* of objects from a foreign module, so they will not
clutter the global name space.

In addition, Guile offers variables as first-class objects. They can
be used for interacting with the module system.

* [General Information about Modules](#General-Information-about-Modules)
* [Using Guile Modules](#Using-Guile-Modules)
* [Creating Guile Modules](#Creating-Guile-Modules)
* [Modules and the File System](#Modules-and-the-File-System)
* [R6RS Version References](#R6RS-Version-References)
* [R6RS Libraries](#R6RS-Libraries)
* [Variables](#Variables)
* [Module System Reflection](#Module-System-Reflection)
* [Declarative Modules](#Declarative-Modules)
* [Accessing Modules from C](#Accessing-Modules-from-C)
* [provide and require](#provide-and-require)
* [Environments](#Environments)

---

Next: [Using Guile Modules](#Using-Guile-Modules), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.1 General Information about Modules [¶](#General-Information-about-Modules-1)

A Guile module can be thought of as a collection of named procedures,
variables and macros. More precisely, it is a set of *bindings*
of symbols (names) to Scheme objects.

Within a module, all bindings are visible. Certain bindings
can be declared *public*, in which case they are added to the
module’s so-called *export list*; this set of public bindings is
called the module’s *public interface* (see [Creating Guile Modules](#Creating-Guile-Modules)).

A client module *uses* a providing module’s bindings by either
accessing the providing module’s public interface, or by building a
custom interface (and then accessing that). In a custom interface, the
client module can *select* which bindings to access and can also
algorithmically *rename* bindings. In contrast, when using the
providing module’s public interface, the entire export list is available
without renaming (see [Using Guile Modules](#Using-Guile-Modules)).

All Guile modules have a unique *module name*, for example
`(ice-9 popen)` or `(srfi srfi-11)`. Module names are lists
of one or more symbols.

When Guile goes to use an interface from a module, for example
`(ice-9 popen)`, Guile first looks to see if it has loaded
`(ice-9 popen)` for any reason. If the module has not been loaded
yet, Guile searches a *load path* for a file that might define it,
and loads that file.

The following subsections go into more detail on using, creating,
installing, and otherwise manipulating modules and the module system.

---

Next: [Creating Guile Modules](#Creating-Guile-Modules), Previous: [General Information about Modules](#General-Information-about-Modules), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.2 Using Guile Modules [¶](#Using-Guile-Modules-1)

To use a Guile module is to access either its public interface or a
custom interface (see [General Information about Modules](#General-Information-about-Modules)). Both
types of access are handled by the syntactic form `use-modules`,
which accepts one or more interface specifications and, upon evaluation,
arranges for those interfaces to be available to the current module.
This process may include locating and loading code for a given module if
that code has not yet been loaded, following `%load-path`
(see [Modules and the File System](#Modules-and-the-File-System)).

An *interface specification* has one of two forms. The first
variation is simply to name the module, in which case its public
interface is the one accessed. For example:

```
(use-modules (ice-9 popen))
```

Here, the interface specification is `(ice-9 popen)`, and the
result is that the current module now has access to `open-pipe`,
`close-pipe`, `open-input-pipe`, and so on (see [Pipes](#Pipes)).

Note in the previous example that if the current module had already
defined `open-pipe`, that definition would be overwritten by the
definition in `(ice-9 popen)`. For this reason (and others), there
is a second variation of interface specification that not only names a
module to be accessed, but also selects bindings from it and renames
them to suit the current module’s needs. For example:

```
(use-modules ((ice-9 popen)
              #:select ((open-pipe . pipe-open) close-pipe)
              #:renamer (symbol-prefix-proc 'unixy:)))
```

or more simply:

```
(use-modules ((ice-9 popen)
              #:select ((open-pipe . pipe-open) close-pipe)
              #:prefix unixy:))
```

Here, the interface specification is more complex than before, and the
result is that a custom interface with only two bindings is created and
subsequently accessed by the current module. The mapping of old to new
names is as follows:

```
(ice-9 popen) sees:             current module sees:
open-pipe                       unixy:pipe-open
close-pipe                      unixy:close-pipe
```

This example also shows how to use the convenience procedure
`symbol-prefix-proc`.

You can also directly refer to bindings in a module by using the
`@` syntax. For example, instead of using the
`use-modules` statement from above and writing
`unixy:pipe-open` to refer to the `pipe-open` from the
`(ice-9 popen)`, you could also write `(@ (ice-9 popen)
open-pipe)`. Thus an alternative to the complete `use-modules`
statement would be

```
(define unixy:pipe-open (@ (ice-9 popen) open-pipe))
(define unixy:close-pipe (@ (ice-9 popen) close-pipe))
```

There is also `@@`, which can be used like `@`, but does
not check whether the variable that is being accessed is actually
exported. Thus, `@@` can be thought of as the impolite version
of `@` and should only be used as a last resort or for
debugging, for example.

Note that just as with a `use-modules` statement, any module that
has not yet been loaded will be loaded when referenced by a `@` or
`@@` form.

You can also use the `@` and `@@` syntaxes as the target
of a `set!` when the binding refers to a variable.

Scheme Procedure: **symbol-prefix-proc** prefix-sym [¶](#index-symbol_002dprefix_002dproc)
:   Return a procedure that prefixes its arg (a symbol) with
    prefix-sym.

syntax: **use-modules** spec … [¶](#index-use_002dmodules)
:   Resolve each interface specification spec into an interface and
    arrange for these to be accessible by the current module. The return
    value is unspecified.

    spec can be a list of symbols, in which case it names a module
    whose public interface is found and used.

    spec can also be of the form:

    ```
     (MODULE-NAME [#:select SELECTION]
                  [#:hide HIDE]
                  [#:prefix PREFIX]
                  [#:renamer RENAMER])
    ```

    in which case a custom interface is newly created and used.
    module-name is a list of symbols, as above; selection is a
    list of selection-specs; hide is a list of bindings which should
    not be imported; prefix is a symbol that is prepended to imported
    names; and renamer is a procedure that takes a symbol and returns
    its new name. A selection-spec is either a symbol or a pair of symbols
    `(ORIG . SEEN)`, where orig is the name in the used module
    and seen is the name in the using module. Note that seen is
    also modified by prefix and renamer.

    The `#:select`, `#:hide`, `#:prefix`, and
    `#:renamer` clauses are optional. If all are omitted, this form
    behaves identically to the previous one. If the `#:select` clause
    is omitted, prefix and renamer operate on the used module’s
    public interface.

    The `#:hide` operates on list of bindings in the module being
    imported, before any renaming is performed. If both `#:select` and
    `#:hide` contain a binding, the `#:hide` wins.

    In addition to the above, spec can also include a `#:version`
    clause, of the form:

    ```
     #:version VERSION-SPEC
    ```

    where version-spec is an R6RS-compatible version reference. An
    error will be signaled in the case in which a module with the same name
    has already been loaded, if that module specifies a version and that
    version is not compatible with version-spec. See [R6RS Version References](#R6RS-Version-References), for more on version references.

    If the module name is not resolvable, `use-modules` will signal an
    error.

    See also `scm_c_use_module` for the C API.

syntax: **@** module-name binding-name [¶](#index-_0040)
:   Refer to the binding named binding-name in module
    module-name. The binding must have been exported by the module.

syntax: **@@** module-name binding-name [¶](#index-_0040_0040)
:   Refer to the binding named binding-name in module
    module-name. The binding must not have been exported by the
    module. This syntax is only intended for debugging purposes or as a
    last resort. See [Declarative Modules](#Declarative-Modules), for some limitations on the
    use of `@@`.

---

Next: [Modules and the File System](#Modules-and-the-File-System), Previous: [Using Guile Modules](#Using-Guile-Modules), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.3 Creating Guile Modules [¶](#Creating-Guile-Modules-1)

When you want to create your own modules, you have to take the following
steps:

* Create a Scheme source file and add all variables and procedures you wish
  to export, or which are required by the exported procedures.
* Add a `define-module` form at the beginning.
* Export all bindings which should be in the public interface, either
  by using `define-public` or `export` (both documented below).

syntax: **define-module** module-name option … [¶](#index-define_002dmodule)
:   module-name is a list of one or more symbols.

    ```
    (define-module (ice-9 popen))
    ```

    `define-module` makes this module available to Guile programs under
    the given module-name.

    option … are keyword/value pairs which specify more about the
    defined module. The recognized options and their meaning are shown in
    the following table.

    `#:use-module interface-specification`
    :   Equivalent to a `(use-modules interface-specification)`
        (see [Using Guile Modules](#Using-Guile-Modules)).

    `#:autoload module symbol-list` [¶](#index-autoload)
    :   Load module when any of symbol-list are accessed. For
        example,

        ```
        (define-module (my mod)
          #:autoload (srfi srfi-1) (partition delete-duplicates))
        ...
        (when something
          (set! foo (delete-duplicates ...)))
        ```

        When a module is autoloaded, only the bindings in symbol-list
        become available[21](#FOOT21).

        An autoload is a good way to put off loading a big module until it’s
        really needed, for instance for faster startup or if it will only be
        needed in certain circumstances.

    `#:export list` [¶](#index-export)
    :   Export all identifiers in list which must be a list of symbols
        or pairs of symbols. This is equivalent to `(export list)`
        in the module body.

    `#:re-export list` [¶](#index-re_002dexport)
    :   Re-export all identifiers in list which must be a list of
        symbols or pairs of symbols. The symbols in list must be
        imported by the current module from other modules. This is equivalent
        to `re-export` below.

    `#:replace list` [¶](#index-replace)
    :   Export all identifiers in list (a list of symbols or pairs of
        symbols) and mark them as *replacing bindings*. In the module
        user’s name space, this will have the effect of replacing any binding
        with the same name that is not also “replacing”. Normally a
        replacement results in an “override” warning message,
        `#:replace` avoids that.

        In general, a module that exports a binding for which the `(guile)`
        module already has a definition should use `#:replace` instead of
        `#:export`. `#:replace`, in a sense, lets Guile know that the
        module *purposefully* replaces a core binding. It is important to
        note, however, that this binding replacement is confined to the name
        space of the module user. In other words, the value of the core binding
        in question remains unchanged for other modules.

        Note that although it is often a good idea for the replaced binding to
        remain compatible with a binding in `(guile)`, to avoid surprising
        the user, sometimes the bindings will be incompatible. For example,
        SRFI-19 exports its own version of `current-time` (see [SRFI-19 Time](#SRFI_002d19-Time)) which is not compatible with the core `current-time`
        function (see [Time](#Time)). Guile assumes that a user importing a module
        knows what she is doing, and uses `#:replace` for this binding
        rather than `#:export`.

        A `#:replace` clause is equivalent to `(export! list)`
        in the module body.

        The `#:duplicates` (see below) provides fine-grain control about
        duplicate binding handling on the module-user side.

    `#:re-export-and-replace list` [¶](#index-re_002dexport_002dand_002dreplace)
    :   Like `#:re-export`, but also marking the bindings as replacements
        in the sense of `#:replace`.

    `#:version list` [¶](#index-module-version)
    :   Specify a version for the module in the form of list, a list of
        zero or more exact, non-negative integers. The corresponding
        `#:version` option in the `use-modules` form allows callers
        to restrict the value of this option in various ways.

    `#:duplicates list` [¶](#index-duplicate-binding-handlers)
    :   Tell Guile to handle duplicate bindings for the bindings imported by
        the current module according to the policy defined by list, a
        list of symbols. list must contain symbols representing a
        duplicate binding handling policy chosen among the following:

        `check`
        :   Raises an error when a binding is imported from more than one place.

        `warn`
        :   Issue a warning when a binding is imported from more than one place
            and leave the responsibility of actually handling the duplication to
            the next duplicate binding handler.

        `replace`
        :   When a new binding is imported that has the same name as a previously
            imported binding, then do the following:

            1. If the old binding was said to be *replacing* (via the
               `#:replace` option above) and the new binding is not replacing,
               the keep the old binding.
            2. If the old binding was not said to be replacing and the new binding is
               replacing, then replace the old binding with the new one.
            3. If neither the old nor the new binding is replacing, then keep the old
               one.

        `warn-override-core`
        :   Issue a warning when a core binding is being overwritten and actually
            override the core binding with the new one.

        `first`
        :   In case of duplicate bindings, the firstly imported binding is always
            the one which is kept.

        `last`
        :   In case of duplicate bindings, the lastly imported binding is always
            the one which is kept.

        `noop`
        :   In case of duplicate bindings, leave the responsibility to the next
            duplicate handler.

        If list contains more than one symbol, then the duplicate
        binding handlers which appear first will be used first when resolving
        a duplicate binding situation. As mentioned above, some resolution
        policies may explicitly leave the responsibility of handling the
        duplication to the next handler in list.

        If GOOPS has been loaded before the `#:duplicates` clause is
        processed, there are additional strategies available for dealing with
        generic functions. See [Merging Generics](#Merging-Generics), for more information.

        The default duplicate binding resolution policy is given by the
        `default-duplicate-binding-handler` procedure, and is

        ```
        (replace warn-override-core warn last)
        ```

    `#:pure` [¶](#index-pure-module)
    :   Create a *pure* module, that is a module which does not contain any
        of the standard procedure bindings except for the syntax forms. This is
        useful if you want to create *safe* modules, that is modules which
        do not know anything about dangerous procedures.

syntax: **export** variable … [¶](#index-export-1)
:   Add all variables (which must be symbols or pairs of symbols) to
    the list of exported bindings of the current module. If variable
    is a pair, its `car` gives the name of the variable as seen by the
    current module and its `cdr` specifies a name for the binding in
    the current module’s public interface.

syntax: **define-public** … [¶](#index-define_002dpublic)
:   Equivalent to `(begin (define foo ...) (export foo))`.

syntax: **re-export** variable … [¶](#index-re_002dexport-1)
:   Add all variables (which must be symbols or pairs of symbols) to
    the list of re-exported bindings of the current module. Pairs of
    symbols are handled as in `export`. Re-exported bindings must be
    imported by the current module from some other module.

syntax: **export!** variable … [¶](#index-export_0021)
:   Like `export`, but marking the exported variables as replacing.
    Using a module with replacing bindings will cause any existing bindings
    to be replaced without issuing any warnings. See the discussion of
    `#:replace` above.

---

Next: [R6RS Version References](#R6RS-Version-References), Previous: [Creating Guile Modules](#Creating-Guile-Modules), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.4 Modules and the File System [¶](#Modules-and-the-File-System-1)

Typical programs only use a small subset of modules installed on a Guile
system. In order to keep startup time down, Guile only loads modules
when a program uses them, on demand.

When a program evaluates `(use-modules (ice-9 popen))`, and the
module is not loaded, Guile searches for a conventionally-named file
in the *load path*.

In this case, loading `(ice-9 popen)` will eventually cause Guile
to run `(primitive-load-path "ice-9/popen")`.
`primitive-load-path` will search for a file `ice-9/popen` in
the `%load-path` (see [Load Paths](#Load-Paths)). For each directory in
`%load-path`, Guile will try to find the file name, concatenated
with the extensions from `%load-extensions`. By default, this will
cause Guile to `stat` `ice-9/popen.scm`, and then
`ice-9/popen`. See [Load Paths](#Load-Paths), for more on
`primitive-load-path`.

If a corresponding compiled `.go` file is found in the
`%load-compiled-path` or in the fallback path, and is as fresh as
the source file, it will be loaded instead of the source file. If no
compiled file is found, Guile may try to compile the source file and
cache away the resulting `.go` file. See [Compiling Scheme Code](#Compilation), for more
on compilation.

Once Guile finds a suitable source or compiled file is found, the file
will be loaded. If, after loading the file, the module under
consideration is still not defined, Guile will signal an error.

For more information on where and how to install Scheme modules,
See [Installing Site Packages](#Installing-Site-Packages).

---

Next: [R6RS Libraries](#R6RS-Libraries), Previous: [Modules and the File System](#Modules-and-the-File-System), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.5 R6RS Version References [¶](#R6RS-Version-References-1)

Guile’s module system includes support for locating modules based on
a declared version specifier of the same form as the one described in
R6RS (see [R6RS Library Form](r6rs.html#Library-form) in The Revised^6
Report on the Algorithmic Language Scheme). By using the
`#:version` keyword in a `define-module` form, a module may
specify a version as a list of zero or more exact, non-negative integers.

This version can then be used to locate the module during the module
search process. Client modules and callers of the `use-modules`
function may specify constraints on the versions of target modules by
providing a *version reference*, which has one of the following
forms:

```
 (sub-version-reference ...)
 (and version-reference ...)
 (or version-reference ...)
 (not version-reference)
```

in which sub-version-reference is in turn one of:

```
 (sub-version)
 (>= sub-version)
 (<= sub-version)
 (and sub-version-reference ...)
 (or sub-version-reference ...)
 (not sub-version-reference)
```

in which sub-version is an exact, non-negative integer as above. A
version reference matches a declared module version if each element of
the version reference matches a corresponding element of the module
version, according to the following rules:

* The `and` sub-form matches a version or version element if every
  element in the tail of the sub-form matches the specified version or
  version element.
* The `or` sub-form matches a version or version element if any
  element in the tail of the sub-form matches the specified version or
  version element.
* The `not` sub-form matches a version or version element if the tail
  of the sub-form does not match the version or version element.
* The `>=` sub-form matches a version element if the element is
  greater than or equal to the sub-version in the tail of the
  sub-form.
* The `<=` sub-form matches a version element if the version is less
  than or equal to the sub-version in the tail of the sub-form.
* A sub-version matches a version element if one is eqv? to
  the other.

For example, a module declared as:

```
 (define-module (mylib mymodule) #:version (1 2 0))
```

would be successfully loaded by any of the following `use-modules`
expressions:

```
 (use-modules ((mylib mymodule) #:version (1 2 (>= 0))))
 (use-modules ((mylib mymodule) #:version (or (1 2 0) (1 2 1))))
 (use-modules ((mylib mymodule) #:version ((and (>= 1) (not 2)) 2 0)))
```

---

Next: [Variables](#Variables), Previous: [R6RS Version References](#R6RS-Version-References), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.6 R6RS Libraries [¶](#R6RS-Libraries-1)

In addition to the API described in the previous sections, you also
have the option to create modules using the portable `library` form
described in R6RS (see [R6RS Library Form](r6rs.html#Library-form) in The
Revised^6 Report on the Algorithmic Language Scheme), and to import
libraries created in this format by other programmers. Guile’s R6RS
library implementation takes advantage of the flexibility built into the
module system by expanding the R6RS library form into a corresponding
Guile `define-module` form that specifies equivalent import and
export requirements and includes the same body expressions. The library
expression:

```
  (library (mylib (1 2))
    (export mybinding)
    (import (otherlib (3))))
```

is equivalent to the module definition:

```
  (define-module (mylib)
    #:version (1 2)
    #:use-module ((otherlib) #:version (3))
    #:export (mybinding))
```

Central to the mechanics of R6RS libraries is the concept of import
and export *levels*, which control the visibility of bindings at
various phases of a library’s lifecycle — macros necessary to
expand forms in the library’s body need to be available at expand
time; variables used in the body of a procedure exported by the
library must be available at runtime. R6RS specifies the optional
`for` sub-form of an *import set* specification (see below)
as a mechanism by which a library author can indicate that a
particular library import should take place at a particular phase
with respect to the lifecycle of the importing library.

Guile’s library implementation uses a technique called
*implicit phasing* (first described by Abdulaziz Ghuloum and R.
Kent Dybvig), which allows the expander and compiler to automatically
determine the necessary visibility of a binding imported from another
library. As such, the `for` sub-form described below is ignored by
Guile (but may be required by Schemes in which phasing is explicit).

Scheme Syntax: **library** name (export export-spec ...) (import import-spec ...) body ... [¶](#index-library)
:   Defines a new library with the specified name, exports, and imports,
    and evaluates the specified body expressions in this library’s
    environment.

    The library name is a non-empty list of identifiers, optionally
    ending with a version specification of the form described above
    (see [Creating Guile Modules](#Creating-Guile-Modules)).

    Each export-spec is the name of a variable defined or imported
    by the library, or must take the form
    `(rename (internal-name external-name) ...)`, where the
    identifier internal-name names a variable defined or imported
    by the library and external-name is the name by which the
    variable is seen by importing libraries.

    Each import-spec must be either an *import set* (see below)
    or must be of the form `(for import-set import-level ...)`,
    where each import-level is one of:

    ```
      run
      expand
      (meta level)
    ```

    where level is an integer. Note that since Guile does not
    require explicit phase specification, any import-sets found
    inside of `for` sub-forms will be “unwrapped” during
    expansion and processed as if they had been specified directly.

    Import sets in turn take one of the following forms:

    ```
      library-reference
      (library library-reference)
      (only import-set identifier ...)
      (except import-set identifier ...)
      (prefix import-set identifier)
      (rename import-set (internal-identifier external-identifier) ...)
    ```

    where library-reference is a non-empty list of identifiers
    ending with an optional version reference (see [R6RS Version References](#R6RS-Version-References)), and the other sub-forms have the following semantics,
    defined recursively on nested import-sets:

    * The `library` sub-form is used to specify libraries for import
      whose names begin with the identifier “library.”
    * The `only` sub-form imports only the specified identifiers
      from the given import-set.
    * The `except` sub-form imports all of the bindings exported by
      import-set except for those that appear in the specified list
      of identifiers.
    * The `prefix` sub-form imports all of the bindings exported
      by import-set, first prefixing them with the specified
      identifier.
    * The `rename` sub-form imports all of the identifiers exported
      by import-set. The binding for each internal-identifier
      among these identifiers is made visible to the importing library as
      the corresponding external-identifier; all other bindings are
      imported using the names provided by import-set.

    Note that because Guile translates R6RS libraries into module
    definitions, an import specification may be used to declare a
    dependency on a native Guile module — although doing so may make
    your libraries less portable to other Schemes.

Scheme Syntax: **import** import-spec ... [¶](#index-import-1)
:   Import into the current environment the libraries specified by the
    given import specifications, where each import-spec takes the
    same form as in the `library` form described above.

---

Next: [Module System Reflection](#Module-System-Reflection), Previous: [R6RS Libraries](#R6RS-Libraries), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.7 Variables [¶](#Variables-1)

Each module has its own hash table, sometimes known as an *obarray*,
that maps the names defined in that module to their corresponding
variable objects.

A variable is a box-like object that can hold any Scheme value. It is
said to be *undefined* if its box holds a special Scheme value that
denotes undefined-ness (which is different from all other Scheme values,
including for example `#f`); otherwise the variable is
*defined*.

On its own, a variable object is anonymous. A variable is said to be
*bound* when it is associated with a name in some way, usually a
symbol in a module obarray. When this happens, the name is said to be
bound to the variable, in that module.

(That’s the theory, anyway. In practice, defined-ness and bound-ness
sometimes get confused, because Lisp and Scheme implementations have
often conflated — or deliberately drawn no distinction between — a
name that is unbound and a name that is bound to a variable whose value
is undefined. We will try to be clear about the difference and explain
any confusion where it is unavoidable.)

Variables do not have a read syntax. Most commonly they are created and
bound implicitly by `define` expressions: a top-level `define`
expression of the form

```
(define name value)
```

creates a variable with initial value value and binds it to the
name name in the current module. But they can also be created
dynamically by calling one of the constructor procedures
`make-variable` and `make-undefined-variable`.

Scheme Procedure: **make-undefined-variable** [¶](#index-make_002dundefined_002dvariable)

C Function: **scm\_make\_undefined\_variable** () [¶](#index-scm_005fmake_005fundefined_005fvariable)
:   Return a variable that is initially unbound.

Scheme Procedure: **make-variable** init [¶](#index-make_002dvariable)

C Function: **scm\_make\_variable** (init) [¶](#index-scm_005fmake_005fvariable)
:   Return a variable initialized to value init.

Scheme Procedure: **variable-bound?** var [¶](#index-variable_002dbound_003f)

C Function: **scm\_variable\_bound\_p** (var) [¶](#index-scm_005fvariable_005fbound_005fp)
:   Return `#t` if var is bound to a value, or `#f`
    otherwise. Throws an error if var is not a variable object.

Scheme Procedure: **variable-ref** var [¶](#index-variable_002dref)

C Function: **scm\_variable\_ref** (var) [¶](#index-scm_005fvariable_005fref)
:   Dereference var and return its value.
    var must be a variable object; see `make-variable`
    and `make-undefined-variable`.

Scheme Procedure: **variable-set!** var val [¶](#index-variable_002dset_0021)

C Function: **scm\_variable\_set\_x** (var, val) [¶](#index-scm_005fvariable_005fset_005fx)
:   Set the value of the variable var to val.
    var must be a variable object, val can be any
    value. Return an unspecified value.

Scheme Procedure: **variable-unset!** var [¶](#index-variable_002dunset_0021)

C Function: **scm\_variable\_unset\_x** (var) [¶](#index-scm_005fvariable_005funset_005fx)
:   Unset the value of the variable var, leaving var unbound.

Scheme Procedure: **variable?** obj [¶](#index-variable_003f)

C Function: **scm\_variable\_p** (obj) [¶](#index-scm_005fvariable_005fp)
:   Return `#t` if obj is a variable object, else return
    `#f`.

---

Next: [Declarative Modules](#Declarative-Modules), Previous: [Variables](#Variables), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.8 Module System Reflection [¶](#Module-System-Reflection-1)

The previous sections have described a declarative view of the module
system. You can also work with it programmatically by accessing and
modifying various parts of the Scheme objects that Guile uses to
implement the module system.

At any time, there is a *current module*. This module is the one
where a top-level `define` and similar syntax will add new
bindings. You can find other module objects with `resolve-module`,
for example.

These module objects can be used as the second argument to `eval`.

Scheme Procedure: **current-module** [¶](#index-current_002dmodule)

C Function: **scm\_current\_module** () [¶](#index-scm_005fcurrent_005fmodule)
:   Return the current module object.

Scheme Procedure: **set-current-module** module [¶](#index-set_002dcurrent_002dmodule)

C Function: **scm\_set\_current\_module** (module) [¶](#index-scm_005fset_005fcurrent_005fmodule)
:   Set the current module to module and return
    the previous current module.

Scheme Procedure: **save-module-excursion** thunk [¶](#index-save_002dmodule_002dexcursion)
:   Call thunk within a `dynamic-wind` such that the module that
    is current at invocation time is restored when thunk’s dynamic
    extent is left (see [Dynamic Wind](#Dynamic-Wind)).

    More precisely, if thunk escapes non-locally, the current module
    (at the time of escape) is saved, and the original current module (at
    the time thunk’s dynamic extent was last entered) is restored. If
    thunk’s dynamic extent is re-entered, then the current module is
    saved, and the previously saved inner module is set current again.

Scheme Procedure: **resolve-module** name [autoload=#t] [version=#f] [#:ensure=#t] [¶](#index-resolve_002dmodule)

C Function: **scm\_resolve\_module** (name) [¶](#index-scm_005fresolve_005fmodule)
:   Find the module named name and return it. When it has not already
    been defined and autoload is true, try to auto-load it. When it
    can’t be found that way either, create an empty module if ensure
    is true, otherwise return `#f`. If version is true, ensure
    that the resulting module is compatible with the given version reference
    (see [R6RS Version References](#R6RS-Version-References)). The name is a list of symbols.

Scheme Procedure: **resolve-interface** name [#:select=#f] [#:hide=’()] [#:prefix=#f] [#:renamer=#f] [#:version=#f] [¶](#index-resolve_002dinterface)
:   Find the module named name as with `resolve-module` and
    return its interface. The interface of a module is also a module
    object, but it contains only the exported bindings.

Scheme Procedure: **module-uses** module [¶](#index-module_002duses)
:   Return a list of the interfaces used by module.

Scheme Procedure: **module-use!** module interface [¶](#index-module_002duse_0021)
:   Add interface to the front of the use-list of module. Both
    arguments should be module objects, and interface should very
    likely be a module returned by `resolve-interface`.

Scheme Procedure: **reload-module** module [¶](#index-reload_002dmodule)
:   Revisit the source file that corresponds to module. Raises an
    error if no source file is associated with the given module.

As mentioned in the previous section, modules contain a mapping between
identifiers (as symbols) and storage locations (as variables). Guile
defines a number of procedures to allow access to this mapping. If you
are programming in C, [Accessing Modules from C](#Accessing-Modules-from-C).

Scheme Procedure: **module-variable** module name [¶](#index-module_002dvariable)
:   Return the variable bound to name (a symbol) in module, or
    `#f` if name is unbound.

Scheme Procedure: **module-add!** module name var [¶](#index-module_002dadd_0021)
:   Define a new binding between name (a symbol) and var (a
    variable) in module.

Scheme Procedure: **module-ref** module name [¶](#index-module_002dref)
:   Look up the value bound to name in module. Like
    `module-variable`, but also does a `variable-ref` on the
    resulting variable, raising an error if name is unbound.

Scheme Procedure: **module-define!** module name value [¶](#index-module_002ddefine_0021)
:   Locally bind name to value in module. If name
    was already locally bound in module, i.e., defined locally and not
    by an imported module, the value stored in the existing variable will be
    updated. Otherwise, a new variable will be added to the module, via
    `module-add!`.

Scheme Procedure: **module-set!** module name value [¶](#index-module_002dset_0021)
:   Update the binding of name in module to value, raising
    an error if name is not already bound in module.

There are many other reflective procedures available in the default
environment. If you find yourself using one of them, please contact the
Guile developers so that we can commit to stability for that interface.

---

Next: [Accessing Modules from C](#Accessing-Modules-from-C), Previous: [Module System Reflection](#Module-System-Reflection), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.9 Declarative Modules [¶](#Declarative-Modules-1)

The first-class access to modules and module variables described in the
previous subsection is very powerful and allows Guile users to build
many tools to dynamically learn things about their Guile systems.
However, as Scheme godparent Mathias Felleisen wrote in “On the
Expressive Power of Programming Languages”, a more expressive language
is necessarily harder to reason about. There are transformations that
Guile’s compiler would like to make which can’t be done if every
top-level definition is subject to mutation at any time.

Consider this module:

```
(define-module (boxes)
  #:export (make-box box-ref box-set! box-swap!))

(define (make-box x) (list x))
(define (box-ref box) (car box))
(define (box-set! box x) (set-car! box x))
(define (box-swap! box x)
  (let ((y (box-ref box)))
    (box-set! box x)
    y))
```

Ideally you’d like for the `box-ref` in `box-swap!` to be
inlined to `car`. Guile’s compiler can do this, but only if it
knows that `box-ref`’s definition is what it appears to be in the
text. However, in the general case it could be that a programmer could
reach into the `(boxes)` module at any time and change the value of
`box-ref`.

To allow Guile to reason about the values of top-levels from a module, a
module can be marked as *declarative*. This flag applies only to
the subset of top-level definitions that are themselves declarative:
those that are defined within the compilation unit, and not assigned
(`set!`) or redefined within the compilation unit.

To explicitly mark a module as being declarative, pass the
`#:declarative?` keyword argument when declaring a module:

```
(define-module (boxes)
  #:export (make-box box-ref box-set! box-swap!)
  #:declarative? #t)
```

By default, modules are compiled declaratively if the
`user-modules-declarative?` parameter is true when the
module is compiled.

Scheme Parameter: **user-modules-declarative?** [¶](#index-user_002dmodules_002ddeclarative_003f)
:   A boolean indicating whether definitions in modules created by
    `define-module` or implicitly as part of a compilation unit without
    an explicit module can be treated as declarative.

Because it’s usually what you want, the default value of
`user-modules-declarative?` is `#t`.

#### Should I Mark My Module As Declarative? [¶](#Should-I-Mark-My-Module-As-Declarative_003f)

In the vast majority of use cases, declarative modules are what you
want. However, there are exceptions.

Consider the `(boxes)` module above. Let’s say you want to be able
to go in and change the definition of `box-set!` at run-time:

```
scheme@(guile-user)> (use-modules (boxes))
scheme@(guile-user)> ,module boxes
scheme@(boxes)> (define (box-set! x y) (set-car! x (pk y)))
```

However, considering that `(boxes)` is a declarative module, it
could be that `box-swap!` inlined the call to `box-set!` – so
it may be that you are surprised if you call `(box-swap! x y)` and
you don’t see the new definition being used. (Note, however, that Guile
has no guarantees about what definitions its compiler will or will not
inline.)

If you want to allow the definition of `box-set!` to be changed and
to have all of its uses updated, then probably the best option is to
edit the module and reload the whole thing:

```
scheme@(guile-user)> ,reload (boxes)
```

The advantage of the reloading approach is that you maintain the
optimizations that declarative modules enable, while also being able to
live-update the code. If the module keeps precious program state, those
definitions can be marked as `define-once` to prevent reloads from
overwriting them. See [Top Level Variable Definitions](#Top-Level), for more on `define-once`.
Incidentally, `define-once` also prevents declarative-definition
optimizations, so if there’s a limited subset of redefinable bindings,
`define-once` could be an interesting tool to mark those
definitions as works-in-progress for interactive program development.

To users, whether a module is declarative or not is mostly immaterial:
besides normal use via `use-modules`, users can reference and
redefine public or private bindings programmatically or interactively.
The only difference is that changing a declarative definition may not
change all of its uses. If this use-case is important to you, and if
reloading whole modules is insufficient, then you can mark all
definitions in a module as non-declarative by adding
`#:declarative? #f` to the module definition.

The default of whether modules are declarative or not can be controlled
via the `(user-modules-declarative?)` parameter mentioned above,
but care should be taken to set this parameter when the modules are
compiled, e.g. via `(eval-when (expand) (user-modules-declarative?
#f))`. See [Eval-when](#Eval-When).

Alternately you can prevent declarative-definition optimizations by
compiling at the `-O1` optimization level instead of the default
`-O2`, or via explicitly passing `-Ono-letrectify` to the
`guild compile` invocation. See [Compiling Scheme Code](#Compilation), for more on
compiler options.

One final note. Currently, definitions from declarative modules can
only be inlined within the module they are defined in, and within a
compilation unit. This may change in the future to allow Guile to
inline imported declarative definitions as well (cross-module inlining).
To Guile, whether a definition is inlinable or not is a property of the
definition, not its use. We hope to improve compiler tooling in the
future to allow the user to identify definitions that are out of date
when a declarative binding is redefined.

---

Next: [provide and require](#provide-and-require), Previous: [Declarative Modules](#Declarative-Modules), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.10 Accessing Modules from C [¶](#Accessing-Modules-from-C-1)

The last sections have described how modules are used in Scheme code,
which is the recommended way of creating and accessing modules. You
can also work with modules from C, but it is more cumbersome.

The following procedures are available.

C Function: `SCM` **scm\_c\_call\_with\_current\_module** `(SCM module, SCM (*func)(void *), void *data)` [¶](#index-scm_005fc_005fcall_005fwith_005fcurrent_005fmodule)
:   Call func and make module the current module during the
    call. The argument data is passed to func. The return
    value of `scm_c_call_with_current_module` is the return value of
    func.

C Function: `SCM` **scm\_public\_variable** `(SCM module_name, SCM name)` [¶](#index-scm_005fpublic_005fvariable)

C Function: `SCM` **scm\_c\_public\_variable** `(const char *module_name, const char *name)` [¶](#index-scm_005fc_005fpublic_005fvariable)
:   Find a the variable bound to the symbol name in the public
    interface of the module named module\_name.

    module\_name should be a list of symbols, when represented as a
    Scheme object, or a space-separated string, in the `const char *`
    case. See `scm_c_define_module` below, for more examples.

    Signals an error if no module was found with the given name. If
    name is not bound in the module, just returns `#f`.

C Function: `SCM` **scm\_private\_variable** `(SCM module_name, SCM name)` [¶](#index-scm_005fprivate_005fvariable)

C Function: `SCM` **scm\_c\_private\_variable** `(const char *module_name, const char *name)` [¶](#index-scm_005fc_005fprivate_005fvariable)
:   Like `scm_public_variable`, but looks in the internals of the
    module named module\_name instead of the public interface.
    Logically, these procedures should only be called on modules you write.

C Function: `SCM` **scm\_public\_lookup** `(SCM module_name, SCM name)` [¶](#index-scm_005fpublic_005flookup)

C Function: `SCM` **scm\_c\_public\_lookup** `(const char *module_name, const char *name)` [¶](#index-scm_005fc_005fpublic_005flookup)

C Function: `SCM` **scm\_private\_lookup** `(SCM module_name, SCM name)` [¶](#index-scm_005fprivate_005flookup)

C Function: `SCM` **scm\_c\_private\_lookup** `(const char *module_name, const char *name)` [¶](#index-scm_005fc_005fprivate_005flookup)
:   Like `scm_public_variable` or `scm_private_variable`, but if
    the name is not bound in the module, signals an error. Returns a
    variable, always.

    ```
    static SCM eval_string_var;

    /* NOTE: It is important that the call to 'my_init'
       happens-before all calls to 'my_eval_string'. */
    void my_init (void)
    {
      eval_string_var = scm_c_public_lookup ("ice-9 eval-string",
                                             "eval-string");
    }

    SCM my_eval_string (SCM str)
    {
      return scm_call_1 (scm_variable_ref (eval_string_var), str);
    }
    ```

C Function: `SCM` **scm\_public\_ref** `(SCM module_name, SCM name)` [¶](#index-scm_005fpublic_005fref)

C Function: `SCM` **scm\_c\_public\_ref** `(const char *module_name, const char *name)` [¶](#index-scm_005fc_005fpublic_005fref)

C Function: `SCM` **scm\_private\_ref** `(SCM module_name, SCM name)` [¶](#index-scm_005fprivate_005fref)

C Function: `SCM` **scm\_c\_private\_ref** `(const char *module_name, const char *name)` [¶](#index-scm_005fc_005fprivate_005fref)
:   Like `scm_public_lookup` or `scm_private_lookup`, but
    additionally dereferences the variable. If the variable object is
    unbound, signals an error. Returns the value bound to name in
    module\_name.

In addition, there are a number of other lookup-related procedures. We
suggest that you use the `scm_public_` and `scm_private_`
family of procedures instead, if possible.

C Function: `SCM` **scm\_c\_lookup** `(const char *name)` [¶](#index-scm_005fc_005flookup)
:   Return the variable bound to the symbol indicated by name in the
    current module. If there is no such binding or the symbol is not
    bound to a variable, signal an error.

C Function: `SCM` **scm\_lookup** `(SCM name)` [¶](#index-scm_005flookup)
:   Like `scm_c_lookup`, but the symbol is specified directly.

C Function: `SCM` **scm\_c\_module\_lookup** `(SCM module, const char *name)` [¶](#index-scm_005fc_005fmodule_005flookup)

C Function: `SCM` **scm\_module\_lookup** `(SCM module, SCM name)` [¶](#index-scm_005fmodule_005flookup)
:   Like `scm_c_lookup` and `scm_lookup`, but the specified
    module is used instead of the current one.

C Function: `SCM` **scm\_module\_variable** `(SCM module, SCM name)` [¶](#index-scm_005fmodule_005fvariable)
:   Like `scm_module_lookup`, but if the binding does not exist, just
    returns `#f` instead of raising an error.

To define a value, use `scm_define`:

C Function: `SCM` **scm\_c\_define** `(const char *name, SCM val)` [¶](#index-scm_005fc_005fdefine-1)
:   Bind the symbol indicated by name to a variable in the current
    module and set that variable to val. When name is already
    bound to a variable, use that. Else create a new variable.

C Function: `SCM` **scm\_define** `(SCM name, SCM val)` [¶](#index-scm_005fdefine-1)
:   Like `scm_c_define`, but the symbol is specified directly.

C Function: `SCM` **scm\_c\_module\_define** `(SCM module, const char *name, SCM val)` [¶](#index-scm_005fc_005fmodule_005fdefine)

C Function: `SCM` **scm\_module\_define** `(SCM module, SCM name, SCM val)` [¶](#index-scm_005fmodule_005fdefine)
:   Like `scm_c_define` and `scm_define`, but the specified
    module is used instead of the current one.

In some rare cases, you may need to access the variable that
`scm_module_define` would have accessed, without changing the
binding of the existing variable, if one is present. In that case, use
`scm_module_ensure_local_variable`:

C Function: `SCM` **scm\_module\_ensure\_local\_variable** `(SCM module, SCM sym)` [¶](#index-scm_005fmodule_005fensure_005flocal_005fvariable)
:   Like `scm_module_define`, but if the sym is already locally
    bound in that module, the variable’s existing binding is not reset.
    Returns a variable.

C Function: `SCM` **scm\_module\_reverse\_lookup** `(SCM module, SCM variable)` [¶](#index-scm_005fmodule_005freverse_005flookup)
:   Find the symbol that is bound to variable in module. When no such binding is found, return `#f`.

C Function: `SCM` **scm\_c\_define\_module** `(const char *name, void (*init)(void *), void *data)` [¶](#index-scm_005fc_005fdefine_005fmodule)
:   Define a new module named name and make it current while
    init is called, passing it data. Return the module.

    The parameter name is a string with the symbols that make up
    the module name, separated by spaces. For example, ‘`"foo bar"`’ names
    the module ‘`(foo bar)`’.

    When there already exists a module named name, it is used
    unchanged, otherwise, an empty module is created.

C Function: `SCM` **scm\_c\_resolve\_module** `(const char *name)` [¶](#index-scm_005fc_005fresolve_005fmodule)
:   Find the module name name and return it. When it has not
    already been defined, try to auto-load it. When it can’t be found
    that way either, create an empty module. The name is interpreted as
    for `scm_c_define_module`.

C Function: `SCM` **scm\_c\_use\_module** `(const char *name)` [¶](#index-scm_005fc_005fuse_005fmodule)
:   Add the module named name to the uses list of the current
    module, as with `(use-modules name)`. The name is
    interpreted as for `scm_c_define_module`.

C Function: `void` **scm\_c\_export** `(const char *name, ...)` [¶](#index-scm_005fc_005fexport)
:   Add the bindings designated by name, ... to the public interface
    of the current module. The list of names is terminated by
    `NULL`.

---

Next: [Environments](#Environments), Previous: [Accessing Modules from C](#Accessing-Modules-from-C), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.11 provide and require [¶](#provide-and-require-1)

Aubrey Jaffer, mostly to support his portable Scheme library SLIB,
implemented a provide/require mechanism for many Scheme implementations.
Library files in SLIB *provide* a feature, and when user programs
*require* that feature, the library file is loaded in.

For example, the file `random.scm` in the SLIB package contains the
line

```
(provide 'random)
```

so to use its procedures, a user would type

```
(require 'random)
```

and they would magically become available, *but still have the same
names!* So this method is nice, but not as good as a full-featured
module system.

When SLIB is used with Guile, provide and require can be used to access
its facilities.

---

Previous: [provide and require](#provide-and-require), Up: [Modules](#Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.18.12 Environments [¶](#Environments-1)

Scheme, as defined in R5RS, does *not* have a full module system.
However it does define the concept of a top-level *environment*.
Such an environment maps identifiers (symbols) to Scheme objects such
as procedures and lists: [The Concept of Closure](#About-Closure). In other words, it
implements a set of *bindings*.

Environments in R5RS can be passed as the second argument to
`eval` (see [Procedures for On the Fly Evaluation](#Fly-Evaluation)). Three procedures are defined to
return environments: `scheme-report-environment`,
`null-environment` and `interaction-environment` (see [Procedures for On the Fly Evaluation](#Fly-Evaluation)).

In addition, in Guile any module can be used as an R5RS environment,
i.e., passed as the second argument to `eval`.

Note: the following two procedures are available only when the
`(ice-9 r5rs)` module is loaded:

```
(use-modules (ice-9 r5rs))
```

Scheme Procedure: **scheme-report-environment** version [¶](#index-scheme_002dreport_002denvironment)

Scheme Procedure: **null-environment** version [¶](#index-null_002denvironment)
:   version must be the exact integer ‘5’, corresponding to revision
    5 of the Scheme report (the Revised^5 Report on Scheme).
    `scheme-report-environment` returns a specifier for an
    environment that is empty except for all bindings defined in the
    report that are either required or both optional and supported by the
    implementation. `null-environment` returns a specifier for an
    environment that is empty except for the (syntactic) bindings for all
    syntactic keywords defined in the report that are either required or
    both optional and supported by the implementation.

    Currently Guile does not support values of version for other
    revisions of the report.

    The effect of assigning (through the use of `eval`) a variable
    bound in a `scheme-report-environment` (for example `car`)
    is unspecified. Currently the environments specified by
    `scheme-report-environment` are not immutable in Guile.

---

Next: [Foreign Objects](#Foreign-Objects), Previous: [Modules](#Modules), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
