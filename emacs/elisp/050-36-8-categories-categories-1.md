### 36.8 Categories [¶](#Categories-1)

*Categories* provide an alternate way of classifying characters
syntactically. You can define several categories as needed, then
independently assign each character to one or more categories. Unlike
syntax classes, categories are not mutually exclusive; it is normal for
one character to belong to several categories.

Each buffer has a *category table* which records which categories
are defined and also which characters belong to each category. Each
category table defines its own categories, but normally these are
initialized by copying from the standard categories table, so that the
standard categories are available in all modes.

Each category has a name, which is an ASCII printing character in
the range ‘’ to ‘`~`’. You specify the name of a category
when you define it with `define-category`.

The category table is actually a char-table (see [Char-Tables](#Char_002dTables)).
The element of the category table at index c is a *category
set*—a bool-vector—that indicates which categories character c
belongs to. In this category set, if the element at index cat is
`t`, that means category cat is a member of the set, and that
character c belongs to category cat.

For the next three functions, the optional argument table
defaults to the current buffer’s category table.

Function: **define-category** char docstring &optional table [¶](#index-define_002dcategory)
:   This function defines a new category, with name char and
    documentation docstring, for the category table table.

    Here’s an example of defining a new category for characters that have
    strong right-to-left directionality (see [Bidirectional Display](#Bidirectional-Display))
    and using it in a special category table. To obtain the information
    about the directionality of characters, the example code uses the
    ‘`bidi-class`’ Unicode property (see [bidi-class](#Character-Properties)).

    ```
    (defvar special-category-table-for-bidi
      ;;     Make an empty category-table.
      (let ((category-table (make-category-table))
            ;; Create a char-table which gives the 'bidi-class' Unicode
            ;; property for each character.
            (uniprop-table
             (unicode-property-table-internal 'bidi-class)))
        (define-category ?R "Characters of bidi-class R, AL, or RLO"
                         category-table)
        ;; Modify the category entry of each character whose
        ;; 'bidi-class' Unicode property is R, AL, or RLO --
        ;; these have a right-to-left directionality.
        (map-char-table
         (lambda (key val)
           (if (memq val '(R AL RLO))
               (modify-category-entry key ?R category-table)))
         uniprop-table)
        category-table))
    ```

Function: **category-docstring** category &optional table [¶](#index-category_002ddocstring)
:   This function returns the documentation string of category category
    in category table table.

    ```
    (category-docstring ?a)
         ⇒ "ASCII"
    (category-docstring ?l)
         ⇒ "Latin"
    ```

Function: **get-unused-category** &optional table [¶](#index-get_002dunused_002dcategory)
:   This function returns a category name (a character) which is not
    currently defined in table. If all possible categories are in use
    in table, it returns `nil`.

Function: **category-table** [¶](#index-category_002dtable)
:   This function returns the current buffer’s category table.

Function: **category-table-p** object [¶](#index-category_002dtable_002dp)
:   This function returns `t` if object is a category table,
    otherwise `nil`.

Function: **standard-category-table** [¶](#index-standard_002dcategory_002dtable)
:   This function returns the standard category table.

Function: **copy-category-table** &optional table [¶](#index-copy_002dcategory_002dtable)
:   This function constructs a copy of table and returns it. If
    table is not supplied (or is `nil`), it returns a copy of the
    standard category table. Otherwise, an error is signaled if table
    is not a category table.

Function: **set-category-table** table [¶](#index-set_002dcategory_002dtable)
:   This function makes table the category table for the current
    buffer. It returns table.

Function: **make-category-table** [¶](#index-make_002dcategory_002dtable)
:   This creates and returns an empty category table. In an empty category
    table, no categories have been allocated, and no characters belong to
    any categories.

Function: **make-category-set** categories [¶](#index-make_002dcategory_002dset)
:   This function returns a new category set—a bool-vector—whose initial
    contents are the categories listed in the string categories. The
    elements of categories should be category names; the new category
    set has `t` for each of those categories, and `nil` for all
    other categories.

    ```
    (make-category-set "al")
         ⇒ #&128"\0\0\0\0\0\0\0\0\0\0\0\0\2\20\0\0"
    ```

Function: **char-category-set** char [¶](#index-char_002dcategory_002dset)
:   This function returns the category set for character char in the
    current buffer’s category table. This is the bool-vector which
    records which categories the character char belongs to. The
    function `char-category-set` does not allocate storage, because
    it returns the same bool-vector that exists in the category table.

    ```
    (char-category-set ?a)
         ⇒ #&128"\0\0\0\0\0\0\0\0\0\0\0\0\2\20\0\0"
    ```

Function: **category-set-mnemonics** category-set [¶](#index-category_002dset_002dmnemonics)
:   This function converts the category set category-set into a string
    containing the characters that designate the categories that are members
    of the set.

    ```
    (category-set-mnemonics (char-category-set ?a))
         ⇒ "al"
    ```

Function: **modify-category-entry** char category &optional table reset [¶](#index-modify_002dcategory_002dentry)
:   This function modifies the category set of char in category
    table table (which defaults to the current buffer’s category
    table). char can be a character, or a cons cell of the form
    `(min . max)`; in the latter case, the function
    modifies the category sets of all characters in the range between
    min and max, inclusive.

    Normally, it modifies a category set by adding category to it.
    But if reset is non-`nil`, then it deletes category
    instead.

Command: **describe-categories** &optional buffer-or-name [¶](#index-describe_002dcategories)
:   This function describes the category specifications in the current
    category table. It inserts the descriptions in a buffer, and then
    displays that buffer. If buffer-or-name is non-`nil`, it
    describes the category table of that buffer instead.

---

Next: [Parsing Program Source](#Parsing-Program-Source), Previous: [Syntax Tables](#Syntax-Tables), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 37 Parsing Expression Grammars [¶](#Parsing-Expression-Grammars-1)

Emacs Lisp provides several tools for parsing and matching text,
from regular expressions (see [Regular Expressions](#Regular-Expressions)) to full
left-to-right (a.k.a. LL) grammar parsers (see [Bovine parser development](https://www.gnu.org/software/emacs/manual/html_mono/bovine.html#Top)). *Parsing Expression Grammars*
(PEG) are another approach to text parsing that offer more
structure and composability than regular expressions, but less
complexity than context-free grammars.

A Parsing Expression Grammar (PEG) describes a formal language
in terms of a set of rules for recognizing strings in the language. In
Emacs, a PEG parser is defined as a list of named rules, each
of which matches text patterns and/or contains references to other
rules. Parsing is initiated with the function `peg-run` or the
macro `peg-parse` (see below), and parses text after point in the
current buffer, using a given set of rules.

Each rule in a PEG is referred to as a *parsing
expression* (PEX), and can be specified a literal string, a
regexp-like character range or set, a peg-specific construct resembling
an Emacs Lisp function call, a reference to another rule, or a
combination of any of these. A grammar is expressed as a tree of rules
in which one rule is typically treated as a “root” or “entry-point”
rule. For instance:

```
((number sign digit (* digit))
 (sign   (or "+" "-" ""))
 (digit  [0-9]))
```

Once defined, grammars can be used to parse text after point in the
current buffer, in a number of ways. The `peg-parse` macro is the
simplest:

Macro: **peg-parse** &rest pexs [¶](#index-peg_002dparse)
:   Match pexs at point.

```
(peg-parse
  (number sign digit (* digit))
  (sign   (or "+" "-" ""))
  (digit  [0-9]))
```

While this macro is simple it is also inflexible, as the rules must be
written directly into the source code. More flexibility can be gained
by using a combination of other functions and macros.

Macro: **with-peg-rules** rules &rest body [¶](#index-with_002dpeg_002drules)
:   Execute body with rules, a list of PEXs, in
    effect. Within BODY, parsing is initiated with a call to
    `peg-run`.

Function: **peg-run** peg-matcher &optional failure-function success-function [¶](#index-peg_002drun)
:   This function accepts a single peg-matcher, which is the result of
    calling `peg` (see below) on a named rule, usually the entry-point
    of a larger grammar.

    At the end of parsing, one of failure-function or
    success-function is called, depending on whether the parsing
    succeeded or not. If success-function is provided, it should be a
    function that receives as its only argument an anonymous function that
    runs all the actions collected on the stack during parsing. By default
    this anonymous function is simply executed. If parsing fails, a
    function provided as failure-function will be called with a list
    of PEG expressions that failed during parsing. By default
    this list is discarded.

The peg-matcher passed to `peg-run` is produced by a call to
`peg`:

Macro: **peg** &rest pexs [¶](#index-peg)
:   Convert pexs into a single peg-matcher suitable for passing to
    `peg-run`.

The `peg-parse` example above expands to a set of calls to these
functions, and could be written in full as:

```
(with-peg-rules
    ((number sign digit (* digit))
     (sign   (or "+" "-" ""))
     (digit  [0-9]))
  (peg-run (peg number)))
```

This approach allows more explicit control over the “entry-point” of
parsing, and allows the combination of rules from different sources.

Individual rules can also be defined using a more `defun`-like
syntax, using the macro `define-peg-rule`:

Macro: **define-peg-rule** name args &rest pexs [¶](#index-define_002dpeg_002drule)
:   Define name as a PEG rule that accepts args and matches
    pexs at point.

For instance:

```
(define-peg-rule digit ()
  [0-9])
```

Arguments can be supplied to rules by the `funcall` PEG rule
(see [PEX Definitions](#PEX-Definitions)).

Another possibility is to define a named set of rules with
`define-peg-ruleset`:

Macro: **define-peg-ruleset** name &rest rules [¶](#index-define_002dpeg_002druleset)
:   Define name as an identifier for rules.

```
(define-peg-ruleset number-grammar
  ;; `digit' here references the definition above.
  (number () sign digit (* digit))
  (sign () (or "+" "-" "")))
```

Rules and rulesets defined this way can be referred to by name in
later calls to `peg-run` or `with-peg-rules`:

```
(with-peg-rules number-grammar
  (peg-run (peg number)))
```

By default, calls to `peg-run` or `peg-parse` produce no
output: parsing simply moves point. In order to return or otherwise
act upon parsed strings, rules can include *actions*, see
[Parsing Actions](#Parsing-Actions).

* [PEX Definitions](#PEX-Definitions)
* [Parsing Actions](#Parsing-Actions)
* [Writing PEG Rules](#Writing-PEG-Rules)

---

Next: [Parsing Actions](#Parsing-Actions), Up: [Parsing Expression Grammars](#Parsing-Expression-Grammars)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 37.1 PEX Definitions [¶](#PEX-Definitions-1)

Parsing expressions can be defined using the following syntax:

`(and e1 e2…)`
:   A sequence of PEXs that must all be matched. The `and`
    form is optional and implicit.

`(or e1 e2…)`
:   Prioritized choices, meaning that, as in Elisp, the choices are tried
    in order, and the first successful match is used. Note that this is
    distinct from context-free grammars, in which selection between
    multiple matches is indeterminate.

`(any)`
:   Matches any single character, as the regexp “.”.

`string`
:   A literal string.

`(char c)`
:   A single character c, as an Elisp character literal.

`(* e)`
:   Zero or more instances of expression e, as the regexp ‘`*`’.
    Matching is always “greedy”.

`(+ e)`
:   One or more instances of expression e, as the regexp ‘`+`’.
    Matching is always “greedy”.

`(opt e)`
:   Zero or one instance of expression e, as the regexp ‘`?`’.

`symbol`
:   A symbol representing a previously-defined PEG rule.

`(range ch1 ch2)`
:   The character range between ch1 and ch2, as the regexp
    ‘`[ch1-ch2]`’.

`[ch1-ch2 "+*" ?x]`
:   A character set, which can include ranges, character literals, or
    strings of characters.

`[ascii cntrl]`
:   A list of named character classes.

`(syntax-class name)`
:   A single syntax class.

`(funcall e args…)`
:   Call PEX e (previously defined with
    `define-peg-rule`) with arguments args.

`(null)`
:   The empty string.

The following expressions are used as anchors or tests – they do not
move point, but return a boolean value which can be used to constrain
matches as a way of controlling the parsing process (see [Writing PEG Rules](#Writing-PEG-Rules)).

`(bob)`
:   Beginning of buffer.

`(eob)`
:   End of buffer.

`(bol)`
:   Beginning of line.

`(eol)`
:   End of line.

`(bow)`
:   Beginning of word.

`(eow)`
:   End of word.

`(bos)`
:   Beginning of symbol.

`(eos)`
:   End of symbol.

`(if e)`
:   Returns non-`nil` if parsing PEX e from point
    succeeds (point is not moved).

`(not e)`
:   Returns non-`nil` if parsing PEX e from point fails
    (point is not moved).

`(guard exp)`
:   Treats the value of the Lisp expression exp as a boolean.

Character-class matching can refer to the classes named in
`peg-char-classes`, equivalent to character classes in regular
expressions (see [Character Classes](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Top))

---

Next: [Writing PEG Rules](#Writing-PEG-Rules), Previous: [PEX Definitions](#PEX-Definitions), Up: [Parsing Expression Grammars](#Parsing-Expression-Grammars)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 37.2 Parsing Actions [¶](#Parsing-Actions-1)

By default the process of parsing simply moves point in the current
buffer, ultimately returning `t` if the parsing succeeds, and
`nil` if it doesn’t. It’s also possible to define *parsing
actions* that can run arbitrary Elisp at certain points in the parsed
text. These actions can optionally affect something called the
*parsing stack*, which is a list of values returned by the parsing
process. These actions only run (and only return values) if the parsing
process ultimately succeeds; if it fails the action code is not run at
all.

Actions can be added anywhere in the definition of a rule. They are
distinguished from parsing expressions by an initial backquote
(‘`` ` ``’), followed by a parenthetical form that must contain a pair
of hyphens (‘`--`’) somewhere within it. Symbols to the left of
the hyphens are bound to values popped from the stack (they are
somewhat analogous to the argument list of a lambda form). Values
produced by code to the right of the hyphens are pushed onto the stack
(analogous to the return value of the lambda). For instance, the
previous grammar can be augmented with actions to return the parsed
number as an actual integer:

```
(with-peg-rules ((number sign digit (* digit
                                       `(a b -- (+ (* a 10) b)))
                         `(sign val -- (* sign val)))
                 (sign (or (and "+" `(-- 1))
                           (and "-" `(-- -1))
                           (and ""  `(-- 1))))
                 (digit [0-9] `(-- (- (char-before) ?0))))
  (peg-run (peg number)))
```

There must be values on the stack before they can be popped and
returned – if there aren’t enough stack values to bind to an action’s
left-hand terms, they will be bound to `nil`. An action with
only right-hand terms will push values to the stack; an action with
only left-hand terms will consume (and discard) values from the stack.
At the end of parsing, stack values are returned as a flat list.

To return the string matched by a PEX (instead of simply
moving point over it), a grammar can use a rule like this:

```
(one-word
  `(-- (point))
  (+ [word])
  `(start -- (buffer-substring start (point))))
```

The first action above pushes the initial value of point to the stack.
The intervening PEX moves point over the next word. The
second action pops the previous value from the stack (binding it to the
variable `start`), then uses that value to extract a substring from
the buffer and push it to the stack. This pattern is so common that
PEG provides a shorthand function that does exactly the above,
along with a few other shorthands for common scenarios:

`(substring e)` [¶](#index-substring-_0028a-PEG-shorthand_0029)
:   Match PEX e and push the matched string onto the stack.

`(region e)` [¶](#index-region-_0028a-PEG-shorthand_0029)
:   Match e and push the start and end positions of the matched
    region onto the stack.

`(replace e replacement)` [¶](#index-replace-_0028a-PEG-shorthand_0029)
:   Match e and replaced the matched region with the string
    replacement.

`(list e)` [¶](#index-list-_0028a-PEG-shorthand_0029)
:   Match e, collect all values produced by e (and its
    sub-expressions) into a list, and push that list onto the stack. Stack
    values are typically returned as a flat list; this is a way of
    “grouping” values together.

---

Previous: [Parsing Actions](#Parsing-Actions), Up: [Parsing Expression Grammars](#Parsing-Expression-Grammars)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 37.3 Writing PEG Rules [¶](#Writing-PEG-Rules-1)

Something to be aware of when writing PEG rules is that they are
greedy. Rules which can consume a variable amount of text will always
consume the maximum amount possible, even if that causes a rule that
might otherwise have matched to fail later on – there is no
backtracking. For instance, this rule will never succeed:

```
(forest (+ "tree" (* [blank])) "tree" (eol))
```

The PEX `(+ "tree" (* [blank]))` will consume all
the repetitions of the word ‘`tree`’, leaving none to match the final
‘`tree`’.

In these situations, the desired result can be obtained by using
predicates and guards – namely the `not`, `if` and
`guard` expressions – to constrain behavior. For instance:

```
(forest (+ "tree" (* [blank])) (not (eol)) "tree" (eol))
```

The `if` and `not` operators accept a parsing expression and
interpret it as a boolean, without moving point. The contents of a
`guard` operator are evaluated as regular Lisp (not a
PEX) and should return a boolean value. A `nil` value
causes the match to fail.

Another potentially unexpected behavior is that parsing will move
point as far as possible, even if the parsing ultimately fails. This
rule:

```
(end-game "game" (eob))
```

when run in a buffer containing the text “game over” after point,
will move point to just after “game” then halt parsing, returning
`nil`. Successful parsing will always return `t`, or the
contexts of the parsing stack.

---

Next: [Abbrevs and Abbrev Expansion](#Abbrevs), Previous: [Parsing Expression Grammars](#Parsing-Expression-Grammars), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 38 Parsing Program Source [¶](#Parsing-Program-Source-1)

Emacs provides various ways to parse program source text and produce a
*syntax tree*. In a syntax tree, text is no longer considered a
one-dimensional stream of characters, but a structured tree of nodes,
where each node represents a piece of text. Thus, a syntax tree can
enable interesting features like precise fontification, indentation,
navigation, structured editing, etc.

Emacs has a simple facility for parsing balanced expressions
(see [Parsing Expressions](#Parsing-Expressions)). There is also the SMIE library for
generic navigation and indentation (see [Simple Minded Indentation Engine](#SMIE)).

In addition to those, Emacs also provides integration with
[the tree-sitter
library](https://tree-sitter.github.io/tree-sitter) if support for it was compiled in. The tree-sitter library
implements an incremental parser and has support for a wide range of
programming languages.

Function: **treesit-available-p** [¶](#index-treesit_002davailable_002dp)
:   This function returns non-`nil` if tree-sitter features are
    available for the current Emacs session.

To be able to parse the program source using the tree-sitter library
and access the syntax tree of the program, a Lisp program needs to
load a language grammar library, and create a parser for that
language and the current buffer. After that, the Lisp program can
query the parser about specific nodes of the syntax tree. Then, it
can access various kinds of information about each node, and search
for nodes using a powerful pattern-matching syntax. This chapter
explains how to do all this, and also how a Lisp program can work with
source files that mix multiple programming languages.

* [Tree-sitter Language Grammar](#Language-Grammar)
* [Using Tree-sitter Parser](#Using-Parser)
* [Retrieving Nodes](#Retrieving-Nodes)
* [Accessing Node Information](#Accessing-Node-Information)
* [Pattern Matching Tree-sitter Nodes](#Pattern-Matching)
* [User-defined “Things” and Navigation](#User_002ddefined-Things)
* [Parsing Text in Multiple Languages](#Multiple-Languages)
* [Developing major modes with tree-sitter](#Tree_002dsitter-Major-Modes)
* [Tree-sitter C API Correspondence](#Tree_002dsitter-C-API)

---

Next: [Using Tree-sitter Parser](#Using-Parser), Up: [Parsing Program Source](#Parsing-Program-Source)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 38.1 Tree-sitter Language Grammar [¶](#Tree_002dsitter-Language-Grammar)

### Loading a language grammar [¶](#Loading-a-language-grammar)

Tree-sitter relies on language grammar to parse text in that
language. In Emacs, a language grammar is represented by a symbol.
For example, the C language grammar is represented as the symbol
`c`, and `c` can be passed to tree-sitter functions as the
language argument.

Tree-sitter language grammars are distributed as dynamic libraries.
In order to use a language grammar in Emacs, you need to make sure
that the dynamic library is installed on the system. Emacs looks for
language grammars in several places, in the following order:

* first, in the list of directories specified by the variable
  `treesit-extra-load-path`;
* then, in the `tree-sitter` subdirectory of the directory
  specified by `user-emacs-directory` (see [The Init File](#Init-File));
* and finally, in the system’s default locations for dynamic libraries.

In each of these directories, Emacs looks for a file with file-name
extensions specified by the variable `dynamic-library-suffixes`.

If Emacs cannot find the library or has problems loading it, Emacs
signals the `treesit-load-language-error` error. The data of
that signal could be one of the following:

`(not-found error-msg …)`
:   This means that Emacs could not find the language grammar library.

`(symbol-error error-msg)`
:   This means that Emacs could not find in the library the expected function
    that every language grammar library should export.

`(version-mismatch error-msg)`
:   This means that the version of the language grammar library is
    incompatible with that of the tree-sitter library.

In all of these cases, error-msg might provide additional
details about the failure.

Function: **treesit-language-available-p** language &optional detail [¶](#index-treesit_002dlanguage_002davailable_002dp)
:   This function returns non-`nil` if the language grammar for
    language exists and can be loaded.

    If detail is non-`nil`, return `(t . nil)` when
    language is available, and `(nil . data)` when it’s
    unavailable. data is the signal data of
    `treesit-load-language-error`.

By convention, the file name of the dynamic library for language is
`libtree-sitter-language.ext`, where ext is the
system-specific extension for dynamic libraries. Also by convention,
the function provided by that library is named
`tree_sitter_language`. If a language grammar library
doesn’t follow this convention, you should add an entry

```
(language library-base-name function-name)
```

to the list in the variable `treesit-load-name-override-list`, where
library-base-name is the basename of the dynamic library’s file name
(usually, `libtree-sitter-language`), and
function-name is the function provided by the library
(usually, `tree_sitter_language`). For example,

```
(cool-lang "libtree-sitter-coool" "tree_sitter_cooool")
```

for a language that considers itself too “cool” to abide by
conventions.

Function: **treesit-library-abi-version** &optional min-compatible [¶](#index-treesit_002dlibrary_002dabi_002dversion)
:   This function returns the version of the language grammar
    Application Binary Interface (ABI) supported by the
    tree-sitter library. By default, it returns the latest ABI version
    supported by the library, but if min-compatible is
    non-`nil`, it returns the oldest ABI version which the library
    still can support. Language grammar libraries must be built for
    ABI versions between the oldest and the latest versions supported by
    the tree-sitter library, otherwise the library will be unable to load
    them.

Function: **treesit-language-abi-version** language [¶](#index-treesit_002dlanguage_002dabi_002dversion)
:   This function returns the ABI version of the language
    grammar library loaded by Emacs for language. If language
    is unavailable, this function returns `nil`.

Function: **treesit-language-display-name** language [¶](#index-treesit_002dlanguage_002ddisplay_002dname)
:   This function translates language to an appropriate display name.
    For example, it translates `ruby` to “Ruby”, `cpp` to
    “C++”.

    Most languages has “regular” names, and their display name is simply
    the symbol name with first letter capitalized. For languages that has
    “irregular” names, treesit-language-display-name-alist maps
    language symbols to their display names.

    If a major mode package uses a language with “irregular” name, they
    should add a mapping into treesit-language-display-name-alist on
    load.

### Concrete syntax tree [¶](#Concrete-syntax-tree)

A syntax tree is what a parser generates. In a syntax tree, each node
represents a piece of text, and is connected to each other by a
parent-child relationship. For example, if the source text is

```
1 + 2
```

its syntax tree could be

```
                  +--------------+
                  | root "1 + 2" |
                  +--------------+
                         |
        +--------------------------------+
        |       expression "1 + 2"       |
        +--------------------------------+
           |             |            |
+------------+   +--------------+   +------------+
| number "1" |   | operator "+" |   | number "2" |
+------------+   +--------------+   +------------+
```

We can also represent it as an s-expression:

```
(root (expression (number) (operator) (number)))
```

#### Node types [¶](#Node-types)

Names like `root`, `expression`, `number`, and
`operator` specify the *type* of the nodes. However, not all
nodes in a syntax tree have a type. Nodes that don’t have a type are
known as *anonymous nodes*, and nodes with a type are *named
nodes*. Anonymous nodes are tokens with fixed spellings, including
punctuation characters like bracket ‘`]`’, and keywords like
`return`.

#### Field names [¶](#Field-names)

To make the syntax tree easier to analyze, many language grammar
assign *field names* to child nodes. For example, a
`function_definition` node could have a `declarator` and a
`body`:

```
(function_definition
 declarator: (declaration)
 body: (compound_statement))
```

### Exploring the syntax tree [¶](#Exploring-the-syntax-tree)

To aid in understanding the syntax of a language and in debugging Lisp
programs that use the syntax tree, Emacs provides an “explore” mode,
which displays the syntax tree of the source in the current buffer in
real time. Emacs also comes with an “inspect mode”, which displays
information of the nodes at point in the mode-line.

Command: **treesit-explore** [¶](#index-treesit_002dexplore)
:   This command pops up a window displaying the syntax tree of the source in
    the current buffer. Selecting text in the source buffer highlights
    the corresponding nodes in the syntax tree display. Clicking
    on nodes in the syntax tree highlights the corresponding text in the
    source buffer.

    To switch to another parser, use `treesit-explorer-switch-parser`.

Command: **treesit-inspect-mode** [¶](#index-treesit_002dinspect_002dmode)
:   This minor mode displays on the mode-line the node that *starts*
    at point. For example, the mode-line can display

    ```
    parent field: (node (child (...)))
    ```

    where node, child, etc., are nodes which begin at point.
    parent is the parent of node. node is displayed in
    a bold typeface. field-names are field names of node and
    of child, etc.

    If no node starts at point, i.e., point is in the middle of a node,
    then the mode line displays the earliest node that spans point, and
    its immediate parent.

    This minor mode doesn’t create parsers on its own. It uses the first
    parser in `(treesit-parser-list)` (see [Using Tree-sitter Parser](#Using-Parser)).

### Reading the grammar definition [¶](#Reading-the-grammar-definition)

Authors of language grammars define the *grammar* of a
programming language, which determines how a parser constructs a
concrete syntax tree out of the program text. In order to use the
syntax tree effectively, you need to consult the *grammar file*.

The grammar file is usually `grammar.js` in a language
grammar’s project repository. The link to a language grammar’s
home page can be found on
[tree-sitter’s
homepage](https://tree-sitter.github.io/tree-sitter).

The grammar definition is written in JavaScript. For example, the
rule matching a `function_definition` node may look like

```
function_definition: $ => seq(
  $.declaration_specifiers,
  field('declarator', $.declaration),
  field('body', $.compound_statement)
)
```

The rules are represented by functions that take a single argument
$, representing the whole grammar. The function itself is
constructed by other functions: the `seq` function puts together
a sequence of children; the `field` function annotates a child
with a field name. If we write the above definition in the so-called
*Backus-Naur Form* (BNF) syntax, it would look like

```
function_definition :=
  <declaration_specifiers> <declaration> <compound_statement>
```

and the node returned by the parser would look like

```
(function_definition
  (declaration_specifier)
  declarator: (declaration)
  body: (compound_statement))
```

Below is a list of functions that one can see in a grammar definition.
Each function takes other rules as arguments and returns a new rule.

`seq(rule1, rule2, …)`
:   matches each rule one after another.

`choice(rule1, rule2, …)`
:   matches one of the rules in its arguments.

`repeat(rule)`
:   matches rule *zero or more* times.
    This is like the ‘`*`’ operator in regular expressions.

`repeat1(rule)`
:   matches rule *one or more* times.
    This is like the ‘`+`’ operator in regular expressions.

`optional(rule)`
:   matches rule *zero or one* times.
    This is like the ‘`?`’ operator in regular expressions.

`field(name, rule)`
:   assigns field name name to the child node matched by rule.

`alias(rule, alias)`
:   makes nodes matched by rule appear as alias in the syntax
    tree generated by the parser. For example,

    ```
    alias(preprocessor_call_exp, call_expression)
    ```

    makes any node matched by `preprocessor_call_exp` appear as
    `call_expression`.

Below are grammar functions of lesser importance for reading a
language grammar.

`token(rule)`
:   marks rule to produce a single leaf node. That is, instead of
    generating a parent node with individual child nodes under it,
    everything is combined into a single leaf node. See [Retrieving Nodes](#Retrieving-Nodes).

`token.immediate(rule)`
:   Normally, grammar rules ignore preceding whitespace; this
    changes rule to match only when there is no preceding
    whitespace.

`prec(n, rule)`
:   gives rule the level-n precedence.

`prec.left([n,] rule)`
:   marks rule as left-associative, optionally with level n.

`prec.right([n,] rule)`
:   marks rule as right-associative, optionally with level n.

`prec.dynamic(n, rule)`
:   this is like `prec`, but the precedence is applied at runtime
    instead.

The documentation of the tree-sitter project has
[more
about writing a grammar](https://tree-sitter.github.io/tree-sitter/creating-parsers). Read especially “The Grammar DSL”
section.

---

Next: [Retrieving Nodes](#Retrieving-Nodes), Previous: [Tree-sitter Language Grammar](#Language-Grammar), Up: [Parsing Program Source](#Parsing-Program-Source)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 38.2 Using Tree-sitter Parser [¶](#Using-Tree_002dsitter-Parser)

This section describes how to create and configure a tree-sitter
parser. In Emacs, each tree-sitter parser is associated with a
buffer. As the user edits the buffer, the associated parser and
syntax tree are automatically kept up-to-date.

Variable: **treesit-max-buffer-size** [¶](#index-treesit_002dmax_002dbuffer_002dsize)
:   This variable contains the maximum size of buffers in which
    tree-sitter can be activated. Major modes should check this value
    when deciding whether to enable tree-sitter features.

Variable: **treesit-languages-require-line-column-tracking** [¶](#index-treesit_002dlanguages_002drequire_002dline_002dcolumn_002dtracking)
:   Emacs by default doesn’t keep track of line and column numbers for
    positions in a buffer. However, some language grammars utilize the line
    and column information for parsing. If parsers of these languages are
    created in a buffer, Emacs will turn on line and column tracking and
    report these information to these parsers. Once the buffer starts
    tracking line and column, it never stops doing so. And once a parser is
    created as tracking/not-tracking line and column, it stays that way
    regardless of changes to this variable.

    This variable is a list of languages that require line and column
    tracking. The vast majority of languages don’t need line and column
    information. So far, only Haskell is known to need it.

    User can use `treesit-tracking-line-column-p` and
    `treesit-parser-tracking-line-column-p` to check if a buffer or
    parser is tracking line and column, respectively.

Function: **treesit-parser-create** language &optional buffer no-reuse tag [¶](#index-treesit_002dparser_002dcreate)
:   Create a parser for the specified buffer and language
    (see [Tree-sitter Language Grammar](#Language-Grammar)), with tag. If buffer is
    omitted or `nil`, it stands for the current buffer.

    By default, this function reuses a parser if one already exists for
    language with tag in buffer, but if no-reuse
    is non-`nil`, this function always creates a new parser.

    tag can be any symbol except `t`, and defaults to
    `nil`. Different parsers can have the same tag.

Given a parser, we can query information about it:
`treesit-parser-buffer` returns the buffer associated with a
parser, and `treesit-parser-language` returns the language used by
the parser. A parser’s tag can be retrieved by
`treesit-parser-tag`. Each parser also optionally carries an embed
level that can be set by `treesit-parser-set-embed-level` and
retrieved by `treesit-parser-embed-level` (see
[parser-embed-level](#parser_002dembed_002dlevel), for more about embed level). Finally,
`treesit-parser-p` tells you whether an object is a tree-sitter
parser.

There is no need to explicitly parse a buffer, because parsing is done
automatically and lazily. A parser only parses when a Lisp program
queries for a node in its syntax tree. Therefore, when a parser is
first created, it doesn’t parse the buffer; it waits until the Lisp
program queries for a node for the first time. Similarly, when some
change is made in the buffer, a parser doesn’t re-parse immediately.

When a parser does parse, it checks for the size of the buffer.
Tree-sitter can only handle buffers no larger than about 4GB. If the
size exceeds that, Emacs signals the `treesit-buffer-too-large`
error with signal data being the buffer size.

Once a parser is created, Emacs automatically adds it to the
internal parser list. Every time a change is made to the buffer,
Emacs updates parsers in this list so they can update their syntax
tree incrementally.

Function: **treesit-parser-list** &optional buffer language tag [¶](#index-treesit_002dparser_002dlist)
:   This function returns the parser list of buffer, filtered by
    language and tag. If buffer is `nil` or
    omitted, it defaults to the current buffer.

    If language is non-nil, only include parsers for that
    language, and only include parsers with tag. tag defaults
    to `nil`. If tag is `t`, include parsers in the
    returned list regardless of their tag.

Function: **treesit-parser-delete** parser [¶](#index-treesit_002dparser_002ddelete)
:   This function deletes parser.

Normally, a parser “sees” the whole buffer, but when the buffer is
narrowed (see [Narrowing](#Narrowing)), the parser will only see the accessible
portion of the buffer. As far as the parser can tell, the hidden
region was deleted. When the buffer is later widened, the parser
thinks text is inserted at the beginning and at the end. Although
parsers respect narrowing, modes should not use narrowing as a means
to handle a multi-language buffer; instead, set the ranges in which the
parser should operate. See [Parsing Text in Multiple Languages](#Multiple-Languages).

Because a parser parses lazily, when the user or a Lisp program
narrows the buffer, the parser is not affected immediately; as long as
the mode doesn’t query for a node while the buffer is narrowed, the
parser is oblivious of the narrowing.

Besides creating a parser for a buffer, a Lisp program can also parse a
string. Unlike a buffer, parsing a string is a one-off operation, and
there is no way to update the result.

Function: **treesit-parse-string** string language [¶](#index-treesit_002dparse_002dstring)
:   This function parses string using language, and returns the
    root node of the generated syntax tree. *Do not* use this function
    in a loop: this is a convenience function intended for one-off use, and
    it isn’t optimized; for heavy workload, use a temporary buffer instead.

### Be notified by changes to the parse tree [¶](#Be-notified-by-changes-to-the-parse-tree)

A Lisp program might want to be notified of text affected by
incremental parsing. For example, inserting a comment-closing token
converts text before that token into a comment. Even
though the text is not directly edited, it is deemed to be “changed”
nevertheless.

Emacs lets a Lisp program register callback functions (a.k.a.
*notifiers*) for these kinds of changes. A notifier function
takes two arguments: ranges and parser. ranges is a
list of cons cells of the form `(start . end)`,
where start and end mark the start and the end positions
of a range. parser is the parser issuing the notification.

Every time a parser reparses a buffer, it compares the old and new
parse-tree, computes the ranges in which nodes have changed, and
passes the ranges to notifier functions. Note that the initial parse
is also considered a “change”, so notifier functions are called on
the initial parse, with range being the whole buffer.

Function: **treesit-parser-add-notifier** parser function [¶](#index-treesit_002dparser_002dadd_002dnotifier)
:   This function adds function to parser’s list of
    after-change notifier functions. function must be a function
    symbol, not a lambda function (see [Anonymous Functions](#Anonymous-Functions)).

Function: **treesit-parser-remove-notifier** parser function [¶](#index-treesit_002dparser_002dremove_002dnotifier)
:   This function removes function from the list of parser’s
    after-change notifier functions. function must be a function
    symbol, rather than a lambda function.

Function: **treesit-parser-notifiers** parser [¶](#index-treesit_002dparser_002dnotifiers)
:   This function returns the list of parser’s notifier functions.

A lisp program can also choose to force a parser to reparse and get the
changed regions immediately with `treesit-parser-changed-regions`.

Function: **treesit-parser-changed-regions** parser [¶](#index-treesit_002dparser_002dchanged_002dregions)
:   This function forces parser to reparse, and returns the affected
    regions: a list of `(start . end)`. If the parser
    has nothing new to reparse, or the affected regions are empty, this
    function returns `nil`.

### Substitute parser for another language [¶](#Substitute-parser-for-another-language)

Sometimes, a grammar for language B is a strict superset of the grammar
of another language A. Then it makes sense to reuse configurations
(font-lock rules, indentation rules, etc.) of language A for language B.
For that purpose, treesit-language-remap-alist allows users to
remap language A into language B.

Variable: **treesit-language-remap-alist** [¶](#index-treesit_002dlanguage_002dremap_002dalist)
:   The value of this variable should be an alist of
    `(language-a . language-b)`. When such pair exists
    in the alist, creating a parser for language-a actually creates a
    parser for language-b. By extension, anything that creates a node
    or makes a query of language-a will be redirected to use
    language-b instead. This mapping is completely transparent, the
    created parser will reported to use language-b, and the same goes for
    nodes created by this parser.

    Specifically, the parser created by `treesit-parser-create` will
    report to use whatever language was given to it. For example,
    if language `cpp` is mapped to `cuda`:

    ```
    (setq treesit-language-remap-alist '((cpp . cuda)))

    (treesit-parser-language (treesit-parser-create 'cpp))
      ⇒ 'cpp

    (treesit-parser-language (treesit-parser-create 'cuda))
      ⇒ 'cuda
    ```

    Even though both parser are actually `cuda` parser.

---

Next: [Accessing Node Information](#Accessing-Node-Information), Previous: [Using Tree-sitter Parser](#Using-Parser), Up: [Parsing Program Source](#Parsing-Program-Source)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 38.3 Retrieving Nodes [¶](#Retrieving-Nodes-1)

Here are some terms and conventions we use when documenting
tree-sitter functions.

A node in a syntax tree spans some portion of the program text in the
buffer. We say that a node is “smaller” or “larger” than another
if it spans, respectively, a smaller or larger portion of buffer text
than the other node. Since nodes that are deeper (“lower”) in the
tree are children of the nodes that are “higher” in the tree, it
follows that a lower node will always be smaller than a node that is
higher in the node hierarchy. A node that is higher up in the syntax
tree contains one or more smaller nodes as its children, and therefore
spans a larger portion of buffer text.

When a function cannot find a node, it returns `nil`. For
convenience, all functions that take a node as argument and return
a node, also accept the node argument of `nil` and in that case
just return `nil`.

Nodes are not automatically updated when the associated buffer is
modified, and there is no way to update a node once it is retrieved.
Using an outdated node signals the `treesit-node-outdated` error.

The printed representation of a tree-sitter node uses the hash notation
described in [Printed Representation and Read Syntax](#Printed-Representation). It looks like
‘`#<treesit-node type in pos1-pos2>`’, where
type is the type of the node (which comes from the tree-sitter
grammar used by the buffer), and pos1 and pos2 are buffer
positions of the node’s span. Tree-sitter nodes have no read syntax.

### Retrieving nodes from syntax tree [¶](#Retrieving-nodes-from-syntax-tree)

Function: **treesit-node-at** pos &optional parser-or-lang named [¶](#index-treesit_002dnode_002dat)
:   This function returns a *leaf* node at buffer position pos.
    A leaf node is a node that doesn’t have any child nodes.

    This function tries to return a node whose span covers pos: the
    node’s beginning position is less than or equal to pos, and the
    node’s end position is greater than or equal to pos.

    If no leaf node’s span covers pos (e.g., pos is in the
    whitespace between two leaf nodes), this function returns the first
    leaf node after pos.

    Finally, if there is no leaf node after pos, return the first
    leaf node before pos.

    If parser-or-lang is a parser object, this function uses that
    parser; if parser-or-lang is a language, this function uses the
    first parser for that language in the current buffer, or creates one
    if none exists; if parser-or-lang is `nil`, this function
    tries to guess the language at pos by calling
    `treesit-language-at` (see [Parsing Text in Multiple Languages](#Multiple-Languages)).

    If this function cannot find a suitable node to return, it returns
    `nil`.

    If named is non-`nil`, this function looks only for named
    nodes (see [named node](#tree_002dsitter-named-node)).

    Example:

    ```
    ;; Find the node at point in a C parser's syntax tree.
    (treesit-node-at (point) 'c)
      ⇒ #<treesit-node primitive_type in 23-27>
    ```

Function: **treesit-node-on** beg end &optional parser-or-lang named [¶](#index-treesit_002dnode_002don)
:   This function returns the *smallest* node that covers the region
    of buffer text between beg and end. In other words, the
    start of the node is before or at beg, and the end of the node
    is at or after end.

    *Beware:* calling this function on an empty line that is not
    inside any top-level construct (function definition, etc.) most
    probably will give you the root node, because the root node is the
    smallest node that covers that empty line. Most of the time, you want
    to use `treesit-node-at` instead.

    If parser-or-lang is a parser object, this function uses that
    parser; if parser-or-lang is a language, this function uses the
    first parser for that language in the current buffer, or creates one
    if none exists; if parser-or-lang is `nil`, this function
    tries to guess the language at beg by calling
    `treesit-language-at`.

    If named is non-`nil`, this function looks for a named node
    only (see [named node](#tree_002dsitter-named-node)).

Function: **treesit-parser-root-node** parser [¶](#index-treesit_002dparser_002droot_002dnode)
:   This function returns the root node of the syntax tree generated by
    parser.

Function: **treesit-buffer-root-node** &optional language [¶](#index-treesit_002dbuffer_002droot_002dnode)
:   This function finds the first parser for language in the current
    buffer, or creates one if none exists, and returns the root node
    generated by that parser. If language is omitted, it uses the
    first parser in the parser list. If it cannot find an appropriate
    parser, it returns `nil`.

Given a node, a Lisp program can retrieve other nodes starting from
it, or query for information about this node.

### Retrieving nodes from other nodes [¶](#Retrieving-nodes-from-other-nodes)

#### By kinship [¶](#By-kinship)

Function: **treesit-node-parent** node [¶](#index-treesit_002dnode_002dparent)
:   This function returns the immediate parent of node.

    If node is more than 1000 levels deep in a parse tree, the
    return value is undefined. Currently it returns `nil`, but that
    could change in the future.

Function: **treesit-node-child** node n &optional named [¶](#index-treesit_002dnode_002dchild)
:   This function returns the n’th child of node. If
    named is non-`nil`, it counts only named nodes
    (see [named node](#tree_002dsitter-named-node)).

    For example, in a node that represents a string `"text"`, there
    are three children nodes: the opening quote `"`, the string text
    `text`, and the closing quote `"`. Among these nodes, the
    first child is the opening quote `"`, and the first named child
    is the string text.

    This function returns `nil` if there is no n’th child.
    n could be negative, e.g., −1 represents the last child.

Function: **treesit-node-children** node &optional named [¶](#index-treesit_002dnode_002dchildren)
:   This function returns all of node’s children as a list. If
    named is non-`nil`, it retrieves only named nodes.

Function: **treesit-node-next-sibling** node &optional named [¶](#index-treesit_002dnode_002dnext_002dsibling)
:   This function finds the next sibling of node. If named is
    non-`nil`, it finds the next named sibling.

Function: **treesit-node-prev-sibling** node &optional named [¶](#index-treesit_002dnode_002dprev_002dsibling)
:   This function finds the previous sibling of node. If
    named is non-`nil`, it finds the previous named sibling.

#### By field name [¶](#By-field-name)

To make the syntax tree easier to analyze, many language grammars
assign *field names* to child nodes (see [field name](#tree_002dsitter-node-field-name)). For example, a `function_definition` node
could have a `declarator` child and a `body` child.

Function: **treesit-node-child-by-field-name** node field-name [¶](#index-treesit_002dnode_002dchild_002dby_002dfield_002dname)
:   This function finds the child of node whose field name is
    field-name, a string.

    ```
    ;; Get the child that has "body" as its field name.
    (treesit-node-child-by-field-name node "body")
      ⇒ #<treesit-node compound_statement in 45-89>
    ```

#### By position [¶](#By-position)

Function: **treesit-node-first-child-for-pos** node pos &optional named [¶](#index-treesit_002dnode_002dfirst_002dchild_002dfor_002dpos)
:   This function finds the first child of node that extends beyond
    buffer position pos. “Extends beyond” means the end of the
    child node is greater or equal to pos. This function only looks
    for immediate children of node, and doesn’t look in its
    grandchildren. If named is non-`nil`, it looks for the
    first named child (see [named node](#tree_002dsitter-named-node)).

Function: **treesit-node-descendant-for-range** node beg end &optional named [¶](#index-treesit_002dnode_002ddescendant_002dfor_002drange)
:   This function finds the *smallest* descendant node of node
    that spans the region of text between positions beg and
    end. It is similar to `treesit-node-at`. If named
    is non-`nil`, it looks for the smallest named child.

### Searching for node [¶](#Searching-for-node)

Function: **treesit-search-subtree** node predicate &optional backward all depth [¶](#index-treesit_002dsearch_002dsubtree)
:   This function traverses the subtree of node (including node
    itself), looking for a node for which predicate returns
    non-`nil`. predicate is a regexp that is matched against
    each node’s type, or a predicate function that takes a node and returns
    non-`nil` if the node matches. predicate can also be a thing
    symbol or thing definition (see [User-defined “Things” and Navigation](#User_002ddefined-Things)). Using an
    undefined thing doesn’t raise an error, the function simply returns
    `nil`.

    This function returns the first node that matches, or `nil` if none
    matches predicate.

    By default, this function only traverses named nodes, but if all
    is non-`nil`, it traverses all the nodes. If backward is
    non-`nil`, it traverses backwards (i.e., it visits the last child
    first when traversing down the tree). If depth is
    non-`nil`, it must be a number that limits the tree traversal to
    that many levels down the tree. If depth is `nil`, it
    defaults to 1000.

Function: **treesit-search-forward** start predicate &optional backward all [¶](#index-treesit_002dsearch_002dforward)
:   Like `treesit-search-subtree`, this function also traverses the
    parse tree and matches each node with predicate (except for
    start), where predicate can be a regexp or a predicate
    function. predicate can also be a thing symbol or thing
    definition (see [User-defined “Things” and Navigation](#User_002ddefined-Things)). Using an undefined thing
    doesn’t raise an error, the function simply returns `nil`.

    For a tree like the one below where start is marked ‘`S`’, this
    function traverses as numbered from 1 to 12:

    ```
                  12
                  |
         S--------3----------11
         |        |          |
    o--o-+--o  1--+--2    6--+-----10
    |  |                  |        |
    o  o                +-+-+   +--+--+
                        |   |   |  |  |
                        4   5   7  8  9
    ```

    Note that this function doesn’t traverse the subtree of start,
    and it always traverses leaf nodes first, before moving upwards.

    Like `treesit-search-subtree`, this function only searches for
    named nodes by default, but if all is non-`nil`, it
    searches for all nodes. If backward is non-`nil`, it
    searches backwards.

    While `treesit-search-subtree` traverses the subtree of a node,
    this function starts with node start and traverses every node
    that comes after it in the buffer position order, i.e., nodes with
    start positions greater than the end position of start.

    In the tree shown above, `treesit-search-subtree` traverses node
    ‘`S`’ (start) and nodes marked with `o`, whereas this
    function traverses the nodes marked with numbers. This function is
    useful for answering questions like “what is the first node after
    start in the buffer that satisfies some condition?”

Function: **treesit-search-forward-goto** node predicate &optional start backward all [¶](#index-treesit_002dsearch_002dforward_002dgoto)
:   This function moves point to the start or end of the next node after
    node in the buffer that matches predicate. If start
    is non-`nil`, stop at the beginning rather than the end of a node.

    This function guarantees that the matched node it returns makes
    progress in terms of buffer position: the start/end position of the
    returned node is always greater than that of node.

    Arguments predicate, backward, and all are the same
    as in `treesit-search-forward`.

Function: **treesit-induce-sparse-tree** root predicate &optional process-fn depth [¶](#index-treesit_002dinduce_002dsparse_002dtree)
:   This function creates a sparse tree from root’s subtree.

    It takes the subtree under root, and combs it so only the nodes
    that match predicate are left. Like previous functions, the
    predicate can be a regexp string that matches against each node’s
    type, or a function that takes a node and returns non-`nil` if it
    matches. predicate can also be a thing symbol or thing definition
    (see [User-defined “Things” and Navigation](#User_002ddefined-Things)). Using an undefined thing doesn’t raise
    an error, the function simply returns `nil`.

    For example, given the subtree on the left that consists of both
    numbers and letters, if predicate is “letter only”, the
    returned tree is the one on the right.

    ```
        a                 a              a
        |                 |              |
    +---+---+         +---+---+      +---+---+
    |   |   |         |   |   |      |   |   |
    b   1   2         b   |   |      b   c   d
        |   |     =>      |   |  =>      |
        c   +--+          c   +          e
        |   |  |          |   |
     +--+   d  4       +--+   d
     |  |              |
     e  5              e
    ```

    If process-fn is non-`nil`, instead of returning the
    matched nodes, this function passes each node to process-fn and
    uses the returned value instead. If non-`nil`, depth
    limits the number of levels to go down from root. If
    depth is `nil`, it defaults to 1000.

    Each node in the returned tree looks like
    `(tree-sitter-node . (child …))`. The
    tree-sitter-node of the root of this tree will be `nil` if
    root doesn’t match predicate. If no node matches
    predicate, the function returns `nil`.

### More convenience functions [¶](#More-convenience-functions)

Function: **treesit-node-get** node instructions [¶](#index-treesit_002dnode_002dget)
:   This is a convenience function that chains together multiple node
    accessor functions together. For example, to get node’s
    parent’s next sibling’s second child’s text:

    ```
    (treesit-node-get node
       '((parent 1)
        (sibling 1 nil)
        (child 1 nil)
        (text nil)))
    ```

    instruction is a list of INSTRUCTIONs of the form
    `(fn arg...)`. The following fn’s are
    supported:

    `(child idx named)`
    :   Get the idx’th child.

    `(parent n)`
    :   Go to parent n times.

    `(field-name)`
    :   Get the field name of the current node.

    `(type)`
    :   Get the type of the current node.

    `(text no-property)`
    :   Get the text of the current node.

    `(children named)`
    :   Get a list of children.

    `(sibling step named)`
    :   Get the nth prev/next sibling, negative step means prev sibling,
        positive means next sibling.

    Note that arguments like named and no-property can’t be
    omitted, unlike in their original functions.

Function: **treesit-filter-child** node predicate &optional named [¶](#index-treesit_002dfilter_002dchild)
:   This function finds immediate children of node that satisfy
    predicate.

    The predicate function takes a node as argument and should
    return non-`nil` to indicate that the node should be kept. If
    named is non-`nil`, this function only examines named
    nodes.

Function: **treesit-parent-until** node predicate &optional include-node [¶](#index-treesit_002dparent_002duntil)
:   This function repeatedly finds the parents of node, and returns
    the parent that satisfies predicate. predicate can be
    either a function that takes a node as argument and returns `t`
    or `nil`, or a regexp matching node type names, or other valid
    predicates described in `treesit-thing-settings`. If no parent
    satisfies predicates, this function returns `nil`.

    Normally this function only looks at the parents of node but not
    node itself. But if include-node is non-`nil`, this
    function returns node if node satisfies predicate.

Function: **treesit-parent-while** node predicate [¶](#index-treesit_002dparent_002dwhile)
:   This function goes up the tree starting from node, and keeps
    doing so as long as the nodes satisfy predicate, a function that
    takes a node as argument. That is, this function returns the highest
    parent of node that still satisfies predicate. Note that if
    node satisfies predicate but its immediate parent doesn’t,
    node itself is returned.

Function: **treesit-node-top-level** node &optional predicate include-node [¶](#index-treesit_002dnode_002dtop_002dlevel)
:   This function returns the highest parent of node that has the
    same type as node. If no such parent exists, it returns
    `nil`. Therefore this function is also useful for testing
    whether node is top-level.

    If predicate is `nil`, this function uses node’s type
    to find the parent. If predicate is non-`nil`, this
    function searches the parent that satisfies predicate. If
    include-node is non-`nil`, this function returns node
    if node satisfies predicate.

---

Next: [Pattern Matching Tree-sitter Nodes](#Pattern-Matching), Previous: [Retrieving Nodes](#Retrieving-Nodes), Up: [Parsing Program Source](#Parsing-Program-Source)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 38.4 Accessing Node Information [¶](#Accessing-Node-Information-1)

### Basic information of Node [¶](#Basic-information-of-Node)

Every node is associated with a parser, and that parser is associated
with a buffer. The following functions retrieve them.

Function: **treesit-node-parser** node [¶](#index-treesit_002dnode_002dparser)
:   This function returns node’s associated parser.

Function: **treesit-node-buffer** node [¶](#index-treesit_002dnode_002dbuffer)
:   This function returns node’s parser’s associated buffer.

Function: **treesit-node-language** node [¶](#index-treesit_002dnode_002dlanguage)
:   This function returns node’s parser’s associated language.

Each node represents a portion of text in the buffer. Functions below
find relevant information about that text.

Function: **treesit-node-start** node [¶](#index-treesit_002dnode_002dstart)
:   Return the start position of node.

Function: **treesit-node-end** node [¶](#index-treesit_002dnode_002dend)
:   Return the end position of node.

Function: **treesit-node-text** node &optional object [¶](#index-treesit_002dnode_002dtext)
:   Return the buffer text that node represents, as a string. (If
    node is retrieved from parsing a string, it will be the text
    from that string.)

Here are some predicates on tree-sitter nodes:

Function: **treesit-node-p** object [¶](#index-treesit_002dnode_002dp)
:   Checks if object is a tree-sitter syntax node.

Function: **treesit-node-eq** node1 node2 [¶](#index-treesit_002dnode_002deq)
:   Checks if node1 and node2 refer to the same node in a
    tree-sitter syntax tree. This function uses the same equivalence
    metric as `equal`. You can also compare nodes using `equal`
    (see [Equality Predicates](#Equality-Predicates)).

### Property information [¶](#Property-information)

In general, nodes in a concrete syntax tree fall into two categories:
*named nodes* and *anonymous nodes*. Whether a node is named
or anonymous is determined by the language grammar
(see [named node](#tree_002dsitter-named-node)).

Apart from being named or anonymous, a node can have other properties.
A node can be “missing”: such nodes are inserted by the parser in
order to recover from certain kinds of syntax errors, i.e., something
should probably be there according to the grammar, but is not there.
This can happen during editing of the program source, when the source
is not yet in its final form.

A node can be “extra”: such nodes represent things like comments,
which can appear anywhere in the text.

A node can be “outdated”, if its parser has reparsed at least once
after the node was created.

A node “has error” if the text it spans contains a syntax error. It
can be that the node itself has an error, or one of its descendants
has an error.

A node is considered *live* if its parser is not deleted, and the
buffer to which it belongs is a live buffer (see [Killing Buffers](#Killing-Buffers)).

Function: **treesit-node-check** node property [¶](#index-treesit_002dnode_002dcheck)
:   This function returns non-`nil` if node has the specified
    property. property can be `named`, `missing`,
    `extra`, `outdated`, `has-error`, or `live`.

Function: **treesit-node-type** node [¶](#index-treesit_002dnode_002dtype)
:   Named nodes have “types” (see [node type](#tree_002dsitter-node-type)).
    For example, a named node can be a `string_literal` node, where
    `string_literal` is its type. The type of an anonymous node is
    just the text that the node represents; e.g., the type of a ‘`,`’
    node is just ‘`,`’.

    This function returns node’s type as a string.

### Information as a child or parent [¶](#Information-as-a-child-or-parent)

Function: **treesit-node-index** node &optional named [¶](#index-treesit_002dnode_002dindex)
:   This function returns the index of node as a child node of its
    parent. If named is non-`nil`, it only counts named nodes
    (see [named node](#tree_002dsitter-named-node)).

Function: **treesit-node-field-name** node [¶](#index-treesit_002dnode_002dfield_002dname)
:   A child of a parent node could have a field name (see [field name](#tree_002dsitter-node-field-name)). This function returns the field name
    of node as a child of its parent.

Function: **treesit-node-field-name-for-child** node n [¶](#index-treesit_002dnode_002dfield_002dname_002dfor_002dchild)
:   This function returns the field name of the n’th child of
    node. It returns `nil` if there is no n’th child, or
    the n’th child doesn’t have a field name.

    Note that n counts both named and anonymous children, and
    n can be negative, e.g., −1 represents the last child.

Function: **treesit-node-child-count** node &optional named [¶](#index-treesit_002dnode_002dchild_002dcount)
:   This function returns the number of children of node. If
    named is non-`nil`, it only counts named children
    (see [named node](#tree_002dsitter-named-node)).

### Convenience functions [¶](#Convenience-functions)

Function: **treesit-node-enclosed-p** smaller larger &optional strict [¶](#index-treesit_002dnode_002denclosed_002dp)
:   This function returns non-`nil` if smaller is enclosed in
    larger. smaller and larger can be either a cons
    `(beg . end)` or a node.

    Return non-`nil` if larger’s start <= smaller’s start
    and larger’s end <= smaller’s end.

    If strict is `t`, compare with < rather than <=.

    If strict is `partial`, consider larger encloses
    smaller when at least one side is strictly enclosing.

---

Next: [User-defined “Things” and Navigation](#User_002ddefined-Things), Previous: [Accessing Node Information](#Accessing-Node-Information), Up: [Parsing Program Source](#Parsing-Program-Source)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 38.5 Pattern Matching Tree-sitter Nodes [¶](#Pattern-Matching-Tree_002dsitter-Nodes)

Tree-sitter lets Lisp programs match patterns using a small
declarative language. This pattern matching consists of two steps:
first tree-sitter matches a *pattern* against nodes in the syntax
tree, then it *captures* specific nodes that matched the pattern
and returns the captured nodes.

We describe first how to write the most basic query pattern and how to
capture nodes in a pattern, then the pattern-matching function, and
finally the more advanced pattern syntax.

### Basic query syntax [¶](#Basic-query-syntax)

A *query* consists of multiple *patterns*. Each pattern is an
s-expression that matches a certain node in the syntax node. A
pattern has the form `(type (child…))`.

For example, a pattern that matches a `binary_expression` node that
contains `number_literal` child nodes would look like

```
(binary_expression (number_literal))
```

To *capture* a node using the query pattern above, append
`@capture-name` after the node pattern you want to
capture. For example,

```
(binary_expression (number_literal) @number-in-exp)
```

captures `number_literal` nodes that are inside a
`binary_expression` node with the capture name
`number-in-exp`.

We can capture the `binary_expression` node as well, with, for
example, the capture name `biexp`:

```
(binary_expression
 (number_literal) @number-in-exp) @biexp
```

### Query function [¶](#Query-function)

Now we can introduce the *query functions*.

Function: **treesit-query-capture** node query &optional beg end node-only grouped [¶](#index-treesit_002dquery_002dcapture)
:   This function matches patterns in query within node. The
    argument query can be either an s-expression, a string, or a
    compiled query object. For now, we focus on the s-expression syntax;
    string syntax and compiled queries are described at the end of
    the section.

    The argument node can also be a parser or a language symbol. A
    parser means use its root node, a language symbol means find or create
    a parser for that language in the current buffer, and use the root
    node.

    The function returns all the captured nodes in an alist with elements
    of the form `(capture_name . node)`. If
    node-only is non-`nil`, it returns the list of nodes
    instead. By default the entire text of node is searched, but if
    beg and end are both non-`nil`, they specify the
    region of buffer text where this function should match nodes. Any
    matching node whose span overlaps with the region between beg
    and end is captured; it doesn’t have to be completely contained
    in the region.

    If grouped is non-`nil`, this function returns a grouped list
    of lists of captured nodes. The grouping is determined by query.
    Captures in the same match of a pattern in query are grouped
    together.

    This function raises the `treesit-query-error` error if
    query is malformed. The signal data contains a description of
    the specific error. You can use `treesit-query-validate` to
    validate and debug the query.

For example, suppose node’s text is `1 + 2`, and
query is

```
(setq query
      '((binary_expression
         (number_literal) @number-in-exp) @biexp)
```

Matching that query would return

```
(treesit-query-capture node query)
    ⇒ ((biexp . <node for "1 + 2">)
       (number-in-exp . <node for "1">)
       (number-in-exp . <node for "2">))
```

As mentioned earlier, query could contain multiple patterns.
For example, it could have two top-level patterns:

```
(setq query
      '((binary_expression) @biexp
        (number_literal) @number)
```

Function: **treesit-query-string** string query language [¶](#index-treesit_002dquery_002dstring)
:   This function parses string as language, matches its root
    node with query, and returns the result.

### More query syntax [¶](#More-query-syntax)

Besides node type and capture name, tree-sitter’s pattern syntax can
express anonymous node, field name, wildcard, quantification,
grouping, alternation, anchor, and predicate.

#### Anonymous node [¶](#Anonymous-node)

An anonymous node is written verbatim, surrounded by quotes. A
pattern matching (and capturing) keyword `return` would be

```
"return" @keyword
```

#### Wild card [¶](#Wild-card)

In a pattern, ‘`(_)`’ matches any named node, and ‘`_`’ matches
any named or anonymous node. For example, to capture any named child
of a `binary_expression` node, the pattern would be

```
(binary_expression (_) @in-biexp)
```

#### Field name [¶](#Field-name)

It is possible to capture child nodes that have specific field names.
In the pattern below, `declarator` and `body` are field
names, indicated by the colon following them.

```
(function_definition
  declarator: (_) @func-declarator
  body: (_) @func-body)
```

It is also possible to capture a node that doesn’t have a certain
field, say, a `function_definition` without a `body` field:

```
(function_definition !body) @func-no-body
```

#### Quantify node [¶](#Quantify-node)

Tree-sitter recognizes quantification operators ‘`:*`’, ‘`:+`’,
and ‘`:?`’. Their meanings are the same as in regular expressions:
‘`:*`’ matches the preceding pattern zero or more times, ‘`:+`’
matches one or more times, and ‘`:?`’ matches zero or one times.

For example, the following pattern matches `type_declaration`
nodes that have *zero or more* `long` keywords.

```
(type_declaration "long" :*) @long-type
```

The following pattern matches a type declaration that may or may not
have a `long` keyword:

```
(type_declaration "long" :?) @long-type
```

#### Grouping [¶](#Grouping)

Similar to groups in regular expressions, we can bundle patterns into
groups and apply quantification operators to them. For example, to
express a comma-separated list of identifiers, one could write

```
(identifier) ("," (identifier)) :*
```

#### Alternation [¶](#Alternation)

Again, similar to regular expressions, we can express “match any one
of these patterns” in a pattern. The syntax is a vector of patterns.
For example, to capture some keywords in C, the pattern would be

```
[
  "return"
  "break"
  "if"
  "else"
] @keyword
```

#### Anchor [¶](#Anchor)

The anchor operator `:anchor` can be used to enforce juxtaposition,
i.e., to enforce two things to be directly next to each other. The
two “things” can be two nodes, or a child and the end of its parent.
For example, to capture the first child, the last child, or two
adjacent children:

```
;; Anchor the child with the end of its parent.
(compound_expression (_) @last-child :anchor)
```

```

```

```
;; Anchor the child with the beginning of its parent.
(compound_expression :anchor (_) @first-child)
```

```

```

```
;; Anchor two adjacent children.
(compound_expression
 (_) @prev-child
 :anchor
 (_) @next-child)
```

Note that the enforcement of juxtaposition ignores any anonymous
nodes.

#### Predicate [¶](#Predicate)

It is possible to add predicate constraints to a pattern. For
example, with the following pattern:

```
(
 (array :anchor (_) @first (_) @last :anchor)
 (:eq? @first @last)
)
```

tree-sitter only matches arrays where the first element is equal to
the last element. To attach a predicate to a pattern, we need to
group them together. Currently there are three predicates:
`:eq?`, `:match?`, and `:pred?`.

Predicate: **:eq?** arg1 arg2 [¶](#index-_003aeq_003f)
:   Matches if arg1 is equal to arg2. Arguments can be either
    strings or capture names. Capture names represent the text that the
    captured node spans in the buffer. Note that this is more like
    `equal` in Elisp, but `eq?` is the convention used by
    tree-sitter. Previously we supported the `:equal` predicate but
    it’s now considered deprecated.

Predicate: **:match?** capture-name regexp [¶](#index-_003amatch_003f)
:   Matches if the text that capture-name’s node spans in the buffer
    matches regular expression regexp, given as a string literal.
    Matching is case-sensitive. The ordering of the arguments doesn’t
    matter. Previously we supported the `:match` predicate but it’s
    now considered deprecated.

Predicate: **:pred?** fn &rest nodes [¶](#index-_003apred_003f)
:   Matches if function fn returns non-`nil` when passed each
    node in nodes as arguments. The function runs with the current
    buffer set to the buffer of node being queried. Be very careful when
    using this predicate, since it can be expensive when used in a tight
    loop. Previously we supported the `:pred` predicate but it’s now
    considered deprecated.

Note that a predicate can only refer to capture names that appear in
the same pattern. Indeed, it makes little sense to refer to capture
names in other patterns.

### String patterns [¶](#String-patterns)

Besides s-expressions, Emacs allows the tree-sitter’s native query
syntax to be used by writing them as strings. It largely resembles
the s-expression syntax. For example, the following query

```
(treesit-query-capture
 node '((addition_expression
         left: (_) @left
         "+" @plus-sign
         right: (_) @right) @addition

         ["return" "break"] @keyword))
```

is equivalent to

```
(treesit-query-capture
 node "(addition_expression
        left: (_) @left
        \"+\" @plus-sign
        right: (_) @right) @addition

        [\"return\" \"break\"] @keyword")
```

Most patterns can be written directly as s-expressions inside a string.
Only a few of them need modification:

* Anchor `:anchor` is written as ‘`.`’.
* ‘`:?`’ is written as ‘`?`’.
* ‘`:*`’ is written as ‘`*`’.
* ‘`:+`’ is written as ‘`+`’.
* `:eq?`, `:match?` and `:pred?` are written as
  `#eq?`, `#match?` and `#pred?`, respectively. In
  general, predicates change the ‘`:`’ to ‘`#`’.

For example,

```
'((
   (compound_expression :anchor (_) @first (_) :* @rest)
   (:match? "love" @first)
   ))
```

is written in string form as

```
"(
  (compound_expression . (_) @first (_)* @rest)
  (#match? \"love\" @first)
  )"
```

### Compiling queries [¶](#Compiling-queries)

If a query is intended to be used repeatedly, especially in tight
loops, it is important to compile that query, because a compiled query
is much faster than an uncompiled one. A compiled query can be used
anywhere a query is accepted.

Function: **treesit-query-compile** language query &optional eager [¶](#index-treesit_002dquery_002dcompile)
:   This function compiles query for language into a compiled
    query object and returns it.

    This function raises the `treesit-query-error` error if
    query is malformed. The signal data contains a description of
    the specific error. You can use `treesit-query-validate` to
    validate and debug the query.

    By default, Emacs lazily compiles query, meaning query isn’t
    actually compiled until it’s used. To compile query immediately,
    pass non-`nil` for eager.

    To tell an actually compiled query apart from one that hasn’t been
    compiled, use `treesit-query-eagerly-compiled-p`.

    If query is malformed or language can’t be loaded, this function
    signals `treesit-query-error`. Obviously this will only happen
    when eager is non-`nil`, since otherwise Emacs doesn’t
    actually compile query.

There are some additional functions for queries:
`treesit-query-language` returns the language of a query;
`treesit-query-source` returns the original string or sexp source
query of a compiled query; `treesit-query-valid-p` checks whether a
query is valid; `treesit-query-expand` converts a s-expression
query into the string format; and `treesit-pattern-expand` converts
a pattern.

Tree-sitter grammars change overtime. To support multiple possible
versions of a grammar, a Lisp program can use
`treesit-query-first-valid` to pick the right query to use. For
example, if a grammar has a `(defun)` node in one version, and
later renamed it to `(function_definition)`, a Lisp program can use

```
(treesit-query-first-valid 'lang
  '((defun) @defun)
  '((function_definition) @defun))
```

to support both versions of the grammar.

For more details, consider reading the tree-sitter project’s
documentation about pattern-matching. The documentation can be found at
<https://tree-sitter.github.io/tree-sitter/using-parsers#pattern-matching-with-queries>.

---

Next: [Parsing Text in Multiple Languages](#Multiple-Languages), Previous: [Pattern Matching Tree-sitter Nodes](#Pattern-Matching), Up: [Parsing Program Source](#Parsing-Program-Source)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
