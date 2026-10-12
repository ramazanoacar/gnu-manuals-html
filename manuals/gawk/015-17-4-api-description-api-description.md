### 17.4 API Description [¶](#API-Description)

C or C++ code for an extension must include the header file
`gawkapi.h`, which declares the functions and defines the data
types used to communicate with `gawk`.
This (rather large) section describes the API in detail.

* [Introduction](#Extension-API-Functions-Introduction)
* [General-Purpose Data Types](#General-Data-Types)
* [Memory Allocation Functions and Convenience Macros](#Memory-Allocation-Functions)
* [Constructor Functions](#Constructor-Functions)
* [Managing MPFR and GMP Values](#API-Ownership-of-MPFR-and-GMP-Values)
* [Registration Functions](#Registration-Functions)
* [Printing Messages](#Printing-Messages)
* [Updating `ERRNO`](#Updating-ERRNO)
* [Requesting Values](#Requesting-Values)
* [Accessing and Updating Parameters](#Accessing-Parameters)
* [Symbol Table Access](#Symbol-Table-Access)
* [Array Manipulation](#Array-Manipulation)
* [Accessing and Manipulating Redirections](#Redirection-API)
* [API Variables](#Extension-API-Variables)
* [Boilerplate Code](#Extension-API-Boilerplate)
* [Changes From Version 1 of the API](#Changes-from-API-V1)

---

Next: [General-Purpose Data Types](#General-Data-Types), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.1 Introduction [¶](#Introduction-1)

Access to facilities within `gawk` is achieved
by calling through function pointers passed into your extension.

API function pointers are provided for the following kinds of operations:

* Allocating, reallocating, and releasing memory.
* Registration functions. You may register:
  + Extension functions
  + Exit callbacks
  + A version string
  + Input parsers
  + Output wrappers
  + Two-way processors

  All of these are discussed in detail later in this chapter.
* Printing fatal, warning, and “lint” warning messages.
* Updating `ERRNO`, or unsetting it.
* Accessing parameters, including converting an undefined parameter into
  an array.
* Symbol table access: retrieving a global variable, creating one,
  or changing one.
* Creating and releasing cached values; this provides an
  efficient way to use values for multiple variables and
  can be a big performance win.
* Manipulating arrays:
  + Retrieving, adding, deleting, and modifying elements
  + Getting the count of elements in an array
  + Creating a new array
  + Clearing an array
  + Flattening an array for easy C-style looping over all its indices and elements
* Accessing and manipulating redirections.

Some points about using the API:

* The following types, macros, and/or functions are referenced
  in `gawkapi.h`. For correct use, you must therefore include the
  corresponding standard header file *before* including `gawkapi.h`.
  The list of macros and related header files is shown in [Table 17.1](#table_002dapi_002dstd_002dheaders).

  | C entity | Header file |
  | --- | --- |
  | `EOF` | `<stdio.h>` |
  | Values for `errno` | `<errno.h>` |
  | `FILE` | `<stdio.h>` |
  | `NULL` | `<stddef.h>` |
  | `memcpy()` | `<string.h>` |
  | `memset()` | `<string.h>` |
  | `size_t` | `<sys/types.h>` |
  | `struct stat` | `<sys/stat.h>` |

  **Table 17.1:** Standard header files needed by API

  Due to portability concerns, especially to systems that are not
  fully standards-compliant, it is your responsibility
  to include the correct files in the correct way. This requirement
  is necessary in order to keep `gawkapi.h` clean, instead of becoming
  a portability hodge-podge as can be seen in some parts of
  the `gawk` source code.
* If your extension uses MPFR facilities, and you wish to receive such
  values from `gawk` and/or pass such values to it, you must include the
  `<mpfr.h>` header before including `<gawkapi.h>`.
* The `gawkapi.h` file may be included more than once without ill effect.
  Doing so, however, is poor coding practice.
* Although the API only uses ISO C 90 features, there is an exception; the
  “constructor” functions use the `inline` keyword. If your compiler
  does not support this keyword, you should either place
  ‘`-Dinline=''`’ on your command line or use the GNU Autotools and include a
  `config.h` file in your extensions.
* All pointers filled in by `gawk` point to memory
  managed by `gawk` and should be treated by the extension as
  read-only.

  Memory for *all* strings passed into `gawk`
  from the extension *must* come from calling one of
  `gawk_malloc()`, `gawk_calloc()`, or `gawk_realloc()`,
  and is managed by `gawk` from then on.

  Memory for MPFR/GMP values that come from `gawk`
  should also be treated as read-only. However, unlike strings,
  memory for MPFR/GMP values allocated by an extension and passed
  into `gawk` is *copied* by `gawk`; the extension
  should then free the values itself to avoid memory leaks. This is
  discussed further in [Managing MPFR and GMP Values](#API-Ownership-of-MPFR-and-GMP-Values).
* The API defines several simple `struct`s that map values as seen
  from `awk`. A value can be a `double`, a string, or an
  array (as in multidimensional arrays, or when creating a new array).

  String values maintain both pointer and length, because embedded NUL
  characters are allowed.

  > **NOTE:** By intent, `gawk` maintains strings using the current multibyte
  > encoding (as defined by `LC_xxx` environment variables)
  > and not using wide characters. This matches how `gawk` stores
  > strings internally and also how characters are likely to be input into
  > and output from files.

  > **NOTE:** String values passed to an extension by `gawk` are always
  > NUL-terminated. Thus it is safe to pass such string values to
  > standard library and system routines. However, because `gawk`
  > allows embedded NUL characters in string data, before using the data
  > as a regular C string, you should check that the length for that string
  > passed to the extension matches the return value of `strlen()`
  > for it.
* When retrieving a value (such as a parameter or that of a global variable
  or array element), the extension requests a specific type (number, string,
  scalar, value cookie, array, or “undefined”). When the request is
  “undefined,” the returned value will have the real underlying type.

  However, if the request and actual type don’t match, the access function
  returns “false” and fills in the type of the actual value that is there,
  so that the extension can, e.g., print an error message
  (such as “scalar passed where array expected”).

You may call the API functions by using the function pointers
directly, but the interface is not so pretty. To make extension code look
more like regular code, the `gawkapi.h` header file defines several
macros that you should use in your code. This section presents
the macros as if they were functions.

---

Next: [Memory Allocation Functions and Convenience Macros](#Memory-Allocation-Functions), Previous: [Introduction](#Extension-API-Functions-Introduction), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.2 General-Purpose Data Types [¶](#General_002dPurpose-Data-Types)

> *I have a true love/hate relationship with unions.*

— *Arnold Robbins*

> *That’s the thing about unions: the compiler will arrange things so they
> can accommodate both love and hate.*

— *Chet Ramey*

The extension API defines a number of simple types and structures for
general-purpose use. Additional, more specialized, data structures are
introduced in subsequent sections, together with the functions
that use them.

The general-purpose types and structures are as follows:

`typedef void *awk_ext_id_t;`
:   A value of this type is received from `gawk` when an extension is loaded.
    That value must then be passed back to `gawk` as the first parameter of
    each API function.

`#define awk_const …`
:   This macro expands to ‘`const`’ when compiling an extension,
    and to nothing when compiling `gawk` itself. This makes
    certain fields in the API data structures unwritable from extension code,
    while allowing `gawk` to use them as it needs to.

`typedef enum awk_bool {`

`awk_false = 0,`

`awk_true`

`} awk_bool_t;`
:   A simple Boolean type.

`typedef struct awk_string {`

`char *str;      /* data */`

`size_t len;     /* length thereof, in chars */`

`} awk_string_t;`
:   This represents a mutable string. `gawk`
    owns the memory pointed to if it supplied
    the value. Otherwise, it takes ownership of the memory pointed to.
    *Such memory must come from calling one of the
    `gawk_malloc()`, `gawk_calloc()`, or
    `gawk_realloc()` functions!*

    As mentioned earlier, strings are maintained using the current
    multibyte encoding.

`typedef enum {`

`AWK_UNDEFINED,`

`AWK_NUMBER,`

`AWK_STRING,`

`AWK_REGEX,`

`AWK_STRNUM,`

`AWK_ARRAY,`

`AWK_SCALAR,         /* opaque access to a variable */`

`AWK_VALUE_COOKIE,   /* for updating a previously created value */`

`AWK_BOOL`

`} awk_valtype_t;`
:   This `enum` indicates the type of a value.
    It is used in the following `struct`.

`typedef struct awk_value {`

`awk_valtype_t val_type;`

`union {`

`awk_string_t       s;`

`awknum_t           n;`

`awk_array_t        a;`

`awk_scalar_t       scl;`

`awk_value_cookie_t vc;`

`awk_bool_t         b;`

`} u;`

`} awk_value_t;`
:   An “`awk` value.”
    The `val_type` member indicates what kind of value the
    `union` holds, and each member is of the appropriate type.

`#define str_value      u.s`

`#define strnum_value   str_value`

`#define regex_value    str_value`

`#define num_value      u.n.d`

`#define num_type       u.n.type`

`#define num_ptr        u.n.ptr`

`#define array_cookie   u.a`

`#define scalar_cookie  u.scl`

`#define value_cookie   u.vc`

`#define bool_value     u.b`
:   Using these macros makes accessing the fields of the `awk_value_t` more
    readable.

`enum AWK_NUMBER_TYPE {`

`AWK_NUMBER_TYPE_DOUBLE,`

`AWK_NUMBER_TYPE_MPFR,`

`AWK_NUMBER_TYPE_MPZ`

`};`
:   This `enum` is used in the following structure for defining the
    type of numeric value that is being worked with. It is declared at the
    top level of the file so that it works correctly for C++ as well as for C.

`typedef struct awk_number {`

`double d;`

`enum AWK_NUMBER_TYPE type;`

`void *ptr;`

`} awk_number_t;`
:   This represents a numeric value. Internally, `gawk` stores
    every number as either a C `double`, a GMP integer, or an MPFR
    arbitrary-precision floating-point value. In order to allow extensions
    to also support GMP and MPFR values, numeric values are passed in this
    structure.

    The double-precision `d` element is always populated
    in data received from `gawk`. In addition, by examining the
    `type` member, an extension can determine if the `ptr`
    member is either a GMP integer (type `mpz_ptr`), or an MPFR
    floating-point value (type `mpfr_ptr_t`), and cast it appropriately.

    > **CAUTION:** Any MPFR or MPZ values that you create and pass to `gawk`
    > to save are *copied*. This means you are responsible to release
    > the storage once you’re done with it. See the sample `intdiv`
    > extension for some example code.

`typedef void *awk_scalar_t;`
:   Scalars can be represented as an opaque type. These values are obtained
    from `gawk` and then passed back into it. This is discussed
    in a general fashion in the text following this list, and in more detail in
    [Variable Access and Update by Cookie](#Symbol-table-by-cookie).

`typedef void *awk_value_cookie_t;`
:   A “value cookie” is an opaque type representing a cached value.
    This is also discussed in a general fashion in the text following this list,
    and in more detail in [Creating and Using Cached Values](#Cached-values).

Scalar values in `awk` are numbers, strings, strnums, or typed regexps. The
`awk_value_t` struct represents values. The `val_type` member
indicates what is in the `union`.

Representing numbers is easy—the API uses a C `double`. Strings
require more work. Because `gawk` allows embedded NUL bytes
in string values, a string must be represented as a pair containing a
data pointer and length. This is the `awk_string_t` type.

A strnum (numeric string) value is represented as a string and consists
of user input data that appears to be numeric.
When an extension creates a strnum value, the result is a string flagged
as user input. Subsequent parsing by `gawk` then determines whether it
looks like a number and should be treated as a strnum, or as a regular string.

This is useful in cases where an extension function would like to do something
comparable to the `split()` function which sets the strnum attribute
on the array elements it creates. For example, an extension that implements
CSV splitting would want to use this feature. This is also useful for a
function that retrieves a data item from a database. The PostgreSQL
`PQgetvalue()` function, for example, returns a string that may be numeric
or textual depending on the contents.

Typed regexp values (see [Strongly Typed Regexp Constants](#Strong-Regexp-Constants)) are not of
much use to extension functions. Extension functions can tell that
they’ve received them, and create them for scalar values. Otherwise,
they can examine the text of the regexp through `regex_value.str`
and `regex_value.len`.

Identifiers (i.e., the names of global variables) can be associated
with either scalar values or with arrays. In addition, `gawk`
provides true arrays of arrays, where any given array element can
itself be an array. Discussion of arrays is delayed until
[Array Manipulation](#Array-Manipulation).

The various macros listed earlier make it easier to use the elements
of the `union` as if they were fields in a `struct`; this
is a common coding practice in C. Such code is easier to write and to
read, but it remains *your* responsibility to make sure that
the `val_type` member correctly reflects the type of the value in
the `awk_value_t` struct.

Conceptually, the first three members of the `union` (number, string,
and array) are all that is needed for working with `awk` values.
However, because the API provides routines for accessing and changing
the value of a global scalar variable only by using the variable’s name,
there is a performance penalty: `gawk` must find the variable
each time it is accessed and changed. This turns out to be a real issue,
not just a theoretical one.

Thus, if you know that your extension will spend considerable time
reading and/or changing the value of one or more scalar variables, you
can obtain a *scalar cookie*[107](#FOOT107)
object for that variable, and then use
the cookie for getting the variable’s value or for changing the variable’s
value.
The `awk_scalar_t` type holds a scalar cookie, and the
`scalar_cookie` macro provides access to the value of that type
in the `awk_value_t` struct.
Given a scalar cookie, `gawk` can directly retrieve or
modify the value, as required, without having to find it first.

The `awk_value_cookie_t` type and `value_cookie` macro are similar.
If you know that you wish to
use the same numeric or string *value* for one or more variables,
you can create the value once, retaining a *value cookie* for it,
and then pass in that value cookie whenever you wish to set the value of a
variable. This saves storage space within the running `gawk`
process and reduces the time needed to create the value.

---

Next: [Constructor Functions](#Constructor-Functions), Previous: [General-Purpose Data Types](#General-Data-Types), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.3 Memory Allocation Functions and Convenience Macros [¶](#Memory-Allocation-Functions-and-Convenience-Macros)

The API provides a number of *memory allocation* functions for
allocating memory that can be passed to `gawk`, as well as a number of
convenience macros.
This subsection presents them all as function prototypes, in
the way that extension code would use them:

`void *gawk_malloc(size_t size);`
:   Call the correct version of `malloc()` to allocate storage that may
    be passed to `gawk`.

`void *gawk_calloc(size_t nmemb, size_t size);`
:   Call the correct version of `calloc()` to allocate storage that may
    be passed to `gawk`.

`void *gawk_realloc(void *ptr, size_t size);`
:   Call the correct version of `realloc()` to allocate storage that may
    be passed to `gawk`.

`void gawk_free(void *ptr);`
:   Call the correct version of `free()` to release storage that was
    allocated with `gawk_malloc()`, `gawk_calloc()`, or `gawk_realloc()`.

The API has to provide these functions because it is possible
for an extension to be compiled and linked against a different
version of the C library than was used for the `gawk`
executable.[108](#FOOT108) If `gawk` were
to use its version of `free()` when the memory came from an
unrelated version of `malloc()`, unexpected behavior would
likely result.

Three convenience macros may be used for allocating storage
from `gawk_malloc()`, `gawk_calloc`, and
`gawk_realloc()`. If the allocation fails, they cause `gawk`
to exit with a fatal error message. They should be used as if they were
procedure calls that do not return a value:

`#define emalloc(pointer, type, size, message) …`
:   The arguments to this macro are as follows:

    `pointer`
    :   The pointer variable to point at the allocated storage.

    `type`
    :   The type of the pointer variable. This is used to create a cast for
        the call to `gawk_malloc()`.

    `size`
    :   The total number of bytes to be allocated.

    `message`
    :   A message to be prefixed to the fatal error message. Typically this is the name
        of the function using the macro.

    For example, you might allocate a string value like so:

    ```
    awk_value_t result;
    char *message;
    const char greet[] = "Don't Panic!";

    emalloc(message, char *, sizeof(greet), "myfunc");
    strcpy(message, greet);
    make_malloced_string(message, strlen(message), & result);
    ```

`#define ezalloc(pointer, type, size, message) …`
:   This is like `emalloc()`, but it calls `gawk_calloc()`
    instead of `gawk_malloc()`.
    The arguments are the same as for the `emalloc()` macro, but this
    macro guarantees that the memory returned is initialized to zero.

`#define erealloc(pointer, type, size, message) …`
:   This is like `emalloc()`, but it calls `gawk_realloc()`
    instead of `gawk_malloc()`.
    The arguments are the same as for the `emalloc()` macro.

Two additional functions allocate MPFR and GMP objects for use
by extension functions that need to create and then return such
values.

> **NOTE:** These functions are obsolete. Extension functions that need local MPFR
> and GMP values should simply allocate them on the stack and clear them,
> as any other code would.

The functions are:

`void *get_mpfr_ptr();`
:   Allocate and initialize an MPFR object and return a pointer to it.
    If the allocation fails, `gawk` exits with a fatal
    “out of memory” error. If `gawk` was compiled without
    MPFR support, calling this function causes a fatal error.

`void *get_mpz_ptr();`
:   Allocate and initialize a GMP object and return a pointer to it.
    If the allocation fails, `gawk` exits with a fatal
    “out of memory” error. If `gawk` was compiled without
    MPFR support, calling this function causes a fatal error.

Both of these functions return ‘`void *`’, since the `gawkapi.h`
header file should not have dependency upon `<mpfr.h>` (and `<gmp.h>`,
which is included from `<mpfr.h>`). The actual return values are of
types `mpfr_ptr` and `mpz_ptr` respectively, and you should cast
the return values appropriately before assigning the results to variables
of the correct types.

The memory allocated by these functions should be freed with
`gawk_free()`.

---

Next: [Managing MPFR and GMP Values](#API-Ownership-of-MPFR-and-GMP-Values), Previous: [Memory Allocation Functions and Convenience Macros](#Memory-Allocation-Functions), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.4 Constructor Functions [¶](#Constructor-Functions-1)

The API provides a number of *constructor* functions for creating
string and numeric values, as well as a number of convenience macros.
This subsection presents them all as function prototypes, in
the way that extension code would use them:

`static inline awk_value_t *`

`make_const_string(const char *string, size_t length, awk_value_t *result);`
:   This function creates a string value in the `awk_value_t` variable
    pointed to by `result`. It expects `string` to be a C string constant
    (or other string data), and automatically creates a *copy* of the data
    for storage in `result`. It returns `result`.

`static inline awk_value_t *`

`make_malloced_string(const char *string, size_t length, awk_value_t *result);`
:   This function creates a string value in the `awk_value_t` variable
    pointed to by `result`. It expects `string` to be a ‘`char *`’
    value pointing to data previously obtained from `gawk_malloc()`, `gawk_calloc()`, or `gawk_realloc()`. The idea here
    is that the data is passed directly to `gawk`, which assumes
    responsibility for it. It returns `result`.

`static inline awk_value_t *`

`make_null_string(awk_value_t *result);`
:   This specialized function creates a null string (the “undefined” value)
    in the `awk_value_t` variable pointed to by `result`.
    It returns `result`.

`static inline awk_value_t *`

`make_number(double num, awk_value_t *result);`
:   This function simply creates a numeric value in the `awk_value_t` variable
    pointed to by `result`.

`static inline awk_value_t *`

`make_number_mpz(void *mpz, awk_value_t *result);`
:   This function creates a GMP number value in `result`.
    The `mpz` must be from a call to `get_mpz_ptr()`
    (and thus be of real underlying type `mpz_ptr`).

`static inline awk_value_t *`

`make_number_mpfr(void *mpfr, awk_value_t *result);`
:   This function creates an MPFR number value in `result`.
    The `mpfr` must be from a call to `get_mpfr_ptr()`.

`static inline awk_value_t *`

`make_const_user_input(const char *string, size_t length, awk_value_t *result);`
:   This function is identical to `make_const_string()`, but the string is
    flagged as user input that should be treated as a strnum value if the contents
    of the string are numeric.

`static inline awk_value_t *`

`make_malloced_user_input(const char *string, size_t length, awk_value_t *result);`
:   This function is identical to `make_malloced_string()`, but the string is
    flagged as user input that should be treated as a strnum value if the contents
    of the string are numeric.

`static inline awk_value_t *`

`make_const_regex(const char *string, size_t length, awk_value_t *result);`
:   This function creates a strongly typed regexp value by allocating a copy of the string.
    `string` is the regular expression of length `len`.

`static inline awk_value_t *`

`make_malloced_regex(const char *string, size_t length, awk_value_t *result);`
:   This function creates a strongly typed regexp value. `string` is
    the regular expression of length `len`. It expects `string`
    to be a ‘`char *`’ value pointing to data previously obtained from
    `gawk_malloc()`, `gawk_calloc()`, or `gawk_realloc()`.

`static inline awk_value_t *`

`make_bool(awk_bool_t boolval, awk_value_t *result);`
:   This function creates a boolean value in the `awk_value_t` variable
    pointed to by `result`.

---

Next: [Registration Functions](#Registration-Functions), Previous: [Constructor Functions](#Constructor-Functions), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.5 Managing MPFR and GMP Values [¶](#Managing-MPFR-and-GMP-Values)

MPFR and GMP values are different from string values, where you can
“take ownership” of the value simply by assigning pointers. For example:

```
char *p = gawk_malloc(42);      p ``owns'' the memory
char *q = p;
p = NULL;                       now q ``owns'' it
```

MPFR and GMP objects are indeed allocated on the stack or dynamically,
but the MPFR and GMP libraries treat these objects as values, the same way that
you would pass an `int` or a `double` by value. There is no
way to “transfer ownership” of MPFR and GMP objects.

The final results of an MPFR or GMP calculation should be passed back
to `gawk`, by value, as you would a string or a `double`.
`gawk` will take care of freeing the storage.

Thus, code in an extension should look like this:

```
mpz_t part1, part2, answer;             declare local values

mpz_set_si(part1, 21);                  do some computations
mpz_set_si(part2, 21);
mpz_add(answer, part1, part2);
...
/* assume that result is a parameter of type (awk_value_t *). */
make_number_mpz(answer, & result);      set it with final GMP value

mpz_clear(part1);                       release intermediate values
mpz_clear(part2);

return result;                          value in answer managed by gawk
```

---

Next: [Printing Messages](#Printing-Messages), Previous: [Managing MPFR and GMP Values](#API-Ownership-of-MPFR-and-GMP-Values), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.6 Registration Functions [¶](#Registration-Functions-1)

This section describes the API functions for
registering parts of your extension with `gawk`.

* [Registering An Extension Function](#Extension-Functions)
* [Registering An Exit Callback Function](#Exit-Callback-Functions)
* [Registering An Extension Version String](#Extension-Version-String)
* [Customized Input Parsers](#Input-Parsers)
* [Customized Output Wrappers](#Output-Wrappers)
* [Customized Two-way Processors](#Two_002dway-processors)

---

Next: [Registering An Exit Callback Function](#Exit-Callback-Functions), Up: [Registration Functions](#Registration-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.6.1 Registering An Extension Function [¶](#Registering-An-Extension-Function)

Extension functions are described by the following record:

```
typedef struct awk_ext_func {
    const char *name;
    awk_value_t *(*const function)(int num_actual_args,
                                   awk_value_t *result,
                                   struct awk_ext_func *finfo);
    const size_t max_expected_args;
    const size_t min_required_args;
    awk_bool_t suppress_lint;
    void *data;        /* opaque pointer to any extra state */
} awk_ext_func_t;
```

The fields are:

`const char *name;`
:   The name of the new function.
    `awk`-level code calls the function by this name.
    This is a regular C string.

    Function names must obey the rules for `awk`
    identifiers. That is, they must begin with either an English letter
    or an underscore, which may be followed by any number of
    letters, digits, and underscores.
    Letter case in function names is significant.

`awk_value_t *(*const function)(int num_actual_args,`

`awk_value_t *result,`

`struct awk_ext_func *finfo);`
:   This is a pointer to the C function that provides the extension’s
    functionality.
    The function must fill in `*result` with either a number,
    a string, or a regexp.
    `gawk` takes ownership of any string memory.
    As mentioned earlier, string memory *must* come from one of
    `gawk_malloc()`, `gawk_calloc()`, or `gawk_realloc()`.

    The `num_actual_args` argument tells the C function how many
    actual parameters were passed from the calling `awk` code.

    The `finfo` parameter is a pointer to the `awk_ext_func_t` for
    this function. The called function may access data within it as desired, or not.

    The function must return the value of `result`.
    This is for the convenience of the calling code inside `gawk`.

`const size_t max_expected_args;`
:   This is the maximum number of arguments the function expects to receive.
    If called with more arguments than this, and if lint checking has
    been enabled, then `gawk` prints a warning message. For more
    information, see the entry for `suppress_lint`, later in this list.

`const size_t min_required_args;`
:   This is the minimum number of arguments the function expects to receive.
    If called with fewer arguments, `gawk` prints a fatal error
    message and exits.

`awk_bool_t suppress_lint;`
:   This flag tells `gawk` not to print a lint message if lint
    checking has been enabled and if more arguments were supplied in the call
    than expected. An extension function can tell if `gawk` already
    printed at least one such message by checking if ‘`num_actual_args >
    finfo->max_expected_args`’. If so, and the function does not want more
    lint messages to be printed, it should set `finfo->suppress_lint`
    to `awk_true`.

`void *data;`
:   This is an opaque pointer to any data that an extension function may
    wish to have available when called. Passing the `awk_ext_func_t`
    structure to the extension function, and having this pointer available
    in it enable writing a single C or C++ function that implements multiple
    `awk`-level extension functions.

Once you have a record representing your extension function, you register
it with `gawk` using this API function:

`awk_bool_t add_ext_func(const char *name_space, awk_ext_func_t *func);`
:   This function returns true upon success, false otherwise.
    The `name_space` parameter is the namespace in which to place
    the function (see [Namespaces in `gawk`](#Namespaces)).
    Use an empty string (`""`) or `"awk"` to place
    the function in the default `awk` namespace.
    The `func` pointer is the address of a
    `struct` representing your function, as just described.

    `gawk` does not modify what `func` points to, but the
    extension function itself receives this pointer and can modify what it
    points to, thus it is purposely not declared to be `const`.

The combination of `min_required_args`, `max_expected_args`,
and `suppress_lint` may be confusing. Here is how you should
set things up.

Any number of arguments is valid
:   Set `min_required_args` and `max_expected_args` to zero and
    set `suppress_lint` to `awk_true`.

A minimum number of arguments is required, no limit on maximum number of arguments
:   Set `min_required_args` to the minimum required. Set
    `max_expected_args` to zero and
    set `suppress_lint` to `awk_true`.

A minimum number of arguments is required, a maximum number is expected
:   Set `min_required_args` to the minimum required. Set
    `max_expected_args` to the maximum expected.
    Set `suppress_lint` to `awk_false`.

A minimum number of arguments is required, and no more than a maximum is allowed
:   Set `min_required_args` to the minimum required. Set
    `max_expected_args` to the maximum expected.
    Set `suppress_lint` to `awk_false`.
    In your extension function, check that `num_actual_args` does not
    exceed `f->max_expected_args`. If it does, issue a fatal error message.

---

Next: [Registering An Extension Version String](#Extension-Version-String), Previous: [Registering An Extension Function](#Extension-Functions), Up: [Registration Functions](#Registration-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.6.2 Registering An Exit Callback Function [¶](#Registering-An-Exit-Callback-Function)

An *exit callback* function is a function that
`gawk` calls before it exits.
Such functions are useful if you have general “cleanup” tasks
that should be performed in your extension (such as closing database
connections or other resource deallocations).
You can register such
a function with `gawk` using the following function:

`void awk_atexit(void (*funcp)(void *data, int exit_status),`

`void *arg0);`
:   The parameters are:

    `funcp`
    :   A pointer to the function to be called before `gawk` exits. The `data`
        parameter will be the original value of `arg0`.
        The `exit_status` parameter is the exit status value that
        `gawk` intends to pass to the `exit()` system call.

    `arg0`
    :   A pointer to private data that `gawk` saves in order to pass to
        the function pointed to by `funcp`.

Exit callback functions are called in last-in, first-out (LIFO)
order—that is, in the reverse order in which they are registered with
`gawk`.

---

Next: [Customized Input Parsers](#Input-Parsers), Previous: [Registering An Exit Callback Function](#Exit-Callback-Functions), Up: [Registration Functions](#Registration-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.6.3 Registering An Extension Version String [¶](#Registering-An-Extension-Version-String)

You can register a version string that indicates the name and
version of your extension with `gawk`, as follows:

`void register_ext_version(const char *version);`
:   Register the string pointed to by `version` with `gawk`.
    Note that `gawk` does *not* copy the `version` string, so
    it should not be changed.

`gawk` prints all registered extension version strings when it
is invoked with the `--version` option.

---

Next: [Customized Output Wrappers](#Output-Wrappers), Previous: [Registering An Extension Version String](#Extension-Version-String), Up: [Registration Functions](#Registration-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.6.4 Customized Input Parsers [¶](#Customized-Input-Parsers)

By default, `gawk` reads text files as its input. It uses the value
of `RS` to find the end of an input record, and then uses `FS`
(or `FIELDWIDTHS` or `FPAT`) to split it into fields (see [Reading Input Files](#Reading-Files)).
Additionally, it sets the value of `RT` (see [Predefined Variables](#Built_002din-Variables)).

If you want, you can provide your own custom input parser. An input
parser’s job is to return a record to the `gawk` record-processing
code, along with indicators for the value and length of the data to be
used for `RT`, if any.

To provide an input parser, you must first provide two functions
(where XXX is a prefix name for your extension):

`awk_bool_t XXX_can_take_file(const awk_input_buf_t *iobuf);`
:   This function examines the information available in `iobuf`
    (which we discuss shortly). Based on the information there, it
    decides if the input parser should be used for this file.
    If so, it should return true. Otherwise, it should return false.
    It should not change any state (variable values, etc.) within `gawk`.

`awk_bool_t XXX_take_control_of(awk_input_buf_t *iobuf);`
:   When `gawk` decides to hand control of the file over to the
    input parser, it calls this function. This function in turn must fill
    in certain fields in the `awk_input_buf_t` structure and ensure
    that certain conditions are true. It should then return true. If an
    error of some kind occurs, it should not fill in any fields and should
    return false; then `gawk` will not use the input parser.
    The details are presented shortly.

Your extension should package these functions inside an
`awk_input_parser_t`, which looks like this:

```
typedef struct awk_input_parser {
    const char *name;   /* name of parser */
    awk_bool_t (*can_take_file)(const awk_input_buf_t *iobuf);
    awk_bool_t (*take_control_of)(awk_input_buf_t *iobuf);
    awk_const struct awk_input_parser *awk_const next;   /* for gawk */
} awk_input_parser_t;
```

The fields are:

`const char *name;`
:   The name of the input parser. This is a regular C string.

`awk_bool_t (*can_take_file)(const awk_input_buf_t *iobuf);`
:   A pointer to your `XXX_can_take_file()` function.

`awk_bool_t (*take_control_of)(awk_input_buf_t *iobuf);`
:   A pointer to your `XXX_take_control_of()` function.

`awk_const struct input_parser *awk_const next;`
:   This is for use by `gawk`;
    therefore it is marked `awk_const` so that the extension cannot
    modify it.

The steps are as follows:

1. Create a `static awk_input_parser_t` variable and initialize it
   appropriately.
2. When your extension is loaded, register your input parser with
   `gawk` using the `register_input_parser()` API function
   (described next).

An `awk_input_buf_t` looks like this:

```
typedef struct awk_input {
    const char *name;       /* filename */
    int fd;                 /* file descriptor */
#define INVALID_HANDLE (-1)
    void *opaque;           /* private data for input parsers */
    int (*get_record)(char **out, struct awk_input *iobuf,
                      int *errcode, char **rt_start, size_t *rt_len,
                      const awk_fieldwidth_info_t **field_width);
    ssize_t (*read_func)();
    void (*close_func)(struct awk_input *iobuf);
    struct stat sbuf;       /* stat buf */
} awk_input_buf_t;
```

The fields can be divided into two categories: those for use (initially,
at least) by `XXX_can_take_file()`, and those for use by
`XXX_take_control_of()`. The first group of fields and their uses
are as follows:

`const char *name;`
:   The name of the file.

`int fd;`
:   A file descriptor for the file. `gawk` attempts to open
    the file for reading using the `open()` system call. If it was
    able to open the file, then `fd` will *not* be equal to
    `INVALID_HANDLE`. Otherwise, it will.

    An extension can decide that it doesn’t want to use the open file descriptor
    provided by `gawk`. In such a case it can close the file and
    set `fd` to `INVALID_HANDLE`, or it can leave it alone and
    keep it’s own file descriptor in private data pointed to by the
    `opaque` pointer (see further in this list). In any case, if
    the file descriptor is valid, it should *not* just overwrite the
    value with something else; doing so would cause a resource leak.

`struct stat sbuf;`
:   If the file descriptor is valid, then `gawk` will have filled
    in this structure via a call to the `fstat()` system call.
    Otherwise, if the `lstat()` system call is available, it will
    use that. If `lstat()` is not available, then it uses `stat()`.

    Getting the file’s information allows extensions to check the type of
    the file even if it could not be opened. This occurs, for example,
    on Windows systems when trying to use `open()` on a directory.

    If `gawk` was not able to get the file information, then
    `sbuf` will be zeroed out. In particular, extension code
    can check if ‘`sbuf.st_mode == 0`’. If that’s true, then there
    is no information in `sbuf`.[109](#FOOT109)

The `XXX_can_take_file()` function should examine these
fields and decide if the input parser should be used for the file.
The decision can be made based upon `gawk` state (the value
of a variable defined previously by the extension and set by
`awk` code), the name of the
file, whether or not the file descriptor is valid, the information
in the `struct stat`, or any combination of these factors.

Once `XXX_can_take_file()` has returned true, and
`gawk` has decided to use your input parser, it calls
`XXX_take_control_of()`. That function then fills
either the `get_record` field or the `read_func` field in
the `awk_input_buf_t`. It must also ensure that `fd` is *not*
set to `INVALID_HANDLE`. The following list describes the fields that
may be filled by `XXX_take_control_of()`:

`void *opaque;`
:   This is used to hold any state information needed by the input parser
    for this file. It is “opaque” to `gawk`. The input parser
    is not required to use this pointer.

`int (*get_record)(char **out,`

`struct awk_input *iobuf,`

`int *errcode,`

`char **rt_start,`

`size_t *rt_len,`

`const awk_fieldwidth_info_t **field_width);`
:   This function pointer should point to a function that creates the input
    records. Said function is the core of the input parser. Its behavior
    is described in the text following this list.

`ssize_t (*read_func)(int, void *, size_t);`
:   This function pointer should point to a function that has the
    same behavior as the standard POSIX `read()` system call.
    It is an alternative to the `get_record` pointer. Its behavior
    is also described in the text following this list.

`void (*close_func)(struct awk_input *iobuf);`
:   This function pointer should point to a function that does
    the “teardown.” It should release any resources allocated by
    `XXX_take_control_of()`. It may also close the file. If it
    does so, it should set the `fd` field to `INVALID_HANDLE`.

    If `fd` is still not `INVALID_HANDLE` after the call to this
    function, `gawk` calls the regular `close()` system call.

    Having a “teardown” function is optional. If your input parser does
    not need it, do not set this field. Then, `gawk` calls the
    regular `close()` system call on the file descriptor, so it should
    be valid.

The `XXX_get_record()` function does the work of creating
input records. The parameters are as follows:

`char **out`
:   This is a pointer to a `char *` variable that is set to point
    to the record. `gawk` makes its own copy of the data, so
    your extension must manage this storage.

`struct awk_input *iobuf`
:   This is the `awk_input_buf_t` for the file. Two of its fields should
    be used by your extension: `fd` for reading data, and `opaque`
    for managing any private state.

`int *errcode`
:   If an error occurs, `*errcode` should be set to an appropriate
    code from `<errno.h>`.

`char **rt_start`

`size_t *rt_len`
:   If the concept of a “record terminator” makes sense, then
    `*rt_start` should be set to point to the data to be used for
    `RT`, and `*rt_len` should be set to the length of the
    data. Otherwise, `*rt_len` should be set to zero.
    Here too, `gawk` makes its own copy of this data, so your
    extension must manage this storage.

`const awk_fieldwidth_info_t **field_width`
:   If `field_width` is not `NULL`, then `*field_width` will be initialized
    to `NULL`, and the function may set it to point to a structure
    supplying field width information to override the default
    field parsing mechanism. Note that this structure will not
    be copied by `gawk`; it must persist at least until the next call
    to `get_record` or `close_func`. Note also that `field_width` is
    `NULL` when `getline` is assigning the results to a variable, thus
    field parsing is not needed.

    If the parser sets `*field_width`,
    then `gawk` uses this layout to parse the input record,
    and the `PROCINFO["FS"]` value will be `"API"` while this record
    is active in `$0`.
    The `awk_fieldwidth_info_t` data structure
    is described below.

The return value is the length of the buffer pointed to by
`*out`, or `EOF` if end-of-file was reached or an
error occurred.

It is guaranteed that `errcode` is a valid pointer, so there is no
need to test for a `NULL` value. `gawk` sets `*errcode`
to zero, so there is no need to set it unless an error occurs.

If an error does occur, the function should return `EOF` and set
`*errcode` to a value greater than zero. In that case, if `*errcode`
does not equal zero, `gawk` automatically updates
the `ERRNO` variable based on the value of `*errcode`.
(In general, setting ‘`*errcode = errno`’ should do the right thing.)

As an alternative to supplying a function that returns an input record,
you may instead supply a function that simply reads bytes, and let
`gawk` parse the data into records. If you do so, the data
should be returned in the multibyte encoding of the current locale.
Such a function should follow the same behavior as the `read()`
system call, and you fill in the `read_func` pointer with its
address in the `awk_input_buf_t` structure.

By default, `gawk` sets the `read_func` pointer to
point to the `read()` system call. So your extension need not
set this field explicitly.

> **NOTE:** You must choose one method or the other: either a function that
> returns a record, or one that returns raw data. In particular,
> if you supply a function to get a record, `gawk` will
> call it, and will never call the raw read function.

`gawk` ships with a sample extension that reads directories,
returning records for each entry in a directory (see [Reading Directories](#Extension-Sample-Readdir)). You may wish to use that code as a guide for writing
your own input parser.

When writing an input parser, you should think about (and document)
how it is expected to interact with `awk` code. You may want
it to always be called, and to take effect as appropriate (as the
`readdir` extension does). Or you may want it to take effect
based upon the value of an `awk` variable, as the XML extension
from the `gawkextlib` project does (see [The `gawkextlib` Project](#gawkextlib)).
In the latter case, code in a `BEGINFILE` rule
can look at `FILENAME` and `ERRNO` to decide whether or
not to activate your input parser (see [The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE)).

If you would like to override the default field parsing mechanism for a given
record, then you must populate an `awk_fieldwidth_info_t` structure,
which looks like this:

```
typedef struct {
        awk_bool_t     use_chars; /* false ==> use bytes */
        size_t         nf;        /* number of fields in record (NF) */
        struct awk_field_info {
                size_t skip;      /* amount to skip before field starts */
                size_t len;       /* length of field */
        } fields[1];              /* actual dimension should be nf */
} awk_fieldwidth_info_t;
```

The fields are:

`awk_bool_t use_chars;`
:   Set this to `awk_true` if the field lengths are specified in terms
    of potentially multi-byte characters, and set it to `awk_false` if
    the lengths are in terms of bytes.
    Performance will be better if the values are supplied in
    terms of bytes.

`size_t nf;`
:   Set this to the number of fields in the input record, i.e. `NF`.

`struct awk_field_info fields[nf];`
:   This is a variable-length array whose actual dimension should be `nf`.
    For each field, the `skip` element should be set to the number
    of characters or bytes, as controlled by the `use_chars` flag,
    to skip before the start of this field. The `len` element provides
    the length of the field. The values in `fields[0]` provide the information
    for `$1`, and so on through the `fields[nf-1]` element containing the information for `$NF`.

A convenience macro `awk_fieldwidth_info_size(numfields)` is provided to
calculate the appropriate size of a variable-length
`awk_fieldwidth_info_t` structure containing `numfields` fields. This can
be used as an argument to `malloc()` or in a union to allocate space
statically. Please refer to the `readdir_test` sample extension for an
example.

You register your input parser with the following function:

`void register_input_parser(awk_input_parser_t *input_parser);`
:   Register the input parser pointed to by `input_parser` with
    `gawk`.

---

Next: [Customized Two-way Processors](#Two_002dway-processors), Previous: [Customized Input Parsers](#Input-Parsers), Up: [Registration Functions](#Registration-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.6.5 Customized Output Wrappers [¶](#Customized-Output-Wrappers)

An *output wrapper* is the mirror image of an input parser.
It allows an extension to take over the output to a file opened
with the ‘`>`’ or ‘`>>`’ I/O redirection operators (see [Redirecting Output of `print` and `printf`](#Redirection)).

The output wrapper is very similar to the input parser structure:

```
typedef struct awk_output_wrapper {
    const char *name;   /* name of the wrapper */
    awk_bool_t (*can_take_file)(const awk_output_buf_t *outbuf);
    awk_bool_t (*take_control_of)(awk_output_buf_t *outbuf);
    awk_const struct awk_output_wrapper *awk_const next;  /* for gawk */
} awk_output_wrapper_t;
```

The members are as follows:

`const char *name;`
:   This is the name of the output wrapper.

`awk_bool_t (*can_take_file)(const awk_output_buf_t *outbuf);`
:   This points to a function that examines the information in
    the `awk_output_buf_t` structure pointed to by `outbuf`.
    It should return true if the output wrapper wants to take over the
    file, and false otherwise. It should not change any state (variable
    values, etc.) within `gawk`.

`awk_bool_t (*take_control_of)(awk_output_buf_t *outbuf);`
:   The function pointed to by this field is called when `gawk`
    decides to let the output wrapper take control of the file. It should
    fill in appropriate members of the `awk_output_buf_t` structure,
    as described next, and return true if successful, false otherwise.

`awk_const struct output_wrapper *awk_const next;`
:   This is for use by `gawk`;
    therefore it is marked `awk_const` so that the extension cannot
    modify it.

The `awk_output_buf_t` structure looks like this:

```
typedef struct awk_output_buf {
    const char *name;   /* name of output file */
    const char *mode;   /* mode argument to fopen */
    FILE *fp;           /* stdio file pointer */
    awk_bool_t redirected;  /* true if a wrapper is active */
    void *opaque;       /* for use by output wrapper */
    size_t (*gawk_fwrite)(const void *buf, size_t size, size_t count,
                FILE *fp, void *opaque);
    int (*gawk_fflush)(FILE *fp, void *opaque);
    int (*gawk_ferror)(FILE *fp, void *opaque);
    int (*gawk_fclose)(FILE *fp, void *opaque);
} awk_output_buf_t;
```

Here too, your extension will define `XXX_can_take_file()`
and `XXX_take_control_of()` functions that examine and update
data members in the `awk_output_buf_t`.
The data members are as follows:

`const char *name;`
:   The name of the output file.

`const char *mode;`
:   The mode string (as would be used in the second argument to `fopen()`)
    with which the file was opened.

`FILE *fp;`
:   The `FILE` pointer from `<stdio.h>`. `gawk` opens the file
    before attempting to find an output wrapper.

`awk_bool_t redirected;`
:   This field must be set to true by the `XXX_take_control_of()` function.

`void *opaque;`
:   This pointer is opaque to `gawk`. The extension should use it to store
    a pointer to any private data associated with the file.

`size_t (*gawk_fwrite)(const void *buf, size_t size, size_t count,`

`FILE *fp, void *opaque);`

`int (*gawk_fflush)(FILE *fp, void *opaque);`

`int (*gawk_ferror)(FILE *fp, void *opaque);`

`int (*gawk_fclose)(FILE *fp, void *opaque);`
:   These pointers should be set to point to functions that perform
    the equivalent function as the `<stdio.h>` functions do, if appropriate.
    `gawk` uses these function pointers for all output.
    `gawk` initializes the pointers to point to internal “pass-through”
    functions that just call the regular `<stdio.h>` functions, so an
    extension only needs to redefine those functions that are appropriate for
    what it does.

The `XXX_can_take_file()` function should make a decision based
upon the `name` and `mode` fields, and any additional state
(such as `awk` variable values) that is appropriate.
`gawk` attempts to open the named file for writing. The `fp`
member will be `NULL` only if it fails.

When `gawk` calls `XXX_take_control_of()`, that function should fill
in the other fields as appropriate, except for `fp`, which it should just
use normally if it’s not `NULL`.

You register your output wrapper with the following function:

`void register_output_wrapper(awk_output_wrapper_t *output_wrapper);`
:   Register the output wrapper pointed to by `output_wrapper` with
    `gawk`.

---

Previous: [Customized Output Wrappers](#Output-Wrappers), Up: [Registration Functions](#Registration-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.6.6 Customized Two-way Processors [¶](#Customized-Two_002dway-Processors)

A *two-way processor* combines an input parser and an output wrapper for
two-way I/O with the ‘`|&`’ operator (see [Redirecting Output of `print` and `printf`](#Redirection)). It makes identical
use of the `awk_input_parser_t` and `awk_output_buf_t` structures
as described earlier.

A two-way processor is represented by the following structure:

```
typedef struct awk_two_way_processor {
    const char *name;   /* name of the two-way processor */
    awk_bool_t (*can_take_two_way)(const char *name);
    awk_bool_t (*take_control_of)(const char *name,
                                  awk_input_buf_t *inbuf,
                                  awk_output_buf_t *outbuf);
    awk_const struct awk_two_way_processor *awk_const next;  /* for gawk */
} awk_two_way_processor_t;
```

The fields are as follows:

`const char *name;`
:   The name of the two-way processor.

`awk_bool_t (*can_take_two_way)(const char *name);`
:   The function pointed to by this field should return true if it wants to take over two-way I/O for this file name.
    It should not change any state (variable
    values, etc.) within `gawk`.

`awk_bool_t (*take_control_of)(const char *name,`

`awk_input_buf_t *inbuf,`

`awk_output_buf_t *outbuf);`
:   The function pointed to by this field should fill in the `awk_input_buf_t` and
    `awk_output_buf_t` structures pointed to by `inbuf` and
    `outbuf`, respectively. These structures were described earlier.

`awk_const struct two_way_processor *awk_const next;`
:   This is for use by `gawk`;
    therefore it is marked `awk_const` so that the extension cannot
    modify it.

As with the input parser and output processor, you provide
“yes I can take this” and “take over for this” functions,
`XXX_can_take_two_way()` and `XXX_take_control_of()`.

You register your two-way processor with the following function:

`void register_two_way_processor(awk_two_way_processor_t *two_way_processor);`
:   Register the two-way processor pointed to by `two_way_processor` with
    `gawk`.

---

Next: [Updating `ERRNO`](#Updating-ERRNO), Previous: [Registration Functions](#Registration-Functions), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.7 Printing Messages [¶](#Printing-Messages-1)

You can print different kinds of warning messages from your
extension, as described here. Note that for these functions,
you must pass in the extension ID received from `gawk`
when the extension was loaded:[110](#FOOT110)

`void fatal(awk_ext_id_t id, const char *format, ...);`
:   Print a message and then cause `gawk` to exit immediately.

`void nonfatal(awk_ext_id_t id, const char *format, ...);`
:   Print a nonfatal error message.

`void warning(awk_ext_id_t id, const char *format, ...);`
:   Print a warning message.

`void lintwarn(awk_ext_id_t id, const char *format, ...);`
:   Print a “lint warning.” Normally this is the same as printing a
    warning message, but if `gawk` was invoked with ‘`--lint=fatal`’,
    then lint warnings become fatal error messages.

All of these functions are otherwise like the C `printf()`
family of functions, where the `format` parameter is a string
with literal characters and formatting codes intermixed.

---

Next: [Requesting Values](#Requesting-Values), Previous: [Printing Messages](#Printing-Messages), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.8 Updating `ERRNO` [¶](#Updating-ERRNO-1)

The following functions allow you to update the `ERRNO`
variable:

`void update_ERRNO_int(int errno_val);`
:   Set `ERRNO` to the string equivalent of the error code
    in `errno_val`. The value should be one of the defined
    error codes in `<errno.h>`, and `gawk` turns it
    into a (possibly translated) string using the C `strerror()` function.

`void update_ERRNO_string(const char *string);`
:   Set `ERRNO` directly to the string value of `ERRNO`.
    `gawk` makes a copy of the value of `string`.

`void unset_ERRNO(void);`
:   Unset `ERRNO`.

---

Next: [Accessing and Updating Parameters](#Accessing-Parameters), Previous: [Updating `ERRNO`](#Updating-ERRNO), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.9 Requesting Values [¶](#Requesting-Values-1)

All of the functions that return values from `gawk`
work in the same way. You pass in an `awk_valtype_t` value
to indicate what kind of value you expect. If the actual value
matches what you requested, the function returns true and fills
in the `awk_value_t` result.
Otherwise, the function returns false, and the `val_type`
member indicates the type of the actual value. You may then
print an error message or reissue the request for the actual
value type, as appropriate. This behavior is summarized in
[Table 17.2](#table_002dvalue_002dtypes_002dreturned).

|  | Type of Actual Value |
| --- | --- |

|  |  | String | Strnum | Number | Regex | Bool | Array | Undefined |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  | **String** | String | String | String | String | String | false | false |
|  | **Strnum** | false | Strnum | Strnum | false | false | false | false |
|  | **Number** | Number | Number | Number | false | Number | false | false |
| **Type** | **Regex** | false | false | false | Regex | false | false | false |
| **Requested** | **Bool** | false | false | false | false | Bool | false | false |
|  | **Array** | false | false | false | false | false | Array | false |
|  | **Scalar** | Scalar | Scalar | Scalar | Scalar | Scalar | false | false |
|  | **Undefined** | String | Strnum | Number | Regex | Bool | Array | Undefined |
|  | **Value cookie** | false | false | false | false | false | false | false |

**Table 17.2:** API value types returned

There are a number of points of note:

* A request for `AWK_UNDEFINED` always returns true, filling in
  the actual type of the particular value. You can think of this
  as a sort of “wildcard” request.
* Requesting an `AWK_STRING` causes `gawk` to convert any
  scalar value to a string result, and that is what is returned.
* Requesting an `AWK_NUMBER` causes `gawk` to convert any
  scalar value, except for a regexp, to a numeric result, and
  that is what is returned.

  Conversion between string and number in the API thus parallels how
  `gawk` behaves in running code.
* The API functions do *not* distinguish between `"untyped"`
  and `"unassigned"` as returned by `typeof()` (see [Dynamic Typing In `gawk`](#Dynamic-Typing-Gawk)). `AWK_UNDEFINED` serves for both. This is unlikely
  to change, as the documentation and code are already complicated enough.

---

Next: [Symbol Table Access](#Symbol-Table-Access), Previous: [Requesting Values](#Requesting-Values), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.10 Accessing and Updating Parameters [¶](#Accessing-and-Updating-Parameters)

Two functions give you access to the arguments (parameters)
passed to your extension function. They are:

`awk_bool_t get_argument(size_t count,`

`awk_valtype_t wanted,`

`awk_value_t *result);`
:   Fill in the `awk_value_t` structure pointed to by `result`
    with the `count`th argument. Return true if the actual
    type matches `wanted`, and false otherwise. In the latter
    case, `result->val_type` indicates the actual type
    (see [Table 17.2](#table_002dvalue_002dtypes_002dreturned)). Counts are zero-based—the first
    argument is numbered zero, the second one, and so on. `wanted`
    indicates the type of value expected.

`awk_bool_t set_argument(size_t count, awk_array_t array);`
:   Convert a parameter that was undefined into an array; this provides
    call by reference for arrays. Return false if `count` is too big,
    or if the argument’s type is not undefined. See [Array Manipulation](#Array-Manipulation)
    for more information on creating arrays.

---

Next: [Array Manipulation](#Array-Manipulation), Previous: [Accessing and Updating Parameters](#Accessing-Parameters), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.11 Symbol Table Access [¶](#Symbol-Table-Access-1)

Two sets of routines provide access to global variables, and one set
allows you to create and release cached values.

* [Variable Access and Update by Name](#Symbol-table-by-name)
* [Variable Access and Update by Cookie](#Symbol-table-by-cookie)
* [Creating and Using Cached Values](#Cached-values)

---

Next: [Variable Access and Update by Cookie](#Symbol-table-by-cookie), Up: [Symbol Table Access](#Symbol-Table-Access)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.11.1 Variable Access and Update by Name [¶](#Variable-Access-and-Update-by-Name)

The following routines provide the ability to access and update
global `awk`-level variables by name. In compiler terminology,
identifiers of different kinds are termed *symbols*, thus the “sym”
in the routines’ names. The data structure that stores information
about symbols is termed a *symbol table*.
The functions are as follows:

`awk_bool_t sym_lookup(const char *name,`

`awk_valtype_t wanted,`

`awk_value_t *result);`
:   Fill in the `awk_value_t` structure pointed to by `result`
    with the value of the variable named by the string `name`, which is
    a regular C string. `wanted` indicates the type of value expected.
    Return true if the actual type matches `wanted`, and false otherwise.
    In the latter case, `result->val_type` indicates the actual type
    (see [Table 17.2](#table_002dvalue_002dtypes_002dreturned)).

`awk_bool_t sym_lookup_ns(const char *name,`

`const char *name_space,`

`awk_valtype_t wanted,`

`awk_value_t *result);`
:   This is like `sym_lookup()`, but the `name_space` parameter allows you
    to specify which namespace `name` is part of. `name_space` cannot be
    `NULL`. If it is `""` or `"awk"`, then `name` is searched
    for in the default `awk` namespace.

    Note that `namespace` is a C++ keyword. For interoperability with C++,
    you should avoid using that identifier in C code.

`awk_bool_t sym_update(const char *name, awk_value_t *value);`
:   Update the variable named by the string `name`, which is a regular
    C string. The variable is added to `gawk`’s symbol table
    if it is not there. Return true if everything worked, and false otherwise.

    Changing types (scalar to array or vice versa) of an existing variable
    is *not* allowed, nor may this routine be used to update an array.
    This routine cannot be used to update any of the predefined
    variables (such as `ARGC` or `NF`).

`awk_bool_t sym_update_ns(const char *name_space, const char *name, awk_value_t *value);`
:   This is like `sym_update()`, but the `name_space` parameter allows you
    to specify which namespace `name` is part of. `name_space` cannot be
    `NULL`. If it is `""` or `"awk"`, then `name` is searched
    for in the default `awk` namespace.

An extension can look up the value of `gawk`’s special variables.
However, with the exception of the `PROCINFO` array, an extension
cannot change any of those variables.

When searching for or updating variables outside the `awk` namespace
(see [Namespaces in `gawk`](#Namespaces)), function and variable names must be simple
identifiers.[111](#FOOT111) In addition, namespace names and variable and function names
must follow the rules given in [Namespace and Component Naming Rules](#Naming-Rules).

---

Next: [Creating and Using Cached Values](#Cached-values), Previous: [Variable Access and Update by Name](#Symbol-table-by-name), Up: [Symbol Table Access](#Symbol-Table-Access)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.11.2 Variable Access and Update by Cookie [¶](#Variable-Access-and-Update-by-Cookie)

A *scalar cookie* is an opaque handle that provides access
to a global variable or array. It is an optimization that
avoids looking up variables in `gawk`’s symbol table every time
access is needed. This was discussed earlier, in [General-Purpose Data Types](#General-Data-Types).

The following functions let you work with scalar cookies:

`awk_bool_t sym_lookup_scalar(awk_scalar_t cookie,`

`awk_valtype_t wanted,`

`awk_value_t *result);`
:   Retrieve the current value of a scalar cookie.
    Once you have obtained a scalar cookie using `sym_lookup()`, you can
    use this function to get its value more efficiently.
    Return false if the value cannot be retrieved.

`awk_bool_t sym_update_scalar(awk_scalar_t cookie, awk_value_t *value);`
:   Update the value associated with a scalar cookie. Return false if
    the new value is not of type `AWK_STRING`, `AWK_STRNUM`, `AWK_REGEX`, or `AWK_NUMBER`.
    Here too, the predefined variables may not be updated.

It is not obvious at first glance how to work with scalar cookies or
what their *raison d’être* really is. In theory, the `sym_lookup()`
and `sym_update()` routines are all you really need to work with
variables. For example, you might have code that looks up the value of
a variable, evaluates a condition, and then possibly changes the value
of the variable based on the result of that evaluation, like so:

```
/*  do_magic --- do something really great */

static awk_value_t *
do_magic(int nargs, awk_value_t *result)
{
    awk_value_t value;

    if (   sym_lookup("MAGIC_VAR", AWK_NUMBER, & value)
        && some_condition(value.num_value)) {
            value.num_value += 42;
            sym_update("MAGIC_VAR", & value);
    }

    return make_number(0.0, result);
}
```

This code looks (and is) simple and straightforward. So what’s the problem?

Well, consider what happens if `awk`-level code associated
with your extension calls the `magic()` function (implemented in
C by `do_magic()`), once per record, while processing hundreds
of thousands or millions of records. The `MAGIC_VAR` variable is
looked up in the symbol table once or twice per function call!

The symbol table lookup is really pure overhead; it is considerably
more efficient to get a cookie that represents the variable, and use
that to get the variable’s value and update it as needed.[112](#FOOT112)

Thus, the way to use cookies is as follows. First, install
your extension’s variable in `gawk`’s symbol table using
`sym_update()`, as usual. Then get a scalar cookie for the variable
using `sym_lookup()`:

```
static awk_scalar_t magic_var_cookie;    /* cookie for MAGIC_VAR */

static void
my_extension_init()
{
    awk_value_t value;
```

```
    /* install initial value */
    sym_update("MAGIC_VAR", make_number(42.0, & value));

    /* get the cookie */
    sym_lookup("MAGIC_VAR", AWK_SCALAR, & value);

    /* save the cookie */
    magic_var_cookie = value.scalar_cookie;
    ...
}
```

Next, use the routines in this section for retrieving and updating
the value through the cookie. Thus, `do_magic()` now becomes
something like this:

```
/*  do_magic --- do something really great */

static awk_value_t *
do_magic(int nargs, awk_value_t *result)
{
    awk_value_t value;

    if (   sym_lookup_scalar(magic_var_cookie, AWK_NUMBER, & value)
        && some_condition(value.num_value)) {
            value.num_value += 42;
            sym_update_scalar(magic_var_cookie, & value);
    }
    ...

    return make_number(0.0, result);
}
```

> **NOTE:** The previous code omitted error checking for
> presentation purposes. Your extension code should be more robust
> and carefully check the return values from the API functions.

---

Previous: [Variable Access and Update by Cookie](#Symbol-table-by-cookie), Up: [Symbol Table Access](#Symbol-Table-Access)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.11.3 Creating and Using Cached Values [¶](#Creating-and-Using-Cached-Values)

The routines in this section allow you to create and release
cached values. Like scalar cookies, in theory, cached values
are not necessary. You can create numbers and strings using
the functions in [Constructor Functions](#Constructor-Functions). You can then
assign those values to variables using `sym_update()`
or `sym_update_scalar()`, as you like.

However, you can understand the point of cached values if you remember that
*every* string value’s storage *must* come from `gawk_malloc()`,
`gawk_calloc()`, or `gawk_realloc()`.
If you have 20 variables, all of which have the same string value, you
must create 20 identical copies of the string.[113](#FOOT113)

It is clearly more efficient, if possible, to create a value once, and
then tell `gawk` to reuse the value for multiple variables. That
is what the routines in this section let you do. The functions are as follows:

`awk_bool_t create_value(awk_value_t *value, awk_value_cookie_t *result);`
:   Create a cached string or numeric value from `value` for
    efficient later assignment. Only values of type `AWK_NUMBER`, `AWK_REGEX`, `AWK_STRNUM`,
    and `AWK_STRING` are allowed. Any other type is rejected.
    `AWK_UNDEFINED` could be allowed, but doing so would result in
    inferior performance.

`awk_bool_t release_value(awk_value_cookie_t vc);`
:   Release the memory associated with a value cookie obtained
    from `create_value()`.

You use value cookies in a fashion similar to the way you use scalar cookies.
In the extension initialization routine, you create the value cookie:

```
static awk_value_cookie_t answer_cookie;  /* static value cookie */

static void
my_extension_init()
{
    awk_value_t value;
    char *long_string;
    size_t long_string_len;

    /* code from earlier */
    ...
    /* ... fill in long_string and long_string_len ... */
    make_malloced_string(long_string, long_string_len, & value);
    create_value(& value, & answer_cookie);    /* create cookie */
    ...
}
```

Once the value is created, you can use it as the value of any number
of variables:

```
static awk_value_t *
do_magic(int nargs, awk_value_t *result)
{
    awk_value_t new_value;

    ...    /* as earlier */

    value.val_type = AWK_VALUE_COOKIE;
    value.value_cookie = answer_cookie;
    sym_update("VAR1", & value);
    sym_update("VAR2", & value);
    ...
    sym_update("VAR100", & value);
    ...
}
```

Using value cookies in this way saves considerable storage, as all of
`VAR1` through `VAR100` share the same value.

You might be wondering, “Is this sharing problematic?
What happens if `awk` code assigns a new value to `VAR1`;
are all the others changed too?”

That’s a great question. The answer is that no, it’s not a problem.
Internally, `gawk` uses *reference-counted strings*. This means
that many variables can share the same string value, and `gawk`
keeps track of the usage. When a variable’s value changes, `gawk`
simply decrements the reference count on the old value and updates
the variable to use the new value.

Finally, as part of your cleanup action (see [Registering An Exit Callback Function](#Exit-Callback-Functions))
you should release any cached values that you created, using
`release_value()`.

---

Next: [Accessing and Manipulating Redirections](#Redirection-API), Previous: [Symbol Table Access](#Symbol-Table-Access), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.12 Array Manipulation [¶](#Array-Manipulation-1)

The primary data structure[114](#FOOT114) in `awk`
is the associative array (see [Arrays in `awk`](#Arrays)).
Extensions need to be able to manipulate `awk` arrays.
The API provides a number of data structures for working with arrays,
functions for working with individual elements, and functions for
working with arrays as a whole. This includes the ability to
“flatten” an array so that it is easy for C code to traverse
every element in an array. The array data structures integrate
nicely with the data structures for values to make it easy to
both work with and create true arrays of arrays (see [General-Purpose Data Types](#General-Data-Types)).

* [Array Data Types](#Array-Data-Types)
* [Array Functions](#Array-Functions)
* [Working With All The Elements of an Array](#Flattening-Arrays)
* [How To Create and Populate Arrays](#Creating-Arrays)

---

Next: [Array Functions](#Array-Functions), Up: [Array Manipulation](#Array-Manipulation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.12.1 Array Data Types [¶](#Array-Data-Types-1)

The data types associated with arrays are as follows:

`typedef void *awk_array_t;`
:   If you request the value of an array variable, you get back an
    `awk_array_t` value. This value is opaque[115](#FOOT115) to the extension; it uniquely identifies the array but can
    only be used by passing it into API functions or receiving it from API
    functions. This is very similar to way ‘`FILE *`’ values are used
    with the `<stdio.h>` library routines.

`typedef struct awk_element {`

`/* convenience linked list pointer, not used by gawk */`

`struct awk_element *next;`

`enum {`

`AWK_ELEMENT_DEFAULT = 0,  /* set by gawk */`

`AWK_ELEMENT_DELETE = 1    /* set by extension */`

`} flags;`

`awk_value_t index;`

`awk_value_t value;`

`} awk_element_t;`
:   The `awk_element_t` is a “flattened”
    array element. `awk` produces an array of these
    inside the `awk_flat_array_t` (see the next item).
    Individual elements may be marked for deletion. New elements must be added
    individually, one at a time, using the separate API for that purpose.
    The fields are as follows:

    `struct awk_element *next;`
    :   This pointer is for the convenience of extension writers. It allows
        an extension to create a linked list of new elements that can then be
        added to an array in a loop that traverses the list.

    `enum { … } flags;`
    :   A set of flag values that convey information between the extension
        and `gawk`. Currently there is only one: `AWK_ELEMENT_DELETE`.
        Setting it causes `gawk` to delete the
        element from the original array upon release of the flattened array.

    `index`

    `value`
    :   The index and value of the element, respectively.
        *All* memory pointed to by `index` and `value` belongs to `gawk`.

`typedef struct awk_flat_array {`

`awk_const void *awk_const opaque1;    /* for use by gawk */`

`awk_const void *awk_const opaque2;    /* for use by gawk */`

`awk_const size_t count;     /* how many elements */`

`awk_element_t elements[1];  /* will be extended */`

`} awk_flat_array_t;`
:   This is a flattened array. When an extension gets one of these
    from `gawk`, the `elements` array is of actual
    size `count`.
    The `opaque1` and `opaque2` pointers are for use by `gawk`;
    therefore they are marked `awk_const` so that the extension cannot
    modify them.

---

Next: [Working With All The Elements of an Array](#Flattening-Arrays), Previous: [Array Data Types](#Array-Data-Types), Up: [Array Manipulation](#Array-Manipulation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.12.2 Array Functions [¶](#Array-Functions-1)

The following functions relate to individual array elements:

`awk_bool_t get_element_count(awk_array_t a_cookie, size_t *count);`
:   For the array represented by `a_cookie`, place in `*count`
    the number of elements it contains. A subarray counts as a single element.
    Return false if there is an error.

`awk_bool_t get_array_element(awk_array_t a_cookie,`

`const awk_value_t *const index,`

`awk_valtype_t wanted,`

`awk_value_t *result);`
:   For the array represented by `a_cookie`, return in `*result`
    the value of the element whose index is `index`.
    `wanted` specifies the type of value you wish to retrieve.
    Return false if `wanted` does not match the actual type or if
    `index` is not in the array (see [Table 17.2](#table_002dvalue_002dtypes_002dreturned)).

    The value for `index` can be numeric, in which case `gawk`
    converts it to a string. Using nonintegral values is possible, but
    requires that you understand how such values are converted to strings
    (see [Conversion of Strings and Numbers](#Conversion)); thus, using integral values is safest.

    As with *all* strings passed into `gawk` from an extension,
    the string value of `index` must come from `gawk_malloc()`,
    `gawk_calloc()`, or `gawk_realloc()`, and
    `gawk` releases the storage.

`awk_bool_t set_array_element(awk_array_t a_cookie,`

`const awk_value_t *const index,`

`const awk_value_t *const value);`
:   In the array represented by `a_cookie`, create or modify
    the element whose index is given by `index`.
    The `ARGV` and `ENVIRON` arrays may not be changed,
    although the `PROCINFO` array can be.
    Note that `index` should be string value.
    However, `gawk` will force a conversion to string if necessary.

`awk_bool_t set_array_element_by_elem(awk_array_t a_cookie,`

`awk_element_t element);`
:   Like `set_array_element()`, but take the `index` and `value`
    from `element`. This is a convenience macro.

`awk_bool_t del_array_element(awk_array_t a_cookie,`

`const awk_value_t* const index);`
:   Remove the element with the given index from the array
    represented by `a_cookie`.
    Return true if the element was removed, or false if the element did
    not exist in the array.

The following functions relate to arrays as a whole:

`awk_array_t create_array(void);`
:   Create a new array to which elements may be added.
    See [How To Create and Populate Arrays](#Creating-Arrays) for a discussion of how to
    create a new array and add elements to it.

`awk_bool_t clear_array(awk_array_t a_cookie);`
:   Clear the array represented by `a_cookie`.
    Return false if there was some kind of problem, true otherwise.
    The array remains an array, but after calling this function, it
    has no elements. This is equivalent to using the `delete`
    statement (see [The `delete` Statement](#Delete)).

`awk_bool_t destroy_array(awk_array_t a_cookie);`
:   Clear the array represented by `a_cookie` and release the array
    allocated by `create_array`.
    Return false if there was some kind of problem, true otherwise.
    The array will no longer exist and cannot be used again.

`awk_bool_t flatten_array_typed(awk_array_t a_cookie,`

`awk_flat_array_t **data,`

`awk_valtype_t index_type,`

`awk_valtype_t value_type);`
:   For the array represented by `a_cookie`, create an `awk_flat_array_t`
    structure and fill it in with indices and values of the requested types.
    Set the pointer whose address is passed as `data`
    to point to this structure.
    Return true upon success, or false otherwise.
    See [Working With All The Elements of an Array](#Flattening-Arrays),
    for a discussion of how to
    flatten an array and work with it.

`awk_bool_t flatten_array(awk_array_t a_cookie, awk_flat_array_t **data);`
:   For the array represented by `a_cookie`, create an `awk_flat_array_t`
    structure and fill it in with `AWK_STRING` indices and
    `AWK_UNDEFINED` values.
    This is superseded by `flatten_array_typed()`.
    It is provided as a macro, and remains for convenience and for source code
    compatibility with the previous version of the API.

`awk_bool_t release_flattened_array(awk_array_t a_cookie,`
