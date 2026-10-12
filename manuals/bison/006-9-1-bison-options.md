### 9.1 Bison Options

Bison supports both traditional single-letter options and mnemonic long
option names. Long option names are indicated with `--` instead of
`-`. Abbreviations for option names are allowed as long as they
are unique. When a long option takes an argument, like
`--file-prefix`, connect the option name and the argument with
‘`=`’.

Here is a list of options that can be used with Bison. It is followed by a
cross key alphabetized by long option.

* [Operation Modes](#Operation-Modes)
* [Diagnostics](#Diagnostics)
* [Tuning the Parser](#Tuning-the-Parser)
* [Output Files](#Output-Files)

---

Next: [Diagnostics](#Diagnostics), Up: [Bison Options](#Bison-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 9.1.1 Operation Modes

Options controlling the global behavior of `bison`.

`-h`

`--help`
:   Print a summary of the command-line options to Bison and exit.

`-V`

`--version`
:   Print the version number of Bison and exit.

`--print-localedir`
:   Print the name of the directory containing locale-dependent data.

`--print-datadir`
:   Print the name of the directory containing skeletons, CSS and XSLT.

`-u`

`--update`
:   Update the grammar file (remove duplicates, update deprecated directives,
    etc.) and exit (i.e., do not generate any of the output files). Leaves a
    backup of the original file with a `~` appended. For instance:

    ```
    $ cat foo.y
    %error-verbose
    %define parse.error verbose
    %%
    exp:;
    ```

    ```
    $ bison -u foo.y
    foo.y:1.1-14: warning: deprecated directive, use '%define parse.error verbose' [-Wdeprecated]
        1 | %error-verbose
          | ^~~~~~~~~~~~~~
    foo.y:2.1-27: warning: %define variable 'parse.error' redefined [-Wother]
        2 | %define parse.error verbose
          | ^~~~~~~~~~~~~~~~~~~~~~~~~~~
    foo.y:1.1-14:     previous definition
        1 | %error-verbose
          | ^~~~~~~~~~~~~~
    bison: file 'foo.y' was updated (backup: 'foo.y~')
    ```

    ```
    $ cat foo.y
    %define parse.error verbose
    %%
    exp:;
    ```

    See the documentation of `--feature=fixit` below for more details.

`-f [feature]`

`--feature[=feature]`
:   Activate miscellaneous features. Feature can be one of:

    `caret`

    `diagnostics-show-caret`
    :   Show caret errors, in a manner similar to GCC’s
        `-fdiagnostics-show-caret`, or Clang’s
        `-fcaret-diagnostics`. The location provided with the message is used
        to quote the corresponding line of the source file, underlining the
        important part of it with carets (‘`^`’). Here is an example, using the
        following file `in.y`:

        ```
        %nterm <ival> exp
        %%
        exp: exp '+' exp { $exp = $1 + $2; };
        ```

        When invoked with `-fcaret` (or nothing), Bison will report:

        ```
        in.y:3.20-23: error: ambiguous reference: '$exp'
            3 | exp: exp '+' exp { $exp = $1 + $2; };
              |                    ^~~~
        ```

        ```
        in.y:3.1-3:       refers to: $exp at $$
            3 | exp: exp '+' exp { $exp = $1 + $2; };
              | ^~~
        ```

        ```
        in.y:3.6-8:       refers to: $exp at $1
            3 | exp: exp '+' exp { $exp = $1 + $2; };
              |      ^~~
        ```

        ```
        in.y:3.14-16:     refers to: $exp at $3
            3 | exp: exp '+' exp { $exp = $1 + $2; };
              |              ^~~
        ```

        ```
        in.y:3.32-33: error: $2 of 'exp' has no declared type
            3 | exp: exp '+' exp { $exp = $1 + $2; };
              |                                ^~
        ```

        Whereas, when invoked with `-fno-caret`, Bison will only report:

        ```
        in.y:3.20-23: error: ambiguous reference: '$exp'
        in.y:3.1-3:       refers to: $exp at $$
        in.y:3.6-8:       refers to: $exp at $1
        in.y:3.14-16:     refers to: $exp at $3
        in.y:3.32-33: error: $2 of 'exp' has no declared type
        ```

        This option is activated by default.

    `fixit`

    `diagnostics-parseable-fixits`
    :   Show machine-readable fixes, in a manner similar to GCC’s and Clang’s
        `-fdiagnostics-parseable-fixits`.

        Fix-its are generated for duplicate directives:

        ```
        $ cat foo.y
        %define api.prefix {foo}
        %define api.prefix {bar}
        %%
        exp:;
        ```

        ```

        ```

        ```
        $ bison -ffixit foo.y
        foo.y:2.1-24: error: %define variable 'api.prefix' redefined
            2 | %define api.prefix {bar}
              | ^~~~~~~~~~~~~~~~~~~~~~~~
        foo.y:1.1-24:     previous definition
            1 | %define api.prefix {foo}
              | ^~~~~~~~~~~~~~~~~~~~~~~~
        fix-it:"foo.y":{2:1-2:25}:""
        foo.y: warning: fix-its can be applied.  Rerun with option '--update'. [-Wother]
        ```

        They are also generated to update deprecated directives, unless
        `-Wno-deprecated` was given:

        ```
        $ cat /tmp/foo.yy
        %error-verbose
        %name-prefix "foo"
        %%
        exp:;
        ```

        ```
        $ bison foo.y
        foo.y:1.1-14: warning: deprecated directive, use '%define parse.error verbose' [-Wdeprecated]
            1 | %error-verbose
              | ^~~~~~~~~~~~~~
        foo.y:2.1-18: warning: deprecated directive, use '%define api.prefix {foo}' [-Wdeprecated]
            2 | %name-prefix "foo"
              | ^~~~~~~~~~~~~~~~~~
        foo.y: warning: fix-its can be applied.  Rerun with option '--update'. [-Wother]
        ```

        The fix-its are applied by `bison` itself when given the option
        `-u`/`--update`. See its documentation above.

    `syntax-only`
    :   Do not generate the output files. The name of this feature is somewhat
        misleading as more than just checking the syntax is done: every stage is run
        (including checking for conflicts for instance), except the generation of
        the output files.

---

Next: [Tuning the Parser](#Tuning-the-Parser), Previous: [Operation Modes](#Operation-Modes), Up: [Bison Options](#Bison-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 9.1.2 Diagnostics

Options controlling the diagnostics.

`-W [category]`

`--warnings[=category]`
:   Output warnings falling in category. category can be one
    of:

    `conflicts-sr`

    `conflicts-rr`
    :   S/R and R/R conflicts. These warnings are enabled by default. However, if
        the `%expect` or `%expect-rr` directive is specified, an
        unexpected number of conflicts is an error, and an expected number of
        conflicts is not reported, so `-W` and `--warning` then have
        no effect on the conflict report.

    `counterexamples`

    `cex`
    :   Provide counterexamples for conflicts. See [Generation of Counterexamples](#Counterexamples).
        Counterexamples take time to compute. The option `-Wcex` should be
        used by the developer when working on the grammar; it hardly makes sense to
        use it in a CI.

    `dangling-alias`
    :   Report string literals that are not bound to a token symbol.

        String literals, which allow for better error messages, are (too) liberally
        accepted by Bison, which might result in silent errors. For instance

        ```
        %type <exVal> cond "condition"
        ```

        does not define “condition” as a string alias to `cond`—nonterminal
        symbols do not have string aliases. It is rather equivalent to

        ```
        %nterm <exVal> cond
        %token <exVal> "condition"
        ```

        i.e., it gives the ‘`"condition"`’ token the type `exVal`.

        Also, because string aliases do not need to be defined, typos such as
        ‘`"baz"`’ instead of ‘`"bar"`’ will be not reported.

        The option `-Wdangling-alias` catches these situations. On

        ```
        %token BAR "bar"
        %type <ival> foo "foo"
        %%
        foo: "baz" {}
        ```

        ‘`bison -Wdangling-alias`’ reports

        ```
        warning: string literal not attached to a symbol
              | %type <ival> foo "foo"
              |                  ^~~~~
        warning: string literal not attached to a symbol
              | foo: "baz" {}
              |      ^~~~~
        ```

    `deprecated`
    :   Deprecated constructs whose support will be removed in future versions of
        Bison.

    `empty-rule`
    :   Empty rules without `%empty`. See [Empty Rules](#Empty-Rules). Disabled by
        default, but enabled by uses of `%empty`, unless
        `-Wno-empty-rule` was specified.

    `midrule-values`
    :   Warn about midrule values that are set but not used within any of the actions
        of the parent rule.
        For example, warn about unused `$2` in:

        ```
        exp: '1' { $$ = 1; } '+' exp { $$ = $1 + $4; };
        ```

        Also warn about midrule values that are used but not set.
        For example, warn about unset `$$` in the midrule action in:

        ```
        exp: '1' { $1 = 1; } '+' exp { $$ = $2 + $4; };
        ```

        These warnings are not enabled by default since they sometimes prove to
        be false alarms in existing grammars employing the Yacc constructs
        `$0` or `$-n` (where n is some positive integer).

    `precedence`
    :   Useless precedence and associativity directives. Disabled by default.

        Consider for instance the following grammar:

        ```
        %nonassoc "="
        %left "+"
        %left "*"
        %precedence "("
        ```

        ```
        %%
        ```

        ```
        stmt:
          exp
        | "var" "=" exp
        ;
        ```

        ```

        ```

        ```
        exp:
          exp "+" exp
        | exp "*" "number"
        | "(" exp ")"
        | "number"
        ;
        ```

        Bison reports:

        ```
        warning: useless precedence and associativity for "="
              | %nonassoc "="
              |           ^~~
        ```

        ```
        warning: useless associativity for "*", use %precedence
              | %left "*"
              |       ^~~
        ```

        ```
        warning: useless precedence for "("
              | %precedence "("
              |             ^~~
        ```

        One would get the exact same parser with the following directives instead:

        ```
        %left "+"
        %precedence "*"
        ```

    `yacc`
    :   Incompatibilities with POSIX Yacc.

    `other`
    :   All warnings not categorized above. These warnings are enabled by default.

        This category is provided merely for the sake of completeness. Future
        releases of Bison may move warnings from this category to new, more specific
        categories.

    `all`
    :   All the warnings except `counterexamples`, `dangling-alias` and
        `yacc`.

    `none`
    :   Turn off all the warnings.

    `error`
    :   See `-Werror`, below.

    A category can be turned off by prefixing its name with ‘`no-`’. For
    instance, `-Wno-yacc` will hide the warnings about
    POSIX Yacc incompatibilities.

`-Werror`
:   Turn enabled warnings for every category into errors, unless they are
    explicitly disabled by `-Wno-error=category`.

`-Werror=category`
:   Enable warnings falling in category, and treat them as errors.

    category is the same as for `--warnings`, with the exception that
    it may not be prefixed with ‘`no-`’ (see above).

    Note that the precedence of the ‘`=`’ and ‘`,`’ operators is such that
    the following commands are *not* equivalent, as the first will not treat
    S/R conflicts as errors.

    ```
    $ bison -Werror=yacc,conflicts-sr input.y
    $ bison -Werror=yacc,error=conflicts-sr input.y
    ```

`-Wno-error`
:   Do not turn enabled warnings for every category into errors, unless
    they are explicitly enabled by `-Werror=category`.

`-Wno-error=category`
:   Deactivate the error treatment for this category. However, the warning
    itself won’t be disabled, or enabled, by this option.

`--color`
:   Equivalent to `--color=always`.

`--color=when`
:   Control whether diagnostics are colorized, depending on when:

    `always`

    `yes`
    :   Enable colorized diagnostics.

    `never`

    `no`
    :   Disable colorized diagnostics.

    `auto (default)`

    `tty`
    :   Diagnostics will be colorized if the output device is a tty, i.e. when the
        output goes directly to a text screen or terminal emulator window.

`--style=file`
:   Specifies the CSS style file to use when colorizing. It has an effect
    only when the `--color` option is effective. The
    `bison-default.css` file provide a good example from which to define
    your own style file. See the documentation of libtextstyle for more
    details.

---

Next: [Output Files](#Output-Files), Previous: [Diagnostics](#Diagnostics), Up: [Bison Options](#Bison-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 9.1.3 Tuning the Parser

Options changing the generated parsers.

`-t`

`--debug`
:   In the parser implementation file, define the macro `YYDEBUG` to 1 if
    it is not already defined, so that the debugging facilities are compiled.
    See [Tracing Your Parser](#Tracing).

`-D name[=value]`

`--define=name[=value]`

`-F name[=value]`

`--force-define=name[=value]`
:   Each of these is equivalent to ‘`%define name value`’
    (see [%define Summary](#g_t_0025define-Summary)). Note that the delimiters are part of
    value: `-Dapi.value.type=union`,
    `-Dapi.value.type={union}` and `-Dapi.value.type="union"`
    correspond to ‘`%define api.value.type union`’, ‘`%define
    api.value.type {union}`’ and ‘`%define api.value.type "union"`’.

    Bison processes multiple definitions for the same name as follows:

    * Bison quietly ignores all command-line definitions for name except
      the last.
    * If that command-line definition is specified by a `-D` or
      `--define`, Bison reports an error for any `%define` definition
      for name.
    * If that command-line definition is specified by a `-F` or
      `--force-define` instead, Bison quietly ignores all `%define`
      definitions for name.
    * Otherwise, Bison reports an error if there are multiple `%define`
      definitions for name.

    You should avoid using `-F` and `--force-define` in your
    make files unless you are confident that it is safe to quietly ignore
    any conflicting `%define` that may be added to the grammar file.

`-L language`

`--language=language`
:   Specify the programming language for the generated parser, as if
    `%language` was specified (see [Bison Declaration Summary](#Decl-Summary)). Currently supported
    languages include C, C++, D and Java. language is case-insensitive.

`--locations`
:   Pretend that `%locations` was specified. See [Bison Declaration Summary](#Decl-Summary).

`-p prefix`

`--name-prefix=prefix`
:   Pretend that `%name-prefix "prefix"` was specified (see [Bison Declaration Summary](#Decl-Summary)). The option `-p` is specified by POSIX. When POSIX
    compatibility is not a requirement, `-Dapi.prefix=prefix` is a
    better option (see [Multiple Parsers in the Same Program](#Multiple-Parsers)).

`-l`

`--no-lines`
:   Don’t put any `#line` preprocessor commands in the parser
    implementation file. Ordinarily Bison puts them in the parser
    implementation file so that the C compiler and debuggers will
    associate errors with your source file, the grammar file. This option
    causes them to associate errors with the parser implementation file,
    treating it as an independent source file in its own right.

`-S file`

`--skeleton=file`
:   Specify the skeleton to use, similar to `%skeleton`
    (see [Bison Declaration Summary](#Decl-Summary)).

    If file does not contain a `/`, file is the name of a skeleton
    file in the Bison installation directory.
    If it does, file is an absolute file name or a file name relative to the
    current working directory.
    This is similar to how most shells resolve commands.

`-k`

`--token-table`
:   Pretend that `%token-table` was specified. See [Bison Declaration Summary](#Decl-Summary).

`-y`

`--yacc`
:   Act more like the traditional `yacc` command:

    * Generate different diagnostics (it implies `-Wyacc`).
    * Generate `#define` statements in addition to an `enum` to
      associate token codes with token kind names.
    * If the `POSIXLY_CORRECT` environment variable is defined, generate
      prototypes for `yyerror` and `yylex`[6](#FOOT6) (since Bison
      3.8):

      ```
      int yylex (void);
      void yyerror (const char *);
      ```

      As a Bison extension, additional arguments required by `%pure-parser`,
      `%locations`, `%lex-param` and `%parse-param` are taken into
      account. You may disable `yyerror`’s prototype with ‘`#define
      yyerror yyerror`’ (as specified by POSIX), or with ‘`#define
      YYERROR_IS_DECLARED`’ (a Bison extension). Likewise for `yylex`.
    * Imitate Yacc’s output file name conventions, so that the parser
      implementation file is called `y.tab.c`, and the other outputs are
      called `y.output` and `y.tab.h`. Do not use `--yacc` just
      to change the output file names since it also triggers all the
      aforementioned behavior changes; rather use ‘`-o y.tab.c`’.

    The `-y`/`--yacc` option is intended for use with traditional
    Yacc grammars. This option only makes sense for the default C skeleton,
    `yacc.c`. If your grammar uses Bison extensions Bison cannot be
    Yacc-compatible, even if this option is specified.

    Thus, the following shell script can substitute for Yacc, and the Bison
    distribution contains such a `yacc` script for compatibility with
    POSIX:

    ```
    #! /bin/sh
    bison -y "$@"
    ```

---

Previous: [Tuning the Parser](#Tuning-the-Parser), Up: [Bison Options](#Bison-Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 9.1.4 Output Files

Options controlling the output.

`-H [file]`

`--header=[file]`
:   Pretend that `%header` was specified, i.e., write an extra output file
    containing definitions for the token kind names defined in the grammar, as
    well as a few other declarations. See [Bison Declaration Summary](#Decl-Summary).

`--defines[=file]`
:   Historical name for option `--header` before Bison 3.8.

`-d`
:   This is the same as `--header` except `-d` does not accept a
    file argument since POSIX Yacc requires that `-d` can be
    bundled with other short options.

`-b file-prefix`

`--file-prefix=prefix`
:   Pretend that `%file-prefix` was specified, i.e., specify prefix to use
    for all Bison output file names. See [Bison Declaration Summary](#Decl-Summary).

`-r things`

`--report=things`
:   Write an extra output file containing verbose description of the comma
    separated list of things among:

    `state`
    :   Description of the grammar, conflicts (resolved and unresolved), and
        parser’s automaton.

    `itemset`
    :   Implies `state` and augments the description of the automaton with
        the full set of items for each state, instead of its core only.

    `lookahead`
    :   Implies `state` and augments the description of the automaton with
        each rule’s lookahead set.

    `solved`
    :   Implies `state`. Explain how conflicts were solved thanks to
        precedence and associativity directives.

    `counterexamples`

    `cex`
    :   Look for counterexamples for the conflicts. See [Generation of Counterexamples](#Counterexamples).
        Counterexamples take time to compute. The option `-rcex` should be
        used by the developer when working on the grammar; it hardly makes sense to
        use it in a CI.

    `all`
    :   Enable all the items.

    `none`
    :   Do not generate the report.

`--report-file=file`
:   Specify the file for the verbose description.

`-v`

`--verbose`
:   Pretend that `%verbose` was specified, i.e., write an extra output
    file containing verbose descriptions of the grammar and
    parser. See [Bison Declaration Summary](#Decl-Summary).

`-o file`

`--output=file`
:   Specify the file for the parser implementation file.

    The names of the other output files are constructed from file as
    described under the `-v` and `-d` options.

`-g [file]`

`--graph[=file]`
:   Output a graphical representation of the parser’s automaton computed by
    Bison, in [Graphviz](https://www.graphviz.org/)
    [DOT](https://www.graphviz.org/doc/info/lang.html) format.
    `file` is optional. If omitted and the grammar file is
    `foo.y`, the output file will be `foo.gv`.

`-x [file]`

`--xml[=file]`
:   Output an XML report of the parser’s automaton computed by Bison.
    `file` is optional.
    If omitted and the grammar file is `foo.y`, the output file will be
    `foo.xml`.

`-M old=new`

`--file-prefix-map=old=new`
:   Replace prefix old with new when writing file paths in output
    files.

---

Next: [Yacc Library](#Yacc-Library), Previous: [Bison Options](#Bison-Options), Up: [Invoking Bison](#Invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 9.2 Option Cross Key

Here is a list of options, alphabetized by long option, to help you find
the corresponding short option and directive.

| Long Option | Short Option | Bison Directive |
| --- | --- | --- |
| `--color[=when]` |  |  |
| `--debug` | `-t` | `%debug` |
| `--define=name[=value]` | `-D name[=value]` | `%define name [value]` |
| `--feature[=features]` | `-f [features]` |  |
| `--file-prefix-map=old=new` | `-M old=new` |  |
| `--file-prefix=prefix` | `-b prefix` | `%file-prefix "prefix"` |
| `--force-define=name[=value]` | `-F name[=value]` | `%define name [value]` |
| `--graph[=file]` | `-g [file]` |  |
| `--header=[file]` | `-H [file]` | `%header ["file"]` |
| `--help` | `-h` |  |
| `--html[=file]` |  |  |
| `--language=language` | `-L language` | `%language "language"` |
| `--locations` |  | `%locations` |
| `--name-prefix=prefix` | `-p prefix` | `%name-prefix "prefix"` |
| `--no-lines` | `-l` | `%no-lines` |
| `--output=file` | `-o file` | `%output "file"` |
| `--print-datadir` |  |  |
| `--print-localedir` |  |  |
| `--report-file=file` |  |  |
| `--report=things` | `-r things` |  |
| `--skeleton=file` | `-S file` | `%skeleton "file"` |
| `--style=file` |  |  |
| `--token-table` | `-k` | `%token-table` |
| `--update` | `-u` |  |
| `--verbose` | `-v` | `%verbose` |
| `--version` | `-V` |  |
| `--warnings[=category]` | `-W [category]` |  |
| `--xml[=file]` | `-x [file]` |  |
| `--yacc` | `-y` | `%yacc` |

---

Previous: [Option Cross Key](#Option-Cross-Key), Up: [Invoking Bison](#Invocation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 9.3 Yacc Library

The Yacc library contains default implementations of the `yyerror` and
`main` functions. These default implementations are normally not
useful, but POSIX requires them. To use the Yacc library, link your program
with the `-ly` option. Note that Bison’s implementation of the Yacc
library is distributed under the terms of the GNU General Public License
(see [GNU GENERAL PUBLIC LICENSE](#Copying)).

If you use the Yacc library’s `yyerror` function, you should declare
`yyerror` as follows:

```
int yyerror (char const *);
```

The `int` value returned by this `yyerror` is ignored.

The implementation of Yacc library’s `main` function is:

```
int main (void)
{
  setlocale (LC_ALL, "");
  return yyparse ();
}
```

so if you use it, the internationalization support is enabled (e.g., error
messages are translated), and your `yyparse` function should have the
following type signature:

```
int yyparse (void);
```

---

Next: [A Brief History of the Greater Ungulates](#History), Previous: [Invoking Bison](#Invocation), Up: [Bison](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]## 10 Parsers Written In Other Languages

In addition to C, Bison can generate parsers in C++, D and Java. This chapter
is devoted to these languages. The reader is expected to understand how
Bison works; read the introductory chapters first if you don’t.

* [C++ Parsers](#C_002b_002b-Parsers)
* [D Parsers](#D-Parsers)
* [Java Parsers](#Java-Parsers)

---

Next: [D Parsers](#D-Parsers), Up: [Parsers Written In Other Languages](#Other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

### 10.1 C++ Parsers

The Bison parser in C++ is an object, an instance of the class
`yy::parser`.

* [A Simple C++ Example](#A-Simple-C_002b_002b-Example)
* [C++ Bison Interface](#C_002b_002b-Bison-Interface)
* [C++ Parser Interface](#C_002b_002b-Parser-Interface)
* [C++ Semantic Values](#C_002b_002b-Semantic-Values)
* [C++ Location Values](#C_002b_002b-Location-Values)
* [C++ Parser Context](#C_002b_002b-Parser-Context)
* [C++ Scanner Interface](#C_002b_002b-Scanner-Interface)
* [A Complete C++ Example](#A-Complete-C_002b_002b-Example)

---

Next: [C++ Bison Interface](#C_002b_002b-Bison-Interface), Up: [C++ Parsers](#C_002b_002b-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.1 A Simple C++ Example

This tutorial about C++ parsers is based on a simple, self contained
example.[7](#FOOT7) The following sections are the reference
manual for Bison with C++, the last one showing a fully blown example
(see [A Complete C++ Example](#A-Complete-C_002b_002b-Example)).

To look nicer, our example will be in C++14. It is not required: Bison
supports the original C++98 standard.

A Bison file has three parts. In the first part, the prologue, we start by
making sure we run a version of Bison which is recent enough, and that we
generate C++.

```
%require "3.2"
%language "c++"
```

Let’s dive directly into the middle part: the grammar. Our input is a
simple list of strings, that we display once the parsing is done.

```
%%
```

```
result:
  list  { std::cout << $1 << '\n'; }
;
```

```
%nterm <std::vector<std::string>> list;
```

```
list:
  %empty     { /* Generates an empty string list */ }
| list item  { $$ = $1; $$.push_back ($2); }
;
```

We used a vector of strings as a semantic value! To use genuine C++ objects
as semantic values—not just PODs—we cannot rely on the union that Bison
uses by default to store them, we need *variants* (see [C++ Variants](#C_002b_002b-Variants)):

```
%define api.value.type variant
```

Obviously, the rule for `result` needs to print a vector of strings.
In the prologue, we add:

```
%code
{
  // Print a list of strings.
  auto
  operator<< (std::ostream& o, const std::vector<std::string>& ss)
    -> std::ostream&
  {
    o << '{';
    const char *sep = "";
```

```
    for (const auto& s: ss)
      {
        o << sep << s;
        sep = ", ";
      }
```

```
    return o << '}';
  }
}
```

You may want to move it into the `yy` namespace to avoid leaking it in
your default namespace. We recommend that you keep the actions simple, and
move details into auxiliary functions, as we did with `operator<<`.

Our list of strings will be built from two types of items: numbers and
strings:

```
%nterm <std::string> item;
%token <std::string> TEXT;
%token <int> NUMBER;
```

```
item:
  TEXT
| NUMBER  { $$ = std::to_string ($1); }
;
```

In the case of `TEXT`, the implicit default action applies: `$$ = $1`.

  

Our scanner deserves some attention. The traditional interface of
`yylex` is not type safe: since the token kind and the token value are
not correlated, you may return a `NUMBER` with a string as semantic
value. To avoid this, we use *token constructors* (see [Complete Symbols](#Complete-Symbols)). This directive:

```
%define api.token.constructor
```

requests that Bison generates the functions `make_TEXT` and
`make_NUMBER`, but also `make_YYEOF`, for the end of input.

Everything is in place for our scanner:

```
%code
{
  namespace yy
  {
    // Return the next token.
    auto yylex () -> parser::symbol_type
    {
      static int count = 0;
      switch (int stage = count++)
        {
```

```
        case 0:
          return parser::make_TEXT ("I have three numbers for you.");
```

```
        case 1: case 2: case 3:
          return parser::make_NUMBER (stage);
```

```
        case 4:
          return parser::make_TEXT ("And that's all!");
```

```
        default:
          return parser::make_YYEOF ();
```

```
        }
    }
  }
}
```

In the epilogue, the third part of a Bison grammar file, we leave simple
details: the error reporting function, and the main function.

```
%%
namespace yy
{
  // Report an error to the user.
  auto parser::error (const std::string& msg) -> void
  {
    std::cerr << msg << '\n';
  }
}

int main ()
{
  yy::parser parse;
  return parse ();
}
```

Compile, and run!

```
$ bison simple.yy -o simple.cc
$ g++ -std=c++14 simple.cc -o simple
```

```
$ ./simple
{I have three numbers for you., 1, 2, 3, And that's all!}
```

---

Next: [C++ Parser Interface](#C_002b_002b-Parser-Interface), Previous: [A Simple C++ Example](#A-Simple-C_002b_002b-Example), Up: [C++ Parsers](#C_002b_002b-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.2 C++ Bison Interface

The C++ deterministic parser is selected using the skeleton directive,
‘`%skeleton "lalr1.cc"`’. See [Bison Declaration Summary](#Decl-Summary).

When run, `bison` will create several entities in the ‘`yy`’
namespace.
Use the ‘`%define api.namespace`’ directive to change the namespace name,
see [%define Summary](#g_t_0025define-Summary). The various classes are generated
in the following files:

`file.hh`
:   (Assuming the extension of the grammar file was ‘`.yy`’.) The
    declaration of the C++ parser class and auxiliary types. By default, this
    file is not generated (see [Bison Declaration Summary](#Decl-Summary)).

`file.cc`
:   The implementation of the C++ parser class. The basename and extension of
    these two files (`file.hh` and `file.cc`) follow the
    same rules as with regular C parsers (see [Invoking Bison](#Invocation)).

`location.hh`
:   Generated when both `%header` and `%locations` are enabled, this
    file contains the definition of the classes `position` and
    `location`, used for location tracking. It is not generated if
    ‘`%define api.location.file none`’ is specified, or if user defined
    locations are used. See [C++ Location Values](#C_002b_002b-Location-Values).

`position.hh`

`stack.hh`
:   Useless legacy files. To get rid of then, use ‘`%require "3.2"`’ or
    newer.

All these files are documented using Doxygen; run `doxygen` for a
complete and accurate documentation.

---

Next: [C++ Semantic Values](#C_002b_002b-Semantic-Values), Previous: [C++ Bison Interface](#C_002b_002b-Bison-Interface), Up: [C++ Parsers](#C_002b_002b-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.3 C++ Parser Interface

The output files `file.hh` and `file.cc` declare and
define the parser class in the namespace `yy`. The class name defaults
to `parser`, but may be changed using ‘`%define api.parser.class
{name}`’. The interface of this class is detailed below. It can be
extended using the `%parse-param` feature: its semantics is slightly
changed since it describes an additional member of the parser class, and an
additional argument for its constructor.

Type of parser: **token** [¶](#index-token-1)
:   A structure that contains (only) the `token_kind_type` enumeration,
    which defines the tokens. To refer to the token `FOO`, use
    `yy::parser::token::FOO`. The scanner can use ‘`typedef
    yy::parser::token token;`’ to “import” the token enumeration (see [Calc++ Scanner](#Calc_002b_002b-Scanner)).

Type of parser: **token\_kind\_type** [¶](#index-token_005fkind_005ftype)
:   An enumeration of the token kinds. Its enumerators are forged from the
    token names, with a possible token prefix
    (see [`api.token.prefix`](#api_002dtoken_002dprefix)):

    ```
    /// Token kinds.
    struct token
    {
      enum token_kind_type
      {
        YYEMPTY = -2,              // No token.
        YYEOF = 0,                 // "end of file"
        YYerror = 256,             // error
        YYUNDEF = 257,             // "invalid token"
        PLUS = 258,                // "+"
        MINUS = 259,               // "-"
        [...]
        VAR = 271,                 // "variable"
        NEG = 272                  // NEG
      };
    };

    /// Token kind, as returned by yylex.
    typedef token::token_kind_type token_kind_type;
    ```

Type of parser: **value\_type** [¶](#index-value_005ftype)
:   The types for semantic values. See [C++ Semantic Values](#C_002b_002b-Semantic-Values).

Type of parser: **location\_type** [¶](#index-location_005ftype)
:   The type of locations, if location tracking is enabled. See [C++ Location Values](#C_002b_002b-Location-Values).

Type of parser: **syntax\_error** [¶](#index-syntax_005ferror)
:   This class derives from `std::runtime_error`. Throw instances of it
    from the scanner or from the actions to raise parse errors. This is
    equivalent with first invoking `error` to report the location and
    message of the syntax error, and then to invoke `YYERROR` to enter the
    error-recovery mode. But contrary to `YYERROR` which can only be
    invoked from user actions (i.e., written in the action itself), the
    exception can be thrown from functions invoked from the user action.

Constructor on parser: **parser** *()* [¶](#index-parser-on-parser)

Constructor on parser: **parser** *(type1 arg1, ...)* [¶](#index-parser-on-parser-1)
:   Build a new parser object. There are no arguments, unless
    ‘`%parse-param {type1 arg1}`’ was used.

Constructor on syntax\_error: **syntax\_error** *(`const location_type&` l, `const std::string&` m)* [¶](#index-syntax_005ferror-on-syntax_005ferror)

Constructor on syntax\_error: **syntax\_error** *(`const std::string&` m)* [¶](#index-syntax_005ferror-on-syntax_005ferror-1)
:   Instantiate a syntax-error exception.

Method on parser: *int* **operator()** *()* [¶](#index-operator_0028_0029-on-parser)

Method on parser: *int* **parse** *()* [¶](#index-parse-on-parser)
:   Run the syntactic analysis, and return 0 on success, 1 otherwise. Both
    routines are equivalent, `operator()` being more C++ish.

    The whole function is wrapped in a `try`/`catch` block, so that
    when an exception is thrown, the `%destructor`s are called to release
    the lookahead symbol, and the symbols pushed on the stack.

    Exception related code in the generated parser is protected by CPP guards
    (`#if`) and disabled when exceptions are not supported (i.e., passing
    `-fno-exceptions` to the C++ compiler).

Method on parser: *std::ostream&* **debug\_stream** *()* [¶](#index-debug_005fstream-on-parser)

Method on parser: *void* **set\_debug\_stream** *(`std::ostream&` o)* [¶](#index-set_005fdebug_005fstream-on-parser)
:   Get or set the stream used for tracing the parsing. It defaults to
    `std::cerr`.

Method on parser: *debug\_level\_type* **debug\_level** *()* [¶](#index-debug_005flevel-on-parser)

Method on parser: *void* **set\_debug\_level** *(debug\_level\_type l)* [¶](#index-set_005fdebug_005flevel-on-parser)
:   Get or set the tracing level (an integral). Currently its value is either
    0, no trace, or nonzero, full tracing.

Method on parser: *void* **error** *(`const location_type&` l, `const std::string&` m)* [¶](#index-error-on-parser)

Method on parser: *void* **error** *(`const std::string&` m)* [¶](#index-error-on-parser-1)
:   The definition for this member function must be supplied by the user: the
    parser uses it to report a parser error occurring at l, described by
    m. If location tracking is not enabled, the second signature is used.

---

Next: [C++ Location Values](#C_002b_002b-Location-Values), Previous: [C++ Parser Interface](#C_002b_002b-Parser-Interface), Up: [C++ Parsers](#C_002b_002b-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.4 C++ Semantic Values

Bison supports two different means to handle semantic values in C++. One is
alike the C interface, and relies on unions. As C++ practitioners know,
unions are inconvenient in C++, therefore another approach is provided,
based on variants.

* [C++ Unions](#C_002b_002b-Unions)
* [C++ Variants](#C_002b_002b-Variants)

---

Next: [C++ Variants](#C_002b_002b-Variants), Up: [C++ Semantic Values](#C_002b_002b-Semantic-Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.4.1 C++ Unions

The `%union` directive works as for C, see [The Union Declaration](#Union-Decl). In
particular it produces a genuine `union`, which have a few specific
features in C++.

* - The value type is `yy::parser::value_type`, not `YYSTYPE`.
* - Non POD (Plain Old Data) types cannot be used. C++98 forbids any instance
  of classes with constructors in unions: only *pointers* to such objects
  are allowed. C++11 relaxed this constraints, but at the cost of safety.

Because objects have to be stored via pointers, memory is not
reclaimed automatically: using the `%destructor` directive is the
only means to avoid leaks. See [Freeing Discarded Symbols](#Destructor-Decl).

---

Previous: [C++ Unions](#C_002b_002b-Unions), Up: [C++ Semantic Values](#C_002b_002b-Semantic-Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.4.2 C++ Variants

Bison provides a *variant* based implementation of semantic values for
C++. This alleviates all the limitations reported in the previous section,
and in particular, object types can be used without pointers.

To enable variant-based semantic values, set the `%define` variable
`api.value.type` to `variant` (see [%define Summary](#g_t_0025define-Summary)). Then
`%union` is ignored; instead of using the name of the fields of the
`%union` to “type” the symbols, use genuine types.

For instance, instead of:

```
%union
{
  int ival;
  std::string* sval;
}
%token <ival> NUMBER;
%token <sval> STRING;
```

write:

```
%token <int> NUMBER;
%token <std::string> STRING;
```

`STRING` is no longer a pointer, which should fairly simplify the user
actions in the grammar and in the scanner (in particular the memory
management).

Since C++ features destructors, and since it is customary to specialize
`operator<<` to support uniform printing of values, variants also
typically simplify Bison printers and destructors.

Variants are stricter than unions. When based on unions, you may play any
dirty game with `yylval`, say storing an `int`, reading a
`char*`, and then storing a `double` in it. This is no longer
possible with variants: they must be initialized, then assigned to, and
eventually, destroyed. As a matter of fact, Bison variants forbid the use
of alternative types such as ‘`$<int>2`’ or ‘`$<std::string>$`’, even
in midrule actions. It is mandatory to use typed midrule actions
(see [Typed Midrule Actions](#Typed-Midrule-Actions)).

Method on value\_type: *T&* **emplace<T>** *()* [¶](#index-emplace_003cT_003e-on-value_005ftype)

Method on value\_type: *T&* **emplace<T>** *(`const T&` t)* [¶](#index-emplace_003cT_003e-on-value_005ftype-1)
:   Available in C++98/C++03 only. Default construct/copy-construct from
    t. Return a reference to where the actual value may be stored.
    Requires that the variant was not initialized yet.

Method on value\_type: *T&* **emplace<T, U>** *(`U&&...` u)* [¶](#index-emplace_003cT_002c-U_003e-on-value_005ftype)
:   Available in C++11 and later only. Build a variant of type `T` from
    the variadic forwarding references u....

**Warning**: We do not use Boost.Variant, for two reasons. First, it
appeared unacceptable to require Boost on the user’s machine (i.e., the
machine on which the generated parser will be compiled, not the machine on
which `bison` was run). Second, for each possible semantic value,
Boost.Variant not only stores the value, but also a tag specifying its
type. But the parser already “knows” the type of the semantic value, so
that would be duplicating the information.

We do not use C++17’s `std::variant` either: we want to support all the
C++ standards, and of course `std::variant` also stores a tag to record
the current type.

Therefore we developed light-weight variants whose type tag is external (so
they are really like `unions` for C++ actually). There is a number of
limitations in (the current implementation of) variants:

* Alignment must be enforced: values should be aligned in memory according to
  the most demanding type. Computing the smallest alignment possible requires
  meta-programming techniques that are not currently implemented in Bison, and
  therefore, since, as far as we know, `double` is the most demanding
  type on all platforms, alignments are enforced for `double` whatever
  types are actually used. This may waste space in some cases.
* There might be portability issues we are not aware of.

As far as we know, these limitations *can* be alleviated. All it takes
is some time and/or some talented C++ hacker willing to contribute to Bison.

---

Next: [C++ Parser Context](#C_002b_002b-Parser-Context), Previous: [C++ Semantic Values](#C_002b_002b-Semantic-Values), Up: [C++ Parsers](#C_002b_002b-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.5 C++ Location Values

When the directive `%locations` is used, the C++ parser supports
location tracking, see [Tracking Locations](#Tracking-Locations).

By default, two auxiliary classes define a `position`, a single point
in a file, and a `location`, a range composed of a pair of
`position`s (possibly spanning several files). If the `%define`
variable `api.location.type` is defined, then these classes will not be
generated, and the user defined type will be used.

* [C++ `position`](#C_002b_002b-position)
* [C++ `location`](#C_002b_002b-location)
* [Exposing the Location Classes](#Exposing-the-Location-Classes)
* [User Defined Location Type](#User-Defined-Location-Type)

---

Next: [C++ `location`](#C_002b_002b-location), Up: [C++ Location Values](#C_002b_002b-Location-Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.5.1 C++ `position`

Type of position: **filename\_type** [¶](#index-filename_005ftype)
:   The base type for file names. Defaults to `const std::string`.
    See [`api.filename.type`](#api_002dfilename_002dtype), to change its definition.

Type of position: **counter\_type** [¶](#index-counter_005ftype)
:   The type used to store line and column numbers. Defined as `int`.

Constructor on position: **position** *(`filename_type*` file = nullptr, `counter_type` line = 1, `counter_type` col = 1)* [¶](#index-position-on-position)
:   Create a `position` denoting a given point. Note that `file` is
    not reclaimed when the `position` is destroyed: memory managed must be
    handled elsewhere.

Method on position: *void* **initialize** *(`filename_type*` file = nullptr, `counter_type` line = 1, `counter_type` col = 1)* [¶](#index-initialize-on-position)
:   Reset the position to the given values.

Instance Variable of position: *filename\_type\** **file** [¶](#index-file-of-position)
:   The name of the file. It will always be handled as a pointer, the parser
    will never duplicate nor deallocate it.

Instance Variable of position: *counter\_type* **line** [¶](#index-line-of-position)
:   The line, starting at 1.

Method on position: *void* **lines** *(`counter_type` height = 1)* [¶](#index-lines-on-position)
:   If height is not null, advance by height lines, resetting the
    column number. The resulting line number cannot be less than 1.

Instance Variable of position: *counter\_type* **column** [¶](#index-column-of-position)
:   The column, starting at 1.

Method on position: *void* **columns** *(`counter_type` width = 1)* [¶](#index-columns-on-position)
:   Advance by width columns, without changing the line number. The
    resulting column number cannot be less than 1.

Method on position: *position&* **operator+=** *(`counter_type` width)* [¶](#index-operator_002b_003d-on-position)

Method on position: *position* **operator+** *(`counter_type` width)* [¶](#index-operator_002b-on-position)

Method on position: *position&* **operator-=** *(`counter_type` width)* [¶](#index-operator_002d_003d-on-position)

Method on position: *position* **operator-** *(`counter_type` width)* [¶](#index-operator_002d-on-position)
:   Various forms of syntactic sugar for `columns`.

Method on position: *bool* **operator==** *(`const position&` that)* [¶](#index-operator_003d_003d-on-position)

Method on position: *bool* **operator!=** *(`const position&` that)* [¶](#index-operator_0021_003d-on-position)
:   Whether `*this` and `that` denote equal/different positions.

Function: *std::ostream&* **operator<<** *(`std::ostream&` o, `const position&` p)* [¶](#index-operator_003c_003c)
:   Report p on o like this:
    ‘`file:line.column`’, or
    ‘`line.column`’ if file is null.

---

Next: [Exposing the Location Classes](#Exposing-the-Location-Classes), Previous: [C++ `position`](#C_002b_002b-position), Up: [C++ Location Values](#C_002b_002b-Location-Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.5.2 C++ `location`

Constructor on location: **location** *(`const position&` begin, `const position&` end)* [¶](#index-location-on-location)
:   Create a `Location` from the endpoints of the range.

Constructor on location: **location** *(`const position&` pos = position())* [¶](#index-location-on-location-1)

Constructor on location: **location** *(`filename_type*` file, `counter_type` line, `counter_type` col)* [¶](#index-location-on-location-2)
:   Create a `Location` denoting an empty range located at a given point.

Method on location: *void* **initialize** *(`filename_type*` file = nullptr, `counter_type` line = 1, `counter_type` col = 1)* [¶](#index-initialize-on-location)
:   Reset the location to an empty range at the given values.

Instance Variable of location: *position* **begin** [¶](#index-begin-of-location)

Instance Variable of location: *position* **end** [¶](#index-end-of-location)
:   The first, inclusive, position of the range, and the first beyond.

Method on location: *void* **columns** *(`counter_type` width = 1)* [¶](#index-columns-on-location)

Method on location: *void* **lines** *(`counter_type` height = 1)* [¶](#index-lines-on-location)
:   Forwarded to the `end` position.

Method on location: *location* **operator+** *(`counter_type` width)* [¶](#index-operator_002b-on-location)

Method on location: *location* **operator+=** *(`counter_type` width)* [¶](#index-operator_002b_003d-on-location)

Method on location: *location* **operator-** *(`counter_type` width)* [¶](#index-operator_002d-on-location)

Method on location: *location* **operator-=** *(`counter_type` width)* [¶](#index-operator_002d_003d-on-location)
:   Various forms of syntactic sugar for `columns`.

Method on location: *location* **operator+** *(`const location&` end)* [¶](#index-operator_002b-on-location-1)

Method on location: *location* **operator+=** *(`const location&` end)* [¶](#index-operator_002b_003d-on-location-1)
:   Join two locations: starts at the position of the first one, and ends at the
    position of the second.

Method on location: *void* **step** *()* [¶](#index-step-on-location)
:   Move `begin` onto `end`.

Method on location: *bool* **operator==** *(`const location&` that)* [¶](#index-operator_003d_003d-on-location)

Method on location: *bool* **operator!=** *(`const location&` that)* [¶](#index-operator_0021_003d-on-location)
:   Whether `*this` and `that` denote equal/different ranges of
    positions.

Function: *std::ostream&* **operator<<** *(`std::ostream&` o, `const location&` p)* [¶](#index-operator_003c_003c-1)
:   Report p on o, taking care of special cases such as: no
    `filename` defined, or equal filename/line or column.

---

Next: [User Defined Location Type](#User-Defined-Location-Type), Previous: [C++ `location`](#C_002b_002b-location), Up: [C++ Location Values](#C_002b_002b-Location-Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.5.3 Exposing the Location Classes

When both `%header` and `%locations` are enabled, Bison generates
an additional file: `location.hh`. If you don’t use locations outside
of the parser, you may avoid its creation with ‘`%define
api.location.file none`’.

However this file is useful if, for instance, your parser builds an abstract
syntax tree decorated with locations: you may use Bison’s `location`
type independently of Bison’s parser. You may name the file differently,
e.g., ‘`%define api.location.file "include/ast/location.hh"`’: this name
can have directory components, or even be absolute. The way the location
file is included is controlled by `api.location.include`.

This way it is possible to have several parsers share the same location
file.

For instance, in `src/foo/parser.yy`, generate the
`include/ast/loc.hh` file:

```
// src/foo/parser.yy
%locations
%define api.namespace {foo}
%define api.location.file "include/ast/loc.hh"
%define api.location.include {<ast/loc.hh>}
```

and use it in `src/bar/parser.yy`:

```
// src/bar/parser.yy
%locations
%define api.namespace {bar}
%code requires {#include <ast/loc.hh>}
%define api.location.type {bar::location}
```

Absolute file names are supported; it is safe in your `Makefile` to
pass the flag
`-Dapi.location.file='"$(top_srcdir)/include/ast/loc.hh"'` to
`bison` for `src/foo/parser.yy`. The generated file will not
have references to this absolute path, thanks to ‘`%define
api.location.include {<ast/loc.hh>}`’. Adding ‘`-I
$(top_srcdir)/include`’ to your `CPPFLAGS` will suffice for the compiler
to find `ast/loc.hh`.

---

Previous: [Exposing the Location Classes](#Exposing-the-Location-Classes), Up: [C++ Location Values](#C_002b_002b-Location-Values)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.5.4 User Defined Location Type

Instead of using the built-in types you may use the `%define` variable
`api.location.type` to specify your own type:

```
%define api.location.type {LocationType}
```

The requirements over your LocationType are:

* it must be copyable;
* in order to compute the (default) value of `@$` in a reduction, the
  parser basically runs

  ```
  @$.begin = @1.begin;
  @$.end   = @N.end; // The location of last right-hand side symbol.
  ```

  so there must be copyable `begin` and `end` members;
* alternatively you may redefine the computation of the default location, in
  which case these members are not required (see [Default Action for Locations](#Location-Default-Action));
* if traces are enabled, then there must exist an ‘`std::ostream&
  operator<< (std::ostream& o, const LocationType& s)`’ function.

  

In programs with several C++ parsers, you may also use the `%define`
variable `api.location.type` to share a common set of built-in
definitions for `position` and `location`. For instance, one
parser `master/parser.yy` might use:

```
%header
%locations
%define api.namespace {master::}
```

to generate the `master/position.hh` and `master/location.hh`
files, reused by other parsers as follows:

```
%define api.location.type {master::location}
%code requires { #include <master/location.hh> }
```

---

Next: [C++ Scanner Interface](#C_002b_002b-Scanner-Interface), Previous: [C++ Location Values](#C_002b_002b-Location-Values), Up: [C++ Parsers](#C_002b_002b-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.6 C++ Parser Context

When ‘`%define parse.error custom`’ is used (see [The Syntax Error Reporting Function `yyreport_syntax_error`](#Syntax-Error-Reporting-Function)), the user must define the following function.

Method on parser: *void* **report\_syntax\_error** *(`const context_type&`ctx) `const`* [¶](#index-report_005fsyntax_005ferror-on-parser)
:   Report a syntax error to the user. Whether it uses `yyerror` is up to
    the user.

Use the following types and functions to build the error message.

Type of parser: **context** [¶](#index-context)
:   A type that captures the circumstances of the syntax error.

Type of parser: **symbol\_kind\_type** [¶](#index-symbol_005fkind_005ftype)
:   An enum of all the grammar symbols, tokens and nonterminals. Its
    enumerators are forged from the symbol names:

    ```
    struct symbol_kind
    {
      enum symbol_kind_type
      {
        S_YYEMPTY = -2,      // No symbol.
        S_YYEOF = 0,         // "end of file"
        S_YYERROR = 1,       // error
        S_YYUNDEF = 2,       // "invalid token"
        S_PLUS = 3,          // "+"
        S_MINUS = 4,         // "-"
        [...]
        S_VAR = 14,          // "variable"
        S_NEG = 15,          // NEG
        S_YYACCEPT = 16,     // $accept
        S_exp = 17,          // exp
        S_input = 18         // input
      };
    };
    typedef symbol_kind::symbol_kind_t symbol_kind_type;
    ```

Method on context: *const symbol\_type&* **lookahead** *() `const`* [¶](#index-lookahead-on-context)
:   The “unexpected” token: the lookahead that caused the syntax error.

Method on context: *symbol\_kind\_type* **token** *() `const`* [¶](#index-token-on-context)
:   The symbol kind of the lookahead token that caused the syntax error. Returns
    `symbol_kind::S_YYEMPTY` if there is no lookahead.

Method on context: *const location&* **location** *() `const`* [¶](#index-location-on-context)
:   The location of the syntax error (that of the lookahead).

Method on context: *int* **expected\_tokens** *(`symbol_kind_type` argv`[]`, `int` argc) `const`* [¶](#index-expected_005ftokens-on-context)
:   Fill argv with the expected tokens, which never includes
    `symbol_kind::S_YYEMPTY`, `symbol_kind::S_YYERROR`, or
    `symbol_kind::S_YYUNDEF`.

    Never put more than argc elements into argv, and on success
    return the number of tokens stored in argv. If there are more
    expected tokens than argc, fill argv up to argc and return
    0. If there are no expected tokens, also return 0, but set `argv[0]`
    to `symbol_kind::S_YYEMPTY`.

    If argv is null, return the size needed to store all the possible
    values, which is always less than `YYNTOKENS`.

Method on parser: *const char \** **symbol\_name** *(`symbol_kind_t` symbol) `const`* [¶](#index-symbol_005fname-on-parser)
:   The name of the symbol whose kind is symbol, possibly translated.

    Returns a `std::string` when `parse.error` is `verbose`.

A custom syntax error function looks as follows. This implementation is
inappropriate for internationalization, see the `c/bistromathic`
example for a better alternative.

```
void
yy::parser::report_syntax_error (const context& ctx)
{
  int res = 0;
  std::cerr << ctx.location () << ": syntax error";
  // Report the tokens expected at this point.
  {
    enum { TOKENMAX = 5 };
    symbol_kind_type expected[TOKENMAX];
    int n = ctx.expected_tokens (ctx, expected, TOKENMAX);
    for (int i = 0; i < n; ++i)
      std::cerr << i == 0 ? ": expected " : " or "
                << symbol_name (expected[i]);
  }
  // Report the unexpected token.
  {
    symbol_kind_type lookahead = ctx.token ();
    if (lookahead != symbol_kind::S_YYEMPTY)
      std::cerr << " before " << symbol_name (lookahead));
  }
  std::cerr << '\n';
}
```

You still must provide a `yyerror` function, used for instance to
report memory exhaustion.

---

Next: [A Complete C++ Example](#A-Complete-C_002b_002b-Example), Previous: [C++ Parser Context](#C_002b_002b-Parser-Context), Up: [C++ Parsers](#C_002b_002b-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.7 C++ Scanner Interface

The parser invokes the scanner by calling `yylex`. Contrary to C
parsers, C++ parsers are always pure: there is no point in using the
‘`%define api.pure`’ directive. The actual interface with `yylex`
depends whether you use unions, or variants.

* [Split Symbols](#Split-Symbols)
* [Complete Symbols](#Complete-Symbols)

---

Next: [Complete Symbols](#Complete-Symbols), Up: [C++ Scanner Interface](#C_002b_002b-Scanner-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.7.1 Split Symbols

The generated parser expects `yylex` to have the following prototype.

Function: *int* **yylex** *(`value_type*` yylval, `location_type*` yylloc, type1 arg1, …)* [¶](#index-yylex-1)

Function: *int* **yylex** *(`value_type*` yylval, type1 arg1, …)* [¶](#index-yylex-2)
:   Return the next token. Its kind is the return value, its semantic value and
    location (if enabled) being yylval and yylloc. Invocations of
    ‘`%lex-param {type1 arg1}`’ yield additional arguments.

Note that when using variants, the interface for `yylex` is the same,
but `yylval` is handled differently.

Regular union-based code in Lex scanner typically looks like:

```
[0-9]+   {
           yylval->ival = text_to_int (yytext);
           return yy::parser::token::INTEGER;
         }
[a-z]+   {
           yylval->sval = new std::string (yytext);
           return yy::parser::token::IDENTIFIER;
         }
```

Using variants, `yylval` is already constructed, but it is not
initialized. So the code would look like:

```
[0-9]+   {
           yylval->emplace<int> () = text_to_int (yytext);
           return yy::parser::token::INTEGER;
         }
[a-z]+   {
           yylval->emplace<std::string> () = yytext;
           return yy::parser::token::IDENTIFIER;
         }
```

or

```
[0-9]+   {
           yylval->emplace (text_to_int (yytext));
           return yy::parser::token::INTEGER;
         }
[a-z]+   {
           yylval->emplace (yytext);
           return yy::parser::token::IDENTIFIER;
         }
```

---

Previous: [Split Symbols](#Split-Symbols), Up: [C++ Scanner Interface](#C_002b_002b-Scanner-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.7.2 Complete Symbols

With both `%define api.value.type variant` and `%define
api.token.constructor`, the parser defines the type `symbol_type`, and
expects `yylex` to have the following prototype.

Function: *parser::symbol\_type* **yylex** *()* [¶](#index-yylex-3)

Function: *parser::symbol\_type* **yylex** *(type1 arg1, …)* [¶](#index-yylex-4)
:   Return a *complete* symbol, aggregating its type (i.e., the traditional
    value returned by `yylex`), its semantic value, and possibly its
    location. Invocations of ‘`%lex-param {type1 arg1}`’ yield
    additional arguments.

Type of parser: **symbol\_type** [¶](#index-symbol_005ftype)
:   A “complete symbol”, that binds together its kind, value and (when
    applicable) location.

Method on symbol\_type: *symbol\_kind\_type* **kind** *() `const`* [¶](#index-kind-on-symbol_005ftype)
:   The kind of this symbol.

Method on symbol\_type: *const char \** **name** *() `const`* [¶](#index-name-on-symbol_005ftype)
:   The name of the kind of this symbol.

    Returns a `std::string` when `parse.error` is `verbose`.

  

For each token kind, Bison generates named constructors as follows.

Constructor on parser::symbol\_type: **symbol\_type** *(`int` token, `const value_type&` value, `const location_type&` location)* [¶](#index-symbol_005ftype-on-parser_003a_003asymbol_005ftype)

Constructor on parser::symbol\_type: **symbol\_type** *(`int` token, `const location_type&` location)* [¶](#index-symbol_005ftype-on-parser_003a_003asymbol_005ftype-1)

Constructor on parser::symbol\_type: **symbol\_type** *(`int` token, `const value_type&` value)* [¶](#index-symbol_005ftype-on-parser_003a_003asymbol_005ftype-2)

Constructor on parser::symbol\_type: **symbol\_type** *(`int` token)* [¶](#index-symbol_005ftype-on-parser_003a_003asymbol_005ftype-3)
:   Build a complete terminal symbol for the token kind token (including
    the `api.token.prefix`), whose semantic value, if it has one, is
    value of adequate value\_type. Pass the location iff
    location tracking is enabled.

    Consistency between token and value\_type is checked via an
    `assert`.

For instance, given the following declarations:

```
%define api.token.prefix {TOK_}
%token <std::string> IDENTIFIER;
%token <int> INTEGER;
%token ':';
```

you may use these constructors:

```
symbol_type (int token, const std::string&, const location_type&);
symbol_type (int token, const int&, const location_type&);
symbol_type (int token, const location_type&);
```

Correct matching between token kinds and value types is checked via
`assert`; for instance, ‘`symbol_type (ID, 42)`’ would abort. Named
constructors are preferable (see below), as they offer better type safety
(for instance ‘`make_ID (42)`’ would not even compile), but symbol\_type
constructors may help when token kinds are discovered at run-time, e.g.,

```
[a-z]+   {
           if (auto i = lookup_keyword (yytext))
             return yy::parser::symbol_type (i, loc);
           else
             return yy::parser::make_ID (yytext, loc);
         }
```

  

Note that it is possible to generate and compile type incorrect code
(e.g. ‘`symbol_type (':', yytext, loc)`’). It will fail at run time,
provided the assertions are enabled (i.e., `-DNDEBUG` was not passed
to the compiler). Bison supports an alternative that guarantees that type
incorrect code will not even compile. Indeed, it generates *named
constructors* as follows.

Method on parser: *symbol\_type* **make\_token** *(`const value_type&` value, `const location_type&` location)* [¶](#index-make_005ftoken-on-parser)

Method on parser: *symbol\_type* **make\_token** *(`const location_type&` location)* [¶](#index-make_005ftoken-on-parser-1)

Method on parser: *symbol\_type* **make\_token** *(`const value_type&` value)* [¶](#index-make_005ftoken-on-parser-2)

Method on parser: *symbol\_type* **make\_token** *()* [¶](#index-make_005ftoken-on-parser-3)
:   Build a complete terminal symbol for the token kind token (not
    including the `api.token.prefix`), whose semantic value, if it has one,
    is value of adequate value\_type. Pass the location iff
    location tracking is enabled.

For instance, given the following declarations:

```
%define api.token.prefix {TOK_}
%token <std::string> IDENTIFIER;
%token <int> INTEGER;
%token COLON;
%token EOF 0;
```

Bison generates:

```
symbol_type make_IDENTIFIER (const std::string&, const location_type&);
symbol_type make_INTEGER (const int&, const location_type&);
symbol_type make_COLON (const location_type&);
symbol_type make_EOF (const location_type&);
```

which should be used in a scanner as follows.

```
[a-z]+   return yy::parser::make_IDENTIFIER (yytext, loc);
[0-9]+   return yy::parser::make_INTEGER (text_to_int (yytext), loc);
":"      return yy::parser::make_COLON (loc);
<<EOF>>  return yy::parser::make_EOF (loc);
```

Tokens that do not have an identifier are not accessible: you cannot simply
use characters such as `':'`, they must be declared with `%token`,
including the end-of-file token.

---

Previous: [C++ Scanner Interface](#C_002b_002b-Scanner-Interface), Up: [C++ Parsers](#C_002b_002b-Parsers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.8 A Complete C++ Example

This section demonstrates the use of a C++ parser with a simple but complete
example. This example should be available on your system, ready to compile,
in the directory `examples/c++/calc++`. It focuses on
the use of Bison, therefore the design of the various C++ classes is very
naive: no accessors, no encapsulation of members etc. We will use a Lex
scanner, and more precisely, a Flex scanner, to demonstrate the various
interactions. A hand-written scanner is actually easier to interface with.

* [Calc++ — C++ Calculator](#Calc_002b_002b-_002d_002d_002d-C_002b_002b-Calculator)
* [Calc++ Parsing Driver](#Calc_002b_002b-Parsing-Driver)
* [Calc++ Parser](#Calc_002b_002b-Parser)
* [Calc++ Scanner](#Calc_002b_002b-Scanner)
* [Calc++ Top Level](#Calc_002b_002b-Top-Level)

---

Next: [Calc++ Parsing Driver](#Calc_002b_002b-Parsing-Driver), Up: [A Complete C++ Example](#A-Complete-C_002b_002b-Example)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.8.1 Calc++ — C++ Calculator

Of course the grammar is dedicated to arithmetic, a single expression,
possibly preceded by variable assignments. An environment containing
possibly predefined variables such as `one` and `two`, is
exchanged with the parser. An example of valid input follows.

```
three := 3
seven := one + two * three
seven * seven
```

---

Next: [Calc++ Parser](#Calc_002b_002b-Parser), Previous: [Calc++ — C++ Calculator](#Calc_002b_002b-_002d_002d_002d-C_002b_002b-Calculator), Up: [A Complete C++ Example](#A-Complete-C_002b_002b-Example)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.8.2 Calc++ Parsing Driver

To support a pure interface with the parser (and the scanner) the technique
of the “parsing context” is convenient: a structure containing all the
data to exchange. Since, in addition to simply launch the parsing, there
are several auxiliary tasks to execute (open the file for scanning,
instantiate the parser etc.), we recommend transforming the simple parsing
context structure into a fully blown *parsing driver* class.

The declaration of this driver class, in `driver.hh`, is as follows.
The first part includes the CPP guard and imports the required standard
library components, and the declaration of the parser class.

```
#ifndef DRIVER_HH
# define DRIVER_HH
# include <string>
# include <map>
# include "parser.hh"
```

Then comes the declaration of the scanning function. Flex expects the
signature of `yylex` to be defined in the macro `YY_DECL`, and the
C++ parser expects it to be declared. We can factor both as follows.

```
// Give Flex the prototype of yylex we want ...
# define YY_DECL \
  yy::parser::symbol_type yylex (driver& drv)
// ... and declare it for the parser's sake.
YY_DECL;
```

The `driver` class is then declared with its most obvious members.

```
// Conducting the whole scanning and parsing of Calc++.
class driver
{
public:
  driver ();

  std::map<std::string, int> variables;

  int result;
```

The main routine is of course calling the parser.

```
  // Run the parser on file F.  Return 0 on success.
  int parse (const std::string& f);
  // The name of the file being parsed.
  std::string file;
  // Whether to generate parser debug traces.
  bool trace_parsing;
```

To encapsulate the coordination with the Flex scanner, it is useful to have
member functions to open and close the scanning phase.

```
  // Handling the scanner.
  void scan_begin ();
  void scan_end ();
  // Whether to generate scanner debug traces.
  bool trace_scanning;
  // The token's location used by the scanner.
  yy::location location;
};
#endif // ! DRIVER_HH
```

The implementation of the driver (`driver.cc`) is straightforward.

```
#include "driver.hh"
#include "parser.hh"
```

```
driver::driver ()
  : trace_parsing (false), trace_scanning (false)
{
  variables["one"] = 1;
  variables["two"] = 2;
}
```

The `parse` member function deserves some attention.

```
int
driver::parse (const std::string &f)
{
  file = f;
  location.initialize (&file);
  scan_begin ();
  yy::parser parse (*this);
  parse.set_debug_level (trace_parsing);
  int res = parse ();
  scan_end ();
  return res;
}
```

---

Next: [Calc++ Scanner](#Calc_002b_002b-Scanner), Previous: [Calc++ Parsing Driver](#Calc_002b_002b-Parsing-Driver), Up: [A Complete C++ Example](#A-Complete-C_002b_002b-Example)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.8.3 Calc++ Parser

The grammar file `parser.yy` starts by asking for the C++ deterministic
parser skeleton, the creation of the parser header file. Because the C++
skeleton changed several times, it is safer to require the version you
designed the grammar for.

```
%skeleton "lalr1.cc" // -*- C++ -*-
%require "3.8.1"
%header
```

Because our scanner returns only genuine tokens and never simple characters
(i.e., it returns ‘`PLUS`’, not ‘`'+'`’), we can avoid conversions.

```
%define api.token.raw
```

This example uses genuine C++ objects as semantic values, therefore, we
require the variant-based storage of semantic values. To make sure we
properly use it, we enable assertions. To fully benefit from type-safety
and more natural definition of “symbol”, we enable
`api.token.constructor`.

```
%define api.token.constructor
%define api.value.type variant
%define parse.assert
```

Then come the declarations/inclusions needed by the semantic values.
Because the parser uses the parsing driver and reciprocally, both would like
to include the header of the other, which is, of course, insane. This
mutual dependency will be broken using forward declarations. Because the
driver’s header needs detailed knowledge about the parser class (in
particular its inner types), it is the parser’s header which will use a
forward declaration of the driver. See [%code Summary](#g_t_0025code-Summary).

```
%code requires {
  # include <string>
  class driver;
}
```

The driver is passed by reference to the parser and to the scanner.
This provides a simple but effective pure interface, not relying on
global variables.

```
// The parsing context.
%param { driver& drv }
```

Then we request location tracking.

```
%locations
```

Use the following two directives to enable parser tracing and detailed error
messages. However, detailed error messages can contain incorrect
information if lookahead correction is not enabled (see [LAC](#LAC)).

```
%define parse.trace
%define parse.error detailed
%define parse.lac full
```

The code between ‘`%code {`’ and ‘`}`’ is output in the `*.cc`
file; it needs detailed knowledge about the driver.

```
%code {
# include "driver.hh"
}
```

User friendly names are provided for each symbol. To avoid name clashes in
the generated files (see [Calc++ Scanner](#Calc_002b_002b-Scanner)), prefix tokens with `TOK_`
(see [%define Summary](#g_t_0025define-Summary)).

```
%define api.token.prefix {TOK_}
%token
  ASSIGN  ":="
  MINUS   "-"
  PLUS    "+"
  STAR    "*"
  SLASH   "/"
  LPAREN  "("
  RPAREN  ")"
;
```

Since we use variant-based semantic values, `%union` is not used, and
`%token`, `%nterm` and `%type` expect genuine types, not type
tags.

```
%token <std::string> IDENTIFIER "identifier"
%token <int> NUMBER "number"
%nterm <int> exp
```

No `%destructor` is needed to enable memory deallocation during error
recovery; the memory, for strings for instance, will be reclaimed by the
regular destructors. All the values are printed using their
`operator<<` (see [Printing Semantic Values](#Printer-Decl)).

```
%printer { yyo << $$; } <*>;
```

The grammar itself is straightforward (see [Location Tracking Calculator: `ltcalc`](#Location-Tracking-Calc)).

```
%%
%start unit;
unit: assignments exp  { drv.result = $2; };

assignments:
  %empty                 {}
| assignments assignment {};

assignment:
  "identifier" ":=" exp { drv.variables[$1] = $3; };

%left "+" "-";
%left "*" "/";
exp:
  "number"
| "identifier"  { $$ = drv.variables[$1]; }
| exp "+" exp   { $$ = $1 + $3; }
| exp "-" exp   { $$ = $1 - $3; }
| exp "*" exp   { $$ = $1 * $3; }
| exp "/" exp   { $$ = $1 / $3; }
| "(" exp ")"   { $$ = $2; }
%%
```

Finally the `error` member function reports the errors.

```
void
yy::parser::error (const location_type& l, const std::string& m)
{
  std::cerr << l << ": " << m << '\n';
}
```

---

Next: [Calc++ Top Level](#Calc_002b_002b-Top-Level), Previous: [Calc++ Parser](#Calc_002b_002b-Parser), Up: [A Complete C++ Example](#A-Complete-C_002b_002b-Example)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.8.4 Calc++ Scanner

In addition to standard headers, the Flex scanner includes the driver’s,
then the parser’s to get the set of defined tokens.

```
%{ /* -*- C++ -*- */
# include <cerrno>
# include <climits>
# include <cstdlib>
# include <cstring> // strerror
# include <string>
# include "driver.hh"
# include "parser.hh"
%}
```

Since our calculator has no `#include`-like feature, we don’t need
`yywrap`. We don’t need the `unput` and `input` functions
either, and we parse an actual file, this is not an interactive session with
the user. Finally, we enable scanner tracing.

```
%option noyywrap nounput noinput batch debug
```

The following function will be handy to convert a string denoting a number
into a `NUMBER` token.

```
%{
  // A number symbol corresponding to the value in S.
  yy::parser::symbol_type
  make_NUMBER (const std::string &s, const yy::parser::location_type& loc);
%}
```

Abbreviations allow for more readable rules.

```
id    [a-zA-Z][a-zA-Z_0-9]*
int   [0-9]+
blank [ \t\r]
```

The following paragraph suffices to track locations accurately. Each time
`yylex` is invoked, the begin position is moved onto the end position.
Then when a pattern is matched, its width is added to the end column. When
matching ends of lines, the end cursor is adjusted, and each time blanks are
matched, the begin cursor is moved onto the end cursor to effectively ignore
the blanks preceding tokens. Comments would be treated equally.

```
%{
  // Code run each time a pattern is matched.
  # define YY_USER_ACTION  loc.columns (yyleng);
%}
```

```
%%
```

```
%{
  // A handy shortcut to the location held by the driver.
  yy::location& loc = drv.location;
  // Code run each time yylex is called.
  loc.step ();
%}
```

```
{blank}+   loc.step ();
\n+        loc.lines (yyleng); loc.step ();
```

The rules are simple. The driver is used to report errors.

```
"-"        return yy::parser::make_MINUS  (loc);
"+"        return yy::parser::make_PLUS   (loc);
"*"        return yy::parser::make_STAR   (loc);
"/"        return yy::parser::make_SLASH  (loc);
"("        return yy::parser::make_LPAREN (loc);
")"        return yy::parser::make_RPAREN (loc);
":="       return yy::parser::make_ASSIGN (loc);

{int}      return make_NUMBER (yytext, loc);
{id}       return yy::parser::make_IDENTIFIER (yytext, loc);
```

```
.          {
             throw yy::parser::syntax_error
               (loc, "invalid character: " + std::string(yytext));
}
```

```
<<EOF>>    return yy::parser::make_YYEOF (loc);
%%
```

You should keep your rules simple, both in the parser and in the scanner.
Throwing from the auxiliary functions is then very handy to report errors.

```
yy::parser::symbol_type
make_NUMBER (const std::string &s, const yy::parser::location_type& loc)
{
  errno = 0;
  long n = strtol (s.c_str(), NULL, 10);
  if (! (INT_MIN <= n && n <= INT_MAX && errno != ERANGE))
    throw yy::parser::syntax_error (loc, "integer is out of range: " + s);
  return yy::parser::make_NUMBER ((int) n, loc);
}
```

Finally, because the scanner-related driver’s member-functions depend
on the scanner’s data, it is simpler to implement them in this file.

```
void
driver::scan_begin ()
{
  yy_flex_debug = trace_scanning;
  if (file.empty () || file == "-")
    yyin = stdin;
  else if (!(yyin = fopen (file.c_str (), "r")))
    {
      std::cerr << "cannot open " << file << ": " << strerror (errno) << '\n';
      exit (EXIT_FAILURE);
    }
}
```

```

```

```
void
driver::scan_end ()
{
  fclose (yyin);
}
```

---

Previous: [Calc++ Scanner](#Calc_002b_002b-Scanner), Up: [A Complete C++ Example](#A-Complete-C_002b_002b-Example)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]

#### 10.1.8.5 Calc++ Top Level

The top level file, `calc++.cc`, poses no problem.

```
#include <iostream>
#include "driver.hh"
```

```
int
main (int argc, char *argv[])
{
  int res = 0;
  driver drv;
  for (int i = 1; i < argc; ++i)
    if (argv[i] == std::string ("-p"))
      drv.trace_parsing = true;
    else if (argv[i] == std::string ("-s"))
      drv.trace_scanning = true;
    else if (!drv.parse (argv[i]))
      std::cout << drv.result << '\n';
    else
      res = 1;
  return res;
}
```

---

Next: [Java Parsers](#Java-Parsers), Previous: [C++ Parsers](#C_002b_002b-Parsers), Up: [Parsers Written In Other Languages](#Other-Languages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Terms "Index")]
