Instruction: **heap-numbers-equal?** `s12:a s12:b` [¶](#index-heap_002dnumbers_002dequal_003f)
:   Set the comparison result to EQUAL if the SCM values a and
    b are numerically equal, in the sense of Scheme `=`. Set to
    `NONE` otherwise. It is known that both a and b are
    heap numbers.

Instruction: **<?** `s12:a s12:b` [¶](#index-_003c_003f)
:   Set the comparison result to `LESS_THAN` if the SCM value a
    is less than the SCM value b, `NONE` if a is greater
    than or equal to b, or `INVALID` otherwise.

Instruction: **immediate-tag=?** `s24:obj c16:mask c16:tag` [¶](#index-immediate_002dtag_003d_003f)
:   Set the comparison result to EQUAL if the result of a bitwise
    `and` between the bits of `scm` value obj and the
    immediate mask is tag, or `NONE` otherwise.

Instruction: **heap-tag=?** `s24:obj c16:mask c16:tag` [¶](#index-heap_002dtag_003d_003f)
:   Set the comparison result to EQUAL if the result of a bitwise
    `and` between the first word of `scm` value obj and the
    immediate mask is tag, or `NONE` otherwise.

Instruction: **eq?** `s12:a s12:b` [¶](#index-eq_003f-2)
:   Set the comparison result to EQUAL if the SCM values a and
    b are `eq?`, or `NONE` otherwise.

Instruction: **eq-immediate?** `s8:a zi16:b` [¶](#index-eq_002dimmediate_003f)
:   Set the comparison result to EQUAL if the SCM value a is
    equal to the immediate SCM value b (sign-extended), or `NONE`
    otherwise.

There are a set of macro-instructions for `immediate-tag=?` and
`heap-tag=?` as well that abstract away the precise type tag
values. See [The SCM Type in Guile](#The-SCM-Type-in-Guile).

Macro Instruction: **fixnum?** x [¶](#index-fixnum_003f-1)

Macro Instruction: **heap-object?** x [¶](#index-heap_002dobject_003f)

Macro Instruction: **char?** x [¶](#index-char_003f-2)

Macro Instruction: **eq-false?** x [¶](#index-eq_002dfalse_003f)

Macro Instruction: **eq-nil?** x [¶](#index-eq_002dnil_003f)

Macro Instruction: **eq-null?** x [¶](#index-eq_002dnull_003f)

Macro Instruction: **eq-true?** x [¶](#index-eq_002dtrue_003f)

Macro Instruction: **unspecified?** x [¶](#index-unspecified_003f)

Macro Instruction: **undefined?** x [¶](#index-undefined_003f)

Macro Instruction: **eof-object?** x [¶](#index-eof_002dobject_003f-3)

Macro Instruction: **null?** x [¶](#index-null_003f-2)

Macro Instruction: **false?** x [¶](#index-false_003f)

Macro Instruction: **nil?** x [¶](#index-nil_003f-1)
:   Emit a `immediate-tag=?` instruction that will set the comparison
    result to `EQUAL` if x would pass the corresponding predicate
    (e.g. `null?`), or `NONE` otherwise.

Macro Instruction: **pair?** x [¶](#index-pair_003f-2)

Macro Instruction: **struct?** x [¶](#index-struct_003f-1)

Macro Instruction: **symbol?** x [¶](#index-symbol_003f-2)

Macro Instruction: **variable?** x [¶](#index-variable_003f-1)

Macro Instruction: **vector?** x [¶](#index-vector_003f-3)

Macro Instruction: **immutable-vector?** x [¶](#index-immutable_002dvector_003f)

Macro Instruction: **mutable-vector?** x [¶](#index-mutable_002dvector_003f)

Macro Instruction: **weak-vector?** x [¶](#index-weak_002dvector_003f-1)

Macro Instruction: **string?** x [¶](#index-string_003f-2)

Macro Instruction: **heap-number?** x [¶](#index-heap_002dnumber_003f)

Macro Instruction: **hash-table?** x [¶](#index-hash_002dtable_003f-1)

Macro Instruction: **pointer?** x [¶](#index-pointer_003f-1)

Macro Instruction: **fluid?** x [¶](#index-fluid_003f-1)

Macro Instruction: **stringbuf?** x [¶](#index-stringbuf_003f)

Macro Instruction: **dynamic-state?** x [¶](#index-dynamic_002dstate_003f-1)

Macro Instruction: **frame?** x [¶](#index-frame_003f-1)

Macro Instruction: **keyword?** x [¶](#index-keyword_003f-2)

Macro Instruction: **atomic-box?** x [¶](#index-atomic_002dbox_003f-1)

Macro Instruction: **syntax?** x [¶](#index-syntax_003f)

Macro Instruction: **program?** x [¶](#index-program_003f-1)

Macro Instruction: **vm-continuation?** x [¶](#index-vm_002dcontinuation_003f)

Macro Instruction: **bytevector?** x [¶](#index-bytevector_003f-1)

Macro Instruction: **weak-set?** x [¶](#index-weak_002dset_003f)

Macro Instruction: **weak-table?** x [¶](#index-weak_002dtable_003f)

Macro Instruction: **array?** x [¶](#index-array_003f-1)

Macro Instruction: **bitvector?** x [¶](#index-bitvector_003f-1)

Macro Instruction: **smob?** x [¶](#index-smob_003f)

Macro Instruction: **port?** x [¶](#index-port_003f-2)

Macro Instruction: **bignum?** x [¶](#index-bignum_003f)

Macro Instruction: **flonum?** x [¶](#index-flonum_003f-1)

Macro Instruction: **compnum?** x [¶](#index-compnum_003f)

Macro Instruction: **fracnum?** x [¶](#index-fracnum_003f)
:   Emit a `heap-tag=?` instruction that will set the comparison result
    to `EQUAL` if x would pass the corresponding predicate
    (e.g. `null?`), or `NONE` otherwise.

---

Next: [Raw Memory Access Instructions](#Raw-Memory-Access-Instructions), Previous: [Comparison Instructions](#Comparison-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.15 Branch Instructions [¶](#Branch-Instructions-1)

All offsets to branch instructions are 24-bit signed numbers, which
count 32-bit units. This gives Guile effectively a 26-bit address range
for relative jumps.

Instruction: **j** `l24:offset` [¶](#index-j)
:   Add offset to the current instruction pointer.

Instruction: **jl** `l24:offset` [¶](#index-jl)
:   If the last comparison result is `LESS_THAN`, add offset, a
    signed 24-bit number, to the current instruction pointer.

Instruction: **je** `l24:offset` [¶](#index-je)
:   If the last comparison result is `EQUAL`, add offset, a
    signed 24-bit number, to the current instruction pointer.

Instruction: **jnl** `l24:offset` [¶](#index-jnl)
:   If the last comparison result is not `LESS_THAN`, add offset,
    a signed 24-bit number, to the current instruction pointer.

Instruction: **jne** `l24:offset` [¶](#index-jne)
:   If the last comparison result is not `EQUAL`, add offset, a
    signed 24-bit number, to the current instruction pointer.

Instruction: **jge** `l24:offset` [¶](#index-jge)
:   If the last comparison result is `NONE`, add offset, a
    signed 24-bit number, to the current instruction pointer.

    This is intended for use after a `<?` comparison, and is different
    from `jnl` in the way it handles not-a-number (NaN) values:
    `<?` sets `INVALID` instead of `NONE` if either value is
    a NaN. For exact numbers, `jge` is the same as `jnl`.

Instruction: **jnge** `l24:offset` [¶](#index-jnge)
:   If the last comparison result is not `NONE`, add offset, a
    signed 24-bit number, to the current instruction pointer.

    This is intended for use after a `<?` comparison, and is different
    from `jl` in the way it handles not-a-number (NaN) values:
    `<?` sets `INVALID` instead of `NONE` if either value is
    a NaN. For exact numbers, `jnge` is the same as `jl`.

Instruction: **jtable** `s24:idx v32:length [x8:_ l24:offset]...` [¶](#index-jtable)
:   Branch to an entry in a table, as in C’s `switch` statement.
    idx is a `u64` local indicating which entry to branch to.
    The immediate len indicates the number of entries in the table,
    and should be greater than or equal to 1. The last entry in the table
    is the "catch-all" entry. The offset... values are signed 24-bit
    immediates (`l24` encoding), indicating a memory address as a
    number of 32-bit words away from the current instruction pointer.

---

Previous: [Branch Instructions](#Branch-Instructions), Up: [Instruction Set](#Instruction-Set)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.7.16 Raw Memory Access Instructions [¶](#Raw-Memory-Access-Instructions-1)

Bytevector operations correspond closely to what the current hardware
can do, so it makes sense to inline them to VM instructions, providing
a clear path for eventual native compilation. Without this, Scheme
programs would need other primitives for accessing raw bytes – but
these primitives are as good as any.

Instruction: **u8-ref** `s8:dst s8:ptr s8:idx` [¶](#index-u8_002dref)

Instruction: **s8-ref** `s8:dst s8:ptr s8:idx` [¶](#index-s8_002dref)

Instruction: **u16-ref** `s8:dst s8:ptr s8:idx` [¶](#index-u16_002dref)

Instruction: **s16-ref** `s8:dst s8:ptr s8:idx` [¶](#index-s16_002dref)

Instruction: **u32-ref** `s8:dst s8:ptr s8:idx` [¶](#index-u32_002dref)

Instruction: **s32-ref** `s8:dst s8:ptr s8:idx` [¶](#index-s32_002dref)

Instruction: **u64-ref** `s8:dst s8:ptr s8:idx` [¶](#index-u64_002dref)

Instruction: **s64-ref** `s8:dst s8:ptr s8:idx` [¶](#index-s64_002dref)

Instruction: **f32-ref** `s8:dst s8:ptr s8:idx` [¶](#index-f32_002dref)

Instruction: **f64-ref** `s8:dst s8:ptr s8:idx` [¶](#index-f64_002dref)
:   Fetch the item at byte offset idx from the raw pointer local
    ptr, and store it in dst. All accesses use native
    endianness.

    The idx value should be an unboxed unsigned 64-bit integer.

    The results are all written to the stack as unboxed values, either as
    signed 64-bit integers, unsigned 64-bit integers, or IEEE double
    floating point numbers.

Instruction: **u8-set!** `s8:ptr s8:idx s8:val` [¶](#index-u8_002dset_0021)

Instruction: **s8-set!** `s8:ptr s8:idx s8:val` [¶](#index-s8_002dset_0021)

Instruction: **u16-set!** `s8:ptr s8:idx s8:val` [¶](#index-u16_002dset_0021)

Instruction: **s16-set!** `s8:ptr s8:idx s8:val` [¶](#index-s16_002dset_0021)

Instruction: **u32-set!** `s8:ptr s8:idx s8:val` [¶](#index-u32_002dset_0021)

Instruction: **s32-set!** `s8:ptr s8:idx s8:val` [¶](#index-s32_002dset_0021)

Instruction: **u64-set!** `s8:ptr s8:idx s8:val` [¶](#index-u64_002dset_0021)

Instruction: **s64-set!** `s8:ptr s8:idx s8:val` [¶](#index-s64_002dset_0021)

Instruction: **f32-set!** `s8:ptr s8:idx s8:val` [¶](#index-f32_002dset_0021)

Instruction: **f64-set!** `s8:ptr s8:idx s8:val` [¶](#index-f64_002dset_0021)
:   Store val into memory pointed to by raw pointer local ptr,
    at byte offset idx. Multibyte values are written using native
    endianness.

    The idx value should be an unboxed unsigned 64-bit integer.

    The val values are all unboxed, either as signed 64-bit integers,
    unsigned 64-bit integers, or IEEE double floating point numbers.

---

Previous: [Instruction Set](#Instruction-Set), Up: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 9.3.8 Just-In-Time Native Code [¶](#Just_002dIn_002dTime-Native-Code-1)

The final piece of Guile’s virtual machine is a just-in-time (JIT)
compiler from bytecode instructions to native code. It is faster to run
a function when its bytecode instructions are compiled to native code,
compared to having the VM interpret the instructions.

The JIT compiler runs automatically, triggered by counters associated
with each function. The counter increments when functions are called
and during each loop iteration. Once a function’s counter passes a
certain value, the function gets JIT-compiled. See [Instrumentation Instructions](#Instrumentation-Instructions), for full details.

Guile’s JIT compiler is what is known as a *template JIT*. This
kind of JIT is very simple: for each instruction in a function, the JIT
compiler will emit a generic sequence of machine code corresponding to
the instruction kind, specializing that generic template to reference
the specific operands of the instruction being compiled.

The strength of a template JIT is principally that it is very fast at
emitting code. It doesn’t need to do any time-consuming analysis on the
bytecode that it is compiling to do its job.

A template JIT is also very predictable: the native code emitted by a
template JIT has the same performance characteristics of the
corresponding bytecode, only that it runs faster. In theory you could
even generate the template-JIT machine code ahead of time, as it doesn’t
depend on any value seen at run-time.

This predictability makes it possible to reason about the performance of
a system in terms of bytecode, knowing that the conclusions apply to
native code emitted by a template JIT.

Because the machine code corresponding to an instruction always performs
the same tasks that the interpreter would do for that instruction,
bytecode and a template JIT also allows Guile programmers to debug their
programs in terms of the bytecode model. When a Guile programmer sets a
breakpoint, Guile will disable the JIT for the thread being debugged,
falling back to the interpreter (which has the corresponding code to run
the hooks). See [VM Hooks](#VM-Hooks).

To emit native code, Guile uses a forked version of GNU Lightning. This
"Lightening" effort, spun out as a separate project, aims to build on
the back-end support from GNU Lightning, but adapting the API and
behavior of the library to match Guile’s needs. This code is included
in the Guile source distribution. For more information, see
<https://gitlab.com/wingo/lightening>. As of mid-2019, Lightening
supports code generation for the x86-64, ia32, ARMv7, and AArch64
architectures.

The weaknesses of a template JIT are two-fold. Firstly, as a simple
back-end that has to run fast, a template JIT doesn’t have time to do
analysis that could help it generate better code, notably global
register allocation and instruction selection.

However this is a minor weakness compared to the inability to perform
significant, speculative program transformations. For example, Guile
could see that in an expression `(f x)`, that in practice f
always refers to the same function. An advanced JIT compiler would
speculatively inline f into the call-site, along with a dynamic
check to make sure that the assertion still held. But as a template JIT
doesn’t pay attention to values only known at run-time, it can’t make
this transformation.

This limitation is mitigated in part by Guile’s robust ahead-of-time
compiler which can already perform significant optimizations when it can
prove they will always be valid, and its low-level bytecode which is
able to represent the effect of those optimizations (e.g. elided
type-checks). See [Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine), for more on
Guile’s compiler.

An ahead-of-time Scheme-to-bytecode strategy, complemented by a template
JIT, also particularly suits the somewhat static nature of Scheme.
Scheme programmers often write code in a way that makes the identity of
free variable references lexically apparent. For example, the `(f
x)` expression could appear within a `(let ((f (lambda (x) (1+
x)))) ...)` expression, or we could see that `f` was imported from
a particular module where we know its binding. Ahead-of-time
compilation techniques can work well for a language like Scheme where
there is little polymorphism and much first-order programming. They do
not work so well for a language like JavaScript, which is highly mutable
at run-time and difficult to analyze due to method calls (which are
effectively higher-order calls).

All that said, a template JIT works well for Guile at this point. It’s
only a few thousand lines of maintainable code, it speeds up Scheme
programs, and it keeps the bulk of the Guile Scheme implementation
written in Scheme itself. The next step is probably to add
ahead-of-time native code emission to the back-end of the compiler
written in Scheme, to take advantage of the opportunity to do global
register allocation and instruction selection. Once this is working, it
can allow Guile to experiment with speculative optimizations in Scheme
as well. See [Extending the Compiler](#Extending-the-Compiler), for more on future directions.

Finally, note that there are a few environment variables that can be
tweaked to make JIT compilation happen sooner, later, or never.
See [Environment Variables](#Environment-Variables), for more.

---

Previous: [A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile), Up: [Guile Implementation](#Guile-Implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
