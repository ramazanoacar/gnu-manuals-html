### 13.13 Mailing Lists

> How do I join the help-bison and bug-bison mailing lists?

See <https://lists.gnu.org/>.

---

Next: [Glossary](#Glossary), Previous: [Frequently Asked Questions](#FAQ), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## Appendix A Bison Symbols

Variable: **@$** [¶](#index-_0040_0024-1)
:   In an action, the location of the left-hand side of the rule.
    See [Tracking Locations](#Tracking-Locations).

Variable: **@n** [¶](#index-_0040n-2)

Symbol: **@n** [¶](#index-_0040n-3)
:   In an action, the location of the n-th symbol of the right-hand side
    of the rule. See [Tracking Locations](#Tracking-Locations).

    In a grammar, the Bison-generated nonterminal symbol for a midrule action
    with a semantic value. See [Midrule Action Translation](#Midrule-Action-Translation).

Variable: **@name** [¶](#index-_0040name)

Variable: **@[name]** [¶](#index-_0040_005bname_005d)
:   In an action, the location of a symbol addressed by name.
    See [Tracking Locations](#Tracking-Locations).

Symbol: **$@n** [¶](#index-_0024_0040n)
:   In a grammar, the Bison-generated nonterminal symbol for a midrule action
    with no semantics value. See [Midrule Action Translation](#Midrule-Action-Translation).

Variable: **$$** [¶](#index-_0024_0024-2)
:   In an action, the semantic value of the left-hand side of the rule.
    See [Actions](#Actions).

Variable: **$n** [¶](#index-_0024n-2)
:   In an action, the semantic value of the n-th symbol of the
    right-hand side of the rule. See [Actions](#Actions).

Variable: **$name** [¶](#index-_0024name)

Variable: **$[name]** [¶](#index-_0024_005bname_005d)
:   In an action, the semantic value of a symbol addressed by name.
    See [Actions](#Actions).

Delimiter: **%%** [¶](#index-_0025_0025-1)
:   Delimiter used to separate the grammar rule section from the
    Bison declarations section or the epilogue.
    See [The Overall Layout of a Bison Grammar](#Grammar-Layout).

Delimiter: **%{code%}** [¶](#index-_0025_007bcode_0025_007d)
:   All code listed between ‘`%{`’ and ‘`%}`’ is copied verbatim
    to the parser implementation file. Such code forms the prologue of
    the grammar file. See [Outline of a Bison Grammar](#Grammar-Outline).

Directive: **%?{expression}** [¶](#index-_0025_003f_007bexpression_007d)
:   Predicate actions. This is a type of action clause that may appear in
    rules. The expression is evaluated, and if false, causes a syntax error. In
    GLR parsers during nondeterministic operation,
    this silently causes an alternative parse to die. During deterministic
    operation, it is the same as the effect of YYERROR.
    See [Controlling a Parse with Arbitrary Predicates](#Semantic-Predicates).

Construct: **/\*** *… \*/* [¶](#index-_002f_002a)

Construct: **//** *…* [¶](#index-_002f_002f)
:   Comments, as in C/C++.

Delimiter: **:** [¶](#index-_003a)
:   Separates a rule’s result from its components. See [Grammar Rules](#Rules).

Delimiter: **;** [¶](#index-_003b)
:   Terminates a rule. See [Grammar Rules](#Rules).

Delimiter: **|** [¶](#index-_007c-1)
:   Separates alternate rules for the same result nonterminal.
    See [Grammar Rules](#Rules).

Directive: **<\*>** [¶](#index-_003c_002a_003e-2)
:   Used to define a default tagged `%destructor` or default tagged
    `%printer`.

    See [Freeing Discarded Symbols](#Destructor-Decl).

Directive: **<>** [¶](#index-_003c_003e-2)
:   Used to define a default tagless `%destructor` or default tagless
    `%printer`.

    See [Freeing Discarded Symbols](#Destructor-Decl).

Symbol: **$accept** [¶](#index-_0024accept)
:   The predefined nonterminal whose only rule is ‘`$accept: start
    $end`’, where start is the start symbol. See [The Start-Symbol](#Start-Decl). It cannot
    be used in the grammar.

Directive: **%code** *{code}* [¶](#index-_0025code-9)

Directive: **%code** *qualifier {code}* [¶](#index-_0025code-10)
:   Insert code verbatim into the output parser source at the
    default location or at the location specified by qualifier.
    See [%code Summary](#g_t_0025code-Summary).

Directive: **%debug** [¶](#index-_0025debug-2)
:   Equip the parser for debugging. See [Bison Declaration Summary](#Decl-Summary).

Directive: **%define** *variable* [¶](#index-_0025define-10)

Directive: **%define** *variable value* [¶](#index-_0025define-11)

Directive: **%define** *variable {value}* [¶](#index-_0025define-12)

Directive: **%define** *variable "value"* [¶](#index-_0025define-13)
:   Define a variable to adjust Bison’s behavior. See [%define Summary](#g_t_0025define-Summary).

Directive: **%defines** [¶](#index-_0025defines-2)

Directive: **%defines** *defines-file* [¶](#index-_0025defines-3)
:   Historical name for `%header`.
    See [Bison Declaration Summary](#Decl-Summary).

Directive: **%destructor** [¶](#index-_0025destructor-5)
:   Specify how the parser should reclaim the memory associated to
    discarded symbols. See [Freeing Discarded Symbols](#Destructor-Decl).

Directive: **%dprec** [¶](#index-_0025dprec-1)
:   Bison declaration to assign a precedence to a rule that is used at parse
    time to resolve reduce/reduce conflicts. See [Writing GLR Parsers](#GLR-Parsers).

Directive: **%empty** [¶](#index-_0025empty-1)
:   Bison declaration to declare make explicit that a rule has an empty
    right-hand side. See [Empty Rules](#Empty-Rules).

Symbol: **$end** [¶](#index-_0024end)
:   The predefined token marking the end of the token stream. It cannot be
    used in the grammar.

Symbol: **error** [¶](#index-error-1)
:   A token name reserved for error recovery. This token may be used in
    grammar rules so as to allow the Bison parser to recognize an error in
    the grammar without halting the process. In effect, a sentence
    containing an error may be recognized as valid. On a syntax error, the
    token `error` becomes the current lookahead token. Actions
    corresponding to `error` are then executed, and the lookahead
    token is reset to the token that originally caused the violation.
    See [Error Recovery](#Error-Recovery).

Directive: **%error-verbose** [¶](#index-_0025error_002dverbose)
:   An obsolete directive standing for ‘`%define parse.error verbose`’.

Directive: **%file-prefix** *"prefix"* [¶](#index-_0025file_002dprefix-1)
:   Bison declaration to set the prefix of the output files. See [Bison Declaration Summary](#Decl-Summary).

Directive: **%glr-parser** [¶](#index-_0025glr_002dparser-2)
:   Bison declaration to produce a GLR parser. See [Writing GLR Parsers](#GLR-Parsers).

Directive: **%header** [¶](#index-_0025header-2)
:   Bison declaration to create a parser header file, which is usually
    meant for the scanner. See [Bison Declaration Summary](#Decl-Summary).

Directive: **%header** *header-file* [¶](#index-_0025header-3)
:   Same as above, but save in the file header-file.
    See [Bison Declaration Summary](#Decl-Summary).

Directive: **%initial-action** [¶](#index-_0025initial_002daction-3)
:   Run user code before parsing. See [Performing Actions before Parsing](#Initial-Action-Decl).

Directive: **%language** [¶](#index-_0025language-1)
:   Specify the programming language for the generated parser.
    See [Bison Declaration Summary](#Decl-Summary).

Directive: **%left** [¶](#index-_0025left-3)
:   Bison declaration to assign precedence and left associativity to token(s).
    See [Operator Precedence](#Precedence-Decl).

Directive: **%lex-param** *{argument-declaration} …* [¶](#index-_0025lex_002dparam-3)
:   Bison declaration to specifying additional arguments that
    `yylex` should accept. See [Calling Conventions for Pure Parsers](#Pure-Calling).

Directive: **%merge** [¶](#index-_0025merge-1)
:   Bison declaration to assign a merging function to a rule. If there is a
    reduce/reduce conflict with a rule having the same merging function, the
    function is applied to the two semantic values to get a single result.
    See [Writing GLR Parsers](#GLR-Parsers).

Directive: **%name-prefix** *"prefix"* [¶](#index-_0025name_002dprefix-1)
:   Obsoleted by the `%define` variable `api.prefix` (see [Multiple Parsers in the Same Program](#Multiple-Parsers)).

    Rename the external symbols (variables and functions) used in the parser so
    that they start with prefix instead of ‘`yy`’. Contrary to
    `api.prefix`, do no rename types and macros.

    The precise list of symbols renamed in C parsers is `yyparse`,
    `yylex`, `yyerror`, `yynerrs`, `yylval`, `yychar`,
    `yydebug`, and (if locations are used) `yylloc`. If you use a
    push parser, `yypush_parse`, `yypull_parse`, `yypstate`,
    `yypstate_new` and `yypstate_delete` will also be renamed. For
    example, if you use ‘`%name-prefix "c_"`’, the names become
    `c_parse`, `c_lex`, and so on. For C++ parsers, see the
    `%define api.namespace` documentation in this section.

Directive: **%no-lines** [¶](#index-_0025no_002dlines-1)
:   Bison declaration to avoid generating `#line` directives in the
    parser implementation file. See [Bison Declaration Summary](#Decl-Summary).

Directive: **%nonassoc** [¶](#index-_0025nonassoc-4)
:   Bison declaration to assign precedence and nonassociativity to token(s).
    See [Operator Precedence](#Precedence-Decl).

Directive: **%nterm** [¶](#index-_0025nterm-4)
:   Bison declaration to declare nonterminals. See [Nonterminal Symbols](#Type-Decl).

Directive: **%output** *"file"* [¶](#index-_0025output-1)
:   Bison declaration to set the name of the parser implementation file.
    See [Bison Declaration Summary](#Decl-Summary).

Directive: **%param** *{argument-declaration} …* [¶](#index-_0025param-2)
:   Bison declaration to specify additional arguments that both
    `yylex` and `yyparse` should accept. See [The Parser Function `yyparse`](#Parser-Function).

Directive: **%parse-param** *{argument-declaration} …* [¶](#index-_0025parse_002dparam-3)
:   Bison declaration to specify additional arguments that `yyparse`
    should accept. See [The Parser Function `yyparse`](#Parser-Function).

Directive: **%prec** [¶](#index-_0025prec-1)
:   Bison declaration to assign a precedence to a specific rule.
    See [Context-Dependent Precedence](#Contextual-Precedence).

Directive: **%precedence** [¶](#index-_0025precedence-2)
:   Bison declaration to assign precedence to token(s), but no associativity
    See [Operator Precedence](#Precedence-Decl).

Directive: **%pure-parser** [¶](#index-_0025pure_002dparser-1)
:   Deprecated version of ‘`%define api.pure`’ (see [%define Summary](#g_t_0025define-Summary)), for which Bison is more careful to warn about
    unreasonable usage.

Directive: **%require** *"version"* [¶](#index-_0025require-2)
:   Require version version or higher of Bison. See [Require a Version of Bison](#Require-Decl).

Directive: **%right** [¶](#index-_0025right-2)
:   Bison declaration to assign precedence and right associativity to token(s).
    See [Operator Precedence](#Precedence-Decl).

Directive: **%skeleton** [¶](#index-_0025skeleton-1)
:   Specify the skeleton to use; usually for development.
    See [Bison Declaration Summary](#Decl-Summary).

Directive: **%start** [¶](#index-_0025start-2)
:   Bison declaration to specify the start symbol. See [The Start-Symbol](#Start-Decl).

Directive: **%token** [¶](#index-_0025token-4)
:   Bison declaration to declare token(s) without specifying precedence.
    See [Token Kind Names](#Token-Decl).

Directive: **%token-table** [¶](#index-_0025token_002dtable-1)
:   Bison declaration to include a token name table in the parser implementation
    file. See [Bison Declaration Summary](#Decl-Summary).

Directive: **%type** [¶](#index-_0025type-3)
:   Bison declaration to declare symbol value types. See [Nonterminal Symbols](#Type-Decl).

Symbol: **$undefined** [¶](#index-_0024undefined)
:   The predefined token onto which all undefined values returned by
    `yylex` are mapped. It cannot be used in the grammar, rather, use
    `error`.

Directive: **%union** [¶](#index-_0025union-3)
:   Bison declaration to specify several possible data types for semantic
    values. See [The Union Declaration](#Union-Decl).

Macro: **YYABORT** [¶](#index-YYABORT-4)
:   Macro to pretend that an unrecoverable syntax error has occurred, by making
    `yyparse` return 1 immediately. The error reporting function
    `yyerror` is not called. See [The Parser Function `yyparse`](#Parser-Function).

    For Java parsers, this functionality is invoked using `return YYABORT;`
    instead.

Macro: **YYACCEPT** [¶](#index-YYACCEPT-4)
:   Macro to pretend that a complete utterance of the language has been
    read, by making `yyparse` return 0 immediately.
    See [The Parser Function `yyparse`](#Parser-Function).

    For Java parsers, this functionality is invoked using `return YYACCEPT;`
    instead.

Macro: **YYBACKUP** [¶](#index-YYBACKUP-2)
:   Macro to discard a value from the parser stack and fake a lookahead
    token. See [Special Features for Use in Actions](#Action-Features).

Macro: **YYBISON** [¶](#index-YYBISON)
:   The version of Bison as an integer, for instance 30704 for version 3.7.4.
    Defined in `yacc.c` only. Before version 3.7.4, `YYBISON` was
    defined to 1.

Variable: **yychar** [¶](#index-yychar-1)
:   External integer variable that contains the integer value of the
    lookahead token. (In a pure parser, it is a local variable within
    `yyparse`.) Error-recovery rule actions may examine this variable.
    See [Special Features for Use in Actions](#Action-Features).

Variable: **yyclearin** [¶](#index-yyclearin-3)
:   Macro used in error-recovery rule actions. It clears the previous
    lookahead token. See [Error Recovery](#Error-Recovery).

Macro: **YYDEBUG** [¶](#index-YYDEBUG-1)
:   Macro to define to equip the parser with tracing code. See [Tracing Your Parser](#Tracing).

Variable: **yydebug** [¶](#index-yydebug-1)
:   External integer variable set to zero by default. If `yydebug`
    is given a nonzero value, the parser will output information on input
    symbols and parser action. See [Tracing Your Parser](#Tracing).

Value: **YYEMPTY** [¶](#index-YYEMPTY-1)
:   The pseudo token kind when there is no lookahead token.

Value: **YYEOF** [¶](#index-YYEOF-1)
:   The token kind denoting is the end of the input stream.

Macro: **yyerrok** [¶](#index-yyerrok-3)
:   Macro to cause parser to recover immediately to its normal mode
    after a syntax error. See [Error Recovery](#Error-Recovery).

Macro: **YYERROR** [¶](#index-YYERROR-3)
:   Cause an immediate syntax error. This statement initiates error
    recovery just as if the parser itself had detected an error; however, it
    does not call `yyerror`, and does not print any message. If you
    want to print an error message, call `yyerror` explicitly before
    the ‘`YYERROR;`’ statement. See [Error Recovery](#Error-Recovery).

    For Java parsers, this functionality is invoked using `return YYERROR;`
    instead.

Function: **yyerror** [¶](#index-yyerror-4)
:   User-supplied function to be called by `yyparse` on error.
    See [The Error Reporting Function `yyerror`](#Error-Reporting-Function).

Macro: **YYFPRINTF** [¶](#index-YYFPRINTF-1)
:   Macro used to output run-time traces in C.
    See [Enabling Traces](#Enabling-Traces).

Macro: **YYINITDEPTH** [¶](#index-YYINITDEPTH)
:   Macro for specifying the initial size of the parser stack.
    See [Memory Management, and How to Avoid Memory Exhaustion](#Memory-Management).

Function: **yylex** [¶](#index-yylex-5)
:   User-supplied lexical analyzer function, called with no arguments to get
    the next token. See [The Lexical Analyzer Function `yylex`](#Lexical).

Variable: **yylloc** [¶](#index-yylloc-1)
:   External variable in which `yylex` should place the line and column
    numbers associated with a token. (In a pure parser, it is a local
    variable within `yyparse`, and its address is passed to
    `yylex`.)
    You can ignore this variable if you don’t use the ‘`@`’ feature in the
    grammar actions.
    See [Textual Locations of Tokens](#Token-Locations).
    In semantic actions, it stores the location of the lookahead token.
    See [Actions and Locations](#Actions-and-Locations).

Type: **YYLTYPE** [¶](#index-YYLTYPE)
:   Data type of `yylloc`. By default in C, a structure with four members
    (start/end line/column). See [Data Type of Locations](#Location-Type).

Variable: **yylval** [¶](#index-yylval-1)
:   External variable in which `yylex` should place the semantic
    value associated with a token. (In a pure parser, it is a local
    variable within `yyparse`, and its address is passed to
    `yylex`.)
    See [Semantic Values of Tokens](#Token-Values).
    In semantic actions, it stores the semantic value of the lookahead token.
    See [Actions](#Actions).

Macro: **YYMAXDEPTH** [¶](#index-YYMAXDEPTH)
:   Macro for specifying the maximum size of the parser stack. See [Memory Management, and How to Avoid Memory Exhaustion](#Memory-Management).

Variable: **yynerrs** [¶](#index-yynerrs)
:   Global variable which Bison increments each time it reports a syntax error.
    (In a pure parser, it is a local variable within `yyparse`. In a
    pure push parser, it is a member of `yypstate`.)
    See [The Error Reporting Function `yyerror`](#Error-Reporting-Function).

Macro: **YYNOMEM** [¶](#index-YYNOMEM-3)
:   Macro to pretend that memory is exhausted, by making `yyparse` return 2
    immediately. The error reporting function `yyerror` is called.
    See [The Parser Function `yyparse`](#Parser-Function).

Function: **yyparse** [¶](#index-yyparse-2)
:   The parser function produced by Bison; call this function to start
    parsing. See [The Parser Function `yyparse`](#Parser-Function).

Function: **yypstate\_delete** [¶](#index-yypstate_005fdelete-2)
:   The function to delete a parser instance, produced by Bison in push mode;
    call this function to delete the memory associated with a parser.
    See [`yypstate_delete`](#yypstate_005fdelete). Does nothing when called
    with a null pointer.

Function: **yypstate\_new** [¶](#index-yypstate_005fnew-2)
:   The function to create a parser instance, produced by Bison in push mode;
    call this function to create a new parser.
    See [`yypstate_new`](#yypstate_005fnew).

Function: **yypull\_parse** [¶](#index-yypull_005fparse-1)
:   The parser function produced by Bison in push mode; call this function to
    parse the rest of the input stream.
    See [`yypull_parse`](#yypull_005fparse).

Function: **yypush\_parse** [¶](#index-yypush_005fparse-2)
:   The parser function produced by Bison in push mode; call this function to
    parse a single token.
    See [`yypush_parse`](#yypush_005fparse).

Macro: **YYRECOVERING** [¶](#index-YYRECOVERING-2)
:   The expression `YYRECOVERING ()` yields 1 when the parser
    is recovering from a syntax error, and 0 otherwise.
    See [Special Features for Use in Actions](#Action-Features).

Macro: **YYSTACK\_USE\_ALLOCA** [¶](#index-YYSTACK_005fUSE_005fALLOCA)
:   Macro used to control the use of `alloca` when the
    deterministic parser in C needs to extend its stacks. If defined to 0,
    the parser will use `malloc` to extend its stacks and memory exhaustion
    occurs if `malloc` fails (see [Memory Management, and How to Avoid Memory Exhaustion](#Memory-Management)). If defined to
    1, the parser will use `alloca`. Values other than 0 and 1 are
    reserved for future Bison extensions. If not defined,
    `YYSTACK_USE_ALLOCA` defaults to 0.

    In the all-too-common case where your code may run on a host with a
    limited stack and with unreliable stack-overflow checking, you should
    set `YYMAXDEPTH` to a value that cannot possibly result in
    unchecked stack overflow on any of your target hosts when
    `alloca` is called. You can inspect the code that Bison
    generates in order to determine the proper numeric values. This will
    require some expertise in low-level implementation details.

Type: **YYSTYPE** [¶](#index-YYSTYPE)
:   In C, data type of semantic values; `int` by default.
    Deprecated in favor of the `%define` variable `api.value.type`.
    See [Data Types of Semantic Values](#Value-Type).

Type: **yysymbol\_kind\_t** [¶](#index-yysymbol_005fkind_005ft-1)
:   An enum of all the symbols, tokens and nonterminals, of the grammar.
    See [The Syntax Error Reporting Function `yyreport_syntax_error`](#Syntax-Error-Reporting-Function). The symbol kinds are used
    internally by the parser, and should not be confused with the token kinds:
    the symbol kind of a terminal symbol is not equal to its token kind! (Unless
    ‘`%define api.token.raw`’ was used.)

Type: **yytoken\_kind\_t** [¶](#index-yytoken_005fkind_005ft)
:   An enum of all the *token kinds* declared with `%token`
    (see [Token Kind Names](#Token-Decl)). These are the return values for `yylex`. They
    should not be confused with the *symbol kinds*, used internally by the
    parser.

Value: **YYUNDEF** [¶](#index-YYUNDEF)
:   The token kind denoting an unknown token.

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [Bison Symbols](#Table-of-Symbols), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## Appendix B Glossary

Accepting state
:   A state whose only action is the accept action.
    The accepting state is thus a consistent state.
    See [Understanding Your Parser](#Understanding).

Backus-Naur Form (BNF; also called “Backus Normal Form”)
:   Formal method of specifying context-free grammars originally proposed
    by John Backus, and slightly improved by Peter Naur in his 1960-01-02
    committee document contributing to what became the Algol 60 report.
    See [Languages and Context-Free Grammars](#Language-and-Grammar).

Consistent state
:   A state containing only one possible action. See [Default Reductions](#Default-Reductions).

Context-free grammars
:   Grammars specified as rules that can be applied regardless of context.
    Thus, if there is a rule which says that an integer can be used as an
    expression, integers are allowed *anywhere* an expression is
    permitted. See [Languages and Context-Free Grammars](#Language-and-Grammar).

Counterexample
:   A sequence of tokens and/or nonterminals, with one dot, that demonstrates a
    conflict. The dot marks the place where the conflict occurs.

    A *unifying* counterexample is a single string that has two different
    parses; its existence proves that the grammar is ambiguous. When a unifying
    counterexample cannot be found in reasonable time, a *nonunifying*
    counterexample is built: *two* different string sharing the prefix up
    to the dot.

    See [Generation of Counterexamples](#Counterexamples)

Default reduction
:   The reduction that a parser should perform if the current parser state
    contains no other action for the lookahead token. In permitted parser
    states, Bison declares the reduction with the largest lookahead set to be
    the default reduction and removes that lookahead set. See [Default Reductions](#Default-Reductions).

Defaulted state
:   A consistent state with a default reduction. See [Default Reductions](#Default-Reductions).

Dynamic allocation
:   Allocation of memory that occurs during execution, rather than at
    compile time or on entry to a function.

Empty string
:   Analogous to the empty set in set theory, the empty string is a
    character string of length zero.

Finite-state stack machine
:   A “machine” that has discrete states in which it is said to exist at
    each instant in time. As input to the machine is processed, the
    machine moves from state to state as specified by the logic of the
    machine. In the case of the parser, the input is the language being
    parsed, and the states correspond to various stages in the grammar
    rules. See [The Bison Parser Algorithm](#Algorithm).

Generalized LR (GLR)
:   A parsing algorithm that can handle all context-free grammars, including those
    that are not LR(1). It resolves situations that Bison’s
    deterministic parsing
    algorithm cannot by effectively splitting off multiple parsers, trying all
    possible parsers, and discarding those that fail in the light of additional
    right context. See [Generalized LR (GLR) Parsing](#Generalized-LR-Parsing).

Grouping
:   A language construct that is (in general) grammatically divisible;
    for example, ‘expression’ or ‘declaration’ in C.
    See [Languages and Context-Free Grammars](#Language-and-Grammar).

IELR(1) (Inadequacy Elimination LR(1))
:   A minimal LR(1) parser table construction algorithm. That is, given any
    context-free grammar, IELR(1) generates parser tables with the full
    language-recognition power of canonical LR(1) but with nearly the same
    number of parser states as LALR(1). This reduction in parser states is
    often an order of magnitude. More importantly, because canonical LR(1)’s
    extra parser states may contain duplicate conflicts in the case of non-LR(1)
    grammars, the number of conflicts for IELR(1) is often an order of magnitude
    less as well. This can significantly reduce the complexity of developing a
    grammar. See [LR Table Construction](#LR-Table-Construction).

Infix operator
:   An arithmetic operator that is placed between the operands on which it
    performs some operation.

Input stream
:   A continuous flow of data between devices or programs.

Kind
:   “Token” and “symbol” are each overloaded to mean either a grammar symbol
    (kind) or all parse info (kind, value, location) associated with occurrences
    of that grammar symbol from the input. To disambiguate,

    * we use “token kind” and “symbol kind” to mean both grammar symbols and
      the values that represent them in a base programming language (C, C++,
      etc.). The names of the types of these values are typically
      `token_kind_t`, or `token_kind_type`, or `TokenKind`,
      depending on the programming language.
    * we use “token” and “symbol” without the word “kind” to mean parsed
      occurrences, and we append the word “type” to refer to the types that
      represent them in a base programming language.

    In summary: When you see “kind”, interpret “symbol” or “token” to mean
    a *grammar symbol*. When you don’t see “kind” (including when you
    see “type”), interpret “symbol” or “token” to mean a *parsed
    symbol*.

LAC (Lookahead Correction)
:   A parsing mechanism that fixes the problem of delayed syntax error
    detection, which is caused by LR state merging, default reductions, and the
    use of `%nonassoc`. Delayed syntax error detection results in
    unexpected semantic actions, initiation of error recovery in the wrong
    syntactic context, and an incorrect list of expected tokens in a verbose
    syntax error message. See [LAC](#LAC).

Language construct
:   One of the typical usage schemas of the language. For example, one of
    the constructs of the C language is the `if` statement.
    See [Languages and Context-Free Grammars](#Language-and-Grammar).

Left associativity
:   Operators having left associativity are analyzed from left to right:
    ‘`a+b+c`’ first computes ‘`a+b`’ and then combines with
    ‘`c`’. See [Operator Precedence](#Precedence).

Left recursion
:   A rule whose result symbol is also its first component symbol; for
    example, ‘`expseq1 : expseq1 ',' exp;`’. See [Recursive Rules](#Recursion).

Left-to-right parsing
:   Parsing a sentence of a language by analyzing it token by token from
    left to right. See [The Bison Parser Algorithm](#Algorithm).

Lexical analyzer (scanner)
:   A function that reads an input stream and returns tokens one by one.
    See [The Lexical Analyzer Function `yylex`](#Lexical).

Lexical tie-in
:   A flag, set by actions in the grammar rules, which alters the way
    tokens are parsed. See [Lexical Tie-ins](#Lexical-Tie_002dins).

Literal string token
:   A token which consists of two or more fixed characters. See [Symbols, Terminal and Nonterminal](#Symbols).

Lookahead token
:   A token already read but not yet shifted. See [Lookahead Tokens](#Lookahead).

LALR(1)
:   The class of context-free grammars that Bison (like most other parser
    generators) can handle by default; a subset of LR(1).
    See [Mysterious Conflicts](#Mysterious-Conflicts).

LR(1)
:   The class of context-free grammars in which at most one token of
    lookahead is needed to disambiguate the parsing of any piece of input.

Nonterminal symbol
:   A grammar symbol standing for a grammatical construct that can
    be expressed through rules in terms of smaller constructs; in other
    words, a construct that is not a token. See [Symbols, Terminal and Nonterminal](#Symbols).

Parser
:   A function that recognizes valid sentences of a language by analyzing
    the syntax structure of a set of tokens passed to it from a lexical
    analyzer.

Postfix operator
:   An arithmetic operator that is placed after the operands upon which it
    performs some operation.

Reduction
:   Replacing a string of nonterminals and/or terminals with a single
    nonterminal, according to a grammar rule. See [The Bison Parser Algorithm](#Algorithm).

Reentrant
:   A reentrant subprogram is a subprogram which can be in invoked any
    number of times in parallel, without interference between the various
    invocations. See [A Pure (Reentrant) Parser](#Pure-Decl).

Reverse Polish Notation
:   A language in which all operators are postfix operators.

Right recursion
:   A rule whose result symbol is also its last component symbol; for
    example, ‘`expseq1: exp ',' expseq1;`’. See [Recursive Rules](#Recursion).

Semantics
:   In computer languages, the semantics are specified by the actions
    taken for each instance of the language, i.e., the meaning of
    each statement. See [Defining Language Semantics](#Semantics).

Shift
:   A parser is said to shift when it makes the choice of analyzing
    further input from the stream rather than reducing immediately some
    already-recognized rule. See [The Bison Parser Algorithm](#Algorithm).

Single-character literal
:   A single character that is recognized and interpreted as is.
    See [From Formal Rules to Bison Input](#Grammar-in-Bison).

Start symbol
:   The nonterminal symbol that stands for a complete valid utterance in
    the language being parsed. The start symbol is usually listed as the
    first nonterminal symbol in a language specification.
    See [The Start-Symbol](#Start-Decl).

Symbol kind
:   A (finite) enumeration of the grammar symbols, as processed by the parser.
    See [Symbols, Terminal and Nonterminal](#Symbols).

Symbol table
:   A data structure where symbol names and associated data are stored during
    parsing to allow for recognition and use of existing information in repeated
    uses of a symbol. See [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc).

Syntax error
:   An error encountered during parsing of an input stream due to invalid
    syntax. See [Error Recovery](#Error-Recovery).

Terminal symbol
:   A grammar symbol that has no rules in the grammar and therefore is
    grammatically indivisible. The piece of text it represents is a token.
    See [Languages and Context-Free Grammars](#Language-and-Grammar).

Token
:   A basic, grammatically indivisible unit of a language. The symbol that
    describes a token in the grammar is a terminal symbol. The input of the
    Bison parser is a stream of tokens which comes from the lexical analyzer.
    See [Symbols, Terminal and Nonterminal](#Symbols).

Token kind
:   A (finite) enumeration of the grammar terminals, as discriminated by the
    scanner. See [Symbols, Terminal and Nonterminal](#Symbols).

Unreachable state
:   A parser state to which there does not exist a sequence of transitions from
    the parser’s start state. A state can become unreachable during conflict
    resolution. See [Unreachable States](#Unreachable-States).

---

Next: [Bibliography](#Bibliography), Previous: [Glossary](#Glossary), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## Appendix C GNU Free Documentation License

Version 1.3, 3 November 2008

```
Copyright © 2000, 2001, 2002, 2007, 2008 Free Software Foundation, Inc.
https://fsf.org/

Everyone is permitted to copy and distribute verbatim copies
of this license document, but changing it is not allowed.
```

0. PREAMBLE

   The purpose of this License is to make a manual, textbook, or other
   functional and useful document *free* in the sense of freedom: to
   assure everyone the effective freedom to copy and redistribute it,
   with or without modifying it, either commercially or noncommercially.
   Secondarily, this License preserves for the author and publisher a way
   to get credit for their work, while not being considered responsible
   for modifications made by others.

   This License is a kind of “copyleft”, which means that derivative
   works of the document must themselves be free in the same sense. It
   complements the GNU General Public License, which is a copyleft
   license designed for free software.

   We have designed this License in order to use it for manuals for free
   software, because free software needs free documentation: a free
   program should come with manuals providing the same freedoms that the
   software does. But this License is not limited to software manuals;
   it can be used for any textual work, regardless of subject matter or
   whether it is published as a printed book. We recommend this License
   principally for works whose purpose is instruction or reference.
1. APPLICABILITY AND DEFINITIONS

   This License applies to any manual or other work, in any medium, that
   contains a notice placed by the copyright holder saying it can be
   distributed under the terms of this License. Such a notice grants a
   world-wide, royalty-free license, unlimited in duration, to use that
   work under the conditions stated herein. The “Document”, below,
   refers to any such manual or work. Any member of the public is a
   licensee, and is addressed as “you”. You accept the license if you
   copy, modify or distribute the work in a way requiring permission
   under copyright law.

   A “Modified Version” of the Document means any work containing the
   Document or a portion of it, either copied verbatim, or with
   modifications and/or translated into another language.

   A “Secondary Section” is a named appendix or a front-matter section
   of the Document that deals exclusively with the relationship of the
   publishers or authors of the Document to the Document’s overall
   subject (or to related matters) and contains nothing that could fall
   directly within that overall subject. (Thus, if the Document is in
   part a textbook of mathematics, a Secondary Section may not explain
   any mathematics.) The relationship could be a matter of historical
   connection with the subject or with related matters, or of legal,
   commercial, philosophical, ethical or political position regarding
   them.

   The “Invariant Sections” are certain Secondary Sections whose titles
   are designated, as being those of Invariant Sections, in the notice
   that says that the Document is released under this License. If a
   section does not fit the above definition of Secondary then it is not
   allowed to be designated as Invariant. The Document may contain zero
   Invariant Sections. If the Document does not identify any Invariant
   Sections then there are none.

   The “Cover Texts” are certain short passages of text that are listed,
   as Front-Cover Texts or Back-Cover Texts, in the notice that says that
   the Document is released under this License. A Front-Cover Text may
   be at most 5 words, and a Back-Cover Text may be at most 25 words.

   A “Transparent” copy of the Document means a machine-readable copy,
   represented in a format whose specification is available to the
   general public, that is suitable for revising the document
   straightforwardly with generic text editors or (for images composed of
   pixels) generic paint programs or (for drawings) some widely available
   drawing editor, and that is suitable for input to text formatters or
   for automatic translation to a variety of formats suitable for input
   to text formatters. A copy made in an otherwise Transparent file
   format whose markup, or absence of markup, has been arranged to thwart
   or discourage subsequent modification by readers is not Transparent.
   An image format is not Transparent if used for any substantial amount
   of text. A copy that is not “Transparent” is called “Opaque”.

   Examples of suitable formats for Transparent copies include plain
   ASCII without markup, Texinfo input format, LaTeX input
   format, SGML or XML using a publicly available
   DTD, and standard-conforming simple HTML,
   PostScript or PDF designed for human modification. Examples
   of transparent image formats include PNG, XCF and
   JPG. Opaque formats include proprietary formats that can be
   read and edited only by proprietary word processors, SGML or
   XML for which the DTD and/or processing tools are
   not generally available, and the machine-generated HTML,
   PostScript or PDF produced by some word processors for
   output purposes only.

   The “Title Page” means, for a printed book, the title page itself,
   plus such following pages as are needed to hold, legibly, the material
   this License requires to appear in the title page. For works in
   formats which do not have any title page as such, “Title Page” means
   the text near the most prominent appearance of the work’s title,
   preceding the beginning of the body of the text.

   The “publisher” means any person or entity that distributes copies
   of the Document to the public.

   A section “Entitled XYZ” means a named subunit of the Document whose
   title either is precisely XYZ or contains XYZ in parentheses following
   text that translates XYZ in another language. (Here XYZ stands for a
   specific section name mentioned below, such as “Acknowledgements”,
   “Dedications”, “Endorsements”, or “History”.) To “Preserve the Title”
   of such a section when you modify the Document means that it remains a
   section “Entitled XYZ” according to this definition.

   The Document may include Warranty Disclaimers next to the notice which
   states that this License applies to the Document. These Warranty
   Disclaimers are considered to be included by reference in this
   License, but only as regards disclaiming warranties: any other
   implication that these Warranty Disclaimers may have is void and has
   no effect on the meaning of this License.
2. VERBATIM COPYING

   You may copy and distribute the Document in any medium, either
   commercially or noncommercially, provided that this License, the
   copyright notices, and the license notice saying this License applies
   to the Document are reproduced in all copies, and that you add no other
   conditions whatsoever to those of this License. You may not use
   technical measures to obstruct or control the reading or further
   copying of the copies you make or distribute. However, you may accept
   compensation in exchange for copies. If you distribute a large enough
   number of copies you must also follow the conditions in section 3.

   You may also lend copies, under the same conditions stated above, and
   you may publicly display copies.
3. COPYING IN QUANTITY

   If you publish printed copies (or copies in media that commonly have
   printed covers) of the Document, numbering more than 100, and the
   Document’s license notice requires Cover Texts, you must enclose the
   copies in covers that carry, clearly and legibly, all these Cover
   Texts: Front-Cover Texts on the front cover, and Back-Cover Texts on
   the back cover. Both covers must also clearly and legibly identify
   you as the publisher of these copies. The front cover must present
   the full title with all words of the title equally prominent and
   visible. You may add other material on the covers in addition.
   Copying with changes limited to the covers, as long as they preserve
   the title of the Document and satisfy these conditions, can be treated
   as verbatim copying in other respects.

   If the required texts for either cover are too voluminous to fit
   legibly, you should put the first ones listed (as many as fit
   reasonably) on the actual cover, and continue the rest onto adjacent
   pages.

   If you publish or distribute Opaque copies of the Document numbering
   more than 100, you must either include a machine-readable Transparent
   copy along with each Opaque copy, or state in or with each Opaque copy
   a computer-network location from which the general network-using
   public has access to download using public-standard network protocols
   a complete Transparent copy of the Document, free of added material.
   If you use the latter option, you must take reasonably prudent steps,
   when you begin distribution of Opaque copies in quantity, to ensure
   that this Transparent copy will remain thus accessible at the stated
   location until at least one year after the last time you distribute an
   Opaque copy (directly or through your agents or retailers) of that
   edition to the public.

   It is requested, but not required, that you contact the authors of the
   Document well before redistributing any large number of copies, to give
   them a chance to provide you with an updated version of the Document.
4. MODIFICATIONS

   You may copy and distribute a Modified Version of the Document under
   the conditions of sections 2 and 3 above, provided that you release
   the Modified Version under precisely this License, with the Modified
   Version filling the role of the Document, thus licensing distribution
   and modification of the Modified Version to whoever possesses a copy
   of it. In addition, you must do these things in the Modified Version:

   1. Use in the Title Page (and on the covers, if any) a title distinct
      from that of the Document, and from those of previous versions
      (which should, if there were any, be listed in the History section
      of the Document). You may use the same title as a previous version
      if the original publisher of that version gives permission.
   2. List on the Title Page, as authors, one or more persons or entities
      responsible for authorship of the modifications in the Modified
      Version, together with at least five of the principal authors of the
      Document (all of its principal authors, if it has fewer than five),
      unless they release you from this requirement.
   3. State on the Title page the name of the publisher of the
      Modified Version, as the publisher.
   4. Preserve all the copyright notices of the Document.
   5. Add an appropriate copyright notice for your modifications
      adjacent to the other copyright notices.
   6. Include, immediately after the copyright notices, a license notice
      giving the public permission to use the Modified Version under the
      terms of this License, in the form shown in the Addendum below.
   7. Preserve in that license notice the full lists of Invariant Sections
      and required Cover Texts given in the Document’s license notice.
   8. Include an unaltered copy of this License.
   9. Preserve the section Entitled “History”, Preserve its Title, and add
      to it an item stating at least the title, year, new authors, and
      publisher of the Modified Version as given on the Title Page. If
      there is no section Entitled “History” in the Document, create one
      stating the title, year, authors, and publisher of the Document as
      given on its Title Page, then add an item describing the Modified
      Version as stated in the previous sentence.
   10. Preserve the network location, if any, given in the Document for
       public access to a Transparent copy of the Document, and likewise
       the network locations given in the Document for previous versions
       it was based on. These may be placed in the “History” section.
       You may omit a network location for a work that was published at
       least four years before the Document itself, or if the original
       publisher of the version it refers to gives permission.
   11. For any section Entitled “Acknowledgements” or “Dedications”, Preserve
       the Title of the section, and preserve in the section all the
       substance and tone of each of the contributor acknowledgements and/or
       dedications given therein.
   12. Preserve all the Invariant Sections of the Document,
       unaltered in their text and in their titles. Section numbers
       or the equivalent are not considered part of the section titles.
   13. Delete any section Entitled “Endorsements”. Such a section
       may not be included in the Modified Version.
   14. Do not retitle any existing section to be Entitled “Endorsements” or
       to conflict in title with any Invariant Section.
   15. Preserve any Warranty Disclaimers.

   If the Modified Version includes new front-matter sections or
   appendices that qualify as Secondary Sections and contain no material
   copied from the Document, you may at your option designate some or all
   of these sections as invariant. To do this, add their titles to the
   list of Invariant Sections in the Modified Version’s license notice.
   These titles must be distinct from any other section titles.

   You may add a section Entitled “Endorsements”, provided it contains
   nothing but endorsements of your Modified Version by various
   parties—for example, statements of peer review or that the text has
   been approved by an organization as the authoritative definition of a
   standard.

   You may add a passage of up to five words as a Front-Cover Text, and a
   passage of up to 25 words as a Back-Cover Text, to the end of the list
   of Cover Texts in the Modified Version. Only one passage of
   Front-Cover Text and one of Back-Cover Text may be added by (or
   through arrangements made by) any one entity. If the Document already
   includes a cover text for the same cover, previously added by you or
   by arrangement made by the same entity you are acting on behalf of,
   you may not add another; but you may replace the old one, on explicit
   permission from the previous publisher that added the old one.

   The author(s) and publisher(s) of the Document do not by this License
   give permission to use their names for publicity for or to assert or
   imply endorsement of any Modified Version.
5. COMBINING DOCUMENTS

   You may combine the Document with other documents released under this
   License, under the terms defined in section 4 above for modified
   versions, provided that you include in the combination all of the
   Invariant Sections of all of the original documents, unmodified, and
   list them all as Invariant Sections of your combined work in its
   license notice, and that you preserve all their Warranty Disclaimers.

   The combined work need only contain one copy of this License, and
   multiple identical Invariant Sections may be replaced with a single
   copy. If there are multiple Invariant Sections with the same name but
   different contents, make the title of each such section unique by
   adding at the end of it, in parentheses, the name of the original
   author or publisher of that section if known, or else a unique number.
   Make the same adjustment to the section titles in the list of
   Invariant Sections in the license notice of the combined work.

   In the combination, you must combine any sections Entitled “History”
   in the various original documents, forming one section Entitled
   “History”; likewise combine any sections Entitled “Acknowledgements”,
   and any sections Entitled “Dedications”. You must delete all
   sections Entitled “Endorsements.”
6. COLLECTIONS OF DOCUMENTS

   You may make a collection consisting of the Document and other documents
   released under this License, and replace the individual copies of this
   License in the various documents with a single copy that is included in
   the collection, provided that you follow the rules of this License for
   verbatim copying of each of the documents in all other respects.

   You may extract a single document from such a collection, and distribute
   it individually under this License, provided you insert a copy of this
   License into the extracted document, and follow this License in all
   other respects regarding verbatim copying of that document.
7. AGGREGATION WITH INDEPENDENT WORKS

   A compilation of the Document or its derivatives with other separate
   and independent documents or works, in or on a volume of a storage or
   distribution medium, is called an “aggregate” if the copyright
   resulting from the compilation is not used to limit the legal rights
   of the compilation’s users beyond what the individual works permit.
   When the Document is included in an aggregate, this License does not
   apply to the other works in the aggregate which are not themselves
   derivative works of the Document.

   If the Cover Text requirement of section 3 is applicable to these
   copies of the Document, then if the Document is less than one half of
   the entire aggregate, the Document’s Cover Texts may be placed on
   covers that bracket the Document within the aggregate, or the
   electronic equivalent of covers if the Document is in electronic form.
   Otherwise they must appear on printed covers that bracket the whole
   aggregate.
8. TRANSLATION

   Translation is considered a kind of modification, so you may
   distribute translations of the Document under the terms of section 4.
   Replacing Invariant Sections with translations requires special
   permission from their copyright holders, but you may include
   translations of some or all Invariant Sections in addition to the
   original versions of these Invariant Sections. You may include a
   translation of this License, and all the license notices in the
   Document, and any Warranty Disclaimers, provided that you also include
   the original English version of this License and the original versions
   of those notices and disclaimers. In case of a disagreement between
   the translation and the original version of this License or a notice
   or disclaimer, the original version will prevail.

   If a section in the Document is Entitled “Acknowledgements”,
   “Dedications”, or “History”, the requirement (section 4) to Preserve
   its Title (section 1) will typically require changing the actual
   title.
9. TERMINATION

   You may not copy, modify, sublicense, or distribute the Document
   except as expressly provided under this License. Any attempt
   otherwise to copy, modify, sublicense, or distribute it is void, and
   will automatically terminate your rights under this License.

   However, if you cease all violation of this License, then your license
   from a particular copyright holder is reinstated (a) provisionally,
   unless and until the copyright holder explicitly and finally
   terminates your license, and (b) permanently, if the copyright holder
   fails to notify you of the violation by some reasonable means prior to
   60 days after the cessation.

   Moreover, your license from a particular copyright holder is
   reinstated permanently if the copyright holder notifies you of the
   violation by some reasonable means, this is the first time you have
   received notice of violation of this License (for any work) from that
   copyright holder, and you cure the violation prior to 30 days after
   your receipt of the notice.

   Termination of your rights under this section does not terminate the
   licenses of parties who have received copies or rights from you under
   this License. If your rights have been terminated and not permanently
   reinstated, receipt of a copy of some or all of the same material does
   not give you any rights to use it.
10. FUTURE REVISIONS OF THIS LICENSE

    The Free Software Foundation may publish new, revised versions
    of the GNU Free Documentation License from time to time. Such new
    versions will be similar in spirit to the present version, but may
    differ in detail to address new problems or concerns. See
    <https://www.gnu.org/licenses/>.

    Each version of the License is given a distinguishing version number.
    If the Document specifies that a particular numbered version of this
    License “or any later version” applies to it, you have the option of
    following the terms and conditions either of that specified version or
    of any later version that has been published (not as a draft) by the
    Free Software Foundation. If the Document does not specify a version
    number of this License, you may choose any version ever published (not
    as a draft) by the Free Software Foundation. If the Document
    specifies that a proxy can decide which future versions of this
    License can be used, that proxy’s public statement of acceptance of a
    version permanently authorizes you to choose that version for the
    Document.
11. RELICENSING

    “Massive Multiauthor Collaboration Site” (or “MMC Site”) means any
    World Wide Web server that publishes copyrightable works and also
    provides prominent facilities for anybody to edit those works. A
    public wiki that anybody can edit is an example of such a server. A
    “Massive Multiauthor Collaboration” (or “MMC”) contained in the
    site means any set of copyrightable works thus published on the MMC
    site.

    “CC-BY-SA” means the Creative Commons Attribution-Share Alike 3.0
    license published by Creative Commons Corporation, a not-for-profit
    corporation with a principal place of business in San Francisco,
    California, as well as future copyleft versions of that license
    published by that same organization.

    “Incorporate” means to publish or republish a Document, in whole or
    in part, as part of another Document.

    An MMC is “eligible for relicensing” if it is licensed under this
    License, and if all works that were first published under this License
    somewhere other than this MMC, and subsequently incorporated in whole
    or in part into the MMC, (1) had no cover texts or invariant sections,
    and (2) were thus incorporated prior to November 1, 2008.

    The operator of an MMC Site may republish an MMC contained in the site
    under CC-BY-SA on the same site at any time before August 1, 2009,
    provided the MMC is eligible for relicensing.
