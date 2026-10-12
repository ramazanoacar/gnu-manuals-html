### 3.1 Outline of a Bison Grammar

A Bison grammar file has four main sections, shown here with the
appropriate delimiters:

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

Comments enclosed in ‘`/* … */`’ may appear in any of the sections.
As a GNU extension, ‘`//`’ introduces a comment that continues until end
of line.

* [The prologue](#Prologue)
* [Prologue Alternatives](#Prologue-Alternatives)
* [The Bison Declarations Section](#Bison-Declarations)
* [The Grammar Rules Section](#Grammar-Rules)
* [The epilogue](#Epilogue)

---

Next: [Prologue Alternatives](#Prologue-Alternatives), Up: [Outline of a Bison Grammar](#Grammar-Outline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.1.1 The prologue

The Prologue section contains macro definitions and declarations of
functions and variables that are used in the actions in the grammar rules.
These are copied to the beginning of the parser implementation file so that
they precede the definition of `yyparse`. You can use ‘`#include`’
to get the declarations from a header file. If you don’t need any C
declarations, you may omit the ‘`%{`’ and ‘`%}`’ delimiters that
bracket this section.

The Prologue section is terminated by the first occurrence of
‘`%}`’ that is outside a comment, a string literal, or a character
constant.

You may have more than one Prologue section, intermixed with the
Bison declarations. This allows you to have C and Bison declarations
that refer to each other. For example, the `%union` declaration may
use types defined in a header file, and you may wish to prototype functions
that take arguments of type `YYSTYPE`. This can be done with two
Prologue blocks, one before and one after the `%union`
declaration.

```
%{
  #define _GNU_SOURCE
  #include <stdio.h>
  #include "ptypes.h"
%}
```

```

```

```
%union {
  long n;
  tree t;  /* tree is defined in ptypes.h. */
}
```

```

```

```
%{
  static void print_token (yytoken_kind_t token, YYSTYPE val);
%}
```

```
…
```

When in doubt, it is usually safer to put prologue code before all Bison
declarations, rather than after. For example, any definitions of feature
test macros like `_GNU_SOURCE` or `_POSIX_C_SOURCE` should appear
before all Bison declarations, as feature test macros can affect the
behavior of Bison-generated `#include` directives.

---

Next: [The Bison Declarations Section](#Bison-Declarations), Previous: [The prologue](#Prologue), Up: [Outline of a Bison Grammar](#Grammar-Outline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.1.2 Prologue Alternatives

The functionality of Prologue sections can often be subtle and
inflexible. As an alternative, Bison provides a `%code` directive with
an explicit qualifier field, which identifies the purpose of the code and
thus the location(s) where Bison should generate it. For C/C++, the
qualifier can be omitted for the default location, or it can be one of
`requires`, `provides`, `top`. See [%code Summary](#g_t_0025code-Summary).

Look again at the example of the previous section:

```
%{
  #define _GNU_SOURCE
  #include <stdio.h>
  #include "ptypes.h"
%}
```

```

```

```
%union {
  long n;
  tree t;  /* tree is defined in ptypes.h. */
}
```

```

```

```
%{
  static void print_token (yytoken_kind_t token, YYSTYPE val);
%}
```

```
…
```

Notice that there are two Prologue sections here, but there’s a subtle
distinction between their functionality. For example, if you decide to
override Bison’s default definition for `YYLTYPE`, in which
Prologue section should you write your new
definition?[5](#FOOT5)
You should
write it in the first since Bison will insert that code into the parser
implementation file *before* the default `YYLTYPE` definition. In
which Prologue section should you prototype an internal function,
`trace_token`, that accepts `YYLTYPE` and `yytoken_kind_t` as
arguments? You should prototype it in the second since Bison will insert
that code *after* the `YYLTYPE` and `yytoken_kind_t`
definitions.

This distinction in functionality between the two Prologue sections is
established by the appearance of the `%union` between them. This
behavior raises a few questions. First, why should the position of a
`%union` affect definitions related to `YYLTYPE` and
`yytoken_kind_t`? Second, what if there is no `%union`? In that
case, the second kind of Prologue section is not available. This
behavior is not intuitive.

To avoid this subtle `%union` dependency, rewrite the example using a
`%code top` and an unqualified `%code`. Let’s go ahead and add
the new `YYLTYPE` definition and the `trace_token` prototype at
the same time:

```
%code top {
  #define _GNU_SOURCE
  #include <stdio.h>

  /* WARNING: The following code really belongs
   * in a '%code requires'; see below. */

  #include "ptypes.h"
  #define YYLTYPE YYLTYPE
  typedef struct YYLTYPE
  {
    int first_line;
    int first_column;
    int last_line;
    int last_column;
    char *filename;
  } YYLTYPE;
}
```

```
%union {
  long n;
  tree t;  /* tree is defined in ptypes.h. */
}
```

```

```

```
%code {
  static void print_token (yytoken_kind_t token, YYSTYPE val);
  static void trace_token (yytoken_kind_t token, YYLTYPE loc);
}
```

```
…
```

In this way, `%code top` and the unqualified `%code` achieve the
same functionality as the two kinds of Prologue sections, but it’s
always explicit which kind you intend. Moreover, both kinds are always
available even in the absence of `%union`.

The `%code top` block above logically contains two parts. The first
two lines before the warning need to appear near the top of the parser
implementation file. The first line after the warning is required by
`YYSTYPE` and thus also needs to appear in the parser implementation
file. However, if you’ve instructed Bison to generate a parser header file
(see [Bison Declaration Summary](#Decl-Summary)), you probably want that line to appear
before the `YYSTYPE` definition in that header file as well. The
`YYLTYPE` definition should also appear in the parser header file to
override the default `YYLTYPE` definition there.

In other words, in the `%code top` block above, all but the first two
lines are dependency code required by the `YYSTYPE` and `YYLTYPE`
definitions.
Thus, they belong in one or more `%code requires`:

```
%code top {
  #define _GNU_SOURCE
  #include <stdio.h>
}
```

```

```

```
%code requires {
  #include "ptypes.h"
}
```

```
%union {
  long n;
  tree t;  /* tree is defined in ptypes.h. */
}
```

```

```

```
%code requires {
  #define YYLTYPE YYLTYPE
  typedef struct YYLTYPE
  {
    int first_line;
    int first_column;
    int last_line;
    int last_column;
    char *filename;
  } YYLTYPE;
}
```

```

```

```
%code {
  static void print_token (yytoken_kind_t token, YYSTYPE val);
  static void trace_token (yytoken_kind_t token, YYLTYPE loc);
}
```

```
…
```

Now Bison will insert `#include "ptypes.h"` and the new `YYLTYPE`
definition before the Bison-generated `YYSTYPE` and `YYLTYPE`
definitions in both the parser implementation file and the parser header
file. (By the same reasoning, `%code requires` would also be the
appropriate place to write your own definition for `YYSTYPE`.)

When you are writing dependency code for `YYSTYPE` and `YYLTYPE`,
you should prefer `%code requires` over `%code top` regardless of
whether you instruct Bison to generate a parser header file. When you are
writing code that you need Bison to insert only into the parser
implementation file and that has no special need to appear at the top of
that file, you should prefer the unqualified `%code` over `%code
top`. These practices will make the purpose of each block of your code
explicit to Bison and to other developers reading your grammar file.
Following these practices, we expect the unqualified `%code` and
`%code requires` to be the most important of the four Prologue
alternatives.

At some point while developing your parser, you might decide to provide
`trace_token` to modules that are external to your parser. Thus, you
might wish for Bison to insert the prototype into both the parser header
file and the parser implementation file. Since this function is not a
dependency required by `YYSTYPE` or `YYLTYPE`, it doesn’t make
sense to move its prototype to a `%code requires`. More importantly,
since it depends upon `YYLTYPE` and `yytoken_kind_t`, `%code
requires` is not sufficient. Instead, move its prototype from the
unqualified `%code` to a `%code provides`:

```
%code top {
  #define _GNU_SOURCE
  #include <stdio.h>
}
```

```

```

```
%code requires {
  #include "ptypes.h"
}
```

```
%union {
  long n;
  tree t;  /* tree is defined in ptypes.h. */
}
```

```

```

```
%code requires {
  #define YYLTYPE YYLTYPE
  typedef struct YYLTYPE
  {
    int first_line;
    int first_column;
    int last_line;
    int last_column;
    char *filename;
  } YYLTYPE;
}
```

```

```

```
%code provides {
  void trace_token (yytoken_kind_t token, YYLTYPE loc);
}
```

```

```

```
%code {
  static void print_token (FILE *file, int token, YYSTYPE val);
}
```

```
…
```

Bison will insert the `trace_token` prototype into both the parser
header file and the parser implementation file after the definitions for
`yytoken_kind_t`, `YYLTYPE`, and `YYSTYPE`.

The above examples are careful to write directives in an order that reflects
the layout of the generated parser implementation and header files:
`%code top`, `%code requires`, `%code provides`, and then
`%code`. While your grammar files may generally be easier to read if
you also follow this order, Bison does not require it. Instead, Bison lets
you choose an organization that makes sense to you.

You may declare any of these directives multiple times in the grammar file.
In that case, Bison concatenates the contained code in declaration order.
This is the only way in which the position of one of these directives within
the grammar file affects its functionality.

The result of the previous two properties is greater flexibility in how you may
organize your grammar file.
For example, you may organize semantic-type-related directives by semantic
type:

```
%code requires { #include "type1.h" }
%union { type1 field1; }
%destructor { type1_free ($$); } <field1>
%printer { type1_print (yyo, $$); } <field1>
```

```

```

```
%code requires { #include "type2.h" }
%union { type2 field2; }
%destructor { type2_free ($$); } <field2>
%printer { type2_print (yyo, $$); } <field2>
```

You could even place each of the above directive groups in the rules section of
the grammar file next to the set of rules that uses the associated semantic
type.
(In the rules section, you must terminate each of those directives with a
semicolon.)
And you don’t have to worry that some directive (like a `%union`) in the
definitions section is going to adversely affect their functionality in some
counter-intuitive manner just because it comes first.
Such an organization is not possible using Prologue sections.

This section has been concerned with explaining the advantages of the four
Prologue alternatives over the original Yacc Prologue.
However, in most cases when using these directives, you shouldn’t need to
think about all the low-level ordering issues discussed here.
Instead, you should simply use these directives to label each block of your
code according to its purpose and let Bison handle the ordering.
`%code` is the most generic label.
Move code to `%code requires`, `%code provides`, or `%code top`
as needed.

---

Next: [The Grammar Rules Section](#Grammar-Rules), Previous: [Prologue Alternatives](#Prologue-Alternatives), Up: [Outline of a Bison Grammar](#Grammar-Outline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.1.3 The Bison Declarations Section

The Bison declarations section contains declarations that define
terminal and nonterminal symbols, specify precedence, and so on.
In some simple grammars you may not need any declarations.
See [Bison Declarations](#Declarations).

---

Next: [The epilogue](#Epilogue), Previous: [The Bison Declarations Section](#Bison-Declarations), Up: [Outline of a Bison Grammar](#Grammar-Outline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.1.4 The Grammar Rules Section

The *grammar rules* section contains one or more Bison grammar
rules, and nothing else. See [Grammar Rules](#Rules).

There must always be at least one grammar rule, and the first
‘`%%`’ (which precedes the grammar rules) may never be omitted even
if it is the first thing in the file.

---

Previous: [The Grammar Rules Section](#Grammar-Rules), Up: [Outline of a Bison Grammar](#Grammar-Outline)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.1.5 The epilogue

The Epilogue is copied verbatim to the end of the parser
implementation file, just as the Prologue is copied to the
beginning. This is the most convenient place to put anything that you
want to have in the parser implementation file but which need not come
before the definition of `yyparse`. For example, the definitions
of `yylex` and `yyerror` often go here. Because C requires
functions to be declared before being used, you often need to declare
functions like `yylex` and `yyerror` in the Prologue, even
if you define them in the Epilogue. See [Parser C-Language Interface](#Interface).

If the last section is empty, you may omit the ‘`%%`’ that separates it
from the grammar rules.

The Bison parser itself contains many macros and identifiers whose names
start with ‘`yy`’ or ‘`YY`’, so it is a good idea to avoid using
any such names (except those documented in this manual) in the epilogue
of the grammar file.

---

Next: [Grammar Rules](#Rules), Previous: [Outline of a Bison Grammar](#Grammar-Outline), Up: [Bison Grammar Files](#Grammar-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 3.2 Symbols, Terminal and Nonterminal

*Symbols* in Bison grammars represent the grammatical classifications
of the language.

A *terminal symbol* (also known as a *token kind*) represents a
class of syntactically equivalent tokens. You use the symbol in grammar
rules to mean that a token in that class is allowed. The symbol is
represented in the Bison parser by a numeric code, and the `yylex`
function returns a token kind code to indicate what kind of token has been
read. You don’t need to know what the code value is; you can use the symbol
to stand for it.

A *nonterminal symbol* stands for a class of syntactically
equivalent groupings. The symbol name is used in writing grammar rules.
By convention, it should be all lower case.

Symbol names can contain letters, underscores, periods, and non-initial
digits and dashes. Dashes in symbol names are a GNU extension, incompatible
with POSIX Yacc. Periods and dashes make symbol names less convenient to
use with named references, which require brackets around such names
(see [Named References](#Named-References)). Terminal symbols that contain periods or dashes
make little sense: since they are not valid symbols (in most programming
languages) they are not exported as token names.

There are three ways of writing terminal symbols in the grammar:

* A *named token kind* is written with an identifier, like an identifier
  in C. By convention, it should be all upper case. Each such name must be
  defined with a Bison declaration such as `%token`. See [Token Kind Names](#Token-Decl).
* A *character token kind* (or *literal character token*) is written
  in the grammar using the same syntax used in C for character constants; for
  example, `'+'` is a character token kind. A character token kind
  doesn’t need to be declared unless you need to specify its semantic value
  data type (see [Data Types of Semantic Values](#Value-Type)), associativity, or precedence
  (see [Operator Precedence](#Precedence)).

  By convention, a character token kind is used only to represent a token that
  consists of that particular character. Thus, the token kind `'+'` is
  used to represent the character ‘`+`’ as a token. Nothing enforces this
  convention, but if you depart from it, your program will confuse other
  readers.

  All the usual escape sequences used in character literals in C can be used
  in Bison as well, but you must not use the null character as a character
  literal because its numeric code, zero, signifies end-of-input
  (see [Calling Convention for `yylex`](#Calling-Convention)). Also, unlike standard C, trigraphs have no
  special meaning in Bison character literals, nor is backslash-newline
  allowed.
* A *literal string token* is written like a C string constant; for
  example, `"<="` is a literal string token. A literal string token
  doesn’t need to be declared unless you need to specify its semantic
  value data type (see [Data Types of Semantic Values](#Value-Type)), associativity, or precedence
  (see [Operator Precedence](#Precedence)).

  You can associate the literal string token with a symbolic name as an alias,
  using the `%token` declaration (see [Token Kind Names](#Token-Decl)). If you don’t do
  that, the lexical analyzer has to retrieve the token code for the literal
  string token from the `yytname` table (see [Calling Convention for `yylex`](#Calling-Convention)).

  **Warning**: literal string tokens do not work in Yacc.

  By convention, a literal string token is used only to represent a token
  that consists of that particular string. Thus, you should use the token
  kind `"<="` to represent the string ‘`<=`’ as a token. Bison
  does not enforce this convention, but if you depart from it, people who
  read your program will be confused.

  All the escape sequences used in string literals in C can be used in
  Bison as well, except that you must not use a null character within a
  string literal. Also, unlike Standard C, trigraphs have no special
  meaning in Bison string literals, nor is backslash-newline allowed. A
  literal string token must contain two or more characters; for a token
  containing just one character, use a character token (see above).

How you choose to write a terminal symbol has no effect on its
grammatical meaning. That depends only on where it appears in rules and
on when the parser function returns that symbol.

The value returned by `yylex` is always one of the terminal
symbols, except that a zero or negative value signifies end-of-input.
Whichever way you write the token kind in the grammar rules, you write
it the same way in the definition of `yylex`. The numeric code
for a character token kind is simply the positive numeric code of the
character, so `yylex` can use the identical value to generate the
requisite code, though you may need to convert it to `unsigned
char` to avoid sign-extension on hosts where `char` is signed.
Each named token kind becomes a C macro in the parser implementation
file, so `yylex` can use the name to stand for the code. (This
is why periods don’t make sense in terminal symbols.) See [Calling Convention for `yylex`](#Calling-Convention).

If `yylex` is defined in a separate file, you need to arrange for the
token-kind definitions to be available there. Use the `-d` option
when you run Bison, so that it will write these definitions into a separate
header file `name.tab.h` which you can include in the other
source files that need it. See [Invoking Bison](#Invocation).

If you want to write a grammar that is portable to any Standard C
host, you must use only nonnull character tokens taken from the basic
execution character set of Standard C. This set consists of the ten
digits, the 52 lower- and upper-case English letters, and the
characters in the following C-language string:

```
"\a\b\t\n\v\f\r !\"#%&'()*+,-./:;<=>?[\\]^_{|}~"
```

The `yylex` function and Bison must use a consistent character set
and encoding for character tokens. For example, if you run Bison in an
ASCII environment, but then compile and run the resulting
program in an environment that uses an incompatible character set like
EBCDIC, the resulting program may not work because the tables
generated by Bison will assume ASCII numeric values for
character tokens. It is standard practice for software distributions to
contain C source files that were generated by Bison in an
ASCII environment, so installers on platforms that are
incompatible with ASCII must rebuild those files before
compiling them.

The symbol `error` is a terminal symbol reserved for error recovery
(see [Error Recovery](#Error-Recovery)); you shouldn’t use it for any other purpose.
In particular, `yylex` should never return this value. The default
value of the error token is 256, unless you explicitly assigned 256 to
one of your tokens with a `%token` declaration.

---

Next: [Defining Language Semantics](#Semantics), Previous: [Symbols, Terminal and Nonterminal](#Symbols), Up: [Bison Grammar Files](#Grammar-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 3.3 Grammar Rules

A Bison grammar is a list of rules.

* [Syntax of Grammar Rules](#Rules-Syntax)
* [Empty Rules](#Empty-Rules)
* [Recursive Rules](#Recursion)

---

Next: [Empty Rules](#Empty-Rules), Up: [Grammar Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.3.1 Syntax of Grammar Rules

A Bison grammar rule has the following general form:

```
result: components…;
```

where result is the nonterminal symbol that this rule describes,
and components are various terminal and nonterminal symbols that
are put together by this rule (see [Symbols, Terminal and Nonterminal](#Symbols)).

For example,

```
exp: exp '+' exp;
```

says that two groupings of type `exp`, with a ‘`+`’ token in between,
can be combined into a larger grouping of type `exp`.

White space in rules is significant only to separate symbols. You can add
extra white space as you wish.

Scattered among the components can be actions that determine
the semantics of the rule. An action looks like this:

```
{C statements}
```

This is an example of *braced code*, that is, C code surrounded by
braces, much like a compound statement in C. Braced code can contain
any sequence of C tokens, so long as its braces are balanced. Bison
does not check the braced code for correctness directly; it merely
copies the code to the parser implementation file, where the C
compiler can check it.

Within braced code, the balanced-brace count is not affected by braces
within comments, string literals, or character constants, but it is
affected by the C digraphs ‘`<%`’ and ‘`%>`’ that represent
braces. At the top level braced code must be terminated by ‘`}`’
and not by a digraph. Bison does not look for trigraphs, so if braced
code uses trigraphs you should ensure that they do not affect the
nesting of braces or the boundaries of comments, string literals, or
character constants.

Usually there is only one action and it follows the components.
See [Actions](#Actions).

Multiple rules for the same result can be written separately or can
be joined with the vertical-bar character ‘`|`’ as follows:

```
result:
  rule1-components…
| rule2-components…
…
;
```

They are still considered distinct rules even when joined in this way.

---

Next: [Recursive Rules](#Recursion), Previous: [Syntax of Grammar Rules](#Rules-Syntax), Up: [Grammar Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.3.2 Empty Rules

A rule is said to be *empty* if its right-hand side (components)
is empty. It means that result in the previous example can match the
empty string. As another example, here is how to define an optional
semicolon:

```
semicolon.opt: | ";";
```

It is easy not to see an empty rule, especially when `|` is used. The
`%empty` directive allows to make explicit that a rule is empty on
purpose:

```
semicolon.opt:
  %empty
| ";"
;
```

Flagging a non-empty rule with `%empty` is an error. If run with
`-Wempty-rule`, `bison` will report empty rules without
`%empty`. Using `%empty` enables this warning, unless
`-Wno-empty-rule` was specified.

The `%empty` directive is a Bison extension, it does not work with
Yacc. To remain compatible with POSIX Yacc, it is customary to write a
comment ‘`/* empty */`’ in each rule with no components:

```
semicolon.opt:
  /* empty */
| ";"
;
```

---

Previous: [Empty Rules](#Empty-Rules), Up: [Grammar Rules](#Rules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.3.3 Recursive Rules

A rule is called *recursive* when its result nonterminal
appears also on its right hand side. Nearly all Bison grammars need to
use recursion, because that is the only way to define a sequence of any
number of a particular thing. Consider this recursive definition of a
comma-separated sequence of one or more expressions:

```
expseq1:
  exp
| expseq1 ',' exp
;
```

Since the recursive use of `expseq1` is the leftmost symbol in the
right hand side, we call this *left recursion*. By contrast, here
the same construct is defined using *right recursion*:

```
expseq1:
  exp
| exp ',' expseq1
;
```

Any kind of sequence can be defined using either left recursion or right
recursion, but you should always use left recursion, because it can
parse a sequence of any number of elements with bounded stack space.
Right recursion uses up space on the Bison stack in proportion to the
number of elements in the sequence, because all the elements must be
shifted onto the stack before the rule can be applied even once.
See [The Bison Parser Algorithm](#Algorithm), for further explanation
of this.

*Indirect* or *mutual* recursion occurs when the result of the
rule does not appear directly on its right hand side, but does appear
in rules for other nonterminals which do appear on its right hand
side.

For example:

```
expr:
  primary
| primary '+' primary
;
```

```

```

```
primary:
  constant
| '(' expr ')'
;
```

defines two mutually-recursive nonterminals, since each refers to the
other.

---

Next: [Tracking Locations](#Tracking-Locations), Previous: [Grammar Rules](#Rules), Up: [Bison Grammar Files](#Grammar-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 3.4 Defining Language Semantics

The grammar rules for a language determine only the syntax. The semantics
are determined by the semantic values associated with various tokens and
groupings, and by the actions taken when various groupings are recognized.

For example, the calculator calculates properly because the value
associated with each expression is the proper number; it adds properly
because the action for the grouping ‘`x + y`’ is to add
the numbers associated with x and y.

* [Data Types of Semantic Values](#Value-Type)
* [More Than One Value Type](#Multiple-Types)
* [Generating the Semantic Value Type](#Type-Generation)
* [The Union Declaration](#Union-Decl)
* [Providing a Structured Semantic Value Type](#Structured-Value-Type)
* [Actions](#Actions)
* [Data Types of Values in Actions](#Action-Types)
* [Actions in Midrule](#Midrule-Actions)

---

Next: [More Than One Value Type](#Multiple-Types), Up: [Defining Language Semantics](#Semantics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.1 Data Types of Semantic Values

In a simple program it may be sufficient to use the same data type for
the semantic values of all language constructs. This was true in the
RPN and infix calculator examples (see [Reverse Polish Notation Calculator](#RPN-Calc)).

Bison normally uses the type `int` for semantic values if your program
uses the same data type for all language constructs. To specify some other
type, define the `%define` variable `api.value.type` like this:

```
%define api.value.type {double}
```

or

```
%define api.value.type {struct semantic_value_type}
```

The value of `api.value.type` should be a type name that does not
contain parentheses or square brackets.

Alternatively in C, instead of relying of Bison’s `%define` support,
you may rely on the C preprocessor and define `YYSTYPE` as a macro:

```
#define YYSTYPE double
```

This macro definition must go in the prologue of the grammar file
(see [Outline of a Bison Grammar](#Grammar-Outline)). If compatibility with POSIX Yacc matters to you,
use this. Note however that Bison cannot know `YYSTYPE`’s value, not
even whether it is defined, so there are services it cannot provide.
Besides this works only for C.

---

Next: [Generating the Semantic Value Type](#Type-Generation), Previous: [Data Types of Semantic Values](#Value-Type), Up: [Defining Language Semantics](#Semantics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.2 More Than One Value Type

In most programs, you will need different data types for different kinds
of tokens and groupings. For example, a numeric constant may need type
`int` or `long`, while a string constant needs type
`char *`, and an identifier might need a pointer to an entry in the
symbol table.

To use more than one data type for semantic values in one parser, Bison
requires you to do two things:

* Specify the entire collection of possible data types. There are several
  options:
  + let Bison compute the union type from the tags you assign to symbols;
  + use the `%union` Bison declaration (see [The Union Declaration](#Union-Decl));
  + define the `%define` variable `api.value.type` to be a union type
    whose members are the type tags (see [Providing a Structured Semantic Value Type](#Structured-Value-Type));
  + use a `typedef` or a `#define` to define `YYSTYPE` to be a
    union type whose member names are the type tags.
* Choose one of those types for each symbol (terminal or nonterminal) for
  which semantic values are used. This is done for tokens with the
  `%token` Bison declaration (see [Token Kind Names](#Token-Decl)) and
  for groupings with the `%nterm`/`%type` Bison declarations
  (see [Nonterminal Symbols](#Type-Decl)).

---

Next: [The Union Declaration](#Union-Decl), Previous: [More Than One Value Type](#Multiple-Types), Up: [Defining Language Semantics](#Semantics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.3 Generating the Semantic Value Type

The special value `union` of the `%define` variable
`api.value.type` instructs Bison that the type tags (used with the
`%token`, `%nterm` and `%type` directives) are genuine types,
not names of members of `YYSTYPE`.

For example:

```
%define api.value.type union
%token <int> INT "integer"
%token <int> 'n'
%nterm <int> expr
%token <char const *> ID "identifier"
```

generates an appropriate value of `YYSTYPE` to support each symbol
type. The name of the member of `YYSTYPE` for tokens than have a
declared identifier id (such as `INT` and `ID` above, but
not `'n'`) is `id`. The other symbols have unspecified
names on which you should not depend; instead, relying on C casts to access
the semantic value with the appropriate type:

```
/* For an "integer". */
yylval.INT = 42;
return INT;

/* For an 'n', also declared as int. */
*((int*)&yylval) = 42;
return 'n';

/* For an "identifier". */
yylval.ID = "42";
return ID;
```

If the `%define` variable `api.token.prefix` is defined
(see [%define Summary](#g_t_0025define-Summary)), then it is also used to prefix
the union member names. For instance, with ‘`%define api.token.prefix
{TOK_}`’:

```
/* For an "integer". */
yylval.TOK_INT = 42;
return TOK_INT;
```

This Bison extension cannot work if `%yacc` (or
`-y`/`--yacc`) is enabled, as POSIX mandates that Yacc
generate tokens as macros (e.g., ‘`#define INT 258`’, or ‘`#define
TOK_INT 258`’).

A similar feature is provided for C++ that in addition overcomes C++
limitations (that forbid non-trivial objects to be part of a `union`):
‘`%define api.value.type variant`’, see [C++ Variants](#C_002b_002b-Variants).

---

Next: [Providing a Structured Semantic Value Type](#Structured-Value-Type), Previous: [Generating the Semantic Value Type](#Type-Generation), Up: [Defining Language Semantics](#Semantics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.4 The Union Declaration

The `%union` declaration specifies the entire collection of possible
data types for semantic values. The keyword `%union` is followed by
braced code containing the same thing that goes inside a `union` in C.

For example:

```
%union {
  double val;
  symrec *tptr;
}
```

This says that the two alternative types are `double` and `symrec
*`. They are given names `val` and `tptr`; these names are used
in the `%token`, `%nterm` and `%type` declarations to pick
one of the types for a terminal or nonterminal symbol (see [Nonterminal Symbols](#Type-Decl)).

As an extension to POSIX, a tag is allowed after the `%union`. For
example:

```
%union value {
  double val;
  symrec *tptr;
}
```

specifies the union tag `value`, so the corresponding C type is
`union value`. If you do not specify a tag, it defaults to
`YYSTYPE` (see [%define Summary](#g_t_0025define-Summary)).

As another extension to POSIX, you may specify multiple `%union`
declarations; their contents are concatenated. However, only the first
`%union` declaration can specify a tag.

Note that, unlike making a `union` declaration in C, you need not write
a semicolon after the closing brace.

---

Next: [Actions](#Actions), Previous: [The Union Declaration](#Union-Decl), Up: [Defining Language Semantics](#Semantics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.5 Providing a Structured Semantic Value Type

Instead of `%union`, you can define and use your own union type
`YYSTYPE` if your grammar contains at least one ‘`<type>`’
tag. For example, you can put the following into a header file
`parser.h`:

```
union YYSTYPE {
  double val;
  symrec *tptr;
};
```

and then your grammar can use the following instead of `%union`:

```
%{
#include "parser.h"
%}
%define api.value.type {union YYSTYPE}
%nterm <val> expr
%token <tptr> ID
```

Actually, you may also provide a `struct` rather that a `union`,
which may be handy if you want to track information for every symbol (such
as preceding comments).

The type you provide may even be structured and include pointers, in which
case the type tags you provide may be composite, with ‘`.`’ and ‘`->`’
operators.

---

Next: [Data Types of Values in Actions](#Action-Types), Previous: [Providing a Structured Semantic Value Type](#Structured-Value-Type), Up: [Defining Language Semantics](#Semantics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.6 Actions

An action accompanies a syntactic rule and contains C code to be executed
each time an instance of that rule is recognized. The task of most actions
is to compute a semantic value for the grouping built by the rule from the
semantic values associated with tokens or smaller groupings.

An action consists of braced code containing C statements, and can be
placed at any position in the rule;
it is executed at that position. Most rules have just one action at the
end of the rule, following all the components. Actions in the middle of
a rule are tricky and used only for special purposes (see [Actions in Midrule](#Midrule-Actions)).

The C code in an action can refer to the semantic values of the
components matched by the rule with the construct `$n`,
which stands for the value of the nth component. The semantic
value for the grouping being constructed is `$$`. In addition,
the semantic values of symbols can be accessed with the named
references construct `$name` or `$[name]`.
Bison translates both of these constructs into expressions of the
appropriate type when it copies the actions into the parser
implementation file. `$$` (or `$name`, when it stands
for the current grouping) is translated to a modifiable lvalue, so it
can be assigned to.

Here is a typical example:

```
exp:
…
| exp '+' exp     { $$ = $1 + $3; }
```

Or, in terms of named references:

```
exp[result]:
…
| exp[left] '+' exp[right]  { $result = $left + $right; }
```

This rule constructs an `exp` from two smaller `exp` groupings
connected by a plus-sign token. In the action, `$1` and `$3`
(`$left` and `$right`)
refer to the semantic values of the two component `exp` groupings,
which are the first and third symbols on the right hand side of the rule.
The sum is stored into `$$` (`$result`) so that it becomes the
semantic value of
the addition-expression just recognized by the rule. If there were a
useful semantic value associated with the ‘`+`’ token, it could be
referred to as `$2`.

See [Named References](#Named-References), for more information about using the named
references construct.

Note that the vertical-bar character ‘`|`’ is really a rule
separator, and actions are attached to a single rule. This is a
difference with tools like Flex, for which ‘`|`’ stands for either
“or”, or “the same action as that of the next rule”. In the
following example, the action is triggered only when ‘`b`’ is found:

```
a-or-b: 'a'|'b'   { a_or_b_found = 1; };
```

If you don’t specify an action for a rule, Bison supplies a default:
`$$ = $1`. Thus, the value of the first symbol in the rule
becomes the value of the whole rule. Of course, the default action is
valid only if the two data types match. There is no meaningful default
action for an empty rule; every empty rule must have an explicit action
unless the rule’s value does not matter.

`$n` with n zero or negative is allowed for reference
to tokens and groupings on the stack *before* those that match the
current rule. This is a very risky practice, and to use it reliably
you must be certain of the context in which the rule is applied. Here
is a case in which you can use this reliably:

```
foo:
  expr bar '+' expr  { … }
| expr bar '-' expr  { … }
;
```

```

```

```
bar:
  %empty    { previous_expr = $0; }
;
```

As long as `bar` is used only in the fashion shown here, `$0`
always refers to the `expr` which precedes `bar` in the
definition of `foo`.

It is also possible to access the semantic value of the lookahead token, if
any, from a semantic action.
This semantic value is stored in `yylval`.
See [Special Features for Use in Actions](#Action-Features).

---

Next: [Actions in Midrule](#Midrule-Actions), Previous: [Actions](#Actions), Up: [Defining Language Semantics](#Semantics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.7 Data Types of Values in Actions

If you have chosen a single data type for semantic values, the `$$`
and `$n` constructs always have that data type.

If you have used `%union` to specify a variety of data types, then you
must declare a choice among these types for each terminal or nonterminal
symbol that can have a semantic value. Then each time you use `$$` or
`$n`, its data type is determined by which symbol it refers to
in the rule. In this example,

```
exp:
  …
| exp '+' exp    { $$ = $1 + $3; }
```

`$1` and `$3` refer to instances of `exp`, so they all
have the data type declared for the nonterminal symbol `exp`. If
`$2` were used, it would have the data type declared for the
terminal symbol `'+'`, whatever that might be.

Alternatively, you can specify the data type when you refer to the value,
by inserting ‘`<type>`’ after the ‘`$`’ at the beginning of the
reference. For example, if you have defined types as shown here:

```
%union {
  int itype;
  double dtype;
}
```

then you can write `$<itype>1` to refer to the first subunit of the
rule as an integer, or `$<dtype>1` to refer to it as a double.

---

Previous: [Data Types of Values in Actions](#Action-Types), Up: [Defining Language Semantics](#Semantics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.8 Actions in Midrule

Occasionally it is useful to put an action in the middle of a rule.
These actions are written just like usual end-of-rule actions, but they
are executed before the parser even recognizes the following components.

* [Using Midrule Actions](#Using-Midrule-Actions)
* [Typed Midrule Actions](#Typed-Midrule-Actions)
* [Midrule Action Translation](#Midrule-Action-Translation)
* [Conflicts due to Midrule Actions](#Midrule-Conflicts)

---

Next: [Typed Midrule Actions](#Typed-Midrule-Actions), Up: [Actions in Midrule](#Midrule-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.8.1 Using Midrule Actions

A midrule action may refer to the components preceding it using
`$n`, but it may not refer to subsequent components because
it is run before they are parsed.

The midrule action itself counts as one of the components of the rule.
This makes a difference when there is another action later in the same rule
(and usually there is another at the end): you have to count the actions
along with the symbols when working out which number n to use in
`$n`.

The midrule action can also have a semantic value. The action can set
its value with an assignment to `$$`, and actions later in the rule
can refer to the value using `$n`. Since there is no symbol
to name the action, there is no way to declare a data type for the value
in advance, so you must use the ‘`$<…>n`’ construct to
specify a data type each time you refer to this value.

There is no way to set the value of the entire rule with a midrule
action, because assignments to `$$` do not have that effect. The
only way to set the value for the entire rule is with an ordinary action
at the end of the rule.

Here is an example from a hypothetical compiler, handling a `let`
statement that looks like ‘`let (variable) statement`’ and
serves to create a variable named variable temporarily for the
duration of statement. To parse this construct, we must put
variable into the symbol table while statement is parsed, then
remove it afterward. Here is how it is done:

```
stmt:
  "let" '(' var ')'
    {
      $<context>$ = push_context ();
      declare_variable ($3);
    }
  stmt
    {
      $$ = $6;
      pop_context ($<context>5);
    }
```

As soon as ‘`let (variable)`’ has been recognized, the first
action is run. It saves a copy of the current semantic context (the
list of accessible variables) as its semantic value, using alternative
`context` in the data-type union. Then it calls
`declare_variable` to add the new variable to that list. Once the
first action is finished, the embedded statement `stmt` can be
parsed.

Note that the midrule action is component number 5, so the ‘`stmt`’ is
component number 6. Named references can be used to improve the readability
and maintainability (see [Named References](#Named-References)):

```
stmt:
  "let" '(' var ')'
    {
      $<context>let = push_context ();
      declare_variable ($3);
    }[let]
  stmt
    {
      $$ = $6;
      pop_context ($<context>let);
    }
```

After the embedded statement is parsed, its semantic value becomes the
value of the entire `let`-statement. Then the semantic value from the
earlier action is used to restore the prior list of variables. This
removes the temporary `let`-variable from the list so that it won’t
appear to exist while the rest of the program is parsed.

Because the types of the semantic values of midrule actions are unknown to
Bison, type-based features (e.g., ‘`%printer`’, ‘`%destructor`’) do
not work, which could result in memory leaks. They also forbid the use of
the `variant` implementation of the `api.value.type` in C++
(see [C++ Variants](#C_002b_002b-Variants)).

See [Typed Midrule Actions](#Typed-Midrule-Actions), for one way to address this issue, and
[Midrule Action Translation](#Midrule-Action-Translation), for another: turning mid-action actions
into regular actions.

---

Next: [Midrule Action Translation](#Midrule-Action-Translation), Previous: [Using Midrule Actions](#Using-Midrule-Actions), Up: [Actions in Midrule](#Midrule-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.8.2 Typed Midrule Actions

In the above example, if the parser initiates error recovery (see [Error Recovery](#Error-Recovery)) while parsing the tokens in the embedded statement `stmt`,
it might discard the previous semantic context `$<context>5` without
restoring it. Thus, `$<context>5` needs a destructor
(see [Freeing Discarded Symbols](#Destructor-Decl)), and Bison needs the
type of the semantic value (`context`) to select the right destructor.

As an extension to Yacc’s midrule actions, Bison offers a means to type
their semantic value: specify its type tag (‘`<...>`’ before the midrule
action.

Consider the previous example, with an untyped midrule action:

```
stmt:
  "let" '(' var ')'
    {
      $<context>$ = push_context (); // ***
      declare_variable ($3);
    }
  stmt
    {
      $$ = $6;
      pop_context ($<context>5);     // ***
    }
```

If instead you write:

```
stmt:
  "let" '(' var ')'
    <context>{                       // ***
      $$ = push_context ();          // ***
      declare_variable ($3);
    }
  stmt
    {
      $$ = $6;
      pop_context ($5);              // ***
    }
```

then `%printer` and `%destructor` work properly (no more leaks!),
C++ `variant`s can be used, and redundancy is reduced (`<context>`
is specified once).

---

Next: [Conflicts due to Midrule Actions](#Midrule-Conflicts), Previous: [Typed Midrule Actions](#Typed-Midrule-Actions), Up: [Actions in Midrule](#Midrule-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.8.3 Midrule Action Translation

Midrule actions are actually transformed into regular rules and actions.
The various reports generated by Bison (textual, graphical, etc., see
[Understanding Your Parser](#Understanding)) reveal this translation,
best explained by means of an example. The following rule:

```
exp: { a(); } "b" { c(); } { d(); } "e" { f(); };
```

is translated into:

```
$@1: %empty { a(); };
$@2: %empty { c(); };
$@3: %empty { d(); };
exp: $@1 "b" $@2 $@3 "e" { f(); };
```

with new nonterminal symbols `$@n`, where n is a number.

A midrule action is expected to generate a value if it uses `$$`, or
the (final) action uses `$n` where n denote the midrule
action. In that case its nonterminal is rather named `@n`:

```
exp: { a(); } "b" { $$ = c(); } { d(); } "e" { f = $1; };
```

is translated into

```
@1: %empty { a(); };
@2: %empty { $$ = c(); };
$@3: %empty { d(); };
exp: @1 "b" @2 $@3 "e" { f = $1; }
```

There are probably two errors in the above example: the first midrule action
does not generate a value (it does not use `$$` although the final
action uses it), and the value of the second one is not used (the final
action does not use `$3`). Bison reports these errors when the
`midrule-value` warnings are enabled (see [Invoking Bison](#Invocation)):

```
$ bison -Wmidrule-value mid.y
```

```
mid.y:2.6-13: warning: unset value: $$
    2 | exp: { a(); } "b" { $$ = c(); } { d(); } "e" { f = $1; };
      |      ^~~~~~~~
```

```
mid.y:2.19-31: warning: unused value: $3
    2 | exp: { a(); } "b" { $$ = c(); } { d(); } "e" { f = $1; };
      |                   ^~~~~~~~~~~~~
```

  

It is sometimes useful to turn midrule actions into regular actions, e.g.,
to factor them, or to escape from their limitations. For instance, as an
alternative to *typed* midrule action, you may bury the midrule action
inside a nonterminal symbol and to declare a printer and a destructor for
that symbol:

```
%nterm <context> let
%destructor { pop_context ($$); } let
%printer { print_context (yyo, $$); } let
```

```
%%
```

```
stmt:
  let stmt
    {
      $$ = $2;
      pop_context ($let);
    };
```

```

```

```
let:
  "let" '(' var ')'
    {
      $let = push_context ();
      declare_variable ($var);
    };
```

---

Previous: [Midrule Action Translation](#Midrule-Action-Translation), Up: [Actions in Midrule](#Midrule-Actions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.4.8.4 Conflicts due to Midrule Actions

Taking action before a rule is completely recognized often leads to
conflicts since the parser must commit to a parse in order to execute the
action. For example, the following two rules, without midrule actions,
can coexist in a working parser because the parser can shift the open-brace
token and look at what follows before deciding whether there is a
declaration or not:

```
compound:
  '{' declarations statements '}'
| '{' statements '}'
;
```

But when we add a midrule action as follows, the rules become nonfunctional:

```
compound:
  { prepare_for_local_variables (); }
     '{' declarations statements '}'
```

```
|    '{' statements '}'
;
```

Now the parser is forced to decide whether to run the midrule action
when it has read no farther than the open-brace. In other words, it
must commit to using one rule or the other, without sufficient
information to do it correctly. (The open-brace token is what is called
the *lookahead* token at this time, since the parser is still
deciding what to do about it. See [Lookahead Tokens](#Lookahead).)

You might think that you could correct the problem by putting identical
actions into the two rules, like this:

```
compound:
  { prepare_for_local_variables (); }
    '{' declarations statements '}'
| { prepare_for_local_variables (); }
    '{' statements '}'
;
```

But this does not help, because Bison does not realize that the two actions
are identical. (Bison never tries to understand the C code in an action.)

If the grammar is such that a declaration can be distinguished from a
statement by the first token (which is true in C), then one solution which
does work is to put the action after the open-brace, like this:

```
compound:
  '{' { prepare_for_local_variables (); }
    declarations statements '}'
| '{' statements '}'
;
```

Now the first token of the following declaration or statement,
which would in any case tell Bison which rule to use, can still do so.

Another solution is to bury the action inside a nonterminal symbol which
serves as a subroutine:

```
subroutine:
  %empty  { prepare_for_local_variables (); }
;
```

```

```

```
compound:
  subroutine '{' declarations statements '}'
| subroutine '{' statements '}'
;
```

Now Bison can execute the action in the rule for `subroutine` without
deciding which rule for `compound` it will eventually use.

---

Next: [Named References](#Named-References), Previous: [Defining Language Semantics](#Semantics), Up: [Bison Grammar Files](#Grammar-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 3.5 Tracking Locations

Though grammar rules and semantic actions are enough to write a fully
functional parser, it can be useful to process some additional information,
especially symbol locations.

The way locations are handled is defined by providing a data type, and
actions to take when rules are matched.

* [Data Type of Locations](#Location-Type)
* [Actions and Locations](#Actions-and-Locations)
* [Printing Locations](#Printing-Locations)
* [Default Action for Locations](#Location-Default-Action)

---

Next: [Actions and Locations](#Actions-and-Locations), Up: [Tracking Locations](#Tracking-Locations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.5.1 Data Type of Locations

Defining a data type for locations is much simpler than for semantic values,
since all tokens and groupings always use the same type. The location type
is specified using ‘`%define api.location.type`’:

```
%define api.location.type {location_t}
```

This defines, in the C generated code, the `YYLTYPE` type name. When
`YYLTYPE` is not defined, Bison uses a default structure type with four
members:

```
typedef struct YYLTYPE
{
  int first_line;
  int first_column;
  int last_line;
  int last_column;
} YYLTYPE;
```

In C, you may also specify the type of locations by defining a macro called
`YYLTYPE`, just as you can specify the semantic value type by defining
a `YYSTYPE` macro (see [Data Types of Semantic Values](#Value-Type)). However, rather than using
macros, we recommend the `api.value.type` and `api.location.type`
`%define` variables.

Default locations represent a range in the source file(s), but this is not a
requirement. It could be a single point or just a line number, or even more
complex structures.

When the default location type is used, Bison initializes all these fields
to 1 for `yylloc` at the beginning of the parsing. To initialize
`yylloc` with a custom location type (or to chose a different
initialization), use the `%initial-action` directive. See [Performing Actions before Parsing](#Initial-Action-Decl).

---

Next: [Printing Locations](#Printing-Locations), Previous: [Data Type of Locations](#Location-Type), Up: [Tracking Locations](#Tracking-Locations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.5.2 Actions and Locations

Actions are not only useful for defining language semantics, but also for
describing the behavior of the output parser with locations.

The most obvious way for building locations of syntactic groupings is very
similar to the way semantic values are computed. In a given rule, several
constructs can be used to access the locations of the elements being matched.
The location of the nth component of the right hand side is
`@n`, while the location of the left hand side grouping is
`@$`.

In addition, the named references construct `@name` and
`@[name]` may also be used to address the symbol locations.
See [Named References](#Named-References), for more information about using the named
references construct.

Here is a basic example using the default data type for locations:

```
exp:
  …
| exp '/' exp
    {
      @$.first_column = @1.first_column;
      @$.first_line = @1.first_line;
      @$.last_column = @3.last_column;
      @$.last_line = @3.last_line;
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

As for semantic values, there is a default action for locations that is
run each time a rule is matched. It sets the beginning of `@$` to the
beginning of the first symbol, and the end of `@$` to the end of the
last symbol.

With this default action, the location tracking can be fully automatic. The
example above simply rewrites this way:

```
exp:
  …
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

It is also possible to access the location of the lookahead token, if any,
from a semantic action.
This location is stored in `yylloc`.
See [Special Features for Use in Actions](#Action-Features).

---

Next: [Default Action for Locations](#Location-Default-Action), Previous: [Actions and Locations](#Actions-and-Locations), Up: [Tracking Locations](#Tracking-Locations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.5.3 Printing Locations

When using the default location type, the debug traces report the symbols’
location. The generated parser does so using the `YYLOCATION_PRINT`
macro.

Macro: **YYLOCATION\_PRINT** *(file, loc)`;`* [¶](#index-YYLOCATION_005fPRINT)
:   When traces are enabled, print loc (of type ‘`YYLTYPE const *`’) on
    file (of type ‘`FILE *`’). Do nothing when traces are disabled, or
    if the location type is user defined.

To get locations in the debug traces with your user-defined location types,
define the `YYLOCATION_PRINT` macro. For instance:

```
#define YYLOCATION_PRINT   location_print
```

---

Previous: [Printing Locations](#Printing-Locations), Up: [Tracking Locations](#Tracking-Locations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.5.4 Default Action for Locations

Actually, actions are not the best place to compute locations. Since
locations are much more general than semantic values, there is room in
the output parser to redefine the default action to take for each
rule. The `YYLLOC_DEFAULT` macro is invoked each time a rule is
matched, before the associated action is run. It is also invoked
while processing a syntax error, to compute the error’s location.
Before reporting an unresolvable syntactic ambiguity, a GLR
parser invokes `YYLLOC_DEFAULT` recursively to compute the location
of that ambiguity.

Most of the time, this macro is general enough to suppress location
dedicated code from semantic actions.

The `YYLLOC_DEFAULT` macro takes three parameters. The first one is
the location of the grouping (the result of the computation). When a
rule is matched, the second parameter identifies locations of
all right hand side elements of the rule being matched, and the third
parameter is the size of the rule’s right hand side.
When a GLR parser reports an ambiguity, which of multiple candidate
right hand sides it passes to `YYLLOC_DEFAULT` is undefined.
When processing a syntax error, the second parameter identifies locations
of the symbols that were discarded during error processing, and the third
parameter is the number of discarded symbols.

By default, `YYLLOC_DEFAULT` is defined this way:

```
# define YYLLOC_DEFAULT(Cur, Rhs, N)                      \
do                                                        \
  if (N)                                                  \
    {                                                     \
      (Cur).first_line   = YYRHSLOC(Rhs, 1).first_line;   \
      (Cur).first_column = YYRHSLOC(Rhs, 1).first_column; \
      (Cur).last_line    = YYRHSLOC(Rhs, N).last_line;    \
      (Cur).last_column  = YYRHSLOC(Rhs, N).last_column;  \
    }                                                     \
  else                                                    \
    {                                                     \
      (Cur).first_line   = (Cur).last_line   =            \
        YYRHSLOC(Rhs, 0).last_line;                       \
      (Cur).first_column = (Cur).last_column =            \
        YYRHSLOC(Rhs, 0).last_column;                     \
    }                                                     \
while (0)
```

where `YYRHSLOC (rhs, k)` is the location of the kth symbol
in rhs when k is positive, and the location of the symbol
just before the reduction when k and n are both zero.

When defining `YYLLOC_DEFAULT`, you should consider that:

* All arguments are free of side-effects. However, only the first one (the
  result) should be modified by `YYLLOC_DEFAULT`.
* For consistency with semantic actions, valid indexes within the
  right hand side range from 1 to n. When n is zero, only 0 is a
  valid index, and it refers to the symbol just before the reduction.
  During error processing n is always positive.
* Your macro should parenthesize its arguments, if need be, since the
  actual arguments may not be surrounded by parentheses. Also, your
  macro should expand to something that can be used as a single
  statement when it is followed by a semicolon.

---

Next: [Bison Declarations](#Declarations), Previous: [Tracking Locations](#Tracking-Locations), Up: [Bison Grammar Files](#Grammar-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 3.6 Named References

As described in the preceding sections, the traditional way to refer to any
semantic value or location is a *positional reference*, which takes the
form `$n`, `$$`, `@n`, and `@$`. However,
such a reference is not very descriptive. Moreover, if you later decide to
insert or remove symbols in the right-hand side of a grammar rule, the need
to renumber such references can be tedious and error-prone.

To avoid these issues, you can also refer to a semantic value or location
using a *named reference*. First of all, original symbol names may be
used as named references. For example:

```
invocation: op '(' args ')'
  { $invocation = new_invocation ($op, $args, @invocation); }
```

Positional and named references can be mixed arbitrarily. For example:

```
invocation: op '(' args ')'
  { $$ = new_invocation ($op, $args, @$); }
```

However, sometimes regular symbol names are not sufficient due to
ambiguities:

```
exp: exp '/' exp
  { $exp = $exp / $exp; } // $exp is ambiguous.

exp: exp '/' exp
  { $$ = $1 / $exp; } // One usage is ambiguous.

exp: exp '/' exp
  { $$ = $1 / $3; } // No error.
```

When ambiguity occurs, explicitly declared names may be used for values and
locations. Explicit names are declared as a bracketed name after a symbol
appearance in rule definitions. For example:

```
exp[result]: exp[left] '/' exp[right]
  { $result = $left / $right; }
```

In order to access a semantic value generated by a midrule action, an
explicit name may also be declared by putting a bracketed name after the
closing brace of the midrule action code:

```
exp[res]: exp[x] '+' {$left = $x;}[left] exp[right]
  { $res = $left + $right; }
```

In references, in order to specify names containing dots and dashes, an explicit
bracketed syntax `$[name]` and `@[name]` must be used:

```
if-stmt: "if" '(' expr ')' "then" then.stmt ';'
  { $[if-stmt] = new_if_stmt ($expr, $[then.stmt]); }
```

It often happens that named references are followed by a dot, dash or other
C punctuation marks and operators. By default, Bison will read
‘`$name.suffix`’ as a reference to symbol value `$name` followed by
‘`.suffix`’, i.e., an access to the `suffix` field of the semantic
value. In order to force Bison to recognize ‘`name.suffix`’ in its
entirety as the name of a semantic value, the bracketed syntax
‘`$[name.suffix]`’ must be used.

---

Next: [Multiple Parsers in the Same Program](#Multiple-Parsers), Previous: [Named References](#Named-References), Up: [Bison Grammar Files](#Grammar-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]
