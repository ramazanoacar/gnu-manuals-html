### 10.2 D Parsers

* [D Bison Interface](#D-Bison-Interface)
* [D Semantic Values](#D-Semantic-Values)
* [D Location Values](#D-Location-Values)
* [D Parser Interface](#D-Parser-Interface)
* [D Parser Context Interface](#D-Parser-Context-Interface)
* [D Scanner Interface](#D-Scanner-Interface)
* [Special Features for Use in D Actions](#D-Action-Features)
* [D Push Parser Interface](#D-Push-Parser-Interface)
* [D Complete Symbols](#D-Complete-Symbols)

---

Next: [D Semantic Values](#D-Semantic-Values), Up: [D Parsers](#D-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.2.1 D Bison Interface

The D parser skeletons are selected using the `%language "D"`
directive or the `-L D`/`--language=D` option.

When generating a D parser, ‘`bison basename.y`’ will create a
single D source file named `basename.d` containing the
parser implementation. Using a grammar file without a `.y` suffix is
currently broken. The basename of the parser implementation file can be
changed by the `%file-prefix` directive or the
`-b`/`--file-prefix` option. The entire parser implementation
file name can be changed by the `%output` directive or the
`-o`/`--output` option. The parser implementation file
contains a single class for the parser.

You can create documentation for generated parsers using Ddoc.

GLR parsers are currently unsupported in D. Do not use the
`glr-parser` directive.

No header file can be generated for D parsers. Do not use the
`%header` directive or the `-d`/`--header` options.

---

Next: [D Location Values](#D-Location-Values), Previous: [D Bison Interface](#D-Bison-Interface), Up: [D Parsers](#D-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.2.2 D Semantic Values

Semantic types are handled by `%union` and ‘`%define api.value.type
union`’, similar to C/C++ parsers. In the latter case, the union of the
values is handled by the backend. In D, unions can hold classes, structs,
etc., so this directive is more similar to ‘`%define api.value.type
variant`’ from C++.

D parsers do not support `%destructor`, since the language
adopts garbage collection. The parser will try to hold references
to semantic values for as little time as needed.

D parsers support `%printer`. An example for the output of type
`int`, where `yyo` is the parser’s debug output:

```
%printer { yyo.write($$); } <int>
```

---

Next: [D Parser Interface](#D-Parser-Interface), Previous: [D Semantic Values](#D-Semantic-Values), Up: [D Parsers](#D-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.2.3 D Location Values

When the directive `%locations` is used, the D parser supports location
tracking, see [Tracking Locations](#Tracking-Locations). The position and the location
structures are provided.

Instance Variable of Location: *Position* **begin** [¶](#index-begin-of-Location)

Instance Variable of Location: *Position* **end** [¶](#index-end-of-Location)
:   The first, inclusive, position of the range, and the first beyond.

Constructor on Location: **this(`Position`** *loc)* [¶](#index-this_0028Position-on-Location)
:   Create a `Location` denoting an empty range located at a given point.

Constructor on Location: **this(`Position`** *begin, `Position` end)* [¶](#index-this_0028Position-on-Location-1)
:   Create a `Location` from the endpoints of the range.

Method on Location: *string* **toString()** [¶](#index-toString_0028_0029-on-Location)
:   The range represented by the location as a string.

---

Next: [D Parser Context Interface](#D-Parser-Context-Interface), Previous: [D Location Values](#D-Location-Values), Up: [D Parsers](#D-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.2.4 D Parser Interface

The name of the generated parser class defaults to `YYParser`. The
`YY` prefix may be changed using the ‘`%define api.prefix`’.
Alternatively, use ‘`%define api.parser.class {name}`’ to give a
custom name to the class. The interface of this class is detailed below.

By default, the parser class has public visibility. To add modifiers to the
parser class, `%define` `api.parser.public`,
`api.parser.abstract` and/or `api.parser.final`.

The superclass and the implemented interfaces of the parser class can be
specified with the ‘`%define api.parser.extends`’ and ‘`%define
api.parser.implements`’ directives.

The parser class defines an interface, `Lexer` (see [D Scanner Interface](#D-Scanner-Interface)). Other than this interface and the members described in the
interface below, all the other members and fields are preceded with a
`yy` or `YY` prefix to avoid clashes with user code.

The parser class can be extended using the `%parse-param`
directive. Each occurrence of the directive will add a by default public
field to the parser class, and an argument to its constructor, which
initializes them automatically.

Constructor on YYParser: **this(lex\_param,** *…, parse\_param, …)* [¶](#index-this_0028lex_005fparam_002c-on-YYParser)
:   Build a new parser object with embedded ‘`%code lexer`’. There are no
    parameters, unless `%param`s and/or `%parse-param`s and/or
    `%lex-param`s are used.

Constructor on YYParser: **this(`Lexer`** *lexer, parse\_param, …)* [¶](#index-this_0028Lexer-on-YYParser)
:   Build a new parser object using the specified scanner. There are no
    additional parameters unless `%param`s and/or `%parse-param`s are
    used.

Method on YYParser: *boolean* **parse()** [¶](#index-parse_0028_0029-on-YYParser)
:   Run the syntactic analysis, and return `true` on success,
    `false` otherwise.

Method on YYParser: *boolean* **getErrorVerbose()** [¶](#index-getErrorVerbose_0028_0029-on-YYParser)

Method on YYParser: *void* **setErrorVerbose(boolean** *verbose)* [¶](#index-setErrorVerbose_0028boolean-on-YYParser)
:   Get or set the option to produce verbose error messages. These are only
    available with ‘`%define parse.error detailed`’,
    which also turns on verbose error messages.

Method on YYParser: *void* **yyerror(`string`** *msg)* [¶](#index-yyerror_0028string-on-YYParser)

Method on YYParser: *void* **yyerror(`Location`** *loc, `string` msg)* [¶](#index-yyerror_0028Location-on-YYParser)
:   Print an error message using the `yyerror` method of the scanner
    instance in use. The `Location` and `Position` parameters are
    available only if location tracking is active.

Method on YYParser: *boolean* **recovering()** [¶](#index-recovering_0028_0029-on-YYParser)
:   During the syntactic analysis, return `true` if recovering
    from a syntax error.
    See [Error Recovery](#Error-Recovery).

Method on YYParser: *File* **getDebugStream()** [¶](#index-getDebugStream_0028_0029-on-YYParser)

Method on YYParser: *void* **setDebugStream(`File`** *o)* [¶](#index-setDebugStream_0028File-on-YYParser)
:   Get or set the stream used for tracing the parsing. It defaults to
    `stderr`.

Method on YYParser: *int* **getDebugLevel()** [¶](#index-getDebugLevel_0028_0029-on-YYParser)

Method on YYParser: *void* **setDebugLevel(`int`** *l)* [¶](#index-setDebugLevel_0028int-on-YYParser)
:   Get or set the tracing level. Currently its value is either 0, no trace,
    or nonzero, full tracing.

Constant of YYParser: *string* **bisonVersion** [¶](#index-bisonVersion-of-YYParser)

Constant of YYParser: *string* **bisonSkeleton** [¶](#index-bisonSkeleton-of-YYParser)
:   Identify the Bison version and skeleton used to generate this parser.

The internationalization in D is very similar to the one in C. The D
parser uses `dgettext` for translating Bison messages.

To enable internationalization, compile using ‘`-version ENABLE_NLS
-version YYENABLE_NLS`’ and import `bindtextdomain` and
`textdomain` from C:

```
extern(C) char* bindtextdomain(const char* domainname, const char* dirname);
extern(C) char* textdomain(const char* domainname);
```

The main function should load the translation catalogs, similarly to the
`c/bistromathic` example:

```
int main()
{
  import core.stdc.locale;

  // Set up internationalization.
  setlocale(LC_ALL, "");
  // Use Bison's standard translation catalog for error messages
  // (the generated messages).
  bindtextdomain("bison-runtime", BISON_LOCALEDIR);
  // For the translation catalog of your own project, use the
  // name of your project.
  bindtextdomain("bison", LOCALEDIR);
  textdomain("bison");

  // usual main content
  ...
}
```

For user message translations, the user must implement the ‘`string
_(const char* msg)`’ function. It is recommended to use
`gettext`:

```
%code imports {
  static if (!is(typeof(_)))
  {
    version(ENABLE_NLS)
    {
      extern(C) char* gettext(const char*);
      string _(const char* s)
      {
        return to!string(gettext(s));
      }
    }
  }
  static if (!is(typeof(_)))
  {
    pragma(inline, true)
    string _(string msg) { return msg; }
  }
}
```

---

Next: [D Scanner Interface](#D-Scanner-Interface), Previous: [D Parser Interface](#D-Parser-Interface), Up: [D Parsers](#D-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.2.5 D Parser Context Interface

The parser context provides information to build error reports when you
invoke ‘`%define parse.error custom`’.

Type of YYParser: **SymbolKind** [¶](#index-SymbolKind)
:   A struct containing an enum of all the grammar symbols, tokens and
    nonterminals. Its enumerators are forged from the symbol names. Use
    ‘`void toString(W)(W sink)`’ to get the symbol names.

Method on YYParser.Context: *YYParser.SymbolKind* **getToken()** [¶](#index-getToken_0028_0029-on-YYParser_002eContext)
:   The kind of the lookahead. Return `null` iff there is no lookahead.

Method on YYParser.Context: *YYParser.Location* **getLocation()** [¶](#index-getLocation_0028_0029-on-YYParser_002eContext)
:   The location of the lookahead.

Method on YYParser.Context: *int* **getExpectedTokens(`YYParser.SymbolKind[]`** *argv, `int` argc)* [¶](#index-getExpectedTokens_0028YYParser_002eSymbolKind_005b_005d-on-YYParser_002eContext)
:   Fill argv with the expected tokens, which never includes
    `SymbolKind.YYERROR`, or `SymbolKind.YYUNDEF`.

    Never put more than argc elements into argv, and on success
    return the number of tokens stored in argv. If there are more
    expected tokens than argc, fill argv up to argc and return
    0. If there are no expected tokens, also return 0, but set `argv[0]`
    to `null`.

    If argv is null, return the size needed to store all the possible
    values, which is always less than `YYNTOKENS`.

---

Next: [Special Features for Use in D Actions](#D-Action-Features), Previous: [D Parser Context Interface](#D-Parser-Context-Interface), Up: [D Parsers](#D-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.2.6 D Scanner Interface

There are two possible ways to interface a Bison-generated D parser
with a scanner: the scanner may be defined by `%code lexer`, or
defined elsewhere. In either case, the scanner has to implement the
`Lexer` inner interface of the parser class. This interface also
contains constants for all user-defined token names and the predefined
`YYEOF` token.

In the first case, the body of the scanner class is placed in
`%code lexer` blocks. If you want to pass parameters from the
parser constructor to the scanner constructor, specify them with
`%lex-param`; they are passed before `%parse-param`s to the
constructor.

In the second case, the scanner has to implement the `Lexer` interface,
which is defined within the parser class (e.g., `YYParser.Lexer`).
The constructor of the parser object will then accept an object
implementing the interface; `%lex-param` is not used in this
case.

In both cases, the scanner has to implement the following methods.

Method on Lexer: *void* **yyerror(`Location`** *loc, `string` msg)* [¶](#index-yyerror_0028Location-on-Lexer)
:   This method is defined by the user to emit an error message. The first
    parameter is omitted if location tracking is not active.

Method on Lexer: *Symbol* **yylex()** [¶](#index-yylex_0028_0029-on-Lexer)
:   Return the next token. The return value is of type `Symbol`, which
    binds together the kind, the semantic value and the location.

Method on Lexer: *void* **reportSyntaxError(`YYParser.Context`** *ctx)* [¶](#index-reportSyntaxError_0028YYParser_002eContext-on-Lexer)
:   If you invoke ‘`%define parse.error custom`’ (see [The Bison Declarations Section](#Bison-Declarations)), then the parser no longer passes syntax error messages to
    `yyerror`, rather it delegates that task to the user by calling the
    `reportSyntaxError` function.

    Whether it uses `yyerror` is up to the user.

    Here is an example of a reporting function (see [D Parser Context Interface](#D-Parser-Context-Interface)).

    ```
    public void reportSyntaxError(YYParser.Context ctx)
    {
      stderr.write(ctx.getLocation(), ": syntax error");
      // Report the expected tokens.
      {
        immutable int TOKENMAX = 5;
        YYParser.SymbolKind[] arg = new YYParser.SymbolKind[TOKENMAX];
        int n = ctx.getExpectedTokens(arg, TOKENMAX);
        if (n < TOKENMAX)
          for (int i = 0; i < n; ++i)
            stderr.write((i == 0 ? ": expected " : " or "), arg[i]);
      }
      // Report the unexpected token which triggered the error.
      {
        YYParser.SymbolKind lookahead = ctx.getToken();
        stderr.writeln(" before ", lookahead);
      }
    }
    ```

    This implementation is inappropriate for internationalization, see
    the `c/bistromathic` example for a better alternative.

---

Next: [D Push Parser Interface](#D-Push-Parser-Interface), Previous: [D Scanner Interface](#D-Scanner-Interface), Up: [D Parsers](#D-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.2.7 Special Features for Use in D Actions

Here is a table of Bison constructs, variables and functions that are useful in
actions.

Variable: **$$** [¶](#index-_0024_0024-1)
:   Acts like a variable that contains the semantic value for the
    grouping made by the current rule. See [Actions](#Actions).

Variable: **$n** [¶](#index-_0024n-1)
:   Acts like a variable that contains the semantic value for the
    nth component of the current rule. See [Actions](#Actions).

Function: **yyerrok** [¶](#index-yyerrok-2)
:   Resume generating error messages immediately for subsequent syntax
    errors. This is useful primarily in error rules.
    See [Error Recovery](#Error-Recovery).

---

Next: [D Complete Symbols](#D-Complete-Symbols), Previous: [Special Features for Use in D Actions](#D-Action-Features), Up: [D Parsers](#D-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.2.8 D Push Parser Interface

Normally, Bison generates a pull parser for D.
The following Bison declaration says that you want the parser to be a push
parser (see [%define Summary](#g_t_0025define-Summary)):

```
%define api.push-pull push
```

Most of the discussion about the D pull Parser Interface, (see [D Parser Interface](#D-Parser-Interface)) applies to the push parser interface as well.

When generating a push parser, the method `pushParse` is created with
the following signature:

Method on YYParser: *int* **pushParse** *(`Symbol` sym)* [¶](#index-pushParse-on-YYParser)

The primary difference with respect to a pull parser is that the parser
method `pushParse` is invoked repeatedly to parse each token. This
function is available if either the ‘`%define api.push-pull push`’ or
‘`%define api.push-pull both`’ declaration is used (see [%define Summary](#g_t_0025define-Summary)).

The value returned by the `pushParse` method is one of the following:
`ACCEPT`, `ABORT`, or `PUSH_MORE`. This new value,
`PUSH_MORE`, may be returned if more input is required to finish
parsing the input.

If `api.push-pull` is defined as `both`, then the generated parser
class will also implement the `parse` method. This method’s body is a
loop that repeatedly invokes the scanner and then passes the values obtained
from the scanner to the `pushParse` method.

---

Previous: [D Push Parser Interface](#D-Push-Parser-Interface), Up: [D Parsers](#D-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.2.9 D Complete Symbols

To build return values for `yylex`, call the `Symbol` method of
the same name as the token kind reported, and adding the parameters for
value and location if necessary. These methods generate compile-time errors
if the parameters are inconsistent. Token constructors work with both
`%union` and ‘`%define api.value.type union`’.

The order of the parameters is the same as for the `Symbol`
constructor. An example for the token kind `NUM`, which has value
`ival` and with location tracking activated:

```
Symbol.NUM(ival, location);
```

---

Previous: [D Parsers](#D-Parsers), Up: [Parsers Written In Other Languages](#Other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 10.3 Java Parsers

* [Java Bison Interface](#Java-Bison-Interface)
* [Java Semantic Values](#Java-Semantic-Values)
* [Java Location Values](#Java-Location-Values)
* [Java Parser Interface](#Java-Parser-Interface)
* [Java Parser Context Interface](#Java-Parser-Context-Interface)
* [Java Scanner Interface](#Java-Scanner-Interface)
* [Special Features for Use in Java Actions](#Java-Action-Features)
* [Java Push Parser Interface](#Java-Push-Parser-Interface)
* [Differences between C/C++ and Java Grammars](#Java-Differences)
* [Java Declarations Summary](#Java-Declarations-Summary)

---

Next: [Java Semantic Values](#Java-Semantic-Values), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.1 Java Bison Interface

The Java parser skeletons are selected using the `%language "Java"`
directive or the `-L java`/`--language=java` option.

When generating a Java parser, ‘`bison basename.y`’ will create a
single Java source file named `basename.java` containing the
parser implementation. Using a grammar file without a `.y` suffix is
currently broken. The basename of the parser implementation file can be
changed by the `%file-prefix` directive or the
`-b`/`--file-prefix` option. The entire parser implementation
file name can be changed by the `%output` directive or the
`-o`/`--output` option. The parser implementation file
contains a single class for the parser.

You can create documentation for generated parsers using Javadoc.

Contrary to C parsers, Java parsers do not use global variables; the state
of the parser is always local to an instance of the parser class.
Therefore, all Java parsers are “pure”, and the `%define api.pure`
directive does nothing when used in Java.

GLR parsers are currently unsupported in Java. Do not use the
`glr-parser` directive.

No header file can be generated for Java parsers. Do not use the
`%header` directive or the `-d`/`-H`/`--header`
options.

Currently, support for tracing is always compiled in. Thus the
‘`%define parse.trace`’ and ‘`%token-table`’ directives and the
`-t`/`--debug` and `-k`/`--token-table` options
have no effect. This may change in the future to eliminate unused code in
the generated parser, so use ‘`%define parse.trace`’ explicitly if
needed. Also, in the future the `%token-table` directive might enable
a public interface to access the token names and codes.

Getting a “code too large” error from the Java compiler means the code hit
the 64KB bytecode per method limitation of the Java class file. Try
reducing the amount of code in actions and static initializers; otherwise,
report a bug so that the parser skeleton will be improved.

---

Next: [Java Location Values](#Java-Location-Values), Previous: [Java Bison Interface](#Java-Bison-Interface), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.2 Java Semantic Values

There is no `%union` directive in Java parsers. Instead, the semantic
values’ types (class names) should be specified in the `%nterm` or
`%token` directive:

```
%nterm <Expression> expr assignment_expr term factor
%nterm <Integer> number
```

By default, the semantic stack is declared to have `Object` members,
which means that the class types you specify can be of any class.
To improve the type safety of the parser, you can declare the common
superclass of all the semantic values using the ‘`%define api.value.type`’
directive. For example, after the following declaration:

```
%define api.value.type {ASTNode}
```

any `%token`, `%nterm` or `%type` specifying a semantic type
which is not a subclass of `ASTNode`, will cause a compile-time error.

Types used in the directives may be qualified with a package name.
Primitive data types are accepted for Java version 1.5 or later. Note
that in this case the autoboxing feature of Java 1.5 will be used.
Generic types may not be used; this is due to a limitation in the
implementation of Bison, and may change in future releases.

Java parsers do not support `%destructor`, since the language
adopts garbage collection. The parser will try to hold references
to semantic values for as little time as needed.

Java parsers do not support `%printer`, as `toString()`
can be used to print the semantic values. This however may change
(in a backwards-compatible way) in future versions of Bison.

---

Next: [Java Parser Interface](#Java-Parser-Interface), Previous: [Java Semantic Values](#Java-Semantic-Values), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.3 Java Location Values

When the directive `%locations` is used, the Java parser supports
location tracking, see [Tracking Locations](#Tracking-Locations). An auxiliary user-defined
class defines a *position*, a single point in a file; Bison itself
defines a class representing a *location*, a range composed of a pair of
positions (possibly spanning several files). The location class is an inner
class of the parser; the name is `Location` by default, and may also be
renamed using `%define api.location.type {class-name}`.

The location class treats the position as a completely opaque value.
By default, the class name is `Position`, but this can be changed
with `%define api.position.type {class-name}`. This class must
be supplied by the user.

Instance Variable of Location: *Position* **begin** [¶](#index-begin-of-Location-1)

Instance Variable of Location: *Position* **end** [¶](#index-end-of-Location-1)
:   The first, inclusive, position of the range, and the first beyond.

Constructor on Location: **Location** *(`Position` loc)* [¶](#index-Location-on-Location)
:   Create a `Location` denoting an empty range located at a given point.

Constructor on Location: **Location** *(`Position` begin, `Position` end)* [¶](#index-Location-on-Location-1)
:   Create a `Location` from the endpoints of the range.

Method on Location: *String* **toString** *()* [¶](#index-toString-on-Location)
:   Prints the range represented by the location. For this to work
    properly, the position class should override the `equals` and
    `toString` methods appropriately.

---

Next: [Java Parser Context Interface](#Java-Parser-Context-Interface), Previous: [Java Location Values](#Java-Location-Values), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.4 Java Parser Interface

The name of the generated parser class defaults to `YYParser`. The
`YY` prefix may be changed using the ‘`%define api.prefix`’.
Alternatively, use ‘`%define api.parser.class {name}`’ to give a
custom name to the class. The interface of this class is detailed below.

By default, the parser class has package visibility. A declaration
‘`%define api.parser.public`’ will change to public visibility. Remember
that, according to the Java language specification, the name of the
`.java` file should match the name of the class in this case.
Similarly, you can use `api.parser.abstract`, `api.parser.final`
and `api.parser.strictfp` with the `%define` declaration to add
other modifiers to the parser class. A single ‘`%define
api.parser.annotations {annotations}`’ directive can be used to add
any number of annotations to the parser class.

The Java package name of the parser class can be specified using the
‘`%define package`’ directive. The superclass and the implemented
interfaces of the parser class can be specified with the `%define
api.parser.extends` and ‘`%define api.parser.implements`’ directives.

The parser class defines an inner class, `Location`, that is used
for location tracking (see [Java Location Values](#Java-Location-Values)), and a inner
interface, `Lexer` (see [Java Scanner Interface](#Java-Scanner-Interface)). Other than
these inner class/interface, and the members described in the interface
below, all the other members and fields are preceded with a `yy` or
`YY` prefix to avoid clashes with user code.

The parser class can be extended using the `%parse-param`
directive. Each occurrence of the directive will add a `protected
final` field to the parser class, and an argument to its constructor,
which initializes them automatically.

Constructor on YYParser: **YYParser** *(lex\_param, …, parse\_param, …)* [¶](#index-YYParser-on-YYParser)
:   Build a new parser object with embedded `%code lexer`. There are
    no parameters, unless `%param`s and/or `%parse-param`s and/or
    `%lex-param`s are used.

    Use `%code init` for code added to the start of the constructor
    body. This is especially useful to initialize superclasses. Use
    ‘`%define init_throws`’ to specify any uncaught exceptions.

Constructor on YYParser: **YYParser** *(`Lexer` lexer, parse\_param, …)* [¶](#index-YYParser-on-YYParser-1)
:   Build a new parser object using the specified scanner. There are no
    additional parameters unless `%param`s and/or `%parse-param`s are
    used.

    If the scanner is defined by `%code lexer`, this constructor is
    declared `protected` and is called automatically with a scanner
    created with the correct `%param`s and/or `%lex-param`s.

    Use `%code init` for code added to the start of the constructor
    body. This is especially useful to initialize superclasses. Use
    ‘`%define init_throws`’ to specify any uncaught exceptions.

Method on YYParser: *boolean* **parse** *()* [¶](#index-parse-on-YYParser)
:   Run the syntactic analysis, and return `true` on success,
    `false` otherwise.

Method on YYParser: *boolean* **getErrorVerbose** *()* [¶](#index-getErrorVerbose-on-YYParser)

Method on YYParser: *void* **setErrorVerbose** *(boolean verbose)* [¶](#index-setErrorVerbose-on-YYParser)
:   Get or set the option to produce verbose error messages. These are only
    available with ‘`%define parse.error detailed`’ (or ‘`verbose`’),
    which also turns on verbose error messages.

Method on YYParser: *void* **yyerror** *(`String` msg)* [¶](#index-yyerror-on-YYParser)

Method on YYParser: *void* **yyerror** *(`Position` pos, `String` msg)* [¶](#index-yyerror-on-YYParser-1)

Method on YYParser: *void* **yyerror** *(`Location` loc, `String` msg)* [¶](#index-yyerror-on-YYParser-2)
:   Print an error message using the `yyerror` method of the scanner
    instance in use. The `Location` and `Position` parameters are
    available only if location tracking is active.

Method on YYParser: *boolean* **recovering** *()* [¶](#index-recovering-on-YYParser)
:   During the syntactic analysis, return `true` if recovering
    from a syntax error.
    See [Error Recovery](#Error-Recovery).

Method on YYParser: *java.io.PrintStream* **getDebugStream** *()* [¶](#index-getDebugStream-on-YYParser)

Method on YYParser: *void* **setDebugStream** *(`java.io.PrintStream` o)* [¶](#index-setDebugStream-on-YYParser)
:   Get or set the stream used for tracing the parsing. It defaults to
    `System.err`.

Method on YYParser: *int* **getDebugLevel** *()* [¶](#index-getDebugLevel-on-YYParser)

Method on YYParser: *void* **setDebugLevel** *(`int` l)* [¶](#index-setDebugLevel-on-YYParser)
:   Get or set the tracing level. Currently its value is either 0, no trace,
    or nonzero, full tracing.

Constant of YYParser: *String* **bisonVersion** [¶](#index-bisonVersion-of-YYParser-1)

Constant of YYParser: *String* **bisonSkeleton** [¶](#index-bisonSkeleton-of-YYParser-1)
:   Identify the Bison version and skeleton used to generate this parser.

If you enabled token internationalization (see [Token Internationalization](#Token-I18n)), you must
provide the parser with the following function:

Static Method of YYParser: *String* **i18n** *(`string` s)* [¶](#index-i18n-of-YYParser)
:   Return the translation of s in the user’s language. As an example:

    ```
    %code {
      static ResourceBundle myResources
        = ResourceBundle.getBundle("domain-name");
      static final String i18n(String s) {
        return myResources.getString(s);
      }
    }
    ```

---

Next: [Java Scanner Interface](#Java-Scanner-Interface), Previous: [Java Parser Interface](#Java-Parser-Interface), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.5 Java Parser Context Interface

The parser context provides information to build error reports when you
invoke ‘`%define parse.error custom`’.

Type of YYParser: **SymbolKind** [¶](#index-SymbolKind-1)
:   An enum of all the grammar symbols, tokens and nonterminals. Its
    enumerators are forged from the symbol names:

    ```
    public enum SymbolKind
    {
      S_YYEOF(0),          /* "end of file"  */
      S_YYERROR(1),        /* error  */
      S_YYUNDEF(2),        /* "invalid token"  */
      S_BANG(3),           /* "!"  */
      S_PLUS(4),           /* "+"  */
      S_MINUS(5),          /* "-"  */
      [...]
      S_NUM(13),           /* "number"  */
      S_NEG(14),           /* NEG  */
      S_YYACCEPT(15),      /* $accept  */
      S_input(16),         /* input  */
      S_line(17);          /* line  */
    };
    ```

Method on YYParser.SymbolKind: *String* **getName** *()* [¶](#index-getName-on-YYParser_002eSymbolKind)
:   The name of this symbol, possibly translated.

Method on YYParser.Context: *YYParser.SymbolKind* **getToken** *()* [¶](#index-getToken-on-YYParser_002eContext)
:   The kind of the lookahead. Return `null` iff there is no lookahead.

Method on YYParser.Context: *YYParser.Location* **getLocation** *()* [¶](#index-getLocation-on-YYParser_002eContext)
:   The location of the lookahead.

Method on YYParser.Context: *int* **getExpectedTokens** *(`YYParser.SymbolKind[]` argv, `int` argc)* [¶](#index-getExpectedTokens-on-YYParser_002eContext)
:   Fill argv with the expected tokens, which never includes
    `SymbolKind.S_YYERROR`, or `SymbolKind.S_YYUNDEF`.

    Never put more than argc elements into argv, and on success
    return the number of tokens stored in argv. If there are more
    expected tokens than argc, fill argv up to argc and return
    0. If there are no expected tokens, also return 0, but set `argv[0]`
    to `null`.

    If argv is null, return the size needed to store all the possible
    values, which is always less than `YYNTOKENS`.

---

Next: [Special Features for Use in Java Actions](#Java-Action-Features), Previous: [Java Parser Context Interface](#Java-Parser-Context-Interface), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.6 Java Scanner Interface

There are two possible ways to interface a Bison-generated Java parser
with a scanner: the scanner may be defined by `%code lexer`, or
defined elsewhere. In either case, the scanner has to implement the
`Lexer` inner interface of the parser class. This interface also
contains constants for all user-defined token names and the predefined
`YYEOF` token.

In the first case, the body of the scanner class is placed in
`%code lexer` blocks. If you want to pass parameters from the
parser constructor to the scanner constructor, specify them with
`%lex-param`; they are passed before `%parse-param`s to the
constructor.

In the second case, the scanner has to implement the `Lexer` interface,
which is defined within the parser class (e.g., `YYParser.Lexer`).
The constructor of the parser object will then accept an object
implementing the interface; `%lex-param` is not used in this
case.

In both cases, the scanner has to implement the following methods.

Method on Lexer: *void* **yyerror** *(`Location` loc, `String` msg)* [¶](#index-yyerror-on-Lexer)
:   This method is defined by the user to emit an error message. The first
    parameter is omitted if location tracking is not active. Its type can be
    changed using `%define api.location.type {class-name}`.

Method on Lexer: *int* **yylex** *()* [¶](#index-yylex-on-Lexer)
:   Return the next token. Its type is the return value, its semantic value and
    location are saved and returned by the their methods in the interface. Not
    needed for push-only parsers.

    Use ‘`%define lex_throws`’ to specify any uncaught exceptions.
    Default is `java.io.IOException`.

Method on Lexer: *Position* **getStartPos** *()* [¶](#index-getStartPos-on-Lexer)

Method on Lexer: *Position* **getEndPos** *()* [¶](#index-getEndPos-on-Lexer)
:   Return respectively the first position of the last token that `yylex`
    returned, and the first position beyond it. These methods are not needed
    unless location tracking and pull parsing are active.

    They should return new objects for each call, to avoid that all the symbol
    share the same Position boundaries.

    The return type can be changed using `%define api.position.type
    {class-name}`.

Method on Lexer: *Object* **getLVal** *()* [¶](#index-getLVal-on-Lexer)
:   Return the semantic value of the last token that yylex returned. Not needed
    for push-only parsers.

    The return type can be changed using ‘`%define api.value.type
    {class-name}`’.

Method on Lexer: *void* **reportSyntaxError** *(`YYParser.Context` ctx)* [¶](#index-reportSyntaxError-on-Lexer)
:   If you invoke ‘`%define parse.error custom`’ (see [The Bison Declarations Section](#Bison-Declarations)), then the parser no longer passes syntax error messages to
    `yyerror`, rather it delegates that task to the user by calling the
    `reportSyntaxError` function.

    Whether it uses `yyerror` is up to the user.

    Here is an example of a reporting function (see [Java Parser Context Interface](#Java-Parser-Context-Interface)).

    ```
    public void reportSyntaxError(YYParser.Context ctx) {
      System.err.print(ctx.getLocation() + ": syntax error");
      // Report the expected tokens.
      {
        final int TOKENMAX = 5;
        YYParser.SymbolKind[] arg = new YYParser.SymbolKind[TOKENMAX];
        int n = ctx.getExpectedTokens(arg, TOKENMAX);
        for (int i = 0; i < n; ++i)
          System.err.print((i == 0 ? ": expected " : " or ")
                           + arg[i].getName());
      }
      // Report the unexpected token which triggered the error.
      {
        YYParser.SymbolKind lookahead = ctx.getToken();
        if (lookahead != null)
          System.err.print(" before " + lookahead.getName());
      }
      System.err.println("");
    }
    ```

    This implementation is inappropriate for internationalization, see the
    `c/bistromathic` example for a better alternative.

---

Next: [Java Push Parser Interface](#Java-Push-Parser-Interface), Previous: [Java Scanner Interface](#Java-Scanner-Interface), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.7 Special Features for Use in Java Actions

The following special constructs can be uses in Java actions.
Other analogous C action features are currently unavailable for Java.

Use ‘`%define throws`’ to specify any uncaught exceptions from parser
actions, and initial actions specified by `%initial-action`.

Variable: **$n** [¶](#index-_0024n-4)
:   The semantic value for the nth component of the current rule.
    This may not be assigned to.
    See [Java Semantic Values](#Java-Semantic-Values).

Variable: **$<typealt>n** [¶](#index-_0024_003ctypealt_003en-1)
:   Like `$n` but specifies a alternative type typealt.
    See [Java Semantic Values](#Java-Semantic-Values).

Variable: **$$** [¶](#index-_0024_0024-4)
:   The semantic value for the grouping made by the current rule. As a
    value, this is in the base type (`Object` or as specified by
    ‘`%define api.value.type`’) as in not cast to the declared subtype because
    casts are not allowed on the left-hand side of Java assignments.
    Use an explicit Java cast if the correct subtype is needed.
    See [Java Semantic Values](#Java-Semantic-Values).

Variable: **$<typealt>$** [¶](#index-_0024_003ctypealt_003e_0024-1)
:   Same as `$$` since Java always allow assigning to the base type.
    Perhaps we should use this and `$<>$` for the value and `$$`
    for setting the value but there is currently no easy way to distinguish
    these constructs.
    See [Java Semantic Values](#Java-Semantic-Values).

Variable: **@n** [¶](#index-_0040n-6)
:   The location information of the nth component of the current rule.
    This may not be assigned to.
    See [Java Location Values](#Java-Location-Values).

Variable: **@$** [¶](#index-_0040_0024-3)
:   The location information of the grouping made by the current rule.
    See [Java Location Values](#Java-Location-Values).

Statement: *return* **YYABORT** *`;`* [¶](#index-YYABORT-3)
:   Return immediately from the parser, indicating failure.
    See [Java Parser Interface](#Java-Parser-Interface).

Statement: *return* **YYACCEPT** *`;`* [¶](#index-YYACCEPT-3)
:   Return immediately from the parser, indicating success.
    See [Java Parser Interface](#Java-Parser-Interface).

Statement: *return* **YYERROR** *`;`* [¶](#index-YYERROR-2)
:   Start error recovery (without printing an error message).
    See [Error Recovery](#Error-Recovery).

Function: *boolean* **recovering** *()* [¶](#index-recovering)
:   Return whether error recovery is being done. In this state, the parser
    reads token until it reaches a known state, and then restarts normal
    operation.
    See [Error Recovery](#Error-Recovery).

Function: *void* **yyerror** *(`String` msg)* [¶](#index-yyerror-1)

Function: *void* **yyerror** *(`Position` loc, `String` msg)* [¶](#index-yyerror-2)

Function: *void* **yyerror** *(`Location` loc, `String` msg)* [¶](#index-yyerror-3)
:   Print an error message using the `yyerror` method of the scanner
    instance in use. The `Location` and `Position` parameters are
    available only if location tracking is active.

---

Next: [Differences between C/C++ and Java Grammars](#Java-Differences), Previous: [Special Features for Use in Java Actions](#Java-Action-Features), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.8 Java Push Parser Interface

Normally, Bison generates a pull parser for Java.
The following Bison declaration says that you want the parser to be a push
parser (see [%define Summary](#g_t_0025define-Summary)):

```
%define api.push-pull push
```

Most of the discussion about the Java pull Parser Interface, (see [Java Parser Interface](#Java-Parser-Interface)) applies to the push parser interface as well.

When generating a push parser, the method `push_parse` is created with
the following signature (depending on if locations are enabled).

Method on YYParser: *void* **push\_parse** *(`int` token, `Object` yylval)* [¶](#index-push_005fparse-on-YYParser)

Method on YYParser: *void* **push\_parse** *(`int` token, `Object` yylval, `Location` yyloc)* [¶](#index-push_005fparse-on-YYParser-1)

Method on YYParser: *void* **push\_parse** *(`int` token, `Object` yylval, `Position` yypos)* [¶](#index-push_005fparse-on-YYParser-2)

The primary difference with respect to a pull parser is that the parser
method `push_parse` is invoked repeatedly to parse each token. This
function is available if either the ‘`%define api.push-pull push`’ or
‘`%define api.push-pull both`’ declaration is used (see [%define Summary](#g_t_0025define-Summary)). The `Location` and `Position` parameters are available
only if location tracking is active.

The value returned by the `push_parse` method is one of the following:
0 (success), 1 (abort), 2 (memory exhaustion), or `YYPUSH_MORE`. This
new value, `YYPUSH_MORE`, may be returned if more input is required to
finish parsing the grammar.

If `api.push-pull` is defined as `both`, then the generated parser
class will also implement the `parse` method. This method’s body is a
loop that repeatedly invokes the scanner and then passes the values obtained
from the scanner to the `push_parse` method.

There is one additional complication. Technically, the push parser does not
need to know about the scanner (i.e. an object implementing the
`YYParser.Lexer` interface), but it does need access to the
`yyerror` method. Currently, the `yyerror` method is defined in
the `YYParser.Lexer` interface. Hence, an implementation of that
interface is still required in order to provide an implementation of
`yyerror`. The current approach (and subject to change) is to require
the `YYParser` constructor to be given an object implementing the
`YYParser.Lexer` interface. This object need only implement the
`yyerror` method; the other methods can be stubbed since they will
never be invoked. The simplest way to do this is to add a trivial scanner
implementation to your grammar file using whatever implementation of
`yyerror` is desired. The following code sample shows a simple way to
accomplish this.

```
%code lexer
{
  public Object getLVal () {return null;}
  public int yylex () {return 0;}
  public void yyerror (String s) {System.err.println(s);}
}
```

---

Next: [Java Declarations Summary](#Java-Declarations-Summary), Previous: [Java Push Parser Interface](#Java-Push-Parser-Interface), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.9 Differences between C/C++ and Java Grammars

The different structure of the Java language forces several differences
between C/C++ grammars, and grammars designed for Java parsers. This
section summarizes these differences.

* Java has no a preprocessor, so obviously the `YYERROR`,
  `YYACCEPT`, `YYABORT` symbols (see [Bison Symbols](#Table-of-Symbols)) cannot be
  macros. Instead, they should be preceded by `return` when they appear
  in an action. The actual definition of these symbols is opaque to the Bison
  grammar, and it might change in the future. The only meaningful operation
  that you can do, is to return them. See [Special Features for Use in Java Actions](#Java-Action-Features).

  Note that of these three symbols, only `YYACCEPT` and
  `YYABORT` will cause a return from the `yyparse`
  method[8](#FOOT8).
* Java lacks unions, so `%union` has no effect. Instead, semantic
  values have a common base type: `Object` or as specified by
  ‘`%define api.value.type`’. Angle brackets on `%token`, `type`,
  `$n` and `$$` specify subtypes rather than fields of
  an union. The type of `$$`, even with angle brackets, is the base
  type since Java casts are not allow on the left-hand side of assignments.
  Also, `$n` and `@n` are not allowed on the
  left-hand side of assignments. See [Java Semantic Values](#Java-Semantic-Values), and
  [Special Features for Use in Java Actions](#Java-Action-Features).
* The prologue declarations have a different meaning than in C/C++ code.

  `%code imports`
  :   blocks are placed at the beginning of the Java source code. They may
      include copyright notices. For a `package` declarations, use
      ‘`%define api.package`’ instead.

  unqualified `%code`
  :   blocks are placed inside the parser class.

  `%code lexer`
  :   blocks, if specified, should include the implementation of the
      scanner. If there is no such block, the scanner can be any class
      that implements the appropriate interface (see [Java Scanner Interface](#Java-Scanner-Interface)).

  Other `%code` blocks are not supported in Java parsers.
  In particular, `%{ … %}` blocks should not be used
  and may give an error in future versions of Bison.

  The epilogue has the same meaning as in C/C++ code and it can
  be used to define other classes used by the parser *outside*
  the parser class.

---

Previous: [Differences between C/C++ and Java Grammars](#Java-Differences), Up: [Java Parsers](#Java-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.3.10 Java Declarations Summary

This summary only include declarations specific to Java or have special
meaning when used in a Java parser.

Directive: **%language "Java"** [¶](#index-_0025language-_0022Java_0022)
:   Generate a Java class for the parser.

Directive: **%lex-param** *{type name}* [¶](#index-_0025lex_002dparam-2)
:   A parameter for the lexer class defined by `%code lexer`
    *only*, added as parameters to the lexer constructor and the parser
    constructor that *creates* a lexer. Default is none.
    See [Java Scanner Interface](#Java-Scanner-Interface).

Directive: **%parse-param** *{type name}* [¶](#index-_0025parse_002dparam-2)
:   A parameter for the parser class added as parameters to constructor(s)
    and as fields initialized by the constructor(s). Default is none.
    See [Java Parser Interface](#Java-Parser-Interface).

Directive: **%token** *<type> token …* [¶](#index-_0025token-3)
:   Declare tokens. Note that the angle brackets enclose a Java *type*.
    See [Java Semantic Values](#Java-Semantic-Values).

Directive: **%nterm** *<type> nonterminal …* [¶](#index-_0025nterm-3)
:   Declare the type of nonterminals. Note that the angle brackets enclose
    a Java *type*.
    See [Java Semantic Values](#Java-Semantic-Values).

Directive: **%code** *{ code … }* [¶](#index-_0025code-8)
:   Code appended to the inside of the parser class.
    See [Differences between C/C++ and Java Grammars](#Java-Differences).

Directive: **%code imports** *{ code … }* [¶](#index-_0025code-imports-1)
:   Code inserted just after the `package` declaration.
    See [Differences between C/C++ and Java Grammars](#Java-Differences).

Directive: **%code init** *{ code … }* [¶](#index-_0025code-init)
:   Code inserted at the beginning of the parser constructor body.
    See [Java Parser Interface](#Java-Parser-Interface).

Directive: **%code lexer** *{ code … }* [¶](#index-_0025code-lexer)
:   Code added to the body of a inner lexer class within the parser class.
    See [Java Scanner Interface](#Java-Scanner-Interface).

Directive: **%%** *code …* [¶](#index-_0025_0025)
:   Code (after the second `%%`) appended to the end of the file,
    *outside* the parser class.
    See [Differences between C/C++ and Java Grammars](#Java-Differences).

Directive: **%{** *code … %}* [¶](#index-_0025_007b)
:   Not supported. Use `%code imports` instead.
    See [Differences between C/C++ and Java Grammars](#Java-Differences).

Directive: **%define api.prefix** *{prefix}* [¶](#index-_0025define-api_002eprefix-1)
:   The prefix of the parser class name `prefixParser` if
    ‘`%define api.parser.class`’ is not used. Default is `YY`.
    See [Java Bison Interface](#Java-Bison-Interface).

Directive: **%define api.parser.abstract** [¶](#index-_0025define-api_002eparser_002eabstract)
:   Whether the parser class is declared `abstract`. Default is false.
    See [Java Bison Interface](#Java-Bison-Interface).

Directive: **%define api.parser.annotations** *{annotations}* [¶](#index-_0025define-api_002eparser_002eannotations)
:   The Java annotations for the parser class. Default is none.
    See [Java Bison Interface](#Java-Bison-Interface).

Directive: **%define api.parser.class** *{name}* [¶](#index-_0025define-api_002eparser_002eclass-1)
:   The name of the parser class. Default is `YYParser` or
    `api.prefixParser`. See [Java Bison Interface](#Java-Bison-Interface).

Directive: **%define api.parser.extends** *{superclass}* [¶](#index-_0025define-api_002eparser_002eextends)
:   The superclass of the parser class. Default is none.
    See [Java Bison Interface](#Java-Bison-Interface).

Directive: **%define api.parser.final** [¶](#index-_0025define-api_002eparser_002efinal)
:   Whether the parser class is declared `final`. Default is false.
    See [Java Bison Interface](#Java-Bison-Interface).

Directive: **%define api.parser.implements** *{interfaces}* [¶](#index-_0025define-api_002eparser_002eimplements)
:   The implemented interfaces of the parser class, a comma-separated list.
    Default is none.
    See [Java Bison Interface](#Java-Bison-Interface).

Directive: **%define api.parser.public** [¶](#index-_0025define-api_002eparser_002epublic)
:   Whether the parser class is declared `public`. Default is false.
    See [Java Bison Interface](#Java-Bison-Interface).

Directive: **%define api.parser.strictfp** [¶](#index-_0025define-api_002eparser_002estrictfp)
:   Whether the parser class is declared `strictfp`. Default is false.
    See [Java Bison Interface](#Java-Bison-Interface).

Directive: **%define init\_throws** *{exceptions}* [¶](#index-_0025define-init_005fthrows)
:   The exceptions thrown by `%code init` from the parser class
    constructor. Default is none.
    See [Java Parser Interface](#Java-Parser-Interface).

Directive: **%define lex\_throws** *{exceptions}* [¶](#index-_0025define-lex_005fthrows)
:   The exceptions thrown by the `yylex` method of the lexer, a
    comma-separated list. Default is `java.io.IOException`.
    See [Java Scanner Interface](#Java-Scanner-Interface).

Directive: **%define api.location.type** *{class}* [¶](#index-_0025define-api_002elocation_002etype-2)
:   The name of the class used for locations (a range between two
    positions). This class is generated as an inner class of the parser
    class by `bison`. Default is `Location`.
    Formerly named `location_type`.
    See [Java Location Values](#Java-Location-Values).

Directive: **%define api.package** *{package}* [¶](#index-_0025define-api_002epackage)
:   The package to put the parser class in. Default is none.
    See [Java Bison Interface](#Java-Bison-Interface).
    Renamed from `package` in Bison 3.7.

Directive: **%define api.position.type** *{class}* [¶](#index-_0025define-api_002eposition_002etype)
:   The name of the class used for positions. This class must be supplied by
    the user. Default is `Position`.
    Formerly named `position_type`.
    See [Java Location Values](#Java-Location-Values).

Directive: **%define api.value.type** *{class}* [¶](#index-_0025define-api_002evalue_002etype-2)
:   The base type of semantic values. Default is `Object`.
    See [Java Semantic Values](#Java-Semantic-Values).

Directive: **%define throws** *{exceptions}* [¶](#index-_0025define-throws)
:   The exceptions thrown by user-supplied parser actions and
    `%initial-action`, a comma-separated list. Default is none.
    See [Java Parser Interface](#Java-Parser-Interface).

---

Next: [Bison Version Compatibility: Best Practices](#Versioning), Previous: [Parsers Written In Other Languages](#Other-Languages), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 11 A Brief History of the Greater Ungulates

* [The ancestral Yacc](#Yacc)
* [yacchack](#yacchack)
* [Berkeley Yacc](#Byacc)
* [Bison](#Bison)
* [Other Ungulates](#Other-Ungulates)

---

Next: [yacchack](#yacchack), Up: [A Brief History of the Greater Ungulates](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 11.1 The ancestral Yacc

Bison originated as a workalike of a program called Yacc — Yet Another
Compiler Compiler.[9](#FOOT9) Yacc was written at Bell Labs as part of the very early
development of Unix; one of its first uses was to develop the original
Portable C Compiler, pcc. The same person, Steven C. Johnson, wrote Yacc and
the original pcc.

According to the author
[10](#FOOT10),
Yacc was first invented in 1971 and reached a form recognizably similar to
the C version in 1973. Johnson published A Portable Compiler: Theory
and Practice (see [Johnson 1978](#Johnson-1978)).

Yacc was not itself originally written in C but in its predecessor language,
B. This goes far to explain its odd interface, which exposes a large number
of global variables rather than bundling them into a C struct. All other
Yacc-like programs are descended from the C port of Yacc.

Yacc, through both its deployment in pcc and as a standalone tool for
generating other parsers, helped drive the early spread of Unix. Yacc
itself, however, passed out of use after around 1990 when workalikes
with less restrictive licenses and more features became available.

Original Yacc became generally available when Caldera released the sources
of old versions of Unix up to V7 and 32V in 2002. By that time it had been
long superseded in practical use by Bison even on Yacc’s native Unix
variants.

---

Next: [Berkeley Yacc](#Byacc), Previous: [The ancestral Yacc](#Yacc), Up: [A Brief History of the Greater Ungulates](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 11.2 yacchack

One of the deficiencies of original Yacc was its inability to produce
reentrant parsers. This was first remedied by a set of drop-in
modifications called “yacchack”, published by Eric S. Raymond on USENET
around 1983. This code was quickly forgotten when zoo and Berkeley Yacc
became available a few years later.

---

Next: [Bison](#Bison), Previous: [yacchack](#yacchack), Up: [A Brief History of the Greater Ungulates](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 11.3 Berkeley Yacc

Berkeley Yacc was originated in 1985 by Robert Corbett (see [Corbett 1984](#Corbett-1984)). It was originally named “zoo”, but by October 1989 it became
known as Berkeley Yacc or byacc.

Berkeley Yacc had three advantages over the ancestral Yacc: it generated
faster parsers, it could generate reentrant parsers, and the source code was
released to the public domain rather than being under an AT&T proprietary
license. The better performance came from implementing techniques from
DeRemer and Penello’s seminal paper on LALR parsing (see [DeRemer 1982](#DeRemer-1982)).

Use of byacc spread rapidly due to its public domain license. However, once
Bison became available, byacc itself passed out of general use.

---

Next: [Other Ungulates](#Other-Ungulates), Previous: [Berkeley Yacc](#Byacc), Up: [A Brief History of the Greater Ungulates](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 11.4 Bison

Robert Corbett actually wrote two (closely related) LALR parsers in 1985,
both using the DeRemer/Penello techniques. One was “zoo”, the other was
“Byson”. In 1987 Richard Stallman began working on Byson; the name changed
to Bison and the interface became Yacc-compatible.

The main visible difference between Yacc and Byson/Bison at the time of
Byson’s first release is that Byson supported the `@n` construct
(giving access to the starting and ending line number and character number
associated with any of the symbols in the current rule).

There was also the command ‘`%expect n`’ which said not to mention the
conflicts if there are n shift/reduce conflicts and no reduce/reduce
conflicts. In more recent versions of Bison, `%expect` and its
`%expect-rr` variant for reduce/reduce conflicts can be applied to
individual rules.

Later versions of Bison added many more new features.

Bison error reporting has been improved in various ways. Notably. ancestral
Yacc and Byson did not have carets in error messages.

Compared to Yacc Bison uses a faster but less space-efficient encoding for
the parse tables (see [Corbett 1984](#Corbett-1984)), and more modern techniques for
generating the lookahead sets (see [DeRemer 1982](#DeRemer-1982)). This approach is the
standard one since then.

(It has also been plausibly alleged the differences in the algorithms stem
mainly from the horrible kludges that Johnson had to perpetrate to make
the original Yacc fit in a PDP-11.)

Named references, semantic predicates, `%locations`,
`%glr-parser`, `%printer`, %destructor, dumps to DOT,
`%parse-param`, `%lex-param`, and dumps to XSLT, LAC, and IELR(1)
generation are new in Bison.

Bison also has many features to support C++ that were not present in the
ancestral Yacc or Byson.

Bison obsolesced all previous Yacc variants and workalikes generating C by
1995.

---

Previous: [Bison](#Bison), Up: [A Brief History of the Greater Ungulates](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 11.5 Other Ungulates

The Yacc concept has frequently been ported to other languages. Some of the
early ports are extinct along with the languages that hosted them; others
have been superseded by parser skeletons shipped with Bison.

However, independent implementations persist. One of the best-known
still in use is David Beazley’s “PLY” (Python Lex-Yacc) for
Python. Another is goyacc, supporting the Go language. An “ocamlyacc”
is shipped as part of the Ocaml compiler suite.

---

Next: [Frequently Asked Questions](#FAQ), Previous: [A Brief History of the Greater Ungulates](#History), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 12 Bison Version Compatibility: Best Practices

Bison provides a Yacc compatibility mode in which it strives to conform with
the POSIX standard. Grammar files which are written to the POSIX standard, and
do not take advantage of any of the special capabilities of Bison, should
work with many versions of Bison without modification.

All other features of Bison are particular to Bison, and are changing. Bison
is actively maintained and continuously evolving. It should come as no
surprise that an older version of Bison will not accept Bison source code which
uses newer features that do no not exist at all in the older Bison.
Regrettably, in spite of reasonable effort to maintain compatibility, the
reverse situation may also occur: it may happen that code developed using an
older version of Bison does not build with a newer version of Bison without
modifications.

Because Bison is a code generation tool, it is possible to retain its output
and distribute that to the users of the program. The users are then not
required to have Bison installed at all, only an implementation of the
programming language, such as C, which is required for processing the generated
output.

It is the output of Bison that is intended to be of the utmost portability.
So, that is to say, whereas the Bison grammar source code may have a dependency
on specific versions of Bison, the generated parser from any version of Bison
should work with with a large number of implementations of C, or whatever
language is applicable.

The recommended best practice for using Bison (in the context of software that
is distributed in source code form) is to ship the generated parser to the
downstream users. Only those downstream users who engage in active development
of the program who need to make changes to the grammar file need to have Bison
installed at all, and those users can install the specific version of Bison
which is required.

Following this recommended practice also makes it possible to use a more recent
Bison than what is available to users through operating system distributions,
thereby taking advantage of the latest techniques that Bison allows.

Some features of Bison have been, or are being adopted into other Yacc-like
programs. Therefore it might seem that is a good idea to write grammar code
which targets multiple implementations, similarly to the way C programs are
often written to target multiple compilers and language versions. Other than
the Yacc subset described by POSIX, the Bison language is not rigorously
standardized. When a Bison feature is adopted by another parser generator, it
may be initially compatible with that version of Bison on which it was based,
but the compatibility may degrade going forward. Developers who strive to make
their Bison code simultaneously compatible with other parser generators are
encouraged to nevertheless use specific versions of all generators, and still
follow the recommended practice of shipping generated output. For example,
a project can internally maintain compatibility with multiple generators,
and choose the output of a particular one to ship to the users. Or else,
the project could ship all of the outputs, arranging for a way for the user
to specify which one is used to build the program.

---

Next: [Bison Symbols](#Table-of-Symbols), Previous: [Bison Version Compatibility: Best Practices](#Versioning), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 13 Frequently Asked Questions

Several questions about Bison come up occasionally. Here some of them
are addressed.

* [Memory Exhausted](#Memory-Exhausted)
* [How Can I Reset the Parser](#How-Can-I-Reset-the-Parser)
* [Strings are Destroyed](#Strings-are-Destroyed)
* [Implementing Gotos/Loops](#Implementing-Gotos_002fLoops)
* [Multiple start-symbols](#Multiple-start_002dsymbols)
* [Secure? Conform?](#Secure_003f-Conform_003f)
* [Enabling Relocatability](#Enabling-Relocatability)
* [I can’t build Bison](#I-can_0027t-build-Bison)
* [Where can I find help?](#Where-can-I-find-help_003f)
* [Bug Reports](#Bug-Reports)
* [More Languages](#More-Languages)
* [Beta Testing](#Beta-Testing)
* [Mailing Lists](#Mailing-Lists)

---

Next: [How Can I Reset the Parser](#How-Can-I-Reset-the-Parser), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.1 Memory Exhausted

> My parser returns with error with a ‘`memory exhausted`’
> message. What can I do?

This question is already addressed elsewhere, see [Recursive Rules](#Recursion).

---

Next: [Strings are Destroyed](#Strings-are-Destroyed), Previous: [Memory Exhausted](#Memory-Exhausted), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.2 How Can I Reset the Parser

The following phenomenon has several symptoms, resulting in the
following typical questions:

> I invoke `yyparse` several times, and on correct input it works
> properly; but when a parse error is found, all the other calls fail
> too. How can I reset the error flag of `yyparse`?

or

> My parser includes support for an ‘`#include`’-like feature, in which
> case I run `yyparse` from `yyparse`. This fails although I did
> specify ‘`%define api.pure full`’.

These problems typically come not from Bison itself, but from
Lex-generated scanners. Because these scanners use large buffers for
speed, they might not notice a change of input file. As a
demonstration, consider the following source file,
`first-line.l`:

```
%{
#include <stdio.h>
#include <stdlib.h>
%}
```

```
%%
.*\n    ECHO; return 1;
%%
```

```
int
yyparse (char const *file)
{
  yyin = fopen (file, "r");
  if (!yyin)
    {
      perror ("fopen");
      exit (EXIT_FAILURE);
    }
```

```
  /* One token only. */
  yylex ();
  if (fclose (yyin) != 0)
    {
      perror ("fclose");
      exit (EXIT_FAILURE);
    }
  return 0;
}
```

```

```

```
int
main (void)
{
  yyparse ("input");
  yyparse ("input");
  return 0;
}
```

If the file `input` contains

```
input:1: Hello,
input:2: World!
```

then instead of getting the first line twice, you get:

```
$ flex -ofirst-line.c first-line.l
$ gcc  -ofirst-line   first-line.c -ll
$ ./first-line
input:1: Hello,
input:2: World!
```

Therefore, whenever you change `yyin`, you must tell the
Lex-generated scanner to discard its current buffer and switch to the
new one. This depends upon your implementation of Lex; see its
documentation for more. For Flex, it suffices to call
‘`YY_FLUSH_BUFFER`’ after each change to `yyin`. If your
Flex-generated scanner needs to read from several input streams to
handle features like include files, you might consider using Flex
functions like ‘`yy_switch_to_buffer`’ that manipulate multiple
input buffers.

If your Flex-generated scanner uses start conditions (see [Start conditions](https://westes.github.io/flex/manual/Start-conditions.html#Start-conditions) in The Flex Manual), you might
also want to reset the scanner’s state, i.e., go back to the initial
start condition, through a call to ‘`BEGIN (0)`’.

---

Next: [Implementing Gotos/Loops](#Implementing-Gotos_002fLoops), Previous: [How Can I Reset the Parser](#How-Can-I-Reset-the-Parser), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.3 Strings are Destroyed

> My parser seems to destroy old strings, or maybe it loses track of
> them. Instead of reporting ‘`"foo", "bar"`’, it reports
> ‘`"bar", "bar"`’, or even ‘`"foo\nbar", "bar"`’.

This error is probably the single most frequent “bug report” sent to
Bison lists, but is only concerned with a misunderstanding of the role
of the scanner. Consider the following Lex code:

```
%{
#include <stdio.h>
char *yylval = NULL;
%}
```

```
%%
.*    yylval = yytext; return 1;
\n    continue;
%%
```

```
int
main ()
{
  /* Similar to using $1, $2 in a Bison action. */
  char *fst = (yylex (), yylval);
  char *snd = (yylex (), yylval);
  printf ("\"%s\", \"%s\"\n", fst, snd);
  return 0;
}
```

If you compile and run this code, you get:

```
$ flex -osplit-lines.c split-lines.l
$ gcc  -osplit-lines   split-lines.c -ll
$ printf 'one\ntwo\n' | ./split-lines
"one
two", "two"
```

this is because `yytext` is a buffer provided for *reading*
in the action, but if you want to keep it, you have to duplicate it
(e.g., using `strdup`). Note that the output may depend on how
your implementation of Lex handles `yytext`. For instance, when
given the Lex compatibility option `-l` (which triggers the
option ‘`%array`’) Flex generates a different behavior:

```
$ flex -l -osplit-lines.c split-lines.l
$ gcc     -osplit-lines   split-lines.c -ll
$ printf 'one\ntwo\n' | ./split-lines
"two", "two"
```

---

Next: [Multiple start-symbols](#Multiple-start_002dsymbols), Previous: [Strings are Destroyed](#Strings-are-Destroyed), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.4 Implementing Gotos/Loops

> My simple calculator supports variables, assignments, and functions,
> but how can I implement gotos, or loops?

Although very pedagogical, the examples included in the document blur
the distinction to make between the parser—whose job is to recover
the structure of a text and to transmit it to subsequent modules of
the program—and the processing (such as the execution) of this
structure. This works well with so called straight line programs,
i.e., precisely those that have a straightforward execution model:
execute simple instructions one after the others.

If you want a richer model, you will probably need to use the parser
to construct a tree that does represent the structure it has
recovered; this tree is usually called the *abstract syntax tree*,
or *AST* for short. Then, walking through this tree,
traversing it in various ways, will enable treatments such as its
execution or its translation, which will result in an interpreter or a
compiler.

This topic is way beyond the scope of this manual, and the reader is
invited to consult the dedicated literature.

---

Next: [Secure? Conform?](#Secure_003f-Conform_003f), Previous: [Implementing Gotos/Loops](#Implementing-Gotos_002fLoops), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.5 Multiple start-symbols

> I have several closely related grammars, and I would like to share their
> implementations. In fact, I could use a single grammar but with multiple
> entry points.

Bison does not support multiple start-symbols, but there is a very simple
means to simulate them. If `foo` and `bar` are the two pseudo
start-symbols, then introduce two new tokens, say `START_FOO` and
`START_BAR`, and use them as switches from the real start-symbol:

```
%token START_FOO START_BAR;
%start start;
start:
  START_FOO foo
| START_BAR bar;
```

These tokens prevent the introduction of new conflicts. As far as the
parser goes, that is all that is needed.

Now the difficult part is ensuring that the scanner will send these tokens
first. If your scanner is hand-written, that should be straightforward. If
your scanner is generated by Lex, them there is simple means to do it:
recall that anything between ‘`%{ ... %}`’ after the first `%%` is
copied verbatim in the top of the generated `yylex` function. Make
sure a variable `start_token` is available in the scanner (e.g., a
global variable or using `%lex-param` etc.), and use the following:

```
  /* Prologue. */
%%
%{
  if (start_token)
    {
      int t = start_token;
      start_token = 0;
      return t;
    }
%}
  /* The rules. */
```

---

Next: [Enabling Relocatability](#Enabling-Relocatability), Previous: [Multiple start-symbols](#Multiple-start_002dsymbols), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.6 Secure? Conform?

> Is Bison secure? Does it conform to POSIX?

If you’re looking for a guarantee or certification, we don’t provide it.
However, Bison is intended to be a reliable program that conforms to the
POSIX specification for Yacc. If you run into problems, please send us a
bug report.

---

Next: [I can’t build Bison](#I-can_0027t-build-Bison), Previous: [Secure? Conform?](#Secure_003f-Conform_003f), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.7 Enabling Relocatability

It has been a pain for many users of GNU packages for a long time that
packages are not relocatable. It means a user cannot copy a program,
installed by another user on the same machine, to his home directory,
and have it work correctly (including i18n). So many users need to go
through `configure; make; make install` with all its
dependencies, options, and hurdles.

Most package management systems, that allow the user to install
pre-built binaries of the packages, solve the “ease of
installation” problem, but they hardwire path names, usually to
`/usr` or `/usr/local`. This means that users need root
privileges to install a binary package, and prevents installing two
different versions of the same binary package.

A relocatable program can be moved or copied to a different location
on the file system. It is possible to make symlinks to the installed
and moved programs, and invoke them through the symlink. It is
possible to do the same thing with a hard link *only* if the hard
link file is in the same directory as the real program.

To configure a program to be relocatable, add
`--enable-relocatable` to the `configure` command line.

On some OSes the executables remember the location of shared libraries
and prefer them over any other search path. Therefore, such an
executable will look for its shared libraries first in the original
installation directory and only then in the current installation
directory. Thus, for reliability, it is best to also give a
`--prefix` option pointing to a directory that does not exist
now and which never will be created, e.g.
`--prefix=/nonexistent`. You may use
`DESTDIR=dest-dir` on the `make` command line to
avoid installing into that directory.

We do not recommend using a prefix writable by unprivileged users
(e.g. `/tmp/inst$$`) because such a directory can be recreated
by an unprivileged user after the original directory has been removed.
We also do not recommend prefixes that might be behind an automounter
(e.g. `$HOME/inst$$`) because of the performance impact of
directory searching.

Here’s a sample installation run that takes into account all these
recommendations:

```
./configure --enable-relocatable --prefix=/nonexistent
make
make install DESTDIR=/tmp/inst$$
```

Installation with `--enable-relocatable` will not work for
setuid or setgid executables, because such executables search only
system library paths for security reasons.

The runtime penalty and size penalty are negligible on GNU/Linux (just
one system call more when an executable is launched), and small on
other systems (the wrapper program just sets an environment variable
and executes the real program).

---

Next: [Where can I find help?](#Where-can-I-find-help_003f), Previous: [Enabling Relocatability](#Enabling-Relocatability), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.8 I can’t build Bison

> I can’t build Bison because `make` complains that
> `msgfmt` is not found.
> What should I do?

Like most GNU packages with internationalization support, that feature
is turned on by default. If you have problems building in the `po`
subdirectory, it indicates that your system’s internationalization
support is lacking. You can re-configure Bison with
`--disable-nls` to turn off this support, or you can install GNU
gettext from <https://ftp.gnu.org/gnu/gettext/> and re-configure
Bison. See the file `ABOUT-NLS` for more information.

> I can’t build Bison because my C compiler is too old.

Except for GLR parsers (which require C99), the C code that Bison generates
requires only C89 or later. However, Bison itself requires common C99
features such as declarations after statements. Bison’s `configure`
script attempts to enable C99 (or later) support on compilers that default
to pre-C99. If your compiler lacks these C99 features entirely, GCC may
well be a better choice; or you can try upgrading to your compiler’s latest
version.

---

Next: [Bug Reports](#Bug-Reports), Previous: [I can’t build Bison](#I-can_0027t-build-Bison), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.9 Where can I find help?

> I’m having trouble using Bison. Where can I find help?

First, read this fine manual. Beyond that, you can send mail to
[help-bison@gnu.org](mailto:help-bison@gnu.org). This mailing list is intended to be
populated with people who are willing to answer questions about using
and installing Bison. Please keep in mind that (most of) the people on
the list have aspects of their lives which are not related to Bison (!),
so you may not receive an answer to your question right away. This can
be frustrating, but please try not to honk them off; remember that any
help they provide is purely voluntary and out of the kindness of their
hearts.

---

Next: [More Languages](#More-Languages), Previous: [Where can I find help?](#Where-can-I-find-help_003f), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.10 Bug Reports

> I found a bug. What should I include in the bug report?

Before sending a bug report, make sure you are using the latest
version. Check <https://ftp.gnu.org/pub/gnu/bison/> or one of its
mirrors. Be sure to include the version number in your bug report. If
the bug is present in the latest version but not in a previous version,
try to determine the most recent version which did not contain the bug.

If the bug is parser-related, you should include the smallest grammar
you can which demonstrates the bug. The grammar file should also be
complete (i.e., I should be able to run it through Bison without having
to edit or add anything). The smaller and simpler the grammar, the
easier it will be to fix the bug.

Include information about your compilation environment, including your
operating system’s name and version and your compiler’s name and
version. If you have trouble compiling, you should also include a
transcript of the build session, starting with the invocation of
`configure`. Depending on the nature of the bug, you may be asked to
send additional files as well (such as `config.h` or `config.cache`).

Patches are most welcome, but not required. That is, do not hesitate to
send a bug report just because you cannot provide a fix.

Send bug reports to [bug-bison@gnu.org](mailto:bug-bison@gnu.org).

---

Next: [Beta Testing](#Beta-Testing), Previous: [Bug Reports](#Bug-Reports), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.11 More Languages

> Will Bison ever have C++ and Java support? How about insert your
> favorite language here?

C++, D and Java are supported. We’d love to add other languages;
contributions are welcome.

---

Next: [Mailing Lists](#Mailing-Lists), Previous: [More Languages](#More-Languages), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 13.12 Beta Testing

> What is involved in being a beta tester?

It’s not terribly involved. Basically, you would download a test
release, compile it, and use it to build and run a parser or two. After
that, you would submit either a bug report or a message saying that
everything is okay. It is important to report successes as well as
failures because test releases eventually become mainstream releases,
but only if they are adequately tested. If no one tests, development is
essentially halted.

Beta testers are particularly needed for operating systems to which the
developers do not have easy access. They currently have easy access to
recent GNU/Linux and Solaris versions. Reports about other operating
systems are especially welcome.

---

Previous: [Beta Testing](#Beta-Testing), Up: [Frequently Asked Questions](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]
