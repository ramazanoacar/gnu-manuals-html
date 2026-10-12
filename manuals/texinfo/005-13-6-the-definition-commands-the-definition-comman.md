### 13.6 The Definition Commands [¶](#The-Definition-Commands)

This section describes all the definition commands of Texinfo.

* [Functions and Similar Entities](#Functions-Commands)
* [Functions in Typed Languages](#Typed-Functions)
* [Variables and Similar Entities](#Variables-Commands)
* [Variables in Typed Languages](#Typed-Variables)
* [Data Types](#Data-Types)
* [Object-Oriented Programming](#Abstract-Objects)

---

Next: [Functions in Typed Languages](#Typed-Functions), Up: [The Definition Commands](#Def-Cmds-in-Detail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 13.6.1 Functions and Similar Entities [¶](#Functions-and-Similar-Entities)

This section describes the commands for describing functions and similar
entities with simple arguments:

`@deffn category name arguments…` [¶](#index-deffn)
:   The `@deffn` command is the general definition command for
    functions, interactive commands, and similar entities that may take
    simple arguments. You must choose a term to describe the category of entity
    being defined; for example, “Function” could be used if the entity is
    a function. The `@deffn` command is written at the beginning of a
    line and is followed on the same line by the category of entity being
    described, the name of this particular entity, and its arguments, if
    any. Terminate the definition with `@end deffn` on a line of its
    own.

    For example, here is a definition:

    ```
    @deffn Command forward-char nchars
    Move point forward @var{nchars} characters.
    @end deffn
    ```

    This shows a rather terse definition for a “command” named
    `forward-char` with one argument, nchars.

    Where the output format allows, `@deffn` uses a typewriter font
    for name, and a slanted font for the rest of the arguments,
    as would be produced by `@var`.

    Within the text of the description, write an argument name
    explicitly with `@var` to refer to the value of the argument.
    In the example above, we used ‘`@var{nchars}`’ in this way.

    In the extremely unusual case when an argument name contains
    ‘`--`’, or another character sequence which is treated specially
    (see [General Syntactic Conventions](#Conventions)), use `@code` around the special
    characters. This avoids the conversion to typographic en-dashes and
    em-dashes.

`@defun name arguments…` [¶](#index-defun)
:   The `@defun` command is the definition command for functions
    with simple arguments. `@defun` is equivalent to
    ‘`@deffn Function …`’. Terminate the definition with
    `@end defun` on a line of its own.

`@defmac name arguments…` [¶](#index-defmac)
:   The `@defmac` command is the definition command for macros.
    `@defmac` is equivalent to ‘`@deffn Macro …`’ and
    works like `@defun`.

`@defspec name arguments…` [¶](#index-defspec)
:   The `@defspec` command is the definition command for special
    forms. (In Lisp, a special form is an entity much like a function;
    see [Special Forms](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Special-Forms) in GNU Emacs Lisp Reference Manual.)
    `@defspec` is equivalent to ‘`@deffn {Special Form}
    …`’ and works like `@defun`.

All these commands create entries in the index of functions.

---

Next: [Variables and Similar Entities](#Variables-Commands), Previous: [Functions and Similar Entities](#Functions-Commands), Up: [The Definition Commands](#Def-Cmds-in-Detail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 13.6.2 Functions in Typed Languages [¶](#Functions-in-Typed-Languages)

The `@deftypefn` command and its variants are generic commands
for describing functions. They are particularly suitable for languages in
which you must declare types of variables and functions, such as C and C++.

`@deftypefn category data-type name arguments…` [¶](#index-deftypefn)
:   The `@deftypefn` command is the general definition command for
    functions and similar entities that may take arguments and that could
    be typed. The `@deftypefn` command is written at the beginning of
    a line and is followed on the same line by the category of entity
    being described, information on the returned value, the name of this
    particular entity, and its arguments, if any.

    For example,

    ```
    @deftypefn {Library Function} int foobar @
      (int @var{foo}, float @var{bar})
    ...
    @end deftypefn
    ```

    produces:

    > Library Function: `int` **foobar** `(int foo, float bar)` [¶](#index-foobar-1)
    > :   …

    This means that `foobar` is a “library function” that returns an
    `int`, and its arguments are foo (an `int`) and
    bar (a `float`). ‘`Library Function`’ has to be
    enclosed in braces to make it a single argument.

    When using `@deftypefn` command and variations,
    you should mark parameter names with
    `@var` to distinguish these from data type names, keywords, and
    other parts of the literal syntax of the programming language.
    Where the output format allows,
    all output on the definition line is in a typewriter font by default.
    `@var` uses an appropriate font where it occurs.

    If you are describing a procedure in a language that has packages,
    such as Ada, you might consider using `@deftypefn` in the
    following manner:

    ```
    @deftypefn stacks private push @
           (@var{s}:in out stack; @
           @var{n}:in integer)
    ...
    @end deftypefn
    ```

    (In these examples the `@deftypefn` arguments are shown using
    continuations (see [Definition Command Continuation Lines](#Def-Cmd-Continuation-Lines)), but could be on a
    single line.)

    In this instance, the procedure is classified as belonging to the
    package `stacks` rather than classified as a ‘procedure’ and its
    data type is described as `private`. (The name of the procedure
    is `push`, and its arguments are s and n.) Output:

    stacks: `private` **push** `(s:in out stack; n:in integer)` [¶](#index-push)
    :   …

    `@deftypefn` and variants could be used when it is convenient to
    interpret the arguments list as literal computer code, marking
    argument names with `@var`.
    It is not necessary for any type names to appear,
    and you can give an empty return type as ‘`{}`’.

    The information on return values may be used to name the return
    variables. For example, in Perl, returned scalars may be given the
    $result and $status name in the following manner:

    ```
    @deftypefn {Sub} {(@var{$result}, @var{$status} =)} @
                         process (@var{$input})
    ... Set @var{$status} to 0 in case of failure ...
    @end deftypefn
    ```

    to get output like

    Sub: `($result, $status) =` **process** `($input)` [¶](#index-process)
    :   … Set $status to 0 in case of failure …

    `@deftypefn` creates an entry in the index of functions for
    name.

`@deftypefun data-type name arguments…` [¶](#index-deftypefun)
:   The `@deftypefun` command is the specialized definition command
    for functions. The command is equivalent to ‘`@deftypefn Function
    …`’.

    `@deftypefun` creates an entry in the index of functions for
    name.

Ordinarily, the return type is printed on the same line as the
function name and arguments, as shown above. In source code, GNU
style for typed functions is to put the return type on a line by itself.
So Texinfo provides an option to do that: `@deftypefnnewline on`.

This affects the generic functions only—not untyped functions with
simple arguments such as `@deffn`, not typed
variables, etc. Specifically, it affects the commands in this
section, and the analogous commands for object-oriented languages,
namely `@deftypeop` and `@deftypemethod`
(see [Object-Oriented Methods](#Object_002dOriented-Methods)).

Specifying `@deftypefnnewline off` reverts to the default.

---

Next: [Variables in Typed Languages](#Typed-Variables), Previous: [Functions in Typed Languages](#Typed-Functions), Up: [The Definition Commands](#Def-Cmds-in-Detail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 13.6.3 Variables and Similar Entities [¶](#Variables-and-Similar-Entities)

Here are the commands for defining variables and similar
entities:

`@defvr category name` [¶](#index-defvr)
:   The `@defvr` command is a general definition command for
    something like a variable—an entity that records a value. You must
    choose a term to describe the category of entity being defined; for
    example, “Variable” could be used if the entity is a variable.
    Write the `@defvr` command at the beginning of a line and
    follow it on the same line by the category of the entity and the
    name of the entity.

    We recommend capitalizing the category name like a title. If the name
    of the category contains spaces, as in the name “User Option”,
    enclose it in braces. Otherwise, the second word will be mistaken for
    the name of the entity. For example,

    ```
    @defvr {User Option} fill-column
    This buffer-local variable specifies
    the maximum width of filled lines.
    ...
    @end defvr
    ```

    Terminate the definition with `@end defvr` on a line of its
    own.

    `@defvr` creates an entry in the index of variables for name.

`@defvar name` [¶](#index-defvar)
:   The `@defvar` command is the definition command for variables.
    `@defvar` is equivalent to ‘`@defvr Variable
    …`’.

    For example:

    ```
    @defvar kill-ring
    ...
    @end defvar
    ```

    `@defvar` creates an entry in the index of variables for
    name.

`@defopt name` [¶](#index-defopt)
:   The `@defopt` command is the definition command for *user
    options*, i.e., variables intended for users to change according to
    taste; Emacs has many such (see [Variables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Variables) in The GNU Emacs
    Manual). `@defopt` is equivalent to ‘`@defvr {User
    Option} …`’.

---

Next: [Data Types](#Data-Types), Previous: [Variables and Similar Entities](#Variables-Commands), Up: [The Definition Commands](#Def-Cmds-in-Detail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 13.6.4 Variables in Typed Languages [¶](#Variables-in-Typed-Languages)

Variables in typed languages are handled in a manner similar to
functions in typed languages. See [Functions in Typed Languages](#Typed-Functions). The general
definition command `@deftypevr` corresponds to
`@deftypefn` and the specialized definition command
`@deftypevar` corresponds to `@deftypefun`.

`@deftypevr category data-type name` [¶](#index-deftypevr)
:   The `@deftypevr` command is the general definition command for
    something like a variable in a typed language—an entity that records
    a value. You must choose a term to describe the category of the
    entity being defined; for example, “Variable” could be used if the
    entity is a variable.

    The `@deftypevr` command is written at the beginning of a line
    and is followed on the same line by the category of the entity
    being described, the data type, and the name of this particular
    entity.

    For example:

    ```
    @deftypevr {Global Flag} int enable
    ...
    @end deftypevr
    ```

    produces the following:

    > Global Flag: `int` **enable** [¶](#index-enable)
    > :   …

`@deftypevar data-type name` [¶](#index-deftypevar)
:   The `@deftypevar` command is the specialized definition command
    for variables in typed languages. `@deftypevar` is equivalent
    to ‘`@deftypevr Variable …`’.

These commands create entries in the index of variables.

---

Next: [Object-Oriented Programming](#Abstract-Objects), Previous: [Variables in Typed Languages](#Typed-Variables), Up: [The Definition Commands](#Def-Cmds-in-Detail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 13.6.5 Data Types [¶](#Data-Types-1)

Here is the command for data types:

`@deftp category name attributes…` [¶](#index-deftp)
:   The `@deftp` command is the generic definition command for data
    types. The command is written at the beginning of a line and is
    followed on the same line by the category, by the name of the type
    (which is a word like `int` or `float`), and then by names of
    attributes of objects of that type. Thus, you could use this command
    for describing `int` or `float`, in which case you could use
    `data type` as the category. (A data type is a category of
    certain objects for purposes of deciding which operations can be
    performed on them.)

    In Lisp, for example, *pair* names a particular data
    type, and an object of that type has two slots called the
    CAR and the CDR. Here is how you would write the first line
    of a definition of `pair`.

    ```
    @deftp {Data type} pair car cdr
    ...
    @end deftp
    ```

    `@deftp` creates an entry in the index of data types.

---

Previous: [Data Types](#Data-Types), Up: [The Definition Commands](#Def-Cmds-in-Detail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 13.6.6 Object-Oriented Programming [¶](#Object_002dOriented-Programming)

Here are the commands for formatting descriptions about abstract
objects, such as are used in object-oriented programming. A class is
a defined type of abstract object. An instance of a class is a
particular object that has the type of the class. An instance
variable is a variable that belongs to the class but for which each
instance has its own value.

* [Object-Oriented Variables](#Object_002dOriented-Variables)
* [Object-Oriented Methods](#Object_002dOriented-Methods)

---

Next: [Object-Oriented Methods](#Object_002dOriented-Methods), Up: [Object-Oriented Programming](#Abstract-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 13.6.6.1 Object-Oriented Variables [¶](#Object_002dOriented-Variables-1)

These commands allow you to define different sorts of variables in
object-oriented programming languages.

`@defcv category class name` [¶](#index-defcv)
:   The `@defcv` command is the general definition command for
    variables associated with classes in object-oriented programming. The
    `@defcv` command is followed by three arguments: the category of
    thing being defined, the class to which it belongs, and its
    name. For instance:

    ```
    @defcv {Class Option} Window border-pattern
    ...
    @end defcv
    ```

    produces:

    Class Option of `Window`: **border-pattern** [¶](#index-border_002dpattern)
    :   …

    `@defcv` creates an entry in the index of variables.

`@deftypecv category class data-type name` [¶](#index-deftypecv)
:   The `@deftypecv` command is the definition command for typed
    class variables in object-oriented programming. It is analogous to
    `@defcv` with the addition of the data-type parameter to
    specify the type of the instance variable.

    ```
    @deftypecv {Class Option} Window int border-pattern
    ...
    @end deftypecv
    ```

    produces:

    Class Option of `Window`: `int` **border-pattern** [¶](#index-border_002dpattern-of-Window)
    :   …

    `@deftypecv` creates an entry in the index of variables.

`@defivar class name` [¶](#index-defivar)
:   The `@defivar` command is the definition command for instance
    variables in object-oriented programming. `@defivar` is
    equivalent to ‘`@defcv {Instance Variable} …`’. For
    instance:

    ```
    @defivar Window border-pattern
    ...
    @end defivar
    ```

    produces:

    Instance Variable of `Window`: **border-pattern** [¶](#index-border_002dpattern-of-Window-1)
    :   …

    `@defivar` creates an entry in the index of variables.

`@deftypeivar class data-type name` [¶](#index-deftypeivar)
:   The `@deftypeivar` command is the definition command for typed
    instance variables in object-oriented programming. It is analogous to
    `@defivar` with the addition of the data-type parameter to
    specify the type of the instance variable.

    ```
    @deftypeivar Window int border-pattern
    ...
    @end deftypeivar
    ```

    produces:

    Instance Variable of `Window`: `int` **border-pattern** [¶](#index-border_002dpattern-of-Window-2)
    :   …

    `@deftypeivar` creates an entry in the index of variables.

---

Previous: [Object-Oriented Variables](#Object_002dOriented-Variables), Up: [Object-Oriented Programming](#Abstract-Objects)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 13.6.6.2 Object-Oriented Methods [¶](#Object_002dOriented-Methods-1)

These commands allow you to define different sorts of function-like
entities resembling methods in object-oriented programming languages.
These entities take arguments, as functions do, but are associated with
particular classes of objects.

`@defop category class name arguments…` [¶](#index-defop)
:   The `@defop` command is the definition command for these
    method-like entities with simple arguments.

    For example, some systems have constructs called *wrappers* that
    are associated with classes as methods are, but that act more like
    macros than like functions. You could use `@defop Wrapper` to
    describe one of these.

    Sometimes it is useful to distinguish methods and *operations*.
    You can think of an operation as the specification for a method.
    Thus, a window system might specify that all window classes have a
    method named `expose`; we would say that this window system
    defines an `expose` operation on windows in general. Typically,
    the operation has a name and also specifies the pattern of arguments;
    all methods that implement the operation must accept the same
    arguments, since applications that use the operation do so without
    knowing which method will implement it.

    Often it makes more sense to document operations than methods. For
    example, window application developers need to know about the
    `expose` operation, but need not be concerned with whether a
    given class of windows has its own method to implement this operation.
    To describe this operation, you would write:

    ```
    @defop Operation windows expose
    ```

    The `@defop` command is written at the beginning of a line and
    is followed on the same line by the overall name of the category of
    operation, the name of the class of the operation, the name of the
    operation, and its arguments, if any.

    `@defop` creates an entry, such as ‘`expose` on
    `windows`’, in the index of functions.

`@deftypeop category class data-type name arguments…` [¶](#index-deftypeop)
:   The `@deftypeop` command is the generic definition command for
    operations in object-oriented programming. It is particularly suitable
    for typed object-oriented languages. It is similar to
    `@defop` with the addition of the data-type parameter to
    specify information on the return value of the method, for example the
    return type.

`@defmethod class name arguments…` [¶](#index-defmethod)
:   The `@defmethod` command is the definition command for methods
    in object-oriented programming. A method is a kind of function that
    implements an operation for a particular class of objects and its
    subclasses.

    `@defmethod` is equivalent to ‘`@defop Method …`’.
    The command is written at the beginning of a line and is followed by
    the name of the class of the method, the name of the method, and its
    arguments, if any.

    For example:

    ```
    @defmethod bar-class bar-method argument
    ...
    @end defmethod
    ```

    illustrates the definition for a method called `bar-method` of
    the class `bar-class`. The method takes an argument.

    `@defmethod` creates an entry in the index of functions.

`@deftypemethod class data-type name arguments…` [¶](#index-deftypemethod)
:   The `@deftypemethod` command is the definition command for methods
    in object-oriented languages, in particular for typed languages
    such as C++ and Java. It is similar to the `@defmethod` command
    with the addition of the data-type parameter to specify information
    on the return value of the method, for example the return type.

The commands with information on return values are affected by the
`@deftypefnnewline` option (see [Functions
in Typed Languages](#Typed-Functions)).

---

Next: [Conventions for Writing Definitions](#Def-Cmd-Conventions), Previous: [The Definition Commands](#Def-Cmds-in-Detail), Up: [Definition Commands](#Definition-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 13.7 Generic Definition Commands [¶](#Generic-Definition-Commands-1)

Texinfo provides commands for definitions that do not produce automatic
index entries.

You create a generic definition environment with ‘`@defblock`’
paired with ‘`@end defblock`’. Within this environment, use a
`@defline` or `@deftypeline` line for each symbol you
document. For example:

```
@defblock
@defline Macro mac (arg1, arg2)
Description of mac.
@deftypeline Builtin int foo (int @var{bar})
Description of foo.
@end defblock
```

This produces the output:

Macro: **mac** (arg1, arg2)
:   Description of mac.

Builtin: `int` **foo** `(int bar)`
:   Description of foo.

The syntax of `@defline` is similar to that of `@deffn`.
The first argument gives a category for the definition. Follow this
with the symbol name, followed by any parameters. You should surround
any arguments containing spaces with braces.

You use `@deftypeline` in a similar way to `@deftypefn`,
following the category with a data type, and marking any parameters
with `@var`. (See [Functions in Typed Languages](#Typed-Functions)).)

To share the same description for multiple symbols, you can put several
`@defline` lines together. For example:

```
@defblock
@defline Function set-var (value)
@defline {Settable Variable} var
Description of set-var and var.
@end defblock
```

This produces the output:

Function: **set-var** (value)

Settable Variable: **var**
:   Description of set-var and var.

It may be useful to define line macros (see [Line Macros](#Line-Macros)) in combination
with these commands.

---

Next: [A Sample Function Definition](#Sample-Function-Definition), Previous: [Generic Definition Commands](#Generic-Definition-Commands), Up: [Definition Commands](#Definition-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 13.8 Conventions for Writing Definitions [¶](#Conventions-for-Writing-Definitions)

A manual need not and should not contain more than one definition for
a given name. An appendix containing a summary should use
`@table` rather than the definition commands.

When you write a definition using `@deffn`, `@defun`, or
one of the other definition commands, please take care to use
arguments that indicate the meaning, as with the count argument
to the `forward-word` function. Also, if the name of an argument
contains the name of a type, such as integer, take care that the
argument actually is of that type.

*Fonts.* As Texinfo is a semantic language, you should nearly never need
to specify font styles with explicit font commands in definitions
(see [Fonts for Printing](#Fonts)).
However, you may be need to work around problems for particular
output formats and/or constructs.
Here are some possibilities:

* Explicitly marking a Lisp keyword like ‘`&keyword`’ with
  `@r{&keyword}`, producing &keyword.

  Note such keywords in definition arguments are (at present)
  rendered in roman in TeX, but this formatting is not done in
  any other output format.
* ‘`@r{@slanted{argument}}`’,
  producing *argument*, similar to `@var`
  but avoiding upper-casing its argument in Info output.

  In this and the previous point, `@r` resets the font from that
  being used in the definition line context (slanted or typewriter) to
  a roman, upright style.
* You could use `@t` (or even ‘`@r{@t{…`’, using `@r`
  to reset font styles) to mark literal syntax
  on a definition line where
  text would otherwise be output in the slanted roman style.
  `@code` would be inappropriate here as it adds quotation
  marks in Info output.

Some degree of trial and error may be needed to get the result you
want. As ever, how nested font commands combine depends on
the output format, so should be avoided where possible.

Hopefully, such usages are kept to a minimum. One possibility is to
wrap these in `@macro` (see [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)),
allowing these usages to be easily changed in the future.

---

Previous: [Conventions for Writing Definitions](#Def-Cmd-Conventions), Up: [Definition Commands](#Definition-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 13.9 A Sample Function Definition [¶](#A-Sample-Function-Definition)

Here is a definition from [Calling Functions](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Calling-Functions) in The GNU Emacs
Lisp Reference Manual, using the `@defun` command. The name of the
function, `apply`, follows immediately after the `@defun`
command and it is followed, on the same line, by the parameter list.

> Function: **apply** function &rest arguments [¶](#index-apply)
> :   `apply` calls function with arguments, just
>     like `funcall` but with one difference: the last of
>     arguments is a list of arguments to give to
>     function, rather than a single argument. We also say
>     that this list is *appended* to the other arguments.
>
>     `apply` returns the result of calling function.
>     As with `funcall`, function must either be a Lisp
>     function or a primitive function; special forms and macros
>     do not make sense in `apply`.
>
>     ```
>     (setq f 'list)
>         ⇒ list
>     (apply f 'x 'y 'z)
>     error→ Wrong type argument: listp, z
>     (apply '+ 1 2 '(3 4))
>         ⇒ 10
>     (apply '+ '(1 2 3 4))
>         ⇒ 10
>
>     (apply 'append '((a b c) nil (x y z) nil))
>         ⇒ (a b c x y z)
>     ```
>
>     An interesting example of using `apply` is found in the description
>     of `mapcar`.

In the Texinfo source file, this example should look like this:

```
@defun apply function @r{&rest} arguments
@code{apply} calls @var{function} with
@var{arguments}, just like @code{funcall} but with one
difference: the last of @var{arguments} is a list of
arguments to give to @var{function}, rather than a single
argument.  We also say that this list is @dfn{appended}
to the other arguments.
```

```

```

```
@code{apply} returns the result of calling
@var{function}.  As with @code{funcall},
@var{function} must either be a Lisp function or a
primitive function; special forms and macros do not make
sense in @code{apply}.
```

```

```

```
@example
(setq f 'list)
    @result{} list
(apply f 'x 'y 'z)
@error{} Wrong type argument: listp, z
(apply '+ 1 2 '(3 4))
    @result{} 10
(apply '+ '(1 2 3 4))
    @result{} 10

(apply 'append '((a b c) nil (x y z) nil))
    @result{} (a b c x y z)
@end example
```

```

```

```
An interesting example of using @code{apply} is found
in the description of @code{mapcar}.
@end defun
```

In this manual, this function is listed in the Command and Variable
Index under `apply`.

---

Next: [Conditionally Visible Text](#Conditionals), Previous: [Definition Commands](#Definition-Commands), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 14 Internationalization [¶](#Internationalization-1)

Texinfo has some support for writing in languages other than English,
although this area still needs considerable work. (If you are
the one helping to translate the fixed strings written to documents,
see [Internationalization of Document Strings](#Internationalization-of-Document-Strings).)

For a list of the various accented and special characters Texinfo
supports, see [Inserting Accents](#Inserting-Accents).

* [`@documentlanguage ll[_cc]`: Set the Document Language](#g_t_0040documentlanguage)
* [`@documentencoding enc`: Set Input Encoding](#g_t_0040documentencoding)

---

Next: [`@documentencoding enc`: Set Input Encoding](#g_t_0040documentencoding), Up: [Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 14.1 `@documentlanguage ll[_cc]`: Set the Document Language [¶](#g_t_0040documentlanguage-ll_005b_005fcc_005d_003a-Set-the-Document-Language)

The `@documentlanguage` command declares the current document
locale. Write it on a line by itself, near the beginning of the file.

```
@documentlanguage ll[_cc]
```

Include a two-letter ISO 639-2 language code (ll) following
the command name, optionally followed by an underscore and two-letter
ISO 3166 two-letter country code (cc). If you have a
multilingual document, the intent is to be able to use this command
multiple times, to declare each language change. If the command is
not used at all, the default is `en_US` for US English.

As with GNU Gettext (see [Gettext](https://www.gnu.org/software/gettext/manual/gettext.html#Top)), if the country
code is omitted, the main dialect is assumed where possible. For
example, `de` is equivalent to `de_DE` (German as spoken in
Germany).

For Info and other online output, this command changes the translation
of various *document strings* such as “see” in cross-references
(see [Cross-references](#Cross-References)), “Function” in defuns (see [Definition Commands](#Definition-Commands)), and so on. Some strings, such as “Node:”, “Next:”,
“Menu:”, etc., are keywords in Info output, so are not translated
there; they are translated in other output formats.
In DocBook output `@documentlanguage` sets the language for
following sections.

For LaTeX, this command causes code to load the ‘`babel`’ package
to be output in the preamble, and `\selectlanguage` to be output.

For TeX, this command causes a file `txi-locale.tex` to
be read (if it exists). If `@documentlanguage` argument
contains the optional ‘`_cc`’ suffix, this is tried first.
For example, with `@documentlanguage de_DE`, TeX first looks
for `txi-de_DE.tex`, then `txi-de.tex`.

Such a `txi-*` file is intended to redefine the various English
words used in TeX output, such as ‘Chapter’, ‘See’, and so on. We
are aware that individual words like these cannot always be translated
in isolation, and that a very different strategy would be required for
ideographic (among other) scripts. Help in improving Texinfo’s
language support is welcome.

`@documentlanguage` also changes TeX’s current hyphenation
patterns, if the TeX program being run has the necessary support
included. This will generally not be the case for `tex`
itself, but will usually be the case for up-to-date distributions of
the extended TeX programs `etex` (DVI output) and
`pdftex` (PDF output). `texi2dvi` will use the
extended TeXs if they are available (see [Format with `texi2dvi`](#Format-with-texi2dvi)).

Since the lists of language codes and country codes are updated
relatively frequently, we don’t attempt to list them here. The valid
language codes are on the official home page for ISO 639,
<http://www.loc.gov/standards/iso639-2/>. The country codes and
the official web site for ISO 3166 can be found via
<https://en.wikipedia.org/wiki/ISO_3166>.

---

Previous: [`@documentlanguage ll[_cc]`: Set the Document Language](#g_t_0040documentlanguage), Up: [Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 14.2 `@documentencoding enc`: Set Input Encoding [¶](#g_t_0040documentencoding-enc_003a-Set-Input-Encoding)

In the default case, the input and output document encoding are assumed
to be UTF-8, the vast global character encoding, expressed in 8-bit bytes.
UTF-8 is compatible with 7-bit ASCII. It is recommended to use UTF-8
encoding for Texinfo manuals.

The `@documentencoding` command declares the input document encoding,
and also affects the encoding of the output. Write it on a line by itself,
with a valid encoding specification following, near the beginning of the file
if your document encoding is not the default encoding.

```
@documentencoding enc
```

UTF-8 should always be the best choice for the encoding.
Texinfo still supports additional encodings, mainly for compatibility with
older manuals[8](#FOOT8):

`US-ASCII`
:   Character encoding based on the English alphabet.

`ISO-8859-1` [¶](#index-Euro-symbol_002c-and-encodings)

`ISO-8859-15`

`ISO-8859-2`
:   These specify the pre-UTF-8 standard encodings for Western European
    (the first two) and Eastern European languages (the third), respectively.
    ISO 8859-15 replaces some little-used characters from 8859-1 (e.g.,
    precomposed fractions) with more commonly needed ones, such as the
    Euro symbol (€).

    A full description of the encodings is beyond our scope here;
    one useful reference is <http://czyborra.com/charsets/iso8859.html>.

`koi8-r`
:   This was a commonly used encoding for the Russian language before UTF-8.

`koi8-u`
:   This was a commonly used encoding for the Ukrainian language before UTF-8.

In Info output, a so-called ‘Local Variables’ section (see [File
Variables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Variables) in The GNU Emacs Manual) is output including
the output encoding. This allows Info readers to set the encoding
appropriately. It looks like this:

```
Local Variables:
coding: UTF-8
End:
```

By default, for Info and plain text output, `texi2any` outputs
accent constructs and special characters (such as `@'e`)
as the actual UTF-8 sequence or 8-bit character in the output
encoding where possible. If this is not possible, or if the option
`--disable-encoding` is given, an ASCII transliteration is
used instead.

In HTML output, a ‘`<meta>`’ tag is output, in the ‘`<head>`’
section of the HTML, that specifies the output encoding. Web servers and
browsers cooperate to use this information so the correct encoding is
used to display the page, if supported by the system. That looks like
this:

```
<meta http-equiv="Content-Type" content="text/html;
     charset=utf-8">
```

In HTML and LaTeX output, if `OUTPUT_CHARACTERS` is set
(see [Other Customization Variables](#Other-Customization-Variables)), accent constructs and special
characters, such as `@'e` or ``` `` ```, are output as the actual
UTF-8 sequence or 8-bit character in the output encoding where possible.
Otherwise, HTML entities are used for those characters in HTML, and
LaTeX macros are used in LaTeX.

In DocBook output, if the encoding is different from UTF-8,
an `encoding` attribute is added to the XML declaration.
If `OUTPUT_CHARACTERS` is set (see [Other Customization Variables](#Other-Customization-Variables)),
accent constructs such as `@'e` are output as the actual 8-bit or
UTF-8 character in the output encoding where possible. Otherwise XML
entities are used for those constructs.

In TeX output, the characters which are supported in the standard
Computer Modern fonts are output accordingly. For example, this
means using constructed accents rather than precomposed glyphs.
Using a missing character generates a warning message, as does
specifying an unimplemented encoding.

Although modern TeX systems support nearly every script in use in
the world, this wide-ranging support is not available in
`texinfo.tex`, and it’s not feasible to duplicate or incorporate
all that effort.

In LaTeX output, code loading the ‘`inputenc`’ package is output
based on the encoding. This, by itself, does not ensures that all
the characters from the input document can be subsequently output.
The fonts used in the default case should cover the specific Texinfo
glyphs, but not all the possible encoded characters. You may need to
load different fonts in the preamble and use
`\DeclareUnicodeCharacter` with a UTF-8 encoding. For example:

```
@latex
\DeclareUnicodeCharacter{017B}{\.Z}
@end latex
```

Cross-references between Info files in different character encodings
with non-ASCII characters in node names fail. We strongly recommend
using UTF-8 only as the encoding for manuals with non-ASCII characters
in the destinations of cross-references.

---

Next: [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands), Previous: [Internationalization](#Internationalization), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 15 Conditionally Visible Text [¶](#Conditionally-Visible-Text)

The *conditional commands* allow you to use different text for
different output formats, or for general conditions that you define.
For example, you can use them to specify different text for the
printed manual and the Info output.

The conditional commands comprise the following categories.

* Commands specific to an output format (Info, TeX, HTML, …).
* Commands specific to any output format *excluding* a given
  one (e.g., not Info, not TeX, …).
* ‘Raw’ formatter text for any output format, passed straight
  through with minimal (but not zero) interpretation of @-commands.
* Format-independent variable substitutions, and testing if a variable
  is set or clear.

* [Conditional Commands](#Conditional-Commands)
* [Conditional Not Commands](#Conditional-Not-Commands)
* [Raw Formatter Commands](#Raw-Formatter-Commands)
* [Inline Conditionals: `@inline`, `@inlineifelse`, `@inlineraw`](#Inline-Conditionals)
* [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value)
* [Testing for Texinfo Commands: `@ifcommanddefined`, `@ifcommandnotdefined`](#Testing-for-Texinfo-Commands)
* [Conditional Nesting](#Conditional-Nesting)

---

Next: [Conditional Not Commands](#Conditional-Not-Commands), Up: [Conditionally Visible Text](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 15.1 Conditional Commands [¶](#Conditional-Commands-1)

Texinfo has an `@ifformat` environment for each output
format, to allow conditional inclusion of text for a particular output
format.

`@ifinfo` begins segments of text that should be ignored when
not producing Info output, in particular in printed output. The
segment of text appears only in the Info file and, for historical
compatibility, the plain text output.

The environments for the other formats are analogous:

`@ifdocbook … @end ifdocbook`
:   Text to appear only in the DocBook output.

`@ifhtml … @end ifhtml`
:   Text to appear only in the HTML output.

`@iflatex … @end iflatex`
:   Text to appear only in the LaTeX output.

`@ifplaintext … @end ifplaintext`
:   Text to appear only in the plain text output.

`@iftex … @end iftex`
:   Text to appear only in the printed manual.

`@ifxml … @end ifxml`
:   Text to appear only in the XML output.

The `@if…` and `@end if…` commands must appear
on lines by themselves in your source file. The newlines following
the commands are (more or less) treated as whitespace, so that the
conditional text is flowed normally into a surrounding paragraph.

The `@if…` constructs are intended to conditionalize
normal Texinfo source; see [Raw Formatter Commands](#Raw-Formatter-Commands), for using
underlying format commands directly.

Here is an example showing all these conditionals:

```
@iftex
This text will appear only in the printed manual.
@end iftex
@ifinfo
However, this text will appear only in Info and plain text.
@end ifinfo
@ifhtml
And this text will only appear in HTML.
@end ifhtml
@iflatex
Moreover, this text will only appear in @LaTeX{}.
@end iflatex
@ifplaintext
Whereas this text will only appear in plain text.
@end ifplaintext
@ifxml
Notwithstanding that this will only appear in XML.
@end ifxml
@ifdocbook
Nevertheless, this will only appear in DocBook.
@end ifdocbook
```

The preceding example produces the following line:

And this text will only appear in HTML.

Notice that you only see one of the input lines, depending on which
version of the manual you are reading.

In complex documents, you may want Texinfo to issue an error message
in some conditionals that should not ever be processed. The
`@errormsg{text}` command will do this; it takes one
argument, the text of the error message.

We mention `@errormsg{}` here even though it is not strictly
related to conditionals, since in practice it is most likely to be
useful in that context. Technically, it can be used anywhere.
See [External Macro Processors: Line Directives](#External-Macro-Processors), for a caveat regarding the line
numbers which `@errormsg` emits in TeX.

---

Next: [Raw Formatter Commands](#Raw-Formatter-Commands), Previous: [Conditional Commands](#Conditional-Commands), Up: [Conditionally Visible Text](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 15.2 Conditional Not Commands [¶](#Conditional-Not-Commands-1)

You can specify text to be included in any output format *other*
than a given one with the `@ifnot…` environments:

```
@ifnotdocbook ... @end ifnotdocbook
@ifnothtml ... @end ifnothtml
@ifnotinfo ... @end ifnotinfo
@ifnotlatex ... @end ifnotlatex
@ifnotplaintext ... @end ifnotplaintext
@ifnottex ... @end ifnottex
@ifnotxml ... @end ifnotxml
```

The `@ifnot…` command and the `@end` command must
appear on lines by themselves in your actual source file.

If the output file is being made in the given format, the
region is *ignored*. Otherwise, it is included.

There is one exception (for historical compatibility):
`@ifnotinfo` text is omitted for both Info and plain text
output, not just Info. To specify text which appears only in Info and
not in plain text, use `@ifnotplaintext`, like this:

```
@ifinfo
@ifnotplaintext
This will be in Info, but not plain text.
@end ifnotplaintext
@end ifinfo
```

The regions delimited by these commands are ordinary Texinfo source as
with `@iftex`, not raw formatter source as with `@tex`
(see [Raw Formatter Commands](#Raw-Formatter-Commands)).

---

Next: [Inline Conditionals: `@inline`, `@inlineifelse`, `@inlineraw`](#Inline-Conditionals), Previous: [Conditional Not Commands](#Conditional-Not-Commands), Up: [Conditionally Visible Text](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 15.3 Raw Formatter Commands [¶](#Raw-Formatter-Commands-1)

The `@if…` conditionals just described must be used only
with normal Texinfo source. For instance, most features of plain
TeX will not work within `@iftex`. The purpose of
`@if…` is to provide conditional processing for Texinfo
source, not provide access to underlying formatting features. For
that, Texinfo provides so-called *raw formatter commands*. They
should only be used when truly required (most documents do not need
them).

The first raw formatter command is `@tex`. You can enter plain
TeX completely, and use ‘`\`’ in the TeX commands, by
delineating a region with the `@tex` and `@end tex`
commands. All plain TeX commands and category codes are restored
within a `@tex` region. The sole exception is that the
`@` character still introduces a command, so that `@end
tex` can be recognized. Texinfo processors will not output material
in such a region unless TeX output is being produced.

In complex cases, you may wish to define new TeX macros within
`@tex`. You must use `\gdef` to do this, not `\def`,
because `@tex` regions are processed in a TeX group. If you
need to make several definitions, you may wish to set
`\globaldefs=1` (its value will be restored to zero as usual when
the group ends at `@end tex`, so it won’t cause problems with
the rest of the document).

As an example, here is a displayed equation written in plain TeX:

```
@tex
$$ \chi^2 = \sum_{i=1}^N
         \left (y_i - (a + b x_i)
         \over \sigma_i\right)^2 $$
@end tex
```

The output of this example will appear only in a printed manual. If
you are reading this in a format not generated by TeX, you will not
see the equation that appears in the printed manual.

Analogously, you can use `@html … @end html` for a region of
raw HTML, `@docbook … @end docbook` for a
region of raw DocBook, `@latex … @end latex` for a
region of raw LaTeX, and `@xml … @end xml` for a
region of raw XML.

The behavior of newlines in raw regions is unspecified.

In all cases, in raw processing, `@` retains the same meaning as
in the remainder of the document. Thus, the Texinfo processors must
recognize and even execute, to some extent, the contents of the raw
regions, regardless of the final output format. Therefore, specifying
changes that globally affect the document inside a raw region leads to
unpredictable and generally undesirable behavior. For example, using
the `@kbdinputstyle` command inside a raw region is undefined.
The remedy is simple: don’t do that.

---

Next: [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value), Previous: [Raw Formatter Commands](#Raw-Formatter-Commands), Up: [Conditionally Visible Text](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 15.4 Inline Conditionals: `@inline`, `@inlineifelse`, `@inlineraw` [¶](#Inline-Conditionals_003a-_0040inline_002c-_0040inlineifelse_002c-_0040inlineraw)

Texinfo provides a set of conditional commands with arguments given
within braces:

`@inlinefmt{format, text}`
:   Process the Texinfo text if format output is being
    generated.

`@inlinefmtifelse{format, then-text, else-text}`
:   Process the Texinfo then-text if format output is being
    generated; otherwise, process else-text.

`@inlineraw{format, text}`
:   Similar, but for raw text (see [Raw Formatter Commands](#Raw-Formatter-Commands)).

The supported format names are:

```
docbook  html  info  latex  plaintext  tex  xml
```

For example,

```
@inlinefmt{html, @emph{HTML-only text}}
```

is nearly equivalent to

```
@ifhtml
@emph{HTML-only text}
@end ifhtml
```

except that no whitespace is added, as happens in the latter
(environment) case.

In these commands, whitespace is ignored after the comma separating
the arguments, as usual, but is *not* ignored at the end of
text.

To insert a literal at sign, left brace, or right brace in one of the
arguments, you must use the alphabetic commands `@atchar{}`
(see [Inserting ‘@’ with `@@` and `@atchar{}`](#Inserting-an-Atsign)), and `@lbracechar{}` or
`@rbracechar{}` (see [Inserting ‘{ ‘}’ with `@{ @}` and `@l rbracechar{}`](#Inserting-Braces)), or the parsing
will become confused.

With `@inlinefmtifelse`, it is also necessary to use
`@comma{}` to avoid mistaking a ‘`,`’ in the text for the
delimiter. With `@inlinefmt` and `@inlineraw`,
`@comma{}` is not required (though it’s fine to use it), since
these commands always have exactly two arguments.

For TeX, the processed text cannot contain newline-delimited
commands. Text to be ignored (i.e., for non-TeX) can, though.

Two other `@inline...` conditionals complement the
`@ifset` and `@ifclear` commands; see the next section.

---

Next: [Testing for Texinfo Commands: `@ifcommanddefined`, `@ifcommandnotdefined`](#Testing-for-Texinfo-Commands), Previous: [Inline Conditionals: `@inline`, `@inlineifelse`, `@inlineraw`](#Inline-Conditionals), Up: [Conditionally Visible Text](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 15.5 Flags: `@set`, `@clear`, conditionals, and `@value` [¶](#Flags_003a-_0040set_002c-_0040clear_002c-conditionals_002c-and-_0040value)

You can direct the Texinfo formatting commands to format or ignore parts
of a Texinfo file with the `@set`, `@clear`, `@ifset`,
and `@ifclear` commands. Here is a summary:

`@set flag [value]`
:   Set the variable flag, to the optional value if specified.

`@clear flag`
:   Undefine the variable flag, whether or not it was previously defined.

`@ifset flag`
:   If flag is set, text through the next `@end ifset` command
    is formatted. If flag is clear, text through the following
    `@end ifset` command is ignored.

`@inlineifset{flag, text}`
:   Brace-delimited version of `@ifset`.

`@ifclear flag`
:   If flag is set, text through the next `@end ifclear` command
    is ignored. If flag is clear, text through the following
    `@end ifclear` command is formatted.

`@inlineifclear{flag, text}`
:   Brace-delimited version of `@ifclear`.

* [`@set` and `@value`](#g_t_0040set-_0040value)
* [`@ifset` and `@ifclear`](#g_t_0040ifset-_0040ifclear)
* [`@inlineifset` and `@inlineifclear`](#g_t_0040inlineifset-_0040inlineifclear)
* [`@value` Example](#g_t_0040value-Example)

---

Next: [`@ifset` and `@ifclear`](#g_t_0040ifset-_0040ifclear), Up: [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 15.5.1 `@set` and `@value` [¶](#g_t_0040set-and-_0040value)

You use the `@set` command to specify a value for a flag, which
is later expanded by the `@value` command.

A *flag* (aka *variable*) name is an identifier starting with
an alphanumeric, ‘`-`’, or ‘`_`’. Subsequent characters, if
any, may not be whitespace, ‘`@`’, braces, angle brackets, or any
of ‘`` ~`^+| ``’; other characters, such as ‘`%`’, may work.
However, it is best to use only letters and numerals in a flag name,
not ‘`-`’ or ‘`_`’ or others—they will work in some contexts,
but not all, due to limitations in TeX.

The value is the remainder of the input line, and can contain anything.

Write the `@set` command like this:

```
@set foo This is a string.
```

This sets the value of the flag `foo` to “This is a string.”.

The Texinfo processors then replace a `@value{flag}`
command with the string to which flag is set. Thus, when
`foo` is set as shown above, the Texinfo processors convert this:

```
@value{foo}
```

```
to this:
```

```
This is a string.
```

You can write a `@value` command within a paragraph; but you
must write a `@set` command on a line of its own.

If you write the `@set` command like this:

```
@set foo
```

without specifying a string, the value of `foo` is the empty string.

If you clear a previously set flag with `@clear flag`, a
subsequent `@value{flag}` command will report an error.

For example, if you set `foo` as follows:

```
@set howmuch very, very, very
```

then the processors transform

```
It is a @value{howmuch} wet day.
```

```
into
```

```
It is a very, very, very wet day.
```

If you write

```
@clear howmuch
```

then the processors transform

```
It is a @value{howmuch} wet day.
```

```
into
```

```
It is a {No value for "howmuch"} wet day.
```

`@value` cannot be reliably used as the argument to an accent
command (see [Inserting Accents](#Inserting-Accents)). For example, this fails:

```
@set myletter a
@'@value{myletter}
```

---

Next: [`@inlineifset` and `@inlineifclear`](#g_t_0040inlineifset-_0040inlineifclear), Previous: [`@set` and `@value`](#g_t_0040set-_0040value), Up: [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 15.5.2 `@ifset` and `@ifclear` [¶](#g_t_0040ifset-and-_0040ifclear)

When a flag is set, the Texinfo formatting commands format text
between subsequent pairs of `@ifset flag` and `@end
ifset` commands. When the flag is cleared, the Texinfo formatting
commands do *not* format the text. `@ifclear` operates
analogously.

Write the conditionally formatted text between `@ifset flag`
and `@end ifset` commands, like this:

```
@ifset flag
conditional-text
@end ifset
```

For example, you can create one document that has two variants, such as
a manual for a ‘large’ and ‘small’ model:

```
You can use this machine to dig up shrubs
without hurting them.

@set large

@ifset large
It can also dig up fully grown trees.
@end ifset

Remember to replant promptly ...
```

In the example, the formatting commands will format the text between
`@ifset large` and `@end ifset` because the `large`
flag is set.

When flag is cleared, the Texinfo formatting commands do
*not* format the text between `@ifset flag` and
`@end ifset`; that text is ignored and does not appear in the output.

For example, if you clear the flag of the preceding example by writing
an `@clear large` command after the `@set large` command
(but before the conditional text), then the Texinfo formatting commands
ignore the text between the `@ifset large` and `@end ifset`
commands. In the formatted output, that text does not appear; you see
only the lines that say, “You can use
this machine to dig up shrubs without hurting them. Remember to replant
promptly …”.

If a flag is cleared with a `@clear flag` command, then
the formatting commands format text between subsequent pairs of
`@ifclear` and `@end ifclear` commands. But if the flag
is set with `@set flag`, then the formatting commands do
*not* format text between an `@ifclear` and an `@end
ifclear` command; rather, they ignore that text. An `@ifclear`
command looks like this:

```
@ifclear flag
```

---

Next: [`@value` Example](#g_t_0040value-Example), Previous: [`@ifset` and `@ifclear`](#g_t_0040ifset-_0040ifclear), Up: [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 15.5.3 `@inlineifset` and `@inlineifclear` [¶](#g_t_0040inlineifset-and-_0040inlineifclear)

`@inlineifset` and `@inlineifclear` provide
brace-delimited alternatives to the `@ifset` and
`@ifclear` forms, similar to the other `@inline...`
Commands (see [Inline Conditionals: `@inline`, `@inlineifelse`, `@inlineraw`](#Inline-Conditionals)). The same caveats about
argument parsing given there apply here too.

`@inlineifset{var, text}`
:   Process the Texinfo text if the flag var is defined.

`@inlineifclear{var, text}`
:   Process the Texinfo text if the flag var is not defined.

Except for the syntax, their general behavior and purposes is the same
as with `@ifset` and `@ifclear`, described in the previous
section.

---

Previous: [`@inlineifset` and `@inlineifclear`](#g_t_0040inlineifset-_0040inlineifclear), Up: [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 15.5.4 `@value` Example [¶](#g_t_0040value-Example-1)

You can use the `@value` command to minimize the number of
places you need to change when you record an update to a manual.
See [GNU Sample Texts](#GNU-Sample-Texts), for the full text of an example of using this
to work with Automake distributions.

This example is adapted from [The GNU Make Manual](https://www.gnu.org/software/make/manual/make.html#Top).

1. Set the flags:

   ```
   @set EDITION 0.35 Beta
   @set VERSION 3.63 Beta
   @set UPDATED 14 August 1992
   @set UPDATE-MONTH August 1992
   ```
2. Write text for the `@copying` section (see [`@copying`: Declare Copying Permissions](#g_t_0040copying)):

   ```
   @copying
   This is Edition @value{EDITION},
   last updated @value{UPDATED},
   of @cite{The GNU Make Manual},
   for @code{make}, version @value{VERSION}.

   Copyright ...

   Permission is granted ...
   @end copying
   ```
3. Write text for the title page, for people reading the printed manual:

   ```
   @titlepage
   @title GNU Make
   @subtitle A Program for Directing Recompilation
   @subtitle Edition @value{EDITION}, ...
   @subtitle @value{UPDATE-MONTH}
   @page
   @insertcopying
   ...
   @end titlepage
   ```

   (On a printed cover, a date listing the month and the year looks less
   fussy than a date listing the day as well as the month and year.)
4. Write text for the Top node, for people reading the Info file:

   ```
   @ifnottex
   @node Top
   @top Make

   This is Edition @value{EDITION},
   last updated @value{UPDATED},
   of @cite{The GNU Make Manual},
   for @code{make}, version @value{VERSION}.
   @end ifnottex
   ```

   After you format the manual, the `@value` constructs have been
   expanded, so the output contains text like this:

   ```
   This is Edition 0.35 Beta, last updated 14 August 1992,
   of `The GNU Make Manual', for `make', Version 3.63 Beta.
   ```

When you update the manual, you change only the values of the flags; you
do not need to edit the three sections.

---

Next: [Conditional Nesting](#Conditional-Nesting), Previous: [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value), Up: [Conditionally Visible Text](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 15.6 Testing for Texinfo Commands: `@ifcommanddefined`, `@ifcommandnotdefined` [¶](#Testing-for-Texinfo-Commands_003a-_0040ifcommanddefined_002c-_0040ifcommandnotdefined)

Occasionally, you may want to arrange for your manual to test if a
given Texinfo command is available and (presumably) do some sort of
fallback formatting if not. There are conditionals
`@ifcommanddefined` and `@ifcommandnotdefined` to do this.
For example:

```
@ifcommanddefined node
Good, @samp{@@node} is defined.
@end ifcommanddefined
```

will output the expected ‘Good, ‘`@node`’ is defined.’.

This conditional will also consider any new commands defined by
the document via `@macro`, `@alias`,
`@definfoenclose`, and `@def(code)index`
(see [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)) to be true. Caveat: the TeX
implementation reports internal TeX commands, in addition to all
the Texinfo commands, as being “defined”; the `texi2any`
implementation is reliable in this regard, however.

You can check the `NEWS` file in the Texinfo source distribution
and linked from the Texinfo home page
(<http://www.gnu.org/software/texinfo>) to see when a particular
command was added.

These command-checking conditionals themselves were added in
Texinfo 5.0, released in 2013—decades after Texinfo’s
inception. In order to test if they themselves are available,
the predefined flag `txicommandconditionals` can be tested, like
this:

```
@ifset txicommandconditionals
@ifcommandnotdefined foobarnode
(Good, @samp{@@foobarnode} is not defined.)
@end ifcommandnotdefined
@end ifset
```

Since flags (see the previous section) were added early in the
existence of Texinfo, there is no problem with assuming they are
available.

We recommend avoiding these tests whenever possible—which is usually
the case. For many software packages, it is reasonable for all
developers to have a given version of Texinfo (or newer) installed,
and thus no reason to worry about older versions. (It is
straightforward for anyone to download and install the Texinfo source;
it does not have any problematic dependencies.)

The issue of Texinfo versions does not generally arise for end users.
With properly distributed packages, users need not process the Texinfo
manual simply to build and install the package; they can use
preformatted Info (or other) output files. This is desirable in
general, to avoid unnecessary dependencies between packages
(see [Releases](https://www.gnu.org/prep/standards/standards.html#Releases) in GNU Coding Standards).

---

Previous: [Testing for Texinfo Commands: `@ifcommanddefined`, `@ifcommandnotdefined`](#Testing-for-Texinfo-Commands), Up: [Conditionally Visible Text](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 15.7 Conditional Nesting [¶](#Conditional-Nesting-1)

Texinfo requires that for a failing conditional, the
ignored text must be properly nested with respect to that failing
conditional. Here’s an example:

```
@ifset somevar
@ifset anothervar
Both somevar and anothervar are set.
@end ifset
@ifclear anothervar
Somevar is set, anothervar is not.
@end ifclear
@end ifset
```

If ‘`somevar`’ is not set, the whole block is skipped.

To allow the processors to reliably determine which commands to consider
for nesting purposes, all conditional commands must be on lines
by themselves, with no text (even spaces) before or after.

---

Next: [Include Files](#Include-Files), Previous: [Conditionally Visible Text](#Conditionals), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 16 Defining New Texinfo Commands [¶](#Defining-New-Texinfo-Commands-1)

Texinfo provides several ways to define new commands (in all cases,
it’s not recommended to try redefining existing commands):

* A Texinfo *macro* allows you to define a new Texinfo command as any
  sequence of text and/or existing commands (including other macros). The
  macro can have any number of *parameters*—text you supply each
  time you use the macro.

  Incidentally, these macros have nothing to do with the `@defmac`
  command, which is for documenting macros in the subject area of the
  manual (see [The Template for a Definition](#Def-Cmd-Template)).
* ‘`@linemacro`’ allows you to define a macro whose arguments
  extend to the end of the line and are separated by spaces.
* ‘`@alias`’ defines a new name for an existing command.
* ‘`@definfoenclose`’ allows you to define new commands with
  customized output for some non-printed output formats. This command
  is deprecated.

Most generally of all (not just for defining new commands), it is
possible to invoke any external macro processor and have Texinfo
recognize so-called `#line` directives for error reporting.

If you want to do simple text substitution, `@set` and
`@value` is the simplest approach (see [Flags: `@set`, `@clear`, conditionals, and `@value`](#g_t_0040set-_0040clear-_0040value)).

* [Defining Macros](#Defining-Macros)
* [Invoking Macros](#Invoking-Macros)
* [Macro Details and Caveats](#Macro-Details)
* [‘`@alias new=existing`’](#g_t_0040alias)
* [Line Macros](#Line-Macros)
* [`@definfoenclose`: Customized Highlighting](#g_t_0040definfoenclose)
* [External Macro Processors: Line Directives](#External-Macro-Processors)

---

Next: [Invoking Macros](#Invoking-Macros), Up: [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 16.1 Defining Macros [¶](#Defining-Macros-1)

You use the Texinfo `@macro` command to define a macro, like this:

```
@macro macroname{param1, param2, ...}
text ... \param1\ ...
@end macro
```

The *parameters* param1, param2, … correspond to
arguments supplied when the macro is subsequently used in the document
(described in the next section).

In principle, macroname should consist of alphanumerics,
and (except as the first character) ‘`-`’. The ‘`_`’ character
is excluded so that macros can be called inside `@math` without
a following space (see [`@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math)). However, for a macro to
work consistently with TeX, macroname must consist entirely
of letters: no digits, hyphens, or other special characters.
So, we recommend using only letters. Texinfo commands should not
be redefined as macros.

If a macro needs no parameters, you can define it either with an empty
list (‘`@macro foo {}`’) or with no braces at all (‘`@macro
foo`’).

The definition or *body* of the macro can contain most Texinfo
commands, including macro invocations. However, a macro definition
that defines another macro does not work in TeX due to limitations
in the design of `@macro`.

In the macro body, instances of a parameter name surrounded by
backslashes, as in ‘`\param1\`’ in the example above, are
replaced by the corresponding argument from the macro invocation. You
can use parameter names any number of times in the body, including zero.

To get a single ‘`\`’ in the macro expansion, use ‘`\\`’. Any
other use of ‘`\`’ in the body yields a warning.

The newline characters after the `@macro` line and before the
`@end macro` line are ignored, that is, not included in the
macro body. All other whitespace is treated according to the usual
Texinfo rules.

To allow a macro to be used recursively, that is, in an argument to a
call to itself, you must define it with ‘`@rmacro`’, like this:

```
@rmacro rmac {arg}
a\arg\b
@end rmacro
...
@rmac{1@rmac{text}2}
```

This produces the output ‘a1atextb2b’. With ‘`@macro`’ instead of
‘`@rmacro`’, an error message is given.

You can undefine a macro foo with `@unmacro foo`.
It is not an error to undefine a macro that is already undefined.
For example:

```
@unmacro foo
```

---

Next: [Macro Details and Caveats](#Macro-Details), Previous: [Defining Macros](#Defining-Macros), Up: [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 16.2 Invoking Macros [¶](#Invoking-Macros-1)

After a macro is defined (see the previous section), you can
*invoke* (use) it in your document like this:

```
@macroname {arg1, arg2, ...}
```

and the result will be more or less as if you typed the body of
macroname at that spot. For example:

```
@macro foo {p, q}
Together: \p\ & \q\.
@end macro
@foo{a, b}
```

produces:

```
Together: a & b.
```

Thus, the arguments and parameters are separated by commas and
delimited by braces; any whitespace after (but not before) a comma is
ignored. The braces are required in the invocation even when the
macro takes no arguments, consistent with other Texinfo commands. For
example:

```
@macro argless {}
No arguments here.
@end macro
@argless{}
```

produces:

```
No arguments here.
```

Passing macro arguments containing commas requires care, since
commas also separate the arguments. To include a comma character in
an argument, the most reliable method is to use the `@comma{}`
command. Another method is to surround the argument with
‘`@asis{…}`’. For `texi2any`, you can also prepend a
backslash character, as in ‘`\,`’, but this does not work with TeX.

It’s not always necessary to worry about commas. To facilitate use of
macros, two rules for *automatic quoting* are implemented:

1. If a macro takes only one argument, all commas in its invocation
   are quoted by default. For example:

   ```
   @macro TRYME{text}
   @strong{TRYME: \text\}
   @end macro

   @TRYME{A nice feature, though it can be dangerous.}
   ```

   will produce the following output

   ```
   TRYME: A nice feature, though it can be dangerous.
   ```

   And indeed, it can. Namely, there is no control on the
   number of arguments passed to one-argument macros, so be careful when
   you invoke them.
2. If a macro invocation includes another command (including a
   recursive invocation of itself), any commas in the nested command
   invocation(s) are quoted by default. For example, in

   ```
   @say{@strong{Yes, I do}, person one}
   ```

   the comma after ‘`Yes`’ is implicitly quoted. Here’s another
   example, with a recursive macro:

   ```
   @rmacro cat{a,b}
   \a\\b\
   @end rmacro

   @cat{@cat{foo, bar}, baz}
   ```

   will produce the string ‘`foobarbaz`’.
3. Otherwise, a comma should be explicitly quoted, as above, for it
   to be treated as a part of an argument.

The backslash itself can be quoted in macro arguments with another
backslash. For example:

```
@macname {\\bleh}
```

will pass the argument ‘`\bleh`’ to macname.

`texi2any` also recognizes ‘`\{`’ and ‘`\}`’ sequences
for curly braces, but these are not recognized by the implementation in
TeX. There should, however, rarely be a need for these, as they are
only needed when a macro argument contains unbalanced braces.

If a macro is defined to take exactly one argument, it can be
invoked without any braces, taking all of the line after the macro name
as the argument. For example:

```
@macro bar {p}
Twice: \p\ & \p\.
@end macro
@bar aah
```

produces:

```
Twice: aah & aah.
```

In these arguments, there is no escaping of special characters, so each
‘`\`’ stands for itself.

If a macro is defined to take more than one argument, but is called
with only one (in braces), the remaining arguments are set to the
empty string, and no error is given. For example:

```
@macro addtwo {p, q}
Both: \p\\q\.
@end macro
@addtwo{a}
```

produces simply:

```
Both: a.
```

---

Next: [‘`@alias new=existing`’](#g_t_0040alias), Previous: [Invoking Macros](#Invoking-Macros), Up: [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 16.3 Macro Details and Caveats [¶](#Macro-Details-and-Caveats)

By design, macro expansion should not happen in the following contexts:

* `@macro` and `@unmacro` lines;
* `@if...` lines, including `@ifset` and similar;
* `@set`, `@clear`, `@value`;
* `@clickstyle` lines;
* `@end` lines when there is no macro expansion in the block
  command.

Unfortunately, TeX may do some expansion in these situations,
possibly yielding errors.

Also, quite a few macro-related constructs cause problems with TeX;
some of the caveats are listed below. Thus, if you get macro-related
errors when producing the printed version of a manual, you might try
expanding the macros with `texi2any` by invoking
`texi2dvi` with the ‘`-E`’ option (see [Format with `texi2dvi`](#Format-with-texi2dvi)). Or, more reliably, eschew Texinfo macros altogether
and use a language designed for macro processing, such as M4
(see [External Macro Processors: Line Directives](#External-Macro-Processors)).

* As mentioned earlier, macro names must consist entirely of letters.
* It is not advisable to redefine any TeX primitive, plain, or
  Texinfo command name as a macro. Unfortunately, this is a large and
  open-ended set of names, and the possible resulting errors are
  unpredictable.
* Arguments to macros taking more than one argument cannot cross lines.
* Macros containing a command which must be on a line by itself, such as
  a conditional, cannot be invoked in the middle of a line. Similarly,
  macros containing line-oriented commands or text, such as
  `@example` environments, may behave unpredictably in TeX.
* Texinfo commands in the expansion of a macro in the text of an index
  entry may end up being typeset as literal text (including an “@”
  sign), instead of being interpreted with their intended meaning.
* White space is ignored at the beginnings of lines.
* Macros can’t be reliably used in the argument to accent commands
  (see [Inserting Accents](#Inserting-Accents)).
* The backslash escape for commas in macro arguments does not work;
  `@comma{}` must be used.
* Ending a macro body with ‘`@c`’ may cause text following the macro
  invocation to be ignored as a comment in `texi2any`. This is
  not the case when processing with TeX. This was often done
  to “comment out” an unwanted newline at the end of a macro body, but
  this is not necessary any more, as the final newline before ‘`@end
  macro`’ is not included in the macro body anyway.
* In general, you can’t arbitrarily substitute a macro (or
  `@value`) call for Texinfo command arguments, even when the text
  is the same. Texinfo is not M4 (or even plain TeX). It might work
  with some commands, it fails with others. Best not to do it at all.
  For instance, this fails:

  ```
  @macro offmacro
  off
  @end macro
  @headings @offmacro
  ```

  This looks equivalent to `@headings off`, but for TeXnical
  reasons, it fails with a mysterious error message (namely,
  ‘`Paragraph ended before @headings was complete`’).
* Expanding macros in a math context may lead to poor spacing in
  TeX output. For example:

  ```
  @iftex
  @macro atan
  \\mathop{\\rm atan}
  @end macro
  @end iftex

  @math{@atan{}(x)}
  ```

  The braces following ‘`@atan`’ are treated by TeX as a subformula,
  leading to extra space before the opening parenthesis. You can work
  around this by defining a raw TeX macro instead:

  ```
  @tex
  \gdef\atan#1{\mathop{\rm atan}}
  @end tex
  ```

  The ‘`#1`’ in this definition absorbs the braces, so that the spacing
  is as expected for the `\mathop` object.
* If you have problems using conditionals within a macro, an alternative
  is to use separate macro definitions inside conditional blocks. For
  example, instead of

  ```
  @macro Mac
  @iftex
  text for TeX output
  @end iftex
  @ifnottex
  text for not TeX output
  @end ifnottex
  @end macro
  ```

  you can do the following instead:

  ```
  @iftex
  @macro Mac
  text for TeX output
  @end macro
  @end iftex

  @ifnottex
  @macro Mac
  text for not TeX output
  @end macro
  @end ifnottex
  ```
* Macros cannot define macros in the natural way. To do this, you must
  use conditionals and raw TeX. For example:

  ```
  @ifnottex
  @macro ctor {name, arg}
  @macro \name\
  something involving \arg\ somehow
  @end macro
  @end macro
  @end ifnottex
  @tex
  \gdef\ctor#1{\ctorx#1,}
  \gdef\ctorx#1,#2,{\def#1{something involving #2 somehow}}
  @end tex
  ```

The following limitations are by design:

* If you want to pass an argument with the Texinfo command
  `@,` (to produce a cedilla, see [Inserting Accents](#Inserting-Accents)), you have
  to use `@value` or another workaround. Otherwise, the comma
  may be taken as separating the arguments. With `texi2any`,
  the comma can be escaped by a backslash. With TeX another workaround
  need to be used, therefore we recommend using such a workaround.

  For example,

  ```
  @macro mactwo{argfirst, argsecond}
  \argfirst\+\argsecond\.
  @end macro
  @set fc Fran@,cois
  @mactwo{@value{fc},}
  ```

  produces:

  ```
  François+.
  ```
* `@verbatim` and macros do not mix; for instance, you can’t start
  a verbatim block inside a macro and end it outside
  (see [`@verbatim`: Literal Text](#g_t_0040verbatim)). Starting any environment inside a macro
  and ending it outside may or may not work, for that matter.
* Macros that completely define macros are ok, but it’s not possible to
  have incompletely nested macro definitions. That is, `@macro`
  and `@end macro` (likewise for `@rmacro`) must be
  correctly paired. For example, you cannot start a macro definition
  within a macro, and then end that nested definition outside the macro.

In the `makeinfo` implementation before Texinfo 5.0, ends of
lines from expansion of a `@macro` definition did not end an
@-command line-delimited argument (`@chapter`, `@center`,
etc.). This is no longer the case. For example:

```
@macro twolines{}
aaa
bbb
@end macro
@center @twolines{}
```

In the current `texi2any`, this is equivalent to:

```
@center aaa
bbb
```

with just ‘`aaa`’ as the argument to `@center`. In
the earlier implementation, it would have been parsed as this:

```
@center aaa bbb
```

---

Next: [Line Macros](#Line-Macros), Previous: [Macro Details and Caveats](#Macro-Details), Up: [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 16.4 ‘`@alias new=existing`’ [¶](#g_t_0040alias-new_003dexisting)

The ‘`@alias`’ command defines a new command to be just like an
existing one. This is useful for defining additional markup names,
thus preserving additional semantic information in the input even
though the output result may be the same.

Write the ‘`@alias`’ command on a line by itself, followed by the
new command name, an equals sign, and the existing command name.
Whitespace around the equals sign is optional and ignored if present.
Thus:

```
@alias new = existing
```

For example, if your document contains citations for both books and
some other media (movies, for example), you might like to define a
macro `@moviecite{}` that does the same thing as an ordinary
`@cite{}` but conveys the extra semantic information as well.
You’d do this as follows:

```
@alias moviecite = cite
```

Macros do not always have the same effect as aliases, due to vagaries
of argument parsing. Also, aliases are much simpler to define than
macros. So the command is not redundant.

Unfortunately, it’s not possible to alias Texinfo environments; for
example, `@alias lang=example` is an error.

Aliases must not be recursive, directly or indirectly.

It is not advisable to redefine any TeX primitive, plain TeX, or
Texinfo command name as an alias. Unfortunately this is a very large
set of names, and the possible resulting errors from TeX are
unpredictable.

Alias identifiers should be the same as for macro names, that is
alphanumerics and (except as the first character) ‘`-`’. However,
with TeX, letters only should be used. So, we recommend using only
letters.

---

Next: [`@definfoenclose`: Customized Highlighting](#g_t_0040definfoenclose), Previous: [‘`@alias new=existing`’](#g_t_0040alias), Up: [Defining New Texinfo Commands](#Defining-New-Texinfo-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
