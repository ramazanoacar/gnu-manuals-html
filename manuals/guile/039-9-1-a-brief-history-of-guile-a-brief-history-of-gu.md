### 9.1 A Brief History of Guile [¶](#A-Brief-History-of-Guile)

Guile is an artifact of historical processes, both as code and as a
community of hackers. It is sometimes useful to know this history when
hacking the source code, to know about past decisions and future
directions.

Of course, the real history of Guile is written by the hackers hacking
and not the writers writing, so we round up the section with a note on
current status and future directions.

* [The Emacs Thesis](#The-Emacs-Thesis)
* [Early Days](#Early-Days)
* [A Scheme of Many Maintainers](#A-Scheme-of-Many-Maintainers)
* [A Timeline of Selected Guile Releases](#A-Timeline-of-Selected-Guile-Releases)
* [Status, or: Your Help Needed](#Status)

---

Next: [Early Days](#Early-Days), Up: [A Brief History of Guile](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.1.1 The Emacs Thesis [¶](#The-Emacs-Thesis-1)

The story of Guile is the story of bringing the development experience
of Emacs to the mass of programs on a GNU system.

Emacs, when it was first created in its GNU form in 1984, was a new
take on the problem of “how to make a program”. The Emacs thesis is
that it is delightful to create composite programs based on an
orthogonal kernel written in a low-level language together with a
powerful, high-level extension language.

Extension languages foster extensible programs, programs which adapt
readily to different users and to changing times. Proof of this can be
seen in Emacs’ current and continued existence, spanning more than a
quarter-century.

Besides providing for modification of a program by others, extension
languages are good for *intension* as well. Programs built in
“the Emacs way” are pleasurable and easy for their authors to flesh
out with the features that they need.

After the Emacs experience was appreciated more widely, a number of
hackers started to consider how to spread this experience to the rest
of the GNU system. It was clear that the easiest way to Emacsify a
program would be to embed a shared language implementation into it.

---

Next: [A Scheme of Many Maintainers](#A-Scheme-of-Many-Maintainers), Previous: [The Emacs Thesis](#The-Emacs-Thesis), Up: [A Brief History of Guile](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.1.2 Early Days [¶](#Early-Days-1)

Tom Lord was the first to fully concentrate his efforts on an
embeddable language runtime, which he named “GEL”, the GNU Extension
Language.

GEL was the product of converting SCM, Aubrey Jaffer’s implementation
of Scheme, into something more appropriate to embedding as a library.
(SCM was itself based on an implementation by George Carrette, SIOD.)

Lord managed to convince Richard Stallman to dub GEL the official
extension language for the GNU project. It was a natural fit, given
that Scheme was a cleaner, more modern Lisp than Emacs Lisp. Part of
the argument was that eventually when GEL became more capable, it
could gain the ability to execute other languages, especially Emacs
Lisp.

Due to a naming conflict with another programming language, Lee Thomas
suggested a new name for GEL: “Guile”. Besides being a recursive
acronym, “Guile” craftily follows the naming of its ancestors,
“Planner”, “Conniver”, and “Schemer”. (The latter was truncated
to “Scheme” due to a 6-character file name limit on an old operating
system.) Finally, “Guile” suggests “guy-ell”, or “Guy L.
Steele”, who, together with Gerald Sussman, originally discovered
Scheme.

Around the same time that Guile (then GEL) was readying itself for
public release, another extension language was gaining in popularity,
Tcl. Many developers found advantages in Tcl because of its shell-like
syntax and its well-developed graphical widgets library, Tk. Also, at
the time there was a large marketing push promoting Tcl as a
“universal extension language”.

Richard Stallman, as the primary author of GNU Emacs, had a particular
vision of what extension languages should be, and Tcl did not seem to
him to be as capable as Emacs Lisp. He posted a criticism to the
comp.lang.tcl newsgroup, sparking one of the internet’s legendary
flamewars. As part of these discussions, retrospectively dubbed the
“Tcl Wars”, he announced the Free Software Foundation’s intent to
promote Guile as the extension language for the GNU project.

It is a common misconception that Guile was created as a reaction to
Tcl. While it is true that the public announcement of Guile happened
at the same time as the “Tcl wars”, Guile was created out of a
condition that existed outside the polemic. Indeed, the need for a
powerful language to bridge the gap between extension of existing
applications and a more fully dynamic programming environment is still
with us today.

---

Next: [A Timeline of Selected Guile Releases](#A-Timeline-of-Selected-Guile-Releases), Previous: [Early Days](#Early-Days), Up: [A Brief History of Guile](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.1.3 A Scheme of Many Maintainers [¶](#A-Scheme-of-Many-Maintainers-1)

Surveying the field, it seems that Scheme implementations correspond
with their maintainers on an N-to-1 relationship. That is to say, that
those people that implement Schemes might do so on a number of
occasions, but that the lifetime of a given Scheme is tied to the
maintainership of one individual.

Guile is atypical in this regard.

Tom Lord maintained Guile for its first year and a half or so,
corresponding to the end of 1994 through the middle of 1996. The
releases made in this time constitute an arc from SCM as a standalone
program to Guile as a reusable, embeddable library, but passing
through a explosion of features: embedded Tcl and Tk, a toolchain for
compiling and disassembling Java, addition of a C-like syntax,
creation of a module system, and a start at a rich POSIX interface.

Only some of those features remain in Guile. There were ongoing
tensions between providing a small, embeddable language, and one which
had all of the features (e.g. a graphical toolkit) that a modern Emacs
might need. In the end, as Guile gained in uptake, the development
team decided to focus on depth, documentation and orthogonality rather
than on breadth. This has been the focus of Guile ever since, although
there is a wide range of third-party libraries for Guile.

Jim Blandy presided over that period of stabilization, in the three
years until the end of 1999, when he too moved on to other projects.
Since then, Guile has had a group maintainership. The first group was
Maciej Stachowiak, Mikael Djurfeldt, and Marius Vollmer, with Vollmer
staying on the longest. By late 2007, Marius had mostly moved on to
other things, so Neil Jerram and Ludovic Courtès stepped up to take on
the primary maintenance responsibility. Neil and Ludovic were joined by
Andy Wingo in late 2009, allowing Neil to step away, and Mark Weaver
joined shortly thereafter. After spending more than 5 years in the
role, Mark stepped down as well, leaving Ludovic and Andy as the current
co-maintainers of Guile as of January 2020.

Of course, a large part of the actual work on Guile has come from
other contributors too numerous to mention, but without whom the world
would be a poorer place.

---

Next: [Status, or: Your Help Needed](#Status), Previous: [A Scheme of Many Maintainers](#A-Scheme-of-Many-Maintainers), Up: [A Brief History of Guile](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.1.4 A Timeline of Selected Guile Releases [¶](#A-Timeline-of-Selected-Guile-Releases-1)

guile-i — 4 February 1995
:   SCM, turned into a library.

guile-ii — 6 April 1995
:   A low-level module system was added. Tcl/Tk support was added,
    allowing extension of Scheme by Tcl or vice versa. POSIX support was
    improved, and there was an experimental stab at Java integration.

guile-iii — 18 August 1995
:   The C-like syntax, ctax, was improved, but mostly this release
    featured a start at the task of breaking Guile into pieces.

1.0 — 5 January 1997
:   `#f` was distinguished from `'()`. User-level, cooperative
    multi-threading was added. Source-level debugging became more useful,
    and programmer’s and user’s manuals were begun. The module system
    gained a high-level interface, which is still used today in more or
    less the same form.

1.1 — 16 May 1997

1.2 — 24 June 1997
:   Support for Tcl/Tk and ctax were split off as separate packages, and
    have remained there since. Guile became more compatible with SCSH, and
    more useful as a UNIX scripting language. Libguile could now be built as
    a shared library, and third-party extensions written in C became
    loadable via dynamic linking.

1.3.0 — 19 October 1998
:   Command-line editing became much more pleasant through the use of the
    readline library. The initial support for internationalization via
    multi-byte strings was removed; 10 years were to pass before proper
    internationalization would land again. Initial Emacs Lisp support
    landed, ports gained better support for file descriptors, and fluids
    were added.

1.3.2 — 20 August 1999

1.3.4 — 25 September 1999

1.4 — 21 June 2000
:   A long list of lispy features were added: hooks, Common Lisp’s
    `format`, optional and keyword procedure arguments,
    `getopt-long`, sorting, random numbers, and many other fixes and
    enhancements. Guile also gained an interactive debugger, interactive
    help, and better backtraces.

1.6 — 6 September 2002
:   Guile gained support for the R5RS standard, and added a number of SRFI
    modules. The module system was expanded with programmatic support for
    identifier selection and renaming. The GOOPS object system was merged
    into Guile core.

1.8 — 20 February 2006
:   Guile’s arbitrary-precision arithmetic switched to use the GMP
    library, and added support for exact rationals. Guile’s embedded
    user-space threading was removed in favor of POSIX pre-emptive
    threads, providing true multiprocessing. Gettext support was added,
    and Guile’s C API was cleaned up and orthogonalized in a massive way.

2.0 — 16 February 2010
:   A virtual machine was added to Guile, along with the associated compiler
    and toolchain. Support for internationalization was finally
    reimplemented, in terms of unicode, locales, and libunistring. Running
    Guile instances became controllable and debuggable from within Emacs,
    via Geiser. Guile caught up to features found in a number of other
    Schemes: SRFI-18 threads, module-hygienic macros, a profiler, tracer,
    and debugger, SSAX XML integration, bytevectors, a dynamic FFI,
    delimited continuations, module versions, and partial support for R6RS.

2.2 — 15 March 2017
:   The virtual machine and introduced in 2.0 was completely rewritten,
    along with much of the compiler and toolchain. This speeds up many
    Guile programs as well as reducing startup time and memory usage.
    Guile’s POSIX multithreading was improved, stacks became dynamically
    expandable, the ports facility gained support for non-blocking I/O.

3.0 – January 2020
:   Guile gained support for native code generation via a simple
    just-in-time (JIT) compiler, further improving the speed of its virtual
    machine. The compiler itself gained a number of new optimizations:
    inlining of top-level bindings, better closure optimization, and better
    unboxing of integer and floating-point values. R7RS support was added,
    and R6RS support improved. The exception facility (throw and catch) was
    rewritten in terms of SRFI-34 exception handlers.

---

Previous: [A Timeline of Selected Guile Releases](#A-Timeline-of-Selected-Guile-Releases), Up: [A Brief History of Guile](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.1.5 Status, or: Your Help Needed [¶](#Status_002c-or_003a-Your-Help-Needed)

Guile has achieved much of what it set out to achieve, but there is
much remaining to do.

There is still the old problem of bringing existing applications into
a more Emacs-like experience. Guile has had some successes in this
respect, but still most applications in the GNU system are without
Guile integration.

Getting Guile to those applications takes an investment, the
“hacktivation energy” needed to wire Guile into a program that only
pays off once it is good enough to enable new kinds of behavior. This
would be a great way for new hackers to contribute: take an
application that you use and that you know well, think of something
that it can’t yet do, and figure out a way to integrate Guile and
implement that task in Guile.

With time, perhaps this exposure can reverse itself, whereby programs
can run under Guile instead of vice versa, eventually resulting in the
Emacsification of the entire GNU system. Indeed, this is the reason
for the naming of the many Guile modules that live in the `ice-9`
namespace, a nod to the fictional substance in Kurt Vonnegut’s
novel, Cat’s Cradle, capable of acting as a seed crystal to
crystallize the mass of software.

Implicit to this whole discussion is the idea that dynamic languages
are somehow better than languages like C. While languages like C have
their place, Guile’s take on this question is that yes, Scheme is more
expressive than C, and more fun to write. This realization carries an
imperative with it to write as much code in Scheme as possible rather
than in other languages.

These days it is possible to write extensible applications almost
entirely from high-level languages, through byte-code and native
compilation, speed gains in the underlying hardware, and foreign call
interfaces in the high-level language. Smalltalk systems are like this,
as are Common Lisp-based systems. While there already are a number of
pure-Guile applications out there, in the past users have still needed
to drop down to C for some tasks: interfacing to system libraries that
don’t have prebuilt Guile interfaces, and for some tasks requiring high
performance. With the arrival of native code generation via a JIT
compiler in Guile 3.0, most of these older applications can now be
updated to move more C code to Scheme.

Still, even with an all-Guile application, sometimes you want to
provide an opportunity for users to extend your program from a
language with a syntax that is closer to C, or to Python. Another
interesting idea to consider is compiling e.g. Python to Guile. It’s
not that far-fetched of an idea: see for example IronPython or JRuby.

Also, there’s Emacs itself. Guile’s Emacs Lisp support has reached an
excellent level of correctness, robustness, and speed. However there is
still work to do to finish its integration into Emacs itself. This will
give lots of exciting things to Emacs: native threads, a real object
system, more sophisticated types, cleaner syntax, and access to all of
the Guile extensions.

Finally, so much of the world’s computation is performed in web browsers
that it makes sense to ask ourselves what the Guile-on-the-web-client
story is. With the advent of WebAssembly, there may finally be a
reasonable compilation target that’s present on almost all user-exposed
devices. Especially with the upcoming proposals to allow for tail
calls, delimited continuations, and GC-managed objects, Scheme might
once again have a place in the web browser. Get to it!

---

Next: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile), Previous: [A Brief History of Guile](#History), Up: [Guile Implementation](#Guile-Implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 9.2 Data Representation [¶](#Data-Representation-1)

Scheme is a latently-typed language; this means that the system cannot,
in general, determine the type of a given expression at compile time.
Types only become apparent at run time. Variables do not have fixed
types; a variable may hold a pair at one point, an integer at the next,
and a thousand-element vector later. Instead, values, not variables,
have fixed types.

In order to implement standard Scheme functions like `pair?` and
`string?` and provide garbage collection, the representation of
every value must contain enough information to accurately determine its
type at run time. Often, Scheme systems also use this information to
determine whether a program has attempted to apply an operation to an
inappropriately typed value (such as taking the `car` of a string).

Because variables, pairs, and vectors may hold values of any type,
Scheme implementations use a uniform representation for values — a
single type large enough to hold either a complete value or a pointer
to a complete value, along with the necessary typing information.

The following sections will present a simple typing system, and then
make some refinements to correct its major weaknesses. We then conclude
with a discussion of specific choices that Guile has made regarding
garbage collection and data representation.

* [A Simple Representation](#A-Simple-Representation)
* [Faster Integers](#Faster-Integers)
* [Cheaper Pairs](#Cheaper-Pairs)
* [Conservative Garbage Collection](#Conservative-GC)
* [The SCM Type in Guile](#The-SCM-Type-in-Guile)

---

Next: [Faster Integers](#Faster-Integers), Up: [Data Representation](#Data-Representation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.1 A Simple Representation [¶](#A-Simple-Representation-1)

The simplest way to represent Scheme values in C would be to represent
each value as a pointer to a structure containing a type indicator,
followed by a union carrying the real value. Assuming that `SCM` is
the name of our universal type, we can write:

```
enum type { integer, pair, string, vector, ... };

typedef struct value *SCM;

struct value {
  enum type type;
  union {
    int integer;
    struct { SCM car, cdr; } pair;
    struct { int length; char *elts; } string;
    struct { int length; SCM  *elts; } vector;
    ...
  } value;
};
```

with the ellipses replaced with code for the remaining Scheme types.

This representation is sufficient to implement all of Scheme’s
semantics. If x is an `SCM` value:

* To test if x is an integer, we can write `x->type == integer`.
* To find its value, we can write `x->value.integer`.
* To test if x is a vector, we can write `x->type == vector`.
* If we know x is a vector, we can write
  `x->value.vector.elts[0]` to refer to its first element.
* If we know x is a pair, we can write
  `x->value.pair.car` to extract its car.

---

Next: [Cheaper Pairs](#Cheaper-Pairs), Previous: [A Simple Representation](#A-Simple-Representation), Up: [Data Representation](#Data-Representation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.2 Faster Integers [¶](#Faster-Integers-1)

Unfortunately, the above representation has a serious disadvantage. In
order to return an integer, an expression must allocate a `struct
value`, initialize it to represent that integer, and return a pointer to
it. Furthermore, fetching an integer’s value requires a memory
reference, which is much slower than a register reference on most
processors. Since integers are extremely common, this representation is
too costly, in both time and space. Integers should be very cheap to
create and manipulate.

One possible solution comes from the observation that, on many
architectures, heap-allocated data (i.e., what you get when you call
`malloc`) must be aligned on an eight-byte boundary. (Whether or
not the machine actually requires it, we can write our own allocator for
`struct value` objects that assures this is true.) In this case,
the lower three bits of the structure’s address are known to be zero.

This gives us the room we need to provide an improved representation
for integers. We make the following rules:

* If the lower three bits of an `SCM` value are zero, then the SCM
  value is a pointer to a `struct value`, and everything proceeds as
  before.
* Otherwise, the `SCM` value represents an integer, whose value
  appears in its upper bits.

Here is C code implementing this convention:

```
enum type { pair, string, vector, ... };

typedef struct value *SCM;

struct value {
  enum type type;
  union {
    struct { SCM car, cdr; } pair;
    struct { int length; char *elts; } string;
    struct { int length; SCM  *elts; } vector;
    ...
  } value;
};

#define POINTER_P(x) (((int) (x) & 7) == 0)
#define INTEGER_P(x) (! POINTER_P (x))

#define GET_INTEGER(x)  ((int) (x) >> 3)
#define MAKE_INTEGER(x) ((SCM) (((x) << 3) | 1))
```

Notice that `integer` no longer appears as an element of `enum
type`, and the union has lost its `integer` member. Instead, we
use the `POINTER_P` and `INTEGER_P` macros to make a coarse
classification of values into integers and non-integers, and do further
type testing as before.

Here’s how we would answer the questions posed above (again, assume
x is an `SCM` value):

* To test if x is an integer, we can write `INTEGER_P (x)`.
* To find its value, we can write `GET_INTEGER (x)`.
* To test if x is a vector, we can write:

  ```
    POINTER_P (x) && x->type == vector
  ```

  Given the new representation, we must make sure x is truly a
  pointer before we dereference it to determine its complete type.
* If we know x is a vector, we can write
  `x->value.vector.elts[0]` to refer to its first element, as
  before.
* If we know x is a pair, we can write
  `x->value.pair.car` to extract its car, just as before.

This representation allows us to operate more efficiently on integers
than the first. For example, if x and y are known to be
integers, we can compute their sum as follows:

```
MAKE_INTEGER (GET_INTEGER (x) + GET_INTEGER (y))
```

Now, integer math requires no allocation or memory references. Most real
Scheme systems actually implement addition and other operations using an
even more efficient algorithm, but this essay isn’t about
bit-twiddling. (Hint: how do you decide when to overflow to a bignum?
How would you do it in assembly?)

---

Next: [Conservative Garbage Collection](#Conservative-GC), Previous: [Faster Integers](#Faster-Integers), Up: [Data Representation](#Data-Representation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.3 Cheaper Pairs [¶](#Cheaper-Pairs-1)

However, there is yet another issue to confront. Most Scheme heaps
contain more pairs than any other type of object; Jonathan Rees said at
one point that pairs occupy 45% of the heap in his Scheme
implementation, Scheme 48. However, our representation above spends
three `SCM`-sized words per pair — one for the type, and two for
the CAR and CDR. Is there any way to represent pairs using
only two words?

Let us refine the convention we established earlier. Let us assert
that:

* If the bottom three bits of an `SCM` value are `#b000`, then
  it is a pointer, as before.
* If the bottom three bits are `#b001`, then the upper bits are an
  integer. This is a bit more restrictive than before.
* If the bottom two bits are `#b010`, then the value, with the bottom
  three bits masked out, is the address of a pair.

Here is the new C code:

```
enum type { string, vector, ... };

typedef struct value *SCM;

struct value {
  enum type type;
  union {
    struct { int length; char *elts; } string;
    struct { int length; SCM  *elts; } vector;
    ...
  } value;
};

struct pair {
  SCM car, cdr;
};

#define POINTER_P(x) (((int) (x) & 7) == 0)

#define INTEGER_P(x)  (((int) (x) & 7) == 1)
#define GET_INTEGER(x)  ((int) (x) >> 3)
#define MAKE_INTEGER(x) ((SCM) (((x) << 3) | 1))

#define PAIR_P(x) (((int) (x) & 7) == 2)
#define GET_PAIR(x) ((struct pair *) ((int) (x) & ~7))
```

Notice that `enum type` and `struct value` now only contain
provisions for vectors and strings; both integers and pairs have become
special cases. The code above also assumes that an `int` is large
enough to hold a pointer, which isn’t generally true.

Our list of examples is now as follows:

* To test if x is an integer, we can write `INTEGER_P
  (x)`; this is as before.
* To find its value, we can write `GET_INTEGER (x)`, as
  before.
* To test if x is a vector, we can write:

  ```
    POINTER_P (x) && x->type == vector
  ```

  We must still make sure that x is a pointer to a `struct
  value` before dereferencing it to find its type.
* If we know x is a vector, we can write
  `x->value.vector.elts[0]` to refer to its first element, as
  before.
* We can write `PAIR_P (x)` to determine if x is a
  pair, and then write `GET_PAIR (x)->car` to refer to its
  car.

This change in representation reduces our heap size by 15%. It also
makes it cheaper to decide if a value is a pair, because no memory
references are necessary; it suffices to check the bottom two bits of
the `SCM` value. This may be significant when traversing lists, a
common activity in a Scheme system.

Again, most real Scheme systems use a slightly different implementation;
for example, if GET\_PAIR subtracts off the low bits of `x`, instead
of masking them off, the optimizer will often be able to combine that
subtraction with the addition of the offset of the structure member we
are referencing, making a modified pointer as fast to use as an
unmodified pointer.

---

Next: [The SCM Type in Guile](#The-SCM-Type-in-Guile), Previous: [Cheaper Pairs](#Cheaper-Pairs), Up: [Data Representation](#Data-Representation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.4 Conservative Garbage Collection [¶](#Conservative-Garbage-Collection)

Aside from the latent typing, the major source of constraints on a
Scheme implementation’s data representation is the garbage collector.
The collector must be able to traverse every live object in the heap, to
determine which objects are not live, and thus collectable.

There are many ways to implement this. Guile’s garbage collection is
built on a library, the Boehm-Demers-Weiser conservative garbage
collector (BDW-GC). The BDW-GC “just works”, for the most part. But
since it is interesting to know how these things work, we include here a
high-level description of what the BDW-GC does.

Garbage collection has two logical phases: a *mark* phase, in which
the set of live objects is enumerated, and a *sweep* phase, in which
objects not traversed in the mark phase are collected. Correct
functioning of the collector depends on being able to traverse the
entire set of live objects.

In the mark phase, the collector scans the system’s global variables and
the local variables on the stack to determine which objects are
immediately accessible by the C code. It then scans those objects to
find the objects they point to, and so on. The collector logically sets
a *mark bit* on each object it finds, so each object is traversed
only once.

When the collector can find no unmarked objects pointed to by marked
objects, it assumes that any objects that are still unmarked will never
be used by the program (since there is no path of dereferences from any
global or local variable that reaches them) and deallocates them.

In the above paragraphs, we did not specify how the garbage collector
finds the global and local variables; as usual, there are many different
approaches. Frequently, the programmer must maintain a list of pointers
to all global variables that refer to the heap, and another list
(adjusted upon entry to and exit from each function) of local variables,
for the collector’s benefit.

The list of global variables is usually not too difficult to maintain,
since global variables are relatively rare. However, an explicitly
maintained list of local variables (in the author’s personal experience)
is a nightmare to maintain. Thus, the BDW-GC uses a technique called
*conservative garbage collection*, to make the local variable list
unnecessary.

The trick to conservative collection is to treat the C stack as an
ordinary range of memory, and assume that *every* word on the C
stack is a pointer into the heap. Thus, the collector marks all objects
whose addresses appear anywhere in the C stack, without knowing for sure
how that word is meant to be interpreted.

In addition to the stack, the BDW-GC will also scan static data
sections. This means that global variables are also scanned when looking
for live Scheme objects.

Obviously, such a system will occasionally retain objects that are
actually garbage, and should be freed. In practice, this is not a
problem, as the set of conservatively-scanned locations is fixed; the
Scheme stack is maintained apart from the C stack, and is scanned
precisely (as opposed to conservatively). The GC-managed heap is also
partitioned into parts that can contain pointers (such as vectors) and
parts that can’t (such as bytevectors), limiting the potential for
confusing a raw integer with a pointer to a live object.

Interested readers should see the BDW-GC web page at
<http://www.hboehm.info/gc/>, for more information on conservative
GC in general and the BDW-GC implementation in particular.

---

Previous: [Conservative Garbage Collection](#Conservative-GC), Up: [Data Representation](#Data-Representation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.5 The SCM Type in Guile [¶](#The-SCM-Type-in-Guile-1)

Guile classifies Scheme objects into two kinds: those that fit entirely
within an `SCM`, and those that require heap storage.

The former class are called *immediates*. The class of immediates
includes small integers, characters, boolean values, the empty list, the
mysterious end-of-file object, and some others.

The remaining types are called, not surprisingly, *non-immediates*.
They include pairs, procedures, strings, vectors, and all other data
types in Guile. For non-immediates, the `SCM` word contains a
pointer to data on the heap, with further information about the object
in question is stored in that data.

This section describes how the `SCM` type is actually represented
and used at the C level. Interested readers should see
`libguile/scm.h` for an exposition of how Guile stores type
information.

In fact, there are two basic C data types to represent objects in
Guile: `SCM` and `scm_t_bits`.

* [Relationship Between `SCM` and `scm_t_bits`](#Relationship-Between-SCM-and-scm_005ft_005fbits)
* [Immediate Objects](#Immediate-Objects)
* [Non-Immediate Objects](#Non_002dImmediate-Objects)
* [Allocating Heap Objects](#Allocating-Heap-Objects)
* [Heap Object Type Information](#Heap-Object-Type-Information)
* [Accessing Heap Object Fields](#Accessing-Heap-Object-Fields)

---

Next: [Immediate Objects](#Immediate-Objects), Up: [The SCM Type in Guile](#The-SCM-Type-in-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.5.1 Relationship Between `SCM` and `scm_t_bits` [¶](#Relationship-Between-SCM-and-scm_005ft_005fbits-1)

A variable of type `SCM` is guaranteed to hold a valid Scheme
object. A variable of type `scm_t_bits`, on the other hand, may
hold a representation of a `SCM` value as a C integral type, but
may also hold any C value, even if it does not correspond to a valid
Scheme object.

For a variable x of type `SCM`, the Scheme object’s type
information is stored in a form that is not directly usable. To be able
to work on the type encoding of the scheme value, the `SCM`
variable has to be transformed into the corresponding representation as
a `scm_t_bits` variable y by using the `SCM_UNPACK`
macro. Once this has been done, the type of the scheme object x
can be derived from the content of the bits of the `scm_t_bits`
value y, in the way illustrated by the example earlier in this
chapter (see [Cheaper Pairs](#Cheaper-Pairs)). Conversely, a valid bit encoding of a
Scheme value as a `scm_t_bits` variable can be transformed into the
corresponding `SCM` value using the `SCM_PACK` macro.

---

Next: [Non-Immediate Objects](#Non_002dImmediate-Objects), Previous: [Relationship Between `SCM` and `scm_t_bits`](#Relationship-Between-SCM-and-scm_005ft_005fbits), Up: [The SCM Type in Guile](#The-SCM-Type-in-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.5.2 Immediate Objects [¶](#Immediate-Objects-1)

A Scheme object may either be an immediate, i.e. carrying all
necessary information by itself, or it may contain a reference to a
*heap object* which is, as the name implies, data on the heap.
Although in general it should be irrelevant for user code whether an
object is an immediate or not, within Guile’s own code the distinction
is sometimes of importance. Thus, the following low level macro is
provided:

Macro: `int` **SCM\_IMP** `(SCM x)` [¶](#index-SCM_005fIMP)
:   A Scheme object is an immediate if it fulfills the `SCM_IMP`
    predicate, otherwise it holds an encoded reference to a heap object. The
    result of the predicate is delivered as a C style boolean value. User
    code and code that extends Guile should normally not be required to use
    this macro.

Summary:

* Given a Scheme object x of unknown type, check first
  with `SCM_IMP (x)` if it is an immediate object.
* If so, all of the type and value information can be determined from the
  `scm_t_bits` value that is delivered by `SCM_UNPACK
  (x)`.

There are a number of special values in Scheme, most of them documented
elsewhere in this manual. It’s not quite the right place to put them,
but for now, here’s a list of the C names given to some of these values:

Macro: `SCM` **SCM\_EOL** [¶](#index-SCM_005fEOL)
:   The Scheme empty list object, or “End Of List” object, usually written
    in Scheme as `'()`.

Macro: `SCM` **SCM\_EOF\_VAL** [¶](#index-SCM_005fEOF_005fVAL)
:   The Scheme end-of-file value. It has no standard written
    representation, for obvious reasons.

Macro: `SCM` **SCM\_UNSPECIFIED** [¶](#index-SCM_005fUNSPECIFIED)
:   The value returned by some (but not all) expressions that the Scheme
    standard says return an “unspecified” value.

    This is sort of a weirdly literal way to take things, but the standard
    read-eval-print loop prints nothing when the expression returns this
    value, so it’s not a bad idea to return this when you can’t think of
    anything else helpful.

Macro: `SCM` **SCM\_UNDEFINED** [¶](#index-SCM_005fUNDEFINED)
:   The “undefined” value. Its most important property is that is not
    equal to any valid Scheme value. This is put to various internal uses
    by C code interacting with Guile.

    For example, when you write a C function that is callable from Scheme
    and which takes optional arguments, the interpreter passes
    `SCM_UNDEFINED` for any arguments you did not receive.

    We also use this to mark unbound variables.

Macro: `int` **SCM\_UNBNDP** `(SCM x)` [¶](#index-SCM_005fUNBNDP)
:   Return true if x is `SCM_UNDEFINED`. Note that this is not a
    check to see if x is `SCM_UNBOUND`. History will not be kind
    to us.

---

Next: [Allocating Heap Objects](#Allocating-Heap-Objects), Previous: [Immediate Objects](#Immediate-Objects), Up: [The SCM Type in Guile](#The-SCM-Type-in-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.5.3 Non-Immediate Objects [¶](#Non_002dImmediate-Objects-1)

A Scheme object of type `SCM` that does not fulfill the
`SCM_IMP` predicate holds an encoded reference to a heap object.
This reference can be decoded to a C pointer to a heap object using the
`SCM_UNPACK_POINTER` macro. The encoding of a pointer to a heap
object into a `SCM` value is done using the `SCM_PACK_POINTER`
macro.

Before Guile 2.0, Guile had a custom garbage collector that allocated
heap objects in units of 2-word *cells*. With the move to the
BDW-GC collector in Guile 2.0, Guile can allocate heap objects of any
size, and the concept of a cell is now obsolete. Still, we mention
it here as the name still appears in various low-level interfaces.

Macro: `scm_t_bits *` **SCM\_UNPACK\_POINTER** `(SCM x)` [¶](#index-SCM_005fUNPACK_005fPOINTER)

Macro: `scm_t_cell *` **SCM2PTR** `(SCM x)` [¶](#index-SCM2PTR)
:   Extract and return the heap object pointer from a non-immediate
    `SCM` object x. The name `SCM2PTR` is deprecated but
    still common.

Macro: `SCM_PACK_POINTER` **(scm\_t\_bits** `* x)` [¶](#index-_0028scm_005ft_005fbits)

Macro: `SCM` **PTR2SCM** `(scm_t_cell * x)` [¶](#index-PTR2SCM)
:   Return a `SCM` value that encodes a reference to the heap object
    pointer x. The name `PTR2SCM` is deprecated but still
    common.

Note that it is also possible to transform a non-immediate `SCM`
value by using `SCM_UNPACK` into a `scm_t_bits` variable.
However, the result of `SCM_UNPACK` may not be used as a pointer to
a heap object: only `SCM_UNPACK_POINTER` is guaranteed to transform
a `SCM` object into a valid pointer to a heap object. Also, it is
not allowed to apply `SCM_PACK_POINTER` to anything that is not a
valid pointer to a heap object.

Summary:

* Only use `SCM_UNPACK_POINTER` on `SCM` values for which
  `SCM_IMP` is false!
* Don’t use `(scm_t_cell *) SCM_UNPACK (x)`! Use
  `SCM_UNPACK_POINTER (x)` instead!
* Don’t use `SCM_PACK_POINTER` for anything but a heap object pointer!

---

Next: [Heap Object Type Information](#Heap-Object-Type-Information), Previous: [Non-Immediate Objects](#Non_002dImmediate-Objects), Up: [The SCM Type in Guile](#The-SCM-Type-in-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.5.4 Allocating Heap Objects [¶](#Allocating-Heap-Objects-1)

Heap objects are heap-allocated data pointed to by non-immediate
`SCM` value. The first word of the heap object should contain a
type code. The object may be any number of words in length, and is
generally scanned by the garbage collector for additional unless the
object was allocated using a “pointerless” allocation function.

You should generally not need these functions, unless you are
implementing a new data type, and thoroughly understand the code in
`<libguile/scm.h>`.

If you just want to allocate pairs, use `scm_cons`.

Function: `SCM` **scm\_words** `(scm_t_bits word_0, uint32_t n_words)` [¶](#index-scm_005fwords)
:   Allocate a new heap object containing n\_words, and initialize the
    first slot to word\_0, and return a non-immediate `SCM` value
    encoding a pointer to the object. Typically word\_0 will contain
    the type tag.

There are also deprecated but common variants of `scm_words` that
use the term “cell” to indicate 2-word objects.

Function: `SCM` **scm\_cell** `(scm_t_bits word_0, scm_t_bits word_1)` [¶](#index-scm_005fcell)
:   Allocate a new 2-word heap object, initialize the two slots with
    word\_0 and word\_1, and return it. Just like calling
    `scm_words (word_0, 2)`, then initializing the second slot to
    word\_1.

    Note that word\_0 and word\_1 are of type `scm_t_bits`.
    If you want to pass a `SCM` object, you need to use
    `SCM_UNPACK`.

Function: `SCM` **scm\_double\_cell** `(scm_t_bits word_0, scm_t_bits word_1, scm_t_bits word_2, scm_t_bits word_3)` [¶](#index-scm_005fdouble_005fcell)
:   Like `scm_cell`, but allocates a 4-word heap object.

---

Next: [Accessing Heap Object Fields](#Accessing-Heap-Object-Fields), Previous: [Allocating Heap Objects](#Allocating-Heap-Objects), Up: [The SCM Type in Guile](#The-SCM-Type-in-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.5.5 Heap Object Type Information [¶](#Heap-Object-Type-Information-1)

Heap objects contain a type tag and are followed by a number of
word-sized slots. The interpretation of the object contents depends on
the type of the object.

Macro: `scm_t_bits` **SCM\_CELL\_TYPE** `(SCM x)` [¶](#index-SCM_005fCELL_005fTYPE)
:   Extract the first word of the heap object pointed to by x. This
    value holds the information about the cell type.

Macro: `void` **SCM\_SET\_CELL\_TYPE** `(SCM x, scm_t_bits t)` [¶](#index-SCM_005fSET_005fCELL_005fTYPE)
:   For a non-immediate Scheme object x, write the value t into
    the first word of the heap object referenced by x. The value
    t must hold a valid cell type.

---

Previous: [Heap Object Type Information](#Heap-Object-Type-Information), Up: [The SCM Type in Guile](#The-SCM-Type-in-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.2.5.6 Accessing Heap Object Fields [¶](#Accessing-Heap-Object-Fields-1)

For a non-immediate Scheme object x, the object type can be
determined by using the `SCM_CELL_TYPE` macro described in the
previous section. For each different type of heap object it is known
which fields hold tagged Scheme objects and which fields hold untagged
raw data. To access the different fields appropriately, the following
macros are provided.

Macro: `scm_t_bits` **SCM\_CELL\_WORD** `(SCM x, unsigned int n)` [¶](#index-SCM_005fCELL_005fWORD)

Macro: `scm_t_bits` **SCM\_CELL\_WORD\_0** `(x)` [¶](#index-SCM_005fCELL_005fWORD_005f0)

Macro: `scm_t_bits` **SCM\_CELL\_WORD\_1** `(x)` [¶](#index-SCM_005fCELL_005fWORD_005f1)

Macro: `scm_t_bits` **SCM\_CELL\_WORD\_2** `(x)` [¶](#index-SCM_005fCELL_005fWORD_005f2)

Macro: `scm_t_bits` **SCM\_CELL\_WORD\_3** `(x)` [¶](#index-SCM_005fCELL_005fWORD_005f3)
:   Deliver the field n of the heap object referenced by the
    non-immediate Scheme object x as raw untagged data. Only use this
    macro for fields containing untagged data; don’t use it for fields
    containing tagged `SCM` objects.

Macro: `SCM` **SCM\_CELL\_OBJECT** `(SCM x, unsigned int n)` [¶](#index-SCM_005fCELL_005fOBJECT)

Macro: `SCM` **SCM\_CELL\_OBJECT\_0** `(SCM x)` [¶](#index-SCM_005fCELL_005fOBJECT_005f0)

Macro: `SCM` **SCM\_CELL\_OBJECT\_1** `(SCM x)` [¶](#index-SCM_005fCELL_005fOBJECT_005f1)

Macro: `SCM` **SCM\_CELL\_OBJECT\_2** `(SCM x)` [¶](#index-SCM_005fCELL_005fOBJECT_005f2)

Macro: `SCM` **SCM\_CELL\_OBJECT\_3** `(SCM x)` [¶](#index-SCM_005fCELL_005fOBJECT_005f3)
:   Deliver the field n of the heap object referenced by the
    non-immediate Scheme object x as a Scheme object. Only use this
    macro for fields containing tagged `SCM` objects; don’t use it for
    fields containing untagged data.

Macro: `void` **SCM\_SET\_CELL\_WORD** `(SCM x, unsigned int n, scm_t_bits w)` [¶](#index-SCM_005fSET_005fCELL_005fWORD)

Macro: `void` **SCM\_SET\_CELL\_WORD\_0** `(x, w)` [¶](#index-SCM_005fSET_005fCELL_005fWORD_005f0)

Macro: `void` **SCM\_SET\_CELL\_WORD\_1** `(x, w)` [¶](#index-SCM_005fSET_005fCELL_005fWORD_005f1)

Macro: `void` **SCM\_SET\_CELL\_WORD\_2** `(x, w)` [¶](#index-SCM_005fSET_005fCELL_005fWORD_005f2)

Macro: `void` **SCM\_SET\_CELL\_WORD\_3** `(x, w)` [¶](#index-SCM_005fSET_005fCELL_005fWORD_005f3)
:   Write the raw value w into field number n of the heap object
    referenced by the non-immediate Scheme value x. Values that are
    written into heap objects as raw values should only be read later using
    the `SCM_CELL_WORD` macros.

Macro: `void` **SCM\_SET\_CELL\_OBJECT** `(SCM x, unsigned int n, SCM o)` [¶](#index-SCM_005fSET_005fCELL_005fOBJECT)

Macro: `void` **SCM\_SET\_CELL\_OBJECT\_0** `(SCM x, SCM o)` [¶](#index-SCM_005fSET_005fCELL_005fOBJECT_005f0)

Macro: `void` **SCM\_SET\_CELL\_OBJECT\_1** `(SCM x, SCM o)` [¶](#index-SCM_005fSET_005fCELL_005fOBJECT_005f1)

Macro: `void` **SCM\_SET\_CELL\_OBJECT\_2** `(SCM x, SCM o)` [¶](#index-SCM_005fSET_005fCELL_005fOBJECT_005f2)

Macro: `void` **SCM\_SET\_CELL\_OBJECT\_3** `(SCM x, SCM o)` [¶](#index-SCM_005fSET_005fCELL_005fOBJECT_005f3)
:   Write the Scheme object o into field number n of the heap
    object referenced by the non-immediate Scheme value x. Values
    that are written into heap objects as objects should only be read using
    the `SCM_CELL_OBJECT` macros.

Summary:

* For a non-immediate Scheme object x of unknown type, get the type
  information by using `SCM_CELL_TYPE (x)`.
* As soon as the type information is available, only use the appropriate
  access methods to read and write data to the different heap object
  fields.
* Note that field 0 stores the cell type information. Generally speaking,
  other data associated with a heap object is stored starting from field
  1.

---

Next: [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine), Previous: [Data Representation](#Data-Representation), Up: [Guile Implementation](#Guile-Implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
