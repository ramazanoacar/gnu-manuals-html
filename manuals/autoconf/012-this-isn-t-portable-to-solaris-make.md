# This isn't portable to Solaris make.
VPATH = ../pkg/src
f.c: if.c
        cp `test -f if.c || echo $(VPATH)/`if.c f.c
```

However, the “prerequisite rewriting” still applies here. So if
`if.c` is in `../pkg/src`, Solaris `make`
executes

```
cp `test -f ../pkg/src/if.c || echo ../pkg/src/`if.c f.c
```

which reduces to

```
cp if.c f.c
```

and thus fails. Oops.

A simple workaround, and good practice anyway, is to use ‘`$?`’ and
‘`$@`’ when possible:

```
VPATH = ../pkg/src
f.c: if.c
        cp $? $@
```

but this does not generalize well to commands with multiple
prerequisites. A more general workaround is to rewrite the rule so that
the prerequisite `if.c` never appears as a plain word. For
example, these three rules would be safe, assuming `if.c` is in
`../pkg/src` and the other files are in the working directory:

```
VPATH = ../pkg/src
f.c: if.c f1.c
        cat `test -f ./if.c || echo $(VPATH)/`if.c f1.c >$@
g.c: if.c g1.c
        cat `test -f 'if.c' || echo $(VPATH)/`if.c g1.c >$@
h.c: if.c h1.c
        cat `test -f "if.c" || echo $(VPATH)/`if.c h1.c >$@
```

Things get worse when your prerequisites are in a macro.

```
VPATH = ../pkg/src
HEADERS = f.h g.h h.h
install-HEADERS: $(HEADERS)
        for i in $(HEADERS); do \
          $(INSTALL) -m 644 \
            `test -f $$i || echo $(VPATH)/`$$i \
            $(DESTDIR)$(includedir)/$$i; \
        done
```

The above `install-HEADERS` rule is not Solaris-proof because `for
i in $(HEADERS);` is expanded to `for i in f.h g.h h.h;`
where `f.h` and `g.h` are plain words and are hence
subject to `VPATH` adjustments.

If the three files are in `../pkg/src`, the rule is run as:

```
for i in ../pkg/src/f.h ../pkg/src/g.h h.h; do \
  install -m 644 \
     `test -f $i || echo ../pkg/src/`$i \
     /usr/local/include/$i; \
done
```

where the two first `install` calls fail. For instance,
consider the `f.h` installation:

```
install -m 644 \
  `test -f ../pkg/src/f.h || \
    echo ../pkg/src/ \
  `../pkg/src/f.h \
  /usr/local/include/../pkg/src/f.h;
```

It reduces to:

```
install -m 644 \
  ../pkg/src/f.h \
  /usr/local/include/../pkg/src/f.h;
```

Note that the manual `VPATH` search did not cause any problems here;
however this command installs `f.h` in an incorrect directory.

Trying to quote `$(HEADERS)` in some way, as we did for
`foo.c` a few makefiles ago, does not help:

```
install-HEADERS: $(HEADERS)
        headers='$(HEADERS)'; \
        for i in $$headers; do \
          $(INSTALL) -m 644 \
            `test -f $$i || echo $(VPATH)/`$$i \
            $(DESTDIR)$(includedir)/$$i; \
        done
```

Now, `headers='$(HEADERS)'` macro-expands to:

```
headers='f.h g.h h.h'
```

but `g.h` is still a plain word. (As an aside, the idiom
`headers='$(HEADERS)'; for i in $$headers;` is a good
idea if `$(HEADERS)` can be empty, because some shells diagnose a
syntax error on `for i in;`.)

One workaround is to strip this unwanted `../pkg/src/` prefix manually:

```
VPATH = ../pkg/src
HEADERS = f.h g.h h.h
install-HEADERS: $(HEADERS)
        headers='$(HEADERS)'; \
        for i in $$headers; do \
          i=`expr "$$i" : '$(VPATH)/\(.*\)'`;
          $(INSTALL) -m 644 \
            `test -f $$i || echo $(VPATH)/`$$i \
            $(DESTDIR)$(includedir)/$$i; \
        done
```

Automake does something similar.

---

Previous: [Automatic Rule Rewriting](#Automatic-Rule-Rewriting), Up: [`VPATH` and Make](#VPATH-and-Make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 12.18.5 Make Target Lookup [¶](#Make-Target-Lookup-1)

GNU `make` uses a complex algorithm to decide when it
should use files found via a `VPATH` search. See [How Directory Searches are Performed](https://www.gnu.org/software/make/manual/make.html#Search-Algorithm) in The GNU Make
Manual.

If a target needs to be rebuilt, GNU `make` discards the
file name found during the `VPATH` search for this target, and
builds the file locally using the file name given in the makefile.
If a target does not need to be rebuilt, GNU `make` uses the
file name found during the `VPATH` search.

Other `make` implementations, like NetBSD `make`, are
easier to describe: the file name found during the `VPATH` search
is used whether the target needs to be rebuilt or not. Therefore
new files are created locally, but existing files are updated at their
`VPATH` location.

OpenBSD and FreeBSD `make`, however,
never perform a
`VPATH` search for a dependency that has an explicit rule.
This is extremely annoying.

When attempting a `VPATH` build for an autoconfiscated package
(e.g., `mkdir build && cd build && ../configure`), this means
GNU
`make` builds everything locally in the `build`
directory, while BSD `make` builds new files locally and
updates existing files in the source directory.

```
$ cat Makefile
VPATH = ..
all: foo.x bar.x
foo.x bar.x: newer.x
        @echo Building $@
$ touch ../bar.x
$ touch ../newer.x
$ make        # GNU make
Building foo.x
Building bar.x
$ pmake       # NetBSD make
Building foo.x
Building ../bar.x
$ fmake       # FreeBSD make, OpenBSD make
Building foo.x
Building bar.x
$ make        # GNU make
Building foo.x
$ pmake       # NetBSD make
Building foo.x
$ fmake       # FreeBSD make, OpenBSD make
Building foo.x
Building bar.x
```

Note how NetBSD `make` updates `../bar.x` in its
VPATH location, and how FreeBSD and OpenBSD
`make` always
update `bar.x`, even when `../bar.x` is up to date.

Another point worth mentioning is that once GNU `make` has
decided to ignore a `VPATH` file name (e.g., it ignored
`../bar.x` in the above example) it continues to ignore it when
the target occurs as a prerequisite of another rule.

The following example shows that GNU `make` does not look up
`bar.x` in `VPATH` before performing the `.x.y` rule,
because it ignored the `VPATH` result of `bar.x` while running
the `bar.x: newer.x` rule.

```
$ cat Makefile
VPATH = ..
all: bar.y
bar.x: newer.x
        @echo Building $@
.SUFFIXES: .x .y
.x.y:
        cp $< $@
$ touch ../bar.x
$ touch ../newer.x
$ make        # GNU make
Building bar.x
cp bar.x bar.y
cp: cannot stat 'bar.x': No such file or directory
make: *** [bar.y] Error 1
$ pmake       # NetBSD make
Building ../bar.x
cp ../bar.x bar.y
$ rm bar.y
$ fmake       # FreeBSD make, OpenBSD make
echo Building bar.x
cp bar.x bar.y
cp: cannot stat 'bar.x': No such file or directory
*** Error code 1
```

Note that if you drop away the command from the `bar.x: newer.x`
rule, GNU `make` magically starts to work: it
knows that `bar.x` hasn’t been updated, therefore it doesn’t
discard the result from `VPATH` (`../bar.x`) in succeeding
uses. FreeBSD and OpenBSD still don’t work, though.

```
$ cat Makefile
VPATH = ..
all: bar.y
bar.x: newer.x
.SUFFIXES: .x .y
.x.y:
        cp $< $@
$ touch ../bar.x
$ touch ../newer.x
$ make        # GNU make
cp ../bar.x bar.y
$ rm bar.y
$ pmake       # NetBSD make
cp ../bar.x bar.y
$ rm bar.y
$ fmake       # FreeBSD make, OpenBSD make
cp bar.x bar.y
cp: cannot stat 'bar.x': No such file or directory
*** Error code 1
```

It seems the sole solution that would please every `make`
implementation is to never rely on `VPATH` searches for targets.
In other words, `VPATH` should be reserved to sources that are not built.

---

Next: [Timestamp Resolution and Make](#Timestamps-and-Make), Previous: [`VPATH` and Make](#VPATH-and-Make), Up: [Portable Make Programming](#Portable-Make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 12.19 Single Suffix Rules and Separated Dependencies [¶](#Single-Suffix-Rules-and-Separated-Dependencies)

A *Single Suffix Rule* is basically a usual suffix (inference) rule
(‘`.from.to:`’), but which *destination* suffix is empty
(‘`.from:`’).

*Separated dependencies* simply refers to listing the prerequisite
of a target, without defining a rule. Usually one can list on the one
hand side, the rules, and on the other hand side, the dependencies.

Solaris `make` does not support separated dependencies for
targets defined by single suffix rules:

```
$ cat Makefile
.SUFFIXES: .in
foo: foo.in
.in:
        cp $< $@
