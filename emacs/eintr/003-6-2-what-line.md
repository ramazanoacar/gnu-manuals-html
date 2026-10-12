### 6.2 `what-line`

The `what-line` command tells you the number of the line in which
the cursor is located. The function illustrates the use of the
`save-restriction` and `save-excursion` commands. Here is the
original text of the function:

```
(defun what-line ()
  "Print the current line number (in the buffer) of point."
  (interactive)
  (save-restriction
    (widen)
    (save-excursion
      (beginning-of-line)
      (message "Line %d"
               (1+ (count-lines 1 (point)))))))
```

(In modern versions of GNU Emacs, the `what-line` function has
been expanded to tell you your line number in a narrowed buffer as
well as your line number in a widened buffer. The modern version is
more complex than the version shown here. If you feel adventurous,
you might want to look at it after figuring out how this version
works. You will probably need to use `C-h f`
(`describe-function`). The newer version uses a conditional to
determine whether the buffer has been narrowed.

Also, the modern version of `what-line` uses
`line-number-at-pos`, which among other simple expressions, such
as `(goto-char (point-min))`, moves point to the beginning of the
current line with `(forward-line 0)` rather than
`beginning-of-line`.)

The `what-line` function as shown here has a documentation line
and is interactive, as you would expect. The next two lines use the
functions `save-restriction` and `widen`.

The `save-restriction` special form notes whatever narrowing is in
effect, if any, in the current buffer and restores that narrowing after
the code in the body of the `save-restriction` has been evaluated.

The `save-restriction` special form is followed by `widen`.
This function undoes any narrowing the current buffer may have had
when `what-line` was called. (The narrowing that was there is
the narrowing that `save-restriction` remembers.) This widening
makes it possible for the line counting commands to count from the
beginning of the buffer. Otherwise, they would have been limited to
counting within the accessible region. Any original narrowing is
restored just before the completion of the function by the
`save-restriction` special form.

The call to `widen` is followed by `save-excursion`, which
saves the location of the cursor (i.e., of point), and
restores it after the code in the body of the `save-excursion`
uses the `beginning-of-line` function to move point.

(Note that the `(widen)` expression comes between the
`save-restriction` and `save-excursion` special forms. When
you write the two `save- …` expressions in sequence, write
`save-excursion` outermost.)

The last two lines of the `what-line` function are functions to
count the number of lines in the buffer and then print the number in the
echo area.

```
(message "Line %d"
         (1+ (count-lines 1 (point)))))))
```

The `message` function prints a one-line message at the bottom of
the Emacs screen. The first argument is inside of quotation marks and
is printed as a string of characters. However, it may contain a
‘`%d`’ expression to print a following argument. ‘`%d`’ prints
the argument as a decimal, so the message will say something such as
‘`Line 243`’.

The number that is printed in place of the ‘`%d`’ is computed by the
last line of the function:

```
(1+ (count-lines 1 (point)))
```

What this does is count the lines from the first position of the
buffer, indicated by the `1`, up to `(point)`, and then add
one to that number. (The `1+` function adds one to its
argument.) We add one to it because line 2 has only one line before
it, and `count-lines` counts only the lines *before* the
current line.

After `count-lines` has done its job, and the message has been
printed in the echo area, the `save-excursion` restores point to
its original position; and `save-restriction` restores
the original narrowing, if any.

---

