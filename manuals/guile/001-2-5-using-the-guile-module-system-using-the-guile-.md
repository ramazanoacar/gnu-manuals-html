### 2.5 Using the Guile Module System [¶](#Using-the-Guile-Module-System-1)

Guile has support for dividing a program into *modules*. By using
modules, you can group related code together and manage the
composition of complete programs from largely independent parts.

For more details on the module system beyond this introductory material,
See [Modules](#Modules).

* [Using Modules](#Using-Modules)
* [Writing new Modules](#Writing-new-Modules)
* [Putting Extensions into Modules](#Putting-Extensions-into-Modules)

---

Next: [Writing new Modules](#Writing-new-Modules), Up: [Using the Guile Module System](#Using-the-Guile-Module-System)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 2.5.1 Using Modules [¶](#Using-Modules-1)

Guile comes with a lot of useful modules, for example for string
processing or command line parsing. Additionally, there exist many
Guile modules written by other Guile hackers, but which have to be
installed manually.

Here is a sample interactive session that shows how to use the
`(ice-9 popen)` module which provides the means for communicating
with other processes over pipes together with the `(ice-9 rdelim)`
module that provides the function `read-line`.[1](#FOOT1)

```
$ guile
scheme@(guile-user)> (use-modules (ice-9 popen))
scheme@(guile-user)> (use-modules (ice-9 rdelim))
scheme@(guile-user)> (define p (open-input-pipe "ls -l"))
scheme@(guile-user)> (read-line p)
$1 = "total 30"
scheme@(guile-user)> (read-line p)
$2 = "drwxr-sr-x    2 mgrabmue mgrabmue     1024 Mar 29 19:57 CVS"
```

---

Next: [Putting Extensions into Modules](#Putting-Extensions-into-Modules), Previous: [Using Modules](#Using-Modules), Up: [Using the Guile Module System](#Using-the-Guile-Module-System)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 2.5.2 Writing new Modules [¶](#Writing-new-Modules-1)

You can create new modules using the syntactic form
`define-module`. All definitions following this form until the
next `define-module` are placed into the new module.

One module is usually placed into one file, and that file is installed
in a location where Guile can automatically find it. The following
session shows a simple example.

```
$ cat /usr/local/share/guile/site/foo/bar.scm

(define-module (foo bar)
  #:export (frob))

(define (frob x) (* 2 x))

$ guile
scheme@(guile-user)> (use-modules (foo bar))
scheme@(guile-user)> (frob 12)
$1 = 24
```

For more on how to install your module, see [Installing Site Packages](#Installing-Site-Packages).

---

Previous: [Writing new Modules](#Writing-new-Modules), Up: [Using the Guile Module System](#Using-the-Guile-Module-System)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 2.5.3 Putting Extensions into Modules [¶](#Putting-Extensions-into-Modules-1)

In addition to Scheme code you can also put things that are defined in
C into a module.

You do this by writing a small Scheme file that defines the module and
call `load-extension` directly in the body of the module.

```
$ cat /usr/local/share/guile/site/math/bessel.scm

(define-module (math bessel)
  #:export (j0))

(load-extension "libguile-bessel" "init_bessel")

$ file /usr/local/lib/guile/3.0/extensions/libguile-bessel.so
... ELF 32-bit LSB shared object ...
$ guile
scheme@(guile-user)> (use-modules (math bessel))
scheme@(guile-user)> (j0 2)
$1 = 0.223890779141236
```

See [Foreign Extensions](#Foreign-Extensions), for more information.

---

Previous: [Using the Guile Module System](#Using-the-Guile-Module-System), Up: [Hello Guile!](#Hello-Guile_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 2.6 Reporting Bugs [¶](#Reporting-Bugs-1)

Any problems with the installation should be reported to
[bug-guile@gnu.org](mailto:bug-guile@gnu.org).

If you find a bug in Guile, please report it to the Guile developers, so
they can fix it. They may also be able to suggest workarounds when it
is not possible for you to apply the bug-fix or install a new version of
Guile yourself.

Before sending in bug reports, please check with the following list that
you really have found a bug.

* Whenever documentation and actual behavior differ, you have certainly
  found a bug, either in the documentation or in the program.
* When Guile crashes, it is a bug.
* When Guile hangs or takes forever to complete a task, it is a bug.
* When calculations produce wrong results, it is a bug.
* When Guile signals an error for valid Scheme programs, it is a bug.
* When Guile does not signal an error for invalid Scheme programs, it may
  be a bug, unless this is explicitly documented.
* When some part of the documentation is not clear and does not make sense
  to you even after re-reading the section, it is a bug.

Before reporting the bug, check whether any programs you have loaded
into Guile, including your `.guile` file, set any variables that
may affect the functioning of Guile. Also, see whether the problem
happens in a freshly started Guile without loading your `.guile`
file (start Guile with the `-q` switch to prevent loading the init
file). If the problem does *not* occur then, you must report the
precise contents of any programs that you must load into Guile in order
to cause the problem to occur.

When you write a bug report, please make sure to include as much of the
information described below in the report. If you can’t figure out some
of the items, it is not a problem, but the more information we get, the
more likely we can diagnose and fix the bug.

* The version number of Guile. You can get this information from invoking
  ‘`guile --version`’ at your shell, or calling `(version)` from
  within Guile.
* Your machine type, as determined by the `config.guess` shell
  script. If you have a Guile checkout, this file is located in
  `build-aux`; otherwise you can fetch the latest version from
  <http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.guess;hb=HEAD>.

  ```
  $ build-aux/config.guess
  x86_64-unknown-linux-gnu
  ```
* If you installed Guile from a binary package, the version of that
  package. On systems that use RPM, use `rpm -qa | grep guile`. On systems
  that use DPKG, `dpkg -l | grep guile`.
* If you built Guile yourself, the build configuration that you used:

  ```
  $ ./config.status --config
  '--enable-error-on-warning' '--disable-deprecated'...
  ```
* A complete description of how to reproduce the bug.

  If you have a Scheme program that produces the bug, please include it in
  the bug report. If your program is too big to include, please try to
  reduce your code to a minimal test case.

  If you can reproduce your problem at the REPL, that is best. Give a
  transcript of the expressions you typed at the REPL.
* A description of the incorrect behavior. For example, "The Guile
  process gets a fatal signal," or, "The resulting output is as follows,
  which I think is wrong."

  If the manifestation of the bug is a Guile error message, it is
  important to report the precise text of the error message, and a
  backtrace showing how the Scheme program arrived at the error. This can
  be done using the `,backtrace` command in Guile’s debugger.

If your bug causes Guile to crash, additional information from a
low-level debugger such as GDB might be helpful. If you have built Guile
yourself, you can run Guile under GDB via the
`meta/gdb-uninstalled-guile` script. Instead of invoking Guile as
usual, invoke the wrapper script, type `run` to start the process,
then `backtrace` when the crash comes. Include that backtrace in
your report.

---

Next: [Programming in Scheme](#Programming-in-Scheme), Previous: [Hello Guile!](#Hello-Guile_0021), Up: [The Guile Reference Manual](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]## 3 Hello Scheme! [¶](#Hello-Scheme_0021-1)

In this chapter, we introduce the basic concepts that underpin the
elegance and power of the Scheme language.

Readers who already possess a background knowledge of Scheme may happily
skip this chapter. For the reader who is new to the language, however,
the following discussions on data, procedures, expressions and closure
are designed to provide a minimum level of Scheme understanding that is
more or less assumed by the chapters that follow.

The style of this introductory material aims about halfway between the terse
precision of R5RS and the discursiveness of existing Scheme tutorials. For
pointers to useful Scheme resources on the web, please see [Further Reading](#Further-Reading).

* [Data Types, Values and Variables](#About-Data)
* [The Representation and Use of Procedures](#About-Procedures)
* [Expressions and Evaluation](#About-Expressions)
* [The Concept of Closure](#About-Closure)
* [Further Reading](#Further-Reading)

---

Next: [The Representation and Use of Procedures](#About-Procedures), Up: [Hello Scheme!](#Hello-Scheme_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 3.1 Data Types, Values and Variables [¶](#Data-Types_002c-Values-and-Variables)

This section discusses the representation of data types and values, what
it means for Scheme to be a *latently typed* language, and the role
of variables. We conclude by introducing the Scheme syntaxes for
defining a new variable, and for changing the value of an existing
variable.

* [Latent Typing](#Latent-Typing)
* [Values and Variables](#Values-and-Variables)
* [Defining and Setting Variables](#Definition)

---

Next: [Values and Variables](#Values-and-Variables), Up: [Data Types, Values and Variables](#About-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.1.1 Latent Typing [¶](#Latent-Typing-1)

The term *latent typing* is used to describe a computer language,
such as Scheme, for which you cannot, *in general*, simply look at
a program’s source code and determine what type of data will be
associated with a particular variable, or with the result of a
particular expression.

Sometimes, of course, you *can* tell from the code what the type of
an expression will be. If you have a line in your program that sets the
variable `x` to the numeric value 1, you can be certain that,
immediately after that line has executed (and in the absence of multiple
threads), `x` has the numeric value 1. Or if you write a procedure
that is designed to concatenate two strings, it is likely that the rest
of your application will always invoke this procedure with two string
parameters, and quite probable that the procedure would go wrong in some
way if it was ever invoked with parameters that were not both strings.

Nevertheless, the point is that there is nothing in Scheme which
requires the procedure parameters always to be strings, or `x`
always to hold a numeric value, and there is no way of declaring in your
program that such constraints should always be obeyed. In the same
vein, there is no way to declare the expected type of a procedure’s
return value.

Instead, the types of variables and expressions are only known – in
general – at run time. If you *need* to check at some point that
a value has the expected type, Scheme provides run time procedures that
you can invoke to do so. But equally, it can be perfectly valid for two
separate invocations of the same procedure to specify arguments with
different types, and to return values with different types.

The next subsection explains what this means in practice, for the ways
that Scheme programs use data types, values and variables.

---

Next: [Defining and Setting Variables](#Definition), Previous: [Latent Typing](#Latent-Typing), Up: [Data Types, Values and Variables](#About-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.1.2 Values and Variables [¶](#Values-and-Variables-1)

Scheme provides many data types that you can use to represent your data.
Primitive types include characters, strings, numbers and procedures.
Compound types, which allow a group of primitive and compound values to
be stored together, include lists, pairs, vectors and multi-dimensional
arrays. In addition, Guile allows applications to define their own data
types, with the same status as the built-in standard Scheme types.

As a Scheme program runs, values of all types pop in and out of
existence. Sometimes values are stored in variables, but more commonly
they pass seamlessly from being the result of one computation to being
one of the parameters for the next.

Consider an example. A string value is created because the interpreter
reads in a literal string from your program’s source code. Then a
numeric value is created as the result of calculating the length of the
string. A second numeric value is created by doubling the calculated
length. Finally the program creates a list with two elements – the
doubled length and the original string itself – and stores this list in
a program variable.

All of the values involved here – in fact, all values in Scheme –
carry their type with them. In other words, every value “knows,” at
runtime, what kind of value it is. A number, a string, a list,
whatever.

A variable, on the other hand, has no fixed type. A variable –
`x`, say – is simply the name of a location – a box – in which
you can store any kind of Scheme value. So the same variable in a
program may hold a number at one moment, a list of procedures the next,
and later a pair of strings. The “type” of a variable – insofar as
the idea is meaningful at all – is simply the type of whatever value
the variable happens to be storing at a particular moment.

---

Previous: [Values and Variables](#Values-and-Variables), Up: [Data Types, Values and Variables](#About-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.1.3 Defining and Setting Variables [¶](#Defining-and-Setting-Variables)

To define a new variable, you use Scheme’s `define` syntax like
this:

```
(define variable-name value)
```

This makes a new variable called variable-name and stores
value in it as the variable’s initial value. For example:

```
;; Make a variable `x' with initial numeric value 1.
(define x 1)

;; Make a variable `organization' with an initial string value.
(define organization "Free Software Foundation")
```

(In Scheme, a semicolon marks the beginning of a comment that continues
until the end of the line. So the lines beginning `;;` are
comments.)

Changing the value of an already existing variable is very similar,
except that `define` is replaced by the Scheme syntax `set!`,
like this:

```
(set! variable-name new-value)
```

Remember that variables do not have fixed types, so new-value may
have a completely different type from whatever was previously stored in
the location named by variable-name. Both of the following
examples are therefore correct.

```
;; Change the value of `x' to 5.
(set! x 5)

;; Change the value of `organization' to the FSF's street number.
(set! organization 545)
```

In these examples, value and new-value are literal numeric
or string values. In general, however, value and new-value
can be any Scheme expression. Even though we have not yet covered the
forms that Scheme expressions can take (see [Expressions and Evaluation](#About-Expressions)), you
can probably guess what the following `set!` example does…

```
(set! x (+ x 1))
```

(Note: this is not a complete description of `define` and
`set!`, because we need to introduce some other aspects of Scheme
before the missing pieces can be filled in. If, however, you are
already familiar with the structure of Scheme, you may like to read
about those missing pieces immediately by jumping ahead to the following
references.

* [Lambda Alternatives](#Lambda-Alternatives), to read about an alternative form of the
  `define` syntax that can be used when defining new procedures.
* [Procedures with Setters](#Procedures-with-Setters), to read about an alternative form of the
  `set!` syntax that helps with changing a single value in the depths
  of a compound data structure.)
* See [Internal definitions](#Internal-Definitions), to read about using `define` other
  than at top level in a Scheme program, including a discussion of when it
  works to use `define` rather than `set!` to change the value
  of an existing variable.

---

Next: [Expressions and Evaluation](#About-Expressions), Previous: [Data Types, Values and Variables](#About-Data), Up: [Hello Scheme!](#Hello-Scheme_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 3.2 The Representation and Use of Procedures [¶](#The-Representation-and-Use-of-Procedures)

This section introduces the basics of using and creating Scheme
procedures. It discusses the representation of procedures as just
another kind of Scheme value, and shows how procedure invocation
expressions are constructed. We then explain how `lambda` is used
to create new procedures, and conclude by presenting the various
shorthand forms of `define` that can be used instead of writing an
explicit `lambda` expression.

* [Procedures as Values](#Procedures-as-Values)
* [Simple Procedure Invocation](#Simple-Invocation)
* [Creating and Using a New Procedure](#Creating-a-Procedure)
* [Lambda Alternatives](#Lambda-Alternatives)

---

Next: [Simple Procedure Invocation](#Simple-Invocation), Up: [The Representation and Use of Procedures](#About-Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.2.1 Procedures as Values [¶](#Procedures-as-Values-1)

One of the great simplifications of Scheme is that a procedure is just
another type of value, and that procedure values can be passed around
and stored in variables in exactly the same way as, for example, strings
and lists. When we talk about a built-in standard Scheme procedure such
as `open-input-file`, what we actually mean is that there is a
pre-defined top level variable called `open-input-file`, whose
value is a procedure that implements what R5RS says that
`open-input-file` should do.

Note that this is quite different from many dialects of Lisp —
including Emacs Lisp — in which a program can use the same name with
two quite separate meanings: one meaning identifies a Lisp function,
while the other meaning identifies a Lisp variable, whose value need
have nothing to do with the function that is associated with the first
meaning. In these dialects, functions and variables are said to live in
different *namespaces*.

In Scheme, on the other hand, all names belong to a single unified
namespace, and the variables that these names identify can hold any kind
of Scheme value, including procedure values.

One consequence of the “procedures as values” idea is that, if you
don’t happen to like the standard name for a Scheme procedure, you can
change it.

For example, `call-with-current-continuation` is a very important
standard Scheme procedure, but it also has a very long name! So, many
programmers use the following definition to assign the same procedure
value to the more convenient name `call/cc`.

```
(define call/cc call-with-current-continuation)
```

Let’s understand exactly how this works. The definition creates a new
variable `call/cc`, and then sets its value to the value of the
variable `call-with-current-continuation`; the latter value is a
procedure that implements the behavior that R5RS specifies under the
name “call-with-current-continuation”. So `call/cc` ends up
holding this value as well.

Now that `call/cc` holds the required procedure value, you could
choose to use `call-with-current-continuation` for a completely
different purpose, or just change its value so that you will get an
error if you accidentally use `call-with-current-continuation` as a
procedure in your program rather than `call/cc`. For example:

```
(set! call-with-current-continuation "Not a procedure any more!")
```

Or you could just leave `call-with-current-continuation` as it was.
It’s perfectly fine for more than one variable to hold the same
procedure value.

---

Next: [Creating and Using a New Procedure](#Creating-a-Procedure), Previous: [Procedures as Values](#Procedures-as-Values), Up: [The Representation and Use of Procedures](#About-Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.2.2 Simple Procedure Invocation [¶](#Simple-Procedure-Invocation)

A procedure invocation in Scheme is written like this:

```
(procedure [arg1 [arg2 ...]])
```

In this expression, procedure can be any Scheme expression whose
value is a procedure. Most commonly, however, procedure is simply
the name of a variable whose value is a procedure.

For example, `string-append` is a standard Scheme procedure whose
behavior is to concatenate together all the arguments, which are
expected to be strings, that it is given. So the expression

```
(string-append "/home" "/" "andrew")
```

is a procedure invocation whose result is the string value
`"/home/andrew"`.

Similarly, `string-length` is a standard Scheme procedure that
returns the length of a single string argument, so

```
(string-length "abc")
```

is a procedure invocation whose result is the numeric value 3.

Each of the parameters in a procedure invocation can itself be any
Scheme expression. Since a procedure invocation is itself a type of
expression, we can put these two examples together to get

```
(string-length (string-append "/home" "/" "andrew"))
```

— a procedure invocation whose result is the numeric value 12.

(You may be wondering what happens if the two examples are combined the
other way round. If we do this, we can make a procedure invocation
expression that is *syntactically* correct:

```
(string-append "/home" (string-length "abc"))
```

but when this expression is executed, it will cause an error, because
the result of `(string-length "abc")` is a numeric value, and
`string-append` is not designed to accept a numeric value as one of
its arguments.)

---

Next: [Lambda Alternatives](#Lambda-Alternatives), Previous: [Simple Procedure Invocation](#Simple-Invocation), Up: [The Representation and Use of Procedures](#About-Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.2.3 Creating and Using a New Procedure [¶](#Creating-and-Using-a-New-Procedure)

Scheme has lots of standard procedures, and Guile provides all of these
via predefined top level variables. All of these standard procedures
are documented in the later chapters of this reference manual.

Before very long, though, you will want to create new procedures that
encapsulate aspects of your own applications’ functionality. To do
this, you can use the famous `lambda` syntax.

For example, the value of the following Scheme expression

```
(lambda (name address) body ...)
```

is a newly created procedure that takes two arguments: `name` and
`address`. The behavior of the new procedure is determined by the
sequence of expressions and definitions in the body of the
procedure definition. (Typically, body would use the arguments in
some way, or else there wouldn’t be any point in giving them to the
procedure.) When invoked, the new procedure returns a value that is the
value of the last expression in the body.

To make things more concrete, let’s suppose that the two arguments are
both strings, and that the purpose of this procedure is to form a
combined string that includes these arguments. Then the full lambda
expression might look like this:

```
(lambda (name address)
  (string-append "Name=" name ":Address=" address))
```

We noted in the previous subsection that the procedure part of a
procedure invocation expression can be any Scheme expression whose value
is a procedure. But that’s exactly what a lambda expression is! So we
can use a lambda expression directly in a procedure invocation, like
this:

```
((lambda (name address)
   (string-append "Name=" name ":Address=" address))
 "FSF"
 "Cambridge")
```

This is a valid procedure invocation expression, and its result is the
string:

```
"Name=FSF:Address=Cambridge"
```

It is more common, though, to store the procedure value in a variable —

```
(define make-combined-string
  (lambda (name address)
    (string-append "Name=" name ":Address=" address)))
```

— and then to use the variable name in the procedure invocation:

```
(make-combined-string "FSF" "Cambridge")
```

Which has exactly the same result.

It’s important to note that procedures created using `lambda` have
exactly the same status as the standard built in Scheme procedures, and
can be invoked, passed around, and stored in variables in exactly the
same ways.

---

Previous: [Creating and Using a New Procedure](#Creating-a-Procedure), Up: [The Representation and Use of Procedures](#About-Procedures)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.2.4 Lambda Alternatives [¶](#Lambda-Alternatives-1)

Since it is so common in Scheme programs to want to create a procedure
and then store it in a variable, there is an alternative form of the
`define` syntax that allows you to do just that.

A `define` expression of the form

```
(define (name [arg1 [arg2 ...]])
  body ...)
```

is exactly equivalent to the longer form

```
(define name
  (lambda ([arg1 [arg2 ...]])
    body ...))
```

So, for example, the definition of `make-combined-string` in the
previous subsection could equally be written:

```
(define (make-combined-string name address)
  (string-append "Name=" name ":Address=" address))
```

This kind of procedure definition creates a procedure that requires
exactly the expected number of arguments. There are two further forms
of the `lambda` expression, which create a procedure that can
accept a variable number of arguments:

```
(lambda (arg1 ... . args) body ...)

(lambda args body ...)
```

The corresponding forms of the alternative `define` syntax are:

```
(define (name arg1 ... . args) body ...)

(define (name . args) body ...)
```

For details on how these forms work, see See [Lambda: Basic Procedure Creation](#Lambda).

Prior to Guile 2.0, Guile provided an extension to `define` syntax
that allowed you to nest the previous extension up to an arbitrary
depth. These are no longer provided by default, and instead have been
moved to [Curried Definitions](#Curried-Definitions).

(It could be argued that the alternative `define` forms are rather
confusing, especially for newcomers to the Scheme language, as they hide
both the role of `lambda` and the fact that procedures are values
that are stored in variables in the same way as any other kind of value.
On the other hand, they are very convenient, and they are also a good
example of another of Scheme’s powerful features: the ability to specify
arbitrary syntactic transformations at run time, which can be applied to
subsequently read input.)

---

Next: [The Concept of Closure](#About-Closure), Previous: [The Representation and Use of Procedures](#About-Procedures), Up: [Hello Scheme!](#Hello-Scheme_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 3.3 Expressions and Evaluation [¶](#Expressions-and-Evaluation)

So far, we have met expressions that *do* things, such as the
`define` expressions that create and initialize new variables, and
we have also talked about expressions that have *values*, for
example the value of the procedure invocation expression:

```
(string-append "/home" "/" "andrew")
```

but we haven’t yet been precise about what causes an expression like
this procedure invocation to be reduced to its “value”, or how the
processing of such expressions relates to the execution of a Scheme
program as a whole.

This section clarifies what we mean by an expression’s value, by
introducing the idea of *evaluation*. It discusses the side effects
that evaluation can have, explains how each of the various types of
Scheme expression is evaluated, and describes the behavior and use of
the Guile REPL as a mechanism for exploring evaluation. The section
concludes with a very brief summary of Scheme’s common syntactic
expressions.

* [Evaluating Expressions and Executing Programs](#Evaluating)
* [Tail calls](#Tail-Calls)
* [Using the Guile REPL](#The-REPL)
* [Summary of Common Syntax](#Syntax-Summary)

---

Next: [Tail calls](#Tail-Calls), Up: [Expressions and Evaluation](#About-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.3.1 Evaluating Expressions and Executing Programs [¶](#Evaluating-Expressions-and-Executing-Programs)

In Scheme, the process of executing an expression is known as
*evaluation*. Evaluation has two kinds of result:

* the *value* of the evaluated expression
* the *side effects* of the evaluation, which consist of any effects of
  evaluating the expression that are not represented by the value.

Of the expressions that we have met so far, `define` and
`set!` expressions have side effects — the creation or
modification of a variable — but no value; `lambda` expressions
have values — the newly constructed procedures — but no side
effects; and procedure invocation expressions, in general, have either
values, or side effects, or both.

It is tempting to try to define more intuitively what we mean by
“value” and “side effects”, and what the difference between them is.
In general, though, this is extremely difficult. It is also
unnecessary; instead, we can quite happily define the behavior of a
Scheme program by specifying how Scheme executes a program as a whole,
and then by describing the value and side effects of evaluation for each
type of expression individually.

So, some[2](#FOOT2) definitions…

* A Scheme program consists of a sequence of expressions.
* A Scheme interpreter executes the program by evaluating these
  expressions in order, one by one.
* An expression can be
  + a piece of literal data, such as a number `2.3` or a string
    `"Hello world!"`
  + a variable name
  + a procedure invocation expression
  + one of Scheme’s special syntactic expressions.

The following subsections describe how each of these types of expression
is evaluated.

* [Evaluating Literal Data](#Eval-Literal)
* [Evaluating a Variable Reference](#Eval-Variable)
* [Evaluating a Procedure Invocation Expression](#Eval-Procedure)
* [Evaluating Special Syntactic Expressions](#Eval-Special)

---

Next: [Evaluating a Variable Reference](#Eval-Variable), Up: [Evaluating Expressions and Executing Programs](#Evaluating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.3.1.1 Evaluating Literal Data [¶](#Evaluating-Literal-Data)

When a literal data expression is evaluated, the value of the expression
is simply the value that the expression describes. The evaluation of a
literal data expression has no side effects.

So, for example,

* the value of the expression `"abc"` is the string value
  `"abc"`
* the value of the expression `3+4i` is the complex number 3 + 4i
* the value of the expression `#(1 2 3)` is a three-element vector
  containing the numeric values 1, 2 and 3.

For any data type which can be expressed literally like this, the syntax
of the literal data expression for that data type — in other words,
what you need to write in your code to indicate a literal value of that
type — is known as the data type’s *read syntax*. This manual
specifies the read syntax for each such data type in the section that
describes that data type.

Some data types do not have a read syntax. Procedures, for example,
cannot be expressed as literal data; they must be created using a
`lambda` expression (see [Creating and Using a New Procedure](#Creating-a-Procedure)) or implicitly
using the shorthand form of `define` (see [Lambda Alternatives](#Lambda-Alternatives)).

---

Next: [Evaluating a Procedure Invocation Expression](#Eval-Procedure), Previous: [Evaluating Literal Data](#Eval-Literal), Up: [Evaluating Expressions and Executing Programs](#Evaluating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.3.1.2 Evaluating a Variable Reference [¶](#Evaluating-a-Variable-Reference)

When an expression that consists simply of a variable name is evaluated,
the value of the expression is the value of the named variable. The
evaluation of a variable reference expression has no side effects.

So, after

```
(define key "Paul Evans")
```

the value of the expression `key` is the string value `"Paul
Evans"`. If key is then modified by

```
(set! key 3.74)
```

the value of the expression `key` is the numeric value 3.74.

If there is no variable with the specified name, evaluation of the
variable reference expression signals an error.

---

Next: [Evaluating Special Syntactic Expressions](#Eval-Special), Previous: [Evaluating a Variable Reference](#Eval-Variable), Up: [Evaluating Expressions and Executing Programs](#Evaluating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.3.1.3 Evaluating a Procedure Invocation Expression [¶](#Evaluating-a-Procedure-Invocation-Expression)

This is where evaluation starts getting interesting! As already noted,
a procedure invocation expression has the form

```
(procedure [arg1 [arg2 ...]])
```

where procedure must be an expression whose value, when evaluated,
is a procedure.

The evaluation of a procedure invocation expression like this proceeds
by

* evaluating individually the expressions procedure, arg1,
  arg2, and so on
* calling the procedure that is the value of the procedure
  expression with the list of values obtained from the evaluations of
  arg1, arg2 etc. as its parameters.

For a procedure defined in Scheme, “calling the procedure with the list
of values as its parameters” means binding the values to the
procedure’s formal parameters and then evaluating the sequence of
expressions that make up the body of the procedure definition. The
value of the procedure invocation expression is the value of the last
evaluated expression in the procedure body. The side effects of calling
the procedure are the combination of the side effects of the sequence of
evaluations of expressions in the procedure body.

For a built-in procedure, the value and side-effects of calling the
procedure are best described by that procedure’s documentation.

Note that the complete side effects of evaluating a procedure invocation
expression consist not only of the side effects of the procedure call,
but also of any side effects of the preceding evaluation of the
expressions procedure, arg1, arg2, and so on.

To illustrate this, let’s look again at the procedure invocation
expression:

```
(string-length (string-append "/home" "/" "andrew"))
```

In the outermost expression, procedure is `string-length` and
arg1 is `(string-append "/home" "/" "andrew")`.

* Evaluation of `string-length`, which is a variable, gives a
  procedure value that implements the expected behavior for
  “string-length”.
* Evaluation of `(string-append "/home" "/" "andrew")`, which is
  another procedure invocation expression, means evaluating each of
  + `string-append`, which gives a procedure value that implements the
    expected behavior for “string-append”
  + `"/home"`, which gives the string value `"/home"`
  + `"/"`, which gives the string value `"/"`
  + `"andrew"`, which gives the string value `"andrew"`

  and then invoking the procedure value with this list of string values as
  its arguments. The resulting value is a single string value that is the
  concatenation of all the arguments, namely `"/home/andrew"`.

In the evaluation of the outermost expression, the interpreter can now
invoke the procedure value obtained from procedure with the value
obtained from arg1 as its arguments. The resulting value is a
numeric value that is the length of the argument string, which is 12.

---

Previous: [Evaluating a Procedure Invocation Expression](#Eval-Procedure), Up: [Evaluating Expressions and Executing Programs](#Evaluating)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.3.1.4 Evaluating Special Syntactic Expressions [¶](#Evaluating-Special-Syntactic-Expressions)

When a procedure invocation expression is evaluated, the procedure and
*all* the argument expressions must be evaluated before the
procedure can be invoked. Special syntactic expressions are special
because they are able to manipulate their arguments in an unevaluated
form, and can choose whether to evaluate any or all of the argument
expressions.

Why is this needed? Consider a program fragment that asks the user
whether or not to delete a file, and then deletes the file if the user
answers yes.

```
(if (string=? (read-answer "Should I delete this file?")
              "yes")
    (delete-file file))
```

If the outermost `(if …)` expression here was a procedure
invocation expression, the expression `(delete-file file)`, whose
side effect is to actually delete a file, would already have been
evaluated before the `if` procedure even got invoked! Clearly this
is no use — the whole point of an `if` expression is that the
*consequent* expression is only evaluated if the condition of the
`if` expression is “true”.

Therefore `if` must be special syntax, not a procedure. Other
special syntaxes that we have already met are `define`, `set!`
and `lambda`. `define` and `set!` are syntax because
they need to know the variable *name* that is given as the first
argument in a `define` or `set!` expression, not that
variable’s value. `lambda` is syntax because it does not
immediately evaluate the expressions that define the procedure body;
instead it creates a procedure object that incorporates these
expressions so that they can be evaluated in the future, when that
procedure is invoked.

The rules for evaluating each special syntactic expression are specified
individually for each special syntax. For a summary of standard special
syntax, see See [Summary of Common Syntax](#Syntax-Summary).

---

Next: [Using the Guile REPL](#The-REPL), Previous: [Evaluating Expressions and Executing Programs](#Evaluating), Up: [Expressions and Evaluation](#About-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.3.2 Tail calls [¶](#Tail-calls)

Scheme is “properly tail recursive”, meaning that tail calls or
recursions from certain contexts do not consume stack space or other
resources and can therefore be used on arbitrarily large data or for
an arbitrarily long calculation. Consider for example,

```
(define (foo n)
  (display n)
  (newline)
  (foo (1+ n)))

(foo 1)
⊣
1
2
3
...
```

`foo` prints numbers infinitely, starting from the given n.
It’s implemented by printing n then recursing to itself to print
*n+1* and so on. This recursion is a tail call, it’s the
last thing done, and in Scheme such tail calls can be made without
limit.

Or consider a case where a value is returned, a version of the SRFI-1
`last` function (see [Selectors](#SRFI_002d1-Selectors)) returning the last
element of a list,

```
(define (my-last lst)
  (if (null? (cdr lst))
      (car lst)
      (my-last (cdr lst))))

(my-last '(1 2 3)) ⇒ 3
```

If the list has more than one element, `my-last` applies itself
to the `cdr`. This recursion is a tail call, there’s no code
after it, and the return value is the return value from that call. In
Scheme this can be used on an arbitrarily long list argument.

  

A proper tail call is only available from certain contexts, namely the
following special form positions,

* `and` — last expression
* `begin` — last expression
* `case` — last expression in each clause
* `cond` — last expression in each clause, and the call to a
  `=>` procedure is a tail call
* `do` — last result expression
* `if` — “true” and “false” leg expressions
* `lambda` — last expression in body
* `let`, `let*`, `letrec`, `let-syntax`,
  `letrec-syntax` — last expression in body
* `or` — last expression

The following core functions make tail calls,

* `apply` — tail call to given procedure
* `call-with-current-continuation` — tail call to the procedure
  receiving the new continuation
* `call-with-values` — tail call to the values-receiving
  procedure
* `eval` — tail call to evaluate the form
* `string-any`, `string-every` — tail call to predicate on
  the last character (if that point is reached)

  

The above are just core functions and special forms. Tail calls in
other modules are described with the relevant documentation, for
example SRFI-1 `any` and `every` (see [Searching](#SRFI_002d1-Searching)).

It will be noted there are a lot of places which could potentially be
tail calls, for instance the last call in a `for-each`, but only
those explicitly described are guaranteed.

---

Next: [Summary of Common Syntax](#Syntax-Summary), Previous: [Tail calls](#Tail-Calls), Up: [Expressions and Evaluation](#About-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.3.3 Using the Guile REPL [¶](#Using-the-Guile-REPL)

If you start Guile without specifying a particular program for it to
execute, Guile enters its standard Read Evaluate Print Loop — or
*REPL* for short. In this mode, Guile repeatedly reads in the next
Scheme expression that the user types, evaluates it, and prints the
resulting value.

The REPL is a useful mechanism for exploring the evaluation behavior
described in the previous subsection. If you type `string-append`,
for example, the REPL replies `#<primitive-procedure
string-append>`, illustrating the relationship between the variable
`string-append` and the procedure value stored in that variable.

In this manual, the notation ⇒ is used to mean “evaluates
to”. Wherever you see an example of the form

```
expression
⇒
result
```

feel free to try it out yourself by typing expression into the
REPL and checking that it gives the expected result.

---

Previous: [Using the Guile REPL](#The-REPL), Up: [Expressions and Evaluation](#About-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.3.4 Summary of Common Syntax [¶](#Summary-of-Common-Syntax)

This subsection lists the most commonly used Scheme syntactic
expressions, simply so that you will recognize common special syntax
when you see it. For a full description of each of these syntaxes,
follow the appropriate reference.

`lambda` (see [Lambda: Basic Procedure Creation](#Lambda)) is used to construct procedure objects.

`define` (see [Top Level Variable Definitions](#Top-Level)) is used to create a new variable and
set its initial value.

`set!` (see [Top Level Variable Definitions](#Top-Level)) is used to modify an existing variable’s
value.

`let`, `let*` and `letrec` (see [Local Variable Bindings](#Local-Bindings))
create an inner lexical environment for the evaluation of a sequence of
expressions, in which a specified set of local variables is bound to the
values of a corresponding set of expressions. For an introduction to
environments, see See [The Concept of Closure](#About-Closure).

`begin` (see [Sequencing and Splicing](#begin)) executes a sequence of expressions in order
and returns the value of the last expression. Note that this is not the
same as a procedure which returns its last argument, because the
evaluation of a procedure invocation expression does not guarantee to
evaluate the arguments in order.

`if` and `cond` (see [Simple Conditional Evaluation](#Conditionals)) provide conditional
evaluation of argument expressions depending on whether one or more
conditions evaluate to “true” or “false”.

`case` (see [Simple Conditional Evaluation](#Conditionals)) provides conditional evaluation of
argument expressions depending on whether a variable has one of a
specified group of values.

`and` (see [Conditional Evaluation of a Sequence of Expressions](#and-or)) executes a sequence of expressions in order
until either there are no expressions left, or one of them evaluates to
“false”.

`or` (see [Conditional Evaluation of a Sequence of Expressions](#and-or)) executes a sequence of expressions in order
until either there are no expressions left, or one of them evaluates to
“true”.

---

Next: [Further Reading](#Further-Reading), Previous: [Expressions and Evaluation](#About-Expressions), Up: [Hello Scheme!](#Hello-Scheme_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 3.4 The Concept of Closure [¶](#The-Concept-of-Closure)

The concept of *closure* is the idea that a lambda expression
“captures” the variable bindings that are in lexical scope at the
point where the lambda expression occurs. The procedure created by the
lambda expression can refer to and mutate the captured bindings, and the
values of those bindings persist between procedure calls.

This section explains and explores the various parts of this idea in
more detail.

* [Names, Locations, Values and Environments](#About-Environments)
* [Local Variables and Environments](#Local-Variables)
* [Environment Chaining](#Chaining)
* [Lexical Scope](#Lexical-Scope)
* [Closure](#Closure)
* [Example 1: A Serial Number Generator](#Serial-Number)
* [Example 2: A Shared Persistent Variable](#Shared-Variable)
* [Example 3: The Callback Closure Problem](#Callback-Closure)
* [Example 4: Object Orientation](#OO-Closure)

---

Next: [Local Variables and Environments](#Local-Variables), Up: [The Concept of Closure](#About-Closure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.1 Names, Locations, Values and Environments [¶](#Names_002c-Locations_002c-Values-and-Environments)

We said earlier that a variable name in a Scheme program is associated
with a location in which any kind of Scheme value may be stored.
(Incidentally, the term “vcell” is often used in Lisp and Scheme
circles as an alternative to “location”.) Thus part of what we mean
when we talk about “creating a variable” is in fact establishing an
association between a name, or identifier, that is used by the Scheme
program code, and the variable location to which that name refers.
Although the value that is stored in that location may change, the
location to which a given name refers is always the same.

We can illustrate this by breaking down the operation of the
`define` syntax into three parts: `define`

* creates a new location
* establishes an association between that location and the name specified
  as the first argument of the `define` expression
* stores in that location the value obtained by evaluating the second
  argument of the `define` expression.

A collection of associations between names and locations is called an
*environment*. When you create a top level variable in a program
using `define`, the name-location association for that variable is
added to the “top level” environment. The “top level” environment
also includes name-location associations for all the procedures that are
supplied by standard Scheme.

It is also possible to create environments other than the top level one,
and to create variable bindings, or name-location associations, in those
environments. This ability is a key ingredient in the concept of
closure; the next subsection shows how it is done.

---

Next: [Environment Chaining](#Chaining), Previous: [Names, Locations, Values and Environments](#About-Environments), Up: [The Concept of Closure](#About-Closure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.2 Local Variables and Environments [¶](#Local-Variables-and-Environments)

We have seen how to create top level variables using the `define`
syntax (see [Defining and Setting Variables](#Definition)). It is often useful to create variables
that are more limited in their scope, typically as part of a procedure
body. In Scheme, this is done using the `let` syntax, or one of
its modified forms `let*` and `letrec`. These syntaxes are
described in full later in the manual (see [Local Variable Bindings](#Local-Bindings)). Here
our purpose is to illustrate their use just enough that we can see how
local variables work.

For example, the following code uses a local variable `s` to
simplify the computation of the area of a triangle given the lengths of
its three sides.

```
(define a 5.3)
(define b 4.7)
(define c 2.8)

(define area
  (let ((s (/ (+ a b c) 2)))
    (sqrt (* s (- s a) (- s b) (- s c)))))
```

The effect of the `let` expression is to create a new environment
and, within this environment, an association between the name `s`
and a new location whose initial value is obtained by evaluating
`(/ (+ a b c) 2)`. The expressions in the body of the `let`,
namely `(sqrt (* s (- s a) (- s b) (- s c)))`, are then evaluated
in the context of the new environment, and the value of the last
expression evaluated becomes the value of the whole `let`
expression, and therefore the value of the variable `area`.

---

Next: [Lexical Scope](#Lexical-Scope), Previous: [Local Variables and Environments](#Local-Variables), Up: [The Concept of Closure](#About-Closure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.3 Environment Chaining [¶](#Environment-Chaining)

In the example of the previous subsection, we glossed over an important
point. The body of the `let` expression in that example refers not
only to the local variable `s`, but also to the top level variables
`a`, `b`, `c` and `sqrt`. (`sqrt` is the
standard Scheme procedure for calculating a square root.) If the body
of the `let` expression is evaluated in the context of the
*local* `let` environment, how does the evaluation get at the
values of these top level variables?

The answer is that the local environment created by a `let`
expression automatically has a reference to its containing environment
— in this case the top level environment — and that the Scheme
interpreter automatically looks for a variable binding in the containing
environment if it doesn’t find one in the local environment. More
generally, every environment except for the top level one has a
reference to its containing environment, and the interpreter keeps
searching back up the chain of environments — from most local to top
level — until it either finds a variable binding for the required
identifier or exhausts the chain.

This description also determines what happens when there is more than
one variable binding with the same name. Suppose, continuing the
example of the previous subsection, that there was also a pre-existing
top level variable `s` created by the expression:

```
(define s "Some beans, my lord!")
```

Then both the top level environment and the local `let` environment
would contain bindings for the name `s`. When evaluating code
within the `let` body, the interpreter looks first in the local
`let` environment, and so finds the binding for `s` created by
the `let` syntax. Even though this environment has a reference to
the top level environment, which also has a binding for `s`, the
interpreter doesn’t get as far as looking there. When evaluating code
outside the `let` body, the interpreter looks up variable names in
the top level environment, so the name `s` refers to the top level
variable.

Within the `let` body, the binding for `s` in the local
environment is said to *shadow* the binding for `s` in the top
level environment.

---

Next: [Closure](#Closure), Previous: [Environment Chaining](#Chaining), Up: [The Concept of Closure](#About-Closure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.4 Lexical Scope [¶](#Lexical-Scope-1)

The rules that we have just been describing are the details of how
Scheme implements “lexical scoping”. This subsection takes a brief
diversion to explain what lexical scope means in general and to present
an example of non-lexical scoping.

“Lexical scope” in general is the idea that

* an identifier at a particular place in a program always refers to the
  same variable location — where “always” means “every time that the
  containing expression is executed”, and that
* the variable location to which it refers can be determined by static
  examination of the source code context in which that identifier appears,
  without having to consider the flow of execution through the program as
  a whole.

In practice, lexical scoping is the norm for most programming languages,
and probably corresponds to what you would intuitively consider to be
“normal”. You may even be wondering how the situation could possibly
— and usefully — be otherwise. To demonstrate that another kind of
scoping is possible, therefore, and to compare it against lexical
scoping, the following subsection presents an example of non-lexical
scoping and examines in detail how its behavior differs from the
corresponding lexically scoped code.

* [An Example of Non-Lexical Scoping](#Scoping-Example)

---

Up: [Lexical Scope](#Lexical-Scope)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.4.1 An Example of Non-Lexical Scoping [¶](#An-Example-of-Non_002dLexical-Scoping)

To demonstrate that non-lexical scoping does exist and can be useful, we
present the following example from Emacs Lisp, which is a “dynamically
scoped” language.

```
(defvar currency-abbreviation "USD")

(defun currency-string (units hundredths)
  (concat currency-abbreviation
          (number-to-string units)
          "."
          (number-to-string hundredths)))

(defun french-currency-string (units hundredths)
  (let ((currency-abbreviation "FRF"))
    (currency-string units hundredths)))
```

The question to focus on here is: what does the identifier
`currency-abbreviation` refer to in the `currency-string`
function? The answer, in Emacs Lisp, is that all variable bindings go
onto a single stack, and that `currency-abbreviation` refers to the
topmost binding from that stack which has the name
“currency-abbreviation”. The binding that is created by the
`defvar` form, to the value `"USD"`, is only relevant if none
of the code that calls `currency-string` rebinds the name
“currency-abbreviation” in the meanwhile.

The second function `french-currency-string` works precisely by
taking advantage of this behavior. It creates a new binding for the
name “currency-abbreviation” which overrides the one established by
the `defvar` form.

```
;; Note!  This is Emacs Lisp evaluation, not Scheme!
(french-currency-string 33 44)
⇒
"FRF33.44"
```

Now let’s look at the corresponding, *lexically scoped* Scheme
code:

```
(define currency-abbreviation "USD")

(define (currency-string units hundredths)
  (string-append currency-abbreviation
                 (number->string units)
                 "."
                 (number->string hundredths)))

(define (french-currency-string units hundredths)
  (let ((currency-abbreviation "FRF"))
    (currency-string units hundredths)))
```

According to the rules of lexical scoping, the
`currency-abbreviation` in `currency-string` refers to the
variable location in the innermost environment at that point in the code
which has a binding for `currency-abbreviation`, which is the
variable location in the top level environment created by the preceding
`(define currency-abbreviation …)` expression.

In Scheme, therefore, the `french-currency-string` procedure does
not work as intended. The variable binding that it creates for
“currency-abbreviation” is purely local to the code that forms the
body of the `let` expression. Since this code doesn’t directly use
the name “currency-abbreviation” at all, the binding is pointless.

```
(french-currency-string 33 44)
⇒
"USD33.44"
```

This begs the question of how the Emacs Lisp behavior can be
implemented in Scheme. In general, this is a design question whose
answer depends upon the problem that is being addressed. In this case,
the best answer may be that `currency-string` should be
redesigned so that it can take an optional third argument. This third
argument, if supplied, is interpreted as a currency abbreviation that
overrides the default.

It is possible to change `french-currency-string` so that it mostly
works without changing `currency-string`, but the fix is inelegant,
and susceptible to interrupts that could leave the
`currency-abbreviation` variable in the wrong state:

```
(define (french-currency-string units hundredths)
  (set! currency-abbreviation "FRF")
  (let ((result (currency-string units hundredths)))
    (set! currency-abbreviation "USD")
    result))
```

The key point here is that the code does not create any local binding
for the identifier `currency-abbreviation`, so all occurrences of
this identifier refer to the top level variable.

---

Next: [Example 1: A Serial Number Generator](#Serial-Number), Previous: [Lexical Scope](#Lexical-Scope), Up: [The Concept of Closure](#About-Closure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.5 Closure [¶](#Closure-1)

Consider a `let` expression that doesn’t contain any
`lambda`s:

```
(let ((s (/ (+ a b c) 2)))
  (sqrt (* s (- s a) (- s b) (- s c))))
```

When the Scheme interpreter evaluates this, it

* creates a new environment with a reference to the environment that was
  current when it encountered the `let`
* creates a variable binding for `s` in the new environment, with
  value given by `(/ (+ a b c) 2)`
* evaluates the expression in the body of the `let` in the context of
  the new local environment, and remembers the value `V`
* forgets the local environment
* continues evaluating the expression that contained the `let`, using
  the value `V` as the value of the `let` expression, in the
  context of the containing environment.

After the `let` expression has been evaluated, the local
environment that was created is simply forgotten, and there is no longer
any way to access the binding that was created in this environment. If
the same code is evaluated again, it will follow the same steps again,
creating a second new local environment that has no connection with the
first, and then forgetting this one as well.

If the `let` body contains a `lambda` expression, however, the
local environment is *not* forgotten. Instead, it becomes
associated with the procedure that is created by the `lambda`
expression, and is reinstated every time that that procedure is called.
In detail, this works as follows.

* When the Scheme interpreter evaluates a `lambda` expression, to
  create a procedure object, it stores the current environment as part of
  the procedure definition.
* Then, whenever that procedure is called, the interpreter reinstates the
  environment that is stored in the procedure definition and evaluates the
  procedure body within the context of that environment.

The result is that the procedure body is always evaluated in the context
of the environment that was current when the procedure was created.

This is what is meant by *closure*. The next few subsections
present examples that explore the usefulness of this concept.

---

Next: [Example 2: A Shared Persistent Variable](#Shared-Variable), Previous: [Closure](#Closure), Up: [The Concept of Closure](#About-Closure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.6 Example 1: A Serial Number Generator [¶](#Example-1_003a-A-Serial-Number-Generator)

This example uses closure to create a procedure with a variable binding
that is private to the procedure, like a local variable, but whose value
persists between procedure calls.

```
(define (make-serial-number-generator)
  (let ((current-serial-number 0))
    (lambda ()
      (set! current-serial-number (+ current-serial-number 1))
      current-serial-number)))

(define entry-sn-generator (make-serial-number-generator))

(entry-sn-generator)
⇒
1

(entry-sn-generator)
⇒
2
```

When `make-serial-number-generator` is called, it creates a local
environment with a binding for `current-serial-number` whose
initial value is 0, then, within this environment, creates a procedure.
The local environment is stored within the created procedure object and
so persists for the lifetime of the created procedure.

Every time the created procedure is invoked, it increments the value of
the `current-serial-number` binding in the captured environment and
then returns the current value.

Note that `make-serial-number-generator` can be called again to
create a second serial number generator that is independent of the
first. Every new invocation of `make-serial-number-generator`
creates a new local `let` environment and returns a new procedure
object with an association to this environment.

---

Next: [Example 3: The Callback Closure Problem](#Callback-Closure), Previous: [Example 1: A Serial Number Generator](#Serial-Number), Up: [The Concept of Closure](#About-Closure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.7 Example 2: A Shared Persistent Variable [¶](#Example-2_003a-A-Shared-Persistent-Variable)

This example uses closure to create two procedures, `get-balance`
and `deposit`, that both refer to the same captured local
environment so that they can both access the `balance` variable
binding inside that environment. The value of this variable binding
persists between calls to either procedure.

Note that the captured `balance` variable binding is private to
these two procedures: it is not directly accessible to any other code.
It can only be accessed indirectly via `get-balance` or
`deposit`, as illustrated by the `withdraw` procedure.

```
(define get-balance #f)
(define deposit #f)

(let ((balance 0))
  (set! get-balance
        (lambda ()
          balance))
  (set! deposit
        (lambda (amount)
          (set! balance (+ balance amount))
          balance)))

(define (withdraw amount)
  (deposit (- amount)))

(get-balance)
⇒
0

(deposit 50)
⇒
50

(withdraw 75)
⇒
-25
```

An important detail here is that the `get-balance` and
`deposit` variables must be set up by `define`ing them at top
level and then `set!`ing their values inside the `let` body.
Using `define` within the `let` body would not work: this
would create variable bindings within the local `let` environment
that would not be accessible at top level.

---

Next: [Example 4: Object Orientation](#OO-Closure), Previous: [Example 2: A Shared Persistent Variable](#Shared-Variable), Up: [The Concept of Closure](#About-Closure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.8 Example 3: The Callback Closure Problem [¶](#Example-3_003a-The-Callback-Closure-Problem)

A frequently used programming model for library code is to allow an
application to register a callback function for the library to call when
some particular event occurs. It is often useful for the application to
make several such registrations using the same callback function, for
example if several similar library events can be handled using the same
application code, but the need then arises to distinguish the callback
function calls that are associated with one callback registration from
those that are associated with different callback registrations.

In languages without the ability to create functions dynamically, this
problem is usually solved by passing a `user_data` parameter on the
registration call, and including the value of this parameter as one of
the parameters on the callback function. Here is an example of
declarations using this solution in C:

```
typedef void (event_handler_t) (int event_type,
                                void *user_data);

void register_callback (int event_type,
                        event_handler_t *handler,
                        void *user_data);
```

In Scheme, closure can be used to achieve the same functionality without
requiring the library code to store a `user-data` for each callback
registration.

```
;; In the library:

(define (register-callback event-type handler-proc)
  ...)

;; In the application:

(define (make-handler event-type user-data)
  (lambda ()
    ...
    <code referencing event-type and user-data>
    ...))

(register-callback event-type
                   (make-handler event-type ...))
```

As far as the library is concerned, `handler-proc` is a procedure
with no arguments, and all the library has to do is call it when the
appropriate event occurs. From the application’s point of view, though,
the handler procedure has used closure to capture an environment that
includes all the context that the handler code needs —
`event-type` and `user-data` — to handle the event
correctly.

---

Previous: [Example 3: The Callback Closure Problem](#Callback-Closure), Up: [The Concept of Closure](#About-Closure)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 3.4.9 Example 4: Object Orientation [¶](#Example-4_003a-Object-Orientation)

Closure is the capture of an environment, containing persistent variable
bindings, within the definition of a procedure or a set of related
procedures. This is rather similar to the idea in some object oriented
languages of encapsulating a set of related data variables inside an
“object”, together with a set of “methods” that operate on the
encapsulated data. The following example shows how closure can be used
to emulate the ideas of objects, methods and encapsulation in Scheme.

```
(define (make-account)
  (let ((balance 0))
    (define (get-balance)
      balance)
    (define (deposit amount)
      (set! balance (+ balance amount))
      balance)
    (define (withdraw amount)
      (deposit (- amount)))

    (lambda args
      (apply
        (case (car args)
          ((get-balance) get-balance)
          ((deposit) deposit)
          ((withdraw) withdraw)
          (else (error "Invalid method!")))
        (cdr args)))))
```

Each call to `make-account` creates and returns a new procedure,
created by the expression in the example code that begins “(lambda
args”.

```
(define my-account (make-account))

my-account
⇒
#<procedure args>
```

This procedure acts as an account object with methods
`get-balance`, `deposit` and `withdraw`. To apply one of
the methods to the account, you call the procedure with a symbol
indicating the required method as the first parameter, followed by any
other parameters that are required by that method.

```
(my-account 'get-balance)
⇒
0

(my-account 'withdraw 5)
⇒
-5

(my-account 'deposit 396)
⇒
391

(my-account 'get-balance)
⇒
391
```

Note how, in this example, both the current balance and the helper
procedures `get-balance`, `deposit` and `withdraw`, used
to implement the guts of the account object’s methods, are all stored in
variable bindings within the private local environment captured by the
`lambda` expression that creates the account object procedure.

---

Previous: [The Concept of Closure](#About-Closure), Up: [Hello Scheme!](#Hello-Scheme_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 3.5 Further Reading [¶](#Further-Reading-1)

* The website <http://www.schemers.org/> is a good starting point for
  all things Scheme.
* Dorai Sitaram’s online Scheme tutorial, *Teach Yourself Scheme in
  Fixnum Days*, at
  <http://www.ccs.neu.edu/home/dorai/t-y-scheme/t-y-scheme.html>.
  Includes a nice explanation of continuations.
* The complete text of *Structure and Interpretation of Computer
  Programs*, the classic introduction to computer science and Scheme by
  Hal Abelson, Jerry Sussman and Julie Sussman, is now available online at
  <http://mitpress.mit.edu/sicp/sicp.html>. This site also provides
  teaching materials related to the book, and all the source code used in
  the book, in a form suitable for loading and running.

---

Next: [Programming in C](#Programming-in-C), Previous: [Hello Scheme!](#Hello-Scheme_0021), Up: [The Guile Reference Manual](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]## 4 Programming in Scheme [¶](#Programming-in-Scheme-1)

Guile’s core language is Scheme, and a lot can be achieved simply by using Guile
to write and run Scheme programs — as opposed to having to dive into C code.
In this part of the manual, we explain how to use Guile in this mode, and
describe the tools that Guile provides to help you with script writing,
debugging, and packaging your programs for distribution.

For detailed reference information on the variables, functions, and so
on that make up Guile’s application programming interface (API), see
[API Reference](#API-Reference).

* [Guile’s Implementation of Scheme](#Guile-Scheme)
* [Invoking Guile](#Invoking-Guile)
* [Guile Scripting](#Guile-Scripting)
* [Using Guile Interactively](#Using-Guile-Interactively)
* [Using Guile in Emacs](#Using-Guile-in-Emacs)
* [Using Guile Tools](#Using-Guile-Tools)
* [Installing Site Packages](#Installing-Site-Packages)
* [Distributing Guile Code](#Distributing-Guile-Code)

---

Next: [Invoking Guile](#Invoking-Guile), Up: [Programming in Scheme](#Programming-in-Scheme)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 4.1 Guile’s Implementation of Scheme [¶](#Guile_0027s-Implementation-of-Scheme)

Guile’s core language is Scheme, which is specified and described in the
series of reports known as *RnRS*. *RnRS* is shorthand for the
*Revised^n Report on the Algorithmic Language Scheme*.
Guile complies fully with R5RS (see [Introduction](/r5rs/latest/en/r5rs.html#Top) in R5RS),
and is largely compliant with R6RS and R7RS.

Guile also has many extensions that go beyond these reports. Some of
the areas where Guile extends standard Scheme are:

* Guile’s interactive documentation system
* Guile’s support for POSIX-compliant network programming
* GOOPS – Guile’s framework for object oriented programming.

---

Next: [Guile Scripting](#Guile-Scripting), Previous: [Guile’s Implementation of Scheme](#Guile-Scheme), Up: [Programming in Scheme](#Programming-in-Scheme)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