$ touch foo.in
$ make
$ ls
Makefile  foo.in
```

while GNU Make does:

```
$ gmake
cp foo.in foo
$ ls
Makefile  foo       foo.in
```

Note it works without the ‘`foo: foo.in`’ dependency.

```
$ cat Makefile
.SUFFIXES: .in
.in:
        cp $< $@
$ make foo
cp foo.in foo
```

and it works with double suffix inference rules:

```
$ cat Makefile
foo.out: foo.in
.SUFFIXES: .in .out
.in.out:
        cp $< $@
$ make
cp foo.in foo.out
```

As a result, in such a case, you have to write target rules.

---

Previous: [Single Suffix Rules and Separated Dependencies](#Single-Suffix-Rules), Up: [Portable Make Programming](#Portable-Make)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 12.20 Timestamp Resolution and Make [¶](#Timestamp-Resolution-and-Make)

Traditionally, file timestamps had 1-second resolution, and
`make` used those timestamps to determine whether one file was
newer than the other. However, many modern file systems have
timestamps with 1-nanosecond resolution. Some `make`
implementations look at the entire timestamp; others ignore the
fractional part, which can lead to incorrect results. Normally this
is not a problem, but in some extreme cases you may need to use tricks
like ‘`sleep 1`’ to work around timestamp truncation bugs.

Traditionally, commands like ‘`cp -p`’ and ‘`touch -r`’ did not copy
file timestamps to their full resolutions (see [Limitations of Usual
Tools](#touch)). Hence you should be wary of rules like this:

```
dest: src
        cp -p src dest
```

as `dest` often appears to be older than `src` after the
timestamp is truncated, and this can cause `make` to do
needless rework the next time it is invoked. To work around this
problem, you can use a timestamp file, e.g.:

```
dest-stamp: src
        cp -p src dest
        echo >dest-stamp
```

Apart from timestamp resolution, there are also differences in handling
equal timestamps. Although POSIX suggests that `make` update a
target with the same timestamp as its newest prerequisite, only
HP-UX `make` is known to do so; GNU and other `make`
implementations do not update, a behavior that POSIX also allows.

The HP-UX behavior can cause spurious rebuilds
and this in turn can cause `make` to fail if it tries to rebuild
generated files in a possibly read-only source tree with tools not
present on the build machine. Conversely, if a source file is updated
immediately after running `make` in a file system with
low-resolution timestamps, the GNU behavior can cause a later
`make` to neglect rebuilds and quietly generate incorrect results.
To avoid both these problems, use GNU `make` on a platform
with high-resolution timestamps.

---

Next: [Manual Configuration](#Manual-Configuration), Previous: [Portable Make Programming](#Portable-Make), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 13 Portable C and C++ Programming [¶](#Portable-C-and-C_002b_002b-Programming)

C and C++ programs often use low-level features of the underlying
system, and therefore are often more difficult to make portable to other
platforms.

Several standards have been developed to help make your programs more
portable. If you write programs with these standards in mind, you can
have greater confidence that your programs work on a wide variety
of systems.
[Language
Standards Supported by GCC](https://gcc.gnu.org/onlinedocs/gcc/Standards.html)
for a list of C-related standards. Many programs also assume the
[POSIX standard](https://en.wikipedia.org/wiki/POSIX).

The first widely used C variant was K&R C, which predates any C
standard. K&R C compilers are no longer of practical interest, though,
and Autoconf assumes at least C89, the first C standard,
which is sometimes called “C90” due to a delay in standardization.
C has since gone through the standards C99, C11, C17, and C23, and
Autoconf is compatible with all these standards.

Program portability is a huge topic, and this section can only briefly
introduce common pitfalls. See [Portability
between System Types](https://www.gnu.org/prep/standards/standards.html#System-Portability) in The GNU Coding Standards, for
more information.

* [Varieties of Unportability](#Varieties-of-Unportability)
* [Integer Overflow](#Integer-Overflow)
* [Preprocessor Arithmetic](#Preprocessor-Arithmetic)
* [Properties of Null Pointers](#Null-Pointers)
* [Buffer Overruns and Subscript Errors](#Buffer-Overruns)
* [Volatile Objects](#Volatile-Objects)
* [Floating Point Portability](#Floating-Point-Portability)
* [Exiting Portably](#Exiting-Portably)

---

Next: [Integer Overflow](#Integer-Overflow), Up: [Portable C and C++ Programming](#Portable-C-and-C_002b_002b)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 13.1 Varieties of Unportability [¶](#Varieties-of-Unportability-1)

Autoconf tests and ordinary programs often need to test what is allowed
on a system, and therefore they may need to deliberately exceed the
boundaries of what the standards allow, if only to see whether an
optional feature is present. When you write such a program, you should
keep in mind the difference between constraints, unspecified behavior,
and undefined behavior.

In C, a *constraint* is a rule that the compiler must enforce. An
example constraint is that C programs must not declare a bit-field with
negative width. Tests can therefore reliably assume that programs with
negative-width bit-fields are rejected by a compiler that conforms
to the standard.

*Unspecified behavior* is valid behavior, where the standard allows
multiple possibilities. For example, the order of evaluation of
function arguments is unspecified. Some unspecified behavior is
*implementation-defined*, i.e., documented by the implementation,
but since Autoconf tests cannot read the documentation they cannot
distinguish between implementation-defined and other unspecified
behavior. It is common for Autoconf tests to probe implementations to
determine otherwise-unspecified behavior.

*Undefined behavior* is invalid behavior, where the standard allows
the implementation to do anything it pleases. For example,
dereferencing a null pointer leads to undefined behavior. If possible,
test programs should avoid undefined behavior, since a program with
undefined behavior might succeed on a test that should fail.

The above rules apply to programs that are intended to conform to the
standard. However, strictly-conforming programs are quite rare, since
the standards are so limiting. A major goal of Autoconf is to support
programs that use implementation features not described by the standard,
and it is fairly common for test programs to violate the above rules, if
the programs work well enough in practice.

---

Next: [Preprocessor Arithmetic](#Preprocessor-Arithmetic), Previous: [Varieties of Unportability](#Varieties-of-Unportability), Up: [Portable C and C++ Programming](#Portable-C-and-C_002b_002b)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 13.2 Integer Overflow [¶](#Integer-Overflow-1)

Although some traditional C programs assume that signed integer overflow
wraps around reliably using two’s complement arithmetic, the C standard
says that program behavior is undefined on overflow, and these C
programs may not work on many modern implementations.

* [Basics of Integer Overflow](#Integer-Overflow-Basics)
* [Examples of Code Assuming Wraparound Overflow](#Signed-Overflow-Examples)
* [Optimizations That Break Wraparound Arithmetic](#Optimization-and-Wraparound)
* [Practical Advice for Signed Overflow Issues](#Signed-Overflow-Advice)
* [Signed Integer Division and Integer Overflow](#Signed-Integer-Division)

---

Next: [Examples of Code Assuming Wraparound Overflow](#Signed-Overflow-Examples), Up: [Integer Overflow](#Integer-Overflow)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 13.2.1 Basics of Integer Overflow [¶](#Basics-of-Integer-Overflow)

In languages like C, integer overflow wraps around for unsigned
integer types that are at least as wide as `unsigned int`;
e.g., `UINT_MAX + 1` yields zero.
This is guaranteed by the C standard and is
portable in practice, unless you specify aggressive,
nonstandard optimization options
suitable only for special applications.

In contrast, the C standard says that signed integer overflow leads to
undefined behavior where a program can do anything, including dumping
core or overrunning a buffer. The misbehavior can even precede the
overflow. Such an overflow can occur during addition, subtraction,
multiplication, division, and left shift. It can even occur for
unsigned types like `unsigned short int` that are narrower
than `int`, as values of these types are widened to `int`
before computation.

Despite this requirement of the standard, some C programs assume that
signed integer overflow silently wraps around modulo a power of two,
using two’s complement arithmetic, so long as you convert the resulting
value to a signed integer type. These programs can have problems,
especially when optimization is enabled. If you assume a GCC-like
compiler, you can work around the problems by compiling with GCC’s
`-fwrapv` option; however, this is not portable.

For historical reasons C17 and earlier also allowed implementations with
ones’ complement or signed magnitude arithmetic, but C23 requires
two’s complement and it is safe to assume two’s complement nowadays.

Also, overflow can occur when converting an out-of-range value to a
signed integer type. Here a standard implementation must define what
happens, and this can include raising an exception. Although practical
implementations typically wrap around silently in this case, a few
debugging implementations trap instead.

---

Next: [Optimizations That Break Wraparound Arithmetic](#Optimization-and-Wraparound), Previous: [Basics of Integer Overflow](#Integer-Overflow-Basics), Up: [Integer Overflow](#Integer-Overflow)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 13.2.2 Examples of Code Assuming Wraparound Overflow [¶](#Examples-of-Code-Assuming-Wraparound-Overflow)

There was long a tension between what the C standard requires for signed
integer overflow, and what traditional C programs commonly assumed. The
standard allows aggressive optimizations based on assumptions that
overflow never occurs, but traditionally many C programs relied on overflow
wrapping around. Although these programs did not conform to the standard,
they formerly worked in practice because traditionally compilers did not
optimize in such a way that would break the programs. Nowadays, though,
compilers do perform these optimizations, so portable programs can no
longer assume reliable wraparound on signed integer overflow.

The C Standard says that if a program has signed integer overflow its
behavior is undefined, and the undefined behavior can even precede the
overflow. To take an extreme example:

```
if (password == expected_password)
  allow_superuser_privileges ();
