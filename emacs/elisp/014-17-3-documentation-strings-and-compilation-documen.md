### 17.3 Documentation Strings and Compilation [¶](#Documentation-Strings-and-Compilation)

When Emacs loads functions and variables from a byte-compiled file,
it normally does not load their documentation strings into memory.
Each documentation string is dynamically loaded from the
byte-compiled file only when needed. This saves memory, and speeds up
loading by skipping the processing of the documentation strings.

This feature has a drawback: if you delete, move, or alter the
compiled file (such as by compiling a new version), Emacs may no
longer be able to access the documentation string of previously-loaded
functions or variables. Such a problem normally only occurs if you
build Emacs yourself, and happen to edit and/or recompile the Lisp
source files. To solve it, just reload each file after recompilation.

Dynamic loading of documentation strings from byte-compiled files is
determined, at compile time, for each byte-compiled file. It can be
disabled via the option `byte-compile-dynamic-docstrings`.

User Option: **byte-compile-dynamic-docstrings** [¶](#index-byte_002dcompile_002ddynamic_002ddocstrings)
:   If this is non-`nil`, the byte compiler generates compiled files
    that are set up for dynamic loading of documentation strings.

    To disable the dynamic loading feature for a specific file, set this
    option to `nil` in its header line (see [Local Variables in Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#File-Variables) in The GNU Emacs Manual), like this:

    ```
    -*-byte-compile-dynamic-docstrings: nil;-*-
    ```

    This is useful mainly if you expect to change the file, and you want
    Emacs sessions that have already loaded it to keep working when the
    file changes.

Internally, the dynamic loading of documentation strings is
accomplished by writing compiled files with a special Lisp reader
construct, ‘`#@count`’. This construct skips the next
count characters. It also uses the ‘`#$`’ construct, which
stands for the name of this file, as a string. Do not use these
constructs in Lisp source files; they are not designed to be clear to
humans reading the file.

---

Next: [Compiler Errors](#Compiler-Errors), Previous: [Documentation Strings and Compilation](#Docs-and-Compilation), Up: [Byte Compilation](#Byte-Compilation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.4 Evaluation During Compilation [¶](#Evaluation-During-Compilation)

These features permit you to write code to be evaluated during
compilation of a program.

Macro: **eval-and-compile** body… [¶](#index-eval_002dand_002dcompile)
:   This form marks body to be evaluated both when you compile the
    containing code and when you run it (whether compiled or not).

    You can get a similar result by putting body in a separate file
    and referring to that file with `require`. That method is
    preferable when body is large. Effectively `require` is
    automatically `eval-and-compile`, the package is loaded both when
    compiling and executing.

    `autoload` is also effectively `eval-and-compile` too. It’s
    recognized when compiling, so uses of such a function don’t produce
    “not known to be defined” warnings.

    Most uses of `eval-and-compile` are fairly sophisticated.

    If a macro has a helper function to build its result, and that macro
    is used both locally and outside the package, then
    `eval-and-compile` should be used to get the helper both when
    compiling and then later when running.

    If functions are defined programmatically (with `fset` say), then
    `eval-and-compile` can be used to have that done at compile-time
    as well as run-time, so calls to those functions are checked (and
    warnings about “not known to be defined” suppressed).

Macro: **eval-when-compile** body… [¶](#index-eval_002dwhen_002dcompile)
:   This form marks body to be evaluated at compile time but not when
    the compiled program is loaded. The result of evaluation by the
    compiler becomes a constant which appears in the compiled program. If
    you load the source file, rather than compiling it, body is
    evaluated normally.

    If you have a constant that needs some calculation to produce,
    `eval-when-compile` can do that at compile-time. For example,

    ```
    (defvar gauss-schoolboy-problem
      (eval-when-compile (apply #'+ (number-sequence 1 100))))
    ```

    If you’re using another package, but only need macros from it (the
    byte compiler will expand those), then `eval-when-compile` can be
    used to load it for compiling, but not executing. For example,

    ```
    (eval-when-compile
      (require 'my-macro-package))
    ```

    The same sort of thing goes for macros and `defsubst` functions
    defined locally and only for use within the file. They are needed for
    compiling the file, but in most cases they are not needed for
    execution of the compiled file. For example,

    ```
    (eval-when-compile
      (unless (fboundp 'some-new-thing)
        (defmacro some-new-thing ()
          (compatibility code))))
    ```

    This is often good for code that’s only a fallback for compatibility
    with other versions of Emacs.

    **Common Lisp Note:** At top level, `eval-when-compile` is analogous to the Common
    Lisp idiom `(eval-when (compile eval) …)`. Elsewhere, the
    Common Lisp ‘`#.`’ reader macro (but not when interpreting) is closer
    to what `eval-when-compile` does.

---

Next: [Closure Function Objects](#Closure-Objects), Previous: [Evaluation During Compilation](#Eval-During-Compile), Up: [Byte Compilation](#Byte-Compilation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.5 Compiler Errors [¶](#Compiler-Errors-1)

Error and warning messages from byte compilation are printed in a
buffer named `*Compile-Log*`. These messages include file names
and line numbers identifying the location of the problem. The usual
Emacs commands for operating on compiler output can be used on these
messages.

When an error is due to invalid syntax in the program, the byte
compiler might get confused about the error’s exact location. One way
to investigate is to switch to the buffer `*Compiler Input*`. (This buffer name starts with a space, so it does not show
up in the Buffer Menu.) This buffer contains the program being
compiled, and point shows how far the byte compiler was able to read;
the cause of the error might be nearby. See [Debugging Invalid Lisp Syntax](#Syntax-Errors), for
some tips for locating syntax errors.

A common type of warning issued by the byte compiler is for
functions and variables that were used but not defined. Such warnings
report the line number for the end of the file, not the locations
where the missing functions or variables were used; to find these, you
must search the file manually.

If you are sure that a warning message about a missing function or
variable is unjustified, there are several ways to suppress it:

* You can suppress the warning for a specific call to a function
  func by conditionalizing it on an `fboundp` test, like
  this:

  ```
  (if (fboundp 'func) ...(func ...)...)
  ```

  The call to func must be in the then-form of the
  `if`, and func must appear quoted in the call to
  `fboundp`. (This feature operates for `cond` as well.)
* Likewise, you can suppress the warning for a specific use of a
  variable variable by conditionalizing it on a `boundp`
  test:

  ```
  (if (boundp 'variable) ...variable...)
  ```

  The reference to variable must be in the then-form of the
  `if`, and variable must appear quoted in the call to
  `boundp`.
* You can tell the compiler that a function is defined using
  `declare-function`. See [Telling the Compiler that a Function is Defined](#Declaring-Functions).
* Likewise, you can tell the compiler that a variable is defined using
  `defvar` with no initial value. (Note that this marks the
  variable as special, i.e. dynamically bound, but only within the
  current lexical scope, or file if at top-level.) See [Defining Global Variables](#Defining-Variables).

You can also suppress compiler warnings within a certain expression
using the `with-suppressed-warnings` macro:

Special Form: **with-suppressed-warnings** warnings body… [¶](#index-with_002dsuppressed_002dwarnings)
:   In execution, this is equivalent to `(progn body...)`, but
    the compiler does not issue warnings for the specified conditions in
    body. warnings is an association list of warning symbols
    and function/variable symbols they apply to. For instance, if you
    wish to call an obsolete function called `foo`, but want to
    suppress the compilation warning, say:

    ```
    (with-suppressed-warnings ((obsolete foo))
      (foo ...))
    ```

For more coarse-grained suppression of compiler warnings, you can use
the `with-no-warnings` construct:

Special Form: **with-no-warnings** body… [¶](#index-with_002dno_002dwarnings)
:   In execution, this is equivalent to `(progn body...)`,
    but the compiler does not issue warnings for anything that occurs
    inside body.

    We recommend that you use `with-suppressed-warnings` instead, but
    if you do use this construct, that you use it around the smallest
    possible piece of code to avoid missing possible warnings other than
    one you intend to suppress.

Byte compiler warnings can be controlled more precisely by setting
the variable `byte-compile-warnings`. See its documentation
string for details.

Sometimes you may wish the byte-compiler warnings to be reported
using `error`. If so, set `byte-compile-error-on-warn` to a
non-`nil` value.

---

Next: [Disassembled Byte-Code](#Disassembly), Previous: [Compiler Errors](#Compiler-Errors), Up: [Byte Compilation](#Byte-Compilation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.6 Closure Function Objects [¶](#Closure-Function-Objects)

Byte-compiled functions use a special data type: they are closures.
Closures are used both for byte-compiled Lisp functions as well as for
interpreted Lisp functions. Whenever such an object appears as
a function to be called, Emacs uses the appropriate interpreter to
execute either the byte-code or the non-compiled Lisp code.

Internally, a closure is much like a vector; its
elements can be accessed using `aref`. Its printed
representation is like that for a vector, with an additional ‘`#`’
before the opening ‘`[`’. It must have at least three elements;
there is no maximum number, but only the first six elements have any
normal use. They are:

argdesc
:   The descriptor of the arguments. This can either be a list of
    arguments, as described in [Features of Argument Lists](#Argument-List), or an integer encoding
    the required number of arguments. In the latter case, the value of
    the descriptor specifies the minimum number of arguments in the bits
    zero to 6, and the maximum number of arguments in bits 8 to 14. If
    the argument list uses `&rest`, then bit 7 is set; otherwise it’s
    cleared.

    When the closure is a byte-code function,
    if argdesc is a list, the arguments will be dynamically bound
    before executing the byte code. If argdesc is an integer, the
    arguments will be instead pushed onto the stack of the byte-code
    interpreter, before executing the code.

code
:   For interpreted functions, this element is the (non-empty) list of Lisp
    forms that make up the function’s body. For byte-compiled functions, it
    is the string containing the byte-code instructions.

constants
:   For byte-compiled functions, this holds the vector of Lisp objects
    referenced by the byte code. These include symbols used as function
    names and variable names.
    For interpreted functions, this is `nil` if the function is using the old
    dynamically scoped dialect of Emacs Lisp, and otherwise it holds the
    function’s lexical environment.

stacksize
:   The maximum stack size this function needs. This element is left unused
    for interpreted functions.

docstring
:   The documentation string (if any); otherwise, `nil`. The value may
    be a number or a list, in case the documentation string is stored in a
    file. Use the function `documentation` to get the real
    documentation string (see [Access to Documentation Strings](#Accessing-Documentation)).

interactive
:   The interactive spec (if any). This can be a string or a Lisp
    expression. It is `nil` for a function that isn’t interactive.

Here’s an example of a byte-code function object, in printed
representation. It is the definition of the command
`backward-sexp`.

```
#[256
  "\211\204^G^@\300\262^A\301^A[!\207"
  [1 forward-sexp]
  3
  1793299
  "^p"]
```

The primitive way to create a byte-code object is with
`make-byte-code`:

Function: **make-byte-code** &rest elements [¶](#index-make_002dbyte_002dcode)
:   This function constructs and returns a closure which represents the
    byte-code function object with elements as its elements.

You should not try to come up with the elements for a byte-code
function yourself, because if they are inconsistent, Emacs may crash
when you call the function. Always leave it to the byte compiler to
create these objects; it makes the elements consistent (we hope).

The primitive way to create an interpreted function is with
`make-interpreted-closure`:

Function: **make-interpreted-closure** args body env &optional docstring iform [¶](#index-make_002dinterpreted_002dclosure)
:   This function constructs and returns a closure representing the
    interpreted function with arguments args and whose body is made of
    body which must be a non-`nil` list of Lisp forms. env is the
    lexical environment in the same form as used with `eval`
    (see [Eval](#Eval)). The documentation docstring if non-`nil` should be
    a string, and the interactive form iform if non-`nil` should be of
    the form `(interactive arg-descriptor)` (see [Using `interactive`](#Using-Interactive)).

---

Previous: [Closure Function Objects](#Closure-Objects), Up: [Byte Compilation](#Byte-Compilation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.7 Disassembled Byte-Code [¶](#Disassembled-Byte_002dCode)

People do not write byte-code; that job is left to the byte
compiler. But we provide a disassembler to satisfy a cat-like
curiosity. The disassembler converts the byte-compiled code into
human-readable form.

The byte-code interpreter is implemented as a simple stack machine.
It pushes values onto a stack of its own, then pops them off to use them
in calculations whose results are themselves pushed back on the stack.
When a byte-code function returns, it pops a value off the stack and
returns it as the value of the function.

In addition to the stack, byte-code functions can use, bind, and set
ordinary Lisp variables, by transferring values between variables and
the stack.

Command: **disassemble** object &optional buffer-or-name [¶](#index-disassemble)
:   This command displays the disassembled code for object. In
    interactive use, or if buffer-or-name is `nil` or omitted,
    the output goes in a buffer named `*Disassemble*`. If
    buffer-or-name is non-`nil`, it must be a buffer or the
    name of an existing buffer. Then the output goes there, at point, and
    point is left before the output.

    The argument object can be a function name, a lambda expression
    (see [Lambda Expressions](#Lambda-Expressions)), or a byte-code object (see [Closure Function Objects](#Closure-Objects)). If it is a lambda expression, `disassemble` compiles
    it and disassembles the resulting compiled code.

Here are two examples of using the `disassemble` function. We
have added explanatory comments to help you relate the byte-code to the
Lisp source; these do not appear in the output of `disassemble`.

```
(defun factorial (integer)
  "Compute factorial of an integer."
  (if (= 1 integer) 1
    (* integer (factorial (1- integer)))))
     ⇒ factorial
```

```

```

```
(factorial 4)
     ⇒ 24
```

```

```

```
(disassemble 'factorial)
     -| byte-code for factorial:
 doc: Compute factorial of an integer.
 args: (arg1)
```

```

```

```
0   dup                   ; Get the value of integer and
                          ;   push it onto the stack.
1   constant 1            ; Push 1 onto stack.
```

```
2   eqlsign               ; Pop top two values off stack, compare
                          ;   them, and push result onto stack.
```

```
3   goto-if-nil 1         ; Pop and test top of stack;
                          ;   if nil, go to 1, else continue.
6   constant 1            ; Push 1 onto top of stack.
7   return                ; Return the top element of the stack.
```

```
8:1 dup                   ; Push value of integer onto stack.
9   constant factorial    ; Push factorial onto stack.
10  stack-ref 2           ; Push value of integer onto stack.
11  sub1                  ; Pop integer, decrement value,
                          ;   push new value onto stack.
12  call     1            ; Call function factorial using first
                          ;   (i.e., top) stack element as argument;
                          ;   push returned value onto stack.
```

```
13  mult                  ; Pop top two values off stack, multiply
                          ;   them, and push result onto stack.
14  return                ; Return the top element of the stack.
```

The `silly-loop` function is somewhat more complex:

```
(defun silly-loop (n)
  "Return time before and after N iterations of a loop."
  (let ((t1 (current-time-string)))
    (while (> (setq n (1- n))
              0))
    (list t1 (current-time-string))))
     ⇒ silly-loop
```

```

```

```
(disassemble 'silly-loop)
     -| byte-code for silly-loop:
 doc: Return time before and after N iterations of a loop.
 args: (arg1)
```

```

```

```
0   constant current-time-string  ; Push current-time-string
                                  ;   onto top of stack.
```

```
1   call     0            ; Call current-time-string with no
                          ;   argument, push result onto stack as t1.
```

```
2:1 stack-ref 1           ; Get value of the argument n
                          ;   and push the value on the stack.
3   sub1                  ; Subtract 1 from top of stack.
```

```
4   dup                   ; Duplicate top of stack; i.e., copy the top
                          ;   of the stack and push copy onto stack.
5   stack-set 3           ; Pop the top of the stack,
                          ;   and set n to the value.

;; (In effect, the sequence dup stack-set copies the top of
;; the stack into the value of n without popping it.)
```

```

```

```
7   constant 0            ; Push 0 onto stack.
8   gtr                   ; Pop top two values off stack,
                          ;   test if n is greater than 0
                          ;   and push result onto stack.
```

```
9   goto-if-not-nil 1     ; Goto 1 if n > 0
                          ;   (this continues the while loop)
                          ;   else continue.
```

```
12  dup                   ; Push value of t1 onto stack.
13  constant current-time-string  ; Push current-time-string
                                  ;   onto the top of the stack.
14  call     0            ; Call current-time-string again.
```

```
15  list2                 ; Pop top two elements off stack, create a
                          ;   list of them, and push it onto stack.
16  return                ; Return value of the top of stack.
```

---

Next: [Debugging Lisp Programs](#Debugging), Previous: [Byte Compilation](#Byte-Compilation), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 18 Compilation of Lisp to Native Code [¶](#Compilation-of-Lisp-to-Native-Code)

In addition to the byte-compilation, described in [the previous chapter](#Byte-Compilation), Emacs can also optionally compile
Lisp function definitions into a true compiled code, known as
*native code*. This feature uses the `libgccjit` library,
which is part of the GCC distribution, and requires that Emacs be
built with support for using that library. It also requires GCC and
Binutils (the assembler and linker) to be available on your system for
you to be able to native-compile Lisp code.

To determine whether the current Emacs process can produce and load
natively compiled Lisp code, call
`native-comp-available-p` (see [Native-Compilation Functions](#Native_002dCompilation-Functions)).

Unlike byte-compiled code, native-compiled Lisp code is executed
directly by the machine’s hardware, and therefore runs at full speed
that the host CPU can provide. The resulting speedup generally
depends on what the Lisp code does, but is usually 2.5 to 5 times
faster than the corresponding byte-compiled code.

Since native code is generally incompatible between different
systems, the native-compiled code is *not* transportable from
one machine to another, it can only be used on the same machine where
it was produced or on very similar ones (having the same CPU and
run-time libraries). The transportability of native-compiled code
is the same as that of shared libraries (`.so` or `.dll`
files).

Libraries of native-compiled code include crucial dependencies on
Emacs Lisp primitives (see [What Is a Function?](#What-Is-a-Function)) and their calling
conventions, and thus Emacs usually won’t load native-compiled code
produced by earlier or later Emacs versions; native compilation of the
same Lisp code by a different Emacs version will usually produce a
natively compiled library under a unique file name that only that
version of Emacs will be able to load. However, the use of unique
file names enables several versions of the same Lisp library
natively compiled by several different versions of Emacs to be placed
within the same directory.

A non-`nil` file-local variable binding of
`no-byte-compile` (see [Byte Compilation](#Byte-Compilation)) also disables the
native compilation of that file. In addition, a similar variable
`no-native-compile` disables just the native compilation of the
file. If both `no-byte-compile` and `no-native-compile` are
specified, the former takes precedence.

Sometimes there could be a need to prevent the native compilation
from writing its results, the `*.eln` files, into a subdirectory
of `user-emacs-directory` (see [The Init File](#Init-File)). You can do that
by either changing the value of `native-comp-eln-load-path`
(see [Native-Compilation Variables](#Native_002dCompilation-Variables)) or by temporarily pointing the
`HOME` environment variable to a non-existing directory. Note
that the latter technique might still produce a small number of
`*.eln` files if Emacs needs to generate *trampolines*, which
are used if Lisp primitives are advised or redefined in your Lisp code
that is being natively compiled. See [trampolines](#Native_002dCompilation-Variables). Alternatively, you can specify that the `*.eln`
files are written to a non-default directory using the
`startup-redirect-eln-cache` function; see [Native-Compilation Functions](#Native_002dCompilation-Functions).

* [Native-Compilation Functions](#Native_002dCompilation-Functions)
* [Native-Compilation Variables](#Native_002dCompilation-Variables)

---

Next: [Native-Compilation Variables](#Native_002dCompilation-Variables), Up: [Compilation of Lisp to Native Code](#Native-Compilation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 18.1 Native-Compilation Functions [¶](#Native_002dCompilation-Functions-1)

Native-Compilation is implemented as a side effect of
byte-compilation (see [Byte Compilation](#Byte-Compilation)). Thus, compiling Lisp
code natively always produces its byte code as well, and therefore all
the rules and caveats of preparing Lisp code for byte compilation
(see [Byte-Compilation Functions](#Compilation-Functions)) are valid for native-compilation as
well.

You can natively-compile either a single function or macro
definition, or a whole file of Lisp code, with the
`native-compile` function. Natively-compiling a file will
produce the `.eln` file with native code.

Native compilation might produce warning or error messages; these
are normally recorded in the buffer called
`*Native-compile-Log*`. In interactive sessions, it uses the
special LIMPLE mode (`native-comp-limple-mode`), which sets up
`font-lock` as appropriate for this log, and is otherwise the
same as Fundamental mode. Logging of messages resulting from
native-compilation can be controlled by the `native-comp-verbose`
variable (see [Native-Compilation Variables](#Native_002dCompilation-Variables)).

When Emacs is run non-interactively, messages produced by
native-compilation are reported by calling `message`
(see [Displaying Messages in the Echo Area](#Displaying-Messages)), and are usually displayed on the
standard error stream of the terminal from which Emacs was invoked.

Function: **native-compile** function-or-file &optional output [¶](#index-native_002dcompile)
:   This function compiles function-or-file into native code. The
    argument function-or-file can be a function symbol, a Lisp form,
    or a name (a string) of the file which contains the Emacs Lisp source
    code to compile. If the optional argument output is provided,
    it must be a string specifying the name of the file to write the
    compiled code into. Otherwise, if function-or-file is a
    function or a Lisp form, this function returns the compiled object,
    and if function-or-file is a file name, the function returns the
    full absolute name of the file it created for the compiled code. The
    output file is by default given the `.eln` extension.

    This function runs the final phase of the native compilation, which
    invokes GCC via `libgccjit`, in a separate subprocess, which
    invokes the same Emacs executable as the process that called this
    function.

Function: **native-compile-directory** directory [¶](#index-native_002dcompile_002ddirectory)
:   This function recursively searches for `.el` files in
    directory and any subdirectories thereof, and runs
    native-compilation on any that have not already been compiled. To
    determine whether a file has already been compiled, it searches for a
    corresponding `.eln` in the `native-comp-eln-load-path`
    variable (see [Native-Compilation Variables](#Native_002dCompilation-Variables)).

Function: **batch-native-compile** &optional for-tarball [¶](#index-batch_002dnative_002dcompile)
:   This function runs native-compilation on files specified on the Emacs
    command line in batch mode. It must be used only in a batch execution
    of Emacs, as it kills Emacs upon completion of the compilation. If
    one or more of the files fail to compile, the Emacs process will
    attempt to compile all the other files, and will terminate with a
    non-zero status code. The optional argument for-tarball, if
    non-`nil`, tells the function to place the resulting `.eln`
    files in the last directory mentioned in
    `native-comp-eln-load-path` (see [Library Search](#Library-Search)); this is
    meant to be used as part of building an Emacs source tarball for the
    first time, when the native-compiled files, which are absent from
    the source tarball, should be generated in the build tree instead of
    the user’s cache directory.

Native compilation can be run entirely asynchronously, in a subprocess
of the main Emacs process. This leaves the main Emacs process free to
use while the compilation runs in the background. This is the method
used by Emacs to natively-compile any Lisp file or byte-compiled Lisp
file that is loaded into Emacs, when no native-compiled file for it
is available. Note that because of this use of a subprocess, native
compilation may produce warning and errors which byte-compilation does
not, and Lisp code may thus need to be modified to work correctly. See
`native-comp-async-report-warnings-errors` in see [Native-Compilation Variables](#Native_002dCompilation-Variables) for more details.

Function: **native-compile-async** files &optional recursively load selector [¶](#index-native_002dcompile_002dasync)
:   This function compiles the named files asynchronously. The
    argument files should be a single file name (a string) or a list
    of one or more file and/or directory names. If directories are
    present in the list, the optional argument recursively should be
    non-`nil` to cause the compilation to recurse into those
    directories. If load is non-`nil`, Emacs will load each
    file that it succeeded to compile. The optional argument
    selector allows control of which of files will be
    compiled; it can have one of the following values:

    `nil` or omitted
    :   Select all the files and directories in files.

    a regular expression string
    :   Select the files and directories whose names match the regexp.

    a function
    :   A predicate function, which will be called with each file and
        directory in files, and should return non-`nil` if the file
        or the directory should be selected for compilation.

    On systems with multiple CPU execution units, when files names
    more than one file, this function will normally start several
    compilation subprocesses in parallel, under the control of
    `native-comp-async-jobs-number` (see [Native-Compilation Variables](#Native_002dCompilation-Variables)).

Function: **native-compile-directory** directory [¶](#index-native_002dcompile_002ddirectory-1)
:   This function compiles into native code all the `*.el` files in the
    specified directory and, recursively, in all of its
    subdirectories, if a corresponding `.eln` file could not be found
    in any of the directories mentioned in the
    `native-comp-eln-load-path` list (see [Library Search](#Library-Search)).

Command: **emacs-lisp-native-compile** [¶](#index-emacs_002dlisp_002dnative_002dcompile)
:   This command compiles the file visited by the current buffer into
    native code, if the file was changed since the last time it was
    natively compiled.

Command: **emacs-lisp-native-compile-and-load** [¶](#index-emacs_002dlisp_002dnative_002dcompile_002dand_002dload)
:   This command compiles the file visited by the current buffer into
    native code, like `emacs-lisp-native-compile`, but it also loads
    the native code when the compilation finishes.

The following function allows Lisp programs to test whether
native-compilation is available at runtime.

Function: **native-comp-available-p** [¶](#index-native_002dcomp_002davailable_002dp)
:   This function returns non-`nil` if the running Emacs process has
    the native-compilation support compiled into it. On systems that load
    `libgccjit` dynamically, it also makes sure that library is
    available and can be loaded. Lisp programs that need to know up front
    whether native-compilation is available should use this predicate.

By default, asynchronous native compilation writes the `*.eln`
files it produces to a subdirectory of the first writable directory
specified by the `native-comp-eln-load-path` variable
(see [Native-Compilation Variables](#Native_002dCompilation-Variables)). You can change this by using
the following function in your startup files:

Function: **startup-redirect-eln-cache** cache-directory [¶](#index-startup_002dredirect_002deln_002dcache)
:   This function arranges for the asynchronous native compilation to
    write the produced `*.eln` files to cache-directory, which
    must be a single directory, a string. It also destructively modifies
    `native-comp-eln-load-path` such that its first element is
    cache-directory. If cache-directory is not an absolute
    file name, it is interpreted relative to `user-emacs-directory`
    (see [The Init File](#Init-File)).

---

Previous: [Native-Compilation Functions](#Native_002dCompilation-Functions), Up: [Compilation of Lisp to Native Code](#Native-Compilation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 18.2 Native-Compilation Variables [¶](#Native_002dCompilation-Variables-1)

This section documents the variables that control
native-compilation.

User Option: **native-comp-speed** [¶](#index-native_002dcomp_002dspeed)
:   This variable specifies the optimization level for native compilation.
    Its value should be a number between −1 and 3. Values between
    0 and 3 specify the optimization levels equivalent to the
    corresponding compiler `-O0`, `-O1`, etc. command-line
    options of the compiler. The value −1 means disable
    native-compilation: functions and files will be only byte-compiled;
    however, the `*.eln` files will still be produced, they will just
    contain the compiled code in bytecode form. (This can be achieved at
    function granularity by using the `(declare (speed -1))`
    form, see [The `declare` Form](#Declare-Form).)
    The default value is 2.

User Option: **compilation-safety** [¶](#index-compilation_002dsafety)
:   This variable specifies the safety level to be used for the emitted
    native code. The value should be a number, either 0 or 1 with the
    following meanings:

    0
    :   Emitted code can misbehave (up to even crashing Emacs) if a function’s
        declaration does not describe correctly what the function does or how it
        is called, and the function is natively compiled.

    1
    :   Emitted code must be generated in a safe manner even if functions are
        mis-declared.

    This can also be controlled at a function granularity, by using the
    `safety` `declare` form, see [The `declare` Form](#Declare-Form).

User Option: **native-comp-debug** [¶](#index-native_002dcomp_002ddebug)
:   This variable specifies the level of debugging information produced by
    native-compilation. Its value should be a number between zero and 3,
    with the following meaning:

    0
    :   No debugging output. This is the default.

    1
    :   Emit debugging symbols with the native code. This allows easier
        debugging of the native code with debuggers such as `gdb`.

    2
    :   Like 1, and in addition dump pseudo-C code.

    3
    :   Like 2, and in addition dump the GCC intermediate passes and
        `libgccjit` log file.

    When generated, the pseudo-C code is deposited in the same directory
    as the corresponding `.eln` file.

User Option: **native-comp-verbose** [¶](#index-native_002dcomp_002dverbose-1)
:   This variable controls the verbosity of native-compilation by
    suppressing some or all of the log messages emitted by it. If its
    value is zero, the default, all of the log messages are suppressed.
    Setting it to a value between 1 and 3 will allow logging of the
    messages whose level is above the value. The values have the
    following interpretations:

    0
    :   No logging. This is the default.

    1
    :   Log the final LIMPLE representation of the code.

    2
    :   Log the LAP, the final LIMPLE, and some additional
        pass info.

    3
    :   Maximum verbosity: log everything.

User Option: **native-comp-async-jobs-number** [¶](#index-native_002dcomp_002dasync_002djobs_002dnumber)
:   This variable determines the maximum number of native-compilation
    subprocesses that will be started simultaneously. It should be a
    non-negative number. The default value is zero, which means use half
    the number of the CPU execution units, or 1 if the CPU has only one
    execution unit.

User Option: **native-comp-async-report-warnings-errors** [¶](#index-native_002dcomp_002dasync_002dreport_002dwarnings_002derrors)
:   If this variable’s value is non-`nil`, warnings and errors from
    asynchronous native-compilation subprocesses are reported in the main
    Emacs session in a buffer named `*Warnings*`. The default value
    `t` means display the resulting buffer. To log warnings without
    popping up the `*Warnings*` buffer, set this variable to
    `silent`.

    A common cause for asynchronous native-compilation to produce
    warnings is compiling a file that is missing some `require` of a
    necessary feature. The feature may be loaded into the main emacs, but
    because native compilation always starts from a subprocess with a
    pristine environment, that may not be true for the subprocess.

User Option: **native-comp-async-query-on-exit** [¶](#index-native_002dcomp_002dasync_002dquery_002don_002dexit)
:   If this variable’s value is non-`nil`, Emacs will query upon exiting
    whether to exit and kill any asynchronous native-compilation
    subprocesses that are still running, thus preventing the corresponding
    `.eln` files from being written. If the value is `nil`, the
    default, Emacs will kill these subprocesses without querying.

The variable `native-comp-eln-load-path` holds the list of
directories where Emacs looks for the `*.eln` files
(see [Library Search](#Library-Search)); in that role it is the equivalent of
`load-path` used to look for `*.el` and `*.elc` files.
The directories in this list are also used for writing the
`*.eln` files produced by asynchronous native-compilation;
specifically, Emacs will write these files into the first writable
directory in the list. Thus, you can control where native-compilation
stores the results by changing the value of this variable.

Variable: **native-comp-jit-compilation** [¶](#index-native_002dcomp_002djit_002dcompilation)
:   This variable, if non-`nil`, enables asynchronous (a.k.a.
    *just-in-time*, or JIT) native compilation of the
    `*.elc` files loaded by Emacs for which the corresponding
    `*.eln` files do not already exist. This JIT compilation uses
    separate Emacs sub-processes running in batch mode, according to the
    value of `native-comp-async-jobs-number`. When the JIT
    compilation of a Lisp file finishes successfully, the resulting
    `.eln` file is loaded and its code replaces the definition of
    functions provided by the `.elc` file.

Setting the value of `native-comp-jit-compilation` to `nil`
disables JIT native compilation. However, even when JIT native
compilation is disabled, Emacs might still need to start asynchronous
native compilation subprocesses to produce *trampolines*. To
control this, use a separate variable, described below.

Variable: **native-comp-enable-subr-trampolines** [¶](#index-native_002dcomp_002denable_002dsubr_002dtrampolines)
:   This variable controls generation of trampolines. A trampoline is a
    small piece of native code required to allow calling Lisp primitives,
    which were advised or redefined, from Lisp code that was
    natively compiled with `native-comp-speed` set to 2 or greater.
    Emacs stores the generated trampolines on separate `*.eln` files.
    By default, this variable’s value is `t`, which enables the
    generation of trampoline files; setting it to `nil` disables the
    generation of trampolines. Note that if a trampoline needed for
    advising or redefining a primitive is not available and cannot be
    generated, calls to that primitive from native-compiled Lisp will
    ignore redefinitions and advices, and will behave as if the primitive
    was called directly from C. Therefore, we don’t recommend disabling
    the trampoline generation, unless you know that all the trampolines
    needed by your Lisp programs are already compiled and accessible to
    Emacs.

    The value of this variable can also be a string, in which case it
    specifies the name of a directory in which to store the generated
    trampoline `*.eln` files, overriding the directories in
    `native-comp-eln-load-path`. This is useful if you want the
    trampolines to be generated as needed, but don’t want to store them
    under the user’s `HOME` directory or in the other public
    directories where `*.eln` files are kept. However, unlike with
    directories in `native-comp-eln-load-path`, the trampolines will
    be stored in the directory given by the value of this variable, not in
    its version-specific subdirectory. If the name of this directory is
    not absolute, it is interpreted relative to
    `invocation-directory` (see [Operating System Environment](#System-Environment))

    If this variable is non-`nil`, and Emacs needs to produce a
    trampoline, but it cannot find any writable directory to store the
    trampoline, it will store it inside `temporary-file-directory`
    (see [Generating Unique File Names](#Unique-File-Names)).

    Trampolines produced when no writable directory is found to store
    them, or when this variable is a string, will only be available for
    the duration of the current Emacs session, because Emacs doesn’t look
    for trampolines in either of these places.

---

Next: [Reading and Printing Lisp Objects](#Read-and-Print), Previous: [Compilation of Lisp to Native Code](#Native-Compilation), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 19 Debugging Lisp Programs [¶](#Debugging-Lisp-Programs)

There are several ways to find and investigate problems in an Emacs
Lisp program.

* If a problem occurs when you run the program, you can use the built-in
  Emacs Lisp debugger (see [The Lisp Debugger](#Debugger)) to suspend the Lisp evaluator,
  and examine and/or alter its internal state.
* You can use Edebug, a source-level debugger for Emacs Lisp.
  See [Edebug](#Edebug).
* You can trace the execution of functions involved in the problem using
  the tracing facilities provided by the `trace.el` package. This
  package provides the functions `trace-function-foreground` and
  `trace-function-background` for tracing function calls, and
  `trace-values` for adding values of select variables to the
  trace. For the details, see the documentation of these facilities in
  `trace.el`.
* If a syntactic problem is preventing Lisp from even reading the
  program, you can locate it using Lisp editing commands.
* You can look at the error and warning messages produced by the byte
  compiler when it compiles the program. See [Compiler Errors](#Compiler-Errors).
* You can use the Testcover package to perform coverage testing on the
  program.
* You can use the ERT package to write regression tests for the program.
  See [the ERT manual](https://www.gnu.org/software/emacs/manual/html_mono/ert.html#Top) in ERT: Emacs Lisp Regression Testing.
* You can profile the program to get hints about how to make it more efficient.
  See [Profiling](#Profiling).

Other useful tools for debugging input and output problems are the
dribble file (see [Terminal Input](#Terminal-Input)) and the `open-termscript`
function (see [Terminal Output](#Terminal-Output)).

* [The Lisp Debugger](#Debugger)
* [Edebug](#Edebug)
* [Debugging Invalid Lisp Syntax](#Syntax-Errors)
* [Test Coverage](#Test-Coverage)
* [Profiling](#Profiling)

---

Next: [Edebug](#Edebug), Up: [Debugging Lisp Programs](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
