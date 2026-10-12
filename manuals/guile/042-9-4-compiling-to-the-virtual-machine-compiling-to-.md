### 9.4 Compiling to the Virtual Machine [¶](#Compiling-to-the-Virtual-Machine-1)

Compilers! The word itself inspires excitement and awe, even among
experienced practitioners. But a compiler is just a program: an
eminently hackable thing. This section aims to describe Guile’s
compiler in such a way that interested Scheme hackers can feel
comfortable reading and extending it.

See [Reading and Evaluating Scheme Code](#Read_002fLoad_002fEval_002fCompile), if you’re lost and you just wanted to
know how to compile your `.scm` file.

* [Compiler Tower](#Compiler-Tower)
* [The Scheme Compiler](#The-Scheme-Compiler)
* [Tree-IL](#Tree_002dIL)
* [Continuation-Passing Style](#Continuation_002dPassing-Style)
* [Bytecode](#Bytecode)
* [Writing New High-Level Languages](#Writing-New-High_002dLevel-Languages)
* [Extending the Compiler](#Extending-the-Compiler)

---

Next: [The Scheme Compiler](#The-Scheme-Compiler), Up: [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.1 Compiler Tower [¶](#Compiler-Tower-1)

Guile’s compiler is quite simple – its *compilers*, to put it more
accurately. Guile defines a tower of languages, starting at Scheme and
progressively simplifying down to languages that resemble the VM
instruction set (see [Instruction Set](#Instruction-Set)).

Each language knows how to compile to the next, so each step is simple
and understandable. Furthermore, this set of languages is not hardcoded
into Guile, so it is possible for the user to add new high-level
languages, new passes, or even different compilation targets.

Languages are registered in the module, `(system base language)`:

```
(use-modules (system base language))
```

They are registered with the `define-language` form.

Scheme Syntax: **define-language** [#:name] [#:title] [#:reader] [#:printer] [#:parser=#f] [#:compilers=’()] [#:decompilers=’()] [#:evaluator=#f] [#:joiner=#f] [#:for-humans?=#t] [#:make-default-environment=make-fresh-user-module] [#:lowerer=#f] [#:analyzer=#f] [#:compiler-chooser=#f] [¶](#index-define_002dlanguage)
:   Define a language.

    This syntax defines a `<language>` object, bound to name in
    the current environment. In addition, the language will be added to the
    global language set. For example, this is the language definition for
    Scheme:

    ```
    (define-language scheme
      #:title	"Scheme"
      #:reader      (lambda (port env) ...)
      #:compilers   `((tree-il . ,compile-tree-il))
      #:decompilers `((tree-il . ,decompile-tree-il))
      #:evaluator	(lambda (x module) (primitive-eval x))
      #:printer	write
      #:make-default-environment (lambda () ...))
    ```

The interesting thing about having languages defined this way is that
they present a uniform interface to the read-eval-print loop. This
allows the user to change the current language of the REPL:

```
scheme@(guile-user)> ,language tree-il
Happy hacking with Tree Intermediate Language!  To switch back, type `,L scheme'.
tree-il@(guile-user)> ,L scheme
Happy hacking with Scheme!  To switch back, type `,L tree-il'.
scheme@(guile-user)>
```

Languages can be looked up by name, as they were above.

Scheme Procedure: **lookup-language** name [¶](#index-lookup_002dlanguage)
:   Looks up a language named name, autoloading it if necessary.

    Languages are autoloaded by looking for a variable named name in
    a module named `(language name spec)`.

    The language object will be returned, or `#f` if there does not
    exist a language with that name.

When Guile goes to compile Scheme to bytecode, it will ask the Scheme
language to choose a compiler from Scheme to the next language on the
path from Scheme to bytecode. Performing this computation recursively
builds transformations from a flexible chain of compilers. The next
link will be obtained by invoking the language’s compiler chooser, or if
not present, from the language’s compilers field.

A language can specify an analyzer, which is run before a term of that
language is lowered and compiled. This is where compiler warnings are
issued.

If a language specifies a lowerer, that procedure is called on
expressions before compilation. This is where optimizations and
canonicalizations go.

Finally a language’s compiler translates a lowered term from one
language to the next one in the chain.

There is a notion of a “current language”, which is maintained in the
`current-language` parameter, defined in the core `(guile)`
module. This language is normally Scheme, and may be rebound by the
user. The run-time compilation interfaces
(see [Reading and Evaluating Scheme Code](#Read_002fLoad_002fEval_002fCompile)) also allow you to choose other source
and target languages.

The normal tower of languages when compiling Scheme goes like this:

* Scheme
* Tree Intermediate Language (Tree-IL)
* Continuation-Passing Style (CPS)
* Bytecode

As discussed before (see [Object File Format](#Object-File-Format)), bytecode is in ELF
format, ready to be serialized to disk. But when compiling Scheme at
run time, you want a Scheme value: for example, a compiled procedure.
For this reason, so as not to break the abstraction, Guile defines a
fake language at the bottom of the tower:

* Value

Compiling to `value` loads the bytecode into a procedure, turning
cold bytes into warm code.

Perhaps this strangeness can be explained by example:
`compile-file` defaults to compiling to bytecode, because it
produces object code that has to live in the barren world outside the
Guile runtime; but `compile` defaults to compiling to `value`,
as its product re-enters the Guile world.

Indeed, the process of compilation can circulate through these
different worlds indefinitely, as shown by the following quine:

```
((lambda (x) ((compile x) x)) '(lambda (x) ((compile x) x)))
```

---

Next: [Tree-IL](#Tree_002dIL), Previous: [Compiler Tower](#Compiler-Tower), Up: [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.2 The Scheme Compiler [¶](#The-Scheme-Compiler-1)

The job of the Scheme compiler is to expand all macros and all of Scheme
to its most primitive expressions. The definition of “primitive
expression” is given by the inventory of constructs provided by
Tree-IL, the target language of the Scheme compiler: procedure calls,
conditionals, lexical references, and so on. This is described more
fully in the next section.

The tricky and amusing thing about the Scheme-to-Tree-IL compiler is
that it is completely implemented by the macro expander. Since the
macro expander has to run over all of the source code already in order
to expand macros, it might as well do the analysis at the same time,
producing Tree-IL expressions directly.

Because this compiler is actually the macro expander, it is extensible.
Any macro which the user writes becomes part of the compiler.

The Scheme-to-Tree-IL expander may be invoked using the generic
`compile` procedure:

```
(compile '(+ 1 2) #:from 'scheme #:to 'tree-il)
⇒
#<tree-il (call (toplevel +) (const 1) (const 2))>
```

`(compile foo #:from 'scheme #:to 'tree-il)` is entirely
equivalent to calling the macro expander as `(macroexpand foo
'c '(compile load eval))`. See [Macro Expansion](#Macro-Expansion).
`compile-tree-il`, the procedure dispatched by `compile` to
`'tree-il`, is a small wrapper around `macroexpand`, to make
it conform to the general form of compiler procedures in Guile’s
language tower.

Compiler procedures take three arguments: an expression, an
environment, and a keyword list of options. They return three values:
the compiled expression, the corresponding environment for the target
language, and a “continuation environment”. The compiled expression
and environment will serve as input to the next language’s compiler.
The “continuation environment” can be used to compile another
expression from the same source language within the same module.

For example, you might compile the expression, `(define-module
(foo))`. This will result in a Tree-IL expression and environment. But
if you compiled a second expression, you would want to take into account
the compile-time effect of compiling the previous expression, which puts
the user in the `(foo)` module. That is the purpose of the
“continuation environment”; you would pass it as the environment when
compiling the subsequent expression.

For Scheme, an environment is a module. By default, the `compile`
and `compile-file` procedures compile in a fresh module, such
that bindings and macros introduced by the expression being compiled
are isolated:

```
(eq? (current-module) (compile '(current-module)))
⇒ #f

(compile '(define hello 'world))
(defined? 'hello)
⇒ #f

(define / *)
(eq? (compile '/) /)
⇒ #f
```

Similarly, changes to the `current-reader` fluid (see [`current-reader`](#Loading)) are isolated:

```
(compile '(fluid-set! current-reader (lambda args 'fail)))
(fluid-ref current-reader)
⇒ #f
```

Nevertheless, having the compiler and *compilee* share the same name
space can be achieved by explicitly passing `(current-module)` as
the compilation environment:

```
(define hello 'world)
(compile 'hello #:env (current-module))
⇒ world
```

---

Next: [Continuation-Passing Style](#Continuation_002dPassing-Style), Previous: [The Scheme Compiler](#The-Scheme-Compiler), Up: [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.3 Tree-IL [¶](#Tree_002dIL-1)

Tree Intermediate Language (Tree-IL) is a structured intermediate
language that is close in expressive power to Scheme. It is an
expanded, pre-analyzed Scheme.

Tree-IL is “structured” in the sense that its representation is
based on records, not S-expressions. This gives a rigidity to the
language that ensures that compiling to a lower-level language only
requires a limited set of transformations. For example, the Tree-IL
type `<const>` is a record type with two fields, `src` and
`exp`. Instances of this type are created via `make-const`.
Fields of this type are accessed via the `const-src` and
`const-exp` procedures. There is also a predicate, `const?`.
See [Records](#Records), for more information on records.

All Tree-IL types have a `src` slot, which holds source location
information for the expression. This information, if present, will be
residualized into the compiled object code, allowing backtraces to
show source information. The format of `src` is the same as that
returned by Guile’s `source-properties` function. See [Source Properties](#Source-Properties), for more information.

Although Tree-IL objects are represented internally using records,
there is also an equivalent S-expression external representation for
each kind of Tree-IL. For example, the S-expression representation
of `#<const src: #f exp: 3>` expression would be:

```
(const 3)
```

Users may program with this format directly at the REPL:

```
scheme@(guile-user)> ,language tree-il
Happy hacking with Tree Intermediate Language!  To switch back, type `,L scheme'.
tree-il@(guile-user)> (call (primitive +) (const 32) (const 10))
⇒ 42
```

The `src` fields are left out of the external representation.

One may create Tree-IL objects from their external representations via
calling `parse-tree-il`, the reader for Tree-IL. If any source
information is attached to the input S-expression, it will be
propagated to the resulting Tree-IL expressions. This is probably the
easiest way to compile to Tree-IL: just make the appropriate external
representations in S-expression format, and let `parse-tree-il`
take care of the rest.

Scheme Variable: **<void>** src [¶](#index-_003cvoid_003e)

External Representation: **(void)** [¶](#index-_0028void_0029)
:   An empty expression. In practice, equivalent to Scheme’s `(if #f
    #f)`.

Scheme Variable: **<const>** src exp [¶](#index-_003cconst_003e)

External Representation: **(const** exp) [¶](#index-_0028const)
:   A constant.

Scheme Variable: **<primitive-ref>** src name [¶](#index-_003cprimitive_002dref_003e)

External Representation: **(primitive** name) [¶](#index-_0028primitive)
:   A reference to a “primitive”. A primitive is a procedure that, when
    compiled, may be open-coded. For example, `cons` is usually
    recognized as a primitive, so that it compiles down to a single
    instruction.

    Compilation of Tree-IL usually begins with a pass that resolves some
    `<module-ref>` and `<toplevel-ref>` expressions to
    `<primitive-ref>` expressions. The actual compilation pass has
    special cases for calls to certain primitives, like `apply` or
    `cons`.

Scheme Variable: **<lexical-ref>** src name gensym [¶](#index-_003clexical_002dref_003e)

External Representation: **(lexical** name gensym) [¶](#index-_0028lexical)
:   A reference to a lexically-bound variable. The name is the
    original name of the variable in the source program. gensym is a
    unique identifier for this variable.

Scheme Variable: **<lexical-set>** src name gensym exp [¶](#index-_003clexical_002dset_003e)

External Representation: **(set!** (lexical name gensym) exp) [¶](#index-_0028set_0021)
:   Sets a lexically-bound variable.

Scheme Variable: **<module-ref>** src mod name public? [¶](#index-_003cmodule_002dref_003e)

External Representation: **(@** mod name) [¶](#index-_0028_0040)

External Representation: **(@@** mod name) [¶](#index-_0028_0040_0040)
:   A reference to a variable in a specific module. mod should be
    the name of the module, e.g. `(guile-user)`.

    If public? is true, the variable named name will be looked
    up in mod’s public interface, and serialized with `@`;
    otherwise it will be looked up among the module’s private bindings,
    and is serialized with `@@`.

Scheme Variable: **<module-set>** src mod name public? exp [¶](#index-_003cmodule_002dset_003e)

External Representation: **(set!** (@ mod name) exp) [¶](#index-_0028set_0021-1)

External Representation: **(set!** (@@ mod name) exp) [¶](#index-_0028set_0021-2)
:   Sets a variable in a specific module.

Scheme Variable: **<toplevel-ref>** src name [¶](#index-_003ctoplevel_002dref_003e)

External Representation: **(toplevel** name) [¶](#index-_0028toplevel)
:   References a variable from the current procedure’s module.

Scheme Variable: **<toplevel-set>** src name exp [¶](#index-_003ctoplevel_002dset_003e)

External Representation: **(set!** (toplevel name) exp) [¶](#index-_0028set_0021-3)
:   Sets a variable in the current procedure’s module.

Scheme Variable: **<toplevel-define>** src name exp [¶](#index-_003ctoplevel_002ddefine_003e)

External Representation: **(define** name exp) [¶](#index-_0028define)
:   Defines a new top-level variable in the current procedure’s module.

Scheme Variable: **<conditional>** src test then else [¶](#index-_003cconditional_003e)

External Representation: **(if** test then else) [¶](#index-_0028if)
:   A conditional. Note that else is not optional.

Scheme Variable: **<call>** src proc args [¶](#index-_003ccall_003e)

External Representation: **(call** proc . args) [¶](#index-_0028call)
:   A procedure call.

Scheme Variable: **<primcall>** src name args [¶](#index-_003cprimcall_003e)

External Representation: **(primcall** name . args) [¶](#index-_0028primcall)
:   A call to a primitive. Equivalent to `(call (primitive name)
    . args)`. This construct is often more convenient to generate and
    analyze than `<call>`.

    As part of the compilation process, instances of `(call (primitive
    name) . args)` are transformed into primcalls.

Scheme Variable: **<seq>** src head tail [¶](#index-_003cseq_003e)

External Representation: **(seq** head tail) [¶](#index-_0028seq)
:   A sequence. The semantics is that head is evaluated first, and
    any resulting values are ignored. Then tail is evaluated, in tail
    position.

Scheme Variable: **<lambda>** src meta body [¶](#index-_003clambda_003e)

External Representation: **(lambda** meta body) [¶](#index-_0028lambda)
:   A closure. meta is an association list of properties for the
    procedure. body is a single Tree-IL expression of type
    `<lambda-case>`. As the `<lambda-case>` clause can chain to
    an alternate clause, this makes Tree-IL’s `<lambda>` have the
    expressiveness of Scheme’s `case-lambda`.

Scheme Variable: **<lambda-case>** req opt rest kw inits gensyms body alternate [¶](#index-_003clambda_002dcase_003e)

External Representation: **(lambda-case** ((req opt rest kw inits gensyms) body) [alternate]) [¶](#index-_0028lambda_002dcase)
:   One clause of a `case-lambda`. A `lambda` expression in
    Scheme is treated as a `case-lambda` with one clause.

    req is a list of the procedure’s required arguments, as symbols.
    opt is a list of the optional arguments, or `#f` if there
    are no optional arguments. rest is the name of the rest
    argument, or `#f`.

    kw is a list of the form, `(allow-other-keys?
    (keyword name var) ...)`, where keyword is the
    keyword corresponding to the argument named name, and whose
    corresponding gensym is var, or `#f` if there are no keyword
    arguments. inits are tree-il expressions corresponding to all of
    the optional and keyword arguments, evaluated to bind variables whose
    value is not supplied by the procedure caller. Each init
    expression is evaluated in the lexical context of previously bound
    variables, from left to right.

    gensyms is a list of gensyms corresponding to all arguments:
    first all of the required arguments, then the optional arguments if
    any, then the rest argument if any, then all of the keyword arguments.

    body is the body of the clause. If the procedure is called with
    an appropriate number of arguments, body is evaluated in tail
    position. Otherwise, if there is an alternate, it should be a
    `<lambda-case>` expression, representing the next clause to try.
    If there is no alternate, a wrong-number-of-arguments error is
    signaled.

Scheme Variable: **<let>** src names gensyms vals exp [¶](#index-_003clet_003e)

External Representation: **(let** names gensyms vals exp) [¶](#index-_0028let)
:   Lexical binding, like Scheme’s `let`. names are the original
    binding names, gensyms are gensyms corresponding to the
    names, and vals are Tree-IL expressions for the values.
    exp is a single Tree-IL expression.

Scheme Variable: **<letrec>** in-order? src names gensyms vals exp [¶](#index-_003cletrec_003e)

External Representation: **(letrec** names gensyms vals exp) [¶](#index-_0028letrec)

External Representation: **(letrec\*** names gensyms vals exp) [¶](#index-_0028letrec_002a)
:   A version of `<let>` that creates recursive bindings, like
    Scheme’s `letrec`, or `letrec*` if in-order? is true.

Scheme Variable: **<prompt>** escape-only? tag body handler [¶](#index-_003cprompt_003e)

External Representation: **(prompt** escape-only? tag body handler) [¶](#index-_0028prompt)
:   A dynamic prompt. Instates a prompt named tag, an expression,
    during the dynamic extent of the execution of body, also an
    expression. If an abort occurs to this prompt, control will be passed
    to handler, also an expression, which should be a procedure. The
    first argument to the handler procedure will be the captured
    continuation, followed by all of the values passed to the abort. If
    escape-only? is true, the handler should be a `<lambda>` with
    a single `<lambda-case>` body expression with no optional or
    keyword arguments, and no alternate, and whose first argument is
    unreferenced. See [Prompts](#Prompts), for more information.

Scheme Variable: **<abort>** tag args tail [¶](#index-_003cabort_003e)

External Representation: **(abort** tag args tail) [¶](#index-_0028abort)
:   An abort to the nearest prompt with the name tag, an expression.
    args should be a list of expressions to pass to the prompt’s
    handler, and tail should be an expression that will evaluate to
    a list of additional arguments. An abort will save the partial
    continuation, which may later be reinstated, resulting in the
    `<abort>` expression evaluating to some number of values.

There are two Tree-IL constructs that are not normally produced by
higher-level compilers, but instead are generated during the
source-to-source optimization and analysis passes that the Tree-IL
compiler does. Users should not generate these expressions directly,
unless they feel very clever, as the default analysis pass will generate
them as necessary.

Scheme Variable: **<let-values>** src names gensyms exp body [¶](#index-_003clet_002dvalues_003e)

External Representation: **(let-values** names gensyms exp body) [¶](#index-_0028let_002dvalues)
:   Like Scheme’s `receive` – binds the values returned by
    evaluating `exp` to the `lambda`-like bindings described by
    gensyms. That is to say, gensyms may be an improper list.

    `<let-values>` is an optimization of a `<call>` to the
    primitive, `call-with-values`.

Scheme Variable: **<fix>** src names gensyms vals body [¶](#index-_003cfix_003e)

External Representation: **(fix** names gensyms vals body) [¶](#index-_0028fix)
:   Like `<letrec>`, but only for vals that are unset
    `lambda` expressions.

    `fix` is an optimization of `letrec` (and `let`).

Tree-IL is a convenient compilation target from source languages. It
can be convenient as a medium for optimization, though CPS is usually
better. The strength of Tree-IL is that it does not fix order of
evaluation, so it makes some code motion a bit easier.

Optimization passes performed on Tree-IL currently include:

* Open-coding (turning toplevel-refs into primitive-refs,
  and calls to primitives to primcalls)
* Partial evaluation (comprising inlining, copy propagation, and
  constant folding)

---

Next: [Bytecode](#Bytecode), Previous: [Tree-IL](#Tree_002dIL), Up: [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.4 Continuation-Passing Style [¶](#Continuation_002dPassing-Style-1)

Continuation-passing style (CPS) is Guile’s principal intermediate
language, bridging the gap between languages for people and languages
for machines. CPS gives a name to every part of a program: every
control point, and every intermediate value. This makes it an excellent
medium for reasoning about programs, which is the principal job of a
compiler.

* [An Introduction to CPS](#An-Introduction-to-CPS)
* [CPS in Guile](#CPS-in-Guile)
* [Building CPS](#Building-CPS)
* [CPS Soup](#CPS-Soup)
* [Compiling CPS](#Compiling-CPS)

---

Next: [CPS in Guile](#CPS-in-Guile), Up: [Continuation-Passing Style](#Continuation_002dPassing-Style)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.4.1 An Introduction to CPS [¶](#An-Introduction-to-CPS-1)

Consider the following Scheme expression:

```
(begin
  (display "The sum of 32 and 10 is: ")
  (display 42)
  (newline))
```

Let us identify all of the sub-expressions in this expression,
annotating them with unique labels:

```
(begin
  (display "The sum of 32 and 10 is: ")
  |k1      k2
  k0
  (display 42)
  |k4      k5
  k3
  (newline))
  |k7
  k6
```

Each of these labels identifies a point in a program. One label may be
the continuation of another label. For example, the continuation of
`k7` is `k6`. This is because after evaluating the value of
`newline`, performed by the expression labelled `k7`, we
continue to apply it in `k6`.

Which expression has `k0` as its continuation? It is either the
expression labelled `k1` or the expression labelled `k2`.
Scheme does not have a fixed order of evaluation of arguments, though it
does guarantee that they are evaluated in some order. Unlike general
Scheme, continuation-passing style makes evaluation order explicit. In
Guile, this choice is made by the higher-level language compilers.

Let us assume a left-to-right evaluation order. In that case the
continuation of `k1` is `k2`, and the continuation of
`k2` is `k0`.

With this example established, we are ready to give an example of CPS in
Scheme:

```
(lambda (ktail)
  (let ((k1 (lambda ()
              (let ((k2 (lambda (proc)
                          (let ((k0 (lambda (arg0)
                                      (proc k4 arg0))))
                            (k0 "The sum of 32 and 10 is: ")))))
                (k2 display))))
        (k4 (lambda _
              (let ((k5 (lambda (proc)
                          (let ((k3 (lambda (arg0)
                                      (proc k7 arg0))))
                            (k3 42)))))
                (k5 display))))
        (k7 (lambda _
              (let ((k6 (lambda (proc)
                          (proc ktail))))
                (k6 newline)))))
    (k1))
```

Holy code explosion, Batman! What’s with all the lambdas? Indeed, CPS
is by nature much more verbose than “direct-style” intermediate
languages like Tree-IL. At the same time, CPS is simpler than full
Scheme, because it makes things more explicit.

In the original program, the expression labelled `k0` is in effect
context. Any values it returns are ignored. In Scheme, this fact is
implicit. In CPS, we can see it explicitly by noting that its
continuation, `k4`, takes any number of values and ignores them.
Compare this to `k2`, which takes a single value; in this way we
can say that `k1` is in a “value” context. Likewise `k6` is
in tail context with respect to the expression as a whole, because its
continuation is the tail continuation, `ktail`. CPS makes these
details manifest, and gives them names.

---

Next: [Building CPS](#Building-CPS), Previous: [An Introduction to CPS](#An-Introduction-to-CPS), Up: [Continuation-Passing Style](#Continuation_002dPassing-Style)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.4.2 CPS in Guile [¶](#CPS-in-Guile-1)

Guile’s CPS language is composed of *continuations*. A continuation
is a labelled program point. If you are used to traditional compilers,
think of a continuation as a trivial basic block. A program is a
“soup” of continuations, represented as a map from labels to
continuations.

Like basic blocks, each continuation belongs to only one function. Some
continuations are special, like the continuation corresponding to a
function’s entry point, or the continuation that represents the tail of
a function. Others contain a *term*. A term contains an
*expression*, which evaluates to zero or more values. The term also
records the continuation to which it will pass its values. Some terms,
like conditional branches, may continue to one of a number of
continuations.

Continuation labels are small integers. This makes it easy to sort them
and to group them into sets. Whenever a term refers to a continuation,
it does so by name, simply recording the label of the continuation.
Continuation labels are unique among the set of labels in a program.

Variables are also named by small integers. Variable names are unique
among the set of variables in a program.

For example, a simple continuation that receives two values and adds
them together can be matched like this, using the `match` form from
`(ice-9 match)`:

```
(match cont
  (($ $kargs (x-name y-name) (x-var y-var)
      ($ $continue k src ($ $primcall '+ #f (x-var y-var))))
   (format #t "Add ~a and ~a and pass the result to label ~a"
           x-var y-var k)))
```

Here we see the most common kind of continuation, `$kargs`, which
binds some number of values to variables and then evaluates a term.

CPS Continuation: **$kargs** names vars term [¶](#index-_0024kargs)
:   Bind the incoming values to the variables vars, with original
    names names, and then evaluate term.

The names of a `$kargs` are just for debugging, and will end
up residualized in the object file for use by the debugger.

The term in a `$kargs` is always a `$continue`, which
evaluates an expression and continues to a continuation.

CPS Term: **$continue** k src exp [¶](#index-_0024continue)
:   Evaluate the expression exp and pass the resulting values (if any)
    to the continuation labelled k. The source information associated
    with the expression may be found in src, which is either an alist
    as in `source-properties` or is `#f` if there is no associated
    source.

There are a number of expression kinds. Above you see an example of
`$primcall`.

CPS Expression: **$primcall** name param args [¶](#index-_0024primcall)
:   Perform the primitive operation identified by `name`, a well-known
    symbol, passing it the arguments args, and pass all resulting
    values to the continuation.

    param is a constant parameter whose interpretation is up to the
    primcall in question. Usually it’s `#f` but for a primcall that
    might need some compile-time constant information – such as
    `add/immediate`, which adds a constant number to a value – the
    parameter holds this information.

    The set of available primitives includes many primitives known to
    Tree-IL and then some more; see the source code for details. Note that
    some Tree-IL primcalls need to be converted to a sequence of lower-level
    CPS primcalls. Again, see `(language tree-il compile-cps)` for
    full details.

The variables that are used by `$primcall`, or indeed by any
expression, must be defined before the expression is evaluated. An
equivalent way of saying this is that predecessor `$kargs`
continuation(s) that bind the variables(s) used by the expression must
*dominate* the continuation that uses the expression: definitions
dominate uses. This condition is trivially satisfied in our example
above, but in general to determine the set of variables that are in
“scope” for a given term, you need to do a flow analysis to see what
continuations dominate a term. The variables that are in scope are
those variables defined by the continuations that dominate a term.

Here is an inventory of the kinds of expressions in Guile’s CPS
language, besides `$primcall` which has already been described.
Recall that all expressions are wrapped in a `$continue` term which
specifies their continuation.

CPS Expression: **$const** val [¶](#index-_0024const)
:   Continue with the constant value val.

CPS Expression: **$prim** name [¶](#index-_0024prim)
:   Continue with the procedure that implements the primitive operation
    named by name.

CPS Expression: **$call** proc args [¶](#index-_0024call)
:   Call proc with the arguments args, and pass all values to
    the continuation. proc and the elements of the args list
    should all be variable names. The continuation identified by the term’s
    k should be a `$kreceive` or a `$ktail` instance.

CPS Expression: **$values** args [¶](#index-_0024values)
:   Pass the values named by the list args to the continuation.

CPS Expression: **$prompt** escape? tag handler [¶](#index-_0024prompt)

There are two sub-languages of CPS, *higher-order CPS* and
*first-order CPS*. The difference is that in higher-order CPS,
there are `$fun` and `$rec` expressions that bind functions or
mutually-recursive functions in the implicit scope of their use sites.
Guile transforms higher-order CPS into first-order CPS by *closure
conversion*, which chooses representations for all closures and which
arranges to access free variables through the implicit closure parameter
that is passed to every function call.

CPS Expression: **$fun** body [¶](#index-_0024fun)
:   Continue with a procedure. body names the entry point of the
    function, which should be a `$kfun`. This expression kind is only
    valid in higher-order CPS, which is the CPS language before closure
    conversion.

CPS Expression: **$rec** names vars funs [¶](#index-_0024rec)
:   Continue with a set of mutually recursive procedures denoted by
    names, vars, and funs. names is a list of
    symbols, vars is a list of variable names (unique integers), and
    funs is a list of `$fun` values. Note that the `$kargs`
    continuation should also define names/vars bindings.

The contification pass will attempt to transform the functions declared
in a `$rec` into local continuations. Any remaining `$fun`
instances are later removed by the closure conversion pass. If the
function has no free variables, it gets allocated as a constant.

CPS Expression: **$const-fun** label [¶](#index-_0024const_002dfun)
:   A constant which is a function whose entry point is label. As a
    constant, instances of `$const-fun` with the same label will
    not allocate; the space for the function is allocated as part of the
    compilation unit.

    In practice, `$const-fun` expressions are reified by CPS-conversion
    for functions whose call sites are not all visible within the
    compilation unit and which have no free variables. This expression kind
    is part of first-order CPS.

Otherwise, if the closure has free variables, it will be allocated at
its definition site via an `allocate-words` primcall and its free
variables initialized there. The code pointer in the closure is
initialized from a `$code` expression.

CPS Expression: **$code** label [¶](#index-_0024code)
:   Continue with the value of label, which should denote some
    `$kfun` continuation in the program. Used when initializing the
    code pointer of closure objects.

However, If the closure can be proven to never escape its scope then
other lighter-weight representations can be chosen. Additionally, if
all call sites are known, closure conversion will hard-wire the calls by
lowering `$call` to `$callk`.

CPS Expression: **$callk** label proc args [¶](#index-_0024callk)
:   Like `$call`, but for the case where the call target is known to be
    in the same compilation unit. label should denote some
    `$kfun` continuation in the program. In this case the proc
    is simply an additional argument, since it is not used to determine the
    call target at run-time.

To summarize: a `$continue` is a CPS term that continues to a
single label. But there are other kinds of CPS terms that can continue
to a different number of labels: `$branch`, `$switch`,
`$throw`, and `$prompt`.

CPS Term: **$branch** kf kt src op param args [¶](#index-_0024branch)
:   Evaluate the branching primcall op, with arguments args and
    constant parameter param, and continue to kt with zero
    values if the test is true. Otherwise continue to kf.

    The `$branch` term is like a `$continue` term with a
    `$primcall` expression, except that instead of binding a value and
    continuing to a single label, the result of the test is not bound but
    instead used to choose the continuation label.

    The set of operations (corresponding to op values) that are valid
    in a $branch is limited. In the general case, bind the result of
    a test expression to a variable, and then make a `$branch` on a
    `true?` op referencing that variable. The optimizer should inline
    the branch if possible.

CPS Term: **$switch** kf kt\* src arg [¶](#index-_0024switch)
:   Continue to a label in the list k\* according to the index argument
    arg, or to the default continuation kf if arg is
    greater than or equal to the length k\*. The index variable
    arg is an unboxed, unsigned 64-bit value.

    The `$switch` term is like C’s `switch` statement. The
    compiler to CPS can generate a `$switch` term directly, if the
    source language has such a concept, or it can rely on the CPS optimizer
    to turn appropriate chains of `$branch` statements to
    `$switch` instances, which is what the Scheme compiler does.

CPS Term: **$throw** src op param args [¶](#index-_0024throw)
:   Throw a non-resumable exception. Throw terms do not continue at all.
    The usual value of op is `throw`, with two arguments
    key and args. There are also some specific primcalls that
    compile to the VM `throw/value` and `throw/value+data`
    instructions; see the code for full details.

    The advantage of having `$throw` as a term is that, because it does
    not continue, this allows the optimizer to gather more information from
    type predicates. For example, if the predicate is `char?` and the
    kf continues to a throw, the set of labels dominated by kt
    is larger than if the throw notationally continued to some label that
    would never be reached by the throw.

CPS Term: **$prompt** k kh src escape? tag [¶](#index-_0024prompt-1)
:   Push a prompt on the stack identified by the variable name tag,
    which may be escape-only if escape? is true, and continue to
    kh with zero values. If the body aborts to this prompt, control
    will proceed at the continuation labelled kh, which should be a
    `$kreceive` continuation. Prompts are later popped by
    `pop-prompt` primcalls.

At this point we have described terms, expressions, and the most common
kind of continuation, `$kargs`. `$kargs` is used when the
predecessors of the continuation can be instructed to pass the values
where the continuation wants them. For example, if a `$kargs`
continuation k binds a variable v, and the compiler decides
to allocate v to slot 6, all predecessors of k should put
the value for v in slot 6 before jumping to k. One
situation in which this isn’t possible is receiving values from function
calls. Guile has a calling convention for functions which currently
places return values on the stack. A continuation of a call must check
that the number of values returned from a function matches the expected
number of values, and then must shuffle or collect those values to named
variables. `$kreceive` denotes this kind of continuation.

CPS Continuation: **$kreceive** arity k [¶](#index-_0024kreceive)
:   Receive values on the stack. Parse them according to arity, and
    then proceed with the parsed values to the `$kargs` continuation
    labelled k. As a limitation specific to `$kreceive`,
    arity may only contain required and rest arguments.

`$arity` is a helper data structure used by `$kreceive` and
also by `$kclause`, described below.

CPS Data: **$arity** req opt rest kw allow-other-keys? [¶](#index-_0024arity)
:   A data type declaring an arity. req and opt are lists of
    source names of required and optional arguments, respectively.
    rest is either the source name of the rest variable, or `#f`
    if this arity does not accept additional values. kw is a list of
    the form `((keyword name var) ...)`, describing
    the keyword arguments. allow-other-keys? is true if other keyword
    arguments are allowed and false otherwise.

    Note that all of these names with the exception of the vars in the
    kw list are source names, not unique variable names.

Additionally, there are three specific kinds of continuations that are
only used in function entries.

CPS Continuation: **$kfun** src meta self tail clause [¶](#index-_0024kfun)
:   Declare a function entry. src is the source information for the
    procedure declaration, and meta is the metadata alist as described
    above in Tree-IL’s `<lambda>`. self is a variable bound to
    the procedure being called, and which may be used for self-references.
    tail is the label of the `$ktail` for this function,
    corresponding to the function’s tail continuation. clause is the
    label of the first `$kclause` for the first `case-lambda`
    clause in the function, or otherwise `#f`.

CPS Continuation: **$ktail** [¶](#index-_0024ktail)
:   A tail continuation.

CPS Continuation: **$kclause** arity cont alternate [¶](#index-_0024kclause)
:   A clause of a function with a given arity. Applications of a function
    with a compatible set of actual arguments will continue to the
    continuation labelled cont, a `$kargs` instance representing
    the clause body. If the arguments are incompatible, control proceeds to
    alternate, which is a `$kclause` for the next clause, or
    `#f` if there is no next clause.

---

Next: [CPS Soup](#CPS-Soup), Previous: [CPS in Guile](#CPS-in-Guile), Up: [Continuation-Passing Style](#Continuation_002dPassing-Style)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.4.3 Building CPS [¶](#Building-CPS-1)

Unlike Tree-IL, the CPS language is built to be constructed and
deconstructed with abstract macros instead of via procedural
constructors or accessors, or instead of S-expression matching.

Deconstruction and matching is handled adequately by the `match`
form from `(ice-9 match)`. See [Pattern Matching](#Pattern-Matching). Construction
is handled by a set of mutually builder macros:
`build-term`, `build-cont`, and `build-exp`.

In the following interface definitions, consider `term` and
`exp` to be built by `build-term` or `build-exp`,
respectively. Consider any other name to be evaluated as a Scheme
expression. Many of these forms recognize `unquote` in some
contexts, to splice in a previously-built value; see the specifications
below for full details.

Scheme Syntax: **build-term** ,val [¶](#index-build_002dterm)

Scheme Syntax: **build-term** ($continue k src exp) [¶](#index-build_002dterm-1)

Scheme Syntax: **build-exp** ,val [¶](#index-build_002dexp)

Scheme Syntax: **build-exp** ($const val) [¶](#index-build_002dexp-1)

Scheme Syntax: **build-exp** ($prim name) [¶](#index-build_002dexp-2)

Scheme Syntax: **build-exp** ($fun kentry) [¶](#index-build_002dexp-3)

Scheme Syntax: **build-exp** ($const-fun kentry) [¶](#index-build_002dexp-4)

Scheme Syntax: **build-exp** ($code kentry) [¶](#index-build_002dexp-5)

Scheme Syntax: **build-exp** ($rec names syms funs) [¶](#index-build_002dexp-6)

Scheme Syntax: **build-exp** ($call proc (arg ...)) [¶](#index-build_002dexp-7)

Scheme Syntax: **build-exp** ($call proc args) [¶](#index-build_002dexp-8)

Scheme Syntax: **build-exp** ($callk k proc (arg ...)) [¶](#index-build_002dexp-9)

Scheme Syntax: **build-exp** ($callk k proc args) [¶](#index-build_002dexp-10)

Scheme Syntax: **build-exp** ($primcall name param (arg ...)) [¶](#index-build_002dexp-11)

Scheme Syntax: **build-exp** ($primcall name param args) [¶](#index-build_002dexp-12)

Scheme Syntax: **build-exp** ($values (arg ...)) [¶](#index-build_002dexp-13)

Scheme Syntax: **build-exp** ($values args) [¶](#index-build_002dexp-14)

Scheme Syntax: **build-exp** ($prompt escape? tag handler) [¶](#index-build_002dexp-15)

Scheme Syntax: **build-term** ($branch kf kt src op param (arg ...)) [¶](#index-build_002dterm-2)

Scheme Syntax: **build-term** ($branch kf kt src op param args) [¶](#index-build_002dterm-3)

Scheme Syntax: **build-term** ($switch kf kt\* src arg) [¶](#index-build_002dterm-4)

Scheme Syntax: **build-term** ($throw src op param (arg ...)) [¶](#index-build_002dterm-5)

Scheme Syntax: **build-term** ($throw src op param args) [¶](#index-build_002dterm-6)

Scheme Syntax: **build-term** ($prompt k kh src escape? tag) [¶](#index-build_002dterm-7)

Scheme Syntax: **build-cont** ,val [¶](#index-build_002dcont)

Scheme Syntax: **build-cont** ($kargs (name ...) (sym ...) term) [¶](#index-build_002dcont-1)

Scheme Syntax: **build-cont** ($kargs names syms term) [¶](#index-build_002dcont-2)

Scheme Syntax: **build-cont** ($kreceive req rest kargs) [¶](#index-build_002dcont-3)

Scheme Syntax: **build-cont** ($kfun src meta self ktail kclause) [¶](#index-build_002dcont-4)

Scheme Syntax: **build-cont** ($kclause ,arity kbody kalt) [¶](#index-build_002dcont-5)

Scheme Syntax: **build-cont** ($kclause (req opt rest kw aok?) kbody) [¶](#index-build_002dcont-6)
:   Construct a CPS term, expression, or continuation.

There are a few more miscellaneous interfaces as well.

Scheme Procedure: **make-arity** req opt rest kw allow-other-keywords? [¶](#index-make_002darity)
:   A procedural constructor for `$arity` objects.

Scheme Syntax: **rewrite-term** val (pat term) ... [¶](#index-rewrite_002dterm)

Scheme Syntax: **rewrite-exp** val (pat exp) ... [¶](#index-rewrite_002dexp)

Scheme Syntax: **rewrite-cont** val (pat cont) ... [¶](#index-rewrite_002dcont)
:   Match val against the series of patterns pat..., using
    `match`. The body of the matching clause should be a template in
    the syntax of `build-term`, `build-exp`, or `build-cont`,
    respectively.

---

Next: [Compiling CPS](#Compiling-CPS), Previous: [Building CPS](#Building-CPS), Up: [Continuation-Passing Style](#Continuation_002dPassing-Style)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.4.4 CPS Soup [¶](#CPS-Soup-1)

We describe programs in Guile’s CPS language as being a kind of “soup”
because all continuations in the program are mixed into the same
“pot”, so to speak, without explicit markers as to what function or
scope a continuation is in. A program in CPS is a map from continuation
labels to continuation values. As discussed in the introduction, a
continuation label is an integer. No label may be negative.

As a matter of convention, label 0 should map to the `$kfun`
continuation of the entry to the program, which should be a function of
no arguments. The body of a function consists of the labelled
continuations that are reachable from the function entry. A program can
refer to other functions, either via `$fun` and `$rec` in
higher-order CPS, or via `$const-fun`, `$callk`, and allocated
closures in first-order CPS. The program logically contains all
continuations of all functions reachable from the entry function. A
compiler pass may leave unreachable continuations in a program;
subsequent compiler passes should ensure that their transformations and
analyses only take reachable continuations into account. It’s OK though
if transformation runs over all continuations if including the
unreachable continuations has no effect on the transformations on the
live continuations.

The “soup” itself is implemented as an *intmap*, a functional
array-mapped trie specialized for integer keys. Intmaps associate
integers with values of any kind. Currently intmaps are a private data
structure only used by the CPS phase of the compiler. To work with
intmaps, load the `(language cps intmap)` module:

```
(use-modules (language cps intmap))
```

Intmaps are functional data structures, so there is no constructor as
such: one can simply start with the empty intmap and add entries to it.

```
(intmap? empty-intmap) ⇒ #t
(define x (intmap-add empty-intmap 42 "hi"))
(intmap? x) ⇒ #t
(intmap-ref x 42) ⇒ "hi"
(intmap-ref x 43) ⇒ error: 43 not present
(intmap-ref x 43 (lambda (k) "yo!")) ⇒ "yo"
(intmap-add x 42 "hej") ⇒ error: 42 already present
```

`intmap-ref` and `intmap-add` are the core of the intmap
interface. There is also `intmap-replace`, which replaces the
value associated with a given key, requiring that the key was present
already, and `intmap-remove`, which removes a key from an intmap.

Intmaps have a tree-like structure that is well-suited to set operations
such as union and intersection, so there are also the binary
`intmap-union` and `intmap-intersect` procedures. If the
result is equivalent to either argument, that argument is returned
as-is; in that way, one can detect whether the set operation produced a
new result simply by checking with `eq?`. This makes intmaps
useful when computing fixed points.

If a key is present in both intmaps and the associated values are not
the same in the sense of `eq?`, the resulting value is determined
by a “meet” procedure, which is the optional last argument to
`intmap-union`, `intmap-intersect`, and also to
`intmap-add`, `intmap-replace`, and similar functions. The
meet procedure will be called with the two values and should return the
intersected or unioned value in some domain-specific way. If no meet
procedure is given, the default meet procedure will raise an error.

To traverse over the set of values in an intmap, there are the
`intmap-next` and `intmap-prev` procedures. For example, if
intmap x has one entry mapping 42 to some value, we would have:

```
(intmap-next x) ⇒ 42
(intmap-next x 0) ⇒ 42
(intmap-next x 42) ⇒ 42
(intmap-next x 43) ⇒ #f
(intmap-prev x) ⇒ 42
(intmap-prev x 42) ⇒ 42
(intmap-prev x 41) ⇒ #f
```

There is also the `intmap-fold` procedure, which folds over keys
and values in the intmap from lowest to highest value, and
`intmap-fold-right` which does so in the opposite direction. These
procedures may take up to 3 seed values. The number of values that the
fold procedure returns is the number of seed values.

```
(define q (intmap-add (intmap-add empty-intmap 1 2) 3 4))
(intmap-fold acons q '()) ⇒ ((3 . 4) (1 . 2))
(intmap-fold-right acons q '()) ⇒ ((1 . 2) (3 . 4))
```

When an entry in an intmap is updated (removed, added, or changed), a
new intmap is created that shares structure with the original intmap.
This operation ensures that the result of existing computations is not
affected by future computations: no mutation is ever visible to user
code. This is a great property in a compiler data structure, as it lets
us hold a copy of a program before a transformation and use it while we
build a post-transformation program. Updating an intmap is O(log
n) in the size of the intmap.

However, the O(log n) allocation costs are sometimes too much,
especially in cases when we know that we can just update the intmap in
place. As an example, say we have an intmap mapping the integers 1 to
100 to the integers 42 to 141. Let’s say that we want to transform this
map by adding 1 to each value. There is already an efficient
`intmap-map` procedure in the `(language cps utils)` module,
but if we didn’t know about that we might do:

```
(define (intmap-increment map)
  (let lp ((k 0) (map map))
    (let ((k (intmap-next map k)))
      (if k
          (let ((v (intmap-ref map k)))
            (lp (1+ k) (intmap-replace map k (1+ v))))
          map))))
```

Observe that the intermediate values created by `intmap-replace`
are completely invisible to the program – only the last result of
`intmap-replace` value is needed. The rest might as well share
state with the last one, and we could update in place. Guile allows
this kind of interface via *transient intmaps*, inspired by
Clojure’s transient interface (<http://clojure.org/transients>).

The in-place `intmap-add!` and `intmap-replace!` procedures
return transient intmaps. If one of these in-place procedures is called
on a normal persistent intmap, a new transient intmap is created. This
is an O(1) operation. In all other respects the interface is like their
persistent counterparts, `intmap-add` and `intmap-replace`.
If an in-place procedure is called on a transient intmap, the intmap is
mutated in-place and the same value is returned.

If a persistent operation like `intmap-add` is called on a
transient intmap, the transient’s mutable substructure is then marked as
persistent, and `intmap-add` then runs on a new persistent intmap
sharing structure but not state with the original transient. Mutating a
transient will cause enough copying to ensure that it can make its
change, but if part of its substructure is already “owned” by it, no
more copying is needed.

We can use transients to make `intmap-increment` more efficient.
The two changed elements have been marked **like this**.

```
(define (intmap-increment map)
  (let lp ((k 0) (map map))
    (let ((k (intmap-next map k)))
      (if k
          (let ((v (intmap-ref map k)))
            (lp (1+ k) (intmap-replace! map k (1+ v))))
          (persistent-intmap map)))))
```

Be sure to tag the result as persistent using the
`persistent-intmap` procedure to prevent the mutability from
leaking to other parts of the program. For added paranoia, you could
call `persistent-intmap` on the incoming map, to ensure that if it
were already transient, that the mutations in the body of
`intmap-increment` wouldn’t affect the incoming value.

In summary, programs in CPS are intmaps whose values are continuations.
See the source code of `(language cps utils)` for a number of
useful facilities for working with CPS values.

---

Previous: [CPS Soup](#CPS-Soup), Up: [Continuation-Passing Style](#Continuation_002dPassing-Style)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.4.5 Compiling CPS [¶](#Compiling-CPS-1)

Compiling CPS in Guile has three phases: conversion, optimization, and
code generation.

CPS conversion is the process of taking a higher-level language and
compiling it to CPS. Source languages can do this directly, or they can
convert to Tree-IL (which is probably easier) and let Tree-IL convert to
CPS later. Going through Tree-IL has the advantage of running Tree-IL
optimization passes, like partial evaluation. Also, the compiler from
Tree-IL to CPS handles assignment conversion, in which assigned local
variables (in Tree-IL, locals that are `<lexical-set>`) are
converted to being boxed values on the heap. See [Variables and the VM](#Variables-and-the-VM).

After CPS conversion, Guile runs some optimization passes over the CPS.
Most optimization in Guile is done on the CPS language. The one major
exception is partial evaluation, which for historic reasons is done on
Tree-IL.

The major optimization performed on CPS is contification, in which
functions that are always called with the same continuation are
incorporated directly into a function’s body. This opens up space for
more optimizations, and turns procedure calls into `goto`. It can
also make loops out of recursive function nests. Guile also does dead
code elimination, common subexpression elimination, loop peeling and
invariant code motion, and range and type inference.

The rest of the optimization passes are really cleanups and
canonicalizations. CPS spans the gap between high-level languages and
low-level bytecodes, which allows much of the compilation process to be
expressed as source-to-source transformations. Such is the case for
closure conversion, in which references to variables that are free in a
function are converted to closure references, and in which functions are
converted to closures. There are a few more passes to ensure that the
only primcalls left in the term are those that have a corresponding
instruction in the virtual machine, and that their continuations expect
the right number of values.

Finally, the backend of the CPS compiler emits bytecode for each
function, one by one. To do so, it determines the set of live variables
at all points in the function. Using this liveness information, it
allocates stack slots to each variable, such that a variable can live in
one slot for the duration of its lifetime, without shuffling. (Of
course, variables with disjoint lifetimes can share a slot.) Finally
the backend emits code, typically just one VM instruction, for each
continuation in the function.

---

Next: [Writing New High-Level Languages](#Writing-New-High_002dLevel-Languages), Previous: [Continuation-Passing Style](#Continuation_002dPassing-Style), Up: [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.5 Bytecode [¶](#Bytecode-1)

As mentioned before, Guile compiles all code to bytecode, and that
bytecode is contained in ELF images. See [Object File Format](#Object-File-Format), for
more on Guile’s use of ELF.

To produce a bytecode image, Guile provides an assembler and a linker.

The assembler, defined in the `(system vm assembler)` module, has a
relatively straightforward imperative interface. It provides a
`make-assembler` function to instantiate an assembler and a set of
`emit-inst` procedures to emit instructions of each kind.

The `emit-inst` procedures are actually generated at
compile-time from a machine-readable description of the VM. With a few
exceptions for certain operand types, each operand of an emit procedure
corresponds to an operand of the corresponding instruction.

Consider `allocate-words`, from see [Memory Access Instructions](#Memory-Access-Instructions).
It is documented as:

Instruction: **allocate-words** `s12:dst s12:nwords` [¶](#index-allocate_002dwords-1)

Therefore the emit procedure has the form:

Scheme Procedure: **emit-allocate-words** asm dst nwords [¶](#index-emit_002dallocate_002dwords)

All emit procedure take the assembler as their first argument, and
return no useful values.

The argument types depend on the operand types. See [Instruction Set](#Instruction-Set).
Most are integers within a restricted range, though labels are generally
expressed as opaque symbols. Besides the emitters that correspond to
instructions, there are a few additional helpers defined in the
assembler module.

Scheme Procedure: **emit-label** asm label [¶](#index-emit_002dlabel)
:   Define a label at the current program point.

Scheme Procedure: **emit-source** asm source [¶](#index-emit_002dsource)
:   Associate source with the current program point.

Scheme Procedure: **emit-cache-ref** asm dst key [¶](#index-emit_002dcache_002dref)

Scheme Procedure: **emit-cache-set!** asm key val [¶](#index-emit_002dcache_002dset_0021)
:   Macro-instructions to implement compilation-unit caches. A single cache
    cell corresponding to key will be allocated for the compilation
    unit.

Scheme Procedure: **emit-load-constant** asm dst constant [¶](#index-emit_002dload_002dconstant)
:   Load the Scheme datum constant into dst.

Scheme Procedure: **emit-begin-program** asm label properties [¶](#index-emit_002dbegin_002dprogram)

Scheme Procedure: **emit-end-program** asm [¶](#index-emit_002dend_002dprogram)
:   Delimit the bounds of a procedure, with the given label and the
    metadata properties.

Scheme Procedure: **emit-load-static-procedure** asm dst label [¶](#index-emit_002dload_002dstatic_002dprocedure)
:   Load a procedure with the given label into local dst. This
    macro-instruction should only be used with procedures without free
    variables – procedures that are not closures.

Scheme Procedure: **emit-begin-standard-arity** asm req nlocals alternate [¶](#index-emit_002dbegin_002dstandard_002darity)

Scheme Procedure: **emit-begin-opt-arity** asm req opt rest nlocals alternate [¶](#index-emit_002dbegin_002dopt_002darity)

Scheme Procedure: **emit-begin-kw-arity** asm req opt rest kw-indices allow-other-keys? nlocals alternate [¶](#index-emit_002dbegin_002dkw_002darity)

Scheme Procedure: **emit-end-arity** asm [¶](#index-emit_002dend_002darity)
:   Delimit a clause of a procedure.

The linker is a complicated beast. Hackers interested in how it works
would do well do read Ian Lance Taylor’s series of articles on linkers.
Searching the internet should find them easily. From the user’s
perspective, there is only one knob to control: whether the resulting
image will be written out to a file or not. If the user passes
`#:to-file? #t` as part of the compiler options (see [The Scheme Compiler](#The-Scheme-Compiler)), the linker will align the resulting segments on page
boundaries, and otherwise not.

Scheme Procedure: **link-assembly** asm #:page-aligned?=#t [¶](#index-link_002dassembly)
:   Link an ELF image, and return the bytevector. If page-aligned? is
    true, Guile will align the segments with different permissions on
    page-sized boundaries, in order to maximize code sharing between
    different processes. Otherwise, padding is minimized, to minimize
    address space consumption.

To write an image to disk, just use `put-bytevector` from
`(ice-9 binary-ports)`.

Compiling object code to the fake language, `value`, is performed
via loading objcode into a program, then executing that thunk with
respect to the compilation environment. Normally the environment
propagates through the compiler transparently, but users may specify the
compilation environment manually as well, as a module. Procedures to
load images can be found in the `(system vm loader)` module:

```
(use-modules (system vm loader))
```

Scheme Variable: **load-thunk-from-file** file [¶](#index-load_002dthunk_002dfrom_002dfile)

C Function: **scm\_load\_thunk\_from\_file** (file) [¶](#index-scm_005fload_005fthunk_005ffrom_005ffile)
:   Load object code from a file named file. The file will be mapped
    into memory via `mmap`, so this is a very fast operation.

Scheme Variable: **load-thunk-from-memory** bv [¶](#index-load_002dthunk_002dfrom_002dmemory)

C Function: **scm\_load\_thunk\_from\_memory** (bv) [¶](#index-scm_005fload_005fthunk_005ffrom_005fmemory)
:   Load object code from a bytevector. The data will be copied out of the
    bytevector in order to ensure proper alignment of embedded Scheme
    values.

Additionally there are procedures to find the ELF image for a given
pointer, or to list all mapped ELF images:

Scheme Variable: **find-mapped-elf-image** ptr [¶](#index-find_002dmapped_002delf_002dimage)
:   Given the integer value ptr, find and return the ELF image that
    contains that pointer, as a bytevector. If no image is found, return
    `#f`. This routine is mostly used by debuggers and other
    introspective tools.

Scheme Variable: **all-mapped-elf-images** [¶](#index-all_002dmapped_002delf_002dimages)
:   Return all mapped ELF images, as a list of bytevectors.

---

Next: [Extending the Compiler](#Extending-the-Compiler), Previous: [Bytecode](#Bytecode), Up: [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.6 Writing New High-Level Languages [¶](#Writing-New-High_002dLevel-Languages-1)

In order to integrate a new language lang into Guile’s compiler
system, one has to create the module `(language lang spec)`
containing the language definition and referencing the parser,
compiler and other routines processing it. The module hierarchy in
`(language brainfuck)` defines a very basic Brainfuck
implementation meant to serve as easy-to-understand example on how to
do this. See for instance <http://en.wikipedia.org/wiki/Brainfuck>
for more information about the Brainfuck language itself.

---

Previous: [Writing New High-Level Languages](#Writing-New-High_002dLevel-Languages), Up: [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.4.7 Extending the Compiler [¶](#Extending-the-Compiler-1)

At this point we take a detour from the impersonal tone of the rest of
the manual. Admit it: if you’ve read this far into the compiler
internals manual, you are a junkie. Perhaps a course at your university
left you unsated, or perhaps you’ve always harbored a desire to hack the
holy of computer science holies: a compiler. Well you’re in good
company, and in a good position. Guile’s compiler needs your help.

There are many possible avenues for improving Guile’s compiler.
Probably the most important improvement, speed-wise, will be some form
of optimized ahead-of-time native compilation with global register
allocation. A first pass could simply extend the compiler to also emit
machine code in addition to bytecode, pre-filling the corresponding JIT
data structures referenced by the `instrument-entry` bytecodes.
See [Instrumentation Instructions](#Instrumentation-Instructions).

The compiler also needs help at the top end, adding new high-level
compilers. We have JavaScript and Emacs Lisp mostly complete, but they
could use some love; Lua would be nice as well, but whatever language it
is that strikes your fancy would be welcome too.

Compilers are for hacking, not for admiring or for complaining about.
Get to it!

---

Next: [Concept Index](#Concept-Index), Previous: [Guile Implementation](#Guile-Implementation), Up: [The Guile Reference Manual](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]## Appendix A GNU Free Documentation License [¶](#GNU-Free-Documentation-License-1)

Version 1.3, 3 November 2008

```
Copyright © 2000, 2001, 2002, 2007, 2008 Free Software Foundation, Inc.
http://fsf.org/

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