else if (counter++ == INT_MAX)
  abort ();
else
  printf ("%d password mismatches\n", counter);
```

If the `int` variable `counter` equals `INT_MAX`,
`counter++` must overflow and the behavior is undefined, so the C
standard allows the compiler to optimize away the test against
`INT_MAX` and the `abort` call.
Worse, if an earlier bug in the program lets the compiler deduce that
`counter == INT_MAX` or that `counter` previously overflowed,
the C standard allows the compiler to optimize away the password test
and generate code that allows superuser privileges unconditionally.

Here is an example derived from the 7th Edition Unix implementation of
`atoi` (1979-01-10):

```
char *p;
int f, n;
...
while (*p >= '0' && *p <= '9')
  n = n * 10 + *p++ - '0';
return (f ? -n : n);
```

Even if the input string is in range, on most modern machines this has
signed overflow when computing the most negative integer (the `-n`
overflows) or a value near an extreme integer (the `+`
overflows).

Here is another example, derived from the 7th Edition implementation of
`rand` (1979-01-10). Here the programmer expects both
multiplication and addition to wrap on overflow:

```
static long int randx = 1;
...
randx = randx * 1103515245 + 12345;
return (randx >> 16) & 077777;
```

In the following example, derived from the GNU C Library 2.15
implementation of `mktime` (2012-03-21), the code assumes
wraparound arithmetic in `+` to detect signed overflow:

```
time_t t, t1, t2;
int sec_requested, sec_adjustment;
...
t1 = t + sec_requested;
t2 = t1 + sec_adjustment;
if (((t1 < t) != (sec_requested < 0))
    | ((t2 < t1) != (sec_adjustment < 0)))
  return -1;
```

Although some of these examples will likely behave as if signed integer
overflow wraps around reliably, other examples are likely to misbehave
when optimization is enabled. All these examples should be avoided in
portable code because signed integer overflow is not reliable on modern
systems, and it’s not worth worrying about which of these examples
happen to work on most platforms and which do not.

---

Next: [Practical Advice for Signed Overflow Issues](#Signed-Overflow-Advice), Previous: [Examples of Code Assuming Wraparound Overflow](#Signed-Overflow-Examples), Up: [Integer Overflow](#Integer-Overflow)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 13.2.3 Optimizations That Break Wraparound Arithmetic [¶](#Optimizations-That-Break-Wraparound-Arithmetic)

Compilers sometimes generate code that is incompatible with wraparound
integer arithmetic. A simple example is an algebraic simplification: a
compiler might translate `(i * 2000) / 1000` to `i * 2`
because it assumes that `i * 2000` does not overflow. The
translation is not equivalent to the original when overflow occurs:
e.g., in the typical case of 32-bit signed two’s complement wraparound
`int`, if `i` has type `int` and value `1073742`,
the original expression returns −2147483 but the optimized
version returns the mathematically correct value 2147484.

More subtly, loop induction optimizations often exploit the undefined
behavior of signed overflow. Consider the following contrived function
`sumc`:

```
int
sumc (int lo, int hi)
{
  int sum = 0;
  for (int i = lo; i <= hi; i++)
    sum ^= i * 53;
  return sum;
}
```

To avoid multiplying by 53 each time through the loop, an optimizing
compiler might internally transform `sumc` to the equivalent of the
following:

```
int
transformed_sumc (int lo, int hi)
{
  int sum = 0;
  int hic = hi * 53;
  for (int ic = lo * 53; ic <= hic; ic += 53)
    sum ^= ic;
  return sum;
}
```

This transformation is allowed by the C standard, but it is invalid for
wraparound arithmetic when `INT_MAX / 53 < hi`, because then the
overflow in computing expressions like `hi * 53` can cause the
expression `i <= hi` to yield a different value from the
transformed expression `ic <= hic`.

For this reason, compilers that use loop induction and similar
techniques often do not support reliable wraparound arithmetic when a
loop induction variable like `ic` is involved. Since loop
induction variables are generated by the compiler, and are not visible
in the source code, it is not always trivial to say whether the problem
affects your code.

Hardly any code actually depends on wraparound arithmetic in cases like
these, so in practice these loop induction optimizations are almost
always useful. However, edge cases in this area can cause problems.
For example:

```
for (int j = 1; 0 < j; j *= 2)
  test (j);
```

Here, the loop attempts to iterate through all powers of 2 that
`int` can represent, but the C standard allows a compiler to
optimize away the comparison and generate an infinite loop,
under the argument that behavior is undefined on overflow. As of this
writing this optimization is done on some platforms by
GCC with `-O2`, so this code is not portable in practice.

---

Next: [Signed Integer Division and Integer Overflow](#Signed-Integer-Division), Previous: [Optimizations That Break Wraparound Arithmetic](#Optimization-and-Wraparound), Up: [Integer Overflow](#Integer-Overflow)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 13.2.4 Practical Advice for Signed Overflow Issues [¶](#Practical-Advice-for-Signed-Overflow-Issues)

Ideally the safest approach is to avoid signed integer overflow
entirely. For example, instead of multiplying two signed integers, you
can convert them to double-width integers, multiply the wider values,
then test whether the result is in the narrower range. Or you can use
more-complicated code employing unsigned integers of the same width.

Rewriting code in this way will be inconvenient, though, especially if
the signed values might be negative and no wider type is available.
Using unsigned arithmetic to check for overflow is
particularly painful to do portably and efficiently when dealing with an
integer type like `uid_t` whose width and signedness vary from
platform to platform. Also, this approach may hurt performance.

Hence it is often useful to maintain code that needs
wraparound on overflow, instead of rewriting the code. The rest of this
section attempts to give practical advice for this situation.

To detect integer overflow portably when attempting operations like
`sum = a + b`, you can use the C23 `<stdckdint.h>` macros
`ckd_add`, `ckd_sub`, and `ckd_mul`.
The following code adds two integers with overflow wrapping around
reliably in the sum:

```
#include <stdckdint.h>
...
/* Set sum = a + b, with wraparound.  */
if (ckd_add (&sum, a, b))
  /* 'sum' has just the low order bits.  */;
else
  /* 'sum' is the correct answer.  */;
```

To be portable to pre-C23 platforms you can use Gnulib’s
`stdckdint` module, which emulates this part of C23 (see [Gnulib](#Gnulib)).
Invoking the `stdckdint` macros typically costs just one machine
instruction for the arithmetic and another instruction for the rare
branch on overflow.

If your code uses a signed loop index, make sure that the index cannot
overflow, along with all signed expressions derived from the index.
Here is a contrived example of problematic code with two instances of
overflow.

```
for (int i = INT_MAX - 10; i <= INT_MAX; i++)
  if (i + 1 < 0)
    {
      report_overflow ();
      break;
    }
```

Because of the two overflows, a compiler might optimize away or
transform the two comparisons in a way that is incompatible with the
wraparound assumption.

If your code is intended to be compiled only by GCC and
assumes wraparound behavior, and you want to insulate it
against any GCC optimizations that would fail to support that
behavior, you should use GCC’s `-fwrapv` option, which
causes signed overflow to wrap around reliably (except for division and
remainder, as discussed in the next section).

If you need to write portable code and therefore cannot assume that
signed integer overflow wraps around reliably, you should consider
debugging with a GCC option that causes signed overflow to raise an
exception. These options include `-fsanitize=undefined` and
`-ftrapv`.

---

Previous: [Practical Advice for Signed Overflow Issues](#Signed-Overflow-Advice), Up: [Integer Overflow](#Integer-Overflow)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 13.2.5 Signed Integer Division and Integer Overflow [¶](#Signed-Integer-Division-and-Integer-Overflow)

Overflow in signed
integer division is not always harmless: for example, on CPUs of the
i386 family, dividing `INT_MIN` by `-1` yields a SIGFPE signal
which by default terminates the program. Worse, taking the remainder
of these two values typically yields the same signal on these CPUs,
behavior that the C standard allows.

---

Next: [Properties of Null Pointers](#Null-Pointers), Previous: [Integer Overflow](#Integer-Overflow), Up: [Portable C and C++ Programming](#Portable-C-and-C_002b_002b)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 13.3 Preprocessor Arithmetic [¶](#Preprocessor-Arithmetic-1)

In C99 and later, preprocessor arithmetic, used for `#if`
expressions, must
be evaluated as if all signed values are of type `intmax_t` and all
unsigned values of type `uintmax_t`. Many compilers are buggy in
this area, though. For example, as of 2007, Sun C mishandles `#if
LLONG_MIN < 0` on a platform with 32-bit `long int` and 64-bit
`long long int`. Also, some older preprocessors mishandle
constants ending in `LL`. To work around these problems, you can
compute the value of expressions like `LONG_MAX < LLONG_MAX` at
`configure`-time rather than at `#if`-time.

