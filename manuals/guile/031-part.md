Scheme Procedure: **test-runner-null** [¶](#index-test_002drunner_002dnull)
:   Creates a new test-runner, that does nothing with the test results.
    This is mainly meant for extending when writing a custom runner.

Scheme Procedure: **test-runner-create** [¶](#index-test_002drunner_002dcreate)
:   Create a new test-runner. Equivalent to ‘`((test-runner-factory))`’.

Scheme Parameter: **test-runner-factory** [¶](#index-test_002drunner_002dfactory)

Scheme Parameter: **test-runner-factory** factory [¶](#index-test_002drunner_002dfactory-1)
:   Get or set the current test-runner factory. A factory is a
    zero-argument function that creates a new test-runner. The default
    value is `test-runner-simple`.

#### Running specific tests with a specified runner [¶](#Running-specific-tests-with-a-specified-runner)

Scheme Procedure: **test-apply** [runner] specifier … procedure [¶](#index-test_002dapply)
:   Calls procedure with no arguments using the specified runner
    as the current test-runner. If runner is omitted, then
    `(test-runner-current)` is used. (If there is no current runner,
    one is created as in `test-begin`.) If one or more
    specifiers are listed then only tests matching the
    specifiers are executed. A specifier has the same form as
    one used for `test-skip`. A test is executed if it matches any of
    the specifiers in the `test-apply` *and* does not match
    any active `test-skip` specifiers.

Scheme Syntax: **test-with-runner** runner decl-or-expr … [¶](#index-test_002dwith_002drunner)
:   Executes each decl-or-expr in order in a context where the current
    test-runner is runner.

---

Next: [SRFI-64 Writing a New Test Runner](#SRFI_002d64-Writing-a-New-Test-Runner), Previous: [SRFI-64 Test Runner](#SRFI_002d64-Test-Runner), Up: [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.37.6 SRFI-64 Test Results [¶](#SRFI_002d64-Test-Results-1)

Running a test sets various status properties in the current test-runner.
This can be examined by a custom test-runner,
or (more rarely) in a test-suite.

#### Result Kind [¶](#Result-Kind)

Running a test may yield one of the following
status symbols:

`'pass`
:   The test passed, as expected.

`'fail`
:   The test failed (and was not expected to).

`'xfail`
:   The test failed and was expected to.

`'xpass`
:   The test passed, but was expected to fail.

`'skip`
:   The test was skipped.

Scheme Procedure: **test-result-kind** [runner] [¶](#index-test_002dresult_002dkind)
:   Returns one of the above result codes from the most recent tests.
    Returns `#f` if no tests have been run yet. If we’ve started on a
    new test, but don’t have a result yet, then the result kind is
    `'xfail` if the test is expected to fail, `'skip` if the test
    is supposed to be skipped, or `#f` otherwise.

Scheme Procedure: **test-passed?** [runner] [¶](#index-test_002dpassed_003f)
:   True if the value of ‘`(test-result-kind [runner])`’ is one of
    `'pass` or `'xpass`. This is a convenient shorthand that
    might be useful in a test suite to only run certain tests if the
    previous test passed.

#### Test result properties [¶](#Test-result-properties)

A test runner also maintains a set of more detailed
“result properties” associated with the current or most recent
test. (I.e. the properties of the most recent test are available as
long as a new test hasn’t started.) Each property has a name (a symbol)
and a value (any value). Some properties are standard or set by the
implementation; implementations can add more.

Scheme Procedure: **test-result-ref** runner pname [default] [¶](#index-test_002dresult_002dref)
:   Returns the property value associated with the pname property name
    (a symbol). If there is no value associated with pname return
    default, or `#f` if default isn’t specified.

Scheme Syntax: **test-result-set!** runner pname value [¶](#index-test_002dresult_002dset_0021)
:   Sets the property value associated with the pname property name to
    value. Usually implementation code should call this function, but
    it may be useful for a custom test-runner to add extra properties.

Scheme Procedure: **test-result-remove** runner pname [¶](#index-test_002dresult_002dremove)
:   Remove the property with the name pname.

Scheme Procedure: **test-result-clear** runner [¶](#index-test_002dresult_002dclear)
:   Remove all result properties. The implementation automatically calls
    `test-result-clear` at the start of a `test-assert` and
    similar procedures.

Scheme Procedure: **test-result-alist** runner [¶](#index-test_002dresult_002dalist)
:   Returns an association list of the current result properties. It is
    unspecified if the result shares state with the test-runner. The result
    should not be modified; on the other hand, the result may be implicitly
    modified by future `test-result-set!` or `test-result-remove`
    calls. However, a `test-result-clear` does not modify the returned
    alist. Thus you can “archive” result objects from previous runs.

#### Standard result properties [¶](#Standard-result-properties)

The set of available result properties is implementation-specific.
However, it is suggested that the following might be provided:

`'result-kind`
:   The result kind, as defined previously.
    This is the only mandatory result property.
    `(test-result-kind runner)` is equivalent to:
    `(test-result-ref runner 'result-kind)`

`'source-file`

`'source-line`
:   If known, the location of test statements (such as `test-assert`)
    in test suite source code.

`'source-form`
:   The source form, if meaningful and known.

`'expected-value`
:   The expected non-error result, if meaningful and known.

`'expected-error`
:   The error-typespecified in a `test-error`, if it meaningful and known.

`'actual-value`
:   The actual non-error result value, if meaningful and known.

`'actual-error`
:   The error value, if an error was signalled and it is known.
    The actual error value is implementation-defined.

---

Previous: [SRFI-64 Test Results](#SRFI_002d64-Test-Results), Up: [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.37.7 SRFI-64 Writing a New Test Runner [¶](#SRFI_002d64-Writing-a-New-Test-Runner-1)

This section specifies how to write a test-runner. It can be ignored if
you just want to write test-cases.

#### Call-back Functions [¶](#Call_002dback-Functions)

These call-back functions are “methods” (in the object-oriented sense)
of a test-runner. A method `test-runner-on-event` is called
by the implementation when event happens.

To define (set) the callback function for event use the following
expression. (This is normally done when initializing a test-runner.)

`(test-runner-on-event! runner event-function)`

An event-function takes a test-runner argument, and possibly other
arguments, depending on the event.

To extract (get) the callback function for event do this:
`(test-runner-on-event runner)`

To extract call the callback function for event use the following
expression. (This is normally done by the implementation core.)
‘`((test-runner-on-event runner) runner
other-args …)`’.

The following call-back hooks are available.

Scheme Procedure: **test-runner-on-test-begin** runner [¶](#index-test_002drunner_002don_002dtest_002dbegin)

Scheme Procedure: **test-runner-on-test-begin!** runner on-test-begin-function [¶](#index-test_002drunner_002don_002dtest_002dbegin_0021)

Scheme Procedure: **on-test-begin-function** runner [¶](#index-on_002dtest_002dbegin_002dfunction)
:   The on-test-begin-function is called at the start of an
    individual testcase, before the test expression (and expected value) are
    evaluated.

Scheme Procedure: **test-runner-on-test-end** runner [¶](#index-test_002drunner_002don_002dtest_002dend)

Scheme Procedure: **test-runner-on-test-end!** runner on-test-end-function [¶](#index-test_002drunner_002don_002dtest_002dend_0021)

Scheme Procedure: **on-test-end-function** runner [¶](#index-on_002dtest_002dend_002dfunction)
:   The on-test-end-function is called at the end of an
    individual testcase, when the result of the test is available.

Scheme Procedure: **test-runner-on-group-begin** runner [¶](#index-test_002drunner_002don_002dgroup_002dbegin)

Scheme Procedure: **test-runner-on-group-begin!** runner on-group-begin-function [¶](#index-test_002drunner_002don_002dgroup_002dbegin_0021)

Scheme Procedure: **on-group-begin-function** runner suite-name count [¶](#index-on_002dgroup_002dbegin_002dfunction)
:   The on-group-begin-function is called by a `test-begin`,
    including at the start of a `test-group`. The suite-name is
    a Scheme string, and count is an integer or `#f`.

Scheme Procedure: **test-runner-on-group-end** runner [¶](#index-test_002drunner_002don_002dgroup_002dend)

Scheme Procedure: **test-runner-on-group-end!** runner on-group-end-function [¶](#index-test_002drunner_002don_002dgroup_002dend_0021)

Scheme Procedure: **on-group-end-function** runner [¶](#index-on_002dgroup_002dend_002dfunction)
:   The on-group-end-function is called by a `test-end`,
    including at the end of a `test-group`.

Scheme Procedure: **test-runner-on-bad-count** runner [¶](#index-test_002drunner_002don_002dbad_002dcount)

Scheme Procedure: **test-runner-on-bad-count!** runner on-bad-count-function [¶](#index-test_002drunner_002don_002dbad_002dcount_0021)

Scheme Procedure: **on-bad-count-function** runner actual-count expected-count [¶](#index-on_002dbad_002dcount_002dfunction)
:   Called from `test-end` (before the on-group-end-function is
    called) if an expected-count was specified by the matching
    `test-begin` and the expected-count does not match the
    actual-count of tests actually executed or skipped.

Scheme Procedure: **test-runner-on-bad-end-name** runner [¶](#index-test_002drunner_002don_002dbad_002dend_002dname)

Scheme Procedure: **test-runner-on-bad-end-name!** runner on-bad-end-name-function [¶](#index-test_002drunner_002don_002dbad_002dend_002dname_0021)

Scheme Procedure: **on-bad-end-name-function** runner begin-name end-name [¶](#index-on_002dbad_002dend_002dname_002dfunction)
:   Called from `test-end` (before the on-group-end-function is
    called) if a suite-name was specified, and it did not that the
    name in the matching `test-begin`.

Scheme Procedure: **test-runner-on-final** runner [¶](#index-test_002drunner_002don_002dfinal)

Scheme Procedure: **test-runner-on-final!** runner on-final-function [¶](#index-test_002drunner_002don_002dfinal_0021)

Scheme Procedure: **on-final-function** runner [¶](#index-on_002dfinal_002dfunction)
:   The on-final-function takes one parameter (a test-runner) and
    typically displays a summary (count) of the tests. The
    on-final-function is called after called the
    on-group-end-function correspondiong to the outermost
    `test-end`. The default value is `test-on-final-simple` which
    writes to the standard output port the number of tests of the various
    kinds.

The default test-runner returned by `test-runner-simple` uses the
following call-back functions:

Scheme Procedure: **test-on-test-begin-simple** runner [¶](#index-test_002don_002dtest_002dbegin_002dsimple)

Scheme Procedure: **test-on-test-end-simple** runner [¶](#index-test_002don_002dtest_002dend_002dsimple)

Scheme Procedure: **test-on-group-begin-simple** runner suite-name count [¶](#index-test_002don_002dgroup_002dbegin_002dsimple)

Scheme Procedure: **test-on-group-end-simple** runner [¶](#index-test_002don_002dgroup_002dend_002dsimple)

Scheme Procedure: **test-on-bad-count-simple** runner actual-count expected-count [¶](#index-test_002don_002dbad_002dcount_002dsimple)

Scheme Procedure: **test-on-bad-end-name-simple** runner begin-name end-name [¶](#index-test_002don_002dbad_002dend_002dname_002dsimple)
:   You can call those if you want to write your own test-runner.

#### Test-runner components [¶](#Test_002drunner-components)

The following functions are for accessing the other components of a
test-runner. They would normally only be used to write a new
test-runner or a match-predicate.

Scheme Procedure: **test-runner-pass-count** runner [¶](#index-test_002drunner_002dpass_002dcount)
:   Returns the number of tests that passed, and were expected to pass.

Scheme Procedure: **test-runner-fail-count** runner [¶](#index-test_002drunner_002dfail_002dcount)
:   Returns the number of tests that failed, but were expected to pass.

Scheme Procedure: **test-runner-xpass-count** runner [¶](#index-test_002drunner_002dxpass_002dcount)
:   Returns the number of tests that passed, but were expected to fail.

Scheme Procedure: **test-runner-xfail-count** runner [¶](#index-test_002drunner_002dxfail_002dcount)
:   Returns the number of tests that failed, and were expected to pass.

Scheme Procedure: **test-runner-skip-count** runner [¶](#index-test_002drunner_002dskip_002dcount)
:   Returns the number of tests or test groups that were skipped.

Scheme Procedure: **test-runner-test-name** runner [¶](#index-test_002drunner_002dtest_002dname)
:   Returns the name of the current test or test group, as a string. During
    execution of `test-begin` this is the name of the test group;
    during the execution of an actual test, this is the name of the
    test-case. If no name was specified, the name is the empty string.

Scheme Procedure: **test-runner-group-path** runner [¶](#index-test_002drunner_002dgroup_002dpath)
:   A list of names of groups we’re nested in, with the outermost group
    first.

Scheme Procedure: **test-runner-group-stack** runner [¶](#index-test_002drunner_002dgroup_002dstack)
:   A list of names of groups we’re nested in, with the outermost group
    last. (This is more efficient than `test-runner-group-path`, since
    it doesn’t require any copying.)

Scheme Procedure: **test-runner-aux-value** runner [¶](#index-test_002drunner_002daux_002dvalue)

Scheme Procedure: **test-runner-aux-value!** runner on-test [¶](#index-test_002drunner_002daux_002dvalue_0021)
:   Get or set the `aux-value` field of a test-runner. This field is
    not used by this API or the `test-runner-simple` test-runner, but
    may be used by custom test-runners to store extra state.

Scheme Procedure: **test-runner-reset** runner [¶](#index-test_002drunner_002dreset)
:   Resets the state of the runner to its initial state.

#### Example [¶](#Example)

This is an example of a simple custom test-runner.
Loading this program before running a test-suite will install
it as the default test runner.

```
(define (my-simple-runner filename)
  (let ((runner (test-runner-null))
	(port (open-output-file filename))
        (num-passed 0)
        (num-failed 0))
    (test-runner-on-test-end! runner
      (lambda (runner)
        (case (test-result-kind runner)
          ((pass xpass) (set! num-passed (+ num-passed 1)))
          ((fail xfail) (set! num-failed (+ num-failed 1)))
          (else #t))))
    (test-runner-on-final! runner
       (lambda (runner)
          (format port "Passing tests: ~d.~%Failing tests: ~d.~%"
                  num-passed num-failed)
	  (close-output-port port)))
    runner))
(test-runner-factory
 (lambda () (my-simple-runner "/tmp/my-test.log")))
```

---

Next: [SRFI-69 - Basic hash tables](#SRFI_002d69), Previous: [SRFI-64: A Scheme API for Test Suites](#SRFI_002d64), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.38 SRFI-67 - Compare procedures [¶](#SRFI_002d67-_002d-Compare-procedures)

See [the
specification of SRFI-67](http://srfi.schemers.org/srfi-67/srfi-67.html).

---

Next: [SRFI-71 - Extended let-syntax for multiple values](#SRFI_002d71), Previous: [SRFI-67 - Compare procedures](#SRFI_002d67), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.39 SRFI-69 - Basic hash tables [¶](#SRFI_002d69-_002d-Basic-hash-tables)

This is a portable wrapper around Guile’s built-in hash table and weak
table support. See [Hash Tables](#Hash-Tables), for information on that built-in
support. Above that, this hash-table interface provides association
of equality and hash functions with tables at creation time, so
variants of each function are not required, as well as a procedure
that takes care of most uses for Guile hash table handles, which this
SRFI does not provide as such.

Access it with:

```
(use-modules (srfi srfi-69))
```

* [Creating hash tables](#SRFI_002d69-Creating-hash-tables)
* [Accessing table items](#SRFI_002d69-Accessing-table-items)
* [Table properties](#SRFI_002d69-Table-properties)
* [Hash table algorithms](#SRFI_002d69-Hash-table-algorithms)

---

Next: [Accessing table items](#SRFI_002d69-Accessing-table-items), Up: [SRFI-69 - Basic hash tables](#SRFI_002d69)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.39.1 Creating hash tables [¶](#Creating-hash-tables)

Scheme Procedure: **make-hash-table** [equal-proc hash-proc #:weak weakness start-size] [¶](#index-make_002dhash_002dtable-1)
:   Create and answer a new hash table with equal-proc as the
    equality function and hash-proc as the hashing function.

    By default, equal-proc is `equal?`. It can be any
    two-argument procedure, and should answer whether two keys are the
    same for this table’s purposes.

    By default hash-proc assumes that `equal-proc` is no
    coarser than `equal?` unless it is literally `string-ci=?`.
    If provided, hash-proc should be a two-argument procedure that
    takes a key and the current table size, and answers a reasonably good
    hash integer between 0 (inclusive) and the size (exclusive).

    weakness should be `#f` or a symbol indicating how “weak”
    the hash table is:

    `#f`
    :   An ordinary non-weak hash table. This is the default.

    `key`
    :   When the key has no more non-weak references at GC, remove that entry.

    `value`
    :   When the value has no more non-weak references at GC, remove that
        entry.

    `key-or-value`
    :   When either has no more non-weak references at GC, remove the
        association.

    As a legacy of the time when Guile couldn’t grow hash tables,
    start-size is an optional integer argument that specifies the
    approximate starting size for the hash table, which will be rounded to
    an algorithmically-sounder number.

By *coarser* than `equal?`, we mean that for all x and
y values where `(equal-proc x y)`,
`(equal? x y)` as well. If that does not hold for
your equal-proc, you must provide a hash-proc.

In the case of weak tables, remember that *references* above
always refers to `eq?`-wise references. Just because you have a
reference to some string `"foo"` doesn’t mean that an association
with key `"foo"` in a weak-key table *won’t* be collected;
it only counts as a reference if the two `"foo"`s are `eq?`,
regardless of equal-proc. As such, it is usually only sensible
to use `eq?` and `hashq` as the equivalence and hash
functions for a weak table. See [Weak References](#Weak-References), for more
information on Guile’s built-in weak table support.

Scheme Procedure: **alist->hash-table** alist [equal-proc hash-proc #:weak weakness start-size] [¶](#index-alist_002d_003ehash_002dtable-1)
:   As with `make-hash-table`, but initialize it with the
    associations in alist. Where keys are repeated in alist,
    the leftmost association takes precedence.

---

Next: [Table properties](#SRFI_002d69-Table-properties), Previous: [Creating hash tables](#SRFI_002d69-Creating-hash-tables), Up: [SRFI-69 - Basic hash tables](#SRFI_002d69)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.39.2 Accessing table items [¶](#Accessing-table-items)

Scheme Procedure: **hash-table-ref** table key [default-thunk] [¶](#index-hash_002dtable_002dref)

Scheme Procedure: **hash-table-ref/default** table key default [¶](#index-hash_002dtable_002dref_002fdefault)
:   Answer the value associated with key in table. If
    key is not present, answer the result of invoking the thunk
    default-thunk, which signals an error instead by default.

    `hash-table-ref/default` is a variant that requires a third
    argument, default, and answers default itself instead of
    invoking it.

Scheme Procedure: **hash-table-set!** table key new-value [¶](#index-hash_002dtable_002dset_0021)
:   Set key to new-value in table.

Scheme Procedure: **hash-table-delete!** table key [¶](#index-hash_002dtable_002ddelete_0021)
:   Remove the association of key in table, if present. If
    absent, do nothing.

Scheme Procedure: **hash-table-exists?** table key [¶](#index-hash_002dtable_002dexists_003f)
:   Answer whether key has an association in table.

Scheme Procedure: **hash-table-update!** table key modifier [default-thunk] [¶](#index-hash_002dtable_002dupdate_0021)

Scheme Procedure: **hash-table-update!/default** table key modifier default [¶](#index-hash_002dtable_002dupdate_0021_002fdefault)
:   Replace key’s associated value in table by invoking
    modifier with one argument, the old value.

    If key is not present, and default-thunk is provided,
    invoke it with no arguments to get the “old value” to be passed to
    modifier as above. If default-thunk is not provided in
    such a case, signal an error.

    `hash-table-update!/default` is a variant that requires the
    fourth argument, which is used directly as the “old value” rather
    than as a thunk to be invoked to retrieve the “old value”.

---

Next: [Hash table algorithms](#SRFI_002d69-Hash-table-algorithms), Previous: [Accessing table items](#SRFI_002d69-Accessing-table-items), Up: [SRFI-69 - Basic hash tables](#SRFI_002d69)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.39.3 Table properties [¶](#Table-properties)

Scheme Procedure: **hash-table-size** table [¶](#index-hash_002dtable_002dsize)
:   Answer the number of associations in table. This is guaranteed
    to run in constant time for non-weak tables.

Scheme Procedure: **hash-table-keys** table [¶](#index-hash_002dtable_002dkeys)
:   Answer an unordered list of the keys in table.

Scheme Procedure: **hash-table-values** table [¶](#index-hash_002dtable_002dvalues)
:   Answer an unordered list of the values in table.

Scheme Procedure: **hash-table-walk** table proc [¶](#index-hash_002dtable_002dwalk)
:   Invoke proc once for each association in table, passing
    the key and value as arguments.

Scheme Procedure: **hash-table-fold** table proc init [¶](#index-hash_002dtable_002dfold)
:   Invoke `(proc key value previous)` for
    each key and value in table, where previous is
    the result of the previous invocation, using init as the first
    previous value. Answer the final proc result.

Scheme Procedure: **hash-table->alist** table [¶](#index-hash_002dtable_002d_003ealist)
:   Answer an alist where each association in table is an
    association in the result.

---

Previous: [Table properties](#SRFI_002d69-Table-properties), Up: [SRFI-69 - Basic hash tables](#SRFI_002d69)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.39.4 Hash table algorithms [¶](#Hash-table-algorithms)

Each hash table carries an *equivalence function* and a *hash
function*, used to implement key lookups. Beginning users should
follow the rules for consistency of the default hash-proc
specified above. Advanced users can use these to implement their own
equivalence and hash functions for specialized lookup semantics.

Scheme Procedure: **hash-table-equivalence-function** hash-table [¶](#index-hash_002dtable_002dequivalence_002dfunction)

Scheme Procedure: **hash-table-hash-function** hash-table [¶](#index-hash_002dtable_002dhash_002dfunction)
:   Answer the equivalence and hash function of hash-table, respectively.

Scheme Procedure: **hash** obj [size] [¶](#index-hash-1)

Scheme Procedure: **string-hash** obj [size] [¶](#index-string_002dhash-1)

Scheme Procedure: **string-ci-hash** obj [size] [¶](#index-string_002dci_002dhash)

Scheme Procedure: **hash-by-identity** obj [size] [¶](#index-hash_002dby_002didentity)
:   Answer a hash value appropriate for equality predicate `equal?`,
    `string=?`, `string-ci=?`, and `eq?`, respectively.

`hash` is a backwards-compatible replacement for Guile’s built-in
`hash`.

---

Next: [SRFI-87 => in case clauses](#SRFI_002d87), Previous: [SRFI-69 - Basic hash tables](#SRFI_002d69), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.40 SRFI-71 - Extended let-syntax for multiple values [¶](#SRFI_002d71-_002d-Extended-let_002dsyntax-for-multiple-values)

This SRFI shadows the forms for `let`, `let*`, and `letrec`
so that they may accept multiple values. For example:

```
(use-modules (srfi srfi-71))

(let* ((x y (values 1 2))
       (z (+ x y)))
  (* z 2))
⇒ 6
```

See [the
specification of SRFI-71](http://srfi.schemers.org/srfi-71/srfi-71.html).

---

Next: [SRFI-88 Keyword Objects](#SRFI_002d88), Previous: [SRFI-71 - Extended let-syntax for multiple values](#SRFI_002d71), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.41 SRFI-87 => in case clauses [¶](#SRFI_002d87-_003d_003e-in-case-clauses)

Starting from version 2.0.6, Guile’s core `case` syntax supports
`=>` in clauses, as specified by SRFI-87/R7RS.
See [Simple Conditional Evaluation](#Conditionals).

---

Next: [SRFI-98 Accessing environment variables.](#SRFI_002d98), Previous: [SRFI-87 => in case clauses](#SRFI_002d87), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.42 SRFI-88 Keyword Objects [¶](#SRFI_002d88-Keyword-Objects)

[SRFI-88](http://srfi.schemers.org/srfi-88/srfi-88.html) provides
*keyword objects*, which are equivalent to Guile’s keywords
(see [Keywords](#Keywords)). SRFI-88 keywords can be entered using the
*postfix keyword syntax*, which consists of an identifier followed
by `:` (see [`postfix` keyword syntax](#Scheme-Read)).
SRFI-88 can be made available with:

```
(use-modules (srfi srfi-88))
```

Doing so installs the right reader option for keyword syntax, using
`(read-set! keywords 'postfix)`. It also provides the procedures
described below.

Scheme Procedure: **keyword?** obj [¶](#index-keyword_003f-1)
:   Return `#t` if obj is a keyword. This is the same procedure
    as the same-named built-in procedure (see [`keyword?`](#Keyword-Procedures)).

    ```
    (keyword? foo:)         ⇒ #t
    (keyword? 'foo:)        ⇒ #t
    (keyword? "foo")        ⇒ #f
    ```

Scheme Procedure: **keyword->string** kw [¶](#index-keyword_002d_003estring)
:   Return the name of kw as a string, i.e., without the trailing
    colon. The returned string may not be modified, e.g., with
    `string-set!`.

    ```
    (keyword->string foo:)  ⇒ "foo"
    ```

Scheme Procedure: **string->keyword** str [¶](#index-string_002d_003ekeyword)
:   Return the keyword object whose name is str.

    ```
    (keyword->string (string->keyword "a b c"))     ⇒ "a b c"
    ```

---

Next: [SRFI-105 Curly-infix expressions.](#SRFI_002d105), Previous: [SRFI-88 Keyword Objects](#SRFI_002d88), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.43 SRFI-98 Accessing environment variables. [¶](#SRFI_002d98-Accessing-environment-variables_002e)

This is a portable wrapper around Guile’s built-in support for
interacting with the current environment, See [Runtime Environment](#Runtime-Environment).

Scheme Procedure: **get-environment-variable** name [¶](#index-get_002denvironment_002dvariable)
:   Returns a string containing the value of the environment variable
    given by the string `name`, or `#f` if the named
    environment variable is not found. This is equivalent to
    `(getenv name)`.

Scheme Procedure: **get-environment-variables** [¶](#index-get_002denvironment_002dvariables)
:   Returns the names and values of all the environment variables as an
    association list in which both the keys and the values are strings.

---

Next: [SRFI-111 Boxes.](#SRFI_002d111), Previous: [SRFI-98 Accessing environment variables.](#SRFI_002d98), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.44 SRFI-105 Curly-infix expressions. [¶](#SRFI_002d105-Curly_002dinfix-expressions_002e)

Guile’s built-in reader includes support for SRFI-105 curly-infix
expressions. See [the specification of SRFI-105](http://srfi.schemers.org/srfi-105/srfi-105.html). Some examples:

```
{n <= 5}                ⇒  (<= n 5)
{a + b + c}             ⇒  (+ a b c)
{a * {b + c}}           ⇒  (* a (+ b c))
{(- a) / b}             ⇒  (/ (- a) b)
{-(a) / b}              ⇒  (/ (- a) b) as well
{(f a b) + (g h)}       ⇒  (+ (f a b) (g h))
{f(a b) + g(h)}         ⇒  (+ (f a b) (g h)) as well
{f[a b] + g(h)}         ⇒  (+ ($bracket-apply$ f a b) (g h))
'{a + f(b) + x}         ⇒  '(+ a (f b) x)
{length(x) >= 6}        ⇒  (>= (length x) 6)
{n-1 + n-2}             ⇒  (+ n-1 n-2)
{n * factorial{n - 1}}  ⇒  (* n (factorial (- n 1)))
{{a > 0} and {b >= 1}}  ⇒  (and (> a 0) (>= b 1))
{f{n - 1}(x)}           ⇒  ((f (- n 1)) x)
{a . z}                 ⇒  ($nfx$ a . z)
{a + b - c}             ⇒  ($nfx$ a + b - c)
```

To enable curly-infix expressions within a file, place the reader
directive `#!curly-infix` before the first use of curly-infix
notation. To globally enable curly-infix expressions in Guile’s reader,
set the `curly-infix` read option.

Guile also implements the following non-standard extension to SRFI-105:
if `curly-infix` is enabled and there is no other meaning assigned
to square brackets (i.e. the `square-brackets` read option is
turned off), then lists within square brackets are read as normal lists
but with the special symbol `$bracket-list$` added to the front.
To enable this combination of read options within a file, use the reader
directive `#!curly-infix-and-bracket-lists`. For example:

```
[a b]    ⇒  ($bracket-list$ a b)
[a . b]  ⇒  ($bracket-list$ a . b)
```

For more information on reader options, See [Reading Scheme Code](#Scheme-Read).

---

Next: [SRFI-119 Wisp: simpler indentation-sensitive Scheme.](#SRFI_002d119), Previous: [SRFI-105 Curly-infix expressions.](#SRFI_002d105), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.45 SRFI-111 Boxes. [¶](#SRFI_002d111-Boxes_002e)

[SRFI-111](http://srfi.schemers.org/srfi-111/srfi-111.html)
provides boxes: objects with a single mutable cell.

Scheme Procedure: **box** value [¶](#index-box)
:   Return a newly allocated box whose contents is initialized to
    value.

Scheme Procedure: **box?** obj [¶](#index-box_003f)
:   Return true if obj is a box, otherwise return false.

Scheme Procedure: **unbox** box [¶](#index-unbox)
:   Return the current contents of box.

Scheme Procedure: **set-box!** box value [¶](#index-set_002dbox_0021)
:   Set the contents of box to value.

---

Next: [Transducers](#SRFI_002d171), Previous: [SRFI-111 Boxes.](#SRFI_002d111), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.46 SRFI-119 Wisp: simpler indentation-sensitive Scheme. [¶](#SRFI_002d119-Wisp_003a-simpler-indentation_002dsensitive-Scheme_002e)

The languages shipped in Guile include SRFI-119, also referred to as
*Wisp* (for “Whitespace to Lisp”), an encoding of Scheme that
allows replacing parentheses with equivalent indentation and inline
colons. See
[the specification
of SRFI-119](http://srfi.schemers.org/srfi-119/srfi-119.html). Some examples:

```
display "Hello World!"         ⇒  (display "Hello World!")
```

```
define : factorial n           ⇒  (define (factorial n)
    if : zero? n               ⇒      (if (zero? n)
       . 1                     ⇒          1
       * n : factorial {n - 1} ⇒    (* n (factorial {n - 1}))))
```

To execute a file with Wisp code, select the language and filename
extension `.w` vie `guile --language=wisp -x .w`.

In files using Wisp, See [SRFI-105 Curly-infix expressions.](#SRFI_002d105) (Curly Infix) is always activated.

---

Next: [SRFI-197: Pipeline Operators](#SRFI_002d197), Previous: [SRFI-119 Wisp: simpler indentation-sensitive Scheme.](#SRFI_002d119), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.47 Transducers [¶](#Transducers)

Some of the most common operations used in the Scheme language are those
transforming lists: map, filter, take and so on. They work well, are well
understood, and are used daily by most Scheme programmers. They are however not
general because they only work on lists, and they do not compose very well
since combining N of them builds `(- N 1)` intermediate lists.

Transducers are oblivious to what kind of process they are used in, and
are composable without building intermediate collections. This means we
can create a transducer that squares all odd numbers:

```
(compose (tfilter odd?) (tmap (lambda (x) (* x x))))
```

and reuse it with lists, vectors, or in just about any context where
data flows in one direction. We could use it as a processing step for
asynchronous channels, with an event framework as a pre-processing step,
or even in lazy contexts where you pass a lazy collection and a
transducer to a function and get a new lazy collection back.

The traditional Scheme approach of having collection-specific procedures
is not changed. We instead specify a general form of transformations
that complement these procedures. The benefits are obvious: a clear,
well-understood way of describing common transformations in a way that
is faster than just chaining the collection-specific counterparts. For
guile in particular this means a lot better GC performance.

Notice however that `(compose …)` composes transducers
left-to-right, due to how transducers are initiated.

* [SRFI-171 General Discussion](#SRFI_002d171-General-Discussion)
* [Applying Transducers](#SRFI_002d171-Applying-Transducers)
* [Reducers](#SRFI_002d171-Reducers)
* [Transducers](#SRFI_002d171-Transducers)
* [Helper functions for writing transducers](#SRFI_002d171-Helpers)

---

Next: [Applying Transducers](#SRFI_002d171-Applying-Transducers), Up: [Transducers](#SRFI_002d171)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.47.1 SRFI-171 General Discussion [¶](#SRFI_002d171-General-Discussion-1)

#### The concept of reducers [¶](#The-concept-of-reducers)

The central part of transducers are 3-arity reducing procedures.

* no arguments: Produces the identity of the reducer.
* (result-so-far): completion. Returns `result-so-far` either with or
  without transforming it first.
* (result-so-far input) combines `result-so-far` and `input` to produce
  a new `result-so-far`.

In the case of a summing `+` reducer, the reducer would produce, in
arity order: `0`, `result-so-far`, `(+ result-so-far
input)`. This happens to be exactly what the regular `+` does.

#### The concept of transducers [¶](#The-concept-of-transducers)

A transducer is a one-arity procedure that takes a reducer and produces a
reducing function that behaves as follows:

* no arguments: calls reducer with no arguments (producing its identity)
* (result-so-far): Maybe transform the result-so-far and call reducer with it.
* (result-so-far input) Maybe do something to input and maybe call the
  reducer with result-so-far and the maybe-transformed input.

A simple example is as following:

```
(list-transduce (tfilter odd?) + '(1 2 3 4 5))
```

This first returns a transducer filtering all odd
elements, then it runs `+` without arguments to retrieve its
identity. It then starts the transduction by passing `+` to the
transducer returned by `(tfilter odd?)` which returns a reducing
function. It works not unlike reduce from SRFI 1, but also checks
whether one of the intermediate transducers returns a "reduced" value
(implemented as a SRFI 9 record), which means the reduction finished
early.

Because transducers compose and the final reduction is only executed in
the last step, composed transducers will not build any intermediate
result or collections. Although the normal way of thinking about
application of composed functions is right to left, due to how the
transduction is built it is applied left to right. `(compose
(tfilter odd?) (tmap sqrt))` will create a transducer that first filters
out any odd values and then computes the square root of the rest.

#### State [¶](#State)

Even though transducers appear to be somewhat of a generalisation of
`map` and friends, this is not really true. Since transducers don’t
know in which context they are being used, some transducers must keep
state where their collection-specific counterparts do not. The
transducers that keep state do so using hidden mutable state, and as
such all the caveats of mutation, parallelism, and multi-shot
continuations apply. Each transducer keeping state is clearly described
as doing so in the documentation.

#### Naming [¶](#Naming)

Reducers exported from the transducers module are named as in their
SRFI-1 counterpart, but prepended with an r. Transducers also follow
that naming, but are prepended with a t.

---

Next: [Reducers](#SRFI_002d171-Reducers), Previous: [SRFI-171 General Discussion](#SRFI_002d171-General-Discussion), Up: [Transducers](#SRFI_002d171)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.47.2 Applying Transducers [¶](#Applying-Transducers)

Scheme Procedure: **list-transduce** xform f lst [¶](#index-list_002dtransduce)

Scheme Procedure: **list-transduce** xform f identity lst [¶](#index-list_002dtransduce-1)
:   Initialize the transducer xform by passing the reducer f
    to it. If no identity is provided, f runs without arguments to
    return the reducer identity. It then reduces over lst using the
    identity as the seed.

    If one of the transducers finishes early (such as `ttake` or
    `tdrop`), it communicates this by returning a reduced value, which
    in the guile implementation is just a value wrapped in a SRFI 9 record
    type named “reduced”. If such a value is returned by the transducer,
    `list-transduce` must stop execution and return an unreduced value
    immediately.

Scheme Procedure: **vector-transduce** xform f vec [¶](#index-vector_002dtransduce)

Scheme Procedure: **vector-transduce** xform f identity vec [¶](#index-vector_002dtransduce-1)

Scheme Procedure: **string-transduce** xform f str [¶](#index-string_002dtransduce)

Scheme Procedure: **string-transduce** xform f identity str [¶](#index-string_002dtransduce-1)

Scheme Procedure: **bytevector-u8-transduce** xform f bv [¶](#index-bytevector_002du8_002dtransduce)

Scheme Procedure: **bytevector-u8-transduce** xform f identity bv [¶](#index-bytevector_002du8_002dtransduce-1)

Scheme Procedure: **generator-transduce** xform f gen [¶](#index-generator_002dtransduce)

Scheme Procedure: **generator-transduce** xform f identity gen [¶](#index-generator_002dtransduce-1)
:   Same as `list-transduce`, but for vectors, strings, u8-bytevectors
    and SRFI-158-styled generators respectively.

Scheme Procedure: **port-transduce** xform f reader [¶](#index-port_002dtransduce)

Scheme Procedure: **port-transduce** xform f reader port [¶](#index-port_002dtransduce-1)

Scheme Procedure: **port-transduce** xform f identity reader port [¶](#index-port_002dtransduce-2)
:   Same as `list-transduce` but for ports. Called without a port, it
    reduces over the results of applying reader until the EOF-object
    is returned, presumably to read from `current-input-port`. With a
    port reader is applied to port instead of without any
    arguments. If identity is provided, that is used as the initial
    identity in the reduction.

---

Next: [Transducers](#SRFI_002d171-Transducers), Previous: [Applying Transducers](#SRFI_002d171-Applying-Transducers), Up: [Transducers](#SRFI_002d171)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.47.3 Reducers [¶](#Reducers)

Scheme Procedure: **rcons** [¶](#index-rcons)
:   a simple consing reducer. When called without values, it returns its
    identity, `'()`. With one value, which will be a list, it reverses
    the list (using `reverse!`). When called with two values, it conses
    the second value to the first.

    ```
    (list-transduce (tmap (lambda (x) (+ x 1)) rcons (list 0 1 2 3))
    ⇒ (1 2 3 4)
    ```

Scheme Procedure: **reverse-rcons** [¶](#index-reverse_002drcons)
:   same as rcons, but leaves the values in their reversed order.

    ```
    (list-transduce (tmap (lambda (x) (+ x 1))) reverse-rcons (list 0 1 2 3))
    ⇒ (4 3 2 1)
    ```

Scheme Procedure: **rany** pred? [¶](#index-rany)
:   The reducer version of any. Returns `(reduced (pred? value))` if
    any `(pred? value)` returns non-#f. The identity is #f.

    ```
    (list-transduce (tmap (lambda (x) (+ x 1))) (rany odd?) (list 1 3 5))
    ⇒ #f

    (list-transduce (tmap (lambda (x) (+ x 1))) (rany odd?) (list 1 3 4 5))
    ⇒ #t
    ```

Scheme Procedure: **revery** pred? [¶](#index-revery)
:   The reducer version of every. Stops the transduction and returns
    `(reduced #f)` if any `(pred? value)` returns #f. If every
    `(pred? value)` returns true, it returns the result of the last
    invocation of `(pred? value)`. The identity is #t.

    ```
    (list-transduce
      (tmap (lambda (x) (+ x 1)))
      (revery (lambda (v) (if (odd? v) v #f)))
      (list 2 4 6))
      ⇒ 7

    (list-transduce (tmap (lambda (x) (+ x 1)) (revery odd?) (list 2 4 5 6))
    ⇒ #f
    ```

Scheme Procedure: **rcount** [¶](#index-rcount)
:   A simple counting reducer. Counts the values that pass through the
    transduction.

    ```
    (list-transduce (tfilter odd?) rcount (list 1 2 3 4)) ⇒ 2.
    ```

---

Next: [Helper functions for writing transducers](#SRFI_002d171-Helpers), Previous: [Reducers](#SRFI_002d171-Reducers), Up: [Transducers](#SRFI_002d171)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.47.4 Transducers [¶](#Transducers-1)

Scheme Procedure: **tmap** proc [¶](#index-tmap)
:   Returns a transducer that applies proc to all values. Stateless.

Scheme Procedure: **tfilter** pred? [¶](#index-tfilter)
:   Returns a transducer that removes values for which pred? returns #f.

    Stateless.

Scheme Procedure: **tremove** pred? [¶](#index-tremove)
:   Returns a transducer that removes values for which pred? returns non-#f.

    Stateless

Scheme Procedure: **tfilter-map** proc [¶](#index-tfilter_002dmap)
:   The same as `(compose (tmap proc) (tfilter values))`. Stateless.

Scheme Procedure: **treplace** mapping [¶](#index-treplace)
:   The argument mapping is an association list (using `equal?`
    to compare keys), a hash-table, a one-argument procedure taking one
    argument and either producing that same argument or a replacement value.

    Returns a transducer which checks for the presence of any value passed
    through it in mapping. If a mapping is found, the value of that mapping
    is returned, otherwise it just returns the original value.

    Does not keep internal state, but modifying the mapping while it’s in
    use by treplace is an error.

Scheme Procedure: **tdrop** n [¶](#index-tdrop)
:   Returns a transducer that discards the first n values.

    Stateful.

Scheme Procedure: **ttake** n [¶](#index-ttake)
:   Returns a transducer that discards all values and stops the transduction
    after the first n values have been let through. Any subsequent values
    are ignored.

    Stateful.

Scheme Procedure: **tdrop-while** pred? [¶](#index-tdrop_002dwhile)
:   Returns a transducer that discards the first values for which
    pred? returns true.

    Stateful.

Scheme Procedure: **ttake-while** pred? [¶](#index-ttake_002dwhile)

Scheme Procedure: **ttake-while** pred? retf [¶](#index-ttake_002dwhile-1)
:   Returns a transducer that stops the transduction after pred? has
    returned #f. Any subsequent values are ignored and the last successful
    value is returned. retf is a function that gets called whenever
    pred? returns false. The arguments passed are the result so far
    and the input for which pred? returns `#f`. The default function is
    `(lambda (result input) result)`.

    Stateful.

Scheme Procedure: **tconcatenate** [¶](#index-tconcatenate)
:   tconcatenate *is* a transducer that concatenates the content of
    each value (that must be a list) into the reduction.

    ```
    (list-transduce tconcatenate rcons '((1 2) (3 4 5) (6 (7 8) 9)))
    ⇒ (1 2 3 4 5 6 (7 8) 9)
    ```

Scheme Procedure: **tappend-map** proc [¶](#index-tappend_002dmap)
:   The same as `(compose (tmap proc) tconcatenate)`.

Scheme Procedure: **tflatten** [¶](#index-tflatten)
:   tflatten *is* a transducer that flattens an input consisting of lists.

    ```
    (list-transduce tflatten rcons '((1 2) 3 (4 (5 6) 7 8) 9)
    ⇒ (1 2 3 4 5 6 7 8 9)
    ```

Scheme Procedure: **tdelete-neighbor-duplicates** [¶](#index-tdelete_002dneighbor_002dduplicates)

Scheme Procedure: **tdelete-neighbor-duplicates** equality-predicate [¶](#index-tdelete_002dneighbor_002dduplicates-1)
:   Returns a transducer that removes any directly following duplicate
    elements. The default equality-predicate is `equal?`.

    Stateful.

Scheme Procedure: **tdelete-duplicates** [¶](#index-tdelete_002dduplicates)

Scheme Procedure: **tdelete-duplicates** equality-predicate [¶](#index-tdelete_002dduplicates-1)
:   Returns a transducer that removes any subsequent duplicate elements
    compared using equality-predicate. The default
    equality-predicate is `equal?`.

    Stateful.

Scheme Procedure: **tsegment** n [¶](#index-tsegment)
:   Returns a transducer that groups inputs into lists of n elements.
    When the transduction stops, it flushes any remaining collection, even
    if it contains fewer than n elements.

    Stateful.

Scheme Procedure: **tpartition** pred? [¶](#index-tpartition)
:   Returns a transducer that groups inputs in lists by whenever
    `(pred? input)` changes value.

    Stateful.

Scheme Procedure: **tadd-between** value [¶](#index-tadd_002dbetween)
:   Returns a transducer which interposes value between each value
    and the next. This does not compose gracefully with transducers like
    `ttake`, as you might end up ending the transduction on
    `value`.

    Stateful.

Scheme Procedure: **tenumerate** [¶](#index-tenumerate)

Scheme Procedure: **tenumerate** start [¶](#index-tenumerate-1)
:   Returns a transducer that indexes values passed through it, starting at
    start, which defaults to 0. The indexing is done through cons
    pairs like `(index . input)`.

    ```
    (list-transduce (tenumerate 1) rcons (list 'first 'second 'third))
    ⇒ ((1 . first) (2 . second) (3 . third))
    ```

    Stateful.

Scheme Procedure: **tlog** [¶](#index-tlog)

Scheme Procedure: **tlog** logger [¶](#index-tlog-1)
:   Returns a transducer that can be used to log or print values and
    results. The result of the logger procedure is discarded. The
    default logger is `(lambda (result input) (write input)
    (newline))`.

    Stateless.

#### Guile-specific transducers [¶](#Guile_002dspecific-transducers)

These transducers are available in the `(srfi srfi-171 gnu)`
library, and are provided outside the standard described by the SRFI-171
document.

Scheme Procedure: **tbatch** reducer [¶](#index-tbatch)

Scheme Procedure: **tbatch** transducer reducer [¶](#index-tbatch-1)
:   A batching transducer that accumulates results using reducer or
    `((transducer) reducer)` until it returns a reduced value. This can
    be used to generalize something like `tsegment`:

    ```
    ;; This behaves exactly like (tsegment 4).
    (list-transduce (tbatch (ttake 4) rcons) rcons (iota 10))
    ⇒ ((0 1 2 3) (4 5 6 7) (8 9))
    ```

Scheme Procedure: **tfold** reducer [¶](#index-tfold)

Scheme Procedure: **tfold** reducer seed [¶](#index-tfold-1)
:   A folding transducer that yields the result of `(reducer seed
    value)`, saving its result between iterations.

    ```
    (list-transduce (tfold +) rcons (iota 10))
    ⇒ (0 1 3 6 10 15 21 28 36 45)
    ```

---

Previous: [Transducers](#SRFI_002d171-Transducers), Up: [Transducers](#SRFI_002d171)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.47.5 Helper functions for writing transducers [¶](#Helper-functions-for-writing-transducers)

These functions are in the `(srfi srfi-171 meta)` module and are only
usable when you want to write your own transducers.

Scheme Procedure: **reduced** value [¶](#index-reduced)
:   Wraps a value in a `<reduced>` container, signaling that the
    reduction should stop.

Scheme Procedure: **reduced?** value [¶](#index-reduced_003f)
:   Returns #t if value is a `<reduced>` record.

Scheme Procedure: **unreduce** reduced-container [¶](#index-unreduce)
:   Returns the value in reduced-container.

Scheme Procedure: **ensure-reduced** value [¶](#index-ensure_002dreduced)
:   Wraps value in a `<reduced>` container if it is not already reduced.

Scheme Procedure: **preserving-reduced** reducer [¶](#index-preserving_002dreduced)
:   Wraps `reducer` in another reducer that encapsulates any returned
    reduced value in another reduced container. This is useful in places
    where you re-use a reducer with [collection]-reduce. If the reducer
    returns a reduced value, [collection]-reduce unwraps it. Unless handled,
    this leads to the reduction continuing.

Scheme Procedure: **list-reduce** f identity lst [¶](#index-list_002dreduce)
:   The reducing function used internally by `list-transduce`. f
    is a reducer as returned by a transducer. identity is the
    identity (sometimes called "seed") of the reduction. lst is a
    list. If f returns a reduced value, the reduction stops
    immediately and the unreduced value is returned.

Scheme Procedure: **vector-reduce** f identity vec [¶](#index-vector_002dreduce)
:   The vector version of list-reduce.

Scheme Procedure: **string-reduce** f identity str [¶](#index-string_002dreduce)
:   The string version of list-reduce.

Scheme Procedure: **bytevector-u8-reduce** f identity bv [¶](#index-bytevector_002du8_002dreduce)
:   The bytevector-u8 version of list-reduce.

Scheme Procedure: **port-reduce** f identity reader port [¶](#index-port_002dreduce)
:   The port version of list-reduce. It reduces over port using reader
    until reader returns the EOF object.

Scheme Procedure: **generator-reduce** f identity gen [¶](#index-generator_002dreduce)
:   The generator version of list-reduce. It reduces over `gen` until
    it returns the EOF object

---

Next: [SRFI-207 String-notated bytevectors](#SRFI_002d207), Previous: [Transducers](#SRFI_002d171), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.48 SRFI-197: Pipeline Operators [¶](#SRFI_002d197_003a-Pipeline-Operators)

[SRFI-197](http://srfi.schemers.org/srfi-197/srfi-197.html)
provides functional pipeline (threading) operators like Clojure’s
`->` or OCaml’s `|>`. Pipelines are a simple, terse, and
readable way to write deeply-nested expressions. This SRFI defines a
family of chain and nest pipeline operators, which can rewrite nested
expressions like `(a b (c d (e f g)))` as a sequence of operations:
`(chain g (e f _) (c d _) (a b _))`.

Note that Like `let*`, `chain` guarantees evaluation order. In
fact, `(chain a (b _) (c _))` expands to something like `(let*
((x (b a)) (x (c x))) x)`, not `(c (b a))`, and so `chain` is
not suitable for pipelines containing syntax like `if` or
`let`.

For pipelines containing complex syntax, the `nest` and
`nest-reverse` operators look like `chain` but are guaranteed
to expand to nested forms, not `let*` forms. `nest` nests in
the opposite direction of `chain`, so `(nest (a _) (b _) c)`
expands to `(a (b c))`.

Scheme Syntax: **chain** initial-value [placeholder [ellipsis]] step … [¶](#index-chain)
:   Converts earch part into a sequence of small integers and returns
    a bytevector of the corresponding bytes as follows:

    initial-value is an expression.

    placeholder and ellipsis are literal symbols; these are the
    placeholder symbol and ellipsis symbol. If placeholder or
    ellipsis are not present, they default to `_` and `...`,
    respectively.

    The syntax of step is (datum …), where each datum is
    either the placeholder symbol, the ellipsis symbol, or an expression. A
    step must contain at least one datum. The ellipsis symbol is
    only allowed at the end of a step, and it must immediately follow
    a placeholder symbol.

    Semantics: `chain` evaluates each step in order from left to right,
    passing the result of each step to the next.

    Each step is evaluated as an application, and the return value(s)
    of that application are passed to the next step as its pipeline
    values. initial-value is the pipeline value of the first step. The
    return value(s) of `chain` are the return value(s) of the last step.

    The placeholder symbols in each step are replaced with that step’s
    pipeline values, in the order they appear. It is an error if the number
    of placeholders for a step does not equal the number of pipeline values
    for that step, unless the step contains no placeholders, in which case
    it will ignore its pipeline values.

    ```
    (chain x (a b _))
      ⇒ (a b x)
    (chain (a b) (c _ d) (e f _))
      ⇒ (let* ((x (a b)) (x (c x d))) (e f x))
    (chain (a) (b _ _) (c _))
      ⇒ (let*-values (((x1 x2) (a)) ((x) (b x1 x2))) (c x))
    ```

    If a step ends with a placeholder symbol followed by an ellipsis
    symbol, that placeholder sequence is replaced with all remaining
    pipeline values that do not have a matching placeholder.

    ```
    (chain (a) (b _ c _ ...) (d _))
      ⇒ (let*-values (((x1 . x2) (a)) ((x) (apply b x1 c x2))) (d x))
    ```

    `chain` and all other SRFI 197 macros support custom placeholder
    symbols, which can help to preserve hygiene when used in the body of a
    syntax definition that may insert a `_` or `....`

    ```
    (chain (a b) <> (c <> d) (e f <>))
      ⇒ (let* ((x (a b)) (x (c x d))) (e f x))
    (chain (a) - --- (b - c - ---) (d -))
      ⇒ (let*-values (((x1 . x2) (a)) ((x) (apply b x1 c x2))) (d x))
    ```

Scheme Syntax: **chain-and** initial-value [placeholder] step … [¶](#index-chain_002dand)
:   initial-value is an expression. placeholder is a literal
    symbol; this is the placeholder symbol. If placeholder is not
    given, the placeholder symbol is `_`. The syntax of step is
    (datum … [placeholder datum …]).

    Semantics: A variant of `chain` that short-circuits and returns
    `#f` if any step returns `#f`. `chain-and` is to
    `chain` as SRFI 2 `and-let*` is to `let*`.

    Each step is evaluated as an application. If the step evaluates to
    `#f`, the remaining steps are not evaluated, and `chain-and`
    returns #f. Otherwise, the return value of the step is passed to the
    next step as its pipeline value. initial-value is the pipeline
    value of the first step. If no step evaluates to `#f`, the return
    value of `chain-and` is the return value of the last step.

    The placeholder placeholder in each step is replaced with
    that step’s pipeline value. If a step does not contain
    placeholder, it will ignore its pipeline value, but
    `chain-and` will still check whether that pipeline value is
    `#f`.

    Because `chain-and` checks the return value of each step, it does
    not support steps with multiple return values. It is an error if a step
    returns more than one value.

Scheme Syntax: **chain-when** initial-value [placeholder] ([guard] step) … [¶](#index-chain_002dwhen)
:   initial-value and guard are expressions. placeholder
    is a literal symbol; this is the placeholder symbol. If
    placeholder is not present, the placeholder symbol is
    `_`. The syntax of step is (datum …
    [placeholder datum …]).

    Semantics: A variant of `chain` in which each step has a guard
    expression and will be skipped if the guard expression evaluates to #f.

    ```
    (define (describe-number n)
      (chain-when '()
        ((odd? n) (cons "odd" _))
        ((even? n) (cons "even" _))
        ((zero? n) (cons "zero" _))
        ((positive? n) (cons "positive" _))))

    (describe-number 3) ; => '("positive" "odd")
    (describe-number 4) ; => '("positive" "even")
    ```

    Each step is evaluated as an application. The return value of the
    step is passed to the next step as its pipeline
    value. initial-value is the pipeline value of the first step.

    The placeholder placeholder in each step is replaced with
    that step’s pipeline value. If a step does not contain
    placeholder, it will ignore its pipeline value.

    If a step’s guard is present and evaluates to `#f`, that step
    will be skipped, and its pipeline value will be reused as the pipeline
    value of the next step. The return value of `chain-when` is the
    return value of the last non-skipped step, or initial-value if all
    steps are skipped.

    Because `chain-when` may skip steps, it does not support steps with
    multiple return values. It is an error if a step returns more than one
    value.

Scheme Syntax: **chain-lambda** [placeholder [ellipsis]] step … [¶](#index-chain_002dlambda)
:   placeholder and ellipsis are literal symbols; these are the
    placeholder symbol and ellipsis symbol. If placeholder or
    ellipsis are not present, they default to `_` and `...`,
    respectively.

    The syntax of step is (datum …), where each
    datum is either the placeholder symbol, the ellipsis symbol, or an
    expression. A step must contain at least one datum. The
    ellipsis symbol is only allowed at the end of a step, and it must
    immediately follow a placeholder symbol.

    Semantics: Creates a procedure from a sequence of `chain`
    steps. When called, a `chain-lambda` procedure evaluates each
    step in order from left to right, passing the result of each step
    to the next.

    ```
    (chain-lambda (a _) (b _))
      ⇒ (lambda (x) (let* ((x (a x))) (b x)))
    (chain-lambda (a _ _) (b c _))
      ⇒ (lambda (x1 x2) (let* ((x (a x1 x2))) (b c x)))
    ```

    Each step is evaluated as an application, and the return value(s)
    of that application are passed to the next step as its pipeline
    values. The procedure’s arguments are the pipeline values of the first
    step. The return value(s) of the procedure are the return value(s) of
    the last step.

    The placeholder symbols in each step are replaced with that step’s
    pipeline values, in the order they appear. It is an error if the number
    of placeholders for a step does not equal the number of pipeline values
    for that step, unless the step contains no placeholders, in which case
    it will ignore its pipeline values.

    If a step ends with a placeholder symbol followed by an ellipsis
    symbol, that placeholder sequence is replaced with all remaining
    pipeline values that do not have a matching placeholder.

    The number of placeholders in the first step determines the arity
    of the procedure. If the first step ends with an ellipsis symbol, the
    procedure is variadic.

Scheme Syntax: **nest** [placeholder] <step> … initial-value [¶](#index-nest)
:   placeholder is a literal symbol; this is the placeholder
    symbol. If placeholder is not present, the placeholder symbol is
    `_`. The syntax of step is (datum …
    placeholder datum …). initial-value is an
    expression.

    Semantics: `nest` is similar to `chain`, but sequences its
    steps in the opposite order. Unlike `chain`, `nest` literally
    nests expressions; as a result, it does not provide the same strict
    evaluation order guarantees as `chain`.

    ```
    (nest (a b _) (c d _) e) ; => (a b (c d e))
    ```

    A `nest` expression is evaluated by lexically replacing the
    placeholder in the last step with initial-value, then
    replacing the placeholder in the next-to-last step with that
    replacement, and so on until the placeholder in the first
    step has been replaced. It is an error if the resulting final
    replacement is not an expression, which is then evaluated and its values
    are returned.

    Because it produces an actual nested form, `nest` can build
    expressions that `chain` cannot. For example, `nest` can build
    a quoted data structure:

    ```
    (nest '_ (1 2 _) (3 _ 5) (_) 4) ; => '(1 2 (3 (4) 5))
    ```

    `nest` can also safely include special forms like if, let, lambda, or parameterize in a pipeline.

    A custom placeholder can be used to safely nest `nest` expressions.

    ```
    (nest (nest _2 '_2 (1 2 3 _2) _ 6)
          (_ 5 _2)
          4)
      ⇒ '(1 2 3 (4 5 6))
    ```

Scheme Syntax: **nest-reverse** initial-value [placeholder] step … [¶](#index-nest_002dreverse)
:   Syntax: initial-value is an expression. placeholder is a
    literal symbol; this is the placeholder symbol. If placeholder is
    not present, the placeholder symbol is `_`. The syntax of
    step is (datum … placeholder datum
    …).

    Semantics: `nest-reverse` is variant of `nest` that nests in
    reverse order, which is the same order as `chain`.

    ```
    (nest-reverse e (c d _) (a b _)) ; => (a b (c d e))
    ```

    A `nest-reverse` expression is evaluated by lexically replacing the
    placeholder in the first step with initial-value, then
    replacing the placeholder in the second step with that
    replacement, and so on until the placeholder in the last
    step has been replaced. It is an error if the resulting final
    replacement is not an expression, which is then evaluated and its values
    are returned.

* [Acknowledgements](#Acknowledgements)

#### 7.5.48.1 Acknowledgements [¶](#Acknowledgements)

Thanks to the participants in the SRFI 197 mailing list who helped Adam
Nelson refine this SRFI, including Marc Nieper-Wißkirchen, Linus
Björnstam, Shiro Kawai, Lassi Kortela, and John Cowan.

Marc provided a paragraph that has been included (with only minor
changes) in the Semantics section of the nest and nest-reverse macros.

Thanks to Rich Hickey for [Clojure](https://clojure.org) and the
original implementation of Clojure threading macros, and to Paulus
Esterhazy for the (EPL licensed) threading macros documentation page,
which was a source of inspiration and some of the examples in this
document.

---

Next: [SRFI-244 - Multiple-value Definitions](#SRFI_002d244), Previous: [SRFI-197: Pipeline Operators](#SRFI_002d197), Up: [SRFI Support Modules](#SRFI-Support)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49 SRFI-207 String-notated bytevectors [¶](#SRFI_002d207-String_002dnotated-bytevectors)

[SRFI-207](http://srfi.schemers.org/srfi-207/srfi-207.html)
provides a more human-friendly representation for binary-data via an
ASCII text notation for see [Bytevectors](#Bytevectors). It also provides
bytestring-oriented procedures for constructing bytevectors from
sequences of integers, characters, strings, and other bytevectors, and
procedures for manipulating bytevectors as if they were strings.

Binary file formats are usually not self-describing, and if they are,
the descriptive portion is itself binary, which makes it hard for human
beings to interpret. To assist with this problem, it is common to have
a human-readable section at the beginning of the file, or in some cases
at the beginning of each distinct section of the file. For historical
reasons and to avoid text encoding complications, it is usual for this
human-readable section to be expressed as ASCII text.

For example, ZIP files begin with the hex bytes `50 4B` which are
the ASCII encoding for the characters "PK", the initials of Phil Katz,
the inventor of ZIP format. As another example, the GIF image format
begins with `47 49 46 38 39 61`, the ASCII encoding for "GIF89a",
where "89a" is the format version. A third example is the PNG image
format, where the file header begins `89 50 4E 47`. The first byte
is intentionally non-ASCII, but the next three are "PNG". Furthermore,
a PNG file is divided into chunks, each of which contains a 4-byte
"chunk type" code. The letters in the chunk type are mnemonics for its
purpose, such as "PLTE" for a palette, "bKGD" for a default background
color, and "iTXt" for descriptive text in UTF-8.

When bytevectors contain string data of this kind, it is much more
tractable for human programmers to deal with
`#u8"\x89;PNG\r\n\x1A;\n"` rather than `#u8(0x89 0x50 0x4E
0x47 0x0D 0x0A 0x1A 0x0A)`.

In addition, this SRFI provides bytevectors with additional procedures
that closely resemble those provided for strings. For example,
bytevectors can be padded or trimmed, compared case-sensitively or
case-insensitively, searched, joined, and split.

Most of the procedures of this SRFI begin with `bytestring-` in
order to distinguish them from other bytevector procedures. This does
not mean that they accept or return a separate bytestring type:
bytestrings and bytevectors are exactly the same type.

* [External Notation](#SRFI_002d207-External-Notation)
* [Constructors](#SRFI_002d207-Contructors)
* [Conversion](#SRFI_002d207-Conversion)
* [Selection](#SRFI_002d207-Selection)
* [Replacement](#SRFI_002d207-Replacement)
* [Comparison](#SRFI_002d207-Comparison)
* [Searching](#SRFI_002d207-Searching)
* [Joining And Splitting](#SRFI_002d207-Joining-And-Splitting)
* [I/O](#SRFI_002d207-I_002fO)
* [Exceptions](#SRFI_002d207-Exceptions)
* [Acknowledgements](#SRFI_002d207-Acknowledgements)

---

Next: [Constructors](#SRFI_002d207-Contructors), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.1 External Notation [¶](#External-Notation)

The basic form of a string-notated bytevector is
`#u8"CONTENT"`. The Scheme reader will read them if bytestrings are
enabled via `(read-enable 'bytestrings)`, and the Scheme writer
will write them if they are enabled via `(print-enable
'bytestrings)`.

To avoid character encoding issues within string-notated bytevectors,
only printable ASCII characters (that is, Unicode codepoints in the
range from U+0020 to U+007E inclusive) are allowed to be used within the
CONTENT of a string-notated bytevector. All other characters must
be expressed through mnemonic or inline hex escapes, and `"` and
`\` must also be escaped as in normal Scheme strings.

Within the CONTENT of a string-notated bytevector:

* `\a` ⇒ 7
* `\b` ⇒ 8
* `\t` ⇒ 9
* `\n` ⇒ 10
* `\r` ⇒ 13
* `\"` ⇒ 34
* `\\` ⇒ 92
* `\|` ⇒ 124
* the sequence `\x` followed by zero or more `0` characters,
  followed by one or two hexadecimal digits, followed by `;`
  represents the integer specified by the hexadecimal digits;
* the sequence `\` followed by zero or more intraline whitespace
  characters, followed by a newline, followed by zero or more further
  intraline whitespace characters, is ignored and corresponds to no entry
  in the resulting bytevector;
* any other printable ASCII character represents the character number of
  that character in the ASCII/Unicode code chart; and
* it is an error to use any other character or sequence beginning with
  `\` within a string-notated bytevector.

Note: The `\|` sequence is provided so that string parsing, symbol
parsing, and string-notated bytevector parsing can all use the same
sequences. However, we give a complete definition of the valid lexical
syntax in this SRFI rather than inheriting the native syntax of strings,
so that it is clear that `#u8"&iota;"` and `#u8"\xE000;"` are
invalid.

When the Scheme reader encounters a string-notated bytevector, it
produces a datum as if that bytevector had been written out in
full. That is, `#u8"A"` is exactly equivalent to `#u8(65)`.

---

Next: [Conversion](#SRFI_002d207-Conversion), Previous: [External Notation](#SRFI_002d207-External-Notation), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.2 Constructors [¶](#Constructors-1)

Scheme Procedure: **bytestring** part … [¶](#index-bytestring)
:   Converts earch part into a sequence of small integers and returns
    a bytevector of the corresponding bytes as follows:

    * If part is an exact integer in the range 0-255 inclusive, it is
      added to the result.
    * If part is an ASCII character (that is, its codepoint is in the
      range 0-127 inclusive), it is converted to its codepoint and added to
      the result.
    * If part is a bytevector, its elements are added to the result.
    * If part is a string of ASCII characters, it is converted to a
      sequence of codepoints which are added to the result.

    Otherwise, an error satisfying `bytestring-error?` is signaled, for
    example:

    ```
    (bytestring "lo" #\r #x65 #u8(#x6d)) ⇒ #u8"lorem"
    ```

    ```
    (bytestring "η" #\space #u8(#x65 #x71 #x75 #x69 #x76))
    ⇒ raised &bytestring-error
    ```

Scheme Procedure: **make-bytestring** parts [¶](#index-make_002dbytestring)
:   If the parts are suitable arguments for `bytestring`, returns
    the bytevector that would result from applying `bytestring` to
    them. Otherwise, an error satisfying `bytestring-error?` is
    raised.

Scheme Procedure: **make-bytestring!** bytevector at parts [¶](#index-make_002dbytestring_0021)
:   If the parts are suitable arguments for `bytestring`, writes
    the bytes of the bytevector that would be the result of calling
    `make-bytestring` into bytevector starting at index
    at. For example:

    ```
    (define bv (make-bytevector 10 #x20))
    (make-bytestring! bv 2 '(#\s #\c "he" #u8(#x6d #x65))) bv)
    ⇒ #u8" scheme "
    ```

---

Next: [Selection](#SRFI_002d207-Selection), Previous: [Constructors](#SRFI_002d207-Contructors), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.3 Conversion [¶](#Conversion-1)

Scheme Procedure: **bytevector->hex-string** bytevector [¶](#index-bytevector_002d_003ehex_002dstring)

Scheme Procedure: **hex-string->bytevecto** string [¶](#index-hex_002dstring_002d_003ebytevecto)
:   Converts between a bytevector and a string containing pairs of
    hexadecimal digits. If string is not pairs of hexadecimal digits,
    an error satisfying `bytestring-error?` is raised

    ```
    (bytevector->hex-string #u8"Ford") ⇒ "467f7264"
    (hex-string->bytevector "5a6170686f64") ⇒ #u8"Zaphod")
    ```

Scheme Procedure: **bytevector->base64** bytevector [digits] [¶](#index-bytevector_002d_003ebase64)

Scheme Procedure: **base64->bytevecto** string [digits] [¶](#index-base64_002d_003ebytevecto)
:   Converts between a bytevector and its base-64 encoding as a string. The
    64 digits are represented by the characters 0-9, A-Z, a-z, and the
    symbols + and /. However, there are different variants of base-64
    encoding which use different representations of the 62nd and 63rd
    digit. If the optional argument digits (a two-character string) is
    provided, those two characters will be used as the 62nd and 63rd digit
    instead. Details can be found in
    [RFC 4648](https://tools.ietf.org/html/rfc4648).

    If string is not in base-64 format, an error satisfying
    `bytestring-error?` is raised. However, characters that satisfy
    `char-whitespace?` are silently ignored.

    ```
    (bytevector->base64 #u8(1 2 3 4 5 6)) ⇒ ⇒ "AQIDBAUG"
    (bytevector->base64 #u8"Arthur Dent") ⇒ "QXJ0aHVyIERlbnQ="
    (base64->bytevector "+/     /+") ⇒ #u8(#xfb #xff #xfe)
    ```

Scheme Procedure: **bytestring->list** bytevector [start [end]] [¶](#index-bytestring_002d_003elist)
:   Converts all or part of a bytevector into a list of the same length
    containing characters for elements in the range 32 to 127 and exact
    integers for all other elements.</p>

    ```
    (bytestring->list #u8(#x41 #x42 1 2) 1 3) ⇒ (#\B 1)
    ```

Scheme Procedure: **make-bytestring-generator** arg … [¶](#index-make_002dbytestring_002dgenerator)
:   Returns a generator that when invoked will return consecutive bytes of
    the bytevector that `bytestring` would create when applied to
    args, but without creating any bytevectors. The args are
    validated before any bytes are generated; if they are ill-formed, an
    error satisfying `bytestring-error?` is raised.

    ```
    (generator->list (make-bytestring-generator "lorem"))
    ⇒ (#x6c #x6f #x72 #x65 #x6d)
    ```

---

Next: [Replacement](#SRFI_002d207-Replacement), Previous: [Conversion](#SRFI_002d207-Conversion), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.4 Selection [¶](#Selection)

Scheme Procedure: **bytestring-pad** bytevector len char-or-u8 [¶](#index-bytestring_002dpad)

Scheme Procedure: **bytestring-pad-right** bytevector len char-or-u8 [¶](#index-bytestring_002dpad_002dright)
:   Returns a newly allocated bytevector with the contents of
    bytevector plus sufficient additional bytes at the beginning/end
    containing char-or-u8 (which can be either an ASCII character or
    an exact integer in the range 0-255) such that the length of the result
    is at least len.

    ```
    (bytestring-pad #u8"Zaphod" 10 #\_) ⇒ #u8"____Zaphod"
    (bytestring-pad-right #u8(#x80 #x7f) 8 0) ⇒ #u8(#x80 #x7f 0 0 0 0 0 0)
    ```

Scheme Procedure: **bytestring-trim** bytevector pred [¶](#index-bytestring_002dtrim)

Scheme Procedure: **bytestring-trim-right** bytevector pred [¶](#index-bytestring_002dtrim_002dright)

Scheme Procedure: **bytestring-trim-both** bytevector pred [¶](#index-bytestring_002dtrim_002dboth)
:   Returns a newly allocated bytevector with the contents of
    bytevector, except that consecutive bytes at the beginning / the
    end / both the beginning and the end that satisfy pred are not
    included.

    ```
    (bytestring-trim #u8"   Trillian" (lambda (b) (= b #x20)))
    ⇒ #u8"Trillian"
    (bytestring-trim-both #u8(0 0 #x80 #x7f 0 0 0) zero?) ⇒ #u8(#x80 #x7f)
    ```

---

Next: [Comparison](#SRFI_002d207-Comparison), Previous: [Selection](#SRFI_002d207-Selection), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.5 Replacement [¶](#Replacement)

Scheme Procedure: **bytestring-replace** bytevector1 bytevector2 start1 end1 [start2 end2] [¶](#index-bytestring_002dreplace)
:   Returns a newly allocated bytevector with the contents of
    bytevector1, except that the bytes indexed by start1 and
    end1 are not included but are replaced by the bytes of
    bytevector2 indexed by start2 and end2.

    ```
    (bytestring-replace #u8"Vogon torture" #u8"poetry" 6 13)
    ⇒ #u8"Vogon poetry"
    ```

---

Next: [Searching](#SRFI_002d207-Searching), Previous: [Replacement](#SRFI_002d207-Replacement), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.6 Comparison [¶](#Comparison-1)

To compare bytevectors for equality, use the `bytevector=?`
procedure from `(rnrs bytevectors)` (see [Bytevectors](#Bytevectors)) or
`equal?`.

Scheme Procedure: **bytestring<?** bytevector1 bytevector2 [¶](#index-bytestring_003c_003f)

Scheme Procedure: **bytestring>?** bytevector1 bytevector2 [¶](#index-bytestring_003e_003f)

Scheme Procedure: **bytestring<=?** bytevector1 bytevector2 [¶](#index-bytestring_003c_003d_003f)

Scheme Procedure: **bytestring>=?** bytevector1 bytevector2 [¶](#index-bytestring_003e_003d_003f)
:   Returns `#t` if bytevector1 is less than / greater than /
    less than or equal to / greater than or equal to
    bytevector2. Comparisons are lexicographical: shorter bytevectors
    compare before longer ones, all elements being equal.

    ```
    (bytestring<? #u8"Heart Of Gold" #u8"Heart of Gold") ⇒ #t
    (bytestring<=? #u8(#x81 #x95) #u8(#x80 #xa0)) ⇒ #f
    (bytestring>? #u8(1 2 3) #u8(1 2)) ⇒ #t
    ```

---

Next: [Joining And Splitting](#SRFI_002d207-Joining-And-Splitting), Previous: [Comparison](#SRFI_002d207-Comparison), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.7 Searching [¶](#Searching-1)

Scheme Procedure: **bytestring-index** bytevector pred [start [end]] [¶](#index-bytestring_002dindex)

Scheme Procedure: **bytestring-index-right** bytevector pred [start [end]] [¶](#index-bytestring_002dindex_002dright)
:   Searches bytevector from start to end / from
    end to start for the first byte that satisfies pred,
    and returns the index into bytevector containing that byte. In
    either direction, start is inclusive and end is
    exclusive. If there are no such bytes, returns `#f`.

    ```
    (bytestring-index #u8(#x65 #x72 #x83 #x6f) (λ (b) (> b #x7f))) ⇒ 2
    (bytestring-index #u8"Beeblebrox" (λ (b) (> b #x7f))) ⇒ #f
    (bytestring-index-right #u8"Zaphod" odd?) ⇒ 4
    ```

Scheme Procedure: **bytestring-break** bytevector pred [¶](#index-bytestring_002dbreak)

Scheme Procedure: **bytestring-span** bytevector pred [¶](#index-bytestring_002dspan)
:   Returns two values, a bytevector containing the maximal sequence of
    characters (searching from the beginning of bytevector to the end)
    that do not satisfy / do satisfy pred, and another bytevector
    containing the remaining characters.

    ```
    (bytestring-break #u8(#x50 #x4b 0 0 #x1 #x5) zero?)
      ⇒ #u8(#x50 #x4b) #u8(0 0 #x1 #x5)
    (bytestring-span #u8"ABCDefg" (lambda (b) (and (> b 40) (< b 91))))
      ⇒ #u8"ABCD" #u8"efg"
    ```

---

Next: [I/O](#SRFI_002d207-I_002fO), Previous: [Searching](#SRFI_002d207-Searching), Up: [SRFI-207 String-notated bytevectors](#SRFI_002d207)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.5.49.8 Joining And Splitting [¶](#Joining-And-Splitting)

Scheme Procedure: **bytestring-join** bytevector-list delimiter [¶](#index-bytestring_002djoin)
:   Joins the bytevectors in bytevector-list together using the
    delimiter, which can be anything suitable as an argument to
    `bytestring`. The grammar argument is a symbol that
    determines how the delimiter is used, and defaults to `infix`. It
    is an error for grammar to be any symbol other than these four:

    `infix`
    :   means an infix or separator grammar: inserts the delimiter between list
        elements. An empty list will produce an empty bytevector

    `strict-infix`
    :   means the same as `infix` if the list is non-empty, but will signal
        an error satisfying `bytestring-error?` if given an empty
        list.

    `suffix`
    :   means a suffix or terminator grammar: inserts the delimiter after every list element.

    `prefix`
    :   means a prefix grammar: inserts the delimiter before every list element.

    For example:
