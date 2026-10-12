### 38.6 User-defined “Things” and Navigation [¶](#User_002ddefined-_0060_0060Things_0027_0027-and-Navigation)

It’s often useful to be able to identify and find certain *things* in
a buffer, like function and class definitions, statements, code blocks,
strings, comments, etc., in terms of node types defined by the
tree-sitter grammar used in the buffer. Emacs allows Lisp programs to
define what kinds of tree-sitter nodes corresponds to each “thing”.
This enables handy features like jumping to the next function, marking
the code block at point, transposing two function arguments, etc.

The “things” feature in Emacs is independent of the pattern matching
feature of tree-sitter (see [Pattern Matching Tree-sitter Nodes](#Pattern-Matching)), and comparatively
less powerful, but more suitable for navigation and traversing the
buffer text in terms of the tree-sitter parse tree.

You can define things with treesit-thing-settings, retrieve the
predicate of a defined thing with `treesit-thing-definition`, and
test if a thing is defined with `treesit-thing-defined-p`.

Variable: **treesit-thing-settings** [¶](#index-treesit_002dthing_002dsettings)
:   This is an alist of thing definitions for each language supported by the
    grammar used in a buffer; it should be defined by the buffer’s major
    mode (the default value is `nil`). The key of each entry is a
    language symbol (e.g., `c` for C, `cpp` for C`++`, etc.),
    and the value is a list of thing definitions of the form
    `(thing pred)`, where thing is a symbol
    representing the thing, and pred specifies what kinds of
    tree-sitter nodes are considered as this thing.

    The symbol used to define the thing can be anything meaningful for
    the major mode: `defun`, `defclass`, `sentence`,
    `comment`, `string`, etc. To support tree-sitter based
    navigation commands (see [Moving over Balanced Expressions](#List-Motion)), the mode should define two
    things: `list` and `sexp`.

    pred can be a regexp string that matches the type of the node; it
    can be a function that takes a node as the argument and returns a
    boolean that indicates whether the node qualifies as the thing; or it can
    be a cons `(regexp . fn)`, which is a combination
    of a regular expression regexp and a function fn—the node
    has to match both the regexp and to satisfy fn to qualify as
    the thing.

    pred can also be recursively defined. It can be `(or pred…)`, meaning that satisfying any one of the preds
    qualifies the node as the thing. It can be `(and pred…)`, meaning that satisfying all of the preds
    qualifies the node as the thing. It can be `(not pred)`,
    meaning that not satisfying pred qualifies the node.

    Finally, pred can refer to other things defined in this
    list. For example, `(or sexp sentence)` defines something
    that’s either a `sexp` thing or a `sentence` thing, as defined
    by some other rules in the alist.

    There are two pre-defined predicates: `named` and `anonymous`,
    which qualify, respectively, named and anonymous nodes of the
    tree-sitter grammar. They can be combined with `and` to narrow
    down the match.

    Here’s an example `treesit-thing-settings` for C and C`++`:

    ```
    ((c
      (defun "function_definition")
      (sexp (not "[](),[{}]"))
      (comment "comment")
      (string "raw_string_literal")
      (text (or comment string)))
    ```

    ```
     (cpp
      (defun ("function_definition" . cpp-ts-mode-defun-valid-p))
      (defclass "class_specifier")
      (comment "comment")))
    ```

    Note that this example is modified for didactic purposes, and isn’t
    exactly how tree-sitter based C and C`++` modes define things.

Emacs builtin functions already make use of some thing definitions.
Command `treesit-forward-sexp` uses the `sexp` definition if
major mode defines it (see [Moving over Balanced Expressions](#List-Motion)); `treesit-forward-list`,
`treesit-down-list`, `treesit-up-list`,
`treesit-show-paren-data` use the `list` definition (its
symbol `list` has the symbol property `treesit-thing-symbol`
to avoid ambiguity with the function that has the same name);
`treesit-forward-sentence` uses the `sentence` definition.
Defun movement functions like `treesit-end-of-defun` uses the
`defun` definition (`defun` definition is overridden by
treesit-defun-type-regexp for backward compatibility). Major
modes can also define `comment`, `string`, and `text`
things (to match comments and strings).

The rest of this section lists a few functions that take advantage of
the thing definitions. Besides the functions below, some other
functions listed elsewhere also utilize the thing feature, e.g.,
tree-traversing functions like `treesit-search-forward`,
`treesit-induce-sparse-tree`, etc. See [Retrieving Nodes](#Retrieving-Nodes).

Function: **treesit-node-match-p** node thing &optional ignore-missing [¶](#index-treesit_002dnode_002dmatch_002dp)
:   This function checks whether node represents a thing.

    If node represents thing, return non-`nil`, otherwise
    return `nil`. For convenience, if `node` is `nil`, this
    function just returns `nil`.

    The thing can be either a thing symbol like `defun`, or
    simply a predicate that defines a thing, like
    `"function_definition"`, or `(or comment string)`.

    By default, if thing is undefined or malformed, this function
    signals `treesit-invalid-predicate` error. If ignore-missing
    is `t`, this function doesn’t signal the error when thing is
    undefined and just returns `nil`; but it still signals the error if
    thing is a malformed predicate.

Functions below are responsible for finding things and moving across
them, and they have to deal with the fact that a buffer sometimes
contains multiple adjacent or nested parsers. By default, these
functions try to be helpful and search in every relevant parser at
point, from most specific (deepest embedded) to the least. Lisp
programs should be cautious and assess whether this behavior is desired
when using these functions as building blocks of other functions; if
not, explicitly pass a parser or language.

Function: **treesit-thing-prev** position thing &optional parser [¶](#index-treesit_002dthing_002dprev)
:   This function returns the first node before position in the
    current buffer that is the specified thing. If no such node
    exists, it returns `nil`.

    It’s guaranteed that, if a node is returned, the node’s end position is
    less or equal to position. In other words, this function never
    returns a node that encloses position.

    Again, thing can be either a symbol or a predicate.

    If parser is non-`nil`, only use that parser’s parse tree.
    Otherwise try each parser covering point, from the most specific
    (deepest-embedded) to the least specific. If there are multiple parsers
    with the same embed level (see [parser-embed-level](#parser_002dembed_002dlevel)) at
    position, which parser is tried first is undefined. If
    parser is a language symbol, the function limits the parsers it
    tries to the ones for that language.

Function: **treesit-thing-next** position thing &optional parser [¶](#index-treesit_002dthing_002dnext)
:   This function is similar to `treesit-thing-prev`, only it returns
    the first node *after* position that’s the thing. It
    also guarantees that if a node is returned, the node’s start position is
    greater or equal to position. The parser parameter is the
    same as in `treesit-thing-prev`.

Function: **treesit-navigate-thing** position arg side thing &optional tactic [¶](#index-treesit_002dnavigate_002dthing)
:   This function builds upon `treesit-thing-prev` and
    `treesit-thing-next` and provides functionality that a navigation
    command would find useful. It returns the position after moving across
    arg instances of thing from position. If
    there aren’t enough things to navigate across, it returns nil. The
    function doesn’t move point.

    A positive arg means moving forward that many instances of
    thing; negative arg means moving backward. If side is
    `beg`, this function returns the position of the beginning of
    thing; if it’s `end`, it returns the position at the end of
    thing.

    Like in `treesit-thing-prev`, thing can be a thing symbol
    defined in `treesit-thing-settings`, or a predicate, and
    parser can be either `nil`, a parser, or a language symbol.
    Like in that function, parser decides which parsers or languages
    are searched. When there are multiple parsers available, this function
    tries each until it succeeds.

    tactic determines how this function moves between things. It can
    be `nested`, `top-level`, `restricted`,
    `parent-first`, or `nil`. `nested` or `nil` means
    normal nested navigation: first try to move across siblings; if there
    aren’t any siblings left in the current level, move to the parent, then
    its siblings, and so on. `top-level` means only navigate across
    top-level things and ignore nested things. `restricted` means
    movement is restricted within the thing that encloses position, if
    there is such a thing. This tactic is useful for commands that want to
    stop at the current nesting level and not move up. parent-first
    means move to the parent if there is one; and move to siblings if
    there’s no parent.

Function: **treesit-thing-at** position thing &optional strict parser [¶](#index-treesit_002dthing_002dat)
:   This function returns the smallest node that’s the thing and
    encloses position; if there’s no such node, it returns `nil`.

    The returned node must enclose position, i.e., its start position is
    less or equal to position, and it’s end position is greater or equal to
    position.

    If strict is non-`nil`, this function uses strict comparison,
    i.e., start position must be strictly smaller than position, and end
    position must be strictly greater than position.

    thing can be either a thing symbol defined in
    `treesit-thing-settings`, or a predicate.

    If parser is non-nil, only use that parser’s parse tree.
    Otherwise try each parser covering point, from the most specific
    (deepest-embedded) to the least specific. If there are multiple parsers
    with the same embed level (see [parser-embed-level](#parser_002dembed_002dlevel)) at
    position, which parser is tried first is undefined. parser
    can also be a language symbol.

There are also some convenient wrapper functions.
`treesit-beginning-of-thing` moves point to the beginning of a
thing, `treesit-end-of-thing` moves to the end of a thing, and
`treesit-thing-at-point` returns the thing at point.

There are also defun commands that specifically use the `defun`
definition (as a fallback of `treesit-defun-type-regexp`), like
`treesit-beginning-of-defun`, `treesit-end-of-defun`, and
`treesit-defun-at-point`. In addition, these functions use
`treesit-defun-tactic` as the navigation tactic. They are
described in more detail in other sections (see [Developing major modes with tree-sitter](#Tree_002dsitter-Major-Modes)).

---

Next: [Developing major modes with tree-sitter](#Tree_002dsitter-Major-Modes), Previous: [User-defined “Things” and Navigation](#User_002ddefined-Things), Up: [Parsing Program Source](#Parsing-Program-Source)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 38.7 Parsing Text in Multiple Languages [¶](#Parsing-Text-in-Multiple-Languages)

Sometimes, the source of a programming language could contain snippets
of other languages; HTML + CSS + JavaScript is one
example. In that case, text segments written in different languages
need to be assigned different parsers. Traditionally, this is
achieved by using narrowing. While tree-sitter works with narrowing
(see [narrowing](#tree_002dsitter-narrowing)), the recommended way is
instead to specify regions of buffer text (i.e., ranges) in which a
parser will operate. This section describes functions for setting and
getting ranges for a parser.

Generally when there are multiple languages at play, there is a
“primary”, or “host” language. The parser for this language—the
*primary parser*, parses the entire buffer. Parsers for other
languages are “embedded” or “guest” parsers, which only work on part
of the buffer. The parse tree of the primary parser is usually used to
determine the ranges in which the embedded parsers operate.

Major modes should set `treesit-primary-parser` to the primary
parser before calling `treesit-major-mode-setup`, so that Emacs can
configure the primary parser correctly for font-lock and other features.

Lisp programs should call `treesit-update-ranges` to make sure the
ranges for each parser are correct before using parsers in a buffer, and
call `treesit-language-at` to figure out the language responsible
for the text at some position. These two functions don’t work by
themselves; they need major modes to set `treesit-range-settings`
and optionally `treesit-language-at-point-function`, which do the actual work.
These functions and variables are explained in more detail towards the
end of the section.

In short, multi-language major modes should set
`treesit-primary-parser`, `treesit-range-settings`, and optionally
`treesit-language-at-point-function` before calling
`treesit-major-mode-setup`.

### Getting and setting ranges [¶](#Getting-and-setting-ranges)

Function: **treesit-parser-set-included-ranges** parser ranges [¶](#index-treesit_002dparser_002dset_002dincluded_002dranges)
:   This function sets up parser to operate on ranges. The
    parser will only read the text of the specified ranges. Each
    range in ranges is a pair of the form `(beg . end)`.

    The ranges in ranges must come in order and must not overlap.
    That is, in pseudo code:

    ```
    (cl-loop for idx from 1 to (1- (length ranges))
             for prev = (nth (1- idx) ranges)
             for next = (nth idx ranges)
             should (<= (car prev) (cdr prev)
                        (car next) (cdr next)))
    ```

    If ranges violates this constraint, or something else went
    wrong, this function signals the `treesit-range-invalid` error.
    The signal data contains a specific error message and the ranges we
    are trying to set.

    This function can also be used for disabling ranges. If ranges
    is `nil`, the parser is set to parse the whole buffer.

    Example:

    ```
    (treesit-parser-set-included-ranges
     parser '((1 . 9) (16 . 24) (24 . 25)))
    ```

Function: **treesit-parser-included-ranges** parser [¶](#index-treesit_002dparser_002dincluded_002dranges)
:   This function returns the ranges set for parser. The return
    value is the same as the ranges argument of
    `treesit-parser-included-ranges`: a list of cons cells of the form
    `(beg . end)`. If parser doesn’t have any
    ranges, the return value is `nil`.

    ```
    (treesit-parser-included-ranges parser)
        ⇒ ((1 . 9) (16 . 24) (24 . 25))
    ```

Function: **treesit-query-range** source query &optional beg end [¶](#index-treesit_002dquery_002drange)
:   This function matches source with query and returns the
    ranges of captured nodes. The return value is a list of cons cells of
    the form `(beg . end)`, where beg and
    end specify the beginning and the end of a region of text.

    For convenience, source can be a language symbol, a parser, or a
    node. If it’s a language symbol, this function matches in the root
    node of the first parser using that language; if a parser, this
    function matches in the root node of that parser; if a node, this
    function matches in that node.

    The argument query is the query used to capture nodes
    (see [Pattern Matching Tree-sitter Nodes](#Pattern-Matching)). The capture names don’t matter. The
    arguments beg and end, if both non-`nil`, limit the
    range in which this function queries.

    Like other query functions, this function raises the
    `treesit-query-error` error if query is malformed.

### Supporting multiple languages in Lisp programs [¶](#Supporting-multiple-languages-in-Lisp-programs)

It should suffice for general Lisp programs to call the following two
functions in order to support program sources that mix multiple
languages.

Function: **treesit-update-ranges** &optional beg end [¶](#index-treesit_002dupdate_002dranges)
:   This function updates ranges for parsers in the buffer. It makes sure
    the parsers’ ranges are set correctly between beg and end,
    according to `treesit-range-settings`. If omitted, beg
    defaults to the beginning of the buffer, and end defaults to the
    end of the buffer.

    For example, fontification functions use this function before querying
    for nodes in a region.

Function: **treesit-language-at** pos [¶](#index-treesit_002dlanguage_002dat)
:   This function returns the language of the text at buffer position
    pos. Under the hood it calls
    `treesit-language-at-point-function` and returns its return value.
    If `treesit-language-at-point-function` is `nil`, this
    function returns the language of the deepest parser by embed level
    (see [parser-embed-level](#parser_002dembed_002dlevel)) among parsers returned by
    `treesit-parsers-at`. If there is no parser at that buffer
    position, it returns `nil`.

### Supporting multiple languages in major modes [¶](#Supporting-multiple-languages-in-major-modes)

Normally, in a set of languages that can be mixed together, there is a
*host language* and one or more *embedded languages*. A Lisp
program usually first parses the whole document with the host
language’s parser, retrieves some information, sets ranges for the
embedded languages with that information, and then parses the embedded
languages.

Take a buffer containing HTML, CSS, and JavaScript
as an example. A Lisp program will first parse the whole buffer with
an HTML parser, then query the parser for
`style_element` and `script_element` nodes, which correspond
to CSS and JavaScript text, respectively. Then it sets the
range of the CSS and JavaScript parsers to the range which
their corresponding nodes span.

Given a simple HTML document:

```
<html>
  <script>1 + 2</script>
  <style>body { color: "blue"; }</style>
</html>
```

a Lisp program will first parse with a HTML parser, then set
ranges for CSS and JavaScript parsers:

```
;; Create parsers.
(setq html (treesit-parser-create 'html))
(setq css (treesit-parser-create 'css))
(setq js (treesit-parser-create 'javascript))
```

```

```

```
;; Set CSS ranges.
(setq css-range
      (treesit-query-range
       'html
       '((style_element (raw_text) @capture))))
(treesit-parser-set-included-ranges css css-range)
```

```

```

```
;; Set JavaScript ranges.
(setq js-range
      (treesit-query-range
       'html
       '((script_element (raw_text) @capture))))
(treesit-parser-set-included-ranges js js-range)
```

Emacs automates this process in `treesit-update-ranges`. A
multi-language major mode should set `treesit-range-settings` so
that `treesit-update-ranges` knows how to perform this process
automatically. Major modes should use the helper function
`treesit-range-rules` to generate a value that can be assigned to
`treesit-range-settings`. The settings in the following example
directly translate into operations shown above.

```
(setq treesit-range-settings
      (treesit-range-rules
       :embed 'javascript
       :host 'html
       '((script_element (raw_text) @capture))
```

```
       :embed 'css
       :host 'html
       '((style_element (raw_text) @capture))))
```

```

```

```
;; Major modes with multiple languages can optionally set
;; `treesit-language-at-point-function' (which see).
(setq treesit-language-at-point-function
      (lambda (pos)
        (let* ((node (treesit-node-at pos 'html))
               (parent (treesit-node-parent node)))
          (cond
           ((and node parent
                 (equal (treesit-node-type node) "raw_text")
                 (equal (treesit-node-type parent) "script_element"))
            'javascript)
           ((and node parent
                 (equal (treesit-node-type node) "raw_text")
                 (equal (treesit-node-type parent) "style_element"))
            'css)
           (t 'html)))))
```

Function: **treesit-range-rules** &rest query-specs [¶](#index-treesit_002drange_002drules)
:   This function is used to set `treesit-range-settings`. It takes
    care of compiling queries and other post-processing, and outputs a
    value that `treesit-range-settings` can have.

    It takes a series of query-specs, where each query-spec is
    a query preceded by zero or more keyword/value
    pairs. Each query is a tree-sitter query in either the string,
    s-expression, or compiled form, or a function.

    `treesit-update-ranges` uses query to figure out how to set
    the ranges for parsers for the embedded language. It queries
    query in a host language parser, computes the ranges which the
    captured nodes span, and applies these ranges to embedded language
    parsers.

    If query is a tree-sitter query, it should be preceded by two
    keyword/value pairs, where the `:embed` keyword
    specifies the embedded language, and the `:host` keyword specifies
    the host language. The captured nodes determine the range of embedded
    parsers. The capture names don’t matter, but names that starts with an
    underscore are ignored, and the `@language` capture name is
    reserved.

    If the embedded language is dynamic, then `:embed` can specify a
    function rathar than a static language. The function takes the node
    captured with `@language` capture name. Care have to be taken that
    the `@language` capture and the range capture are in the same
    match group. The function should return either a language symbol, or nil
    if no valid language can be found.

    If the query is given a `:offset` keyword whose value is a pair
    of numbers, each captured range is offset by those numbers. For
    example, an offset of `(1 . -1)` will update a captured range of
    `(2 . 8)` to be `(3 . 7)`. This can be used to exclude things
    like surrounding delimiters from being included in the range covered by
    an embedded parser.

    By default, a parser sees its ranges as a continuum, rather than
    treating them as separate independent segments. Therefore, if the
    embedded ranges are semantically independent segments, they should be
    processed by local parsers, described below.

    If the query is given the `:local` keyword whose value is
    `t`, the range set by this query has a dedicated local parser;
    otherwise the range shares a parser with other ranges for the same
    language.

    Local parsers set to a range can be retrieved by
    `treesit-local-parsers-at` and `treesit-local-parsers-on`.

    Emacs treats local embedded parsers and non-local embedded parsers quite
    differently:

    * Local parsers are managed by Emacs. Emacs creates new local parsers when
      they are needed and deletes them when they are not. Non-local parsers
      must be created by major modes, and Emacs only make use of them if one
      exists.
    * Because local parsers can be freely created by Emacs, they are suitable
      for recursively nested embedded parsers, where the embed level can be
      arbitrarily deep. On the other hand, since non-local parsers created by
      major modes, they can only handle a fix number of embed levels.

    The embed level of the top-level parsers is 0 or omitted, the first
    level of embedded parsers get embed level 1, and so on. For local
    parsers, each embed level creates a new parser; for non-local parsers,
    since Emacs cannot create non-local parsers freely, it tries to find a
    usable parser at the right embed level; if none exists, Emacs just gives
    up.

    If the query is given a `:range-fn` keyword whose value is a
    function, Emacs uses that function to compute the ranges to use for the
    embedded parser. The function takes as arguments the captured node and
    the offset given by the `:offset` keyword, and returns a list of
    ranges, where each range is a cons of the start and end position. For
    local parsers, each query creates one local parser, and the ranges
    set by the `:range-fn` are applied to that parser; for non-local
    parsers, all the ranges (including ranges created by `:range-fn`)
    in that embed level are joined together and applied to the same parser.

    If query is a function, it doesn’t need any keyword and
    value pair. It should be a function that takes 2 arguments,
    start and end, and sets the ranges for parsers in the
    current buffer in the region between start and end. It is
    fine for this function to set ranges in a larger region that
    encompasses the region between start and end.

Variable: **treesit-range-settings** [¶](#index-treesit_002drange_002dsettings)
:   This variable helps `treesit-update-ranges` in updating the
    ranges for parsers in the buffer. It is a list of settings
    where the exact format of a setting is considered internal. You
    should use `treesit-range-rules` to generate a value that this
    variable can have.

Variable: **treesit-language-at-point-function** [¶](#index-treesit_002dlanguage_002dat_002dpoint_002dfunction)
:   This variable’s value should be a function that takes a single
    argument, pos, which is a buffer position, and returns the
    language of the buffer text at pos. This variable is used by
    `treesit-language-at`.

Function: **treesit-parsers-at** &optional pos language with-host only [¶](#index-treesit_002dparsers_002dat)
:   This function returns all parsers at pos in the current buffer.
    pos defaults to point. The returned parsers are sorted by the
    decreasing embed level.

    If language is non-`nil`, return parsers only for that
    language.

    If with-host is non-`nil`, return a list of
    `(parser . host-parser)` where host-parser
    is the host parser which created the parser.

    If only is non-`nil`, return all parsers including the
    primary parser.

    The argument only can be a list of symbols that specify what
    parsers to include in the return value.

    If only contains the symbol `local`, include local parsers.
    Local parsers are those which only parse a limited region marked by an
    overlay with a non-`nil` `treesit-parser-local-p` property.

    If only contains the symbol `global`, include non-local parsers
    excluding the primary parser.

    If only contains the symbol ‘primary’, include the primary parser.

Function: **treesit-local-parsers-at** &optional pos language with-host [¶](#index-treesit_002dlocal_002dparsers_002dat)
:   This function returns all the local parsers at pos in the
    current buffer. pos defaults to point.

    Local parsers are those which only parse a limited region marked by an
    overlay with a non-`nil` `treesit-parser-local-p` property.
    If language is non-`nil`, only return parsers for that
    language.

Function: **treesit-local-parsers-on** &optional beg end language with-host [¶](#index-treesit_002dlocal_002dparsers_002don)
:   This function is the same as `treesit-local-parsers-at`, but it
    returns the local parsers in the range between beg and end
    instead of at point.

    beg and end default to the entire accessible portion of
    the buffer.

---

Next: [Tree-sitter C API Correspondence](#Tree_002dsitter-C-API), Previous: [Parsing Text in Multiple Languages](#Multiple-Languages), Up: [Parsing Program Source](#Parsing-Program-Source)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 38.8 Developing major modes with tree-sitter [¶](#Developing-major-modes-with-tree_002dsitter)

This section covers some general guidelines on developing tree-sitter
integration for a major mode.

A major mode supporting tree-sitter features should roughly follow
this pattern:

```
(define-derived-mode woomy-mode prog-mode "Woomy"
  "A mode for Woomy programming language."
  (when (treesit-ready-p 'woomy)
    (setq-local treesit-variables ...)
    ...
    (treesit-major-mode-setup)))
```

`treesit-ready-p` automatically emits a warning if conditions for
enabling tree-sitter aren’t met.

If a tree-sitter major mode shares setup with its “native”
counterpart, one can create a “base mode” that contains the common
setup, like this:

```
(define-derived-mode woomy--base-mode prog-mode "Woomy"
  "An internal mode for Woomy programming language."
  (common-setup)
  ...)
```

```

```

```
(define-derived-mode woomy-mode woomy--base-mode "Woomy"
  "A mode for Woomy programming language."
  (native-setup)
  ...)
```

```

```

```
(define-derived-mode woomy-ts-mode woomy--base-mode "Woomy"
  "A mode for Woomy programming language."
  (when (treesit-ready-p 'woomy)
    (setq-local treesit-variables ...)
    ...
    (treesit-major-mode-setup)))
```

Function: **treesit-ready-p** language &optional quiet [¶](#index-treesit_002dready_002dp)
:   This function checks for conditions for activating tree-sitter. It
    checks whether Emacs was built with tree-sitter, whether the buffer’s
    size is not too large for tree-sitter to handle, and whether the
    grammar for language is available on the system (see [Tree-sitter Language Grammar](#Language-Grammar)).

    This function emits a warning if tree-sitter cannot be activated. If
    quiet is `message`, the warning is turned into a message;
    if quiet is `t`, no warning or message is displayed.

    If all the necessary conditions are met, this function returns
    non-`nil`; otherwise it returns `nil`.

Function: **treesit-major-mode-setup** [¶](#index-treesit_002dmajor_002dmode_002dsetup)
:   This function activates some tree-sitter features for a major mode.

    Currently, it sets up the following features:

    * If `treesit-font-lock-settings` (see [Parser-based Font Lock](#Parser_002dbased-Font-Lock))
      is non-`nil`, it sets up fontification.
    * If either `treesit-simple-indent-rules` or
      `treesit-indent-function` (see [Parser-based Indentation](#Parser_002dbased-Indentation)) is
      non-`nil`, it sets up indentation.
    * If `treesit-defun-type-regexp` is non-`nil`, it sets up
      navigation functions for `beginning-of-defun` and
      `end-of-defun`. See [Moving over Balanced Expressions](#List-Motion).
    * If `treesit-defun-name-function` is non-`nil`, it sets up
      add-log functions used by `add-log-current-defun`.
    * If `treesit-simple-imenu-settings` (see [Imenu](#Imenu)) is
      non-`nil`, it sets up Imenu.
    * If `treesit-outline-predicate` (see [Outline Minor Mode](#Outline-Minor-Mode)) is
      non-`nil`, it sets up Outline minor mode.
    * If `sexp` and/or `sentence` are defined in
      `treesit-thing-settings` (see [User-defined “Things” and Navigation](#User_002ddefined-Things)), it enables
      navigation commands that move, respectively, by sexps and sentences by
      defining variables such as `forward-sexp-function` and
      `forward-sentence-function`.

For more information on these built-in tree-sitter features,
see [Parser-based Font Lock](#Parser_002dbased-Font-Lock), see [Parser-based Indentation](#Parser_002dbased-Indentation), and
see [Moving over Balanced Expressions](#List-Motion).

For supporting mixing of multiple languages in a major mode,
see [Parsing Text in Multiple Languages](#Multiple-Languages).

Besides `beginning-of-defun` and `end-of-defun`, Emacs
provides some additional functions for working with defuns:
`treesit-defun-at-point` returns the defun node at point, and
`treesit-defun-name` returns the name of a defun node.

Function: **treesit-defun-at-point** [¶](#index-treesit_002ddefun_002dat_002dpoint)
:   This function returns the defun node at point, or `nil` if none
    is found. It respects `treesit-defun-tactic`: if its value is
    `top-level`, this function returns the top-level defun, and if
    its value is `nested`, it returns the immediate enclosing defun.

    This function requires `treesit-defun-type-regexp` to work. If
    it is `nil`, this function simply returns `nil`.

Function: **treesit-defun-name** node [¶](#index-treesit_002ddefun_002dname)
:   This function returns the defun name of node. It returns
    `nil` if there is no defun name for node, or if node
    is not a defun node, or if node is `nil`.

    Depending on the language and major mode, the defun names are names
    like function name, class name, struct name, etc.

    If `treesit-defun-name-function` is `nil`, this function
    always returns `nil`.

Variable: **treesit-defun-name-function** [¶](#index-treesit_002ddefun_002dname_002dfunction)
:   If non-`nil`, this variable’s value should be a function that is
    called with a node as its argument, and returns the defun name of the
    node. The function should have the same semantics as
    `treesit-defun-name`: if the node is not a defun node, or the
    node is a defun node but doesn’t have a name, or the node is
    `nil`, it should return `nil`.

---

Previous: [Developing major modes with tree-sitter](#Tree_002dsitter-Major-Modes), Up: [Parsing Program Source](#Parsing-Program-Source)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 38.9 Tree-sitter C API Correspondence [¶](#Tree_002dsitter-C-API-Correspondence)

Emacs’s tree-sitter integration doesn’t expose every feature
provided by tree-sitter’s C API. Missing features include:

* Creating a tree cursor and navigating the syntax tree with it.
* Setting timeout and cancellation flag for a parser.
* Setting the logger for a parser.
* Printing a DOT graph of the syntax tree to a file.
* Copying and modifying a syntax tree. (Emacs doesn’t expose a tree
  object.)
* Using (row, column) coordinates as position.
* Updating a node with changes. (In Emacs, retrieve a new node instead
  of updating the existing one.)
* Querying statics of a language grammar.

In addition, Emacs makes some changes to the C API to make the API more
convenient and idiomatic:

* Instead of using byte positions, the Emacs Lisp API uses character
  positions.
* Null nodes are converted to `nil`.

Below is the correspondence between all C API functions and their
Lisp counterparts. Sometimes one Lisp function corresponds to
multiple C functions, and many C functions don’t have a Lisp
counterpart.

```
ts_parser_new                           treesit-parser-create
ts_parser_delete
ts_parser_set_language
ts_parser_language                      treesit-parser-language
ts_parser_set_included_ranges           treesit-parser-set-included-ranges
ts_parser_included_ranges               treesit-parser-included-ranges
ts_parser_parse
ts_parser_parse_string                  treesit-parse-string
ts_parser_parse_string_encoding
ts_parser_reset
ts_parser_set_timeout_micros
ts_parser_timeout_micros
ts_parser_set_cancellation_flag
ts_parser_cancellation_flag
ts_parser_set_logger
ts_parser_logger
ts_parser_print_dot_graphs
ts_tree_copy
ts_tree_delete
ts_tree_root_node
ts_tree_language
ts_tree_edit
ts_tree_get_changed_ranges
ts_tree_print_dot_graph
ts_node_type                            treesit-node-type
ts_node_symbol
ts_node_start_byte                      treesit-node-start
ts_node_start_point
ts_node_end_byte                        treesit-node-end
ts_node_end_point
ts_node_string                          treesit-node-string
ts_node_is_null
ts_node_is_named                        treesit-node-check
ts_node_is_missing                      treesit-node-check
ts_node_is_extra                        treesit-node-check
ts_node_has_changes
ts_node_has_error                       treesit-node-check
ts_node_parent                          treesit-node-parent
ts_node_child                           treesit-node-child
ts_node_field_name_for_child            treesit-node-field-name-for-child
ts_node_child_count                     treesit-node-child-count
ts_node_named_child                     treesit-node-child
ts_node_named_child_count               treesit-node-child-count
ts_node_child_by_field_name             treesit-node-child-by-field-name
ts_node_child_by_field_id
ts_node_next_sibling                    treesit-node-next-sibling
ts_node_prev_sibling                    treesit-node-prev-sibling
ts_node_next_named_sibling              treesit-node-next-sibling
ts_node_prev_named_sibling              treesit-node-prev-sibling
ts_node_first_child_for_byte            treesit-node-first-child-for-pos
ts_node_first_named_child_for_byte      treesit-node-first-child-for-pos
ts_node_descendant_for_byte_range       treesit-node-descendant-for-range
ts_node_descendant_for_point_range
ts_node_named_descendant_for_byte_range treesit-node-descendant-for-range
ts_node_named_descendant_for_point_range
ts_node_edit
ts_node_eq                              treesit-node-eq
ts_tree_cursor_new
ts_tree_cursor_delete
ts_tree_cursor_reset
ts_tree_cursor_current_node
ts_tree_cursor_current_field_name
ts_tree_cursor_current_field_id
ts_tree_cursor_goto_parent
ts_tree_cursor_goto_next_sibling
ts_tree_cursor_goto_first_child
ts_tree_cursor_goto_first_child_for_byte
ts_tree_cursor_goto_first_child_for_point
ts_tree_cursor_copy
ts_query_new
ts_query_delete
ts_query_pattern_count
ts_query_capture_count
ts_query_string_count
ts_query_start_byte_for_pattern
ts_query_predicates_for_pattern
ts_query_step_is_definite
ts_query_capture_name_for_id
ts_query_string_value_for_id
ts_query_disable_capture
ts_query_disable_pattern
ts_query_cursor_new
ts_query_cursor_delete
ts_query_cursor_exec                    treesit-query-capture
ts_query_cursor_did_exceed_match_limit
ts_query_cursor_match_limit
ts_query_cursor_set_match_limit
ts_query_cursor_set_byte_range
ts_query_cursor_set_point_range
ts_query_cursor_next_match
ts_query_cursor_remove_match
ts_query_cursor_next_capture
ts_language_symbol_count
ts_language_symbol_name
ts_language_symbol_for_name
ts_language_field_count
ts_language_field_name_for_id
ts_language_field_id_for_name
ts_language_symbol_type
ts_language_version
```

---

Next: [Threads](#Threads), Previous: [Parsing Program Source](#Parsing-Program-Source), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 39 Abbrevs and Abbrev Expansion [¶](#Abbrevs-and-Abbrev-Expansion)

An abbreviation or *abbrev* is a string of characters that may be
expanded to a longer string. The user can insert the abbrev string and
find it replaced automatically with the expansion of the abbrev. This
saves typing.

The set of abbrevs currently in effect is recorded in an *abbrev
table*. Each buffer has a local abbrev table, but normally all buffers
in the same major mode share one abbrev table. There is also a global
abbrev table. Normally both are used.

An abbrev table is represented as an obarray. See [Creating and Interning Symbols](#Creating-Symbols), for information about obarrays. Each abbreviation is
represented by a symbol in the obarray. The symbol’s name is the
abbreviation; its value is the expansion; its function definition is
the hook function for performing the expansion (see [Defining Abbrevs](#Defining-Abbrevs)); and its property list cell contains various additional
properties, including the use count and the number of times the
abbreviation has been expanded (see [Abbrev Properties](#Abbrev-Properties)).

Certain abbrevs, called *system abbrevs*, are defined by a major
mode instead of the user. A system abbrev is identified by its
non-`nil` `:system` property (see [Abbrev Properties](#Abbrev-Properties)).
When abbrevs are saved to an abbrev file, system abbrevs are omitted.
See [Saving Abbrevs in Files](#Abbrev-Files).

Because the symbols used for abbrevs are not interned in the usual
obarray, they will never appear as the result of reading a Lisp
expression; in fact, normally they are never used except by the code
that handles abbrevs. Therefore, it is safe to use them in a
nonstandard way.

If the minor mode Abbrev mode is enabled, the buffer-local variable
`abbrev-mode` is non-`nil`, and abbrevs are automatically
expanded in the buffer. For the user-level commands for abbrevs, see
[Abbrev Mode](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Abbrevs) in The GNU Emacs Manual.

* [Abbrev Tables](#Abbrev-Tables)
* [Defining Abbrevs](#Defining-Abbrevs)
* [Saving Abbrevs in Files](#Abbrev-Files)
* [Looking Up and Expanding Abbreviations](#Abbrev-Expansion)
* [Standard Abbrev Tables](#Standard-Abbrev-Tables)
* [Abbrev Properties](#Abbrev-Properties)
* [Abbrev Table Properties](#Abbrev-Table-Properties)

---

Next: [Defining Abbrevs](#Defining-Abbrevs), Up: [Abbrevs and Abbrev Expansion](#Abbrevs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 39.1 Abbrev Tables [¶](#Abbrev-Tables-1)

This section describes how to create and manipulate abbrev tables.

Function: **make-abbrev-table** &optional props [¶](#index-make_002dabbrev_002dtable)
:   This function creates and returns a new, empty abbrev table—an
    obarray containing no symbols.
    props is a property list that is applied to the new table
    (see [Abbrev Table Properties](#Abbrev-Table-Properties)).

Function: **abbrev-table-p** object [¶](#index-abbrev_002dtable_002dp)
:   This function returns a non-`nil` value if object is an
    abbrev table.

Function: **clear-abbrev-table** abbrev-table [¶](#index-clear_002dabbrev_002dtable)
:   This function undefines all the abbrevs in abbrev-table, leaving
    it empty.

Function: **copy-abbrev-table** abbrev-table [¶](#index-copy_002dabbrev_002dtable)
:   This function returns a copy of abbrev-table—a new abbrev
    table containing the same abbrev definitions. It does *not* copy
    any property lists; only the names, values, and functions.

Function: **define-abbrev-table** tabname definitions &optional docstring &rest props [¶](#index-define_002dabbrev_002dtable)
:   This function defines tabname (a symbol) as an abbrev table
    name, i.e., as a variable whose value is an abbrev table. It defines
    abbrevs in the table according to definitions, a list of
    elements of the form `(abbrevname expansion
    [hook] [props...])`. These elements are passed as
    arguments to `define-abbrev`.

    The optional string docstring is the documentation string of the
    variable tabname. The property list props is applied to
    the abbrev table (see [Abbrev Table Properties](#Abbrev-Table-Properties)).

    If this function is called more than once for the same tabname,
    subsequent calls add the definitions in definitions to
    tabname, rather than overwriting the entire original contents.
    (A subsequent call only overrides abbrevs explicitly redefined or
    undefined in definitions.)

Variable: **abbrev-table-name-list** [¶](#index-abbrev_002dtable_002dname_002dlist)
:   This is a list of symbols whose values are abbrev tables.
    `define-abbrev-table` adds the new abbrev table name to this list.

Function: **insert-abbrev-table-description** name &optional human [¶](#index-insert_002dabbrev_002dtable_002ddescription)
:   This function inserts before point a description of the abbrev table
    named name. The argument name is a symbol whose value is an
    abbrev table.

    If human is non-`nil`, the description is human-oriented.
    System abbrevs are listed and identified as such. Otherwise the
    description is a Lisp expression—a call to `define-abbrev-table`
    that would define name as it is currently defined, but without
    the system abbrevs. (The mode or package using name is supposed
    to add these to name separately.)

---

Next: [Saving Abbrevs in Files](#Abbrev-Files), Previous: [Abbrev Tables](#Abbrev-Tables), Up: [Abbrevs and Abbrev Expansion](#Abbrevs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 39.2 Defining Abbrevs [¶](#Defining-Abbrevs-1)

`define-abbrev` is the low-level basic function for defining an
abbrev in an abbrev table.

When a major mode defines a system abbrev, it should call
`define-abbrev` and specify `t` for the `:system`
property. Be aware that any saved non-system abbrevs are restored
at startup, i.e., before some major modes are loaded. Therefore, major
modes should not assume that their abbrev tables are empty when they
are first loaded.

Function: **define-abbrev** abbrev-table name expansion &optional hook &rest props [¶](#index-define_002dabbrev)
:   This function defines an abbrev named name, in
    abbrev-table, to expand to expansion and call hook,
    with properties props (see [Abbrev Properties](#Abbrev-Properties)). The return
    value is name. The `:system` property in props is
    treated specially here: if it has the value `force`, then it will
    overwrite an existing definition even for a non-system abbrev of
    the same name.

    name should be a string. The argument expansion is
    normally the desired expansion (a string), or `nil` to undefine
    the abbrev. If it is anything but a string or `nil`, then the
    abbreviation expands solely by running hook.

    The argument hook is a function or `nil`. If hook is
    non-`nil`, then it is called with no arguments after the abbrev is
    replaced with expansion; point is located at the end of
    expansion when hook is called.

    If hook is a non-`nil` symbol whose `no-self-insert`
    property is non-`nil`, hook can explicitly control whether
    to insert the self-inserting input character that triggered the
    expansion. If hook returns non-`nil` in this case, that
    inhibits insertion of the character. By contrast, if hook
    returns `nil`, `expand-abbrev` (or `abbrev-insert`)
    also returns `nil`, as if expansion had not really occurred.

    Normally, `define-abbrev` sets the variable
    `abbrevs-changed` to `t`, if it actually changes the abbrev.
    This is so that some commands will offer to save the abbrevs. It
    does not do this for a system abbrev, since those aren’t saved anyway.

User Option: **only-global-abbrevs** [¶](#index-only_002dglobal_002dabbrevs)
:   If this variable is non-`nil`, it means that the user plans to use
    global abbrevs only. This tells the commands that define mode-specific
    abbrevs to define global ones instead. This variable does not alter the
    behavior of the functions in this section; it is examined by their
    callers.

---

Next: [Looking Up and Expanding Abbreviations](#Abbrev-Expansion), Previous: [Defining Abbrevs](#Defining-Abbrevs), Up: [Abbrevs and Abbrev Expansion](#Abbrevs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 39.3 Saving Abbrevs in Files [¶](#Saving-Abbrevs-in-Files)

A file of saved abbrev definitions is actually a file of Lisp code.
The abbrevs are saved in the form of a Lisp program to define the same
abbrev tables with the same contents. Therefore, you can load the file
with `load` (see [How Programs Do Loading](#How-Programs-Do-Loading)). However, the
function `quietly-read-abbrev-file` is provided as a more
convenient interface. Emacs automatically calls this function at
startup.

User-level facilities such as `save-some-buffers` can save
abbrevs in a file automatically, under the control of variables
described here.

User Option: **abbrev-file-name** [¶](#index-abbrev_002dfile_002dname)
:   This is the default file name for reading and saving abbrevs. By
    default, Emacs will look for `~/.emacs.d/abbrev_defs`, and, if
    not found, for `~/.abbrev_defs`; if neither file exists, Emacs
    will create `~/.emacs.d/abbrev_defs`.

Function: **quietly-read-abbrev-file** &optional filename [¶](#index-quietly_002dread_002dabbrev_002dfile)
:   This function reads abbrev definitions from a file named filename,
    previously written with `write-abbrev-file`. If filename is
    omitted or `nil`, the file specified in `abbrev-file-name` is
    used.

    As the name implies, this function does not display any messages.

User Option: **save-abbrevs** [¶](#index-save_002dabbrevs)
:   A non-`nil` value for `save-abbrevs` means that Emacs should
    offer to save abbrevs (if any have changed) when files are saved. If
    the value is `silently`, Emacs saves the abbrevs without asking
    the user. `abbrev-file-name` specifies the file to save the
    abbrevs in. The default value is `t`.

Variable: **abbrevs-changed** [¶](#index-abbrevs_002dchanged)
:   This variable is set non-`nil` by defining or altering any
    abbrevs (except system abbrevs). This serves as a flag for various
    Emacs commands to offer to save your abbrevs.

Command: **write-abbrev-file** &optional filename [¶](#index-write_002dabbrev_002dfile)
:   Save all abbrev definitions (except system abbrevs), for all abbrev
    tables listed in `abbrev-table-name-list`, in the file
    filename, in the form of a Lisp program that when loaded will
    define the same abbrevs. Tables that do not have any abbrevs to save
    are omitted. If filename is `nil` or omitted,
    `abbrev-file-name` is used. This function returns `nil`.

---

Next: [Standard Abbrev Tables](#Standard-Abbrev-Tables), Previous: [Saving Abbrevs in Files](#Abbrev-Files), Up: [Abbrevs and Abbrev Expansion](#Abbrevs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 39.4 Looking Up and Expanding Abbreviations [¶](#Looking-Up-and-Expanding-Abbreviations)

Abbrevs are usually expanded by certain interactive commands,
including `self-insert-command`. This section describes the
subroutines used in writing such commands, as well as the variables they
use for communication.

Function: **abbrev-symbol** abbrev &optional table [¶](#index-abbrev_002dsymbol)
:   This function returns the symbol representing the abbrev named
    abbrev. It returns `nil` if that abbrev is not
    defined. The optional second argument table is the abbrev table
    in which to look it up. If table is `nil`, this function
    tries first the current buffer’s local abbrev table, and second the
    global abbrev table.

Function: **abbrev-expansion** abbrev &optional table [¶](#index-abbrev_002dexpansion)
:   This function returns the string that abbrev would expand into (as
    defined by the abbrev tables used for the current buffer). It returns
    `nil` if abbrev is not a valid abbrev.
    The optional argument table specifies the abbrev table to use,
    as in `abbrev-symbol`.

Command: **expand-abbrev** [¶](#index-expand_002dabbrev)
:   This command expands the abbrev before point, if any. If point does not
    follow an abbrev, this command does nothing. To do the expansion, it
    calls the function that is the value of the `abbrev-expand-function`
    variable, with no arguments, and returns whatever that function does.

    The default expansion function returns the abbrev symbol if it did
    expansion, and `nil` otherwise. If the abbrev symbol has a hook
    function that is a symbol whose `no-self-insert` property is
    non-`nil`, and if the hook function returns `nil` as its
    value, then the default expansion function returns `nil`,
    even though expansion did occur.

Function: **abbrev-insert** abbrev &optional name start end [¶](#index-abbrev_002dinsert)
:   This function inserts the abbrev expansion of `abbrev`, replacing
    the text between `start` and `end`. If `start` is
    omitted, it defaults to point. `name`, if non-`nil`, should
    be the name by which this abbrev was found (a string); it is used to
    figure out whether to adjust the capitalization of the expansion. The
    function returns `abbrev` if the abbrev was successfully
    inserted, otherwise it returns `nil`.

Command: **abbrev-prefix-mark** &optional arg [¶](#index-abbrev_002dprefix_002dmark)
:   This command marks the current location of point as the beginning of
    an abbrev. The next call to `expand-abbrev` will use the text
    from here to point (where it is then) as the abbrev to expand, rather
    than using the previous word as usual.

    First, this command expands any abbrev before point, unless arg
    is non-`nil`. (Interactively, arg is the prefix argument.)
    Then it inserts a hyphen before point, to indicate the start of the
    next abbrev to be expanded. The actual expansion removes the hyphen.

User Option: **abbrev-all-caps** [¶](#index-abbrev_002dall_002dcaps)
:   When this is set non-`nil`, an abbrev entered entirely in upper
    case is expanded using all upper case. Otherwise, an abbrev entered
    entirely in upper case is expanded by capitalizing each word of the
    expansion.

Variable: **abbrev-start-location** [¶](#index-abbrev_002dstart_002dlocation)
:   The value of this variable is a buffer position (an integer or a marker)
    for `expand-abbrev` to use as the start of the next abbrev to be
    expanded. The value can also be `nil`, which means to use the
    word before point instead. `abbrev-start-location` is set to
    `nil` each time `expand-abbrev` is called. This variable is
    also set by `abbrev-prefix-mark`.

Variable: **abbrev-start-location-buffer** [¶](#index-abbrev_002dstart_002dlocation_002dbuffer)
:   The value of this variable is the buffer for which
    `abbrev-start-location` has been set. Trying to expand an abbrev
    in any other buffer clears `abbrev-start-location`. This variable
    is set by `abbrev-prefix-mark`.

Variable: **last-abbrev** [¶](#index-last_002dabbrev)
:   This is the `abbrev-symbol` of the most recent abbrev expanded. This
    information is left by `expand-abbrev` for the sake of the
    `unexpand-abbrev` command (see [Expanding
    Abbrevs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Expanding-Abbrevs) in The GNU Emacs Manual).

Variable: **last-abbrev-location** [¶](#index-last_002dabbrev_002dlocation)
:   This is the location of the most recent abbrev expanded. This contains
    information left by `expand-abbrev` for the sake of the
    `unexpand-abbrev` command.

Variable: **last-abbrev-text** [¶](#index-last_002dabbrev_002dtext)
:   This is the exact expansion text of the most recent abbrev expanded,
    after case conversion (if any). Its value is `nil` if the abbrev
    has already been unexpanded. This contains information left by
    `expand-abbrev` for the sake of the `unexpand-abbrev` command.

Variable: **abbrev-expand-function** [¶](#index-abbrev_002dexpand_002dfunction)
:   The value of this variable is a function that `expand-abbrev`
    will call with no arguments to do the expansion. The function can do
    anything it wants before and after performing the expansion.
    It should return the abbrev symbol if expansion took place.

The following sample code shows a simple use of
`abbrev-expand-function`. It assumes that `foo-mode` is a
mode for editing certain files in which lines that start with ‘`#`’
are comments. You want to use Text mode abbrevs for those lines. The
regular local abbrev table, `foo-mode-abbrev-table` is
appropriate for all other lines. See [Standard Abbrev Tables](#Standard-Abbrev-Tables), for the
definitions of `local-abbrev-table` and `text-mode-abbrev-table`.
See [Advising Emacs Lisp Functions](#Advising-Functions), for details of `add-function`.

```
(defun foo-mode-abbrev-expand-function (expand)
  (if (not (save-excursion (forward-line 0) (eq (char-after) ?#)))
      ;; Performs normal expansion.
      (funcall expand)
    ;; We're inside a comment: use the text-mode abbrevs.
    (let ((local-abbrev-table text-mode-abbrev-table))
      (funcall expand))))

(add-hook 'foo-mode-hook
          (lambda ()
            (add-function :around (local 'abbrev-expand-function)
                          #'foo-mode-abbrev-expand-function)))
```

---

Next: [Abbrev Properties](#Abbrev-Properties), Previous: [Looking Up and Expanding Abbreviations](#Abbrev-Expansion), Up: [Abbrevs and Abbrev Expansion](#Abbrevs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 39.5 Standard Abbrev Tables [¶](#Standard-Abbrev-Tables-1)

Here we list the variables that hold the abbrev tables for the
preloaded major modes of Emacs.

Variable: **global-abbrev-table** [¶](#index-global_002dabbrev_002dtable)
:   This is the abbrev table for mode-independent abbrevs. The abbrevs
    defined in it apply to all buffers. Each buffer may also have a local
    abbrev table, whose abbrev definitions take precedence over those in the
    global table.

Variable: **local-abbrev-table** [¶](#index-local_002dabbrev_002dtable)
:   The value of this buffer-local variable is the (mode-specific)
    abbreviation table of the current buffer. It can also be a list of
    such tables.

Variable: **abbrev-minor-mode-table-alist** [¶](#index-abbrev_002dminor_002dmode_002dtable_002dalist)
:   The value of this variable is a list of elements of the form
    `(mode . abbrev-table)` where mode is the name
    of a variable: if the variable is bound to a non-`nil` value,
    then the abbrev-table is active, otherwise it is ignored.
    abbrev-table can also be a list of abbrev tables.

Variable: **fundamental-mode-abbrev-table** [¶](#index-fundamental_002dmode_002dabbrev_002dtable)
:   This is the local abbrev table used in Fundamental mode; in other words,
    it is the local abbrev table in all buffers in Fundamental mode.

Variable: **text-mode-abbrev-table** [¶](#index-text_002dmode_002dabbrev_002dtable)
:   This is the local abbrev table used in Text mode.

Variable: **lisp-mode-abbrev-table** [¶](#index-lisp_002dmode_002dabbrev_002dtable)
:   This is the local abbrev table used in Lisp mode. It is the parent
    of the local abbrev table used in Emacs Lisp mode. See [Abbrev Table Properties](#Abbrev-Table-Properties).

---

Next: [Abbrev Table Properties](#Abbrev-Table-Properties), Previous: [Standard Abbrev Tables](#Standard-Abbrev-Tables), Up: [Abbrevs and Abbrev Expansion](#Abbrevs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 39.6 Abbrev Properties [¶](#Abbrev-Properties-1)

Abbrevs have properties, some of which influence the way they work.
You can provide them as arguments to `define-abbrev`, and
manipulate them with the following functions:

Function: **abbrev-put** abbrev prop val [¶](#index-abbrev_002dput)
:   Set the property prop of abbrev to value val.

Function: **abbrev-get** abbrev prop [¶](#index-abbrev_002dget)
:   Return the property prop of abbrev, or `nil` if the
    abbrev has no such property.

The following properties have special meanings:

`:count`
:   This property counts the number of times the abbrev has
    been expanded. If not explicitly set, it is initialized to 0 by
    `define-abbrev`.

`:system`
:   If non-`nil`, this property marks the abbrev as a system abbrev.
    Such abbrevs are not saved (see [Saving Abbrevs in Files](#Abbrev-Files)).

`:enable-function`
:   If non-`nil`, this property should be a function of no
    arguments which returns `nil` if the abbrev should not be used
    and `t` otherwise.

`:case-fixed`
:   If non-`nil`, this property indicates that the case of the
    abbrev’s name is significant and should only match a text with the
    same pattern of capitalization. It also disables the code that
    modifies the capitalization of the expansion.

---

Previous: [Abbrev Properties](#Abbrev-Properties), Up: [Abbrevs and Abbrev Expansion](#Abbrevs)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 39.7 Abbrev Table Properties [¶](#Abbrev-Table-Properties-1)

Like abbrevs, abbrev tables have properties, some of which influence
the way they work. You can provide them as arguments to
`define-abbrev-table`, and manipulate them with the functions:

Function: **abbrev-table-put** table prop val [¶](#index-abbrev_002dtable_002dput)
:   Set the property prop of abbrev table table to value val.

Function: **abbrev-table-get** table prop [¶](#index-abbrev_002dtable_002dget)
:   Return the property prop of abbrev table table, or `nil`
    if table has no such property.

The following properties have special meaning:

`:enable-function`
:   This is like the `:enable-function` abbrev property except that
    it applies to all abbrevs in the table. It is used before even trying
    to find the abbrev before point, so it can dynamically modify the
    abbrev table.

`:case-fixed`
:   This is like the `:case-fixed` abbrev property except that it
    applies to all abbrevs in the table.

`:regexp`
:   If non-`nil`, this property is a regular expression that
    indicates how to extract the name of the abbrev before point, before
    looking it up in the table. When the regular expression matches
    before point, the abbrev name is expected to be in submatch 1.
    If this property is `nil`, the default is to use
    `backward-word` and `forward-word` to find the name. This
    property allows the use of abbrevs whose name contains characters of
    non-word syntax.

`:parents`
:   This property holds a list of tables from which to inherit
    other abbrevs.

`:abbrev-table-modiff`
:   This property holds a counter incremented each time a new abbrev is
    added to the table.

---

Next: [Processes](#Processes), Previous: [Abbrevs and Abbrev Expansion](#Abbrevs), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 40 Threads [¶](#Threads-1)

Emacs Lisp provides a limited form of concurrency, called
*threads*. All the threads in a given instance of Emacs share the
same memory. Concurrency in Emacs Lisp is “mostly cooperative”,
meaning that Emacs will only switch execution between threads at
well-defined times. However, the Emacs thread support has been
designed in a way to later allow more fine-grained concurrency, and
correct programs should not rely on cooperative threading.

Currently, thread switching will occur upon explicit request via
`thread-yield`, when waiting for keyboard input or for process
output from asynchronous processes (e.g., during
`accept-process-output`), or during blocking operations relating
to threads, such as mutex locking or `thread-join`.

Emacs Lisp provides primitives to create and control threads, and
also to create and control mutexes and condition variables, useful for
thread synchronization.

While global variables are shared among all Emacs Lisp threads,
local variables are not—a dynamic `let` binding is local. Each
thread also has its own current buffer (see [The Current Buffer](#Current-Buffer)) and
its own match data (see [The Match Data](#Match-Data)).

Note that `let` bindings are treated specially by the Emacs
Lisp implementation. There is no way to duplicate this unwinding and
rewinding behavior other than by using `let`. For example, a
manual implementation of `let` written using
`unwind-protect` cannot arrange for variable values to be
thread-specific.

In the case of lexical bindings (see [Scoping Rules for Variable Bindings](#Variable-Scoping)), a
closure is an object like any other in Emacs Lisp, and bindings in a
closure are shared by any threads invoking the closure.

* [Basic Thread Functions](#Basic-Thread-Functions)
* [Mutexes](#Mutexes)
* [Condition Variables](#Condition-Variables)
* [The Thread List](#The-Thread-List)

---

Next: [Mutexes](#Mutexes), Up: [Threads](#Threads)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 40.1 Basic Thread Functions [¶](#Basic-Thread-Functions-1)

Threads can be created and waited for. A thread cannot be exited
directly, but the current thread can be exited implicitly, and other
threads can be signaled.

Function: **make-thread** function &optional name buffer-disposition [¶](#index-make_002dthread)
:   Create a new thread of execution which invokes function. When
    function returns, the thread exits.

    The new thread is created with no local variable bindings in effect.
    The new thread’s current buffer is inherited from the current thread.

    name can be supplied to give a name to the thread. The name is
    used for debugging and informational purposes only; it has no meaning
    to Emacs. If name is provided, it must be a string.

    buffer-disposition indicates what happens if the thread’s current
    buffer is about to be killed. If the value is `t`, killing the
    buffer is not allowed. Any other value, including `nil` (which is
    the default), means that the buffer is killed and the thread is assigned
    another current buffer, and it’s signaled the error
    `thread-buffer-killed`. But if the value is the symbol
    `silently`, the error is not signaled.

    This function returns the new thread.

After a thread had been created, you can inspect or change its
buffer-disposition using functions `thread-buffer-disposition` and
`thread-set-buffer-disposition`.

Function: **threadp** object [¶](#index-threadp)
:   This function returns `t` if object represents an Emacs
    thread, `nil` otherwise.

Function: **thread-join** thread [¶](#index-thread_002djoin)
:   Block until thread exits, or until the current thread is
    signaled. It returns the result of the thread function. If
    thread has already exited, this returns immediately.

Function: **thread-signal** thread error-symbol data [¶](#index-thread_002dsignal)
:   Like `signal` (see [How to Signal an Error](#Signaling-Errors)), but the signal is
    delivered in the thread thread. If thread is the current
    thread, then this just calls `signal` immediately. Otherwise,
    thread will receive the signal as soon as it becomes current.
    If thread was blocked by a call to `mutex-lock`,
    `condition-wait`, or `thread-join`; `thread-signal`
    will unblock it.

    If thread is the main thread, the signal is not propagated
    there. Instead, it is shown as message in the main thread.

Function: **thread-yield** [¶](#index-thread_002dyield)
:   Yield execution to the next runnable thread.

Function: **thread-name** thread [¶](#index-thread_002dname)
:   Return the name of thread, as specified to `make-thread`.

Function: **thread-live-p** thread [¶](#index-thread_002dlive_002dp)
:   Return `t` if thread is alive, or `nil` if it is not.
    A thread is alive as long as its function is still executing.

Function: **thread--blocker** thread [¶](#index-thread_002d_002dblocker)
:   Return the object that thread is waiting on. This function is
    primarily intended for debugging, and is given a “double hyphen”
    name to indicate that.

    If thread is blocked in `thread-join`, this returns the
    thread for which it is waiting.

    If thread is blocked in `mutex-lock`, this returns the mutex.

    If thread is blocked in `condition-wait`, this returns the
    condition variable.

    Otherwise, this returns `nil`.

Function: **current-thread** [¶](#index-current_002dthread)
:   Return the current thread.

Function: **all-threads** [¶](#index-all_002dthreads)
:   Return a list of all the live thread objects. A new list is returned
    by each invocation.

Variable: **main-thread** [¶](#index-main_002dthread)
:   This variable keeps the main thread Emacs is running, or `nil` if
    Emacs is compiled without thread support.

When code run by a thread signals an error that is unhandled, the
thread exits. Other threads can access the error form which caused
the thread to exit using the following function.

Function: **thread-last-error** &optional cleanup [¶](#index-thread_002dlast_002derror)
:   This function returns the last error form recorded when a thread
    exited due to an error. Each thread that exits abnormally overwrites
    the form stored by the previous thread’s error with a new value, so
    only the last one can be accessed. If cleanup is
    non-`nil`, the stored form is reset to `nil`.

---

Next: [Condition Variables](#Condition-Variables), Previous: [Basic Thread Functions](#Basic-Thread-Functions), Up: [Threads](#Threads)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 40.2 Mutexes [¶](#Mutexes-1)

A *mutex* is an exclusive lock. At any moment, zero or one
threads may own a mutex. If a thread attempts to acquire a mutex, and
the mutex is already owned by some other thread, then the acquiring
thread will block until the mutex becomes available.

Emacs Lisp mutexes are of a type called *recursive*, which means
that a thread can re-acquire a mutex it owns any number of times. A
mutex keeps a count of how many times it has been acquired, and each
acquisition of a mutex must be paired with a release. The last
release by a thread of a mutex reverts it to the unowned state,
potentially allowing another thread to acquire the mutex.

Function: **mutexp** object [¶](#index-mutexp)
:   This function returns `t` if object represents an Emacs
    mutex, `nil` otherwise.

Function: **make-mutex** &optional name [¶](#index-make_002dmutex)
:   Create a new mutex and return it. If name is specified, it is a
    name given to the mutex. It must be a string. The name is for
    debugging purposes only; it has no meaning to Emacs.

Function: **mutex-name** mutex [¶](#index-mutex_002dname)
:   Return the name of mutex, as specified to `make-mutex`.

Function: **mutex-lock** mutex [¶](#index-mutex_002dlock)
:   This will block until this thread acquires mutex, or until this
    thread is signaled using `thread-signal`. If mutex is
    already owned by this thread, this simply returns.

Function: **mutex-unlock** mutex [¶](#index-mutex_002dunlock)
:   Release mutex. If mutex is not owned by this thread, this
    will signal an error.

Macro: **with-mutex** mutex body… [¶](#index-with_002dmutex)
:   This macro is the simplest and safest way to evaluate forms while
    holding a mutex. It acquires mutex, invokes body, and
    then releases mutex. It returns the result of body.

---

Next: [The Thread List](#The-Thread-List), Previous: [Mutexes](#Mutexes), Up: [Threads](#Threads)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 40.3 Condition Variables [¶](#Condition-Variables-1)

A *condition variable* is a way for a thread to block until some
event occurs. A thread can wait on a condition variable, to be woken
up when some other thread notifies the condition.

A condition variable is associated with a mutex and, conceptually,
with some condition. For proper operation, the mutex must be
acquired, and then a waiting thread must loop, testing the condition
and waiting on the condition variable. For example:

```
(with-mutex mutex
  (while (not global-variable)
    (condition-wait cond-var)))
```

The mutex ensures atomicity, and the loop is for robustness—there
may be spurious notifications.

Similarly, the mutex must be held before notifying the condition.
The typical, and best, approach is to acquire the mutex, make the
changes associated with this condition, and then notify it:

```
(with-mutex mutex
  (setq global-variable (some-computation))
  (condition-notify cond-var))
```

Function: **make-condition-variable** mutex &optional name [¶](#index-make_002dcondition_002dvariable)
:   Make a new condition variable associated with mutex. If
    name is specified, it is a name given to the condition variable.
    It must be a string. The name is for debugging purposes only; it has
    no meaning to Emacs.

Function: **condition-variable-p** object [¶](#index-condition_002dvariable_002dp)
:   This function returns `t` if object represents a condition
    variable, `nil` otherwise.

Function: **condition-wait** cond [¶](#index-condition_002dwait)
:   Wait for another thread to notify cond, a condition variable.
    This function will block until the condition is notified, or until a
    signal is delivered to this thread using `thread-signal`.

    It is an error to call `condition-wait` without holding the
    condition’s associated mutex.

    `condition-wait` releases the associated mutex while waiting.
    This allows other threads to acquire the mutex in order to notify the
    condition.

Function: **condition-notify** cond &optional all [¶](#index-condition_002dnotify)
:   Notify cond. The mutex with cond must be held before
    calling this. Ordinarily a single waiting thread is woken by
    `condition-notify`; but if all is not `nil`, then all
    threads waiting on cond are notified.

    `condition-notify` releases the associated mutex.
    This allows other threads to acquire the mutex in order to wait on the
    condition.

Function: **condition-name** cond [¶](#index-condition_002dname)
:   Return the name of cond, as passed to
    `make-condition-variable`.

Function: **condition-mutex** cond [¶](#index-condition_002dmutex)
:   Return the mutex associated with cond. Note that the associated
    mutex cannot be changed.

---

Previous: [Condition Variables](#Condition-Variables), Up: [Threads](#Threads)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 40.4 The Thread List [¶](#The-Thread-List-1)

The `list-threads` command lists all the currently alive threads.
In the resulting buffer, each thread is identified either by the name
passed to `make-thread` (see [Basic Thread Functions](#Basic-Thread-Functions)), or by
its unique internal identifier if it was not created with a name. The
status of each thread at the time of the creation or last update of
the buffer is shown, in addition to the object the thread was blocked
on at the time, if it was blocked.

Variable: **thread-list-refresh-seconds** [¶](#index-thread_002dlist_002drefresh_002dseconds)
:   The `*Threads*` buffer will automatically update twice per
    second. You can make the refresh rate faster or slower by customizing
    this variable.

Here are the commands available in the thread list buffer:

`b` [¶](#index-backtrace-of-thread)
:   Show a backtrace of the thread at point. This will show where in its
    code the thread had yielded or was blocked at the moment you pressed
    `b`. Be aware that the backtrace is a snapshot; the thread could
    have meanwhile resumed execution, and be in a different state, or
    could have exited.

    You may use `g` in the thread’s backtrace buffer to get an updated
    backtrace, as backtrace buffers do not automatically update.
    See [Backtraces](#Backtraces), for a description of backtraces and the other
    commands which work on them.

`s`
:   Signal the thread at point. After `s`, type `q` to send a
    quit signal or `e` to send an error signal. Threads may implement
    handling of signals, but the default behavior is to exit on any
    signal. Therefore you should only use this command if you understand
    how to restart the target thread, because your Emacs session may
    behave incorrectly if necessary threads are killed.

`g`
:   Update the list of threads and their statuses.

---

Next: [Emacs Display](#Display), Previous: [Threads](#Threads), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 41 Processes [¶](#Processes-1)

In the terminology of operating systems, a *process* is a space in
which a program can execute. Emacs runs in a process. Emacs Lisp
programs can invoke other programs in processes of their own. These are
called *subprocesses* or *child processes* of the Emacs process,
which is their *parent process*.

A subprocess of Emacs may be *synchronous* or *asynchronous*,
depending on how it is created. When you create a synchronous
subprocess, the Lisp program waits for the subprocess to terminate
before continuing execution. When you create an asynchronous
subprocess, it can run in parallel with the Lisp program. This kind of
subprocess is represented within Emacs by a Lisp object which is also
called a “process”. Lisp programs can use this object to communicate
with the subprocess or to control it. For example, you can send
signals, obtain status information, receive output from the process, or
send input to it.

In addition to processes that run programs, Lisp programs can open
connections of several types to devices or processes running on the
same machine or on other machines. The supported connection types
are: TCP and UDP network connections, serial port connections, and
pipe connections. Each such connection is also represented by a
process object.

Function: **processp** object [¶](#index-processp)
:   This function returns `t` if object represents an Emacs
    process object, `nil` otherwise. The process object can
    represent a subprocess running a program or a connection of any
    supported type.

In addition to subprocesses of the current Emacs session, you can
also access other processes running on your machine. See [Accessing Other Processes](#System-Processes).

* [Functions that Create Subprocesses](#Subprocess-Creation)
* [Shell Arguments](#Shell-Arguments)
* [Creating a Synchronous Process](#Synchronous-Processes)
* [Creating an Asynchronous Process](#Asynchronous-Processes)
* [Deleting Processes](#Deleting-Processes)
* [Process Information](#Process-Information)
* [Sending Input to Processes](#Input-to-Processes)
* [Sending Signals to Processes](#Signals-to-Processes)
* [Receiving Output from Processes](#Output-from-Processes)
* [Sentinels: Detecting Process Status Changes](#Sentinels)
* [Querying Before Exit](#Query-Before-Exit)
* [Accessing Other Processes](#System-Processes)
* [Transaction Queues](#Transaction-Queues)
* [Network Connections](#Network)
* [Network Servers](#Network-Servers)
* [Datagrams](#Datagrams)
* [Low-Level Network Access](#Low_002dLevel-Network)
* [Misc Network Facilities](#Misc-Network)
* [Communicating with Serial Ports](#Serial-Ports)
* [Packing and Unpacking Byte Arrays](#Byte-Packing)

---

Next: [Shell Arguments](#Shell-Arguments), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
