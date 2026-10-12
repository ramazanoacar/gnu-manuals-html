## before executing them.
AM_TESTS_FD_REDIRECT = 9>&2
```

As another example, a notice that a test is starting can be emitted
using `AM_TESTS_ENVIRONMENT` (for package maintainers) or
`TESTS_ENVIRONMENT` by users:

```
make -j12 ... TESTS_ENVIRONMENT='echo RUNNING: "$$f";' check
```

The shell variable `$f` contains the test name. (Although
technically this is not guaranteed, in practice it is extremely
unlikely to ever change.) This can be helpful to see when trying to
debug test failures.

Notwithstanding these benefits, `AM_TESTS_ENVIRONMENT` is, for
historical and implementation reasons, *not* supported by the
serial harness (see [Older (and discouraged) Serial Test Harness](#Serial-Test-Harness)).

---

Next: [Parallel Test Harness](#Parallel-Test-Harness), Previous: [Scripts-based Testsuites](#Scripts_002dbased-Testsuites), Up: [Simple Tests](#Simple-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.2.2 Older (and discouraged) Serial Test Harness [¶](#Older-_0028and-discouraged_0029-Serial-Test-Harness)

First, note that today the use of this harness is strongly discouraged in
favor of the parallel test harness (see [Parallel Test Harness](#Parallel-Test-Harness)).
Still, there are a *few* situations when the advantages offered by
the parallel harness are irrelevant, and when test concurrency can
even cause tricky problems. In those cases, it might make sense to
still use the serial harness, for simplicity and reliability (we still
suggest trying to give the parallel harness a shot though).

The serial test harness is enabled by the Automake option
`serial-tests`. It operates by simply running the tests serially,
one at the time, without any I/O redirection. It’s up to the user to
implement logging of tests’ output, if that’s required or desired.

For historical and implementation reasons, the `AM_TESTS_ENVIRONMENT`
variable is *not* supported by this harness (it will be silently
ignored if defined); only `TESTS_ENVIRONMENT` is, and it is to be
considered a developer-reserved variable. This is done so that, when
using the serial harness, `TESTS_ENVIRONMENT` can be defined to an
invocation of an interpreter through which the tests are to be run.
For instance, the following setup may be used to run tests with Perl:

```
TESTS_ENVIRONMENT = $(PERL) -Mstrict -w
TESTS = foo.pl bar.pl baz.pl
```

It’s important to note that the use of `TESTS_ENVIRONMENT` endorsed
here would be *invalid* with the parallel harness. That harness
provides a more elegant way to achieve the same effect, with the further
benefit of freeing the `TESTS_ENVIRONMENT` variable for the user
(see [Parallel Test Harness](#Parallel-Test-Harness)).

Another, less serious limitation of the serial harness is that it
doesn’t distinguish between simple failures and hard errors; this is
for historical reasons, and might be fixed in future Automake
versions.

---

Previous: [Older (and discouraged) Serial Test Harness](#Serial-Test-Harness), Up: [Simple Tests](#Simple-Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.2.3 Parallel Test Harness [¶](#Parallel-Test-Harness-1)

By default, Automake generates a parallel (concurrent) test harness. It
features automatic collection of the test scripts output in `.log`
files, concurrent execution of tests with `make -j`, specification
of inter-test dependencies, lazy reruns of tests that have not completed
in a prior run, and hard errors for exceptional failures.

* [Parallel Test Basics](#Parallel-Test-Basics)
* [Parallel Test Runners](#Parallel-Test-Runners)
* [Parallel Test Selection](#Parallel-Test-Selection)
* [Parallel Test Dependencies](#Parallel-Test-Dependencies)
* [Parallel Test Variables](#Parallel-Test-Variables)

---

Next: [Parallel Test Runners](#Parallel-Test-Runners), Up: [Parallel Test Harness](#Parallel-Test-Harness)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.2.3.1 Parallel Test Basics [¶](#Parallel-Test-Basics-1)

The parallel test harness operates by defining a set of `make`
rules that run the test scripts listed in `TESTS`, and, for each
such script, save its output in a corresponding `.log` file and
its results (and other “metadata”, see [API for Custom Test Drivers](#API-for-Custom-Test-Drivers))
in a corresponding `.trs` (as in **T**est **R**e**S**ults) file.
The `.log` file will contain all the output emitted by the test on
its standard output and its standard error. The `.trs` file will
contain, among the other things, the results of the test cases run by
the script.

The parallel test harness will also create a summary log file,
`TEST_SUITE_LOG`, which defaults to `test-suite.log` and requires
a `.log` suffix. This file depends upon all the `.log` and
`.trs` files created for the test scripts listed in `TESTS`.
It contains the output of all tests that failed, encountered a hard error,
succeeded unexpectedly, or—unless the variable `IGNORE_SKIPPED_LOGS`
is set to a non-empty value—were skipped.

As with the serial harness above, by default one status line is printed
per completed test, and a short summary after the suite has completed.
However, standard output and standard error of the test are redirected
to a per-test log file, so that parallel execution does not produce
intermingled output. The output from failed tests is collected in the
`test-suite.log` file. If the variable ‘`VERBOSE`’ is set, this
file is output after the summary.

Each pair of `.log` and `.trs` files is created when the
corresponding test has completed. The set of log files is listed in
the read-only variable `TEST_LOGS`, and defaults to `TESTS`,
with the executable extension if any (see [Support for executable extensions](#EXEEXT)), as well as any
suffix listed in `TEST_EXTENSIONS` removed, and `.log` appended.
Results are undefined if a test file name ends in several concatenated
suffixes. `TEST_EXTENSIONS` defaults to `.test`; it can be
overridden by the user, in which case any extension listed in it must be
constituted by a dot, followed by a non-digit alphabetic character,
followed by any number of alphabetic characters.
For example, ‘`.sh`’, ‘`.T`’ and ‘`.t1`’ are valid extensions,
while ‘`.x-y`’, ‘`.6c`’ and ‘`.t.1`’ are not.

It is important to note that, due to current limitations (unlikely to be
lifted), configure substitutions in the definition of `TESTS` can
only work if they will expand to a list of tests that have a suffix listed
in `TEST_EXTENSIONS`.

---

Next: [Parallel Test Selection](#Parallel-Test-Selection), Previous: [Parallel Test Basics](#Parallel-Test-Basics), Up: [Parallel Test Harness](#Parallel-Test-Harness)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.2.3.2 Parallel Test Runners [¶](#Parallel-Test-Runners-1)

For tests that match an extension `.ext` listed in
`TEST_EXTENSIONS`, you can provide a custom “test runner” using
the variable `ext_LOG_COMPILER` (note the upper-case
extension) and pass options in `AM_ext_LOG_FLAGS` and allow
the user to pass options in `ext_LOG_FLAGS`. It will cause
all tests with this extension to be called with this runner. For all
tests without a registered extension, the variables `LOG_COMPILER`,
`AM_LOG_FLAGS`, and `LOG_FLAGS` may be used. For example,

```
TESTS = foo.pl bar.py baz
TEST_EXTENSIONS = .pl .py
PL_LOG_COMPILER = $(PERL)
AM_PL_LOG_FLAGS = -w
PY_LOG_COMPILER = $(PYTHON)
AM_PY_LOG_FLAGS = -v
LOG_COMPILER = ./wrapper-script
AM_LOG_FLAGS = -d
```

will invoke ‘`$(PERL) -w foo.pl`’, ‘`$(PYTHON) -v bar.py`’,
and ‘`./wrapper-script -d baz`’ to produce `foo.log`,
`bar.log`, and `baz.log`, respectively. The `foo.trs`,
`bar.trs` and `baz.trs` files will be automatically produced
as a side-effect.

It’s important to note that, differently from what we’ve seen for the
serial test harness (see [Older (and discouraged) Serial Test Harness](#Serial-Test-Harness)), the
`AM_TESTS_ENVIRONMENT` and `TESTS_ENVIRONMENT` variables
*cannot* be used to define a custom test runner; the
`LOG_COMPILER` and `LOG_FLAGS` (or their extension-specific
counterparts) should be used instead:

```
## This is WRONG!
AM_TESTS_ENVIRONMENT = PERL5LIB='$(srcdir)/lib' $(PERL) -Mstrict -w
```

```
## Do this instead.
AM_TESTS_ENVIRONMENT = PERL5LIB='$(srcdir)/lib'; export PERL5LIB;
LOG_COMPILER = $(PERL)
AM_LOG_FLAGS = -Mstrict -w
```

---

Next: [Parallel Test Dependencies](#Parallel-Test-Dependencies), Previous: [Parallel Test Runners](#Parallel-Test-Runners), Up: [Parallel Test Harness](#Parallel-Test-Harness)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.2.3.3 Parallel Test Selection [¶](#Parallel-Test-Selection-1)

By default, the test suite harness will run all tests, but there are
several ways to limit the set of tests that are run:

* You can set the `TESTS` variable. For example, you can use a
  command like this to run only a subset of the tests:

  ```
  env TESTS="foo.test bar.test" make -e check
  ```

  If you’re using a recursive make setup, you’ll probably also need to
  override `SUBDIRS`:

  ```
  env TESTS="foo.test bar.test" make -e check SUBDIRS=
  ```

  Otherwise, the test harness will descend into all subdirectories,
  where the tests presumably do not exist, and thus fail. (Patch to
  provide better behavior would be welcome.)

  Another issue: the command above will unconditionally overwrite the
  `test-suite.log` file, thus clobbering the recorded results
  of any previous testsuite run. This might be undesirable for packages
  whose testsuite takes a long time to execute. Luckily, this problem can
  easily be avoided by also overriding `TEST_SUITE_LOG` at runtime;
  for example,

  ```
  env TEST_SUITE_LOG=partial.log TESTS="..." make -e check
  ```

  will write the result of the partial testsuite runs to the
  `partial.log`, without touching `test-suite.log`.
* You can set the `TEST_LOGS` variable. By default, this variable is
  computed at `make` run time from the value of `TESTS` as
  described above. For example, you can use the following:

  ```
  set x subset*.log; shift
  env TEST_LOGS="foo.log $*" make -e check
  ```

  The comments made above about `TEST_SUITE_LOG` overriding applies
  here too.
* By default, the test harness removes all old per-test `.log` and
  `.trs` files before it starts running tests to regenerate them. The
  variable `RECHECK_LOGS` contains the set of `.log` (and, by
  implication, `.trs`) files which are removed. `RECHECK_LOGS`
  defaults to `TEST_LOGS`, which means all tests need to be rechecked.
  By overriding this variable, you can choose which tests need to be
  reconsidered. For example, you can lazily rerun only those tests which
  are outdated, i.e., older than their prerequisite test files, by setting
  this variable to the empty value:

  ```
  env RECHECK_LOGS= make -e check
  ```
* You can ensure that all tests are rerun which have failed or passed
  unexpectedly, by running `make recheck` in the test directory.
  This convenience target will set `RECHECK_LOGS` appropriately
  before invoking the main test harness.

---

Next: [Parallel Test Variables](#Parallel-Test-Variables), Previous: [Parallel Test Selection](#Parallel-Test-Selection), Up: [Parallel Test Harness](#Parallel-Test-Harness)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.2.3.4 Parallel Test Dependencies [¶](#Parallel-Test-Dependencies-1)

In order to guarantee an ordering between tests even with `make
-jN`, dependencies between the corresponding `.log` files
may be specified through usual `make` dependencies. For example,
the following snippet lets the test named `foo-execute.test` depend
upon completion of the test `foo-compile.test`:

```
TESTS = foo-compile.test foo-execute.test
foo-execute.log: foo-compile.log
```

Please note that this ordering ignores the *results* of required
tests, thus the test `foo-execute.test` is run even if the test
`foo-compile.test` failed or was skipped beforehand. Further,
specifying such dependencies currently works only for tests that end
in one of the suffixes listed in `TEST_EXTENSIONS`.

Tests without such specified dependencies may be run concurrently with
parallel `make -jN`, so be sure they are prepared for
concurrent execution.

The combination of lazy test execution and correct dependencies
between tests and their sources may be exploited for efficient unit
testing during development. To further speed up the edit-compile-test
cycle, it may even be useful to specify compiled programs in
`EXTRA_PROGRAMS` instead of with `check_PROGRAMS`, as the
former allows intertwined compilation and test execution (but
`EXTRA_PROGRAMS` are not cleaned automatically; see [The Uniform Naming Scheme](#Uniform)).

---

Previous: [Parallel Test Dependencies](#Parallel-Test-Dependencies), Up: [Parallel Test Harness](#Parallel-Test-Harness)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.2.3.5 Parallel Test Variables [¶](#Parallel-Test-Variables-1)

The variables `TESTS` and `XFAIL_TESTS` may contain
conditional parts as well as configure substitutions. In the latter
case, however, certain restrictions apply: substituted test names
must end with a nonempty test suffix like `.test`, so that one of
the inference rules generated by `automake` can apply. For
literal test names, `automake` can generate per-target rules
to avoid this limitation.

It is currently not possible to use `$(srcdir)/` or
`$(top_srcdir)/` in the `TESTS` variable. This technical
limitation is necessary to avoid generating test logs in the source
tree and has the unfortunate consequence that it is not possible to
specify distributed tests that are themselves generated by means of
explicit rules, in a way that is portable to all `make`
implementations (see [Make Target Lookup](https://www.gnu.org/software/autoconf/manual/autoconf.html#Make-Target-Lookup) in The Autoconf
Manual; the semantics of FreeBSD and OpenBSD `make` conflict
with this). In case of doubt you may want to require to use GNU
`make`, or work around the issue with inference rules to
generate the tests.

---

Next: [Using the TAP test protocol](#Using-the-TAP-test-protocol), Previous: [Simple Tests](#Simple-Tests), Up: [Support for test suites](#Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.3 Custom Test Drivers [¶](#Custom-Test-Drivers-1)

* [Overview of Custom Test Drivers Support](#Overview-of-Custom-Test-Drivers-Support)
* [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers)
* [API for Custom Test Drivers](#API-for-Custom-Test-Drivers)

---

Next: [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers), Up: [Custom Test Drivers](#Custom-Test-Drivers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.3.1 Overview of Custom Test Drivers Support [¶](#Overview-of-Custom-Test-Drivers-Support-1)

Starting from Automake version 1.12, the parallel test harness allows
the package authors to use third-party custom test drivers, in case the
default ones are inadequate for their purposes, or do not support their
testing protocol of choice.

A custom test driver is expected to properly run the test programs passed
to it (including the command-line arguments passed to those programs, if
any), to analyze their execution and outcome, to create the `.log`
and `.trs` files associated to these test runs, and to display the test
results on the console. It is responsibility of the author of the test
driver to ensure that it implements all the above steps meaningfully and
correctly; Automake isn’t and can’t be of any help here. On the other
hand, the Automake-provided code for testsuite summary generation offers
support for test drivers allowing several test results per test script,
if they take care to register such results properly (see [Log files generation and test results recording](#Log-files-generation-and-test-results-recording)).

The exact details of how test scripts’ results are to be determined and
analyzed is left to the individual drivers. Some drivers might only
consider the test script exit status (this is done for example by the
default test driver used by the parallel test harness, described
in the previous section). Other drivers might implement more complex and
advanced test protocols, which might require them to parse and interpret
the output emitted by the test script they’re running (examples of such
protocols are TAP and SubUnit).

It’s very important to note that, even when using custom test drivers,
most of the infrastructure described in the previous section about the
parallel harness remains in place; this includes:

* list of test scripts defined in `TESTS`, and overridable at
  runtime through the redefinition of `TESTS` or `TEST_LOGS`;
* concurrency through the use of `make`’s option `-j`;
* per-test `.log` and `.trs` files, and generation of a summary
  `.log` file from them;
* `recheck` target, `RECHECK_LOGS` variable, and lazy reruns
  of tests;
* inter-test dependencies;
* support for `check_*` variables (`check_PROGRAMS`,
  `check_LIBRARIES`, ...);
* use of `VERBOSE` environment variable to get verbose output on
  testsuite failures;
* definition and honoring of `TESTS_ENVIRONMENT`,
  `AM_TESTS_ENVIRONMENT` and `AM_TESTS_FD_REDIRECT`
  variables;
* definition of generic and extension-specific `LOG_COMPILER` and
  `LOG_FLAGS` variables.

On the other hand, the exact semantics of how (and if) testsuite output
colorization, `XFAIL_TESTS`, and hard errors are supported and
handled is left to the individual test drivers.

---

Next: [API for Custom Test Drivers](#API-for-Custom-Test-Drivers), Previous: [Overview of Custom Test Drivers Support](#Overview-of-Custom-Test-Drivers-Support), Up: [Custom Test Drivers](#Custom-Test-Drivers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.3.2 Declaring Custom Test Drivers [¶](#Declaring-Custom-Test-Drivers-1)

Custom testsuite drivers are declared by defining the make variables
`LOG_DRIVER` or `ext_LOG_DRIVER` (where ext must
be declared in `TEST_EXTENSIONS`). They must be defined to
programs or scripts that will be used to drive the execution, logging,
and outcome report of the tests with corresponding extensions (or of
those with no registered extension in the case of `LOG_DRIVER`).
Clearly, multiple distinct test drivers can be declared in the same
`Makefile.am`. Note moreover that the `LOG_DRIVER` variables
are *not* a substitute for the `LOG_COMPILER` variables: the
two sets of variables can, and often do, usefully and legitimately
coexist.

The developer-reserved variable `AM_LOG_DRIVER_FLAGS` and the
user-reserved variable `LOG_DRIVER_FLAGS` can be used to define
flags that will be passed to each invocation of `LOG_DRIVER`,
with the user-defined flags obviously taking precedence over the
developer-reserved ones. Similarly, for each extension ext
declared in `TEST_EXTENSIONS`, flags listed in
`AM_ext_LOG_DRIVER_FLAGS` and
`ext_LOG_DRIVER_FLAGS` will be passed to
invocations of `ext_LOG_DRIVER`.

---

Previous: [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers), Up: [Custom Test Drivers](#Custom-Test-Drivers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.3.3 API for Custom Test Drivers [¶](#API-for-Custom-Test-Drivers-1)

Note that *the APIs described here are still highly
experimental*, and will very likely undergo tightening and possibly
extensive changes in the future, to accommodate for new features or to
satisfy additional portability requirements.

The main characteristic of these APIs is that they are designed to share
as much infrastructure, semantics, and implementation detail as possible
with the parallel test harness and its default driver.

* [Command-line arguments for test drivers](#Command_002dline-arguments-for-test-drivers)
* [Log files generation and test results recording](#Log-files-generation-and-test-results-recording)
* [Testsuite progress output](#Testsuite-progress-output)

---

Next: [Log files generation and test results recording](#Log-files-generation-and-test-results-recording), Up: [API for Custom Test Drivers](#API-for-Custom-Test-Drivers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.3.3.1 Command-line arguments for test drivers [¶](#Command_002dline-arguments-for-test-drivers-1)

A custom driver can rely on various command-line options and arguments
being passed to it automatically by the Automake-generated test
harness. It is *mandatory* that it understands all of them (even
if the exact interpretation of the associated semantics can
legitimately change between a test driver and another, and even be a
no-op in some drivers). Options are separated from their arguments by
whitespace.[4](#FOOT4)

Here is the list of options:

`--test-name NAME`
:   The name of the test, with VPATH prefix (if any) removed. This can have a
    suffix and a directory component (as in e.g., `sub/foo.test`), and is
    mostly meant to be used in console reports about testsuite advancements and
    results (see [Testsuite progress output](#Testsuite-progress-output)).

`--log-file PATH.log`
:   The `.log` file the test driver must create (see [Basics of test metadata](#Basics-of-test-metadata)). If it has a directory component (as in e.g.,
    `sub/foo.log`), the test harness will ensure that such directory
    exists *before* the test driver is called.

`--trs-file PATH.trs`
:   The `.trs` file the test driver must create (see [Basics of test metadata](#Basics-of-test-metadata)). If it has a directory component (as in e.g.,
    `sub/foo.trs`), the test harness will ensure that such directory
    exists *before* the test driver is called.

`--color-tests {yes|no}`
:   Whether the console output should be colorized or not (see [Simple tests and color-tests](#Simple-tests-and-color_002dtests), to learn when this option gets activated and
    when it doesn’t).

`--collect-skipped-logs {yes|no}`
:   Whether to include the logs of skipped tests in the global
    `test-suite.log` file.

`--expect-failure {yes|no}`
:   Whether the tested program is expected to fail.

`--enable-hard-errors {yes|no}`
:   Whether “hard errors” in the tested program should be treated differently
    from normal failures or not (the default should be `yes`). The exact
    meaning of “hard error” is highly dependent from the test protocols or
    conventions in use.

`--`
:   Explicitly terminate the list of options.

The first non-option argument passed to the test driver is the program to
be run, and all the following ones are command-line options and arguments
for this program.

Exact semantics attached to the `--color-tests`,
`--collect-skipped-logs`, `--expect-failure`, and
`--enable-hard-errors` options are left up to the individual
test drivers. Still, having a behavior compatible or at least similar
to that provided by the default driver is advised, as that results
in better consistency and thus a more pleasant user experience.

---

Next: [Testsuite progress output](#Testsuite-progress-output), Previous: [Command-line arguments for test drivers](#Command_002dline-arguments-for-test-drivers), Up: [API for Custom Test Drivers](#API-for-Custom-Test-Drivers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.3.3.2 Log files generation and test results recording [¶](#Log-files-generation-and-test-results-recording-1)

The test driver must correctly generate the files specified by the
`--log-file` and `--trs-file` option (even when the tested
program fails or crashes).

The `.log` file should ideally contain all the output produced by the
tested program, plus optionally other information that might facilitate
debugging or analysis of bug reports. Apart from that, its format is
basically free.

The `.trs` file is used to register some metadata through the use
of custom reStructuredText fields. This metadata is expected to be
employed in various ways by the parallel test harness; for example, to
count the test results when printing the testsuite summary, or to decide
which tests to rerun upon `make recheck`. Unrecognized metadata
in a `.trs` file is currently ignored by the harness, but this might
change in the future. The list of currently recognized metadata follows.

`:test-result:` [¶](#index-Register-test-result)
:   The test driver must use this field to register the results of *each*
    test case run by a test script file. Several `:test-result:` fields
    can be present in the same `.trs` file; this is done in order to
    support test protocols that allow a single test script to run more test
    cases.

    The only recognized test results are currently `PASS`, `XFAIL`,
    `SKIP`, `FAIL`, `XPASS` and `ERROR`. These results,
    when declared with `:test-result:`, can be optionally followed by
    text holding the name and/or a brief description of the corresponding
    test; the harness will ignore such extra text when generating
    `test-suite.log` and preparing the testsuite summary.

`:recheck:` [¶](#index-_003arecheck_003a)
:   If this field is present and defined to `no`, then the corresponding
    test script will *not* be run upon a `make recheck`. What
    happens when two or more `:recheck:` fields are present in the same
    `.trs` file is undefined behavior.

`:copy-in-global-log:` [¶](#index-_003acopy_002din_002dglobal_002dlog_003a)
:   If this field is present and defined to `no`, then the content
    of the `.log` file will *not* be copied into the global
    `test-suite.log`. We allow to forsake such copying because, while
    it can be useful in debugging and analysis of bug report, it can also be
    just a waste of space in normal situations, e.g., when a test script is
    successful. What happens when two or more `:copy-in-global-log:`
    fields are present in the same `.trs` file is undefined behavior.

`:global-test-result:` [¶](#index-_003aglobal_002dtest_002dresult_003a)
:   This is used to declare the “global result” of the script. Currently,
    the value of this field is only needed to be reported (more or less
    verbatim) in the generated global log file `$(TEST_SUITE_LOG)`,
    so it’s quite free-form. For example, a test script which runs 10 test
    cases, 6 of which pass and 4 of which are skipped, could reasonably have
    a `PASS/SKIP` value for this field, while a test script which runs
    19 successful tests and one failed test could have an `ALMOST
    PASSED` value. What happens when two or more `:global-test-result:`
    fields are present in the same `.trs` file is undefined behavior.

Let’s see a small example. Assume a `.trs` file contains the
following lines:

```
:test-result: PASS server starts
:global-log-copy: no
:test-result: PASS HTTP/1.1 request
:test-result: FAIL HTTP/1.0 request
:recheck: yes
:test-result: SKIP HTTPS request (TLS library wasn't available)
:test-result: PASS server stops
```

Then the corresponding test script will be rerun by `make check`,
will contribute with *five* test results to the testsuite summary
(three of these tests being successful, one failed, and one skipped), and
the content of the corresponding `.log` file will *not* be
copied into the global log file `test-suite.log`.

---

Previous: [Log files generation and test results recording](#Log-files-generation-and-test-results-recording), Up: [API for Custom Test Drivers](#API-for-Custom-Test-Drivers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.3.3.3 Testsuite progress output [¶](#Testsuite-progress-output-1)

A custom test driver also has the task of displaying, on the standard
output, the test results as soon as they become available. Depending on
the protocol in use, it can also display the reasons for failures and
skips, and, more generally, any useful diagnostic output (but remember
that each line on the screen is precious, so that cluttering the screen
with overly verbose information is bad idea). The exact format of this
progress output is left up to the test driver; in fact, a custom test
driver might *theoretically* even decide not to do any such report,
leaving it all to the testsuite summary (that would be a very lousy idea,
of course, and serves only to illustrate the flexibility that is
granted here).

Remember that consistency is good; so, if possible, try to be consistent
with the output of the built-in Automake test drivers, providing a similar
“look & feel”. In particular, the testsuite progress output should be
colorized when the `--color-tests` is passed to the driver. On the
other end, if you are using a known and widespread test protocol with
well-established implementations, being consistent with those
implementations’ output might be a good idea too.

---

Next: [DejaGnu Tests](#DejaGnu-Tests), Previous: [Custom Test Drivers](#Custom-Test-Drivers), Up: [Support for test suites](#Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.4 Using the TAP test protocol [¶](#Using-the-TAP-test-protocol-1)

* [Introduction to TAP](#Introduction-to-TAP)
* [Use TAP with the Automake test harness](#Use-TAP-with-the-Automake-test-harness)
* [Incompatibility with other TAP parsers and drivers](#Incompatibility-with-other-TAP-parsers-and-drivers)
* [Links and external resources on TAP](#Links-and-external-resources-on-TAP)

---

Next: [Use TAP with the Automake test harness](#Use-TAP-with-the-Automake-test-harness), Up: [Using the TAP test protocol](#Using-the-TAP-test-protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.4.1 Introduction to TAP [¶](#Introduction-to-TAP-1)

TAP, the Test Anything Protocol, is a simple text-based interface between
testing modules or programs and a test harness. The tests (also called
“TAP producers” in this context) write test results in a simple format
on standard output; a test harness (also called “TAP consumer”) will
parse and interpret these results, and properly present them to the user,
and/or register them for later analysis. The exact details of how this
is accomplished can vary among different test harnesses. The Automake
harness will present the results on the console in the usual
fashion (see [Testsuite progress on console](#Testsuite-progress-on-console)), and will use the
`.trs` files (see [Basics of test metadata](#Basics-of-test-metadata)) to store the test
results and related metadata. Apart from that, it will try to remain
as compatible as possible with pre-existing and widespread utilities,
such as the
[`prove` utility](https://metacpan.org/pod/distribution/Test-Harness/bin/prove), at least for the simpler usages.

TAP started its life as part of the test harness for Perl, but today
it has been (mostly) standardized, and has various independent
implementations in different languages; among them, C, C++, Perl,
Python, PHP, and Java. For a semi-official specification of the
TAP protocol, please refer to the documentation of
[‘`Test::Harness`’](https://metacpan.org/pod/Test::Harness).

The most relevant real-world usages of TAP are obviously in the testsuites
of `perl` and of many Perl modules. Still, other important
third-party packages, such as [`git`](https://git-scm.com/),
also use TAP in their testsuite.

---

Next: [Incompatibility with other TAP parsers and drivers](#Incompatibility-with-other-TAP-parsers-and-drivers), Previous: [Introduction to TAP](#Introduction-to-TAP), Up: [Using the TAP test protocol](#Using-the-TAP-test-protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.4.2 Use TAP with the Automake test harness [¶](#Use-TAP-with-the-Automake-test-harness-1)

Currently, the TAP driver that comes with Automake requires some by-hand
steps on the developer’s part (this situation should hopefully be improved
in future Automake versions). You’ll have to grab the `tap-driver.sh`
script from the Automake distribution by hand, copy it in your source tree,
and use the Automake support for third-party test drivers to instruct the
harness to use the `tap-driver.sh` script and the awk program found
by `AM_INIT_AUTOMAKE` to run your TAP-producing tests. See the example
below for clarification.

Apart from the options common to all the Automake test drivers
(see [Command-line arguments for test drivers](#Command_002dline-arguments-for-test-drivers)), `tap-driver.sh`
supports the following options, whose names are chosen for enhanced
compatibility with the `prove` utility.

`--ignore-exit`
:   Causes the test driver to ignore the exit status of the test scripts;
    by default, the driver will report an error if the script exits with a
    non-zero status. This option has effect also on non-zero exit statuses
    due to termination by a signal.

`--comments`
:   Instruct the test driver to display TAP diagnostics (i.e., lines beginning
    with the ‘`#`’ character) in the testsuite progress output too; by
    default, TAP diagnostics are only copied to the `.log` file.

`--no-comments`
:   Revert the effects of `--comments`.

`--merge`
:   Instruct the test driver to merge the test scripts’ standard error into
    their standard output. This is necessary if you want to ensure that
    diagnostics from the test scripts are displayed in the correct order
    relative to test results; this can be of great help in debugging
    (especially if your test scripts are shell scripts run with shell
    tracing active). As a downside, this option might cause the test
    harness to get confused if anything that appears on standard error
    looks like a test result. Use `--stderr-prefix` to prefix each
    line of standard error to avoid this problem.

`--no-merge`
:   Revert the effects of `--merge`.

`--stderr-prefix STRING`
:   Prefix each line of the test script’s standard error with
    `STRING`. Defaults to the empty string. This option makes
    it possible to avoid parsing ambiguity when `--merge` is in use,
    but it can also be used without `--merge` to make it easier to
    read test log files. When used with `--merge`, users will
    typically want to start `STRING` with ‘`#`’, which starts
    a TAP comment. The ‘`#`’ character is difficult to portably include
    in a `make` variable, so the following quadrigraph
    substitutions, copied from Autoconf (see [Quadrigraphs](https://www.gnu.org/software/autoconf/manual/autoconf.html#Quadrigraphs) in The Autoconf Manual), are performed on `STRING`:

    ‘`@%:@`’
    :   Replaced with ‘`#`’.

    ‘`@&t@`’
    :   Replaced with the empty string. This makes it possible to include the
        literal characters ‘`@%:@`’ in the prefix (‘`@%@&t@:@`’, for
        example).

    For example, the option ‘`--stderr-prefix "@%:@[stderr] "`’
    passed on the command line will prefix each line of the test script’s
    standard error with ‘`#[stderr]` ’.

    For more details on `--stderr-prefix` processing, especially
    the occasional output reordering due to a race condition, see [TAP prefixing stderr](#TAP-prefixing-stderr).

`--diagnostic-string STRING`
:   Change the string that introduces TAP diagnostics from the default value
    of “`#`” to `STRING`. This can be useful if your
    TAP-based test scripts produce verbose output on which they have limited
    control (because, say, the output comes from other tools invoked in the
    scripts), and it might contain text that gets spuriously interpreted as
    TAP diagnostics: such an issue can be solved by redefining the string that
    activates TAP diagnostics to a value you know won’t appear by chance in
    the tests’ output. Note however that this feature is non-standard, as
    the “official” TAP protocol does not allow for such a customization; so
    don’t use it if you can avoid it.

* [TAP setup and examples](#TAP-setup-and-examples)
* [TAP stderr examples](#TAP-stderr-examples)
* [TAP prefixing stderr](#TAP-prefixing-stderr)

---

Next: [TAP stderr examples](#TAP-stderr-examples), Up: [Use TAP with the Automake test harness](#Use-TAP-with-the-Automake-test-harness)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.4.2.1 TAP setup and examples [¶](#TAP-setup-and-examples-1)

Here is an example of how the TAP driver can be set up and used.

```
% cat configure.ac
AC_INIT([GNU Try Tap], [1.0], [bug-automake@gnu.org])
AC_CONFIG_AUX_DIR([build-aux])
AM_INIT_AUTOMAKE([foreign -Wall -Werror])
AC_CONFIG_FILES([Makefile])
AC_REQUIRE_AUX_FILE([tap-driver.sh])
AC_OUTPUT

% cat Makefile.am
TEST_LOG_DRIVER = env AM_TAP_AWK='$(AWK)' $(SHELL) \
                  $(top_srcdir)/build-aux/tap-driver.sh
TESTS = foo.test bar.test baz.test
EXTRA_DIST = $(TESTS)

% cat foo.test
#!/bin/sh
echo 1..4 # Number of tests to be executed.
echo 'ok 1 - Swallows fly'
echo 'not ok 2 - Caterpillars fly # TODO metamorphosis in progress'
echo 'ok 3 - Pigs fly # SKIP not enough acid'
echo '# I just love word plays ...'
echo 'ok 4 - Flies fly too :-)'

% cat bar.test
#!/bin/sh
echo 1..3
echo 'not ok 1 - Bummer, this test has failed.'
echo 'ok 2 - This passed though.'
echo 'Bail out! Ennui kicking in, sorry...'
echo 'ok 3 - This will not be seen.'

% cat baz.test
#!/bin/sh
echo 1..1
echo ok 1
# Exit with error, even if all the tests have been successful.
exit 7

% cp PREFIX/share/automake-APIVERSION/tap-driver.sh .
% autoreconf -vi && ./configure && make check
...
PASS: foo.test 1 - Swallows fly
XFAIL: foo.test 2 - Caterpillars fly # TODO metamorphosis in progress
SKIP: foo.test 3 - Pigs fly # SKIP not enough acid
PASS: foo.test 4 - Flies fly too :-)
FAIL: bar.test 1 - Bummer, this test has failed.
PASS: bar.test 2 - This passed though.
ERROR: bar.test - Bail out! Ennui kicking in, sorry...
PASS: baz.test 1
ERROR: baz.test - exited with status 7
...
Please report to bug-automake@gnu.org
...
% echo exit status: $?
exit status: 1

% env TEST_LOG_DRIVER_FLAGS='--comments --ignore-exit' \
      TESTS='foo.test baz.test' make -e check
...
PASS: foo.test 1 - Swallows fly
XFAIL: foo.test 2 - Caterpillars fly # TODO metamorphosis in progress
SKIP: foo.test 3 - Pigs fly # SKIP not enough acid
# foo.test: I just love word plays...
PASS: foo.test 4 - Flies fly too :-)
PASS: baz.test 1
...
% echo exit status: $?
exit status: 0
```

---

Next: [TAP prefixing stderr](#TAP-prefixing-stderr), Previous: [TAP setup and examples](#TAP-setup-and-examples), Up: [Use TAP with the Automake test harness](#Use-TAP-with-the-Automake-test-harness)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.4.2.2 TAP stderr examples [¶](#TAP-stderr-examples-1)

Here is a sequence of examples showing the issue with merging stdout
and stderr, and the resolution with `--stderr-prefix`. Read
the comments for specifics.

```
% cat stderr-demo.test
#!/bin/sh
echo 'ok 1'
# These sleeps help highlight the reordering of standard output and
# standard error lines that can happen if --merge is not used.
sleep 1
# Note that this is a diagnostic message written to standard error, so
# it should not affect the tests.  However, it will break the tests if
# --merge is used without --stderr-prefix because "Bail out!" will be
# interpreted as a TAP directive.
echo 'Bail out! (this should not actually bail out)' >&2
sleep 1
echo 'ok 2'
echo '1..2'

% make check TESTS=stderr-demo.test
...
PASS: stderr-demo.test 1
PASS: stderr-demo.test 2
...

% cat stderr-demo.log
Bail out! (this should not actually bail out)
ok 1
PASS: stderr-demo.test 1
ok 2
PASS: stderr-demo.test 2
1..2

% make check TESTS=stderr-demo.test \
      TEST_LOG_DRIVER_FLAGS='--merge'
...
PASS: stderr-demo.test 1
ERROR: stderr-demo.test - Bail out! (this should not actually bail out)
...

% make check TESTS=stderr-demo.test \
      TEST_LOG_DRIVER_FLAGS='--merge --stderr-prefix "@%:@[stderr] "'
...
PASS: stderr-demo.test 1
PASS: stderr-demo.test 2
...

% cat stderr-demo.log
ok 1
PASS: stderr-demo.test 1
#[stderr] Bail out! (this should not actually bail out)
ok 2
PASS: stderr-demo.test 2
1..2
```

---

Previous: [TAP stderr examples](#TAP-stderr-examples), Up: [Use TAP with the Automake test harness](#Use-TAP-with-the-Automake-test-harness)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.4.2.3 TAP prefixing stderr [¶](#TAP-prefixing-stderr-1)

When using the above `--stderr-prefix` TAP option, the test
program’s standard error lines might occasionally be processed out of
order relative to the test program’s standard output lines, even when
`--merge` is also used. This can cause confusion when reading a
test’s log because a stdout line might appear earlier (or later) than a
stderr line despite being output by the test program after (or before)
the stderr line. This reordering can be due to many factors, including:

* Output buffering in the test program: The test program might be
  accumulating output data and writing it to the operating system in
  batches, not as soon as it is available. You can try setvbuf(3) to
  change the buffering mode in the test program, or you can add regular
  flushes. OS-specific utilities to control buffering may be available,
  such as stdbuf(1) on GNU/Linux systems.
* Input buffering in the TAP driver: The TAP driver relies on the awk(1)
  utility, and some awk implementations (notably mawk) aggressively buffer
  their input (the test program output). This causes stderr lines to
  appear in the test log much earlier than stdout lines. The
  `--merge` option addresses this by sending the stderr lines to
  awk along with the stdout lines, rendering awk’s input buffering
  irrelevant. (This issue also applies when `--stderr-prefix` is
  not used.)
* Concurrent processing of standard output and standard error: Due to a
  limitation of the shell command language, the TAP driver processes
  standard output in a different thread or process than standard error,
  causing a race condition. Processing order is influenced by how the
  operating system schedules the two threads or processes for execution,
  which is outside the TAP driver’s control.
* Line-based processing: When prefixing standard error, the TAP driver
  waits until a complete line is read from the test program before
  processing the line. This reduces the chances that characters from a
  standard output line are mixed with characters from a standard error
  line, but it delays processing until the line terminator is encountered.

All stderr lines are still processed in order relative to other stderr
lines, and the same goes for the stdout lines relative to other stdout
lines. So the reordering shouldn’t break a package’s TAP tests, since
there’s no reason to output TAP commands to stderr; even more so when
stderr lines are prefixed.

When troubleshooting a particular test, if you find the reordering is
causing confusion, you can omit `--stderr-prefix`, or run the
test program directly (not via the TAP driver) to get a more accurate
understanding of output timing.

Overall, though, the order of processing (and logging) should not
differ from the order of the test program’s write(2) calls by much.
Out-of-order processing only happens if the test program writes to
stdout then stderr but the kernel wakes up the TAP driver’s
thread/process that is reading stderr before it wakes up the TAP
driver’s thread/process that is reading stdout, or vice versa.

---

Next: [Links and external resources on TAP](#Links-and-external-resources-on-TAP), Previous: [Use TAP with the Automake test harness](#Use-TAP-with-the-Automake-test-harness), Up: [Using the TAP test protocol](#Using-the-TAP-test-protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.4.3 Incompatibility with other TAP parsers and drivers [¶](#Incompatibility-with-other-TAP-parsers-and-drivers-1)

For implementation or historical reasons, the TAP driver and harness as
implemented by Automake have some minor compatibility issues with the
mainstream versions, which you should be aware of.

* A `Bail out!` directive doesn’t stop the whole testsuite, but only
  the test script it occurs in. This doesn’t follow TAP specifications,
  but on the other hand it maximizes compatibility (and code sharing) with
  the “hard error” concept of the default testsuite driver.
* The `version` and `pragma` directives are not supported.
* The `--diagnostic-string` option of our driver allows modification of
  the string that introduces TAP diagnostics from the default value
  of “`#`”. The standard TAP protocol currently has no way to
  allow this, so if you use it your diagnostic will be lost to more
  compliant tools like `prove` and `Test::Harness`
* And there are probably some other small and yet undiscovered
  compatibility issues, especially in corner cases or with rare usages.

---

Previous: [Incompatibility with other TAP parsers and drivers](#Incompatibility-with-other-TAP-parsers-and-drivers), Up: [Using the TAP test protocol](#Using-the-TAP-test-protocol)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.4.4 Links and external resources on TAP [¶](#Links-and-external-resources-on-TAP-1)

Here are some links to more extensive official or third-party
documentation and resources about the TAP protocol and related
tools and libraries.

* [‘`Test::Harness`’](https://metacpan.org/pod/Test::Harness),
  the (mostly) official documentation about the TAP format and protocol.
* [`prove`](https://metacpan.org/pod/distribution/Test-Harness/bin/prove),
  the most famous command-line TAP test driver, included in the distribution
  of `perl` and
   [‘`Test::Harness`’](https://metacpan.org/pod/distribution/Test-Harness/lib/Test/Harness.pm).
* The [TAP wiki](https://testanything.org/).
* A “gentle introduction” to testing for Perl coders:
   [‘`Test::Tutorial`’](https://metacpan.org/pod/distribution/Test-Simple/lib/Test/Tutorial.pod).
* [‘`Test::Simple`’](https://metacpan.org/pod/distribution/Test-Simple/lib/Test/Simple.pm)
  and
   [‘`Test::More`’](https://metacpan.org/pod/distribution/Test-Simple/lib/Test/More.pm),
  the standard Perl testing libraries, which are based on TAP.
* [C TAP Harness](https://www.eyrie.org/~eagle/software/c-tap-harness/),
  a C-based project implementing both a TAP producer and a TAP consumer.
* [tap4j](https://tap4j.org/),
  a Java-based project implementing both a TAP producer and a TAP consumer.

---

Next: [Install Tests](#Install-Tests), Previous: [Using the TAP test protocol](#Using-the-TAP-test-protocol), Up: [Support for test suites](#Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.5 DejaGnu Tests [¶](#DejaGnu-Tests-1)

If `dejagnu` (see [Introduction](https://www.gnu.org/software/dejagnu/manual/index.html#Top) in DejaGnu)
appears in `AUTOMAKE_OPTIONS`, then a `dejagnu`-based
test suite is assumed. The variable `DEJATOOL` is a list of
names that are passed, one at a time, as the `--tool` argument
to `runtest` invocations; it defaults to the name of the
package.

The variable `RUNTESTDEFAULTFLAGS` holds the `--tool` and
`--srcdir` flags that are passed to dejagnu by default; this can be
overridden if necessary.

The variables `EXPECT` and `RUNTEST` can
also be overridden to provide project-specific values. For instance,
you will need to do this if you are testing a compiler toolchain,
because the default values do not take into account host and target
names.

The contents of the variable `RUNTESTFLAGS` are passed to the
`runtest` invocation. This is considered a “user variable”
(see [Variables reserved for the user](#User-Variables)). If you need to set `runtest` flags in
`Makefile.am`, you can use `AM_RUNTESTFLAGS` instead.

Automake will generate rules to create a local `site.exp` file,
defining various variables detected by `configure`. This file
is automatically read by DejaGnu. It is OK for the user of a package
to edit this file in order to tune the test suite. However this is
not the place where the test suite author should define new variables:
this should be done elsewhere in the real test suite code.
Especially, `site.exp` should not be distributed.

Still, if the package author has legitimate reasons to extend
`site.exp` at `make` time, he can do so by defining
the variable `EXTRA_DEJAGNU_SITE_CONFIG`; the files listed
there will be considered `site.exp` prerequisites, and their
content will be appended to it (in the same order in which they
appear in `EXTRA_DEJAGNU_SITE_CONFIG`). Note that files are
*not* distributed by default.

For more information regarding DejaGnu test suites, see [The DejaGnu Manual](https://www.gnu.org/software/dejagnu/manual/index.html#Top).

---

Previous: [DejaGnu Tests](#DejaGnu-Tests), Up: [Support for test suites](#Tests)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.6 Install Tests [¶](#Install-Tests-1)

The `installcheck` target is available to the user as a way to
run any tests after the package has been installed. You can add tests
to this by writing an `installcheck-local` rule.

---

Next: [Changing Automake’s Behavior](#Options), Previous: [Support for test suites](#Tests), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 16 Rebuilding Makefiles [¶](#Rebuilding-Makefiles)

Automake generates rules to automatically rebuild `Makefile`s,
`configure`, and other derived files like `Makefile.in`.

If you are using `AM_MAINTAINER_MODE` in `configure.ac`, then
these automatic rebuilding rules are only enabled in maintainer mode.

Sometimes it is convenient to supplement the rebuild rules for
`configure` or `config.status` with additional dependencies.
The variables `CONFIGURE_DEPENDENCIES` and
`CONFIG_STATUS_DEPENDENCIES` can be used to list these extra
dependencies. These variables should be defined in all
`Makefile`s of the tree (because these two rebuild rules are
output in all of them), so it is safer and easier to `AC_SUBST` them
from `configure.ac`. For instance, the following statement will
cause `configure` to be rerun each time `version.sh` is
changed.

```
AC_SUBST([CONFIG_STATUS_DEPENDENCIES], ['$(top_srcdir)/version.sh'])
```

Note the ‘`$(top_srcdir)/`’ in the file name. Since this variable
is to be used in all `Makefile`s, its value must be sensible at
any level in the build hierarchy.

Beware not to mistake `CONFIGURE_DEPENDENCIES` for
`CONFIG_STATUS_DEPENDENCIES`.

`CONFIGURE_DEPENDENCIES` adds dependencies to the
`configure` rule, whose effect is to run `autoconf`.
This variable should be seldom used, because `automake`
already tracks `m4_include`d files, but it can be useful when
playing tricky games with `m4_esyscmd` or similar
non-recommendable macros with side effects. Also be aware that
interactions of this variable with the [autom4te
cache](https://www.gnu.org/software/autoconf/manual/autoconf.html#Autom4te-Cache) in The Autoconf Manual are quite problematic and can
cause subtle breakage, so you might want to disable the cache if you
want to use `CONFIGURE_DEPENDENCIES`.

`CONFIG_STATUS_DEPENDENCIES` adds dependencies to the
`config.status` rule, whose effect is to run `configure`.
This variable should therefore carry any non-standard source that may
be read as a side effect of running `configure`, like `version.sh`
in the example above.

Speaking of `version.sh` scripts, we recommend against them
today. They are mainly used when the version of a package is updated
automatically by a script (e.g., in daily builds). Here is what some
old-style `configure.ac`s may look like:

```
AC_INIT
. $srcdir/version.sh
AM_INIT_AUTOMAKE([name], $VERSION_NUMBER)
...
```

Here, `version.sh` is a shell fragment that sets
`VERSION_NUMBER`. The problem with this example is that
`automake` cannot track dependencies (listing `version.sh`
in `CONFIG_STATUS_DEPENDENCIES`, and distributing this file is up
to the user), and that it uses the obsolete form of `AC_INIT` and
`AM_INIT_AUTOMAKE`. Upgrading to the new syntax is not
straightforward, because shell variables are not allowed in
`AC_INIT`’s arguments. We recommend that `version.sh` be
replaced by an M4 file that is included by `configure.ac`:

```
m4_include([version.m4])
AC_INIT([name], VERSION_NUMBER)
AM_INIT_AUTOMAKE
...
```

Here `version.m4` could contain something like
‘`m4_define([VERSION_NUMBER], [1.2])`’. The advantage of this
second form is that `automake` will take care of the
dependencies when defining the rebuild rule, and will also distribute
the file automatically. An inconvenience is that `autoconf`
will now be rerun each time the version number is bumped, when only
`configure` had to be rerun in the previous setup.

GNU Make, at least, has an option `--always-make` which tells
Make to consider that all targets are out of date. This interacts
badly with Automake-generated Makefiles, which implement their own
careful rules for when to regenerate Makefiles, as described above.
The result is an endless loop, or other poor behavior. The only thing
to do, as far as we know, is to refrain from using
`--always-make`.

---

Next: [Miscellaneous Rules](#Miscellaneous), Previous: [Rebuilding Makefiles](#Rebuilding), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 17 Changing Automake’s Behavior [¶](#Changing-Automake_0027s-Behavior)

* [Options generalities](#Options-generalities)
* [List of Automake options](#List-of-Automake-options)

---

Next: [List of Automake options](#List-of-Automake-options), Up: [Changing Automake’s Behavior](#Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 17.1 Options generalities [¶](#Options-generalities-1)

Various features of Automake can be controlled by options. Except where
noted otherwise, options can be specified in one of several ways. Most
options can be applied on a per-`Makefile` basis when listed in a
special `Makefile` variable named `AUTOMAKE_OPTIONS`. Some
of these options only make sense when specified in the toplevel
`Makefile.am` file. Options are applied globally to all processed
`Makefile` files when listed in the first argument of
`AM_INIT_AUTOMAKE` in `configure.ac`, and some options which
require changes to the `configure` script can only be specified
there. These are annotated below.

As a general rule, options specified in `AUTOMAKE_OPTIONS` take
precedence over those specified in `AM_INIT_AUTOMAKE`, which in
turn take precedence over those specified on the command line.

Also, some care must be taken about the interactions among strictness
level and warning categories. As a general rule, strictness-implied
warnings are overridden by those specified by explicit options. For
example, even if ‘`portability`’ warnings are disabled by default
in `foreign` strictness, a usage like this will end up enabling
them:

```
AUTOMAKE_OPTIONS = -Wportability foreign
```

However, a strictness level specified in a higher-priority context
will override all the explicit warnings specified in a lower-priority
context. For example, if `configure.ac` contains:

```
AM_INIT_AUTOMAKE([-Wportability])
```

and `Makefile.am` contains:

```
AUTOMAKE_OPTIONS = foreign
```

then ‘`portability`’ warnings will be *disabled* in
`Makefile.am`.

---

Previous: [Options generalities](#Options-generalities), Up: [Changing Automake’s Behavior](#Options)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 17.2 List of Automake options [¶](#List-of-Automake-options-1)

`gnits` [¶](#index-Option_002c-gnits)

`gnu`

`foreign`
:   Set the strictness as appropriate. See [Strictness](#Strictness).
    The `gnits` option also implies options `readme-alpha` and
    `check-news`.

`check-news` [¶](#index-Option_002c-check_002dnews)
:   Cause ‘`make dist`’ to fail unless the current version number appears
    in the first few lines of the `NEWS` file.

`dejagnu` [¶](#index-Option_002c-dejagnu)
:   Cause `dejagnu`-specific rules to be generated. See [DejaGnu Tests](#DejaGnu-Tests).

`dist-bzip2` [¶](#index-Option_002c-dist_002dbzip2)
:   Hook `dist-bzip2` to `dist`.

`dist-bzip3` [¶](#index-Option_002c-dist_002dbzip3)
:   Hook `dist-bzip3` to `dist`.

`dist-lzip` [¶](#index-Option_002c-dist_002dlzip)
:   Hook `dist-lzip` to `dist`.

`dist-xz` [¶](#index-Option_002c-dist_002dxz)
:   Hook `dist-xz` to `dist`.

`dist-zip` [¶](#index-Option_002c-dist_002dzip)
:   Hook `dist-zip` to `dist`.

`dist-zstd` [¶](#index-Option_002c-dist_002dzstd)
:   Hook `dist-zstd` to `dist`.

`dist-shar` [¶](#index-Option_002c-dist_002dshar)
:   Hook `dist-shar` to `dist`. Use of this option
    is deprecated, as the ‘`shar`’ format is obsolescent and
    problematic. Support for it will be removed altogether in
    Automake 2.0.

`dist-tarZ` [¶](#index-Option_002c-dist_002dtarZ)
:   Hook `dist-tarZ` to `dist`. Use of this option
    is deprecated, as the ‘`compress`’ program is obsolete.
    Support for it will be removed altogether in Automake 2.0.

`filename-length-max=max` [¶](#index-Option_002c-filename_002dlength_002dmax_003dmax)
:   Abort ‘`make dist`’ if the distribution contains file names longer
    than max characters; names of exactly max characters are
    accepted. What is measured is the name under which `tar` will
    store the file, so it includes the `package-version/`
    prefix.

    How long a file name may be depends on the archive format in use
    (see [tar-formats](#tar_002dformats)).

    * The `tar-v7` format supports at most 99 characters, hence the
      customary `filename-length-max=99`.
    * The `tar-ustar` format, the default as of Automake 1.18, stores
      file names of up to 256 characters, provided they can be split at a
      directory separator into a prefix of at most 155 characters and a
      remainder of at most 100; any name of at most 100 characters therefore
      fits, and `filename-length-max=155` is a safe choice as long as
      no single component of a file name exceeds 100 characters.
      `tar` programs should fail if a file name does not meet these
      rules, and Automake will notice that failure and abort the `make
      dist`.
    * The `tar-pax` format does not limit file name lengths at all.

    So, unless you distribute a V7 archive,
    `filename-length-max=99` is stricter than necessary.

    This option should be used in the top-level `Makefile.am` or as
    an argument of `AM_INIT_AUTOMAKE` in `configure.ac`; it will
    be ignored otherwise. It will also be ignored in sub-packages of
    nested packages (see [Nesting Packages](#Subpackages)).

`info-in-builddir` [¶](#index-Option_002c-info_002din_002dbuilddir)
:   Instruct Automake to place the generated `.info` files in the
    `builddir` rather than in the `srcdir`. Note that this
    might make VPATH builds with some non-GNU make implementations more
    brittle.

`no-define` [¶](#index-Option_002c-no_002ddefine)
:   This option is meaningful only when passed as an argument to
    `AM_INIT_AUTOMAKE`. It will prevent the `PACKAGE` and
    `VERSION` variables from being `AC_DEFINE`d. But notice
    that they will remain defined as shell variables in the generated
    `configure`, and as make variables in the generated
    `Makefile`; this is deliberate, and required for backward
    compatibility.

`no-dependencies` [¶](#index-Option_002c-no_002ddependencies)
:   This is similar to using `--ignore-deps` on the command line,
    but is useful for those situations where you don’t have the necessary
    bits to make automatic dependency tracking work
    (see [Automatic dependency tracking](#Dependencies)). In this case the effect is to effectively
    disable automatic dependency tracking.

`no-dist` [¶](#index-Option_002c-no_002ddist)
:   Don’t emit any code related to `dist` target. This is useful
    when a package has its own method for making distributions.

`no-dist-built-sources` [¶](#index-Option_002c-no_002ddist_002dbuilt_002dsources)
:   Don’t build `BUILT_SOURCES` as part of `dist`. This option
    can be set if building the distribution only requires the source
    files, and doesn’t compile anything as a side-effect. The default is
    for ‘`$(distdir)`’ to depend on ‘`$(BUILT_SOURCES)`’ because it
    is common, at least among GNU packages, to want to build the program
    to generate man pages with `help2man` (see [Errors with `distclean`](#Errors-with-distclean)). Admittedly the default behavior should perhaps be to
    omit the dependency, but to preserve compatibility, we don’t want to
    change it now.

`no-dist-gzip` [¶](#index-Option_002c-no_002ddist_002dgzip)
:   Do not hook `dist-gzip` to `dist`.

`no-exeext` [¶](#index-Option_002c-no_002dexeext)
:   If your `Makefile.am` defines a rule for target `foo`, it
    will override a rule for a target named ‘`foo$(EXEEXT)`’. This is
    necessary when `EXEEXT` is found to be empty. However, by
    default `automake` will generate an error for this use. The
    `no-exeext` option will disable this error. This is intended for
    use only where it is known in advance that the package will not be
    ported to Windows, or any other operating system using extensions on
    executables.

`no-installinfo` [¶](#index-Option_002c-no_002dinstallinfo-1)
:   The generated `Makefile.in` will not cause info pages to be built
    or installed by default. However, `info` and `install-info`
    targets will still be available. This option is disallowed at
    `gnu` strictness and above.

`no-installman` [¶](#index-Option_002c-no_002dinstallman-1)
:   The generated `Makefile.in` will not cause man pages to be
    installed by default. However, an `install-man` target will still
    be available for optional installation. This option is disallowed at
    `gnu` strictness and above.

`nostdinc` [¶](#index-Option_002c-nostdinc)
:   This option can be used to disable the standard `-I` options that
    are ordinarily automatically provided by Automake.

`no-texinfo.tex` [¶](#index-Option_002c-no_002dtexinfo_002etex)
:   Don’t require `texinfo.tex`, even if there are texinfo files in
    this directory.

`posix` [¶](#index-Option_002c-posix)
:   Generate the special target `.POSIX` as the first non-comment
    line in the `Makefile.in` files. This alters the behavior of the
    `make` program. See [Special targets](https://www.gnu.org/software/make/manual/make.html#Special-targets) in The GNU Make
    Manual.

`serial-tests` [¶](#index-Option_002c-serial_002dtests)
:   Enable the older serial test suite harness for `TESTS` (see [Older (and discouraged) Serial Test Harness](#Serial-Test-Harness), for more information).

`parallel-tests` [¶](#index-Option_002c-parallel_002dtests)
:   Enable test suite harness for `TESTS` that can run tests in parallel
    (see [Parallel Test Harness](#Parallel-Test-Harness), for more information). This option is
    only kept for backward-compatibility, since the parallel test harness is
    the default now.

`readme-alpha` [¶](#index-Option_002c-readme_002dalpha)
:   If this release is an alpha release, and the file `README-alpha`
    exists, then it will be added to the distribution. If this option is
    given, version numbers are expected to follow one of two forms. The
    first form is ‘`major.minor.alpha`’, where each
    element is a number; the final period and number should be left off for
    non-alpha releases. The second form is
    ‘`major.minoralpha`’, where alpha is a
    letter; it should be omitted for non-alpha releases.

`std-options` [¶](#index-Options_002c-std_002doptions)
:   Make the `installcheck` rule check that installed scripts and
    programs support the `--help` and `--version` options.
    This also provides a basic check that the program’s
    run-time dependencies are satisfied after installation.

    In a few situations, programs (or scripts) have to be exempted from this
    test. For instance, `false` (from GNU coreutils) is never
    successful, even for `--help` or `--version`. You can list
    such programs in the variable `AM_INSTALLCHECK_STD_OPTIONS_EXEMPT`.
    Programs (not scripts) listed in this variable should be suffixed by
    ‘`$(EXEEXT)`’ for the sake of Windows or OS/2. For instance, suppose we
    build `false` as a program but `true.sh` as a script, and that
    neither of them support `--help` or `--version`:

    ```
    AUTOMAKE_OPTIONS = std-options
    bin_PROGRAMS = false ...
    bin_SCRIPTS = true.sh ...
    AM_INSTALLCHECK_STD_OPTIONS_EXEMPT = false$(EXEEXT) true.sh
    ```

`subdir-objects` [¶](#index-Options_002c-subdir_002dobjects)
:   If this option is specified, then objects are placed into the
    subdirectory of the build directory corresponding to the subdirectory of
    the source file. For instance, if the source file is
    `subdir/file.cxx`, then the output file would be
    `subdir/file.o`. See [Program and Library Variables](#Program-and-Library-Variables).

`tar-v7` [¶](#index-Option_002c-tar_002dv7)

`tar-ustar`

`tar-pax`
:   These three mutually exclusive options select the tar format to use
    when generating tarballs with ‘`make dist`’. (The tar file created
    is then compressed according to the set of `no-dist-gzip`,
    `dist-bzip2`, `dist-bzip3`, `dist-lzip`, `dist-xz`,
    `dist-zstd` and `dist-tarZ` options in use.)

    These options must be passed as arguments to `AM_INIT_AUTOMAKE`
    (see [Autoconf macros supplied with Automake](#Macros)) because they can require additional configure checks.
    Automake will complain if it sees such options in an
    `AUTOMAKE_OPTIONS` variable.

    `tar-v7` selects the old V7 tar format. This was the historical
    default. This antiquated format is understood by all tar
    implementations and supports file names with up to 99 characters. When
    given longer file names some tar implementations will diagnose the
    problem while others will generate broken tarballs or use non-portable
    extensions. Furthermore, the V7 format cannot store empty
    directories. When using this format, consider using the
    `filename-length-max=99` option to catch file names too long.

    `tar-ustar` selects the ustar format defined by POSIX
    1003.1-1988. This format became the default in Automake version 1.18
    (released in 2025).
    As of 2018, it is supported by the native `tar` command on
    GNU, FreeBSD, NetBSD, OpenBSD, AIX, HP-UX, and Solaris, at least.
    It fully supports empty directories. It can store file names with up
    to 256 characters, provided that the file name can be split at
    directory separator in two parts, first of them being at most 155
    bytes long. So, in most cases the maximum file name length will be
    shorter than 256 characters.

    `tar-pax` selects the new pax interchange format defined by POSIX
    1003.1-2001. It does not limit the length of file names. However,
    this format is relatively young and should probably be restricted to
    packages that target modern platforms.
    As of 2018, this format is supported by the native `tar` command only
    on GNU, FreeBSD, and OpenBSD systems; it is not supported by the native
    `tar` command on NetBSD, AIX, HP-UX, or Solaris.
    There are moves to
    change the pax format in an upward-compatible way, so this option may
    refer to a more recent version in the future.

    See [Controlling the Archive Format](https://www.gnu.org/software/tar/manual/tar.html#Formats) in GNU Tar, for
    further discussion about tar formats.

    `configure` knows several ways to construct these formats. It
    will not abort if it cannot find a tool up to the task (so that the
    package can still be built), but ‘`make dist`’ will fail.

version [¶](#index-Option_002c-version)
:   A version number (e.g., ‘`0.30`’) can be specified. If Automake is not
    the same version or
    newer than the version specified, creation of the `Makefile.in`
    will be suppressed.

`-Wcategory` or `--warnings=category` [¶](#index-Option_002c-warnings)
:   These options behave exactly like their command-line counterpart
    (see [Creating a `Makefile.in`: Invoking `automake`](#automake-Invocation)). This allows you to enable or disable some
    warning categories on a per-file basis. You can also setup some warnings
    for your entire project; for instance, try ‘`AM_INIT_AUTOMAKE([-Wall])`’
    in your `configure.ac`.

Unrecognized options are diagnosed by `automake`.

If you want an option to apply to all the files in the tree, you can use
the `AM_INIT_AUTOMAKE` macro in `configure.ac`.
See [Autoconf macros supplied with Automake](#Macros).

---

Next: [Include](#Include), Previous: [Changing Automake’s Behavior](#Options), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 18 Miscellaneous Rules [¶](#Miscellaneous-Rules)

There are a few rules and variables that didn’t fit anywhere else.

* [Interfacing to `etags`](#Tags)
* [Handling new file extensions](#Suffixes)

---

Next: [Handling new file extensions](#Suffixes), Up: [Miscellaneous Rules](#Miscellaneous)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 18.1 Interfacing to `etags` [¶](#Interfacing-to-etags)

Automake will generate rules to generate `TAGS` files for use with
GNU Emacs under some circumstances.

If any C, C++ or Fortran 77 source code or headers are present, then
`tags` and `TAGS` rules will be generated for the directory.
All files listed using the `_SOURCES`, `_HEADERS`, and
`_LISP` primaries will be used to generate tags. Generated
source files that are not distributed must be declared in variables
like `nodist_noinst_HEADERS` or `nodist_prog_SOURCES`
or they will be ignored.

A `tags` rule will be output at the topmost directory of a
multi-directory package. When run from this topmost directory,
‘`make tags`’ will generate a `TAGS` file that includes by
reference all `TAGS` files from subdirectories.

The `tags` rule will also be generated if the variable
`ETAGS_ARGS` is defined. This variable is intended for use in
directories that contain taggable source that `etags` does
not understand. The user can use the `ETAGSFLAGS` to pass
additional flags to `etags`; `AM_ETAGSFLAGS` is also
available for use in `Makefile.am`. The variable `ETAGS`
is the name of the program to invoke (by default `etags`).

Here is how Automake generates tags for its source, and for nodes in its
Texinfo file:

```
ETAGS_ARGS = automake.in --lang=none \
 --regex='/^@node[ \t]+\([^,]+\)/\1/' automake.texi
```

If you add file names to `ETAGS_ARGS`, you will probably also
want to define `TAGS_DEPENDENCIES`. The contents of this variable
are added directly to the dependencies for the `tags` rule.

Automake also generates a `ctags` rule that can be used to
build `vi`-style `tags` files. The variable `CTAGS`
is the name of the program to invoke (by default `ctags`);
`CTAGSFLAGS` can be used by the user to pass additional flags,
and `AM_CTAGSFLAGS` can be used by the `Makefile.am`.

Automake will also generate an `ID` rule that will run
`mkid` on the source. This is only supported on a
directory-by-directory basis.

Similarly, the `cscope` rule will create a list of all the source
files in the tree and run `cscope` to build an inverted index
database. The variable `CSCOPE` is the name of the program to invoke
(by default `cscope`); `CSCOPEFLAGS` and
`CSCOPE_ARGS` can be used by the user to pass additional flags and
file names respectively, while `AM_CSCOPEFLAGS` can be used by the
`Makefile.am`. Note that, currently, the Automake-provided
`cscope` support, when used in a VPATH build, might not work well
with non-GNU make implementations (especially with make implementations
performing [VPATH rewrites](https://www.gnu.org/software/autoconf/manual/autoconf.html#Automatic-Rule-Rewriting) in The Autoconf Manual).

Finally, Automake also emits rules to support the
[GNU Global Tags program](https://www.gnu.org/software/global/).
The `GTAGS` rule runs Global Tags and puts the
result in the top build directory. The variable `GTAGS_ARGS`
holds arguments that are passed to `gtags`.

---

Previous: [Interfacing to `etags`](#Tags), Up: [Miscellaneous Rules](#Miscellaneous)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 18.2 Handling new file extensions [¶](#Handling-new-file-extensions)

It is sometimes useful to introduce a new implicit rule to handle a file
type that Automake does not know about.

For instance, suppose you had a compiler that could compile `.foo`
files to `.o` files. You would simply define a suffix rule for
your language:

```
.foo.o:
        foocc -c -o $@ $<
```

Then you could directly use a `.foo` file in a `_SOURCES`
variable and expect the correct results:

```
bin_PROGRAMS = doit
doit_SOURCES = doit.foo
```

That is the simpler and more common case. In other cases, you have to
help Automake to figure out which extensions you are defining your
suffix rule for. This usually happens when your extension does not
start with a dot. Then, you have to put the list of new suffixes in
the `SUFFIXES` variable *before* you define your implicit
rule.

For instance, the following definition prevents Automake from misinterpreting
the ‘`.idlC.cpp:`’ rule as an attempt to transform `.idlC` files into
`.cpp` files.

```
SUFFIXES = .idl C.cpp
.idlC.cpp:
        # whatever
```

As you may have noted, the `SUFFIXES` variable behaves like the
`.SUFFIXES` special target of `make`. You should not touch
`.SUFFIXES` yourself, but use `SUFFIXES` instead and let
Automake generate the suffix list for `.SUFFIXES`. Any given
`SUFFIXES` go at the start of the generated suffixes list, followed
by Automake generated suffixes not already in the list.

Automake disables the Make program’s built-in rules with a
`.SUFFIXES:` rule, and then adds whatever suffixes are
necessary. Automake also disables GNU Make’s built-in pattern rules.

---

Next: [Conditionals](#Conditionals), Previous: [Miscellaneous Rules](#Miscellaneous), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 19 Include [¶](#Include-1)

Automake supports an `include` directive that can be used to
include other `Makefile` fragments when `automake` is run.
Note that these fragments are read and interpreted by `automake`,
not by `make`. As with conditionals, `make` has no idea that
`include` is in use.

There are two forms of `include`:

`include $(srcdir)/file`
:   Include a fragment that is found relative to the current source
    directory.

`include $(top_srcdir)/file`
:   Include a fragment that is found relative to the top source directory.

Note that if a fragment is included inside a conditional, then the
condition applies to the entire contents of that fragment.

`Makefile` fragments included this way are always distributed because
they are needed to rebuild `Makefile.in`.

Inside a fragment, the construct `%reldir%` is replaced with the
directory of the fragment relative to the base `Makefile.am`.
Similarly, `%canon_reldir%` is replaced with the canonicalized
(see [How derived variables are named](#Canonicalization)) form of `%reldir%`. As a convenience,
`%D%` is a synonym for `%reldir%`, and `%C%`
is a synonym for `%canon_reldir%`.

A special feature is that if the fragment is in the same directory as
the base `Makefile.am` (i.e., `%reldir%` is `.`), then:

`%reldir%`
:   together with a following slash expands to the empty string, otherwise
    it expands to a dot;

`%canon_reldir%`
:   together with a following underscore expands to the empty string, otherwise
    it expands to an underscore.

Thus, a `Makefile` fragment might look like this:

```
bin_PROGRAMS += %reldir%/mumble
%canon_reldir%_mumble_SOURCES = %reldir%/one.c
```

---

Next: [Silencing `make`](#Silencing-Make), Previous: [Include](#Include), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 20 Conditionals [¶](#Conditionals-1)

Automake supports a simple type of conditional.

These conditionals are not the same as conditionals in
GNU Make. Automake conditionals are checked at configure time by the
`configure` script, and affect the translation from
`Makefile.in` to `Makefile`. They are based on options passed
to `configure` and on results that `configure` has discovered
about the host system. GNU Make conditionals are checked at `make`
time, and are based on variables passed to the make program or defined
in the `Makefile`.

Automake conditionals will work with any make program.

* [Usage of Conditionals](#Usage-of-Conditionals)
* [Limits of Conditionals](#Limits-of-Conditionals)

---

Next: [Limits of Conditionals](#Limits-of-Conditionals), Up: [Conditionals](#Conditionals)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
