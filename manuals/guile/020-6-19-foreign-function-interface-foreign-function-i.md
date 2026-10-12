### 6.19 Foreign Function Interface [¶](#Foreign-Function-Interface-1)

Sometimes you need to use libraries written in C or Rust or some other
non-Scheme language. More rarely, you might need to write some C to
extend Guile. This section describes how to load these “foreign
libraries”, look up data and functions inside them, and so on.

* [Foreign Libraries](#Foreign-Libraries)
* [Foreign Extensions](#Foreign-Extensions)
* [Foreign Pointers](#Foreign-Pointers)
* [Foreign Types](#Foreign-Types)
* [Foreign Functions](#Foreign-Functions)
* [Void Pointers and Byte Access](#Void-Pointers-and-Byte-Access)
* [Foreign Structs](#Foreign-Structs)
* [More Foreign Functions](#More-Foreign-Functions)

---

Next: [Foreign Extensions](#Foreign-Extensions), Up: [Foreign Function Interface](#Foreign-Function-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.19.1 Foreign Libraries [¶](#Foreign-Libraries-1)

Just as Guile can load up Scheme libraries at run-time, Guile can also
load some system libraries written in C or other low-level languages.
We refer to these as dynamically-loadable modules as *foreign
libraries*, to distinguish them from native libraries written in Scheme
or other languages implemented by Guile.

Foreign libraries usually come in two forms. Some foreign libraries are
part of the operating system, such as the compression library
`libz`. These shared libraries are built in such a way that many
programs can use their functionality without duplicating their code.
When a program written in C is built, it can declare that it uses a
specific set of shared libraries.

When the program is run, the operating system takes care of locating and
loading the shared libraries.

The operating system components that can dynamically load and link
shared libraries when a program is run are also available
programmatically during a program’s execution. This is the interface
that’s most useful for Guile, and this is what we mean in Guile when we
refer to *dynamic linking*. Dynamic linking at run-time is
sometimes called *dlopening*, to distinguish it from the dynamic
linking that happens at program start-up.

The other kind of foreign library is sometimes known as a module,
plug-in, bundle, or an extension. These foreign libraries aren’t meant
to be linked to by C programs, but rather only to be dynamically loaded
at run-time – they extend some main program with functionality, but
don’t stand on their own. Sometimes a Guile library will implement some
of its functionality in a loadable module.

In either case, the interface on the Guile side is the same. You load
the interface using `load-foreign-library`. The resulting foreign
library object implements a simple lookup interface whereby the user can
get addresses of data or code exported by the library. There is no
facility to inspect foreign libraries; you have to know what’s in there
already before you look.

Routines for loading foreign libraries and accessing their contents are
implemented in the `(system foreign-library)` module.

```
(use-modules (system foreign-library))
```

Scheme Procedure: **load-foreign-library** [library] [#:extensions=system-library-extensions] [#:search-ltdl-library-path?=#t] [#:search-path=search-path] [#:search-system-paths?=#t] [#:lazy?=#t] [#:global=#f] [#:host-type-rename?=#t] [#:allow-dll-version-suffix?=#t] [¶](#index-load_002dforeign_002dlibrary)
:   This procedure finds the shared library denoted by library (a
    string) and dynamically links it into the running Guile application. On
    success, the procedure returns a Scheme object suitable for representing
    the linked object file. Otherwise an error is thrown.

    In the common usage, the library parameter is a filename
    with no path and with no filename extension, such as `.so`,
    `.dylib` or `.dll`. The procedure will search for the library
    in a set of standard locations using the common filename extensions for
    the OS. The optional parameters can customize this behavior.

    When library has directory elements or a filename extension, a
    more targeted search is performed.

    For each system, Guile has a default set of extensions
    that it will try. On GNU systems, the default extension set is just
    `.so`; on Windows, just `.dll`; and on Darwin (Mac OS), it is
    `.bundle`, `.so`, and `.dylib`. Pass `#:extensions
    extensions` to override the default extensions list. If
    library contains one of the extensions, no extensions are tried,
    so it is possible to specify the extension if you know exactly what file
    to load.

    Unless library denotes an absolute file name or otherwise contains
    a directory separator (`/`, and also `\` on Windows), Guile
    will search for the library in the directories listed in
    search-paths. The default search path has three components, which
    can all be overridden by colon-delimited (semicolon on Windows)
    environment variables:

    `GUILE_EXTENSIONS_PATH`
    :   This is the environment variable for users to add directories containing
        Guile extensions to the search path. The default value has no entries.
        This environment variable was added in Guile 3.0.6.

    `LTDL_LIBRARY_PATH`
    :   When search-ltdl-library-path? is true, this environment variable
        can also be used to add directories to the search path. For each
        directory given in this environment variable, two directories are added
        to the search path: the given directory (for example, `D`) and a
        `.libs` subdirectory (`D/.libs`).

        For more information on the rationale, see the note below.

    `GUILE_SYSTEM_EXTENSIONS_PATH`
    :   The last path in Guile’s search path belongs to Guile itself, and
        defaults to the libdir and the extensiondir, in that order. For
        example, if you install to `/opt/guile`, these would probably be
        `/opt/guile/lib` and
        `/opt/guile/lib/guile/3.0/extensions`,
        respectively. See [Parallel Installations](#Parallel-Installations), for more details on
        `extensionsdir`.

        For DLL-using systems, it searches bindir rather than libdir, so
        `/opt/guile/bin` in this example.

    Finally, if no library is found in the search path, and if library
    is not absolute and does not include directory separators, and if
    search-system-paths? is true, the operating system may have its
    own logic for where to locate library. For example, on GNU, there
    will be a default set of paths (often `/usr/lib` and `/lib`,
    though it depends on the system), and the `LD_LIBRARY_PATH`
    environment variable can add additional paths. On DLL-using systems,
    the `PATH` is searched. Other operating systems have other
    conventions.

    Falling back to the operating system for search is usually not a great
    thing; it is a recipe for making programs that work on one machine but
    not on others. Still, when wrapping system libraries, it can be the
    only way to get things working at all.

    If lazy? is true (the default), Guile will request the operating
    system to resolve symbols used by the loaded library as they are first
    used. If global? is true, symbols defined by the loaded library
    will be available when other modules need to resolve symbols; the
    default is `#f`, which keeps symbols local.

    If host-type-rename? is true (the default) library names may be
    modified based on the current `%host-type`. On Cygwin hosts,
    the search behavior is modified such that a filename that starts with
    “lib” will be searched for under the name “cyg”, as is customary for
    Cygwin. Similarly, for MSYS hosts, “lib” becomes “msys-”.

    If dll-version-suffix? is true (the default), the search behavior
    is modified such that when searching for a DLL, it will also search for
    DLLs with version suffixes. For example, a search for
    `libtiff.dll` will also allow `libtiff-1.dll`. When the
    unversioned DLL is not found and multiple versioned DLLs exists, it will
    return the versioned DLL with the highest version. Note that when
    searching, directories take precedence. It does not return the highest
    versioned DLL among all search directories collectively; it returns the
    highest versioned in the first directory to have the DLL.

    If library argument is omitted, it defaults to `#f`. If
    `library` is false, the resulting foreign library gives access to
    all symbols available for dynamic linking in the currently running
    executable.

The environment variables mentioned above are parsed when the
foreign-library module is first loaded and bound to parameters. Null
path components, for example the three components of
`GUILE_SYSTEM_EXTENSIONS_PATH="::"`, are ignored.

Scheme Parameter: **guile-extensions-path** [¶](#index-guile_002dextensions_002dpath)

Scheme Parameter: **ltdl-library-path** [¶](#index-ltdl_002dlibrary_002dpath)

Scheme Parameter: **guile-system-extensions-path** [¶](#index-guile_002dsystem_002dextensions_002dpath)
:   Parameters whose initial values are taken from
    `GUILE_EXTENSIONS_PATH`, `LTDL_LIBRARY_PATH`, and
    `GUILE_SYSTEM_EXTENSIONS_PATH`, respectively. See [Parameters](#Parameters).
    The current values of these parameters are used when building the search
    path when `load-foreign-library` is called, unless the caller
    explicitly passes a `#:search-path` argument.

Scheme Procedure: **foreign-library?** obj [¶](#index-foreign_002dlibrary_003f)
:   Return `#t` if obj is a foreign library, or `#f`
    otherwise.

Before Guile 3.0.6, Guile loaded foreign libraries using `libltdl`,
the dynamic library loader provided by libtool. This loader used
`LTDL_LIBRARY_PATH`, and for backwards compatibility we still
support that path.

However, `libltdl` would not only open `.so` (or `.dll`
and so on) files, but also the `.la` files created by libtool. In
installed libraries – libraries that are in the target directories of
`make install` – `.la` files are never needed, to the extent
that most GNU/Linux distributions remove them entirely. It is
sufficient to just load the `.so` (or `.dll` and so on) files,
which are always located in the same directory as the `.la` files.

But for uninstalled dynamic libraries, like those in a build tree, the
situation is a bit of a mess. If you have a project that uses libtool
to build libraries – which is the case for Guile, and for most projects
using autotools – and you build `foo.so` in directory `D`,
libtool will put `foo.la` in `D`, but `foo.so` gets put
into `D/.libs`.

Users were mostly oblivious to this situation, as `libltdl` had
special logic to be able to read the `.la` file to know where to
find the `.so`, even from an uninstalled build tree, preventing the
existence of `.libs` from leaking out to the user.

We don’t use libltdl now, essentially for flexibility and
error-reporting reasons. But, to keep this old use-case working, if
search-ltdl-library-path? is true, we add each entry of
`LTDL_LIBRARY_PATH` to the default extensions load path,
additionally adding the `.libs` subdirectories for each entry, in
case there are `.so` files there instead of alongside the
`.la` files.

---

Next: [Foreign Pointers](#Foreign-Pointers), Previous: [Foreign Libraries](#Foreign-Libraries), Up: [Foreign Function Interface](#Foreign-Function-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.19.2 Foreign Extensions [¶](#Foreign-Extensions-1)

One way to use shared libraries is to extend Guile. Such loadable
modules generally define one distinguished initialization function that,
when called, will use the `libguile` API to define procedures in
the current module.

Concretely, you might extend Guile with an implementation of the Bessel
function, `j0`:

```
#include <math.h>
#include <libguile.h>

SCM
j0_wrapper (SCM x)
{
  return scm_from_double (j0 (scm_to_double (x, "j0")));
}

void
init_math_bessel (void)
{
  scm_c_define_gsubr ("j0", 1, 0, 0, j0_wrapper);
}
```

The C source file would then need to be compiled into a shared library.
On GNU/Linux, the compiler invocation might look like this:

```
gcc -shared -o bessel.so -fPIC bessel.c
```

A good default place to put shared libraries that extend Guile is into
the extensions dir. From the command line or a build script, invoke
`pkg-config --variable=extensionsdir
guile-3.0` to print the extensions dir.
See [Parallel Installations](#Parallel-Installations), for more details.

Guile can load up `bessel.so` via `load-extension`.

Scheme Procedure: **load-extension** lib init [¶](#index-load_002dextension)

C Function: **scm\_load\_extension** (lib, init) [¶](#index-scm_005fload_005fextension)
:   Load and initialize the extension designated by LIB and INIT.

The normal way for a extension to be used is to write a small Scheme
file that defines a module, and to load the extension into this
module. When the module is auto-loaded, the extension is loaded as
well. For example:

```
(define-module (math bessel)
  #:export (j0))

(load-extension "bessel" "init_math_bessel")
```

This `load-extension` invocation loads the `bessel` library
via `(load-foreign-library "bessel")`, then looks up the
`init_math_bessel` symbol in the library, treating it as a function
of no arguments, and calls that function.

If you decide to put your extension outside the default search path for
`load-foreign-library`, probably you should adapt the Scheme module
to specify its absolute path. For example, if you use `automake`
to build your extension and place it in `$(pkglibdir)`, you might
define a build-parameters module that gets created by the build system:

```
(define-module (math config)
  #:export (extensiondir))
(define extensiondir "PKGLIBDIR")
```

This file would be `config.scm.in`. You would define a `make`
rule to substitute in the absolute installed file name:

```
config.scm: config.scm.in
        sed 's|PKGLIBDIR|$(pkglibdir)|' <$< >$ 
```

Then your `(math bessel)` would import `(math config)`, then
`(load-extension (in-vicinity extensiondir "bessel")
"init_math_bessel")`.

An alternate approach would be to rebind the
`guile-extensions-path` parameter, or its corresponding environment
variable, but note that changing those parameters applies to other users
of `load-foreign-library` as well.

Note that the new primitives that the extension adds to Guile with
`scm_c_define_gsubr` (see [Primitive Procedures](#Primitive-Procedures)) or with any of
the other mechanisms are placed into the module that is current when the
`scm_c_define_gsubr` is executed, so to be clear about what goes
where it’s best to include the `load-extension` in a module, as
above. Alternately, the C code can use `scm_c_define_module` to
specify which module is being created:

```
static void
do_init (void *unused)
{
  scm_c_define_gsubr ("j0", 1, 0, 0, j0_wrapper);
  scm_c_export ("j0", NULL);
}

void
init_math_bessel ()
{
  scm_c_define_module ("math bessel", do_init, NULL);
}
```

And yet... if what we want is just the `j0` function, it seems like
a lot of ceremony to have to compile a Guile-specific wrapper library
complete with an initialization function and wrapper module to allow
Guile users to call it. There is another way, but to get there, we have
to talk about function pointers and function types first. See [Foreign Functions](#Foreign-Functions), to skip to the good parts.

---

Next: [Foreign Types](#Foreign-Types), Previous: [Foreign Extensions](#Foreign-Extensions), Up: [Foreign Function Interface](#Foreign-Function-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.19.3 Foreign Pointers [¶](#Foreign-Pointers-1)

Foreign libraries are essentially key-value mappings, where the keys are
names of definitions and the values are the addresses of those
definitions. To look up the address of a definition, use
`foreign-library-pointer` from the `(system foreign-library)`
module.

Scheme Procedure: **foreign-library-pointer** lib name [¶](#index-foreign_002dlibrary_002dpointer)
:   Return a “wrapped pointer” for the symbol name in the shared
    object referred to by lib. The returned pointer points to a C
    object.

    As a convenience, if lib is not a foreign library, it will be
    passed to `load-foreign-library`.

If we continue with the `bessel.so` example from before, we can get
the address of the `init_math_bessel` function via:

```
(use-modules (system foreign-library))
(define init (foreign-library-pointer "bessel" "init_math_bessel"))
init
⇒ #<pointer 0x7fb35b1b4688>
```

A value returned by `foreign-library-pointer` is a Scheme wrapper
for a C pointer. Pointers are a data type in Guile that is disjoint
from all other types. The next section discusses ways to dereference
pointers, but before then we describe the usual type predicates and so
on.

Note that the rest of the interfaces in this section are part of the
`(system foreign)` library:

```
(use-modules (system foreign))
```

Scheme Procedure: **pointer-address** pointer [¶](#index-pointer_002daddress)

C Function: **scm\_pointer\_address** (pointer) [¶](#index-scm_005fpointer_005faddress)
:   Return the numerical value of pointer.

    ```
    (pointer-address init)
    ⇒ 139984413364296 ; YMMV
    ```

Scheme Procedure: **make-pointer** address [finalizer] [¶](#index-make_002dpointer)
:   Return a foreign pointer object pointing to address. If
    finalizer is passed, it should be a pointer to a one-argument C
    function that will be called when the pointer object becomes
    unreachable.

Scheme Procedure: **pointer?** obj [¶](#index-pointer_003f)
:   Return `#t` if obj is a pointer object, or `#f`
    otherwise.

Scheme Variable: **%null-pointer** [¶](#index-_0025null_002dpointer)
:   A foreign pointer whose value is 0.

Scheme Procedure: **null-pointer?** pointer [¶](#index-null_002dpointer_003f)
:   Return `#t` if pointer is the null pointer, `#f` otherwise.

For the purpose of passing SCM values directly to foreign functions, and
allowing them to return SCM values, Guile also supports some unsafe
casting operators.

Scheme Procedure: **scm->pointer** scm [¶](#index-scm_002d_003epointer)
:   Return a foreign pointer object with the `object-address`
    of scm.

Scheme Procedure: **pointer->scm** pointer [¶](#index-pointer_002d_003escm)
:   Unsafely cast pointer to a Scheme object.
    Cross your fingers!

Sometimes you want to give C extensions access to the dynamic FFI. At
that point, the names get confusing, because “pointer” can refer to a
`SCM` object that wraps a pointer, or to a `void*` value. We
will try to use “pointer object” to refer to Scheme objects, and
“pointer value” to refer to `void *` values.

C Function: `SCM` **scm\_from\_pointer** `(void *ptr, void (*finalizer) (void*))` [¶](#index-scm_005ffrom_005fpointer)
:   Create a pointer object from a pointer value.

    If finalizer is non-null, Guile arranges to call it on the pointer
    value at some point after the pointer object becomes collectible.

C Function: `void*` **scm\_to\_pointer** `(SCM obj)` [¶](#index-scm_005fto_005fpointer)
:   Unpack the pointer value from a pointer object.

---

Next: [Foreign Functions](#Foreign-Functions), Previous: [Foreign Pointers](#Foreign-Pointers), Up: [Foreign Function Interface](#Foreign-Function-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.19.4 Foreign Types [¶](#Foreign-Types-1)

From Scheme’s perspective, foreign pointers are shards of chaos. The
user can create a foreign pointer for any address, and do with it what
they will. The only thing that lends a sense of order to the whole is a
shared hallucination that certain storage locations have certain types.
When making Scheme wrappers for foreign interfaces, we hide the madness
by explicitly representing the the data types of parameters and fields.

These “foreign type values” may be constructed using the constants and
procedures from the `(system foreign)` module, which may be loaded
like this:

```
(use-modules (system foreign))
```

`(system foreign)` exports a number of values expressing the basic
C types.

Scheme Variable: **int8** [¶](#index-int8)

Scheme Variable: **uint8** [¶](#index-uint8)

Scheme Variable: **uint16** [¶](#index-uint16)

Scheme Variable: **int16** [¶](#index-int16)

Scheme Variable: **uint32** [¶](#index-uint32)

Scheme Variable: **int32** [¶](#index-int32)

Scheme Variable: **uint64** [¶](#index-uint64)

Scheme Variable: **int64** [¶](#index-int64)

Scheme Variable: **float** [¶](#index-float)

Scheme Variable: **double** [¶](#index-double)

Scheme Variable: **complex-double** [¶](#index-complex_002ddouble)

Scheme Variable: **complex-float** [¶](#index-complex_002dfloat)
:   These values represent the C numeric types of the specified sizes and
    signednesses. `complex-float` and `complex-double` stand for
    C99 `float _Complex` and `double _Complex` respectively.

In addition there are some convenience bindings for indicating types of
platform-dependent size.

Scheme Variable: **int** [¶](#index-int)

Scheme Variable: **unsigned-int** [¶](#index-unsigned_002dint)

Scheme Variable: **long** [¶](#index-long)

Scheme Variable: **unsigned-long** [¶](#index-unsigned_002dlong)

Scheme Variable: **short** [¶](#index-short)

Scheme Variable: **unsigned-short** [¶](#index-unsigned_002dshort)

Scheme Variable: **size\_t** [¶](#index-size_005ft-1)

Scheme Variable: **ssize\_t** [¶](#index-ssize_005ft)

Scheme Variable: **ptrdiff\_t** [¶](#index-ptrdiff_005ft)

Scheme Variable: **intptr\_t** [¶](#index-intptr_005ft)

Scheme Variable: **uintptr\_t** [¶](#index-uintptr_005ft)
:   Values exported by the `(system foreign)` module, representing C
    numeric types. For example, `long` may be `equal?` to
    `int64` on a 64-bit platform.

Scheme Variable: **void** [¶](#index-void)
:   The `void` type. It can be used as the first argument to
    `pointer->procedure` to wrap a C function that returns nothing.

In addition, the symbol `*` is used by convention to denote pointer
types. Procedures detailed in the following sections, such as
`pointer->procedure`, accept it as a type descriptor.

---

Next: [Void Pointers and Byte Access](#Void-Pointers-and-Byte-Access), Previous: [Foreign Types](#Foreign-Types), Up: [Foreign Function Interface](#Foreign-Function-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.19.5 Foreign Functions [¶](#Foreign-Functions-1)

The most natural thing to do with a dynamic library is to grovel around
in it for a function pointer: a *foreign function*. Load the
`(system foreign)` module to use these Scheme interfaces.

```
(use-modules (system foreign))
```

Scheme Procedure: **pointer->procedure** return\_type func\_ptr arg\_types [#:return-errno?=#f] [¶](#index-pointer_002d_003eprocedure)

C Function: **scm\_pointer\_to\_procedure** (return\_type, func\_ptr, arg\_types) [¶](#index-scm_005fpointer_005fto_005fprocedure)

C Function: **scm\_pointer\_to\_procedure\_with\_errno** (return\_type, func\_ptr, arg\_types) [¶](#index-scm_005fpointer_005fto_005fprocedure_005fwith_005ferrno)
:   Make a foreign function.

    Given the foreign void pointer func\_ptr, its argument and
    return types arg\_types and return\_type, return a
    procedure that will pass arguments to the foreign function
    and return appropriate values.

    arg\_types should be a list of foreign types.
    `return_type` should be a foreign type. See [Foreign Types](#Foreign-Types), for
    more information on foreign types.

    If return-errno? is true, or when calling
    `scm_pointer_to_procedure_with_errno`, the returned procedure will
    return two values, with `errno` as the second value.

Finally, in `(system foreign-library)` there is a convenient
wrapper function, joining together `foreign-library-pointer` and
`pointer->procedure`:

Scheme Procedure: **foreign-library-function** lib name [#:return-type=void] [#:arg-types=’()] [#:return-errno?=#f] [¶](#index-foreign_002dlibrary_002dfunction)
:   Load the address of name from lib, and treat it as a
    function taking arguments arg-types and returning
    return-type, optionally also with errno.

    An invocation of `foreign-library-function` is entirely equivalent
    to:

    ```
    (pointer->procedure return-type
                        (foreign-library-pointer lib name)
                        arg-types
                        #:return-errno? return-errno?).
    ```

Pulling all this together, here is a better definition of `(math
bessel)`:

```
(define-module (math bessel)
  #:use-module (system foreign)
  #:use-module (system foreign-library)
  #:export (j0))

(define j0
  (foreign-library-function "libm" "j0"
                            #:return-type double
                            #:arg-types (list double)))
```

That’s it! No C at all.

Before going on to more detailed examples, the next two sections discuss
how to deal with data that is more complex than, say, `int8`.
See [More Foreign Functions](#More-Foreign-Functions), to continue with foreign function examples.

---

Next: [Foreign Structs](#Foreign-Structs), Previous: [Foreign Functions](#Foreign-Functions), Up: [Foreign Function Interface](#Foreign-Function-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.19.6 Void Pointers and Byte Access [¶](#Void-Pointers-and-Byte-Access-1)

Wrapped pointers are untyped, so they are essentially equivalent to C
`void` pointers. As in C, the memory region pointed to by a
pointer can be accessed at the byte level. This is achieved using
*bytevectors* (see [Bytevectors](#Bytevectors)). The `(rnrs bytevectors)`
module contains procedures that can be used to convert byte sequences to
Scheme objects such as strings, floating point numbers, or integers.

Load the `(system foreign)` module to use these Scheme interfaces.

```
(use-modules (system foreign))
```

Scheme Procedure: **pointer->bytevector** pointer len [offset [uvec\_type]] [¶](#index-pointer_002d_003ebytevector)

C Function: **scm\_pointer\_to\_bytevector** (pointer, len, offset, uvec\_type) [¶](#index-scm_005fpointer_005fto_005fbytevector)
:   Return a bytevector aliasing the len bytes pointed to by
    pointer.

    The user may specify an alternate default interpretation for the memory
    by passing the uvec\_type argument, to indicate that the memory is
    an array of elements of that type. uvec\_type should be something
    that `array-type` would return, like `f32` or `s16`.

    When offset is passed, it specifies the offset in bytes relative
    to pointer of the memory region aliased by the returned
    bytevector.

    Mutating the returned bytevector mutates the memory pointed to by
    pointer, so buckle your seatbelts.

Scheme Procedure: **bytevector->pointer** bv [offset] [¶](#index-bytevector_002d_003epointer)

C Function: **scm\_bytevector\_to\_pointer** (bv, offset) [¶](#index-scm_005fbytevector_005fto_005fpointer)
:   Return a pointer aliasing the memory pointed to by bv or
    offset bytes after bv when offset is passed.

In addition to these primitives, convenience procedures are available:

Scheme Procedure: **dereference-pointer** pointer [¶](#index-dereference_002dpointer)
:   Assuming pointer points to a memory region that holds a pointer,
    return this pointer.

Scheme Procedure: **string->pointer** string [encoding] [¶](#index-string_002d_003epointer)
:   Return a foreign pointer to a nul-terminated copy of string in the
    given encoding, defaulting to the current locale encoding. The C
    string is freed when the returned foreign pointer becomes unreachable.

    This is the Scheme equivalent of `scm_to_stringn`.

Scheme Procedure: **pointer->string** pointer [length] [encoding] [¶](#index-pointer_002d_003estring)
:   Return the string representing the C string pointed to by pointer.
    If length is omitted or `-1`, the string is assumed to be
    nul-terminated. Otherwise length is the number of bytes in memory
    pointed to by pointer. The C string is assumed to be in the given
    encoding, defaulting to the current locale encoding.

    This is the Scheme equivalent of `scm_from_stringn`.

Most object-oriented C libraries use pointers to specific data
structures to identify objects. It is useful in such cases to reify the
different pointer types as disjoint Scheme types. The
`define-wrapped-pointer-type` macro simplifies this.

Scheme Syntax: **define-wrapped-pointer-type** type-name pred wrap unwrap print [¶](#index-define_002dwrapped_002dpointer_002dtype)
:   Define helper procedures to wrap pointer objects into Scheme objects
    with a disjoint type. Specifically, this macro defines:

    * pred, a predicate for the new Scheme type;
    * wrap, a procedure that takes a pointer object and returns an
      object that satisfies pred;
    * unwrap, which does the reverse.

    wrap preserves pointer identity, for two pointer objects p1
    and p2 that are `equal?`, `(eq? (wrap p1)
    (wrap p2)) ⇒ #t`.

    Finally, print should name a user-defined procedure to print such
    objects. The procedure is passed the wrapped object and a port to write
    to.

    For example, assume we are wrapping a C library that defines a type,
    `bottle_t`, and functions that can be passed `bottle_t *`
    pointers to manipulate them. We could write:

    ```
    (define-wrapped-pointer-type bottle
      bottle?
      wrap-bottle unwrap-bottle
      (lambda (b p)
        (format p "#<bottle of ~a ~x>"
                (bottle-contents b)
                (pointer-address (unwrap-bottle b)))))

    (define grab-bottle
      ;; Wrapper for `bottle_t *grab (void)'.
      (let ((grab (foreign-library-function libbottle "grab_bottle"
                                            #:return-type '*)))
        (lambda ()
          "Return a new bottle."
          (wrap-bottle (grab)))))

    (define bottle-contents
      ;; Wrapper for `const char *bottle_contents (bottle_t *)'.
      (let ((contents (foreign-library-function libbottle "bottle_contents"
                                                #:return-type '*
                                                #:arg-types  '(*))))
        (lambda (b)
          "Return the contents of B."
          (pointer->string (contents (unwrap-bottle b))))))

    (write (grab-bottle))
    ⇒ #<bottle of Château Haut-Brion 803d36>
    ```

    In this example, `grab-bottle` is guaranteed to return a genuine
    `bottle` object satisfying `bottle?`. Likewise,
    `bottle-contents` errors out when its argument is not a genuine
    `bottle` object.

As another example, currently Guile has a variable, `scm_numptob`,
as part of its API. It is declared as a C `long`. So, to read its
value, we can do:

```
(use-modules (system foreign))
(use-modules (rnrs bytevectors))
(define numptob
  (foreign-library-pointer #f "scm_numptob"))
numptob
(bytevector-uint-ref (pointer->bytevector numptob (sizeof long))
                     0 (native-endianness)
                     (sizeof long))
⇒ 8
```

If we wanted to corrupt Guile’s internal state, we could set
`scm_numptob` to another value; but we shouldn’t, because that
variable is not meant to be set. Indeed this point applies more widely:
the C API is a dangerous place to be. Not only might setting a value
crash your program, simply accessing the data pointed to by a dangling
pointer or similar can prove equally disastrous.

---

Next: [More Foreign Functions](#More-Foreign-Functions), Previous: [Void Pointers and Byte Access](#Void-Pointers-and-Byte-Access), Up: [Foreign Function Interface](#Foreign-Function-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.19.7 Foreign Structs [¶](#Foreign-Structs-1)

Finally, one last note on foreign values before moving on to actually
calling foreign functions. Sometimes you need to deal with C structs,
which requires interpreting each element of the struct according to the
its type, offset, and alignment. The `(system foreign)` module has
some primitives to support this.

```
(use-modules (system foreign))
```

Scheme Procedure: **sizeof** type [¶](#index-sizeof)

C Function: **scm\_sizeof** (type) [¶](#index-scm_005fsizeof)
:   Return the size of type, in bytes.

    type should be a valid C type, like `int`.
    Alternately type may be the symbol `*`, in which
    case the size of a pointer is returned. type may
    also be a list of types, in which case the size of a
    `struct` with ABI-conventional packing is returned.

Scheme Procedure: **alignof** type [¶](#index-alignof)

C Function: **scm\_alignof** (type) [¶](#index-scm_005falignof)
:   Return the alignment of type, in bytes.

    type should be a valid C type, like `int`.
    Alternately type may be the symbol `*`, in which
    case the alignment of a pointer is returned. type may
    also be a list of types, in which case the alignment of a
    `struct` with ABI-conventional packing is returned.

Guile also provides some convenience syntax to efficiently read and
write C structs to and from bytevectors.

Scheme Syntax: **read-c-struct** bv offset ((field type) …) k [¶](#index-read_002dc_002dstruct)
:   Read a C struct with fields of type type... from the bytevector
    bv, at offset offset. Bind the fields to the identifiers
    field..., and return `(k field ...)`.

    Unless cross-compiling, the field types are evaluated at macro-expansion
    time. This allows the resulting bytevector accessors and size/alignment
    computations to be completely inlined.

Scheme Syntax: **write-c-struct** bv offset ((field type) …) [¶](#index-write_002dc_002dstruct)
:   Write a C struct with fields field... of type type... to the bytevector
    bv, at offset offset. Return zero values.

    Like `write-c-struct` above, unless cross-compiling, the field
    types are evaluated at macro-expansion time.

For example, to define a parser and serializer for the equivalent of a
`struct { int64_t a; uint8_t b; }`, one might do this:

```
(use-modules (system foreign) (rnrs bytevectors))

(define-syntax-rule
    (define-serialization (reader writer) (field type) ...)
  (begin
    (define (reader bv offset)
      (read-c-struct bv offset ((field type) ...) values))
    (define (writer bv offset field ...)
      (write-c-struct bv offset ((field type) ...)))))

(define-serialization (read-struct write-struct)
  (a int64) (b uint8))

(define bv (make-bytevector (sizeof (list int64 uint8))))

(write-struct bv 0 300 43)
(call-with-values (lambda () (read-struct bv 0))
  list)
⇒ (300 43)
```

There is also an older interface that is mostly equivalent to
`read-c-struct` and `write-c-struct`, but which uses run-time
dispatch, and operates on foreign pointers instead of bytevectors.

Scheme Procedure: **parse-c-struct** foreign types [¶](#index-parse_002dc_002dstruct)
:   Parse a foreign pointer to a C struct, returning a list of values.

    `types` should be a list of C types.

Our parser and serializer example for `struct { int64_t a; uint8_t
b; }` looks more like this:

```
(parse-c-struct (make-c-struct (list int64 uint8)
                               (list 300 43))
                (list int64 uint8))
⇒ (300 43)
```

As yet, Guile only has convenience routines to support
conventionally-packed structs. But given the `bytevector->pointer`
and `pointer->bytevector` routines, one can create and parse
tightly packed structs and unions by hand. See the code for
`(system foreign)` for details.

---

Previous: [Foreign Structs](#Foreign-Structs), Up: [Foreign Function Interface](#Foreign-Function-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.19.8 More Foreign Functions [¶](#More-Foreign-Functions-1)

It is possible to pass pointers to foreign functions, and to return them
as well. In that case the type of the argument or return value should
be the symbol `*`, indicating a pointer. For example, the following
code makes `memcpy` available to Scheme:

```
(use-modules (system foreign))
(define memcpy
  (foreign-library-function #f "memcpy"
                            #:return-type '*
                            #:arg-types (list '* '* size_t)))
```

To invoke `memcpy`, one must pass it foreign pointers:

```
(use-modules (rnrs bytevectors))

(define src-bits
  (u8-list->bytevector '(0 1 2 3 4 5 6 7)))
(define src
  (bytevector->pointer src-bits))
(define dest
  (bytevector->pointer (make-bytevector 16 0)))

(memcpy dest src (bytevector-length src-bits))

(bytevector->u8-list (pointer->bytevector dest 16))
⇒ (0 1 2 3 4 5 6 7 0 0 0 0 0 0 0 0)
```

One may also pass structs as values, passing structs as foreign
pointers. See [Foreign Structs](#Foreign-Structs), for more information on how to express
struct types and struct values.

“Out” arguments are passed as foreign pointers. The memory pointed to
by the foreign pointer is mutated in place.

```
;; struct timeval {
;;      time_t      tv_sec;     /* seconds */
;;      suseconds_t tv_usec;    /* microseconds */
;; };
;; assuming fields are of type "long"

(define gettimeofday
  (let ((f (foreign-library-function #f "gettimeofday"
                                     #:return-type int
                                     #:arg-types (list '* '*)))
        (tv-type (list long long)))
    (lambda ()
      (let* ((timeval (make-c-struct tv-type (list 0 0)))
             (ret (f timeval %null-pointer)))
        (if (zero? ret)
            (apply values (parse-c-struct timeval tv-type))
            (error "gettimeofday returned an error" ret))))))

(gettimeofday)    
⇒ 1270587589
⇒ 499553
```

As you can see, this interface to foreign functions is at a very low,
somewhat dangerous level[22](#FOOT22).

The FFI can also work in the opposite direction: making Scheme
procedures callable from C. This makes it possible to use Scheme
procedures as “callbacks” expected by C function.

Scheme Procedure: **procedure->pointer** return-type proc arg-types [¶](#index-procedure_002d_003epointer)

C Function: **scm\_procedure\_to\_pointer** (return\_type, proc, arg\_types) [¶](#index-scm_005fprocedure_005fto_005fpointer)
:   Return a pointer to a C function of type return-type
    taking arguments of types arg-types (a list) and
    behaving as a proxy to procedure proc. Thus
    proc’s arity, supported argument types, and return
    type should match return-type and arg-types.

As an example, here’s how the C library’s `qsort` array sorting
function can be made accessible to Scheme (see [`qsort`](/libc/latest/en/libc.html#Array-Sort-Function) in The GNU C Library Reference Manual):

```
(define qsort!
  (let ((qsort (foreign-library-function
                #f "qsort" #:arg-types (list '* size_t size_t '*))))
    (lambda (bv compare)
      ;; Sort bytevector BV in-place according to comparison
      ;; procedure COMPARE.
      (let ((ptr (procedure->pointer int
                                     (lambda (x y)
                                       ;; X and Y are pointers so,
                                       ;; for convenience, dereference
                                       ;; them before calling COMPARE.
                                       (compare (dereference-uint8* x)
                                                (dereference-uint8* y)))
                                     (list '* '*))))
        (qsort (bytevector->pointer bv)
               (bytevector-length bv) 1 ;; we're sorting bytes
               ptr)))))

(define (dereference-uint8* ptr)
  ;; Helper function: dereference the byte pointed to by PTR.
  (let ((b (pointer->bytevector ptr 1)))
    (bytevector-u8-ref b 0)))

(define bv
  ;; An unsorted array of bytes.
  (u8-list->bytevector '(7 1 127 3 5 4 77 2 9 0)))

;; Sort BV.
(qsort! bv (lambda (x y) (- x y)))

;; Let's see what the sorted array looks like:
(bytevector->u8-list bv)
⇒ (0 1 2 3 4 5 7 9 77 127)
```

And voilà!

Note that `procedure->pointer` is not supported (and not defined)
on a few exotic architectures. Thus, user code may need to check
`(defined? 'procedure->pointer)`. Nevertheless, it is available on
many architectures, including (as of libffi 3.0.9) x86, ia64, SPARC,
PowerPC, ARM, and MIPS, to name a few.

---

Next: [Smobs](#Smobs), Previous: [Foreign Function Interface](#Foreign-Function-Interface), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.20 Foreign Objects [¶](#Foreign-Objects-1)

This chapter contains reference information related to defining and
working with foreign objects. See [Defining New Foreign Object Types](#Defining-New-Foreign-Object-Types),
for a tutorial-like introduction to foreign objects.

C Type: **scm\_t\_struct\_finalize** [¶](#index-scm_005ft_005fstruct_005ffinalize)
:   This function type returns `void` and takes one `SCM`
    argument.

C Function: `SCM` **scm\_make\_foreign\_object\_type** `(SCM name, SCM slots, scm_t_struct_finalize finalizer)` [¶](#index-scm_005fmake_005fforeign_005fobject_005ftype)
:   Create a fresh foreign object type. name is a symbol naming the
    type. slots is a list of symbols, each one naming a field in the
    foreign object type. finalizer indicates the finalizer, and may
    be `NULL`.

We recommend that finalizers be avoided if possible. See [Foreign Object Memory Management](#Foreign-Object-Memory-Management). Finalizers must be async-safe and
thread-safe. Again, see [Foreign Object Memory Management](#Foreign-Object-Memory-Management). If you
are embedding Guile in an application that is not thread-safe, and you
define foreign object types that need finalization, you might want to
disable automatic finalization, and arrange to call
`scm_manually_run_finalizers ()` yourself.

C Function: `int` **scm\_set\_automatic\_finalization\_enabled** `(int enabled_p)` [¶](#index-scm_005fset_005fautomatic_005ffinalization_005fenabled)
:   Enable or disable automatic finalization. By default, Guile arranges to
    invoke object finalizers automatically, in a separate thread if
    possible. Passing a zero value for enabled\_p will disable
    automatic finalization for Guile as a whole. If you disable automatic
    finalization, you will have to call `scm_run_finalizers ()`
    periodically.

    Unlike most other Guile functions, you can call
    `scm_set_automatic_finalization_enabled` before Guile has been
    initialized.

    Return the previous status of automatic finalization.

C Function: `int` **scm\_run\_finalizers** `(void)` [¶](#index-scm_005frun_005ffinalizers)
:   Invoke any pending finalizers. Returns the number of finalizers that
    were invoked. This function should be called when automatic
    finalization is disabled, though it may be called if it is enabled as
    well.

C Function: `void` **scm\_assert\_foreign\_object\_type** `(SCM type, SCM val)` [¶](#index-scm_005fassert_005fforeign_005fobject_005ftype)
:   When val is a foreign object of the given type, do nothing.
    Otherwise, signal an error.

C Function: `SCM` **scm\_make\_foreign\_object\_0** `(SCM type)` [¶](#index-scm_005fmake_005fforeign_005fobject_005f0)

C Function: `SCM` **scm\_make\_foreign\_object\_1** `(SCM type, void *val0)` [¶](#index-scm_005fmake_005fforeign_005fobject_005f1)

C Function: `SCM` **scm\_make\_foreign\_object\_2** `(SCM type, void *val0, void *val1)` [¶](#index-scm_005fmake_005fforeign_005fobject_005f2)

C Function: `SCM` **scm\_make\_foreign\_object\_3** `(SCM type, void *val0, void *val1, void *val2)` [¶](#index-scm_005fmake_005fforeign_005fobject_005f3)

C Function: `SCM` **scm\_make\_foreign\_object\_n** `(SCM type, size_t n, void *vals[])` [¶](#index-scm_005fmake_005fforeign_005fobject_005fn)
:   Make a new foreign object of the type with type type and
    initialize the first n fields to the given values, as appropriate.

    The number of fields for objects of a given type is fixed when the type
    is created. It is an error to give more initializers than there are
    fields in the value. It is perfectly fine to give fewer initializers
    than needed; this is convenient when some fields are of non-pointer
    types, and would be easier to initialize with the setters described
    below.

C Function: `void*` **scm\_foreign\_object\_ref** `(SCM obj, size_t n);` [¶](#index-scm_005fforeign_005fobject_005fref)

C Function: `scm_t_bits` **scm\_foreign\_object\_unsigned\_ref** `(SCM obj, size_t n);` [¶](#index-scm_005fforeign_005fobject_005funsigned_005fref)

C Function: `scm_t_signed_bits` **scm\_foreign\_object\_signed\_ref** `(SCM obj, size_t n);` [¶](#index-scm_005fforeign_005fobject_005fsigned_005fref)
:   Return the value of the nth field of the foreign object obj.
    The backing store for the fields is as wide as a `scm_t_bits`
    value, which is at least as wide as a pointer. The different variants
    handle casting in a portable way.

C Function: `void` **scm\_foreign\_object\_set\_x** `(SCM obj, size_t n, void *val);` [¶](#index-scm_005fforeign_005fobject_005fset_005fx)

C Function: `void` **scm\_foreign\_object\_unsigned\_set\_x** `(SCM obj, size_t n, scm_t_bits val);` [¶](#index-scm_005fforeign_005fobject_005funsigned_005fset_005fx)

C Function: `void` **scm\_foreign\_object\_signed\_set\_x** `(SCM obj, size_t n, scm_t_signed_bits val);` [¶](#index-scm_005fforeign_005fobject_005fsigned_005fset_005fx)
:   Set the value of the nth field of the foreign object obj to
    val, after portably converting to a `scm_t_bits` value, if
    needed.

One can also access foreign objects from Scheme. See [Foreign Objects and Scheme](#Foreign-Objects-and-Scheme), for some examples.

```
(use-modules (system foreign-object))
```

Scheme Procedure: **make-foreign-object-type** name slots [#:finalizer=#f] [#:supers=’()] [¶](#index-make_002dforeign_002dobject_002dtype)
:   Make a new foreign object type. See the above documentation for
    `scm_make_foreign_object_type`; these functions are exactly
    equivalent, except for the way in which the finalizer gets attached to
    instances (an internal detail), and the fact that this function accepts
    an optional list of superclasses, which will be paseed to
    `make-class`.

    The resulting value is a GOOPS class. See [GOOPS](#GOOPS), for more on classes
    in Guile.

Scheme Syntax: **define-foreign-object-type** name constructor (slot ...) [#:finalizer=#f] [¶](#index-define_002dforeign_002dobject_002dtype)
:   A convenience macro to define a type, using
    `make-foreign-object-type`, and bind it to name. A
    constructor will be bound to constructor, and getters will be
    bound to each of slot....

---

Next: [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling), Previous: [Foreign Objects](#Foreign-Objects), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.21 Smobs [¶](#Smobs-1)

A *smob* is a “small object”. Before foreign objects were
introduced in Guile 2.0.12 (see [Foreign Objects](#Foreign-Objects)), smobs were the
preferred way to for C code to define new kinds of Scheme objects. With
the exception of the so-called “applicable SMOBs” discussed below,
smobs are now a legacy interface and are headed for eventual
deprecation. See [Deprecation](#Deprecation). New code should use the foreign
object interface.

This section contains reference information related to defining and
working with smobs. For a tutorial-like introduction to smobs, see
“Defining New Types (Smobs)” in previous versions of this manual.

Function: `scm_t_bits` **scm\_make\_smob\_type** `(const char *name, size_t size)` [¶](#index-scm_005fmake_005fsmob_005ftype)
:   This function adds a new smob type, named name, with instance size
    size, to the system. The return value is a tag that is used in
    creating instances of the type.

    If size is 0, the default *free* function will do nothing.

    If size is not 0, the default *free* function will
    deallocate the memory block pointed to by `SCM_SMOB_DATA` with
    `scm_gc_free`. The what parameter in the call to
    `scm_gc_free` will be name.

    Default values are provided for the *mark*, *free*,
    *print*, and *equalp* functions. If you want to customize any
    of these functions, the call to `scm_make_smob_type` should be
    immediately followed by calls to one or several of
    `scm_set_smob_mark`, `scm_set_smob_free`,
    `scm_set_smob_print`, and/or `scm_set_smob_equalp`.

C Function: `void` **scm\_set\_smob\_free** `(scm_t_bits tc, size_t (*free) (SCM obj))` [¶](#index-scm_005fset_005fsmob_005ffree)
:   This function sets the smob freeing procedure (sometimes referred to as
    a *finalizer*) for the smob type specified by the tag
    tc. tc is the tag returned by `scm_make_smob_type`.

    The free procedure must deallocate all resources that are
    directly associated with the smob instance obj. It must assume
    that all `SCM` values that it references have already been freed
    and are thus invalid.

    It must also not call any libguile function or macro except
    `scm_gc_free`, `SCM_SMOB_FLAGS`, `SCM_SMOB_DATA`,
    `SCM_SMOB_DATA_2`, and `SCM_SMOB_DATA_3`.

    The free procedure must return 0.

    Note that defining a freeing procedure is not necessary if the resources
    associated with obj consists only of memory allocated with
    `scm_gc_malloc` or `scm_gc_malloc_pointerless` because this
    memory is automatically reclaimed by the garbage collector when it is no
    longer needed (see [`scm_gc_malloc`](#Memory-Blocks)).

Smob free functions must be thread-safe. See [Foreign Object Memory Management](#Foreign-Object-Memory-Management), for a discussion on finalizers and concurrency. If you are
embedding Guile in an application that is not thread-safe, and you
define smob types that need finalization, you might want to disable
automatic finalization, and arrange to call
`scm_manually_run_finalizers ()` yourself. See [Foreign Objects](#Foreign-Objects).

C Function: `void` **scm\_set\_smob\_mark** `(scm_t_bits tc, SCM (*mark) (SCM obj))` [¶](#index-scm_005fset_005fsmob_005fmark)
:   This function sets the smob marking procedure for the smob type specified by
    the tag tc. tc is the tag returned by `scm_make_smob_type`.

    Defining a marking procedure is almost always the wrong thing to do. It
    is much, much preferable to allocate smob data with the
    `scm_gc_malloc` and `scm_gc_malloc_pointerless` functions, and
    allow the GC to trace pointers automatically.

    Any mark procedures you see currently almost surely date from the time
    of Guile 1.8, before the switch to the Boehm-Demers-Weiser collector.
    Such smob implementations should be changed to just use
    `scm_gc_malloc` and friends, and to lose their mark function.

    If you decide to keep the mark function, note that it may be called on
    objects that are on the free list. Please read and digest the comments
    from the BDW GC’s `gc/gc_mark.h` header.

    The mark procedure must cause `scm_gc_mark` to be called
    for every `SCM` value that is directly referenced by the smob
    instance obj. One of these `SCM` values can be returned
    from the procedure and Guile will call `scm_gc_mark` for it.
    This can be used to avoid deep recursions for smob instances that form
    a list.

    It must not call any libguile function or macro except
    `scm_gc_mark`, `SCM_SMOB_FLAGS`, `SCM_SMOB_DATA`,
    `SCM_SMOB_DATA_2`, and `SCM_SMOB_DATA_3`.

C Function: `void` **scm\_set\_smob\_print** `(scm_t_bits tc, int (*print) (SCM obj, SCM port, scm_print_state* pstate))` [¶](#index-scm_005fset_005fsmob_005fprint)
:   This function sets the smob printing procedure for the smob type
    specified by the tag tc. tc is the tag returned by
    `scm_make_smob_type`.

    The print procedure should output a textual representation of
    the smob instance obj to port, using information in
    pstate.

    The textual representation should be of the form `#<name ...>`.
    This ensures that `read` will not interpret it as some other
    Scheme value.

    It is often best to ignore pstate and just print to port
    with `scm_display`, `scm_write`, `scm_simple_format`,
    and `scm_puts`.

C Function: `void` **scm\_set\_smob\_equalp** `(scm_t_bits tc, SCM (*equalp) (SCM obj1, SCM obj2))` [¶](#index-scm_005fset_005fsmob_005fequalp)
:   This function sets the smob equality-testing predicate for the smob
    type specified by the tag tc. tc is the tag returned by
    `scm_make_smob_type`.

    The equalp procedure should return `SCM_BOOL_T` when
    obj1 is `equal?` to obj2. Else it should return
    `SCM_BOOL_F`. Both obj1 and obj2 are instances of the
    smob type tc.

C Function: `void` **scm\_assert\_smob\_type** `(scm_t_bits tag, SCM val)` [¶](#index-scm_005fassert_005fsmob_005ftype)
:   When val is a smob of the type indicated by tag, do nothing.
    Else, signal an error.

C Macro: `int` **SCM\_SMOB\_PREDICATE** `(scm_t_bits tag, SCM exp)` [¶](#index-SCM_005fSMOB_005fPREDICATE)
:   Return true if exp is a smob instance of the type indicated by
    tag, or false otherwise. The expression exp can be
    evaluated more than once, so it shouldn’t contain any side effects.

C Function: `SCM` **scm\_new\_smob** `(scm_t_bits tag, void *data)` [¶](#index-scm_005fnew_005fsmob)

C Function: `SCM` **scm\_new\_double\_smob** `(scm_t_bits tag, void *data, void *data2, void *data3)` [¶](#index-scm_005fnew_005fdouble_005fsmob)
:   Make a new smob of the type with tag tag and smob data data,
    data2, and data3, as appropriate.

    The tag is what has been returned by `scm_make_smob_type`.
    The initial values data, data2, and data3 are of
    type `scm_t_bits`; when you want to use them for `SCM`
    values, these values need to be converted to a `scm_t_bits` first
    by using `SCM_UNPACK`.

    The flags of the smob instance start out as zero.

C Macro: `scm_t_bits` **SCM\_SMOB\_FLAGS** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fFLAGS)
:   Return the 16 extra bits of the smob obj. No meaning is
    predefined for these bits, you can use them freely.

C Macro: `scm_t_bits` **SCM\_SET\_SMOB\_FLAGS** `(SCM obj, scm_t_bits flags)` [¶](#index-SCM_005fSET_005fSMOB_005fFLAGS)
:   Set the 16 extra bits of the smob obj to flags. No
    meaning is predefined for these bits, you can use them freely.

C Macro: `scm_t_bits` **SCM\_SMOB\_DATA** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fDATA)

C Macro: `scm_t_bits` **SCM\_SMOB\_DATA\_2** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fDATA_005f2)

C Macro: `scm_t_bits` **SCM\_SMOB\_DATA\_3** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fDATA_005f3)
:   Return the first (second, third) immediate word of the smob obj
    as a `scm_t_bits` value. When the word contains a `SCM`
    value, use `SCM_SMOB_OBJECT` (etc.) instead.

C Macro: `void` **SCM\_SET\_SMOB\_DATA** `(SCM obj, scm_t_bits val)` [¶](#index-SCM_005fSET_005fSMOB_005fDATA)

C Macro: `void` **SCM\_SET\_SMOB\_DATA\_2** `(SCM obj, scm_t_bits val)` [¶](#index-SCM_005fSET_005fSMOB_005fDATA_005f2)

C Macro: `void` **SCM\_SET\_SMOB\_DATA\_3** `(SCM obj, scm_t_bits val)` [¶](#index-SCM_005fSET_005fSMOB_005fDATA_005f3)
:   Set the first (second, third) immediate word of the smob obj to
    val. When the word should be set to a `SCM` value, use
    `SCM_SMOB_SET_OBJECT` (etc.) instead.

C Macro: `SCM` **SCM\_SMOB\_OBJECT** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fOBJECT)

C Macro: `SCM` **SCM\_SMOB\_OBJECT\_2** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fOBJECT_005f2)

C Macro: `SCM` **SCM\_SMOB\_OBJECT\_3** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fOBJECT_005f3)
:   Return the first (second, third) immediate word of the smob obj
    as a `SCM` value. When the word contains a `scm_t_bits`
    value, use `SCM_SMOB_DATA` (etc.) instead.

C Macro: `void` **SCM\_SET\_SMOB\_OBJECT** `(SCM obj, SCM val)` [¶](#index-SCM_005fSET_005fSMOB_005fOBJECT)

C Macro: `void` **SCM\_SET\_SMOB\_OBJECT\_2** `(SCM obj, SCM val)` [¶](#index-SCM_005fSET_005fSMOB_005fOBJECT_005f2)

C Macro: `void` **SCM\_SET\_SMOB\_OBJECT\_3** `(SCM obj, SCM val)` [¶](#index-SCM_005fSET_005fSMOB_005fOBJECT_005f3)
:   Set the first (second, third) immediate word of the smob obj to
    val. When the word should be set to a `scm_t_bits` value, use
    `SCM_SMOB_SET_DATA` (etc.) instead.

C Macro: `SCM *` **SCM\_SMOB\_OBJECT\_LOC** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fOBJECT_005fLOC)

C Macro: `SCM *` **SCM\_SMOB\_OBJECT\_2\_LOC** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fOBJECT_005f2_005fLOC)

C Macro: `SCM *` **SCM\_SMOB\_OBJECT\_3\_LOC** `(SCM obj)` [¶](#index-SCM_005fSMOB_005fOBJECT_005f3_005fLOC)
:   Return a pointer to the first (second, third) immediate word of the
    smob obj. Note that this is a pointer to `SCM`. If you
    need to work with `scm_t_bits` values, use `SCM_PACK` and
    `SCM_UNPACK`, as appropriate.

Function: `SCM` **scm\_markcdr** `(SCM x)` [¶](#index-scm_005fmarkcdr)
:   Mark the references in the smob x, assuming that x’s first
    data word contains an ordinary Scheme object, and x refers to no
    other objects. This function simply returns x’s first data word.

---

Next: [Configuration, Features and Runtime Options](#Options-and-Config), Previous: [Smobs](#Smobs), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
