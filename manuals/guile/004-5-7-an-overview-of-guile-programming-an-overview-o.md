### 5.7 An Overview of Guile Programming [¶](#An-Overview-of-Guile-Programming)

Guile is designed as an extension language interpreter that is
straightforward to integrate with applications written in C (and C++).
The big win here for the application developer is that Guile
integration, as the Guile web page says, “lowers your project’s
hacktivation energy.” Lowering the hacktivation energy means that you,
as the application developer, *and your users*, reap the benefits
that flow from being able to extend the application in a high level
extension language rather than in plain old C.

In abstract terms, it’s difficult to explain what this really means and
what the integration process involves, so instead let’s begin by jumping
straight into an example of how you might integrate Guile into an
existing program, and what you could expect to gain by so doing. With
that example under our belts, we’ll then return to a more general
analysis of the arguments involved and the range of programming options
available.

* [How One Might Extend Dia Using Guile](#Extending-Dia)
* [Why Scheme is More Hackable Than C](#Scheme-vs-C)
* [Example: Using Guile for an Application Testbed](#Testbed-Example)
* [A Choice of Programming Options](#Programming-Options)
* [How About Application Users?](#User-Programming)

---

Next: [Why Scheme is More Hackable Than C](#Scheme-vs-C), Up: [An Overview of Guile Programming](#Programming-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.1 How One Might Extend Dia Using Guile [¶](#How-One-Might-Extend-Dia-Using-Guile)

Dia is a free software program for drawing schematic diagrams like flow
charts and floor plans (<http://www.gnome.org/projects/dia/>).
This section conducts the thought
experiment of adding Guile to Dia. In so doing, it aims to illustrate
several of the steps and considerations involved in adding Guile to
applications in general.

* [Deciding Why You Want to Add Guile](#Dia-Objective)
* [Four Steps Required to Add Guile](#Dia-Steps)
* [How to Represent Dia Data in Scheme](#Dia-Objects)
* [Writing Guile Primitives for Dia](#Dia-Primitives)
* [Providing a Hook for the Evaluation of Scheme Code](#Dia-Hook)
* [Top-level Structure of Guile-enabled Dia](#Dia-Structure)
* [Going Further with Dia and Guile](#Dia-Advanced)

---

Next: [Four Steps Required to Add Guile](#Dia-Steps), Up: [How One Might Extend Dia Using Guile](#Extending-Dia)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.1.1 Deciding Why You Want to Add Guile [¶](#Deciding-Why-You-Want-to-Add-Guile)

First off, you should understand why you want to add Guile to Dia at
all, and that means forming a picture of what Dia does and how it does
it. So, what are the constituents of the Dia application?

* Most importantly, the *application domain objects* — in other
  words, the concepts that differentiate Dia from another application such
  as a word processor or spreadsheet: shapes, templates, connectors,
  pages, plus the properties of all these things.
* The code that manages the graphical face of the application, including
  the layout and display of the objects above.
* The code that handles input events, which indicate that the application
  user is wanting to do something.

(In other words, a textbook example of the *model - view -
controller* paradigm.)

Next question: how will Dia benefit once the Guile integration is
complete? Several (positive!) answers are possible here, and the choice
is obviously up to the application developers. Still, one answer is
that the main benefit will be the ability to manipulate Dia’s
application domain objects from Scheme.

Suppose that Dia made a set of procedures available in Scheme,
representing the most basic operations on objects such as shapes,
connectors, and so on. Using Scheme, the application user could then
write code that builds upon these basic operations to create more
complex procedures. For example, given basic procedures to enumerate
the objects on a page, to determine whether an object is a square, and
to change the fill pattern of a single shape, the user can write a
Scheme procedure to change the fill pattern of all squares on the
current page:

```
(define (change-squares'-fill-pattern new-pattern)
  (for-each-shape current-page
    (lambda (shape)
      (if (square? shape)
          (change-fill-pattern shape new-pattern)))))
```

---

Next: [How to Represent Dia Data in Scheme](#Dia-Objects), Previous: [Deciding Why You Want to Add Guile](#Dia-Objective), Up: [How One Might Extend Dia Using Guile](#Extending-Dia)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.1.2 Four Steps Required to Add Guile [¶](#Four-Steps-Required-to-Add-Guile)

Assuming this objective, four steps are needed to achieve it.

First, you need a way of representing your application-specific objects
— such as `shape` in the previous example — when they are
passed into the Scheme world. Unless your objects are so simple that
they map naturally into builtin Scheme data types like numbers and
strings, you will probably want to use Guile’s *foreign object*
interface to create a new Scheme data type for your objects.

Second, you need to write code for the basic operations like
`for-each-shape` and `square?` such that they access and
manipulate your existing data structures correctly, and then make these
operations available as *primitives* on the Scheme level.

Third, you need to provide some mechanism within the Dia application
that a user can hook into to cause arbitrary Scheme code to be
evaluated.

Finally, you need to restructure your top-level application C code a
little so that it initializes the Guile interpreter correctly and
declares your *foreign objects* and *primitives* to the Scheme
world.

The following subsections expand on these four points in turn.

---

Next: [Writing Guile Primitives for Dia](#Dia-Primitives), Previous: [Four Steps Required to Add Guile](#Dia-Steps), Up: [How One Might Extend Dia Using Guile](#Extending-Dia)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.1.3 How to Represent Dia Data in Scheme [¶](#How-to-Represent-Dia-Data-in-Scheme)

For all but the most trivial applications, you will probably want to
allow some representation of your domain objects to exist on the Scheme
level. This is where foreign objects come in, and with them issues of
lifetime management and garbage collection.

To get more concrete about this, let’s look again at the example we gave
earlier of how application users can use Guile to build higher-level
functions from the primitives that Dia itself provides.

```
(define (change-squares'-fill-pattern new-pattern)
  (for-each-shape current-page
    (lambda (shape)
      (if (square? shape)
          (change-fill-pattern shape new-pattern)))))
```

Consider what is stored here in the variable `shape`. For each
shape on the current page, the `for-each-shape` primitive calls
`(lambda (shape) …)` with an argument representing that
shape. Question is: how is that argument represented on the Scheme
level? The issues are as follows.

* Whatever the representation, it has to be decodable again by the C code
  for the `square?` and `change-fill-pattern` primitives. In
  other words, a primitive like `square?` has somehow to be able to
  turn the value that it receives back into something that points to the
  underlying C structure describing a shape.
* The representation must also cope with Scheme code holding on to the
  value for later use. What happens if the Scheme code stores
  `shape` in a global variable, but then that shape is deleted (in a
  way that the Scheme code is not aware of), and later on some other
  Scheme code uses that global variable again in a call to, say,
  `square?`?
* The lifetime and memory allocation of objects that exist *only* in
  the Scheme world is managed automatically by Guile’s garbage collector
  using one simple rule: when there are no remaining references to an
  object, the object is considered dead and so its memory is freed. But
  for objects that exist in both C and Scheme, the picture is more
  complicated; in the case of Dia, where the `shape` argument passes
  transiently in and out of the Scheme world, it would be quite wrong the
  **delete** the underlying C shape just because the Scheme code has
  finished evaluation. How do we avoid this happening?

One resolution of these issues is for the Scheme-level representation of
a shape to be a new, Scheme-specific C structure wrapped up as a foreign
object. The foreign object is what is passed into and out of Scheme
code, and the Scheme-specific C structure inside the foreign object
points to Dia’s underlying C structure so that the code for primitives
like `square?` can get at it.

To cope with an underlying shape being deleted while Scheme code is
still holding onto a Scheme shape value, the underlying C structure
should have a new field that points to the Scheme-specific foreign
object. When a shape is deleted, the relevant code chains through to
the Scheme-specific structure and sets its pointer back to the
underlying structure to NULL. Thus the foreign object value for the
shape continues to exist, but any primitive code that tries to use it
will detect that the underlying shape has been deleted because the
underlying structure pointer is NULL.

So, to summarize the steps involved in this resolution of the problem
(and assuming that the underlying C structure for a shape is
`struct dia_shape`):

* Define a new Scheme-specific structure that *points* to the
  underlying C structure:

  ```
  struct dia_guile_shape
  {
    struct dia_shape * c_shape;   /* NULL => deleted */
  }
  ```
* Add a field to `struct dia_shape` that points to its `struct
  dia_guile_shape` if it has one —

  ```
  struct dia_shape
  {
    ...
    struct dia_guile_shape * guile_shape;
  }
  ```

  — so that C code can set `guile_shape->c_shape` to NULL when the
  underlying shape is deleted.
* Wrap `struct dia_guile_shape` as a foreign object type.
* Whenever you need to represent a C shape onto the Scheme level, create a
  foreign object instance for it, and pass that.
* In primitive code that receives a shape foreign object instance, check the
  `c_shape` field when decoding it, to find out whether the
  underlying C shape is still there.

As far as memory management is concerned, the foreign object values and
their Scheme-specific structures are under the control of the garbage
collector, whereas the underlying C structures are explicitly managed in
exactly the same way that Dia managed them before we thought of adding
Guile.

When the garbage collector decides to free a shape foreign object value,
it calls the *finalizer* function that was specified when defining
the shape foreign object type. To maintain the correctness of the
`guile_shape` field in the underlying C structure, this function
should chain through to the underlying C structure (if it still exists)
and set its `guile_shape` field to NULL.

For full documentation on defining and using foreign object types, see
[Defining New Foreign Object Types](#Defining-New-Foreign-Object-Types).

---

Next: [Providing a Hook for the Evaluation of Scheme Code](#Dia-Hook), Previous: [How to Represent Dia Data in Scheme](#Dia-Objects), Up: [How One Might Extend Dia Using Guile](#Extending-Dia)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.1.4 Writing Guile Primitives for Dia [¶](#Writing-Guile-Primitives-for-Dia)

Once the details of object representation are decided, writing the
primitive function code that you need is usually straightforward.

A primitive is simply a C function whose arguments and return value are
all of type `SCM`, and whose body does whatever you want it to do.
As an example, here is a possible implementation of the `square?`
primitive:

```
static SCM square_p (SCM shape)
{
  struct dia_guile_shape * guile_shape;

  /* Check that arg is really a shape object. */
  scm_assert_foreign_object_type (shape_type, shape);

  /* Access Scheme-specific shape structure. */
  guile_shape = scm_foreign_object_ref (shape, 0);

  /* Find out if underlying shape exists and is a
     square; return answer as a Scheme boolean. */
  return scm_from_bool (guile_shape->c_shape &&
                        (guile_shape->c_shape->type == DIA_SQUARE));
}
```

Notice how easy it is to chain through from the `SCM shape`
parameter that `square_p` receives — which is a foreign object
— to the Scheme-specific structure inside the foreign object, and
thence to the underlying C structure for the shape.

In this code, `scm_assert_foreign_object_type`,
`scm_foreign_object_ref`, and `scm_from_bool` are from the
standard Guile API. We assume that `shape_type` was given to us
when we made the shape foreign object type, using
`scm_make_foreign_object_type`. The call to
`scm_assert_foreign_object_type` ensures that shape is indeed
a shape. This is needed to guard against Scheme code using the
`square?` procedure incorrectly, as in `(square? "hello")`;
Scheme’s latent typing means that usage errors like this must be caught
at run time.

Having written the C code for your primitives, you need to make them
available as Scheme procedures by calling the `scm_c_define_gsubr`
function. `scm_c_define_gsubr` (see [Primitive Procedures](#Primitive-Procedures))
takes arguments that specify the Scheme-level name for the primitive and
how many required, optional and rest arguments it can accept. The
`square?` primitive always requires exactly one argument, so the
call to make it available in Scheme reads like this:

```
scm_c_define_gsubr ("square?", 1, 0, 0, square_p);
```

For where to put this call, see the subsection after next on the
structure of Guile-enabled code (see [Top-level Structure of Guile-enabled Dia](#Dia-Structure)).

---

Next: [Top-level Structure of Guile-enabled Dia](#Dia-Structure), Previous: [Writing Guile Primitives for Dia](#Dia-Primitives), Up: [How One Might Extend Dia Using Guile](#Extending-Dia)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.1.5 Providing a Hook for the Evaluation of Scheme Code [¶](#Providing-a-Hook-for-the-Evaluation-of-Scheme-Code)

To make the Guile integration useful, you have to design some kind of
hook into your application that application users can use to cause their
Scheme code to be evaluated.

Technically, this is straightforward; you just have to decide on a
mechanism that is appropriate for your application. Think of Emacs, for
example: when you type `ESC :`, you get a prompt where you can
type in any Elisp code, which Emacs will then evaluate. Or, again like
Emacs, you could provide a mechanism (such as an init file) to allow
Scheme code to be associated with a particular key sequence, and
evaluate the code when that key sequence is entered.

In either case, once you have the Scheme code that you want to evaluate,
as a null terminated string, you can tell Guile to evaluate it by
calling the `scm_c_eval_string` function.

---

Next: [Going Further with Dia and Guile](#Dia-Advanced), Previous: [Providing a Hook for the Evaluation of Scheme Code](#Dia-Hook), Up: [How One Might Extend Dia Using Guile](#Extending-Dia)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.1.6 Top-level Structure of Guile-enabled Dia [¶](#Top_002dlevel-Structure-of-Guile_002denabled-Dia)

Let’s assume that the pre-Guile Dia code looks structurally like this:

* `main ()`
  + do lots of initialization and setup stuff
  + enter Gtk main loop

When you add Guile to a program, one (rather technical) requirement is
that Guile’s garbage collector needs to know where the bottom of the C
stack is. The easiest way to ensure this is to use
`scm_boot_guile` like this:

* `main ()`
  + do lots of initialization and setup stuff
  + `scm_boot_guile (argc, argv, inner_main, NULL)`
* `inner_main ()`
  + define all foreign object types
  + export primitives to Scheme using `scm_c_define_gsubr`
  + enter Gtk main loop

In other words, you move the guts of what was previously in your
`main` function into a new function called `inner_main`, and
then add a `scm_boot_guile` call, with `inner_main` as a
parameter, to the end of `main`.

Assuming that you are using foreign objects and have written primitive
code as described in the preceding subsections, you also need to insert
calls to declare your new foreign objects and export the primitives to
Scheme. These declarations must happen *inside* the dynamic scope
of the `scm_boot_guile` call, but also *before* any code is
run that could possibly use them — the beginning of `inner_main`
is an ideal place for this.

---

Previous: [Top-level Structure of Guile-enabled Dia](#Dia-Structure), Up: [How One Might Extend Dia Using Guile](#Extending-Dia)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.1.7 Going Further with Dia and Guile [¶](#Going-Further-with-Dia-and-Guile)

The steps described so far implement an initial Guile integration that
already gives a lot of additional power to Dia application users. But
there are further steps that you could take, and it’s interesting to
consider a few of these.

In general, you could progressively move more of Dia’s source code from
C into Scheme. This might make the code more maintainable and
extensible, and it could open the door to new programming paradigms that
are tricky to effect in C but straightforward in Scheme.

A specific example of this is that you could use the guile-gtk package,
which provides Scheme-level procedures for most of the Gtk+ library, to
move the code that lays out and displays Dia objects from C to Scheme.

As you follow this path, it naturally becomes less useful to maintain a
distinction between Dia’s original non-Guile-related source code, and
its later code implementing foreign objects and primitives for the
Scheme world.

For example, suppose that the original source code had a
`dia_change_fill_pattern` function:

```
void dia_change_fill_pattern (struct dia_shape * shape,
                              struct dia_pattern * pattern)
{
  /* real pattern change work */
}
```

During initial Guile integration, you add a `change_fill_pattern`
primitive for Scheme purposes, which accesses the underlying structures
from its foreign object values and uses `dia_change_fill_pattern`
to do the real work:

```
SCM change_fill_pattern (SCM shape, SCM pattern)
{
  struct dia_shape * d_shape;
  struct dia_pattern * d_pattern;

  ...

  dia_change_fill_pattern (d_shape, d_pattern);

  return SCM_UNSPECIFIED;
}
```

At this point, it makes sense to keep `dia_change_fill_pattern` and
`change_fill_pattern` separate, because
`dia_change_fill_pattern` can also be called without going through
Scheme at all, say because the user clicks a button which causes a
C-registered Gtk+ callback to be called.

But, if the code for creating buttons and registering their callbacks is
moved into Scheme (using guile-gtk), it may become true that
`dia_change_fill_pattern` can no longer be called other than
through Scheme. In which case, it makes sense to abolish it and move
its contents directly into `change_fill_pattern`, like this:

```
SCM change_fill_pattern (SCM shape, SCM pattern)
{
  struct dia_shape * d_shape;
  struct dia_pattern * d_pattern;

  ...

  /* real pattern change work */

  return SCM_UNSPECIFIED;
}
```

So further Guile integration progressively *reduces* the amount of
functional C code that you have to maintain over the long term.

A similar argument applies to data representation. In the discussion of
foreign objects earlier, issues arose because of the different memory
management and lifetime models that normally apply to data structures in
C and in Scheme. However, with further Guile integration, you can
resolve this issue in a more radical way by allowing all your data
structures to be under the control of the garbage collector, and kept
alive by references from the Scheme world. Instead of maintaining an
array or linked list of shapes in C, you would instead maintain a list
in Scheme.

Rather like the coalescing of `dia_change_fill_pattern` and
`change_fill_pattern`, the practical upshot of such a change is
that you would no longer have to keep the `dia_shape` and
`dia_guile_shape` structures separate, and so wouldn’t need to
worry about the pointers between them. Instead, you could change the
foreign object definition to wrap the `dia_shape` structure
directly, and send `dia_guile_shape` off to the scrap yard. Cut
out the middle man!

Finally, we come to the holy grail of Guile’s free software / extension
language approach. Once you have a Scheme representation for
interesting Dia data types like shapes, and a handy bunch of primitives
for manipulating them, it suddenly becomes clear that you have a bundle
of functionality that could have far-ranging use beyond Dia itself. In
other words, the data types and primitives could now become a library,
and Dia becomes just one of the many possible applications using that
library — albeit, at this early stage, a rather important one!

In this model, Guile becomes just the glue that binds everything
together. Imagine an application that usefully combined functionality
from Dia, Gnumeric and GnuCash — it’s tricky right now, because no
such application yet exists; but it’ll happen some day …

---

Next: [Example: Using Guile for an Application Testbed](#Testbed-Example), Previous: [How One Might Extend Dia Using Guile](#Extending-Dia), Up: [An Overview of Guile Programming](#Programming-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.2 Why Scheme is More Hackable Than C [¶](#Why-Scheme-is-More-Hackable-Than-C)

Underlying Guile’s value proposition is the assumption that programming
in a high level language, specifically Guile’s implementation of Scheme,
is necessarily better in some way than programming in C. What do we
mean by this claim, and how can we be so sure?

One class of advantages applies not only to Scheme, but more generally
to any interpretable, high level, scripting language, such as Emacs
Lisp, Python, Ruby, or TeX’s macro language. Common features of all
such languages, when compared to C, are that:

* They lend themselves to rapid and experimental development cycles,
  owing usually to a combination of their interpretability and the
  integrated development environment in which they are used.
* They free developers from some of the low level bookkeeping tasks
  associated with C programming, notably memory management.
* They provide high level features such as container objects and exception
  handling that make common programming tasks easier.

In the case of Scheme, particular features that make programming easier
— and more fun! — are its powerful mechanisms for abstracting parts
of programs (closures — see [The Concept of Closure](#About-Closure)) and for iteration
(see [Iteration mechanisms](#while-do)).

The evidence in support of this argument is empirical: the huge amount
of code that has been written in extension languages for applications
that support this mechanism. Most notable are extensions written in
Emacs Lisp for GNU Emacs, in TeX’s macro language for TeX, and in
Script-Fu for the Gimp, but there is increasingly now a significant code
eco-system for Guile-based applications as well, such as Lilypond and
GnuCash. It is close to inconceivable that similar amounts of
functionality could have been added to these applications just by
writing new code in their base implementation languages.

---

Next: [A Choice of Programming Options](#Programming-Options), Previous: [Why Scheme is More Hackable Than C](#Scheme-vs-C), Up: [An Overview of Guile Programming](#Programming-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.3 Example: Using Guile for an Application Testbed [¶](#Example_003a-Using-Guile-for-an-Application-Testbed)

As an example of what this means in practice, imagine writing a testbed
for an application that is tested by submitting various requests (via a
C interface) and validating the output received. Suppose further that
the application keeps an idea of its current state, and that the
“correct” output for a given request may depend on the current
application state. A complete “white box”[7](#FOOT7) test plan for this application would aim to
submit all possible requests in each distinguishable state, and validate
the output for all request/state combinations.

To write all this test code in C would be very tedious. Suppose instead
that the testbed code adds a single new C function, to submit an
arbitrary request and return the response, and then uses Guile to export
this function as a Scheme procedure. The rest of the testbed can then
be written in Scheme, and so benefits from all the advantages of
programming in Scheme that were described in the previous section.

(In this particular example, there is an additional benefit of writing
most of the testbed in Scheme. A common problem for white box testing
is that mistakes and mistaken assumptions in the application under test
can easily be reproduced in the testbed code. It is more difficult to
copy mistakes like this when the testbed is written in a different
language from the application.)

---

Next: [How About Application Users?](#User-Programming), Previous: [Example: Using Guile for an Application Testbed](#Testbed-Example), Up: [An Overview of Guile Programming](#Programming-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.4 A Choice of Programming Options [¶](#A-Choice-of-Programming-Options)

The preceding arguments and example point to a model of Guile
programming that is applicable in many cases. According to this model,
Guile programming involves a balance between C and Scheme programming,
with the aim being to extract the greatest possible Scheme level benefit
from the least amount of C level work.

The C level work required in this model usually consists of packaging
and exporting functions and application objects such that they can be
seen and manipulated on the Scheme level. To help with this, Guile’s C
language interface includes utility features that aim to make this kind
of integration very easy for the application developer.

This model, though, is really just one of a range of possible
programming options. If all of the functionality that you need is
available from Scheme, you could choose instead to write your whole
application in Scheme (or one of the other high level languages that
Guile supports through translation), and simply use Guile as an
interpreter for Scheme. (In the future, we hope that Guile will also be
able to compile Scheme code, so lessening the performance gap between C
and Scheme code.) Or, at the other end of the C–Scheme scale, you
could write the majority of your application in C, and only call out to
Guile occasionally for specific actions such as reading a configuration
file or executing a user-specified extension. The choices boil down to
two basic questions:

* Which parts of the application do you write in C, and which in Scheme
  (or another high level translated language)?
* How do you design the interface between the C and Scheme parts of your
  application?

These are of course design questions, and the right design for any given
application will always depend upon the particular requirements that you
are trying to meet. In the context of Guile, however, there are some
generally applicable considerations that can help you when designing
your answers.

* [What Functionality is Already Available?](#Available-Functionality)
* [Functional and Performance Constraints](#Basic-Constraints)
* [Your Preferred Programming Style](#Style-Choices)
* [What Controls Program Execution?](#Program-Control)

---

Next: [Functional and Performance Constraints](#Basic-Constraints), Up: [A Choice of Programming Options](#Programming-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.4.1 What Functionality is Already Available? [¶](#What-Functionality-is-Already-Available_003f)

Suppose, for the sake of argument, that you would prefer to write your
whole application in Scheme. Then the API available to you consists of:

* standard Scheme
* plus the extensions to standard Scheme provided by
  Guile in its core distribution
* plus any additional functionality that you or others have packaged so
  that it can be loaded as a Guile Scheme module.

A module in the last category can either be a pure Scheme module — in
other words a collection of utility procedures coded in Scheme — or a
module that provides a Scheme interface to an extension library coded in
C — in other words a nice package where someone else has done the work
of wrapping up some useful C code for you. The set of available modules
is growing quickly and already includes such useful examples as
`(gtk gtk)`, which makes Gtk+ drawing functions available in
Scheme, and `(database postgres)`, which provides SQL access to a
Postgres database.

Given the growing collection of pre-existing modules, it is quite
feasible that your application could be implemented by combining a
selection of these modules together with new application code written in
Scheme.

If this approach is not enough, because the functionality that your
application needs is not already available in this form, and it is
impossible to write the new functionality in Scheme, you will need to
write some C code. If the required function is already available in C
(e.g. in a library), all you need is a little glue to connect it to the
world of Guile. If not, you need both to write the basic code and to
plumb it into Guile.

In either case, two general considerations are important. Firstly, what
is the interface by which the functionality is presented to the Scheme
world? Does the interface consist only of function calls (for example,
a simple drawing interface), or does it need to include *objects* of
some kind that can be passed between C and Scheme and manipulated by
both worlds. Secondly, how does the lifetime and memory management of
objects in the C code relate to the garbage collection governed approach
of Scheme objects? In the case where the basic C code is not already
written, most of the difficulties of memory management can be avoided by
using Guile’s C interface features from the start.

For the full documentation on writing C code for Guile and connecting
existing C code to the Guile world, see [Defining New Foreign Object Types](#Defining-New-Foreign-Object-Types), [Primitive Procedures](#Primitive-Procedures), and [Foreign Function Interface](#Foreign-Function-Interface).

---

Next: [Your Preferred Programming Style](#Style-Choices), Previous: [What Functionality is Already Available?](#Available-Functionality), Up: [A Choice of Programming Options](#Programming-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.4.2 Functional and Performance Constraints [¶](#Functional-and-Performance-Constraints)

---

Next: [What Controls Program Execution?](#Program-Control), Previous: [Functional and Performance Constraints](#Basic-Constraints), Up: [A Choice of Programming Options](#Programming-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.4.3 Your Preferred Programming Style [¶](#Your-Preferred-Programming-Style)

---

Previous: [Your Preferred Programming Style](#Style-Choices), Up: [A Choice of Programming Options](#Programming-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.4.4 What Controls Program Execution? [¶](#What-Controls-Program-Execution_003f)

---

Previous: [A Choice of Programming Options](#Programming-Options), Up: [An Overview of Guile Programming](#Programming-Overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.7.5 How About Application Users? [¶](#How-About-Application-Users_003f)

So far we have considered what Guile programming means for an
application developer. But what if you are instead *using* an
existing Guile-based application, and want to know what your
options are for programming and extending this application?

The answer to this question varies from one application to another,
because the options available depend inevitably on whether the
application developer has provided any hooks for you to hang your own
code on and, if there are such hooks, what they allow you to
do.[8](#FOOT8) For example…

* If the application permits you to load and execute any Guile code, the
  world is your oyster. You can extend the application in any way that
  you choose.
* A more cautious application might allow you to load and execute Guile
  code, but only in a *safe* environment, where the interface
  available is restricted by the application from the standard Guile API.
* Or a really fearful application might not provide a hook to really
  execute user code at all, but just use Scheme syntax as a convenient way
  for users to specify application data or configuration options.

In the last two cases, what you can do is, by definition, restricted by
the application, and you should refer to the application’s own manual to
find out your options.

The most well known example of the first case is Emacs, with its
extension language Emacs Lisp: as well as being a text editor, Emacs
supports the loading and execution of arbitrary Emacs Lisp code. The
result of such openness has been dramatic: Emacs now benefits from
user-contributed Emacs Lisp libraries that extend the basic editing
function to do everything from reading news to psychoanalysis and
playing adventure games. The only limitation is that extensions are
restricted to the functionality provided by Emacs’s built-in set of
primitive operations. For example, you can interact and display data by
manipulating the contents of an Emacs buffer, but you can’t pop-up and
draw a window with a layout that is totally different to the Emacs
standard.

This situation with a Guile application that supports the loading of
arbitrary user code is similar, except perhaps even more so, because
Guile also supports the loading of extension libraries written in C.
This last point enables user code to add new primitive operations to
Guile, and so to bypass the limitation present in Emacs Lisp.

At this point, the distinction between an application developer and an
application user becomes rather blurred. Instead of seeing yourself as
a user extending an application, you could equally well say that you are
developing a new application of your own using some of the primitive
functionality provided by the original application. As such, all the
discussions of the preceding sections of this chapter are relevant to
how you can proceed with developing your extension.

---

Previous: [An Overview of Guile Programming](#Programming-Overview), Up: [Programming in C](#Programming-in-C)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 5.8 Autoconf Support [¶](#Autoconf-Support-1)

Autoconf, a part of the GNU build system, makes it easy for users to
build your package. This section documents Guile’s Autoconf support.

* [Autoconf Background](#Autoconf-Background)
* [Autoconf Macros](#Autoconf-Macros)
* [Using Autoconf Macros](#Using-Autoconf-Macros)

---

Next: [Autoconf Macros](#Autoconf-Macros), Up: [Autoconf Support](#Autoconf-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.8.1 Autoconf Background [¶](#Autoconf-Background-1)

As explained in the GNU Autoconf Manual, any package needs
configuration at build-time (see [Introduction](https://www.gnu.org/software/autoconf/manual/autoconf.html#Top) in The GNU
Autoconf Manual). If your package uses Guile (or uses a package that
in turn uses Guile), you probably need to know what specific Guile
features are available and details about them.

The way to do this is to write feature tests and arrange for their execution
by the `configure` script, typically by adding the tests to
`configure.ac`, and running `autoconf` to create `configure`.
Users of your package then run `configure` in the normal way.

Macros are a way to make common feature tests easy to express.
Autoconf provides a wide range of macros
(see [Existing Tests](https://www.gnu.org/software/autoconf/manual/autoconf.html#Existing-Tests) in The GNU Autoconf Manual),
and Guile installation provides Guile-specific tests in the areas of:
program detection, compilation flags reporting, and Scheme module
checks.

---

Next: [Using Autoconf Macros](#Using-Autoconf-Macros), Previous: [Autoconf Background](#Autoconf-Background), Up: [Autoconf Support](#Autoconf-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.8.2 Autoconf Macros [¶](#Autoconf-Macros-1)

As mentioned earlier in this chapter, Guile supports parallel
installation, and uses `pkg-config` to let the user choose which
version of Guile they are interested in. `pkg-config` has its own
set of Autoconf macros that are probably installed on most every
development system. The most useful of these macros is
`PKG_CHECK_MODULES`.

```
PKG_CHECK_MODULES([GUILE], [guile-3.0])
```

This example looks for Guile and sets the `GUILE_CFLAGS` and
`GUILE_LIBS` variables accordingly, or prints an error and exits if
Guile was not found.

Guile comes with additional Autoconf macros providing more information,
installed as `prefix/share/aclocal/guile.m4`. Their names
all begin with `GUILE_`.

Autoconf Macro: **GUILE\_PKG** [VERSIONS] [¶](#index-GUILE_005fPKG)
:   This macro runs the `pkg-config` tool to find development files
    for an available version of Guile.

    By default, this macro will search for the latest stable version of
    Guile (e.g. 3.0), falling back to the previous stable version
    (e.g. 2.2) if it is available. If no guile-VERSION.pc file is
    found, an error is signaled. The found version is stored in
    GUILE\_EFFECTIVE\_VERSION.

    If `GUILE_PROGS` was already invoked, this macro ensures that the
    development files have the same effective version as the Guile
    program.

    GUILE\_EFFECTIVE\_VERSION is marked for substitution, as by
    `AC_SUBST`.

Autoconf Macro: **GUILE\_FLAGS** [¶](#index-GUILE_005fFLAGS)
:   This macro runs the `pkg-config` tool to find out how to compile
    and link programs against Guile. It sets four variables:
    GUILE\_CFLAGS, GUILE\_LDFLAGS, GUILE\_LIBS, and
    GUILE\_LTLIBS.

    GUILE\_CFLAGS: flags to pass to a C or C++ compiler to build code that
    uses Guile header files. This is almost always just one or more `-I`
    flags.

    GUILE\_LDFLAGS: flags to pass to the compiler to link a program
    against Guile. This includes `-lguile-VERSION` for the
    Guile library itself, and may also include one or more `-L` flag
    to tell the compiler where to find the libraries. But it does not
    include flags that influence the program’s runtime search path for
    libraries, and will therefore lead to a program that fails to start,
    unless all necessary libraries are installed in a standard location
    such as `/usr/lib`.

    GUILE\_LIBS and GUILE\_LTLIBS: flags to pass to the compiler or to
    libtool, respectively, to link a program against Guile. It includes flags
    that augment the program’s runtime search path for libraries, so that shared
    libraries will be found at the location where they were during linking, even
    in non-standard locations. GUILE\_LIBS is to be used when linking the
    program directly with the compiler, whereas GUILE\_LTLIBS is to be used
    when linking the program is done through libtool.

    The variables are marked for substitution, as by `AC_SUBST`.

Autoconf Macro: **GUILE\_SITE\_DIR** [¶](#index-GUILE_005fSITE_005fDIR)
:   This looks for Guile’s "site" directories. The variable GUILE\_SITE will
    be set to Guile’s "site" directory for Scheme source files (usually something
    like PREFIX/share/guile/site). GUILE\_SITE\_CCACHE will be set to the
    directory for compiled Scheme files also known as `.go` files
    (usually something like
    PREFIX/lib/guile/GUILE\_EFFECTIVE\_VERSION/site-ccache).
    GUILE\_EXTENSION will be set to the directory for compiled C extensions
    (usually something like
    PREFIX/lib/guile/GUILE\_EFFECTIVE\_VERSION/extensions). The latter two
    are set to blank if the particular version of Guile does not support
    them. Note that this macro will run the macros `GUILE_PKG` and
    `GUILE_PROGS` if they have not already been run.

    The variables are marked for substitution, as by `AC_SUBST`.

Autoconf Macro: **GUILE\_PROGS** [VERSION] [¶](#index-GUILE_005fPROGS)
:   This macro looks for programs `guile` and `guild`, setting
    variables GUILE and GUILD to their paths, respectively.
    The macro will attempt to find `guile` with the suffix of
    `-X.Y`, followed by looking for it with the suffix `X.Y`, and
    then fall back to looking for `guile` with no suffix. If
    `guile` is still not found, signal an error. The suffix, if any,
    that was required to find `guile` will be used for `guild`
    as well.

    By default, this macro will search for the latest stable version of
    Guile (e.g. 3.0). x.y or x.y.z versions can be specified. If an older
    version is found, the macro will signal an error.

    The effective version of the found `guile` is set to
    GUILE\_EFFECTIVE\_VERSION. This macro ensures that the effective
    version is compatible with the result of a previous invocation of
    `GUILE_FLAGS`, if any.

    As a legacy interface, it also looks for `guile-config` and
    `guile-tools`, setting GUILE\_CONFIG and GUILE\_TOOLS.

    The variables are marked for substitution, as by `AC_SUBST`.

Autoconf Macro: **GUILE\_CHECK\_RETVAL** var check [¶](#index-GUILE_005fCHECK_005fRETVAL)
:   var is a shell variable name to be set to the return value.
    check is a Guile Scheme expression, evaluated with "$GUILE -c", and
    returning either 0 or non-#f to indicate the check passed.
    Non-0 number or #f indicates failure.
    Avoid using the character "#" since that confuses autoconf.

Autoconf Macro: **GUILE\_MODULE\_CHECK** var module featuretest description [¶](#index-GUILE_005fMODULE_005fCHECK)
:   var is a shell variable name to be set to "yes" or "no".
    module is a list of symbols, like: (ice-9 common-list).
    featuretest is an expression acceptable to GUILE\_CHECK, q.v.
    description is a present-tense verb phrase (passed to AC\_MSG\_CHECKING).

Autoconf Macro: **GUILE\_MODULE\_AVAILABLE** var module [¶](#index-GUILE_005fMODULE_005fAVAILABLE)
:   var is a shell variable name to be set to "yes" or "no".
    module is a list of symbols, like: (ice-9 common-list).

Autoconf Macro: **GUILE\_MODULE\_REQUIRED** symlist [¶](#index-GUILE_005fMODULE_005fREQUIRED)
:   symlist is a list of symbols, WITHOUT surrounding parens,
    like: ice-9 common-list.

Autoconf Macro: **GUILE\_MODULE\_EXPORTS** var module modvar [¶](#index-GUILE_005fMODULE_005fEXPORTS)
:   var is a shell variable to be set to "yes" or "no".
    module is a list of symbols, like: (ice-9 common-list).
    modvar is the Guile Scheme variable to check.

Autoconf Macro: **GUILE\_MODULE\_REQUIRED\_EXPORT** module modvar [¶](#index-GUILE_005fMODULE_005fREQUIRED_005fEXPORT)
:   module is a list of symbols, like: (ice-9 common-list).
    modvar is the Guile Scheme variable to check.

---

Previous: [Autoconf Macros](#Autoconf-Macros), Up: [Autoconf Support](#Autoconf-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 5.8.3 Using Autoconf Macros [¶](#Using-Autoconf-Macros-1)

Using the autoconf macros is straightforward: Add the macro "calls" (actually
instantiations) to `configure.ac`, run `aclocal`, and finally,
run `autoconf`. If your system doesn’t have guile.m4 installed, place
the desired macro definitions (`AC_DEFUN` forms) in `acinclude.m4`,
and `aclocal` will do the right thing.

Some of the macros can be used inside normal shell constructs: `if foo ;
then GUILE_BAZ ; fi`, but this is not guaranteed. It’s probably a good idea
to instantiate macros at top-level.

We now include two examples, one simple and one complicated.

The first example is for a package that uses libguile, and thus needs to
know how to compile and link against it. So we use
`PKG_CHECK_MODULES` to set the vars `GUILE_CFLAGS` and
`GUILE_LIBS`, which are automatically substituted in the Makefile.

```
In configure.ac:

  PKG_CHECK_MODULES([GUILE], [guile-3.0])

In Makefile.in:

  GUILE_CFLAGS  = @GUILE_CFLAGS@
  GUILE_LIBS = @GUILE_LIBS@

  myprog.o: myprog.c
          $(CC) -o $ $(GUILE_CFLAGS) $<
  myprog: myprog.o
          $(CC) -o $ $< $(GUILE_LIBS)
```

The second example is for a package of Guile Scheme modules that uses an
external program and other Guile Scheme modules (some might call this a "pure
scheme" package). So we use the `GUILE_SITE_DIR` macro, a regular
`AC_PATH_PROG` macro, and the `GUILE_MODULE_AVAILABLE` macro.

```
In configure.ac:

  GUILE_SITE_DIR

  probably_wont_work=""

  # pgtype pgtable
  GUILE_MODULE_AVAILABLE(have_guile_pg, (database postgres))
  test $have_guile_pg = no &&
      probably_wont_work="(my pgtype) (my pgtable) $probably_wont_work"

  # gpgutils
  AC_PATH_PROG(GNUPG,gpg)
  test x"$GNUPG" = x &&
      probably_wont_work="(my gpgutils) $probably_wont_work"

  if test ! "$probably_wont_work" = "" ; then
      p="         ***"
      echo
      echo "$p"
      echo "$p NOTE:"
      echo "$p The following modules probably won't work:"
      echo "$p   $probably_wont_work"
      echo "$p They can be installed anyway, and will work if their"
      echo "$p dependencies are installed later.  Please see README."
      echo "$p"
      echo
  fi

In Makefile.in:

  instdir = @GUILE_SITE@/my

  install:
        $(INSTALL) my/*.scm $(instdir)
```

---

Next: [Guile Modules](#Guile-Modules), Previous: [Programming in C](#Programming-in-C), Up: [The Guile Reference Manual](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]## 6 API Reference [¶](#API-Reference-1)

Guile provides an application programming interface (*API*) to
developers in two core languages: Scheme and C. This part of the manual
contains reference documentation for all of the functionality that is
available through both Scheme and C interfaces.

* [Overview of the Guile API](#API-Overview)
* [Deprecation](#Deprecation)
* [The SCM Type](#The-SCM-Type)
* [Initializing Guile](#Initialization)
* [Snarfing Macros](#Snarfing-Macros)
* [Data Types](#Data-Types)
* [Procedures](#Procedures)
* [Macros](#Macros)
* [General Utility Functions](#Utility-Functions)
* [Definitions and Variable Bindings](#Binding-Constructs)
* [Controlling the Flow of Program Execution](#Control-Mechanisms)
* [Input and Output](#Input-and-Output)
* [Regular Expressions](#Regular-Expressions)
* [LALR(1) Parsing](#LALR_00281_0029-Parsing)
* [PEG Parsing](#PEG-Parsing)
* [Reading and Evaluating Scheme Code](#Read_002fLoad_002fEval_002fCompile)
* [Memory Management and Garbage Collection](#Memory-Management)
* [Modules](#Modules)
* [Foreign Function Interface](#Foreign-Function-Interface)
* [Foreign Objects](#Foreign-Objects)
* [Smobs](#Smobs)
* [Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)
* [Configuration, Features and Runtime Options](#Options-and-Config)
* [Support for Other Languages](#Other-Languages)
* [Support for Internationalization](#Internationalization)
* [Debugging Infrastructure](#Debugging)
* [Code Coverage Reports](#Code-Coverage)

---

Next: [Deprecation](#Deprecation), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.1 Overview of the Guile API [¶](#Overview-of-the-Guile-API)

Guile’s application programming interface (*API*) makes
functionality available that an application developer can use in either
C or Scheme programming. The interface consists of *elements* that
may be macros, functions or variables in C, and procedures, variables,
syntax or other types of object in Scheme.

Many elements are available to both Scheme and C, in a form that is
appropriate. For example, the `assq` Scheme procedure is also
available as `scm_assq` to C code. These elements are documented
only once, addressing both the Scheme and C aspects of them.

The Scheme name of an element is related to its C name in a regular
way. Also, a C function takes its parameters in a systematic way.

Normally, the name of a C function can be derived given its Scheme name,
using some simple textual transformations:

* Replace `-` (hyphen) with `_` (underscore).
* Replace `?` (question mark) with `_p`.
* Replace `!` (exclamation point) with `_x`.
* Replace internal `->` with `_to_`.
* Replace `<=` (less than or equal) with `_leq`.
* Replace `>=` (greater than or equal) with `_geq`.
* Replace `<` (less than) with `_less`.
* Replace `>` (greater than) with `_gr`.
* Prefix with `scm_`.

A C function always takes a fixed number of arguments of type
`SCM`, even when the corresponding Scheme function takes a
variable number.

For some Scheme functions, some last arguments are optional; the
corresponding C function must always be invoked with all optional
arguments specified. To get the effect as if an argument has not been
specified, pass `SCM_UNDEFINED` as its value. You can not do
this for an argument in the middle; when one argument is
`SCM_UNDEFINED` all the ones following it must be
`SCM_UNDEFINED` as well.

Some Scheme functions take an arbitrary number of *rest*
arguments; the corresponding C function must be invoked with a list of
all these arguments. This list is always the last argument of the C
function.

These two variants can also be combined.

The type of the return value of a C function that corresponds to a
Scheme function is always `SCM`. In the descriptions below,
types are therefore often omitted but for the return value and for the
arguments.

---

Next: [The SCM Type](#The-SCM-Type), Previous: [Overview of the Guile API](#API-Overview), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.2 Deprecation [¶](#Deprecation-1)

From time to time functions and other features of Guile become obsolete.
Guile’s *deprecation* is a mechanism that can help you cope with
this.

When you use a feature that is deprecated, you will likely get a warning
message at run-time. Also, if you have a new enough toolchain, using a
deprecated function from `libguile` will cause a link-time warning.

The primary source for information about just what interfaces are
deprecated in a given release is the file `NEWS`. That file also
documents what you should use instead of the obsoleted things.

The file `README` contains instructions on how to control the
inclusion or removal of the deprecated features from the public API of
Guile, and how to control the deprecation warning messages.

The idea behind this mechanism is that normally all deprecated
interfaces are available, but you get feedback when compiling and
running code that uses them, so that you can migrate to the newer APIs
at your leisure.

---

Next: [Initializing Guile](#Initialization), Previous: [Deprecation](#Deprecation), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.3 The SCM Type [¶](#The-SCM-Type-1)

Guile represents all Scheme values with the single C type `SCM`.
For an introduction to this topic, See [Dynamic Types](#Dynamic-Types).

C Type: **SCM** [¶](#index-SCM)
:   `SCM` is the user level abstract C type that is used to represent
    all of Guile’s Scheme objects, no matter what the Scheme object type is.
    No C operation except assignment is guaranteed to work with variables of
    type `SCM`, so you should only use macros and functions to work
    with `SCM` values. Values are converted between C data types and
    the `SCM` type with utility functions and macros.

C Type: **scm\_t\_bits** [¶](#index-scm_005ft_005fbits)
:   `scm_t_bits` is an unsigned integral data type that is guaranteed
    to be large enough to hold all information that is required to
    represent any Scheme object. While this data type is mostly used to
    implement Guile’s internals, the use of this type is also necessary to
    write certain kinds of extensions to Guile.

C Type: **scm\_t\_signed\_bits** [¶](#index-scm_005ft_005fsigned_005fbits)
:   This is a signed integral type of the same size as `scm_t_bits`.

C Macro: `scm_t_bits` **SCM\_UNPACK** `(SCM x)` [¶](#index-SCM_005fUNPACK)
:   Transforms the `SCM` value x into its representation as an
    integral type. Only after applying `SCM_UNPACK` it is possible to
    access the bits and contents of the `SCM` value.

C Macro: `SCM` **SCM\_PACK** `(scm_t_bits x)` [¶](#index-SCM_005fPACK)
:   Takes a valid integral representation of a Scheme object and transforms
    it into its representation as a `SCM` value.

---

Next: [Snarfing Macros](#Snarfing-Macros), Previous: [The SCM Type](#The-SCM-Type), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.4 Initializing Guile [¶](#Initializing-Guile)

Each thread that wants to use functions from the Guile API needs to
put itself into guile mode with either `scm_with_guile` or
`scm_init_guile`. The global state of Guile is initialized
automatically when the first thread enters guile mode.

When a thread wants to block outside of a Guile API function, it
should leave guile mode temporarily with `scm_without_guile`,
See [Blocking in Guile Mode](#Blocking).

Threads that are created by `call-with-new-thread` or
`scm_spawn_thread` start out in guile mode so you don’t need to
initialize them.

C Function: `void *` **scm\_with\_guile** `(void *(*func)(void *), void *data)` [¶](#index-scm_005fwith_005fguile)
:   Call func, passing it data and return what func
    returns. While func is running, the current thread is in guile
    mode and can thus use the Guile API.

    When `scm_with_guile` is called from guile mode, the thread remains
    in guile mode when `scm_with_guile` returns.

    Otherwise, it puts the current thread into guile mode and, if needed,
    gives it a Scheme representation that is contained in the list returned
    by `all-threads`, for example. This Scheme representation is not
    removed when `scm_with_guile` returns so that a given thread is
    always represented by the same Scheme value during its lifetime, if at
    all.

    When this is the first thread that enters guile mode, the global state
    of Guile is initialized before calling `func`.

    The function func is called via
    `scm_with_continuation_barrier`; thus, `scm_with_guile`
    returns exactly once.

    When `scm_with_guile` returns, the thread is no longer in guile
    mode (except when `scm_with_guile` was called from guile mode, see
    above). Thus, only `func` can store `SCM` variables on the
    stack and be sure that they are protected from the garbage collector.
    See `scm_init_guile` for another approach at initializing Guile
    that does not have this restriction.

    It is OK to call `scm_with_guile` while a thread has temporarily
    left guile mode via `scm_without_guile`. It will then simply
    temporarily enter guile mode again.

C Function: `void` **scm\_init\_guile** `()` [¶](#index-scm_005finit_005fguile)
:   Arrange things so that all of the code in the current thread executes as
    if from within a call to `scm_with_guile`. That is, all functions
    called by the current thread can assume that `SCM` values on their
    stack frames are protected from the garbage collector (except when the
    thread has explicitly left guile mode, of course).

    When `scm_init_guile` is called from a thread that already has been
    in guile mode once, nothing happens. This behavior matters when you
    call `scm_init_guile` while the thread has only temporarily left
    guile mode: in that case the thread will not be in guile mode after
    `scm_init_guile` returns. Thus, you should not use
    `scm_init_guile` in such a scenario.

    When a uncaught throw happens in a thread that has been put into guile
    mode via `scm_init_guile`, a short message is printed to the
    current error port and the thread is exited via `scm_pthread_exit
    (NULL)`. No restrictions are placed on continuations.

    The function `scm_init_guile` might not be available on all
    platforms since it requires some stack-bounds-finding magic that might
    not have been ported to all platforms that Guile runs on. Thus, if you
    can, it is better to use `scm_with_guile` or its variation
    `scm_boot_guile` instead of this function.

C Function: `void` **scm\_boot\_guile** `(int argc, char **argv, void (*main_func) (void *data, int argc, char **argv), void *data)` [¶](#index-scm_005fboot_005fguile)
:   Enter guile mode as with `scm_with_guile` and call main\_func,
    passing it data, argc, and argv as indicated. When
    main\_func returns, `scm_boot_guile` calls `exit (0)`;
    `scm_boot_guile` never returns. If you want some other exit value,
    have main\_func call `exit` itself. If you don’t want to exit
    at all, use `scm_with_guile` instead of `scm_boot_guile`.

    The function `scm_boot_guile` arranges for the Scheme
    `command-line` function to return the strings given by argc
    and argv. If main\_func modifies argc or argv,
    it should call `scm_set_program_arguments` with the final list, so
    Scheme code will know which arguments have been processed
    (see [Runtime Environment](#Runtime-Environment)).

C Function: `void` **scm\_shell** `(int argc, char **argv)` [¶](#index-scm_005fshell)
:   Process command-line arguments in the manner of the `guile`
    executable. This includes loading the normal Guile initialization
    files, interacting with the user or running any scripts or expressions
    specified by `-s` or `-e` options, and then exiting.
    See [Invoking Guile](#Invoking-Guile), for more details.

    Since this function does not return, you must do all
    application-specific initialization before calling this function.

---

Next: [Data Types](#Data-Types), Previous: [Initializing Guile](#Initialization), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 6.5 Snarfing Macros [¶](#Snarfing-Macros-1)

The following macros do two different things: when compiled normally,
they expand in one way; when processed during snarfing, they cause the
`guile-snarf` program to pick up some initialization code,
See [Function Snarfing](#Function-Snarfing).

The descriptions below use the term ‘normally’ to refer to the case
when the code is compiled normally, and ‘while snarfing’ when the code
is processed by `guile-snarf`.

C Macro: **SCM\_SNARF\_INIT** (code) [¶](#index-SCM_005fSNARF_005fINIT)
:   Normally, `SCM_SNARF_INIT` expands to nothing; while snarfing, it
    causes code to be included in the initialization action file,
    followed by a semicolon.

    This is the fundamental macro for snarfing initialization actions.
    The more specialized macros below use it internally.

C Macro: **SCM\_DEFINE** (c\_name, scheme\_name, req, opt, var, arglist, docstring) [¶](#index-SCM_005fDEFINE)
:   Normally, this macro expands into

    ```
    static const char s_c_name[] = scheme_name;
    SCM
    c_name arglist
    ```

    While snarfing, it causes

    ```
    scm_c_define_gsubr (s_c_name, req, opt, var,
                        c_name);
    ```

    to be added to the initialization actions. Thus, you can use it to
    declare a C function named c\_name that will be made available to
    Scheme with the name scheme\_name.

    Note that the arglist argument must have parentheses around it.

C Macro: **SCM\_SYMBOL** (c\_name, scheme\_name) [¶](#index-SCM_005fSYMBOL)

C Macro: **SCM\_GLOBAL\_SYMBOL** (c\_name, scheme\_name) [¶](#index-SCM_005fGLOBAL_005fSYMBOL)
:   Normally, these macros expand into

    ```
    static SCM c_name
    ```

    or

    ```
    SCM c_name
    ```

    respectively. While snarfing, they both expand into the
    initialization code

    ```
    c_name = scm_permanent_object (scm_from_locale_symbol (scheme_name));
    ```

    Thus, you can use them declare a static or global variable of type
    `SCM` that will be initialized to the symbol named
    scheme\_name.

C Macro: **SCM\_KEYWORD** (c\_name, scheme\_name) [¶](#index-SCM_005fKEYWORD)

C Macro: **SCM\_GLOBAL\_KEYWORD** (c\_name, scheme\_name) [¶](#index-SCM_005fGLOBAL_005fKEYWORD)
:   Normally, these macros expand into

    ```
    static SCM c_name
    ```

    or

    ```
    SCM c_name
    ```

    respectively. While snarfing, they both expand into the
    initialization code

    ```
    c_name = scm_permanent_object (scm_c_make_keyword (scheme_name));
    ```

    Thus, you can use them declare a static or global variable of type
    `SCM` that will be initialized to the keyword named
    scheme\_name.

C Macro: **SCM\_VARIABLE** (c\_name, scheme\_name) [¶](#index-SCM_005fVARIABLE)

C Macro: **SCM\_GLOBAL\_VARIABLE** (c\_name, scheme\_name) [¶](#index-SCM_005fGLOBAL_005fVARIABLE)
:   These macros are equivalent to `SCM_VARIABLE_INIT` and
    `SCM_GLOBAL_VARIABLE_INIT`, respectively, with a value of
    `SCM_BOOL_F`.

C Macro: **SCM\_VARIABLE\_INIT** (c\_name, scheme\_name, value) [¶](#index-SCM_005fVARIABLE_005fINIT)

C Macro: **SCM\_GLOBAL\_VARIABLE\_INIT** (c\_name, scheme\_name, value) [¶](#index-SCM_005fGLOBAL_005fVARIABLE_005fINIT)
:   Normally, these macros expand into

    ```
    static SCM c_name
    ```

    or

    ```
    SCM c_name
    ```

    respectively. While snarfing, they both expand into the
    initialization code

    ```
    c_name = scm_permanent_object (scm_c_define (scheme_name, value));
    ```

    Thus, you can use them declare a static or global C variable of type
    `SCM` that will be initialized to the object representing the
    Scheme variable named scheme\_name in the current module. The
    variable will be defined when it doesn’t already exist. It is always
    set to value.

---

Next: [Procedures](#Procedures), Previous: [Snarfing Macros](#Snarfing-Macros), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
