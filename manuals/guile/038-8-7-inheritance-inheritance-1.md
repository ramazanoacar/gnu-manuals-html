### 8.7 Inheritance [¶](#Inheritance-1)

Here are some class definitions to help illustrate inheritance:

```
(define-class A () a)
(define-class B () b)
(define-class C () c)
(define-class D (A B) d a)
(define-class E (A C) e c)
(define-class F (D E) f)
```

`A`, `B`, `C` have a null list of superclasses. In this
case, the system will replace the null list by a list which only
contains `<object>`, the root of all the classes defined by
`define-class`. `D`, `E`, `F` use multiple
inheritance: each class inherits from two previously defined classes.
Those class definitions define a hierarchy which is shown in
[Figure 8.2](#fig_003ahier). In this figure, the class `<top>` is also shown;
this class is the superclass of all Scheme objects. In particular,
`<top>` is the superclass of all standard Scheme
types.

```
          <top>
          / \\\_____________________
         /   \\___________          \
        /     \           \          \
    <object>  <pair>  <procedure>  <number>
    /  |  \                           |
   /   |   \                          |
  A    B    C                      <complex>
  |\__/__   |                         |
   \ /   \ /                          |
    D     E                         <real>
     \   /                            |
       F                              |
                                   <integer>
```

**Figure 8.2:** A class hierarchy.

When a class has superclasses, its set of slots is calculated by taking
the union of its own slots and those of all its superclasses. Thus each
instance of D will have three slots, `a`, `b` and
`d`). The slots of a class can be discovered using the
`class-slots` primitive. For instance,

```
(class-slots A) ⇒ ((a))
(class-slots E) ⇒ ((a) (e) (c))
(class-slots F) ⇒ ((e) (c) (b) (d) (a) (f))
```

The ordering of the returned slots is not significant.

