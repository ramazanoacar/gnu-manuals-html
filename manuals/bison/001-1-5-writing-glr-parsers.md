### 1.5 Writing GLR Parsers

In some grammars, Bison’s deterministic
LR(1) parsing algorithm cannot decide whether to apply a
certain grammar rule at a given point. That is, it may not be able to
decide (on the basis of the input read so far) which of two possible
reductions (applications of a grammar rule) applies, or whether to apply
a reduction or read more of the input and apply a reduction later in the
input. These are known respectively as *reduce/reduce* conflicts
(see [Reduce/Reduce Conflicts](#Reduce_002fReduce)), and *shift/reduce* conflicts
(see [Shift/Reduce Conflicts](#Shift_002fReduce)).

To use a grammar that is not easily modified to be LR(1), a more general
parsing algorithm is sometimes necessary. If you include `%glr-parser`
among the Bison declarations in your file (see [Outline of a Bison Grammar](#Grammar-Outline)), the
result is a Generalized LR (GLR) parser. These parsers handle Bison
grammars that contain no unresolved conflicts (i.e., after applying
precedence declarations) identically to deterministic parsers. However,
when faced with unresolved shift/reduce and reduce/reduce conflicts, GLR
parsers use the simple expedient of doing both, effectively cloning the
parser to follow both possibilities. Each of the resulting parsers can
again split, so that at any given time, there can be any number of possible
parses being explored. The parsers proceed in lockstep; that is, all of
them consume (shift) a given input symbol before any of them proceed to the
next. Each of the cloned parsers eventually meets one of two possible
fates: either it runs into a parsing error, in which case it simply
vanishes, or it merges with another parser, because the two of them have
reduced the input to an identical set of symbols.

During the time that there are multiple parsers, semantic actions are
recorded, but not performed. When a parser disappears, its recorded
semantic actions disappear as well, and are never performed. When a
reduction makes two parsers identical, causing them to merge, Bison records
both sets of semantic actions. Whenever the last two parsers merge,
reverting to the single-parser case, Bison resolves all the outstanding
actions either by precedences given to the grammar rules involved, or by
performing both actions, and then calling a designated user-defined function
on the resulting values to produce an arbitrary merged result.

* [Using GLR on Unambiguous Grammars](#Simple-GLR-Parsers)
* [Using GLR to Resolve Ambiguities](#Merging-GLR-Parses)
* [GLR Semantic Actions](#GLR-Semantic-Actions)
* [Controlling a Parse with Arbitrary Predicates](#Semantic-Predicates)

---

Next: [Using GLR to Resolve Ambiguities](#Merging-GLR-Parses), Up: [Writing GLR Parsers](#GLR-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 1.5.1 Using GLR on Unambiguous Grammars

In the simplest cases, you can use the GLR algorithm
to parse grammars that are unambiguous but fail to be LR(1).
Such grammars typically require more than one symbol of lookahead.

Consider a problem that
arises in the declaration of enumerated and subrange types in the
programming language Pascal. Here are some examples:

```
type subrange = lo .. hi;
type enum = (a, b, c);
```

The original language standard allows only numeric literals and constant
identifiers for the subrange bounds (‘`lo`’ and ‘`hi`’), but Extended
Pascal (ISO/IEC 10206) and many other Pascal implementations allow arbitrary
expressions there. This gives rise to the following situation, containing a
superfluous pair of parentheses:

```
type subrange = (a) .. b;
```

Compare this to the following declaration of an enumerated
type with only one value:

```
type enum = (a);
```

(These declarations are contrived, but they are syntactically valid, and
more-complicated cases can come up in practical programs.)

These two declarations look identical until the ‘`..`’ token. With
normal LR(1) one-token lookahead it is not possible to decide between the
two forms when the identifier ‘`a`’ is parsed. It is, however, desirable
for a parser to decide this, since in the latter case ‘`a`’ must become a
new identifier to represent the enumeration value, while in the former case
‘`a`’ must be evaluated with its current meaning, which may be a constant
or even a function call.

You could parse ‘`(a)`’ as an “unspecified identifier in parentheses”,
to be resolved later, but this typically requires substantial contortions in
both semantic actions and large parts of the grammar, where the parentheses
are nested in the recursive rules for expressions.

You might think of using the lexer to distinguish between the two forms by
returning different tokens for currently defined and undefined identifiers.
But if these declarations occur in a local scope, and ‘`a`’ is defined in
an outer scope, then both forms are possible—either locally redefining
‘`a`’, or using the value of ‘`a`’ from the outer scope. So this
approach cannot work.

A simple solution to this problem is to declare the parser to use the GLR
algorithm. When the GLR parser reaches the critical state, it merely splits
into two branches and pursues both syntax rules simultaneously. Sooner or
later, one of them runs into a parsing error. If there is a ‘`..`’ token
before the next ‘`;`’, the rule for enumerated types fails since it
cannot accept ‘`..`’ anywhere; otherwise, the subrange type rule fails
since it requires a ‘`..`’ token. So one of the branches fails silently,
and the other one continues normally, performing all the intermediate
actions that were postponed during the split.

If the input is syntactically incorrect, both branches fail and the parser
reports a syntax error as usual.

The effect of all this is that the parser seems to “guess” the correct
branch to take, or in other words, it seems to use more lookahead than the
underlying LR(1) algorithm actually allows for. In this example, LR(2)
would suffice, but also some cases that are not LR(*k*) for any
*k* can be handled this way.

In general, a GLR parser can take quadratic or cubic worst-case time, and
the current Bison parser even takes exponential time and space for some
grammars. In practice, this rarely happens, and for many grammars it is
possible to prove that it cannot happen. The present example contains only
one conflict between two rules, and the type-declaration context containing
the conflict cannot be nested. So the number of branches that can exist at
any time is limited by the constant 2, and the parsing time is still linear.

Here is a Bison grammar corresponding to the example above. It
parses a vastly simplified form of Pascal type declarations.

```
%token TYPE DOTDOT ID
```

```
%left '+' '-'
%left '*' '/'
```

```
%%
type_decl: TYPE ID '=' type ';' ;
```

```
type:
  '(' id_list ')'
| expr DOTDOT expr
;
```

```

```

```
id_list:
  ID
| id_list ',' ID
;
```

```

```

```
expr:
  '(' expr ')'
| expr '+' expr
| expr '-' expr
| expr '*' expr
| expr '/' expr
| ID
;
```

When used as a normal LR(1) grammar, Bison correctly complains
about one reduce/reduce conflict. In the conflicting situation the
parser chooses one of the alternatives, arbitrarily the one
declared first. Therefore the following correct input is not
recognized:

```
type t = (a) .. b;
```

The parser can be turned into a GLR parser, while also telling Bison
to be silent about the one known reduce/reduce conflict, by adding
these two declarations to the Bison grammar file (before the first
‘`%%`’):

```
%glr-parser
%expect-rr 1
```

No change in the grammar itself is required. Now the parser recognizes all
valid declarations, according to the limited syntax above, transparently.
In fact, the user does not even notice when the parser splits.

So here we have a case where we can use the benefits of GLR, almost without
disadvantages. Even in simple cases like this, however, there are at least
two potential problems to beware. First, always analyze the conflicts
reported by Bison to make sure that GLR splitting is only done where it is
intended. A GLR parser splitting inadvertently may cause problems less
obvious than an LR parser statically choosing the wrong alternative in a
conflict. Second, consider interactions with the lexer (see [Semantic Info in Token Kinds](#Semantic-Tokens)) with great care. Since a split parser consumes tokens without
performing any actions during the split, the lexer cannot obtain information
via parser actions. Some cases of lexer interactions can be eliminated by
using GLR to shift the complications from the lexer to the parser. You must
check the remaining cases for correctness.

In our example, it would be safe for the lexer to return tokens based on
their current meanings in some symbol table, because no new symbols are
defined in the middle of a type declaration. Though it is possible for a
parser to define the enumeration constants as they are parsed, before the
type declaration is completed, it actually makes no difference since they
cannot be used within the same enumerated type declaration.

---

Next: [GLR Semantic Actions](#GLR-Semantic-Actions), Previous: [Using GLR on Unambiguous Grammars](#Simple-GLR-Parsers), Up: [Writing GLR Parsers](#GLR-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 1.5.2 Using GLR to Resolve Ambiguities

Let’s consider an example, vastly simplified from a C++
grammar.[1](#FOOT1)

```
%{
  #include <stdio.h>
  int yylex (void);
  void yyerror (char const *);
%}

%define api.value.type {char const *}

%token TYPENAME ID

%right '='
%left '+'

%glr-parser

%%

prog:
  %empty
| prog stmt   { printf ("\n"); }
;

stmt:
  expr ';'  %dprec 1
| decl      %dprec 2
;

expr:
  ID               { printf ("%s ", $$); }
| TYPENAME '(' expr ')'
                   { printf ("%s <cast> ", $1); }
| expr '+' expr    { printf ("+ "); }
| expr '=' expr    { printf ("= "); }
;

decl:
  TYPENAME declarator ';'
                   { printf ("%s <declare> ", $1); }
| TYPENAME declarator '=' expr ';'
                   { printf ("%s <init-declare> ", $1); }
;

declarator:
  ID               { printf ("\"%s\" ", $1); }
| '(' declarator ')'
;
```

This models a problematic part of the C++ grammar—the ambiguity between
certain declarations and statements. For example,

```
T (x) = y+z;
```

parses as either an `expr` or a `stmt`
(assuming that ‘`T`’ is recognized as a `TYPENAME` and
‘`x`’ as an `ID`).
Bison detects this as a reduce/reduce conflict between the rules
`expr : ID` and `declarator : ID`, which it cannot resolve at the
time it encounters `x` in the example above. Since this is a
GLR parser, it therefore splits the problem into two parses, one for
each choice of resolving the reduce/reduce conflict.
Unlike the example from the previous section (see [Using GLR on Unambiguous Grammars](#Simple-GLR-Parsers)),
however, neither of these parses “dies,” because the grammar as it stands is
ambiguous. One of the parsers eventually reduces `stmt : expr ';'` and
the other reduces `stmt : decl`, after which both parsers are in an
identical state: they’ve seen ‘`prog stmt`’ and have the same unprocessed
input remaining. We say that these parses have *merged.*

At this point, the GLR parser requires a specification in the
grammar of how to choose between the competing parses.
In the example above, the two `%dprec`
declarations specify that Bison is to give precedence
to the parse that interprets the example as a
`decl`, which implies that `x` is a declarator.
The parser therefore prints

```
"x" y z + T <init-declare>
```

The `%dprec` declarations only come into play when more than one
parse survives. Consider a different input string for this parser:

```
T (x) + y;
```

This is another example of using GLR to parse an unambiguous
construct, as shown in the previous section (see [Using GLR on Unambiguous Grammars](#Simple-GLR-Parsers)).
Here, there is no ambiguity (this cannot be parsed as a declaration).
However, at the time the Bison parser encounters `x`, it does not
have enough information to resolve the reduce/reduce conflict (again,
between `x` as an `expr` or a `declarator`). In this
case, no precedence declaration is used. Again, the parser splits
into two, one assuming that `x` is an `expr`, and the other
assuming `x` is a `declarator`. The second of these parsers
then vanishes when it sees `+`, and the parser prints

```
x T <cast> y +
```

Suppose that instead of resolving the ambiguity, you wanted to see all
the possibilities. For this purpose, you must merge the semantic
actions of the two possible parsers, rather than choosing one over the
other. To do so, you could change the declaration of `stmt` as
follows:

```
stmt:
  expr ';'  %merge <stmt_merge>
| decl      %merge <stmt_merge>
;
```

and define the `stmt_merge` function as:

```
static YYSTYPE
stmt_merge (YYSTYPE x0, YYSTYPE x1)
{
  printf ("<OR> ");
  return "";
}
```

with an accompanying forward declaration
in the C declarations at the beginning of the file:

```
%{
  static YYSTYPE stmt_merge (YYSTYPE x0, YYSTYPE x1);
%}
```

With these declarations, the resulting parser parses the first example
as both an `expr` and a `decl`, and prints

```
"x" y z + T <init-declare> x T <cast> y z + = <OR>
```

Bison requires that all of the
productions that participate in any particular merge have identical
‘`%merge`’ clauses. Otherwise, the ambiguity would be unresolvable,
and the parser will report an error during any parse that results in
the offending merge.

  

The signature of the merger depends on the type of the symbol. In the
previous example, the merged-to symbol (`stmt`) does not have a
specific type, and the merger is

```
YYSTYPE stmt_merge (YYSTYPE x0, YYSTYPE x1);
```

However, if `stmt` had a declared type, e.g.,

```
%type <Node *> stmt;
```

or

```
%union {
  Node *node;
  ...
};
```

```
%type <node> stmt;
```

then the prototype of the merger must be:

```
Node *stmt_merge (YYSTYPE x0, YYSTYPE x1);
```

(This signature might be a mistake originally, and maybe it should have been
‘`Node *stmt_merge (Node *x0, Node *x1)`’. If you have an opinion about
it, please let us know.)

---

Next: [Controlling a Parse with Arbitrary Predicates](#Semantic-Predicates), Previous: [Using GLR to Resolve Ambiguities](#Merging-GLR-Parses), Up: [Writing GLR Parsers](#GLR-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 1.5.3 GLR Semantic Actions

The nature of GLR parsing and the structure of the generated
parsers give rise to certain restrictions on semantic values and actions.

* [Deferred semantic actions](#Deferred-semantic-actions)
* [YYERROR](#YYERROR)
* [Restrictions on semantic values and locations](#Restrictions-on-semantic-values-and-locations)

#### 1.5.3.1 Deferred semantic actions

By definition, a deferred semantic action is not performed at the same time as
the associated reduction.
This raises caveats for several Bison features you might use in a semantic
action in a GLR parser.

In any semantic action, you can examine `yychar` to determine the kind
of the lookahead token present at the time of the associated reduction.
After checking that `yychar` is not set to `YYEMPTY` or
`YYEOF`, you can then examine `yylval` and `yylloc` to
determine the lookahead token’s semantic value and location, if any. In a
nondeferred semantic action, you can also modify any of these variables to
influence syntax analysis. See [Lookahead Tokens](#Lookahead).

In a deferred semantic action, it’s too late to influence syntax analysis.
In this case, `yychar`, `yylval`, and `yylloc` are set to
shallow copies of the values they had at the time of the associated reduction.
For this reason alone, modifying them is dangerous.
Moreover, the result of modifying them is undefined and subject to change with
future versions of Bison.
For example, if a semantic action might be deferred, you should never write it
to invoke `yyclearin` (see [Special Features for Use in Actions](#Action-Features)) or to attempt to free
memory referenced by `yylval`.

#### 1.5.3.2 YYERROR

Another Bison feature requiring special consideration is `YYERROR`
(see [Special Features for Use in Actions](#Action-Features)), which you can invoke in a semantic action to
initiate error recovery.
During deterministic GLR operation, the effect of `YYERROR` is
the same as its effect in a deterministic parser.
The effect in a deferred action is similar, but the precise point of the
error is undefined; instead, the parser reverts to deterministic operation,
selecting an unspecified stack on which to continue with a syntax error.
In a semantic predicate (see [Controlling a Parse with Arbitrary Predicates](#Semantic-Predicates)) during nondeterministic
parsing, `YYERROR` silently prunes
the parse that invoked the test.

#### 1.5.3.3 Restrictions on semantic values and locations

GLR parsers require that you use POD (Plain Old Data) types for
semantic values and location types when using the generated parsers as
C++ code.

---

Previous: [GLR Semantic Actions](#GLR-Semantic-Actions), Up: [Writing GLR Parsers](#GLR-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 1.5.4 Controlling a Parse with Arbitrary Predicates

In addition to the `%dprec` and `%merge` directives,
GLR parsers
allow you to reject parses on the basis of arbitrary computations executed
in user code, without having Bison treat this rejection as an error
if there are alternative parses. For example,

```
widget:
  %?{  new_syntax } "widget" id new_args  { $$ = f($3, $4); }
| %?{ !new_syntax } "widget" id old_args  { $$ = f($3, $4); }
;
```

is one way to allow the same parser to handle two different syntaxes for
widgets. The clause preceded by `%?` is treated like an ordinary
midrule action, except that its text is handled as an expression and is always
evaluated immediately (even when in nondeterministic mode). If the
expression yields 0 (false), the clause is treated as a syntax error,
which, in a nondeterministic parser, causes the stack in which it is reduced
to die. In a deterministic parser, it acts like `YYERROR`.

As the example shows, predicates otherwise look like semantic actions, and
therefore you must take them into account when determining the numbers
to use for denoting the semantic values of right-hand side symbols.
Predicate actions, however, have no defined value, and may not be given
labels.

There is a subtle difference between semantic predicates and ordinary
actions in nondeterministic mode, since the latter are deferred.
For example, we could try to rewrite the previous example as

```
widget:
  { if (!new_syntax) YYERROR; }
    "widget" id new_args  { $$ = f($3, $4); }
|  { if (new_syntax) YYERROR; }
    "widget" id old_args  { $$ = f($3, $4); }
;
```

(reversing the sense of the predicate tests to cause an error when they are
false). However, this
does *not* have the same effect if `new_args` and `old_args`
have overlapping syntax.
Since the midrule actions testing `new_syntax` are deferred,
a GLR parser first encounters the unresolved ambiguous reduction
for cases where `new_args` and `old_args` recognize the same string
*before* performing the tests of `new_syntax`. It therefore
reports an error.

Finally, be careful in writing predicates: deferred actions have not been
evaluated, so that using them in a predicate will have undefined effects.

---

Next: [Bison Output: the Parser Implementation File](#Bison-Parser), Previous: [Writing GLR Parsers](#GLR-Parsers), Up: [The Concepts of Bison](#Concepts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 1.6 Locations

Many applications, like interpreters or compilers, have to produce verbose
and useful error messages. To achieve this, one must be able to keep track of
the *textual location*, or *location*, of each syntactic construct.
Bison provides a mechanism for handling these locations.

Each token has a semantic value. In a similar fashion, each token has an
associated location, but the type of locations is the same for all tokens
and groupings. Moreover, the output parser is equipped with a default data
structure for storing locations (see [Tracking Locations](#Tracking-Locations), for more
details).

Like semantic values, locations can be reached in actions using a dedicated
set of constructs. In the example above, the location of the whole grouping
is `@$`, while the locations of the subexpressions are `@1` and
`@3`.

When a rule is matched, a default action is used to compute the semantic value
of its left hand side (see [Actions](#Actions)). In the same way, another default
action is used for locations. However, the action for locations is general
enough for most cases, meaning there is usually no need to describe for each
rule how `@$` should be formed. When building a new location for a given
grouping, the default behavior of the output parser is to take the beginning
of the first symbol, and the end of the last symbol.

---

Next: [Stages in Using Bison](#Stages), Previous: [Locations](#Locations), Up: [The Concepts of Bison](#Concepts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 1.7 Bison Output: the Parser Implementation File

When you run Bison, you give it a Bison grammar file as input. The
most important output is a C source file that implements a parser for
the language described by the grammar. This parser is called a
*Bison parser*, and this file is called a *Bison parser
implementation file*. Keep in mind that the Bison utility and the
Bison parser are two distinct programs: the Bison utility is a program
whose output is the Bison parser implementation file that becomes part
of your program.

The job of the Bison parser is to group tokens into groupings according to
the grammar rules—for example, to build identifiers and operators into
expressions. As it does this, it runs the actions for the grammar rules it
uses.

The tokens come from a function called the *lexical analyzer* that
you must supply in some fashion (such as by writing it in C). The Bison
parser calls the lexical analyzer each time it wants a new token. It
doesn’t know what is “inside” the tokens (though their semantic values
may reflect this). Typically the lexical analyzer makes the tokens by
parsing characters of text, but Bison does not depend on this.
See [The Lexical Analyzer Function `yylex`](#Lexical).

The Bison parser implementation file is C code which defines a
function named `yyparse` which implements that grammar. This
function does not make a complete C program: you must supply some
additional functions. One is the lexical analyzer. Another is an
error-reporting function which the parser calls to report an error.
In addition, a complete C program must start with a function called
`main`; you have to provide this, and arrange for it to call
`yyparse` or the parser will never run. See [Parser C-Language Interface](#Interface).

Aside from the token kind names and the symbols in the actions you
write, all symbols defined in the Bison parser implementation file
itself begin with ‘`yy`’ or ‘`YY`’. This includes interface
functions such as the lexical analyzer function `yylex`, the
error reporting function `yyerror` and the parser function
`yyparse` itself. This also includes numerous identifiers used
for internal purposes. Therefore, you should avoid using C
identifiers starting with ‘`yy`’ or ‘`YY`’ in the Bison grammar
file except for the ones defined in this manual. Also, you should
avoid using the C identifiers ‘`malloc`’ and ‘`free`’ for
anything other than their usual meanings.

In some cases the Bison parser implementation file includes system
headers, and in those cases your code should respect the identifiers
reserved by those headers. On some non-GNU hosts, `<limits.h>`,
`<stddef.h>`, `<stdint.h>` (if available), and `<stdlib.h>`
are included to declare memory allocators and integer types and constants.
`<libintl.h>` is included if message translation is in use
(see [Parser Internationalization](#Internationalization)). Other system headers may be included
if you define `YYDEBUG` (see [Tracing Your Parser](#Tracing)) or
`YYSTACK_USE_ALLOCA` (see [Bison Symbols](#Table-of-Symbols)) to a nonzero value.

---

Next: [The Overall Layout of a Bison Grammar](#Grammar-Layout), Previous: [Bison Output: the Parser Implementation File](#Bison-Parser), Up: [The Concepts of Bison](#Concepts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 1.8 Stages in Using Bison

The actual language-design process using Bison, from grammar specification
to a working compiler or interpreter, has these parts:

1. Formally specify the grammar in a form recognized by Bison
   (see [Bison Grammar Files](#Grammar-File)). For each grammatical rule
   in the language, describe the action that is to be taken when an
   instance of that rule is recognized. The action is described by a
   sequence of C statements.
2. Write a lexical analyzer to process input and pass tokens to the parser.
   The lexical analyzer may be written by hand in C (see [The Lexical Analyzer Function `yylex`](#Lexical)). It
   could also be produced using Lex, but the use of Lex is not discussed in
   this manual.
3. Write a controlling function that calls the Bison-produced parser.
4. Write error-reporting routines.

To turn this source code as written into a runnable program, you
must follow these steps:

1. Run Bison on the grammar to produce the parser.
2. Compile the code output by Bison, as well as any other source files.
3. Link the object files to produce the finished product.

---

Previous: [Stages in Using Bison](#Stages), Up: [The Concepts of Bison](#Concepts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 1.9 The Overall Layout of a Bison Grammar

The input file for the Bison utility is a *Bison grammar file*. The
general form of a Bison grammar file is as follows:

```
%{
Prologue
%}

Bison declarations

%%
Grammar rules
%%
Epilogue
```

The ‘`%%`’, ‘`%{`’ and ‘`%}`’ are punctuation that appears
in every Bison grammar file to separate the sections.

The prologue may define types and variables used in the actions. You can
also use preprocessor commands to define macros used there, and use
`#include` to include header files that do any of these things.
You need to declare the lexical analyzer `yylex` and the error
printer `yyerror` here, along with any other global identifiers
used by the actions in the grammar rules.

The Bison declarations declare the names of the terminal and nonterminal
symbols, and may also describe operator precedence and the data types of
semantic values of various symbols.

The grammar rules define how to construct each nonterminal symbol from its
parts.

The epilogue can contain any code you want to use. Often the
definitions of functions declared in the prologue go here. In a
simple program, all the rest of the program can go here.

---

Next: [Bison Grammar Files](#Grammar-File), Previous: [The Concepts of Bison](#Concepts), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 2 Examples

Now we show and explain several sample programs written using Bison: a
Reverse Polish Notation calculator, an algebraic (infix) notation
calculator — later extended to track “locations” —
and a multi-function calculator. All
produce usable, though limited, interactive desk-top calculators.

These examples are simple, but Bison grammars for real programming
languages are written the same way. You can copy these examples into a
source file to try them.

  

Bison comes with several examples (including for the different target
languages). If this package is properly installed, you shall find them in
`prefix/share/doc/bison/examples`, where prefix is the root
of the installation, probably something like `/usr/local` or
`/usr`.

* [Reverse Polish Notation Calculator](#RPN-Calc)
* [Infix Notation Calculator: `calc`](#Infix-Calc)
* [Simple Error Recovery](#Simple-Error-Recovery)
* [Location Tracking Calculator: `ltcalc`](#Location-Tracking-Calc)
* [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc)
* [Exercises](#Exercises)

---

Next: [Infix Notation Calculator: `calc`](#Infix-Calc), Up: [Examples](#Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 2.1 Reverse Polish Notation Calculator

The first example[2](#FOOT2) is that of a simple double-precision *Reverse
Polish
Notation* calculator (a calculator using postfix operators). This example
provides a good starting point, since operator precedence is not an issue.
The second example will illustrate how operator precedence is handled.

The source code for this calculator is named `rpcalc.y`. The
‘`.y`’ extension is a convention used for Bison grammar files.

* [Declarations for `rpcalc`](#Rpcalc-Declarations)
* [Grammar Rules for `rpcalc`](#Rpcalc-Rules)
* [The `rpcalc` Lexical Analyzer](#Rpcalc-Lexer)
* [The Controlling Function](#Rpcalc-Main)
* [The Error Reporting Routine](#Rpcalc-Error)
* [Running Bison to Make the Parser](#Rpcalc-Generate)
* [Compiling the Parser Implementation File](#Rpcalc-Compile)

---

Next: [Grammar Rules for `rpcalc`](#Rpcalc-Rules), Up: [Reverse Polish Notation Calculator](#RPN-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.1 Declarations for `rpcalc`

Here are the C and Bison declarations for the Reverse Polish Notation
calculator. As in C, comments are placed between ‘`/*…*/`’ or
after ‘`//`’.

```
/* Reverse Polish Notation calculator. */
```

```
%{
  #include <stdio.h>
  #include <math.h>
  int yylex (void);
  void yyerror (char const *);
%}
```

```
%define api.value.type {double}
%token NUM

%% /* Grammar rules and actions follow. */
```

The declarations section (see [The prologue](#Prologue)) contains two
preprocessor directives and two forward declarations.

The `#include` directive is used to declare the exponentiation
function `pow`.

The forward declarations for `yylex` and `yyerror` are
needed because the C language requires that functions be declared
before they are used. These functions will be defined in the
epilogue, but the parser calls them so they must be declared in the
prologue.

The second section, Bison declarations, provides information to Bison about
the tokens and their types (see [The Bison Declarations Section](#Bison-Declarations)).

The `%define` directive defines the variable `api.value.type`,
thus specifying the C data type for semantic values of both tokens and
groupings (see [Data Types of Semantic Values](#Value-Type)). The Bison
parser will use whatever type `api.value.type` is defined as; if you
don’t define it, `int` is the default. Because we specify
‘`{double}`’, each token and each expression has an associated value,
which is a floating point number. C code can use `YYSTYPE` to refer to
the value `api.value.type`.

Each terminal symbol that is not a single-character literal must be
declared. (Single-character literals normally don’t need to be declared.)
In this example, all the arithmetic operators are designated by
single-character literals, so the only terminal symbol that needs to be
declared is `NUM`, the token kind for numeric constants.

---

Next: [The `rpcalc` Lexical Analyzer](#Rpcalc-Lexer), Previous: [Declarations for `rpcalc`](#Rpcalc-Declarations), Up: [Reverse Polish Notation Calculator](#RPN-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.2 Grammar Rules for `rpcalc`

Here are the grammar rules for the Reverse Polish Notation calculator.

```
input:
  %empty
| input line
;
```

```

```

```
line:
  '\n'
| exp '\n'      { printf ("%.10g\n", $1); }
;
```

```

```

```
exp:
  NUM
| exp exp '+'   { $$ = $1 + $2;      }
| exp exp '-'   { $$ = $1 - $2;      }
| exp exp '*'   { $$ = $1 * $2;      }
| exp exp '/'   { $$ = $1 / $2;      }
| exp exp '^'   { $$ = pow ($1, $2); }  /* Exponentiation */
| exp 'n'       { $$ = -$1;          }  /* Unary minus   */
;
```

```
%%
```

The groupings of the rpcalc “language” defined here are the expression
(given the name `exp`), the line of input (`line`), and the
complete input transcript (`input`). Each of these nonterminal
symbols has several alternate rules, joined by the vertical bar ‘`|`’
which is read as “or”. The following sections explain what these rules
mean.

The semantics of the language is determined by the actions taken when a
grouping is recognized. The actions are the C code that appears inside
braces. See [Actions](#Actions).

You must specify these actions in C, but Bison provides the means for
passing semantic values between the rules. In each action, the
pseudo-variable `$$` stands for the semantic value for the grouping
that the rule is going to construct. Assigning a value to `$$` is the
main job of most actions. The semantic values of the components of the
rule are referred to as `$1`, `$2`, and so on.

* [Explanation of `input`](#Rpcalc-Input)
* [Explanation of `line`](#Rpcalc-Line)
* [Explanation of `exp`](#Rpcalc-Exp)

---

Next: [Explanation of `line`](#Rpcalc-Line), Up: [Grammar Rules for `rpcalc`](#Rpcalc-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.2.1 Explanation of `input`

Consider the definition of `input`:

```
input:
  %empty
| input line
;
```

This definition reads as follows: “A complete input is either an empty
string, or a complete input followed by an input line”. Notice that
“complete input” is defined in terms of itself. This definition is said
to be *left recursive* since `input` appears always as the
leftmost symbol in the sequence. See [Recursive Rules](#Recursion).

The first alternative is empty because there are no symbols between the
colon and the first ‘`|`’; this means that `input` can match an
empty string of input (no tokens). We write the rules this way because it
is legitimate to type `Ctrl-d` right after you start the calculator.
It’s conventional to put an empty alternative first and to use the
(optional) `%empty` directive, or to write the comment ‘`/* empty
*/`’ in it (see [Empty Rules](#Empty-Rules)).

The second alternate rule (`input line`) handles all nontrivial input.
It means, “After reading any number of lines, read one more line if
possible.” The left recursion makes this rule into a loop. Since the
first alternative matches empty input, the loop can be executed zero or
more times.

The parser function `yyparse` continues to process input until a
grammatical error is seen or the lexical analyzer says there are no more
input tokens; we will arrange for the latter to happen at end-of-input.

---

Next: [Explanation of `exp`](#Rpcalc-Exp), Previous: [Explanation of `input`](#Rpcalc-Input), Up: [Grammar Rules for `rpcalc`](#Rpcalc-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.2.2 Explanation of `line`

Now consider the definition of `line`:

```
line:
  '\n'
| exp '\n'  { printf ("%.10g\n", $1); }
;
```

The first alternative is a token which is a newline character; this means
that rpcalc accepts a blank line (and ignores it, since there is no
action). The second alternative is an expression followed by a newline.
This is the alternative that makes rpcalc useful. The semantic value of
the `exp` grouping is the value of `$1` because the `exp` in
question is the first symbol in the alternative. The action prints this
value, which is the result of the computation the user asked for.

This action is unusual because it does not assign a value to `$$`. As
a consequence, the semantic value associated with the `line` is
uninitialized (its value will be unpredictable). This would be a bug if
that value were ever used, but we don’t use it: once rpcalc has printed the
value of the user’s input line, that value is no longer needed.

---

Previous: [Explanation of `line`](#Rpcalc-Line), Up: [Grammar Rules for `rpcalc`](#Rpcalc-Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.2.3 Explanation of `exp`

The `exp` grouping has several rules, one for each kind of expression.
The first rule handles the simplest expressions: those that are just
numbers. The second handles an addition-expression, which looks like two
expressions followed by a plus-sign. The third handles subtraction, and so
on.

```
exp:
  NUM
| exp exp '+'     { $$ = $1 + $2;    }
| exp exp '-'     { $$ = $1 - $2;    }
…
;
```

We have used ‘`|`’ to join all the rules for `exp`, but we could
equally well have written them separately:

```
exp: NUM;
exp: exp exp '+'  { $$ = $1 + $2; };
exp: exp exp '-'  { $$ = $1 - $2; };
…
```

Most of the rules have actions that compute the value of the expression in
terms of the value of its parts. For example, in the rule for addition,
`$1` refers to the first component `exp` and `$2` refers to
the second one. The third component, `'+'`, has no meaningful
associated semantic value, but if it had one you could refer to it as
`$3`. The first rule relies on the implicit default action: ‘`{
$$ = $1; }`’.

When `yyparse` recognizes a sum expression using this rule, the sum of
the two subexpressions’ values is produced as the value of the entire
expression. See [Actions](#Actions).

You don’t have to give an action for every rule. When a rule has no action,
Bison by default copies the value of `$1` into `$$`. This is what
happens in the first rule (the one that uses `NUM`).

The formatting shown here is the recommended convention, but Bison does not
require it. You can add or change white space as much as you wish. For
example, this:

```
exp: NUM | exp exp '+' {$$ = $1 + $2; } | … ;
```

means the same thing as this:

```
exp:
  NUM
| exp exp '+'    { $$ = $1 + $2; }
| …
;
```

The latter, however, is much more readable.

---

Next: [The Controlling Function](#Rpcalc-Main), Previous: [Grammar Rules for `rpcalc`](#Rpcalc-Rules), Up: [Reverse Polish Notation Calculator](#RPN-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.3 The `rpcalc` Lexical Analyzer

The lexical analyzer’s job is low-level parsing: converting characters
or sequences of characters into tokens. The Bison parser gets its
tokens by calling the lexical analyzer. See [The Lexical Analyzer Function `yylex`](#Lexical).

Only a simple lexical analyzer is needed for the RPN
calculator. This
lexical analyzer skips blanks and tabs, then reads in numbers as
`double` and returns them as `NUM` tokens. Any other character
that isn’t part of a number is a separate token. Note that the token-code
for such a single-character token is the character itself.

The return value of the lexical analyzer function is a numeric code which
represents a token kind. The same text used in Bison rules to stand for
this token kind is also a C expression for the numeric code of the kind.
This works in two ways. If the token kind is a character literal, then its
numeric code is that of the character; you can use the same character
literal in the lexical analyzer to express the number. If the token kind is
an identifier, that identifier is defined by Bison as a C enum whose
definition is the appropriate code. In this example, therefore, `NUM`
becomes an enum for `yylex` to use.

The semantic value of the token (if it has one) is stored into the global
variable `yylval`, which is where the Bison parser will look for it.
(The C data type of `yylval` is `YYSTYPE`, whose value was defined
at the beginning of the grammar via ‘`%define api.value.type
{double}`’; see [Declarations for `rpcalc`](#Rpcalc-Declarations).)

A token kind code of zero is returned if the end-of-input is encountered.
(Bison recognizes any nonpositive value as indicating end-of-input.)

Here is the code for the lexical analyzer:

```
/* The lexical analyzer returns a double floating point
   number on the stack and the token NUM, or the numeric code
   of the character read if not a number.  It skips all blanks
   and tabs, and returns 0 for end-of-input. */

#include <ctype.h>
#include <stdlib.h>
```

```

```

```
int
yylex (void)
{
  int c = getchar ();
  /* Skip white space. */
  while (c == ' ' || c == '\t')
    c = getchar ();
```

```
  /* Process numbers. */
  if (c == '.' || isdigit (c))
    {
      ungetc (c, stdin);
      if (scanf ("%lf", &yylval) != 1)
        abort ();
      return NUM;
    }
```

```
  /* Return end-of-input. */
  else if (c == EOF)
    return YYEOF;
  /* Return a single char. */
  else
    return c;
}
```

---

Next: [The Error Reporting Routine](#Rpcalc-Error), Previous: [The `rpcalc` Lexical Analyzer](#Rpcalc-Lexer), Up: [Reverse Polish Notation Calculator](#RPN-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.4 The Controlling Function

In keeping with the spirit of this example, the controlling function is
kept to the bare minimum. The only requirement is that it call
`yyparse` to start the process of parsing.

```
int
main (void)
{
  return yyparse ();
}
```

---

Next: [Running Bison to Make the Parser](#Rpcalc-Generate), Previous: [The Controlling Function](#Rpcalc-Main), Up: [Reverse Polish Notation Calculator](#RPN-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.5 The Error Reporting Routine

When `yyparse` detects a syntax error, it calls the error reporting
function `yyerror` to print an error message (usually but not
always `"syntax error"`). It is up to the programmer to supply
`yyerror` (see [Parser C-Language Interface](#Interface)), so
here is the definition we will use:

```
#include <stdio.h>
```

```
/* Called by yyparse on error. */
void
yyerror (char const *s)
{
  fprintf (stderr, "%s\n", s);
}
```

After `yyerror` returns, the Bison parser may recover from the error
and continue parsing if the grammar contains a suitable error rule
(see [Error Recovery](#Error-Recovery)). Otherwise, `yyparse` returns nonzero. We
have not written any error rules in this example, so any invalid input will
cause the calculator program to exit. This is not clean behavior for a
real calculator, but it is adequate for the first example.

---

Next: [Compiling the Parser Implementation File](#Rpcalc-Compile), Previous: [The Error Reporting Routine](#Rpcalc-Error), Up: [Reverse Polish Notation Calculator](#RPN-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.6 Running Bison to Make the Parser

Before running Bison to produce a parser, we need to decide how to
arrange all the source code in one or more source files. For such a
simple example, the easiest thing is to put everything in one file,
the grammar file. The definitions of `yylex`, `yyerror` and
`main` go at the end, in the epilogue of the grammar file
(see [The Overall Layout of a Bison Grammar](#Grammar-Layout)).

For a large project, you would probably have several source files, and use
`make` to arrange to recompile them.

With all the source in the grammar file, you use the following command
to convert it into a parser implementation file:

```
$ bison file.y
```

In this example, the grammar file is called `rpcalc.y` (for
“Reverse Polish CALCulator”). Bison produces a parser
implementation file named `file.tab.c`, removing the
‘`.y`’ from the grammar file name. The parser implementation file
contains the source code for `yyparse`. The additional functions
in the grammar file (`yylex`, `yyerror` and `main`) are
copied verbatim to the parser implementation file.

---

Previous: [Running Bison to Make the Parser](#Rpcalc-Generate), Up: [Reverse Polish Notation Calculator](#RPN-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.1.7 Compiling the Parser Implementation File

Here is how to compile and run the parser implementation file:

```
# List files in current directory.
$ ls
rpcalc.tab.c  rpcalc.y
```

```

```

```
# Compile the Bison parser.
# -lm tells compiler to search math library for pow.
$ cc -lm -o rpcalc rpcalc.tab.c
```

```

```

```
# List files again.
$ ls
rpcalc  rpcalc.tab.c  rpcalc.y
```

The file `rpcalc` now contains the executable code. Here is an
example session using `rpcalc`.

```
$ rpcalc
4 9 +
⇒ 13
3 7 + 3 4 5 *+-
⇒ -13
3 7 + 3 4 5 * + - n              Note the unary minus, ‘n’
⇒ 13
5 6 / 4 n +
⇒ -3.166666667
3 4 ^                            Exponentiation
⇒ 81
^D                               End-of-file indicator
$
```

---

Next: [Simple Error Recovery](#Simple-Error-Recovery), Previous: [Reverse Polish Notation Calculator](#RPN-Calc), Up: [Examples](#Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 2.2 Infix Notation Calculator: `calc`

We now modify rpcalc to handle infix operators instead of
postfix.[3](#FOOT3) Infix
notation involves the concept of operator precedence and the need for
parentheses nested to arbitrary depth. Here is the Bison code for
`calc.y`, an infix desk-top calculator.

```
/* Infix notation calculator. */
```

```
%{
  #include <math.h>
  #include <stdio.h>
  int yylex (void);
  void yyerror (char const *);
%}
```

```

```

```
/* Bison declarations. */
%define api.value.type {double}
%token NUM
%left '-' '+'
%left '*' '/'
%precedence NEG   /* negation--unary minus */
%right '^'        /* exponentiation */
```

```
%% /* The grammar follows. */
```

```
input:
  %empty
| input line
;
```

```

```

```
line:
  '\n'
| exp '\n'  { printf ("\t%.10g\n", $1); }
;
```

```

```

```
exp:
  NUM
| exp '+' exp        { $$ = $1 + $3;      }
| exp '-' exp        { $$ = $1 - $3;      }
| exp '*' exp        { $$ = $1 * $3;      }
| exp '/' exp        { $$ = $1 / $3;      }
| '-' exp  %prec NEG { $$ = -$2;          }
| exp '^' exp        { $$ = pow ($1, $3); }
| '(' exp ')'        { $$ = $2;           }
;
```

```
%%
```

The functions `yylex`, `yyerror` and `main` can be the
same as before.

There are two important new features shown in this code.

In the second section (Bison declarations), `%left` declares token
kinds and says they are left-associative operators. The declarations
`%left` and `%right` (right associativity) take the place of
`%token` which is used to declare a token kind name without
associativity/precedence. (These tokens are single-character literals,
which ordinarily don’t need to be declared. We declare them here to specify
the associativity/precedence.)

Operator precedence is determined by the line ordering of the
declarations; the higher the line number of the declaration (lower on
the page or screen), the higher the precedence. Hence, exponentiation
has the highest precedence, unary minus (`NEG`) is next, followed
by ‘`*`’ and ‘`/`’, and so on. Unary minus is not associative,
only precedence matters (`%precedence`. See [Operator Precedence](#Precedence).

The other important new feature is the `%prec` in the grammar
section for the unary minus operator. The `%prec` simply instructs
Bison that the rule ‘`| '-' exp`’ has the same precedence as
`NEG`—in this case the next-to-highest. See [Context-Dependent Precedence](#Contextual-Precedence).

Here is a sample run of `calc.y`:

```
$ calc
4 + 4.5 - (34/(8*3+-3))
6.880952381
-56 + 2
-54
3 ^ 2
9
```

---

Next: [Location Tracking Calculator: `ltcalc`](#Location-Tracking-Calc), Previous: [Infix Notation Calculator: `calc`](#Infix-Calc), Up: [Examples](#Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 2.3 Simple Error Recovery

Up to this point, this manual has not addressed the issue of *error
recovery*—how to continue parsing after the parser detects a syntax
error. All we have handled is error reporting with `yyerror`.
Recall that by default `yyparse` returns after calling
`yyerror`. This means that an erroneous input line causes the
calculator program to exit. Now we show how to rectify this deficiency.

The Bison language itself includes the reserved word `error`, which
may be included in the grammar rules. In the example below it has
been added to one of the alternatives for `line`:

```
line:
  '\n'
| exp '\n'   { printf ("\t%.10g\n", $1); }
| error '\n' { yyerrok;                  }
;
```

This addition to the grammar allows for simple error recovery in the
event of a syntax error. If an expression that cannot be evaluated is
read, the error will be recognized by the third rule for `line`,
and parsing will continue. (The `yyerror` function is still called
upon to print its message as well.) The action executes the statement
`yyerrok`, a macro defined automatically by Bison; its meaning is
that error recovery is complete (see [Error Recovery](#Error-Recovery)). Note the
difference between `yyerrok` and `yyerror`; neither one is a
misprint.

This form of error recovery deals with syntax errors. There are other
kinds of errors; for example, division by zero, which raises an exception
signal that is normally fatal. A real calculator program must handle this
signal and use `longjmp` to return to `main` and resume parsing
input lines; it would also have to discard the rest of the current line of
input. We won’t discuss this issue further because it is not specific to
Bison programs.

---

Next: [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc), Previous: [Simple Error Recovery](#Simple-Error-Recovery), Up: [Examples](#Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 2.4 Location Tracking Calculator: `ltcalc`

This example extends the infix notation calculator with location
tracking. This feature will be used to improve the error messages. For
the sake of clarity, this example is a simple integer calculator, since
most of the work needed to use locations will be done in the lexical
analyzer.

* [Declarations for `ltcalc`](#Ltcalc-Declarations)
* [Grammar Rules for `ltcalc`](#Ltcalc-Rules)
* [The `ltcalc` Lexical Analyzer.](#Ltcalc-Lexer)

---

Next: [Grammar Rules for `ltcalc`](#Ltcalc-Rules), Up: [Location Tracking Calculator: `ltcalc`](#Location-Tracking-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.4.1 Declarations for `ltcalc`

The C and Bison declarations for the location tracking calculator are
the same as the declarations for the infix notation calculator.

```
/* Location tracking calculator. */

%{
  #include <math.h>
  int yylex (void);
  void yyerror (char const *);
%}

/* Bison declarations. */
%define api.value.type {int}
%token NUM

%left '-' '+'
%left '*' '/'
%precedence NEG
%right '^'

%% /* The grammar follows. */
```

Note there are no declarations specific to locations. Defining a data type
for storing locations is not needed: we will use the type provided by
default (see [Data Type of Locations](#Location-Type)), which is a four member structure with the
following integer fields: `first_line`, `first_column`,
`last_line` and `last_column`. By conventions, and in accordance
with the GNU Coding Standards and common practice, the line and column count
both start at 1.

---

Next: [The `ltcalc` Lexical Analyzer.](#Ltcalc-Lexer), Previous: [Declarations for `ltcalc`](#Ltcalc-Declarations), Up: [Location Tracking Calculator: `ltcalc`](#Location-Tracking-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.4.2 Grammar Rules for `ltcalc`

Whether handling locations or not has no effect on the syntax of your
language. Therefore, grammar rules for this example will be very close
to those of the previous example: we will only modify them to benefit
from the new information.

Here, we will use locations to report divisions by zero, and locate the
wrong expressions or subexpressions.

```
input:
  %empty
| input line
;
```

```

```

```
line:
  '\n'
| exp '\n' { printf ("%d\n", $1); }
;
```

```

```

```
exp:
  NUM
| exp '+' exp   { $$ = $1 + $3; }
| exp '-' exp   { $$ = $1 - $3; }
| exp '*' exp   { $$ = $1 * $3; }
```

```
| exp '/' exp
    {
      if ($3)
        $$ = $1 / $3;
      else
        {
          $$ = 1;
          fprintf (stderr, "%d.%d-%d.%d: division by zero",
                   @3.first_line, @3.first_column,
                   @3.last_line, @3.last_column);
        }
    }
```

```
| '-' exp %prec NEG     { $$ = -$2; }
| exp '^' exp           { $$ = pow ($1, $3); }
| '(' exp ')'           { $$ = $2; }
```

This code shows how to reach locations inside of semantic actions, by
using the pseudo-variables `@n` for rule components, and the
pseudo-variable `@$` for groupings.

We don’t need to assign a value to `@$`: the output parser does it
automatically. By default, before executing the C code of each action,
`@$` is set to range from the beginning of `@1` to the end of
`@n`, for a rule with n components. This behavior can be
redefined (see [Default Action for Locations](#Location-Default-Action)), and for very specific rules,
`@$` can be computed by hand.

---

Previous: [Grammar Rules for `ltcalc`](#Ltcalc-Rules), Up: [Location Tracking Calculator: `ltcalc`](#Location-Tracking-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.4.3 The `ltcalc` Lexical Analyzer.

Until now, we relied on Bison’s defaults to enable location
tracking. The next step is to rewrite the lexical analyzer, and make it
able to feed the parser with the token locations, as it already does for
semantic values.

To this end, we must take into account every single character of the
input text, to avoid the computed locations of being fuzzy or wrong:

```
int
yylex (void)
{
  int c;
```

```

```

```
  /* Skip white space. */
  while ((c = getchar ()) == ' ' || c == '\t')
    ++yylloc.last_column;
```

```

```

```
  /* Step. */
  yylloc.first_line = yylloc.last_line;
  yylloc.first_column = yylloc.last_column;
```

```

```

```
  /* Process numbers. */
  if (isdigit (c))
    {
      yylval = c - '0';
      ++yylloc.last_column;
      while (isdigit (c = getchar ()))
        {
          ++yylloc.last_column;
          yylval = yylval * 10 + c - '0';
        }
      ungetc (c, stdin);
      return NUM;
    }
```

```
  /* Return end-of-input. */
  if (c == EOF)
    return YYEOF;
```

```
  /* Return a single char, and update location. */
  if (c == '\n')
    {
      ++yylloc.last_line;
      yylloc.last_column = 0;
    }
  else
    ++yylloc.last_column;
  return c;
}
```

Basically, the lexical analyzer performs the same processing as before: it
skips blanks and tabs, and reads numbers or single-character tokens. In
addition, it updates `yylloc`, the global variable (of type
`YYLTYPE`) containing the token’s location.

Now, each time this function returns a token, the parser has its kind as
well as its semantic value, and its location in the text. The last needed
change is to initialize `yylloc`, for example in the controlling
function:

```
int
main (void)
{
  yylloc.first_line = yylloc.last_line = 1;
  yylloc.first_column = yylloc.last_column = 0;
  return yyparse ();
}
```

Remember that computing locations is not a matter of syntax. Every
character must be associated to a location update, whether it is in
valid input, in comments, in literal strings, and so on.

---

Next: [Exercises](#Exercises), Previous: [Location Tracking Calculator: `ltcalc`](#Location-Tracking-Calc), Up: [Examples](#Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 2.5 Multi-Function Calculator: `mfcalc`

Now that the basics of Bison have been discussed, it is time to move on to a
more advanced problem.[4](#FOOT4) The above calculators provided only
five functions, ‘`+`’, ‘`-`’, ‘`*`’, ‘`/`’ and ‘`^`’. It
would be nice to have a calculator that provides other mathematical
functions such as `sin`, `cos`, etc.

It is easy to add new operators to the infix calculator as long as they are
only single-character literals. The lexical analyzer `yylex` passes
back all nonnumeric characters as tokens, so new grammar rules suffice for
adding a new operator. But we want something more flexible: built-in
functions whose syntax has this form:

```
function_name (argument)
```

At the same time, we will add memory to the calculator, by allowing you
to create named variables, store values in them, and use them later.
Here is a sample session with the multi-function calculator:

```
$ mfcalc
pi = 3.141592653589
⇒ 3.1415926536
```

```
sin(pi)
⇒ 0.0000000000
```

```
alpha = beta1 = 2.3
⇒ 2.3000000000
alpha
⇒ 2.3000000000
ln(alpha)
⇒ 0.8329091229
exp(ln(beta1))
⇒ 2.3000000000
$
```

Note that multiple assignment and nested function calls are permitted.

* [Declarations for `mfcalc`](#Mfcalc-Declarations)
* [Grammar Rules for `mfcalc`](#Mfcalc-Rules)
* [The `mfcalc` Symbol Table](#Mfcalc-Symbol-Table)
* [The `mfcalc` Lexer](#Mfcalc-Lexer)
* [The `mfcalc` Main](#Mfcalc-Main)

---

Next: [Grammar Rules for `mfcalc`](#Mfcalc-Rules), Up: [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.5.1 Declarations for `mfcalc`

Here are the C and Bison declarations for the multi-function
calculator.

```
%{
  #include <stdio.h>  /* For printf, etc. */
  #include <math.h>   /* For pow, used in the grammar. */
  #include "calc.h"   /* Contains definition of 'symrec'. */
  int yylex (void);
  void yyerror (char const *);
%}
```

```
%define api.value.type union /* Generate YYSTYPE from these types: */
%token <double>  NUM     /* Double precision number. */
%token <symrec*> VAR FUN /* Symbol table pointer: variable/function. */
%nterm <double>  exp
```

```
%precedence '='
%left '-' '+'
%left '*' '/'
%precedence NEG /* negation--unary minus */
%right '^'      /* exponentiation */
```

The above grammar introduces only two new features of the Bison language.
These features allow semantic values to have various data types
(see [More Than One Value Type](#Multiple-Types)).

The special `union` value assigned to the `%define` variable
`api.value.type` specifies that the symbols are defined with their data
types. Bison will generate an appropriate definition of `YYSTYPE` to
store these values.

Since values can now have various types, it is necessary to associate a type
with each grammar symbol whose semantic value is used. These symbols are
`NUM`, `VAR`, `FUN`, and `exp`. Their declarations are
augmented with their data type (placed between angle brackets). For
instance, values of `NUM` are stored in `double`.

The Bison construct `%nterm` is used for declaring nonterminal symbols,
just as `%token` is used for declaring token kinds. Previously we did
not use `%nterm` before because nonterminal symbols are normally
declared implicitly by the rules that define them. But `exp` must be
declared explicitly so we can specify its value type. See [Nonterminal Symbols](#Type-Decl).

---

Next: [The `mfcalc` Symbol Table](#Mfcalc-Symbol-Table), Previous: [Declarations for `mfcalc`](#Mfcalc-Declarations), Up: [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.5.2 Grammar Rules for `mfcalc`

Here are the grammar rules for the multi-function calculator.
Most of them are copied directly from `calc`; three rules,
those which mention `VAR` or `FUN`, are new.

```
%% /* The grammar follows. */
```

```
input:
  %empty
| input line
;
```

```

```

```
line:
  '\n'
| exp '\n'   { printf ("%.10g\n", $1); }
| error '\n' { yyerrok;                }
;
```

```

```

```
exp:
  NUM
| VAR                { $$ = $1->value.var;              }
| VAR '=' exp        { $$ = $3; $1->value.var = $3;     }
| FUN '(' exp ')'    { $$ = $1->value.fun ($3);         }
| exp '+' exp        { $$ = $1 + $3;                    }
| exp '-' exp        { $$ = $1 - $3;                    }
| exp '*' exp        { $$ = $1 * $3;                    }
| exp '/' exp        { $$ = $1 / $3;                    }
| '-' exp  %prec NEG { $$ = -$2;                        }
| exp '^' exp        { $$ = pow ($1, $3);               }
| '(' exp ')'        { $$ = $2;                         }
;
```

```
/* End of grammar. */
%%
```

---

Next: [The `mfcalc` Lexer](#Mfcalc-Lexer), Previous: [Grammar Rules for `mfcalc`](#Mfcalc-Rules), Up: [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.5.3 The `mfcalc` Symbol Table

The multi-function calculator requires a symbol table to keep track of the
names and meanings of variables and functions. This doesn’t affect the
grammar rules (except for the actions) or the Bison declarations, but it
requires some additional C functions for support.

The symbol table itself consists of a linked list of records. Its
definition, which is kept in the header `calc.h`, is as follows. It
provides for either functions or variables to be placed in the table.

```
/* Function type. */
typedef double (func_t) (double);
```

```

```

```
/* Data type for links in the chain of symbols. */
struct symrec
{
  char *name;  /* name of symbol */
  int type;    /* type of symbol: either VAR or FUN */
  union
  {
    double var;    /* value of a VAR */
    func_t *fun;   /* value of a FUN */
  } value;
  struct symrec *next;  /* link field */
};
```

```

```

```
typedef struct symrec symrec;

/* The symbol table: a chain of 'struct symrec'. */
extern symrec *sym_table;

symrec *putsym (char const *name, int sym_type);
symrec *getsym (char const *name);
```

The new version of `main` will call `init_table` to initialize
the symbol table:

```
struct init
{
  char const *name;
  func_t *fun;
};
```

```

```

```
struct init const funs[] =
{
  { "atan", atan },
  { "cos",  cos  },
  { "exp",  exp  },
  { "ln",   log  },
  { "sin",  sin  },
  { "sqrt", sqrt },
  { 0, 0 },
};
```

```

```

```
/* The symbol table: a chain of 'struct symrec'. */
symrec *sym_table;
```

```

```

```
/* Put functions in table. */
static void
init_table (void)
```

```
{
  for (int i = 0; funs[i].name; i++)
    {
      symrec *ptr = putsym (funs[i].name, FUN);
      ptr->value.fun = funs[i].fun;
    }
}
```

By simply editing the initialization list and adding the necessary include
files, you can add additional functions to the calculator.

Two important functions allow look-up and installation of symbols in the
symbol table. The function `putsym` is passed a name and the kind
(`VAR` or `FUN`) of the object to be installed. The object is
linked to the front of the list, and a pointer to the object is returned.
The function `getsym` is passed the name of the symbol to look up. If
found, a pointer to that symbol is returned; otherwise zero is returned.

```
/* The mfcalc code assumes that malloc and realloc
   always succeed, and that integer calculations
   never overflow.  Production-quality code should
   not make these assumptions.  */
#include <assert.h>
#include <stdlib.h> /* malloc, realloc. */
#include <string.h> /* strlen. */
```

```

```

```
symrec *
putsym (char const *name, int sym_type)
{
  symrec *res = (symrec *) malloc (sizeof (symrec));
  res->name = strdup (name);
  res->type = sym_type;
  res->value.var = 0; /* Set value to 0 even if fun. */
  res->next = sym_table;
  sym_table = res;
  return res;
}
```

```

```

```
symrec *
getsym (char const *name)
{
  for (symrec *p = sym_table; p; p = p->next)
    if (strcmp (p->name, name) == 0)
      return p;
  return NULL;
}
```

---

Next: [The `mfcalc` Main](#Mfcalc-Main), Previous: [The `mfcalc` Symbol Table](#Mfcalc-Symbol-Table), Up: [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.5.4 The `mfcalc` Lexer

The function `yylex` must now recognize variables, numeric values, and
the single-character arithmetic operators. Strings of alphanumeric
characters with a leading letter are recognized as either variables or
functions depending on what the symbol table says about them.

The string is passed to `getsym` for look up in the symbol table. If
the name appears in the table, a pointer to its location and its type
(`VAR` or `FUN`) is returned to `yyparse`. If it is not
already in the table, then it is installed as a `VAR` using
`putsym`. Again, a pointer and its type (which must be `VAR`) is
returned to `yyparse`.

No change is needed in the handling of numeric values and arithmetic
operators in `yylex`.

```
#include <ctype.h>
#include <stddef.h>
```

```
int
yylex (void)
{
  int c = getchar ();

  /* Ignore white space, get first nonwhite character. */
  while (c == ' ' || c == '\t')
    c = getchar ();

  if (c == EOF)
    return YYEOF;
```

```

```

```
  /* Char starts a number => parse the number. */
  if (c == '.' || isdigit (c))
    {
      ungetc (c, stdin);
      if (scanf ("%lf", &yylval.NUM) != 1)
        abort ();
      return NUM;
    }
```

Bison generated a definition of `YYSTYPE` with a member named
`NUM` to store value of `NUM` symbols.

```
  /* Char starts an identifier => read the name. */
  if (isalpha (c))
    {
      static ptrdiff_t bufsize = 0;
      static char *symbuf = 0;
```

```
      ptrdiff_t i = 0;
      do
```

```
        {
          /* If buffer is full, make it bigger. */
          if (bufsize <= i)
            {
              bufsize = 2 * bufsize + 40;
              symbuf = realloc (symbuf, (size_t) bufsize);
            }
          /* Add this character to the buffer. */
          symbuf[i++] = (char) c;
          /* Get another character. */
          c = getchar ();
        }
```

```
      while (isalnum (c));

      ungetc (c, stdin);
      symbuf[i] = '\0';
```

```

```

```
      symrec *s = getsym (symbuf);
      if (!s)
        s = putsym (symbuf, VAR);
      yylval.VAR = s; /* or yylval.FUN = s. */
      return s->type;
    }

  /* Any other character is a token by itself. */
  return c;
}
```

---

Previous: [The `mfcalc` Lexer](#Mfcalc-Lexer), Up: [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 2.5.5 The `mfcalc` Main

The error reporting function is unchanged, and the new version of
`main` includes a call to `init_table` and sets the `yydebug`
on user demand (See [Tracing Your Parser](#Tracing), for details):

```
/* Called by yyparse on error. */
void yyerror (char const *s)
{
  fprintf (stderr, "%s\n", s);
}
```

```

```

```
int main (int argc, char const* argv[])
```

```
{
  /* Enable parse traces on option -p. */
  if (argc == 2 && strcmp(argv[1], "-p") == 0)
    yydebug = 1;
```

```
  init_table ();
  return yyparse ();
}
```

This program is both powerful and flexible. You may easily add new
functions, and it is a simple job to modify this code to install
predefined variables such as `pi` or `e` as well.

---

Previous: [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc), Up: [Examples](#Examples)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 2.6 Exercises

1. Add some new functions from `math.h` to the initialization list.
2. Add another array that contains constants and their values. Then modify
   `init_table` to add these constants to the symbol table. It will be
   easiest to give the constants type `VAR`.
3. Make the program report an error if the user refers to an uninitialized
   variable in any way except to store a value in it.

---

Next: [Parser C-Language Interface](#Interface), Previous: [Examples](#Examples), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 3 Bison Grammar Files

Bison takes as input a context-free grammar specification and produces a
C-language function that recognizes correct instances of the grammar.

The Bison grammar file conventionally has a name ending in ‘`.y`’.
See [Invoking Bison](#Invocation).

* [Outline of a Bison Grammar](#Grammar-Outline)
* [Symbols, Terminal and Nonterminal](#Symbols)
* [Grammar Rules](#Rules)
* [Defining Language Semantics](#Semantics)
* [Tracking Locations](#Tracking-Locations)
* [Named References](#Named-References)
* [Bison Declarations](#Declarations)
* [Multiple Parsers in the Same Program](#Multiple-Parsers)

---

Next: [Symbols, Terminal and Nonterminal](#Symbols), Up: [Bison Grammar Files](#Grammar-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]
