`awk_flat_array_t *data);`
:   When done with a flattened array, release the storage using this function.
    You must pass in both the original array cookie and the address of
    the created `awk_flat_array_t` structure.
    The function returns true upon success, false otherwise.

---

Next: [How To Create and Populate Arrays](#Creating-Arrays), Previous: [Array Functions](#Array-Functions), Up: [Array Manipulation](#Array-Manipulation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.12.3 Working With All The Elements of an Array [¶](#Working-With-All-The-Elements-of-an-Array)

To *flatten* an array is to create a structure that
represents the full array in a fashion that makes it easy
for C code to traverse the entire array. Some of the code
in `extension/testext.c` does this, and also serves
as a nice example showing how to use the APIs.

We walk through that part of the code one step at a time.
First, the `gawk` script that drives the test extension:

```
@load "testext"
BEGIN {
    n = split("blacky rusty sophie raincloud lucky", pets)
    printf("pets has %d elements\n", length(pets))
    ret = dump_array_and_delete("pets", "3")
    printf("dump_array_and_delete(pets) returned %d\n", ret)
    if ("3" in pets)
        printf("dump_array_and_delete() did NOT remove index \"3\"!\n")
    else
        printf("dump_array_and_delete() did remove index \"3\"!\n")
    print ""
}
```

This code creates an array with `split()` (see [String-Manipulation Functions](#String-Functions))
and then calls `dump_array_and_delete()`. That function looks up
the array whose name is passed as the first argument, and
deletes the element at the index passed in the second argument.
The `awk` code then prints the return value and checks if the element
was indeed deleted. Here is the C code that implements
`dump_array_and_delete()`. It has been edited slightly for
presentation.

The first part declares variables, sets up the default
return value in `result`, and checks that the function
was called with the correct number of arguments:

```
static awk_value_t *
dump_array_and_delete(int nargs, awk_value_t *result)
{
    awk_value_t value, value2, value3;
    awk_flat_array_t *flat_array;
    size_t count;
    char *name;
    int i;

    assert(result != NULL);
    make_number(0.0, result);

    if (nargs != 2) {
        printf("dump_array_and_delete: nargs not right "
               "(%d should be 2)\n", nargs);
        goto out;
    }
```

The function then proceeds in steps, as follows. First, retrieve
the name of the array, passed as the first argument, followed by
the array itself. If either operation fails, print an
error message and return:

```
    /* get argument named array as flat array and print it */
    if (get_argument(0, AWK_STRING, & value)) {
        name = value.str_value.str;
        if (sym_lookup(name, AWK_ARRAY, & value2))
            printf("dump_array_and_delete: sym_lookup of %s passed\n",
                   name);
        else {
            printf("dump_array_and_delete: sym_lookup of %s failed\n",
                   name);
            goto out;
        }
    } else {
        printf("dump_array_and_delete: get_argument(0) failed\n");
        goto out;
    }
```

For testing purposes and to make sure that the C code sees
the same number of elements as the `awk` code,
the second step is to get the count of elements in the array
and print it:

```
    if (! get_element_count(value2.array_cookie, & count)) {
        printf("dump_array_and_delete: get_element_count failed\n");
        goto out;
    }

    printf("dump_array_and_delete: incoming size is %lu\n",
           (unsigned long) count);
```

The third step is to actually flatten the array, and then
to double-check that the count in the `awk_flat_array_t`
is the same as the count just retrieved:

```
    if (! flatten_array_typed(value2.array_cookie, & flat_array,
                              AWK_STRING, AWK_UNDEFINED)) {
        printf("dump_array_and_delete: could not flatten array\n");
        goto out;
    }

    if (flat_array->count != count) {
        printf("dump_array_and_delete: flat_array->count (%lu)"
               " != count (%lu)\n",
                (unsigned long) flat_array->count,
                (unsigned long) count);
        goto out;
    }
```

The fourth step is to retrieve the index of the element
to be deleted, which was passed as the second argument.
Remember that argument counts passed to `get_argument()`
are zero-based, and thus the second argument is numbered one:

```
    if (! get_argument(1, AWK_STRING, & value3)) {
        printf("dump_array_and_delete: get_argument(1) failed\n");
        goto out;
    }
```

The fifth step is where the “real work” is done. The function
loops over every element in the array, printing the index and
element values. In addition, upon finding the element with the
index that is supposed to be deleted, the function sets the
`AWK_ELEMENT_DELETE` bit in the `flags` field
of the element. When the array is released, `gawk`
traverses the flattened array, and deletes any elements that
have this flag bit set:

```
    for (i = 0; i < flat_array->count; i++) {
        printf("\t%s[\"%.*s\"] = %s\n",
            name,
            (int) flat_array->elements[i].index.str_value.len,
            flat_array->elements[i].index.str_value.str,
            valrep2str(& flat_array->elements[i].value));

        if (strcmp(value3.str_value.str,
                   flat_array->elements[i].index.str_value.str) == 0) {
            flat_array->elements[i].flags |= AWK_ELEMENT_DELETE;
            printf("dump_array_and_delete: marking element \"%s\" "
                   "for deletion\n",
                flat_array->elements[i].index.str_value.str);
        }
    }
```

The sixth step is to release the flattened array. This tells
`gawk` that the extension is no longer using the array,
and that it should delete any elements marked for deletion.
`gawk` also frees any storage that was allocated,
so you should not use the pointer (`flat_array` in this
code) once you have called `release_flattened_array()`:

```
    if (! release_flattened_array(value2.array_cookie, flat_array)) {
        printf("dump_array_and_delete: could not release flattened array\n");
        goto out;
    }
```

Finally, because everything was successful, the function sets the
return value to success, and returns:

```
    make_number(1.0, result);
out:
    return result;
}
```

Here is the output from running this part of the test:

```
pets has 5 elements
dump_array_and_delete: sym_lookup of pets passed
dump_array_and_delete: incoming size is 5
        pets["1"] = "blacky"
        pets["2"] = "rusty"
        pets["3"] = "sophie"
dump_array_and_delete: marking element "3" for deletion
        pets["4"] = "raincloud"
        pets["5"] = "lucky"
dump_array_and_delete(pets) returned 1
dump_array_and_delete() did remove index "3"!
```

---

Previous: [Working With All The Elements of an Array](#Flattening-Arrays), Up: [Array Manipulation](#Array-Manipulation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.12.4 How To Create and Populate Arrays [¶](#How-To-Create-and-Populate-Arrays)

Besides working with arrays created by `awk` code, you can
create arrays and populate them as you see fit, and then `awk`
code can access them and manipulate them.

There are two important points about creating arrays from extension code:

* You must install a new array into `gawk`’s symbol
  table immediately upon creating it. Once you have done so,
  you can then populate the array.

  Similarly, if installing a new array as a subarray of an existing array,
  you must add the new array to its parent before adding any elements to it.

  Thus, the correct way to build an array is to work “top down.” Create
  the array, and immediately install it in `gawk`’s symbol table
  using `sym_update()`, or install it as an element in a previously
  existing array using `set_array_element()`. We show example code shortly.
* Due to `gawk` internals, after using `sym_update()` to install an array
  into `gawk`, you have to retrieve the array cookie from the value
  passed in to `sym_update()` before doing anything else with it, like so:

  ```
  awk_value_t val;
  awk_array_t new_array;

  new_array = create_array();
  val.val_type = AWK_ARRAY;
  val.array_cookie = new_array;

  /* install array in the symbol table */
  sym_update("array", & val);

  new_array = val.array_cookie;    /* YOU MUST DO THIS */
  ```

  If installing an array as a subarray, you must also retrieve the value
  of the array cookie after the call to `set_element()`.

The following C code is a simple test extension to create an array
with two regular elements and with a subarray. The leading `#include`
directives and boilerplate variable declarations
(see [Boilerplate Code](#Extension-API-Boilerplate))
are omitted for brevity.
The first step is to create a new array and then install it
in the symbol table:

```
/* create_new_array --- create a named array */

static void
create_new_array()
{
    awk_array_t a_cookie;
    awk_array_t subarray;
    awk_value_t index, value;

    a_cookie = create_array();
    value.val_type = AWK_ARRAY;
    value.array_cookie = a_cookie;

    if (! sym_update("new_array", & value))
        printf("create_new_array: sym_update(\"new_array\") failed!\n");
    a_cookie = value.array_cookie;
```

Note how `a_cookie` is reset from the `array_cookie` field in
the `value` structure.

The second step is to install two regular values into `new_array`:

```
    (void) make_const_string("hello", 5, & index);
    (void) make_const_string("world", 5, & value);
    if (! set_array_element(a_cookie, & index, & value)) {
        printf("fill_in_array: set_array_element failed\n");
        return;
    }

    (void) make_const_string("answer", 6, & index);
    (void) make_number(42.0, & value);
    if (! set_array_element(a_cookie, & index, & value)) {
        printf("fill_in_array: set_array_element failed\n");
        return;
    }
```

The third step is to create the subarray and install it:

```
    (void) make_const_string("subarray", 8, & index);
    subarray = create_array();
    value.val_type = AWK_ARRAY;
    value.array_cookie = subarray;
    if (! set_array_element(a_cookie, & index, & value)) {
        printf("fill_in_array: set_array_element failed\n");
        return;
    }
    subarray = value.array_cookie;
```

The final step is to populate the subarray with its own element:

```
    (void) make_const_string("foo", 3, & index);
    (void) make_const_string("bar", 3, & value);
    if (! set_array_element(subarray, & index, & value)) {
        printf("fill_in_array: set_array_element failed\n");
        return;
    }
}
```

Here is a sample script that loads the extension
and then dumps the array:

```
@load "subarray"

function dumparray(name, array,     i)
{
    for (i in array)
        if (isarray(array[i]))
            dumparray(name "[\"" i "\"]", array[i])
        else
            printf("%s[\"%s\"] = %s\n", name, i, array[i])
}

BEGIN {
    dumparray("new_array", new_array);
}
```

Here is the result of running the script:

```
$ AWKLIBPATH=$PWD gawk -f subarray.awk
-| new_array["subarray"]["foo"] = bar
-| new_array["hello"] = world
-| new_array["answer"] = 42
```

(See [How `gawk` Finds Extensions](#Finding-Extensions) for more information on the
`AWKLIBPATH` environment variable.)

---

Next: [API Variables](#Extension-API-Variables), Previous: [Array Manipulation](#Array-Manipulation), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.13 Accessing and Manipulating Redirections [¶](#Accessing-and-Manipulating-Redirections)

The following function allows extensions to access and manipulate redirections.

`awk_bool_t get_file(const char *name,`

`size_t name_len,`

`const char *filetype,`

`int fd,`

`const awk_input_buf_t **ibufp,`

`const awk_output_buf_t **obufp);`
:   Look up file `name` in `gawk`’s internal redirection table.
    If `name` is `NULL` or `name_len` is zero, return
    data for the currently open input file corresponding to `FILENAME`.
    (This does not access the `filetype` argument, so that may be undefined).
    If the file is not already open, attempt to open it.
    The `filetype` argument must be zero-terminated and should be one of:

    `">"`
    :   A file opened for output.

    `">>"`
    :   A file opened for append.

    `"<"`
    :   A file opened for input.

    `"|>"`
    :   A pipe opened for output.

    `"|<"`
    :   A pipe opened for input.

    `"|&"`
    :   A two-way coprocess.

    On error, return `awk_false`. Otherwise, return
    `awk_true`, and return additional information about the redirection
    in the `ibufp` and `obufp` pointers.

    For input redirections, the `*ibufp` value should be non-`NULL`,
    and `*obufp` should be `NULL`. For output redirections,
    the `*obufp` value should be non-`NULL`, and `*ibufp`
    should be `NULL`. For two-way coprocesses, both values should
    be non-`NULL`.

    In the usual case, the extension is interested in `(*ibufp)->fd`
    and/or `fileno((*obufp)->fp)`. If the file is not already
    open, and the `fd` argument is nonnegative, `gawk`
    will use that file descriptor instead of opening the file in the
    usual way. If `fd` is nonnegative, but the file exists already,
    `gawk` ignores `fd` and returns the existing file. It is
    the caller’s responsibility to notice that neither the `fd` in
    the returned `awk_input_buf_t` nor the `fd` in the returned
    `awk_output_buf_t` matches the requested value.

    Note that supplying a file descriptor is currently *not* supported
    for pipes. However, supplying a file descriptor should work for input,
    output, append, and two-way (coprocess) sockets. If `filetype`
    is two-way, `gawk` assumes that it is a socket! Note that in
    the two-way case, the input and output file descriptors may differ.
    To check for success, you must check whether either matches.

It is anticipated that this API function will be used to implement I/O
multiplexing and a socket library.

---

Next: [Boilerplate Code](#Extension-API-Boilerplate), Previous: [Accessing and Manipulating Redirections](#Redirection-API), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.14 API Variables [¶](#API-Variables)

The API provides two sets of variables. The first provides information
about the version of the API (both with which the extension was compiled,
and with which `gawk` was compiled). The second provides
information about how `gawk` was invoked.

* [API Version Constants and Variables](#Extension-Versioning)
* [GMP and MPFR Version Information](#Extension-GMP_002fMPFR-Versioning)
* [Informational Variables](#Extension-API-Informational-Variables)

---

Next: [GMP and MPFR Version Information](#Extension-GMP_002fMPFR-Versioning), Up: [API Variables](#Extension-API-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.14.1 API Version Constants and Variables [¶](#API-Version-Constants-and-Variables)

The API provides both a “major” and a “minor” version number.
The API versions are available at compile time as C preprocessor defines
to support conditional compilation, and as enum constants to facilitate
debugging:

| API Version | C Preprocessor Define | enum constant |
| --- | --- | --- |
| Major | `gawk_api_major_version` | `GAWK_API_MAJOR_VERSION` |
| Minor | `gawk_api_minor_version` | `GAWK_API_MINOR_VERSION` |

**Table 17.3:** gawk API version constants

The minor version increases when new functions are added to the API. Such
new functions are always added to the end of the API `struct`.

The major version increases (and the minor version is reset to zero) if any
of the data types change size or member order, or if any of the existing
functions change signature.

It could happen that an extension may be compiled against one version
of the API but loaded by a version of `gawk` using a different
version. For this reason, the major and minor API versions of the
running `gawk` are included in the API `struct` as read-only
constant integers:

`api->major_version`
:   The major version of the running `gawk`.

`api->minor_version`
:   The minor version of the running `gawk`.

It is up to the extension to decide if there are API incompatibilities.
Typically, a check like this is enough:

```
if (   api->major_version != GAWK_API_MAJOR_VERSION
    || api->minor_version < GAWK_API_MINOR_VERSION) {
        fprintf(stderr, "foo_extension: version mismatch with gawk!\n");
        fprintf(stderr, "\tmy version (%d, %d), gawk version (%d, %d)\n",
                GAWK_API_MAJOR_VERSION, GAWK_API_MINOR_VERSION,
                api->major_version, api->minor_version);
        exit(1);
}
```

Such code is included in the boilerplate `dl_load_func()` macro
provided in `gawkapi.h` (discussed in
[Boilerplate Code](#Extension-API-Boilerplate)).

---

Next: [Informational Variables](#Extension-API-Informational-Variables), Previous: [API Version Constants and Variables](#Extension-Versioning), Up: [API Variables](#Extension-API-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.14.2 GMP and MPFR Version Information [¶](#GMP-and-MPFR-Version-Information)

The API also includes information about the versions of GMP and MPFR
with which the running `gawk` was compiled (if any).
They are included in the API `struct` as read-only
constant integers:

`api->gmp_major_version`
:   The major version of the GMP library used to compile `gawk`.

`api->gmp_minor_version`
:   The minor version of the GMP library used to compile `gawk`.

`api->mpfr_major_version`
:   The major version of the MPFR library used to compile `gawk`.

`api->mpfr_minor_version`
:   The minor version of the MPFR library used to compile `gawk`.

These fields are set to zero if `gawk` was compiled without
MPFR support.

You can check if the versions of MPFR and GMP that you are using match those
of `gawk` with the following macro:

`check_mpfr_version(extension)`
:   The `extension` is the extension id passed to all the other macros
    and functions defined in `gawkapi.h`. If you have not included
    the `<mpfr.h>` header file, then this macro will be defined to do nothing.

    If you have included that file, then this macro compares the MPFR
    and GMP major and minor versions against those of the library you are
    compiling against. If your libraries are newer than `gawk`’s, it
    produces a fatal error message.

    The `dl_load_func()` macro (see [Boilerplate Code](#Extension-API-Boilerplate))
    calls `check_mpfr_version()`.

---

Previous: [GMP and MPFR Version Information](#Extension-GMP_002fMPFR-Versioning), Up: [API Variables](#Extension-API-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.14.3 Informational Variables [¶](#Informational-Variables)

The API provides access to several variables that describe
whether the corresponding command-line options were enabled when
`gawk` was invoked. The variables are:

`do_csv`
:   This variable is true if `gawk` was invoked with `--csv` option.

`do_debug`
:   This variable is true if `gawk` was invoked with `--debug` option.

`do_lint`
:   This variable is true if `gawk` was invoked with `--lint` option.

`do_mpfr`
:   This variable is true if `gawk` was invoked with `--bignum` option.

`do_profile`
:   This variable is true if `gawk` was invoked with `--profile` option.

`do_sandbox`
:   This variable is true if `gawk` was invoked with `--sandbox` option.

`do_traditional`
:   This variable is true if `gawk` was invoked with `--traditional` option.

The value of `do_lint` can change if `awk` code
modifies the `LINT` predefined variable (see [Predefined Variables](#Built_002din-Variables)).
The others should not change during execution.

---

Next: [Changes From Version 1 of the API](#Changes-from-API-V1), Previous: [API Variables](#Extension-API-Variables), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.15 Boilerplate Code [¶](#Boilerplate-Code)

As mentioned earlier (see [How It Works at a High Level](#Extension-Mechanism-Outline)), the function
definitions as presented are really macros. To use these macros, your
extension must provide a small amount of boilerplate code (variables and
functions) toward the top of your source file, using predefined names
as described here. The boilerplate needed is also provided in comments
in the `gawkapi.h` header file:

```
/* Boilerplate code: */
int plugin_is_GPL_compatible;

static gawk_api_t *const api;
```

```
static awk_ext_id_t ext_id;
static const char *ext_version = NULL; /* or ... = "some string" */

static awk_ext_func_t func_table[] = {
    { "name", do_name, 1, 0, awk_false, NULL },
    /* ... */
};

/* EITHER: */

static awk_bool_t (*init_func)(void) = NULL;

/* OR: */

static awk_bool_t
init_my_extension(void)
{
    ...
}

static awk_bool_t (*init_func)(void) = init_my_extension;

dl_load_func(func_table, some_name, "name_space_in_quotes")
```

These variables and functions are as follows:

`int plugin_is_GPL_compatible;`
:   This asserts that the extension is compatible with
    the GNU GPL (see [GNU General Public License](#Copying)).
    If your extension does not have this, `gawk`
    will not load it (see [Extension Licensing](#Plugin-License)).

`static gawk_api_t *const api;`
:   This global `static` variable should be set to point to
    the `gawk_api_t` pointer that `gawk` passes to your
    `dl_load()` function. This variable is used by all of the macros.

`static awk_ext_id_t ext_id;`
:   This global static variable should be set to the `awk_ext_id_t`
    value that `gawk` passes to your `dl_load()` function.
    This variable is used by all of the macros.

`static const char *ext_version = NULL; /* or … = "some string" */`
:   This global `static` variable should be set either
    to `NULL`, or to point to a string giving the name and version of
    your extension.

`static awk_ext_func_t func_table[] = { … };`
:   This is an array of one or more `awk_ext_func_t` structures,
    as described earlier (see [Registering An Extension Function](#Extension-Functions)).
    It can then be looped over for multiple calls to
    `add_ext_func()`.

`static awk_bool_t (*init_func)(void) = NULL;`

`OR`

`static awk_bool_t init_my_extension(void) { … }`

`static awk_bool_t (*init_func)(void) = init_my_extension;`
:   If you need to do some initialization work, you should define a
    function that does it (creates variables, opens files, etc.)
    and then define the `init_func` pointer to point to your
    function.
    The function should return `awk_false` upon failure, or `awk_true`
    if everything goes well.

    If you don’t need to do any initialization, define the pointer and
    initialize it to `NULL`.

`dl_load_func(func_table, some_name, "name_space_in_quotes")`
:   This macro expands to a `dl_load()` function that performs
    all the necessary initializations.

The point of all the variables and arrays is to let the
`dl_load()` function (from the `dl_load_func()`
macro) do all the standard work. It does the following:

1. Check the API versions. If the extension major version does not match
   `gawk`’s, or if the extension minor version is greater than
   `gawk`’s, it prints a fatal error message and exits.
2. Check the MPFR and GMP versions. If there is a mismatch, it prints
   a fatal error message and exits.
3. Load the functions defined in `func_table`.
   If any of them fails to load, it prints a warning message but
   continues on.
4. If the `init_func` pointer is not `NULL`, call the
   function it points to. If it returns `awk_false`, print a
   warning message.
5. If `ext_version` is not `NULL`, register
   the version string with `gawk`.

---

Previous: [Boilerplate Code](#Extension-API-Boilerplate), Up: [API Description](#Extension-API-Description)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.4.16 Changes From Version 1 of the API [¶](#Changes-From-Version-1-of-the-API)

The current API is *not* binary compatible with version 1 of the API.
You will have to recompile your extensions in order to use them with
the current version of `gawk`.

Fortunately, at the possible expense of some compile-time warnings, the API remains
source-code–compatible with the previous API. The major differences are
the additional members in the `awk_ext_func_t` structure, and the
addition of the third argument to the C implementation function
(see [Registering An Extension Function](#Extension-Functions)).

Here is a list of individual features that changed from version 1 to
version 2 of the API:

* Numeric values can now have MPFR/MPZ variants
  (see [General-Purpose Data Types](#General-Data-Types)).
* There are new string types: `AWK_REGEX` and `AWK_STRNUM`
  (see [General-Purpose Data Types](#General-Data-Types)).
* The `ezalloc()` macro is new
  (see [Memory Allocation Functions and Convenience Macros](#Memory-Allocation-Functions)).
* The `awk_ext_func_t` structure changed. Instead of
  `num_expected_args`, it now has `max_expected` and
  `min_required`
  (see [Registering An Extension Function](#Extension-Functions)).
* For `get_record()`, an input parser can now specify field widths
  (see [Customized Input Parsers](#Input-Parsers)).
* Extensions can now produce nonfatal error messages
  (see [Printing Messages](#Printing-Messages)).
* When flattening an array, you can now specify the index and value types
  (see [Array Functions](#Array-Functions)).
* The `get_file()` API is new
  (see [Accessing and Manipulating Redirections](#Redirection-API)).

---

Next: [Example: Some File Functions](#Extension-Example), Previous: [API Description](#Extension-API-Description), Up: [Writing Extensions for `gawk`](#Dynamic-Extensions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.5 How `gawk` Finds Extensions [¶](#How-gawk-Finds-Extensions)

Compiled extensions have to be installed in a directory where
`gawk` can find them. If `gawk` is configured and
built in the default fashion, the directory in which to find
extensions is `/usr/local/lib/gawk`. You can also specify a search
path with a list of directories to search for compiled extensions.
See [The `AWKLIBPATH` Environment Variable](#AWKLIBPATH-Variable) for more information.

---

Next: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples), Previous: [How `gawk` Finds Extensions](#Finding-Extensions), Up: [Writing Extensions for `gawk`](#Dynamic-Extensions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.6 Example: Some File Functions [¶](#Example_003a-Some-File-Functions)

> *No matter where you go, there you are.*

— *Buckaroo Banzai*

Two useful functions that are not in `awk` are `chdir()` (so
that an `awk` program can change its directory) and `stat()`
(so that an `awk` program can gather information about a file).
In order to illustrate the API in action, this section implements
these functions for `gawk` in an extension.

* [Using `chdir()` and `stat()`](#Internal-File-Description)
* [C Code for `chdir()` and `stat()`](#Internal-File-Ops)
* [Integrating the Extensions](#Using-Internal-File-Ops)

---

Next: [C Code for `chdir()` and `stat()`](#Internal-File-Ops), Up: [Example: Some File Functions](#Extension-Example)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.6.1 Using `chdir()` and `stat()` [¶](#Using-chdir_0028_0029-and-stat_0028_0029)

This section shows how to use the new functions at
the `awk` level once they’ve been integrated into the
running `gawk` interpreter. Using `chdir()` is very
straightforward. It takes one argument, the new directory to change to:

```
@load "filefuncs"
...
newdir = "/home/arnold/funstuff"
ret = chdir(newdir)
if (ret < 0) {
    printf("could not change to %s: %s\n", newdir, ERRNO) > "/dev/stderr"
    exit 1
}
...
```

The return value is negative if the `chdir()` failed, and
`ERRNO` (see [Predefined Variables](#Built_002din-Variables)) is set to a string indicating
the error.

Using `stat()` is a bit more complicated. The C `stat()`
function fills in a structure that has a fair amount of information.
The right way to model this in `awk` is to fill in an associative
array with the appropriate information:

```
file = "/home/arnold/.profile"
ret = stat(file, fdata)
if (ret < 0) {
    printf("could not stat %s: %s\n",
             file, ERRNO) > "/dev/stderr"
    exit 1
}
printf("size of %s is %d bytes\n", file, fdata["size"])
```

The `stat()` function always clears the data array, even if
the `stat()` fails. It fills in the following elements:

`"name"`
:   The name of the file that was `stat()`ed.

`"dev"`

`"ino"`
:   The file’s device and inode numbers, respectively.

`"mode"`
:   The file’s mode, as a numeric value. This includes both the file’s
    type and its permissions.

`"nlink"`
:   The number of hard links (directory entries) the file has.

`"uid"`

`"gid"`
:   The numeric user and group ID numbers of the file’s owner.

`"size"`
:   The size in bytes of the file.

`"blocks"`
:   The number of disk blocks the file actually occupies. This may not
    be a function of the file’s size if the file has holes.

`"atime"`

`"mtime"`

`"ctime"`
:   The file’s last access, modification, and inode update times,
    respectively. These are numeric timestamps, suitable for formatting
    with `strftime()`
    (see [Time Functions](#Time-Functions)).

`"pmode"`
:   The file’s “printable mode.” This is a string representation of
    the file’s type and permissions, such as is produced by
    ‘`ls -l`’—for example, `"drwxr-xr-x"`.

`"type"`
:   A printable string representation of the file’s type. The value
    is one of the following:

    `"blockdev"`

    `"chardev"`
    :   The file is a block or character device (“special file”).

    `"directory"`
    :   The file is a directory.

    `"fifo"`
    :   The file is a named pipe (also known as a FIFO).

    `"file"`
    :   The file is just a regular file.

    `"socket"`
    :   The file is an `AF_UNIX` (“Unix domain”) socket in the
        filesystem.

    `"symlink"`
    :   The file is a symbolic link.

`"devbsize"`
:   The size of a block for the element indexed by `"blocks"`.
    This information is derived from either the `DEV_BSIZE`
    constant defined in `<sys/param.h>` on most systems,
    or the `S_BLKSIZE` constant in `<sys/stat.h>` on BSD systems.
    For some other systems, *a priori* knowledge is used to provide
    a value. Where no value can be determined, it defaults to 512.

Several additional elements may be present, depending upon the operating
system and the type of the file. You can test for them in your `awk`
program by using the `in` operator
(see [Referring to an Array Element](#Reference-to-Elements)):

`"blksize"`
:   The preferred block size for I/O to the file. This field is not
    present on all POSIX-like systems in the C `stat` structure.

`"linkval"`
:   If the file is a symbolic link, this element is the name of the
    file the link points to (i.e., the value of the link).

`"rdev"`

`"major"`

`"minor"`
:   If the file is a block or character device file, then these values
    represent the numeric device number and the major and minor components
    of that number, respectively.

---

Next: [Integrating the Extensions](#Using-Internal-File-Ops), Previous: [Using `chdir()` and `stat()`](#Internal-File-Description), Up: [Example: Some File Functions](#Extension-Example)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.6.2 C Code for `chdir()` and `stat()` [¶](#C-Code-for-chdir_0028_0029-and-stat_0028_0029)

Here is the C code for these extensions.[116](#FOOT116)

The file includes a number of standard header files, and then includes
the `gawkapi.h` header file, which provides the API definitions.
Those are followed by the necessary variable declarations
to make use of the API macros and boilerplate code
(see [Boilerplate Code](#Extension-API-Boilerplate)):

```
#ifdef HAVE_CONFIG_H
#include <config.h>
#endif

#include <stdio.h>
#include <assert.h>
#include <errno.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

#include <sys/types.h>
#include <sys/stat.h>

#include "gawkapi.h"

#include "gettext.h"
#define _(msgid)  gettext(msgid)
#define N_(msgid) msgid

#include "gawkfts.h"
#include "stack.h"

static const gawk_api_t *api;    /* for convenience macros to work */
static awk_ext_id_t ext_id;
static awk_bool_t init_filefuncs(void);
static awk_bool_t (*init_func)(void) = init_filefuncs;
static const char *ext_version = "filefuncs extension: version 1.0";

int plugin_is_GPL_compatible;
```

By convention, for an `awk` function `foo()`, the C function
that implements it is called `do_foo()`. The function should have
two arguments. The first is an `int`, usually called `nargs`,
that represents the number of actual arguments for the function.
The second is a pointer to an `awk_value_t` structure, usually named
`result`:

```
/*  do_chdir --- provide dynamically loaded chdir() function for gawk */

static awk_value_t *
do_chdir(int nargs, awk_value_t *result, struct awk_ext_func *unused)
```

```
{
    awk_value_t newdir;
    int ret = -1;

    assert(result != NULL);
```

The `newdir`
variable represents the new directory to change to, which is retrieved
with `get_argument()`. Note that the first argument is
numbered zero.

If the argument is retrieved successfully, the function calls the
`chdir()` system call. Otherwise, if the `chdir()` fails,
it updates `ERRNO`:

```
    if (get_argument(0, AWK_STRING, & newdir)) {
        ret = chdir(newdir.str_value.str);
        if (ret < 0)
            update_ERRNO_int(errno);
    }
```

Finally, the function returns the return value to the `awk` level:

```
    return make_number(ret, result);
}
```

The `stat()` extension is more involved. First comes a function
that turns a numeric mode into a printable representation
(e.g., octal `0644` becomes ‘`-rw-r--r--`’). This is omitted here for brevity:

```
/* format_mode --- turn a stat mode field into something readable */

static char *
format_mode(unsigned long fmode)
{
    ...
}
```

Next comes a function for reading symbolic links, which is also
omitted here for brevity:

```
/* read_symlink --- read a symbolic link into an allocated buffer.
   ... */

static char *
read_symlink(const char *fname, size_t bufsize, ssize_t *linksize)
{
    ...
}
```

Two helper functions simplify entering values in the
array that will contain the result of the `stat()`:

```
/* array_set --- set an array element */

static void
array_set(awk_array_t array, const char *sub, awk_value_t *value)
{
    awk_value_t index;

    set_array_element(array,
                      make_const_string(sub, strlen(sub), & index),
                      value);

}

/* array_set_numeric --- set an array element with a number */

static void
array_set_numeric(awk_array_t array, const char *sub, double num)
{
    awk_value_t tmp;

    array_set(array, sub, make_number(num, & tmp));
}
```

The following function does most of the work to fill in
the `awk_array_t` result array with values obtained
from a valid `struct stat`. This work is done in a separate function
to support the `stat()` function for `gawk` and also
to support the `fts()` extension, which is included in
the same file but whose code is not shown here
(see [File-Related Functions](#Extension-Sample-File-Functions)).

The first part of the function is variable declarations,
including a table to map file types to strings:

```
/* fill_stat_array --- do the work to fill an array with stat info */

static int
fill_stat_array(const char *name, awk_array_t array, struct stat *sbuf)
{
    char *pmode;    /* printable mode */
    const char *type = "unknown";
    awk_value_t tmp;
    static struct ftype_map {
        unsigned int mask;
        const char *type;
    } ftype_map[] = {
        { S_IFREG, "file" },
        { S_IFBLK, "blockdev" },
        { S_IFCHR, "chardev" },
        { S_IFDIR, "directory" },
#ifdef S_IFSOCK
        { S_IFSOCK, "socket" },
#endif
#ifdef S_IFIFO
        { S_IFIFO, "fifo" },
#endif
#ifdef S_IFLNK
        { S_IFLNK, "symlink" },
#endif
#ifdef S_IFDOOR /* Solaris weirdness */
        { S_IFDOOR, "door" },
#endif
    };
    int j, k;
```

The destination array is cleared, and then code fills in
various elements based on values in the `struct stat`:

```
    /* empty out the array */
    clear_array(array);

    /* fill in the array */
    array_set(array, "name", make_const_string(name, strlen(name),
                                               & tmp));
    array_set_numeric(array, "dev", sbuf->st_dev);
    array_set_numeric(array, "ino", sbuf->st_ino);
    array_set_numeric(array, "mode", sbuf->st_mode);
    array_set_numeric(array, "nlink", sbuf->st_nlink);
    array_set_numeric(array, "uid", sbuf->st_uid);
    array_set_numeric(array, "gid", sbuf->st_gid);
    array_set_numeric(array, "size", sbuf->st_size);
    array_set_numeric(array, "blocks", sbuf->st_blocks);
    array_set_numeric(array, "atime", sbuf->st_atime);
    array_set_numeric(array, "mtime", sbuf->st_mtime);
    array_set_numeric(array, "ctime", sbuf->st_ctime);

    /* for block and character devices, add rdev,
       major and minor numbers */
    if (S_ISBLK(sbuf->st_mode) || S_ISCHR(sbuf->st_mode)) {
        array_set_numeric(array, "rdev", sbuf->st_rdev);
        array_set_numeric(array, "major", major(sbuf->st_rdev));
        array_set_numeric(array, "minor", minor(sbuf->st_rdev));
    }
```

The latter part of the function makes selective additions
to the destination array, depending upon the availability of
certain members and/or the type of the file. It then returns zero,
for success:

```
#ifdef HAVE_STRUCT_STAT_ST_BLKSIZE
    array_set_numeric(array, "blksize", sbuf->st_blksize);
#endif
```

```
    pmode = format_mode(sbuf->st_mode);
    array_set(array, "pmode", make_const_string(pmode, strlen(pmode),
                                                & tmp));

    /* for symbolic links, add a linkval field */
    if (S_ISLNK(sbuf->st_mode)) {
        char *buf;
        ssize_t linksize;

        if ((buf = read_symlink(name, sbuf->st_size,
                    & linksize)) != NULL)
            array_set(array, "linkval",
                      make_malloced_string(buf, linksize, & tmp));
        else
            warning(ext_id, _("stat: unable to read symbolic link `%s'"),
                    name);
    }

    /* add a type field */
    type = "unknown";   /* shouldn't happen */
    for (j = 0, k = sizeof(ftype_map)/sizeof(ftype_map[0]); j < k; j++) {
        if ((sbuf->st_mode & S_IFMT) == ftype_map[j].mask) {
            type = ftype_map[j].type;
            break;
        }
    }

    array_set(array, "type", make_const_string(type, strlen(type), & tmp));

    return 0;
}
```

The third argument to `stat()` was not discussed previously. This
argument is optional. If present, it causes `do_stat()` to use
the `stat()` system call instead of the `lstat()` system
call. This is done by using a function pointer: `statfunc`.
`statfunc` is initialized to point to `lstat()` (instead
of `stat()`) to get the file information, in case the file is a
symbolic link. However, if the third argument is included, `statfunc`
is set to point to `stat()`, instead.

Here is the `do_stat()` function, which starts with
variable declarations and argument checking:

```
/* do_stat --- provide a stat() function for gawk */

static awk_value_t *
do_stat(int nargs, awk_value_t *result, struct awk_ext_func *unused)
{
    awk_value_t file_param, array_param;
    char *name;
    awk_array_t array;
    int ret;
    struct stat sbuf;
    /* default is lstat() */
    int (*statfunc)(const char *path, struct stat *sbuf) = lstat;

    assert(result != NULL);
```

Then comes the actual work. First, the function gets the arguments.
Next, it gets the information for the file. If the called function
(`lstat()` or `stat()`) returns an error, the code sets
`ERRNO` and returns:

```
    /* file is first arg, array to hold results is second */
    if (   ! get_argument(0, AWK_STRING, & file_param)
        || ! get_argument(1, AWK_ARRAY, & array_param)) {
        warning(ext_id, _("stat: bad parameters"));
        return make_number(-1, result);
    }

    if (nargs == 3) {
        statfunc = stat;
    }

    name = file_param.str_value.str;
    array = array_param.array_cookie;

    /* always empty out the array */
    clear_array(array);

    /* stat the file; if error, set ERRNO and return */
    ret = statfunc(name, & sbuf);
```

```
    if (ret < 0) {
        update_ERRNO_int(errno);
        return make_number(ret, result);
    }
```

The tedious work is done by `fill_stat_array()`, shown
earlier. When done, the function returns the result from `fill_stat_array()`:

```
    ret = fill_stat_array(name, array, & sbuf);

    return make_number(ret, result);
}
```

Finally, it’s necessary to provide the “glue” that loads the
new function(s) into `gawk`.

The `filefuncs` extension also provides an `fts()`
function, which we omit here
(see [File-Related Functions](#Extension-Sample-File-Functions)).
For its sake, there is an initialization
function:

```
/* init_filefuncs --- initialization routine */

static awk_bool_t
init_filefuncs(void)
{
    ...
}
```

We are almost done. We need an array of `awk_ext_func_t`
structures for loading each function into `gawk`:

```
static awk_ext_func_t func_table[] = {
    { "chdir", do_chdir, 1, 1, awk_false, NULL },
    { "stat",  do_stat, 3, 2, awk_false, NULL },
    ...
};
```

Each extension must have a routine named `dl_load()` to load
everything that needs to be loaded. It is simplest to use the
`dl_load_func()` macro in `gawkapi.h`:

```
/* define the dl_load() function using the boilerplate macro */

dl_load_func(func_table, filefuncs, "")
```

And that’s it!

---

Previous: [C Code for `chdir()` and `stat()`](#Internal-File-Ops), Up: [Example: Some File Functions](#Extension-Example)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.6.3 Integrating the Extensions [¶](#Integrating-the-Extensions)

Now that the code is written, it must be possible to add it at
runtime to the running `gawk` interpreter. First, the
code must be compiled. Assuming that the functions are in
a file named `filefuncs.c`, and idir is the location
of the `gawkapi.h` header file,
the following steps[117](#FOOT117) create a GNU/Linux shared library:

```
$ gcc -fPIC -shared -DHAVE_CONFIG_H -c -O -g -Iidir filefuncs.c
$ gcc -o filefuncs.so -shared filefuncs.o
```

Once the library exists, it is loaded by using the `@load` directive:

```
# file testff.awk
@load "filefuncs"

BEGIN {
    "pwd" | getline curdir  # save current directory
    close("pwd")

    chdir("/tmp")
    system("pwd")   # test it
    chdir(curdir)   # go back

    print "Info for testff.awk"
    ret = stat("testff.awk", data)
    print "ret =", ret
    for (i in data)
        printf "data[\"%s\"] = %s\n", i, data[i]
    print "testff.awk modified:",
        strftime("%m %d %Y %H:%M:%S", data["mtime"])

    print "\nInfo for JUNK"
    ret = stat("JUNK", data)
    print "ret =", ret
    for (i in data)
        printf "data[\"%s\"] = %s\n", i, data[i]
    print "JUNK modified:", strftime("%m %d %Y %H:%M:%S", data["mtime"])
}
```

The `AWKLIBPATH` environment variable tells
`gawk` where to find extensions (see [How `gawk` Finds Extensions](#Finding-Extensions)).
We set it to the current directory and run the program:

```
$ AWKLIBPATH=$PWD gawk -f testff.awk
-| /tmp
-| Info for testff.awk
-| ret = 0
-| data["blksize"] = 4096
-| data["devbsize"] = 512
-| data["mtime"] = 1412004710
-| data["mode"] = 33204
-| data["type"] = file
-| data["dev"] = 2053
-| data["gid"] = 1000
-| data["ino"] = 10358899
-| data["ctime"] = 1412004710
-| data["blocks"] = 8
-| data["nlink"] = 1
-| data["name"] = testff.awk
-| data["atime"] = 1412004716
-| data["pmode"] = -rw-rw-r--
-| data["size"] = 666
-| data["uid"] = 1000
-| testff.awk modified: 09 29 2014 18:31:50
-|
-| Info for JUNK
-| ret = -1
-| JUNK modified: 01 01 1970 02:00:00
```

---

Next: [The `gawkextlib` Project](#gawkextlib), Previous: [Example: Some File Functions](#Extension-Example), Up: [Writing Extensions for `gawk`](#Dynamic-Extensions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.7 The Sample Extensions in the `gawk` Distribution [¶](#The-Sample-Extensions-in-the-gawk-Distribution)

This section provides a brief overview of the sample extensions
that come in the `gawk` distribution. Some of them are intended
for production use (e.g., the `filefuncs`, `readdir`, and
`inplace` extensions). Others mainly provide example code that
shows how to use the extension API.

* [File-Related Functions](#Extension-Sample-File-Functions)
* [Interface to `fnmatch()`](#Extension-Sample-Fnmatch)
* [Interface to `fork()`, `wait()`, and `waitpid()`](#Extension-Sample-Fork)
* [Enabling In-Place File Editing](#Extension-Sample-Inplace)
* [Character and Numeric values: `ord()` and `chr()`](#Extension-Sample-Ord)
* [Reading Directories](#Extension-Sample-Readdir)
* [Reversing Output](#Extension-Sample-Revout)
* [Two-Way I/O Example](#Extension-Sample-Rev2way)
* [Dumping and Restoring an Array](#Extension-Sample-Read-write-array)
* [Reading an Entire File](#Extension-Sample-Readfile)
* [Extension Time Functions](#Extension-Sample-Time)
* [API Tests](#Extension-Sample-API-Tests)

---

Next: [Interface to `fnmatch()`](#Extension-Sample-Fnmatch), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.1 File-Related Functions [¶](#File_002dRelated-Functions)

The `filefuncs` extension provides three different functions, as follows.
The usage is:

`@load "filefuncs"`
:   This is how you load the extension.

`result = chdir("/some/directory")` [¶](#index-chdir_0028_0029-extension-function)
:   The `chdir()` function is a direct hook to the `chdir()`
    system call to change the current directory. It returns zero
    upon success or a value less than zero upon error.
    In the latter case, it updates `ERRNO`.

`result = stat("/some/path", statdata` [`, follow`]`)` [¶](#index-stat_0028_0029-extension-function)
:   The `stat()` function provides a hook into the
    `stat()` system call.
    It returns zero upon success or a value less than zero upon error.
    In the latter case, it updates `ERRNO`.

    By default, it uses the `lstat()` system call. However, if passed
    a third argument, it uses `stat()` instead.

    In all cases, it clears the `statdata` array.
    When the call is successful, `stat()` fills the `statdata`
    array with information retrieved from the filesystem, as follows:

    | Subscript | Field in `struct stat` | File type |
    | --- | --- | --- |
    | `"name"` | The file name | All |
    | `"dev"` | `st_dev` | All |
    | `"ino"` | `st_ino` | All |
    | `"mode"` | `st_mode` | All |
    | `"nlink"` | `st_nlink` | All |
    | `"uid"` | `st_uid` | All |
    | `"gid"` | `st_gid` | All |
    | `"size"` | `st_size` | All |
    | `"atime"` | `st_atime` | All |
    | `"mtime"` | `st_mtime` | All |
    | `"ctime"` | `st_ctime` | All |
    | `"rdev"` | `st_rdev` | Device files |
    | `"major"` | `st_major` | Device files |
    | `"minor"` | `st_minor` | Device files |
    | `"blksize"` | `st_blksize` | All |
    | `"pmode"` | A human-readable version of the mode value, like that printed by `ls` (for example, `"-rwxr-xr-x"`) | All |
    | `"linkval"` | The value of the symbolic link | Symbolic links |
    | `"type"` | The type of the file as a string—one of `"file"`, `"blockdev"`, `"chardev"`, `"directory"`, `"socket"`, `"fifo"`, `"symlink"`, `"door"`, or `"unknown"` (not all systems support all file types) | All |

`flags = or(FTS_PHYSICAL, ...)` [¶](#index-fts_0028_0029-extension-function)

`result = fts(pathlist, flags, filedata)`
:   Walk the file trees provided in `pathlist` and fill in the
    `filedata` array, as described next. `flags` is the bitwise
    OR of several predefined values, also described in a moment.
    Return zero if there were no errors, otherwise return −1.

The `fts()` function provides a hook to the C library `fts()`
routines for traversing file hierarchies. Instead of returning data
about one file at a time in a stream, it fills in a multidimensional
array with data about each file and directory encountered in the requested
hierarchies.

The arguments are as follows:

`pathlist`
:   An array of file names. The element values are used; the index values are ignored.

`flags`
:   This should be the bitwise OR of one or more of the following
    predefined constant flag values. At least one of
    `FTS_LOGICAL` or `FTS_PHYSICAL` must be provided; otherwise
    `fts()` returns an error value and sets `ERRNO`.
    The flags are:

    `FTS_LOGICAL`
    :   Do a “logical” file traversal, where the information returned for
        a symbolic link refers to the linked-to file, and not to the symbolic
        link itself. This flag is mutually exclusive with `FTS_PHYSICAL`.

    `FTS_PHYSICAL`
    :   Do a “physical” file traversal, where the information returned for a
        symbolic link refers to the symbolic link itself. This flag is mutually
        exclusive with `FTS_LOGICAL`.

    `FTS_NOCHDIR`
    :   As a performance optimization, the C library `fts()` routines
        change directory as they traverse a file hierarchy. This flag disables
        that optimization.

    `FTS_COMFOLLOW`
    :   Immediately follow a symbolic link named in `pathlist`,
        whether or not `FTS_LOGICAL` is set.

    `FTS_SEEDOT`
    :   By default, the C library `fts()` routines do not return entries for
        `.` (dot) and `..` (dot-dot). This option causes entries for
        dot-dot to also be included. (The extension always includes an entry
        for dot; more on this in a moment.)

    `FTS_XDEV`
    :   During a traversal, do not cross onto a different mounted filesystem.

`filedata`
:   The `filedata` array holds the results.
    `fts()` first clears it. Then it creates
    an element in `filedata` for every element in `pathlist`.
    The index is the name of the directory or file given in `pathlist`.
    The element for this index is itself an array. There are two cases:

    *The path is a file*
    :   In this case, the array contains two or three elements:

        `"path"`
        :   The full path to this file, starting from the “root” that was given
            in the `pathlist` array.

        `"stat"`
        :   This element is itself an array, containing the same information as provided
            by the `stat()` function described earlier for its
            `statdata` argument. The element may not be present if
            the `stat()` system call for the file failed.

        `"error"`
        :   If some kind of error was encountered, the array will also
            contain an element named `"error"`, which is a string describing the error.

    *The path is a directory*
    :   In this case, the array contains one element for each entry in the
        directory. If an entry is a file, that element is the same as for files, just
        described. If the entry is a directory, that element is (recursively)
        an array describing the subdirectory. If `FTS_SEEDOT` was provided
        in the flags, then there will also be an element named `".."`. This
        element will be an array containing the data as provided by `stat()`.

        In addition, there will be an element whose index is `"."`.
        This element is an array containing the same two or three elements as
        for a file: `"path"`, `"stat"`, and `"error"`.

The `fts()` function returns zero if there were no errors.
Otherwise, it returns −1.

> **NOTE:** The `fts()` extension does not exactly mimic the
> interface of the C library `fts()` routines, choosing instead to
> provide an interface that is based on associative arrays, which is
> more comfortable to use from an `awk` program. This includes the
> lack of a comparison function, because `gawk` already provides
> powerful array sorting facilities. Although an `fts_read()`-like
> interface could have been provided, this felt less natural than simply
> creating a multidimensional array to represent the file hierarchy and
> its information.

See `test/fts.awk` in the `gawk` distribution for an example
use of the `fts()` extension function.

---

Next: [Interface to `fork()`, `wait()`, and `waitpid()`](#Extension-Sample-Fork), Previous: [File-Related Functions](#Extension-Sample-File-Functions), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.2 Interface to `fnmatch()` [¶](#Interface-to-fnmatch_0028_0029)

This extension provides an interface to the C library
`fnmatch()` function. The usage is:

`@load "fnmatch"`
:   This is how you load the extension.

`result = fnmatch(pattern, string, flags)` [¶](#index-fnmatch_0028_0029-extension-function)
:   The return value is zero on success, `FNM_NOMATCH`
    if the string did not match the pattern, or
    a different nonzero value if an error occurred.

In addition to the `fnmatch()` function, the `fnmatch` extension
adds one constant (`FNM_NOMATCH`), and an array of flag values
named `FNM`.

The arguments to `fnmatch()` are:

`pattern`
:   The file name wildcard to match

`string`
:   The file name string

`flag`
:   Either zero, or the bitwise OR of one or more of the
    flags in the `FNM` array

The flags are as follows:

| Array element | Corresponding flag defined by `fnmatch()` |
| --- | --- |
| `FNM["CASEFOLD"]` | `FNM_CASEFOLD` |
| `FNM["FILE_NAME"]` | `FNM_FILE_NAME` |
| `FNM["LEADING_DIR"]` | `FNM_LEADING_DIR` |
| `FNM["NOESCAPE"]` | `FNM_NOESCAPE` |
| `FNM["PATHNAME"]` | `FNM_PATHNAME` |
| `FNM["PERIOD"]` | `FNM_PERIOD` |

Here is an example:

```
@load "fnmatch"
...
flags = or(FNM["PERIOD"], FNM["NOESCAPE"])
if (fnmatch("*.a", "foo.c", flags) == FNM_NOMATCH)
    print "no match"
```

---

Next: [Enabling In-Place File Editing](#Extension-Sample-Inplace), Previous: [Interface to `fnmatch()`](#Extension-Sample-Fnmatch), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.3 Interface to `fork()`, `wait()`, and `waitpid()` [¶](#Interface-to-fork_0028_0029_002c-wait_0028_0029_002c-and-waitpid_0028_0029)

The `fork` extension adds three functions, as follows:

`@load "fork"`
:   This is how you load the extension.

`pid = fork()` [¶](#index-fork_0028_0029-extension-function)
:   This function creates a new process. The return value is zero in the
    child and the process ID number of the child in the parent, or −1
    upon error. In the latter case, `ERRNO` indicates the problem.
    In the child, `PROCINFO["pid"]` and `PROCINFO["ppid"]` are
    updated to reflect the correct values.

`ret = waitpid(pid)` [¶](#index-waitpid_0028_0029-extension-function)
:   This function takes a numeric argument, which is the process ID to
    wait for. The return value is that of the
    `waitpid()` system call.

`ret = wait()` [¶](#index-wait_0028_0029-extension-function)
:   This function waits for the first child to die.
    The return value is that of the
    `wait()` system call.

There is no corresponding `exec()` function.

Here is an example:

```
@load "fork"
...
if ((pid = fork()) == 0)
    print "hello from the child"
else
    print "hello from the parent"
```

---

Next: [Character and Numeric values: `ord()` and `chr()`](#Extension-Sample-Ord), Previous: [Interface to `fork()`, `wait()`, and `waitpid()`](#Extension-Sample-Fork), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.4 Enabling In-Place File Editing [¶](#Enabling-In_002dPlace-File-Editing)

The `inplace` extension emulates GNU `sed`’s `-i` option,
which performs “in-place” editing of each input file. Like GNU `sed`,
the `inplace` extension replaces links (both hard and symbolic) with new
files. If you wish to affect the target, you must dereference it first, for
example using `realpath` from GNU Coreutils.
It uses the bundled `inplace.awk` include file to invoke the extension
properly. This extension makes use of the namespace facility to place
all the variables and functions in the `inplace` namespace
(see [Namespaces in `gawk`](#Namespaces)):

```
# inplace --- load and invoke the inplace extension.

@load "inplace"

# Please set inplace::suffix to make a backup copy.  For example, you may
# want to set inplace::suffix to .bak on the command line or in a BEGIN rule.

# Before there were namespaces in gawk, this extension used
# INPLACE_SUFFIX as the variable for making backup copies. We allow this
# too, so that any code that used the previous version continues to work.

# By default, each filename on the command line will be edited inplace.
# But you can selectively disable this by adding an inplace::enable=0 argument
# prior to files that you do not want to process this way.  You can then
# reenable it later on the commandline by putting inplace::enable=1 before files
# that you wish to be subject to inplace editing.

# N.B. We call inplace::end() in the BEGINFILE and END rules so that any
# actions in an ENDFILE rule will be redirected as expected.

@namespace "inplace"
```

```

```

```
BEGIN {
    enable = 1         # enabled by default
}
```

```

```

```
BEGINFILE {
    sfx = (suffix ? suffix : awk::INPLACE_SUFFIX)
    if (filename != "")
        end(filename, sfx)
    if (enable)
        begin(filename = FILENAME, sfx)
    else
        filename = ""
}
```

```

```

```
END {
    if (filename != "")
        end(filename, (suffix ? suffix : awk::INPLACE_SUFFIX))
}
```

For each regular file that is processed, the extension redirects
standard output to a temporary file configured to have the same owner
and permissions as the original. After the file has been processed,
the extension restores standard output to its original destination.
(Due to this implementation, it helps to redirect `gawk`’s
standard output to `/dev/null`, instead of leaving it set to
your terminal, so that output will be block-buffered instead of
line-buffered.)

If `inplace::suffix` is not an empty string, the original file is
linked to a backup file name created by appending that suffix. Finally,
the temporary file is renamed to the original file name.

Note that the use of this feature can be controlled by placing
‘`inplace::enable=0`’ on the command-line prior to listing files that
should not be processed this way. You can reenable inplace editing by adding
an ‘`inplace::enable=1`’ argument prior to files that should be subject
to inplace editing.

The `inplace::filename` variable serves to keep track of the
current file name so as to not invoke `inplace::end()` before
processing the first file.

If any error occurs, the extension issues a fatal error to terminate
processing immediately without damaging the original file.

Here are some simple examples:

```
$ gawk -i inplace '{ gsub(/foo/, "bar") }; { print }' file1 file2 file3
```

To keep a backup copy of the original files, try this:

```
$ gawk -i inplace -v inplace::suffix=.bak '{ gsub(/foo/, "bar") }
> { print }' file1 file2 file3
```

Please note that, while the extension does attempt to preserve ownership
and permissions, it makes no attempt to copy the ACLs from the original
file.

If the program dies prematurely, as might happen if an unhandled signal
is received, a temporary file may be left behind.

---

Next: [Reading Directories](#Extension-Sample-Readdir), Previous: [Enabling In-Place File Editing](#Extension-Sample-Inplace), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.5 Character and Numeric values: `ord()` and `chr()` [¶](#Character-and-Numeric-values_003a-ord_0028_0029-and-chr_0028_0029)

The `ordchr` extension adds two functions, named
`ord()` and `chr()`, as follows:

`@load "ordchr"`
:   This is how you load the extension.

`number = ord(string)` [¶](#index-ord_0028_0029-extension-function)
:   Return the numeric value of the first character in `string`.

`char = chr(number)` [¶](#index-chr_0028_0029-extension-function)
:   Return a string whose first character is that represented by `number`.

These functions are inspired by the Pascal language functions
of the same name. Here is an example:

```
@load "ordchr"
...
printf("The numeric value of 'A' is %d\n", ord("A"))
printf("The string value of 65 is %s\n", chr(65))
```

As of release 5.4 of `gawk`, the extension can handle multibyte /
wide character sets. If given an invalid multibyte string, in a UTF-8
locale, `ord()` returns the Unicode “invalid character” value,
`0xFFFD`. Otherwise, it returns the numeric value of the first byte
in the string. In a single-byte locale, `ord()` always returns the
numeric value of the first byte in the string.

---

Next: [Reversing Output](#Extension-Sample-Revout), Previous: [Character and Numeric values: `ord()` and `chr()`](#Extension-Sample-Ord), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.6 Reading Directories [¶](#Reading-Directories)

The `readdir` extension adds an input parser for directories.
The usage is as follows:

```
@load "readdir"
```

When this extension is in use, instead of skipping directories named
on the command line (or with `getline`),
they are read, with each entry returned as a record.

The record consists of three fields separated by forward slash characters.
The first two are the inode number and the file name, and the third field
is a single letter indicating the type of the file. The letters and their
corresponding file types are shown in [Table 17.4](#table_002dreaddir_002dfile_002dtypes).

| Letter | File type |
| --- | --- |
| `b` | Block device |
| `c` | Character device |
| `d` | Directory |
| `f` | Regular file |
| `l` | Symbolic link |
| `p` | Named pipe (FIFO) |
| `s` | Socket |

**Table 17.4:** File types returned by the `readdir` extension

On systems where the directory entry contains the file type, the third
field is filled in from that information.
On systems without the file type information, the extension falls back
to calling the `stat()` system call
in order to provide the information.
Thus the third field should never be ‘`u`’ (for “unknown”).

Normally, when reading directories, you should set `FS`
equal to `"/"`. However, you may instead chose to create
`PROCINFO["readdir_override"]` (with any value). If this element
exists when the directory is opened, then the extension automatically
sets the fields in each record for you.

By default, if a directory cannot be opened (due to permission problems,
for example), `gawk` will exit. As with regular files, this
situation can be handled using a `BEGINFILE` rule that checks
`ERRNO` and prints an error or otherwise handles the problem.

Here is an example:

```
@load "readdir"
...
BEGIN { FS = "/" }
{ print "file name is", $2 }
```

---

Next: [Two-Way I/O Example](#Extension-Sample-Rev2way), Previous: [Reading Directories](#Extension-Sample-Readdir), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.7 Reversing Output [¶](#Reversing-Output)

The `revoutput` extension adds a simple output wrapper that reverses
the characters in each output line. Its main purpose is to show how to
write an output wrapper, although it may be mildly amusing for the unwary.
Here is an example:

```
@load "revoutput"

BEGIN {
    REVOUT = 1
    print "don't panic" > "/dev/stdout"
}
```

The output from this program is ‘`cinap t'nod`’.

---

Next: [Dumping and Restoring an Array](#Extension-Sample-Read-write-array), Previous: [Reversing Output](#Extension-Sample-Revout), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.8 Two-Way I/O Example [¶](#Two_002dWay-I_002fO-Example)

The `revtwoway` extension adds a simple two-way processor that
reverses the characters in each line sent to it for reading back by
the `awk` program. Its main purpose is to show how to write
a two-way processor, although it may also be mildly amusing.
The following example shows how to use it:

```
@load "revtwoway"

BEGIN {
    cmd = "/magic/mirror"
    print "don't panic" |& cmd
    cmd |& getline result
    print result
    close(cmd)
}
```

The output from this program
also is:
‘`cinap t'nod`’.

---

Next: [Reading an Entire File](#Extension-Sample-Readfile), Previous: [Two-Way I/O Example](#Extension-Sample-Rev2way), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.9 Dumping and Restoring an Array [¶](#Dumping-and-Restoring-an-Array)

The `rwarray` extension adds four functions,
named `writea()`, `reada()`,
`writeall()` and `readall()`, as follows:

`@load "rwarray"`
:   This is how you load the extension.

`ret = writea(file, array)` [¶](#index-writea_0028_0029-extension-function)
:   This function takes a string argument, which is the name of the file
    to which to dump the array, and the array itself as the second argument.
    `writea()` understands arrays of arrays. It returns one on
    success, or zero upon failure.

`ret = reada(file, array)` [¶](#index-reada_0028_0029-extension-function)
:   `reada()` is the inverse of `writea()`;
    it reads the file named as its first argument, filling in
    the array named as the second argument. It clears the array first.
    Here too, the return value is one on success, or zero upon failure.

`ret = writeall(file)` [¶](#index-writeall_0028_0029-extension-function)
:   This function takes a string argument, which is the name of the file
    to which to dump the state of all variables.
    Calling this function
    is completely equivalent to calling
    `writea(file, SYMTAB)`.
    It returns one on success, or zero upon failure

`ret = readall(file)` [¶](#index-readall_0028_0029-extension-function)
:   This function takes a string argument, which is the name of the
    file from which to read the contents of various global variables.
    For each variable in the file, the data is loaded unless the variable
    has already been assigned a value or used as an array.
    In that case, the data for that variable
    in the file is ignored.
    It returns one on success, or zero upon failure.

The array created by `reada()` is identical to that written by
`writea()` in the sense that the contents are the same. However,
due to implementation issues, the array traversal order of the re-created
array is likely to be different from that of the original array. As array
traversal order in `awk` is by default undefined, this is (technically)
not a problem. If you need to guarantee a particular traversal
order, use the array sorting features in `gawk` to do so
(see [Controlling Array Traversal and Array Sorting](#Array-Sorting)).

The file contains binary data. All integral values are written in network
byte order. However, double-precision floating-point values are written
as native binary data. Thus, arrays containing only string data can
theoretically be dumped on systems with one byte order and restored on
systems with a different one, but this has not been tried.

Note that the `writeall()` and `readall()` functions provide
a mechanism for maintaining persistent state across repeated invocations of a
program. If, for example, a program calculates some statistics based on the
data in a series of files, it could save state using `writeall()` after
processing N files, and then reload the state using `readall()` when
the N+1st file arrives to update the results.

Here is an example:

```
@load "rwarray"
...
ret = writea("arraydump.bin", array)
...
ret = reada("arraydump.bin", array)
...
ret = writeall("globalstate.bin")
...
ret = readall("globalstate.bin")
```

---

Next: [Extension Time Functions](#Extension-Sample-Time), Previous: [Dumping and Restoring an Array](#Extension-Sample-Read-write-array), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.10 Reading an Entire File [¶](#Reading-an-Entire-File)

The `readfile` extension adds a single function
named `readfile()`, and an input parser:

`@load "readfile"`
:   This is how you load the extension.

`result = readfile("/some/path")` [¶](#index-readfile_0028_0029-extension-function)
:   The argument is the name of the file to read. The return value is a
    string containing the entire contents of the requested file. Upon error,
    the function returns the empty string and sets `ERRNO`.

`BEGIN { PROCINFO["readfile"] = 1 }`
:   In addition, the extension adds an input parser that is activated if
    `PROCINFO["readfile"]` exists.
    When activated, each input file is returned in its entirety as `$0`.
    `RT` is set to the null string.

Here is an example:

```
@load "readfile"
...
contents = readfile("/path/to/file");
if (contents == "" && ERRNO != "") {
    print("problem reading file", ERRNO) > "/dev/stderr"
    ...
}
```

---

Next: [API Tests](#Extension-Sample-API-Tests), Previous: [Reading an Entire File](#Extension-Sample-Readfile), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.11 Extension Time Functions [¶](#Extension-Time-Functions)

The `time` extension adds three functions, named `gettimeofday()`
`sleep()`, and `strptime()`, as follows:

`@load "time"`
:   This is how you load the extension.

`the_time = gettimeofday()` [¶](#index-gettimeofday_0028_0029-extension-function)
:   Return the time in seconds that has elapsed since 1970-01-01 UTC as a
    floating-point value. If the time is unavailable on this platform, return
    −1 and set `ERRNO`. The returned time should have sub-second
    precision, but the actual precision may vary based on the platform.
    If the standard C `gettimeofday()` system call is available on this
    platform, then it simply returns the value. Otherwise, if on MS-Windows,
    it tries to use `GetSystemTimeAsFileTime()`.

`result = sleep(seconds)` [¶](#index-sleep_0028_0029-extension-function)
:   Attempt to sleep for seconds seconds. If seconds is negative,
    or the attempt to sleep fails, return −1 and set `ERRNO`.
    Otherwise, return zero after sleeping for the indicated amount of time.
    Note that seconds may be a floating-point (nonintegral) value.
    Implementation details: depending on platform availability, this function
    tries to use `nanosleep()` or `select()` to implement the delay.

`timeval = strptime(string, format)` [¶](#index-strptime_0028_0029-extension-function)
:   This function takes two arguments, a string representing a date and
    time, and a format string describing the data in the string. It
    calls the C library `strptime()` function with the given values.
    If the parsing succeeds, the results are passed to the C library
    `mktime()` function, and its result is returned, expressing
    the time in seconds since the epoch in the current local timezone,
    regardless of any timezone specified in the string arguments.
    Otherwise it returns −1 upon error.

---

Previous: [Extension Time Functions](#Extension-Sample-Time), Up: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 17.7.12 API Tests [¶](#API-Tests)

The `testext` extension exercises parts of the extension API that
are not tested by the other samples. The `extension/testext.c`
file contains both the C code for the extension and `awk`
test code inside C comments that run the tests. The testing framework
extracts the `awk` code and runs the tests. See the source file
for more information.

---

Next: [Summary](#Extension-summary), Previous: [The Sample Extensions in the `gawk` Distribution](#Extension-Samples), Up: [Writing Extensions for `gawk`](#Dynamic-Extensions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.8 The `gawkextlib` Project [¶](#The-gawkextlib-Project)

The [`gawkextlib`](https://sourceforge.net/projects/gawkextlib/)
project provides a number of `gawk` extensions, including one for
processing XML files. This is the evolution of the original `xgawk`
(XML `gawk`) project.

There are a number of extensions. The list of available extensions as well
as their on-line full documentation can be seen in the
[`gawkextlib` web pages](https://gawkextlib.sourceforge.net/).

Some of the more interesting extensions are:

* `abort` extension. It allows you to exit immediately from your
  `awk` program without running the `END` rules.
* `json` extension.
  This serializes a multidimensional array into a JSON string, and
  can deserialize a JSON string into a `gawk` array.
  This extension is interesting since it is written in C++ instead of C.
* MPFR library extension.
  This provides access to a number of MPFR functions that `gawk`’s
  native MPFR support does not.
* Select extension. It provides functionality based on the
  `select()` system call.
* XML parser extension, using the [Expat](https://expat.sourceforge.net)
  XML parsing library

You can check out the code for the `gawkextlib` project
using the [Git](https://git-scm.com) distributed source
code control system. The command is as follows:

```
git clone git://git.code.sf.net/p/gawkextlib/code gawkextlib-code
```

You will need to have the [RapidJson](https://www.rapidjson.org)
JSON parser library installed in order to build and use the `json` extension.

You will need to have the [Expat](https://expat.sourceforge.net)
XML parser library installed in order to build and use the XML extension.

In addition, you must have the GNU Autotools installed
([Autoconf](https://www.gnu.org/software/autoconf),
[Automake](https://www.gnu.org/software/automake),
[Libtool](https://www.gnu.org/software/libtool),
and
[GNU `gettext`](https://www.gnu.org/software/gettext)).

The simple recipe for building and testing `gawkextlib` is as follows.
First, build and install `gawk`:

```
cd .../path/to/gawk/code
./configure --prefix=/tmp/newgawk     Install in /tmp/newgawk for now
make && make check                    Build and check that all is OK
make install                          Install gawk
```

Next, go to <https://sourceforge.net/projects/gawkextlib/files> to
download `gawkextlib` and any extensions that you would like to build.
The `README` file at that site explains how to build the code. If you
installed `gawk` in a non-standard location, you will need to
specify ‘`./configure --with-gawk=/path/to/gawk`’ to find it.
You may need to use the `sudo` utility
to install both `gawk` and `gawkextlib`, depending upon
how your system works.

If you write an extension that you wish to share with other
`gawk` users, consider doing so through the
`gawkextlib` project.
See the project’s website for more information.

---

Next: [Exercises](#Extension-Exercises), Previous: [The `gawkextlib` Project](#gawkextlib), Up: [Writing Extensions for `gawk`](#Dynamic-Extensions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
