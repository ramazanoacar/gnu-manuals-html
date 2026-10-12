### 7.17 `sxml-match`: Pattern Matching of SXML [¶](#sxml_002dmatch_003a-Pattern-Matching-of-SXML)

The `(sxml match)` module provides syntactic forms for pattern
matching of SXML trees, in a “by example” style reminiscent of the
pattern matching of the `syntax-rules` and `syntax-case` macro
systems. See [SXML](#SXML), for more information on SXML.

The following example[29](#FOOT29) provides a brief
illustration, transforming a music album catalog language into HTML.

```
(define (album->html x)
  (sxml-match x
    ((album (@ (title ,t)) (catalog (num ,n) (fmt ,f)) ...)
     `(ul (li ,t)
          (li (b ,n) (i ,f)) ...))))
```

Three macros are provided: `sxml-match`, `sxml-match-let`, and
`sxml-match-let*`.

Compared to a standard s-expression pattern matcher (see [Pattern Matching](#Pattern-Matching)), `sxml-match` provides the following benefits:

* matching of SXML elements does not depend on any degree of normalization of the
  SXML;
* matching of SXML attributes (within an element) is under-ordered; the order of
  the attributes specified within the pattern need not match the ordering with the
  element being matched;
* all attributes specified in the pattern must be present in the element being
  matched; in the spirit that XML is ’extensible’, the element being matched may
  include additional attributes not specified in the pattern.

The present module is a descendant of WebIt!, and was inspired by an
s-expression pattern matcher developed by Erik Hilsdale, Dan Friedman, and Kent
Dybvig at Indiana University.

* [Syntax](#Syntax)
* [Matching XML Elements](#Matching-XML-Elements)
* [Ellipses in Patterns](#Ellipses-in-Patterns)
* [Ellipses in Quasiquote’d Output](#Ellipses-in-Quasiquote_0027d-Output)
* [Matching Nodesets](#Matching-Nodesets)
* [Matching the “Rest” of a Nodeset](#Matching-the-_0060_0060Rest_0027_0027-of-a-Nodeset)
* [Matching the Unmatched Attributes](#Matching-the-Unmatched-Attributes)
* [Default Values in Attribute Patterns](#Default-Values-in-Attribute-Patterns)
* [Guards in Patterns](#Guards-in-Patterns)
* [Catamorphisms](#Catamorphisms)
* [Named-Catamorphisms](#Named_002dCatamorphisms)
* [`sxml-match-let` and `sxml-match-let*`](#sxml_002dmatch_002dlet-and-sxml_002dmatch_002dlet_002a)

#### Syntax [¶](#Syntax)

`sxml-match` provides `case`-like form for pattern matching of XML
nodes.

Scheme Syntax: **sxml-match** input-expression clause1 clause2 … [¶](#index-sxml_002dmatch)
:   Match input-expression, an SXML tree, according to the given clauses
    (one or more), each consisting of a pattern and one or more expressions to be
    evaluated if the pattern match succeeds. Optionally, each clause within
    `sxml-match` may include a *guard expression*.

The pattern notation is based on that of Scheme’s `syntax-rules` and
`syntax-case` macro systems. The grammar for the `sxml-match` syntax
is given below:

```
match-form ::= (sxml-match input-expression
                 clause+)

clause ::= [node-pattern action-expression+]
         | [node-pattern (guard expression*) action-expression+]

node-pattern ::= literal-pattern
               | pat-var-or-cata
               | element-pattern
               | list-pattern

literal-pattern ::= string
                  | character
                  | number
                  | #t
                  | #f

attr-list-pattern ::= (@ attribute-pattern*)
                    | (@ attribute-pattern* . pat-var-or-cata)

attribute-pattern ::= (tag-symbol attr-val-pattern)

attr-val-pattern ::= literal-pattern
                   | pat-var-or-cata
                   | (pat-var-or-cata default-value-expr)

element-pattern ::= (tag-symbol attr-list-pattern?)
                  | (tag-symbol attr-list-pattern? nodeset-pattern)
                  | (tag-symbol attr-list-pattern?
                                nodeset-pattern? . pat-var-or-cata)

list-pattern ::= (list nodeset-pattern)
               | (list nodeset-pattern? . pat-var-or-cata)
               | (list)

nodeset-pattern ::= node-pattern
                  | node-pattern ...
                  | node-pattern nodeset-pattern
                  | node-pattern ... nodeset-pattern

pat-var-or-cata ::= (unquote var-symbol)
                  | (unquote [var-symbol*])
                  | (unquote [cata-expression -> var-symbol*])
