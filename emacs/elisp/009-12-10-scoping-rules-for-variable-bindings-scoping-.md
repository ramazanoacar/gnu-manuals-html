### 12.10 Scoping Rules for Variable Bindings [¶](#Scoping-Rules-for-Variable-Bindings)

When you create a local binding for a variable, that binding takes
effect only within a limited portion of the program (see [Local Variables](#Local-Variables)). This section describes exactly what this means.

Each local binding has a certain *scope* and *extent*.
*Scope* refers to *where* in the textual source code the
binding can be accessed. *Extent* refers to *when*, as the
program is executing, the binding exists.

For historical reasons, there are two dialects of Emacs Lisp,
selected via the `lexical-binding` buffer-local variable.
In the modern Emacs Lisp dialect, local bindings are lexical by default.
A *lexical binding* has *lexical scope*, meaning that any
reference to the variable must be located textually within the binding
construct[13](#FOOT13). It also has
*indefinite extent*, meaning that under some circumstances the
binding can live on even after the binding construct has finished
executing, by means of objects called *closures*.
Lexical scoping is also commonly called *static scoping*.

Local bindings can also be dynamic, which they always are in the
old Emacs Lisp dialect and optionally in the modern dialect.
A *dynamic binding* has *dynamic scope*, meaning that any
part of the program can potentially access the variable binding. It
also has *dynamic extent*, meaning that the binding lasts only
while the binding construct (such as the body of a `let` form) is
being executed.

The old dynamic-only Emacs Lisp dialect is still the default in code
loaded or evaluated from Lisp files that lack a dialect declaration.
Eventually the modern dialect will be made the default.
All Lisp files should declare the dialect used to ensure that they
keep working correctly in the future.

The following subsections describe lexical binding and dynamic
binding in greater detail, and how to enable lexical binding in Emacs
Lisp programs.

* [Lexical Binding](#Lexical-Binding)
* [Dynamic Binding](#Dynamic-Binding)
* [Proper Use of Dynamic Binding](#Dynamic-Binding-Tips)
* [Selecting Lisp Dialect](#Selecting-Lisp-Dialect)
* [Converting to Lexical Binding](#Converting-to-Lexical-Binding)

---

Next: [Dynamic Binding](#Dynamic-Binding), Up: [Scoping Rules for Variable Bindings](#Variable-Scoping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.10.1 Lexical Binding [¶](#Lexical-Binding-1)

Lexical binding is only available in the modern Emacs Lisp dialect.
(See [Selecting Lisp Dialect](#Selecting-Lisp-Dialect).)
A lexically-bound variable has *lexical scope*, meaning that any
reference to the variable must be located textually within the binding
construct. Here is an example

```
(let ((x 1))    ; x is lexically bound.
  (+ x 3))
     ⇒ 4

(defun getx ()
  x)            ; x is used free in this function.

(let ((x 1))    ; x is lexically bound.
  (getx))
error→ Symbol's value as variable is void: x
```

Here, the variable `x` has no global value. When it is lexically
bound within a `let` form, it can be used in the textual confines
of that `let` form. But it can *not* be used from within a
`getx` function called from the `let` form, since the
function definition of `getx` occurs outside the `let` form
itself.

Here is how lexical binding works. Each binding construct defines a
*lexical environment*, specifying the variables that are bound
within the construct and their local values. When the Lisp evaluator
wants the current value of a variable, it looks first in the lexical
environment; if the variable is not specified in there, it looks in
the symbol’s value cell, where the dynamic value is stored.

Lexical bindings have indefinite extent. Even after a binding
construct has finished executing, its lexical environment can be
“kept around” in Lisp objects called *closures*. A closure is
created when you define a named or anonymous function with lexical
binding enabled. See [Closures](#Closures), for details.

When a closure is called as a function, any lexical variable
references within its definition use the retained lexical environment.
Here is an example:

```
(defvar my-ticker nil)   ; We will use this dynamically bound
                         ; variable to store a closure.

(let ((x 0))             ; x is lexically bound.
  (setq my-ticker (lambda ()
                    (setq x (1+ x)))))
    ⇒ #f(lambda () [(x 0)]
          (setq x (1+ x)))

(funcall my-ticker)
    ⇒ 1

(funcall my-ticker)
    ⇒ 2

(funcall my-ticker)
    ⇒ 3

x                        ; Note that x has no global value.
error→ Symbol's value as variable is void: x
```

Here, the `let` binding defines a lexical environment in which the
variable `x` is locally bound to 0. Within this binding
construct, we define a lambda expression which increments `x` by
one and returns the incremented value. This lambda expression is
automatically turned into a closure, in which the lexical environment
lives on even after the `let` binding construct has exited. Each
time we evaluate the closure, it increments `x`, using the
binding of `x` in that lexical environment.

Note that unlike dynamic variables which are tied to the symbol
object itself, the relationship between lexical variables and symbols
is only present in the interpreter (or compiler). Therefore,
functions which take a symbol argument (like `symbol-value`,
`boundp`, and `set`) can only retrieve or modify a
variable’s dynamic binding (i.e., the contents of its symbol’s value
cell).

Note also that variables may be declared special, in which case they
will use dynamic binding, even for new bindings such as a `let`
binding. Depending on how the variable is declared, it can be
special globally, for a single file, or for a portion of a file.
See [Dynamic Binding](#Dynamic-Binding), for further details.

---

Next: [Proper Use of Dynamic Binding](#Dynamic-Binding-Tips), Previous: [Lexical Binding](#Lexical-Binding), Up: [Scoping Rules for Variable Bindings](#Variable-Scoping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.10.2 Dynamic Binding [¶](#Dynamic-Binding-1)

Local variable bindings are dynamic in the modern Lisp dialect for
special variables (see below), and for all variables in the old Lisp
dialect. (See [Selecting Lisp Dialect](#Selecting-Lisp-Dialect).)
Dynamic variable bindings have their uses but are in general more
error-prone and less efficient than lexical bindings, and the compiler
is less able to find mistakes in code using dynamic bindings.

When a variable is dynamically bound, its current binding
at any point in the execution of the Lisp program is simply the most
recently-created dynamic local binding for that symbol, or the global
binding if there is no such local binding.

Dynamic bindings have dynamic scope and extent, as shown by the
following example:

```
(defvar x -99)  ; x receives an initial value of −99.

(defun getx ()
  x)            ; x is used free in this function.

(let ((x 1))    ; x is dynamically bound.
  (getx))
     ⇒ 1

;; After the let form finishes, x reverts to its
;; previous value, which is −99.

(getx)
     ⇒ -99
```

The function `getx` refers to `x`. This is a *free*
reference, in the sense that there is no binding for `x` within
that `defun` construct itself. When we call `getx` from
within a `let` form in which `x` is (dynamically) bound, it
retrieves the local value (i.e., 1). But when we call `getx`
outside the `let` form, it retrieves the global value (i.e.,
−99).

Here is another example, which illustrates setting a dynamically
bound variable using `setq`:

```
(defvar x -99)      ; x receives an initial value of −99.

(defun addx ()
  (setq x (1+ x)))  ; Add 1 to x and return its new value.

(let ((x 1))
  (addx)
  (addx))
     ⇒ 3           ; The two addx calls add to x twice.

;; After the let form finishes, x reverts to its
;; previous value, which is −99.

(addx)
     ⇒ -98
```

Even when lexical binding is enabled, certain variables will
continue to be dynamically bound. These are called *special
variables*. Every variable that has been defined with `defvar`,
`defcustom` or `defconst` is a special variable
(see [Defining Global Variables](#Defining-Variables)). All other variables are subject to
lexical binding.

Using `defvar` without a value, it is possible to bind a variable
dynamically just in one file, or in just one part of a file while
still binding it lexically elsewhere. For example:

```
(let (_)
  (defvar x)      ; Let-bindings of x will be dynamic within this let.
  (let ((x -99))  ; This is a dynamic binding of x.
    (defun get-dynamic-x ()
      x)))

(let ((x 'lexical)) ; This is a lexical binding of x.
  (defun get-lexical-x ()
    x))

(let (_)
  (defvar x)
  (let ((x 'dynamic))
    (list (get-lexical-x)
          (get-dynamic-x))))
    ⇒ (lexical dynamic)
```

Function: **special-variable-p** symbol [¶](#index-special_002dvariable_002dp)
:   This function returns non-`nil` if symbol is a special
    variable (i.e., it has a `defvar`, `defcustom`, or
    `defconst` variable definition). Otherwise, the return value is
    `nil`.

    Note that since this is a function, it can only return
    non-`nil` for variables which are permanently special, but not
    for those that are only special in the current lexical scope.

The use of a special variable as a formal argument in a function is
not supported.

Dynamic binding is implemented in Emacs Lisp in a simple way. Each
symbol has a value cell, which specifies its current dynamic value (or
absence of value). See [Symbol Components](#Symbol-Components). When a symbol is given
a dynamic local binding, Emacs records the contents of the value cell
(or absence thereof) in a stack, and stores the new local value in the
value cell. When the binding construct finishes executing, Emacs pops
the old value off the stack, and puts it in the value cell.

---

Next: [Selecting Lisp Dialect](#Selecting-Lisp-Dialect), Previous: [Dynamic Binding](#Dynamic-Binding), Up: [Scoping Rules for Variable Bindings](#Variable-Scoping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.10.3 Proper Use of Dynamic Binding [¶](#Proper-Use-of-Dynamic-Binding)

Dynamic binding is a powerful feature, as it allows programs to
refer to variables that are not defined within their local textual
scope. However, if used without restraint, this can also make
programs hard to understand.

First, choose the variable’s name to avoid name conflicts
(see [Emacs Lisp Coding Conventions](#Coding-Conventions)).

* If the variable is only used when locally bound to a value, declare it
  special using a `defvar` form without an initial value, and never
  assign to it unless it is already bound. This way, any attempt to
  refer to the variable when unbound will result in a
  `void-variable` error.
* Otherwise, define the variable with `defvar`, `defconst`
  (see [Defining Global Variables](#Defining-Variables)), or `defcustom` (see [Defining Customization Variables](#Variable-Definitions)). Usually, the definition should be at top-level in an
  Emacs Lisp file. As far as possible, it should include a
  documentation string which explains the meaning and purpose of the
  variable.

  Then you can bind the variable anywhere in a program, knowing reliably
  what the effect will be. Wherever you encounter the variable, it will
  be easy to refer back to the definition, e.g., via the `C-h v`
  command (provided the variable definition has been loaded into Emacs).
  See [Name Help](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Name-Help) in The GNU Emacs Manual.

  For example, it is common to use local bindings for customizable
  variables like `case-fold-search`:

  ```
  (defun search-for-abc ()
    "Search for the string \"abc\", ignoring case differences."
    (let ((case-fold-search t))
      (re-search-forward "abc")))
  ```

---

Next: [Converting to Lexical Binding](#Converting-to-Lexical-Binding), Previous: [Proper Use of Dynamic Binding](#Dynamic-Binding-Tips), Up: [Scoping Rules for Variable Bindings](#Variable-Scoping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.10.4 Selecting Lisp Dialect [¶](#Selecting-Lisp-Dialect-1)

When loading an Emacs Lisp file or evaluating a Lisp buffer, the
Lisp dialect is selected using the buffer-local variable
`lexical-binding`.

Variable: **lexical-binding** [¶](#index-lexical_002dbinding)
:   If this buffer-local variable is non-`nil`, Emacs Lisp files and
    buffers are evaluated using the modern Lisp dialect that by default
    uses lexical binding instead of dynamic binding. If `nil`, the
    old dialect is used that uses dynamic binding for all local variables.
    This variable is typically set for a whole Emacs Lisp file, as a
    file-local variable (see [File Local Variables](#File-Local-Variables)). Note that unlike
    other such variables, this one must be set in the first line of a
    file.

In practice, dialect selection means that the first line in an Emacs
Lisp file looks like:

```
;;; ...  -*- lexical-binding: t -*-
```

for the modern lexical-binding dialect, and

```
;;; ...  -*- lexical-binding: nil -*-
```

for the old dynamic-only dialect. When no declaration is present the
old dialect is used, but this may change in a future release.
The compiler will warn if no declaration is present.

When evaluating Emacs Lisp code directly using an `eval` call,
lexical binding is enabled if the lexical argument to
`eval` is non-`nil`. See [Eval](#Eval).

Lexical binding is also enabled in Lisp Interaction and IELM mode,
used in the `*scratch*` and `*ielm*` buffers, and also when
evaluating expressions via `M-:` (`eval-expression`), when
processing the `--eval`, `-x`, and `--script`
command-line options of Emacs (see [Action Arguments](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Action-Arguments) in The GNU
Emacs Manual), and when processing the `--eval` command-line
option of `emacsclient` (see [emacsclient Options](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#emacsclient-Options) in The GNU Emacs Manual).

---

Previous: [Selecting Lisp Dialect](#Selecting-Lisp-Dialect), Up: [Scoping Rules for Variable Bindings](#Variable-Scoping)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.10.5 Converting to Lexical Binding [¶](#Converting-to-Lexical-Binding-1)

Converting an Emacs Lisp program to lexical binding is easy. First,
add a file-local variable setting of `lexical-binding` to
`t` in the header line of the Emacs Lisp source file (see [File Local Variables](#File-Local-Variables)). Second, check that every variable in the program
which needs to be dynamically bound has a variable definition, so that
it is not inadvertently bound lexically.

A simple way to find out which variables need a variable definition
is to byte-compile the source file. See [Byte Compilation](#Byte-Compilation). If a
non-special variable is used outside of a `let` form, the
byte-compiler will warn about reference or assignment to a free
variable. If a non-special variable is bound but not used within a
`let` form, the byte-compiler will warn about an unused lexical
variable. The byte-compiler will also issue a warning if you use a
special variable as a function argument.

A warning about a reference or an assignment to a free variable is
usually a clear sign that that variable should be marked as
dynamically scoped, so you need to add an appropriate `defvar`
before the first use of that variable.

A warning about an unused variable may be a good hint that the
variable was intended to be dynamically scoped (because it is actually
used, but in another function), but it may also be an indication that
the variable is simply really not used and could simply be removed.
So you need to find out which case it is, and based on that, either
add a `defvar` or remove the variable altogether. If removal is
not possible or not desirable (typically because it is a formal
argument and that we cannot or don’t want to change all the callers),
you can also add a leading underscore to the variable’s name to
indicate to the compiler that this is a variable known not to
be used.)

#### Cross-file variable checking [¶](#Cross_002dfile-variable-checking)

**Caution:** This is an experimental feature that may change or
disappear without prior notice.

The byte-compiler can also warn about lexical variables that are
special in other Emacs Lisp files, often indicating a missing
`defvar` declaration. This useful but somewhat specialized check
requires three steps:

1. Byte-compile all files whose special variable declarations may be of
   interest, with the environment variable `EMACS_GENERATE_DYNVARS`
   set to a nonempty string. These are typically all the files in the
   same package or related packages or Emacs subsystems. The process
   will generate a file whose name ends in `.dynvars` for each
   compiled Emacs Lisp file.
2. Concatenate the `.dynvars` files into a single file.
3. Byte-compile the files that need to be checked, this time with
   the environment variable `EMACS_DYNVARS_FILE` set to the name
   of the aggregated file created in step 2.

Here is an example illustrating how this could be done, assuming that
a Unix shell and `make` are used for byte-compilation:

```
$ rm *.elc                                # force recompilation
$ EMACS_GENERATE_DYNVARS=1 make           # generate .dynvars
$ cat *.dynvars > ~/my-dynvars            # combine .dynvars
$ rm *.elc                                # force recompilation
$ EMACS_DYNVARS_FILE=~/my-dynvars make    # perform checks
```

---

Next: [File Local Variables](#File-Local-Variables), Previous: [Scoping Rules for Variable Bindings](#Variable-Scoping), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.11 Buffer-Local Variables [¶](#Buffer_002dLocal-Variables-1)

Global and local variable bindings are found in most programming
languages in one form or another. Emacs, however, also supports
additional, unusual kinds of variable binding, such as
*buffer-local* bindings, which apply only in one buffer. Having
different values for a variable in different buffers is an important
customization method. (Variables can also have bindings that are
local to each terminal. See [Multiple Terminals](#Multiple-Terminals).)

* [Introduction to Buffer-Local Variables](#Intro-to-Buffer_002dLocal)
* [Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal)
* [The Default Value of a Buffer-Local Variable](#Default-Value)

---

Next: [Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal), Up: [Buffer-Local Variables](#Buffer_002dLocal-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.11.1 Introduction to Buffer-Local Variables [¶](#Introduction-to-Buffer_002dLocal-Variables)

A buffer-local variable has a buffer-local binding associated with a
particular buffer. The binding is in effect when that buffer is
current; otherwise, it is not in effect. If you set the variable while
a buffer-local binding is in effect, the new value goes in that binding,
so its other bindings are unchanged. This means that the change is
visible only in the buffer where you made it.

The variable’s ordinary binding, which is not associated with any
specific buffer, is called the *default binding*. In most cases,
this is the global binding.

A variable can have buffer-local bindings in some buffers but not in
other buffers. The default binding is shared by all the buffers that
don’t have their own bindings for the variable. (This includes all
newly-created buffers.) If you set the variable in a buffer that does
not have a buffer-local binding for it, this sets the default binding,
so the new value is visible in all the buffers that see the default
binding.

The most common use of buffer-local bindings is for major modes to change
variables that control the behavior of commands. For example, C mode and
Lisp mode both set the variable `paragraph-start` to specify that only
blank lines separate paragraphs. They do this by making the variable
buffer-local in the buffer that is being put into C mode or Lisp mode, and
then setting it to the new value for that mode. See [Major Modes](#Major-Modes).

The usual way to make a buffer-local binding is with
`make-local-variable`, which is what major mode commands typically
use. This affects just the current buffer; all other buffers (including
those yet to be created) will continue to share the default value unless
they are explicitly given their own buffer-local bindings.

A more powerful operation is to mark the variable as
*automatically buffer-local* by calling
`make-variable-buffer-local`. You can think of this as making the
variable local in all buffers, even those yet to be created. More
precisely, the effect is that setting the variable automatically makes
the variable local to the current buffer if it is not already so. All
buffers start out by sharing the default value of the variable as usual,
but setting the variable creates a buffer-local binding for the current
buffer. The new value is stored in the buffer-local binding, leaving
the default binding untouched. This means that the default value cannot
be changed with `setq` in any buffer; the only way to change it is
with `setq-default`.

**Warning:** When a variable has buffer-local
bindings in one or more buffers, `let` rebinds the binding that’s
currently in effect. For instance, if the current buffer has a
buffer-local value, `let` temporarily rebinds that. If no
buffer-local bindings are in effect, `let` rebinds
the default value. If inside the `let` you then change to a
different current buffer in which a different binding is in effect,
you won’t see the `let` binding any more. And if you exit the
`let` while still in the other buffer, you won’t see the
unbinding occur (though it will occur properly). Here is an example
to illustrate:

```
(setq foo 'g)
(set-buffer "a")
(make-local-variable 'foo)
```

```
(setq foo 'a)
(let ((foo 'temp))
  ;; foo ⇒ 'temp  ; let binding in buffer ‘a’
  (set-buffer "b")
  ;; foo ⇒ 'g     ; the global value since foo is not local in ‘b’
  body...)
```

```
foo ⇒ 'g        ; exiting restored the local value in buffer ‘a’,
                 ; but we don’t see that in buffer ‘b’
```

```
(set-buffer "a") ; verify the local value was restored
foo ⇒ 'a
```

Note that references to `foo` in body access the
buffer-local binding of buffer ‘`b`’.

When a file specifies local variable values, these become buffer-local
values when you visit the file. See [File Variables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Variables) in The
GNU Emacs Manual.

A terminal-local variable cannot be made buffer-local
(see [Multiple Terminals](#Multiple-Terminals)).

---

Next: [The Default Value of a Buffer-Local Variable](#Default-Value), Previous: [Introduction to Buffer-Local Variables](#Intro-to-Buffer_002dLocal), Up: [Buffer-Local Variables](#Buffer_002dLocal-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.11.2 Creating and Deleting Buffer-Local Bindings [¶](#Creating-and-Deleting-Buffer_002dLocal-Bindings)

Command: **make-local-variable** variable [¶](#index-make_002dlocal_002dvariable)
:   This function creates a buffer-local binding in the current buffer for
    variable (a symbol). Other buffers are not affected. The value
    returned is variable.

    The buffer-local value of variable starts out as the same value
    variable previously had. If variable was void, it remains
    void.

    ```
    ;; In buffer ‘b1’:
    (setq foo 5)                ; Affects all buffers.
         ⇒ 5
    ```

    ```
    (make-local-variable 'foo)  ; Now it is local in ‘b1’.
         ⇒ foo
    ```

    ```
    foo                         ; That did not change
         ⇒ 5                   ;   the value.
    ```

    ```
    (setq foo 6)                ; Change the value
         ⇒ 6                   ;   in ‘b1’.
    ```

    ```
    foo
         ⇒ 6
    ```

    ```

    ```

    ```
    ;; In buffer ‘b2’, the value hasn’t changed.
    (with-current-buffer "b2"
      foo)
         ⇒ 5
    ```

    Making a variable buffer-local within a `let`-binding for that
    variable does not work reliably, unless the buffer in which you do this
    is not current either on entry to or exit from the `let`. This is
    because `let` does not distinguish between different kinds of
    bindings; it knows only which variable the binding was made for.

    It is an error to make a constant or a read-only variable
    buffer-local. See [Variables that Never Change](#Constant-Variables).

    If the variable is terminal-local (see [Multiple Terminals](#Multiple-Terminals)), this
    function signals an error. Such variables cannot have buffer-local
    bindings as well.

    **Warning:** do not use `make-local-variable` for a hook
    variable. The hook variables are automatically made buffer-local as
    needed if you use the local argument to `add-hook` or
    `remove-hook`.

Macro: **setq-local** &rest pairs [¶](#index-setq_002dlocal)
:   pairs is a list of variable and value pairs. This macro creates
    a buffer-local binding in the current buffer for each of the
    variables, and gives them a buffer-local value. It is equivalent to
    calling `make-local-variable` followed by `setq` for each of
    the variables. The variables should be unquoted symbols.

    ```
    (setq-local var1 "value1"
                var2 "value2")
    ```

Command: **make-variable-buffer-local** variable [¶](#index-make_002dvariable_002dbuffer_002dlocal)
:   This function marks variable (a symbol) automatically
    buffer-local, so that any subsequent attempt to set it will make it
    local to the current buffer at the time. Unlike
    `make-local-variable`, with which it is often confused, this
    cannot be undone, and affects the behavior of the variable in all
    buffers.

    A peculiar wrinkle of this feature is that binding the variable (with
    `let` or other binding constructs) does not create a buffer-local
    binding for it. Only setting the variable (with `set` or
    `setq`), while the variable does not have a `let`-style
    binding that was made in the current buffer, does so.

    If variable does not have a default value, then calling this
    command will give it a default value of `nil`. If variable
    already has a default value, that value remains unchanged.
    Subsequently calling `makunbound` on variable will result
    in a void buffer-local value and leave the default value unaffected.

    The value returned is variable.

    It is an error to make a constant or a read-only variable
    buffer-local. See [Variables that Never Change](#Constant-Variables).

    **Warning:** Don’t assume that you should use
    `make-variable-buffer-local` for user-option variables, simply
    because users *might* want to customize them differently in
    different buffers. Users can make any variable local, when they wish
    to. It is better to leave the choice to them.

    The time to use `make-variable-buffer-local` is when it is crucial
    that no two buffers ever share the same binding. For example, when a
    variable is used for internal purposes in a Lisp program which depends
    on having separate values in separate buffers, then using
    `make-variable-buffer-local` can be the best solution.

Macro: **defvar-local** variable &optional value docstring [¶](#index-defvar_002dlocal)
:   This macro defines variable as a variable with initial value
    value and docstring, and marks it as automatically
    buffer-local. It is equivalent to calling `defvar` followed by
    `make-variable-buffer-local`. variable should be an
    unquoted symbol.

Function: **local-variable-p** variable &optional buffer [¶](#index-local_002dvariable_002dp)
:   This returns `t` if variable is buffer-local in buffer
    buffer (which defaults to the current buffer); otherwise,
    `nil`.

Function: **local-variable-if-set-p** variable &optional buffer [¶](#index-local_002dvariable_002dif_002dset_002dp)
:   This returns `t` if variable either has a buffer-local
    value in buffer buffer, or is automatically buffer-local.
    Otherwise, it returns `nil`. If omitted or `nil`,
    buffer defaults to the current buffer.

Function: **buffer-local-value** variable buffer [¶](#index-buffer_002dlocal_002dvalue)
:   This function returns the buffer-local binding of variable (a
    symbol) in buffer buffer. If variable does not have a
    buffer-local binding in buffer buffer, it returns the default
    value (see [The Default Value of a Buffer-Local Variable](#Default-Value)) of variable instead.

Function: **buffer-local-boundp** variable buffer [¶](#index-buffer_002dlocal_002dboundp)
:   This returns non-`nil` if there’s either a buffer-local binding
    of variable (a symbol) in buffer buffer, or variable
    has a global binding.

Function: **buffer-local-variables** &optional buffer [¶](#index-buffer_002dlocal_002dvariables)
:   This function returns a list describing the buffer-local variables in
    buffer buffer. (If buffer is omitted, the current buffer
    is used.) Normally, each list element has the form
    `(sym . val)`, where sym is a buffer-local
    variable (a symbol) and val is its buffer-local value. But when
    a variable’s buffer-local binding in buffer is void, its list
    element is just sym.

    ```
    (make-local-variable 'foobar)
    (makunbound 'foobar)
    (make-local-variable 'bind-me)
    (setq bind-me 69)
    ```

    ```
    (setq lcl (buffer-local-variables))
        ;; First, built-in variables local in all buffers:
    ⇒ ((mark-active . nil)
        (buffer-undo-list . nil)
        (mode-name . "Fundamental")
        ...
    ```

    ```
        ;; Next, non-built-in buffer-local variables.
        ;; This one is buffer-local and void:
        foobar
        ;; This one is buffer-local and nonvoid:
        (bind-me . 69))
    ```

    Note that storing new values into the CDRs of cons cells in this
    list does *not* change the buffer-local values of the variables.

Command: **kill-local-variable** variable [¶](#index-kill_002dlocal_002dvariable)
:   This function deletes the buffer-local binding (if any) for
    variable (a symbol) in the current buffer. As a result, the
    default binding of variable becomes visible in this buffer. This
    typically results in a change in the value of variable, since the
    default value is usually different from the buffer-local value just
    eliminated.

    If you kill the buffer-local binding of a variable that automatically
    becomes buffer-local when set, this makes the default value visible in
    the current buffer. However, if you set the variable again, that will
    once again create a buffer-local binding for it.

    `kill-local-variable` returns variable.

    This function is a command because it is sometimes useful to kill one
    buffer-local variable interactively, just as it is useful to create
    buffer-local variables interactively.

Function: **kill-all-local-variables** &optional kill-permanent [¶](#index-kill_002dall_002dlocal_002dvariables)
:   This function eliminates all the buffer-local variable bindings of the
    current buffer. As a result, the buffer will see the default values
    of most variables. By default, variables marked as permanent and
    local hook functions that have a non-`nil`
    `permanent-local-hook` property (see [Setting Hooks](#Setting-Hooks)) won’t be
    killed, but if the optional kill-permanent argument is
    non-`nil`, even those variables will be killed.

    This function also resets certain other information pertaining to the
    buffer: it sets the local keymap to `nil`, the syntax table to the
    value of `(standard-syntax-table)`, the case table to
    `(standard-case-table)`, and the abbrev table to the value of
    `fundamental-mode-abbrev-table`.

    The very first thing this function does is run the normal hook
    `change-major-mode-hook` (see below).

    Every major mode command begins by calling this function, which has the
    effect of switching to Fundamental mode and erasing most of the effects
    of the previous major mode. To ensure that this does its job, the
    variables that major modes set should not be marked permanent.

    `kill-all-local-variables` returns `nil`.

Variable: **change-major-mode-hook** [¶](#index-change_002dmajor_002dmode_002dhook)
:   The function `kill-all-local-variables` runs this normal hook
    before it does anything else. This gives major modes a way to arrange
    for something special to be done if the user switches to a different
    major mode. It is also useful for buffer-specific minor modes
    that should be forgotten if the user changes the major mode.

    For best results, make this variable buffer-local, so that it will
    disappear after doing its job and will not interfere with the
    subsequent major mode. See [Hooks](#Hooks).

A buffer-local variable is *permanent* if the variable name (a
symbol) has a `permanent-local` property that is non-`nil`.
Such variables are unaffected by `kill-all-local-variables`, and
their local bindings are therefore not cleared by changing major modes.
Permanent locals are appropriate for data pertaining to where the file
came from or how to save it, rather than with how to edit the contents.

---

Previous: [Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal), Up: [Buffer-Local Variables](#Buffer_002dLocal-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.11.3 The Default Value of a Buffer-Local Variable [¶](#The-Default-Value-of-a-Buffer_002dLocal-Variable)

The global value of a variable with buffer-local bindings is also
called the *default* value, because it is the value that is in
effect whenever the current buffer lacks its own binding for the
variable.

The functions `default-value` and `setq-default` access and
change a variable’s default value regardless of whether the current
buffer has a buffer-local binding. For example, you could use
`setq-default` to change the default setting of
`paragraph-start` for most buffers; and this would work even when
you are in a C or Lisp mode buffer that has a buffer-local value for
this variable.

The special forms `defvar` and `defconst` also set the
default value (if they set the variable at all), rather than any
buffer-local value.

Function: **default-value** symbol [¶](#index-default_002dvalue)
:   This function returns symbol’s default value. This is the value
    that is seen in buffers and frames that do not have their own values for
    this variable. If symbol is not buffer-local, this is equivalent
    to `symbol-value` (see [Accessing Variable Values](#Accessing-Variables)).

Function: **default-boundp** symbol [¶](#index-default_002dboundp)
:   The function `default-boundp` tells you whether symbol’s
    default value is nonvoid. If `(default-boundp 'foo)` returns
    `nil`, then `(default-value 'foo)` would get an error.

    `default-boundp` is to `default-value` as `boundp` is to
    `symbol-value`.

Special Form: **setq-default** [symbol form]… [¶](#index-setq_002ddefault)
:   This special form gives each symbol a new default value, which is
    the result of evaluating the corresponding form. It does not
    evaluate symbol, but does evaluate form. The value of the
    `setq-default` form is the value of the last form.

    If a symbol is not buffer-local for the current buffer, and is not
    marked automatically buffer-local, `setq-default` has the same
    effect as `setq`. If symbol is buffer-local for the current
    buffer, then this changes the value that other buffers will see (as long
    as they don’t have a buffer-local value), but not the value that the
    current buffer sees.

    ```
    ;; In buffer ‘foo’:
    (make-local-variable 'buffer-local)
         ⇒ buffer-local
    ```

    ```
    (setq buffer-local 'value-in-foo)
         ⇒ value-in-foo
    ```

    ```
    (setq-default buffer-local 'new-default)
         ⇒ new-default
    ```

    ```
    buffer-local
         ⇒ value-in-foo
    ```

    ```
    (default-value 'buffer-local)
         ⇒ new-default
    ```

    ```

    ```

    ```
    ;; In (the new) buffer ‘bar’:
    buffer-local
         ⇒ new-default
    ```

    ```
    (default-value 'buffer-local)
         ⇒ new-default
    ```

    ```
    (setq buffer-local 'another-default)
         ⇒ another-default
    ```

    ```
    (default-value 'buffer-local)
         ⇒ another-default
    ```

    ```

    ```

    ```
    ;; Back in buffer ‘foo’:
    buffer-local
         ⇒ value-in-foo
    (default-value 'buffer-local)
         ⇒ another-default
    ```

Function: **set-default** symbol value [¶](#index-set_002ddefault)
:   This function is like `setq-default`, except that symbol is
    an ordinary evaluated argument.

    ```
    (set-default (car '(a b c)) 23)
         ⇒ 23
    ```

    ```
    (default-value 'a)
         ⇒ 23
    ```

A variable can be let-bound (see [Local Variables](#Local-Variables)) to a value.
This makes its global value shadowed by the binding;
`default-value` will then return the value from that binding, not
the global value, and `set-default` will be prevented from
setting the global value (it will change the let-bound value instead).
The following two functions allow referencing the global value even
if it’s shadowed by a let-binding.

Function: **default-toplevel-value** symbol [¶](#index-default_002dtoplevel_002dvalue)
:   This function returns the *top-level* default value of
    symbol, which is its value outside of any let-binding.

```
(defvar variable 'global-value)
    ⇒ variable
```

```
(let ((variable 'let-binding))
  (default-value 'variable))
    ⇒ let-binding
```

```
(let ((variable 'let-binding))
  (default-toplevel-value 'variable))
    ⇒ global-value
```

Function: **set-default-toplevel-value** symbol value [¶](#index-set_002ddefault_002dtoplevel_002dvalue)
:   This function sets the top-level default value of symbol to the
    specified value. This comes in handy when you want to set the
    global value of symbol regardless of whether your code runs in
    the context of symbol’s let-binding.

A variable’s buffer-local value may also be shadowed by a
let-binding. Two functions allow getting and setting the
top-level buffer-local value of a variable, i.e., the value outside of
the let-binding.

Function: **buffer-local-toplevel-value** symbol &optional buffer [¶](#index-buffer_002dlocal_002dtoplevel_002dvalue)
:   This function returns the local value for symbol in buffer,
    defaulting to the current buffer, outside of any let-binding. If
    symbol is not local in buffer, this function signals an
    error.

Function: **set-buffer-local-toplevel-value** symbol value &optional buffer [¶](#index-set_002dbuffer_002dlocal_002dtoplevel_002dvalue)
:   This function sets the local value of symbol to value in
    buffer, defaulting to the current buffer, outside of any
    let-binding.

    symbol is made local in buffer if it was not already. For
    global variables, this means symbol will have a separate value in
    buffer; for variables that are automatically buffer-local, this
    function causes a local value for them to exist in buffer. If
    symbol is permanently buffer-local, value will now persist
    as the buffer-local value for the variable across changes of major mode.

    This is useful when you want to make a change to a buffer-local value
    that will persist after the command now being executed completes,
    where your code may be executed with that variable let-bound. In this
    case the usual way of setting buffer-local values, using `setq-local`,
    will only change the value inside the let-binding, and not the underlying
    buffer-local value. This function sets the latter.

---

Next: [Directory Local Variables](#Directory-Local-Variables), Previous: [Buffer-Local Variables](#Buffer_002dLocal-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.12 File Local Variables [¶](#File-Local-Variables-1)

A file can specify local variable values; Emacs uses these to create
buffer-local bindings for those variables in the buffer visiting that
file. See [Local Variables in Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Variables) in The
GNU Emacs Manual, for basic information about file-local variables.
This section describes the functions and variables that affect how
file-local variables are processed.

If a file-local variable could specify an arbitrary function or Lisp
expression that would be called later, visiting a file could take over
your Emacs. Emacs protects against this by automatically setting only
those file-local variables whose specified values are known to be
safe. Other file-local variables are set only if the user agrees.

For additional safety, `read-circle` is temporarily bound to
`nil` when Emacs reads file-local variables (see [Input Functions](#Input-Functions)). This prevents the Lisp reader from recognizing circular
and shared Lisp structures (see [Read Syntax for Circular Objects](#Circular-Objects)).

User Option: **enable-local-variables** [¶](#index-enable_002dlocal_002dvariables)
:   This variable controls whether to process file-local variables.
    The possible values are:

    `t` (the default)
    :   Set the safe variables, and query (once) about any unsafe variables.

    `:safe`
    :   Set only the safe variables and do not query.

    `:all`
    :   Set all the variables and do not query.

    `nil`
    :   Don’t set any variables.

    anything else
    :   Query (once) about all the variables.

Variable: **inhibit-local-variables-regexps** [¶](#index-inhibit_002dlocal_002dvariables_002dregexps)
:   This is a list of regular expressions. If a file has a name
    matching an element of this list, then it is not scanned for
    any form of file-local variable. For examples of why you might want
    to use this, see [How Emacs Chooses a Major Mode](#Auto-Major-Mode).

Variable: **permanently-enabled-local-variables** [¶](#index-permanently_002denabled_002dlocal_002dvariables)
:   Some local variable settings will, by default, be heeded even if
    `enable-local-variables` is `nil`. By default, this is the
    case only for the `lexical-binding` local variable setting, but
    this can be controlled by using this variable, which is a list of
    symbols.

Variable: **safe-local-variable-directories** [¶](#index-safe_002dlocal_002dvariable_002ddirectories)
:   This is a list of directories where local variables are always
    enabled. Directory-local variables loaded from these directories,
    such as the variables in `.dir-locals.el`, will be enabled even
    if they are risky. The directories in this list must be
    fully-expanded absolute file names. They may also be remote
    directories if the variable `enable-remote-dir-locals` is set
    non-`nil`.

Function: **hack-local-variables** &optional handle-mode [¶](#index-hack_002dlocal_002dvariables)
:   This function parses, and binds or evaluates as appropriate, any local
    variables specified by the contents of the current buffer. The variable
    `enable-local-variables` has its effect here. However, this
    function does not look for the ‘`mode:`’ local variable in the
    ‘`-*-`’ line. `set-auto-mode` does that, also taking
    `enable-local-variables` into account (see [How Emacs Chooses a Major Mode](#Auto-Major-Mode)).

    This function also puts into effect directory-local variables as
    specified in `.dir-locals.el`. If the buffer is not visiting any
    file, then the directory-local variables that apply are those which
    would be applicable to files in the `default-directory`
    (see [Directory Local Variables](#Directory-Local-Variables)).

    This function works by walking the alists stored in
    `file-local-variables-alist` and `dir-local-variables-alist`
    and applying each local variable in turn. It calls
    `before-hack-local-variables-hook` and
    `hack-local-variables-hook` before and after applying the
    variables, respectively. It only calls the before-hook if
    `file-local-variables-alist` is non-`nil`; it always calls the
    other hook. This function ignores a ‘`mode`’ element if it specifies
    the same major mode as the buffer already has.

    If the optional argument handle-mode is `t`, then all this
    function does is return a symbol specifying the major mode, if the
    ‘`-*-`’ line or the local variables list specifies one, and
    `nil` otherwise. It does not set the mode or any other
    file-local variable. If handle-mode has any value other than
    `nil` or `t`, any settings of ‘`mode`’ in the
    ‘`-*-`’ line or the local variables list are ignored, and the
    other settings are applied. If handle-mode is `nil`, all
    the file local variables are set.

Variable: **file-local-variables-alist** [¶](#index-file_002dlocal_002dvariables_002dalist)
:   This buffer-local variable holds the alist of file-local variable
    settings. Each element of the alist is of the form
    `(var . value)`, where var is a symbol of
    the local variable and value is its value. When Emacs visits a
    file, it first collects all the file-local variables into this alist,
    and then the `hack-local-variables` function applies them one by
    one.

Variable: **before-hack-local-variables-hook** [¶](#index-before_002dhack_002dlocal_002dvariables_002dhook)
:   Emacs calls this hook immediately before applying file-local variables
    stored in `file-local-variables-alist`.

Variable: **hack-local-variables-hook** [¶](#index-hack_002dlocal_002dvariables_002dhook)
:   Emacs calls this hook immediately after it finishes applying
    file-local variables stored in `file-local-variables-alist`.

You can specify safe values for a variable with a
`safe-local-variable` property. The property has to be a
function of one argument; any value is safe if the function returns
non-`nil` given that value. Many commonly-encountered file
variables have `safe-local-variable` properties; these include
`fill-column`, `fill-prefix`, and `indent-tabs-mode`.
For boolean-valued variables that are safe, use `booleanp` as the
property value.

Since the value of `safe-local-variable` is consulted when
loading files, the function should be efficient and should ideally not
lead to loading any libraries to determine the safeness (e.g., it should
not be an autoloaded function).

If you want to define `safe-local-variable` properties for
variables defined in C source code, add the names and the properties
of those variables to the list in the “Safe local variables” section
of `files.el`.

When defining a user option using `defcustom`, you can set its
`safe-local-variable` property by adding the arguments
`:safe function` to `defcustom` (see [Defining Customization Variables](#Variable-Definitions)). However, a safety predicate defined using `:safe`
will only be known once the package that contains the `defcustom`
is loaded, which is often too late. As an alternative, you can use
the autoload cookie (see [Autoload](#Autoload)) to assign the option its safety
predicate, like this:

```
;;;###autoload (put 'var 'safe-local-variable 'pred)
```

The safe value definitions specified with `autoload` are copied
into the package’s autoloads file (`loaddefs.el` for most
packages bundled with Emacs), and are known to Emacs since the
beginning of a session.

User Option: **safe-local-variable-values** [¶](#index-safe_002dlocal_002dvariable_002dvalues)
:   This variable provides another way to mark some variable values as
    safe. It is a list of cons cells `(var . val)`,
    where var is a variable name and val is a value which is
    safe for that variable.

    When Emacs asks the user whether or not to obey a set of file-local
    variable specifications, the user can choose to mark them as safe.
    Doing so adds those variable/value pairs to
    `safe-local-variable-values`, and saves it to the user’s custom
    file.

User Option: **ignored-local-variable-values** [¶](#index-ignored_002dlocal_002dvariable_002dvalues)
:   If there are some values of particular local variables that you always
    want to ignore completely, you can use this variable. Its value has
    the same form as `safe-local-variable-values`; a file-local
    variable setting to the value that appears in the list will always be
    ignored when processing the local variables specified by the file. As
    with that variable, when Emacs queries the user about whether to obey
    file-local variables, the user can choose to ignore their particular
    values permanently, and that will alter this variable and save it to
    the user’s custom file. Variable-value pairs that appear in this
    variable take precedence over the same pairs in
    `safe-local-variable-values`.

Function: **safe-local-variable-p** sym val [¶](#index-safe_002dlocal_002dvariable_002dp)
:   This function returns non-`nil` if it is safe to give sym
    the value val, based on the above criteria.

Some variables are considered *risky*. If a variable is risky,
it is never entered automatically into
`safe-local-variable-values`; Emacs always queries before setting
a risky variable, unless the user explicitly allows a value by
customizing `safe-local-variable-values` directly.

Any variable whose name has a non-`nil`
`risky-local-variable` property is considered risky. When you
define a user option using `defcustom`, you can set its
`risky-local-variable` property by adding the arguments
`:risky value` to `defcustom` (see [Defining Customization Variables](#Variable-Definitions)). In addition, any variable whose name ends in any of
‘`-command`’, ‘`-frame-alist`’, ‘`-function`’,
‘`-functions`’, ‘`-hook`’, ‘`-hooks`’, ‘`-form`’,
‘`-forms`’, ‘`-map`’, ‘`-map-alist`’, ‘`-mode-alist`’,
‘`-program`’, or ‘`-predicate`’ is automatically considered
risky. The variables ‘`font-lock-keywords`’,
‘`font-lock-keywords`’ followed by a digit, and
‘`font-lock-syntactic-keywords`’ are also considered risky.

Function: **risky-local-variable-p** sym [¶](#index-risky_002dlocal_002dvariable_002dp)
:   This function returns non-`nil` if sym is a risky variable,
    based on the above criteria.

Variable: **ignored-local-variables** [¶](#index-ignored_002dlocal_002dvariables)
:   This variable holds a list of variables that should not be given local
    values by files. Any value specified for one of these variables is
    completely ignored.

The ‘`Eval:`’ “variable” is also a potential loophole, so Emacs
normally asks for confirmation before handling it.

User Option: **enable-local-eval** [¶](#index-enable_002dlocal_002deval)
:   This variable controls processing of ‘`Eval:`’ in ‘`-*-`’ lines
    or local variables
    lists in files being visited. A value of `t` means process them
    unconditionally; `nil` means ignore them; anything else means ask
    the user what to do for each file. The default value is `maybe`.

User Option: **safe-local-eval-forms** [¶](#index-safe_002dlocal_002deval_002dforms)
:   This variable holds a list of expressions that are safe to
    evaluate when found in the ‘`Eval:`’ “variable” in a file
    local variables list.

If the expression is a function call and the function has a
`safe-local-eval-function` property, the property value
determines whether the expression is safe to evaluate. The property
value can be a predicate to call to test the expression, a list of
such predicates (it’s safe if any predicate succeeds), or `t`
(always safe provided the arguments are constant).

Text properties are also potential loopholes, since their values
could include functions to call. So Emacs discards all text
properties from string values specified for file-local variables.

---

Next: [Connection Local Variables](#Connection-Local-Variables), Previous: [File Local Variables](#File-Local-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.13 Directory Local Variables [¶](#Directory-Local-Variables-1)

A directory can specify local variable values common to all files in
that directory; Emacs uses these to create buffer-local bindings for
those variables in buffers visiting any file in that directory. This
is useful when the files in the directory belong to some *project*
and therefore share the same local variables.

There are two different methods for specifying directory local
variables: by putting them in a special file, or by defining a
*project class* for that directory.

Constant: **dir-locals-file** [¶](#index-dir_002dlocals_002dfile)
:   This constant is the name of the file where Emacs expects to find the
    directory-local variables. The name of the file is
    `.dir-locals.el`[14](#FOOT14). A file by that name in a directory causes Emacs to apply its
    settings to any file in that directory or any of its subdirectories
    (optionally, you can exclude subdirectories; see below).
    If some of the subdirectories have their own `.dir-locals.el`
    files, Emacs uses the settings from the deepest file it finds starting
    from the file’s directory and moving up the directory tree. This
    constant is also used to derive the name of a second dir-locals file
    `.dir-locals-2.el`. If this second dir-locals file is present in
    the same directory as `.dir-locals.el`, then it will be loaded in
    addition to `.dir-locals.el`. This is useful
    when `.dir-locals.el` is under version control in a shared
    repository and cannot be used for personal customizations. The file
    specifies local variables as a specially formatted list; see
    [Per-directory Local Variables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Directory-Variables) in The
    GNU Emacs Manual, for more details.

Function: **hack-dir-local-variables** [¶](#index-hack_002ddir_002dlocal_002dvariables)
:   This function reads the `.dir-locals.el` file and stores the
    directory-local variables in `file-local-variables-alist` that is
    local to the buffer visiting any file in the directory, without
    applying them. It also stores the directory-local settings in
    `dir-locals-class-alist`, where it defines a special class for
    the directory in which `.dir-locals.el` file was found. This
    function works by calling `dir-locals-set-class-variables` and
    `dir-locals-set-directory-class`, described below.

Function: **hack-dir-local-variables-non-file-buffer** [¶](#index-hack_002ddir_002dlocal_002dvariables_002dnon_002dfile_002dbuffer)
:   This function looks for directory-local variables, and immediately
    applies them in the current buffer. It is intended to be called in
    the mode commands for non-file buffers, such as Dired buffers, to let
    them obey directory-local variable settings. For non-file buffers,
    Emacs looks for directory-local variables in `default-directory`
    and its parent directories.

Function: **dir-locals-set-class-variables** class variables [¶](#index-dir_002dlocals_002dset_002dclass_002dvariables)
:   This function defines a set of variable settings for the named
    class, which is a symbol. You can later assign the class to one
    or more directories, and Emacs will apply those variable settings to
    all files in those directories. The list in variables can be of
    one of the two forms: `(major-mode . alist)` or
    `(directory . list)`. With the first form, if the
    file’s buffer turns on a mode that is derived from major-mode,
    then all the variables in the associated alist are applied;
    alist should be of the form `(name . value)`.
    A special value `nil` for major-mode means the settings are
    applicable to any mode. In alist, you can use a special
    name: `subdirs`. If the associated value is
    `nil`, the alist is only applied to files in the relevant
    directory, not to those in any subdirectories.

    With the second form of variables, if directory is the
    initial substring of the file’s directory, then list is applied
    recursively by following the above rules; list should be of one
    of the two forms accepted by this function in variables.

Function: **dir-locals-set-directory-class** directory class &optional mtime [¶](#index-dir_002dlocals_002dset_002ddirectory_002dclass)
:   This function assigns class to all the files in `directory`
    and its subdirectories. Thereafter, all the variable settings
    specified for class will be applied to any visited file in
    directory and its children. class must have been already
    defined by `dir-locals-set-class-variables`.

    Emacs uses this function internally when it loads directory variables
    from a `.dir-locals.el` file. In that case, the optional
    argument mtime holds the file modification time (as returned by
    `file-attributes`). Emacs uses this time to check stored
    local variables are still valid. If you are assigning a class
    directly, not via a file, this argument should be `nil`.

Variable: **dir-locals-class-alist** [¶](#index-dir_002dlocals_002dclass_002dalist)
:   This alist holds the class symbols and the associated variable
    settings. It is updated by `dir-locals-set-class-variables`.

Variable: **dir-locals-directory-cache** [¶](#index-dir_002dlocals_002ddirectory_002dcache)
:   This alist holds directory names, their assigned class names, and
    modification times of the associated directory local variables file
    (if there is one). The function `dir-locals-set-directory-class`
    updates this list.

Variable: **dir-local-variables-alist** [¶](#index-dir_002dlocal_002dvariables_002dalist)
:   This buffer-local variable holds the alist of directory-local variable
    settings. Each element of the alist is of the form `(var . value)`, where var is a symbol of the local variable and
    value is its value (this is the same structure as that of
    `file-local-variables-alist`, see [File Local Variables](#File-Local-Variables)). When
    Emacs visits a file, it collects all the directory-local variables into
    this alist, and then the `hack-local-variables` function applies
    them one by one (again, see [File Local Variables](#File-Local-Variables)).

Variable: **hack-dir-local-get-variables-functions** [¶](#index-hack_002ddir_002dlocal_002dget_002dvariables_002dfunctions)
:   This special hook holds the functions that gather the directory-local
    variables to use for a given buffer. By default it contains just the
    function that obeys the other settings described in the present section.
    But it can be used to add support for more sources of directory-local
    variables, such as those used by other text editors.

    The functions on this hook are called with no argument, in the buffer to
    which we intend to apply the directory-local variables, after the
    buffer’s major mode function has been run, so they can use sources of
    information such as `major-mode` or `buffer-file-name` to find
    the variables that should be applied.

    It should return either a cons cell of the form `(directory
    . alist)` or a list of such cons-cells. A `nil` return value
    means that it found no directory-local variables. directory
    should be a string: the name of the directory to which the variables
    apply. alist is a list of variables together with their values
    that apply to the current buffer, where every element is of the form
    `(varname . value)`.

    The various alist returned by these functions will be combined,
    and in case of conflicts, the settings coming from deeper directories
    will take precedence over those coming from higher directories in the
    directory hierarchy. Finally, since this hook is run every time we visit
    a file it is important to try and keep those functions efficient, which
    will usually require some kind of caching.

Variable: **enable-dir-local-variables** [¶](#index-enable_002ddir_002dlocal_002dvariables)
:   If `nil`, directory-local variables are ignored. This variable
    may be useful for modes that want to ignore directory-locals while
    still respecting file-local variables (see [File Local Variables](#File-Local-Variables)).

---

Next: [Variable Aliases](#Variable-Aliases), Previous: [Directory Local Variables](#Directory-Local-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.14 Connection Local Variables [¶](#Connection-Local-Variables-1)

Connection-local variables provide a general mechanism for different
variable settings in buffers with a remote connection (see [Remote Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Remote-Files) in The GNU Emacs Manual). They are bound
and set depending on the remote connection a buffer is dedicated to.

* [Connection Local Profiles](#Connection-Local-Profiles)
* [Applying Connection Local Variables](#Applying-Connection-Local-Variables)

---

Next: [Applying Connection Local Variables](#Applying-Connection-Local-Variables), Up: [Connection Local Variables](#Connection-Local-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.14.1 Connection Local Profiles [¶](#Connection-Local-Profiles-1)

Emacs uses connection-local profiles to store the variable settings
to apply to particular connections. You can then associate these with
remote connections by defining the criteria when they should apply,
using `connection-local-set-profiles`.

Function: **connection-local-set-profile-variables** profile variables [¶](#index-connection_002dlocal_002dset_002dprofile_002dvariables)
:   This function defines a set of variable settings for the connection
    profile, which is a symbol. You can later assign the connection
    profile to one or more remote connections, and Emacs will apply those
    variable settings to all process buffers for those connections. The
    list in variables is an alist of the form
    `(name . value)`. Example:

    ```
    (connection-local-set-profile-variables
      'remote-bash
      '((shell-file-name . "/bin/bash")
        (shell-command-switch . "-c")
        (shell-interactive-switch . "-i")
        (shell-login-switch . "-l")))
    ```

    ```

    ```

    ```
    (connection-local-set-profile-variables
      'remote-ksh
      '((shell-file-name . "/bin/ksh")
        (shell-command-switch . "-c")
        (shell-interactive-switch . "-i")
        (shell-login-switch . "-l")))
    ```

    ```

    ```

    ```
    (connection-local-set-profile-variables
      'remote-null-device
      '((null-device . "/dev/null")))
    ```

    If you want to append variable settings to an existing profile, you
    could use the function `connection-local-get-profile-variables`
    in order to retrieve the existing settings, like

    ```
    (connection-local-set-profile-variables
      'remote-bash
      (append
       (connection-local-get-profile-variables 'remote-bash)
       '((shell-command-dont-erase-buffer . t))))
    ```

User Option: **connection-local-profile-alist** [¶](#index-connection_002dlocal_002dprofile_002dalist)
:   This alist holds the connection profile symbols and the associated
    variable settings. It is updated by
    `connection-local-set-profile-variables`.

Function: **connection-local-set-profiles** criteria &rest profiles [¶](#index-connection_002dlocal_002dset_002dprofiles)
:   This function assigns profiles, which are symbols, to all remote
    connections identified by criteria. criteria is a plist
    identifying a connection and the application using this connection.
    Property names might be `:application`, `:protocol`,
    `:user` and `:machine`. The property value of
    `:application` is a symbol, all other property values are
    strings. All properties are optional; if criteria is `nil`, it
    always applies. Example:

    ```
    (connection-local-set-profiles
      '(:application tramp :protocol "ssh" :machine "localhost")
      'remote-bash 'remote-null-device)
    ```

    ```

    ```

    ```
    (connection-local-set-profiles
      '(:application tramp :protocol "sudo"
        :user "root" :machine "localhost")
      'remote-ksh 'remote-null-device)
    ```

    If criteria is `nil`, it applies for all remote connections.
    Therefore, the example above would be equivalent to

    ```
    (connection-local-set-profiles
      '(:application tramp :protocol "ssh" :machine "localhost")
      'remote-bash)
    ```

    ```

    ```

    ```
    (connection-local-set-profiles
      '(:application tramp :protocol "sudo"
        :user "root" :machine "localhost")
      'remote-ksh)
    ```

    ```

    ```

    ```
    (connection-local-set-profiles
      nil 'remote-null-device)
    ```

    Any connection profile of profiles must have been already
    defined by `connection-local-set-profile-variables`.

User Option: **connection-local-criteria-alist** [¶](#index-connection_002dlocal_002dcriteria_002dalist)
:   This alist contains connection criteria and their assigned profile
    names. The function `connection-local-set-profiles` updates this
    list.

---

Previous: [Connection Local Profiles](#Connection-Local-Profiles), Up: [Connection Local Variables](#Connection-Local-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.14.2 Applying Connection Local Variables [¶](#Applying-Connection-Local-Variables-1)

When writing connection-aware code, you’ll need to collect, and
possibly apply, any connection-local variables. There are several
ways to do this, as described below.

Function: **hack-connection-local-variables** criteria [¶](#index-hack_002dconnection_002dlocal_002dvariables)
:   This function collects applicable connection-local variables
    associated with criteria in
    `connection-local-variables-alist`, without applying them.
    Example:

    ```
    (hack-connection-local-variables
      '(:application tramp :protocol "ssh" :machine "localhost"))
    ```

    ```

    ```

    ```
    connection-local-variables-alist
         ⇒ ((null-device . "/dev/null")
            (shell-login-switch . "-l")
            (shell-interactive-switch . "-i")
            (shell-command-switch . "-c")
            (shell-file-name . "/bin/bash"))
    ```

Function: **hack-connection-local-variables-apply** criteria [¶](#index-hack_002dconnection_002dlocal_002dvariables_002dapply)
:   This function looks for connection-local variables according to
    criteria, and immediately applies them in the current buffer.

Macro: **with-connection-local-application-variables** application &rest body [¶](#index-with_002dconnection_002dlocal_002dapplication_002dvariables)
:   Apply all connection-local variables for `application`, which are
    specified by `default-directory`.

    After that, body is executed, and the connection-local variables
    are unwound. Example:

    ```
    (connection-local-set-profile-variables
      'my-remote-perl
      '((perl-command-name . "/usr/local/bin/perl5")
        (perl-command-switch . "-e %s")))
    ```

    ```

    ```

    ```
    (connection-local-set-profiles
      '(:application my-app :protocol "ssh" :machine "remotehost")
      'my-remote-perl)
    ```

    ```

    ```

    ```
    (let ((default-directory "/ssh:remotehost:/working/dir/"))
      (with-connection-local-application-variables 'my-app
        do something useful))
    ```

Variable: **connection-local-default-application** [¶](#index-connection_002dlocal_002ddefault_002dapplication)
:   The default application, a symbol, to be applied in
    `with-connection-local-variables`, `connection-local-p` and
    `connection-local-value`. It defaults to `tramp`, but you
    can let-bind it to change the application temporarily (see [Local Variables](#Local-Variables)).

    This variable must not be changed globally.

Macro: **with-connection-local-variables** &rest body [¶](#index-with_002dconnection_002dlocal_002dvariables)
:   This is equivalent to
    `with-connection-local-application-variables`, but uses
    `connection-local-default-application` for the application.

Macro: **setq-connection-local** [symbol form]… [¶](#index-setq_002dconnection_002dlocal)
:   This macro sets each symbol connection-locally to the result of
    evaluating the corresponding form, using the connection-local
    profile specified in `connection-local-profile-name-for-setq`; if
    the profile name is `nil`, this macro will just set the variables
    normally, as with `setq` (see [Setting Variable Values](#Setting-Variables)).

    For example, you can use this macro in combination with
    `with-connection-local-variables` or
    `with-connection-local-application-variables` to lazily
    initialize connection-local settings:

    ```
    (defvar my-app-variable nil)

    (connection-local-set-profile-variables
     'my-app-connection-default-profile
     '((my-app-variable . nil)))

    (connection-local-set-profiles
     '(:application my-app)
     'my-app-connection-default-profile)
    ```

    ```

    ```

    ```
    (defun my-app-get-variable ()
      (with-connection-local-application-variables 'my-app
        (or my-app-variable
            (setq-connection-local my-app-variable
                                   do something useful))))
    ```

Variable: **connection-local-profile-name-for-setq** [¶](#index-connection_002dlocal_002dprofile_002dname_002dfor_002dsetq)
:   The connection-local profile name, a symbol, to use when setting
    variables via `setq-connection-local`. This is let-bound in the
    body of `with-connection-local-variables`, but you can also
    let-bind it yourself if you’d like to set variables on a different
    profile.

    This variable must not be changed globally.

Macro: **connection-local-p** symbol &optional application [¶](#index-connection_002dlocal_002dp)
:   This macro returns non-`nil` if symbol has a
    connection-local binding for application. If application
    is `nil`, the value of
    `connection-local-default-application` is used.

Macro: **connection-local-value** symbol &optional application [¶](#index-connection_002dlocal_002dvalue)
:   This macro returns the connection-local value of symbol for
    application. If application is `nil`, the value of
    `connection-local-default-application` is used.

    If symbol does not have a connection-local
    binding, the value is the default binding of the variable.

    The difference to `with-connection-local{-application}-variables`
    is, that `symbol` is not set buffer-local. A typical usage pattern
    is to use only the the connection value of a variable if it exists, and
    not to use its default value otherwise (using `my-app-variable`
    initialized above):

    ```
    (if (connection-local-p my-app-variable 'my-app)
        (connection-local-value my-app-variable 'my-app)
      ;; Something else.
      )
    ```

Variable: **enable-connection-local-variables** [¶](#index-enable_002dconnection_002dlocal_002dvariables)
:   If `nil`, connection-local variables are ignored. This variable
    shall be changed temporarily only in special modes.

---

Next: [Variables with Restricted Values](#Variables-with-Restricted-Values), Previous: [Connection Local Variables](#Connection-Local-Variables), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.15 Variable Aliases [¶](#Variable-Aliases-1)

It is sometimes useful to make two variables synonyms, so that both
variables always have the same value, and changing either one also
changes the other. Whenever you change the name of a
variable—either because you realize its old name was not well
chosen, or because its meaning has partly changed—it can be useful
to keep the old name as an *alias* of the new one for
compatibility. You can do this with `defvaralias`.

Function: **defvaralias** new-alias base-variable &optional docstring [¶](#index-defvaralias)
:   This function defines the symbol new-alias as a variable alias
    for symbol base-variable. This means that retrieving the value
    of new-alias returns the value of base-variable, and
    changing the value of new-alias changes the value of
    base-variable. The two aliased variable names always share the
    same value and the same bindings.

    If the docstring argument is non-`nil`, it specifies the
    documentation for new-alias; otherwise, the alias gets the same
    documentation as base-variable has, if any, unless
    base-variable is itself an alias, in which case new-alias gets
    the documentation of the variable at the end of the chain of aliases.

    This function returns base-variable.

    If the resulting variable definition chain would be circular, then
    Emacs will signal a `cyclic-variable-indirection` error.

Variable aliases are convenient for replacing an old name for a
variable with a new name. `make-obsolete-variable` declares that
the old name is obsolete and therefore that it may be removed at some
stage in the future.

Function: **make-obsolete-variable** obsolete-name current-name when &optional access-type [¶](#index-make_002dobsolete_002dvariable)
:   This function makes the byte compiler warn that the variable
    obsolete-name is obsolete. If current-name is a symbol,
    it is the variable’s new name; then the warning message says to use
    current-name instead of obsolete-name. If
    current-name is a string, this is the message (after passing
    through `substitute-command-keys`) and there is no replacement
    variable. when should be a string indicating when the variable
    was first made obsolete (usually a version number string).

    The optional argument access-type, if non-`nil`, should
    specify the kind of access that will trigger obsolescence warnings; it
    can be either `get` or `set`.

You can make two variables synonyms and declare one obsolete at the
same time using the macro `define-obsolete-variable-alias`.

Macro: **define-obsolete-variable-alias** obsolete-name current-name when &optional docstring [¶](#index-define_002dobsolete_002dvariable_002dalias)
:   This macro marks the variable obsolete-name as obsolete and also
    makes it an alias for the variable current-name. It is
    equivalent to the following:

    ```
    (defvaralias obsolete-name current-name docstring)
    (make-obsolete-variable obsolete-name current-name when)
    ```

    This macro evaluates all its parameters, and both obsolete-name
    and current-name should be symbols, so a typical usage would
    look like:

    ```
    (define-obsolete-variable-alias 'foo-thing 'bar-thing "27.1")
    ```

Function: **indirect-variable** variable [¶](#index-indirect_002dvariable)
:   This function returns the variable at the end of the chain of aliases
    of variable. If variable is not a symbol, or if variable is
    not defined as an alias, the function returns variable.

```
(defvaralias 'foo 'bar)
(indirect-variable 'foo)
     ⇒ bar
(indirect-variable 'bar)
     ⇒ bar
(setq bar 2)
bar
     ⇒ 2
```

```
foo
     ⇒ 2
```

```
(setq foo 0)
bar
     ⇒ 0
foo
     ⇒ 0
```

---

Next: [Generalized Variables](#Generalized-Variables), Previous: [Variable Aliases](#Variable-Aliases), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.16 Variables with Restricted Values [¶](#Variables-with-Restricted-Values-1)

Ordinary Lisp variables can be assigned any value that is a valid
Lisp object. However, certain Lisp variables are not defined in Lisp,
but in C. Most of these variables are defined in the C code using
`DEFVAR_LISP`. Like variables defined in Lisp, these can take on
any value. However, some variables are defined using
`DEFVAR_INT` or `DEFVAR_BOOL`. See [Writing Emacs Primitives](#Defining-Lisp-variables-in-C), in particular the
description of functions of the type `syms_of_filename`,
for a brief discussion of the C implementation.

Variables of type `DEFVAR_BOOL` can only take on the values
`nil` or `t`. Attempting to assign them any other value
will set them to `t`:

```
(let ((display-hourglass 5))
  display-hourglass)
     ⇒ t
```

Variable: **byte-boolean-vars** [¶](#index-byte_002dboolean_002dvars)
:   This variable holds a list of all variables of type `DEFVAR_BOOL`.

Variables of type `DEFVAR_INT` can take on only integer values.
Attempting to assign them any other value will result in an error:

```
(setq undo-limit 1000.0)
error→ Wrong type argument: integerp, 1000.0
```

---

Next: [Multisession Variables](#Multisession-Variables), Previous: [Variables with Restricted Values](#Variables-with-Restricted-Values), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