Previous: [`what-line`](#what_002dline), Up: [Narrowing and Widening](#Narrowing-_0026-Widening)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.3 Exercise with Narrowing

Write a function that will display the first 60 characters of the
current buffer, even if you have narrowed the buffer to its latter
half so that the first line is inaccessible. Restore point, mark, and
narrowing. For this exercise, you need to use a whole potpourri of
functions, including `save-restriction`, `widen`,
`goto-char`, `point-min`, `message`, and
`buffer-substring`.

(`buffer-substring` is a previously unmentioned function you will
have to investigate yourself; or perhaps you will have to use
`buffer-substring-no-properties` or
`filter-buffer-substring` …, yet other functions. Text
properties are a feature otherwise not discussed here. See [Text Properties](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Text-Properties) in The GNU Emacs Lisp Reference
Manual.)

Additionally, do you really need `goto-char` or `point-min`?
Or can you write the function without them?

---

Next: [Cutting and Storing Text](#Cutting-_0026-Storing-Text), Previous: [Narrowing and Widening](#Narrowing-_0026-Widening), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 7 `car`, `cdr`, `cons`: Fundamental Functions

In Lisp, `car`, `cdr`, and `cons` are fundamental
functions. The `cons` function is used to construct lists, and
the `car` and `cdr` functions are used to take them apart.

In the walk through of the `copy-region-as-kill` function, we
will see `cons` as well as two variants on `cdr`,
namely, `setcdr` and `nthcdr`. (See [`copy-region-as-kill`](#copy_002dregion_002das_002dkill).)

* [Strange Names](#Strange-Names)
* [`car` and `cdr`](#car-_0026-cdr)
* [`cons`](#cons)
* [`nthcdr`](#nthcdr)
* [`nth`](#nth)
* [`setcar`](#setcar)
* [`setcdr`](#setcdr)
* [Exercise](#cons-Exercise)

---

Next: [`car` and `cdr`](#car-_0026-cdr), Up: [`car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Strange Names

The name of the `cons` function is not unreasonable: it is an
abbreviation of the word “construct”. The origins of the names for
`car` and `cdr`, on the other hand, are esoteric: `car`
is an acronym from the phrase “Contents of the Address part of the
Register”; and `cdr` (pronounced “could-er”) is an acronym
from the phrase “Contents of the Decrement part of the Register”.
These phrases refer to the IBM 704 computer on which the original Lisp
was developed.

The IBM 704 is a footnote in history, but these names are now beloved
traditions of Lisp.

---

Next: [`cons`](#cons), Previous: [Strange Names](#Strange-Names), Up: [`car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.1 `car` and `cdr`

The CAR of a list is, quite simply, the first item in the list.
Thus the CAR of the list `(rose violet daisy buttercup)` is
`rose`.

If you are reading this in Info in GNU Emacs, you can see this by
evaluating the following:

```
(car '(rose violet daisy buttercup))
```

After evaluating the expression, `rose` will appear in the echo
area.

`car` does not remove the first item from the list; it only reports
what it is. After `car` has been applied to a list, the list is
still the same as it was. In the jargon, `car` is
“non-destructive”. This feature turns out to be important.

The CDR of a list is the rest of the list, that is, the
`cdr` function returns the part of the list that follows the
first item. Thus, while the CAR of the list `'(rose violet
daisy buttercup)` is `rose`, the rest of the list, the value
returned by the `cdr` function, is `(violet daisy
buttercup)`.

You can see this by evaluating the following in the usual way:

```
(cdr '(rose violet daisy buttercup))
```

When you evaluate this, `(violet daisy buttercup)` will appear in
the echo area.

Like `car`, `cdr` does not remove any elements from the
list—it just returns a report of what the second and subsequent
elements are.

Incidentally, in the example, the list of flowers is quoted. If it were
not, the Lisp interpreter would try to evaluate the list by calling
`rose` as a function. In this example, we do not want to do that.

For operating on lists, the names `first` and `rest` would
make more sense than the names `car` and `cdr`. Indeed,
some programmers define `first` and `rest` as aliases for
`car` and `cdr`, then write `first` and `rest` in
their code.

However, lists in Lisp are built using a lower-level structure known
as “cons cells” (see [How Lists are Implemented](#List-Implementation)), in which there is no
such thing as “first” or “rest”, and the CAR and the CDR
are symmetrical. Lisp does not try to hide the existence of cons
cells, and programs do use them for things other than lists. For this
reason, the names are helpful for reminding programmers that
`car` and `cdr` are in fact symmetrical, despite the
asymmetrical way they are used in lists.

When `car` and `cdr` are applied to a list made up of symbols,
such as the list `(pine fir oak maple)`, the element of the list
returned by the function `car` is the symbol `pine` without
any parentheses around it. `pine` is the first element in the
list. However, the CDR of the list is a list itself, `(fir
oak maple)`, as you can see by evaluating the following expressions in
the usual way:

```
(car '(pine fir oak maple))

(cdr '(pine fir oak maple))
```

On the other hand, in a list of lists, the first element is itself a
list. `car` returns this first element as a list. For example,
the following list contains three sub-lists, a list of carnivores, a
list of herbivores and a list of sea mammals:

```
(car '((lion tiger cheetah)
       (gazelle antelope zebra)
       (whale dolphin seal)))
```

In this example, the first element or CAR of the list is the list of
carnivores, `(lion tiger cheetah)`, and the rest of the list is
`((gazelle antelope zebra) (whale dolphin seal))`.

```
(cdr '((lion tiger cheetah)
       (gazelle antelope zebra)
       (whale dolphin seal)))
```

It is worth saying again that `car` and `cdr` are
non-destructive—that is, they do not modify or change lists to which
they are applied. This is very important for how they are used.

Also, in the first chapter, in the discussion about atoms, I said that
in Lisp, certain kinds of atom, such as an array, can be separated
into parts; but the mechanism for doing this is different from the
mechanism for splitting a list. As far as Lisp is concerned, the
atoms of a list are unsplittable. (See [Lisp Atoms](#Lisp-Atoms).) The
`car` and `cdr` functions are used for splitting lists and
are considered fundamental to Lisp. Since they cannot split or gain
access to the parts of an array, an array is considered an atom.
Conversely, the other fundamental function, `cons`, can put
together or construct a list, but not an array. (Arrays are handled
by array-specific functions. See [Arrays](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Arrays) in The GNU
Emacs Lisp Reference Manual.)

---

Next: [`nthcdr`](#nthcdr), Previous: [`car` and `cdr`](#car-_0026-cdr), Up: [`car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.2 `cons`

The `cons` function constructs lists; it is the inverse of
`car` and `cdr`. For example, `cons` can be used to make
a four element list from the three element list, `(fir oak maple)`:

```
(cons 'pine '(fir oak maple))
```

After evaluating this list, you will see

```
(pine fir oak maple)
```

appear in the echo area. `cons` causes the creation of a new
list in which the element is followed by the elements of the original
list.

We often say that `cons` puts a new element at the beginning of
a list, or that it attaches or pushes elements onto the list, but this
phrasing can be misleading, since `cons` does not change an
existing list, but creates a new one.

Like `car` and `cdr`, `cons` is non-destructive.

* [Build a list](#Build-a-list)
* [Find the Length of a List: `length`](#length)

---

Next: [Find the Length of a List: `length`](#length), Up: [`cons`](#cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Build a list

`cons` must have a list to attach to.[13](#FOOT13) You
cannot start from absolutely nothing. If you are building a list, you
need to provide at least an empty list at the beginning. Here is a
series of `cons` expressions that build up a list of flowers. If
you are reading this in Info in GNU Emacs, you can evaluate each of
the expressions in the usual way; the value is printed in this text
after ‘`⇒`’, which you may read as “evaluates to”.

```
(cons 'buttercup ())
     ⇒ (buttercup)
```

```

```

```
(cons 'daisy '(buttercup))
     ⇒ (daisy buttercup)
```

```

```

```
(cons 'violet '(daisy buttercup))
     ⇒ (violet daisy buttercup)
```

```

```

```
(cons 'rose '(violet daisy buttercup))
     ⇒ (rose violet daisy buttercup)
```

In the first example, the empty list is shown as `()` and a list
made up of `buttercup` followed by the empty list is constructed.
As you can see, the empty list is not shown in the list that was
constructed. All that you see is `(buttercup)`. The empty list is
not counted as an element of a list because there is nothing in an empty
list. Generally speaking, an empty list is invisible.

The second example, `(cons 'daisy '(buttercup))` constructs a new,
two element list by putting `daisy` in front of `buttercup`;
and the third example constructs a three element list by putting
`violet` in front of `daisy` and `buttercup`.

---

Previous: [Build a list](#Build-a-list), Up: [`cons`](#cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.2.1 Find the Length of a List: `length`

You can find out how many elements there are in a list by using the Lisp
function `length`, as in the following examples:

```
(length '(buttercup))
     ⇒ 1
```

```

```

```
(length '(daisy buttercup))
     ⇒ 2
```

```

```

```
(length (cons 'violet '(daisy buttercup)))
     ⇒ 3
```

In the third example, the `cons` function is used to construct a
three element list which is then passed to the `length` function as
its argument.

We can also use `length` to count the number of elements in an
empty list:

```
(length ())
     ⇒ 0
```

As you would expect, the number of elements in an empty list is zero.

An interesting experiment is to find out what happens if you try to find
the length of no list at all; that is, if you try to call `length`
without giving it an argument, not even an empty list:

```
(length )
```

What you see, if you evaluate this, is the error message

```
Lisp error: (wrong-number-of-arguments length 0)
```

This means that the function receives the wrong number of
arguments, zero, when it expects some other number of arguments. In
this case, one argument is expected, the argument being a list whose
length the function is measuring. (Note that *one* list is
*one* argument, even if the list has many elements inside it.)

The part of the error message that says ‘`length`’ is the name of
the function.

---

Next: [`nth`](#nth), Previous: [`cons`](#cons), Up: [`car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.3 `nthcdr`

The `nthcdr` function is associated with the `cdr` function.
What it does is take the CDR of a list repeatedly.

If you take the CDR of the list `(pine fir
oak maple)`, you will be returned the list `(fir oak maple)`. If you
repeat this on what was returned, you will be returned the list
`(oak maple)`. (Of course, repeated CDRing on the original
list will just give you the original CDR since the function does
not change the list. You need to evaluate the CDR of the
CDR and so on.) If you continue this, eventually you will be
returned an empty list, which in this case, instead of being shown as
`()` is shown as `nil`.

For review, here is a series of repeated CDRs, the text following
the ‘`⇒`’ shows what is returned.

```
(cdr '(pine fir oak maple))
     ⇒ (fir oak maple)
```

```

```

```
(cdr '(fir oak maple))
     ⇒ (oak maple)
```

```

```

```
(cdr '(oak maple))
     ⇒ (maple)
```

```

```

```
(cdr '(maple))
     ⇒ nil
```

```

```

```
(cdr 'nil)
     ⇒ nil
```

```

```

```
(cdr ())
     ⇒ nil
```

You can also do several CDRs without printing the values in
between, like this:

```
(cdr (cdr '(pine fir oak maple)))
     ⇒ (oak maple)
```

In this example, the Lisp interpreter evaluates the innermost list first.
The innermost list is quoted, so it just passes the list as it is to the
innermost `cdr`. This `cdr` passes a list made up of the
second and subsequent elements of the list to the outermost `cdr`,
which produces a list composed of the third and subsequent elements of
the original list. In this example, the `cdr` function is repeated
and returns a list that consists of the original list without its
first two elements.

The `nthcdr` function does the same as repeating the call to
`cdr`. In the following example, the argument 2 is passed to the
function `nthcdr`, along with the list, and the value returned is
the list without its first two items, which is exactly the same
as repeating `cdr` twice on the list:

```
(nthcdr 2 '(pine fir oak maple))
     ⇒ (oak maple)
```

Using the original four element list, we can see what happens when
various numeric arguments are passed to `nthcdr`, including 0, 1,
and 5:

```
;; Leave the list as it was.
(nthcdr 0 '(pine fir oak maple))
     ⇒ (pine fir oak maple)
```

```

```

```
;; Return a copy without the first element.
(nthcdr 1 '(pine fir oak maple))
     ⇒ (fir oak maple)
```

```

```

```
;; Return a copy of the list without three elements.
(nthcdr 3 '(pine fir oak maple))
     ⇒ (maple)
```

```

```

```
;; Return a copy lacking all four elements.
(nthcdr 4 '(pine fir oak maple))
     ⇒ nil
```

```

```

```
;; Return a copy lacking all elements.
(nthcdr 5 '(pine fir oak maple))
     ⇒ nil
```

---

Next: [`setcar`](#setcar), Previous: [`nthcdr`](#nthcdr), Up: [`car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.4 `nth`

The `nthcdr` function takes the CDR of a list repeatedly.
The `nth` function takes the CAR of the result returned by
`nthcdr`. It returns the Nth element of the list.

Thus, if it were not defined in C for speed, the definition of
`nth` would be:

```
(defun nth (n list)
  "Returns the Nth element of LIST.
N counts from zero.  If LIST is not that long, nil is returned."
  (car (nthcdr n list)))
```

(Originally, `nth` was defined in Emacs Lisp in `subr.el`,
but its definition was redone in C in the 1980s.)

The `nth` function returns a single element of a list.
This can be very convenient.

Note that the elements are numbered from zero, not one. That is to
say, the first element of a list, its CAR is the zeroth element.
This zero-based counting often bothers people who
are accustomed to the first element in a list being number one, which
is one-based.

For example:

```
(nth 0 '("one" "two" "three"))
    ⇒ "one"

(nth 1 '("one" "two" "three"))
    ⇒ "two"
```

It is worth mentioning that `nth`, like `nthcdr` and
`cdr`, does not change the original list—the function is
non-destructive. This is in sharp contrast to the `setcar` and
`setcdr` functions.

---

Next: [`setcdr`](#setcdr), Previous: [`nth`](#nth), Up: [`car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.5 `setcar`

As you might guess from their names, the `setcar` and `setcdr`
functions set the CAR or the CDR of a list to a new value.
They actually change the original list, unlike `car` and `cdr`
which leave the original list as it was. One way to find out how this
works is to experiment. We will start with the `setcar` function.

First, we can make a list and then set the value of a variable to the
list, using the `setq` special form. Because we intend to use
`setcar` to change the list, this `setq` should not use the
quoted form `'(antelope giraffe lion tiger)`, as that would yield
a list that is part of the program and bad things could happen if we
tried to change part of the program while running it. Generally
speaking an Emacs Lisp program’s components should be constant (or
unchanged) while the program is running. So we instead construct an
animal list by using the `list` function, as follows:

```
(setq animals (list 'antelope 'giraffe 'lion 'tiger))
```

If you are reading this in Info inside of GNU Emacs, you can evaluate
this expression in the usual fashion, by positioning the cursor after
the expression and typing `C-x C-e`. (I’m doing this right here
as I write this. This is one of the advantages of having the
interpreter built into the computing environment. Incidentally, when
there is nothing on the line after the final parentheses, such as a
comment, point can be on the next line. Thus, if your cursor is in
the first column of the next line, you do not need to move it.
Indeed, Emacs permits any amount of white space after the final
parenthesis.)

When we evaluate the variable `animals`, we see that it is bound to
the list `(antelope giraffe lion tiger)`:

```
animals
     ⇒ (antelope giraffe lion tiger)
```

Put another way, the variable `animals` points to the list
`(antelope giraffe lion tiger)`.

Next, evaluate the function `setcar` while passing it two
arguments, the variable `animals` and the quoted symbol
`hippopotamus`; this is done by writing the three element list
`(setcar animals 'hippopotamus)` and then evaluating it in the
usual fashion:

```
(setcar animals 'hippopotamus)
```

After evaluating this expression, evaluate the variable `animals`
again. You will see that the list of animals has changed:

```
animals
     ⇒ (hippopotamus giraffe lion tiger)
```

The first element on the list, `antelope` is replaced by
`hippopotamus`.

So we can see that `setcar` did not add a new element to the list
as `cons` would have; it replaced `antelope` with
`hippopotamus`; it *changed* the list.

---

Next: [Exercise](#cons-Exercise), Previous: [`setcar`](#setcar), Up: [`car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.6 `setcdr`

The `setcdr` function is similar to the `setcar` function,
except that the function replaces the second and subsequent elements of
a list rather than the first element.

(To see how to change the last element of a list, look ahead to
[The `kill-new` function](#kill_002dnew-function), which uses
the `nthcdr` and `setcdr` functions.)

To see how this works, set the value of the variable to a list of
domesticated animals by evaluating the following expression:

```
(setq domesticated-animals (list 'horse 'cow 'sheep 'goat))
```

If you now evaluate the list, you will be returned the list
`(horse cow sheep goat)`:

```
domesticated-animals
     ⇒ (horse cow sheep goat)
```

Next, evaluate `setcdr` with two arguments, the name of the
variable which has a list as its value, and the list to which the
CDR of the first list will be set;

```
(setcdr domesticated-animals '(cat dog))
```

If you evaluate this expression, the list `(cat dog)` will appear
in the echo area. This is the value returned by the function. The
result we are interested in is the side effect, which we can see by
evaluating the variable `domesticated-animals`:

```
domesticated-animals
     ⇒ (horse cat dog)
```

Indeed, the list is changed from `(horse cow sheep goat)` to
`(horse cat dog)`. The CDR of the list is changed from
`(cow sheep goat)` to `(cat dog)`.

---

Previous: [`setcdr`](#setcdr), Up: [`car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.7 Exercise

Construct a list of four birds by evaluating several expressions with
`cons`. Find out what happens when you `cons` a list onto
itself. Replace the first element of the list of four birds with a
fish. Replace the rest of that list with a list of other fish.

---

Next: [How Lists are Implemented](#List-Implementation), Previous: [`car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 8 Cutting and Storing Text

Whenever you cut or clip text out of a buffer with a *kill* command in
GNU Emacs, it is stored in a list and you can bring it back with a
*yank* command.

(The use of the word “kill” in Emacs for processes which specifically
*do not* destroy the values of the entities is an unfortunate
historical accident. A much more appropriate word would be “clip” since
that is what the kill commands do; they clip text out of a buffer and
put it into storage from which it can be brought back. I have often
been tempted to replace globally all occurrences of “kill” in the Emacs
sources with “clip” and all occurrences of “killed” with “clipped”.)

* [Storing Text in a List](#Storing-Text)
* [`zap-to-char`](#zap_002dto_002dchar)
* [`kill-region`](#kill_002dregion)
* [`copy-region-as-kill`](#copy_002dregion_002das_002dkill)
* [Digression into C](#Digression-into-C)
* [Initializing a Variable with `defvar`](#defvar)
* [Review](#cons-_0026-search_002dfwd-Review)
* [Searching Exercises](#search-Exercises)

---

Next: [`zap-to-char`](#zap_002dto_002dchar), Up: [Cutting and Storing Text](#Cutting-_0026-Storing-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Storing Text in a List

When text is cut out of a buffer, it is stored on a list. Successive
pieces of text are stored on the list successively, so the list might
look like this:

```
("a piece of text" "previous piece")
```

The function `cons` can be used to create a new list from a piece
of text (an “atom”, to use the jargon) and an existing list, like
this:

```
(cons "another piece"
      '("a piece of text" "previous piece"))
```

If you evaluate this expression, a list of three elements will appear in
the echo area:

```
("another piece" "a piece of text" "previous piece")
```

With the `car` and `nthcdr` functions, you can retrieve
whichever piece of text you want. For example, in the following code,
`nthcdr 1 …` returns the list with the first item removed;
and the `car` returns the first element of that remainder—the
second element of the original list:

```
(car (nthcdr 1 '("another piece"
                 "a piece of text"
                 "previous piece")))
     ⇒ "a piece of text"
```

The actual functions in Emacs are more complex than this, of course.
The code for cutting and retrieving text has to be written so that
Emacs can figure out which element in the list you want—the first,
second, third, or whatever. In addition, when you get to the end of
the list, Emacs should give you the first element of the list, rather
than nothing at all.

The list that holds the pieces of text is called the *kill ring*.
This chapter leads up to a description of the kill ring and how it is
used by first tracing how the `zap-to-char` function works. This
function calls a function that invokes a function that
manipulates the kill ring. Thus, before reaching the mountains, we
climb the foothills.

A subsequent chapter describes how text that is cut from the buffer is
retrieved. See [Yanking Text Back](#Yanking).

---

Next: [`kill-region`](#kill_002dregion), Previous: [Storing Text in a List](#Storing-Text), Up: [Cutting and Storing Text](#Cutting-_0026-Storing-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.1 `zap-to-char`

Let us look at the interactive `zap-to-char` function.

* [The Complete `zap-to-char` Implementation](#Complete-zap_002dto_002dchar)
* [The `interactive` Expression](#zap_002dto_002dchar-interactive)
* [The Body of `zap-to-char`](#zap_002dto_002dchar-body)
* [The `search-forward` Function](#search_002dforward)
* [The `progn` Special Form](#progn)
* [Summing up `zap-to-char`](#Summing-up-zap_002dto_002dchar)

---

Next: [The `interactive` Expression](#zap_002dto_002dchar-interactive), Up: [`zap-to-char`](#zap_002dto_002dchar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The Complete `zap-to-char` Implementation

The `zap-to-char` function removes the text in the region between
the location of the cursor (i.e., of point) up to and including the
next occurrence of a specified character. The text that
`zap-to-char` removes is put in the kill ring; and it can be
retrieved from the kill ring by typing `C-y` (`yank`). If
the command is given an argument, it removes text through that number
of occurrences. Thus, if the cursor were at the beginning of this
sentence and the character were ‘`s`’, ‘`Thus`’ would be
removed. If the argument were two, ‘`Thus, if the curs`’ would be
removed, up to and including the ‘`s`’ in ‘`cursor`’.

If the specified character is not found, `zap-to-char` will say
“Search failed”, tell you the character you typed, and not remove
any text.

In order to determine how much text to remove, `zap-to-char` uses
a search function. Searches are used extensively in code that
manipulates text, and we will focus attention on them as well as on the
deletion command.

Here is the complete text of the version 22 implementation of the function:

```
(defun zap-to-char (arg char)
  "Kill up to and including ARG'th occurrence of CHAR.
Case is ignored if `case-fold-search' is non-nil in the current buffer.
Goes backward if ARG is negative; error if CHAR not found."
  (interactive "p\ncZap to char: ")
  (if (char-table-p translation-table-for-input)
      (setq char (or (aref translation-table-for-input char) char)))
  (kill-region (point) (progn
                         (search-forward (char-to-string char)
                                         nil nil arg)
                         (point))))
```

The documentation is thorough. You do need to know the jargon meaning
of the word “kill”.

The version 22 documentation string for `zap-to-char` uses ASCII
grave accent and apostrophe to quote a symbol, so it appears as
`` `case-fold-search' ``. This quoting style was inspired by 1970s-era
displays in which grave accent and apostrophe were often mirror images
suitable for use as quotes. On most modern displays this is no longer
true, and when these two ASCII characters appear in documentation
strings or diagnostic message formats, Emacs typically transliterates
them to *curved quotes* (left and right single quotation marks),
so that the abovequoted symbol appears
as `‘case-fold-search’`. Source-code strings can also simply use
curved quotes directly.

---

Next: [The Body of `zap-to-char`](#zap_002dto_002dchar-body), Previous: [The Complete `zap-to-char` Implementation](#Complete-zap_002dto_002dchar), Up: [`zap-to-char`](#zap_002dto_002dchar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.1 The `interactive` Expression

The interactive expression in the `zap-to-char` command looks like
this:

```
(interactive "p\ncZap to char: ")
```

The part within quotation marks, `"p\ncZap to char: "`, specifies
two different things. First, and most simply, is the ‘`p`’.
This part is separated from the next part by a newline, ‘`\n`’.
The ‘`p`’ means that the first argument to the function will be
passed the value of a *processed prefix*. The prefix argument is
passed by typing `C-u` and a number, or `M-` and a number. If
the function is called interactively without a prefix, 1 is passed to
this argument.

The second part of `"p\ncZap to char: "` is
‘`cZap to char:` ’. In this part, the lower case ‘`c`’
indicates that `interactive` expects a prompt and that the
argument will be a character. The prompt follows the ‘`c`’ and is
the string ‘`Zap to char:` ’ (with a space after the colon to
make it look good).

What all this does is prepare the arguments to `zap-to-char` so they
are of the right type, and give the user a prompt.

In a read-only buffer, the `zap-to-char` function copies the text
to the kill ring, but does not remove it. The echo area displays a
message saying that the buffer is read-only. Also, the terminal may
beep or blink at you.

---

Next: [The `search-forward` Function](#search_002dforward), Previous: [The `interactive` Expression](#zap_002dto_002dchar-interactive), Up: [`zap-to-char`](#zap_002dto_002dchar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.2 The Body of `zap-to-char`

The body of the `zap-to-char` function contains the code that
kills (that is, removes) the text in the region from the current
position of the cursor up to and including the specified character.

The first part of the code looks like this:

```
(if (char-table-p translation-table-for-input)
    (setq char (or (aref translation-table-for-input char) char)))
(kill-region (point) (progn
                       (search-forward (char-to-string char) nil nil arg)
                       (point)))
```

`char-table-p` is a hitherto unseen function. It determines
whether its argument is a character table. When it is, it sets the
character passed to `zap-to-char` to one of them, if that
character exists, or to the character itself. (This becomes important
for certain characters in non-European languages. The `aref`
function extracts an element from an array. It is an array-specific
function that is not described in this document. See [Arrays](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Arrays) in The GNU Emacs Lisp Reference Manual.)

`(point)` is the current position of the cursor.

The next part of the code is an expression using `progn`. The body
of the `progn` consists of calls to `search-forward` and
`point`.

It is easier to understand how `progn` works after learning about
`search-forward`, so we will look at `search-forward` and
then at `progn`.

---

Next: [The `progn` Special Form](#progn), Previous: [The Body of `zap-to-char`](#zap_002dto_002dchar-body), Up: [`zap-to-char`](#zap_002dto_002dchar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.3 The `search-forward` Function

The `search-forward` function is used to locate the
zapped-for-character in `zap-to-char`. If the search is
successful, `search-forward` leaves point immediately after the
last character in the target string. (In `zap-to-char`, the
target string is just one character long. `zap-to-char` uses the
function `char-to-string` to ensure that the computer treats that
character as a string.) If the search is backwards,
`search-forward` leaves point just before the first character in
the target. Also, `search-forward` returns `t` for true.
(Moving point is therefore a side effect.)

In `zap-to-char`, the `search-forward` function looks like this:

```
(search-forward (char-to-string char) nil nil arg)
```

The `search-forward` function takes four arguments:

1. The first argument is the target, what is searched for. This must be a
   string, such as ‘`"z"`’.

   As it happens, the argument passed to `zap-to-char` is a single
   character. Because of the way computers are built, the Lisp
   interpreter may treat a single character as being different from a
   string of characters. Inside the computer, a single character has a
   different electronic format than a string of one character. (A single
   character can often be recorded in the computer using exactly one
   byte; but a string may be longer, and the computer needs to be ready
   for this.) Since the `search-forward` function searches for a
   string, the character that the `zap-to-char` function receives as
   its argument must be converted inside the computer from one format to
   the other; otherwise the `search-forward` function will fail.
   The `char-to-string` function is used to make this conversion.
2. The second argument bounds the search; it is specified as a position in
   the buffer. In this case, the search can go to the end of the buffer,
   so no bound is set and the second argument is `nil`.
3. The third argument tells the function what it should do if the search
   fails—it can signal an error (and print a message) or it can return
   `nil`. A `nil` as the third argument causes the function to
   signal an error when the search fails.
4. The fourth argument to `search-forward` is the repeat count—how
   many occurrences of the string to look for. This argument is optional
   and if the function is called without a repeat count, this argument is
   passed the value 1. If this argument is negative, the search goes
   backwards.

In template form, a `search-forward` expression looks like this:

```
(search-forward "target-string"
                limit-of-search
                what-to-do-if-search-fails
                repeat-count)
```

We will look at `progn` next.

---

Next: [Summing up `zap-to-char`](#Summing-up-zap_002dto_002dchar), Previous: [The `search-forward` Function](#search_002dforward), Up: [`zap-to-char`](#zap_002dto_002dchar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.4 The `progn` Special Form

`progn` is a special form that causes each of its arguments to be
evaluated in sequence and then returns the value of the last one. The
preceding expressions are evaluated only for the side effects they
perform. The values produced by them are discarded.

The template for a `progn` expression is very simple:

```
(progn
  body…)
```

In `zap-to-char`, the `progn` expression has to do two things:
put point in exactly the right position; and return the location of
point so that `kill-region` will know how far to kill to.

The first argument to the `progn` is `search-forward`. When
`search-forward` finds the string, the function leaves point
immediately after the last character in the target string. (In this
case the target string is just one character long.) If the search is
backwards, `search-forward` leaves point just before the first
character in the target. The movement of point is a side effect.

The second and last argument to `progn` is the expression
`(point)`. This expression returns the value of point, which in
this case will be the location to which it has been moved by
`search-forward`. (In the source, a line that tells the function
to go to the previous character, if it is going forward, was commented
out in 1999; I don’t remember whether that feature or mis-feature was
ever a part of the distributed source.) The value of `point` is
returned by the `progn` expression and is passed to
`kill-region` as `kill-region`’s second argument.

---

Previous: [The `progn` Special Form](#progn), Up: [`zap-to-char`](#zap_002dto_002dchar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.1.5 Summing up `zap-to-char`

Now that we have seen how `search-forward` and `progn` work,
we can see how the `zap-to-char` function works as a whole.

The first argument to `kill-region` is the position of the cursor
when the `zap-to-char` command is given—the value of point at
that time. Within the `progn`, the search function then moves
point to just after the zapped-to-character and `point` returns the
value of this location. The `kill-region` function puts together
these two values of point, the first one as the beginning of the region
and the second one as the end of the region, and removes the region.

The `progn` special form is necessary because the
`kill-region` command takes two arguments; and it would fail if
`search-forward` and `point` expressions were written in
sequence as two additional arguments. The `progn` expression is
a single argument to `kill-region` and returns the one value that
`kill-region` needs for its second argument.

---

Next: [`copy-region-as-kill`](#copy_002dregion_002das_002dkill), Previous: [`zap-to-char`](#zap_002dto_002dchar), Up: [Cutting and Storing Text](#Cutting-_0026-Storing-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.2 `kill-region`

The `zap-to-char` function uses the `kill-region` function.
This function clips text from a region and copies that text to
the kill ring, from which it may be retrieved.

The Emacs 22 version of that function uses `condition-case` and
`copy-region-as-kill`, both of which we will explain.
`condition-case` is an important special form.

In essence, the `kill-region` function calls
`condition-case`, which takes three arguments. In this function,
the first argument does nothing. The second argument contains the
code that does the work when all goes well. The third argument
contains the code that is called in the event of an error.

* [The Complete `kill-region` Definition](#Complete-kill_002dregion)
* [`condition-case`](#condition_002dcase)
* [Lisp macro](#Lisp-macro)

---

Next: [`condition-case`](#condition_002dcase), Up: [`kill-region`](#kill_002dregion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The Complete `kill-region` Definition

We will go through the `condition-case` code in a moment. First,
let us look at the definition of `kill-region`, with comments
added:

```
(defun kill-region (beg end)
  "Kill (\"cut\") text between point and mark.
This deletes the text from the buffer and saves it in the kill ring.
The command \\[yank] can retrieve it from there. … "
```

```

```

```
  ;; • Since order matters, pass point first.
  (interactive (list (point) (mark)))
  ;; • And tell us if we cannot cut the text.
  ;; 'unless' is an 'if' without a then-part.
  (unless (and beg end)
    (error "The mark is not set now, so there is no region"))
```

```

```

```
  ;; • 'condition-case' takes three arguments.
  ;;    If the first argument is nil, as it is here,
  ;;    information about the error signal is not
  ;;    stored for use by another function.
  (condition-case nil
```

```

```

```
      ;; • The second argument to 'condition-case' tells the
      ;;    Lisp interpreter what to do when all goes well.
```

```

```

```
      ;;    It starts with a 'let' function that extracts the string
      ;;    and tests whether it exists.  If so (that is what the
      ;;    'when' checks), it calls an 'if' function that determines
      ;;    whether the previous command was another call to
      ;;    'kill-region'; if it was, then the new text is appended to
      ;;    the previous text; if not, then a different function,
      ;;    'kill-new', is called.
```

```

```

```
      ;;    The 'kill-append' function concatenates the new string and
      ;;    the old.  The 'kill-new' function inserts text into a new
      ;;    item in the kill ring.
```

```

```

```
      ;;    'when' is an 'if' without an else-part.  The second 'when'
      ;;    again checks whether the current string exists; in
      ;;    addition, it checks whether the previous command was
      ;;    another call to 'kill-region'.  If one or the other
      ;;    condition is true, then it sets the current command to
      ;;    be 'kill-region'.
```

```
      (let ((string (filter-buffer-substring beg end t)))
        (when string                    ;STRING is nil if BEG = END
          ;; Add that string to the kill ring, one way or another.
          (if (eq last-command 'kill-region)
```

```
              ;;    − 'yank-handler' is an optional argument to
              ;;    'kill-region' that tells the 'kill-append' and
              ;;    'kill-new' functions how deal with properties
              ;;    added to the text, such as 'bold' or 'italics'.
              (kill-append string (< end beg) yank-handler)
            (kill-new string nil yank-handler)))
        (when (or string (eq last-command 'kill-region))
          (setq this-command 'kill-region))
        nil)
```

```

```

```
    ;;  • The third argument to 'condition-case' tells the interpreter
    ;;    what to do with an error.
```

```
    ;;    The third argument has a conditions part and a body part.
    ;;    If the conditions are met (in this case,
    ;;             if text or buffer are read-only)
    ;;    then the body is executed.
```

```
    ;;    The first part of the third argument is the following:
    ((buffer-read-only text-read-only) ;; the if-part
     ;; …  the then-part
     (copy-region-as-kill beg end)
```

```
     ;;    Next, also as part of the then-part, set this-command, so
     ;;    it will be set in an error
     (setq this-command 'kill-region)
     ;;    Finally, in the then-part, send a message if you may copy
     ;;    the text to the kill ring without signaling an error, but
     ;;    don't if you may not.
```

```
     (if kill-read-only-ok
         (progn (message "Read only text copied to kill ring") nil)
       (barf-if-buffer-read-only)
       ;; If the buffer isn't read-only, the text is.
       (signal 'text-read-only (list (current-buffer)))))))
```

---

Next: [Lisp macro](#Lisp-macro), Previous: [The Complete `kill-region` Definition](#Complete-kill_002dregion), Up: [`kill-region`](#kill_002dregion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.2.1 `condition-case`

As we have seen earlier (see [Generate an Error
Message](#Making-Errors)), when the Emacs Lisp interpreter has trouble evaluating an
expression, it provides you with help; in the jargon, this is called
“signaling an error”. Usually, the computer stops the program and
shows you a message.

However, some programs undertake complicated actions. They should not
simply stop on an error. In the `kill-region` function, the most
likely error is that you will try to kill text that is read-only and
cannot be removed. So the `kill-region` function contains code
to handle this circumstance. This code, which makes up the body of
the `kill-region` function, is inside of a `condition-case`
special form.

The template for `condition-case` looks like this:

```
(condition-case
  var
  bodyform
  error-handler…)
```

The second argument, bodyform, is straightforward. The
`condition-case` special form causes the Lisp interpreter to
evaluate the code in bodyform. If no error occurs, the special
form returns the code’s value and produces the side-effects, if any.

In short, the bodyform part of a `condition-case`
expression determines what should happen when everything works
correctly.

However, if an error occurs, among its other actions, the function
generating the error signal will define one or more error condition
names.

An error handler is the third argument to `condition-case`.
An error handler has two parts, a condition-name and a
body. If the condition-name part of an error handler
matches a condition name generated by an error, then the body
part of the error handler is run.

As you will expect, the condition-name part of an error handler
may be either a single condition name or a list of condition names.

Also, a complete `condition-case` expression may contain more
than one error handler. When an error occurs, the first applicable
handler is run.

Lastly, the first argument to the `condition-case` expression,
the var argument, is sometimes bound to a variable that contains
information about the error. However, if that argument is `nil`,
as is the case in `kill-region`, that information is discarded.

In brief, in the `kill-region` function, the code
`condition-case` works like this:

```
If no errors, run only this code
    but, if errors, run this other code.
```

---

Previous: [`condition-case`](#condition_002dcase), Up: [`kill-region`](#kill_002dregion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.2.2 Lisp macro

The part of the `condition-case` expression that is evaluated in
the expectation that all goes well has a `when`. The code uses
`when` to determine whether the `string` variable points to
text that exists.

A `when` expression is simply a programmers’ convenience. It is
like an `if` without the possibility of an else clause. In your
mind, you can replace `when` with `if` and understand what
goes on. That is what the Lisp interpreter does.

Technically speaking, `when` is a Lisp macro. A Lisp macro
enables you to define new control constructs and other language
features. It tells the interpreter how to compute another Lisp
expression which will in turn compute the value. In this case, the
other expression is an `if` expression.

The `kill-region` function definition also has an `unless`
macro; it is the opposite of `when`. The `unless` macro is
like an `if` except that it has no then-clause, and it supplies
an implicit `nil` for that.

For more about Lisp macros, see [Macros](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Macros) in The GNU
Emacs Lisp Reference Manual. The C programming language also
provides macros. These are different, but also useful.

Regarding the `when` macro, in the `condition-case`
expression, when the string has content, then another conditional
expression is executed. This is an `if` with both a then-part
and an else-part.

```
(if (eq last-command 'kill-region)
    (kill-append string (< end beg) yank-handler)
  (kill-new string nil yank-handler))
```

The then-part is evaluated if the previous command was another call to
`kill-region`; if not, the else-part is evaluated.

`yank-handler` is an optional argument to `kill-region` that
tells the `kill-append` and `kill-new` functions how deal
with properties added to the text, such as bold or italics.

`last-command` is a variable that comes with Emacs that we have
not seen before. Normally, whenever a function is executed, Emacs
sets the value of `last-command` to the previous command.

In this segment of the definition, the `if` expression checks
whether the previous command was `kill-region`. If it was,

```
(kill-append string (< end beg) yank-handler)
```

concatenates a copy of the newly clipped text to the just previously
clipped text in the kill ring.

---

Next: [Digression into C](#Digression-into-C), Previous: [`kill-region`](#kill_002dregion), Up: [Cutting and Storing Text](#Cutting-_0026-Storing-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.3 `copy-region-as-kill`

The `copy-region-as-kill` function copies a region of text from a
buffer and (via either `kill-append` or `kill-new`) saves it
in the `kill-ring`.

If you call `copy-region-as-kill` immediately after a
`kill-region` command, Emacs appends the newly copied text to the
previously copied text. This means that if you yank back the text, you
get it all, from both this and the previous operation. On the other
hand, if some other command precedes the `copy-region-as-kill`,
the function copies the text into a separate entry in the kill ring.

* [The complete `copy-region-as-kill` function definition](#Complete-copy_002dregion_002das_002dkill)
* [The Body of `copy-region-as-kill`](#copy_002dregion_002das_002dkill-body)

---

Next: [The Body of `copy-region-as-kill`](#copy_002dregion_002das_002dkill-body), Up: [`copy-region-as-kill`](#copy_002dregion_002das_002dkill)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The complete `copy-region-as-kill` function definition

Here is the complete text of the version 22 `copy-region-as-kill`
function:

```
(defun copy-region-as-kill (beg end)
  "Save the region as if killed, but don't kill it.
In Transient Mark mode, deactivate the mark.
If `interprogram-cut-function' is non-nil, also save the text for a window
system cut and paste."
  (interactive "r")
```

```
  (if (eq last-command 'kill-region)
      (kill-append (filter-buffer-substring beg end) (< end beg))
    (kill-new (filter-buffer-substring beg end)))
```

```
  (if transient-mark-mode
      (setq deactivate-mark t))
  nil)
```

As usual, this function can be divided into its component parts:

```
(defun copy-region-as-kill (argument-list)
  "documentation…"
  (interactive "r")
  body…)
```

The arguments are `beg` and `end` and the function is
interactive with `"r"`, so the two arguments must refer to the
beginning and end of the region. If you have been reading through this
document from the beginning, understanding these parts of a function is
almost becoming routine.

The documentation is somewhat confusing unless you remember that the
word “kill” has a meaning different from usual. The Transient Mark
and `interprogram-cut-function` comments explain certain
side-effects.

After you once set a mark, a buffer always contains a region. If you
wish, you can use Transient Mark mode to highlight the region
temporarily. (No one wants to highlight the region all the time, so
Transient Mark mode highlights it only at appropriate times. Many
people turn off Transient Mark mode, so the region is never
highlighted.)

Also, a windowing system allows you to copy, cut, and paste among
different programs. In the X windowing system, for example, the
`interprogram-cut-function` function is `x-select-text`,
which works with the windowing system’s equivalent of the Emacs kill
ring.

The body of the `copy-region-as-kill` function starts with an
`if` clause. What this clause does is distinguish between two
different situations: whether or not this command is executed
immediately after a previous `kill-region` command. In the first
case, the new region is appended to the previously copied text.
Otherwise, it is inserted into the beginning of the kill ring as a
separate piece of text from the previous piece.

The last two lines of the function prevent the region from lighting up
if Transient Mark mode is turned on.

The body of `copy-region-as-kill` merits discussion in detail.

---

Previous: [The complete `copy-region-as-kill` function definition](#Complete-copy_002dregion_002das_002dkill), Up: [`copy-region-as-kill`](#copy_002dregion_002das_002dkill)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.3.1 The Body of `copy-region-as-kill`

The `copy-region-as-kill` function works in much the same way as
the `kill-region` function. Both are written so that two or more
kills in a row combine their text into a single entry. If you yank
back the text from the kill ring, you get it all in one piece.
Moreover, kills that kill forward from the current position of the
cursor are added to the end of the previously copied text and commands
that copy text backwards add it to the beginning of the previously
copied text. This way, the words in the text stay in the proper
order.

Like `kill-region`, the `copy-region-as-kill` function makes
use of the `last-command` variable that keeps track of the
previous Emacs command.

* [`last-command` and `this-command`](#last_002dcommand-_0026-this_002dcommand)
* [The `kill-append` function](#kill_002dappend-function)
* [The `kill-new` function](#kill_002dnew-function)

---

Next: [The `kill-append` function](#kill_002dappend-function), Up: [The Body of `copy-region-as-kill`](#copy_002dregion_002das_002dkill-body)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### `last-command` and `this-command`

Normally, whenever a function is executed, Emacs sets the value of
`this-command` to the function being executed (which in this case
would be `copy-region-as-kill`). At the same time, Emacs sets
the value of `last-command` to the previous value of
`this-command`.

In the first part of the body of the `copy-region-as-kill`
function, an `if` expression determines whether the value of
`last-command` is `kill-region`. If so, the then-part of
the `if` expression is evaluated; it uses the `kill-append`
function to concatenate the text copied at this call to the function
with the text already in the first element (the CAR) of the kill
ring. On the other hand, if the value of `last-command` is not
`kill-region`, then the `copy-region-as-kill` function
attaches a new element to the kill ring using the `kill-new`
function.

The `if` expression reads as follows; it uses `eq`:

```
  (if (eq last-command 'kill-region)
      ;; then-part
      (kill-append  (filter-buffer-substring beg end) (< end beg))
    ;; else-part
    (kill-new  (filter-buffer-substring beg end)))
```

(The `filter-buffer-substring` function returns a filtered
substring of the buffer, if any. Optionally—the arguments are not
here, so neither is done—the function may delete the initial text or
return the text without its properties; this function is a replacement
for the older `buffer-substring` function, which came before text
properties were implemented.)

The `eq` function tests whether its first argument is the same Lisp
object as its second argument. The `eq` function is similar to the
`equal` function in that it is used to test for equality, but
differs in that it determines whether two representations are actually
the same object inside the computer, but with different names.
`equal` determines whether the structure and contents of two
expressions are the same.

If the previous command was `kill-region`, then the Emacs Lisp
interpreter calls the `kill-append` function

---

Next: [The `kill-new` function](#kill_002dnew-function), Previous: [`last-command` and `this-command`](#last_002dcommand-_0026-this_002dcommand), Up: [The Body of `copy-region-as-kill`](#copy_002dregion_002das_002dkill-body)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The `kill-append` function

The `kill-append` function looks like this:

```
(defun kill-append (string before-p &optional yank-handler)
  "Append STRING to the end of the latest kill in the kill ring.
If BEFORE-P is non-nil, prepend STRING to the kill.
… "
  (let* ((cur (car kill-ring)))
    (kill-new (if before-p (concat string cur) (concat cur string))
              (or (= (length cur) 0)
                  (equal yank-handler
                         (get-text-property 0 'yank-handler cur)))
              yank-handler)))
```

The `kill-append` function is fairly straightforward. It uses
the `kill-new` function, which we will discuss in more detail in
a moment.

(Also, the function provides an optional argument called
`yank-handler`; when invoked, this argument tells the function
how to deal with properties added to the text, such as bold or
italics.)

It has a `let*` function to set the value of the first element of
the kill ring to `cur`. (I do not know why the function does not
use `let` instead; only one value is set in the expression.
Perhaps this is a bug that produces no problems?)

Consider the conditional that is one of the two arguments to
`kill-new`. It uses `concat` to concatenate the new text to
the CAR of the kill ring. Whether it prepends or appends the
text depends on the results of an `if` expression:

```
(if before-p                            ; if-part
    (concat string cur)                 ; then-part
  (concat cur string))                  ; else-part
```

If the region being killed is before the region that was killed in the
last command, then it should be prepended before the material that was
saved in the previous kill; and conversely, if the killed text follows
what was just killed, it should be appended after the previous text.
The `if` expression depends on the predicate `before-p` to
decide whether the newly saved text should be put before or after the
previously saved text.

The symbol `before-p` is the name of one of the arguments to
`kill-append`. When the `kill-append` function is
evaluated, it is bound to the value returned by evaluating the actual
argument. In this case, this is the expression `(< end beg)`.
This expression does not directly determine whether the killed text in
this command is located before or after the kill text of the last
command; what it does is determine whether the value of the variable
`end` is less than the value of the variable `beg`. If it
is, it means that the user is most likely heading towards the
beginning of the buffer. Also, the result of evaluating the predicate
expression, `(< end beg)`, will be true and the text will be
prepended before the previous text. On the other hand, if the value of
the variable `end` is greater than the value of the variable
`beg`, the text will be appended after the previous text.

When the newly saved text will be prepended, then the string with the new
text will be concatenated before the old text:

```
(concat string cur)
```

But if the text will be appended, it will be concatenated
after the old text:

```
(concat cur string))
```

To understand how this works, we first need to review the
`concat` function. The `concat` function links together or
unites two strings of text. The result is a string. For example:

```
(concat "abc" "def")
     ⇒ "abcdef"
```

```

```

```
(concat "new "
        (car '("first element" "second element")))
     ⇒ "new first element"

(concat (car
        '("first element" "second element")) " modified")
     ⇒ "first element modified"
```

We can now make sense of `kill-append`: it modifies the contents
of the kill ring. The kill ring is a list, each element of which is
saved text. The `kill-append` function uses the `kill-new`
function which in turn uses the `setcar` function.

---

Previous: [The `kill-append` function](#kill_002dappend-function), Up: [The Body of `copy-region-as-kill`](#copy_002dregion_002das_002dkill-body)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The `kill-new` function

In version 22 the `kill-new` function looks like this:

```
(defun kill-new (string &optional replace yank-handler)
  "Make STRING the latest kill in the kill ring.
Set `kill-ring-yank-pointer' to point to it.

If `interprogram-cut-function' is non-nil, apply it to STRING.
Optional second argument REPLACE non-nil means that STRING will replace
the front of the kill ring, rather than being added to the list.
…"
```

```
  (if (> (length string) 0)
      (if yank-handler
          (put-text-property 0 (length string)
                             'yank-handler yank-handler string))
    (if yank-handler
        (signal 'args-out-of-range
                (list string "yank-handler specified for empty string"))))
```

```
  (if (fboundp 'menu-bar-update-yank-menu)
      (menu-bar-update-yank-menu string (and replace (car kill-ring))))
```

```
  (if (and replace kill-ring)
      (setcar kill-ring string)
    (push string kill-ring)
    (if (> (length kill-ring) kill-ring-max)
        (setcdr (nthcdr (1- kill-ring-max) kill-ring) nil)))
```

```
  (setq kill-ring-yank-pointer kill-ring)
  (if interprogram-cut-function
      (funcall interprogram-cut-function string (not replace))))
```

(Notice that the function is not interactive.)

As usual, we can look at this function in parts.

The function definition has an optional `yank-handler` argument,
which when invoked tells the function how to deal with properties
added to the text, such as bold or italics. We will skip that.

The first line of the documentation makes sense:

```
Make STRING the latest kill in the kill ring.
```

Let’s skip over the rest of the documentation for the moment.

Also, let’s skip over the initial `if` expression and those lines
of code involving `menu-bar-update-yank-menu`. We will explain
them below.

The critical lines are these:

```
  (if (and replace kill-ring)
      ;; then
      (setcar kill-ring string)
```

```
    ;; else
    (push string kill-ring)
```

```
    (if (> (length kill-ring) kill-ring-max)
        ;; avoid overly long kill ring
        (setcdr (nthcdr (1- kill-ring-max) kill-ring) nil)))
```

```
  (setq kill-ring-yank-pointer kill-ring)
  (if interprogram-cut-function
      (funcall interprogram-cut-function string (not replace))))
```

The conditional test is `(and replace kill-ring)`.
This will be true when two conditions are met: the kill ring has
something in it, and the `replace` variable is true.

When the `kill-append` function sets `replace` to be true
and when the kill ring has at least one item in it, the `setcar`
expression is executed:

```
(setcar kill-ring string)
```

The `setcar` function actually changes the first element of the
`kill-ring` list to the value of `string`. It replaces the
first element.

On the other hand, if the kill ring is empty, or replace is false, the
else-part of the condition is executed:

```
(push string kill-ring)
```

`push` puts its first argument onto the second. It is similar to
the older

```
(setq kill-ring (cons string kill-ring))
```

or the newer

```
(add-to-list kill-ring string)
```

When it is false, the expression first constructs a new version of the
kill ring by prepending `string` to the existing kill ring as a
new element (that is what the `push` does). Then it executes a
second `if` clause. This second `if` clause keeps the kill
ring from growing too long.

Let’s look at these two expressions in order.

The `push` line of the else-part sets the new value of the kill
ring to what results from adding the string being killed to the old
kill ring.

We can see how this works with an example.

First,

```
(setq example-list '("here is a clause" "another clause"))
```

After evaluating this expression with `C-x C-e`, you can evaluate
`example-list` and see what it returns:

```
example-list
     ⇒ ("here is a clause" "another clause")
```

Now, we can add a new element on to this list by evaluating the
following expression:

```
(push "a third clause" example-list)
```

When we evaluate `example-list`, we find its value is:

```
example-list
     ⇒ ("a third clause" "here is a clause" "another clause")
```

Thus, the third clause is added to the list by `push`.

Now for the second part of the `if` clause. This expression
keeps the kill ring from growing too long. It looks like this:

```
(if (> (length kill-ring) kill-ring-max)
    (setcdr (nthcdr (1- kill-ring-max) kill-ring) nil))
```

The code checks whether the length of the kill ring is greater than
the maximum permitted length. This is the value of
`kill-ring-max` (which is 120, by default). If the length of the
kill ring is too long, then this code sets the last element of the
kill ring to `nil`. It does this by using two functions,
`nthcdr` and `setcdr`.

We looked at `setcdr` earlier (see [`setcdr`](#setcdr)).
It sets the CDR of a list, just as `setcar` sets the
CAR of a list. In this case, however, `setcdr` will not be
setting the CDR of the whole kill ring; the `nthcdr`
function is used to cause it to set the CDR of the next to last
element of the kill ring—this means that since the CDR of the
next to last element is the last element of the kill ring, it will set
the last element of the kill ring.

The `nthcdr` function works by repeatedly taking the CDR of a
list—it takes the CDR of the CDR of the CDR
… It does this N times and returns the results.
(See [`nthcdr`](#nthcdr).)

Thus, if we had a four element list that was supposed to be three
elements long, we could set the CDR of the next to last element
to `nil`, and thereby shorten the list. (If you set the last
element to some other value than `nil`, which you could do, then
you would not have shortened the list. See [`setcdr`](#setcdr).)

You can see shortening by evaluating the following three expressions
in turn. First set the value of `trees` to `(maple oak pine
birch)`, then set the CDR of its second CDR to `nil`
and then find the value of `trees`:

```
(setq trees (list 'maple 'oak 'pine 'birch))
     ⇒ (maple oak pine birch)
```

```

```

```
(setcdr (nthcdr 2 trees) nil)
     ⇒ nil

trees
     ⇒ (maple oak pine)
```

(The value returned by the `setcdr` expression is `nil` since
that is what the CDR is set to.)

To repeat, in `kill-new`, the `nthcdr` function takes the
CDR a number of times that is one less than the maximum permitted
size of the kill ring and `setcdr` sets the CDR of that
element (which will be the rest of the elements in the kill ring) to
`nil`. This prevents the kill ring from growing too long.

The next to last expression in the `kill-new` function is

```
(setq kill-ring-yank-pointer kill-ring)
```

The `kill-ring-yank-pointer` is a global variable that is set to be
the `kill-ring`.

Even though the `kill-ring-yank-pointer` is called a
‘`pointer`’, it is a variable just like the kill ring. However, the
name has been chosen to help humans understand how the variable is used.

Now, to return to an early expression in the body of the function:

```
  (if (fboundp 'menu-bar-update-yank-menu)
       (menu-bar-update-yank-menu string (and replace (car kill-ring))))
```

It starts with an `if` expression

In this case, the expression tests first to see whether
`menu-bar-update-yank-menu` exists as a function, and if so,
calls it. The `fboundp` function returns true if the symbol it
is testing has a function definition that is not void. If the
symbol’s function definition were void, we would receive an error
message, as we did when we created errors intentionally (see [Generate an Error Message](#Making-Errors)).

The then-part contains an expression whose first element is the
function `and`.

The `and` special form evaluates each of its arguments until one
of the arguments returns a value of `nil`, in which case the
`and` expression returns `nil`; however, if none of the
arguments returns a value of `nil`, the value resulting from
evaluating the last argument is returned. (Since such a value is not
`nil`, it is considered true in Emacs Lisp.) In other words, an
`and` expression returns a true value only if all its arguments
are true. (See [Review](#Second-Buffer-Related-Review).)

The expression determines whether the second argument to
`menu-bar-update-yank-menu` is true or not.

`menu-bar-update-yank-menu` is one of the functions that make it
possible to use the “Select and Paste” menu in the Edit item of a menu
bar; using a mouse, you can look at the various pieces of text you
have saved and select one piece to paste.

The last expression in the `kill-new` function adds the newly
copied string to whatever facility exists for copying and pasting
among different programs running in a windowing system. In the X
Windowing system, for example, the `x-select-text` function takes
the string and stores it in memory operated by X. You can paste the
string in another program, such as an Xterm.

The expression looks like this:

```
  (if interprogram-cut-function
      (funcall interprogram-cut-function string (not replace))))
```

If an `interprogram-cut-function` exists, then Emacs executes
`funcall`, which in turn calls its first argument as a function
and passes the remaining arguments to it. (Incidentally, as far as I
can see, this `if` expression could be replaced by an `and`
expression similar to the one in the first part of the function.)

We are not going to discuss windowing systems and other programs
further, but merely note that this is a mechanism that enables GNU
Emacs to work easily and well with other programs.

This code for placing text in the kill ring, either concatenated with
an existing element or as a new element, leads us to the code for
bringing back text that has been cut out of the buffer—the yank
commands. However, before discussing the yank commands, it is better
to learn how lists are implemented in a computer. This will make
clear such mysteries as the use of the term “pointer”. But before
that, we will digress into C.

---

Next: [Initializing a Variable with `defvar`](#defvar), Previous: [`copy-region-as-kill`](#copy_002dregion_002das_002dkill), Up: [Cutting and Storing Text](#Cutting-_0026-Storing-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.4 Digression into C

The `copy-region-as-kill` function (see [`copy-region-as-kill`](#copy_002dregion_002das_002dkill)) uses the `filter-buffer-substring`
function, which in turn uses the `delete-and-extract-region`
function. It removes the contents of a region and you cannot get them
back.

Unlike the other code discussed here, the
`delete-and-extract-region` function is not written in Emacs
Lisp; it is written in C and is one of the primitives of the GNU Emacs
system. Since it is very simple, I will digress briefly from Lisp and
describe it here.

Like many of the other Emacs primitives,
`delete-and-extract-region` is written as an instance of a C
macro, a macro being a template for code. The complete macro looks
like this:

```
DEFUN ("delete-and-extract-region", Fdelete_and_extract_region,
       Sdelete_and_extract_region, 2, 2, 0,
       doc: /* Delete the text between START and END and return it.  */)
  (Lisp_Object start, Lisp_Object end)
{
  validate_region (&start, &end);
  if (XFIXNUM (start) == XFIXNUM (end))
    return empty_unibyte_string;
  return del_range_1 (XFIXNUM (start), XFIXNUM (end), 1, 1);
}
```

Without going into the details of the macro writing process, let me
point out that this macro starts with the word `DEFUN`. The word
`DEFUN` was chosen since the code serves the same purpose as
`defun` does in Lisp. (The `DEFUN` C macro is defined in
`emacs/src/lisp.h`.)

The word `DEFUN` is followed by seven parts inside of
parentheses:

* The first part is the name given to the function in Lisp,
  `delete-and-extract-region`.
* The second part is the name of the function in C,
  `Fdelete_and_extract_region`. By convention, it starts with
  ‘`F`’. Since C does not use hyphens in names, underscores are used
  instead.
* The third part is the name for the C constant structure that records
  information on this function for internal use. It is the name of the
  function in C but begins with an ‘`S`’ instead of an ‘`F`’.
* The fourth and fifth parts specify the minimum and maximum number of
  arguments the function can have. This function demands exactly 2
  arguments.
* The sixth part is nearly like the argument that follows the
  `interactive` declaration in a function written in Lisp: a letter
  followed, perhaps, by a prompt. The only difference from Lisp is
  when the macro is called with no arguments. Then you write a `0`
  (which is a null string), as in this macro.

  If you were to specify arguments, you would place them between
  quotation marks. The C macro for `goto-char` includes
  `"NGoto char: "` in this position to indicate that the function
  expects a raw prefix, in this case, a numerical location in a buffer,
  and provides a prompt.
* The seventh part is a documentation string, just like the one for a
  function written in Emacs Lisp. This is written as a C comment. (When
  you build Emacs, the program `lib-src/make-docfile` extracts
  these comments and uses them to make the documentation.)

In a C macro, the formal parameters come next, with a statement of
what kind of object they are, followed by the body
of the macro. For `delete-and-extract-region` the body
consists of the following four lines:

```
validate_region (&start, &end);
if (XFIXNUM (start) == XFIXNUM (end))
  return empty_unibyte_string;
return del_range_1 (XFIXNUM (start), XFIXNUM (end), 1, 1);
```

The `validate_region` function checks whether the values
passed as the beginning and end of the region are the proper type and
are within range. If the beginning and end positions are the same,
then return an empty string.

The `del_range_1` function actually deletes the text. It is a
complex function we will not look into. It updates the buffer and
does other things. However, it is worth looking at the two arguments
passed to `del_range_1`. These are `XFIXNUM (start)` and
`XFIXNUM (end)`.

As far as the C language is concerned, `start` and `end` are
two opaque values that mark the beginning and end of the region to be
deleted. More precisely, and requiring more expert knowledge
to understand, the two values are of type `Lisp_Object`, which
might be a C pointer, a C integer, or a C `struct`; C code
ordinarily should not care how `Lisp_Object` is implemented.

`Lisp_Object` widths depend on the machine, and are typically 32
or 64 bits. A few of the bits are used to specify the type of
information; the remaining bits are used as content.

‘`XFIXNUM`’ is a C macro that extracts the relevant integer from the
longer collection of bits; the type bits are discarded.

The command in `delete-and-extract-region` looks like this:

```
del_range_1 (XFIXNUM (start), XFIXNUM (end), 1, 1);
```

It deletes the region between the beginning position, `start`,
and the ending position, `end`.

From the point of view of the person writing Lisp, Emacs is all very
simple; but hidden underneath is a great deal of complexity to make it
all work.

---

Next: [Review](#cons-_0026-search_002dfwd-Review), Previous: [Digression into C](#Digression-into-C), Up: [Cutting and Storing Text](#Cutting-_0026-Storing-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
