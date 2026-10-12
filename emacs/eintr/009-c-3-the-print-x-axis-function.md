### C.3 The `print-X-axis` Function

X axis labels are much like Y axis labels, except that the ticks are on a
line above the numbers. Labels should look like this:

```
    |   |    |    |
    1   5   10   15
```

The first tic is under the first column of the graph and is preceded by
several blank spaces. These spaces provide room in rows above for the Y
axis labels. The second, third, fourth, and subsequent ticks are all
spaced equally, according to the value of `X-axis-label-spacing`.

The second row of the X axis consists of numbers, preceded by several
blank spaces and also separated according to the value of the variable
`X-axis-label-spacing`.

The value of the variable `X-axis-label-spacing` should itself be
measured in units of `symbol-width`, since you may want to change
the width of the symbols that you are using to print the body of the
graph without changing the ways the graph is labeled.

* [Similarities and differences](#Similarities-differences)
* [X Axis Tic Marks](#X-Axis-Tic-Marks)

---

Next: [X Axis Tic Marks](#X-Axis-Tic-Marks), Up: [The `print-X-axis` Function](#print_002dX_002daxis)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Similarities and differences

The `print-X-axis` function is constructed in more or less the
same fashion as the `print-Y-axis` function except that it has
two lines: the line of tic marks and the numbers. We will write a
separate function to print each line and then combine them within the
`print-X-axis` function.

This is a three step process:

1. Write a function to print the X axis tic marks, `print-X-axis-tic-line`.
2. Write a function to print the X numbers, `print-X-axis-numbered-line`.
3. Write a function to print both lines, the `print-X-axis` function,
   using `print-X-axis-tic-line` and
   `print-X-axis-numbered-line`.

---

Previous: [Similarities and differences](#Similarities-differences), Up: [The `print-X-axis` Function](#print_002dX_002daxis)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.3.1 X Axis Tic Marks

The first function should print the X axis tic marks. We must specify
the tic marks themselves and their spacing:

```
(defvar X-axis-label-spacing
  (if (boundp 'graph-blank)
      (* 5 (length graph-blank)) 5)
  "Number of units from one X axis label to next.")
```

(Note that the value of `graph-blank` is set by another
`defvar`. The `boundp` predicate checks whether it has
already been set; `boundp` returns `nil` if it has not. If
`graph-blank` were unbound and we did not use this conditional
construction, we would enter the debugger and see an error message
saying ‘`Debugger entered--Lisp error:
(void-variable graph-blank)`’.)

Here is the `defvar` for `X-axis-tic-symbol`:

```
(defvar X-axis-tic-symbol "|"
  "String to insert to point to a column in X axis.")
```

The goal is to make a line that looks like this:

```
       |   |    |    |
```

The first tic is indented so that it is under the first column, which is
indented to provide space for the Y axis labels.

A tic element consists of the blank spaces that stretch from one tic to
the next plus a tic symbol. The number of blanks is determined by the
width of the tic symbol and the `X-axis-label-spacing`.

The code looks like this:

```
;;; X-axis-tic-element
…
(concat
 (make-string
  ;; Make a string of blanks.
  (-  (* symbol-width X-axis-label-spacing)
      (length X-axis-tic-symbol))
  ? )
 ;; Concatenate blanks with tic symbol.
 X-axis-tic-symbol)
…
```

Next, we determine how many blanks are needed to indent the first tic
mark to the first column of the graph. This uses the value of
`full-Y-label-width` passed it by the `print-graph` function.

The code to make `X-axis-leading-spaces`
looks like this:

```
;; X-axis-leading-spaces
…
(make-string full-Y-label-width ? )
…
```

We also need to determine the length of the horizontal axis, which is
the length of the numbers list, and the number of ticks in the horizontal
axis:

```
;; X-length
…
(length numbers-list)
```

```

```

```
;; tic-width
…
(* symbol-width X-axis-label-spacing)
```

```

```

```
;; number-of-X-ticks
(if (zerop (% (X-length tic-width)))
    (/ (X-length tic-width))
  (1+ (/ (X-length tic-width))))
```

All this leads us directly to the function for printing the X axis tic line:

```
(defun print-X-axis-tic-line
  (number-of-X-tics X-axis-leading-spaces X-axis-tic-element)
  "Print ticks for X axis."
    (insert X-axis-leading-spaces)
    (insert X-axis-tic-symbol)  ; Under first column.
```

```
    ;; Insert second tic in the right spot.
    (insert (concat
             (make-string
              (-  (* symbol-width X-axis-label-spacing)
                  ;; Insert white space up to second tic symbol.
                  (* 2 (length X-axis-tic-symbol)))
              ? )
             X-axis-tic-symbol))
```

```
    ;; Insert remaining ticks.
    (while (> number-of-X-tics 1)
      (insert X-axis-tic-element)
      (setq number-of-X-tics (1- number-of-X-tics))))
```

The line of numbers is equally straightforward:

First, we create a numbered element with blank spaces before each number:

```
(defun X-axis-element (number)
  "Construct a numbered X axis element."
  (let ((leading-spaces
         (-  (* symbol-width X-axis-label-spacing)
             (length (number-to-string number)))))
    (concat (make-string leading-spaces ? )
            (number-to-string number))))
```

Next, we create the function to print the numbered line, starting with
the number 1 under the first column:

```
(defun print-X-axis-numbered-line
  (number-of-X-tics X-axis-leading-spaces)
  "Print line of X-axis numbers"
  (let ((number X-axis-label-spacing))
    (insert X-axis-leading-spaces)
    (insert "1")
```

```
    (insert (concat
             (make-string
              ;; Insert white space up to next number.
              (-  (* symbol-width X-axis-label-spacing) 2)
              ? )
             (number-to-string number)))
```

```
    ;; Insert remaining numbers.
    (setq number (+ number X-axis-label-spacing))
    (while (> number-of-X-tics 1)
      (insert (X-axis-element number))
      (setq number (+ number X-axis-label-spacing))
      (setq number-of-X-tics (1- number-of-X-tics)))))
```

Finally, we need to write the `print-X-axis` that uses
`print-X-axis-tic-line` and
`print-X-axis-numbered-line`.

The function must determine the local values of the variables used by both
`print-X-axis-tic-line` and `print-X-axis-numbered-line`, and
then it must call them. Also, it must print the carriage return that
separates the two lines.

The function consists of a varlist that specifies five local variables,
and calls to each of the two line printing functions:

```
(defun print-X-axis (numbers-list)
  "Print X axis labels to length of NUMBERS-LIST."
  (let* ((leading-spaces
          (make-string full-Y-label-width ? ))
```

```
       ;; symbol-width is provided by graph-body-print
       (tic-width (* symbol-width X-axis-label-spacing))
       (X-length (length numbers-list))
```

```
       (X-tic
        (concat
         (make-string
```

```
          ;; Make a string of blanks.
          (-  (* symbol-width X-axis-label-spacing)
              (length X-axis-tic-symbol))
          ? )
```

```
         ;; Concatenate blanks with tic symbol.
         X-axis-tic-symbol))
```

```
       (tic-number
        (if (zerop (% X-length tic-width))
            (/ X-length tic-width)
          (1+ (/ X-length tic-width)))))
```

```
    (print-X-axis-tic-line tic-number leading-spaces X-tic)
    (insert "\n")
    (print-X-axis-numbered-line tic-number leading-spaces)))
```

You can test `print-X-axis`:

1. Install `X-axis-tic-symbol`, `X-axis-label-spacing`,
   `print-X-axis-tic-line`, as well as `X-axis-element`,
   `print-X-axis-numbered-line`, and `print-X-axis`.
2. Copy the following expression:

   ```
   (progn
    (let ((full-Y-label-width 5)
          (symbol-width 1))
      (print-X-axis
       '(1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16))))
   ```
3. Switch to the `*scratch*` buffer and place the cursor where you
   want the axis labels to start.
4. Type `M-:` (`eval-expression`).
5. Yank the test expression into the minibuffer
   with `C-y` (`yank)`.
6. Press `RET` to evaluate the expression.

Emacs will print the horizontal axis like this:

  

```
     |   |    |    |    |
     1   5   10   15   20
```

---

Previous: [The `print-X-axis` Function](#print_002dX_002daxis), Up: [A Graph with Labeled Axes](#Full-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### C.4 Printing the Whole Graph

Now we are nearly ready to print the whole graph.

The function to print the graph with the proper labels follows the
outline we created earlier (see [A Graph with Labeled
Axes](#Full-Graph)), but with additions.

Here is the outline:

```
(defun print-graph (numbers-list)
  "documentation…"
  (let ((height  …
        …))
```

```
    (print-Y-axis height … )
    (graph-body-print numbers-list)
    (print-X-axis … )))
```

* [Changes for the Final Version](#The-final-version)
* [Testing `print-graph`](#Test-print_002dgraph)
* [Graphing Numbers of Words and Symbols](#Graphing-words-in-defuns)
* [A `lambda` Expression: Useful Anonymity](#lambda)
* [The `mapcar` Function](#mapcar)
* [Another Bug … Most Insidious](#Another-Bug)
* [The Printed Graph](#Final-printed-graph)

---

Next: [Testing `print-graph`](#Test-print_002dgraph), Up: [Printing the Whole Graph](#Print-Whole-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Changes for the Final Version

The final version is different from what we planned in two ways:
first, it contains additional values calculated once in the varlist;
second, it carries an option to specify the labels’ increment per row.
This latter feature turns out to be essential; otherwise, a graph may
have more rows than fit on a display or on a sheet of paper.

This new feature requires a change to the `Y-axis-column`
function, to add `vertical-step` to it. The function looks like
this:

```
;;; Final version.
(defun Y-axis-column
  (height width-of-label &optional vertical-step)
  "Construct list of labels for Y axis.
HEIGHT is maximum height of graph.
WIDTH-OF-LABEL is maximum width of label.
VERTICAL-STEP, an option, is a positive integer
that specifies how much a Y axis label increments
for each line.  For example, a step of 5 means
that each line is five units of the graph."
```

```
  (let (Y-axis
        (number-per-line (or vertical-step 1)))
    (while (> height 1)
      (if (zerop (% height Y-axis-label-spacing))
```

```
          ;; Insert label.
          (setq Y-axis
                (cons
                 (Y-axis-element
                  (* height number-per-line)
                  width-of-label)
                 Y-axis))
```

```
        ;; Else, insert blanks.
        (setq Y-axis
              (cons
               (make-string width-of-label ? )
               Y-axis)))
      (setq height (1- height)))
```

```
    ;; Insert base line.
    (setq Y-axis (cons (Y-axis-element
                        (or vertical-step 1)
                        width-of-label)
                       Y-axis))
    (nreverse Y-axis)))
```

The values for the maximum height of graph and the width of a symbol
are computed by `print-graph` in its `let` expression; so
`graph-body-print` must be changed to accept them.

```
;;; Final version.
(defun graph-body-print (numbers-list height symbol-width)
  "Print a bar graph of the NUMBERS-LIST.
The numbers-list consists of the Y-axis values.
HEIGHT is maximum height of graph.
SYMBOL-WIDTH is number of each column."
```

```
  (let (from-position)
    (while numbers-list
      (setq from-position (point))
      (insert-rectangle
       (column-of-graph height (car numbers-list)))
      (goto-char from-position)
      (forward-char symbol-width)
```

```
      ;; Draw graph column by column.
      (sit-for 0)
      (setq numbers-list (cdr numbers-list)))
    ;; Place point for X axis labels.
    (forward-line height)
    (insert "\n")))
```

Finally, the code for the `print-graph` function:

```
;;; Final version.
(defun print-graph
  (numbers-list &optional vertical-step)
  "Print labeled bar graph of the NUMBERS-LIST.
The numbers-list consists of the Y-axis values.
```

```

```

```
Optionally, VERTICAL-STEP, a positive integer,
specifies how much a Y axis label increments for
each line.  For example, a step of 5 means that
each row is five units."
```

```
  (let* ((symbol-width (length graph-blank))
         ;; height is both the largest number
         ;; and the number with the most digits.
         (height (apply 'max numbers-list))
```

```
         (height-of-top-line
          (if (zerop (% height Y-axis-label-spacing))
              height
            ;; else
            (* (1+ (/ height Y-axis-label-spacing))
               Y-axis-label-spacing)))
```

```
         (vertical-step (or vertical-step 1))
         (full-Y-label-width
          (length
```

```
           (concat
            (number-to-string
             (* height-of-top-line vertical-step))
            Y-axis-tic))))
```

```

```

```
    (print-Y-axis
     height-of-top-line full-Y-label-width vertical-step)
```

```
    (graph-body-print
     numbers-list height-of-top-line symbol-width)
    (print-X-axis numbers-list)))
```

---

Next: [Graphing Numbers of Words and Symbols](#Graphing-words-in-defuns), Previous: [Changes for the Final Version](#The-final-version), Up: [Printing the Whole Graph](#Print-Whole-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.4.1 Testing `print-graph`

We can test the `print-graph` function with a short list of numbers:

1. Install the final versions of `Y-axis-column`,
   `graph-body-print`, and `print-graph` (in addition to the
   rest of the code.)
2. Copy the following expression:

   ```
   (print-graph '(3 2 5 6 7 5 3 4 6 4 3 2 1))
   ```
3. Switch to the `*scratch*` buffer and place the cursor where you
   want the axis labels to start.
4. Type `M-:` (`eval-expression`).
5. Yank the test expression into the minibuffer
   with `C-y` (`yank)`.
6. Press `RET` to evaluate the expression.

Emacs will print a graph that looks like this:

```
10 -

         *
        **   *
 5 -   ****  *
       **** ***
     * *********
     ************
 1 - *************

     |   |    |    |
     1   5   10   15
```

On the other hand, if you pass `print-graph` a
`vertical-step` value of 2, by evaluating this expression:

```
(print-graph '(3 2 5 6 7 5 3 4 6 4 3 2 1) 2)
```

The graph looks like this:

```
20 -

         *
        **   *
10 -   ****  *
       **** ***
     * *********
     ************
 2 - *************

     |   |    |    |
     1   5   10   15
```

(A question: is the ‘`2`’ on the bottom of the vertical axis a bug or a
feature? If you think it is a bug, and should be a ‘`1`’ instead, (or
even a ‘`0`’), you can modify the sources.)

---

Next: [A `lambda` Expression: Useful Anonymity](#lambda), Previous: [Testing `print-graph`](#Test-print_002dgraph), Up: [Printing the Whole Graph](#Print-Whole-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.4.2 Graphing Numbers of Words and Symbols

Now for the graph for which all this code was written: a graph that
shows how many function definitions contain fewer than 10 words and
symbols, how many contain between 10 and 19 words and symbols, how
many contain between 20 and 29 words and symbols, and so on.

This is a multi-step process. First make sure you have loaded all the
requisite code.

It is a good idea to reset the value of `top-of-ranges` in case
you have set it to some different value. You can evaluate the
following:

```
(setq top-of-ranges
 '(10  20  30  40  50
   60  70  80  90 100
  110 120 130 140 150
  160 170 180 190 200
  210 220 230 240 250
  260 270 280 290 300)
```

Next create a list of the number of words and symbols in each range.

Evaluate the following:

```
(setq list-for-graph
       (defuns-per-range
         (sort
          (recursive-lengths-list-many-files
           (directory-files "/usr/local/emacs/lisp"
                            t ".+el$"))
          '<)
         top-of-ranges))
```

On my old machine, this took about an hour. It looked though 303 Lisp
files in my copy of Emacs version 19.23. After all that computing,
the `list-for-graph` had this value:

```
(537 1027 955 785 594 483 349 292 224 199 166 120 116 99
90 80 67 48 52 45 41 33 28 26 25 20 12 28 11 13 220)
```

This means that my copy of Emacs had 537 function definitions with
fewer than 10 words or symbols in them, 1,027 function definitions
with 10 to 19 words or symbols in them, 955 function definitions with
20 to 29 words or symbols in them, and so on.

Clearly, just by looking at this list we can see that most function
definitions contain ten to thirty words and symbols.

Now for printing. We do *not* want to print a graph that is
1,030 lines high … Instead, we should print a graph that is
fewer than twenty-five lines high. A graph that height can be
displayed on almost any monitor, and easily printed on a sheet of paper.

This means that each value in `list-for-graph` must be reduced to
one-fiftieth its present value.

Here is a short function to do just that, using two functions we have
not yet seen, `mapcar` and `lambda`.

```
(defun one-fiftieth (full-range)
  "Return list, each number one-fiftieth of previous."
 (mapcar (lambda (arg) (/ arg 50)) full-range))
```

---

Next: [The `mapcar` Function](#mapcar), Previous: [Graphing Numbers of Words and Symbols](#Graphing-words-in-defuns), Up: [Printing the Whole Graph](#Print-Whole-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.4.3 A `lambda` Expression: Useful Anonymity

`lambda` is the symbol for an anonymous function, a function
without a name. Every time you use an anonymous function, you need to
include its whole body.

Thus,

```
(lambda (arg) (/ arg 50))
```

is a function that returns the value resulting from
dividing whatever is passed to it as `arg` by 50.

Earlier, for example, we had a function `multiply-by-seven`; it
multiplied its argument by 7. This function is similar, except it
divides its argument by 50; and, it has no name. The anonymous
equivalent of `multiply-by-seven` is:

```
(lambda (number) (* 7 number))
```

(See [The `defun` Macro](#defun).)

If we want to multiply 3 by 7, we can write:

```
(multiply-by-seven 3)
 \_______________/ ^
         |         |
      function  argument
```

  
  

This expression returns 21.

Similarly, we can write:

```
((lambda (number) (* 7 number)) 3)
 \____________________________/ ^
               |                |
      anonymous function     argument
```

  
  

If we want to divide 100 by 50, we can write:

```
((lambda (arg) (/ arg 50)) 100)
 \______________________/  \_/
             |              |
    anonymous function   argument
```

  
  

This expression returns 2. The 100 is passed to the function, which
divides that number by 50.

See [Lambda Expressions](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Lambda-Expressions) in The GNU Emacs
Lisp Reference Manual, for more about `lambda`. Lisp and lambda
expressions derive from the Lambda Calculus.

---

Next: [Another Bug … Most Insidious](#Another-Bug), Previous: [A `lambda` Expression: Useful Anonymity](#lambda), Up: [Printing the Whole Graph](#Print-Whole-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.4.4 The `mapcar` Function

`mapcar` is a function that calls its first argument with each
element of its second argument, in turn. The second argument must be
a sequence.

The ‘`map`’ part of the name comes from the mathematical phrase,
“mapping over a domain”, meaning to apply a function to each of the
elements in a domain. The mathematical phrase is based on the
metaphor of a surveyor walking, one step at a time, over an area he is
mapping. And ‘`car`’, of course, comes from the Lisp notion of the
first of a list.

For example,

```
(mapcar '1+ '(2 4 6))
     ⇒ (3 5 7)
```

The function `1+` which adds one to its argument, is executed on
*each* element of the list, and a new list is returned.

Contrast this with `apply`, which applies its first argument to
all the remaining.
(See [Readying a Graph](#Readying-a-Graph), for an explanation of
`apply`.)

In the definition of `one-fiftieth`, the first argument is the
anonymous function:

```
(lambda (arg) (/ arg 50))
```

and the second argument is `full-range`, which will be bound to
`list-for-graph`.

The whole expression looks like this:

```
(mapcar (lambda (arg) (/ arg 50)) full-range))
```

See [Mapping Functions](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Mapping-Functions) in The GNU Emacs
Lisp Reference Manual, for more about `mapcar`.

Using the `one-fiftieth` function, we can generate a list in
which each element is one-fiftieth the size of the corresponding
element in `list-for-graph`.

```
(setq fiftieth-list-for-graph
      (one-fiftieth list-for-graph))
```

The resulting list looks like this:

```
(10 20 19 15 11 9 6 5 4 3 3 2 2
1 1 1 1 0 1 0 0 0 0 0 0 0 0 0 0 0 4)
```

This, we are almost ready to print! (We also notice the loss of
information: many of the higher ranges are 0, meaning that fewer than
50 defuns had that many words or symbols—but not necessarily meaning
that none had that many words or symbols.)

---

Next: [The Printed Graph](#Final-printed-graph), Previous: [The `mapcar` Function](#mapcar), Up: [Printing the Whole Graph](#Print-Whole-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.4.5 Another Bug … Most Insidious

I said “almost ready to print”! Of course, there is a bug in the
`print-graph` function … It has a `vertical-step`
option, but not a `horizontal-step` option. The
`top-of-range` scale goes from 10 to 300 by tens. But the
`print-graph` function will print only by ones.

This is a classic example of what some consider the most insidious
type of bug, the bug of omission. This is not the kind of bug you can
find by studying the code, for it is not in the code; it is an omitted
feature. Your best actions are to try your program early and often;
and try to arrange, as much as you can, to write code that is easy to
understand and easy to change. Try to be aware, whenever you can,
that whatever you have written, *will* be rewritten, if not soon,
eventually. A hard maxim to follow.

It is the `print-X-axis-numbered-line` function that needs the
work; and then the `print-X-axis` and the `print-graph`
functions need to be adapted. Not much needs to be done; there is one
nicety: the numbers ought to line up under the tic marks. This takes
a little thought.

Here is the corrected `print-X-axis-numbered-line`:

```
(defun print-X-axis-numbered-line
  (number-of-X-tics X-axis-leading-spaces
   &optional horizontal-step)
  "Print line of X-axis numbers"
  (let ((number X-axis-label-spacing)
        (horizontal-step (or horizontal-step 1)))
```

```
    (insert X-axis-leading-spaces)
    ;; Delete extra leading spaces.
    (delete-char
     (- (1-
         (length (number-to-string horizontal-step)))))
    (insert (concat
             (make-string
```

```
              ;; Insert white space.
              (-  (* symbol-width
                     X-axis-label-spacing)
                  (1-
                   (length
                    (number-to-string horizontal-step)))
                  2)
              ? )
             (number-to-string
              (* number horizontal-step))))
```

```
    ;; Insert remaining numbers.
    (setq number (+ number X-axis-label-spacing))
    (while (> number-of-X-tics 1)
      (insert (X-axis-element
               (* number horizontal-step)))
      (setq number (+ number X-axis-label-spacing))
      (setq number-of-X-tics (1- number-of-X-tics)))))
```

If you are reading this in Info, you can see the new versions of
`print-X-axis` `print-graph` and evaluate them. If you are
reading this in a printed book, you can see the changed lines here
(the full text is too much to print).

```
(defun print-X-axis (numbers-list horizontal-step)
  "Print X axis labels to length of NUMBERS-LIST.
Optionally, HORIZONTAL-STEP, a positive integer,
specifies how much an X  axis label increments for
each column."
```

```
;; Value of symbol-width and full-Y-label-width
;; are passed by print-graph.
  (let* ((leading-spaces
          (make-string full-Y-label-width ? ))
       ;; symbol-width is provided by graph-body-print
       (tic-width (* symbol-width X-axis-label-spacing))
       (X-length (length numbers-list))
```

```
       (X-tic
        (concat
         (make-string
          ;; Make a string of blanks.
          (-  (* symbol-width X-axis-label-spacing)
              (length X-axis-tic-symbol))
          ? )
```

```
         ;; Concatenate blanks with tic symbol.
         X-axis-tic-symbol))
       (tic-number
        (if (zerop (% X-length tic-width))
            (/ X-length tic-width)
          (1+ (/ X-length tic-width)))))
```

```

```

```
    (print-X-axis-tic-line
     tic-number leading-spaces X-tic)
    (insert "\n")
    (print-X-axis-numbered-line
     tic-number leading-spaces horizontal-step)))
```

```
(defun print-graph
  (numbers-list &optional vertical-step horizontal-step)
  "Print labeled bar graph of the NUMBERS-LIST.
The numbers-list consists of the Y-axis values.
```

```

```

```
Optionally, VERTICAL-STEP, a positive integer,
specifies how much a Y axis label increments for
each line.  For example, a step of 5 means that
each row is five units.
```

```

```

```
Optionally, HORIZONTAL-STEP, a positive integer,
specifies how much an X  axis label increments for
each column."
  (let* ((symbol-width (length graph-blank))
         ;; height is both the largest number
         ;; and the number with the most digits.
         (height (apply 'max numbers-list))
```

```
         (height-of-top-line
          (if (zerop (% height Y-axis-label-spacing))
              height
            ;; else
            (* (1+ (/ height Y-axis-label-spacing))
               Y-axis-label-spacing)))
```

```
         (vertical-step (or vertical-step 1))
         (full-Y-label-width
          (length
           (concat
            (number-to-string
             (* height-of-top-line vertical-step))
            Y-axis-tic))))
```

```
    (print-Y-axis
     height-of-top-line full-Y-label-width vertical-step)
    (graph-body-print
        numbers-list height-of-top-line symbol-width)
    (print-X-axis numbers-list horizontal-step)))
```

---

Previous: [Another Bug … Most Insidious](#Another-Bug), Up: [Printing the Whole Graph](#Print-Whole-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.4.6 The Printed Graph

When made and installed, you can call the `print-graph` command
like this:

  

```
(print-graph fiftieth-list-for-graph 50 10)
```

  

Here is the graph:

  
  

```
1000 -  *
        **
        **
        **
        **
 750 -  ***
        ***
        ***
        ***
        ****
 500 - *****
       ******
       ******
       ******
       *******
 250 - ********
       *********                     *
       ***********                   *
       *************                 *
  50 - ***************** *           *
       |   |    |    |    |    |    |    |
      10  50  100  150  200  250  300  350
```

  
  

The largest group of functions contain 10–19 words and symbols each.

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [A Graph with Labeled Axes](#Full-Graph), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix D Free Software and Free Manuals

**by Richard M. Stallman**

  

The biggest deficiency in free operating systems is not in the
software—it is the lack of good free manuals that we can include in
these systems. Many of our most important programs do not come with
full manuals. Documentation is an essential part of any software
package; when an important free software package does not come with a
free manual, that is a major gap. We have many such gaps today.

Once upon a time, many years ago, I thought I would learn Perl. I got
a copy of a free manual, but I found it hard to read. When I asked
Perl users about alternatives, they told me that there were better
introductory manuals—but those were not free.

Why was this? The authors of the good manuals had written them for
O’Reilly Associates, which published them with restrictive terms—no
copying, no modification, source files not available—which exclude
them from the free software community.

That wasn’t the first time this sort of thing has happened, and (to
our community’s great loss) it was far from the last. Proprietary
manual publishers have enticed a great many authors to restrict their
manuals since then. Many times I have heard a GNU user eagerly tell me
about a manual that he is writing, with which he expects to help the
GNU project—and then had my hopes dashed, as he proceeded to explain
that he had signed a contract with a publisher that would restrict it
so that we cannot use it.

Given that writing good English is a rare skill among programmers, we
can ill afford to lose manuals this way.

Free documentation, like free software, is a matter of freedom, not
price. The problem with these manuals was not that O’Reilly Associates
charged a price for printed copies—that in itself is fine. The Free
Software Foundation [sells printed copies](https://shop.fsf.org) of
free [GNU manuals](https://www.gnu.org/doc/doc.html), too.
But GNU manuals are available in source code form, while these manuals
are available only on paper. GNU manuals come with permission to copy
and modify; the Perl manuals do not. These restrictions are the
problems.

The criterion for a free manual is pretty much the same as for free
software: it is a matter of giving all users certain
freedoms. Redistribution (including commercial redistribution) must be
permitted, so that the manual can accompany every copy of the program,
on-line or on paper. Permission for modification is crucial too.

As a general rule, I don’t believe that it is essential for people to
have permission to modify all sorts of articles and books. The issues
for writings are not necessarily the same as those for software. For
example, I don’t think you or I are obliged to give permission to
modify articles like this one, which describe our actions and our
views.

But there is a particular reason why the freedom to modify is crucial
for documentation for free software. When people exercise their right
to modify the software, and add or change its features, if they are
conscientious they will change the manual too—so they can provide
accurate and usable documentation with the modified program. A manual
which forbids programmers to be conscientious and finish the job, or
more precisely requires them to write a new manual from scratch if
they change the program, does not fill our community’s needs.

While a blanket prohibition on modification is unacceptable, some
kinds of limits on the method of modification pose no problem. For
example, requirements to preserve the original author’s copyright
notice, the distribution terms, or the list of authors, are ok. It is
also no problem to require modified versions to include notice that
they were modified, even to have entire sections that may not be
deleted or changed, as long as these sections deal with nontechnical
topics. (Some GNU manuals have them.)

These kinds of restrictions are not a problem because, as a practical
matter, they don’t stop the conscientious programmer from adapting the
manual to fit the modified program. In other words, they don’t block
the free software community from making full use of the manual.

However, it must be possible to modify all the technical content of
the manual, and then distribute the result in all the usual media,
through all the usual channels; otherwise, the restrictions do block
the community, the manual is not free, and so we need another manual.

Unfortunately, it is often hard to find someone to write another
manual when a proprietary manual exists. The obstacle is that many
users think that a proprietary manual is good enough—so they don’t
see the need to write a free manual. They do not see that the free
operating system has a gap that needs filling.

Why do users think that proprietary manuals are good enough? Some have
not considered the issue. I hope this article will do something to
change that.

Other users consider proprietary manuals acceptable for the same
reason so many people consider proprietary software acceptable: they
judge in purely practical terms, not using freedom as a
criterion. These people are entitled to their opinions, but since
those opinions spring from values which do not include freedom, they
are no guide for those of us who do value freedom.

Please spread the word about this issue. We continue to lose manuals
to proprietary publishing. If we spread the word that proprietary
manuals are not sufficient, perhaps the next person who wants to help
GNU by writing documentation will realize, before it is too late, that
he must above all make it free.

We can also encourage commercial publishers to sell free, copylefted
manuals instead of proprietary ones. One way you can help this is to
check the distribution terms of a manual before you buy it, and prefer
copylefted manuals to non-copylefted ones.

  
  

Note: The Free Software Foundation maintains a page on its Web site
that lists free books available from other publishers:  
<https://www.gnu.org/doc/other-free-books.html>

---

Next: [Index](#Index), Previous: [Free Software and Free Manuals](#Free-Software-and-Free-Manuals), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix E GNU Free Documentation License

Version 1.3, 3 November 2008

```
Copyright © 2000, 2001, 2002, 2007, 2008 Free Software Foundation, Inc.
https://fsf.org/

Everyone is permitted to copy and distribute verbatim copies
of this license document, but changing it is not allowed.
```

0. PREAMBLE

   The purpose of this License is to make a manual, textbook, or other
   functional and useful document *free* in the sense of freedom: to
   assure everyone the effective freedom to copy and redistribute it,
   with or without modifying it, either commercially or noncommercially.
   Secondarily, this License preserves for the author and publisher a way
   to get credit for their work, while not being considered responsible
   for modifications made by others.

   This License is a kind of “copyleft”, which means that derivative
   works of the document must themselves be free in the same sense. It
   complements the GNU General Public License, which is a copyleft
   license designed for free software.

   We have designed this License in order to use it for manuals for free
   software, because free software needs free documentation: a free
   program should come with manuals providing the same freedoms that the
   software does. But this License is not limited to software manuals;
   it can be used for any textual work, regardless of subject matter or
   whether it is published as a printed book. We recommend this License
   principally for works whose purpose is instruction or reference.
1. APPLICABILITY AND DEFINITIONS

   This License applies to any manual or other work, in any medium, that
   contains a notice placed by the copyright holder saying it can be
   distributed under the terms of this License. Such a notice grants a
   world-wide, royalty-free license, unlimited in duration, to use that
   work under the conditions stated herein. The “Document”, below,
   refers to any such manual or work. Any member of the public is a
   licensee, and is addressed as “you”. You accept the license if you
   copy, modify or distribute the work in a way requiring permission
   under copyright law.

   A “Modified Version” of the Document means any work containing the
   Document or a portion of it, either copied verbatim, or with
   modifications and/or translated into another language.

   A “Secondary Section” is a named appendix or a front-matter section
   of the Document that deals exclusively with the relationship of the
   publishers or authors of the Document to the Document’s overall
   subject (or to related matters) and contains nothing that could fall
   directly within that overall subject. (Thus, if the Document is in
   part a textbook of mathematics, a Secondary Section may not explain
   any mathematics.) The relationship could be a matter of historical
   connection with the subject or with related matters, or of legal,
   commercial, philosophical, ethical or political position regarding
   them.

   The “Invariant Sections” are certain Secondary Sections whose titles
   are designated, as being those of Invariant Sections, in the notice
   that says that the Document is released under this License. If a
   section does not fit the above definition of Secondary then it is not
   allowed to be designated as Invariant. The Document may contain zero
   Invariant Sections. If the Document does not identify any Invariant
   Sections then there are none.

   The “Cover Texts” are certain short passages of text that are listed,
   as Front-Cover Texts or Back-Cover Texts, in the notice that says that
   the Document is released under this License. A Front-Cover Text may
   be at most 5 words, and a Back-Cover Text may be at most 25 words.

   A “Transparent” copy of the Document means a machine-readable copy,
   represented in a format whose specification is available to the
   general public, that is suitable for revising the document
   straightforwardly with generic text editors or (for images composed of
   pixels) generic paint programs or (for drawings) some widely available
   drawing editor, and that is suitable for input to text formatters or
   for automatic translation to a variety of formats suitable for input
   to text formatters. A copy made in an otherwise Transparent file
   format whose markup, or absence of markup, has been arranged to thwart
   or discourage subsequent modification by readers is not Transparent.
   An image format is not Transparent if used for any substantial amount
   of text. A copy that is not “Transparent” is called “Opaque”.

   Examples of suitable formats for Transparent copies include plain
   ASCII without markup, Texinfo input format, LaTeX input
   format, SGML or XML using a publicly available
   DTD, and standard-conforming simple HTML,
   PostScript or PDF designed for human modification. Examples
   of transparent image formats include PNG, XCF and
   JPG. Opaque formats include proprietary formats that can be
   read and edited only by proprietary word processors, SGML or
   XML for which the DTD and/or processing tools are
   not generally available, and the machine-generated HTML,
   PostScript or PDF produced by some word processors for
   output purposes only.

   The “Title Page” means, for a printed book, the title page itself,
   plus such following pages as are needed to hold, legibly, the material
   this License requires to appear in the title page. For works in
   formats which do not have any title page as such, “Title Page” means
   the text near the most prominent appearance of the work’s title,
   preceding the beginning of the body of the text.

   The “publisher” means any person or entity that distributes copies
   of the Document to the public.

   A section “Entitled XYZ” means a named subunit of the Document whose
   title either is precisely XYZ or contains XYZ in parentheses following
   text that translates XYZ in another language. (Here XYZ stands for a
   specific section name mentioned below, such as “Acknowledgements”,
   “Dedications”, “Endorsements”, or “History”.) To “Preserve the Title”
   of such a section when you modify the Document means that it remains a
   section “Entitled XYZ” according to this definition.

   The Document may include Warranty Disclaimers next to the notice which
   states that this License applies to the Document. These Warranty
   Disclaimers are considered to be included by reference in this
   License, but only as regards disclaiming warranties: any other
   implication that these Warranty Disclaimers may have is void and has
   no effect on the meaning of this License.
2. VERBATIM COPYING

   You may copy and distribute the Document in any medium, either
   commercially or noncommercially, provided that this License, the
   copyright notices, and the license notice saying this License applies
   to the Document are reproduced in all copies, and that you add no other
   conditions whatsoever to those of this License. You may not use
   technical measures to obstruct or control the reading or further
   copying of the copies you make or distribute. However, you may accept
   compensation in exchange for copies. If you distribute a large enough
   number of copies you must also follow the conditions in section 3.

   You may also lend copies, under the same conditions stated above, and
   you may publicly display copies.
3. COPYING IN QUANTITY

   If you publish printed copies (or copies in media that commonly have
   printed covers) of the Document, numbering more than 100, and the
   Document’s license notice requires Cover Texts, you must enclose the
   copies in covers that carry, clearly and legibly, all these Cover
   Texts: Front-Cover Texts on the front cover, and Back-Cover Texts on
   the back cover. Both covers must also clearly and legibly identify
   you as the publisher of these copies. The front cover must present
   the full title with all words of the title equally prominent and
   visible. You may add other material on the covers in addition.
   Copying with changes limited to the covers, as long as they preserve
   the title of the Document and satisfy these conditions, can be treated
   as verbatim copying in other respects.

   If the required texts for either cover are too voluminous to fit
   legibly, you should put the first ones listed (as many as fit
   reasonably) on the actual cover, and continue the rest onto adjacent
   pages.

   If you publish or distribute Opaque copies of the Document numbering
   more than 100, you must either include a machine-readable Transparent
   copy along with each Opaque copy, or state in or with each Opaque copy
   a computer-network location from which the general network-using
   public has access to download using public-standard network protocols
   a complete Transparent copy of the Document, free of added material.
   If you use the latter option, you must take reasonably prudent steps,
   when you begin distribution of Opaque copies in quantity, to ensure
   that this Transparent copy will remain thus accessible at the stated
   location until at least one year after the last time you distribute an
   Opaque copy (directly or through your agents or retailers) of that
   edition to the public.

   It is requested, but not required, that you contact the authors of the
   Document well before redistributing any large number of copies, to give
   them a chance to provide you with an updated version of the Document.
4. MODIFICATIONS

   You may copy and distribute a Modified Version of the Document under
   the conditions of sections 2 and 3 above, provided that you release
   the Modified Version under precisely this License, with the Modified
   Version filling the role of the Document, thus licensing distribution
   and modification of the Modified Version to whoever possesses a copy
   of it. In addition, you must do these things in the Modified Version:

   1. Use in the Title Page (and on the covers, if any) a title distinct
      from that of the Document, and from those of previous versions
      (which should, if there were any, be listed in the History section
      of the Document). You may use the same title as a previous version
      if the original publisher of that version gives permission.
   2. List on the Title Page, as authors, one or more persons or entities
      responsible for authorship of the modifications in the Modified
      Version, together with at least five of the principal authors of the
      Document (all of its principal authors, if it has fewer than five),
      unless they release you from this requirement.
   3. State on the Title page the name of the publisher of the
      Modified Version, as the publisher.
   4. Preserve all the copyright notices of the Document.
   5. Add an appropriate copyright notice for your modifications
      adjacent to the other copyright notices.
   6. Include, immediately after the copyright notices, a license notice
      giving the public permission to use the Modified Version under the
      terms of this License, in the form shown in the Addendum below.
   7. Preserve in that license notice the full lists of Invariant Sections
      and required Cover Texts given in the Document’s license notice.
   8. Include an unaltered copy of this License.
   9. Preserve the section Entitled “History”, Preserve its Title, and add
      to it an item stating at least the title, year, new authors, and
      publisher of the Modified Version as given on the Title Page. If
      there is no section Entitled “History” in the Document, create one
      stating the title, year, authors, and publisher of the Document as
      given on its Title Page, then add an item describing the Modified
      Version as stated in the previous sentence.
   10. Preserve the network location, if any, given in the Document for
       public access to a Transparent copy of the Document, and likewise
       the network locations given in the Document for previous versions
       it was based on. These may be placed in the “History” section.
       You may omit a network location for a work that was published at
       least four years before the Document itself, or if the original
       publisher of the version it refers to gives permission.
   11. For any section Entitled “Acknowledgements” or “Dedications”, Preserve
       the Title of the section, and preserve in the section all the
       substance and tone of each of the contributor acknowledgements and/or
       dedications given therein.
   12. Preserve all the Invariant Sections of the Document,
       unaltered in their text and in their titles. Section numbers
       or the equivalent are not considered part of the section titles.
   13. Delete any section Entitled “Endorsements”. Such a section
       may not be included in the Modified Version.
   14. Do not retitle any existing section to be Entitled “Endorsements” or
       to conflict in title with any Invariant Section.
   15. Preserve any Warranty Disclaimers.

   If the Modified Version includes new front-matter sections or
   appendices that qualify as Secondary Sections and contain no material
   copied from the Document, you may at your option designate some or all
   of these sections as invariant. To do this, add their titles to the
   list of Invariant Sections in the Modified Version’s license notice.
   These titles must be distinct from any other section titles.

   You may add a section Entitled “Endorsements”, provided it contains
   nothing but endorsements of your Modified Version by various
   parties—for example, statements of peer review or that the text has
   been approved by an organization as the authoritative definition of a
   standard.

   You may add a passage of up to five words as a Front-Cover Text, and a
   passage of up to 25 words as a Back-Cover Text, to the end of the list
   of Cover Texts in the Modified Version. Only one passage of
   Front-Cover Text and one of Back-Cover Text may be added by (or
   through arrangements made by) any one entity. If the Document already
   includes a cover text for the same cover, previously added by you or
   by arrangement made by the same entity you are acting on behalf of,
   you may not add another; but you may replace the old one, on explicit
   permission from the previous publisher that added the old one.

   The author(s) and publisher(s) of the Document do not by this License
   give permission to use their names for publicity for or to assert or
   imply endorsement of any Modified Version.
5. COMBINING DOCUMENTS

   You may combine the Document with other documents released under this
   License, under the terms defined in section 4 above for modified
   versions, provided that you include in the combination all of the
   Invariant Sections of all of the original documents, unmodified, and
   list them all as Invariant Sections of your combined work in its
   license notice, and that you preserve all their Warranty Disclaimers.

   The combined work need only contain one copy of this License, and
   multiple identical Invariant Sections may be replaced with a single
   copy. If there are multiple Invariant Sections with the same name but
   different contents, make the title of each such section unique by
   adding at the end of it, in parentheses, the name of the original
   author or publisher of that section if known, or else a unique number.
   Make the same adjustment to the section titles in the list of
   Invariant Sections in the license notice of the combined work.

   In the combination, you must combine any sections Entitled “History”
   in the various original documents, forming one section Entitled
   “History”; likewise combine any sections Entitled “Acknowledgements”,
   and any sections Entitled “Dedications”. You must delete all
   sections Entitled “Endorsements.”
6. COLLECTIONS OF DOCUMENTS

   You may make a collection consisting of the Document and other documents
   released under this License, and replace the individual copies of this
   License in the various documents with a single copy that is included in
   the collection, provided that you follow the rules of this License for
   verbatim copying of each of the documents in all other respects.

   You may extract a single document from such a collection, and distribute
   it individually under this License, provided you insert a copy of this
   License into the extracted document, and follow this License in all
   other respects regarding verbatim copying of that document.
7. AGGREGATION WITH INDEPENDENT WORKS

   A compilation of the Document or its derivatives with other separate
   and independent documents or works, in or on a volume of a storage or
   distribution medium, is called an “aggregate” if the copyright
   resulting from the compilation is not used to limit the legal rights
   of the compilation’s users beyond what the individual works permit.
   When the Document is included in an aggregate, this License does not
   apply to the other works in the aggregate which are not themselves
   derivative works of the Document.

   If the Cover Text requirement of section 3 is applicable to these
   copies of the Document, then if the Document is less than one half of
   the entire aggregate, the Document’s Cover Texts may be placed on
   covers that bracket the Document within the aggregate, or the
   electronic equivalent of covers if the Document is in electronic form.
   Otherwise they must appear on printed covers that bracket the whole
   aggregate.
8. TRANSLATION

   Translation is considered a kind of modification, so you may
   distribute translations of the Document under the terms of section 4.
   Replacing Invariant Sections with translations requires special
   permission from their copyright holders, but you may include
   translations of some or all Invariant Sections in addition to the
   original versions of these Invariant Sections. You may include a
   translation of this License, and all the license notices in the
   Document, and any Warranty Disclaimers, provided that you also include
   the original English version of this License and the original versions
   of those notices and disclaimers. In case of a disagreement between
   the translation and the original version of this License or a notice
   or disclaimer, the original version will prevail.

   If a section in the Document is Entitled “Acknowledgements”,
   “Dedications”, or “History”, the requirement (section 4) to Preserve
   its Title (section 1) will typically require changing the actual
   title.
9. TERMINATION

   You may not copy, modify, sublicense, or distribute the Document
   except as expressly provided under this License. Any attempt
   otherwise to copy, modify, sublicense, or distribute it is void, and
   will automatically terminate your rights under this License.

   However, if you cease all violation of this License, then your license
   from a particular copyright holder is reinstated (a) provisionally,
   unless and until the copyright holder explicitly and finally
   terminates your license, and (b) permanently, if the copyright holder
   fails to notify you of the violation by some reasonable means prior to
   60 days after the cessation.

   Moreover, your license from a particular copyright holder is
   reinstated permanently if the copyright holder notifies you of the
   violation by some reasonable means, this is the first time you have
   received notice of violation of this License (for any work) from that
   copyright holder, and you cure the violation prior to 30 days after
   your receipt of the notice.

   Termination of your rights under this section does not terminate the
   licenses of parties who have received copies or rights from you under
   this License. If your rights have been terminated and not permanently
   reinstated, receipt of a copy of some or all of the same material does
   not give you any rights to use it.
10. FUTURE REVISIONS OF THIS LICENSE

    The Free Software Foundation may publish new, revised versions
    of the GNU Free Documentation License from time to time. Such new
    versions will be similar in spirit to the present version, but may
    differ in detail to address new problems or concerns. See
    <https://www.gnu.org/licenses/>.

    Each version of the License is given a distinguishing version number.
    If the Document specifies that a particular numbered version of this
    License “or any later version” applies to it, you have the option of
    following the terms and conditions either of that specified version or
    of any later version that has been published (not as a draft) by the
    Free Software Foundation. If the Document does not specify a version
    number of this License, you may choose any version ever published (not
    as a draft) by the Free Software Foundation. If the Document
    specifies that a proxy can decide which future versions of this
    License can be used, that proxy’s public statement of acceptance of a
    version permanently authorizes you to choose that version for the
    Document.
11. RELICENSING

    “Massive Multiauthor Collaboration Site” (or “MMC Site”) means any
    World Wide Web server that publishes copyrightable works and also
    provides prominent facilities for anybody to edit those works. A
    public wiki that anybody can edit is an example of such a server. A
    “Massive Multiauthor Collaboration” (or “MMC”) contained in the
    site means any set of copyrightable works thus published on the MMC
    site.

    “CC-BY-SA” means the Creative Commons Attribution-Share Alike 3.0
    license published by Creative Commons Corporation, a not-for-profit
    corporation with a principal place of business in San Francisco,
    California, as well as future copyleft versions of that license
    published by that same organization.

    “Incorporate” means to publish or republish a Document, in whole or
    in part, as part of another Document.

    An MMC is “eligible for relicensing” if it is licensed under this
    License, and if all works that were first published under this License
    somewhere other than this MMC, and subsequently incorporated in whole
    or in part into the MMC, (1) had no cover texts or invariant sections,
    and (2) were thus incorporated prior to November 1, 2008.

    The operator of an MMC Site may republish an MMC contained in the site
    under CC-BY-SA on the same site at any time before August 1, 2009,
    provided the MMC is eligible for relicensing.
