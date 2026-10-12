### 14.5 Common Problems Using Macros [¶](#Common-Problems-Using-Macros)

Macro expansion can have counterintuitive consequences. This
section describes some important consequences that can lead to
trouble, and rules to follow to avoid trouble.

* [Wrong Time](#Wrong-Time)
* [Evaluating Macro Arguments Repeatedly](#Argument-Evaluation)
* [Local Variables in Macro Expansions](#Surprising-Local-Vars)
* [Evaluating Macro Arguments in Expansion](#Eval-During-Expansion)
* [How Many Times is the Macro Expanded?](#Repeated-Expansion)

---

Next: [Evaluating Macro Arguments Repeatedly](#Argument-Evaluation), Up: [Common Problems Using Macros](#Problems-with-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.5.1 Wrong Time [¶](#Wrong-Time-1)

The most common problem in writing macros is doing some of the
real work prematurely—while expanding the macro, rather than in the
expansion itself. For instance, one real package had this macro
definition:

```
(defmacro my-set-buffer-multibyte (arg)
  (if (fboundp 'set-buffer-multibyte)
      (set-buffer-multibyte arg)))
```

With this erroneous macro definition, the program worked fine when
interpreted but failed when compiled. This macro definition called
`set-buffer-multibyte` during compilation, which was wrong, and
then did nothing when the compiled package was run. The definition
that the programmer really wanted was this:

```
(defmacro my-set-buffer-multibyte (arg)
  (if (fboundp 'set-buffer-multibyte)
      `(set-buffer-multibyte ,arg)))
```

This macro expands, if appropriate, into a call to
`set-buffer-multibyte` that will be executed when the compiled
program is actually run.

---

Next: [Local Variables in Macro Expansions](#Surprising-Local-Vars), Previous: [Wrong Time](#Wrong-Time), Up: [Common Problems Using Macros](#Problems-with-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.5.2 Evaluating Macro Arguments Repeatedly [¶](#Evaluating-Macro-Arguments-Repeatedly)

When defining a macro you must pay attention to the number of times
the arguments will be evaluated when the expansion is executed. The
following macro (used to facilitate iteration) illustrates the
problem. This macro allows us to write a for-loop construct.

```
(defmacro for (var from init to final do &rest body)
  "Execute a simple \"for\" loop.
For example, (for i from 1 to 10 do (print i))."
  (list 'let (list (list var init))
        (cons 'while
              (cons (list '<= var final)
                    (append body (list (list 'inc var)))))))
```

```

```

```
(for i from 1 to 3 do
   (setq square (* i i))
   (princ (format "\n%d %d" i square)))
→
```

```
(let ((i 1))
  (while (<= i 3)
    (setq square (* i i))
    (princ (format "\n%d %d" i square))
    (inc i)))
```

```
     -|1       1
     -|2       4
     -|3       9
⇒ nil
```

The arguments `from`, `to`, and `do` in this macro are
syntactic sugar; they are entirely ignored. The idea is that you
will write noise words (such as `from`, `to`, and `do`)
in those positions in the macro call.

Here’s an equivalent definition simplified through use of backquote:

```
(defmacro for (var from init to final do &rest body)
  "Execute a simple \"for\" loop.
For example, (for i from 1 to 10 do (print i))."
  `(let ((,var ,init))
     (while (<= ,var ,final)
       ,@body
       (inc ,var))))
```

Both forms of this definition (with backquote and without) suffer from
the defect that final is evaluated on every iteration. If
final is a constant, this is not a problem. If it is a more
complex form, say `(long-complex-calculation x)`, this can slow
down the execution significantly. If final has side effects,
executing it more than once is probably incorrect.

A well-designed macro definition takes steps to avoid this problem by
producing an expansion that evaluates the argument expressions exactly
once unless repeated evaluation is part of the intended purpose of the
macro. Here is a correct expansion for the `for` macro:

```
(let ((i 1)
      (max 3))
  (while (<= i max)
    (setq square (* i i))
    (princ (format "%d      %d" i square))
    (inc i)))
```

Here is a macro definition that creates this expansion:

```
(defmacro for (var from init to final do &rest body)
  "Execute a simple for loop: (for i from 1 to 10 do (print i))."
  `(let ((,var ,init)
         (max ,final))
     (while (<= ,var max)
       ,@body
       (inc ,var))))
```

Unfortunately, this fix introduces another problem,
described in the following section.

---

Next: [Evaluating Macro Arguments in Expansion](#Eval-During-Expansion), Previous: [Evaluating Macro Arguments Repeatedly](#Argument-Evaluation), Up: [Common Problems Using Macros](#Problems-with-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.5.3 Local Variables in Macro Expansions [¶](#Local-Variables-in-Macro-Expansions)

In the previous section, the definition of `for` was fixed as
follows to make the expansion evaluate the macro arguments the proper
number of times:

```
(defmacro for (var from init to final do &rest body)
  "Execute a simple for loop: (for i from 1 to 10 do (print i))."
```

```
  `(let ((,var ,init)
         (max ,final))
     (while (<= ,var max)
       ,@body
       (inc ,var))))
```

The new definition of `for` has a new problem: it introduces a
local variable named `max` which the user does not expect. This
causes trouble in examples such as the following:

```
(let ((max 0))
  (for x from 0 to 10 do
    (let ((this (frob x)))
      (if (< max this)
          (setq max this)))))
```

The references to `max` inside the body of the `for`, which
are supposed to refer to the user’s binding of `max`, really access
the binding made by `for`.

The way to correct this is to use an uninterned symbol instead of
`max` (see [Creating and Interning Symbols](#Creating-Symbols)). The uninterned symbol can be
bound and referred to just like any other symbol, but since it is
created by `for`, we know that it cannot already appear in the
user’s program. Since it is not interned, there is no way the user can
put it into the program later. It will never appear anywhere except
where put by `for`. Here is a definition of `for` that works
this way:

```
(defmacro for (var from init to final do &rest body)
  "Execute a simple for loop: (for i from 1 to 10 do (print i))."
  (let ((tempvar (make-symbol "max")))
    `(let ((,var ,init)
           (,tempvar ,final))
       (while (<= ,var ,tempvar)
         ,@body
         (inc ,var)))))
```

This creates an uninterned symbol named `max` and puts it in the
expansion instead of the usual interned symbol `max` that appears
in expressions ordinarily.

---

Next: [How Many Times is the Macro Expanded?](#Repeated-Expansion), Previous: [Local Variables in Macro Expansions](#Surprising-Local-Vars), Up: [Common Problems Using Macros](#Problems-with-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.5.4 Evaluating Macro Arguments in Expansion [¶](#Evaluating-Macro-Arguments-in-Expansion)

Another problem can happen if the macro definition itself
evaluates any of the macro argument expressions, such as by calling
`eval` (see [Eval](#Eval)). You have to take into account that macro
expansion may take place long before the code is executed, when the
context of the caller (where the macro expansion will be evaluated) is
not yet accessible.

Also, if your macro definition does not use `lexical-binding`, its
formal arguments may hide the user’s variables of the same name. Inside
the macro body, the macro argument binding is the most local binding of
such variable, so any references inside the form being evaluated do refer
to it. Here is an example:

```
(defmacro foo (a)
  (list 'setq (eval a) t))
```

```
(setq x 'b)
(foo x) → (setq b t)
     ⇒ t                  ; and b has been set.
;; but
(setq a 'c)
(foo a) → (setq a t)
     ⇒ t                  ; but this set a, not c.
```

It makes a difference whether the user’s variable is named `a` or
`x`, because `a` conflicts with the macro argument variable
`a`.

Also, the expansion of `(foo x)` above will return something
different or signal an error when the code is compiled, since in that case
`(foo x)` is expanded during compilation, whereas the execution of
`(setq x 'b)` will only take place later when the code is executed.

To avoid these problems, **don’t evaluate an argument expression
while computing the macro expansion**. Instead, substitute the
expression into the macro expansion, so that its value will be computed
as part of executing the expansion. This is how the other examples in
this chapter work.

---

Previous: [Evaluating Macro Arguments in Expansion](#Eval-During-Expansion), Up: [Common Problems Using Macros](#Problems-with-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.5.5 How Many Times is the Macro Expanded? [¶](#How-Many-Times-is-the-Macro-Expanded_003f)

Occasionally problems result from the fact that a macro call is
expanded each time it is evaluated in an interpreted function, but is
expanded only once (during compilation) for a compiled function. If the
macro definition has side effects, they will work differently depending
on how many times the macro is expanded.

Therefore, you should avoid side effects in computation of the
macro expansion, unless you really know what you are doing.

One special kind of side effect can’t be avoided: constructing Lisp
objects. Almost all macro expansions include constructed lists; that is
the whole point of most macros. This is usually safe; there is just one
case where you must be careful: when the object you construct is part of a
quoted constant in the macro expansion.

If the macro is expanded just once, in compilation, then the object is
constructed just once, during compilation. But in interpreted
execution, the macro is expanded each time the macro call runs, and this
means a new object is constructed each time.

In most clean Lisp code, this difference won’t matter. It can matter
only if you perform side-effects on the objects constructed by the macro
definition. Thus, to avoid trouble, **avoid side effects on
objects constructed by macro definitions**. Here is an example of how
such side effects can get you into trouble:

```
(defmacro empty-object ()
  (list 'quote (cons nil nil)))
```

```

```

```
(defun initialize (condition)
  (let ((object (empty-object)))
    (if condition
        (setcar object condition))
    object))
```

If `initialize` is interpreted, a new list `(nil)` is
constructed each time `initialize` is called. Thus, no side effect
survives between calls. If `initialize` is compiled, then the
macro `empty-object` is expanded during compilation, producing a
single constant `(nil)` that is reused and altered each time
`initialize` is called.

One way to avoid pathological cases like this is to think of
`empty-object` as a funny kind of constant, not as a memory
allocation construct. You wouldn’t use `setcar` on a constant such
as `'(nil)`, so naturally you won’t use it on `(empty-object)`
either.

---

Previous: [Common Problems Using Macros](#Problems-with-Macros), Up: [Macros](#Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.6 Indenting Macros [¶](#Indenting-Macros-1)

Within a macro definition, you can use the `declare` form
(see [Defining Macros](#Defining-Macros)) to specify how `TAB` should indent
calls to the macro. An indentation specification is written like this:

```
(declare (indent indent-spec))
```

This results in the `lisp-indent-function` property being set on
the macro name.

Here are the possibilities for indent-spec:

`nil`
:   This is the same as no property—use the standard indentation pattern.

`defun`
:   Handle this function like a ‘`def`’ construct: treat the second
    line as the start of a *body*.

an integer, number
:   The first number arguments of the function are
    *distinguished* arguments; the rest are considered the body
    of the expression. A line in the expression is indented according to
    whether the first argument on it is distinguished or not. If the
    argument is part of the body, the line is indented `lisp-body-indent`
    more columns than the open-parenthesis starting the containing
    expression. If the argument is distinguished and is either the first
    or second argument, it is indented *twice* that many extra columns.
    If the argument is distinguished and not the first or second argument,
    the line uses the standard pattern.

a symbol, symbol
:   symbol should be a function name; that function is called to
    calculate the indentation of a line within this expression. The
    function receives two arguments:

    pos
    :   The position at which the line being indented begins.

    state
    :   The value returned by `parse-partial-sexp` (a Lisp primitive for
        indentation and nesting computation) when it parses up to the
        beginning of this line.

    It should return either a number, which is the number of columns of
    indentation for that line, or a list whose car is such a number. The
    difference between returning a number and returning a list is that a
    number says that all following lines at the same nesting level should
    be indented just like this one; a list says that following lines might
    call for different indentations. This makes a difference when the
    indentation is being computed by `C-M-q`; if the value is a
    number, `C-M-q` need not recalculate indentation for the following
    lines until the end of the list.

Variable: **lisp-indent-local-overrides** [¶](#index-lisp_002dindent_002dlocal_002doverrides)
:   This variable can be used to override the indentation specification used
    for certain symbol, as specified using their `lisp-indent-function`
    symbol property (see [Symbol Properties](#Symbol-Properties)). It is intended to be used
    as a file-local or directory-local variable; See [Local Variables in Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Variables) in The GNU Emacs Manual.

    Each key is a symbol and each value is an indent specification, which
    overrides the value of the symbol’s `lisp-indent-function`
    property. The value can take the same forms as the value of the symbol
    property, documented above. Note that using a function is not safe and
    should be avoided, else all users, who visit the file, would have to
    explicitly approve or decline their use.

    This variable is used by the functions `lisp-indent-function`
    and `common-lisp-indent-function`. In case of the latter, the
    symbol properties `common-lisp-indent-function-for-elisp` and
    `common-lisp-indent-function` take precedence not only over the
    `lisp-indent-function` property but also over this variable.

---

Next: [Loading](#Loading), Previous: [Macros](#Macros), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 15 Customization Settings [¶](#Customization-Settings)

Users of Emacs can customize variables and faces without writing
Lisp code, by using the Customize interface. See [Easy
Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Easy-Customization) in The GNU Emacs Manual. This chapter describes
how to define *customization items* that users can interact with
through the Customize interface.

Customization items include customizable variables, which are
defined with the
`defcustom` macro;
customizable faces, which are defined with `defface` (described
separately in [Defining Faces](#Defining-Faces)); and *customization groups*,
defined with
`defgroup`,
which act as containers for groups of related customization items.

* [Common Item Keywords](#Common-Keywords)
* [Defining Customization Groups](#Group-Definitions)
* [Defining Customization Variables](#Variable-Definitions)
* [Customization Types](#Customization-Types)
* [Applying Customizations](#Applying-Customizations)
* [Custom Themes](#Custom-Themes)

---

Next: [Defining Customization Groups](#Group-Definitions), Up: [Customization Settings](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.1 Common Item Keywords [¶](#Common-Item-Keywords)

The customization declarations that we will describe in the next few
sections—`defcustom`, `defgroup`, etc.—all accept
keyword arguments (see [Variables that Never Change](#Constant-Variables)) for specifying various
information. This section describes keywords that apply to all types
of customization declarations.

All of these keywords, except `:tag`, can be used more than once
in a given item. Each use of the keyword has an independent effect.
The keyword `:tag` is an exception because any given item can only
display one name.

`:tag label` [¶](#index-tag_002c-customization-keyword)
:   Use label, a string, instead of the item’s name, to label the
    item in customization menus and buffers. **Don’t use a tag
    which is substantially different from the item’s real name; that would
    cause confusion.**

`:group group` [¶](#index-group_002c-customization-keyword)
:   Put this customization item in group group. If this keyword is
    missing from a customization item, it’ll be placed in the same group
    that was last defined (in the current file).

    When you use `:group` in a `defgroup`, it makes the new
    group a subgroup of group.

    If you use this keyword more than once, you can put a single item into
    more than one group. Displaying any of those groups will show this
    item. Please don’t overdo this, since the result would be annoying.

`:link link-data` [¶](#index-link_002c-customization-keyword)
:   Include an external link after the documentation string for this item.
    This is a sentence containing a button that references some
    other documentation.

    There are several alternatives you can use for link-data:

    `(custom-manual info-node)`
    :   Link to an Info node; info-node is a string which specifies the
        node name, as in `"(emacs)Top"`. The link appears as
        ‘`[Manual]`’ in the customization buffer and enters the built-in
        Info reader on info-node.

    `(info-link info-node)`
    :   Like `custom-manual` except that the link appears
        in the customization buffer with the Info node name.

    `(url-link url)`
    :   Link to a web page; url is a string which specifies the
        URL. The link appears in the customization buffer as
        url and invokes the WWW browser specified by
        `browse-url-browser-function`.

    `(emacs-commentary-link library)`
    :   Link to the commentary section of a library; library is a string
        which specifies the library name. See [Conventional Headers for Emacs Libraries](#Library-Headers).

    `(emacs-library-link library)`
    :   Link to an Emacs Lisp library file; library is a string which
        specifies the library name.

    `(file-link file)`
    :   Link to a file; file is a string which specifies the name of the
        file to visit with `find-file` when the user invokes this link.

    `(function-link function)`
    :   Link to the documentation of a function; function is a string
        which specifies the name of the function to describe with
        `describe-function` when the user invokes this link.

    `(variable-link variable)`
    :   Link to the documentation of a variable; variable is a string
        which specifies the name of the variable to describe with
        `describe-variable` when the user invokes this link.

    `(face-link face)`
    :   Link to the documentation of a face; face is a string which
        specifies the name of the face to describe with `describe-face`
        when the user invokes this link.

    `(custom-group-link group)`
    :   Link to another customization group. Invoking it creates a new
        customization buffer for group.

    You can specify the text to use in the customization buffer by adding
    `:tag name` after the first element of the link-data;
    for example, `(info-link :tag "foo" "(emacs)Top")` makes a link to
    the Emacs manual which appears in the buffer as ‘`foo`’.

    You can use this keyword more than once, to add multiple links.

`:load file` [¶](#index-load_002c-customization-keyword)
:   Load file file (a string) before displaying this customization
    item (see [Loading](#Loading)). Loading is done with `load`, and only if
    the file is not already loaded.

`:require feature` [¶](#index-require_002c-customization-keyword)
:   Execute `(require 'feature)` when your saved customizations
    set the value of this item. feature should be a symbol.

    The most common reason to use `:require` is when a variable enables
    a feature such as a minor mode, and just setting the variable won’t have
    any effect unless the code which implements the mode is loaded.

`:version version` [¶](#index-version_002c-customization-keyword)
:   This keyword specifies that the item was first introduced in Emacs
    version version, or that its default value was changed in that
    version. The value version must be a string.

`:package-version '(package . version)` [¶](#index-package_002dversion_002c-customization-keyword)
:   This keyword specifies that the item was first introduced in
    package version version, or that its meaning or default
    value was changed in that version. This keyword takes priority over
    `:version`.

    package should be the official name of the package, as a symbol
    (e.g., `MH-E`). version should be a string. If the
    package package is released as part of Emacs, package and
    version should appear in the value of
    `customize-package-emacs-version-alist`.

Packages distributed as part of Emacs that use the
`:package-version` keyword must also update the
`customize-package-emacs-version-alist` variable.

Variable: **customize-package-emacs-version-alist** [¶](#index-customize_002dpackage_002demacs_002dversion_002dalist)
:   This alist provides a mapping for the versions of Emacs that are
    associated with versions of a package listed in the
    `:package-version` keyword. Its elements are:

    ```
    (package (pversion . eversion)...)
    ```

    For each package, which is a symbol, there are one or more
    elements that contain a package version pversion with an
    associated Emacs version eversion. These versions are strings.
    For example, the MH-E package updates this alist with the following:

    ```
    (add-to-list 'customize-package-emacs-version-alist
                 '(MH-E ("6.0" . "22.1") ("6.1" . "22.1") ("7.0" . "22.1")
                        ("7.1" . "22.1") ("7.2" . "22.1") ("7.3" . "22.1")
                        ("7.4" . "22.1") ("8.0" . "22.1")))
    ```

    The value of package needs to be unique and it needs to match
    the package value appearing in the `:package-version`
    keyword. Since the user might see the value in an error message, a good
    choice is the official name of the package, such as MH-E or Gnus.

---

Next: [Defining Customization Variables](#Variable-Definitions), Previous: [Common Item Keywords](#Common-Keywords), Up: [Customization Settings](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.2 Defining Customization Groups [¶](#Defining-Customization-Groups)

Each Emacs Lisp package should have one main customization group
which contains all the options, faces and other groups in the package.
If the package has a small number of options and faces, use just one
group and put everything in it. When there are more than twenty or so
options and faces, then you should structure them into subgroups, and
put the subgroups under the package’s main customization group. It is
OK to put some of the options and faces in the package’s main group
alongside the subgroups.

The package’s main or only group should be a member of one or more of
the standard customization groups. (To display the full list of them,
use `M-x customize`.) Choose one or more of them (but not too
many), and add your group to each of them using the `:group`
keyword.

The way to declare new customization groups is with `defgroup`.

Macro: **defgroup** group members doc [keyword value]… [¶](#index-defgroup)
:   Declare group as a customization group containing members.
    Do not quote the symbol group. The argument doc specifies
    the documentation string for the group.

    The argument members is a list specifying an initial set of
    customization items to be members of the group. However, most often
    members is `nil`, and you specify the group’s members by
    using the `:group` keyword when defining those members.

    If you want to specify group members through members, each element
    should have the form `(name widget)`. Here name
    is a symbol, and widget is a widget type for editing that symbol.
    Useful widgets are `custom-variable` for a variable,
    `custom-face` for a face, and `custom-group` for a group.

    When you introduce a new group into Emacs, use the `:version`
    keyword in the `defgroup`; then you need not use it for
    the individual members of the group.

    In addition to the common keywords (see [Common Item Keywords](#Common-Keywords)), you can
    also use this keyword in `defgroup`:

    `:prefix prefix` [¶](#index-prefix_002c-defgroup-keyword)
    :   If the name of an item in the group starts with prefix, and the
        customizable variable `custom-unlispify-remove-prefixes` is
        non-`nil`, the item’s tag will omit prefix. A group can
        have any number of prefixes.

    The variables, faces, and subgroups of a group are stored in the
    `custom-group` property of the group’s symbol. See [Accessing Symbol Properties](#Symbol-Plists). The value of that property is a list of pairs whose
    `car` is the symbol of the variable or the face or the subgroup,
    and the `cdr` is one of the corresponding symbols
    `custom-variable`, `custom-face`, or `custom-group`.

User Option: **custom-unlispify-remove-prefixes** [¶](#index-custom_002dunlispify_002dremove_002dprefixes)
:   If this variable is non-`nil`, the prefixes specified by a
    group’s `:prefix` keyword are omitted from tag names, whenever
    the user customizes the group.

    The default value is `nil`, i.e., the prefix-discarding feature
    is disabled. This is because discarding prefixes often leads to
    confusing names for options and faces.

---

Next: [Customization Types](#Customization-Types), Previous: [Defining Customization Groups](#Group-Definitions), Up: [Customization Settings](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.3 Defining Customization Variables [¶](#Defining-Customization-Variables)

*Customizable variables*, also called *user options*, are
global Lisp variables whose values can be set through the Customize
interface. Unlike other global variables, which are defined with
`defvar` (see [Defining Global Variables](#Defining-Variables)), customizable variables are
defined using the `defcustom` macro. In addition to calling
`defvar` as a subroutine, `defcustom` states how the
variable should be displayed in the Customize interface, the values it
is allowed to take, etc.

Macro: **defcustom** option standard doc [keyword value]… [¶](#index-defcustom)
:   This macro declares option as a user option (i.e., a
    customizable variable). You should not quote option.

    The argument standard is an expression that specifies the
    standard value for option. Evaluating the `defcustom` form
    evaluates standard, but does not necessarily bind the option to
    that value. If option already has a default value, it is left
    unchanged. If the user has already saved a customization for
    option, the user’s customized value is installed as the default
    value. Otherwise, the result of evaluating standard is
    installed as the default value.

    Like `defvar`, this macro marks `option` as a special
    variable, meaning that it should always be dynamically bound. If
    option is already lexically bound, that lexical binding remains
    in effect until the binding construct exits. See [Scoping Rules for Variable Bindings](#Variable-Scoping).

    The expression standard can be evaluated at various other times,
    too—whenever the customization facility needs to know option’s
    standard value. So be sure to use an expression which is harmless to
    evaluate at any time.

    The argument doc specifies the documentation string for the
    variable.

    If a `defcustom` does not specify any `:group`, the last group
    defined with `defgroup` in the same file will be used. This way, most
    `defcustom` do not need an explicit `:group`.

    When you evaluate a `defcustom` form with `C-M-x` in Emacs Lisp
    mode (`eval-defun`), a special feature of `eval-defun`
    arranges to set the variable unconditionally, without testing whether
    its value is void. (The same feature applies to `defvar`,
    see [Defining Global Variables](#Defining-Variables).) Using `eval-defun` on a defcustom
    that is already defined calls the `:set` function (see below),
    if there is one.

    If you put a `defcustom` in a pre-loaded Emacs Lisp file
    (see [Building Emacs](#Building-Emacs)), the standard value installed at dump time
    might be incorrect, e.g., because another variable that it depends on
    has not been assigned the right value yet. In that case, use
    `custom-reevaluate-setting`, described below, to re-evaluate the
    standard value after Emacs starts up.

In addition to the keywords listed in [Common Item Keywords](#Common-Keywords), this
macro accepts the following keywords:

`:type type`
:   Use type as the data type for this option. It specifies which
    values are legitimate, and how to display the value
    (see [Customization Types](#Customization-Types)). Every `defcustom` should specify
    a value for this keyword.

`:options value-list` [¶](#index-options_002c-defcustom-keyword)
:   Specify the list of reasonable values for use in this
    option. The user is not restricted to using only these values, but they
    are offered as convenient alternatives.

    This is meaningful only for certain types, currently including
    `hook`, `plist` and `alist`. See the definition of the
    individual types for a description of how to use `:options`.

    Re-evaluating a `defcustom` form with a different `:options`
    value does not clear the values added by previous evaluations, or
    added by calls to `custom-add-frequent-value` (see below).

`:set setfunction` [¶](#index-set_002c-defcustom-keyword)
:   Specify setfunction as the way to change the value of this
    option when using the Customize interface. The function
    setfunction should take two or three arguments, a symbol (the option
    name), the new value, and an optional buffer-local indicator.
    setfunction should do whatever is necessary to update
    the value properly for this option (which may not mean simply setting
    the option as a Lisp variable); preferably, though, it should not
    modify its value argument destructively. If optional buffer-local
    is non-nil, the new value should be set buffer locally and not affect its
    global or default values. The default for setfunction is
    `set-default-toplevel-value`.

    If defined, setfunction will also be called when evaluating a
    `defcustom` form with `C-M-x` in Emacs Lisp mode and when the
    option’s value is changed via the `setopt` macro
    (see [setopt](#Setting-Variables)).

    If you specify this keyword, the variable’s documentation string
    should describe how to do the same job in hand-written Lisp code,
    either by invoking setfunction directly or by using
    `setopt` or `setopt-local`.

`:get getfunction` [¶](#index-get_002c-defcustom-keyword)
:   Specify getfunction as the way to extract the value of this
    option. The function getfunction should take one argument, a
    symbol, and should return whatever customize should use as the
    current value for that symbol (which need not be the symbol’s Lisp
    value). The default is `default-toplevel-value`.

    You have to really understand the workings of Custom to use
    `:get` correctly. It is meant for values that are treated in
    Custom as variables but are not actually stored in Lisp variables. It
    is almost surely a mistake to specify getfunction for a value
    that really is stored in a Lisp variable.

`:initialize function` [¶](#index-initialize_002c-defcustom-keyword)
:   function should be a function used to initialize the variable
    when the `defcustom` is evaluated. It should take two arguments,
    the option name (a symbol) and the value. Here are some predefined
    functions meant for use in this way:

    `custom-initialize-set`
    :   Use the variable’s `:set` function to initialize the variable, but
        do not reinitialize it if it is already non-void.

    `custom-initialize-default`
    :   Like `custom-initialize-set`, but use the function
        `set-default-toplevel-value` to set the variable, instead of the
        variable’s `:set` function. This is the usual choice for a
        variable whose `:set` function enables or disables a minor mode;
        with this choice, defining the variable will not call the minor mode
        function, but customizing the variable will do so.

    `custom-initialize-reset`
    :   Always use the `:set` function to initialize the variable. If
        the variable is already non-void, reset it by calling the `:set`
        function using the current value (returned by the `:get` method).
        This is the default `:initialize` function.

    `custom-initialize-changed`
    :   Use the `:set` function to initialize the variable, if it is
        already set or has been customized; otherwise, just use
        `set-default-toplevel-value`.

    `custom-initialize-after-file-load`
    :   This function behaves like `custom-initialize-set`, but it
        delays the actual initialization until after the containing file is loaded.
        This can be useful to break the common dependency where the setter
        is (or uses) a function which needs to be defined after the variable,
        such as when a global minor mode has a non-`nil` `:init-value`.

    `custom-initialize-delay`
    :   This function behaves like `custom-initialize-set`, but it delays
        the actual initialization to the next Emacs start. This should be used
        in Lisp files that are preloaded (loaded when Emacs is built), when it
        is important to ensure that the initialization is performed in the
        run-time context (which could be on a different system or programming
        environment). See [custom-initialize-delay](#Building-Emacs).

`:local value` [¶](#index-local_002c-defcustom-keyword)
:   If the value is `t`, mark option as automatically
    buffer-local; if the value is `permanent`, also set options
    `permanent-local` property to `t`. Finally, if the value is
    `permanent-only`, set options `permanent-local` property
    to `t` without marking it as automatically buffer-local.
    See [Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal).

`:risky value` [¶](#index-risky_002c-defcustom-keyword)
:   Set the variable’s `risky-local-variable` property to
    value (see [File Local Variables](#File-Local-Variables)).

`:safe function` [¶](#index-safe_002c-defcustom-keyword)
:   Set the variable’s `safe-local-variable` property to
    function (see [File Local Variables](#File-Local-Variables)).

`:set-after variables` [¶](#index-set_002dafter_002c-defcustom-keyword)
:   When setting variables according to saved customizations, make sure to
    set the variables variables before this one; i.e., delay
    setting this variable until after those others have been handled. Use
    `:set-after` if setting this variable won’t work properly unless
    those other variables already have their intended values.

It is useful to specify the `:require` keyword for an option
that turns on a certain feature. This causes Emacs to load the
feature, if it is not already loaded, whenever the option is set.
See [Common Item Keywords](#Common-Keywords). Here is an example:

```
(defcustom frobnicate-automatically nil
  "Non-nil means automatically frobnicate all buffers."
  :type 'boolean
  :require 'frobnicate-mode
  :group 'frobnicate)
```

If a customization item has a type such as `hook` or
`alist`, which supports `:options`, you can add additional
values to the list from outside the `defcustom` declaration by
calling `custom-add-frequent-value`. For example, if you define a
function `my-lisp-mode-initialization` intended to be called from
`emacs-lisp-mode-hook`, you might want to add that to the list of
reasonable values for `emacs-lisp-mode-hook`, but not by editing
its definition. You can do it thus:

```
(custom-add-frequent-value 'emacs-lisp-mode-hook
   'my-lisp-mode-initialization)
```

Function: **custom-add-frequent-value** symbol value [¶](#index-custom_002dadd_002dfrequent_002dvalue)
:   For the customization option symbol, add value to the
    list of reasonable values.

    The precise effect of adding a value depends on the customization type
    of symbol.

    Since evaluating a `defcustom` form does not clear values added
    previously, Lisp programs can use this function to add values for user
    options not yet defined.

Internally, `defcustom` uses the symbol property
`standard-value` to record the expression for the standard value,
`saved-value` to record the value saved by the user with the
customization buffer, and `customized-value` to record the value
set by the user with the customization buffer, but not saved.
See [Symbol Properties](#Symbol-Properties). In addition, there’s `themed-value`,
which is used to record the value set by a theme (see [Custom Themes](#Custom-Themes)). These properties are lists, the car of which is an
expression that evaluates to the value.

Function: **custom-reevaluate-setting** symbol [¶](#index-custom_002dreevaluate_002dsetting)
:   This function re-evaluates the standard value of symbol, which
    should be a user option declared via `defcustom`. If the
    variable was customized, this function re-evaluates the saved value
    instead. Then it sets the user option to that value (using the
    option’s `:set` property if that is defined).

    This is useful for customizable options that are defined before their
    value could be computed correctly. For example, during startup Emacs
    calls this function for some user options that were defined in
    pre-loaded Emacs Lisp files, but whose initial values depend on
    information available only at run-time.

Function: **custom-variable-p** arg [¶](#index-custom_002dvariable_002dp)
:   This function returns non-`nil` if arg is a customizable
    variable. A customizable variable is either a variable that has a
    `standard-value` or `custom-autoload` property (usually
    meaning it was declared with `defcustom`), or an alias for
    another customizable variable.

---

Next: [Applying Customizations](#Applying-Customizations), Previous: [Defining Customization Variables](#Variable-Definitions), Up: [Customization Settings](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.4 Customization Types [¶](#Customization-Types-1)

When you define a user option with `defcustom`, you must specify
its *customization type*. That is a Lisp object which describes (1)
which values are legitimate and (2) how to display the value in the
customization buffer for editing.

You specify the customization type in `defcustom` with the
`:type` keyword. The argument of `:type` is evaluated, but
only once when the `defcustom` is executed, so it isn’t useful
for the value to vary. Normally we use a quoted constant. For
example:

```
(defcustom diff-command "diff"
  "The command to use to run diff."
  :type '(string)
  :group 'diff)
```

In general, a customization type is a list whose first element is a
symbol, one of the customization type names defined in the following
sections. After this symbol come a number of arguments, depending on
the symbol. Between the type symbol and its arguments, you can
optionally write keyword-value pairs (see [Type Keywords](#Type-Keywords)).

Some type symbols do not use any arguments; those are called
*simple types*. For a simple type, if you do not use any
keyword-value pairs, you can omit the parentheses around the type
symbol. For example just `string` as a customization type is
equivalent to `(string)`.

All customization types are implemented as widgets; see [Introduction](https://www.gnu.org/software/emacs/manual/html_mono/widget.html#Top) in The Emacs Widget Library, for details.

* [Simple Types](#Simple-Types)
* [Composite Types](#Composite-Types)
* [Splicing into Lists](#Splicing-into-Lists)
* [Type Keywords](#Type-Keywords)
* [Defining New Types](#Defining-New-Types)

---

Next: [Composite Types](#Composite-Types), Up: [Customization Types](#Customization-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 15.4.1 Simple Types [¶](#Simple-Types-1)

This section describes all the simple customization types. For
several of these customization types, the customization widget
provides inline completion with `C-M-i` or `M-TAB`.

`sexp`
:   The value may be any Lisp object that can be printed and read back.
    You can use `sexp` as a fall-back for any option, if you don’t
    want to take the time to work out a more specific type to use.

`integer`
:   The value must be an integer.

`natnum`
:   The value must be a nonnegative integer.

`number`
:   The value must be a number (floating point or integer).

`float`
:   The value must be floating point.

`string`
:   The value must be a string. The customization buffer shows the string
    without delimiting ‘`"`’ characters or ‘`\`’ quotes.

`regexp`
:   Like `string` except that the string must be a valid regular
    expression.

`character`
:   The value must be a character code. A character code is actually an
    integer, but this type shows the value by inserting the character in the
    buffer, rather than by showing the number.

`file`
:   The value must be a file name. The widget provides completion.

`(file :must-match t)`
:   The value must be a file name for an existing file. The widget
    provides completion.

`directory`
:   The value must be a directory. The widget provides completion.

`hook`
:   The value must be a list of functions. This customization type is
    used for hook variables. You can use the `:options` keyword in a
    hook variable’s `defcustom` to specify a list of functions
    recommended for use in the hook; See [Defining Customization Variables](#Variable-Definitions).

`symbol`
:   The value must be a symbol. It appears in the customization buffer as
    the symbol name. The widget provides completion.

`function`
:   The value must be either a lambda expression or a function name. The
    widget provides completion for function names.

`buffer-predicate`
:   The value must be a form suitable for the condition argument of
    the `buffer-match-p` function (see [The Buffer List](#Buffer-List)). This is handy
    for options whose value is submitted to `buffer-match-p` to select
    suitable buffers.

`variable`
:   The value must be a variable name. The widget provides completion.

`face`
:   The value must be a symbol which is a face name. The widget provides
    completion.

`boolean`
:   The value is boolean—either `nil` or `t`. Note that by
    using `choice` and `const` together (see the next section),
    you can specify that the value must be `nil` or `t`, but also
    specify the text to describe each value in a way that fits the specific
    meaning of the alternative.

`key`
:   The value is a valid key according to `key-valid-p`, and suitable
    for use with, for example `keymap-set`.

`key-sequence`
:   The value is a key sequence. The customization buffer shows the key
    sequence using the same syntax as the `kbd` function. See [Key Sequences](#Key-Sequences). This is a legacy type; use `key` instead.

`coding-system`
:   The value must be a coding-system name, and you can do completion with
    `M-TAB`.

`color`
:   The value must be a valid color name. The widget provides completion
    for color names, as well as a sample and a button for selecting a
    color name from a list of color names shown in a `*Colors*`
    buffer.

`fringe-bitmap`
:   The value must be a valid fringe bitmap name. The widget provides
    completion.

---

Next: [Splicing into Lists](#Splicing-into-Lists), Previous: [Simple Types](#Simple-Types), Up: [Customization Types](#Customization-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 15.4.2 Composite Types [¶](#Composite-Types-1)

When none of the simple types is appropriate, you can use composite
types, which build new types from other types or from specified data.
The specified types or data are called the *arguments* of the
composite type. The composite type normally looks like this:

```
(constructor arguments...)
```

but you can also add keyword-value pairs before the arguments, like
this:

```
(constructor {keyword value}... arguments...)
```

Here is a table of constructors and how to use them to write
composite types:

`(cons car-type cdr-type)`
:   The value must be a cons cell, its CAR must fit car-type, and
    its CDR must fit cdr-type. For example, `(cons string
    symbol)` is a customization type which matches values such as
    `("foo" . foo)`.

    In the customization buffer, the CAR and CDR are displayed
    and edited separately, each according to their specified type.

`(list element-types…)`
:   The value must be a list with exactly as many elements as the
    element-types given; and each element must fit the
    corresponding element-type.

    For example, `(list integer string function)` describes a list of
    three elements; the first element must be an integer, the second a
    string, and the third a function.

    In the customization buffer, each element is displayed and edited
    separately, according to the type specified for it.

`(group element-types…)`
:   This works like `list` except for the formatting
    of text in the Custom buffer. `list` labels each
    element value with its tag; `group` does not.

`(vector element-types…)`
:   Like `list` except that the value must be a vector instead of a
    list. The elements work the same as in `list`.

`(alist :key-type key-type :value-type value-type)`
:   The value must be a list of cons-cells, the CAR of each cell
    representing a key of customization type key-type, and the
    CDR of the same cell representing a value of customization type
    value-type. The user can add and delete key/value pairs, and
    edit both the key and the value of each pair.

    If omitted, key-type and value-type default to
    `sexp`.

    The user can add any key matching the specified key type, but you can
    give some keys a preferential treatment by specifying them with the
    `:options` (see [Defining Customization Variables](#Variable-Definitions)). The specified keys
    will always be shown in the customize buffer (together with a suitable
    value), with a checkbox to include or exclude or disable the key/value
    pair from the alist. The user will not be able to edit the keys
    specified by the `:options` keyword argument.

    The argument to the `:options` keywords should be a list of
    specifications for reasonable keys in the alist. Ordinarily, they are
    simply atoms, which stand for themselves. For example:

    ```
    :options '("foo" "bar" "baz")
    ```

    specifies that there are three known keys, namely `"foo"`,
    `"bar"` and `"baz"`, which will always be shown first.

    You may want to restrict the value type for specific keys, for
    example, the value associated with the `"bar"` key can only be an
    integer. You can specify this by using a list instead of an atom in
    the list. The first element will specify the key, like before, while
    the second element will specify the value type. For example:

    ```
    :options '("foo" ("bar" integer) "baz")
    ```

    Finally, you may want to change how the key is presented. By default,
    the key is simply shown as a `const`, since the user cannot change
    the special keys specified with the `:options` keyword. However,
    you may want to use a more specialized type for presenting the key, like
    `function-item` if you know it is a symbol with a function binding.
    This is done by using a customization type specification instead of a
    symbol for the key.

    ```
    :options '("foo"
               ((function-item some-function) integer)
               "baz")
    ```

    Many alists use lists with two elements, instead of cons cells. For
    example,

    ```
    (defcustom list-alist
      '(("foo" 1) ("bar" 2) ("baz" 3))
      "Each element is a list of the form (KEY VALUE).")
    ```

    instead of

    ```
    (defcustom cons-alist
      '(("foo" . 1) ("bar" . 2) ("baz" . 3))
      "Each element is a cons-cell (KEY . VALUE).")
    ```

    Because of the way lists are implemented on top of cons cells, you can
    treat `list-alist` in the example above as a cons cell alist, where
    the value type is a list with a single element containing the real
    value.

    ```
    (defcustom list-alist '(("foo" 1) ("bar" 2) ("baz" 3))
      "Each element is a list of the form (KEY VALUE)."
      :type '(alist :value-type (group integer)))
    ```

    The `group` widget is used here instead of `list` only because
    the formatting is better suited for the purpose.

    Similarly, you can have alists with more values associated with each
    key, using variations of this trick:

    ```
    (defcustom person-data '(("brian"  50 t)
                             ("dorith" 55 nil)
                             ("ken"    52 t))
      "Alist of basic info about people.
    Each element has the form (NAME AGE MALE-FLAG)."
      :type '(alist :value-type (group integer boolean)))
    ```

`(plist :key-type key-type :value-type value-type)`
:   This customization type is similar to `alist` (see above), except
    that (i) the information is stored as a property list,
    (see [Property Lists](#Property-Lists)), and (ii) key-type, if omitted,
    defaults to `symbol` rather than `sexp`.

`(choice alternative-types…)`
:   The value must fit one of alternative-types. For example,
    `(choice integer string)` allows either an integer or a string.

    In the customization buffer, the user selects an alternative
    using a menu, and can then edit the value in the usual way for that
    alternative.

    Normally the strings in this menu are determined automatically from the
    choices; however, you can specify different strings for the menu by
    including the `:tag` keyword in the alternatives. For example, if
    an integer stands for a number of spaces, while a string is text to use
    verbatim, you might write the customization type this way,

    ```
    (choice (integer :tag "Number of spaces")
            (string :tag "Literal text"))
    ```

    so that the menu offers ‘`Number of spaces`’ and ‘`Literal text`’.

    In any alternative for which `nil` is not a valid value, other than
    a `const`, you should specify a valid default for that alternative
    using the `:value` keyword. See [Type Keywords](#Type-Keywords).

    If some values are covered by more than one of the alternatives,
    customize will choose the first alternative that the value fits. This
    means you should always list the most specific types first, and the
    most general last. Here’s an example of proper usage:

    ```
    (choice (const :tag "Off" nil)
            symbol (sexp :tag "Other"))
    ```

    This way, the special value `nil` is not treated like other
    symbols, and symbols are not treated like other Lisp expressions.

`(radio element-types…)` [¶](#index-radio_002c-customization-types)
:   This is similar to `choice`, except that the choices are displayed
    using radio buttons rather than a menu. This has the advantage of
    displaying documentation for the choices when applicable and so is often
    a good choice for a choice between constant functions
    (`function-item` customization types).

`(const value)`
:   The value must be value—nothing else is allowed.

    The main use of `const` is inside of `choice`. For example,
    `(choice integer (const nil))` allows either an integer or
    `nil`.

    `:tag` is often used with `const`, inside of `choice`.
    For example,

    ```
    (choice (const :tag "Yes" t)
            (const :tag "No" nil)
            (const :tag "Ask" foo))
    ```

    describes a variable for which `t` means yes, `nil` means no,
    and `foo` means “ask”.

`(other value)`
:   This alternative can match any Lisp value, but if the user chooses this
    alternative, that selects the value value.

    The main use of `other` is as the last element of `choice`.
    For example,

    ```
    (choice (const :tag "Yes" t)
            (const :tag "No" nil)
            (other :tag "Ask" foo))
    ```

    describes a variable for which `t` means yes, `nil` means no,
    and anything else means “ask”. If the user chooses ‘`Ask`’ from
    the menu of alternatives, that specifies the value `foo`; but any
    other value (not `t`, `nil` or `foo`) displays as
    ‘`Ask`’, just like `foo`.

`(function-item function)`
:   Like `const`, but used for values which are functions. This
    displays the documentation string as well as the function name.
    The documentation string is either the one you specify with
    `:doc`, or function’s own documentation string.

`(variable-item variable)`
:   Like `const`, but used for values which are variable names. This
    displays the documentation string as well as the variable name. The
    documentation string is either the one you specify with `:doc`, or
    variable’s own documentation string.

`(set types…)`
:   The value must be a list, and each element of the list must match one of
    the types specified.

    This appears in the customization buffer as a checklist, so that each of
    types may have either one corresponding element or none. It is
    not possible to specify two different elements that match the same one
    of types. For example, `(set integer symbol)` allows one
    integer and/or one symbol in the list; it does not allow multiple
    integers or multiple symbols. As a result, it is rare to use
    nonspecific types such as `integer` in a `set`.

    Most often, the types in a `set` are `const` types, as
    shown here:

    ```
    (set (const :bold) (const :italic))
    ```

    Sometimes they describe possible elements in an alist:

    ```
    (set (cons :tag "Height" (const height) integer)
         (cons :tag "Width" (const width) integer))
    ```

    That lets the user specify a height value optionally
    and a width value optionally.

`(repeat element-type)`
:   The value must be a list and each element of the list must fit the type
    element-type. This appears in the customization buffer as a
    list of elements, with ‘`[INS]`’ and ‘`[DEL]`’ buttons for adding
    more elements or removing elements.

`(restricted-sexp :match-alternatives criteria)` [¶](#index-restricted_002dsexp_002c-customization-types)
:   This is the most general composite type construct. The value may be
    any Lisp object that satisfies one of criteria. criteria
    should be a list, and each element should be one of these
    possibilities:

    * A predicate—that is, a function of one argument that returns either
      `nil` or non-`nil` according to the argument. Using a
      predicate in the list says that objects for which the predicate
      returns non-`nil` are acceptable.
    * A quoted constant—that is, `'object`. This sort of element
      in the list says that object itself is an acceptable value.

    For example,

    ```
    (restricted-sexp :match-alternatives
                     (integerp 't 'nil))
    ```

    allows integers, `t` and `nil` as legitimate values.

    The customization buffer shows all legitimate values using their read
    syntax, and the user edits them textually.

Here is a table of the keywords you can use in keyword-value pairs
in a composite type:

`:tag tag`
:   Use tag as the name of this alternative, for user communication
    purposes. This is useful for a type that appears inside of a
    `choice`.

`:match-alternatives criteria` [¶](#index-match_002dalternatives_002c-customization-keyword)
:   Use criteria to match possible values. This is used only in
    `restricted-sexp`.

`:args argument-list` [¶](#index-args_002c-customization-keyword)
:   Use the elements of argument-list as the arguments of the type
    construct. For instance, `(const :args (foo))` is equivalent to
    `(const foo)`. You rarely need to write `:args` explicitly,
    because normally the arguments are recognized automatically as
    whatever follows the last keyword-value pair.

---

Next: [Type Keywords](#Type-Keywords), Previous: [Composite Types](#Composite-Types), Up: [Customization Types](#Customization-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 15.4.3 Splicing into Lists [¶](#Splicing-into-Lists-1)

The `:inline` feature lets you splice a variable number of
elements into the middle of a `list` or `vector`
customization type. You use it by adding `:inline t` to a type
specification which is contained in a `list` or `vector`
specification.

Normally, each entry in a `list` or `vector` type
specification describes a single element type. But when an entry
contains `:inline t`, the value it matches is merged directly
into the containing sequence. For example, if the entry matches a
list with three elements, those become three elements of the overall
sequence. This is analogous to ‘`,@`’ in a backquote construct
(see [Backquote](#Backquote)).

For example, to specify a list whose first element must be `baz`
and whose remaining arguments should be zero or more of `foo` and
`bar`, use this customization type:

```
(list (const baz) (set :inline t (const foo) (const bar)))
```

This matches values such as `(baz)`, `(baz foo)`, `(baz bar)`
and `(baz foo bar)`.

When the element-type is a `choice`, you use `:inline` not
in the `choice` itself, but in (some of) the alternatives of the
`choice`. For example, to match a list which must start with a
file name, followed either by the symbol `t` or two strings, use
this customization type:

```
(list file
      (choice (const t)
              (list :inline t string string)))
```

If the user chooses the first alternative in the choice, then the
overall list has two elements and the second element is `t`. If
the user chooses the second alternative, then the overall list has three
elements and the second and third must be strings.

The widgets can specify predicates to say whether an inline value
matches the widget with the `:match-inline` element.

---

Next: [Defining New Types](#Defining-New-Types), Previous: [Splicing into Lists](#Splicing-into-Lists), Up: [Customization Types](#Customization-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 15.4.4 Type Keywords [¶](#Type-Keywords-1)

You can specify keyword-argument pairs in a customization type after the
type name symbol. Here are the keywords you can use, and their
meanings:

`:value default`
:   Provide a default value.

    If `nil` is not a valid value for the alternative, then it is
    essential to specify a valid default with `:value`.

    If you use this for a type that appears as an alternative inside of
    `choice`; it specifies the default value to use, at first, if and
    when the user selects this alternative with the menu in the
    customization buffer.

    Of course, if the actual value of the option fits this alternative, it
    will appear showing the actual value, not default.

`:format format-string` [¶](#index-format_002c-customization-keyword)
:   This string will be inserted in the buffer to represent the value
    corresponding to the type. The following ‘`%`’ escapes are available
    for use in format-string:

    ‘`%[button%]`’
    :   Display the text button marked as a button. The `:action`
        attribute specifies what the button will do if the user invokes it;
        its value is a function which takes two arguments—the widget which
        the button appears in, and the event.

        There is no way to specify two different buttons with different
        actions.

    ‘`%{sample%}`’
    :   Show sample in a special face specified by `:sample-face`.

    ‘`%v`’
    :   Substitute the item’s value. How the value is represented depends on
        the kind of item, and (for variables) on the customization type.

    ‘`%d`’
    :   Substitute the item’s documentation string.

    ‘`%h`’
    :   Like ‘`%d`’, but if the documentation string is more than one line,
        add a button to control whether to show all of it or just the first line.

    ‘`%t`’
    :   Substitute the tag here. You specify the tag with the `:tag`
        keyword.

    ‘`%%`’
    :   Display a literal ‘`%`’.

`:action action` [¶](#index-action_002c-customization-keyword)
:   Perform action if the user clicks on a button.

`:button-face face` [¶](#index-button_002dface_002c-customization-keyword)
:   Use the face face (a face name or a list of face names) for button
    text displayed with ‘`%[…%]`’.

`:button-prefix prefix` [¶](#index-button_002dprefix_002c-customization-keyword)

`:button-suffix suffix`
:   These specify the text to display before and after a button.
    Each can be:

    `nil`
    :   No text is inserted.

    a string
    :   The string is inserted literally.

    a symbol
    :   The symbol’s value is used.

`:tag tag`
:   Use tag (a string) as the tag for the value (or part of the value)
    that corresponds to this type. It’s important to provide an informative
    tag for the customization interface, especially if you’re using the
    `restricted-sexp` type or if you’re defining a new type.
    See [Defining New Types](#Defining-New-Types).

`:doc doc` [¶](#index-doc_002c-customization-keyword)
:   Use doc as the documentation string for this value (or part of the
    value) that corresponds to this type. In order for this to work, you
    must specify a value for `:format`, and use ‘`%d`’ or ‘`%h`’
    in that value.

    The usual reason to specify a documentation string for a type is to
    provide more information about the meanings of alternatives inside a
    `choice` type or the parts of some other composite type.

`:help-echo motion-doc` [¶](#index-help_002decho_002c-customization-keyword)
:   When you move to this item with `widget-forward` or
    `widget-backward`, it will display the string motion-doc in
    the echo area. In addition, motion-doc is used as the mouse
    `help-echo` string and may actually be a function or form evaluated
    to yield a help string. If it is a function, it is called with one
    argument, the widget.

`:match function` [¶](#index-match_002c-customization-keyword)
:   Specify how to decide whether a value matches the type. The
    corresponding value, function, should be a function that accepts
    two arguments, a widget and a value; it should return non-`nil` if
    the value is acceptable.

`:match-inline function` [¶](#index-match_002dinline_002c-customization-keyword)
:   Specify how to decide whether an inline value matches the type. The
    corresponding value, function, should be a function that accepts
    two arguments, a widget and an inline value; it should return
    non-`nil` if the value is acceptable. See [Splicing into Lists](#Splicing-into-Lists), for more information about inline values.

`:validate function`
:   Specify a validation function for input. function takes a
    widget as an argument, and should return `nil` if the widget’s
    current value is valid for the widget. Otherwise, it should return
    the widget containing the invalid data, and set that widget’s
    `:error` property to a string explaining the error.

`:type-error string` [¶](#index-type_002derror_002c-customization-keyword)
:   string should be a string that describes why a value doesn’t
    match the type, as determined by the `:match` function. When the
    `:match` function returns `nil`, the widget’s `:error`
    property will be set to string.

---

Previous: [Type Keywords](#Type-Keywords), Up: [Customization Types](#Customization-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 15.4.5 Defining New Types [¶](#Defining-New-Types-1)

In the previous sections we have described how to construct elaborate
type specifications for `defcustom`. In some cases you may want
to give such a type specification a name. The obvious case is when
you are using the same type for many user options: rather than repeat
the specification for each option, you can give the type specification
a name, and use that name each `defcustom`. The other case is
when a user option’s value is a recursive data structure. To make it
possible for a datatype to refer to itself, it needs to have a name.

Since custom types are implemented as widgets, the way to define a new
customize type is to define a new widget. We are not going to describe
the widget interface here in details, see [Introduction](https://www.gnu.org/software/emacs/manual/html_mono/widget.html#Top) in The Emacs Widget Library, for that. Instead we are going to
demonstrate the minimal functionality needed for defining new customize
types by a simple example.

```
(define-widget 'binary-tree-of-string 'lazy
  "A binary tree made of cons-cells and strings."
  :offset 4
  :tag "Node"
  :type '(choice (string :tag "Leaf" :value "")
                 (cons :tag "Interior"
                       :value ("" . "")
                       binary-tree-of-string
                       binary-tree-of-string)))

(defcustom foo-bar ""
  "Sample variable holding a binary tree of strings."
  :type 'binary-tree-of-string)
```

The function to define a new widget is called `define-widget`. The
first argument is the symbol we want to make a new widget type. The
second argument is a symbol representing an existing widget, the new
widget is going to be defined in terms of difference from the existing
widget. For the purpose of defining new customization types, the
`lazy` widget is perfect, because it accepts a `:type` keyword
argument with the same syntax as the keyword argument to
`defcustom` with the same name. The third argument is a
documentation string for the new widget. You will be able to see that
string with the `M-x widget-browse RET binary-tree-of-string
RET` command.

After these mandatory arguments follow the keyword arguments. The most
important is `:type`, which describes the data type we want to match
with this widget. Here a `binary-tree-of-string` is described as
being either a string, or a cons-cell whose car and cdr are themselves
both `binary-tree-of-string`. Note the reference to the widget
type we are currently in the process of defining. The `:tag`
is another important keyword argument because we are using the
`lazy` widget for our new widget. By default, the `lazy`
widget doesn’t have a tag, and in its absence the customization buffer
will show the entire widget’s value (that is, the value of the user
option being customized). Since that’s almost never a good idea, we
provide a string to name the `binary-tree-or-string` widget. The
`:offset` argument is there to ensure that child nodes are indented
four spaces relative to the parent node, making the tree structure
apparent in the customization buffer.

The `defcustom` shows how the new widget can be used as an ordinary
customization type.

The reason for the name `lazy` is that the other composite
widgets convert their inferior widgets to internal form when the
widget is instantiated in a buffer. This conversion is recursive, so
the inferior widgets will convert *their* inferior widgets. If
the data structure is itself recursive, this conversion is an infinite
recursion. The `lazy` widget prevents the recursion: it convert
its `:type` argument only when needed.

---

Next: [Custom Themes](#Custom-Themes), Previous: [Customization Types](#Customization-Types), Up: [Customization Settings](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.5 Applying Customizations [¶](#Applying-Customizations-1)

The following functions are responsible for installing the user’s
customization settings for variables and faces, respectively. When
the user invokes ‘`Save for future sessions`’ in the Customize
interface, that takes effect by writing a `custom-set-variables`
and/or a `custom-set-faces` form into the custom file, to be
evaluated the next time Emacs starts.

Function: **custom-set-variables** &rest args [¶](#index-custom_002dset_002dvariables)
:   This function installs the variable customizations specified by
    args. Each argument in args should have the form

    ```
    (var expression [now [request [comment]]])
    ```

    var is a variable name (a symbol), and expression is an
    expression which evaluates to the desired customized value.

    If the `defcustom` form for var has been evaluated prior to
    this `custom-set-variables` call, expression is immediately
    evaluated, and the variable’s value is set to the result. Otherwise,
    expression is stored into the variable’s `saved-value`
    property, to be evaluated when the relevant `defcustom` is called
    (usually when the library defining that variable is loaded into
    Emacs).

    The now, request, and comment entries are for
    internal use only, and may be omitted. now, if non-`nil`,
    means to set the variable’s value now, even if the variable’s
    `defcustom` form has not been evaluated. request is a list
    of features to be loaded immediately (see [Features](#Named-Features)).
    comment is a string describing the customization.

Function: **custom-set-faces** &rest args [¶](#index-custom_002dset_002dfaces)
:   This function installs the face customizations specified by
    args. Each argument in args should have the form

    ```
    (face spec [now [comment]])
    ```

    face is a face name (a symbol), and spec is the customized
    face specification for that face (see [Defining Faces](#Defining-Faces)).

    The now and comment entries are for internal use only, and
    may be omitted. now, if non-`nil`, means to install the
    face specification now, even if the `defface` form has not been
    evaluated. comment is a string describing the customization.

---

Previous: [Applying Customizations](#Applying-Customizations), Up: [Customization Settings](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
