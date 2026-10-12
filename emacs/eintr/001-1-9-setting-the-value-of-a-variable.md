### 1.9 Setting the Value of a Variable

There are several ways by which a variable can be given a value.
One of the ways is to use the special form `setq`. Another way
is to use `let` (see [`let`](#let)). (The jargon for this process is
to *bind* a variable to a value.)

The following sections not only describe how `setq` works but
also illustrate how arguments are passed.

* [Using `setq`](#Using-setq)
* [Counting](#Counting)

---

Next: [Counting](#Counting), Up: [Setting the Value of a Variable](#setq)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.9.1 Using `setq`

To set the value of the symbol `flowers` to the list `(rose
violet daisy buttercup)`, evaluate the following expression by
positioning the cursor after the expression and typing `C-x C-e`.

```
(setq flowers '(rose violet daisy buttercup))
```

The list `(rose violet daisy buttercup)` will appear in the echo
area. This is what is *returned* by the `setq` special
form. As a side effect, the symbol `flowers` is bound to the
list; that is, the symbol `flowers`, which can be viewed as
a variable, is given the list as its value. (This process, by the
way, illustrates how a side effect to the Lisp interpreter, setting
the value, can be the primary effect that we humans are interested in.
This is because every Lisp function must return a value if it does not
get an error, but it will only have a side effect if it is designed to
have one.)

After evaluating the `setq` expression, you can evaluate the
symbol `flowers` and it will return the value you just set.
Here is the symbol. Place your cursor after it and type `C-x C-e`.

```
flowers
```

When you evaluate `flowers`, the list
`(rose violet daisy buttercup)` appears in the echo area.

Incidentally, if you evaluate `'flowers`, the variable with a quote
in front of it, what you will see in the echo area is the symbol itself,
`flowers`. Here is the quoted symbol, so you can try this:

```
'flowers
```

Also, as an added convenience, `setq` permits you to set several
different variables to different values, all in one expression.

To set the value of the variable `carnivores` to the list
`'(lion tiger leopard)` using `setq`, the following expression
is used:

```
(setq carnivores '(lion tiger leopard))
```

Also, `setq` can be used to assign different values to
different variables. The first argument is bound to the value
of the second argument, the third argument is bound to the value of the
fourth argument, and so on. For example, you could use the following to
assign a list of trees to the symbol `trees` and a list of herbivores
to the symbol `herbivores`:

```
(setq trees '(pine fir oak maple)
      herbivores '(gazelle antelope zebra))
```

(The expression could just as well have been on one line, but it might
not have fit on a page; and humans find it easier to read nicely
formatted lists.)

Although I have been using the term “assign”, there is another way
of thinking about the workings of `setq`; and that is to say that
`setq` makes the symbol *point* to the list. This latter
way of thinking is very common and in forthcoming chapters we shall
come upon at least one symbol that has “pointer” as part of its
name. The name is chosen because the symbol has a value, specifically
a list, attached to it; or, expressed another way, the symbol is set
to point to the list.

---

Previous: [Using `setq`](#Using-setq), Up: [Setting the Value of a Variable](#setq)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.9.2 Counting

Here is an example that shows how to use `setq` in a counter. You
might use this to count how many times a part of your program repeats
itself. First set a variable to zero; then add one to the number each
time the program repeats itself. To do this, you need a variable that
serves as a counter, and two expressions: an initial `setq`
expression that sets the counter variable to zero; and a second
`setq` expression that increments the counter each time it is
evaluated.

```
(setq counter 0)                ; Let’s call this the initializer.

(setq counter (+ counter 1))    ; This is the incrementer.

counter                         ; This is the counter.
```

(The text following the ‘`;`’ are comments. See [Change a Function Definition](#Change-a-defun).)

If you evaluate the first of these expressions, the initializer,
`(setq counter 0)`, and then evaluate the third expression,
`counter`, the number `0` will appear in the echo area. If
you then evaluate the second expression, the incrementer, `(setq
counter (+ counter 1))`, the counter will get the value 1. So if you
again evaluate `counter`, the number `1` will appear in the
echo area. Each time you evaluate the second expression, the value of
the counter will be incremented.

When you evaluate the incrementer, `(setq counter (+ counter 1))`,
the Lisp interpreter first evaluates the innermost list; this is the
addition. In order to evaluate this list, it must evaluate the variable
`counter` and the number `1`. When it evaluates the variable
`counter`, it receives its current value. It passes this value and
the number `1` to the `+` which adds them together. The sum
is then returned as the value of the inner list and passed to the
`setq` which sets the variable `counter` to this new value.
Thus, the value of the variable, `counter`, is changed.

---

Next: [Exercises](#Error-Message-Exercises), Previous: [Setting the Value of a Variable](#setq), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.10 Summary

Learning Lisp is like climbing a hill in which the first part is the
steepest. You have now climbed the most difficult part; what remains
becomes easier as you progress onwards.

In summary,

* Lisp programs are made up of expressions, which are lists or single atoms.
* Lists are made up of zero or more atoms or inner lists, separated by whitespace and
  surrounded by parentheses. A list can be empty.
* Atoms are multi-character symbols, like `forward-paragraph`, single
  character symbols like `+`, strings of characters between double
  quotation marks, or numbers.
* A number evaluates to itself.
* A string between double quotes also evaluates to itself.
* When you evaluate a symbol by itself, its value is returned.
* When you evaluate a list, the Lisp interpreter looks at the first symbol
  in the list and then at the function definition bound to that symbol.
  Then the instructions in the function definition are carried out.
* A single-quote ‘`'`’ tells the Lisp interpreter that it should
  return the following expression as written, and not evaluate it as it
  would if the quote were not there.
* Arguments are the information passed to a function. The arguments to a
  function are computed by evaluating the rest of the elements of the list
  of which the function is the first element.
* A function always returns a value when it is evaluated (unless it gets
  an error); in addition, it may also carry out some action that is a
  side effect. In many cases, a function’s primary purpose is to
  create a side effect.

---

Previous: [Summary](#Summary), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.11 Exercises

A few simple exercises:

* Generate an error message by evaluating an appropriate symbol that is
  not within parentheses.
* Generate an error message by evaluating an appropriate symbol that is
  between parentheses.
* Create a counter that increments by two rather than one.
* Write an expression that prints a message in the echo area when
  evaluated.

---

Next: [How To Write Function Definitions](#Writing-Defuns), Previous: [List Processing](#List-Processing), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 2 Practicing Evaluation

Before learning how to write a function definition in Emacs Lisp, it is
useful to spend a little time evaluating various expressions that have
already been written. These expressions will be lists with the
functions as their first (and often only) element. Since some of the
functions associated with buffers are both simple and interesting, we
will start with those. In this section, we will evaluate a few of
these. In another section, we will study the code of several other
buffer-related functions, to see how they were written.

* [How to Evaluate](#How-to-Evaluate)
* [Buffer Names](#Buffer-Names)
* [Getting Buffers](#Getting-Buffers)
* [Switching Buffers](#Switching-Buffers)
* [Buffer Size and the Location of Point](#Buffer-Size-_0026-Locations)
* [Exercise](#Evaluation-Exercise)

---

Next: [Buffer Names](#Buffer-Names), Up: [Practicing Evaluation](#Practicing-Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### How to Evaluate

*Whenever you give an editing command* to Emacs Lisp, such as the
command to move the cursor or to scroll the screen, *you are evaluating
an expression,* the first element of which is a function. *This is
how Emacs works.*

When you type keys, you cause the Lisp interpreter to evaluate an
expression and that is how you get your results. Even typing plain text
involves evaluating an Emacs Lisp function, in this case, one that uses
`self-insert-command`, which simply inserts the character you
typed. The functions you evaluate by typing keystrokes are called
*interactive* functions, or *commands*; how you make a function
interactive will be illustrated in the chapter on how to write function
definitions. See [Making a Function Interactive](#Interactive).

In addition to typing keyboard commands, we have seen a second way to
evaluate an expression: by positioning the cursor after a list and
typing `C-x C-e`. This is what we will do in the rest of this
section. There are other ways to evaluate an expression as well; these
will be described as we come to them.

Besides being used for practicing evaluation, the functions shown in the
next few sections are important in their own right. A study of these
functions makes clear the distinction between buffers and files, how to
switch to a buffer, and how to determine a location within it.

---

Next: [Getting Buffers](#Getting-Buffers), Previous: [How to Evaluate](#How-to-Evaluate), Up: [Practicing Evaluation](#Practicing-Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.1 Buffer Names

The two functions, `buffer-name` and `buffer-file-name`, show
the difference between a file and a buffer. When you evaluate the
following expression, `(buffer-name)`, the name of the buffer
appears in the echo area. When you evaluate `(buffer-file-name)`,
the name of the file to which the buffer refers appears in the echo
area. Usually, the name returned by `(buffer-name)` is the same as
the name of the file to which it refers, and the name returned by
`(buffer-file-name)` is the full path-name of the file.

A file and a buffer are two different entities. A file is information
recorded permanently in the computer (unless you delete it). A buffer,
on the other hand, is information inside of Emacs that will vanish at
the end of the editing session (or when you kill the buffer). Usually,
a buffer contains information that you have copied from a file; we say
the buffer is *visiting* that file. This copy is what you work on
and modify. Changes to the buffer do not change the file, until you
save the buffer. When you save the buffer, the buffer is copied to the file
and is thus saved permanently.

If you are reading this in Info inside of GNU Emacs, you can evaluate
each of the following expressions by positioning the cursor after it and
typing `C-x C-e`.

```
(buffer-name)

(buffer-file-name)
```

When I do this in Info, the value returned by evaluating
`(buffer-name)` is `"*info*"`, and the value returned by
evaluating `(buffer-file-name)` is `nil`.

On the other hand, while I am writing this document, the value
returned by evaluating `(buffer-name)` is
`"introduction.texinfo"`, and the value returned by evaluating
`(buffer-file-name)` is
`"/gnu/work/intro/introduction.texinfo"`.

The former is the name of the buffer and the latter is the name of the
file. In Info, the buffer name is `"*info*"`. Info does not
point to any file, so the result of evaluating
`(buffer-file-name)` is `nil`. The symbol `nil` is
from the Latin word for “nothing”; in this case, it means that the
buffer is not associated with any file. (In Lisp, `nil` is also
used to mean “false” and is a synonym for the empty list, `()`.)

When I am writing, the name of my buffer is
`"introduction.texinfo"`. The name of the file to which it
points is `"/gnu/work/intro/introduction.texinfo"`.

(In the expressions, the parentheses tell the Lisp interpreter to
treat `buffer-name` and `buffer-file-name` as
functions; without the parentheses, the interpreter would attempt to
evaluate the symbols as variables. See [Variables](#Variables).)

In spite of the distinction between files and buffers, you will often
find that people refer to a file when they mean a buffer and vice versa.
Indeed, most people say, “I am editing a file,” rather than saying,
“I am editing a buffer which I will soon save to a file.” It is
almost always clear from context what people mean. When dealing with
computer programs, however, it is important to keep the distinction in mind,
since the computer is not as smart as a person.

The word “buffer”, by the way, comes from the meaning of the word as a
cushion that deadens the force of a collision. In early computers, a
buffer cushioned the interaction between files and the computer’s
central processing unit. The drums or tapes that held a file and the
central processing unit were pieces of equipment that were very
different from each other, working at their own speeds, in spurts. The
buffer made it possible for them to work together effectively.
Eventually, the buffer grew from being an intermediary, a temporary
holding place, to being the place where work is done. This
transformation is rather like that of a small seaport that grew into a
great city: once it was merely the place where cargo was warehoused
temporarily before being loaded onto ships; then it became a business
and cultural center in its own right.

Not all buffers are associated with files. For example, a
`*scratch*` buffer does not visit any file. Similarly, a
`*Help*` buffer is not associated with any file.

In the old days, when you lacked a `~/.emacs` file and started an
Emacs session by typing the command `emacs` alone, without naming
any files, Emacs started with the `*scratch*` buffer visible.
Nowadays, you will see a splash screen. You can follow one of the
commands suggested on the splash screen, visit a file, or press `q`
to quit the splash screen and reach the `*scratch*` buffer.

If you switch to the `*scratch*` buffer, type
`(buffer-name)`, position the cursor after it, and then type
`C-x C-e` to evaluate the expression. The name `"*scratch*"`
will be returned and will appear in the echo area. `"*scratch*"`
is the name of the buffer. When you type `(buffer-file-name)` in
the `*scratch*` buffer and evaluate that, `nil` will appear
in the echo area, just as it does when you evaluate
`(buffer-file-name)` in Info.

Incidentally, if you are in the `*scratch*` buffer and want the
value returned by an expression to appear in the `*scratch*`
buffer itself rather than in the echo area, type `C-u C-x C-e`
instead of `C-x C-e`. This causes the value returned to appear
after the expression. The buffer will look like this:

```
(buffer-name)"*scratch*"
```

You cannot do this in Info since Info is read-only and it will not allow
you to change the contents of the buffer. But you can do this in any
buffer you can edit; and when you write code or documentation (such as
this book), this feature is very useful.

---

Next: [Switching Buffers](#Switching-Buffers), Previous: [Buffer Names](#Buffer-Names), Up: [Practicing Evaluation](#Practicing-Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.2 Getting Buffers

The `buffer-name` function returns the *name* of the buffer;
to get the buffer *itself*, a different function is needed: the
`current-buffer` function. If you use this function in code, what
you get is the buffer itself.

A name and the object or entity to which the name refers are different
from each other. You are not your name. You are a person to whom
others refer by name. If you ask to speak to George and someone hands you
a card with the letters ‘`G`’, ‘`e`’, ‘`o`’, ‘`r`’,
‘`g`’, and ‘`e`’ written on it, you might be amused, but you would
not be satisfied. You do not want to speak to the name, but to the
person to whom the name refers. A buffer is similar: the name of the
scratch buffer is `*scratch*`, but the name is not the buffer. To
get a buffer itself, you need to use a function such as
`current-buffer`.

However, there is a slight complication: if you evaluate
`current-buffer` in an expression on its own, as we will do here,
what you see is a printed representation of the name of the buffer
without the contents of the buffer. Emacs works this way for two
reasons: the buffer may be thousands of lines long—too long to be
conveniently displayed; and, another buffer may have the same contents
but a different name, and it is important to distinguish between them.

Here is an expression containing the function:

```
(current-buffer)
```

If you evaluate this expression in Info in Emacs in the usual way,
`#<buffer *info*>` will appear in the echo area. The special
format indicates that the buffer itself is being returned, rather than
just its name.

Incidentally, while you can type a number or symbol into a program, you
cannot do that with the printed representation of a buffer: the only way
to get a buffer itself is with a function such as `current-buffer`.

A related function is `other-buffer`. This returns the most
recently selected buffer other than the one you are in currently, not
a printed representation of its name. If you have recently switched
back and forth from the `*scratch*` buffer, `other-buffer`
will return that buffer.

You can see this by evaluating the expression:

```
(other-buffer)
```

You should see `#<buffer *scratch*>` appear in the echo area, or
the name of whatever other buffer you switched back from most
recently[6](#FOOT6).

---

Next: [Buffer Size and the Location of Point](#Buffer-Size-_0026-Locations), Previous: [Getting Buffers](#Getting-Buffers), Up: [Practicing Evaluation](#Practicing-Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.3 Switching Buffers

The `other-buffer` function actually provides a buffer when it is
used as an argument to a function that requires one. We can see this
by using `other-buffer` and `switch-to-buffer` to switch to a
different buffer.

But first, a brief introduction to the `switch-to-buffer`
function. When you switched back and forth from Info to the
`*scratch*` buffer to evaluate `(buffer-name)`, you most
likely typed `C-x b` and then typed `*scratch*`[7](#FOOT7) when prompted in the minibuffer for the name of
the buffer to which you wanted to switch. The keystrokes, `C-x
b`, cause the Lisp interpreter to evaluate the interactive function
`switch-to-buffer`. As we said before, this is how Emacs works:
different keystrokes call or run different functions. For example,
`C-f` calls `forward-char`, `M-e` calls
`forward-sentence`, and so on.

By writing `switch-to-buffer` in an expression, and giving it a
buffer to switch to, we can switch buffers just the way `C-x b`
does:

```
(switch-to-buffer (other-buffer))
```

The symbol `switch-to-buffer` is the first element of the list,
so the Lisp interpreter will treat it as a function and carry out the
instructions that are attached to it. But before doing that, the
interpreter will note that `other-buffer` is inside parentheses
and work on that symbol first. `other-buffer` is the first (and
in this case, the only) element of this list, so the Lisp interpreter
calls or runs the function. It returns another buffer. Next, the
interpreter runs `switch-to-buffer`, passing to it, as an
argument, the other buffer, which is what Emacs will switch to. If
you are reading this in Info, try this now. Evaluate the expression.
(To get back, type `C-x b RET`.)[8](#FOOT8)

In the programming examples in later sections of this document, you will
see the function `set-buffer` more often than
`switch-to-buffer`. This is because of a difference between
computer programs and humans: humans have eyes and expect to see the
buffer on which they are working on their computer terminals. This is
so obvious, it almost goes without saying. However, programs do not
have eyes. When a computer program works on a buffer, that buffer does
not need to be visible on the screen.

`switch-to-buffer` is designed for humans and does two different
things: it switches the buffer to which Emacs’s attention is directed; and
it switches the buffer displayed in the window to the new buffer.
`set-buffer`, on the other hand, does only one thing: it switches
the attention of the computer program to a different buffer. The buffer
on the screen remains unchanged (of course, normally nothing happens
there until the command finishes running).

Also, we have just introduced another jargon term, the word *call*.
When you evaluate a list in which the first symbol is a function, you
are calling that function. The use of the term comes from the notion of
the function as an entity that can do something for you if you call
it—just as a plumber is an entity who can fix a leak if you call him
or her.

---

Next: [Exercise](#Evaluation-Exercise), Previous: [Switching Buffers](#Switching-Buffers), Up: [Practicing Evaluation](#Practicing-Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.4 Buffer Size and the Location of Point

Finally, let’s look at several rather simple functions,
`buffer-size`, `point`, `point-min`, and
`point-max`. These give information about the size of a buffer and
the location of point within it.

The function `buffer-size` tells you the size of the current
buffer; that is, the function returns a count of the number of
characters in the buffer.

```
(buffer-size)
```

You can evaluate this in the usual way, by positioning the
cursor after the expression and typing `C-x C-e`.

In Emacs, the current position of the cursor is called *point*.
The expression `(point)` returns a number that tells you where the
cursor is located as a count of the number of characters from the
beginning of the buffer up to point.

You can see the character count for point in this buffer by evaluating
the following expression in the usual way:

```
(point)
```

As I write this, the value of point is 65724. The `point`
function is frequently used in some of the examples later in this
book.

The value of point depends, of course, on its location within the
buffer. If you evaluate point in this spot, the number will be larger:

```
(point)
```

For me, the value of point in this location is 66043, which means that
there are 319 characters (including spaces) between the two
expressions. (Doubtless, you will see different numbers, since I will
have edited this since I first evaluated point.)

The function `point-min` is somewhat similar to `point`, but
it returns the value of the minimum permissible value of point in the
current buffer. This is the number 1 unless *narrowing* is in
effect. (Narrowing is a mechanism whereby you can restrict yourself,
or a program, to operations on just a part of a buffer.
See [Narrowing and Widening](#Narrowing-_0026-Widening).) Likewise, the
function `point-max` returns the value of the maximum permissible
value of point in the current buffer.

---

Previous: [Buffer Size and the Location of Point](#Buffer-Size-_0026-Locations), Up: [Practicing Evaluation](#Practicing-Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.5 Exercise

Find a file with which you are working and move towards its middle.
Find its buffer name, file name, length, and your position in the file.

---

Next: [A Few Buffer-Related Functions](#Buffer-Walk-Through), Previous: [Practicing Evaluation](#Practicing-Evaluation), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 3 How To Write Function Definitions

When the Lisp interpreter evaluates a list, it looks to see whether the
first symbol on the list has a function definition attached to it; or,
put another way, whether the symbol points to a function definition. If
it does, the computer carries out the instructions in the definition. A
symbol that has a function definition is called, simply, a function
(although, properly speaking, the definition is the function and the
symbol refers to it.)

* [An Aside about Primitive Functions](#Primitive-Functions)
* [The `defun` Macro](#defun)
* [Install a Function Definition](#Install)
* [Make a Function Interactive](#Interactive)
* [Different Options for `interactive`](#Interactive-Options)
* [Install Code Permanently](#Permanent-Installation)
* [`let`](#let)
* [The `if` Special Form](#if)
* [If–then–else Expressions](#else)
* [Truth and Falsehood in Emacs Lisp](#Truth-_0026-Falsehood)
* [`save-excursion`](#save_002dexcursion)
* [Review](#Review)
* [Exercises](#defun-Exercises)

---

Next: [The `defun` Macro](#defun), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### An Aside about Primitive Functions

All functions are defined in terms of other functions, except for a few
*primitive* functions that are written in the C programming
language. When you write functions’ definitions, you will write them in
Emacs Lisp and use other functions as your building blocks. Some of the
functions you will use will themselves be written in Emacs Lisp (perhaps
by you) and some will be primitives written in C. The primitive
functions are used exactly like those written in Emacs Lisp and behave
like them. They are written in C so we can easily run GNU Emacs on any
computer that has sufficient power and can run C.

Let me re-emphasize this: when you write code in Emacs Lisp, you do not
distinguish between the use of functions written in C and the use of
functions written in Emacs Lisp. The difference is irrelevant. I
mention the distinction only because it is interesting to know. Indeed,
unless you investigate, you won’t know whether an already-written
function is written in Emacs Lisp or C.

---

Next: [Install a Function Definition](#Install), Previous: [An Aside about Primitive Functions](#Primitive-Functions), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.1 The `defun` Macro

In Lisp, a symbol such as `mark-whole-buffer` has code attached to
it that tells the computer what to do when the function is called.
This code is called the *function definition* and is created by
evaluating a Lisp expression that starts with the symbol `defun`
(which is an abbreviation for *define function*).

In subsequent sections, we will look at function definitions from the
Emacs source code, such as `mark-whole-buffer`. In this section,
we will describe a simple function definition so you can see how it
looks. This function definition uses arithmetic because it makes for a
simple example. Some people dislike examples using arithmetic; however,
if you are such a person, do not despair. Hardly any of the code we
will study in the remainder of this introduction involves arithmetic or
mathematics. The examples mostly involve text in one way or another.

A function definition has up to five parts following the word
`defun`:

1. The name of the symbol to which the function definition should be
   attached.
2. A list of the arguments that will be passed to the function. If no
   arguments will be passed to the function, this is an empty list,
   `()`.
3. Documentation describing the function. (Technically optional, but
   strongly recommended.)
4. Optionally, an expression to make the function interactive so you can
   use it by typing `M-x` and then the name of the function; or by
   typing an appropriate key or keychord.
5. The code that instructs the computer what to do: the *body* of the
   function definition.

It is helpful to think of the five parts of a function definition as
being organized in a template, with slots for each part:

```
(defun function-name (arguments…)
  "optional-documentation…"
  (interactive argument-passing-info)     ; optional
  body…)
```

As an example, here is the code for a function that multiplies its
argument by 7. (This example is not interactive. See [Making a Function Interactive](#Interactive), for that information.)

```
(defun multiply-by-seven (number)
  "Multiply NUMBER by seven."
  (* 7 number))
```

This definition begins with a parenthesis and the symbol `defun`,
followed by the name of the function.

The name of the function is followed by a list that contains the
arguments that will be passed to the function. This list is called
the *argument list*. In this example, the list has only one
element, the symbol, `number`. When the function is used, the
symbol will be bound to the value that is used as the argument to the
function.

Instead of choosing the word `number` for the name of the argument,
I could have picked any other name. For example, I could have chosen
the word `multiplicand`. I picked the word “number” because it
tells what kind of value is intended for this slot; but I could just as
well have chosen the word “multiplicand” to indicate the role that the
value placed in this slot will play in the workings of the function. I
could have called it `foogle`, but that would have been a bad
choice because it would not tell humans what it means. The choice of
name is up to the programmer and should be chosen to make the meaning of
the function clear.

Indeed, you can choose any name you wish for a symbol in an argument
list, even the name of a symbol used in some other function: the name
you use in an argument list is private to that particular definition.
In that definition, the name refers to a different entity than any use
of the same name outside the function definition. Suppose you have a
nick-name “Shorty” in your family; when your family members refer to
“Shorty”, they mean you. But outside your family, in a movie, for
example, the name “Shorty” refers to someone else. Because a name in an
argument list is private to the function definition, you can change the
value of such a symbol inside the body of a function without changing
its value outside the function. The effect is similar to that produced
by a `let` expression. (See [`let`](#let).)

The argument list is followed by the documentation string that
describes the function. This is what you see when you type
`C-h f` and the name of a function. Incidentally, when you
write a documentation string like this, you should make the first line
a complete sentence since some commands, such as `apropos`, print
only the first line of a multi-line documentation string. Also, you
should not indent the second line of a documentation string, if you
have one, because that looks odd when you use `C-h f`
(`describe-function`). The documentation string is optional, but
it is so useful, it should be included in almost every function you
write.

The third line of the example consists of the body of the function
definition. (Most functions’ definitions, of course, are longer than
this.) In this function, the body is the list, `(* 7 number)`, which
says to multiply the value of number by 7. (In Emacs Lisp,
`*` is the function for multiplication, just as `+` is the
function for addition.)

When you use the `multiply-by-seven` function, the argument
`number` evaluates to the actual number you want used. Here is an
example that shows how `multiply-by-seven` is used; but don’t try
to evaluate this yet!

```
(multiply-by-seven 3)
```

The symbol `number`, specified in the function definition in the
next section, is bound to the value 3 in the actual use of
the function. Note that although `number` was inside parentheses
in the function definition, the argument passed to the
`multiply-by-seven` function is not in parentheses. The
parentheses are written in the function definition so the computer can
figure out where the argument list ends and the rest of the function
definition begins.

If you evaluate this example, you are likely to get an error message.
(Go ahead, try it!) This is because we have written the function
definition, but not yet told the computer about the definition—we have
not yet loaded the function definition in Emacs.
Installing a function is the process that tells the Lisp interpreter the
definition of the function. Installation is described in the next
section.

---

Next: [Make a Function Interactive](#Interactive), Previous: [The `defun` Macro](#defun), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.2 Install a Function Definition

If you are reading this inside of Info in Emacs, you can try out the
`multiply-by-seven` function by first evaluating the function
definition and then evaluating `(multiply-by-seven 3)`. A copy of
the function definition follows. Place the cursor after the last
parenthesis of the function definition and type `C-x C-e`. When you
do this, `multiply-by-seven` will appear in the echo area. (What
this means is that when a function definition is evaluated, the value it
returns is the name of the defined function.) At the same time, this
action installs the function definition.

```
(defun multiply-by-seven (number)
  "Multiply NUMBER by seven."
  (* 7 number))
```

By evaluating this `defun`, you have just installed
`multiply-by-seven` in Emacs. The function is now just as much a
part of Emacs as `forward-word` or any other editing function you
use. (`multiply-by-seven` will stay installed until you quit
Emacs. To reload code automatically whenever you start Emacs, see
[Installing Code Permanently](#Permanent-Installation).)

* [The effect of installation](#Effect-of-installation)
* [Change a Function Definition](#Change-a-defun)

---

Next: [Change a Function Definition](#Change-a-defun), Up: [Install a Function Definition](#Install)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The effect of installation

You can see the effect of installing `multiply-by-seven` by
evaluating the following sample. Place the cursor after the following
expression and type `C-x C-e`. The number 21 will appear in the
echo area.

```
(multiply-by-seven 3)
```

If you wish, you can read the documentation for the function by typing
`C-h f` (`describe-function`) and then the name of the
function, `multiply-by-seven`. When you do this, a
`*Help*` window will appear on your screen that says:

```
multiply-by-seven is a Lisp function.

(multiply-by-seven NUMBER)

Multiply NUMBER by seven.
```

(To return to a single window on your screen, type `C-x 1`.)

---

Previous: [The effect of installation](#Effect-of-installation), Up: [Install a Function Definition](#Install)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.2.1 Change a Function Definition

If you want to change the code in `multiply-by-seven`, just rewrite
it. To install the new version in place of the old one, evaluate the
function definition again. This is how you modify code in Emacs. It is
very simple.

As an example, you can change the `multiply-by-seven` function to
add the number to itself seven times instead of multiplying the number
by seven. It produces the same answer, but by a different path. At
the same time, we will add a comment to the code; a comment is text
that the Lisp interpreter ignores, but that a human reader may find
useful or enlightening. The comment is that this is the second
version.

```
(defun multiply-by-seven (number)       ; Second version.
  "Multiply NUMBER by seven."
  (+ number number number number number number number))
```

The comment follows a semicolon, ‘`;`’. In Lisp, everything on a
line that follows a semicolon is a comment. The end of the line is the
end of the comment. To stretch a comment over two or more lines, begin
each line with a semicolon.

See [Beginning a `.emacs`
File](#Beginning-init-File), and [Comments](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Comments) in The GNU Emacs Lisp
Reference Manual, for more about comments.

You can install this version of the `multiply-by-seven` function by
evaluating it in the same way you evaluated the first function: place
the cursor after the last parenthesis and type `C-x C-e`.

In summary, this is how you write code in Emacs Lisp: you write a
function; install it; test it; and then make fixes or enhancements and
install it again.

---

Next: [Different Options for `interactive`](#Interactive-Options), Previous: [Install a Function Definition](#Install), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.3 Make a Function Interactive

You make a function interactive by placing a list that begins with
the special form `interactive` immediately after the
documentation. A user can invoke an interactive function by typing
`M-x` and then the name of the function; or by typing the keys to
which it is bound, for example, by typing `C-n` for
`next-line` or `C-x h` for `mark-whole-buffer`.

Interestingly, when you call an interactive function interactively,
the value returned is not automatically displayed in the echo area.
This is because you often call an interactive function for its side
effects, such as moving forward by a word or line, and not for the
value returned. If the returned value were displayed in the echo area
each time you typed a key, it would be very distracting.

* [An Interactive `multiply-by-seven`, An Overview](#Interactive-multiply_002dby_002dseven)
* [An Interactive `multiply-by-seven`](#multiply_002dby_002dseven-in-detail)

---

Next: [An Interactive `multiply-by-seven`](#multiply_002dby_002dseven-in-detail), Up: [Make a Function Interactive](#Interactive)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### An Interactive `multiply-by-seven`, An Overview

Both the use of the special form `interactive` and one way to
display a value in the echo area can be illustrated by creating an
interactive version of `multiply-by-seven`.

Here is the code:

```
(defun multiply-by-seven (number)       ; Interactive version.
  "Multiply NUMBER by seven."
  (interactive "p")
  (message "The result is %d" (* 7 number)))
```

You can install this code by placing your cursor after it and typing
`C-x C-e`. The name of the function will appear in your echo area.
Then, you can use this code by typing `C-u` and a number and then
typing `M-x multiply-by-seven` and pressing `RET`. The phrase
‘`The result is …`’ followed by the product will appear in the
echo area.

Speaking more generally, you invoke a function like this in either of two
ways:

1. By typing a prefix argument that contains the number to be passed, and
   then typing `M-x` and the name of the function, as with
   `C-u 3 M-x forward-sentence`; or,
2. By typing whatever key or keychord the function is bound to, as with
   `C-u 3 M-e`.

Both the examples just mentioned work identically to move point forward
three sentences. (Since `multiply-by-seven` is not bound to a key,
it could not be used as an example of key binding.)

(See [Some Key Bindings](#Key-Bindings), to learn how to bind a command
to a key.)

A *prefix argument* is passed to an interactive function by typing the
`META` key followed by a number, for example, `M-3 M-e`, or by
typing `C-u` and then a number, for example, `C-u 3 M-e` (if you
type `C-u` without a number, it defaults to 4).

---

Previous: [An Interactive `multiply-by-seven`, An Overview](#Interactive-multiply_002dby_002dseven), Up: [Make a Function Interactive](#Interactive)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.3.1 An Interactive `multiply-by-seven`

Let’s look at the use of the special form `interactive` and then at
the function `message` in the interactive version of
`multiply-by-seven`. You will recall that the function definition
looks like this:

```
(defun multiply-by-seven (number)       ; Interactive version.
  "Multiply NUMBER by seven."
  (interactive "p")
  (message "The result is %d" (* 7 number)))
```

In this function, the expression, `(interactive "p")`, is a list of
two elements. The `"p"` tells Emacs to pass the prefix argument to
the function and use its value for the argument of the function.

The argument will be a number. This means that the symbol
`number` will be bound to a number in the line:

```
(message "The result is %d" (* 7 number))
```

For example, if your prefix argument is 5, the Lisp interpreter will
evaluate the line as if it were:

```
(message "The result is %d" (* 7 5))
```

(If you are reading this in GNU Emacs, you can evaluate this expression
yourself.) First, the interpreter will evaluate the inner list, which
is `(* 7 5)`. This returns a value of 35. Next, it
will evaluate the outer list, passing the values of the second and
subsequent elements of the list to the function `message`.

As we have seen, `message` is an Emacs Lisp function especially
designed for sending a one line message to a user. (See [The `message` function](#message).) In summary, the `message`
function prints its first argument in the echo area as is, except for
occurrences of ‘`%d`’ or ‘`%s`’ (and various other %-sequences
which we have not mentioned). When it sees a control sequence, the
function looks to the second or subsequent arguments and prints the
value of the argument in the location in the string where the control
sequence is located.

In the interactive `multiply-by-seven` function, the control string
is ‘`%d`’, which requires a number, and the value returned by
evaluating `(* 7 5)` is the number 35. Consequently, the number 35
is printed in place of the ‘`%d`’ and the message is ‘`The result
is 35`’.

(Note that when you call the function `multiply-by-seven`, the
message is printed without quotes, but when you call `message`, the
text is printed in double quotes. This is because the value returned by
`message` is what appears in the echo area when you evaluate an
expression whose first element is `message`; but when embedded in a
function, `message` prints the text as a side effect without
quotes.)

---

Next: [Install Code Permanently](#Permanent-Installation), Previous: [Make a Function Interactive](#Interactive), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.4 Different Options for `interactive`

In the example, `multiply-by-seven` used `"p"` as the
argument to `interactive`. This argument told Emacs to interpret
your typing either `C-u` followed by a number or `META`
followed by a number as a command to pass that number to the function
as its argument. Emacs has more than twenty characters predefined for
use with `interactive`. In almost every case, one of these
options will enable you to pass the right information interactively to
a function. (See [Code Characters for
`interactive`](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Interactive-Codes) in The GNU Emacs Lisp Reference Manual.)

Consider the function `zap-to-char`. Its interactive expression
is

```
(interactive "p\ncZap to char: ")
```

The first part of the argument to `interactive` is ‘`p`’, with
which you are already familiar. This argument tells Emacs to
interpret a prefix, as a number to be passed to the function. You
can specify a prefix either by typing `C-u` followed by a number
or by typing `META` followed by a number. The prefix is the
number of specified characters. Thus, if your prefix is three and the
specified character is ‘`x`’, then you will delete all the text up
to and including the third next ‘`x`’. If you do not set a prefix,
then you delete all the text up to and including the specified
character, but no more.

The ‘`c`’ tells the function the name of the character to which to delete.

More formally, a function with two or more arguments can have
information passed to each argument by adding parts to the string that
follows `interactive`. When you do this, the information is
passed to each argument in the same order it is specified in the
`interactive` list. In the string, each part is separated from
the next part by a ‘`\n`’, which is a newline. For example, you
can follow ‘`p`’ with a ‘`\n`’ and an ‘`cZap to char:` ’.
This causes Emacs to pass the value of the prefix argument (if there
is one) and the character.

In this case, the function definition looks like the following, where
`arg` and `char` are the symbols to which `interactive`
binds the prefix argument and the specified character:

```
(defun name-of-function (arg char)
  "documentation…"
  (interactive "p\ncZap to char: ")
  body-of-function…)
```

(The space after the colon in the prompt makes it look better when you
are prompted. See [The Definition of
`copy-to-buffer`](#copy_002dto_002dbuffer), for an example.)

When a function does not take arguments, `interactive` does not
require any. Such a function contains the simple expression
`(interactive)`. The `mark-whole-buffer` function is like
this.

Alternatively, if the special letter-codes are not right for your
application, you can pass your own arguments to `interactive` as
a list.

See [The Definition of `append-to-buffer`](#append_002dto_002dbuffer),
for an example. See [Using `Interactive`](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Using-Interactive) in The GNU Emacs Lisp Reference Manual, for a more complete
explanation about this technique.

---

Next: [`let`](#let), Previous: [Different Options for `interactive`](#Interactive-Options), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.5 Install Code Permanently

When you install a function definition by evaluating it, it will stay
installed until you quit Emacs. The next time you start a new session
of Emacs, the function will not be installed unless you evaluate the
function definition again.

At some point, you may want to have code installed automatically
whenever you start a new session of Emacs. There are several ways of
doing this:

* If you have code that is just for yourself, you can put the code for the
  function definition in your `.emacs` initialization file. When you
  start Emacs, your `.emacs` file is automatically evaluated and all
  the function definitions within it are installed.
  See [Your `.emacs` File](#Emacs-Initialization).
* Alternatively, you can put the function definitions that you want
  installed in one or more files of their own and use the `load`
  function to cause Emacs to evaluate and thereby install each of the
  functions in the files.
  See [Loading Files](#Loading-Files).
* Thirdly, if you have code that your whole site will use, it is usual
  to put it in a file called `site-init.el` that is loaded when
  Emacs is built. This makes the code available to everyone who uses
  your machine. (See the `INSTALL` file that is part of the Emacs
  distribution.)

Finally, if you have code that everyone who uses Emacs may want, you
can post it on a computer network or send a copy to the Free Software
Foundation. (When you do this, please license the code and its
documentation under a license that permits other people to run, copy,
study, modify, and redistribute the code and which protects you from
having your work taken from you.) If you send a copy of your code to
the Free Software Foundation, and properly protect yourself and
others, it may be included in the next release of Emacs. In large
part, this is how Emacs has grown over the past years, by donations.

---

Next: [The `if` Special Form](#if), Previous: [Install Code Permanently](#Permanent-Installation), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.6 `let`

The `let` expression is a special form in Lisp that you will need
to use in most function definitions.

`let` is used to attach or bind a symbol to a value in such a way
that the Lisp interpreter will not confuse the variable with a
variable of the same name that is not part of the function.

To understand why the `let` special form is necessary, consider
the situation in which you own a home that you generally refer to as
“the house”, as in the sentence, “The house needs painting.” If you
are visiting a friend and your host refers to “the house”, he is
likely to be referring to *his* house, not yours, that is, to a
different house.

If your friend is referring to his house and you think he is referring
to your house, you may be in for some confusion. The same thing could
happen in Lisp if a variable that is used inside of one function has
the same name as a variable that is used inside of another function,
and the two are not intended to refer to the same value. The
`let` special form prevents this kind of confusion.

* [`let` Prevents Confusion](#Prevent-confusion)
* [The Parts of a `let` Expression](#Parts-of-let-Expression)
* [Sample `let` Expression](#Sample-let-Expression)
* [Uninitialized Variables in a `let` Statement](#Uninitialized-let-Variables)
* [How `let` Binds Variables](#How-let-Binds-Variables)

---

Next: [The Parts of a `let` Expression](#Parts-of-let-Expression), Up: [`let`](#let)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### `let` Prevents Confusion

The `let` special form prevents confusion. `let` creates a
name for a *local variable* that overshadows any use of the same
name outside the `let` expression (in computer science jargon, we
call this *binding* the variable). This is like understanding
that in your host’s home, whenever he refers to “the house”, he
means his house, not yours. (The symbols used to name function
arguments are bound as local variables in exactly the same way.
See [The `defun` Macro](#defun).)

Another way to think about `let` is that it defines a special
region in your code: within the body of the `let` expression, the
variables you’ve named have their own local meaning. Outside of the
`let` body, they have other meanings (or they may not be defined
at all). This means that inside the `let` body, calling
`setq` for a variable named by the `let` expression will set
the value of the *local* variable of that name. However, outside
of the `let` body (such as when calling a function that was
defined elsewhere), calling `setq` for a variable named by the
`let` expression will *not* affect that local
variable.[9](#FOOT9)

`let` can create more than one variable at once. Also,
`let` gives each variable it creates an initial value, either a
value specified by you, or `nil`. (In the jargon, this is
binding the variable to the value.) After `let` has created
and bound the variables, it executes the code in the body of the
`let`, and returns the value of the last expression in the body,
as the value of the whole `let` expression. (“Execute” is a jargon
term that means to evaluate a list; it comes from the use of the word
meaning “to give practical effect to” (Oxford English
Dictionary). Since you evaluate an expression to perform an action,
“execute” has evolved as a synonym to “evaluate”.)

---

Next: [Sample `let` Expression](#Sample-let-Expression), Previous: [`let` Prevents Confusion](#Prevent-confusion), Up: [`let`](#let)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.6.1 The Parts of a `let` Expression

A `let` expression is a list of three parts. The first part is
the symbol `let`. The second part is a list, called a
*varlist*, each element of which is either a symbol by itself or a
two-element list, the first element of which is a symbol. The third
part of the `let` expression is the body of the `let`. The
body usually consists of one or more lists.

A template for a `let` expression looks like this:

```
(let varlist body…)
```

The symbols in the varlist are the variables that are given initial
values by the `let` special form. Symbols by themselves are given
the initial value of `nil`; and each symbol that is the first
element of a two-element list is bound to the value that is returned
when the Lisp interpreter evaluates the second element.

Thus, a varlist might look like this: `(thread (needles 3))`. In
this case, in a `let` expression, Emacs binds the symbol
`thread` to an initial value of `nil`, and binds the symbol
`needles` to an initial value of 3.

When you write a `let` expression, what you do is put the
appropriate expressions in the slots of the `let` expression
template.

If the varlist is composed of two-element lists, as is often the case,
the template for the `let` expression looks like this:

```
(let ((variable value)
      (variable value)
      …)
  body…)
```

---

Next: [Uninitialized Variables in a `let` Statement](#Uninitialized-let-Variables), Previous: [The Parts of a `let` Expression](#Parts-of-let-Expression), Up: [`let`](#let)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.6.2 Sample `let` Expression

The following expression creates and gives initial values
to the two variables `zebra` and `tiger`. The body of the
`let` expression is a list which calls the `message` function.

```
(let ((zebra "stripes")
      (tiger "fierce"))
  (message "One kind of animal has %s and another is %s."
           zebra tiger))
```

Here, the varlist is `((zebra "stripes") (tiger "fierce"))`.

The two variables are `zebra` and `tiger`. Each variable is
the first element of a two-element list and each value is the second
element of its two-element list. In the varlist, Emacs binds the
variable `zebra` to the value `"stripes"`[10](#FOOT10), and binds the
variable `tiger` to the value `"fierce"`. In this example,
both values are strings. The values could just as well have been
another list or a symbol. The body of the `let`
follows after the list holding the variables. In this example, the
body is a list that uses the `message` function to print a string
in the echo area.

You may evaluate the example in the usual fashion, by placing the
cursor after the last parenthesis and typing `C-x C-e`. When you do
this, the following will appear in the echo area:

```
"One kind of animal has stripes and another is fierce."
```

As we have seen before, the `message` function prints its first
argument, except for ‘`%s`’. In this example, the value of the variable
`zebra` is printed at the location of the first ‘`%s`’ and the
value of the variable `tiger` is printed at the location of the
second ‘`%s`’.

---

Next: [How `let` Binds Variables](#How-let-Binds-Variables), Previous: [Sample `let` Expression](#Sample-let-Expression), Up: [`let`](#let)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.6.3 Uninitialized Variables in a `let` Statement

If you do not bind the variables in a `let` statement to specific
initial values, they will automatically be bound to an initial value of
`nil`, as in the following expression:

```
(let ((birch 3)
      pine
      fir
      (oak 'some))
  (message
   "Here are %d variables with %s, %s, and %s value."
   birch pine fir oak))
```

Here, the varlist is `((birch 3) pine fir (oak 'some))`.

If you evaluate this expression in the usual way, the following will
appear in your echo area:

```
"Here are 3 variables with nil, nil, and some value."
```

In this example, Emacs binds the symbol `birch` to the number 3,
binds the symbols `pine` and `fir` to `nil`, and binds
the symbol `oak` to the value `some`.

Note that in the first part of the `let`, the variables `pine`
and `fir` stand alone as atoms that are not surrounded by
parentheses; this is because they are being bound to `nil`, the
empty list. But `oak` is bound to `some` and so is a part of
the list `(oak 'some)`. Similarly, `birch` is bound to the
number 3 and so is in a list with that number. (Since a number
evaluates to itself, the number does not need to be quoted. Also, the
number is printed in the message using a ‘`%d`’ rather than a
‘`%s`’.) The four variables as a group are put into a list to
delimit them from the body of the `let`.

---

Previous: [Uninitialized Variables in a `let` Statement](#Uninitialized-let-Variables), Up: [`let`](#let)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.6.4 How `let` Binds Variables

Emacs Lisp supports two different ways of binding variable names to
their values. These ways affect the parts of your program where a
particular binding is valid. For historical reasons, Emacs Lisp uses
a form of variable binding called *dynamic binding* by default.
However, in this manual we discuss the preferred form of binding,
called *lexical binding*, unless otherwise noted (in the future,
the Emacs maintainers plan to change the default to lexical binding).
If you have programmed in other languages before, you’re likely
already familiar with how lexical binding behaves.

In order to use lexical binding in a program, you should add this to
the first line of your Emacs Lisp file:

```
;;; -*- lexical-binding: t -*-
```

For more information about this, see [Variable Scoping](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Variable-Scoping) in The Emacs Lisp Reference Manual.

* [Differences Between Lexical and Dynamic Binding](#Lexical-_0026-Dynamic-Binding-Differences)
* [Example of Lexical vs. Dynamic Binding](#Lexical-vs-Dynamic-Binding-Example)

---

Next: [Example of Lexical vs. Dynamic Binding](#Lexical-vs-Dynamic-Binding-Example), Up: [How `let` Binds Variables](#How-let-Binds-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Differences Between Lexical and Dynamic Binding

As we discussed before (see [`let` Prevents Confusion](#Prevent-confusion)), when you create
local variables with `let` under lexical binding, those variables
are valid only within the body of the `let` expression. In other
parts of your code, they have other meanings, so if you call a
function defined elsewhere within the `let` body, that function
would be unable to “see” the local variables you’ve created. (On
the other hand, if you call a function that was defined within a
`let` body, that function *would* be able to see—and
modify—the local variables from that `let` expression.)

Under dynamic binding, the rules are different: instead, when you use
`let`, the local variables you’ve created are valid during
execution of the `let` expression. This means that, if your
`let` expression calls a function, that function can see these
local variables, regardless of where the function is defined
(including in another file entirely).

Another way to think about `let` when using dynamic binding is
that every variable name has a global “stack” of bindings, and
whenever you use that variable’s name, it refers to the binding on the
top of the stack. (You can imagine this like a stack of papers on
your desk with the values written on them.) When you bind a variable
dynamically with `let`, it puts the new binding you’ve specified
on the top of the stack, and then executes the `let` body. Once
the `let` body finishes, it takes that binding off of the stack,
revealing the one it had (if any) before the `let` expression.

---

Previous: [Differences Between Lexical and Dynamic Binding](#Lexical-_0026-Dynamic-Binding-Differences), Up: [How `let` Binds Variables](#How-let-Binds-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Example of Lexical vs. Dynamic Binding

In some cases, both lexical and dynamic binding behave identically.
However, in other cases, they can change the meaning of your program.
For example, see what happens in this code under lexical binding:

```
;;; -*- lexical-binding: t -*-

(setq x 0)

(defun getx ()
  x)

(setq x 1)

(let ((x 2))
  (getx))
     ⇒ 1
```

Here, the result of `(getx)` is `1`. Under lexical binding,
`getx` doesn’t see the value from our `let` expression.
That’s because the body of `getx` is outside of the body of our
`let` expression. Since `getx` is defined at the top,
global level of our code (i.e. not inside the body of any `let`
expression), it looks for and finds `x` at the global level as
well. When executing `getx`, the current global value of
`x` is `1`, so that’s what `getx` returns.

If we use dynamic binding instead, the behavior is different:

```
;;; -*- lexical-binding: nil -*-

(setq x 0)

(defun getx ()
  x)

(setq x 1)

(let ((x 2))
  (getx))
     ⇒ 2
```

Now, the result of `(getx)` is `2`! That’s because under
dynamic binding, when executing `getx`, the current binding for
`x` at the top of our stack is the one from our `let`
binding. This time, `getx` doesn’t see the global value for
`x`, since its binding is below the one from our `let`
expression in the stack of bindings.

(Some variables are also “special”, and they are always dynamically
bound even when `lexical-binding` is `t`. See [Initializing a Variable with `defvar`](#defvar).)

---

Next: [If–then–else Expressions](#else), Previous: [`let`](#let), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.7 The `if` Special Form

Another special form is the conditional `if`. This form is used
to instruct the computer to make decisions. You can write function
definitions without using `if`, but it is used often enough, and
is important enough, to be included here. It is used, for example, in
the code for the function `beginning-of-buffer`.

The basic idea behind an `if`, is that *if* a test is true,
*then* an expression is evaluated. If the test is not true, the
expression is not evaluated. For example, you might make a decision
such as, “if it is warm and sunny, then go to the beach!”

* [`if` in more detail](#if-in-more-detail)
* [The `type-of-animal` Function in Detail](#type_002dof_002danimal-in-detail)

---

Next: [The `type-of-animal` Function in Detail](#type_002dof_002danimal-in-detail), Up: [The `if` Special Form](#if)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### `if` in more detail

An `if` expression written in Lisp does not use the word “then”;
the test and the action are the second and third elements of the list
whose first element is `if`. Nonetheless, the test part of an
`if` expression is often called the *if-part* and the second
argument is often called the *then-part*.

Also, when an `if` expression is written, the true-or-false-test
is usually written on the same line as the symbol `if`, but the
action to carry out if the test is true, the then-part, is written
on the second and subsequent lines. This makes the `if`
expression easier to read.

```
(if true-or-false-test
    action-to-carry-out-if-test-is-true)
```

The true-or-false-test will be an expression that
is evaluated by the Lisp interpreter.

Here is an example that you can evaluate in the usual manner. The test
is whether the number 5 is greater than the number 4. Since it is, the
message ‘`5 is greater than 4!`’ will be printed.

```
(if (> 5 4)                             ; if-part
    (message "5 is greater than 4!"))   ; then-part
```

(The function `>` tests whether its first argument is greater than
its second argument and returns true if it is.)

Of course, in actual use, the test in an `if` expression will not
be fixed for all time as it is by the expression `(> 5 4)`.
Instead, at least one of the variables used in the test will be bound to
a value that is not known ahead of time. (If the value were known ahead
of time, we would not need to run the test!)

For example, the value may be bound to an argument of a function
definition. In the following function definition, the character of the
animal is a value that is passed to the function. If the value bound to
`characteristic` is `"fierce"`, then the message, ‘`It is a
tiger!`’ will be printed; otherwise, `nil` will be returned.

```
(defun type-of-animal (characteristic)
  "Print message in echo area depending on CHARACTERISTIC.
If the CHARACTERISTIC is the string \"fierce\",
then warn of a tiger."
  (if (equal characteristic "fierce")
      (message "It is a tiger!")))
```

If you are reading this inside of GNU Emacs, you can evaluate the
function definition in the usual way to install it in Emacs, and then you
can evaluate the following two expressions to see the results:

```
(type-of-animal "fierce")

(type-of-animal "striped")
```

When you evaluate `(type-of-animal "fierce")`, you will see the
following message printed in the echo area: `"It is a tiger!"`; and
when you evaluate `(type-of-animal "striped")` you will see `nil`
printed in the echo area.

---

Previous: [`if` in more detail](#if-in-more-detail), Up: [The `if` Special Form](#if)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.7.1 The `type-of-animal` Function in Detail

Let’s look at the `type-of-animal` function in detail.

The function definition for `type-of-animal` was written by filling
the slots of two templates, one for a function definition as a whole, and
a second for an `if` expression.

The template for every function that is not interactive is:

```
(defun name-of-function (argument-list)
  "documentation…"
  body…)
```

The parts of the function that match this template look like this:

```
(defun type-of-animal (characteristic)
  "Print message in echo area depending on CHARACTERISTIC.
If the CHARACTERISTIC is the string \"fierce\",
then warn of a tiger."
  body: the if expression)
```

The name of function is `type-of-animal`; it is passed the value
of one argument. The argument list is followed by a multi-line
documentation string. The documentation string is included in the
example because it is a good habit to write documentation string for
every function definition. The body of the function definition
consists of the `if` expression.

The template for an `if` expression looks like this:

```
(if true-or-false-test
    action-to-carry-out-if-the-test-returns-true)
```

In the `type-of-animal` function, the code for the `if`
looks like this:

```
(if (equal characteristic "fierce")
    (message "It is a tiger!"))
```

Here, the true-or-false-test is the expression:

```
(equal characteristic "fierce")
```

In Lisp, `equal` is a function that determines whether its first
argument is equal to its second argument. The second argument is the
string `"fierce"` and the first argument is the value of the
symbol `characteristic`—in other words, the argument passed to
this function.

In the first exercise of `type-of-animal`, the argument
`"fierce"` is passed to `type-of-animal`. Since `"fierce"`
is equal to `"fierce"`, the expression, `(equal characteristic
"fierce")`, returns a value of true. When this happens, the `if`
evaluates the second argument or then-part of the `if`:
`(message "It is a tiger!")`.

On the other hand, in the second exercise of `type-of-animal`, the
argument `"striped"` is passed to `type-of-animal`. `"striped"`
is not equal to `"fierce"`, so the then-part is not evaluated and
`nil` is returned by the `if` expression.

---

Next: [Truth and Falsehood in Emacs Lisp](#Truth-_0026-Falsehood), Previous: [The `if` Special Form](#if), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.8 If–then–else Expressions

An `if` expression may have an optional third argument, called
the *else-part*, for the case when the true-or-false-test returns
false. When this happens, the second argument or then-part of the
overall `if` expression is *not* evaluated, but the third or
else-part *is* evaluated. You might think of this as the cloudy
day alternative for the decision “if it is warm and sunny, then go to
the beach, else read a book!”.

The word “else” is not written in the Lisp code; the else-part of an
`if` expression comes after the then-part. In the written Lisp, the
else-part is usually written to start on a line of its own and is
indented less than the then-part:

```
(if true-or-false-test
    action-to-carry-out-if-the-test-returns-true
  action-to-carry-out-if-the-test-returns-false)
```

For example, the following `if` expression prints the message ‘`4
is not greater than 5!`’ when you evaluate it in the usual way:

```
(if (> 4 5)                               ; if-part
    (message "4 falsely greater than 5!") ; then-part
  (message "4 is not greater than 5!"))   ; else-part
```

Note that the different levels of indentation make it easy to
distinguish the then-part from the else-part. (GNU Emacs has several
commands that automatically indent `if` expressions correctly.
See [GNU Emacs Helps You Type Lists](#Typing-Lists).)

We can extend the `type-of-animal` function to include an
else-part by simply incorporating an additional part to the `if`
expression.

You can see the consequences of doing this if you evaluate the following
version of the `type-of-animal` function definition to install it
and then evaluate the two subsequent expressions to pass different
arguments to the function.

```
(defun type-of-animal (characteristic)  ; Second version.
  "Print message in echo area depending on CHARACTERISTIC.
If the CHARACTERISTIC is the string \"fierce\",
then warn of a tiger; else say it is not fierce."
  (if (equal characteristic "fierce")
      (message "It is a tiger!")
    (message "It is not fierce!")))
```

  

```
(type-of-animal "fierce")

(type-of-animal "striped")
```

When you evaluate `(type-of-animal "fierce")`, you will see the
following message printed in the echo area: `"It is a tiger!"`; but
when you evaluate `(type-of-animal "striped")`, you will see
`"It is not fierce!"`.

(Of course, if the characteristic were `"ferocious"`, the
message `"It is not fierce!"` would be printed; and it would be
misleading! When you write code, you need to take into account the
possibility that some such argument will be tested by the `if`
and write your program accordingly.)

---

Next: [`save-excursion`](#save_002dexcursion), Previous: [If–then–else Expressions](#else), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.9 Truth and Falsehood in Emacs Lisp

There is an important aspect to the truth test in an `if`
expression. So far, we have spoken of “true” and “false” as values of
predicates as if they were new kinds of Emacs Lisp objects. In fact,
“false” is just our old friend `nil`. Anything else—anything
at all—is “true”.

The expression that tests for truth is interpreted as *true*
if the result of evaluating it is a value that is not `nil`. In
other words, the result of the test is considered true if the value
returned is a number such as 47, a string such as `"hello"`, or a
symbol (other than `nil`) such as `flowers`, or a list (so
long as it is not empty), or even a buffer!

* [An explanation of `nil`](#nil-explained)

---

Up: [Truth and Falsehood in Emacs Lisp](#Truth-_0026-Falsehood)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### An explanation of `nil`

Before illustrating a test for truth, we need an explanation of `nil`.

In Emacs Lisp, the symbol `nil` has two meanings. First, it means the
empty list. Second, it means false and is the value returned when a
true-or-false-test tests false. `nil` can be written as an empty
list, `()`, or as `nil`. As far as the Lisp interpreter is
concerned, `()` and `nil` are the same. Humans, however, tend
to use `nil` for false and `()` for the empty list.

In Emacs Lisp, any value that is not `nil`—is not the empty
list—is considered true. This means that if an evaluation returns
something that is not an empty list, an `if` expression will test
true. For example, if a number is put in the slot for the test, it
will be evaluated and will return itself, since that is what numbers
do when evaluated. In this conditional, the `if` expression will
test true. The expression tests false only when `nil`, an empty
list, is returned by evaluating the expression.

You can see this by evaluating the two expressions in the following examples.

In the first example, the number 4 is evaluated as the test in the
`if` expression and returns itself; consequently, the then-part
of the expression is evaluated and returned: ‘`true`’ appears in
the echo area. In the second example, the `nil` indicates false;
consequently, the else-part of the expression is evaluated and
returned: ‘`false`’ appears in the echo area.

```
(if 4
    'true
  'false)
```

```

```

```
(if nil
    'true
  'false)
```

Incidentally, if some other useful value is not available for a test that
returns true, then the Lisp interpreter will return the symbol `t`
for true. For example, the expression `(> 5 4)` returns `t`
when evaluated, as you can see by evaluating it in the usual way:

```
(> 5 4)
```

On the other hand, this function returns `nil` if the test is false.

```
(> 4 5)
```

---

Next: [Review](#Review), Previous: [Truth and Falsehood in Emacs Lisp](#Truth-_0026-Falsehood), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.10 `save-excursion`

The `save-excursion` function is the final special form that we
will discuss in this chapter.

In Emacs Lisp programs used for editing, the `save-excursion`
function is very common. It saves the location of point,
executes the body of the function, and then restores point to
its previous position if its location was changed. Its primary
purpose is to keep the user from being surprised and disturbed by
unexpected movement of point.

* [Point and Mark](#Point-and-mark)
* [Template for a `save-excursion` Expression](#Template-for-save_002dexcursion)

---

Next: [Template for a `save-excursion` Expression](#Template-for-save_002dexcursion), Up: [`save-excursion`](#save_002dexcursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Point and Mark

Before discussing `save-excursion`, however, it may be useful
first to review what point and mark are in GNU Emacs. *Point* is
the current location of the cursor. Wherever the cursor
is, that is point. More precisely, on terminals where the cursor
appears to be on top of a character, point is immediately before the
character. In Emacs Lisp, point is an integer. The first character in
a buffer is number one, the second is number two, and so on. The
function `point` returns the current position of the cursor as a
number. Each buffer has its own value for point.

The *mark* is another position in the buffer; its value can be set
with a command such as `C-SPC` (`set-mark-command`). If
a mark has been set, you can use the command `C-x C-x`
(`exchange-point-and-mark`) to cause the cursor to jump to the mark
and set the mark to be the previous position of point. In addition, if
you set another mark, the position of the previous mark is saved in the
mark ring. Many mark positions can be saved this way. You can jump the
cursor to a saved mark by typing `C-u C-SPC` one or more
times.

The part of the buffer between point and mark is called *the
region*. Numerous commands work on the region, including
`center-region`, `count-words-region`, `kill-region`, and
`print-region`.

The `save-excursion` special form saves the location of point and
restores this position after the code within the body of the
special form is evaluated by the Lisp interpreter. Thus, if point were
in the beginning of a piece of text and some code moved point to the end
of the buffer, the `save-excursion` would put point back to where
it was before, after the expressions in the body of the function were
evaluated.

In Emacs, a function frequently moves point as part of its internal
workings even though a user would not expect this. For example,
`count-words-region` moves point. To prevent the user from being
bothered by jumps that are both unexpected and (from the user’s point of
view) unnecessary, `save-excursion` is often used to keep point in
the location expected by the user. The use of
`save-excursion` is good housekeeping.

To make sure the house stays clean, `save-excursion` restores the
value of point even if something goes wrong in the code inside
of it (or, to be more precise and to use the proper jargon, “in case of
abnormal exit”). This feature is very helpful.

In addition to recording the value of point,
`save-excursion` keeps track of the current buffer, and restores
it, too. This means you can write code that will change the buffer and
have `save-excursion` switch you back to the original buffer.
This is how `save-excursion` is used in `append-to-buffer`.
(See [The Definition of `append-to-buffer`](#append_002dto_002dbuffer).)

---

Previous: [Point and Mark](#Point-and-mark), Up: [`save-excursion`](#save_002dexcursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.10.1 Template for a `save-excursion` Expression

The template for code using `save-excursion` is simple:

```
(save-excursion
  body…)
```

The body of the function is one or more expressions that will be
evaluated in sequence by the Lisp interpreter. If there is more than
one expression in the body, the value of the last one will be returned
as the value of the `save-excursion` function. The other
expressions in the body are evaluated only for their side effects; and
`save-excursion` itself is used only for its side effect (which
is restoring the position of point).

In more detail, the template for a `save-excursion` expression
looks like this:

```
(save-excursion
  first-expression-in-body
  second-expression-in-body
  third-expression-in-body
   …
  last-expression-in-body)
```

An expression, of course, may be a symbol on its own or a list.

In Emacs Lisp code, a `save-excursion` expression often occurs
within the body of a `let` expression. It looks like this:

```
(let varlist
  (save-excursion
    body…))
```

---

Next: [Exercises](#defun-Exercises), Previous: [`save-excursion`](#save_002dexcursion), Up: [How To Write Function Definitions](#Writing-Defuns)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
