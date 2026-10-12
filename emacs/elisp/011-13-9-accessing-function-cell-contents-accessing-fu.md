### 13.9 Accessing Function Cell Contents [¶](#Accessing-Function-Cell-Contents)

The *function definition* of a symbol is the object stored in the
function cell of the symbol. The functions described here access, test,
and set the function cell of symbols.

See also the function `indirect-function`. See [Definition of indirect-function](#Definition-of-indirect_002dfunction).

Function: **symbol-function** symbol [¶](#index-symbol_002dfunction)
:   This returns the object in the function cell of symbol. It does
    not check that the returned object is a legitimate function.
    If the function is void, the return value is `nil`.

    ```
    (defun bar (n) (+ n 2))
    (symbol-function 'bar)
         ⇒ #f(lambda (n) [t] (+ n 2))
    ```

    ```
    (fset 'baz 'bar)
         ⇒ bar
    ```

    ```
    (symbol-function 'baz)
         ⇒ bar
    ```

If you have never given a symbol any function definition, its function
cell contains the default value `nil` and we say
that that function is *void*. If you try to call
the symbol as a function, Emacs signals a `void-function` error.

Unlike with void variables (see [When a Variable is Void](#Void-Variables)), a symbol’s
function cell that contains `nil` is indistinguishable from the
function’s being void. Note that void is not the same as the symbol
`void`: `void` can be a valid function if you define it with
`defun`.

You can test the voidness of a symbol’s function definition with
`fboundp`. After you have given a symbol a function definition, you
can make it void once more using `fmakunbound`.

Function: **fboundp** symbol [¶](#index-fboundp)
:   This function returns `t` if the symbol has a non-`nil` object
    in its function cell, `nil` otherwise. It does not check that the
    object is a legitimate function.

Function: **fmakunbound** symbol [¶](#index-fmakunbound)
:   This function makes symbol’s function cell `nil`, so that a
    subsequent attempt to access this cell will cause a
    `void-function` error. It returns symbol. (See also
    `makunbound`, in [When a Variable is Void](#Void-Variables).)

    ```
    (defun foo (x) x)
    (foo 1)
         ⇒1
    ```

    ```
    (fmakunbound 'foo)
         ⇒ foo
    ```

    ```
    (foo 1)
    error→ Symbol's function definition is void: foo
    ```

Function: **fset** symbol definition [¶](#index-fset)
:   This function stores definition in the function cell of
    symbol. The result is definition. Normally
    definition should be a function or the name of a function, but
    this is not checked. The argument symbol is an ordinary evaluated
    argument.

    The primary use of this function is as a subroutine by constructs that define
    or alter functions, like `defun` or `advice-add` (see [Advising Emacs Lisp Functions](#Advising-Functions)). You can also use it to give a symbol a function definition that
    is not a function, e.g., a keyboard macro (see [Keyboard Macros](#Keyboard-Macros)):

    ```
    ;; Define a named keyboard macro.
    (fset 'kill-two-lines "\^u2\^k")
         ⇒ "\^u2\^k"
    ```

    If you wish to use `fset` to make an alternate name for a
    function, consider using `defalias` instead. See [Definition of defalias](#Definition-of-defalias).

    If the resulting function definition chain would be circular, then
    Emacs will signal a `cyclic-function-indirection` error.

---

Next: [Open Closures](#OClosures), Previous: [Accessing Function Cell Contents](#Function-Cells), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.10 Closures [¶](#Closures-1)

As explained in [Scoping Rules for Variable Bindings](#Variable-Scoping), Emacs can optionally enable
lexical binding of variables. When lexical binding is enabled, any
named function that you create (e.g., with `defun`), as well as
any anonymous function that you create using the `lambda` macro
or the `function` special form or the `#'` syntax
(see [Anonymous Functions](#Anonymous-Functions)), is automatically converted into a
*closure*.

A closure is a function that also carries a record of the lexical
environment that existed when the function was defined. When it is
invoked, any lexical variable references within its definition use the
retained lexical environment. In all other respects, closures behave
much like ordinary functions; in particular, they can be called in the
same way as ordinary functions.

See [Lexical Binding](#Lexical-Binding), for an example of using a closure.

```
;; lexical binding is enabled.
(lambda (x) (* x x))
     ⇒ #f(lambda (x) [t] (* x x))
```

The internal structure of a closure is an implementation matter and we
recommend against examining or altering it directly. For the curious,
see [Closure Function Objects](#Closure-Objects).

---

Next: [Advising Emacs Lisp Functions](#Advising-Functions), Previous: [Closures](#Closures), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.11 Open Closures [¶](#Open-Closures)

Traditionally, functions are opaque objects which offer no other
functionality but to call them. (Emacs Lisp functions aren’t fully
opaque since you can extract some info out of them such as their
docstring, their arglist, or their interactive spec, but they are
still mostly opaque.) This is usually what we want, but occasionally
we need functions to expose a bit more information about themselves.

*Open closures*, or *OClosures* for short, are function
objects which carry additional type information and expose some
information about themselves in the form of slots which you can access
via accessor functions.

OClosures are defined in two steps: first you use
`oclosure-define` to define a new OClosure type by specifying the
slots carried by the OClosures of this type, and then you use
`oclosure-lambda` to create an OClosure object of a given type.

Let’s say we want to define keyboard macros, i.e. interactive
functions which re-execute a sequence of key events (see [Keyboard Macros](#Keyboard-Macros)). You could do it with a plain function as follows:

```
(defun kbd-macro (key-sequence)
  (lambda (&optional arg)
    (interactive "P")
    (execute-kbd-macro key-sequence arg)))
```

But with such a definition there is no easy way to extract the
key-sequence from that function, for example to print it.

We can solve this problem using OClosures as follows. First we define
the type of our keyboard macros (to which we decided to add
a `counter` slot while at it):

```
(oclosure-define kbd-macro
  "Keyboard macro."
  keys (counter :mutable t))
```

After which we can rewrite our `kbd-macro` function:

```
(defun kbd-macro (key-sequence)
  (oclosure-lambda (kbd-macro (keys key-sequence) (counter 0))
      (&optional arg)
    (interactive "P")
    (execute-kbd-macro keys arg)
    (setq counter (1+ counter))))
```

As you can see, the `keys` and `counter` slots of the
OClosure can be accessed as local variables from within the body
of the OClosure. But we can now also access them from outside of the
body of the OClosure, for example to describe a keyboard macro:

```
(defun describe-kbd-macro (km)
  (if (not (eq 'kbd-macro (oclosure-type km)))
      (message "Not a keyboard macro")
    (let ((keys    (kbd-macro--keys km))
          (counter (kbd-macro--counter km)))
      (message "Keys=%S, called %d times" keys counter))))
```

Where `kbd-macro--keys` and `kbd-macro--counter` are
accessor functions generated by the `oclosure-define` macro for
oclosures whose type is `kbd-macro`.

Macro: **oclosure-define** oname &optional docstring &rest slots [¶](#index-oclosure_002ddefine)
:   This macro defines a new OClosure type along with accessor functions
    for its slots. oname can be a symbol (the name of the new
    type), or a list of the form
    `(oname . type-props)`, in which case
    type-props is a list of additional properties of this oclosure
    type. slots is a list of slot descriptions where each slot can
    be either a symbol (the name of the slot) or it can be of the form
    `(slot-name . slot-props)`, where
    slot-props is a property list of the corresponding slot
    slot-name.
    The OClosure type’s properties specified by type-props can
    include the following:

    `(:predicate pred-name)`
    :   This requests creation of a predicate function named pred-name.
        This function will be used to recognize OClosures of the type
        oname. If this type property is not specified,
        `oclosure-define` will generate a default name for the
        predicate.

    `(:parent otype)`
    :   This makes type otype of OClosures be the parent of the type
        oname. The OClosures of type oname inherit the
        slots defined by their parent type.

    `(:copier copier-name copier-args)`
    :   This causes the definition of a functional update function, knows as
        the *copier*, which takes an OClosure of type oname as its
        first argument and returns a copy of it with the slots named in
        copier-args modified to contain the value passed in the
        corresponding argument in the actual call to copier-name.

    For each slot in slots, the `oclosure-define` macro creates
    an accessor function named `oname--slot-name`; these
    can be used to access the values of the slots. The slot definitions
    in slots can specify the following properties of the slots:

    `:mutable val`
    :   By default, slots are immutable, but if you specify the
        `:mutable` property with a non-`nil` value, the slot can be
        mutated, for example with `setf` (see [The `setf` Macro](#Setting-Generalized-Variables)).

    `:type val-type`
    :   This specifies the type of the values expected to appear in the slot.

Macro: **oclosure-lambda** (type . slots) arglist &rest body [¶](#index-oclosure_002dlambda)
:   This macro creates an anonymous OClosure of type type, which
    should have been defined with `oclosure-define`. slots
    should be a list of elements of the form
    `(slot-name expr)`. At run time, each expr
    is evaluated, in order, after which the OClosure is created with its
    slots initialized with the resulting values.

    When called as a function (see [Calling Functions](#Calling-Functions)), the OClosure
    created by this macro will accept arguments according to arglist
    and will execute the code in body. body can refer to the
    value of any of its slot directly as if it were a local variable that
    had been captured by static scoping.

Function: **oclosure-type** object [¶](#index-oclosure_002dtype)
:   This function returns the OClosure type (a symbol) of object if
    it is an OClosure, and `nil` otherwise.

One other function related to OClosures is
`oclosure-interactive-form`, which allows some types of OClosures
to compute their interactive forms dynamically. See [oclosure-interactive-form](#Using-Interactive).

---

Next: [Declaring Functions Obsolete](#Obsolete-Functions), Previous: [Open Closures](#OClosures), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.12 Advising Emacs Lisp Functions [¶](#Advising-Emacs-Lisp-Functions)

When you need to modify a function defined in another library, or when you need
to modify a hook like `foo-function`, a process filter, or basically
any variable or object field which holds a function value, you can use the
appropriate setter function, such as `fset` or `defun` for named
functions, `setq` for hook variables, or `set-process-filter` for
process filters, but those are often too blunt, completely throwing away the
previous value.

The *advice* feature lets you add to the existing definition of
a function, by *advising the function*. This is a cleaner method
than redefining the whole function.

Emacs’s advice system provides two sets of primitives for that: the core set,
for function values held in variables and object fields (with the corresponding
primitives being `add-function` and `remove-function`) and another
set layered on top of it for named functions (with the main primitives being
`advice-add` and `advice-remove`).

As a trivial example, here’s how to add advice that’ll modify the
return value of a function every time it’s called:

```
(defun my-double (x)
  (* x 2))
(defun my-increase (x)
  (+ x 1))
(advice-add 'my-double :filter-return #'my-increase)
```

After adding this advice, if you call `my-double` with ‘`3`’,
the return value will be ‘`7`’. To remove this advice, say

```
(advice-remove 'my-double #'my-increase)
```

A more advanced example would be to trace the calls to the process
filter of a process proc:

```
(defun my-tracing-function (proc string)
  (message "Proc %S received %S" proc string))

(add-function :before (process-filter proc) #'my-tracing-function)
```

This will cause the process’s output to be passed to `my-tracing-function`
before being passed to the original process filter. `my-tracing-function`
receives the same arguments as the original function. When you’re done with
it, you can revert to the untraced behavior with:

```
(remove-function (process-filter proc) #'my-tracing-function)
```

Similarly, if you want to trace the execution of the function named
`display-buffer`, you could use:

```
(defun his-tracing-function (orig-fun &rest args)
  (message "display-buffer called with args %S" args)
  (let ((res (apply orig-fun args)))
    (message "display-buffer returned %S" res)
    res))

(advice-add 'display-buffer :around #'his-tracing-function)
```

Here, `his-tracing-function` is called instead of the original function
and receives the original function (additionally to that function’s arguments)
as argument, so it can call it if and when it needs to.
When you’re tired of seeing this output, you can revert to the untraced
behavior with:

```
(advice-remove 'display-buffer #'his-tracing-function)
```

The arguments `:before` and `:around` used in the above examples
specify how the two functions are composed, since there are many different
ways to do it. The added function is also called a piece of *advice*.

* [Primitives to manipulate advices](#Core-Advising-Primitives)
* [Advising Named Functions](#Advising-Named-Functions)
* [Ways to compose advice](#Advice-Combinators)
* [Adapting code using the old defadvice](#Porting-Old-Advice)
* [Advice and Byte Code](#Advice-and-Byte-Code)

---

Next: [Advising Named Functions](#Advising-Named-Functions), Up: [Advising Emacs Lisp Functions](#Advising-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.12.1 Primitives to manipulate advices [¶](#Primitives-to-manipulate-advices)

Macro: **add-function** where place function &optional props [¶](#index-add_002dfunction)
:   This macro is the handy way to add the advice function to the function
    stored in place (see [Generalized Variables](#Generalized-Variables)).

    where determines how function is composed with the
    existing function, e.g., whether function should be called before, or
    after the original function. See [Ways to compose advice](#Advice-Combinators), for the list of
    available ways to compose the two functions.

    When modifying a variable (whose name will usually end with `-function`),
    you can choose whether function is used globally or only in the current
    buffer: if place is just a symbol, then function is added to the
    global value of place. Whereas if place is of the form
    `(local symbol)`, where symbol is an expression which returns
    the variable name, then function will only be added in the
    current buffer. Finally, if you want to modify a lexical variable, you will
    have to use `(var variable)`.

    Every function added with `add-function` can be accompanied by an
    association list of properties props. Currently only two of those
    properties have a special meaning:

    `name`
    :   This gives a name to the advice, which `remove-function` can use to
        identify which function to remove. Typically used when function is an
        anonymous function.

    `depth`
    :   This specifies how to order the advice, should several pieces of
        advice be present. By default, the depth is 0. A depth of 100
        indicates that this piece of advice should be kept as deep as
        possible, whereas a depth of −100 indicates that it should stay as the
        outermost piece. When two pieces of advice specify the same depth,
        the most recently added one will be outermost.

        For `:before` advice, being outermost means that this advice will
        be run first, before any other advice, whereas being innermost means
        that it will run right before the original function, with no other
        advice run between itself and the original function. Similarly, for
        `:after` advice innermost means that it will run right after the
        original function, with no other advice run in between, whereas
        outermost means that it will be run right at the end after all other
        advice. An innermost `:override` piece of advice will only
        override the original function and other pieces of advice will apply
        to it, whereas an outermost `:override` piece of advice will
        override not only the original function but all other advice applied
        to it as well.

    If function is not interactive, then the combined function will inherit
    the interactive spec, if any, of the original function. Else, the combined
    function will be interactive and will use the interactive spec of
    function. One exception: if the interactive spec of function
    is a function (i.e., a `lambda` expression or an `fbound`
    symbol rather than an expression or a string), then the interactive
    spec of the combined function will be a call to that function with
    the interactive spec of the original function as sole argument. To
    interpret the spec received as argument, use
    `advice-eval-interactive-spec`.

    Note: The interactive spec of function will apply to the combined
    function and should hence obey the calling convention of the combined function
    rather than that of function. In many cases, it makes no difference
    since they are identical, but it does matter for `:around`,
    `:filter-args`, and `:filter-return`, where function
    receives different arguments than the original function stored in
    place.

Macro: **remove-function** place function [¶](#index-remove_002dfunction)
:   This macro removes function from the function stored in
    place. This only works if function was added to place
    using `add-function`.

    function is compared with functions added to place using
    `equal`, to try and make it work also with lambda expressions. It is
    additionally compared also with the `name` property of the functions added
    to place, which can be more reliable than comparing lambda expressions
    using `equal`.

Function: **advice-function-member-p** advice function-def [¶](#index-advice_002dfunction_002dmember_002dp)
:   Return non-`nil` if advice is already in function-def.
    Like for `remove-function` above, instead of advice being the actual
    function, it can also be the `name` of the piece of advice.

Function: **advice-function-mapc** f function-def [¶](#index-advice_002dfunction_002dmapc)
:   Call the function f for every piece of advice that was added to
    function-def. f is called with two arguments: the advice function
    and its properties.

Function: **advice-eval-interactive-spec** spec [¶](#index-advice_002deval_002dinteractive_002dspec)
:   Evaluate the interactive spec just like an interactive call to a function
    with such a spec would, and then return the corresponding list of arguments
    that was built. E.g., `(advice-eval-interactive-spec "r\nP")` will
    return a list of three elements, containing the boundaries of the region and
    the current prefix argument.

    For instance, if you want to make the `C-x m`
    (`compose-mail`) command prompt for a ‘`From:`’ header, you
    could say something like this:

    ```
    (defun my-compose-mail-advice (&rest _)
      "Read From: address interactively."
      (interactive
       (lambda (spec)
         (let* ((user-mail-address
                 (completing-read "From: "
                                  '("one.address@example.net"
                                    "alternative.address@example.net")))
                (from (message-make-from user-full-name
                                         user-mail-address))
                (spec (advice-eval-interactive-spec spec)))
           ;; Put the From header into the OTHER-HEADERS argument.
           (push (cons 'From from) (nth 2 spec))
           spec)))
      ;; This body is not used.
      nil)

    (advice-add 'compose-mail :interactive-only #'my-compose-mail-advice)
    ```

---

Next: [Ways to compose advice](#Advice-Combinators), Previous: [Primitives to manipulate advices](#Core-Advising-Primitives), Up: [Advising Emacs Lisp Functions](#Advising-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.12.2 Advising Named Functions [¶](#Advising-Named-Functions-1)

A common use of advice is for named functions and macros.
You could just use `add-function` as in:

```
(add-function :around (symbol-function 'fun) #'his-tracing-function)
```

But you should use `advice-add` and `advice-remove` for that
instead. This separate set of functions to manipulate pieces of advice applied
to named functions, offers the following extra features compared to
`add-function`: they know how to deal with macros and autoloaded
functions, they let `describe-function` preserve the original docstring as
well as document the added advice, and they let you add and remove
pieces of advice before a function is even defined.

`advice-add` can be useful for altering the behavior of existing calls
to an existing function without having to redefine the whole function.
However, it can be a source of bugs, since existing callers to the function may
assume the old behavior, and work incorrectly when the behavior is changed by
advice. Advice can also cause confusion in debugging, if the person doing the
debugging does not notice or remember that the function has been modified
by advice.

Note that the problems are not due to advice per se, but to the act
of modifying a named function. It is even more problematic to modify
a named function via lower-level primitives like `fset`,
`defalias`, or `cl-letf`. From that point of view, advice
is the better way to modify a named function because it keeps track of
the modifications, so they can be listed and undone.

Modifying a named function should be reserved for
the cases where you cannot modify Emacs’s behavior in any other way.
If it is possible to do the same thing via a hook, that is preferable
(see [Hooks](#Hooks)). If you simply want to change what a particular key
does, it may be better to write a new command, and remap the old
command’s key bindings to the new one (see [Remapping Commands](#Remapping-Commands)).

If you are writing code for release, for others to use, try to avoid
including advice in it. If the function you want to advise has no
hook to do the job, please talk with the Emacs developers about adding
a suitable hook. Especially, Emacs’s own source files should not put
advice on functions in Emacs. (There are currently a few exceptions
to this convention, but we aim to correct them.) It is generally
cleaner to create a new hook in `foo`, and make `bar` use
the hook, than to have `bar` put advice in `foo`.

Special forms (see [Special Forms](#Special-Forms)) cannot be advised, however macros can
be advised, in much the same way as functions. Of course, this will not affect
code that has already been macro-expanded, so you need to make sure the advice
is installed before the macro is expanded.

It is possible to advise a primitive (see [What Is a Function?](#What-Is-a-Function)),
but one should typically *not* do so, for two reasons. Firstly,
some primitives are used by the advice mechanism, and advising them
could cause an infinite recursion. Secondly, many primitives are
called directly from C, and such calls ignore advice; hence, one ends
up in a confusing situation where some calls (occurring from Lisp
code) obey the advice and other calls (from C code) do not.

Macro: **define-advice** symbol (where lambda-list &optional name depth) &rest body [¶](#index-define_002dadvice)
:   This macro defines a piece of advice and adds it to the function named
    symbol. If name is non-`nil`, the advice is named
    `symbol@name` and installed with the name name; otherwise,
    the advice is anonymous. See `advice-add` for explanation of
    other arguments.

Function: **advice-add** symbol where function &optional props [¶](#index-advice_002dadd)
:   Add the advice function to the named function symbol.
    where and props have the same meaning as for `add-function`
    (see [Primitives to manipulate advices](#Core-Advising-Primitives)).

Command: **advice-remove** symbol function [¶](#index-advice_002dremove)
:   Remove the advice function from the named function symbol.
    function can also be the `name` of a piece of advice. When
    called interactively, prompt for both an advised function and
    the advice to remove.

Function: **advice-member-p** function symbol [¶](#index-advice_002dmember_002dp)
:   Return non-`nil` if the advice function is already in the named
    function symbol. function can also be the `name` of
    a piece of advice.

Function: **advice-mapc** function symbol [¶](#index-advice_002dmapc)
:   Call function for every piece of advice that was added to the
    named function symbol. function is called with two
    arguments: the advice function and its properties.

---

Next: [Adapting code using the old defadvice](#Porting-Old-Advice), Previous: [Advising Named Functions](#Advising-Named-Functions), Up: [Advising Emacs Lisp Functions](#Advising-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.12.3 Ways to compose advice [¶](#Ways-to-compose-advice)

Here are the different possible values for the where argument of
`add-function` and `advice-add`, specifying how the advice
function and the original function should be composed.

`:before`
:   Call function before the old function. Both functions receive the
    same arguments, and the return value of the composition is the return value of
    the old function. More specifically, the composition of the two functions
    behaves like:

    ```
    (lambda (&rest r) (apply function r) (apply oldfun r))
    ```

    `(add-function :before funvar function)` is comparable for
    single-function hooks to `(add-hook 'hookvar function)` for
    normal hooks.

`:after`
:   Call function after the old function. Both functions receive the
    same arguments, and the return value of the composition is the return value of
    the old function. More specifically, the composition of the two functions
    behaves like:

    ```
    (lambda (&rest r) (prog1 (apply oldfun r) (apply function r)))
    ```

    `(add-function :after funvar function)` is comparable for
    single-function hooks to `(add-hook 'hookvar function
    'append)` for normal hooks.

`:override`
:   This completely replaces the old function with the new one. The old function
    can of course be recovered if you later call `remove-function`.

`:around`
:   Call function instead of the old function, but provide the old function
    as an extra argument to function. This is the most flexible composition.
    For example, it lets you call the old function with different arguments, or
    many times, or within a let-binding, or you can sometimes delegate the work to
    the old function and sometimes override it completely. More specifically, the
    composition of the two functions behaves like:

    ```
    (lambda (&rest r) (apply function oldfun r))
    ```

`:before-while`
:   Call function before the old function and don’t call the old
    function if function returns `nil`. Both functions receive the
    same arguments, and the return value of the composition is the return value of
    the old function. More specifically, the composition of the two functions
    behaves like:

    ```
    (lambda (&rest r) (and (apply function r) (apply oldfun r)))
    ```

    `(add-function :before-while funvar function)` is comparable
    for single-function hooks to `(add-hook 'hookvar function)`
    when hookvar is run via `run-hook-with-args-until-failure`.

`:before-until`
:   Call function before the old function and only call the old function if
    function returns `nil`. More specifically, the composition of the
    two functions behaves like:

    ```
    (lambda (&rest r) (or (apply function r) (apply oldfun r)))
    ```

    `(add-function :before-until funvar function)` is comparable
    for single-function hooks to `(add-hook 'hookvar function)`
    when hookvar is run via `run-hook-with-args-until-success`.

`:after-while`
:   Call function after the old function and only if the old function
    returned non-`nil`. Both functions receive the same arguments, and the
    return value of the composition is the return value of function.
    More specifically, the composition of the two functions behaves like:

    ```
    (lambda (&rest r) (and (apply oldfun r) (apply function r)))
    ```

    `(add-function :after-while funvar function)` is comparable
    for single-function hooks to `(add-hook 'hookvar function
    'append)` when hookvar is run via
    `run-hook-with-args-until-failure`.

`:after-until`
:   Call function after the old function and only if the old function
    returned `nil`. More specifically, the composition of the two functions
    behaves like:

    ```
    (lambda (&rest r) (or  (apply oldfun r) (apply function r)))
    ```

    `(add-function :after-until funvar function)` is comparable
    for single-function hooks to `(add-hook 'hookvar function
    'append)` when hookvar is run via
    `run-hook-with-args-until-success`.

`:filter-args`
:   Call function first and use the result (which should be a list) as the
    new arguments to pass to the old function. More specifically, the composition
    of the two functions behaves like:

    ```
    (lambda (&rest r) (apply oldfun (funcall function r)))
    ```

`:filter-return`
:   Call the old function first and pass the result to function.
    More specifically, the composition of the two functions behaves like:

    ```
    (lambda (&rest r) (funcall function (apply oldfun r)))
    ```

`:interactive-only`
:   While the where option controls how the body of the two functions
    are composed, it does not actually affect the way interactive forms are
    composed. So, in a sense, this does the opposite of `:override`:
    call only the old function as if no advice was applied. But it still
    affects the interactive form like any other where value would: The
    interactive form of function, if any, overrides that of
    oldfun and if it is a lambda expression, it receives
    function’s interactive form as argument.
    See see [advice-eval-interactive-spec](#advice_002deval_002dinteractive_002dspec) for an example.

---

Next: [Advice and Byte Code](#Advice-and-Byte-Code), Previous: [Ways to compose advice](#Advice-Combinators), Up: [Advising Emacs Lisp Functions](#Advising-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.12.4 Adapting code using the old defadvice [¶](#Adapting-code-using-the-old-defadvice)

A lot of code uses the old `defadvice` mechanism, which has been made
obsolete by the new `advice-add`, whose implementation and semantics is
significantly simpler.

An old piece of advice such as:

```
(defadvice previous-line (before next-line-at-end
                                 (&optional arg try-vscroll))
  "Insert an empty line when moving up from the top line."
  (if (and next-line-add-newlines (= arg 1)
           (save-excursion (beginning-of-line) (bobp)))
      (progn
        (beginning-of-line)
        (newline))))
```

could be translated in the new advice mechanism into a plain function:

```
(defun previous-line--next-line-at-end (&optional arg try-vscroll)
  "Insert an empty line when moving up from the top line."
  (if (and next-line-add-newlines (= arg 1)
           (save-excursion (beginning-of-line) (bobp)))
      (progn
        (beginning-of-line)
        (newline))))
```

Obviously, this does not actually modify `previous-line`. For that the
old advice needed:

```
(ad-activate 'previous-line)
```

whereas the new advice mechanism needs:

```
(advice-add 'previous-line :before #'previous-line--next-line-at-end)
```

Note that `ad-activate` had a global effect: it activated all pieces of
advice enabled for that specified function. If you wanted to only activate or
deactivate a particular piece, you needed to *enable* or *disable*
it with `ad-enable-advice` and `ad-disable-advice`.
The new mechanism does away with this distinction.

Around advice such as:

```
(defadvice foo (around foo-around)
  "Ignore case in `foo'."
  (let ((case-fold-search t))
    ad-do-it))
(ad-activate 'foo)
```

could translate into:

```
(defun foo--foo-around (orig-fun &rest args)
  "Ignore case in `foo'."
  (let ((case-fold-search t))
    (apply orig-fun args)))
(advice-add 'foo :around #'foo--foo-around)
```

Regarding the advice’s *class*, note that the new `:before` is not
quite equivalent to the old `before`, because in the old advice you could
modify the function’s arguments (e.g., with `ad-set-arg`), and that would
affect the argument values seen by the original function, whereas in the new
`:before`, modifying an argument via `setq` in the advice has no
effect on the arguments seen by the original function.
When porting `before` advice which relied on this behavior, you’ll need
to turn it into new `:around` or `:filter-args` advice instead.

Similarly old `after` advice could modify the returned value by
changing `ad-return-value`, whereas new `:after` advice cannot, so
when porting such old `after` advice, you’ll need to turn it into new
`:around` or `:filter-return` advice instead.

---

Previous: [Adapting code using the old defadvice](#Porting-Old-Advice), Up: [Advising Emacs Lisp Functions](#Advising-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.12.5 Advice and Byte Code [¶](#Advice-and-Byte-Code-1)

Not all functions can be reliably advised. The byte compiler may
choose to replace a call to a function with a sequence of instructions
that doesn’t call the function you were interested in altering.

This usually happens due to one of the three following mechanisms:

`byte-compile` properties
:   If a function’s symbol has a `byte-compile` property, that
    property will be used instead of the symbol’s function definition.
    See [Byte-Compilation Functions](#Compilation-Functions).

`byte-optimize` properties
:   If a function’s symbol has a `byte-optimize` property, the byte
    compiler may rewrite the function arguments, or decide to use a
    different function altogether.

`compiler-macro` declare forms
:   A function can have a special `compiler-macro` `declare`
    form in its definition (see [The `declare` Form](#Declare-Form)) that defines an
    *expander* to call when compiling the function. The expander
    could then cause the produced byte-code not to call the original
    function.

---

Next: [Inline Functions](#Inline-Functions), Previous: [Advising Emacs Lisp Functions](#Advising-Functions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.13 Declaring Functions Obsolete [¶](#Declaring-Functions-Obsolete)

You can mark a named function as *obsolete*, meaning that it may
be removed at some point in the future. This causes Emacs to warn
that the function is obsolete whenever it byte-compiles code
containing that function, and whenever it displays the documentation
for that function. In all other respects, an obsolete function
behaves like any other function.

The easiest way to mark a function as obsolete is to put a
`(declare (obsolete …))` form in the function’s
`defun` definition. See [The `declare` Form](#Declare-Form). Alternatively, you can
use the `make-obsolete` function, described below.

A macro (see [Macros](#Macros)) can also be marked obsolete with
`make-obsolete`; this has the same effects as for a function. An
alias for a function or macro can also be marked as obsolete; this
makes the alias itself obsolete, not the function or macro which it
resolves to.

Function: **make-obsolete** obsolete-name current-name when [¶](#index-make_002dobsolete)
:   This function marks obsolete-name as obsolete.
    obsolete-name should be a symbol naming a function or macro, or
    an alias for a function or macro.

    If current-name is a symbol, the warning message says to use
    current-name instead of obsolete-name. current-name
    does not need to be an alias for obsolete-name; it can be a
    different function with similar functionality. current-name can
    also be a string, which serves as the warning message. The message
    should begin in lower case, and end with a period. It can also be
    `nil`, in which case the warning message provides no additional
    details.

    The argument when should be a string indicating when the function
    was first made obsolete—for example, a date or a release number.

Macro: **define-obsolete-function-alias** obsolete-name current-name when &optional doc [¶](#index-define_002dobsolete_002dfunction_002dalias)
:   This convenience macro marks the function obsolete-name obsolete
    and also defines it as an alias for the function current-name.
    It is equivalent to the following:

    ```
    (defalias obsolete-name current-name doc)
    (make-obsolete obsolete-name current-name when)
    ```

In addition, you can mark a particular calling convention for a
function as obsolete:

Function: **set-advertised-calling-convention** function signature when [¶](#index-set_002dadvertised_002dcalling_002dconvention)
:   This function specifies the argument list signature as the
    correct way to call function. This causes the Emacs byte
    compiler to issue a warning whenever it comes across an Emacs Lisp
    program that calls function any other way (however, it will
    still allow the code to be byte compiled). when should be a
    string indicating when the variable was first made obsolete (usually a
    version number string).

    For instance, in old versions of Emacs the `sit-for` function
    accepted three arguments, like this

    ```
      (sit-for seconds milliseconds nodisp)
    ```

    During a transition period, the function accepted those three
    arguments, but declared this old calling convention as deprecated like
    this:

    ```
    (set-advertised-calling-convention
      'sit-for '(seconds &optional nodisp) "22.1")
    ```

    The alternative to using this function is the
    `advertised-calling-convention` `declare` spec, see
    [The `declare` Form](#Declare-Form).

---

Next: [The `declare` Form](#Declare-Form), Previous: [Declaring Functions Obsolete](#Obsolete-Functions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.14 Inline Functions [¶](#Inline-Functions-1)

An *inline function* is a function that works just like an
ordinary function, except for one thing: when you byte-compile a call
to the function (see [Byte Compilation](#Byte-Compilation)), the function’s definition
is expanded into the caller.

The simple way to define an inline function, is to write
`defsubst` instead of `defun`. The rest of the definition
looks just the same, but using `defsubst` says to make it inline
for byte compilation.

Macro: **defsubst** name args [doc] [declare] [interactive] body… [¶](#index-defsubst)
:   This macro defines an inline function. Its syntax is exactly the same
    as `defun` (see [Defining Functions](#Defining-Functions)).

Making a function inline often makes its function calls run faster.
But it also has disadvantages. For one thing, it reduces flexibility;
if you change the definition of the function, calls already inlined
still use the old definition until you recompile them.

Another disadvantage is that making a large function inline can
increase the size of compiled code both in files and in memory. Since
the speed advantage of inline functions is greatest for small
functions, you generally should not make large functions inline.

Also, inline functions do not behave well with respect to debugging,
tracing, and advising (see [Advising Emacs Lisp Functions](#Advising-Functions)). Since ease of
debugging and the flexibility of redefining functions are important
features of Emacs, you should not make a function inline, even if it’s
small, unless its speed is really crucial, and you’ve timed the code
to verify that using `defun` actually has performance problems.

After an inline function is defined, its inline expansion can be
performed later on in the same file, just like macros.

It’s possible to use `defmacro` to define a macro to expand
into the same code that an inline function would execute
(see [Macros](#Macros)). But the macro would be limited to direct use in
expressions—a macro cannot be called with `apply`,
`mapcar` and so on. Also, it takes some work to convert an
ordinary function into a macro. To convert it into an inline function
is easy; just replace `defun` with `defsubst`. Since each
argument of an inline function is evaluated exactly once, you needn’t
worry about how many times the body uses the arguments, as you do for
macros.

Alternatively, you can define a function by providing the code which
will inline it as a compiler macro (see [The `declare` Form](#Declare-Form)). The
following macros make this possible.

Macro: **define-inline** name args [doc] [declare] body… [¶](#index-define_002dinline)
:   Define a function name by providing code that does its inlining,
    as a compiler macro. The function will accept the argument list
    args and will have the specified body.

    If present, doc should be the function’s documentation string
    (see [Documentation Strings of Functions](#Function-Documentation)); declare, if present, should be
    a `declare` form (see [The `declare` Form](#Declare-Form)) specifying the function’s
    metadata. In addition to the usual declarations, declare
    can include `(noinline NOINLINE)` when a non-`nil`
    NOINLINE prevents Emacs from inlining the defined function.

Functions defined via `define-inline` have several advantages
with respect to macros defined by `defsubst` or `defmacro`:

* They can be passed to `mapcar` (see [Mapping Functions](#Mapping-Functions)).
* They are more efficient.
* They can be used as *place forms* to store values
  (see [Generalized Variables](#Generalized-Variables)).
* They behave in a more predictable way than `cl-defsubst`
  (see [Argument Lists](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Argument-Lists) in Common Lisp Extensions for GNU Emacs
  Lisp).

Like `defmacro`, a function inlined with `define-inline`
inherits the scoping rules, either dynamic or lexical, from the call
site. See [Scoping Rules for Variable Bindings](#Variable-Scoping).

The following macros should be used in the body of a function defined
by `define-inline`.

Macro: **inline-quote** expression [¶](#index-inline_002dquote)
:   Quote expression for `define-inline`. This is similar to
    the backquote (see [Backquote](#Backquote)), but quotes code and accepts only
    `,`, not `,@`.

Macro: **inline-letevals** (bindings…) body… [¶](#index-inline_002dletevals)
:   This provides a convenient way to ensure that the arguments to an
    inlined function are evaluated exactly once, as well as to create
    local variables.

    It’s similar to `let` (see [Local Variables](#Local-Variables)): It sets up local
    variables as specified by bindings, and then evaluates
    body with those bindings in effect.

    Each element of bindings should be either a symbol or a list of
    the form `(var expr)`; the result is to evaluate
    expr and bind var to the result. However, when an element
    of bindings is just a symbol var, the result of evaluating
    var is re-bound to var (which is quite different from the
    way `let` works).

    The tail of bindings can be either `nil` or a symbol which
    should hold a list of arguments, in which case each argument is
    evaluated, and the symbol is bound to the resulting list.

Macro: **inline-const-p** expression [¶](#index-inline_002dconst_002dp)
:   Return non-`nil` if the value of expression is already
    known.

Macro: **inline-const-val** expression [¶](#index-inline_002dconst_002dval)
:   Return the value of expression.

Macro: **inline-error** format &rest args [¶](#index-inline_002derror)
:   Signal an error, formatting args according to format.

Here’s an example of using `define-inline`:

```
(define-inline myaccessor (obj)
  (inline-letevals (obj)
    (inline-quote (if (foo-p ,obj) (aref (cdr ,obj) 3) (aref ,obj 2)))))
```

This is equivalent to

```
(defsubst myaccessor (obj)
  (if (foo-p obj) (aref (cdr obj) 3) (aref obj 2)))
```

---

Next: [Telling the Compiler that a Function is Defined](#Declaring-Functions), Previous: [Inline Functions](#Inline-Functions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.15 The `declare` Form [¶](#The-declare-Form)

`declare` is a special macro which can be used to add meta
properties to a function or macro: for example, marking it as
obsolete, or giving its forms a special `TAB` indentation
convention in Emacs Lisp mode.

Macro: **declare** specs… [¶](#index-declare-1)
:   This macro ignores its arguments and evaluates to `nil`; it has
    no run-time effect. However, when a `declare` form occurs in the
    declare argument of a `defun` or `defsubst` function
    definition (see [Defining Functions](#Defining-Functions)) or a `defmacro` macro
    definition (see [Defining Macros](#Defining-Macros)), it appends the properties
    specified by specs to the function or macro. This work is
    specially performed by `defun`, `defsubst`, and
    `defmacro`.

    Each element in specs should have the form `(property
    args…)`, which should not be quoted. These have the
    following effects:

    `(advertised-calling-convention signature when)` [¶](#index-advertised_002dcalling_002dconvention-_0028declare-spec_0029)
    :   This acts like a call to `set-advertised-calling-convention`
        (see [Declaring Functions Obsolete](#Obsolete-Functions)); signature specifies the correct
        argument list for calling the function or macro, and when should
        be a string indicating when the old argument list was first made obsolete.

    `(debug edebug-form-spec)`
    :   This is valid for macros only. When stepping through the macro with
        Edebug, use edebug-form-spec. See [Instrumenting Macro Calls](#Instrumenting-Macro-Calls).

    `(doc-string n)`
    :   This is used when defining a function or macro which itself will be used to
        define entities like functions, macros, or variables. It indicates that
        the nth argument, if any, should be considered
        as a documentation string.

    `(indent indent-spec)`
    :   Indent calls to this function or macro according to indent-spec.
        This is typically used for macros, though it works for functions too.
        See [Indenting Macros](#Indenting-Macros).

    `(autoload-macro value)` [¶](#index-autoload_002dmacro-_0028declare-spec_0029)
    :   This is used when defining a macro. If value is ‘`expand`’,
        any calls to the macro which follow an autoload comment will first be
        expanded during generation of the autoloads. This declaration is used
        as an alternative to hard-coding lists of macros to expand in
        `loaddefs-generate--make-autoload`. See [Autoload](#Autoload).

    `(interactive-only value)`
    :   Set the function’s `interactive-only` property to value.
        See [The interactive-only property](#The-interactive_002donly-property).

    `(obsolete current-name when)` [¶](#index-obsolete-_0028declare-spec_0029)
    :   Mark the function or macro as obsolete, similar to a call to
        `make-obsolete` (see [Declaring Functions Obsolete](#Obsolete-Functions)). current-name
        should be a symbol (in which case the warning message says to use that
        instead), a string (specifying the warning message), or `nil` (in
        which case the warning message gives no extra details). when
        should be a string indicating when the function or macro was first
        made obsolete.

    `(compiler-macro expander)` [¶](#index-compiler-macro)
    :   This can only be used for functions, and tells the compiler to use
        expander as an optimization function. When encountering a call to the
        function, of the form `(function args…)`, the macro
        expander will call expander with that form as well as with
        args…, and expander can either return a new expression to use
        instead of the function call, or it can return just the form unchanged,
        to indicate that the function call should be left alone.

        When expander is a lambda form it should be written with
        a single argument (i.e., be of the form `(lambda (arg)
        body)`) because the function’s formal arguments are
        automatically added to the lambda’s list of arguments for you.

    `(gv-expander expander)`
    :   Declare expander to be the function to handle calls to the macro (or
        function) as a generalized variable, similarly to `gv-define-expander`.
        expander can be a symbol or it can be of the form `(lambda
        (arg) body)` in which case that function will additionally have
        access to the macro (or function)’s arguments.

    `(gv-setter setter)`
    :   Declare setter to be the function to handle calls to the macro (or
        function) as a generalized variable. setter can be a symbol in which
        case it will be passed to `gv-define-simple-setter`, or it can be of the
        form `(lambda (arg) body)` in which case that function will
        additionally have access to the macro (or function)’s arguments and it will
        be passed to `gv-define-setter`.

    `(completion completion-predicate)`
    :   Declare completion-predicate as a function to determine whether
        to include a function’s symbol in the list of functions when asking
        for completions in `M-x`. This predicate function will only be
        called when `read-extended-command-predicate` is customized to
        `command-completion-default-include-p`; by default the value of
        `read-extended-command-predicate` is nil (see [execute-extended-command](#Interactive-Call)). The predicate
        completion-predicate is called with two arguments: the
        function’s symbol and the current buffer.

    `(modes modes)`
    :   Specify that this command is meant to be applicable only to specified
        modes. See [Specifying Modes For Commands](#Command-Modes).

    `(interactive-args arg ...)`
    :   Specify the arguments that should be stored for `repeat-command`.
        Each arg is on the form `argument-name form`.

    `(pure val)`
    :   If val is non-`nil`, this function is *pure*
        (see [What Is a Function?](#What-Is-a-Function)). This is the same as the `pure`
        property of the function’s symbol (see [Standard Symbol Properties](#Standard-Properties)).

    `(side-effect-free val)`
    :   If val is non-`nil`, this function is free of side effects,
        so the byte compiler can ignore calls whose value is ignored. This is
        the same as the `side-effect-free` property of the function’s
        symbol, see [Standard Symbol Properties](#Standard-Properties).

    `(important-return-value val)`
    :   If val is non-`nil`, the byte compiler will warn about
        calls to this function that do not use the returned value. This is the
        same as the `important-return-value` property of the function’s
        symbol, see [Standard Symbol Properties](#Standard-Properties).

    `(speed n)`
    :   Specify the value of `native-comp-speed` in effect for native
        compilation of this function (see [Native-Compilation Variables](#Native_002dCompilation-Variables)).
        This allows function-level control of the optimization level used for
        native code emitted for the function. In particular, if n is
        −1, native compilation of the function will emit bytecode
        instead of native code for the function.

    `(safety n)`
    :   Specify the value of `compilation-safety` in effect for this
        function. This allows function-level control of the safety level used
        for the code emitted for the function (see [Native-Compilation Variables](#Native_002dCompilation-Variables)).

    `(ftype type &optional function)` [¶](#index-function-type-declaration)
    :   Declare type to be the type of this function. This type is used
        by `describe-function` for documentation, and by the native
        compiler (see [Compilation of Lisp to Native Code](#Native-Compilation)) for optimizing code generation and
        inferring types. Incorrect type declarations may cause crashes in
        natively compiled code (see below). Functions with type declarations
        are shown by `C-h C-f` as having a *declared type*, as opposed
        to an *inferred type* for functions without them.

        type is a *type specifier* (see [Type Specifiers](#Type-Specifiers)) of the
        form `(function (arg-1-type … arg-n-type) RETURN-TYPE)`. Argument types can be interleaved with `&optional`
        and `&rest` to reflect the function’s calling convention
        (see [Features of Argument Lists](#Argument-List)).

        function if present should be the name of function being defined.

        Here’s an example of using `ftype` inside `declare` to define
        a function `positive-p`, which takes an argument of type
        number and returns a boolean:

        ```
        (defun positive-p (x)
          (declare (ftype (function (number) boolean)))
          (when (> x 0)
            t))
        ```

        Similarly, this defines a function `cons-or-number` that takes a
        first argument of type cons or a number, a second optional
        argument of type string, and returns one of the symbols
        `is-cons` or `is-number`:

        ```
        (defun cons-or-number (x &optional err-msg)
          (declare (ftype (function ((or cons number) &optional string)
        			    (member is-cons is-number))))
          (if (consp x)
              'is-cons
            (if (numberp x)
        	'is-number
              (error (or err-msg "Unexpected input")))))
        ```

        For description of additional types, see [Lisp Data Types](#Lisp-Data-Types)).

        Declaring a function with an incorrect type causes undefined behavior.
        If such a function is natively compiled with `compilation-safety`
        set to zero (see [compilation-safety](#compilation_002dsafety)), this may result in incorrect
        execution or even Emacs crashing when the compiled code is loaded.
        Redefining or advising a type-declared function must preserve the
        original signature to avoid these issues.

    `no-font-lock-keyword`
    :   This is valid for macros only. Macros with this declaration are
        highlighted by font-lock (see [Font Lock Mode](#Font-Lock-Mode)) as normal functions,
        not specially as macros.

---

Next: [Determining whether a Function is Safe to Call](#Function-Safety), Previous: [The `declare` Form](#Declare-Form), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.16 Telling the Compiler that a Function is Defined [¶](#Telling-the-Compiler-that-a-Function-is-Defined)

Byte-compiling a file often produces warnings about functions that the
compiler doesn’t know about (see [Compiler Errors](#Compiler-Errors)). Sometimes this
indicates a real problem, but usually the functions in question are
defined in other files which would be loaded if that code is run. For
example, byte-compiling `simple.el` used to warn:

```
simple.el:8727:1:Warning: the function `shell-mode' is not known to be
    defined.
```

In fact, `shell-mode` is used only in a function that executes
`(require 'shell)` before calling `shell-mode`, so
`shell-mode` will be defined properly at run-time. When you know
that such a warning does not indicate a real problem, it is good to
suppress the warning. That makes new warnings which might mean real
problems more visible. You do that with `declare-function`.

All you need to do is add a `declare-function` statement before the
first use of the function in question:

```
(declare-function shell-mode "shell" ())
```

This says that `shell-mode` is defined in `shell.el` (the
‘`.el`’ can be omitted). The compiler takes for granted that that file
really defines the function, and does not check.

The optional third argument specifies the argument list of
`shell-mode`. In this case, it takes no arguments
(`nil` is different from not specifying a value). In other
cases, this might be something like `(file &optional overwrite)`.
You don’t have to specify the argument list, but if you do the
byte compiler can check that the calls match the declaration.

Macro: **declare-function** function file &optional arglist fileonly [¶](#index-declare_002dfunction-1)
:   Tell the byte compiler to assume that function is defined in the
    file file. The optional third argument arglist is either
    `t`, meaning the argument list is unspecified, or a list of
    formal parameters in the same style as `defun` (including the
    parentheses). An omitted arglist defaults to `t`, not
    `nil`; this is atypical behavior for omitted arguments, and it
    means that to supply a fourth but not third argument one must specify
    `t` for the third-argument placeholder instead of the usual
    `nil`. The optional fourth argument fileonly
    non-`nil` means check only that file exists, not that it
    actually defines function.

To verify that these functions really are declared where
`declare-function` says they are, use `check-declare-file`
to check all `declare-function` calls in one source file, or use
`check-declare-directory` check all the files in and under a
certain directory.

These commands find the file that ought to contain a function’s
definition using `locate-library`; if that finds no file, they
expand the definition file name relative to the directory of the file
that contains the `declare-function` call.

You can also say that a function is a primitive by specifying a file
name ending in ‘`.c`’ or ‘`.m`’. This is useful only when you
call a primitive that is defined only on certain systems. Most
primitives are always defined, so they will never give you a warning.

Sometimes a file will optionally use functions from an external package.
If you prefix the filename in the `declare-function` statement with
‘`ext:`’, then it will be checked if it is found, otherwise skipped
without error.

There are some function definitions that ‘`check-declare`’ does not
understand (e.g., `defstruct` and some other macros). In such cases,
you can pass a non-`nil` fileonly argument to
`declare-function`, meaning to only check that the file exists, not
that it actually defines the function. Note that to do this without
having to specify an argument list, you should set the arglist
argument to `t` (because `nil` means an empty argument list, as
opposed to an unspecified one).

---

Next: [Other Topics Related to Functions](#Related-Topics), Previous: [Telling the Compiler that a Function is Defined](#Declaring-Functions), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.17 Determining whether a Function is Safe to Call [¶](#Determining-whether-a-Function-is-Safe-to-Call)

Some major modes, such as SES, call functions that are stored in user
files. (See [Simple Emacs Spreadsheet](https://www.gnu.org/software/emacs/manual/html_mono/ses.html#Top), for more
information on SES.) User files sometimes have poor pedigrees—you
can get a spreadsheet from someone you’ve just met, or you can get one
through email from someone you’ve never met. So it is risky to call a
function whose source code is stored in a user file until you have
determined that it is safe.

Function: **unsafep** form &optional unsafep-vars [¶](#index-unsafep)
:   Returns `nil` if form is a *safe* Lisp expression, or
    returns a list that describes why it might be unsafe. The argument
    unsafep-vars is a list of symbols known to have temporary
    bindings at this point; it is mainly used for internal recursive
    calls. The current buffer is an implicit argument, which provides a
    list of buffer-local bindings.

Being quick and simple, `unsafep` does a very light analysis and
rejects many Lisp expressions that are actually safe. There are no
known cases where `unsafep` returns `nil` for an unsafe
expression. However, a safe Lisp expression can return a string
with a `display` property, containing an associated Lisp
expression to be executed after the string is inserted into a buffer.
This associated expression can be malicious. In order to be safe, you
must delete properties from all strings calculated by user code before
inserting them into buffers.

---

Previous: [Determining whether a Function is Safe to Call](#Function-Safety), Up: [Functions](#Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.18 Other Topics Related to Functions [¶](#Other-Topics-Related-to-Functions)

Here is a table of several functions that do things related to
function calling and function definitions. They are documented
elsewhere, but we provide cross references here.

`apply`
:   See [Calling Functions](#Calling-Functions).

`autoload`
:   See [Autoload](#Autoload).

`call-interactively`
:   See [Interactive Call](#Interactive-Call).

`called-interactively-p`
:   See [Distinguish Interactive Calls](#Distinguish-Interactive).

`commandp`
:   See [Interactive Call](#Interactive-Call).

`documentation`
:   See [Access to Documentation Strings](#Accessing-Documentation).

`eval`
:   See [Eval](#Eval).

`funcall`
:   See [Calling Functions](#Calling-Functions).

`function`
:   See [Anonymous Functions](#Anonymous-Functions).

`ignore`
:   See [Calling Functions](#Calling-Functions).

`indirect-function`
:   See [Symbol Function Indirection](#Function-Indirection).

`interactive`
:   See [Using `interactive`](#Using-Interactive).

`interactive-p`
:   See [Distinguish Interactive Calls](#Distinguish-Interactive).

`mapatoms`
:   See [Creating and Interning Symbols](#Creating-Symbols).

`mapcar`
:   See [Mapping Functions](#Mapping-Functions).

`map-char-table`
:   See [Char-Tables](#Char_002dTables).

`mapconcat`
:   See [Mapping Functions](#Mapping-Functions).

`undefined`
:   See [Functions for Key Lookup](#Functions-for-Key-Lookup).

---

Next: [Customization Settings](#Customization), Previous: [Functions](#Functions), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 14 Macros [¶](#Macros-1)

*Macros* enable you to define new control constructs and other
language features. A macro is defined much like a function, but instead
of telling how to compute a value, it tells how to compute another Lisp
expression which will in turn compute the value. We call this
expression the *expansion* of the macro.

Macros can do this because they operate on the unevaluated expressions
for the arguments, not on the argument values as functions do. They can
therefore construct an expansion containing these argument expressions
or parts of them.

If you are using a macro to do something an ordinary function could
do, just for the sake of speed, consider using an inline function
instead. See [Inline Functions](#Inline-Functions).

* [A Simple Example of a Macro](#Simple-Macro)
* [Expansion of a Macro Call](#Expansion)
* [Macros and Byte Compilation](#Compiling-Macros)
* [Defining Macros](#Defining-Macros)
* [Common Problems Using Macros](#Problems-with-Macros)
* [Indenting Macros](#Indenting-Macros)

---

Next: [Expansion of a Macro Call](#Expansion), Up: [Macros](#Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.1 A Simple Example of a Macro [¶](#A-Simple-Example-of-a-Macro)

Suppose we would like to define a Lisp construct to increment a
variable value, much like the `++` operator in C. We would like to
write `(inc x)` and have the effect of `(setq x (1+ x))`.
Here’s a macro definition that does the job:

```
(defmacro inc (var)
   (list 'setq var (list '1+ var)))
```

When this is called with `(inc x)`, the argument var is the
symbol `x`—*not* the *value* of `x`, as it would
be in a function. The body of the macro uses this to construct the
expansion, which is `(setq x (1+ x))`. Once the macro definition
returns this expansion, Lisp proceeds to evaluate it, thus incrementing
`x`.

Function: **macrop** object [¶](#index-macrop)
:   This predicate tests whether its argument is a macro, and returns
    `t` if so, `nil` otherwise.

---

Next: [Macros and Byte Compilation](#Compiling-Macros), Previous: [A Simple Example of a Macro](#Simple-Macro), Up: [Macros](#Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.2 Expansion of a Macro Call [¶](#Expansion-of-a-Macro-Call)

A macro call looks just like a function call in that it is a list which
starts with the name of the macro. The rest of the elements of the list
are the arguments of the macro.

Evaluation of the macro call begins like evaluation of a function call
except for one crucial difference: the macro arguments are the actual
expressions appearing in the macro call. They are not evaluated before
they are given to the macro definition. By contrast, the arguments of a
function are results of evaluating the elements of the function call
list.

Having obtained the arguments, Lisp invokes the macro definition just
as a function is invoked. The argument variables of the macro are bound
to the argument values from the macro call, or to a list of them in the
case of a `&rest` argument. And the macro body executes and
returns its value just as a function body does.

The second crucial difference between macros and functions is that
the value returned by the macro body is an alternate Lisp expression,
also known as the *expansion* of the macro. The Lisp interpreter
proceeds to evaluate the expansion as soon as it comes back from the
macro.

Since the expansion is evaluated in the normal manner, it may contain
calls to other macros. It may even be a call to the same macro, though
this is unusual.

Note that Emacs tries to expand macros when loading an uncompiled
Lisp file. This is not always possible, but if it is, it speeds up
subsequent execution. See [How Programs Do Loading](#How-Programs-Do-Loading).

You can see the expansion of a given macro call by calling
`macroexpand`.

Function: **macroexpand** form &optional environment [¶](#index-macroexpand)
:   This function expands form, if it is a macro call. If the result
    is another macro call, it is expanded in turn, until something which is
    not a macro call results. That is the value returned by
    `macroexpand`. If form is not a macro call to begin with, it
    is returned as given.

    Note that `macroexpand` does not look at the subexpressions of
    form (although some macro definitions may do so). Even if they
    are macro calls themselves, `macroexpand` does not expand them.

    The function `macroexpand` does not expand calls to inline functions.
    Normally there is no need for that, since a call to an inline function is
    no harder to understand than a call to an ordinary function.

    If environment is provided, it specifies an alist of macro
    definitions that shadow the currently defined macros. Byte compilation
    uses this feature.

    ```
    (defmacro inc (var)
        (list 'setq var (list '1+ var)))
    ```

    ```

    ```

    ```
    (macroexpand '(inc r))
         ⇒ (setq r (1+ r))
    ```

    ```

    ```

    ```
    (defmacro inc2 (var1 var2)
        (list 'progn (list 'inc var1) (list 'inc var2)))
    ```

    ```

    ```

    ```
    (macroexpand '(inc2 r s))
         ⇒ (progn (inc r) (inc s))  ; inc not expanded here.
    ```

Function: **macroexpand-all** form &optional environment [¶](#index-macroexpand_002dall)
:   `macroexpand-all` expands macros like `macroexpand`, but
    will look for and expand all macros in form, not just at the
    top-level. If no macros are expanded, the return value is `eq`
    to form.

    Repeating the example used for `macroexpand` above with
    `macroexpand-all`, we see that `macroexpand-all` *does*
    expand the embedded calls to `inc`:

    ```
    (macroexpand-all '(inc2 r s))
         ⇒ (progn (setq r (1+ r)) (setq s (1+ s)))
    ```

Function: **macroexpand-1** form &optional environment [¶](#index-macroexpand_002d1)
:   This function expands macros like `macroexpand`, but it only
    performs one step of the expansion: if the result is another macro
    call, `macroexpand-1` will not expand it.

---

Next: [Defining Macros](#Defining-Macros), Previous: [Expansion of a Macro Call](#Expansion), Up: [Macros](#Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.3 Macros and Byte Compilation [¶](#Macros-and-Byte-Compilation)

You might ask why we take the trouble to compute an expansion for a
macro and then evaluate the expansion. Why not have the macro body
produce the desired results directly? The reason has to do with
compilation.

When a macro call appears in a Lisp program being compiled, the Lisp
compiler calls the macro definition just as the interpreter would, and
receives an expansion. But instead of evaluating this expansion, it
compiles the expansion as if it had appeared directly in the program.
As a result, the compiled code produces the value and side effects
intended for the macro, but executes at full compiled speed. This would
not work if the macro body computed the value and side effects
itself—they would be computed at compile time, which is not useful.

In order for compilation of macro calls to work, the macros must
already be defined in Lisp when the calls to them are compiled. The
compiler has a special feature to help you do this: if a file being
compiled contains a `defmacro` form, the macro is defined
temporarily for the rest of the compilation of that file.

Byte-compiling a file also executes any `require` calls at
top-level in the file, so you can ensure that necessary macro
definitions are available during compilation by requiring the files
that define them (see [Features](#Named-Features)). To avoid loading the macro
definition files when someone *runs* the compiled program, write
`eval-when-compile` around the `require` calls (see [Evaluation During Compilation](#Eval-During-Compile)).

---

Next: [Common Problems Using Macros](#Problems-with-Macros), Previous: [Macros and Byte Compilation](#Compiling-Macros), Up: [Macros](#Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.4 Defining Macros [¶](#Defining-Macros-1)

A Lisp macro object is a list whose CAR is `macro`, and
whose CDR is a function. Expansion of the macro works
by applying the function (with `apply`) to the list of
*unevaluated* arguments from the macro call.

It is possible to use an anonymous Lisp macro just like an anonymous
function, but this is never done, because it does not make sense to
pass an anonymous macro to functionals such as `mapcar`. In
practice, all Lisp macros have names, and they are almost always
defined with the `defmacro` macro.

Macro: **defmacro** name args [doc] [declare] body… [¶](#index-defmacro)
:   `defmacro` defines the symbol name (which should not be
    quoted) as a macro that looks like this:

    ```
    (macro lambda args . body)
    ```

    (Note that the CDR of this list is a lambda expression.) This
    macro object is stored in the function cell of name. The
    meaning of args is the same as in a function, and the keywords
    `&rest` and `&optional` may be used (see [Features of Argument Lists](#Argument-List)).
    Neither name nor args should be quoted. The return value
    of `defmacro` is undefined.

    doc, if present, should be a string specifying the macro’s
    documentation string. declare, if present, should be a
    `declare` form specifying metadata for the macro (see [The `declare` Form](#Declare-Form)). Note that macros cannot have interactive declarations, since
    they cannot be called interactively.

Macros often need to construct large list structures from a mixture
of constants and nonconstant parts. To make this easier, use the
‘`` ` ``’ syntax (see [Backquote](#Backquote)). For example:

```
(defmacro t-becomes-nil (variable)
  `(if (eq ,variable t)
       (setq ,variable nil)))
```

```

```

```
(t-becomes-nil foo)
     ≡ (if (eq foo t) (setq foo nil))
```

---

Next: [Indenting Macros](#Indenting-Macros), Previous: [Defining Macros](#Defining-Macros), Up: [Macros](#Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