```

Within a list or element body pattern, ellipses may appear only once, but may be
followed by zero or more node patterns.

Guard expressions cannot refer to the return values of catamorphisms.

Ellipses in the output expressions must appear only in an expression context;
ellipses are not allowed in a syntactic form.

The sections below illustrate specific aspects of the `sxml-match` pattern
matcher.

#### Matching XML Elements [¶](#Matching-XML-Elements)

The example below illustrates the pattern matching of an XML element:

```
(sxml-match '(e (@ (i 1)) 3 4 5)
  ((e (@ (i ,d)) ,a ,b ,c) (list d a b c))
  (,otherwise #f))
```

Each clause in `sxml-match` contains two parts: a pattern and one or more
expressions which are evaluated if the pattern is successfully match. The
example above matches an element `e` with an attribute `i` and three
children.

Pattern variables must be “unquoted” in the pattern. The above expression
binds d to `1`, a to `3`, b to `4`, and c
to `5`.

#### Ellipses in Patterns [¶](#Ellipses-in-Patterns)

As in `syntax-rules`, ellipses may be used to specify a repeated pattern.
Note that the pattern `item ...` specifies zero-or-more matches of the
pattern `item`.

The use of ellipses in a pattern is illustrated in the code fragment below,
where nested ellipses are used to match the children of repeated instances of an
`a` element, within an element `d`.

```
(define x '(d (a 1 2 3) (a 4 5) (a 6 7 8) (a 9 10)))

(sxml-match x
  ((d (a ,b ...) ...)
   (list (list b ...) ...)))
```

The above expression returns a value of `((1 2 3) (4 5) (6 7 8) (9 10))`.

#### Ellipses in Quasiquote’d Output [¶](#Ellipses-in-Quasiquote_0027d-Output)

Within the body of an `sxml-match` form, a slightly extended version of
quasiquote is provided, which allows the use of ellipses. This is illustrated
in the example below.

```
(sxml-match '(e 3 4 5 6 7)
  ((e ,i ... 6 7) `("start" ,(list 'wrap i) ... "end"))
  (,otherwise #f))
```

The general pattern is that `` `(something ,i ...) `` is rewritten as
`` `(something ,@i) ``.

#### Matching Nodesets [¶](#Matching-Nodesets)

A nodeset pattern is designated by a list in the pattern, beginning the
identifier list. The example below illustrates matching a nodeset.

```
(sxml-match '("i" "j" "k" "l" "m")
  ((list ,a ,b ,c ,d ,e)
   `((p ,a) (p ,b) (p ,c) (p ,d) (p ,e))))
```

This example wraps each nodeset item in an HTML paragraph element. This example
can be rewritten and simplified through using ellipsis:

```
(sxml-match '("i" "j" "k" "l" "m")
  ((list ,i ...)
   `((p ,i) ...)))
```

This version will match nodesets of any length, and wrap each item in the
nodeset in an HTML paragraph element.

#### Matching the “Rest” of a Nodeset [¶](#Matching-the-_0060_0060Rest_0027_0027-of-a-Nodeset)

Matching the “rest” of a nodeset is achieved by using a `. rest)` pattern
at the end of an element or nodeset pattern.

This is illustrated in the example below:

```
(sxml-match '(e 3 (f 4 5 6) 7)
  ((e ,a (f . ,y) ,d)
   (list a y d)))
```

The above expression returns `(3 (4 5 6) 7)`.

#### Matching the Unmatched Attributes [¶](#Matching-the-Unmatched-Attributes)

Sometimes it is useful to bind a list of attributes present in the element being
matched, but which do not appear in the pattern. This is achieved by using a
`. rest)` pattern at the end of the attribute list pattern. This is
illustrated in the example below:

```
(sxml-match '(a (@ (z 1) (y 2) (x 3)) 4 5 6)
  ((a (@ (y ,www) . ,qqq) ,t ,u ,v)
   (list www qqq t u v)))
```

The above expression matches the attribute `y` and binds a list of the
remaining attributes to the variable qqq. The result of the above
expression is `(2 ((z 1) (x 3)) 4 5 6)`.

This type of pattern also allows the binding of all attributes:

```
(sxml-match '(a (@ (z 1) (y 2) (x 3)))
  ((a (@ . ,qqq))
   qqq))
```

#### Default Values in Attribute Patterns [¶](#Default-Values-in-Attribute-Patterns)

It is possible to specify a default value for an attribute which is used if the
attribute is not present in the element being matched. This is illustrated in
the following example:

```
(sxml-match '(e 3 4 5)
  ((e (@ (z (,d 1))) ,a ,b ,c) (list d a b c)))
```

The value `1` is used when the attribute `z` is absent from the
element `e`.

#### Guards in Patterns [¶](#Guards-in-Patterns)

Guards may be added to a pattern clause via the `guard` keyword. A guard
expression may include zero or more expressions which are evaluated only if the
pattern is matched. The body of the clause is only evaluated if the guard
expressions evaluate to `#t`.

The use of guard expressions is illustrated below:

```
(sxml-match '(a 2 3)
  ((a ,n) (guard (number? n)) n)
  ((a ,m ,n) (guard (number? m) (number? n)) (+ m n)))
```

#### Catamorphisms [¶](#Catamorphisms)

The example below illustrates the use of explicit recursion within an
`sxml-match` form. This example implements a simple calculator for the
basic arithmetic operations, which are represented by the XML elements
`plus`, `minus`, `times`, and `div`.

```
(define simple-eval
  (lambda (x)
    (sxml-match x
      (,i (guard (integer? i)) i)
      ((plus ,x ,y) (+ (simple-eval x) (simple-eval y)))
      ((times ,x ,y) (* (simple-eval x) (simple-eval y)))
      ((minus ,x ,y) (- (simple-eval x) (simple-eval y)))
      ((div ,x ,y) (/ (simple-eval x) (simple-eval y)))
      (,otherwise (error "simple-eval: invalid expression" x)))))
```

Using the catamorphism feature of `sxml-match`, a more concise version of
`simple-eval` can be written. The pattern `,(x)` recursively invokes
the pattern matcher on the value bound in this position.

```
(define simple-eval
  (lambda (x)
    (sxml-match x
      (,i (guard (integer? i)) i)
      ((plus ,(x) ,(y)) (+ x y))
      ((times ,(x) ,(y)) (* x y))
      ((minus ,(x) ,(y)) (- x y))
      ((div ,(x) ,(y)) (/ x y))
      (,otherwise (error "simple-eval: invalid expression" x)))))
```

#### Named-Catamorphisms [¶](#Named_002dCatamorphisms)

It is also possible to explicitly name the operator in the “cata” position.
Where `,(id*)` recurs to the top of the current `sxml-match`,
`,(cata -> id*)` recurs to `cata`. `cata` must evaluate to a
procedure which takes one argument, and returns as many values as there are
identifiers following `->`.

Named catamorphism patterns allow processing to be split into multiple, mutually
recursive procedures. This is illustrated in the example below: a
transformation that formats a “TV Guide” into HTML.

```
(define (tv-guide->html g)
  (define (cast-list cl)
    (sxml-match cl
      ((CastList (CastMember (Character (Name ,ch)) (Actor (Name ,a))) ...)
       `(div (ul (li ,ch ": " ,a) ...)))))
  (define (prog p)
    (sxml-match p
      ((Program (Start ,start-time) (Duration ,dur) (Series ,series-title)
                (Description ,desc ...))
       `(div (p ,start-time
                (br) ,series-title
                (br) ,desc ...)))
      ((Program (Start ,start-time) (Duration ,dur) (Series ,series-title)
                (Description ,desc ...)
                ,(cast-list -> cl))
       `(div (p ,start-time
                (br) ,series-title
                (br) ,desc ...)
             ,cl))))
  (sxml-match g
    ((TVGuide (@ (start ,start-date)
                 (end ,end-date))
              (Channel (Name ,nm) ,(prog -> p) ...) ...)
     `(html (head (title "TV Guide"))
            (body (h1 "TV Guide")
                  (div (h2 ,nm) ,p ...) ...)))))
```

#### `sxml-match-let` and `sxml-match-let*` [¶](#sxml_002dmatch_002dlet-and-sxml_002dmatch_002dlet_002a)

Scheme Syntax: **sxml-match-let** ((pat expr) ...) expression0 expression ... [¶](#index-sxml_002dmatch_002dlet)

Scheme Syntax: **sxml-match-let\*** ((pat expr) ...) expression0 expression ... [¶](#index-sxml_002dmatch_002dlet_002a)
:   These forms generalize the `let` and `let*` forms of Scheme to allow
    an XML pattern in the binding position, rather than a simple variable.

For example, the expression below:

```
(sxml-match-let (((a ,i ,j) '(a 1 2)))
  (+ i j))
```

binds the variables i and j to `1` and `2` in the XML
value given.

---

Next: [Curried Definitions](#Curried-Definitions), Previous: [`sxml-match`: Pattern Matching of SXML](#sxml_002dmatch), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 7.18 The Scheme shell (scsh) [¶](#The-Scheme-shell-_0028scsh_0029-1)

An incomplete port of the Scheme shell (scsh) was once available for
Guile as a separate package. However this code has bitrotten
somewhat. The pieces are available in Guile’s legacy CVS repository,
which may be browsed at
<http://cvs.savannah.gnu.org/viewvc/guile/guile-scsh/?root=guile>.

For information about scsh see
<http://www.scsh.net/>.

This bitrotting is a bit of a shame, as there is a good deal of
well-written Scheme code in scsh. Adopting this code and porting it to
current Guile should be an educational experience, in addition to
providing something of value to Guile folks.

---

Next: [Statprof](#Statprof), Previous: [The Scheme shell (scsh)](#The-Scheme-shell-_0028scsh_0029), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 7.19 Curried Definitions [¶](#Curried-Definitions-1)

The macros in this section are provided by

```
(use-modules (ice-9 curried-definitions))
```

and replace those provided by default.

Prior to Guile 2.0, Guile provided a type of definition known colloquially
as a “curried definition”. The idea is to extend the syntax of
`define` so that you can conveniently define procedures that return
procedures, up to any desired depth.

For example,

```
(define ((foo x) y)
  (list x y))
```

is a convenience form of

```
(define foo
  (lambda (x)
    (lambda (y)
      (list x y))))
```

Scheme Syntax: **define** (… (name args …) …) body … [¶](#index-define-2)

Scheme Syntax: **define\*** (… (name args …) …) body … [¶](#index-define_002a-1)

Scheme Syntax: **define-public** (… (name args …) …) body … [¶](#index-define_002dpublic-1)
:   Create a top level variable name bound to the procedure with
    parameter list args. If name is itself a formal parameter
    list, then a higher order procedure is created using that
    formal-parameter list, and returning a procedure that has parameter list
    args. This nesting may occur to arbitrary depth.

    `define*` is similar but the formal parameter lists take additional
    options as described in [lambda\* and define\*.](#lambda_002a-and-define_002a). For example,

    ```
    (define* ((foo #:keys (bar 'baz) (quux 'zot)) frotz #:rest rest)
      (list bar quux frotz rest))

    ((foo #:quux 'foo) 1 2 3 4 5)
    ⇒ (baz foo 1 (2 3 4 5))
    ```

    `define-public` is similar to `define` but it also adds
    name to the list of exported bindings of the current module.

---

Next: [SXML](#SXML), Previous: [Curried Definitions](#Curried-Definitions), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 7.20 Statprof [¶](#Statprof-1)

Statprof is a statistical profiler for Guile.

A simple use of statprof would look like this:

```
(use-modules (statprof))
(statprof (lambda ()
            (map 1+ (iota 1000000))
            #f))
```

This would run the thunk with statistical profiling, finally displaying
a flat table of statistics which could look something like this:

```
%     cumulative   self
time   seconds     seconds  procedure
 57.14  39769.73      0.07  ice-9/boot-9.scm:249:5:map1
 28.57      0.04      0.04  ice-9/boot-9.scm:1165:0:iota
 14.29      0.02      0.02  1+
  0.00      0.12      0.00  <current input>:2:10
---
Sample count: 7
Total time: 0.123490713 seconds (0.201983993 seconds in GC)
```

All of the numerical data with the exception of the calls column is
statistically approximate. In the following column descriptions, and in
all of statprof, “time” refers to execution time (both user and
system), not wall clock time.

The `% time` column indicates the percentage of the run-time time
spent inside the procedure itself (not counting children). It is
calculated as `self seconds`, measuring the amount of time spent in
the procedure, divided by the total run-time.

`cumulative seconds` also counts time spent in children of a
function. For recursive functions, this can exceed the total time, as
in our example above, because each activation on the stack adds to the
cumulative time.

Finally, the GC time measures the time spent in the garbage collector.
On systems with multiple cores, this time can be larger than the run
time, because it counts time spent in all threads, and will run the
“marking” phase of GC in parallel. If GC time is a significant
fraction of the run time, that means that most time in your program is
spent allocating objects and cleaning up after those allocations. To
speed up your program, one good place to start would be to look at how
to reduce the allocation rate.

Statprof’s main mode of operation is as a statistical profiler. However
statprof can also run in a “precise” mode as well. Pass the
`#:count-calls? #t` keyword argument to `statprof` to record
all calls:

```
(use-modules (statprof))
(statprof (lambda ()
            (map 1+ (iota 1000000))
            #f)
          #:count-calls? #t)
```

The result has an additional `calls` column:

```
%     cumulative   self             
time   seconds    seconds   calls   procedure
 82.26      0.73      0.73 1000000  1+
 11.29 420925.80      0.10 1000001  ice-9/boot-9.scm:249:5:map1
  4.84      0.06      0.04       1  ice-9/boot-9.scm:1165:0:iota
[...]
---
Sample count: 62
Total time: 0.893098065 seconds (1.222796536 seconds in GC)
```

As you can see, the profile is perturbed: `1+` ends up on top,
whereas it was not marked as hot in the earlier profile. This is
because the overhead of call-counting unfairly penalizes calls. Still,
this precise mode can be useful at times to do algorithmic optimizations
based on the precise call counts.

### Implementation notes [¶](#Implementation-notes)

The profiler works by setting the unix profiling signal
`ITIMER_PROF` to go off after the interval you define in the call
to `statprof-reset`. When the signal fires, a sampling routine
runs which crawls up the stack, recording all instruction pointers into
a buffer. After the sample is complete, the profiler resets profiling
timer to fire again after the appropriate interval.

Later, when profiling stops, that log buffer is analyzed to produce the
“self seconds” and “cumulative seconds” statistics. A procedure at
the top of the stack counts toward “self” samples, and everything on
the stack counts towards “cumulative” samples.

While the profiler is running it measures how much CPU time (system and
user – which is also what `ITIMER_PROF` tracks) has elapsed while
code has been executing within the profiler. Only run time counts
towards the profile, not wall-clock time. For example, sleeping and
waiting for input or output do not cause the timer clock to advance.

### Usage [¶](#Usage-13)

Scheme Procedure: **statprof** thunk [#:loop loop=1] [#:hz hz=100] [#:port port=(current-output-port)] [#:count-calls? count-calls?=#f] [#:display-style display-style=’flat] [¶](#index-statprof)
:   Profile the execution of thunk, and return its return values.

    The stack will be sampled hz times per second, and the thunk
    itself will be called loop times.

    If count-calls? is true, all procedure calls will be recorded.
    This operation is somewhat expensive.

    After the thunk has been profiled, print out a profile to
    port. If display-style is `flat`, the results will be
    printed as a flat profile. Otherwise if display-style is
    `tree`, print the results as a tree profile.

    Note that `statprof` requires a working profiling timer. Some
    platforms do not support profiling timers. `(provided?
    'ITIMER_PROF)` can be used to check for support of profiling timers.

Profiling can also be enabled and disabled manually.

Scheme Procedure: **statprof-active?** [¶](#index-statprof_002dactive_003f)
:   Returns `#t` if `statprof-start` has been called more times
    than `statprof-stop`, `#f` otherwise.

Scheme Procedure: **statprof-start** [¶](#index-statprof_002dstart)

Scheme Procedure: **statprof-stop** [¶](#index-statprof_002dstop)
:   Start or stop the profiler.

Scheme Procedure: **statprof-reset** sample-seconds sample-microseconds count-calls? [¶](#index-statprof_002dreset)
:   Reset the profiling sample interval to sample-seconds and
    sample-microseconds. If count-calls? is true, arrange to
    instrument procedure calls as well as collecting statistical profiling
    data.

If you use the manual `statprof-start`/`statprof-stop`
interface, an implicit statprof state will persist starting from the
last call to `statprof-reset`, or the first call to
`statprof-start`. There are a number of accessors to fetch
statistics from this implicit state.

Scheme Procedure: **statprof-accumulated-time** [¶](#index-statprof_002daccumulated_002dtime)
:   Returns the time accumulated during the last statprof run.

Scheme Procedure: **statprof-sample-count** [¶](#index-statprof_002dsample_002dcount)
:   Returns the number of samples taken during the last statprof run.

Scheme Procedure: **statprof-fold-call-data** proc init [¶](#index-statprof_002dfold_002dcall_002ddata)
:   Fold proc over the call-data accumulated by statprof. This
    procedure cannot be called while statprof is active.

    proc will be called with arguments, call-data and
    prior-result.

Scheme Procedure: **statprof-proc-call-data** proc [¶](#index-statprof_002dproc_002dcall_002ddata)
:   Returns the call-data associated with proc, or `#f` if none
    is available.

Scheme Procedure: **statprof-call-data-name** cd [¶](#index-statprof_002dcall_002ddata_002dname)

Scheme Procedure: **statprof-call-data-calls** cd [¶](#index-statprof_002dcall_002ddata_002dcalls)

Scheme Procedure: **statprof-call-data-cum-samples** cd [¶](#index-statprof_002dcall_002ddata_002dcum_002dsamples)

Scheme Procedure: **statprof-call-data-self-samples** cd [¶](#index-statprof_002dcall_002ddata_002dself_002dsamples)
:   Accessors for the fields in a statprof call-data object.

Scheme Procedure: **statprof-call-data->stats** call-data [¶](#index-statprof_002dcall_002ddata_002d_003estats)
:   Returns an object of type `statprof-stats`.

Scheme Procedure: **statprof-stats-proc-name** stats [¶](#index-statprof_002dstats_002dproc_002dname)

Scheme Procedure: **statprof-stats-%-time-in-proc** stats [¶](#index-statprof_002dstats_002d_0025_002dtime_002din_002dproc)

Scheme Procedure: **statprof-stats-cum-secs-in-proc** stats [¶](#index-statprof_002dstats_002dcum_002dsecs_002din_002dproc)

Scheme Procedure: **statprof-stats-self-secs-in-proc** stats [¶](#index-statprof_002dstats_002dself_002dsecs_002din_002dproc)

Scheme Procedure: **statprof-stats-calls** stats [¶](#index-statprof_002dstats_002dcalls)

Scheme Procedure: **statprof-stats-self-secs-per-call** stats [¶](#index-statprof_002dstats_002dself_002dsecs_002dper_002dcall)

Scheme Procedure: **statprof-stats-cum-secs-per-call** stats [¶](#index-statprof_002dstats_002dcum_002dsecs_002dper_002dcall)
:   Accessors for the fields in a `statprof-stats` object.

Scheme Procedure: **statprof-display** [port=(current-output-port)] [#:style style=flat] [¶](#index-statprof_002ddisplay)
:   Displays a summary of the statistics collected. Possible values for
    style include:

    `flat`
    :   Display a traditional gprof-style flat profile.

    `anomalies`
    :   Find statistical anomalies in the data.

    `tree`
    :   Display a tree profile.

Scheme Procedure: **statprof-fetch-stacks** [¶](#index-statprof_002dfetch_002dstacks)
:   Returns a list of stacks, as they were captured since the last call to
    `statprof-reset`.

Scheme Procedure: **statprof-fetch-call-tree** [#:precise precise?=#f] [¶](#index-statprof_002dfetch_002dcall_002dtree)
:   ```
    Return a call tree for the previous statprof run.

    The return value is a list of nodes.  A node is a list of the form:
    @code
     node ::= (@var{proc} @var{count} . @var{nodes})
    @end code

    The @var{proc} is a printable representation of a procedure, as a
    string.  If @var{precise?} is false, which is the default, then a node
    corresponds to a procedure invocation.  If it is true, then a node
    corresponds to a return point in a procedure.  Passing @code{#:precise?
    #t} allows a user to distinguish different source lines in a procedure,
    but usually it is too much detail, so it is off by default.
    ```

Scheme Procedure: **gcprof** thunk [#:loop] [¶](#index-gcprof)
:   Like the `statprof` procedure, but instead of profiling CPU time,
    we profile garbage collection.

    The stack will be sampled soon after every garbage collection during the
    evaluation of thunk, yielding an approximate idea of what is
    causing allocation in your program.

    Since GC does not occur very frequently, you may need to use the
    loop parameter, to cause thunk to be called loop
    times.

---

Next: [Texinfo Processing](#Texinfo-Processing), Previous: [Statprof](#Statprof), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 7.21 SXML [¶](#SXML-1)

SXML is a native representation of XML in terms of standard Scheme data
types: lists, symbols, and strings. For example, the simple XML
fragment:

```
<parrot type="African Grey"><name>Alfie</name></parrot>
```

may be represented with the following SXML:

```
(parrot (@ (type "African Grey")) (name "Alfie"))
```

SXML is very general, and is capable of representing all of XML.
Formally, this means that SXML is a conforming implementation of the
[XML Information Set](http://www.w3.org/TR/xml-infoset/) standard.

Guile includes several facilities for working with XML and SXML:
parsers, serializers, and transformers.

* [SXML Overview](#SXML-Overview)
* [Reading and Writing XML](#Reading-and-Writing-XML)
* [SSAX: A Functional XML Parsing Toolkit](#SSAX)
* [Transforming SXML](#Transforming-SXML)
* [SXML Tree Fold](#SXML-Tree-Fold)
* [SXPath](#SXPath)
* [(sxml ssax input-parse)](#sxml-ssax-input_002dparse)
* [(sxml apply-templates)](#sxml-apply_002dtemplates)

---

Next: [Reading and Writing XML](#Reading-and-Writing-XML), Up: [SXML](#SXML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.21.1 SXML Overview [¶](#SXML-Overview-1)

(This section needs to be written; volunteers welcome.)

---

Next: [SSAX: A Functional XML Parsing Toolkit](#SSAX), Previous: [SXML Overview](#SXML-Overview), Up: [SXML](#SXML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.21.2 Reading and Writing XML [¶](#Reading-and-Writing-XML-1)

The `(sxml simple)` module presents a basic interface for parsing
XML from a port into the Scheme SXML format, and for serializing it back
to text.

```
(use-modules (sxml simple))
```

Scheme Procedure: **xml->sxml** [string-or-port] [#:namespaces=’()] [#:declare-namespaces?=#t] [#:trim-whitespace?=#f] [#:entities=’()] [#:default-entity-handler=#f] [#:doctype-handler=#f] [¶](#index-xml_002d_003esxml)
:   Use SSAX to parse an XML document into SXML. Takes one optional
    argument, string-or-port, which defaults to the current input
    port. Returns the resulting SXML document. If string-or-port is
    a port, it will be left pointing at the next available character in the
    port.

As is normal in SXML, XML elements parse as tagged lists. Attributes,
if any, are placed after the tag, within an `@` element. The root
of the resulting XML will be contained in a special tag, `*TOP*`.
This tag will contain the root element of the XML, but also any prior
processing instructions.

```
(xml->sxml "<foo/>")
⇒ (*TOP* (foo))
(xml->sxml "<foo>text</foo>")
⇒ (*TOP* (foo "text"))
(xml->sxml "<foo kind=\"bar\">text</foo>")
⇒ (*TOP* (foo (@ (kind "bar")) "text"))
(xml->sxml "<?xml version=\"1.0\"?><foo/>")
⇒ (*TOP* (*PI* xml "version=\"1.0\"") (foo))
```

All namespaces in the XML document must be declared, via `xmlns`
attributes. SXML elements built from non-default namespaces will have
their tags prefixed with their URI. Users can specify custom prefixes
for certain namespaces with the `#:namespaces` keyword argument to
`xml->sxml`. A namespace can be removed by using a `#f` custom
prefix.

```
(xml->sxml "<foo xmlns=\"http://example.org/ns1\">text</foo>")
⇒ (*TOP* (http://example.org/ns1:foo "text"))
(xml->sxml "<foo xmlns=\"http://example.org/ns1\">text</foo>"
           #:namespaces '((ns1 . "http://example.org/ns1")))
⇒ (*TOP* (ns1:foo "text"))
(xml->sxml "<foo xmlns:bar=\"http://example.org/ns2\"><bar:baz/></foo>"
           #:namespaces '((ns2 . "http://example.org/ns2")))
⇒ (*TOP* (foo (ns2:baz)))
```

By default, namespaces passed to `xml->sxml` are treated as if they
were declared on the root element. Passing a false
`#:declare-namespaces?` argument will disable this behavior,
requiring in-document declarations of namespaces before use.

```
(xml->sxml "<foo><ns2:baz/></foo>"
           #:namespaces '((ns2 . "http://example.org/ns2")))
⇒ (*TOP* (foo (ns2:baz)))
(xml->sxml "<foo><ns2:baz/></foo>"
           #:namespaces '((ns2 . "http://example.org/ns2"))
           #:declare-namespaces? #f)
⇒ error: undeclared namespace: `bar'
```

By default, all whitespace in XML is significant. Passing the
`#:trim-whitespace?` keyword argument to `xml->sxml` will trim
whitespace in front, behind and between elements, treating it as
“unsignificant”. Whitespace in text fragments is left alone.

```
(xml->sxml "<foo>\n<bar> Alfie the parrot! </bar>\n</foo>")
⇒ (*TOP* (foo "\n" (bar " Alfie the parrot! ") "\n"))
(xml->sxml "<foo>\n<bar> Alfie the parrot! </bar>\n</foo>"
           #:trim-whitespace? #t)
⇒ (*TOP* (foo (bar " Alfie the parrot! ")))
```

Parsed entities may be declared with the `#:entities` keyword
argument, or handled with the `#:default-entity-handler`. By
default, only the standard `&lt;`, `&gt;`, `&amp;`,
`&apos;` and `&quot;` entities are defined, as well as the
`&#N;` and `&#xN;` (decimal and hexadecimal)
numeric character entities.

```
(xml->sxml "<foo>&amp;</foo>")
⇒ (*TOP* (foo "&"))
(xml->sxml "<foo>&nbsp;</foo>")
⇒ error: undefined entity: nbsp
(xml->sxml "<foo>&#xA0;</foo>")
⇒ (*TOP* (foo "\xa0"))
(xml->sxml "<foo>&nbsp;</foo>"
           #:entities '((nbsp . "\xa0")))
⇒ (*TOP* (foo "\xa0"))
(xml->sxml "<foo>&nbsp; &foo;</foo>"
           #:default-entity-handler
           (lambda (port name)
             (case name
               ((nbsp) "\xa0")
               (else
                (format (current-warning-port)
                        "~a:~a:~a: undefined entitity: ~a\n"
                        (or (port-filename port) "<unknown file>")
                        (port-line port) (port-column port)
                        name)
                (symbol->string name)))))
⊣ <unknown file>:0:17: undefined entitity: foo
⇒ (*TOP* (foo "\xa0 foo"))
```

By default, `xml->sxml` skips over the `<!DOCTYPE>`
declaration, if any. This behavior can be overridden with the
`#:doctype-handler` argument, which should be a procedure of three
arguments: the *docname* (a symbol), *systemid* (a string), and
the internal doctype subset (as a string or `#f` if not present).

The handler should return keyword arguments as multiple values, as if it
were calling its continuation with keyword arguments. The continuation
accepts the `#:entities` and `#:namespaces` keyword arguments,
in the same format that `xml->sxml` itself takes. These entities
and namespaces will be prepended to those given to the `xml->sxml`
invocation.

```
(define (handle-foo docname systemid internal-subset)
  (case docname
    ((foo)
     (values #:entities '((greets . "<i>Hello, world!</i>"))))
    (else
     (values))))

(xml->sxml "<!DOCTYPE foo><p>&greets;</p>"
           #:doctype-handler handle-foo)
⇒ (*TOP* (p (i "Hello, world!")))
```

If the document has no doctype declaration, the doctype-handler is
invoked with `#f` for the three arguments.

In the future, the continuation may accept other keyword arguments, for
example to validate the parsed SXML against the doctype.

Scheme Procedure: **sxml->xml** tree [port] [¶](#index-sxml_002d_003exml)
:   Serialize the SXML tree tree as XML. The output will be written to
    the current output port, unless the optional argument port is
    present.

Output is automatically escaped as expected for a serializer.

```
(sxml->xml `(code "<i>Some example</i>"))
⇒ <code>&lt;i&gt;Some example&lt;/i&gt;</code>
```

In order to avoid some string from being escaped, it must be wrapped in a
procedure that writes the string to the `(current-output-port)`.

```
(sxml->xml `(code ,(lambda () (display "<i>Some example</i>"))))
⇒ <code><i>Some example</i></code>
```

Scheme Procedure: **sxml->string** sxml [¶](#index-sxml_002d_003estring)
:   Detag an sxml tree sxml into a string. Does not perform any
    formatting.

---

Next: [Transforming SXML](#Transforming-SXML), Previous: [Reading and Writing XML](#Reading-and-Writing-XML), Up: [SXML](#SXML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.21.3 SSAX: A Functional XML Parsing Toolkit [¶](#SSAX_003a-A-Functional-XML-Parsing-Toolkit)

Guile’s XML parser is based on Oleg Kiselyov’s powerful XML parsing
toolkit, SSAX.

* [History](#History-1)
* [Implementation](#Implementation)
* [Usage](#Usage)

#### 7.21.3.1 History [¶](#History-1)

Back in the 1990s, when the world was young again and XML was the
solution to all of its problems, there were basically two kinds of XML
parsers out there: DOM parsers and SAX parsers.

A DOM parser reads through an entire XML document, building up a tree of
“DOM objects” representing the document structure. They are very easy
to use, but sometimes you don’t actually want all of the information in
a document; building an object tree is not necessary if all you want to
do is to count word frequencies in a document, for example.

SAX parsers were created to give the programmer more control on the
parsing process. A programmer gives the SAX parser a number of
“callbacks”: functions that will be called on various features of the
XML stream as they are encountered. SAX parsers are more efficient, but
much harder to use, as users typically have to manually maintain a
stack of open elements.

Kiselyov realized that the SAX programming model could be made much
simpler if the callbacks were formulated not as a linear fold across the
features of the XML stream, but as a *tree fold* over the structure
implicit in the XML. In this way, the user has a very convenient,
functional-style interface that can still generate optimal parsers.

The `xml->sxml` interface from the `(sxml simple)` module is a
DOM-style parser built using SSAX, though it returns SXML instead of DOM
objects.

#### 7.21.3.2 Implementation [¶](#Implementation)

`(sxml ssax)` is a package of low-to-high level lexing and parsing
procedures that can be combined to yield a SAX, a DOM, a validating
parser, or a parser intended for a particular document type. The
procedures in the package can be used separately to tokenize or parse
various pieces of XML documents. The package supports XML Namespaces,
internal and external parsed entities, user-controlled handling of
whitespace, and validation. This module therefore is intended to be a
framework, a set of “Lego blocks” you can use to build a parser
following any discipline and performing validation to any degree. As an
example of the parser construction, the source file includes a
semi-validating SXML parser.

SSAX has a “sequential” feel of SAX yet a “functional style” of DOM.
Like a SAX parser, the framework scans the document only once and
permits incremental processing. An application that handles document
elements in order can run as efficiently as possible. *Unlike* a
SAX parser, the framework does not require an application register
stateful callbacks and surrender control to the parser. Rather, it is
the application that can drive the framework – calling its functions to
get the current lexical or syntax element. These functions do not
maintain or mutate any state save the input port. Therefore, the
framework permits parsing of XML in a pure functional style, with the
input port being a monad (or a linear, read-once parameter).

Besides the port, there is another monad – seed. Most of
the middle- and high-level parsers are single-threaded through the
seed. The functions of this framework do not process or affect
the seed in any way: they simply pass it around as an instance of
an opaque datatype. User functions, on the other hand, can use the seed
to maintain user’s state, to accumulate parsing results, etc. A user
can freely mix their own functions with those of the framework. On the
other hand, the user may wish to instantiate a high-level parser:
`SSAX:make-elem-parser` or `SSAX:make-parser`. In the latter
case, the user must provide functions of specific signatures, which are
called at predictable moments during the parsing: to handle character
data, element data, or processing instructions (PI). The functions are
always given the seed, among other parameters, and must return the
new seed.

From a functional point of view, XML parsing is a combined
pre-post-order traversal of a “tree” that is the XML document itself.
This down-and-up traversal tells the user about an element when its
start tag is encountered. The user is notified about the element once
more, after all element’s children have been handled. The process of
XML parsing therefore is a fold over the raw XML document. Unlike a
fold over trees defined in [1], the parser is necessarily
single-threaded – obviously as elements in a text XML document are laid
down sequentially. The parser therefore is a tree fold that has been
transformed to accept an accumulating parameter [1,2].

Formally, the denotational semantics of the parser can be expressed as

```
 parser:: (Start-tag -> Seed -> Seed) ->
	   (Start-tag -> Seed -> Seed -> Seed) ->
	   (Char-Data -> Seed -> Seed) ->
	   XML-text-fragment -> Seed -> Seed
 parser fdown fup fchar "<elem attrs> content </elem>" seed
  = fup "<elem attrs>" seed
	(parser fdown fup fchar "content" (fdown "<elem attrs>" seed))

 parser fdown fup fchar "char-data content" seed
  = parser fdown fup fchar "content" (fchar "char-data" seed)

 parser fdown fup fchar "elem-content content" seed
  = parser fdown fup fchar "content" (
	parser fdown fup fchar "elem-content" seed)
```

Compare the last two equations with the left fold

```
 fold-left kons elem:list seed = fold-left kons list (kons elem seed)
```

The real parser created by `SSAX:make-parser` is slightly more
complicated, to account for processing instructions, entity references,
namespaces, processing of document type declaration, etc.

The XML standard document referred to in this module is
<http://www.w3.org/TR/1998/REC-xml-19980210.html>

The present file also defines a procedure that parses the text of an XML
document or of a separate element into SXML, an S-expression-based model
of an XML Information Set. SXML is also an Abstract Syntax Tree of an
XML document. SXML is similar but not identical to DOM; SXML is
particularly suitable for Scheme-based XML/HTML authoring, SXPath
queries, and tree transformations. See SXML.html for more details.
SXML is a term implementation of evaluation of the XML document [3].
The other implementation is context-passing.

The present frameworks fully supports the XML Namespaces Recommendation:
<http://www.w3.org/TR/REC-xml-names/>.

Other links:

[1]
:   Jeremy Gibbons, Geraint Jones, "The Under-appreciated Unfold," Proc.
    ICFP’98, 1998, pp. 273-279.

[2]
:   Richard S. Bird, The promotion and accumulation strategies in
    transformational programming, ACM Trans. Progr. Lang. Systems,
    6(4):487-504, October 1984.

[3]
:   Ralf Hinze, "Deriving Backtracking Monad Transformers," Functional
    Pearl. Proc ICFP’00, pp. 186-197.

#### 7.21.3.3 Usage [¶](#Usage)

Scheme Procedure: **current-ssax-error-port** [¶](#index-current_002dssax_002derror_002dport)

Scheme Procedure: **with-ssax-error-to-port** port thunk [¶](#index-with_002dssax_002derror_002dto_002dport)

Scheme Procedure: **xml-token?** \_ [¶](#index-xml_002dtoken_003f)
:   ```
     -- Scheme Procedure: pair? x
         Return `#t' if X is a pair; otherwise return `#f'.
    ```

Scheme Syntax: **xml-token-kind** token [¶](#index-xml_002dtoken_002dkind)

Scheme Syntax: **xml-token-head** token [¶](#index-xml_002dtoken_002dhead)

Scheme Procedure: **make-empty-attlist** [¶](#index-make_002dempty_002dattlist)

Scheme Procedure: **attlist-add** attlist name-value [¶](#index-attlist_002dadd)

Scheme Procedure: **attlist-null?** x [¶](#index-attlist_002dnull_003f)
:   Return `#t` if x is the empty list, else `#f`.

Scheme Procedure: **attlist-remove-top** attlist [¶](#index-attlist_002dremove_002dtop)

Scheme Procedure: **attlist->alist** attlist [¶](#index-attlist_002d_003ealist)

Scheme Procedure: **attlist-fold** kons knil lis1 [¶](#index-attlist_002dfold)

Scheme Procedure: **define-parsed-entity!** entity str [¶](#index-define_002dparsed_002dentity_0021)
:   Define a new parsed entity. entity should be a symbol.

    Instances of &entity; in XML text will be replaced with the string
    str, which will then be parsed.

Scheme Procedure: **reset-parsed-entity-definitions!** [¶](#index-reset_002dparsed_002dentity_002ddefinitions_0021)
:   Restore the set of parsed entity definitions to its initial state.

Scheme Procedure: **ssax:uri-string->symbol** uri-str [¶](#index-ssax_003auri_002dstring_002d_003esymbol)

Scheme Procedure: **ssax:skip-internal-dtd** port [¶](#index-ssax_003askip_002dinternal_002ddtd)

Scheme Procedure: **ssax:read-pi-body-as-string** port [¶](#index-ssax_003aread_002dpi_002dbody_002das_002dstring)

Scheme Procedure: **ssax:reverse-collect-str-drop-ws** fragments [¶](#index-ssax_003areverse_002dcollect_002dstr_002ddrop_002dws)

Scheme Procedure: **ssax:read-markup-token** port [¶](#index-ssax_003aread_002dmarkup_002dtoken)

Scheme Procedure: **ssax:read-cdata-body** port str-handler seed [¶](#index-ssax_003aread_002dcdata_002dbody)

Scheme Procedure: **ssax:read-char-ref** port [¶](#index-ssax_003aread_002dchar_002dref)

Scheme Procedure: **ssax:read-attributes** port entities [¶](#index-ssax_003aread_002dattributes)

Scheme Procedure: **ssax:complete-start-tag** tag-head port elems entities namespaces [¶](#index-ssax_003acomplete_002dstart_002dtag)

Scheme Procedure: **ssax:read-external-id** port [¶](#index-ssax_003aread_002dexternal_002did)

Scheme Procedure: **ssax:read-char-data** port expect-eof? str-handler seed [¶](#index-ssax_003aread_002dchar_002ddata)

Scheme Procedure: **ssax:xml->sxml** port namespace-prefix-assig [¶](#index-ssax_003axml_002d_003esxml)

Scheme Syntax: **ssax:make-parser** . kw-val-pairs [¶](#index-ssax_003amake_002dparser)

Scheme Syntax: **ssax:make-pi-parser** orig-handlers [¶](#index-ssax_003amake_002dpi_002dparser)

Scheme Syntax: **ssax:make-elem-parser** my-new-level-seed my-finish-element my-char-data-handler my-pi-handlers [¶](#index-ssax_003amake_002delem_002dparser)

---

Next: [SXML Tree Fold](#SXML-Tree-Fold), Previous: [SSAX: A Functional XML Parsing Toolkit](#SSAX), Up: [SXML](#SXML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.21.4 Transforming SXML [¶](#Transforming-SXML-1)

* [Overview](#Overview)
* [Usage](#Usage-1)

#### 7.21.4.1 Overview [¶](#Overview)

### SXML expression tree transformers [¶](#SXML-expression-tree-transformers)

#### Pre-Post-order traversal of a tree and creation of a new tree [¶](#Pre_002dPost_002dorder-traversal-of-a-tree-and-creation-of-a-new-tree)

```
pre-post-order:: <tree> x <bindings> -> <new-tree>
```

where

```
 <bindings> ::= (<binding> ...)
 <binding> ::= (<trigger-symbol> *preorder* . <handler>) |
               (<trigger-symbol> *macro* . <handler>) |
		(<trigger-symbol> <new-bindings> . <handler>) |
		(<trigger-symbol> . <handler>)
 <trigger-symbol> ::= XMLname | *text* | *default*
 <handler> :: <trigger-symbol> x [<tree>] -> <new-tree>
```

The `pre-post-order` function, in the `(sxml transform)`
module, visits the nodes and nodelists pre-post-order (depth-first).
For each `<Node>` of the form `(name <Node> ...)`, it
looks up an association with the given name among its
<bindings>. If failed, `pre-post-order` tries to locate a
`*default*` binding. It’s an error if the latter attempt fails as
well. Having found a binding, the `pre-post-order` function first
checks to see if the binding is of the form

```
	(<trigger-symbol> *preorder* . <handler>)
```

If it is, the handler is ’applied’ to the current node. Otherwise, the
pre-post-order function first calls itself recursively for each child of
the current node, with <new-bindings> prepended to the
<bindings> in effect. The result of these calls is passed to the
<handler> (along with the head of the current <Node>). To be
more precise, the handler is \_applied\_ to the head of the current node
and its processed children. The result of the handler, which should also
be a `<tree>`, replaces the current <Node>. If the current
<Node> is a text string or other atom, a special binding with a
symbol `*text*` is looked up.

A binding can also be of a form

```
	(<trigger-symbol> *macro* . <handler>)
```

This is equivalent to `*preorder*` described above. However, the
result is re-processed again, with the current stylesheet.

#### 7.21.4.2 Usage [¶](#Usage-1)

Scheme Procedure: **SRV:send-reply** . fragments [¶](#index-SRV_003asend_002dreply)
:   Output the fragments to the current output port.

    The fragments are a list of strings, characters, numbers, thunks,
    `#f`, `#t` – and other fragments. The function traverses the
    tree depth-first, writes out strings and characters, executes thunks,
    and ignores `#f` and `'()`. The function returns `#t` if
    anything was written at all; otherwise the result is `#f` If
    `#t` occurs among the fragments, it is not written out but causes
    the result of `SRV:send-reply` to be `#t`.

Scheme Procedure: **foldts** fdown fup fhere seed tree [¶](#index-foldts)

Scheme Procedure: **post-order** tree bindings [¶](#index-post_002dorder)

Scheme Procedure: **pre-post-order** tree bindings [¶](#index-pre_002dpost_002dorder)

Scheme Procedure: **replace-range** beg-pred end-pred forest [¶](#index-replace_002drange)

---

Next: [SXPath](#SXPath), Previous: [Transforming SXML](#Transforming-SXML), Up: [SXML](#SXML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.21.5 SXML Tree Fold [¶](#SXML-Tree-Fold-1)

* [Overview](#Overview-1)
* [Usage](#Usage-2)

#### 7.21.5.1 Overview [¶](#Overview-1)

`(sxml fold)` defines a number of variants of the *fold*
algorithm for use in transforming SXML trees. Additionally it defines
the layout operator, `fold-layout`, which might be described as a
context-passing variant of SSAX’s `pre-post-order`.

#### 7.21.5.2 Usage [¶](#Usage-2)

Scheme Procedure: **foldt** fup fhere tree [¶](#index-foldt)
:   The standard multithreaded tree fold.

    fup is of type [a] -> a. fhere is of type object -> a.

Scheme Procedure: **foldts** fdown fup fhere seed tree [¶](#index-foldts-1)
:   The single-threaded tree fold originally defined in SSAX. See [SSAX: A Functional XML Parsing Toolkit](#SSAX),
    for more information.

Scheme Procedure: **foldts\*** fdown fup fhere seed tree [¶](#index-foldts_002a)
:   A variant of `foldts` that allows pre-order tree
    rewrites. Originally defined in Andy Wingo’s 2007 paper,
    *Applications of fold to XML transformation*.

Scheme Procedure: **fold-values** proc list . seeds [¶](#index-fold_002dvalues)
:   A variant of `fold` that allows multi-valued seeds. Note that the
    order of the arguments differs from that of `fold`. See [Fold, Unfold & Map](#SRFI_002d1-Fold-and-Map).

Scheme Procedure: **foldts\*-values** fdown fup fhere tree . seeds [¶](#index-foldts_002a_002dvalues)
:   A variant of `foldts*` that allows multi-valued
    seeds. Originally defined in Andy Wingo’s 2007 paper, *Applications
    of fold to XML transformation*.

Scheme Procedure: **fold-layout** tree bindings params layout stylesheet [¶](#index-fold_002dlayout)
:   A traversal combinator in the spirit of `pre-post-order`.
    See [Transforming SXML](#Transforming-SXML).

    `fold-layout` was originally presented in Andy Wingo’s 2007 paper,
    *Applications of fold to XML transformation*.

    ```
    bindings := (<binding>...)
    binding  := (<tag> <handler-pair>...)
              | (*default* . <post-handler>)
              | (*text* . <text-handler>)
    tag      := <symbol>
    handler-pair := (pre-layout . <pre-layout-handler>)
              | (post . <post-handler>)
              | (bindings . <bindings>)
              | (pre . <pre-handler>)
              | (macro . <macro-handler>)
    ```

    pre-layout-handler
    :   A function of three arguments:

        kids
        :   the kids of the current node, before traversal

        params
        :   the params of the current node

        layout
        :   the layout coming into this node

        pre-layout-handler is expected to use this information to return a
        layout to pass to the kids. The default implementation returns the
        layout given in the arguments.

    post-handler
    :   A function of five arguments:

        tag
        :   the current tag being processed

        params
        :   the params of the current node

        layout
        :   the layout coming into the current node, before any kids were processed

        klayout
        :   the layout after processing all of the children

        kids
        :   the already-processed child nodes

        post-handler should return two values, the layout to pass to the
        next node and the final tree.

    text-handler
    :   text-handler is a function of three arguments:

        text
        :   the string

        params
        :   the current params

        layout
        :   the current layout

        text-handler should return two values, the layout to pass to the
        next node and the value to which the string should transform.

---

Next: [(sxml ssax input-parse)](#sxml-ssax-input_002dparse), Previous: [SXML Tree Fold](#SXML-Tree-Fold), Up: [SXML](#SXML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.21.6 SXPath [¶](#SXPath-1)

* [Overview](#Overview-2)
* [Basic Converters and Applicators](#Basic-Converters-and-Applicators)
* [Converter Combinators](#Converter-Combinators)

#### 7.21.6.1 Overview [¶](#Overview-2)

### SXPath: SXML Query Language [¶](#SXPath_003a-SXML-Query-Language)

SXPath is a query language for SXML, an instance of XML Information set
(Infoset) in the form of s-expressions. See `(sxml ssax)` for the
definition of SXML and more details. SXPath is also a translation into
Scheme of an XML Path Language, [XPath](http://www.w3.org/TR/xpath).
XPath and SXPath describe means of selecting a set of Infoset’s items or
their properties.

To facilitate queries, XPath maps the XML Infoset into an explicit tree,
and introduces important notions of a location path and a current,
context node. A location path denotes a selection of a set of nodes
relative to a context node. Any XPath tree has a distinguished, root
node – which serves as the context node for absolute location paths.
Location path is recursively defined as a location step joined with a
location path. A location step is a simple query of the database
relative to a context node. A step may include expressions that further
filter the selected set. Each node in the resulting set is used as a
context node for the adjoining location path. The result of the step is
a union of the sets returned by the latter location paths.

The SXML representation of the XML Infoset (see SSAX.scm) is rather
suitable for querying as it is. Bowing to the XPath specification, we
will refer to SXML information items as ’Nodes’:

```
 	<Node> ::= <Element> | <attributes-coll> | <attrib>
 		   | "text string" | <PI>
```

This production can also be described as

```
	<Node> ::= (name . <Nodeset>) | "text string"
```

An (ordered) set of nodes is just a list of the constituent nodes:

```
 	<Nodeset> ::= (<Node> ...)
```

Nodesets, and Nodes other than text strings are both lists. A <Nodeset>
however is either an empty list, or a list whose head is not a symbol. A
symbol at the head of a node is either an XML name (in which case it’s a
tag of an XML element), or an administrative name such as ’@’. This
uniform list representation makes processing rather simple and elegant,
while avoiding confusion. The multi-branch tree structure formed by the
mutually-recursive datatypes <Node> and <Nodeset> lends itself well to
processing by functional languages.

A location path is in fact a composite query over an XPath tree or its
branch. A single step is a combination of a projection, selection or a
transitive closure. Multiple steps are combined via join and union
operations. This insight allows us to *elegantly* implement XPath
as a sequence of projection and filtering primitives – converters –
joined by *combinators*. Each converter takes a node and returns a
nodeset which is the result of the corresponding query relative to that
node. A converter can also be called on a set of nodes. In that case it
returns a union of the corresponding queries over each node in the set.
The union is easily implemented as a list append operation as all nodes
in a SXML tree are considered distinct, by XPath conventions. We also
preserve the order of the members in the union. Query combinators are
high-order functions: they take converter(s) (which is a Node|Nodeset ->
Nodeset function) and compose or otherwise combine them. We will be
concerned with only relative location paths [XPath]: an absolute
location path is a relative path applied to the root node.

Similarly to XPath, SXPath defines full and abbreviated notations for
location paths. In both cases, the abbreviated notation can be
mechanically expanded into the full form by simple rewriting rules. In
the case of SXPath the corresponding rules are given in the
documentation of the `sxpath` procedure.
See [SXPath procedure documentation](#sxpath_002dprocedure_002ddocs).

The regression test suite at the end of the file `SXPATH-old.scm`
shows a representative sample of SXPaths in both notations, juxtaposed
with the corresponding XPath expressions. Most of the samples are
borrowed literally from the XPath specification.

Much of the following material is taken from the SXPath sources by Oleg
Kiselyov et al.

#### 7.21.6.2 Basic Converters and Applicators [¶](#Basic-Converters-and-Applicators)

A converter is a function mapping a nodeset (or a single node) to another
nodeset. Its type can be represented like this:

```
type Converter = Node|Nodeset -> Nodeset
```

A converter can also play the role of a predicate: in that case, if a
converter, applied to a node or a nodeset, yields a non-empty nodeset,
the converter-predicate is deemed satisfied. Likewise, an empty nodeset
is equivalent to `#f` in denoting failure.

Scheme Procedure: **nodeset?** x [¶](#index-nodeset_003f)
:   Return `#t` if x is a nodeset.

Scheme Procedure: **node-typeof?** crit [¶](#index-node_002dtypeof_003f)
:   This function implements a ’Node test’ as defined in Sec. 2.3 of the
    XPath document. A node test is one of the components of a location
    step. It is also a converter-predicate in SXPath.

    The function `node-typeof?` takes a type criterion and returns a
    function, which, when applied to a node, will tell if the node satisfies
    the test.

    The criterion crit is a symbol, one of the following:

    `id`
    :   tests if the node has the right name (id)

    `@`
    :   tests if the node is an <attributes-coll>

    `*`
    :   tests if the node is an <Element>

    `*text*`
    :   tests if the node is a text node

    `*PI*`
    :   tests if the node is a PI (processing instruction) node

    `*any*`
    :   `#t` for any type of node

Scheme Procedure: **node-eq?** other [¶](#index-node_002deq_003f)
:   A curried equivalence converter predicate that takes a node other
    and returns a function that takes another node. The two nodes are
    compared using `eq?`.

Scheme Procedure: **node-equal?** other [¶](#index-node_002dequal_003f)
:   A curried equivalence converter predicate that takes a node other
    and returns a function that takes another node. The two nodes are
    compared using `equal?`.

Scheme Procedure: **node-pos** n [¶](#index-node_002dpos)
:   Select the n’th element of a nodeset and return as a singular
    nodeset. If the n’th element does not exist, return an empty
    nodeset. If n is a negative number the node is picked from the
    tail of the list.

    ```
    ((node-pos 1) nodeset)  ; return the the head of the nodeset (if exists)
    ((node-pos 2) nodeset)  ; return the node after that (if exists)
    ((node-pos -1) nodeset) ; selects the last node of a non-empty nodeset
    ((node-pos -2) nodeset) ; selects the last but one node, if exists.
    ```

Scheme Procedure: **filter** pred? [¶](#index-filter-2)
:   A filter applicator, which introduces a filtering context. The argument
    converter pred? is considered a predicate, with either `#f`
    or `nil` meaning failure.

Scheme Procedure: **take-until** pred? [¶](#index-take_002duntil)
:   ```
    take-until:: Converter -> Converter, or
    take-until:: Pred -> Node|Nodeset -> Nodeset
    ```

    Given a converter-predicate pred? and a nodeset, apply the
    predicate to each element of the nodeset, until the predicate yields
    anything but `#f` or `nil`. Return the elements of the input
    nodeset that have been processed until that moment (that is, which fail
    the predicate).

    `take-until` is a variation of the `filter` above:
    `take-until` passes elements of an ordered input set up to (but not
    including) the first element that satisfies the predicate. The nodeset
    returned by `((take-until (not pred)) nset)` is a subset – to be
    more precise, a prefix – of the nodeset returned by `((filter
    pred) nset)`.

Scheme Procedure: **take-after** pred? [¶](#index-take_002dafter)
:   ```
    take-after:: Converter -> Converter, or
    take-after:: Pred -> Node|Nodeset -> Nodeset
    ```

    Given a converter-predicate pred? and a nodeset, apply the
    predicate to each element of the nodeset, until the predicate yields
    anything but `#f` or `nil`. Return the elements of the input
    nodeset that have not been processed: that is, return the elements of
    the input nodeset that follow the first element that satisfied the
    predicate.

    `take-after` along with `take-until` partition an input
    nodeset into three parts: the first element that satisfies a predicate,
    all preceding elements and all following elements.

Scheme Procedure: **map-union** proc lst [¶](#index-map_002dunion)
:   Apply proc to each element of lst and return the list of results.
    If proc returns a nodeset, splice it into the result

    From another point of view, `map-union` is a function
    `Converter->Converter`, which places an argument-converter in a joining
    context.

Scheme Procedure: **node-reverse** node-or-nodeset [¶](#index-node_002dreverse)
:   ```
    node-reverse :: Converter, or
    node-reverse:: Node|Nodeset -> Nodeset
    ```

    Reverses the order of nodes in the nodeset. This basic converter is
    needed to implement a reverse document order (see the XPath
    Recommendation).

Scheme Procedure: **node-trace** title [¶](#index-node_002dtrace)
:   ```
    node-trace:: String -> Converter
    ```

    `(node-trace title)` is an identity converter. In addition it
    prints out the node or nodeset it is applied to, prefixed with the
    title. This converter is very useful for debugging.

#### 7.21.6.3 Converter Combinators [¶](#Converter-Combinators)

Combinators are higher-order functions that transmogrify a converter or
glue a sequence of converters into a single, non-trivial converter. The
goal is to arrive at converters that correspond to XPath location paths.

From a different point of view, a combinator is a fixed, named
*pattern* of applying converters. Given below is a complete set of
such patterns that together implement XPath location path specification.
As it turns out, all these combinators can be built from a small number
of basic blocks: regular functional composition, `map-union` and
`filter` applicators, and the nodeset union.

Scheme Procedure: **select-kids** test-pred? [¶](#index-select_002dkids)
:   `select-kids` takes a converter (or a predicate) as an argument and
    returns another converter. The resulting converter applied to a nodeset
    returns an ordered subset of its children that satisfy the predicate
    test-pred?.

Scheme Procedure: **node-self** pred? [¶](#index-node_002dself)
:   Similar to `select-kids` except that the predicate pred? is
    applied to the node itself rather than to its children. The resulting
    nodeset will contain either one component, or will be empty if the node
    failed the predicate.

Scheme Procedure: **node-join** . selectors [¶](#index-node_002djoin)
:   ```
    node-join:: [LocPath] -> Node|Nodeset -> Nodeset, or
    node-join:: [Converter] -> Converter
    ```

    Join the sequence of location steps or paths as described above.

Scheme Procedure: **node-reduce** . converters [¶](#index-node_002dreduce)
:   ```
    node-reduce:: [LocPath] -> Node|Nodeset -> Nodeset, or
    node-reduce:: [Converter] -> Converter
    ```

    A regular functional composition of converters. From a different point
    of view, `((apply node-reduce converters) nodeset)` is equivalent
    to `(foldl apply nodeset converters)`, i.e., folding, or reducing,
    a list of converters with the nodeset as a seed.

Scheme Procedure: **node-or** . converters [¶](#index-node_002dor)
:   ```
    node-or:: [Converter] -> Converter
    ```

    This combinator applies all converters to a given node and produces the
    union of their results. This combinator corresponds to a union
    (`|` operation) for XPath location paths.

Scheme Procedure: **node-closure** test-pred? [¶](#index-node_002dclosure)
:   ```
    node-closure:: Converter -> Converter
    ```

    Select all *descendants* of a node that satisfy a
    converter-predicate test-pred?. This combinator is similar to
    `select-kids` but applies to grand... children as well. This
    combinator implements the `descendant::` XPath axis. Conceptually,
    this combinator can be expressed as

    ```
    (define (node-closure f)
      (node-or
        (select-kids f)
        (node-reduce (select-kids (node-typeof? '*)) (node-closure f))))
    ```

    This definition, as written, looks somewhat like a fixpoint, and it will
    run forever. It is obvious however that sooner or later
    `(select-kids (node-typeof? '*))` will return an empty nodeset. At
    this point further iterations will no longer affect the result and can
    be stopped.

Scheme Procedure: **node-parent** rootnode [¶](#index-node_002dparent)
:   ```
    node-parent:: RootNode -> Converter
    ```

    `(node-parent rootnode)` yields a converter that returns a parent
    of a node it is applied to. If applied to a nodeset, it returns the
    list of parents of nodes in the nodeset. The rootnode does not
    have to be the root node of the whole SXML tree – it may be a root node
    of a branch of interest.

    Given the notation of Philip Wadler’s paper on semantics of XSLT,

    ```
      parent(x) = { y | y=subnode*(root), x=subnode(y) }
    ```

    Therefore, `node-parent` is not the fundamental converter: it can
    be expressed through the existing ones. Yet `node-parent` is a
    rather convenient converter. It corresponds to a `parent::` axis
    of SXPath. Note that the `parent::` axis can be used with an
    attribute node as well.

Scheme Procedure: **sxpath** path [¶](#index-sxpath)
:   Evaluate an abbreviated SXPath.

    ```
    sxpath:: AbbrPath -> Converter, or
    sxpath:: AbbrPath -> Node|Nodeset -> Nodeset
    ```

    path is a list. It is translated to the full SXPath according to
    the following rewriting rules:

    ```
    (sxpath '())
    ⇒ (node-join)

    (sxpath '(path-component ...))
    ⇒ (node-join (sxpath1 path-component) (sxpath '(...)))

    (sxpath1 '//)
    ⇒ (node-or
       (node-self (node-typeof? '*any*))
       (node-closure (node-typeof? '*any*)))

    (sxpath1 '(equal? x))
    ⇒ (select-kids (node-equal? x))

    (sxpath1 '(eq? x))
    ⇒ (select-kids (node-eq? x))

    (sxpath1 ?symbol)
    ⇒ (select-kids (node-typeof? ?symbol)

    (sxpath1 procedure)
    ⇒ procedure

    (sxpath1 '(?symbol ...))
    ⇒ (sxpath1 '((?symbol) ...))

    (sxpath1 '(path reducer ...))
    ⇒ (node-reduce (sxpath path) (sxpathr reducer) ...)

    (sxpathr number)
    ⇒ (node-pos number)

    (sxpathr path-filter)
    ⇒ (filter (sxpath path-filter))
    ```

---

Next: [(sxml apply-templates)](#sxml-apply_002dtemplates), Previous: [SXPath](#SXPath), Up: [SXML](#SXML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.21.7 (sxml ssax input-parse) [¶](#g_t_0028sxml-ssax-input_002dparse_0029)

* [Overview](#Overview-3)
* [Usage](#Usage-3)

#### 7.21.7.1 Overview [¶](#Overview-3)

A simple lexer.

The procedures in this module surprisingly often suffice to parse an
input stream. They either skip, or build and return tokens, according to
inclusion or delimiting semantics. The list of characters to expect,
include, or to break at may vary from one invocation of a function to
another. This allows the functions to easily parse even
context-sensitive languages.

EOF is generally frowned on, and thrown up upon if encountered.
Exceptions are mentioned specifically. The list of expected characters
(characters to skip until, or break-characters) may include an EOF
"character", which is to be coded as the symbol, `*eof*`.

The input stream to parse is specified as a *port*, which is usually
the last (and optional) argument. It defaults to the current input port
if omitted.

If the parser encounters an error, it will throw an exception to the key
`parser-error`. The arguments will be of the form `(port
message specialising-msg*)`.

The first argument is a port, which typically points to the offending
character or its neighborhood. You can then use `port-column` and
`port-line` to query the current position. message is the
description of the error. Other arguments supply more details about the
problem.

#### 7.21.7.2 Usage [¶](#Usage-3)

Scheme Procedure: **peek-next-char** [port] [¶](#index-peek_002dnext_002dchar)

Scheme Procedure: **assert-curr-char** expected-chars comment [port] [¶](#index-assert_002dcurr_002dchar)

Scheme Procedure: **skip-until** arg [port] [¶](#index-skip_002duntil)

Scheme Procedure: **skip-while** skip-chars [port] [¶](#index-skip_002dwhile)

Scheme Procedure: **next-token** prefix-skipped-chars break-chars [comment] [port] [¶](#index-next_002dtoken)

Scheme Procedure: **next-token-of** incl-list/pred [port] [¶](#index-next_002dtoken_002dof)

Scheme Procedure: **read-text-line** [port] [¶](#index-read_002dtext_002dline)

Scheme Procedure: **read-string** n [port] [¶](#index-read_002dstring)

Scheme Procedure: **find-string-from-port?** \_ \_ . \_ [¶](#index-find_002dstring_002dfrom_002dport_003f)
:   Looks for str in <input-port>, optionally within the first
    max-no-char characters.

---

Previous: [(sxml ssax input-parse)](#sxml-ssax-input_002dparse), Up: [SXML](#SXML)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.21.8 (sxml apply-templates) [¶](#g_t_0028sxml-apply_002dtemplates_0029)

* [Overview](#Overview-4)
* [Usage](#Usage-4)

#### 7.21.8.1 Overview [¶](#Overview-4)

Pre-order traversal of a tree and creation of a new tree:

```
	apply-templates:: tree x <templates> -> <new-tree>
```

where

```
 <templates> ::= (<template> ...)
 <template>  ::= (<node-test> <node-test> ... <node-test> . <handler>)
 <node-test> ::= an argument to node-typeof? above
 <handler>   ::= <tree> -> <new-tree>
```

This procedure does a *normal*, pre-order traversal of an SXML
tree. It walks the tree, checking at each node against the list of
matching templates.

If the match is found (which must be unique, i.e., unambiguous), the
corresponding handler is invoked and given the current node as an
argument. The result from the handler, which must be a `<tree>`,
takes place of the current node in the resulting tree. The name of the
function is not accidental: it resembles rather closely an
`apply-templates` function of XSLT.

#### 7.21.8.2 Usage [¶](#Usage-4)

Scheme Procedure: **apply-templates** tree templates [¶](#index-apply_002dtemplates)

---

Previous: [SXML](#SXML), Up: [Guile Modules](#Guile-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
