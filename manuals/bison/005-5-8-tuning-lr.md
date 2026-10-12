### 5.8 Tuning LR

The default behavior of Bison’s LR-based parsers is chosen mostly for
historical reasons, but that behavior is often not robust. For example, in
the previous section, we discussed the mysterious conflicts that can be
produced by LALR(1), Bison’s default parser table construction algorithm.
Another example is Bison’s `%define parse.error verbose` directive,
which instructs the generated parser to produce verbose syntax error
messages, which can sometimes contain incorrect information.

In this section, we explore several modern features of Bison that allow you
to tune fundamental aspects of the generated LR-based parsers. Some of
these features easily eliminate shortcomings like those mentioned above.
Others can be helpful purely for understanding your parser.

* [LR Table Construction](#LR-Table-Construction)
* [Default Reductions](#Default-Reductions)
* [LAC](#LAC)
* [Unreachable States](#Unreachable-States)

---

Next: [Default Reductions](#Default-Reductions), Up: [Tuning LR](#Tuning-LR)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.8.1 LR Table Construction

For historical reasons, Bison constructs LALR(1) parser tables by default.
However, LALR does not possess the full language-recognition power of LR.
As a result, the behavior of parsers employing LALR parser tables is often
mysterious. We presented a simple example of this effect in [Mysterious Conflicts](#Mysterious-Conflicts).

As we also demonstrated in that example, the traditional approach to
eliminating such mysterious behavior is to restructure the grammar.
Unfortunately, doing so correctly is often difficult. Moreover, merely
discovering that LALR causes mysterious behavior in your parser can be
difficult as well.

Fortunately, Bison provides an easy way to eliminate the possibility of such
mysterious behavior altogether. You simply need to activate a more powerful
parser table construction algorithm by using the `%define lr.type`
directive.

Directive: **%define lr.type** *type* [¶](#index-_0025define-lr_002etype-2)
:   Specify the type of parser tables within the LR(1) family. The accepted
    values for type are:

    * `lalr` (default)
    * `ielr`
    * `canonical-lr`

For example, to activate IELR, you might add the following directive to you
grammar file:

```
%define lr.type ielr
```

For the example in [Mysterious Conflicts](#Mysterious-Conflicts), the mysterious
conflict is then eliminated, so there is no need to invest time in
comprehending the conflict or restructuring the grammar to fix it. If,
during future development, the grammar evolves such that all mysterious
behavior would have disappeared using just LALR, you need not fear that
continuing to use IELR will result in unnecessarily large parser tables.
That is, IELR generates LALR tables when LALR (using a deterministic parsing
algorithm) is sufficient to support the full language-recognition power of
LR. Thus, by enabling IELR at the start of grammar development, you can
safely and completely eliminate the need to consider LALR’s shortcomings.

While IELR is almost always preferable, there are circumstances where LALR
or the canonical LR parser tables described by Knuth (see [Knuth 1965](#Knuth-1965)) can
be useful. Here we summarize the relative advantages of each parser table
construction algorithm within Bison:

* LALR

  There are at least two scenarios where LALR can be worthwhile:

  + GLR without static conflict resolution.

    When employing GLR parsers (see [Writing GLR Parsers](#GLR-Parsers)), if you do not resolve any
    conflicts statically (for example, with `%left` or `%precedence`),
    then
    the parser explores all potential parses of any given input. In this case,
    the choice of parser table construction algorithm is guaranteed not to alter
    the language accepted by the parser. LALR parser tables are the smallest
    parser tables Bison can currently construct, so they may then be preferable.
    Nevertheless, once you begin to resolve conflicts statically, GLR behaves
    more like a deterministic parser in the syntactic contexts where those
    conflicts appear, and so either IELR or canonical LR can then be helpful to
    avoid LALR’s mysterious behavior.
  + Malformed grammars.

    Occasionally during development, an especially malformed grammar with a
    major recurring flaw may severely impede the IELR or canonical LR parser
    table construction algorithm. LALR can be a quick way to construct parser
    tables in order to investigate such problems while ignoring the more subtle
    differences from IELR and canonical LR.
* IELR

  IELR (Inadequacy Elimination LR) is a minimal LR algorithm. That is, given
  any grammar (LR or non-LR), parsers using IELR or canonical LR parser tables
  always accept exactly the same set of sentences. However, like LALR, IELR
  merges parser states during parser table construction so that the number of
  parser states is often an order of magnitude less than for canonical LR.
  More importantly, because canonical LR’s extra parser states may contain
  duplicate conflicts in the case of non-LR grammars, the number of conflicts
  for IELR is often an order of magnitude less as well. This effect can
  significantly reduce the complexity of developing a grammar.
* Canonical LR

  While inefficient, canonical LR parser tables can be an interesting means to
  explore a grammar because they possess a property that IELR and LALR tables
  do not. That is, if `%nonassoc` is not used and default reductions are
  left disabled (see [Default Reductions](#Default-Reductions)), then, for every left context of
  every canonical LR state, the set of tokens accepted by that state is
  guaranteed to be the exact set of tokens that is syntactically acceptable in
  that left context. It might then seem that an advantage of canonical LR
  parsers in production is that, under the above constraints, they are
  guaranteed to detect a syntax error as soon as possible without performing
  any unnecessary reductions. However, IELR parsers that use LAC are also
  able to achieve this behavior without sacrificing `%nonassoc` or
  default reductions. For details and a few caveats of LAC, see [LAC](#LAC).

For a more detailed exposition of the mysterious behavior in LALR parsers
and the benefits of IELR, see [Denny 2008](#Denny-2008), and [Denny 2010 November](#Denny-2010-November).

---

Next: [LAC](#LAC), Previous: [LR Table Construction](#LR-Table-Construction), Up: [Tuning LR](#Tuning-LR)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.8.2 Default Reductions

After parser table construction, Bison identifies the reduction with the
largest lookahead set in each parser state. To reduce the size of the
parser state, traditional Bison behavior is to remove that lookahead set and
to assign that reduction to be the default parser action. Such a reduction
is known as a *default reduction*.

Default reductions affect more than the size of the parser tables. They
also affect the behavior of the parser:

* Delayed `yylex` invocations.

  A *consistent state* is a state that has only one possible parser
  action. If that action is a reduction and is encoded as a default
  reduction, then that consistent state is called a *defaulted state*.
  Upon reaching a defaulted state, a Bison-generated parser does not bother to
  invoke `yylex` to fetch the next token before performing the reduction.
  In other words, whether default reductions are enabled in consistent states
  determines how soon a Bison-generated parser invokes `yylex` for a
  token: immediately when it *reaches* that token in the input or when it
  eventually *needs* that token as a lookahead to determine the next
  parser action. Traditionally, default reductions are enabled, and so the
  parser exhibits the latter behavior.

  The presence of defaulted states is an important consideration when
  designing `yylex` and the grammar file. That is, if the behavior of
  `yylex` can influence or be influenced by the semantic actions
  associated with the reductions in defaulted states, then the delay of the
  next `yylex` invocation until after those reductions is significant.
  For example, the semantic actions might pop a scope stack that `yylex`
  uses to determine what token to return. Thus, the delay might be necessary
  to ensure that `yylex` does not look up the next token in a scope that
  should already be considered closed.
* Delayed syntax error detection.

  When the parser fetches a new token by invoking `yylex`, it checks
  whether there is an action for that token in the current parser state. The
  parser detects a syntax error if and only if either (1) there is no action
  for that token or (2) the action for that token is the error action (due to
  the use of `%nonassoc`). However, if there is a default reduction in
  that state (which might or might not be a defaulted state), then it is
  impossible for condition 1 to exist. That is, all tokens have an action.
  Thus, the parser sometimes fails to detect the syntax error until it reaches
  a later state.

  While default reductions never cause the parser to accept syntactically
  incorrect sentences, the delay of syntax error detection can have unexpected
  effects on the behavior of the parser. However, the delay can be caused
  anyway by parser state merging and the use of `%nonassoc`, and it can
  be fixed by another Bison feature, LAC. We discuss the effects of delayed
  syntax error detection and LAC more in the next section (see [LAC](#LAC)).

For canonical LR, the only default reduction that Bison enables by default
is the accept action, which appears only in the accepting state, which has
no other action and is thus a defaulted state. However, the default accept
action does not delay any `yylex` invocation or syntax error detection
because the accept action ends the parse.

For LALR and IELR, Bison enables default reductions in nearly all states by
default. There are only two exceptions. First, states that have a shift
action on the `error` token do not have default reductions because
delayed syntax error detection could then prevent the `error` token
from ever being shifted in that state. However, parser state merging can
cause the same effect anyway, and LAC fixes it in both cases, so future
versions of Bison might drop this exception when LAC is activated. Second,
GLR parsers do not record the default reduction as the action on a lookahead
token for which there is a conflict. The correct action in this case is to
split the parse instead.

To adjust which states have default reductions enabled, use the
`%define lr.default-reduction` directive.

Directive: **%define lr.default-reduction** *where* [¶](#index-_0025define-lr_002edefault_002dreduction-2)
:   Specify the kind of states that are permitted to contain default reductions.
    The accepted values of where are:

    * `most` (default for LALR and IELR)
    * `consistent`
    * `accepting` (default for canonical LR)

---

Next: [Unreachable States](#Unreachable-States), Previous: [Default Reductions](#Default-Reductions), Up: [Tuning LR](#Tuning-LR)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.8.3 LAC

Canonical LR, IELR, and LALR can suffer from a couple of problems upon
encountering a syntax error. First, the parser might perform additional
parser stack reductions before discovering the syntax error. Such
reductions can perform user semantic actions that are unexpected because
they are based on an invalid token, and they cause error recovery to begin
in a different syntactic context than the one in which the invalid token was
encountered. Second, when verbose error messages are enabled (see [Error Reporting](#Error-Reporting)), the expected token list in the syntax error message can both
contain invalid tokens and omit valid tokens.

The culprits for the above problems are `%nonassoc`, default reductions
in inconsistent states (see [Default Reductions](#Default-Reductions)), and parser state
merging. Because IELR and LALR merge parser states, they suffer the most.
Canonical LR can suffer only if `%nonassoc` is used or if default
reductions are enabled for inconsistent states.

LAC (Lookahead Correction) is a new mechanism within the parsing algorithm
that solves these problems for canonical LR, IELR, and LALR without
sacrificing `%nonassoc`, default reductions, or state merging. You can
enable LAC with the `%define parse.lac` directive.

Directive: **%define parse.lac** *value* [¶](#index-_0025define-parse_002elac-2)
:   Enable LAC to improve syntax error handling.

    * `none` (default)
    * `full`

    This feature is currently only available for deterministic parsers in C and C++.

Conceptually, the LAC mechanism is straight-forward. Whenever the parser
fetches a new token from the scanner so that it can determine the next
parser action, it immediately suspends normal parsing and performs an
exploratory parse using a temporary copy of the normal parser state stack.
During this exploratory parse, the parser does not perform user semantic
actions. If the exploratory parse reaches a shift action, normal parsing
then resumes on the normal parser stacks. If the exploratory parse reaches
an error instead, the parser reports a syntax error. If verbose syntax
error messages are enabled, the parser must then discover the list of
expected tokens, so it performs a separate exploratory parse for each token
in the grammar.

There is one subtlety about the use of LAC. That is, when in a consistent
parser state with a default reduction, the parser will not attempt to fetch
a token from the scanner because no lookahead is needed to determine the
next parser action. Thus, whether default reductions are enabled in
consistent states (see [Default Reductions](#Default-Reductions)) affects how soon the parser
detects a syntax error: immediately when it *reaches* an erroneous
token or when it eventually *needs* that token as a lookahead to
determine the next parser action. The latter behavior is probably more
intuitive, so Bison currently provides no way to achieve the former behavior
while default reductions are enabled in consistent states.

Thus, when LAC is in use, for some fixed decision of whether to enable
default reductions in consistent states, canonical LR and IELR behave almost
exactly the same for both syntactically acceptable and syntactically
unacceptable input. While LALR still does not support the full
language-recognition power of canonical LR and IELR, LAC at least enables
LALR’s syntax error handling to correctly reflect LALR’s
language-recognition power.

There are a few caveats to consider when using LAC:

* Infinite parsing loops.

  IELR plus LAC does have one shortcoming relative to canonical LR. Some
  parsers generated by Bison can loop infinitely. LAC does not fix infinite
  parsing loops that occur between encountering a syntax error and detecting
  it, but enabling canonical LR or disabling default reductions sometimes
  does.
* Verbose error message limitations.

  Because of internationalization considerations, Bison-generated parsers
  limit the size of the expected token list they are willing to report in a
  verbose syntax error message. If the number of expected tokens exceeds that
  limit, the list is simply dropped from the message. Enabling LAC can
  increase the size of the list and thus cause the parser to drop it. Of
  course, dropping the list is better than reporting an incorrect list.
* Performance.

  Because LAC requires many parse actions to be performed twice, it can have a
  performance penalty. However, not all parse actions must be performed
  twice. Specifically, during a series of default reductions in consistent
  states and shift actions, the parser never has to initiate an exploratory
  parse. Moreover, the most time-consuming tasks in a parse are often the
  file I/O, the lexical analysis performed by the scanner, and the user’s
  semantic actions, but none of these are performed during the exploratory
  parse. Finally, the base of the temporary stack used during an exploratory
  parse is a pointer into the normal parser state stack so that the stack is
  never physically copied. In our experience, the performance penalty of LAC
  has proved insignificant for practical grammars.

While the LAC algorithm shares techniques that have been recognized in the
parser community for years, for the publication that introduces LAC, see
[Denny 2010 May](#Denny-2010-May).

---

Previous: [LAC](#LAC), Up: [Tuning LR](#Tuning-LR)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 5.8.4 Unreachable States

If there exists no sequence of transitions from the parser’s start state to
some state s, then Bison considers s to be an *unreachable
state*. A state can become unreachable during conflict resolution if Bison
disables a shift action leading to it from a predecessor state.

By default, Bison removes unreachable states from the parser after conflict
resolution because they are useless in the generated parser. However,
keeping unreachable states is sometimes useful when trying to understand the
relationship between the parser and the grammar.

Directive: **%define lr.keep-unreachable-state** *value* [¶](#index-_0025define-lr_002ekeep_002dunreachable_002dstate-2)
:   Request that Bison allow unreachable states to remain in the parser tables.
    value must be a Boolean. The default is `false`.

There are a few caveats to consider:

* Missing or extraneous warnings.

  Unreachable states may contain conflicts and may use rules not used in any
  other state. Thus, keeping unreachable states may induce warnings that are
  irrelevant to your parser’s behavior, and it may eliminate warnings that are
  relevant. Of course, the change in warnings may actually be relevant to a
  parser table analysis that wants to keep unreachable states, so this
  behavior will likely remain in future Bison releases.
* Other useless states.

  While Bison is able to remove unreachable states, it is not guaranteed to
  remove other kinds of useless states. Specifically, when Bison disables
  reduce actions during conflict resolution, some goto actions may become
  useless, and thus some additional states may become useless. If Bison were
  to compute which goto actions were useless and then disable those actions,
  it could identify such states as unreachable and then remove those states.
  However, Bison does not compute which goto actions are useless.

---

Next: [Memory Management, and How to Avoid Memory Exhaustion](#Memory-Management), Previous: [Tuning LR](#Tuning-LR), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 5.9 Generalized LR (GLR) Parsing

Bison produces *deterministic* parsers that choose uniquely
when to reduce and which reduction to apply
based on a summary of the preceding input and on one extra token of lookahead.
As a result, normal Bison handles a proper subset of the family of
context-free languages.
Ambiguous grammars, since they have strings with more than one possible
sequence of reductions cannot have deterministic parsers in this sense.
The same is true of languages that require more than one symbol of
lookahead, since the parser lacks the information necessary to make a
decision at the point it must be made in a shift/reduce parser.
Finally, as previously mentioned (see [Mysterious Conflicts](#Mysterious-Conflicts)),
there are languages where Bison’s default choice of how to
summarize the input seen so far loses necessary information.

When you use the ‘`%glr-parser`’ declaration in your grammar file,
Bison generates a parser that uses a different algorithm, called
Generalized LR (or GLR). A Bison GLR
parser uses the same basic
algorithm for parsing as an ordinary Bison parser, but behaves
differently in cases where there is a shift/reduce conflict that has not
been resolved by precedence rules (see [Operator Precedence](#Precedence)) or a
reduce/reduce conflict. When a GLR parser encounters such a
situation, it
effectively *splits* into a several parsers, one for each possible
shift or reduction. These parsers then proceed as usual, consuming
tokens in lock-step. Some of the stacks may encounter other conflicts
and split further, with the result that instead of a sequence of states,
a Bison GLR parsing stack is what is in effect a tree of states.

In effect, each stack represents a guess as to what the proper parse
is. Additional input may indicate that a guess was wrong, in which case
the appropriate stack silently disappears. Otherwise, the semantics
actions generated in each stack are saved, rather than being executed
immediately. When a stack disappears, its saved semantic actions never
get executed. When a reduction causes two stacks to become equivalent,
their sets of semantic actions are both saved with the state that
results from the reduction. We say that two stacks are equivalent
when they both represent the same sequence of states,
and each pair of corresponding states represents a
grammar symbol that produces the same segment of the input token
stream.

Whenever the parser makes a transition from having multiple
states to having one, it reverts to the normal deterministic parsing
algorithm, after resolving and executing the saved-up actions.
At this transition, some of the states on the stack will have semantic
values that are sets (actually multisets) of possible actions. The
parser tries to pick one of the actions by first finding one whose rule
has the highest dynamic precedence, as set by the ‘`%dprec`’
declaration. Otherwise, if the alternative actions are not ordered by
precedence, but there the same merging function is declared for both
rules by the ‘`%merge`’ declaration,
Bison resolves and evaluates both and then calls the merge function on
the result. Otherwise, it reports an ambiguity.

It is possible to use a data structure for the GLR parsing tree that
permits the processing of any LR(1) grammar in linear time (in the
size of the input), any unambiguous (not necessarily
LR(1)) grammar in
quadratic worst-case time, and any general (possibly ambiguous)
context-free grammar in cubic worst-case time. However, Bison currently
uses a simpler data structure that requires time proportional to the
length of the input times the maximum number of stacks required for any
prefix of the input. Thus, really ambiguous or nondeterministic
grammars can require exponential time and space to process. Such badly
behaving examples, however, are not generally of practical interest.
Usually, nondeterminism in a grammar is local—the parser is “in
doubt” only for a few tokens at a time. Therefore, the current data
structure should generally be adequate. On LR(1) portions of a
grammar, in particular, it is only slightly slower than with the
deterministic LR(1) Bison parser.

For a more detailed exposition of GLR parsers, see [Scott 2000](#Scott-2000).

---

Previous: [Generalized LR (GLR) Parsing](#Generalized-LR-Parsing), Up: [The Bison Parser Algorithm](#Algorithm)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 5.10 Memory Management, and How to Avoid Memory Exhaustion

The Bison parser stack can run out of memory if too many tokens are shifted and
not reduced. When this happens, the parser function `yyparse`
calls `yyerror` and then returns 2.

Because Bison parsers have growing stacks, hitting the upper limit
usually results from using a right recursion instead of a left
recursion, see [Recursive Rules](#Recursion).

By defining the macro `YYMAXDEPTH`, you can control how deep the
parser stack can become before memory is exhausted. Define the
macro with a value that is an integer. This value is the maximum number
of tokens that can be shifted (and not reduced) before overflow.

The stack space allowed is not necessarily allocated. If you specify a
large value for `YYMAXDEPTH`, the parser normally allocates a small
stack at first, and then makes it bigger by stages as needed. This
increasing allocation happens automatically and silently. Therefore,
you do not need to make `YYMAXDEPTH` painfully small merely to save
space for ordinary inputs that do not need much stack.

However, do not allow `YYMAXDEPTH` to be a value so large that
arithmetic overflow could occur when calculating the size of the stack
space. Also, do not allow `YYMAXDEPTH` to be less than
`YYINITDEPTH`.

The default value of `YYMAXDEPTH`, if you do not define it, is
10000.

You can control how much stack is allocated initially by defining the
macro `YYINITDEPTH` to a positive integer. For the deterministic
parser in C, this value must be a compile-time constant
unless you are assuming C99 or some other target language or compiler
that allows variable-length arrays. The default is 200.

Do not allow `YYINITDEPTH` to be greater than `YYMAXDEPTH`.

You can generate a deterministic parser containing C++ user code from the
default (C) skeleton, as well as from the C++ skeleton (see [C++ Parsers](#C_002b_002b-Parsers)). However, if you do use the default skeleton and want to allow
the parsing stack to grow, be careful not to use semantic types or location
types that require non-trivial copy constructors. The C skeleton bypasses
these constructors when copying data to new, larger stacks.

---

Next: [Handling Context Dependencies](#Context-Dependency), Previous: [The Bison Parser Algorithm](#Algorithm), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 6 Error Recovery

It is not usually acceptable to have a program terminate on a syntax
error. For example, a compiler should recover sufficiently to parse the
rest of the input file and check it for errors; a calculator should accept
another expression.

In a simple interactive command parser where each input is one line, it may
be sufficient to allow `yyparse` to return 1 on error and have the
caller ignore the rest of the input line when that happens (and then call
`yyparse` again). But this is inadequate for a compiler, because it
forgets all the syntactic context leading up to the error. A syntax error
deep within a function in the compiler input should not cause the compiler
to treat the following line like the beginning of a source file.

You can define how to recover from a syntax error by writing rules to
recognize the special token `error`. This is a terminal symbol that
is always defined (you need not declare it) and reserved for error
handling. The Bison parser generates an `error` token whenever a
syntax error happens; if you have provided a rule to recognize this token
in the current context, the parse can continue.

For example:

```
stmts:
  %empty
| stmts '\n'
| stmts exp '\n'
| stmts error '\n'
```

The fourth rule in this example says that an error followed by a newline
makes a valid addition to any `stmts`.

What happens if a syntax error occurs in the middle of an `exp`? The
error recovery rule, interpreted strictly, applies to the precise sequence
of a `stmts`, an `error` and a newline. If an error occurs in
the middle of an `exp`, there will probably be some additional tokens
and subexpressions on the stack after the last `stmts`, and there
will be tokens to read before the next newline. So the rule is not
applicable in the ordinary way.

But Bison can force the situation to fit the rule, by discarding part of the
semantic context and part of the input. First it discards states and
objects from the stack until it gets back to a state in which the
`error` token is acceptable. (This means that the subexpressions
already parsed are discarded, back to the last complete `stmts`.) At
this point the `error` token can be shifted. Then, if the old
lookahead token is not acceptable to be shifted next, the parser reads
tokens and discards them until it finds a token which is acceptable. In
this example, Bison reads and discards input until the next newline so that
the fourth rule can apply. Note that discarded symbols are possible sources
of memory leaks, see [Freeing Discarded Symbols](#Destructor-Decl), for a means to reclaim this
memory.

The choice of error rules in the grammar is a choice of strategies for
error recovery. A simple and useful strategy is simply to skip the rest of
the current input line or current statement if an error is detected:

```
stmt: error ';'  /* On error, skip until ';' is read. */
```

It is also useful to recover to the matching close-delimiter of an
opening-delimiter that has already been parsed. Otherwise the
close-delimiter will probably appear to be unmatched, and generate another,
spurious error message:

```
primary:
  '(' expr ')'
| '(' error ')'
…
;
```

Error recovery strategies are necessarily guesses. When they guess wrong,
one syntax error often leads to another. In the above example, the error
recovery rule guesses that an error is due to bad input within one
`stmt`. Suppose that instead a spurious semicolon is inserted in the
middle of a valid `stmt`. After the error recovery rule recovers from
the first error, another syntax error will be found straight away, since the
text following the spurious semicolon is also an invalid `stmt`.

To prevent an outpouring of error messages, the parser will output no error
message for another syntax error that happens shortly after the first; only
after three consecutive input tokens have been successfully shifted will
error messages resume.

Note that rules which accept the `error` token may have actions, just
as any other rules can.

You can make error messages resume immediately by using the macro
`yyerrok` in an action. If you do this in the error rule’s action, no
error messages will be suppressed. This macro requires no arguments;
‘`yyerrok;`’ is a valid C statement.

The previous lookahead token is reanalyzed immediately after an error. If
this is unacceptable, then the macro `yyclearin` may be used to clear
this token. Write the statement ‘`yyclearin;`’ in the error rule’s
action.
See [Special Features for Use in Actions](#Action-Features).

For example, suppose that on a syntax error, an error handling routine is
called that advances the input stream to some point where parsing should
once again commence. The next symbol returned by the lexical scanner is
probably correct. The previous lookahead token ought to be discarded
with ‘`yyclearin;`’.

The expression `YYRECOVERING ()` yields 1 when the parser
is recovering from a syntax error, and 0 otherwise.
Syntax error diagnostics are suppressed while recovering from a syntax
error.

---

Next: [Debugging Your Parser](#Debugging), Previous: [Error Recovery](#Error-Recovery), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 7 Handling Context Dependencies

The Bison paradigm is to parse tokens first, then group them into larger
syntactic units. In many languages, the meaning of a token is affected by
its context. Although this violates the Bison paradigm, certain techniques
(known as *kludges*) may enable you to write Bison parsers for such
languages.

(Actually, “kludge” means any technique that gets its job done but is
neither clean nor robust.)

* [Semantic Info in Token Kinds](#Semantic-Tokens)
* [Lexical Tie-ins](#Lexical-Tie_002dins)
* [Lexical Tie-ins and Error Recovery](#Tie_002din-Recovery)

---

Next: [Lexical Tie-ins](#Lexical-Tie_002dins), Up: [Handling Context Dependencies](#Context-Dependency)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 7.1 Semantic Info in Token Kinds

The C language has a context dependency: the way an identifier is used
depends on what its current meaning is. For example, consider this:

```
foo (x);
```

This looks like a function call statement, but if `foo` is a typedef
name, then this is actually a declaration of `x`. How can a Bison
parser for C decide how to parse this input?

The method used in GNU C is to have two different token kinds,
`IDENTIFIER` and `TYPENAME`. When `yylex` finds an
identifier, it looks up the current declaration of the identifier in order
to decide which token kind to return: `TYPENAME` if the identifier is
declared as a typedef, `IDENTIFIER` otherwise.

The grammar rules can then express the context dependency by the choice of
token kind to recognize. `IDENTIFIER` is accepted as an expression,
but `TYPENAME` is not. `TYPENAME` can start a declaration, but
`IDENTIFIER` cannot. In contexts where the meaning of the identifier
is *not* significant, such as in declarations that can shadow a
typedef name, either `TYPENAME` or `IDENTIFIER` is
accepted—there is one rule for each of the two token kinds.

This technique is simple to use if the decision of which kinds of
identifiers to allow is made at a place close to where the identifier is
parsed. But in C this is not always so: C allows a declaration to
redeclare a typedef name provided an explicit type has been specified
earlier:

```
typedef int foo, bar;
int baz (void)
```

```
{
  static bar (bar);      /* redeclare bar as static variable */
  extern foo foo (foo);  /* redeclare foo as function */
  return foo (bar);
}
```

Unfortunately, the name being declared is separated from the declaration
construct itself by a complicated syntactic structure—the “declarator”.

As a result, part of the Bison parser for C needs to be duplicated, with
all the nonterminal names changed: once for parsing a declaration in
which a typedef name can be redefined, and once for parsing a
declaration in which that can’t be done. Here is a part of the
duplication, with actions omitted for brevity:

```
initdcl:
  declarator maybeasm '=' init
| declarator maybeasm
;
```

```

```

```
notype_initdcl:
  notype_declarator maybeasm '=' init
| notype_declarator maybeasm
;
```

Here `initdcl` can redeclare a typedef name, but `notype_initdcl`
cannot. The distinction between `declarator` and
`notype_declarator` is the same sort of thing.

There is some similarity between this technique and a lexical tie-in
(described next), in that information which alters the lexical analysis is
changed during parsing by other parts of the program. The difference is
here the information is global, and is used for other purposes in the
program. A true lexical tie-in has a special-purpose flag controlled by
the syntactic context.

---

Next: [Lexical Tie-ins and Error Recovery](#Tie_002din-Recovery), Previous: [Semantic Info in Token Kinds](#Semantic-Tokens), Up: [Handling Context Dependencies](#Context-Dependency)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 7.2 Lexical Tie-ins

One way to handle context-dependency is the *lexical tie-in*: a flag
which is set by Bison actions, whose purpose is to alter the way tokens are
parsed.

For example, suppose we have a language vaguely like C, but with a special
construct ‘`hex (hex-expr)`’. After the keyword `hex` comes
an expression in parentheses in which all integers are hexadecimal. In
particular, the token ‘`a1b`’ must be treated as an integer rather than
as an identifier if it appears in that context. Here is how you can do it:

```
%{
  int hexflag;
  int yylex (void);
  void yyerror (char const *);
%}
%%
…
```

```
expr:
  IDENTIFIER
| constant
| HEX '('        { hexflag = 1; }
    expr ')'     { hexflag = 0; $$ = $4; }
| expr '+' expr  { $$ = make_sum ($1, $3); }
…
;
```

```

```

```
constant:
  INTEGER
| STRING
;
```

Here we assume that `yylex` looks at the value of `hexflag`; when
it is nonzero, all integers are parsed in hexadecimal, and tokens starting
with letters are parsed as integers if possible.

The declaration of `hexflag` shown in the prologue of the grammar file
is needed to make it accessible to the actions (see [The prologue](#Prologue)). You must
also write the code in `yylex` to obey the flag.

---

Previous: [Lexical Tie-ins](#Lexical-Tie_002dins), Up: [Handling Context Dependencies](#Context-Dependency)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 7.3 Lexical Tie-ins and Error Recovery

Lexical tie-ins make strict demands on any error recovery rules you have.
See [Error Recovery](#Error-Recovery).

The reason for this is that the purpose of an error recovery rule is to
abort the parsing of one construct and resume in some larger construct.
For example, in C-like languages, a typical error recovery rule is to skip
tokens until the next semicolon, and then start a new statement, like this:

```
stmt:
  expr ';'
| IF '(' expr ')' stmt { … }
…
| error ';'  { hexflag = 0; }
;
```

If there is a syntax error in the middle of a ‘`hex (expr)`’
construct, this error rule will apply, and then the action for the
completed ‘`hex (expr)`’ will never run. So `hexflag` would
remain set for the entire rest of the input, or until the next `hex`
keyword, causing identifiers to be misinterpreted as integers.

To avoid this problem the error recovery rule itself clears `hexflag`.

There may also be an error recovery rule that works within expressions.
For example, there could be a rule which applies within parentheses
and skips to the close-parenthesis:

```
expr:
  …
| '(' expr ')'   { $$ = $2; }
| '(' error ')'
…
```

If this rule acts within the `hex` construct, it is not going to abort
that construct (since it applies to an inner level of parentheses within
the construct). Therefore, it should not clear the flag: the rest of
the `hex` construct should be parsed with the flag still in effect.

What if there is an error recovery rule which might abort out of the
`hex` construct or might not, depending on circumstances? There is no
way you can write the action to determine whether a `hex` construct is
being aborted or not. So if you are using a lexical tie-in, you had better
make sure your error recovery rules are not of this kind. Each rule must
be such that you can be sure that it always will, or always won’t, have to
clear the flag.

---

Next: [Invoking Bison](#Invocation), Previous: [Handling Context Dependencies](#Context-Dependency), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 8 Debugging Your Parser

Developing a parser can be a challenge, especially if you don’t understand
the algorithm (see [The Bison Parser Algorithm](#Algorithm)). This chapter explains how to understand
and debug a parser.

The most frequent issue users face is solving their conflicts. To fix them,
the first step is understanding how they arise in a given grammar. This is
made much easier by automated generation of counterexamples, cover in the
first section (see [Generation of Counterexamples](#Counterexamples)).

In most cases though, looking at the structure of the automaton is still
needed. The following sections explain how to generate and read the
detailed structural description of the automaton. There are several formats
available:

* - as text, see [Understanding Your Parser](#Understanding);
* - as a graph, see [Visualizing Your Parser](#Graphviz);
* - or as a markup report that can be turned, for instance, into HTML, see
  [Visualizing your parser in multiple formats](#Xml).

The last section focuses on the dynamic part of the parser: how to enable
and understand the parser run-time traces (see [Tracing Your Parser](#Tracing)).

* [Generation of Counterexamples](#Counterexamples)
* [Understanding Your Parser](#Understanding)
* [Visualizing Your Parser](#Graphviz)
* [Visualizing your parser in multiple formats](#Xml)
* [Tracing Your Parser](#Tracing)

---

Next: [Understanding Your Parser](#Understanding), Up: [Debugging Your Parser](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 8.1 Generation of Counterexamples

Solving conflicts is probably the most delicate part of the design of an LR
parser, as demonstrated by the number of sections devoted to them in this
very documentation. To solve a conflict, one must understand it: when does
it occur? Is it because of a flaw in the grammar? Is it rather because
LR(1) cannot cope with this grammar?

One difficulty is that conflicts occur in the *automaton*, and it can
be tricky to relate them to issues in the *grammar* itself. With
experience and patience, analysis of the detailed description of the
automaton (see [Understanding Your Parser](#Understanding)) allows one to find example strings that
reach these conflicts.

That task is made much easier thanks to the generation of counterexamples,
initially developed by Chinawat Isradisaikul and Andrew Myers
(see [Isradisaikul 2015](#Isradisaikul-2015)).

As a first example, see the grammar of [Shift/Reduce Conflicts](#Shift_002fReduce), which features
one shift/reduce conflict:

```
$ bison else.y
else.y: warning: 1 shift/reduce conflict [-Wconflicts-sr]
else.y: note: rerun with option '-Wcounterexamples' to generate conflict counterexamples
```

Let’s rerun `bison` with the option
`-Wcex`/`-Wcounterexamples`:

```
else.y: warning: 1 shift/reduce conflict [-Wconflicts-sr]
else.y: warning: shift/reduce conflict on token "else" [-Wcounterexamples]
```

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

This shows two different derivations for one single expression, which proves
that the grammar is ambiguous.

  

As a more delicate example, consider the example grammar of
[Reduce/Reduce Conflicts](#Reduce_002fReduce), which features a reduce/reduce conflict:

```
%%
sequence:
  %empty
| maybeword
| sequence "word"
;
maybeword:
  %empty
| "word"
;
```

Bison generates the following counterexamples:

```
$ bison -Wcex sequence.y
sequence.y: warning: 1 shift/reduce conflict [-Wconflicts-sr]
sequence.y: warning: 2 reduce/reduce conflicts [-Wconflicts-rr]
```

```
sequence.y: warning: shift/reduce conflict on token "word" [-Wcounterexamples]
  Example: • "word"
  Shift derivation
    sequence
    ↳ 2: maybeword
          ↳ 5: • "word"
  Example: • "word"
  Reduce derivation
    sequence
    ↳ 3: sequence "word"
          ↳ 1: •
```

```
sequence.y: warning: reduce/reduce conflict on tokens $end, "word" [-Wcounterexamples]
  Example: •
  First reduce derivation
    sequence
    ↳ 1: •
  Example: •
  Second reduce derivation
    sequence
    ↳ 2: maybeword
          ↳ 4: •
```

```
sequence.y: warning: shift/reduce conflict on token "word" [-Wcounterexamples]
  Example: • "word"
  Shift derivation
    sequence
    ↳ 2: maybeword
          ↳ 5: • "word"
  Example: • "word"
  Reduce derivation
    sequence
    ↳ 3: sequence        "word"
          ↳ 2: maybeword
                ↳ 4: •
```

```
sequence.y:8.3-45: warning: rule useless in parser due to conflicts [-Wother]
    8 |   %empty    { printf ("empty maybeword\n"); }
      |   ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

Each of these three conflicts, again, prove that the grammar is ambiguous.
For instance, the second conflict (the reduce/reduce one) shows that the
grammar accepts the empty input in two different ways.

  

Sometimes, the search will not find an example that can be derived in two
ways. In these cases, counterexample generation will provide two examples
that are the same up until the dot. Most notably, this will happen when
your grammar requires a stronger parser (more lookahead, LR instead of
LALR). The following example isn’t LR(1):

```
%token ID
%%
s: a ID
a: expr
expr: %empty | expr ID ','
```

`bison` reports:

```
ids.y: warning: 1 shift/reduce conflict [-Wconflicts-sr]
ids.y: warning: shift/reduce conflict on token ID [-Wcounterexamples]
```

```
  First example: expr • ID ',' ID $end
  Shift derivation
    $accept
    ↳ 0: s                                 $end
         ↳ 1: a                        ID
              ↳ 2: expr
                    ↳ 4: expr • ID ','
  Second example: expr • ID $end
  Reduce derivation
    $accept
    ↳ 0: s                   $end
         ↳ 1: a           ID
              ↳ 2: expr •
```

```
ids.y:4.4-7: warning: rule useless in parser due to conflicts [-Wother]
    4 | a: expr
      |    ^~~~
```

This conflict is caused by the parser not having enough information to know
the difference between these two examples. The parser would need an
additional lookahead token to know whether or not a comma follows the
`ID` after `expr`. These types of conflicts tend to be more
difficult to fix, and usually need a rework of the grammar. In this case,
it can be fixed by changing around the recursion: `expr: ID | ',' expr
ID`.

Alternatively, you might also want to consider using a GLR parser
(see [Writing GLR Parsers](#GLR-Parsers)).

  

On occasions, it is useful to look at counterexamples *in situ*: with
the automaton report (See [Understanding Your Parser](#Understanding), in particular [State 8](#state_002d8)).

---

Next: [Visualizing Your Parser](#Graphviz), Previous: [Generation of Counterexamples](#Counterexamples), Up: [Debugging Your Parser](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 8.2 Understanding Your Parser

Bison parsers are *shift/reduce automata* (see [The Bison Parser Algorithm](#Algorithm)). In some
cases (much more frequent than one would hope), looking at this automaton is
required to tune or simply fix a parser.

The textual file is generated when the options `--report` or
`--verbose` are specified, see [Invoking Bison](#Invocation). Its name is made by
removing ‘`.tab.c`’ or ‘`.c`’ from the parser implementation file
name, and adding ‘`.output`’ instead. Therefore, if the grammar file is
`foo.y`, then the parser implementation file is called `foo.tab.c`
by default. As a consequence, the verbose output file is called
`foo.output`.

The following grammar file, `calc.y`, will be used in the sequel:

```
%union
{
  int ival;
  const char *sval;
}
```

```
%token <ival> NUM
%nterm <ival> exp
```

```
%token <sval> STR
%nterm <sval> useless
```

```
%left '+' '-'
%left '*'
```

```
%%
```

```
exp:
  exp '+' exp
| exp '-' exp
| exp '*' exp
| exp '/' exp
| NUM
;
```

```
useless: STR;
%%
```

`bison` reports:

```
calc.y: warning: 1 nonterminal useless in grammar [-Wother]
calc.y: warning: 1 rule useless in grammar [-Wother]
calc.y:19.1-7: warning: nonterminal useless in grammar: useless [-Wother]
   19 | useless: STR;
      | ^~~~~~~
calc.y: warning: 7 shift/reduce conflicts [-Wconflicts-sr]
calc.y: note: rerun with option '-Wcounterexamples' to generate conflict counterexamples
```

Going back to the calc example, when given `--report=state`,
in addition to `calc.tab.c`, it creates a file `calc.output`
with contents detailed below. The order of the output and the exact
presentation might vary, but the interpretation is the same.

The first section reports useless tokens, nonterminals and rules. Useless
nonterminals and rules are removed in order to produce a smaller parser, but
useless tokens are preserved, since they might be used by the scanner (note
the difference between “useless” and “unused” below):

```
Nonterminals useless in grammar
   useless

Terminals unused in grammar
   STR

Rules useless in grammar
    6 useless: STR
```

The next section lists states that still have conflicts.

```
State 8 conflicts: 1 shift/reduce
State 9 conflicts: 1 shift/reduce
State 10 conflicts: 1 shift/reduce
State 11 conflicts: 4 shift/reduce
```

Then Bison reproduces the exact grammar it used:

```
Grammar

    0 $accept: exp $end

    1 exp: exp '+' exp
    2    | exp '-' exp
    3    | exp '*' exp
    4    | exp '/' exp
    5    | NUM
```

and reports the uses of the symbols:

```
Terminals, with rules where they appear

    $end (0) 0
    '*' (42) 3
    '+' (43) 1
    '-' (45) 2
    '/' (47) 4
    error (256)
    NUM <ival> (258) 5
    STR <sval> (259)
```

```

```

```
Nonterminals, with rules where they appear

    $accept (9)
        on left: 0
    exp <ival> (10)
        on left: 1 2 3 4 5
        on right: 0 1 2 3 4
```

Bison then proceeds onto the automaton itself, describing each state with
its set of *items*, also known as *dotted rules*. Each item is a
production rule together with a point (‘`.`’) marking the location of the
input cursor.

```
State 0

    0 $accept: • exp $end

    NUM  shift, and go to state 1

    exp  go to state 2
```

This reads as follows: “state 0 corresponds to being at the very
beginning of the parsing, in the initial rule, right before the start
symbol (here, `exp`). When the parser returns to this state right
after having reduced a rule that produced an `exp`, the control
flow jumps to state 2. If there is no such transition on a nonterminal
symbol, and the lookahead is a `NUM`, then this token is shifted onto
the parse stack, and the control flow jumps to state 1. Any other
lookahead triggers a syntax error.”

Even though the only active rule in state 0 seems to be rule 0, the
report lists `NUM` as a lookahead token because `NUM` can be
at the beginning of any rule deriving an `exp`. By default Bison
reports the so-called *core* or *kernel* of the item set, but if
you want to see more detail you can invoke `bison` with
`--report=itemset` to list the derived items as well:

```
State 0

    0 $accept: • exp $end
    1 exp: • exp '+' exp
    2    | • exp '-' exp
    3    | • exp '*' exp
    4    | • exp '/' exp
    5    | • NUM

    NUM  shift, and go to state 1

    exp  go to state 2
```

In the state 1…

```
State 1

    5 exp: NUM •

    $default  reduce using rule 5 (exp)
```

the rule 5, ‘`exp: NUM;`’, is completed. Whatever the lookahead token
(‘`$default`’), the parser will reduce it. If it was coming from State
0, then, after this reduction it will return to state 0, and will jump to
state 2 (‘`exp: go to state 2`’).

```
State 2

    0 $accept: exp • $end
    1 exp: exp • '+' exp
    2    | exp • '-' exp
    3    | exp • '*' exp
    4    | exp • '/' exp

    $end  shift, and go to state 3
    '+'   shift, and go to state 4
    '-'   shift, and go to state 5
    '*'   shift, and go to state 6
    '/'   shift, and go to state 7
```

In state 2, the automaton can only shift a symbol. For instance, because of
the item ‘`exp: exp • '+' exp`’, if the lookahead is ‘`+`’ it is
shifted onto the parse stack, and the automaton jumps to state 4,
corresponding to the item ‘`exp: exp '+' • exp`’. Since there is no
default action, any lookahead not listed triggers a syntax error.

The state 3 is named the *final state*, or the *accepting
state*:

```
State 3

    0 $accept: exp $end •

    $default  accept
```

the initial rule is completed (the start symbol and the end-of-input were
read), the parsing exits successfully.

The interpretation of states 4 to 7 is straightforward, and is left to
the reader.

```
State 4

    1 exp: exp '+' • exp

    NUM  shift, and go to state 1

    exp  go to state 8

State 5

    2 exp: exp '-' • exp

    NUM  shift, and go to state 1

    exp  go to state 9

State 6

    3 exp: exp '*' • exp

    NUM  shift, and go to state 1

    exp  go to state 10

State 7

    4 exp: exp '/' • exp

    NUM  shift, and go to state 1

    exp  go to state 11
```

As was announced in beginning of the report, ‘`State 8 conflicts:
1 shift/reduce`’:

```
State 8

    1 exp: exp • '+' exp
    1    | exp '+' exp •
    2    | exp • '-' exp
    3    | exp • '*' exp
    4    | exp • '/' exp

    '*'  shift, and go to state 6
    '/'  shift, and go to state 7

    '/'       [reduce using rule 1 (exp)]
    $default  reduce using rule 1 (exp)
```

Indeed, there are two actions associated to the lookahead ‘`/`’:
either shifting (and going to state 7), or reducing rule 1. The
conflict means that either the grammar is ambiguous, or the parser lacks
information to make the right decision. Indeed the grammar is
ambiguous, as, since we did not specify the precedence of ‘`/`’, the
sentence ‘`NUM + NUM / NUM`’ can be parsed as ‘`NUM + (NUM /
NUM)`’, which corresponds to shifting ‘`/`’, or as ‘`(NUM + NUM) /
NUM`’, which corresponds to reducing rule 1.

Because in deterministic parsing a single decision can be made, Bison
arbitrarily chose to disable the reduction, see [Shift/Reduce Conflicts](#Shift_002fReduce).
Discarded actions are reported between square brackets.

Note that all the previous states had a single possible action: either
shifting the next token and going to the corresponding state, or
reducing a single rule. In the other cases, i.e., when shifting
*and* reducing is possible or when *several* reductions are
possible, the lookahead is required to select the action. State 8 is
one such state: if the lookahead is ‘`*`’ or ‘`/`’ then the action
is shifting, otherwise the action is reducing rule 1. In other words,
the first two items, corresponding to rule 1, are not eligible when the
lookahead token is ‘`*`’, since we specified that ‘`*`’ has higher
precedence than ‘`+`’. More generally, some items are eligible only
with some set of possible lookahead tokens. When run with
`--report=lookahead`, Bison specifies these lookahead tokens:

```
State 8

    1 exp: exp • '+' exp
    1    | exp '+' exp •  [$end, '+', '-', '/']
    2    | exp • '-' exp
    3    | exp • '*' exp
    4    | exp • '/' exp

    '*'  shift, and go to state 6
    '/'  shift, and go to state 7

    '/'       [reduce using rule 1 (exp)]
    $default  reduce using rule 1 (exp)
```

Note however that while ‘`NUM + NUM / NUM`’ is ambiguous (which results in
the conflicts on ‘`/`’), ‘`NUM + NUM * NUM`’ is not: the conflict was
solved thanks to associativity and precedence directives. If invoked with
`--report=solved`, Bison includes information about the solved
conflicts in the report:

```
Conflict between rule 1 and token '+' resolved as reduce (%left '+').
Conflict between rule 1 and token '-' resolved as reduce (%left '-').
Conflict between rule 1 and token '*' resolved as shift ('+' < '*').
```

When given `--report=counterexamples`, `bison` will generate
counterexamples within the report, augmented with the corresponding items
(see [Generation of Counterexamples](#Counterexamples)).

```
shift/reduce conflict on token '/':
    1 exp: exp '+' exp •
    4 exp: exp • '/' exp
```

```
  Example: exp '+' exp • '/' exp
  Shift derivation
    exp
    ↳ 1: exp '+' exp
                 ↳ 4: exp • '/' exp
  Example: exp '+' exp • '/' exp
  Reduce derivation
    exp
    ↳ 4: exp                 '/' exp
         ↳ 1: exp '+' exp •
```

This shows two separate derivations in the grammar for the same `exp`:
‘`e1 + e2 / e3`’. The derivations show how your rules would parse the
given example. Here, the first derivation completes a reduction when seeing
‘`/`’, causing ‘`e1 + e2`’ to be grouped as an `exp`. The second
derivation shifts on ‘`/`’, resulting in ‘`e2 / e3`’ being grouped as
an `exp`. Therefore, it is easy to see that adding
precedence/associativity directives would fix this conflict.

The remaining states are similar:

```
State 9

    1 exp: exp • '+' exp
    2    | exp • '-' exp
    2    | exp '-' exp •
    3    | exp • '*' exp
    4    | exp • '/' exp

    '*'  shift, and go to state 6
    '/'  shift, and go to state 7

    '/'       [reduce using rule 2 (exp)]
    $default  reduce using rule 2 (exp)
```

```

```

```
State 10

    1 exp: exp • '+' exp
    2    | exp • '-' exp
    3    | exp • '*' exp
    3    | exp '*' exp •
    4    | exp • '/' exp

    '/'  shift, and go to state 7

    '/'       [reduce using rule 3 (exp)]
    $default  reduce using rule 3 (exp)
```

```

```

```
State 11

    1 exp: exp • '+' exp
    2    | exp • '-' exp
    3    | exp • '*' exp
    4    | exp • '/' exp
    4    | exp '/' exp •

    '+'  shift, and go to state 4
    '-'  shift, and go to state 5
    '*'  shift, and go to state 6
    '/'  shift, and go to state 7

    '+'       [reduce using rule 4 (exp)]
    '-'       [reduce using rule 4 (exp)]
    '*'       [reduce using rule 4 (exp)]
    '/'       [reduce using rule 4 (exp)]
    $default  reduce using rule 4 (exp)
```

Observe that state 11 contains conflicts not only due to the lack of
precedence of ‘`/`’ with respect to ‘`+`’, ‘`-`’, and ‘`*`’, but
also because the associativity of ‘`/`’ is not specified.

Bison may also produce an HTML version of this output, via an XML file and
XSLT processing (see [Visualizing your parser in multiple formats](#Xml)).

---

Next: [Visualizing your parser in multiple formats](#Xml), Previous: [Understanding Your Parser](#Understanding), Up: [Debugging Your Parser](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 8.3 Visualizing Your Parser

As another means to gain better understanding of the shift/reduce
automaton corresponding to the Bison parser, a DOT file can be generated. Note
that debugging a real grammar with this is tedious at best, and impractical
most of the times, because the generated files are huge (the generation of
a PDF or PNG file from it will take very long, and more often than not it will
fail due to memory exhaustion). This option was rather designed for beginners,
to help them understand LR parsers.

This file is generated when the `--graph` option is specified
(see [Invoking Bison](#Invocation)). Its name is made by removing
‘`.tab.c`’ or ‘`.c`’ from the parser implementation file name, and
adding ‘`.gv`’ instead. If the grammar file is `foo.y`, the
Graphviz output file is called `foo.gv`. A DOT file may also be
produced via an XML file and XSLT processing (see [Visualizing your parser in multiple formats](#Xml)).

The following grammar file, `rr.y`, will be used in the sequel:

```
%%
```

```
exp: a ";" | b ".";
a: "0";
b: "0";
```

The graphical output
(see [Figure 8.1](#fig_003agraph))
is very similar to the textual one, and as such it is easier understood by
making direct comparisons between them. See [Debugging Your Parser](#Debugging), for a detailed
analysis of the textual report.

**Figure 8.1:** A graphical rendering of the parser.

#### Graphical Representation of States

The items (dotted rules) for each state are grouped together in graph nodes.
Their numbering is the same as in the verbose file. See the following
points, about transitions, for examples

When invoked with `--report=lookaheads`, the lookahead tokens, when
needed, are shown next to the relevant rule between square brackets as a
comma separated list. This is the case in the figure for the representation of
reductions, below.

  

The transitions are represented as directed edges between the current and
the target states.

#### Graphical Representation of Shifts

Shifts are shown as solid arrows, labeled with the lookahead token for that
shift. The following describes a reduction in the `rr.output` file:

```
State 3

    1 exp: a • ";"

    ";"  shift, and go to state 6
```

A Graphviz rendering of this portion of the graph could be:

#### Graphical Representation of Reductions

Reductions are shown as solid arrows, leading to a diamond-shaped node
bearing the number of the reduction rule. The arrow is labeled with the
appropriate comma separated lookahead tokens. If the reduction is the default
action for the given state, there is no such label.

This is how reductions are represented in the verbose file `rr.output`:

```
State 1

    3 a: "0" •  [";"]
    4 b: "0" •  ["."]

    "."       reduce using rule 4 (b)
    $default  reduce using rule 3 (a)
```

A Graphviz rendering of this portion of the graph could be:

When unresolved conflicts are present, because in deterministic parsing
a single decision can be made, Bison can arbitrarily choose to disable a
reduction, see [Shift/Reduce Conflicts](#Shift_002fReduce). Discarded actions
are distinguished by a red filling color on these nodes, just like how they are
reported between square brackets in the verbose file.

The reduction corresponding to the rule number 0 is the acceptation
state. It is shown as a blue diamond, labeled “Acc”.

#### Graphical Representation of Gotos

The ‘`go to`’ jump transitions are represented as dotted lines bearing
the name of the rule being jumped to.

---

Next: [Tracing Your Parser](#Tracing), Previous: [Visualizing Your Parser](#Graphviz), Up: [Debugging Your Parser](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 8.4 Visualizing your parser in multiple formats

Bison supports two major report formats: textual output
(see [Understanding Your Parser](#Understanding)) when invoked
with option `--verbose`, and DOT
(see [Visualizing Your Parser](#Graphviz)) when invoked with
option `--graph`. However,
another alternative is to output an XML file that may then be, with
`xsltproc`, rendered as either a raw text format equivalent to the
verbose file, or as an HTML version of the same file, with clickable
transitions, or even as a DOT. The `.output` and DOT files obtained via
XSLT have no difference whatsoever with those obtained by invoking
`bison` with options `--verbose` or `--graph`.

The XML file is generated when the options `-x` or
`--xml[=FILE]` are specified, see [Invoking Bison](#Invocation).
If not specified, its name is made by removing ‘`.tab.c`’ or ‘`.c`’
from the parser implementation file name, and adding ‘`.xml`’ instead.
For instance, if the grammar file is `foo.y`, the default XML output
file is `foo.xml`.

Bison ships with a `data/xslt` directory, containing XSL Transformation
files to apply to the XML file. Their names are non-ambiguous:

`xml2dot.xsl`
:   Used to output a copy of the DOT visualization of the automaton.

`xml2text.xsl`
:   Used to output a copy of the ‘`.output`’ file.

`xml2xhtml.xsl`
:   Used to output an xhtml enhancement of the ‘`.output`’ file.

Sample usage (requires `xsltproc`):

```
$ bison -x gr.y
```

```
$ bison --print-datadir
/usr/local/share/bison
```

```
$ xsltproc /usr/local/share/bison/xslt/xml2xhtml.xsl gr.xml >gr.html
```

---

Previous: [Visualizing your parser in multiple formats](#Xml), Up: [Debugging Your Parser](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 8.5 Tracing Your Parser

When a Bison grammar compiles properly but parses “incorrectly”, the
`yydebug` parser-trace feature helps figuring out why.

* [Enabling Traces](#Enabling-Traces)
* [Enabling Debug Traces for `mfcalc`](#Mfcalc-Traces)

---

Next: [Enabling Debug Traces for `mfcalc`](#Mfcalc-Traces), Up: [Tracing Your Parser](#Tracing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 8.5.1 Enabling Traces

There are several means to enable compilation of trace facilities, in
decreasing order of preference:

the variable ‘`parse.trace`’ [¶](#index-_0025define-parse_002etrace-1)
:   Add the ‘`%define parse.trace`’ directive (see [%define Summary](#g_t_0025define-Summary)), or pass the `-Dparse.trace` option
    (see [Tuning the Parser](#Tuning-the-Parser)). This is a Bison extension. Unless POSIX and
    Yacc portability matter to you, this is the preferred solution.

the option `-t` (POSIX Yacc compliant)

the option `--debug` (Bison extension)
:   Use the `-t` option when you run Bison (see [Invoking Bison](#Invocation)). With
    ‘`%define api.prefix {c}`’, it defines `CDEBUG` to 1, otherwise it
    defines `YYDEBUG` to 1.

the directive ‘`%debug`’ (deprecated) [¶](#index-_0025debug-1)
:   Add the `%debug` directive (see [Bison Declaration Summary](#Decl-Summary)). This Bison
    extension is maintained for backward compatibility; use `%define
    parse.trace` instead.

the macro `YYDEBUG` (C/C++ only) [¶](#index-YYDEBUG)
:   Define the macro `YYDEBUG` to a nonzero value when you compile the
    parser. This is compliant with POSIX Yacc. You could use
    `-DYYDEBUG=1` as a compiler option or you could put ‘`#define
    YYDEBUG 1`’ in the prologue of the grammar file (see [The prologue](#Prologue)).

    If the `%define` variable `api.prefix` is used (see [Multiple Parsers in the Same Program](#Multiple-Parsers)), for instance ‘`%define
    api.prefix {c}`’, then if `CDEBUG` is defined, its value controls the
    tracing feature (enabled if and only if nonzero); otherwise tracing is
    enabled if and only if `YYDEBUG` is nonzero.

    In C++, where POSIX compliance makes no sense, avoid this option, and prefer
    ‘`%define parse.trace`’. If you `#define` the `YYDEBUG` macro
    at the wrong place (e.g., in ‘`%code top`’ instead of ‘`%code
    require`’), the parser class will have two different definitions, thus
    leading to ODR violations and happy debugging times.

We suggest that you always enable the trace option so that debugging is
always possible.

In C the trace facility outputs messages with macro calls of the form
`YYFPRINTF (stderr, format, args)` where format and
args are the usual `printf` format and variadic arguments. If
you define `YYDEBUG` to a nonzero value but do not define
`YYFPRINTF`, `<stdio.h>` is automatically included and
`YYFPRINTF` is defined to `fprintf`.

Once you have compiled the program with trace facilities, the way to request
a trace is to store a nonzero value in the variable `yydebug`. You can
do this by making the C code do it (in `main`, perhaps), or you can
alter the value with a C debugger.

Each step taken by the parser when `yydebug` is nonzero produces a line
or two of trace information, written on `stderr`. The trace messages
tell you these things:

* Each time the parser calls `yylex`, what kind of token was read.
* Each time a token is shifted, the depth and complete contents of the state
  stack (see [Parser States](#Parser-States)).
* Each time a rule is reduced, which rule it is, and the complete contents of
  the state stack afterward.

To make sense of this information, it helps to refer to the automaton
description file (see [Understanding Your Parser](#Understanding)). This
file shows the meaning of each state in terms of positions in various rules,
and also what each state will do with each possible input token. As you
read the successive trace messages, you can see that the parser is
functioning according to its specification in the listing file. Eventually
you will arrive at the place where something undesirable happens, and you
will see which parts of the grammar are to blame.

The parser implementation file is a C/C++/D/Java program and you can use
debuggers on it, but it’s not easy to interpret what it is doing. The
parser function is a finite-state machine interpreter, and aside from the
actions it executes the same code over and over. Only the values of
variables show where in the grammar it is working.

---

Previous: [Enabling Traces](#Enabling-Traces), Up: [Tracing Your Parser](#Tracing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 8.5.2 Enabling Debug Traces for `mfcalc`

The debugging information normally gives the token kind of each token read,
but not its semantic value. The `%printer` directive allows specify
how semantic values are reported, see [Printing Semantic Values](#Printer-Decl).

As a demonstration of `%printer`, consider the multi-function
calculator, `mfcalc` (see [Multi-Function Calculator: `mfcalc`](#Multi_002dfunction-Calc)). To enable run-time
traces, and semantic value reports, insert the following directives in its
prologue:

```
/* Generate the parser description file. */
%verbose
/* Enable run-time traces (yydebug). */
%define parse.trace

/* Formatting semantic values. */
%printer { fprintf (yyo, "%s", $$->name); } VAR;
%printer { fprintf (yyo, "%s()", $$->name); } FUN;
%printer { fprintf (yyo, "%g", $$); } <double>;
```

The `%define` directive instructs Bison to generate run-time trace
support. Then, activation of these traces is controlled at run-time by the
`yydebug` variable, which is disabled by default. Because these traces
will refer to the “states” of the parser, it is helpful to ask for the
creation of a description of that parser; this is the purpose of (admittedly
ill-named) `%verbose` directive.

The set of `%printer` directives demonstrates how to format the
semantic value in the traces. Note that the specification can be done
either on the symbol type (e.g., `VAR` or `FUN`), or on the type
tag: since `<double>` is the type for both `NUM` and `exp`,
this printer will be used for them.

Here is a sample of the information provided by run-time traces. The traces
are sent onto standard error.

```
$ echo 'sin(1-1)' | ./mfcalc -p
Starting parse
Entering state 0
Reducing stack by rule 1 (line 34):
-> $$ = nterm input ()
Stack now 0
Entering state 1
```

This first batch shows a specific feature of this grammar: the first rule
(which is in line 34 of `mfcalc.y` can be reduced without even having
to look for the first token. The resulting left-hand symbol (`$$`) is
a valueless (‘`()`’) `input` nonterminal (`nterm`).

Then the parser calls the scanner.

```
Reading a token
Next token is token FUN (sin())
Shifting token FUN (sin())
Entering state 6
```

That token (`token`) is a function (`FUN`) whose value is
‘`sin`’ as formatted per our `%printer` specification: ‘`sin()`’.
The parser stores (`Shifting`) that token, and others, until it can do
something about it.

```
Reading a token
Next token is token '(' ()
Shifting token '(' ()
Entering state 14
Reading a token
Next token is token NUM (1.000000)
Shifting token NUM (1.000000)
Entering state 4
Reducing stack by rule 6 (line 44):
   $1 = token NUM (1.000000)
-> $$ = nterm exp (1.000000)
Stack now 0 1 6 14
Entering state 24
```

The previous reduction demonstrates the `%printer` directive for
`<double>`: both the token `NUM` and the resulting nonterminal
`exp` have ‘`1`’ as value.

```
Reading a token
Next token is token '-' ()
Shifting token '-' ()
Entering state 17
Reading a token
Next token is token NUM (1.000000)
Shifting token NUM (1.000000)
Entering state 4
Reducing stack by rule 6 (line 44):
   $1 = token NUM (1.000000)
-> $$ = nterm exp (1.000000)
Stack now 0 1 6 14 24 17
Entering state 26
Reading a token
Next token is token ')' ()
Reducing stack by rule 11 (line 49):
   $1 = nterm exp (1.000000)
   $2 = token '-' ()
   $3 = nterm exp (1.000000)
-> $$ = nterm exp (0.000000)
Stack now 0 1 6 14
Entering state 24
```

The rule for the subtraction was just reduced. The parser is about to
discover the end of the call to `sin`.

```
Next token is token ')' ()
Shifting token ')' ()
Entering state 31
Reducing stack by rule 9 (line 47):
   $1 = token FUN (sin())
   $2 = token '(' ()
   $3 = nterm exp (0.000000)
   $4 = token ')' ()
-> $$ = nterm exp (0.000000)
Stack now 0 1
Entering state 11
```

Finally, the end-of-line allow the parser to complete the computation, and
display its result.

```
Reading a token
Next token is token '\n' ()
Shifting token '\n' ()
Entering state 22
Reducing stack by rule 4 (line 40):
   $1 = nterm exp (0.000000)
   $2 = token '\n' ()
⇒ 0
-> $$ = nterm line ()
Stack now 0 1
Entering state 10
Reducing stack by rule 2 (line 35):
   $1 = nterm input ()
   $2 = nterm line ()
-> $$ = nterm input ()
Stack now 0
Entering state 1
```

The parser has returned into state 1, in which it is waiting for the next
expression to evaluate, or for the end-of-file token, which causes the
completion of the parsing.

```
Reading a token
Now at end of input.
Shifting token $end ()
Entering state 2
Stack now 0 1 2
Cleanup: popping token $end ()
Cleanup: popping nterm input ()
```

---

Next: [Parsers Written In Other Languages](#Other-Languages), Previous: [Debugging Your Parser](#Debugging), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 9 Invoking Bison

The usual way to invoke Bison is as follows:

```
$ bison file
```

Here file is the grammar file name, which usually ends in ‘`.y`’.
The parser implementation file’s name is made by replacing the ‘`.y`’
with ‘`.tab.c`’ and removing any leading directory. Thus, the
‘`bison foo.y`’ file name yields `foo.tab.c`, and the ‘`bison
hack/foo.y`’ file name yields `foo.tab.c`. It’s also possible, in case
you are writing C++ code instead of C in your grammar file, to name it
`foo.ypp` or `foo.y++`. Then, the output files will take an
extension like the given one as input (respectively `foo.tab.cpp` and
`foo.tab.c++`). This feature takes effect with all options that
manipulate file names like `-o` or `-d`.

For example:

```
$ bison -d file.yxx
```

will produce `file.tab.cxx` and `file.tab.hxx`, and

```
$ bison -d -o output.c++ file.y
```

will produce `output.c++` and `output.h++`.

For compatibility with POSIX, the standard Bison distribution also contains
a shell script called `yacc` that invokes Bison with the `-y`
option.

  

The exit status of `bison` is:

0 (success)
:   when there were no errors. Warnings, which are diagnostics about dubious
    constructs, do not change the exit status, unless they are turned into
    errors (see [`-Werror`](#Werror)).

1 (failure)
:   when there were errors. No file was generated (except the reports generated
    by `--verbose`, etc.). In particular, the output files that possibly
    existed were not changed.

63 (mismatch)
:   when `bison` does not meet the version requirements of the grammar
    file. See [Require a Version of Bison](#Require-Decl). No file was generated or changed.

* [Bison Options](#Bison-Options)
* [Option Cross Key](#Option-Cross-Key)
* [Yacc Library](#Yacc-Library)

---

Next: [Option Cross Key](#Option-Cross-Key), Up: [Invoking Bison](#Invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]
