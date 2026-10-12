### 17.1 Creating User Keys [¶](#Creating-User-Keys-1)

Any Calculator command may be bound to a key using the `Z D`
(`calc-user-define`) command. Actually, it is bound to a two-key
sequence beginning with the lower-case `z` prefix.

The `Z D` command first prompts for the key to define. For example,
press `Z D a` to define the new key sequence `z a`. You are then
prompted for the name of the Calculator command that this key should
run. For example, the `calc-sincos` command is not normally
available on a key. Typing `Z D s sincos RET` programs the
`z s` key sequence to run `calc-sincos`. This definition will remain
in effect for the rest of this Emacs session, or until you redefine
`z s` to be something else.

You can actually bind any Emacs command to a `z` key sequence by
backspacing over the ‘`calc-`’ when you are prompted for the command name.

As with any other prefix key, you can type `z ?` to see a list of
all the two-key sequences you have defined that start with `z`.
Initially, no `z` sequences (except `z ?` itself) are defined.

User keys are typically letters, but may in fact be any key.
(`META`-keys are not permitted, nor are a terminal’s special
function keys which generate multi-character sequences when pressed.)
You can define different commands on the shifted and unshifted versions
of a letter if you wish.

The `Z U` (`calc-user-undefine`) command unbinds a user key.
For example, the key sequence `Z U s` will undefine the `sincos`
key we defined above.

