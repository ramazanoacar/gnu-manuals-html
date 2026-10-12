### D.9 Conventional Headers for Emacs Libraries [¶](#Conventional-Headers-for-Emacs-Libraries)

Emacs has conventions for using special comments in Lisp libraries
to divide them into sections and give information such as who wrote
them. Using a standard format for these items makes it easier for
tools (and people) to extract the relevant information. This section
explains these conventions, starting with an example:

```
;;; foo.el --- Support for the Foo programming language  -*- lexical-binding: t; -*-

;; Copyright (C) 2010-2026 Your Name
```

```
;; Author: Your Name <yourname@example.com>
;; Maintainer: Someone Else <someone@example.com>
;; Created: 14 Jul 2010
```

```
;; Keywords: languages
;; URL: https://example.com/foo

;; This file is not part of GNU Emacs.

;; This file is free software...
...
;; along with this file.  If not, see <https://www.gnu.org/licenses/>.
```

The very first line should have this format:

```
;;; filename --- description  -*- lexical-binding: t; -*-
```

The description should be contained in one line. If the file needs to
set more variables in the ‘`-*-`’ specification, add it after
`lexical-binding`. If this would make the first line too long, use
a Local Variables section at the end of the file.

The copyright notice usually lists your name (if you wrote the file).
If you have an employer who claims copyright on your work, you might
need to list them instead. Do not say that the copyright holder is the
Free Software Foundation (or that the file is part of GNU Emacs) unless
your file has been accepted into the Emacs distribution or GNU ELPA.
For more information on the form of copyright and license notices, see
[the guide on the GNU
website](https://www.gnu.org/licenses/gpl-howto.html).

After the copyright notice come several *header comment* lines,
each beginning with ‘`;; header-name:`’. Here is a table of
the conventional possibilities for header-name:

‘`Author`’
:   This header states the name and email address of at least the principal
    author of the library. If there are multiple authors, list them on
    continuation lines led by `;;` and a tab or at least two spaces.
    We recommend including a contact email address, of the form
    ‘`<…>`’. For example:

    ```
    ;; Author: Your Name <yourname@example.com>
    ;;      Someone Else <someone@example.com>
    ;;      Another Person <another@example.com>
    ```

‘`Maintainer`’
:   This header has the same format as the Author header. It lists the
    person(s) who currently maintain(s) the file (respond to bug reports,
    etc.).

    If there is no Maintainer header, the person(s) in the Author header
    is/are presumed to be the maintainer(s). Some files in Emacs use
    ‘`emacs-devel@gnu.org`’ for the maintainer, which means the author is
    no longer responsible for the file, and that it is maintained as part
    of Emacs.

‘`Created`’
:   This optional line gives the original creation date of the file, and
    is for historical interest only.

‘`Version`’
:   If you wish to record version numbers for the individual Lisp program,
    put them in this line. Lisp files distributed with Emacs generally do
    not have a ‘`Version`’ header, since the version number of Emacs
    itself serves the same purpose. If you are distributing a collection
    of multiple files, we recommend not writing the version in every file,
    but only the main one.

‘`Keywords`’ [¶](#index-checkdoc_002dpackage_002dkeywords_002dflag)
:   This line lists keywords for the `finder-by-keyword` help command.
    Please use that command to see a list of the meaningful keywords. The
    command `M-x checkdoc-package-keywords RET` will find and display
    any keywords that are not in `finder-known-keywords`. If you set
    the variable `checkdoc-package-keywords-flag` non-`nil`,
    checkdoc commands will include the keyword verification in its checks.

    This field is how people will find your package when they’re looking
    for things by topic. To separate the keywords, you can use spaces,
    commas, or both.

    The name of this field is unfortunate, since people often assume it is
    the place to write arbitrary keywords that describe their package,
    rather than just the relevant Finder keywords.

‘`URL`’
:   This line states the website of the library. ‘`Homepage`’ is a
    deprecated alternative to ‘`URL`’.

‘`Package-Version`’
:   If ‘`Version`’ is not suitable for use by the package manager, then
    a package can define ‘`Package-Version`’; it will be used instead.
    This is handy if ‘`Version`’ is an RCS id or something else that
    cannot be parsed by `version-to-list`. See [Packaging Basics](#Packaging-Basics).

‘`Package-Requires`’
:   If this exists, it names packages on which the current package depends
    for proper operation. See [Packaging Basics](#Packaging-Basics). This is used by the
    package manager both at download time (to ensure that a complete set
    of packages is downloaded) and at activation time (to ensure that a
    package is only activated if all its dependencies have been).

    Its format is a list of lists on a single line. The `car` of
    each sub-list is the name of a package, as a symbol. The `cadr`
    of each sub-list is the minimum acceptable version number, as a string
    that can be parsed by `version-to-list`. An entry that lacks a
    version (i.e., an entry which is just a symbol, or a sub-list of one
    element) is equivalent to entry with version "0". For instance:

    ```
    ;; Package-Requires: ((gnus "1.0") (bubbles "2.7.2") cl-lib (seq))
    ```

    Packages that don’t need to support Emacs versions older than Emacs 27
    can have the ‘`Package-Requires`’ header split across multiple
    lines, like this:

    ```
    ;; Package-Requires: ((emacs "27.1")
    ;;                    (compat "29.1.4.1"))
    ```

    Note that with this format, you still need to start the list on the
    same line as ‘`Package-Requires`’.

    The package code automatically defines a package named ‘`emacs`’
    with the version number of the currently running Emacs. This can be
    used to require a minimal version of Emacs for a package.

Just about every Lisp library ought to have the ‘`Author`’ and
‘`Keywords`’ header comment lines. Use the others if they are
appropriate. You can also put in header lines with other header
names—they have no standard meanings, so they can’t do any harm.

We use additional stylized comments to subdivide the contents of the
library file. These should be separated from anything else by blank
lines. Here is a table of them:

‘`;;; Commentary:`’
:   This begins introductory comments that explain how the library works.
    It should come right after the copying permissions, and is terminated by
    one of the comment lines described below: ‘`Change Log`’,
    ‘`History`’ or ‘`Code`’. This text is used by the Finder package,
    so it should make sense in that context.

‘`;;; Change Log:`’
:   This begins an optional log of changes to the file over time. Don’t
    put too much information in this section—it is better to keep the
    detailed logs in a version control system (as Emacs does) or in a
    separate `ChangeLog` file. ‘`History`’ is an alternative to
    ‘`Change Log`’.

‘`;;; Code:`’
:   This begins the actual code of the program.

‘`;;; filename ends here`’
:   This is the *footer line*; it appears at the very end of the file.
    Its purpose is to enable people to detect truncated versions of the file
    from the lack of a footer line.

---

Next: [Standard Errors](#Standard-Errors), Previous: [Tips and Conventions](#Tips), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix E GNU Emacs Internals [¶](#GNU-Emacs-Internals-1)

This chapter describes how the runnable Emacs executable is dumped with
the preloaded Lisp libraries in it, how storage is allocated, and some
internal aspects of GNU Emacs that may be of interest to C programmers.

* [Building Emacs](#Building-Emacs)
* [Garbage Collection](#Garbage-Collection)
* [Stack-allocated Objects](#Stack_002dallocated-Objects)
* [Memory Usage](#Memory-Usage)
* [C Dialect](#C-Dialect)
* [Writing Emacs Primitives](#Writing-Emacs-Primitives)
* [Writing Dynamically-Loaded Modules](#Writing-Dynamic-Modules)
* [Object Internals](#Object-Internals)
* [C Integer Types](#C-Integer-Types)

---

Next: [Garbage Collection](#Garbage-Collection), Up: [GNU Emacs Internals](#GNU-Emacs-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### E.1 Building Emacs [¶](#Building-Emacs-1)

This section explains the steps involved in building the Emacs
executable. You don’t have to know this material to build and install
Emacs, since the makefiles do all these things automatically. This
information is pertinent to Emacs developers.

Building Emacs requires GNU Make version 3.81 or later.

Compilation of the C source files in the `src` directory
produces an executable file called `temacs`, also called a
*bare impure Emacs*. It contains the Emacs Lisp interpreter and
I/O routines, but not the editing commands.

The command `temacs -l loadup` would run `temacs`
and direct it to load `loadup.el`. The `loadup` library
loads additional Lisp libraries, which set up the normal Emacs editing
environment. After this step, the Emacs executable is no longer
*bare*.

Because it takes some time to load the standard Lisp files, the
`temacs` executable usually isn’t run directly by users.
Instead, one of the last steps of building Emacs runs the command
‘`temacs -batch -l loadup --temacs=dump-method`’. The
special option `--temacs` tells `temacs` how to record
all the standard preloaded Lisp functions and variables, so that when
you subsequently run Emacs, it will start much faster. The
`--temacs` option requires an argument dump-method, which
can be one of the following:

‘`pdump`’ [¶](#index-dump-file)
:   Record the preloaded Lisp data in a *dump file*. This
    method produces an additional data file which Emacs will load at
    startup. The produced dump file is usually called `emacs.pdmp`,
    and is installed in the Emacs `exec-directory` (see [Help Functions](#Help-Functions)). This method is the most preferred one, as it does not
    require Emacs to employ any special techniques of memory allocation,
    which might get in the way of various memory-layout techniques used by
    modern systems to enhance security and privacy.

‘`pbootstrap`’ [¶](#index-bootstrapping-Emacs)
:   Like ‘`pdump`’, but used while *bootstrapping* Emacs, when no
    previous Emacs binary and no `*.elc` byte-compiled Lisp files are
    available. The produced dump file is usually named
    `bootstrap-emacs.pdmp` in this case.

The dumped `emacs` executable (also called a *pure* Emacs)
is the one which is installed. If the portable dumper was used to
build Emacs, the `emacs` executable is actually an exact copy of
`temacs`, and the corresponding `emacs.pdmp` file is
installed as well. The variable `preloaded-file-list` stores a
list of the preloaded Lisp files recorded in the dump file or
in the dumped Emacs executable. If you port Emacs to a new operating
system, and are not able to implement dumping of any kind, then Emacs
must load `loadup.el` each time it starts.

By default the dumped `emacs` executable records details such
as the build time and host name. Use the
`--disable-build-details` option of `configure` to
suppress these details, so that building and installing Emacs twice
from the same sources is more likely to result in identical copies of
Emacs.

You can specify additional files to preload by writing a library named
`site-load.el` that loads them. However, the advantage of
preloading additional files decreases as machines get faster. On modern
machines, it is usually not advisable.

After `loadup.el` reads `site-load.el`, it finds the
documentation strings for primitive and preloaded functions (and
variables) in the file `etc/DOC` where they are stored, by
calling `Snarf-documentation` (see [Accessing Documentation](#Definition-of-Snarf_002ddocumentation)).

You can specify other Lisp expressions to execute just before dumping
by putting them in a library named `site-init.el`. This file is
executed after the documentation strings are found.

If you want to preload function or variable definitions, there are
three ways you can do this and make their documentation strings
accessible when you subsequently run Emacs:

* Arrange to scan these files when producing the `etc/DOC` file,
  and load them with `site-load.el`.
* Load the files with `site-init.el`, then copy the files into the
  installation directory for Lisp files when you install Emacs.
* Specify a `nil` value for `byte-compile-dynamic-docstrings`
  as a local variable in each of these files, and load them with either
  `site-load.el` or `site-init.el`. (This method has the
  drawback that the documentation strings take up space in Emacs all the
  time.)

It is not advisable to put anything in `site-load.el` or
`site-init.el` that would alter any of the features that users
expect in an ordinary unmodified Emacs. If you feel you must override
normal features for your site, do it with `default.el`, so that
users can override your changes if they wish. See [Summary: Sequence of Actions at Startup](#Startup-Summary).
Note that if either `site-load.el` or `site-init.el` changes
`load-path`, the changes will be lost after dumping.
See [Library Search](#Library-Search). To make a permanent change to
`load-path`, use the `--enable-locallisppath` option
of `configure`.

In a package that can be preloaded, it is sometimes necessary (or
useful) to delay certain evaluations until Emacs subsequently starts
up. The vast majority of such cases relate to the values of
customizable variables. For example, `tutorial-directory` is a
variable defined in `startup.el`, which is preloaded. The default
value is set based on `data-directory`. The variable needs to
access the value of `data-directory` when Emacs starts, not when
it is dumped, because the Emacs executable has probably been installed
in a different location since it was dumped.

Function: **custom-initialize-delay** symbol value [¶](#index-custom_002dinitialize_002ddelay)
:   This function delays the initialization of symbol to the next
    Emacs start. You normally use this function by specifying it as the
    `:initialize` property of a customizable variable. (The argument
    value is unused, and is provided only for compatibility with the
    form Custom expects.)

In the unlikely event that you need a more general functionality than
`custom-initialize-delay` provides, you can use
`before-init-hook` (see [Summary: Sequence of Actions at Startup](#Startup-Summary)).

Function: **dump-emacs-portable** to-file &optional track-referrers [¶](#index-dump_002demacs_002dportable)
:   This function dumps the current state of Emacs into a dump
    file to-file, using the `pdump` method. Normally, the
    dump file is called `emacs-name.dmp`, where
    emacs-name is the name of the Emacs executable file. The
    optional argument track-referrers, if non-`nil`, causes the
    portable dumper to keep additional information to help track
    down the provenance of object types that are not yet supported by the
    `pdump` method.

    Although the portable dumper code can run on many platforms, the dump
    files that it produces are not portable—they can be loaded only by
    the Emacs executable that dumped them.

    If you want to use this function in an Emacs that was already dumped,
    you must run Emacs with the ‘`-batch`’ option.

    If you’re including ‘`.el`’ files in the dumped Emacs and that
    ‘`.el`’ file has code that is normally run at load time, that code
    won’t be run when Emacs starts after dumping. To help work around
    that problem, you can put functions on the
    `after-pdump-load-hook` hook. This hook is run when starting
    Emacs.

Function: **pdumper-stats** [¶](#index-pdumper_002dstats)
:   If the current Emacs session restored its state from a dump
    file, this function returns information about the dump file and the
    time it took to restore the Emacs state. The value is an alist
    `((dumped-with-pdumper . t) (load-time . time) (dump-file-name . file))`,
    where file is the name of the dump file, and time is the
    time in seconds it took to restore the state from the dump file.
    If the current session was not restored from a dump file, the
    value is `nil`.

---

Next: [Stack-allocated Objects](#Stack_002dallocated-Objects), Previous: [Building Emacs](#Building-Emacs), Up: [GNU Emacs Internals](#GNU-Emacs-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### E.2 Garbage Collection [¶](#Garbage-Collection-1)

When a program creates a list or the user defines a new function
(such as by loading a library), that data is placed in normal storage.
If normal storage runs low, then Emacs asks the operating system to
allocate more memory. Different types of Lisp objects, such as
symbols, cons cells, small vectors, markers, etc., are segregated in
distinct blocks in memory. (Large vectors, long strings, buffers and
certain other editing types, which are fairly large, are allocated in
individual blocks, one per object; small strings are packed into blocks
of 8k bytes, and small vectors are packed into blocks of 4k bytes).

Beyond the basic vector, a lot of objects like markers, overlays and
buffers are managed as if they were vectors. The corresponding C data
structures include the `union vectorlike_header` field whose
`size` member contains the subtype enumerated by `enum pvec_type`
and an information about how many `Lisp_Object` fields this structure
contains and what the size of the rest data is. This information is
needed to calculate the memory footprint of an object, and used
by the vector allocation code while iterating over the vector blocks.

It is quite common to use some storage for a while, then release it
by (for example) killing a buffer or deleting the last pointer to an
object. Emacs provides a *garbage collector* to reclaim this
abandoned storage. The garbage collector operates, in essence, by
finding and marking all Lisp objects that are still accessible to Lisp
programs. To begin with, it assumes all the symbols, their values and
associated function definitions, and any data presently on the stack,
are accessible. Any objects that can be reached indirectly through
other accessible objects are also accessible, but this calculation is
done “conservatively”, so it may slightly overestimate how many
objects that are accessible.

When marking is finished, all objects still unmarked are garbage. No
matter what the Lisp program or the user does, it is impossible to refer
to them, since there is no longer a way to reach them. Their space
might as well be reused, since no one will miss them. The second
(sweep) phase of the garbage collector arranges to reuse them. (But
since the marking was done “conservatively”, not all unused objects
are guaranteed to be garbage-collected by any one sweep.)

The sweep phase puts unused cons cells onto a *free list*
for future allocation; likewise for symbols and markers. It compacts
the accessible strings so they occupy fewer 8k blocks; then it frees the
other 8k blocks. Unreachable vectors from vector blocks are coalesced
to create largest possible free areas; if a free area spans a complete
4k block, that block is freed. Otherwise, the free area is recorded
in a free list array, where each entry corresponds to a free list
of areas of the same size. Large vectors, buffers, and other large
objects are allocated and freed individually.

> **Common Lisp note:** Unlike other Lisps, GNU Emacs Lisp does not
> call the garbage collector when the free list is empty. Instead, it
> simply requests the operating system to allocate more storage, and
> processing continues until `gc-cons-threshold` bytes have been
> used.
>
> This means that you can make sure that the garbage collector will not
> run during a certain portion of a Lisp program by calling the garbage
> collector explicitly just before it (provided that portion of the
> program does not use so much space as to force a second garbage
> collection).

Function: **garbage-collect-heapsize** [¶](#index-garbage_002dcollect_002dheapsize)
:   This function returns information on the current memory usage.
    The return value is a list with information on amount of space in
    use, where each entry has the form ‘`(name size used)`’
    or ‘`(name size used free)`’. In the entry,
    name is a symbol describing the kind of objects this entry represents,
    size is the number of bytes used by each one, used is the number
    of those objects that were found live in the heap, and optional free is
    the number of those objects that are not live but that Emacs keeps around for
    future allocations. So an overall result is:

    ```
    ((conses cons-size used-conses free-conses)
     (symbols symbol-size used-symbols free-symbols)
     (strings string-size used-strings free-strings)
     (string-bytes byte-size used-bytes)
     (vectors vector-size used-vectors)
     (vector-slots slot-size used-slots free-slots)
     (floats float-size used-floats free-floats)
     (intervals interval-size used-intervals free-intervals)
     (buffers buffer-size used-buffers)
     (heap unit-size total-size free-size))
    ```

    Here is an example:

    ```
    (garbage-collect)
          ⇒ ((conses 16 49126 8058) (symbols 48 14607 0)
                     (strings 32 2942 2607)
                     (string-bytes 1 78607) (vectors 16 7247)
                     (vector-slots 8 341609 29474) (floats 8 71 102)
                     (intervals 56 27 26) (buffers 944 8)
                     (heap 1024 11715 2678))
    ```

    Below is a table explaining each element. Note that last `heap` entry
    is optional and present only if an underlying `malloc` implementation
    provides `mallinfo` function.

    cons-size
    :   Internal size of a cons cell, i.e., `sizeof (struct Lisp_Cons)`.

    used-conses
    :   The number of cons cells in use.

    free-conses
    :   The number of cons cells for which space has been obtained from
        the operating system, but that are not currently being used.

    symbol-size
    :   Internal size of a symbol, i.e., `sizeof (struct Lisp_Symbol)`.

    used-symbols
    :   The number of symbols in use.

    free-symbols
    :   The number of symbols for which space has been obtained from
        the operating system, but that are not currently being used.

    string-size
    :   Internal size of a string header, i.e., `sizeof (struct Lisp_String)`.

    used-strings
    :   The number of string headers in use.

    free-strings
    :   The number of string headers for which space has been obtained
        from the operating system, but that are not currently being used.

    byte-size
    :   This is used for convenience and equals to `sizeof (char)`.

    used-bytes
    :   The total size of all string data in bytes.

    vector-size
    :   Size in bytes of a vector of length 1, including its header.

    used-vectors
    :   The number of vector headers allocated from the vector blocks.

    slot-size
    :   Internal size of a vector slot, always equal to `sizeof (Lisp_Object)`.

    used-slots
    :   The number of slots in all used vectors.
        Slot counts might include some or all overhead from vector headers,
        depending on the platform.

    free-slots
    :   The number of free slots in all vector blocks.

    float-size
    :   Internal size of a float object, i.e., `sizeof (struct Lisp_Float)`.
        (Do not confuse it with the native platform `float` or `double`.)

    used-floats
    :   The number of floats in use.

    free-floats
    :   The number of floats for which space has been obtained from
        the operating system, but that are not currently being used.

    interval-size
    :   Internal size of an interval object, i.e., `sizeof (struct interval)`.

    used-intervals
    :   The number of intervals in use.

    free-intervals
    :   The number of intervals for which space has been obtained from
        the operating system, but that are not currently being used.

    buffer-size
    :   Internal size of a buffer, i.e., `sizeof (struct buffer)`.
        (Do not confuse with the value returned by `buffer-size` function.)

    used-buffers
    :   The number of buffer objects in use. This includes killed buffers
        invisible to users, i.e., all buffers in `all_buffers` list.

    unit-size
    :   The unit of heap space measurement, always equal to 1024 bytes.

    total-size
    :   Total heap size, in unit-size units.

    free-size
    :   Heap space which is not currently used, in unit-size units.

Command: **garbage-collect** [¶](#index-garbage_002dcollect)
:   This command runs a garbage collection, and returns information on
    the amount of space in use. (Garbage collection can also occur
    spontaneously if you use more than `gc-cons-threshold` bytes of
    Lisp data since the previous garbage collection.)

    `garbage-collect` returns the same list as shown above for
    `garbage-collect-heapsize`.

User Option: **garbage-collection-messages** [¶](#index-garbage_002dcollection_002dmessages)
:   If this variable is non-`nil`, Emacs displays a message at the
    beginning and end of garbage collection. The default value is
    `nil`.

Variable: **post-gc-hook** [¶](#index-post_002dgc_002dhook)
:   This is a normal hook that is run at the end of garbage collection.
    Garbage collection is inhibited while the hook functions run, so be
    careful writing them.

User Option: **gc-cons-threshold** [¶](#index-gc_002dcons_002dthreshold)
:   The value of this variable is the number of bytes of storage that must
    be allocated for Lisp objects after one garbage collection in order to
    trigger another garbage collection. You can use the result returned by
    `garbage-collect` to get an information about size of the particular
    object type; space allocated to the contents of buffers does not count.

    The initial threshold value is `GC_DEFAULT_THRESHOLD`, defined in
    `alloc.c`. Since it’s defined in `word_size` units, the
    value is 400,000 for the default 32-bit configuration, and 800,000 for
    the 64-bit one and for 32-bit builds configured with the
    `--with-wide-int` option. If you specify a larger value, garbage
    collection will happen less often. This reduces the amount of time
    spent garbage collecting (so Lisp programs will run faster between
    cycles of garbage collection that happen more rarely), but increases
    total memory use. You may want to do this when running a program that
    creates lots of Lisp data, especially if you need it to run faster.
    However, we recommend against increasing the threshold for prolonged
    periods of time, and advise that you never set it higher than needed for
    the program to run in reasonable time. Using thresholds higher than
    necessary could potentially cause higher system-wide memory pressure,
    and also make each garbage-collection cycle take much more time, and
    should therefore be avoided.

    You can make collections more frequent by specifying a smaller value, down
    to 1/10th of `GC_DEFAULT_THRESHOLD`. A value less than this minimum
    will remain in effect only until the subsequent garbage collection, at which
    time `garbage-collect` will set the threshold back to the minimum.

User Option: **gc-cons-percentage** [¶](#index-gc_002dcons_002dpercentage)
:   The value of this variable specifies the amount of consing before a
    garbage collection occurs, as a fraction of the current heap size.
    This criterion and `gc-cons-threshold` apply in parallel, and
    garbage collection occurs only when both criteria are satisfied.

    As the heap size increases, the time to perform a garbage collection
    increases. Thus, it can be desirable to do them less frequently in
    proportion.

    The initial percentage value is 0.1 in interactive sessions and while
    dumping Emacs (see [Building Emacs](#Building-Emacs)), and 1.0 in non-interactive
    (a.k.a. “batch”) sessions.

    As with `gc-cons-threshold`, do not enlarge this more than
    necessary, and never for prolonged periods of time.

Control over the garbage collector via `gc-cons-threshold` and
`gc-cons-percentage` is only approximate. Although Emacs checks
for threshold exhaustion regularly, for efficiency reasons it does not
do so immediately after every change to the heap or to
`gc-cons-threshold` or `gc-cons-percentage`, so exhausting
the threshold does not immediately trigger garbage collection. Also,
for efficiency in threshold calculations Emacs approximates the heap
size, which counts the bytes used by currently-accessible objects in
the heap.

The value returned by `garbage-collect` describes the amount of
memory used by Lisp data, broken down by data type. By contrast, the
function `memory-limit` provides information on the total amount of
memory Emacs is currently using.

Function: **memory-limit** [¶](#index-memory_002dlimit)
:   This function returns an estimate of the total amount of bytes of
    virtual memory that Emacs is currently using, divided by 1024.
    You can use this to get a general idea of how your actions affect the
    memory usage.

Variable: **memory-full** [¶](#index-memory_002dfull)
:   This variable is `t` if Emacs is nearly out of memory for Lisp
    objects, and `nil` otherwise.

Function: **memory-use-counts** [¶](#index-memory_002duse_002dcounts)
:   This returns a list of numbers that count the number of objects
    created in this Emacs session. Each of these counters increments for
    a certain kind of object. See the documentation string for details.

Function: **memory-info** [¶](#index-memory_002dinfo)
:   This functions returns an amount of total system memory and how much
    of it is free. On an unsupported system, the value may be `nil`.

    If `default-directory` points to a remote host, memory
    information of that host is returned.

Variable: **gcs-done** [¶](#index-gcs_002ddone)
:   This variable contains the total number of garbage collections
    done so far in this Emacs session.

Variable: **gc-elapsed** [¶](#index-gc_002delapsed)
:   This variable contains the total number of seconds of elapsed time
    during garbage collection so far in this Emacs session, as a
    floating-point number.

Function: **memory-report** [¶](#index-memory_002dreport)
:   It can sometimes be useful to see where Emacs is using memory (in
    various variables, buffers, and caches). This command will open a new
    buffer (called ‘`"*Memory Report*"`’) that will give an overview,
    in addition to listing the “largest” buffers and variables.

    All the data here is approximate, because there’s really no consistent
    way to compute the size of a variable. For instance, two variables
    may share parts of a data structure, and this will be counted twice,
    but this command may still give a useful high-level overview of which
    parts of Emacs are using memory.

---

Next: [Memory Usage](#Memory-Usage), Previous: [Garbage Collection](#Garbage-Collection), Up: [GNU Emacs Internals](#GNU-Emacs-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### E.3 Stack-allocated Objects [¶](#Stack_002dallocated-Objects-1)

The garbage collector described above is used to manage data visible
from Lisp programs, as well as most of the data internally used by the
Lisp interpreter. Sometimes it may be useful to allocate temporary
internal objects using the C stack of the interpreter. This can help
performance, as stack allocation is typically faster than using heap
memory to allocate and the garbage collector to free. The downside is
that using such objects after they are freed results in undefined
behavior, so uses should be well thought out and carefully debugged by
using the `GC_CHECK_MARKED_OBJECTS` feature (see
`src/alloc.c`). In particular, stack-allocated objects should
never be made visible to user Lisp code.

Currently, cons cells and strings can be allocated this way. This
is implemented by C macros like `AUTO_CONS` and
`AUTO_STRING` that define a named `Lisp_Object` with block
lifetime. These objects are not freed by the garbage collector;
instead, they have automatic storage duration, i.e., they are
allocated like local variables and are automatically freed at the end
of execution of the C block that defined the object.

For performance reasons, stack-allocated strings are limited to
ASCII characters, and many of these strings are immutable,
i.e., calling `ASET` on them produces undefined behavior.

---

Next: [C Dialect](#C-Dialect), Previous: [Stack-allocated Objects](#Stack_002dallocated-Objects), Up: [GNU Emacs Internals](#GNU-Emacs-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### E.4 Memory Usage [¶](#Memory-Usage-1)

These functions and variables give information about the total amount
of memory allocation that Emacs has done, broken down by data type.
Note the difference between these and the values returned by
`garbage-collect`; those count objects that currently exist, but
these count the number or size of all allocations, including those for
objects that have since been freed.

Variable: **cons-cells-consed** [¶](#index-cons_002dcells_002dconsed)
:   The total number of cons cells that have been allocated so far
    in this Emacs session.

Variable: **floats-consed** [¶](#index-floats_002dconsed)
:   The total number of floats that have been allocated so far
    in this Emacs session.

Variable: **vector-cells-consed** [¶](#index-vector_002dcells_002dconsed)
:   The total number of vector cells that have been allocated so far
    in this Emacs session.
    This includes vector-like objects such as markers and overlays, plus
    certain objects not visible to users.

Variable: **symbols-consed** [¶](#index-symbols_002dconsed)
:   The total number of symbols that have been allocated so far
    in this Emacs session.

Variable: **string-chars-consed** [¶](#index-string_002dchars_002dconsed)
:   The total number of string characters that have been allocated so far
    in this session.

Variable: **intervals-consed** [¶](#index-intervals_002dconsed)
:   The total number of intervals that have been allocated so far
    in this Emacs session.

Variable: **strings-consed** [¶](#index-strings_002dconsed)
:   The total number of strings that have been allocated so far in this
    Emacs session.

---

Next: [Writing Emacs Primitives](#Writing-Emacs-Primitives), Previous: [Memory Usage](#Memory-Usage), Up: [GNU Emacs Internals](#GNU-Emacs-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### E.5 C Dialect [¶](#C-Dialect-1)

The C part of Emacs is portable to C99 or later: later C features such
as ‘`<stdckdint.h>`’ and ‘`[[noreturn]]`’ are not used without a check,
typically at configuration time, and the Emacs build procedure
provides a substitute implementation if necessary. Some later features,
such as anonymous structures and unions, are too difficult to emulate,
so they are avoided entirely.

At some point in the future the base C dialect will no doubt change to
something later than C99.

---

Next: [Writing Dynamically-Loaded Modules](#Writing-Dynamic-Modules), Previous: [C Dialect](#C-Dialect), Up: [GNU Emacs Internals](#GNU-Emacs-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### E.6 Writing Emacs Primitives [¶](#Writing-Emacs-Primitives-1)

Lisp primitives are Lisp functions implemented in C. The details of
interfacing the C function so that Lisp can call it are handled by a few
C macros. The only way to really understand how to write new C code is
to read the source, but we can explain some things here.

An example of a special form is the definition of `or`, from
`eval.c`. (An ordinary function would have the same general
appearance.)

```
DEFUN ("or", For, Sor, 0, UNEVALLED, 0,
       doc: /* Eval args until one of them yields non-nil,
then return that value.
The remaining args are not evalled at all.
If all args return nil, return nil.
```

```
usage: (or CONDITIONS...)  */)
  (Lisp_Object args)
{
  Lisp_Object val = Qnil;
```

```

```

```
  while (CONSP (args))
    {
      val = eval_sub (XCAR (args));
      if (!NILP (val))
        break;
      args = XCDR (args);
      maybe_quit ();
    }
```

```

```

```
  return val;
}
```

Let’s start with a precise explanation of the arguments to the
`DEFUN` macro. Here is a template for them:

```
DEFUN (lname, fname, sname, min, max, interactive, doc)
```

lname
:   This is the name of the Lisp symbol to define as the function name; in
    the example above, it is `or`.

fname
:   This is the C function name for this function. This is the name that
    is used in C code for calling the function. The name is, by
    convention, ‘`F`’ prepended to the Lisp name, with all dashes
    (‘`-`’) in the Lisp name changed to underscores. Thus, to call
    this function from C code, call `For`.

sname
:   This is a C variable name to use for a structure that holds the data for
    the subr object that represents the function in Lisp. This structure
    conveys the Lisp symbol name to the initialization routine that will
    create the symbol and store the subr object as its definition. By
    convention, this name is always fname with ‘`F`’ replaced with
    ‘`S`’.

min
:   This is the minimum number of arguments that the function requires. The
    function `or` allows a minimum of zero arguments.

max
:   This is the maximum number of arguments that the function accepts, if
    there is a fixed maximum. Alternatively, it can be `UNEVALLED`,
    indicating a special form that receives unevaluated arguments, or
    `MANY`, indicating an unlimited number of evaluated arguments (the
    equivalent of `&rest`). Both `UNEVALLED` and `MANY` are
    macros. If max is a number, it must be more than min but
    less than 8.

interactive [¶](#index-interactive-specification-in-primitives)
:   This is an interactive specification, a string such as might be used
    as the argument of `interactive` in a Lisp function
    (see [Using `interactive`](#Using-Interactive)). In the case
    of `or`, it is `0` (a null pointer), indicating that `or`
    cannot be called interactively. A value of `""` indicates a
    function that should receive no arguments when called interactively.
    If the value begins with a ‘`"(`’, the string is evaluated as a
    Lisp form. For example:

    ```
    DEFUN ("foo", Ffoo, Sfoo, 0, 3,
           "(list (read-char-by-name \"Insert character: \")\
                  (prefix-numeric-value current-prefix-arg)\
                  t)",
           doc: /* ... */)
    ```

doc
:   This is the documentation string. It uses C comment syntax rather
    than C string syntax because comment syntax requires nothing special
    to include multiple lines. The ‘`doc:`’ identifies the comment
    that follows as the documentation string. The ‘`/*`’ and ‘`*/`’
    delimiters that begin and end the comment are not part of the
    documentation string.

    If the last line of the documentation string begins with the keyword
    ‘`usage:`’, the rest of the line is treated as the argument list
    for documentation purposes. This way, you can use different argument
    names in the documentation string from the ones used in the C code.
    ‘`usage:`’ is required if the function has an unlimited number of
    arguments.

    Some primitives have multiple definitions, one per platform (e.g.,
    `x-create-frame`). In such cases, rather than writing the
    same documentation string in each definition, only one definition has
    the actual documentation. The others have placeholders beginning with
    ‘`SKIP`’, which are ignored by the function that parses the
    `DOC` file.

    All the usual rules for documentation strings in Lisp code
    (see [Tips for Documentation Strings](#Documentation-Tips)) apply to C code documentation strings
    too.

    The documentation string can be followed by a list of C function
    attributes for the C function that implements the primitive, like
    this:

    ```
    DEFUN ("bar", Fbar, Sbar, 0, UNEVALLED, 0
           doc: /* ... */
           attributes: attr1 attr2 ...)
    ```

    You can specify more than a single attribute, one after the other.
    Currently, only the following attributes are recognized:

    `noreturn`
    :   Declares the C function as one that never returns. This corresponds
        to C23’s `[[noreturn]]`, to C11’s `_Noreturn`, and to GCC’s
        `__attribute__ ((__noreturn__))` (see [Function
        Attributes](https://gcc.gnu.org/onlinedocs/gcc/Function-Attributes.html#Function-Attributes) in Using the GNU Compiler Collection). (Internally,
        Emacs’s own C code uses `_Noreturn` as it can be defined as a
        macro on C platforms that do not support it.)

    `const`
    :   Declares that the function does not examine any values except its
        arguments, and has no effects except the return value. This
        corresponds to C23’s `[[unsequenced]]` and to GCC’s
        `__attribute__ ((__const__))`.

    `noinline`
    :   This corresponds to `__attribute__ ((__noinline__))`
        attribute of GCC, which prevents the function from being considered
        for inlining. This might be needed, e.g., to countermand effects of
        link-time optimizations on stack-based variables.

After the call to the `DEFUN` macro, you must write the
argument list for the C function, including the types for the
arguments. If the primitive accepts a fixed maximum number of Lisp
arguments, there must be one C argument for each Lisp argument, and
each argument must be of type `Lisp_Object`. (Various macros and
functions for creating values of type `Lisp_Object` are declared
in the file `lisp.h`.) If the primitive is a special form, it
must accept a Lisp list containing its unevaluated Lisp arguments as a
single argument of type `Lisp_Object`. If the primitive has no
upper limit on the number of evaluated Lisp arguments, it must have
exactly two C arguments: the first is the number of Lisp arguments,
and the second is the address of a block containing their values.
These have types `ptrdiff_t` and `Lisp_Object *`,
respectively. Since `Lisp_Object` can hold any Lisp object of
any data type, you can determine the actual data type only at run
time; so if you want a primitive to accept only a certain type of
argument, you must check the type explicitly using a suitable
predicate (see [Type Predicates](#Type-Predicates)).

Within the function `For` itself, the local variable
`args` refers to objects controlled by Emacs’s stack-marking
garbage collector. Although the garbage collector does not reclaim
objects reachable from C `Lisp_Object` stack variables, it may
move some of the components of an object, such as the contents of a
string or the text of a buffer. Therefore, functions that access
these components must take care to refetch their addresses after
performing Lisp evaluation. This means that instead of keeping C
pointers to string contents or buffer text, the code should keep the
buffer or string position, and recompute the C pointer from the
position after performing Lisp evaluation. Lisp evaluation can occur
via calls to `eval_sub` or `Feval`, either directly or
indirectly.

Note the call to `maybe_quit` inside the loop: this function
checks whether the user pressed `C-g`, and if so, aborts the
processing. You should do that in any loop that can potentially
require a large number of iterations; in this case, the list of
arguments could be very long. This increases Emacs responsiveness and
improves user experience.

You must not use C initializers for static or global variables unless
the variables are never written once Emacs is dumped. These variables
with initializers are allocated in an area of memory that becomes
read-only (on certain operating systems) as a result of dumping Emacs.

Defining the C function is not enough to make a Lisp primitive
available; you must also create the Lisp symbol for the primitive and
store a suitable subr object in its function cell. The code looks like
this:

```
defsubr (&sname);
```

Here sname is the name you used as the third argument to `DEFUN`.

If you add a new primitive to a file that already has Lisp primitives
defined in it, find the function (near the end of the file) named
`syms_of_something`, and add the call to `defsubr`
there. If the file doesn’t have this function, or if you create a new
file, add to it a `syms_of_filename` (e.g.,
`syms_of_myfile`). Then find the spot in `emacs.c` where all
of these functions are called, and add a call to
`syms_of_filename` there.

The function `syms_of_filename` is also the place to define
any C variables that are to be visible as Lisp variables.
`DEFVAR_LISP` makes a C variable of type `Lisp_Object` visible
in Lisp. `DEFVAR_INT` makes a C variable of type `int`
visible in Lisp with a value that is always an integer.
`DEFVAR_BOOL` makes a C variable of type `int` visible in Lisp
with a value that is either `t` or `nil`. Note that variables
defined with `DEFVAR_BOOL` are automatically added to the list
`byte-boolean-vars` used by the byte compiler.

These macros all expect three arguments:

`lname`
:   The name of the variable to be used by Lisp programs.

`vname`
:   The name of the variable in the C sources.

`doc`
:   The documentation for the variable, as a C
    comment. See [Documentation Basics](#Documentation-Basics), for more details.

By convention, when defining variables of a “native” type
(`int` and `bool`), the name of the C variable is the name
of the Lisp variable with `-` replaced by `_`. When the
variable has type `Lisp_Object`, the convention is to also prefix
the C variable name with `V`. This is an example:

```
DEFVAR_INT ("my-int-variable", my_int_variable,
           doc: /* An integer variable.  */);

DEFVAR_LISP ("my-lisp-variable", Vmy_lisp_variable,
           doc: /* A Lisp variable.  */);
```

There are situations in Lisp where you need to refer to the symbol
itself rather than the value of that symbol. One such case is when
temporarily overriding the value of a variable, which in Lisp is done
with `let`. In C sources, this is done by defining a
corresponding, constant symbol, and using `specbind`. By
convention, `Qmy_lisp_variable` corresponds to
`Vmy_lisp_variable`; to define it, use the `DEFSYM` macro.

```
DEFSYM (Qmy_lisp_variable, "my-lisp-variable");
```

To perform the actual binding:

```
specbind (Qmy_lisp_variable, Qt);
```

In Lisp, symbols sometimes need to be quoted. To achieve the same
effect in C, you again use the corresponding constant symbol
`Qmy_lisp_variable`. For example, when creating a buffer-local
variable (see [Buffer-Local Variables](#Buffer_002dLocal-Variables)) in Lisp, you would write:

```
(make-variable-buffer-local 'my-lisp-variable)
```

In C, the corresponding code uses `Fmake_variable_buffer_local` in
combination with `DEFSYM`:

```
DEFSYM (Qmy_lisp_variable, "my-lisp-variable");
Fmake_variable_buffer_local (Qmy_lisp_variable);
```

If you want to make a Lisp variable that is defined in C behave
like one declared with `defcustom`, add an appropriate entry to
`cus-start.el`. See [Defining Customization Variables](#Variable-Definitions), for a description of
the format to use.

If you directly define a file-scope C variable of type
`Lisp_Object`, you must protect it from garbage collection by
calling `staticpro` in `syms_of_filename`, like this:

```
staticpro (&variable);
```

Here is another example function, with more complicated arguments.
This comes from the code in `window.c`, and it demonstrates the use
of macros and functions to manipulate Lisp objects.

```
DEFUN ("coordinates-in-window-p", Fcoordinates_in_window_p,
       Scoordinates_in_window_p, 2, 2, 0,
       doc: /* Return non-nil if COORDINATES are in WINDOW.
  ...
```

```
  or `right-margin' is returned.  */)
  (register Lisp_Object coordinates, Lisp_Object window)
{
  struct window *w;
  struct frame *f;
  int x, y;
  Lisp_Object lx, ly;
```

```

```

```
  w = decode_live_window (window);
  f = XFRAME (w->frame);
  CHECK_CONS (coordinates);
  lx = Fcar (coordinates);
  ly = Fcdr (coordinates);
  CHECK_NUMBER (lx);
  CHECK_NUMBER (ly);
  x = FRAME_PIXEL_X_FROM_CANON_X (f, lx) + FRAME_INTERNAL_BORDER_WIDTH (f);
  y = FRAME_PIXEL_Y_FROM_CANON_Y (f, ly) + FRAME_INTERNAL_BORDER_WIDTH (f);
```

```

```

```
  switch (coordinates_in_window (w, x, y))
    {
    case ON_NOTHING:            /* NOT in window at all.  */
      return Qnil;
```

```
    ...
```

```
    case ON_MODE_LINE:          /* In mode line of window.  */
      return Qmode_line;
```

```
    ...
```

```
    case ON_SCROLL_BAR:         /* On scroll-bar of window.  */
      /* Historically we are supposed to return nil in this case.  */
      return Qnil;
```

```

```

```
    default:
      emacs_abort ();
    }
}
```

Note that C code cannot call functions by name unless they are defined
in C. The way to call a function written in Lisp is to use
`Ffuncall`, which embodies the Lisp function `funcall`. Since
the Lisp function `funcall` accepts an unlimited number of
arguments, in C it takes two: the number of Lisp-level arguments, and a
one-dimensional array containing their values. The first Lisp-level
argument is the Lisp function to call, and the rest are the arguments to
pass to it.

The C macro `calln` is a convenient way to call a Lisp function
without having to specify the number of arguments. It works by calling
`Ffuncall`.

`eval.c` is a very good file to look through for examples;
`lisp.h` contains the definitions for some important macros and
functions.

If you define a function which is side-effect free or pure, give it
a non-`nil` `side-effect-free` or `pure` property,
respectively (see [Standard Symbol Properties](#Standard-Properties)). See the lists defined in
‘`byte-opt.el`’.

---

Next: [Object Internals](#Object-Internals), Previous: [Writing Emacs Primitives](#Writing-Emacs-Primitives), Up: [GNU Emacs Internals](#GNU-Emacs-Internals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
