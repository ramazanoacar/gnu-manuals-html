### 3.7 Bison Declarations

The *Bison declarations* section of a Bison grammar defines the symbols
used in formulating the grammar and the data types of semantic values.
See [Symbols, Terminal and Nonterminal](#Symbols).

All token kind names (but not single-character literal tokens such as
`'+'` and `'*'`) must be declared. Nonterminal symbols must be
declared if you need to specify which data type to use for the semantic
value (see [More Than One Value Type](#Multiple-Types)).

The first rule in the grammar file also specifies the start symbol, by
default. If you want some other symbol to be the start symbol, you
must declare it explicitly (see [Languages and Context-Free Grammars](#Language-and-Grammar)).

* [Require a Version of Bison](#Require-Decl)
* [Token Kind Names](#Token-Decl)
* [Operator Precedence](#Precedence-Decl)
* [Nonterminal Symbols](#Type-Decl)
* [Syntax of Symbol Declarations](#Symbol-Decls)
* [Performing Actions before Parsing](#Initial-Action-Decl)
* [Freeing Discarded Symbols](#Destructor-Decl)
* [Printing Semantic Values](#Printer-Decl)
* [Suppressing Conflict Warnings](#Expect-Decl)
* [The Start-Symbol](#Start-Decl)
* [A Pure (Reentrant) Parser](#Pure-Decl)
* [A Push Parser](#Push-Decl)
* [Bison Declaration Summary](#Decl-Summary)
* [%define Summary](#g_t_0025define-Summary)
* [%code Summary](#g_t_0025code-Summary)

---

Next: [Token Kind Names](#Token-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.1 Require a Version of Bison

You may require the minimum version of Bison to process the grammar. If
the requirement is not met, `bison` exits with an error (exit
status 63).

```
%require "version"
```

Some deprecated behaviors are disabled for some required version:

`"3.2"` (or better)
:   The C++ deprecated files `position.hh` and `stack.hh` are no
    longer generated.

---

Next: [Operator Precedence](#Precedence-Decl), Previous: [Require a Version of Bison](#Require-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.2 Token Kind Names

The basic way to declare a token kind name (terminal symbol) is as follows:

```
%token name
```

Bison will convert this into a definition in the parser, so that the
function `yylex` (if it is in this file) can use the name name to
stand for this token kind’s code.

Alternatively, you can use `%left`, `%right`, `%precedence`,
or `%nonassoc` instead of `%token`, if you wish to specify
associativity and precedence. See [Operator Precedence](#Precedence-Decl). However, for
clarity, we recommend to use these directives only to declare associativity
and precedence, and not to add string aliases, semantic types, etc.

You can explicitly specify the numeric code for a token kind by appending a
nonnegative decimal or hexadecimal integer value in the field immediately
following the token name:

```
%token NUM 300
%token XNUM 0x12d // a GNU extension
```

It is generally best, however, to let Bison choose the numeric codes for all
token kinds. Bison will automatically select codes that don’t conflict with
each other or with normal characters.

In the event that the stack type is a union, you must augment the
`%token` or other token declaration to include the data type
alternative delimited by angle-brackets (see [More Than One Value Type](#Multiple-Types)).

For example:

```
%union {              /* define stack type */
  double val;
  symrec *tptr;
}
%token <val> NUM      /* define token NUM and its type */
```

You can associate a literal string token with a token kind name by writing
the literal string at the end of a `%token` declaration which declares
the name. For example:

```
%token ARROW "=>"
```

For example, a grammar for the C language might specify these names with
equivalent literal string tokens:

```
%token  <operator>  OR      "||"
%token  <operator>  LE 134  "<="
%left  OR  "<="
```

Once you equate the literal string and the token kind name, you can use them
interchangeably in further declarations or the grammar rules. The
`yylex` function can use the token name or the literal string to obtain
the token kind code (see [Calling Convention for `yylex`](#Calling-Convention)).

String aliases allow for better error messages using the literal strings
instead of the token names, such as ‘`syntax error, unexpected ||,
expecting number or (`’ rather than ‘`syntax error, unexpected OR,
expecting NUM or LPAREN`’.

String aliases may also be marked for internationalization (see [Token Internationalization](#Token-I18n)):

```
%token
    OR     "||"
    LPAREN "("
    RPAREN ")"
    '\n'   _("end of line")
  <double>
    NUM    _("number")
```

would produce in French ‘`erreur de syntaxe, || inattendu, attendait
nombre ou (`’ rather than ‘`erreur de syntaxe, || inattendu, attendait
number ou (`’.

---

Next: [Nonterminal Symbols](#Type-Decl), Previous: [Token Kind Names](#Token-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.3 Operator Precedence

Use the `%left`, `%right`, `%nonassoc`, or `%precedence`
declaration to declare a token and specify its precedence and associativity,
all at once. These are called *precedence declarations*.
See [Operator Precedence](#Precedence), for general information on operator
precedence.

The syntax of a precedence declaration is nearly the same as that of
`%token`: either

```
%left symbols…
```

or

```
%left <type> symbols…
```

And indeed any of these declarations serves the purposes of `%token`.
But in addition, they specify the associativity and relative precedence for
all the symbols:

* The associativity of an operator op determines how repeated uses of
  the operator nest: whether ‘`x op y op z`’
  is parsed by grouping x with y first or by grouping y with
  z first. `%left` specifies left-associativity (grouping x
  with y first) and `%right` specifies right-associativity
  (grouping y with z first). `%nonassoc` specifies no
  associativity, which means that ‘`x op y op
  z`’ is considered a syntax error.

  `%precedence` gives only precedence to the symbols, and defines
  no associativity at all. Use this to define precedence only, and leave any
  potential conflict due to associativity enabled.
* The precedence of an operator determines how it nests with other operators.
  All the tokens declared in a single precedence declaration have equal
  precedence and nest together according to their associativity. When two
  tokens declared in different precedence declarations associate, the one
  declared later has the higher precedence and is grouped first.

For backward compatibility, there is a confusing difference between the
argument lists of `%token` and precedence declarations. Only a
`%token` can associate a literal string with a token kind name. A
precedence declaration always interprets a literal string as a reference to
a separate token. For example:

```
%left  OR "<="         // Does not declare an alias.
%left  OR 134 "<=" 135 // Declares 134 for OR and 135 for "<=".
```

---

Next: [Syntax of Symbol Declarations](#Symbol-Decls), Previous: [Operator Precedence](#Precedence-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.4 Nonterminal Symbols

When you use `%union` to specify multiple value types, you must
declare the value type of each nonterminal symbol for which values are
used. This is done with a `%type` declaration, like this:

```
%type <type> nonterminal…
```

Here nonterminal is the name of a nonterminal symbol, and type
is the name given in the `%union` to the alternative that you want
(see [The Union Declaration](#Union-Decl)). You can give any number of nonterminal symbols in the
same `%type` declaration, if they have the same value type. Use spaces
to separate the symbol names.

While POSIX Yacc allows `%type` only for nonterminals, Bison accepts
that this directive be also applied to terminal symbols. To declare
exclusively nonterminal symbols, use the safer `%nterm`:

```
%nterm <type> nonterminal…
```

---

Next: [Performing Actions before Parsing](#Initial-Action-Decl), Previous: [Nonterminal Symbols](#Type-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.5 Syntax of Symbol Declarations

The syntax of the various directives to declare symbols is as follows.

```
%token tag? ( id number? string? )+ ( tag ( id number? string? )+ )*
%left  tag? ( id number?)+ ( tag ( id number? )+ )*
%type  tag? ( id | char | string )+ ( tag ( id | char | string )+ )*
%nterm tag? id+ ( tag id+ )*
```

where tag denotes a type tag such as ‘`<ival>`’, id denotes
an identifier such as ‘`NUM`’, number a decimal or hexadecimal
integer such as ‘`300`’ or ‘`0x12d`’, char a character literal
such as ‘`'+'`’, and string a string literal such as
‘`"number"`’. The postfix quantifiers are ‘`?`’ (zero or one),
‘`*`’ (zero or more) and ‘`+`’ (one or more).

The directives `%precedence`, `%right` and `%nonassoc` behave
like `%left`.

---

Next: [Freeing Discarded Symbols](#Destructor-Decl), Previous: [Syntax of Symbol Declarations](#Symbol-Decls), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.6 Performing Actions before Parsing

Sometimes your parser needs to perform some initializations before parsing.
The `%initial-action` directive allows for such arbitrary code.

Directive: **%initial-action** *{ code }* [¶](#index-_0025initial_002daction-1)
:   Declare that the braced code must be invoked before parsing each time
    `yyparse` is called. The code may use `$$` (or
    `$<tag>$`) and `@$` — initial value and location of the
    lookahead — and the `%parse-param`.

For instance, if your locations use a file name, you may use

```
%parse-param { char const *file_name };
%initial-action
{
  @$.initialize (file_name);
};
```

---

Next: [Printing Semantic Values](#Printer-Decl), Previous: [Performing Actions before Parsing](#Initial-Action-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.7 Freeing Discarded Symbols

During error recovery (see [Error Recovery](#Error-Recovery)), symbols already pushed on
the stack and tokens coming from the rest of the file are discarded until
the parser falls on its feet. If the parser runs out of memory, or if it
returns via `YYABORT`, `YYACCEPT` or `YYNOMEM`, all the
symbols on the stack must be discarded. Even if the parser succeeds, it
must discard the start symbol.

When discarded symbols convey heap based information, this memory is
lost. While this behavior can be tolerable for batch parsers, such as
in traditional compilers, it is unacceptable for programs like shells or
protocol implementations that may parse and execute indefinitely.

The `%destructor` directive defines code that is called when a
symbol is automatically discarded.

Directive: **%destructor** *{ code } symbols* [¶](#index-_0025destructor-2)
:   Invoke the braced code whenever the parser discards one of the
    symbols. Within code, `$$` (or `$<tag>$`)
    designates the semantic value associated with the discarded symbol, and
    `@$` designates its location. The additional parser parameters are
    also available (see [The Parser Function `yyparse`](#Parser-Function)).

    When a symbol is listed among symbols, its `%destructor` is called a
    per-symbol `%destructor`.
    You may also define a per-type `%destructor` by listing a semantic type
    tag among symbols.
    In that case, the parser will invoke this code whenever it discards any
    grammar symbol that has that semantic type tag unless that symbol has its own
    per-symbol `%destructor`.

    Finally, you can define two different kinds of default `%destructor`s.
    You can place each of `<*>` and `<>` in the symbols list of
    exactly one `%destructor` declaration in your grammar file.
    The parser will invoke the code associated with one of these whenever it
    discards any user-defined grammar symbol that has no per-symbol and no per-type
    `%destructor`.
    The parser uses the code for `<*>` in the case of such a grammar
    symbol for which you have formally declared a semantic type tag (`%token`,
    `%nterm`, and `%type`
    count as such a declaration, but `$<tag>$` does not).
    The parser uses the code for `<>` in the case of such a grammar
    symbol that has no declared semantic type tag.

For example:

```
%union { char *string; }
%token <string> STRING1 STRING2
%nterm <string> string1 string2
%union { char character; }
%token <character> CHR
%nterm <character> chr
%token TAGLESS

%destructor { } <character>
%destructor { free ($$); } <*>
%destructor { free ($$); printf ("%d", @$.first_line); } STRING1 string1
%destructor { printf ("Discarding tagless symbol.\n"); } <>
```

guarantees that, when the parser discards any user-defined symbol that has a
semantic type tag other than `<character>`, it passes its semantic value
to `free` by default.
However, when the parser discards a `STRING1` or a `string1`,
it uses the third `%destructor`, which frees it and
prints its line number to `stdout` (`free` is invoked only once).
Finally, the parser merely prints a message whenever it discards any symbol,
such as `TAGLESS`, that has no semantic type tag.

A Bison-generated parser invokes the default `%destructor`s only for
user-defined as opposed to Bison-defined symbols.
For example, the parser will not invoke either kind of default
`%destructor` for the special Bison-defined symbols `$accept`,
`$undefined`, or `$end` (see [Bison Symbols](#Table-of-Symbols)),
none of which you can reference in your grammar.
It also will not invoke either for the `error` token (see [Bison Symbols](#Table-of-Symbols)), which is always defined by Bison regardless of whether you
reference it in your grammar.
However, it may invoke one of them for the end token (token 0) if you
redefine it from `$end` to, for example, `END`:

```
%token END 0
```

Finally, Bison will never invoke a `%destructor` for an unreferenced
midrule semantic value (see [Actions in Midrule](#Midrule-Actions)).
That is, Bison does not consider a midrule to have a semantic value if you
do not reference `$$` in the midrule’s action or `$n`
(where n is the right-hand side symbol position of the midrule) in
any later action in that rule. However, if you do reference either, the
Bison-generated parser will invoke the `<>` `%destructor` whenever
it discards the midrule symbol.

  

*Discarded symbols* are the following:

* stacked symbols popped during the first phase of error recovery,
* incoming terminals during the second phase of error recovery,
* the current lookahead and the entire stack (except the current
  right-hand side symbols) when the parser returns immediately, and
* the current lookahead and the entire stack (including the current right-hand
  side symbols) when the C++ parser (`lalr1.cc`) catches an exception in
  `parse`,
* the start symbol, when the parser succeeds.

The parser can *return immediately* because of an explicit call to
`YYABORT`, `YYACCEPT` or `YYNOMEM`, or failed error recovery,
or memory exhaustion.

Right-hand side symbols of a rule that explicitly triggers a syntax
error via `YYERROR` are not discarded automatically. As a rule
of thumb, destructors are invoked only when user actions cannot manage
the memory.

---

Next: [Suppressing Conflict Warnings](#Expect-Decl), Previous: [Freeing Discarded Symbols](#Destructor-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.8 Printing Semantic Values

When run-time traces are enabled (see [Tracing Your Parser](#Tracing)),
the parser reports its actions, such as reductions. When a symbol involved
in an action is reported, only its kind is displayed, as the parser cannot
know how semantic values should be formatted.

The `%printer` directive defines code that is called when a symbol is
reported. Its syntax is the same as `%destructor` (see [Freeing Discarded Symbols](#Destructor-Decl)).

Directive: **%printer** *{ code } symbols* [¶](#index-_0025printer-1)
:   Invoke the braced code whenever the parser displays one of the
    symbols. Within code, `yyo` denotes the output stream (a
    `FILE*` in C, an `std::ostream&` in C++, and `stdout` in D), `$$` (or
    `$<tag>$`) designates the semantic value associated with the
    symbol, and `@$` its location. The additional parser parameters are
    also available (see [The Parser Function `yyparse`](#Parser-Function)).

    The symbols are defined as for `%destructor` (see [Freeing Discarded Symbols](#Destructor-Decl).): they can be per-type (e.g.,
    ‘`<ival>`’), per-symbol (e.g., ‘`exp`’, ‘`NUM`’, ‘`"float"`’),
    typed per-default (i.e., ‘`<*>`’, or untyped per-default (i.e.,
    ‘`<>`’).

For example:

```
%union { char *string; }
%token <string> STRING1 STRING2
%nterm <string> string1 string2
%union { char character; }
%token <character> CHR
%nterm <character> chr
%token TAGLESS

%printer { fprintf (yyo, "'%c'", $$); } <character>
%printer { fprintf (yyo, "&%p", $$); } <*>
%printer { fprintf (yyo, "\"%s\"", $$); } STRING1 string1
%printer { fprintf (yyo, "<>"); } <>
```

guarantees that, when the parser print any symbol that has a semantic type
tag other than `<character>`, it display the address of the semantic
value by default. However, when the parser displays a `STRING1` or a
`string1`, it formats it as a string in double quotes. It performs
only the second `%printer` in this case, so it prints only once.
Finally, the parser print ‘`<>`’ for any symbol, such as `TAGLESS`,
that has no semantic type tag. See [Enabling Debug Traces for `mfcalc`](#Mfcalc-Traces), for a complete example.

---

Next: [The Start-Symbol](#Start-Decl), Previous: [Printing Semantic Values](#Printer-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.9 Suppressing Conflict Warnings

Bison normally warns if there are any conflicts in the grammar
(see [Shift/Reduce Conflicts](#Shift_002fReduce)), but most real grammars
have harmless shift/reduce conflicts which are resolved in a predictable
way and would be difficult to eliminate. It is desirable to suppress
the warning about these conflicts unless the number of conflicts
changes. You can do this with the `%expect` declaration.

The declaration looks like this:

```
%expect n
```

Here n is a decimal integer. The declaration says there should
be n shift/reduce conflicts and no reduce/reduce conflicts.
Bison reports an error if the number of shift/reduce conflicts differs
from n, or if there are any reduce/reduce conflicts.

For deterministic parsers, reduce/reduce conflicts are more
serious, and should be eliminated entirely. Bison will always report
reduce/reduce conflicts for these parsers. With GLR
parsers, however, both kinds of conflicts are routine; otherwise,
there would be no need to use GLR parsing. Therefore, it is
also possible to specify an expected number of reduce/reduce conflicts
in GLR parsers, using the declaration:

```
%expect-rr n
```

You may wish to be more specific in your
specification of expected conflicts. To this end, you can also attach
`%expect` and `%expect-rr` modifiers to individual rules.
The interpretation of these modifiers differs from their use as
declarations. When attached to rules, they indicate the number of states
in which the rule is involved in a conflict. You will need to consult the
output resulting from `-v` to determine appropriate numbers to use.
For example, for the following grammar fragment, the first rule for
`empty_dims` appears in two states in which the ‘`[`’ token is a
lookahead. Having determined that, you can document this fact with an
`%expect` modifier as follows:

```
dims:
  empty_dims
| '[' expr ']' dims
;

empty_dims:
  %empty   %expect 2
| empty_dims '[' ']'
;
```

Mid-rule actions generate implicit rules that are also subject to conflicts
(see [Conflicts due to Midrule Actions](#Midrule-Conflicts)). To attach
an `%expect` or `%expect-rr` annotation to an implicit
mid-rule action’s rule, put it before the action. For example,

```
%glr-parser
%expect-rr 1

%%

clause:
  "condition" %expect-rr 1 { value_mode(); } '(' exprs ')'
| "condition" %expect-rr 1 { class_mode(); } '(' types ')'
;
```

Here, the appropriate mid-rule action will not be determined until after
the ‘`(`’ token is shifted. Thus,
the two actions will clash with each other, and we should expect one
reduce/reduce conflict for each.

In general, using `%expect` involves these steps:

* Compile your grammar without `%expect`. Use the `-v` option
  to get a verbose list of where the conflicts occur. Bison will also
  print the number of conflicts.
* Check each of the conflicts to make sure that Bison’s default
  resolution is what you really want. If not, rewrite the grammar and
  go back to the beginning.
* Add an `%expect` declaration, copying the number n from the
  number that Bison printed. With GLR parsers, add an
  `%expect-rr` declaration as well.
* Optionally, count up the number of states in which one or more
  conflicted reductions for particular rules appear and add these numbers
  to the affected rules as `%expect-rr` or `%expect` modifiers
  as appropriate. Rules that are in conflict appear in the output listing
  surrounded by square brackets or, in the case of reduce/reduce conflicts,
  as reductions having the same lookahead symbol as a square-bracketed
  reduction in the same state.

Now Bison will report an error if you introduce an unexpected conflict,
but will keep silent otherwise.

---

Next: [A Pure (Reentrant) Parser](#Pure-Decl), Previous: [Suppressing Conflict Warnings](#Expect-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.10 The Start-Symbol

Bison assumes by default that the start symbol for the grammar is the first
nonterminal specified in the grammar specification section. The programmer
may override this restriction with the `%start` declaration as follows:

```
%start symbol
```

---

Next: [A Push Parser](#Push-Decl), Previous: [The Start-Symbol](#Start-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.11 A Pure (Reentrant) Parser

A *reentrant* program is one which does not alter in the course of
execution; in other words, it consists entirely of *pure* (read-only)
code. Reentrancy is important whenever asynchronous execution is possible;
for example, a nonreentrant program may not be safe to call from a signal
handler. In systems with multiple threads of control, a nonreentrant
program must be called only within interlocks.

Normally, Bison generates a parser which is not reentrant. This is
suitable for most uses, and it permits compatibility with Yacc. (The
standard Yacc interfaces are inherently nonreentrant, because they use
statically allocated variables for communication with `yylex`,
including `yylval` and `yylloc`.)

Alternatively, you can generate a pure, reentrant parser. The Bison
declaration ‘`%define api.pure`’ says that you want the parser to be
reentrant. It looks like this:

```
%define api.pure full
```

The result is that the communication variables `yylval` and
`yylloc` become local variables in `yyparse`, and a different
calling convention is used for the lexical analyzer function `yylex`.
See [Calling Conventions for Pure Parsers](#Pure-Calling), for the details of this. The variable `yynerrs`
becomes local in `yyparse` in pull mode but it becomes a member of
`yypstate` in push mode. (see [The Error Reporting Function `yyerror`](#Error-Reporting-Function)). The
convention for calling `yyparse` itself is unchanged.

Whether the parser is pure has nothing to do with the grammar rules.
You can generate either a pure parser or a nonreentrant parser from any
valid grammar.

---

Next: [Bison Declaration Summary](#Decl-Summary), Previous: [A Pure (Reentrant) Parser](#Pure-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.12 A Push Parser

A pull parser is called once and it takes control until all its input
is completely parsed. A push parser, on the other hand, is called
each time a new token is made available.

A push parser is typically useful when the parser is part of a
main event loop in the client’s application. This is typically
a requirement of a GUI, when the main event loop needs to be triggered
within a certain time period.

Normally, Bison generates a pull parser.
The following Bison declaration says that you want the parser to be a push
parser (see [%define Summary](#g_t_0025define-Summary)):

```
%define api.push-pull push
```

In almost all cases, you want to ensure that your push parser is also
a pure parser (see [A Pure (Reentrant) Parser](#Pure-Decl)). The only
time you should create an impure push parser is to have backwards
compatibility with the impure Yacc pull mode interface. Unless you know
what you are doing, your declarations should look like this:

```
%define api.pure full
%define api.push-pull push
```

There is a major notable functional difference between the pure push parser
and the impure push parser. It is acceptable for a pure push parser to have
many parser instances, of the same type of parser, in memory at the same time.
An impure push parser should only use one parser at a time.

When a push parser is selected, Bison will generate some new symbols in
the generated parser. `yypstate` is a structure that the generated
parser uses to store the parser’s state. `yypstate_new` is the
function that will create a new parser instance. `yypstate_delete`
will free the resources associated with the corresponding parser instance.
Finally, `yypush_parse` is the function that should be called whenever a
token is available to provide the parser. A trivial example
of using a pure push parser would look like this:

```
int status;
yypstate *ps = yypstate_new ();
do {
  status = yypush_parse (ps, yylex (), NULL);
} while (status == YYPUSH_MORE);
yypstate_delete (ps);
```

If the user decided to use an impure push parser, a few things about the
generated parser will change. The `yychar` variable becomes a global
variable instead of a local one in the `yypush_parse` function. For
this reason, the signature of the `yypush_parse` function is changed to
remove the token as a parameter. A nonreentrant push parser example would
thus look like this:

```
extern int yychar;
int status;
yypstate *ps = yypstate_new ();
do {
  yychar = yylex ();
  status = yypush_parse (ps);
} while (status == YYPUSH_MORE);
yypstate_delete (ps);
```

That’s it. Notice the next token is put into the global variable `yychar`
for use by the next invocation of the `yypush_parse` function.

Bison also supports both the push parser interface along with the pull parser
interface in the same generated parser. In order to get this functionality,
you should replace the ‘`%define api.push-pull push`’ declaration with the
‘`%define api.push-pull both`’ declaration. Doing this will create all of
the symbols mentioned earlier along with the two extra symbols, `yyparse`
and `yypull_parse`. `yyparse` can be used exactly as it normally
would be used. However, the user should note that it is implemented in the
generated parser by calling `yypull_parse`.
This makes the `yyparse` function that is generated with the
‘`%define api.push-pull both`’ declaration slower than the normal
`yyparse` function. If the user
calls the `yypull_parse` function it will parse the rest of the input
stream. It is possible to `yypush_parse` tokens to select a subgrammar
and then `yypull_parse` the rest of the input stream. If you would like
to switch back and forth between between parsing styles, you would have to
write your own `yypull_parse` function that knows when to quit looking
for input. An example of using the `yypull_parse` function would look
like this:

```
yypstate *ps = yypstate_new ();
yypull_parse (ps); /* Will call the lexer */
yypstate_delete (ps);
```

Adding the ‘`%define api.pure`’ declaration does exactly the same thing to
the generated parser with ‘`%define api.push-pull both`’ as it did for
‘`%define api.push-pull push`’.

---

Next: [%define Summary](#g_t_0025define-Summary), Previous: [A Push Parser](#Push-Decl), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.13 Bison Declaration Summary

Here is a summary of the declarations used to define a grammar:

Directive: **%union** [¶](#index-_0025union-2)
:   Declare the collection of data types that semantic values may have
    (see [The Union Declaration](#Union-Decl)).

Directive: **%token** [¶](#index-_0025token-2)
:   Declare a terminal symbol (token kind name) with no precedence
    or associativity specified (see [Token Kind Names](#Token-Decl)).

Directive: **%right** [¶](#index-_0025right)
:   Declare a terminal symbol (token kind name) that is right-associative
    (see [Operator Precedence](#Precedence-Decl)).

Directive: **%left** [¶](#index-_0025left-1)
:   Declare a terminal symbol (token kind name) that is left-associative
    (see [Operator Precedence](#Precedence-Decl)).

Directive: **%nonassoc** [¶](#index-_0025nonassoc)
:   Declare a terminal symbol (token kind name) that is nonassociative
    (see [Operator Precedence](#Precedence-Decl)).
    Using it in a way that would be associative is a syntax error.

Directive: **%nterm** [¶](#index-_0025nterm-2)
:   Declare the type of semantic values for a nonterminal symbol (see [Nonterminal Symbols](#Type-Decl)).

Directive: **%type** [¶](#index-_0025type-2)
:   Declare the type of semantic values for a symbol (see [Nonterminal Symbols](#Type-Decl)).

Directive: **%start** [¶](#index-_0025start-1)
:   Specify the grammar’s start symbol (see [The Start-Symbol](#Start-Decl)).

Directive: **%expect** [¶](#index-_0025expect-1)
:   Declare the expected number of shift/reduce conflicts, either overall or
    for a given rule
    (see [Suppressing Conflict Warnings](#Expect-Decl)).

Directive: **%expect-rr** [¶](#index-_0025expect_002drr-2)
:   Declare the expected number of reduce/reduce conflicts, either overall or
    for a given rule
    (see [Suppressing Conflict Warnings](#Expect-Decl)).

  

In order to change the behavior of `bison`, use the following
directives:

Directive: **%code** *{code}* [¶](#index-_0025code-1)

Directive: **%code** *qualifier {code}* [¶](#index-_0025code-2)
:   Insert code verbatim into the output parser source at the
    default location or at the location specified by qualifier.
    See [%code Summary](#g_t_0025code-Summary).

Directive: **%debug** [¶](#index-_0025debug)
:   Instrument the parser for traces. Obsoleted by ‘`%define
    parse.trace`’.
    See [Tracing Your Parser](#Tracing).

Directive: **%define** *variable* [¶](#index-_0025define)

Directive: **%define** *variable value* [¶](#index-_0025define-1)

Directive: **%define** *variable {value}* [¶](#index-_0025define-2)

Directive: **%define** *variable "value"* [¶](#index-_0025define-3)
:   Define a variable to adjust Bison’s behavior. See [%define Summary](#g_t_0025define-Summary).

Directive: **%defines** [¶](#index-_0025defines)

Directive: **%defines** *defines-file* [¶](#index-_0025defines-1)
:   Historical name for `%header`. See [`%header`](#g_t_0025header).

Directive: **%destructor** [¶](#index-_0025destructor-4)
:   Specify how the parser should reclaim the memory associated to
    discarded symbols. See [Freeing Discarded Symbols](#Destructor-Decl).

Directive: **%file-prefix** *"prefix"* [¶](#index-_0025file_002dprefix)
:   Specify a prefix to use for all Bison output file names. The names
    are chosen as if the grammar file were named `prefix.y`.

Directive: **%header** [¶](#index-_0025header)
:   Write a parser header file containing definitions for the token kind names
    defined in the grammar as well as a few other declarations. If the parser
    implementation file is named `name.c` then the parser header file
    is named `name.h`.

    For C parsers, the parser header file declares `YYSTYPE` unless
    `YYSTYPE` is already defined as a macro or you have used a
    `<type>` tag without using `%union`. Therefore, if you are
    using a `%union` (see [More Than One Value Type](#Multiple-Types)) with components that require
    other definitions, or if you have defined a `YYSTYPE` macro or type
    definition (see [Data Types of Semantic Values](#Value-Type)), you need to arrange for these definitions
    to be propagated to all modules, e.g., by putting them in a prerequisite
    header that is included both by your parser and by any other module that
    needs `YYSTYPE`.

    Unless your parser is pure, the parser header file declares
    `yylval` as an external variable. See [A Pure (Reentrant) Parser](#Pure-Decl).

    If you have also used locations, the parser header file declares
    `YYLTYPE` and `yylloc` using a protocol similar to that of the
    `YYSTYPE` macro and `yylval`. See [Tracking Locations](#Tracking-Locations).

    This parser header file is normally essential if you wish to put the
    definition of `yylex` in a separate source file, because
    `yylex` typically needs to be able to refer to the
    above-mentioned declarations and to the token kind codes. See [Semantic Values of Tokens](#Token-Values).

    If you have declared `%code requires` or `%code provides`, the output
    header also contains their code.
    See [%code Summary](#g_t_0025code-Summary).

    The generated header is protected against multiple inclusions with a C
    preprocessor guard: ‘`YY_PREFIX_FILE_INCLUDED`’, where
    PREFIX and FILE are the prefix (see [Multiple Parsers in the Same Program](#Multiple-Parsers)) and
    generated file name turned uppercase, with each series of non alphanumerical
    characters converted to a single underscore.

    For instance with ‘`%define api.prefix {calc}`’ and ‘`%header
    "lib/parse.h"`’, the header will be guarded as follows.

    ```
    #ifndef YY_CALC_LIB_PARSE_H_INCLUDED
    # define YY_CALC_LIB_PARSE_H_INCLUDED
    ...
    #endif /* ! YY_CALC_LIB_PARSE_H_INCLUDED */
    ```

    Introduced in Bison 3.8.

Directive: **%header** *header-file* [¶](#index-_0025header-1)
:   Same as above, but save in the file `header-file`.

Directive: **%language** *"language"* [¶](#index-_0025language)
:   Specify the programming language for the generated parser. Currently
    supported languages include C, C++, D and Java. language is
    case-insensitive.

Directive: **%locations** [¶](#index-_0025locations)
:   Generate the code processing the locations (see [Special Features for Use in Actions](#Action-Features)). This
    mode is enabled as soon as the grammar uses the special ‘`@n`’
    tokens, but if your grammar does not use it, using ‘`%locations`’ allows
    for more accurate syntax error messages.

Directive: **%name-prefix** *"prefix"* [¶](#index-_0025name_002dprefix)
:   Obsoleted by ‘`%define api.prefix {prefix}`’. See [Multiple Parsers in the Same Program](#Multiple-Parsers). For C++ parsers, see the
    ‘`%define api.namespace`’ documentation in this section.

    Rename the external symbols used in the parser so that they start with
    prefix instead of ‘`yy`’. The precise list of symbols renamed in C
    parsers is `yyparse`, `yylex`, `yyerror`, `yynerrs`,
    `yylval`, `yychar`, `yydebug`, and (if locations are used)
    `yylloc`. If you use a push parser, `yypush_parse`,
    `yypull_parse`, `yypstate`, `yypstate_new` and
    `yypstate_delete` will also be renamed. For example, if you use
    ‘`%name-prefix "c_"`’, the names become `c_parse`, `c_lex`, and
    so on.

    Contrary to defining `api.prefix`, some symbols are *not* renamed
    by `%name-prefix`, for instance `YYDEBUG`, `YYTOKENTYPE`,
    `yytoken_kind_t`, `YYSTYPE`, `YYLTYPE`.

Directive: **%no-lines** [¶](#index-_0025no_002dlines)
:   Don’t generate any `#line` preprocessor commands in the parser
    implementation file. Ordinarily Bison writes these commands in the parser
    implementation file so that the C compiler and debuggers will associate
    errors and object code with your source file (the grammar file). This
    directive causes them to associate errors with the parser implementation
    file, treating it as an independent source file in its own right.

Directive: **%output** *"file"* [¶](#index-_0025output)
:   Generate the parser implementation in `file`.

Directive: **%pure-parser** [¶](#index-_0025pure_002dparser)
:   Deprecated version of ‘`%define api.pure`’ (see [%define Summary](#g_t_0025define-Summary)), for which Bison is more careful to warn about
    unreasonable usage.

Directive: **%require** *"version"* [¶](#index-_0025require-1)
:   Require version version or higher of Bison. See [Require a Version of Bison](#Require-Decl).

Directive: **%skeleton** *"file"* [¶](#index-_0025skeleton)
:   Specify the skeleton to use.

    If file does not contain a `/`, file is the name of a skeleton
    file in the Bison installation directory.
    If it does, file is an absolute file name or a file name relative to the
    directory of the grammar file.
    This is similar to how most shells resolve commands.

Directive: **%token-table** [¶](#index-_0025token_002dtable)
:   This feature is obsolescent, avoid it in new projects.

    Generate an array of token names in the parser implementation file. The
    name of the array is `yytname`; `yytname[i]` is the name of
    the token whose internal Bison token code is i. The first three
    elements of `yytname` correspond to the predefined tokens
    `"$end"`, `"error"`, and `"$undefined"`; after these come the
    symbols defined in the grammar file.

    The name in the table includes all the characters needed to represent the
    token in Bison. For single-character literals and literal strings, this
    includes the surrounding quoting characters and any escape sequences. For
    example, the Bison single-character literal `'+'` corresponds to a
    three-character name, represented in C as `"'+'"`; and the Bison
    two-character literal string `"\\/"` corresponds to a five-character
    name, represented in C as `"\"\\\\/\""`.

    When you specify `%token-table`, Bison also generates macro definitions
    for macros `YYNTOKENS`, `YYNNTS`, and `YYNRULES`, and
    `YYNSTATES`:

    `YYNTOKENS`
    :   The number of terminal symbols, i.e., the highest token code, plus one.

    `YYNNTS`
    :   The number of nonterminal symbols.

    `YYNRULES`
    :   The number of grammar rules,

    `YYNSTATES`
    :   The number of parser states (see [Parser States](#Parser-States)).

    Here’s code for looking up a multicharacter token in `yytname`,
    assuming that the characters of the token are stored in `token_buffer`,
    and assuming that the token does not contain any characters like ‘`"`’
    that require escaping.

    ```
    for (int i = 0; i < YYNTOKENS; i++)
      if (yytname[i]
          && yytname[i][0] == '"'
          && ! strncmp (yytname[i] + 1, token_buffer,
                        strlen (token_buffer))
          && yytname[i][strlen (token_buffer) + 1] == '"'
          && yytname[i][strlen (token_buffer) + 2] == 0)
        break;
    ```

    This method is discouraged: the primary purpose of string aliases is forging
    good error messages, not describing the spelling of keywords. In addition,
    looking for the token kind at runtime incurs a (small but noticeable) cost.

    Finally, `%token-table` is incompatible with the `custom` and
    `detailed` values of the `parse.error` `%define` variable.

Directive: **%verbose** [¶](#index-_0025verbose)
:   Write an extra output file containing verbose descriptions of the parser
    states and what is done for each type of lookahead token in that state.
    See [Understanding Your Parser](#Understanding), for more information.

Directive: **%yacc** [¶](#index-_0025yacc)
:   Pretend the option `--yacc` was given
    (see [`--yacc`](#option_002dyacc)), i.e., imitate Yacc, including its
    naming conventions. Only makes sense with the `yacc.c`
    skeleton. See [Tuning the Parser](#Tuning-the-Parser), for more.

    Of course, being a Bison extension, `%yacc` is somewhat
    self-contradictory…

---

Next: [%code Summary](#g_t_0025code-Summary), Previous: [Bison Declaration Summary](#Decl-Summary), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.14 %define Summary

There are many features of Bison’s behavior that can be controlled by
assigning the feature a single value. For historical reasons, some such
features are assigned values by dedicated directives, such as `%start`,
which assigns the start symbol. However, newer such features are associated
with variables, which are assigned by the `%define` directive:

Directive: **%define** *variable* [¶](#index-_0025define-4)

Directive: **%define** *variable value* [¶](#index-_0025define-5)

Directive: **%define** *variable {value}* [¶](#index-_0025define-6)

Directive: **%define** *variable "value"* [¶](#index-_0025define-7)
:   Define variable to value.

    The type of the values depend on the syntax. Braces denote value in the
    target language (e.g., a namespace, a type, etc.). Keyword values (no
    delimiters) denote finite choice (e.g., a variation of a feature). String
    values denote remaining cases (e.g., a file name).

    It is an error if a variable is defined by `%define` multiple
    times, but see [`-D name[=value]`](#Tuning-the-Parser).

The rest of this section summarizes variables and values that `%define`
accepts.

Some variables take Boolean values. In this case, Bison will complain
if the variable definition does not meet one of the following four
conditions:

1. `value` is `true`
2. `value` is omitted (or `""` is specified).
   This is equivalent to `true`.
3. `value` is `false`.
4. variable is never defined.
   In this case, Bison selects a default value.

What variables are accepted, as well as their meanings and default
values, depend on the selected target language and/or the parser skeleton
(see [Bison Declaration Summary](#Decl-Summary), see [Bison Declaration Summary](#Decl-Summary)).
Unaccepted variables produce an error. Some of the accepted
variables are described below.

Directive: **%define api.filename.type** *{type}* [¶](#index-_0025define-api_002efilename_002etype)
:   * Language(s): C++
    * Purpose:
      Define the type of file names in Bison’s default location and position
      types. See [Exposing the Location Classes](#Exposing-the-Location-Classes).
    * Accepted Values:
      Any type that is printable (via streams) and comparable (with `==` and
      `!=`).
    * Default Value: `const std::string`.
    * History:
      Introduced in Bison 2.0 as `filename_type` (with `std::string` as
      default), renamed as `api.filename.type` in Bison 3.7 (with `const
      std::string` as default).

Directive: **%define api.header.include** *{"header.h"}* [¶](#index-_0025define-api_002eheader_002einclude)

Directive: **%define api.header.include** *{<header.h>}* [¶](#index-_0025define-api_002eheader_002einclude-1)
:   * Languages(s): C (`yacc.c`)
    * Purpose: Specify how the generated parser should include the generated header.

      Historically, when option `-d` or `--header` was used,
      `bison` generated a header and pasted an exact copy of it into the
      generated parser implementation file. Since Bison 3.6, it is
      `#include`d as ‘`"basename.h"`’, instead of duplicated, unless
      file is ‘`y.tab`’, see below.

      The `api.header.include` variable allows to control how the generated
      parser `#include`s the generated header. For instance:

      ```
      %define api.header.include {"parse.h"}
      ```

      or

      ```
      %define api.header.include {<parser/parse.h>}
      ```

      Using `api.header.include` does not change the name of the generated
      header, only how it is included.

      To work around limitations of Automake’s `ylwrap` (which runs
      `bison` with `--yacc`), `api.header.include` is
      *not* predefined when the output file is `y.tab.c`. Define it to
      avoid the duplication.
    * Accepted Values:
      An argument for `#include`.
    * Default Value:
      ‘`"header-basename"`’, unless the header file is `y.tab.h`,
      where header-basename is the name of the generated header, without
      directory part. For instance with ‘`bison -d calc/parse.y`’,
      `api.header.include` defaults to ‘`"parse.h"`’, not
      ‘`"calc/parse.h"`’.
    * History:
      Introduced in Bison 3.4. Defaults to ‘`"basename.h"`’ since Bison
      3.7, unless the header file is `y.tab.h`.

Directive: **%define api.location.file** *"file"* [¶](#index-_0025define-api_002elocation_002efile)

Directive: **%define api.location.file** *`none`* [¶](#index-_0025define-api_002elocation_002efile-1)
:   * Language(s): C++
    * Purpose:
      Define the name of the file in which Bison’s default location and position
      types are generated. See [Exposing the Location Classes](#Exposing-the-Location-Classes).
    * Accepted Values:

      `none`
      :   If locations are enabled, generate the definition of the `position` and
          `location` classes in the header file if `%header`, otherwise in
          the parser implementation.

      "file"
      :   Generate the definition of the `position` and `location` classes
          in file. This file name can be relative (to where the parser file is
          output) or absolute.
    * Default Value:
      Not applicable if locations are not enabled, or if a user location type is
      specified (see `api.location.type`). Otherwise, Bison’s
      `location` is generated in `location.hh` (see [C++ `location`](#C_002b_002b-location)).
    * History:
      Introduced in Bison 3.2.

Directive: **%define api.location.include** *{"file"}* [¶](#index-_0025define-api_002elocation_002einclude)

Directive: **%define api.location.include** *{<file>}* [¶](#index-_0025define-api_002elocation_002einclude-1)
:   * Language(s): C++
    * Purpose:
      Specify how the generated file that defines the `position` and
      `location` classes is included. This makes sense when the
      `location` class is exposed to the rest of your application/library in
      another directory. See [Exposing the Location Classes](#Exposing-the-Location-Classes).
    * Accepted Values: Argument for `#include`.
    * Default Value:
      ‘`"dir/location.hh"`’ where dir is the directory part of the
      output. For instance `src/parse` if
      `--output=src/parse/parser.cc` was given.
    * History:
      Introduced in Bison 3.2.

Directive: **%define api.location.type** *{type}* [¶](#index-_0025define-api_002elocation_002etype)
:   * Language(s): C, C++, Java
    * Purpose: Define the location type.
      See [Data Type of Locations](#Location-Type), and [User Defined Location Type](#User-Defined-Location-Type).
    * Accepted Values: String
    * Default Value: none
    * History:
      Introduced in Bison 2.7 for C++ and Java, in Bison 3.4 for C. Was
      originally named `location_type` in Bison 2.5 and 2.6.

Directive: **%define api.namespace** *{namespace}* [¶](#index-_0025define-api_002enamespace)
:   * Languages(s): C++
    * Purpose: Specify the namespace for the parser class.
      For example, if you specify:

      ```
      %define api.namespace {foo::bar}
      ```

      Bison uses `foo::bar` verbatim in references such as:

      ```
      foo::bar::parser::value_type
      ```

      However, to open a namespace, Bison removes any leading `::` and then
      splits on any remaining occurrences:

      ```
      namespace foo { namespace bar {
        class position;
        class location;
      } }
      ```
    * Accepted Values:
      Any absolute or relative C++ namespace reference without a trailing
      `"::"`. For example, `"foo"` or `"::foo::bar"`.
    * Default Value:
      `yy`, unless you used the obsolete ‘`%name-prefix "prefix"`’
      directive.

Directive: **%define api.parser.class** *{name}* [¶](#index-_0025define-api_002eparser_002eclass)
:   * Language(s):
      C++, Java, D
    * Purpose:
      The name of the parser class.
    * Accepted Values:
      Any valid identifier.
    * Default Value:
      In C++, `parser`. In D and Java, `YYParser` or
      `api.prefixParser` (see [Java Bison Interface](#Java-Bison-Interface)).
    * History:
      Introduced in Bison 3.3 to replace `parser_class_name`.

Directive: **%define api.prefix** *{prefix}* [¶](#index-_0025define-api_002eprefix)
:   * Language(s): C, C++, Java
    * Purpose: Rename exported symbols.
      See [Multiple Parsers in the Same Program](#Multiple-Parsers).
    * Accepted Values: String
    * Default Value: `YY` for Java, `yy` otherwise.
    * History:
      introduced in Bison 2.6, with its argument in double quotes. Uses braces
      since Bison 3.0 (double quotes are still supported for backward
      compatibility).

Directive: **%define api.pure** *purity* [¶](#index-_0025define-api_002epure-1)
:   * Language(s): C
    * Purpose: Request a pure (reentrant) parser program.
      See [A Pure (Reentrant) Parser](#Pure-Decl).
    * Accepted Values: `true`, `false`, `full`

      The value may be omitted: this is equivalent to specifying `true`, as is
      the case for Boolean values.

      When `%define api.pure full` is used, the parser is made reentrant. This
      changes the signature for `yylex` (see [Calling Conventions for Pure Parsers](#Pure-Calling)), and also that of
      `yyerror` when the tracking of locations has been activated, as shown
      below.

      The `true` value is very similar to the `full` value, the only
      difference is in the signature of `yyerror` on Yacc parsers without
      `%parse-param`, for historical reasons.

      I.e., if ‘`%locations %define api.pure`’ is passed then the prototypes for
      `yyerror` are:

      ```
      void yyerror (char const *msg);                 // Yacc parsers.
      void yyerror (YYLTYPE *locp, char const *msg);  // GLR parsers.
      ```

      But if ‘`%locations %define api.pure %parse-param {int *nastiness}`’ is
      used, then both parsers have the same signature:

      ```
      void yyerror (YYLTYPE *llocp, int *nastiness, char const *msg);
      ```

      (see [The Error Reporting Function `yyerror`](#Error-Reporting-Function))
    * Default Value: `false`
    * History:
      the `full` value was introduced in Bison 2.7

Directive: **%define api.push-pull** *kind* [¶](#index-_0025define-api_002epush_002dpull-1)
:   * Language(s): C (deterministic parsers only), D, Java
    * Purpose: Request a pull parser, a push parser, or both.
      See [A Push Parser](#Push-Decl).
    * Accepted Values: `pull`, `push`, `both`
    * Default Value: `pull`

Directive: **%define api.symbol.prefix** *{prefix}* [¶](#index-_0025define-api_002esymbol_002eprefix)
:   * Languages(s): all
    * Purpose:
      Add a prefix to the name of the symbol kinds. For instance

      ```
      %define api.symbol.prefix {S_}
      %token FILE for ERROR
      %%
      start: FILE for ERROR;
      ```

      generates this definition in C:

      ```
      /* Symbol kind.  */
      enum yysymbol_kind_t
      {
        S_YYEMPTY = -2,   /* No symbol.  */
        S_YYEOF = 0,      /* $end  */
        S_YYERROR = 1,    /* error  */
        S_YYUNDEF = 2,    /* $undefined  */
        S_FILE = 3,       /* FILE  */
        S_for = 4,        /* for  */
        S_ERROR = 5,      /* ERROR  */
        S_YYACCEPT = 6,   /* $accept  */
        S_start = 7       /* start  */
      };
      ```
    * Accepted Values:
      Any non empty string. Must be a valid identifier in the target language
      (typically a non empty sequence of letters, underscores, and —not at the
      beginning— digits).

      The empty prefix is (generally) invalid:

      + in C it would create collision with the `YYERROR` macro, and
        potentially token kind definitions and symbol kind definitions would
        collide;
      + unnamed symbols (such as ‘`'+'`’) have a name which starts with a digit;
      + even in languages with scoped enumerations such as Java, an empty prefix is
        dangerous: symbol names may collide with the target language keywords, or
        with other members of the `SymbolKind` class.
    * Default Value:
      `YYSYMBOL_` in C, `S_` in C++ and Java, empty in D.
    * History:
      introduced in Bison 3.6.

Directive: **%define api.token.constructor** [¶](#index-_0025define-api_002etoken_002econstructor)
:   * Language(s):
      C++, D
    * Purpose:
      Request that symbols be handled as a whole (type, value, and possibly
      location) in the scanner. In the case of C++, it works only when
      variant-based semantic values are enabled (see [C++ Variants](#C_002b_002b-Variants)), see
      [Complete Symbols](#Complete-Symbols), for details. In D, token constructors work with both
      ‘`%union`’ and ‘`%define api.value.type union`’.
    * Accepted Values:
      Boolean.
    * Default Value:
      `false`
    * History:
      introduced in Bison 3.0.

Directive: **%define api.token.prefix** *{prefix}* [¶](#index-_0025define-api_002etoken_002eprefix)
:   * Languages(s): all
    * Purpose:
      Add a prefix to the token names when generating their definition in the
      target language. For instance

      ```
      %define api.token.prefix {TOK_}
      %token FILE for ERROR
      %%
      start: FILE for ERROR;
      ```

      generates the definition of the symbols `TOK_FILE`, `TOK_for`, and
      `TOK_ERROR` in the generated source files. In particular, the scanner
      must use these prefixed token names, while the grammar itself may still use
      the short names (as in the sample rule given above). The generated
      informational files (`*.output`, `*.xml`, `*.gv`) are not
      modified by this prefix.

      Bison also prefixes the generated member names of the semantic value union.
      See [Generating the Semantic Value Type](#Type-Generation), for more
      details.

      See [Calc++ Parser](#Calc_002b_002b-Parser) and [Calc++ Scanner](#Calc_002b_002b-Scanner), for a complete example.
    * Accepted Values:
      Any string. Must be a valid identifier prefix in the target language
      (typically, a possibly empty sequence of letters, underscores, and —not at
      the beginning— digits).
    * Default Value:
      empty
    * History:
      introduced in Bison 3.0.

Directive: **%define api.token.raw** [¶](#index-_0025define-api_002etoken_002eraw)
:   * Language(s):
      all
    * Purpose:
      The output files normally define the enumeration of the *token kinds*
      with Yacc-compatible token codes: sequential numbers starting at 257 except
      for single character tokens which stand for themselves (e.g., in ASCII,
      ‘`'a'`’ is numbered 65). The parser however uses *symbol kinds*
      which are assigned numbers sequentially starting at 0. Therefore each time
      the scanner returns an (external) token kind, it must be mapped to the
      (internal) symbol kind.

      When `api.token.raw` is set, the code of the token kinds are forced to
      coincide with the symbol kind. This saves one table lookup per token to map
      them from the token kind to the symbol kind, and also saves the generation
      of the mapping table. The gain is typically moderate, but in extreme cases
      (very simple user actions), a 10% improvement can be observed.

      When `api.token.raw` is set, the grammar cannot use character literals
      (such as ‘`'a'`’).
    * Accepted Values: Boolean.
    * Default Value:
      `true` in D, `false` otherwise
    * History:
      introduced in Bison 3.5. Was initially introduced in Bison 1.25 as
      ‘`%raw`’, but never worked and was removed in Bison 1.29.

Directive: **%define api.value.automove** [¶](#index-_0025define-api_002evalue_002eautomove)
:   * Language(s):
      C++
    * Purpose:
      Let occurrences of semantic values of the right-hand sides of a rule be
      implicitly turned in rvalues. When enabled, a grammar such as:

      ```
      exp:
        "number"     { $$ = make_number ($1); }
      | exp "+" exp  { $$ = make_binary (add, $1, $3); }
      | "(" exp ")"  { $$ = $2; }
      ```

      is actually compiled as if you had written:

      ```
      exp:
        "number"     { $$ = make_number (std::move ($1)); }
      | exp "+" exp  { $$ = make_binary (add,
                                         std::move ($1),
                                         std::move ($3)); }
      | "(" exp ")"  { $$ = std::move ($2); }
      ```

      Using a value several times with automove enabled is typically an error.
      For instance, instead of:

      ```
      exp: "twice" exp  { $$ = make_binary (add, $2, $2); }
      ```

      write:

      ```
      exp: "twice" exp { auto v = $2; $$ = make_binary (add, v, v); }
      ```

      It is tempting to use `std::move` on one of the `v`, but the
      argument evaluation order in C++ is unspecified.
    * Accepted Values:
      Boolean.
    * Default Value:
      `false`
    * History:
      introduced in Bison 3.2

Directive: **%define api.value.type** *support* [¶](#index-_0025define-api_002evalue_002etype)

Directive: **%define api.value.type** *{type}* [¶](#index-_0025define-api_002evalue_002etype-1)
:   * Language(s):
      all
    * Purpose:
      The type for semantic values.
    * Accepted Values:

      ‘`{}`’
      :   This grammar has no semantic value at all. This is not properly supported
          yet.

      ‘`union-directive`’ (C, C++, D)
      :   The type is defined thanks to the `%union` directive. You don’t have
          to define `api.value.type` in that case, using `%union` suffices.
          See [The Union Declaration](#Union-Decl).
          For instance:

          ```
          %define api.value.type union-directive
          %union
          {
            int ival;
            char *sval;
          }
          %token <ival> INT "integer"
          %token <sval> STR "string"
          ```

      ‘`union`’ (C, C++)
      :   The symbols are defined with type names, from which Bison will generate a
          `union`. For instance:

          ```
          %define api.value.type union
          %token <int> INT "integer"
          %token <char *> STR "string"
          ```

          Most C++ objects cannot be stored in a `union`, use ‘`variant`’
          instead.

      ‘`variant`’ (C++)
      :   This is similar to `union`, but special storage techniques are used to
          allow any kind of C++ object to be used. For instance:

          ```
          %define api.value.type variant
          %token <int> INT "integer"
          %token <std::string> STR "string"
          ```

          See [C++ Variants](#C_002b_002b-Variants).

      ‘`{type}`’
      :   Use this type as semantic value.

          ```
          %code requires
          {
            struct my_value
            {
              enum
              {
                is_int, is_str
              } kind;
              union
              {
                int ival;
                char *sval;
              } u;
            };
          }
          %define api.value.type {struct my_value}
          %token <u.ival> INT "integer"
          %token <u.sval> STR "string"
          ```
    * Default Value:
      + - `union-directive` if `%union` is used, otherwise …
      + - `int` if type tags are used (i.e., ‘`%token <type>…`’ or
        ‘`%nterm <type>…`’ is used), otherwise …
      + - undefined.
    * History:
      introduced in Bison 3.0. Was introduced for Java only in 2.3b as
      `stype`.

Directive: **%define api.value.union.name** *name* [¶](#index-_0025define-api_002evalue_002eunion_002ename)
:   * Language(s):
      C
    * Purpose:
      The tag of the generated `union` (*not* the name of the
      `typedef`). This variable is set to `id` when ‘`%union
      id`’ is used. There is no clear reason to give this union a name.
    * Accepted Values:
      Any valid identifier.
    * Default Value:
      `YYSTYPE`.
    * History:
      Introduced in Bison 3.0.3.

Directive: **%define lr.default-reduction** *when* [¶](#index-_0025define-lr_002edefault_002dreduction)
:   * Language(s): all
    * Purpose: Specify the kind of states that are permitted to
      contain default reductions. See [Default Reductions](#Default-Reductions).
    * Accepted Values: `most`, `consistent`, `accepting`
    * Default Value:
      + `accepting` if `lr.type` is `canonical-lr`.
      + `most` otherwise.
    * History:
      introduced as `lr.default-reductions` in 2.5, renamed as
      `lr.default-reduction` in 3.0.

Directive: **%define lr.keep-unreachable-state** [¶](#index-_0025define-lr_002ekeep_002dunreachable_002dstate)
:   * Language(s): all
    * Purpose: Request that Bison allow unreachable parser states to
      remain in the parser tables. See [Unreachable States](#Unreachable-States).
    * Accepted Values: Boolean
    * Default Value: `false`
    * History:
      introduced as `lr.keep_unreachable_states` in 2.3b, renamed as
      `lr.keep-unreachable-states` in 2.5, and as
      `lr.keep-unreachable-state` in 3.0.

Directive: **%define lr.type** *type* [¶](#index-_0025define-lr_002etype)
:   * Language(s): all
    * Purpose: Specify the type of parser tables within the
      LR(1) family. See [LR Table Construction](#LR-Table-Construction).
    * Accepted Values: `lalr`, `ielr`, `canonical-lr`
    * Default Value: `lalr`

Directive: **%define** *namespace {namespace}* [¶](#index-_0025define-8)
:   Obsoleted by `api.namespace`

Directive: **%define parse.assert** [¶](#index-_0025define-parse_002eassert)
:   * Languages(s): C, C++
    * Purpose: Issue runtime assertions to catch invalid uses.
      In C, some important invariants in the implementation of the parser are
      checked when this option is enabled.

      In C++, when variants are used (see [C++ Variants](#C_002b_002b-Variants)), symbols must be
      constructed and destroyed properly. This option checks these constraints
      using runtime type information (RTTI). Therefore the generated code cannot
      be compiled with RTTI disabled (via compiler options such as
      `-fno-rtti`).
    * Accepted Values: Boolean
    * Default Value: `false`

Directive: **%define parse.error** *verbosity* [¶](#index-_0025define-parse_002eerror)
:   * Languages(s):
      all
    * Purpose:
      Control the generation of syntax error messages. See [Error Reporting](#Error-Reporting).
    * Accepted Values:
      + `simple`
        Error messages passed to `yyerror` are simply `"syntax error"`.
      + `detailed`
        Error messages report the unexpected token, and possibly the expected ones.
        However, this report can often be incorrect when LAC is not enabled
        (see [LAC](#LAC)). Token name internationalization is supported.
      + `verbose`
        Similar (but inferior) to `detailed`. The D parser does not support this value.

        Error messages report the unexpected token, and possibly the expected ones.
        However, this report can often be incorrect when LAC is not enabled
        (see [LAC](#LAC)).

        Does not support token internationalization. Using non-ASCII characters in
        token aliases is not portable.
      + `custom`
        The user is in charge of generating the syntax error message by defining the
        `yyreport_syntax_error` function. See [The Syntax Error Reporting Function `yyreport_syntax_error`](#Syntax-Error-Reporting-Function).
    * Default Value:
      `simple`
    * History:
      introduced in 3.0 with support for `simple` and `verbose`. Values
      `custom` and `detailed` were introduced in 3.6.

Directive: **%define parse.lac** *when* [¶](#index-_0025define-parse_002elac)
:   * Languages(s): C/C++ (deterministic parsers only), D and Java.
    * Purpose: Enable LAC (lookahead correction) to improve
      syntax error handling. See [LAC](#LAC).
    * Accepted Values: `none`, `full`
    * Default Value: `none`

Directive: **%define parse.trace** [¶](#index-_0025define-parse_002etrace)
:   * Languages(s): C, C++, D, Java
    * Purpose: Require parser instrumentation for tracing.
      See [Tracing Your Parser](#Tracing).

      In C/C++, define the macro `YYDEBUG` (or `prefixDEBUG` with
      ‘`%define api.prefix {prefix}`’), see [Multiple Parsers in the Same Program](#Multiple-Parsers)) to
      1 (if it is not already defined) so that the debugging facilities are
      compiled.
    * Accepted Values: Boolean
    * Default Value: `false`

Directive: **%define** *parser\_class\_name {name}* [¶](#index-_0025define-9)
:   Obsoleted by `api.parser.class`

---

Previous: [%define Summary](#g_t_0025define-Summary), Up: [Bison Declarations](#Declarations)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 3.7.15 %code Summary

The `%code` directive inserts code verbatim into the output
parser source at any of a predefined set of locations. It thus serves
as a flexible and user-friendly alternative to the traditional Yacc
prologue, `%{code%}`. This section summarizes the
functionality of `%code` for the various target languages
supported by Bison. For a detailed discussion of how to use
`%code` in place of `%{code%}` for C/C++ and why it
is advantageous to do so, see [Prologue Alternatives](#Prologue-Alternatives).

Directive: **%code** *{code}* [¶](#index-_0025code-5)
:   This is the unqualified form of the `%code` directive. It
    inserts code verbatim at a language-dependent default location
    in the parser implementation.

    For C/C++, the default location is the parser implementation file
    after the usual contents of the parser header file. Thus, the
    unqualified form replaces `%{code%}` for most purposes.

    For D and Java, the default location is inside the parser class.

Directive: **%code** *qualifier {code}* [¶](#index-_0025code-6)
:   This is the qualified form of the `%code` directive.
    qualifier identifies the purpose of code and thus the
    location(s) where Bison should insert it. That is, if you need to
    specify location-sensitive code that does not belong at the
    default location selected by the unqualified `%code` form, use
    this form instead.

For any particular qualifier or for the unqualified form, if there are
multiple occurrences of the `%code` directive, Bison concatenates
the specified code in the order in which it appears in the grammar
file.

Not all qualifiers are accepted for all target languages. Unaccepted
qualifiers produce an error. Some of the accepted qualifiers are:

`requires` [¶](#index-_0025code-requires-2)
:   * Language(s): C, C++
    * Purpose:
      This is the best place to write dependency code required for the value and
      location types (`YYSTYPE` and `YYLTYPE` in C). In other words,
      it’s the best place to define types referenced in `%union` directives.
      In C, if you use `#define` to override Bison’s default `YYSTYPE`
      and `YYLTYPE` definitions, then it is also the best place. However you
      should rather `%define` `api.value.type` and
      `api.location.type`.
    * Location(s):
      The parser header file and the parser implementation file before the
      Bison-generated definitions of the value and location types (`YYSTYPE`
      and `YYLTYPE` in C).

`provides` [¶](#index-_0025code-provides-2)
:   * Language(s): C, C++
    * Purpose: This is the best place to write additional definitions and
      declarations that should be provided to other modules.
    * Location(s):
      The parser header file and the parser implementation file after the
      Bison-generated value and location types (`YYSTYPE` and `YYLTYPE`
      in C), and token definitions.

`top` [¶](#index-_0025code-top-1)
:   * Language(s): C, C++
    * Purpose: The unqualified `%code` or `%code requires`
      should usually be more appropriate than `%code top`. However,
      occasionally it is necessary to insert code much nearer the top of the
      parser implementation file. For example:

      ```
      %code top {
        #define _GNU_SOURCE
        #include <stdio.h>
      }
      ```
    * Location(s): Near the top of the parser implementation file.

`imports` [¶](#index-_0025code-imports)
:   * Language(s): D, Java
    * Purpose: This is the best place to write Java import directives. D syntax
      allows for import statements all throughout the code.
    * Location(s): The parser Java file after any Java package directive and
      before any class definitions. The parser D file before any class definitions.

Though we say the insertion locations are language-dependent, they are
technically skeleton-dependent. Writers of non-standard skeletons
however should choose their locations consistently with the behavior
of the standard Bison skeletons.

---

Previous: [Bison Declarations](#Declarations), Up: [Bison Grammar Files](#Grammar-File)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 3.8 Multiple Parsers in the Same Program

Most programs that use Bison parse only one language and therefore contain
only one Bison parser. But what if you want to parse more than one language
with the same program? Then you need to avoid name conflicts between
different definitions of functions and variables such as `yyparse`,
`yylval`. To use different parsers from the same compilation unit, you
also need to avoid conflicts on types and macros (e.g., `YYSTYPE`)
exported in the generated header.

The easy way to do this is to define the `%define` variable
`api.prefix`. With different `api.prefix`s it is guaranteed that
headers do not conflict when included together, and that compiled objects
can be linked together too. Specifying ‘`%define api.prefix
{prefix}`’ (or passing the option `-Dapi.prefix={prefix}`, see
[Invoking Bison](#Invocation)) renames the interface functions and
variables of the Bison parser to start with prefix instead of
‘`yy`’, and all the macros to start by PREFIX (i.e., prefix
upper-cased) instead of ‘`YY`’.

The renamed symbols include `yyparse`, `yylex`, `yyerror`,
`yynerrs`, `yylval`, `yylloc`, `yychar` and
`yydebug`. If you use a push parser, `yypush_parse`,
`yypull_parse`, `yypstate`, `yypstate_new` and
`yypstate_delete` will also be renamed. The renamed macros include
`YYSTYPE`, `YYLTYPE`, and `YYDEBUG`, which is treated
specifically — more about this below.

For example, if you use ‘`%define api.prefix {c}`’, the names become
`cparse`, `clex`, …, `CSTYPE`, `CLTYPE`, and so
on.

Users of Flex must update the signature of the generated `yylex`
function. Since the Flex scanner usually includes the generated header of
the parser (to get the definitions of the tokens, etc.), the most convenient
way is to insert the declaration of `yylex` in the `provides`
section:

```
%define api.prefix {c}
// Emitted in the header file, after the definition of YYSTYPE.
%code provides
{
  // Tell Flex the expected prototype of yylex.
  #define YY_DECL                             \
    int clex (CSTYPE *yylval, CLTYPE *yylloc)

  // Declare the scanner.
  YY_DECL;
}
```

  

The `%define` variable `api.prefix` works in two different ways.
In the implementation file, it works by adding macro definitions to the
beginning of the parser implementation file, defining `yyparse` as
`prefixparse`, and so on:

```
#define YYSTYPE CTYPE
#define yyparse cparse
#define yylval  clval
...
YYSTYPE yylval;
int yyparse (void);
```

This effectively substitutes one name for the other in the entire parser
implementation file, thus the “original” names (`yylex`,
`YYSTYPE`, …) are also usable in the parser implementation file.

However, in the parser header file, the symbols are defined renamed, for
instance:

```
extern CSTYPE clval;
int cparse (void);
```

The macro `YYDEBUG` is commonly used to enable the tracing support in
parsers. To comply with this tradition, when `api.prefix` is used,
`YYDEBUG` (not renamed) is used as a default value:

```
/* Debug traces. */
#ifndef CDEBUG
# if defined YYDEBUG
#  if YYDEBUG
#   define CDEBUG 1
#  else
#   define CDEBUG 0
#  endif
# else
#  define CDEBUG 0
# endif
#endif
#if CDEBUG
extern int cdebug;
#endif
```

  
  

Prior to Bison 2.6, a feature similar to `api.prefix` was provided by
the obsolete directive `%name-prefix` (see [Bison Symbols](#Table-of-Symbols)) and
the option `--name-prefix` (see [Output Files](#Output-Files)).

---

Next: [The Bison Parser Algorithm](#Algorithm), Previous: [Bison Grammar Files](#Grammar-File), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 4 Parser C-Language Interface

The Bison parser is actually a C function named `yyparse`. Here we
describe the interface conventions of `yyparse` and the other
functions that it needs to use.

Keep in mind that the parser uses many C identifiers starting with
‘`yy`’ and ‘`YY`’ for internal purposes. If you use such an
identifier (aside from those in this manual) in an action or in epilogue
in the grammar file, you are likely to run into trouble.

* [The Parser Function `yyparse`](#Parser-Function)
* [Push Parser Interface](#Push-Parser-Interface)
* [The Lexical Analyzer Function `yylex`](#Lexical)
* [Error Reporting](#Error-Reporting)
* [Special Features for Use in Actions](#Action-Features)
* [Parser Internationalization](#Internationalization)

---

Next: [Push Parser Interface](#Push-Parser-Interface), Up: [Parser C-Language Interface](#Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 4.1 The Parser Function `yyparse`

You call the function `yyparse` to cause parsing to occur. This
function reads tokens, executes actions, and ultimately returns when it
encounters end-of-input or an unrecoverable syntax error. You can also
write an action which directs `yyparse` to return immediately
without reading further.

Function: *int* **yyparse** *(`void`)* [¶](#index-yyparse-1)
:   The value returned by `yyparse` is 0 if parsing was successful (return
    is due to end-of-input).

    The value is 1 if parsing failed because of invalid input, i.e., input
    that contains a syntax error or that causes `YYABORT` to be
    invoked.

    The value is 2 if parsing failed due to memory exhaustion.

In an action, you can cause immediate return from `yyparse` by using
these macros:

Macro: **YYACCEPT** [¶](#index-YYACCEPT)
:   Return immediately with value 0 (to report success).

Macro: **YYABORT** [¶](#index-YYABORT)
:   Return immediately with value 1 (to report failure).

Macro: **YYNOMEM** [¶](#index-YYNOMEM)
:   Return immediately with value 2 (to report memory exhaustion).

If you use a reentrant parser, you can optionally pass additional
parameter information to it in a reentrant way. To do so, use the
declaration `%parse-param`:

Directive: **%parse-param** *{argument-declaration} …* [¶](#index-_0025parse_002dparam)
:   Declare that one or more
    argument-declaration are additional `yyparse` arguments.
    The argument-declaration is used when declaring
    functions or prototypes. The last identifier in
    argument-declaration must be the argument name.

Here’s an example. Write this in the parser:

```
%parse-param {int *nastiness} {int *randomness}
```

Then call the parser like this:

```
{
  int nastiness, randomness;
  …  /* Store proper data in nastiness and randomness. */
  value = yyparse (&nastiness, &randomness);
  …
}
```

In the grammar actions, use expressions like this to refer to the data:

```
exp: …    { …; *randomness += 1; … }
```

Using the following:

```
%parse-param {int *randomness}
```

Results in these signatures:

```
void yyerror (int *randomness, const char *msg);
int  yyparse (int *randomness);
```

Or, if both `%define api.pure full` (or just `%define api.pure`)
and `%locations` are used:

```
void yyerror (YYLTYPE *llocp, int *randomness, const char *msg);
int  yyparse (int *randomness);
```

---

Next: [The Lexical Analyzer Function `yylex`](#Lexical), Previous: [The Parser Function `yyparse`](#Parser-Function), Up: [Parser C-Language Interface](#Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]