---

Next: [Buffer Overruns and Subscript Errors](#Buffer-Overruns), Previous: [Preprocessor Arithmetic](#Preprocessor-Arithmetic), Up: [Portable C and C++ Programming](#Portable-C-and-C_002b_002b)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 13.4 Properties of Null Pointers [¶](#Properties-of-Null-Pointers)

Most modern hosts reliably fail when you attempt to dereference a null
pointer.

On almost all modern hosts, null pointers use an all-bits-zero internal
representation, so you can reliably use `memset` with 0 to set all
the pointers in an array to null values.

If `p` is a null pointer to an object type, the C expression
`p + 0` always evaluates to `p` on modern hosts, even though
the standard says that it has undefined behavior.

---

Next: [Volatile Objects](#Volatile-Objects), Previous: [Properties of Null Pointers](#Null-Pointers), Up: [Portable C and C++ Programming](#Portable-C-and-C_002b_002b)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 13.5 Buffer Overruns and Subscript Errors [¶](#Buffer-Overruns-and-Subscript-Errors)

Buffer overruns and subscript errors are the most common dangerous
errors in C programs. They result in undefined behavior because storing
outside an array typically modifies storage that is used by some other
object, and most modern systems lack runtime checks to catch these
errors. Programs should not rely on buffer overruns being caught.

There is one exception to the usual rule that a portable program cannot
address outside an array. In C, it is valid to compute the address just
past an object, e.g., `&a[N]` where `a` has `N` elements,
so long as you do not dereference the resulting pointer. But it is not
valid to compute the address just before an object, e.g., `&a[-1]`;
nor is it valid to compute two past the end, e.g., `&a[N+1]`. On
most platforms `&a[-1] < &a[0] && &a[N] < &a[N+1]`, but this is not
reliable in general, and it is usually easy enough to avoid the
potential portability problem, e.g., by allocating an extra unused array
element at the start or end.

[Valgrind](https://www.valgrind.org/) can catch many overruns.
GCC users might also consider using the `-fsanitize=` options to
catch overruns. See [Program Instrumentation
Options](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html#Instrumentation-Options) in Using the GNU Compiler Collection (GCC).

Buffer overruns are usually caused by off-by-one errors, but there are
more subtle ways to get them.

Using `int` values to index into an array or compute array sizes
causes problems on typical 64-bit hosts where an array index might
be *2^{31}* or larger. Index values of type `size_t` avoid this
problem, but cannot be negative. Index values of type `ptrdiff_t`
are signed, and are wide enough in practice.

If you add or multiply two numbers to calculate an array size, e.g.,
`malloc (x * sizeof y + z)`, havoc ensues if the addition or
multiplication overflows.

Many implementations of the `alloca` function silently misbehave
and can generate buffer overflows if given sizes that are too large.
The size limits are implementation dependent, but are at least 4000
bytes on all platforms that we know about.

The standard functions `asctime`, `asctime_r`, `ctime`,
`ctime_r`, and `gets` are prone to buffer overflows, and
portable code should not use them unless the inputs are known to be
within certain limits. The time-related functions can overflow their
buffers if given timestamps out of range (e.g., a year less than -999
or greater than 9999). Time-related buffer overflows cannot happen with
recent-enough versions of the GNU C library, but are possible
with other
implementations. The `gets` function is the worst, since it almost
invariably overflows its buffer when presented with an input line larger
than the buffer.

---

Next: [Floating Point Portability](#Floating-Point-Portability), Previous: [Buffer Overruns and Subscript Errors](#Buffer-Overruns), Up: [Portable C and C++ Programming](#Portable-C-and-C_002b_002b)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 13.6 Volatile Objects [¶](#Volatile-Objects-1)

The keyword `volatile` is often misunderstood in portable code.
Its use inhibits some memory-access optimizations, but programmers often
wish that it had a different meaning than it actually does.

`volatile` was designed for code that accesses special objects like
memory-mapped device registers whose contents spontaneously change.
Such code is inherently low-level, and it is difficult to specify
portably what `volatile` means in these cases. The C standard
says, “What constitutes an access to an object that has
volatile-qualified type is implementation-defined,” so in theory each
implementation is supposed to fill in the gap by documenting what
`volatile` means for that implementation. In practice, though,
this documentation is usually absent or incomplete.

One area of confusion is the distinction between objects defined with
volatile types, and volatile lvalues. From the C standard’s point of
view, an object defined with a volatile type has externally visible
behavior. You can think of such objects as having little oscilloscope
probes attached to them, so that the user can observe some properties of
accesses to them, just as the user can observe data written to output
files. However, accesses via volatile lvalues to ordinary objects
are merely side effects (i.e., changes to the state of the execution
environment), and the implementation is not required to document
their visibility any further. For example:

```
/* Declare and access a volatile object.
   Accesses to X are "visible" to users.  */
static int volatile x;
x = 1;

/* Access two ordinary objects via a volatile lvalue.
   Although each read and write is a side effect,
   the accesses are not directly "visible" to users.  */
int y = 0;
int *z = malloc (sizeof *z);
*z = 7;
int volatile *p;
p = &y;
*p = *p + 1;
p = z;
*p = *p + 1;
```

Programmers often wish that `volatile` meant “Perform the memory
access here and now, without merging several memory accesses, without
changing the memory word size, and without reordering.” But the C
standard does not require this. For objects defined with a volatile
type, accesses must be done before the next sequence point; but
otherwise merging, reordering, and word-size change is allowed.

Even when accessing objects defined with a volatile type,
the C standard allows only
extremely limited signal handlers: in C23 the behavior is undefined if a
signal handler refers to any non-local object that is not a lock-free
atomic object and that is not `constexpr` (other than by writing to
a `sig_atomic_t volatile` object), or calls any standard library
function other than from a small set that includes `abort`,
`_Exit`, `quick_exit`, some `<stdatomic.h>` functions,
and `signal`. Hence C compilers need not worry about a signal
handler disturbing ordinary computation. POSIX allows some additional
behavior in a portable signal handler, but is still quite restrictive.
See [When is a Volatile Object Accessed?](https://gcc.gnu.org/onlinedocs/gcc/Volatiles.html#Volatiles) in Using the
GNU Compiler Collection (GCC), for some
restrictions imposed by GCC. See [Defining Signal Handlers](https://www.gnu.org/software/libc/manual/html_mono/libc.html#Defining-Handlers) in The GNU C Library, for some
restrictions imposed by the GNU C library. Restrictions
differ on other platforms.

If possible, it is best to use a signal handler that fits within the
limits imposed by the C and POSIX standards.

If this is not practical, you can try the following rules of thumb. A
signal handler should access only volatile lvalues, preferably lvalues
that refer to objects defined with a volatile type, and should not
assume that the accessed objects have an internally consistent state
if they are larger than a machine word. Furthermore, installers
should employ compilers and compiler options that are commonly used
for building operating system kernels, because kernels often need more
from `volatile` than the C Standard requires, and installers who
compile an application in a similar environment can sometimes benefit
from the extra constraints imposed by kernels on compilers.
Admittedly we are hand-waving somewhat here, as there are few
guarantees in this area; the rules of thumb may help to fix some bugs
but there is a good chance that they will not fix them all.

For `volatile`, C++ has the same problems that C does.
Multithreaded applications have even more problems with `volatile`,
but they are beyond the scope of this section.

The bottom line is that using `volatile` typically hurts
performance but should not hurt correctness. In some cases its use
does help correctness, but these cases are often so poorly understood
that all too often adding `volatile` to a data structure merely
alleviates some symptoms of a bug while not fixing the bug in general.

---

Next: [Exiting Portably](#Exiting-Portably), Previous: [Volatile Objects](#Volatile-Objects), Up: [Portable C and C++ Programming](#Portable-C-and-C_002b_002b)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 13.7 Floating Point Portability [¶](#Floating-Point-Portability-1)

Almost all modern systems use IEEE-754 floating point, and it is safe to
assume IEEE-754 in most portable code these days. For more information,
please see David Goldberg’s classic paper
[What Every Computer
Scientist Should Know About Floating-Point Arithmetic](https://www.validlab.com/goldberg/paper.pdf).

---

Previous: [Floating Point Portability](#Floating-Point-Portability), Up: [Portable C and C++ Programming](#Portable-C-and-C_002b_002b)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 13.8 Exiting Portably [¶](#Exiting-Portably-1)

A C or C++ program can exit with status N by returning
N from the `main` function. Portable programs are supposed
to exit either with status 0 or `EXIT_SUCCESS` to succeed, or with
status `EXIT_FAILURE` to fail, but in practice it is portable to
fail by exiting with status 1, and test programs that assume POSIX can
fail by exiting with status values from 1 through 255.

A program can also exit with status N by passing N to the
`exit` function, and a program can fail by calling the `abort`
function. If a program is specialized to just some platforms, it can fail
by calling functions specific to those platforms, e.g., `_exit`
(POSIX). However, like other functions, an exit
function should be declared, typically by including a header. For
example, if a C program calls `exit`, it should include `stdlib.h`
either directly or via the default includes (see [Default Includes](#Default-Includes)).

A program can fail due to undefined behavior such as dereferencing a null
pointer, but this is not recommended as undefined behavior allows an
implementation to do whatever it pleases and this includes exiting
successfully.

---

Next: [Site Configuration](#Site-Configuration), Previous: [Portable C and C++ Programming](#Portable-C-and-C_002b_002b), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 14 Manual Configuration [¶](#Manual-Configuration-1)

A few kinds of features can’t be guessed automatically by running test
programs. For example, the details of the object-file format, or
special options that need to be passed to the compiler or linker.
Autoconf provides a uniform method for handling unguessable features,
by giving each operating system a *canonical system type*, also
known as a *canonical name* or *target triplet*.

If you use any of the macros described in this chapter, you must
distribute the helper scripts `config.guess` and
`config.sub` along with your source code. Some Autoconf macros
use these macros internally, so you may need to distribute these scripts
even if you do not use any of these macros yourself. See [Configure Input: Source Code, Macros, and Auxiliary Files](#Input), for
information about the `AC_CONFIG_AUX_DIR` macro which you can use
to control in which directory `configure` looks for helper
scripts, and where to get the scripts from.

* [Specifying target triplets](#Specifying-Target-Triplets)
* [Getting the Canonical System Type](#Canonicalizing)
* [Using the System Type](#Using-System-Type)

---

Next: [Getting the Canonical System Type](#Canonicalizing), Up: [Manual Configuration](#Manual-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 14.1 Specifying target triplets [¶](#Specifying-target-triplets)

Autoconf-generated
`configure` scripts can make decisions based on a canonical name
for the system type, or *target triplet*, which has the form:
‘`cpu-vendor-os`’, where os can be
‘`system`’ or ‘`kernel-system`’

`configure` can usually guess the canonical name for the type of
system it’s running on. To do so it runs a script called
`config.guess`, which infers the name using the `uname`
command or symbols predefined by the C preprocessor.

Alternately, the user can specify the system type with command line
arguments to `configure` (see [Specifying a System Type](#System-Types). Doing so is
necessary when
cross-compiling. In the most complex case of cross-compiling, three
system types are involved. The options to specify them are:

`--build=build-type`
:   the type of system on which the package is being configured and
    compiled. It defaults to the result of running `config.guess`.
    Specifying a build-type that differs from host-type enables
    cross-compilation mode.

`--host=host-type`
:   the type of system on which the package runs. By default it is the
    same as the build machine. The tools that get used to build and
    manipulate binaries will, by default, all be prefixed with
    `host-type-`, such as `host-type-gcc`,
    `host-type-g++`, `host-type-ar`, and
    `host-type-nm`. If the binaries produced by these tools can
    be executed by the build system, the configure script will make use of
    it in `AC_RUN_IFELSE` invocations; otherwise, cross-compilation
    mode is enabled. Specifying a host-type that differs
    from build-type, when build-type was also explicitly
    specified, equally enables cross-compilation mode.

`--target=target-type`
:   the type of system for which any compiler tools in the package
    produce code (rarely needed). By default, it is the same as host.

If you mean to override the result of `config.guess` but
still produce binaries for the build machine, use `--build`,
not `--host`.

So, for example, to produce binaries for 64-bit MinGW, use a command
like this:

```
./configure --host=x86_64-w64-mingw64
```

If your system has the ability to execute MinGW binaries but you don’t
want to make use of this feature and instead prefer cross-compilation
guesses, use a command like this:

```
./configure --build=x86_64-pc-linux-gnu --host=x86_64-w64-mingw64
```

Note that if you do not specify `--host`, `configure`
fails if it can’t run the code generated by the specified compiler. For
example, configuring as follows fails:

```
./configure CC=x86_64-w64-mingw64-gcc
```

When cross-compiling, `configure` will warn about any tools
(compilers, linkers, assemblers) whose name is not prefixed with the
host type. This is an aid to users performing cross-compilation.
Continuing the example above, if a cross-compiler named `cc` is
used with a native `pkg-config`, then libraries found by
`pkg-config` will likely cause subtle build failures; but using
the names `x86_64-w64-mingw64-gcc` and
`x86_64-w64-mingw64-pkg-config`
avoids any confusion. Avoiding the warning is as simple as creating the
correct symlinks naming the cross tools.

`configure` recognizes short aliases for some system types; for
example, ‘`mingw64`’ can be used instead of
‘`x86_64-pc-mingw64`’. `configure` runs a script called
`config.sub` to canonicalize system type aliases.

This section deliberately omits the description of the obsolete
interface; see [Hosts and Cross-Compilation](#Hosts-and-Cross_002dCompilation).

---

Next: [Using the System Type](#Using-System-Type), Previous: [Specifying target triplets](#Specifying-Target-Triplets), Up: [Manual Configuration](#Manual-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 14.2 Getting the Canonical System Type [¶](#Getting-the-Canonical-System-Type)

The following macros make the system type available to `configure`
scripts.

The variables ‘`build_alias`’, ‘`host_alias`’, and
‘`target_alias`’ are always exactly the arguments of `--build`,
`--host`, and `--target`; in particular, they are left empty
if the user did not use them, even if the corresponding
`AC_CANONICAL` macro was run. Any configure script may use these
variables anywhere. These are the variables that should be used when in
interaction with the user.

If you need to recognize some special environments based on their system
type, run the following macros to get canonical system names. These
variables are not set before the macro call.

Macro: **AC\_CANONICAL\_BUILD** [¶](#index-AC_005fCANONICAL_005fBUILD-1)
:   Compute the canonical build-system type variable, `build`, and its
    three individual parts `build_cpu`, `build_vendor`, and
    `build_os`.

    If `--build` was specified, then `build` is the
    canonicalization of `build_alias` by `config.sub`,
    otherwise it is determined by the shell script `config.guess`.

Macro: **AC\_CANONICAL\_HOST** [¶](#index-AC_005fCANONICAL_005fHOST-1)
:   Compute the canonical host-system type variable, `host`, and its
    three individual parts `host_cpu`, `host_vendor`, and
    `host_os`.

    If `--host` was specified, then `host` is the
    canonicalization of `host_alias` by `config.sub`,
    otherwise it defaults to `build`.

Macro: **AC\_CANONICAL\_TARGET** [¶](#index-AC_005fCANONICAL_005fTARGET-1)
:   Compute the canonical target-system type variable, `target`, and its
    three individual parts `target_cpu`, `target_vendor`, and
    `target_os`.

    If `--target` was specified, then `target` is the
    canonicalization of `target_alias` by `config.sub`,
    otherwise it defaults to `host`.

Note that there can be artifacts due to the backward compatibility
code. See [Hosts and Cross-Compilation](#Hosts-and-Cross_002dCompilation), for more.

---

Previous: [Getting the Canonical System Type](#Canonicalizing), Up: [Manual Configuration](#Manual-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 14.3 Using the System Type [¶](#Using-the-System-Type)

In `configure.ac` the system type is generally used by one or more
`case` statements to select system-specifics. Shell wildcards can
be used to match a group of system types.

For example, an extra assembler code object file could be chosen, giving
access to a CPU cycle counter register. `$(CYCLE_OBJ)` in the
following would be used in a makefile to add the object to a
program or library.

```
AS_CASE([$host],
  [aarch64*-*-*], [CYCLE_OBJ=pmccntr.o],
  [i?86-*-*],     [CYCLE_OBJ=rdtsc.o],
  [CYCLE_OBJ=""])
AC_SUBST([CYCLE_OBJ])
```

`AC_CONFIG_LINKS` (see [Creating Configuration Links](#Configuration-Links)) is another good way
to select variant source files, for example optimized code for some
CPUs. The configured CPU type doesn’t always indicate exact CPU types,
so some runtime capability checks may be necessary too.

```
AS_CASE([$host],
  [aarch64*-*-*], [AC_CONFIG_LINKS([dither.c:aarch64/dither.c])],
  [powerpc*-*-*], [AC_CONFIG_LINKS([dither.c:powerpc/dither.c])],
  [AC_CONFIG_LINKS([dither.c:generic/dither.c])])
```

The host system type can also be used to find cross-compilation tools
with `AC_CHECK_TOOL` (see [Generic Program and File Checks](#Generic-Programs)).

The above examples all show ‘`$host`’, since this is where the code
is going to run. Only rarely is it necessary to test ‘`$build`’
(which is where the build is being done).

Whenever you’re tempted to use ‘`$host`’ it’s worth considering
whether some sort of probe would be better. New system types come along
periodically or previously missing features are added. Well-written
probes can adapt themselves to such things, but hard-coded lists of
names can’t. Here are some guidelines,

* Availability of libraries and library functions should always be checked
  by probing.
* Variant behavior of system calls is best identified with runtime tests
  if possible, but bug workarounds or obscure difficulties might have to
  be driven from ‘`$host`’.
* Assembler code is inevitably highly CPU-specific and is best selected
  according to ‘`$host_cpu`’.
* Assembler variations like underscore prefix on globals or ELF versus
  COFF type directives are however best determined by probing, perhaps
  even examining the compiler output.

‘`$target`’ is for use by a package creating a compiler or similar.
For ordinary packages it’s meaningless and should not be used. It
indicates what the created compiler should generate code for, if it can
cross-compile. ‘`$target`’ generally selects various hard-coded CPU
and system conventions, since usually the compiler or tools under
construction themselves determine how the target works.

---

Next: [Running `configure` Scripts](#Running-configure-Scripts), Previous: [Manual Configuration](#Manual-Configuration), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 15 Site Configuration [¶](#Site-Configuration-1)

`configure` scripts support several kinds of local configuration
decisions. There are ways for users to specify where external software
packages are, include or exclude optional features, install programs
under modified names, and set default values for `configure`
options.

* [Controlling Help Output](#Help-Formatting)
* [Working With External Software](#External-Software)
* [Choosing Package Options](#Package-Options)
* [Making Your Help Strings Look Pretty](#Pretty-Help-Strings)
* [Controlling Checking of `configure` Options](#Option-Checking)
* [Configuring Site Details](#Site-Details)
* [Transforming Program Names When Installing](#Transforming-Names)
* [Setting Site Defaults](#Site-Defaults)

---

Next: [Working With External Software](#External-Software), Up: [Site Configuration](#Site-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.1 Controlling Help Output [¶](#Controlling-Help-Output)

Users consult ‘`configure --help`’ to learn of configuration
decisions specific to your package. By default, `configure`
breaks this output into sections for each type of option; within each
section, help strings appear in the order `configure.ac` defines
them:

```
Optional Features:
  ...
  --enable-bar            include bar

Optional Packages:
  ...
  --with-foo              use foo
```

Macro: **AC\_PRESERVE\_HELP\_ORDER** [¶](#index-AC_005fPRESERVE_005fHELP_005fORDER-1)
:   Request an alternate `--help` format, in which options of all
    types appear together, in the order defined. Call this macro before any
    `AC_ARG_ENABLE` or `AC_ARG_WITH`.

    ```
    Optional Features and Packages:
      ...
      --enable-bar            include bar
      --with-foo              use foo
    ```

---

Next: [Choosing Package Options](#Package-Options), Previous: [Controlling Help Output](#Help-Formatting), Up: [Site Configuration](#Site-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.2 Working With External Software [¶](#Working-With-External-Software)

Some packages require, or can optionally use, other software packages
that are already installed. The user can give `configure`
command line options to specify which such external software to use.
The options have one of these forms:

```
--with-package[=arg]
--without-package
```

For example, `--with-gnu-ld` means work with the GNU linker
instead of some other linker. `--with-x` means work with The X
Window System.

The user can give an argument by following the package name with
‘`=`’ and the argument. Giving an argument of ‘`no`’ is for
packages that are used by default; it says to *not* use the
package. An argument that is neither ‘`yes`’ nor ‘`no`’ could
include a name or number of a version of the other package, to specify
more precisely which other package this program is supposed to work
with. If no argument is given, it defaults to ‘`yes`’.
`--without-package` is equivalent to
`--with-package=no`.

Normally `configure` scripts complain about
`--with-package` options that they do not support.
See [Controlling Checking of `configure` Options](#Option-Checking), for details, and for how to override the
defaults.

For each external software package that may be used, `configure.ac`
should call `AC_ARG_WITH` to detect whether the `configure`
user asked to use it. Whether each package is used or not by default,
and which arguments are valid, is up to you.

Macro: **AC\_ARG\_WITH**(package, help-string, [action-if-given], [action-if-not-given]) [¶](#index-AC_005fARG_005fWITH-1)
:   If the user gave `configure` the option `--with-package`
    or `--without-package`, run shell commands
    action-if-given. If neither option was given, run shell commands
    action-if-not-given. The name package indicates another
    software package that this program should work with. It should consist
    only of alphanumeric characters, dashes, plus signs, and dots.

    The option’s argument is available to the shell commands
    action-if-given in the shell variable `withval`, which is
    actually just the value of the shell variable named
    `with_package`, with any non-alphanumeric characters in
    package changed into ‘`_`’. You may use that variable instead,
    if you wish.

    Note that action-if-not-given is not expanded until the point that
    `AC_ARG_WITH` was expanded. If you need the value of
    `with_package` set to a default value by the time argument
    parsing is completed, use `m4_divert_text` to the `DEFAULTS`
    diversion (see [m4\_divert\_text](#m4_005fdivert_005ftext)) (if done as an argument to
    `AC_ARG_WITH`, also provide non-diverted text to avoid a shell
    syntax error).

    The argument help-string is a description of the option that
    looks like this:

    ```
      --with-readline         support fancy command line editing
    ```

    help-string may be more than one line long, if more detail is
    needed. Just make sure the columns line up in ‘`configure
    --help`’. Avoid tabs in the help string. The easiest way to provide the
    proper leading whitespace is to format your help-string with the macro
    `AS_HELP_STRING` (see [Making Your Help Strings Look Pretty](#Pretty-Help-Strings)).

    The following example shows how to use the `AC_ARG_WITH` macro in
    a common situation. You want to let the user decide whether to enable
    support for an external library (e.g., the readline library); if the user
    specified neither `--with-readline` nor `--without-readline`,
    you want to enable support for readline only if the library is available
    on the system.

    ```
    AC_ARG_WITH([readline],
      [AS_HELP_STRING([--with-readline],
        [support fancy command line editing @<:@default=check@:>@])],
      [],
      [: m4_divert_text([DEFAULTS], [with_readline=check])])

    LIBREADLINE=
    AS_IF([test "x$with_readline" != xno],
      [AC_CHECK_LIB([readline], [main],
        [AC_SUBST([LIBREADLINE], ["-lreadline -lncurses"])
         AC_DEFINE([HAVE_LIBREADLINE], [1],
                   [Define if you have libreadline])
        ],
        [if test "x$with_readline" != xcheck; then
           AC_MSG_FAILURE(
             [--with-readline was given, but test for readline failed])
         fi
        ], -lncurses)])
    ```

    The next example shows how to use `AC_ARG_WITH` to give the user the
    possibility to enable support for the readline library, in case it is still
    experimental and not well tested, and is therefore disabled by default.

    ```
    AC_ARG_WITH([readline],
      [AS_HELP_STRING([--with-readline],
        [enable experimental support for readline])],
      [],
      [with_readline=no])

    LIBREADLINE=
    AS_IF([test "x$with_readline" != xno],
      [AC_CHECK_LIB([readline], [main],
        [AC_SUBST([LIBREADLINE], ["-lreadline -lncurses"])
         AC_DEFINE([HAVE_LIBREADLINE], [1],
                   [Define if you have libreadline])
        ],
        [AC_MSG_FAILURE(
           [--with-readline was given, but test for readline failed])],
        [-lncurses])])
    ```

    The last example shows how to use `AC_ARG_WITH` to give the user the
    possibility to disable support for the readline library, given that it is
    an important feature and that it should be enabled by default.

    ```
    AC_ARG_WITH([readline],
      [AS_HELP_STRING([--without-readline],
        [disable support for readline])],
      [],
      [with_readline=yes])

    LIBREADLINE=
    AS_IF([test "x$with_readline" != xno],
      [AC_CHECK_LIB([readline], [main],
        [AC_SUBST([LIBREADLINE], ["-lreadline -lncurses"])
         AC_DEFINE([HAVE_LIBREADLINE], [1],
                   [Define if you have libreadline])
        ],
        [AC_MSG_FAILURE(
           [readline test failed (--without-readline to disable)])],
        [-lncurses])])
    ```

    These three examples can be easily adapted to the case where
    `AC_ARG_ENABLE` should be preferred to `AC_ARG_WITH` (see
    [Choosing Package Options](#Package-Options)).

---

Next: [Making Your Help Strings Look Pretty](#Pretty-Help-Strings), Previous: [Working With External Software](#External-Software), Up: [Site Configuration](#Site-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.3 Choosing Package Options [¶](#Choosing-Package-Options)

If a software package has optional compile-time features, the user can
give `configure` command line options to specify whether to
compile them. The options have one of these forms:

```
--enable-feature[=arg]
--disable-feature
```

These options allow users to choose which optional features to build and
install. `--enable-feature` options should never make a
feature behave differently or cause one feature to replace another.
They should only cause parts of the program to be built rather than left
out.

The user can give an argument by following the feature name with
‘`=`’ and the argument. Giving an argument of ‘`no`’ requests
that the feature *not* be made available. A feature with an
argument looks like `--enable-debug=stabs`. If no argument is
given, it defaults to ‘`yes`’. `--disable-feature` is
equivalent to `--enable-feature=no`.

Normally `configure` scripts complain about
`--enable-package` options that they do not support.
See [Controlling Checking of `configure` Options](#Option-Checking), for details, and for how to override the
defaults.

For each optional feature, `configure.ac` should call
`AC_ARG_ENABLE` to detect whether the `configure` user asked
to include it. Whether each feature is included or not by default, and
which arguments are valid, is up to you.

Macro: **AC\_ARG\_ENABLE**(feature, help-string, [action-if-given], [action-if-not-given]) [¶](#index-AC_005fARG_005fENABLE-1)
:   If the user gave `configure` the option
    `--enable-feature` or `--disable-feature`, run
    shell commands action-if-given. If neither option was given, run
    shell commands action-if-not-given. The name feature
    indicates an optional user-level facility. It should consist only of
    alphanumeric characters, dashes, plus signs, and dots.

    The option’s argument is available to the shell commands
    action-if-given in the shell variable `enableval`, which is
    actually just the value of the shell variable named
    `enable_feature`, with any non-alphanumeric characters in
    feature changed into ‘`_`’. You may use that variable instead,
    if you wish. The help-string argument is like that of
    `AC_ARG_WITH` (see [Working With External Software](#External-Software)).

    Note that action-if-not-given is not expanded until the point that
    `AC_ARG_ENABLE` was expanded. If you need the value of
    `enable_feature` set to a default value by the time argument
    parsing is completed, use `m4_divert_text` to the `DEFAULTS`
    diversion (see [m4\_divert\_text](#m4_005fdivert_005ftext)) (if done as an argument to
    `AC_ARG_ENABLE`, also provide non-diverted text to avoid a shell
    syntax error).

    You should format your help-string with the macro
    `AS_HELP_STRING` (see [Making Your Help Strings Look Pretty](#Pretty-Help-Strings)).

    See the examples suggested with the definition of `AC_ARG_WITH`
    (see [Working With External Software](#External-Software)) to get an idea of possible applications of
    `AC_ARG_ENABLE`.

---

Next: [Controlling Checking of `configure` Options](#Option-Checking), Previous: [Choosing Package Options](#Package-Options), Up: [Site Configuration](#Site-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.4 Making Your Help Strings Look Pretty [¶](#Making-Your-Help-Strings-Look-Pretty)

Properly formatting the ‘`help strings`’ which are used in
`AC_ARG_WITH` (see [Working With External Software](#External-Software)) and `AC_ARG_ENABLE`
(see [Choosing Package Options](#Package-Options)) can be challenging. Specifically, you want
your own ‘`help strings`’ to line up in the appropriate columns of
‘`configure --help`’ just like the standard Autoconf ‘`help
strings`’ do. This is the purpose of the `AS_HELP_STRING` macro.

Macro: **AS\_HELP\_STRING**(left-hand-side, right-hand-side [indent-column = ‘`26`’], [wrap-column = ‘`79`’]) [¶](#index-AS_005fHELP_005fSTRING-1)
:   Expands into a help string that looks pretty when the user executes
    ‘`configure --help`’. It is typically used in `AC_ARG_WITH`
    (see [Working With External Software](#External-Software)) or `AC_ARG_ENABLE` (see [Choosing Package Options](#Package-Options)). The following example makes this clearer.

    ```
    AC_ARG_WITH([foo],
      [AS_HELP_STRING([--with-foo],
         [use foo (default is no)])],
      [use_foo=$withval],
      [use_foo=no])
    ```

    Then the last few lines of ‘`configure --help`’ appear like
    this:

    ```
    --enable and --with options recognized:
      --with-foo              use foo (default is no)
    ```

    Macro expansion is performed on the first argument. However, the second
    argument of `AS_HELP_STRING` is treated as a whitespace separated
    list of text to be reformatted, and is not subject to macro expansion.
    Since it is not expanded, it should not be double quoted.
    See [The Autoconf Language](#Autoconf-Language), for a more detailed explanation.

    The `AS_HELP_STRING` macro is particularly helpful when the
    left-hand-side and/or right-hand-side are composed of macro
    arguments, as shown in the following example. Be aware that
    left-hand-side may not expand to unbalanced quotes,
    although quadrigraphs can be used.

    ```
    AC_DEFUN([MY_ARG_WITH],
      [AC_ARG_WITH(m4_translit([[$1]], [_], [-]),
         [AS_HELP_STRING([--with-m4_translit([$1], [_], [-])],
                         [use $1 (default is $2)])],
         [use_[]$1=$withval],
         [use_[]$1=$2])])
    MY_ARG_WITH([a_b], [no])
    ```

    Here, the last few lines of ‘`configure --help`’ will include:

    ```
    --enable and --with options recognized:
      --with-a-b              use a_b (default is no)
    ```

    The parameters indent-column and wrap-column were introduced
    in Autoconf 2.62. Generally, they should not be specified; they exist
    for fine-tuning of the wrapping.

    ```
    AS_HELP_STRING([--option], [description of option])
    ⇒  --option                description of option
    AS_HELP_STRING([--option], [description of option], [15], [30])
    ⇒  --option     description of
    ⇒               option
    ```

---

Next: [Configuring Site Details](#Site-Details), Previous: [Making Your Help Strings Look Pretty](#Pretty-Help-Strings), Up: [Site Configuration](#Site-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.5 Controlling Checking of `configure` Options [¶](#Controlling-Checking-of-configure-Options)

The `configure` script checks its command-line options against a
list of known options, like `--help` or `--config-cache`.
An unknown option ordinarily indicates a mistake by the user and
`configure` halts with an error. However, by default unknown
`--with-package` and `--enable-feature`
options elicit only a warning, to support configuring entire source
trees.

Source trees often contain multiple packages with a top-level
`configure` script that uses the `AC_CONFIG_SUBDIRS` macro
(see [Configuring Other Packages in Subdirectories](#Subdirectories)). Because the packages generally support
different `--with-package` and
`--enable-feature` options, the GNU Coding
Standards say they must accept unrecognized options without halting.
Even a warning message is undesirable here, so `AC_CONFIG_SUBDIRS`
automatically disables the warnings.

This default behavior may be modified in two ways. First, the installer
can invoke `configure --disable-option-checking` to disable
these warnings, or invoke `configure --enable-option-checking=fatal`
options to turn them into fatal errors, respectively. Second, the
maintainer can use `AC_DISABLE_OPTION_CHECKING`.

Macro: **AC\_DISABLE\_OPTION\_CHECKING** [¶](#index-AC_005fDISABLE_005fOPTION_005fCHECKING-1)
:   By default, disable warnings related to any unrecognized
    `--with-package` or `--enable-feature`
    options. This is implied by `AC_CONFIG_SUBDIRS`.

    The installer can override this behavior by passing
    `--enable-option-checking` (enable warnings) or
    `--enable-option-checking=fatal` (enable errors) to
    `configure`.

---

Next: [Transforming Program Names When Installing](#Transforming-Names), Previous: [Controlling Checking of `configure` Options](#Option-Checking), Up: [Site Configuration](#Site-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.6 Configuring Site Details [¶](#Configuring-Site-Details)

Some software packages require complex site-specific information. Some
examples are host names to use for certain services, company names, and
email addresses to contact. Since some configuration scripts generated
by Metaconfig ask for such information interactively, people sometimes
wonder how to get that information in Autoconf-generated configuration
scripts, which aren’t interactive.

Such site configuration information should be put in a file that is
edited *only by users*, not by programs. The location of the file
can either be based on the `prefix` variable, or be a standard
location such as the user’s home directory. It could even be specified
by an environment variable. The programs should examine that file at
runtime, rather than at compile time. Runtime configuration is more
convenient for users and makes the configuration process simpler than
getting the information while configuring. See [Variables for Installation Directories](https://www.gnu.org/prep/standards/standards.html#Directory-Variables) in The GNU Coding
Standards, for more information on where to put data files.

---

Next: [Setting Site Defaults](#Site-Defaults), Previous: [Configuring Site Details](#Site-Details), Up: [Site Configuration](#Site-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.7 Transforming Program Names When Installing [¶](#Transforming-Program-Names-When-Installing)

Autoconf supports changing the names of programs when installing them.
In order to use these transformations, `configure.ac` must call the
macro `AC_ARG_PROGRAM`.

Macro: **AC\_ARG\_PROGRAM** [¶](#index-AC_005fARG_005fPROGRAM-1)
:   Place in output variable `program_transform_name` a sequence of
    `sed` commands for changing the names of installed programs.

    If any of the options described below are given to `configure`,
    program names are transformed accordingly. Otherwise, if
    `AC_CANONICAL_TARGET` has been called and a `--target` value
    is given, the target type followed by a dash is used as a prefix.
    Otherwise, no program name transformation is done.

* [Transformation Options](#Transformation-Options)
* [Transformation Examples](#Transformation-Examples)
* [Transformation Rules](#Transformation-Rules)

---

Next: [Transformation Examples](#Transformation-Examples), Up: [Transforming Program Names When Installing](#Transforming-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.7.1 Transformation Options [¶](#Transformation-Options-1)

You can specify name transformations by giving `configure` these
command line options:

`--program-prefix=prefix`
:   prepend prefix to the names;

`--program-suffix=suffix`
:   append suffix to the names;

`--program-transform-name=expression`
:   perform `sed` substitution expression on the names.

---

Next: [Transformation Rules](#Transformation-Rules), Previous: [Transformation Options](#Transformation-Options), Up: [Transforming Program Names When Installing](#Transforming-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.7.2 Transformation Examples [¶](#Transformation-Examples-1)

These transformations are useful with programs that can be part of a
cross-compilation development environment. For example, a
cross-assembler running on x86-64 configured with
`--target=aarch64-linux-gnu` is normally installed as
`aarch64-linux-gnu-as`, rather than `as`, which could be confused
with a native x86-64 assembler.

You can force a program name to begin with `g`, if you don’t want
GNU programs installed on your system to shadow other programs with
the same name. For example, if you configure GNU `diff` with
`--program-prefix=g`, then when you run ‘`make install`’ it is
installed as `/usr/local/bin/gdiff`.

As a more sophisticated example, you could use

```
--program-transform-name='s/^/g/; s/^gg/g/; s/^gless/less/'
```

to prepend ‘`g`’ to most of the program names in a source tree,
excepting those like `gdb` that already have one and those like
`less` and `lesskey` that aren’t GNU programs. (That is
assuming that you have a source tree containing those programs that is
set up to use this feature.)

One way to install multiple versions of some programs simultaneously is
to append a version number to the name of one or both. For example, if
you want to keep Autoconf version 1 around for awhile, you can configure
Autoconf version 2 using `--program-suffix=2` to install the
programs as `/usr/local/bin/autoconf2`,
`/usr/local/bin/autoheader2`, etc. Nevertheless, pay attention
that only the binaries are renamed, therefore you’d have problems with
the library files which might overlap.

---

Previous: [Transformation Examples](#Transformation-Examples), Up: [Transforming Program Names When Installing](#Transforming-Names)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 15.7.3 Transformation Rules [¶](#Transformation-Rules-1)

Here is how to use the variable `program_transform_name` in a
`Makefile.in`:

```
PROGRAMS = cp ls rm
transform = @program_transform_name@
install:
        for p in $(PROGRAMS); do \
          $(INSTALL_PROGRAM) $$p $(DESTDIR)$(bindir)/`echo $$p | \
                                              sed '$(transform)'`; \
        done

uninstall:
        for p in $(PROGRAMS); do \
          rm -f $(DESTDIR)$(bindir)/`echo $$p | sed '$(transform)'`; \
        done
```

It is guaranteed that `program_transform_name` is never empty, and
that there are no useless separators. Therefore you may safely embed
`program_transform_name` within a sed program using ‘`;`’:

```
transform = @program_transform_name@
transform_exe = s/$(EXEEXT)$$//;$(transform);s/$$/$(EXEEXT)/
```

Whether to do the transformations on documentation files (Texinfo or
`man`) is a tricky question; there seems to be no perfect answer,
due to the several reasons for name transforming. Documentation is not
usually particular to a specific architecture, and Texinfo files do not
conflict with system documentation. But they might conflict with
earlier versions of the same files, and `man` pages sometimes do
conflict with system documentation. As a compromise, it is probably
best to do name transformations on `man` pages but not on Texinfo
manuals.

---

Previous: [Transforming Program Names When Installing](#Transforming-Names), Up: [Site Configuration](#Site-Configuration)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 15.8 Setting Site Defaults [¶](#Setting-Site-Defaults)

Autoconf-generated `configure` scripts allow your site to provide
default values for some configuration values. You do this by creating
site- and system-wide initialization files.

If the environment variable `CONFIG_SITE` is set, `configure`
uses its value as a space-separated list of shell scripts to read;
it is recommended that these be absolute file names. Otherwise, it
reads the shell script `prefix/share/config.site` if it exists,
then `prefix/etc/config.site` if it exists. Thus,
settings in machine-specific files override those in machine-independent
ones in case of conflict.

Site files can be arbitrary shell scripts, but only certain kinds of
code are really appropriate to be in them. Because `configure`
reads any cache file after it has read any site files, a site file can
define a default cache file to be shared between all Autoconf-generated
`configure` scripts run on that system (see [Cache Files](#Cache-Files)). If
you set a default cache file in a site file, it is a good idea to also
set the output variable `CC` in that site file, because the cache
file is only valid for a particular compiler, but many systems have
several available.

You can examine or override the value set by a command line option to
`configure` in a site file; options set shell variables that have
the same names as the options, with any dashes turned into underscores.
The exceptions are that `--without-` and `--disable-` options
are like giving the corresponding `--with-` or `--enable-`
option and the value ‘`no`’. Thus, `--cache-file=localcache`
sets the variable `cache_file` to the value ‘`localcache`’;
`--enable-warnings=no` or `--disable-warnings` sets the variable
`enable_warnings` to the value ‘`no`’; `--prefix=/usr` sets the
variable `prefix` to the value ‘`/usr`’; etc.

Site files are also good places to set default values for other output
variables, such as `CFLAGS`, if you need to give them non-default
values: anything you would normally do, repetitively, on the command
line. If you use non-default values for prefix or
exec\_prefix (wherever you locate the site file), you can set them
in the site file if you specify it with the `CONFIG_SITE`
environment variable.

You can set some cache values in the site file itself. Doing this is
useful if you are cross-compiling, where it is impossible to check features
that require running a test program. You could “prime the cache” by
setting those values correctly for that system in
`prefix/etc/config.site`. To find out the names of the cache
variables you need to set, see the documentation of the respective
Autoconf macro. If the variables or their semantics are undocumented,
you may need to look for shell variables with ‘`_cv_`’ in their names
in the affected `configure` scripts, or in the Autoconf M4
source code for those macros; but in that case, their name or semantics
may change in a future Autoconf version.

The cache file is careful to not override any variables set in the site
files. Similarly, you should not override command-line options in the
site files. Your code should check that variables such as `prefix`
and `cache_file` have their default values (as set near the top of
`configure`) before changing them.

Here is a sample file `/usr/share/local/gnu/share/config.site`. The
command ‘`configure --prefix=/usr/share/local/gnu`’ would read this
file (if `CONFIG_SITE` is not set to a different file).

```
# /usr/share/local/gnu/share/config.site for configure
#
# Change some defaults.
test "$prefix" = NONE && prefix=/usr/share/local/gnu
test "$exec_prefix" = NONE && exec_prefix=/usr/local/gnu
test "$sharedstatedir" = '${prefix}/com' && sharedstatedir=/var
test "$localstatedir" = '${prefix}/var' && localstatedir=/var
test "$runstatedir" = '${localstatedir}/run' && runstatedir=/run

# Give Autoconf 2.x generated configure scripts a shared default
# cache file for feature test results, architecture-specific.
if test "$cache_file" = /dev/null; then
  cache_file="$prefix/var/config.cache"
  # A cache file is only valid for one C compiler.
  CC=gcc
fi
```

Another use of `config.site` is for priming the directory variables
in a manner consistent with the Filesystem Hierarchy Standard
(FHS). Once the following file is installed at
`/usr/share/config.site`, a user can execute simply
`./configure --prefix=/usr` to get all the directories chosen in
the locations recommended by FHS.

```
# /usr/share/config.site for FHS defaults when installing below /usr,
# and the respective settings were not changed on the command line.
if test "$prefix" = /usr; then
  test "$sysconfdir" = '${prefix}/etc' && sysconfdir=/etc
  test "$sharedstatedir" = '${prefix}/com' && sharedstatedir=/var
  test "$localstatedir" = '${prefix}/var' && localstatedir=/var
fi
```

Likewise, on platforms where 64-bit libraries are built by default, then
installed in `/usr/local/lib64` instead of `/usr/local/lib`,
it is appropriate to install `/usr/local/share/config.site`:

```
# /usr/local/share/config.site for platforms that prefer
