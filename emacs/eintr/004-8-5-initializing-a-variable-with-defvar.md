### 8.5 Initializing a Variable with `defvar`

The `copy-region-as-kill` function is written in Emacs Lisp. Two
functions within it, `kill-append` and `kill-new`, copy a
region in a buffer and save it in a variable called the
`kill-ring`. This section describes how the `kill-ring`
variable is created and initialized using the `defvar` special
form.

(Again we note that the term `kill-ring` is a misnomer. The text
that is clipped out of the buffer can be brought back; it is not a ring
of corpses, but a ring of resurrectable text.)

In Emacs Lisp, a variable such as the `kill-ring` is created and
given an initial value by using the `defvar` special form. The
name comes from “define variable”.

The `defvar` special form is similar to `setq` in that it
sets the value of a variable. It is unlike `setq` in three ways:
first, it marks the variable as “special” so that it is always
dynamically bound, even when `lexical-binding` is `t`
(see [How `let` Binds Variables](#How-let-Binds-Variables)). Second, it only sets the value of
the variable if the variable does not already have a value. If the
variable already has a value, `defvar` does not override the
existing value. Third, `defvar` has a documentation string.

(There is a related macro, `defcustom`, designed for variables
that people customize. It has more features than `defvar`.
(See [Setting Variables with `defcustom`](#defcustom).)

* [Seeing the Current Value of a Variable](#See-variable-current-value)
* [`defvar` and an asterisk](#defvar-and-asterisk)

---

Next: [`defvar` and an asterisk](#defvar-and-asterisk), Up: [Initializing a Variable with `defvar`](#defvar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Seeing the Current Value of a Variable

You can see the current value of a variable, any variable, by using
the `describe-variable` function, which is usually invoked by
typing `C-h v`. If you type `C-h v` and then `kill-ring`
(followed by `RET`) when prompted, you will see what is in your
current kill ring—this may be quite a lot! Conversely, if you have
been doing nothing this Emacs session except read this document, you
may have nothing in it. Also, you will see the documentation for
`kill-ring`:

```
Documentation:
List of killed text sequences.
Since the kill ring is supposed to interact nicely with cut-and-paste
facilities offered by window systems, use of this variable should
```

```
interact nicely with `interprogram-cut-function' and
`interprogram-paste-function'.  The functions `kill-new',
`kill-append', and `current-kill' are supposed to implement this
interaction; you may want to use them instead of manipulating the kill
ring directly.
```

The kill ring is defined by a `defvar` in the following way:

```
(defvar kill-ring nil
  "List of killed text sequences.
…")
```

In this variable definition, the variable is given an initial value of
`nil`, which makes sense, since if you have saved nothing, you want
nothing back if you give a `yank` command. The documentation
string is written just like the documentation string of a `defun`.
As with the documentation string of the `defun`, the first line of
the documentation should be a complete sentence, since some commands,
like `apropos`, print only the first line of documentation.
Succeeding lines should not be indented; otherwise they look odd when
you use `C-h v` (`describe-variable`).

---

Previous: [Seeing the Current Value of a Variable](#See-variable-current-value), Up: [Initializing a Variable with `defvar`](#defvar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.5.1 `defvar` and an asterisk

In the past, Emacs used the `defvar` special form both for
internal variables that you would not expect a user to change and for
variables that you do expect a user to change. Although you can still
use `defvar` for user customizable variables, please use
`defcustom` instead, since it provides a path into
the Customization commands. (See [Specifying Variables
using `defcustom`](#defcustom).)

When you specified a variable using the `defvar` special form,
you could distinguish a variable that a user might want to change from
others by typing an asterisk, ‘`*`’, in the first column of its
documentation string. For example:

```
(defvar shell-command-default-error-buffer nil
  "*Buffer name for `shell-command' … error output.
… ")
```

You could (and still can) use the `set-variable` command to
change the value of `shell-command-default-error-buffer`
temporarily. However, options set using `set-variable` are set
only for the duration of your editing session. The new values are not
saved between sessions. Each time Emacs starts, it reads the original
value, unless you change the value within your `.emacs` file,
either by setting it manually or by using `customize`.
See [Your `.emacs` File](#Emacs-Initialization).

For me, the major use of the `set-variable` command is to suggest
variables that I might want to set in my `.emacs` file. There
are now more than 700 such variables, far too many to remember
readily. Fortunately, you can press `TAB` after calling the
`M-x set-variable` command to see the list of variables.
(See [Examining and Setting Variables](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Examining) in The GNU Emacs Manual.)

---

Next: [Searching Exercises](#search-Exercises), Previous: [Initializing a Variable with `defvar`](#defvar), Up: [Cutting and Storing Text](#Cutting-_0026-Storing-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.6 Review

Here is a brief summary of some recently introduced functions.

`car`

`cdr`
:   `car` returns the first element of a list; `cdr` returns the
    second and subsequent elements of a list.

    For example:

    ```
    (car '(1 2 3 4 5 6 7))
         ⇒ 1
    (cdr '(1 2 3 4 5 6 7))
         ⇒ (2 3 4 5 6 7)
    ```

`cons`
:   `cons` constructs a list by prepending its first argument to its
    second argument.

    For example:

    ```
    (cons 1 '(2 3 4))
         ⇒ (1 2 3 4)
    ```

`funcall`
:   `funcall` evaluates its first argument as a function. It passes
    its remaining arguments to its first argument.

`nthcdr`
:   Return the result of taking CDR n times on a list.
    The “rest of the rest”, as it were.

    For example:

    ```
    (nthcdr 3 '(1 2 3 4 5 6 7))
         ⇒ (4 5 6 7)
    ```

`setcar`

`setcdr`
:   `setcar` changes the first element of a list; `setcdr`
    changes the second and subsequent elements of a list.

    For example:

    ```
    (setq triple (list 1 2 3))

    (setcar triple '37)

    triple
         ⇒ (37 2 3)

    (setcdr triple '("foo" "bar"))

    triple
         ⇒ (37 "foo" "bar")
    ```

`progn`
:   Evaluate each argument in sequence and then return the value of the
    last.

    For example:

    ```
    (progn 1 2 3 4)
         ⇒ 4
    ```

`save-restriction`
:   Record whatever narrowing is in effect in the current buffer, if any,
    and restore that narrowing after evaluating the arguments.

`search-forward`
:   Search for a string, and if the string is found, move point. With a
    regular expression, use the similar `re-search-forward`.
    (See [Regular Expression Searches](#Regexp-Search), for an
    explanation of regular expression patterns and searches.)

    `search-forward` and `re-search-forward` take four
    arguments:

    1. The string or regular expression to search for.
    2. Optionally, the limit of the search.
    3. Optionally, what to do if the search fails, return `nil` or an
       error message.
    4. Optionally, how many times to repeat the search; if negative, the
       search goes backwards.

`kill-region`

`delete-and-extract-region`

`copy-region-as-kill`
:   `kill-region` cuts the text between point and mark from the
    buffer and stores that text in the kill ring, so you can get it back
    by yanking.

    `copy-region-as-kill` copies the text between point and mark into
    the kill ring, from which you can get it by yanking. The function
    does not cut or remove the text from the buffer.

`delete-and-extract-region` removes the text between point and
mark from the buffer and throws it away. You cannot get it back.
(This is not an interactive command.)

---

Previous: [Review](#cons-_0026-search_002dfwd-Review), Up: [Cutting and Storing Text](#Cutting-_0026-Storing-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.7 Searching Exercises

* Write an interactive function that searches for a string. If the
  search finds the string, leave point after it and display a message
  that says “Found!”. (Do not use `search-forward` for the name
  of this function; if you do, you will overwrite the existing version of
  `search-forward` that comes with Emacs. Use a name such as
  `test-search` instead.)
* Write a function that prints the third element of the kill ring in the
  echo area, if any; if the kill ring does not contain a third element,
  print an appropriate message.

---

Next: [Yanking Text Back](#Yanking), Previous: [Cutting and Storing Text](#Cutting-_0026-Storing-Text), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 9 How Lists are Implemented

In Lisp, atoms are recorded in a straightforward fashion; if the
implementation is not straightforward in practice, it is, nonetheless,
straightforward in theory. The atom ‘`rose`’, for example, is
recorded as the four contiguous letters ‘`r`’, ‘`o`’, ‘`s`’,
‘`e`’. A list, on the other hand, is kept differently. The mechanism
is equally simple, but it takes a moment to get used to the idea. A
list is kept using a series of pairs of pointers. In the series, the
first pointer in each pair points to an atom or to another list, and the
second pointer in each pair points to the next pair, or to the symbol
`nil`, which marks the end of the list.

A pointer itself is quite simply the electronic address of what is
pointed to. Hence, a list is kept as a series of electronic addresses.

* [Lists diagrammed](#Lists-diagrammed)
* [Symbols as a Chest of Drawers](#Symbols-as-Chest)
* [Exercise](#List-Exercise)

---

Next: [Symbols as a Chest of Drawers](#Symbols-as-Chest), Up: [How Lists are Implemented](#List-Implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Lists diagrammed

For example, the list `(rose violet buttercup)` has three
elements, ‘`rose`’, ‘`violet`’, and ‘`buttercup`’. In the
computer, the electronic address of ‘`rose`’ is recorded in a
segment of computer memory called a *cons cell* (because it’s what
the function `cons` actually creates). That cons cell also holds
the address of a second cons cell, whose CAR is the atom
‘`violet`’; and that address (the one that tells where to find
‘`violet`’) is kept along with the address of a third cons cell
which holds the address for the atom ‘`buttercup`’.

This sounds more complicated than it is and is easier seen in a diagram:

```
    ___ ___      ___ ___      ___ ___
   |___|___|--> |___|___|--> |___|___|--> nil
     |            |            |
     |            |            |
      --> rose     --> violet   --> buttercup
```

  
  

In the diagram, each box represents a word of computer memory that
holds a Lisp object, usually in the form of a memory address. The boxes,
i.e., the addresses, are in pairs. Each arrow points to what the address
is the address of, either an atom or another pair of addresses. The
first box is the electronic address of ‘`rose`’ and the arrow points
to ‘`rose`’; the second box is the address of the next pair of boxes,
the first part of which is the address of ‘`violet`’ and the second
part of which is the address of the next pair. The very last box
points to the symbol `nil`, which marks the end of the list.

When a variable is set to a list with an operation such as `setq`,
it stores the address of the first box in the variable. Thus,
evaluation of the expression

```
(setq bouquet '(rose violet buttercup))
```

creates a situation like this:

```
bouquet
     |
     |     ___ ___      ___ ___      ___ ___
      --> |___|___|--> |___|___|--> |___|___|--> nil
            |            |            |
            |            |            |
             --> rose     --> violet   --> buttercup
```

  
  

In this example, the symbol `bouquet` holds the address of the first
pair of boxes.

This same list can be illustrated in a different sort of box notation
like this:

```
bouquet
 |
 |    --------------       ---------------       ----------------
 |   | car   | cdr  |     | car    | cdr  |     | car     | cdr  |
  -->| rose  |   o------->| violet |   o------->| butter- |  nil |
     |       |      |     |        |      |     | cup     |      |
      --------------       ---------------       ----------------
```

  
  

(Symbols consist of more than pairs of addresses, but the structure of
a symbol is made up of addresses. Indeed, the symbol `bouquet`
consists of a group of address-boxes, one of which is the address of
the printed word ‘`bouquet`’, a second of which is the address of a
function definition attached to the symbol, if any, a third of which
is the address of the first pair of address-boxes for the list
`(rose violet buttercup)`, and so on. Here we are showing that
the symbol’s third address-box points to the first pair of
address-boxes for the list.)

If a symbol is set to the CDR of a list, the list itself is not
changed; the symbol simply has an address further down the list. (In
the jargon, CAR and CDR are “non-destructive”.) Thus,
evaluation of the following expression

```
(setq flowers (cdr bouquet))
```

produces this:

  

```
bouquet        flowers
  |              |
  |     ___ ___  |     ___ ___      ___ ___
   --> |   |   |  --> |   |   |    |   |   |
       |___|___|----> |___|___|--> |___|___|--> nil
         |              |            |
         |              |            |
          --> rose       --> violet   --> buttercup
```

  
  
  

The value of `flowers` is `(violet buttercup)`, which is
to say, the symbol `flowers` holds the address of the pair of
address-boxes, the first of which holds the address of `violet`,
and the second of which holds the address of `buttercup`.

A pair of address-boxes is called a *cons cell* or *dotted
pair*. See [Cons Cell and List Types](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Cons-Cell-Type) in The GNU Emacs Lisp
Reference Manual, and [Dotted Pair
Notation](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Dotted-Pair-Notation) in The GNU Emacs Lisp Reference Manual, for more
information about cons cells and dotted pairs.

The function `cons` adds a new pair of addresses to the front of
a series of addresses like that shown above. For example, evaluating
the expression

```
(setq bouquet (cons 'lily bouquet))
```

produces:

  

```
bouquet                       flowers
  |                             |
  |     ___ ___        ___ ___  |     ___ ___       ___ ___
   --> |   |   |      |   |   |  --> |   |   |     |   |   |
       |___|___|----> |___|___|----> |___|___|---->|___|___|--> nil
         |              |              |             |
         |              |              |             |
          --> lily      --> rose       --> violet    --> buttercup
```

  
  
  

However, this does not change the value of the symbol
`flowers`, as you can see by evaluating the following,

```
(eq (cdr (cdr bouquet)) flowers)
```

which returns `t` for true.

Until it is reset, `flowers` still has the value
`(violet buttercup)`; that is, it has the address of the cons
cell whose first address is of `violet`. Also, this does not
alter any of the pre-existing cons cells; they are all still there.

Thus, in Lisp, to get the CDR of a list, you just get the address
of the next cons cell in the series; to get the CAR of a list,
you get the address of the first element of the list; to `cons` a
new element on a list, you add a new cons cell to the front of the list.
That is all there is to it! The underlying structure of Lisp is
brilliantly simple!

And what does the last address in a series of cons cells refer to? It
is the address of the empty list, of `nil`.

In summary, when a Lisp variable is set to a value, it is provided with
the address of the list to which the variable refers.

---

Next: [Exercise](#List-Exercise), Previous: [Lists diagrammed](#Lists-diagrammed), Up: [How Lists are Implemented](#List-Implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.1 Symbols as a Chest of Drawers

In an earlier section, I suggested that you might imagine a symbol as
being a chest of drawers. The function definition is put in one
drawer, the value in another, and so on. What is put in the drawer
holding the value can be changed without affecting the contents of the
drawer holding the function definition, and vice versa.

Actually, what is put in each drawer is the address of the value or
function definition. It is as if you found an old chest in the attic,
and in one of its drawers you found a map giving you directions to
where the buried treasure lies.

(In addition to its name, symbol definition, and variable value, a
symbol has a drawer for a *property list* which can be used to
record other information. Property lists are not discussed here; see
[Property Lists](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Property-Lists) in The GNU Emacs Lisp
Reference Manual.)

Here is a fanciful representation:

  

```
            Chest of Drawers            Contents of Drawers

            __   o0O0o   __
          /                 \
         ---------------------
        |    directions to    |            [map to]
        |     symbol name     |             bouquet
        |                     |
        +---------------------+
        |    directions to    |
        |  symbol definition  |             [none]
        |                     |
        +---------------------+
        |    directions to    |            [map to]
        |    variable value   |             (rose violet buttercup)
        |                     |
        +---------------------+
        |    directions to    |
        |    property list    |             [not described here]
        |                     |
        +---------------------+
        |/                   \|
```

  
  
  

---

Previous: [Symbols as a Chest of Drawers](#Symbols-as-Chest), Up: [How Lists are Implemented](#List-Implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.2 Exercise

Set `flowers` to `violet` and `buttercup`. Cons two
more flowers on to this list and set this new list to
`more-flowers`. Set the CAR of `flowers` to a fish.
What does the `more-flowers` list now contain?

---

Next: [Loops and Recursion](#Loops-_0026-Recursion), Previous: [How Lists are Implemented](#List-Implementation), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 10 Yanking Text Back

Whenever you cut text out of a buffer with a kill command in GNU Emacs,
you can bring it back with a yank command. The text that is cut out of
the buffer is put in the kill ring and the yank commands insert the
appropriate contents of the kill ring back into a buffer (not necessarily
the original buffer).

A simple `C-y` (`yank`) command inserts the first item from
the kill ring into the current buffer. If the `C-y` command is
followed immediately by `M-y`, the first element is replaced by
the second element. Successive `M-y` commands replace the second
element with the third, fourth, or fifth element, and so on. When the
last element in the kill ring is reached, it is replaced by the first
element and the cycle is repeated. (Thus the kill ring is called a
“ring” rather than just a “list”. However, the actual data structure
that holds the text is a list.
See [Handling the Kill Ring](#Kill-Ring), for the details of how the
list is handled as a ring.)

* [Kill Ring Overview](#Kill-Ring-Overview)
* [The `kill-ring-yank-pointer` Variable](#kill_002dring_002dyank_002dpointer)
* [Exercises with `yank` and `nthcdr`](#yank-nthcdr-Exercises)

---

Next: [The `kill-ring-yank-pointer` Variable](#kill_002dring_002dyank_002dpointer), Up: [Yanking Text Back](#Yanking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.1 Kill Ring Overview

The kill ring is a list of textual strings. This is what it looks like:

```
("some text" "a different piece of text" "yet more text")
```

If this were the contents of my kill ring and I pressed `C-y`, the
string of characters saying ‘`some text`’ would be inserted in this
buffer where my cursor is located.

The `yank` command is also used for duplicating text by copying it.
The copied text is not cut from the buffer, but a copy of it is put on the
kill ring and is inserted by yanking it back.

Three functions are used for bringing text back from the kill ring:
`yank`, which is usually bound to `C-y`; `yank-pop`,
which is usually bound to `M-y`; and `rotate-yank-pointer`,
which is used by the two other functions.

These functions refer to the kill ring through a variable called the
`kill-ring-yank-pointer`. Indeed, the insertion code for both the
`yank` and `yank-pop` functions is:

```
(insert (car kill-ring-yank-pointer))
```

(Well, no more. In GNU Emacs 22, the function has been replaced by
`insert-for-yank` which calls `insert-for-yank-1`
repetitively for each `yank-handler` segment. In turn,
`insert-for-yank-1` strips text properties from the inserted text
according to `yank-excluded-properties`. Otherwise, it is just
like `insert`. We will stick with plain `insert` since it
is easier to understand.)

To begin to understand how `yank` and `yank-pop` work, it is
first necessary to look at the `kill-ring-yank-pointer` variable.

---

Next: [Exercises with `yank` and `nthcdr`](#yank-nthcdr-Exercises), Previous: [Kill Ring Overview](#Kill-Ring-Overview), Up: [Yanking Text Back](#Yanking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.2 The `kill-ring-yank-pointer` Variable

`kill-ring-yank-pointer` is a variable, just as `kill-ring` is
a variable. It points to something by being bound to the value of what
it points to, like any other Lisp variable.

Thus, if the value of the kill ring is:

```
("some text" "a different piece of text" "yet more text")
```

and the `kill-ring-yank-pointer` points to the second clause, the
value of `kill-ring-yank-pointer` is:

```
("a different piece of text" "yet more text")
```

As explained in the previous chapter (see [How Lists are Implemented](#List-Implementation)), the
computer does not keep two different copies of the text being pointed to
by both the `kill-ring` and the `kill-ring-yank-pointer`. The
words “a different piece of text” and “yet more text” are not
duplicated. Instead, the two Lisp variables point to the same pieces of
text. Here is a diagram:

```
kill-ring     kill-ring-yank-pointer
    |               |
    |      ___ ___  |     ___ ___      ___ ___
     ---> |   |   |  --> |   |   |    |   |   |
          |___|___|----> |___|___|--> |___|___|--> nil
            |              |            |
            |              |            |
            |              |             --> "yet more text"
            |              |
            |               --> "a different piece of text"
            |
             --> "some text"
```

  
  
  

Both the variable `kill-ring` and the variable
`kill-ring-yank-pointer` are pointers. But the kill ring itself is
usually described as if it were actually what it is composed of. The
`kill-ring` is spoken of as if it were the list rather than that it
points to the list. Conversely, the `kill-ring-yank-pointer` is
spoken of as pointing to a list.

These two ways of talking about the same thing sound confusing at first but
make sense on reflection. The kill ring is generally thought of as the
complete structure of data that holds the information of what has recently
been cut out of the Emacs buffers. The `kill-ring-yank-pointer`
on the other hand, serves to indicate—that is, to point to—that part
of the kill ring of which the first element (the CAR) will be
inserted.

---

Previous: [The `kill-ring-yank-pointer` Variable](#kill_002dring_002dyank_002dpointer), Up: [Yanking Text Back](#Yanking)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.3 Exercises with `yank` and `nthcdr`

* Using `C-h v` (`describe-variable`), look at the value of
  your kill ring. Add several items to your kill ring; look at its
  value again. Using `M-y` (`yank-pop)`, move all the way
  around the kill ring. How many items were in your kill ring? Find
  the value of `kill-ring-max`. Was your kill ring full, or could
  you have kept more blocks of text within it?
* Using `nthcdr` and `car`, construct a series of expressions
  to return the first, second, third, and fourth elements of a list.

---

Next: [Regular Expression Searches](#Regexp-Search), Previous: [Yanking Text Back](#Yanking), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 11 Loops and Recursion

Emacs Lisp has two primary ways to cause an expression, or a series of
expressions, to be evaluated repeatedly: one uses a `while`
loop, and the other uses *recursion*.

Repetition can be very valuable. For example, to move forward four
sentences, you need only write a program that will move forward one
sentence and then repeat the process four times. Since a computer does
not get bored or tired, such repetitive action does not have the
deleterious effects that excessive or the wrong kinds of repetition can
have on humans.

People mostly write Emacs Lisp functions using `while` loops and
their kin; but you can use recursion, which provides a very powerful
way to think about and then to solve problems[14](#FOOT14).

* [`while`](#while)
* [Save your time: `dolist` and `dotimes`](#dolist-dotimes)
* [Recursion](#Recursion)
* [Looping Exercise](#Looping-exercise)

---

Next: [Save your time: `dolist` and `dotimes`](#dolist-dotimes), Up: [Loops and Recursion](#Loops-_0026-Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.1 `while`

The `while` special form tests whether the value returned by
evaluating its first argument is true or false. This is similar to what
the Lisp interpreter does with an `if`; what the interpreter does
next, however, is different.

In a `while` expression, if the value returned by evaluating the
first argument is false, the Lisp interpreter skips the rest of the
expression (the *body* of the expression) and does not evaluate it.
However, if the value is true, the Lisp interpreter evaluates the body
of the expression and then again tests whether the first argument to
`while` is true or false. If the value returned by evaluating the
first argument is again true, the Lisp interpreter again evaluates the
body of the expression.

The template for a `while` expression looks like this:

```
(while true-or-false-test
  body…)
```

* [Looping with `while`](#Looping-with-while)
* [A `while` Loop and a List](#Loop-Example)
* [An Example: `print-elements-of-list`](#print_002delements_002dof_002dlist)
* [A Loop with an Incrementing Counter](#Incrementing-Loop)
* [Details of an Incrementing Loop](#Incrementing-Loop-Details)
* [Loop with a Decrementing Counter](#Decrementing-Loop)

---

Next: [A `while` Loop and a List](#Loop-Example), Up: [`while`](#while)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Looping with `while`

So long as the true-or-false-test of the `while` expression
returns a true value when it is evaluated, the body is repeatedly
evaluated. This process is called a loop since the Lisp interpreter
repeats the same thing again and again, like an airplane doing a loop.
When the result of evaluating the true-or-false-test is false, the
Lisp interpreter does not evaluate the rest of the `while`
expression and exits the loop.

Clearly, if the value returned by evaluating the first argument to
`while` is always true, the body following will be evaluated
again and again … and again … forever. Conversely, if the
value returned is never true, the expressions in the body will never
be evaluated. The craft of writing a `while` loop consists of
choosing a mechanism such that the true-or-false-test returns true
just the number of times that you want the subsequent expressions to
be evaluated, and then have the test return false.

The value returned by evaluating a `while` is the value of the
true-or-false-test. An interesting consequence of this is that a
`while` loop that evaluates without error will return `nil`
or false regardless of whether it has looped 1 or 100 times or none at
all. A `while` expression that evaluates successfully never
returns a true value! What this means is that `while` is always
evaluated for its side effects, which is to say, the consequences of
evaluating the expressions within the body of the `while` loop.
This makes sense. It is not the mere act of looping that is desired,
but the consequences of what happens when the expressions in the loop
are repeatedly evaluated.

---

Next: [An Example: `print-elements-of-list`](#print_002delements_002dof_002dlist), Previous: [Looping with `while`](#Looping-with-while), Up: [`while`](#while)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.1.1 A `while` Loop and a List

A common way to control a `while` loop is to test whether a list
has any elements. If it does, the loop is repeated; but if it does not,
the repetition is ended. Since this is an important technique, we will
create a short example to illustrate it.

A simple way to test whether a list has elements is to evaluate the
list: if it has no elements, it is an empty list and will return the
empty list, `()`, which is a synonym for `nil` or false. On
the other hand, a list with elements will return those elements when it
is evaluated. Since Emacs Lisp considers as true any value that is not
`nil`, a list that returns elements will test true in a
`while` loop.

For example, you can set the variable `empty-list` to `nil` by
evaluating the following `setq` expression:

```
(setq empty-list ())
```

After evaluating the `setq` expression, you can evaluate the
variable `empty-list` in the usual way, by placing the cursor after
the symbol and typing `C-x C-e`; `nil` will appear in your
echo area:

```
empty-list
```

On the other hand, if you set a variable to be a list with elements, the
list will appear when you evaluate the variable, as you can see by
evaluating the following two expressions:

```
(setq animals '(gazelle giraffe lion tiger))

animals
```

Thus, to create a `while` loop that tests whether there are any
items in the list `animals`, the first part of the loop will be
written like this:

```
(while animals
       …
```

When the `while` tests its first argument, the variable
`animals` is evaluated. It returns a list. So long as the list
has elements, the `while` considers the results of the test to be
true; but when the list is empty, it considers the results of the test
to be false.

To prevent the `while` loop from running forever, some mechanism
needs to be provided to empty the list eventually. An oft-used
technique is to have one of the subsequent forms in the `while`
expression set the value of the list to be the CDR of the list.
Each time the `cdr` function is evaluated, the list will be made
shorter, until eventually only the empty list will be left. At this
point, the test of the `while` loop will return false, and the
arguments to the `while` will no longer be evaluated.

For example, the list of animals bound to the variable `animals`
can be set to be the CDR of the original list with the
following expression:

```
(setq animals (cdr animals))
```

If you have evaluated the previous expressions and then evaluate this
expression, you will see `(giraffe lion tiger)` appear in the echo
area. If you evaluate the expression again, `(lion tiger)` will
appear in the echo area. If you evaluate it again and yet again,
`(tiger)` appears and then the empty list, shown by `nil`.

A template for a `while` loop that uses the `cdr` function
repeatedly to cause the true-or-false-test eventually to test false
looks like this:

```
(while test-whether-list-is-empty
  body…
  set-list-to-cdr-of-list)
```

This test and use of `cdr` can be put together in a function that
goes through a list and prints each element of the list on a line of its
own.

---

Next: [A Loop with an Incrementing Counter](#Incrementing-Loop), Previous: [A `while` Loop and a List](#Loop-Example), Up: [`while`](#while)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.1.2 An Example: `print-elements-of-list`

The `print-elements-of-list` function illustrates a `while`
loop with a list.

The function requires several lines for its output. If you are
reading this in a recent instance of GNU Emacs, you can evaluate the
following expression inside of Info, as usual.

If you are using an earlier version of Emacs, you need to copy the
necessary expressions to your `*scratch*` buffer and evaluate
them there. This is because the echo area had only one line in the
earlier versions.

You can copy the expressions by marking the beginning of the region
with `C-SPC` (`set-mark-command`), moving the cursor to
the end of the region and then copying the region using `M-w`
(`kill-ring-save`, which calls `copy-region-as-kill` and
then provides visual feedback). In the `*scratch*`
buffer, you can yank the expressions back by typing `C-y`
(`yank`).

After you have copied the expressions to the `*scratch*` buffer,
evaluate each expression in turn. Be sure to evaluate the last
expression, `(print-elements-of-list animals)`, by typing
`C-u C-x C-e`, that is, by giving an argument to
`eval-last-sexp`. This will cause the result of the evaluation
to be printed in the `*scratch*` buffer instead of being printed
in the echo area. (Otherwise you will see something like this in your
echo area: `^Jgazelle^J^Jgiraffe^J^Jlion^J^Jtiger^Jnil`, in which
each ‘`^J`’ stands for a newline.)

You can evaluate these expressions directly in the Info buffer, and
the echo area will grow to show the results.

```
(setq animals '(gazelle giraffe lion tiger))

(defun print-elements-of-list (list)
  "Print each element of LIST on a line of its own."
  (while list
    (print (car list))
    (setq list (cdr list))))

(print-elements-of-list animals)
```

When you evaluate the three expressions in sequence, you will see
this:

```
gazelle

giraffe

lion

tiger
nil
```

Each element of the list is printed on a line of its own (that is what
the function `print` does) and then the value returned by the
function is printed. Since the last expression in the function is the
`while` loop, and since `while` loops always return
`nil`, a `nil` is printed after the last element of the list.

---

Next: [Details of an Incrementing Loop](#Incrementing-Loop-Details), Previous: [An Example: `print-elements-of-list`](#print_002delements_002dof_002dlist), Up: [`while`](#while)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.1.3 A Loop with an Incrementing Counter

A loop is not useful unless it stops when it ought. Besides
controlling a loop with a list, a common way of stopping a loop is to
write the first argument as a test that returns false when the correct
number of repetitions are complete. This means that the loop must
have a counter—an expression that counts how many times the loop
repeats itself.

---

Next: [Loop with a Decrementing Counter](#Decrementing-Loop), Previous: [A Loop with an Incrementing Counter](#Incrementing-Loop), Up: [`while`](#while)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Details of an Incrementing Loop

The test for a loop with an incrementing counter can be an expression
such as `(< count desired-number)` which returns `t` for
true if the value of `count` is less than the
`desired-number` of repetitions and `nil` for false if the
value of `count` is equal to or is greater than the
`desired-number`. The expression that increments the count can
be a simple `setq` such as `(setq count (1+ count))`, where
`1+` is a built-in function in Emacs Lisp that adds 1 to its
argument. (The expression `(1+ count)` has the same result
as `(+ count 1)`, but is easier for a human to read.)

The template for a `while` loop controlled by an incrementing
counter looks like this:

```
set-count-to-initial-value
(while (< count desired-number)         ; true-or-false-test
  body…
  (setq count (1+ count)))              ; incrementer
```

Note that you need to set the initial value of `count`; usually it
is set to 1.

* [Example with incrementing counter](#Incrementing-Example)
* [The parts of the function definition](#Inc-Example-parts)
* [Putting the function definition together](#Inc-Example-altogether)

---

Next: [The parts of the function definition](#Inc-Example-parts), Up: [Details of an Incrementing Loop](#Incrementing-Loop-Details)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Example with incrementing counter

Suppose you are playing on the beach and decide to make a triangle of
pebbles, putting one pebble in the first row, two in the second row,
three in the third row and so on, like this:

  

```
               *
              * *
             * * *
            * * * *
```

  

(About 2500 years ago, Pythagoras and others developed the beginnings of
number theory by considering questions such as this.)

Suppose you want to know how many pebbles you will need to make a
triangle with 7 rows?

Clearly, what you need to do is add up the numbers from 1 to 7. There
are two ways to do this; start with the smallest number, one, and add up
the list in sequence, 1, 2, 3, 4 and so on; or start with the largest
number and add the list going down: 7, 6, 5, 4 and so on. Because both
mechanisms illustrate common ways of writing `while` loops, we will
create two examples, one counting up and the other counting down. In
this first example, we will start with 1 and add 2, 3, 4 and so on.

If you are just adding up a short list of numbers, the easiest way to do
it is to add up all the numbers at once. However, if you do not know
ahead of time how many numbers your list will have, or if you want to be
prepared for a very long list, then you need to design your addition so
that what you do is repeat a simple process many times instead of doing
a more complex process once.

For example, instead of adding up all the pebbles all at once, what you
can do is add the number of pebbles in the first row, 1, to the number
in the second row, 2, and then add the total of those two rows to the
third row, 3. Then you can add the number in the fourth row, 4, to the
total of the first three rows; and so on.

The critical characteristic of the process is that each repetitive
action is simple. In this case, at each step we add only two numbers,
the number of pebbles in the row and the total already found. This
process of adding two numbers is repeated again and again until the last
row has been added to the total of all the preceding rows. In a more
complex loop the repetitive action might not be so simple, but it will
be simpler than doing everything all at once.

---

Next: [Putting the function definition together](#Inc-Example-altogether), Previous: [Example with incrementing counter](#Incrementing-Example), Up: [Details of an Incrementing Loop](#Incrementing-Loop-Details)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The parts of the function definition

The preceding analysis gives us the bones of our function definition:
first, we will need a variable that we can call `total` that will
be the total number of pebbles. This will be the value returned by
the function.

Second, we know that the function will require an argument: this
argument will be the total number of rows in the triangle. It can be
called `number-of-rows`.

Finally, we need a variable to use as a counter. We could call this
variable `counter`, but a better name is `row-number`. That
is because what the counter does in this function is count rows, and a
program should be written to be as understandable as possible.

When the Lisp interpreter first starts evaluating the expressions in the
function, the value of `total` should be set to zero, since we have
not added anything to it. Then the function should add the number of
pebbles in the first row to the total, and then add the number of
pebbles in the second to the total, and then add the number of
pebbles in the third row to the total, and so on, until there are no
more rows left to add.

Both `total` and `row-number` are used only inside the
function, so they can be declared as local variables with `let`
and given initial values. Clearly, the initial value for `total`
should be 0. The initial value of `row-number` should be 1,
since we start with the first row. This means that the `let`
statement will look like this:

```
  (let ((total 0)
        (row-number 1))
    body…)
```

After the internal variables are declared and bound to their initial
values, we can begin the `while` loop. The expression that serves
as the test should return a value of `t` for true so long as the
`row-number` is less than or equal to the `number-of-rows`.
(If the expression tests true only so long as the row number is less
than the number of rows in the triangle, the last row will never be
added to the total; hence the row number has to be either less than or
equal to the number of rows.)

Lisp provides the `<=` function that returns true if the value of
its first argument is less than or equal to the value of its second
argument and false otherwise. So the expression that the `while`
will evaluate as its test should look like this:

```
(<= row-number number-of-rows)
```

The total number of pebbles can be found by repeatedly adding the number
of pebbles in a row to the total already found. Since the number of
pebbles in the row is equal to the row number, the total can be found by
adding the row number to the total. (Clearly, in a more complex
situation, the number of pebbles in the row might be related to the row
number in a more complicated way; if this were the case, the row number
would be replaced by the appropriate expression.)

```
(setq total (+ total row-number))
```

What this does is set the new value of `total` to be equal to the
sum of adding the number of pebbles in the row to the previous total.

After setting the value of `total`, the conditions need to be
established for the next repetition of the loop, if there is one. This
is done by incrementing the value of the `row-number` variable,
which serves as a counter. After the `row-number` variable has
been incremented, the true-or-false-test at the beginning of the
`while` loop tests whether its value is still less than or equal to
the value of the `number-of-rows` and if it is, adds the new value
of the `row-number` variable to the `total` of the previous
repetition of the loop.

The built-in Emacs Lisp function `1+` adds 1 to a number, so the
`row-number` variable can be incremented with this expression:

```
(setq row-number (1+ row-number))
```

---

Previous: [The parts of the function definition](#Inc-Example-parts), Up: [Details of an Incrementing Loop](#Incrementing-Loop-Details)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Putting the function definition together

We have created the parts for the function definition; now we need to
put them together.

First, the contents of the `while` expression:

```
(while (<= row-number number-of-rows)   ; true-or-false-test
  (setq total (+ total row-number))
  (setq row-number (1+ row-number)))    ; incrementer
```

Along with the `let` expression varlist, this very nearly
completes the body of the function definition. However, it requires
one final element, the need for which is somewhat subtle.

The final touch is to place the variable `total` on a line by
itself after the `while` expression. Otherwise, the value returned
by the whole function is the value of the last expression that is
evaluated in the body of the `let`, and this is the value
returned by the `while`, which is always `nil`.

This may not be evident at first sight. It almost looks as if the
incrementing expression is the last expression of the whole function.
But that expression is part of the body of the `while`; it is the
last element of the list that starts with the symbol `while`.
Moreover, the whole of the `while` loop is a list within the body
of the `let`.

In outline, the function will look like this:

```
(defun name-of-function (argument-list)
  "documentation…"
  (let (varlist)
    (while (true-or-false-test)
      body-of-while… )
    … ))                    ; Need final expression here.
```

The result of evaluating the `let` is what is going to be returned
by the `defun` since the `let` is not embedded within any
containing list, except for the `defun` as a whole. However, if
the `while` is the last element of the `let` expression, the
function will always return `nil`. This is not what we want!
Instead, what we want is the value of the variable `total`. This
is returned by simply placing the symbol as the last element of the list
starting with `let`. It gets evaluated after the preceding
elements of the list are evaluated, which means it gets evaluated after
it has been assigned the correct value for the total.

It may be easier to see this by printing the list starting with
`let` all on one line. This format makes it evident that the
varlist and `while` expressions are the second and third
elements of the list starting with `let`, and the `total` is
the last element:

```
(let (varlist) (while (true-or-false-test) body-of-while… ) total)
```

Putting everything together, the `triangle` function definition
looks like this:

```
(defun triangle (number-of-rows)    ; Version with
                                    ;   incrementing counter.
  "Add up the number of pebbles in a triangle.
The first row has one pebble, the second row two pebbles,
the third row three pebbles, and so on.
The argument is NUMBER-OF-ROWS."
```

```
  (let ((total 0)
        (row-number 1))
    (while (<= row-number number-of-rows)
      (setq total (+ total row-number))
      (setq row-number (1+ row-number)))
    total))
```

After you have installed `triangle` by evaluating the function, you
can try it out. Here are two examples:

```
(triangle 4)

(triangle 7)
```

The sum of the first four numbers is 10 and the sum of the first seven
numbers is 28.

---

Previous: [Details of an Incrementing Loop](#Incrementing-Loop-Details), Up: [`while`](#while)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 11.1.4 Loop with a Decrementing Counter

Another common way to write a `while` loop is to write the test
so that it determines whether a counter is greater than zero. So long
as the counter is greater than zero, the loop is repeated. But when
the counter is equal to or less than zero, the loop is stopped. For
this to work, the counter has to start out greater than zero and then
be made smaller and smaller by a form that is evaluated
repeatedly.

The test will be an expression such as `(> counter 0)` which
returns `t` for true if the value of `counter` is greater
than zero, and `nil` for false if the value of `counter` is
equal to or less than zero. The expression that makes the number
smaller and smaller can be a simple `setq` such as `(setq
counter (1- counter))`, where `1-` is a built-in function in
Emacs Lisp that subtracts 1 from its argument.

The template for a decrementing `while` loop looks like this:

```
(while (> counter 0)                    ; true-or-false-test
  body…
  (setq counter (1- counter)))          ; decrementer
```

* [Example with decrementing counter](#Decrementing-Example)
* [The parts of the function definition](#Dec-Example-parts)
* [Putting the function definition together](#Dec-Example-altogether)

---

Next: [The parts of the function definition](#Dec-Example-parts), Up: [Loop with a Decrementing Counter](#Decrementing-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Example with decrementing counter

To illustrate a loop with a decrementing counter, we will rewrite the
`triangle` function so the counter decreases to zero.

This is the reverse of the earlier version of the function. In this
case, to find out how many pebbles are needed to make a triangle with
3 rows, add the number of pebbles in the third row, 3, to the number
in the preceding row, 2, and then add the total of those two rows to
the row that precedes them, which is 1.

Likewise, to find the number of pebbles in a triangle with 7 rows, add
the number of pebbles in the seventh row, 7, to the number in the
preceding row, which is 6, and then add the total of those two rows to
the row that precedes them, which is 5, and so on. As in the previous
example, each addition only involves adding two numbers, the total of
the rows already added up and the number of pebbles in the row that is
being added to the total. This process of adding two numbers is
repeated again and again until there are no more pebbles to add.

We know how many pebbles to start with: the number of pebbles in the
last row is equal to the number of rows. If the triangle has seven
rows, the number of pebbles in the last row is 7. Likewise, we know how
many pebbles are in the preceding row: it is one less than the number in
the row.

---

Next: [Putting the function definition together](#Dec-Example-altogether), Previous: [Example with decrementing counter](#Decrementing-Example), Up: [Loop with a Decrementing Counter](#Decrementing-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The parts of the function definition

We start with three variables: the total number of rows in the
triangle; the number of pebbles in a row; and the total number of
pebbles, which is what we want to calculate. These variables can be
named `number-of-rows`, `number-of-pebbles-in-row`, and
`total`, respectively.

Both `total` and `number-of-pebbles-in-row` are used only
inside the function and are declared with `let`. The initial
value of `total` should, of course, be zero. However, the
initial value of `number-of-pebbles-in-row` should be equal to
the number of rows in the triangle, since the addition will start with
the longest row.

This means that the beginning of the `let` expression will look
like this:

```
(let ((total 0)
      (number-of-pebbles-in-row number-of-rows))
  body…)
```

The total number of pebbles can be found by repeatedly adding the number
of pebbles in a row to the total already found, that is, by repeatedly
evaluating the following expression:

```
(setq total (+ total number-of-pebbles-in-row))
```

After the `number-of-pebbles-in-row` is added to the `total`,
the `number-of-pebbles-in-row` should be decremented by one, since
the next time the loop repeats, the preceding row will be
added to the total.

The number of pebbles in a preceding row is one less than the number of
pebbles in a row, so the built-in Emacs Lisp function `1-` can be
used to compute the number of pebbles in the preceding row. This can be
done with the following expression:

```
(setq number-of-pebbles-in-row
      (1- number-of-pebbles-in-row))
```

Finally, we know that the `while` loop should stop making repeated
additions when there are no pebbles in a row. So the test for
the `while` loop is simply:

```
(while (> number-of-pebbles-in-row 0)
```

---

Previous: [The parts of the function definition](#Dec-Example-parts), Up: [Loop with a Decrementing Counter](#Decrementing-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Putting the function definition together

We can put these expressions together to create a function definition
that works. However, on examination, we find that one of the local
variables is unneeded!

The function definition looks like this:

```
;;; First subtractive version.
(defun triangle (number-of-rows)
  "Add up the number of pebbles in a triangle."
  (let ((total 0)
        (number-of-pebbles-in-row number-of-rows))
    (while (> number-of-pebbles-in-row 0)
      (setq total (+ total number-of-pebbles-in-row))
      (setq number-of-pebbles-in-row
            (1- number-of-pebbles-in-row)))
    total))
```

As written, this function works.

However, we do not need `number-of-pebbles-in-row`.

When the `triangle` function is evaluated, the symbol
`number-of-rows` will be bound to a number, giving it an initial
value. That number can be changed in the body of the function as if
it were a local variable, without any fear that such a change will
effect the value of the variable outside of the function. This is a
very useful characteristic of Lisp; it means that the variable
`number-of-rows` can be used anywhere in the function where
`number-of-pebbles-in-row` is used.

Here is a second version of the function written a bit more cleanly:

```
(defun triangle (number)                ; Second version.
  "Return sum of numbers 1 through NUMBER inclusive."
  (let ((total 0))
    (while (> number 0)
      (setq total (+ total number))
      (setq number (1- number)))
    total))
```

In brief, a properly written `while` loop will consist of three parts:

1. A test that will return false after the loop has repeated itself the
   correct number of times.
2. An expression the evaluation of which will return the value desired
   after being repeatedly evaluated.
3. An expression to change the value passed to the true-or-false-test so
   that the test returns false after the loop has repeated itself the right
   number of times.

---

Next: [Recursion](#Recursion), Previous: [`while`](#while), Up: [Loops and Recursion](#Loops-_0026-Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 11.2 Save your time: `dolist` and `dotimes`

In addition to `while`, both `dolist` and `dotimes`
provide for looping. Sometimes these are quicker to write than the
equivalent `while` loop. Both are Lisp macros. (See [Macros](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Macros) in The GNU Emacs Lisp Reference Manual. )

`dolist` works like a `while` loop that CDRs down a
list: `dolist` automatically shortens the list each time it
loops—takes the CDR of the list—and binds the CAR of
each shorter version of the list to the first of its arguments.

`dotimes` loops a specific number of times: you specify the number.

* [The `dolist` Macro](#dolist)
* [The `dotimes` Macro](#dotimes)

---

Next: [The `dotimes` Macro](#dotimes), Up: [Save your time: `dolist` and `dotimes`](#dolist-dotimes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The `dolist` Macro

Suppose, for example, you want to reverse a list, so that
“first” “second” “third” becomes “third” “second” “first”.

In practice, you would use the `reverse` function, like this:

```
(setq animals '(gazelle giraffe lion tiger))

(reverse animals)
```

Here is how you could reverse the list using a `while` loop:

```
(setq animals '(gazelle giraffe lion tiger))

(defun reverse-list-with-while (list)
  "Using while, reverse the order of LIST."
  (let (value)  ; make sure list starts empty
    (while list
      (setq value (cons (car list) value))
      (setq list (cdr list)))
    value))

(reverse-list-with-while animals)
```

And here is how you could use the `dolist` macro:

```
(setq animals '(gazelle giraffe lion tiger))

(defun reverse-list-with-dolist (list)
  "Using dolist, reverse the order of LIST."
  (let (value)  ; make sure list starts empty
    (dolist (element list value)
      (setq value (cons element value)))))

(reverse-list-with-dolist animals)
```

In Info, you can place your cursor after the closing parenthesis of
each expression and type `C-x C-e`; in each case, you should see

```
(tiger lion giraffe gazelle)
```

in the echo area.

For this example, the existing `reverse` function is obviously best.
The `while` loop is just like our first example (see [A `while` Loop and a List](#Loop-Example)). The `while` first
checks whether the list has elements; if so, it constructs a new list
by adding the first element of the list to the existing list (which in
the first iteration of the loop is `nil`). Since the second
element is prepended in front of the first element, and the third
element is prepended in front of the second element, the list is reversed.

In the expression using a `while` loop,
the `(setq list (cdr list))`
expression shortens the list, so the `while` loop eventually
stops. In addition, it provides the `cons` expression with a new
first element by creating a new and shorter list at each repetition of
the loop.

The `dolist` expression does very much the same as the
`while` expression, except that the `dolist` macro does some
of the work you have to do when writing a `while` expression.

Like a `while` loop, a `dolist` loops. What is different is
that it automatically shortens the list each time it loops—it
CDRs down the list on its own—and it automatically binds
the CAR of each shorter version of the list to the first of its
arguments.

In the example, the CAR of each shorter version of the list is
referred to using the symbol ‘`element`’, the list itself is called
‘`list`’, and the value returned is called ‘`value`’. The
remainder of the `dolist` expression is the body.

The `dolist` expression binds the CAR of each shorter
version of the list to `element` and then evaluates the body of
the expression; and repeats the loop. The result is returned in
`value`.

---

Previous: [The `dolist` Macro](#dolist), Up: [Save your time: `dolist` and `dotimes`](#dolist-dotimes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The `dotimes` Macro

The `dotimes` macro is similar to `dolist`, except that it
loops a specific number of times.

The first argument to `dotimes` is assigned the numbers 0, 1, 2
and so forth each time around the loop. You need to provide the value
of the second argument, which is how many times the macro loops.

For example, the following binds the numbers from 0 up to, but not
including, the number 3 to the first argument, number, and then
constructs a list of the three numbers. (The first number is 0, the
second number is 1, and the third number is 2; this makes a total of
three numbers in all, starting with zero as the first number.)

```
(let (value)      ; otherwise a value is a void variable
  (dotimes (number 3)
    (setq value (cons number value)))
  value)

⇒ (2 1 0)
```

The way to use `dotimes` is to operate on some expression
number number of times and then return the result, either as
a list or an atom.

Here is an example of a `defun` that uses `dotimes` to add
up the number of pebbles in a triangle.

```
(defun triangle-using-dotimes (number-of-rows)
  "Using `dotimes', add up the number of pebbles in a triangle."
(let ((total 0))  ; otherwise a total is a void variable
  (dotimes (number number-of-rows)
    (setq total (+ total (1+ number))))
  total))

(triangle-using-dotimes 4)
```

---

Next: [Looping Exercise](#Looping-exercise), Previous: [Save your time: `dolist` and `dotimes`](#dolist-dotimes), Up: [Loops and Recursion](#Loops-_0026-Recursion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