The `Z P` (`calc-user-define-permanent`) command makes a key
binding permanent so that it will remain in effect even in future Emacs
sessions. (It does this by adding a suitable bit of Lisp code into
your Calc init file; that is, the file given by the variable
`calc-settings-file`, typically `~/.emacs.d/calc.el`.) For example,
`Z P s` would register our `sincos` command permanently. If
you later wish to unregister this command you must edit your Calc init
file by hand. (See [General Mode Commands](#General-Mode-Commands), for a way to tell Calc to
use a different file for the Calc init file.)

The `Z P` command also saves the user definition, if any, for the
command bound to the key. After `Z F` and `Z C`, a given user
key could invoke a command, which in turn calls an algebraic function,
which might have one or more special display formats. A single `Z P`
command will save all of these definitions.
To save an algebraic function, type `'` (the apostrophe)
when prompted for a key, and type the function name. To save a command
without its key binding, type `M-x` and enter a function name. (The
‘`calc-`’ prefix will automatically be inserted for you.)
(If the command you give implies a function, the function will be saved,
and if the function has any display formats, those will be saved, but
not the other way around: Saving a function will not save any commands
or key bindings associated with the function.)

The `Z E` (`calc-user-define-edit`) command edits the definition
of a user key. This works for keys that have been defined by either
keyboard macros or formulas; further details are contained in the relevant
following sections.

---

Next: [Invocation Macros](#Invocation-Macros), Previous: [Creating User Keys](#Creating-User-Keys), Up: [Programming](#Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 17.2 Programming with Keyboard Macros [¶](#Programming-with-Keyboard-Macros)

The easiest way to “program” the Emacs Calculator is to use standard
keyboard macros. Press `C-x (` to begin recording a macro. From
this point on, keystrokes you type will be saved away as well as
performing their usual functions. Press `C-x )` to end recording.
Press shift-`X` (or the standard Emacs key sequence `C-x e`) to
execute your keyboard macro by replaying the recorded keystrokes.
See [Keyboard Macros](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Keyboard-Macros) in the Emacs Manual, for further
information.

When you use `X` to invoke a keyboard macro, the entire macro is
treated as a single command by the undo and trail features. The stack
display buffer is not updated during macro execution, but is instead
fixed up once the macro completes. Thus, commands defined with keyboard
macros are convenient and efficient. The `C-x e` command, on the
other hand, invokes the keyboard macro with no special treatment: Each
command in the macro will record its own undo information and trail entry,
and update the stack buffer accordingly. If your macro uses features
outside of Calc’s control to operate on the contents of the Calc stack
buffer, or if it includes Undo, Redo, or last-arguments commands, you
must use `C-x e` to make sure the buffer and undo list are up-to-date
at all times. You could also consider using `K` (`calc-keep-args`)
instead of `M-RET` (`calc-last-args`).

Calc extends the standard Emacs keyboard macros in several ways.
Keyboard macros can be used to create user-defined commands. Keyboard
macros can include conditional and iteration structures, somewhat
analogous to those provided by a traditional programmable calculator.

* [Naming Keyboard Macros](#Naming-Keyboard-Macros)
* [Conditionals in Keyboard Macros](#Conditionals-in-Macros)
* [Loops in Keyboard Macros](#Loops-in-Macros)
* [Local Values in Macros](#Local-Values-in-Macros)
* [Queries in Keyboard Macros](#Queries-in-Macros)

---

Next: [Conditionals in Keyboard Macros](#Conditionals-in-Macros), Up: [Programming with Keyboard Macros](#Keyboard-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.2.1 Naming Keyboard Macros [¶](#Naming-Keyboard-Macros-1)

Once you have defined a keyboard macro, you can bind it to a `z`
key sequence with the `Z K` (`calc-user-define-kbd-macro`) command.
This command prompts first for a key, then for a command name. For
example, if you type `C-x ( n TAB n TAB C-x )` you will
define a keyboard macro which negates the top two numbers on the stack
(`TAB` swaps the top two stack elements). Now you can type
`Z K n RET` to define this keyboard macro onto the `z n` key
sequence. The default command name (if you answer the second prompt with
just the `RET` key as in this example) will be something like
‘`calc-User-n`’. The keyboard macro will now be available as both
`z n` and `M-x calc-User-n`. You can backspace and enter a more
descriptive command name if you wish.

Macros defined by `Z K` act like single commands; they are executed
in the same way as by the `X` key. If you wish to define the macro
as a standard no-frills Emacs macro (to be executed as if by `C-x e`),
give a negative prefix argument to `Z K`.

Once you have bound your keyboard macro to a key, you can use
`Z P` to register it permanently with Emacs. See [Creating User Keys](#Creating-User-Keys).

The `Z E` (`calc-user-define-edit`) command on a key that has
been defined by a keyboard macro tries to use the `edmacro` package
edit the macro. Type `C-c C-c` to finish editing and update
the definition stored on the key, or, to cancel the edit, kill the
buffer with `C-x k`.
The special characters `RET`, `LFD`, `TAB`, `SPC`,
`DEL`, and `NUL` must be entered as these three character
sequences, written in all uppercase, as must the prefixes `C-` and
`M-`. Spaces and line breaks are ignored. Other characters are
copied verbatim into the keyboard macro. Basically, the notation is the
same as is used in all of this manual’s examples, except that the manual
takes some liberties with spaces: When we say `' [1 2 3] RET`,
we take it for granted that it is clear we really mean
`' [1 SPC 2 SPC 3] RET`.

The `C-x * m` (`read-kbd-macro`) command reads an Emacs “region”
of spelled-out keystrokes and defines it as the current keyboard macro.
It is a convenient way to define a keyboard macro that has been stored
in a file, or to define a macro without executing it at the same time.

---

Next: [Loops in Keyboard Macros](#Loops-in-Macros), Previous: [Naming Keyboard Macros](#Naming-Keyboard-Macros), Up: [Programming with Keyboard Macros](#Keyboard-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.2.2 Conditionals in Keyboard Macros [¶](#Conditionals-in-Keyboard-Macros)

The `Z [` (`calc-kbd-if`) and `Z ]` (`calc-kbd-end-if`)
commands allow you to put simple tests in a keyboard macro. When Calc
sees the `Z [`, it pops an object from the stack and, if the object is
a non-zero value, continues executing keystrokes. But if the object is
zero, or if it is not provably nonzero, Calc skips ahead to the matching
`Z ]` keystroke. See [Logical Operations](#Logical-Operations), for a set of commands for
performing tests which conveniently produce 1 for true and 0 for false.

For example, `RET 0 a < Z [ n Z ]` implements an absolute-value
function in the form of a keyboard macro. This macro duplicates the
number on the top of the stack, pushes zero and compares using `a <`
(`calc-less-than`), then, if the number was less than zero,
executes `n` (`calc-change-sign`). Otherwise, the change-sign
command is skipped.

To program this macro, type `C-x (`, type the above sequence of
keystrokes, then type `C-x )`. Note that the keystrokes will be
executed while you are making the definition as well as when you later
re-execute the macro by typing `X`. Thus you should make sure a
suitable number is on the stack before defining the macro so that you
don’t get a stack-underflow error during the definition process.

Conditionals can be nested arbitrarily. However, there should be exactly
one `Z ]` for each `Z [` in a keyboard macro.

The `Z :` (`calc-kbd-else`) command allows you to choose between
two keystroke sequences. The general format is `cond Z [
then-part Z : else-part Z ]`. If cond is true
(i.e., if the top of stack contains a non-zero number after cond
has been executed), the then-part will be executed and the
else-part will be skipped. Otherwise, the then-part will
be skipped and the else-part will be executed.

The `Z |` (`calc-kbd-else-if`) command allows you to choose
between any number of alternatives. For example,
`cond1 Z [ part1 Z : cond2 Z | part2 Z :
part3 Z ]` will execute part1 if cond1 is true,
otherwise it will execute part2 if cond2 is true, otherwise
it will execute part3.

More precisely, `Z [` pops a number and conditionally skips to the
next matching `Z :` or `Z ]` key. `Z ]` has no effect when
actually executed. `Z :` skips to the next matching `Z ]`.
`Z |` pops a number and conditionally skips to the next matching
`Z :` or `Z ]`; thus, `Z [` and `Z |` are functionally
equivalent except that `Z [` participates in nesting but `Z |`
does not.

Calc’s conditional and looping constructs work by scanning the
keyboard macro for occurrences of character sequences like ‘`Z:`’
and ‘`Z]`’. One side-effect of this is that if you use these
constructs you must be careful that these character pairs do not
occur by accident in other parts of the macros. Since Calc rarely
uses shift-`Z` for any purpose except as a prefix character, this
is not likely to be a problem. Another side-effect is that it will
not work to define your own custom key bindings for these commands.
Only the standard shift-`Z` bindings will work correctly.

If Calc gets stuck while skipping characters during the definition of a
macro, type `Z C-g` to cancel the definition. (Typing plain `C-g`
actually adds a `C-g` keystroke to the macro.)

---

Next: [Local Values in Macros](#Local-Values-in-Macros), Previous: [Conditionals in Keyboard Macros](#Conditionals-in-Macros), Up: [Programming with Keyboard Macros](#Keyboard-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.2.3 Loops in Keyboard Macros [¶](#Loops-in-Keyboard-Macros)

The `Z <` (`calc-kbd-repeat`) and `Z >`
(`calc-kbd-end-repeat`) commands pop a number from the stack,
which must be an integer, then repeat the keystrokes between the brackets
the specified number of times. If the integer is zero or negative, the
body is skipped altogether. For example, `1 TAB Z < 2 * Z >`
computes two to a nonnegative integer power. First, we push 1 on the
stack and then swap the integer argument back to the top. The `Z <`
pops that argument leaving the 1 back on top of the stack. Then, we
repeat a multiply-by-two step however many times.

Once again, the keyboard macro is executed as it is being entered.
In this case it is especially important to set up reasonable initial
conditions before making the definition: Suppose the integer 1000 just
happened to be sitting on the stack before we typed the above definition!
Another approach is to enter a harmless dummy definition for the macro,
then go back and edit in the real one with a `Z E` command. Yet
another approach is to type the macro as written-out keystroke names
in a buffer, then use `C-x * m` (`read-kbd-macro`) to read the
macro.

The `Z /` (`calc-kbd-break`) command allows you to break out
of a keyboard macro loop prematurely. It pops an object from the stack;
if that object is true (a non-zero number), control jumps out of the
innermost enclosing `Z <` … `Z >` loop and continues
after the `Z >`. If the object is false, the `Z /` has no
effect. Thus `cond Z /` is similar to ‘`if (cond) break;`’
in the C language.

The `Z (` (`calc-kbd-for`) and `Z )` (`calc-kbd-end-for`)
commands are similar to `Z <` and `Z >`, except that they make the
value of the counter available inside the loop. The general layout is
`init final Z ( body step Z )`. The `Z (`
command pops initial and final values from the stack. It then creates
a temporary internal counter and initializes it with the value init.
The `Z (` command then repeatedly pushes the counter value onto the
stack and executes body and step, adding step to the
counter each time until the loop finishes.

By default, the loop finishes when the counter becomes greater than (or
less than) final, assuming initial is less than (greater
than) final. If initial is equal to final, the body
executes exactly once. The body of the loop always executes at least
once. For example, `0 1 10 Z ( 2 ^ + 1 Z )` computes the sum of the
squares of the integers from 1 to 10, in steps of 1.

If you give a numeric prefix argument of 1 to `Z (`, the loop is
forced to use upward-counting conventions. In this case, if initial
is greater than final the body will not be executed at all.
Note that step may still be negative in this loop; the prefix
argument merely constrains the loop-finished test. Likewise, a prefix
argument of *-1* forces downward-counting conventions.

The `Z {` (`calc-kbd-loop`) and `Z }`
(`calc-kbd-end-loop`) commands are similar to `Z <` and
`Z >`, except that they do not pop a count from the stack—they
effectively create an infinite loop. Every `Z {` … `Z }`
loop ought to include at least one `Z /` to make sure the loop
doesn’t run forever. (If any error message occurs which causes Emacs
to beep, the keyboard macro will also be halted; this is a standard
feature of Emacs. You can also generally press `C-g` to halt a
running keyboard macro, although not all versions of Unix support
this feature.)

The conditional and looping constructs are not actually tied to
keyboard macros, but they are most often used in that context.
For example, the keystrokes `10 Z < 23 RET Z >` push
ten copies of 23 onto the stack. This can be typed “live” just
as easily as in a macro definition.

See [Conditionals in Keyboard Macros](#Conditionals-in-Macros), for some additional notes about
conditional and looping commands.

---

Next: [Queries in Keyboard Macros](#Queries-in-Macros), Previous: [Loops in Keyboard Macros](#Loops-in-Macros), Up: [Programming with Keyboard Macros](#Keyboard-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.2.4 Local Values in Macros [¶](#Local-Values-in-Macros-1)

Keyboard macros sometimes want to operate under known conditions
without affecting surrounding conditions. For example, a keyboard
macro may wish to turn on Fraction mode, or set a particular
precision, independent of the user’s normal setting for those
modes.

Macros also sometimes need to use local variables. Assignments to
local variables inside the macro should not affect any variables
outside the macro. The `` Z ` `` (`calc-kbd-push`) and `Z '`
(`calc-kbd-pop`) commands give you both of these capabilities.

When you type `` Z ` `` (with a grave accent),
the values of various mode settings are saved away. The ten “quick”
variables `q0` through `q9` are also saved. When
you type `Z '` (with an apostrophe), these values are restored.
Pairs of `` Z ` `` and `Z '` commands may be nested.

If a keyboard macro halts due to an error in between a `` Z ` `` and
a `Z '`, the saved values will be restored correctly even though
the macro never reaches the `Z '` command. Thus you can use
`` Z ` `` and `Z '` without having to worry about what happens
in exceptional conditions.

If you type `` Z ` `` “live” (not in a keyboard macro), Calc puts
you into a “recursive edit.” You can tell you are in a recursive
edit because there will be extra square brackets in the mode line,
as in ‘`[(Calculator)]`’. These brackets will go away when you
type the matching `Z '` command. The modes and quick variables
will be saved and restored in just the same way as if actual keyboard
macros were involved.

The modes saved by `` Z ` `` and `Z '` are the current precision
and binary word size, the angular mode (Deg, Rad, or HMS), the
simplification mode, Algebraic mode, Symbolic mode, Infinite mode,
Matrix or Scalar mode, Fraction mode, and the current complex mode
(Polar or Rectangular). The ten “quick” variables’ values (or lack
thereof) are also saved.

Most mode-setting commands act as toggles, but with a numeric prefix
they force the mode either on (positive prefix) or off (negative
or zero prefix). Since you don’t know what the environment might
be when you invoke your macro, it’s best to use prefix arguments
for all mode-setting commands inside the macro.

In fact, `` C-u Z ` `` is like `` Z ` `` except that it sets the modes
listed above to their default values. As usual, the matching `Z '`
will restore the modes to their settings from before the `` C-u Z ` ``.
Also, `` Z ` `` with a negative prefix argument resets the algebraic mode
to its default (off) but leaves the other modes the same as they were
outside the construct.

The contents of the stack and trail, values of non-quick variables, and
other settings such as the language mode and the various display modes,
are *not* affected by `` Z ` `` and `Z '`.

---

Previous: [Local Values in Macros](#Local-Values-in-Macros), Up: [Programming with Keyboard Macros](#Keyboard-Macros)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 17.2.5 Queries in Keyboard Macros [¶](#Queries-in-Keyboard-Macros)

The `Z #` (`calc-kbd-query`) command prompts for an algebraic
entry which takes its input from the keyboard, even during macro
execution. All the normal conventions of algebraic input, including the
use of `$` characters, are supported. The prompt message itself is
taken from the top of the stack, and so must be entered (as a string)
before the `Z #` command. (Recall, as a string it can be entered by
pressing the `"` key and will appear as a vector when it is put on
the stack. The prompt message is only put on the stack to provide a
prompt for the `Z #` command; it will not play any role in any
subsequent calculations.) This command allows your keyboard macros to
accept numbers or formulas as interactive input.

As an example,
`2 RET "Power: " RET Z # 3 RET ^` will prompt for
input with “Power: ” in the minibuffer, then return 2 to the provided
power. (The response to the prompt that’s given, 3 in this example,
will not be part of the macro.)

See [Keyboard Macro Query](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Keyboard-Macro-Query) in the Emacs Manual, for a description of
`C-x q` (`kbd-macro-query`), the standard Emacs way to accept
keyboard input during a keyboard macro. In particular, you can use
`C-x q` to enter a recursive edit, which allows the user to perform
any Calculator operations interactively before pressing `C-M-c` to
return control to the keyboard macro.

---

Next: [Programming with Formulas](#Algebraic-Definitions), Previous: [Programming with Keyboard Macros](#Keyboard-Macros), Up: [Programming](#Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 17.3 Invocation Macros [¶](#Invocation-Macros-1)

Calc provides one special keyboard macro, called up by `C-x * z`
(`calc-user-invocation`), that is intended to allow you to define
your own special way of starting Calc. To define this “invocation
macro,” create the macro in the usual way with `C-x (` and
`C-x )`, then type `Z I` (`calc-user-define-invocation`).
There is only one invocation macro, so you don’t need to type any
additional letters after `Z I`. From now on, you can type
`C-x * z` at any time to execute your invocation macro.

For example, suppose you find yourself often grabbing rectangles of
numbers into Calc and multiplying their columns. You can do this
by typing `C-x * r` to grab, and `V R : *` to multiply columns.
To make this into an invocation macro, just type `C-x ( C-x * r
V R : * C-x )`, then `Z I`. Then, to multiply a rectangle of data,
just mark the data in its buffer in the usual way and type `C-x * z`.

Invocation macros are treated like regular Emacs keyboard macros;
all the special features described above for `Z K`-style macros
do not apply. `C-x * z` is just like `C-x e`, except that it
uses the macro that was last stored by `Z I`. (In fact, the
macro does not even have to have anything to do with Calc!)

The `m m` command saves the last invocation macro defined by
`Z I` along with all the other Calc mode settings.
See [General Mode Commands](#General-Mode-Commands).

---

Next: [Programming Calc, with Lisp](#Lisp-Definitions), Previous: [Invocation Macros](#Invocation-Macros), Up: [Programming](#Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 17.4 Programming with Formulas [¶](#Programming-with-Formulas)

Another way to create a new Calculator command uses algebraic formulas.
The `Z F` (`calc-user-define-formula`) command stores the
formula at the top of the stack as the definition for a key. This
command prompts for five things: The key, the command name, the function
name, the argument list, and the behavior of the command when given
non-numeric arguments.

For example, suppose we type `' a+2b RET` to push the formula
‘`a + 2*b`’ onto the stack. We now type `Z F m` to define this
formula on the `z m` key sequence. The next prompt is for a command
name, beginning with ‘`calc-`’, which should be the long (`M-x`) form
for the new command. If you simply press `RET`, a default name like
`calc-User-m` will be constructed. In our example, suppose we enter
`spam RET` to define the new command as `calc-spam`.

If you want to give the formula a long-style name only, you can press
`SPC` or `RET` when asked which single key to use. For example
`Z F RET spam RET` defines the new command as
`M-x calc-spam`, with no keyboard equivalent.

The third prompt is for an algebraic function name. The default is to
use the same name as the command name but without the ‘`calc-`’
prefix. (If this is of the form ‘`User-m`’, the hyphen is removed so
it won’t be taken for a minus sign in algebraic formulas.)
This is the name you will use if you want to enter your
new function in an algebraic formula. Suppose we enter `yow RET`.
Then the new function can be invoked by pushing two numbers on the
stack and typing `z m` or `x spam`, or by entering the algebraic
formula ‘`yow(x,y)`’.

The fourth prompt is for the function’s argument list. This is used to
associate values on the stack with the variables that appear in the formula.
The default is a list of all variables which appear in the formula, sorted
into alphabetical order. In our case, the default would be ‘`(a b)`’.
This means that, when the user types `z m`, the Calculator will remove
two numbers from the stack, substitute these numbers for ‘`a`’ and
‘`b`’ (respectively) in the formula, then simplify the formula and
push the result on the stack. In other words, `10 RET 100 z m`
would replace the 10 and 100 on the stack with the number 210, which is
‘`a + 2 b`’ with ‘`a=10`’ and ‘`b=100`’. Likewise, the formula
‘`yow(10, 100)`’ will be evaluated by substituting ‘`a=10`’ and
‘`b=100`’ in the definition.

You can rearrange the order of the names before pressing `RET` to
control which stack positions go to which variables in the formula. If
you remove a variable from the argument list, that variable will be left
in symbolic form by the command. Thus using an argument list of ‘`(b)`’
for our function would cause `10 z m` to replace the 10 on the stack
with the formula ‘`a + 20`’. If we had used an argument list of
‘`(b a)`’, the result with inputs 10 and 100 would have been 120.

You can also put a nameless function on the stack instead of just a
formula, as in ‘`<a, b : a + 2 b>`’. See [Specifying Operators](#Specifying-Operators).
In this example, the command will be defined by the formula ‘`a + 2 b`’
using the argument list ‘`(a b)`’.

The final prompt is a y-or-n question concerning what to do if symbolic
arguments are given to your function. If you answer `y`, then
executing `z m` (using the original argument list ‘`(a b)`’) with
arguments ‘`10`’ and ‘`x`’ will leave the function in symbolic
form, i.e., ‘`yow(10,x)`’. On the other hand, if you answer `n`,
then the formula will always be expanded, even for non-constant
arguments: ‘`10 + 2 x`’. If you never plan to feed algebraic
formulas to your new function, it doesn’t matter how you answer this
question.

If you answered `y` to this question you can still cause a function
call to be expanded by typing `a "` (`calc-expand-formula`).
Also, Calc will expand the function if necessary when you take a
derivative or integral or solve an equation involving the function.

Once you have defined a formula on a key, you can retrieve this formula
with the `Z G` (`calc-user-define-get-defn`) command. Press a
key, and this command pushes the formula that was used to define that
key onto the stack. Actually, it pushes a nameless function that
specifies both the argument list and the defining formula. You will get
an error message if the key is undefined, or if the key was not defined
by a `Z F` command.

The `Z E` (`calc-user-define-edit`) command on a key that has
been defined by a formula uses a variant of the `calc-edit` command
to edit the defining formula. Press `C-c C-c` to finish editing and
store the new formula back in the definition, or kill the buffer with
`C-x k` to
cancel the edit. (The argument list and other properties of the
definition are unchanged; to adjust the argument list, you can use
`Z G` to grab the function onto the stack, edit with `` ` ``, and
then re-execute the `Z F` command.)

As usual, the `Z P` command records your definition permanently.
In this case it will permanently record all three of the relevant
definitions: the key, the command, and the function.

You may find it useful to turn off the default simplifications with
`m O` (`calc-no-simplify-mode`) when entering a formula to be
used as a function definition. For example, the formula ‘`deriv(a^2,v)`’
which might be used to define a new function ‘`dsqr(a,v)`’ will be
“simplified” to 0 immediately upon entry since `deriv` considers
‘`a`’ to be constant with respect to ‘`v`’. Turning off
default simplifications cures this problem: The definition will be stored
in symbolic form without ever activating the `deriv` function. Press
`m D` to turn the default simplifications back on afterwards.

---

Previous: [Programming with Formulas](#Algebraic-Definitions), Up: [Programming](#Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
