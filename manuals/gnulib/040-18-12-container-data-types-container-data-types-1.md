### 18.12 Container data types [¶](#Container-data-types-1)

Gnulib provides several generic container data types. They can be used
to organize collections of application-defined objects.

* [Ordinary container data types](#Ordinary-containers)
* [Specialized container data types](#Specialized-containers)

---

Next: [Specialized container data types](#Specialized-containers), Up: [Container data types](#Container-data-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.12.1 Ordinary container data types [¶](#Ordinary-container-data-types)

| Data type | Details | Module | Main include file | Include file for operations with out-of-memory checking |
| --- | --- | --- | --- | --- |
| Sequential list | Can contain any number of objects in any given order. Duplicates are allowed, but can optionally be forbidden. | `list` | `"gl_list.h"` | `"gl_xlist.h"` |
| Set | Can contain any number of objects; the order does not matter. Duplicates (in the sense of the comparator) are forbidden. | `set` | `"gl_set.h"` | `"gl_xset.h"` |
| Ordered set | Can contain any number of objects in the order of a given comparator function. Duplicates (in the sense of the comparator) are forbidden. | `oset` | `"gl_oset.h"` | `"gl_xoset.h"` |
| Map | Can contain any number of (key, value) pairs, where keys and values are objects; there are no (key, value1) and (key, value2) pairs with the same key (in the sense of a given comparator function). | `map` | `"gl_map.h"` | `"gl_xmap.h"` |
| Ordered map | Can contain any number of (key, value) pairs, where keys and values are objects; the (key, value) pairs are ordered by the key, in the order of a given comparator function; there are no (key, value1) and (key, value2) pairs with the same key (in the sense of the comparator function). | `omap` | `"gl_omap.h"` | `"gl_xomap.h"` |

Operations without out-of-memory checking (suitable for use in libraries) are
declared in the “main include file”. Whereas operations with out-of-memory
checking (suitable only in programs) are declared in the “include file for
operations with out-of-memory checking”.

For each of the data types, several implementations are available, with
different performance profiles with respect to the available operations.
This enables you to start with the simplest implementation (ARRAY) initially,
and switch to a more suitable implementation after profiling your application.
The implementation of each container instance is specified in a single place
only: in the invocation of the function `gl_*_create_empty` that creates
the instance.

* [Sequential lists](#Sequential-lists)
* [Sets](#Sets)
* [Ordered sets](#Ordered-sets)
* [Maps](#Maps)
* [Ordered maps](#Ordered-maps)
* [C++ classes for container data types](#C_002b_002b-classes-for-container-data-types)

#### 18.12.1.1 Sequential lists [¶](#Sequential-lists)

The implementations and the guaranteed average performance for the operations
for the “sequential list” data type are:

| Operation | ARRAY | CARRAY | LINKED | TREE | LINKEDHASH with duplicates | LINKEDHASH without duplicates | TREEHASH with duplicates | TREEHASH without duplicates |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `gl_list_size` | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* |
| `gl_list_node_value` | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* |
| `gl_list_node_set_value` | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O(1)* | *O((log n)2)* | *O(1)* |
| `gl_list_next_node` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O(log n)* | *O(log n)* |
| `gl_list_previous_node` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O(log n)* | *O(log n)* |
| `gl_list_first_node` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O(log n)* | *O(log n)* |
| `gl_list_last_node` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O(log n)* | *O(log n)* |
| `gl_list_get_at` | *O(1)* | *O(1)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O(log n)* | *O(log n)* |
| `gl_list_get_first` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O(log n)* | *O(log n)* |
| `gl_list_get_last` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O(log n)* | *O(log n)* |
| `gl_list_set_at` | *O(1)* | *O(1)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O((log n)2)* | *O(log n)* |
| `gl_list_set_first` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(n)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_set_last` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(n)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_search` | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(1)* | *O(log n)* | *O(1)* |
| `gl_list_search_from` | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_search_from_to` | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_indexof` | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(log n)* | *O(log n)* |
| `gl_list_indexof_from` | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O((log n)2)* | *O(log n)* |
| `gl_list_indexof_from_to` | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O((log n)2)* | *O(log n)* |
| `gl_list_add_first` | *O(n)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_add_last` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_add_before` | *O(n)* | *O(n)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_add_after` | *O(n)* | *O(n)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_add_at` | *O(n)* | *O(n)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O((log n)2)* | *O(log n)* |
| `gl_list_remove_node` | *O(n)* | *O(n)* | *O(1)* | *O(log n)* | *O(n)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_remove_at` | *O(n)* | *O(n)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O((log n)2)* | *O(log n)* |
| `gl_list_remove_first` | *O(n)* | *O(1)* | *O(1)* | *O(log n)* | *O(n)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_remove_last` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(n)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_remove` | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(n)* | *O(1)* | *O((log n)2)* | *O(log n)* |
| `gl_list_iterator` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O(log n)* | *O(log n)* |
| `gl_list_iterator_from_to` | *O(1)* | *O(1)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O(log n)* | *O(log n)* |
| `gl_list_iterator_next` | *O(1)* | *O(1)* | *O(1)* | *O(log n)* | *O(1)* | *O(1)* | *O(log n)* | *O(log n)* |
| `gl_sortedlist_search` | *O(log n)* | *O(log n)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O(log n)* | *O(log n)* |
| `gl_sortedlist_search_from` | *O(log n)* | *O(log n)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O(log n)* | *O(log n)* |
| `gl_sortedlist_indexof` | *O(log n)* | *O(log n)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O(log n)* | *O(log n)* |
| `gl_sortedlist_indexof_from` | *O(log n)* | *O(log n)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O(log n)* | *O(log n)* |
| `gl_sortedlist_add` | *O(n)* | *O(n)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O((log n)2)* | *O(log n)* |
| `gl_sortedlist_remove` | *O(n)* | *O(n)* | *O(n)* | *O(log n)* | *O(n)* | *O(n)* | *O((log n)2)* | *O(log n)* |

The Gnulib modules for sequential lists are:

| Implementation | Modules |
| --- | --- |
| Abstract | `list`, `xlist` |
| ARRAY | `array-list` |
| CARRAY | `carray-list` |
| LINKED | `linked-list` |
| TREE | `avltree-list`, `rbtree-list` |
| LINKEDHASH | `linkedhash-list` |
| TREEHASH | `avltreehash-list`, `rbtreehash-list` |
| Portion of a list | `sublist`, `xsublist` |

#### 18.12.1.2 Sets [¶](#Sets)

The implementations and the guaranteed average performance for the operations
for the “set” data type are:

| Operation | ARRAY | LINKEDHASH, HASH |
| --- | --- | --- |
| `gl_set_size` | *O(1)* | *O(1)* |
| `gl_set_add` | *O(n)* | *O(1)* |
| `gl_set_remove` | *O(n)* | *O(1)* |
| `gl_set_search` | *O(n)* | *O(1)* |
| `gl_set_iterator` | *O(1)* | *O(1)* |
| `gl_set_iterator_next` | *O(1)* | *O(1)* |

The Gnulib modules for sets are:

| Implementation | Modules |
| --- | --- |
| Abstract | `set`, `xset` |
| ARRAY | `array-set` |
| LINKEDHASH | `linkedhash-set` |
| HASH | `hash-set` |

#### 18.12.1.3 Ordered sets [¶](#Ordered-sets)

The implementations and the guaranteed average performance for the operations
for the “ordered set” data type are:

| Operation | ARRAY | TREE |
| --- | --- | --- |
| `gl_oset_size` | *O(1)* | *O(1)* |
| `gl_oset_add` | *O(n)* | *O(log n)* |
| `gl_oset_remove` | *O(n)* | *O(log n)* |
| `gl_oset_search` | *O(log n)* | *O(log n)* |
| `gl_oset_search_atleast` | *O(log n)* | *O(log n)* |
| `gl_oset_iterator` | *O(1)* | *O(log n)* |
| `gl_oset_iterator_next` | *O(1)* | *O(log n)* |

The Gnulib modules for ordered sets are:

| Implementation | Modules |
| --- | --- |
| Abstract | `oset`, `xoset` |
| ARRAY | `array-oset` |
| TREE | `avltree-oset`, `rbtree-oset` |

#### 18.12.1.4 Maps [¶](#Maps)

The implementations and the guaranteed average performance for the operations
for the “map” data type are:

| Operation | ARRAY | LINKEDHASH, HASH |
| --- | --- | --- |
| `gl_map_size` | *O(1)* | *O(1)* |
| `gl_map_get` | *O(n)* | *O(1)* |
| `gl_map_put` | *O(n)* | *O(1)* |
| `gl_map_remove` | *O(n)* | *O(1)* |
| `gl_map_search` | *O(n)* | *O(1)* |
| `gl_map_iterator` | *O(1)* | *O(1)* |
| `gl_map_iterator_next` | *O(1)* | *O(1)* |

The Gnulib modules for maps are:

| Implementation | Modules |
| --- | --- |
| Abstract | `map`, `xmap` |
| ARRAY | `array-map` |
| LINKEDHASH | `linkedhash-map` |
| HASH | `hash-map` |

#### 18.12.1.5 Ordered maps [¶](#Ordered-maps)

The implementations and the guaranteed average performance for the operations
for the “ordered map” data type are:

| Operation | ARRAY | TREE |
| --- | --- | --- |
| `gl_omap_size` | *O(1)* | *O(1)* |
| `gl_omap_get` | *O(log n)* | *O(log n)* |
| `gl_omap_put` | *O(n)* | *O(log n)* |
| `gl_omap_remove` | *O(n)* | *O(log n)* |
| `gl_omap_search` | *O(log n)* | *O(log n)* |
| `gl_omap_search_atleast` | *O(log n)* | *O(log n)* |
| `gl_omap_iterator` | *O(1)* | *O(log n)* |
| `gl_omap_iterator_next` | *O(1)* | *O(log n)* |

The Gnulib modules for ordered maps are:

| Implementation | Modules |
| --- | --- |
| Abstract | `omap`, `xomap` |
| ARRAY | `array-omap` |
| TREE | `avltree-omap`, `rbtree-omap` |

#### 18.12.1.6 C++ classes for container data types [¶](#C_002b_002b-classes-for-container-data-types)

For C++, Gnulib provides a C++ template class for each of these container data types.

| Data type | C++ class | Module | Include file |
| --- | --- | --- | --- |
| Sequential list | `gl_List` | `list-c++` | `"gl_list.hh"` |
| Set | `gl_Set` | `set-c++` | `"gl_set.hh"` |
| Ordered set | `gl_OSet` | `oset-c++` | `"gl_oset.hh"` |
| Map | `gl_Map` | `map-c++` | `"gl_map.hh"` |
| Ordered map | `gl_OMap` | `omap-c++` | `"gl_omap.hh"` |

---

Previous: [Ordinary container data types](#Ordinary-containers), Up: [Container data types](#Container-data-types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.12.2 Specialized container data types [¶](#Specialized-container-data-types)

The `hamt` module implements the hash array mapped trie (HAMT) data
structure. This is a data structure that contains (key, value) pairs.
Lookup of a (key, value) pair given the key is on average an *O(1)*
operation, assuming a good hash function for the keys is employed.

The HAMT data structure is useful when you want modifications (additions
of pairs, removal, value changes) to be visible only to some part of
your program, whereas other parts of the program continue to use the
unmodified HAMT. The HAMT makes this possible in a space-efficient
manner: the modified and the unmodified HAMT share most of their
allocated memory. It is also time-efficient: Every such modification
is *O(1)* on average, again assuming a good hash function for the keys.

A HAMT can be used whenever an ordinary hash table would be used. It
does however, provide non-destructive updating operations without the
need to copy the whole container. On the other hand, a hash table is
simpler so that its performance may be better when non-destructive
update operations are not needed.

For example, a HAMT can be used to model the dynamic environment in a
LISP interpreter. Updating a value in the dynamic environment of one
continuation frame would not modify values in earlier frames.

To use the module, include `hamt.h` in your code. The public
interface is documented in that header file. You have to provide a hash
function and an equivalence relation, which defines key equality. The
module includes a test file `test-hamt.c`, which demonstrates how
the API can be used.

In the current implementation, each inner node of the HAMT can store up
to *32 = 2^5* entries and subtries. Whenever a collision between
the initial bits of the hash values of two entries would happen, the
next *5* bits of the hash values are examined and the two entries
pushed down one level in the trie.

HAMTs have the same average access times as hash tables but grow and
shrink dynamically, so they use memory more economically and do not have
to be periodically resized.

They were described and analyzed in Phil Bagwell (2000). Ideal
Hash Trees (Report). Infoscience Department, École Polytechnique
Fédérale de Lausanne.

The persistence aspect of the HAMT data structure, which means that each
updating operation (like inserting, replacing, or removing an entry)
returns a new HAMT while leaving the original one intact, is achieved
through structure sharing, which is even safe in the presence of
multiple threads when the used C compiler supports atomics.

---

Next: [Advanced functions on FILE streams](#Advanced-stdio-functions), Previous: [Container data types](#Container-data-types), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.13 Modernized printf [¶](#Modernized-printf-1)

The `*zprintf` family of functions is
a modernized form of the `*printf` family of functions.

#### The problem [¶](#The-problem)

The `*printf` functions have a return type ‘`int`’
and therefore can only produce results that are up to (2 GiB - 1 byte) long.

The problem with this is not so much that it is an arbitrary limitation
(that persists even in processes that have, say, 50 GiB of RAM available).
The bigger problem is that in reliable programs,
it requires handling of an error code `EOVERFLOW`
that indicates a result whose size would be 2 GiB or larger.

How does a reliable program do error handling of `*printf` function calls?
For output to strings and file descriptors
(such as `sprintf` and `dprintf`),
there is no other way than to check each such call.

For output to `FILE` streams (such as `fprintf`),
beginners are tempted to ignore the return value of each call
and instead check `ferror (stream)` at the end.
The problem with this approach is that
at the moment the error is detected,
incorrect output has already been sent onto the stream.
So, in this case as well, the reliable approach is to check each such call.

The *simple format strings* that most programs use in 99% of the places,
namely with no wide string or wide character arguments,
nor with widths passed as `int` argument,
can only fail with two possible error codes:

* `ENOMEM`, when
  the result would be too large to allocate in the process’ memory.
* `EOVERFLOW`, when
  the result is 2 GiB or larger but still allocatable.

Many GNU programs use “checking” wrappers (functions `xvasprintf`, etc.)
that check for `ENOMEM` and call `xalloc_die`,
thus aborting the program in that case.
The problem is that `EOVERFLOW` is not handled, even with such wrappers.

Should `EOVERFLOW` be handled like `ENOMEM`, by aborting the program?
No, as mentioned above, that would be an arbitrary limitation, which the
[GNU Coding Standards](https://www.gnu.org/prep/standards/html_node/Semantics.html)
urge us to avoid.

#### The solution [¶](#The-solution)

The `*zprintf` functions are like the `*printf` functions,
except that the return type is

* `ptrdiff_t` instead of `int`,
  for output to strings,
* `off64_t` (which is always equivalent to `int64_t`)
  instead of `int`,
  for output to file streams and file descriptors.

Thus, for these functions, `EOVERFLOW` cannot occur
(except for format strings which take widths as argument,
which we have excluded above),
and the “checking” wrappers (functions `xvasprintf`, etc.)
are thus sufficient for ensuring an error-free result.

Note:
In 64-bit processes, `ptrdiff_t` is 64 bits wide,
i.e. equivalent to `int64_t`.
In 32-bit processes, `ptrdiff_t` is only 32 bits wide,
but since in these environments,
memory regions of 2 GiB or larger cannot be allocated anyway
(`malloc` would fail with `ENOMEM`),
this type is sufficient.

The following Gnulib functions and modules exist:

| Original function | Modernized function | Modules |
| --- | --- | --- |
| `sprintf` | `szprintf` | `szprintf`, `szprintf-posix`, `szprintf-gnu` |
| `vsprintf` | `vszprintf` | `vszprintf`, `vszprintf-posix`, `vszprintf-gnu` |
| `snprintf` | `snzprintf` | `snzprintf`, `snzprintf-posix`, `snzprintf-gnu` |
| `vsnprintf` | `vsnzprintf` | `vsnzprintf`, `vsnzprintf-posix`, `vsnzprintf-gnu` |
| `asprintf` | `aszprintf` | `vaszprintf`, `vaszprintf-posix`, `vaszprintf-gnu` |
| `vasprintf` | `vaszprintf` | `vaszprintf`, `vaszprintf-posix`, `vaszprintf-gnu` |
| `c_snprintf` | `c_snzprintf` | `c-snzprintf`, `c-snzprintf-gnu` |
| `c_vsnprintf` | `c_vsnzprintf` | `c-vsnzprintf`, `c-vsnzprintf-gnu` |
| `c_asprintf` | `c_aszprintf` | `c-vaszprintf`, `c-vaszprintf-gnu` |
| `c_vasprintf` | `c_vaszprintf` | `c-vaszprintf`, `c-vaszprintf-gnu` |
| `fprintf` | `fzprintf` | `fzprintf`, `fzprintf-posix`, `fzprintf-gnu` |
| `vfprintf` | `vfzprintf` | `vfzprintf`, `vfzprintf-posix`, `vfzprintf-gnu` |
| `printf` | `zprintf` | `zprintf`, `zprintf-posix`, `zprintf-gnu` |
| `vprintf` | `vzprintf` | `vzprintf`, `vzprintf-posix`, `vzprintf-gnu` |
| `dprintf` | `dzprintf` | `dzprintf`, `dzprintf-posix`, `dzprintf-gnu` |
| `vdprintf` | `vdzprintf` | `vdzprintf`, `vdzprintf-posix`, `vdzprintf-gnu` |
| `obstack_printf` | `obstack_zprintf` | `obstack-zprintf`, `obstack-zprintf-posix`, `obstack-zprintf-gnu` |
| `obstack_vprintf` | `obstack_vzprintf` | `obstack-zprintf`, `obstack-zprintf-posix`, `obstack-zprintf-gnu` |

The following functions use the `*zprintf` functions under the hood
and thus don’t need a `*zprintf` variant:

| Function | Modules |
| --- | --- |
| `xasprintf` | `xvasprintf`, `xvasprintf-posix`, `xvasprintf-gnu` |
| `xvasprintf` | `xvasprintf`, `xvasprintf-posix`, `xvasprintf-gnu` |
| `c_xasprintf` | `c-xvasprintf` |
| `c_xvasprintf` | `c-xvasprintf` |
| `xprintf` | `xprintf`, `xprintf-posix`, `xprintf-gnu` |
| `xvprintf` | `xprintf`, `xprintf-posix`, `xprintf-gnu` |
| `xfprintf` | `xprintf`, `xprintf-posix`, `xprintf-gnu` |
| `xvfprintf` | `xprintf`, `xprintf-posix`, `xprintf-gnu` |

Note: Even with the `*zprintf` functions,
you need to be prepared to handle specific error codes
when you use non-simple format strings:

* `EILSEQ` when
  the format string takes wide strings or wide characters as arguments,
* `EOVERFLOW` when
  the format string takes a width as argument
  and you cannot ensure that its value is in the range `0`...`INT_MAX`.

---

Next: [libunistring](#libunistring), Previous: [Modernized printf](#Modernized-printf), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.14 Advanced functions on FILE streams [¶](#Advanced-functions-on-FILE-streams)

Gnulib provides a few functions that operate on `FILE *` streams
and that go beyond what ISO C and POSIX specify.

Each such function is provided in a Gnulib module of the same name.

When Gnulib implemented them in 2007, some people considered them
“unportable”, because they required digging in undocumented internals
of the `FILE` structure.
But Gnulib made them portable, by providing implementations for all
platforms, from glibc to native Windows, and from Minix to QNX.
Nowadays, new platforms already implement them in their libc;
this has been seen for the Android libc and musl libc.

The stream argument of these functions must not be wide-character oriented.
But this is not an actual restriction, since
no programs use wide-character orientation on `FILE` streams anyway:
no one uses the `fwide` function.

* [Determining the state of a FILE stream](#State-of-a-stream)
* [Setting an error in a FILE stream](#Error-in-a-stream)
* [Dropping the contents of the buffers of a FILE stream](#Dropping-the-buffer-contents)
* [Accessing the write buffer of a FILE stream](#Accessing-the-write-buffer)
* [Accessing the read buffer of a FILE stream](#Accessing-the-read-buffer)

---

Next: [Setting an error in a FILE stream](#Error-in-a-stream), Up: [Advanced functions on FILE streams](#Advanced-stdio-functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.14.1 Determining the state of a FILE stream [¶](#Determining-the-state-of-a-FILE-stream)

Gnulib modules: freadable, fwritable, freading, fwriting

The `freadable` function returns
`true` if the given stream supports reading,
`false` if it supports only writing,
i.e. if it was opened write-only or append-only.

The `fwritable` function returns
`true` if the given stream supports writing,
`false` if it supports only reading.

The `freading` function returns
`true` if the given stream is opened read-only,
or if the last operation on the stream was a read operation.

The `fwriting` function returns
`true` if the given stream is opened write-only or append-only,
or if the last operation on the stream was a write operation.

`freading` and `fwriting` will never both be true
on the same stream at the same time.
If the stream supports both reads and writes, then

* both `freading` and `fwriting` might be false
  when the stream is first opened, after read encounters EOF,
  or after `fflush`,
* `freading` might be false or true and `fwriting` might be false
  after repositioning (such as `fseek`, `fsetpos`, or `rewind`),

depending on the underlying implementation.

---

Next: [Dropping the contents of the buffers of a FILE stream](#Dropping-the-buffer-contents), Previous: [Determining the state of a FILE stream](#State-of-a-stream), Up: [Advanced functions on FILE streams](#Advanced-stdio-functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.14.2 Setting an error in a FILE stream [¶](#Setting-an-error-in-a-FILE-stream)

A `FILE *` stream has an error indicator, that can be
tested through the `ferror` function and
cleared through the `clearerr` function.
The error indicator is set when an I/O operation on the stream fails.
But ISO C and POSIX don’t provide a way to set the error indicator!

The `fseterr` function sets the error indicator in the given stream.

---

Next: [Accessing the write buffer of a FILE stream](#Accessing-the-write-buffer), Previous: [Setting an error in a FILE stream](#Error-in-a-stream), Up: [Advanced functions on FILE streams](#Advanced-stdio-functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.14.3 Dropping the contents of the buffers of a FILE stream [¶](#Dropping-the-contents-of-the-buffers-of-a-FILE-stream)

The `fpurge` function drops
the contents of the input and output buffers of the given `FILE *` stream:

* It discards the contents of the output buffer, if the stream is currently
  writing.
  That is the opposite of what the `fflush` function does.
* It discards the contents of the input buffer, if the stream is currently
  reading.
  That is the same as what the `fflush` function does.

---

Next: [Accessing the read buffer of a FILE stream](#Accessing-the-read-buffer), Previous: [Dropping the contents of the buffers of a FILE stream](#Dropping-the-buffer-contents), Up: [Advanced functions on FILE streams](#Advanced-stdio-functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.14.4 Accessing the write buffer of a FILE stream [¶](#Accessing-the-write-buffer-of-a-FILE-stream)

The following two functions provide information about the output buffer
of a `FILE *` stream.

The `fpending` function returns
the number of pending (a.k.a. buffered, unflushed) bytes in the given stream.

The `fbufmode` function returns the buffering mode of the given stream:

| Result | means |
| --- | --- |
| `_IONBF` | unbuffered |
| `_IOLBF` | line buffered |
| `_IOFBF` | fully buffered |

Note that on some platforms,
it is impossible to distinguish `_IOFBF` and `_IOLBF`.

---

Previous: [Accessing the write buffer of a FILE stream](#Accessing-the-write-buffer), Up: [Advanced functions on FILE streams](#Advanced-stdio-functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.14.5 Accessing the read buffer of a FILE stream [¶](#Accessing-the-read-buffer-of-a-FILE-stream)

The following functions allow code to scan the contents of the input buffer
of a `FILE *` stream, without first copying it into a separate memory
area via `fread`.

The function `freadahead` returns
the number of bytes waiting in the input buffer of the given stream.
This includes both
the bytes that have been read from the underlying input source
and the bytes that have been pushed back through `ungetc`.

A function call `freadptr (stream, &size)` returns
a pointer to the input buffer of the given `stream`, or NULL.
If the returned pointer is non-NULL,
`size` is set to the (positive) size of the input buffer.
If the returned pointer is NULL, you should use
`getc (stream)`,
`fgetc (stream)`,
or `fread (..., stream)`
to access the input from `stream`.

A function call `freadseek (stream, offset)`
is the same as `fseek (stream, offset, SEEK_CUR)`,
except that the latter does not work
on non-seekable input streams (such as pipes).

---

Next: [Stack traces](#Stack-traces), Previous: [Advanced functions on FILE streams](#Advanced-stdio-functions), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.15 libunistring [¶](#libunistring-1)

GNU libunistring provides functions for manipulating Unicode strings
and for manipulating C strings according to the Unicode standard.

It consists of the following parts:

|  |  |
| --- | --- |
| `<unistr.h>` | elementary string functions |
| `<uniconv.h>` | conversion from/to legacy encodings |
| `<unistdio.h>` | formatted output to strings |
| `<uniname.h>` | character names |
| `<unictype.h>` | character classification and properties |
| `<uniwidth.h>` | string width when using nonproportional fonts |
| `<unigbrk.h>` | grapheme cluster breaks |
| `<uniwbrk.h>` | word breaks |
| `<unilbrk.h>` | line breaking algorithm |
| `<uninorm.h>` | normalization (composition and decomposition) |
| `<unicase.h>` | case folding |

Nearly all of the source code of GNU libunistring resides in Gnulib.

* [How to use libunistring](#How-to-use-libunistring)
* [The libunistring modules](#The-libunistring-modules)

---

Next: [The libunistring modules](#The-libunistring-modules), Up: [libunistring](#libunistring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.1 How to use libunistring [¶](#How-to-use-libunistring-1)

There are three ways to use GNU libunistring:

* You can declare GNU libunistring as a prerequisite of your package
  and then, at build time, link with the installed libunistring.
  To achieve this, use the Gnulib module `libunistring`.
* You can include the source code of specific libunistring modules
  in your package, through the usual Gnulib mechanism.
  Then, there is no prerequisite.
* A combination of both:
  You can declare GNU libunistring as an optional prerequisite of your package
  and include the source code of specific libunistring modules in your package.
  At configure time, when the user
  has specified the `configure` option `--with-included-libunistring`,
  the included modules will be used
  and the package will not link against an installed libunistring.
  Otherwise, the package will link against an installed libunistring if found,
  and only those modules will be built
  that are not contained or that are too old in the found libunistring.
  To achieve this, request from Gnulib the specific modules
  and the Gnulib module `libunistring-optional`.

---

Previous: [How to use libunistring](#How-to-use-libunistring), Up: [libunistring](#libunistring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2 The libunistring modules [¶](#The-libunistring-modules-1)

Every function or variable that is part of the GNU libunistring API
is available through a specific Gnulib module.
For the documentation, refer to the
[GNU libunistring documentation](https://www.gnu.org/software/libunistring/manual/html_node/).

* [Declarations in <unitypes.h>](#g_t_003cunitypes_002eh_003e-modules)
* [Declarations in <unistr.h>](#g_t_003cunistr_002eh_003e-modules)
* [Declarations in <uniconv.h>](#g_t_003cuniconv_002eh_003e-modules)
* [Declarations in <unistdio.h>](#g_t_003cunistdio_002eh_003e-modules)
* [Declarations in <uniname.h>](#g_t_003cuniname_002eh_003e-modules)
* [Declarations in <unictype.h>](#g_t_003cunictype_002eh_003e-modules)
* [Declarations in <uniwidth.h>](#g_t_003cuniwidth_002eh_003e-modules)
* [Declarations in <unigbrk.h>](#g_t_003cunigbrk_002eh_003e-modules)
* [Declarations in <uniwbrk.h>](#g_t_003cuniwbrk_002eh_003e-modules)
* [Declarations in <unilbrk.h>](#g_t_003cunilbrk_002eh_003e-modules)
* [Declarations in <uninorm.h>](#g_t_003cuninorm_002eh_003e-modules)
* [Declarations in <unicase.h>](#g_t_003cunicase_002eh_003e-modules)

---

Next: [Declarations in <unistr.h>](#g_t_003cunistr_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.1 Declarations in <unitypes.h> [¶](#Declarations-in-_003cunitypes_002eh_003e)

The `<unitypes.h>` header file is provided by the module `unitypes-h`.

---

Next: [Declarations in <uniconv.h>](#g_t_003cuniconv_002eh_003e-modules), Previous: [Declarations in <unitypes.h>](#g_t_003cunitypes_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.2 Declarations in <unistr.h> [¶](#Declarations-in-_003cunistr_002eh_003e)

| Function | Module |
| --- | --- |
| `u8_check` | `unistr/u8-check` |
| `u16_check` | `unistr/u16-check` |
| `u32_check` | `unistr/u32-check` |
| `u8_to_u16` | `unistr/u8-to-u16` |
| `u8_to_u32` | `unistr/u8-to-u32` |
| `u16_to_u8` | `unistr/u16-to-u8` |
| `u16_to_u32` | `unistr/u16-to-u32` |
| `u32_to_u8` | `unistr/u32-to-u8` |
| `u32_to_u16` | `unistr/u32-to-u16` |
| `u8_mblen` | `unistr/u8-mblen` |
| `u16_mblen` | `unistr/u16-mblen` |
| `u32_mblen` | `unistr/u32-mblen` |
| `u8_mbtouc` | `unistr/u8-mbtouc` |
| `u16_mbtouc` | `unistr/u16-mbtouc` |
| `u32_mbtouc` | `unistr/u32-mbtouc` |
| `u8_mbtouc_unsafe` | `unistr/u8-mbtouc-unsafe` |
| `u16_mbtouc_unsafe` | `unistr/u16-mbtouc-unsafe` |
| `u32_mbtouc_unsafe` | `unistr/u32-mbtouc-unsafe` |
| `u8_mbtoucr` | `unistr/u8-mbtoucr` |
| `u16_mbtoucr` | `unistr/u16-mbtoucr` |
| `u32_mbtoucr` | `unistr/u32-mbtoucr` |
| `u8_uctomb` | `unistr/u8-uctomb` |
| `u16_uctomb` | `unistr/u16-uctomb` |
| `u32_uctomb` | `unistr/u32-uctomb` |
| `u8_cpy` | `unistr/u8-cpy` |
| `u16_cpy` | `unistr/u16-cpy` |
| `u32_cpy` | `unistr/u32-cpy` |
| `u8_pcpy` | `unistr/u8-pcpy` |
| `u16_pcpy` | `unistr/u16-pcpy` |
| `u32_pcpy` | `unistr/u32-pcpy` |
| `u8_move` | `unistr/u8-move` |
| `u16_move` | `unistr/u16-move` |
| `u32_move` | `unistr/u32-move` |
| `u8_set` | `unistr/u8-set` |
| `u16_set` | `unistr/u16-set` |
| `u32_set` | `unistr/u32-set` |
| `u8_cmp` | `unistr/u8-cmp` |
| `u16_cmp` | `unistr/u16-cmp` |
| `u32_cmp` | `unistr/u32-cmp` |
| `u8_cmp2` | `unistr/u8-cmp2` |
| `u16_cmp2` | `unistr/u16-cmp2` |
| `u32_cmp2` | `unistr/u32-cmp2` |
| `u8_chr` | `unistr/u8-chr` |
| `u16_chr` | `unistr/u16-chr` |
| `u32_chr` | `unistr/u32-chr` |
| `u8_mbsnlen` | `unistr/u8-mbsnlen` |
| `u16_mbsnlen` | `unistr/u16-mbsnlen` |
| `u32_mbsnlen` | `unistr/u32-mbsnlen` |
| `u8_cpy_alloc` | `unistr/u8-cpy-alloc` |
| `u16_cpy_alloc` | `unistr/u16-cpy-alloc` |
| `u32_cpy_alloc` | `unistr/u32-cpy-alloc` |
| `u8_strmblen` | `unistr/u8-strmblen` |
| `u16_strmblen` | `unistr/u16-strmblen` |
| `u32_strmblen` | `unistr/u32-strmblen` |
| `u8_strmbtouc` | `unistr/u8-strmbtouc` |
| `u16_strmbtouc` | `unistr/u16-strmbtouc` |
| `u32_strmbtouc` | `unistr/u32-strmbtouc` |
| `u8_next` | `unistr/u8-next` |
| `u16_next` | `unistr/u16-next` |
| `u32_next` | `unistr/u32-next` |
| `u8_prev` | `unistr/u8-prev` |
| `u16_prev` | `unistr/u16-prev` |
| `u32_prev` | `unistr/u32-prev` |
| `u8_strlen` | `unistr/u8-strlen` |
| `u16_strlen` | `unistr/u16-strlen` |
| `u32_strlen` | `unistr/u32-strlen` |
| `u8_strnlen` | `unistr/u8-strnlen` |
| `u16_strnlen` | `unistr/u16-strnlen` |
| `u32_strnlen` | `unistr/u32-strnlen` |
| `u8_strcpy` | `unistr/u8-strcpy` |
| `u16_strcpy` | `unistr/u16-strcpy` |
| `u32_strcpy` | `unistr/u32-strcpy` |
| `u8_stpcpy` | `unistr/u8-stpcpy` |
| `u16_stpcpy` | `unistr/u16-stpcpy` |
| `u32_stpcpy` | `unistr/u32-stpcpy` |
| `u8_strncpy` | `unistr/u8-strncpy` |
| `u16_strncpy` | `unistr/u16-strncpy` |
| `u32_strncpy` | `unistr/u32-strncpy` |
| `u8_stpncpy` | `unistr/u8-stpncpy` |
| `u16_stpncpy` | `unistr/u16-stpncpy` |
| `u32_stpncpy` | `unistr/u32-stpncpy` |
| `u8_strcat` | `unistr/u8-strcat` |
| `u16_strcat` | `unistr/u16-strcat` |
| `u32_strcat` | `unistr/u32-strcat` |
| `u8_strncat` | `unistr/u8-strncat` |
| `u16_strncat` | `unistr/u16-strncat` |
| `u32_strncat` | `unistr/u32-strncat` |
| `u8_strcmp` | `unistr/u8-strcmp` |
| `u16_strcmp` | `unistr/u16-strcmp` |
| `u32_strcmp` | `unistr/u32-strcmp` |
| `u8_strcoll` | `unistr/u8-strcoll` |
| `u16_strcoll` | `unistr/u16-strcoll` |
| `u32_strcoll` | `unistr/u32-strcoll` |
| `u8_strncmp` | `unistr/u8-strncmp` |
| `u16_strncmp` | `unistr/u16-strncmp` |
| `u32_strncmp` | `unistr/u32-strncmp` |
| `u8_strdup` | `unistr/u8-strdup` |
| `u16_strdup` | `unistr/u16-strdup` |
| `u32_strdup` | `unistr/u32-strdup` |
| `u8_strchr` | `unistr/u8-strchr` |
| `u16_strchr` | `unistr/u16-strchr` |
| `u32_strchr` | `unistr/u32-strchr` |
| `u8_strrchr` | `unistr/u8-strrchr` |
| `u16_strrchr` | `unistr/u16-strrchr` |
| `u32_strrchr` | `unistr/u32-strrchr` |
| `u8_strcspn` | `unistr/u8-strcspn` |
| `u16_strcspn` | `unistr/u16-strcspn` |
| `u32_strcspn` | `unistr/u32-strcspn` |
| `u8_strspn` | `unistr/u8-strspn` |
| `u16_strspn` | `unistr/u16-strspn` |
| `u32_strspn` | `unistr/u32-strspn` |
| `u8_strpbrk` | `unistr/u8-strpbrk` |
| `u16_strpbrk` | `unistr/u16-strpbrk` |
| `u32_strpbrk` | `unistr/u32-strpbrk` |
| `u8_strstr` | `unistr/u8-strstr` |
| `u16_strstr` | `unistr/u16-strstr` |
| `u32_strstr` | `unistr/u32-strstr` |
| `u8_startswith` | `unistr/u8-startswith` |
| `u16_startswith` | `unistr/u16-startswith` |
| `u32_startswith` | `unistr/u32-startswith` |
| `u8_endswith` | `unistr/u8-endswith` |
| `u16_endswith` | `unistr/u16-endswith` |
| `u32_endswith` | `unistr/u32-endswith` |
| `u8_strtok` | `unistr/u8-strtok` |
| `u16_strtok` | `unistr/u16-strtok` |
| `u32_strtok` | `unistr/u32-strtok` |

---

Next: [Declarations in <unistdio.h>](#g_t_003cunistdio_002eh_003e-modules), Previous: [Declarations in <unistr.h>](#g_t_003cunistr_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.3 Declarations in <uniconv.h> [¶](#Declarations-in-_003cuniconv_002eh_003e)

| Function | Module |
| --- | --- |
| `locale_charset` | `localcharset` |
| `u8_conv_from_encoding` | `uniconv/u8-conv-from-enc` |
| `u16_conv_from_encoding` | `uniconv/u16-conv-from-enc` |
| `u32_conv_from_encoding` | `uniconv/u32-conv-from-enc` |
| `u8_conv_to_encoding` | `uniconv/u8-conv-to-enc` |
| `u16_conv_to_encoding` | `uniconv/u16-conv-to-enc` |
| `u32_conv_to_encoding` | `uniconv/u32-conv-to-enc` |
| `u8_strconv_from_encoding` | `uniconv/u8-strconv-from-enc` |
| `u16_strconv_from_encoding` | `uniconv/u16-strconv-from-enc` |
| `u32_strconv_from_encoding` | `uniconv/u32-strconv-from-enc` |
| `u8_strconv_to_encoding` | `uniconv/u8-strconv-to-enc` |
| `u16_strconv_to_encoding` | `uniconv/u16-strconv-to-enc` |
| `u32_strconv_to_encoding` | `uniconv/u32-strconv-to-enc` |
| `u8_strconv_from_locale` | `uniconv/u8-strconv-from-locale` |
| `u16_strconv_from_locale` | `uniconv/u16-strconv-from-locale` |
| `u32_strconv_from_locale` | `uniconv/u32-strconv-from-locale` |
| `u8_strconv_to_locale` | `uniconv/u8-strconv-to-locale` |
| `u16_strconv_to_locale` | `uniconv/u16-strconv-to-locale` |
| `u32_strconv_to_locale` | `uniconv/u32-strconv-to-locale` |

---

Next: [Declarations in <uniname.h>](#g_t_003cuniname_002eh_003e-modules), Previous: [Declarations in <uniconv.h>](#g_t_003cuniconv_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.4 Declarations in <unistdio.h> [¶](#Declarations-in-_003cunistdio_002eh_003e)

| Function | Module |
| --- | --- |
| `ulc_sprintf` | `unistdio/ulc-sprintf` |
| `ulc_snprintf` | `unistdio/ulc-snprintf` |
| `ulc_asprintf` | `unistdio/ulc-asprintf` |
| `ulc_asnprintf` | `unistdio/ulc-asnprintf` |
| `ulc_vsprintf` | `unistdio/ulc-vsprintf` |
| `ulc_vsnprintf` | `unistdio/ulc-vsnprintf` |
| `ulc_vasprintf` | `unistdio/ulc-vasprintf` |
| `ulc_vasnprintf` | `unistdio/ulc-vasnprintf` |
| `u8_sprintf` | `unistdio/u8-sprintf` |
| `u8_snprintf` | `unistdio/u8-snprintf` |
| `u8_asprintf` | `unistdio/u8-asprintf` |
| `u8_asnprintf` | `unistdio/u8-asnprintf` |
| `u8_vsprintf` | `unistdio/u8-vsprintf` |
| `u8_vsnprintf` | `unistdio/u8-vsnprintf` |
| `u8_vasprintf` | `unistdio/u8-vasprintf` |
| `u8_vasnprintf` | `unistdio/u8-vasnprintf` |
| `u8_u8_sprintf` | `unistdio/u8-u8-sprintf` |
| `u8_u8_snprintf` | `unistdio/u8-u8-snprintf` |
| `u8_u8_asprintf` | `unistdio/u8-u8-asprintf` |
| `u8_u8_asnprintf` | `unistdio/u8-u8-asnprintf` |
| `u8_u8_vsprintf` | `unistdio/u8-u8-vsprintf` |
| `u8_u8_vsnprintf` | `unistdio/u8-u8-vsnprintf` |
| `u8_u8_vasprintf` | `unistdio/u8-u8-vasprintf` |
| `u8_u8_vasnprintf` | `unistdio/u8-u8-vasnprintf` |
| `u16_sprintf` | `unistdio/u16-sprintf` |
| `u16_snprintf` | `unistdio/u16-snprintf` |
| `u16_asprintf` | `unistdio/u16-asprintf` |
| `u16_asnprintf` | `unistdio/u16-asnprintf` |
| `u16_vsprintf` | `unistdio/u16-vsprintf` |
| `u16_vsnprintf` | `unistdio/u16-vsnprintf` |
| `u16_vasprintf` | `unistdio/u16-vasprintf` |
| `u16_vasnprintf` | `unistdio/u16-vasnprintf` |
| `u16_u16_sprintf` | `unistdio/u16-u16-sprintf` |
| `u16_u16_snprintf` | `unistdio/u16-u16-snprintf` |
| `u16_u16_asprintf` | `unistdio/u16-u16-asprintf` |
| `u16_u16_asnprintf` | `unistdio/u16-u16-asnprintf` |
| `u16_u16_vsprintf` | `unistdio/u16-u16-vsprintf` |
| `u16_u16_vsnprintf` | `unistdio/u16-u16-vsnprintf` |
| `u16_u16_vasprintf` | `unistdio/u16-u16-vasprintf` |
| `u16_u16_vasnprintf` | `unistdio/u16-u16-vasnprintf` |
| `u32_sprintf` | `unistdio/u32-sprintf` |
| `u32_snprintf` | `unistdio/u32-snprintf` |
| `u32_asprintf` | `unistdio/u32-asprintf` |
| `u32_asnprintf` | `unistdio/u32-asnprintf` |
| `u32_vsprintf` | `unistdio/u32-vsprintf` |
| `u32_vsnprintf` | `unistdio/u32-vsnprintf` |
| `u32_vasprintf` | `unistdio/u32-vasprintf` |
| `u32_vasnprintf` | `unistdio/u32-vasnprintf` |
| `u32_u32_sprintf` | `unistdio/u32-u32-sprintf` |
| `u32_u32_snprintf` | `unistdio/u32-u32-snprintf` |
| `u32_u32_asprintf` | `unistdio/u32-u32-asprintf` |
| `u32_u32_asnprintf` | `unistdio/u32-u32-asnprintf` |
| `u32_u32_vsprintf` | `unistdio/u32-u32-vsprintf` |
| `u32_u32_vsnprintf` | `unistdio/u32-u32-vsnprintf` |
| `u32_u32_vasprintf` | `unistdio/u32-u32-vasprintf` |
| `u32_u32_vasnprintf` | `unistdio/u32-u32-vasnprintf` |
| `ulc_fprintf` | `unistdio/ulc-fprintf` |
| `ulc_vfprintf` | `unistdio/ulc-vfprintf` |

---

Next: [Declarations in <unictype.h>](#g_t_003cunictype_002eh_003e-modules), Previous: [Declarations in <unistdio.h>](#g_t_003cunistdio_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.5 Declarations in <uniname.h> [¶](#Declarations-in-_003cuniname_002eh_003e)

| Function | Module |
| --- | --- |
| `unicode_character_name` | `uniname/uniname` |
| `unicode_name_character` | `uniname/uniname` |

---

Next: [Declarations in <uniwidth.h>](#g_t_003cuniwidth_002eh_003e-modules), Previous: [Declarations in <uniname.h>](#g_t_003cuniname_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.6 Declarations in <unictype.h> [¶](#Declarations-in-_003cunictype_002eh_003e)

| Function or constant | Module |
| --- | --- |
| `UC_CATEGORY_L` | `unictype/category-L` |
| `UC_CATEGORY_LC` | `unictype/category-LC` |
| `UC_CATEGORY_Lu` | `unictype/category-Lu` |
| `UC_CATEGORY_Ll` | `unictype/category-Ll` |
| `UC_CATEGORY_Lt` | `unictype/category-Lt` |
| `UC_CATEGORY_Lm` | `unictype/category-Lm` |
| `UC_CATEGORY_Lo` | `unictype/category-Lo` |
| `UC_CATEGORY_M` | `unictype/category-M` |
| `UC_CATEGORY_Mn` | `unictype/category-Mn` |
| `UC_CATEGORY_Mc` | `unictype/category-Mc` |
| `UC_CATEGORY_Me` | `unictype/category-Me` |
| `UC_CATEGORY_N` | `unictype/category-N` |
| `UC_CATEGORY_Nd` | `unictype/category-Nd` |
| `UC_CATEGORY_Nl` | `unictype/category-Nl` |
| `UC_CATEGORY_No` | `unictype/category-No` |
| `UC_CATEGORY_P` | `unictype/category-P` |
| `UC_CATEGORY_Pc` | `unictype/category-Pc` |
| `UC_CATEGORY_Pd` | `unictype/category-Pd` |
| `UC_CATEGORY_Ps` | `unictype/category-Ps` |
| `UC_CATEGORY_Pe` | `unictype/category-Pe` |
| `UC_CATEGORY_Pi` | `unictype/category-Pi` |
| `UC_CATEGORY_Pf` | `unictype/category-Pf` |
| `UC_CATEGORY_Po` | `unictype/category-Po` |
| `UC_CATEGORY_S` | `unictype/category-S` |
| `UC_CATEGORY_Sm` | `unictype/category-Sm` |
| `UC_CATEGORY_Sc` | `unictype/category-Sc` |
| `UC_CATEGORY_Sk` | `unictype/category-Sk` |
| `UC_CATEGORY_So` | `unictype/category-So` |
| `UC_CATEGORY_Z` | `unictype/category-Z` |
| `UC_CATEGORY_Zs` | `unictype/category-Zs` |
| `UC_CATEGORY_Zl` | `unictype/category-Zl` |
| `UC_CATEGORY_Zp` | `unictype/category-Zp` |
| `UC_CATEGORY_C` | `unictype/category-C` |
| `UC_CATEGORY_Cc` | `unictype/category-Cc` |
| `UC_CATEGORY_Cf` | `unictype/category-Cf` |
| `UC_CATEGORY_Cs` | `unictype/category-Cs` |
| `UC_CATEGORY_Co` | `unictype/category-Co` |
| `UC_CATEGORY_Cn` | `unictype/category-Cn` |
| `uc_general_category_or` | `unictype/category-or` |
| `uc_general_category_and` | `unictype/category-and` |
| `uc_general_category_and_not` | `unictype/category-and-not` |
| `uc_general_category_name` | `unictype/category-name` |
| `uc_general_category_long_name` | `unictype/category-longname` |
| `uc_general_category_byname` | `unictype/category-byname` |
| `uc_general_category` | `unictype/category-of` |
| `uc_is_general_category` | `unictype/category-test` |
| `uc_is_general_category_withtable` | `unictype/category-test-withtable` |
| `uc_combining_class_name` | `unictype/combining-class-name` |
| `uc_combining_class_long_name` | `unictype/combining-class-longname` |
| `uc_combining_class_byname` | `unictype/combining-class-byname` |
| `uc_combining_class` | `unictype/combining-class` |
| `uc_bidi_class_name` | `unictype/bidiclass-name` |
| `uc_bidi_category_name` | `unictype/bidiclass-name` |
| `uc_bidi_class_long_name` | `unictype/bidiclass-longname` |
| `uc_bidi_class_byname` | `unictype/bidiclass-byname` |
| `uc_bidi_category_byname` | `unictype/bidiclass-byname` |
| `uc_bidi_class` | `unictype/bidiclass-of` |
| `uc_bidi_category` | `unictype/bidiclass-of` |
| `uc_is_bidi_class` | `unictype/bidiclass-test` |
| `uc_is_bidi_category` | `unictype/bidiclass-test` |
| `uc_decimal_value` | `unictype/decimal-digit` |
| `uc_digit_value` | `unictype/digit` |
| `uc_numeric_value` | `unictype/numeric` |
| `uc_mirror_char` | `unictype/mirror` |
| `uc_joining_type_name` | `unictype/joiningtype-name` |
| `uc_joining_type_long_name` | `unictype/joiningtype-longname` |
| `uc_joining_type_byname` | `unictype/joiningtype-byname` |
| `uc_joining_type` | `unictype/joiningtype-of` |
| `uc_joining_group_name` | `unictype/joininggroup-name` |
| `uc_joining_group_byname` | `unictype/joininggroup-byname` |
| `uc_joining_group` | `unictype/joininggroup-of` |
| `UC_PROPERTY_WHITE_SPACE` | `unictype/property-white-space` |
| `UC_PROPERTY_ALPHABETIC` | `unictype/property-alphabetic` |
| `UC_PROPERTY_OTHER_ALPHABETIC` | `unictype/property-other-alphabetic` |
| `UC_PROPERTY_NOT_A_CHARACTER` | `unictype/property-not-a-character` |
| `UC_PROPERTY_DEFAULT_IGNORABLE_CODE_POINT` | `unictype/property-default-ignorable-code-point` |
| `UC_PROPERTY_OTHER_DEFAULT_IGNORABLE_CODE_POINT` | `unictype/property-other-default-ignorable-code-point` |
| `UC_PROPERTY_DEPRECATED` | `unictype/property-deprecated` |
| `UC_PROPERTY_LOGICAL_ORDER_EXCEPTION` | `unictype/property-logical-order-exception` |
| `UC_PROPERTY_VARIATION_SELECTOR` | `unictype/property-variation-selector` |
| `UC_PROPERTY_PRIVATE_USE` | `unictype/property-private-use` |
| `UC_PROPERTY_UNASSIGNED_CODE_VALUE` | `unictype/property-unassigned-code-value` |
| `UC_PROPERTY_UPPERCASE` | `unictype/property-uppercase` |
| `UC_PROPERTY_OTHER_UPPERCASE` | `unictype/property-other-uppercase` |
| `UC_PROPERTY_LOWERCASE` | `unictype/property-lowercase` |
| `UC_PROPERTY_OTHER_LOWERCASE` | `unictype/property-other-lowercase` |
| `UC_PROPERTY_TITLECASE` | `unictype/property-titlecase` |
| `UC_PROPERTY_CASED` | `unictype/property-cased` |
| `UC_PROPERTY_CASE_IGNORABLE` | `unictype/property-case-ignorable` |
| `UC_PROPERTY_CHANGES_WHEN_LOWERCASED` | `unictype/property-changes-when-lowercased` |
| `UC_PROPERTY_CHANGES_WHEN_UPPERCASED` | `unictype/property-changes-when-uppercased` |
| `UC_PROPERTY_CHANGES_WHEN_TITLECASED` | `unictype/property-changes-when-titlecased` |
| `UC_PROPERTY_CHANGES_WHEN_CASEFOLDED` | `unictype/property-changes-when-casefolded` |
| `UC_PROPERTY_CHANGES_WHEN_CASEMAPPED` | `unictype/property-changes-when-casemapped` |
| `UC_PROPERTY_SOFT_DOTTED` | `unictype/property-soft-dotted` |
| `UC_PROPERTY_ID_START` | `unictype/property-id-start` |
| `UC_PROPERTY_OTHER_ID_START` | `unictype/property-other-id-start` |
| `UC_PROPERTY_ID_CONTINUE` | `unictype/property-id-continue` |
| `UC_PROPERTY_OTHER_ID_CONTINUE` | `unictype/property-other-id-continue` |
| `UC_PROPERTY_XID_START` | `unictype/property-xid-start` |
| `UC_PROPERTY_XID_CONTINUE` | `unictype/property-xid-continue` |
| `UC_PROPERTY_ID_COMPAT_MATH_START` | `unictype/property-id-compat-math-start` |
| `UC_PROPERTY_ID_COMPAT_MATH_CONTINUE` | `unictype/property-id-compat-math-continue` |
| `UC_PROPERTY_PATTERN_WHITE_SPACE` | `unictype/property-pattern-white-space` |
| `UC_PROPERTY_PATTERN_SYNTAX` | `unictype/property-pattern-syntax` |
| `UC_PROPERTY_JOIN_CONTROL` | `unictype/property-join-control` |
| `UC_PROPERTY_GRAPHEME_BASE` | `unictype/property-grapheme-base` |
| `UC_PROPERTY_GRAPHEME_EXTEND` | `unictype/property-grapheme-extend` |
| `UC_PROPERTY_OTHER_GRAPHEME_EXTEND` | `unictype/property-other-grapheme-extend` |
| `UC_PROPERTY_GRAPHEME_LINK` | `unictype/property-grapheme-link` |
| `UC_PROPERTY_MODIFIER_COMBINING_MARK` | `unictype/property-modifier-combining-mark` |
| `UC_PROPERTY_BIDI_CONTROL` | `unictype/property-bidi-control` |
| `UC_PROPERTY_BIDI_LEFT_TO_RIGHT` | `unictype/property-bidi-left-to-right` |
| `UC_PROPERTY_BIDI_HEBREW_RIGHT_TO_LEFT` | `unictype/property-bidi-hebrew-right-to-left` |
| `UC_PROPERTY_BIDI_ARABIC_RIGHT_TO_LEFT` | `unictype/property-bidi-arabic-right-to-left` |
| `UC_PROPERTY_BIDI_EUROPEAN_DIGIT` | `unictype/property-bidi-european-digit` |
| `UC_PROPERTY_BIDI_EUR_NUM_SEPARATOR` | `unictype/property-bidi-eur-num-separator` |
| `UC_PROPERTY_BIDI_EUR_NUM_TERMINATOR` | `unictype/property-bidi-eur-num-terminator` |
| `UC_PROPERTY_BIDI_ARABIC_DIGIT` | `unictype/property-bidi-arabic-digit` |
| `UC_PROPERTY_BIDI_COMMON_SEPARATOR` | `unictype/property-bidi-common-separator` |
| `UC_PROPERTY_BIDI_BLOCK_SEPARATOR` | `unictype/property-bidi-block-separator` |
| `UC_PROPERTY_BIDI_SEGMENT_SEPARATOR` | `unictype/property-bidi-segment-separator` |
| `UC_PROPERTY_BIDI_WHITESPACE` | `unictype/property-bidi-whitespace` |
| `UC_PROPERTY_BIDI_NON_SPACING_MARK` | `unictype/property-bidi-non-spacing-mark` |
| `UC_PROPERTY_BIDI_BOUNDARY_NEUTRAL` | `unictype/property-bidi-boundary-neutral` |
| `UC_PROPERTY_BIDI_PDF` | `unictype/property-bidi-pdf` |
| `UC_PROPERTY_BIDI_EMBEDDING_OR_OVERRIDE` | `unictype/property-bidi-embedding-or-override` |
| `UC_PROPERTY_BIDI_OTHER_NEUTRAL` | `unictype/property-bidi-other-neutral` |
| `UC_PROPERTY_HEX_DIGIT` | `unictype/property-hex-digit` |
| `UC_PROPERTY_ASCII_HEX_DIGIT` | `unictype/property-ascii-hex-digit` |
| `UC_PROPERTY_IDEOGRAPHIC` | `unictype/property-ideographic` |
| `UC_PROPERTY_UNIFIED_IDEOGRAPH` | `unictype/property-unified-ideograph` |
| `UC_PROPERTY_RADICAL` | `unictype/property-radical` |
| `UC_PROPERTY_IDS_UNARY_OPERATOR` | `unictype/property-ids-unary-operator` |
| `UC_PROPERTY_IDS_BINARY_OPERATOR` | `unictype/property-ids-binary-operator` |
| `UC_PROPERTY_IDS_TRINARY_OPERATOR` | `unictype/property-ids-trinary-operator` |
| `UC_PROPERTY_EMOJI` | `unictype/property-emoji` |
| `UC_PROPERTY_EMOJI_PRESENTATION` | `unictype/property-emoji-presentation` |
| `UC_PROPERTY_EMOJI_MODIFIER` | `unictype/property-emoji-modifier` |
| `UC_PROPERTY_EMOJI_MODIFIER_BASE` | `unictype/property-emoji-modifier-base` |
| `UC_PROPERTY_EMOJI_COMPONENT` | `unictype/property-emoji-component` |
| `UC_PROPERTY_EXTENDED_PICTOGRAPHIC` | `unictype/property-extended-pictographic` |
| `UC_PROPERTY_ZERO_WIDTH` | `unictype/property-zero-width` |
| `UC_PROPERTY_SPACE` | `unictype/property-space` |
| `UC_PROPERTY_NON_BREAK` | `unictype/property-non-break` |
| `UC_PROPERTY_ISO_CONTROL` | `unictype/property-iso-control` |
| `UC_PROPERTY_FORMAT_CONTROL` | `unictype/property-format-control` |
| `UC_PROPERTY_PREPENDED_CONCATENATION_MARK` | `unictype/property-prepended-concatenation-mark` |
| `UC_PROPERTY_DASH` | `unictype/property-dash` |
| `UC_PROPERTY_HYPHEN` | `unictype/property-hyphen` |
| `UC_PROPERTY_PUNCTUATION` | `unictype/property-punctuation` |
| `UC_PROPERTY_LINE_SEPARATOR` | `unictype/property-line-separator` |
| `UC_PROPERTY_PARAGRAPH_SEPARATOR` | `unictype/property-paragraph-separator` |
| `UC_PROPERTY_QUOTATION_MARK` | `unictype/property-quotation-mark` |
| `UC_PROPERTY_SENTENCE_TERMINAL` | `unictype/property-sentence-terminal` |
| `UC_PROPERTY_TERMINAL_PUNCTUATION` | `unictype/property-terminal-punctuation` |
| `UC_PROPERTY_CURRENCY_SYMBOL` | `unictype/property-currency-symbol` |
| `UC_PROPERTY_MATH` | `unictype/property-math` |
| `UC_PROPERTY_OTHER_MATH` | `unictype/property-other-math` |
| `UC_PROPERTY_PAIRED_PUNCTUATION` | `unictype/property-paired-punctuation` |
| `UC_PROPERTY_LEFT_OF_PAIR` | `unictype/property-left-of-pair` |
| `UC_PROPERTY_COMBINING` | `unictype/property-combining` |
| `UC_PROPERTY_COMPOSITE` | `unictype/property-composite` |
| `UC_PROPERTY_DECIMAL_DIGIT` | `unictype/property-decimal-digit` |
| `UC_PROPERTY_NUMERIC` | `unictype/property-numeric` |
| `UC_PROPERTY_DIACRITIC` | `unictype/property-diacritic` |
| `UC_PROPERTY_EXTENDER` | `unictype/property-extender` |
| `UC_PROPERTY_IGNORABLE_CONTROL` | `unictype/property-ignorable-control` |
| `UC_PROPERTY_REGIONAL_INDICATOR` | `unictype/property-regional-indicator` |
| `uc_property_byname` | `unictype/property-byname` |
| `uc_property_is_valid` | `unictype/base` |
| `uc_is_property` | `unictype/property-test` |
| `uc_is_property_white_space` | `unictype/property-white-space` |
| `uc_is_property_alphabetic` | `unictype/property-alphabetic` |
| `uc_is_property_other_alphabetic` | `unictype/property-other-alphabetic` |
| `uc_is_property_not_a_character` | `unictype/property-not-a-character` |
| `uc_is_property_default_ignorable_code_point` | `unictype/property-default-ignorable-code-point` |
| `uc_is_property_other_default_ignorable_code_point` | `unictype/property-other-default-ignorable-code-point` |
| `uc_is_property_deprecated` | `unictype/property-deprecated` |
| `uc_is_property_logical_order_exception` | `unictype/property-logical-order-exception` |
| `uc_is_property_variation_selector` | `unictype/property-variation-selector` |
| `uc_is_property_private_use` | `unictype/property-private-use` |
| `uc_is_property_unassigned_code_value` | `unictype/property-unassigned-code-value` |
| `uc_is_property_uppercase` | `unictype/property-uppercase` |
| `uc_is_property_other_uppercase` | `unictype/property-other-uppercase` |
| `uc_is_property_lowercase` | `unictype/property-lowercase` |
| `uc_is_property_other_lowercase` | `unictype/property-other-lowercase` |
| `uc_is_property_titlecase` | `unictype/property-titlecase` |
| `uc_is_property_cased` | `unictype/property-cased` |
| `uc_is_property_case_ignorable` | `unictype/property-case-ignorable` |
| `uc_is_property_changes_when_lowercased` | `unictype/property-changes-when-lowercased` |
| `uc_is_property_changes_when_uppercased` | `unictype/property-changes-when-uppercased` |
| `uc_is_property_changes_when_titlecased` | `unictype/property-changes-when-titlecased` |
| `uc_is_property_changes_when_casefolded` | `unictype/property-changes-when-casefolded` |
| `uc_is_property_changes_when_casemapped` | `unictype/property-changes-when-casemapped` |
| `uc_is_property_soft_dotted` | `unictype/property-soft-dotted` |
| `uc_is_property_id_start` | `unictype/property-id-start` |
| `uc_is_property_other_id_start` | `unictype/property-other-id-start` |
| `uc_is_property_id_continue` | `unictype/property-id-continue` |
| `uc_is_property_other_id_continue` | `unictype/property-other-id-continue` |
| `uc_is_property_xid_start` | `unictype/property-xid-start` |
| `uc_is_property_xid_continue` | `unictype/property-xid-continue` |
| `uc_is_property_id_compat_math_start` | `unictype/property-id-compat-math-start` |
| `uc_is_property_id_compat_math_continue` | `unictype/property-id-compat-math-continue` |
| `uc_is_property_pattern_white_space` | `unictype/property-pattern-white-space` |
| `uc_is_property_pattern_syntax` | `unictype/property-pattern-syntax` |
| `uc_is_property_join_control` | `unictype/property-join-control` |
| `uc_is_property_grapheme_base` | `unictype/property-grapheme-base` |
| `uc_is_property_grapheme_extend` | `unictype/property-grapheme-extend` |
| `uc_is_property_other_grapheme_extend` | `unictype/property-other-grapheme-extend` |
| `uc_is_property_grapheme_link` | `unictype/property-grapheme-link` |
| `uc_is_property_modifier_combining_mark` | `unictype/property-modifier-combining-mark` |
| `uc_is_property_bidi_control` | `unictype/property-bidi-control` |
| `uc_is_property_bidi_left_to_right` | `unictype/property-bidi-left-to-right` |
| `uc_is_property_bidi_hebrew_right_to_left` | `unictype/property-bidi-hebrew-right-to-left` |
| `uc_is_property_bidi_arabic_right_to_left` | `unictype/property-bidi-arabic-right-to-left` |
| `uc_is_property_bidi_european_digit` | `unictype/property-bidi-european-digit` |
| `uc_is_property_bidi_eur_num_separator` | `unictype/property-bidi-eur-num-separator` |
| `uc_is_property_bidi_eur_num_terminator` | `unictype/property-bidi-eur-num-terminator` |
| `uc_is_property_bidi_arabic_digit` | `unictype/property-bidi-arabic-digit` |
| `uc_is_property_bidi_common_separator` | `unictype/property-bidi-common-separator` |
| `uc_is_property_bidi_block_separator` | `unictype/property-bidi-block-separator` |
| `uc_is_property_bidi_segment_separator` | `unictype/property-bidi-segment-separator` |
| `uc_is_property_bidi_whitespace` | `unictype/property-bidi-whitespace` |
| `uc_is_property_bidi_non_spacing_mark` | `unictype/property-bidi-non-spacing-mark` |
| `uc_is_property_bidi_boundary_neutral` | `unictype/property-bidi-boundary-neutral` |
| `uc_is_property_bidi_pdf` | `unictype/property-bidi-pdf` |
| `uc_is_property_bidi_embedding_or_override` | `unictype/property-bidi-embedding-or-override` |
| `uc_is_property_bidi_other_neutral` | `unictype/property-bidi-other-neutral` |
| `uc_is_property_hex_digit` | `unictype/property-hex-digit` |
| `uc_is_property_ascii_hex_digit` | `unictype/property-ascii-hex-digit` |
| `uc_is_property_ideographic` | `unictype/property-ideographic` |
| `uc_is_property_unified_ideograph` | `unictype/property-unified-ideograph` |
| `uc_is_property_radical` | `unictype/property-radical` |
| `uc_is_property_ids_unary_operator` | `unictype/property-ids-unary-operator` |
| `uc_is_property_ids_binary_operator` | `unictype/property-ids-binary-operator` |
| `uc_is_property_ids_trinary_operator` | `unictype/property-ids-trinary-operator` |
| `uc_is_property_emoji` | `unictype/property-emoji` |
| `uc_is_property_emoji_presentation` | `unictype/property-emoji-presentation` |
| `uc_is_property_emoji_modifier` | `unictype/property-emoji-modifier` |
| `uc_is_property_emoji_modifier_base` | `unictype/property-emoji-modifier-base` |
| `uc_is_property_emoji_component` | `unictype/property-emoji-component` |
| `uc_is_property_extended_pictographic` | `unictype/property-extended-pictographic` |
| `uc_is_property_zero_width` | `unictype/property-zero-width` |
| `uc_is_property_space` | `unictype/property-space` |
| `uc_is_property_non_break` | `unictype/property-non-break` |
| `uc_is_property_iso_control` | `unictype/property-iso-control` |
| `uc_is_property_format_control` | `unictype/property-format-control` |
| `uc_is_property_prepended_concatenation_mark` | `unictype/property-prepended-concatenation-mark` |
| `uc_is_property_dash` | `unictype/property-dash` |
| `uc_is_property_hyphen` | `unictype/property-hyphen` |
| `uc_is_property_punctuation` | `unictype/property-punctuation` |
| `uc_is_property_line_separator` | `unictype/property-line-separator` |
| `uc_is_property_paragraph_separator` | `unictype/property-paragraph-separator` |
| `uc_is_property_quotation_mark` | `unictype/property-quotation-mark` |
| `uc_is_property_sentence_terminal` | `unictype/property-sentence-terminal` |
| `uc_is_property_terminal_punctuation` | `unictype/property-terminal-punctuation` |
| `uc_is_property_currency_symbol` | `unictype/property-currency-symbol` |
| `uc_is_property_math` | `unictype/property-math` |
| `uc_is_property_other_math` | `unictype/property-other-math` |
| `uc_is_property_paired_punctuation` | `unictype/property-paired-punctuation` |
| `uc_is_property_left_of_pair` | `unictype/property-left-of-pair` |
| `uc_is_property_combining` | `unictype/property-combining` |
| `uc_is_property_composite` | `unictype/property-composite` |
| `uc_is_property_decimal_digit` | `unictype/property-decimal-digit` |
| `uc_is_property_numeric` | `unictype/property-numeric` |
| `uc_is_property_diacritic` | `unictype/property-diacritic` |
| `uc_is_property_extender` | `unictype/property-extender` |
| `uc_is_property_ignorable_control` | `unictype/property-ignorable-control` |
| `uc_is_property_regional_indicator` | `unictype/property-regional-indicator` |
| `uc_indic_conjunct_break_name` | `unictype/incb-name` |
| `uc_indic_conjunct_break_byname` | `unictype/incb-byname` |
| `uc_indic_conjunct_break` | `unictype/incb-of` |
| `uc_script` | `unictype/scripts` |
| `uc_script_byname` | `unictype/scripts` |
| `uc_is_script` | `unictype/scripts` |
| `uc_all_scripts` | `unictype/scripts` |
| `uc_block` | `unictype/block-of` |
| `uc_is_block` | `unictype/block-test` |
| `uc_all_blocks` | `unictype/block-list` |
| `uc_is_c_whitespace` | `unictype/syntax-c-whitespace` |
| `uc_is_java_whitespace` | `unictype/syntax-java-whitespace` |
| `uc_c_ident_category` | `unictype/syntax-c-ident` |
| `uc_java_ident_category` | `unictype/syntax-java-ident` |
| `uc_is_alnum` | `unictype/ctype-alnum` |
| `uc_is_alpha` | `unictype/ctype-alpha` |
| `uc_is_cntrl` | `unictype/ctype-cntrl` |
| `uc_is_digit` | `unictype/ctype-digit` |
| `uc_is_graph` | `unictype/ctype-graph` |
| `uc_is_lower` | `unictype/ctype-lower` |
| `uc_is_print` | `unictype/ctype-print` |
| `uc_is_punct` | `unictype/ctype-punct` |
| `uc_is_space` | `unictype/ctype-space` |
| `uc_is_upper` | `unictype/ctype-upper` |
| `uc_is_xdigit` | `unictype/ctype-xdigit` |
| `uc_is_blank` | `unictype/ctype-blank` |

---

Next: [Declarations in <unigbrk.h>](#g_t_003cunigbrk_002eh_003e-modules), Previous: [Declarations in <unictype.h>](#g_t_003cunictype_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.7 Declarations in <uniwidth.h> [¶](#Declarations-in-_003cuniwidth_002eh_003e)

| Function | Module |
| --- | --- |
| `uc_width` | `uniwidth/width` |
| `u8_width` | `uniwidth/u8-width` |
| `u16_width` | `uniwidth/u16-width` |
| `u32_width` | `uniwidth/u32-width` |
| `u8_strwidth` | `uniwidth/u8-strwidth` |
| `u16_strwidth` | `uniwidth/u16-strwidth` |
| `u32_strwidth` | `uniwidth/u32-strwidth` |

---

Next: [Declarations in <uniwbrk.h>](#g_t_003cuniwbrk_002eh_003e-modules), Previous: [Declarations in <uniwidth.h>](#g_t_003cuniwidth_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.8 Declarations in <unigbrk.h> [¶](#Declarations-in-_003cunigbrk_002eh_003e)

| Function | Module |
| --- | --- |
| `u8_grapheme_next` | `unigbrk/u8-grapheme-next` |
| `u16_grapheme_next` | `unigbrk/u16-grapheme-next` |
| `u32_grapheme_next` | `unigbrk/u32-grapheme-next` |
| `u8_grapheme_prev` | `unigbrk/u8-grapheme-prev` |
| `u16_grapheme_prev` | `unigbrk/u16-grapheme-prev` |
| `u32_grapheme_prev` | `unigbrk/u32-grapheme-prev` |
| `u8_grapheme_breaks` | `unigbrk/u8-grapheme-breaks` |
| `u16_grapheme_breaks` | `unigbrk/u16-grapheme-breaks` |
| `u32_grapheme_breaks` | `unigbrk/u32-grapheme-breaks` |
| `ulc_grapheme_breaks` | `unigbrk/ulc-grapheme-breaks` |
| `uc_grapheme_breaks` | `unigbrk/uc-grapheme-breaks` |
| `uc_graphemeclusterbreak_property` | `unigbrk/uc-gbrk-prop` |
| `uc_is_grapheme_break` | `unigbrk/uc-is-grapheme-break` |

---

Next: [Declarations in <unilbrk.h>](#g_t_003cunilbrk_002eh_003e-modules), Previous: [Declarations in <unigbrk.h>](#g_t_003cunigbrk_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.9 Declarations in <uniwbrk.h> [¶](#Declarations-in-_003cuniwbrk_002eh_003e)

| Function | Module |
| --- | --- |
| `u8_wordbreaks` | `uniwbrk/u8-wordbreaks` |
| `u16_wordbreaks` | `uniwbrk/u16-wordbreaks` |
| `u32_wordbreaks` | `uniwbrk/u32-wordbreaks` |
| `ulc_wordbreaks` | `uniwbrk/ulc-wordbreaks` |
| `uc_wordbreak_property` | `uniwbrk/wordbreak-property` |

---

Next: [Declarations in <uninorm.h>](#g_t_003cuninorm_002eh_003e-modules), Previous: [Declarations in <uniwbrk.h>](#g_t_003cuniwbrk_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.10 Declarations in <unilbrk.h> [¶](#Declarations-in-_003cunilbrk_002eh_003e)

| Function | Module |
| --- | --- |
| `u8_possible_linebreaks` | `unilbrk/u8-possible-linebreaks` |
| `u16_possible_linebreaks` | `unilbrk/u16-possible-linebreaks` |
| `u32_possible_linebreaks` | `unilbrk/u32-possible-linebreaks` |
| `ulc_possible_linebreaks` | `unilbrk/ulc-possible-linebreaks` |
| `u8_width_linebreaks` | `unilbrk/u8-width-linebreaks` |
| `u16_width_linebreaks` | `unilbrk/u16-width-linebreaks` |
| `u32_width_linebreaks` | `unilbrk/u32-width-linebreaks` |
| `ulc_width_linebreaks` | `unilbrk/ulc-width-linebreaks` |

---

Next: [Declarations in <unicase.h>](#g_t_003cunicase_002eh_003e-modules), Previous: [Declarations in <unilbrk.h>](#g_t_003cunilbrk_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.11 Declarations in <uninorm.h> [¶](#Declarations-in-_003cuninorm_002eh_003e)

| Function or macro | Module |
| --- | --- |
| `uc_decomposition` | `uninorm/decomposition` |
| `uc_canonical_decomposition` | `uninorm/canonical-decomposition` |
| `uc_composition` | `uninorm/composition` |
| `UNINORM_NFD` | `uninorm/nfd` |
| `UNINORM_NFC` | `uninorm/nfc` |
| `UNINORM_NFKD` | `uninorm/nfkd` |
| `UNINORM_NFKC` | `uninorm/nfkc` |
| `uninorm_is_compat_decomposing` | `uninorm/base` |
| `uninorm_is_composing` | `uninorm/base` |
| `uninorm_decomposing_form` | `uninorm/decomposing-form` |
| `u8_normalize` | `uninorm/u8-normalize` |
| `u16_normalize` | `uninorm/u16-normalize` |
| `u32_normalize` | `uninorm/u32-normalize` |
| `u8_normcmp` | `uninorm/u8-normcmp` |
| `u16_normcmp` | `uninorm/u16-normcmp` |
| `u32_normcmp` | `uninorm/u32-normcmp` |
| `u8_normxfrm` | `uninorm/u8-normxfrm` |
| `u16_normxfrm` | `uninorm/u16-normxfrm` |
| `u32_normxfrm` | `uninorm/u32-normxfrm` |
| `u8_normcoll` | `uninorm/u8-normcoll` |
| `u16_normcoll` | `uninorm/u16-normcoll` |
| `u32_normcoll` | `uninorm/u32-normcoll` |
| `uninorm_filter_create` | `uninorm/filter` |
| `uninorm_filter_write` | `uninorm/filter` |
| `uninorm_filter_flush` | `uninorm/filter` |
| `uninorm_filter_free` | `uninorm/filter` |

---

Previous: [Declarations in <uninorm.h>](#g_t_003cuninorm_002eh_003e-modules), Up: [The libunistring modules](#The-libunistring-modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 18.15.2.12 Declarations in <unicase.h> [¶](#Declarations-in-_003cunicase_002eh_003e)

| Function or constant | Module |
| --- | --- |
| `uc_toupper` | `unicase/toupper` |
| `uc_tolower` | `unicase/tolower` |
| `uc_totitle` | `unicase/totitle` |
| `uc_locale_language` | `unicase/locale-language` |
| `u8_toupper` | `unicase/u8-toupper` |
| `u16_toupper` | `unicase/u16-toupper` |
| `u32_toupper` | `unicase/u32-toupper` |
| `u8_tolower` | `unicase/u8-tolower` |
| `u16_tolower` | `unicase/u16-tolower` |
| `u32_tolower` | `unicase/u32-tolower` |
| `u8_totitle` | `unicase/u8-totitle` |
| `u16_totitle` | `unicase/u16-totitle` |
| `u32_totitle` | `unicase/u32-totitle` |
| `unicase_empty_prefix_context` | `unicase/empty-prefix-context` |
| `u8_casing_prefix_context` | `unicase/u8-prefix-context` |
| `u16_casing_prefix_context` | `unicase/u16-prefix-context` |
| `u32_casing_prefix_context` | `unicase/u32-prefix-context` |
| `u8_casing_prefixes_context` | `unicase/u8-prefix-context` |
| `u16_casing_prefixes_context` | `unicase/u16-prefix-context` |
| `u32_casing_prefixes_context` | `unicase/u32-prefix-context` |
| `unicase_empty_suffix_context` | `unicase/empty-suffix-context` |
| `u8_casing_suffix_context` | `unicase/u8-suffix-context` |
| `u16_casing_suffix_context` | `unicase/u16-suffix-context` |
| `u32_casing_suffix_context` | `unicase/u32-suffix-context` |
| `u8_casing_suffixes_context` | `unicase/u8-suffix-context` |
| `u16_casing_suffixes_context` | `unicase/u16-suffix-context` |
| `u32_casing_suffixes_context` | `unicase/u32-suffix-context` |
| `u8_ct_toupper` | `unicase/u8-ct-toupper` |
| `u16_ct_toupper` | `unicase/u16-ct-toupper` |
| `u32_ct_toupper` | `unicase/u32-ct-toupper` |
| `u8_ct_tolower` | `unicase/u8-ct-tolower` |
| `u16_ct_tolower` | `unicase/u16-ct-tolower` |
| `u32_ct_tolower` | `unicase/u32-ct-tolower` |
| `u8_ct_totitle` | `unicase/u8-ct-totitle` |
| `u16_ct_totitle` | `unicase/u16-ct-totitle` |
| `u32_ct_totitle` | `unicase/u32-ct-totitle` |
| `u8_casefold` | `unicase/u8-casefold` |
| `u16_casefold` | `unicase/u16-casefold` |
| `u32_casefold` | `unicase/u32-casefold` |
| `u8_ct_casefold` | `unicase/u8-ct-casefold` |
| `u16_ct_casefold` | `unicase/u16-ct-casefold` |
| `u32_ct_casefold` | `unicase/u32-ct-casefold` |
| `u8_casecmp` | `unicase/u8-casecmp` |
| `u16_casecmp` | `unicase/u16-casecmp` |
| `u32_casecmp` | `unicase/u32-casecmp` |
| `ulc_casecmp` | `unicase/ulc-casecmp` |
| `u8_casexfrm` | `unicase/u8-casexfrm` |
| `u16_casexfrm` | `unicase/u16-casexfrm` |
| `u32_casexfrm` | `unicase/u32-casexfrm` |
| `ulc_casexfrm` | `unicase/ulc-casexfrm` |
| `u8_casecoll` | `unicase/u8-casecoll` |
| `u16_casecoll` | `unicase/u16-casecoll` |
| `u32_casecoll` | `unicase/u32-casecoll` |
| `ulc_casecoll` | `unicase/ulc-casecoll` |
| `u8_is_uppercase` | `unicase/u8-is-uppercase` |
| `u16_is_uppercase` | `unicase/u16-is-uppercase` |
| `u32_is_uppercase` | `unicase/u32-is-uppercase` |
| `u8_is_lowercase` | `unicase/u8-is-lowercase` |
| `u16_is_lowercase` | `unicase/u16-is-lowercase` |
| `u32_is_lowercase` | `unicase/u32-is-lowercase` |
| `u8_is_titlecase` | `unicase/u8-is-titlecase` |
| `u16_is_titlecase` | `unicase/u16-is-titlecase` |
| `u32_is_titlecase` | `unicase/u32-is-titlecase` |
| `u8_is_casefolded` | `unicase/u8-is-casefolded` |
| `u16_is_casefolded` | `unicase/u16-is-casefolded` |
| `u32_is_casefolded` | `unicase/u32-is-casefolded` |
| `u8_is_cased` | `unicase/u8-is-cased` |
| `u16_is_cased` | `unicase/u16-is-cased` |
| `u32_is_cased` | `unicase/u32-is-cased` |

---

Next: [Printing –version and bug-reporting information](#Printing-_002d_002dversion-and-bug_002dreporting-information), Previous: [libunistring](#libunistring), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.16 Stack traces [¶](#Stack-traces-1)

Printing a stack trace
was traditionally seen as a feature of the debugging environment
and thus only implemented in the debuggers (`gdb` etc.).
However, they are also useful in production code,
for use in two circumstances:

* When a problem occurs on a user’s machine,
  when the user is merely a user, not a programmer.
* In unit tests that run in continuous-integration environments.
  In such environments, the virtual machine is discarded
  immediately after the tests have run.
  It is not possible to run a debugger in such environments.

And in fact, printing a stack trace is part of the basic runtime system
in programming languages such as
Java ([printStackTrace method](https://docs.oracle.com/javase/8/docs/api/java/lang/Throwable.html#printStackTrace--)),
Python ([print\_exception method](https://docs.python.org/3/library/traceback.html)),
Go ([PrintStack function](https://pkg.go.dev/runtime/debug#PrintStack)),
and
ISO C++ 23 ([std::stacktrace class](https://en.cppreference.com/w/cpp/utility/basic_stacktrace)).

Gnulib provides a module ‘`stack-trace`’ with this feature:
`print_stack_trace ()`
prints a stack trace of the current thread to standard error.

For it to work best, three requirements need to be met:

* The [libbacktrace library](https://github.com/ianlancetaylor/libbacktrace)
  or GCC’s sanitizer library `libasan` needs to be installed.
* The program needs to be compiled with debugging information (option `-g`).
* On macOS, where debugging information
  is stored in a separate directory rather than in the compiled binary
  (see <https://stackoverflow.com/questions/10044697/#12827463>),
  the `dsymutil` program needs to be used when linking,
  and the debugging information needs to be copied when the program is installed.
  Cf. <https://github.com/ianlancetaylor/libbacktrace/issues/122#issuecomment-2122589147>.

When these requirements are not met, the function `print_stack_trace ()`
either prints a stack trace without source file names and line numbers,
or prints nothing at all.

Gnulib also provides a module ‘`abort-debug`’,
that overrides the `abort` function so that
it prints the stack trace of the current thread, before actually aborting.
Thus, `abort ()` remains the idiom of choice
for signaling a fatal situation that requires developer attention:
it is useful both in debugging environments and production code.

Note:
While the original `abort` function is safe to call in signal handlers,
the overridden `abort` function is not.
In signal handlers, you will need to call the original `abort` function,
by doing `#undef abort` first.

---

Next: [Recognizing Option Arguments](#Recognizing-Option-Arguments), Previous: [Stack traces](#Stack-traces), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 18.17 Printing –version and bug-reporting information [¶](#Printing-_002d_002dversion-and-bug_002dreporting-information-1)

Gnulib provides a few modules for printing `--version` and
bug-reporting information according to the GNU Coding Standards; see
<https://www.gnu.org/prep/standards/html_node/_002d_002dversion.html#g_t_002d_002dversion>

The ‘`version-etc`’ module defines functions used to emit
`--version` and bug-reporting input in a standard way. The module
expects the string `version_etc_copyright` to be defined to display
the copyright holder along with the package release year. The
`version_etc` function can then be used like so:

```
const char version_etc_copyright[] = "Copyright %s %d copyright-holder";

int
main (void)
{
  version_etc (stdout, "my-program", "my-package", "1.0",
               "author1", "author2", (const char *) NULL);
  return 0;
}
```

The above example generates the following output:

```
my-program (my-package) 1.0
Copyright (C) 2025 copyright-holder
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by author1 and author2.
```

The ‘`version-etc`’ module also defines the function
`emit_bug_reporting_address` which emits the bug-reporting location
based on arguments passed to `AC_INIT`. For example, using the
following in `configure.ac`:

```
AC_INIT([gnulib], [0.0], [bug-gnulib@gnu.org], [gnulib],
        [https://www.gnu.org/software/gnulib/])
```

And the following program:

```
#include <config.h>

#include "version-etc.h"

int
main (void)
{
  emit_bug_reporting_address ();
  return 0;
}
```

Will output:

```
Report bugs to: bug-gnulib@gnu.org
gnulib home page: <https://www.gnu.org/software/gnulib/>
General help using GNU software: <https://www.gnu.org/gethelp/>
```

The ‘`version-etc-fsf`’ module defines and links to the variable
`version_etc_copyright` containing copyright information for Free
Software Foundation. This module should be used by programs which are
FSF-copyrighted and use the ‘`version-etc`’ module.

The ‘`argp-version-etc`’ defines the `argp_version_setup`
function takes the program name and authors as arguments. This function
should be called before `argp_parse` since it modifies argp global
variables; see
<https://www.gnu.org/software/libc/manual/html_node/Argp-Global-Variables.html>.

---

Next: [Quoting](#Quoting), Previous: [Printing –version and bug-reporting information](#Printing-_002d_002dversion-and-bug_002dreporting-information), Up: [Particular Modules](#Particular-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
