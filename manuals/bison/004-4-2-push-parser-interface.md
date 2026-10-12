### 4.2 Push Parser Interface

You call the function `yypstate_new` to create a new parser instance.
This function is available if either the ‘`%define api.push-pull push`’
or ‘`%define api.push-pull both`’ declaration is used. See [A Push Parser](#Push-Decl).

Function: *yypstate\** **yypstate\_new** *(`void`)* [¶](#index-yypstate_005fnew-1)
:   Return a valid parser instance if there is memory available, 0 otherwise.
    In impure mode, it will also return 0 if a parser instance is currently
    allocated.

You call the function `yypstate_delete` to delete a parser instance.
function is available if either the ‘`%define api.push-pull push`’ or
‘`%define api.push-pull both`’ declaration is used.
See [A Push Parser](#Push-Decl).

Function: *void* **yypstate\_delete** *(`yypstate *`yyps)* [¶](#index-yypstate_005fdelete-1)
:   Reclaim the memory associated with a parser instance. After this call, you
    should no longer attempt to use the parser instance.

You call the function `yypush_parse` to parse a single token. This
function is available if either the ‘`%define api.push-pull push`’ or
‘`%define api.push-pull both`’ declaration is used. See [A Push Parser](#Push-Decl).

Function: *int* **yypush\_parse** *(`yypstate *`yyps)* [¶](#index-yypush_005fparse-1)
:   The value returned by `yypush_parse` is the same as for `yyparse`
    with the following exception: it returns `YYPUSH_MORE` if more input is
    required to finish parsing the grammar.

    After `yypush_parse` returned, the instance may be consulted. For
    instance check `yynerrs` to see whether there were (possibly recovered)
    syntax errors.

    After `yypush_parse` returns a status other than `YYPUSH_MORE`,
    the parser instance `yyps` may be reused for a new parse.

The fact that the parser state is reusable even after an error simplifies
reuse. For example, a calculator application which parses each input line
as an expression can just keep reusing the same `yyps` even if an input
was invalid.

You call the function `yypull_parse` to parse the rest of the input
stream. This function is available if the ‘`%define api.push-pull both`’
declaration is used. See [A Push Parser](#Push-Decl).

Function: *int* **yypull\_parse** *(`yypstate *`yyps)* [¶](#index-yypull_005fparse)
:   The value returned by `yypull_parse` is the same as for `yyparse`.

    The parser instance `yyps` may be reused for new parses.

Function: *int* **yypstate\_expected\_tokens** *(`const yypstate *`yyps, `yysymbol_kind_t` argv`[]`, `int` argc)* [¶](#index-yypstate_005fexpected_005ftokens)
:   Fill argv with the expected tokens, which never includes
    `YYSYMBOL_YYEMPTY`, `YYSYMBOL_YYerror`, or
    `YYSYMBOL_YYUNDEF`.

    Never put more than argc elements into argv, and on success
    return the number of tokens stored in argv. If there are more
    expected tokens than argc, fill argv up to argc and return
    0. If there are no expected tokens, also return 0, but set `argv[0]`
    to `YYSYMBOL_YYEMPTY`.

    When LAC is enabled, may return a negative number on errors,
    such as `YYENOMEM` on memory exhaustion.

    If argv is null, return the size needed to store all the possible
    values, which is always less than `YYNTOKENS`.

---

Next: [Error Reporting](#Error-Reporting), Previous: [Push Parser Interface](#Push-Parser-Interface), Up: [Parser C-Language Interface](#Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 4.3 The Lexical Analyzer Function `yylex`

The *lexical analyzer* function, `yylex`, recognizes tokens from
the input stream and returns them to the parser. Bison does not create
this function automatically; you must write it so that `yyparse` can
call it. The function is sometimes referred to as a lexical scanner.

In simple programs, `yylex` is often defined at the end of the Bison
grammar file. If `yylex` is defined in a separate source file, you
need to arrange for the token-kind definitions to be available there. To do
this, use the `-d` option when you run Bison, so that it will write
these definitions into the separate parser header file,
`name.tab.h`, which you can include in the other source files
that need it. See [Invoking Bison](#Invocation).

* [Calling Convention for `yylex`](#Calling-Convention)
* [Special Tokens](#Special-Tokens)
* [Finding Tokens by String Literals](#Tokens-from-Literals)
* [Semantic Values of Tokens](#Token-Values)
* [Textual Locations of Tokens](#Token-Locations)
* [Calling Conventions for Pure Parsers](#Pure-Calling)

---

Next: [Special Tokens](#Special-Tokens), Up: [The Lexical Analyzer Function `yylex`](#Lexical)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.3.1 Calling Convention for `yylex`

The value that `yylex` returns must be the positive numeric code for
the kind of token it has just found; a zero or negative value signifies
end-of-input.

When a token kind is referred to in the grammar rules by a name, that name
in the parser implementation file becomes an enumerator of the enum
`yytoken_kind_t` whose definition is the proper numeric code for that
token kind. So `yylex` should use the name to indicate that type.
See [Symbols, Terminal and Nonterminal](#Symbols).

When a token is referred to in the grammar rules by a character literal, the
numeric code for that character is also the code for the token kind. So
`yylex` can simply return that character code, possibly converted to
`unsigned char` to avoid sign-extension. The null character must not
be used this way, because its code is zero and that signifies end-of-input.

Here is an example showing these things:

```
int
yylex (void)
{
  …
  if (c == EOF)    /* Detect end-of-input. */
    return YYEOF;
  …
  else if (c == '+' || c == '-')
    return c;      /* Assume token kind for '+' is '+'. */
  …
  else
    return INT;    /* Return the kind of the token. */
  …
}
```

This interface has been designed so that the output from the `lex`
utility can be used without change as the definition of `yylex`.

---

Next: [Finding Tokens by String Literals](#Tokens-from-Literals), Previous: [Calling Convention for `yylex`](#Calling-Convention), Up: [The Lexical Analyzer Function `yylex`](#Lexical)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.3.2 Special Tokens

In addition to the user defined tokens, Bison generates a few special tokens
that `yylex` may return.

The `YYEOF` token denotes the end of file, and signals to the parser
that there is nothing left afterwards. See [Calling Convention for `yylex`](#Calling-Convention), for an
example.

Returning `YYUNDEF` tells the parser that some lexical error was found.
It will emit an error message about an “invalid token”, and enter
error-recovery (see [Error Recovery](#Error-Recovery)). Returning an unknown token kind
results in the exact same behavior.

Returning `YYerror` requires the parser to enter error-recovery
*without* emitting an error message. This way the lexical analyzer can
produce an accurate error messages about the invalid input (something the
parser cannot do), and yet benefit from the error-recovery features of the
parser.

```
int
yylex (void)
{
  …
  switch (c)
    {
      …
      case '0': case '1': case '2': case '3': case '4':
      case '5': case '6': case '7': case '8': case '9':
        …
        return TOK_NUM;
      …
      case EOF:
        return YYEOF;
      default:
        yyerror ("syntax error: invalid character: %c", c);
        return YYerror;
    }
}
```

---

Next: [Semantic Values of Tokens](#Token-Values), Previous: [Special Tokens](#Special-Tokens), Up: [The Lexical Analyzer Function `yylex`](#Lexical)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.3.3 Finding Tokens by String Literals

If the grammar uses literal string tokens, there are two ways that
`yylex` can determine the token kind codes for them:

* If the grammar defines symbolic token names as aliases for the literal
  string tokens, `yylex` can use these symbolic names like all others.
  In this case, the use of the literal string tokens in the grammar file has
  no effect on `yylex`.

  This is the preferred approach.
* `yylex` can search for the multicharacter token in the `yytname`
  table. This method is discouraged: the primary purpose of string aliases is
  forging good error messages, not describing the spelling of keywords. In
  addition, looking for the token kind at runtime incurs a (small but
  noticeable) cost.

  The `yytname` table is generated only if you use the
  `%token-table` declaration. See [Bison Declaration Summary](#Decl-Summary).

---

Next: [Textual Locations of Tokens](#Token-Locations), Previous: [Finding Tokens by String Literals](#Tokens-from-Literals), Up: [The Lexical Analyzer Function `yylex`](#Lexical)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.3.4 Semantic Values of Tokens

In an ordinary (nonreentrant) parser, the semantic value of the token must
be stored into the global variable `yylval`. When you are using just
one data type for semantic values, `yylval` has that type. Thus, if
the type is `int` (the default), you might write this in `yylex`:

```
  …
  yylval = value;  /* Put value onto Bison stack. */
  return INT;      /* Return the kind of the token. */
  …
```

When you are using multiple data types, `yylval`’s type is a union made
from the `%union` declaration (see [The Union Declaration](#Union-Decl)). So when you store
a token’s value, you must use the proper member of the union. If the
`%union` declaration looks like this:

```
%union {
  int intval;
  double val;
  symrec *tptr;
}
```

then the code in `yylex` might look like this:

```
  …
  yylval.intval = value; /* Put value onto Bison stack. */
  return INT;            /* Return the kind of the token. */
  …
```

---

Next: [Calling Conventions for Pure Parsers](#Pure-Calling), Previous: [Semantic Values of Tokens](#Token-Values), Up: [The Lexical Analyzer Function `yylex`](#Lexical)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.3.5 Textual Locations of Tokens

If you are using the ‘`@n`’-feature (see [Tracking Locations](#Tracking-Locations))
in actions to keep track of the textual locations of tokens and groupings,
then you must provide this information in `yylex`. The function
`yyparse` expects to find the textual location of a token just parsed
in the global variable `yylloc`. So `yylex` must store the proper
data in that variable.

By default, the value of `yylloc` is a structure and you need only
initialize the members that are going to be used by the actions. The
four members are called `first_line`, `first_column`,
`last_line` and `last_column`. Note that the use of this
feature makes the parser noticeably slower.

The data type of `yylloc` has the name `YYLTYPE`.

---

Previous: [Textual Locations of Tokens](#Token-Locations), Up: [The Lexical Analyzer Function `yylex`](#Lexical)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.3.6 Calling Conventions for Pure Parsers

When you use the Bison declaration `%define api.pure full` to request a
pure, reentrant parser, the global communication variables `yylval` and
`yylloc` cannot be used. (See [A Pure (Reentrant) Parser](#Pure-Decl).) In such parsers the two
global variables are replaced by pointers passed as arguments to
`yylex`. You must declare them as shown here, and pass the information
back by storing it through those pointers.

```
int
yylex (YYSTYPE *lvalp, YYLTYPE *llocp)
{
  …
  *lvalp = value;  /* Put value onto Bison stack. */
  return INT;      /* Return the kind of the token. */
  …
}
```

If the grammar file does not use the ‘`@`’ constructs to refer to
textual locations, then the type `YYLTYPE` will not be defined. In
this case, omit the second argument; `yylex` will be called with
only one argument.

If you wish to pass additional arguments to `yylex`, use
`%lex-param` just like `%parse-param` (see [The Parser Function `yyparse`](#Parser-Function)). To pass additional arguments to both `yylex` and
`yyparse`, use `%param`.

Directive: **%lex-param** *{argument-declaration} …* [¶](#index-_0025lex_002dparam)
:   Specify that argument-declaration are additional `yylex` argument
    declarations. You may pass one or more such declarations, which is
    equivalent to repeating `%lex-param`.

Directive: **%param** *{argument-declaration} …* [¶](#index-_0025param)
:   Specify that argument-declaration are additional
    `yylex`/`yyparse` argument declaration. This is equivalent to
    ‘`%lex-param {argument-declaration} … %parse-param
    {argument-declaration} …`’. You may pass one or more
    declarations, which is equivalent to repeating `%param`.

For instance:

```
%lex-param   {scanner_mode *mode}
%parse-param {parser_mode *mode}
%param       {environment_type *env}
```

results in the following signatures:

```
int yylex   (scanner_mode *mode, environment_type *env);
int yyparse (parser_mode *mode, environment_type *env);
```

If ‘`%define api.pure full`’ is added:

```
int yylex   (YYSTYPE *lvalp, scanner_mode *mode, environment_type *env);
int yyparse (parser_mode *mode, environment_type *env);
```

and finally, if both ‘`%define api.pure full`’ and `%locations` are
used:

```
int yylex   (YYSTYPE *lvalp, YYLTYPE *llocp,
             scanner_mode *mode, environment_type *env);
int yyparse (parser_mode *mode, environment_type *env);
```

---

Next: [Special Features for Use in Actions](#Action-Features), Previous: [The Lexical Analyzer Function `yylex`](#Lexical), Up: [Parser C-Language Interface](#Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 4.4 Error Reporting

During its execution the parser may have error messages to pass to the user,
such as syntax error, or memory exhaustion. How this message is delivered
to the user must be specified by the developer.

* [The Error Reporting Function `yyerror`](#Error-Reporting-Function)
* [The Syntax Error Reporting Function `yyreport_syntax_error`](#Syntax-Error-Reporting-Function)

---

Next: [The Syntax Error Reporting Function `yyreport_syntax_error`](#Syntax-Error-Reporting-Function), Up: [Error Reporting](#Error-Reporting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.4.1 The Error Reporting Function `yyerror`

The Bison parser detects a *syntax error* (or *parse error*)
whenever it reads a token which cannot satisfy any syntax rule. An
action in the grammar can also explicitly proclaim an error, using the
macro `YYERROR` (see [Special Features for Use in Actions](#Action-Features)).

The Bison parser expects to report the error by calling an error
reporting function named `yyerror`, which you must supply. It is
called by `yyparse` whenever a syntax error is found, and it
receives one argument. For a syntax error, the string is normally
`"syntax error"`.

If you invoke ‘`%define parse.error detailed`’ (or ‘`custom`’) in the
Bison declarations section (see [The Bison Declarations Section](#Bison-Declarations)), then Bison provides
a more verbose and specific error message string instead of just plain
`"syntax error"`. However, that message sometimes contains
incorrect information if LAC is not enabled (see [LAC](#LAC)).

The parser can detect one other kind of error: memory exhaustion. This
can happen when the input contains constructions that are very deeply
nested. It isn’t likely you will encounter this, since the Bison
parser normally extends its stack automatically up to a very large limit. But
if memory is exhausted, `yyparse` calls `yyerror` in the usual
fashion, except that the argument string is `"memory exhausted"`.

In some cases diagnostics like `"syntax error"` are
translated automatically from English to some other language before
they are passed to `yyerror`. See [Parser Internationalization](#Internationalization).

The following definition suffices in simple programs:

```
void
yyerror (char const *s)
{
```

```
  fprintf (stderr, "%s\n", s);
}
```

After `yyerror` returns to `yyparse`, the latter will attempt
error recovery if you have written suitable error recovery grammar rules
(see [Error Recovery](#Error-Recovery)). If recovery is impossible, `yyparse` will
immediately return 1.

Obviously, in location tracking pure parsers, `yyerror` should have
an access to the current location. With `%define api.pure`, this is
indeed the case for the GLR parsers, but not for the Yacc parser, for
historical reasons, and this is the why `%define api.pure full` should be
preferred over `%define api.pure`.

When `%locations %define api.pure full` is used, `yyerror` has the
following signature:

```
void yyerror (YYLTYPE *locp, char const *msg);
```

The prototypes are only indications of how the code produced by Bison
uses `yyerror`. Bison-generated code always ignores the returned
value, so `yyerror` can return any type, including `void`.
Also, `yyerror` can be a variadic function; that is why the
message is always passed last.

Traditionally `yyerror` returns an `int` that is always
ignored, but this is purely for historical reasons, and `void` is
preferable since it more accurately describes the return type for
`yyerror`.

The variable `yynerrs` contains the number of syntax errors
reported so far. Normally this variable is global; but if you
request a pure parser (see [A Pure (Reentrant) Parser](#Pure-Decl))
then it is a local variable which only the actions can access.

---

Previous: [The Error Reporting Function `yyerror`](#Error-Reporting-Function), Up: [Error Reporting](#Error-Reporting)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.4.2 The Syntax Error Reporting Function `yyreport_syntax_error`

If you invoke ‘`%define parse.error custom`’ (see [The Bison Declarations Section](#Bison-Declarations)), then the parser no longer passes syntax error messages to
`yyerror`, rather it delegates that task to the user by calling the
`yyreport_syntax_error` function.

The following functions and types are “`static`”: they are defined in
the implementation file (`*.c`) and available only from there. They
are meant to be used from the grammar’s epilogue.

Function: *static int* **yyreport\_syntax\_error** *(`const yypcontext_t *`ctx)* [¶](#index-yyreport_005fsyntax_005ferror)
:   Report a syntax error to the user. Return 0 on success, `YYENOMEM` on
    memory exhaustion. Whether it uses `yyerror` is up to the user.

Use the following types and functions to build the error message.

Type: **yypcontext\_t** [¶](#index-yypcontext_005ft)
:   An opaque type that captures the circumstances of the syntax error.

Type: **yysymbol\_kind\_t** [¶](#index-yysymbol_005fkind_005ft)
:   An enum of all the grammar symbols, tokens and nonterminals. Its
    enumerators are forged from the symbol names:

    ```
    enum yysymbol_kind_t
    {
      YYSYMBOL_YYEMPTY = -2,      /* No symbol.  */
      YYSYMBOL_YYEOF = 0,         /* "end of file"  */
      YYSYMBOL_YYerror = 1,       /* error  */
      YYSYMBOL_YYUNDEF = 2,       /* "invalid token"  */
      YYSYMBOL_PLUS = 3,          /* "+"  */
      YYSYMBOL_MINUS = 4,         /* "-"  */
      [...]
      YYSYMBOL_VAR = 14,          /* "variable"  */
      YYSYMBOL_NEG = 15,          /* NEG  */
      YYSYMBOL_YYACCEPT = 16,     /* $accept  */
      YYSYMBOL_exp = 17,          /* exp  */
      YYSYMBOL_input = 18         /* input  */
    };
    typedef enum yysymbol_kind_t yysymbol_kind_t;
    ```

Function: *static yysymbol\_kind\_t* **yypcontext\_token** *(`const yypcontext_t *`ctx)* [¶](#index-yypcontext_005ftoken)
:   The “unexpected” token: the symbol kind of the lookahead token that caused
    the syntax error. Returns `YYSYMBOL_YYEMPTY` if there is no lookahead.

Function: *static YYLTYPE \** **yypcontext\_location** *(`const yypcontext_t *`ctx)* [¶](#index-yypcontext_005flocation)
:   The location of the syntax error (that of the unexpected token).

Function: *static int* **yypcontext\_expected\_tokens** *(`const yypcontext_t *`ctx, `yysymbol_kind_t` argv`[]`, `int` argc)* [¶](#index-yypcontext_005fexpected_005ftokens)
:   Fill argv with the expected tokens, which never includes
    `YYSYMBOL_YYEMPTY`, `YYSYMBOL_YYerror`, or
    `YYSYMBOL_YYUNDEF`.

    Never put more than argc elements into argv, and on success
    return the number of tokens stored in argv. If there are more
    expected tokens than argc, fill argv up to argc and return
    0. If there are no expected tokens, also return 0, but set `argv[0]`
    to `YYSYMBOL_YYEMPTY`.

    When LAC is enabled, may return a negative number on errors,
    such as `YYENOMEM` on memory exhaustion.

    If argv is null, return the size needed to store all the possible
    values, which is always less than `YYNTOKENS`.

Function: *static const char \** **yysymbol\_name** *(`symbol_kind_t` symbol)* [¶](#index-yysymbol_005fname)
:   The name of the symbol whose kind is symbol, possibly translated.

A custom syntax error function looks as follows. This implementation is
inappropriate for internationalization, see the `c/bistromathic`
example for a better alternative.

```
static int
yyreport_syntax_error (const yypcontext_t *ctx)
{
  int res = 0;
  YYLOCATION_PRINT (stderr, *yypcontext_location (ctx));
  fprintf (stderr, ": syntax error");
  // Report the tokens expected at this point.
  {
    enum { TOKENMAX = 5 };
    yysymbol_kind_t expected[TOKENMAX];
    int n = yypcontext_expected_tokens (ctx, expected, TOKENMAX);
    if (n < 0)
      // Forward errors to yyparse.
      res = n;
    else
      for (int i = 0; i < n; ++i)
        fprintf (stderr, "%s %s",
                 i == 0 ? ": expected" : " or", yysymbol_name (expected[i]));
  }
  // Report the unexpected token.
  {
    yysymbol_kind_t lookahead = yypcontext_token (ctx);
    if (lookahead != YYSYMBOL_YYEMPTY)
      fprintf (stderr, " before %s", yysymbol_name (lookahead));
  }
  fprintf (stderr, "\n");
  return res;
}
```

You still must provide a `yyerror` function, used for instance to
report memory exhaustion.

---

Next: [Parser Internationalization](#Internationalization), Previous: [Error Reporting](#Error-Reporting), Up: [Parser C-Language Interface](#Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 4.5 Special Features for Use in Actions

Here is a table of Bison constructs, variables and macros that are useful in
actions.

Variable: **$$** [¶](#index-_0024_0024)
:   Acts like a variable that contains the semantic value for the
    grouping made by the current rule. See [Actions](#Actions).

Variable: **$n** [¶](#index-_0024n)
:   Acts like a variable that contains the semantic value for the
    nth component of the current rule. See [Actions](#Actions).

Variable: **$<typealt>$** [¶](#index-_0024_003ctypealt_003e_0024)
:   Like `$$` but specifies alternative typealt in the union
    specified by the `%union` declaration. See [Data Types of Values in Actions](#Action-Types).

Variable: **$<typealt>n** [¶](#index-_0024_003ctypealt_003en)
:   Like `$n` but specifies alternative typealt in the
    union specified by the `%union` declaration.
    See [Data Types of Values in Actions](#Action-Types).

Macro: **YYABORT** *`;`* [¶](#index-YYABORT-2)
:   Return immediately from `yyparse`, indicating failure.
    See [The Parser Function `yyparse`](#Parser-Function).

Macro: **YYACCEPT** *`;`* [¶](#index-YYACCEPT-2)
:   Return immediately from `yyparse`, indicating success.
    See [The Parser Function `yyparse`](#Parser-Function).

Macro: **YYBACKUP** *(token, value)`;`* [¶](#index-YYBACKUP)
:   Unshift a token. This macro is allowed only for rules that reduce
    a single value, and only when there is no lookahead token.
    It is also disallowed in GLR parsers.
    It installs a lookahead token with token kind token and
    semantic value value; then it discards the value that was
    going to be reduced by this rule.

    If the macro is used when it is not valid, such as when there is
    a lookahead token already, then it reports a syntax error with
    a message ‘`cannot back up`’ and performs ordinary error
    recovery.

    In either case, the rest of the action is not executed.

Value: **YYEMPTY** [¶](#index-YYEMPTY)
:   Value stored in `yychar` when there is no lookahead token.

Value: **YYEOF** [¶](#index-YYEOF)
:   Value stored in `yychar` when the lookahead is the end of the input
    stream.

Macro: **YYERROR** *`;`* [¶](#index-YYERROR-1)
:   Cause an immediate syntax error. This statement initiates error
    recovery just as if the parser itself had detected an error; however, it
    does not call `yyerror`, and does not print any message. If you
    want to print an error message, call `yyerror` explicitly before
    the ‘`YYERROR;`’ statement. See [Error Recovery](#Error-Recovery).

Macro: **YYNOMEM** *`;`* [¶](#index-YYNOMEM-2)
:   Return immediately from `yyparse`, indicating memory exhaustion.
    See [The Parser Function `yyparse`](#Parser-Function).

Macro: **YYRECOVERING** [¶](#index-YYRECOVERING)
:   The expression `YYRECOVERING ()` yields 1 when the parser
    is recovering from a syntax error, and 0 otherwise.
    See [Error Recovery](#Error-Recovery).

Variable: **yychar** [¶](#index-yychar)
:   Variable containing either the lookahead token, or `YYEOF` when the
    lookahead is the end of the input stream, or `YYEMPTY` when no lookahead
    has been performed so the next token is not yet known.
    Do not modify `yychar` in a deferred semantic action (see [GLR Semantic Actions](#GLR-Semantic-Actions)).
    See [Lookahead Tokens](#Lookahead).

Macro: **yyclearin** *`;`* [¶](#index-yyclearin-1)
:   Discard the current lookahead token. This is useful primarily in
    error rules.
    Do not invoke `yyclearin` in a deferred semantic action (see [GLR Semantic Actions](#GLR-Semantic-Actions)).
    See [Error Recovery](#Error-Recovery).

Macro: **yyerrok** *`;`* [¶](#index-yyerrok)
:   Resume generating error messages immediately for subsequent syntax
    errors. This is useful primarily in error rules.
    See [Error Recovery](#Error-Recovery).

Variable: **yylloc** [¶](#index-yylloc)
:   Variable containing the lookahead token location when `yychar` is not set
    to `YYEMPTY` or `YYEOF`.
    Do not modify `yylloc` in a deferred semantic action (see [GLR Semantic Actions](#GLR-Semantic-Actions)).
    See [Actions and Locations](#Actions-and-Locations).

Variable: **yylval** [¶](#index-yylval)
:   Variable containing the lookahead token semantic value when `yychar` is
    not set to `YYEMPTY` or `YYEOF`.
    Do not modify `yylval` in a deferred semantic action (see [GLR Semantic Actions](#GLR-Semantic-Actions)).
    See [Actions](#Actions).

Value: **@$** [¶](#index-_0040_0024)
:   Acts like a structure variable containing information on the textual
    location of the grouping made by the current rule. See [Tracking Locations](#Tracking-Locations).

Value: **@n** [¶](#index-_0040n)
:   Acts like a structure variable containing information on the textual
    location of the nth component of the current rule. See [Tracking Locations](#Tracking-Locations).

---

Previous: [Special Features for Use in Actions](#Action-Features), Up: [Parser C-Language Interface](#Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 4.6 Parser Internationalization

A Bison-generated parser can print diagnostics, including error and
tracing messages. By default, they appear in English. However, Bison
also supports outputting diagnostics in the user’s native language. To
make this work, the user should set the usual environment variables.
See [The User’s View](https://www.gnu.org/software/gettext/manual/gettext.html#Users) in GNU `gettext` utilities.
For example, the shell command ‘`export LC_ALL=fr_CA.UTF-8`’ might
set the user’s locale to French Canadian using the UTF-8
encoding. The exact set of available locales depends on the user’s
installation.

* [Enabling Internationalization](#Enabling-I18n)
* [Token Internationalization](#Token-I18n)

---

Next: [Token Internationalization](#Token-I18n), Up: [Parser Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.6.1 Enabling Internationalization

The maintainer of a package that uses a Bison-generated parser enables
the internationalization of the parser’s output through the following
steps. Here we assume a package that uses GNU Autoconf and
GNU Automake.

1. Into the directory containing the GNU Autoconf macros used
   by the package —often called `m4`— copy the
   `bison-i18n.m4` file installed by Bison under
   ‘`share/aclocal/bison-i18n.m4`’ in Bison’s installation directory.
   For example:

   ```
   cp /usr/local/share/aclocal/bison-i18n.m4 m4/bison-i18n.m4
   ```
2. In the top-level `configure.ac`, after the `AM_GNU_GETTEXT`
   invocation, add an invocation of `BISON_I18N`. This macro is
   defined in the file `bison-i18n.m4` that you copied earlier. It
   causes `configure` to find the value of the
   `BISON_LOCALEDIR` variable, and it defines the source-language
   symbol `YYENABLE_NLS` to enable translations in the
   Bison-generated parser.
3. In the `main` function of your program, designate the directory
   containing Bison’s runtime message catalog, through a call to
   ‘`bindtextdomain`’ with domain name ‘`bison-runtime`’.
   For example:

   ```
   bindtextdomain ("bison-runtime", BISON_LOCALEDIR);
   ```

   Typically this appears after any other call `bindtextdomain
   (PACKAGE, LOCALEDIR)` that your package already has. Here we rely on
   ‘`BISON_LOCALEDIR`’ to be defined as a string through the
   `Makefile`.
4. In the `Makefile.am` that controls the compilation of the `main`
   function, make ‘`BISON_LOCALEDIR`’ available as a C preprocessor macro,
   either in ‘`DEFS`’ or in ‘`AM_CPPFLAGS`’. For example:

   ```
   DEFS = @DEFS@ -DBISON_LOCALEDIR='"$(BISON_LOCALEDIR)"'
   ```

   or:

   ```
   AM_CPPFLAGS = -DBISON_LOCALEDIR='"$(BISON_LOCALEDIR)"'
   ```
5. Finally, invoke the command `autoreconf` to generate the build
   infrastructure.

---

Previous: [Enabling Internationalization](#Enabling-I18n), Up: [Parser Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 4.6.2 Token Internationalization

When the `%define` variable `parse.error` is set to `custom`
or `detailed`, token aliases can be internationalized:

```
%token
    '\n'   _("end of line")
  <double>
    NUM    _("number")
  <symrec*>
    FUN    _("function")
    VAR    _("variable")
```

The remainder of the grammar may freely use either the token symbol
(`FUN`) or its alias (`"function"`), but not with the
internationalization marker (`_("function")`).

If at least one token alias is internationalized, then the generated parser
will use both `N_` and `_`, that must be defined
(see [The Programmer’s View](https://www.gnu.org/software/gettext/manual/gettext.html#Programmers) in GNU `gettext`
utilities). They are used only on string aliases marked for translation.
In other words, even if your catalog features a translation for
“function”, then with

```
%token
  <symrec*>
    FUN      "function"
    VAR    _("variable")
```

“function” will appear untranslated in debug traces and error messages.

Unless defined by the user, the end-of-file token, `YYEOF`, is provided
“end of file” as an alias. It is also internationalized if the user
internationalized tokens. To map it to another string, use:

```
%token END 0 _("end of input")
```

---

Next: [Error Recovery](#Error-Recovery), Previous: [Parser C-Language Interface](#Interface), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 5 The Bison Parser Algorithm

As Bison reads tokens, it pushes them onto a stack along with their
semantic values. The stack is called the *parser stack*. Pushing a
token is traditionally called *shifting*.

For example, suppose the infix calculator has read ‘`1 + 5 *`’, with a
‘`3`’ to come. The stack will have four elements, one for each token
that was shifted.

But the stack does not always have an element for each token read. When
the last n tokens and groupings shifted match the components of a
grammar rule, they can be combined according to that rule. This is called
*reduction*. Those tokens and groupings are replaced on the stack by a
single grouping whose symbol is the result (left hand side) of that rule.
Running the rule’s action is part of the process of reduction, because this
is what computes the semantic value of the resulting grouping.

For example, if the infix calculator’s parser stack contains this:

```
1 + 5 * 3
```

and the next input token is a newline character, then the last three
elements can be reduced to 15 via the rule:

```
expr: expr '*' expr;
```

Then the stack contains just these three elements:

```
1 + 15
```

At this point, another reduction can be made, resulting in the single value
16. Then the newline token can be shifted.

The parser tries, by shifts and reductions, to reduce the entire input down
to a single grouping whose symbol is the grammar’s start-symbol
(see [Languages and Context-Free Grammars](#Language-and-Grammar)).

This kind of parser is known in the literature as a bottom-up parser.

* [Lookahead Tokens](#Lookahead)
* [Shift/Reduce Conflicts](#Shift_002fReduce)
* [Operator Precedence](#Precedence)
* [Context-Dependent Precedence](#Contextual-Precedence)
* [Parser States](#Parser-States)
* [Reduce/Reduce Conflicts](#Reduce_002fReduce)
* [Mysterious Conflicts](#Mysterious-Conflicts)
* [Tuning LR](#Tuning-LR)
* [Generalized LR (GLR) Parsing](#Generalized-LR-Parsing)
* [Memory Management, and How to Avoid Memory Exhaustion](#Memory-Management)

---

Next: [Shift/Reduce Conflicts](#Shift_002fReduce), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 5.1 Lookahead Tokens

The Bison parser does *not* always reduce immediately as soon as the
last n tokens and groupings match a rule. This is because such a
simple strategy is inadequate to handle most languages. Instead, when a
reduction is possible, the parser sometimes “looks ahead” at the next
token in order to decide what to do.

When a token is read, it is not immediately shifted; first it becomes the
*lookahead token*, which is not on the stack. Now the parser can
perform one or more reductions of tokens and groupings on the stack, while
the lookahead token remains off to the side. When no more reductions
should take place, the lookahead token is shifted onto the stack. This
does not mean that all possible reductions have been done; depending on the
token kind of the lookahead token, some rules may choose to delay their
application.

Here is a simple case where lookahead is needed. These three rules define
expressions which contain binary addition operators and postfix unary
factorial operators (‘`!`’), and allow parentheses for grouping.

```
expr:
  term '+' expr
| term
;
```

```

```

```
term:
  '(' expr ')'
| term '!'
| "number"
;
```

Suppose that the tokens ‘`1 + 2`’ have been read and shifted; what
should be done? If the following token is ‘`)`’, then the first three
tokens must be reduced to form an `expr`. This is the only valid
course, because shifting the ‘`)`’ would produce a sequence of symbols
`term ')'`, and no rule allows this.

If the following token is ‘`!`’, then it must be shifted immediately so
that ‘`2 !`’ can be reduced to make a `term`. If instead the
parser were to reduce before shifting, ‘`1 + 2`’ would become an
`expr`. It would then be impossible to shift the ‘`!`’ because
doing so would produce on the stack the sequence of symbols `expr
'!'`. No rule allows that sequence.

The lookahead token is stored in the variable `yychar`. Its semantic
value and location, if any, are stored in the variables `yylval` and
`yylloc`. See [Special Features for Use in Actions](#Action-Features).

---

Next: [Operator Precedence](#Precedence), Previous: [Lookahead Tokens](#Lookahead), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 5.2 Shift/Reduce Conflicts

Suppose we are parsing a language which has if-then and if-then-else
statements, with a pair of rules like this:

```
if_stmt:
  "if" expr "then" stmt
| "if" expr "then" stmt "else" stmt
;
```

Here `"if"`, `"then"` and `"else"` are terminal symbols for
specific keyword tokens.

When the `"else"` token is read and becomes the lookahead token, the
contents of the stack (assuming the input is valid) are just right for
reduction by the first rule. But it is also legitimate to shift the
`"else"`, because that would lead to eventual reduction by the second
rule.

This situation, where either a shift or a reduction would be valid, is
called a *shift/reduce conflict*. Bison is designed to resolve
these conflicts by choosing to shift, unless otherwise directed by
operator precedence declarations. To see the reason for this, let’s
contrast it with the other alternative.

Since the parser prefers to shift the `"else"`, the result is to attach
the else-clause to the innermost if-statement, making these two inputs
equivalent:

```
if x then if y then win; else lose;

if x then do; if y then win; else lose; end;
```

But if the parser chose to reduce when possible rather than shift, the
result would be to attach the else-clause to the outermost if-statement,
making these two inputs equivalent:

```
if x then if y then win; else lose;

if x then do; if y then win; end; else lose;
```

The conflict exists because the grammar as written is ambiguous: either
parsing of the simple nested if-statement is legitimate. The established
convention is that these ambiguities are resolved by attaching the
else-clause to the innermost if-statement; this is what Bison accomplishes
by choosing to shift rather than reduce. (It would ideally be cleaner to
write an unambiguous grammar, but that is very hard to do in this case.)
This particular ambiguity was first encountered in the specifications of
Algol 60 and is called the “dangling `else`” ambiguity.

To assist the grammar author in understanding the nature of each conflict,
Bison can be asked to generate “counterexamples”. In the present case it
actually even proves that the grammar is ambiguous by exhibiting a string
with two different parses:

```
  Example: "if" expr "then" "if" expr "then" stmt • "else" stmt
  Shift derivation
    if_stmt
    ↳ 3: "if" expr "then" stmt
                           ↳ 2: if_stmt
                                 ↳ 4: "if" expr "then" stmt • "else" stmt
  Example: "if" expr "then" "if" expr "then" stmt • "else" stmt
  Reduce derivation
    if_stmt
    ↳ 4: "if" expr "then" stmt                                "else" stmt
                           ↳ 2: if_stmt
                                 ↳ 3: "if" expr "then" stmt •
```

See [Generation of Counterexamples](#Counterexamples), for more details.

  

To avoid warnings from Bison about predictable, *legitimate* shift/reduce
conflicts, you can use the `%expect n` declaration.
There will be no warning as long as the number of shift/reduce conflicts
is exactly n, and Bison will report an error if there is a
different number.
See [Suppressing Conflict Warnings](#Expect-Decl). However, we don’t
recommend the use of `%expect` (except ‘`%expect 0`’!), as an equal
number of conflicts does not mean that they are the *same*. When
possible, you should rather use precedence directives to *fix* the
conflicts explicitly (see [Using Precedence For Non Operators](#Non-Operators)).

The definition of `if_stmt` above is solely to blame for the
conflict, but the conflict does not actually appear without additional
rules. Here is a complete Bison grammar file that actually manifests
the conflict:

```
%%
```

```
stmt:
  expr
| if_stmt
;
```

```

```

```
if_stmt:
  "if" expr "then" stmt
| "if" expr "then" stmt "else" stmt
;
```

```
expr:
  "identifier"
;
```

---

Next: [Context-Dependent Precedence](#Contextual-Precedence), Previous: [Shift/Reduce Conflicts](#Shift_002fReduce), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 5.3 Operator Precedence

Another situation where shift/reduce conflicts appear is in arithmetic
expressions. Here shifting is not always the preferred resolution; the
Bison declarations for operator precedence allow you to specify when to
shift and when to reduce.

* [When Precedence is Needed](#Why-Precedence)
* [Specifying Operator Precedence](#Using-Precedence)
* [Specifying Precedence Only](#Precedence-Only)
* [Precedence Examples](#Precedence-Examples)
* [How Precedence Works](#How-Precedence)
* [Using Precedence For Non Operators](#Non-Operators)

---

Next: [Specifying Operator Precedence](#Using-Precedence), Up: [Operator Precedence](#Precedence)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.3.1 When Precedence is Needed

Consider the following ambiguous grammar fragment (ambiguous because the
input ‘`1 - 2 * 3`’ can be parsed in two different ways):

```
expr:
  expr '-' expr
| expr '*' expr
| expr '<' expr
| '(' expr ')'
…
;
```

Suppose the parser has seen the tokens ‘`1`’, ‘`-`’ and ‘`2`’;
should it reduce them via the rule for the subtraction operator? It
depends on the next token. Of course, if the next token is ‘`)`’, we
must reduce; shifting is invalid because no single rule can reduce the
token sequence ‘`- 2 )`’ or anything starting with that. But if
the next token is ‘`*`’ or ‘`<`’, we have a choice: either
shifting or reduction would allow the parse to complete, but with
different results.

To decide which one Bison should do, we must consider the results. If
the next operator token op is shifted, then it must be reduced
first in order to permit another opportunity to reduce the difference.
The result is (in effect) ‘`1 - (2 op 3)`’. On the other
hand, if the subtraction is reduced before shifting op, the result
is ‘`(1 - 2) op 3`’. Clearly, then, the choice of shift or
reduce should depend on the relative precedence of the operators
‘`-`’ and op: ‘`*`’ should be shifted first, but not
‘`<`’.

What about input such as ‘`1 - 2 - 5`’; should this be
‘`(1 - 2) - 5`’ or should it be ‘`1 - (2 - 5)`’? For most
operators we prefer the former, which is called *left association*.
The latter alternative, *right association*, is desirable for
assignment operators. The choice of left or right association is a
matter of whether the parser chooses to shift or reduce when the stack
contains ‘`1 - 2`’ and the lookahead token is ‘`-`’: shifting
makes right-associativity.

---

Next: [Specifying Precedence Only](#Precedence-Only), Previous: [When Precedence is Needed](#Why-Precedence), Up: [Operator Precedence](#Precedence)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.3.2 Specifying Operator Precedence

Bison allows you to specify these choices with the operator precedence
declarations `%left` and `%right`. Each such declaration
contains a list of tokens, which are operators whose precedence and
associativity is being declared. The `%left` declaration makes all
those operators left-associative and the `%right` declaration makes
them right-associative. A third alternative is `%nonassoc`, which
declares that it is a syntax error to find the same operator twice “in a
row”.
The last alternative, `%precedence`, allows to define only
precedence and no associativity at all. As a result, any
associativity-related conflict that remains will be reported as an
compile-time error. The directive `%nonassoc` creates run-time
error: using the operator in a associative way is a syntax error. The
directive `%precedence` creates compile-time errors: an operator
*can* be involved in an associativity-related conflict, contrary to
what expected the grammar author.

The relative precedence of different operators is controlled by the
order in which they are declared. The first precedence/associativity
declaration in the file declares the operators whose
precedence is lowest, the next such declaration declares the operators
whose precedence is a little higher, and so on.

---

Next: [Precedence Examples](#Precedence-Examples), Previous: [Specifying Operator Precedence](#Using-Precedence), Up: [Operator Precedence](#Precedence)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.3.3 Specifying Precedence Only

Since POSIX Yacc defines only `%left`, `%right`, and
`%nonassoc`, which all defines precedence and associativity, little
attention is paid to the fact that precedence cannot be defined without
defining associativity. Yet, sometimes, when trying to solve a
conflict, precedence suffices. In such a case, using `%left`,
`%right`, or `%nonassoc` might hide future (associativity
related) conflicts that would remain hidden.

The dangling `else` ambiguity (see [Shift/Reduce Conflicts](#Shift_002fReduce)) can be solved
explicitly. This shift/reduce conflicts occurs in the following situation,
where the period denotes the current parsing state:

```
if e1 then if  e2 then s1 • else s2
```

The conflict involves the reduction of the rule ‘`IF expr THEN
stmt`’, which precedence is by default that of its last token
(`THEN`), and the shifting of the token `ELSE`. The usual
disambiguation (attach the `else` to the closest `if`),
shifting must be preferred, i.e., the precedence of `ELSE` must be
higher than that of `THEN`. But neither is expected to be involved
in an associativity related conflict, which can be specified as follows.

```
%precedence THEN
%precedence ELSE
```

The unary-minus is another typical example where associativity is usually
over-specified, see [Infix Notation Calculator: `calc`](#Infix-Calc). The `%left` directive is
traditionally used to declare the precedence of `NEG`, which is more
than needed since it also defines its associativity. While this is harmless
in the traditional example, who knows how `NEG` might be used in future
evolutions of the grammar…

---

Next: [How Precedence Works](#How-Precedence), Previous: [Specifying Precedence Only](#Precedence-Only), Up: [Operator Precedence](#Precedence)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.3.4 Precedence Examples

In our example, we would want the following declarations:

```
%left '<'
%left '-'
%left '*'
```

In a more complete example, which supports other operators as well, we
would declare them in groups of equal precedence. For example, `'+'` is
declared with `'-'`:

```
%left '<' '>' '=' "!=" "<=" ">="
%left '+' '-'
%left '*' '/'
```

---

Next: [Using Precedence For Non Operators](#Non-Operators), Previous: [Precedence Examples](#Precedence-Examples), Up: [Operator Precedence](#Precedence)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.3.5 How Precedence Works

The first effect of the precedence declarations is to assign precedence
levels to the terminal symbols declared. The second effect is to assign
precedence levels to certain rules: each rule gets its precedence from
the last terminal symbol mentioned in the components. (You can also
specify explicitly the precedence of a rule. See [Context-Dependent Precedence](#Contextual-Precedence).)

Finally, the resolution of conflicts works by comparing the precedence
of the rule being considered with that of the lookahead token. If the
token’s precedence is higher, the choice is to shift. If the rule’s
precedence is higher, the choice is to reduce. If they have equal
precedence, the choice is made based on the associativity of that
precedence level. The verbose output file made by `-v`
(see [Invoking Bison](#Invocation)) says how each conflict was
resolved.

Not all rules and not all tokens have precedence. If either the rule or
the lookahead token has no precedence, then the default is to shift.

---

Previous: [How Precedence Works](#How-Precedence), Up: [Operator Precedence](#Precedence)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.3.6 Using Precedence For Non Operators

Using properly precedence and associativity directives can help fixing
shift/reduce conflicts that do not involve arithmetic-like operators. For
instance, the “dangling `else`” problem (see [Shift/Reduce Conflicts](#Shift_002fReduce)) can be
solved elegantly in two different ways.

In the present case, the conflict is between the token `"else"` willing
to be shifted, and the rule ‘`if_stmt: "if" expr "then" stmt`’, asking
for reduction. By default, the precedence of a rule is that of its last
token, here `"then"`, so the conflict will be solved appropriately
by giving `"else"` a precedence higher than that of `"then"`, for
instance as follows:

```
%precedence "then"
%precedence "else"
```

Alternatively, you may give both tokens the same precedence, in which case
associativity is used to solve the conflict. To preserve the shift action,
use right associativity:

```
%right "then" "else"
```

Neither solution is perfect however. Since Bison does not provide, so far,
“scoped” precedence, both force you to declare the precedence
of these keywords with respect to the other operators your grammar.
Therefore, instead of being warned about new conflicts you would be unaware
of (e.g., a shift/reduce conflict due to ‘`if test then 1 else 2 + 3`’
being ambiguous: ‘`if test then 1 else (2 + 3)`’ or ‘`(if test then 1
else 2) + 3`’?), the conflict will be already “fixed”.

---

Next: [Parser States](#Parser-States), Previous: [Operator Precedence](#Precedence), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 5.4 Context-Dependent Precedence

Often the precedence of an operator depends on the context. This sounds
outlandish at first, but it is really very common. For example, a minus
sign typically has a very high precedence as a unary operator, and a
somewhat lower precedence (lower than multiplication) as a binary operator.

The Bison precedence declarations
can only be used once for a given token; so a token has
only one precedence declared in this way. For context-dependent
precedence, you need to use an additional mechanism: the `%prec`
modifier for rules.

The `%prec` modifier declares the precedence of a particular rule by
specifying a terminal symbol whose precedence should be used for that rule.
It’s not necessary for that symbol to appear otherwise in the rule. The
modifier’s syntax is:

```
%prec terminal-symbol
```

and it is written after the components of the rule. Its effect is to
assign the rule the precedence of terminal-symbol, overriding
the precedence that would be deduced for it in the ordinary way. The
altered rule precedence then affects how conflicts involving that rule
are resolved (see [Operator Precedence](#Precedence)).

Here is how `%prec` solves the problem of unary minus. First, declare
a precedence for a fictitious terminal symbol named `UMINUS`. There
are no tokens of this type, but the symbol serves to stand for its
precedence:

```
…
%left '+' '-'
%left '*'
%left UMINUS
```

Now the precedence of `UMINUS` can be used in specific rules:

```
exp:
  …
| exp '-' exp
  …
| '-' exp %prec UMINUS
```

---

Next: [Reduce/Reduce Conflicts](#Reduce_002fReduce), Previous: [Context-Dependent Precedence](#Contextual-Precedence), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 5.5 Parser States

The function `yyparse` is implemented using a finite-state machine.
The values pushed on the parser stack are not simply token kind codes; they
represent the entire sequence of terminal and nonterminal symbols at or
near the top of the stack. The current state collects all the information
about previous input which is relevant to deciding what to do next.

Each time a lookahead token is read, the current parser state together with
the kind of lookahead token are looked up in a table. This table entry can
say, “Shift the lookahead token.” In this case, it also specifies the new
parser state, which is pushed onto the top of the parser stack. Or it can
say, “Reduce using rule number n.” This means that a certain number
of tokens or groupings are taken off the top of the stack, and replaced by
one grouping. In other words, that number of states are popped from the
stack, and one new state is pushed.

There is one other alternative: the table can say that the lookahead token
is erroneous in the current state. This causes error processing to begin
(see [Error Recovery](#Error-Recovery)).

---

Next: [Mysterious Conflicts](#Mysterious-Conflicts), Previous: [Parser States](#Parser-States), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 5.6 Reduce/Reduce Conflicts

A reduce/reduce conflict occurs if there are two or more rules that apply
to the same sequence of input. This usually indicates a serious error
in the grammar.

For example, here is an erroneous attempt to define a sequence
of zero or more `word` groupings.

```
sequence:
  %empty         { printf ("empty sequence\n"); }
| maybeword
| sequence word  { printf ("added word %s\n", $2); }
;
```

```

```

```
maybeword:
  %empty    { printf ("empty maybeword\n"); }
| word      { printf ("single word %s\n", $1); }
;
```

The error is an ambiguity: as counterexample generation would demonstrate
(see [Generation of Counterexamples](#Counterexamples)), there is more than one way to parse a single
`word` into a `sequence`. It could be reduced to a
`maybeword` and then into a `sequence` via the second rule.
Alternatively, nothing-at-all could be reduced into a `sequence`
via the first rule, and this could be combined with the `word`
using the third rule for `sequence`.

There is also more than one way to reduce nothing-at-all into a
`sequence`. This can be done directly via the first rule,
or indirectly via `maybeword` and then the second rule.

You might think that this is a distinction without a difference, because it
does not change whether any particular input is valid or not. But it does
affect which actions are run. One parsing order runs the second rule’s
action; the other runs the first rule’s action and the third rule’s action.
In this example, the output of the program changes.

Bison resolves a reduce/reduce conflict by choosing to use the rule that
appears first in the grammar, but it is very risky to rely on this. Every
reduce/reduce conflict must be studied and usually eliminated. Here is the
proper way to define `sequence`:

```
sequence:
  %empty         { printf ("empty sequence\n"); }
| sequence word  { printf ("added word %s\n", $2); }
;
```

Here is another common error that yields a reduce/reduce conflict:

```
sequence:
  %empty
| sequence words
| sequence redirects
;
```

```

```

```
words:
  %empty
| words word
;
```

```

```

```
redirects:
  %empty
| redirects redirect
;
```

The intention here is to define a sequence which can contain either
`word` or `redirect` groupings. The individual definitions of
`sequence`, `words` and `redirects` are error-free, but the
three together make a subtle ambiguity: even an empty input can be parsed
in infinitely many ways!

Consider: nothing-at-all could be a `words`. Or it could be two
`words` in a row, or three, or any number. It could equally well be a
`redirects`, or two, or any number. Or it could be a `words`
followed by three `redirects` and another `words`. And so on.

Here are two ways to correct these rules. First, to make it a single level
of sequence:

```
sequence:
  %empty
| sequence word
| sequence redirect
;
```

Second, to prevent either a `words` or a `redirects`
from being empty:

```
sequence:
  %empty
| sequence words
| sequence redirects
;
```

```

```

```
words:
  word
| words word
;
```

```

```

```
redirects:
  redirect
| redirects redirect
;
```

Yet this proposal introduces another kind of ambiguity! The input
‘`word word`’ can be parsed as a single `words` composed of two
‘`word`’s, or as two one-`word` `words` (and likewise for
`redirect`/`redirects`). However this ambiguity is now a
shift/reduce conflict, and therefore it can now be addressed with precedence
directives.

To simplify the matter, we will proceed with `word` and `redirect`
being tokens: `"word"` and `"redirect"`.

To prefer the longest `words`, the conflict between the token
`"word"` and the rule ‘`sequence: sequence words`’ must be resolved
as a shift. To this end, we use the same techniques as exposed above, see
[Using Precedence For Non Operators](#Non-Operators). One solution
relies on precedences: use `%prec` to give a lower precedence to the
rule:

```
%precedence "word"
%precedence "sequence"
%%
```

```
sequence:
  %empty
| sequence word      %prec "sequence"
| sequence redirect  %prec "sequence"
;
```

```

```

```
words:
  word
| words "word"
;
```

Another solution relies on associativity: provide both the token and the
rule with the same precedence, but make them right-associative:

```
%right "word" "redirect"
%%
```

```
sequence:
  %empty
| sequence word      %prec "word"
| sequence redirect  %prec "redirect"
;
```

---

Next: [Tuning LR](#Tuning-LR), Previous: [Reduce/Reduce Conflicts](#Reduce_002fReduce), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 5.7 Mysterious Conflicts

Sometimes reduce/reduce conflicts can occur that don’t look warranted.
Here is an example:

```
%%
def: param_spec return_spec ',';
param_spec:
  type
| name_list ':' type
;
```

```

```

```
return_spec:
  type
| name ':' type
;
```

```
type: "id";
```

```
name: "id";
name_list:
  name
| name ',' name_list
;
```

It would seem that this grammar can be parsed with only a single token of
lookahead: when a `param_spec` is being read, an `"id"` is a
`name` if a comma or colon follows, or a `type` if another
`"id"` follows. In other words, this grammar is LR(1). Yet Bison
finds one reduce/reduce conflict, for which counterexample generation
(see [Generation of Counterexamples](#Counterexamples)) would find a *nonunifying* example.

This is because Bison does not handle all LR(1) grammars *by default*,
for historical reasons.
In this grammar, two contexts, that after an `"id"` at the beginning
of a `param_spec` and likewise at the beginning of a
`return_spec`, are similar enough that Bison assumes they are the
same.
They appear similar because the same set of rules would be
active—the rule for reducing to a `name` and that for reducing to
a `type`. Bison is unable to determine at that stage of processing
that the rules would require different lookahead tokens in the two
contexts, so it makes a single parser state for them both. Combining
the two contexts causes a conflict later. In parser terminology, this
occurrence means that the grammar is not LALR(1).

For many practical grammars (specifically those that fall into the non-LR(1)
class), the limitations of LALR(1) result in difficulties beyond just
mysterious reduce/reduce conflicts. The best way to fix all these problems
is to select a different parser table construction algorithm. Either
IELR(1) or canonical LR(1) would suffice, but the former is more efficient
and easier to debug during development. See [LR Table Construction](#LR-Table-Construction), for
details.

If you instead wish to work around LALR(1)’s limitations, you
can often fix a mysterious conflict by identifying the two parser states
that are being confused, and adding something to make them look
distinct. In the above example, adding one rule to
`return_spec` as follows makes the problem go away:

```
…
return_spec:
  type
| name ':' type
| "id" "bogus"       /* This rule is never used. */
;
```

This corrects the problem because it introduces the possibility of an
additional active rule in the context after the `"id"` at the beginning of
`return_spec`. This rule is not active in the corresponding context
in a `param_spec`, so the two contexts receive distinct parser states.
As long as the token `"bogus"` is never generated by `yylex`,
the added rule cannot alter the way actual input is parsed.

In this particular example, there is another way to solve the problem:
rewrite the rule for `return_spec` to use `"id"` directly
instead of via `name`. This also causes the two confusing
contexts to have different sets of active rules, because the one for
`return_spec` activates the altered rule for `return_spec`
rather than the one for `name`.

```
param_spec:
  type
| name_list ':' type
;
```

```

```

```
return_spec:
  type
| "id" ':' type
;
```

For a more detailed exposition of LALR(1) parsers and parser generators, see
[DeRemer 1982](#DeRemer-1982).

---

Next: [Generalized LR (GLR) Parsing](#Generalized-LR-Parsing), Previous: [Mysterious Conflicts](#Mysterious-Conflicts), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]
