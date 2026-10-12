### 9.3 A Virtual Machine for Guile [¶](#A-Virtual-Machine-for-Guile-1)

Enough about data—how does Guile run code?

Code is a grammatical production of a language. Sometimes these
languages are implemented using interpreters: programs that run
along-side the program being interpreted, dynamically translating the
high-level code to low-level code. Sometimes these languages are
implemented using compilers: programs that translate high-level
programs to equivalent low-level code, and pass on that low-level code
to some other language implementation. Each of these languages can be
thought to be virtual machines: they offer programs an abstract machine
on which to run.

Guile implements a number of interpreters and compilers on different
language levels. For example, there is an interpreter for the Scheme
language that is itself implemented as a Scheme program compiled to a
bytecode for a low-level virtual machine shipped with Guile. That
virtual machine is implemented by both an interpreter—a C program that
interprets the bytecodes—and a compiler—a C program that dynamically
translates bytecode programs to native machine code[37](#FOOT37).

This section describes the language implemented by Guile’s bytecode
virtual machine, as well as some examples of translations of Scheme
programs to Guile’s VM.

* [Why a VM?](#Why-a-VM_003f)
* [VM Concepts](#VM-Concepts)
* [Stack Layout](#Stack-Layout)
* [Variables and the VM](#Variables-and-the-VM)
* [Compiled Procedures are VM Programs](#VM-Programs)
* [Object File Format](#Object-File-Format)
* [Instruction Set](#Instruction-Set)
* [Just-In-Time Native Code](#Just_002dIn_002dTime-Native-Code)

---

Next: [VM Concepts](#VM-Concepts), Up: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.1 Why a VM? [¶](#Why-a-VM_003f-1)

For a long time, Guile only had a Scheme interpreter, implemented in C.
Guile’s interpreter operated directly on the S-expression representation
of Scheme source code.

But while the interpreter was highly optimized and hand-tuned, it still
performed many needless computations during the course of evaluating a
Scheme expression. For example, application of a function to arguments
needlessly consed up the arguments in a list. Evaluation of an
expression like `(f x y)` always had to figure out whether f
was a procedure, or a special form like `if`, or something else.
The interpreter represented the lexical environment as a heap data
structure, so every evaluation caused allocation, which was of course
slow. Et cetera.

The solution to the slow-interpreter problem was to compile the
higher-level language, Scheme, into a lower-level language for which all
of the checks and dispatching have already been done—the code is
instead stripped to the bare minimum needed to “do the job”.

The question becomes then, what low-level language to choose? There are
many options. We could compile to native code directly, but that poses
portability problems for Guile, as it is a highly cross-platform
project.

So we want the performance gains that compilation provides, but we
also want to maintain the portability benefits of a single code path.
The obvious solution is to compile to a virtual machine that is
present on all Guile installations.

The easiest (and most fun) way to depend on a virtual machine is to
implement the virtual machine within Guile itself. Guile contains a
bytecode interpreter (written in C) and a Scheme to bytecode compiler
(written in Scheme). This way the virtual machine provides what Scheme
needs (tail calls, multiple values, `call/cc`) and can provide
optimized inline instructions for Guile as well (GC-managed allocations,
type checks, etc.).

Guile also includes a just-in-time (JIT) compiler to translate bytecode
to native code. Because Guile embeds a portable code generation library
(<https://gitlab.com/wingo/lightening>), we keep the benefits of
portability while also benefitting from fast native code. To avoid too
much time spent in the JIT compiler itself, Guile is tuned to only emit
machine code for bytecode that is called often.

The rest of this section describes that VM that Guile implements, and
the compiled procedures that run on it.

Before moving on, though, we should note that though we spoke of the
interpreter in the past tense, Guile still has an interpreter. The
difference is that before, it was Guile’s main Scheme implementation,
and so was implemented in highly optimized C; now, it is actually
implemented in Scheme, and compiled down to VM bytecode, just like any
other program. (There is still a C interpreter around, used to
bootstrap the compiler, but it is not normally used at runtime.)

The upside of implementing the interpreter in Scheme is that we preserve
tail calls and multiple-value handling between interpreted and compiled
code, and with advent of the JIT compiler in Guile 3.0 we reach the
speed of the old hand-tuned C implementation; it’s the best of both
worlds.

Also note that this decision to implement a bytecode compiler does not
preclude ahead-of-time native compilation. More possibilities are
discussed in [Extending the Compiler](#Extending-the-Compiler).

---

Next: [Stack Layout](#Stack-Layout), Previous: [Why a VM?](#Why-a-VM_003f), Up: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.2 VM Concepts [¶](#VM-Concepts-1)

The bytecode in a Scheme procedure is interpreted by a virtual machine
(VM). Each thread has its own instantiation of the VM. The virtual
machine executes the sequence of instructions in a procedure.

Each VM instruction starts by indicating which operation it is, and then
follows by encoding its source and destination operands. Each procedure
declares that it has some number of local variables, including the
function arguments. These local variables form the available operands
of the procedure, and are accessed by index.

The local variables for a procedure are stored on a stack. Calling a
procedure typically enlarges the stack, and returning from a procedure
shrinks it. Stack memory is exclusive to the virtual machine that owns
it.

In addition to their stacks, virtual machines also have access to the
global memory (modules, global bindings, etc) that is shared among other
parts of Guile, including other VMs.

The registers that a VM has are as follows:

* ip - Instruction pointer
* sp - Stack pointer
* fp - Frame pointer

In other architectures, the instruction pointer is sometimes called the
“program counter” (pc). This set of registers is pretty typical for
virtual machines; their exact meanings in the context of Guile’s VM are
described in the next section.

---

Next: [Variables and the VM](#Variables-and-the-VM), Previous: [VM Concepts](#VM-Concepts), Up: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.3 Stack Layout [¶](#Stack-Layout-1)

The stack of Guile’s virtual machine is composed of *frames*. Each
frame corresponds to the application of one compiled procedure, and
contains storage space for arguments, local variables, and some
bookkeeping information (such as what to do after the frame is
finished).

While the compiler is free to do whatever it wants to, as long as the
semantics of a computation are preserved, in practice every time you
call a function, a new frame is created. (The notable exception of
course is the tail call case, see [Tail calls](#Tail-Calls).)

The structure of the top stack frame is as follows:

```
   | ...previous frame locals...  |
   +==============================+ <- fp + 3
   | Dynamic link                 |
   +------------------------------+
   | Virtual return address (vRA) |
   +------------------------------+
   | Machine return address (mRA) |
   +==============================+ <- fp
   | Local 0                      |
   +------------------------------+
   | Local 1                      |
   +------------------------------+
   | ...                          |
   +------------------------------+
   | Local N-1                    |
   \------------------------------/ <- sp
```

In the above drawing, the stack grows downward. At the beginning of a
function call, the procedure being applied is in local 0, followed by
the arguments from local 1. After the procedure checks that it is being
passed a compatible set of arguments, the procedure allocates some
additional space in the frame to hold variables local to the function.

Note that once a value in a local variable slot is no longer needed,
Guile is free to re-use that slot. This applies to the slots that were
initially used for the callee and arguments, too. For this reason,
backtraces in Guile aren’t always able to show all of the arguments: it
could be that the slot corresponding to that argument was re-used by
some other variable.

The *virtual return address* is the `ip` that was in effect
before this program was applied. When we return from this activation
frame, we will jump back to this `ip`. Likewise, the *dynamic
link* is the offset of the `fp` that was in effect before this
program was applied, relative to the current `fp`.

There are two return addresses: the virtual return address (vRA), and
the machine return address (mRA). The vRA is always present and
indicates a bytecode address. The mRA is only present when a call is
made from a function with machine code (e.g. a function that has been
JIT-compiled).

To prepare for a non-tail application, Guile’s VM will emit code that
shuffles the function to apply and its arguments into appropriate stack
slots, with three free slots below them. The call then initializes
those free slots to hold the machine return address (or NULL), the
virtual return address, and the offset to the previous frame pointer
(`fp`). It then gets the `ip` for the function being called
and adjusts `fp` to point to the new call frame.

In this way, the dynamic link links the current frame to the previous
frame. Computing a stack trace involves traversing these frames.

Each stack local in Guile is 64 bits wide, even on 32-bit architectures.
This allows Guile to preserve its uniform treatment of stack locals
while allowing for unboxed arithmetic on 64-bit integers and
floating-point numbers. See [Instruction Set](#Instruction-Set), for more on unboxed
arithmetic.

As an implementation detail, we actually store the dynamic link as an
offset and not an absolute value because the stack can move at runtime
as it expands or during partial continuation calls. If it were an
absolute value, we would have to walk the frames, relocating frame
pointers.

---

Next: [Compiled Procedures are VM Programs](#VM-Programs), Previous: [Stack Layout](#Stack-Layout), Up: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.4 Variables and the VM [¶](#Variables-and-the-VM-1)

Consider the following Scheme code as an example:

```
  (define (foo a)
    (lambda (b) (vector foo a b)))
```

Within the lambda expression, `foo` is a top-level variable,
`a` is a lexically captured variable, and `b` is a local
variable.

Another way to refer to `a` and `b` is to say that `a` is
a “free” variable, since it is not defined within the lambda, and
`b` is a “bound” variable. These are the terms used in the
*lambda calculus*, a mathematical notation for describing functions.
The lambda calculus is useful because it is a language in which to
reason precisely about functions and variables. It is especially good
at describing scope relations, and it is for that reason that we mention
it here.

Guile allocates all variables on the stack. When a lexically enclosed
procedure with free variables—a *closure*—is created, it copies
those variables into its free variable vector. References to free
variables are then redirected through the free variable vector.

If a variable is ever `set!`, however, it will need to be
heap-allocated instead of stack-allocated, so that different closures
that capture the same variable can see the same value. Also, this
allows continuations to capture a reference to the variable, instead
of to its value at one point in time. For these reasons, `set!`
variables are allocated in “boxes”—actually, in variable cells.
See [Variables](#Variables), for more information. References to `set!`
variables are indirected through the boxes.

Thus perhaps counterintuitively, what would seem “closer to the
metal”, viz `set!`, actually forces an extra memory allocation and
indirection. Sometimes Guile’s optimizer can remove this allocation,
but not always.

Going back to our example, `b` may be allocated on the stack, as
it is never mutated.

`a` may also be allocated on the stack, as it too is never
mutated. Within the enclosed lambda, its value will be copied into
(and referenced from) the free variables vector.

`foo` is a top-level variable, because `foo` is not
lexically bound in this example.

---

Next: [Object File Format](#Object-File-Format), Previous: [Variables and the VM](#Variables-and-the-VM), Up: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.5 Compiled Procedures are VM Programs [¶](#Compiled-Procedures-are-VM-Programs)

By default, when you enter in expressions at Guile’s REPL, they are
first compiled to bytecode. Then that bytecode is executed to produce a
value. If the expression evaluates to a procedure, the result of this
process is a compiled procedure.

A compiled procedure is a compound object consisting of its bytecode and
a reference to any captured lexical variables. In addition, when a
procedure is compiled, it has associated metadata written to side
tables, for instance a line number mapping, or its docstring. You can
pick apart these pieces with the accessors in `(system vm
program)`. See [Compiled Procedures](#Compiled-Procedures), for a full API reference.

A procedure may reference data that was statically allocated when the
procedure was compiled. For example, a pair of immediate objects
(see [Immediate Objects](#Immediate-Objects)) can be allocated directly in the memory
segment that contains the compiled bytecode, and accessed directly by
the bytecode.

Another use for statically allocated data is to serve as a cache for a
bytecode. Top-level variable lookups are handled in this way; the first
time a top-level binding is referenced, the resolved variable will be
stored in a cache. Thereafter all access to the variable goes through
the cache cell. The variable’s value may change in the future, but the
variable itself will not.

We can see how these concepts tie together by disassembling the
`foo` function we defined earlier to see what is going on:

```
scheme@(guile-user)> (define (foo a) (lambda (b) (vector foo a b)))
scheme@(guile-user)> ,x foo
Disassembly of #<procedure foo (a)> at #xf1da30:

   0    (instrument-entry 164)                                at (unknown file):5:0
   2    (assert-nargs-ee/locals 2 1)    ;; 3 slots (1 arg)
   3    (allocate-words/immediate 2 3)                        at (unknown file):5:16
   4    (load-u64 0 0 65605)
   7    (word-set!/immediate 2 0 0)
   8    (load-label 0 7)                ;; anonymous procedure at #xf1da6c
  10    (word-set!/immediate 2 1 0)
  11    (scm-set!/immediate 2 2 1)
  12    (reset-frame 1)                 ;; 1 slot
  13    (handle-interrupts)
  14    (return-values)

----------------------------------------
Disassembly of anonymous procedure at #xf1da6c:

   0    (instrument-entry 183)                                at (unknown file):5:16
   2    (assert-nargs-ee/locals 2 3)    ;; 5 slots (1 arg)
   3    (static-ref 2 152)              ;; #<variable 112e530 value: #<procedure foo (a)>>
   5    (immediate-tag=? 2 7 0)         ;; heap-object?
   7    (je 19)                         ;; -> L2
   8    (static-ref 2 119)              ;; #<directory (guile-user) ca9750>
  10    (static-ref 1 127)              ;; foo
  12    (call-scm<-scm-scm 2 2 1 40)
  14    (immediate-tag=? 2 7 0)         ;; heap-object?
  16    (jne 8)                         ;; -> L1
  17    (scm-ref/immediate 0 2 1)
  18    (immediate-tag=? 0 4095 2308)   ;; undefined?
  20    (je 4)                          ;; -> L1
  21    (static-set! 2 134)             ;; #<variable 112e530 value: #<procedure foo (a)>>
  23    (j 3)                           ;; -> L2
L1:
  24    (throw/value 1 151)             ;; #(unbound-variable #f "Unbound variable: ~S")
L2:
  26    (scm-ref/immediate 2 2 1)
  27    (allocate-words/immediate 1 4)                        at (unknown file):5:28
  28    (load-u64 0 0 781)
  31    (word-set!/immediate 1 0 0)
  32    (scm-set!/immediate 1 1 2)
  33    (scm-ref/immediate 4 4 2)
  34    (scm-set!/immediate 1 2 4)
  35    (scm-set!/immediate 1 3 3)
  36    (mov 4 1)
  37    (reset-frame 1)                 ;; 1 slot
  38    (handle-interrupts)
  39    (return-values)
```

The first thing to notice is that the bytecode is at a fairly low level.
When a program is compiled from Scheme to bytecode, it is expressed in
terms of more primitive operations. As such, there can be more
instructions than you might expect.

The first chunk of instructions is the outer `foo` procedure. It
is followed by the code for the contained closure. The code can look
daunting at first glance, but with practice it quickly becomes
comprehensible, and indeed being able to read bytecode is an important
step to understanding the low-level performance of Guile programs.

The `foo` function begins with a prelude. The
`instrument-entry` bytecode increments a counter associated with
the function. If the counter reaches a certain threshold, Guile will
emit machine code (“JIT-compile”) for `foo`. Emitting machine
code is fairly cheap but it does take time, so it’s not something you
want to do for every function. Using a per-function counter and a
global threshold allows Guile to spend time JIT-compiling only the
“hot” functions.

Next in the prelude is an argument-checking instruction, which checks
that it was called with only 1 argument (plus the callee function itself
makes 2) and then reserves stack space for an additional 1 local.

Then from `ip` 3 to 11, we allocate a new closure by allocating a
three-word object, initializing its first word to store a type tag,
setting its second word to its code pointer, and finally at `ip`
11, storing local value 1 (the `a` argument) into the third word
(the first free variable).

Before returning, `foo` “resets the frame” to hold only one local
(the return value), runs any pending interrupts (see [Asynchronous Interrupts](#Asyncs)) and
then returns.

Note that local variables in Guile’s virtual machine are usually
addressed relative to the stack pointer, which leads to a pleasantly
efficient `sp[n]` access. However it can make the
disassembly hard to read, because the `sp` can change during the
function, and because incoming arguments are relative to the `fp`,
not the `sp`.

To know what `fp`-relative slot corresponds to an
`sp`-relative reference, scan up in the disassembly until you get
to a “n slots” annotation; in our case, 3, indicating that the
frame has space for 3 slots. Thus a zero-indexed `sp`-relative
slot of 2 corresponds to the `fp`-relative slot of 0, which
initially held the value of the closure being called. This means that
Guile doesn’t need the value of the closure to compute its result, and
so slot 0 was free for re-use, in this case for the result of making a
new closure.

A closure is code with data. As you can see, making the closure
involved making an object (`ip` 3), putting a code pointer in it
(`ip` 8 and 10), and putting in the closure’s free variable
(`ip` 11).

The second stanza disassembles the code for the closure. After the
prelude, all of the code between `ip` 5 and 24 is related to
loading the toplevel variable `foo` into slot 1. This lookup
happens only once, and is associated with a cache; after the first run,
the value in the cache will be a bound variable, and the code will jump
from `ip` 7 to 26. On the first run, Guile gets the module
associated with the function, calls out to a run-time routine to look up
the variable, and checks that the variable is bound before initializing
the cache. Either way, `ip` 26 dereferences the variable into
local 2.

What follows is the allocation and initialization of the vector return
value. `Ip` 27 does the allocation, and the following two
instructions initialize the type-and-length tag for the object’s first
word. `Ip` 32 sets word 1 of the object (the first vector slot) to
the value of `foo`; `ip` 33 fetches the closure variable for
`a`, then in `ip` 34 stores it in the second vector slot; and
finally, in `ip` 35, local `b` is stored to the third vector
slot. This is followed by the return sequence.

---

Next: [Instruction Set](#Instruction-Set), Previous: [Compiled Procedures are VM Programs](#VM-Programs), Up: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.6 Object File Format [¶](#Object-File-Format-1)

To compile a file to disk, we need a format in which to write the
compiled code to disk, and later load it into Guile. A good *object
file format* has a number of characteristics:

* Above all else, it should be very cheap to load a compiled file.
* It should be possible to statically allocate constants in the
  file. For example, a bytevector literal in source code can be emitted
  directly into the object file.
* The compiled file should enable maximum code and data sharing
  between different processes.
* The compiled file should contain debugging information, such as
  line numbers, but that information should be separated from the code
  itself. It should be possible to strip debugging information if space
  is tight.

These characteristics are not specific to Scheme. Indeed, mainstream
languages like C and C++ have solved this issue many times in the past.
Guile builds on their work by adopting ELF, the object file format of
GNU and other Unix-like systems, as its object file format. Although
Guile uses ELF on all platforms, we do not use platform support for ELF.
Guile implements its own linker and loader. The advantage of using ELF
is not sharing code, but sharing ideas. ELF is simply a well-designed
object file format.

An ELF file has two meta-tables describing its contents. The first
meta-table is for the loader, and is called the *program table* or
sometimes the *segment table*. The program table divides the file
into big chunks that should be treated differently by the loader.
Mostly the difference between these *segments* is their
permissions.

Typically all segments of an ELF file are marked as read-only, except
that part that represents modifiable static data or static data that
needs load-time initialization. Loading an ELF file is as simple as
mmapping the thing into memory with read-only permissions, then using
the segment table to mark a small sub-region of the file as writable.
This writable section is typically added to the root set of the garbage
collector as well.

One ELF segment is marked as “dynamic”, meaning that it has data of
interest to the loader. Guile uses this segment to record the Guile
version corresponding to this file. There is also an entry in the
dynamic segment that points to the address of an initialization thunk
that is run to perform any needed link-time initialization. (This is
like dynamic relocations for normal ELF shared objects, except that we
compile the relocations as a procedure instead of having the loader
interpret a table of relocations.) Finally, the dynamic segment marks
the location of the “entry thunk” of the object file. This thunk is
returned to the caller of `load-thunk-from-memory` or
`load-thunk-from-file`. When called, it will execute the “body”
of the compiled expression.

The other meta-table in an ELF file is the *section table*. Whereas
the program table divides an ELF file into big chunks for the loader,
the section table specifies small sections for use by introspective
tools like debuggers or the like. One segment (program table entry)
typically contains many sections. There may be sections outside of any
segment, as well.

Typical sections in a Guile `.go` file include:

`.rtl-text`
:   Bytecode.

`.data`
:   Data that needs initialization, or which may be modified at runtime.

`.rodata`
:   Statically allocated data that needs no run-time initialization, and
    which therefore can be shared between processes.

`.dynamic`
:   The dynamic section, discussed above.

`.symtab`

`.strtab`
:   A table mapping addresses in the `.rtl-text` to procedure names.
    `.strtab` is used by `.symtab`.

`.guile.procprops`

`.guile.arities`

`.guile.arities.strtab`

`.guile.docstrs`

`.guile.docstrs.strtab`
:   Side tables of procedure properties, arities, and docstrings.

`.guile.docstrs.strtab`
:   Side table of frame maps, describing the set of live slots for ever
    return point in the program text, and whether those slots are pointers
    are not. Used by the garbage collector.

`.debug_info`

`.debug_abbrev`

`.debug_str`

`.debug_loc`

`.debug_line`
:   Debugging information, in DWARF format. See the DWARF specification,
    for more information.

`.shstrtab`
:   Section name string table.

For more information, see [the
elf(5) man page](http://linux.die.net/man/5/elf). See [the DWARF
specification](http://dwarfstd.org/) for more on the DWARF debugging format. Or if you are an
adventurous explorer, try running `readelf` or `objdump` on
compiled `.go` files. It’s good times!

---

Next: [Just-In-Time Native Code](#Just_002dIn_002dTime-Native-Code), Previous: [Object File Format](#Object-File-Format), Up: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7 Instruction Set [¶](#Instruction-Set-1)

There are currently about 150 instructions in Guile’s virtual machine.
These instructions represent atomic units of a program’s execution.
Ideally, they perform one task without conditional branches, then
dispatch to the next instruction in the stream.

Instructions themselves are composed of 1 or more 32-bit units. The low
8 bits of the first word indicate the opcode, and the rest of
instruction describe the operands. There are a number of different ways
operands can be encoded.

`sn`
:   An unsigned n-bit integer, indicating the `sp`-relative index
    of a local variable.

`fn`
:   An unsigned n-bit integer, indicating the `fp`-relative index
    of a local variable. Used when a continuation accepts a variable number
    of values, to shuffle received values into known locations in the
    frame.

`cn`
:   An unsigned n-bit integer, indicating a constant value.

`l24`
:   An offset from the current `ip`, in 32-bit units, as a signed
    24-bit value. Indicates a bytecode address, for a relative jump.

`zi16`

`i16`

`i32`
:   An immediate Scheme value (see [Immediate Objects](#Immediate-Objects)), encoded directly
    in 16 or 32 bits. `zi16` is sign-extended; the others are
    zero-extended.

`a32`

`b32`
:   An immediate Scheme value, encoded as a pair of 32-bit words.
    `a32` and `b32` values always go together on the same opcode,
    and indicate the high and low bits, respectively. Normally only used on
    64-bit systems.

`n32`
:   A statically allocated non-immediate. The address of the non-immediate
    is encoded as a signed 32-bit integer, and indicates a relative offset
    in 32-bit units. Think of it as `SCM x = ip + offset`.

`r32`
:   Indirect scheme value, like `n32` but indirected. Think of it as
    `SCM *x = ip + offset`.

`l32`

`lo32`
:   An ip-relative address, as a signed 32-bit integer. Could indicate a
    bytecode address, as in `make-closure`, or a non-immediate address,
    as with `static-patch!`.

    `l32` and `lo32` are the same from the perspective of the
    virtual machine. The difference is that an assembler might want to
    allow an `lo32` address to be specified as a label and then some
    number of words offset from that label, for example when patching a
    field of a statically allocated object.

`v32:x8-l24`
:   Almost all VM instructions have a fixed size. The `jtable`
    instruction used to perform optimized `case` branches is an
    exception, which uses a `v32` trailing word to indicate the number
    of additional words in the instruction, which themselves are encoded as
    `x8-l24` values.

`b1`
:   A boolean value: 1 for true, otherwise 0.

`xn`
:   An ignored sequence of n bits.

An instruction is specified by giving its name, then describing its
operands. The operands are packed by 32-bit words, with earlier
operands occupying the lower bits.

For example, consider the following instruction specification:

Instruction: **call** `f24:proc x8:_ c24:nlocals` [¶](#index-call)

The first word in the instruction will start with the 8-bit value
corresponding to the call opcode in the low bits, followed by
proc as a 24-bit value. The second word starts with 8 dead bits,
followed by the index as a 24-bit immediate value.

For instructions with operands that encode references to the stack, the
interpretation of those stack values is up to the instruction itself.
Most instructions expect their operands to be tagged SCM values
(`scm` representation), but some instructions expect unboxed
integers (`u64` and `s64` representations) or floating-point
numbers (`f64` representation). It is assumed that the bits for a
`u64` value are the same as those for an `s64` value, and that
`s64` values are stored in two’s complement.

Instructions have static types: they must receive their operands in the
format they expect. It’s up to the compiler to ensure this is the case.

Unless otherwise mentioned, all operands and results are in the
`scm` representation.

* [Call and Return Instructions](#Call-and-Return-Instructions)
* [Function Prologue Instructions](#Function-Prologue-Instructions)
* [Shuffling Instructions](#Shuffling-Instructions)
* [Trampoline Instructions](#Trampoline-Instructions)
* [Non-Local Control Flow Instructions](#Non_002dLocal-Control-Flow-Instructions)
* [Instrumentation Instructions](#Instrumentation-Instructions)
* [Intrinsic Call Instructions](#Intrinsic-Call-Instructions)
* [Constant Instructions](#Constant-Instructions)
* [Memory Access Instructions](#Memory-Access-Instructions)
* [Atomic Memory Access Instructions](#Atomic-Memory-Access-Instructions)
* [Tagging and Untagging Instructions](#Tagging-and-Untagging-Instructions)
* [Integer Arithmetic Instructions](#Integer-Arithmetic-Instructions)
* [Floating-Point Arithmetic Instructions](#Floating_002dPoint-Arithmetic-Instructions)
* [Comparison Instructions](#Comparison-Instructions)
* [Branch Instructions](#Branch-Instructions)
* [Raw Memory Access Instructions](#Raw-Memory-Access-Instructions)

---

Next: [Function Prologue Instructions](#Function-Prologue-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.1 Call and Return Instructions [¶](#Call-and-Return-Instructions-1)

As described earlier (see [Stack Layout](#Stack-Layout)), Guile’s calling convention
is that arguments are passed and values returned on the stack.

For calls, both in tail position and in non-tail position, we require
that the procedure and the arguments already be shuffled into place
before the call instruction. “Into place” for a tail call means that
the procedure should be in slot 0, relative to the `fp`, and the
arguments should follow. For a non-tail call, if the procedure is in
`fp`-relative slot n, the arguments should follow from slot
n+1, and there should be three free slots between n-1 and
n-3 in which to save the mRA, vRA, and `fp`.

Returning values is similar. Multiple-value returns should have values
already shuffled down to start from `fp`-relative slot 0 before
emitting `return-values`.

In both calls and returns, the `sp` is used to indicate to the
callee or caller the number of arguments or return values, respectively.
After receiving return values, it is the caller’s responsibility to
*restore the frame* by resetting the `sp` to its former value.

Instruction: **call** `f24:proc x8:_ c24:nlocals` [¶](#index-call-1)
:   Call a procedure. proc is the local corresponding to a procedure.
    The three values below proc will be overwritten by the saved call
    frame data. The new frame will have space for nlocals locals: one
    for the procedure, and the rest for the arguments which should already
    have been pushed on.

    When the call returns, execution proceeds with the next instruction.
    There may be any number of values on the return stack; the precise
    number can be had by subtracting the address of proc-1 from the
    post-call `sp`.

Instruction: **call-label** `f24:proc x8:_ c24:nlocals l32:label` [¶](#index-call_002dlabel)
:   Call a procedure in the same compilation unit.

    This instruction is just like `call`, except that instead of
    dereferencing proc to find the call target, the call target is
    known to be at label, a signed 32-bit offset in 32-bit units from
    the current `ip`. Since proc is not dereferenced, it may be
    some other representation of the closure.

Instruction: **tail-call** `x24:_` [¶](#index-tail_002dcall)
:   Tail-call a procedure. Requires that the procedure and all of the
    arguments have already been shuffled into position, and that the frame
    has already been reset to the number of arguments to the call.

Instruction: **tail-call-label** `x24:_ l32:label` [¶](#index-tail_002dcall_002dlabel)
:   Tail-call a known procedure. As `call` is to `call-label`,
    `tail-call` is to `tail-call-label`.

Instruction: **return-values** `x24:_` [¶](#index-return_002dvalues)
:   Return a number of values from a call frame. The return values should
    have already been shuffled down to a contiguous array starting at slot
    0, and the frame already reset.

Instruction: **receive** `f12:dst f12:proc x8:_ c24:nlocals` [¶](#index-receive-2)
:   Receive a single return value from a call whose procedure was in
    proc, asserting that the call actually returned at least one
    value. Afterwards, resets the frame to nlocals locals.

Instruction: **receive-values** `f24:proc b1:allow-extra? x7:_ c24:nvalues` [¶](#index-receive_002dvalues)
:   Receive a return of multiple values from a call whose procedure was in
    proc. If fewer than nvalues values were returned, signal an
    error. Unless allow-extra? is true, require that the number of
    return values equals nvalues exactly. After `receive-values`
    has run, the values can be copied down via `mov`, or used in place.

---

Next: [Shuffling Instructions](#Shuffling-Instructions), Previous: [Call and Return Instructions](#Call-and-Return-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.2 Function Prologue Instructions [¶](#Function-Prologue-Instructions-1)

A function call in Guile is very cheap: the VM simply hands control to
the procedure. The procedure itself is responsible for asserting that it
has been passed an appropriate number of arguments. This strategy allows
arbitrarily complex argument parsing idioms to be developed, without
harming the common case.

For example, only calls to keyword-argument procedures “pay” for the
cost of parsing keyword arguments. (At the time of this writing, calling
procedures with keyword arguments is typically two to four times as
costly as calling procedures with a fixed set of arguments.)

Instruction: **assert-nargs-ee** `c24:expected` [¶](#index-assert_002dnargs_002dee)

Instruction: **assert-nargs-ge** `c24:expected` [¶](#index-assert_002dnargs_002dge)

Instruction: **assert-nargs-le** `c24:expected` [¶](#index-assert_002dnargs_002dle)
:   If the number of actual arguments is not `==`, `>=`, or
    `<=` expected, respectively, signal an error.

    The number of arguments is determined by subtracting the stack pointer
    from the frame pointer (`fp - sp`). See [Stack Layout](#Stack-Layout), for more
    details on stack frames. Note that expected includes the
    procedure itself.

Instruction: **arguments<=?** `c24:expected` [¶](#index-arguments_003c_003d_003f)
:   Set the `LESS_THAN`, `EQUAL`, or `NONE` comparison result
    values if the number of arguments is respectively less than, equal to,
    or greater than expected.

Instruction: **positional-arguments<=?** `c24:nreq x8:_ c24:expected` [¶](#index-positional_002darguments_003c_003d_003f)
:   Set the `LESS_THAN`, `EQUAL`, or `NONE` comparison result
    values if the number of positional arguments is respectively less than,
    equal to, or greater than expected. The first nreq
    arguments are positional arguments, as are the subsequent arguments that
    are not keywords.

The `arguments<=?` and `positional-arguments<=?` instructions
are used to implement multiple arities, as in `case-lambda`.
See [Case-lambda](#Case_002dlambda), for more information. See [Branch Instructions](#Branch-Instructions),
for more on comparison results.

Instruction: **bind-kwargs** `c24:nreq c8:flags c24:nreq-and-opt x8:_ c24:ntotal n32:kw-offset` [¶](#index-bind_002dkwargs)
:   flags is a bitfield, whose lowest bit is allow-other-keys,
    second bit is has-rest, and whose following six bits are unused.

    Find the last positional argument, and shuffle all the rest above
    ntotal. Initialize the intervening locals to
    `SCM_UNDEFINED`. Then load the constant at kw-offset words
    from the current ip, and use it and the allow-other-keys
    flag to bind keyword arguments. If has-rest, collect all shuffled
    arguments into a list, and store it in nreq-and-opt. Finally,
    clear the arguments that we shuffled up.

    The parsing is driven by a keyword arguments association list, looked up
    using kw-offset. The alist is a list of pairs of the form
    `(kw . index)`, mapping keyword arguments to their
    local slot indices. Unless `allow-other-keys` is set, the parser
    will signal an error if an unknown key is found.

    A macro-mega-instruction.

Instruction: **bind-optionals** `f24:nlocals` [¶](#index-bind_002doptionals)
:   Expand the current frame to have at least nlocals locals, filling
    in any fresh values with `SCM_UNDEFINED`. If the frame has more
    than nlocals locals, it is left as it is.

Instruction: **bind-rest** `f24:dst` [¶](#index-bind_002drest)
:   Collect any arguments at or above dst into a list, and store that
    list at dst.

Instruction: **alloc-frame** `c24:nlocals` [¶](#index-alloc_002dframe)
:   Ensure that there is space on the stack for nlocals local
    variables. The value of any new local is undefined.

Instruction: **reset-frame** `c24:nlocals` [¶](#index-reset_002dframe)
:   Like `alloc-frame`, but doesn’t check that the stack is big enough,
    and doesn’t initialize values to `SCM_UNDEFINED`. Used to reset
    the frame size to something less than the size that was previously set
    via alloc-frame.

Instruction: **assert-nargs-ee/locals** `c12:expected c12:nlocals` [¶](#index-assert_002dnargs_002dee_002flocals)
:   Equivalent to a sequence of `assert-nargs-ee` and
    `alloc-frame`. The number of locals reserved is expected +
    nlocals.

---

Next: [Trampoline Instructions](#Trampoline-Instructions), Previous: [Function Prologue Instructions](#Function-Prologue-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.3 Shuffling Instructions [¶](#Shuffling-Instructions-1)

These instructions are used to move around values on the stack.

Instruction: **mov** `s12:dst s12:src` [¶](#index-mov)

Instruction: **long-mov** `s24:dst x8:_ s24:src` [¶](#index-long_002dmov)
:   Copy a value from one local slot to another.

    As discussed previously, procedure arguments and local variables are
    allocated to local slots. Guile’s compiler tries to avoid shuffling
    variables around to different slots, which often makes `mov`
    instructions redundant. However there are some cases in which shuffling
    is necessary, and in those cases, `mov` is the thing to use.

Instruction: **long-fmov** `f24:dst x8:_ f24:src` [¶](#index-long_002dfmov)
:   Copy a value from one local slot to another, but addressing slots
    relative to the `fp` instead of the `sp`. This is used when
    shuffling values into place after multiple-value returns.

Instruction: **push** `s24:src` [¶](#index-push)
:   Bump the stack pointer by one word, and fill it with the value from slot
    src. The offset to src is calculated before the stack
    pointer is adjusted.

The `push` instruction is used when another instruction is unable
to address an operand because the operand is encoded with fewer than 24
bits. In that case, Guile’s assembler will transparently emit code that
temporarily pushes any needed operands onto the stack, emits the
original instruction to address those now-near variables, then shuffles
the result (if any) back into place.

Instruction: **pop** `s24:dst` [¶](#index-pop)
:   Pop the stack pointer, storing the value that was there in slot
    dst. The offset to dst is calculated after the stack
    pointer is adjusted.

Instruction: **drop** `c24:count` [¶](#index-drop-1)
:   Pop the stack pointer by count words, discarding any values that
    were stored there.

Instruction: **shuffle-down** `f12:from f12:to` [¶](#index-shuffle_002ddown)
:   Shuffle down values from from to to, reducing the frame size
    by FROM-TO slots. Part of the internal implementation of
    `call-with-values`, `values`, and `apply`.

Instruction: **expand-apply-argument** `x24:_` [¶](#index-expand_002dapply_002dargument)
:   Take the last local in a frame and expand it out onto the stack, as for
    the last argument to `apply`.

---

Next: [Non-Local Control Flow Instructions](#Non_002dLocal-Control-Flow-Instructions), Previous: [Shuffling Instructions](#Shuffling-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.4 Trampoline Instructions [¶](#Trampoline-Instructions-1)

Though most applicable objects in Guile are procedures implemented in
bytecode, not all are. There are primitives, continuations, and other
procedure-like objects that have their own calling convention. Instead
of adding special cases to the `call` instruction, Guile wraps
these other applicable objects in VM trampoline procedures, then
provides special support for these objects in bytecode.

Trampoline procedures are typically generated by Guile at runtime, for
example in response to a call to `scm_c_make_gsubr`. As such, a
compiler probably shouldn’t emit code with these instructions. However,
it’s still interesting to know how these things work, so we document
these trampoline instructions here.

Instruction: **subr-call** `c24:idx` [¶](#index-subr_002dcall)
:   Call a subr, passing all locals in this frame as arguments, and storing
    the results on the stack, ready to be returned.

Instruction: **foreign-call** `c12:cif-idx c12:ptr-idx` [¶](#index-foreign_002dcall)
:   Call a foreign function. Fetch the cif and foreign pointer from
    cif-idx and ptr-idx closure slots of the callee. Arguments
    are taken from the stack, and results placed on the stack, ready to be
    returned.

Instruction: **builtin-ref** `s12:dst c12:idx` [¶](#index-builtin_002dref)
:   Load a builtin stub by index into dst.

---

Next: [Instrumentation Instructions](#Instrumentation-Instructions), Previous: [Trampoline Instructions](#Trampoline-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.5 Non-Local Control Flow Instructions [¶](#Non_002dLocal-Control-Flow-Instructions-1)

Instruction: **capture-continuation** `s24:dst` [¶](#index-capture_002dcontinuation)
:   Capture the current continuation, and write it to dst. Part of
    the implementation of `call/cc`.

Instruction: **continuation-call** `c24:contregs` [¶](#index-continuation_002dcall)
:   Return to a continuation, nonlocally. The arguments to the continuation
    are taken from the stack. contregs is a free variable containing
    the reified continuation.

Instruction: **abort** `x24:_` [¶](#index-abort-1)
:   Abort to a prompt handler. The tag is expected in slot 1, and the rest
    of the values in the frame are returned to the prompt handler. This
    corresponds to a tail application of `abort-to-prompt`.

    If no prompt can be found in the dynamic environment with the given tag,
    an error is signaled. Otherwise all arguments are passed to the
    prompt’s handler, along with the captured continuation, if necessary.

    If the prompt’s handler can be proven to not reference the captured
    continuation, no continuation is allocated. This decision happens
    dynamically, at run-time; the general case is that the continuation may
    be captured, and thus resumed. A reinstated continuation will have its
    arguments pushed on the stack from slot 0, as if from a multiple-value
    return, and control resumes in the caller. Thus to the calling
    function, a call to `abort-to-prompt` looks like any other function
    call.

Instruction: **compose-continuation** `c24:cont` [¶](#index-compose_002dcontinuation)
:   Compose a partial continuation with the current continuation. The
    arguments to the continuation are taken from the stack. cont is a
    free variable containing the reified continuation.

Instruction: **prompt** `s24:tag b1:escape-only? x7:_ f24:proc-slot x8:_ l24:handler-offset` [¶](#index-prompt)
:   Push a new prompt on the dynamic stack, with a tag from tag and a
    handler at handler-offset words from the current ip.

    If an abort is made to this prompt, control will jump to the handler.
    The handler will expect a multiple-value return as if from a call with
    the procedure at proc-slot, with the reified partial continuation
    as the first argument, followed by the values returned to the handler.
    If control returns to the handler, the prompt is already popped off by
    the abort mechanism. (Guile’s `prompt` implements Felleisen’s
    *–F–* operator.)

    If escape-only? is nonzero, the prompt will be marked as
    escape-only, which allows an abort to this prompt to avoid reifying the
    continuation.

    See [Prompts](#Prompts), for more information on prompts.

Instruction: **throw** `s12:key s12:args` [¶](#index-throw-1)
:   Raise an error by throwing to key and args. args
    should be a list.

Instruction: **throw/value** `s24:value n32:key-subr-and-message` [¶](#index-throw_002fvalue)

Instruction: **throw/value+data** `s24:value n32:key-subr-and-message` [¶](#index-throw_002fvalue_002bdata)
:   Raise an error, indicating val as the bad value.
    key-subr-and-message should be a vector, where the first element
    is the symbol to which to throw, the second is the procedure in which to
    signal the error (a string) or `#f`, and the third is a format
    string for the message, with one template. These instructions do not
    fall through.

    Both of these instructions throw to a key with four arguments: the
    procedure that indicates the error (or `#f`, the format string, a
    list with value, and either `#f` or the list with value
    as the last argument respectively.

Instruction: **unreachable** `x24:_` [¶](#index-unreachable)
:   Abort the process. This instruction should never be reached and must
    not continue. You would think this is useless but that’s not the case:
    it is inserted after a primcall to `raise-exception`, and allows
    compilers to know that this branch of control flow does not rejoin the
    graph.

---

Next: [Intrinsic Call Instructions](#Intrinsic-Call-Instructions), Previous: [Non-Local Control Flow Instructions](#Non_002dLocal-Control-Flow-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.6 Instrumentation Instructions [¶](#Instrumentation-Instructions-1)

Instruction: **instrument-entry** `x24:_ n32:data` [¶](#index-instrument_002dentry)

Instruction: **instrument-loop** `x24:_ n32:data` [¶](#index-instrument_002dloop)
:   Increase execution counter for this function and potentially tier up to
    the next JIT level. data is an offset to a structure recording
    execution counts and the next-level JIT code corresponding to this
    function. The increment values are currently 30 for
    `instrument-entry` and 2 for `instrument-loop`.

    `instrument-entry` will also run the apply hook, if VM hooks are
    enabled.

Instruction: **handle-interrupts** `x24:_` [¶](#index-handle_002dinterrupts)
:   Handle pending asynchronous interrupts (asyncs). See [Asynchronous Interrupts](#Asyncs). The
    compiler inserts `handle-interrupts` instructions before any call,
    return, or loop back-edge.

Instruction: **return-from-interrupt** `x24:_` [¶](#index-return_002dfrom_002dinterrupt)
:   A special instruction to return from a call and also pop off the stack
    frame from the call. Used when returning from asynchronous interrupts.

---

Next: [Constant Instructions](#Constant-Instructions), Previous: [Instrumentation Instructions](#Instrumentation-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.7 Intrinsic Call Instructions [¶](#Intrinsic-Call-Instructions-1)

Guile’s instruction set is low-level. This is good because the separate
components of, say, a `vector-ref` operation might be able to be
optimized out, leaving only the operations that need to be performed at
run-time.

However some macro-operations may need to perform large amounts of
computation at run-time to handle all the edge cases, and whose
micro-operation components aren’t amenable to optimization.
Residualizing code for the entire macro-operation would lead to code
bloat with no benefit.

In this kind of a case, Guile’s VM calls out to *intrinsics*:
run-time routines written in the host language (currently C, possibly
more in the future if Guile gains more run-time targets like
WebAssembly). There is one instruction for each instrinsic prototype;
the intrinsic is specified by index in the instruction.

Instruction: **call-thread** `x24:_ c32:idx` [¶](#index-call_002dthread)
:   Call the `void`-returning instrinsic with index idx, passing
    the current `scm_thread*` as the argument.

Instruction: **call-thread-scm** `s24:a c32:idx` [¶](#index-call_002dthread_002dscm)
:   Call the `void`-returning instrinsic with index idx, passing
    the current `scm_thread*` and the `scm` local a as
    arguments.

Instruction: **call-thread-scm-scm** `s12:a s12:b c32:idx` [¶](#index-call_002dthread_002dscm_002dscm)
:   Call the `void`-returning instrinsic with index idx, passing
    the current `scm_thread*` and the `scm` locals a and
    b as arguments.

Instruction: **call-scm-sz-u32** `s12:a s12:b c32:idx` [¶](#index-call_002dscm_002dsz_002du32)
:   Call the `void`-returning instrinsic with index idx, passing
    the locals a, b, and c as arguments. a is a
    `scm` value, while b and c are raw `u64` values
    which fit into `size_t` and `uint32_t` types, respectively.

Instruction: **call-scm<-thread** `s24:dst c32:idx` [¶](#index-call_002dscm_003c_002dthread)
:   Call the `SCM`-returning instrinsic with index idx, passing
    the current `scm_thread*` as the argument. Place the result in
    dst.

Instruction: **call-scm<-u64** `s12:dst s12:a c32:idx` [¶](#index-call_002dscm_003c_002du64)
:   Call the `SCM`-returning instrinsic with index idx, passing
    `u64` local a as the argument. Place the result in
    dst.

Instruction: **call-scm<-s64** `s12:dst s12:a c32:idx` [¶](#index-call_002dscm_003c_002ds64)
:   Call the `SCM`-returning instrinsic with index idx, passing
    `s64` local a as the argument. Place the result in
    dst.

Instruction: **call-scm<-scm** `s12:dst s12:a c32:idx` [¶](#index-call_002dscm_003c_002dscm)
:   Call the `SCM`-returning instrinsic with index idx, passing
    `scm` local a as the argument. Place the result in
    dst.

Instruction: **call-u64<-scm** `s12:dst s12:a c32:idx` [¶](#index-call_002du64_003c_002dscm)
:   Call the `uint64_t`-returning instrinsic with index idx,
    passing `scm` local a as the argument. Place the `u64`
    result in dst.

Instruction: **call-s64<-scm** `s12:dst s12:a c32:idx` [¶](#index-call_002ds64_003c_002dscm)
:   Call the `int64_t`-returning instrinsic with index idx,
    passing `scm` local a as the argument. Place the `s64`
    result in dst.

Instruction: **call-f64<-scm** `s12:dst s12:a c32:idx` [¶](#index-call_002df64_003c_002dscm)
:   Call the `double`-returning instrinsic with index idx,
    passing `scm` local a as the argument. Place the `f64`
    result in dst.

Instruction: **call-scm<-scm-scm** `s8:dst s8:a s8:b c32:idx` [¶](#index-call_002dscm_003c_002dscm_002dscm)
:   Call the `SCM`-returning instrinsic with index idx, passing
    `scm` locals a and b as arguments. Place the
    `scm` result in dst.

Instruction: **call-scm<-scm-uimm** `s8:dst s8:a c8:b c32:idx` [¶](#index-call_002dscm_003c_002dscm_002duimm)
:   Call the `SCM`-returning instrinsic with index idx, passing
    `scm` local a and `uint8_t` immediate b as
    arguments. Place the `scm` result in dst.

Instruction: **call-scm<-thread-scm** `s12:dst s12:a c32:idx` [¶](#index-call_002dscm_003c_002dthread_002dscm)
:   Call the `SCM`-returning instrinsic with index idx, passing
    the current `scm_thread*` and `scm` local a as
    arguments. Place the `scm` result in dst.

Instruction: **call-scm<-scm-u64** `s8:dst s8:a s8:b c32:idx` [¶](#index-call_002dscm_003c_002dscm_002du64)
:   Call the `SCM`-returning instrinsic with index idx, passing
    `scm` local a and `u64` local b as arguments.
    Place the `scm` result in dst.

Instruction: **call-scm-scm** `s12:a s12:b c32:idx` [¶](#index-call_002dscm_002dscm)
:   Call the `void`-returning instrinsic with index idx, passing
    `scm` locals a and b as arguments.

Instruction: **call-scm-scm-scm** `s8:a s8:b s8:c c32:idx` [¶](#index-call_002dscm_002dscm_002dscm)
:   Call the `void`-returning instrinsic with index idx, passing
    `scm` locals a, b, and c as arguments.

Instruction: **call-scm-uimm-scm** `s8:a c8:b s8:c c32:idx` [¶](#index-call_002dscm_002duimm_002dscm)
:   Call the `void`-returning instrinsic with index idx, passing
    `scm` local a, `uint8_t` immediate b, and
    `scm` local c as arguments.

There are corresponding macro-instructions for specific intrinsics.
These are equivalent to `call-instrinsic-kind` instructions
with the appropriate intrinsic idx arguments.

Macro Instruction: **add** dst a b [¶](#index-add)

Macro Instruction: **add/immediate** dst a b/imm [¶](#index-add_002fimmediate)
:   Add `SCM` values a and b and place the result in
    dst.

Macro Instruction: **sub** dst a b [¶](#index-sub)

Macro Instruction: **sub/immediate** dst a b/imm [¶](#index-sub_002fimmediate)
:   Subtract `SCM` value b from a and place the result in
    dst.

Macro Instruction: **mul** dst a b [¶](#index-mul)
:   Multiply `SCM` values a and b and place the result in
    dst.

Macro Instruction: **div** dst a b [¶](#index-div-1)
:   Divide `SCM` value a by b and place the result in
    dst.

Macro Instruction: **quo** dst a b [¶](#index-quo)
:   Compute the quotient of `SCM` values a and b and place
    the result in dst.

Macro Instruction: **rem** dst a b [¶](#index-rem)
:   Compute the remainder of `SCM` values a and b and place
    the result in dst.

Macro Instruction: **mod** dst a b [¶](#index-mod-1)
:   Compute the modulo of `SCM` value a by b and place the
    result in dst.

Macro Instruction: **logand** dst a b [¶](#index-logand-1)
:   Compute the bitwise `and` of `SCM` values a and b
    and place the result in dst.

Macro Instruction: **logior** dst a b [¶](#index-logior-1)
:   Compute the bitwise inclusive `or` of `SCM` values a and
    b and place the result in dst.

Macro Instruction: **logxor** dst a b [¶](#index-logxor-1)
:   Compute the bitwise exclusive `or` of `SCM` values a and
    b and place the result in dst.

Macro Instruction: **logsub** dst a b [¶](#index-logsub)
:   Compute the bitwise `and` of `SCM` value a and the
    bitwise `not` of b and place the result in dst.

Macro Instruction: **lsh** dst a b [¶](#index-lsh)

Macro Instruction: **lsh/immediate** a b/imm [¶](#index-lsh_002fimmediate)
:   Shift `SCM` value a left by `u64` value b bits and
    place the result in dst.

Macro Instruction: **rsh** dst a b [¶](#index-rsh)

Macro Instruction: **rsh/immediate** dst a b/imm [¶](#index-rsh_002fimmediate)
:   Shifts `SCM` value a right by `u64` value b bits
    and place the result in dst.

Macro Instruction: **scm->f64** dst src [¶](#index-scm_002d_003ef64)
:   Convert src to an unboxed `f64` and place the result in
    dst, or raises an error if src is not a real number.

Macro Instruction: **scm->u64** dst src [¶](#index-scm_002d_003eu64)
:   Convert src to an unboxed `u64` and place the result in
    dst, or raises an error if src is not an integer within
    range.

Macro Instruction: **scm->u64/truncate** dst src [¶](#index-scm_002d_003eu64_002ftruncate)
:   Convert src to an unboxed `u64` and place the result in
    dst, truncating to the low 64 bits, or raises an error if
    src is not an integer.

Macro Instruction: **scm->s64** dst src [¶](#index-scm_002d_003es64)
:   Convert src to an unboxed `s64` and place the result in
    dst, or raises an error if src is not an integer within
    range.

Macro Instruction: **u64->scm** dst src [¶](#index-u64_002d_003escm)
:   Convert u64 value src to a Scheme integer in dst.

Macro Instruction: **s64->scm** scm<-s64 [¶](#index-s64_002d_003escm)
:   Convert s64 value src to a Scheme integer in dst.

Macro Instruction: **string-set!** str idx ch [¶](#index-string_002dset_0021-1)
:   Sets the character idx (a `u64`) of string str to
    ch (a `u64` that is a valid character value).

Macro Instruction: **string->number** dst src [¶](#index-string_002d_003enumber-2)
:   Call `string->number` on src and place the result in
    dst.

Macro Instruction: **string->symbol** dst src [¶](#index-string_002d_003esymbol-2)
:   Call `string->symbol` on src and place the result in
    dst.

Macro Instruction: **symbol->keyword** dst src [¶](#index-symbol_002d_003ekeyword-1)
:   Call `symbol->keyword` on src and place the result in
    dst.

Macro Instruction: **class-of** dst src [¶](#index-class_002dof-1)
:   Set dst to the GOOPS class of `src`.

Macro Instruction: **wind** winder unwinder [¶](#index-wind)
:   Push wind and unwind procedures onto the dynamic stack. Note that
    neither are actually called; the compiler should emit calls to
    winder and unwinder for the normal dynamic-wind control
    flow. Also note that the compiler should have inserted checks that
    winder and unwinder are thunks, if it could not prove that
    to be the case. See [Dynamic Wind](#Dynamic-Wind).

Macro Instruction: **unwind** [¶](#index-unwind)
:   Exit from the dynamic extent of an expression, popping the top entry off
    of the dynamic stack.

Macro Instruction: **push-fluid** fluid value [¶](#index-push_002dfluid)
:   Dynamically bind value to fluid by creating a with-fluids
    object, pushing that object on the dynamic stack. See [Fluids and Dynamic States](#Fluids-and-Dynamic-States).

Macro Instruction: **pop-fluid** [¶](#index-pop_002dfluid)
:   Leave the dynamic extent of a `with-fluid*` expression, restoring
    the fluid to its previous value. `push-fluid` should always be
    balanced with `pop-fluid`.

Macro Instruction: **fluid-ref** dst fluid [¶](#index-fluid_002dref-1)
:   Place the value associated with the fluid fluid in dst.

Macro Instruction: **fluid-set!** fluid value [¶](#index-fluid_002dset_0021-1)
:   Set the value of the fluid fluid to value.

Macro Instruction: **push-dynamic-state** state [¶](#index-push_002ddynamic_002dstate)
:   Save the current set of fluid bindings on the dynamic stack and instate
    the bindings from state instead. See [Fluids and Dynamic States](#Fluids-and-Dynamic-States).

Macro Instruction: **pop-dynamic-state** [¶](#index-pop_002ddynamic_002dstate)
:   Restore a saved set of fluid bindings from the dynamic stack.
    `push-dynamic-state` should always be balanced with
    `pop-dynamic-state`.

Macro Instruction: **resolve-module** dst name public? [¶](#index-resolve_002dmodule-1)
:   Look up the module named name, resolve its public interface if the
    immediate operand public? is true, then place the result in
    dst.

Macro Instruction: **lookup** dst mod sym [¶](#index-lookup)
:   Look up sym in module mod, placing the resulting variable
    (or `#f` if not found) in dst.

Macro Instruction: **define!** dst mod sym [¶](#index-define_0021)
:   Look up sym in module mod, placing the resulting variable in
    dst, creating the variable if needed.

Macro Instruction: **current-module** dst [¶](#index-current_002dmodule-1)
:   Set dst to the current module.

Macro Instruction: **$car** dst src [¶](#index-_0024car)

Macro Instruction: **$cdr** dst src [¶](#index-_0024cdr)

Macro Instruction: **$set-car!** x val [¶](#index-_0024set_002dcar_0021)

Macro Instruction: **$set-cdr!** x val [¶](#index-_0024set_002dcdr_0021)

Macro Instruction: **$variable-ref** dst src [¶](#index-_0024variable_002dref)

Macro Instruction: **$variable-set!** x val [¶](#index-_0024variable_002dset_0021)

Macro Instruction: **$vector-length** dst x [¶](#index-_0024vector_002dlength)

Macro Instruction: **$vector-ref** dst x idx [¶](#index-_0024vector_002dref)

Macro Instruction: **$vector-ref/immediate** dst x idx/imm [¶](#index-_0024vector_002dref_002fimmediate)

Macro Instruction: **$vector-set!** x idx v [¶](#index-_0024vector_002dset_0021)

Macro Instruction: **$vector-set!/immediate** x idx/imm v [¶](#index-_0024vector_002dset_0021_002fimmediate)

Macro Instruction: **$allocate-struct** dst vtable nwords [¶](#index-_0024allocate_002dstruct)

Macro Instruction: **$struct-vtable** dst src [¶](#index-_0024struct_002dvtable)

Macro Instruction: **$struct-ref** dst src idx [¶](#index-_0024struct_002dref)

Macro Instruction: **$struct-ref/immediate** dst src idx/imm [¶](#index-_0024struct_002dref_002fimmediate)

Macro Instruction: **$struct-set!** x idx v [¶](#index-_0024struct_002dset_0021)

Macro Instruction: **$struct-set!/immediate** x idx/imm v [¶](#index-_0024struct_002dset_0021_002fimmediate)
:   Intrinsics for use by the baseline compiler. The usual strategy for CPS
    compilation is to expose the component parts of e.g. `vector-ref`
    so that the compiler can learn from them and eliminate needless bits.
    However in the non-optimizing baseline compiler, that’s just overhead,
    so we have some intrinsics that encapsulate all the usual type checks.

---

Next: [Memory Access Instructions](#Memory-Access-Instructions), Previous: [Intrinsic Call Instructions](#Intrinsic-Call-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.8 Constant Instructions [¶](#Constant-Instructions-1)

The following instructions load literal data into a program. There are
two kinds.

The first set of instructions loads immediate values. These
instructions encode the immediate directly into the instruction stream.

Instruction: **make-immediate** `s8:dst zi16:low-bits` [¶](#index-make_002dimmediate)
:   Make an immediate whose low bits are low-bits, sign-extended.

Instruction: **make-short-immediate** `s8:dst i16:low-bits` [¶](#index-make_002dshort_002dimmediate)
:   Make an immediate whose low bits are low-bits, and whose top bits are
    0.

Instruction: **make-long-immediate** `s24:dst i32:low-bits` [¶](#index-make_002dlong_002dimmediate)
:   Make an immediate whose low bits are low-bits, and whose top bits are
    0.

Instruction: **make-long-long-immediate** `s24:dst a32:high-bits b32:low-bits` [¶](#index-make_002dlong_002dlong_002dimmediate)
:   Make an immediate with high-bits and low-bits.

Non-immediate constant literals are referenced either directly or
indirectly. For example, Guile knows at compile-time what the layout of
a string will be like, and arranges to embed that object directly in the
compiled image. A reference to a string will use
`make-non-immediate` to treat a pointer into the compilation unit
as a `scm` value directly.

Instruction: **make-non-immediate** `s24:dst n32:offset` [¶](#index-make_002dnon_002dimmediate)
:   Load a pointer to statically allocated memory into dst. The
    object’s memory will be found offset 32-bit words away from the
    current instruction pointer. Whether the object is mutable or immutable
    depends on where it was allocated by the compiler, and loaded by the
    loader.

Sometimes you need to load up a code pointer into a register; for this,
use `load-label`.

Instruction: **load-label** `s24:dst l32:offset` [¶](#index-load_002dlabel)
:   Load a label offset words away from the current `ip` and
    write it to dst. offset is a signed 32-bit integer.

Finally, Guile supports a number of unboxed data types, with their
associate constant loaders.

Instruction: **load-f64** `s24:dst au32:high-bits au32:low-bits` [¶](#index-load_002df64)
:   Load a double-precision floating-point value formed by joining
    high-bits and low-bits, and write it to dst.

Instruction: **load-u64** `s24:dst au32:high-bits au32:low-bits` [¶](#index-load_002du64)
:   Load an unsigned 64-bit integer formed by joining high-bits and
    low-bits, and write it to dst.

Instruction: **load-s64** `s24:dst au32:high-bits au32:low-bits` [¶](#index-load_002ds64)
:   Load a signed 64-bit integer formed by joining high-bits and
    low-bits, and write it to dst.

Some objects must be unique across the whole system. This is the case
for symbols and keywords. For these objects, Guile arranges to
initialize them when the compilation unit is loaded, storing them into a
slot in the image. References go indirectly through that slot.
`static-ref` is used in this case.

Instruction: **static-ref** `s24:dst r32:offset` [¶](#index-static_002dref)
:   Load a scm value into dst. The scm value will be fetched from
    memory, offset 32-bit words away from the current instruction
    pointer. offset is a signed value.

Fields of non-immediates may need to be fixed up at load time, because
we do not know in advance at what address they will be loaded. This is
the case, for example, for a pair containing a non-immediate in one of
its fields. `static-set!` and `static-patch!` are used in
these situations.

Instruction: **static-set!** `s24:src lo32:offset` [¶](#index-static_002dset_0021)
:   Store a scm value into memory, offset 32-bit words away from the
    current instruction pointer. offset is a signed value.

Instruction: **static-patch!** `x24:_ lo32:dst-offset l32:src-offset` [¶](#index-static_002dpatch_0021)
:   Patch a pointer at dst-offset to point to src-offset. Both offsets
    are signed 32-bit values, indicating a memory address as a number
    of 32-bit words away from the current instruction pointer.

---

Next: [Atomic Memory Access Instructions](#Atomic-Memory-Access-Instructions), Previous: [Constant Instructions](#Constant-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.9 Memory Access Instructions [¶](#Memory-Access-Instructions-1)

In these instructions, the `/immediate` variants represent their
indexes or counts as immediates; otherwise these values are unboxed u64
locals.

Instruction: **allocate-words** `s12:dst s12:count` [¶](#index-allocate_002dwords)

Instruction: **allocate-words/immediate** `s12:dst c12:count` [¶](#index-allocate_002dwords_002fimmediate)
:   Allocate a fresh GC-traced object consisting of count words and
    store it into dst.

Instruction: **scm-ref** `s8:dst s8:obj s8:idx` [¶](#index-scm_002dref)

Instruction: **scm-ref/immediate** `s8:dst s8:obj c8:idx` [¶](#index-scm_002dref_002fimmediate)
:   Load the `SCM` object at word offset idx from local
    obj, and store it to dst.

Instruction: **scm-set!** `s8:dst s8:idx s8:obj` [¶](#index-scm_002dset_0021)

Instruction: **scm-set!/immediate** `s8:dst c8:idx s8:obj` [¶](#index-scm_002dset_0021_002fimmediate)
:   Store the `scm` local val into object obj at word
    offset idx.

Instruction: **scm-ref/tag** `s8:dst s8:obj c8:tag` [¶](#index-scm_002dref_002ftag)
:   Load the first word of obj, subtract the immediate tag, and store the
    resulting `SCM` to dst.

Instruction: **scm-set!/tag** `s8:obj c8:tag s8:val` [¶](#index-scm_002dset_0021_002ftag)
:   Set the first word of obj to the unpacked bits of the `scm`
    value val plus the immediate value tag.

Instruction: **word-ref** `s8:dst s8:obj s8:idx` [¶](#index-word_002dref)

Instruction: **word-ref/immediate** `s8:dst s8:obj c8:idx` [¶](#index-word_002dref_002fimmediate)
:   Load the word at offset idx from local obj, and store it to
    the `u64` local dst.

Instruction: **word-set!** `s8:dst s8:idx s8:obj` [¶](#index-word_002dset_0021)

Instruction: **word-set!/immediate** `s8:dst c8:idx s8:obj` [¶](#index-word_002dset_0021_002fimmediate)
:   Store the `u64` local val into object obj at word
    offset idx.

Instruction: **pointer-ref/immediate** `s8:dst s8:obj c8:idx` [¶](#index-pointer_002dref_002fimmediate)
:   Load the pointer at offset idx from local obj, and store it
    to the unboxed pointer local dst.

Instruction: **pointer-set!/immediate** `s8:dst c8:idx s8:obj` [¶](#index-pointer_002dset_0021_002fimmediate)
:   Store the unboxed pointer local val into object obj at word
    offset idx.

Instruction: **tail-pointer-ref/immediate** `s8:dst s8:obj c8:idx` [¶](#index-tail_002dpointer_002dref_002fimmediate)
:   Compute the address of word offset idx from local obj, and store it
    to dst.

---

Next: [Tagging and Untagging Instructions](#Tagging-and-Untagging-Instructions), Previous: [Memory Access Instructions](#Memory-Access-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.10 Atomic Memory Access Instructions [¶](#Atomic-Memory-Access-Instructions-1)

Instruction: **current-thread** `s24:dst` [¶](#index-current_002dthread-2)
:   Write the current thread into dst.

Instruction: **atomic-scm-ref/immediate** `s8:dst s8:obj c8:idx` [¶](#index-atomic_002dscm_002dref_002fimmediate)
:   Atomically load the `SCM` object at word offset idx from
    local obj, using the sequential consistency memory model. Store
    the result to dst.

Instruction: **atomic-scm-set!/immediate** `s8:obj c8:idx s8:val` [¶](#index-atomic_002dscm_002dset_0021_002fimmediate)
:   Atomically set the `SCM` object at word offset idx from local
    obj to val, using the sequential consistency memory model.

Instruction: **atomic-scm-swap!/immediate** `s24:dst x8:_ s24:obj c8:idx s24:val` [¶](#index-atomic_002dscm_002dswap_0021_002fimmediate)
:   Atomically swap the `SCM` value stored in object obj at word
    offset idx with val, using the sequentially consistent
    memory model. Store the previous value to dst.

Instruction: **atomic-scm-compare-and-swap!/immediate** `s24:dst x8:_ s24:obj c8:idx s24:expected x8:_ s24:desired` [¶](#index-atomic_002dscm_002dcompare_002dand_002dswap_0021_002fimmediate)
:   Atomically swap the `SCM` value stored in object obj at word
    offset idx with desired, if and only if the value that was
    there was expected, using the sequentially consistent memory
    model. Store the value that was previously at idx from obj
    in dst.

---

Next: [Integer Arithmetic Instructions](#Integer-Arithmetic-Instructions), Previous: [Atomic Memory Access Instructions](#Atomic-Memory-Access-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.11 Tagging and Untagging Instructions [¶](#Tagging-and-Untagging-Instructions-1)

Instruction: **tag-char** `s12:dst s12:src` [¶](#index-tag_002dchar)
:   Make a `SCM` character whose integer value is the `u64` in
    src, and store it in dst.

Instruction: **untag-char** `s12:dst s12:src` [¶](#index-untag_002dchar)
:   Extract the integer value from the `SCM` character src, and
    store the resulting `u64` in dst.

Instruction: **tag-fixnum** `s12:dst s12:src` [¶](#index-tag_002dfixnum)
:   Make a `SCM` integer whose value is the `s64` in src,
    and store it in dst.

Instruction: **untag-fixnum** `s12:dst s12:src` [¶](#index-untag_002dfixnum)
:   Extract the integer value from the `SCM` integer src, and
    store the resulting `s64` in dst.

---

Next: [Floating-Point Arithmetic Instructions](#Floating_002dPoint-Arithmetic-Instructions), Previous: [Tagging and Untagging Instructions](#Tagging-and-Untagging-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.12 Integer Arithmetic Instructions [¶](#Integer-Arithmetic-Instructions-1)

Instruction: **uadd** `s8:dst s8:a s8:b` [¶](#index-uadd)

Instruction: **uadd/immediate** `s8:dst s8:a c8:b` [¶](#index-uadd_002fimmediate)
:   Add the `u64` values a and b, and store the `u64`
    result to dst. Overflow will wrap.

Instruction: **usub** `s8:dst s8:a s8:b` [¶](#index-usub)

Instruction: **usub/immediate** `s8:dst s8:a c8:b` [¶](#index-usub_002fimmediate)
:   Subtract the `u64` value b from a, and store the
    `u64` result to dst. Underflow will wrap.

Instruction: **umul** `s8:dst s8:a s8:b` [¶](#index-umul)

Instruction: **umul/immediate** `s8:dst s8:a c8:b` [¶](#index-umul_002fimmediate)
:   Multiply the `u64` values a and b, and store the
    `u64` result to dst. Overflow will wrap.

Instruction: **ulogand** `s8:dst s8:a s8:b` [¶](#index-ulogand)
:   Place the bitwise `and` of the `u64` values a and
    b into the `u64` local dst.

Instruction: **ulogior** `s8:dst s8:a s8:b` [¶](#index-ulogior)
:   Place the bitwise inclusive `or` of the `u64` values a
    and b into the `u64` local dst.

Instruction: **ulogxor** `s8:dst s8:a s8:b` [¶](#index-ulogxor)
:   Place the bitwise exclusive `or` of the `u64` values a
    and b into the `u64` local dst.

Instruction: **ulogsub** `s8:dst s8:a s8:b` [¶](#index-ulogsub)
:   Place the bitwise `and` of the `u64` values a and the
    bitwise `not` of b into the `u64` local dst.

Instruction: **ulsh** `s8:dst s8:a s8:b` [¶](#index-ulsh)

Instruction: **ulsh/immediate** `s8:dst s8:a c8:b` [¶](#index-ulsh_002fimmediate)
:   Shift the unboxed unsigned 64-bit integer in a left by b
    bits, also an unboxed unsigned 64-bit integer. Truncate to 64 bits and
    write to dst as an unboxed value. Only the lower 6 bits of
    b are used.

Instruction: **ursh** `s8:dst s8:a s8:b` [¶](#index-ursh)

Instruction: **ursh/immediate** `s8:dst s8:a c8:b` [¶](#index-ursh_002fimmediate)
:   Shift the unboxed unsigned 64-bit integer in a right by b
    bits, also an unboxed unsigned 64-bit integer. Truncate to 64 bits and
    write to dst as an unboxed value. Only the lower 6 bits of
    b are used.

Instruction: **srsh** `s8:dst s8:a s8:b` [¶](#index-srsh)

Instruction: **srsh/immediate** `s8:dst s8:a c8:b` [¶](#index-srsh_002fimmediate)
:   Shift the unboxed signed 64-bit integer in a right by b
    bits, also an unboxed signed 64-bit integer. Truncate to 64 bits and
    write to dst as an unboxed value. Only the lower 6 bits of
    b are used.

---

Next: [Comparison Instructions](#Comparison-Instructions), Previous: [Integer Arithmetic Instructions](#Integer-Arithmetic-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.13 Floating-Point Arithmetic Instructions [¶](#Floating_002dPoint-Arithmetic-Instructions-1)

Instruction: **fadd** `s8:dst s8:a s8:b` [¶](#index-fadd)
:   Add the `f64` values a and b, and store the `f64`
    result to dst.

Instruction: **fsub** `s8:dst s8:a s8:b` [¶](#index-fsub)
:   Subtract the `f64` value b from a, and store the
    `f64` result to dst.

Instruction: **fmul** `s8:dst s8:a s8:b` [¶](#index-fmul)
:   Multiply the `f64` values a and b, and store the
    `f64` result to dst.

Instruction: **fdiv** `s8:dst s8:a s8:b` [¶](#index-fdiv)
:   Divide the `f64` values a by b, and store the
    `f64` result to dst.

---

Next: [Branch Instructions](#Branch-Instructions), Previous: [Floating-Point Arithmetic Instructions](#Floating_002dPoint-Arithmetic-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.14 Comparison Instructions [¶](#Comparison-Instructions-1)

Comparison instructions set the comparison result, which may then be
utilized by the [Branch Instructions](#Branch-Instructions).

Instruction: **u64=?** `s12:a s12:b` [¶](#index-u64_003d_003f)
:   Set the comparison result to EQUAL if the `u64` values
    a and b are the same, or `NONE` otherwise.

Instruction: **u64<?** `s12:a s12:b` [¶](#index-u64_003c_003f)
:   Set the comparison result to `LESS_THAN` if the `u64` value
    a is less than the `u64` value b are the same, or
    `NONE` otherwise.

Instruction: **s64<?** `s12:a s12:b` [¶](#index-s64_003c_003f)
:   Set the comparison result to `LESS_THAN` if the `s64` value
    a is less than the `s64` value b are the same, or
    `NONE` otherwise.

Instruction: **s64-imm=?** `s12:a z12:b` [¶](#index-s64_002dimm_003d_003f)
:   Set the comparison result to EQUAL if the `s64` value a
    is equal to the immediate `s64` value b, or `NONE`
    otherwise.

Instruction: **u64-imm<?** `s12:a c12:b` [¶](#index-u64_002dimm_003c_003f)
:   Set the comparison result to `LESS_THAN` if the `u64` value
    a is less than the immediate `u64` value b, or
    `NONE` otherwise.

Instruction: **imm-u64<?** `s12:a s12:b` [¶](#index-imm_002du64_003c_003f)
:   Set the comparison result to `LESS_THAN` if the `u64`
    immediate b is less than the `u64` value a, or
    `NONE` otherwise.

Instruction: **s64-imm<?** `s12:a z12:b` [¶](#index-s64_002dimm_003c_003f)
:   Set the comparison result to `LESS_THAN` if the `s64` value
    a is less than the immediate `s64` value b, or
    `NONE` otherwise.

Instruction: **imm-s64<?** `s12:a z12:b` [¶](#index-imm_002ds64_003c_003f)
:   Set the comparison result to `LESS_THAN` if the `s64`
    immediate b is less than the `s64` value a, or
    `NONE` otherwise.

Instruction: **f64=?** `s12:a s12:b` [¶](#index-f64_003d_003f)
:   Set the comparison result to EQUAL if the f64 value a is
    equal to the f64 value b, or `NONE` otherwise.

Instruction: **f64<?** `s12:a s12:b` [¶](#index-f64_003c_003f)
:   Set the comparison result to `LESS_THAN` if the f64 value a
    is less than the f64 value b, `NONE` if a is greater
    than or equal to b, or `INVALID` otherwise.

Instruction: **=?** `s12:a s12:b` [¶](#index-_003d_003f)
:   Set the comparison result to EQUAL if the SCM values a and
    b are numerically equal, in the sense of the Scheme `=`
    operator. Set to `NONE` otherwise.