* [Class Precedence List](#Class-Precedence-List)
* [Sorting Methods](#Sorting-Methods)
* [Inheritance and accessors](#Inheritance-and-accessors)

---

Next: [Sorting Methods](#Sorting-Methods), Up: [Inheritance](#Inheritance)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.7.1 Class Precedence List [¶](#Class-Precedence-List-1)

What happens when a class inherits from two or more superclasses that
have a slot with the same name but incompatible definitions — for
example, different init values or slot allocations? We need a rule for
deciding which slot definition the derived class ends up with, and this
rule is provided by the class’s *Class Precedence
List*.[35](#FOOT35)

Another problem arises when invoking a generic function, and there is
more than one method that could apply to the call arguments. Here we
need a way of ordering the applicable methods, so that Guile knows which
method to use first, which to use next if that method calls
`next-method`, and so on. One of the ingredients for this ordering
is determining, for each given call argument, which of the specializing
classes, from each applicable method’s definition, is the most specific
for that argument; and here again the class precedence list helps.

If inheritance was restricted such that each class could only have one
superclass — which is known as *single* inheritance — class
ordering would be easy. The rule would be simply that a subclass is
considered more specific than its superclass.

With multiple inheritance, ordering is less obvious, and we have to
impose an arbitrary rule to determine precedence. Suppose we have

```
(define-class X ()
   (x #:init-value 1))

(define-class Y ()
   (x #:init-value 2))

(define-class Z (X Y)
   (...))
```

Clearly the `Z` class is more specific than `X` or `Y`,
for instances of `Z`. But which is more specific out of `X`
and `Y` — and hence, for the definitions above, which
`#:init-value` will take effect when creating an instance of
`Z`? The rule in GOOPS is that the superclasses listed earlier
are more specific than those listed later. Hence `X` is more
specific than `Y`, and the `#:init-value` for slot `x` in
instances of `Z` will be 1.

Hence there is a linear ordering for a class and all its
superclasses, from most specific to least specific, and this ordering is
called the Class Precedence List of the class.

In fact the rules above are not quite enough to always determine a
unique order, but they give an idea of how things work. For example,
for the `F` class shown in [Figure 8.2](#fig_003ahier), the class precedence
list is

```
(f d e a c b <object> <top>)
```

In cases where there is any ambiguity (like this one), it is a bad idea
for programmers to rely on exactly what the order is. If the order for
some superclasses is important, it can be expressed directly in the
class definition.

The precedence list of a class can be obtained by calling
`class-precedence-list`. This function returns a ordered list
whose first element is the most specific class. For instance:

```
(class-precedence-list B) ⇒ (#<<class> B 401b97c8> 
                                     #<<class> <object> 401e4a10> 
                                     #<<class> <top> 4026a9d8>)
```

Or for a more immediately readable result:

```
(map class-name (class-precedence-list B)) ⇒ (B <object> <top>)
```

---

Next: [Inheritance and accessors](#Inheritance-and-accessors), Previous: [Class Precedence List](#Class-Precedence-List), Up: [Inheritance](#Inheritance)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.7.2 Sorting Methods [¶](#Sorting-Methods-1)

Now, with the idea of the class precedence list, we can state precisely
how the possible methods are sorted when more than one of the methods of
a generic function are applicable to the call arguments.

The rules are that

* the applicable methods are sorted in order of specificity, and the most
  specific method is used first, then the next if that method calls
  `next-method`, and so on
* a method M1 is more specific than another method M2 if the first
  specializing class that differs, between the definitions of M1 and M2,
  is more specific, in M1’s definition, for the corresponding actual call
  argument, than the specializing class in M2’s definition
* a class C1 is more specific than another class C2, for an object of
  actual class C, if C1 comes before C2 in C’s class precedence list.

---

Previous: [Sorting Methods](#Sorting-Methods), Up: [Inheritance](#Inheritance)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.7.3 Inheritance and accessors [¶](#Inheritance-and-accessors-1)

When a class <A> defines a getter, setter or accessor for one of
its slots x, an *accessor method* specialized to class
<A> is created. Accessor methods are special in that they always
refer to a concrete class. When you subclass <A>, a new accessor
method for x is created automatically, specialized to the
subclass:

```
(define-class <A> () (x #:accessor x))
(define-class <B> (<A>))
(generic-function-methods x)
⇒
(#<<accessor-method> (<B>) 7faa66b5b1c0>
 #<<accessor-method> (<A>) 7faa66b5b240>)
```

Note, in particular, that the x accessor method specialized to
<A> is *not* applicable to objects of class B:

```
(define o (make <B>))
(compute-applicable-methods x (list o))
⇒
(#<<accessor-method> (<B>) 7faa66b5b1c0>)
```

As a consequence, an accessor doesn’t have a next-method. The fact that
accessor methods always apply to concrete classes allows for extensive
optimization.

---

Next: [Error Handling](#GOOPS-Error-Handling), Previous: [Inheritance](#Inheritance), Up: [GOOPS](#GOOPS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 8.8 Introspection [¶](#Introspection-1)

*Introspection*, or *reflection*, means being able to obtain
information dynamically about GOOPS objects. It is perhaps best
illustrated by considering an object oriented language that does not
provide any introspection, namely C++.

Nothing in C++ allows a running program to obtain answers to the following
types of question:

* What are the data members of this object or class?
* What classes does this class inherit from?
* Is this method call virtual or non-virtual?
* If I invoke `Employee::adjustHoliday()`, what class contains the
  `adjustHoliday()` method that will be applied?

In C++, answers to such questions can only be determined by looking at
the source code, if you have access to it. GOOPS, on the other hand,
includes procedures that allow answers to these questions — or their
GOOPS equivalents — to be obtained dynamically, at run time.

* [Classes](#Classes)
* [Instances](#Instances)
* [Slots](#Slots)
* [Generic Functions](#Generic-Functions)
* [Accessing Slots](#Accessing-Slots)

---

Next: [Instances](#Instances), Up: [Introspection](#Introspection)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.8.1 Classes [¶](#Classes-1)

A GOOPS class is itself an instance of the `<class>` class, or of a
subclass of `<class>`. The definition of the `<class>` class
has slots that are used to describe the properties of a class, including
the following.

primitive procedure: **class-name** class [¶](#index-class_002dname)
:   Return the name of class class. This is the value of
    class’s `name` slot.

primitive procedure: **class-direct-supers** class [¶](#index-class_002ddirect_002dsupers)
:   Return a list containing the direct superclasses of class. This
    is the value of class’s `direct-supers` slot.

primitive procedure: **class-direct-slots** class [¶](#index-class_002ddirect_002dslots)
:   Return a list containing the slot definitions of the direct slots of
    class. This is the value of class’s `direct-slots`
    slot.

primitive procedure: **class-direct-subclasses** class [¶](#index-class_002ddirect_002dsubclasses)
:   Return a list containing the direct subclasses of class. This is
    the value of class’s `direct-subclasses` slot.

primitive procedure: **class-direct-methods** class [¶](#index-class_002ddirect_002dmethods)
:   Return a list of all the generic function methods that use class
    as a formal parameter specializer. This is the value of class’s
    `direct-methods` slot.

primitive procedure: **class-precedence-list** class [¶](#index-class_002dprecedence_002dlist)
:   Return the class precedence list for class class (see [Class Precedence List](#Class-Precedence-List)). This is the value of class’s `cpl` slot.

primitive procedure: **class-slots** class [¶](#index-class_002dslots)
:   Return a list containing the slot definitions for all class’s
    slots, including any slots that are inherited from superclasses. This
    is the value of class’s `slots` slot.

procedure: **class-subclasses** class [¶](#index-class_002dsubclasses)
:   Return a list of all subclasses of class.

procedure: **class-methods** class [¶](#index-class_002dmethods)
:   Return a list of all methods that use class or a subclass of
    class as one of its formal parameter specializers.

---

Next: [Slots](#Slots), Previous: [Classes](#Classes), Up: [Introspection](#Introspection)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.8.2 Instances [¶](#Instances-1)

primitive procedure: **class-of** value [¶](#index-class_002dof)
:   Return the GOOPS class of any Scheme value.

primitive procedure: **instance?** object [¶](#index-instance_003f)
:   Return `#t` if object is any GOOPS instance, otherwise
    `#f`.

procedure: **is-a?** object class [¶](#index-is_002da_003f)
:   Return `#t` if object is an instance of class or one of
    its subclasses.

You can use the `is-a?` predicate to ask whether any given value
belongs to a given class, or `class-of` to discover the class of a
given value. Note that when GOOPS is loaded (by code using the
`(oop goops)` module) built-in classes like `<string>`,
`<list>` and `<number>` are automatically set up,
corresponding to all Guile Scheme types.

```
(is-a? 2.3 <number>) ⇒ #t
(is-a? 2.3 <real>) ⇒ #t
(is-a? 2.3 <string>) ⇒ #f
(is-a? '("a" "b") <string>) ⇒ #f
(is-a? '("a" "b") <list>) ⇒ #t
(is-a? (car '("a" "b")) <string>) ⇒ #t
(is-a? <string> <class>) ⇒ #t
(is-a? <class> <string>) ⇒ #f

(class-of 2.3) ⇒ #<<class> <real> 908c708>
(class-of #(1 2 3)) ⇒ #<<class> <vector> 908cd20>
(class-of <string>) ⇒ #<<class> <class> 8bd3e10>
(class-of <class>) ⇒ #<<class> <class> 8bd3e10>
```

---

Next: [Generic Functions](#Generic-Functions), Previous: [Instances](#Instances), Up: [Introspection](#Introspection)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.8.3 Slots [¶](#Slots-1)

procedure: **class-slot-definition** class slot-name [¶](#index-class_002dslot_002ddefinition)
:   Return the slot definition for the slot named slot-name in class
    class. slot-name should be a symbol.

procedure: **slot-definition-name** slot-def [¶](#index-slot_002ddefinition_002dname)
:   Extract and return the slot name from slot-def.

procedure: **slot-definition-options** slot-def [¶](#index-slot_002ddefinition_002doptions)
:   Extract and return the slot options from slot-def.

procedure: **slot-definition-allocation** slot-def [¶](#index-slot_002ddefinition_002dallocation)
:   Extract and return the slot allocation option from slot-def. This
    is the value of the `#:allocation` keyword (see [allocation](#Slot-Options)), or `#:instance` if the `#:allocation` keyword is
    absent.

procedure: **slot-definition-getter** slot-def [¶](#index-slot_002ddefinition_002dgetter)
:   Extract and return the slot getter option from slot-def. This is
    the value of the `#:getter` keyword (see [getter](#Slot-Options)), or `#f` if the `#:getter` keyword is absent.

procedure: **slot-definition-setter** slot-def [¶](#index-slot_002ddefinition_002dsetter)
:   Extract and return the slot setter option from slot-def. This is
    the value of the `#:setter` keyword (see [setter](#Slot-Options)), or `#f` if the `#:setter` keyword is absent.

procedure: **slot-definition-accessor** slot-def [¶](#index-slot_002ddefinition_002daccessor)
:   Extract and return the slot accessor option from slot-def. This
    is the value of the `#:accessor` keyword (see [accessor](#Slot-Options)), or `#f` if the `#:accessor` keyword is absent.

procedure: **slot-definition-init-value** slot-def [¶](#index-slot_002ddefinition_002dinit_002dvalue)
:   Extract and return the slot init-value option from slot-def. This
    is the value of the `#:init-value` keyword (see [init-value](#Slot-Options)), or the unbound value if the `#:init-value` keyword is
    absent.

procedure: **slot-definition-init-form** slot-def [¶](#index-slot_002ddefinition_002dinit_002dform)
:   Extract and return the slot init-form option from slot-def. This
    is the value of the `#:init-form` keyword (see [init-form](#Slot-Options)), or the unbound value if the `#:init-form` keyword is
    absent.

procedure: **slot-definition-init-thunk** slot-def [¶](#index-slot_002ddefinition_002dinit_002dthunk)
:   Extract and return the slot init-thunk option from slot-def. This
    is the value of the `#:init-thunk` keyword (see [init-thunk](#Slot-Options)), or `#f` if the `#:init-thunk` keyword is absent.

procedure: **slot-definition-init-keyword** slot-def [¶](#index-slot_002ddefinition_002dinit_002dkeyword)
:   Extract and return the slot init-keyword option from slot-def.
    This is the value of the `#:init-keyword` keyword (see [init-keyword](#Slot-Options)), or `#f` if the `#:init-keyword`
    keyword is absent.

procedure: **slot-init-function** class slot-name [¶](#index-slot_002dinit_002dfunction)
:   Return the initialization function for the slot named slot-name in
    class class. slot-name should be a symbol.

    The returned initialization function incorporates the effects of the
    standard `#:init-thunk`, `#:init-form` and `#:init-value`
    slot options. These initializations can be overridden by the
    `#:init-keyword` slot option or by a specialized `initialize`
    method, so, in general, the function returned by
    `slot-init-function` may be irrelevant. For a fuller discussion,
    see [init-value](#Slot-Options).

---

Next: [Accessing Slots](#Accessing-Slots), Previous: [Slots](#Slots), Up: [Introspection](#Introspection)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.8.4 Generic Functions [¶](#Generic-Functions-1)

A generic function is an instance of the `<generic>` class, or of a
subclass of `<generic>`. The definition of the `<generic>`
class has slots that are used to describe the properties of a generic
function.

primitive procedure: **generic-function-name** gf [¶](#index-generic_002dfunction_002dname)
:   Return the name of generic function gf.

primitive procedure: **generic-function-methods** gf [¶](#index-generic_002dfunction_002dmethods)
:   Return a list of the methods of generic function gf. This is the
    value of gf’s `methods` slot.

Similarly, a method is an instance of the `<method>` class, or of a
subclass of `<method>`; and the definition of the `<method>`
class has slots that are used to describe the properties of a method.

primitive procedure: **method-generic-function** method [¶](#index-method_002dgeneric_002dfunction)
:   Return the generic function that method belongs to. This is the
    value of method’s `generic-function` slot.

primitive procedure: **method-specializers** method [¶](#index-method_002dspecializers)
:   Return a list of method’s formal parameter specializers . This is
    the value of method’s `specializers` slot.

primitive procedure: **method-procedure** method [¶](#index-method_002dprocedure)
:   Return the procedure that implements method. This is the value of
    method’s `procedure` slot.

generic: **method-source** [¶](#index-method_002dsource)

method: **method-source** (m <method>) [¶](#index-method_002dsource-1)
:   Return an expression that prints to show the definition of method
    m.

    ```
    (define-generic cube)

    (define-method (cube (n <number>))
      (* n n n))

    (map method-source (generic-function-methods cube))
    ⇒
    ((method ((n <number>)) (* n n n)))
    ```

---

Previous: [Generic Functions](#Generic-Functions), Up: [Introspection](#Introspection)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.8.5 Accessing Slots [¶](#Accessing-Slots-1)

Any slot, regardless of its allocation, can be queried, referenced and
set using the following four primitive procedures.

primitive procedure: **slot-exists?** obj slot-name [¶](#index-slot_002dexists_003f)
:   Return `#t` if obj has a slot with name slot-name,
    otherwise `#f`.

primitive procedure: **slot-bound?** obj slot-name [¶](#index-slot_002dbound_003f)
:   Return `#t` if the slot named slot-name in obj has a
    value, otherwise `#f`.

    `slot-bound?` calls the generic function `slot-missing` if
    obj does not have a slot called slot-name (see [slot-missing](#Accessing-Slots)).

primitive procedure: **slot-ref** obj slot-name [¶](#index-slot_002dref-1)
:   Return the value of the slot named slot-name in obj.

    `slot-ref` calls the generic function `slot-missing` if
    obj does not have a slot called slot-name (see [slot-missing](#Accessing-Slots)).

    `slot-ref` calls the generic function `slot-unbound` if the
    named slot in obj does not have a value (see [slot-unbound](#Accessing-Slots)).

primitive procedure: **slot-set!** obj slot-name value [¶](#index-slot_002dset_0021-1)
:   Set the value of the slot named slot-name in obj to value.

    `slot-set!` calls the generic function `slot-missing` if
    obj does not have a slot called slot-name (see [slot-missing](#Accessing-Slots)).

GOOPS stores information about slots in classes. Internally,
all of these procedures work by looking up the slot definition for the
slot named slot-name in the class `(class-of
obj)`, and then using the slot definition’s “getter” and
“setter” closures to get and set the slot value.

The next four procedures differ from the previous ones in that they take
the class as an explicit argument, rather than assuming
`(class-of obj)`. Therefore they allow you to apply the
“getter” and “setter” closures of a slot definition in one class to
an instance of a different class.

primitive procedure: **slot-exists-using-class?** class obj slot-name [¶](#index-slot_002dexists_002dusing_002dclass_003f)
:   Return `#t` if class has a slot definition for a slot with
    name slot-name, otherwise `#f`.

primitive procedure: **slot-bound-using-class?** class obj slot-name [¶](#index-slot_002dbound_002dusing_002dclass_003f)
:   Return `#t` if applying `slot-ref-using-class` to the same
    arguments would call the generic function `slot-unbound`, otherwise
    `#f`.

    `slot-bound-using-class?` calls the generic function
    `slot-missing` if class does not have a slot definition for a
    slot called slot-name (see [slot-missing](#Accessing-Slots)).

primitive procedure: **slot-ref-using-class** class obj slot-name [¶](#index-slot_002dref_002dusing_002dclass)
:   Apply the “getter” closure for the slot named slot-name in
    class to obj, and return its result.

    `slot-ref-using-class` calls the generic function
    `slot-missing` if class does not have a slot definition for a
    slot called slot-name (see [slot-missing](#Accessing-Slots)).

    `slot-ref-using-class` calls the generic function
    `slot-unbound` if the application of the “getter” closure to
    obj returns an unbound value (see [slot-unbound](#Accessing-Slots)).

primitive procedure: **slot-set-using-class!** class obj slot-name value [¶](#index-slot_002dset_002dusing_002dclass_0021)
:   Apply the “setter” closure for the slot named slot-name in
    class to obj and value.

    `slot-set-using-class!` calls the generic function
    `slot-missing` if class does not have a slot definition for a
    slot called slot-name (see [slot-missing](#Accessing-Slots)).

Slots whose allocation is per-class rather than per-instance can be
referenced and set without needing to specify any particular instance.

procedure: **class-slot-ref** class slot-name [¶](#index-class_002dslot_002dref)
:   Return the value of the slot named slot-name in class class.
    The named slot must have `#:class` or `#:each-subclass`
    allocation (see [allocation](#Slot-Options)).

    If there is no such slot with `#:class` or `#:each-subclass`
    allocation, `class-slot-ref` calls the `slot-missing` generic
    function with arguments class and slot-name. Otherwise, if
    the slot value is unbound, `class-slot-ref` calls the
    `slot-unbound` generic function, with the same arguments.

procedure: **class-slot-set!** class slot-name value [¶](#index-class_002dslot_002dset_0021)
:   Set the value of the slot named slot-name in class class to
    value. The named slot must have `#:class` or
    `#:each-subclass` allocation (see [allocation](#Slot-Options)).

    If there is no such slot with `#:class` or `#:each-subclass`
    allocation, `class-slot-ref` calls the `slot-missing` generic
    function with arguments class and slot-name.

When a `slot-ref` or `slot-set!` call specifies a non-existent
slot name, or tries to reference a slot whose value is unbound, GOOPS
calls one of the following generic functions.

generic: **slot-missing** [¶](#index-slot_002dmissing)

method: **slot-missing** (class <class>) slot-name [¶](#index-slot_002dmissing-1)

method: **slot-missing** (class <class>) (object <object>) slot-name [¶](#index-slot_002dmissing-2)

method: **slot-missing** (class <class>) (object <object>) slot-name value [¶](#index-slot_002dmissing-3)
:   When an application attempts to reference or set a class or instance
    slot by name, and the slot name is invalid for the specified class
    or object, GOOPS calls the `slot-missing` generic function.

    The default methods all call `goops-error` with an appropriate
    message.

generic: **slot-unbound** [¶](#index-slot_002dunbound)

method: **slot-unbound** (object <object>) [¶](#index-slot_002dunbound-1)

method: **slot-unbound** (class <class>) slot-name [¶](#index-slot_002dunbound-2)

method: **slot-unbound** (class <class>) (object <object>) slot-name [¶](#index-slot_002dunbound-3)
:   When an application attempts to reference a class or instance slot, and
    the slot’s value is unbound, GOOPS calls the `slot-unbound` generic
    function.

    The default methods all call `goops-error` with an appropriate
    message.

---

Next: [GOOPS Object Miscellany](#GOOPS-Object-Miscellany), Previous: [Introspection](#Introspection), Up: [GOOPS](#GOOPS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 8.9 Error Handling [¶](#Error-Handling-2)

The procedure `goops-error` is called to raise an appropriate error
by the default methods of the following generic functions:

* `slot-missing` (see [slot-missing](#Accessing-Slots))
* `slot-unbound` (see [slot-unbound](#Accessing-Slots))
* `no-method` (see [no-method](#Handling-Invocation-Errors))
* `no-applicable-method` (see [no-applicable-method](#Handling-Invocation-Errors))
* `no-next-method` (see [no-next-method](#Handling-Invocation-Errors))

If you customize these functions for particular classes or metaclasses,
you may still want to use `goops-error` to signal any error
conditions that you detect.

procedure: **goops-error** format-string arg … [¶](#index-goops_002derror)
:   Raise an error with key `goops-error` and error message constructed
    from format-string and arg .... Error message
    formatting is as done by `scm-error`.

---

Next: [The Metaobject Protocol](#The-Metaobject-Protocol), Previous: [Error Handling](#GOOPS-Error-Handling), Up: [GOOPS](#GOOPS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 8.10 GOOPS Object Miscellany [¶](#GOOPS-Object-Miscellany-1)

Here we cover some points about GOOPS objects that aren’t substantial
enough to merit sections on their own.

#### Object Equality [¶](#Object-Equality)

When GOOPS is loaded, `eqv?`, `equal?` and `=` become
generic functions, and you can define methods for them, specialized for
your own classes, so as to control what the various kinds of equality
mean for your classes.

For example, the `assoc` procedure, for looking up an entry in an
alist, is specified as using `equal?` to determine when the car of
an entry in the alist is the same as the key parameter that `assoc`
is called with. Hence, if you had defined a new class, and wanted to
use instances of that class as the keys in an alist, you could define a
method for `equal?`, for your class, to control `assoc`’s
lookup precisely.

#### Cloning Objects [¶](#Cloning-Objects)

generic: **shallow-clone** [¶](#index-shallow_002dclone)

method: **shallow-clone** (self <object>) [¶](#index-shallow_002dclone-1)
:   Return a “shallow” clone of self. The default method makes a
    shallow clone by allocating a new instance and copying slot values from
    self to the new instance. Each slot value is copied either as an
    immediate value or by reference.

generic: **deep-clone** [¶](#index-deep_002dclone)

method: **deep-clone** (self <object>) [¶](#index-deep_002dclone-1)
:   Return a “deep” clone of self. The default method makes a deep
    clone by allocating a new instance and copying or cloning slot values
    from self to the new instance. If a slot value is an instance
    (satisfies `instance?`), it is cloned by calling `deep-clone`
    on that value. Other slot values are copied either as immediate values
    or by reference.

#### Write and Display [¶](#Write-and-Display)

primitive generic: **write** object port [¶](#index-write-3)

primitive generic: **display** object port [¶](#index-display-3)
:   When GOOPS is loaded, `write` and `display` become generic
    functions with special methods for printing

    * objects - instances of the class `<object>`
    * foreign objects - instances of the class `<foreign-object>`
    * classes - instances of the class `<class>`
    * generic functions - instances of the class `<generic>`
    * methods - instances of the class `<method>`.

    `write` and `display` print non-GOOPS values in the same way
    as the Guile primitive `write` and `display` functions.

In addition to the cases mentioned, you can of course define
`write` and `display` methods for your own classes, to
customize how instances of those classes are printed.

---

Next: [Redefining a Class](#Redefining-a-Class), Previous: [GOOPS Object Miscellany](#GOOPS-Object-Miscellany), Up: [GOOPS](#GOOPS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 8.11 The Metaobject Protocol [¶](#The-Metaobject-Protocol-1)

At this point, we’ve said about as much as can be said about GOOPS
without having to confront the idea of the metaobject protocol. There
are a couple more topics that could be discussed in isolation first —
class redefinition, and changing the class of existing instances — but
in practice developers using them will be advanced enough to want to
understand the metaobject protocol too, and will probably be using the
protocol to customize exactly what happens during these events.

So let’s plunge in. GOOPS is based on a “metaobject protocol” (aka
“MOP”) derived from the ones used in CLOS (the Common Lisp Object
System), tiny-clos (a small Scheme implementation of a subset of CLOS
functionality) and STKlos.

The MOP underlies many possible GOOPS customizations — such as
defining an `initialize` method to customize the initialization of
instances of an application-defined class — and an understanding of
the MOP makes it much easier to explain such customizations in a precise
way. And at a deeper level, understanding the MOP is a key part of
understanding GOOPS, and of taking full advantage of GOOPS’ power, by
customizing the behavior of GOOPS itself.

* [Metaobjects and the Metaobject Protocol](#Metaobjects-and-the-Metaobject-Protocol)
* [Metaclasses](#Metaclasses)
* [MOP Specification](#MOP-Specification)
* [Instance Creation Protocol](#Instance-Creation-Protocol)
* [Class Definition Protocol](#Class-Definition-Protocol)
* [Customizing Class Definition](#Customizing-Class-Definition)
* [Method Definition](#Method-Definition)
* [Method Definition Internals](#Method-Definition-Internals)
* [Generic Function Internals](#Generic-Function-Internals)
* [Generic Function Invocation](#Generic-Function-Invocation)

---

Next: [Metaclasses](#Metaclasses), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.1 Metaobjects and the Metaobject Protocol [¶](#Metaobjects-and-the-Metaobject-Protocol-1)

The building blocks of GOOPS are classes, slot definitions, instances,
generic functions and methods. A class is a grouping of inheritance
relations and slot definitions. An instance is an object with slots
that are allocated following the rules implied by its class’s
superclasses and slot definitions. A generic function is a collection
of methods and rules for determining which of those methods to apply
when the generic function is invoked. A method is a procedure and a set
of specializers that specify the type of arguments to which the
procedure is applicable.

Of these entities, GOOPS represents classes, generic functions and
methods as “metaobjects”. In other words, the values in a GOOPS
program that describe classes, generic functions and methods, are
themselves instances (or “objects”) of special GOOPS classes that
encapsulate the behavior, respectively, of classes, generic functions,
and methods.

(The other two entities are slot definitions and instances. Slot
definitions are not strictly instances, but every slot definition is
associated with a GOOPS class that specifies the behavior of the slot
as regards accessibility and protection from garbage collection.
Instances are of course objects in the usual sense, and there is no
benefit from thinking of them as metaobjects.)

The “metaobject protocol” (or “MOP”) is the specification of the
generic functions which determine the behavior of these metaobjects and
the circumstances in which these generic functions are invoked.

For a concrete example of what this means, consider how GOOPS calculates
the set of slots for a class that is being defined using
`define-class`. The desired set of slots is the union of the new
class’s direct slots and the slots of all its superclasses. But
`define-class` itself does not perform this calculation. Instead,
there is a method of the `initialize` generic function that is
specialized for instances of type `<class>`, and it is this method
that performs the slot calculation.

`initialize` is a generic function which GOOPS calls whenever a new
instance is created, immediately after allocating memory for a new
instance, in order to initialize the new instance’s slots. The sequence
of steps is as follows.

* `define-class` uses `make` to make a new instance of the
  `<class>` class, passing as initialization arguments the
  superclasses, slot definitions and class options that were specified in
  the `define-class` form.
* `make` allocates memory for the new instance, and invokes the
  `initialize` generic function to initialize the new instance’s
  slots.
* The `initialize` generic function applies the method that is
  specialized for instances of type `<class>`, and this method
  performs the slot calculation.

In other words, rather than being hardcoded in `define-class`, the
default behavior of class definition is encapsulated by generic
function methods that are specialized for the class `<class>`.

It is possible to create a new class that inherits from `<class>`,
which is called a “metaclass”, and to write a new `initialize`
method that is specialized for instances of the new metaclass. Then, if
the `define-class` form includes a `#:metaclass` class option
whose value is the new metaclass, the class that is defined by the
`define-class` form will be an instance of the new metaclass rather
than of the default `<class>`, and will be defined in accordance
with the new `initialize` method. Thus the default slot
calculation, as well as any other aspect of the new class’s relationship
with its superclasses, can be modified or overridden.

In a similar way, the behavior of generic functions can be modified or
overridden by creating a new class that inherits from the standard
generic function class `<generic>`, writing appropriate methods
that are specialized to the new class, and creating new generic
functions that are instances of the new class.

The same is true for method metaobjects. And the same basic mechanism
allows the application class author to write an `initialize` method
that is specialized to their application class, to initialize instances
of that class.

Such is the power of the MOP. Note that `initialize` is just one
of a large number of generic functions that can be customized to modify
the behavior of application objects and classes and of GOOPS itself.
Each following section covers a particular area of GOOPS functionality,
and describes the generic functions that are relevant for customization
of that area.

---

Next: [MOP Specification](#MOP-Specification), Previous: [Metaobjects and the Metaobject Protocol](#Metaobjects-and-the-Metaobject-Protocol), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.2 Metaclasses [¶](#Metaclasses-1)

A *metaclass* is the class of an object which represents a GOOPS
class. Put more succinctly, a metaclass is a class’s class.

Most GOOPS classes have the metaclass `<class>` and, by default,
any new class that is created using `define-class` has the
metaclass `<class>`.

But what does this really mean? To find out, let’s look in more detail
at what happens when a new class is created using `define-class`:

```
(define-class <my-class> (<object>) . slots)
```

Guile expands this to something like:

```
(define <my-class> (class (<object>) . slots))
```

which in turn expands to:

```
(define <my-class>
  (make <class> #:dsupers (list <object>) #:slots slots))
```

As this expansion makes clear, the resulting value of `<my-class>`
is an instance of the class `<class>` with slot values specifying
the superclasses and slot definitions for the class `<my-class>`.
(`#:dsupers` and `#:slots` are initialization keywords for the
`dsupers` and `dslots` slots of the `<class>` class.)

Now suppose that you want to define a new class with a metaclass other
than the default `<class>`. This is done by writing:

```
(define-class <my-class2> (<object>)
   slot ...
   #:metaclass <my-metaclass>)
```

and Guile expands *this* to something like:

```
(define <my-class2>
  (make <my-metaclass> #:dsupers (list <object>) #:slots slots))
```

In this case, the value of `<my-class2>` is an instance of the more
specialized class `<my-metaclass>`. Note that
`<my-metaclass>` itself must previously have been defined as a
subclass of `<class>`. For a full discussion of when and how it is
useful to define new metaclasses, see [MOP Specification](#MOP-Specification).

Now let’s make an instance of `<my-class2>`:

```
(define my-object (make <my-class2> ...))
```

All of the following statements are correct expressions of the
relationships between `my-object`, `<my-class2>`,
`<my-metaclass>` and `<class>`.

* `my-object` is an instance of the class `<my-class2>`.
* `<my-class2>` is an instance of the class `<my-metaclass>`.
* `<my-metaclass>` is an instance of the class `<class>`.
* The class of `my-object` is `<my-class2>`.
* The class of `<my-class2>` is `<my-metaclass>`.
* The class of `<my-metaclass>` is `<class>`.

---

Next: [Instance Creation Protocol](#Instance-Creation-Protocol), Previous: [Metaclasses](#Metaclasses), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.3 MOP Specification [¶](#MOP-Specification-1)

The aim of the MOP specification in this chapter is to specify all the
customizable generic function invocations that can be made by the standard
GOOPS syntax, procedures and methods, and to explain the protocol for
customizing such invocations.

A generic function invocation is customizable if the types of the
arguments to which it is applied are not completely determined by the
lexical context in which the invocation appears. For example, the
`(initialize instance initargs)` invocation in the
default `make-instance` method is customizable, because the type of
the `instance` argument is determined by the class that was
passed to `make-instance`.

(Whereas — to give a counter-example — the `(make <generic>
#:name ',name)` invocation in `define-generic` is not customizable,
because all of its arguments have lexically determined types.)

When using this rule to decide whether a given generic function invocation
is customizable, we ignore arguments that are expected to be handled in
method definitions as a single “rest” list argument.

For each customizable generic function invocation, the *invocation
protocol* is explained by specifying

* what, conceptually, the applied method is intended to do
* what assumptions, if any, the caller makes about the applied method’s side
  effects
* what the caller expects to get as the applied method’s return value.

---

Next: [Class Definition Protocol](#Class-Definition-Protocol), Previous: [MOP Specification](#MOP-Specification), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.4 Instance Creation Protocol [¶](#Instance-Creation-Protocol-1)

`make <class> . initargs` (method)

* `allocate-instance class initargs` (generic)

  The applied `allocate-instance` method should allocate storage for
  a new instance of class class and return the uninitialized instance.
* `initialize instance initargs` (generic)

  instance is the uninitialized instance returned by
  `allocate-instance`. The applied method should initialize the new
  instance in whatever sense is appropriate for its class. The method’s
  return value is ignored.

`make` itself is a generic function. Hence the `make`
invocation itself can be customized in the case where the new instance’s
metaclass is more specialized than the default `<class>`, by
defining a `make` method that is specialized to that metaclass.

Normally, however, the method for classes with metaclass `<class>`
will be applied. This method calls two generic functions:

* (allocate-instance class . initargs)
* (initialize instance . initargs)

`allocate-instance` allocates storage for and returns the new
instance, uninitialized. You might customize `allocate-instance`,
for example, if you wanted to provide a GOOPS wrapper around some other
object programming system.

To do this, you would create a specialized metaclass, which would act as
the metaclass for all classes and instances from the other system. Then
define an `allocate-instance` method, specialized to that
metaclass, which calls a Guile primitive C function (or FFI code), which
in turn allocates the new instance using the interface of the other
object system.

In this case, for a complete system, you would also need to customize a
number of other generic functions like `make` and
`initialize`, so that GOOPS knows how to make classes from the
other system, access instance slots, and so on.

`initialize` initializes the instance that is returned by
`allocate-instance`. The standard GOOPS methods perform
initializations appropriate to the instance class.

* At the least specialized level, the method for instances of type
  `<object>` performs internal GOOPS instance initialization, and
  initializes the instance’s slots according to the slot definitions and
  any slot initialization keywords that appear in initargs.
* The method for instances of type `<class>` calls
  `(next-method)`, then performs the class initializations described
  in [Class Definition Protocol](#Class-Definition-Protocol).
* and so on for generic functions, methods, operator classes …

Similarly, you can customize the initialization of instances of any
application-defined class by defining an `initialize` method
specialized to that class.

Imagine a class whose instances’ slots need to be initialized at
instance creation time by querying a database. Although it might be
possible to achieve this a combination of `#:init-thunk` keywords
and closures in the slot definitions, it may be neater to write an
`initialize` method for the class that queries the database once
and initializes all the dependent slot values according to the results.

---

Next: [Customizing Class Definition](#Customizing-Class-Definition), Previous: [Instance Creation Protocol](#Instance-Creation-Protocol), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.5 Class Definition Protocol [¶](#Class-Definition-Protocol-1)

Here is a summary diagram of the syntax, procedures and generic
functions that may be involved in class definition.

`define-class` (syntax)

* `class` (syntax)
  + `make-class` (procedure)
    - `ensure-metaclass` (procedure)
    - `make metaclass …` (generic)
      * `allocate-instance` (generic)
      * `initialize` (generic)
        + `compute-cpl` (generic)
          - `compute-std-cpl` (procedure)
        + `compute-slots` (generic)
        + `compute-get-n-set` (generic)
        + `compute-getter-method` (generic)
        + `compute-setter-method` (generic)
* `class-redefinition` (generic)
  + `remove-class-accessors` (generic)
  + `update-direct-method!` (generic)
  + `update-direct-subclass!` (generic)

Wherever a step above is marked as “generic”, it can be customized,
and the detail shown below it is only “correct” insofar as it
describes what the default method of that generic function does. For
example, if you write an `initialize` method, for some metaclass,
that does not call `next-method` and does not call
`compute-cpl`, then `compute-cpl` will not be called when a
class is defined with that metaclass.

A `(define-class ...)` form (see [Class Definition](#Class-Definition)) expands to
an expression which

* checks that it is being evaluated only at top level
* defines any accessors that are implied by the slot-definitions
* uses `class` to create the new class
* checks for a previous class definition for name and, if found,
  handles the redefinition by invoking `class-redefinition`
  (see [Redefining a Class](#Redefining-a-Class)).

syntax: **class** name (super …) slot-definition … class-option … [¶](#index-class-1)
:   Return a newly created class that inherits from supers, with
    direct slots defined by slot-definitions and class-options.
    For the format of slot-definitions and class-options, see
    [define-class](#Class-Definition).

`class` expands to an expression which

* processes the class and slot definition options to check that they are
  well-formed, to convert the `#:init-form` option to an
  `#:init-thunk` option, to supply a default environment parameter
  (the current top-level environment) and to evaluate all the bits that
  need to be evaluated
* calls `make-class` to create the class with the processed and
  evaluated parameters.

procedure: **make-class** supers slots class-option … [¶](#index-make_002dclass)
:   Return a newly created class that inherits from supers, with
    direct slots defined by slots and class-options. For the
    format of slots and class-options, see [define-class](#Class-Definition), except note that for `make-class`,
    slots is a separate list of slot definitions.

`make-class`

* adds `<object>` to the supers list if supers is empty
  or if none of the classes in supers have `<object>` in their
  class precedence list
* defaults the `#:environment`, `#:name` and `#:metaclass`
  options, if they are not specified by options, to the current
  top-level environment, the unbound value, and `(ensure-metaclass
  supers)` respectively
* checks for duplicate classes in supers and duplicate slot names in
  slots, and signals an error if there are any duplicates
* calls `make`, passing the metaclass as the first parameter and all
  other parameters as option keywords with values.

procedure: **ensure-metaclass** supers env [¶](#index-ensure_002dmetaclass)
:   Return a metaclass suitable for a class that inherits from the list of
    classes in supers. The returned metaclass is the union by
    inheritance of the metaclasses of the classes in supers.

    In the simplest case, where all the supers are straightforward
    classes with metaclass `<class>`, the returned metaclass is just
    `<class>`.

    For a more complex example, suppose that supers contained one
    class with metaclass `<operator-class>` and one with metaclass
    `<foreign-object-class>`. Then the returned metaclass would be a
    class that inherits from both `<operator-class>` and
    `<foreign-object-class>`.

    If supers is the empty list, `ensure-metaclass` returns the
    default GOOPS metaclass `<class>`.

    GOOPS keeps a list of the metaclasses created by
    `ensure-metaclass`, so that each required type of metaclass only
    has to be created once.

    The `env` parameter is ignored.

generic: **make** metaclass initarg … [¶](#index-make-3)
:   metaclass is the metaclass of the class being defined, either
    taken from the `#:metaclass` class option or computed by
    `ensure-metaclass`. The applied method must create and return the
    fully initialized class metaobject for the new class definition.

The `(make metaclass initarg …)` invocation is a
particular case of the instance creation protocol covered in the
previous section. It will create an class metaobject with metaclass
metaclass. By default, this metaobject will be initialized by the
`initialize` method that is specialized for instances of type
`<class>`.

The `initialize` method for classes (signature `(initialize
<class> initargs)`) calls the following generic functions.

* `compute-cpl class` (generic)

  The applied method should compute and return the class precedence list
  for class as a list of class metaobjects. When `compute-cpl`
  is called, the following class metaobject slots have all been
  initialized: `name`, `direct-supers`, `direct-slots`,
  `direct-subclasses` (empty), `direct-methods`. The value
  returned by `compute-cpl` will be stored in the `cpl` slot.
* `compute-slots class` (generic)

  The applied method should compute and return the slots (union of direct
  and inherited) for class as a list of slot definitions. When
  `compute-slots` is called, all the class metaobject slots
  mentioned for `compute-cpl` have been initialized, plus the
  following: `cpl`, `redefined` (`#f`), `environment`.
  The value returned by `compute-slots` will be stored in the
  `slots` slot.
* `compute-get-n-set class slot-def` (generic)

  `initialize` calls `compute-get-n-set` for each slot computed
  by `compute-slots`. The applied method should compute and return a
  pair of closures that, respectively, get and set the value of the specified
  slot. The get closure should have arity 1 and expect a single argument
  that is the instance whose slot value is to be retrieved. The set closure
  should have arity 2 and expect two arguments, where the first argument is
  the instance whose slot value is to be set and the second argument is the
  new value for that slot. The closures should be returned in a two element
  list: `(list get set)`.

  The closures returned by `compute-get-n-set` are stored as part of
  the value of the class metaobject’s `getters-n-setters` slot.
  Specifically, the value of this slot is a list with the same number of
  elements as there are slots in the class, and each element looks either like

  ```
  (slot-name-symbol init-function . index)
  ```

  or like

  ```
  (slot-name-symbol init-function get set)
  ```

  Where the get and set closures are replaced by index, the slot is
  an instance slot and index is the slot’s index in the underlying
  structure: GOOPS knows how to get and set the value of such slots and so
  does not need specially constructed get and set closures. Otherwise,
  get and set are the closures returned by `compute-get-n-set`.

  The structure of the `getters-n-setters` slot value is important when
  understanding the next customizable generic functions that `initialize`
  calls…
* `compute-getter-method class gns` (generic)

  `initialize` calls `compute-getter-method` for each of the
  class’s slots (as determined by `compute-slots`) that includes a
  `#:getter` or `#:accessor` slot option. gns is the
  element of the class metaobject’s `getters-n-setters` slot
  that specifies how the slot in question is referenced and set, as
  described above under `compute-get-n-set`. The applied method
  should create and return a method that is specialized for instances of
  type class and uses the get closure to retrieve the slot’s value.
  `initialize` uses `add-method!` to add the returned method to
  the generic function named by the slot definition’s `#:getter` or
  `#:accessor` option.
* `compute-setter-method class gns` (generic)

  `compute-setter-method` is invoked with the same arguments as
  `compute-getter-method`, for each of the class’s slots that includes
  a `#:setter` or `#:accessor` slot option. The applied method
  should create and return a method that is specialized for instances of
  type class and uses the set closure to set the slot’s value.
  `initialize` then uses `add-method!` to add the returned method
  to the generic function named by the slot definition’s `#:setter`
  or `#:accessor` option.

---

Next: [Method Definition](#Method-Definition), Previous: [Class Definition Protocol](#Class-Definition-Protocol), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.6 Customizing Class Definition [¶](#Customizing-Class-Definition-1)

If the metaclass of the new class is something more specialized than the
default `<class>`, then the type of class in the calls above
is more specialized than `<class>`, and hence it becomes possible
to define generic function methods, specialized for the new class’s
metaclass, that can modify or override the default behavior of
`initialize`, `compute-cpl` or `compute-get-n-set`.

`compute-cpl` computes the class precedence list (“CPL”) for the
new class (see [Class Precedence List](#Class-Precedence-List)), and returns it as a list of
class objects. The CPL is important because it defines a superclass
ordering that is used, when a generic function is invoked upon an
instance of the class, to decide which of the available generic function
methods is the most specific. Hence `compute-cpl` could be
customized in order to modify the CPL ordering algorithm for all classes
with a special metaclass.

The default CPL algorithm is encapsulated by the `compute-std-cpl`
procedure, which is called by the default `compute-cpl` method.

procedure: **compute-std-cpl** class [¶](#index-compute_002dstd_002dcpl)
:   Compute and return the class precedence list for class according
    to the algorithm described in [Class Precedence List](#Class-Precedence-List).

`compute-slots` computes and returns a list of all slot definitions
for the new class. By default, this list includes the direct slot
definitions from the `define-class` form, plus the slot definitions
that are inherited from the new class’s superclasses. The default
`compute-slots` method uses the CPL computed by `compute-cpl`
to calculate this union of slot definitions, with the rule that slots
inherited from superclasses are shadowed by direct slots with the same
name. One possible reason for customizing `compute-slots` would be
to implement an alternative resolution strategy for slot name conflicts.

`compute-get-n-set` computes the low-level closures that will be
used to get and set the value of a particular slot, and returns them in
a list with two elements.

The closures returned depend on how storage for that slot is allocated.
The standard `compute-get-n-set` method, specialized for classes of
type `<class>`, handles the standard GOOPS values for the
`#:allocation` slot option (see [allocation](#Slot-Options)). By
defining a new `compute-get-n-set` method for a more specialized
metaclass, it is possible to support new types of slot allocation.

Suppose you wanted to create a large number of instances of some class
with a slot that should be shared between some but not all instances of
that class - say every 10 instances should share the same slot storage.
The following example shows how to implement and use a new type of slot
allocation to do this.

```
(define-class <batched-allocation-metaclass> (<class>))

(let ((batch-allocation-count 0)
      (batch-get-n-set #f))
  (define-method (compute-get-n-set
                     (class <batched-allocation-metaclass>) s)
    (case (slot-definition-allocation s)
      ((#:batched)
       ;; If we've already used the same slot storage for 10 instances,
       ;; reset variables.
       (if (= batch-allocation-count 10)
           (begin
             (set! batch-allocation-count 0)
             (set! batch-get-n-set #f)))
       ;; If we don't have a current pair of get and set closures,
       ;; create one.  make-closure-variable returns a pair of closures
       ;; around a single Scheme variable - see goops.scm for details.
       (or batch-get-n-set
           (set! batch-get-n-set (make-closure-variable)))
       ;; Increment the batch allocation count.
       (set! batch-allocation-count (+ batch-allocation-count 1))
       batch-get-n-set)

      ;; Call next-method to handle standard allocation types.
      (else (next-method)))))

(define-class <class-using-batched-slot> ()
  ...
  (c #:allocation #:batched)
  ...
  #:metaclass <batched-allocation-metaclass>)
```

The usage of `compute-getter-method` and `compute-setter-method`
is described in [Class Definition Protocol](#Class-Definition-Protocol).

`compute-cpl` and `compute-get-n-set` are called by the
standard `initialize` method for classes whose metaclass is
`<class>`. But `initialize` itself can also be modified, by
defining an `initialize` method specialized to the new class’s
metaclass. Such a method could complete override the standard
behavior, by not calling `(next-method)` at all, but more
typically it would perform additional class initialization steps before
and/or after calling `(next-method)` for the standard behavior.

---

Next: [Method Definition Internals](#Method-Definition-Internals), Previous: [Customizing Class Definition](#Customizing-Class-Definition), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.7 Method Definition [¶](#Method-Definition-1)

`define-method` (syntax)

* `add-method! target method` (generic)

`define-method` invokes the `add-method!` generic function to
handle adding the new method to a variety of possible targets. GOOPS
includes methods to handle target as

* a generic function (the most common case)
* a procedure
* a primitive generic (see [Extending Primitives](#Extending-Primitives))

By defining further methods for `add-method!`, you can
theoretically handle adding methods to further types of target.

---

Next: [Generic Function Internals](#Generic-Function-Internals), Previous: [Method Definition](#Method-Definition), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.8 Method Definition Internals [¶](#Method-Definition-Internals-1)

`define-method`:

* checks the form of the first parameter, and applies the following steps
  to the accessor’s setter if it has the `(setter …)` form
* interpolates a call to `define-generic` or `define-accessor`
  if a generic function is not already defined with the supplied name
* calls `method` with the parameters and body, to make a
  new method instance
* calls `add-method!` to add this method to the relevant generic
  function.

syntax: **method** (parameter …) body … [¶](#index-method)
:   Make a method whose specializers are defined by the classes in
    parameters and whose procedure definition is constructed from the
    parameter symbols and body forms.

    The parameter and body parameters should be as for
    `define-method` (see [define-method](#Methods-and-Generic-Functions)).

`method`:

* extracts formals and specializing classes from the parameters,
  defaulting the class for unspecialized parameters to `<top>`
* creates a closure using the formals and the body forms
* calls `make` with metaclass `<method>` and the specializers
  and closure using the `#:specializers` and `#:procedure`
  keywords.

procedure: **make-method** specializers procedure [¶](#index-make_002dmethod)
:   Make a method using specializers and procedure.

    specializers should be a list of classes that specifies the
    parameter combinations to which this method will be applicable.

    procedure should be the closure that will applied to the generic
    function parameters when this method is invoked.

`make-method` is a simple wrapper around `make` with metaclass
`<method>`.

generic: **add-method!** target method [¶](#index-add_002dmethod_0021)
:   Generic function for adding method method to target.

method: **add-method!** (generic <generic>) (method <method>) [¶](#index-add_002dmethod_0021-1)
:   Add method method to the generic function generic.

method: **add-method!** (proc <procedure>) (method <method>) [¶](#index-add_002dmethod_0021-2)
:   If proc is a procedure with generic capability (see [generic-capability?](#Extending-Primitives)), upgrade it to a primitive generic
    and add method to its generic function definition.

method: **add-method!** (pg <primitive-generic>) (method <method>) [¶](#index-add_002dmethod_0021-3)
:   Add method method to the generic function definition of pg.

    Implementation: `(add-method! (primitive-generic-generic pg) method)`.

method: **add-method!** (whatever <top>) (method <method>) [¶](#index-add_002dmethod_0021-4)
:   Raise an error indicating that whatever is not a valid generic
    function.

---

Next: [Generic Function Invocation](#Generic-Function-Invocation), Previous: [Method Definition Internals](#Method-Definition-Internals), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.9 Generic Function Internals [¶](#Generic-Function-Internals-1)

`define-generic` calls `ensure-generic` to upgrade a
pre-existing procedure value, or `make` with metaclass
`<generic>` to create a new generic function.

`define-accessor` calls `ensure-accessor` to upgrade a
pre-existing procedure value, or `make-accessor` to create a new
accessor.

procedure: **ensure-generic** old-definition [name] [¶](#index-ensure_002dgeneric)
:   Return a generic function with name name, if possible by using or
    upgrading old-definition. If unspecified, name defaults to
    `#f`.

    If old-definition is already a generic function, it is returned
    unchanged.

    If old-definition is a Scheme procedure or procedure-with-setter,
    `ensure-generic` returns a new generic function that uses
    old-definition for its default procedure and setter.

    Otherwise `ensure-generic` returns a new generic function with no
    defaults and no methods.

procedure: **make-generic** [name] [¶](#index-make_002dgeneric)
:   Return a new generic function with name `(car name)`. If
    unspecified, name defaults to `#f`.

`ensure-generic` calls `make` with metaclasses
`<generic>` and `<generic-with-setter>`, depending on the
previous value of the variable that it is trying to upgrade.

`make-generic` is a simple wrapper for `make` with metaclass
`<generic>`.

procedure: **ensure-accessor** proc [name] [¶](#index-ensure_002daccessor)
:   Return an accessor with name name, if possible by using or
    upgrading proc. If unspecified, name defaults to `#f`.

    If proc is already an accessor, it is returned unchanged.

    If proc is a Scheme procedure, procedure-with-setter or generic
    function, `ensure-accessor` returns an accessor that reuses the
    reusable elements of proc.

    Otherwise `ensure-accessor` returns a new accessor with no defaults
    and no methods.

procedure: **make-accessor** [name] [¶](#index-make_002daccessor)
:   Return a new accessor with name `(car name)`. If
    unspecified, name defaults to `#f`.

`ensure-accessor` calls `make` with
metaclass `<generic-with-setter>`, as well as calls to
`ensure-generic`, `make-accessor` and (tail recursively)
`ensure-accessor`.

`make-accessor` calls `make` twice, first
with metaclass `<generic>` to create a generic function for the
setter, then with metaclass `<generic-with-setter>` to create the
accessor, passing the setter generic function as the value of the
`#:setter` keyword.

---

Previous: [Generic Function Internals](#Generic-Function-Internals), Up: [The Metaobject Protocol](#The-Metaobject-Protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.11.10 Generic Function Invocation [¶](#Generic-Function-Invocation-1)

There is a detailed and customizable protocol involved in the process of
invoking a generic function — i.e., in the process of deciding which
of the generic function’s methods are applicable to the current
arguments, and which one of those to apply. Here is a summary diagram
of the generic functions involved.

`apply-generic` (generic)

* `no-method` (generic)
* `compute-applicable-methods` (generic)
* `sort-applicable-methods` (generic)
  + `method-more-specific?` (generic)
* `apply-methods` (generic)
  + `apply-method` (generic)
  + `no-next-method` (generic)
* `no-applicable-method`

We do not yet have full documentation for these. Please refer to the
code (`oop/goops.scm`) for details.

---

Next: [Changing the Class of an Instance](#Changing-the-Class-of-an-Instance), Previous: [The Metaobject Protocol](#The-Metaobject-Protocol), Up: [GOOPS](#GOOPS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 8.12 Redefining a Class [¶](#Redefining-a-Class-1)

Suppose that a class `<my-class>` is defined using `define-class`
(see [define-class](#Class-Definition)), with slots that have
accessor functions, and that an application has created several instances
of `<my-class>` using `make` (see [make](#Instance-Creation)). What then happens if `<my-class>` is redefined by calling
`define-class` again?

* [Redefinable Classes](#Redefinable-Classes)
* [Default Class Redefinition Behavior](#Default-Class-Redefinition-Behavior)
* [Customizing Class Redefinition](#Customizing-Class-Redefinition)

---

Next: [Default Class Redefinition Behavior](#Default-Class-Redefinition-Behavior), Up: [Redefining a Class](#Redefining-a-Class)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.12.1 Redefinable Classes [¶](#Redefinable-Classes-1)

The ability for a class to be redefined is a choice for a class author
to make. By default, classes in GOOPS are *not* redefinable. A
redefinable class is an instance of `<redefinable-class>`; that is
to say, a class with `<redefinable-class>` as its metaclass.
Accordingly, to define a redefinable class, add `#:metaclass
<redefinable-class>` to its class definition:

```
(define-class <foo> ()
  #:metaclass <redefinable-class>)
```

Note that any subclass of `<foo>` is also redefinable, without the
need to explicitly pass the `#:metaclass` argument, so you only
need to specify `#:metaclass` for the roots of your application’s
class hierarchy.

```
(define-class <bar> (<foo>))
(class-of <bar>) ⇒ <redefinable-class>
```

Note that prior to Guile 3.0, all GOOPS classes were redefinable in
theory. In practice, attempting to, for example, redefine
`<class>` itself would almost certainly not do what you want.
Still, redefinition is an interesting capability when building
long-lived resilient systems, so GOOPS does offer this facility.

---

Next: [Customizing Class Redefinition](#Customizing-Class-Redefinition), Previous: [Redefinable Classes](#Redefinable-Classes), Up: [Redefining a Class](#Redefining-a-Class)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.12.2 Default Class Redefinition Behavior [¶](#Default-Class-Redefinition-Behavior-1)

When a class is defined using `define-class` and the class name was
previously defined, by default the new binding just replaces the old
binding. This is the normal behavior for `define`. However if
both the old and new bindings are redefinable classes (instances of
`<redefinable-class>`), then the class will be updated in place,
and its instances lazily migrated over.

The way that the class is updated and the way that the instances migrate
over are of course part of the meta-object protocol. However the
default behavior usually suffices, and it goes as follows.

* All existing direct instances of `<my-class>` are converted to be
  instances of the new class. This is achieved by preserving the values
  of slots that exist in both the old and new definitions, and
  initializing the values of new slots in the usual way (see [make](#Instance-Creation)).
* All existing subclasses of `<my-class>` are redefined, as though
  the `define-class` expressions that defined them were re-evaluated
  following the redefinition of `<my-class>`, and the class
  redefinition process described here is applied recursively to the
  redefined subclasses.
* Once all of its instances and subclasses have been updated, the class
  metaobject previously bound to the variable `<my-class>` is no
  longer needed and so can be allowed to be garbage collected.

To keep things tidy, GOOPS also needs to do a little housekeeping on
methods that are associated with the redefined class.

* Slot accessor methods for slots in the old definition should be removed
  from their generic functions. They will be replaced by accessor methods
  for the slots of the new class definition.
* Any generic function method that uses the old `<my-class>` metaobject
  as one of its formal parameter specializers must be updated to refer to
  the new `<my-class>` metaobject. (Whenever a new generic function
  method is defined, `define-method` adds the method to a list stored
  in the class metaobject for each class used as a formal parameter
  specializer, so it is easy to identify all the methods that must be
  updated when a class is redefined.)

If this class redefinition strategy strikes you as rather counter-intuitive,
bear in mind that it is derived from similar behavior in other object
systems such as CLOS, and that experience in those systems has shown it to be
very useful in practice.

Also bear in mind that, like most of GOOPS’ default behavior, it can
be customized…

---

Previous: [Default Class Redefinition Behavior](#Default-Class-Redefinition-Behavior), Up: [Redefining a Class](#Redefining-a-Class)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 8.12.3 Customizing Class Redefinition [¶](#Customizing-Class-Redefinition-1)

When `define-class` notices that a class is being redefined, it
constructs the new class metaobject as usual, then invokes the
`class-redefinition` generic function with the old and new classes
as arguments. Therefore, if the old or new classes have metaclasses
other than the default `<redefinable-class>`, class redefinition
behavior can be customized by defining a `class-redefinition`
method that is specialized for the relevant metaclasses.

generic: **class-redefinition** [¶](#index-class_002dredefinition)
:   Handle the class redefinition from old to new, and return
    the new class metaobject that should be bound to the variable specified
    by `define-class`’s first argument.

method: **class-redefinition** (old <top>) (new <class>) [¶](#index-class_002dredefinition-1)
:   Not all classes are redefinable, and not all previous bindings are
    classes. See [Redefinable Classes](#Redefinable-Classes). This default method just returns
    new.

method: **class-redefinition** (old <redefinable-class>) (new <redefinable-class>) [¶](#index-class_002dredefinition-2)
:   This method implements GOOPS’ default class redefinition behavior, as
    described in [Default Class Redefinition Behavior](#Default-Class-Redefinition-Behavior). Returns the
    metaobject for the new class definition.

The `class-redefinition` method for classes with metaclass
`<redefinable-class>` calls the following generic functions, which
could of course be individually customized.

generic: **remove-class-accessors!** old [¶](#index-remove_002dclass_002daccessors_0021)
:   The default `remove-class-accessors!` method removes the accessor
    methods of the old class from all classes which they specialize.

generic: **update-direct-method!** method old new [¶](#index-update_002ddirect_002dmethod_0021)
:   The default `update-direct-method!` method substitutes the new
    class for the old in all methods specialized to the old class.

generic: **update-direct-subclass!** subclass old new [¶](#index-update_002ddirect_002dsubclass_0021)
:   The default `update-direct-subclass!` method invokes
    `class-redefinition` recursively to handle the redefinition of
    subclasses.

An alternative class redefinition strategy could be to leave all
existing instances as instances of the old class, but accepting that the
old class is now “nameless”, since its name has been taken over by the
new definition. In this strategy, any existing subclasses could also
be left as they are, on the understanding that they inherit from a nameless
superclass.

This strategy is easily implemented in GOOPS, by defining a new metaclass,
that will be used as the metaclass for all classes to which the strategy
should apply, and then defining a `class-redefinition` method that
is specialized for this metaclass:

```
(define-class <can-be-nameless> (<redefinable-class>))

(define-method (class-redefinition (old <can-be-nameless>)
                                   (new <class>))
  new)
```

When customization can be as easy as this, aren’t you glad that GOOPS
implements the far more difficult strategy as its default!

---

Previous: [Redefining a Class](#Redefining-a-Class), Up: [GOOPS](#GOOPS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 8.13 Changing the Class of an Instance [¶](#Changing-the-Class-of-an-Instance-1)

When a redefinable class is redefined, any existing instance of the
redefined class will be modified for the new class definition before the
next time that any of the instance’s slots is referenced or set. GOOPS
modifies each instance by calling the generic function
`change-class`.

More generally, you can change the class of an existing instance at any
time by invoking the generic function `change-class` with two
arguments: the instance and the new class.

The default method for `change-class` decides how to implement the
change of class by looking at the slot definitions for the instance’s
existing class and for the new class. If the new class has slots with
the same name as slots in the existing class, the values for those slots
are preserved. Slots that are present only in the existing class are
discarded. Slots that are present only in the new class are initialized
using the corresponding slot definition’s init function (see [slot-init-function](#Classes)).

generic: **change-class** instance new-class [¶](#index-change_002dclass)

method: **change-class** (obj <object>) (new <redefinable-class>) [¶](#index-change_002dclass-1)
:   Modify instance obj to make it an instance of class new.
    obj itself must already be an instance of a redefinable class.

    The value of each of obj’s slots is preserved only if a similarly named
    slot exists in new; any other slot values are discarded.

    The slots in new that do not correspond to any of obj’s
    pre-existing slots are initialized according to new’s slot definitions’
    init functions.

The default `change-class` method also invokes another generic
function, `update-instance-for-different-class`, as the last thing
that it does before returning. The applied
`update-instance-for-different-class` method can make any further
adjustments to new-instance that are required to complete or
modify the change of class. The return value from the applied method is
ignored.

generic: **update-instance-for-different-class** old-instance new-instance [¶](#index-update_002dinstance_002dfor_002ddifferent_002dclass)
:   A generic function that can be customized to put finishing touches to an
    instance whose class has just been changed. The default
    `update-instance-for-different-class` method does nothing.

Customized change of class behavior can be implemented by defining
`change-class` methods that are specialized either by the class
of the instances to be modified or by the metaclass of the new class.

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [GOOPS](#GOOPS), Up: [The Guile Reference Manual](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]## 9 Guile Implementation [¶](#Guile-Implementation-1)

At some point, after one has been programming in Scheme for some time,
another level of Scheme comes into view: its implementation. Knowledge
of how Scheme can be implemented turns out to be necessary to become
an expert hacker. As Peter Norvig notes in his retrospective on
PAIP[36](#FOOT36), “The expert Lisp
programmer eventually develops a good ‘efficiency model’.”

By this Norvig means that over time, the Lisp hacker eventually
develops an understanding of how much her code “costs” in terms of
space and time.

This chapter describes Guile as an implementation of Scheme: its
history, how it represents and evaluates its data, and its compiler.
This knowledge can help you to make that step from being one who is
merely familiar with Scheme to being a real hacker.

* [A Brief History of Guile](#History)
* [Data Representation](#Data-Representation)
* [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)
* [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine)

---

Next: [Data Representation](#Data-Representation), Up: [Guile Implementation](#Guile-Implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
