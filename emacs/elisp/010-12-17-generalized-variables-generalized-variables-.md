### 12.17 Generalized Variables [¶](#Generalized-Variables-1)

A *generalized variable* or *place form* is one of the many
places in Lisp memory where values can be stored using the `setf`
macro (see [The `setf` Macro](#Setting-Generalized-Variables)). The simplest place
form is a regular Lisp variable. But the CARs and CDRs of
lists, elements of arrays, properties of symbols, and many other
locations are also places where Lisp values get stored.

Generalized variables are analogous to lvalues in the C
language, where ‘`x = a[i]`’ gets an element from an array
and ‘`a[i] = x`’ stores an element using the same notation.
Just as certain forms like `a[i]` can be lvalues in C, there
is a set of forms that can be generalized variables in Lisp.

* [The `setf` Macro](#Setting-Generalized-Variables)
* [Defining new `setf` forms](#Adding-Generalized-Variables)

---

Next: [Defining new `setf` forms](#Adding-Generalized-Variables), Up: [Generalized Variables](#Generalized-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.17.1 The `setf` Macro [¶](#The-setf-Macro)

The `setf` macro is the most basic way to operate on generalized
variables. The `setf` form is like `setq`, except that it
accepts arbitrary place forms in the first (left) argument of each
pair rather than just symbols. For example, `(setf (car a) b)`
sets the car of `a` to `b`, doing the same operation as
`(setcar a b)`, but without you having to use two separate
functions for setting and accessing this type of place.

Macro: **setf** [place form]… [¶](#index-setf)
:   This macro evaluates form and stores its value in place,
    which must be a valid generalized variable form. If there are several
    place and form pairs, the assignments are done sequentially
    just as with `setq`. `setf` returns the value of the last
    form.

The following Lisp forms are the forms in Emacs that will work as
generalized variables, and so may appear in the place argument
of `setf`:

* A symbol. In other words, `(setf x y)` is exactly equivalent to
  `(setq x y)`, and `setq` itself is strictly speaking
  redundant given that `setf` exists. Most programmers will
  continue to prefer `setq` for setting simple variables, though,
  for stylistic and historical reasons. The macro `(setf x y)`
  actually expands to `(setq x y)`, so there is no performance
  penalty for using it in compiled code.
* A call to any of the following standard Lisp functions:

  ```
  aref      cddr      symbol-function
  car       elt       symbol-plist
  caar      get       symbol-value
  cadr      gethash
  cdr       nth
  cdar      nthcdr
  ```
* A call to any of the following Emacs-specific functions:

  ```
  alist-get                     overlay-start
  default-value                 overlay-get
  face-background               process-buffer
  face-font                     process-filter
  face-foreground               process-get
  face-stipple                  process-sentinel
  face-underline-p              terminal-parameter
  file-modes                    window-buffer
  frame-parameter               window-dedicated-p
  frame-parameters              window-display-table
  get-register                  window-hscroll
  getenv                        window-parameter
  keymap-parent                 window-point
  match-data                    window-start
  overlay-end
  ```
* A call of the form `(substring subplace n [m])`,
  where subplace is itself a valid generalized variable whose
  current value is a string, and where the value stored is also a
  string. The new string is spliced into the specified part of the
  destination string. For example:

  ```
  (setq a (list "hello" "world"))
       ⇒ ("hello" "world")
  (cadr a)
       ⇒ "world"
  (substring (cadr a) 2 4)
       ⇒ "rl"
  (setf (substring (cadr a) 2 4) "o")
       ⇒ "o"
  (cadr a)
       ⇒ "wood"
  a
       ⇒ ("hello" "wood")
  ```
* The `if` and `cond` conditionals will work as generalized
  variables. For instance, this will set either the `foo` or the
  `bar` variable to `zot`:

  ```
  (setf (if (zerop (random 2))
  	  foo
  	bar)
        'zot)
  ```

`setf` signals an error if you pass a place form that it
does not know how to handle.

Note that for `nthcdr`, the list argument of the function must
itself be a valid place form. For example, `(setf (nthcdr
0 foo) 7)` will set `foo` itself to 7.

The macros `push` (see [Modifying List Variables](#List-Variables)) and `pop`
(see [Accessing Elements of Lists](#List-Elements)) can manipulate generalized variables,
not just lists. `(pop place)` removes and returns the first
element of the list stored in place. It is analogous to
`(prog1 (car place) (setf place (cdr place)))`,
except that it takes care to evaluate all subforms only once.
`(push x place)` inserts x at the front of
the list stored in place. It is analogous to `(setf
place (cons x place))`, except for evaluation of the
subforms. Note that `push` and `pop` on an `nthcdr`
place can be used to insert or delete at any position in a list.

Similarly, the macros `incf` and `decf` (see [Arithmetic Operations](#Arithmetic-Operations)) can be used to increment or decrement generalized
variables that are numbers.

The `cl-lib` library defines various extensions for generalized
variables, including additional `setf` places.
See [Generalized Variables](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Generalized-Variables) in Common Lisp Extensions.

---

Previous: [The `setf` Macro](#Setting-Generalized-Variables), Up: [Generalized Variables](#Generalized-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.17.2 Defining new `setf` forms [¶](#Defining-new-setf-forms)

This section describes how to define new forms that `setf` can
operate on.

Macro: **gv-define-simple-setter** name setter &optional fix-return [¶](#index-gv_002ddefine_002dsimple_002dsetter)
:   This macro enables you to easily define `setf` methods for simple
    cases. name is the name of a function, macro, or special form.
    You can use this macro whenever name has a directly
    corresponding setter function that updates it, e.g.,
    `(gv-define-simple-setter car setcar)`.

    This macro translates a call of the form

    ```
    (setf (name args...) value)
    ```

    into

    ```
    (setter args... value)
    ```

    Such a `setf` call is documented to return value. This is
    no problem with, e.g., `car` and `setcar`, because
    `setcar` returns the value that it set. If your setter
    function does not return value, use a non-`nil` value for
    the fix-return argument of `gv-define-simple-setter`. This
    expands into something equivalent to

    ```
    (let ((temp value))
      (setter args... temp)
      temp)
    ```

    so ensuring that it returns the correct result.

Macro: **gv-define-setter** name arglist &rest body [¶](#index-gv_002ddefine_002dsetter)
:   This macro allows for more complex `setf` expansions than the
    previous form. You may need to use this form, for example, if there
    is no simple setter function to call, or if there is one but it
    requires different arguments to the place form.

    This macro expands the form
    `(setf (name args…) value)` by
    first binding the `setf` argument forms
    `(value args…)` according to arglist,
    and then executing body. body should return a Lisp
    form that does the assignment, and finally returns the value that was
    set. An example of using this macro is:

    ```
    (gv-define-setter caar (val x) `(setcar (car ,x) ,val))
    ```

Macro: **gv-define-expander** name handler [¶](#index-gv_002ddefine_002dexpander)
:   For more control over the expansion, the `gv-define-expander`
    macro can be used. For instance, a settable `substring` could be
    implemented this way:

    ```
    (gv-define-expander substring
      (lambda (do place from &optional to)
        (gv-letplace (getter setter) place
          (macroexp-let2* (from to)
            (funcall do `(substring ,getter ,from ,to)
                     (lambda (v)
                       (macroexp-let2* (v)
                         `(progn
                            ,(funcall setter `(cl--set-substring
                                               ,getter ,from ,to ,v))
                            ,v))))))))
    ```

Macro: **gv-letplace** (getter setter) place &rest body [¶](#index-gv_002dletplace)
:   The macro `gv-letplace` can be useful in defining macros that
    perform similarly to `setf`; for example, the `incf` macro
    of Common Lisp could be implemented this way:

    ```
    (defmacro incf (place &optional n)
      (gv-letplace (getter setter) place
        (macroexp-let2* ((v (or n 1)))
          (funcall setter `(+ ,v ,getter)))))
    ```

    getter will be bound to a copyable expression that returns the
    value of place. setter will be bound to a function that
    takes an expression v and returns a new expression that sets
    place to v. body should return a Emacs Lisp
    expression manipulating place via getter and setter.

Consult the source file `gv.el` for more details.

Function: **make-obsolete-generalized-variable** obsolete-name current-name when [¶](#index-make_002dobsolete_002dgeneralized_002dvariable)
:   This function makes the byte compiler warn that the generalized
    variable obsolete-name is obsolete. If current-name is a
    symbol, then the warning message says to use current-name
    instead of obsolete-name. If current-name is a string,
    this is the message. when should be a string indicating when
    the variable was first made obsolete (usually a version number
    string).

> **Common Lisp note:** Common Lisp defines another way to specify the
> `setf` behavior of a function, namely `setf` functions,
> whose names are lists `(setf name)` rather than symbols.
> For example, `(defun (setf foo) …)` defines the function
> that is used when `setf` is applied to `foo`. Emacs does
> not support this. It is a compile-time error to use `setf` on a
> form that has not already had an appropriate expansion defined. In
> Common Lisp, this is not an error since the function `(setf
> func)` might be defined later.

---

Previous: [Generalized Variables](#Generalized-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.18 Multisession Variables [¶](#Multisession-Variables-1)

When you set a variable to a value and then close Emacs and restart
it, that value won’t be automatically restored. Users usually set
normal variables in their startup files, or use Customize
(see [Customization Settings](#Customization)) to set user options permanently, and various
packages have various files where they store the data (e.g., Gnus
stores this in `.newsrc.eld` and the URL library stores cookies
in `~/.emacs.d/url/cookies`).

For things in between these two extremes (i.e., configuration which
goes in the startup file, and massive application state that goes into
separate files), Emacs provides a facility to replicate data between
sessions called *multisession variables*. (This facility may not
be available on all systems.) To give you an idea of how these are
meant to be used, here’s a small example:

```
(define-multisession-variable foo 0)
(defun my-adder (num)
  (interactive "nAdd number: ")
  (setf (multisession-value foo)
        (+ (multisession-value foo) num))
  (message "The new number is: %s" (multisession-value foo)))
```

This defines the variable `foo` and binds it to a special
multisession object which is initialized with the value ‘`0`’ (if
the variable doesn’t already exist from a previous session). The
`my-adder` command queries the user for a number, adds this to
the old (possibly saved value), and then saves the new value.

This facility isn’t meant to be used for huge data structures, but
should be performant for most values.

Macro: **define-multisession-variable** name initial-value &optional doc &rest args [¶](#index-define_002dmultisession_002dvariable)
:   This macro defines name as a multisession variable, and gives it
    the initial-value if this variable hasn’t been assigned a value
    earlier. doc is the doc string, and several keyword arguments can
    be used in args:

    `:package package-symbol`
    :   This keyword says that a multisession variable belongs to the package
        specified by package-symbol. The combination of
        package-symbol and name has to be unique. If
        package-symbol isn’t given, this will default to the first
        “segment” of the name symbol’s name, which is the part of its
        name up to and excluding the first ‘`-`’. For instance, if
        name is `foo` and package-symbol isn’t given,
        package-symbol will default to `foo`.

    `:synchronized bool` [¶](#index-synchronized-multisession-variables)
    :   Multisession variables can be *synchronized* if bool is
        non-`nil`. This means that if there’re two concurrent Emacs
        instances running, and the other Emacs changes the multisession
        variable `foo`, the current Emacs instance will retrieve that
        modified data when accessing the value. If synchronized is
        `nil` or missing, this won’t happen, and the values in all
        Emacs sessions using the variable will be independent of each other.

    `:storage storage`
    :   Use the specified storage method. This can be either
        `sqlite` (in Emacs compiled with SQLite support) or `files`.
        If not given, this defaults to the value of the
        `multisession-storage` variable, described below.

Function: **multisession-value** variable [¶](#index-multisession_002dvalue)
:   This function returns the current value of variable. If this
    variable hasn’t been accessed before in this Emacs session, or if it’s
    changed externally, it will be read in from external storage. If not,
    the current value in this session is returned as is. It is an error
    to call this function for a variable that is not a multisession
    variable.

    Values retrieved via `multisession-value` may or may not be
    `eq` to each other, but they will always be `equal`.

    This is a generalized variable (see [Generalized Variables](#Generalized-Variables)), so the
    way to update such a variable is to say, for instance:

    ```
    (setf (multisession-value foo-bar) 'zot)
    ```

    Only Emacs Lisp values that have a readable print syntax
    (see [Printed Representation and Read Syntax](#Printed-Representation)) can be saved this way.

    If the multisession variable is synchronized, setting it may update
    the value first. For instance:

    ```
    (incf (multisession-value foo-bar))
    ```

    This first checks whether the value has changed in a different
    Emacs instance, retrieves that value, and then adds 1 to that value and
    stores it. But note that this is done without locking, so if many
    instances are updating the value at the same time, it’s unpredictable
    which instance “wins”.

Function: **multisession-delete** object [¶](#index-multisession_002ddelete)
:   This function deletes object and its value from its persistent
    storage.

Function: **make-multisession** [¶](#index-make_002dmultisession)
:   You can also make persistent values that aren’t tied to a specific
    variable, but are tied to an explicit package and key.

    ```
    (setq foo (make-multisession :package "mail"
                                 :key "friends"))
    (setf (multisession-value foo) 'everybody)
    ```

    This supports the same keywords as
    `define-multisession-variable`, but also supports a
    `:initial-value` keyword, which specifies the default value.

User Option: **multisession-storage** [¶](#index-multisession_002dstorage)
:   This variable controls how the multisession variables are stored. It
    value defaults to `files`, which means that the values are stored
    in a one-file-per-variable structure inside the directory specified by
    `multisession-directory`. If this value is `sqlite`
    instead, the values are stored in an SQLite database; this is only
    available if Emacs was built with SQLite support.

User Option: **multisession-directory** [¶](#index-multisession_002ddirectory)
:   The multisession variables are stored under this directory, which
    defaults to `multisession/` subdirectory of the
    `user-emacs-directory`, which is typically
    `~/.emacs.d/multisession/`.

Command: **list-multisession-values** [¶](#index-list_002dmultisession_002dvalues)
:   This command pops up a buffer listing all the multisession variables,
    and enters a special mode `multisession-edit-mode` which allows
    you to delete them and edit their values.

---

Next: [Macros](#Macros), Previous: [Variables](#Variables), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 13 Functions [¶](#Functions-1)

A Lisp program is composed mainly of Lisp functions. This chapter
explains what functions are, how they accept arguments, and how to
define them.

* [What Is a Function?](#What-Is-a-Function)
* [Lambda Expressions](#Lambda-Expressions)
* [Naming a Function](#Function-Names)
* [Defining Functions](#Defining-Functions)
* [Calling Functions](#Calling-Functions)
* [Mapping Functions](#Mapping-Functions)
* [Anonymous Functions](#Anonymous-Functions)
* [Generic Functions](#Generic-Functions)
* [Accessing Function Cell Contents](#Function-Cells)
* [Closures](#Closures)
* [Open Closures](#OClosures)
* [Advising Emacs Lisp Functions](#Advising-Functions)
* [Declaring Functions Obsolete](#Obsolete-Functions)
* [Inline Functions](#Inline-Functions)
* [The `declare` Form](#Declare-Form)
* [Telling the Compiler that a Function is Defined](#Declaring-Functions)
* [Determining whether a Function is Safe to Call](#Function-Safety)
* [Other Topics Related to Functions](#Related-Topics)

---

Next: [Lambda Expressions](#Lambda-Expressions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.1 What Is a Function? [¶](#What-Is-a-Function_003f)

In a general sense, a function is a rule for carrying out a
computation given input values called *arguments*. The result of
the computation is called the *value* or *return value* of the
function. The computation can also have side effects, such as lasting
changes in the values of variables or the contents of data structures
(see [Definition of side effect](#Definition-of-side-effect)). A *pure function* is a
function which, in addition to having no side effects, always returns
the same value for the same combination of arguments, regardless of
external factors such as machine type or system state.

In most computer languages, every function has a name. But in Lisp,
a function in the strictest sense has no name: it is an object which
can *optionally* be associated with a symbol (e.g., `car`)
that serves as the function name. See [Naming a Function](#Function-Names). When a
function has been given a name, we usually also refer to that symbol
as a “function” (e.g., we refer to “the function `car`”).
In this manual, the distinction between a function name and the
function object itself is usually unimportant, but we will take note
wherever it is relevant.

Certain function-like objects, called *special forms* and
*macros*, also accept arguments to carry out computations.
However, as explained below, these are not considered functions in
Emacs Lisp.

Here are important terms for functions and function-like objects:

*lambda expression*
:   A function (in the strict sense, i.e., a function object) which is
    written in Lisp. These are described in the following section.
    See [Lambda Expressions](#Lambda-Expressions).

*primitive* [¶](#index-primitive)
:   A function which is callable from Lisp but is actually written in C.
    Primitives are also called *built-in functions*, or *subrs*.
    Examples include functions like `car` and `append`. In
    addition, all special forms (see below) are also considered
    primitives.

    Usually, a function is implemented as a primitive because it is a
    fundamental part of Lisp (e.g., `car`), or because it provides a
    low-level interface to operating system services, or because it needs
    to run fast. Unlike functions defined in Lisp, primitives can be
    modified or added only by changing the C sources and recompiling
    Emacs. See [Writing Emacs Primitives](#Writing-Emacs-Primitives).

*special form*
:   A primitive that is like a function but does not evaluate all of its
    arguments in the usual way. It may evaluate only some of the
    arguments, or may evaluate them in an unusual order, or several times.
    Examples include `if`, `and`, and `while`.
    See [Special Forms](#Special-Forms).

*macro* [¶](#index-macro)
:   A construct defined in Lisp, which differs from a function in that it
    translates a Lisp expression into another expression which is to be
    evaluated instead of the original expression. Macros enable Lisp
    programmers to do the sorts of things that special forms can do.
    See [Macros](#Macros).

*command* [¶](#index-command)
:   An object which can be invoked via the `command-execute`
    primitive, usually due to the user typing in a key sequence
    *bound* to that command. See [Interactive Call](#Interactive-Call). A command is
    usually a function; if the function is written in Lisp, it is made
    into a command by an `interactive` form in the function
    definition (see [Defining Commands](#Defining-Commands)). Commands that are functions
    can also be called from Lisp expressions, just like other functions.

    Keyboard macros (strings and vectors) are commands also, even though
    they are not functions. See [Keyboard Macros](#Keyboard-Macros). We say that a symbol
    is a command if its function cell contains a command (see [Symbol Components](#Symbol-Components)); such a *named command* can be invoked with
    `M-x`.

*closure*
:   A function object that is much like a lambda expression, except that
    it also encloses an environment of lexical variable bindings.
    See [Closures](#Closures).

*byte-code function*
:   A function that has been compiled by the byte compiler.
    See [Closure Function Type](#Closure-Type).

*autoload object* [¶](#index-autoload-object)
:   A place-holder for a real function. If the autoload object is called,
    Emacs loads the file containing the definition of the real function,
    and then calls the real function. See [Autoload](#Autoload).

You can use the function `functionp` to test if an object is a
function:

Function: **functionp** object [¶](#index-functionp)
:   This function returns `t` if object is any kind of
    function, i.e., can be passed to `funcall`. Note that
    `functionp` returns `t` for symbols that are function names,
    and returns `nil` for symbols that are macros or special forms.

    If object is not a function, this function ordinarily returns
    `nil`. However, the representation of function objects is
    complicated, and for efficiency reasons in rare cases this function
    can return `t` even when object is not a function.

It is also possible to find out how many arguments an arbitrary
function expects:

Function: **func-arity** function [¶](#index-func_002darity)
:   This function provides information about the argument list of the
    specified function. The returned value is a cons cell of the
    form `(min . max)`, where min is the
    minimum number of arguments, and max is either the maximum
    number of arguments, or the symbol `many` for functions with
    `&rest` arguments, or the symbol `unevalled` if
    function is a special form.

    Note that this function might return inaccurate results in some
    situations, such as the following:

    * Functions defined using `apply-partially` (see [apply-partially](#Calling-Functions)).
    * Functions that are advised using `advice-add` (see [Advising Named Functions](#Advising-Named-Functions)).
    * Functions that determine the argument list dynamically, as part of
      their code.

Unlike `functionp`, the next functions do *not* treat a symbol
as its function definition.

Function: **subrp** object [¶](#index-subrp)
:   This function returns `t` if object is a built-in function
    (i.e., a Lisp primitive).

    ```
    (subrp 'message)            ; message is a symbol,
         ⇒ nil                 ;   not a subr object.
    ```

    ```
    (subrp (symbol-function 'message))
         ⇒ t
    ```

Function: **byte-code-function-p** object [¶](#index-byte_002dcode_002dfunction_002dp)
:   This function returns `t` if object is a byte-code
    function. For example:

    ```
    (byte-code-function-p (symbol-function 'next-line))
         ⇒ t
    ```

Function: **compiled-function-p** object [¶](#index-compiled_002dfunction_002dp)
:   This function returns `t` if object is a function object
    that is not in the form of Lisp source code but something like
    machine code or byte code instead. More specifically it returns
    `t` if the function is built-in (a.k.a. “primitive”,
    see [What Is a Function?](#What-Is-a-Function)), or byte-compiled (see [Byte Compilation](#Byte-Compilation)), or native-compiled (see [Compilation of Lisp to Native Code](#Native-Compilation)), or
    a function loaded from a dynamic module (see [Emacs Dynamic Modules](#Dynamic-Modules)).

Function: **interpreted-function-p** object [¶](#index-interpreted_002dfunction_002dp)
:   This function returns `t` if object is an interpreted function.

Function: **closurep** object [¶](#index-closurep)
:   This function returns `t` if object is a closure, which is
    a particular kind of function object. Currently closures are used
    for all byte-code functions and all interpreted functions.

Function: **subr-arity** subr [¶](#index-subr_002darity)
:   This works like `func-arity`, but only for built-in functions and
    without symbol indirection. It signals an error for non-built-in
    functions. We recommend to use `func-arity` instead.

Function: **cl-functionp** object [¶](#index-cl_002dfunctionp)
:   This function is like `functionp`, except it returns `nil` for
    lists and symbols.

Function: **primitive-function-p** object [¶](#index-primitive_002dfunction_002dp)
:   This function returns `t` if object is a built-in primitive
    written in C (see [Primitive Function Type](#Primitive-Function-Type)). Note that special forms
    are explicitly excluded, as they are not functions. Use
    `subr-primitive-p` if you need to recognize special forms as well.

---

Next: [Naming a Function](#Function-Names), Previous: [What Is a Function?](#What-Is-a-Function), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.2 Lambda Expressions [¶](#Lambda-Expressions-1)

A lambda expression is a function object written in Lisp. Here is
an example:

```
(lambda (x)
  "Return the hyperbolic cosine of X."
  (* 0.5 (+ (exp x) (exp (- x)))))
```

In Emacs Lisp, such a list is a valid expression which evaluates to
a function object.

A lambda expression, by itself, has no name; it is an *anonymous
function*. Although lambda expressions can be used this way
(see [Anonymous Functions](#Anonymous-Functions)), they are more commonly associated with
symbols to make *named functions* (see [Naming a Function](#Function-Names)).
Before going into these details, the following subsections describe
the components of a lambda expression and what they do.

* [Components of a Lambda Expression](#Lambda-Components)
* [A Simple Lambda Expression Example](#Simple-Lambda)
* [Features of Argument Lists](#Argument-List)
* [Documentation Strings of Functions](#Function-Documentation)

---

Next: [A Simple Lambda Expression Example](#Simple-Lambda), Up: [Lambda Expressions](#Lambda-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.2.1 Components of a Lambda Expression [¶](#Components-of-a-Lambda-Expression)

A lambda expression is a list that looks like this:

```
(lambda (arg-variables...)
  [documentation-string]
  [interactive-declaration]
  body-forms...)
```

The first element of a lambda expression is always the symbol
`lambda`. This indicates that the list represents a function. The
reason functions are defined to start with `lambda` is so that
other lists, intended for other uses, will not accidentally be valid as
functions.

The second element is a list of symbols—the argument variable
names (see [Features of Argument Lists](#Argument-List)).
This is called the *lambda list*. When a Lisp function is called,
the argument values are matched up against the variables in the lambda
list, which are given local bindings with the values provided.
See [Local Variables](#Local-Variables).

The documentation string is a Lisp string object placed within the
function definition to describe the function for the Emacs help
facilities. See [Documentation Strings of Functions](#Function-Documentation).

The interactive declaration is a list of the form `(interactive
code-string)`. This declares how to provide arguments if the
function is used interactively. Functions with this declaration are called
*commands*; they can be called using `M-x` or bound to a key.
Functions not intended to be called in this way should not have interactive
declarations. See [Defining Commands](#Defining-Commands), for how to write an interactive
declaration.

The rest of the elements are the *body* of the function: the Lisp
code to do the work of the function (or, as a Lisp programmer would say,
“a list of Lisp forms to evaluate”). The value returned by the
function is the value returned by the last element of the body.

---

Next: [Features of Argument Lists](#Argument-List), Previous: [Components of a Lambda Expression](#Lambda-Components), Up: [Lambda Expressions](#Lambda-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.2.2 A Simple Lambda Expression Example [¶](#A-Simple-Lambda-Expression-Example)

Consider the following example:

```
(lambda (a b c) (+ a b c))
```

We can call this function by passing it to `funcall`, like this:

```
(funcall (lambda (a b c) (+ a b c))
         1 2 3)
```

This call evaluates the body of the lambda expression with the variable
`a` bound to 1, `b` bound to 2, and `c` bound to 3.
Evaluation of the body adds these three numbers, producing the result 6;
therefore, this call to the function returns the value 6.

Note that the arguments can be the results of other function calls, as in
this example:

```
(funcall (lambda (a b c) (+ a b c))
         1 (* 2 3) (- 5 4))
```

This evaluates the arguments `1`, `(* 2 3)`, and `(- 5
4)` from left to right. Then it applies the lambda expression to the
argument values 1, 6 and 1 to produce the value 8.

As these examples show, you can use a form with a lambda expression
as its CAR to make local variables and give them values. In the
old days of Lisp, this technique was the only way to bind and
initialize local variables. But nowadays, it is clearer to use the
special form `let` for this purpose (see [Local Variables](#Local-Variables)).
Lambda expressions are mainly used as anonymous functions for passing
as arguments to other functions (see [Anonymous Functions](#Anonymous-Functions)), or
stored as symbol function definitions to produce named functions
(see [Naming a Function](#Function-Names)).

---

Next: [Documentation Strings of Functions](#Function-Documentation), Previous: [A Simple Lambda Expression Example](#Simple-Lambda), Up: [Lambda Expressions](#Lambda-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.2.3 Features of Argument Lists [¶](#Features-of-Argument-Lists)

Our simple sample function, `(lambda (a b c) (+ a b c))`,
specifies three argument variables, so it must be called with three
arguments: if you try to call it with only two arguments or four
arguments, you get a `wrong-number-of-arguments` error
(see [Errors](#Errors)).

It is often convenient to write a function that allows certain
arguments to be omitted. For example, the function `substring`
accepts three arguments—a string, the start index and the end
index—but the third argument defaults to the length of the
string if you omit it. It is also convenient for certain functions to
accept an indefinite number of arguments, as the functions `list`
and `+` do.

To specify optional arguments that may be omitted when a function
is called, simply include the keyword `&optional` before the optional
arguments. To specify a list of zero or more extra arguments, include the
keyword `&rest` before one final argument.

Thus, the complete syntax for an argument list is as follows:

```
(required-vars...
 [&optional [optional-vars...]]
 [&rest rest-var])
```

The square brackets indicate that the `&optional` and `&rest`
clauses, and the variables that follow them, are optional.

A call to the function requires one actual argument for each of the
required-vars. There may be actual arguments for zero or more of
the optional-vars, and there cannot be any actual arguments beyond
that unless the lambda list uses `&rest`. In that case, there may
be any number of extra actual arguments.

If actual arguments for the optional and rest variables are omitted,
then they always default to `nil`. There is no way for the
function to distinguish between an explicit argument of `nil` and
an omitted argument. However, the body of the function is free to
consider `nil` an abbreviation for some other meaningful value.
This is what `substring` does; `nil` as the third argument to
`substring` means to use the length of the string supplied.

> **Common Lisp note:** Common Lisp allows the function to specify what
> default value to use when an optional argument is omitted; Emacs Lisp
> always uses `nil`. Emacs Lisp does not support `supplied-p`
> variables that tell you whether an argument was explicitly passed.

For example, an argument list that looks like this:

```
(a b &optional c d &rest e)
```

binds `a` and `b` to the first two actual arguments, which are
required. If one or two more arguments are provided, `c` and
`d` are bound to them respectively; any arguments after the first
four are collected into a list and `e` is bound to that list.
Thus, if there are only two arguments, `c`, `d` and `e`
are `nil`; if two or three arguments, `d` and `e` are
`nil`; if four arguments or fewer, `e` is `nil`. Note
that exactly five arguments with an explicit `nil` argument
provided for `e` will cause that `nil` argument to be passed
as a list with one element, `(nil)`, as with any other single
value for `e`.

There is no way to have required arguments following optional
ones—it would not make sense. To see why this must be so, suppose
that `c` in the example were optional and `d` were required.
Suppose three actual arguments are given; which variable would the
third argument be for? Would it be used for the c, or for
d? One can argue for both possibilities. Similarly, it makes
no sense to have any more arguments (either required or optional)
after a `&rest` argument.

Here are some examples of argument lists and proper calls:

```
(funcall (lambda (n) (1+ n))        ; One required:
         1)                         ; requires exactly one argument.
     ⇒ 2
(funcall (lambda (n &optional n1)   ; One required and one optional:
           (if n1 (+ n n1) (1+ n))) ; 1 or 2 arguments.
         1 2)
     ⇒ 3
(funcall (lambda (n &rest ns)       ; One required and one rest:
           (+ n (apply '+ ns)))     ; 1 or more arguments.
         1 2 3 4 5)
     ⇒ 15
```

---

Previous: [Features of Argument Lists](#Argument-List), Up: [Lambda Expressions](#Lambda-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.2.4 Documentation Strings of Functions [¶](#Documentation-Strings-of-Functions)

A lambda expression may optionally have a *documentation string*
just after the lambda list. This string does not affect execution of
the function; it is a kind of comment, but a systematized comment
which actually appears inside the Lisp world and can be used by the
Emacs help facilities. See [Documentation](#Documentation), for how the
documentation string is accessed.

It is a good idea to provide documentation strings for all the
functions in your program, even those that are called only from within
your program. Documentation strings are like comments, except that they
are easier to access.

The first line of the documentation string should stand on its own,
because `apropos` displays just this first line. It should consist
of one or two complete sentences that summarize the function’s purpose.

The start of the documentation string is usually indented in the
source file, but since these spaces come before the starting
double-quote, they are not part of the string. Some people make a
practice of indenting any additional lines of the string so that the
text lines up in the program source. *That is a mistake.* The
indentation of the following lines is inside the string; what looks
nice in the source code will look ugly when displayed by the help
commands.

A documentation string must always be followed by at least one Lisp
expression; otherwise, it is not a documentation string at all but the
single expression of the body and used as the return value.
When there is no meaningful value to return from a function, it is
standard practice to return `nil` by adding it after the
documentation string.

The last line of the documentation string can specify calling
conventions different from the actual function arguments. Write
text like this:

```
\(fn arglist)
```

following a blank line, at the beginning of the line, with no newline
following it inside the documentation string. (The ‘`\`’ is used
to avoid confusing the Emacs motion commands.) The calling convention
specified in this way appears in help messages in place of the one
derived from the actual arguments of the function.

This feature is particularly useful for macro definitions, since the
arguments written in a macro definition often do not correspond to the
way users think of the parts of the macro call.

Do not use this feature if you want to deprecate the calling
convention and favor the one you advertise by the above specification.
Instead, use the `advertised-calling-convention` declaration
(see [The `declare` Form](#Declare-Form)) or `set-advertised-calling-convention`
(see [Declaring Functions Obsolete](#Obsolete-Functions)), because these two will cause the byte
compiler emit a warning message when it compiles Lisp programs which
use the deprecated calling convention.

The `(fn)` feature is typically used in the following situations:

* To spell out arguments and their purposes in a macro or a function. Example:

  ```
  (defmacro lambda (&rest cdr)
    "...
  \(fn ARGS [DOCSTRING] [INTERACTIVE] BODY)"...)
  ```
* To provide a more detailed description and names of arguments. Example:

  ```
  (defmacro macroexp--accumulate (var+list &rest body)
    "...
  \(fn (VAR LIST) BODY...)"
    (declare (indent 1))
    (let ((var (car var+list))
  	  (list (cadr var+list))
  ...)))
  ```
* To better explain the purpose of a `defalias`. Example:

  ```
  (defalias 'abbrev-get 'get
    "...
  \(fn ABBREV PROP)")
  ```

Documentation strings are usually static, but occasionally it can be
necessary to generate them dynamically. In some cases you can do so
by writing a macro which generates at compile time the code of the
function, including the desired documentation string. But you can
also generate the docstring dynamically by writing
`(:documentation form)` instead of the documentation
string. This will evaluate form at run-time when the function
is defined and use it as the documentation string[15](#FOOT15). You can also compute
the documentation string on the fly when it is requested, by setting
the `function-documentation` property of the function’s symbol to
a Lisp form that evaluates to a string.

For example:

```
(defun adder (x)
  (lambda (y)
    (:documentation (format "Add %S to the argument Y." x))
    (+ x y)))
(defalias 'adder5 (adder 5))
(documentation 'adder5)
    ⇒ "Add 5 to the argument Y."
```

```

```

```
(put 'adder5 'function-documentation
     '(concat (documentation (symbol-function 'adder5) 'raw)
              "  Consulted at " (format-time-string "%H:%M:%S")))
(documentation 'adder5)
    ⇒ "Add 5 to the argument Y.  Consulted at 15:52:13"
(documentation 'adder5)
    ⇒ "Add 5 to the argument Y.  Consulted at 15:52:18"
```

---

Next: [Defining Functions](#Defining-Functions), Previous: [Lambda Expressions](#Lambda-Expressions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.3 Naming a Function [¶](#Naming-a-Function)

A symbol can serve as the name of a function. This happens when the
symbol’s *function cell* (see [Symbol Components](#Symbol-Components)) contains a
function object (e.g., a lambda expression). Then the symbol itself
becomes a valid, callable function, equivalent to the function object
in its function cell.

The contents of the function cell are also called the symbol’s
*function definition*. The procedure of using a symbol’s function
definition in place of the symbol is called *symbol function
indirection*; see [Symbol Function Indirection](#Function-Indirection). If you have not given a
symbol a function definition, its function cell is said to be
*void*, and it cannot be used as a function.

In practice, nearly all functions have names, and are referred to by
their names. You can create a named Lisp function by defining a
lambda expression and putting it in a function cell (see [Accessing Function Cell Contents](#Function-Cells)). However, it is more common to use the `defun` macro,
described in the next section.
See [Defining Functions](#Defining-Functions).

We give functions names because it is convenient to refer to them by
their names in Lisp expressions. Also, a named Lisp function can
easily refer to itself—it can be recursive. Furthermore, primitives
can only be referred to textually by their names, since primitive
function objects (see [Primitive Function Type](#Primitive-Function-Type)) have no read
syntax.

A function need not have a unique name. A given function object
*usually* appears in the function cell of only one symbol, but
this is just a convention. It is easy to store it in several symbols
using `fset`; then each of the symbols is a valid name for the
same function.

Note that a symbol used as a function name may also be used as a
variable; these two uses of a symbol are independent and do not
conflict. (This is not the case in some dialects of Lisp, like
Scheme.)

By convention, if a function’s symbol consists of two names
separated by ‘`--`’, the function is intended for internal use and
the first part names the file defining the function. For example, a
function named `vc-git--rev-parse` is an internal function
defined in `vc-git.el`. Internal-use functions written in C have
names ending in ‘`-internal`’, e.g., `bury-buffer-internal`.
Emacs code contributed before 2018 may follow other internal-use
naming conventions, which are being phased out.

---

Next: [Calling Functions](#Calling-Functions), Previous: [Naming a Function](#Function-Names), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.4 Defining Functions [¶](#Defining-Functions-1)

We usually give a name to a function when it is first created. This
is called *defining a function*, and we usually do it with the
`defun` macro. This section also describes other ways to define
a function.

Macro: **defun** name args [doc] [declare] [interactive] body… [¶](#index-defun)
:   `defun` is the usual way to define new Lisp functions. It
    defines the symbol name as a function with argument list
    args (see [Features of Argument Lists](#Argument-List)) and body forms given by body.
    Neither name nor args should be quoted.

    doc, if present, should be a string specifying the function’s
    documentation string (see [Documentation Strings of Functions](#Function-Documentation)). declare,
    if present, should be a `declare` form specifying function
    metadata (see [The `declare` Form](#Declare-Form)). interactive, if present,
    should be an `interactive` form specifying how the function is to
    be called interactively (see [Interactive Call](#Interactive-Call)).

    The return value of `defun` is undefined.

    Here are some examples:

    ```
    (defun foo () 5)
    (foo)
         ⇒ 5
    ```

    ```

    ```

    ```
    (defun bar (a &optional b &rest c)
        (list a b c))
    (bar 1 2 3 4 5)
         ⇒ (1 2 (3 4 5))
    ```

    ```
    (bar 1)
         ⇒ (1 nil nil)
    ```

    ```
    (bar)
    error→ Wrong number of arguments.
    ```

    ```

    ```

    ```
    (defun capitalize-backwards ()
      "Upcase the last letter of the word at point."
      (interactive)
      (backward-word 1)
      (forward-word 1)
      (backward-char 1)
      (capitalize-word 1))
    ```

    Most Emacs functions are part of the source code of Lisp programs, and
    are defined when the Emacs Lisp reader reads the program source before
    executing it. However, you can also define functions dynamically at
    run time, e.g., by generating `defun` calls when your program’s
    code is executed. If you do this, be aware that Emacs’s Help
    commands, such as `C-h f`, which present in the `*Help*`
    buffer a button to jump to the function’s definition, might be unable
    to find the source code because generating a function dynamically
    usually looks very different from the usual static calls to
    `defun`. You can make the job of finding the code that
    generates such functions easier by using the `definition-name`
    or `find-function-type-alist` property, see [Standard Symbol Properties](#Standard-Properties).

    Be careful not to redefine existing functions unintentionally.
    `defun` redefines even primitive functions such as `car`
    without any hesitation or notification. Emacs does not prevent you
    from doing this, because redefining a function is sometimes done
    deliberately, and there is no way to distinguish deliberate
    redefinition from unintentional redefinition.

Function: **defalias** name definition &optional doc [¶](#index-defalias)
:   This function defines the symbol name as a function, with
    definition definition. definition can be any valid Lisp
    function or macro, or a special form (see [Special Forms](#Special-Forms)), or a
    keymap (see [Keymaps](#Keymaps)), or a vector or string (a keyboard macro).
    The return value of `defalias` is *undefined*.

    If doc is non-`nil`, it becomes the function documentation
    of name. Otherwise, any documentation provided by
    definition is used.

    Internally, `defalias` normally uses `fset` to set the definition.
    If name has a `defalias-fset-function` property, however,
    the associated value is used as a function to call in place of `fset`.

    The proper place to use `defalias` is where a specific function
    or macro name is being defined—especially where that name appears
    explicitly in the source file being loaded. This is because
    `defalias` records which file defined the function, just like
    `defun` (see [Unloading](#Unloading)).

    By contrast, in programs that manipulate function definitions for other
    purposes, it is better to use `fset`, which does not keep such
    records. See [Accessing Function Cell Contents](#Function-Cells).

    If the resulting function definition chain would be circular, then
    Emacs will signal a `cyclic-function-indirection` error.

Function: **function-alias-p** object [¶](#index-function_002dalias_002dp)
:   Checks whether object is a function alias. If it is, it returns
    a list of symbols representing the function alias chain, else
    `nil`. For instance, if `a` is an alias for `b`, and
    `b` is an alias for `c`:

    ```
    (function-alias-p 'a)
        ⇒ (b c)
    ```

    There is also a second, optional argument that is obsolete and has no
    effect.

You cannot create a new primitive function with `defun` or
`defalias`, but you can use them to change the function definition of
any symbol, even one such as `car` or `x-popup-menu` whose
normal definition is a primitive. However, this is risky: for
instance, it is next to impossible to redefine `car` without
breaking Lisp completely. Redefining an obscure function such as
`x-popup-menu` is less dangerous, but it still may not work as
you expect. If there are calls to the primitive from C code, they
call the primitive’s C definition directly, so changing the symbol’s
definition will have no effect on them.

See also `defsubst`, which defines a function like `defun`
and tells the Lisp compiler to perform inline expansion on it.
See [Inline Functions](#Inline-Functions).

To undefine a function name, use `fmakunbound`.
See [Accessing Function Cell Contents](#Function-Cells).

* [Finding Definitions](#Finding-Definitions)

---

Up: [Defining Functions](#Defining-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.4.1 Finding Definitions [¶](#Finding-Definitions-1)

Tools such as Emacs’s built-in help command `C-h f` (see [Help](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Help) in The GNU Emacs Manual) can find the definition site of functions
and other Lisp objects in the source code. To do this, they use the
variable `find-function-regexp-alist`.

The alist `find-function-regexp-alist` associates object types with
a regexp or a function that finds the definition of that object in its
source file. Each element’s `car` is a symbol that describes the
type of object, or `nil`, which means a function defined with
`defun`. Each element’s `cdr` can be one of the following:

* A symbol whose value is a string interpreted as a regexp.
* A symbol naming a function that can find the definition.
* A cons cell where the `car` is a regexp (or function that returns
  one) and the `cdr` is *form-matcher*: a function that creates a
  matcher for macro-expanded forms.

A regexp string is actually a format string, and `%s` will be
substituted with the name of the symbol we are looking for.

A function will be called with one argument, the (symbol for) the object
we are searching for.

The form-matcher function in a cons cell value is called with one
argument (the symbol being sought) and should return a function; that
function should take a form and return non-`nil` if the form
defines the sought symbol. This is useful for finding definitions that
are created by expansion of macros.

If the function to be found is defined by a macro, it may be hard for
Emacs to find the definition site in the source code. A macro call may
have an unexpected look to it, and `find-function-regexp-alist`
will fail to identify the definition.

For example, a `define-derived-mode` (see [Defining Derived Modes](#Derived-Modes)) might
define a mode-specific function or a variable implicitly; or your Lisp
program might generate a run-time call to `defun` to define a
function. In these and similar cases, the `definition-name`
property of the symbol should be another symbol whose definition can be
found by textual search and whose code defines the original symbol. In
the example with `define-derived-mode`, the value of this property
of the functions and variables it defines should be the mode symbol.

In some cases, the definition cannot be found by looking for the
definition of another symbol. For example, a test file might use a
macro to generate calls to `ert-deftest` (see [How to Write
Tests](https://www.gnu.org/software/emacs/manual/html_mono/ert.html#How-to-Write-Tests) in ERT: Emacs Lisp Regression Testing) where the code is
boiler plate and only varying data need to be passed in. In such cases,
the `find-function-type-alist` property of the symbol can be an
alist that augments `find-function-regexp-alist` telling how to
find the definition of symbols of this type.

The `find-function-regexp-alist` property is most easily maintained
with the convenience function `find-function-update-type-alist`.

Function: **find-function-update-type-alist** symbol type variable [¶](#index-find_002dfunction_002dupdate_002dtype_002dalist)
:   Update property `find-function-type-alist` of symbol with
    a new element containing key type and value variable.

In the example of a macro defining calls to `ert-deftest`,
the macro could put the property `find-function-type-alist` on each
test defined, associating `ert--test` (the internal type of ERT
tests) with the name of a regexp or function that can find the correct
macro call. The file defining the macro would also have to provide that
definition-finding function or regexp.
Here is an example using a function to find the definition:

```
(defmacro define-foo-test (data)
  "Define a test of the foo system using DATA."
  (declare (debug (&rest sexp)))
  (let ((test-name (intern (concat ...))))
    `(progn
       (find-function-update-type-alist
        ',test-name 'ert--test 'foo-find-test-def-function)
       (ert-deftest ,test-name ()
         ,(concat "Test foo with " ...)
         ...))))
```

```

```

```
(defun foo-find-test-def-function (test-name)
  "Search for the `define-foo-test' call defining TEST-NAME.
Return non-nil if the definition is found."
  (let ((regexp ...))
    (re-search-forward regexp nil t)))
```

---

Next: [Mapping Functions](#Mapping-Functions), Previous: [Defining Functions](#Defining-Functions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.5 Calling Functions [¶](#Calling-Functions-1)

Defining functions is only half the battle. Functions don’t do
anything until you *call* them, i.e., tell them to run. Calling a
function is also known as *invocation*.

The most common way of invoking a function is by evaluating a list.
For example, evaluating the list `(concat "a" "b")` calls the
function `concat` with arguments `"a"` and `"b"`.
See [Evaluation](#Evaluation), for a description of evaluation.

When you write a list as an expression in your program, you specify
which function to call, and how many arguments to give it, in the text
of the program. Usually that’s just what you want. Occasionally you
need to compute at run time which function to call. To do that, use
the function `funcall`. When you also need to determine at run
time how many arguments to pass, use `apply`.

Function: **funcall** function &rest arguments [¶](#index-funcall)
:   `funcall` calls function with arguments, and returns
    whatever function returns.

    Since `funcall` is a function, all of its arguments, including
    function, are evaluated before `funcall` is called. This
    means that you can use any expression to obtain the function to be
    called. It also means that `funcall` does not see the
    expressions you write for the arguments, only their values.
    These values are *not* evaluated a second time in the act of
    calling function; the operation of `funcall` is like the
    normal procedure for calling a function, once its arguments have
    already been evaluated.

    The argument function must be either a Lisp function or a
    primitive function. Special forms and macros are not allowed, because
    they make sense only when given the unevaluated argument
    expressions. `funcall` cannot provide these because, as we saw
    above, it never knows them in the first place.

    If you need to use `funcall` to call a command and make it behave
    as if invoked interactively, use `funcall-interactively`
    (see [Interactive Call](#Interactive-Call)).

    ```
    (setq f 'list)
         ⇒ list
    ```

    ```
    (funcall f 'x 'y 'z)
         ⇒ (x y z)
    ```

    ```
    (funcall f 'x 'y '(z))
         ⇒ (x y (z))
    ```

    ```
    (funcall 'and t nil)
    error→ Invalid function: #<subr and>
    ```

    Compare these examples with the examples of `apply`.

Function: **apply** function &rest arguments [¶](#index-apply)
:   `apply` calls function with arguments, just like
    `funcall` but with one difference: the last of arguments is a
    list of objects, which are passed to function as separate
    arguments, rather than a single list. We say that `apply`
    *spreads* this list so that each individual element becomes an
    argument.

    `apply` with a single argument is special: the first element of
    the argument, which must be a non-empty list, is called as a function
    with the remaining elements as individual arguments. Passing two or
    more arguments will be faster.

    `apply` returns the result of calling function. As with
    `funcall`, function must either be a Lisp function or a
    primitive function; special forms and macros do not make sense in
    `apply`.

    ```
    (setq f 'list)
         ⇒ list
    ```

    ```
    (apply f 'x 'y 'z)
    error→ Wrong type argument: listp, z
    ```

    ```
    (apply '+ 1 2 '(3 4))
         ⇒ 10
    ```

    ```
    (apply '+ '(1 2 3 4))
         ⇒ 10
    ```

    ```

    ```

    ```
    (apply 'append '((a b c) nil (x y z) nil))
         ⇒ (a b c x y z)
    ```

    ```

    ```

    ```
    (apply '(+ 3 4))
         ⇒ 7
    ```

    For an interesting example of using `apply`, see [Definition of mapcar](#Definition-of-mapcar).

Sometimes it is useful to fix some of the function’s arguments at
certain values, and leave the rest of arguments for when the function
is actually called. The act of fixing some of the function’s
arguments is called *partial application* of the function[16](#FOOT16).
The result is a new function that accepts the rest of
arguments and calls the original function with all the arguments
combined.

In Emacs Lisp, this is best done with an anonymous function. For
example, if you have a function ‘`my-function`’ that takes two
arguments, you could do something like this:

```
(mapcar (lambda (x) (my-function 123 x)) ...)
```

You can also do partial application using the function
`apply-partially`. However, this will be slower than using an
anonymous function with `lambda`.

Function: **apply-partially** func &rest args [¶](#index-apply_002dpartially)
:   This function returns a new function which, when called, will call
    func with the list of arguments composed from args and
    additional arguments specified at the time of the call. If func
    accepts n arguments, then a call to `apply-partially` with
    `m <= n` arguments will produce a new function of
    `n - m` arguments[17](#FOOT17).

    Here’s how we could define the built-in function `1+`, if it
    didn’t exist, using `apply-partially` and `+`, another
    built-in function[18](#FOOT18):

    ```
    (defalias '1+ (apply-partially '+ 1)
      "Increment argument by one.")
    ```

    ```
    (1+ 10)
         ⇒ 11
    ```

It is common for Lisp functions to accept functions as arguments or
find them in data structures (especially in hook variables and property
lists) and call them using `funcall` or `apply`. Functions
that accept function arguments are often called *functionals*.

Sometimes, when you call a functional, it is useful to supply a no-op
function as the argument. Here are three different kinds of no-op
functions:

Function: **identity** argument [¶](#index-identity)
:   This function returns argument and has no side effects.

Function: **ignore** &rest arguments [¶](#index-ignore)
:   This function ignores any arguments and returns `nil`.

Function: **always** &rest arguments [¶](#index-always)
:   This function ignores any arguments and returns `t`.

Some functions are user-visible *commands*, which can be called
interactively (usually by a key sequence). It is possible to invoke
such a command exactly as though it was called interactively, by using
the `call-interactively` function. See [Interactive Call](#Interactive-Call).

---

Next: [Anonymous Functions](#Anonymous-Functions), Previous: [Calling Functions](#Calling-Functions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.6 Mapping Functions [¶](#Mapping-Functions-1)

A *mapping function* applies a given function (*not* a
special form or macro) to each element of a sequence, such as a list or
a vector or a string (see [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)).
Emacs Lisp has several such functions; this section describes
`mapcar`, `mapc`, `mapconcat`, and `mapcan`, which
map over sequences. See [Definition of mapatoms](#Definition-of-mapatoms), for the function
`mapatoms` which maps over the symbols in an obarray.
See [Definition of maphash](#Definition-of-maphash), for the function `maphash` which
maps over key/value associations in a hash table.

These mapping functions do not work on char-tables because a char-table
is a sparse array whose nominal range of indices is very large. To map
over a char-table in a way that deals properly with its sparse nature,
use the function `map-char-table` (see [Char-Tables](#Char_002dTables)).

Function: **mapcar** function sequence [¶](#index-mapcar)
:   `mapcar` applies function to each element of sequence
    in turn, and returns a list of the results.

    The argument sequence can be any kind of sequence except a
    char-table; that is, a list, a vector, a bool-vector, or a string. The
    result is always a list. The length of the result is the same as the
    length of sequence. For example:

    ```
    (mapcar #'car '((a b) (c d) (e f)))
         ⇒ (a c e)
    (mapcar #'1+ [1 2 3])
         ⇒ (2 3 4)
    (mapcar #'string "abc")
         ⇒ ("a" "b" "c")
    ```

    ```

    ```

    ```
    ;; Call each function in my-hooks.
    (mapcar 'funcall my-hooks)
    ```

    ```

    ```

    ```
    (defun mapcar* (function &rest args)
      "Apply FUNCTION to successive cars of all ARGS.
    Return the list of results."
      ;; If no list is exhausted,
      (if (not (memq nil args))
          ;; apply function to CARs.
          (cons (apply function (mapcar #'car args))
                (apply #'mapcar* function
                       ;; Recurse for rest of elements.
                       (mapcar #'cdr args)))))
    ```

    ```

    ```

    ```
    (mapcar* #'cons '(a b c) '(1 2 3 4))
         ⇒ ((a . 1) (b . 2) (c . 3))
    ```

Function: **mapcan** function sequence [¶](#index-mapcan)
:   This function applies function to each element of
    sequence, like `mapcar`, but instead of collecting the
    results into a list, it returns a single list with all the elements of
    the results (which must be lists), by altering the results (using
    `nconc`; see [Functions that Rearrange Lists](#Rearrangement)). Like with `mapcar`,
    sequence can be of any type except a char-table.

    ```
    ;; Contrast this:
    (mapcar #'list '(a b c d))
         ⇒ ((a) (b) (c) (d))
    ;; with this:
    (mapcan #'list '(a b c d))
         ⇒ (a b c d)
    ```

Function: **mapc** function sequence [¶](#index-mapc)
:   `mapc` is like `mapcar` except that function is used for
    side-effects only—the values it returns are ignored, not collected
    into a list. `mapc` always returns sequence.

Function: **mapconcat** function sequence &optional separator [¶](#index-mapconcat)
:   `mapconcat` applies function to each element of
    sequence; the results, which must be sequences of characters
    (strings, vectors, or lists), are concatenated into a single string
    return value. Between each pair of result sequences, `mapconcat`
    inserts the characters from separator, which also must be a
    string, or a vector or list of characters; a `nil` value is
    treated as the empty string. See [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors).

    The argument function must be a function that can take one
    argument and returns a sequence of characters: a string, a vector, or
    a list. The argument sequence can be any kind of sequence
    except a char-table; that is, a list, a vector, a bool-vector, or a
    string.

    ```
    (mapconcat #'symbol-name
               '(The cat in the hat)
               " ")
         ⇒ "The cat in the hat"
    ```

    ```

    ```

    ```
    (mapconcat (lambda (x) (format "%c" (1+ x)))
               "HAL-8000")
         ⇒ "IBM.9111"
    ```

---

Next: [Generic Functions](#Generic-Functions), Previous: [Mapping Functions](#Mapping-Functions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.7 Anonymous Functions [¶](#Anonymous-Functions-1)

Although functions are usually defined with `defun` and given
names at the same time, it is sometimes convenient to use an explicit
lambda expression—an *anonymous function*. Anonymous functions
are valid wherever function names are. They are often assigned as
variable values, or as arguments to functions; for instance, you might
pass one as the function argument to `mapcar`, which
applies that function to each element of a list (see [Mapping Functions](#Mapping-Functions)). See [describe-symbols example](#describe_002dsymbols-example), for a realistic example
of this.

When defining a lambda expression that is to be used as an anonymous
function, you should use the `lambda` macro, or the
`function` special form, or the `#'` read syntax:

Macro: **lambda** args [doc] [interactive] body… [¶](#index-lambda)
:   This macro returns an anonymous function with argument list
    args, documentation string doc (if any), interactive spec
    interactive (if any), and body forms given by body.

    For example, this macro makes `lambda` forms almost self-quoting:
    evaluating a form whose CAR is `lambda` yields a value that is
    almost like the form itself:

    ```
    (lambda (x) (* x x))
         ⇒ #f(lambda (x) :dynbind (* x x))
    ```

    When evaluating under lexical binding the result is a similar
    closure object, where the `:dynbind` marker is replaced by the
    captured variables (see [Closures](#Closures)).

    The `lambda` form has one other effect: it tells the Emacs
    evaluator and byte-compiler that its argument is a function, by using
    `function` as a subroutine (see below).

Special Form: **function** function-object [¶](#index-function)
:   This special form returns the function value of the function-object.
    In many ways, it is similar to `quote` (see [Quoting](#Quoting)). But unlike
    `quote`, it also serves as a note to the Emacs evaluator and
    byte-compiler that function-object is intended to be used as a
    function. Assuming function-object is a valid lambda
    expression, this has two effects:

    * When the code is byte-compiled, function-object is compiled into
      a byte-code function object (see [Byte Compilation](#Byte-Compilation)).
    * When lexical binding is enabled, function-object is converted
      into a closure. See [Closures](#Closures).

    When function-object is a symbol and the code is byte compiled,
    the byte-compiler will warn if that function is not defined or might
    not be known at run time.

The read syntax `#'` is a short-hand for using `function`.
The following forms are all equivalent:

```
(lambda (x) (* x x))
(function (lambda (x) (* x x)))
#'(lambda (x) (* x x))
```

In the following example, we define a `change-property`
function that takes a function as its third argument, followed by a
`double-property` function that makes use of
`change-property` by passing it an anonymous function:

```
(defun change-property (symbol prop function)
  (let ((value (get symbol prop)))
    (put symbol prop (funcall function value))))
```

```

```

```
(defun double-property (symbol prop)
  (change-property symbol prop (lambda (x) (* 2 x))))
```

Note that we do not quote the `lambda` form.

If you compile the above code, the anonymous function is also
compiled. This would not happen if, say, you had constructed the
anonymous function by quoting it as a list:

```
(defun double-property (symbol prop)
  (change-property symbol prop '(lambda (x) (* 2 x))))
```

In that case, the anonymous function is kept as a lambda expression in
the compiled code. The byte-compiler cannot assume this list is a
function, even though it looks like one, since it does not know that
`change-property` intends to use it as a function.

---

Next: [Accessing Function Cell Contents](#Function-Cells), Previous: [Anonymous Functions](#Anonymous-Functions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.8 Generic Functions [¶](#Generic-Functions-1)

Functions defined using `defun` have a hard-coded set of
assumptions about the types and expected values of their arguments.
For example, a function that was designed to handle values of its
argument that are either numbers or lists of numbers will fail or
signal an error if called with a value of any other type, such as a
vector or a string. This happens because the implementation of the
function is not prepared to deal with types other than those assumed
during the design.

By contrast, object-oriented programs use *polymorphic
functions*: a set of specialized functions having the same name, each
one of which was written for a certain specific set of argument types.
Which of the functions is actually called is decided at run time based
on the types of the actual arguments.

Emacs provides support for polymorphism. Like other Lisp
environments, notably Common Lisp and its Common Lisp Object System
(CLOS), this support is based on *generic functions*.
The Emacs generic functions closely follow CLOS, including
use of similar names, so if you have experience with CLOS,
the rest of this section will sound very familiar.

A generic function specifies an abstract operation, by defining its
name and list of arguments, but (usually) no implementation. The
actual implementation for several specific classes of arguments is
provided by *methods*, which should be defined separately. Each
method that implements a generic function has the same name as the
generic function, but the method’s definition indicates what kinds of
arguments it can handle by *specializing* the arguments defined by
the generic function. These *argument specializers* can be more
or less specific; for example, a `string` type is more specific
than a more general type, such as `sequence`.

Note that, unlike in message-based OO languages, such as C`++` and
Simula, methods that implement generic functions don’t belong to a
class, they belong to the generic function they implement.

When a generic function is invoked, it selects the applicable
methods by comparing the actual arguments passed by the caller with
the argument specializers of each method. A method is applicable if
the actual arguments of the call are compatible with the method’s
specializers. If more than one method is applicable, they are
combined using certain rules, described below, and the combination
then handles the call.

Macro: **cl-defgeneric** name arguments [documentation] [options-and-methods…] &rest body [¶](#index-cl_002ddefgeneric)
:   This macro defines a generic function with the specified name
    and arguments. If body is present, it provides the
    default implementation. If documentation is present (it should
    always be), it specifies the documentation string for the generic
    function, in the form `(:documentation docstring)`. The
    optional options-and-methods can be one of the following forms:

    `(declare declarations)`
    :   A declare form, as described in [The `declare` Form](#Declare-Form).

    `(:argument-precedence-order &rest args)`
    :   This form affects the sorting order for combining applicable methods.
        Normally, when two methods are compared during combination, method
        arguments are examined left to right, and the first method whose
        argument specializer is more specific will come before the other one.
        The order defined by this form overrides that, and the arguments are
        examined according to their order in this form, and not left to right.

    `(:method [qualifiers…] args &rest body)`
    :   This form defines a method like `cl-defmethod` does.

Macro: **cl-defmethod** name [extra] [qualifier] arguments [&context (expr spec)…] &rest [docstring] body [¶](#index-cl_002ddefmethod)
:   This macro defines a particular implementation for the generic
    function called name. The implementation code is given by
    body. If present, docstring is the documentation string
    for the method. The arguments list, which must be identical in
    all the methods that implement a generic function, and must match the
    argument list of that function, provides argument specializers of the
    form `(arg spec)`, where arg is the argument
    name as specified in the `cl-defgeneric` call, and spec is
    one of the following specializer forms:

    `type`
    :   This specializer requires the argument to be of the given type,
        one of the types from the type hierarchy described below.

    `(eql object)`
    :   This specializer requires the argument be `eql` to the given
        object.

    `(head object)`
    :   The argument must be a cons cell whose `car` is `eql` to
        object.

    `struct-type`
    :   The argument must be an instance of a class named struct-type
        defined with `cl-defstruct` (see [Structures](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Structures) in Common Lisp
        Extensions for GNU Emacs Lisp), or of one of its child classes.

    Method definitions can make use of a new argument-list keyword,
    `&context`, which introduces extra specializers that test the
    environment at the time the method is run. This keyword should appear
    after the list of required arguments, but before any `&rest` or
    `&optional` keywords. The `&context` specializers look much
    like regular argument specializers—(expr spec)—except
    that expr is an expression to be evaluated in the current
    context, and the spec is a value to compare against. For
    example, `&context (overwrite-mode (eql t))` will make the method
    applicable only when `overwrite-mode` is turned on. The
    `&context` keyword can be followed by any number of context
    specializers. Because the context specializers are not part of the
    generic function’s argument signature, they may be omitted in methods
    that don’t require them.

    The type specializer, `(arg type)`, can specify one
    of the *system types* in the following list. When a parent type
    is specified, an argument whose type is any of its more specific child
    types, as well as grand-children, grand-grand-children, etc. will also
    be compatible.

    `integer`
    :   Parent type: `number`.

    `number`

    `null`
    :   Parent type: `symbol`

    `symbol`

    `string`
    :   Parent type: `array`.

    `array`
    :   Parent type: `sequence`.

    `cons`
    :   Parent type: `list`.

    `list`
    :   Parent type: `sequence`.

    `marker`

    `overlay`

    `float`
    :   Parent type: `number`.

    `window-configuration`

    `process`

    `window`

    `subr`

    `compiled-function`

    `buffer`

    `char-table`
    :   Parent type: `array`.

    `bool-vector`
    :   Parent type: `array`.

    `vector`
    :   Parent type: `array`.

    `frame`

    `hash-table`

    `font-spec`

    `font-entity`

    `font-object`

    The optional extra element, expressed as ‘`:extra
    string`’, allows you to add more methods, distinguished by
    string, for the same specializers and qualifiers.

    The optional qualifier allows combining several applicable
    methods. If it is not present, the defined method is a *primary*
    method, responsible for providing the primary implementation of the
    generic function for the specialized arguments. You can also define
    *auxiliary methods*, by using one of the following values as
    qualifier:

    `:before`
    :   This auxiliary method will run before the primary method. More
        accurately, all the `:before` methods will run before the
        primary, in the most-specific-first order.

    `:after`
    :   This auxiliary method will run after the primary method. More
        accurately, all such methods will run after the primary, in the
        most-specific-last order.

    `:around`
    :   This auxiliary method will run *instead* of the primary method.
        The most specific of such methods will be run before any other method.
        Such methods normally use `cl-call-next-method`, described below,
        to invoke the other auxiliary or primary methods.

    Functions defined using `cl-defmethod` cannot be made
    interactive, i.e. commands (see [Defining Commands](#Defining-Commands)), by adding
    the `interactive` form to them. If you need a polymorphic
    command, we recommend defining a normal command that calls a
    polymorphic function defined via `cl-defgeneric` and
    `cl-defmethod`.

Each time a generic function is called, it builds the *effective
method* which will handle this invocation by combining the applicable
methods defined for the function. The process of finding the
applicable methods and producing the effective method is called
*dispatch*. The applicable methods are those all of whose
specializers are compatible with the actual arguments of the call.
Since all of the arguments must be compatible with the specializers,
they all determine whether a method is applicable. Methods that
explicitly specialize more than one argument are called
*multiple-dispatch methods*.

The applicable methods are sorted into the order in which they will be
combined. The method whose left-most argument specializer is the most
specific one will come first in the order. (Specifying
`:argument-precedence-order` as part of `cl-defmethod`
overrides that, as described above.) If the method body calls
`cl-call-next-method`, the next most-specific method will run.
If there are applicable `:around` methods, the most-specific of
them will run first; it should call `cl-call-next-method` to run
any of the less specific `:around` methods. Next, the
`:before` methods run in the order of their specificity, followed
by the primary method, and lastly the `:after` methods in the
reverse order of their specificity.

Function: **cl-call-next-method** &rest args [¶](#index-cl_002dcall_002dnext_002dmethod)
:   When invoked from within the lexical body of a primary or an
    `:around` auxiliary method, call the next applicable method for
    the same generic function. Normally, it is called with no arguments,
    which means to call the next applicable method with the same arguments
    that the calling method was invoked. Otherwise, the specified
    arguments are used instead.

Function: **cl-next-method-p** [¶](#index-cl_002dnext_002dmethod_002dp)
:   This function, when called from within the lexical body of a primary
    or an `:around` auxiliary method, returns non-`nil` if there
    is a next method to call.

---

Next: [Closures](#Closures), Previous: [Generic Functions](#Generic-Functions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
