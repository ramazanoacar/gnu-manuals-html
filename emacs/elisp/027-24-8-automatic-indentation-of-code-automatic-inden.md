### 24.8 Automatic Indentation of code [¶](#Automatic-Indentation-of-code)

For programming languages, an important feature of a major mode is to
provide automatic indentation. There are two parts: one is to decide what
is the right indentation of a line, and the other is to decide when to
reindent a line. By default, Emacs reindents a line whenever you
type a character in `electric-indent-chars`, which by default only
includes Newline. Major modes can add chars to `electric-indent-chars`
according to the syntax of the language.

Deciding what is the right indentation is controlled in Emacs by
`indent-line-function` (see [Indentation Controlled by Major Mode](#Mode_002dSpecific-Indent)). For some modes,
the *right* indentation cannot be known reliably, typically because
indentation is significant so several indentations are valid but with different
meanings. In that case, the mode should set `electric-indent-inhibit` to
make sure the line is not constantly re-indented against the user’s wishes.

Writing a good indentation function can be difficult and to a large extent it
is still a black art. Many major mode authors will start by writing a simple
indentation function that works for simple cases, for example by comparing with
the indentation of the previous text line. For most programming languages that
are not really line-based, this tends to scale very poorly: improving
such a function to let it handle more diverse situations tends to become more
and more difficult, resulting in the end with a large, complex, unmaintainable
indentation function which nobody dares to touch.

A good indentation function will usually need to actually parse the
text, according to the syntax of the language. Luckily, it is not
necessary to parse the text in as much detail as would be needed
for a compiler, but on the other hand, the parser embedded in the
indentation code will want to be somewhat friendly to syntactically
incorrect code.

Good maintainable indentation functions usually fall into two categories:
either parsing forward from some safe starting point until the
position of interest, or parsing backward from the position of interest.
Neither of the two is a clearly better choice than the other: parsing
backward is often more difficult than parsing forward because
programming languages are designed to be parsed forward, but for the
purpose of indentation it has the advantage of not needing to
guess a safe starting point, and it generally enjoys the property
that only a minimum of text will be analyzed to decide the indentation
of a line, so indentation will tend to be less affected by syntax errors in
some earlier unrelated piece of code. Parsing forward on the other hand
is usually easier and has the advantage of making it possible to
reindent efficiently a whole region at a time, with a single parse.

Rather than write your own indentation function from scratch, it is
often preferable to try and reuse some existing ones or to rely
on a generic indentation engine. There are sadly few such
engines. The CC-mode indentation code (used with C, C++, Java, Awk
and a few other such modes) has been made more generic over the years,
so if your language seems somewhat similar to one of those languages,
you might try to use that engine. Another one is SMIE which takes an approach in the spirit
of Lisp sexps and adapts it to non-Lisp languages. Yet another one is
to rely on a full-blown parser, for example, the tree-sitter library.

* [Simple Minded Indentation Engine](#SMIE)
* [Parser-based Indentation](#Parser_002dbased-Indentation)

---

Next: [Parser-based Indentation](#Parser_002dbased-Indentation), Up: [Automatic Indentation of code](#Auto_002dIndentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1 Simple Minded Indentation Engine [¶](#Simple-Minded-Indentation-Engine)

SMIE is a package that provides a generic navigation and indentation
engine. Based on a very simple parser using an operator precedence
grammar, it lets major modes extend the sexp-based navigation of Lisp
to non-Lisp languages as well as provide a simple to use but reliable
auto-indentation.

Operator precedence grammar is a very primitive technology for parsing
compared to some of the more common techniques used in compilers.
It has the following characteristics: its parsing power is very limited,
and it is largely unable to detect syntax errors, but it has the
advantage of being algorithmically efficient and able to parse forward
just as well as backward. In practice that means that SMIE can use it
for indentation based on backward parsing, that it can provide both
`forward-sexp` and `backward-sexp` functionality, and that it
will naturally work on syntactically incorrect code without any extra
effort. The downside is that it also means that most programming
languages cannot be parsed correctly using SMIE, at least not without
resorting to some special tricks (see [Living With a Weak Parser](#SMIE-Tricks)).

* [SMIE Setup and Features](#SMIE-setup)
* [Operator Precedence Grammars](#Operator-Precedence-Grammars)
* [Defining the Grammar of a Language](#SMIE-Grammar)
* [Defining Tokens](#SMIE-Lexer)
* [Living With a Weak Parser](#SMIE-Tricks)
* [Specifying Indentation Rules](#SMIE-Indentation)
* [Helper Functions for Indentation Rules](#SMIE-Indentation-Helpers)
* [Sample Indentation Rules](#SMIE-Indentation-Example)
* [Customizing Indentation](#SMIE-Customization)

---

Next: [Operator Precedence Grammars](#Operator-Precedence-Grammars), Up: [Simple Minded Indentation Engine](#SMIE)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1.1 SMIE Setup and Features [¶](#SMIE-Setup-and-Features)

SMIE is meant to be a one-stop shop for structural navigation and
various other features which rely on the syntactic structure of code, in
particular automatic indentation. The main entry point is
`smie-setup` which is a function typically called while setting
up a major mode.

Function: **smie-setup** grammar rules-function &rest keywords [¶](#index-smie_002dsetup)
:   Setup SMIE navigation and indentation.
    grammar is a grammar table generated by `smie-prec2->grammar`.
    rules-function is a set of indentation rules for use on
    `smie-rules-function`.
    keywords are additional arguments, which can include the following
    keywords:

    * `:forward-token` fun: Specify the forward lexer to use.
    * `:backward-token` fun: Specify the backward lexer to use.

Calling this function is sufficient to make commands such as
`forward-sexp`, `backward-sexp`, and `transpose-sexps` be
able to properly handle structural elements other than just the paired
parentheses already handled by syntax tables. For example, if the
provided grammar is precise enough, `transpose-sexps` can correctly
transpose the two arguments of a `+` operator, taking into account
the precedence rules of the language.

Calling `smie-setup` is also sufficient to make `TAB`
indentation work in the expected way, extends
`blink-matching-paren` to apply to elements like
`begin...end`, and provides some commands that you can bind in
the major mode keymap.

Command: **smie-close-block** [¶](#index-smie_002dclose_002dblock)
:   This command closes the most recently opened (and not yet closed) block.

Command: **smie-down-list** &optional arg [¶](#index-smie_002ddown_002dlist)
:   This command is like `down-list` but it also pays attention to
    nesting of tokens other than parentheses, such as `begin...end`.

---

Next: [Defining the Grammar of a Language](#SMIE-Grammar), Previous: [SMIE Setup and Features](#SMIE-setup), Up: [Simple Minded Indentation Engine](#SMIE)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1.2 Operator Precedence Grammars [¶](#Operator-Precedence-Grammars-1)

SMIE’s precedence grammars simply give to each token a pair of
precedences: the left-precedence and the right-precedence. We say
`T1 < T2` if the right-precedence of token `T1` is less than
the left-precedence of token `T2`. A good way to read this
`<` is as a kind of parenthesis: if we find `... T1 something
T2 ...` then that should be parsed as `... T1 (something T2 ...`
rather than as `... T1 something) T2 ...`. The latter
interpretation would be the case if we had `T1 > T2`. If we have
`T1 = T2`, it means that token T2 follows token T1 in the same
syntactic construction, so typically we have `"begin" = "end"`.
Such pairs of precedences are sufficient to express left-associativity
or right-associativity of infix operators, nesting of tokens like
parentheses and many other cases.

Function: **smie-prec2->grammar** table [¶](#index-smie_002dprec2_002d_003egrammar)
:   This function takes a *prec2* grammar table and returns an
    alist suitable for use in `smie-setup`. The *prec2*
    table is itself meant to be built by one of the functions below.

Function: **smie-merge-prec2s** &rest tables [¶](#index-smie_002dmerge_002dprec2s)
:   This function takes several *prec2* tables and merges them
    into a new *prec2* table.

Function: **smie-precs->prec2** precs [¶](#index-smie_002dprecs_002d_003eprec2)
:   This function builds a *prec2* table from a table of precedences
    precs. precs should be a list, sorted by precedence (for
    example `"+"` will come before `"*"`), of elements of the form
    `(assoc op ...)`, where each op is a token that
    acts as an operator; assoc is their associativity, which can be
    either `left`, `right`, `assoc`, or `nonassoc`.
    All operators in a given element share the same precedence level
    and associativity.

Function: **smie-bnf->prec2** bnf &rest resolvers [¶](#index-smie_002dbnf_002d_003eprec2)
:   This function lets you specify the grammar using a BNF notation.
    It accepts a bnf description of the grammar along with a set of
    conflict resolution rules resolvers, and
    returns a *prec2* table.

    bnf is a list of nonterminal definitions of the form
    `(nonterm rhs1 rhs2 ...)` where each rhs
    is a (non-empty) list of terminals (aka tokens) or non-terminals.

    Not all grammars are accepted:

    * An rhs cannot be an empty list (an empty list is never needed,
      since SMIE allows all non-terminals to match the empty string anyway).
    * An rhs cannot have 2 consecutive non-terminals: each pair of
      non-terminals needs to be separated by a terminal (aka token).
      This is a fundamental limitation of operator precedence grammars.

    Additionally, conflicts can occur:

    * The returned *prec2* table holds constraints between pairs of tokens, and
      for any given pair only one constraint can be present: T1 < T2,
      T1 = T2, or T1 > T2.
    * A token can be an `opener` (something similar to an open-paren),
      a `closer` (like a close-paren), or `neither` of the two
      (e.g., an infix operator, or an inner token like `"else"`).

    Precedence conflicts can be resolved via resolvers, which
    is a list of *precs* tables (see `smie-precs->prec2`): for
    each precedence conflict, if those `precs` tables
    specify a particular constraint, then the conflict is resolved by using
    this constraint instead, else a conflict is reported and one of the
    conflicting constraints is picked arbitrarily and the others are
    simply ignored.

---

Next: [Defining Tokens](#SMIE-Lexer), Previous: [Operator Precedence Grammars](#Operator-Precedence-Grammars), Up: [Simple Minded Indentation Engine](#SMIE)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1.3 Defining the Grammar of a Language [¶](#Defining-the-Grammar-of-a-Language)

The usual way to define the SMIE grammar of a language is by
defining a new global variable that holds the precedence table by
giving a set of BNF rules.
For example, the grammar definition for a small Pascal-like language
could look like:

```
(require 'smie)
(defvar sample-smie-grammar
  (smie-prec2->grammar
   (smie-bnf->prec2
```

```
    '((id)
      (inst ("begin" insts "end")
            ("if" exp "then" inst "else" inst)
            (id ":=" exp)
            (exp))
      (insts (insts ";" insts) (inst))
      (exp (exp "+" exp)
           (exp "*" exp)
           ("(" exps ")"))
      (exps (exps "," exps) (exp)))
```

```
    '((assoc ";"))
    '((assoc ","))
    '((assoc "+") (assoc "*")))))
```

A few things to note:

* The above grammar does not explicitly mention the syntax of function
  calls: SMIE will automatically allow any sequence of sexps, such as
  identifiers, balanced parentheses, or `begin ... end` blocks
  to appear anywhere anyway.
* The grammar category `id` has no right hand side: this does not
  mean that it can match only the empty string, since as mentioned any
  sequence of sexps can appear anywhere anyway.
* Because non terminals cannot appear consecutively in the BNF grammar, it
  is difficult to correctly handle tokens that act as terminators, so the
  above grammar treats `";"` as a statement *separator* instead,
  which SMIE can handle very well.
* Separators used in sequences (such as `","` and `";"` above)
  are best defined with BNF rules such as `(foo (foo "separator" foo) ...)`
  which generate precedence conflicts which are then resolved by giving
  them an explicit `(assoc "separator")`.
* The `("(" exps ")")` rule was not needed to pair up parens, since
  SMIE will pair up any characters that are marked as having paren syntax
  in the syntax table. What this rule does instead (together with the
  definition of `exps`) is to make it clear that `","` should
  not appear outside of parentheses.
* Rather than have a single *precs* table to resolve conflicts, it is
  preferable to have several tables, so as to let the BNF part of the
  grammar specify relative precedences where possible.
* Unless there is a very good reason to prefer `left` or
  `right`, it is usually preferable to mark operators as associative,
  using `assoc`. For that reason `"+"` and `"*"` are
  defined above as `assoc`, although the language defines them
  formally as left associative.

---

Next: [Living With a Weak Parser](#SMIE-Tricks), Previous: [Defining the Grammar of a Language](#SMIE-Grammar), Up: [Simple Minded Indentation Engine](#SMIE)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1.4 Defining Tokens [¶](#Defining-Tokens)

SMIE comes with a predefined lexical analyzer which uses syntax tables
in the following way: any sequence of characters that have word or
symbol syntax is considered a token, and so is any sequence of
characters that have punctuation syntax. This default lexer is
often a good starting point but is rarely actually correct for any given
language. For example, it will consider `"2,+3"` to be composed
of 3 tokens: `"2"`, `",+"`, and `"3"`.

To describe the lexing rules of your language to SMIE, you need
2 functions, one to fetch the next token, and another to fetch the
previous token. Those functions will usually first skip whitespace and
comments and then look at the next chunk of text to see if it
is a special token. If so it should skip the token and
return a description of this token. Usually this is simply the string
extracted from the buffer, but it can be anything you want.
For example:

```
(defvar sample-keywords-regexp
  (regexp-opt '("+" "*" "," ";" ">" ">=" "<" "<=" ":=" "=")))
```

```
(defun sample-smie-forward-token ()
  (forward-comment (point-max))
  (cond
   ((looking-at sample-keywords-regexp)
    (goto-char (match-end 0))
    (match-string-no-properties 0))
   (t (buffer-substring-no-properties
       (point)
       (progn (skip-syntax-forward "w_")
              (point))))))
```

```
(defun sample-smie-backward-token ()
  (forward-comment (- (point)))
  (cond
   ((looking-back sample-keywords-regexp (- (point) 2) t)
    (goto-char (match-beginning 0))
    (match-string-no-properties 0))
   (t (buffer-substring-no-properties
       (point)
       (progn (skip-syntax-backward "w_")
              (point))))))
```

Notice how those lexers return the empty string when in front of
parentheses. This is because SMIE automatically takes care of the
parentheses defined in the syntax table. More specifically if the lexer
returns `nil` or an empty string, SMIE tries to handle the corresponding
text as a sexp according to syntax tables.

---

Next: [Specifying Indentation Rules](#SMIE-Indentation), Previous: [Defining Tokens](#SMIE-Lexer), Up: [Simple Minded Indentation Engine](#SMIE)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1.5 Living With a Weak Parser [¶](#Living-With-a-Weak-Parser)

The parsing technique used by SMIE does not allow tokens to behave
differently in different contexts. For most programming languages, this
manifests itself by precedence conflicts when converting the
BNF grammar.

Sometimes, those conflicts can be worked around by expressing the
grammar slightly differently. For example, for Modula-2 it might seem
natural to have a BNF grammar that looks like this:

```
  ...
  (inst ("IF" exp "THEN" insts "ELSE" insts "END")
        ("CASE" exp "OF" cases "END")
        ...)
  (cases (cases "|" cases)
         (caselabel ":" insts)
         ("ELSE" insts))
  ...
```

But this will create conflicts for `"ELSE"`: on the one hand, the
IF rule implies (among many other things) that `"ELSE" = "END"`;
but on the other hand, since `"ELSE"` appears within `cases`,
which appears left of `"END"`, we also have `"ELSE" > "END"`.
We can solve the conflict either by using:

```
  ...
  (inst ("IF" exp "THEN" insts "ELSE" insts "END")
        ("CASE" exp "OF" cases "END")
        ("CASE" exp "OF" cases "ELSE" insts "END")
        ...)
  (cases (cases "|" cases) (caselabel ":" insts))
  ...
```

or

```
  ...
  (inst ("IF" exp "THEN" else "END")
        ("CASE" exp "OF" cases "END")
        ...)
  (else (insts "ELSE" insts))
  (cases (cases "|" cases) (caselabel ":" insts) (else))
  ...
```

Reworking the grammar to try and solve conflicts has its downsides, tho,
because SMIE assumes that the grammar reflects the logical structure of
the code, so it is preferable to keep the BNF closer to the intended
abstract syntax tree.

Other times, after careful consideration you may conclude that those
conflicts are not serious and simply resolve them via the
resolvers argument of `smie-bnf->prec2`. Usually this is
because the grammar is simply ambiguous: the conflict does not affect
the set of programs described by the grammar, but only the way those
programs are parsed. This is typically the case for separators and
associative infix operators, where you want to add a resolver like
`'((assoc "|"))`. Another case where this can happen is for the
classic *dangling else* problem, where you will use `'((assoc
"else" "then"))`. It can also happen for cases where the conflict is
real and cannot really be resolved, but it is unlikely to pose a problem
in practice.

Finally, in many cases some conflicts will remain despite all efforts to
restructure the grammar. Do not despair: while the parser cannot be
made more clever, you can make the lexer as smart as you want. So, the
solution is then to look at the tokens involved in the conflict and to
split one of those tokens into 2 (or more) different tokens. E.g., if
the grammar needs to distinguish between two incompatible uses of the
token `"begin"`, make the lexer return different tokens (say
`"begin-fun"` and `"begin-plain"`) depending on which kind of
`"begin"` it finds. This pushes the work of distinguishing the
different cases to the lexer, which will thus have to look at the
surrounding text to find ad-hoc clues.

---

Next: [Helper Functions for Indentation Rules](#SMIE-Indentation-Helpers), Previous: [Living With a Weak Parser](#SMIE-Tricks), Up: [Simple Minded Indentation Engine](#SMIE)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1.6 Specifying Indentation Rules [¶](#Specifying-Indentation-Rules)

Based on the provided grammar, SMIE will be able to provide automatic
indentation without any extra effort. But in practice, this default
indentation style will probably not be good enough. You will want to
tweak it in many different cases.

SMIE indentation is based on the idea that indentation rules should be
as local as possible. To this end, it relies on the idea of
*virtual* indentation, which is the indentation that a particular
program point would have if it were at the beginning of a line.
Of course, if that program point is indeed at the beginning of a line,
its virtual indentation is its current indentation. But if not, then
SMIE uses the indentation algorithm to compute the virtual indentation
of that point. Now in practice, the virtual indentation of a program
point does not have to be identical to the indentation it would have if
we inserted a newline before it. To see how this works, the SMIE rule
for indentation after a `{` in C does not care whether the
`{` is standing on a line of its own or is at the end of the
preceding line. Instead, these different cases are handled in the
indentation rule that decides how to indent before a `{`.

Another important concept is the notion of *parent*: The
*parent* of a token, is the head token of the nearest enclosing
syntactic construct. For example, the parent of an `else` is the
`if` to which it belongs, and the parent of an `if`, in turn,
is the lead token of the surrounding construct. The command
`backward-sexp` jumps from a token to its parent, but there are
some caveats: for *openers* (tokens which start a construct, like
`if`), you need to start with point before the token, while for
others you need to start with point after the token.
`backward-sexp` stops with point before the parent token if that is
the *opener* of the token of interest, and otherwise it stops with
point after the parent token.

SMIE indentation rules are specified using a function that takes two
arguments method and arg where the meaning of arg and the
expected return value depend on method.

method can be:

* `:after`, in which case arg is a token and the function
  should return the offset to use for indentation after arg.
* `:before`, in which case arg is a token and the function
  should return the offset to use to indent arg itself.
* `:elem`, in which case the function should return either the offset
  to use to indent function arguments (if arg is the symbol
  `args`) or the basic indentation step (if arg is the symbol
  `basic`).
* `:list-intro`, in which case arg is a token and the function
  should return non-`nil` if the token is followed by a list of
  expressions (not separated by any token) rather than an expression.

When arg is a token, the function is called with point just before
that token. A return value of `nil` always means to fallback on the
default behavior, so the function should return `nil` for arguments it
does not expect.

offset can be:

* `nil`: use the default indentation rule.
* `(column . column)`: indent to column column.
* number: offset by number, relative to a base token which is
  the current token for `:after` and its parent for `:before`.

---

Next: [Sample Indentation Rules](#SMIE-Indentation-Example), Previous: [Specifying Indentation Rules](#SMIE-Indentation), Up: [Simple Minded Indentation Engine](#SMIE)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1.7 Helper Functions for Indentation Rules [¶](#Helper-Functions-for-Indentation-Rules)

SMIE provides various functions designed specifically for use in the
indentation rules function (several of those functions break if used in
another context). These functions all start with the prefix
`smie-rule-`.

Function: **smie-rule-bolp** [¶](#index-smie_002drule_002dbolp)
:   Return non-`nil` if the current token is the first on the line.

Function: **smie-rule-hanging-p** [¶](#index-smie_002drule_002dhanging_002dp)
:   Return non-`nil` if the current token is *hanging*.
    A token is *hanging* if it is the last token on the line
    and if it is preceded by other tokens: a lone token on a line is not
    hanging.

Function: **smie-rule-next-p** &rest tokens [¶](#index-smie_002drule_002dnext_002dp)
:   Return non-`nil` if the next token is among tokens.

Function: **smie-rule-prev-p** &rest tokens [¶](#index-smie_002drule_002dprev_002dp)
:   Return non-`nil` if the previous token is among tokens.

Function: **smie-rule-parent-p** &rest parents [¶](#index-smie_002drule_002dparent_002dp)
:   Return non-`nil` if the current token’s parent is among parents.

Function: **smie-rule-sibling-p** [¶](#index-smie_002drule_002dsibling_002dp)
:   Return non-`nil` if the current token’s parent is actually a
    sibling. This is the case for example when the parent of a `","`
    is just the previous `","`.

Function: **smie-rule-parent** &optional offset [¶](#index-smie_002drule_002dparent)
:   Return the proper offset to align the current token with the parent.
    If non-`nil`, offset should be an integer giving an
    additional offset to apply.

Function: **smie-rule-separator** method [¶](#index-smie_002drule_002dseparator)
:   Indent current token as a *separator*.

    By *separator*, we mean here a token whose sole purpose is to
    separate various elements within some enclosing syntactic construct, and
    which does not have any semantic significance in itself (i.e., it would
    typically not exist as a node in an abstract syntax tree).

    Such a token is expected to have an associative syntax and be closely
    tied to its syntactic parent. Typical examples are `","` in lists
    of arguments (enclosed inside parentheses), or `";"` in sequences
    of instructions (enclosed in a `{...}` or `begin...end`
    block).

    method should be the method name that was passed to
    `smie-rules-function`.

---

Next: [Customizing Indentation](#SMIE-Customization), Previous: [Helper Functions for Indentation Rules](#SMIE-Indentation-Helpers), Up: [Simple Minded Indentation Engine](#SMIE)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1.8 Sample Indentation Rules [¶](#Sample-Indentation-Rules)

Here is an example of an indentation function:

```
(defun sample-smie-rules (kind token)
  (pcase (cons kind token)
    (`(:elem . basic) sample-indent-basic)
    (`(,_ . ",") (smie-rule-separator kind))
    (`(:after . ":=") sample-indent-basic)
    (`(:before . ,(or `"begin" `"(" `"{"))
     (if (smie-rule-hanging-p) (smie-rule-parent)))
    (`(:before . "if")
     (and (not (smie-rule-bolp)) (smie-rule-prev-p "else")
          (smie-rule-parent)))))
```

A few things to note:

* The first case indicates the basic indentation increment to use.
  If `sample-indent-basic` is `nil`, then SMIE uses the global
  setting `smie-indent-basic`. The major mode could have set
  `smie-indent-basic` buffer-locally instead, but that
  is discouraged.
* The rule for the token `","` make SMIE try to be more clever when
  the comma separator is placed at the beginning of lines. It tries to
  outdent the separator so as to align the code after the comma; for
  example:

  ```
  x = longfunctionname (
          arg1
        , arg2
      );
  ```
* The rule for indentation after `":="` exists because otherwise
  SMIE would treat `":="` as an infix operator and would align the
  right argument with the left one.
* The rule for indentation before `"begin"` is an example of the use
  of virtual indentation: This rule is used only when `"begin"` is
  hanging, which can happen only when `"begin"` is not at the
  beginning of a line. So this is not used when indenting
  `"begin"` itself but only when indenting something relative to this
  `"begin"`. Concretely, this rule changes the indentation from:

  ```
      if x > 0 then begin
              dosomething(x);
          end
  ```

  to

  ```
      if x > 0 then begin
          dosomething(x);
      end
  ```
* The rule for indentation before `"if"` is similar to the one for
  `"begin"`, but where the purpose is to treat `"else if"`
  as a single unit, so as to align a sequence of tests rather than indent
  each test further to the right. This function does this only in the
  case where the `"if"` is not placed on a separate line, hence the
  `smie-rule-bolp` test.

  If we know that the `"else"` is always aligned with its `"if"`
  and is always at the beginning of a line, we can use a more efficient
  rule:

  ```
  ((equal token "if")
   (and (not (smie-rule-bolp))
        (smie-rule-prev-p "else")
        (save-excursion
          (sample-smie-backward-token)
          (cons 'column (current-column)))))
  ```

  The advantage of this formulation is that it reuses the indentation of
  the previous `"else"`, rather than going all the way back to the
  first `"if"` of the sequence.

---

Previous: [Sample Indentation Rules](#SMIE-Indentation-Example), Up: [Simple Minded Indentation Engine](#SMIE)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.1.9 Customizing Indentation [¶](#Customizing-Indentation)

If you are using a mode whose indentation is provided by SMIE, you can
customize the indentation to suit your preferences. You can do this
on a per-mode basis (using the option `smie-config`), or a
per-file basis (using the function `smie-config-local` in a
file-local variable specification).

User Option: **smie-config** [¶](#index-smie_002dconfig)
:   This option lets you customize indentation on a per-mode basis.
    It is an alist with elements of the form `(mode . rules)`.
    For the precise form of rules, see the variable’s documentation; but
    you may find it easier to use the command `smie-config-guess`.

Command: **smie-config-guess** [¶](#index-smie_002dconfig_002dguess)
:   This command tries to work out appropriate settings to produce
    your preferred style of indentation. Simply call the command while
    visiting a file that is indented with your style.

Command: **smie-config-save** [¶](#index-smie_002dconfig_002dsave)
:   Call this command after using `smie-config-guess`, to save your
    settings for future sessions.

Command: **smie-config-show-indent** &optional move [¶](#index-smie_002dconfig_002dshow_002dindent)
:   This command displays the rules that are used to indent the current
    line.

Command: **smie-config-set-indent** [¶](#index-smie_002dconfig_002dset_002dindent)
:   This command adds a local rule to adjust the indentation of the current line.

Function: **smie-config-local** rules [¶](#index-smie_002dconfig_002dlocal)
:   This function adds rules as indentation rules for the current buffer.
    These add to any mode-specific rules defined by the `smie-config` option.
    To specify custom indentation rules for a specific file, add an entry
    to the file’s local variables of the form:
    `eval: (smie-config-local '(rules))`.

---

Previous: [Simple Minded Indentation Engine](#SMIE), Up: [Automatic Indentation of code](#Auto_002dIndentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.8.2 Parser-based Indentation [¶](#Parser_002dbased-Indentation-1)

When built with the tree-sitter library (see [Parsing Program Source](#Parsing-Program-Source)), Emacs is capable of parsing the program source and producing
a syntax tree. This syntax tree can be used for guiding the program
source indentation commands. For maximum flexibility, it is possible
to write a custom indentation function that queries the syntax tree
and indents accordingly for each language, but that is a lot of work.
It is more convenient to use the simple indentation engine described
below: then the major mode needs only write some indentation rules,
and the engine takes care of the rest.

To enable the parser-based indentation engine, set either
`treesit-simple-indent-rules` or `treesit-indent-function`,
then call `treesit-major-mode-setup`. (All that
`treesit-major-mode-setup` does is set the value of
`indent-line-function` to `treesit-indent`, and
`indent-region-function` to `treesit-indent-region`.)

Variable: **treesit-indent-function** [¶](#index-treesit_002dindent_002dfunction)
:   This variable stores the actual function called by
    `treesit-indent`. By default, its value is
    `treesit-simple-indent`. In the future we might add other,
    more complex indentation engines.

### Writing indentation rules [¶](#Writing-indentation-rules)

Variable: **treesit-simple-indent-rules** [¶](#index-treesit_002dsimple_002dindent_002drules)
:   This local variable stores indentation rules for every language. It is
    an list of elements of the form `(language rule…)`, where language is a language symbol, and each
    rule is either a list with elements of the form
    `(matcher anchor offset)`, or a function.

    Here’s the description of the list variant, followed by the function
    variant.

    First, Emacs passes the smallest tree-sitter node at the beginning of
    the current line to matcher; if it returns non-`nil`, this
    rule is applicable. Then Emacs passes the node to anchor, which
    returns a buffer position. Emacs takes the column number of that
    position, adds offset to it, and the result is the indentation
    column for the current line.

    The matcher and anchor are functions, and Emacs provides
    convenient defaults for them.

    Each matcher or anchor is a function that takes three
    arguments: node, parent, and bol. The argument
    bol is the buffer position whose indentation is required: the
    position of the first non-whitespace character after the beginning of
    the line. The argument node is the largest node that starts at
    that position (and is not a root node); and parent is the parent
    of node. However, when that position is in a whitespace or
    inside a multi-line string, no node can start at that position, so
    node is `nil`. In that case, parent would be the
    smallest node that spans that position.

    matcher should return non-`nil` if the rule is applicable,
    and anchor should return a buffer position.

    offset can be an integer, a variable whose value is an integer,
    or a function that returns an integer. If it is a function, it is
    passed node, parent, and bol, like matchers and
    anchors.

    If ruleis a function, it is useful for the complex cases where a
    rule needs to consider the matching rule and the anchor together. The
    rule function is passed the same argument as matcher:
    node, parent, and bol. If it matches, rule
    should return a cons `(anchor-pos . offset)`, where
    anchor-pos is a buffer position, and offset is the indent
    offset. If rule doesn’t match, it should return `nil`.

Variable: **treesit-simple-indent-presets** [¶](#index-treesit_002dsimple_002dindent_002dpresets)
:   This is a list of defaults for matchers and anchors in
    `treesit-simple-indent-rules`. Each of them represents a
    function that takes 3 arguments: node, parent, and
    bol. The available default functions are:

    `no-node` [¶](#index-no_002dnode)
    :   This matcher is a function that is called with 3 arguments:
        node, parent, and bol. It returns non-`nil`,
        indicating a match, if node is `nil`, i.e., there is no
        node that starts at bol. This is the case when bol is on
        an empty line or inside a multi-line string, etc.

    `parent-is` [¶](#index-parent_002dis)
    :   This matcher is a function of one argument, type; it returns a
        function that is called with 3 arguments: node, parent,
        and bol, and returns non-`nil` (i.e., a match) if
        parent’s type matches regexp type.

    `node-is` [¶](#index-node_002dis)
    :   This matcher is a function of one argument, type; it returns a
        function that is called with 3 arguments: node, parent,
        and bol, and returns non-`nil` if node’s type matches
        regexp type.

    `field-is` [¶](#index-field_002dis)
    :   This matcher is a function of one argument, name; it returns a
        function that is called with 3 arguments: node, parent,
        and bol, and returns non-`nil` if node’s field name
        in parent matches regexp name.

    `query` [¶](#index-query)
    :   This matcher is a function of one argument, query; it returns a
        function that is called with 3 arguments: node, parent,
        and bol, and returns non-`nil` if querying parent
        with query captures node (see [Pattern Matching Tree-sitter Nodes](#Pattern-Matching)).

    `match` [¶](#index-match)
    :   This matcher is a function of 5 arguments: node-type,
        parent-type, node-field, node-index-min, and
        node-index-max). It returns a function that is called with 3
        arguments: node, parent, and bol, and returns
        non-`nil` if node’s type matches regexp node-type,
        parent’s type matches regexp parent-type, node’s
        field name in parent matches regexp node-field, and
        node’s index among its siblings is between node-index-min
        and node-index-max. If the value of an argument is `nil`,
        this matcher doesn’t check that argument. For example, to match the
        first child where parent is `argument_list`, use

        ```
        (match nil "argument_list" nil 0 0)
        ```

        In addition, node-type can be a special value `null`,
        which matches when the value of node is `nil`.

    `n-p-gp` [¶](#index-n_002dp_002dgp)
    :   Short for “node-parent-grandparent”, this matcher is a function of 3
        arguments: node-type, parent-type, and
        grandparent-type. It returns a function that is called with 3
        arguments: node, parent, and bol, and returns
        non-`nil` if: (1) node-type matches node’s type, and
        (2) parent-type matches parent’s type, and (3)
        grandparent-type matches parent’s parent’s type. If any
        of node-type, parent-type, and grandparent-type is
        `nil`, this function doesn’t check for it.

    `comment-end` [¶](#index-comment_002dend)
    :   This matcher is a function that is called with 3 arguments:
        node, parent, and bol, and returns non-`nil` if
        point is before a comment-ending token. Comment-ending tokens are
        defined by regexp `comment-end-skip`.

    `catch-all` [¶](#index-catch_002dall)
    :   This matcher is a function that is called with 3 arguments:
        node, parent, and bol. It always returns
        non-`nil`, indicating a match.

    `first-sibling` [¶](#index-first_002dsibling)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the start of the first child
        of parent.

    `nth-sibling` [¶](#index-nth_002dsibling)
    :   This anchor is a function of two arguments: n, and an optional
        argument named. It returns a function that is called with 3
        arguments: node, parent, and bol, and returns the
        start of the nth child of parent. If named is
        non-`nil`, only named children are counted (see [named node](#tree_002dsitter-named-node)).

    `parent` [¶](#index-parent)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the start of parent.

    `grand-parent` [¶](#index-grand_002dparent)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the start of parent’s
        parent.

    `great-grand-parent` [¶](#index-great_002dgrand_002dparent)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the start of parent’s
        parent’s parent.

    `parent-bol` [¶](#index-parent_002dbol)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the first non-space character
        on the line which parent’s start is on.

    `standalone-parent` [¶](#index-standalone_002dparent)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol. It finds the first ancestor node (parent,
        grandparent, etc.) of node that starts on its own line, and
        return the start of that node. “Starting on its own line” means there
        is only whitespace character before the node on the line which the
        node’s start is on. The exact definition of “Starting on its own
        line” can be relaxed by setting
        `treesit-simple-indent-standalone-predicate`, some major mode might
        want to do that for easier indentation for method chaining.

    `prev-sibling` [¶](#index-prev_002dsibling)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the start of the previous
        sibling of node.

    `no-indent` [¶](#index-no_002dindent)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the start of node.

    `prev-line` [¶](#index-prev_002dline)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the first non-whitespace
        character on the previous line.

    `column-0` [¶](#index-column_002d0)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the beginning of the current
        line, which is at column 0.

    `comment-start` [¶](#index-comment_002dstart)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol, and returns the position after the
        comment-start token. Comment-start tokens are defined by regular
        expression `comment-start-skip`. This function assumes
        parent is the comment node.

    `prev-adaptive-prefix` [¶](#index-prev_002dadaptive_002dprefix)
    :   This anchor is a function that is called with 3 arguments: node,
        parent, and bol. It tries to match
        `adaptive-fill-regexp` to the text at the beginning of the
        previous non-empty line. If there is a match, this function returns
        the end of the match, otherwise it returns `nil`. However, if
        the current line begins with a prefix (e.g., ‘`-`’), return the
        beginning of the prefix of the previous line instead, so that the two
        prefixes align. This anchor is useful for an
        `indent-relative`-like indent behavior for block comments.

### Indentation utilities [¶](#Indentation-utilities)

Here are some utility functions that can help writing parser-based
indentation rules.

Command: **treesit-check-indent** mode [¶](#index-treesit_002dcheck_002dindent)
:   This command checks the current buffer’s indentation against major
    mode mode. It indents the current buffer according to
    mode and compares the results with the current indentation.
    Then it pops up a buffer showing the differences. Correct
    indentation (target) is shown in green color, current indentation is
    shown in red color.

It is also helpful to use `treesit-inspect-mode` (see [Tree-sitter Language Grammar](#Language-Grammar)) when writing indentation rules.

---

Previous: [Automatic Indentation of code](#Auto_002dIndentation), Up: [Major and Minor Modes](#Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 24.9 Desktop Save Mode [¶](#Desktop-Save-Mode-1)

*Desktop Save Mode* is a feature to save the state of Emacs from
one session to another. The user-level commands for using Desktop
Save Mode are described in the GNU Emacs Manual (see [Saving Emacs
Sessions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Saving-Emacs-Sessions) in the GNU Emacs Manual). Modes whose buffers visit
a file, don’t have to do anything to use this feature.

For buffers not visiting a file to have their state saved, the major
mode must bind the buffer local variable `desktop-save-buffer` to
a non-`nil` value.

Variable: **desktop-save-buffer** [¶](#index-desktop_002dsave_002dbuffer)
:   If this buffer-local variable is non-`nil`, the buffer will have
    its state saved in the desktop file at desktop save. If the value is
    a function, it is called at desktop save with argument
    desktop-dirname, and its value is saved in the desktop file along
    with the state of the buffer for which it was called. When file names
    are returned as part of the auxiliary information, they should be
    formatted using the call

    ```
    (desktop-file-name file-name desktop-dirname)
    ```

For buffers not visiting a file to be restored, the major mode must
define a function to do the job, and that function must be listed in
the alist `desktop-buffer-mode-handlers`.

Variable: **desktop-buffer-mode-handlers** [¶](#index-desktop_002dbuffer_002dmode_002dhandlers)
:   Alist with elements

    ```
    (major-mode . restore-buffer-function)
    ```

    The function restore-buffer-function will be called with
    argument list

    ```
    (buffer-file-name buffer-name desktop-buffer-misc)
    ```

    and it should return the restored buffer.
    Here desktop-buffer-misc is the value returned by the function
    optionally bound to `desktop-save-buffer`.

---

Next: [Files](#Files), Previous: [Major and Minor Modes](#Modes), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 25 Documentation [¶](#Documentation-1)

GNU Emacs has convenient built-in help facilities, most of which
derive their information from documentation strings associated with
functions and variables. This chapter describes how to access
documentation strings in Lisp programs.

The contents of a documentation string should follow certain
conventions. In particular, its first line should be a complete
sentence (or two complete sentences) that briefly describes what the
function or variable does. See [Tips for Documentation Strings](#Documentation-Tips), for how to
write good documentation strings.

Note that the documentation strings for Emacs are not the same thing
as the Emacs manual. Manuals have their own source files, written in
the Texinfo language; documentation strings are specified in the
definitions of the functions and variables they apply to. A collection
of documentation strings is not sufficient as a manual because a good
manual is not organized in that fashion; it is organized in terms of
topics of discussion.

For commands to display documentation strings, see [Help](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Help) in The GNU Emacs Manual.

* [Documentation Basics](#Documentation-Basics)
* [Access to Documentation Strings](#Accessing-Documentation)
* [Substituting Key Bindings in Documentation](#Keys-in-Documentation)
* [Text Quoting Style](#Text-Quoting-Style)
* [Describing Characters for Help Messages](#Describing-Characters)
* [Help Functions](#Help-Functions)
* [Documentation Groups](#Documentation-Groups)

---

Next: [Access to Documentation Strings](#Accessing-Documentation), Up: [Documentation](#Documentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 25.1 Documentation Basics [¶](#Documentation-Basics-1)

A documentation string is written using the Lisp syntax for strings,
with double-quote characters surrounding the text. It is, in fact, an
actual Lisp string. When the string appears in the proper place in a
function or variable definition, it serves as the function’s or
variable’s documentation.

In a function definition (a `lambda` or `defun` form), the
documentation string is specified after the argument list, and is
normally stored directly in the function object. See [Documentation Strings of Functions](#Function-Documentation). You can also put function documentation in the
`function-documentation` property of a function name
(see [Access to Documentation Strings](#Accessing-Documentation)).

In a variable definition (a `defvar` form), the documentation
string is specified after the initial value. See [Defining Global Variables](#Defining-Variables). The string is stored in the variable’s
`variable-documentation` property.

Sometimes, Emacs does not keep documentation strings in memory.
There are two such circumstances. Firstly, to save memory, the
documentation for primitive functions (see [What Is a Function?](#What-Is-a-Function)) and
built-in variables is kept in a file named `DOC`, in the
directory specified by `doc-directory` (see [Access to Documentation Strings](#Accessing-Documentation)). Secondly, when a function or variable is loaded from
a byte-compiled file, Emacs avoids loading its documentation string
(see [Documentation Strings and Compilation](#Docs-and-Compilation)). In both cases, Emacs looks up the
documentation string from the file only when needed, such as when the
user calls `C-h f` (`describe-function`) for a function.

Documentation strings can contain special *key substitution
sequences*, referring to key bindings which are looked up only when
the user views the documentation. This allows the help commands to
display the correct keys even if a user rearranges the default key
bindings. See [Substituting Key Bindings in Documentation](#Keys-in-Documentation).

In the documentation string of an autoloaded command
(see [Autoload](#Autoload)), these key-substitution sequences have an
additional special effect: they cause `C-h f` on the command to
trigger autoloading. (This is needed for correctly setting up the
hyperlinks in the `*Help*` buffer.)

---

Next: [Substituting Key Bindings in Documentation](#Keys-in-Documentation), Previous: [Documentation Basics](#Documentation-Basics), Up: [Documentation](#Documentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 25.2 Access to Documentation Strings [¶](#Access-to-Documentation-Strings)

Function: **documentation-property** symbol property &optional verbatim [¶](#index-documentation_002dproperty)
:   This function returns the documentation string recorded in
    symbol’s property list under property property. It is
    most often used to look up the documentation strings of variables, for
    which property is `variable-documentation`. However, it
    can also be used to look up other kinds of documentation, such as for
    customization groups (but for function documentation, use the
    `documentation` function, below).

    If the property value refers to a documentation string stored in the
    `DOC` file or a byte-compiled file, this function looks up that
    string and returns it.

    If the property value isn’t `nil`, isn’t a string, and doesn’t
    refer to text in a file, then it is evaluated as a Lisp expression to
    obtain a string.

    Finally, this function passes the string through
    `substitute-command-keys` to substitute key bindings (see [Substituting Key Bindings in Documentation](#Keys-in-Documentation)). It skips this step if verbatim is
    non-`nil`.

    ```
    (documentation-property 'command-line-processed
       'variable-documentation)
         ⇒ "Non-nil once command line has been processed"
    ```

    ```
    (symbol-plist 'command-line-processed)
         ⇒ (variable-documentation 188902)
    ```

    ```
    (documentation-property 'emacs 'group-documentation)
         ⇒ "Customization of the One True Editor."
    ```

Function: **documentation** function &optional verbatim [¶](#index-documentation)
:   This function returns the documentation string of function. It
    handles macros, named keyboard macros, and special forms, as well as
    ordinary functions.

    If function is a symbol, this function first looks for the
    `function-documentation` property of that symbol; if that has a
    non-`nil` value, the documentation comes from that value (if the
    value is not a string, it is evaluated).

    If function is not a symbol, or if it has no
    `function-documentation` property, then `documentation`
    extracts the documentation string from the actual function definition,
    reading it from a file if called for.

    Finally, unless verbatim is non-`nil`, this function calls
    `substitute-command-keys`. The result is the documentation
    string to return.

    The `documentation` function signals a `void-function` error
    if function has no function definition. However, it is OK if
    the function definition has no documentation string. In that case,
    `documentation` returns `nil`.

Function: **function-documentation** function [¶](#index-function_002ddocumentation)
:   Generic function used by `documentation` to extract the raw
    docstring from a function object. You can specify how to get the
    docstring of a specific function type by adding a corresponding method
    to it.

Function: **face-documentation** face [¶](#index-face_002ddocumentation)
:   This function returns the documentation string of face as a
    face.

Here is an example of using the two functions, `documentation` and
`documentation-property`, to display the documentation strings for
several symbols in a `*Help*` buffer.

```
(defun describe-symbols (pattern)
  "Describe the Emacs Lisp symbols matching PATTERN.
All symbols that have PATTERN in their name are described
in the *Help* buffer."
  (interactive "sDescribe symbols matching: ")
  (let ((describe-func
         (lambda (s)
```

```
           ;; Print description of symbol.
           (if (fboundp s)             ; It is a function.
               (princ
                (format "%s\t%s\n%s\n\n" s
                  (if (commandp s)
                      (let ((keys (where-is-internal s)))
                        (if keys
                            (concat
                             "Keys: "
                             (mapconcat 'key-description
                                        keys " "))
                          "Keys: none"))
                    "Function")
```

```
                  (or (documentation s)
                      "not documented"))))

           (if (boundp s)              ; It is a variable.
```

```
               (princ
                (format "%s\t%s\n%s\n\n" s
                  (if (custom-variable-p s)
                      "Option " "Variable")
```

```
                  (or (documentation-property
                        s 'variable-documentation)
                      "not documented"))))))
        sym-list)
```

```

```

```
    ;; Build a list of symbols that match pattern.
    (mapatoms (lambda (sym)
                (if (string-match pattern (symbol-name sym))
                    (setq sym-list (cons sym sym-list)))))
```

```

```

```
    ;; Display the data.
    (help-setup-xref (list 'describe-symbols pattern)
                 (called-interactively-p 'interactive))
    (with-help-window (help-buffer)
      (mapcar describe-func (sort sym-list)))))
```

The `describe-symbols` function works like `apropos`,
but provides more information.

```
(describe-symbols "goal")

---------- Buffer: *Help* ----------
goal-column     Option
Semipermanent goal column for vertical motion, as set by ...
```

```

```

```
minibuffer-temporary-goal-position      Variable
not documented
```

```

```

```
set-goal-column Keys: C-x C-n
Set the current horizontal position as a goal for C-n and C-p.
```

```
Those commands will move to this position in the line moved to
rather than trying to keep the same horizontal position.
With a non-nil argument ARG, clears out the goal column
so that C-n and C-p resume vertical motion.
The goal column is stored in the variable `goal-column'.

(fn ARG)
```

```

```

```
temporary-goal-column   Variable
Current goal column for vertical motion.
It is the column where point was at the start of the current run
of vertical motion commands.

When moving by visual lines via the function `line-move-visual', it is a cons
cell (COL . HSCROLL), where COL is the x-position, in pixels,
divided by the default column width, and HSCROLL is the number of
columns by which window is scrolled from left margin.

When the `track-eol' feature is doing its job, the value is
`most-positive-fixnum'.
---------- Buffer: *Help* ----------
```

Function: **Snarf-documentation** filename [¶](#index-Snarf_002ddocumentation)
:   This function is used when building Emacs, just before the runnable
    Emacs is dumped. It finds the positions of the documentation strings
    stored in the file filename, and records those positions into
    memory in the function definitions and variable property lists.
    See [Building Emacs](#Building-Emacs).

    Emacs reads the file filename from the `emacs/etc` directory.
    When the dumped Emacs is later executed, the same file will be looked
    for in the directory `doc-directory`. Usually filename is
    `"DOC"`.

Variable: **doc-directory** [¶](#index-doc_002ddirectory)
:   This variable holds the name of the directory which should contain the
    file `"DOC"` that contains documentation strings for
    built-in functions and variables.

    In most cases, this is the same as `data-directory`. They may be
    different when you run Emacs from the directory where you built it,
    without actually installing it. See [Definition of data-directory](#Definition-of-data_002ddirectory).

---

Next: [Text Quoting Style](#Text-Quoting-Style), Previous: [Access to Documentation Strings](#Accessing-Documentation), Up: [Documentation](#Documentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 25.3 Substituting Key Bindings in Documentation [¶](#Substituting-Key-Bindings-in-Documentation)

When documentation strings refer to key sequences, they should use the
current, actual key bindings. They can do so using certain special text
sequences described below. Accessing documentation strings in the usual
way substitutes current key binding information for these special
sequences. This works by calling `substitute-command-keys`. You
can also call that function yourself.

Here is a list of the special sequences and what they mean:

`\[command]`
:   stands for a key sequence that will invoke command, or ‘`M-x
    command`’ if command has no key bindings.

`\{mapvar}`
:   stands for a summary of the keymap which is the value of the variable
    mapvar. The summary is made using `describe-bindings`.
    The summary will normally exclude menu bindings, but if the
    include-menus argument to `substitute-command-keys` is
    non-`nil`, the menu bindings will be included.

`\<mapvar>`
:   stands for no text itself. It is used only for a side effect: it
    specifies mapvar’s value as the keymap for any following
    ‘`\[command]`’ sequences in this documentation string.

`` \`KEYSEQ' ``
:   stands for a key sequence KEYSEQ, which will use the same face
    as a command substitution. This should be used only when a key
    sequence has no corresponding command, for example when it is read
    directly with `read-key-sequence`. It must be a valid key
    sequence according to `key-valid-p`. It can also be used with
    command names, like ‘`` \`M-x foo' ``’, where you want this to be
    fontified like a keyboard sequence, but you want to inhibit
    translating it into a key sequence like ‘`\[foo]`’ does.

`` ` ``
:   (grave accent) stands for a left quote.
    This generates a left single quotation mark, an apostrophe, or a grave
    accent depending on the value of `text-quoting-style`.
    See [Text Quoting Style](#Text-Quoting-Style).

`'`
:   (apostrophe) stands for a right quote.
    This generates a right single quotation mark or an apostrophe
    depending on the value of `text-quoting-style`.

`\=`
:   quotes the following character and is discarded; thus, ‘`` \=` ``’ puts
    ‘`` ` ``’ into the output, ‘`\=\[`’ puts ‘`\[`’ into the output,
    and ‘`\=\=`’ puts ‘`\=`’ into the output.

`\+`
:   This indicates that the symbol directly following should not be marked
    as a link in the `*Help*` buffer.

**Please note:** Each ‘`\`’ must be doubled when written in a
string in Emacs Lisp (see [Syntax for Strings](#Syntax-for-Strings)).

Function: **substitute-command-keys** string &optional no-face include-menus [¶](#index-substitute_002dcommand_002dkeys)
:   This function scans string for the above special sequences and
    replaces them by what they stand for, returning the result as a string.
    This permits display of documentation that refers accurately to the
    user’s own customized key bindings. By default, the key bindings are
    given a special face `help-key-binding`, but if the optional
    argument no-face is non-`nil`, the function doesn’t add
    this face to the produced string.

    If a command has multiple bindings, this function normally uses the
    first one it finds. You can specify one particular key binding by
    assigning an `:advertised-binding` symbol property to the
    command, like this:

    ```
    (put 'undo :advertised-binding [?\C-/])
    ```

    The `:advertised-binding` property also affects the binding shown
    in menu items (see [The Menu Bar](#Menu-Bar)). The property is ignored if it
    specifies a key binding that the command does not actually have.

Here are examples of the special sequences:

```
(substitute-command-keys
   "To abort recursive edit, type `\\[abort-recursive-edit]'.")
⇒ "To abort recursive edit, type ‘C-]’."
```

```

```

```
(substitute-command-keys
   "The keys that are defined for the minibuffer here are:
  \\{minibuffer-local-must-match-map}")
⇒ "The keys that are defined for the minibuffer here are:
```

```
?               minibuffer-completion-help
SPC             minibuffer-complete-word
TAB             minibuffer-complete
C-j             minibuffer-complete-and-exit
RET             minibuffer-complete-and-exit
C-g             abort-recursive-edit
"

The keymap description will normally exclude menu items, but if
include-menus is non-nil, include them.
```

```
(substitute-command-keys
   "To abort a recursive edit from the minibuffer, type \
`\\<minibuffer-local-must-match-map>\\[abort-recursive-edit]'.")
⇒ "To abort a recursive edit from the minibuffer, type ‘C-g’."
```

Function: **substitute-quotes** string [¶](#index-substitute_002dquotes)
:   This function works like `substitute-command-keys`, but only
    replaces quote characters.

There are other special conventions for the text in documentation
strings—for instance, you can refer to functions, variables, and
sections of this manual. See [Tips for Documentation Strings](#Documentation-Tips), for details.

---

Next: [Describing Characters for Help Messages](#Describing-Characters), Previous: [Substituting Key Bindings in Documentation](#Keys-in-Documentation), Up: [Documentation](#Documentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 25.4 Text Quoting Style [¶](#Text-Quoting-Style-1)

Typically, grave accents and apostrophes are treated specially in
documentation strings and diagnostic messages, and translate to matching
single quotation marks (also called “curved quotes”). For example,
the documentation string `` "Alias for `foo'." `` and the function call
`` (message "Alias for `foo'.") `` both translate to `"Alias for
‘foo’."`. Less commonly, Emacs displays grave accents and apostrophes
as themselves, or as apostrophes only (e.g., `"Alias for 'foo'."`).
Documentation strings and message formats should be written so that
they display well with any of these styles. For example, the
documentation string `"Alias for 'foo'."` is probably not what you
want, as it can display as `"Alias for ’foo’."`, an unusual style in
English.

Sometimes you may need to display a grave accent or apostrophe
without translation, regardless of text quoting style. In a
documentation string, you can do this with escapes. For example, in
the documentation string `` "\\=`(a ,(sin 0)) ==> (a 0.0)" `` the grave
accent is intended to denote Lisp code, so it is escaped and displays
as itself regardless of quoting style. In a call to `message` or
`error`, you can avoid translation by using a format `"%s"`
with an argument that is a call to `format`. For example,
`` (message "%s" (format "`(a ,(sin %S)) ==> (a %S)" x (sin x))) ``
displays a message that starts with grave accent regardless of text
quoting style.

User Option: **text-quoting-style** [¶](#index-text_002dquoting_002dstyle-1)
:   The value of this user option is a symbol that specifies the style
    Emacs should use for single quotes in the wording of help and
    messages. If the option’s value is `curve`, the style is
    `‘like this’` with curved single quotes. If the value is
    `straight`, the style is `'like this'` with straight
    apostrophes. If the value is `grave`, quotes are not translated
    and the style is `` `like this' `` with grave accent and apostrophe, the
    standard style before Emacs version 25. The default value `nil`
    acts like `curve` if curved single quotes seem to be displayable,
    and like `grave` otherwise.

    This option is useful on platforms that have problems with curved
    quotes. You can customize it freely according to your personal
    preference.

Function: **text-quoting-style** [¶](#index-text_002dquoting_002dstyle)
:   You should not read the value of the variable
    `text-quoting-style` directly. Instead, use this function with
    the same name to dynamically compute the correct quoting style on the
    current terminal in the `nil` case described above.

---

Next: [Help Functions](#Help-Functions), Previous: [Text Quoting Style](#Text-Quoting-Style), Up: [Documentation](#Documentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 25.5 Describing Characters for Help Messages [¶](#Describing-Characters-for-Help-Messages)

These functions convert events, key sequences, or characters to
textual descriptions. These descriptions are useful for including
arbitrary text characters or key sequences in messages, because they
convert non-printing and whitespace characters to sequences of printing
characters. The description of a non-whitespace printing character is
the character itself.

Function: **key-description** sequence &optional prefix [¶](#index-key_002ddescription)
:   This function returns a string containing the Emacs standard notation
    for the input events in sequence. If prefix is
    non-`nil`, it is a sequence of input events leading up to
    sequence and is included in the return value. Both arguments
    may be strings, vectors or lists. See [Input Events](#Input-Events), for more
    information about valid events.

    ```
    (key-description [?\M-3 delete])
         ⇒ "M-3 <delete>"
    ```

    ```
    (key-description [delete] "\M-3")
         ⇒ "M-3 <delete>"
    ```

    See also the examples for `single-key-description`, below.

Function: **single-key-description** event &optional no-angles [¶](#index-single_002dkey_002ddescription)
:   This function returns a string describing event in the standard
    Emacs notation for keyboard input. A normal printing character
    appears as itself, but a control character turns into a string
    starting with ‘`C-`’, a meta character turns into a string starting
    with ‘`M-`’, and space, tab, etc., appear as ‘`SPC`’,
    ‘`TAB`’, etc. A function key symbol appears inside angle brackets
    ‘`<…>`’. An event that is a list appears as the name of the
    symbol in the CAR of the list, inside angle brackets.

    If the optional argument no-angles is non-`nil`, the angle
    brackets around function keys and event symbols are omitted; this is
    for compatibility with old versions of Emacs which didn’t use the
    brackets.

    ```
    (single-key-description ?\C-x)
         ⇒ "C-x"
    ```

    ```
    (key-description "\C-x \M-y \n \t \r \f123")
         ⇒ "C-x SPC M-y SPC C-j SPC TAB SPC RET SPC C-l 1 2 3"
    ```

    ```
    (single-key-description 'delete)
         ⇒ "<delete>"
    ```

    ```
    (single-key-description 'C-mouse-1)
         ⇒ "C-<mouse-1>"
    ```

    ```
    (single-key-description 'C-mouse-1 t)
         ⇒ "C-mouse-1"
    ```

Function: **text-char-description** character [¶](#index-text_002dchar_002ddescription)
:   This function returns a string describing character in the
    standard Emacs notation for characters that can appear in
    text—similar to `single-key-description`, except that the
    argument must be a valid character code that passes a
    `characterp` test (see [Character Codes](#Character-Codes)). The function
    produces descriptions of control characters with a leading caret
    (which is how Emacs usually displays control characters in buffers).
    Characters with modifier bits will cause this function to signal an
    error (ASCII characters with the Control modifier are an
    exception, they are represented as control characters).

    ```
    (text-char-description ?\C-c)
         ⇒ "^C"
    ```

    ```
    (text-char-description ?\M-m)
         error→ Wrong type argument: characterp, 134217837
    ```

Command: **read-kbd-macro** string &optional need-vector [¶](#index-read_002dkbd_002dmacro)
:   This function is used mainly for operating on keyboard macros, but it
    can also be used as a rough inverse for `key-description`. You
    call it with a string containing key descriptions, separated by spaces;
    it returns a string or vector containing the corresponding events.
    (This may or may not be a single valid key sequence, depending on what
    events you use; see [Key Sequences](#Key-Sequences).) If need-vector is
    non-`nil`, the return value is always a vector.

---

Next: [Documentation Groups](#Documentation-Groups), Previous: [Describing Characters for Help Messages](#Describing-Characters), Up: [Documentation](#Documentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 25.6 Help Functions [¶](#Help-Functions-1)

Emacs provides a variety of built-in help functions, all accessible to
the user as subcommands of the prefix `C-h`. For more information
about them, see [Help](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Help) in The GNU Emacs Manual. Here
we describe some program-level interfaces to the same information.

Command: **apropos** pattern &optional do-all [¶](#index-apropos)
:   This function finds all meaningful symbols whose names contain a
    match for the apropos pattern pattern. An apropos pattern is
    either a word to match, a space-separated list of words of which at
    least two must match, or a regular expression (if any special regular
    expression characters occur). A symbol is meaningful if it has a
    definition as a function, variable, or face, or has properties.

    The function returns a list of elements that look like this:

    ```
    (symbol score function-doc variable-doc
     plist-doc widget-doc face-doc group-doc)
    ```

    Here, score is an integer measure of how important the symbol
    seems to be as a match. Each of the remaining elements is a
    documentation string, or `nil`, for symbol as a function,
    variable, etc.

    It also displays the symbols in a buffer named `*Apropos*`, each
    with a one-line description taken from the beginning of its
    documentation string.

    If do-all is non-`nil`, or if the user option
    `apropos-do-all` is non-`nil`, then `apropos` also
    shows key bindings for the functions that are found; it also shows
    *all* interned symbols, not just meaningful ones (and it lists
    them in the return value as well).

Variable: **help-map** [¶](#index-help_002dmap)
:   The value of this variable is a local keymap for characters following the
    Help key, `C-h`.

Prefix Command: **help-command** [¶](#index-help_002dcommand)
:   This symbol is not a function; its function definition cell holds the
    keymap known as `help-map`. It is defined in `help.el` as
    follows:

    ```
    (keymap-set global-map (key-description (string help-char)) 'help-command)
    (fset 'help-command help-map)
    ```

User Option: **help-char** [¶](#index-help_002dchar)
:   The value of this variable is the help character—the character that
    Emacs recognizes as meaning Help. By default, its value is 8, which
    stands for `C-h`. When Emacs reads this character, if
    `help-form` is a non-`nil` Lisp expression, it evaluates that
    expression, and displays the result in a window if it is a string.

    Usually the value of `help-form` is `nil`. Then the
    help character has no special meaning at the level of command input, and
    it becomes part of a key sequence in the normal way. The standard key
    binding of `C-h` is a prefix key for several general-purpose help
    features.

    The help character is special after prefix keys, too. If it has no
    binding as a subcommand of the prefix key, it runs
    `describe-prefix-bindings`, which displays a list of all the
    subcommands of the prefix key.

User Option: **help-event-list** [¶](#index-help_002devent_002dlist)
:   The value of this variable is a list of event types that serve as
    alternative help characters. These events are handled just like the
    event specified by `help-char`.

Variable: **help-form** [¶](#index-help_002dform)
:   If this variable is non-`nil`, its value is a form to evaluate
    whenever the character `help-char` is read. If evaluating the form
    produces a string, that string is displayed.

    A command that calls `read-event`, `read-char-choice`,
    `read-char`, `read-char-from-minibuffer`, or
    `y-or-n-p` probably should bind `help-form` to a
    non-`nil` expression while it does input. (The time when you
    should not do this is when `C-h` has some other meaning.)
    Evaluating this expression should result in a string that explains
    what the input is for and how to enter it properly.

    Entry to the minibuffer binds this variable to the value of
    `minibuffer-help-form` (see [Definition of minibuffer-help-form](#Definition-of-minibuffer_002dhelp_002dform)).

Variable: **prefix-help-command** [¶](#index-prefix_002dhelp_002dcommand)
:   This variable holds a function to print help for a prefix key. The
    function is called when the user types a prefix key followed by the help
    character, and the help character has no binding after that prefix. The
    variable’s default value is `describe-prefix-bindings`.

Command: **describe-prefix-bindings** [¶](#index-describe_002dprefix_002dbindings)
:   This function calls `describe-bindings` to display a list of all
    the subcommands of the prefix key of the most recent key sequence. The
    prefix described consists of all but the last event of that key
    sequence. (The last event is, presumably, the help character.)

The following two functions are meant for modes that want to provide
help without relinquishing control, such as the electric modes.
Their names begin with ‘`Helper`’ to distinguish them from the
ordinary help functions.

Command: **Helper-describe-bindings** [¶](#index-Helper_002ddescribe_002dbindings)
:   This command pops up a window displaying a help buffer containing a
    listing of all of the key bindings from both the local and global keymaps.
    It works by calling `describe-bindings`.

Command: **Helper-help** [¶](#index-Helper_002dhelp)
:   This command provides help for the current mode. It prompts the user
    in the minibuffer with the message ‘`Help (Type ? for further
    options)`’, and then provides assistance in finding out what the key
    bindings are, and what the mode is intended for. It returns `nil`.

    This can be customized by changing the map `Helper-help-map`.

Variable: **data-directory** [¶](#index-data_002ddirectory)
:   This variable holds the name of the directory in which Emacs finds
    certain documentation and text files that come with Emacs.

Function: **help-buffer** [¶](#index-help_002dbuffer)
:   This function returns the name of the help buffer, which is normally
    `*Help*`; if such a buffer does not exist, it is first created.

Macro: **with-help-window** buffer-or-name body… [¶](#index-with_002dhelp_002dwindow)
:   This macro evaluates body like `with-output-to-temp-buffer`
    (see [Temporary Displays](#Temporary-Displays)), inserting any output produced by its
    forms into a buffer specified by buffer-or-name, which can be a
    buffer or the name of a buffer. (Frequently, buffer-or-name is
    the value returned by the function `help-buffer`.) This macro
    puts the specified buffer into Help mode and displays a message
    telling the user how to quit and scroll the help window. It selects
    the help window if the current value of the user option
    `help-window-select` has been set accordingly. It returns the
    last value in body.

Function: **help-setup-xref** item interactive-p [¶](#index-help_002dsetup_002dxref)
:   This function updates the cross reference data in the `*Help*`
    buffer, which is used to regenerate the help information when the user
    clicks on the ‘`Back`’ or ‘`Forward`’ buttons. Most commands
    that use the `*Help*` buffer should invoke this function before
    clearing the buffer. The item argument should have the form
    `(function . args)`, where function is a function
    to call, with argument list args, to regenerate the help buffer.
    The interactive-p argument is non-`nil` if the calling
    command was invoked interactively; in that case, the stack of items
    for the `*Help*` buffer’s ‘`Back`’ buttons is cleared.

See [describe-symbols example](#describe_002dsymbols-example), for an example of using
`help-buffer`, `with-help-window`, and
`help-setup-xref`.

Macro: **make-help-screen** fname help-line help-text help-map [¶](#index-make_002dhelp_002dscreen)
:   This macro defines a help command named fname that acts like a
    prefix key that shows a list of the subcommands it offers.

    When invoked, fname displays help-text in a window, then
    reads and executes a key sequence according to help-map. The
    string help-text should describe the bindings available in
    help-map.

    The command fname is defined to handle a few events itself, by
    scrolling the display of help-text. When fname reads one of
    those special events, it does the scrolling and then reads another
    event. When it reads an event that is not one of those few, and which
    has a binding in help-map, it executes that key’s binding and
    then returns.

    The argument help-line should be a single-line summary of the
    alternatives in help-map. In the current version of Emacs, this
    argument is used only if you set the option `three-step-help` to
    `t`.

    This macro is used in the command `help-for-help` which is the
    binding of `C-h C-h`.

User Option: **three-step-help** [¶](#index-three_002dstep_002dhelp)
:   If this variable is non-`nil`, commands defined with
    `make-help-screen` display their help-line strings in the
    echo area at first, and display the longer help-text strings only
    if the user types the help character again.

---

Previous: [Help Functions](#Help-Functions), Up: [Documentation](#Documentation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
