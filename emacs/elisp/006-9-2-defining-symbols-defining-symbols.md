### 9.2 Defining Symbols [¶](#Defining-Symbols)

A *definition* is a special kind of Lisp expression that
announces your intention to use a symbol in a particular way. It
typically specifies a value or meaning for the symbol for one kind of
use, plus documentation for its meaning when used in this way. Thus,
when you define a symbol as a variable, you can supply an initial
value for the variable, plus documentation for the variable.

`defvar` and `defconst` are special forms that define a
symbol as a *global variable*—a variable that can be accessed at
any point in a Lisp program. See [Variables](#Variables), for details about
variables. To define a customizable variable, use the
`defcustom` macro, which also calls `defvar` as a subroutine
(see [Customization Settings](#Customization)).

In principle, you can assign a variable value to any symbol with
`setq`, whether or not it has first been defined as a variable.
However, you ought to write a variable definition for each global
variable that you want to use; otherwise, your Lisp program may not
act correctly if it is evaluated with lexical scoping enabled
(see [Scoping Rules for Variable Bindings](#Variable-Scoping)).

`defun` defines a symbol as a function, creating a lambda
expression and storing it in the function cell of the symbol. This
lambda expression thus becomes the function definition of the symbol.
(The term “function definition”, meaning the contents of the function
cell, is derived from the idea that `defun` gives the symbol its
definition as a function.) `defsubst` and `defalias` are two
other ways of defining a function. See [Functions](#Functions).

`defmacro` defines a symbol as a macro. It creates a macro
object and stores it in the function cell of the symbol. Note that a
given symbol can be a macro or a function, but not both at once, because
both macro and function definitions are kept in the function cell, and
that cell can hold only one Lisp object at any given time.
See [Macros](#Macros).

As previously noted, Emacs Lisp allows the same symbol to be defined
both as a variable (e.g., with `defvar`) and as a function or
macro (e.g., with `defun`). Such definitions do not conflict.

These definitions also act as guides for programming tools. For
example, the `C-h f` and `C-h v` commands create help buffers
containing links to the relevant variable, function, or macro
definitions. See [Name Help](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Name-Help) in The GNU Emacs Manual.

---

Next: [Symbol Properties](#Symbol-Properties), Previous: [Defining Symbols](#Definitions), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.3 Creating and Interning Symbols [¶](#Creating-and-Interning-Symbols)

To understand how symbols are created in GNU Emacs Lisp, you must
know how Lisp reads them. Lisp must ensure that it finds the same
symbol every time it reads the same sequence of characters in the same
context. Failure to do so would cause complete confusion.

When the Lisp reader encounters a name that references a symbol in
the source code, it looks up that name in a table called an *obarray*
to find the symbol that the programmer meant. An obarray is an unordered
container of symbols, indexed by name.

The Lisp reader also considers “shorthands”.
If the programmer supplied them, this allows the reader to find a
symbol even if its name isn’t present in its full form in the source
code. See [Shorthands](#Shorthands).

If a symbol with the desired name is found, the reader uses that
symbol. If the obarray does not contain a symbol with that name, the
reader makes a new symbol and adds it to the obarray. Finding or adding
a symbol with a certain name is called *interning* it, and the
symbol is then called an *interned symbol*.

Interning ensures that each obarray has just one symbol with any
particular name. Other like-named symbols may exist, but not in the
same obarray. Thus, the reader gets the same symbols for the same
names, as long as you keep reading with the same obarray.

Interning usually happens automatically in the reader, but sometimes
other programs may want to do it. For example, after the `M-x`
command obtains the command name as a string using the minibuffer, it
then interns the string, to get the interned symbol with that name.
As another example, a hypothetical telephone book program could intern
the name of each looked up person’s name as a symbol, even if the
obarray did not contain it, so that it could attach information to
that new symbol, such as the last time someone looked it up.

No obarray contains all symbols; in fact, some symbols are not in any
obarray. They are called *uninterned symbols*. An uninterned
symbol has the same four cells as other symbols; however, the only way
to gain access to it is by finding it in some other object or as the
value of a variable. Uninterned symbols are sometimes useful in
generating Lisp code, see below.

> **Common Lisp note:** Unlike Common Lisp, Emacs Lisp does not provide
> for interning the same name in several different “packages”, thus
> creating multiple symbols with the same name but different packages.
> Emacs Lisp provides a different namespacing system called
> “shorthands” (see [Shorthands](#Shorthands)).

Function: **obarray-make** &optional size [¶](#index-obarray_002dmake)
:   This function creates and returns a new obarray.
    The optional size may be used to specify the number of symbols
    that it is expected to hold, but since obarrays grow automatically
    as needed, this rarely provides any benefit.

Function: **obarrayp** object [¶](#index-obarrayp)
:   This function returns `t` if object is an obarray,
    `nil` otherwise.

Most of the functions below take a name and sometimes an obarray as
arguments. A `wrong-type-argument` error is signaled if the name
is not a string, or if the obarray is not an obarray object.

Function: **symbol-name** symbol [¶](#index-symbol_002dname)
:   This function returns the string that is symbol’s name. For example:

    ```
    (symbol-name 'foo)
         ⇒ "foo"
    ```

    **Warning:** Never alter the string returned by that function.
    Doing that might make Emacs dysfunctional, and might even crash Emacs.

Creating an uninterned symbol is useful in generating Lisp code,
because an uninterned symbol used as a variable in the code you
generate cannot clash with any variables used in other Lisp programs.

Function: **make-symbol** name [¶](#index-make_002dsymbol)
:   This function returns a newly-allocated, uninterned symbol whose name is
    name (which must be a string). Its value and function definition
    are void, and its property list is `nil`. In the example below,
    the value of `sym` is not `eq` to `foo` because it is a
    distinct uninterned symbol whose name is also ‘`foo`’.

    ```
    (setq sym (make-symbol "foo"))
         ⇒ foo
    (eq sym 'foo)
         ⇒ nil
    ```

Function: **gensym** &optional prefix [¶](#index-gensym)
:   This function returns a symbol using `make-symbol`, whose name is
    made by appending `gensym-counter` to prefix and incrementing
    that counter, guaranteeing that no two calls to this function will
    generate a symbol with the same name. The prefix defaults to
    `"g"`.

To avoid problems when accidentally interning printed representation
of generated code (see [Printed Representation and Read Syntax](#Printed-Representation)), it is recommended
to use `gensym` instead of `make-symbol`.

Function: **intern** name &optional obarray [¶](#index-intern)
:   This function returns the interned symbol whose name is name. If
    there is no such symbol in the obarray obarray, `intern`
    creates a new one, adds it to the obarray, and returns it. If
    obarray is omitted, the value of the global variable
    `obarray` is used.

    ```
    (setq sym (intern "foo"))
         ⇒ foo
    (eq sym 'foo)
         ⇒ t

    (setq sym1 (intern "foo" other-obarray))
         ⇒ foo
    (eq sym1 'foo)
         ⇒ nil
    ```

> **Common Lisp note:** In Common Lisp, you can intern an existing symbol
> in an obarray. In Emacs Lisp, you cannot do this, because the argument
> to `intern` must be a string, not a symbol.

Function: **intern-soft** name &optional obarray [¶](#index-intern_002dsoft)
:   This function returns the symbol in obarray whose name is
    name, or `nil` if obarray has no symbol with that name.
    Therefore, you can use `intern-soft` to test whether a symbol with
    a given name is already interned. If obarray is omitted, the
    value of the global variable `obarray` is used.

    The argument name may also be a symbol; in that case,
    the function returns name if name is interned
    in the specified obarray, and otherwise `nil`.

    ```
    (intern-soft "frazzle")        ; No such symbol exists.
         ⇒ nil
    (make-symbol "frazzle")        ; Create an uninterned one.
         ⇒ frazzle
    ```

    ```
    (intern-soft "frazzle")        ; That one cannot be found.
         ⇒ nil
    ```

    ```
    (setq sym (intern "frazzle"))  ; Create an interned one.
         ⇒ frazzle
    ```

    ```
    (intern-soft "frazzle")        ; That one can be found!
         ⇒ frazzle
    ```

    ```
    (eq sym 'frazzle)              ; And it is the same one.
         ⇒ t
    ```

Variable: **obarray** [¶](#index-obarray-1)
:   This variable is the standard obarray for use by `intern` and
    `read`.

Function: **mapatoms** function &optional obarray [¶](#index-mapatoms)
:   This function calls function once with each symbol in the obarray
    obarray. Then it returns `nil`. If obarray is
    omitted, it defaults to the value of `obarray`, the standard
    obarray for ordinary symbols.

    ```
    (setq count 0)
         ⇒ 0
    (defun count-syms (s)
      (setq count (1+ count)))
         ⇒ count-syms
    (mapatoms 'count-syms)
         ⇒ nil
    count
         ⇒ 1871
    ```

    See `documentation` in [Access to Documentation Strings](#Accessing-Documentation), for another
    example using `mapatoms`.

Function: **unintern** symbol obarray [¶](#index-unintern)
:   This function deletes symbol from the obarray obarray. If
    `symbol` is not actually in the obarray, `unintern` does
    nothing. If obarray is `nil`, the current obarray is used.

    If you provide a string instead of a symbol as symbol, it stands
    for a symbol name. Then `unintern` deletes the symbol (if any) in
    the obarray which has that name. If there is no such symbol,
    `unintern` does nothing.

    If `unintern` does delete a symbol, it returns `t`. Otherwise
    it returns `nil`.

Function: **obarray-clear** obarray [¶](#index-obarray_002dclear)
:   This function removes all symbols from obarray.

---

Next: [Shorthands](#Shorthands), Previous: [Creating and Interning Symbols](#Creating-Symbols), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.4 Symbol Properties [¶](#Symbol-Properties-1)

A symbol may possess any number of *symbol properties*, which
can be used to record miscellaneous information about the symbol. For
example, when a symbol has a `risky-local-variable` property with
a non-`nil` value, that means the variable which the symbol names
is a risky file-local variable (see [File Local Variables](#File-Local-Variables)).

Each symbol’s properties and property values are stored in the
symbol’s property list cell (see [Symbol Components](#Symbol-Components)), in the form
of a property list (see [Property Lists](#Property-Lists)).

* [Accessing Symbol Properties](#Symbol-Plists)
* [Standard Symbol Properties](#Standard-Properties)

---

Next: [Standard Symbol Properties](#Standard-Properties), Up: [Symbol Properties](#Symbol-Properties)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.1 Accessing Symbol Properties [¶](#Accessing-Symbol-Properties)

The following functions can be used to access symbol properties.

Function: **get** symbol property [¶](#index-get)
:   This function returns the value of the property named property
    in symbol’s property list. If there is no such property, it
    returns `nil`. Thus, there is no distinction between a value of
    `nil` and the absence of the property.

    The name property is compared with the existing property names
    using `eq`, so any object is a legitimate property.

    See `put` for an example.

Function: **put** symbol property value [¶](#index-put)
:   This function puts value onto symbol’s property list under
    the property name property, replacing any previous property value.
    The `put` function returns value.

    ```
    (put 'fly 'verb 'transitive)
         ⇒'transitive
    (put 'fly 'noun '(a buzzing little bug))
         ⇒ (a buzzing little bug)
    (get 'fly 'verb)
         ⇒ transitive
    (symbol-plist 'fly)
         ⇒ (verb transitive noun (a buzzing little bug))
    ```

Function: **symbol-plist** symbol [¶](#index-symbol_002dplist)
:   This function returns the property list of symbol.

Function: **setplist** symbol plist [¶](#index-setplist)
:   This function sets symbol’s property list to plist.
    Normally, plist should be a well-formed property list, but this is
    not enforced. The return value is plist.

    ```
    (setplist 'foo '(a 1 b (2 3) c nil))
         ⇒ (a 1 b (2 3) c nil)
    (symbol-plist 'foo)
         ⇒ (a 1 b (2 3) c nil)
    ```

    For symbols in special obarrays, which are not used for ordinary
    purposes, it may make sense to use the property list cell in a
    nonstandard fashion; in fact, the abbrev mechanism does so
    (see [Abbrevs and Abbrev Expansion](#Abbrevs)).

    You could define `put` in terms of `setplist` and
    `plist-put`, as follows:

    ```
    (defun put (symbol prop value)
      (setplist symbol
                (plist-put (symbol-plist symbol) prop value)))
    ```

Function: **function-get** symbol property &optional autoload [¶](#index-function_002dget)
:   This function is identical to `get`, except that if symbol
    is the name of a function alias, it looks in the property list of the
    symbol naming the actual function. See [Defining Functions](#Defining-Functions). If the
    optional argument autoload is non-`nil`, and symbol
    is auto-loaded, this function will try to autoload it, since
    autoloading might set property of symbol. If
    autoload is the symbol `macro`, only try autoloading if
    symbol is an auto-loaded macro.

Function: **function-put** function property value [¶](#index-function_002dput)
:   This function sets property of function to value.
    function should be a symbol. This function is preferred to
    calling `put` for setting properties of a function, because it
    will allow us some day to implement remapping of old properties to new
    ones.

---

Previous: [Accessing Symbol Properties](#Symbol-Plists), Up: [Symbol Properties](#Symbol-Properties)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.2 Standard Symbol Properties [¶](#Standard-Symbol-Properties)

Here, we list the symbol properties which are used for special
purposes in Emacs. In the following table, whenever we say “the
named function”, that means the function whose name is the relevant
symbol; similarly for “the named variable” etc.

`:advertised-binding`
:   This property value specifies the preferred key binding, when showing
    documentation, for the named function. See [Substituting Key Bindings in Documentation](#Keys-in-Documentation).

`char-table-extra-slots`
:   The value, if non-`nil`, specifies the number of extra slots in
    the named char-table type. See [Char-Tables](#Char_002dTables).

`customized-face`

`face-defface-spec`

`saved-face`

`theme-face`
:   These properties are used to record a face’s standard, saved,
    customized, and themed face specs. Do not set them directly; they are
    managed by `defface` and related functions. See [Defining Faces](#Defining-Faces).

`customized-value`

`saved-value`

`standard-value`

`theme-value`
:   These properties are used to record a customizable variable’s standard
    value, saved value, customized-but-unsaved value, and themed values.
    Do not set them directly; they are managed by `defcustom` and
    related functions. See [Defining Customization Variables](#Variable-Definitions).

`definition-name`

`find-function-type-alist`
:   These properties help find the definition of a symbol in the source
    code when it might be hard to find the definition by textual search
    of the source file, as when the symbol is defined by a macro.
    See [Finding Definitions](#Finding-Definitions).

`disabled`
:   If the value is non-`nil`, the named function is disabled as a
    command. See [Disabling Commands](#Disabling-Commands).

`face-documentation`
:   The value stores the documentation string of the named face. This is
    set automatically by `defface`. See [Defining Faces](#Defining-Faces).

`history-length`
:   The value, if non-`nil`, specifies the maximum minibuffer history
    length for the named history list variable. See [Minibuffer History](#Minibuffer-History).

`important-return-value` [¶](#index-important_002dreturn_002dvalue-_0028symbol-property_0029)
:   A non-`nil` value makes the byte compiler warn about code that
    calls the named function without using its returned value. This is
    useful for functions where doing so is likely to be a mistake.
    This property is normally added to a function with `declare`
    (see [The `declare` Form](#Declare-Form)).

`interactive-form`
:   The value is an interactive form for the named function. Normally,
    you should not set this directly; use the `interactive` special
    form instead. See [Using `interactive`](#Using-Interactive).

`interactive-only`
:   If the value is non-`nil`, the named function should not be called
    from Lisp. The value is an error string or the function to call
    instead. See [Defining Commands](#Defining-Commands).

`menu-alias`
:   If non-nil, this symbol is an alias menu entry, and its own key binding
    should not be shown. See [Alias Menu Items](#Alias-Menu-Items).

`menu-enable`
:   The value is an expression for determining whether the named menu item
    should be enabled in menus. See [Simple Menu Items](#Simple-Menu-Items).

`mode-class`
:   If the value is `special`, the named major mode is special.
    See [Major Mode Conventions](#Major-Mode-Conventions).

`ignored-mouse-command`

`mouse-1-menu-command`
:   These properties affect how commands bound to `down-mouse-1` behave.
    See [Touchscreen Events](#Touchscreen-Events).

`permanent-local`
:   If the value is non-`nil`, the named variable is a buffer-local
    variable whose value should not be reset when changing major modes.
    See [Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal).

`permanent-local-hook`
:   If the value is non-`nil`, the named function should not be
    deleted from the local value of a hook variable when changing major
    modes. See [Setting Hooks](#Setting-Hooks).

`pure` [¶](#index-pure-_0028symbol-property_0029)
:   If the value is non-`nil`, the named function is considered to be
    pure (see [What Is a Function?](#What-Is-a-Function)). Calls with constant arguments can
    be evaluated at compile time. This may shift run time errors to
    compile time. This property is normally added to a function with
    `declare` (see [The `declare` Form](#Declare-Form)).

`risky-local-variable`
:   If the value is non-`nil`, the named variable is considered risky
    as a file-local variable. See [File Local Variables](#File-Local-Variables).

`safe-function` [¶](#index-safe_002dfunction-_0028symbol-property_0029)
:   If the value is non-`nil`, the named function is considered
    generally safe for evaluation. See [Determining whether a Function is Safe to Call](#Function-Safety).

`safe-local-eval-function`
:   If the value is non-`nil`, the named function is safe to call in
    file-local evaluation forms. See [File Local Variables](#File-Local-Variables).

`safe-local-variable`
:   The value specifies a function for determining safe file-local values
    for the named variable. See [File Local Variables](#File-Local-Variables).

`side-effect-free` [¶](#index-side_002deffect_002dfree-_0028symbol-property_0029)
:   A non-`nil` value indicates that the named function is free of
    side effects (see [What Is a Function?](#What-Is-a-Function)), so the byte compiler may
    ignore a call whose value is unused. If the property’s value is
    `error-free`, the byte compiler may even delete such unused
    calls. In addition to byte compiler optimizations, this property is
    also used for determining function safety (see [Determining whether a Function is Safe to Call](#Function-Safety)).
    This property is normally added to a function with
    `declare` (see [The `declare` Form](#Declare-Form)).

`undo-inhibit-region`
:   If non-`nil`, the named function prevents the `undo` operation
    from being restricted to the active region, if `undo` is invoked
    immediately after the function. See [Undo](#Undo).

`variable-documentation`
:   If non-`nil`, this specifies the named variable’s documentation
    string. This is set automatically by `defvar` and related
    functions. See [Documentation Basics](#Documentation-Basics).

---

Next: [Symbols with Position](#Symbols-with-Position), Previous: [Symbol Properties](#Symbol-Properties), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.5 Shorthands [¶](#Shorthands-1)

The symbol *shorthands*, sometimes known as “renamed symbols”, are
symbolic forms found in Lisp source. They’re just like regular
symbolic forms, except that when the Lisp reader encounters them, it
produces symbols which have a different and usually longer *print
name* (see [Symbol Components](#Symbol-Components)).

It is useful to think of shorthands as *abbreviating* the full
names of intended symbols. Despite this, do not confuse shorthands with the
Abbrev system (see [Abbrevs and Abbrev Expansion](#Abbrevs)).

Shorthands make Emacs Lisp’s *namespacing etiquette* easier to work
with. Since all symbols are stored in a single obarray
(see [Creating and Interning Symbols](#Creating-Symbols)), programmers commonly prefix each symbol
name with the name of the library where it originates. For example,
the functions `text-property-search-forward` and
`text-property-search-backward` both belong to the
`text-property-search.el` library (see [Loading](#Loading)). By properly
prefixing symbol names, one effectively prevents clashes between
similarly named symbols which belong to different libraries and thus do
different things. However, this practice commonly originates very
long symbols names, which are inconvenient to type and read after a
while. Shorthands solve these issues in a clean way.

Variable: **read-symbol-shorthands** [¶](#index-read_002dsymbol_002dshorthands)
:   This variable’s value is an alist whose elements have the form
    `(shorthand-prefix . longhand-prefix)`. Each element
    instructs the Lisp reader to read every symbol form which starts with
    shorthand-prefix as if it started with longhand-prefix
    instead.

    This variable may only be set in file-local variables (see [Local Variables in Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Variables) in The GNU Emacs Manual).

Here’s an example of shorthands usage in a hypothetical string
manipulating library `some-nice-string-utils.el`.

```
(defun some-nice-string-utils-split (separator s &optional omit-nulls)
  "A match-data saving variant of `split-string'."
  (save-match-data (split-string s separator omit-nulls)))

(defun some-nice-string-utils-lines (s)
  "Split string S at newline characters into a list of strings."
  (some-nice-string-utils-split "\\(\r\n\\|[\n\r]\\)" s))
```

As can be seen, it’s quite tedious to read or develop this code since
the symbol names to type are so long. We can use shorthands to
alleviate that.

```
(defun snu-split (separator s &optional omit-nulls)
  "A match-data saving variation on `split-string'."
  (save-match-data (split-string s separator omit-nulls)))

(defun snu-lines (s)
  "Split string S into a list of strings on newline characters."
  (snu-split "\\(\r\n\\|[\n\r]\\)" s))

;; Local Variables:
;; read-symbol-shorthands: (("snu-" . "some-nice-string-utils-"))
;; End:
```

Even though the two excerpts look different, they are quite identical
after the Lisp reader processes them. Both will lead to the very same
symbols being interned (see [Creating and Interning Symbols](#Creating-Symbols)). Thus loading or
byte-compiling any of the two files has equivalent results. The
shorthands `snu-split` and `snu-lines` used in the second
version are *not* interned in the obarray. This is easily seen
by moving point to the location where the shorthands are used and
waiting for ElDoc (see [Local Variables
in Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Programming-Language-Doc) in The GNU Emacs Manual) to hint at the true full name
of the symbol under point in the echo area.

Since `read-symbol-shorthands` is a file-local variable, it is
possible that multiple libraries depending on
`some-nice-string-utils-lines.el` refer to the same symbols under
*different* shorthands, or not using shorthands at all. In the
next example, the `my-tricks.el` library refers to the symbol
`some-nice-string-utils-lines` using the `sns-` prefix
instead of `snu-`.

```
(defun t-reverse-lines (s) (string-join (reverse (sns-lines s)) "\n")

;; Local Variables:
;; read-symbol-shorthands: (("t-" . "my-tricks-")
;;                          ("sns-" . "some-nice-string-utils-"))
;; End:
```

Note that if you have two shorthands in the same file where one is the
prefix of the other, the longer shorthand will be attempted first.
This happens regardless of the order you specify shorthands in the
local variables section of your file.

```
'(
  t//foo ; reads to 'my-tricks--foo', not 'my-tricks-/foo'
  t/foo  ; reads to 'my-tricks-foo'
  )

;; Local Variables:
;; read-symbol-shorthands: (("t/" . "my-tricks-")
;;                          ("t//" . "my-tricks--"))
;; End:
```

* [Exceptions](#Exceptions)

#### 9.5.1 Exceptions [¶](#Exceptions)

There are two exceptions to rules governing Shorthand transformations:

* Symbol forms comprised entirely of characters of
  the set ‘`^*+-/<=>_|`’ are not transformed.
  For example, it’s possible to use `-` or `/=` as shorthand
  prefixes, but that won’t shadow the arithmetic *functions* of
  those names.
* Symbol forms whose names start with ‘`#_`’ are not transformed.

---

Previous: [Shorthands](#Shorthands), Up: [Symbols](#Symbols)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.6 Symbols with Position [¶](#Symbols-with-Position-1)

A *symbol with position* is a symbol, called the *bare symbol*,
together with a nonnegative fixnum called the *position*.
Even though a symbol with position often acts like its bare symbol,
it is not a symbol: instead, it is an object that has both a bare symbol
and a position. Because symbols with position are not symbols,
they don’t have entries in the obarray, though their bare symbols
typically do (see [Creating and Interning Symbols](#Creating-Symbols)).

The byte compiler uses symbols with position,
records in them the position of each symbol occurrence, and uses those
positions in warning and error messages. They shouldn’t normally be
used otherwise. Doing so can cause unexpected results with basic
Emacs functions such as `eq` and `equal`.

The printed representation of a symbol with position uses the hash
notation outlined in [Printed Representation and Read Syntax](#Printed-Representation). It looks like
‘`#<symbol foo at 12345>`’. It has no read syntax. You can cause
just the bare symbol to be printed by binding the variable
`print-symbols-bare` to non-`nil` around the print
operation. The byte compiler does this before writing its output to
the compiled Lisp file.

When the flag variable `symbols-with-pos-enabled` is non-`nil`,
a symbol with position ordinarily behaves like its bare symbol.
For example, ‘`(eq (position-symbol 'foo 12345) 'foo)`’ yields `t`,
and `equal` likewise treats a symbol with position as its bare symbol.

When `symbols-with-pos-enabled` is `nil`, symbols with
position behave as themselves, not as symbols. For example, ‘`(eq
(position-symbol 'foo 12345) 'foo)`’ yields `nil`, and `equal`
likewise treats a symbol with position as not equal to its bare symbol.

Most of the time in Emacs `symbols-with-pos-enabled` is
`nil`, but the byte compiler and the native compiler bind it to
`t` when they run and Emacs runs a little more slowly in this case.

Typically, symbols with position are created by the byte compiler
calling the reader function `read-positioning-symbols`
(see [Input Functions](#Input-Functions)). One can also be created with the function
`position-symbol`.

Variable: **symbols-with-pos-enabled** [¶](#index-symbols_002dwith_002dpos_002denabled)
:   This variable affects the behavior of symbols with position when they
    are not being printed and are not arguments to one of the functions
    defined later in this section. When this variable is non-`nil`,
    such a symbol with position behaves like its bare symbol; otherwise it
    behaves as itself, not as a symbol.

Variable: **print-symbols-bare** [¶](#index-print_002dsymbols_002dbare)
:   When bound to non-`nil`, the Lisp printer prints only the bare
    symbol of a symbol with position, ignoring the position.
    Otherwise a symbol with position prints as itself, not as a symbol.

Function: **symbol-with-pos-p** object [¶](#index-symbol_002dwith_002dpos_002dp)
:   This function returns `t` if object is a symbol with
    position, `nil` otherwise.
    Unlike `symbolp`, this function ignores `symbols-with-pos-enabled`.

Function: **bare-symbol** sym [¶](#index-bare_002dsymbol)
:   This function returns the bare symbol of the symbol with
    position sym, or sym itself if it is already a symbol.
    For any other type of object, it signals an error.
    This function ignores `symbols-with-pos-enabled`.

Function: **symbol-with-pos-pos** sympos [¶](#index-symbol_002dwith_002dpos_002dpos)
:   This function returns the position, a nonnegative fixnum, from the symbol with
    position sympos. For any other type of object, it signals an error.
    This function ignores `symbols-with-pos-enabled`.

Function: **position-symbol** sym pos [¶](#index-position_002dsymbol)
:   Make a new symbol with position. The new object’s bare symbol is taken
    from sym, which is either a symbol, or a symbol with position
    whose bare symbol is used. The new object’s position is taken from
    pos, which is either a nonnegative fixnum, or a symbol with
    position whose position is used.
    Emacs signals an error if either argument is invalid.
    This function ignores `symbols-with-pos-enabled`.

---

Next: [Control Structures](#Control-Structures), Previous: [Symbols](#Symbols), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 10 Evaluation [¶](#Evaluation-1)

The *evaluation* of expressions in Emacs Lisp is performed by the
*Lisp interpreter*—a program that receives a Lisp object as input
and computes its *value as an expression*. How it does this depends
on the data type of the object, according to rules described in this
chapter. The interpreter runs automatically to evaluate portions of
your program, but can also be called explicitly via the Lisp primitive
function `eval`.

* [Introduction to Evaluation](#Intro-Eval)
* [Kinds of Forms](#Forms)
* [Quoting](#Quoting)
* [Backquote](#Backquote)
* [Eval](#Eval)
* [Deferred and Lazy Evaluation](#Deferred-Eval)

---

Next: [Kinds of Forms](#Forms), Up: [Evaluation](#Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.1 Introduction to Evaluation [¶](#Introduction-to-Evaluation)

The Lisp interpreter, or evaluator, is the part of Emacs that
computes the value of an expression that is given to it. When a
function written in Lisp is called, the evaluator computes the value
of the function by evaluating the expressions in the function body.
Thus, running any Lisp program really means running the Lisp
interpreter.

A Lisp object that is intended for evaluation is called a *form*
or *expression*[8](#FOOT8). The fact that forms are data objects
and not merely text is one of the fundamental differences between
Lisp-like languages and typical programming languages. Any object can
be evaluated, but in practice only numbers, symbols, lists and strings
are evaluated very often.

In subsequent sections, we will describe the details of what
evaluation means for each kind of form.

It is very common to read a Lisp form and then evaluate the form,
but reading and evaluation are separate activities, and either can be
performed alone. Reading per se does not evaluate anything; it
converts the printed representation of a Lisp object to the object
itself. It is up to the caller of `read` to specify whether this
object is a form to be evaluated, or serves some entirely different
purpose. See [Input Functions](#Input-Functions).

Evaluation is a recursive process, and evaluating a form often
involves evaluating parts within that form. For instance, when you
evaluate a *function call* form such as `(car x)`, Emacs
first evaluates the argument (the subform `x`). After evaluating
the argument, Emacs *executes* the function (`car`), and if
the function is written in Lisp, execution works by evaluating the
*body* of the function (in this example, however, `car` is
not a Lisp function; it is a primitive function implemented in C).
See [Functions](#Functions), for more information about functions and function
calls.

Evaluation takes place in a context called the *environment*,
which consists of the current values and bindings of all Lisp
variables (see [Variables](#Variables)).[9](#FOOT9) Whenever a form refers to a
variable without creating a new binding for it, the variable evaluates
to the value given by the current environment. Evaluating a form may
also temporarily alter the environment by binding variables
(see [Local Variables](#Local-Variables)).

Evaluating a form may also make changes that persist; these changes
are called *side effects*. An example of a form that produces a
side effect is `(setq foo 1)`.

Do not confuse evaluation with command key interpretation. The
editor command loop translates keyboard input into a command (an
interactively callable function) using the active keymaps, and then
uses `call-interactively` to execute that command. Executing the
command usually involves evaluation, if the command is written in
Lisp; however, this step is not considered a part of command key
interpretation. See [Command Loop](#Command-Loop).

---

Next: [Quoting](#Quoting), Previous: [Introduction to Evaluation](#Intro-Eval), Up: [Evaluation](#Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.2 Kinds of Forms [¶](#Kinds-of-Forms)

A Lisp object that is intended to be evaluated is called a
*form* (or an *expression*). How Emacs evaluates a form
depends on its data type. Emacs has three different kinds of form
that are evaluated differently: symbols, lists, and all other
types. This section describes all three kinds, one by one, starting
with the other types, which are self-evaluating forms.

* [Self-Evaluating Forms](#Self_002dEvaluating-Forms)
* [Symbol Forms](#Symbol-Forms)
* [Classification of List Forms](#Classifying-Lists)
* [Symbol Function Indirection](#Function-Indirection)
* [Evaluation of Function Forms](#Function-Forms)
* [Lisp Macro Evaluation](#Macro-Forms)
* [Special Forms](#Special-Forms)
* [Autoloading](#Autoloading)

---

Next: [Symbol Forms](#Symbol-Forms), Up: [Kinds of Forms](#Forms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.1 Self-Evaluating Forms [¶](#Self_002dEvaluating-Forms-1)

A *self-evaluating form* is any form that is not a list or
symbol. Self-evaluating forms evaluate to themselves: the result of
evaluation is the same object that was evaluated. Thus, the number 25
evaluates to 25, and the string `"foo"` evaluates to the string
`"foo"`. Likewise, evaluating a vector does not cause evaluation
of the elements of the vector—it returns the same vector with its
contents unchanged.

```
'123               ; A number, shown without evaluation.
     ⇒ 123
```

```
123                ; Evaluated as usual—result is the same.
     ⇒ 123
```

```
(eval '123)        ; Evaluated "by hand"—result is the same.
     ⇒ 123
```

```
(eval (eval '123)) ; Evaluating twice changes nothing.
     ⇒ 123
```

A self-evaluating form yields a value that becomes part of the program,
and you should not try to modify it via `setcar`, `aset` or
similar operations. The Lisp interpreter might unify the constants
yielded by your program’s self-evaluating forms, so that these
constants might share structure. See [Mutability](#Mutability).

It is common to write numbers, characters, strings, and even vectors
in Lisp code, taking advantage of the fact that they self-evaluate.
However, it is quite unusual to do this for types that lack a read
syntax, because there’s no way to write them textually. It is possible
to construct Lisp expressions containing these types by means of a Lisp
program. Here is an example:

```
;; Build an expression containing a buffer object.
(setq print-exp (list 'print (current-buffer)))
     ⇒ (print #<buffer eval.texi>)
```

```
;; Evaluate it.
(eval print-exp)
     -| #<buffer eval.texi>
     ⇒ #<buffer eval.texi>
```

---

Next: [Classification of List Forms](#Classifying-Lists), Previous: [Self-Evaluating Forms](#Self_002dEvaluating-Forms), Up: [Kinds of Forms](#Forms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.2 Symbol Forms [¶](#Symbol-Forms-1)

When a symbol is evaluated, it is treated as a variable. The result
is the variable’s value, if it has one. If the symbol has no value as
a variable, the Lisp interpreter signals an error. For more
information on the use of variables, see [Variables](#Variables).

In the following example, we set the value of a symbol with
`setq`. Then we evaluate the symbol, and get back the value that
`setq` stored.

```
(setq a 123)
     ⇒ 123
```

```
(eval 'a)
     ⇒ 123
```

```
a
     ⇒ 123
```

The symbols `nil` and `t` are treated specially, so that the
value of `nil` is always `nil`, and the value of `t` is
always `t`; you cannot set or bind them to any other values. Thus,
these two symbols act like self-evaluating forms, even though
`eval` treats them like any other symbol. A symbol whose name
starts with ‘`:`’ also self-evaluates in the same way; likewise,
its value ordinarily cannot be changed. See [Variables that Never Change](#Constant-Variables).

---

Next: [Symbol Function Indirection](#Function-Indirection), Previous: [Symbol Forms](#Symbol-Forms), Up: [Kinds of Forms](#Forms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.3 Classification of List Forms [¶](#Classification-of-List-Forms)

A form that is a nonempty list is either a function call, a macro
call, or a special form, according to its first element. These three
kinds of forms are evaluated in different ways, described below. The
remaining list elements constitute the *arguments* for the function,
macro, or special form.

The first step in evaluating a nonempty list is to examine its first
element. This element alone determines what kind of form the list is
and how the rest of the list is to be processed. The first element is
*not* evaluated, as it would be in some Lisp dialects such as
Scheme.

---

Next: [Evaluation of Function Forms](#Function-Forms), Previous: [Classification of List Forms](#Classifying-Lists), Up: [Kinds of Forms](#Forms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.4 Symbol Function Indirection [¶](#Symbol-Function-Indirection)

If the first element of the list is a symbol then evaluation
examines the symbol’s function cell, and uses its contents instead of
the original symbol. If the contents are another symbol, this
process, called *symbol function indirection*, is repeated until
it obtains a non-symbol. See [Naming a Function](#Function-Names), for more information
about symbol function indirection.
We eventually obtain a non-symbol, which ought to be a function or
other suitable object.

More precisely, we should now have a Lisp function (a lambda
expression), a byte-code function, a primitive function, a Lisp macro,
a special form, or an autoload object. Each of these types is a case
described in one of the following sections. If the object is not one
of these types, Emacs signals an `invalid-function` error.

The following example illustrates the symbol indirection process.
We use `fset` to set the function cell of a symbol and
`symbol-function` to get the function cell contents
(see [Accessing Function Cell Contents](#Function-Cells)). Specifically, we store the symbol
`car` into the function cell of `first`, and the symbol
`first` into the function cell of `erste`.

```
;; Build this function cell linkage:
;;   -------------       -----        -------        -------
;;  | #<subr car> | <-- | car |  <-- | first |  <-- | erste |
;;   -------------       -----        -------        -------
```

```
(symbol-function 'car)
     ⇒ #<subr car>
```

```
(fset 'first 'car)
     ⇒ car
```

```
(fset 'erste 'first)
     ⇒ first
```

```
(erste '(1 2 3))   ; Call the function referenced by erste.
     ⇒ 1
```

By contrast, the following example calls a function without any symbol
function indirection, because the first element is an anonymous Lisp
function, not a symbol.

```
((lambda (arg) (erste arg))
 '(1 2 3))
     ⇒ 1
```

Executing the function itself evaluates its body; this does involve
symbol function indirection when calling `erste`.

This form is rarely used and is now deprecated. Instead, you should write it
as:

```
(funcall (lambda (arg) (erste arg))
         '(1 2 3))
```

or just

```
(let ((arg '(1 2 3))) (erste arg))
```

The built-in function `indirect-function` provides an easy way to
perform symbol function indirection explicitly.

Function: **indirect-function** function [¶](#index-indirect_002dfunction)
:   This function returns the meaning of function as a function. If
    function is a symbol, then it finds function’s function
    definition and starts over with that value. If function is not a
    symbol, then it returns function itself.

    This function returns `nil` if the final symbol is unbound.

    There is also a second, optional argument that is obsolete and has no
    effect.

    Here is how you could define `indirect-function` in Lisp:

    ```
    (defun indirect-function (function)
      (if (and function
               (symbolp function))
          (indirect-function (symbol-function function))
        function))
    ```

---

Next: [Lisp Macro Evaluation](#Macro-Forms), Previous: [Symbol Function Indirection](#Function-Indirection), Up: [Kinds of Forms](#Forms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.5 Evaluation of Function Forms [¶](#Evaluation-of-Function-Forms)

If the first element of a list being evaluated is a Lisp function
object, byte-code object or primitive function object, then that list is
a *function call*. For example, here is a call to the function
`+`:

```
(+ 1 x)
```

The first step in evaluating a function call is to evaluate the
remaining elements of the list from left to right. The results are the
actual argument values, one value for each list element. The next step
is to call the function with this list of arguments, effectively using
the function `apply` (see [Calling Functions](#Calling-Functions)). If the function
is written in Lisp, the arguments are used to bind the argument
variables of the function (see [Lambda Expressions](#Lambda-Expressions)); then the forms
in the function body are evaluated in order, and the value of the last
body form becomes the value of the function call.

---

Next: [Special Forms](#Special-Forms), Previous: [Evaluation of Function Forms](#Function-Forms), Up: [Kinds of Forms](#Forms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.6 Lisp Macro Evaluation [¶](#Lisp-Macro-Evaluation)

If the first element of a list being evaluated is a macro object, then
the list is a *macro call*. When a macro call is evaluated, the
elements of the rest of the list are *not* initially evaluated.
Instead, these elements themselves are used as the arguments of the
macro. The macro definition computes a replacement form, called the
*expansion* of the macro, to be evaluated in place of the original
form. The expansion may be any sort of form: a self-evaluating
constant, a symbol, or a list. If the expansion is itself a macro call,
this process of expansion repeats until some other sort of form results.

Ordinary evaluation of a macro call finishes by evaluating the
expansion. However, the macro expansion is not necessarily evaluated
right away, or at all, because other programs also expand macro calls,
and they may or may not evaluate the expansions.

Normally, the argument expressions are not evaluated as part of
computing the macro expansion, but instead appear as part of the
expansion, so they are computed when the expansion is evaluated.

For example, given a macro defined as follows:

```
(defmacro cadr (x)
  (list 'car (list 'cdr x)))
```

an expression such as `(cadr (assq 'handler list))` is a macro
call, and its expansion is:

```
(car (cdr (assq 'handler list)))
```

Note that the argument `(assq 'handler list)` appears in the
expansion.

See [Macros](#Macros), for a complete description of Emacs Lisp macros.

---

Next: [Autoloading](#Autoloading), Previous: [Lisp Macro Evaluation](#Macro-Forms), Up: [Kinds of Forms](#Forms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.7 Special Forms [¶](#Special-Forms-1)

A *special form* is a primitive specially marked so that
its arguments are not all evaluated. Most special forms define control
structures or perform variable bindings—things which functions cannot
do.

Each special form has its own rules for which arguments are evaluated
and which are used without evaluation. Whether a particular argument is
evaluated may depend on the results of evaluating other arguments.

If an expression’s first symbol is that of a special form, the
expression should follow the rules of that special form; otherwise,
Emacs’s behavior is not well-defined (though it will not crash). For
example, `((lambda (x) x . 3) 4)` contains a subexpression that
begins with `lambda` but is not a well-formed `lambda`
expression, so Emacs may signal an error, or may return 3 or 4 or
`nil`, or may behave in other ways.

Function: **special-form-p** object [¶](#index-special_002dform_002dp)
:   This predicate tests whether its argument is a special form, and
    returns `t` if so, `nil` otherwise.

Here is a list, in alphabetical order, of all of the special forms in
Emacs Lisp with a reference to where each is described.

`and`
:   see [Constructs for Combining Conditions](#Combining-Conditions)

`catch`
:   see [Explicit Nonlocal Exits: `catch` and `throw`](#Catch-and-Throw)

`cond`
:   see [Conditionals](#Conditionals)

`condition-case`
:   see [Writing Code to Handle Errors](#Handling-Errors)

`defconst`
:   see [Defining Global Variables](#Defining-Variables)

`defvar`
:   see [Defining Global Variables](#Defining-Variables)

`function`
:   see [Anonymous Functions](#Anonymous-Functions)

`if`
:   see [Conditionals](#Conditionals)

`interactive`
:   see [Interactive Call](#Interactive-Call)

`lambda`
:   see [Lambda Expressions](#Lambda-Expressions)

`let`

`let*`
:   see [Local Variables](#Local-Variables)

`or`
:   see [Constructs for Combining Conditions](#Combining-Conditions)

`prog1`

`prog2`

`progn`
:   see [Sequencing](#Sequencing)

`quote`
:   see [Quoting](#Quoting)

`save-current-buffer`
:   see [The Current Buffer](#Current-Buffer)

`save-excursion`
:   see [Excursions](#Excursions)

`save-restriction`
:   see [Narrowing](#Narrowing)

`setq`
:   see [Setting Variable Values](#Setting-Variables)

`setq-default`
:   see [Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal)

`unwind-protect`
:   see [Nonlocal Exits](#Nonlocal-Exits)

`while`
:   see [Iteration](#Iteration)

> **Common Lisp note:** Here are some comparisons of special forms in
> GNU Emacs Lisp and Common Lisp. `setq`, `if`, and
> `catch` are special forms in both Emacs Lisp and Common Lisp.
> `save-excursion` is a special form in Emacs Lisp, but
> doesn’t exist in Common Lisp. `throw` is a special form in
> Common Lisp (because it must be able to throw multiple values), but it
> is a function in Emacs Lisp (which doesn’t have multiple
> values).

---

Previous: [Special Forms](#Special-Forms), Up: [Kinds of Forms](#Forms)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.2.8 Autoloading [¶](#Autoloading-1)

The *autoload* feature allows you to call a function or macro
whose function definition has not yet been loaded into Emacs. It
specifies which file contains the definition. When an autoload object
appears as a symbol’s function definition, calling that symbol as a
function automatically loads the specified file; then it calls the
real definition loaded from that file. The way to arrange for an
autoload object to appear as a symbol’s function definition is
described in [Autoload](#Autoload).

---

Next: [Backquote](#Backquote), Previous: [Kinds of Forms](#Forms), Up: [Evaluation](#Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.3 Quoting [¶](#Quoting-1)

The special form `quote` returns its single argument, as written,
without evaluating it. This provides a way to include constant symbols
and lists, which are not self-evaluating objects, in a program. (It is
not necessary to quote self-evaluating objects such as numbers, strings,
and vectors.)

Special Form: **quote** object [¶](#index-quote)
:   This special form returns object, without evaluating it.
    The returned value might be shared and should not be modified.
    See [Self-Evaluating Forms](#Self_002dEvaluating-Forms).

Because `quote` is used so often in programs, Lisp provides a
convenient read syntax for it. An apostrophe character (‘`'`’)
followed by a Lisp object (in read syntax) expands to a list whose first
element is `quote`, and whose second element is the object. Thus,
the read syntax `'x` is an abbreviation for `(quote x)`.

Here are some examples of expressions that use `quote`:

```
(quote (+ 1 2))
     ⇒ (+ 1 2)
```

```
(quote foo)
     ⇒ foo
```

```
'foo
     ⇒ foo
```

```
''foo
     ⇒ 'foo
```

```
'(quote foo)
     ⇒ 'foo
```

```
['foo]
     ⇒ ['foo]
```

Although the expressions `(list '+ 1 2)` and `'(+ 1 2)`
both yield lists equal to `(+ 1 2)`, the former yields a
freshly-minted mutable list whereas the latter yields a list
built from conses that might be shared and should not be modified.
See [Self-Evaluating Forms](#Self_002dEvaluating-Forms).

Other quoting constructs include `function` (see [Anonymous Functions](#Anonymous-Functions)), which causes an anonymous lambda expression written in Lisp
to be compiled, and ‘`` ` ``’ (see [Backquote](#Backquote)), which is used to quote
only part of a list, while computing and substituting other parts.

---

Next: [Eval](#Eval), Previous: [Quoting](#Quoting), Up: [Evaluation](#Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.4 Backquote [¶](#Backquote-1)

*Backquote constructs* allow you to quote a list, but
selectively evaluate elements of that list. In the simplest case, it
is identical to the special form
`quote`
(described in the previous section; see [Quoting](#Quoting)).
For example, these two forms yield identical results:

```
`(a list of (+ 2 3) elements)
     ⇒ (a list of (+ 2 3) elements)
```

```
'(a list of (+ 2 3) elements)
     ⇒ (a list of (+ 2 3) elements)
```

The special marker ‘`,`’ inside of the argument to backquote
indicates a value that isn’t constant. The Emacs Lisp evaluator
evaluates the argument of ‘`,`’, and puts the value in the list
structure:

```
`(a list of ,(+ 2 3) elements)
     ⇒ (a list of 5 elements)
```

Substitution with ‘`,`’ is allowed at deeper levels of the list
structure also. For example:

```
`(1 2 (3 ,(+ 4 5)))
     ⇒ (1 2 (3 9))
```

You can also *splice* an evaluated value into the resulting list,
using the special marker ‘`,@`’. The elements of the spliced list
become elements at the same level as the other elements of the resulting
list. The equivalent code without using ‘`` ` ``’ is often unreadable.
Here are some examples:

```
(setq some-list '(2 3))
     ⇒ (2 3)
```

```
(cons 1 (append some-list '(4) some-list))
     ⇒ (1 2 3 4 2 3)
```

```
`(1 ,@some-list 4 ,@some-list)
     ⇒ (1 2 3 4 2 3)
```

```

```

```
(setq list '(hack foo bar))
     ⇒ (hack foo bar)
```

```
(cons 'use
  (cons 'the
    (cons 'words (append (cdr list) '(as elements)))))
     ⇒ (use the words foo bar as elements)
```

```
`(use the words ,@(cdr list) as elements)
     ⇒ (use the words foo bar as elements)
```

A backquote construct acts like `quote` in that it yields conses,
vectors and strings that might be shared and should not be modified.
See [Self-Evaluating Forms](#Self_002dEvaluating-Forms).

---

Next: [Deferred and Lazy Evaluation](#Deferred-Eval), Previous: [Backquote](#Backquote), Up: [Evaluation](#Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.5 Eval [¶](#Eval-1)

Most often, forms are evaluated automatically, by virtue of their
occurrence in a program being run. On rare occasions, you may need to
write code that evaluates a form that is computed at run time, such as
after reading a form from text being edited or getting one from a
property list. On these occasions, use the `eval` function.
Often `eval` is not needed and something else should be used instead.
For example, to get the value of a variable, while `eval` works,
`symbol-value` is preferable; or rather than store expressions
in a property list that then need to go through `eval`, it is better to
store functions instead that are then passed to `funcall`.

The functions and variables described in this section evaluate forms,
specify limits to the evaluation process, or record recently returned
values. Loading a file also does evaluation (see [Loading](#Loading)).

It is generally cleaner and more flexible to store a function in a
data structure, and call it with `funcall` or `apply`, than
to store an expression in the data structure and evaluate it. Using
functions provides the ability to pass information to them as
arguments.

Function: **eval** form &optional lexical [¶](#index-eval)
:   This is the basic function for evaluating an expression. It evaluates
    form in the current environment, and returns the result. The
    type of the form object determines how it is evaluated.
    See [Kinds of Forms](#Forms).

    The argument lexical specifies the scoping rule for local
    variables (see [Scoping Rules for Variable Bindings](#Variable-Scoping)). If it is `t`, that means
    to evaluate form using lexical scoping; this is the recommended
    value. If it is omitted or `nil`, that means to use the old
    dynamic-only variable scoping rule.

    The value of lexical can also be a non-empty list specifying a
    particular *lexical environment* for lexical bindings; however,
    this feature is only useful for specialized purposes, such as in Emacs
    Lisp debuggers. Each member of the list is either a cons cell which
    represents a lexical symbol-value pair, or a symbol representing a
    (special) variable that would use dynamic scoping if bound.

    Since `eval` is a function, the argument expression that appears
    in a call to `eval` is evaluated twice: once as preparation before
    `eval` is called, and again by the `eval` function itself.
    Here is an example:

    ```
    (setq foo 'bar)
         ⇒ bar
    ```

    ```
    (setq bar 'baz)
         ⇒ baz
    ;; Here eval receives argument foo
    (eval 'foo)
         ⇒ bar
    ;; Here eval receives argument bar, which is the value of foo
    (eval foo)
         ⇒ baz
    ```

    The number of currently active calls to `eval` is limited to
    `max-lisp-eval-depth` (see below).

Command: **eval-region** start end &optional stream read-function [¶](#index-eval_002dregion)
:   This function evaluates the forms in the current buffer in the region
    defined by the positions start and end. It reads forms from
    the region and calls `eval` on them until the end of the region is
    reached, or until an error is signaled and not handled.

    By default, `eval-region` does not produce any output. However,
    if stream is non-`nil`, any output produced by output
    functions (see [Output Functions](#Output-Functions)), as well as the values that
    result from evaluating the expressions in the region are printed using
    stream. See [Output Streams](#Output-Streams).

    If read-function is non-`nil`, it should be a function,
    which is used instead of `read` to read expressions one by one.
    This function is called with one argument, the stream for reading
    input. You can also use the variable `load-read-function`
    (see [How Programs Do Loading](#Definition-of-load_002dread_002dfunction))
    to specify this function, but it is more robust to use the
    read-function argument.

    `eval-region` does not move point. It always returns `nil`.

Command: **eval-buffer** &optional buffer-or-name stream filename unibyte print [¶](#index-eval_002dbuffer)
:   This is similar to `eval-region`, but the arguments provide
    different optional features. `eval-buffer` operates on the
    entire accessible portion of buffer buffer-or-name
    (see [Narrowing](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Narrowing) in The GNU Emacs Manual).
    buffer-or-name can be a buffer, a buffer name (a string), or
    `nil` (or omitted), which means to use the current buffer.
    stream is used as in `eval-region`, unless stream is
    `nil` and print non-`nil`. In that case, values that
    result from evaluating the expressions are still discarded, but the
    output of the output functions is printed in the echo area.
    filename is the file name to use for `load-history`
    (see [Unloading](#Unloading)), and defaults to `buffer-file-name`
    (see [Buffer File Name](#Buffer-File-Name)). If unibyte is non-`nil`,
    `read` converts strings to unibyte whenever possible.

User Option: **max-lisp-eval-depth** [¶](#index-max_002dlisp_002deval_002ddepth)
:   This variable defines the maximum depth allowed in calls to `eval`,
    `apply`, and `funcall` before an error is signaled (with error
    message `"Lisp nesting exceeds max-lisp-eval-depth"`).

    This limit, with the associated error when it is exceeded, is how
    Emacs Lisp avoids infinite recursion on an ill-defined function. If
    you increase the value of `max-lisp-eval-depth` too much, such
    code can cause stack overflow instead. On some systems, this overflow
    can be handled. In that case, normal Lisp evaluation is interrupted
    and control is transferred back to the top level command loop
    (`top-level`). Note that there is no way to enter Emacs Lisp
    debugger in this situation. See [Entering the Debugger on an Error](#Error-Debugging).

    The depth limit counts internal uses of `eval`, `apply`, and
    `funcall`, such as for calling the functions mentioned in Lisp
    expressions, and recursive evaluation of function call arguments and
    function body forms, as well as explicit calls in Lisp code.

    The default value of this variable is 1600. If you set it to a value
    less than 100, Lisp will reset it to 100 if the given value is
    reached.

User Option: **lisp-eval-depth-reserve** [¶](#index-lisp_002deval_002ddepth_002dreserve)
:   In order to be able to debug infinite recursion errors, when invoking the
    Lisp debugger, Emacs increases temporarily the value of
    `max-lisp-eval-depth`, if there is little room left, to make sure
    the debugger itself has room to execute. The same happens when
    running the handler of a `handler-bind`. See [Writing Code to Handle Errors](#Handling-Errors).

    The variable `lisp-eval-depth-reserve` bounds the extra depth
    that Emacs can add to `max-lisp-eval-depth` for those
    exceptional circumstances.

    The default value of this variable is 200.

Variable: **values** [¶](#index-values)
:   The value of this variable is a list of the values returned by all the
    expressions that were read, evaluated, and printed from buffers
    (including the minibuffer) by the standard Emacs commands which do
    this. (Note that this does *not* include evaluation in
    `*ielm*` buffers, nor evaluation using `C-j`, `C-x C-e`,
    and similar evaluation commands in `lisp-interaction-mode`.)

    This variable is obsolete, and will be removed in a future version,
    since it constantly enlarges the memory footprint of the Emacs
    process. For that reason, we recommend against using it.

    The elements of `values` are ordered most recent first.

    ```
    (setq x 1)
         ⇒ 1
    ```

    ```
    (list 'A (1+ 2) auto-save-default)
         ⇒ (A 3 t)
    ```

    ```
    values
         ⇒ ((A 3 t) 1 ...)
    ```

    This variable could be useful for referring back to values of forms
    recently evaluated. It is generally a bad idea to print the value of
    `values` itself, since this may be very long. Instead, examine
    particular elements, like this:

    ```
    ;; Refer to the most recent evaluation result.
    (nth 0 values)
         ⇒ (A 3 t)
    ```

    ```
    ;; That put a new element on,
    ;;   so all elements move back one.
    (nth 1 values)
         ⇒ (A 3 t)
    ```

    ```
    ;; This gets the element that was next-to-most-recent
    ;;   before this example.
    (nth 3 values)
         ⇒ 1
    ```

---

Previous: [Eval](#Eval), Up: [Evaluation](#Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.6 Deferred and Lazy Evaluation [¶](#Deferred-and-Lazy-Evaluation)

Sometimes it is useful to delay the evaluation of an expression, for
example if you want to avoid performing a time-consuming calculation
if it turns out that the result is not needed in the future of the
program. The `thunk` library provides the following functions
and macros to support such *deferred evaluation*:

Macro: **thunk-delay** forms… [¶](#index-thunk_002ddelay)
:   Return a *thunk* for evaluating the forms. A thunk is a
    closure (see [Closures](#Closures)) that inherits the lexical environment of the
    `thunk-delay` call. Using this macro requires
    `lexical-binding`.

Function: **thunk-force** thunk [¶](#index-thunk_002dforce)
:   Force thunk to perform the evaluation of the forms specified in
    the `thunk-delay` that created the thunk. The result of the
    evaluation of the last form is returned. The thunk also
    “remembers” that it has been forced: Any further calls of
    `thunk-force` with the same thunk will just return the same
    result without evaluating the forms again.

Macro: **thunk-let** (bindings…) forms… [¶](#index-thunk_002dlet)
:   This macro is analogous to `let` but creates “lazy” variable
    bindings. Any binding has the form `(symbol value-form)`. Unlike `let`, the evaluation of any
    value-form is deferred until the binding of the according
    symbol is used for the first time when evaluating the
    forms. Any value-form is evaluated at most once. Using
    this macro requires `lexical-binding`.

Example:

```
(defun f (number)
  (thunk-let ((derived-number
              (progn (message "Calculating 1 plus 2 times %d" number)
                     (1+ (* 2 number)))))
    (if (> number 10)
        derived-number
      number)))
```

```

```

```
(f 5)
⇒ 5
```

```

```

```
(f 12)
-| Calculating 1 plus 2 times 12
⇒ 25
```

```

```

Because of the special nature of lazily bound variables, it is an error
to set them (e.g. with `setq`).

Macro: **thunk-let\*** (bindings…) forms… [¶](#index-thunk_002dlet_002a)
:   This is like `thunk-let` but any expression in bindings is allowed
    to refer to preceding bindings in this `thunk-let*` form. Using
    this macro requires `lexical-binding`.

```
(thunk-let* ((x (prog2 (message "Calculating x...")
                    (+ 1 1)
                  (message "Finished calculating x")))
             (y (prog2 (message "Calculating y...")
                    (+ x 1)
                  (message "Finished calculating y")))
             (z (prog2 (message "Calculating z...")
                    (+ y 1)
                  (message "Finished calculating z")))
             (a (prog2 (message "Calculating a...")
                    (+ z 1)
                  (message "Finished calculating a"))))
  (* z x))

-| Calculating z...
-| Calculating y...
-| Calculating x...
-| Finished calculating x
-| Finished calculating y
-| Finished calculating z
⇒ 8
```

`thunk-let` and `thunk-let*` use thunks implicitly: their
expansion creates helper symbols and binds them to thunks wrapping the
binding expressions. All references to the original variables in the
body forms are then replaced by an expression that calls
`thunk-force` with the according helper variable as the argument.
So, any code using `thunk-let` or `thunk-let*` could be
rewritten to use thunks, but in many cases using these macros results
in nicer code than using thunks explicitly.

---

Next: [Variables](#Variables), Previous: [Evaluation](#Evaluation), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 11 Control Structures [¶](#Control-Structures-1)

A Lisp program consists of a set of *expressions*, or
*forms* (see [Kinds of Forms](#Forms)). We control the order of execution of
these forms by enclosing them in *control structures*. Control
structures are special forms which control when, whether, or how many
times to execute the forms they contain.

The simplest order of execution is sequential execution: first form
a, then form b, and so on. This is what happens when you
write several forms in succession in the body of a function, or at top
level in a file of Lisp code—the forms are executed in the order
written. We call this *textual order*. For example, if a function
body consists of two forms a and b, evaluation of the
function evaluates first a and then b. The result of
evaluating b becomes the value of the function.

Explicit control structures make possible an order of execution other
than sequential.

Emacs Lisp provides several kinds of control structure, including
other varieties of sequencing, conditionals, iteration, and (controlled)
jumps—all discussed below. The built-in control structures are
special forms since their subforms are not necessarily evaluated or not
evaluated sequentially. You can use macros to define your own control
structure constructs (see [Macros](#Macros)).

* [Sequencing](#Sequencing)
* [Conditionals](#Conditionals)
* [Constructs for Combining Conditions](#Combining-Conditions)
* [Pattern-Matching Conditional](#Pattern_002dMatching-Conditional)
* [Iteration](#Iteration)
* [Generators](#Generators)
* [Nonlocal Exits](#Nonlocal-Exits)
* [Conditional Compilation](#Conditional-Compilation)

---

Next: [Conditionals](#Conditionals), Up: [Control Structures](#Control-Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.1 Sequencing [¶](#Sequencing-1)

Evaluating forms in the order they appear is the most common way
control passes from one form to another. In some contexts, such as in a
function body, this happens automatically. Elsewhere you must use a
control structure construct to do this: `progn`, the simplest
control construct of Lisp.

A `progn` special form looks like this:

```
(progn a b c ...)
```

and it says to execute the forms a, b, c, and so on, in
that order. These forms are called the *body* of the `progn` form.
The value of the last form in the body becomes the value of the entire
`progn`. `(progn)` returns `nil`.

In the early days of Lisp, `progn` was the only way to execute
two or more forms in succession and use the value of the last of them.
But programmers found they often needed to use a `progn` in the
body of a function, where (at that time) only one form was allowed. So
the body of a function was made into an implicit `progn`:
several forms are allowed just as in the body of an actual `progn`.
Many other control structures likewise contain an implicit `progn`.
As a result, `progn` is not used as much as it was many years ago.
It is needed now most often inside an `unwind-protect`, `and`,
`or`, or in the then-part of an `if`.

Special Form: **progn** forms… [¶](#index-progn)
:   This special form evaluates all of the forms, in textual
    order, returning the result of the final form.

    ```
    (progn (print "The first form")
           (print "The second form")
           (print "The third form"))
         -| "The first form"
         -| "The second form"
         -| "The third form"
    ⇒ "The third form"
    ```

Two other constructs likewise evaluate a series of forms but return
different values:

Special Form: **prog1** form1 forms… [¶](#index-prog1)
:   This special form evaluates form1 and all of the forms, in
    textual order, returning the result of form1.

    ```
    (prog1 (print "The first form")
           (print "The second form")
           (print "The third form"))
         -| "The first form"
         -| "The second form"
         -| "The third form"
    ⇒ "The first form"
    ```

    Here is a way to remove the first element from a list in the variable
    `x`, then return the value of that former element:

    ```
    (prog1 (car x) (setq x (cdr x)))
    ```

Special Form: **prog2** form1 form2 forms… [¶](#index-prog2)
:   This special form evaluates form1, form2, and all of the
    following forms, in textual order, returning the result of
    form2.

    ```
    (prog2 (print "The first form")
           (print "The second form")
           (print "The third form"))
         -| "The first form"
         -| "The second form"
         -| "The third form"
    ⇒ "The second form"
    ```

---

Next: [Constructs for Combining Conditions](#Combining-Conditions), Previous: [Sequencing](#Sequencing), Up: [Control Structures](#Control-Structures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
